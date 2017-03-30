---
title: 'SQL-Datenbankleistung: Dienstebenen | Microsoft Docs'
description: "Vergleichen Sie die Dienstebenen für SQL-Datenbank."
keywords: Datenbankoptionen, Datenbankleistung
services: sql-database
documentationcenter: 
author: janeng
manager: jhubbard
editor: 
ms.assetid: f5c5c596-cd1e-451f-92a7-b70d4916e974
ms.service: sql-database
ms.custom: overview
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-management
wms.date: 03/06/2017
ms.author: janeng
translationtype: Human Translation
ms.sourcegitcommit: 07635b0eb4650f0c30898ea1600697dacb33477c
ms.openlocfilehash: ab637f9910523cc8d8967dd1507dbcfad9f7ae88
ms.lasthandoff: 03/28/2017


---
# <a name="sql-database-options-and-performance-understand-whats-available-in-each-service-tier"></a>SQL-Datenbankoptionen und -leistung: Grundlegendes zum Angebot in den einzelnen Tarifen

Azure SQL-Datenbank bietet vier [Dienstebenen](sql-database-technical-overview.md): **Basic**, **Standard**, **Premium** und **Premium RS**. Jede Dienstebene hat mehrere Leistungsstufen für unterschiedliche Workloads. Bei höheren Leistungsstufen stehen zusätzliche Ressourcen zur Verfügung, um einen höheren Durchsatz zu ermöglichen. Sie können die Tarife und Leistungsebenen dynamisch und ohne Ausfallzeiten ändern. Die Dienstebenen „Basic“, „Standard“ und „Premium“ haben alle eine Betriebszeit-SLA von 99,99 Prozent und bieten flexible Optionen für Geschäftskontinuität, Sicherheitsfeatures und stündliche Abrechnung. Die Premium RS-Ebene bietet die gleichen Leistungsstufen, Sicherheits- und Geschäftskontinuitätsfeatures wie die Premium-Ebene, wenn auch mit reduzierter SLA.

> [!IMPORTANT]
> Premium RS-Datenbanken werden mit einer niedrigeren Anzahl redundanter Kopien als Premium- oder Standard-Datenbanken ausgeführt. Im Falle eines Dienstfehlers müssen Sie daher Ihre Datenbank unter Umständen aus einer Sicherung mit einem Rückstand von bis zu 5 Minuten wiederherstellen.
>

