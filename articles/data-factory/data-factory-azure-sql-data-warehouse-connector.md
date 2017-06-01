---
title: Verschieben von Daten in/aus Azure SQL Data Warehouse | Microsoft Docs
description: Erfahren Sie, wie Daten mithilfe von Azure Data Factory in und aus Azure SQL Data Warehouse verschoben werden.
services: data-factory
documentationcenter: 
author: linda33wj
manager: jhubbard
editor: monicar
ms.assetid: d90fa9bd-4b79-458a-8d40-e896835cfd4a
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/14/2017
ms.author: jingwang
ms.translationtype: Human Translation
ms.sourcegitcommit: e72275ffc91559a30720a2b125fbd3d7703484f0
ms.openlocfilehash: 02c5b7c8932a08bac4bc9e89bd7df3b3e5c57f94
ms.contentlocale: de-de
ms.lasthandoff: 05/05/2017


---
# <a name="move-data-to-and-from-azure-sql-data-warehouse-using-azure-data-factory"></a>Verschieben von Daten in und aus Azure SQL Data Warehouse mithilfe von Azure Data Factory
Dieser Artikel beschreibt, wie Sie die Kopieraktivität in Azure Data Factory verwenden, um Daten in und aus Azure SQL Data Warehouse zu verschieben. Dieser Artikel baut auf dem Artikel zu [Datenverschiebungsaktivitäten](data-factory-data-movement-activities.md) auf, der eine allgemeine Übersicht zur Datenverschiebung mit der Kopieraktivität bietet.  

