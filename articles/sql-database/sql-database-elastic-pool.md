<properties
	pageTitle="Was ist ein Azure-Pool für elastische Datenbanken? | Microsoft Azure"
	description="Verwalten Sie hunderte oder tausende Datenbanken mithilfe eines Pools. Ein Preis für einen Satz Leistungseinheiten kann über den Pool verteilt werden. Verschieben Sie die Datenbanken je nach Wunsch in den oder aus dem Pool."
	keywords="elastische Datenbank, SQL-Datenbanken"
	services="sql-database"
	documentationCenter=""
	authors="CarlRabeler"
	manager="jhubbard"
	editor="cgronlun"/>

<tags
	ms.service="sql-database"
	ms.devlang="NA"
	ms.date="07/12/2016"
	ms.author="CarlRabeler"
	ms.workload="data-management"
	ms.topic="article"
	ms.tgt_pltfrm="NA"/>


# Was ist ein Azure-Pool für elastische Datenbanken?

Elastische Pools stellen eine einfache und kostengünstige Lösung zum Verwalten der Leistungsziele für mehrere Datenbanken mit unterschiedlichsten und unvorhersehbaren Nutzungsmustern dar.

> [AZURE.NOTE] Elastische Pools sind in allen Azure-Regionen allgemein verfügbar, mit Ausnahme von „USA, Norden-Mitte“ und „Indien, Westen“. Dort befinden sie sich derzeit in der Vorschauphase. Die allgemeine Verfügbarkeit von elastischen Pools in diesen Regionen wird so bald wie möglich bereitgestellt.

## So funktioniert's

Ein weit verbreitetes Saas-Anwendungsmuster ist das Einzelmandanten-Datenbankmodell: Jeder Kunde erhält eine Datenbank. Jeder Kunde (jede Datenbank) verfügt über nicht vorhersagbare Ressourcenanforderungen für Arbeitsspeicher, E/A und CPU. Wie ordnen Sie bei einer schwankenden Nutzung mit Spitzen- und Tiefstwerten also Ressourcen zu? In der Vergangenheit hatten Sie zwei Optionen: (1) überdimensionierte Bereitstellung der Ressourcen, die sich an der Nutzung zu Spitzenzeiten orientiert (samt der damit verbundenen Kosten) oder (2) unterdimensionierte Bereitstellung, um Kosten einzusparen, zu Lasten von Leistung und Kundenzufriedenheit während der Spitzenzeiten. Pools für elastische Datenbanken beheben dieses Problem, indem sichergestellt wird, dass Datenbanken zum benötigten Zeitpunkt über die erforderlichen Leistungsressourcen verfügen, wobei gleichzeitig ein einfacher Mechanismus für die Ressourcenbelegung bei einem vorhersagbaren Budget geboten wird. Weitere Informationen zu Entwurfsmustern für SaaS-Anwendungen, für die elastische Pools verwendet werden, finden Sie unter [Design Patterns for Multi-tenant SaaS Applications with Azure SQL Database](sql-database-design-patterns-multi-tenancy-saas-applications.md) (Entwurfsmuster für mehrinstanzenfähige SaaS-Anwendungen und Azure SQL-Datenbank).

> [AZURE.VIDEO elastic-databases-helps-saas-developers-tame-explosive-growth]

