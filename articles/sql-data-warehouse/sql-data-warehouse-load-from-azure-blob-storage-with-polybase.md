<properties
   pageTitle="Laden von Daten aus Azure Blob Storage in SQL Data Warehouse (PolyBase) | Microsoft Azure"
   description="Erfahren Sie, wie Sie PolyBase verwenden, um Daten aus Azure Blob Storage in SQL Data Warehouse zu laden. Laden Sie einige Tabellen aus öffentlichen Daten in das Contoso Retail-Data Warehouse-Schema."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="ckarst"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="08/25/2016"
   ms.author="cakarst;barbkess;sonyama"/>


# Laden von Daten aus Azure Blob Storage in SQL Data Warehouse (PolyBase)

> [AZURE.SELECTOR]
- [Data Factory](sql-data-warehouse-load-from-azure-blob-storage-with-data-factory.md)
- [PolyBase](sql-data-warehouse-load-from-azure-blob-storage-with-polybase.md)

Verwenden Sie PolyBase- und T-SQL-Befehle, um Daten aus Azure Blob Storage in Azure SQL Data Warehouse zu laden.

Der Einfachheit halber lädt dieses Tutorial nur zwei Tabellen aus einem öffentlichen Azure Storage-Blob in das Contoso Retail-Data Warehouse-Schema. Folgen Sie im Repository für Microsoft SQL Server-Beispiele dem Beispiel [Laden des kompletten Contoso Retail-Data Warehouse][], um das komplette DataSet zu laden.

In diesem Lernprogramm führen Sie folgende Schritte aus:

1. Konfigurieren von PolyBase zum Laden aus Azure Blob Storage
2. Laden von öffentlichen Daten in Ihre Datenbank
3. Durchführen von Optimierungen, nachdem das Laden abgeschlossen ist


## Voraussetzungen
Sie benötigen ein Azure-Konto, das bereits über eine SQL Data Warehouse-Datenbank verfügt, um dieses Tutorial ausführen zu können. Wenn Sie noch über keines verfügen, gehen Sie unter [Erstellen eines SQL Data Warehouse][].

## 1\. Konfigurieren der Datenquelle

PolyBase verwendet externe T-SQL-Objekte, um den Speicherort und die Attribute der externen Daten zu definieren. Die externen Objektdefinitionen werden in SQL Data Warehouse gespeichert. Die Daten selbst werden extern gespeichert.

### 1\.1. Erstellen einer Anmeldeinformation

**Überspringen Sie diesen Schritt**, wenn Sie die öffentliche Contoso-Daten laden. Sie benötigen keinen sicheren Zugriff auf die öffentlichen Daten, da diese schon für jeden zugänglich sind.

**Überspringen Sie diesen Schritt nicht**, wenn Sie dieses Tutorial als Vorlage für das Laden Ihrer eigenen Daten verwenden. Für den Zugriff auf die Daten durch eine Anmeldeinformation, verwenden Sie das folgende Skript zum Erstellen einer datenbankbezogenen Anmeldeinformation, und verwenden Sie es anschließend zum Definieren des Speicherorts der Datenquelle.


```sql
-- A: Create a master key.
-- Only necessary if one does not already exist.
-- Required to encrypt the credential secret in the next step.

CREATE MASTER KEY;


-- B: Create a database scoped credential
-- IDENTITY: Provide any string, it is not used for authentication to Azure storage.
-- SECRET: Provide your Azure storage account key.


CREATE DATABASE SCOPED CREDENTIAL AzureStorageCredential
WITH
    IDENTITY = 'user',
    SECRET = '<azure_storage_account_key>'
;


-- C: Create an external data source
-- TYPE: HADOOP - PolyBase uses Hadoop APIs to access data in Azure blob storage.
-- LOCATION: Provide Azure storage account name and blob container name.
-- CREDENTIAL: Provide the credential created in the previous step.

CREATE EXTERNAL DATA SOURCE AzureStorage
WITH (
    TYPE = HADOOP,
    LOCATION = 'wasbs://<blob_container_name>@<azure_storage_account_name>.blob.core.windows.net',
    CREDENTIAL = AzureStorageCredential
);
```

