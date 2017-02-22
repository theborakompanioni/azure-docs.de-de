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
ms.date: 01/09/2017
ms.author: kyliel
translationtype: Human Translation
ms.sourcegitcommit: 71ad04b10bc49500197db6fecdcc0305a1ea0dd2
ms.openlocfilehash: ecb1c385de6c1b12674326afe7d5a0ebf6cd9ad0


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
Die Bereitstellung eines virtuellen FreeBSD-Computers ist ein unkomplizierter Prozess, wenn Sie ein Image aus dem Azure Marketplace verwenden:

- [FreeBSD 10.3 auf Azure Marketplace](https://azure.microsoft.com/marketplace/partners/microsoft/freebsd103/)
- [FreeBSD 11.0 auf Azure Marketplace](https://azure.microsoft.com/marketplace/partners/microsoft/freebsd110/)

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

[!NOTE]FreeBSD VM unterstützt inzwischen nur die CustomScript-Version 1.x.  

## <a name="authentication-user-names-passwords-and-ssh-keys"></a>Authentifizierung: Benutzernamen, Kennwörter und SSH-Schlüssel
Wenn Sie im Azure-Portal einen virtuellen FreeBSD-Computer erstellen, müssen einen Benutzernamen, ein Kennwort oder einen öffentlichen SSH-Schlüssel angeben.
Benutzernamen für die Bereitstellung einer FreeBSD-VM in Azure dürfen nicht mit Namen von Systemkonten (UID <&100;) übereinstimmen, die bereits auf dem virtuellen Computer vorhanden sind (Beispiel: root).
Derzeit wird nur der RSA SSH-Schlüssel unterstützt. Mehrzeilige SSH-Schlüssel müssen mit `---- BEGIN SSH2 PUBLIC KEY ----` beginnen und mit `---- END SSH2 PUBLIC KEY ----` enden.

## <a name="obtaining-superuser-privileges"></a>Erlangen von Superuser-Berechtigungen
Das Benutzerkonto, das während der Bereitstellung der virtuellen Computerinstanz auf Azure angegeben wird, ist ein Konto mit weit reichenden Berechtigungen. Das sudo-Paket wurde im veröffentlichten FreeBSD-Image installiert.
Sobald Sie über dieses Benutzerkonto angemeldet sind, können Sie mit der Befehlssyntax Befehle als „root“ ausführen.

    $ sudo <COMMAND>

Optional können Sie über `sudo -s` eine Root-Shell abrufen.

## <a name="known-issues"></a>Bekannte Probleme
1. Es gibt derzeit ein ungelöstes Problem mit FreeBSD 11.0 unter Hyper-V (und Azure), bei dem beim Starten von VMs ein Fehler auftreten kann, wenn das Betriebssystem mit `freebsd-update` gepatcht wurde. Das [vorgeschlagenen Patch](https://bugs.freebsd.org/bugzilla/show_bug.cgi?id=212721) ist in FreeBSD-Images auf Azure Marketplace enthalten. Es wurde allerdings vom FreeBSD-Team nicht aufwärts zusammengeführt, weshalb bei Ausführung von `freebsd-update` der Kernel durch einen ohne Patches ersetzt wird. Benutzern in Azure wird daher empfohlen, für FreeBSD 11.0 erst dann Patches zu installieren, nachdem der Fix als ERRATA veröffentlicht wurde.

2. Der [Azure VM-Gast-Agent](https://github.com/Azure/WALinuxAgent/) mit der Version 2.2.2 weist ein [bekanntes Problem] auf (https://github.com/Azure/WALinuxAgent/pull/517), das die Bereitstellung der FreeBSD-VM in Azure verhindert. Es wird empfohlen, dass Benutzer der FreeBSD-VM in Azure die Version 2.2.1 oder niedriger verwenden. Der [Azure VM-Gast-Agent](https://github.com/Azure/WALinuxAgent/) mit der Version 2.2.3 wird der Fix zur Verfügung gestellt. 

## <a name="next-steps"></a>Nächste Schritte
* Wechseln Sie zum [Azure Marketplace](https://azure.microsoft.com/marketplace/partners/microsoft/freebsd110/) , um eine FreeBSD-VM zu erstellen.
* Wenn Sie Ihre eigene FreeBSD-VHD in Azure aufnehmen möchten, finden Sie relevante Informationen unter [Erstellen und Hochladen einer FreeBSD-VHD in Azure](./virtual-machines-linux-classic-freebsd-create-upload-vhd.md).



<!--HONumber=Jan17_HO2-->


