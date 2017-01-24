---
title: Partitionieren von Tabellen in SQL Data Warehouse | Microsoft Docs
description: "Enthält Informationen zu den ersten Schritten bei der Tabellenpartitionierung in Azure SQL Data Warehouse."
services: sql-data-warehouse
documentationcenter: NA
author: jrowlandjones
manager: jhubbard
editor: 
ms.assetid: 6cef870c-114f-470c-af10-02300c58885d
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.date: 10/31/2016
ms.author: jrj;barbkess
translationtype: Human Translation
ms.sourcegitcommit: dcda8b30adde930ab373a087d6955b900365c4cc
ms.openlocfilehash: 0d2ff3ce90c355ba63f3fb66982baa621091ae6e


---
# <a name="partitioning-tables-in-sql-data-warehouse"></a>Partitionieren von Tabellen in SQL Data Warehouse
> [!div class="op_single_selector"]
> * [Übersicht][Overview]
> * [Datentypen][Data Types]
> * [Verteilen][Distribute]
> * [Index][Index]
> * [Partition][Partition]
> * [Statistiken][Statistics]
> * [Temporär][Temporary]
> 
> 

Das Partitionieren wird für alle SQL Data Warehouse-Tabellentypen unterstützt, z.B. gruppierter Columnstore, gruppierter Index und Heap.  Außerdem wird die Partitionierung für alle Verteilungstypen unterstützt, z.B. Hash- oder Roundrobin-Verteilung.  Die Partitionierung ermöglicht Ihnen die Aufteilung Ihrer Daten in kleinere Gruppen, und in den meisten Fällen wird die Partitionierung für eine Datumsspalte durchgeführt.

## <a name="benefits-of-partitioning"></a>Vorteile der Partitionierung
Durch die Partitionierung können sich Vorteile für die Wartung und die Abfrageleistung ergeben.  Ob sich Vorteile für beide oder nur einen dieser Punkte ergeben, hängt davon ab, wie Daten geladen werden und ob dieselbe Spalte für beide Zwecke genutzt werden kann. Die Partitionierung kann nämlich nur für eine Spalte durchgeführt werden.

### <a name="benefits-to-loads"></a>Vorteile für Lasten
Der Hauptvorteil der Partitionierung in SQL Data Warehouse ist die Verbesserung der Effizienz und Leistung beim Laden von Daten, indem Partitionen gelöscht, gewechselt und zusammengeführt werden.  In den meisten Fällen werden Daten nach einer Datumsspalte partitioniert, die eng an die Sequenz gebunden ist, mit der die Daten in die Datenbank geladen werden.  Einer der größten Vorteile bei der Verwendung von Partitionen zum Verwalten von Daten ist die Vermeidung der Transaktionsprotokollierung.  Das simple Einfügen, Aktualisieren oder Löschen von Daten kann zwar der einfachste Ansatz sein, aber wenn Sie etwas Planung und Arbeit investieren, kann die Leistung durch die Verwendung der Partitionierung während des Ladevorgangs erheblich verbessert werden.

Sie können den Partitionswechsel einsetzen, um einen Abschnitt einer Tabelle schnell zu entfernen oder auszutauschen.  So kann beispielsweise eine Umsatzfaktentabelle erstellt werden, die nur Daten für die letzten 36 Monate enthält.  Am Monatsende wird jeweils der älteste Verkaufsdatenmonat aus der Tabelle gelöscht.  Sie können eine delete-Anweisung verwenden, um jeweils die Daten für den ältesten Monat zu löschen.  Das Löschen einer großen Datenmenge Zeile für Zeile mit einer delete-Anweisung kann aber sehr lange dauern und das Risiko für große Transaktionen erhöhen, deren Rollback sehr viel Zeit in Anspruch nimmt, wenn es zu Fehlern kommt.  Ein wesentlich besserer Ansatz besteht darin, einfach die älteste Partition der Daten zu löschen.  Während das Löschen der einzelnen Zeilen Stunden dauern kann, werden für das Löschen einer gesamten Partition meist nur wenige Sekunden benötigt.

