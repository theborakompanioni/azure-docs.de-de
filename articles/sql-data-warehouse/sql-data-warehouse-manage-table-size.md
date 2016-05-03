<properties
   pageTitle="Verwalten der Tabellengröße in Azure SQL Data Warehouse | Microsoft Azure"
   description="Ermitteln Sie die Größe Ihrer Tabellen und Partitionen." 
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
   ms.date="04/13/2016"
   ms.author="jrj;barbkess;sonyama"/>

# Verwalten der Tabellengröße in Azure SQL Data Warehouse

In diesem Artikel wird die Größe der Tabellenpartitionen angegeben, sodass Sie die Größe Ihrer Datenbank ermitteln können.


## Schritt 1: Erstellen einer Sicht, die die Tabellengröße zusammenfasst

Erstellen Sie diese Ansicht, um die Tabellen mit Datenschiefe zu identifizieren.

```sql
CREATE VIEW dbo.vPartitionSizing
AS

WITH base
AS
(
SELECT 
	SUBSTRING(@@version,34,4)																AS build_number
,	DB_NAME()																				AS database_name
,	s.name																					AS schema_name
,	t.name																					AS table_name
,	QUOTENAME(s.name)+'.'+QUOTENAME(t.name)													AS two_part_name
,	nt.name																					AS node_table_name
,   i.[type]                                                                                AS table_type
,   i.[type_desc]                                                                           AS table_type_desc
,	tp.distribution_policy_desc	                        									AS distribution_policy_name
,	pn.pdw_node_id                                                                          AS pdw_node_id
,	pn.[type]																				AS pdw_node_type
,	pn.name																					AS pdw_node_name
,   di.distribution_id                                                                      AS dist_id
,   di.name                                                                                 AS dist_name
,   di.position                                                                             AS dist_position
,	nps.partition_number																	AS partition_nmbr
,	nps.reserved_page_count																	AS reserved_space_page_count
,	nps.reserved_page_count - nps.used_page_count											AS unused_space_page_count
,	nps.in_row_data_page_count + nps.row_overflow_used_page_count + nps.lob_used_page_count	AS  data_space_page_count
,	nps.reserved_page_count 
	- (nps.reserved_page_count - nps.used_page_count) 
	- (in_row_data_page_count+row_overflow_used_page_count+lob_used_page_count)				AS index_space_page_count
,	nps.[row_count]                                                                         AS row_count
from sys.schemas s
join sys.tables t								ON	s.schema_id			= t.schema_id
JOIN sys.indexes i                              ON  t.object_id         = i.object_id
                                                AND i.index_id          <= 1
join sys.pdw_table_distribution_properties	tp	ON	t.object_id			= tp.object_id
join sys.pdw_table_mappings tm					ON	t.object_id			= tm.object_id
join sys.pdw_nodes_tables nt					ON	tm.physical_name	= nt.name
join sys.dm_pdw_nodes pn 						ON  nt.pdw_node_id		= pn.pdw_node_id
join sys.pdw_distributions di                   ON  nt.distribution_id  = di.distribution_id
join sys.dm_pdw_nodes_db_partition_stats nps	ON	nt.object_id		= nps.object_id
												AND nt.pdw_node_id		= nps.pdw_node_id
                                                AND nt.distribution_id  = nps.distribution_id
)
, size
AS
(
SELECT	build_number
,		database_name
,		schema_name
,		table_name
,		two_part_name
,		node_table_name
,       table_type
,       table_type_desc
,		distribution_policy_name
,       dist_id
,       dist_name
,       dist_position
,		pdw_node_id
,		pdw_node_type
,		pdw_node_name
,		partition_nmbr
,		reserved_space_page_count
,		unused_space_page_count
,		data_space_page_count
,		index_space_page_count
,		row_count
,		(reserved_space_page_count * 8.0)				AS reserved_space_KB
,		(reserved_space_page_count * 8.0)/1000			AS reserved_space_MB
,		(reserved_space_page_count * 8.0)/1000000		AS reserved_space_GB
,		(reserved_space_page_count * 8.0)/1000000000	AS reserved_space_TB
,		(unused_space_page_count * 8.0)					AS unused_space_KB
,		(unused_space_page_count * 8.0)/1000			AS unused_space_MB
,		(unused_space_page_count * 8.0)/1000000			AS unused_space_GB
,		(unused_space_page_count * 8.0)/1000000000		AS unused_space_TB
,		(data_space_page_count * 8.0)					AS data_space_KB
,		(data_space_page_count * 8.0)/1000				AS data_space_MB
,		(data_space_page_count * 8.0)/1000000			AS data_space_GB
,		(data_space_page_count * 8.0)/1000000000		AS data_space_TB
,		(index_space_page_count * 8.0)	    			AS index_space_KB
,		(index_space_page_count * 8.0)/1000				AS index_space_MB
,		(index_space_page_count * 8.0)/1000000			AS index_space_GB
,		(index_space_page_count * 8.0)/1000000000		AS index_space_TB
FROM	base
)
SELECT	* 
FROM	size
;
```

