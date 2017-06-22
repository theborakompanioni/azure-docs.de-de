---
title: Was ist Site Recovery? | Microsoft Docs
description: "Enthält eine Übersicht über den Azure Site Recovery-Dienst und eine Zusammenfassung der Bereitstellungsszenarien."
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: cfreeman
editor: 
ms.assetid: e9b97b00-0c92-4970-ae92-5166a4d43b68
ms.service: site-recovery
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 03/14/2017
ms.author: raynew
ms.translationtype: Human Translation
ms.sourcegitcommit: a643f139be40b9b11f865d528622bafbe7dec939
ms.openlocfilehash: 276d7949e01b8936804514c5fe8bd26af438bd08
ms.contentlocale: de-de
ms.lasthandoff: 05/31/2017


---
# <a name="what-is-site-recovery"></a>Was ist Site Recovery?

Willkommen beim Azure Site Recovery-Dienst! Dieser Artikel enthält eine kurze Übersicht über den Dienst.

Ausfälle werden durch natürliche Ereignisse und durch Betriebsfehler verursacht. Ihre Organisation benötigt eine Strategie für die Bereiche Geschäftskontinuität und Notfallwiederherstellung (Business Continuity and Disaster Recovery, BCDR), damit Apps und Daten bei geplanten und ungeplanten Ausfallzeiten geschützt und verfügbar bleiben und die normalen Arbeitsbedingungen so schnell wie möglich wiederhergestellt werden können.

