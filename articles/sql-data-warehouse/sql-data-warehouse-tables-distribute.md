<properties
   pageTitle="Verteilen von Tabellen in SQL Data Warehouse | Microsoft Azure"
   description="Enthält Informationen zu den ersten Schritten bei der Verteilung von Tabellen in Azure SQL Data Warehouse."
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
   ms.date="08/01/2016"
   ms.author="jrj;barbkess;sonyama"/>

# Verteilen von Tabellen in SQL Data Warehouse

> [AZURE.SELECTOR]
- [Übersicht][]
- [Datentypen][]
- [Verteilen][]
- [Index][]
- [Partition][]
- [Statistiken][]
- [Temporär][]

SQL Data Warehouse ist ein verteiltes Datenbanksystem mit paralleler Massenverarbeitung (Massively Parallel Processing, MPP). Indem die Daten- und Verarbeitungsfunktionen über mehrere Knoten verteilt werden, ermöglicht SQL Data Warehouse eine deutlich umfassendere Skalierbarkeit als ein Einzelsystem. Die Entscheidung, wie Sie Ihre Daten in Ihrer SQL Data Warehouse-Instanz verteilen, ist einer der wichtigsten Faktoren zur Erzielung einer optimalen Leistung. Der Schlüssel für eine optimale Leistung ist die Minimierung der Datenverschiebungen, und der Schlüssel für die Minimierung der Datenverschiebungen ist wiederum die Wahl der richtigen Verteilungsstrategie.

## Grundlegendes zur Datenverschiebung

In einem MPP-System werden die Daten aus den einzelnen Tabellen auf mehrere zugrunde liegende Datenbanken aufgeteilt. Die meisten optimierten Abfragen für ein MPP-System können einfach weitergeleitet und ohne Interaktion zwischen den anderen Datenbanken für die einzelnen verteilten Datenbanken ausgeführt werden. Ein Beispiel: Angenommen, Sie verfügen über eine Vertriebsdatenbank mit zwei Tabellen: eine für den Vertrieb, eine für Kunden. Wenn Sie eine Abfrage haben, für die Ihre Vertriebstabelle mit Ihrer Kundentabelle verknüpft werden muss, und Sie sowohl die Vertriebs- als auch die Kundentabelle nach Kundennummer aufteilen, sodass sich die einzelnen Kunden jeweils in einer eigenen Datenbank befinden, kann jede Abfrage, die eine Verknüpfung von Vertrieb und Kunde erfordert, ohne Kenntnis der anderen Datenbanken innerhalb der einzelnen Datenbanken ausgeführt werden. Wenn Sie Ihre Vertriebsdaten dagegen nach Auftragsnummer und Ihre Kundendaten nach Kundennummer aufgeteilt haben, verfügen die einzelnen Datenbanken nicht über die entsprechenden Daten für die Kunden. Falls Sie nun also die Vertriebsdaten mit den Kundendaten verknüpfen möchten, müssen Sie die Daten für die einzelnen Kunden aus den anderen Datenbanken abrufen. Im zweiten Beispiel müssten die Kundendaten also in die Vertriebsdaten verschoben werden, um die beiden Tabellen zu verknüpfen.

Eine Datenverschiebung ist nicht unbedingt etwas Schlechtes und bei manchen Abfragen auch unvermeidlich. Wenn dieser zusätzliche Schritt jedoch vermieden werden kann, beschleunigt dies natürlich Ihre Abfrage. Zu einer Datenverschiebung kommt es meist, wenn Tabellen verknüpft oder Aggregationen durchgeführt werden. Oftmals ist beides erforderlich. Wenn Sie also eine Optimierung für eines der Szenarien (etwa für die Verknüpfung) vornehmen können, wird die Datenverschiebung trotzdem für das andere Szenario (etwa für die Aggregation) benötigt. Der Trick ist, herauszufinden, was weniger Arbeit macht. Die effektivste Methode zur Verringerung von Datenverschiebungen ist in den meisten Fällen die Verteilung großer Faktentabellen auf der Grundlage einer verknüpften Spalte. Die Verteilung von Daten in verknüpften Spalten wird deutlich häufiger zur Verringerung von Datenverschiebungen verwendet als die Verteilung von Daten auf der Grundlage von Spalten in einer Aggregation.

