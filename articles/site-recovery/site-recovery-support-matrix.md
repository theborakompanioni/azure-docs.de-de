<properties
	pageTitle="Azure Site Recovery-Supportmatrix | Microsoft Azure"
	description="Enthält eine Zusammenfassung der unterstützten Betriebssysteme und Komponenten für Azure Site Recovery"
	services="site-recovery"
	documentationCenter=""
	authors="rayne-wiselman"
	manager="jwhit"
	editor=""/>

<tags
	ms.service="site-recovery"
	ms.devlang="na"
	ms.topic="article"
	ms.tgt_pltfrm="na"
	ms.workload="storage-backup-recovery"
	ms.date="09/07/2016"
	ms.author="raynew"/>

# Azure Site Recovery-Supportmatrix

Dieser Artikel beschreibt die unterstützten Betriebssysteme und Komponenten für die Bereitstellung von Azure Site Recovery. Eine Liste der unterstützten Komponenten und Voraussetzungen steht für die einzelnen Bereitstellungsszenarien im jeweiligen Bereitstellungsartikel zur Verfügung, aber dieses Thema fasst sie zusammen.

## Unterstützte Betriebssysteme für Virtualisierungsserver


**Replikat** | **Was wird repliziert** | **Replikationsziel** | **Hostbetriebssystem**
---|---|---|--- 
**Hyper-V-Hosts** | Beliebige Workload | Azure | Windows Server 2012 R2 mit aktuellen Updates
**Hyper-V-Hosts in VMM-Clouds** | Beliebige Workload | Azure | Windows Server 2012 R2 mit aktuellen Updates
**Hyper-V-Hosts in VMM-Clouds** | Beliebige Workload | Sekundärer VMM-Standort | Mindestens Windows Server 2012 mit aktuellen Updates
**VMware-Hosts/vCenter** | Beliebige Workload | Azure | vCenter 5.5 oder 6.0 (Unterstützung nur für 5.5-Features) <br/><br/> vSphere 6.0, 5.5 oder 5.1 mit aktuellen Updates
**VMware-Hosts/vCenter** | Beliebige Workload | Sekundärer VMware-Standort | vCenter 5.5 oder 6.0 (Unterstützung nur für 5.5-Features) <br/><br/> vSphere 6.0, 5.5 oder 5.1 mit aktuellen Updates

## Unterstützte Anforderungen für replizierte Computer

