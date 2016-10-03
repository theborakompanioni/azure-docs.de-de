<properties
   pageTitle="Übersicht über Tabellen in SQL Data Warehouse | Microsoft Azure"
   description="Enthält Informationen zu den ersten Schritten mit Azure SQL Data Warehouse-Tabellen."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="sonyam"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="08/04/2016"
   ms.author="sonyama;barbkess;jrj"/>

# Übersicht über Tabellen in SQL Data Warehouse

> [AZURE.SELECTOR]
- [Übersicht][]
- [Datentypen][]
- [Verteilen][]
- [Index][]
- [Partition][]
- [Statistiken][]
- [Temporär][]

Die ersten Schritte zur Erstellung von Tabellen in SQL Data Warehouse sind einfach. Die grundlegende [CREATE TABLE][]-Syntax basiert auf der allgemeinen Syntax, mit der Sie aufgrund der Arbeit mit anderen Datenbanken wahrscheinlich bereits vertraut sind. Zum Erstellen einer Tabelle müssen Sie nur der Tabelle einen Namen geben, die Spalten benennen und Datentypen für jede Spalte definieren. Wenn Sie schon Tabellen in anderen Datenbanken erstellt haben, sollte Ihnen dies bekannt vorkommen.

```sql  
CREATE TABLE Customers (FirstName VARCHAR(25), LastName VARCHAR(25))
 ``` 

Im obigen Beispiel wird eine Tabelle mit dem Namen „Customers“ mit den beiden Spalten „FirstName“ und „LastName“ erstellt. Jede Spalte ist mit dem Datentyp VARCHAR(25) definiert, bei dem die Daten auf 25 Zeichen beschränkt sind. Diese grundlegenden Attribute und weitere Attribute einer Tabelle sind größtenteils mit den Attributen anderer Datenbanken identisch. Datentypen werden für jede Spalte definiert und sorgen für die Integrität Ihrer Daten. Indizes können hinzugefügt werden, um die Leistung zu verbessern, indem eine E/A-Reduzierung erreicht wird. Eine Partitionierung kann hinzugefügt werden, um die Leistung beim Ändern von Daten zu verbessern.

Das [Umbenennen][RENAME] einer SQL Data Warehouse-Tabelle sieht wie folgt aus:

```sql  
RENAME OBJECT Customer TO CustomerOrig; 
 ```

## Verteilte Tabellen

Ein neues grundlegendes Attribut, das mit verteilten Systemen wie SQL Data Warehouse eingeführt wird, ist die **Verteilungsspalte**. Der Zweck der Verteilungsspalte lässt sich schon am Namen erkennen. Anhand dieser Spalte wird bestimmt, wie Ihre Daten im Hintergrund verteilt bzw. aufgeteilt werden. Wenn Sie eine Tabelle erstellen, ohne die Verteilungsspalte anzugeben, wird für die Tabelle automatisch die **Roundrobin**-Verteilung verwendet. In einigen Szenarien können Roundrobin-Tabellen ausreichend sein, aber durch das Definieren von Verteilungsspalten kann die Datenverschiebung bei Abfragen deutlich reduziert werden, was zu einer Leistungssteigerung führt. Weitere Informationen zur Auswahl einer Verteilungsspalte finden Sie unter [Verteilen einer Tabelle][Distribute].

## Indizieren und Partitionieren von Tabellen

Wenn Sie Erfahrung mit der Verwendung von SQL Data Warehouse gesammelt haben und die Leistung optimieren möchten, ist es ratsam, sich über den Tabellenentwurf zu informieren. Weitere Informationen finden Sie in den Artikeln [Tabellendatentypen][Data Types], [Verteilen einer Tabelle][Distribute], [Indizieren einer Tabelle][Index] und [Partitionieren einer Tabelle][Partition].

## Tabellenstatistiken

Statistiken sind sehr wichtig, um für SQL Data Warehouse die bestmögliche Leistung zu erzielen. Da SQL Data Warehouse noch nicht automatisch Statistiken für Sie erstellt und aktualisiert, wie Sie dies von Azure SQL-Datenbank kennen, ist es unter Umständen sehr wichtig, dass Sie den Artikel über [Statistiken][] lesen. Mit den darin enthaltenen Informationen können Sie sicherstellen, dass Sie für Ihre Abfragen die beste Leistung erzielen.

## Temporäre Tabellen

Temporäre Tabellen sind Tabellen, die nur für die Dauer Ihrer Anmeldung vorhanden sind und anderen Benutzern nicht angezeigt werden. Temporäre Tabellen können eine gute Möglichkeit sein, um zu verhindern, dass andere Benutzer temporäre Ergebnisse sehen, und um die Notwendigkeit von Bereinigungen zu reduzieren. Da temporäre Tabellen auch lokalen Speicher nutzen, bieten Sie für einige Vorgänge eine schnellere Leistung. Ausführlichere Informationen zu temporären Tabellen finden Sie unter [Temporäre Tabelle][Temporary].

