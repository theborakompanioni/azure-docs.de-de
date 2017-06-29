---
title: "Wie funktioniert die Replikation eines lokalen Computers an einem sekundären Standort in Azure Site Recovery? | Microsoft Docs"
description: "Dieser Artikel bietet einen Überblick über die Komponenten und Architektur, die beim Replizieren von lokalen virtuellen Computern und physischen Servern an einem sekundären Standort mit dem Azure Site Recovery-Dienst verwendet werden."
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: c413efcd-d750-4b22-b34b-15bcaa03934a
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 05/29/2017
ms.author: raynew
ms.translationtype: Human Translation
ms.sourcegitcommit: db18dd24a1d10a836d07c3ab1925a8e59371051f
ms.openlocfilehash: fca95c63964b955db7ddfbe53250702cc8af122e
ms.contentlocale: de-de
ms.lasthandoff: 06/15/2017


---
# <a name="how-does-on-premises-machine-replication-to-a-secondary-site-work-in-site-recovery"></a>Wie funktioniert die Replikation eines lokalen Computers an einem sekundären Standort in Site Recovery?

Dieser Artikel beschreibt die Komponenten und Prozesse bei der Replikation lokaler virtueller Computer und physischer Server in Azure mithilfe des [Azure Site Recovery](site-recovery-overview.md)-Diensts.

Sie können Folgendes an einem sekundären lokalen Standort replizieren:
- Lokale virtuelle Hyper-V-Computer, virtuelle Hyper-V-Computer auf Hyper-V-Clustern und eigenständigen Hosts, die in System Center Virtual Machine Manager-Clouds (VMM) verwaltet werden.
- Lokale virtuelle VMware-Computer und physische Windows- und Linux-Server In diesem Szenario wird die Replikation von Scout verwaltet.

Kommentare können Sie am Ende dieses Artikels eingeben oder im [Forum zu Azure Recovery Services](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr) veröffentlichen.

## <a name="replicate-hyper-v-vms-to-a-secondary-on-premises-site"></a>Replizieren von Hyper-V-VMs an einem sekundären lokalen Standort


### <a name="architectural-components"></a>Komponenten der Architektur

Für das Replizieren von virtuellen Hyper-V-Computern an einem sekundären Standort benötigen Sie Folgendes:

**Komponente** | **Standort** | **Details**
--- | --- | ---
**Azure** | Sie benötigen ein Konto in Microsoft. |
**VMM-Server** | Wir empfehlen die Verwendung eines VMM-Servers am primären Standort und eines VMM-Servers am sekundären Standort. | Jeder VMM-Server muss über eine Internetverbindung verfügen.<br/><br/> Jeder Server sollte mindestens über eine private VMM-Cloud verfügen, und das Hyper-V-Funktionsprofil sollte eingestellt sein.<br/><br/> Sie Installieren den Azure Site Recovery-Anbieter auf dem VMM-Server. Der Anbieter koordiniert und orchestriert die Replikation mit dem Site Recovery-Dienst über das Internet. Die Kommunikation zwischen dem Anbieter und Azure ist geschützt und verschlüsselt.
**Hyper-V-Server** |  Mindestens ein Hyper-V-Hostserver in der primären und der sekundären VMM-Cloud.<br/><br/> Die Server sollten über eine Internetverbindung verfügen.<br/><br/> Die Daten werden zwischen dem primären und sekundären Hyper-V-Hostserver über das LAN oder VPN per Kerberos- oder Zertifikatauthentifizierung repliziert.  
**Virtuelle Hyper-V-Computer** | Auf dem Hyper-V-Quellhostserver. | Der Quellhostserver muss über mindestens einen virtuellen Computer verfügen, den Sie replizieren möchten.

### <a name="replication-process"></a>Replikationsprozess

1. Sie richten das Azure-Konto ein.
2. Sie erstellen einen Replikationsdiensttresor für Site Recovery und konfigurieren die Tresoreinstellungen, z.B.:

    - Replikationsquelle und -ziel (primäre und sekundäre Standorte).
    - Die Installation des Azure Site Recovery-Anbieters und des Microsoft Azure Recovery Services-Agents. Der Anbieter wird auf VMM-Servern installiert, und der Agent wird auf allen Hyper-V-Hosts installiert.
    - Sie erstellen eine Replikationsrichtlinie für die VMM-Quellcloud. Die Richtlinie wird auf alle VMs angewendet, die sich auf Hosts in der Cloud befinden.
    - Sie aktivieren die Replikation für Hyper-V-VMs. Die erste Replikation wird gemäß den Einstellungen der Replikationsrichtlinie durchgeführt.
4. Datenänderungen werden nachverfolgt, und die Replikation von Deltaänderungen in Azure beginnt, nachdem die erste Replikation abgeschlossen wurde. Nachverfolgte Änderungen für eine Komponente werden in einer HRL-Datei gespeichert.
5. Sie führen ein Testfailover aus, um sicherzustellen, dass alles richtig funktioniert.

**Abbildung 1: Replikation von VMM zu VMM**

![Lokal zu lokal](./media/site-recovery-components/arch-onprem-onprem.png)