## Auswählen der Verteilungsmethode

Im Hintergrund teilt SQL Data Warehouse Ihre Daten auf 60 Datenbanken auf. Jede einzelne Datenbank wird als **Verteilung** bezeichnet. Wenn Daten in die einzelnen Tabellen geladen werden, muss SQL Data Warehouse wissen, wie die Daten auf diese 60 Verteilungen aufgeteilt werden sollen.

Die Verteilungsmethode wird auf der Tabellenebene definiert, und derzeit sind zwei Optionen verfügbar:

1. Bei der **Roundrobin-Verteilung** werden Daten gleichmäßig nach dem Zufallsprinzip verteilt.
2. Bei der **Hashverteilung** werden Daten auf der Grundlage von Hashwerten einer einzelnen Spalte verteilt.

Wenn Sie keine Verteilungsmethode definieren, wird für die Tabelle standardmäßig die **Roundrobin**-Verteilungsmethode verwendet. Bei zunehmender Komplexität der Implementierung können Sie die Nutzung von Tabellen mit **Hashverteilung** erwägen, um Datenverschiebungen zu verringern, was wiederum der Abfrageleistung zugute kommt.

### Roundrobin-Tabellen

Bei der Roundrobin-Methode werden Daten, wie der Name schon sagt, „reihum“ verteilt. Beim Laden der Daten wird jede Zeile einfach an die nächste Verteilung gesendet. Bei dieser Verteilungsmethode werden die Daten immer zufällig und sehr gleichmäßig auf alle Verteilungen aufgeteilt. Während des Roundrobin-Prozesses zum Anordnen der Daten wird also keine Sortierung durchgeführt. Aus diesem Grund wird eine Roundrobin-Verteilung auch als „Random Hash“ (zufälliger Hash) bezeichnet. Bei einer Tabelle mit Roundrobin-Verteilung ist es nicht erforderlich, die Daten zu „verstehen“. Daher stellen Roundrobin-Tabellen häufig gute Ladeziele dar.

Wenn keine Verteilungsmethode gewählt wird, wird die Roundrobin-Verteilungsmethode verwendet. Roundrobin-Tabellen sind einfach zu verwenden, weil die Daten zufällig im System verteilt werden. Dies bedeutet aber, dass das System nicht garantieren kann, in welcher Verteilung sich die einzelnen Zeilen befinden. Aus diesem Grund muss das System in einigen Fällen zunächst einen Datenverschiebungsvorgang auslösen, um die Daten besser zu organisieren, bevor die Aufgabenstellung einer Abfrage gelöst werden kann. Aufgrund dieses zusätzlichen Schritts können die Abfragen verlangsamt werden.

Erwägen Sie die Verwendung der Roundrobin-Verteilung für die Tabelle in den folgenden Fällen:

- Als einfacher Ausgangspunkt beim Einstieg
- Wenn kein offensichtlicher Verknüpfungsschlüssel vorhanden ist
- Wenn keine gute Kandidatenspalte für die Hashverteilung der Tabelle vorhanden ist
- Wenn die Tabelle nicht über einen gemeinsamen Verknüpfungsschlüssel mit anderen Tabellen verfügt
- Wenn die Verknüpfung weniger signifikant als andere Verknüpfungen in der Abfrage ist
- Wenn die Tabelle eine temporäre Stagingtabelle ist

In diesen beiden Beispielen wird eine Roundrobin-Tabelle erstellt:

```SQL
-- Round Robin created by default
CREATE TABLE [dbo].[FactInternetSales]
(   [ProductKey]            int          NOT NULL
,   [OrderDateKey]          int          NOT NULL
,   [CustomerKey]           int          NOT NULL
,   [PromotionKey]          int          NOT NULL
,   [SalesOrderNumber]      nvarchar(20) NOT NULL
,   [OrderQuantity]         smallint     NOT NULL
,   [UnitPrice]             money        NOT NULL
,   [SalesAmount]           money        NOT NULL
)
;

-- Explicitly Created Round Robin Table
CREATE TABLE [dbo].[FactInternetSales]
(   [ProductKey]            int          NOT NULL
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
,   DISTRIBUTION = ROUND_ROBIN
)
;
```

