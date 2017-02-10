---
title: Docker und Compose auf einem virtuellen Computer | Microsoft Docker
description: "Kurze Einführung in die Arbeit mit Compose und Docker auf virtuellen Linux-Computern in Azure"
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
ms.date: 09/22/2016
ms.author: iainfou
translationtype: Human Translation
ms.sourcegitcommit: 63cf1a5476a205da2f804fb2f408f4d35860835f
ms.openlocfilehash: 6433922d1acfad4e84f57a51d6ebb5b6fdea31b1


---
# <a name="get-started-with-docker-and-compose-to-define-and-run-a-multi-container-application-on-an-azure-virtual-machine"></a>Erste Schritte mit Docker und Compose zum Definieren und Ausführen einer Anwendung mit mehreren Containern auf einem virtuellen Azure-Computer
Einführung in Docker und [Compose](http://github.com/docker/compose) zum Definieren und Ausführen einer komplexen Anwendung auf einem virtuellen Linux-Computer in Azure. Bei Compose verwenden Sie eine einfache Textdatei zum Definieren einer Anwendung, die aus mehreren Docker-Containern besteht. Sie starten Ihre Anwendung dann mit einem einzelnen Befehl, mit dem alle Schritte zur Bereitstellung Ihrer definierten Umgebung ausgeführt werden. 

In diesem Artikel wird beispielsweise veranschaulicht, wie Sie schnell einen WordPress-Blog mit einer MariaDB SQL-Back-End-Datenbank auf einem virtuellen Ubuntu-Computer einrichten. Sie können aber auch Compose verwenden, um komplexere Anwendungen einzurichten.

## <a name="step-1-set-up-a-linux-vm-as-a-docker-host"></a>Schritt 1: Einrichten eines virtuellen Linux-Computers als Docker-Host
Im Azure Marketplace stehen Ihnen verschieden Azure-Verfahren und Images oder Resource Manager-Vorlagen zur Verfügung, um einen virtuellen Linux-Computer zu erstellen und als Docker-Host einzurichten. So finden Sie etwa unter [Verwenden der Docker-VM-Erweiterung zum Bereitstellen Ihrer Umgebung](virtual-machines-linux-dockerextension.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) ein schnelles Verfahren zum Erstellen eines virtuellen Ubuntu-Computers mit der Azure Docker-VM-Erweiterung unter Verwendung einer [Schnellstartvorlage](https://github.com/Azure/azure-quickstart-templates/tree/master/docker-simple-on-ubuntu). 

Bei Verwendung der Docker-VM-Erweiterung wird Ihr virtueller Computer automatisch als Docker-Host eingerichtet, und Compose ist bereits installiert. Das Beispiel in diesem Artikel veranschaulicht, wie Sie die [Azure-Befehlszeilenschnittstelle für Mac, Linux und Windows](../xplat-cli-install.md) (Azure-CLI) im Resource Manager-Modus verwenden, um den virtuellen Computer zu erstellen.

Mit dem grundlegenden Befehl aus dem vorherigen Dokument wird eine Ressourcengruppe mit dem Namen `myResourceGroup` für den Standort `West US` erstellt und eine VM mit der Docker-VM-Erweiterung von Azure installiert:

```azurecli
azure group create --name myResourceGroup --location "West US" \
  --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/docker-simple-on-ubuntu/azuredeploy.json
```

## <a name="step-2-verify-that-compose-is-installed"></a>Schritt 2: Vergewissern, dass Compose installiert ist
Stellen Sie nach Abschluss der Bereitstellung eine SSH-Verbindung mit dem neuen Docker-Host her. Verwenden Sie dazu den DNS-Namen, den Sie während der Bereitstellung angegeben haben. Sie können `azure vm show -g myDockerResourceGroup -n myDockerVM` verwenden, um die Details Ihrer VM anzuzeigen, einschließlich DNS-Name.

Führen Sie den folgenden Befehl aus, um sicherzustellen, dass Compose auf dem virtuellen Computer installiert ist:

```bash
docker-compose --version
```

Die Ausgabe sieht in etwa wie folgt aus: `docker-compose 1.6.2, build 4d72027`.

> [!TIP]
> Wenn Sie den Docker-Host auf eine andere Art erstellt haben und Compose selbst installieren müssen, finden Sie in der [Compose-Dokumentation](https://github.com/docker/compose/blob/882dc673ce84b0b29cd59b6815cb93f74a6c4134/docs/install.md)weitere Informationen.
> 
> 

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
> 
> 

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



<!--HONumber=Nov16_HO3-->


