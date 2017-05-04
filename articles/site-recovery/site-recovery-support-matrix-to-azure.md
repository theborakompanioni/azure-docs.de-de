---
title: Azure Site Recovery-Supportmatrix zum Replizieren in Azure | Microsoft-Dokumentation
description: "Enthält eine Zusammenfassung der unterstützten Betriebssysteme und Komponenten für Azure Site Recovery"
services: site-recovery
documentationcenter: 
author: Rajani-Janaki-Ram
manager: rochakm
editor: 
ms.assetid: 
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 01/25/2017
ms.author: rajanaki
translationtype: Human Translation
ms.sourcegitcommit: 0d6f6fb24f1f01d703104f925dcd03ee1ff46062
ms.openlocfilehash: 711fb0715b7f12e12a742136f75af8069cbc83d8
ms.lasthandoff: 04/17/2017


---
# <a name="azure-site-recovery-support-matrix-for-replicating-to-azure"></a>Azure Site Recovery-Supportmatrix zum Replizieren in Azure

> [!div class="op_single_selector"]
> * [Replizieren in Azure](site-recovery-support-matrix-to-azure.md)
> * [Replizieren am sekundären Standort im Besitz des Kunden](site-recovery-support-matrix-to-sec-site.md)


In diesem Artikel werden die unterstützten Konfigurationen und Komponenten für Azure Site Recovery bei der Replikation und Wiederherstellung in Azure beschrieben. Weitere Informationen zu den Anforderungen für Azure Site Recovery finden Sie in den [Voraussetzungen](site-recovery-prereq.md).


## <a name="support-for-deployment-options"></a>Unterstützung für Bereitstellungsoptionen

**Bereitstellung** | **VMware-/physische Server** | **Hyper-V (mit/ohne Virtual Machine Manager)** |
--- | --- | ---
**Azure-Portal** | Lokale VMware-VMs in Azure Storage, mit Azure Resource Manager oder klassischem Speicher und Netzwerken.<br/><br/> Failover auf Resource Manager-basierte oder klassische VMs. | Lokale virtuelle Hyper-V-Computer zu Azure Storage, mit Resource Manager oder klassischem Speicher und Netzwerken.<br/><br/> Failover auf Resource Manager-basierte oder klassische VMs.
**Klassisches Portal** | Nur im Wartungsmodus. Neue Tresore können nicht erstellt werden. | Nur im Wartungsmodus.
**PowerShell** | Derzeit nicht unterstützt. | Unterstützt


## <a name="support-for-datacenter-management-servers"></a>Unterstützung für Datencenter-Verwaltungsserver

### <a name="virtualization-management-entities"></a>Virtualisierungsverwaltungsentitäten

**Bereitstellung** | **Unterstützung**
--- | ---
**VMware-VM/physische Server** | vSphere 6.0, 5.5 oder 5.1 mit aktuellem Update
**Hyper-V (mit Virtual Machine Manager)** | System Center Virtual Machine Manager 2016 und System Center Virtual Machine Manager 2012 R2

  >[!Note]
  > Eine System Center Virtual Machine Manager 2016-Cloud mit einer Mischung aus Windows Server 2016- und Windows Server 2012 R2-Hosts wird derzeit nicht unterstützt.

### <a name="host-servers"></a>Hostserver

**Bereitstellung** | **Unterstützung**
--- | ---
**VMware-VM/physische Server** | vCenter 5.5 oder 6.0 (Unterstützung nur für 5.5-Features) 
**Hyper-V (mit/ohne Virtual Machine Manager)** | Windows Server 2016, Windows Server 2012 R2 mit den neuesten Updates.<br></br>Wenn SCVMM verwendet wird, müssen Windows Server 2016-Hosts mit SCVMM 2016 verwaltet werden.


  >[!Note]
  >Hyper-V-Sites mit einer Mischung aus Windows Server 2016- und Windows Server 2012 R2-Hosts werden derzeit nicht unterstützt. Wiederherstellung an einem alternativen Speicherort für virtuelle Computer auf einem Windows Server 2016-Host wird derzeit nicht unterstützt.

