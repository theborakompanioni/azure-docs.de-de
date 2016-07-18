<properties
   pageTitle="Verwalten von Statistiken für Tabellen in SQL Data Warehouse | Microsoft Azure"
   description="Enthält Informationen zu den ersten Schritten zu Statistiken von Tabellen in Azure SQL Data Warehouse."
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
   ms.date="06/30/2016"
   ms.author="jrj;barbkess;sonyama"/>

# Verwalten von Statistiken für Tabellen in SQL Data Warehouse

> [AZURE.SELECTOR]
- [Übersicht][]
- [Datentypen][]
- [Verteilen][]
- [Index][]
- [Partition][]
- [Statistiken][]
- [Temporär][]

Je mehr Informationen SQL Data Warehouse zu Ihren Daten besitzt, desto schneller können Abfragen für die Daten durchgeführt werden. Sie lassen SQL Data Warehouse diese Informationen zu den Daten zukommen, indem Sie Statistiken zu Ihren Daten anlegen. Das Anlegen von Statistiken über Ihre Daten ist eine der wichtigsten Maßnahmen, die Sie zur Optimierung von Abfragen ergreifen können. Mithilfe von Statistiken kann SQL Data Warehouse einen möglichst optimalen Plan für Ihre Abfragen erstellen. Der Grund ist, dass der Abfrageoptimierer von SQL Data Warehouse ein kostenbasierter Optimierer ist. Die Kosten der verschiedenen Abfragepläne werden verglichen, und dann wird der Plan mit den geringsten Kosten gewählt. Dies sollte auch der Plan sein, der am schnellsten ausgeführt wird.

Statistiken können für eine einzelne Spalte, mehrere Spalten oder einen Index einer Tabelle erstellt werden. Statistiken werden in einem Histogramm gespeichert, in dem der Wertebereich und die Selektivität von Werten erfasst werden. Dies ist von besonderem Interesse, wenn der Optimierer JOINs, GROUP BY-, HAVING- und WHERE-Klauseln in einer Abfrage auswerten muss. Wenn vom Optimierer beispielsweise geschätzt wird, dass für das Filterdatum der Abfrage eine Zeile zurückgegeben wird, wird unter Umständen ein völlig anderer Plan gewählt, als wenn die Schätzung lautet, dass für das ausgewählte Datum 1 Million Zeilen zurückgegeben werden. Das Erstellen von Statistiken ist zwar äußerst wichtig, aber es ist genauso wichtig, dass die Statistiken den aktuellen Zustand der Tabelle *genau* widerspiegeln. Durch die Verwendung von aktuellen Statistiken wird sichergestellt, dass vom Optimierer ein guter Plan gewählt wird. Die vom Optimierer erstellten Pläne sind nur so gut wie die Statistiken für Ihre Daten.

Das Erstellen und Aktualisieren von Statistiken ist derzeit ein manueller Prozess, der aber sehr einfach ist. Dies ist ein Unterschied zu SQL Server, wo Statistiken für einzelne Spalten und Indizes automatisch erstellt und aktualisiert werden. Mit den unten angegebenen Informationen können Sie die Verwaltung der Statistiken über Ihre Daten stark automatisieren.

## Erste Schritte mit Statistiken

 Das Erstellen von erfassten Statistiken für jede Spalte ist eine einfache Möglichkeit zum Einsteigen in das Thema Statistiken. Da es genauso wichtig ist, die Statistiken aktuell zu halten, kann ein konservativer Ansatz das Aktualisieren von Statistiken jeden Tag oder nach jedem Ladevorgang sein. Es gibt immer Vor- und Nachteile, was die Leistung einerseits und die Kosten für die Erstellung und Aktualisierung von Statistiken andererseits betrifft. Falls die Verwaltung aller Statistiken zu lange dauert, können Sie versuchen, selektiver auszuwählen, welche Spalten über Statistiken verfügen sollen oder häufig aktualisiert werden müssen. Beispielsweise kann das tägliche Aktualisieren von Datumsspalten ratsam sein, wenn neue Werte hinzugefügt werden, anstatt nach jedem Ladevorgang. Der größte Vorteil ergibt sich wieder, wenn Sie über Statistiken für Spalten mit JOINs und GROUP BY-, HAVING- und WHERE-Klauseln verfügen. Wenn Sie eine Tabelle mit vielen Spalten haben, die nur in der SELECT-Klausel verwendet werden, sind Statistiken für diese Spalten ggf. nicht hilfreich. Indem Sie etwas Zeit investieren, um nur die Spalten zu ermitteln, für die Statistiken hilfreich sind, können Sie den Zeitaufwand für die Verwaltung Ihrer Statistiken reduzieren.

