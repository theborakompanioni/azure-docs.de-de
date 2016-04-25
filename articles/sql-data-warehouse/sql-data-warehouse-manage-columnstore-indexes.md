<properties
   pageTitle="Verwalten von Columnstore-Indizes in Azure SQL Data Warehouse | Microsoft Azure"
   description="Tutorial für die Verwaltung von Columnstore-Indizes zum Verbessern der Komprimierung und Abfrageleistung in Azure SQL Data Warehouse."
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
   ms.date="04/07/2016"
   ms.author="jrj;barbkess;sonyama"/>

# Verwalten von Columnstore-Indizes in Azure SQL Data Warehouse

In diesem Tutorial wird erklärt, wie Columnstore-Indizes zur Verbesserung der Leistung von Abfragen verwaltet werden.

Die Leistung von Abfragen für Columnstore-Indizes ist am besten, wenn der Index Zeilen in „Zeilengruppen“ mit einer Million Zeilen (bzw. genau 1.048.576) komprimiert. Dadurch wird die höchste Komprimierung und die beste Abfrageleistung erreicht. Jedoch können Situationen auftreten, die dazu führen, dass die Zeilengruppen wesentlich weniger als eine Million Zeilen enthalten. Wenn Zeilengruppen nicht dicht mit Zeilen gefüllt werden, sollten Sie Anpassungen vornehmen.

In diesem Lernprogramm lernen Sie Folgendes:

- Verwenden von Metadaten, um die durchschnittliche Anzahl von Zeilen pro Zeilengruppe zu bestimmen
- Untersuchen der Grundursache für spärlich gefüllte Zeilengruppen
- Neuerstellen eines Columnstore-Index, um alle Zeilen erneut in neuen Zeilengruppen zu komprimieren 

