---
title: Installieren von Azure CLI 1.0 | Microsoft Docs
description: "Installieren von Azure CLI 1.0 für Mac, Linux und Windows für das Starten von Azure-Diensten"
editor: 
manager: timlt
documentationcenter: 
author: squillace
services: virtual-machines-linux,virtual-network,storage,azure-resource-manager
tags: azure-resource-manager,azure-service-management
ms.assetid: bdb776c8-7a76-4f3a-887c-236b4fffee10
ms.service: multiple
ms.workload: multiple
ms.tgt_pltfrm: command-line-interface
ms.devlang: na
ms.topic: article
ms.date: 03/20/2017
ms.author: rasquill
translationtype: Human Translation
ms.sourcegitcommit: 0d8472cb3b0d891d2b184621d62830d1ccd5e2e7
ms.openlocfilehash: de3f8cb4a603b3aab7cef21b06f9615ce7d892cb
ms.lasthandoff: 03/21/2017


---
# <a name="install-the-azure-cli-10"></a>Installieren von Azure CLI 1.0
> [!div class="op_single_selector"]
> * [PowerShell](https://docs.microsoft.com/powershell/azureps-cmdlets-docs)
> * [Azure CLI 1.0](cli-install-nodejs.md)
> * [Azure CLI 2.0](/cli/azure/install-azure-cli)

> [!IMPORTANT]
> In diesem Thema wird beschrieben, wie Azure CLI 1.0 installiert wird. Die Befehlszeilenschnittstelle basiert auf Node.js und unterstützt alle klassischen Bereitstellungs-API-Aufrufe sowie eine große Anzahl von Resource Manager-Bereitstellungsaktivitäten. Verwenden Sie [Azure CLI 2.0](/cli/azure/overview) für neue oder langfristig ausgelegte CLI-Bereitstellung und die Verwaltung.

Installieren Sie schnell die Azure-Befehlszeilenschnittstelle (Azure CLI 1.0), um eine Reihe von auf der Shell basierenden Open-Source-Befehlen für das Erstellen und Verwalten von Ressourcen in Microsoft Azure zu verwenden. Zum Installieren dieser plattformübergreifenden Tools auf Ihrem Computer stehen Ihnen mehrere Optionen zur Verfügung:

* **npm-Paket:** Führen Sie npm (den Paket-Manager für JavaScript) aus, um das neueste Azure CLI 1.0-Paket auf Ihrer Linux-Distribution oder dem Betriebssystem zu installieren. Dafür sind Node.js und npm auf Ihrem Computer erforderlich.
* **Installer** : Laden Sie ein Installationsprogramm für die einfache Installation unter Mac OS oder Windows herunter.
* **Docker-Container** : Beginnen Sie mit der Verwendung der neuesten Befehlszeilenschnittstelle in einem sofort betriebsbereiten Docker-Container. Dazu benötigen Sie den Docker-Host auf Ihrem Computer.

Weitere Optionen und Hintergrundinformationen finden Sie im Projektrepository auf [GitHub](https://github.com/azure/azure-xplat-cli).

Sobald Azure CLI 1.0 installiert ist, [verbinden Sie die Befehlszeilenschnittstelle mit Ihrem Azure-Abonnement](xplat-cli-connect.md), und führen Sie die **azure**-Befehle über die Befehlszeilenschnittstelle (Bash, Terminal, Eingabeaufforderung usw.) für die Arbeit mit Ihren Azure-Ressourcen aus.

## <a name="option-1-install-an-npm-package"></a>Option 1: Installieren eines npm-Pakets
Um die CLI aus einem npm-Paket zu installieren, müssen Sie die aktuellen Versionen von [Node.js und npm](https://nodejs.org/en/download/package-manager/) herunterladen und installieren. Führen Sie anschließend zum Installieren des Azure-CLI-Pakets **npm install** aus:

```bash
npm install -g azure-cli
```

Bei Linux-Distributionen müssen Sie möglicherweise **sudo** wie folgt verwenden, um den Befehl **npm** erfolgreich ausführen zu können:

```bash
sudo npm install -g azure-cli
```

> [!NOTE]
> Falls Sie Node.js und npm für Ihre Linux-Distribution oder Ihr Betriebssystem installieren oder aktualisieren müssen, wird die Installation der aktuellsten LTS-Version von Node.js (4.x) empfohlen. Wenn Sie eine ältere Version verwenden, kommt es möglicherweise zu Fehlern bei der Installation.

Falls gewünscht, laden Sie die neuesten Linux-[TAR-Datei][linux-installer] für das npm-Paket lokal herunter. Installieren Sie dann das heruntergeladene npm-Paket wie folgt (in Linux-Distributionen müssen Sie möglicherweise **sudo**verwenden):

```bash
npm install -g <path to downloaded tar file>
```

## <a name="option-2-use-an-installer"></a>Option 2: Verwenden eines Installers
Wenn Sie einen Mac- oder Windows-Computer verwenden, stehen die folgenden CLI-Installer zum Download zur Verfügung:

* [Mac OS X-Installer][mac-installer]
* [Windows-MSI][windows-installer]

> [!TIP]
> Unter Windows können Sie auch den [Webplattform-Installer](https://go.microsoft.com/?linkid=9828653) herunterladen, um die Befehlszeilenschnittstelle zu installieren. Dieser Installer bietet Ihnen die Möglichkeit, nach der Installation der Befehlszeilenschnittstelle zusätzliche Azure SDK- und -Befehlszeilentools zu installieren.

## <a name="option-3-use-a-docker-container"></a>Option 3: Verwenden eines Docker-Containers
Wenn Sie Ihren Computer als [Docker](https://docs.docker.com/engine/understanding-docker/)-Host eingerichtet haben, können Sie die neueste Azure CLI 1.0 in einem Docker-Container ausführen. Führen Sie den folgenden Befehl aus (bei Linux-Distributionen müssen Sie möglicherweise **sudo** verwenden):

```bash
docker run -it microsoft/azure-cli
```

## <a name="run-azure-cli-10-commands"></a>Ausführen von Azure CLI 1.0-Befehlen
Sobald Azure CLI 1.0 installiert ist, führen Sie den Befehl **azure** in der Befehlszeilenschnittstelle (Bash, Terminal, Eingabeaufforderung usw.) aus. Um beispielsweise den Hilfebefehl auszuführen, geben Sie Folgendes ein:

```azurecli
azure help
```

> [!NOTE]
> In einigen Linux-Distributionen erhalten Sie unter Umständen eine Fehlermeldung wie `/usr/bin/env: ‘node’: No such file or directory`. Dieser Fehler stammt von kürzlich durchgeführten Installationen von „Node.js“, die unter „/usr/bin/nodejs“ installiert sind. Erstellen Sie zum Beheben dieses Fehlers eine symbolische Verknüpfung mit „/usr/bin/node“, indem Sie den folgenden Befehl ausführen:

```bash
sudo ln -s /usr/bin/nodejs /usr/bin/node
```

Um die installierte Version von Azure CLI 1.0 anzuzeigen, geben Sie Folgendes ein:

```azurecli
azure --version
```

Jetzt sind Sie soweit. Um Zugriff auf alle CLI-Befehle für die Arbeit mit Ihren eigenen Ressourcen zu erhalten, [stellen Sie über die Azure-CLI eine Verbindung mit Ihrem Azure-Abonnement her](xplat-cli-connect.md).

> [!NOTE]
> Wenn Sie die Azure-CLI zum ersten Mal verwenden, werden Sie in einer Meldung gefragt, ob Sie Microsoft das Erfassen von Nutzungsinformationen erlauben möchten. Die Teilnahme ist freiwillig. Wenn Sie sich für die Teilnahme entscheiden, können Sie diese durch Ausführen von `azure telemetry --disable`jederzeit beenden. Zum Aktivieren der Teilnahme können Sie jederzeit `azure telemetry --enable`ausführen.

## <a name="update-the-cli"></a>Aktualisieren der CLI
Microsoft veröffentlicht regelmäßig aktualisierte Versionen der Azure-Befehlszeilenschnittstelle. Installieren Sie die CLI erneut, indem Sie das Installationsprogramm für Ihr Betriebssystem verwenden, oder führen Sie den neuesten Docker-Container aus. Oder wenn Node.js und npm installiert sind, aktualisieren Sie sie, indem Sie Folgendes eingeben (bei Linux-Distributionen müssen Sie möglicherweise **sudo**verwenden).

```bash
npm update -g azure-cli
```

## <a name="enable-tab-completion"></a>Aktivieren der Vervollständigung mit der TAB-Taste
Die Vervollständigung mit der TAB-Taste für CLI-Befehle wird für Mac und Linux unterstützt.

Für die Aktivierung in zsh führen Sie Folgendes aus:

```bash
echo '. <(azure --completion)' >> .zshrc
```

Für die Aktivierung in bash führen Sie Folgendes aus:

```bash
azure --completion >> ~/azure.completion.sh
echo 'source ~/azure.completion.sh' >> ~/.bash_profile
```


## <a name="next-steps"></a>Nächste Schritte
* [Verbinden mit Ihrem Azure-Abonnement über die CLI](xplat-cli-connect.md) zum Erstellen und Verwalten von Azure-Ressourcen.
* Wenn Sie weitere Informationen zur Azure-Befehlszeilenschnittstelle erhalten, den Quellcode herunterladen, Probleme melden oder etwas zum Projekt beitragen möchten, besuchen Sie die Webseite [GitHub repository for the Azure CLI](https://github.com/azure/azure-xplat-cli).
* Wenn Sie Fragen zur Verwendung der Azure-CLI oder zu Azure haben, besuchen Sie die [Azure-Foren](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurescripting).


[mac-installer]: http://aka.ms/mac-azure-cli
[windows-installer]: http://aka.ms/webpi-azure-cli
[linux-installer]: http://aka.ms/linux-azure-cli
[cliasm]: /cli/azure/get-started-with-az-cli2
[cliarm]: ./virtual-machines/azure-cli-arm-commands.md

