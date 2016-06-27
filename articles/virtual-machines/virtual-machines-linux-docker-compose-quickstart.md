<properties
   pageTitle="Docker und Compose auf einem virtuellen Computer | Microsoft Azure"
   description="Kurze Einführung in die Arbeit mit Compose und Docker auf virtuellen Linux-Computern in Azure"
   services="virtual-machines-linux"
   documentationCenter=""
   authors="dlepow"
   manager="timlt"
   editor=""
   tags="azure-resource-manager"/>

<tags
   ms.service="virtual-machines-linux"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="vm-linux"
   ms.workload="infrastructure-services"
   ms.date="06/10/2016"
   ms.author="danlep"/>

# Erste Schritte mit Docker und Compose zum Definieren und Ausführen einer Anwendung mit mehreren Containern auf einem virtuellen Azure-Computer

Einführung in Docker und [Compose](http://github.com/docker/compose) zum Definieren und Ausführen einer komplexen Anwendung auf einem virtuellen Linux-Computer in Azure. Bei Compose (dem Nachfolger von *Fig*) verwenden Sie eine einfache Textdatei zum Definieren einer Anwendung, die aus mehreren Docker-Containern besteht. Anschließend erstellen Sie Ihre Anwendung mit nur einem Befehl, der alle erforderlichen Schritte zur Ausführung der Anwendung auf dem virtuellen Computer ausführt.

In diesem Artikel wird beispielsweise veranschaulicht, wie Sie schnell einen WordPress-Blog mit einer MariaDB SQL-Back-End-Datenbank auf einem virtuellen Ubuntu-Computer einrichten. Sie können aber auch Compose verwenden, um komplexere Anwendungen einzurichten.

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]


