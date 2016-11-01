<properties
   pageTitle="Einführung in FreeBSD in Azure | Microsoft Azure"
   description="Erfahren Sie, wie Sie virtuelle FreeBSD-Computer auf Azure verwenden"
   services="virtual-machines-linux"
   documentationCenter=""
   authors="KylieLiang"
   manager="timlt"
   editor=""
   tags="azure-service-management"/>

<tags
   ms.service="virtual-machines-linux"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-linux"
   ms.workload="infrastructure-services"
   ms.date="08/27/2016"
   ms.author="kyliel"/>

# Einführung in FreeBSD in Azure
Dieses Thema enthält einen Überblick über die Ausführung eines virtuellen FreeBSD-Computers in Azure.

## Übersicht
FreeBSD für Microsoft Azure ist ein erweitertes Computerbetriebssystem, das für moderne Server, Desktops und eingebettete Plattformen verwendet wird. Das Image „FreeBSD 10.3“ wird von der Microsoft Corporation bereitgestellt und ist in Azure verfügbar. Es basiert auf der FreeBSD 10.3-Version. Der Azure VM-Gast-Agent [2\.1.4](https://github.com/Azure/WALinuxAgent/releases/tag/v2.1.4) ist installiert. Der Agent ist verantwortlich für die Kommunikation zwischen der FreeBSD-VM und Azure Fabric für Vorgänge wie z.B. das Bereitstellen der VM für die erste Verwendung (Benutzername, Kennwort, Hostname usw.) und das Aktivieren von Funktionalität für selektive VM-Erweiterungen. Die Strategie für künftige Versionen von FreeBSD ist, stets aktuell zu bleiben und die neuesten Versionen kurz nach ihrer Freigabe durch das FreeBSD Release Engineering-Team verfügbar zu machen. Die nächste Version ist [FreeBSD 11](https://www.freebsd.org/releases/11.0R/schedule.html).

## Bereitstellen eines virtuellen FreeBSD-Computers
Die Bereitstellung eines virtuellen FreeBSD-Computers ist ein unkomplizierter Prozess, wenn Sie ein Image aus dem [Azure Marketplace](https://azure.microsoft.com/marketplace/partners/microsoft/freebsd103/) verwenden.

## VM-Erweiterung für FreeBSD
Folgende VM-Erweiterungen werden in FreeBSD unterstützt.

### VMAccess

Die Erweiterung [VMAccess](https://github.com/Azure/azure-linux-extensions/tree/master/VMAccess) ermöglicht Folgendes:

- Zurücksetzen des Kennworts des ursprünglichen Benutzers „sudo“
- Erstellen eines neuen Benutzers „sudo“ mit angegebenem Kennwort
- Festlegen des öffentlichen Hostschlüssels mit dem angegebenen Schlüssel
- Zurücksetzen des öffentlichen Hostschlüssels, der während der VM-Bereitstellung angegeben wurde, wenn kein Hostschlüssel angegeben wird
- Öffnen des SSH-Ports (22) und Wiederherstellen von „sshd\_config“, wenn „reset\_ssh“ auf „true“ festgelegt ist
- Entfernen des vorhandenen Benutzers
- Überprüfen der Datenträger
- Reparieren hinzugefügter Datenträger

### CustomScript

Die Erweiterung [CustomScript](https://github.com/Azure/azure-linux-extensions/tree/master/CustomScript) ermöglicht Folgendes:

- Herunterladen benutzerdefinierter Skripts aus Azure Storage oder einem externen öffentlichen Speicher (z.B. Github), sofern bereitgestellt
- Ausführen des Skripts für den Einstiegspunkt
- Unterstützen von Inlinebefehlen
- Automatisches Konvertieren von Zeilenumbrüchen im Windows-Stil in Shell- und Python-Skripts
- Automatisches Entfernen der Bytereihenfolge-Marke in Shell- und Python-Skripts
- Schützen vertraulicher Daten in „CommandToExecute“

## Authentifizierung: Benutzernamen, Kennwörter und SSH-Schlüssel
Wenn Sie im Azure-Portal einen virtuellen FreeBSD-Computer erstellen, müssen einen Benutzernamen, ein Kennwort oder einen öffentlichen SSH-Schlüssel angeben. Benutzernamen für die Bereitstellung einer FreeBSD-VM in Azure dürfen nicht mit Namen von Systemkonten (UID < 100) übereinstimmen, die bereits auf dem virtuellen Computer vorhanden sind (Beispiel: root). Derzeit wird nur der RSA SSH-Schlüssel unterstützt. Mehrzeilige SSH-Schlüssel müssen mit ---- BEGIN SSH2 PUBLIC KEY ---- beginnen und mit ---- END SSH2 PUBLIC KEY ---- enden.

## Erlangen von Superuser-Berechtigungen
Das Benutzerkonto, das während der Bereitstellung der virtuellen Computerinstanz auf Azure angegeben wird, ist ein Konto mit weit reichenden Berechtigungen. Das sudo-Paket wurde im veröffentlichten FreeBSD-Image installiert. Sobald Sie über dieses Benutzerkonto angemeldet sind, können Sie mit der Befehlssyntax Befehle als „root“ ausführen.

    # sudo <COMMAND>

Optional können Sie über „sudo -s“ eine Root-Shell abrufen.

## Nächste Schritte
- Wechseln Sie zum [Azure Marketplace](https://azure.microsoft.com/marketplace/partners/microsoft/freebsd103/), um eine FreeBSD-VM zu erstellen.
- Wenn Sie Ihre eigene FreeBSD-VHD in Azure aufnehmen möchten, finden Sie relevante Informationen unter [Erstellen und Hochladen einer FreeBSD-VHD in Azure](../virtual-machines-linux-classic-freebsd-create-upload-vhd.md).

<!---HONumber=AcomDC_0914_2016-->