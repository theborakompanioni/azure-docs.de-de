---
title: "Schützen einer SAP NetWeaver-Anwendungsbereitstellung mit mehreren Schichten mit Azure Site Recovery | Microsoft-Dokumentation"
description: "In diesem Artikel wird beschrieben, wie Sie SAP NetWeaver-Anwendungsbereitstellungen mit Azure Site Recovery schützen."
services: site-recovery
documentationcenter: 
author: mayanknayar
manager: rochakm
editor: 
ms.assetid: 
ms.service: site-recovery
ms.workload: backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/07/2017
ms.author: manayar
ms.translationtype: HT
ms.sourcegitcommit: caaf10d385c8df8f09a076d0a392ca0d5df64ed2
ms.openlocfilehash: 951980eeba61e53c983d5b23c301c81eee9528bd
ms.contentlocale: de-de
ms.lasthandoff: 08/08/2017

---
# <a name="protect-a-multi-tier-sap-netweaver-application-deployment-using-azure-site-recovery"></a>Schützen einer SAP NetWeaver-Anwendungsbereitstellung mit mehreren Schichten mit Azure Site Recovery

Die meisten großen und mittleren SAP-Bereitstellungen weisen eine Lösung zur Notfallwiederherstellung auf.  Die Bedeutung robuster und testfähiger Notfallwiederherstellungslösungen ist weiter gewachsen, da immer mehr Kerngeschäftsprozesse in Anwendungen wie SAP verschoben werden.  Azure Site Recovery wurde mit SAP-Anwendungen getestet und integriert und übersteigt die Möglichkeiten der meisten lokalen Notfallwiederherstellungslösungen zu geringeren Gesamtkosten (TCO) als konkurrierende Lösungen.
Mit Azure Site Recovery haben Sie folgende Möglichkeiten:
* Ermöglichen Sie den Schutz von SAP NetWeaver und Produktionsanwendungen, die nicht von NetWeaver stammen und lokal ausgeführt werden, indem Sie die Komponenten in Azure replizieren.
* Ermöglichen Sie den Schutz von SAP NetWeaver und Produktionsanwendungen, die nicht von NetWeaver stammen und Azure ausführen, indem Sie die Komponenten in einem anderen Azure-Datencenter replizieren.
* Vereinfachen der Cloudmigration mithilfe von Site Recovery zum Migrieren Ihrer SAP-Bereitstellung in Azure.
* Vereinfachen Sie SAP-Projektupgrades, Testläufe und Prototyperstellung, indem Sie bei Bedarf einen Produktionsklon zum Testen von SAP-Anwendungen erstellen.

In diesem Artikel wird beschrieben, wie Sie SAP NetWeaver-Anwendungsbereitstellungen mit [Azure Site Recovery](site-recovery-overview.md) schützen. Dieser Artikel behandelt bewährte Methoden für den Schutz einer SAP NetWeaver-Bereitstellung auf drei Ebenen in Azure durch Replikation in ein anderes Azure-Rechenzentrum mithilfe von Azure Site Recovery, die unterstützten Szenarien und Konfigurationen sowie das Ausführen von Failovern, sowohl Testfailover (Übungen zur Notfallwiederherstellung) als auch tatsächliche Failover.


## <a name="prerequisites"></a>Voraussetzungen
Stellen Sie zunächst sicher, dass Sie mit den folgenden Verfahren vertraut sind:

1. [Replizieren von virtuellen VMware-Computern in Azure mithilfe von Site Recovery](azure-to-azure-walkthrough-enable-replication.md)
2. [Entwerfen Ihres Netzwerks für die Notfallwiederherstellung](site-recovery-azure-to-azure-networking-guidance.md)
3. [Testfailover in Azure in Site Recovery](azure-to-azure-walkthrough-test-failover.md)
4. [Failover in Site Recovery](site-recovery-failover.md)
5. Replizieren eines Domänencontrollers unter [Schützen von Active Directory und DNS mit Azure Site Recovery](site-recovery-active-directory.md)
6. Replizieren von SQL Server unter [Schützen von SQL Server mit der Notfallwiederherstellung von SQL Server und Azure Site Recovery](site-recovery-sql.md)