> [AZURE.NOTE] „Roundrobin“ ist zwar der Standardtabellentyp, aber als bewährte Methode wird empfohlen, dass Sie in Ihrer DDL explizit vorgehen, damit die Ziele Ihres Tabellenlayouts für andere Personen verständlich sind.

### Tabellen mit Hashverteilung

Die Verwendung eines Algorithmus mit **Hashverteilung** zum Verteilen Ihrer Tabellen kann die Leistung für viele Szenarien verbessern, da sich dadurch die Datenverschiebung zur Abfragezeit verringert. Tabellen mit Hashverteilung sind Tabellen, die auf die verteilten Datenbanken mit einem Hashalgorithmus für eine einzelne Spalte aufgeteilt werden, die Sie auswählen. Anhand der Verteilungsspalte wird bestimmt, wie die Daten auf Ihre verteilten Datenbanken aufgeteilt werden. Die Hashfunktion verwendet die Verteilungsspalte, um Zeilen Verteilungen zuzuweisen. Der Hashalgorithmus und die sich ergebende Verteilung sind deterministisch. Dies bedeutet, dass ein Wert mit einem bestimmten Datentyp per Hashverteilung immer derselben Verteilung zugeordnet wird.

In diesem Beispiel wird eine Tabelle mit Verteilung nach der ID erstellt:

```SQL
CREATE TABLE [dbo].[FactInternetSales]
(   [ProductKey]            int          NOT NULL
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
,  DISTRIBUTION = HASH([ProductKey])
)
;
```

## Auswählen der Verteilungsspalte

Wenn Sie die **Hashverteilung** für eine Tabelle verwenden, müssen Sie eine einzelne Verteilungsspalte auswählen. Beim Auswählen einer Verteilungsspalte müssen drei Hauptfaktoren berücksichtigt werden.

Wählen Sie eine einzelne Spalte aus, für die Folgendes gilt:

1. Sie wird nicht aktualisiert
2. Daten werden gleichmäßig verteilt, Datenschiefe wird vermieden
3. Minimierung der Datenverschiebung

### Auswählen der Verteilungsspalte, die nicht aktualisiert wird

Verteilungsspalten können nicht aktualisiert werden. Wählen Sie daher eine Spalte mit statischen Werten aus. Wenn eine Spalte aktualisiert werden muss, handelt es sich im Allgemeinen nicht um einen guten Verteilungskandidaten. Falls Sie eine Verteilungsspalte aktualisieren müssen, ist dies möglich, indem Sie zuerst die Zeile löschen und dann eine neue Zeile einfügen.

### Auswählen der Verteilungsspalte mit gleichmäßiger Datenverteilung

Da ein verteiltes System nur so schnell wie die langsamste Verteilung ist, ist es wichtig, die Arbeit gleichmäßig auf die Verteilungen aufzuteilen. So erzielen Sie für das System eine ausgewogene Ausführung. Die Verteilung der Arbeit auf ein verteiltes System richtet sich danach, wo sich die Daten für die einzelnen Verteilungen befinden. Daher ist es sehr wichtig, die richtige Verteilungsspalte für die Verteilung der Daten auszuwählen, damit jede Verteilung über die gleiche Menge an Arbeit verfügt und die gleiche Zeit für deren Abarbeitung benötigt. Wenn die Arbeit im System gut verteilt ist, wird dies als ausgewogene Ausführung bezeichnet. Falls die Daten im System nicht gleichmäßig verteilt und nicht ausgewogen sind, wird dies als **Datenschiefe** bezeichnet.

Beachten Sie beim Auswählen der Verteilungsspalte Folgendes, um Daten gleichmäßig zu verteilen und Datenschiefe zu vermeiden:

1. Wählen Sie eine Spalte aus, die eine hohe Zahl von unterschiedlichen Werten enthält.
2. Vermeiden Sie die Verteilung von Daten auf Spalten mit einer hohen Häufigkeit von wenigen Werten oder einer hohen Häufigkeit von Nullen.
3. Vermeiden Sie die Verteilung von Daten auf Datumsspalten.
4. Vermeiden Sie die Verteilung auf Spalten mit weniger als 60 Werten.

