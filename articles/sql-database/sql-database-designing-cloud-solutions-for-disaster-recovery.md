---
title: "Entwerfen eines hoch verfügbaren Diensts mit Azure SQL-Datenbank | Microsoft-Dokumentation"
description: "Erfahren Sie, wie Sie einen Anwendungsentwurf für hoch verfügbare Dienste mithilfe von Azure SQL-Datenbank erstellen."
keywords: "cloudbasierte Notfallwiederherstellung,Notfallwiederherstellungslösungen,App-Datensicherung,Georeplikation,Planen der Geschäftskontinuität"
services: sql-database
documentationcenter: 
author: anosov1960
manager: jhubbard
editor: monicar
ms.assetid: e8a346ac-dd08-41e7-9685-46cebca04582
ms.service: sql-database
ms.custom: business continuity
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-management
ms.date: 04/21/2017
ms.author: sashan
ms.translationtype: Human Translation
ms.sourcegitcommit: 95b8c100246815f72570d898b4a5555e6196a1a0
ms.openlocfilehash: b1b67a83a25159414a80382030903d300aad71f7
ms.contentlocale: de-de
ms.lasthandoff: 05/18/2017


---
# <a name="designing-highly-available-services-using-azure-sql-database"></a>Entwerfen eines hoch verfügbaren Diensts mit Azure SQL-Datenbank

Beim Erstellen und Bereitstellen von hoch verfügbaren Diensten auf Azure SQL-Datenbank verwenden Sie [Failovergruppen und aktive Georeplikation](sql-database-geo-replication-overview.md), um Resilienz gegenüber regionalen Fehlern und katastrophalen Ausfällen bereitzustellen und eine schnelle Wiederherstellung in den sekundären Datenbanken zu ermöglichen. Dieser Artikel behandelt allgemeine Anwendungsmuster und erläutert die Vor- und Nachteile der einzelnen Optionen je nach Anwendungsbereitstellungsanforderungen, gewünschter Vereinbarung zum Servicelevel, Datenverkehrslatenz sowie Kosten. Informationen zur Verwendung der aktiven Georeplikation mit elastischen Pools finden Sie unter [Strategien zur Notfallwiederherstellung mit elastischen Pools](sql-database-disaster-recovery-strategies-for-applications-with-elastic-pool.md).

## <a name="design-pattern-1-active-passive-deployment-for-cloud-disaster-recovery-with-a-co-located-database"></a>Entwurfsmuster 1: Aktiv-/Passiv-Bereitstellung für eine cloudbasierte Notfallwiederherstellung mit zusammengestellter Datenbank
Diese Option eignet sich am besten für Anwendungen mit den folgenden Merkmalen:

* Aktive Instanz in einer einzelnen Azure-Region
* Starke Abhängigkeit vom Lese-/ Schreibzugriff auf Daten
* Regionsübergreifende Konnektivität zwischen Webanwendung und Datenbank ist aufgrund von Latenz und Datenverkehrskosten nicht akzeptabel.    

In diesem Fall wird die Topologie für die Anwendungsbereitstellung für den Umgang mit regionalen Notfällen optimiert, bei denen alle Anwendungskomponenten betroffen sind und ihr Failover als Einheit erfolgen muss. Um geografische Redundanz zu erzielen, werden Anwendungslogik und Datenbank in eine andere Region repliziert, aber unter normalen Umständen nicht für Anwendungsworkloads verwendet. Die Anwendung in der sekundären Region muss für das Verwenden einer SQL-Verbindungszeichenfolge für die sekundäre Datenbank konfiguriert werden. Traffic Manager ist für die [Failoverroutingmethode](../traffic-manager/traffic-manager-configure-failover-routing-method.md)eingerichtet.  

> [!NOTE]
> [Azure Traffic Manager](../traffic-manager/traffic-manager-overview.md) wird in diesem Artikel ausschließlich zu Demonstrationszwecken genutzt. Sie können eine beliebige Lastenausgleichslösung einsetzen, die die Failoverroutingmethode unterstützt.    
>

Das folgende Diagramm zeigt diese Konfiguration vor einem Ausfall.

![Konfiguration der SQL-Datenbank-Georeplikation Cloudbasierte Notfallwiederherstellung](./media/sql-database-designing-cloud-solutions-for-disaster-recovery/pattern1-1.png)