## <a name="support-for-replicated-machine-os-versions"></a>Unterstützung für replizierte Computer-Betriebssystemversionen

Geschützte virtuelle Computer müssen für das Replizieren in Azure die [Azure-Anforderungen](#failed-over-azure-vm-requirements) erfüllen.
Die folgende Tabelle fasst die Unterstützung der replizierten Betriebssysteme in verschiedenen Bereitstellungsszenarien bei der Verwendung von Azure Site Recovery zusammen. Diese Unterstützung gilt für alle Workloads unter dem zuvor erwähnten Betriebssystem.

 **VMware-/physische Server** | **Hyper-V (mit/ohne Virtual Machine Manager)** |
--- | --- |
64-Bit-Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2 mit mindestens SP1<br/><br/> Red Hat Enterprise Linux 6.7, 6.8, 7.1, 7.2 <br/><br/> CentOS 6.5, 6.6, 6.7, 6.8, 7.0, 7.1, 7.2 <br/><br/> Oracle Enterprise Linux 6.4, 6.5, auf dem entweder der Red Hat-kompatible Kernel oder UEK3 (Unbreakable Enterprise Kernel Release 3) ausgeführt wird <br/><br/> SUSE Linux Enterprise Server 11 SP3 <br/><br/> SUSE Linux Enterprise Server 11 SP4 <br/>(Ein Upgrade von replizierenden Computern von SLES 11 SP3 auf SLES 11 SP4 wird nicht unterstützt. Wenn für einen replizierten Computer ein Upgrade von SLES 11 SP3 auf SLES 11 SP4 durchgeführt wurde, müssen Sie die Replikation deaktivieren und den Computer nach dem Upgrade erneut schützen.) | Alle [von Azure unterstützten](https://technet.microsoft.com/library/cc794868.aspx) Gastbetriebssysteme


>[!IMPORTANT]
>(Gilt für VMware-/physische Server, die zu Azure replizieren)
>
> Server mit Red Hat Enterprise Linux Server 7+ und CentOS 7+ mit der Kernel-Version 3.10.0-514 werden ab Version 9.8 des Azure Site Recovery Mobility Service unterstützt.<br/><br/>
> Kunden mit dem Kernel 3.10.0-514 mit einer Version des Mobility Service, die niedriger als Version 9.8 ist, müssen die Replikation deaktivieren, die Version des Mobility Service auf Version 9.8 aktualisieren und dann die Replikation erneut aktivieren.  

## <a name="supported-file-systems-and-guest-storage-configurations-on-linux-vmwarephysical-servers"></a>Unterstützte Dateisysteme und Gastspeicherkonfigurationen unter Linux (VMware-/physische Server)

Die folgenden Dateisysteme und die folgende Software für Speicherkonfigurationen wird auf Linux-Servern unterstützt, die auf VMware- oder physischen Servern ausgeführt werden:
* Dateisysteme: ext3, ext4, ReiserFS (nur Suse Linux Enterprise Server), XFS (nur bis Version 4)
* Volume-Manager: LVM2
* Multipfad-Software: Gerätemapper

Physische Server mit HP CCISS-Speichercontroller werden nicht unterstützt.

>[!Note]
> Auf Linux-Servern müssen sich die folgenden Verzeichnisse (sofern als separate Partitionen/Dateisysteme eingerichtet) auf demselben Datenträger (dem Datenträger mit dem Betriebssystem) auf dem Quellserver befinden: / (root), /boot, /usr, /usr/local, /var, /etc<br/><br/>
> XFS v5-Features, z. B. die Metadatenprüfsumme, werden derzeit von ASR nicht auf XFS-Dateisystemen unterstützt. Stellen Sie sicher, dass Ihre XFS-Dateisysteme keine v5-Features verwenden. Mit dem Hilfsprogramm „xfs_info“ können Sie den XFS-Superblock für die Partition überprüfen. Wenn ftype auf 1 festgelegt ist, werden Features von XFS v5 verwendet. 
>


## <a name="support-for-network-configuration"></a>Unterstützung der Netzwerkkonfiguration
Die folgenden Tabellen fassen die Unterstützung der Netzwerkkonfiguration in verschiedenen Bereitstellungsszenarien bei Verwendung von Azure Site Recovery zur Replikation in Azure zusammen.

### <a name="host-network-configuration"></a>Konfiguration von Hostnetzwerken

**Konfiguration** | **VMware-/physische Server** | **Hyper-V (mit/ohne Virtual Machine Manager)**
--- | --- | ---
NIC-Teaming | Ja<br/><br/>Nicht auf physischen Computern unterstützt| Ja
VLAN | Ja | Ja
IPv4 | Ja | Ja
IPv6 | Nein | Nein

### <a name="guest-vm-network-configuration"></a>Konfiguration von Gast-VM-Netzwerken

**Konfiguration** | **VMware-/physische Server** | **Hyper-V (mit/ohne Virtual Machine Manager)**
--- | --- | ---
NIC-Teaming | Nein | Nein
IPv4 | Ja | Ja
IPv6 | Nein | Nein
Statische IP-Adresse (Windows) | Ja | Ja
Statische IP-Adresse (Linux) | Nein | Nein
Multi-NIC | Ja | Ja

### <a name="failed-over-azure-vm-network-configuration"></a>Netzwerkkonfiguration für virtuellen Azure-Computer nach Failover

**Azure-Netzwerke** | **VMware-/physische Server** | **Hyper-V (mit/ohne Virtual Machine Manager)**
--- | --- | ---
ExpressRoute | Ja | Ja
ILB | Ja | Ja
ELB | Ja | Ja
Traffic Manager | Ja | Ja
Multi-NIC | Ja | Ja
Reservierte IP | Ja | Ja
IPv4 | Ja | Ja
Behalten der Quell-IP | Ja | Ja


## <a name="support-for-storage"></a>Speicherunterstützung
Die folgenden Tabellen fassen die Unterstützung der Speicherkonfiguration in verschiedenen Bereitstellungsszenarien bei Verwendung von Azure Site Recovery zur Replikation in Azure zusammen.

### <a name="host-storage-configuration"></a>Konfiguration von Hostspeichern

**Konfiguration** | **VMware-/physische Server** | **Hyper-V (mit/ohne Virtual Machine Manager)**
--- | --- | --- | ---
NFS | Ja für VMware<br/><br/> Nein für physische Server | N/V
SMB 3.0 | N/V | Ja
SAN (ISCSI) | Ja | Ja
Multipfad (MPIO)<br></br>Getestet mit: Microsoft DSM, EMC PowerPath 5.7 SP4, EMC PowerPath DSM für CLARiiON | Ja | Ja

### <a name="guest-or-physical-server-storage-configuration"></a>Konfiguration des Gast- oder physischen Serverspeichers

**Konfiguration** | **VMware-/physische Server** | **Hyper-V (mit/ohne Virtual Machine Manager)**
--- | --- | ---
VMDK | Ja | –
VHD/VHDX | N/V | Ja
Gen 2-VM | – | Ja
EFI/UEFI| Nein | Ja
Freigegebener Clusterdatenträger | Ja für VMware<br/><br/> Nicht verfügbar für physische Server | Nein
Verschlüsselter Datenträger | Nein | Nein
NFS | Nein | N/V
SMB 3.0 | Nein | Nein
RDM | Ja<br/><br/> Nicht verfügbar für physische Server | N/V
Datenträger > 1 TB | Nein | Nein
Volume mit Stripesetdatenträgern > 1 TB<br/><br/> LVM (logische Volumeverwaltung) | Ja | Ja
Speicherplätze | Nein | Ja
Datenträger laufendem Systembetrieb hinzufügen/entfernen | Nein | Nein
Ausschließen von Datenträgern | Ja | Ja
Multipfad (MPIO) | N/V | Ja

**Azure-Speicher** | **VMware-/physische Server** | **Hyper-V (mit/ohne Virtual Machine Manager)**
--- | --- | ---
LRS | Ja | Ja
GRS | Ja | Ja
RA-GRS | Ja | Ja
Speicherebene „Kalt“ | Nein | Nein
Speicherebene „Heiß“| Nein | Nein
Verschlüsselung ruhender Daten (SSE)| Ja | Ja
Storage Premium | Ja | Ja
Import-/Exportdienst | Nein | Nein


## <a name="support-for-azure-compute-configuration"></a>Unterstützung für Azure-Computekonfiguration

**Computerfeature** | **VMware-/physische Server** | **Hyper-V (mit/ohne Virtual Machine Manager)**
--- | --- | --- | ---
Verfügbarkeitsgruppen | Ja | Ja
HUB | Ja | Ja  

## <a name="failed-over-azure-vm-requirements"></a>Anforderungen für virtuellen Computer nach Failover

Sie können Site Recovery zum Replizieren virtueller Maschinen und physischer Server bereitstellen, auf denen ein von Azure unterstütztes Betriebssystem ausgeführt wird. Dies gilt für die meisten Versionen von Windows und Linux. Lokale virtuelle Computer, die Sie replizieren möchten, müssen zum Replizieren in Azure den folgenden Azure-Anforderungen entsprechen.

**Entität** | **Anforderungen** | **Details**
--- | --- | ---
**Gastbetriebssystem** | Replikation von Hyper-V in Azure: Site Recovery unterstützt alle Betriebssysteme, die [von Azure unterstützt werden](https://technet.microsoft.com/library/cc794868%28v=ws.10%29.aspx). <br/><br/> Replikation von VMware und physischen Servern: Lesen Sie die [Voraussetzungen](site-recovery-vmware-to-azure-classic.md#before-you-start-deployment) | Für die Überprüfung der Voraussetzungen tritt ein Fehler auf, wenn keine Unterstützung vorhanden ist.
**Architektur des Gastbetriebssystems** | 64 Bit | Für die Überprüfung der Voraussetzungen tritt ein Fehler auf, wenn keine Unterstützung vorhanden ist.
**Größe des Betriebssystemdatenträgers** | Bis zu 1.023 GB | Für die Überprüfung der Voraussetzungen tritt ein Fehler auf, wenn keine Unterstützung vorhanden ist.
**Anzahl von Betriebssystemdatenträgern** | 1 | Für die Überprüfung der Voraussetzungen tritt ein Fehler auf, wenn keine Unterstützung vorhanden ist.
**Anzahl von Datenträgern für Daten** | Maximal 64, wenn Sie **VMware-VMs in Azure** replizieren, maximal 16, wenn Sie **Hyper-V-VMs in Azure** replizieren | Für die Überprüfung der Voraussetzungen tritt ein Fehler auf, wenn keine Unterstützung vorhanden ist.
**Größe des VHD-Datenträgers** | Bis zu 1.023 GB | Für die Überprüfung der Voraussetzungen tritt ein Fehler auf, wenn keine Unterstützung vorhanden ist.
**Netzwerkadapter** | Es werden mehrere Adapter unterstützt. |
**Freigegebene VHD** | Nicht unterstützt | Für die Überprüfung der Voraussetzungen tritt ein Fehler auf, wenn keine Unterstützung vorhanden ist.
**FC-Datenträger** | Nicht unterstützt | Für die Überprüfung der Voraussetzungen tritt ein Fehler auf, wenn keine Unterstützung vorhanden ist.
**Festplattenformat** | VHD  <br/><br/> VHDX | Obwohl VHDX in Azure derzeit nicht unterstützt wird, führt Site Recovery automatisch die Konvertierung von VHDX in VHD durch, wenn Sie das Failover auf Azure anstoßen. Wenn Sie das Failover in den lokalen Speicher durchführen, wird für die virtuellen Computer weiterhin das VHDX-Format verwendet.
**BitLocker** | Nicht unterstützt | BitLocker muss deaktiviert werden, bevor Sie einen virtuellen Computer schützen.
**VM-Name** | Zwischen 1 und 63 Zeichen. Ist auf Buchstaben, Zahlen und Bindestriche beschränkt. Der VM-Name muss mit einem Buchstaben oder einer Zahl beginnen und enden. | Aktualisieren Sie den Wert in den Eigenschaften für virtuelle Computer in Site Recovery.
**VM-Typ** | Generation 1<br/><br/> Generation 2: Windows | Zwei virtuelle Computer der Generation 2 mit einem Betriebssystem-Datenträger des Typs „Basic“ (mit einem oder zwei als VHDX formatierten Datenvolumes) und weniger als 300 GB Speicherplatz werden unterstützt.<br></br>Linux-VMs der Generation 2 werden nicht unterstützt. [Weitere Informationen](https://azure.microsoft.com/blog/2015/04/28/disaster-recovery-to-azure-enhanced-and-were-listening/)|

## <a name="support-for-recovery-services-vault-actions"></a>Support für Recovery Services-Tresoraktionen

**Aktion** | **VMware-/physische Server** | **Hyper-V (ohne Virtual Machine Manager)** | **Hyper-V (mit Virtual Machine Manager)**
--- | --- | --- | ---
Tresor über Ressourcengruppen hinweg verschieben<br/><br/> Innerhalb von und über Abonnements hinweg | Nein | Nein | Nein
Speicher, Netzwerk, Azure-VMs über Ressourcengruppen hinweg verschieben<br/><br/> Innerhalb von und über Abonnements hinweg | Nein | Nein | Nein


## <a name="support-for-provider-and-agent"></a>Unterstützung für Anbieter und Agent

**Name** | **Beschreibung** | **Aktuelle Version** | **Details**
--- | --- | --- | --- | ---
**Azure Site Recovery-Anbieter** | Koordiniert die Kommunikation zwischen lokalen Servern und Azure <br/><br/> Wird auf lokalen Virtual Machine Manager-Servern oder auf Hyper-V-Servern installiert, wenn kein Virtual Machine Manager-Server vorhanden ist | 5.1.19 ([über das Portal verfügbar](http://aka.ms/downloaddra)) | [Neueste Features und Fixes](https://support.microsoft.com/kb/3155002)
**Einheitliches Setup für Azure Site Recovery (VMware zu Azure)** | Koordiniert die Kommunikation zwischen lokalen VMware-Servern und Azure  <br/><br/> Installiert auf lokalen VMware-Servern | 9.3.4246.1 (verfügbar über das Portal) | [Neueste Features und Fixes](https://support.microsoft.com/kb/3155002)
**Mobilitätsdienst** | Koordiniert die Replikation zwischen lokalen VMware-Servern/physischen Servern und Azure/sekundärem Standort<br/><br/> Installiert auf einem virtuellen VMware-Computer oder auf physischen Servern, die Sie replizieren möchten  | Nicht verfügbar (über das Portal verfügbar) | –
**Microsoft Azure Recovery Services-Agent (MARS)** | Koordiniert die Replikation zwischen Hyper-V-VMs und Azure<br/><br/> Wird auf lokalen Hyper-V-Servern (mit oder ohne Virtual Machine Manager-Server) installiert | Aktueller Agent ([im Portal verfügbar](http://aka.ms/latestmarsagent)) |






## <a name="next-steps"></a>Nächste Schritte
[Überprüfen der Voraussetzungen](site-recovery-prereq.md)

