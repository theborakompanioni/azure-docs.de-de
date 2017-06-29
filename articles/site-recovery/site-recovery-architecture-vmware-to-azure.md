---
title: Wie funktioniert die VMware-Replikation in Azure mit Azure Site Recovery? | Microsoft Docs
description: "Dieser Artikel bietet einen Überblick über die Komponenten und die Architektur, die beim Replizieren von lokalen virtuellen VMware-Computern und physischen Servern in Azure mit dem Azure Site Recovery-Dienst verwendet werden."
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
ROBOTS: NOINDEX, NOFOLLOW
redirect_url: vmware-walkthrough-architecture
ms.translationtype: Human Translation
ms.sourcegitcommit: db18dd24a1d10a836d07c3ab1925a8e59371051f
ms.openlocfilehash: 1ebf6e562c99c5113cc33bc8cb87416c663029c6
ms.contentlocale: de-de
ms.lasthandoff: 06/15/2017

---



# <a name="how-does-vmware-replication-to-azure-work-in-site-recovery"></a>Wie funktioniert die VMware-Replikation in Azure mit Site Recovery?

Dieser Artikel beschreibt die Komponenten und Prozesse bei der Replizierung lokaler virtueller VMware-Computer und physischer Windows-/Linux-Server in Azure mithilfe des [Azure Site Recovery](site-recovery-overview.md)-Diensts.

Beim Replizieren von physischen lokalen Servern in Azure verwendet die Replikation zwar auch die gleichen Komponenten und Prozesse wie bei der Replikation von virtuellen VMware-Computern, beachten Sie jedoch diese Unterschiede:

- Anstelle eines virtuellen VMware-Computers können Sie für den Konfigurationsserver einen physischen Server verwenden.
- Sie benötigen eine lokale VMware-Infrastruktur für das Failback. Ein Failback zu einem physischen Computer ist nicht möglich.

Kommentare können Sie am Ende dieses Artikels senden oder im [Forum zu Azure Recovery Services](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr) Fragen stellen.


## <a name="architectural-components"></a>Komponenten der Architektur

Beim Replizieren von virtuellen VMware-Computern und physischen Servern in Azure sind eine Reihe von Komponenten beteiligt.

**Komponente** | **Standort** | **Details**
--- | --- | ---
**Azure** | In Azure benötigen Sie ein Azure-Konto, ein Azure-Speicherkonto und ein Azure-Netzwerk. | Replizierte Daten werden im Speicherkonto gespeichert, und Azure-VMs werden mit den replizierten Daten erstellt, wenn ein Failover von Ihrem lokalen Standort durchgeführt wird. Für die Azure-VMs wird eine Verbindung mit dem virtuellen Azure-Netzwerk hergestellt, wenn diese erstellt werden.
**Konfigurationsserver** | Ein einzelner lokaler Verwaltungsserver (virtueller VMware-Computer), auf dem alle lokalen Komponenten ausgeführt werden, die für die Bereitstellung benötigt werden, u.a. der Konfigurationsserver, Prozessserver und der Masterzielserver. | Die Konfigurationsserverkomponente koordiniert die Kommunikation zwischen der lokalen Umgebung und Azure und verwaltet die Datenreplikation.
 **Prozessserver**  | Wird standardmäßig auf dem Konfigurationsserver installiert. | Fungiert als Replikationsgateway. Empfängt Replikationsdaten, optimiert sie durch Zwischenspeicherung, Komprimierung und Verschlüsselung und sendet sie an Azure Storage.<br/><br/> Darüber hinaus wickelt der Prozessserver die Pushinstallation des Mobilitätsdiensts auf geschützten Computern ab und führt die automatische Ermittlung von virtuellen VMware-Computern durch.<br/><br/> Bei zunehmender Größe der Bereitstellung können Sie zusätzlich separate dedizierte Prozessserver hinzufügen, um steigende Mengen von Replikationsdatenverkehr zu bewältigen.
 **Masterzielserver** | Wird standardmäßig auf dem lokalen Konfigurationsserver installiert. | Verarbeitet die Replikationsdaten während des Failbacks von Azure.<br/><br/> Wenn der Failback-Datenverkehr ein hohes Volumen aufweist, können Sie für das Failback einen separaten Masterzielserver bereitstellen.
**VMware-Server** | Virtuelle VMware-Computer werden auf vSphere ESXi-Servern gehostet. Wir empfehlen, die Hosts mit einem vCenter-Server zu verwalten. | VMware-Server werden dem Recovery Services-Tresor hinzugefügt.
**Replizierte Computer** | Der Mobilitätsdienst wird auf jedem virtuellen VMware-Computer installiert, den Sie replizieren möchten. Er kann auf jedem Computer manuell installiert werden, oder Sie können eine Pushinstallation über den Prozessserver verwenden.

Weitere Informationen zu Voraussetzungen für die Bereitstellung und Anforderungen für die Komponenten finden Sie in der [Supportmatrix](site-recovery-support-matrix-to-azure.md).

**Abbildung 1: Komponenten für VMware zu Azure**

![Komponenten](./media/site-recovery-components/arch-enhanced.png)

## <a name="replication-process"></a>Replikationsprozess