## Mehrspaltenstatistiken

Zusätzlich zum Erstellen von Statistiken zu einzelnen Spalten werden Sie ggf. herausfinden, dass Ihre Abfragen von Mehrspaltenstatistiken profitieren. Statistiken für mehrere Spalten sind Statistiken, die für eine Liste mit Spalten erstellt werden. Sie enthalten Einspaltenstatistiken für die erste Spalte der Liste sowie einige spaltenübergreifende Korrelationsinformationen, die als „Densities“ (Dichten) bezeichnet werden. Wenn Sie beispielsweise über eine Tabelle verfügen, die über zwei Spalten mit einer anderen Tabelle verknüpft ist, kann SQL Data Warehouse den Plan unter Umständen besser optimieren, wenn die Beziehung zwischen den beiden Spalten klar ist. Mehrspaltenstatistiken können die Abfrageleistung für bestimmte Vorgänge verbessern, z.B. zusammengesetzte Verknüpfungen (Joins) und Group By-Vorgänge.

## Aktualisieren von Statistiken

Das Aktualisieren von Statistiken ist ein wichtiger Bestandteil der Datenbankverwaltung. Wenn sich die Verteilung der Daten in der Datenbank ändert, müssen die Statistiken aktualisiert werden. Veraltete Statistiken führen zu einer suboptimalen Abfrageleistung.

Eine bewährte Methode ist es, die Statistiken für Datenspalten im Zuge des Hinzufügens neuer Daten täglich zu aktualisieren. Bei jedem Laden von neuen Zeilen in das Data Warehouse werden neue Datumsangaben für Lade- oder Transaktionsvorgänge hinzugefügt. Dadurch wird die Datenverteilung geändert, und die Statistiken sind nicht mehr aktuell. Im Gegensatz dazu müssen Statistiken zu einer Länderspalte in einer Kundentabelle möglicherweise nie aktualisiert werden, da sich die Verteilung der Werte in der Regel nicht ändert. Wenn davon auszugehen ist, dass die Verteilung zwischen Kunden konstant ist, bewirkt das Hinzufügen neuer Zeilen zur Tabellenvariante keine Änderung der Datenverteilung. Wenn Ihr Data Warehouse allerdings nur ein Land enthält und Sie Daten eines neuen Landes hinzufügen, führt dies dazu, dass Daten aus mehreren Ländern gespeichert werden. Sie müssen dann auf jedem Fall die Statistiken in der Länderspalte aktualisieren.

Eine der ersten Fragen bei der Problembehandlung für eine Abfrage sollte lauten: „Sind die Statistiken auf dem aktuellen Stand?“

Diese Frage kann nicht anhand des Alters der Daten beantwortet werden. Ein Statistikobjekt auf dem aktuellen Stand könnte sehr alt sein, falls sich die zugrunde liegenden Daten nicht wesentlich geändert haben. Wenn sich die Anzahl von Zeilen deutlich geändert hat oder es eine wesentliche Änderung bei der Verteilung der Werte für eine bestimmte Spalte gibt, *ist der Zeitpunkt gekommen*, die Statistiken zu aktualisieren.

Für die spätere Verwendung aktualisiert **SQL Server** (nicht SQL Data Warehouse) die Statistiken für folgende Situationen automatisch:

- Wenn Sie null Zeilen in der Tabelle haben und Zeilen hinzufügen, erhalten Sie ein automatisches Update der Statistiken.
- Wenn Sie einer Tabelle, die mit weniger als 500 Zeilen beginnt, mehr als 500 Zeilen hinzufügen (Sie haben z.B. am Anfang 499 und fügen 500 Zeilen hinzu, sodass Sie insgesamt 999 Zeilen haben), erhalten Sie ein automatisches Update.
- Sobald Sie über mehr als 500 Zeilen verfügen, müssen Sie 500 zusätzliche Zeilen + 20 % der Tabellengröße hinzufügen, bevor Sie ein automatisches Update der Statistiken erhalten.

