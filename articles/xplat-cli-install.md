<properties
	pageTitle="Installieren der Azure-Befehlszeilenschnittstelle | Microsoft Azure"
	description="Installieren der Azure-Befehlszeilenschnittstelle (CLI) für Mac, Linux und Windows, um Azure Services zu nutzen"
	editor=""
	manager="timlt"
	documentationCenter=""
	authors="dlepow"
	services="virtual-machines-linux,virtual-network,storage,azure-resource-manager"
	tags="azure-resource-manager,azure-service-management"/>

<tags
	ms.service="multiple"
	ms.workload="multiple"
	ms.tgt_pltfrm="command-line-interface"
	ms.devlang="na"
	ms.topic="article"
	ms.date="08/17/2016"
	ms.author="danlep"/>
    
# Installieren der Azure-Befehlszeilenschnittstelle

> [AZURE.SELECTOR]
- [PowerShell](powershell-install-configure.md)
- [Azure-Befehlszeilenschnittstelle](xplat-cli-install.md)

Installieren Sie schnell die Azure-Befehlszeilenschnittstelle (Azure-CLI), um eine Reihe von auf der Open-Source-Shell basierenden Befehlen zum Erstellen und Verwalten von Ressourcen in Microsoft Azure zu verwenden. Es gibt mehrere Möglichkeiten, um die neueste Version zu installieren:

* Installieren von einem npm-Paket (erfordert Node.js und npm)
* Verwenden eines der bereitgestellten Installationspakete für verschiedene Betriebssysteme
* Installieren der Azure-CLI als Container in einem Docker-Host
    