### <a name="failover-and-failback-process"></a>Failover- und Failbackprozesse

1. Sie können ein geplantes oder ungeplantes [Failover](site-recovery-failover.md) zwischen lokalen Standorten ausführen. Wenn Sie ein geplantes Failover durchführen, werden die Quell-VMs heruntergefahren, um sicherzustellen, dass kein Datenverlust auftritt.
2. Sie können ein Failover für einen einzelnen Computer durchführen oder [Wiederherstellungspläne](site-recovery-create-recovery-plans.md) erstellen, um das Failover von mehreren Computern zu orchestrieren.
4. Nach einem nicht geplanten Failover zu einem sekundären Standort sind die Computer am sekundären Standort nicht für den Schutz und die Replikation aktiviert. Nach einem geplanten Failover sind die Computer am sekundären Standort geschützt.
5. Anschließend führen Sie ein Commit für das Failover durch, um von der Replikat-VM auf die Workload zuzugreifen.
6. Wenn der primäre Standort wieder verfügbar ist, initiieren Sie die umgekehrte Replikation, um vom sekundären Standort an den primären Standort zu replizieren. Die umgekehrte Replikation versetzt die virtuellen Computer in einen geschützten Zustand, aber das sekundäre Rechenzentrum bleibt weiterhin aktiv.
7. Initiieren Sie ein geplantes Failover vom sekundären zum primären Standort, und führen Sie anschließend eine erneute umgekehrte Replikation durch, um den primären Standort wieder zum aktiven Standort zu machen.




## <a name="replicate-vmware-vmsphysical-servers-to-a-secondary-site"></a>Replizieren von VMware-VMs/physischen Servern an einem sekundären Standort

Zum Replizieren von virtuellen VMware-Computern oder physischen Servern an einem sekundären Standort verwenden Sie InMage Scout und die folgenden Architekturkomponenten:


### <a name="architectural-components"></a>Komponenten der Architektur

**Komponente** | **Standort** | **Details**
--- | --- | ---
**Azure** | InMage Scout | Zum Herunterladen von InMage Scout benötigen Sie ein Azure-Abonnement.<br/><br/> Nachdem Sie einen Recovery Services-Tresor erstellt haben, laden Sie InMage Scout herunter und installieren die aktuellen Updates, um die Bereitstellung einzurichten.
**Prozessserver** | Befindet sich am primären Standort. | Der Prozessserver wird zur Abwicklung von Zwischenspeicherung, Komprimierung und Datenoptimierung bereitgestellt.<br/><br/> Außerdem erfolgt hierüber die Pushinstallation des vereinheitlichten Agents auf Maschinen, die Sie schützen möchten.
**Konfigurationsserver** | Befindet sich am sekundären Standort. | Der Konfigurationsserver dient zum Verwalten, Konfigurieren und Überwachen Ihrer Bereitstellung (entweder über die Verwaltungswebsite oder über die vContinuum-Konsole).
**vContinuum-Server** | Optional. Wird an demselben Standort wie der Konfigurationsserver installiert. | Er enthält eine Konsole zum Verwalten und Überwachen Ihrer geschützten Umgebung.
**Masterzielserver** | Befindet sich am sekundären Standort. | Der Masterzielserver enthält replizierte Daten. Er empfängt Daten vom Prozessserver und erstellt einen Replikatcomputer am sekundären Standort. Außerdem enthält er die Punkte für die Beibehaltung der Daten.<br/><br/> Die Anzahl der benötigten Masterzielserver richtet sich nach der Anzahl der Computer, die Sie schützen.<br/><br/> Wenn Sie ein Failback zum primären Standort durchführen möchten, benötigen Sie dort ebenfalls einen Masterzielserver. Der vereinheitlichte Agent wird auf diesem Server installiert.
**VMware ESX/ESXi und vCenter-Server** |  Virtuelle Computer werden auf ESX/ESXi-Hosts gehostet. Hosts werden mit einem vCenter-Server verwaltet. | Sie benötigen eine VMware-Infrastruktur, um VMware-VMs zu replizieren.
**VMs/physische Server** |  Vereinheitlichter Agent, installiert auf virtuellen VMware-Computern und auf physischen Servern, die Sie replizieren möchten. | Der Agent fungiert als Kommunikationsanbieter zwischen allen Komponenten.


### <a name="replication-process"></a>Replikationsprozess

1. Sie richten die Komponentenserver an jedem Standort ein (Konfiguration, Prozess, Masterziel) und installieren den vereinheitlichten Agent auf Computern, die Sie replizieren möchten.
2. Nach der ersten Replikation sendet der Agent auf jedem Computer die Deltareplikationsänderungen an den Prozessserver.
3. Der Prozessserver optimiert die Daten und überträgt sie an den Masterzielserver am sekundären Standort. Der Konfigurationsserver verwaltet den Replikationsprozess.

**Abbildung 2: Replikation von VMware zu VMware**

![VMware zu VMware](./media/site-recovery-components/vmware-to-vmware.png)


## <a name="next-steps"></a>Nächste Schritte

Überprüfen Sie die [Supportmatrix](site-recovery-support-matrix-to-sec-site.md).

