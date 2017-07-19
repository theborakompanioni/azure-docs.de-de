---
title: "Überprüfen der Architektur für die VMware-Replikation in Azure | Microsoft-Dokumentation"
description: "Dieser Artikel bietet einen Überblick über die Komponenten und Architektur, die beim Replizieren von lokalen virtuellen VMware-Computern in Azure mit dem Azure Site Recovery-Dienst verwendet werden."
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
ms.topic: article
ms.date: 06/27.017
ms.author: raynew
ms.translationtype: Human Translation
ms.sourcegitcommit: 138f04f8e9f0a9a4f71e43e73593b03386e7e5a9
ms.openlocfilehash: 4aae04a793bab11562c20ceec0e1ae8f1a035a0f
ms.contentlocale: de-de
ms.lasthandoff: 06/29/2017

---

# <a name="step-1-review-the-architecture-for-vmware-replication-to-azure"></a>Schritt 1: Überprüfen der Architektur für die VMware-Replikation in Azure

Dieser Artikel beschreibt die Komponenten und Prozesse bei der Replikation lokaler virtueller VMware-Computer in Azure mithilfe des [Azure Site Recovery](site-recovery-overview.md)-Diensts.

Kommentare können Sie am Ende dieses Artikels senden oder im [Forum zu Azure Recovery Services](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr) Fragen stellen.


## <a name="architectural-components"></a>Komponenten der Architektur

Die Tabelle fasst die Komponenten zusammen, die Sie benötigen.

**Komponente** | **Anforderung** | **Details**
--- | --- | ---
**Azure** | Sie benötigen ein Azure-Abonnement, ein Azure-Speicherkonto und ein Azure-Netzwerk. | Replizierte Daten werden im Speicherkonto gespeichert. Azure-VMs werden mit den replizierten Daten erstellt, wenn ein Failover an Ihrem lokalen Standort auftritt. Für die Azure-VMs wird eine Verbindung mit dem virtuellen Azure-Netzwerk hergestellt, wenn diese erstellt werden.
**Konfigurationsserver** | Ein einzelner lokaler Verwaltungsserver (VMware-VM), der alle lokalen Site Recovery-Komponenten für die Bereitstellung ausführt. Dazu gehören ein Konfigurationsserver, ein Prozessserver und ein Masterzielserver. | Die Konfigurationsserverkomponente koordiniert die Kommunikation zwischen der lokalen Umgebung und Azure und verwaltet die Datenreplikation.
 **Prozessserver**  | Wird standardmäßig auf dem Konfigurationsserver installiert. | Fungiert als Replikationsgateway. Empfängt Replikationsdaten, optimiert sie durch Zwischenspeicherung, Komprimierung und Verschlüsselung und sendet sie an Azure Storage.<br/><br/> Darüber hinaus wickelt der Prozessserver die Pushinstallation des Mobilitätsdiensts auf geschützten Computern ab und führt die automatische Ermittlung von virtuellen VMware-Computern durch.<br/><br/> Bei zunehmender Größe der Bereitstellung können Sie zusätzlich separate dedizierte Prozessserver hinzufügen, um steigende Mengen von Replikationsdatenverkehr zu bewältigen.
 **Masterzielserver** | Wird standardmäßig auf dem lokalen Konfigurationsserver installiert. | Verarbeitet die Replikationsdaten während des Failbacks von Azure.<br/><br/> Wenn der Failback-Datenverkehr ein hohes Volumen aufweist, können Sie für das Failback einen separaten Masterzielserver bereitstellen.
**VMware-Server** | Virtuelle VMware-Computer werden auf vSphere ESXi-Servern gehostet. Wir empfehlen, die Hosts mit einem vCenter-Server zu verwalten. | VMware-Server werden dem Recovery Services-Tresor hinzugefügt.
**Replizierte Computer** | Der Mobilitätsdienst wird auf jedem virtuellen VMware-Computer installiert, den Sie replizieren. Er kann auf jedem Computer manuell installiert werden, oder Sie können eine Pushinstallation über den Prozessserver verwenden.

**Abbildung 1: Komponenten für VMware zu Azure**

![Komponenten](./media/vmware-walkthrough-architecture/arch-enhanced.png)

## <a name="replication-process"></a>Replikationsprozess

1. Sie richten die Bereitstellung ein, einschließlich lokaler Komponenten und Azure-Komponenten. Im Recovery Services-Tresor geben Sie die Quelle und das Ziel der Replikation an, richten den Konfigurationsserver ein, erstellen eine Replikationsrichtlinie, stellen den Mobilitätsdienst bereit, aktivieren die Replikation und führen ein Testfailover aus.
2. Computer replizieren gemäß der Replikationsrichtlinie, und eine erste Kopie der Daten wird im Azure-Speicher repliziert.
3. Die Replikation von Deltaänderungen in Azure beginnt, nachdem die erste Replikation abgeschlossen wurde. Nachverfolgte Änderungen für einen Computer werden in einer HRL-Datei gespeichert.
    - An der Replikation beteiligte Computer kommunizieren mit dem Konfigurationsserver über Port HTTPS 443 für eingehenden Datenverkehr, um die Replikationsverwaltung durchzuführen.
    - An der Replikation beteiligte Computer senden Replikationsdaten an den Prozessserver über Port HTTPS 9443 für eingehenden Datenverkehr (Änderung möglich).
    - Der Konfigurationsserver orchestriert die Replikationsverwaltung mit Azure über Port HTTPS 443 für ausgehenden Datenverkehr.
    - Der Prozessserver empfängt Daten von Quellcomputern, optimiert und verschlüsselt sie und sendet sie über Port 443 für ausgehenden Datenverkehr an den Azure-Speicher.
    - Wenn Sie die Multi-VM-Konsistenz aktivieren, kommunizieren Computer in der Replikationsgruppe über Port 20004 miteinander. Multi-VM wird verwendet, wenn Sie mehrere Computer in Replikationsgruppen zusammenfassen, für die bei einem Failover gemeinsame ausfallsichere und anwendungskonsistente Wiederherstellungspunkte verwendet werden. Dies ist hilfreich, wenn auf Computern dieselbe Workload ausgeführt wird und die Computer einheitlich sein müssen.
4. Der Datenverkehr wird auf öffentlichen Endpunkten des Azure-Speichers über das Internet repliziert. Alternativ hierzu können Sie das [öffentliche Peering](../expressroute/expressroute-circuit-peerings.md#public-peering) von Azure ExpressRoute verwenden. Das Replizieren von Datenverkehr über ein Site-to-Site-VPN von einem lokalen Standort nach Azure wird nicht unterstützt.


**Abbildung 2: Replikation von VMware zu Azure**

![Verbessert](./media/vmware-walkthrough-architecture/v2a-architecture-henry.png)

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

![Failback](./media/vmware-walkthrough-architecture/enhanced-failback.png)


## <a name="next-steps"></a>Nächste Schritte

Gehen Sie zu [Schritt 2: Überprüfen der Voraussetzungen und Einschränkungen](vmware-walkthrough-prerequisites.md).