> [!TIP]
> Das beste Ergebnis erzielen Sie, indem Sie Daten mithilfe von PolyBase in Azure SQL Data Warehouse laden. Details finden Sie im Abschnitt [Daten unter Verwendung von PolyBase in Azure SQL Data Warehouse laden](data-factory-azure-sql-data-warehouse-connector.md#use-polybase-to-load-data-into-azure-sql-data-warehouse) . Eine exemplarische Vorgehensweise mit einem Anwendungsfall finden Sie unter [Laden von 1 TB in Azure SQL Data Warehouse in weniger als 15 Minuten mit Azure Data Factory](data-factory-load-sql-data-warehouse.md).

## <a name="supported-scenarios"></a>Unterstützte Szenarien
Sie können Daten **aus Azure SQL Data Warehouse** in die folgenden Datenspeicher kopieren:

[!INCLUDE [data-factory-supported-sinks](../../includes/data-factory-supported-sinks.md)]

Sie können Daten aus den folgenden Datenspeichern **in Azure SQL Data Warehouse** kopieren:

[!INCLUDE [data-factory-supported-sources](../../includes/data-factory-supported-sources.md)]

> [!TIP]
> Beim Kopieren von Daten aus SQL Server oder Azure SQL-Datenbank in Azure SQL Data Warehouse kann Data Factory die Tabelle, wenn sie im Zielspeicher nicht vorhanden ist, anhand des Schemas im Quelldatenspeicher automatisch in SQL Data Warehouse erstellen. Einzelheiten finden Sie unter [Automatische Tabellenerstellung](#auto-table-creation).

## <a name="supported-authentication-type"></a>Unterstützter Authentifizierungstyp
Der Azure SQL Data Warehouse-Connector unterstützt Standardauthentifizierung.

## <a name="getting-started"></a>Erste Schritte
Sie können eine Pipeline mit einer Kopieraktivität erstellen, die Daten mithilfe verschiedener Tools/APIs in und aus Azure SQL Data Warehouse kopiert.

Die einfachste Möglichkeit zum Erstellen einer Pipeline, die Daten in und aus Azure SQL Data Warehouse kopiert, ist die Verwendung des Assistenten zum Kopieren von Daten. Im [Tutorial: Laden von Daten in SQL Data Warehouse mit Data Factory](../sql-data-warehouse/sql-data-warehouse-load-with-data-factory.md) finden Sie eine kurze exemplarische Vorgehensweise zum Erstellen einer Pipeline mithilfe des Assistent zum Kopieren von Daten.

Sie können auch die folgenden Tools für das Erstellen einer Pipeline verwenden: **Azure-Portal**, **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager-Vorlagen**, **.NET-API** und **REST-API**. Im [Tutorial zur Kopieraktivität](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) finden Sie detaillierte Anweisungen, wie Sie eine Pipeline mit einer Kopieraktivität erstellen können.

Unabhängig davon, ob Sie Tools oder APIs verwenden, führen Sie die folgenden Schritte aus, um eine Pipeline zu erstellen, die Daten aus einem Quelldatenspeicher in einen Senkendatenspeicher verschiebt:

1. Eine **Data Factory**. Eine Data Factory kann eine oder mehrere Pipelines enthalten. 
2. Erstellen **verknüpfter Dienste** zum Verknüpfen von Eingabe- und Ausgabedatenspeichern mit Ihrer Data Factory. Wenn Sie beispielsweise Daten aus einem Azure-Blobspeicher in eine Azure SQL Data Warehouse-Instanz kopieren, erstellen Sie zwei verknüpfte Dienste, um Ihr Azure-Speicherkonto und die Azure SQL Data Warehouse-Instanz mit Ihrer Data Factory zu verknüpfen. Informationen zu Eigenschaften von verknüpften Diensten, die spezifisch für Azure SQL Data Warehouse sind, finden Sie im Abschnitt [Eigenschaften des verknüpften Diensts](#linked-service-properties). 
3. Erstellen von **Datasets** zur Darstellung von Eingabe- und Ausgabedaten für den Kopiervorgang. Im Beispiel, das im letzten Schritt erwähnt wurde, erstellen Sie ein Dataset, um den Blobcontainer und den Ordner mit den Eingabedaten anzugeben. Und Sie erstellen ein weiteres Dataset zum Angeben der Tabelle in der Azure SQL Data Warehouse-Instanz, in der die aus dem Blobspeicher kopierten Daten enthalten sind. Informationen zu Dataset-Eigenschaften, die spezifisch für Azure SQL Data Warehouse sind, finden Sie im Abschnitt [Dataset-Eigenschaften](#dataset-properties).
4. Erstellen einer **Pipeline** mit einer Kopieraktivität, die ein Dataset als Eingabe und ein Dataset als Ausgabe akzeptiert. Im oben erwähnten Beispiel verwenden Sie „BlobSource“ als Quelle und „SqlDWSink“ als Senke für die Kopieraktivität. Wenn Sie einen Kopiervorgang von Azure SQL Data Warehouse zu Azure Blob Storage durchführen, verwenden Sie entsprechend „SqlDWSource“ und „BlobSink“ in der Kopieraktivität. Informationen zu den Eigenschaften von Kopieraktivitäten, die spezifisch für Azure SQL Data Warehouse sind, finden Sie im Abschnitt [Eigenschaften der Kopieraktivität](#copy-activity-properties). Ausführliche Informationen zur Verwendung eines Datenspeichers als Quelle oder Senke erhalten Sie, indem Sie im vorherigen Abschnitt auf den Link für Ihren Datenspeicher klicken.

Wenn Sie den Assistenten verwenden, werden automatisch JSON-Definitionen für diese Data Factory-Entitäten (verknüpfte Diensten, Datasets und die Pipeline) erstellt. Bei Verwendung von Tools und APIs (mit Ausnahme der .NET-API) definieren Sie diese Data Factory-Entitäten im JSON-Format.  Beispiele mit JSON-Definitionen für Data Factory-Entitäten für das Kopieren von Daten in und aus Azure SQL Data Warehouse finden Sie in diesem Artikel im Abschnitt [JSON-Beispiele](#json-examples-for-copying-data-to-and-from-sql-data-warehouse).

Die folgenden Abschnitte enthalten Details zu JSON-Eigenschaften, die zum Definieren von Data Factory-Entitäten speziell für Azure SQL Data Warehouse verwendet werden:

## <a name="linked-service-properties"></a>Eigenschaften des verknüpften Diensts
Die folgende Tabelle enthält eine Beschreibung der JSON-Elemente, die für den mit Azure SQL Data Warehouse verknüpften Dienst spezifisch sind.

| Eigenschaft | Beschreibung | Erforderlich |
| --- | --- | --- |
| Typ |Die "type"-Eigenschaft muss auf **AzureSqlDW** |Ja |
| connectionString |Geben Sie Informationen, die zur Verbindung mit der Azure SQL Data Warehouse-Instanz erforderlich sind, für die Eigenschaft "connectionString" ein. Es wird nur Standardauthentifizierung unterstützt. |Ja |

> [!IMPORTANT]
> Konfigurieren Sie die [Azure SQL-Datenbankfirewall](https://msdn.microsoft.com/library/azure/ee621782.aspx#ConnectingFromAzure) und den Datenbankserver, um [Azure-Diensten den Zugriff auf den Server zu ermöglichen](https://msdn.microsoft.com/library/azure/ee621782.aspx#ConnectingFromAzure). Wenn Sie nicht aus Azure stammende Daten in Azure SQL Data Warehouse kopieren, müssen Sie außerdem den entsprechenden IP-Adressbereich für den Computer konfigurieren, der Daten an Azure SQL Data Warehouse sendet. Dies gilt auch beim Kopieren von Daten aus lokalen Datenquellen mit Data Factory-Gateway.

## <a name="dataset-properties"></a>Dataset-Eigenschaften
Eine vollständige Liste der Abschnitte und Eigenschaften, die zum Definieren von Datasets zur Verfügung stehen, finden Sie im Artikel [Erstellen von Datasets](data-factory-create-datasets.md). Abschnitte wie „structure“, „availability“ und „policy“ des JSON-Codes eines Datasets sind bei allen Dataset-Typen (Azure SQL, Azure-Blob, Azure-Tabelle usw.) ähnlich.

Der Abschnitt "typeProperties" unterscheidet sich bei jedem Typ von Dataset und bietet Informationen zum Speicherort der Daten im Datenspeicher. Der Abschnitt **typeProperties** für ein Dataset des Typs **AzureSqlDWTable** weist die folgenden Eigenschaften auf:

| Eigenschaft | Beschreibung | Erforderlich |
| --- | --- | --- |
| tableName |Name der Tabelle oder Sicht in der Azure SQL Data Warehouse-Datenbank, auf die der verknüpfte Dienst verweist. |Ja |

## <a name="copy-activity-properties"></a>Eigenschaften der Kopieraktivität
Eine vollständige Liste der Abschnitte und Eigenschaften zum Definieren von Aktivitäten finden Sie im Artikel [Erstellen von Pipelines](data-factory-create-pipelines.md). Eigenschaften wie Name, Beschreibung, Eingabe- und Ausgabetabellen und Richtlinie sind für alle Arten von Aktivitäten verfügbar.

> [!NOTE]
> Die Kopieraktivität verwendet nur eine Eingabe und erzeugt nur eine Ausgabe.

Eigenschaften im Abschnitt typeProperties der Aktivität können dagegen je nach Aktivitätstyp variieren. Für die Kopieraktivität variieren die Eigenschaften je nach Art der Quellen und Senken.

### <a name="sqldwsource"></a>SqlDWSource
Bei einer Quelle vom Typ **SqlDWSource** sind im Abschnitt **typeProperties** folgende Eigenschaften verfügbar:

| Eigenschaft | Beschreibung | Zulässige Werte | Erforderlich |
| --- | --- | --- | --- |
| SqlReaderQuery |Verwendet die benutzerdefinierte Abfrage zum Lesen von Daten. |SQL-Abfragezeichenfolge. Beispiel: select * from MyTable. |Nein |
| sqlReaderStoredProcedureName |Der Name der gespeicherten Prozedur, die Daten aus der Quelltabelle liest. |Name der gespeicherten Prozedur. Die letzte SQL-Anweisung muss eine SELECT-Anweisung in der gespeicherten Prozedur sein. |Nein |
| storedProcedureParameters |Parameter für die gespeicherte Prozedur. |Name-Wert-Paare. Die Namen und die Groß-/Kleinschreibung von Parametern müssen denen der Parameter der gespeicherten Prozedur entsprechen. |Nein |

Wenn **sqlReaderQuery** für "SqlDWSource" angegeben ist, führt die Kopieraktivität diese Abfrage für die Azure SQL Data Warehouse-Quelle aus, um die Daten abzurufen.

Alternativ dazu können Sie eine gespeicherte Prozedur angeben, indem Sie **sqlReaderStoredProcedureName** und **storedProcedureParameters** angeben (sofern die gespeicherten Prozeduren Parameter verwenden).

Ohne Angabe von „sqlReaderQuery“ oder „sqlReaderStoredProcedureName“ werden die im Strukturabschnitt des DataSet-JSON-Bereichs definierten Spalten verwendet, um eine Abfrage für SQL Data Warehouse zu erstellen. Beispiel: `select column1, column2 from mytable`. Falls die DataSet-Definition nicht über die Struktur verfügt, werden alle Spalten der Tabelle ausgewählt.

#### <a name="sqldwsource-example"></a>Beispiel für "SqlDWSource"

```JSON
"source": {
    "type": "SqlDWSource",
    "sqlReaderStoredProcedureName": "CopyTestSrcStoredProcedureWithParameters",
    "storedProcedureParameters": {
        "stringData": { "value": "str3" },
        "identifier": { "value": "$$Text.Format('{0:yyyy}', SliceStart)", "type": "Int"}
    }
}
```
**Die Definition der gespeicherten Prozedur:**

```SQL
CREATE PROCEDURE CopyTestSrcStoredProcedureWithParameters
(
    @stringData varchar(20),
    @identifier int
)
AS
SET NOCOUNT ON;
BEGIN
     select *
     from dbo.UnitTestSrcTable
     where dbo.UnitTestSrcTable.stringData != stringData
    and dbo.UnitTestSrcTable.identifier != identifier
END
GO
```

### <a name="sqldwsink"></a>SqlDWSink
**SqlDWSink** unterstützt die folgenden Eigenschaften:

| Eigenschaft | Beschreibung | Zulässige Werte | Erforderlich |
| --- | --- | --- | --- |
| sqlWriterCleanupScript |Geben Sie eine Abfrage für die Kopieraktivität an, bei deren Ausführung die Daten eines bestimmten Slice bereinigt werden. Ausführlichere Informationen finden Sie im [Abschnitt zur Wiederholbarkeit](#repeatability-during-copy). |Eine Abfrageanweisung. |Nein |
| allowPolyBase |Gibt an, ob (falls zutreffend) PolyBase anstelle des BULKINSERT-Mechanismus verwendet werden soll. <br/><br/> **as empfohlene Verfahren zum Laden von Daten in SQL Data Warehouse ist PolyBase.** Einschränkungen und Einzelheiten finden Sie im Abschnitt [Daten unter Verwendung von PolyBase in Azure SQL Data Warehouse laden](#use-polybase-to-load-data-into-azure-sql-data-warehouse) . |True  <br/>False (Standardwert) |Nein |
| polyBaseSettings |Eine Gruppe von Eigenschaften, die angegeben werden können, wenn die **allowPolybase**-Eigenschaft auf **true** festgelegt ist. |&nbsp; |Nein |
| rejectValue |Gibt die Anzahl oder den Prozentsatz von Zeilen an, die abgelehnt werden können, bevor für die Abfrage ein Fehler auftritt. <br/><br/>Weitere Informationen zu den PolyBase-Ablehnungsoptionen finden Sie im Abschnitt **Argumente** des Themas [CREATE EXTERNAL TABLE (Transact-SQL)](https://msdn.microsoft.com/library/dn935021.aspx) . |0 (Standardwert), 1, 2, … |Nein |
| rejectType |Gibt an, ob die rejectValue-Option als Literalwert oder Prozentsatz angegeben ist. |Value (Standardwert), Percentage |Nein |
| rejectSampleValue |Gibt die Anzahl von Zeilen an, die abgerufen werden, bevor PolyBase den Prozentsatz der abgelehnten Zeilen neu berechnet. |1, 2, … |Ja, wenn für **rejectType** der Wert **percentage** festgelegt ist. |
| useTypeDefault |Gibt an, wie fehlende Werte in durch Trennzeichen getrennten Textdateien verarbeitet werden sollen, wenn PolyBase Daten aus der Textdatei abruft.<br/><br/>Weitere Informationen zu dieser Eigenschaft finden Sie im Abschnitt zu Argumenten im Thema [CREATE EXTERNAL FILE FORMAT (Transact-SQL)](https://msdn.microsoft.com/library/dn935026.aspx)verwenden können. |True/False (Standardwert) |Nein |
| writeBatchSize |Fügt Daten in die SQL-Tabelle ein, wenn die Puffergröße "writeBatchSize" erreicht. |Integer (Gesamtanzahl von Zeilen) |Nein (Standard = 10000) |
| writeBatchTimeout |Die Wartezeit für den Abschluss der Batcheinfügung, bis das Timeout wirksam wird. |Zeitraum<br/><br/> Beispiel: 00:30:00 (30 Minuten) |Nein |

#### <a name="sqldwsink-example"></a>Beispiel für "SqlDWSink"

```JSON
"sink": {
    "type": "SqlDWSink",
    "allowPolyBase": true
}
```

## <a name="use-polybase-to-load-data-into-azure-sql-data-warehouse"></a>Daten unter Verwendung von PolyBase in Azure SQL Data Warehouse laden
Mit **[PolyBase](https://docs.microsoft.com/sql/relational-databases/polybase/polybase-guide)** lassen sich große Datenmengen effizient und mit hohem Durchsatz in Azure SQL Data Warehouse laden. Wenn Sie PolyBase anstelle des standardmäßigen BULKINSERT-Mechanismus verwenden, wird der Durchsatz erheblich gesteigert. Einen ausführlichen Vergleich finden Sie in der [Leistungsreferenz zur Kopieraktivität](data-factory-copy-activity-performance.md#performance-reference). Eine exemplarische Vorgehensweise mit einem Anwendungsfall finden Sie unter [Laden von 1 TB in Azure SQL Data Warehouse in weniger als 15 Minuten mit Azure Data Factory](data-factory-load-sql-data-warehouse.md).

* Wenn sich die Quelldaten in **Azure Blob Storage oder Azure Data Lake Store** befinden und ihr Format mit PolyBase kompatibel ist, können Sie sie mithilfe von PolyBase direkt nach Azure SQL Data Warehouse kopieren. Details finden Sie unter **[Direktes Kopieren mithilfe von PolyBase](#direct-copy-using-polybase)**.
* Wenn der Speicher und das Format der Quelldaten von PolyBase ursprünglich nicht unterstützt wird, können Sie stattdessen das Feature **[Gestaffeltes Kopieren mit PolyBase](#staged-copy-using-polybase)** verwenden. Es bietet auch einen besseren Durchsatz, da die Daten automatisch in ein PolyBase-kompatibles Format konvertiert und in Azure Blob Storage gespeichert werden. Anschließend werden die Daten in SQL Data Warehouse geladen.

Legen Sie die `allowPolyBase`-Eigenschaft auf **true** fest, wie im folgenden Beispiel für Azure Data Factory gezeigt, um Daten unter Verwendung von PolyBase in Azure SQL Data Warehouse zu kopieren. Wenn Sie „allowPolyBase“ auf „true“ festlegen, können Sie über die `polyBaseSettings`-Eigenschaftengruppe PolyBase-spezifische Eigenschaften festlegen. Im Abschnitt [SqlDWSink](#SqlDWSink) finden Sie Einzelheiten zu den Eigenschaften, die mit „polyBaseSettings“ verwendet werden können.

```JSON
"sink": {
    "type": "SqlDWSink",
    "allowPolyBase": true,
    "polyBaseSettings":
    {
        "rejectType": "percentage",
        "rejectValue": 10.0,
        "rejectSampleValue": 100,
        "useTypeDefault": true
    }
}
```

### <a name="direct-copy-using-polybase"></a>Direktes Kopieren mithilfe von PolyBase
PolyBase in SQL Data Warehouse bietet direkte Unterstützung für Azure Blob Storage und Azure Data Lake Store (mit dem Dienstprinzipal) als Quelle und mit bestimmten Anforderungen an das Datendateiformat. Wenn Ihre Daten die in diesem Abschnitt beschriebenen Kriterien erfüllen, können Sie mithilfe von PolyBase direkt aus dem Quelldatenspeicher in Azure SQL Data Warehouse kopieren. Andernfalls können Sie [gestaffeltes Kopieren mit PolyBase](#staged-copy-using-polybase)verwenden.

> [!TIP]
> Weitere Informationen zum effizienten Kopieren von Daten aus Data Lake Store nach SQL Data Warehouse finden Sie unter [Azure Data Factory makes it even easier and convenient to uncover insights from data when using Data Lake Store with SQL Data Warehouse](https://blogs.msdn.microsoft.com/azuredatalake/2017/04/08/azure-data-factory-makes-it-even-easier-and-convenient-to-uncover-insights-from-data-when-using-data-lake-store-with-sql-data-warehouse/) (Wenn Data Lake Store mit SQL Data Warehouse verwendet wird, lassen sich mit Azure Data Factory noch einfacher Erkenntnisse aus Daten gewinnen) (in englischer Sprache).

Falls die Anforderungen nicht erfüllt werden, überprüft Azure Data Factory die Einstellungen und greift bei der Datenverschiebung automatisch auf den BULKINSERT-Mechanismus zurück.

1. Der **mit der Quelle verknüpfte Dienst** ist vom Typ **AzureStorage** oder **AzureDataLakeStore mit Dienstprinzipalauthentifizierung**.  
2. Das **Eingabedataset** ist vom Typ **AzureBlob** oder **AzureDataLakeStore**, und der Formattyp unter den `type`-Eigenschaften lautet **OrcFormat** oder **TextFormat** mit folgenden Konfigurationen:

   1. `rowDelimiter` muss **\n** sein.
   2. `nullValue` ist auf eine **leere Zeichenfolge** ("") festgelegt, oder `treatEmptyAsNull` ist auf **true** festgelegt.
   3. `encodingName` ist auf **utf-8** festgelegt. (Dies ist der **Standardwert**.)
   4. `escapeChar`, `quoteChar`, `firstRowAsHeader` und `skipLineCount` sind nicht angegeben.
   5. `compression` kann auf **keine Komprimierung** oder auf **Gzip** oder **Deflate** (Verkleinern) festgelegt sein.

    ```JSON
    "typeProperties": {
       "folderPath": "<blobpath>",
       "format": {
           "type": "TextFormat",     
           "columnDelimiter": "<any delimiter>",
           "rowDelimiter": "\n",       
           "nullValue": "",           
           "encodingName": "utf-8"    
       },
       "compression": {  
           "type": "GZip",  
           "level": "Optimal"  
       }  
    },
    ```

3. Für die Kopieraktivität in der Pipeline ist unter **BlobSource** oder **AzureDataLakeStore** keine `skipHeaderLineCount`-Einstellung vorhanden.
4. Für die Kopieraktivität in der Pipeline ist unter **SqlDWSink** keine `sliceIdentifierColumnName`-Einstellung vorhanden. (PolyBase stellt sicher, dass in einem Durchlauf entweder alle oder keine Daten aktualisiert werden). Um **Wiederholbarkeit** zu erreichen, kann `sqlWriterCleanupScript` verwendet werden.)
5. In der zugehörigen Kopieraktivität wird `columnMapping` nicht verwendet.

### <a name="staged-copy-using-polybase"></a>gestaffeltes Kopieren mit PolyBase
Falls Ihre Quelldaten die Kriterien aus dem vorherigen Abschnitt nicht erfüllen, können Sie die Daten über einen zwischengeschalteten Azure-Stagingblobspeicher kopieren (darf nicht Storage Premium sein). In diesem Fall führt Azure Data Factory die erforderlichen Transformationen für die Daten automatisch durch, um die Datenformatanforderungen von PolyBase zu erfüllen. Laden Sie anschließend die Daten mithilfe von PolyBase in SQL Data Warehouse, und bereinigen Sie zumindest die temporären Daten im Blob Storage. Unter [Gestaffeltes Kopieren](data-factory-copy-activity-performance.md#staged-copy) finden Sie ausführliche Informationen zur allgemeinen Funktionsweise des Kopierens von Daten über ein Azure-Stagingblob.

> [!NOTE]
> Wenn Sie beim Kopieren von Daten aus einem lokalen Datenspeicher in Azure SQL Data Warehouse mithilfe von PolyBase und einem Stagingverfahren ein Datenverwaltungsgateway mit einer Version vor 2.4 verwenden, ist JRE (Java Runtime Environment) auf dem Gatewaycomputer erforderlich, mit dem die Quelldaten in das richtige Format transformiert werden. Es wird empfohlen, dass Sie Ihr Gateway auf die aktuelle Version aktualisieren, um eine solche Abhängigkeit zu vermeiden.
>

Erstellen Sie zum Verwenden dieses Features einen [verknüpften Azure Storage-Dienst](data-factory-azure-blob-connector.md#azure-storage-linked-service), der auf das Azure Storage-Konto mit dem zwischengeschalteten Blobspeicher verweist, und geben Sie dann für die Kopieraktivität die Eigenschaften `enableStaging` und `stagingSettings` an, wie im folgenden Code zu sehen:

```json
"activities":[  
{
    "name": "Sample copy activity from SQL Server to SQL Data Warehouse via PolyBase",
    "type": "Copy",
    "inputs": [{ "name": "OnpremisesSQLServerInput" }],
    "outputs": [{ "name": "AzureSQLDWOutput" }],
    "typeProperties": {
        "source": {
            "type": "SqlSource",
        },
        "sink": {
            "type": "SqlDwSink",
            "allowPolyBase": true
        },
        "enableStaging": true,
        "stagingSettings": {
            "linkedServiceName": "MyStagingBlob"
        }
    }
}
]
```

## <a name="best-practices-when-using-polybase"></a>Bewährte Methoden bei Verwendung von PolyBase
Die folgenden Abschnitte enthalten bewährte Methoden zusätzlich zu den in [Bewährte Methoden für Azure SQL Data Warehouse](../sql-data-warehouse/sql-data-warehouse-best-practices.md) aufgeführten Vorgängen.

### <a name="required-database-permission"></a>Erforderliche Datenbankberechtigungen
Um PolyBase verwenden zu können, muss der zum Laden von Daten in SQL Data Warehouse verwendete Benutzer über die [Berechtigung „CONTROL“](https://msdn.microsoft.com/library/ms191291.aspx) für die Zieldatenbank verfügen. Sie können dies beispielsweise erreichen, indem Sie diesen Benutzer als Mitglied der Rolle „db_owner“ hinzufügen. [In diesem Abschnitt](../sql-data-warehouse/sql-data-warehouse-overview-manage-security.md#authorization) erfahren Sie, wie das geht.

### <a name="row-size-and-data-type-limitation"></a>Beschränkungen hinsichtlich Zeilengröße und Datentyp
Der Ladevorgang in PolyBase ist auf das Laden von Zeilen beschränkt, die kleiner als **1 MB** sind und nicht in Spalten des Typs VARCHR(MAX), NVARCHAR(MAX) oder VARBINARY(MAX) geladen werden können. Informationen finden Sie [hier](../sql-data-warehouse/sql-data-warehouse-service-capacity-limits.md#loads).

Wenn Sie über Quelldaten mit Zeilen verfügen, deren Größe über 1 MB liegt, sollten Sie die Quelltabellen vertikal in kleinere Tabellen unterteilen, sodass die Zeilengröße der einzelnen Tabellen den Höchstwert nicht überschreitet. Die kleineren Tabellen können dann mit PolyBase geladen und in Azure SQL Data Warehouse zusammengeführt werden.

### <a name="sql-data-warehouse-resource-class"></a>SQL Data Warehouse-Ressourcenklasse
Um einen optimalen Durchsatz zu erzielen, sollten Sie dem Benutzer, der zum Laden von Daten in SQL Data Warehouse über PolyBase verwendet wird, eine größere Ressourcenklasse zuweisen. Erfahren Sie, wie das geht, indem Sie das folgende [Beispiel: Ändern der Ressourcenklasse eines Benutzers](../sql-data-warehouse/sql-data-warehouse-develop-concurrency.md#change-a-user-resource-class-example) ausführen.

### <a name="tablename-in-azure-sql-data-warehouse"></a>„tableName“ in Azure SQL Data Warehouse
Die folgende Tabelle enthält Beispiele für das Angeben der **tableName** -Eigenschaft in Dataset-JSON für diverse Kombinationen aus Schema und Tabellenname:

| Datenbankschema | Tabellenname | JSON-Eigenschaft „tableName“ |
| --- | --- | --- |
| dbo |MyTable |MyTable oder dbo.MyTable oder [dbo].[MyTable] |
| dbo1 |MyTable |dbo1.MyTable oder [dbo1].[MyTable] |
| dbo |My.Table |[My.Table] oder [dbo].[My.Table] |
| dbo1 |My.Table |[dbo1].[My.Table] |

Sollte der folgende Fehler auftreten, liegt dies unter Umständen am Wert für die tableName-Eigenschaft. Informationen zur korrekten Angabe von Werten für die JSON-Eigenschaft „tableName“ finden Sie in der Tabelle.  

```
Type=System.Data.SqlClient.SqlException,Message=Invalid object name 'stg.Account_test'.,Source=.Net SqlClient Data Provider
```

### <a name="columns-with-default-values"></a>Spalten mit Standardwerten
Das PolyBase-Feature in Data Factory akzeptiert aktuell lediglich dieselbe Anzahl von Spalten wie in der Zieltabelle. Angenommen, Sie verfügen über eine Tabelle mit vier Spalten, von denen eine mit einem Standardwert definiert ist. Die Eingabedaten müssen trotzdem vier Spalten umfassen. Bei Bereitstellung eines Eingabedatasets mit drei Spalten tritt ein Fehler wie der folgende auf:

```
All columns of the table must be specified in the INSERT BULK statement.
```
Der NULL-Wert ist eine Sonderform eines Standardwerts. Wenn die Spalte NULL-Werte zulässt, könnten die Eingabedaten (im Blob) für diese Spalte leer sein (sie dürfen im Eingabedataset nicht fehlen). PolyBase fügt für diese Daten in Azure SQL Data Warehouse den Wert „NULL“ ein.  

## <a name="auto-table-creation"></a>Automatische Tabellenerstellung
Wenn Sie den Kopier-Assistenten zum Kopieren von Daten aus SQL Server oder Azure SQL-Datenbank in Azure SQL Data Warehouse verwenden, kann Data Factory eine Tabelle aus dem Quellspeicher, wenn diese im Zielspeicher nicht vorhanden ist, anhand des Quelltabellenschemas automatisch in Data Warehouse erstellen.

Data Factory erstellt die Tabelle im Zielspeicher mit dem gleichen Tabellennamen wie im Quelldatenspeicher. Die Datentypen für die Spalten werden basierend auf der folgenden Typzuordnung ausgewählt. Bei Bedarf werden Typkonvertierungen durchgeführt, um Inkompatibilitäten zwischen Quell- und Zielspeichern zu korrigieren. Darüber hinaus wird für Tabellen die Roundrobin-Verteilung verwendet.

| SQL-Datenbank-Quellspaltentyp | SQL DW-Zielspaltentyp (Größenbeschränkung) |
| --- | --- |
| int | int |
| BigInt | BigInt |
| SmallInt | SmallInt |
| TinyInt | TinyInt |
| Bit | Bit |
| Decimal | Decimal |
| Numeric | Decimal |
| Float | Float |
| Money | Money |
| Real | Real |
| SmallMoney | SmallMoney |
| Binär | Binär |
| Varbinary | Varbinary (maximal 8000) |
| Datum | Datum |
| DateTime | DateTime |
| DateTime2 | DateTime2 |
| Time | Time |
| Datetimeoffset | Datetimeoffset |
| SmallDateTime | SmallDateTime |
| Text | Varchar (maximal 8000) |
| NText | NVarChar (maximal 4000) |
| Image | VarBinary (maximal 8000) |
| UniqueIdentifier | UniqueIdentifier |
| Char | Char |
| NChar | NChar |
| VarChar | VarChar (maximal 8000) |
| NVarChar | NVarChar (maximal 4000) |
| xml | Varchar (maximal 8000) |

[!INCLUDE [data-factory-type-repeatability-for-sql-sources](../../includes/data-factory-type-repeatability-for-sql-sources.md)]

### <a name="type-mapping-for-azure-sql-data-warehouse"></a>Typzuordnung für Azure SQL Data Warehouse
Wie im Artikel [Datenverschiebungsaktivitäten](data-factory-data-movement-activities.md) beschrieben, führt die Kopieraktivität mithilfe der folgenden beiden Schritte automatische Typkonvertierungen von Quelltypen in Senkentypen durch:

1. Konvertieren von systemeigenen Quelltypen in den .NET-Typ
2. Konvertieren vom .NET-Typ in systemeigenen Senkentyp

Beim Verschieben von Daten in und aus Azure SQL, SQL Server und Sybase werden die folgenden Zuordnungen zwischen SQL-Typ und .NET-Typ und umgekehrt verwendet.

Die Zuordnung ist mit der [SQL Server-Datentypzuordnung für ADO.NET](https://msdn.microsoft.com/library/cc716729.aspx)identisch.

| Typ "SQL Server-Datenbankmodul" | Typ ".NET Framework" |
| --- | --- |
| bigint |Int64 |
| binary |Byte[] |
| Bit |Boolean |
| char |String, Char[] |
| date |DateTime |
| DateTime |DateTime |
| datetime2 |DateTime |
| Datetimeoffset |Datetimeoffset |
| Decimal |Decimal |
| FILESTREAM-Attribut (varbinary(max)) |Byte[] |
| Float |Doppelt |
| image |Byte[] |
| int |Int32 |
| money |Decimal |
| nchar |String, Char[] |
| ntext |String, Char[] |
| numeric |Decimal |
| nvarchar |String, Char[] |
| real |Single |
| rowversion |Byte[] |
| smalldatetime |DateTime |
| smallint |Int16 |
| smallmoney |Decimal |
| sql_variant |Object * |
| text |String, Char[] |
| in |Zeitraum |
| timestamp |Byte[] |
| tinyint |Byte |
| uniqueidentifier |GUID |
| varbinary |Byte[] |
| varchar |String, Char[] |
| xml |xml |

Sie können in der Definition der Kopieraktivität auch Spalten aus dem Quelldataset Spalten im Senkendataset zuordnen. Weitere Informationen finden Sie unter [Zuordnen von Datasetspalten in Azure Data Factory](data-factory-map-columns.md).

## <a name="json-examples-for-copying-data-to-and-from-sql-data-warehouse"></a>JSON-Beispiele zum Kopieren von Daten in bzw. aus SQL Data Warehouse
Die folgenden Beispiele zeigen JSON-Beispieldefinitionen, die Sie zum Erstellen einer Pipeline mit dem [Azure-Portal](data-factory-copy-activity-tutorial-using-azure-portal.md), mit [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) oder mit [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md) verwenden können. Sie zeigen Ihnen das Kopieren von Daten in und aus Azure SQL Data Warehouse und Azure Blob Storage. Daten können jedoch mithilfe der Kopieraktivität in Azure Data Factory **direkt** aus beliebigen Quellen in die [hier](data-factory-data-movement-activities.md#supported-data-stores-and-formats) aufgeführten Senken kopiert werden.

### <a name="example-copy-data-from-azure-sql-data-warehouse-to-azure-blob"></a>Beispiel: Kopieren von Daten aus Azure SQL Data Warehouse in ein Azure-Blob
Im Beispiel werden folgende Data Factory-Entitäten definiert:

1. Einen verknüpften Dienst des Typs [AzureSqlDW](#linked-service-properties)
2. Einen verknüpften Dienst des Typs [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties)
3. Ein [Eingabedataset](data-factory-create-datasets.md) des Typs [AzureSqlDWTable](#dataset-properties)
4. Ein [Ausgabedataset](data-factory-create-datasets.md) des Typs [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties)
5. Eine [Pipeline](data-factory-create-pipelines.md) mit Kopieraktivität, die [SqlDWSource](#copy-activity-properties) und [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties) verwendet

In dem Beispiel werden jede Stunde Zeitreihendaten (stündlich, täglich usw.) aus einer Tabelle in einer Azure SQL Data Warehouse-Datenbank in ein Blob kopiert. Die bei diesen Beispielen verwendeten JSON-Eigenschaften werden in den Abschnitten beschrieben, die auf die Beispiele folgen.

**Mit Azure SQL Data Warehouse verknüpfter Dienst:**

```JSON
{
  "name": "AzureSqlDWLinkedService",
  "properties": {
    "type": "AzureSqlDW",
    "typeProperties": {
      "connectionString": "Server=tcp:<servername>.database.windows.net,1433;Database=<databasename>;User ID=<username>@<servername>;Password=<password>;Trusted_Connection=False;Encrypt=True;Connection Timeout=30"
    }
  }
}
```
**Mit Azure-Blobspeicher verknüpfter Dienst:**

```JSON
{
  "name": "StorageLinkedService",
  "properties": {
    "type": "AzureStorage",
    "typeProperties": {
      "connectionString": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"
    }
  }
}
```
**Azure SQL Data Warehouse-Eingabedataset:**

Im Beispiel wird vorausgesetzt, dass Sie die Tabelle "MyTable" in Azure SQL Data Warehouse erstellt haben, die für Zeitreihendaten eine Spalte namens "timestampcolumn" enthält.

Durch Festlegen von „external“ auf „true“ wird dem Data Factory-Dienst mitgeteilt, dass das Dataset für die Data Factory extern ist und nicht durch eine Aktivität in der Data Factory erzeugt wird.

```JSON
{
  "name": "AzureSqlDWInput",
  "properties": {
    "type": "AzureSqlDWTable",
    "linkedServiceName": "AzureSqlDWLinkedService",
    "typeProperties": {
      "tableName": "MyTable"
    },
    "external": true,
    "availability": {
      "frequency": "Hour",
      "interval": 1
    },
    "policy": {
      "externalData": {
        "retryInterval": "00:01:00",
        "retryTimeout": "00:10:00",
        "maximumRetry": 3
      }
    }
  }
}
```
**Azure-Blob-Ausgabedataset:**

Daten werden stündlich in ein neues Blob geschrieben ("frequency": "hour", "interval": 1). Der Ordnerpfad des Blobs wird basierend auf der Startzeit des Slices, der verarbeitet wird, dynamisch ausgewertet. Im Ordnerpfad werden Jahr, Monat, Tag und die Stundenteile der Startzeit verwendet.

```JSON
{
  "name": "AzureBlobOutput",
  "properties": {
    "type": "AzureBlob",
    "linkedServiceName": "StorageLinkedService",
    "typeProperties": {
      "folderPath": "mycontainer/myfolder/yearno={Year}/monthno={Month}/dayno={Day}/hourno={Hour}",
      "partitionedBy": [
        {
          "name": "Year",
          "value": {
            "type": "DateTime",
            "date": "SliceStart",
            "format": "yyyy"
          }
        },
        {
          "name": "Month",
          "value": {
            "type": "DateTime",
            "date": "SliceStart",
            "format": "MM"
          }
        },
        {
          "name": "Day",
          "value": {
            "type": "DateTime",
            "date": "SliceStart",
            "format": "dd"
          }
        },
        {
          "name": "Hour",
          "value": {
            "type": "DateTime",
            "date": "SliceStart",
            "format": "HH"
          }
        }
      ],
      "format": {
        "type": "TextFormat",
        "columnDelimiter": "\t",
        "rowDelimiter": "\n"
      }
    },
    "availability": {
      "frequency": "Hour",
      "interval": 1
    }
  }
}
```

**Kopieraktivität in einer Pipeline mit „SqlDWSource“ und „BlobSink“:**

Die Pipeline enthält eine Kopieraktivität, die für die Verwendung der Ein- und Ausgabedatasets und für eine stündliche Ausführung konfiguriert ist. In der JSON-Definition der Pipeline ist der Typ **source** auf **SqlDWSource** und der Typ **sink** auf **BlobSink** festgelegt. Die für die **SqlReaderQuery** -Eigenschaft angegebene SQL-Abfrage wählt die Daten der letzten Stunde zum Kopieren aus.

```JSON
{  
    "name":"SamplePipeline",
    "properties":{  
    "start":"2014-06-01T18:00:00",
    "end":"2014-06-01T19:00:00",
    "description":"pipeline for copy activity",
    "activities":[  
      {
        "name": "AzureSQLDWtoBlob",
        "description": "copy activity",
        "type": "Copy",
        "inputs": [
          {
            "name": "AzureSqlDWInput"
          }
        ],
        "outputs": [
          {
            "name": "AzureBlobOutput"
          }
        ],
        "typeProperties": {
          "source": {
            "type": "SqlDWSource",
            "sqlReaderQuery": "$$Text.Format('select * from MyTable where timestampcolumn >= \\'{0:yyyy-MM-dd HH:mm}\\' AND timestampcolumn < \\'{1:yyyy-MM-dd HH:mm}\\'', WindowStart, WindowEnd)"
          },
          "sink": {
            "type": "BlobSink"
          }
        },
       "scheduler": {
          "frequency": "Hour",
          "interval": 1
        },
        "policy": {
          "concurrency": 1,
          "executionPriorityOrder": "OldestFirst",
          "retry": 0,
          "timeout": "01:00:00"
        }
      }
     ]
   }
}
```
> [!NOTE]
> In dem Beispiel wird **sqlReaderQuery** für „SqlSWSource“ angegeben. Mit der Kopieraktivität wird diese Abfrage für die Azure SQL Data Warehouse-Quelle ausgeführt, um die Daten abzurufen.
>
> Alternativ dazu können Sie eine gespeicherte Prozedur angeben, indem Sie **sqlReaderStoredProcedureName** und **storedProcedureParameters** angeben (sofern die gespeicherten Prozeduren Parameter verwenden).
>
> Wenn Sie "sqlReaderQuery" oder "sqlReaderStoredProcedureName" nicht angeben, werden die im Strukturabschnitt des DataSet-JSON-Bereichs definierten Spalten verwendet, um eine Abfrage ("select column1, column2 from mytable") zur Ausführung für das SQL Data Warehouse zu erstellen. Falls die DataSet-Definition nicht über die Struktur verfügt, werden alle Spalten der Tabelle ausgewählt.
>
>

### <a name="example-copy-data-from-azure-blob-to-azure-sql-data-warehouse"></a>Beispiel: Kopieren von Daten aus einem Azure-Blob in Azure SQL Data Warehouse
Im Beispiel werden folgende Data Factory-Entitäten definiert:

1. Einen verknüpften Dienst des Typs [AzureSqlDW](#linked-service-properties)
2. Einen verknüpften Dienst des Typs [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties)
3. Ein [Eingabedataset](data-factory-create-datasets.md) des Typs [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties)
4. Ein [Ausgabedataset](data-factory-create-datasets.md) des Typs [AzureSqlDWTable](#dataset-properties)
5. Eine [Pipeline](data-factory-create-pipelines.md) mit Kopieraktivität, die [BlobSource](data-factory-azure-blob-connector.md#copy-activity-properties) und [SqlDWSink](#copy-activity-properties) verwendet

In dem Beispiel werden jede Stunde Zeitreihendaten (stündlich, täglich usw.) aus einem Azure-Blob in eine Tabelle in einer Azure SQL Data Warehouse-Datenbank kopiert. Die bei diesen Beispielen verwendeten JSON-Eigenschaften werden in den Abschnitten beschrieben, die auf die Beispiele folgen.

**Mit Azure SQL Data Warehouse verknüpfter Dienst:**

```JSON
{
  "name": "AzureSqlDWLinkedService",
  "properties": {
    "type": "AzureSqlDW",
    "typeProperties": {
      "connectionString": "Server=tcp:<servername>.database.windows.net,1433;Database=<databasename>;User ID=<username>@<servername>;Password=<password>;Trusted_Connection=False;Encrypt=True;Connection Timeout=30"
    }
  }
}
```
**Mit Azure-Blobspeicher verknüpfter Dienst:**

```JSON
{
  "name": "StorageLinkedService",
  "properties": {
    "type": "AzureStorage",
    "typeProperties": {
      "connectionString": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"
    }
  }
}
```
**Azure-Blob-Eingabedataset:**

Daten werden stündlich aus einem neuen Blob übernommen ("frequency": "hour", "interval": 1). Ordnerpfad und Dateiname des Blobs werden basierend auf der Startzeit des Slices, der verarbeitet wird, dynamisch ausgewertet. Der Ordnerpfad enthält das Jahr, den Monat und den Tag der Startzeit, der Dateiname enthält die Stunde der Startzeit. Die Festlegung von „external“ auf „true“ teilt dem Data Factory-Dienst mit, dass diese Tabelle für die Data Factory extern ist und nicht durch eine Aktivität in der Data Factory erzeugt wird.

```JSON
{
  "name": "AzureBlobInput",
  "properties": {
    "type": "AzureBlob",
    "linkedServiceName": "StorageLinkedService",
    "typeProperties": {
      "folderPath": "mycontainer/myfolder/yearno={Year}/monthno={Month}/dayno={Day}",
      "fileName": "{Hour}.csv",
      "partitionedBy": [
        {
          "name": "Year",
          "value": {
            "type": "DateTime",
            "date": "SliceStart",
            "format": "yyyy"
          }
        },
        {
          "name": "Month",
          "value": {
            "type": "DateTime",
            "date": "SliceStart",
            "format": "MM"
          }
        },
        {
          "name": "Day",
          "value": {
            "type": "DateTime",
            "date": "SliceStart",
            "format": "dd"
          }
        },
        {
          "name": "Hour",
          "value": {
            "type": "DateTime",
            "date": "SliceStart",
            "format": "HH"
          }
        }
      ],
      "format": {
        "type": "TextFormat",
        "columnDelimiter": ",",
        "rowDelimiter": "\n"
      }
    },
    "external": true,
    "availability": {
      "frequency": "Hour",
      "interval": 1
    },
    "policy": {
      "externalData": {
        "retryInterval": "00:01:00",
        "retryTimeout": "00:10:00",
        "maximumRetry": 3
      }
    }
  }
}
```
**Azure SQL Data Warehouse-Ausgabedataset:**

Das Beispiel kopiert Daten in eine Tabelle namens "MyTable" in einer Azure SQL Data Warehouse-Datenbank. Erstellen Sie die Tabelle in Azure SQL Data Warehouse mit der gleichen Anzahl von Spalten, die Sie auch in der Blob-CSV-Datei erwarten. Neue Zeilen werden der Tabelle stündlich hinzugefügt.

```JSON
{
  "name": "AzureSqlDWOutput",
  "properties": {
    "type": "AzureSqlDWTable",
    "linkedServiceName": "AzureSqlDWLinkedService",
    "typeProperties": {
      "tableName": "MyOutputTable"
    },
    "availability": {
      "frequency": "Hour",
      "interval": 1
    }
  }
}
```
**Kopieraktivität in einer Pipeline mit BlobSource und SqlDWSink:**

Die Pipeline enthält eine Kopieraktivität, die für die Verwendung der Ein- und Ausgabedatasets und für eine stündliche Ausführung konfiguriert ist. In der JSON-Definition der Pipeline ist der Typ **source** auf **BlobSource** und der Typ **sink** auf **SqlDWSink** festgelegt.

```JSON
{  
    "name":"SamplePipeline",
    "properties":{  
    "start":"2014-06-01T18:00:00",
    "end":"2014-06-01T19:00:00",
    "description":"pipeline with copy activity",
    "activities":[  
      {
        "name": "AzureBlobtoSQLDW",
        "description": "Copy Activity",
        "type": "Copy",
        "inputs": [
          {
            "name": "AzureBlobInput"
          }
        ],
        "outputs": [
          {
            "name": "AzureSqlDWOutput"
          }
        ],
        "typeProperties": {
          "source": {
            "type": "BlobSource",
            "blobColumnSeparators": ","
          },
          "sink": {
            "type": "SqlDWSink",
            "allowPolyBase": true
          }
        },
       "scheduler": {
          "frequency": "Hour",
          "interval": 1
        },
        "policy": {
          "concurrency": 1,
          "executionPriorityOrder": "OldestFirst",
          "retry": 0,
          "timeout": "01:00:00"
        }
      }
      ]
   }
}
```
Eine exemplarische Vorgehensweise finden Sie unter [Laden von 1 TB in Azure SQL Data Warehouse in weniger als 15 Minuten mit Azure Data Factory](data-factory-load-sql-data-warehouse.md) sowie im Artikel [Laden von Daten in SQL Data Warehouse mit Data Factory](../sql-data-warehouse/sql-data-warehouse-get-started-load-with-azure-data-factory.md) der Azure SQL Data Warehouse-Dokumentation.

## <a name="performance-and-tuning"></a>Leistung und Optimierung
Der Artikel [Handbuch zur Leistung und Optimierung der Kopieraktivität](data-factory-copy-activity-performance.md) beschreibt wichtige Faktoren, die sich auf die Leistung der Datenverschiebung (Kopieraktivität) in Azure Data Factory auswirken, sowie verschiedene Möglichkeiten zur Leistungsoptimierung.

