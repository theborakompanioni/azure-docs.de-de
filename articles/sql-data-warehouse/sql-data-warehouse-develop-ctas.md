---
title: Create Table As Select (CTAS) in SQL Data Warehouse | Microsoft Docs
description: "Tipps für die Codierung mit der Anweisung „Create Table As Select“ (CTAS) in Azure SQL Data Warehouse zum Entwickeln von Lösungen"
services: sql-data-warehouse
documentationcenter: NA
author: shivaniguptamsft
manager: jhubbard
editor: 
ms.assetid: 68ac9a94-09f9-424b-b536-06a125a653bd
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: queries
ms.date: 01/30/2017
ms.author: shigu;barbkess
ms.translationtype: Human Translation
ms.sourcegitcommit: 68655fff239bfd76f93ab9177d161d9534cbb901
ms.openlocfilehash: 150113dda95ab021dd7ad8696b5886373ba982b8
ms.contentlocale: de-de
ms.lasthandoff: 01/31/2017


---
# <a name="create-table-as-select-ctas-in-sql-data-warehouse"></a>Create Table As Select (CTAS) in SQL Data Warehouse
„Create Table As Select“ ( `CTAS` ) ist eines der wichtigsten verfügbaren T-SQL-Features. Es handelt sich dabei um einen vollständig parallelisierten Vorgang, bei dem eine neue Tabelle anhand der Ausgabe einer SELECT-Anweisung erstellt wird. `CTAS` ist die einfachste und schnellste Möglichkeit, eine Kopie einer Tabelle zu erstellen. Dieses Dokument beinhaltet sowohl Beispiele als auch bewährte Methoden für `CTAS`.

## <a name="selectinto-vs-ctas"></a>SELECT..INTO im Vergleich zu CTAS
Sie können `CTAS` als überladene Version von `SELECT..INTO` betrachten.

Nachstehend sehen Sie ein Beispiel für eine einfache `SELECT..INTO`-Anweisung:

```sql
SELECT *
INTO    [dbo].[FactInternetSales_new]
FROM    [dbo].[FactInternetSales]
```

Im obigen Beispiel würde `[dbo].[FactInternetSales_new]` als verteilte ROUND_ROBIN-Tabelle mit einem CLUSTERED COLUMNSTORE INDEX erstellt, da dies die Tabellenstandardwerte in Azure SQL Data Warehouse sind.

`SELECT..INTO` lässt jedoch nicht zu, dass Sie im Rahmen des Vorgangs die Verteilungsmethode oder den Indextyp ändern. An dieser Stelle tritt `CTAS` auf den Plan.

Die Konvertierung des obigen in `CTAS` ist relativ einfach:

```sql
CREATE TABLE [dbo].[FactInternetSales_new]
WITH
(
    DISTRIBUTION = ROUND_ROBIN
,    CLUSTERED COLUMNSTORE INDEX
)
AS
SELECT  *
FROM    [dbo].[FactInternetSales]
;
```

Mit `CTAS` können Sie sowohl die Verteilung der Tabellendaten als auch den Tabellentyp ändern. 

> [!NOTE]
> Wenn Sie in Ihrem `CTAS`-Vorgang nur den Index ändern möchten und es sich bei der Quelltabelle um eine verteilte Hashtabelle handelt, funktioniert der `CTAS`-Vorgang am besten, wenn Sie dieselbe Verteilungsspalte und denselben Datentyp beibehalten. So wird effizienterweise ein domänenübergreifendes Verschieben von Daten während des Vorgangs vermieden.
> 
> 

## <a name="using-ctas-to-copy-a-table"></a>Kopieren einer Tabelle mithilfe von CTAS
Einer der häufigsten Verwendungszwecke für `CTAS` ist womöglich das Kopieren einer Tabelle, um den DDL-Code ändern zu können. Wenn Sie z.B. die Tabelle ursprünglich als `ROUND_ROBIN` erstellt haben und sie nun in eine Tabelle ändern möchten, die über eine Spalte verteilt ist, ändern Sie die Verteilungsspalte mit `CTAS`. `CTAS` kann auch zum Ändern der Partitionierung, Indizierung oder von Spaltentypen verwendet werden.

