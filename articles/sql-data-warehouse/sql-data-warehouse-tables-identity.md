---
title: "Erstellen von Ersatzschlüsseln mit IDENTITY | Microsoft-Dokumentation"
description: "Erfahren Sie, wie Sie mit IDENTITY Ersatzschlüssel in Ihren Tabellen erstellen können."
services: sql-data-warehouse
documentationcenter: NA
author: jrowlandjones
manager: jhubbard
editor: 
ms.assetid: faa1034d-314c-4f9d-af81-f5a9aedf33e4
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.date: 06/13/2017
ms.author: jrj;barbkess
ms.translationtype: Human Translation
ms.sourcegitcommit: 6dbb88577733d5ec0dc17acf7243b2ba7b829b38
ms.openlocfilehash: 3ab5d159e6eaeb830135fe134e108b0e4de4b7d6
ms.contentlocale: de-de
ms.lasthandoff: 07/04/2017


---
# <a name="create-surrogate-keys-by-using-identity"></a>Erstellen von Ersatzschlüsseln mit IDENTITY
> [!div class="op_single_selector"]
> * [Übersicht][Overview]
> * [Datentypen][Data Types]
> * [Verteilen][Distribute]
> * [Index][Index]
> * [Partition][Partition]
> * [Statistiken][Statistics]
> * [Temporär][Temporary]
> * [Identität][Identity]
> 
> 

Viele Datenmodellierer erstellen Ersatzschlüssel in Ihren Tabellen, während sie Data Warehouse-Modelle erstellen. Mit der IDENTITY-Eigenschaft können Sie dies einfach und effektiv durchführen, ohne die Leistung beim Laden zu beeinträchtigen. 

## <a name="get-started-with-identity"></a>Erste Schritte mit IDENTITY
Sie können beim Erstellen einer Tabelle definieren, dass diese die IDENTITY-Eigenschaft hat, indem Sie Syntax verwenden, die der folgenden Anweisung ähnelt:

```sql
CREATE TABLE dbo.T1
(   C1 INT IDENTITY(1,1) NOT NULL
,   C2 INT NULL
)
WITH
(   DISTRIBUTION = HASH(C2)
,   CLUSTERED COLUMNSTORE INDEX
)
;
```

Anschließend können Sie `INSERT..SELECT` zum Auffüllen der Tabelle verwenden.

## <a name="behavior"></a>Verhalten
Die IDENTITY-Eigenschaft wurde so entwickelt, dass sie über alle Verteilungen im Data Warehouse horizontal hochskaliert werden kann. Aus diesem Grund ist die Implementierung von IDENTITY auf das Erreichen dieser Ziele ausgerichtet. In diesem Abschnitt werden die Feinheiten der Implementierung hervorgehoben, um Ihr Verständnis zu vertiefen.  

### <a name="allocation-of-values"></a>Zuordnung von Werten
Die IDENTITY-Eigenschaft garantiert nicht die Reihenfolge, in der die Ersatzwerte zugeordnet sind. Dies spiegelt das Verhalten von SQL Server und Azure SQL-Datenbank wieder. Allerdings ist in Azure SQL Data Warehouse die Abwesenheit einer Garantie ausgeprägter. 

Dies wird in folgendem Beispiel veranschaulicht:

```sql
CREATE TABLE dbo.T1
(   C1 INT IDENTITY(1,1)    NOT NULL
,   C2 VARCHAR(30)              NULL
)
WITH
(   DISTRIBUTION = HASH(C2)
,   CLUSTERED COLUMNSTORE INDEX
)
;

INSERT INTO dbo.T1
VALUES (NULL);

INSERT INTO dbo.T1
VALUES (NULL);

SELECT *
FROM dbo.T1;

DBCC PDW_SHOWSPACEUSED('dbo.T1');
```

Im vorherigen Beispiel wurden zwei Zeilen der Verteilung 1 zugeordnet. Die erste Zeile weist den Ersatzwert 1 in der Spalte `C1` auf, und die zweite Zeile den Ersatzwert 61. Beide Werte wurden durch die IDENTITY-Eigenschaft generiert. Die Zuordnung der Werte ist jedoch nicht fortlaufend. Dieses Verhalten ist beabsichtigt.

