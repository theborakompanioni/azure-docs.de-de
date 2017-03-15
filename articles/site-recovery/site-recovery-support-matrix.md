---
title: Azure Site Recovery-Supportmatrix | Microsoft Docs
description: "Enthält eine Zusammenfassung der unterstützten Betriebssysteme und Komponenten für Azure Site Recovery"
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: jwhit
editor: 
ms.assetid: 1bbcc13c-ea21-4349-9ddf-0d7dfdcdcbfb
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 12/04/2016
ms.author: raynew
translationtype: Human Translation
ms.sourcegitcommit: 3b9d269a780e9a4c61263208f26f440b1121c682
ms.openlocfilehash: f437598b612a145c5dd8b46a1ba340d298a76981
ms.lasthandoff: 03/01/2017


---
# <a name="azure-site-recovery-support-matrix"></a>Azure Site Recovery-Supportmatrix

Dieser Artikel fasst die unterstützten Betriebssysteme und Komponenten für Azure Site Recovery zusammen. Eine Liste der unterstützten Komponenten und Voraussetzungen steht für die einzelnen Bereitstellungsszenarios im jeweiligen Bereitstellungsartikel zur Verfügung, und dieses Dokument fasst sie zusammen.

## <a name="support-for-azure-replication-scenarios"></a>Unterstützung für Azure-Replikationsszenarien

**Bereitstellung** | **VMware-/physische Server** | **Hyper-V (keine VMM)** | **Hyper-V (mit VMM)**
--- | --- | --- | ---
**Azure-Portal** | Lokale virtuelle VMware-Computer zu Azure Storage, mit Resource Manager oder klassischem Speicher und Netzwerken.<br/><br/> Führen Sie ein Failover zu Resource Manager-basierten oder klassischen VMs aus. | Lokale virtuelle Hyper-V-Computer (nicht in VMM-Clouds) zu Azure Storage, mit Resource Manager oder klassischem Speicher und Netzwerken.<br/><br/> Führen Sie ein Failover zu Resource Manager-basierten oder klassischen VMs aus. | Lokale virtuelle Hyper-V-Computer (in VMM-Clouds) zu Azure Storage, mit Resource Manager oder klassischem Speicher und Netzwerken.<br/><br/> Führen Sie ein Failover zu Resource Manager-basierten oder klassischen VMs aus.
**Klassisches Portal** | Nur im Wartungsmodus. Neue Tresore können nicht erstellt werden. | Nur im Wartungsmodus. | Nur im Wartungsmodus.
**PowerShell** | Derzeit nicht unterstützt. | Unterstützt | Unterstützt


## <a name="support-for-secondary-site-replication-scenarios"></a>Unterstützung für Szenarien mit Replikation an einen sekundären Standort