Da keine DMV existiert, mit der Sie feststellen können, ob sich die Daten innerhalb der Tabelle seit der letzten Aktualisierung der Statistik geändert haben, kann das Alter Ihrer Statistiken Ihnen teilweise Einblick bieten, ob die Daten aktuell sind. Sie können die folgende Abfrage verwenden, um den Zeitpunkt zu ermitteln, zu dem die Statistiken für jede Tabelle zuletzt aktualisiert wurden.

> [AZURE.NOTE] Beachten Sie, dass Sie immer wenn sich die Verteilung der Werte einer bestimmten Sprache wesentlich ändern, Sie Statistiken aktualisieren sollten, unabhängig vom Zeitpunkt des letzten Updates.

```sql
SELECT
    sm.[name] AS [schema_name],
    tb.[name] AS [table_name],
    co.[name] AS [stats_column_name],
    st.[name] AS [stats_name],
    STATS_DATE(st.[object_id],st.[stats_id]) AS [stats_last_updated_date]
FROM
    sys.objects ob
    JOIN sys.stats st
        ON  ob.[object_id] = st.[object_id]
    JOIN sys.stats_columns sc    
        ON  st.[stats_id] = sc.[stats_id]
        AND st.[object_id] = sc.[object_id]
    JOIN sys.columns co    
        ON  sc.[column_id] = co.[column_id]
        AND sc.[object_id] = co.[object_id]
    JOIN sys.types  ty    
        ON  co.[user_type_id] = ty.[user_type_id]
    JOIN sys.tables tb    
        ON  co.[object_id] = tb.[object_id]
    JOIN sys.schemas sm    
        ON  tb.[schema_id] = sm.[schema_id]
WHERE
    st.[user_created] = 1;
```

Beispielsweise benötigen Datumsspalten in einem Data Warehouse normalerweise häufige Statistikaktualisierungen. Bei jedem Laden von neuen Zeilen in das Data Warehouse werden neue Datumsangaben für Lade- oder Transaktionsvorgänge hinzugefügt. Dadurch wird die Datenverteilung geändert, und die Statistiken sind nicht mehr aktuell. Im Gegensatz dazu müssen die Statistiken für die Spalte „Geschlecht“ in einer Kundentabelle unter Umständen nie aktualisiert werden. Wenn davon auszugehen ist, dass die Verteilung zwischen Kunden konstant ist, bewirkt das Hinzufügen neuer Zeilen zur Tabellenvariante keine Änderung der Datenverteilung. Wenn Ihr Data Warehouse aber nur ein Geschlecht enthält und eine neue Anforderung zu mehr als einem Geschlecht führt, müssen Sie die Statistiken für die Spalte „Geschlecht“ auf jeden Fall aktualisieren.

Weitere Informationen finden Sie unter [Statistiken][] auf der MSDN-Website.

## Implementieren der Statistikverwaltung

Häufig ist es ratsam, den Datenladeprozess zu erweitern, um sicherzustellen, dass die Statistiken am Ende des Ladevorgangs aktualisiert werden. Das Laden von Daten ist der Zeitpunkt, zu dem Tabellen am häufigsten ihre Größe oder die Verteilung der Werte ändern. Daher ist dies ein logischer Ansatzpunkt zum Implementieren einiger Verwaltungsprozesse.

Unten sind einige Richtlinien zur Aktualisierung von Statistiken während des Ladeprozesses angegeben:

- Stellen Sie sicher, dass jede geladene Tabelle mindestens über ein aktualisiertes Statistikobjekt verfügt. Im Rahmen der Statistikaktualisierung werden dann die Informationen zur Tabellengröße (Zeilen- und Seitenanzahl) aktualisiert.
- Konzentrieren Sie sich auf Spalten mit JOIN-, GROUP BY-, ORDER BY- und DISTINCT-Klauseln.
- Erwägen Sie, Spalten vom Typ „aufsteigender Schlüssel“, z. B. Transaktionsdaten, häufiger zu aktualisieren, da diese Werte nicht in das Statistikhistogramm einbezogen werden.
- Erwägen Sie, Spalten mit statischer Verteilung weniger häufig zu aktualisieren.
- Bedenken Sie, dass jedes statistische Objekt der Reihe nach aktualisiert wird. Es ist ggf. nicht ideal, einfach `UPDATE STATISTICS <TABLE_NAME>` zu implementieren. Dies gilt besonders für breite Tabellen mit vielen Statistikobjekten.