### <a name="skewed-data"></a>Verfälschte Daten 
Der Wertebereich für den Datentyp ist gleichmäßig auf die Verteilungen verteilt. Wenn eine verteilte Tabelle verfälschte Daten aufweist, kann der Wertebereich, der für den Datentyp zu Verfügung steht, frühzeitig erschöpft sein. Wenn z.B. alle Daten in einer einzelnen Verteilung enden, hat die Tabelle tatsächlich nur Zugriff auf ein Sechzehntel des Datentyps. Aus diesem Grund ist die IDENTITY-Eigenschaft auf die Datentypen `INT` und `BIGINT` beschränkt.

### <a name="selectinto"></a>SELECT..INTO
Wenn eine vorhandene Identitätsspalte in einer neuer Tabelle ausgewählt ist, erbt die neue Spalte die IDENTITY-Eigenschaft, es sei denn, eine der folgenden Bedingungen trifft zu:
- Die SELECT-Anweisung enthält einen Join.
- Mehrere SELECT-Anweisungen werden mithilfe von UNION verknüpft.
- Die IDENTITY-Spalte ist mehr als einmal in der Auswahlliste aufgeführt.
- Die IDENTITY-Spalte ist Teil eines Ausdrucks.
    
Wenn eine dieser Bedingungen zutrifft, wird die Spalte als NOT NULL erstellt, statt die IDENTITY-Eigenschaft zu erben.

### <a name="create-table-as-select"></a>CREATE TABLE AS SELECT
CREATE TABLE AS SELECT (CTAS) folgt dem gleichen SQL Server-Verhalten wie SELECT..INTO. Sie können jedoch keine IDENTITY-Eigenschaft in der Spaltendefinition im `CREATE TABLE`-Teil der Anweisung angeben. Außerdem können Sie keine IDENTITY-Funktion im `SELECT`-Teil von CTAS verwenden. Um eine Tabelle zu aufzufüllen, müssen Sie `CREATE TABLE` verwenden, um die Tabelle zu definieren, und dann `INSERT..SELECT`, um sie aufzufüllen.

## <a name="explicitly-insert-values-into-an-identity-column"></a>Fügen Sie Werte explizit in eine IDENTITY-Spalte ein 
SQL Data Warehouse unterstützt `SET IDENTITY_INSERT <your table> ON|OFF`-Syntax. Mit dieser Syntax können Sie explizit Werte in die IDENTITY-Spalte einfügen.

Viele Datenmodellierer verwenden vordefinierte negative Werte für bestimmte Zeilen in ihren Dimensionen. Die Zeile „-1“ oder „unbekannter Member“ ist ein Beispiel. 

Das nächste Skript zeigt, wie Sie diese Zeile explizit mithilfe von SET IDENTITY_INSERT hinzufügen können:

```sql
SET IDENTITY_INSERT dbo.T1 ON;

INSERT INTO dbo.T1
(   C1
,   C2
)
VALUES (-1,'UNKNOWN')
;

SET IDENTITY_INSERT dbo.T1 OFF;

SELECT  *
FROM    dbo.T1
;
```    

## <a name="load-data-into-a-table-with-identity"></a>Laden Sie Daten mit IDENTITY in eine Tabelle

Die IDENTITY-Eigenschaft wirkt sich auf Ihren Code zum Laden von Daten aus. In diesem Abschnitt werden einige grundlegende Muster zum Laden von Daten in Tabellen mit IDENTITY hervorgehoben. 

### <a name="load-data-with-polybase"></a>Laden von Daten mit PolyBase
Um Daten in eine Tabelle zu laden und einen Ersatzschlüssel mit IDENTITY zu generieren, erstellen Sie die Tabelle und verwenden Sie dann INSERT..SELECT oder INSERT..VALUES, um den Ladevorgang durchzuführen.

Im folgende Beispiel wird das grundlegende Muster aufgezeigt:
 