## Externe Tabellen

Externe Tabellen, die auch als PolyBase-Tabellen bezeichnet werden, sind Tabellen, die von SQL Data Warehouse abgefragt werden können, aber auf Daten außerhalb von SQL Data Warehouse verweisen. Beispielsweise können Sie eine externe Tabelle erstellen, die auf Dateien in Azure Blob Storage verweist. Weitere Informationen zum Erstellen und Abfragen einer externen Tabelle finden Sie unter [Laden von Daten mit PolyBase][].

## Nicht unterstützte Tabellenfunktionen

SQL Data Warehouse enthält zwar viele Tabellenfunktionen, die auch für andere Datenbanken vorhanden sind, aber es gibt einige Funktionen, die noch nicht unterstützt werden. Unten ist eine Liste mit den Tabellenfunktionen angegeben, die noch nicht unterstützt werden.

| Nicht unterstützte Funktionen |
| --- |
|[Identity-Eigenschaft][] \(siehe [Assigning Surrogate Key Workaround][] \(Zuweisen von Ersatzschlüsseln – Problemumgehung))|
|Primary Key, Foreign Key, Unique und Check – [Tabelleneinschränkungen][]|
|[Eindeutige Indizes][]|
|[Berechnete Spalten][]|
|[Spalten mit geringer Dichte][]|
|[Benutzerdefinierte Typen][]|
|[Sequenz][]|
|[Trigger][]|
|[Indizierte Sichten][]|
|[Synonyme][]|

## Abfragen für die Tabellengröße

Eine einfache Möglichkeit, den jeweiligen Speicherplatz- und Zeilenverbrauch einer Tabelle in den 60 Verteilungen zu ermitteln, ist die Verwendung von [DBCC PDW\_SHOWSPACEUSED][].

```sql
DBCC PDW_SHOWSPACEUSED('dbo.FactInternetSales');
```

Die Verwendung von DBCC-Befehlen kann aber auch mit größeren Einschränkungen verbunden sein. Dynamische Verwaltungssichten (DMVs) liefern Ihnen deutlich mehr Details und ermöglichen eine viel bessere Kontrolle der Abfrageergebnisse. Erstellen Sie zunächst diese Sicht, auf die in vielen Beispielen in diesem und anderen Artikeln verwiesen wird.