> [AZURE.NOTE] Weitere Informationen zum Thema [Aufsteigender Schlüssel] finden Sie im Whitepaper zum Kardinalitätsschätzungsmodell von SQL Server 2014.

Weitere Informationen finden Sie unter [Kardinalitätsschätzung][] auf der MSDN-Website.

## Beispiele: Erstellen von Statistiken

In diesen Beispielen wird veranschaulicht, wie Sie verschiedene Optionen zum Erstellen von Statistiken verwenden. Die Optionen, die Sie für die einzelnen Spalten verwenden, richten Sie nach den Merkmalen Ihrer Daten und nach der Verwendung der Spalten in Abfragen.

### A: Erstellen von Einspaltenstatistiken mit Standardoptionen

Zum Erstellen von Statistiken für eine Spalte geben Sie einfach einen Namen für das Statistikobjekt und den Namen der Spalte an.

Bei dieser Syntax werden alle Standardoptionen verwendet. Standardmäßig wird in SQL Data Warehouse beim Erstellen von Statistiken eine Stichprobe von 20 % der Tabelle verwendet.

```sql
CREATE STATISTICS [statistics_name] ON [schema_name].[table_name]([column_name]);
```

Beispiel:

```sql
CREATE STATISTICS col1_stats ON dbo.table1 (col1);
```

### B. Erstellen von Einspaltenstatistiken per Untersuchung jeder Zeile

Die standardmäßige Stichprobenrate von 20 % ist in den meisten Fällen ausreichend. Sie können die Stichprobenrate aber auch anpassen.

Verwenden Sie die folgende Syntax, um die gesamte Tabelle zu verwenden:

```sql
CREATE STATISTICS [statistics_name] ON [schema_name].[table_name]([column_name]) WITH FULLSCAN;
```

Beispiel:

```sql
CREATE STATISTICS col1_stats ON dbo.table1 (col1) WITH FULLSCAN;
```

### C. Erstellen von Einspaltenstatistiken durch Angeben der Stichprobengröße

Alternativ dazu können Sie die Stichprobengröße als Prozentwert angeben:

```sql
CREATE STATISTICS col1_stats ON dbo.table1 (col1) WITH SAMPLE = 50 PERCENT;
```

### D. Erstellen von Einspaltenstatistiken für einen Teil der Zeilen

Eine weitere Möglichkeit ist das Erstellen von Statistiken für einen Teil der Zeilen einer Tabelle. Dies wird als gefilterte Statistik bezeichnet.

Sie können gefilterte Statistiken beispielsweise verwenden, wenn Sie planen, eine bestimmte Partition einer großen partitionierten Tabelle abzufragen. Indem Sie nur für die Partitionswerte Statistiken erstellen, wird die Genauigkeit der Statistiken erhöht und die Abfrageleistung verbessert.

In diesem Beispiel werden Statistiken für einen Bereich von Werten erstellt. Die Werte können leicht so definiert werden, dass sie den Werten in einer Partition entsprechen.

```sql
CREATE STATISTICS stats_col1 ON table1(col1) WHERE col1 > '2000101' AND col1 < '20001231';
```

> [AZURE.NOTE] Damit der Abfrageoptimierer beim Auswählen des Plans für die verteilte Abfrage die Verwendung von gefilterten Statistiken berücksichtigt, muss die Abfrage in die Definition des Statistikobjekts passen. Im vorherigen Beispiel müssen für die Where-Klausel der Abfrage col1-Werte zwischen 2000101 und 20001231 angegeben werden.

### E. Erstellen von Einspaltenstatistiken mit allen Optionen

Sie können die Optionen natürlich kombinieren. Im folgenden Beispiel wird ein Objekt vom Typ „gefilterte Statistik“ mit einer benutzerdefinierten Stichprobengröße erstellt:

```sql
CREATE STATISTICS stats_col1 ON table1 (col1) WHERE col1 > '2000101' AND col1 < '20001231' WITH SAMPLE = 50 PERCENT;
```

Die gesamte Referenz finden Sie unter [CREATE STATISTICS][] auf der MSDN-Website.

### F. Erstellen von Mehrspaltenstatistiken

Verwenden Sie zum Erstellen einer Mehrspaltenstatistik einfach die vorherigen Beispiele, aber geben Sie mehr Spalten an.

> [AZURE.NOTE] Das Histogramm, das zum Schätzen der Zeilenanzahl im Abfrageergebnis verwendet wird, ist nur für die erste Spalte verfügbar, die in der Definition des Statistikobjekts aufgelistet ist.

