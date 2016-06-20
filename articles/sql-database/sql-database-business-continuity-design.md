<properties 
   pageTitle="SQL-Datenbank-Entwurf für Geschäftskontinuität" 
   description="In diesem Abschnitt finden Sie Anleitungen für die Auswahl der Features für die Notfallwiederherstellung mit Geschäftskontinuität (BCDR). Dazu gehören Beschreibungen der Funktionen, die Sie automatisch durch Verwendung einer SQL-Datenbank erhalten."
   services="sql-database" 
   documentationCenter="" 
   authors="elfisher" 
   manager="jhubbard" 
   editor="monicar"/>

<tags
   ms.service="sql-database"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-management" 
   ms.date="05/27/2016"
   ms.author="elfish"/>

# Entwerfen für Geschäftskontinuität

Beim Entwerfen von Anwendungen für Geschäftskontinuität müssen Sie die folgenden Fragen beantworten:

1. Welche Funktion der Geschäftskontinuität eignet sich am besten, um meine Anwendung vor Ausfällen zu schützen?
2. Welches Maß an Redundanz und welche Replikationstopologie kann ich verwenden?

Detaillierte Wiederherstellungsstrategien bei Verwendung eines elastischen Pools finden Sie unter [Strategien für die Notfallwiederherstellung für Anwendungen mit elastischem SQL-Datenbankpool](sql-database-disaster-recovery-strategies-for-applications-with-elastic-pool.md).

## Gründe für das Verwenden der geografischen Wiederherstellung

Die [geografische Wiederherstellung](sql-database-geo-restore.md) ist die Standardoption für die Wiederherstellung, wenn eine Datenbank aufgrund eines Vorfalls in der Region, in der die Datenbank gehostet wird, nicht verfügbar ist. SQL-Datenbank bietet standardmäßig einen integrierten Basisschutz für jede Datenbank. Dies wird durch das Speichern von Sicherungskopien der Datenbank in geografisch redundantem Azure-Speicher (GRS) erreicht. Wenn Sie diese Methode auswählen, ist keine besondere Konfiguration oder zusätzliche Ressourcenzuordnung erforderlich. Sie können mit diesen Sicherungen Ihre Datenbank in einer beliebigen Region mit dem Befehl "Geo-Restore" wiederherstellen. Im Abschnitt [Wiederherstellen nach einem Ausfall](sql-database-disaster-recovery.md) finden Sie Details zur Verwendung von "geo-restore" zur Wiederherstellung der Anwendung.

Sie sollten den integrierten Schutz verwenden, wenn Ihre Anwendung die folgenden Kriterien erfüllt:

1. Es handelt sich um keine unternehmenskritische Anwendung. Sie besitzt keine bindende SLA, sodass eine Ausfallzeit von 24 Stunden oder länger keine finanzielle Haftung nach sich zieht.
2. Die Häufigkeit von Datenänderungen ist niedrig (z. B. Transaktionen pro Stunde). Ein RPO-Wert von einer Stunde führt nicht zu erheblichen Datenverlusten.
3. Bei der Anwendung muss sehr auf die Kosten geachtet werden, sodass keine zusätzlichen Kosten für die Georeplikation möglich sind. 

Informationen zur Aktvierung der geografischen Wiederherstellung finden Sie unter [Geowiederherstellung einer Azure SQL-Datenbank aus einer georedundanten Sicherung](sql-database-geo-restore-portal.md).

> [AZURE.NOTE] Bei der geografischen Wiederherstellung wird keine Rechenkapazität in bestimmten Regionen im Voraus reserviert, um aktive Datenbanken nach einem Ausfall aus einer Sicherungskopie wiederherzustellen. Der Dienst verwaltet die Arbeitsauslastung im Zusammenhang mit "geo-restore"-Anforderungen so, dass die Auswirkungen auf vorhandene Datenbanken in dieser Region minimiert werden und deren Kapazitätsanforderungen Priorität haben. Daher hängt die Wiederherstellungszeit der Datenbank davon ab, wie viele andere Datenbanken in derselben Region zur gleichen Zeit wiederhergestellt werden.

## Gründe für das Verwenden der aktiven Georeplikation

Die [aktive Georeplikation](sql-database-geo-replication-overview.md) ermöglicht die Erstellung lesbarer (sekundärer) Datenbanken in einer anderen Region als der Region Ihrer primären Datenbank. Damit wird sichergestellt, dass die erforderlichen Daten und Rechenressourcen zur Unterstützung der Arbeitsauslastung der Anwendung nach der Wiederherstellung für die Datenbank zur Verfügung stehen. Im Abschnitt [Wiederherstellen nach einem Ausfall](sql-database-disaster-recovery.md) finden Sie Informationen zum Verwenden eines Failovers zum Wiederherstellen der Anwendung.

Sie sollten die Georeplikation verwenden, wenn Ihre Anwendung die folgenden Kriterien erfüllt:

1. Sie ist geschäftskritisch. Sie besitzt eine bindende SLA mit anspruchsvollen RPO- und RTO-Werten. Der Verlust von Daten und ein Ausfall bei der Verfügbarkeit ziehen eine finanzielle Haftung nach sich. 
2. Die Häufigkeit von Datenänderungen ist hoch (z. B. Transaktionen pro Minute oder Sekunde). Der für den Standardschutz geltende RPO-Wert von einer Stunde führt wahrscheinlich zu nicht akzeptablen Datenverlusten.
3. Die Kosten für die Verwendung der Georeplikation sind deutlich niedriger als die potenziellen finanziellen Haftungsschäden und die damit einhergehenden Geschäftsverluste.

Informationen zum Aktivieren der aktiven Georeplikation finden Sie unter [Konfigurieren der Georeplikation für die Azure SQL-Datenbank mit dem Azure-Portal](sql-database-geo-replication-portal.md).

> [AZURE.NOTE] Die aktive Georeplikation unterstützt auch schreibgeschützten Zugriff auf die sekundäre Datenbank, wodurch zusätzliche Kapazität für die schreibgeschützten Arbeitsauslastungen bereitgestellt wird.




##Auswählen der Failover-Konfiguration 

Sie sollten beim Entwerfen der Anwendung für die Geschäftskontinuität verschiedene Konfigurationsoptionen berücksichtigen. Die Auswahl hängt von der Bereitstellungstopologie für die Anwendung ab und davon, welche Teile der Anwendungen am anfälligsten für einen Ausfall sind. Anleitungen finden Sie unter [Entwerfen von Cloudlösungen für die Notfallwiederherstellung mithilfe der Georeplikation](sql-database-designing-cloud-solutions-for-disaster-recovery.md).

<!---HONumber=AcomDC_0608_2016-->