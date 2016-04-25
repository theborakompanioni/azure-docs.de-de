<properties
	pageTitle="Pool für elastische Datenbanken für SQL-Datenbanken | Microsoft Azure"
	description="Verwalten Sie hunderte oder tausende Datenbanken mithilfe eines Pools. Ein Preis für einen Satz Leistungseinheiten kann über den Pool verteilt werden. Verschieben Sie die Datenbanken je nach Wunsch in den oder aus dem Pool."
	keywords="elastische Datenbank, SQL-Datenbanken"
	services="sql-database"
	documentationCenter=""
	authors="sidneyh"
	manager="jhubbard"
	editor="cgronlun"/>

<tags
	ms.service="sql-database"
	ms.devlang="NA"
	ms.date="04/04/2016"
	ms.author="sidneyh"
	ms.workload="data-management"
	ms.topic="article"
	ms.tgt_pltfrm="NA"/>


# Was ist ein Azure-Pool für elastische Datenbanken?

Ein SaaS-Entwickler muss Dutzende, Hunderte oder sogar Tausende von SQL-Datenbanken erstellen und verwalten. Ein Pool für elastische Datenbanken erleichtert die Erstellung, Wartung und Leistungsverwaltung über zahlreiche Datenbanken hinweg. Fügen Sie dem Pool je nach Bedarf Datenbanken hinzu, oder entfernen Sie diese. Weitere Informationen finden Sie unter [Erstellen eines skalierbaren Pools für elastische SQL-Datenbanken im Azure-Portal](sql-database-elastic-pool-create-portal.md), mithilfe von [PowerShell](sql-database-elastic-pool-create-powershell.md) oder [C#](sql-database-elastic-pool-csharp.md).

Weitere Informationen zur API und zu Fehlern finden Sie unter [Referenz für Pools für elastische Datenbanken](sql-database-elastic-pool-reference.md).

## So funktioniert's

Ein weit verbreitetes Saas-Anwendungsmuster ist das Einzelmandanten-Datenbankmodell: Jeder Kunde erhält eine Datenbank. Jeder Kunde (jede Datenbank) verfügt über nicht vorhersagbare Ressourcenanforderungen für Arbeitsspeicher, E/A und CPU. Wie ordnen Sie bei einer schwankenden Nutzung mit Spitzen- und Tiefstwerten also Ressourcen zu? In der Vergangenheit hatten Sie zwei Optionen: (1) überdimensionierte Bereitstellung der Ressourcen, die sich an der Nutzung zu Spitzenzeiten orientiert (samt der damit verbundenen Kosten) oder (2) unterdimensionierte Bereitstellung, um Kosten einzusparen, zu Lasten von Leistung und Kundenzufriedenheit während der Spitzenzeiten. Pools für elastische Datenbanken beheben dieses Problem, indem sichergestellt wird, dass Datenbanken zum benötigten Zeitpunkt über die erforderlichen Leistungsressourcen verfügen, wobei gleichzeitig ein einfacher Mechanismus für die Ressourcenbelegung bei einem vorhersagbaren Budget geboten wird.

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

## Eigenschaften von Pools für elastische Datenbanken
Grenzen von elastischen Pools und elastischen Datenbanken.

| Eigenschaft | Beschreibung |
| :-- | :-- |
| Dienstebene | Basic, Standard oder Premium. Die Dienstebene bestimmt den Bereich der Leistungs- und Speichergrenzen, die konfiguriert werden können, sowie die Wahlmöglichkeiten für die Geschäftskontinuität. Jede Datenbank innerhalb eines Pools hat die gleiche Dienstebene wie der Pool. „Dienstebene“ wird auch als „Edition“ bezeichnet.|
| eDTU per pool | Maximale Anzahl von eDTUs, die von Datenbanken im Pool gemeinsam verwendet werden können. Die Gesamtanzahl der eDTUs, die von Datenbanken im Pool genutzt werden, kann diese Grenze zum gleichen Zeitpunkt nicht überschreiten. |
| Storage per pool | Maximale Menge von Speicherplatz, die von Datenbanken im Pool gemeinsam verwendet werden kann. Der Gesamtspeicher, der von allen Datenbanken im Pool verwendet wird, kann diese Grenze nicht überschreiten. Die Grenze wird durch die eDTUs per Pool bestimmt. Wenn diese Grenze überschritten wird, werden alle Datenbanken zu schreibgeschützten Datenbanken. |
| Max eDTU per database | Maximale Anzahl von eDTUs, die jede Datenbank im Pool verwenden darf; gilt für alle Datenbanken im Pool. Die maximale Anzahl eDTUs pro Datenbank ist keine Ressourcengarantie. |
| Min eDTU per database | Die Mindestanzahl von eDTUs, die jeder Datenbank im Pool garantiert ist; sie gilt für alle Datenbanken im Pool. Die Mindestanzahl für eDTUs pro Datenbank kann auf 0 festgelegt werden. Beachten Sie, dass das Produkt aus der Anzahl von Datenbanken im Pool und der Mindestzahl von eDTUs pro Datenbank nicht die tatsächliche Anzahl der eDTUs pro Pool übersteigen kann. |


## eDTUs und Speicherbeschränkungen für elastische Pools und elastische Datenbanken


[AZURE.INCLUDE [Tabelle der SQL-Datenbank-Dienstebenen für elastische Datenbanken](../../includes/sql-database-service-tiers-table-elastic-db-pools.md)]

## Aufträge für die elastische Datenbank

Mit einem Pool werden die Verwaltungsaufgaben vereinfacht, indem Skripts in **[elastischen Aufträgen](sql-database-elastic-jobs-overview.md)** ausgeführt werden. Durch einen Auftrag für die elastische Datenbank werden die meisten aufwändigen Schritte beseitigt, die bei der Verwendung einer großen Zahl von Datenbanken anfallen. Lesen Sie sich [Erste Schritte mit Aufträgen für die elastische Datenbank](sql-database-elastic-jobs-getting-started.md) durch, bevor Sie beginnen.

Weitere Informationen zu anderen Tools finden Sie unter [SQL-Datenbank – Features und Tools für elastische Datenbanken verwenden](https://azure.microsoft.com/documentation/learning-paths/sql-database-elastic-scale/).

## Funktionen der Geschäftskontinuität für Datenbanken in einem Pool

Derzeit unterstützen elastische Datenbanken in der Vorschau die meisten [Funktionen der Geschäftskontinuität](sql-database-business-continuity.md), die auch für Einzeldatenbanken auf V12-Servern zur Verfügung stehen.

### Point-in-Time-Wiederherstellung

Datenbanken in einem Pool für elastische Datenbanken werden automatisch vom System gesichert, und die Aufbewahrungsrichtlinie für die Sicherung stimmt mit der entsprechenden Dienstebene für einzelne Datenbanken überein. Die Datenbanken der einzelnen Ebenen haben also jeweils einen anderen Wiederherstellungsbereich:

* **Basic-Pool:** Die Wiederherstellung ist für jeden Zeitpunkt innerhalb der letzten sieben Tage möglich.
* **Standard-Pool:** Die Wiederherstellung ist für jeden Zeitpunkt innerhalb der letzten 14 Tage möglich.
* **Premium-Pool:** Die Wiederherstellung ist für jeden Zeitpunkt innerhalb der letzten 35 Tage möglich.

Während der Vorschau werden Datenbanken in einem Pool als eine neue Datenbank im gleichen Pool wiederhergestellt. Gelöschte Datenbanken werden immer als eigenständige Datenbank außerhalb des Pools in der niedrigsten Leistungsstufe für diese Dienstebene wiederhergestellt. Beispielsweise wird eine elastische Datenbank in einem Standard-Pool, die gelöscht wird, als S0-Datenbank wiederhergestellt. Sie können Datenbank-Wiederherstellungsvorgänge über das Azure-Verwaltungsportal oder programmgesteuert mithilfe der REST-API ausführen. PowerShell-Cmdlet-Unterstützung wird demnächst zur Verfügung stehen.

### Geografische Wiederherstellung

Mit der geografischen Wiederherstellung können Sie eine Datenbank in einem Pool auf einem Server an einem anderen Standort wiederherstellen. Bei der Vorschau muss zum Wiederherstellen einer Datenbank in einem Pool auf einem anderen Server der Pool auf dem Zielserver über den gleichen Namen wie der Quellpool verfügen. Bei Bedarf erstellen Sie einen neuen Pool auf dem Zielserver und geben ihm den gleichen Namen wie vor dem Wiederherstellen der Datenbank. Die geografische Wiederherstellung schlägt fehl, wenn kein Pool mit dem gleichen Namen auf dem Zielserver vorhanden ist. Informationen finden Sie unter [Wiederherstellen mit geografischer Wiederherstellung](sql-database-disaster-recovery.md#recover-using-geo-restore).


### Georeplikation

Georeplikation ist für jede Datenbank in einem Standard- oder Premiumpool mit elastischen Datenbanken verfügbar. Eine oder alle Datenbanken in einer Georeplikationspartnerschaft können in einem Pool mit elastischen Datenbanken enthalten sein, sofern die Dienstebenen identisch sind. Sie können die Georeplikation für Pools mit elastischen Datenbanken mit dem [Azure-Portal](sql-database-geo-replication-portal.md), [PowerShell](sql-database-geo-replication-powershell.md) oder [Transact-SQL](sql-database-geo-replication-transact-sql.md) konfigurieren.

### Import und Export

Das Exportieren einer Datenbank aus einem Pool wird unterstützt. Derzeit wird das direkte Importieren einer Datenbank in einen Pool nicht unterstützt, aber Sie können Daten in eine Einzeldatenbank importieren und die Datenbank anschließend in einen Pool verschieben.


<!--Image references-->
[1]: ./media/sql-database-elastic-pool/databases.png

<!---HONumber=AcomDC_0413_2016-->