```sql
--CREATE TABLE with IDENTITY
CREATE TABLE dbo.T1
(   C1 INT IDENTITY(1,1)
,   C2 VARCHAR(30)
)
WITH
(   DISTRIBUTION = HASH(C2)
,   CLUSTERED COLUMNSTORE INDEX
)
;

--Use INSERT..SELECT to populate the table from an external table
INSERT INTO dbo.T1
(C2)
SELECT  C2
FROM    ext.T1
;

SELECT  *
FROM    dbo.T1
;

DBCC PDW_SHOWSPACEUSED('dbo.T1');
```

> [!NOTE] 
> Aktuell ist es nicht möglich, `CREATE TABLE AS SELECT` zu verwenden, wenn Sie Daten mit einer IDENTITY-Spalte in eine Tabelle laden.
> 

Weitere Informationen zum Laden von Daten mit dem BCP-Tool (Bulk Copy Program) finden Sie in den folgenden Artikeln:

- [Laden mit PolyBase][]
- [Bewährte Methoden mit PolyBase][]

### <a name="load-data-with-bcp"></a>Laden von Daten mit BCP
BCP ist ein Befehlszeilentool, das Sie zum Laden von Daten in SQL Data Warehouse verwenden können. Einen seiner Parameter (– E) steuert das Verhalten von BCP beim Laden von Daten in eine Tabelle mit einer IDENTITY-Spalte. 

Wenn -E angegeben ist, werden die Werte in der Eingabedatei für die IDENTITY-Spalte beibehalten. Wenn -E *nicht* angegeben ist, werden die Werte in dieser Spalte ignoriert. Wenn die IDENTITY-Spalte nicht enthalten ist, werden die Daten wie gewohnt geladen. Die Werte werden entsprechend der Richtlinie für die Schrittweite und den Startwert der Eigenschaft generiert.

Weitere Informationen zum Laden von Daten mithilfe von BCP finden Sie in den folgenden Artikeln:

- [Laden mit BCP][]
- [BCP in MSDN][]

## <a name="catalog-views"></a>Katalogansichten
SQL Data Warehouse unterstützt die `sys.identity_columns`-Katalogansicht. Diese Ansicht kann verwendet werden, um eine Spalte zu identifizieren, die die IDENTITY-Eigenschaft hat.

Damit Sie das Datenbankschema besser verstehen, wird in diesem Beispiel gezeigt, wie Sie `sys.identity_columns` in andere Systemkatalogansichten integrieren können:

```sql
SELECT  sm.name
,       tb.name
,       co.name
,       CASE WHEN ic.column_id IS NOT NULL
             THEN 1
        ELSE 0
        END AS is_identity 
FROM        sys.schemas AS sm
JOIN        sys.tables  AS tb           ON  sm.schema_id = tb.schema_id
JOIN        sys.columns AS co           ON  tb.object_id = co.object_id
LEFT JOIN   sys.identity_columns AS ic  ON  co.object_id = ic.object_id
                                        AND co.column_id = ic.column_id
WHERE   sm.name = 'dbo'
AND     tb.name = 'T1'
;
```

## <a name="limitations"></a>Einschränkungen
Die IDENTITY-Eigenschaft kann in den folgenden Szenarios nicht verwendet werden:
- Wenn der Spaltendatentyp weder INT noch BIGINT ist
- Wenn die Spalte auch der Verteilungsschlüssel ist
- Wenn die Tabelle eine externe Tabelle ist 

Die folgenden verwandten Funktionen werden in SQL Data Warehouse nicht unterstützt:

- [IDENTITY()][]
- [@@IDENTITY][]
- [SCOPE_IDENTITY][]
- [IDENT_CURRENT][]
- [IDENT_INCR][]
- [IDENT_SEED][]
- [DBCC CHECK_IDENT()][]

## <a name="tasks"></a>Aufgaben

Dieser Abschnitt enthält Beispielcode, den Sie verwenden können, um bei der Arbeit mit der IDENTITY-Spalten allgemeine Aufgaben auszuführen.