Da jeder Wert per Hashverteilung einer von 60 Verteilungen zugeordnet wird, empfiehlt sich zur Erzielung einer gleichmäßigen Verteilung die Auswahl einer Spalte, die einen hohen Eindeutigkeitsfaktor aufweist und deutlich mehr als 60 eindeutige Werte enthält. Stellen Sie sich einen Extremfall vor, in dem eine Spalte nur 40 eindeutige Werte enthält. Wenn diese Spalte als Verteilungsschlüssel ausgewählt wird, werden die Daten dieser Tabelle nur auf einen Teil des Systems verteilt, und 20 Verteilungen enthalten keine Daten und werden nicht an der Verarbeitung beteiligt. Die anderen 40 Verteilungen haben dagegen mehr Arbeit zu verrichten, als wenn die Daten gleichmäßig auf 60 Verteilungen aufgeteilt werden.

Wenn Sie eine Tabelle nach einer Spalte mit einem hohem Anteil von Nullen verteilen, werden alle Nullwerte derselben Verteilung zugeordnet. Diese Verteilung muss dann mehr Arbeit als die anderen Verteilungen verrichten, was zu einer Verlangsamung des gesamten Systems führt. Die Verteilung nach einer Datumsspalte kann zu einer Verarbeitungsschiefe führen, wenn bei Abfragen das Datum eine wichtige Rolle spielt und nur einige Datumsangaben an einer Abfrage beteiligt sind.

Falls keine guten Kandidatenspalten vorhanden sind, können Sie sich für die Roundrobin-Verteilungsmethode entscheiden.

### Auswählen einer Verteilungsspalte zur Reduzierung von Datenverschiebungen

Das Reduzieren von Datenverschiebungen durch das Auswählen der richtigen Verteilungsspalte ist eine der wichtigsten Strategien zum Optimieren der Leistung Ihrer SQL Data Warehouse-Instanz. Zu einer Datenverschiebung kommt es meist, wenn Tabellen verknüpft oder Aggregationen durchgeführt werden. Spalten, die in `JOIN`, `GROUP BY`, `DISTINCT`, `OVER` und `HAVING`-Klauseln verwendet werden, sind **gute** Kandidaten für die Hashverteilung. Spalten in der `WHERE`-Klausel sind dagegen **keine** guten Kandidaten für die Hashverteilung, da sie die für die Abfrage zur Verfügung stehenden Verteilungen beschränken.

Grundsätzlich gilt: Wenn Sie über zwei große Faktentabellen verfügen, die häufig an einer Verknüpfung beteiligt sind, erzielen Sie die besten Ergebnisse mit einer Verteilung, die auf einer der Verknüpfungsspalten basiert. Wenn Sie über eine Tabelle verfügen, die nie mit einer anderen großen Faktentabelle verknüpft wird, können Sie Spalten verwenden, die häufig in der `GROUP BY`-Klausel vorkommen.

Zur Vermeidung einer Datenverschiebung im Zuge einer Verknüpfung müssen einige zentrale Kriterien erfüllt werden:

1. Die an der Verknüpfung beteiligten Tabellen müssen per Hashverteilung auf der Grundlage **einer** der an der Verknüpfung beteiligten Spalten verteilt werden.
2. Die Datentypen der Verknüpfungsspalten müssen in beiden Tabellen übereinstimmen.
3. Die Spalten müssen mit dem Operator „Equals“ (Gleich) verknüpft werden.
4. Der Verknüpfungstyp darf nicht `CROSS JOIN` sein.


## Problembehandlung bei Datenschiefe

Wenn Tabellendaten mithilfe der Hashverteilungsmethode verteilt werden, besteht die Möglichkeit, dass einige Verteilungen ungleichmäßig sind und im Vergleich mit anderen übermäßig viele Daten enthalten. Eine übermäßige Datenschiefe kann die Abfrageleistung beeinträchtigen, da für das Endergebnis einer verteilten Abfrage auf den Abschluss der Verteilung mit der längsten Ausführungsdauer gewartet werden muss. Abhängig vom Grad der Datenschiefe sollten Sie diese beheben.

### Identifizieren der Datenschiefe

