---
title: "Failovergruppen und aktive Georeplikation – Azure SQL-Datenbank | Microsoft-Dokumentation"
description: "Gruppen für automatisches Failover mit aktiver Georeplikation ermöglichen Ihnen die Einrichtung von vier Replikaten Ihrer Datenbank in einem beliebigen Azure-Rechenzentrum und automatisches Failover bei einem Ausfall."
services: sql-database
documentationcenter: na
author: anosov1960
manager: jhubbard
editor: monicar
ms.assetid: 2a29f657-82fb-4283-9a83-e14a144bfd93
ms.service: sql-database
ms.custom: business continuity
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: NA
ms.date: 09/26/2016
ms.author: sashan
ms.translationtype: Human Translation
ms.sourcegitcommit: 95b8c100246815f72570d898b4a5555e6196a1a0
ms.openlocfilehash: 867cc69e18e5b31f707c1942e7aa1b691403e3e0
ms.contentlocale: de-de
ms.lasthandoff: 05/18/2017


---
# <a name="overview-failover-groups-and-active-geo-replication"></a>Übersicht: Failovergruppen und aktive Georeplikation
Mit der aktiven Georeplikation können Sie bis zu vier lesbare sekundäre Datenbanken am gleichen oder an verschiedenen Standorten von Rechenzentren (Regionen) konfigurieren. Sekundäre Datenbanken stehen für Abfragen und Failover zur Verfügung, wenn ein Rechenzentrum ausgefallen ist oder keine Verbindung mit der primären Datenbank möglich ist. Das Failover muss durch die Anwendung des Benutzers manuell eingeleitet werden. Nach einem Failover hat die neue primäre Datenbank einen anderen Verbindungsendpunkt. 

> [!NOTE]
> Die aktive Georeplikation ist jetzt für alle Datenbanken in allen Diensttarifen und allen Regionen verfügbar.
>  

Gruppen für automatisches Failover in Azure SQL-Datenbank (in der Vorschau) sind ein SQL-Datenbank-Feature für die Verwaltung von Georeplikationsbeziehungen, Konnektivität und skaliertes Failover. Damit erhalten Kunden die Möglichkeit, nach schwerwiegenden regionalen Ausfällen oder anderen ungeplanten Ereignissen, die zum vollständigen oder teilweisen Verlust der Verfügbarkeit der Dienste von SQL-Datenbank in der primären Region führen, automatisch mehrere verwandte Datenbanken in der sekundären Region wiederherzustellen. Außerdem können sie die lesbaren sekundären Datenbanken zur Auslagerung schreibgeschützter Workloads verwenden.  Da Gruppen für automatisches Failover mehrere Datenbanken beinhalten, müssen sie auf dem primären Server konfiguriert werden. Primärer und sekundärer Server müssen zu dem gleichen Abonnement gehören. Gruppen für automatisches Failover unterstützen die Replikation aller Datenbanken in der Gruppe auf nur einen sekundären Server in einer anderen Region. Die aktive Georeplikation ohne Gruppen für automatisches Failover erlaubt bis zu 4 sekundäre Server in beliebigen Regionen.

Falls Sie die aktive Georeplikation verwenden und Ihre primäre Datenbank aus beliebigem Grund ausfällt oder einfach offline geschaltet werden muss, können Sie ein Failover auf eine der sekundären Datenbanken initiieren. Wenn das Failover auf eine sekundäre Datenbank aktiviert ist, werden alle anderen sekundären Datenbanken automatisch mit der neuen primären Datenbank verknüpft. Wenn Sie Gruppen für automatisches Failover (in der Vorschau) zum Verwalten der Datenbankwiederherstellung verwenden, führt jeder Ausfall, der eine oder mehrere der Datenbanken in der Gruppe betrifft, zu einem automatischen Failover. Sie können eine Richtlinie für automatisches Failover konfigurieren, die Ihren Anwendungsanforderungen am besten entspricht, oder Sie können stattdessen die manuelle Aktivierung verwenden. Darüber hinaus bieten Gruppen für automatisches Failover (in der Vorschau) Lese-/Schreib-Listenerendpunkte, die während eines Failovers unverändert bleiben. Sowohl bei manueller als auch automatischer Failoveraktivierung schaltet das Failover alle sekundären Datenbanken in der Gruppe zu primären um. Nach Abschluss des Datenbankfailovers wird der DNS-Eintrag automatisch aktualisiert, um die Endpunkte in die neue Region umzuleiten.

