---
title: "Überprüfen der Architektur für die Replikation physischer Server in Azure mithilfe von Azure Site Recovery | Microsoft-Dokumentation"
description: "Dieser Artikel bietet einen Überblick über die Komponenten und die Architektur, die beim Replizieren von lokalen physischen Servern in Azure mit dem Azure Site Recovery-Dienst verwendet werden."
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: 09c9b136-35f5-465e-8d0f-f4c5d5d9f880
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/27/2017
ms.author: raynew
ms.translationtype: Human Translation
ms.sourcegitcommit: 138f04f8e9f0a9a4f71e43e73593b03386e7e5a9
ms.openlocfilehash: 843c3f1b54f50fe50b162ed242deab717a080830
ms.contentlocale: de-de
ms.lasthandoff: 06/29/2017

---

# <a name="step-1-review-the-architecture-for-physical-server-replication-to-azure"></a>Schritt 1: Überprüfen der Architektur für die Replikation physischer Server in Azure

Dieser Artikel beschreibt die Komponenten und Prozesse, die bei der Replikation lokaler physischer Windows-/Linux-Server in Azure mithilfe des [Azure Site Recovery](site-recovery-overview.md)-Diensts.

Kommentare können Sie am Ende dieses Artikels senden oder im [Forum zu Azure Recovery Services](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr) Fragen stellen.


## <a name="architectural-components"></a>Komponenten der Architektur

Die Tabelle fasst die Komponenten zusammen, die Sie benötigen.

**Komponente** | **Anforderung** | **Details**
--- | --- | ---
**Azure** | Sie benötigen ein Azure-Konto, ein Azure-Speicherkonto und ein Azure-Netzwerk. | Replizierte Daten werden im Speicherkonto gespeichert, und Azure-VMs werden mit den replizierten Daten erstellt, wenn ein Failover auftritt. Für die Azure-VMs wird eine Verbindung mit dem virtuellen Azure-Netzwerk hergestellt, wenn diese erstellt werden.
**Konfigurationsserver** | Ein einzelner lokaler Verwaltungsserver (physischer Server oder VMware-VM), der alle lokalen Site Recovery-Komponenten ausführt. Dazu gehören ein Konfigurationsserver, ein Prozessserver und ein Masterzielserver. | Die Konfigurationsserverkomponente koordiniert die Kommunikation zwischen der lokalen Umgebung und Azure und verwaltet die Datenreplikation.
 **Prozessserver**  | Wird standardmäßig auf dem Konfigurationsserver installiert. | Fungiert als Replikationsgateway. Empfängt Replikationsdaten, optimiert sie durch Zwischenspeicherung, Komprimierung und Verschlüsselung und sendet sie an Azure Storage.<br/><br/> Darüber hinaus wickelt der Prozessserver die Pushinstallation des Mobilitätsdiensts auf geschützten Computern ab.<br/><br/> Sie können zusätzlich separate dedizierte Prozessserver hinzufügen, um steigende Mengen von Replikationsdatenverkehr zu bewältigen.
 **Masterzielserver** | Wird standardmäßig auf dem lokalen Konfigurationsserver installiert. | Verarbeitet die Replikationsdaten während des Failbacks von Azure.<br/><br/> Wenn der Failback-Datenverkehr ein hohes Volumen aufweist, können Sie für das Failback einen separaten Masterzielserver bereitstellen.
**Replizierte Server** | Die Mobilitätsdienstkomponente muss auf jedem Windows- bzw. Linux-Computer installiert sein, den Sie replizieren möchten. Er kann auf jedem Computer manuell installiert werden, oder Sie können eine Pushinstallation über den Prozessserver verwenden.
**Failback** | Für ein Failback ist eine VMware-Infrastruktur erforderlich. Ein Failback zu einem physischen Server ist nicht möglich.


**Abbildung 1: Komponenten für physische Server in Azure**

![Komponenten](./media/physical-walkthrough-architecture/arch-enhanced.png)

## <a name="replication-process"></a>Replikationsprozess

