---
title: "Entwurfsleitfaden für replizierte Tabellen – Azure SQL Data Warehouse | Microsoft-Dokumentation"
description: "Empfehlungen für das Entwerfen von replizierten Tabellen im Azure SQL Data Warehouse-Schema."
services: sql-data-warehouse
documentationcenter: NA
author: ronortloff
manager: jhubbard
editor: 
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: tables
ms.date: 07/14/2017
ms.author: rortloff;barbkess
ms.translationtype: HT
ms.sourcegitcommit: 79bebd10784ec74b4800e19576cbec253acf1be7
ms.openlocfilehash: efb5361496a2199a960564f6213f509b693e3cfa
ms.contentlocale: de-de
ms.lasthandoff: 08/03/2017

---

# <a name="design-guidance-for-using-replicated-tables-in-azure-sql-data-warehouse"></a>Entwurfsleitfaden für die Verwendung von replizierten Tabellen in Azure SQL Data Warehouse
Dieser Artikel enthält Empfehlungen für das Entwerfen von replizierten Tabellen im SQL Data Warehouse-Schema. Nutzen Sie diese Empfehlungen, um die Abfrageleistung zu verbessern, indem Sie die Datenverschiebung und die Komplexität von Abfragen reduzieren.

> [!NOTE]
> Das Feature für replizierte Tabellen ist derzeit als öffentliche Vorschauversion verfügbar. Manches Verhalten des Features kann noch geändert werden.
> 

## <a name="prerequisites"></a>Voraussetzungen
In diesem Artikel wird davon ausgegangen, dass Sie mit den Konzepten der Datenverteilung und Datenbewegung in SQL Data Warehouse vertraut sind.  Weitere Informationen finden Sie unter [Verteilte Daten](sql-data-warehouse-distributed-data.md). 

Für den Tabellenentwurf sollten Sie möglichst umfassende Kenntnisse zu Ihren Daten und über das Abfragen der Daten besitzen.  Stellen Sie sich beispielsweise die folgenden Fragen:

- Wie groß ist die Tabelle?   
- Wie oft wird die Tabelle aktualisiert?   
- Habe ich Fakten- und Dimensionstabellen in einem Data Warehouse?   

## <a name="what-is-a-replicated-table"></a>Was ist eine replizierte Tabelle?
Eine replizierte Tabelle verfügt über eine vollständige Kopie der Tabelle, auf die auf jedem Computeknoten zugegriffen werden kann. Durch das Replizieren einer Tabelle müssen Daten vor einem Join oder einer Aggregation nicht mehr auf Computeknoten übertragen werden. Da die Tabelle über mehrere Kopien verfügt, lässt sich mit replizierten Tabellen am besten arbeiten, wenn die komprimierte Tabelle kleiner als 2 GB ist.

Im folgenden Diagramm wird eine replizierte Tabelle dargestellt, auf die auf jedem Computeknoten zugegriffen werden kann. In SQL Data Warehouse wird die replizierte Tabelle vollständig in eine Verteilungsdatenbank auf jedem Computeknoten kopiert. 

![Replizierte Tabelle](media/guidance-for-using-replicated-tables/replicated-table.png "Replizierte Tabelle")  

Replizierte Tabellen eignen sich gut für kleine Dimensionstabellen in einem Sternschema. Dimensionstabellen haben normalerweise eine Größe, die das Speichern und Verwalten mehrerer Kopien ermöglicht. In Dimensionen werden beschreibende Daten gespeichert, die sich selten ändern, z.B. Kundenname und -adresse sowie Produktdetails. Da sich die Daten nicht häufig ändern, muss die replizierte Tabelle seltener neu erstellt werden. 

Die Verwendung einer replizierten Tabelle kann sinnvoll sein, wenn folgende Bedingungen zutreffen:

- Die Tabellengröße auf dem Datenträger ist geringer als 2 GB, unabhängig von der Anzahl der Zeilen. Sie können die Größe der Tabelle mit dem [DBCC PDW_SHOWSPACEUSED](https://docs.microsoft.com/en-us/sql/t-sql/database-console-commands/dbcc-pdw-showspaceused-transact-sql)-Befehl ermitteln: `DBCC PDW_SHOWSPACEUSED('ReplTableCandidate')`. 
- Die Tabelle wird in Joins verwendet, die andernfalls Datenverschiebung erfordern. Beispielsweise erfordert ein Join für eine Tabelle mit Hashverteilung Datenverschiebung, wenn es sich bei den zu verknüpfenden Spalten nicht um die gleiche Verteilungsspalte handelt. Wenn eine der Tabellen mit Hashverteilung klein ist, empfiehlt sich möglicherweise eine replizierte Tabelle. Ein Join für eine Roundrobin-Tabelle erfordert Datenverschiebung. In den meisten Fällen wird empfohlen, replizierte Tabellen statt Roundrobintabellen zu verwenden. 


Es empfiehlt sich, eine vorhandene verteilte Tabelle in eine replizierte Tabelle zu konvertieren, wenn folgende Bedingungen zutreffen:

- In Abfrageplänen werden Datenverschiebungen verwendet, die die Daten auf alle Computeknoten übertragen. Die BroadcastMoveOperation ist aufwendig und beeinträchtigt die Abfrageleistung. Verwenden Sie [sys.dm_pdw_request_steps](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-pdw-request-steps-transact-sql), um Datenverschiebungen in Abfrageplänen anzuzeigen.
 
Replizierte Tabellen liefern möglicherweise nicht die optimale Abfrageleistung, wenn folgende Bedingungen zutreffen:

- In der Tabelle erfolgen häufig Einfüge-, Aktualisierungs- und Löschvorgänge. Vorgänge der Datenbearbeitungssprache (Data Manipulation Language, DML) erfordern das erneute Erstellen der replizierten Tabelle. Ein häufiges Neuerstellen kann zu Leistungseinbußen führen.
- Das Data Warehouse wird häufig skaliert. Durch das Skalieren des Data Warehouse ändert sich die Anzahl der Computeknoten, was eine Neuerstellung nach sich zieht.
- Die Tabelle enthält eine große Anzahl von Spalten, die Datenvorgänge greifen jedoch in der Regel nur auf eine kleine Anzahl von Spalten zu. In diesem Szenario ist es möglicherweise effektiver, eine Hashverteilung der Tabelle und dann einen Index für die Spalten mit häufigem Zugriff zu erstellen, statt die gesamte Tabelle zu replizieren. Wenn eine Abfrage Datenverschiebung erfordert, verschiebt SQL Data Warehouse nur Daten in den angeforderten Spalten. 



## <a name="use-replicated-tables-with-simple-query-predicates"></a>Verwenden Sie replizierte Tabellen mit einfachen Abfrageprädikaten
Bevor Sie sich für das Verteilen oder Replizieren einer Tabelle entscheiden, sollten Sie überlegen, welche Typen von Abfragen Sie für die Tabelle ausführen werden. Gehen Sie nach Möglichkeit wie folgt vor:

- Verwenden Sie replizierte Tabellen für Abfragen mit einfachen Abfrageprädikaten, z.B. Gleichheit oder Ungleichheit.
- Verwenden Sie verteile Tabellen für Abfragen mit komplexen Abfrageprädikaten, z.B. „LIKE“ oder „NOT LIKE“.

CPU-intensive Abfragen zeigen die beste Leistung, wenn die Verarbeitung auf alle Computeknoten verteilt ist. Beispielsweise bieten Abfragen, die Berechnungen für jede Zeile einer Tabelle ausführen, mit verteilten Tabellen eine bessere Leistung als mit replizierten Tabellen. Da eine replizierte Tabelle vollständig auf jedem Computeknoten gespeichert wird, wird eine CPU-intensive Abfrage einer replizierten Tabelle für die gesamte Tabelle auf jedem Computeknoten ausgeführt. Die zusätzliche Berechnung kann die Abfrageleistung beeinträchtigen.

Beispielsweise weist diese Abfrage ein komplexes Prädikat auf.  Sie wird schneller ausgeführt, wenn die Eingabetabelle keine replizierte Tabelle, sondern eine verteilte Tabelle ist. In diesem Beispiel kann dies eine Eingabetabelle mit Hashverteilung oder mit Roundrobinverteilung sein.

```sql

SELECT EnglishProductName 
FROM DimProduct 
WHERE EnglishDescription LIKE '%frame%comfortable%'

```

## <a name="convert-existing-round-robin-tables-to-replicated-tables"></a>Konvertieren Sie vorhandene Roundrobintabellen in replizierte Tabellen
Wenn Sie bereits über Roundrobintabellen verfügen, sollten Sie sie in replizierte Tabellen konvertieren, sofern sie die in diesem Artikel beschriebenen Kriterien erfüllen. Replizierte Tabellen bieten eine bessere Leistung als Roundrobintabellen, da sie nicht das Verschieben von Daten erfordern.  Eine Roundrobintabelle erfordert für Joins immer Datenverschiebung. 

In diesem Beispiel wird mithilfe von [CTAS](https://docs.microsoft.com/sql/t-sql/statements/create-table-as-select-azure-sql-data-warehouse) die Tabelle „DimSalesTerritory“ in eine replizierte Tabelle geändert. Dieses Beispiel ist unabhängig davon gültig, ob „DimSalesTerritory“ eine Tabelle mit Hashverteilung oder eine Roundrobintabelle ist.

```sql
CREATE TABLE [dbo].[DimSalesTerritory_REPLICATE]   
WITH   
  (   
    CLUSTERED COLUMNSTORE INDEX,  
    DISTRIBUTION = REPLICATE  
  )  
AS SELECT * FROM [dbo].[DimSalesTerritory]
OPTION  (LABEL  = 'CTAS : DimSalesTerritory_REPLICATE') 

--Create statistics on new table
CREATE STATISTICS [SalesTerritoryKey] ON [DimSalesTerritory_REPLICATE] ([SalesTerritoryKey]);
CREATE STATISTICS [SalesTerritoryAlternateKey] ON [DimSalesTerritory_REPLICATE] ([SalesTerritoryAlternateKey]);
CREATE STATISTICS [SalesTerritoryRegion] ON [DimSalesTerritory_REPLICATE] ([SalesTerritoryRegion]);
CREATE STATISTICS [SalesTerritoryCountry] ON [DimSalesTerritory_REPLICATE] ([SalesTerritoryCountry]);
CREATE STATISTICS [SalesTerritoryGroup] ON [DimSalesTerritory_REPLICATE] ([SalesTerritoryGroup]);

-- Switch table names
RENAME OBJECT [dbo].[DimSalesTerritory] to [DimSalesTerritory_old];
RENAME OBJECT [dbo].[DimSalesTerritory_REPLICATE] TO [DimSalesTerritory];

DROP TABLE [dbo].[DimSalesTerritory_old];
```  

### <a name="query-performance-example-for-round-robin-versus-replicated"></a>Beispiel für die Abfrageleistung bei Roundrobintabellen und replizierten Tabellen 

Eine replizierte Tabelle erfordert für Joins keine Datenverschiebung, da die gesamte Tabelle bereits auf jedem Computeknoten vorhanden ist. Wenn die Dimensionstabellen nach dem Roundrobinprinzip verteilt sind, wird für einen Join die Dimensionstabelle vollständig auf jeden Computeknoten kopiert. Zum Verschieben der Daten enthält der Abfrageplan einen Vorgang mit dem Namen „BroadcastMoveOperation“. Dieser Typ von Datenverschiebungsvorgang verringert die Abfrageleistung und lässt sich durch die Verwendung replizierter Tabellen vermeiden. Verwenden Sie zum Anzeigen der Schritte von Abfrageplänen die Systemkatalogsicht [sys.dm_pdw_request_steps](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-pdw-request-steps-transact-sql). 

Beispielsweise ist in der folgenden Abfrage des AdventureWorks-Schemas die Tabelle ` FactInternetSales` eine Tabelle mit Hashverteilung. Die Tabellen `DimDate` und `DimSalesTerritory` sind kleinere Dimensionstabellen. Diese Abfrage gibt den Gesamtumsatz in Nordamerika für das Geschäftsjahr 2004 zurück:
 
```sql
SELECT [TotalSalesAmount] = SUM(SalesAmount)
FROM dbo.FactInternetSales s
INNER JOIN dbo.DimDate d
  ON d.DateKey = s.OrderDateKey
INNER JOIN dbo.DimSalesTerritory t
  ON t.SalesTerritoryKey = s.SalesTerritoryKey
WHERE d.FiscalYear = 2004
  AND t.SalesTerritoryGroup = 'North America'
```
Wir haben `DimDate` und `DimSalesTerritory` als Roundrobintabellen neu erstellt. Deshalb wies die Abfrage den folgenden Abfrageplan auf, der mehrere Broadcastverschiebevorgänge enthält: 
 
![Roundrobin-Abfrageplan](media/design-guidance-for-replicated-tables/round-robin-tables-query-plan.jpg "Roundrobin-Abfrageplan") 

Wir haben `DimDate` und `DimSalesTerritory` als replizierte Tabellen neu erstellt und die Abfrage erneut ausgeführt. Der resultierende Abfrageplan ist sehr viel kürzer und enthält keine Broadcastverschiebungen.

![Replizierter Abfrageplan](media/design-guidance-for-replicated-tables/replicated-tables-query-plan.jpg "Roundrobinabfrageplan") 


## <a name="performance-considerations-for-modifying-replicated-tables"></a>Überlegungen zur Leistung beim Ändern replizierter Tabellen
SQL Data Warehouse implementiert eine replizierte Tabelle, indem eine Masterversion der Tabelle erhalten bleibt. Die Masterversion wird in eine Verteilungsdatenbank auf jedem Computeknoten kopiert. Wenn eine Änderung erfolgt, aktualisiert SQL Data Warehouse zuerst die Mastertabelle. Anschließend müssen die Tabellen auf jedem Computeknoten neu erstellt werden. Das Neuerstellen einer replizierten Tabelle umfasst das Kopieren der Tabelle auf jeden Computeknoten und das anschließende Neuerstellen der Indizes.

Nach folgenden Vorgängen ist eine Neuerstellung erforderlich:
- Daten werden geladen oder geändert.
- Das Data Warehouse wird auf eine andere DWU-Einstellung skaliert.
- Die Tabellendefinition wird aktualisiert.

Nach folgenden Vorgängen ist keine Neuerstellung erforderlich:
- Anhalten
- Fortsetzen

Die Neuerstellung erfolgt nicht unmittelbar nach dem Ändern der Daten. Stattdessen wird die Neuerstellung ausgelöst, wenn eine Abfrage zum ersten Mal etwas aus der Tabelle auswählt.  Die erste SELECT-Anweisung für die Tabelle enthält Schritte zum Neuerstellen der replizierten Tabelle.  Da die Neuerstellung innerhalb der Abfrage erfolgt, können abhängig von der Größe der Tabelle die Auswirkungen der ersten SELECT-Anweisung erheblich sein.  Wenn mehrere replizierte Tabellen neu erstellt werden müssen, wird jede Kopie fortlaufend durch Schritte in der Anweisung neu erstellt.  Damit während der Neuerstellung der replizierten Tabelle die Datenkonsistenz erhalten bleibt, wird eine exklusive Sperre auf die Tabelle angewendet.  Die Sperre verhindert für die Dauer der Neuerstellung jeglichen Zugriff auf die Tabelle. 

### <a name="use-indexes-conservatively"></a>Verwenden Sie Indizes zurückhaltend
Für replizierte Tabellen gelten Standardindizierungsverfahren. SQL Data Warehouse erstellt während der Neuerstellung jeden replizierten Tabellenindex neu. Verwenden Sie Indizes nur, wenn der Leistungsgewinn die Kosten für die Neuerstellung der Indizes überwiegt.  
 
### <a name="batch-data-loads"></a>Fassen Sie Ladevorgänge in Batches zusammen
Versuchen Sie beim Laden von Daten in replizierte Tabellen Neuerstellungen zu minimieren, indem Sie die Ladevorgänge in Batches zusammenfassen. Führen Sie alle in Batches zusammengefassten Ladevorgänge aus, bevor die SELECT-Anweisungen ausgeführt werden.

Beispielsweise werden in diesem Auslastungsmuster Daten aus vier Quellen geladen und vier Neuerstellungen aufgerufen. 

- Laden aus Quelle 1.
- Eine SELECT-Anweisung löst Neuerstellung 1 aus.
- Laden aus Quelle 2.
- Eine SELECT-Anweisung löst Neuerstellung 2 aus.
- Laden aus Quelle 3.
- Eine SELECT-Anweisung löst Neuerstellung 3 aus.
- Laden aus Quelle 4.
- Eine SELECT-Anweisung löst Neuerstellung 4 aus.

Beispielsweise werden in diesem Auslastungsmuster Daten aus vier Quellen geladen, jedoch wird nur eine Neuerstellung aufgerufen.

- Laden aus Quelle 1.
- Laden aus Quelle 2.
- Laden aus Quelle 3.
- Laden aus Quelle 4.
- Eine SELECT-Anweisung löst die Neuerstellung aus.


### <a name="rebuild-a-replicated-table-after-a-batch-load"></a>Erstellen Sie nach jedem Batchladevorgang eine replizierte Tabelle neu
Um einheitliche Abfrageausführungszeiten sicherzustellen, wird empfohlen, nach jedem Batchladevorgang das Aktualisieren der replizierten Tabellen zu erzwingen. Andernfalls kann die erste Abfrage erst ausgeführt werden, nachdem die Tabellen aktualisiert wurden, wozu auch das Neuerstellen der Indizes gehört. Je nach Größe und Anzahl der betroffenen replizierten Tabellen können die Auswirkungen auf die Leistung erheblich sein.  

In dieser Abfrage wird die dynamische Verwaltungssicht [sys.pdw_replicated_table_cache_state](https://docs.microsoft.com/sql/relational-databases/system-catalog-views/sys-pdw-replicated-table-cache-state-transact-sql) verwendet, um die replizierten Tabellen aufzulisten, die geändert, jedoch nicht neu erstellt wurden.

```sql 
SELECT [ReplicatedTable] = t.[name]
  FROM sys.tables t  
  JOIN sys.pdw_replicated_table_cache_state c  
    ON c.object_id = t.object_id 
  JOIN sys.pdw_table_distribution_properties p 
    ON p.object_id = t.object_id 
  WHERE c.[state] = 'NotReady'
    AND p.[distribution_policy_desc] = 'REPLICATE'
```
 
Um eine Neuerstellung zu erzwingen, führen Sie für jede Tabelle in der obigen Ausgabe die folgende Anweisung aus. 

```sql
SELECT TOP 1 * FROM [ReplicatedTable]
``` 
 
## <a name="next-steps"></a>Nächste Schritte 
Verwenden Sie zum Erstellen einer replizierten Tabelle eine dieser Anweisungen:

- [CREATE TABLE (Azure SQL Data Warehouse)](https://docs.microsoft.com/sql/t-sql/statements/create-table-azure-sql-data-warehouse)
- [CREATE TABLE AS SELECT (Azure SQL Data Warehouse](https://docs.microsoft.com/sql/t-sql/statements/create-table-as-select-azure-sql-data-warehouse)

Einen Überblick über verteilte Tabellen finden Sie unter [Verteilen von Tabellen in SQL Data Warehouse](sql-data-warehouse-tables-distribute.md).