Mithilfe von `DBCC PDW_SHOWSPACEUSED` können Sie ganz einfach ermitteln, ob für eine Tabelle eine Datenschiefe vorliegt. Dies ist eine sehr schnelle und einfache Möglichkeit, die Anzahl der Tabellenzeilen anzuzeigen, die in jeder der 60 Verteilungen Ihrer Datenbank gespeichert sind. Denken Sie daran, dass für eine optimale Leistung die Zeilen in Ihrer verteilten Tabelle gleichmäßig auf alle Verteilungen verteilt sein sollten.

```sql
-- Find data skew for a distributed table
DBCC PDW_SHOWSPACEUSED('dbo.FactInternetSales');
```

Wenn Sie jedoch die dynamischen Verwaltungsansichten (Dynamic Management Views, DMV) von Azure SQL Data Warehouse abfragen, können Sie eine ausführlichere Analyse ausführen. Erstellen Sie zunächst die Sicht [dbo.vTableSizes][], indem Sie den SQL-Code aus dem Artikel [Übersicht über Tabellen][Overview] verwenden. Führen Sie nach dem Erstellen der Sicht diese Abfrage aus, um zu identifizieren, welche Tabellen eine Datenschiefe von mehr als 10% aufweisen.

```sql
select *
from dbo.vTableSizes 
where two_part_name in 
    (
    select two_part_name
    from dbo.vTableSizes
    where row_count > 0
    group by two_part_name
    having min(row_count * 1.000)/max(row_count * 1.000) > .10
	)
order by two_part_name, row_count
;
```

### Auflösen von Datenschiefe

Nicht für jede Datenschiefe ist eine Behebung erforderlich. In bestimmten Fällen kann die Leistung einer Tabelle bei einigen Abfragen die negative Beeinträchtigung durch die Datenschiefe ausgleichen. Um zu entscheiden, ob die Datenschiefe in einer Tabelle beseitigt werden sollte, müssen Sie die Datenmenge und die Abfragen in Ihrer Workload möglichst umfassend verstehen. Eine Möglichkeit zur Untersuchung der Datenschiefe ist die Ausführung der Schritte im Artikel [Abfrageüberwachung][]. Hiermit wird überwacht, wie stark sich die Datenschiefe auf die Abfrageleistung auswirkt und wie lange die Durchführung von Abfragen für die einzelnen Verteilungen dauert.

Verteilen von Daten bedeutet, das richtige Gleichgewicht zwischen der Minimierung der Datenschiefe und der Minimierung von Datenverschiebungen zu finden. Dies können gegensätzliche Ziele sein, und manchmal könnten Sie die Datenschiefe beibehalten, um Datenverschiebungen zu reduzieren. Beispiel: Wenn die Verteilungsspalte häufig die freigegebene Spalte in Verknüpfungen und Aggregationen ist, minimieren Sie Datenverschiebungen. Der Vorteil durch minimale Datenverschiebungen überwiegt möglicherweise die Auswirkungen der Datenschiefe.

Das übliche Verfahren zum Beseitigen von Datenschiefe ist, die Tabelle mit einer anderen Verteilungsspalte neu zu erstellen. Da es keine Möglichkeit gibt, die Verteilungsspalte in einer vorhandenen Tabelle zu ändern, sollte die Verteilung einer Tabelle geändert werden, indem sie per [CTAS][] neu erstellt wird. Hier sind zwei Beispiele für das Auflösen der Datenschiefe angegeben:

### Beispiel 1: Neuerstellen der Tabelle mit einer neuen Verteilungsspalte

In diesem Beispiel wird [CTAS][] verwendet, um eine Tabelle mit einer anderen Hashverteilungsspalte neu zu erstellen.

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

