<properties
   pageTitle="Verwalten von Datenschiefe für verteilte Tabellen in Azure SQL Data Warehouse | Microsoft Azure"
   description="Finden und beseitigen Sie Datenschiefe, wenn Zeilen ungleichmäßig über alle Verteilungen einer verteilten Hashtabelle in Azure SQL Data Warehouse verteilt sind." 
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="jrowlandjones"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="04/14/2016"
   ms.author="jrj;barbkess;sonyama"/>

# Verwalten von Datenschiefe für verteilte Tabellen in Azure SQL Data Warehouse
In diesem Tutorial wird Datenschiefe in Ihren verteilten Hashtabellen identifiziert, und Sie erhalten Vorschläge zur Behebung des Problems.

Wenn Tabellendaten mithilfe der Hashverteilungsmethode verteilt werden, besteht die Möglichkeit, dass einige Verteilungen ungleichmäßig sind und im Vergleich mit anderen übermäßig viele Daten enthalten. Eine übermäßige Datenschiefe kann die Abfrageleistung beeinträchtigen, da das Endergebnis einer verteilten Abfrage erst bereit steht, wenn die Verteilung mit der längsten Ausführungsdauer beendet wurde. Abhängig vom Grad der Datenschiefe sollten Sie diese beheben.

In diesem Lernprogramm führen Sie folgende Schritte aus:

- Verwenden von Metadaten, um die Tabellen mit Datenschiefe zu bestimmen
- Kennenlernen von Tipps, um herauszufinden, wann die Datenschiefe beseitigt werden muss
- Neuerstellen der Tabelle zur Beseitigung der Datenschiefe

## DBCC PDW\_SHOWSPACEUSED

Eine Methode zur Identifizierung von Datenschiefe ist die Verwendung von [DBCC PDW\_SHOWSPACEUSED()][].

```sql
-- Find data skew for a distributed table
DBCC PDW_SHOWSPACEUSED('dbo.FactInternetSales');
```

Dies ist eine sehr schnelle und einfache Möglichkeit, die Anzahl der Tabellenzeilen anzuzeigen, die in jeder der 60 Verteilungen Ihrer Datenbank gespeichert sind. Denken Sie daran, dass für eine optimale Leistung die Zeilen in Ihrer verteilten Tabelle gleichmäßig auf alle Verteilungen verteilt sein sollten.

Wenn Sie jedoch die dynamischen Verwaltungsansichten (Dynamic Management Views, DMV) von Azure SQL Data Warehouse abfragen, können Sie eine ausführlichere Analyse ausführen. Im übrigen Teil des Artikels erfahren Sie, wie Sie dabei vorgehen.

## Schritt 1: Erstellen einer Ansicht, mit der die Datenschiefe gefunden wird

Erstellen Sie diese Ansicht, um die Tabellen mit Datenschiefe zu identifizieren.