Sie können Replikation und Failover für eine einzelne Datenbank oder eine Gruppe von Datenbanken auf einem Server oder in einem Pool für elastische Datenbanken mithilfe der aktiven Georeplikation verwalten. Sie können hierzu das [Azure-Portal](sql-database-geo-replication-portal.md), [PowerShell](sql-database-geo-replication-powershell.md), [Transact-SQL](sql-database-geo-replication-transact-sql.md) oder die [REST-API](https://msdn.microsoft.com/library/azure/mt163685.aspx) verwenden. Stellen Sie nach dem Failover sicher, dass die Authentifizierungsanforderungen für Ihren Server und Ihre Datenbank auf der neuen primären konfiguriert sind. Weitere Informationen finden Sie unter [Verwalten der Sicherheit der Azure SQL-Datenbank nach der Notfallwiederherstellung](sql-database-geo-replication-security-config.md). Dies gilt für aktive Georeplikation und Gruppen für automatisches Failover (in der Vorschau) gleichermaßen.

Aktive Georeplikation nutzt die [Always On](https://docs.microsoft.com/sql/database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server)-Technologie von SQL Server, um Transaktionen mit ausgeführtem Commit in der primären Datenbank asynchron mit Read Commited-Momentaufnahmeisolation (Read Committed Snapshot Isolation, RCSI) in eine sekundäre Datenbank zu replizieren. Gruppen für automatisches Failover stellen die Gruppensemantik über der aktiven Georeplikation bereit, es wird aber der gleiche asynchrone Replikationsmechanismus verwendet. Wenngleich die sekundäre Datenbank stets ein wenig hinter der primären Datenbank zurückliegt, sind unvollständige Transaktionen bei sekundären Daten garantiert ausgeschlossen. Regionsübergreifende Redundanz ermöglicht Anwendungen die schnelle Wiederherstellung nach einem dauerhaften Ausfall eines gesamten Rechenzentrums oder von Teilen eines Rechenzentrums aufgrund von Naturkatastrophen, schwerwiegendem menschlichen Versagen oder böswilligen Handlungen. Die spezifischen RPO-Daten finden Sie unter [Übersicht über die Geschäftskontinuität mit Azure SQL-Datenbank](sql-database-business-continuity.md).

Die folgende Abbildung zeigt ein Beispiel für die Konfiguration der aktiven Georeplikation, wobei sich die primäre Datenbank in der Region „USA, Norden-Mitte“ und die sekundäre Datenbank in der Region „USA, Süden-Mitte“ befindet.

![Georeplikationsbeziehung](./media/sql-database-active-geo-replication/geo-replication-relationship.png)

Weil die sekundären Datenbanken lesbar sind, können sie zum Auslagern schreibgeschützter Workloads wie Berichtsaufträge verwendet werden. Bei Verwendung der aktiven Georeplikation können Sie die sekundäre Datenbank in der gleichen Region wie die primäre erstellen, aber dies steigert nicht die Stabilität der Anwendung gegenüber schwerwiegenden Ausfällen. Wenn Sie Gruppen für automatisches Failover (in der Vorschau) verwenden, werden die sekundären Datenbanken immer in einer anderen Region erstellt.

Zusätzlich zur Wiederherstellung im Notfall kann aktive Georeplikation in den folgenden Szenarien verwendet werden:

* **Datenbankmigration:** Sie können die aktive Georeplikation zur Onlinemigration einer Datenbank von einem Server auf einen anderen mit minimalen Ausfallzeiten nutzen.
* **Anwendungsupgrades**: Sie können bei Anwendungsupgrades eine zusätzliche sekundäre Datenbank als Failbackkopie erstellen.

Wenn Sie echte Geschäftskontinuität erreichen möchten, ist das Bereitstellen von Datenbankredundanz zwischen Rechenzentren jedoch nur ein Teil der Lösung. Für die komplette Wiederherstellung einer Anwendung bzw. eines Diensts nach einem schwerwiegenden Fehler ist das Wiederherstellen aller Komponenten erforderlich, aus denen sich der Dienst und alle abhängigen Dienste zusammensetzen. Beispiele dieser Komponenten sind die Clientsoftware (z. B. ein Browser mit benutzerdefiniertem JavaScript), Web-Front-Ends, Speicher und DNS. Es ist wichtig, dass alle Komponenten hinsichtlich derselben Fehler gegen Ausfälle geschützt und innerhalb des RTO (Recovery Time Objective) der Anwendung wieder verfügbar sind. Daher müssen Sie alle abhängigen Dienste bestimmen und mit dem Leistungsumfang und den Funktionen vertraut sein, die sie bieten. Dann müssen Sie entsprechende Maßnahmen ergreifen, um sicherzustellen, dass Ihr Dienst während des Failovers der Dienste funktioniert, von denen er abhängig ist. Weitere Informationen zum Entwerfen von Lösungen für die Notfallwiederherstellung finden Sie unter [Entwerfen von Cloudlösungen für die Notfallwiederherstellung mithilfe der aktiven Georeplikation](sql-database-designing-cloud-solutions-for-disaster-recovery.md).

## <a name="active-geo-replication-capabilities"></a>Möglichkeiten der aktiven Georeplikation
Das Feature der aktiven Georeplikation bietet die folgenden wichtigen Möglichkeiten:

* **Automatische asynchrone Replikation**: Sie können eine sekundäre Datenbank nur durch Hinzufügen zu einer vorhandenen Datenbank erstellen. Die sekundäre Datenbank kann auf einem anderen Azure SQL-Datenbank-Server erstellt werden. Nach der Erstellung wird die sekundäre Datenbank mit den Daten aufgefüllt, die aus der primären Datenbank kopiert wurden. Dieser Prozess wird als Seeding bezeichnet. Nachdem eine sekundäre Datenbank erstellt wurde und das Seeding erfolgt ist, werden Aktualisierungen der primären Datenbank automatisch asynchron in die sekundäre Datenbank repliziert. Asynchrone Replikation bedeutet, dass für Transaktionen in der primären Datenbank ein Commit erfolgt, ehe sie in die sekundäre Datenbank repliziert werden. 
* **Lesbare sekundäre Datenbanken:** Eine Anwendung kann für schreibgeschützte Vorgänge auf eine sekundäre Datenbank zugreifen, indem sie die Sicherheitsprinzipale für den Zugriff auf die primäre Datenbank oder andere Sicherheitsprinzipale verwendet. Die sekundären Datenbanken werden im Momentaufnahme-Isolationsmodus betrieben. Auf diese Weise wird sichergestellt, dass die Replikation von Aktualisierungen der primären Datenbank (Protokollwiedergabe) nicht durch Abfragen verzögert wird, die für die sekundäre Datenbank ausgeführt werden.

   > [!NOTE]
   > Die Protokollwiedergabe wird in der sekundären Datenbank verzögert, wenn sie Schemaaktualisierungen von der primären Datenbank erhält, die eine Schemasperre für die sekundäre Datenbank erfordern. 
   > 

* **Mehrere lesbare sekundäre Datenbanken**: Mehrere sekundäre Datenbanken sorgen für mehr Redundanz und einen besseren Schutz der primären Datenbank und Anwendung. Wenn mehrere sekundäre Datenbanken vorhanden sind, bleibt die Anwendung auch bei Ausfall einer der sekundären Datenbanken geschützt. Wenn es nur eine sekundäre Datenbank gibt und diese ausfällt, ist die Anwendung bis zum Erstellen einer neuen sekundären Datenbank einem höheren Risiko ausgesetzt.

   > [!NOTE]
   > Wenn Sie mit der aktiven Georeplikation eine global verteilte Anwendung erstellen und schreibgeschützten Zugriff auf Daten in mehr als vier Regionen bereitstellen müssen, können Sie eine sekundäre Datenbank einer sekundären Datenbank erstellen (dieser Prozess wird als Verkettung bezeichnet). Auf diese Weise können Sie die Datenbankreplikation praktisch unbegrenzt skalieren. Darüber hinaus verkürzt die Verkettung den Mehraufwand der Replikation von der primären Datenbank. Der Nachteil besteht in der erhöhten Replikationsverzögerung in den äußersten sekundären Datenbanken. verfügbar. 
   >

* **Unterstützung von Datenbanken in elastischen Pools:** Die aktive Georeplikation kann für jede Datenbank in einem beliebigen Pool für elastische Datenbanken konfiguriert werden. Die sekundäre Datenbank kann sich in einem anderen elastischen Pool befinden. Für normale Datenbanken kann die sekundäre Datenbank ein elastischer Pool sein und umgekehrt, sofern die Dienstebenen identisch sind. 
* **Konfigurierbare Leistungsstufe der sekundären Datenbank**: Eine sekundäre Datenbank kann mit einer niedrigeren Leistungsstufe erstellt werden als die primäre Datenbank. Sowohl die primäre als auch die sekundäre Datenbank müssen die gleiche Dienstebene aufweisen. Diese Option wird nicht für Anwendungen mit Datenbanken empfohlen, die eine hohe Schreibaktivität aufweisen, da die größere Replikationsverzögerung das Risiko erheblicher Datenverluste nach einem Failover steigert. Darüber hinaus wird nach einem Failover die Leistung der Anwendung beeinträchtigt, bis die neue primäre Datenbank auf eine höhere Leistungsstufe aktualisiert ist. Das Diagramm mit dem Protokoll-E/A-Prozentsatz im Azure-Portal bietet eine gute Möglichkeit, einzuschätzen, welche Leistungsstufe für die sekundäre Datenbank mindestens erforderlich ist, um die Replikationslast zu bewältigen. Wenn die Leistungsstufe der primären Datenbank beispielsweise P6 (1.000 DTU) ist und ihr Protokoll-E/A-Prozentsatz 50 % beträgt, muss die Leistungsstufe der sekundären Datenbank mindestens P4 (500 DTU) sein. Sie können die Protokoll-E/A-Daten auch mithilfe der Datenbanksicht [sys.resource_stats](https://msdn.microsoft.com/library/dn269979.aspx) oder [sys.dm_db_resource_stats](https://msdn.microsoft.com/library/dn800981.aspx) abrufen.  Weitere Informationen zu den SQL-Datenbankleistungsstufen finden Sie unter [SQL-Datenbankoptionen und -leistung](sql-database-service-tiers.md). 
* **Benutzergesteuertes Failover und Failback:**Eine sekundäre Datenbank kann jederzeit durch die Anwendung oder den Benutzer explizit die primäre Rolle erhalten. Bei einem echten Ausfall sollte die Option „Nicht geplant“ verwendet werden, die eine sekundäre Datenbank umgehend zu einer primären Datenbank heraufstuft. Wenn die ausgefallene primäre Datenbank wiederhergestellt ist und wieder zur Verfügung steht, kennzeichnet das System diese wiederhergestellte primäre Datenbank automatisch als sekundäre Datenbank und bringt sie auf den aktuellen Stand der neuen primären Datenbank. Aufgrund der asynchronen Natur der Replikation kann eine kleine Menge von Daten bei einem nicht geplanten Failover verloren gehen, wenn eine primäre Datenbank ausfällt, bevor die aktuellen Änderungen in die sekundäre Datenbank repliziert wurden. Wenn für eine primäre Datenbank mit mehreren sekundären Datenbanken ein Failover durchgeführt wird, konfiguriert das System automatisch die Replikationsbeziehungen neu und verknüpft die verbleibenden sekundären Datenbanken mit der soeben heraufgestuften primären Datenbank, ohne dass ein Benutzereingriff erforderlich ist. Wenn der Ausfall behoben ist, durch den das Failover verursacht wurde, kann es wünschenswert sein, die Anwendung wieder in die primäre Region zurückzuführen. Zu diesem Zweck sollte der Failoverbefehl mit der Option „Geplant“ aufgerufen werden. 
* **Synchronisieren von Anmeldeinformationen und Firewallregeln:** Wir empfehlen die Verwendung von [Datenbank-Firewallregeln](sql-database-firewall-configure.md) für georeplizierte Datenbanken, damit diese Regeln mit der Datenbank repliziert werden können. So wird sichergestellt, dass alle sekundären Datenbanken die gleichen Firewallregeln besitzen wie die primäre Datenbank. Mit diesem Ansatz müssen Kunden auf Servern, auf denen sowohl die primäre als auch die sekundäre Datenbank gehostet wird, keine Firewallregeln mehr manuell konfigurieren und verwalten. Analog dazu wird durch die Verwendung von [eigenständigen Datenbankbenutzern](sql-database-manage-logins.md) für den Datenzugriff sichergestellt, dass für primäre und sekundäre Datenbanken immer die gleichen Benutzeranmeldeinformationen gelten, damit bei einem Failover keine Unterbrechungen durch Benutzernamen- und Kennwortkonflikte auftreten. Durch Hinzufügen von [Azure Active Directory](../active-directory/active-directory-whatis.md) können Kunden den Benutzerzugriff sowohl für primäre als auch für sekundäre Datenbanken verwalten, sodass die Notwendigkeit der Verwaltung von Anmeldeinformationen in Datenbanken vollständig entfällt.

## <a name="auto-failover-group-capabilities"></a>Funktionen von Gruppen für automatisches Failover

Gruppen für automatisches Failover bieten eine leistungsfähige Abstraktion der aktiven Georeplikation durch Unterstützung der Replikation auf Gruppenebene und des automatischen Failovers. Darüber hinaus entfällt damit die Notwendigkeit, die SQL-Verbindungszeichenfolge nach dem Failover zu ändern, indem die zusätzlichen Listenerendpunkte bereitgestellt werden. 

* **Failovergruppe**: Eine oder mehrere Failovergruppen können zwischen zwei Servern in verschiedenen Regionen erstellt werden (primäre und sekundäre Server). Jede Gruppe kann eine oder mehrere Datenbanken enthalten, die als Einheit wiederhergestellt werden, falls alle oder einige primäre Datenbanken aufgrund eines Ausfalls in der primären Region nicht mehr verfügbar sind.  
* **Primärserver:** ein Server, auf dem die primären Datenbanken in der Failovergruppe gehostet werden.
* **Sekundärserver:** ein Server, auf dem die sekundären Datenbanken in der Failovergruppe gehostet werden. Der Sekundärserver kann sich nicht in der gleichen Region wie der primäre Server befinden.
* **Hinzufügen von Datenbanken zu einer Failovergruppe**: Sie können mehrere Datenbanken in einem Server oder in einem elastischen Pool in die gleiche Failovergruppe einfügen. Wenn Sie der Gruppe eine eigenständige Datenbank hinzufügen, wird automatisch eine sekundäre Datenbank mit der gleichen Edition und Leistungsstufe erstellt. Wenn die primäre Datenbank sich in einem elastischen Pool befindet, wird die sekundäre Datenbank automatisch im elastischen Pool gleichen Namens erstellt. Wenn Sie eine Datenbank hinzufügen, die bereits auf dem sekundären Server eine sekundäre Datenbank hat, erbt die Gruppe diese Georeplikation.

   > [!NOTE]
   > Wenn Sie eine Datenbank hinzufügen, die bereits eine sekundäre Datenbank auf einem Server hat, der nicht Teil der Failovergruppe ist, wird eine neue sekundäre Datenbank auf dem sekundären Server erstellt. 
   >

* **Lese-/Schreib-Failovergruppenlistener**: Ein DNS CNAME-Eintrag, der auf die URL des aktuellen primären Servers verweist. Er ermöglicht Lese-/Schreib-SQL-Anwendungen das transparente erneute Herstellen einer Verbindung mit der primären Datenbank, wenn die primäre Datenbank nach einem Failover geändert wird. 
* **Schreibgeschützter Failovergruppenlistener**: Ein DNS CNAME-Eintrag, der auf die URL des sekundären Servers verweist. Er ermöglicht den schreibgeschützten SQL-Anwendungen das transparente Herstellen einer Verbindung mit der sekundären Datenbank unter Verwendung der angegebenen Regeln für den Lastenausgleich. Optional können Sie angeben, ob der schreibgeschützte Datenverkehr automatisch zum primären Server umgeleitet wird, wenn der sekundäre Server nicht verfügbar ist.
* **Richtlinie für automatisches Failover**: Standardmäßig wird die Failovergruppe mit einer Richtlinie für automatisches Failover konfiguriert. Das System löst das Failover aus, sobald der Fehler erkannt wird. Wenn Sie den Failoverworkflow aus der Anwendung steuern möchten, können Sie automatisches Failover deaktivieren. 
* **Manuelles Failover**: Sie können ein Failover zu einem beliebigen Zeitpunkt unabhängig von der Konfiguration für automatisches Failover manuell initiieren. Wenn die Richtlinie für automatisches Failover nicht konfiguriert ist, müssen die Datenbanken in der Failovergruppe mit manuellem Failover wiederhergestellt werden. Sie können ein erzwungenes oder freundliches Failover (mit vollständiger Datensynchronisierung) initiieren. Mit letzterem könnten aktive Server in die primäre Region verschoben werden. Nach Abschluss des Failovers werden die DNS-Einträge automatisch aktualisiert, um die Konnektivität mit dem richtigen Server sicherzustellen.
* **Toleranzperiode mit Datenverlust**: Da die primären und sekundären Datenbanken mithilfe der asynchronen Replikation synchronisiert werden, kann das Failover zu Datenverlust führen. Sie können die Richtlinie für automatisches Failover entsprechend der Toleranz Ihrer Anwendung gegenüber Datenverlust anpassen. Durch Konfigurieren von **GracePeriodWithDataLossHours** können Sie steuern, wie lange das System wartet, bevor das Failover initiiert wird, das wahrscheinlich zu Datenverlust führt. 

   > [!NOTE]
   > Wenn das System erkennt, dass die Datenbanken in der Gruppe noch immer online sind (etwa wenn der Ausfall nur die Dienststeuerungsebene betroffen hat), aktiviert es sofort unabhängig von dem durch **GracePeriodWithDataLossHours** festgelegten Wert das Failover mit vollständiger Datensynchronisierung (freundliches Failover). Dadurch wird sichergestellt, dass es während der Wiederherstellung nicht zu Datenverlusten kommt. Die Toleranzperiode ist nur wirksam, wenn kein freundliches Failover möglich ist. Wenn der Ausfall vor Ablauf der Toleranzperiode behoben wird, wird das Failover nicht aktiviert.
   >

* **Mehrere Failovergruppen**: Sie können mehrere Failovergruppen für das gleiche Serverpaar konfigurieren, um die Skalierung der Failover zu steuern. Jede Gruppe führt ein unabhängiges Failover durch. Wenn Ihre mehrinstanzenfähige Anwendung elastische Pools verwendet, können Sie diese Funktion zum Mischen primärer und sekundärer Datenbanken in den einzelnen Pools verwenden. Auf diese Weise können Sie die Auswirkungen eines Ausfalls auf nur die Hälfte der Mandanten reduzieren.

## <a name="best-practices-of-building-highly-available-service"></a>Best Practices zum Erstellen von hoch verfügbaren Diensten

Zum Erstellen eines hoch verfügbaren Diensts, der Azure SQL-Datenbank verwendet, sollten Kunden die folgenden Richtlinien beachten:
- **Failovergruppe**: Eine oder mehrere Failovergruppen können zwischen zwei Servern in verschiedenen Regionen erstellt werden (primäre und sekundäre Server). Jede Gruppe kann eine oder mehrere Datenbanken enthalten, die als Einheit wiederhergestellt werden, falls alle oder einige primäre Datenbanken aufgrund eines Ausfalls in der primären Region nicht mehr verfügbar sind.
- **Verwenden von Gruppen mit automatischem Failover:** Hierbei wird eine geosekundäre Datenbank mit dem gleichen Dienstziel wie bei der primären Datenbank erstellt. Wenn Sie der Failovergruppe eine vorhandene Georeplikationsbeziehung hinzufügen, stellen Sie sicher, dass die geosekundäre Datenbank mit dem gleichen Servicelevelziel wie die primäre Datenbank konfiguriert ist.
- **Verwenden von ApplicationIntent = ReadOnly:** Wenn Sie die schreibgeschützten Teile der Workload verwenden, die gegenüber einer gewissen Überalterung der Daten tolerant ist, geben Sie in der Verbindungszeichenfolge die Leseabsicht an, indem Sie **ApplicationIntent = ReadOnly** verwenden. Die Verbindung wird dann automatisch an die sekundäre Datenbank weitergeleitet. 
- Wenn ein Ausfall erkannt wird, löst SQL automatisch ein Lese-/Schreib-Failover aus, sofern nach unserem Erkenntnissen keine Daten verloren gegangen sind. Andernfalls wartet es für den mit **GracePeriodWithDataLossHours** festgelegten Zeitraum. 
- **Vorbereitung auf die Beeinträchtigung der Leistung:** Die SQL-Failoverentscheidung ist unabhängig vom Rest der Anwendung oder der anderen verwendeten Dienste. Die Anwendung kann mit einigen Komponenten in einer Region und anderen in einer anderen „vermischt“ werden. Um die Beeinträchtigung zu vermeiden, stellen Sie redundante Anwendungsbereitstellung in der DR-Region sicher, und befolgen Sie die Richtlinien in diesem Artikel. Beachten Sie, dass die Anwendung in der DR-Region nicht unbedingt in eine andere Verbindungszeichenfolge verwenden muss. 
- **Vorbereiten auf Datenverluste:** Wenn Sie **GracePeriodWithDataLossHours** angegeben haben, müssen Sie auf Datenverluste vorbereitet sein. Bei Ausfällen priorisiert Azure grundsätzlich die Verfügbarkeit. Wenn keine Datenverluste auftreten dürfen, legen Sie **GracePeriodWithDataLossHours** auf eine ausreichend große Zahl fest, z.B. 24 Stunden. 


## <a name="upgrading-or-downgrading-a-primary-database"></a>Upgrade oder Downgrade einer primären Datenbank
Sie können für eine primäre Datenbank ein Upgrade oder Downgrade auf eine andere Leistungsstufe (im gleichen Diensttarif) ausführen, ohne die Verbindung mit sekundären Datenbanken zu trennen. Bei einem Upgrade wird empfohlen, zuerst das Upgrade für die sekundäre Datenbank und anschließend das Upgrade für die primäre Datenbank auszuführen. Drehen Sie bei einem Downgrade die Reihenfolge um: Führen Sie zuerst das Downgrade für die primäre und anschließend das Downgrade für die sekundäre Datenbank aus. Wenn Sie ein Upgrade oder Downgrade der Datenbank auf eine andere Dienstebene durchführen, wird diese Empfehlung erzwungen. 

> [!NOTE]
> Wenn Sie eine sekundäre Datenbank als Teil der Konfiguration der Failovergruppe erstellt haben, sollten Sie kein Downgrade der sekundären Datenbank durchführen. So wird sichergestellt, dass Ihre Datenebene nach dem Aktivieren des Failovers ausreichende Kapazität zum Verarbeiten des normalen Workloads hat. 
>

## <a name="preventing-the-loss-of-critical-data"></a>Verhindern des Verlusts wichtiger Daten
Aufgrund der hohen Latenz von WANs wird für die fortlaufende Kopie ein asynchroner Replikationsmechanismus verwendet. Asynchrone Replikation macht Datenverlust unvermeidlich, sobald ein Ausfall auftritt. Bei einigen Anwendung dürfen jedoch ggf. keine Daten verloren gehen. Um diese kritischen Aktualisierungen zu schützen, kann ein Anwendungsentwickler die Systemprozedur [sp_wait_for_database_copy_sync](https://msdn.microsoft.com/library/dn467644.aspx) aufrufen, unmittelbar nachdem der Commit für die Transaktion erfolgt ist. Das Aufrufen von **sp_wait_for_database_copy_sync** blockiert den aufrufenden Thread so lange, bis die letzte Transaktion mit erfolgtem Commit in die sekundäre Datenbank repliziert wurde. Die Prozedur wartet, bis alle in der Warteschlange befindlichen Transaktionen von der sekundären Onlinedatenbank bestätigt wurden. **sp_wait_for_database_copy_sync** ist auf eine bestimmte fortlaufende Kopierverknüpfung begrenzt. Jeder Benutzer mit den Rechten zum Herstellen der Verbindung mit der primären Datenbank kann diese Prozedur aufrufen.

> [!NOTE]
> Die Verzögerung durch einen Aufruf der Prozedur **sp_wait_for_database_copy_sync** kann erheblich sein. Die Verzögerung hängt von der aktuellen Größe des Transaktionsprotokolls ab, und dieser Aufruf sendet erst dann eine Rückmeldung, wenn das gesamte Protokoll repliziert ist. Vermeiden Sie den Aufruf dieser Prozedur, es sei denn, er ist erforderlich.
> 

## <a name="programmatically-managing-active-geo-replication"></a>Programmgesteuertes Verwalten der aktiven Georeplikation
Wie bereits erwähnt, können Gruppen für automatisches Failover (in der Vorschau) und aktive Georeplikation auch programmgesteuert mit Azure PowerShell und der REST-API verwaltet werden. Die folgenden Tabellen beschreiben den verfügbaren Satz von Befehlen.

**Azure Resource Manager-API und rollenbasierte Sicherheit:** Die aktive Georeplikation umfasst eine Reihe von [Azure Resource Manager-APIs](https://msdn.microsoft.com/library/azure/mt163571.aspx) für die Verwaltung. Hierzu zählen auch [Azure Resource Manager-basierte PowerShell-Cmdlets](scripts/sql-database-setup-geodr-and-failover-database-powershell.md). Diese APIs erfordern die Verwendung von Ressourcengruppen und unterstützen rollenbasierte Sicherheit (RBAC). Weitere Informationen zur Implementierung von Zugriffsrollen finden Sie unter [Rollenbasierte Zugriffssteuerung in Azure](../active-directory/role-based-access-control-configure.md).

> [!NOTE]
> Viele neue Features für die aktive Georeplikation werden nur in der auf [Azure Resource Manager](../azure-resource-manager/resource-group-overview.md) basierenden [Azure SQL-REST-API](https://msdn.microsoft.com/library/azure/mt163571.aspx) und mit den Azure [SQL-Datenbank-PowerShell-Cmdlets](https://msdn.microsoft.com/library/azure/mt574084.aspx) unterstützt. Zur Gewährleistung der Abwärtskompatibilität werden zwar die [(klassische) REST-API](https://msdn.microsoft.com/library/azure/dn505719.aspx) sowie [(klassische) Azure SQL-Datenbank-Cmdlets](https://msdn.microsoft.com/library/azure/dn546723.aspx) unterstützt, Sie sollten jedoch die Azure Resource Manager-basierten APIs verwenden. 
> 
> 

### <a name="transact-sql"></a>Transact-SQL
| Befehl | Beschreibung |
| --- | --- |
| [ALTER DATABASE (Azure SQL-Datenbank)](https://msdn.microsoft.com/library/mt574871.aspx) |Verwenden Sie das Argument ADD SECONDARY ON SERVER, um eine sekundäre Datenbank für eine vorhandene Datenbank zu erstellen und die Datenreplikation zu starten. |
| [ALTER DATABASE (Azure SQL-Datenbank)](https://msdn.microsoft.com/library/mt574871.aspx) |Verwenden Sie FAILOVER oder FORCE_FAILOVER_ALLOW_DATA_LOSS, um die sekundäre Datenbank zur primären zu erklären und zu ihr zu wechseln – damit starten Sie das Failover. |
| [ALTER DATABASE (Azure SQL-Datenbank)](https://msdn.microsoft.com/library/mt574871.aspx) |Verwenden Sie REMOVE SECONDARY ON SERVER, um die Datenreplikation zwischen einer SQL-Datenbank und der angegebenen sekundären Datenbank zu beenden. |
| [sys.geo_replication_links (Azure SQL-Datenbank)](https://msdn.microsoft.com/library/mt575501.aspx) |Gibt Informationen über alle vorhandenen Replikationsverknüpfungen für alle Datenbanken auf dem logischen Azure SQL-Datenbankserver zurück. |
| [sys.dm_geo_replication_link_status (Azure SQL-Datenbank)](https://msdn.microsoft.com/library/mt575504.aspx) |Ruft den Zeitpunkt der letzten Replikation, die Verzögerung der letzten Replikation und andere Informationen über die Replikationsverknüpfung für eine angegebene SQL-Datenbank ab. |
| [sys.dm_operation_status (Azure SQL-Datenbank)](https://msdn.microsoft.com/library/dn270022.aspx) |Zeigt den Status für alle Datenbankvorgänge an, einschließlich des Status der Replikationsverknüpfungen. |
| [sp_wait_for_database_copy_sync (Azure SQL-Datenbank)](https://msdn.microsoft.com/library/dn467644.aspx) |Bewirkt, dass die Anwendung wartet, bis alle Transaktionen mit erfolgtem Commit repliziert und von der aktiven sekundären Datenbank bestätigt wurden. |
|  | |

### <a name="powershell"></a>PowerShell
| Cmdlet | Beschreibung |
| --- | --- |
| [Get-AzureRmSqlDatabase](https://docs.microsoft.com/powershell/module/azurerm.sql/get-azurermsqldatabase?view=azurermps-3.7.0) |Ruft mindestens eine Datenbank ab. |
| [New-AzureRmSqlDatabaseSecondary](https://docs.microsoft.com/powershell/module/azurerm.sql/new-azurermsqldatabasesecondary?view=azurermps-3.7.0) |Erstellt eine sekundäre Datenbank für eine vorhandene Datenbank und startet die Datenreplikation. |
| [Set-AzureRmSqlDatabaseSecondary](https://docs.microsoft.com/powershell/module/azurerm.sql/set-azurermsqldatabasesecondary?view=azurermps-3.7.0) |Erklärt die sekundäre Datenbank zur primären und wechselt zu ihr – dadurch wird das Failover gestartet. |
| [Remove-AzureRmSqlDatabaseSecondary](https://docs.microsoft.com/powershell/module/azurerm.sql/remove-azurermsqldatabasesecondary?view=azurermps-3.7.0) |Beendet die Datenreplikation zwischen einer SQL-Datenbank und der angegebenen sekundären Datenbank. |
| [Get-AzureRmSqlDatabaseReplicationLink](https://docs.microsoft.com/powershell/module/azurerm.sql/get-azurermsqldatabasereplicationlink?view=azurermps-3.7.0) |Ruft die Georeplikationsverknüpfungen zwischen einer Azure SQL-Datenbank und einer Ressourcengruppe oder einer SQL Server-Instanz ab. |
| [New-AzureRmSqlDatabaseFailoverGroup](https://docs.microsoft.com/powershell/module/azurerm.sql/set-azurermsqldatabasefailovergroup?view=azurermps-3.7.0) |    Dieser Befehl erstellt eine Failovergruppe und registriert sie auf primären und sekundären Servern.|
| [Remove-AzureRmSqlDatabaseFailoverGroup](https://docs.microsoft.com/powershell/module/azurerm.sql/remove-azurermsqldatabasefailovergroup?view=azurermps-3.7.0) | Entfernt die Failovergruppe vom Server und löscht alle in der Gruppe enthaltenen sekundäre Datenbanken. |
| [Get-AzureRmSqlDatabaseFailoverGroup](https://docs.microsoft.com/powershell/module/azurerm.sql/get-azurermsqldatabasefailovergroup?view=azurermps-3.7.0) | Ruft die Konfiguration der Failovergruppe ab. |
| [Set-AzureRmSqlDatabaseFailoverGroup](https://docs.microsoft.com/powershell/module/azurerm.sql/set-azurermsqldatabasefailovergroup?view=azurermps-3.7.0) |    Ändert die Konfiguration der Failovergruppe. |
| [Switch-AzureRMSqlDatabaseFailoverGroup](https://docs.microsoft.com/powershell/module/azurerm.sql/switch-azurermsqldatabasefailovergroup?view=azurermps-3.7.0) | Löst das Failover der Failovergruppe auf den sekundären Server aus. |
|  | |

### <a name="rest-api"></a>REST-API
| API | Beschreibung |
| --- | --- |
| [Create or Update Database (createMode=Restore)](https://docs.microsoft.com/rest/api/sql/databases#Databases_CreateOrUpdate) |Erstellt oder aktualisiert eine primäre oder sekundäre Datenbank oder stellt diese wieder her. |
| [Get Create or Update Database Status](https://docs.microsoft.com/rest/api/sql/databases#Databases) |Ruft den Status während eines Erstellungsvorgangs ab. |
| [Set Secondary Database as Primary (Planned Failover)](https://docs.microsoft.com/rest/api/sql/databasereplicationlinks#Databases_FailoverReplicationLink) |Stuft eine sekundäre Datenbank in einer Georeplikationspartnerschaft zur neuen primären Datenbank hoch. |
| [Set Secondary Database as Primary (Unplanned Failover)](https://docs.microsoft.com/rest/api/sql/databasereplicationlinks#Databases_FailoverReplicationLinkAllowDataLoss) |Erzwingt ein Failover zur sekundären Datenbank und legt die sekundäre Datenbank als primäre fest. |
| [Get Replication Links](https://docs.microsoft.com/rest/api/sql/databasereplicationlinks#Databases_ListReplicationLinks) |Ruft alle Replikationsverknüpfungen für eine angegebene SQL-Datenbank in einer Georeplikationspartnerschaft ab. Es werden die Informationen abgerufen, die in der Katalogsicht „sys.geo_replication_links“ sichtbar sind. |
| [Get Replication Link](https://docs.microsoft.com/rest/api/sql/databasereplicationlinks#Databases_GetReplicationLink) |Ruft eine spezifische Replikationsverknüpfung für eine angegebene SQL-Datenbank in einer Georeplikationspartnerschaft ab. Es werden die Informationen abgerufen, die in der Katalogsicht „sys.geo_replication_links“ sichtbar sind. |
| [Create failover group](https://docs.microsoft.com/rest/api/sql/databases#Databases_CreateOrUpdate) | Erstellt eine Failovergruppe und registriert sie auf primären und sekundären Servern |
| [Remove Failover Group](https://docs.microsoft.com/rest/api/sql/databases#Databases_CreateOrUpdate) | Entfernt die Failovergruppe vom Server |
| [Get Failover Group](https://docs.microsoft.com/rest/api/sql/databases#Databases) | Ruft die Konfiguration der Failovergruppe ab. |
| [Set Failover Group](https://docs.microsoft.com/rest/api/sql/databases#Databases_CreateOrUpdate) | Ändert die Konfiguration der Failovergruppe. |
| [Failover](https://docs.microsoft.com/rest/api/sql/databasereplicationlinks#Databases_FailoverReplicationLinkAllowDataLoss) | Löst das Failover der Failovergruppe auf den sekundären Server aus. |
|  | |

## <a name="next-steps"></a>Nächste Schritte
* Eine Übersicht und verschiedene Szenarien zum Thema Geschäftskontinuität finden Sie unter [Übersicht über die Geschäftskontinuität](sql-database-business-continuity.md)
* Informationen über automatisierte Sicherungen von Azure SQL-Datenbanken finden Sie unter [Übersicht: Automatisierte SQL-Datenbanksicherungen](sql-database-automated-backups.md).
* Informationen zum Verwenden automatisierter Sicherungen für die Wiederherstellung finden Sie unter [Wiederherstellen einer Datenbank aus vom Dienst initiierten Sicherungen](sql-database-recovery-using-backups.md).
* Informationen zum Verwenden automatisierter Sicherungen für die Archivierung finden Sie unter [Datenbankkopie](sql-database-copy.md).
* Weitere Informationen zu Authentifizierungsanforderungen für einen neuen primären Server und die Datenbank finden Sie unter [Verwalten der Sicherheit der Azure SQL-Datenbank nach der Notfallwiederherstellung](sql-database-geo-replication-security-config.md).


