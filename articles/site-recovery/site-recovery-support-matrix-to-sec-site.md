---
title: "Azure Site Recovery-Supportmatrix zum Replizieren am sekundären Standort | Microsoft-Dokumentation"
description: "Enthält eine Zusammenfassung der unterstützten Betriebssysteme und Komponenten für Azure Site Recovery"
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: jwhit
editor: 
ms.assetid: 
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 01/24/2017
ms.author: raynew
translationtype: Human Translation
ms.sourcegitcommit: e858a70f2ce173eb6b99cbe7a130abb5fdc265e1
ms.openlocfilehash: 0a2c3bec5fc6fb44b4baddc393d6f9387bbb5b94


---
# <a name="azure-site-recovery-support-matrix-for-replicating-to-customer-owned-secondary-site"></a>Azure Site Recovery-Unterstützungsmatrix zum Replizieren am sekundären Standort im Besitz des Kunden

> [!div class="op_single_selector"]
> * [Replizieren in Azure](site-recovery-support-matrix-to-azure.md)
> * [Replizieren am sekundären Standort im Besitz des Kunden](site-recovery-support-matrix-to-sec-site.md)

Eine Liste der Voraussetzungen für Azure Site Recovery finden Sie [hier](site-recovery-best-practices.md). Im folgenden Artikel werden die unterstützten Konfigurationen und Komponenten für Azure Site Recovery beim Replizieren und Wiederherstellen am sekundären Standort im Besitz des Kunden zusammengefasst.

## <a name="support-for-deployment-options"></a>Unterstützung für Bereitstellungsoptionen