Die Grundlagen zu Columnstore-Indizes finden Sie unter [Beschreibung von Columnstore-Indizes](https://msdn.microsoft.com/library/gg492088.aspx).

## Schritt 1: Erstellen einer Ansicht, in der Metadaten von Zeilengruppen angezeigt werden

In dieser Ansicht werden die durchschnittlichen Zeilen pro Zeilengruppe berechnet. Sie zeigt zudem weitere Informationen zu Zeilengruppen an.

```sql
CREATE VIEW dbo.vColumnstoreDensity
AS
WITH CSI
AS
(
SELECT
        SUBSTRING(@@version,34,4)                                               AS [build_number]
,       GETDATE()                                                               AS [execution_date]
,       DB_Name()                                                               AS [database_name]
,       t.name                                                                  AS [table_name]
,		COUNT(DISTINCT rg.[partition_number])									AS [table_partition_count]
,       SUM(rg.[total_rows])                                                    AS [row_count_total]
,       SUM(rg.[total_rows])/COUNT(DISTINCT rg.[distribution_id])               AS [row_count_per_distribution_MAX]
,		CEILING	(	(SUM(rg.[total_rows])*1.0/COUNT(DISTINCT rg.[distribution_id])
						)/1048576
				)																AS [rowgroup_per_distribution_MAX]
,       SUM(CASE WHEN rg.[State] = 1 THEN 1                   ELSE 0    END)    AS [OPEN_rowgroup_count]
,       SUM(CASE WHEN rg.[State] = 1 THEN rg.[total_rows]     ELSE 0    END)    AS [OPEN_rowgroup_rows]
,       MIN(CASE WHEN rg.[State] = 1 THEN rg.[total_rows]     ELSE NULL END)    AS [OPEN_rowgroup_rows_MIN]
,       MAX(CASE WHEN rg.[State] = 1 THEN rg.[total_rows]     ELSE NULL END)    AS [OPEN_rowgroup_rows_MAX]
,       AVG(CASE WHEN rg.[State] = 1 THEN rg.[total_rows]     ELSE NULL END)    AS [OPEN_rowgroup_rows_AVG]
,       SUM(CASE WHEN rg.[State] = 2 THEN 1                   ELSE 0    END)    AS [CLOSED_rowgroup_count]
,       SUM(CASE WHEN rg.[State] = 2 THEN rg.[total_rows]     ELSE 0    END)    AS [CLOSED_rowgroup_rows]
,       MIN(CASE WHEN rg.[State] = 2 THEN rg.[total_rows]     ELSE NULL END)    AS [CLOSED_rowgroup_rows_MIN]
,       MAX(CASE WHEN rg.[State] = 2 THEN rg.[total_rows]     ELSE NULL END)    AS [CLOSED_rowgroup_rows_MAX]
,       AVG(CASE WHEN rg.[State] = 2 THEN rg.[total_rows]     ELSE NULL END)    AS [CLOSED_rowgroup_rows_AVG]
,       SUM(CASE WHEN rg.[State] = 3 THEN 1                   ELSE 0    END)    AS [COMPRESSED_rowgroup_count]
,       SUM(CASE WHEN rg.[State] = 3 THEN rg.[total_rows]     ELSE 0    END)    AS [COMPRESSED_rowgroup_rows]
,       SUM(CASE WHEN rg.[State] = 3 THEN rg.[deleted_rows]   ELSE 0    END)    AS [COMPRESSED_rowgroup_rows_DELETED]
,       MIN(CASE WHEN rg.[State] = 3 THEN rg.[total_rows]     ELSE NULL END)    AS [COMPRESSED_rowgroup_rows_MIN]
,       MAX(CASE WHEN rg.[State] = 3 THEN rg.[total_rows]     ELSE NULL END)    AS [COMPRESSED_rowgroup_rows_MAX]
,       AVG(CASE WHEN rg.[State] = 3 THEN rg.[total_rows]     ELSE NULL END)    AS [COMPRESSED_rowgroup_rows_AVG]
FROM    sys.[pdw_nodes_column_store_row_groups] rg
JOIN    sys.[pdw_nodes_tables] nt                   ON  rg.[object_id]          = nt.[object_id]
                                                    AND rg.[pdw_node_id]        = nt.[pdw_node_id]
                                                    AND rg.[distribution_id]    = nt.[distribution_id]
JOIN    sys.[pdw_table_mappings] mp                 ON  nt.[name]               = mp.[physical_name]
JOIN    sys.[tables] t                              ON  mp.[object_id]          = t.[object_id]
GROUP BY
        t.[name]
)
SELECT  *
FROM    CSI
;
```

## Schritt 2: Abfragen der Ansicht

Nachdem Sie die Ansicht erstellt haben, führen Sie diese Beispielabfrage aus, um die Metadaten von Zeilengruppen für den Columnstore-Index anzuzeigen.

```sql
SELECT	[table_name]
,		[table_partition_count]
,		[row_count_total]
,		[row_count_per_distribution_MAX]
,		[COMPRESSED_rowgroup_rows]
,		[COMPRESSED_rowgroup_rows_AVG]
,		[COMPRESSED_rowgroup_rows_DELETED]
,		[COMPRESSED_rowgroup_count]
,		[OPEN_rowgroup_count]
,		[OPEN_rowgroup_rows]
,		[CLOSED_rowgroup_count]
,		[CLOSED_rowgroup_rows]
FROM	[dbo].[vColumnstoreDensity]
WHERE	[table_name] = 'FactInternetSales'
```

## Schritt 3: Analysieren der Ergebnisse

Nachdem Sie die Abfrage ausgeführt haben, können Sie die Daten anzeigen und die Ergebnisse analysieren. In dieser Tabelle wird erläutert, auf was Sie bei der Zeilengruppenanalyse achten sollten.


| Column | Verwendung dieser Daten |
| ---------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| [table\_partition\_count] | Wenn die Tabelle partitioniert ist, könnten Sie einen höheren Wert für die Anzahl der offenen Zeilengruppen erwarten. Jeder Partition in der Verteilung könnte theoretisch eine offene Zeilengruppe zugeordnet sein. Berücksichtigen Sie dies in der Analyse. Eine kleine Tabelle, die partitioniert wurde, könnte durch das vollständige Entfernen der Partitionierung optimiert werden, da dies die Komprimierung verbessern würde. |
| [row\_count\_total] | Gesamtanzahl der Zeilen für die Tabelle. Mit diesem Wert können Sie z. B. den Prozentsatz der Zeilen im komprimierten Zustand berechnen. |
| [row\_count\_per\_distribution\_MAX] | Wenn alle Zeilen gleichmäßig verteilt sind, wäre dieser Wert die Zielanzahl von Zeilen pro Verteilung. Vergleichen Sie diesen Wert mit „compressed\_rowgroup\_count“. |
| [COMPRESSED\_rowgroup\_rows] | Gesamtanzahl der Zeilen im Columnstore-Format für die Tabelle. |
| [COMPRESSED\_rowgroup\_rows\_AVG] | Wenn für eine Zeilengruppe die durchschnittliche Anzahl von Zeilen erheblich kleiner als die maximale Anzahl von Zeilen ist, sollten Sie CTAS oder ALTER INDEX REBUILD zum erneuten Komprimieren der Daten verwenden. |
| [COMPRESSED\_rowgroup\_count] | Die Anzahl von Zeilengruppen im Columnstore-Format. Wenn diese Anzahl in Relation zur Tabelle sehr hoch ist, ist dies ein Indikator dafür, dass die Columnstore-Dichte gering ist. |
| [COMPRESSED\_rowgroup\_rows\_DELETED] | Zeilen werden im Columnstore-Format logisch gelöscht. Wenn die Anzahl in Relation zur Tabellengröße hoch ist, sollten Sie die Partition oder den Index neu erstellen, da sie dadurch physisch entfernt werden. |
| [COMPRESSED\_rowgroup\_rows\_MIN] | Verwenden Sie diese Spalte zusammen mit den Spalten AVG und MAX, um den Wertebereich für die Zeilengruppen im Columnstore zu verstehen. Eine geringe Anzahl über dem Schwellenwert für das Laden (102.400 pro nach Verteilung ausgerichteter Partition) zeigt an, dass Optimierungen beim Laden von Daten möglich sind. |
| [COMPRESSED\_rowgroup\_rows\_MAX] | Wie oben. |
| [OPEN\_rowgroup\_count] | Offene Zeilengruppen sind normal. Sie sollten sinnvollerweise eine OPEN-Zeilengruppe pro Tabellenverteilung (60) erwarten. Eine sehr große Anzahl deutet auf das partitionsübergreifende Laden von Daten hin. Überprüfen Sie die Partitionierungsstrategie, um sicherzustellen, dass sie fehlerfrei ist. |
| [OPEN\_rowgroup\_rows] | Jede Zeilengruppe kann maximal 1.048.576 Zeilen enthalten. Verwenden Sie diesen Wert, um festzustellen, wie voll die offenen Zeilengruppen derzeit sind. |
| [OPEN\_rowgroup\_rows\_MIN] | Offene Gruppen geben an, dass Daten langsam in die Tabelle geladen werden oder dass beim vorherigen Laden verbleibende Zeilen in diese Zeilengruppe aufgenommen wurden. Verwenden die Spalten MIN, MAX und AVG, um festzustellen, wie viele Daten sich in OPEN-Zeilengruppen befinden. Bei kleinen Tabellen können es 100 % aller Daten sein! Verwenden Sie in diesem Fall ALTER INDEX REBUILD, um die Datenaufnahme in einen Columnstore zu erzwingen. |
| [OPEN\_rowgroup\_rows\_MAX] | Wie oben. |
| [OPEN\_rowgroup\_rows\_AVG] | Wie oben. |
| [CLOSED\_rowgroup\_rows] | Betrachten Sie die Zeilen in geschlossenen Zeilengruppen als Integritätsprüfung. |
| [CLOSED\_rowgroup\_count] | Die Anzahl der geschlossenen Zeilengruppen sollte niedrig sein, wenn überhaupt welche vorhanden werden. Geschlossene Zeilengruppen können mit dem Befehl ALTER INDEX ... REORGANISE in komprimierte Zeilengruppen konvertiert werden. Dies ist jedoch normalerweise nicht erforderlich. Geschlossene Gruppen werden automatisch durch den Tupelverschiebungsvorgang im Hintergrund in Columnstore-Zeilengruppen konvertiert. |
| [CLOSED\_rowgroup\_rows\_MIN] | Geschlossene Zeilengruppen sollten eine sehr hohe Füllrate aufweisen. Wenn die Füllrate für eine geschlossene Zeilengruppe niedrig ist, ist eine weitere Analyse des Columnstore erforderlich. |
| [CLOSED\_rowgroup\_rows\_MAX] | Wie oben. |
| [CLOSED\_rowgroup\_rows\_AVG] | Wie oben. |


## Schritt 4: Untersuchen der Grundursache

Durch die Untersuchung der Grundursache erfahren Sie, ob Sie Änderungen am Tabellenentwurf, an Ladevorgängen oder an anderen Prozessen vornehmen können, die die Zeilendichte in den Zeilengruppen verbessern. Auf diese Weise verbessern Sie auch die Komprimierung und Leistung von Abfragen.

Durch diese Faktoren kann ein Columnstore-Index deutlich weniger als 1.048.576 Zeilen pro Zeilengruppe enthalten. Sie können auch verursachen, dass Zeilen in die Deltazeilengruppe statt in eine komprimierte Zeilengruppe aufgenommen werden.

1. Intensive DML-Vorgänge
2. Kleine oder langsame Ladevorgänge
3. Zu viele Partitionen

### Intensive DML-Vorgänge** 

Intensive DML-Vorgänge, mit denen Zeilen aktualisiert und gelöscht werden, sorgen für Ineffizienz im Columnstore. Dies gilt insbesondere, wenn der Großteil der Zeilen in einer Zeilengruppe geändert wird.

- Das Löschen einer Zeile aus einer komprimierten Zeilengruppe kennzeichnet die Zeile nur logisch als gelöscht. Die Zeile verbleibt in der komprimierten Zeilengruppe, bis die Partition oder Tabelle erneut erstellt wird.
- Das Einfügen einer Zeile fügt die Zeile einer internen Rowstore-Tabelle hinzu, die Deltazeilengruppe genannt wird. Die eingefügte Zeile wird erst in das Columnstore-Format konvertiert, wenn die Deltazeilengruppe voll und als geschlossen markiert ist. Zeilengruppen werden geschlossen, sobald sie die maximale Kapazität von 1.048.576 Zeilen erreichen. 
- Das Aktualisieren einer Zeile im Columnstore-Format wird als logisches Löschen und anschließendes Einfügen verarbeitet. Die eingefügte Zeile wird im Deltaspeicher gespeichert.

Als Batch ausgeführte Aktualisierungs- und Einfügevorgänge, die den Massenschwellenwert von 102.400 Zeilen pro nach Partition ausgerichteter Verteilung überschreiten, werden direkt in das Columnstore-Format geschrieben. Bei einer gleichmäßigen Verteilung müssten Sie allerdings 6,144 Millionen Zeilen in einem einzigen Vorgang ändern, damit dies eintritt. Wenn die Anzahl der Zeilen für eine bestimmte nach Partition ausgerichtete Verteilung geringer als 102.400 Zeilen ist, werden die Zeilen in den Deltaspeicher aufgenommen, bis genügend Zeilen eingefügt oder geändert wurden, um die Zeilengruppe zu schließen, oder bis der Index neu erstellt wird.

### Kleine oder langsame Ladevorgänge

Kleine Ladevorgänge in SQL Data Warehouse werden manchmal auch als langsame Ladevorgänge bezeichnet. Sie stellen in der Regel einen annähernd konstanten Datenstrom dar, der vom System erfasst wird. Dieser Datenstrom ist zwar fast kontinuierlich, die Anzahl der Zeilen ist jedoch nicht besonders groß. In den meisten Fällen liegt die Datenmenge deutlich unter dem Schwellenwert, der für ein direktes Laden in das Columnstore-Format erforderlich ist.

In diesen Situationen ist es oft besser, die Daten zunächst in den Azure-Blobspeicher zu laden, damit sie sich vor dem Laden ansammeln können. Diese Technik wird auch als *Verarbeitung von Mikro-Batches* bezeichnet.

### Zu viele Partitionen

Sie haben möglicherweise zu viele Partitionen. In der MPP-Architektur (Massively Parallel Processing) wird eine benutzerdefinierte Tabelle verteilt und als 60 Tabellen implementiert. Daher wird jeder Columnstore-Index als 60 Columnstore-Indizes implementiert. Ebenso wird jede Partition über diese 60 Columnstore-Indizes implementiert. Dies ist ein **Hauptunterschied** zur SMP-Architektur (symmetrischer Multiprozessor) von SQL Server.

Wenn Sie 1.048.576 Zeilen in eine Partition einer SQL Server-Tabelle mit SMP-Architektur laden, wird sie in den Columnstore komprimiert. Wenn Sie jedoch 1.048.576 Zeilen in eine Partition einer SQL Data Warehouse-Tabelle laden, gelangen nur 17.467 Zeilen in jede der 60 Verteilungen (bei einer gleichmäßigen Verteilung der Daten). Da 17.467 unter dem Schwellenwert von 102.400 Zeilen pro Verteilung liegt, speichert SQL Data Warehouse die Daten in Zeilengruppen des Deltaspeichers. Um Zeilen direkt im komprimierten Columnstore-Format zu laden, müssen Sie daher mehr als 6,1444 Millionen Zeilen (60 x 102.400) in eine einzelne Partition einer SQL Data Warehouse-Tabelle laden.

## Schritt 5: Zuordnen zusätzlicher Computeressourcen

Für das Neuerstellen von Indizes – vor allem bei größeren Tabellen – sind häufig zusätzliche Ressourcen erforderlich. SQL Data Warehouse bietet eine Funktion für die Workloadverwaltung, mit der einem Benutzer mehr Speicherplatz zugeordnet werden kann. Informationen zum Reservieren von mehr Speicherplatz für die Neuerstellung des Index finden Sie im Abschnitt zur Workloadverwaltung im Artikel [Parallelität][].

## Schritt 6: Neuerstellen des Index zur Verbesserung der durchschnittlichen Zeilen pro Zeilengruppe

Zum Zusammenführen vorhandener komprimierter Zeilengruppen und zum Erzwingen der Aufnahme von Deltazeilengruppen in den Columnstore müssen Sie den Index neu erstellen. Meist sind zu viele Daten vorhanden, um den gesamten Index neu zu erstellen. Daher empfiehlt es sich, eine oder mehrere Partitionen neu zu erstellen. In SQL Data Warehouse können Sie den Index nach einem der folgenden beiden Verfahren neu erstellen:

1. Verwenden Sie [ALTER INDEX][], um eine Partition neu zu erstellen.
2. Verwenden Sie [CTAS][], um eine Partition in einer neuen Tabelle neu zu erstellen. Nutzen Sie dann den Partitionswechsel, um die Partition wieder in die ursprüngliche Tabelle zu verschieben.

Welche Methode ist am besten geeignet? Für große Datenmengen ist [CTAS][] in der Regel schneller als [ALTER INDEX][]. Für kleinere Datenmengen ist [ALTER INDEX][] einfacher zu verwenden.

### Methode 1: Verwenden von ALTER INDEX, um kleine Datenmengen offline neu zu erstellen

Diese Beispiele zeigen, wie Sie den gesamten Columnstore-Index neu erstellen und eine einzelne Partition neu erstellen. Bei großen Tabellen ist es nur zweckmäßig, eine einzelne Partition neu erstellen. Dies ist ein Offlinevorgang.

```sql
-- Rebuild the entire clustered index
ALTER INDEX ALL ON [dbo].[DimProduct] REBUILD
```

```sql
-- Rebuild a single partition
ALTER INDEX ALL ON [dbo].[FactInternetSales] REBUILD Partition = 5
```

Weitere Informationen finden Sie im Abschnitt zu ALTER INDEX REBUILD in [Columnstore Indexes Defragmentation](https://msdn.microsoft.com/library/dn935013.aspx#Anchor_1) (Columnstore-Indexdefragmentierung) und im Syntaxthema [ALTER INDEX (Transact-SQL)](https://msdn.microsoft.com/library/ms188388.aspx).

### Methode 2: Verwenden von CTAS, um große Datenmengen online neu zu erstellen und Partitionswechsel durchzuführen

In diesem Beispiel werden [CTAS][] und Partitionswechsel verwendet, um eine Tabellenpartition neu zu erstellen.


```sql
-- Step 01. Select the partition of data and write it out to a new table using CTAS
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
FROM    [dbo].[FactInternetSales]
WHERE   [OrderDateKey] >= 20000101
AND     [OrderDateKey] <  20010101
;

-- Step 02. Create a SWITCH out table
 
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
FROM    [dbo].[FactInternetSales]
WHERE   1=2 -- Note this table will be empty

-- Step 03. Switch OUT the data 
ALTER TABLE [dbo].[FactInternetSales] SWITCH PARTITION 2 TO  [dbo].[FactInternetSales_20000101] PARTITION 2;

-- Step 04. Switch IN the rebuilt data
ALTER TABLE [dbo].[FactInternetSales_20000101_20010101] SWITCH PARTITION 2 TO  [dbo].[FactInternetSales] PARTITION 2;
```

Weitere Informationen zum Neuerstellen von Partitionen mit `CTAS` finden Sie unter [Tabellenpartitionen][] und [Parallelität][].


## Nächste Schritte

Detaillierte Hinweise zur Indexverwaltung finden Sie im Artikel [Verwalten von Indizes][].

Weitere Verwaltungstipps finden Sie in der Übersicht über die [Verwaltung][].

<!--Image references-->

<!--Article references-->
[CTAS]: sql-data-warehouse-develop-ctas.md
[Tabellenpartitionen]: sql-data-warehouse-develop-table-partitions.md
[Parallelität]: sql-data-warehouse-develop-concurrency.md
[Verwaltung]: sql-data-warehouse-manage-monitor.md
[Verwalten von Indizes]: sql-data-warehouse-manage-indexes.md

<!--MSDN references-->
[ALTER INDEX]: https://msdn.microsoft.com/library/ms188388.aspx


<!--Other Web references-->

<!---HONumber=AcomDC_0413_2016-->