Weitere Optionen und Hintergrundinformationen finden Sie im Projektrepository auf [GitHub](https://github.com/azure/azure-xplat-cli).

Sobald die Azure-CLI installiert ist, [verbinden Sie sie mit Ihrem Azure-Abonnement](xplat-cli-connect.md), und führen Sie die **azure**-Befehle in der Befehlszeilenschnittstelle (Bash, Terminal, Eingabeaufforderung usw.) für die Arbeit mit Ihren Azure-Ressourcen aus.



## Option 1. Installieren eines npm-Pakets

Um die CLI aus einem npm-Paket zu installieren, müssen auf Ihrem System die aktuellen Versionen von Node.js und npm installiert sein. Führen Sie dann den folgenden Befehl aus, um die Azure-CLI zu installieren. (Bei Linux-Distributionen müssen Sie möglicherweise **sudo** verwenden, um den Befehl __npm__ erfolgreich ausführen zu können.)

	npm install azure-cli -g

> [AZURE.NOTE]Falls Sie Node.js und npm für Ihr Betriebssystem installieren oder aktualisieren müssen, finden Sie weitere Informationen unter [Nodejs.org](https://nodejs.org/en/download/package-manager/). Wir empfehlen die Installation der aktuellsten LTS-Version von Node.js (4.x). Wenn Sie eine ältere Version verwenden, kommt es möglicherweise zu Fehlern bei der Installation. Weitere Informationen zu npm finden Sie unter [npmjs.com](https://www.npmjs.com/).

## Option 2. Verwenden eines Installers

Die folgenden CLI-Installationspakete stehen ebenfalls zum Download bereit:


* [Mac OS X-Installer][mac-installer]

* [Windows Installer][windows-installer]

* [Tar-Datei für Linux][linux-installer] \(erfordert Node.js und npm): Installation durch Ausführen von `sudo npm install -g <path to downloaded tar file>`


## Option 3. Verwenden eines Docker-Containers

Wenn Sie einen Docker-Host eingerichtet haben, können Sie die Azure-CLI in einem Docker-Container ausführen. Führen Sie folgenden Befehl aus:

```
docker run -it microsoft/azure-cli
```

Weitere Informationen zu Docker finden Sie unter [docker.com](https://docs.docker.com/engine/understanding-docker/).

## Ausführen von Azure-CLI-Befehlen
Sobald die Azure-CLI installiert ist, führen Sie den Befehl **azure** in der CLI (Bash, Terminal, Eingabeaufforderung usw.) aus. Um beispielsweise den Hilfebefehl auszuführen, geben Sie Folgendes ein:

```
azure help
```
> [AZURE.NOTE]In einigen Linux-Distributionen erhalten Sie unter Umständen eine Fehlermeldung wie `/usr/bin/env: ‘node’: No such file or directory`. Dieser Fehler stammt von jüngsten Installationen von Node.js, die unter „/usr/bin/nodejs“ installiert sind. Erstellen Sie zum Beheben dieses Fehlers eine symbolische Verknüpfung mit „/usr/bin/node“, indem Sie den folgenden Befehl ausführen:

```
sudo ln -s /usr/bin/nodejs /usr/bin/node
```

Um die installierte Version der Azure-Befehlszeilenschnittstelle anzuzeigen, geben Sie Folgendes ein:

```
azure --version
```

Jetzt sind Sie soweit. Um Zugriff auf alle CLI-Befehle für die Arbeit mit Ihren eigenen Ressourcen zu erhalten, [stellen Sie über die Azure-CLI eine Verbindung mit Ihrem Azure-Abonnement her](xplat-cli-connect.md).

>[AZURE.NOTE] Wenn Sie die Azure-CLI zum ersten Mal verwenden, werden Sie in einer Meldung gefragt, ob Sie Microsoft das Sammeln von Informationen zur Verwendung der CLI erlauben möchten. Die Teilnahme ist freiwillig. Wenn Sie sich für die Teilnahme entscheiden, können Sie diese durch Ausführen von `azure telemetry --disable` jederzeit beenden. Zum Aktivieren der Teilnahme können Sie jederzeit `azure telemetry --enable` ausführen.


## Aktualisieren der CLI

Microsoft veröffentlicht regelmäßig aktualisierte Versionen der Azure-Befehlszeilenschnittstelle. Installieren Sie die CLI erneut, indem Sie das Installationsprogramm für Ihr Betriebssystem verwenden, oder führen Sie den neuesten Docker-Container aus. Oder wenn Node.js und npm installiert sind, aktualisieren Sie sie, indem Sie Folgendes eingeben (bei Linux-Distributionen müssen Sie möglicherweise **sudo** verwenden).

```
npm update -g azure-cli
```

## Aktivieren der Vervollständigung mit der TAB-Taste

Die Vervollständigung mit der TAB-Taste für CLI-Befehle wird für Mac und Linux unterstützt.

Für die Aktivierung in zsh führen Sie Folgendes aus:

```
echo '. <(azure --completion)' >> .zshrc
```

Für die Aktivierung in bash führen Sie Folgendes aus:

```
azure --completion >> ~/azure.completion.sh
echo 'source ~/azure.completion.sh' >> ~/.bash_profile
```


## Nächste Schritte 

* [Verbinden mit Ihrem Azure-Abonnement über die CLI](xplat-cli-connect.md) zum Erstellen und Verwalten von Azure-Ressourcen.

* Wenn Sie weitere Informationen zur Azure-Befehlszeilenschnittstelle erhalten, den Quellcode herunterladen, Probleme melden oder etwas zum Projekt beitragen möchten, besuchen Sie die Webseite [GitHub repository for the Azure CLI](https://github.com/azure/azure-xplat-cli).

* Wenn Sie Fragen zur Verwendung der Azure-CLI oder zu Azure haben, besuchen Sie die [Azure-Foren](https://social.msdn.microsoft.com/Forums/de-DE/home?forum=azurescripting).

* Bei Linux-Systemen können Sie die Azure-Befehlszeilenschnittstelle auch installieren, erstellen Sie sie aus dem [Quellcode](http://aka.ms/linux-azure-cli) neu erstellen. Weitere Informationen zum Erstellen aus dem Quellcode finden Sie in der Datei INSTALL, die im Quellarchiv enthalten ist.

[mac-installer]: http://aka.ms/mac-azure-cli
[windows-installer]: http://aka.ms/webpi-azure-cli
[linux-installer]: http://aka.ms/linux-azure-cli
[cliasm]: virtual-machines-command-line-tools.md
[cliarm]: ./virtual-machines/azure-cli-arm-commands.md

<!---HONumber=AcomDC_0907_2016-->