Fahren Sie mit Schritt 2 fort.

### 1\.2. Erstellen der externen Datenquelle

Verwenden Sie den Befehl [CREATE EXTERNAL DATA SOURCE][], um den Speicherort und den Typ der Daten zu speichern.

```sql
CREATE EXTERNAL DATA SOURCE AzureStorage_west_public
WITH 
(  
    TYPE = Hadoop 
,   LOCATION = 'wasbs://contosoretaildw-tables@contosoretaildw.blob.core.windows.net/'
); 
```

> [AZURE.IMPORTANT] Wenn Sie Ihre Azure Blob Storage-Container öffentlich machen möchten, sollten Sie bedenken, dass Ihnen als Datenbesitzer Datenausgangsgebühren in Rechnung gestellt werden, wenn Daten das Rechenzentrum verlassen.

## 2\. Konfigurieren des Datenformats

Die Daten werden in Textdateien in Azure Blob Storage gespeichert, und jedes Feld wird durch ein Trennzeichen getrennt. Führen Sie diesen [CREATE EXTERNAL FILE FORMAT][]-Befehl aus, um das Format der Daten in den Textdateien anzugeben. Die Contoso-Dateien sind nicht komprimiert und nicht durch senkrechte Striche getrennt.

```sql
CREATE EXTERNAL FILE FORMAT TextFileFormat 
WITH 
(   FORMAT_TYPE = DELIMITEDTEXT
,	FORMAT_OPTIONS	(   FIELD_TERMINATOR = '|'
					,	STRING_DELIMITER = ''
					,	DATE_FORMAT		 = 'yyyy-MM-dd HH:mm:ss.fff'
					,	USE_TYPE_DEFAULT = FALSE 
					)
);
``` 

## 3\. Erstellen von externen Tabellen

Nun, da Sie die Datenquelle und das Dateiformat festgelegt haben, können Sie die externen Tabellen erstellen.

### 3\.1. Erstellen eines Schemas für die Daten 

Erstellen Sie ein Schema, um einen Speicherort für die Contoso-Daten in der Datenbank zu erstellen.

```sql
CREATE SCHEMA [asb]
GO
```

### 3\.2. Erstellen von externen Tabellen 

Führen Sie dieses Skript zum Erstellen der externen DimProduct und FactOnlineSales-Tabellen aus. Wir definieren hier nur Tabellennamen und Datentypen und binden diese an den Speicherort und an das Format der Azure Blob Storage-Dateien Die Definition wird in SQL Data Warehouse gespeichert und die Daten befinden sich weiterhin im Azure Storage-Blob.

Der Parameter **LOCATION** ist der Ordner im Stammordner im Azure Storage-Blob. Jede Tabelle befindet sich in einem anderen Ordner.