Nach einem Ausfall in der primären Region erkennt der SQL-Datenbankdienst, dass auf die primäre Datenbank nicht zugegriffen werden kann, und löst basierend auf den Parametern der Richtlinie für automatisches Failover ein Failover auf die sekundäre Datenbank aus. Abhängig von Ihrer Anwendungs-SLA können Sie entscheiden, zwischen der Erkennung eines Ausfalls und dem Failover selbst eine Toleranzperiode zu konfigurieren. Das Konfigurieren einer Toleranzperiode reduziert das Risiko eines Datenverlusts in den Fällen, in denen der Ausfall schwerwiegend ist und Verfügbarkeit in der Region nicht schnell wiederhergestellt werden kann. Wenn das Endpunktfailover von Traffic Manager initiiert wird, bevor die Failovergruppe das Failover der Datenbank auslöst, kann die Webanwendung keine neue Verbindung mit der Datenbank herstellen. Der Versuch der Anwendung, erneut eine Verbindung herzustellen, ist automatisch erfolgreich, sobald das Datenbankfailover abgeschlossen ist. 

> [!NOTE]
> Um ein vollständig koordiniertes Failover der Anwendung und der Datenbanken zu erreichen, sollten Sie Ihre eigene Überwachungsmethode entwickeln und manuelles Failover der Endpunkte der Webanwendung und der Datenbanken verwenden.
>

Nach Abschluss des Failovers der Endpunkte der Anwendung und der Datenbank startet die Anwendung neu, verarbeitet die Benutzeranforderungen in der Region B und bleibt zusammengestellt mit der Datenbank, weil die primäre Datenbank sich jetzt in Region B befindet. Dieses Szenario wird im folgenden Diagramm veranschaulicht. In allen Diagrammen stehen durchgezogene Linien für aktive Verbindungen, gepunktete Linien für unterbrochene Verbindungen und Stoppschilder für Aktionstrigger.

![Georeplikation: Failover auf sekundäre Datenbank App-Datensicherung](./media/sql-database-designing-cloud-solutions-for-disaster-recovery/pattern1-2.png)

Bei einem Ausfall in der sekundären Region wird der Replikationslink zwischen der primären und sekundären Datenbank unterbrochen, aber das Failover wird nicht ausgelöst, weil die primäre Datenbank nicht betroffen ist. Die Verfügbarkeit der Anwendung ändert sich in diesem Fall nicht, sie wird jedoch ungeschützt und mit dem erhöhten Risiko betrieben, dass beide Regionen nacheinander ausfallen.

