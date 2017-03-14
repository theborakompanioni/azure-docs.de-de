---
title: "Push-Übertragung von Daten in einen Search-Index mithilfe von Data Factory | Microsoft Docs"
description: "Erfahren Sie mehr über die Push-Übertragung von Daten in den Azure Search-Index mithilfe von Azure Data Factory."
services: data-factory
documentationcenter: 
author: linda33wj
manager: jhubbard
editor: monicar
ms.assetid: f8d46e1e-5c37-4408-80fb-c54be532a4ab
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/08/2017
ms.author: jingwang
translationtype: Human Translation
ms.sourcegitcommit: 55c988bf74ff0f2e519e895a735dc68f3dc99855
ms.openlocfilehash: e2deed13106db9467eef181f25a0a226034df5a2
ms.lasthandoff: 12/21/2016

---

# <a name="push-data-to-an-azure-search-index-by-using-azure-data-factory"></a>Push-Übertragung von Daten in den Azure Search-Index mithilfe von Azure Data Factory
In diesem Artikel wird beschrieben, wie Sie die Kopieraktivität verwenden können, um eine Push-Übertragung von Daten aus einem lokalen, vom Data Factory-Dienst unterstützten Datenspeicher in den Azure Search-Index durchführen. In der Spalte „Quelle“ der Tabelle [Unterstützte Datenquellen und Senken](data-factory-data-movement-activities.md#supported-data-stores-and-formats) sind unterstützte Quelldatenspeicher aufgelistet. Dieser Artikel baut auf dem Artikel [Datenverschiebungsaktivitäten](data-factory-data-movement-activities.md) auf, der eine allgemeine Übersicht zur Datenverschiebung mit Kopieraktivität und unterstützten Datenspeicherkombinationen bietet.

## <a name="enabling-connectivity"></a>Herstellen der Verbindung
Installieren Sie das Datenverwaltungsgateway in Ihrer lokalen Umgebung, um die Verbindung des Data Factory-Diensts mit einem lokalen Datenspeicher zuzulassen. Sie können das Gateway auf dem Computer, der den Quelldatenspeicher hostet oder auf einem separaten Computer installieren, um zu vermeiden, dass der Computer mit dem Datenspeicher um Ressourcen konkurriert.

Das Datenverwaltungsgateway verbindet die lokalen Datenquellen mit Cloud-Diensten auf sichere und verwaltete Weise. Weitere Informationen zum Datenverwaltungsgateway finden Sie im Artikel [Verschieben von Daten zwischen lokalen Quellen und der Cloud](data-factory-move-data-between-onprem-and-cloud.md) .

## <a name="copy-data-wizard"></a>Assistent zum Kopieren von Daten
Die einfachste Art, eine Pipeline zu erstellen, die Daten in Azure Search aus einem der unterstützten Datenspeicher kopiert, ist die Verwendung des Assistenten zum Kopieren von Daten. Eine kurze exemplarische Vorgehensweise finden Sie im [Tutorial: Erstellen einer Pipeline mit dem Kopier-Assistenten](data-factory-copy-data-wizard-tutorial.md).

Die folgenden Beispiele zeigen JSON-Beispieldefinitionen, die Sie zum Erstellen einer Pipeline mit dem [Azure-Portal](data-factory-copy-activity-tutorial-using-azure-portal.md), mit [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) oder [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md) verwenden können. Darin wird veranschaulicht, wie Sie Daten aus einem lokalen SQL Server in Azure Search-Index kopieren. Daten können jedoch auch mithilfe der Kopieraktivität in Azure Data Factory direkt aus den [hier](data-factory-data-movement-activities.md#supported-data-stores-and-formats) aufgeführten lokalen Datenspeichern kopiert werden.   

## <a name="sample-copy-data-from-on-premises-sql-server-to-azure-search-index"></a>Beispiel: Kopieren von Daten aus einem lokalen SQL Server in Azure Search-Index

Dieses Beispiel zeigt Folgendes:

1.    Einen verknüpften Dienst des Typs [AzureSearch](#azure-search-linked-service-properties).
2.    Einen verknüpften Dienst des Typs [OnPremisesSqlServer](data-factory-sqlserver-connector.md#sql-server-linked-service-properties)
3.    Ein [Eingabedataset](data-factory-create-datasets.md) des Typs [SqlServerTable](data-factory-sqlserver-connector.md#sql-server-dataset-type-properties).
4.    Ein [Ausgabedataset](data-factory-create-datasets.md) des Typs [AzureSearchIndex](#azure-search-index-dataset-properties).
4.    Eine [Pipeline](data-factory-create-pipelines.md) mit Kopieraktivität, die [SqlSource](data-factory-sqlserver-connector.md#sql-server-copy-activity-type-properties) und [AzureSearchIndexSink](#azure-search-index-sink-properties) verwendet.

In diesem Beispiel werden Zeitreihendaten aus einer lokalen SQL Server-Datenbank stündlich in den Azure Search-Index kopiert. Die in diesem Beispiel verwendeten JSON-Eigenschaften werden in den Abschnitten beschrieben, die auf die Beispiele folgen.

Als Erstes richten Sie das Datenverwaltungsgateway auf Ihrem lokalen Computer ein. Anweisungen dazu finden Sie im Artikel [Verschieben von Daten zwischen lokalen Standorten und Cloud](data-factory-move-data-between-onprem-and-cloud.md) .

**Mit Azure Search verknüpfter Dienst:**

```JSON
{
    "name": "AzureSearchLinkedService",
       "properties": {
        "type": "AzureSearch",
           "typeProperties": {
            "url": "https://<service>.search.windows.net",
            "key": "<AdminKey>"
        }
       }
}
```

**Mit SQL Server verknüpfter Dienst**

```JSON
{
  "Name": "SqlServerLinkedService",
  "properties": {
    "type": "OnPremisesSqlServer",
    "typeProperties": {
      "connectionString": "Data Source=<servername>;Initial Catalog=<databasename>;Integrated Security=False;User ID=<username>;Password=<password>;",
      "gatewayName": "<gatewayname>"
    }
  }
}
```

**SQL Server-Eingabedataset**

Im Beispiel wird vorausgesetzt, dass Sie die Tabelle "MyTable" in SQL Server erstellt haben, die für Zeitreihendaten eine Spalte namens "timestampcolumn" enthält. Sie können mehrere Tabellen in derselben Datenbank mithilfe eines einzigen Datasets abfragen, aber für die typeProperty-Eigenschaft tableName des Datasets muss eine einzelne Tabelle verwendet werden.

Durch Festlegen von „external“ auf „true“ wird dem Data Factory-Dienst mitgeteilt, dass das Dataset für die Data Factory extern ist und nicht durch eine Aktivität in der Data Factory erzeugt wird.

```JSON
{
  "name": "SqlServerDataset",
  "properties": {
    "type": "SqlServerTable",
    "linkedServiceName": "SqlServerLinkedService",
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

**Azure Search-Ausgabedataset:**

In diesem Beispiel werden Daten in einen Azure Search-Index namens **Produkte** kopiert. Data Factory erstellt den Index nicht. Um das Beispiel zu testen, erstellen Sie einen Index mit diesem Namen. Erstellen Sie den Azure Search-Index mit der gleichen Anzahl Spalten wie im Eingabedataset. Neue Einträge werden stündlich zum Azure Search-Index hinzugefügt.

```JSON
{
    "name": "AzureSearchIndexDataset",
    "properties": {
        "type": "AzureSearchIndex",
        "linkedServiceName": "AzureSearchLinkedService",
         "typeProperties" : {
            "indexName": "products",
        },
        "availability": {
            "frequency": "Minute",
            "interval": 15
        }
   }
}
```

**Pipeline mit Kopieraktivität:**

Die Pipeline enthält eine Kopieraktivität, die für die Verwendung der Ein- und Ausgabedatasets und für eine stündliche Ausführung konfiguriert ist. In der JSON-Definition der Pipeline ist der Typ **source** auf **SqlSource** und der Typ **sink** auf **AzureSearchIndexSink** festgelegt. Die für die **SqlReaderQuery** -Eigenschaft angegebene SQL-Abfrage wählt die Daten der letzten Stunde zum Kopieren aus.

```JSON
{  
    "name":"SamplePipeline",
    "properties":{  
    "start":"2014-06-01T18:00:00",
    "end":"2014-06-01T19:00:00",
    "description":"pipeline for copy activity",
    "activities":[  
      {
        "name": "SqlServertoAzureSearchIndex",
        "description": "copy activity",
        "type": "Copy",
        "inputs": [
          {
            "name": " SqlServerInput"
          }
        ],
        "outputs": [
          {
            "name": "AzureSearchIndexDataset"
          }
        ],
        "typeProperties": {
          "source": {
            "type": "SqlSource",
            "SqlReaderQuery": "$$Text.Format('select * from MyTable where timestampcolumn >= \\'{0:yyyy-MM-dd HH:mm}\\' AND timestampcolumn < \\'{1:yyyy-MM-dd HH:mm}\\'', WindowStart, WindowEnd)"
          },
          "sink": {
            "type": "AzureSearchIndexSink"
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

Wenn Sie Daten aus einem Clouddatenspeicher in Azure Search kopieren, ist die `executionLocation`-Eigenschaft erforderlich. Im Folgenden werden als Beispiel die Änderung gezeigt, die unter der Kopieraktivität `typeProperties` erforderlich sind. Im Abschnitt [Kopieren von Daten zwischen Clouddatenspeichern](data-factory-data-movement-activities.md#global) finden Sie Informationen zu den unterstützten Werten sowie weitere Details.

```JSON
"typeProperties": {
  "source": {
    "type": "BlobSource"
  },
  "sink": {
    "type": "AzureSearchIndexSink"
  },
  "executionLocation": "West US"
}
```

## <a name="azure-search-linked-service-properties"></a>Eigenschaften des mit Azure Search verknüpften Diensts

Die folgende Tabelle enthält Beschreibungen der JSON-Elemente, die für den mit Azure Search verknüpften Dienst spezifisch sind.

| Eigenschaft | Beschreibung | Erforderlich |
| -------- | ----------- | -------- |
| Typ | Die Typeigenschaft muss auf **AzureSearch** festgelegt werden. | Ja |
| URL | URL für den Azure Search-Dienst. | Ja |
| key | Admin-Schlüssel für den Azure Search-Dienst. | Ja |

## <a name="azure-search-index-dataset-properties"></a>Azure Search-Index Dataset-Eigenschaften

Eine vollständige Liste mit den Abschnitten und Eigenschaften, die zum Definieren von Datasets zur Verfügung stehen, finden Sie im Artikel [Erstellen von Datasets](data-factory-create-datasets.md) . Abschnitte wie „structure“, „availability“ und „policy“ des JSON-Codes eines Datasets sind bei allen Dataset-Typen ähnlich. Der Abschnitt **typeProperties** ist bei jeder Art von Dataset unterschiedlich. Der Abschnitt „typeProperties“ für ein Dataset vom Typ **AzureSearchIndex** hat die folgenden Eigenschaften:

| Eigenschaft | Beschreibung | Erforderlich |
| -------- | ----------- | -------- |
| Typ | Die Typeigenschaft muss auf **AzureSearchIndex** eingestellt sein.| Ja |
| IndexName | Name eines Azure Search-Index. Data Factory erstellt den Index nicht. Der Index muss in Azure Search vorhanden sein. | Ja |


## <a name="azure-search-index-sink-properties"></a>Azure Search-Index Senke-Eigenschaften
Eine vollständige Liste mit den Abschnitten und Eigenschaften zum Definieren von Aktivitäten finden Sie im Artikel [Erstellen von Pipelines](data-factory-create-pipelines.md) . Eigenschaften wie Name, Beschreibung, Eingabe- und Ausgabetabellen und zahlreiche Richtlinien sind für alle Arten von Aktivitäten verfügbar. Die Eigenschaften im Abschnitt „typeProperties“ der Aktivität können dagegen je nach Aktivitätstyp variieren. Für die Kopieraktivität variieren die Eigenschaften je nach Art der Quellen und Senken.

Wenn bei der Kopieraktivität die Quelle den Typ **AzureSearchIndexSink** hat, sind im Abschnitt „typeProperties“ die folgenden Eigenschaften verfügbar:

| Eigenschaft | Beschreibung | Zulässige Werte | Erforderlich |
| -------- | ----------- | -------------- | -------- |
| WriteBehavior | Gibt an, ob ein Dokument zusammengeführt oder ersetzt werden soll, wenn es bereits im Index vorhanden ist. Siehe [Eigenschaft „WriteBehavior“](#writebehavior-property).| Zusammenführen (Standard)<br/>Hochladen| Nein |
| writeBatchSize | Lädt Daten in den Azure Search-Index hoch,wenn die Puffergröße „writeBatchSize“ erreicht. Einzelheiten finden Sie unter [Eigenschaft „WriteBatchSize“](#writebatchsize-property). | 1 bis 1.000. Der Standardwert ist 1000. | Nein |

### <a name="writebehavior-property"></a>Eigenschaft „WriteBehavior“
AzureSearchSink fügt Daten ein/aktualisiert beim Schreiben von Daten. Dies bedeutet, dass Azure Search beim Schreiben eines Dokuments das bestehende Dokument aktualisiert, anstatt eine Konfliktausnahme auszulösen, wenn der Dokumentenschlüssel bereits im Azure Search-Index vorhanden ist.

AzureSearchSink bietet die folgenden zwei Verhalten zum Einfügen/Aktualisieren (mithilfe des Azure Search SDK):

- **Merge** (Zusammenführen): kombiniert alle Spalten im neuen Dokument mit dem bestehenden. Bei Spalten mit Null-Wert im neuen Dokument wird der Wert im bestehenden Dokument beibehalten.
- **Upload** (Hochladen): das neue Dokument ersetzt das bestehende. Bei Spalten, die nicht im neuen Dokument angegeben werden, wird der Wert auf Null gesetzt, unabhängig davon, ob ein Null-Wert im bestehenden Dokument vorhanden ist oder nicht.

Das Standardverhalten ist **Merge**.

### <a name="writebatchsize-property"></a>Eigenschaft „writeBatchSize“
Der Azure Search-Dienst unterstützt das Schreiben von Dokumenten als Batch. Ein Batch kann 1 bis 1.000 Aktionen enthalten. Eine Aktion bearbeitet ein Dokument, um den Vorgang upload/merge auszuführen.

### <a name="data-type-support"></a>Unterstützung von Datentypen
In der folgenden Tabelle wird angegeben, ob ein Azure Search-Datentyp unterstützt wird oder nicht.

| Azure Search-Datentyp | In Azure Search-Senke unterstützt |
| ---------------------- | ------------------------------ |
| String | J |
| Int32 | J |
| Int64 | J |
| Double | J |
| Boolean | J |
| DataTimeOffset | J |
| String Array | N |
| GeographyPoint | N |

## <a name="copy-from-a-cloud-source"></a>Kopieren aus einer Clouddatenquelle
Wenn Sie Daten aus einem Clouddatenspeicher in Azure Search kopieren, ist die `executionLocation`-Eigenschaft erforderlich. Im Folgenden werden als Beispiel die Änderung gezeigt, die unter der Kopieraktivität `typeProperties` erforderlich sind. Im Abschnitt [Kopieren von Daten zwischen Clouddatenspeichern](data-factory-data-movement-activities.md#global) finden Sie Informationen zu den unterstützten Werten sowie weitere Details.

```JSON
"typeProperties": {
  "source": {
    "type": "BlobSource"
  },
  "sink": {
    "type": "AzureSearchIndexSink"
  },
  "executionLocation": "West US"
}
```

## <a name="specifying-structure-definition-for-rectangular-datasets"></a>Angeben der Strukturdefinition für rechteckige Datasets
Der Abschnitt „structure“ in der JSON von Datasets ist ein **optionaler** Abschnitt für rechteckige Tabellen (mit Zeilen und Spalten) und enthält eine Auflistung der Spalten der Tabelle. Verwenden Sie den Abschnitt „structure“ entweder zum Angeben von Typinformationen für Typkonvertierungen oder für Spaltenzuordnungen. In den folgenden Abschnitten werden diese Features ausführlich beschrieben.

Jede Spalte enthält die folgenden Eigenschaften:

| Eigenschaft | Beschreibung | Erforderlich |
| -------- | ----------- | -------- |
| name | Name der Spalte. | Ja |
| type | Datentyp der Spalte. Weitere Informationen dazu, wann Sie Typinformationen angeben sollten, finden Sie im Bereich [Typkonvertierungen](#type-conversion-sample). | Nein |
| culture | Zu verwendendes .NET-basiertes Gebietsschema, wenn der Typ angegeben ist, und den .NET-Typ `Datetime` oder `Datetimeoffset` hat. Der Standardwert ist `en-us`.  | Nein |
| format | Zu verwendende Formatzeichenfolge, wenn der Typ angegeben ist und den .NET-Typ `Datetime` oder `Datetimeoffset` hat. | Nein |

Das folgende Beispiel zeigt den Abschnitt „structure“ der JSON für eine Tabelle mit den drei Spalten `userid`, `name` und `lastlogindate`.

```JSON
"structure":
[
    { "name": "userid"},
    { "name": "name"},
    { "name": "lastlogindate"}
],
```
Befolgen Sie die folgenden Angaben dazu, wann der Abschnitt **structure** mit welchen Informationen verwendet werden sollte.

- **Bei strukturierten Datenquellen**, die Datenschema- und Typinformationen neben den Daten selbst speichern (Beispiel: SQL Server, Oracle, Azure-Tabelle usw.), geben Sie den Abschnitt „structure“ nur an, wenn Sie bestimmte Quellspalten bestimmten Zielspalten in der Senke zuweisen, und deren Namen nicht identisch sind. Einzelheiten finden Sie in der nachfolgenden Abschnitt zur Spaltenzuordnung.

    Wie bereits erwähnt, ist die Typinformation im Abschnitt „structure“ optional. Für strukturierte Datenquellen steht die Typinformation als Teil der Datasetdefinition im Datenspeicher zur Verfügung, weshalb Sie Typinformationen nicht dem Abschnitt „structure“ hinzufügen müssen.
- **Für das Schema von Lesedatenquellen (insbesondere Azure-Blob)** können Sie Daten speichern, ohne Schema- oder Typinformationen mit den Daten zu speichern. Bei diesen Typen von Datenquellen sollten Sie den Abschnitt „structure“ in den beiden folgenden Fällen hinzufügen:
    - Beim Zuordnen von Quellspalten zu Spalten in der Senke.
    - Wenn das Dataset eine Quelle in einer Kopieraktivität ist, können Sie Typinformationen im Bereich „structure“ angeben. Data Factory verwendet Typinformationen für die Konvertierung in native Typen für die Senke. Weitere Informationen finden Sie im Artikel [Verschieben von Daten in einen und aus einem Azure-Blob](data-factory-azure-blob-connector.md).

### <a name="supported-net-based-types"></a>Unterstützte .NET-basierte Typen
Data Factory unterstützt die folgenden CLS-kompatiblen auf .NET basierenden Typwerte für die Bereitstellung von Typinformationen in „structure“ für das Schema von Lesedatenquellen wie Azure-Blob.

- Int16
- Int32
- Int64
- Single
- Double
- DECIMAL
- Bool
- String
- Datetime
- Datetimeoffset
- Timespan

Für „Datetime“ und „Datetimeoffset“ können Sie optional auch die Zeichenfolgen „culture“ & „format“ angeben, um das Analysieren Ihrer benutzerdefinierten „Datetime“-Zeichenfolge zu erleichtern. Im folgenden Abschnitt finden Sie Beispiele für die Typkonvertierung:


[!INCLUDE [data-factory-type-conversion-sample](../../includes/data-factory-type-conversion-sample.md)]

[!INCLUDE [data-factory-column-mapping](../../includes/data-factory-column-mapping.md)]


## <a name="performance-and-tuning"></a>Leistung und Optimierung  
Im [Handbuch zur Leistung und Optimierung der Kopieraktivität](data-factory-copy-activity-performance.md) werden wichtige Faktoren beschrieben, die sich auf die Leistung der Datenverschiebung (Kopieraktivität) auswirken, sowie verschiedene Möglichkeiten zur Leistungsoptimierung.

## <a name="next-steps"></a>Nächste Schritte
Entsprechende Informationen finden Sie in den folgenden Artikeln:

* [Kopieraktivität-Tutorial](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) für schrittweise Anleitungen zum Erstellen einer Pipeline mit einer Kopieraktivität.