```sql
CREATE VIEW dbo.vDistributionSkew
AS
WITH base
AS
(
SELECT 
	SUBSTRING(@@version,34,4)															AS  [build_number]
,	GETDATE()																			AS  [execution_time]
,	DB_NAME()																			AS  [database_name]
,	s.name																				AS  [schema_name]
,	t.name																				AS  [table_name]
,	QUOTENAME(s.name)+'.'+QUOTENAME(t.name)												AS  [two_part_name]
,	nt.[name]																			AS  [node_table_name]
,	ROW_NUMBER() OVER(PARTITION BY nt.[name] ORDER BY (SELECT NULL))					AS  [node_table_name_seq]
,	tp.[distribution_policy_desc]														AS  [distribution_policy_name]
,	nt.[distribution_id]																AS  [distribution_id]
,	nt.[pdw_node_id]																	AS  [pdw_node_id]
,	pn.[type]																			AS	[pdw_node_type]
,	pn.[name]																			AS	[pdw_node_name]
,	nps.[partition_number]																AS	[partition_nmbr]
,	nps.[reserved_page_count]															AS	[reserved_space_page_count]
,	nps.[reserved_page_count] - nps.[used_page_count]									AS	[unused_space_page_count]
,	nps.[in_row_data_page_count] 
	+ nps.[row_overflow_used_page_count] + nps.[lob_used_page_count]					AS  [data_space_page_count]
,	nps.[reserved_page_count] 
	- (nps.[reserved_page_count] - nps.[used_page_count]) 
	- ([in_row_data_page_count]+[row_overflow_used_page_count]+[lob_used_page_count])	AS  [index_space_page_count]
,	nps.[row_count]																		AS  [row_count]
from sys.schemas s
join sys.tables t								ON	s.[schema_id]			= t.[schema_id]
join sys.pdw_table_distribution_properties	tp	ON	t.[object_id]			= tp.[object_id]
join sys.pdw_table_mappings tm					ON	t.[object_id]			= tm.[object_id]
join sys.pdw_nodes_tables nt					ON	tm.[physical_name]		= nt.[name]
join sys.dm_pdw_nodes pn 						ON  nt.[pdw_node_id]		= pn.[pdw_node_id]
join sys.dm_pdw_nodes_db_partition_stats nps	ON	nt.[object_id]			= nps.[object_id]
												AND nt.[pdw_node_id]		= nps.[pdw_node_id]
												AND nt.[distribution_id]	= nps.[distribution_id]
)
, size
AS
(
SELECT	[build_number]
,		[execution_time]
,		[database_name]
,		[schema_name]
,		[table_name]
,		[two_part_name]
,		[node_table_name]
,		[node_table_name_seq]
,		[distribution_policy_name]
,		[distribution_id]
,		[pdw_node_id]
,		[pdw_node_type]
,		[pdw_node_name]
,		[partition_nmbr]
,		[reserved_space_page_count]
,		[unused_space_page_count]
,		[data_space_page_count]
,		[index_space_page_count]
,		[row_count]
,		([reserved_space_page_count] * 8.0)				AS [reserved_space_KB]
,		([reserved_space_page_count] * 8.0)/1000		AS [reserved_space_MB]
,		([reserved_space_page_count] * 8.0)/1000000		AS [reserved_space_GB]
,		([reserved_space_page_count] * 8.0)/1000000000	AS [reserved_space_TB]
,		([unused_space_page_count]   * 8.0)				AS [unused_space_KB]
,		([unused_space_page_count]   * 8.0)/1000		AS [unused_space_MB]
,		([unused_space_page_count]   * 8.0)/1000000		AS [unused_space_GB]
,		([unused_space_page_count]   * 8.0)/1000000000	AS [unused_space_TB]
,		([data_space_page_count]     * 8.0)				AS [data_space_KB]
,		([data_space_page_count]     * 8.0)/1000		AS [data_space_MB]
,		([data_space_page_count]     * 8.0)/1000000		AS [data_space_GB]
,		([data_space_page_count]     * 8.0)/1000000000	AS [data_space_TB]
,		([index_space_page_count]	 * 8.0)				AS [index_space_KB]
,		([index_space_page_count]	 * 8.0)/1000		AS [index_space_MB]
,		([index_space_page_count]	 * 8.0)/1000000		AS [index_space_GB]
,		([index_space_page_count]	 * 8.0)/1000000000	AS [index_space_TB]
FROM	base
)
SELECT	* 
FROM	size
;
```

## Schritt 2: Abfragen der Ansicht

Nachdem Sie die Ansicht erstellt haben, führen Sie diese Beispielabfrage aus, um die Tabellen mit Datenschiefe zu bestimmen.

```sql
SELECT	[two_part_name]
,		[distribution_id]
,		[row_count]
,		[reserved_space_GB]
,		[unused_space_GB]
,		[data_space_GB]
,		[index_space_GB]
FROM	[dbo].[vDistributionSkew]
WHERE	[table_name] = 'FactInternetSales'
ORDER BY [row_count] DESC
```

>[AZURE.NOTE] In mit ROUND\_ROBIN verteilte Tabellen sollte es nicht zu einer ungleichmäßigen Verteilung kommen. Daten werden standardmäßig einheitlich auf die Knoten verteilt.

## Schritt 3: Entscheiden, ob die Datenschiefe beseitigt werden sollte

Um zu entscheiden, ob die Datenschiefe in einer Tabelle beseitigt werden sollte, müssen Sie die Datenmenge und die Abfragen in Ihrer Workload möglichst umfassend verstehen.

Verteilen von Daten bedeutet, das richtige Gleichgewicht zwischen der Minimierung der Datenschiefe und der Minimierung von Datenverschiebungen zu finden. Dies können gegensätzliche Ziele sein, und manchmal könnten Sie die Datenschiefe beibehalten, um Datenverschiebungen zu reduzieren. Beispiel: Wenn die Verteilungsspalte häufig die freigegebene Spalte in Verknüpfungen und Aggregationen ist, minimieren Sie Datenverschiebungen. Der Vorteil durch minimale Datenverschiebungen überwiegt möglicherweise die Auswirkungen der Datenschiefe.