```sql

--DimProduct
CREATE EXTERNAL TABLE [asb].DimProduct (
	[ProductKey] [int] NOT NULL,
	[ProductLabel] [nvarchar](255) NULL,
	[ProductName] [nvarchar](500) NULL,
	[ProductDescription] [nvarchar](400) NULL,
	[ProductSubcategoryKey] [int] NULL,
	[Manufacturer] [nvarchar](50) NULL,
	[BrandName] [nvarchar](50) NULL,
	[ClassID] [nvarchar](10) NULL,
	[ClassName] [nvarchar](20) NULL,
	[StyleID] [nvarchar](10) NULL,
	[StyleName] [nvarchar](20) NULL,
	[ColorID] [nvarchar](10) NULL,
	[ColorName] [nvarchar](20) NOT NULL,
	[Size] [nvarchar](50) NULL,
	[SizeRange] [nvarchar](50) NULL,
	[SizeUnitMeasureID] [nvarchar](20) NULL,
	[Weight] [float] NULL,
	[WeightUnitMeasureID] [nvarchar](20) NULL,
	[UnitOfMeasureID] [nvarchar](10) NULL,
	[UnitOfMeasureName] [nvarchar](40) NULL,
	[StockTypeID] [nvarchar](10) NULL,
	[StockTypeName] [nvarchar](40) NULL,
	[UnitCost] [money] NULL,
	[UnitPrice] [money] NULL,
	[AvailableForSaleDate] [datetime] NULL,
	[StopSaleDate] [datetime] NULL,
	[Status] [nvarchar](7) NULL,
	[ImageURL] [nvarchar](150) NULL,
	[ProductURL] [nvarchar](150) NULL,
	[ETLLoadID] [int] NULL,
	[LoadDate] [datetime] NULL,
	[UpdateDate] [datetime] NULL
)
WITH
(
    LOCATION='/DimProduct/' 
,   DATA_SOURCE = AzureStorage_west_public
,   FILE_FORMAT = TextFileFormat
,   REJECT_TYPE = VALUE
,   REJECT_VALUE = 0
)
;
 
--FactOnlineSales
CREATE EXTERNAL TABLE [asb].FactOnlineSales 
(
	[OnlineSalesKey] [int]  NOT NULL,
	[DateKey] [datetime] NOT NULL,
	[StoreKey] [int] NOT NULL,
	[ProductKey] [int] NOT NULL,
	[PromotionKey] [int] NOT NULL,
	[CurrencyKey] [int] NOT NULL,
	[CustomerKey] [int] NOT NULL,
	[SalesOrderNumber] [nvarchar](20) NOT NULL,
	[SalesOrderLineNumber] [int] NULL,
	[SalesQuantity] [int] NOT NULL,
	[SalesAmount] [money] NOT NULL,
	[ReturnQuantity] [int] NOT NULL,
	[ReturnAmount] [money] NULL,
	[DiscountQuantity] [int] NULL,
	[DiscountAmount] [money] NULL,
	[TotalCost] [money] NOT NULL,
	[UnitCost] [money] NULL,
	[UnitPrice] [money] NULL,
	[ETLLoadID] [int] NULL,
	[LoadDate] [datetime] NULL,
	[UpdateDate] [datetime] NULL
)
WITH
(
    LOCATION='/FactOnlineSales/' 
,   DATA_SOURCE = AzureStorage_west_public
,   FILE_FORMAT = TextFileFormat
,   REJECT_TYPE = VALUE
,   REJECT_VALUE = 0
)
;
```

## 4\. Laden der Daten
Es gibt verschiedene Arten, auf externe Daten zuzugreifen. Sie können Daten direkt aus der externen Tabelle abfragen, Daten in neue Datenbanktabellen laden oder externe Daten zu vorhandenen Datenbanktabellen hinzufügen.


### 4\.1. Erstellen eines neuen Schemas

CTAS erstellt eine neue Tabelle, die Daten enthält. Erstellen Sie zunächst ein Schema für die Contoso-Daten.

```sql
CREATE SCHEMA [cso]
GO
```

### 4\.2. Laden der Daten in neue Tabellen

Um Daten aus einem Azure-Blobspeicher zu laden und in einer Tabelle innerhalb Ihrer Datenbank zu speichern, verwenden Sie die Anweisung [CREATE TABLE AS SELECT (Transact-SQL)][]. Das Laden mit CTAS nutzt die stark typisierten externe Tabellen, die Sie gerade erstellt haben. Verwenden Sie eine [CTAS][]-Anweisung pro Tabelle, um die Daten in neue Tabellen zu laden.

CTAS erstellt eine neue Tabelle und füllt sie mit den Ergebnissen einer SELECT-Anweisung. CTAS definiert die neue Tabelle, sodass sie die gleichen Spalten und Datentypen wie die Ergebnisse der SELECT-Anweisung aufweist. Wenn Sie alle Spalten einer externen Tabelle auswählen, wird die neue Tabelle ein Replikat der Spalten und Datentypen in der externen Tabelle sein.