Sie können Einzeldatenbanken mit dedizierten Ressourcen innerhalb einer Dienstebene auf einer bestimmten [Leistungsstufe](sql-database-service-tiers.md#single-database-service-tiers-and-performance-levels) erstellen. Außerdem können Sie Datenbanken in einem [Pool für elastische Datenbanken ](sql-database-service-tiers.md#elastic-pool-service-tiers-and-performance-in-edtus) erstellen, in dem die Ressourcen gemeinsam von mehreren Datenbanken verwendet werden. Die für Einzeldatenbanken verfügbaren Ressourcen werden als Datenbanktransaktionseinheiten (Database Transaction Units, DTUs) bezeichnet, die für Pools für elastische Datenbanken verfügbaren Ressourcen als elastische DTUs bzw. eDTUs (elastic Database Transaction Units). Weitere Informationen zu DTUs und eDTUs finden Sie unter [Was sind Datenbanktransaktionseinheiten (DTUs)?](sql-database-what-is-a-dtu.md). 

## <a name="choosing-a-service-tier"></a>Auswählen einer Dienstebene
In der folgenden Tabelle sind Beispiele für Dienstebenen aufgeführt, die sich für unterschiedliche Anwendungsworkloads am besten eignen.

| Dienstebene | Zielworkloads |
| :--- | --- |
| **Basic** | Eignet sich am besten für eine kleine Datenbank, in der normalerweise ein einzelner aktiver Vorgang zu einem bestimmten Zeitpunkt unterstützt wird. Beispiele hierfür sind Datenbanken, die für die Entwicklung, für Tests oder für kleine, selten verwendete Anwendungen verwendet werden. |
| **Standard** |Optimale Option für Cloudanwendungen mit geringen bis mittleren E/A-Leistungsanforderungen. Unterstützt mehrere parallele Abfragen. Beispiele hierfür sind Arbeitsgruppen-oder Webanwendungen. |
| **Premium** | Konzipiert für hohes Transaktionsvolumen mit hohen E/A-Leistungsanforderungen. Unterstützt zahlreiche gleichzeitige Benutzer. Beispiele hierfür sind Datenbanken für unternehmenskritische Anwendungen. |
| **Premium RS** | Speziell für E/A-intensive Workloads, die nicht die höchsten Verfügbarkeitsgarantien benötigen. Beispiele hierfür sind das Testen von Hochleistungsworkloads oder eine analytische Workload, wo die Datenbank nicht das bevorzugte System ist. |
|||

Entscheiden Sie zuerst, ob Sie eine einzelne Datenbank mit einer definierten Menge dedizierter Ressourcen ausführen möchten, oder ob Sie einen Pool von Ressourcen für eine Gruppe von Datenbanken gemeinsam verwenden möchten. Informieren Sie sich über die [Überlegungen zu elastischen Pools](sql-database-elastic-pool-guidance.md). Bestimmen Sie zu Beginn die Datenbankfeatures, die Sie mindestens benötigen:

| **Dienstebenenfeatures** | **Basic** | **Standard** | **Premium** | **Premium RS**|
| :-- | --: | --: | --: | --: |
| Maximale Größe der einzelnen Datenbank | 2 GB | 250 GB | 4TB*  | 500 GB  |
| Maximaler Gesamtspeicher in einem Pool für elastische Datenbanken | 117 GB | 1.200GB | 750 GB | 750 GB |
| Maximale Anzahl von Datenbanken pro Pool | 400  | 400 | 50 | 50 |
| Aufbewahrungszeitraum von Datenbanksicherungen | 7 Tage | 35 Tage | 35 Tage | 35 Tage |
||||||

> [!IMPORTANT]
> Kunden mit den Leistungsstufen P11 und P15 können bis zu 4TB Speicher ohne Aufpreis nutzen. Diese 4-TB-Option ist derzeit in folgenden Regionen in der Public Preview verfügbar: USA, Osten 2; USA, Westen; Europa, Westen; Asien, Südosten; Japan, Osten; Australien, Osten; Kanada, Mitte und Kanada, Osten. Aktuelle Einschränkungen finden Sie unter [Current limitations of P11 and P15 databases with 4 TB maxsize](sql-database-service-tiers.md#current-limitations-of-p11-and-p15-databases-with-4-tb-maxsize) (Aktuelle Einschränkungen bei P11- und P15-Datenbanken mit maximal 4TB).
>

Nach der Ermittlung der Mindestdienstebene können Sie die Leistungsebene für die Datenbank (Anzahl von DTUs) bestimmen. Die standardmäßigen Leistungsebenen S2 und S3 sind meist ein guter Ausgangspunkt. Für Datenbanken mit hohen CPU- oder E/A-Anforderungen empfehlen sich dagegen eher die Premium-Leistungsebenen. Premium bietet im Vergleich zur höchsten Standard-Leistungsebene mehr CPU- und eine mindestens zehnmal höhere E/A-Leistung.

## <a name="single-database-service-tiers-and-performance-levels"></a>Tarife und Leistungsstufen für Einzeldatenbanken
Bei Einzeldatenbanken gibt es mehrere Leistungsstufen auf den einzelnen Dienstebenen. Über das [Azure-Portal](sql-database-manage-single-databases-portal.md) oder mit [PowerShell](scripts/sql-database-monitor-and-scale-database-powershell.md), [Transact-SQL](sql-database-manage-single-databases-tsql.md), C# und der REST-API können Sie flexibel den Tarif auswählen, der die Anforderungen Ihrer Workload am besten erfüllt. 

Unabhängig von der Anzahl von gehosteten Datenbanken erhält Ihre Datenbank einen garantierten Ressourcensatz, und die erwarteten Leistungsmerkmale Ihrer Datenbank werden nicht beeinträchtigt.

[!INCLUDE [SQL DB service tiers table](../../includes/sql-database-service-tiers-table.md)]

> [!NOTE]
> Eine ausführliche Beschreibung aller anderen Zeilen in dieser Dienstebenentabelle finden Sie unter [Service tier capabilities and limits](sql-database-performance-guidance.md#service-tier-capabilities-and-limits)(Funktionen und Limits von Dienstebenen; in englischer Sprache).
> 

## <a name="scaling-up-or-scaling-down-a-single-database"></a>Zentrales Hoch- oder Herunterskalieren einer einzelnen Datenbank

Nach dem Auswählen eines Tarifs und einer Leistungsebene können Sie eine einzelne Datenbank dynamisch und bedarfsgerecht zentral hoch- oder herunterskalieren. Wenn Sie zentral hoch- oder herunterskalieren müssen, können Sie die Tarife Ihrer Datenbank im [Azure-Portal](sql-database-manage-single-databases-portal.md) oder mit [PowerShell](scripts/sql-database-monitor-and-scale-database-powershell.md), [Transact-SQL](sql-database-manage-single-databases-tsql.md), C# und der REST-API problemlos ändern, und zwar ohne Ausfallzeiten der Anwendung. 

> [!VIDEO https://channel9.msdn.com/Blogs/Azure/Azure-SQL-Database-dynamically-scale-up-or-scale-down/player]
>

Wenn Sie die Dienst- und/oder die Leistungsstufe einer Datenbank ändern, wird ein Replikat der ursprünglichen Datenbank mit der neuen Leistungsebene erstellt und anschließend die Verbindung auf dieses Replikat umgestellt. Während dieses Vorgangs gehen keine Daten verloren. Allerdings sind die Verbindungen zur Datenbank inaktiv, während kurz auf das Replikat umgestellt wird. Daher werden möglicherweise einige aktive Transaktionen zurückgesetzt. Dieses Zeitfenster variiert, ist aber durchschnittlich kleiner als vier Sekunden und in mehr als 99 % der Fälle kürzer als 30 Sekunden. Falls im Moment der Verbindungstrennung viele Transaktionen stattfinden, ist das Zeitfenster unter Umständen größer.  

Die Dauer des gesamten zentralen Hochskalierungsvorgangs hängt sowohl von der Größe als auch vom Diensttarif der Datenbank vor und nach der Änderung ab. Beispielsweise sollte eine 250-GB-Datenbank beim Wechsel in, aus oder innerhalb eines Standarddiensttarifs innerhalb von sechs Stunden zentral hochskalieren. Eine Datenbank der gleichen Größe, die ihre Leistungsstufen innerhalb des Premium-Diensttarifs ändert, sollte dies innerhalb von drei Stunden vollziehen.

* Für ein Downgrade einer Datenbank sollte die Datenbank kleiner als die in der Zieldienstebene maximal zulässige Größe sein. 
* Beim Aktualisieren einer Datenbank, für die [Georeplikation](sql-database-geo-replication-portal.md) aktiviert ist, müssen Sie vor der Aktualisierung der primären Datenbank zunächst die zugehörigen sekundären Datenbanken auf die gewünschte Leistungsstufe aktualisieren.
* Beim Downgrade von einer Premium-Dienstebene müssen Sie zuerst alle geografischen Replikationsbeziehungen beenden. Sie können die im Thema [Wiederherstellen nach einem Ausfall](sql-database-disaster-recovery.md) beschriebenen Schritte verwenden, um den Replikationsprozess zwischen der primären und den aktiven sekundären Datenbanken zu beenden.
* Die Angebote des Wiederherstellungsdienstes variieren für die verschiedenen Dienstebenen. Wenn Sie ein Downgrade durchführen, verlieren Sie eventuell die Möglichkeit einer Zeitpunktwiederherstellung, oder der Aufbewahrungszeitraum für Sicherungen verkürzt sich. Weitere Informationen finden Sie unter [Sichern und Wiederherstellen der Azure SQL-Datenbank](sql-database-business-continuity.md).
* Die neuen Eigenschaften für die Datenbank werden erst angewendet, wenn die Änderungen abgeschlossen sind.

> [!IMPORTANT]
> Ausführliche Schritte finden Sie unter [SQL-Datenbankoptionen und -leistung: Grundlegendes zum Angebot in den einzelnen Tarifen](sql-database-manage-single-databases-portal.md), [Manage a single database with PowerShell](scripts/sql-database-monitor-and-scale-database-powershell.md) (Verwalten einzelner Datenbanken mit PowerShell) oder [Erstellen und Verwalten einzelner Azure SQL-Datenbanken mit Transact-SQL](sql-database-manage-single-databases-tsql.md).
>

## <a name="elastic-pool-service-tiers-and-performance-in-edtus"></a>Dienstebenen und Leistung für elastische Pools in eDTUs

Durch Pools können Datenbanken eDTU-Ressourcen freigeben und gemeinsam nutzen, ohne dass jeder Datenbank im Pool eine bestimmte Leistungsebene zugewiesen werden muss. Beispielsweise kann eine Einzeldatenbank in einem Pool der Dienstebene „Standard“ 0 eDTU bis hin zum maximalen eDTU-Wert für Datenbanken nutzen, den Sie beim Konfigurieren des Pools festlegen. Bei Pools können mehrere Datenbanken mit unterschiedlichen Workloads die für den gesamten Pool verfügbaren eDTU-Ressourcen effizient nutzen. Weitere Informationen finden Sie unter den [Überlegungen zum Preis und zur Leistung eines elastischen Pools](sql-database-elastic-pool-guidance.md) .

In der folgenden Tabelle sind die Merkmale der Dienstebenen eines Pools beschrieben.

[!INCLUDE [SQL DB service tiers table for elastic pools](../../includes/sql-database-service-tiers-table-elastic-pools.md)]

Für jede Datenbank in einem Pool gelten auch die Merkmale für Einzeldatenbanken für die entsprechende Dienstebene. Der Basic-Pool verfügt in Bezug auf die maximalen Sitzungen pro Pool beispielsweise über eine Beschränkung auf den Bereich von 4.800 bis 28.800. Eine eigenständige Datenbank in einem Basic-Pool verfügt dagegen über eine Datenbankbeschränkung von 300 Sitzungen.

## <a name="scaling-up-or-scaling-down-an-elastic-pool"></a>Zentrales Hoch- oder Herunterskalieren eines Pools für elastische Datenbanken

Nach dem Auswählen eines Tarifs und einer Leistungsebene können Sie den Pool für elastische Datenbanken dynamisch und bedarfsgerecht zentral hoch- oder herunterskalieren. 

* Änderungen der minimalen oder maximalen Anzahl von eDTUs pro Datenbank werden in der Regel in höchstens fünf Minuten durchgeführt.
* Wie lange es dauert, die Poolgröße (eDTUs) zu ändern, hängt von der kombinierten Größe aller Datenbanken im Pool ab. Änderungen dauern durchschnittlich 90 Minuten oder weniger pro 100 GB. Wenn beispielsweise der gesamte Speicherplatz für alle Datenbanken im Pool 200 GB beträgt, ist für die Änderung der eDTUs pro Pool eine Latenzzeit von drei Stunden oder weniger zu erwarten.

Ausführliche Schritte finden Sie unter [Erstellen und Verwalten eines Pools für elastische Datenbanken über das Azure-Portal](sql-database-elastic-pool-manage-portal.md), [Erstellen und Verwalten eines Pools für elastische Datenbanken mit PowerShell](scripts/sql-database-monitor-and-scale-pool-powershell.md), [Überwachen und Verwalten eines elastischen Pools per Transact-SQL](sql-database-elastic-pool-manage-tsql.md) oder [Erstellen und Verwalten eines Pools für elastische Datenbanken mit C#](sql-database-elastic-pool-manage-csharp.md).

## <a name="creating-or-upgrading-to-4tb"></a>Erstellen oder Aktualisieren auf 4TB

In den folgenden Abschnitten werden die Implementierungsdetails für die 4-TB-Option erläutert.

### <a name="creating-in-the-azure-portal"></a>Erstellen im Azure-Portal

Beim Erstellen einer P11-/P15-Datenbank ist die Standardspeicheroption von 1 TB vorausgewählt. Für Datenbanken, die sich in einer der unterstützten Regionen befinden, können Sie den Speicher auf maximal 4TB erhöhen. Für alle anderen Regionen kann der Speicherschieberegler nicht geändert werden. Der Preis ändert sich nicht, wenn Sie 4TB Speicher wählen.

### <a name="creating-using-powershell-or-transact-sql"></a>Erstellen mithilfe von PowerShell oder Transact-SQL

Beim Erstellen einer P11-/P15-Datenbank können Sie den MAXSIZE-Wert auf 1TB (Standard) oder 4TB festlegen. Werte von „1.024GB“ und „4.096GB“ werden ebenfalls akzeptiert. Falls Sie die MAXSIZE-Option von 4TB wählen, tritt beim Erstellungsbefehl ein Fehler auf, wenn die Datenbank in einer nicht unterstützten Region bereitgestellt wird.

### <a name="upgrading-to-4tb"></a>Upgrade auf 4TB 

Für vorhandene P11- und P15-Datenbanken, die sich in einer der unterstützten Regionen befinden, können Sie den MAXSIZE-Wert auf 4TB erhöhen. Dies ist im Azure-Portal, in PowerShell oder mit Transact-SQL möglich. Das folgende Beispiel zeigt den MAXSIZE-Wert der mit dem Befehl ALTER DATABASE geändert wird:

```ALTER DATABASE <DatabaseName> MODIFY (MAXSIZE = 4096 GB);
```

Upgrading an existing P11 or P15 database can only be performed by a server-level principal login or by members of the dbmanager database role. 
If executed in a supported region the configuration will be updated immediately. This can be checked using the [SELECT DATABASEPROPERTYEX](https://msdn.microsoft.com/library/ms186823.aspx) or by inspecting the database size in the Azure portal. The database will remain online during the upgrade process. However, you will not be able to utilize the full 4 TB of storage until the actual database files have been upgraded to the new maxsize. The length of time required depends upon on the size of the database being upgraded.  

### Error messages
When creating or upgrading an P11/P15 database in an unsupported region, the create or upgrade operation will fail with the following error message: **P11 and P15 database with up to 4TB of storage are available in US East 2, West US, South East Asia, West Europe, Canada East, Canada Central, Japan East, and Australia East.**

## Current limitations of P11 and P15 databases with 4 TB maxsize

- When creating or updating a P11 or P15 database, you can only chose between 1 TB and 4 TB maxsize. Intermediate storage sizes are not currently supported.
- The 4 TB database maxsize cannot be changed to 1 TB even if the actual storage used is below 1 TB. Thus, you cannot downgrade a P11-4TB/P15-4TB to a P11-1TB/P15-1TB or a lower performance tier (e.g., to P1-P6) until we are providing additional storage options for the rest of the performance tiers. This restriction also applies to the restore and copy scenarios including point-in-time, geo-restore, long-term-backup-retention, and database copy. Once a database is configured with the 4 TB option, all restore operations of this database must be into a P11/P15 with 4 TB maxsize.
- For Active Geo-Replication scenarios:
   - Setting up a geo-replication relationship: If the primary database is P11 or P15, the secondary(ies) must also be P11 or P15; lower performance tiers will be rejected as secondaries since they are not capable of supporting 4 TB.
   - Upgrading the primary database in a geo-replication relationship: Changing the maxsize to 4 TB on a primary database will trigger the same change on the secondary database. Both upgrades must be successful for the change on the primary to take effect. Region limitations for the 4TB option apply (see above). If the secondary is in a region that does not support 4 TB, the primary will not be upgraded.
- Using the Import/Export service for loading P11-4TB/P15-4TB databases is not supported. Use SqlPackage.exe to [import](sql-database-import-sqlpackage.md) and [export](sql-database-export-sqlpackage.md) data.

## Next steps

* Learn the details of [elastic pools](sql-database-elastic-pool-guidance.md) and [price and performance considerations for elastic pools](sql-database-elastic-pool-guidance.md).
* Learn how to [Monitor, manage, and resize elastic pools](sql-database-elastic-pool-manage-portal.md) and [Monitor the performance of single databases](sql-database-single-database-monitor.md).
* Now that you know about the SQL Database tiers, try them out with a [free account](https://azure.microsoft.com/pricing/free-trial/) and learn [how to create your first SQL database](sql-database-get-started.md).
* For migration scenarios, use the [DTU Calculator](http://dtucalculator.azurewebsites.net/) to approximate the number of DTUs needed. 


