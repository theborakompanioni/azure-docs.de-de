<properties
	pageTitle="Installieren der Azure-Befehlszeilenschnittstelle | Microsoft Azure"
	description="Installieren der Azure-Befehlszeilenschnittstelle (CLI) für Mac, Linux und Windows, um Azure Services zu nutzen"
	editor=""
	manager="timlt"
	documentationCenter=""
	authors="dlepow"
	services=""
	tags="azure-resource-manager,azure-service-management"/>

<tags
	ms.service="multiple"
	ms.workload="multiple"
	ms.tgt_pltfrm="command-line-interface"
	ms.devlang="na"
	ms.topic="article"
	ms.date="04/20/2016"
	ms.author="danlep"/>
    
# Installieren der Azure-Befehlszeilenschnittstelle

> [AZURE.SELECTOR]
- [PowerShell](powershell-install-configure.md)
- [Azure-Befehlszeilenschnittstelle](xplat-cli-install.md)

Installieren Sie schnell die Azure-Befehlszeilenschnittstelle (Azure-CLI), um eine Reihe von auf der Open-Source-Shell basierenden Befehlen zum Erstellen und Verwalten von Ressourcen in Microsoft Azure zu verwenden. Sie haben mehrere Möglichkeiten: Verwenden Sie eines der bereitgestellten Installationspakete für verschiedene Betriebssysteme, installieren Sie von einem npm-Paket aus, oder installieren Sie die Azure-Befehlszeilenschnittstelle als Container auf einem Docker-Host. Weitere Optionen und Hintergrundinformationen finden Sie im Projektrepository auf [GitHub](https://github.com/azure/azure-xplat-cli).


Sobald die Azure-Befehlszeilenschnittstelle installiert ist, können Sie sie [mit Ihrem Azure-Abonnement verbinden](xplat-cli-connect.md) und die **azure**-Befehle in der Befehlszeilenschnittstelle (Bash, Terminal, Eingabeaufforderung usw.) für die Arbeit mit Ihren Azure-Ressourcen verwenden.


## Verwenden eines Installers

Die folgenden Installer-Pakete sind verfügbar:

* [Windows Installer][windows-installer]

* [OS X Installer][mac-installer]

* [Linux Installer][linux-installer]


## Installieren eines npm-Pakets

Wenn die aktuellsten Versionen von Node.js und npm bereits auf Ihrem System installiert sind, führen Sie alternativ den folgenden Befehl aus, um die Azure-Befehlszeilenschnittstelle zu installieren (Bei Linux-Distributionen müssen Sie möglicherweise **sudo** verwenden, um den Befehl __npm__ erfolgreich ausführen zu können).

	npm install azure-cli -g

> [AZURE.NOTE]Falls Sie Node.js und npm für Ihr Betriebssystem installieren oder aktualisieren müssen, finden Sie weitere Informationen unter [Nodejs.org](https://nodejs.org/en/download/package-manager/). Wir empfehlen die Installation der aktuellsten LTS-Version von Node.js (4.x). Wenn Sie eine ältere Version verwenden, kommt es möglicherweise zu Fehlern bei der Installation.


## Verwenden eines Docker-Containers

Führen Sie auf einem Docker-Host folgenden Befehl aus:

```
docker run -it microsoft/azure-cli
```

## Ausführen von Azure-CLI-Befehlen
Sobald die Azure-Befehlszeilenschnittstelle installiert ist, können Sie den Befehl **azure** in der Befehlszeilenschnittstelle (Bash, Terminal, Eingabeaufforderung usw.) ausführen. Um beispielsweise den Hilfebefehl auszuführen, geben Sie Folgendes ein:

```
azure help
```
> [AZURE.NOTE]Bei einigen Linux-Distributionen erhalten Sie möglicherweise die folgende Fehlermeldung: /usr/bin/env: „Knoten“: Datei oder Verzeichnis nicht vorhanden. Diese stammt von jüngsten Installationen von Nodejs, das unter „/usr/bin/nodejs“ installiert wird. Erstellen Sie zum Beheben dieses Fehlers eine symbolische Verknüpfung mit „/usr/bin/node“, indem Sie den folgenden Befehl ausführen:

```
sudo ln -s /usr/bin/nodejs /usr/bin/node
```

Um die installierte Version der Azure-Befehlszeilenschnittstelle anzuzeigen, geben Sie Folgendes ein:

```
azure --version
```

Jetzt sind Sie soweit. Um Zugriff auf alle CLI-Befehle für die Arbeit mit Ihren eigenen Ressourcen zu erhalten, [stellen Sie über die Azure-Befehlszeilenschnittstelle eine Verbindung mit Ihrem Azure-Abonnement her](xplat-cli-connect.md).

>[AZURE.NOTE] Wenn Sie zuerst die Azure CLI-Version 0.9.20 oder später verwenden, werden Sie in einer Meldung gefragt, ob Sie Microsoft das Sammeln von Informationen zur Verwendung der Befehlszeilenschnittstelle erlauben möchten. Die Teilnahme ist freiwillig. Wenn Sie sich zur Teilnahme entscheiden, Sie können diese durch Ausführen von `azure telemetry --disable` jederzeit beenden. Zum Aktivieren der Teilnahme können Sie jederzeit `azure telemetry --enable` ausführen.


## Aktualisieren der CLI

Microsoft veröffentlicht regelmäßig aktualisierte Versionen der Azure-Befehlszeilenschnittstelle. Installieren Sie die CLI erneut mit dem Installationsprogramm für Ihr Betriebssystem. Wenn Node.js und npm installiert sind, aktualisieren Sie sie, indem Sie Folgendes eingeben (bei Linux-Distributionen müssen Sie möglicherweise **sudo** verwenden):

```
npm update -g azure-cli
```

## Nächste Schritte 

* [Verbinden mit Ihrem Azure-Abonnement über die Befehlszeilenschnittstelle](xplat-cli-connect.md) zum Erstellen und Verwalten von Azure-Ressourcen.

* Wenn Sie weitere Informationen zur Azure-Befehlszeilenschnittstelle erhalten, den Quellcode herunterladen, Probleme melden oder etwas zum Projekt beitragen möchten, besuchen Sie die Webseite [GitHub repository for the Azure CLI](https://github.com/azure/azure-xplat-cli).

* Wenn Sie Fragen zur Verwendung der Azure-Befehlszeilenschnittstelle oder zu Azure haben, besuchen Sie die [Azure-Foren](https://social.msdn.microsoft.com/Forums/de-DE/home?forum=azurescripting).

* Bei Linux-Systemen können Sie die Azure-Befehlszeilenschnittstelle auch installieren, erstellen Sie sie aus dem [Quellcode](http://aka.ms/linux-azure-cli) neu erstellen. Weitere Informationen zum Erstellen aus dem Quellcode finden Sie in der Datei INSTALL, die im Quellarchiv enthalten ist.

[mac-installer]: http://aka.ms/mac-azure-cli
[windows-installer]: https://www.microsoft.com/web/handlers/webpi.ashx?command=getinstallerredirect&appid=windowsazurexplatcli&mode=new
[linux-installer]: http://aka.ms/linux-azure-cli
[cliasm]: virtual-machines-command-line-tools.md
[cliarm]: ./virtual-machines/azure-cli-arm-commands.md

<!---HONumber=AcomDC_0518_2016-->