1. Sie richten die Bereitstellung ein, einschließlich lokaler Komponenten und Azure-Komponenten. Im Recovery Services-Tresor geben Sie die Quelle und das Ziel der Replikation an, richten den Konfigurationsserver ein, erstellen eine Replikationsrichtlinie, stellen den Mobilitätsdienst bereit, aktivieren die Replikation und führen ein Testfailover aus.
2.  Die Replikation der Computer beginnt gemäß der Replikationsrichtlinie, und eine erste Kopie der Daten wird im Azure-Speicher repliziert.
4. Die Replikation von Deltaänderungen in Azure beginnt, nachdem die erste Replikation abgeschlossen wurde. Nachverfolgte Änderungen für einen Computer werden in einer HRL-Datei gespeichert.
    - An der Replikation beteiligte Computer kommunizieren mit dem Konfigurationsserver über Port HTTPS 443 für eingehenden Datenverkehr, um die Replikationsverwaltung durchzuführen.
    - An der Replikation beteiligte Computer senden Replikationsdaten an den Prozessserver über Port HTTPS 9443 für eingehenden Datenverkehr (Konfiguration möglich).
    - Der Konfigurationsserver orchestriert die Replikationsverwaltung mit Azure über Port HTTPS 443 für ausgehenden Datenverkehr.
    - Der Prozessserver empfängt Daten von Quellcomputern, optimiert und verschlüsselt sie und sendet sie über Port 443 für ausgehenden Datenverkehr an den Azure-Speicher.
    - Wenn Sie die Multi-VM-Konsistenz aktivieren, kommunizieren Computer in der Replikationsgruppe über Port 20004 miteinander. Multi-VM wird verwendet, wenn Sie mehrere Computer in Replikationsgruppen zusammenfassen, für die bei einem Failover gemeinsame ausfallsichere und anwendungskonsistente Wiederherstellungspunkte verwendet werden. Dies ist hilfreich, wenn auf Computern dieselbe Workload ausgeführt wird und die Computer einheitlich sein müssen.
5. Der Datenverkehr wird auf öffentlichen Endpunkten des Azure-Speichers über das Internet repliziert. Alternativ hierzu können Sie das [öffentliche Peering](../expressroute/expressroute-circuit-peerings.md#public-peering) von Azure ExpressRoute verwenden. Das Replizieren von Datenverkehr über ein Site-to-Site-VPN von einem lokalen Standort nach Azure wird nicht unterstützt.

**Abbildung 2: Replikation für physische Server in Azure**

![Verbessert](./media/physical-walkthrough-architecture/v2a-architecture-henry.png)

## <a name="failover-and-failback-process"></a>Failover- und Failbackprozesse

Nachdem Sie sich vergewissert haben, dass das Testfailover erwartungsgemäß funktioniert, können Sie nach Bedarf ungeplante Failover auf Azure ausführen. Wenn Sie ein Failover ausführen, werden Azure-VMs aus replizierten Daten erstellt. Wenn Ihr primärer lokaler Standort wieder verfügbar ist, können Sie ein Failback durchführen. Beachten Sie Folgendes:

- Geplante Failover werden nicht unterstützt.
- Sie können ein Failover für einen einzelnen Computer ausführen oder [Wiederherstellungspläne](site-recovery-create-recovery-plans.md) erstellen, um ein Failover für mehrere Computer auszuführen.
- Sie führen ein Commit für ein Failover durch, um von der Replikat-VM in Azure auf die Workload zuzugreifen.
- Wenn der primäre Standort wieder verfügbar ist, replizieren Sie den Computer von der sekundären Datenbank auf den primären Standort. Führen Sie dann ein ungeplantes Failover vom sekundären Standort aus. Nachdem Sie den Commit für dieses Failover durchgeführt haben, befinden sich die Daten wieder am lokalen Standort, und Sie müssen die Replikation in Azure erneut aktivieren.

Failbackkomponenten enthalten Folgendes:

- **Temporärer Prozessserver in Azure**: Sie müssen eine Azure-VM einrichten, die als Prozessserver agiert, um die Replikation von Azure zu verarbeiten. Nach Beendigung des Failbacks können Sie diese VM löschen.
- **VPN-Verbindung**: Sie benötigen eine VPN-Verbindung (oder Azure ExpressRoute-Verbindung) zwischen dem Azure-Netzwerk und dem lokalen Standort.
- **Separater lokaler Masterzielserver**: Der lokale Masterzielserver (standardmäßig auf dem Konfigurationsserver installiert) führt das Failback durch. Wenn Sie ein Failback für große Volumen an Datenverkehr ausführen, müssen Sie einen separaten lokalen Masterzielserver einrichten.
- **Failbackrichtlinie**: Sie benötigen eine Failbackrichtlinie. Sie wird automatisch erstellt, wenn Sie die Replikationsrichtlinie erstellen.
- **VMware-Infrastruktur**: Sie müssen ein Failback auf einen lokalen virtuellen VMware-Computer durchführen. Daher benötigen Sie eine lokale VMware-Infrastruktur, auch wenn Sie lokale physische Server in Azure replizieren.

**Abbildung 3: Failback auf physischen Server**

![Failback](./media/physical-walkthrough-architecture/enhanced-failback.png)


## <a name="next-steps"></a>Nächste Schritte

Gehen Sie zu [Schritt 2: Überprüfen der Voraussetzungen und Einschränkungen](physical-walkthrough-prerequisites.md).