> [!NOTE] 
> Spalte C1 ist die IDENTITY in allen folgenden Aufgaben.
> 
 
### <a name="find-the-highest-allocated-value-for-a-table"></a>Suchen Sie den höchsten zugeordneten Wert für eine Tabelle
Verwenden Sie die Funktion `MAX()`, um den höchsten zugeordneten Wert einer verteilten Tabelle zu bestimmen:

```sql
SELECT  MAX(C1)
FROM    dbo.T1
``` 

### <a name="find-the-seed-and-increment-for-the-identity-property"></a>Suchen Sie den Startwert und die Schrittweite für die IDENTITY-Eigenschaft
Sie können die Katalogansichten verwenden, um die Konfigurationswerte für die ID-Schrittweite und den Startwert für eine Tabelle mit der folgenden Abfrage zu ermitteln: 

```sql
SELECT  sm.name
,       tb.name
,       co.name
,       ic.seed_value
,       ic.increment_value 
FROM        sys.schemas AS sm
JOIN        sys.tables  AS tb           ON  sm.schema_id = tb.schema_id
JOIN        sys.columns AS co           ON  tb.object_id = co.object_id
JOIN        sys.identity_columns AS ic  ON  co.object_id = ic.object_id
                                        AND co.column_id = ic.column_id
WHERE   sm.name = 'dbo'
AND     tb.name = 'T1'
;
```

## <a name="next-steps"></a>Nächste Schritte

* Weitere Informationen zum Entwickeln von Tabellen finden Sie unter [Tabellenübersicht][Overview], [Tabellendatentypen][Data Types], [Verteilen einer Tabelle][Distribute], [Indizieren einer Tabelle][Index], [Partitionieren einer Tabelle][Partition] und [Temporäre Tabellen][Temporary]. 
* Weitere Informationen zu bewährten Methoden finden Sie unter [Bewährte Methoden für SQL Data Warehouse][SQL Data Warehouse Best Practices].  

<!--Image references-->

<!--Article references-->
[Overview]: ./sql-data-warehouse-tables-overview.md
[Data Types]: ./sql-data-warehouse-tables-data-types.md
[Distribute]: ./sql-data-warehouse-tables-distribute.md
[Index]: ./sql-data-warehouse-tables-index.md
[Partition]: ./sql-data-warehouse-tables-partition.md
[Statistics]: ./sql-data-warehouse-tables-statistics.md
[Temporary]: ./sql-data-warehouse-tables-temporary.md
[Identity]: ./sql-data-warehouse-tables-identity.md
[SQL Data Warehouse Best Practices]: ./sql-data-warehouse-best-practices.md

[Laden mit BCP]: https://docs.microsoft.com/azure/sql-data-warehouse/sql-data-warehouse-load-with-bcp/
[Laden mit PolyBase]: https://docs.microsoft.com/azure/sql-data-warehouse/sql-data-warehouse-load-from-azure-blob-storage-with-polybase/
[Bewährte Methoden mit PolyBase]: https://docs.microsoft.com/azure/sql-data-warehouse/sql-data-warehouse-load-polybase-guide/


<!--MSDN references-->
[Identity property]: https://msdn.microsoft.com/library/ms186775.aspx
[sys.identity_columns]: https://msdn.microsoft.com/library/ms187334.aspx
[IDENTITY()]: https://msdn.microsoft.com/library/ms189838.aspx
[@@IDENTITY]: https://msdn.microsoft.com/library/ms187342.aspx
[SCOPE_IDENTITY]: https://msdn.microsoft.com/library/ms190315.aspx
[IDENT_CURRENT]: https://msdn.microsoft.com/library/ms175098.aspx
[IDENT_INCR]: https://msdn.microsoft.com/library/ms189795.aspx
[IDENT_SEED]: https://msdn.microsoft.com/library/ms189834.aspx
[DBCC CHECK_IDENT()]: https://msdn.microsoft.com/library/ms176057.aspx

[BCP in MSDN]: https://msdn.microsoft.com/library/ms162802.aspx
  

<!--Other Web references-->  