In diesem Beispiel erstellen wir sowohl die Dimensions- als auch die Faktentabelle als verteilte Hashtabellen.


```sql
SELECT GETDATE();
GO

CREATE TABLE [cso].[DimProduct]            WITH (DISTRIBUTION = HASH([ProductKey]  ) ) AS SELECT * FROM [asb].[DimProduct]             OPTION (LABEL = 'CTAS : Load [cso].[DimProduct]             ');
CREATE TABLE [cso].[FactOnlineSales]       WITH (DISTRIBUTION = HASH([ProductKey]  ) ) AS SELECT * FROM [asb].[FactOnlineSales]        OPTION (LABEL = 'CTAS : Load [cso].[FactOnlineSales]        ');
```

### 4\.3 Verfolgen des Ladeprozesses

Sie können den Ladefortschritt mithilfe der dynamischen Verwaltungsansicht (Dynamic Management View, DMV) verfolgen.

```sql
-- To see all requests
SELECT * FROM sys.dm_pdw_exec_requests;

-- To see a particular request identified by its label
SELECT * FROM sys.dm_pdw_exec_requests as r;
WHERE r.[label] = 'CTAS : Load [cso].[DimProduct]             '
      OR r.[label] = 'CTAS : Load [cso].[FactOnlineSales]        '
;

-- To track bytes and files
SELECT
    r.command,
    s.request_id,
    r.status,
    count(distinct input_name) as nbr_files, 
    sum(s.bytes_processed)/1024/1024 as gb_processed
FROM
    sys.dm_pdw_exec_requests r
    inner join sys.dm_pdw_dms_external_work s
        on r.request_id = s.request_id
WHERE 
    r.[label] = 'CTAS : Load [cso].[DimProduct]             '
    OR r.[label] = 'CTAS : Load [cso].[FactOnlineSales]        '
GROUP BY
    r.command,
    s.request_id,
    r.status
ORDER BY
    nbr_files desc,
    gb_processed desc;
```

## 5\. Optimieren der Columnstore-Komprimierung

Standardmäßig speichert SQL Data Warehouse die Tabelle als gruppierten Columnstore-Index. Nach Abschluss eines Ladevorgangs sind einige der Datenzeilen möglicherweise nicht in den Columnstore-Index komprimiert. Es gibt zahlreiche Gründe, warum dies geschieht. Weitere Informationen finden Sie unter [Verwalten von Columnstore-Indizes][].

Um die Abfrageleistung und die Columnstore-Komprimierung nach dem Ladevorgang zu optimieren, stellen Sie die Tabelle wieder her, um den Columstore-Index zu zwingen alle Zeilen zu komprimieren.

```sql
SELECT GETDATE();
GO

ALTER INDEX ALL ON [cso].[DimProduct]               REBUILD;
ALTER INDEX ALL ON [cso].[FactOnlineSales]          REBUILD;
```

Weitere Informationen zum Verwalten von Columnstore-Indizes finden Sie im Artikel [Verwalten von Columnstore-Indizes][].

## 6\. Optimieren von Statistiken

Es empfiehlt sich, Statistiken für einzelne Spalten sofort nach dem Ladevorgang zu erstellen. Es gibt einige Optionen für Statistiken. Beim Erstellen einer Einspaltenstatistik auf jeder Spalten vergeht möglicherweise viel Zeit bis alle Statistiken wiederhergestellt werden. Wenn Sie wissen, dass bestimmte Spalten nicht in den Abfrageprädikaten erhalten sein werden, können Sie die Erstellung von Statistiken für diese Spalten überspringen.

Wenn Sie Einzelspaltenstatistiken für jede Spalte jeder Tabelle erstellen möchten, können Sie das Codebeispiel für die gespeicherte Prozedur `prc_sqldw_create_stats` im Artikel [Statistiken][] verwenden.