In diesem Beispiel basiert das Histogramm auf *product\_category*. Spaltenübergreifende Statistiken werden für *product\_category* und *product\_sub\_c\\ategory* berechnet:

```sql
CREATE STATISTICS stats_2cols ON table1 (product_category, product_sub_category) WHERE product_category > '2000101' AND product_category < '20001231' WITH SAMPLE = 50 PERCENT;
```

Da zwischen *product\_category* und *product\_sub\_category* eine Korrelation besteht, kann eine Mehrspaltenstatistik nützlich sein, wenn gleichzeitig auf diese Spalten zugegriffen wird.

### G. Erstellen von Statistiken für alle Spalten einer Tabelle

Eine Möglichkeit zum Erstellen von Statistiken ist das Ausgeben von CREATE STATISTICS-Befehlen nach dem Erstellen der Tabelle.

```sql
CREATE TABLE dbo.table1
(
   col1 int
,  col2 int
,  col3 int
)
WITH
  (
    CLUSTERED COLUMNSTORE INDEX
  )
;

CREATE STATISTICS stats_col1 on dbo.table1 (col1);
CREATE STATISTICS stats_col2 on dbo.table2 (col2);
CREATE STATISTICS stats_col3 on dbo.table3 (col3);
```

### H. Verwenden einer gespeicherten Prozedur zum Erstellen von Statistiken für alle Spalten einer Datenbank

SQL Data Warehouse verfügt nicht über eine im System gespeicherte Prozedur, die [sp\_create\_stats][] in SQL Server entspricht. Mit dieser gespeicherten Prozedur wird ein Einzelspaltenstatistik-Objekt für jede Spalte der Datenbank erstellt, die nicht bereits über eine Statistik verfügt.

Dies ist eine nützliche Einstiegshilfe für den Datenbankentwurf. Sie können diesen Vorgang an Ihre Anforderungen anpassen.

```sql
CREATE PROCEDURE    [dbo].[prc_sqldw_create_stats]
(   @create_type    tinyint -- 1 default 2 Fullscan 3 Sample
,   @sample_pct     tinyint
)
AS

IF @create_type NOT IN (1,2,3)
BEGIN
    THROW 151000,'Invalid value for @stats_type parameter. Valid range 1 (default), 2 (fullscan) or 3 (sample).',1;
END;

IF @sample_pct IS NULL
BEGIN;
    SET @sample_pct = 20;
END;

IF OBJECT_ID('tempdb..#stats_ddl') IS NOT NULL
BEGIN;
	DROP TABLE #stats_ddl;
END;

CREATE TABLE #stats_ddl
WITH    (   DISTRIBUTION    = HASH([seq_nmbr])
        ,   LOCATION        = USER_DB
        )
AS
WITH T
AS
(
SELECT      t.[name]                        AS [table_name]
,           s.[name]                        AS [table_schema_name]
,           c.[name]                        AS [column_name]
,           c.[column_id]                   AS [column_id]
,           t.[object_id]                   AS [object_id]
,           ROW_NUMBER()
            OVER(ORDER BY (SELECT NULL))    AS [seq_nmbr]
FROM        sys.[tables] t
JOIN        sys.[schemas] s         ON  t.[schema_id]       = s.[schema_id]
JOIN        sys.[columns] c         ON  t.[object_id]       = c.[object_id]
LEFT JOIN   sys.[stats_columns] l   ON  l.[object_id]       = c.[object_id]
                                    AND l.[column_id]       = c.[column_id]
                                    AND l.[stats_column_id] = 1
LEFT JOIN	sys.[external_tables] e	ON	e.[object_id]		= t.[object_id]
WHERE       l.[object_id] IS NULL
AND			e.[object_id] IS NULL -- not an external table
)
SELECT  [table_schema_name]
,       [table_name]
,       [column_name]
,       [column_id]
,       [object_id]
,       [seq_nmbr]
,       CASE @create_type
        WHEN 1
        THEN    CAST('CREATE STATISTICS '+QUOTENAME('stat_'+table_schema_name+ '_' + table_name + '_'+column_name)+' ON '+QUOTENAME(table_schema_name)+'.'+QUOTENAME(table_name)+'('+QUOTENAME(column_name)+')' AS VARCHAR(8000))
        WHEN 2
        THEN    CAST('CREATE STATISTICS '+QUOTENAME('stat_'+table_schema_name+ '_' + table_name + '_'+column_name)+' ON '+QUOTENAME(table_schema_name)+'.'+QUOTENAME(table_name)+'('+QUOTENAME(column_name)+') WITH FULLSCAN' AS VARCHAR(8000))
        WHEN 3
        THEN    CAST('CREATE STATISTICS '+QUOTENAME('stat_'+table_schema_name+ '_' + table_name + '_'+column_name)+' ON '+QUOTENAME(table_schema_name)+'.'+QUOTENAME(table_name)+'('+QUOTENAME(column_name)+') WITH SAMPLE '+@sample_pct+'PERCENT' AS VARCHAR(8000))
        END AS create_stat_ddl
FROM T
;

DECLARE @i INT              = 1
,       @t INT              = (SELECT COUNT(*) FROM #stats_ddl)
,       @s NVARCHAR(4000)   = N''
;

WHILE @i <= @t
BEGIN
    SET @s=(SELECT create_stat_ddl FROM #stats_ddl WHERE seq_nmbr = @i);

    PRINT @s
    EXEC sp_executesql @s
    SET @i+=1;
END

DROP TABLE #stats_ddl;
```