Nehmen wir an, dass Sie diese Tabelle mit dem Standardverteilungstyp `ROUND_ROBIN` erstellt haben, da in `CREATE TABLE` keine Verteilungsspalte angegeben wurde.

```sql
CREATE TABLE FactInternetSales
(
    ProductKey int NOT NULL,
    OrderDateKey int NOT NULL,
    DueDateKey int NOT NULL,
    ShipDateKey int NOT NULL,
    CustomerKey int NOT NULL,
    PromotionKey int NOT NULL,
    CurrencyKey int NOT NULL,
    SalesTerritoryKey int NOT NULL,
    SalesOrderNumber nvarchar(20) NOT NULL,
    SalesOrderLineNumber tinyint NOT NULL,
    RevisionNumber tinyint NOT NULL,
    OrderQuantity smallint NOT NULL,
    UnitPrice money NOT NULL,
    ExtendedAmount money NOT NULL,
    UnitPriceDiscountPct float NOT NULL,
    DiscountAmount float NOT NULL,
    ProductStandardCost money NOT NULL,
    TotalProductCost money NOT NULL,
    SalesAmount money NOT NULL,
    TaxAmt money NOT NULL,
    Freight money NOT NULL,
    CarrierTrackingNumber nvarchar(25),
    CustomerPONumber nvarchar(25)
);
```

Nun möchten Sie eine neue Kopie dieser Tabelle mit einem gruppierten Columnstore-Index erstellen, sodass Sie die Leistung von gruppierten Columnstore-Tabellen nutzen können. Außerdem soll diese Tabelle nach ProductKey verteilt werden, da Sie Verknüpfungen für diese Spalte erwarten und Datenverschiebungen während der Verknüpfung nach ProductKey vermeiden möchten. Schließlich möchten Sie auch eine Partitionierung für OrderDateKey hinzufügen, sodass Sie schnell alte Daten löschen können, indem Sie alte Partitionen verwerfen. Im Folgenden finden Sie die CTAS-Anweisung, mit der sie die alte Tabelle in eine neue Tabelle kopieren.

```sql
CREATE TABLE FactInternetSales_new
WITH
(
    CLUSTERED COLUMNSTORE INDEX,
    DISTRIBUTION = HASH(ProductKey),
    PARTITION
    (
        OrderDateKey RANGE RIGHT FOR VALUES
        (
        20000101,20010101,20020101,20030101,20040101,20050101,20060101,20070101,20080101,20090101,
        20100101,20110101,20120101,20130101,20140101,20150101,20160101,20170101,20180101,20190101,
        20200101,20210101,20220101,20230101,20240101,20250101,20260101,20270101,20280101,20290101
        )
    )
)
AS SELECT * FROM FactInternetSales;
```

Schließlich können Sie die Tabellen umbenennen, um zur neuen Tabelle zu wechseln und die alte Tabelle zu verwerfen.

```sql
RENAME OBJECT FactInternetSales TO FactInternetSales_old;
RENAME OBJECT FactInternetSales_new TO FactInternetSales;

DROP TABLE FactInternetSales_old;
```

> [!NOTE]
> Azure SQL Data Warehouse bietet noch keine Unterstützung für die automatische Erstellung oder die automatische Aktualisierung von Statistiken.  Um die beste Leistung bei Abfragen zu erhalten, ist es wichtig, dass die Statistiken für alle Spalten aller Tabellen nach dem ersten Laden oder nach allen wesentlichen Datenänderungen erstellt werden.  Eine ausführliche Erläuterung der Statistik finden Sie im Thema [Statistiken][Statistics] in der Entwicklungsgruppe der Themen.
> 
> 