Das folgende Beispiel ist ein guter Ausgangspunkt zum Erstellen von Statistiken. Es werden Statistiken für einzelne Spalten für jede Spalte in der Dimensionstabelle erstellt sowie für jede verknüpfte Spalte in der Faktentabelle. Sie können später immer Statistiken für einzelne oder mehrere Spalten auf anderen Faktentabellenspalten hinzufügen.


```sql
CREATE STATISTICS [stat_cso_DimProduct_AvailableForSaleDate] ON [cso].[DimProduct]([AvailableForSaleDate]);
CREATE STATISTICS [stat_cso_DimProduct_BrandName] ON [cso].[DimProduct]([BrandName]);
CREATE STATISTICS [stat_cso_DimProduct_ClassID] ON [cso].[DimProduct]([ClassID]);
CREATE STATISTICS [stat_cso_DimProduct_ClassName] ON [cso].[DimProduct]([ClassName]);
CREATE STATISTICS [stat_cso_DimProduct_ColorID] ON [cso].[DimProduct]([ColorID]);
CREATE STATISTICS [stat_cso_DimProduct_ColorName] ON [cso].[DimProduct]([ColorName]);
CREATE STATISTICS [stat_cso_DimProduct_ETLLoadID] ON [cso].[DimProduct]([ETLLoadID]);
CREATE STATISTICS [stat_cso_DimProduct_ImageURL] ON [cso].[DimProduct]([ImageURL]);
CREATE STATISTICS [stat_cso_DimProduct_LoadDate] ON [cso].[DimProduct]([LoadDate]);
CREATE STATISTICS [stat_cso_DimProduct_Manufacturer] ON [cso].[DimProduct]([Manufacturer]);
CREATE STATISTICS [stat_cso_DimProduct_ProductDescription] ON [cso].[DimProduct]([ProductDescription]);
CREATE STATISTICS [stat_cso_DimProduct_ProductKey] ON [cso].[DimProduct]([ProductKey]);
CREATE STATISTICS [stat_cso_DimProduct_ProductLabel] ON [cso].[DimProduct]([ProductLabel]);
CREATE STATISTICS [stat_cso_DimProduct_ProductName] ON [cso].[DimProduct]([ProductName]);
CREATE STATISTICS [stat_cso_DimProduct_ProductSubcategoryKey] ON [cso].[DimProduct]([ProductSubcategoryKey]);
CREATE STATISTICS [stat_cso_DimProduct_ProductURL] ON [cso].[DimProduct]([ProductURL]);
CREATE STATISTICS [stat_cso_DimProduct_Size] ON [cso].[DimProduct]([Size]);
CREATE STATISTICS [stat_cso_DimProduct_SizeRange] ON [cso].[DimProduct]([SizeRange]);
CREATE STATISTICS [stat_cso_DimProduct_SizeUnitMeasureID] ON [cso].[DimProduct]([SizeUnitMeasureID]);
CREATE STATISTICS [stat_cso_DimProduct_Status] ON [cso].[DimProduct]([Status]);
CREATE STATISTICS [stat_cso_DimProduct_StockTypeID] ON [cso].[DimProduct]([StockTypeID]);
CREATE STATISTICS [stat_cso_DimProduct_StockTypeName] ON [cso].[DimProduct]([StockTypeName]);
CREATE STATISTICS [stat_cso_DimProduct_StopSaleDate] ON [cso].[DimProduct]([StopSaleDate]);
CREATE STATISTICS [stat_cso_DimProduct_StyleID] ON [cso].[DimProduct]([StyleID]);
CREATE STATISTICS [stat_cso_DimProduct_StyleName] ON [cso].[DimProduct]([StyleName]);
CREATE STATISTICS [stat_cso_DimProduct_UnitCost] ON [cso].[DimProduct]([UnitCost]);
CREATE STATISTICS [stat_cso_DimProduct_UnitOfMeasureID] ON [cso].[DimProduct]([UnitOfMeasureID]);
CREATE STATISTICS [stat_cso_DimProduct_UnitOfMeasureName] ON [cso].[DimProduct]([UnitOfMeasureName]);
CREATE STATISTICS [stat_cso_DimProduct_UnitPrice] ON [cso].[DimProduct]([UnitPrice]);
CREATE STATISTICS [stat_cso_DimProduct_UpdateDate] ON [cso].[DimProduct]([UpdateDate]);
CREATE STATISTICS [stat_cso_DimProduct_Weight] ON [cso].[DimProduct]([Weight]);
CREATE STATISTICS [stat_cso_DimProduct_WeightUnitMeasureID] ON [cso].[DimProduct]([WeightUnitMeasureID]);
CREATE STATISTICS [stat_cso_FactOnlineSales_CurrencyKey] ON [cso].[FactOnlineSales]([CurrencyKey]);
CREATE STATISTICS [stat_cso_FactOnlineSales_CustomerKey] ON [cso].[FactOnlineSales]([CustomerKey]);
CREATE STATISTICS [stat_cso_FactOnlineSales_DateKey] ON [cso].[FactOnlineSales]([DateKey]);
CREATE STATISTICS [stat_cso_FactOnlineSales_OnlineSalesKey] ON [cso].[FactOnlineSales]([OnlineSalesKey]);
CREATE STATISTICS [stat_cso_FactOnlineSales_ProductKey] ON [cso].[FactOnlineSales]([ProductKey]);
CREATE STATISTICS [stat_cso_FactOnlineSales_PromotionKey] ON [cso].[FactOnlineSales]([PromotionKey]);
CREATE STATISTICS [stat_cso_FactOnlineSales_StoreKey] ON [cso].[FactOnlineSales]([StoreKey]);
```