### <a name="benefits-to-queries"></a>Vorteile für Abfragen
Die Partitionierung kann auch verwendet werden, um die Abfrageleistung zu verbessern.  Wenn eine Abfrage einen Filter auf eine partitionierte Spalte anwendet, kann der Scan nur auf die qualifizierten Partitionen beschränkt werden. Dies kann eine viele kleinere Teilmenge der Daten sein, und ein Scan der gesamten Tabelle wird vermieden.  Aufgrund der Einführung von gruppierten Columnstore-Indizes sind die Leistungsvorteile bei der Prädikatbeseitigung nicht so groß, aber in einigen Fällen können sich trotzdem Vorteile für Abfragen ergeben.  Beispiel: Wenn eine Umsatzfaktentabelle auf der Grundlage des Verkaufsdatumsfelds in 36 Monate partitioniert wird, kann bei Abfragen, die das Verkaufsdatum als Filter verwenden, das Durchsuchen von Partitionen übersprungen werden, die nicht dem Filter entsprechen.

## <a name="partition-sizing-guidance"></a>Anleitung zur Festlegung der Partitionsgröße
Die Partitionierung kann zwar verwendet werden, um die Leistung in einigen Szenarien zu verbessern, aber das Erstellen einer Tabelle mit **zu vielen** Partitionen kann die Leistung unter Umständen beeinträchtigen.  Dies gilt besonders für gruppierte Columnstore-Tabellen.  Es muss klar sein, wann sich der Einsatz der Partitionierung anbietet und wie viele Partitionen erstellt werden sollten, damit die Partitionierung hilfreich ist.  Es gibt keine genaue Vorgabe, welche Anzahl von Partitionen zu hoch ist. Dies hängt von Ihren Daten und außerdem davon ab, wie viele Partitionen gleichzeitig geladen werden.  Als Faustregel gilt aber, dass nicht Tausende von Partitionen hinzugefügt werden sollten, sondern eher Dutzende oder Hunderte.

Beim Erstellen der Partitionierung für **gruppierte Columnstore**-Tabellen ist es wichtig zu beachten, wie viele Zeilen in jede Partition eingefügt werden.  Für eine optimale Komprimierung und Leistung von gruppierten Columnstore-Tabellen sind mindestens 1 Million Zeilen pro Verteilung und Partition erforderlich.  Bereits vor der Erstellung von Partitionen teilt SQL Data Warehouse jede Tabelle auf 60 verteilte Datenbanken auf.  Jegliche Partitionierungen, die einer Tabelle hinzugefügt werden, werden zusätzlich zu den im Hintergrund erstellten Verteilungen durchgeführt.  Für dieses Beispiel bedeutet dies Folgendes: Wenn die Umsatzfaktentabelle 36 Monatspartitionen enthält und SQL Data Warehouse 60 Verteilungen umfasst, muss die Umsatzfaktentabelle mindestens 60 Millionen Zeilen pro Monat umfassen (oder 2,1 Milliarden Zeilen, wenn alle Monate aufgefüllt sind).  Wenn eine Tabelle deutlich weniger Zeilen als das empfohlene Minimum an Zeilen pro Partition enthält, sollten Sie die Verwendung von weniger Partitionen erwägen, um die Anzahl von Zeilen pro Partition zu erhöhen.  Lesen Sie sich auch den Artikel zur [Indizierung][Index] durch. Darin sind Abfragen enthalten, die für SQL Data Warehouse ausgeführt werden können, um die Qualität von gruppierten Columnstore-Indizes zu bewerten.

## <a name="syntax-difference-from-sql-server"></a>Syntaxunterschied gegenüber SQL Server
Mit SQL Data Warehouse wird eine vereinfachte Definition der Partitionen eingeführt, die sich geringfügig von der in SQL Server unterscheidet.  Partitionierungsfunktionen und -schemen werden in SQL Data Warehouse nicht so wie in SQL Server verwendet.  Stattdessen müssen Sie lediglich die partitionierte Spalte und die Grenzpunkte identifizieren.  Die Syntax der Partitionierung kann gegenüber SQL Server leicht variieren, aber die grundlegenden Konzepte sind identisch.  SQL Server und SQL Data Warehouse unterstützen eine Partitionsspalte pro Tabelle, und es kann sich um eine Bereichspartition handeln.  Weitere Informationen zur Partitionierung finden Sie unter [Partitionierte Tabellen und Indizes][Partitioned Tables and Indexes].

Im folgenden Beispiel für eine SQL Data Warehouse-Partition mit der [CREATE TABLE][CREATE TABLE]-Anweisung wird die Tabelle „FactInternetSales“ nach der Spalte „OrderDateKey“ partitioniert:

```sql
CREATE TABLE [dbo].[FactInternetSales]
(
    [ProductKey]            int          NOT NULL
,   [OrderDateKey]          int          NOT NULL
,   [CustomerKey]           int          NOT NULL
,   [PromotionKey]          int          NOT NULL
,   [SalesOrderNumber]      nvarchar(20) NOT NULL
,   [OrderQuantity]         smallint     NOT NULL
,   [UnitPrice]             money        NOT NULL
,   [SalesAmount]           money        NOT NULL
)
WITH
(   CLUSTERED COLUMNSTORE INDEX
,   DISTRIBUTION = HASH([ProductKey])
,   PARTITION   (   [OrderDateKey] RANGE RIGHT FOR VALUES
                    (20000101,20010101,20020101
                    ,20030101,20040101,20050101
                    )
                )
)
;
```

## <a name="migrating-partitioning-from-sql-server"></a>Migrieren der Partitionierung von SQL Server
Gehen Sie einfach wie folgt vor, um SQL Server-Partitionsdefinitionen zu SQL Data Warehouse zu migrieren:

* Entfernen Sie das SQL Server-[Partitionsschema][partition scheme].
* Fügen Sie die Definition der [Partitionsfunktion][partition function] von CREATE TABLE hinzu.

Wenn Sie eine partitionierte Tabelle von einer SQL Server-Instanz migrieren, können Sie mit dem unten angegebenen SQL-Code die Anzahl von Zeilen in jeder Partition ermitteln.  Beachten Sie Folgendes: Wenn für SQL Data Warehouse die gleiche Partitionierungsgranularität verwendet wird, verringert sich die Anzahl von Zeilen pro Partition um den Faktor 60.  

```sql
-- Partition information for a SQL Server Database
SELECT      s.[name]                        AS      [schema_name]
,           t.[name]                        AS      [table_name]
,           i.[name]                        AS      [index_name]
,           p.[partition_number]            AS      [partition_number]
,           SUM(a.[used_pages]*8.0)         AS      [partition_size_kb]
,           SUM(a.[used_pages]*8.0)/1024    AS      [partition_size_mb]
,           SUM(a.[used_pages]*8.0)/1048576 AS      [partition_size_gb]
,           p.[rows]                        AS      [partition_row_count]
,           rv.[value]                      AS      [partition_boundary_value]
,           p.[data_compression_desc]       AS      [partition_compression_desc]
FROM        sys.schemas s
JOIN        sys.tables t                    ON      t.[schema_id]         = s.[schema_id]
JOIN        sys.partitions p                ON      p.[object_id]         = t.[object_id]
JOIN        sys.allocation_units a          ON      a.[container_id]      = p.[partition_id]
JOIN        sys.indexes i                   ON      i.[object_id]         = p.[object_id]
                                            AND     i.[index_id]          = p.[index_id]
JOIN        sys.data_spaces ds              ON      ds.[data_space_id]    = i.[data_space_id]
LEFT JOIN   sys.partition_schemes ps        ON      ps.[data_space_id]    = ds.[data_space_id]
LEFT JOIN   sys.partition_functions pf      ON      pf.[function_id]      = ps.[function_id]
LEFT JOIN   sys.partition_range_values rv   ON      rv.[function_id]      = pf.[function_id]
                                            AND     rv.[boundary_id]      = p.[partition_number]
WHERE       p.[index_id] <=1
GROUP BY    s.[name]
,           t.[name]
,           i.[name]
,           p.[partition_number]
,           p.[rows]
,           rv.[value]
,           p.[data_compression_desc]
;
```

## <a name="workload-management"></a>Workloadverwaltung
Ein letzter Punkt, den Sie der Tabellenpartitionsentscheidung berücksichtigen müssen, ist die [Workloadverwaltung][workload management].  Bei der Workloadverwaltung in SQL Data Warehouse geht es hauptsächlich um die Verwaltung von Arbeitsspeicher und Parallelität.  In SQL Data Warehouse wird die maximale Speichermenge für die einzelnen Verteilungspunkte während der Abfrageausführung über Ressourcenklassen geregelt.  Idealerweise sollte die Größe unter Berücksichtigung anderer Faktoren festgelegt werden, z.B. den Arbeitsspeicheranforderungen in Bezug auf die Erstellung von gruppierten Columnstore-Indizes.  Gruppierte Columnstore-Indizes profitieren stark, wenn ihnen mehr Arbeitsspeicher zugeordnet wird.  Aus diesem Grund sollten Sie sicherstellen, dass für die Neuerstellung eines Partitionsindex genügend Arbeitsspeicher zur Verfügung steht. Sie können die Menge an Arbeitsspeicher für die Abfrage erhöhen, indem Sie von der Standardrolle (smallrc) zu einer der anderen verfügbaren Rollen (z.B. largerc) wechseln.

