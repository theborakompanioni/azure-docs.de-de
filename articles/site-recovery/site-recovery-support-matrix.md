---
title: Azure Site Recovery-Supportmatrix | Microsoft Docs
description: Enthält eine Zusammenfassung der unterstützten Betriebssysteme und Komponenten für Azure Site Recovery
services: site-recovery
documentationcenter: ''
author: rayne-wiselman
manager: jwhit
editor: ''

ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 10/05/2016
ms.author: raynew

---
# <a name="azure-site-recovery-support-matrix"></a>Azure Site Recovery-Supportmatrix
Dieser Artikel beschreibt die unterstützten Betriebssysteme und Komponenten für die Bereitstellung von Azure Site Recovery. Eine Liste der unterstützten Komponenten und Voraussetzungen steht für die einzelnen Bereitstellungsszenarios im jeweiligen Bereitstellungsartikel zur Verfügung, und dieses Dokument fasst sie zusammen.

## <a name="supported-operating-systems-for-virtualization-servers"></a>Unterstützte Betriebssysteme für Virtualisierungsserver
| **Quelle** | **Ziel** | **Hostbetriebssystem** |
| --- | --- | --- | --- |
| **Hyper-V-Hosts (ohne VMM)** |Azure |Windows Server 2012 R2 mit aktuellen Updates |
| **Hyper-V-Hosts (mit VMM)** |Azure |Windows Server 2012 R2 mit aktuellen Updates |
| **Hyper-V-Hosts (mit VMM)** |Sekundärer VMM-Standort |Mindestens Windows Server 2012 mit aktuellen Updates |
| **VMware-Hosts/vCenter** |Azure |vCenter 5.5 oder 6.0 (Unterstützung nur für 5.5-Features)  <br/><br/>  vSphere 6.0, 5.5 oder 5.1 mit aktuellen Updates |
| **VMware-Hosts/vCenter** |Sekundärer VMware-Standort |vCenter 5.5 oder 6.0 (Unterstützung nur für 5.5-Features)  <br/><br/>  vSphere 6.0, 5.5 oder 5.1 mit aktuellen Updates |