In SQL-Datenbank wird das relative Maß für die Fähigkeit einer Datenbank, mit Ressourcenanforderungen umzugehen, in Datenbanktransaktionseinheiten (Database Transaction Units, DTUs) für einzelne Datenbanken und in elastischen DTUs (eDTUs) für Pools für elastische Datenbanken angegeben. Weitere Informationen zu DTUs und eDTUs finden Sie unter [Einführung in SQL-Datenbank](sql-database-technical-overview.md#understand-dtus).

Ein Pool erhält eine festgelegte Anzahl von eDTUs zu einem festen Preis. Im Pool können einzelne Datenbanken die automatische Skalierung innerhalb der angegebenen Parameter flexibel automatisch skalieren. Bei hoher Auslastung kann eine Datenbank mehr eDTUs nutzen, um den Bedarf zu befriedigen. Datenbanken verbrauchen bei geringerer Auslastung weniger Ressourcen, und Datenbanken ohne Auslastung verbrauchen gar keine eDTUs. Durch die Bereitstellung von Ressourcen für den gesamten Pool und nicht nur für einzelne Datenbanken vereinfachen Sie Ihre Verwaltungsaufgaben. Außerdem verfügen Sie über ein vorhersagbares Budget für den Pool.

Einem vorhandenen Pool können zusätzliche eDTUs hinzugefügt werden, ohne dass es zu Datenbankausfällen oder einer negativen Auswirkung auf die Datenbanken kommt. Ebenso können zusätzliche eDTUs, die nicht mehr benötigt werden, zu jedem beliebigen Zeitpunkt aus einem vorhandenen Pool entfernt werden.

Und Sie können dem Pool Datenbanken hinzufügen oder Datenbanken aus dem Pool entfernen. Wenn die Ressourcen für eine Datenbank voraussichtlich nicht ausgeschöpft werden, sollten Sie sie entfernen.

## Welche Datenbanken werden in einem Pool angeordnet?

![SQL-Datenbanken, die eDTUs in einem Pool für elastische Datenbanken gemeinsam nutzen][1]

Datenbanken, die für Pools für elastische Datenbanken besonders geeignet sind, weisen in der Regel Zeiträume der Aktivität sowie Zeiträume der Inaktivität auf. Im obigen Beispiel können Sie die Aktivität einer einzelnen Datenbank, von vier Datenbanken und schließlich eines Pools für elastische Datenbanken mit 20 Datenbanken sehen. Datenbanken mit veränderlicher Aktivität im Laufe der Zeit eignen sich besonders für elastische Pools, da sie nicht alle zur gleichen Zeit aktiv sind und eDTUs gemeinsam nutzen können. Nicht alle Datenbanken entsprechen jedoch diesem Muster. Datenbanken mit einem konstanteren Ressourcenbedarf eignen sich besser für die Basic-, Standard- und Premium-Dienstebenen, bei denen Ressourcen einzeln zugewiesen werden.

[Überlegungen zum Preis und zur Leistung eines Pools für elastische Datenbanken](sql-database-elastic-pool-guidance.md)


> [AZURE.NOTE] Pools für elastische Datenbanken sind derzeit als Vorschauversion ausschließlich für Server mit SQL-Datenbank V12 verfügbar.

## eDTUs und Speicherbeschränkungen für elastische Pools und elastische Datenbanken

[AZURE.INCLUDE [Tabelle der SQL-Datenbank-Dienstebenen für elastische Datenbanken](../../includes/sql-database-service-tiers-table-elastic-db-pools.md)]

Wenn alle DTUs eines elastischen Pools verwendet werden, erhält jede Datenbank im Pool gleich viel Ressourcen zum Verarbeiten von Abfragen. Der SQL-DB-Dienst bietet eine faire Ressourcenfreigabe für Datenbanken, indem gleiche Slices an Serverzeit garantiert werden. Diese faire Ressourcenfreigabe ergänzt jede anderweitige Menge an Ressourcen, die jeder Datenbank garantiert wird, wenn das DTU-Minimum pro Datenbank auf einen Wert ungleich null festgelegt ist.

## Eigenschaften von Pools für elastische Datenbanken

### Beschränkungen für elastische Pools

| Eigenschaft | Beschreibung |
| :-- | :-- |
| Dienstebene | Basic, Standard oder Premium. Die Dienstebene bestimmt den Bereich der Leistungs- und Speichergrenzen, die konfiguriert werden können, sowie die Wahlmöglichkeiten für die Geschäftskontinuität. Jede Datenbank innerhalb eines Pools hat die gleiche Dienstebene wie der Pool. „Dienstebene“ wird auch als „Edition“ bezeichnet. |
| eDTUs pro Pool | Die maximale Anzahl der eDTUs, die von Datenbanken im Pool gemeinsam verwendet werden können. Die Gesamtanzahl der eDTUs, die von Datenbanken im Pool genutzt werden, kann diese Grenze zum gleichen Zeitpunkt nicht überschreiten. |
| Max. Speicherkapazität pro Pool (GB) | Der maximale Speicherplatz in GB, der von Datenbanken im Pool gemeinsam verwendet werden kann. Der Gesamtspeicher, der von allen Datenbanken im Pool verwendet wird, kann diese Grenze nicht überschreiten. Die Grenze wird durch die eDTUs per Pool bestimmt. Wenn diese Grenze überschritten wird, werden alle Datenbanken zu schreibgeschützten Datenbanken. |
| Max. Anzahl von Datenbanken pro Pool | Die maximale Anzahl der pro Pool zulässigen Datenbanken. |
| Max. gleichzeitige Worker pro Pool | Die maximale Anzahl der gleichzeitigen für alle Datenbanken im Pool verfügbaren Worker (Anforderungen). |
| Max. gleichzeitige Anmeldungen pro Pool | Die maximale Anzahl der gleichzeitigen Anmeldungen für alle Datenbanken im Pool. |
| Max. gleichzeitige Sitzungen pro Pool | Die maximale Anzahl der für alle Datenbanken im Pool verfügbaren Sitzungen. |


### Beschränkungen für elastische Datenbanken

| Eigenschaft | Beschreibung |
| :-- | :-- |
| Max. Anz. von eDTUs pro Datenbank | Die maximale Anzahl der eDTUs, die jede Datenbank im Pool bei Verfügbarkeit verwenden kann, basierend auf der Nutzung durch andere Datenbanken im Pool. Die maximale Anzahl der eDTUs pro Datenbank ist keine Ressourcengarantie für eine Datenbank. Dies ist eine globale Einstellung, die für alle Datenbanken im Pool gilt. Legen Sie die maximale Anzahl der eDTUs pro Datenbank hoch genug fest, sodass Spitzen bei der Datenbanknutzung verarbeitet werden können. Sie sollten ein gewisses Maß an Mehrlast einplanen, da für den Pool im Allgemeinen von Nutzungsmustern starker und schwacher Auslastung ausgegangen wird, bei der aber nicht alle Datenbanken gleichzeitig stark ausgelastet sind. Angenommen, die Spitzenauslastung pro Datenbank beträgt 20 eDTUs und betrifft nur 20 % der 100 Datenbanken im Pool. Wenn die eDTU-Höchstanzahl pro Datenbank auf 20 eDTUs festgelegt ist, ist es sinnvoll, die fünffache Mehrlast für den Pool einzuplanen und die eDTUs pro Pool auf 400 festzulegen. |
| Min. Anz. von eDTUs pro Datenbank | Die minimale Anzahl der eDTUs, die für jede Datenbank im Pool garantiert werden können. Dies ist eine globale Einstellung, die für alle Datenbanken im Pool gilt. Die Mindestanzahl der eDTUs pro Datenbank kann auf 0 festgelegt werden. Dies ist auch der Standardwert. Diese Eigenschaft ist normalerweise auf einen Wert zwischen 0 und der durchschnittlichen eDTU-Nutzung pro Datenbank festgelegt. Beachten Sie, dass das Produkt aus der Anzahl von Datenbanken im Pool und der Mindestzahl von eDTUs pro Datenbank nicht die tatsächliche Anzahl der eDTUs pro Pool übersteigen kann. Wenn ein Pool beispielsweise 20 Datenbanken umfasst und die Mindestanzahl der eDTUs pro Datenbank auf 10 eDTUs festgelegt hat, müssen mindestens 200 eDTUs pro Pool festgelegt sein. |
| Max. Speicherkapazität pro Datenbank (GB) | Der maximale Speicherplatz für eine Datenbank in einem Pool. Elastische Datenbanken nutzen den Poolspeicher gemeinsam, daher ist der Datenbankspeicher auf den jeweils kleineren Wert des verbleibenden Poolspeichers oder des maximalen Speicherplatzes pro Datenbank beschränkt.|


## Aufträge für die elastische Datenbank

Mit einem Pool werden die Verwaltungsaufgaben vereinfacht, indem Skripts in **[elastischen Aufträgen](sql-database-elastic-jobs-overview.md)** ausgeführt werden. Durch einen Auftrag für die elastische Datenbank werden die meisten aufwändigen Schritte beseitigt, die bei der Verwendung einer großen Zahl von Datenbanken anfallen. Lesen Sie sich [Erste Schritte mit Aufträgen für die elastische Datenbank](sql-database-elastic-jobs-getting-started.md) durch, bevor Sie beginnen.

Weitere Informationen zu anderen Tools finden Sie unter [SQL-Datenbank – Features und Tools elastischer Datenbanken verwenden](https://azure.microsoft.com/documentation/learning-paths/sql-database-elastic-scale/).

## Funktionen der Geschäftskontinuität für Datenbanken in einem Pool

Elastische Datenbanken unterstützen in der Regel die gleichen [Funktionen der Geschäftskontinuität](sql-database-business-continuity.md), die auch für Einzeldatenbanken auf V12-Servern zur Verfügung stehen.


### Point-in-Time-Wiederherstellung

Bei der Point-in-Time-Wiederherstellung werden automatische Datenbanksicherungen verwendet, um den Status einer Datenbank in einem Pool zu einem bestimmten Zeitpunkt wiederherzustellen. Siehe [Point-in-Time-Wiederherstellung](sql-database-recovery-using-backups.md#point-in-time-restore).

### Geowiederherstellung

Die geografische Wiederherstellung ist die Standardoption für die Wiederherstellung, wenn eine Datenbank aufgrund eines Vorfalls in der Region, in der die Datenbank gehostet wird, nicht verfügbar ist. Siehe [Wiederherstellen einer Azure SQL-Datenbank oder Failover auf eine sekundäre Datenbank](sql-database-disaster-recovery.md).

### Aktive Georeplikation

Bei Anwendungen, für die umfangreichere Wiederherstellungsanforderungen als bei der geografischen Wiederherstellung erforderlich sind, konfigurieren Sie die aktive Georeplikation über das [Azure-Portal](sql-database-geo-replication-portal.md), mit [PowerShell](sql-database-geo-replication-powershell.md) oder [Transact-SQL](sql-database-geo-replication-transact-sql.md).


<!--Image references-->
[1]: ./media/sql-database-elastic-pool/databases.png

<!---HONumber=AcomDC_0810_2016-->