## Schritt 2: Abfragen der Ansicht

Nachdem Sie die Sicht erstellt haben, führen Sie diese Beispielabfragen aus, um weitere Informationen über die Größe Ihrer Tabellen zu erhalten:

### Zusammenfassung des in der Datenbank verwendeten Tabellenbereichs

```sql
SELECT
	database_name
,	SUM(row_count)				as total_row_count
,	SUM(reserved_space_MB)		as total_reserved_space_MB
,	SUM(data_space_MB)			as total_data_space_MB
,	SUM(index_space_MB)			as total_index_space_MB
,	SUM(unused_space_MB)		as total_unused_space_MB
FROM dbo.vPartitionSizing
GROUP BY database_name
;
```

### Genutzter Tabellenbereich pro Tabelle

```sql
SELECT 
	two_part_name
,	SUM(row_count)				as table_row_count
,	SUM(reserved_space_GB)		as table_reserved_space_GB
,	SUM(data_space_GB)			as table_data_space_GB
,	SUM(index_space_GB)			as table_index_space_GB
,	SUM(unused_space_GB)		as table_unused_space_GB
FROM dbo.vPartitionSizing
GROUP BY two_part_name
;
```

### Tabellenbereich nach Tabellengeometrietyp

```sql
SELECT 
   table_type_desc
,	SUM(row_count)				as table_type_row_count
,	SUM(reserved_space_GB)		as table_type_reserved_space_GB
,	SUM(data_space_GB)			as table_type_data_space_GB
,	SUM(index_space_GB)			as table_type_index_space_GB
,	SUM(unused_space_GB)		as table_type_unused_space_GB
FROM dbo.vPartitionSizing
GROUP BY table_type_desc
;
```

### Tabellenbereich nach Verteilung

```sql
SELECT 
	pdw_node_id
,	distribution_policy_name
,	dist_name
,	SUM(row_count)				as total_node_distribution_row_count
,	SUM(reserved_space_MB)		as total_node_distribution_reserved_space_MB
,	SUM(data_space_MB)			as total_node_distribution_data_space_MB
,	SUM(index_space_MB)			as total_node_distribution_index_space_MB
,	SUM(unused_space_MB)		as total_node_distribution_unused_space_MB
FROM dbo.vPartitionSizing
GROUP BY 	distribution_policy_name
,			dist_name
,			pdw_node_id
ORDER BY    pdw_node_id
,           distribution_policy_name
,		    dist_name
;
```

## Schritt 3: Verwalten der Tabellen

Haben Sie einige Tabellen gefunden, die Sie nicht mehr brauchen? Löschen Sie sie, oder exportieren Sie sie mit [CREATE EXTERNAL TABLE AS SELECT (CETAS)][] in den Blobspeicher.

Waren einige Tabellen größer als Sie dachten? Möglicherweise möchten Sie sie partitionieren. Weitere Informationen hierzu finden Sie im Artikel zu [Tabellenpartitionen][].

Nähern Sie sich der konfigurierten maximalen Größe für Ihre Datenbank? Verwenden Sie [ALTER DATABASE][], um sie zu ändern.

## Nächste Schritte
Weitere Details zum Thema „Tabellenverteilung“ finden Sie in den folgenden Artikeln:

* [Tabellenentwurf][]
* [Hashverteilung][]

<!--Image references-->

<!--Article references-->
[Tabellenentwurf]: sql-data-warehouse-develop-table-design.md
[Hashverteilung]: sql-data-warehouse-develop-hash-distribution-key.md

<!--MSDN references-->

<!--Other Web references-->

<!---HONumber=AcomDC_0420_2016-->