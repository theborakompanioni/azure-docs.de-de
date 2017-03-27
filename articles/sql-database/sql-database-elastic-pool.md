---
title: "Was sind Pools für elastische Datenbanken? Verwalten mehrerer SQL-Datenbanken – Azure | Microsoft-Dokumentation"
description: "Verwalten und skalieren Sie mehrere SQL-Datenbanken – Hunderte und Tausende - mit elastischen Pools. Ein Preis für Ressourcen, die Sie nach Bedarf verteilen können."
keywords: mehrere Datenbanken, Datenbankressourcen, Datenbankleistung
services: sql-database
documentationcenter: 
author: ddove
manager: jhubbard
editor: 
ms.assetid: b46e7fdc-2238-4b3b-a944-8ab36c5bdb8e
ms.service: sql-database
ms.custom: multiple databases
ms.devlang: NA
ms.date: 03/06/2017
ms.author: ddove
ms.workload: data-management
ms.topic: article
ms.tgt_pltfrm: NA
translationtype: Human Translation
ms.sourcegitcommit: 8a531f70f0d9e173d6ea9fb72b9c997f73c23244
ms.openlocfilehash: 53c54c490e7888edef0dc83d1657adbb3db0b745
ms.lasthandoff: 03/10/2017


---

# <a name="how-elastic-pools-help-you-manage-and-scale-multiple-sql-databases"></a>So helfen Ihnen Pools für elastische Datenbanken beim Verwalten und Skalieren mehrerer SQL-Datenbanken

Pools für elastische SQL-Datenbanken sind eine einfache, kostengünstige Lösung zum Verwalten und Skalieren mehrerer Datenbanken mit variierenden und unvorhersehbaren Anforderungen. Die Datenbanken in einem elastischen Pool befinden sich auf einem einzelnen Azure SQL-Datenbank-Server und nutzen gemeinsam eine festgelegte Anzahl von Ressourcen zu einem festen Preis.