Azure Recovery Services unterstützen Sie bei Ihrer BCDR-Strategie. Mit dem [Azure Backup](https://docs.microsoft.com/en-us/azure/backup/)-Dienst sind Ihre Daten geschützt und wiederherstellbar. Site Recovery führt Replikations-, Failover- und Wiederherstellungsvorgänge für Workloads durch und gewährleistet so deren Verfügbarkeit während eines Ausfalls.

## <a name="what-does-site-recovery-provide"></a>Was bietet Site Recovery?

- **Notfallwiederherstellung in der Cloud:** Sie können virtuelle Azure-Computer mit der Azure-zu-Azure-Notfallwiederherstellungslösung replizieren und schützen. Workloads auf virtuellen Computern und physischen Servern können in Azure repliziert werden, anstatt eine Replikation an einem sekundären Standort durchzuführen. So entfallen die Kosten und die Komplexität, die mit der Verwaltung eines sekundären Datencenters verbunden sind.
- **Flexible Replikation für Hybridumgebungen:** Sie können beliebige Workloads replizieren, die von virtuellen Azure-Computern, lokalen virtuellen Hyper-V-Computern, virtuellen VMware-Computern und physischen Windows- bzw. Linux-Servern unterstützt werden.
- **Migration:** Mit Site Recovery können Sie lokale Instanzen und AWS-Instanzen zu virtuellen Azure-Computern oder virtuelle Azure-Computer zwischen Azure-Regionen migrieren.
- **Einfachere Geschäftskontinuität und Notfallwiederherstellung:** Die Replikation kann über einen zentralen Ort im Azure-Portal bereitgestellt werden.  Sie können einfache Failover- und Failbackvorgänge für einzelne und mehrere Computern ausführen.
- **Resilienz:** Site Recovery orchestriert die Replikation und das Failover, ohne Anwendungsdaten abzufangen. Da die replizierten Daten in Azure Storage gespeichert werden, profitieren Sie von der entsprechenden Resilienz. Bei einem Failover werden virtuelle Azure-Computer auf der Grundlage der replizierten Daten erstellt.
- **Replikationsleistung:** Site Recovery bietet eine fortlaufende Replikation für virtuelle Azure-Computer und virtuelle VMware-Computer sowie eine Replikationsfrequenz von 30 Sekunden für Hyper-V. Der RTO-Wert (Recovery Time Objective) kann durch den automatisierten Wiederherstellungsprozess von Site Recovery sowie durch die Integration in [Azure Traffic Manager](https://azure.microsoft.com/blog/reduce-rto-by-using-azure-traffic-manager-with-azure-site-recovery/) verringert werden.
- **Anwendungskonsistenz:** Sie können anwendungskonsistente Momentaufnahmen für die Wiederherstellungspunkte konfigurieren. Zusätzlich zur Erfassung von Datenträgerdaten werden mit anwendungskonsistenten Momentaufnahmen alle Daten im Arbeitsspeicher und alle Transaktionen des Prozesses erfasst.
- **Unterbrechungsfreie Tests:** Zur Unterstützung von Notfallwiederherstellungsübungen können Sie problemlos Testfailover ohne Beeinträchtigung der Produktionsumgebungen oder der laufenden Replikation durchführen.
- **Flexible Failover- und Wiederherstellungsvorgänge:** Für erwartete Ausfälle können geplante Failover ohne Datenverlust ausgeführt werden, für unerwartete Notfälle sind ungeplante Failover mit minimalem Datenverlust (je nach Replikationsfrequenz) möglich. Wenn der primäre Standort wieder verfügbar ist, kann problemlos ein Failback ausgeführt werden.
- **Benutzerdefinierte Wiederherstellungspläne:** Mit Wiederherstellungsplänen können Sie das Failover und die Wiederherstellung von Anwendungen mit mehreren Ebenen, die auf mehrere virtuelle Computer verteilt sind, modellieren und anpassen. Sie sortieren Gruppen innerhalb von Plänen und fügen Skripts und manuelle Aktionen hinzu. Wiederherstellungspläne können in Azure Automation-Runbooks integriert werden.
- **Apps mit mehreren Ebenen:** Sie können Wiederherstellungspläne für sequenzierte Failover- und Wiederherstellungsvorgänge von Apps mit mehreren Ebenen erstellen. Sie können Computer auf verschiedenen Ebenen (Datenbank, Web, App...) in einem Wiederherstellungsplan gruppieren und festlegen, wie Failover- und Startvorgänge für die einzelnen Gruppen ausgeführt werden sollen.
* **Integration in vorhandene BCDR-Technologien:** Site Recovery kann in andere BCDR-Technologien integriert werden. Beispielsweise können Sie Site Recovery verwenden, um das SQL Server-Back-End von geschäftlichen Workloads zu schützen, einschließlich der nativen Unterstützung für SQL Server Always On zum Verwalten des Failovers von Verfügbarkeitsgruppen.
* **Integration der Automation-Bibliothek:** Eine umfassende Azure Automation-Bibliothek bietet produktionsbereite, anwendungsspezifische Skripts, die heruntergeladen und in Site Recovery integriert werden können.
* **Einfache Netzwerkverwaltung:** Die erweiterte Netzwerkverwaltung in Site Recovery und Azure vereinfacht Anforderungen für Anwendungsnetzwerke, z.B. das Reservieren von IP-Adressen, Konfigurieren von Lastenausgleichsmodulen und Integrieren von Azure Traffic Manager für effiziente Netzwerkwechsel.


## <a name="whats-supported"></a>Was wird unterstützt?

**Unterstützt** | **Details**
--- | ---
**Welche Regionen werden für Site Recovery unterstützt?** | [Unterstützte Regionen](https://azure.microsoft.com/en-us/regions/services/) |
**Was kann ich replizieren?** | Virtuelle Azure-Computer (in der Vorschauphase), lokale virtuelle VMware-Computer, virtuelle Hyper-V-Computer sowie physische Windows- und Linux-Server.
**Welche Betriebssysteme benötigen replizierte Computer?** | [Unterstützte Betriebssysteme](site-recovery-support-matrix-azure-to-azure.md#support-for-replicated-machine-os-versions) für virtuelle Azure-Computer<br></br>[Unterstützte Betriebssysteme](site-recovery-support-matrix-to-azure.md#support-for-replicated-machine-os-versions) für virtuelle VMware-Computer<br/><br/> Für virtuelle Hyper-V-Computer wird jedes [Gastbetriebssystem](https://technet.microsoft.com/en-us/windows-server-docs/compute/hyper-v/supported-windows-guest-operating-systems-for-hyper-v-on-windows) unterstützt, das auch von Azure und Hyper-V unterstützt wird.<br/><br/> [Betriebssysteme](site-recovery-support-matrix-to-azure.md#support-for-replicated-machine-os-versions) für physische Server
**Wo kann ich replizieren?** | Für virtuelle Azure-Computer kann eine Replikation in einer anderen Azure-Region durchgeführt werden. <br></br> Für lokale Computer kann eine Replikation in Azure Storage oder in einem sekundären Datencenter erfolgen.<br/><br/> 

>[!NOTE]
>
> Für Hyper-V gilt: In einem sekundären Datencenter können nur virtuelle Computer auf Hyper-V-Hosts repliziert werden, die in System Center VMM-Clouds verwaltet werden.

**Welche VMware-Server/-Hosts benötige ich?** | Virtuelle VMware-Computer, die Sie replizieren möchten, können durch [unterstützte vSphere-Hosts/vCenter-Server](site-recovery-support-matrix-to-azure.md#support-for-datacenter-management-servers) verwaltet werden.
**Welche Workloads kann ich replizieren?** | Sie können jede Workload replizieren, die auf einem unterstützten Replikationscomputer ausgeführt wird. Darüber hinaus hat das Site Recovery-Team App-spezifische Tests für eine [Reihe von Apps](site-recovery-workload.md#workload-summary) durchgeführt.


## <a name="which-azure-portal"></a>Welches Azure-Portal?

* Site Recovery kann im [Azure-Portal](https://portal.azure.com) bereitgestellt werden.
* Im klassischen Azure-Portal können Sie Site Recovery mit dem klassischen Dienstverwaltungsmodell verwalten.
- Das klassische Portal sollte nur zur Verwaltung bereits vorhandener Site Recovery-Bereitstellungen verwendet werden. Über das klassische Portal können keine neuen Tresore erstellt werden.

## <a name="next-steps"></a>Nächste Schritte
* Erfahren Sie mehr über das [Replizieren virtueller Azure-Computer] (site-recovery-azure-to-azure.md).
* Informieren Sie sich ausführlicher über [Workloadunterstützung](site-recovery-workload.md).
* Informieren Sie sich ausführlicher über die [Architektur und Komponenten von Site Recovery](site-recovery-components.md).

