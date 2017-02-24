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
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 02/13/2017
ms.author: iainfou
translationtype: Human Translation
ms.sourcegitcommit: 9fc3f1fbe9ab03257d613e31f5890a63d1aeba1f
ms.openlocfilehash: 70796d5dc7c1a47d65d51d4873705606ef32c869


---
# <a name="get-started-with-docker-and-compose-to-define-and-run-a-multi-container-application-on-an-azure-virtual-machine"></a>Erste Schritte mit Docker und Compose zum Definieren und Ausführen einer Anwendung mit mehreren Containern auf einem virtuellen Azure-Computer
Bei [Compose](http://github.com/docker/compose) verwenden Sie eine einfache Textdatei zum Definieren einer Anwendung, die aus mehreren Docker-Containern besteht. Sie starten Ihre Anwendung dann mit einem einzelnen Befehl, mit dem alle Schritte zur Bereitstellung Ihrer definierten Umgebung ausgeführt werden. In diesem Artikel wird beispielsweise veranschaulicht, wie Sie schnell einen WordPress-Blog mit einer MariaDB SQL-Back-End-Datenbank auf einem virtuellen Ubuntu-Computer einrichten. Sie können aber auch Compose verwenden, um komplexere Anwendungen einzurichten.

## <a name="step-1-set-up-a-linux-vm-as-a-docker-host"></a>Schritt 1: Einrichten eines virtuellen Linux-Computers als Docker-Host
Im Azure Marketplace stehen Ihnen verschieden Azure-Verfahren und Images oder Resource Manager-Vorlagen zur Verfügung, um einen virtuellen Linux-Computer zu erstellen und als Docker-Host einzurichten. So finden Sie etwa unter [Verwenden der Docker-VM-Erweiterung zum Bereitstellen Ihrer Umgebung](virtual-machines-linux-dockerextension.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) ein schnelles Verfahren zum Erstellen eines virtuellen Ubuntu-Computers mit der Azure Docker-VM-Erweiterung unter Verwendung einer [Schnellstartvorlage](https://github.com/Azure/azure-quickstart-templates/tree/master/docker-simple-on-ubuntu). 

Bei Verwendung der Docker-VM-Erweiterung wird Ihr virtueller Computer automatisch als Docker-Host eingerichtet, und Compose ist bereits installiert. Sie können eine der folgenden Versionen der Befehlszeilenschnittstelle verwenden, um einen virtuellen Computer zu erstellen und die Docker-VM-Erweiterung zu nutzen:

- [Azure CLI 1.0:](#azure-cli-10) Unsere CLI für das klassische Bereitstellungsmodell und das Resource Manager-Bereitstellungsmodell
- [Azure CLI 2.0 (Vorschau):](#azure-cli-20-preview) Unsere CLI der nächsten Generation für das Resource Manager-Bereitstellungsmodell


### <a name="azure-cli-10"></a>Azure-Befehlszeilenschnittstelle 1.0
Installieren Sie die neueste [Azure-Befehlszeilenschnittstelle 1.0](../xplat-cli-install.md), und melden Sie sich bei einem Azure-Konto an. Stellen Sie sicher, dass der Resource Manager-Modus (`azure config mode arm`) aktiviert ist, bevor Sie den virtuellen Computer erstellen.

Das folgende Beispiel erstellt eine Ressourcengruppe mit dem Namen `myResourceGroup` im Standort `West US` und stellt einen virtuellen Computer mit der Azure Docker-VM-Erweiterung bereit. Zum Bereitstellen der Umgebung wird eine [Azure Resource Manager-Vorlage von GitHub](https://github.com/Azure/azure-quickstart-templates/tree/master/docker-simple-on-ubuntu) verwendet:

```azurecli
azure group create --name myResourceGroup --location "West US" \
  --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/docker-simple-on-ubuntu/azuredeploy.json
```

In der Azure-Befehlszeilenschnittstelle gelangen Sie nach wenigen Sekunden zurück zur Eingabeaufforderung, der Docker-Host wird jedoch weiterhin erstellt und konfiguriert. Es dauert einige Minuten, bis die Bereitstellung abgeschlossen ist. Sie können Details zum Docker-Hoststatus mit dem Befehl `azure vm show` anzeigen. Im folgenden Beispiel wird der Status der VM mit dem Namen `myDockerVM` (Standardname aus der Vorlage, nicht ändern) in der Ressourcengruppe mit dem Namen `myResourceGroup` überprüft. Geben Sie den Namen der Ressourcengruppe ein, die Sie im vorherigen Schritt erstellt haben:

```azurecli
azure vm show --resource-group myResourceGroup --name myDockerVM
```

### <a name="azure-cli-20-preview"></a>Azure CLI 2.0 (Vorschau)
Installieren Sie die neueste [Azure-Befehlszeilenschnittstelle 2.0 (Vorschau)](/cli/azure/install-az-cli2), und melden Sie sich mit [az login](/cli/azure/#login) bei einem Azure-Konto an.

Erstellen Sie zuerst mit [az group create](/cli/azure/group#create) eine Ressourcengruppe für Ihre Docker-Umgebung. Das folgende Beispiel erstellt eine Ressourcengruppe mit dem Namen `myResourceGroup` am Standort `West US`:

```azurecli
az group create --name myResourceGroup --location westus
```

Stellen Sie als Nächstes mit [az group deployment create](/cli/azure/group/deployment#create) einen virtuellen Computer bereit, der die Azure Docker-VM-Erweiterung aus [dieser Azure Resource Manager-Vorlage auf GitHub](https://github.com/Azure/azure-quickstart-templates/tree/master/docker-simple-on-ubuntu) enthält. Geben Sie eigene Werte für `newStorageAccountName`, `adminUsername`, `adminPassword` und `dnsNameForPublicIP` an:

```azurecli
az group deployment create --resource-group myResourceGroup \
  --parameters '{"newStorageAccountName": {"value": "mystorageaccount"},
    "adminUsername": {"value": "azureuser"},
    "adminPassword": {"value": "P@ssw0rd!"},
    "dnsNameForPublicIP": {"value": "mypublicdns"}}' \
  --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/docker-simple-on-ubuntu/azuredeploy.json
```

Es dauert einige Minuten, bis die Bereitstellung abgeschlossen ist. Wenn die Bereitstellung abgeschlossen ist, [fahren Sie mit dem nächsten Schritt fort](#step-2-verify-that-compose-is-installed), um eine SSH-Verbindung mit Ihrem virtuellen Computer herzustellen. 

Optional können Sie dem vorherigen Befehl das `--no-wait`-Flag hinzufügen, um stattdessen die Steuerung an die Eingabeaufforderung zurückzugeben und die Bereitstellung im Hintergrund auszuführen. Auf diese Weise können Sie die Befehlszeilenschnittstelle für andere Aufgaben nutzen, während die Bereitstellung ausgeführt wird. Sie können dann mit dem Befehl [az vm show](/cli/azure/vm#show) Details zum Docker-Hoststatus anzeigen. Im folgenden Beispiel wird der Status des virtuellen Computers `myDockerVM` (der Standardname aus der Vorlage – ändern Sie diesen Namen nicht) in der Ressourcengruppe `myResourceGroup` überprüft:

```azurecli
az vm show --resource-group myResourceGroup --name myDockerVM \
  --query [provisioningState] --output tsv
```

Wenn diese Befehl `Succeeded` zurückgibt, ist die Bereitstellung abgeschlossen, und Sie können im nächsten Schritt eine SSH-Verbindung mit dem virtuellen Computer herstellen.

## <a name="step-2-verify-that-compose-is-installed"></a>Schritt 2: Vergewissern, dass Compose installiert ist
Stellen Sie nach Abschluss der Bereitstellung eine SSH-Verbindung mit dem neuen Docker-Host her. Verwenden Sie dazu den DNS-Namen, den Sie während der Bereitstellung angegeben haben. Sie können `azure vm show -g myResourceGroup -n myDockerVM` (Azure-Befehlszeilenschnittstelle 1.0) oder `az vm show -g myResourceGroup -n myDockerVM -d --query [fqdns] -o tsv` (Azure-Befehlszeilenschnittstelle 2.0 [Vorschau]) verwenden, um die Details Ihres virtuellen Computers anzuzeigen, beispielsweise den DNS-Namen.

Führen Sie den folgenden Befehl aus, um sicherzustellen, dass Compose auf dem virtuellen Computer installiert ist:

```bash
docker-compose --version
```

Die Ausgabe sieht in etwa wie folgt aus: `docker-compose 1.6.2, build 4d72027`.

> [!TIP]
> Wenn Sie den Docker-Host auf eine andere Art erstellt haben und Compose selbst installieren müssen, finden Sie in der [Compose-Dokumentation](https://github.com/docker/compose/blob/882dc673ce84b0b29cd59b6815cb93f74a6c4134/docs/install.md)weitere Informationen.


## <a name="step-3-create-a-docker-composeyml-configuration-file"></a>Schritt 3: Erstellen der Konfigurationsdatei „docker-compose.yml“
Als Nächstes erstellen Sie die Datei `docker-compose.yml` (eine reine Textkonfigurationsdatei), um die Docker-Container zu definieren, die auf dem virtuellen Computer ausgeführt werden sollen. Die Datei enthält Angaben zum Image, das für jeden Container ausgeführt werden soll (es kann auch ein Build einer Docker-Datei sein), erforderliche Umgebungsvariablen und Abhängigkeiten, Ports und Links zwischen Containern. Details zur Syntax der YML-Datei finden Sie in der [Compose-Dateireferenz](http://docs.docker.com/compose/yml/).

Erstellen Sie die Datei `docker-compose.yml` wie folgt:

```bash
touch docker-compose.yml
```

Verwenden Sie Ihren bevorzugten Text-Editor, um der Datei einige Daten hinzuzufügen. Im folgenden Beispiel wird der Editor `vi` verwendet:

```bash
vi docker-compose.yml
```

Fügen Sie das folgende Beispiel in die Textdatei ein. Diese Konfiguration nutzt Images aus der [Docker-Hub-Registrierung](https://registry.hub.docker.com/_/wordpress/) , um WordPress (Open-Source-Blogging- und Content Management-System) und die verknüpfte MariaDB SQL-Back-End-Datenbank zu installieren. Geben Sie Ihr eigenes `MYSQL_ROOT_PASSWORD` wie folgt ein:

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

## <a name="step-4-start-the-containers-with-compose"></a>Schritt 4: Starten des Containers mit Compose
Führen Sie in demselben Verzeichnis, in dem sich die Datei `docker-compose.yml` befindet, den folgenden Befehl aus (je nach Umgebung kann es erforderlich sein, `docker-compose` mit `sudo` auszuführen):

```bash
docker-compose up -d
```

Mit diesem Befehl werden die in `docker-compose.yml`angegebenen Docker-Container gestartet. Es dauert ein bis zwei Minuten, bis dieser Schritt abgeschlossen ist. Ihnen wird daraufhin eine Ausgabe angezeigt, die in etwa wie im folgenden Beispiel aussieht:

```bash
Creating wordpress_db_1...
Creating wordpress_wordpress_1...
...
```

> [!NOTE]
> Verwenden Sie zu Beginn die Option **-d** , damit die Container kontinuierlich im Hintergrund ausgeführt werden.


Um zu prüfen, ob die Container ausgeführt werden, geben Sie `docker-compose ps`ein. Die Ausgabe sollte folgendermaßen aussehen:

```bash
Name             Command             State              Ports
-------------------------------------------------------------------------
wordpress_db_1     /docker-           Up                 3306/tcp
             entrypoint.sh
             mysqld
wordpress_wordpr   /entrypoint.sh     Up                 0.0.0.0:80->80
ess_1              apache2-for ...                       /tcp
```

Sie können nun auf dem virtuellen Computer über Port 80 eine direkte Verbindung mit WordPress herstellen. Öffnen Sie einen Webbrowser, und geben Sie den DNS-Namen Ihrer VM ein (z.B. `http://myresourcegroup.westus.cloudapp.azure.com`). Daraufhin sollte der Startbildschirm von WordPress angezeigt werden, auf dem Sie die Installation abschließen und erste Schritte mit der Anwendung ausführen können.

![WordPress-Startbildschirm][wordpress_start]

## <a name="next-steps"></a>Nächste Schritte
* Weitere Optionen zum Konfigurieren von Docker und Compose auf dem virtuellen Docker-Computer finden Sie im [Leitfaden für die Azure-Docker-VM-Erweiterung](https://github.com/Azure/azure-docker-extension/blob/master/README.md) . Eine Möglichkeit wäre beispielsweise, die (in JSON konvertierte) Compose-YML-Datei direkt in die Konfiguration der Docker-VM-Erweiterung zu integrieren.
* Weitere Beispiele zum Erstellen und Bereitstellen von Apps mit mehreren Containern finden Sie in der [Composer-Befehlszeilenreferenz](http://docs.docker.com/compose/reference/) und im [Benutzerhandbuch](http://docs.docker.com/compose/).
* Verwenden Sie eine von Ihnen oder der [Community](https://azure.microsoft.com/documentation/templates/)erstellte Vorlage des Azure-Ressourcen-Managers, um eine Azure-VM mit Docker und eine mit Compose eingerichtete Anwendung bereitzustellen. Die Vorlage [WordPress-Blog mit Docker bereitstellen](https://github.com/Azure/azure-quickstart-templates/tree/master/docker-wordpress-mysql) verwendet Docker und Compose, um WordPress schnell mit einem MySQL-Back-End auf einem virtuellen Ubuntu-Computer bereitzustellen.
* Sie können Docker Compose auch in ein [Docker Swarm-Cluster](virtual-machines-linux-docker-swarm.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) integrieren. Entsprechende Szenarien finden Sie unter [Using Compose with Swarm](https://docs.docker.com/compose/swarm/) (Verwenden von Compose mit Swarm).

<!--Image references-->

[wordpress_start]: ./media/virtual-machines-linux-docker-compose-quickstart/WordPress.png



<!--HONumber=Feb17_HO2-->