**Bereitstellung** | **VMware-/physische Server** | **Hyper-V (keine VMM)** | **Hyper-V (mit VMM)**
--- | --- | --- | ---
**Azure-Portal** | Lokale virtuelle VMware-Computer zu sekundärem VMware-Standort.<br/><br/> Hilfehandbuch herunterladen](http://download.microsoft.com/download/E/0/8/E08B3BCE-3631-4CED-8E65-E3E7D252D06D/InMage_Scout_Standard_User_Guide_8.0.1.pdf) InMage Scout-Benutzerhandbuch. Nicht im Azure-Portal verfügbar. | Nicht unterstützt | Lokale virtuelle Hyper-V-Computer in VMM-Clouds zu einer sekundären VMM-Cloud<br/><br/> Nur Hyper-V-Standardreplikation, keine SAN-Unterstützung
**Klassisches Portal** | Nur im Wartungsmodus. Neue Tresore können nicht erstellt werden. | Nur im Wartungsmodus. | Nur im Wartungsmodus.
**PowerShell** | Nicht unterstützt. | NA | Unterstützt



## <a name="support-for-virtualization-server-operating-systems"></a>Unterstützung für Betriebssysteme für Virtualisierungsserver

### <a name="host-servers-replicate-to-azure"></a>Hostserver (Replikation zu Azure)

**VMware-VM/physische Server** | **Hyper-V (keine VMM)** | **Hyper-V (mit VMM)**
--- | --- | ---
vCenter 5.5 oder 6.0 (Unterstützung nur für 5.5-Features)  <br/><br/> vSphere 6.0, 5.5 oder 5.1 mit aktuellen Updates | Windows Server 2016, Windows Server 2012 R2 mit aktuellen Updates<br/><br/> Hyper-V-Sites mit einer Mischung aus Windows Server 2016- und Windows Server 2012 R2-Hosts werden derzeit nicht unterstützt. | Windows Server 2016, Windows Server 2012 R2 mit aktuellen Updates<br/><br/> Windows Server 2016-Hosts sollten per VMM unter System Center 2016 verwaltet werden.<br/><br/> VMM 2016-Clouds mit einer Mischung aus Windows Server 2016- und Windows Server 2012 R2-Hosts werden derzeit nicht unterstützt.

### <a name="host-servers-replicate-to-secondary-site"></a>Hostserver (Replikation zu einem sekundären Standort)

**VMware-VM/physische Server** | **Hyper-V (mit VMM)**
--- | --- | ---
vCenter 5.5 oder 6.0 (Unterstützung nur für 5.5-Features)  <br/><br/> vSphere 6.0, 5.5 oder 5.1 mit aktuellen Updates | Windows Server 2016, Windows Server 2012 R2 oder Windows Server 2012 mit aktuellen Updates.<br/><br/> Windows Server 2016-Hosts sollten per VMM unter System Center 2016 verwaltet werden.<br/><br/> VMM 2016-Clouds mit einer Mischung aus Windows Server 2016-Hosts und früheren Hosts werden derzeit nicht unterstützt.


## <a name="support-for-replicated-machines"></a>Unterstützung für replizierte Computer

### <a name="machines-replicate-to-azure"></a>Computer (Replikation zu Azure)

Die virtuellen Computer müssen die [Azure-Anforderungen](site-recovery-support-matrix-to-azure.md#failed-over-azure-vm-requirements)erfüllen.

**Anforderung** | **VMware-/physische Server** | **Hyper-V (keine VMM)** | **Hyper-V (mit VMM)**
--- | --- | --- | ---
Was wird repliziert | Jede Workload unter Windows- oder Linux-VM | Beliebige Workload | Beliebige Workload
Hostbetriebssystem | 64-Bit-Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2 mit mindestens SP1<br/><br/> Red Hat Enterprise Linux 6.7, 7.1, 7.2 <br/><br/> CentOS 6.5, 6.6, 6.7, 7.0, 7.1, 7.2 <br/><br/> Oracle Enterprise Linux 6.4, 6.5, auf dem entweder der Red Hat-kompatible Kernel oder UEK3 (Unbreakable Enterprise Kernel Release 3) ausgeführt wird <br/><br/> SUSE Linux Enterprise Server 11 SP3<bbr/><br/> Erforderlicher Speicher: Dateisystem (EXT3, ETX4, ReiserFS, XFS); Multipfad-Software-Gerätemapper (Multipfad); Volume-Manager: (LVM2). Physische Server mit HP CCISS-Controllerspeicher werden nicht unterstützt. Das ReiserFS-Dateisystem wird nur unter SUSE Linux Enterprise Server 11 SP3 unterstützt. | Alle [von Azure unterstützten](https://technet.microsoft.com/library/cc794868.aspx) Gastbetriebssysteme | Alle [von Azure unterstützten](https://technet.microsoft.com/library/cc794868.aspx) Gastbetriebssysteme


### <a name="machines-replicate-to-secondary-site"></a>Computer (Replikation zu sekundärem Standort)

**Anforderung** | **VMware-/physische Server** | **Hyper-V (mit VMM)**
--- | --- | ---
Was wird repliziert | Jede Workload unter Windows- oder Linux-VM | Beliebige Workload | Beliebige Workload
Hostbetriebssystem | 64-Bit-Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2 mit mindestens SP1<br/><br/> Red Hat Enterprise Linux 6.7, 7.1, 7.2 <br/><br/> CentOS 6.5, 6.6, 6.7, 7.0, 7.1, 7.2 <br/><br/> Oracle Enterprise Linux 6.4, 6.5, auf dem entweder der Red Hat-kompatible Kernel oder UEK3 (Unbreakable Enterprise Kernel Release 3) ausgeführt wird <br/><br/> SUSE Linux Enterprise Server 11 SP3<bbr/><br/> Erforderlicher Speicher: Dateisystem (EXT3, ETX4, ReiserFS, XFS); Multipfad-Software-Gerätemapper (Multipfad); Volume-Manager: (LVM2).<br/><br/> Physische Server mit HP CCISS-Controllerspeicher werden nicht unterstützt. Das ReiserFS-Dateisystem wird nur unter SUSE Linux Enterprise Server 11 SP3 unterstützt. | Alle Gastbetriebssysteme, die von Hyper-V unterstützt werden (https://technet.microsoft.com/library/mt126277.aspx)


## <a name="support-for-provider-and-agent"></a>Unterstützung für Anbieter und Agent

**Name** | **Beschreibung** | **Aktuelle Version** | **Details**
--- | --- | --- | --- | ---
**Azure Site Recovery-Anbieter** | Koordiniert die Kommunikation zwischen lokalen Servern und Azure/sekundären Standort  <br/><br/> Installiert auf lokalen VMM-Servern oder auf Hyper-V-Servern, falls kein VMM-Server vorhanden ist | 5.1.1700 (verfügbar über das Portal) | [Neueste Features und Fixes](https://support.microsoft.com/kb/3155002)
**Einheitliches Setup für Azure Site Recovery (VMware zu Azure)** | Koordiniert die Kommunikation zwischen lokalen VMware-Servern und Azure  <br/><br/> Installiert auf lokalen VMware-Servern | 9.3.4246.1 (verfügbar über das Portal) | [Neueste Features und Fixes](https://support.microsoft.com/kb/3155002)
**Mobilitätsdienst** | Koordiniert die Replikation zwischen lokalen VMware-Servern/physischen Servern und Azure/sekundärem Standort<br/><br/> Installiert auf einem virtuellen VMware-Computer oder auf physischen Servern, die Sie replizieren möchten  | NV (verfügbar über das Portal) | verfügbar.
**Microsoft Azure Recovery Services-Agent (MARS)** | Koordiniert die Replikation zwischen Hyper-V-VMs und Azure<br/><br/> Installiert auf lokalen Hyper-V-Servern (mit oder ohne VMM-Server) | |

## <a name="support-for-networking"></a>Support für Netzwerke

### <a name="networking-replicate-to-azure"></a>Netzwerk (Replikation zu Azure)

**Hostnetzwerk** | **VMware-/physische Server** | **Hyper-V (keine VMM)** | **Hyper-V (mit VMM)**
--- | --- | --- | ---
NIC-Teaming | Ja<br/><br/>Nicht auf physischen Computern unterstützt| Ja | Ja
VLAN | Ja | Ja | Ja
IPv4 | Ja | Ja | Ja
IPv6 | Nein | Nein | Nein

**Gast-VM-Netzwerke** | **VMware-/physische Server** | **Hyper-V (keine VMM)** | **Hyper-V (mit VMM)**
--- | --- | --- | ---
NIC-Teaming | Nein | Nein | Nein
IPv4 | Ja | Ja | Ja
IPv6 | Nein | Nein | Nein
Statische IP-Adresse (Windows) | Ja | Ja | Ja
Statische IP-Adresse (Linux) | Nein | Nein | Nein
Multi-NIC | Ja | Ja | Ja

**Azure-Netzwerke** | **VMware-/physische Server** | **Hyper-V (keine VMM)** | **Hyper-V (mit VMM)**
--- | --- | --- | ---
ExpressRoute | Ja | Ja | Ja
ILB | Ja | Ja | Ja
ELB | Ja |  |
Traffic Manager | Ja | Ja | Ja
Multi-NIC | Ja | Ja | Ja
Reservierte IP | Ja | Ja | Ja
IPv4 | Ja | Ja | Ja
Behalten der Quell-IP | Ja | Ja | Ja

### <a name="networking-replicate-to-secondary-site"></a>Netzwerk (Replikation zu einem sekundären Standort)

**Hostnetzwerk** | **VMware-/physische Server** | **Hyper-V (mit VMM)**
--- | --- | ---
NIC-Teaming | Ja | Ja
VLAN | Ja | Ja
IPv4 | Ja | Ja
IPv6 | Nein | Nein

**Gast-VM-Netzwerke** | **VMware-/physische Server** | **Hyper-V (mit VMM)**
--- | --- | ---
NIC-Teaming | Nein | Nein
IPv4 | Ja | Ja
IPv6 | Nein | Nein
Statische IP-Adresse (Windows) | Ja | Ja
Statische IP-Adresse (Linux) | Ja | Ja
Multi-NIC | Ja | Ja


## <a name="support-for-storage"></a>Speicherunterstützung

### <a name="storage-replicate-to-azure"></a>Speicher (Replikation zu Azure)

**Speicher (Host)** | **VMware-/physische Server** | **Hyper-V (keine VMM)** | **Hyper-V (mit VMM)**
--- | --- | --- | ---
NFS | Ja für VMware<br/><br/> Nein für physische Server | NA | NA
SMB 3.0 | NA | Ja | Ja
SAN (ISCSI) | Ja | Ja | Ja
Multipfad (MPIO) | Ja für VMware<br/><br/> Nein für physische Server | Ja | Ja

**Speicher (Gast-VM/physische Server)** | **VMware-/physische Server** | **Hyper-V (keine VMM)** | **Hyper-V (mit VMM)**
--- | --- | --- | ---
VMDK | Ja | NA | NA
VHD/VHDX | NA | Ja | Ja
Gen 2-VM | NA | Ja | Ja
Freigegebener Clusterdatenträger | Ja für VMware<br/><br/> Nein für physische Server | Nein | Nein
Verschlüsselter Datenträger | Nein | Nein | Nein
EFI/UEFI| Nein | Ja | Ja
NFS | Nein | Nein | Nein
SMB 3.0 | Nein | Nein | Nein
RDM | Ja<br/><br/> Nein für physische Server | NA | NA
Datenträger > 1 TB | Nein | Nein | Nein
Volume mit Stripesetdatenträgern > 1 TB<br/><br/> LVM | Ja | Ja | Ja
Speicherplätze | Nein | Ja | Ja
Datenträger laufendem Systembetrieb hinzufügen/entfernen | Nein | Nein | Nein
Ausschließen von Datenträgern | Ja | Ja | Ja
Multipfad (MPIO) | NA | Ja | Ja

**Azure-Speicher** | **VMware-/physische Server** | **Hyper-V (keine VMM)** | **Hyper-V (mit VMM)**
--- | --- | --- | ---
LRS | Ja | Ja | Ja
GRS (nur für Standardspeicher) | Ja | Ja | Ja
Speicherebene „Kalt“ | Nein | Nein | Nein
Speicherebene „Heiß“| Nein | Nein | Nein
Verschlüsselung ruhender Daten | Ja | Ja | Ja
Storage Premium | Ja | Ja | Ja
Import-/Exportdienst | Nein | Nein | Nein


### <a name="storage-replicate-to-secondary-site"></a>Speicher (Replikation zu einem sekundären Standort)

**Speicher (Host)** | **VMware-/physische Server** | **Hyper-V (mit VMM)**
--- | --- | ---
NFS | Ja | NA
SMB 3.0 | NA | Ja
SAN (ISCSI) | Ja | Ja
Multipfad (MPIO) | Ja | Ja

**Speicher (Gast-VM/physische Server)** | **VMware-/physische Server** | **Hyper-V (mit VMM)**
--- | --- | ---
VMDK | Ja | NA
VHD/VHDX | NA | Ja (bis zu 16 Datenträger)
Gen 2-VM | NA | Ja
Freigegebener Clusterdatenträger | Ja  | Nein
Verschlüsselter Datenträger | Nein | Nein
UEFI| Nein | NA
NFS | Nein | Nein
SMB 3.0 | Nein | Nein
RDM | Ja | NA
Datenträger > 1 TB | Nein | Ja
Volume mit Stripesetdatenträgern > 1 TB<br/><br/> LVM | Ja | Ja
Speicherplätze | Nein | Ja
Datenträger laufendem Systembetrieb hinzufügen/entfernen | Nein | Nein
Ausschließen von Datenträgern | Nein | Nein
Multipfad (MPIO) | NA | Ja

## <a name="support-for-recovery-services-vault-actions"></a>Support für Recovery Services-Tresoraktionen

### <a name="vaults-replicate-to-azure"></a>Tresore (Replikation zu Azure)

**Aktion** | **VMware-/physische Server** | **Hyper-V (keine VMM)** | **Hyper-V (mit VMM)**
--- | --- | --- | ---
Tresor über Ressourcengruppen hinweg verschieben<br/><br/> Innerhalb von und über Abonnements hinweg | Nein | Nein | Nein
Speicher, Netzwerk, Azure-VMs über Ressourcengruppen hinweg verschieben<br/><br/> Innerhalb von und über Abonnements hinweg | Nein | Nein | Nein

### <a name="vaults-replicate-to-secondary-site"></a>Tresore (Replikation zu einem sekundären Standort)

**Aktion** | **VMware-/physische Server** | **Hyper-V (mit VMM)**
--- | --- | ---
Tresor über Ressourcengruppen hinweg verschieben<br/><br/> Innerhalb von und über Abonnements hinweg | Nein | Nein
Speicher, Netzwerk, Azure-VMs über Ressourcengruppen hinweg verschieben<br/><br/> Innerhalb von und über Abonnements hinweg | Nein | Nein


## <a name="support-for-azure-compute-replicate-to-azure"></a>Support für Azure-Computer (Replikation zu Azure)

**Computerfeature** | **VMware-/physische Server** | **Hyper-V (keine VMM)** | **Hyper-V (mit VMM)**
--- | --- | --- | ---
Gastcluster mit freigegebenen Datenträgern | Nein | Nein | Nein
Verfügbarkeitsgruppen | Nein | Nein | Nein
HUB | Ja | Ja | Ja

## <a name="support-for-azure-vms"></a>Unterstützung für virtuelle Azure-Computer

Sie können Site Recovery zum Replizieren virtueller Computer und physischer Server bereitstellen, auf denen ein von Azure unterstütztes Betriebssystem ausgeführt wird. Dies gilt für die meisten Versionen von Windows und Linux. Lokale virtuelle Computer, die Sie replizieren möchten, müssen die Azure-Anforderungen erfüllen.

**Feature** | **Anforderungen** | **Details**
--- | --- | ---
**Hyper-V-Host** | Sollte Windows Server 2012 R2 oder höher ausführen | Bei der Überprüfung der Voraussetzungen tritt ein Fehler auf, wenn das Betriebssystem nicht unterstützt wird.
**VMware-Hypervisor** | Unterstütztes Betriebssystem | [Überprüfen der Anforderungen](site-recovery-vmware-to-azure-classic.md#before-you-start-deployment)
**Gastbetriebssystem** | Replikation von Hyper-V in Azure: Site Recovery unterstützt alle Betriebssysteme, die [von Azure unterstützt werden](https://technet.microsoft.com/library/cc794868%28v=ws.10%29.aspx). <br/><br/> Replikation von VMware und physischen Servern: Lesen Sie die [Voraussetzungen](site-recovery-vmware-to-azure-classic.md#before-you-start-deployment) | Für die Überprüfung der Voraussetzungen tritt ein Fehler auf, wenn keine Unterstützung vorhanden ist.
**Architektur des Gastbetriebssystems** | 64 Bit | Für die Überprüfung der Voraussetzungen tritt ein Fehler auf, wenn keine Unterstützung vorhanden ist.
**Größe des Betriebssystemdatenträgers** | Bis zu 1.023 GB | Für die Überprüfung der Voraussetzungen tritt ein Fehler auf, wenn keine Unterstützung vorhanden ist.
**Anzahl von Betriebssystemdatenträgern** | 1 | Für die Überprüfung der Voraussetzungen tritt ein Fehler auf, wenn keine Unterstützung vorhanden ist.
**Anzahl von Datenträgern für Daten** | Maximal 64, wenn Sie **VMware-VMs in Azure** replizieren, maximal 16, wenn Sie **Hyper-V-VMs in Azure** replizieren | Für die Überprüfung der Voraussetzungen tritt ein Fehler auf, wenn keine Unterstützung vorhanden ist.
**Größe des VHD-Datenträgers** | Bis zu 1.023 GB | Für die Überprüfung der Voraussetzungen tritt ein Fehler auf, wenn keine Unterstützung vorhanden ist.
**Netzwerkadapter** | Es werden mehrere Adapter unterstützt. |
**Statische IP-Adresse** | Unterstützt | Wenn für den primären virtuellen Computer eine statische IP-Adresse verwendet wird, können Sie die statische IP-Adresse für den virtuellen Computer angeben, der in Azure erstellt wird.<br/><br/> Statische IP-Adressen werden für eine **Linux-VM unter Hyper-V** nicht unterstützt.
**iSCSI-Datenträger** | Nicht unterstützt | Für die Überprüfung der Voraussetzungen tritt ein Fehler auf, wenn keine Unterstützung vorhanden ist.
**Freigegebene VHD** | Nicht unterstützt | Für die Überprüfung der Voraussetzungen tritt ein Fehler auf, wenn keine Unterstützung vorhanden ist.
**FC-Datenträger** | Nicht unterstützt | Für die Überprüfung der Voraussetzungen tritt ein Fehler auf, wenn keine Unterstützung vorhanden ist.
**Festplattenformat** | VHD  <br/><br/> VHDX | Obwohl VHDX in Azure derzeit nicht unterstützt wird, führt Site Recovery automatisch die Konvertierung von VHDX in VHD durch, wenn Sie das Failover auf Azure anstoßen. Wenn Sie das Failover in den lokalen Speicher durchführen, wird für die virtuellen Computer weiterhin das VHDX-Format verwendet.
**BitLocker** | Nicht unterstützt | BitLocker muss deaktiviert werden, bevor Sie einen virtuellen Computer schützen.
**VM-Name** | Zwischen 1 und 63 Zeichen. Ist auf Buchstaben, Zahlen und Bindestriche beschränkt. Muss mit einem Buchstaben oder einer Zahl beginnen und enden. | Aktualisieren Sie den Wert in den Eigenschaften für virtuelle Computer in Site Recovery.
**VM-Typ** | Generation 1<br/><br/> Generation 2 – Windows | Es werden zwei virtuelle Computer von Generation 2 mit einem Betriebssystemdatenträger des Typs „Basic“ unterstützt, der einen oder zwei Datenträger enthält, die als VHDX formatiert sind und weniger als 300 GB brauchen.<br/><br/>verfügbar. Linux Generation 2-VMs werden nicht unterstützt. [Weitere Informationen](https://azure.microsoft.com/blog/2015/04/28/disaster-recovery-to-azure-enhanced-and-were-listening/) |








## <a name="next-steps"></a>Nächste Schritte
Lesen Sie die [Voraussetzungen](site-recovery-prereq.md).

