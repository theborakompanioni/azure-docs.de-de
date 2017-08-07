---
title: "Überprüfen der Architektur für die Hyper-V-Replikation an einem sekundären Standort mit Azure Site Recovery | Microsoft-Dokumentation"
description: "Dieser Artikel bietet einen Überblick über die Architektur zum Replizieren lokaler virtueller Hyper-V-Computer an einem sekundären System Center-VMM-Standort mit Azure Site Recovery."
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: 07099161-4cc7-4f32-8eb9-2a71bbf0750b
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 07/30/2017
ms.author: raynew
ms.translationtype: HT
ms.sourcegitcommit: fff84ee45818e4699df380e1536f71b2a4003c71
ms.openlocfilehash: b78cd0d5a5395873afaddc8856004775f447e8ea
ms.contentlocale: de-de
ms.lasthandoff: 08/01/2017

---
# <a name="step-1-review-the-architecture-for-hyper-v-replication-to-a-secondary-site"></a>Schritt 1: Überprüfen der Architektur für die Hyper-V-Replikation an einem sekundären Standort

Dieser Artikel beschreibt die Komponenten und Prozesse bei der Replikation lokaler virtueller Hyper-V-Computer (Virtual Machines, VMs) in System Center-VMM-Clouds (Virtual Machine Manager) an einem sekundären VMM-Standort mithilfe des Diensts [Azure Site Recovery](site-recovery-overview.md) über das Azure-Portal.

Kommentare können Sie am Ende dieses Artikels eingeben oder im [Forum zu Azure Recovery Services](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr) veröffentlichen.



## <a name="architectural-components"></a>Komponenten der Architektur

Für das Replizieren virtueller Hyper-V-Computer an einem sekundären VMM-Standort benötigen Sie Folgendes:

**Komponente** | **Standort** | **Details**
--- | --- | ---
**Azure** | Abonnement in Azure. | Sie erstellen einen Recovery Services-Tresor im Azure-Abonnement, um die Replikation zwischen VMM-Standorten zu orchestrieren und zu verwalten.
**VMM-Server** | Sie benötigen einen primären und sekundären VMM-Standort. | Wir empfehlen die Verwendung eines VMM-Servers am primären Standort und eines VMM-Servers am sekundären Standort. 
**Hyper-V-Server** |  Mindestens ein Hyper-V-Hostserver in der primären und der sekundären VMM-Cloud. | Die Daten werden zwischen dem primären und sekundären Hyper-V-Hostserver über das LAN oder VPN per Kerberos- oder Zertifikatauthentifizierung repliziert.  
**Virtuelle Hyper-V-Computer** | Auf dem Hyper-V-Hostserver. | Der Quellhostserver muss über mindestens einen virtuellen Computer verfügen, den Sie replizieren möchten.

## <a name="replication-process"></a>Replikationsprozess

1. Sie richten das Azure-Konto ein, erstellen einen Recovery Services-Tresor und geben an, was Sie replizieren möchten.
2. Sie konfigurieren die Replikationseinstellungen für Quelle und Ziel (einschließlich Installation des Azure Site Recovery-Anbieters auf VMM-Servern und des Microsoft Azure Recovery Services-Agents auf den einzelnen Hyper-V-Hosts).
3. Sie erstellen eine Replikationsrichtlinie für die VMM-Quellcloud. Die Richtlinie wird auf alle VMs angewendet, die sich auf Hosts in der Cloud befinden.
4. Sie aktivieren die Replikation für jede VMM-Instanz. Die erste Replikation eines virtuellen Computers findet gemäß den gewählten Einstellungen statt.
5. Nach Abschluss der ersten Replikation beginnt die Replikation von Deltaänderungen. Nachverfolgte Änderungen für eine Komponente werden in einer HRL-Datei gespeichert.


![Lokal zu lokal](./media/vmm-to-vmm-walkthrough-architecture/arch-onprem-onprem.png)

## <a name="failover-and-failback-process"></a>Failover- und Failbackprozesse

1. Sie können ein geplantes oder ungeplantes [Failover](site-recovery-failover.md) zwischen lokalen Standorten ausführen. Wenn Sie ein geplantes Failover durchführen, werden die Quell-VMs heruntergefahren, um sicherzustellen, dass kein Datenverlust auftritt.
2. Sie können ein Failover für einen einzelnen Computer durchführen oder [Wiederherstellungspläne](site-recovery-create-recovery-plans.md) erstellen, um das Failover von mehreren Computern zu orchestrieren.
4. Nach einem nicht geplanten Failover zu einem sekundären Standort sind die Computer am sekundären Standort nicht für den Schutz und die Replikation aktiviert. Nach einem geplanten Failover sind die Computer am sekundären Standort geschützt.
5. Anschließend führen Sie ein Commit für das Failover durch, um von der Replikat-VM auf die Workload zuzugreifen.
6. Wenn der primäre Standort wieder verfügbar ist, initiieren Sie die umgekehrte Replikation, um vom sekundären Standort an den primären Standort zu replizieren. Die umgekehrte Replikation versetzt die virtuellen Computer in einen geschützten Zustand, aber das sekundäre Rechenzentrum bleibt weiterhin aktiv.
7. Initiieren Sie ein geplantes Failover vom sekundären zum primären Standort, und führen Sie anschließend eine erneute umgekehrte Replikation durch, um den primären Standort wieder zum aktiven Standort zu machen.



## <a name="next-steps"></a>Nächste Schritte

Fahren Sie mit [Schritt 2: Überprüfen der Voraussetzungen und Einschränkungen](vmm-to-vmm-walkthrough-prerequisites.md) fort.

