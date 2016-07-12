<properties
   pageTitle="Geschäftskontinuität für die Cloud – Datenbankwiederherstellung – SQL-Datenbank | Microsoft Azure"
   description="Erfahren Sie, wie Azure SQL-Datenbank die Geschäftskontinuität in der Cloud sowie die Datenbankwiederherstellung unterstützt und dafür sorgt, dass geschäftskritische Cloudanwendungen in Betrieb gehalten werden."
   keywords="Geschäftskontinuität,Geschäftskontinuität in der Cloud,Notfallwiederherstellung von Datenbanken,Datenbankwiederherstellung"
   services="sql-database"
   documentationCenter=""
   authors="carlrabeler"
   manager="jhubbard"
   editor="monicar"/>

<tags
   ms.service="sql-database"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="sqldb-bcdr"
   ms.date="06/09/2016"
   ms.author="carlrab"/>

# Geschäftskontinuität mit Azure SQL-Datenbank

Die Azure SQL-Datenbank bietet eine Reihe von Geschäftskontinuitätslösungen. Bei Geschäftskontinuität geht es um das Entwerfen, Bereitstellen und Ausführen von Anwendungen mit Flexibilität gegenüber geplanten oder ungeplanten Unterbrechungen, die zu einem dauerhaften oder vorübergehenden Ausfall der von der Anwendung ausgeführten Geschäftsfunktion führen können. Ungeplante Ereignisse reichen von Benutzerfehlern über dauerhafte oder zeitweise Ausfälle bis hin zu regionalen Katastrophen, die weitreichende Verluste an Einrichtungen in einer bestimmten Azure-Region verursachen können. Zu geplanten Ereignissen gehören die erneute Bereitstellung der Anwendung in einer anderen Region, Anwendungsupgrades usw. Das Ziel der Geschäftskontinuität ist die Fortsetzung der Anwendung während solcher Ereignisse mit minimalen Auswirkungen auf die Geschäftsfunktion.

Im Zusammenhang mit den cloudbasierten Geschäftskontinuitätslösungen der SQL-Datenbank müssen Sie mit einigen Konzepten vertraut sein. Dies sind:

* **Notfallwiederherstellung:** ein Vorgang zur Wiederherstellung der normalen Geschäftsfunktionen der Anwendung

* **Geschätzte Wiederherstellungszeit:** die geschätzte Dauer bis zur vollständigen Verfügbarkeit der Datenbank nach einer Wiederherstellungs- oder Failover-Anforderung.

* **RTO (Recovery Time Objective)**: die maximal zulässige Zeitspanne bis zur vollständigen Wiederherstellung der Anwendung nach der Störung. RTO misst den maximalen Verlust der Verfügbarkeit während des Ausfalls.

* **RPO (Recovery Point Objective)**: die maximale Anzahl von letzten Updates (Zeitraum), die für die Anwendung bis zum Moment der vollständigen Wiederherstellung nach dem Ausfall verloren gehen dürfen. RPO misst den maximalen Datenverlust während des Ausfalls.


## Szenarien für die cloudbasierte Geschäftskontinuität der SQL-Datenbank

Folgende zentrale Szenarien für die Planung von Geschäftskontinuität und Datenbankwiederherstellung sollten Sie bedenken:

### Entwerfen von Anwendungen für Geschäftskontinuität

Die Anwendung, die ich erstelle, ist entscheidend für mein Unternehmen. Ich möchte sie so entwickeln und konfigurieren, dass sie einen regionalen Ausfall oder Totalausfall des Diensts überstehen kann. Ich kenne die Anforderungen für RPO und RTO für meine Anwendung und wähle eine Konfiguration, die diese Anforderungen erfüllt.

### Wiederherstellen nach Benutzerfehlern

Ich habe über Administratorrechte Zugriff auf die Produktionsversion der Anwendung. Ich habe bei der regelmäßigen Wartung einen Fehler gemacht und einige wichtige Daten in der Produktionsumgebung gelöscht. Ich möchte die Daten schnell wiederherstellen, um die Auswirkungen des Fehlers zu minimieren.

### Wiederherstellen nach einem Ausfall

Ich verwende meine Anwendung in der Produktion und werde in einer Warnung über einen größeren Ausfall in der Region, in der sie bereitgestellt wird, informiert. Ich möchte die Wiederherstellung initiieren, um sie in einer anderen Region wieder in Betrieb zu nehmen und die Auswirkungen auf das Unternehmen zu minimieren.

### Ausführen von Notfallwiederherstellungsübungen

Da die Datenschicht der Anwendung bei der Wiederherstellung nach einem Ausfall in eine andere Region verschoben wird, möchte ich in regelmäßigen Abständen den Wiederherstellungsprozess testen und die Auswirkungen auf die Bereitstellung der Anwendung bewerten.

### Anwendungsupgrades ohne Ausfallzeiten

Ich gebe ein größeres Upgrade meiner Anwendung frei. Es umfasst Änderungen am Datenbankschema, die Bereitstellung zusätzlicher gespeicherter Prozeduren usw. Für diesen Vorgang muss der Benutzerzugriff auf die Datenbank beendet werden. Gleichzeitig möchte ich sicherstellen, dass das Upgrade keine wesentliche Unterbrechungen der Geschäftsabläufe verursacht.