## Der Weg ist frei!

Sie haben erfolgreich öffentliche Daten in Azure SQL Data Warehouse geladen. Großartig!

Jetzt können Sie die Tabellen mit Abfragen wie die folgenden abfragen:

```sql
SELECT  SUM(f.[SalesAmount]) AS [sales_by_brand_amount]
,       p.[BrandName]
FROM    [cso].[FactOnlineSales] AS f
JOIN    [cso].[DimProduct]      AS p ON f.[ProductKey] = p.[ProductKey]
GROUP BY p.[BrandName]
```

## Nächste Schritte
Verwenden Sie das Skript unter [Entwurfsentscheidungen und Programmiertechniken für SQL Data Warehouse][], um die vollständigen Contoso Retail-Data Warehouse-Daten zu laden.

<!--Image references-->

<!--Article references-->
[Erstellen eines SQL Data Warehouse]: sql-data-warehouse-get-started-provision.md
[Load data into SQL Data Warehouse]: sql-data-warehouse-overview-load.md
[Entwurfsentscheidungen und Programmiertechniken für SQL Data Warehouse]: sql-data-warehouse-overview-develop.md
[Verwalten von Columnstore-Indizes]: sql-data-warehouse-tables-index.md
[Statistiken]: sql-data-warehouse-tables-statistics.md
[CTAS]: sql-data-warehouse-develop-ctas.md
[label]: sql-data-warehouse-develop-label.md

<!--MSDN references-->
[CREATE EXTERNAL DATA SOURCE]: https://msdn.microsoft.com/de-DE/library/dn935022.aspx
[CREATE EXTERNAL FILE FORMAT]: https://msdn.microsoft.com/de-DE/library/dn935026.aspx
[CREATE TABLE AS SELECT (Transact-SQL)]: https://msdn.microsoft.com/library/mt204041.aspx
[sys.dm_pdw_exec_requests]: https://msdn.microsoft.com/library/mt203887.aspx
[REBUILD]: https://msdn.microsoft.com/library/ms188388.aspx

<!--Other Web references-->
[Microsoft Download Center]: http://www.microsoft.com/download/details.aspx?id=36433
[Laden des kompletten Contoso Retail-Data Warehouse]: https://github.com/Microsoft/sql-server-samples/tree/master/samples/databases/contoso-data-warehouse/readme.md

<!---HONumber=AcomDC_0907_2016-->