## <a name="using-ctas-to-work-around-unsupported-features"></a>Umgehen von nicht unterstützten Funktionen mit CTAS
Mit `CTAS` können auch einige nicht unterstützte Funktionen umgangen werden, die nachfolgend aufgeführt sind. Dies kann häufig zu einer Win-Win-Situation führen, da Ihr Code nicht nur kompatibel ist, sondern unter SQL Data Warehouse häufig auch schneller ausgeführt wird. Der Grund hierfür ist das vollständig parallelisierte Design. Szenarien, die mit CTAS umgangen werden können, sind z. B.:

* ANSI JOINS bei UPDATEs
* ANSI JOINs bei DELETEs
* MERGE-Anweisung

> [!NOTE]
> Denken Sie zuerst an „CTAS“. Wenn Sie ein Problem voraussichtlich mit `CTAS` lösen können, ist dies meist der beste Ansatz. Dies gilt auch, wenn Sie dabei mehr Daten schreiben.
> 
> 

## <a name="ansi-join-replacement-for-update-statements"></a>ANSI-Verknüpfungsersatz für update-Anweisungen
Unter Umständen verfügen Sie über ein komplexes Update, mit dem zwei oder mehr Tabellen verknüpft werden, indem ANSI-Verknüpfungssyntax zum Durchführen des UPDATE- oder DELETE-Vorgangs genutzt wird.

Stellen Sie sich vor, Sie müssen diese Tabelle aktualisieren:

```sql
CREATE TABLE [dbo].[AnnualCategorySales]
(    [EnglishProductCategoryName]    NVARCHAR(50)    NOT NULL
,    [CalendarYear]                    SMALLINT        NOT NULL
,    [TotalSalesAmount]                MONEY            NOT NULL
)
WITH
(
    DISTRIBUTION = ROUND_ROBIN
)
;
```

Die ursprüngliche Abfrage könnte etwa wie folgt ausgesehen haben:

```sql
UPDATE    acs
SET        [TotalSalesAmount] = [fis].[TotalSalesAmount]
FROM    [dbo].[AnnualCategorySales]     AS acs
JOIN    (
        SELECT    [EnglishProductCategoryName]
        ,        [CalendarYear]
        ,        SUM([SalesAmount])                AS [TotalSalesAmount]
        FROM    [dbo].[FactInternetSales]        AS s
        JOIN    [dbo].[DimDate]                    AS d    ON s.[OrderDateKey]                = d.[DateKey]
        JOIN    [dbo].[DimProduct]                AS p    ON s.[ProductKey]                = p.[ProductKey]
        JOIN    [dbo].[DimProductSubCategory]    AS u    ON p.[ProductSubcategoryKey]    = u.[ProductSubcategoryKey]
        JOIN    [dbo].[DimProductCategory]        AS c    ON u.[ProductCategoryKey]        = c.[ProductCategoryKey]
        WHERE     [CalendarYear] = 2004
        GROUP BY
                [EnglishProductCategoryName]
        ,        [CalendarYear]
        ) AS fis
ON    [acs].[EnglishProductCategoryName]    = [fis].[EnglishProductCategoryName]
AND    [acs].[CalendarYear]                = [fis].[CalendarYear]
;
```

Da SQL Data Warehouse ANSI-Joins in der `FROM`-Klausel einer `UPDATE`-Anweisung nicht unterstützt, können Sie diesen Code nicht einfach kopieren, ohne ihn geringfügig zu ändern.

Sie können eine Kombination aus `CTAS` und einer impliziten Verknüpfung verwenden, um diesen Code zu ersetzen:

```sql
-- Create an interim table
CREATE TABLE CTAS_acs
WITH (DISTRIBUTION = ROUND_ROBIN)
AS
SELECT    ISNULL(CAST([EnglishProductCategoryName] AS NVARCHAR(50)),0)    AS [EnglishProductCategoryName]
,        ISNULL(CAST([CalendarYear] AS SMALLINT),0)                         AS [CalendarYear]
,        ISNULL(CAST(SUM([SalesAmount]) AS MONEY),0)                        AS [TotalSalesAmount]
FROM    [dbo].[FactInternetSales]        AS s
JOIN    [dbo].[DimDate]                    AS d    ON s.[OrderDateKey]                = d.[DateKey]
JOIN    [dbo].[DimProduct]                AS p    ON s.[ProductKey]                = p.[ProductKey]
JOIN    [dbo].[DimProductSubCategory]    AS u    ON p.[ProductSubcategoryKey]    = u.[ProductSubcategoryKey]
JOIN    [dbo].[DimProductCategory]        AS c    ON u.[ProductCategoryKey]        = c.[ProductCategoryKey]
WHERE     [CalendarYear] = 2004
GROUP BY
        [EnglishProductCategoryName]
,        [CalendarYear]
;

-- Use an implicit join to perform the update
UPDATE  AnnualCategorySales
SET     AnnualCategorySales.TotalSalesAmount = CTAS_ACS.TotalSalesAmount
FROM    CTAS_acs
WHERE   CTAS_acs.[EnglishProductCategoryName] = AnnualCategorySales.[EnglishProductCategoryName]
AND     CTAS_acs.[CalendarYear]               = AnnualCategorySales.[CalendarYear]
;

--Drop the interim table
DROP TABLE CTAS_acs
;
```

## <a name="ansi-join-replacement-for-delete-statements"></a>ANSI-Verknüpfungsersatz für delete-Anweisungen
Manchmal ist `CTAS` der beste Ansatz zum Löschen von Daten. Anstatt die Daten zu löschen, wählen Sie einfach die Daten aus, die Sie behalten möchten. Diese gilt insbesondere für `DELETE`-Anweisungen, die die ANSI-Joinsyntax verwenden. Der Grund dafür ist, dass SQL Data Warehouse keine ANSI-Joins in der `FROM`-Klausel einer `DELETE`-Anweisung unterstützt.

Beispiel für eine konvertierte DELETE-Anweisung:

```sql
CREATE TABLE dbo.DimProduct_upsert
WITH
(   Distribution=HASH(ProductKey)
,   CLUSTERED INDEX (ProductKey)
)
AS -- Select Data you wish to keep
SELECT     p.ProductKey
,          p.EnglishProductName
,          p.Color
FROM       dbo.DimProduct p
RIGHT JOIN dbo.stg_DimProduct s
ON         p.ProductKey = s.ProductKey
;

RENAME OBJECT dbo.DimProduct        TO DimProduct_old;
RENAME OBJECT dbo.DimProduct_upsert TO DimProduct;
```

## <a name="replace-merge-statements"></a>Ersetzen von MERGE-Anweisungen
MERGE-Anweisungen können mit `CTAS`zumindest teilweise ersetzt werden. Sie können `INSERT` und `UPDATE` zu einer einzelnen Anweisung zusammenfassen. Alle gelöschten Datensätze müssen in einer zweiten Anweisung abgeschlossen werden.

Beispiel für `UPSERT` :

```sql
CREATE TABLE dbo.[DimProduct_upsert]
WITH
(   DISTRIBUTION = HASH([ProductKey])
,   CLUSTERED INDEX ([ProductKey])
)
AS
-- New rows and new versions of rows
SELECT      s.[ProductKey]
,           s.[EnglishProductName]
,           s.[Color]
FROM      dbo.[stg_DimProduct] AS s
UNION ALL  
-- Keep rows that are not being touched
SELECT      p.[ProductKey]
,           p.[EnglishProductName]
,           p.[Color]
FROM      dbo.[DimProduct] AS p
WHERE NOT EXISTS
(   SELECT  *
    FROM    [dbo].[stg_DimProduct] s
    WHERE   s.[ProductKey] = p.[ProductKey]
)
;

RENAME OBJECT dbo.[DimProduct]          TO [DimProduct_old];
RENAME OBJECT dbo.[DimpProduct_upsert]  TO [DimProduct];

```