## Geschäftskontinuitätsfeatures der SQL-Datenbank

Die folgende Tabelle enthält die Geschäftskontinuitätsfeatures der SQL-Datenbank und verdeutlicht die jeweiligen Unterschiede in den verschiedenen [Dienstebenen](sql-database-service-tiers.md):

| Funktion | Basic-Tarif | Standard-Tarif |Premium-Tarif
| --- |--- | --- | ---
| Zeitpunktwiederherstellung | Jeder Wiederherstellungspunkt innerhalb von 7 Tagen | Jeder Wiederherstellungspunkt innerhalb von 14 Tagen | Jeder Wiederherstellungspunkt innerhalb von 35 Tagen
| Geowiederherstellung | Geschätzte Wiederherstellungszeit < 12 h, RPO < 1 h | Geschätzte Wiederherstellungszeit < 12 h, RPO < 1 h | Geschätzte Wiederherstellungszeit < 12 h, RPO < 1 h
| Aktive Georeplikation | Geschätzte Wiederherstellungszeit < 30 s, RPO < 5 s | Geschätzte Wiederherstellungszeit < 30 s, RPO < 5 s | Geschätzte Wiederherstellungszeit < 30 s, RPO < 5 s

Diese Funktionen werden für die zuvor beschriebenen Szenarios bereitgestellt.