> [!NOTE]
> Wir empfehlen ausschließlich Bereitstellungskonfigurationen mit einer einzelnen Notfallwiederherstellungsregion. Der Grund ist, dass die meisten Azure-Gebiete zwei Regionen aufweisen. Diese Konfigurationen bieten Ihrer Anwendung keinen Schutz vor einem katastrophenbedingten Ausfall beider Regionen. Im unwahrscheinlichen Fall eines solchen Ausfalls können Sie Ihre Datenbanken mithilfe eines [Geowiederherstellungsvorgangs](sql-database-disaster-recovery.md#recover-using-geo-restore) in einer dritten Region wiederherstellen.
>

Nach Behebung der Ausfallursache wird die sekundäre Datenbank automatisch mit der primären neu synchronisiert. Während der Synchronisierung kann die Leistung der primären Datenbank geringfügig beeinträchtigt sein, abhängig von der Menge der Daten, die synchronisiert werden müssen. Das folgende Diagramm zeigt einen Ausfall in der sekundären Region.

![Synchronisieren der sekundären Datenbank mit der primären Datenbank Cloudbasierte Notfallwiederherstellung](./media/sql-database-designing-cloud-solutions-for-disaster-recovery/pattern1-3.png)

Die **Hauptvorteile** dieses Entwurfsmusters sind:

* Die gleiche Webanwendung wird beiden Regionen ohne regionsspezifische Konfiguration und ohne zusätzliche Logik bereitgestellt, um auf das Failover zu reagieren. 
* Die Leistung der Anwendung ist vom Failover nicht betroffen, da Webanwendung und Datenbank stets zusammengestellt sind.

Der **Hauptnachteil** besteht darin, dass die redundante Anwendungsinstanz in der sekundären Region nur für die Notfallwiederherstellung verwendet wird.

## <a name="design-pattern-2-active-active-deployment-for-application-load-balancing"></a>Entwurfsmuster 2: Aktiv-/Aktiv-Bereitstellung für Anwendungslastenausgleich
Diese cloudbasierte Notfallwiederherstellung eignet sich am besten für Anwendungen mit den folgenden Merkmalen:

* Hohes Verhältnis von Datenbanklesevorgängen zu -schreibvorgängen
* Die Latenz beim Lesen der Datenbank ist für die Benutzerfreundlichkeit entscheidender als die Latenz beim Schreiben 
* Schreibgeschützte Logik kann von Lese-/Schreiblogik mittels einer anderen Verbindungszeichenfolge getrennt werden
* Schreibgeschützte Logik ist nicht davon abhängig, dass Daten vollständig mit den neuesten Aktualisierungen synchronisiert sind  

Wenn Ihre Anwendungen diese Merkmale aufweisen, kann ein Lastenausgleich der Endbenutzerverbindungen über mehrere Anwendungsinstanzen in verschiedenen Regionen die gesamte Benutzerfreundlichkeit wesentlich verbessern. Zwei der Regionen sollten als DR-Paar ausgewählt werden, und die Failovergruppe sollte die Datenbanken in diesen Regionen enthalten. Zum Implementieren des Lastenausgleichs muss jede Region über eine aktive Instanz der Anwendung verfügen, wobei die Lese-/Schreiblogik (RW) mit dem Lese-/Schreib-Listenerendpunkt der Failovergruppe verbunden ist. Dies garantiert, dass das Failover automatisch initiiert wird, wenn die primäre Datenbank von einem Ausfall betroffen ist. Die schreibgeschützte Logik (RO) in der Webanwendung sollte direkt mit der Datenbank in dieser Region verbunden sein. Traffic Manager muss so eingerichtet werden, dass für jede Anwendungsinstanz das [leistungsorientierte Routing](../traffic-manager/traffic-manager-configure-performance-routing-method.md) mit [Endpunktüberwachung](../traffic-manager/traffic-manager-monitoring.md) aktiviert wird.

Wie bei Muster 1 sollten Sie das Bereitstellen einer ähnlichen Überwachungsanwendung erwägen. Doch im Gegensatz zu Muster 1 ist die Überwachungsanwendung nicht für das Auslösen des Endpunktfailovers zuständig.

> [!NOTE]
> Wenngleich bei diesem Muster mehrere sekundäre Datenbanken zum Einsatz kommen, wird nur die sekundäre Datenbank in Region B für das Failover genutzt und sollte Teil dieser Failovergruppe sein.
>

Traffic Manager muss für das Leistungsrouting so konfiguriert werden, dass die Benutzerverbindungen zur Anwendungsinstanz geleitet werden, die dem geografischen Standort des Benutzers am nächsten ist. Das folgende Diagramm veranschaulicht diese Konfiguration vor einem Ausfall.

![Kein Ausfall: leistungsorientiertes Routing zur nächstgelegenen Anwendung. Georeplikation.](./media/sql-database-designing-cloud-solutions-for-disaster-recovery/pattern2-1.png)

Wenn ein Ausfall der Datenbank in der Region A erkannt wird, leitet die Failovergruppe automatisch ein Failover der primären Datenbank in Region A zur sekundären Datenbank in Region B ein. Sie aktualisiert auch automatisch den Lese-/Schreib-Listenerendpunkt mit Region B, sodass Lese-/Schreibverbindungen in der Webanwendung nicht betroffen sind. Traffic Manager schließt den Endpunkt im Offlinestatus aus der Routingtabelle aus, setzt aber das Routing des Endbenutzerdatenverkehrs an die verbleibenden Online-Instanzen fort. Die schreibgeschützten SQL-Verbindungszeichenfolgen sind nicht betroffen, da sie stets auf die Datenbank in der gleichen Region zeigen. 

Das folgende Diagramm veranschaulicht die neue Konfiguration nach dem Failover.

![Konfiguration nach Failover. Cloudbasierte Notfallwiederherstellung](./media/sql-database-designing-cloud-solutions-for-disaster-recovery/pattern2-2.png)

Bei einem Ausfall in einer der sekundären Regionen entfernt Traffic Manager automatisch den Endpunkt im Offlinestatus in dieser Region aus der Routingtabelle. Der Replikationskanal zur sekundären Datenbank in dieser Region wird unterbrochen. Da in diesem Szenario die übrigen Regionen zusätzlichen Benutzerdatenverkehr verarbeiten müssen, ist die Leistung der Anwendung während des Ausfalls beeinträchtigt. Nach Behebung der Ausfallursache wird die sekundäre Datenbank in der betroffenen Region sofort mit der primären Datenbank synchronisiert. Während der Synchronisierung kann die Leistung der primären Datenbank geringfügig beeinträchtigt werden, was von der Menge der Daten abhängt, die synchronisiert werden muss. Das folgende Diagramm zeigt einen Ausfall in Region B.

![Ausfall in sekundärer Region Cloudbasierte Notfallwiederherstellung – Georeplikation](./media/sql-database-designing-cloud-solutions-for-disaster-recovery/pattern2-3.png)

Der **Hauptvorteil** dieses Entwurfsmusters ist, dass Sie die Anwendungsworkload auf mehrere sekundäre Datenbanken verteilen können, um eine optimale Leistung für die Endbenutzer zu erzielen. Diese Option hat folgende **Nachteile** :

* Lese-/Schreibzugriffsverbindungen zwischen Anwendungsinstanzen und Datenbank haben unterschiedliche Wartezeiten und Kosten.
* Die Anwendungsleistung ist während des Ausfalls beeinträchtigt.

> [!NOTE]
> Ein ähnlicher Ansatz kann verwendet werden, um spezielle Workloads wie Berichtsaufträge, Business Intelligence-Tools oder Sicherungen auszulagern. Diese Workloads nutzen in der Regel umfangreiche Datenbankressourcen. Deshalb sollten Sie ihnen eine der sekundären Datenbanken mit der Leistungsstufe zuweisen, die der erwarteten Workload entspricht.
>

## <a name="design-pattern-3-active-passive-deployment-for-data-preservation"></a>Entwurfsmuster 3: Aktiv-/Passiv-Bereitstellung für die Beibehaltung von Daten
Diese Option eignet sich am besten für Anwendungen mit den folgenden Merkmalen:

* Jeder Datenverlust stellt ein hohes Geschäftsrisiko dar. Das Datenbankfailover darf nur als letzter Ausweg in Frage kommen, falls der Ausfall schwerwiegend ist.
* Die Anwendung unterstützt schreibgeschützte Betriebsmodi und Betriebsmodi mit Lese-/Schreibzugriff und kann für gewisse Zeit im „schreibgeschützten Modus“ betrieben werden.

In diesem Muster wechselt die Anwendung in den schreibgeschützten Modus, sobald bei Lese-/Schreibverbindungen Timeoutfehler auftreten. Die Webanwendung wird beiden Regionen bereitgestellt und enthält eine Verbindung mit dem Lese-/Schreib-Listenerendpunkt und eine andere Verbindung mit dem schreibgeschützten Listenerendpunkt. Traffic Manager muss so eingerichtet werden, dass für den Anwendungsendpunkt in jeder Region das [Failoverrouting](../traffic-manager/traffic-manager-configure-failover-routing-method.md) mit [Endpunktüberwachung](../traffic-manager/traffic-manager-monitoring.md) aktiviert wird.

Das folgende Diagramm veranschaulicht diese Konfiguration vor einem Ausfall.

![Aktiv/Passiv-Bereitstellung vor Failover. Cloudbasierte Notfallwiederherstellung](./media/sql-database-designing-cloud-solutions-for-disaster-recovery/pattern3-1.png)

Wenn Traffic Manager einen Fehler in der Konnektivität mit Region A erkennt, leitet er den Benutzerdatenverkehr automatisch zur Anwendungsinstanz in Region B um. Bei diesem Muster ist wichtig, dass Sie die Toleranzperiode mit Datenverlust auf einen ausreichend hohen Wert einstellen, z.B. 24 Stunden. Dies garantiert, dass Datenverlust verhindert wird, wenn der Ausfall innerhalb dieser Zeit behoben wird. Wenn die Webanwendung in der Region B aktiviert wird, treten bei den Lese-/ Schreibvorgängen Fehler auf. An diesem Punkt sollte sie in den schreibgeschützten Modus wechseln. In diesem Modus werden die Anforderungen automatisch an die sekundäre Datenbank weitergeleitet. Im Falle eines schwerwiegenden Fehlers wird der Ausfall nicht in der Toleranzperiode beseitigt, und die Failovergruppe löst das Failover aus. Danach ist der Lese-/Schreiblistener verfügbar, und die an ihn gerichteten Aufrufe sind erfolgreich. Dies wird im folgenden Diagramm veranschaulicht.

![Ausfall: Anwendung im schreibgeschützten Modus Cloudbasierte Notfallwiederherstellung](./media/sql-database-designing-cloud-solutions-for-disaster-recovery/pattern3-2.png)

Wenn die Ausfallursache in der primären Region innerhalb der Toleranzperiode beseitigt wurde, erkennt Traffic Manager die Wiederherstellung der Konnektivität in der primären Region und leitet den Datenverkehr der Benutzer wieder zur Anwendungsinstanz in Region A. Diese Anwendungsinstanz in Region A nimmt den Betrieb im Lese-/Schreibmodus unter Verwendung der primären Datenbank in Region A wieder auf.

Bei einem Ausfall in der Region B erkennt Traffic Manager den Ausfall des Anwendungsendpunkts in Region B, und die Failovergruppe leitet den schreibgeschützten Listener nach Region A um. Der Endbenutzer merkt nichts von diesem Ausfall, aber die primäre Datenbank wird während des Ausfalls verfügbar gemacht. Dies wird im folgenden Diagramm veranschaulicht.

![Ausfall: sekundäre Datenbank  Cloudbasierte Notfallwiederherstellung](./media/sql-database-designing-cloud-solutions-for-disaster-recovery/pattern3-3.png)

Nach Behebung des Ausfalls wird die sekundäre Datenbank sofort mit der primären synchronisiert, und der schreibgeschützte Listener wird wieder zur sekundären Datenbank in Region B umgeleitet. Während der Synchronisierung könnte die Leistung der primären Datenbank abhängig vom Umfang der zu synchronisierenden Daten leicht beeinträchtigt werden.

Dieses Entwurfsmuster bietet mehrere **Vorteile**:

* Datenverluste während temporärer Ausfälle werden verhindert.
* Ausfallzeiten hängen nur davon ab, wie schnell Traffic Manager den Verbindungsausfall erkennt, was konfigurierbar ist.

Der **Kompromiss** ist:

* Die Anwendung muss im schreibgeschützten Modus betrieben werden können.

> [!NOTE]
> Bei einem dauerhaften Dienstausfall in der Region müssen Sie das Datenbankfailover manuell aktivieren und den Datenverlust akzeptieren. Die Anwendung ist in der sekundären Region mit Lese-/ Schreibzugriff auf die Datenbank betriebsbereit.
>

## <a name="business-continuity-planning-choose-an-application-design-for-cloud-disaster-recovery"></a>Planen der Geschäftskontinuität: Auswählen eines Anwendungsentwurfs für die cloudbasierte Notfallwiederherstellung
Für Ihre spezifische Strategie einer cloudbasierten Notfallwiederherstellung können diese Muster kombiniert oder erweitert werden, um die Anforderungen Ihrer Anwendung bestmöglich zu erfüllen.  Wie bereits erwähnt, basiert die von Ihnen gewählte Strategie auf der SLA, die Sie Ihren Kunden anbieten möchten, und auf der Topologie der Anwendungsbereitstellung. Um Ihnen die Entscheidung zu erleichtern, werden in der folgenden Tabelle die Optionen basierend auf dem geschätzten Datenverlust bzw. auf der RPO (Recovery Point Objective) und der geschätzten Wiederherstellungszeit verglichen.

| Muster | RPO | Geschätzte Wiederherstellungszeit |
|:--- |:--- |:--- |
| Aktiv-/Passiv-Bereitstellung für Notfallwiederherstellung mit zusammengestelltem Datenbankzugriff |Lese-/Schreibzugriff < 5 Sek. |Zeitpunkt der Fehlererkennung + DNS-TTL |
| Aktiv-/Aktiv-Bereitstellung für den Anwendungslastenausgleich |Lese-/Schreibzugriff < 5 Sek. |Zeitpunkt der Fehlererkennung + DNS-TTL |
| Aktiv-/Passiv-Bereitstellung für die Beibehaltung von Daten |Schreibgeschützter Zugriff < 5 Sek. | Schreibgeschützter Zugriff = 0 |
||Lese-/Schreibzugriff = 0 | Lese-/Schreibzugriff = Zeitpunkt der Fehlererkennung + Toleranzperiode mit Verlust von Daten |
|||

## <a name="next-steps"></a>Nächste Schritte
* Informationen über automatisierte Sicherungen von Azure SQL-Datenbanken finden Sie unter [Übersicht: Automatisierte SQL-Datenbanksicherungen](sql-database-automated-backups.md)
* Eine Übersicht und verschiedene Szenarien zum Thema Geschäftskontinuität finden Sie unter [Übersicht über die Geschäftskontinuität](sql-database-business-continuity.md)
* Informationen zum Verwenden automatisierter Sicherungen für die Wiederherstellung finden Sie unter [Wiederherstellen einer Datenbank aus vom Dienst initiierten Sicherungen](sql-database-recovery-using-backups.md)
* Informationen zu schnelleren Wiederherstellungsoptionen finden Sie unter [Aktive Georeplikation](sql-database-geo-replication-overview.md).  
* Informationen zum Verwenden automatisierter Sicherungen für die Archivierung finden Sie unter [Datenbankkopie](sql-database-copy.md)
* Informationen zur Verwendung der aktiven Georeplikation mit elastischen Pools finden Sie unter [Strategien zur Notfallwiederherstellung mit elastischen Pools](sql-database-disaster-recovery-strategies-for-applications-with-elastic-pool.md).