Wenn Sie mit Docker und Containern noch nicht vertraut sind, lesen Sie die Informationen unter [Docker High Level Whiteboard](https://azure.microsoft.com/documentation/videos/docker-high-level-whiteboard/) (Whiteboard auf hoher Ebene zu Docker; in englischer Sprache).

## Schritt 1: Einrichten eines virtuellen Linux-Computers als Docker-Host

Im Azure Marketplace stehen Ihnen verschieden Azure-Verfahren und Images oder Resource Manager-Vorlagen zur Verfügung, um einen virtuellen Linux-Computer zu erstellen und als Docker-Host einzurichten. So finden Sie etwa unter [Verwenden der Docker-VM-Erweiterung zum Bereitstellen Ihrer Umgebung](virtual-machines-linux-dockerextension.md) ein schnelles Verfahren zum Erstellen eines virtuellen Ubuntu-Computers mit der Azure Docker-VM-Erweiterung unter Verwendung einer [Schnellstartvorlage](https://github.com/Azure/azure-quickstart-templates/tree/master/docker-simple-on-ubuntu). Bei Verwendung der Docker-VM-Erweiterung wird Ihr virtueller Computer automatisch als Docker-Host eingerichtet, und Compose ist bereits installiert. Das Beispiel in diesem Artikel veranschaulicht, wie Sie die [Azure-Befehlszeilenschnittstelle für Mac, Linux und Windows](../xplat-cli-install.md) (Azure-CLI) im Resource Manager-Modus verwenden, um den virtuellen Computer zu erstellen.

## Schritt 2: Vergewissern, dass Compose installiert ist

Wenn der virtuelle Linux-Computer mit Docker ausgeführt wird, stellen Sie von Ihrem Clientcomputer aus über SSH eine Verbindung mit ihm her.

Führen Sie zum Testen der Compose-Installation auf dem virtuellen Computer den folgenden Befehl aus:

```
$ docker-compose --version
```

Daraufhin wird eine Ausgabe angezeigt, die `docker-compose 1.6.2, build 4d72027` ähnelt.

>[AZURE.TIP] Wenn Sie den Docker-Host auf eine andere Art erstellt haben und Compose manuell installieren müssen, finden Sie weitere Informationen in der [Compose-Dokumentation](https://github.com/docker/compose/blob/882dc673ce84b0b29cd59b6815cb93f74a6c4134/docs/install.md).


## Schritt 3: Erstellen der Konfigurationsdatei „docker-compose.yml“

Als Nächstes erstellen Sie die Datei `docker-compose.yml` (eine reine Textkonfigurationsdatei), um die Docker-Container zu definieren, die auf dem virtuellen Computer ausgeführt werden sollen. Die Datei beinhaltet Angaben zum Image, das für jeden Container ausgeführt werden soll (es kann auch ein Build einer Docker-Datei sein), erforderliche Umgebungsvariablen und Abhängigkeiten, Ports, Links zwischen Containern usw. Details zur Syntax der YML-Datei finden Sie in der [Compose-Dateireferenz](http://docs.docker.com/compose/yml/).

Legen Sie ein Arbeitsverzeichnis auf Ihrem virtuellen Computer an, und erstellen Sie mit Ihrem bevorzugten Texteditor die Datei `docker-compose.yml`. Kopieren Sie probehalber den folgenden Text in die Datei. Diese Konfiguration nutzt Images aus der [Docker-Hub-Registrierung](https://registry.hub.docker.com/_/wordpress/), um WordPress (Open-Source-Blogging- und Content Management-System) und die verknüpfte MariaDB SQL-Back-End-Datenbank zu installieren.

 ```
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

## Schritt 4: Starten des Containers mit Compose

Führen Sie im Arbeitsverzeichnis auf Ihrem virtuellen Computer einfach den folgenden Befehl aus. (Je nach Umgebung muss `docker-compose` möglicherweise mit `sudo` ausgeführt werden.)

```
$ docker-compose up -d

```

Dadurch werden die in `docker-compose.yml` angegebenen Docker-Container gestartet. Die Ausgabe sieht in etwa wie folgt aus:

```
Creating wordpress_db_1...
Creating wordpress_wordpress_1...
```

>[AZURE.NOTE] Verwenden Sie zu Beginn die Option **-d**, damit die Container kontinuierlich im Hintergrund ausgeführt werden.

Um zu prüfen, ob die Container ausgeführt werden, geben Sie `docker-compose ps` ein. Die Ausgabe sollte folgendermaßen aussehen:

```
Name             Command             State              Ports
-------------------------------------------------------------------------
wordpress_db_1     /docker-           Up                 3306/tcp
             entrypoint.sh
             mysqld
wordpress_wordpr   /entrypoint.sh     Up                 0.0.0.0:80->80
ess_1              apache2-for ...                       /tcp
```

Sie können nun auf dem virtuellen Computer über Port 80 eine direkte Verbindung mit WordPress herstellen. Wenn Sie den virtuellen Computer mithilfe einer Resource Manager-Vorlage erstellt haben, versuchen Sie, eine Verbindung mit `http://<dnsname>.<region>.cloudapp.azure.com` herzustellen. Wenn Sie den virtuellen Computer unter Verwendung des klassischen Bereitstellungsmodells erstellt haben, verwenden Sie `http://<cloudservicename>.cloudapp.net`. Daraufhin sollte der Startbildschirm von WordPress angezeigt werden, auf dem Sie die Installation abschließen und erste Schritte mit der Anwendung ausführen können.

![WordPress-Startbildschirm][wordpress_start]


## Nächste Schritte

* Weitere Optionen zum Konfigurieren von Docker und Compose auf dem virtuellen Docker-Computer finden Sie im [Benutzerhandbuch für die Azure-Docker-VM-Erweiterung](https://github.com/Azure/azure-docker-extension/blob/master/README.md). Eine Möglichkeit wäre beispielsweise, die (in JSON konvertierte) Compose-YML-Datei direkt in die Konfiguration der Docker-VM-Erweiterung zu integrieren.
* Weitere Beispiele zum Erstellen und Bereitstellen von Apps mit mehreren Containern finden Sie in der [Composer-Befehlszeilenreferenz](http://docs.docker.com/compose/reference/) und im [Benutzerhandbuch](http://docs.docker.com/compose/).
* Verwenden Sie eine von Ihnen oder der [Community](https://azure.microsoft.com/documentation/templates/) erstellte Vorlage des Azure-Ressourcen-Managers, um eine Azure-VM mit Docker und eine mit Compose eingerichtete Anwendung bereitzustellen. Die Vorlage [WordPress-Blog mit Docker bereitstellen](https://github.com/Azure/azure-quickstart-templates/tree/master/docker-wordpress-mysql) verwendet Docker und Compose, um WordPress schnell mit einem MySQL-Back-End auf einem virtuellen Ubuntu-Computer bereitzustellen.
* Sie können Docker Compose auch in ein [Docker Swarm-Cluster](virtual-machines-linux-docker-swarm.md) integrieren. Entsprechende Szenarien finden Sie unter [Using Compose with Swarm](https://docs.docker.com/compose/swarm/) (Verwenden von Compose mit Swarm).

<!--Image references-->

[wordpress_start]: ./media/virtual-machines-linux-docker-compose-quickstart/WordPress.png

<!---HONumber=AcomDC_0615_2016-->