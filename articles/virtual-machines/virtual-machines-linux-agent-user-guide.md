---
title: "Azure-Linux-VM-Agent – Übersicht | Microsoft-Dokumentation"
description: Erfahren Sie, wie Sie den Linux-Agent (waagent) zum Verwalten der Interaktion Ihres virtuellen Computers mit Azure Fabric Controller installieren und konfigurieren.
services: virtual-machines-linux
documentationcenter: 
author: szarkos
manager: timlt
editor: 
tags: azure-service-management,azure-resource-manager
ms.assetid: e41de979-6d56-40b0-8916-895bf215ded6
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 10/17/2016
ms.author: szark
ms.custom: H1Hack27Feb2017
translationtype: Human Translation
ms.sourcegitcommit: 07ee32a293d53dd9c0e0afa5da2bac7ec146a536
ms.openlocfilehash: c7f39a5396f0f9b6f9a577a3225ccadff1fd0dc9
ms.lasthandoff: 03/01/2017

---
# <a name="understanding-and-using-the-azure-linux-agent"></a>Grundlegendes zum Azure Linux-Agent und seiner Verwendung
[!INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]

## <a name="introduction"></a>Einführung
Der Microsoft Azure Linux-Agent (waagent) verwaltet die Bereitstellung von Linux und FreeBSD sowie die Interaktion virtueller Computer mit dem Azure Fabric Controller. Er stellt die folgenden Funktionen für Linux- und FreeBSD-basierte IaaS-Bereitstellungen zur Verfügung:

> [!NOTE]
> Ausführlichere Informationen finden Sie in der [INFODATEI](https://github.com/Azure/WALinuxAgent/blob/master/README.md) des Azure Linux-Agents.
> 
> 

* **Image-Bereitstellung**
  
  * Erstellen eines Benutzerkontos
  * Konfigurieren von SSH-Authentifizierungstypen
  * Bereitstellen von öffentlichen SSH-Schlüsseln und -Schlüsselpaaren
  * Festlegen des Hostnamens
  * Veröffentlichen des Hostnamens beim Plattform-DNS
  * Mitteilen des SSH-Hostschlüssel-Fingerabdrucks an die Plattform
  * Verwalten von Ressourcendatenträgern
  * Formatieren und Bereitstellen des Ressourcendatenträgers
  * Konfigurieren von Auslagerungsbereich
* **Netzwerk**
  
  * Verwaltet von Routen zur Verbesserung der Kompatibilität mit Plattform-DHCP-Servern
  * Stellt die Stabilität des Netzwerkschnittstellennamens sicher
* **Kernel**
  
  * Konfiguriert einen virtuellen NUMA (muss für Kernel vor Version&2;.6.37 deaktiviert werden)
  * Verbraucht Hyper-V-Entropie für /dev/random
  * Konfiguriert SCSI-Zeitüberschreitungen für das Stammgerät (das auch remote sein kann)
* **Diagnose**
  
  * Konsolenumleitung an den seriellen Port
* **SCVMM-Bereitstellungen**
  
  * Erkennt und startet den VMM-Agenten für Linux bei Ausführung in einer Umgebung mit System Center Virtual Machine Manager 2012 R2
* **VM-Erweiterung**
  
  * Fügt von Microsoft und Partnern erstellte Komponenten in einen virtuellen Linux-Computer ein, um die Automatisierung der Software und der Konfiguration zu ermöglichen
  * Referenzimplementierung einer VM-Erweiterung unter [https://github.com/Azure/azure-linux-extensions](https://github.com/Azure/azure-linux-extensions)

## <a name="communication"></a>Kommunikation
Der Informationsfluss von der Plattform zum Agenten erfolgt über zwei Kanäle:

* Über eine beim Start angeschlossene DVD für IaaS-Bereitstellungen. Diese DVD enthält eine OVF-kompatible Konfigurationsdatei mit allen Bereitstellungsinformationen außer den tatsächlichen SSH-Schlüsselpaaren.
* Über einen TCP-Endpunkt, der eine REST-API verfügbar macht, um die Bereitstellungs- und Topologiekonfiguration abzurufen.

## <a name="requirements"></a>Anforderungen
Folgende Systeme können nachweislich mit dem Azure Linux-Agent verwendet werden:

> [!NOTE]
> Diese Liste kann von der offiziellen Liste unterstützter Systemen für die Microsoft Azure-Plattform abweichen. Weitere Informationen finden Sie unter [http://support.microsoft.com/kb/2805216](http://support.microsoft.com/kb/2805216).
> 
> 

* CoreOS
* CentOS 6.3+
* Red Hat Enterprise Linux 6.7+
* Debian 7.0+
* Ubuntu 12.04+
* openSUSE 12.3+
* SLES 11 SP3+
* Oracle Linux 6.4+

Andere unterstützte Systeme:

* FreeBSD 10+ (Azure Linux-Agent v2.0.10+)

Der Linux-Agent erfordert zur ordnungsgemäßen Funktion bestimmte Systempakete:

* Python 2.6+
* OpenSSL 1.0+
* OpenSSH 5.3+
* Dienstprogramme für das Dateisystem: sfdisk, fdisk, mkfs, parted
* Kennworttools: chpasswd, sudo
* Textverarbeitungstools: sed, grep
* Netzwerktools: ip-route
* Kernel-Unterstützung für die Einbindung von UDF-Dateisystemen.

## <a name="installation"></a>Installation
Für die Installation und Upgrades des Azure Linux-Agents sollte nach Möglichkeit ein RPM- oder DEB-Paket aus dem Paketrepository Ihrer Verteilung installiert werden. Das Azure Linux-Agent-Paket wird von allen [unterstützten Distributionsanbietern](virtual-machines-linux-endorsed-distros.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) in ihre jeweiligen Images und Repositorys integriert.

Informationen zu erweiterten Installationsoptionen (beispielsweise zu einer quellbasierten Installation, zum Installieren an benutzerdefinierten Orten oder zur Verwendung von Präfixen) finden Sie in der Dokumentation im [Azure Linux-Agent-Repository auf Github](https://github.com/Azure/WALinuxAgent).

## <a name="command-line-options"></a>Befehlszeilenoptionen
### <a name="flags"></a>Kennzeichen
* verbose: Ausführlichkeit des angegebenen Befehls erhöhen
* force: Interaktive Bestätigung für einige Befehle überspringen

### <a name="commands"></a>Befehle
* help: Listet die unterstützten Befehle und Kennzeichen auf
* deprovision: Versucht, das System zu bereinigen und für eine erneute Bereitstellung vorzubereiten. Bei diesem Vorgang wird Folgendes gelöscht:
  
  * Alle SSH-Hostschlüssel (sofern Provisioning.RegenerateSshHostKeyPair in der Konfigurationsdatei auf "y" festgelegt ist)
  * Namenserverkonfiguration in "/etc/resolv.conf"
  * Stammkennwort aus "/etc/shadow" (sofern Provisioning.DeleteRootPassword in der Konfigurationsdatei auf "y" festgelegt ist)
  * Zwischengespeicherte DHCP-Clientleases
  * Setzt den Hostnamen auf "localhost.localdomain" zurück

> [!WARNING]
> Die Aufhebung der Bereitstellung garantiert nicht, dass alle vertraulichen Informationen aus dem Image gelöscht werden und das Image für eine weitere Verteilung geeignet ist.
> 
> 

* deprovision+user: Führt alle vorstehend für "-deprovision" aufgeführten Aktionen aus und löscht darüber hinaus das zuletzt bereitgestellte Benutzerkonto (aus "/var/lib/waagent" abgerufen) und zugehörige Daten. Dieser Parameter sollte verwendet werden, um die Bereitstellung eines Image aufzuheben, das zuvor für Bereitstellungen auf Azure verwendet wurde, sodass es erfasst und erneut verwendet werden kann.
* version: Zeigt die Version von waagent an
* serialconsole: Konfiguriert GRUB so, dass ttyS0 (der erste serielle Port) als Startkonsole markiert wird. Hierdurch wird sichergestellt, dass Kernelstartprotokolle an den seriellen Port gesendet und für das Debuggen verfügbar gemacht werden.
* daemon: Führt waagent als Daemon aus, um die Interaktion mit der Plattform zu verwalten. Dieses Argument wird waagent im waagent-Initialisierungsskript mitgeteilt.
* start: Führt waagent als Hintergrundprozess aus.

## <a name="configuration"></a>Konfiguration
Eine Konfigurationsdatei (/etc/waagent.conf) steuert die Aktionen von waagent. Nachfolgend sehen Sie ein Beispiel einer Konfigurationsdatei:

    Provisioning.Enabled=y
    Provisioning.DeleteRootPassword=n
    Provisioning.RegenerateSshHostKeyPair=y
    Provisioning.SshHostKeyPairType=rsa
    Provisioning.MonitorHostName=y
    Provisioning.DecodeCustomData=n
    Provisioning.ExecuteCustomData=n
    Provisioning.PasswordCryptId=6
    Provisioning.PasswordCryptSaltLength=10
    ResourceDisk.Format=y
    ResourceDisk.Filesystem=ext4
    ResourceDisk.MountPoint=/mnt/resource
    ResourceDisk.MountOptions=None
    ResourceDisk.EnableSwap=n
    ResourceDisk.SwapSizeMB=0
    LBProbeResponder=y
    Logs.Verbose=n
    OS.RootDeviceScsiTimeout=300
    OS.OpensslPath=None
    HttpProxy.Host=None
    HttpProxy.Port=None

Die verschiedenen Konfigurationsoptionen werden nachfolgend ausführlich erläutert. Konfigurationsoptionen weisen einen von drei Datentypen auf: Boolean, String oder Integer. Die Konfigurationsoptionen vom Typ "Boolesch" können als "j" oder "n" angegeben werden. Für einige Konfigurationseinträge des Typs "Zeichenfolge" kann das spezielle Schlüsselwort "Keine" verwendet werden, wie nachfolgend beschrieben.

**Provisioning.Enabled:**  
Typ: Boolescher Wert  
Voreinstellung: j

Hiermit kann der Benutzer die Bereitstellungsfunktion im Agenten aktivieren oder deaktivieren. Gültige Werte sind "y" und "n". Bei deaktivierter Bereitstellung werden die SSH-Host- und -Benutzerschlüssel im Image beibehalten, und alle in der Azure-Bereitstellungs-API angegebenen Konfigurationen werden ignoriert.

> [!NOTE]
> Der Parameter `Provisioning.Enabled` ist für Ubuntu-Cloud-Images, die „cloud-init“ für die Bereitstellung verwenden, standardmäßig auf „n“ festgelegt.
> 
> 

**Provisioning.DeleteRootPassword:**  
Typ: Boolescher Wert  
Voreinstellung: n

Bei aktivierter Option wird das Stammkennwort in der Datei "/etc/shadow" bei der Bereitstellung gelöscht.

**Provisioning.RegenerateSshHostKeyPair:**  
Typ: Boolescher Wert  
Voreinstellung: j

Bei aktivierter Option werden alle SSH-Hostschlüsselpaare (ecdsa, dsa und rsa) bei der Bereitstellung aus "/etc/ssh/" gelöscht. Außerdem wird ein einzelnes neues Schlüsselpaar generiert.

Der Verschlüsselungstyp für das neue Schlüsselpaar kann mit dem Provisioning.SshHostKeyPairType-Eintrag konfiguriert werden. Beachten Sie, dass einige Distributionen SSH-Schlüsselpaare für alle fehlenden Verschlüsselungstypen neu erstellen, wenn der SSH-Daemon neu gestartet wird (z. B. bei einem Systemneustart).

**Provisioning.SshHostKeyPairType:**  
Typ: Zeichenfolge  
Voreinstellung: rsa

Diese Option kann auf einen Verschlüsselungsalgorithmustyp festgelegt werden, der vom SSH-Daemon auf dem virtuellen Computer unterstützt wird. Typischerweise werden die Werte "rsa", "dsa" und "ecdsa" unterstützt. Beachten Sie, dass "putty.exe" unter Windows keine Unterstützung für "ecdsa" bietet. Wenn Sie also beabsichtigen, mithilfe von "putty.exe" unter Windows eine Verbindung zu einer Linux-Bereitstellung herzustellen, verwenden Sie "rsa" oder "dsa".

**Provisioning.MonitorHostName:**  
Typ: Boolescher Wert  
Voreinstellung: j

Bei aktivierter Option überwacht waagent den virtuellen Linux-Computer auf Änderungen des Hostnamens (wie vom Befehl "hostname" zurückgegeben) und aktualisiert die Netzwerkkonfiguration im Image automatisch entsprechend der Änderung. Um die Namensänderung per Push an die DNS-Server zu übertragen, wird die Netzwerkfunktion auf dem virtuellen Computer neu gestartet. Dadurch geht die Internetverbindung verloren.

**Provisioning.DecodeCustomData**  
Typ: Boolescher Wert  
Voreinstellung: n

Ist diese Option festgelegt, decodiert waagent benutzerdefinierte Daten aus Base64.

**Provisioning.ExecuteCustomData**  
Typ: Boolescher Wert  
Voreinstellung: n

Ist diese Option festgelegt, führt waagent benutzerdefinierte Daten nach der Bereitstellung aus.

**Provisioning.PasswordCryptId**  
Typ: Zeichenfolge  
Standardwert:&6;

Algorithmus, der von Crypt zum Generieren des Kennworthashs verwendet wird.  
 1: MD5  
 2: Blowfish  
 5: SHA-256  
 6: SHA-512  

**Provisioning.PasswordCryptSaltLength**  
Typ: Zeichenfolge  
Standardeinstellung:&10;

Länge des Zufallssalts, das beim Generieren des Kennworthashs verwendet wird.

**ResourceDisk.Format:**  
Typ: Boolescher Wert  
Voreinstellung: j

Bei aktivierter Option wird der von der Plattform bereitgestellte Ressourcendatenträger von waagent formatiert und bereitgestellt, falls der Benutzer in "ResourceDisk.Filesystem" einen anderen Dateisystemtyp als "ntfs" anfordert. Auf dem Datenträger wird eine einzelne Linux-Partition (83) verfügbar gemacht. Diese Partition wird nicht formatiert, falls sie erfolgreich bereitgestellt werden kann.

**ResourceDisk.Filesystem:**  
Typ: Zeichenfolge  
Voreinstellung: ext4

Diese Option gibt den Dateisystemtyp für den Ressourcendatenträger an. Die unterstützten Werte sind je nach Linux-Distribution verschieden. Bei der Zeichenfolge X sollte mkfs.X im Linux-Image vorhanden sein. Für SLES 11-Images sollte normalerweise "ext3" verwendet werden. Für FreeBSD-Images sollte hier "ufs2" verwendet werden.

**ResourceDisk.MountPoint:**  
Typ: Zeichenfolge  
Standardwert: /mnt/resource 

Diese Option gibt den Pfad an, in dem der Ressourcendatenträger bereitgestellt wird. Beachten Sie, dass der Ressourcendatenträger ein *temporärer* Datenträger ist und geleert werden kann, wenn die Bereitstellung des virtuellen Computers aufgehoben wird.

**ResourceDisk.MountOptions**  
Typ: Zeichenfolge  
Standardwert: keiner

Gibt Optionen für die Datenträgereinbindung an, die an den Befehl „mount -o“ übergeben werden. Hierbei handelt es sich um eine kommagetrennte Liste. Beispiel: 'nodev,nosuid'. Weitere Informationen finden Sie unter „mount(8)“.

**ResourceDisk.EnableSwap:**  
Typ: Boolescher Wert  
Voreinstellung: n

Bei aktivierter Option wird eine Auslagerungsdatei (/swapfile) auf dem Ressourcendatenträger erstellt und dem Systemauslagerungsbereich hinzugefügt.

**ResourceDisk.SwapSizeMB:**  
Typ: Ganze Zahl  
Standardwert: 0

Die Größe der Auslagerungsdatei in Megabyte.

**Logs.Verbose:**  
Typ: Boolescher Wert  
Voreinstellung: n

Bei aktivierter Option wird ein ausführlicheres Protokoll erstellt. Waagent schreibt das Protokoll in "/var/log/waagent.log" und nutzt die logrotate-Funktion des Systems zu Rotieren von Protokollen.

**OS.EnableRDMA**  
Typ: Boolescher Wert  
Voreinstellung: n

Ist diese Option festgelegt, versucht der Agent, einen passenden RDMA-Kernel-Treiber für die Firmwareversion der zugrunde liegenden Hardware zu installieren und anschließend zu laden.

**OS.RootDeviceScsiTimeout:**  
Typ: Ganze Zahl  
Standardwert: 300

Hiermit wird die SCSI-Zeitüberschreitung auf dem Betriebssystemdatenträger und Datenlaufwerken in Sekunden konfiguriert. Wird kein Wert festgelegt, gelten die Systemstandardwerte.

**OS.OpensslPath:**  
Typ: Zeichenfolge  
Standardwert: keiner

Hiermit kann ein alternativer Pfad für die openssl-Binärdatei zur Verwendung für kryptografische Vorgänge angegeben werden.

**HttpProxy.Host, HttpProxy.Port**  
Typ: Zeichenfolge  
Standardwert: keiner

Ist diese Option festgelegt, greift der Agent über den angegebenen Proxyserver auf das Internet zu. 

## <a name="ubuntu-cloud-images"></a>Ubuntu-Cloud-Images
Beachten Sie, dass Cloud Ubuntu-Images für zahlreiche Konfigurationsaufgaben, die andernfalls vom Azure Linux-Agent verwaltet würden, [cloud-init](https://launchpad.net/ubuntu/+source/cloud-init) verwenden.  Beachten Sie die folgenden Unterschiede:

* **Provisioning.Enabled** ist für Ubuntu-Cloud-Images, die für Bereitstellungsaufgaben Cloud-Init verwenden, standardmäßig auf "n" gesetzt.
* Die folgenden Konfigurationsparameter wirken sich nicht auf Ubuntu-Cloud-Images aus, die Ressourcendatenträger und Auslagerungsbereiche mit Cloud-Init verwalten:
  
  * **ResourceDisk.Format**
  * **ResourceDisk.Filesystem**
  * **ResourceDisk.MountPoint**
  * **ResourceDisk.EnableSwap**
  * **ResourceDisk.SwapSizeMB**
* Berücksichtigen Sie zum Konfigurieren des Bereitstellungspunkts und des Auslagerungsbereichs für den Ressourcendatenträger von Ubuntu-Cloud-Images die folgenden Ressourcen:
  
  * [Ubuntu-Wiki: Konfigurieren von Swap-Partitionen](http://go.microsoft.com/fwlink/?LinkID=532955&clcid=0x409)
  * [Einfügen benutzerdefinierter Daten in einen virtuellen Azure-Computer](virtual-machines-windows-classic-inject-custom-data.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json)