Informationen über die Zuordnung von Arbeitsspeicher pro Verteilung erhalten Sie durch Abfragen der dynamischen Resource Governor-Verwaltungsansichten. In der Praxis ist die Arbeitsspeicherzuweisung kleiner als in den folgenden Abbildungen. Die Abbildung bietet jedoch eine Richtlinie, die Sie für die Größe der Partitionen für Verwaltungsvorgänge verwenden können.  Versuchen Sie, zu vermeiden, Ihre Partitionen größer als die Arbeitsspeicherzuweisung zu erstellen, die durch die größte Ressourcenklasse bereitgestellt wird. Wenn die Partitionen die Größen in dieser Abbildung übersteigen, besteht das Risiko der Speicherauslastung, was wiederum zu weniger optimaler Komprimierung führt.

```sql
SELECT  rp.[name]                                AS [pool_name]
,       rp.[max_memory_kb]                        AS [max_memory_kb]
,       rp.[max_memory_kb]/1024                    AS [max_memory_mb]
,       rp.[max_memory_kb]/1048576                AS [mex_memory_gb]
,       rp.[max_memory_percent]                    AS [max_memory_percent]
,       wg.[name]                                AS [group_name]
,       wg.[importance]                            AS [group_importance]
,       wg.[request_max_memory_grant_percent]    AS [request_max_memory_grant_percent]
FROM    sys.dm_pdw_nodes_resource_governor_workload_groups    wg
JOIN    sys.dm_pdw_nodes_resource_governor_resource_pools    rp ON wg.[pool_id] = rp.[pool_id]
WHERE   wg.[name] like 'SloDWGroup%'
AND     rp.[name]    = 'SloDWPool'
;
```

## <a name="partition-switching"></a>Partitionswechsel
SQL Data Warehouse unterstützt das Aufteilen, Zusammenführen und Wechseln von Partitionen. Jede dieser Funktionen wird mithilfe der [ALTER TABLE][ALTER TABLE]-Anweisung ausgeführt.

Für den Wechsel zweier Partitionen zwischen zwei Tabellen müssen Sie sicherstellen, dass die Partitionen an ihren jeweiligen Grenzen ausgerichtet sind und die Tabellendefinitionen übereinstimmen. Da keine Überprüfungseinschränkungen verfügbar sind, um den Bereich der Werte in einer Tabelle zu erzwingen, muss die Quelltabelle die gleichen Partitionsgrenzen enthalten wie die Zieltabelle. Ist dies nicht der Fall, schlägt der Partitionswechsel fehl, da die Partitionsmetadaten nicht synchronisiert werden.

### <a name="how-to-split-a-partition-that-contains-data"></a>Aufteilen einer Partition mit Daten
Die effizienteste Methode, um eine Partition zu teilen, die bereits Daten enthält, ist die Verwendung einer `CTAS` -Anweisung. Wenn es sich bei der partitionierten Tabelle um einen gruppierten Columnstore handelt, muss die Tabellenpartition leer sein, bevor sie aufgeteilt werden kann.

Im Folgenden finden Sie ein Beispiel für eine partitionierte Columnstore-Tabelle, die eine Zeile in jeder Partition enthält:

```sql
CREATE TABLE [dbo].[FactInternetSales]
(
        [ProductKey]            int          NOT NULL
    ,   [OrderDateKey]          int          NOT NULL
    ,   [CustomerKey]           int          NOT NULL
    ,   [PromotionKey]          int          NOT NULL
    ,   [SalesOrderNumber]      nvarchar(20) NOT NULL
    ,   [OrderQuantity]         smallint     NOT NULL
    ,   [UnitPrice]             money        NOT NULL
    ,   [SalesAmount]           money        NOT NULL
)
WITH
(   CLUSTERED COLUMNSTORE INDEX
,   DISTRIBUTION = HASH([ProductKey])
,   PARTITION   (   [OrderDateKey] RANGE RIGHT FOR VALUES
                    (20000101
                    )
                )
)
;

INSERT INTO dbo.FactInternetSales
VALUES (1,19990101,1,1,1,1,1,1);
INSERT INTO dbo.FactInternetSales
VALUES (1,20000101,1,1,1,1,1,1);


CREATE STATISTICS Stat_dbo_FactInternetSales_OrderDateKey ON dbo.FactInternetSales(OrderDateKey);
```