Rufen Sie einfach die Prozedur auf, um damit Statistiken für alle Spalten der Tabelle zu erstellen.

```sql
prc_sqldw_create_stats;
```

## Beispiele: Aktualisieren von Statistiken

Sie können wie folgt vorgehen, um Statistiken zu aktualisieren:

1. Aktualisieren Sie ein Statistikobjekt. Geben Sie den Namen des Statistikobjekts an, das Sie aktualisieren möchten.
2. Aktualisieren Sie alle Statistikobjekte einer Tabelle. Geben Sie anstelle eines bestimmten Statistikobjekts den Namen der Tabelle an.


### A. Aktualisieren eines bestimmten Statistikobjekts ###
Verwenden Sie die folgende Syntax, um ein bestimmtes Statistikobjekt zu aktualisieren:

```sql
UPDATE STATISTICS [schema_name].[table_name]([stat_name]);
```

Beispiel:

```sql
UPDATE STATISTICS [dbo].[table1] ([stats_col1]);
```

Indem Sie bestimmte Statistikobjekte aktualisieren, können Sie den Zeit- und Ressourcenaufwand reduzieren, der zum Verwalten von Statistiken erforderlich ist. Hierfür ist aber Überlegung gefragt, damit die besten Statistikobjekte für die Aktualisierung ausgewählt werden können.


### B. Aktualisieren aller Statistiken einer Tabelle ###
Hier wird eine einfache Methode zum Aktualisieren aller Statistikobjekte einer Tabelle gezeigt.

```sql
UPDATE STATISTICS [schema_name].[table_name];
```

Beispiel:

```sql
UPDATE STATISTICS dbo.table1;
```

Diese Anweisung ist einfach zu verwenden. Bedenken Sie, dass hiermit alle Statistiken der Tabelle aktualisiert werden, sodass unter Umständen mehr Arbeit als erforderlich erledigt wird. Wenn die Leistung kein Problem darstellt, ist dies auf jeden Fall die einfachste und umfassendste Möglichkeit sicherzustellen, dass die Statistiken aktuell sind.

> [AZURE.NOTE] Beim Aktualisieren aller Statistiken einer Tabelle führt SQL Data Warehouse einen Scan durch, um für jede Statistik Stichproben der Tabelle zu nehmen. Wenn die Tabelle groß ist und viele Spalten und Statistiken enthält, kann es effizienter sein, je nach Bedarf einzelne Spalten zu aktualisieren.

Eine Implementierung einer `UPDATE STATISTICS`-Prozedur wird im Artikel [Temporäre Tabellen][Temporary] beschrieben. Die Implementierungsmethode unterscheidet sich etwas von der obigen `CREATE STATISTICS`-Prozedur, aber das Endergebnis ist identisch.

Die vollständige Syntax finden Sie unter [Aktualisieren von Statistiken][] auf der MSDN-Website.

## Statistikmetadaten
Es gibt mehrere Systemsichten und -funktionen, die Sie zum Suchen nach Informationen zu Statistiken verwenden können. Beispielsweise können Sie sehen, ob ein Statistikobjekt veraltet ist, indem Sie die stats-date-Funktion verwenden. Damit können Sie anzeigen, wann Statistiken zuletzt erstellt oder aktualisiert wurden.