## Schritt 4: Beseitigen der Datenschiefe

Es gibt zwei Arten, die Datenschiefe zu beseitigen. Verwenden Sie eine davon, wenn Sie entschieden haben, dass Sie die Datenschiefe beseitigen möchten.

### Methode 1: Neuerstellen der Tabelle mit einer anderen Verteilungsspalte

Das übliche Verfahren zum Beseitigen von Datenschiefe ist, die Tabelle mit einer anderen Verteilungsspalte neu zu erstellen. Hilfestellung bei der Auswahl einer Hashverteilungsspalte finden Sie unter [Hashverteilung][]. In diesem Beispiel wird [CTAS][] verwendet, um eine Tabelle mit einer anderen Verteilungsspalte neu zu erstellen.

```sql
CREATE TABLE [dbo].[FactInternetSales_CustomerKey] 
WITH (  CLUSTERED COLUMNSTORE INDEX
     ,  DISTRIBUTION =  HASH([CustomerKey])
     ,  PARTITION       ( [OrderDateKey] RANGE RIGHT FOR VALUES (   20000101, 20010101, 20020101, 20030101
                                                                ,   20040101, 20050101, 20060101, 20070101
                                                                ,   20080101, 20090101, 20100101, 20110101
                                                                ,   20120101, 20130101, 20140101, 20150101
                                                                ,   20160101, 20170101, 20180101, 20190101
                                                                ,   20200101, 20210101, 20220101, 20230101
                                                                ,   20240101, 20250101, 20260101, 20270101
                                                                ,   20280101, 20290101
                                                                )
                        )
    )
AS
SELECT  *
FROM    [dbo].[FactInternetSales]
OPTION  (LABEL  = 'CTAS : FactInternetSales_CustomerKey')
;

--Rename the objects
RENAME OBJECT [dbo].[FactInternetSales] TO [FactInternetSales_ProductKey];
RENAME OBJECT [dbo].[FactInternetSales_CustomerKey] TO [FactInternetSales];
```

### Methode 2: Erstellen der Tabelle mithilfe der ROUNDROBIN-Verteilung

In diesem Beispiel wird die Tabelle mithilfe von ROUNDROBIN statt der Hashverteilung neu erstellt. Diese Änderung führt zu einer gleichmäßigen Datenverteilung. Normalerweise nehmen dadurch jedoch die Datenverschiebungen für Abfragen zu.

```sql
CREATE TABLE [dbo].[FactInternetSales_ROUND_ROBIN] 
WITH (  CLUSTERED COLUMNSTORE INDEX
     ,  DISTRIBUTION =  ROUND_ROBIN
     ,  PARTITION       ( [OrderDateKey] RANGE RIGHT FOR VALUES (   20000101, 20010101, 20020101, 20030101
                                                                ,   20040101, 20050101, 20060101, 20070101
                                                                ,   20080101, 20090101, 20100101, 20110101
                                                                ,   20120101, 20130101, 20140101, 20150101
                                                                ,   20160101, 20170101, 20180101, 20190101
                                                                ,   20200101, 20210101, 20220101, 20230101
                                                                ,   20240101, 20250101, 20260101, 20270101
                                                                ,   20280101, 20290101
                                                                )
                        )
    )
AS
SELECT  *
FROM    [dbo].[FactInternetSales]
OPTION  (LABEL  = 'CTAS : FactInternetSales_ROUND_ROBIN')
;

--Rename the objects
RENAME OBJECT [dbo].[FactInternetSales] TO [FactInternetSales_HASH];
RENAME OBJECT [dbo].[FactInternetSales_ROUND_ROBIN] TO [FactInternetSales];
```

## Nächste Schritte
Weitere Details zum Thema „Tabellenverteilung“ finden Sie in den folgenden Artikeln:

* [Tabellenentwurf][]
* [Hashverteilung][]

<!--Image references-->

<!--Article references-->
[Tabellenentwurf]: sql-data-warehouse-develop-table-design.md
[Hashverteilung]: sql-data-warehouse-develop-hash-distribution-key.md

<!--MSDN references-->
[DBCC PDW\_SHOWSPACEUSED()]: https://msdn.microsoft.com/de-DE/library/mt204028.aspx

<!--Other Web references-->

<!---HONumber=AcomDC_0420_2016-->