## <a name="ctas-recommendation-explicitly-state-data-type-and-nullability-of-output"></a>CTAS-Empfehlung: Explizites Angeben des Datentyps und der NULL-Zulässigkeit der Ausgabe
Beim Migrieren von Code stoßen Sie unter Umständen auf diese Art von Codiermuster:

```sql
DECLARE @d decimal(7,2) = 85.455
,       @f float(24)    = 85.455

CREATE TABLE result
(result DECIMAL(7,2) NOT NULL
)
WITH (DISTRIBUTION = ROUND_ROBIN)

INSERT INTO result
SELECT @d*@f
;
```

Instinktiv denken Sie vielleicht, dass Sie diesen Code zu CTAS migrieren sollten, und damit hätten Sie auch recht. Hierbei liegt jedoch ein verstecktes Problem vor.

Der folgende Code führt NICHT zum gleichen Ergebnis:

```sql
DECLARE @d decimal(7,2) = 85.455
,       @f float(24)    = 85.455
;

CREATE TABLE ctas_r
WITH (DISTRIBUTION = ROUND_ROBIN)
AS
SELECT @d*@f as result
;
```

Beachten Sie, dass für die Spalte „result“ der Datentyp und die NULL-Zulässigkeitswerte des Ausdrucks übernommen werden. Dies kann zu geringfügigen Abweichungen bei den Werten führen, wenn nicht darauf geachtet wird.

Probieren Sie folgende Eingabe aus:

```sql
SELECT result,result*@d
from result
;

SELECT result,result*@d
from ctas_r
;
```

Der für „result“ gespeicherte Wert ist anders. Da der in der Spalte „result“ beibehaltene Wert in anderen Ausdrücken verwendet wird, wird der Fehler noch signifikanter.

![][1]

Dies ist besonders für Datenmigrationen wichtig. Auch wenn die zweite Abfrage natürlich genauer ist, liegt ein Problem vor. Die Daten würden sich gegenüber dem Quellsystem unterscheiden, und dies stellt die Integrität der Migration infrage. Dies ist einer der seltenen Fälle, in denen die „falsche“ Antwort eigentlich die richtige Antwort ist!

Der Grund für diese Ungleichheit zwischen den beiden Ergebnissen ist die implizite Typumwandlung. Im ersten Beispiel wird für die Tabelle die Spaltendefinition festgelegt. Beim Einfügen der Zeile tritt eine implizite Typumwandlung auf. Im zweiten Beispiel ist keine implizite Typumwandlung vorhanden, da der Ausdruck den Datentyp der Spalte definiert. Beachten Sie auch, dass die Spalte im zweiten Beispiel so konfiguriert wurde, dass NULL-Werte zulässig sind. Im ersten Beispiel ist dies nicht der Fall. Als die Tabelle im ersten Beispiel erstellt wurde, wurde die NULL-Zulässigkeit der Spalte explizit definiert. Im zweiten Beispiel wurde dies dem Ausdruck überlassen, was standardmäßig zu einer NULL-Definition führen würde.  

Zum Lösen dieser Probleme müssen Sie die Typumwandlung und NULL-Zulässigkeit im `SELECT`-Teil der `CTAS`-Anweisung explizit festlegen. Sie können diese Eigenschaften nicht im Create Table-Teil festlegen.

Im folgenden Beispiel wird veranschaulicht, wie Sie dies im Code beheben:

```sql
DECLARE @d decimal(7,2) = 85.455
,       @f float(24)    = 85.455

CREATE TABLE ctas_r
WITH (DISTRIBUTION = ROUND_ROBIN)
AS
SELECT ISNULL(CAST(@d*@f AS DECIMAL(7,2)),0) as result
```

Beachten Sie Folgendes:

* CAST oder CONVERT hätten verwendet werden können
* ISNULL wird zum Erzwingen der NULL-Zulässigkeit verwendet, nicht COALESCE
* ISNULL ist die äußerste Funktion
* Der zweite Teil von ISNULL ist eine Konstante, also 0