**Bereitstellung** | **VMware-/physische Server** | **Hyper-V (keine VMM)** | **Hyper-V (mit VMM)**
--- | --- | --- | ---
**Azure-Portal** | Lokale virtuelle VMware-Computer zu sekundärem VMware-Standort.<br/><br/> Hilfehandbuch herunterladen](http://download.microsoft.com/download/E/0/8/E08B3BCE-3631-4CED-8E65-E3E7D252D06D/InMage_Scout_Standard_User_Guide_8.0.1.pdf) InMage Scout-Benutzerhandbuch. Nicht im Azure-Portal verfügbar. | Nicht unterstützt | Lokale virtuelle Hyper-V-Computer in VMM-Clouds zu einer sekundären VMM-Cloud<br/><br/> Nur Hyper-V-Standardreplikation, keine SAN-Unterstützung
**Klassisches Portal** | Nur im Wartungsmodus. Neue Tresore können nicht erstellt werden. | Nicht unterstützt | Nur im Wartungsmodus.
**PowerShell** | Nicht unterstützt. | Nicht unterstützt | Unterstützt



## <a name="support-for-datacenter-management-servers"></a>Unterstützung für Datencenter-Verwaltungsserver

### <a name="virtualization-management-entities"></a>Virtualisierungsverwaltungsentitäten

**Bereitstellung** | **Unterstützung**
--- | ---
**VMware-VM/physische Server** | vSphere 6.0, 5.5 oder 5.1 mit aktuellem Update
**Hyper-V (mit VMM)** | SCVMM 2016 und SCVMM 2012 R2

  >[!Note]
  > Eine SCVMM 2016-Cloud mit einer Mischung aus Windows Server 2016- und Windows Server 2012 R2-Hosts wird derzeit nicht unterstützt.

### <a name="host-servers"></a>Hostserver

**Bereitstellung** | **Unterstützung**
--- | ---
**VMware-VM/physische Server** | vCenter 5.5 oder 6.0 (Unterstützung nur für 5.5-Features) 
**Hyper-V (keine VMM)** | Keine unterstützte Konfiguration zum Replizieren am sekundären Standort
**Hyper-V mit VMM** | Windows Server 2016, Windows Server 2012 R2 mit aktuellen Updates<br/><br/> Windows Server 2016-Hosts müssen mit SCVMM 2016 verwaltet werden.

## <a name="support-for-replicated-machine-machine-os-versions"></a>Unterstützung für replizierte Computer-Betriebssystemversionen
Die folgende Tabelle fasst die Unterstützung von Betriebssystemen in verschiedenen Bereitstellungsszenarien bei der Verwendung von Azure Site Recovery zusammen. Diese Unterstützung gilt für **alle Workloads** auf dem zuvor erwähnten Betriebssystem.

**VMware-/physische Server** | **Hyper-V (mit VMM)**
--- | --- | ---
64-Bit-Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2 mit mindestens SP1<br/><br/> Red Hat Enterprise Linux 6.7, 7.1, 7.2 <br/><br/> CentOS 6.5, 6.6, 6.7, 7.0, 7.1, 7.2 <br/><br/> Oracle Enterprise Linux 6.4, 6.5, auf dem entweder der Red Hat-kompatible Kernel oder UEK3 (Unbreakable Enterprise Kernel Release 3) ausgeführt wird <br/><br/> SUSE Linux Enterprise Server 11 SP3 | Alle [von Hyper-V unterstützten](https://technet.microsoft.com/library/mt126277.aspx)

>[!Note]
>**Speicherunterstützung für Linux-Versionen** Dateisystem (EXT3, ETX4, ReiserFS, XFS) Multipfad-Software-Gerätemapper (Multipfad) Volume-Manager: (LVM2) Physische Server mit HP CCISS-Controller-Speicher werden **nicht** unterstützt.
>Das ReiserFS-Dateisystem wird nur unter SUSE Linux Enterprise Server 11 SP3 unterstützt.

## <a name="support-for-network"></a>Unterstützung für Netzwerke
Die folgenden Tabellen fassen die Unterstützung der Netzwerkkonfiguration in verschiedenen Bereitstellungsszenarien bei der Verwendung von Azure Site Recovery zur Replikation in Azure zusammen.

### <a name="host-network-configuration"></a>Konfiguration von Hostnetzwerken

**Konfiguration** | **VMware-/physische Server** | **Hyper-V (mit VMM)**
--- | --- | ---
NIC-Teaming | Ja | Ja
VLAN | Ja | Ja
IPv4 | Ja | Ja
IPv6 | Nein | Nein

### <a name="guest-vm-network-configuration"></a>Konfiguration von Gast-VM-Netzwerken

**Konfiguration** | **VMware-/physische Server** | **Hyper-V (mit VMM)**
--- | --- | ---
NIC-Teaming | Nein | Nein
IPv4 | Ja | Ja
IPv6 | Nein | Nein
Statische IP-Adresse (Windows) | Ja | Ja
Statische IP-Adresse (Linux) | Ja | Ja
Multi-NIC | Ja | Ja


## <a name="support-for-storage"></a>Speicherunterstützung
Die folgenden Tabellen fassen die Unterstützung der Speicherkonfiguration in verschiedenen Bereitstellungsszenarien bei der Verwendung von Azure Site Recovery zur Replikation in Azure zusammen.

### <a name="host-storage-configuration"></a>Konfiguration von Hostspeichern

**Speicher (Host)** | **VMware-/physische Server** | **Hyper-V (mit VMM)**
--- | --- | ---
NFS | Ja | NA
SMB 3.0 | NA | Ja
SAN (ISCSI) | Ja | Ja
Multipfad (MPIO) | Ja | Ja

### <a name="guest-physical-server-storage-configuration"></a>Konfiguration des Gastserverspeichers/physischen Serverspeichers

**Konfiguration** | **VMware-/physische Server** | **Hyper-V (mit VMM)**
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

**Aktion** | **VMware-/physische Server** | **Hyper-V (mit VMM)**
--- | --- | ---
Tresor über Ressourcengruppen hinweg verschieben<br/><br/> Innerhalb von und über Abonnements hinweg | Nein | Nein
Speicher, Netzwerk, Azure-VMs über Ressourcengruppen hinweg verschieben<br/><br/> Innerhalb von und über Abonnements hinweg | Nein | Nein

## <a name="support-for-provider-and-agent"></a>Unterstützung für Anbieter und Agent

**Name** | **Beschreibung** | **Aktuelle Version** | **Details**
--- | --- | --- | --- | ---
**Azure Site Recovery-Anbieter** | Koordiniert die Kommunikation zwischen lokalen Servern und Azure <br/><br/> Installiert auf lokalen VMM-Servern oder auf Hyper-V-Servern, falls kein VMM-Server vorhanden ist | 5.1.19 ([verfügbar über das Portal](http://aka.ms/downloaddra)) | [Neueste Features und Fixes](https://support.microsoft.com/kb/3155002)
**Mobilitätsdienst** | Koordiniert die Replikation zwischen lokalen VMware-Servern/physischen Servern und dem sekundären Standort<br/><br/> Installiert auf einem virtuellen VMware-Computer oder auf physischen Servern, die Sie replizieren möchten  | NV (verfügbar über das Portal) | NA


## <a name="next-steps"></a>Nächste Schritte
[Vorbereiten der Bereitstellung](site-recovery-best-practices.md)



<!--HONumber=Jan17_HO4-->


