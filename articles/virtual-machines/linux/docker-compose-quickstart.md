---
title: "Verwenden von Docker Compose für einen virtuellen Linux-Computer in Azure | Microsoft Docs"
description: "Verwenden von Docker und Compose für virtuelle Linux-Computer mit der Azure-Befehlszeilenschnittstelle"
services: virtual-machines-linux
documentationcenter: 
author: iainfoulds
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 02ab8cf9-318d-4a28-9d0c-4a31dccc2a84
ms.service: virtual-machines-linux
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 05/11/2017
ms.author: iainfou
ms.translationtype: Human Translation
ms.sourcegitcommit: fc4172b27b93a49c613eb915252895e845b96892
ms.openlocfilehash: ad9759f20135a87356819d5b819eab357b688cdc
ms.contentlocale: de-de
ms.lasthandoff: 05/12/2017


---
# <a name="get-started-with-docker-and-compose-to-define-and-run-a-multi-container-application-in-azure"></a>Erste Schritte mit Docker und Compose zum Definieren und Ausführen einer Anwendung mit mehreren Containern in Azure
Bei [Compose](http://github.com/docker/compose) verwenden Sie eine einfache Textdatei zum Definieren einer Anwendung, die aus mehreren Docker-Containern besteht. Sie starten Ihre Anwendung dann mit einem einzelnen Befehl, mit dem alle Schritte zur Bereitstellung Ihrer definierten Umgebung ausgeführt werden. In diesem Artikel wird beispielsweise veranschaulicht, wie Sie schnell einen WordPress-Blog mit einer MariaDB SQL-Back-End-Datenbank auf einem virtuellen Ubuntu-Computer einrichten. Sie können aber auch Compose verwenden, um komplexere Anwendungen einzurichten.


## <a name="set-up-a-linux-vm-as-a-docker-host"></a>Einrichten eines virtuellen Linux-Computers als Docker-Host
Im Azure Marketplace stehen Ihnen verschieden Azure-Verfahren und Images oder Resource Manager-Vorlagen zur Verfügung, um einen virtuellen Linux-Computer zu erstellen und als Docker-Host einzurichten. So finden Sie etwa unter [Verwenden der Docker-VM-Erweiterung zum Bereitstellen Ihrer Umgebung](dockerextension.md) ein schnelles Verfahren zum Erstellen eines virtuellen Ubuntu-Computers mit der Azure Docker-VM-Erweiterung unter Verwendung einer [Schnellstartvorlage](https://github.com/Azure/azure-quickstart-templates/tree/master/docker-simple-on-ubuntu). 

Bei Verwendung der Docker-VM-Erweiterung wird Ihr virtueller Computer automatisch als Docker-Host eingerichtet, und Compose ist bereits installiert.


### <a name="create-docker-host-with-azure-cli-20"></a>Erstellen eines Docker-Hosts mit Azure CLI 2.0
Installieren Sie die neueste Version von [Azure CLI 2.0](/cli/azure/install-az-cli2), und melden Sie sich mit [az login](/cli/azure/#login) bei einem Azure-Konto an.

Erstellen Sie zuerst mit [az group create](/cli/azure/group#create) eine Ressourcengruppe für Ihre Docker-Umgebung. Im folgenden Beispiel wird am Standort *westus* eine Ressourcengruppe mit dem Namen *myResourceGroup* erstellt:

```azurecli
az group create --name myResourceGroup --location westus
```

Stellen Sie als Nächstes mit [az group deployment create](/cli/azure/group/deployment#create) einen virtuellen Computer bereit, der die Azure Docker-VM-Erweiterung aus [dieser Azure Resource Manager-Vorlage auf GitHub](https://github.com/Azure/azure-quickstart-templates/tree/master/docker-simple-on-ubuntu) enthält. Geben Sie Ihre eigenen Werte für *newStorageAccountName*, *adminUsername*, *adminPassword* und *dnsNameForPublicIP* an:

```azurecli
az group deployment create --resource-group myResourceGroup \
  --parameters '{"newStorageAccountName": {"value": "mystorageaccount"},
    "adminUsername": {"value": "azureuser"},
    "adminPassword": {"value": "P@ssw0rd!"},
    "dnsNameForPublicIP": {"value": "mypublicdns"}}' \
  --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/docker-simple-on-ubuntu/azuredeploy.json
```

Es dauert einige Minuten, bis die Bereitstellung abgeschlossen ist. Wenn die Bereitstellung abgeschlossen ist, [fahren Sie mit dem nächsten Schritt fort](#verify-that-compose-is-installed), um eine SSH-Verbindung mit Ihrem virtuellen Computer herzustellen. 

Optional können Sie dem vorherigen Befehl das `--no-wait`-Flag hinzufügen, um stattdessen die Steuerung an die Eingabeaufforderung zurückzugeben und die Bereitstellung im Hintergrund auszuführen. Auf diese Weise können Sie die Befehlszeilenschnittstelle für andere Aufgaben nutzen, während die Bereitstellung ausgeführt wird. Sie können dann mit dem Befehl [az vm show](/cli/azure/vm#show) Details zum Docker-Hoststatus anzeigen. Im folgenden Beispiel wird der Status der VM mit dem Namen *myDockerVM* (Standardname aus der Vorlage, nicht ändern) in der Ressourcengruppe mit dem Namen *myResourceGroup* überprüft:

```azurecli
az vm show \
    --resource-group myResourceGroup \
    --name myDockerVM \
    --query [provisioningState] \
    --output tsv
```

Wenn dieser Befehl *Succeeded* (Erfolgreich) zurückgibt, ist die Bereitstellung abgeschlossen, und Sie können im nächsten Schritt eine SSH-Verbindung mit dem virtuellen Computer herstellen.


## <a name="verify-that-compose-is-installed"></a>Vergewissern, dass Compose installiert ist
Stellen Sie nach Abschluss der Bereitstellung eine SSH-Verbindung mit dem neuen Docker-Host her. Verwenden Sie dazu den DNS-Namen, den Sie während der Bereitstellung angegeben haben. Sie können `az vm show -g myResourceGroup -n myDockerVM -d --query [fqdns] -o tsv` verwenden, um die Details Ihrer VM anzuzeigen, z.B. den DNS-Namen.

```bash
ssh azureuser@mypublicdns.westus.cloudapp.azure.com
```

Führen Sie den folgenden Befehl aus, um sicherzustellen, dass Compose auf dem virtuellen Computer installiert ist:

```bash
docker-compose --version
```

Die Ausgabe lautet etwa wie folgt: *docker-compose 1.6.2, build 4d72027*.

> [!TIP]
> Wenn Sie den Docker-Host auf eine andere Art erstellt haben und Compose selbst installieren müssen, finden Sie in der [Compose-Dokumentation](https://github.com/docker/compose/blob/882dc673ce84b0b29cd59b6815cb93f74a6c4134/docs/install.md)weitere Informationen.


## <a name="create-a-docker-composeyml-configuration-file"></a>Erstellen der Konfigurationsdatei „docker-compose.yml“
Als Nächstes erstellen Sie die Datei `docker-compose.yml` (eine reine Textkonfigurationsdatei), um die Docker-Container zu definieren, die auf dem virtuellen Computer ausgeführt werden sollen. Die Datei enthält Angaben zum Image, das für jeden Container ausgeführt werden soll (es kann auch ein Build einer Docker-Datei sein), erforderliche Umgebungsvariablen und Abhängigkeiten, Ports und Links zwischen Containern. Details zur Syntax der YML-Datei finden Sie in der [Compose-Dateireferenz](http://docs.docker.com/compose/yml/).

Erstellen Sie die Datei *docker-compose.yml* wie folgt:

```bash
touch docker-compose.yml
```

Verwenden Sie Ihren bevorzugten Text-Editor, um der Datei einige Daten hinzuzufügen. Im folgenden Beispiel wird der Editor *vi* verwendet:

```bash
vi docker-compose.yml
```

Fügen Sie das folgende Beispiel in die Textdatei ein. Diese Konfiguration nutzt Images aus der [Docker-Hub-Registrierung](https://registry.hub.docker.com/_/wordpress/) , um WordPress (Open-Source-Blogging- und Content Management-System) und die verknüpfte MariaDB SQL-Back-End-Datenbank zu installieren. Geben Sie Ihr eigenes *MYSQL_ROOT_PASSWORD* wie folgt an:

```sh
wordpress:
  image: wordpress
  links:
    - db:mysql
  ports:
    - 80:80

db:
  image: mariadb
  environment:
    MYSQL_ROOT_PASSWORD: <your password>
```

## <a name="start-the-containers-with-compose"></a>Starten des Containers mit Compose
Führen Sie in demselben Verzeichnis, in dem sich die Datei *docker-compose.yml* befindet, den folgenden Befehl aus (je nach Umgebung kann es erforderlich sein, `docker-compose` mit `sudo` auszuführen):

```bash
docker-compose up -d
```

Mit diesem Befehl werden die in *docker-compose.yml* angegebenen Docker-Container gestartet. Es dauert ein bis zwei Minuten, bis dieser Schritt abgeschlossen ist. Ihnen wird daraufhin eine Ausgabe angezeigt, die in etwa wie im folgenden Beispiel aussieht:

```bash
Creating wordpress_db_1...
Creating wordpress_wordpress_1...
...
```

> [!NOTE]
> Verwenden Sie zu Beginn die Option **-d** , damit die Container kontinuierlich im Hintergrund ausgeführt werden.


Um zu prüfen, ob die Container ausgeführt werden, geben Sie `docker-compose ps`ein. Die Ausgabe sollte folgendermaßen aussehen:

```bash
        Name                       Command               State         Ports
-----------------------------------------------------------------------------------
azureuser_db_1          docker-entrypoint.sh mysqld      Up      3306/tcp
azureuser_wordpress_1   docker-entrypoint.sh apach ...   Up      0.0.0.0:80->80/tcp
```

Sie können nun auf dem virtuellen Computer über Port 80 eine direkte Verbindung mit WordPress herstellen. Öffnen Sie einen Webbrowser, und geben Sie den DNS-Namen Ihrer VM ein (z.B. `http://mypublicdns.westus.cloudapp.azure.com`). Daraufhin sollte der Startbildschirm von WordPress angezeigt werden, auf dem Sie die Installation abschließen und erste Schritte mit der Anwendung ausführen können.

![WordPress-Startbildschirm][wordpress_start]

## <a name="next-steps"></a>Nächste Schritte
* Weitere Optionen zum Konfigurieren von Docker und Compose auf dem virtuellen Docker-Computer finden Sie im [Leitfaden für die Azure-Docker-VM-Erweiterung](https://github.com/Azure/azure-docker-extension/blob/master/README.md) . Eine Möglichkeit wäre beispielsweise, die (in JSON konvertierte) Compose-YML-Datei direkt in die Konfiguration der Docker-VM-Erweiterung zu integrieren.
* Weitere Beispiele zum Erstellen und Bereitstellen von Apps mit mehreren Containern finden Sie in der [Composer-Befehlszeilenreferenz](http://docs.docker.com/compose/reference/) und im [Benutzerhandbuch](http://docs.docker.com/compose/).
* Verwenden Sie eine von Ihnen oder der [Community](https://azure.microsoft.com/documentation/templates/)erstellte Vorlage des Azure-Ressourcen-Managers, um eine Azure-VM mit Docker und eine mit Compose eingerichtete Anwendung bereitzustellen. Die Vorlage [WordPress-Blog mit Docker bereitstellen](https://github.com/Azure/azure-quickstart-templates/tree/master/docker-wordpress-mysql) verwendet Docker und Compose, um WordPress schnell mit einem MySQL-Back-End auf einem virtuellen Ubuntu-Computer bereitzustellen.
* Sie können Docker Compose auch in ein Docker Swarm-Cluster integrieren. Entsprechende Szenarien finden Sie unter [Using Compose with Swarm](https://docs.docker.com/compose/swarm/) (Verwenden von Compose mit Swarm).

<!--Image references-->

[wordpress_start]: media/docker-compose-quickstart/WordPress.png

