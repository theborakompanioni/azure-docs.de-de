---
title: "App-Entwurf für die Notfallwiederherstellung – Azure SQL-Datenbank | Microsoft-Dokumentation"
description: "Erfahren Sie mehr über den Anwendungsentwurf für die Notfallwiederherstellung für Azure SQL-Datenbank mithilfe der Georeplikation"
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
ms.date: 07/20/2016
ms.author: sashan
translationtype: Human Translation
ms.sourcegitcommit: 7e607debe47efb6a22ca6fa47a40554d13d29359
ms.openlocfilehash: dd56a8d1ee428b1845ed80f0b899cc73c2c4b7f6


---
# <a name="application-design-for-cloud-disaster-recovery-using-active-geo-replication-in-sql-database"></a>Anwendungsentwurf für die cloudbasierte Notfallwiederherstellung mithilfe der aktiven Georeplikation in SQL-Datenbank
> [!NOTE]
> [Die aktive Georeplikation](sql-database-geo-replication-overview.md) ist jetzt für alle Datenbanken in allen Tarifen verfügbar.
>
>

Erfahren Sie, wie Sie die [aktive Georeplikation](sql-database-geo-replication-overview.md) in der SQL-Datenbank verwenden, um Datenbankanwendungen zu entwerfen, die gegen regionale Ausfälle und schwerwiegende Fehler resistent sind. Für die Planung der Geschäftskontinuität müssen Sie die Topologie für die Anwendungsbereitstellung, die angestrebte Vereinbarung zum Servicelevel (Service Level Agreement, SLA), die Datenverkehrslatenz und die Kosten berücksichtigen. In diesem Artikel untersuchen wir die gängigsten Anwendungsmuster und erörtern die Vor- und Nachteile der einzelnen Optionen. Informationen zur Verwendung der aktiven Georeplikation mit elastischen Pools finden Sie unter [Strategien zur Notfallwiederherstellung mit elastischen Pools](sql-database-disaster-recovery-strategies-for-applications-with-elastic-pool.md).

## <a name="design-pattern-1-active-passive-deployment-for-cloud-disaster-recovery-with-a-co-located-database"></a>Entwurfsmuster 1: Aktiv-/Passiv-Bereitstellung für eine cloudbasierte Notfallwiederherstellung mit zusammengestellter Datenbank
Diese Option eignet sich am besten für Anwendungen mit den folgenden Merkmalen:

* Aktive Instanz in einer einzelnen Azure-Region
* Starke Abhängigkeit vom Lese-/ Schreibzugriff auf Daten
* Regionsübergreifende Konnektivität zwischen Anwendungslogik und Datenbank aufgrund von Latenz und Datenverkehrskosten nicht akzeptabel    

In diesem Fall wird die Topologie für die Anwendungsbereitstellung für den Umgang mit regionalen Notfällen optimiert, bei denen alle Anwendungskomponenten betroffen sind und ihr Failover als Einheit erfolgen muss. Um geografische Redundanz zu erzielen, werden Anwendungslogik und Datenbank in eine andere Region repliziert, aber unter normalen Umständen nicht für Anwendungsworkloads verwendet. Die Anwendung in der sekundären Region muss für das Verwenden einer SQL-Verbindungszeichenfolge für die sekundäre Datenbank konfiguriert werden. Traffic Manager ist für die [Failoverroutingmethode](../traffic-manager/traffic-manager-configure-failover-routing-method.md)eingerichtet.  

> [!NOTE]
> [Azure Traffic Manager](../traffic-manager/traffic-manager-overview.md) wird in diesem Artikel ausschließlich zu Demonstrationszwecken genutzt. Sie können eine beliebige Lastenausgleichslösung einsetzen, die die Failoverroutingmethode unterstützt.    
>
>

