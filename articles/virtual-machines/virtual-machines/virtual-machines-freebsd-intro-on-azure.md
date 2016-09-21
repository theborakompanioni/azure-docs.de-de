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

#Einführung in FreeBSD in Azure
Dieses Thema enthält einen Überblick über die Ausführung eines virtuellen FreeBSD-Computers in Azure.

##Übersicht
FreeBSD für Microsoft Azure wird von der Microsoft Corporation zur Verfügung gestellt. FreeBSD ist ein erweitertes Computerbetriebssystem, das für moderne Server, Desktops und eingebettete Plattformen verwendet wird. Ab jetzt ist das Image FreeBSD 10.3 in Azure verfügbar. Das Image basiert auf der Version FreeBSD 10.3. Und der Azure-VM-Gast-Agent der Version [2\.1.4](https://github.com/Azure/WALinuxAgent/releases/tag/v2.1.4) wird installiert. Der Agent ist verantwortlich für die Kommunikation zwischen der FreeBSD-VM und Azure Fabric für Vorgänge wie z.B. das Bereitstellen der VM für die erste Verwendung (Benutzername, Kennwort, Hostname usw.) und das Aktivieren der Funktion für selektive VM-Erweiterungen. Die Strategie für zukünftige Versionen von FreeBSD ist, stets aktuell zu bleiben und die neuesten Versionen kurz nach ihrer Freigabe durch das FreeBSD Release Engineering-Team verfügbar zu machen. Die nächste Version ist [FreeBSD 11](https://www.freebsd.org/releases/11.0R/schedule.html).

##Bereitstellen eines virtuellen FreeBSD-Computers
Die Bereitstellung eines virtuellen FreeBSD-Computers ist ein unkomplizierter Prozess, wenn Sie ein Image aus dem [Azure Marketplace](https://azure.microsoft.com/marketplace/partners/microsoft/freebsd103/) verwenden.

##VM-Erweiterung für FreeBSD
Folgende VM-Erweiterungen werden in FreeBSD-VMs unterstützt.

• [VMAccess](https://github.com/Azure/azure-linux-extensions/tree/master/VMAccess)

Funktionen der VMAccess-Erweiterung:

- Zurücksetzen des Kennworts des ursprünglichen sudo-Benutzers
- Erstellen eines neuen sudo-Benutzers mit dem angegebenen Kennwort
- Festlegen des öffentlichen Hostschlüssels mit dem angegebenen Schlüssel
- Zurücksetzen des öffentlichen Hostschlüssels, der während VM-Bereitstellung angegeben wurde, wenn kein Hostschlüssel angegeben wird
- Öffnen des SSH-Ports (22) und Wiederherstellen von „sshd\_config“, wenn „reset\_ssh“ auf „true“ festgelegt ist
- Entfernen des vorhandenen Benutzers
- Überprüfen der Datenträger
- Reparieren hinzugefügter Datenträger

• [CustomSript](https://github.com/Azure/azure-linux-extensions/tree/master/CustomScript)

Funktionen der CustomScript-Erweiterung:

- Herunterladen benutzerdefinierter Skripts aus Azure Storage oder einem externen öffentlichen Speicher (z.B. Github), wenn angegeben
- Ausführen des Skripts für den Einstiegspunkt
- Unterstützen von Inlinebefehlen
- Automatisches Konvertieren von Zeilenumbrüchen im Windows-Stil in Shell- und Python-Skripts
- Automatisches Entfernen der Bytereihenfolge-Marke in Shell- und Python-Skripts
- Schützen vertraulicher Daten in commandToExecute

##Authentifizierung: Benutzernamen, Kennwörter und SSH-Schlüssel
Wenn Sie mithilfe des klassischen Azure-Portals einen virtuellen FreeBSD-Computer erstellen, werden Sie aufgefordert, einen Benutzernamen, ein Kennwort oder einen öffentlichen SSH-Schlüssel anzugeben. Bei der Auswahl eines Benutzernamens während der Bereitstellung virtueller FreeBSD-Computer gibt es folgende Einschränkungen: Bereits im virtuellen Computer vorhandene Namen von Systemkonten (UID <100) sind nicht zulässig, z.B. „root“. Bisher wird nur der RSA-SSH-Schlüssel unterstützt. Mehrzeilige SSH-Schlüssel müssen mit ---- BEGIN SSH2 PUBLIC KEY ---- beginnen und mit ---- END SSH2 PUBLIC KEY ---- enden.

##Abrufen von Superuser-Berechtigungen
Das Benutzerkonto, das während der Bereitstellung der virtuellen Computerinstanz auf Azure angegeben wird, ist ein Konto mit weit reichenden Berechtigungen. Das sudo-Paket wurde im veröffentlichten FreeBSD-Image installiert. Sobald Sie über dieses Benutzerkonto angemeldet sind, können Sie mit der Befehlssyntax Befehle als „root“ ausführen.
  
    # sudo <COMMAND>

Optional können Sie über sudo -s eine Root-Shell abrufen.

##Nächste Schritte
- Wechseln Sie zum [Azure Marketplace](https://azure.microsoft.com/marketplace/partners/microsoft/freebsd103/), um eine FreeBSD-VM zu erstellen.
- Wenn Sie Ihre eigene FreeBSD-VHD in Azure aufnehmen möchten, finden Sie relevante Informationen unter [Erstellen und Hochladen einer FreeBSD-VHD in Azure](../virtual-machines-linux-classic-freebsd-create-upload-vhd.md).

<!---HONumber=AcomDC_0907_2016-->