> [!NOTE]
> Damit die NULL-Zulässigkeit richtig festgelegt wird, muss unbedingt `ISNULL` verwendet werden, und nicht `COALESCE`. `COALESCE` ist keine deterministische Funktion, sodass für das Ergebnis des Ausdrucks immer NULL-Werte zulässig wären. Dies ist bei `ISNULL` anders. Die Funktion ist deterministisch. Wenn der zweite Teil der `ISNULL`-Funktion eine Konstante oder ein Literal ist, ist der sich ergebende Wert daher NOT NULL.
> 
> 

Dieser Tipp ist nicht nur nützlich, um die Integrität der Berechnungen sicherzustellen. Er ist auch für das Wechseln der Tabellenpartition wichtig. Stellen Sie sich vor, Sie haben die folgende Tabelle als Faktentabelle definiert:

```sql
CREATE TABLE [dbo].[Sales]
(
    [date]      INT     NOT NULL
,   [product]   INT     NOT NULL
,   [store]     INT     NOT NULL
,   [quantity]  INT     NOT NULL
,   [price]     MONEY   NOT NULL
,   [amount]    MONEY   NOT NULL
)
WITH
(   DISTRIBUTION = HASH([product])
,   PARTITION   (   [date] RANGE RIGHT FOR VALUES
                    (20000101,20010101,20020101
                    ,20030101,20040101,20050101
                    )
                )
)
;
```

Das Wertfeld ist aber ein berechneter Ausdruck und nicht Teil der Quelldaten.

Zum Erstellen des partitionierten Datasets können Sie beispielsweise wie folgt vorgehen:

```sql
CREATE TABLE [dbo].[Sales_in]
WITH    
(   DISTRIBUTION = HASH([product])
,   PARTITION   (   [date] RANGE RIGHT FOR VALUES
                    (20000101,20010101
                    )
                )
)
AS
SELECT
    [date]    
,   [product]
,   [store]
,   [quantity]
,   [price]   
,   [quantity]*[price]  AS [amount]
FROM [stg].[source]
OPTION (LABEL = 'CTAS : Partition IN table : Create')
;
```

Die Abfrage würde problemlos ausgeführt werden. Das Problem entsteht, wenn Sie versuchen, den Partitionswechsel durchzuführen. Die Tabellendefinitionen stimmen nicht überein. CTAS muss geändert werden, damit die Tabellendefinitionen übereinstimmen.

```sql
CREATE TABLE [dbo].[Sales_in]
WITH    
(   DISTRIBUTION = HASH([product])
,   PARTITION   (   [date] RANGE RIGHT FOR VALUES
                    (20000101,20010101
                    )
                )
)
AS
SELECT
    [date]    
,   [product]
,   [store]
,   [quantity]
,   [price]   
,   ISNULL(CAST([quantity]*[price] AS MONEY),0) AS [amount]
FROM [stg].[source]
OPTION (LABEL = 'CTAS : Partition IN table : Create');
```

Sie sehen also, dass die Typkonsistenz und die Pflege der Eigenschaften für die NULL-Zulässigkeit für CTAS eine bewährte Methode für das Engineering sind. Es ist hilfreich, die Integrität in Ihren Berechnungen zu wahren. Außerdem wird so sichergestellt, dass der Partitionswechsel möglich ist.

Weitere Informationen zur Verwendung von [CTAS][CTAS] finden Sie auf der MSDN-Website. Dabei handelt es sich um eine der wichtigsten Anweisungen in Azure SQL Data Warehouse. Machen Sie sich damit eingehend vertraut.

## <a name="next-steps"></a>Nächste Schritte
Weitere Hinweise zur Entwicklung finden Sie in der [Entwicklungsübersicht][development overview].

<!--Image references-->
[1]: media/sql-data-warehouse-develop-ctas/ctas-results.png

<!--Article references-->
[development overview]: sql-data-warehouse-overview-develop.md
[Statistics]: ./sql-data-warehouse-tables-statistics.md

<!--MSDN references-->
[CTAS]: https://msdn.microsoft.com/library/mt204041.aspx

<!--Other Web references-->