--Create statistics on new table
CREATE STATISTICS [ProductKey] ON [FactInternetSales_CustomerKey] ([ProductKey]);
CREATE STATISTICS [OrderDateKey] ON [FactInternetSales_CustomerKey] ([OrderDateKey]);
CREATE STATISTICS [CustomerKey] ON [FactInternetSales_CustomerKey] ([CustomerKey]);
CREATE STATISTICS [PromotionKey] ON [FactInternetSales_CustomerKey] ([PromotionKey]);
CREATE STATISTICS [SalesOrderNumber] ON [FactInternetSales_CustomerKey] ([SalesOrderNumber]);
CREATE STATISTICS [OrderQuantity] ON [FactInternetSales_CustomerKey] ([OrderQuantity]);
CREATE STATISTICS [UnitPrice] ON [FactInternetSales_CustomerKey] ([UnitPrice]);
CREATE STATISTICS [SalesAmount] ON [FactInternetSales_CustomerKey] ([SalesAmount]);

--Rename the tables
RENAME OBJECT [dbo].[FactInternetSales] TO [FactInternetSales_ProductKey];
RENAME OBJECT [dbo].[FactInternetSales_CustomerKey] TO [FactInternetSales];
```

### Beispiel 2: Neuerstellen der Tabelle mit der Roundrobin-Verteilung

In diesem Beispiel wird [CTAS][] verwendet, um eine Tabelle nicht mit der Hashverteilung neu zu erstellen, sondern mit der Roundrobin-Verteilung. Diese Änderung bewirkt eine gleichmäßige Datenverteilung auf Kosten von vermehrten Datenverschiebungen.

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

--Create statistics on new table
CREATE STATISTICS [ProductKey] ON [FactInternetSales_ROUND_ROBIN] ([ProductKey]);
CREATE STATISTICS [OrderDateKey] ON [FactInternetSales_ROUND_ROBIN] ([OrderDateKey]);
CREATE STATISTICS [CustomerKey] ON [FactInternetSales_ROUND_ROBIN] ([CustomerKey]);
CREATE STATISTICS [PromotionKey] ON [FactInternetSales_ROUND_ROBIN] ([PromotionKey]);
CREATE STATISTICS [SalesOrderNumber] ON [FactInternetSales_ROUND_ROBIN] ([SalesOrderNumber]);
CREATE STATISTICS [OrderQuantity] ON [FactInternetSales_ROUND_ROBIN] ([OrderQuantity]);
CREATE STATISTICS [UnitPrice] ON [FactInternetSales_ROUND_ROBIN] ([UnitPrice]);
CREATE STATISTICS [SalesAmount] ON [FactInternetSales_ROUND_ROBIN] ([SalesAmount]);

--Rename the tables
RENAME OBJECT [dbo].[FactInternetSales] TO [FactInternetSales_HASH];
RENAME OBJECT [dbo].[FactInternetSales_ROUND_ROBIN] TO [FactInternetSales];
```

## Nächste Schritte

Weitere Informationen zum Tabellendesign finden Sie in den Artikeln [Verteilen][], [Indizieren][], [Partitionieren][], [Datentypen][], [Statistiken][] und [Temporäre Tabellen][Temporary].

Eine Übersicht über bewährte Methoden finden Sie unter [Bewährte Methoden für SQL Data Warehouse][].


<!--Image references-->

<!--Article references-->
[Overview]: ./sql-data-warehouse-tables-overview.md
[Übersicht]: ./sql-data-warehouse-tables-overview.md
[Datentypen]: ./sql-data-warehouse-tables-data-types.md
[Verteilen]: ./sql-data-warehouse-tables-distribute.md
[Index]: ./sql-data-warehouse-tables-index.md
[Indizieren]: ./sql-data-warehouse-tables-index.md
[Partition]: ./sql-data-warehouse-tables-partition.md
[Partitionieren]: ./sql-data-warehouse-tables-partition.md
[Statistiken]: ./sql-data-warehouse-tables-statistics.md
[Temporary]: ./sql-data-warehouse-tables-temporary.md
[Temporär]: ./sql-data-warehouse-tables-temporary.md
[Bewährte Methoden für SQL Data Warehouse]: ./sql-data-warehouse-best-practices.md
[Abfrageüberwachung]: ./sql-data-warehouse-manage-monitor.md
[dbo.vTableSizes]: ./sql-data-warehouse-tables-overview.md#querying-table-sizes

<!--MSDN references-->
[DBCC PDW_SHOWSPACEUSED()]: https://msdn.microsoft.com/library/mt204028.aspx

<!--Other Web references-->

<!---HONumber=AcomDC_0803_2016-->