**Replikat** | **Was wird repliziert** | **Replikationsziel** | **Hostbetriebssystem**
---|---|---|--- 
**Virtuelle Hyper-V-Computer** | Beliebige Workload | Azure | Alle [von Azure unterstützten](https://technet.microsoft.com/library/cc794868.aspx) Gastbetriebssysteme<br/><br/> VMs müssen [Azure-Anforderungen](site-recovery-best-practices.md#azure-virtual-machine-requirements) erfüllen
**Hyper-V-VMs in VMM-Cloud** | Beliebige Workload | Azure | Alle [von Azure unterstützten](https://technet.microsoft.com/library/cc794868.aspx) Gastbetriebssysteme<br/><br/> VMs müssen [Azure-Anforderungen](site-recovery-best-practices.md#azure-virtual-machine-requirements) erfüllen
**Hyper-V-VMs in VMM-Clouds** | Beliebige Workload | Sekundärer VMM-Standort | Alle [von Hyper-V unterstützten](https://technet.microsoft.com/library/mt126277.aspx) Gastbetriebssysteme
**Virtuelle VMware-Computer** | Alle Workloads, die auf Windows-VMs ausgeführt werden | Azure | 64-Bit-Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2 mit mindestens SP1<br/><br/> VMs müssen [Azure-Anforderungen](site-recovery-best-practices.md#azure-virtual-machine-requirements) erfüllen
**Virtuelle VMware-Computer** | Alle Workloads, die auf Linux-VMs ausgeführt werden | Azure | Red Hat Enterprise Linux 6.7, 7.1, 7.2 <br/><br/> Centos 6.5, 6.6, 6.7, 7.0, 7.1, 7.2 <br/><br/> Oracle Enterprise Linux 6.4, 6.5, entweder mit dem Red Hat-kompatiblen Kernel oder Unbreakable Enterprise Kernel Release 3 (UEK3) ausgeführt <br/><br/> SUSE Linux Enterprise Server 11 SP3 <br/><br/> Erforderlicher Speicher: Dateisystem (EXT3, ETX4, ReiserFS, XFS); Multipfad-Software-Gerätemapper (Multipfad); Volume-Manager: (LVM2). Physische Server mit HP CCISS-Controllerspeicher werden nicht unterstützt. Das ReiserFS-Dateisystem wird nur unter SUSE Linux Enterprise Server 11 SP3 unterstützt.<br/><br/> VMs müssen [Azure-Anforderungen](site-recovery-best-practices.md#azure-virtual-machine-requirements) erfüllen
**Virtuelle VMware-Computer** | Alle Workloads, die auf Windows-VMs ausgeführt werden | Sekundärer VMware-Standort | 64-Bit-Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2 mit SP1 oder höher
**Virtuelle VMware-Computer** | Alle Workloads, die auf Linux-VMs ausgeführt werden | Sekundärer VMware-Standort | Red Hat Enterprise Linux 6.7, 7.1, 7.2 <br/><br/> Centos 6.5, 6.6, 6.7, 7.0, 7.1, 7.2 <br/><br/> Oracle Enterprise Linux 6.4, 6.5, entweder mit dem Red Hat-kompatiblen Kernel oder Unbreakable Enterprise Kernel Release 3 (UEK3) ausgeführt <br/><br/> SUSE Linux Enterprise Server 11 SP3 <br/><br/> Erforderlicher Speicher: Dateisystem (EXT3, ETX4, ReiserFS, XFS); Multipfad-Software-Gerätemapper (Multipfad); Volume-Manager: (LVM2). Physische Server mit HP CCISS-Controllerspeicher werden nicht unterstützt. Das ReiserFS-Dateisystem wird nur unter SUSE Linux Enterprise Server 11 SP3 unterstützt.
**Physische Server** | Alle Workloads, die unter Windows ausgeführt werden | Azure | 64-Bit-Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 mit SP1 oder höher
**Physische Server** | Beliebige unter Linux ausgeführte Workload | Azure | Red Hat Enterprise Linux 6.7, 7.1, 7.2 <br/><br/> Centos 6.5, 6.6, 6.7, 7.0, 7.1, 7.2 <br/><br/> Oracle Enterprise Linux 6.4, 6.5, entweder mit dem Red Hat-kompatiblen Kernel oder Unbreakable Enterprise Kernel Release 3 (UEK3) ausgeführt <br/><br/> SUSE Linux Enterprise Server 11 SP3 <br/><br/> Erforderlicher Speicher: Dateisystem (EXT3, ETX4, ReiserFS, XFS); Multipfad-Software-Gerätemapper (Multipfad); Volume-Manager: (LVM2). Physische Server mit HP CCISS-Controllerspeicher werden nicht unterstützt. Das ReiserFS-Dateisystem wird nur unter SUSE Linux Enterprise Server 11 SP3 unterstützt.
**Physische Server** | Alle Workloads, die unter Windows ausgeführt werden | Sekundärer Standort | 64-Bit-Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 mit SP1 oder höher
**Physische Server** | Beliebige unter Linux ausgeführte Workload | Sekundärer Standort | Red Hat Enterprise Linux 6.7, 7.1, 7.2 <br/><br/> Centos 6.5, 6.6, 6.7, 7.0, 7.1, 7.2 <br/><br/> Oracle Enterprise Linux 6.4, 6.5, entweder mit dem Red Hat-kompatiblen Kernel oder Unbreakable Enterprise Kernel Release 3 (UEK3) ausgeführt <br/><br/> SUSE Linux Enterprise Server 11 SP3 <br/><br/> Erforderlicher Speicher: Dateisystem (EXT3, ETX4, ReiserFS, XFS); Multipfad-Software-Gerätemapper (Multipfad); Volume-Manager: (LVM2). Physische Server mit HP CCISS-Controllerspeicher werden nicht unterstützt. Das ReiserFS-Dateisystem wird nur unter SUSE Linux Enterprise Server 11 SP3 unterstützt.


## Anbieterversionen

**Name** | **Beschreibung** | **Aktuelle Version** | **Unterstützung** | **Details**
---|---|---|---| ---
**Azure Site Recovery-Anbieter** | Koordiniert die Kommunikation zwischen lokalen Servern und Azure/sekundären Standort <br/><br/> Installiert auf lokalen VMM-Servern, oder Hyper-V-Servern, wenn kein VMM-Server vorhanden ist | 5\.1.1700 (verfügbar über das Portal) | [Neueste Features und Fixes](https://support.microsoft.com/kb/3155002)
**Einheitliches Setup für Azure Site Recovery (VMware zu Azure)** | Koordiniert die Kommunikation zwischen lokalen VMware-Servern und Azure <br/><br/> Installiert auf lokalen VMware-Servern | 9\.3.4246.1 (verfügbar über das Portal) | [Neueste Features und Fixes](https://support.microsoft.com/kb/3155002)
**Mobilitätsdienst** | Koordiniert die Replikation zwischen lokalen VMware-Servern/physischen Servern und Azure/sekundärem Standort | NV (verfügbar über das Portal) | Installiert auf jedem virtuellen VMware-Computer oder physischen Server, den Sie replizieren möchten. **Microsoft Azure Recovery Services-Agent (MARS)** | Koordiniert die Replikation zwischen Hyper-V-VMs und Azure<br/><br/> Installiert auf lokalen Hyper-V-Servern (mit oder ohne VMM-Server) | 2\.0.8689.0 (verfügbar über das Portal) | Dieser Agent wird von Azure Site Recovery und Azure Backup verwendet. [Weitere Informationen](https://support.microsoft.com/de-DE/kb/2997692)

## Nächste Schritte

[Vorbereiten der Bereitstellung](site-recovery-best-practices.md)

<!---HONumber=AcomDC_0907_2016-->