> [!NOTE]
> Durch das Erstellen des Statistikobjekts stellen wir sicher, dass die Tabellenmetadaten genauer sind. Wenn wir das Erstellen von Statistiken auslassen, verwendet SQL Data Warehouse Standardwerte. Ausführliche Informationen zu Statistiken finden Sie unter [Statistiken][statistics].
> 
> 

Anschließend können wir mit der `sys.partitions` -Katalogsicht die Zeilenanzahl abfragen:

```sql
SELECT  QUOTENAME(s.[name])+'.'+QUOTENAME(t.[name]) as Table_name
,       i.[name] as Index_name
,       p.partition_number as Partition_nmbr
,       p.[rows] as Row_count
,       p.[data_compression_desc] as Data_Compression_desc
FROM    sys.partitions p
JOIN    sys.tables     t    ON    p.[object_id]   = t.[object_id]
JOIN    sys.schemas    s    ON    t.[schema_id]   = s.[schema_id]
JOIN    sys.indexes    i    ON    p.[object_id]   = i.[object_Id]
                            AND   p.[index_Id]    = i.[index_Id]
WHERE t.[name] = 'FactInternetSales'
;
```

Wenn wir versuchen, diese Tabelle aufzuteilen, erhalten wir einen Fehler:

```sql
ALTER TABLE FactInternetSales SPLIT RANGE (20010101);
```

Msg 35346, Level 15, State 1, Line 44 SPLIT clause of ALTER PARTITION statement failed because the partition is not empty.  Nur leere Partitionen können aufgeteilt werden, wenn ein Columnstore-Index für die Tabelle vorhanden ist. Deaktivieren Sie ggf. den Columnstore-Index vor Ausgabe der ALTER PARTITION-Anweisung, und erstellen Sie dann nach Abschluss von ALTER PARTITION den Columnstore-Index neu.

Wir können aber auch `CTAS` für die Erstellung einer neuen Tabelle zum Speichern von Daten verwenden.

```sql
CREATE TABLE dbo.FactInternetSales_20000101
    WITH    (   DISTRIBUTION = HASH(ProductKey)
            ,   CLUSTERED COLUMNSTORE INDEX
            ,   PARTITION   (   [OrderDateKey] RANGE RIGHT FOR VALUES
                                (20000101
                                )
                            )
            )
AS
SELECT *
FROM    FactInternetSales
WHERE   1=2
;
```

Ein Wechsel ist zulässig, da die Partitionsgrenzen ausgerichtet sind. Dadurch verbleibt die Quelltabelle mit einer leeren Partition, die wir später aufteilen können.

```sql
ALTER TABLE FactInternetSales SWITCH PARTITION 2 TO  FactInternetSales_20000101 PARTITION 2;

ALTER TABLE FactInternetSales SPLIT RANGE (20010101);
```

Wir müssen nun lediglich die Daten mit `CTAS` an die neuen Partitionsgrenzen anpassen und wieder in die Haupttabelle zurückführen.

```sql
CREATE TABLE [dbo].[FactInternetSales_20000101_20010101]
    WITH    (   DISTRIBUTION = HASH([ProductKey])
            ,   CLUSTERED COLUMNSTORE INDEX
            ,   PARTITION   (   [OrderDateKey] RANGE RIGHT FOR VALUES
                                (20000101,20010101
                                )
                            )
            )
AS
SELECT  *
FROM    [dbo].[FactInternetSales_20000101]
WHERE   [OrderDateKey] >= 20000101
AND     [OrderDateKey] <  20010101
;

ALTER TABLE dbo.FactInternetSales_20000101_20010101 SWITCH PARTITION 2 TO dbo.FactInternetSales PARTITION 2;
```

Nach Abschluss der Datenverschiebung empfiehlt es sich, die Statistiken für die Zieltabelle zu aktualisieren, um sicherzustellen, dass sie genau die neue Verteilung der Daten in den entsprechenden Partitionen wiedergeben:

```sql
UPDATE STATISTICS [dbo].[FactInternetSales];
```

### <a name="table-partitioning-source-control"></a>Datenquellen-Steuerelement für die Tabellenpartitionierung
Um ein **Rosten** der Tabellendefinition in Ihrem Quellcodeverwaltungssystem zu vermeiden, sollten Sie die folgende Vorgehensweise befolgen:

1. Erstellen der Tabelle als partitionierte Tabelle ohne Partitionswerte

```sql
CREATE TABLE [dbo].[FactInternetSales]
(
    [ProductKey]            int          NOT NULL
,   [OrderDateKey]          int          NOT NULL
,   [CustomerKey]           int          NOT NULL
,   [PromotionKey]          int          NOT NULL
,   [SalesOrderNumber]      nvarchar(20) NOT NULL
,   [OrderQuantity]         smallint     NOT NULL
,   [UnitPrice]             money        NOT NULL
,   [SalesAmount]           money        NOT NULL
)
WITH
(   CLUSTERED COLUMNSTORE INDEX
,   DISTRIBUTION = HASH([ProductKey])
,   PARTITION   (   [OrderDateKey] RANGE RIGHT FOR VALUES
                    ()
                )
)
;
```

1. `SPLIT` für die Tabelle als Teil des Bereitstellungsprozesses durch:

```sql
-- Create a table containing the partition boundaries

CREATE TABLE #partitions
WITH
(
    LOCATION = USER_DB
,   DISTRIBUTION = HASH(ptn_no)
)
AS
SELECT  ptn_no
,       ROW_NUMBER() OVER (ORDER BY (ptn_no)) as seq_no
FROM    (
        SELECT CAST(20000101 AS INT) ptn_no
        UNION ALL
        SELECT CAST(20010101 AS INT)
        UNION ALL
        SELECT CAST(20020101 AS INT)
        UNION ALL
        SELECT CAST(20030101 AS INT)
        UNION ALL
        SELECT CAST(20040101 AS INT)
        ) a
;

-- Iterate over the partition boundaries and split the table

DECLARE @c INT = (SELECT COUNT(*) FROM #partitions)
,       @i INT = 1                                 --iterator for while loop
,       @q NVARCHAR(4000)                          --query
,       @p NVARCHAR(20)     = N''                  --partition_number
,       @s NVARCHAR(128)    = N'dbo'               --schema
,       @t NVARCHAR(128)    = N'FactInternetSales' --table
;

WHILE @i <= @c
BEGIN
    SET @p = (SELECT ptn_no FROM #partitions WHERE seq_no = @i);
    SET @q = (SELECT N'ALTER TABLE '+@s+N'.'+@t+N' SPLIT RANGE ('+@p+N');');

    -- PRINT @q;
    EXECUTE sp_executesql @q;

    SET @i+=1;
END

-- Code clean-up

DROP TABLE #partitions;
```

Bei diesem Ansatz bleibt der Code in der Quellcodeverwaltung statisch und die Partitionierungsgrenzwerte können sich mit der Zeit dynamisch mit dem Warehouse entwickeln.

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen finden Sie in den Artikeln [Übersicht über Tabellen][Overview], [Tabellendatentypen][Data Types], [Verteilen einer Tabelle][Distribute], [Indizieren einer Tabelle][Index], [Verwalten von Statistiken][Statistics] und [Temporäre Tabellen][Temporary].  Weitere Informationen zu Best Practices finden Sie unter [SQL Data Warehouse – Best Practices][SQL Data Warehouse Best Practices].

<!--Image references-->

<!--Article references-->
[Overview]: ./sql-data-warehouse-tables-overview.md
[Data Types]: ./sql-data-warehouse-tables-data-types.md
[Distribute]: ./sql-data-warehouse-tables-distribute.md
[Index]: ./sql-data-warehouse-tables-index.md
[Partition]: ./sql-data-warehouse-tables-partition.md
[Statistics]: ./sql-data-warehouse-tables-statistics.md
[Temporary]: ./sql-data-warehouse-tables-temporary.md
[workload management]: ./sql-data-warehouse-develop-concurrency.md
[SQL Data Warehouse Best Practices]: ./sql-data-warehouse-best-practices.md

<!-- MSDN Articles -->
[Partitioned Tables and Indexes]: https://msdn.microsoft.com/library/ms190787.aspx
[ALTER TABLE]: https://msdn.microsoft.com/en-us/library/ms190273.aspx
[CREATE TABLE]: https://msdn.microsoft.com/library/mt203953.aspx
[partition function]: https://msdn.microsoft.com/library/ms187802.aspx
[partition scheme]: https://msdn.microsoft.com/library/ms179854.aspx


<!-- Other web references -->



<!--HONumber=Dec16_HO2-->