### Katalogsichten für Statistiken
Diese Systemsichten enthalten Informationen zu Statistiken:

| Katalogsicht | Beschreibung |
| :----------- | :---------- |
| [sys.columns][] | Eine Zeile für jede Spalte. |
| [sys.objects][] | Eine Zeile für jedes Objekt in der Datenbank. | |
| [sys.schemas][] | Eine Zeile für jedes Schema in der Datenbank. | |
| [sys.stats][] | Eine Zeile für jedes Statistikobjekt. |
| [sys.stats\_columns][] | Eine Zeile für jede Spalte im Statistikobjekt. Eine Verknüpfung zurück zu sys.columns. |
| [sys.tables][] | Eine Zeile für jede Tabelle (enthält externe Tabellen). |
| [sys.table\_types][] | Eine Zeile für jeden Datentyp. |


### Systemfunktionen für Statistiken
Diese Systemfunktionen sind nützlich für die Arbeit mit Statistiken:

| Systemfunktion | Beschreibung |
| :-------------- | :---------- |
| [STATS\_DATE][] | Datum, an dem das Statistikobjekt zuletzt aktualisiert wurde. |
| [DBCC SHOW\_STATISTICS][] | Stellt Zusammenfassungsebenen und ausführliche Informationen zur Verteilung der Werte gemäß Statistikobjekt bereit. |

### Kombinieren von Statistikspalten und -funktionen zu einer Sicht

In dieser Sicht werden Spalten, die sich auf Statistiken beziehen, und Ergebnisse aus der [STATS\_DATE()][]-Funktion zusammengefasst.

```sql
CREATE VIEW dbo.vstats_columns
AS
SELECT
        sm.[name]                           AS [schema_name]
,       tb.[name]                           AS [table_name]
,       st.[name]                           AS [stats_name]
,       st.[filter_definition]              AS [stats_filter_defiinition]
,       st.[has_filter]                     AS [stats_is_filtered]
,       STATS_DATE(st.[object_id],st.[stats_id])
                                            AS [stats_last_updated_date]
,       co.[name]                           AS [stats_column_name]
,       ty.[name]                           AS [column_type]
,       co.[max_length]                     AS [column_max_length]
,       co.[precision]                      AS [column_precision]
,       co.[scale]                          AS [column_scale]
,       co.[is_nullable]                    AS [column_is_nullable]
,       co.[collation_name]                 AS [column_collation_name]
,       QUOTENAME(sm.[name])+'.'+QUOTENAME(tb.[name])
                                            AS two_part_name
,       QUOTENAME(DB_NAME())+'.'+QUOTENAME(sm.[name])+'.'+QUOTENAME(tb.[name])
                                            AS three_part_name
FROM    sys.objects                         AS ob
JOIN    sys.stats           AS st ON    ob.[object_id]      = st.[object_id]
JOIN    sys.stats_columns   AS sc ON    st.[stats_id]       = sc.[stats_id]
                            AND         st.[object_id]      = sc.[object_id]
JOIN    sys.columns         AS co ON    sc.[column_id]      = co.[column_id]
                            AND         sc.[object_id]      = co.[object_id]
JOIN    sys.types           AS ty ON    co.[user_type_id]   = ty.[user_type_id]
JOIN    sys.tables          AS tb ON  co.[object_id]        = tb.[object_id]
JOIN    sys.schemas         AS sm ON  tb.[schema_id]        = sm.[schema_id]
WHERE   1=1
AND     st.[user_created] = 1
;
```

## DBCC SHOW\_STATISTICS()-Beispiele

Mit DBCC SHOW\_STATISTICS() werden die Daten angezeigt, die in einem Statistikobjekt enthalten sind. Diese Daten bestehen aus drei Teilen.

1. Header
2. Dichtevektor
3. Histogramm

Dies sind die Headermetadaten zur Statistik. Im Histogramm wird die Verteilung der Werte in der ersten Schlüsselspalte des Statistikobjekts angezeigt. Der Dichtevektor misst die spaltenübergreifende Korrelation. SQLDW berechnet Kardinalitätsschätzungen anhand der Daten im Statistikobjekt.

### Anzeigen von Header, Dichte und Histogramm

In diesem einfachen Beispiel werden alle drei Teile eines Statistikobjekts angezeigt.