Zusätzlich zu den Hauptanwendungsinstanzen sollten Sie die Bereitstellung einer kleinen [Workerrollenanwendung](../cloud-services/cloud-services-choose-me.md#tellmecs) erwägen, die Ihre primäre Datenbank überwacht, indem sie in regelmäßigen Abständen schreibgeschützte T-SQL-Befehle ausgibt. Sie können sie verwenden, um automatisch ein Failover auszulösen, eine Warnung in der Verwaltungskonsole der Anwendung auszulösen oder beides. Um sicherzustellen, dass die Überwachung nicht von Ausfällen in der gesamten Region betroffen ist, müssen Sie zunächst die Instanzen der Überwachungsanwendung in den einzelnen Regionen bereitstellen. Anschließend müssen Sie jede Instanz mit der primären Datenbank in der primären Region und mit der sekundären Datenbank in der sekundären Region verbinden.

> [!NOTE]
> Beide Überwachungsanwendungen sollten aktiv sein und sowohl primäre als auch sekundäre Datenbanken testen. Letztere kann verwendet werden, um einen Ausfall in der sekundären Region zu erkennen und zu warnen, wenn die Anwendung nicht geschützt ist.     
>
>

Das folgende Diagramm zeigt diese Konfiguration vor einem Ausfall.

![Konfiguration der SQL-Datenbank-Georeplikation Cloudbasierte Notfallwiederherstellung](./media/sql-database-designing-cloud-solutions-for-disaster-recovery/pattern1-1.png)

Nach einem Ausfall in der primären Region erkennt die Überwachungsanwendung, dass kein Zugriff auf die primäre Datenbank möglich ist, und löst eine Warnung aus. Je nach der für Ihre Anwendung geltenden SLA können Sie bestimmen, wie viele nachfolgende Überwachungstests fehlerhaft sein dürfen, ehe ein Datenbankausfall bekanntgegeben wird. Um ein koordiniertes Failover des Anwendungsendpunkts und der Datenbank zu ermöglichen, muss die Überwachungsanwendung die folgenden Schritte ausführen:

1. [Aktualisieren des Status des primären Endpunkts](https://msdn.microsoft.com/library/hh758250.aspx) , um ein Endpunktfailover auszulösen.
2. Aufrufen der sekundären Datenbank, um ein [Datenbankfailover zu initiieren](sql-database-geo-replication-portal.md).

Nach einem Failover verarbeitet die Anwendung die Benutzeranforderungen in der sekundären Region, bleibt jedoch mit der Datenbank zusammengestellt, da die primäre Datenbank sich nun in der sekundären Region befindet. Dieses Szenario wird im folgenden Diagramm veranschaulicht. In allen Diagrammen stehen durchgezogene Linien für aktive Verbindungen, gepunktete Linien für unterbrochene Verbindungen und Stoppschilder für Aktionstrigger.

![Georeplikation: Failover auf sekundäre Datenbank App-Datensicherung](./media/sql-database-designing-cloud-solutions-for-disaster-recovery/pattern1-2.png)

Bei einem Ausfall in der sekundären Region wird der Replikationslink zwischen der primären und sekundären Datenbank unterbrochen, und die Überwachungsanwendung löst die Warnung aus, dass die primäre Datenbank ungeschützt ist. Dies hat keine Auswirkung auf die Leistung der Anwendung, sie wird jedoch ungeschützt und mit dem erhöhten Risiko betrieben, dass beide Regionen nacheinander ausfallen.

> [!NOTE]
> Wir empfehlen ausschließlich Bereitstellungskonfigurationen mit einer einzelnen Notfallwiederherstellungsregion. Der Grund ist, dass die meisten Azure-Gebiete zwei Regionen aufweisen. Diese Konfigurationen bieten Ihrer Anwendung keinen Schutz vor einem katastrophenbedingten Ausfall beider Regionen. Im unwahrscheinlichen Fall eines solchen Ausfalls können Sie Ihre Datenbanken mithilfe eines [Geowiederherstellungsvorgangs](sql-database-disaster-recovery.md#recover-using-geo-restore) in einer dritten Region wiederherstellen.
>
>

Nach Behebung der Ausfallursache wird die sekundäre Datenbank automatisch mit der primären synchronisiert. Während der Synchronisierung kann die Leistung der primären Datenbank geringfügig beeinträchtigt sein, abhängig von der Menge der Daten, die synchronisiert werden müssen. Das folgende Diagramm zeigt einen Ausfall in der sekundären Region.

![Synchronisieren der sekundären Datenbank mit der primären Datenbank Cloudbasierte Notfallwiederherstellung](./media/sql-database-designing-cloud-solutions-for-disaster-recovery/pattern1-3.png)

Die **Hauptvorteile** dieses Entwurfsmusters sind:

* Die SQL-Verbindungszeichenfolge wird während der Anwendungsbereitstellung in den einzelnen Regionen festgelegt und ändert sich nach dem Failover nicht.
* Die Leistung der Anwendung wird nicht durch das Failover beeinträchtigt, da Anwendung und Datenbank stets zusammengestellt sind.

Der **Hauptnachteil** besteht darin, dass die redundante Anwendungsinstanz in der sekundären Region nur für die Notfallwiederherstellung verwendet wird.

## <a name="design-pattern-2-active-active-deployment-for-application-load-balancing"></a>Entwurfsmuster 2: Aktiv-/Aktiv-Bereitstellung für Anwendungslastenausgleich
Diese cloudbasierte Notfallwiederherstellung eignet sich am besten für Anwendungen mit den folgenden Merkmalen:

* Hohes Verhältnis von Datenbanklesevorgängen zu -schreibvorgängen
* Latenz von Datenbankschreibvorgängen ohne Auswirkung auf Endbenutzer  
* Schreibgeschützte Logik kann von Lese-/Schreiblogik mittels einer anderen Verbindungszeichenfolge getrennt werden
* Schreibgeschützte Logik ist nicht davon abhängig, dass Daten vollständig mit den neuesten Aktualisierungen synchronisiert sind  

Wenn Ihre Anwendungen diese Merkmale aufweisen, kann ein Lastenausgleich der Endbenutzerverbindungen über mehrere Anwendungsinstanzen in verschiedenen Regionen die Leistung und Benutzerfreundlichkeit verbessern. Zum Implementieren des Lastenausgleichs muss jede Region über eine aktive Instanz der Anwendung verfügen, wobei die Lese-/Schreiblogik (RW) mit der primären Datenbank in der primären Region verbunden ist. Die schreibgeschützte Logik muss mit einer sekundären Datenbank in derselben Region wie die Anwendungsinstanz verbunden werden. Traffic Manager muss so eingerichtet werden, dass für jede Anwendungsinstanz das [Roundrobinrouting](../traffic-manager/traffic-manager-configure-round-robin-routing-method.md) oder das [leistungsorientierte Routing](../traffic-manager/traffic-manager-configure-performance-routing-method.md) mit [Endpunktüberwachung](../traffic-manager/traffic-manager-monitoring.md) aktiviert wird.

Wie bei Muster 1 sollten Sie das Bereitstellen einer ähnlichen Überwachungsanwendung erwägen. Doch im Gegensatz zu Muster 1 ist die Überwachungsanwendung nicht für das Auslösen des Endpunktfailovers zuständig.

> [!NOTE]
> Wenngleich bei diesem Muster mehr als eine sekundäre Datenbank zum Einsatz kommt, wird aus den zuvor genannten Gründen nur eine dieser sekundären Datenbanken für das Failover genutzt. Da dieses Muster schreibgeschützten Zugriff auf die sekundäre Datenbank verlangt, ist eine aktive Georeplikation erforderlich.
>
>

Traffic Manager muss für das Leistungsrouting so konfiguriert werden, dass die Benutzerverbindungen zur Anwendungsinstanz geleitet werden, die dem geografischen Standort des Benutzers am nächsten ist. Das folgende Diagramm veranschaulicht diese Konfiguration vor einem Ausfall.
![Kein Ausfall: leistungsorientiertes Routing zur nächstgelegenen Anwendung. Georeplikation.](./media/sql-database-designing-cloud-solutions-for-disaster-recovery/pattern2-1.png)

Wenn in der primären Region ein Datenbankausfall erkannt wird, lösen Sie das Failover der primären Datenbank in eine der sekundären Regionen aus, wodurch sich der Standort der primären Datenbank ändert. Traffic Manager schließt automatisch den Endpunkt im Offlinestatus aus der Routingtabelle aus, setzt aber das Routing des Endbenutzerdatenverkehrs an die verbleibenden Online-Instanzen fort. Da sich die primäre Datenbank nun in einer anderen Region befindet, muss bei allen Online-Instanzen die SQL-Verbindungszeichenfolge für den Lese-/Schreibzugriff so geändert werden, dass eine Verbindung mit der neuen primären Datenbank hergestellt wird. Wichtig ist, dass Sie diese Änderung vor dem Auslösen des Datenbankfailovers vornehmen. Schreibgeschützte SQL-Verbindungszeichenfolgen müssen unverändert bleiben, da sie stets auf die Datenbank in derselben Region zeigen. Die Failoverschritte sind wie folgt:  

1. Ändern der SQL-Verbindungszeichenfolgen für den Lese-/ Schreibzugriff dergestalt, dass sie auf die neue primäre Datenbank zeigen
2. Aufrufen der vorgesehenen sekundären Datenbank zum [Auslösen des Datenbankfailovers](sql-database-geo-replication-portal.md)

Das folgende Diagramm veranschaulicht die neue Konfiguration nach dem Failover.
![Konfiguration nach Failover. Cloudbasierte Notfallwiederherstellung.](./media/sql-database-designing-cloud-solutions-for-disaster-recovery/pattern2-2.png)

Bei einem Ausfall in einer der sekundären Regionen entfernt Traffic Manager automatisch den Endpunkt im Offlinestatus in dieser Region aus der Routingtabelle. Der Replikationskanal zur sekundären Datenbank in dieser Region wird unterbrochen. Da in diesem Szenario die übrigen Regionen zusätzlichen Benutzerdatenverkehr verarbeiten müssen, ist die Leistung der Anwendung während des Ausfalls beeinträchtigt. Nach Behebung der Ausfallursache wird die sekundäre Datenbank in der betroffenen Region sofort mit der primären Datenbank synchronisiert. Während der Synchronisierung kann die Leistung der primären Datenbank geringfügig beeinträchtigt werden, was von der Menge der Daten abhängt, die synchronisiert werden muss. Das folgende Diagramm zeigt einen Ausfall in einer der sekundären Regionen.

![Ausfall in sekundärer Region Cloudbasierte Notfallwiederherstellung – Georeplikation](./media/sql-database-designing-cloud-solutions-for-disaster-recovery/pattern2-3.png)

Der **Hauptvorteil** dieses Entwurfsmusters ist, dass Sie die Anwendungsworkload auf mehrere sekundäre Datenbanken verteilen können, um eine optimale Leistung für die Endbenutzer zu erzielen. Diese Option hat folgende **Nachteile** :

* Lese-/Schreibzugriffsverbindungen zwischen Anwendungsinstanzen und Datenbank haben unterschiedliche Wartezeiten und Kosten.
* Die Anwendungsleistung ist während des Ausfalls beeinträchtigt.
* Anwendungsinstanzen müssen die SQL-Verbindungszeichenfolge nach einem Datenbankfailover dynamisch ändern.  

> [!NOTE]
> Ein ähnlicher Ansatz kann verwendet werden, um spezielle Workloads wie Berichtsaufträge, Business Intelligence-Tools oder Sicherungen auszulagern. Diese Workloads nutzen in der Regel umfangreiche Datenbankressourcen. Deshalb wird empfohlen, dass Sie ihnen eine der sekundären Datenbanken mit der Leistungsstufe zuweisen, die der erwarteten Workload entspricht.
>
>

## <a name="design-pattern-3-active-passive-deployment-for-data-preservation"></a>Entwurfsmuster 3: Aktiv-/Passiv-Bereitstellung für die Beibehaltung von Daten
Diese Option eignet sich am besten für Anwendungen mit den folgenden Merkmalen:

* Jeder Datenverlust stellt ein hohes Geschäftsrisiko dar. Das Datenbankfailover darf nur als letzter Ausweg in Frage kommen, sollte der Ausfall dauerhaft sein.
* Die Anwendung kann für bestimmte Zeit im „schreibgeschützten Modus“ betrieben werden.

Bei diesem Muster schaltet die Anwendung in den schreibgeschützten Modus um, sobald die Verbindung mit der sekundären Datenbank hergestellt wurde. Die Anwendungslogik in der primären Region befindet sich am gleichen Standort wie die primäre Datenbank und arbeitet im Lese-/Schreibmodus (RW). Die Anwendungslogik in der sekundären Region befindet sich am gleichen Standort wie die sekundäre Datenbank und ist auf den Betrieb im schreibgeschützten Modus (RO) vorbereitet.  Traffic Manager muss so eingerichtet werden, dass für beide Anwendungsinstanzen das [Failoverrouting](../traffic-manager/traffic-manager-configure-failover-routing-method.md) mit [Endpunktüberwachung](../traffic-manager/traffic-manager-monitoring.md) aktiviert wird.

Das folgende Diagramm veranschaulicht diese Konfiguration vor einem Ausfall.
![Aktiv/Passiv-Bereitstellung vor Failover. Cloudbasierte Notfallwiederherstellung.](./media/sql-database-designing-cloud-solutions-for-disaster-recovery/pattern3-1.png)

Wenn Traffic Manager einen Ausfall der Konnektivität in der primären Region erkennt, wird der Datenverkehr der Benutzer automatisch zur Anwendungsinstanz in der sekundären Region umgeleitet. Bei diesem Muster dürfen Sie **kein** Datenbankfailover einleiten, nachdem der Ausfall erkannt wurde. Die Anwendung in der sekundären Region wird aktiviert und im schreibgeschützten Modus unter Verwendung der sekundären Datenbank betrieben. Dies wird im folgenden Diagramm veranschaulicht.

![Ausfall: Anwendung im schreibgeschützten Modus Cloudbasierte Notfallwiederherstellung](./media/sql-database-designing-cloud-solutions-for-disaster-recovery/pattern3-2.png)

Nachdem die Ausfallursache in der primären Region beseitigt wurde, erkennt Traffic Manager die Wiederherstellung der Konnektivität in der primären Region und leitet den Datenverkehr der Benutzer wieder zur Anwendungsinstanz in der primären Region. Diese Anwendungsinstanz nimmt wieder den Betrieb im Lese-/Schreibmodus unter Verwendung der primären Datenbank auf.

> [!NOTE]
> Da dieses Muster einen schreibgeschützten Zugriff auf die sekundäre Datenbank verlangt, ist eine aktive Georeplikation erforderlich.
>
>

Bei einem Ausfall in der sekundären Region markiert Traffic Manager den Anwendungsendpunkt in der primären Region als beeinträchtigt, und der Replikationskanal wird unterbrochen. Dieser Ausfall wirkt sich jedoch nicht auf die Leistung der Anwendung aus. Nach Behebung der Ausfallursache wird die sekundäre Datenbank sofort mit der primären synchronisiert. Während der Synchronisierung kann die Leistung der primären Datenbank geringfügig beeinträchtigt werden, was von der Menge der Daten abhängt, die synchronisiert werden muss.

![Ausfall: sekundäre Datenbank  Cloudbasierte Notfallwiederherstellung](./media/sql-database-designing-cloud-solutions-for-disaster-recovery/pattern3-3.png)

Dieses Entwurfsmuster bietet mehrere **Vorteile**:

* Datenverluste während temporärer Ausfälle werden verhindert.
* Es muss keine Überwachungsanwendung bereitgestellt werden, da die Wiederherstellung von Traffic Manager ausgelöst wird.
* Ausfallzeiten hängen nur davon ab, wie schnell Traffic Manager den Verbindungsausfall erkennt, was konfigurierbar ist.

Die **Nachteile** sind:

* Die Anwendung muss im schreibgeschützten Modus betrieben werden können.
* Ein dynamisches Umschalten zu ihr ist erforderlich, wenn sie mit einer schreibgeschützten Datenbank verbunden ist.
* Die Fortsetzung von Lese-/Schreibvorgängen erfordert die Wiederherstellung der primären Region, was bedeuten kann, dass der vollständige Datenzugriff stunden- oder sogar tagelang deaktiviert ist.

> [!NOTE]
> Bei einem dauerhaften Dienstausfall in der Region müssen Sie das Datenbankfailover manuell aktivieren und den Datenverlust akzeptieren. Die Anwendung ist in der sekundären Region mit Lese-/ Schreibzugriff auf die Datenbank betriebsbereit.
>
>

## <a name="business-continuity-planning-choose-an-application-design-for-cloud-disaster-recovery"></a>Planen der Geschäftskontinuität: Auswählen eines Anwendungsentwurfs für die cloudbasierte Notfallwiederherstellung
Für Ihre spezifische Strategie einer cloudbasierten Notfallwiederherstellung können diese Muster kombiniert oder erweitert werden, um die Anforderungen Ihrer Anwendung bestmöglich zu erfüllen.  Wie bereits erwähnt, basiert die von Ihnen gewählte Strategie auf der SLA, die Sie Ihren Kunden anbieten möchten, und auf der Topologie der Anwendungsbereitstellung. Um Ihnen die Entscheidung zu erleichtern, werden in der folgenden Tabelle die Optionen basierend auf dem geschätzten Datenverlust bzw. auf der RPO (Recovery Point Objective) und der geschätzten Wiederherstellungszeit verglichen.

| Muster | RPO | Geschätzte Wiederherstellungszeit |
|:--- |:--- |:--- |
| Aktiv-/Passiv-Bereitstellung für Notfallwiederherstellung mit zusammengestelltem Datenbankzugriff |Lese-/Schreibzugriff < 5 Sek. |Ausfallerkennungszeit + Aufruf der Failover-API + Anwendungsüberprüfungstest |
| Aktiv-/Aktiv-Bereitstellung für den Anwendungslastenausgleich |Lese-/Schreibzugriff < 5 Sek. |Ausfallerkennungszeit + Aufruf der Failover-API + Änderung der SQL-Verbindungszeichenfolge + Anwendungsüberprüfungstest |
| Aktiv-/Passiv-Bereitstellung für die Beibehaltung von Daten |Schreibgeschützter Zugriff < 5 Sek. Lese-/ Schreibzugriff = 0 (null) |Schreibgeschützter Zugriff = Erkennungszeit für Verbindungsausfall + Anwendungsüberprüfungstest  <br>Lese-/Schreibzugriff = Zeit zum Beseitigen der Ausfallursache |

## <a name="next-steps"></a>Nächste Schritte
* Informationen über automatisierte Sicherungen von Azure SQL-Datenbanken finden Sie unter [Übersicht: Automatisierte SQL-Datenbanksicherungen](sql-database-automated-backups.md)
* Eine Übersicht und verschiedene Szenarien zum Thema Geschäftskontinuität finden Sie unter [Übersicht über die Geschäftskontinuität](sql-database-business-continuity.md)
* Informationen zum Verwenden automatisierter Sicherungen für die Wiederherstellung finden Sie unter [Wiederherstellen einer Datenbank aus vom Dienst initiierten Sicherungen](sql-database-recovery-using-backups.md)
* Informationen über schnellere Wiederherstellungsoptionen finden Sie unter [Aktive Georeplikation](sql-database-geo-replication-overview.md)  
* Informationen zum Verwenden automatisierter Sicherungen für die Archivierung finden Sie unter [Datenbankkopie](sql-database-copy.md)
* Informationen zur Verwendung der aktiven Georeplikation mit elastischen Pools finden Sie unter [Strategien zur Notfallwiederherstellung mit elastischen Pools](sql-database-disaster-recovery-strategies-for-applications-with-elastic-pool.md).



<!--HONumber=Jan17_HO4-->