## <a name="supported-scenarios"></a>Unterstützte Szenarien
Mit Azure Site Recovery können Sie eine Notfallwiederherstellungslösung für die folgenden Szenarien implementieren:
* SAP-Systeme, die in einem Azure-Rechenzentrum ausgeführt werden, das in einem anderen Azure-Rechenzentrum repliziert wird (Azure-zu-Azure-DR), wie als Architektur [hier](https://aka.ms/asr-a2a-architecture) dargestellt.
* SAP-Systeme unter lokalen VMware-Servern (oder physischen Servern), die (mit einigen zusätzlichen Komponenten) an einem DR-Standort in einem Azure-Rechenzentrum repliziert werden (VMware-zu-Azure-DR), wie als Architektur [hier](https://aka.ms/asr-v2a-architecture) dargestellt.
* SAP-Systeme unter lokalen Hyper-V-Servern, die (mit einigen zusätzlichen Komponenten) an einem DR-Standort in einem Azure-Rechenzentrum repliziert werden (Hyper-V-zu-Azure-DR), wie als Architektur [hier](https://aka.ms/asr-h2a-architecture) dargestellt.

In diesem Dokument wird der erste Fall verwendet – Azure-zu-Azure-DR –, um die SAP-Notfallwiederherstellungsfunktionen von Azure Site Recovery zu veranschaulichen. Da die Replikation mit Azure Site Recovery anwendungsunabhängig ist, wird davon ausgegangen, dass das beschriebene Verfahren auch auf andere Szenarien anwendbar ist.

### <a name="required-foundation-services"></a>Erforderliche Grunddienste
Dieses Dokumentationsszenario wurde mit den folgenden Grunddiensten bereitgestellt:
* ExpressRoute- oder Site-to-Site-VPN (virtuelles privates Netzwerk)
* Mindestens ein Active Directory-Domänencontroller und ein DNS-Server unter Azure

Es wird empfohlen, die oben angegebene Infrastruktur vor der Bereitstellung von Azure Site Recovery einzurichten.


## <a name="typical-sap-application-deployment"></a>Typische SAP-Anwendungsbereitstellung
Große SAP-Kunden stellen in der Regel zwischen 6 und 20 einzelne SAP-Anwendungen bereit.  Die meisten dieser Anwendungen basieren auf den SAP NetWeaver ABAP- oder Java-Modulen.  Diese NetWeaver-Kernanwendungen werden durch viele spezifische kleinere eigenständige Nicht-NetWeaver-SAP-Module unterstützt sowie normalerweise durch einige Nicht-SAP-Anwendungen.  

Es ist ausgesprochen wichtig, den Bestand aller in einer Landschaft ausgeführten SAP-Anwendungen aufzunehmen und den Bereitstellungsmodus (2 oder 3 Ebenen) sowie Versionen, Patches, Größen, Änderungsraten und Datenträger-Persistenzanforderungen zu ermitteln.

![Bereitstellungsmuster](./media/site-recovery-sap/sap-typical-deployment.png)

Der Persistenzebene der SAP-Datenbank sollte über die nativen DBMS-Tools wie SQL Server AlwaysOn, Oracle DataGuard oder HANA System Replication geschützt werden. Die Clientebene ist ebenfalls nicht durch Azure Site Recovery geschützt, aber es müssen Themen berücksichtigt werden, die sich auf diese Ebene auswirken, z.B. Verzögerung der DNS-Verteilung, Sicherheit und Remotezugriff auf das DR-Rechenzentrum.

Azure Site Recovery ist die empfohlene Lösung für die Anwendungsebene, z.B. (A)SCS. Andere Anwendungen wie Nicht-NetWeaver-SAP-Anwendungen und Nicht-SAP-Anwendungen bilden einen Teil der allgemeinen SAP-Bereitstellungsumgebung und sollten auch mit Azure Site Recovery geschützt werden.

## <a name="replicate-virtual-machines"></a>Replizieren von virtuellen Computern
Befolgen Sie [diese Anleitung](azure-to-azure-walkthrough-enable-replication.md), um mit dem Replizieren aller virtuellen Computer für SAP-Anwendungen in das Azure-DR-Rechenzentrum zu beginnen.

Wenn Sie eine statische IP-Adresse verwenden, können Sie die IP-Adresse angeben, die vom virtuellen Computer im Abschnitt „Netzwerkschnittstellenkarte“ der Compute- und Netzwerkeinstellungen genutzt werden soll.

![Ziel-IP](./media/site-recovery-sap/sap-static-ip.png)


## <a name="creating-a-recovery-plan"></a>Erstellen eines Wiederherstellungsplans
Ein Wiederherstellungsplan ermöglicht die Sequenzierung des Failovers verschiedener Ebenen in einer Anwendung mit mehreren Ebenen, damit die Anwendungskonsistenz gewahrt wird. Führen Sie die [hier](site-recovery-create-recovery-plans.md) beschriebenen Schritte aus, während Sie einen Wiederherstellungsplan für eine Webanwendung mit mehreren Ebenen erstellen.

### <a name="adding-scripts-to-the-recovery-plan"></a>Hinzufügen von Skripts zum Wiederherstellungsplan
Es kann erforderlich sein, nach dem Failover bzw. Testfailover einige Vorgänge auf den virtuellen Azure-Computern durchzuführen, damit Ihre Anwendungen richtig funktionieren. Sie können den Vorgang nach dem Failover, z.B. das Aktualisieren des DNS-Eintrags und das Ändern der Websitebindung und der Verbindungen, automatisieren, indem Sie im Wiederherstellungsplan entsprechende Skripts hinzufügen, wie [in diesem Artikel](site-recovery-create-recovery-plans.md#add-scripts) dargestellt.

### <a name="dns-update"></a>DNS-Update
Wenn das DNS für das dynamische DNS-Update konfiguriert ist, führen virtuelle Computer nach dem Starten normalerweise ein Update des DNS mit der neuen IP-Adresse durch. Falls Sie einen expliziten Schritt zum Aktualisieren des DNS mit den neuen IPs der virtuellen Computer hinzufügen möchten, können Sie dieses [Skript zum Aktualisieren der IP-Adresse im DNS](https://aka.ms/asr-dns-update) als nachfolgende Aktion in Wiederherstellungsplangruppen hinzufügen.  

## <a name="example-azure-to-azure-deployment"></a>Beispiel für eine Azure-zu-Azure-Bereitstellung
Im nachstehenden Diagramm ist das Szenario für Azure-zu-Azure-DR mit Azure Site Recovery dargestellt:
* Das primäre Rechenzentrum befindet sich in Singapur (Azure Asien, Südosten) und das DR-Rechenzentrum in Hongkong (Azure Asien, Osten).  In diesem Szenario wird eine hohe lokale Verfügbarkeit durch die Verwendung von zwei virtuellen Computern mit SQL Server AlwaysOn im synchronen Modus in Singapur bereitgestellt.
* Mit dem Dateifreigabe-ASCS kann hohe Verfügbarkeit für die einzelnen SAP-Fehlerquellen bereitgestellt werden. Der Dateifreigabe ASCS erfordert keinen freigegebenen Clusterdatenträger, und Anwendungen wie SIOS sind nicht erforderlich.
* DR-Schutz für die DBMS-Ebene wird mithilfe der asynchronen Replikation erreicht.
* Dieses Szenario zeigt „symmetrische DR“, eine DR-Lösung, die ein exaktes Replikat des Produktion darstellt, daher weist die SQL Server-DR-Lösung eine lokale hohe Verfügbarkeit auf. Die Verwendung von symmetrischer DR ist für die Datenbankebene nicht zwingend erforderlich, und viele Kunden nutzen die Flexibilität von Cloudbereitstellungen, um nach einem Notfall schnell einen lokalen Hochverfügbarkeitsknoten zu erstellen.
* Das Diagramm zeigt die SAP NetWeaver-ASCS- und Anwendungsserver-Ebene, die durch Azure Site Recovery repliziert wird.

![Replikationsszenario](./media/site-recovery-sap/sap-replication-scenario.png)

## <a name="doing-a-test-failover"></a>Durchführen eines Testfailovers
Befolgen Sie [diese Anleitung](azure-to-azure-walkthrough-test-failover.md), um ein Testfailover durchzuführen.

1.  Wechseln Sie zum Azure-Portal, und wählen Sie Ihren Recovery Services-Tresor aus.
2.  Klicken Sie auf den Wiederherstellungsplan für SAP-Anwendungen.
3.  Klicken Sie auf „Testfailover“.
4.  Wählen Sie den Wiederherstellungspunkt und das virtuelle Azure-Netzwerk aus, um den Prozess für das Testfailover zu starten.
5.  Wenn die sekundäre Umgebung aktiv ist, können Sie Ihre Überprüfungen durchführen.
6.  Klicken Sie nach Abschluss der Überprüfungen auf „Testfailover bereinigen“, um die Failoverumgebung zu bereinigen.

## <a name="doing-a-failover"></a>Durchführen eines Failovers
Befolgen Sie [diese Anleitung](site-recovery-failover.md), wenn Sie ein Failover durchführen.

1.  Wechseln Sie zum Azure-Portal, und wählen Sie Ihren Recovery Services-Tresor aus.
2.  Klicken Sie auf den Wiederherstellungsplan für SAP-Anwendungen.
3.  Klicken Sie auf „Failover“.
4.  Wählen Sie einen Wiederherstellungspunkt aus, um den Failoverprozess zu starten.

## <a name="next-steps"></a>Nächste Schritte
Erfahren Sie mehr über das Erstellen einer Notfallwiederherstellungslösung für SAP NetWeaver-Bereitstellungen mithilfe von Azure Site Recovery in [diesem Whitepaper](http://aka.ms/asr-sap). Das Whitepaper behandelt auch Empfehlungen für unterschiedliche SAP-Architekturen, listet unterstützte Anwendungen und VM-Typen für SAP in Azure auf und beschreibt mögliche Testpläne für Ihre Notfallwiederherstellungslösung.

Erfahren Sie mehr über das [Replizieren anderer Workloads](site-recovery-workload.md) mit Site Recovery.