```sql
DBCC SHOW_STATISTICS([<schema_name>.<table_name>],<stats_name>)
```

Beispiel:

```sql
DBCC SHOW_STATISTICS (dbo.table1, stats_col1);
```

### Anzeigen eines oder mehrerer Teile von DBCC SHOW\_STATISTICS();

Wenn Sie nur bestimmte Teile anzeigen möchten, verwenden Sie die `WITH`-Klausel und geben an, welche Teile dies sein sollen:

```sql
DBCC SHOW_STATISTICS([<schema_name>.<table_name>],<stats_name>) WITH stat_header, histogram, density_vector
```

Beispiel:

```sql
DBCC SHOW_STATISTICS (dbo.table1, stats_col1) WITH histogram, density_vector
```

## DBCC SHOW\_STATISTICS()-Unterschiede
DBCC SHOW\_STATISTICS() ist im Vergleich zu SQL Server strenger in SQL Data Warehouse implementiert.

1. Nicht dokumentierte Funktionen werden nicht unterstützt.
- Verwendung von Stats\_stream nicht möglich
- Ergebnisse für bestimmte Teilmengen von Statistikdaten können nicht verknüpft werden, z. B. (STAT\_HEADER JOIN DENSITY\_VECTOR)
2. NO\_INFOMSGS kann für die Meldungsunterdrückung nicht festgelegt werden
3. Eckige Klammern um Namen von Statistiken können nicht verwendet werden
4. Spaltennamen können nicht zum Identifizieren von Statistikobjekten verwendet werden
5. Benutzerdefinierter Fehler 2767 wird nicht unterstützt

## Nächste Schritte

Weitere Informationen finden Sie unter [DBCC SHOW\_STATISTICS][] auf der MSDN-Website. Weitere Informationen finden Sie in den Artikeln [Übersicht über Tabellen][Overview], [Tabellendatentypen][Data Types], [Verteilen einer Tabelle][Distribute], [Indizieren einer Tabelle][Index], [Partitionieren einer Tabelle][Partition] und [Temporäre Tabellen][Temporary]. Weitere Informationen zu bewährten Methoden finden Sie unter [Bewährte Methoden für SQL Data Warehouse][].

<!--Image references-->

<!--Article references-->
[Overview]: ./sql-data-warehouse-tables-overview.md
[Übersicht]: ./sql-data-warehouse-tables-overview.md
[Data Types]: ./sql-data-warehouse-tables-data-types.md
[Datentypen]: ./sql-data-warehouse-tables-data-types.md
[Distribute]: ./sql-data-warehouse-tables-distribute.md
[Verteilen]: ./sql-data-warehouse-tables-distribute.md
[Index]: ./sql-data-warehouse-tables-index.md
[Partition]: ./sql-data-warehouse-tables-partition.md
[Statistics]: ./sql-data-warehouse-tables-statistics.md
[Temporary]: ./sql-data-warehouse-tables-temporary.md
[Temporär]: ./sql-data-warehouse-tables-temporary.md
[Bewährte Methoden für SQL Data Warehouse]: ./sql-data-warehouse-best-practices.md

<!--MSDN references-->  
[Kardinalitätsschätzung]: https://msdn.microsoft.com/library/dn600374.aspx
[CREATE STATISTICS]: https://msdn.microsoft.com/library/ms188038.aspx
[DBCC SHOW\_STATISTICS]: https://msdn.microsoft.com/library/ms174384.aspx
[Statistiken]: https://msdn.microsoft.com/library/ms190397.aspx
[STATS\_DATE]: https://msdn.microsoft.com/library/ms190330.aspx
[sys.columns]: https://msdn.microsoft.com/library/ms176106.aspx
[sys.objects]: https://msdn.microsoft.com/library/ms190324.aspx
[sys.schemas]: https://msdn.microsoft.com/library/ms190324.aspx
[sys.stats]: https://msdn.microsoft.com/library/ms177623.aspx
[sys.stats\_columns]: https://msdn.microsoft.com/library/ms187340.aspx
[sys.tables]: https://msdn.microsoft.com/library/ms187406.aspx
[sys.table\_types]: https://msdn.microsoft.com/library/bb510623.aspx
[Aktualisieren von Statistiken]: https://msdn.microsoft.com/library/ms187348.aspx

<!--Other Web references-->  

<!---HONumber=AcomDC_0706_2016-->