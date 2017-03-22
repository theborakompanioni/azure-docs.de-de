---
title: "Einführung in FreeBSD in Azure | Microsoft Docs"
description: Erfahren Sie, wie Sie virtuelle FreeBSD-Computer auf Azure verwenden
services: virtual-machines-linux
documentationcenter: 
author: KylieLiang
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: 32b87a5f-d024-4da0-8bf0-77e233d1422b
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 02/28/2017
ms.author: kyliel
translationtype: Human Translation
ms.sourcegitcommit: 24410a07995d5ac813b2bf4cdeed320c72ce7e06
ms.openlocfilehash: 7845b552bd1360927eae414f57fefbd74ac0b7f7
ms.lasthandoff: 03/01/2017


---
# <a name="introduction-to-freebsd-on-azure"></a>Einführung in FreeBSD in Azure
Dieses Thema enthält einen Überblick über die Ausführung eines virtuellen FreeBSD-Computers in Azure.

## <a name="overview"></a>Übersicht
FreeBSD für Microsoft Azure ist ein erweitertes Computerbetriebssystem, das für moderne Server, Desktops und eingebettete Plattformen verwendet wird.

Die Microsoft Corporation stellt Images von FreeBSD unter Azure zur Verfügung, bei denen der [Gast-Agent für virtuelle Azure-Computer](https://github.com/Azure/WALinuxAgent/) vorkonfiguriert ist. Derzeit werden die folgenden FreeBSD-Versionen von Microsoft als Image angeboten:

- FreeBSD 10.3-RELEASE
- FreeBSD 11.0-RELEASE

Der Agent ist verantwortlich für die Kommunikation zwischen der FreeBSD-VM und Azure Fabric für Vorgänge wie beispielsweise das Bereitstellen der VM für die erste Verwendung (Benutzername, Kennwort oder SSH-Schlüssel, Hostname usw.) und das Aktivieren von Funktionalität für selektive VM-Erweiterungen.

Die Strategie für künftige Versionen von FreeBSD ist, stets aktuell zu bleiben und die neuesten Versionen kurz nach ihrer Veröffentlichung durch das FreeBSD Release Engineering-Team verfügbar zu machen.

## <a name="deploying-a-freebsd-virtual-machine"></a>Bereitstellen eines virtuellen FreeBSD-Computers
Die Bereitstellung eines virtuellen FreeBSD-Computers ist ein unkomplizierter Prozess, wenn Sie ein Image aus dem Azure Marketplace vom Azure-Portal verwenden:

- [FreeBSD 10.3 auf Azure Marketplace](https://azure.microsoft.com/marketplace/partners/microsoft/freebsd103/)
- [FreeBSD 11.0 auf Azure Marketplace](https://azure.microsoft.com/marketplace/partners/microsoft/freebsd110/)

### <a name="create-a-freebsd-vm-through-azure-cli-20-on-freebsd"></a>Erstellen einer FreeBSD-VM über Azure CLI 2.0 auf FreeBSD
Zunächst müssen Sie [Azure CLI 2.0](https://docs.microsoft.com/cli/azure/get-started-with-azure-cli) über den folgenden Befehl auf einem FreeBSD-Computer installieren.

```bash 
    curl -L https://aka.ms/InstallAzureCli | bash
```

Wenn Bash nicht auf Ihrem FreeBSD-Computer installiert ist, führen Sie vor der Installation den folgenden Befehl aus. 

```
    sudo pkg install bash
```

Wenn Python nicht auf Ihrem FreeBSD-Computer installiert ist, führen Sie vor der Installation die folgenden Befehle aus. 

```
    sudo pkg install python35
    cd /usr/local/bin 
    sudo rm /usr/local/bin/python 
    sudo ln -s /usr/local/bin/python3.5 /usr/local/bin/python
```

Während der Installation werden Sie gefragt: `Modify profile to update your $PATH and enable shell/tab completion now? (Y/n)`. Wenn Sie `y` als Antwort geben, und `/etc/rc.conf` als `a path to an rc file to update` eingeben, stoßen Sie vielleicht auf das Problem `ERROR: [Errno 13] Permission denied`. Um dieses Problem zu beheben, müssen Sie dem aktuellen Benutzer das Schreibrecht für die Datei `etc/rc.conf` gewähren.

Nun können Sie sich bei Azure einloggen, und Ihre FreeBSD-VM erstellen. Unten ist ein Beispiel zum Erstellen einer FreeBSD 11.0-VM. Sie können auch den Parameter `--public-ip-address-dns-name` mit einem global eindeutigen DNS-Namen für eine neu erstellte öffentliche IP-Adresse hinzufügen. 

```azurecli
    az login 
    az group create -n myResourceGroup -l westus az vm create -n myFreeBSD11 -g myResourceGroup --image MicrosoftOSTC:FreeBSD:11.0:latest --admin-username azureuser --ssh-key-value /etc/ssh/ssh_host_rsa_key.pub 
```

Anschließend können Sie sich bei Ihrer FreeBSD-VM über die IP-Adresse anmelden, die in der Ausgabe der oben stehenden Bereitstellung angezeigt wird. 

```bash
    ssh azureuser@xx.xx.xx.xx -i /etc/ssh/ssh_host_rsa_key
```   

## <a name="vm-extensions-for-freebsd"></a>VM-Erweiterung für FreeBSD
Folgende VM-Erweiterungen werden in FreeBSD unterstützt.

### <a name="vmaccess"></a>VMAccess
Die Erweiterung [VMAccess](https://github.com/Azure/azure-linux-extensions/tree/master/VMAccess) ermöglicht Folgendes:

* Zurücksetzen des Kennworts des ursprünglichen Benutzers „sudo“
* Erstellen eines neuen Benutzers „sudo“ mit angegebenem Kennwort
* Festlegen des öffentlichen Hostschlüssels mit dem angegebenen Schlüssel
* Zurücksetzen des öffentlichen Hostschlüssels, der während der VM-Bereitstellung angegeben wurde, wenn kein Hostschlüssel angegeben wird
* Öffnen des SSH-Ports (22) und Wiederherstellen von „sshd_config“, wenn „reset_ssh“ auf „true“ festgelegt ist
* Entfernen des vorhandenen Benutzers
* Überprüfen der Datenträger
* Reparieren hinzugefügter Datenträger

### <a name="customscript"></a>CustomScript
Die Erweiterung [CustomScript](https://github.com/Azure/azure-linux-extensions/tree/master/CustomScript) ermöglicht Folgendes:

* Herunterladen benutzerdefinierter Skripts aus Azure Storage oder einem externen öffentlichen Speicher (z.B. Github), sofern bereitgestellt
* Ausführen des Skripts für den Einstiegspunkt
* Unterstützen von Inlinebefehlen
* Automatisches Konvertieren von Zeilenumbrüchen im Windows-Stil in Shell- und Python-Skripts
* Automatisches Entfernen der Bytereihenfolge-Marke in Shell- und Python-Skripts
* Schützen vertraulicher Daten in „CommandToExecute“

> [!NOTE]
> FreeBSD VM unterstützt inzwischen nur die CustomScript-Version 1.x.  

## <a name="authentication-user-names-passwords-and-ssh-keys"></a>Authentifizierung: Benutzernamen, Kennwörter und SSH-Schlüssel
Wenn Sie im Azure-Portal einen virtuellen FreeBSD-Computer erstellen, müssen einen Benutzernamen, ein Kennwort oder einen öffentlichen SSH-Schlüssel angeben.
Benutzernamen für die Bereitstellung einer FreeBSD-VM in Azure dürfen nicht mit Namen von Systemkonten (UID <&100;) übereinstimmen, die bereits auf dem virtuellen Computer vorhanden sind (Beispiel: root).
Derzeit wird nur der RSA SSH-Schlüssel unterstützt. Mehrzeilige SSH-Schlüssel müssen mit `---- BEGIN SSH2 PUBLIC KEY ----` beginnen und mit `---- END SSH2 PUBLIC KEY ----` enden.

## <a name="obtaining-superuser-privileges"></a>Erlangen von Superuser-Berechtigungen
Das Benutzerkonto, das während der Bereitstellung der virtuellen Computerinstanz auf Azure angegeben wird, ist ein Konto mit weit reichenden Berechtigungen. Das sudo-Paket wurde im veröffentlichten FreeBSD-Image installiert.
Sobald Sie über dieses Benutzerkonto angemeldet sind, können Sie mit der Befehlssyntax Befehle als „root“ ausführen.

```
    $ sudo <COMMAND>
```

Optional können Sie über `sudo -s` eine Root-Shell abrufen.

## <a name="known-issues"></a>Bekannte Probleme
Der [Azure VM-Gast-Agent](https://github.com/Azure/WALinuxAgent/) mit der Version 2.2.2 weist ein [bekanntes Problem] auf (https://github.com/Azure/WALinuxAgent/pull/517), das die Bereitstellung der FreeBSD-VM in Azure verhindert. Die Korrektur wurde mit dem [Gast-Agent für virtuelle Azure-Computer](https://github.com/Azure/WALinuxAgent/) ab Version 2.2.3 zur Verfügung gestellt. 

## <a name="next-steps"></a>Nächste Schritte
* Wechseln Sie zum [Azure Marketplace](https://azure.microsoft.com/marketplace/partners/microsoft/freebsd110/) , um eine FreeBSD-VM zu erstellen.
* Wenn Sie Ihre eigene FreeBSD-VHD in Azure aufnehmen möchten, finden Sie relevante Informationen unter [Erstellen und Hochladen einer FreeBSD-VHD in Azure](./virtual-machines-linux-classic-freebsd-create-upload-vhd.md).