1. Sie richten die Bereitstellung (einschließlich Azure-Komponenten) und einen Recovery Services-Tresor ein. Im Tresor geben Sie die Quelle und das Ziel der Replikation an, richten den Konfigurationsserver ein, fügen VMware-Server hinzu, erstellen eine Replikationsrichtlinie, stellen den Mobilitätsdienst bereit, aktivieren die Replikation und führen ein Testfailover aus.
2.  Die Replikation der Computer beginnt gemäß der Replikationsrichtlinie, und eine erste Kopie der Daten wird im Azure-Speicher repliziert.
4. Die Replikation von Deltaänderungen in Azure beginnt, nachdem die erste Replikation abgeschlossen wurde. Nachverfolgte Änderungen für einen Computer werden in einer HRL-Datei gespeichert.
    - An der Replikation beteiligte Computer kommunizieren mit dem Konfigurationsserver über Port HTTPS 443 für eingehenden Datenverkehr, um die Replikationsverwaltung durchzuführen.
    - An der Replikation beteiligte Computer senden Replikationsdaten an den Prozessserver über Port HTTPS 9443 für eingehenden Datenverkehr (Konfiguration möglich).
    - Der Konfigurationsserver orchestriert die Replikationsverwaltung mit Azure über Port HTTPS 443 für ausgehenden Datenverkehr.
    - Der Prozessserver empfängt Daten von Quellcomputern, optimiert und verschlüsselt sie und sendet sie über Port 443 für ausgehenden Datenverkehr an den Azure-Speicher.
    - Wenn Sie die Multi-VM-Konsistenz aktivieren, kommunizieren Computer in der Replikationsgruppe über Port 20004 miteinander. Multi-VM wird verwendet, wenn Sie mehrere Computer in Replikationsgruppen zusammenfassen, für die bei einem Failover gemeinsame ausfallsichere und anwendungskonsistente Wiederherstellungspunkte verwendet werden. Dies ist hilfreich, wenn auf Computern dieselbe Workload ausgeführt wird und die Computer einheitlich sein müssen.
5. Der Datenverkehr wird auf öffentlichen Endpunkten des Azure-Speichers über das Internet repliziert. Alternativ hierzu können Sie das [öffentliche Peering](../expressroute/expressroute-circuit-peerings.md#public-peering) von Azure ExpressRoute verwenden. Das Replizieren von Datenverkehr über ein Site-to-Site-VPN von einem lokalen Standort nach Azure wird nicht unterstützt.

**Abbildung 2: Replikation von VMware zu Azure**

![Verbessert](./media/site-recovery-components/v2a-architecture-henry.png)

## <a name="failover-and-failback-process"></a>Failover- und Failbackprozesse

1. Nachdem Sie sich vergewissert haben, dass das Testfailover erwartungsgemäß funktioniert, können Sie nach Bedarf ungeplante Failover auf Azure ausführen. Geplante Failover werden nicht unterstützt.
2. Sie können ein Failover für einen einzelnen Computer ausführen oder [Wiederherstellungspläne](site-recovery-create-recovery-plans.md) erstellen, um ein Failover für mehrere Computer auszuführen.
3. Wenn Sie ein Failover durchführen, werden in Azure Replikat-VMs erstellt. Sie führen ein Commit für ein Failover durch, um von der Replikat-VM in Azure auf die Workload zuzugreifen.
4. Wenn Ihr primärer lokaler Standort wieder verfügbar ist, können Sie das Failback durchführen. Sie richten eine Failbackinfrastruktur ein, beginnen mit der Replikation des Computers vom sekundären Standort zum primären Standort und führen ein ungeplantes Failover vom sekundären Standort aus. Nachdem Sie den Commit für dieses Failover durchgeführt haben, befinden sich die Daten wieder am lokalen Standort, und Sie müssen die Replikation in Azure erneut aktivieren. [Weitere Informationen](site-recovery-failback-azure-to-vmware.md)

Für Failbacks gelten einige Anforderungen:


- **Temporärer Prozessserver in Azure**: Wenn Sie nach einem Failover ein Failback aus Azure durchführen möchten, müssen Sie eine als Prozessserver konfigurierte Azure-VM für die Replikation aus Azure einrichten. Nach Beendigung des Failbacks können Sie diese VM löschen.
- **VPN-Verbindung**: Für das Failback benötigen Sie eine VPN-Verbindung (oder Azure ExpressRoute) zwischen dem Azure-Netzwerk und dem lokalen Standort.
- **Separater lokaler Masterzielserver**: Der lokale Masterzielserver führt das Failback durch. Der Masterzielserver wird standardmäßig auf dem Verwaltungsserver installiert, aber wenn Sie Failbacks für größere Mengen von Datenverkehr durchführen, sollten Sie für diese Zwecke einen separaten lokalen Masterzielserver einrichten.
- **Failbackrichtlinie**: Für die Replikation zurück an den lokalen Standort benötigen Sie eine Failbackrichtlinie. Sie wird automatisch erstellt, wenn Sie die Replikationsrichtlinie erstellen.
- **VMware-Infrastruktur**: Sie müssen ein Failback auf einen lokalen virtuellen VMware-Computer durchführen. Daher benötigen Sie eine lokale VMware-Infrastruktur, auch wenn Sie lokale physische Server in Azure replizieren.

**Abbildung 3: Failback „VMware/physisch“**

![Failback](./media/site-recovery-components/enhanced-failback.png)


## <a name="next-steps"></a>Nächste Schritte

Überprüfen Sie die [Supportmatrix](site-recovery-support-matrix-to-azure.md).