## <a name="supported-requirements-for-replicated-machines"></a>Unterstützte Anforderungen für replizierte Computer
| **Quelle** | **Was wird repliziert** | **Ziel** | **Hostbetriebssystem** |
| --- | --- | --- | --- |
| **Virtuelle Hyper-V-Computer** |Beliebige Workload |Azure |Alle [von Azure unterstützten](https://technet.microsoft.com/library/cc794868.aspx) Gastbetriebssysteme<br/><br/> VMs müssen [Azure-Anforderungen](site-recovery-best-practices.md#azure-virtual-machine-requirements) |
| **Hyper-V-VMs (mit VMM)** |Beliebige Workload |Azure |Alle [von Azure unterstützten](https://technet.microsoft.com/library/cc794868.aspx) Gastbetriebssysteme<br/><br/> VMs müssen [Azure-Anforderungen](site-recovery-best-practices.md#azure-virtual-machine-requirements) |
| **Hyper-V-VMs (mit VMM)** |Beliebige Workload |Sekundärer VMM-Standort |Alle [von Hyper-V unterstützten](https://technet.microsoft.com/library/mt126277.aspx) |
| **Virtuelle VMware-Computer** |Alle Workloads, die auf Windows-VMs ausgeführt werden |Azure |64-Bit-Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2 mit mindestens SP1<br/><br/> VMs müssen [Azure-Anforderungen](site-recovery-best-practices.md#azure-virtual-machine-requirements) |
| **Virtuelle VMware-Computer** |Alle Workloads, die auf Linux-VMs ausgeführt werden |Azure |Red Hat Enterprise Linux 6.7, 7.1, 7.2 <br/><br/> CentOS 6.5, 6.6, 6.7, 7.0, 7.1, 7.2 <br/><br/> Oracle Enterprise Linux 6.4, 6.5, auf dem entweder der Red Hat-kompatible Kernel oder UEK3 (Unbreakable Enterprise Kernel Release 3) ausgeführt wird <br/><br/> SUSE Linux Enterprise Server 11 SP3 <br/><br/>  Erforderlicher Speicher: Dateisystem (EXT3, ETX4, ReiserFS, XFS); Multipfad-Software-Gerätemapper (Multipfad); Volume-Manager: (LVM2). Physische Server mit HP CCISS-Controllerspeicher werden nicht unterstützt. Das ReiserFS-Dateisystem wird nur unter SUSE Linux Enterprise Server 11 SP3 unterstützt.<br/><br/> VMs müssen [Azure-Anforderungen](site-recovery-best-practices.md#azure-virtual-machine-requirements) |
| **Virtuelle VMware-Computer** |Alle Workloads, die auf Windows-VMs ausgeführt werden |Sekundärer VMware-Standort |64-Bit-Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2 mit mindestens SP1 |
| **Virtuelle VMware-Computer** |Alle Workloads, die auf Linux-VMs ausgeführt werden |Sekundärer VMware-Standort |Red Hat Enterprise Linux 6.7, 7.1, 7.2 <br/><br/> CentOS 6.5, 6.6, 6.7, 7.0, 7.1, 7.2 <br/><br/> Oracle Enterprise Linux 6.4, 6.5, auf dem entweder der Red Hat-kompatible Kernel oder UEK3 (Unbreakable Enterprise Kernel Release 3) ausgeführt wird <br/><br/> SUSE Linux Enterprise Server 11 SP3 <br/><br/>  Erforderlicher Speicher: Dateisystem (EXT3, ETX4, ReiserFS, XFS); Multipfad-Software-Gerätemapper (Multipfad); Volume-Manager: (LVM2). Physische Server mit HP CCISS-Controllerspeicher werden nicht unterstützt. Das ReiserFS-Dateisystem wird nur unter SUSE Linux Enterprise Server 11 SP3 unterstützt. |
| **Physische Server** |Alle Workloads, die unter Windows ausgeführt werden |Azure |64-Bit-Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 mit SP1 oder höher |
| **Physische Server** |Beliebige unter Linux ausgeführte Workload |Azure |Red Hat Enterprise Linux 6.7, 7.1, 7.2  <br/><br/> CentOS 6.5, 6.6,6.7,7.0,7.1,7.2 <br/><br/> Oracle Enterprise Linux 6.4, 6.5, auf dem entweder der Red Hat-kompatible Kernel oder UEK3 (Unbreakable Enterprise Kernel Release 3) ausgeführt wird <br/><br/> SUSE Linux Enterprise Server 11 SP3 <br/><br/>  Erforderlicher Speicher: Dateisystem (EXT3, ETX4, ReiserFS, XFS); Multipfad-Software-Gerätemapper (Multipfad); Volume-Manager: (LVM2). Physische Server mit HP CCISS-Controllerspeicher werden nicht unterstützt. Das ReiserFS-Dateisystem wird nur unter SUSE Linux Enterprise Server 11 SP3 unterstützt. |
| **Physische Server** |Alle Workloads, die unter Windows ausgeführt werden |Sekundärer Standort |64-Bit-Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 mit SP1 oder höher |
| **Physische Server** |Beliebige unter Linux ausgeführte Workload |Sekundärer Standort |Red Hat Enterprise Linux 6.7, 7.1, 7.2  <br/><br/> CentOS 6.5, 6.6,6.7,7.0,7.1,7.2 <br/><br/> Oracle Enterprise Linux 6.4, 6.5, auf dem entweder der Red Hat-kompatible Kernel oder UEK3 (Unbreakable Enterprise Kernel Release 3) ausgeführt wird <br/><br/> SUSE Linux Enterprise Server 11 SP3 <br/><br/>  Erforderlicher Speicher: Dateisystem (EXT3, ETX4, ReiserFS, XFS); Multipfad-Software-Gerätemapper (Multipfad); Volume-Manager: (LVM2). Physische Server mit HP CCISS-Controllerspeicher werden nicht unterstützt. Das ReiserFS-Dateisystem wird nur unter SUSE Linux Enterprise Server 11 SP3 unterstützt. |

## <a name="provider-versions"></a>Anbieterversionen
| **Name** | **Beschreibung** | **Aktuelle Version** | **Unterstützung** | **Details** |
| --- | --- | --- | --- | --- |
| **Azure Site Recovery-Anbieter** |Koordiniert die Kommunikation zwischen lokalen Servern und Azure/sekundären Standort  <br/><br/>  Installiert auf lokalen VMM-Servern, oder Hyper-V-Servern, wenn kein VMM-Server vorhanden ist |5.1.1700 (verfügbar über das Portal) |[Neueste Features und Fixes](https://support.microsoft.com/kb/3155002) | |
| **Einheitliches Setup für Azure Site Recovery (VMware zu Azure)** |Koordiniert die Kommunikation zwischen lokalen VMware-Servern und Azure  <br/><br/>  Installiert auf lokalen VMware-Servern |9.3.4246.1 (verfügbar über das Portal) |[Neueste Features und Fixes](https://support.microsoft.com/kb/3155002) | |
| **Mobilitätsdienst** |Koordiniert die Replikation zwischen lokalen VMware-Servern/physischen Servern und Azure/sekundärem Standort |NV (verfügbar über das Portal) |Installiert auf jedem virtuellen VMware-Computer oder physischen Server, den Sie replizieren möchten. **Microsoft Azure Recovery Services-Agent (MARS)** |Koordiniert die Replikation zwischen Hyper-V-VMs und Azure<br/><br/>  Installiert auf lokalen Hyper-V-Servern (mit oder ohne VMM-Server) |

## <a name="next-steps"></a>Nächste Schritte
[Vorbereiten der Bereitstellung](site-recovery-best-practices.md)

<!--HONumber=Oct16_HO2-->


