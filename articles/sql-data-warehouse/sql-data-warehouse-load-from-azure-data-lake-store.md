---
title: "Laden – Azure Data Lake Store in SQL Data Warehouse | Microsoft-Dokumentation"
description: Hier erfahren Sie, wie Sie mithilfe externer PolyBase-Tabellen Daten aus Azure Data Lake Store in Azure SQL Data Warehouse laden.
services: sql-data-warehouse
documentationcenter: NA
author: ckarst
manager: barbkess
editor: 
ms.assetid: 
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.date: 01/25/2017
ms.author: cakarst;barbkess
translationtype: Human Translation
ms.sourcegitcommit: e4ce651be6e708f284b7ca4dc32bb906ec0f14a9
ms.openlocfilehash: bba2c3fc1c96f5f8a4bb7f23db8ddab052042539


---
# <a name="load-data-from-azure-data-lake-store-into-sql-data-warehouse"></a>Laden von Daten aus Azure Data Lake Store in SQL Data Warehouse
Dieses Dokument enthält alle Schritte, die ausgeführt werden müssen, um Ihre eigenen Daten aus Azure Data Lake Store (ADLS) mithilfe von PolyBase in SQL Data Warehouse zu laden.
Für die in ADLS gespeicherten Daten können mithilfe der externen Tabellen zwar auch Ad-hoc-Abfragen ausgeführt werden, es empfiehlt sich jedoch, die Daten in SQL Data Warehouse zu importieren.
Voraussichtliche Dauer: 10 Minuten (sofern die Voraussetzungen bereits erfüllt sind).
>
In diesem Lernprogramm lernen Sie Folgendes:

1. Erstellen externer Datenbankobjekte zum Laden von Daten aus Azure Data Lake Store
2. Herstellen einer Verbindung mit einem Azure Data Lake Store-Verzeichnis
3. Laden von Daten in Azure SQL Data Warehouse

## <a name="before-you-begin"></a>Voraussetzungen
Für dieses Tutorial benötigen Sie Folgendes:

* Azure Active Directory-Anwendung für die Dienst-zu-Dienst-Authentifizierung. Eine Erstellungsanleitung finden Sie unter [Authentifizieren bei Data Lake Store mithilfe von Azure Active Directory](https://docs.microsoft.com/azure/data-lake-store/data-lake-store-authenticate-using-active-directory.md).

>[!NOTE] 
> Sie benötigen die Client-ID, den Schlüssel und den OAuth2.0-Token-Endpunktwert Ihrer Active Directory-Anwendung, um über SQL Data Warehouse eine Verbindung mit Ihrer Azure Data Lake-Instanz herstellen zu können. Ausführliche Informationen zum Ermitteln dieser Werte finden Sie unter dem oben angegebenen Link.

* SQL Server Management Studio oder SQL Server Data Tools. Informationen zum Herunterladen von SSMS sowie zum Herstellen einer Verbindung finden Sie unter [Herstellen einer Verbindung mit der SQL Data Warehouse-Instanz mit SQL Server Management Studio (SSMS)](https://docs.microsoft.com/azure/sql-data-warehouse/sql-data-warehouse-query-ssms.md).

* Eine Azure SQL Data Warehouse-Instanz. Eine Erstellungsanleitung finden Sie unter „https://docs.microsoft.com/azure/sql-data-warehouse/sql-data-warehouse-get-started-provision“.

* Eine Azure Data Lake Store-Instanz ohne aktivierte Verschlüsselung. Eine Erstellungsanleitung finden Sie unter „https://docs.microsoft.com/azure/data-lake-store/data-lake-store-get-started-portal“.




## <a name="configure-the-data-source"></a>Konfigurieren der Datenquelle
PolyBase verwendet externe T-SQL-Objekte, um den Speicherort und die Attribute der externen Daten zu definieren. Die externen Objekte werden in SQL Data Warehouse gespeichert und verweisen auf die extern gespeicherten Daten.


###  <a name="create-a-credential"></a>Erstellen einer Anmeldeinformation
Für den Zugriff auf Ihre Azure Data Lake Store-Instanz müssen Sie einen Datenbank-Hauptschlüssel erstellen, um die geheimen Anmeldeinformationen zu verschlüsseln, die Sie im nächsten Schritt verwenden.
Anschließend erstellen Sie datenbankbezogene Anmeldeinformationen, in denen die in AAD eingerichteten Dienstprinzipal-Anmeldeinformationen gespeichert werden. Bei Benutzern, die die Verbindung mit Azure Storage Blob-Instanzen über PolyBase herstellen, unterscheidet sich die Syntax für die Anmeldeinformationen.
Zum Herstellen einer Verbindung mit Azure Data Lake Store muss vor dem Erstellen datenbankbezogener Anmeldeinformationen zunächst eine Azure Active Directory-Anwendung erstellt werden.

```sql
-- A: Create a Database Master Key.
-- Only necessary if one does not already exist.
-- Required to encrypt the credential secret in the next step.
-- For more information on Master Key: https://msdn.microsoft.com/en-us/library/ms174382.aspx?f=255&MSPPError=-2147217396

CREATE MASTER KEY;


-- B: Create a database scoped credential
-- IDENTITY: Pass the client id and OAuth 2.0 Token Endpoint taken from your Azure Active Directory Application
-- SECRET: Provide your AAD Application Service Principal key.
-- For more information on Create Database Scoped Credential: https://msdn.microsoft.com/en-us/library/mt270260.aspx

CREATE DATABASE SCOPED CREDENTIAL ADL_User
WITH
    IDENTITY = '<client_id>@<OAuth_2.0_Token_EndPoint>',
    SECRET = '<key>'
;

```


### <a name="create-the-external-data-source"></a>Erstellen der externen Datenquelle
Verwenden Sie den Befehl [CREATE EXTERNAL DATA SOURCE][CREATE EXTERNAL DATA SOURCE], um den Speicherort und den Typ der Daten zu speichern.
Den ADL-URI finden Sie im Azure-Portal sowie auf „www.portal.azure.com“.

```sql
-- C: Create an external data source
-- TYPE: HADOOP - PolyBase uses Hadoop APIs to access data in Azure Data Lake Store.
-- LOCATION: Provide Azure Data Lake accountname and URI
-- CREDENTIAL: Provide the credential created in the previous step.

CREATE EXTERNAL DATA SOURCE AzureDataLakeStore
WITH (
    TYPE = HADOOP,
    LOCATION = 'adl://<AzureDataLake account_name>.azuredatalake.net',
    CREDENTIAL = AzureStorageCredential
);
```



## <a name="configure-data-format"></a>Konfigurieren des Datenformats
Zum Importieren der Daten aus ADLS müssen Sie das externe Dateiformat angeben. Dieser Befehl verfügt über formatspezifische Optionen, um Ihre Daten zu beschreiben.
Im Anschluss sehen Sie ein Beispiel für ein gängiges Dateiformat (eine Textdatei mit senkrechten Strichen als Trennzeichen).
Eine vollständige Liste für [CREATE EXTERNAL FILE FORMAT][CREATE EXTERNAL FILE FORMAT] finden Sie in unserer T-SQL-Dokumentation.

```sql
-- D: Create an external file format
-- FIELD_TERMINATOR: Marks the end of each field (column) in a delimited text file
-- STRING_DELIMITER: Specifies the field terminator for data of type string in the text-delimited file.
-- DATE_FORMAT: Specifies a custom format for all date and time data that might appear in a delimited text file.
-- Use_Type_Default: Store all Missing values as NULL

CREATE EXTERNAL FILE FORMAT TextFileFormat
WITH
(   FORMAT_TYPE = DELIMITEDTEXT
,    FORMAT_OPTIONS    (   FIELD_TERMINATOR = '|'
                    ,    STRING_DELIMITER = ''
                    ,    DATE_FORMAT         = 'yyyy-MM-dd HH:mm:ss.fff'
                    ,    USE_TYPE_DEFAULT = FALSE
                    )
);
```

## <a name="create-the-external-tables"></a>Erstellen von externen Tabellen
Nun, da Sie die Datenquelle und das Dateiformat festgelegt haben, können Sie die externen Tabellen erstellen. Externe Tabellen ermöglichen die Interaktion mit externen Daten. PolyBase durchläuft Verzeichnisse rekursiv, um alle Dateien in allen Unterverzeichnissen des im Speicherortparameter angegebenen Verzeichnisses zu lesen. Das folgende Beispiel veranschaulicht außerdem die Erstellung des Objekts. Die Anweisung muss für Ihre Daten in ADLS angepasst werden.

```sql
-- D: Create an External Table
-- LOCATION: Folder under the ADLS root folder.
-- DATA_SOURCE: Specifies which Data Source Object to use.
-- FILE_FORMAT: Specifies which File Format Object to use
-- REJECT_TYPE: Specifies how you want to deal with rejected rows. Either Value or percentage of the total
-- REJECT_VALUE: Sets the Reject value based on the reject type.

-- DimProduct
CREATE EXTERNAL TABLE [dbo].[DimProduct_external] (
    [ProductKey] [int] NOT NULL,
    [ProductLabel] [nvarchar](255) NULL,
    [ProductName] [nvarchar](500) NULL
)
WITH
(
    LOCATION='/DimProduct/'
,   DATA_SOURCE = AzureDataLakeStore
,   FILE_FORMAT = TextFileFormat
,   REJECT_TYPE = VALUE
,   REJECT_VALUE = 0
)
;

```

## <a name="external-table-considerations"></a>Überlegungen zu externen Tabellen
Eine externe Tabelle lässt sich sehr einfach erstellen, es gibt jedoch einige Feinheiten zu beachten.

Das Laden von Daten mit PolyBase ist stark typisiert. Das bedeutet, dass jede Zeile der erfassten Daten der Tabellenschemadefinition entsprechen muss.
Zeilen, die der Schemadefinition nicht entsprechen, werden nicht geladen.

Mithilfe des Ablehnungstyps und des Ablehnungswerts können Sie definieren, wie viele Zeilen oder wie viel Prozent der Daten in der endgültigen Tabelle vorhanden sein müssen.
Wird während des Ladevorgangs der Ablehnungswert erreicht, ist der Vorgang nicht erfolgreich. Die Ablehnung von Zeilen ist in den meisten Fällen auf einen Konflikt mit der Schemadefinition zurückzuführen.
Wenn also beispielsweise eine Spalte fälschlicherweise mit einem int-Schema versehen wird, obwohl es sich bei den Daten in der Datei um eine Zeichenfolge handelt, können die Zeilen nicht geladen werden.

Der Speicherort gibt das oberste Verzeichnis an, aus dem Daten gelesen werden sollen.
Wenn in diesem Fall also „/DimProduct/“ über Unterverzeichnisse verfügt, importiert PolyBase alle Daten aus diesen Unterverzeichnissen.

## <a name="load-the-data"></a>Laden der Daten
Verwenden Sie die Anweisung [CREATE TABLE AS SELECT (Transact-SQL)][CREATE TABLE AS SELECT (Transact-SQL)], um Daten aus Azure Data Lake Store zu laden. Beim Laden mit CTAS wird die stark typisierte externe Tabelle verwendet, die Sie erstellt haben.

CTAS erstellt eine neue Tabelle und füllt sie mit den Ergebnissen einer SELECT-Anweisung. CTAS definiert die neue Tabelle, sodass sie die gleichen Spalten und Datentypen wie die Ergebnisse der SELECT-Anweisung aufweist. Wenn Sie alle Spalten einer externen Tabelle auswählen, ist die neue Tabelle ein Replikat der Spalten und Datentypen aus der externen Tabelle.

In diesem Beispiel erstellen wir auf der Grundlage der externen Tabelle „DimProduct_external“ eine verteilte Hashtabelle namens „DimProduct“.

```sql

CREATE TABLE [dbo].[DimProduct]
WITH (DISTRIBUTION = HASH([ProductKey]  ) )
AS
SELECT * FROM [dbo].[DimProduct_external]
OPTION (LABEL = 'CTAS : Load [dbo].[DimProduct]');
```


## <a name="optimize-columnstore-compression"></a>Optimieren der Columnstore-Komprimierung
Standardmäßig speichert SQL Data Warehouse die Tabelle als gruppierten Columnstore-Index. Nach Abschluss eines Ladevorgangs sind einige der Datenzeilen möglicherweise nicht in den Columnstore-Index komprimiert.  Es gibt zahlreiche Gründe, warum dies geschieht. Weitere Informationen finden Sie unter [Verwalten von Columnstore-Indizes][manage columnstore indexes].

Um die Abfrageleistung und die Columnstore-Komprimierung nach dem Ladevorgang zu optimieren, stellen Sie die Tabelle wieder her, um den Columstore-Index zu zwingen alle Zeilen zu komprimieren.

```sql

ALTER INDEX ALL ON [dbo].[DimProduct] REBUILD;

```

Weitere Informationen zum Verwalten von Columnstore-Indizes finden Sie im Artikel [Verwalten von Columnstore-Indizes][manage columnstore indexes].

## <a name="optimize-statistics"></a>Optimieren von Statistiken
Es empfiehlt sich, Statistiken für einzelne Spalten sofort nach dem Ladevorgang zu erstellen. Es gibt einige Optionen für Statistiken. Beim Erstellen einer Einspaltenstatistik auf jeder Spalten vergeht möglicherweise viel Zeit bis alle Statistiken wiederhergestellt werden. Wenn Sie wissen, dass bestimmte Spalten nicht in den Abfrageprädikaten erhalten sein werden, können Sie die Erstellung von Statistiken für diese Spalten überspringen.

Wenn Sie Einzelspaltenstatistiken für jede Spalte jeder Tabelle erstellen möchten, können Sie das Codebeispiel für die gespeicherte Prozedur `prc_sqldw_create_stats` im Artikel [Statistiken][statistics] verwenden.

Das folgende Beispiel ist ein guter Ausgangspunkt zum Erstellen von Statistiken. Es werden Statistiken für einzelne Spalten für jede Spalte in der Dimensionstabelle erstellt sowie für jede verknüpfte Spalte in der Faktentabelle. Sie können später immer Statistiken für einzelne oder mehrere Spalten auf anderen Faktentabellenspalten hinzufügen.


## <a name="achievement-unlocked"></a>Der Weg ist frei!
Sie haben erfolgreich Daten in Azure SQL Data Warehouse geladen. Großartig!

##<a name="next-steps"></a>Nächste Schritte
Das Laden von Daten ist der erste Schritt auf dem Weg zu einer Data Warehouse-Lösung mit SQL Data Warehouse. Sehen Sie sich unsere Entwicklungsressourcen zu [Tabellen](https://docs.microsoft.com/azure/sql-data-warehouse/sql-data-warehouse-tables-overview) und [T-SQL](https://docs.microsoft.com/azure/sql-data-warehouse/sql-data-warehouse-develop-loops.md) an.


<!--Image references-->

<!--Article references-->
[Create a SQL Data Warehouse]: sql-data-warehouse-get-started-provision.md
[Load data into SQL Data Warehouse]: sql-data-warehouse-overview-load.md
[SQL Data Warehouse development overview]: sql-data-warehouse-overview-develop.md
[manage columnstore indexes]: sql-data-warehouse-tables-index.md
[Statistics]: sql-data-warehouse-tables-statistics.md
[CTAS]: sql-data-warehouse-develop-ctas.md
[label]: sql-data-warehouse-develop-label.md

<!--MSDN references-->
[CREATE EXTERNAL DATA SOURCE]: https://msdn.microsoft.com/library/dn935022.aspx
[CREATE EXTERNAL FILE FORMAT]: https://msdn.microsoft.com/library/dn935026.aspx
[CREATE TABLE AS SELECT (Transact-SQL)]: https://msdn.microsoft.com/library/mt204041.aspx
[sys.dm_pdw_exec_requests]: https://msdn.microsoft.com/library/mt203887.aspx
[REBUILD]: https://msdn.microsoft.com/library/ms188388.aspx

<!--Other Web references-->
[Microsoft Download Center]: http://www.microsoft.com/download/details.aspx?id=36433
[Load the full Contoso Retail Data Warehouse]: https://github.com/Microsoft/sql-server-samples/tree/master/samples/databases/contoso-data-warehouse/readme.md



<!--HONumber=Feb17_HO1-->