```sql
CREATE VIEW dbo.vTableSizes
AS
WITH base
AS
(
SELECT 
 GETDATE()                                                             AS  [execution_time]
, DB_NAME()                                                            AS  [database_name]
, s.name                                                               AS  [schema_name]
, t.name                                                               AS  [table_name]
, QUOTENAME(s.name)+'.'+QUOTENAME(t.name)                              AS  [two_part_name]
, nt.[name]                                                            AS  [node_table_name]
, ROW_NUMBER() OVER(PARTITION BY nt.[name] ORDER BY (SELECT NULL))     AS  [node_table_name_seq]
, tp.[distribution_policy_desc]                                        AS  [distribution_policy_name]
, c.[name]                                                             AS  [distribution_column]
, nt.[distribution_id]                                                 AS  [distribution_id]
, i.[type]                                                             AS  [index_type]
, i.[type_desc]                                                        AS  [index_type_desc]
, nt.[pdw_node_id]                                                     AS  [pdw_node_id]
, pn.[type]                                                            AS  [pdw_node_type]
, pn.[name]                                                            AS  [pdw_node_name]
, di.name                                                              AS  [dist_name]
, di.position                                                          AS  [dist_position]
, nps.[partition_number]                                               AS  [partition_nmbr]
, nps.[reserved_page_count]                                            AS  [reserved_space_page_count]
, nps.[reserved_page_count] - nps.[used_page_count]                    AS  [unused_space_page_count]
, nps.[in_row_data_page_count] 
    + nps.[row_overflow_used_page_count] 
    + nps.[lob_used_page_count]                                        AS  [data_space_page_count]
, nps.[reserved_page_count] 
 - (nps.[reserved_page_count] - nps.[used_page_count]) 
 - ([in_row_data_page_count] 
         + [row_overflow_used_page_count]+[lob_used_page_count])       AS  [index_space_page_count]
, nps.[row_count]                                                      AS  [row_count]
from 
    sys.schemas s
INNER JOIN sys.tables t
    ON s.[schema_id] = t.[schema_id]
INNER JOIN sys.indexes i
    ON  t.[object_id] = i.[object_id]
    AND i.[index_id] <= 1
INNER JOIN sys.pdw_table_distribution_properties tp
    ON t.[object_id] = tp.[object_id]
INNER JOIN sys.pdw_table_mappings tm
    ON t.[object_id] = tm.[object_id]
INNER JOIN sys.pdw_nodes_tables nt
    ON tm.[physical_name] = nt.[name]
INNER JOIN sys.dm_pdw_nodes pn
    ON  nt.[pdw_node_id] = pn.[pdw_node_id]
INNER JOIN sys.pdw_distributions di
    ON  nt.[distribution_id] = di.[distribution_id]
INNER JOIN sys.dm_pdw_nodes_db_partition_stats nps
    ON nt.[object_id] = nps.[object_id]
    AND nt.[pdw_node_id] = nps.[pdw_node_id]
    AND nt.[distribution_id] = nps.[distribution_id]
LEFT OUTER JOIN (select * from sys.pdw_column_distribution_properties where distribution_ordinal = 1) cdp
    ON t.[object_id] = cdp.[object_id]
LEFT OUTER JOIN sys.columns c
    ON cdp.[object_id] = c.[object_id]
    AND cdp.[column_id] = c.[column_id]
)
, size
AS
(
SELECT
   [execution_time]
,  [database_name]
,  [schema_name]
,  [table_name]
,  [two_part_name]
,  [node_table_name]
,  [node_table_name_seq]
,  [distribution_policy_name]
,  [distribution_column]
,  [distribution_id]
,  [index_type]
,  [index_type_desc]
,  [pdw_node_id]
,  [pdw_node_type]
,  [pdw_node_name]
,  [dist_name]
,  [dist_position]
,  [partition_nmbr]
,  [reserved_space_page_count]
,  [unused_space_page_count]
,  [data_space_page_count]
,  [index_space_page_count]
,  [row_count]
,  ([reserved_space_page_count] * 8.0)                                 AS [reserved_space_KB]
,  ([reserved_space_page_count] * 8.0)/1000                            AS [reserved_space_MB]
,  ([reserved_space_page_count] * 8.0)/1000000                         AS [reserved_space_GB]
,  ([reserved_space_page_count] * 8.0)/1000000000                      AS [reserved_space_TB]
,  ([unused_space_page_count]   * 8.0)                                 AS [unused_space_KB]
,  ([unused_space_page_count]   * 8.0)/1000                            AS [unused_space_MB]
,  ([unused_space_page_count]   * 8.0)/1000000                         AS [unused_space_GB]
,  ([unused_space_page_count]   * 8.0)/1000000000                      AS [unused_space_TB]
,  ([data_space_page_count]     * 8.0)                                 AS [data_space_KB]
,  ([data_space_page_count]     * 8.0)/1000                            AS [data_space_MB]
,  ([data_space_page_count]     * 8.0)/1000000                         AS [data_space_GB]
,  ([data_space_page_count]     * 8.0)/1000000000                      AS [data_space_TB]
,  ([index_space_page_count]  * 8.0)                                   AS [index_space_KB]
,  ([index_space_page_count]  * 8.0)/1000                              AS [index_space_MB]
,  ([index_space_page_count]  * 8.0)/1000000                           AS [index_space_GB]
,  ([index_space_page_count]  * 8.0)/1000000000                        AS [index_space_TB]
FROM base
)
SELECT * 
FROM size
;
```

### Tabellenspeicherplatz – Zusammenfassung

Diese Abfrage gibt die Zeilen und den Speicherplatz nach Tabelle zurück. Diese Abfrage eignet sich hervorragend dazu, anzuzeigen, welche Tabellen die größten Tabellen sind und ob es sich um Roundrobin-Tabellen oder Tabellen mit Hashverteilung handelt. Für Tabellen mit Hashverteilung wird zudem die Verteilungsspalte angezeigt. In den meisten Fällen sollte es sich bei der größten Tabelle um eine Tabelle mit Hashverteilung mit einem gruppierten Columnstore-Index ausgeführt handeln.

```sql
SELECT 
     database_name
,    schema_name
,    table_name
,    distribution_policy_name
,	  distribution_column
,    index_type_desc
,    COUNT(distinct partition_nmbr) as nbr_partitions
,    SUM(row_count)                 as table_row_count
,    SUM(reserved_space_GB)         as table_reserved_space_GB
,    SUM(data_space_GB)             as table_data_space_GB
,    SUM(index_space_GB)            as table_index_space_GB
,    SUM(unused_space_GB)           as table_unused_space_GB
FROM 
    dbo.vTableSizes
GROUP BY 
     database_name
,    schema_name
,    table_name
,    distribution_policy_name
,	  distribution_column
,    index_type_desc
ORDER BY
    table_reserved_space_GB desc
;
```