> [AZURE.NOTE] Die Werte für die geschätzte Wiederherstellungszeit und RPO sind technische Ziele und dienen lediglich als Leitfaden. Sie sind nicht Bestandteil des [SLA für die SQL-Datenbank](https://azure.microsoft.com/support/legal/sla/sql-database/v1_0/).


###Point-in-Time-Wiederherstellung

Mit der [Point-in-Time-Wiederherstellung](sql-database-recovery-using-backups.md#point-in-time-restore) können Sie Ihre Datenbank in einen früheren Zustand versetzen. Dazu werden Datenbanksicherungen, inkrementelle Sicherungen und Transaktionsprotokollsicherungen verwendet, die der Dienst automatisch für jede Benutzerdatenbank verwaltet. Diese Funktion steht für alle Dienstebenen zur Verfügung. Bei Basic können Sie 7 Tage wiederherstellen, bei Standard 14 Tage und bei Premium 35 Tage.

### Geowiederherstellung

Die [Geowiederherstellung](sql-database-recovery-using-backups.md#geo-restore) steht auch für Basic-, Standard- und Premium-Datenbanken zur Verfügung. Sie stellt die Standardoption für die Wiederherstellung dar, wenn auch die Datenbank aufgrund eines Vorfalls in der Region, in dem die Datenbank gehostet wird, nicht verfügbar ist. Ähnlich wie die Point-in-Time-Wiederherstellung ist die Geowiederherstellung von Datenbanksicherungen in geografisch redundantem Azure-Speicher abhängig. Die Wiederherstellung erfolgt aus der geografisch replizierten Sicherungskopie und ist daher in Bezug auf Speicherausfälle in der primären Region flexibel.

### Aktive Georeplikation

Die [aktive Georeplikation](sql-database-geo-replication-overview.md) ist für alle Datenbanktarife verfügbar. Sie ist für Anwendungen vorgesehen, für die umfangreichere Wiederherstellungsanforderungen erforderlich sind, als die Geowiederherstellung bieten kann. Bei der aktiven Georeplikation können Sie bis zu vier lesbare sekundäre Replikate auf Servern in verschiedenen Regionen erstellen. Sie können ein Failover auf ein beliebiges sekundäres Replikat initiieren. Darüber hinaus kann die aktive Georeplikation verwendet werden, um [Anwendungsupgrades oder die räumliche Verlegung von Anwendungen](sql-database-manage-application-rolling-upgrade.md) zu unterstützen, sowie als Lastenausgleich für schreibgeschützte Workloads.

## Auswählen von Geschäftskontinuitätsfeatures

Beim Entwerfen von Anwendungen für Geschäftskontinuität müssen Sie die folgenden Fragen beantworten:

1. Welche Funktion der Geschäftskontinuität eignet sich am besten, um meine Anwendung vor Ausfällen zu schützen?
2. Welches Maß an Redundanz und welche Replikationstopologie kann ich verwenden?

### Gründe für das Verwenden der Geowiederherstellung

Die [Geowiederherstellung](sql-database-recovery-using-backups.md#geo-restore) ist die Standardoption für die Wiederherstellung, wenn eine Datenbank aufgrund eines Incidents in der Region, in der die Datenbank gehostet wird, nicht verfügbar ist. SQL-Datenbank bietet standardmäßig einen integrierten Basisschutz für jede Datenbank. Dieser wird durch Erstellen und Speichern von [Datenbanksicherungen](sql-database-automated-backups.md) im georedundanten Azure-Speicher (GRS) erreicht. Wenn Sie diese Methode auswählen, ist keine besondere Konfiguration oder zusätzliche Ressourcenzuordnung erforderlich. Sie können die Datenbank in jeder Region wiederherstellen, indem Sie aus diesen automatisierten georedundanten Sicherungen eine Wiederherstellung in einer neuen Datenbank durchführen.

Sie sollten den integrierten Schutz verwenden, wenn Ihre Anwendung die folgenden Kriterien erfüllt:

1. Es handelt sich um keine unternehmenskritische Anwendung. Sie besitzt keine bindende SLA, sodass eine Ausfallzeit von 24 Stunden oder länger keine finanzielle Haftung nach sich zieht.
2. Die Häufigkeit von Datenänderungen ist niedrig (z. B. Transaktionen pro Stunde). Ein RPO-Wert von einer Stunde führt nicht zu erheblichen Datenverlusten.
3. Bei der Anwendung muss sehr auf die Kosten geachtet werden, sodass keine zusätzlichen Kosten für die Georeplikation möglich sind.

> [AZURE.NOTE] Bei der Geowiederherstellung wird keine Rechenkapazität in bestimmten Regionen im Voraus reserviert, um aktive Datenbanken nach einem Ausfall aus einer Sicherungskopie wiederherzustellen. Der Dienst verwaltet die Arbeitsauslastung im Zusammenhang mit "geo-restore"-Anforderungen so, dass die Auswirkungen auf vorhandene Datenbanken in dieser Region minimiert werden und deren Kapazitätsanforderungen Priorität haben. Daher hängt die Wiederherstellungszeit der Datenbank davon ab, wie viele andere Datenbanken in derselben Region zur gleichen Zeit wiederhergestellt werden. Zudem sind die Größe der Datenbank, die Anzahl der Transaktionsprotokolle, die Netzwerkbandbreite usw. von Bedeutung.

### Gründe für das Verwenden der aktiven Georeplikation

Die [aktive Georeplikation](sql-database-geo-replication-overview.md) ermöglicht die Erstellung und Wartung von lesbaren (sekundären) Datenbanken in einer anderen Region als Ihrer primären Region. Diese Datenbanken bleiben durch einen asynchronen Replikationsmechanismus aktuell. Damit wird sichergestellt, dass die erforderlichen Daten und Rechenressourcen zur Unterstützung der Arbeitsauslastung der Anwendung nach der Wiederherstellung für die Datenbank zur Verfügung stehen.

Sie sollten die aktive Georeplikation verwenden, wenn Ihre Anwendung die folgenden Kriterien erfüllt:

1. Sie ist geschäftskritisch. Sie besitzt eine bindende SLA mit anspruchsvollen RPO- und RTO-Werten. Der Verlust von Daten und ein Ausfall bei der Verfügbarkeit ziehen eine finanzielle Haftung nach sich.
2. Die Häufigkeit von Datenänderungen ist hoch (z. B. Transaktionen pro Minute oder Sekunde). Der für den Standardschutz geltende RPO-Wert von einer Stunde führt wahrscheinlich zu nicht akzeptablen Datenverlusten.
3. Die Kosten für die Verwendung der Georeplikation sind deutlich niedriger als die potenziellen finanziellen Haftungsschäden und die damit einhergehenden Geschäftsverluste.

## Entwerfen von Cloudlösungen für die Notfallwiederherstellung 

Sie sollten beim Entwerfen der Anwendung für die Geschäftskontinuität verschiedene Konfigurationsoptionen berücksichtigen. Die Auswahl hängt von der Bereitstellungstopologie für die Anwendung ab und davon, welche Teile der Anwendungen am anfälligsten für einen Ausfall sind. Anleitungen finden Sie unter [Entwerfen von Cloudlösungen für die Notfallwiederherstellung mithilfe der Georeplikation](sql-database-designing-cloud-solutions-for-disaster-recovery.md).

Detaillierte Wiederherstellungsstrategien bei Verwendung eines elastischen Pools finden Sie unter [Strategien für die Notfallwiederherstellung für Anwendungen mit elastischem SQL-Datenbankpool](sql-database-disaster-recovery-strategies-for-applications-with-elastic-pool.md).

## Nächste Schritte

- Informationen über automatisierte Sicherungen von Azure SQL-Datenbanken finden Sie unter [Übersicht: Automatisierte SQL-Datenbanksicherungen](sql-database-automated-backups.md).
- Informationen über Entwurfs- und Wiederherstellungsszenarien für die Geschäftskontinuität finden Sie unter [Geschäftskontinuitätsszenarien](sql-database-business-continuity-scenarios.md).
- Informationen zum Verwenden automatisierter Sicherungen für die Wiederherstellung finden Sie unter [Wiederherstellen einer Datenbank aus vom Dienst initiierten Sicherungen](sql-database-recovery-using-backups.md).
- Informationen über schnellere Wiederherstellungsoptionen finden Sie unter [Aktive Georeplikation](sql-database-geo-replication-overview.md).
- Informationen zum Verwenden automatisierter Sicherungen für die Archivierung finden Sie unter [Datenbankkopie](sql-database-copy.md).

<!---HONumber=AcomDC_0629_2016-->