Sie können einen elastischen Pool über das [Azure-Portal](sql-database-elastic-pool-manage-portal.md), mit [PowerShell](sql-database-elastic-pool-manage-powershell.md), [Transact-SQL](sql-database-elastic-pool-manage-tsql.md), [C#](sql-database-elastic-pool-manage-csharp.md) oder der REST-API erstellen und verwalten. Ressourcen für Pools für elastische Datenbanken werden in [eDTUs](sql-database-what-is-a-dtu.md) gemessen.


> [!NOTE]
> Elastische Pools sind in allen Azure-Regionen allgemein verfügbar, mit Ausnahme von „Indien, Westen“. Dort befinden sie sich derzeit in der Vorschauphase.  Die allgemeine Verfügbarkeit von elastischen Pools in dieser Region wird so bald wie möglich bereitgestellt.
>
>


## <a name="how-do-elastic-pools-help-manage-database-resources"></a>Wie unterstützen Pools für elastische Datenbanken die Verwaltung von Datenbankressourcen?

Ein weit verbreitetes SaaS-Anwendungsmuster ist das Einzelmandanten-Datenbankmodell: Jeder Kunde (Datenbank) erhält eine eigene Datenbank. Jeder Kunde verfügt über nicht vorhersagbare Ressourcenanforderungen für Arbeitsspeicher, E/A und CPU. Wie ordnen Sie bei einer schwankenden Nutzung mit Spitzen- und Tiefstwerten Ressourcen effizient und kostengünstig zu? In der Vergangenheit hatten Sie zwei Optionen: (1) überdimensionierte Bereitstellung der Ressourcen, die sich an der Nutzung zu Spitzenzeiten orientiert (samt der damit verbundenen Kosten) oder (2) unterdimensionierte Bereitstellung, um Kosten einzusparen, zu Lasten von Leistung und Kundenzufriedenheit während der Spitzenzeiten. Elastische Pools lösen dieses Problem, indem sie sicherstellen, dass die Datenbanken die Leistungsressourcen erhalten, die sie benötigen, und zwar genau zu dem Zeitpunkt, zu dem sie sie benötigen. Sie stellen einen einfachen Ressourcenzuordnungsmechanismus innerhalb eines vorhersagbaren Budgets bereit. Weitere Informationen zu Entwurfsmustern für SaaS-Anwendungen, für die elastische Pools verwendet werden, finden Sie unter [Entwurfsmuster für SaaS-Anwendungen mit mehreren Mandanten und Azure SQL-Datenbank](sql-database-design-patterns-multi-tenancy-saas-applications.md).

> [!VIDEO https://channel9.msdn.com/Blogs/Azure/Elastic-databases-helps-SaaS-developers-tame-explosive-growth/player]
>

In SQL-Datenbank wird das relative Maß für die Fähigkeit einer Datenbank, mit Ressourcenanforderungen umzugehen, in Datenbanktransaktionseinheiten (Database Transaction Units, DTUs) für einzelne Datenbanken und in elastischen DTUs (elastic Database Transaction Units, eDTUs) für Datenbanken in einem elastischen Pool angegeben. Weitere Informationen zu DTUs und eDTUs finden Sie unter [Einführung in SQL-Datenbank](sql-database-technical-overview.md).

Ein Pool für elastische Datenbanken erhält eine festgelegte Anzahl von eDTUs zu einem festen Preis. Im Pool können einzelne Datenbanken die automatische Skalierung innerhalb der angegebenen Parameter flexibel automatisch skalieren. Bei hoher Auslastung kann eine Datenbank mehr eDTUs nutzen, um die Anforderungen zu erfüllen. Datenbanken verbrauchen bei geringerer Auslastung weniger Ressourcen und ohne Auslastung gar keine eDTUs. Durch die Bereitstellung von Ressourcen für den gesamten Pool und nicht nur für einzelne Datenbanken vereinfachen Sie Ihre Verwaltungsaufgaben. Außerdem verfügen Sie über ein vorhersagbares Budget für den Pool.

Einem vorhandenen Pool können zusätzliche eDTUs hinzugefügt werden, ohne dass es zu Datenbankausfällen kommt. Es gilt jedoch die Ausnahme, dass die Datenbanken unter Umständen verschoben werden müssen, um die zusätzlichen Computeressourcen für die neue eDTU-Reserverierung bereitzustellen. Ebenso können zusätzliche eDTUs, die nicht mehr benötigt werden, zu jedem beliebigen Zeitpunkt aus einem vorhandenen Pool entfernt werden.

Und Sie können dem Pool Datenbanken hinzufügen oder Datenbanken aus dem Pool entfernen. Wenn die Ressourcen für eine Datenbank voraussichtlich nicht ausgeschöpft werden, sollten Sie sie entfernen.

## <a name="which-databases-go-in-a-pool"></a>Welche Datenbanken werden in einem Pool angeordnet?
![SQL-Datenbanken, die eDTUs in einem elastischen Pool gemeinsam nutzen.][1]

Datenbanken, die für elastische Pools besonders geeignet sind, weisen in der Regel Zeiträume der Aktivität als auch der Inaktivität auf. Im obigen Beispiel sehen Sie die Aktivität einer einzelnen Datenbank, von 4 Datenbanken und schließlich eines elastischen Pools mit 20 Datenbanken. Datenbanken mit veränderlicher Aktivität im Laufe der Zeit eignen sich besonders für elastische Pools, da sie nicht alle zur gleichen Zeit aktiv sind und eDTUs gemeinsam nutzen können. Nicht alle Datenbanken entsprechen jedoch diesem Muster. Datenbanken mit einem konstanteren Ressourcenbedarf eignen sich besser für die Basic-, Standard- und Premium-Dienstebenen, bei denen Ressourcen einzeln zugewiesen werden.

[Überlegungen zum Preis und zur Leistung eines elastischen Pools](sql-database-elastic-pool-guidance.md).

## <a name="edtu-and-storage-limits-for-elastic-pools"></a>eDTUs und Speicherbeschränkungen für elastische Pools

In der folgenden Tabelle sind die Merkmale von elastischen Basic-, Standard- und Premium-Pools beschrieben.

[!INCLUDE [SQL DB service tiers table for elastic pools](../../includes/sql-database-service-tiers-table-elastic-pools.md)]

Wenn alle DTUs eines elastischen Pools verwendet werden, erhält jede Datenbank im Pool gleich viel Ressourcen zum Verarbeiten von Abfragen.  Der SQL-Datenbank-Dienst bietet eine faire gemeinsame Nutzung von Ressourcen durch Datenbanken, indem gleiche Slices an Computezeit zugesichert werden. Diese faire gemeinsame Nutzung in elastischen Pools ergänzt jegliche Ressourcen, die jeder Datenbank auf andere Weise garantiert werden, wenn das DTU-Minimum pro Datenbank auf einen Wert ungleich null festgelegt ist.

## <a name="elastic-pool-properties"></a>Eigenschaften elastischer Pools

Die folgenden Tabellen beschreiben die Beschränkungen für elastische Pools und in einem Pool zusammengefasste Datenbanken.

### <a name="limits-for-elastic-pools"></a>Beschränkungen für elastische Pools
| Eigenschaft | Beschreibung |
|:--- |:--- |
| Dienstebene |Basic, Standard oder Premium. Die Dienstebene bestimmt den Bereich der Leistungs- und Speichergrenzen, die konfiguriert werden können, sowie die Wahlmöglichkeiten für die Geschäftskontinuität. Jede Datenbank innerhalb eines Pools hat die gleiche Dienstebene wie der Pool. „Dienstebene“ wird auch als „Edition“ bezeichnet. |
| eDTUs pro Pool |Die maximale Anzahl der eDTUs, die von Datenbanken im Pool gemeinsam verwendet werden können. Die Gesamtanzahl der eDTUs, die von Datenbanken im Pool genutzt werden, kann diese Grenze zum gleichen Zeitpunkt nicht überschreiten. |
| Max. Speicherkapazität pro Pool (GB) |Der maximale Speicherplatz in GB, der von Datenbanken im Pool gemeinsam verwendet werden kann. Der Gesamtspeicher, der von allen Datenbanken im Pool verwendet wird, kann diese Grenze nicht überschreiten. Die Grenze wird durch die eDTUs per Pool bestimmt. Wenn diese Grenze überschritten wird, werden alle Datenbanken zu schreibgeschützten Datenbanken. Der maximale Speicherplatz pro Pool bezieht sich auf den maximalen Speicher der Datendateien im Pool und beinhaltet nicht den von Protokolldateien belegten Speicherplatz. |
| Max. Anzahl von Datenbanken pro Pool |Die maximale Anzahl der pro Pool zulässigen Datenbanken. |
| Max. gleichzeitige Worker pro Pool |Die maximale Anzahl der gleichzeitigen für alle Datenbanken im Pool verfügbaren Worker (Anforderungen). |
| Max. gleichzeitige Anmeldungen pro Pool |Die maximale Anzahl der gleichzeitigen Anmeldungen für alle Datenbanken im Pool. |
| Max. gleichzeitige Sitzungen pro Pool |Die maximale Anzahl der für alle Datenbanken im Pool verfügbaren Sitzungen. |
|||

### <a name="limits-for-pooled-databases"></a>Grenzwerte für in einem Pool zusammengefasste Datenbanken
| Eigenschaft | Beschreibung |
|:--- |:--- |
| Max. Anz. von eDTUs pro Datenbank |Die maximale Anzahl von eDTUs, die jede Datenbank im Pool verwenden kann, sofern basierend auf der Nutzung durch andere Datenbanken im Pool verfügbar.  Die maximale Anzahl der eDTUs pro Datenbank ist keine Ressourcengarantie für eine Datenbank.  Dies ist eine globale Einstellung, die für alle Datenbanken im Pool gilt. Legen Sie die maximale Anzahl der eDTUs pro Datenbank hoch genug fest, sodass Spitzen bei der Datenbanknutzung verarbeitet werden können. Sie sollten ein gewisses Maß an Mehrlast einplanen, da für den Pool im Allgemeinen von Nutzungsmustern starker und schwacher Auslastung ausgegangen wird, bei der aber nicht alle Datenbanken gleichzeitig stark ausgelastet sind. Angenommen, die Spitzenauslastung pro Datenbank beträgt 20 eDTUs und betrifft nur 20 % der 100 Datenbanken im Pool.  Wenn die eDTU-Höchstanzahl pro Datenbank auf 20 eDTUs festgelegt ist, ist es sinnvoll, die fünffache Mehrlast für den Pool einzuplanen und die eDTUs pro Pool auf 400 festzulegen. |
| Min. Anz. von eDTUs pro Datenbank |Die minimale Anzahl der eDTUs, die für jede Datenbank im Pool garantiert werden können.  Dies ist eine globale Einstellung, die für alle Datenbanken im Pool gilt. Die Mindestanzahl der eDTUs pro Datenbank kann auf 0 festgelegt werden. Dies ist auch der Standardwert. Diese Eigenschaft ist auf einen Wert zwischen 0 und der durchschnittlichen eDTU-Nutzung pro Datenbank festgelegt. Das Produkt aus der Anzahl von Datenbanken im Pool und der Mindestzahl von eDTUs pro Datenbank darf die tatsächliche Anzahl der eDTUs pro Pool nicht übersteigen.  Wenn ein Pool beispielsweise 20 Datenbanken umfasst und die Mindestanzahl der eDTUs pro Datenbank auf 10 eDTUs festgelegt hat, müssen mindestens 200 eDTUs pro Pool festgelegt sein. |
| Max. Speicherkapazität pro Datenbank (GB) |Der maximale Speicherplatz für eine Datenbank in einem Pool. In einem Pool zusammengefasste Datenbanken nutzen den Poolspeicher gemeinsam, daher ist der Datenbankspeicher auf den jeweils kleineren Wert des verbleibenden Poolspeichers oder des maximalen Speicherplatzes pro Datenbank beschränkt. Der maximale Speicherplatz pro Datenbank bezieht sich auf die maximale Größe der Datendateien und beinhaltet nicht den von Protokolldateien belegten Speicherplatz. |
|||

## <a name="elastic-jobs"></a>Elastische Aufträge
Mit einem Pool werden die Verwaltungsaufgaben vereinfacht, indem Skripts in **[elastischen Aufträgen](sql-database-elastic-jobs-overview.md)** ausgeführt werden. Durch einen elastischen Auftrag werden die meisten aufwändigen Schritte beseitigt, die bei der Verwendung einer großen Anzahl von Datenbanken anfallen. Lesen Sie sich [Erste Schritte mit Aufträgen für die elastische Datenbank](sql-database-elastic-jobs-getting-started.md) durch, bevor Sie beginnen.

Weitere Informationen zu anderen Datenbanktools für die Verwendung mit mehreren Datenbanken finden Sie unter [Horizontales Hochskalieren mit Azure SQL-Datenbank](sql-database-elastic-scale-introduction.md).

## <a name="business-continuity-features-for-databases-in-a-pool"></a>Funktionen der Geschäftskontinuität für Datenbanken in einem Pool
In einem Pool zusammengefasste Datenbanken unterstützen in der Regel die gleichen [Geschäftskontinuitätsfeatures](sql-database-business-continuity.md), die auch bei Einzeldatenbanken zur Verfügung stehen.

### <a name="point-in-time-restore"></a>Point-in-Time-Wiederherstellung
Bei der Point-in-Time-Wiederherstellung werden automatische Datenbanksicherungen verwendet, um den Status einer Datenbank in einem Pool zu einem bestimmten Zeitpunkt wiederherzustellen. Siehe [Point-in-Time-Wiederherstellung](sql-database-recovery-using-backups.md#point-in-time-restore)

### <a name="geo-restore"></a>Geowiederherstellung
Die geografische Wiederherstellung ist die Standardoption für die Wiederherstellung, wenn eine Datenbank aufgrund eines Vorfalls in der Region, in der die Datenbank gehostet wird, nicht verfügbar ist. Siehe [Wiederherstellen einer Azure SQL-Datenbank oder Failover auf eine sekundäre Datenbank](sql-database-disaster-recovery.md)

### <a name="active-geo-replication"></a>Aktive Georeplikation
Bei Anwendungen, für die umfangreichere Wiederherstellungsanforderungen als bei der geografischen Wiederherstellung erforderlich sind, konfigurieren Sie die aktive Georeplikation über das [Azure-Portal](sql-database-geo-replication-portal.md), mit [PowerShell](sql-database-geo-replication-powershell.md) oder [Transact-SQL](sql-database-geo-replication-transact-sql.md).

## <a name="next-steps"></a>Nächste Schritte

* Sie können einen elastischen Pool über das [Azure-Portal](sql-database-elastic-pool-manage-portal.md), mit [PowerShell](sql-database-elastic-pool-manage-powershell.md), [Transact-SQL](sql-database-elastic-pool-manage-tsql.md), [C#](sql-database-elastic-pool-manage-csharp.md) oder der REST-API erstellen und verwalten.
* Anleitungen dazu, wann Sie elastische Pools verwenden sollten, finden Sie unter [Wann sollte ein elastischer Pool verwendet werden?](sql-database-elastic-pool-guidance.md).
* Ein Video finden Sie unter [Microsoft Virtual Academy video course on Azure SQL Database elastic capabilities](https://mva.microsoft.com/en-US/training-courses/elastic-database-capabilities-with-azure-sql-db-16554) (Microsoft Virtual Academy-Videokurs zu Funktionen elastischer Datenbanken in Azure SQL-Datenbank).

<!--Image references-->
[1]: ./media/sql-database-elastic-pool/databases.png