### Tabellenspeicherplatz nach Verteilungstyp

```sql
SELECT 
     distribution_policy_name
,    SUM(row_count)                as table_type_row_count
,    SUM(reserved_space_GB)        as table_type_reserved_space_GB
,    SUM(data_space_GB)            as table_type_data_space_GB
,    SUM(index_space_GB)           as table_type_index_space_GB
,    SUM(unused_space_GB)          as table_type_unused_space_GB
FROM dbo.vTableSizes
GROUP BY distribution_policy_name
;
```

### Tabellenspeicherplatz nach Indextyp

```sql
SELECT 
     index_type_desc
,    SUM(row_count)                as table_type_row_count
,    SUM(reserved_space_GB)        as table_type_reserved_space_GB
,    SUM(data_space_GB)            as table_type_data_space_GB
,    SUM(index_space_GB)           as table_type_index_space_GB
,    SUM(unused_space_GB)          as table_type_unused_space_GB
FROM dbo.vTableSizes
GROUP BY index_type_desc
;
```

### Verteilungsspeicherplatz – Zusammenfassung

```sql
SELECT 
    distribution_id
,    SUM(row_count)                as total_node_distribution_row_count
,    SUM(reserved_space_MB)        as total_node_distribution_reserved_space_MB
,    SUM(data_space_MB)            as total_node_distribution_data_space_MB
,    SUM(index_space_MB)           as total_node_distribution_index_space_MB
,    SUM(unused_space_MB)          as total_node_distribution_unused_space_MB
FROM dbo.vTableSizes
GROUP BY     distribution_id
ORDER BY    distribution_id
;
```

## Nächste Schritte

Weitere Informationen finden Sie in den Artikeln [Tabellendatentypen][Data Types], [Verteilen einer Tabelle][Distribute], [Indizieren einer Tabelle][Index], [Partitionieren einer Tabelle][Partition], [Verwalten von Statistiken für Tabellen in SQL Data Warehouse][Statistics] und [Temporäre Tabellen][Temporary]. Weitere Informationen zu bewährten Methoden finden Sie unter [Bewährte Methoden für SQL Data Warehouse][].

<!--Image references-->

<!--Article references-->
[Übersicht]: ./sql-data-warehouse-tables-overview.md
[Data Types]: ./sql-data-warehouse-tables-data-types.md
[Datentypen]: ./sql-data-warehouse-tables-data-types.md
[Distribute]: ./sql-data-warehouse-tables-distribute.md
[Verteilen]: ./sql-data-warehouse-tables-distribute.md
[Index]: ./sql-data-warehouse-tables-index.md
[Partition]: ./sql-data-warehouse-tables-partition.md
[Statistics]: ./sql-data-warehouse-tables-statistics.md
[Statistiken]: ./sql-data-warehouse-tables-statistics.md
[Temporary]: ./sql-data-warehouse-tables-temporary.md
[Temporär]: ./sql-data-warehouse-tables-temporary.md
[Bewährte Methoden für SQL Data Warehouse]: ./sql-data-warehouse-best-practices.md
[Laden von Daten mit PolyBase]: ./sql-data-warehouse-load-from-azure-blob-storage-with-polybase.md

<!--MSDN references-->
[CREATE TABLE]: https://msdn.microsoft.com/library/mt203953.aspx
[RENAME]: https://msdn.microsoft.com/library/mt631611.aspx
[DBCC PDW\_SHOWSPACEUSED]: https://msdn.microsoft.com/library/mt204028.aspx
[Identity-Eigenschaft]: https://msdn.microsoft.com/library/ms186775.aspx
[Assigning Surrogate Key Workaround]: https://blogs.msdn.microsoft.com/sqlcat/2016/02/18/assigning-surrogate-key-to-dimension-tables-in-sql-dw-and-aps/
[Tabelleneinschränkungen]: https://msdn.microsoft.com/library/ms188066.aspx
[Berechnete Spalten]: https://msdn.microsoft.com/library/ms186241.aspx
[Spalten mit geringer Dichte]: https://msdn.microsoft.com/library/cc280604.aspx
[Benutzerdefinierte Typen]: https://msdn.microsoft.com/library/ms131694.aspx
[Sequenz]: https://msdn.microsoft.com/library/ff878091.aspx
[Trigger]: https://msdn.microsoft.com/library/ms189799.aspx
[Indizierte Sichten]: https://msdn.microsoft.com/library/ms191432.aspx
[Synonyme]: https://msdn.microsoft.com/library/ms177544.aspx
[Eindeutige Indizes]: https://msdn.microsoft.com/library/ms188783.aspx

<!--Other Web references-->

<!---HONumber=AcomDC_0810_2016-->