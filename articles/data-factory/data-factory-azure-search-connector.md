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
ms.sourcegitcommit: 356de369ec5409e8e6e51a286a20af70a9420193
ms.openlocfilehash: 94f0a741be663aae9f2ec4ee221a4b2b58e390e2
ms.lasthandoff: 03/27/2017

---

# <a name="push-data-to-an-azure-search-index-by-using-azure-data-factory"></a>Push-Übertragung von Daten in den Azure Search-Index mithilfe von Azure Data Factory
In diesem Artikel wird beschrieben, wie Sie die Kopieraktivität verwenden können, um Daten aus einem lokalen, unterstützten Quelldatenspeicher per Push in den Azure Search-Index zu übertragen. In der Spalte „Quelle“ der Tabelle [Unterstützte Datenquellen und Senken](data-factory-data-movement-activities.md#supported-data-stores-and-formats) sind unterstützte Quelldatenspeicher aufgelistet. Dieser Artikel baut auf dem Artikel [Datenverschiebungsaktivitäten](data-factory-data-movement-activities.md) auf, der eine allgemeine Übersicht zur Datenverschiebung mit Kopieraktivität und unterstützten Datenspeicherkombinationen bietet.

## <a name="enabling-connectivity"></a>Herstellen der Verbindung
Installieren Sie das Datenverwaltungsgateway in Ihrer lokalen Umgebung, um die Verbindung des Data Factory-Diensts mit einem lokalen Datenspeicher zuzulassen. Sie können das Gateway auf dem Computer, der den Quelldatenspeicher hostet oder auf einem separaten Computer installieren, um zu vermeiden, dass der Computer mit dem Datenspeicher um Ressourcen konkurriert.

Das Datenverwaltungsgateway verbindet die lokalen Datenquellen mit Cloud-Diensten auf sichere und verwaltete Weise. Weitere Informationen zum Datenverwaltungsgateway finden Sie im Artikel [Verschieben von Daten zwischen lokalen Quellen und der Cloud](data-factory-move-data-between-onprem-and-cloud.md) .

## <a name="getting-started"></a>Erste Schritte
Sie können eine Pipeline mit einer Kopieraktivität erstellen, die Daten mithilfe verschiedener Tools/APIs aus einem Quelldatenspeicher per Push in den Azure Search-Index überträgt.

Am einfachsten erstellen Sie eine Pipeline mit dem **Kopier-Assistenten**. Unter [Tutorial: Erstellen einer Pipeline mit dem Assistenten zum Kopieren](data-factory-copy-data-wizard-tutorial.md) finden Sie eine kurze exemplarische Vorgehensweise zum Erstellen einer Pipeline mithilfe des Assistenten zum Kopieren von Daten.

Sie können auch die folgenden Tools für das Erstellen einer Pipeline verwenden: **Azure-Portal**, **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager-Vorlagen**, **.NET-API** und **REST-API**. Im [Tutorial zur Kopieraktivität](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) finden Sie detaillierte Anweisungen, wie Sie eine Pipeline mit einer Kopieraktivität erstellen können. 

Unabhängig davon, ob Sie Tools oder APIs verwenden, führen Sie die folgenden Schritte aus, um eine Pipeline zu erstellen, die Daten aus einem Quelldatenspeicher in einen Senkendatenspeicher verschiebt: 

1. Erstellen **verknüpfter Dienste** zum Verknüpfen von Eingabe- und Ausgabedatenspeichern mit Ihrer Data Factory
2. Erstellen von **Datasets** zur Darstellung von Eingabe- und Ausgabedaten für den Kopiervorgang 
3. Erstellen einer **Pipeline** mit einer Kopieraktivität, die ein Dataset als Eingabe und ein Dataset als Ausgabe akzeptiert 

Wenn Sie den Assistenten verwenden, werden automatisch JSON-Definitionen für diese Data Factory-Entitäten (verknüpfte Diensten, Datasets und die Pipeline) erstellt. Bei Verwendung von Tools und APIs (mit Ausnahme der .NET-API) definieren Sie diese Data Factory-Entitäten im JSON-Format.  Ein Beispiel mit JSON-Definitionen für Data Factory-Entitäten, die zum Kopieren von Daten in den Azure Search-Index verwendet werden, finden Sie in diesem Artikel im Abschnitt [JSON-Beispiel: Kopieren von Daten aus einer lokalen SQL Server-Instanz in den Azure Search-Index](#json-example-copy-data-from-on-premises-sql-server-to-azure-search-index). 

Die folgenden Abschnitte enthalten Details zu JSON-Eigenschaften, die zum Definieren von Data Factory-Entitäten speziell für den Azure Search-Index verwendet werden:

## <a name="linked-service-properties"></a>Eigenschaften des verknüpften Diensts

Die folgende Tabelle enthält Beschreibungen der JSON-Elemente, die für den mit Azure Search verknüpften Dienst spezifisch sind.

| Eigenschaft | Beschreibung | Erforderlich |
| -------- | ----------- | -------- |
| Typ | Die Typeigenschaft muss auf **AzureSearch** festgelegt werden. | Ja |
| URL | URL für den Azure Search-Dienst. | Ja |
| key | Admin-Schlüssel für den Azure Search-Dienst. | Ja |

## <a name="dataset-properties"></a>Dataset-Eigenschaften

Eine vollständige Liste mit den Abschnitten und Eigenschaften, die zum Definieren von Datasets zur Verfügung stehen, finden Sie im Artikel [Erstellen von Datasets](data-factory-create-datasets.md) . Abschnitte wie „structure“, „availability“ und „policy“ des JSON-Codes eines Datasets sind bei allen Dataset-Typen ähnlich. Der Abschnitt **typeProperties** ist bei jeder Art von Dataset unterschiedlich. Der Abschnitt „typeProperties“ für ein Dataset vom Typ **AzureSearchIndex** hat die folgenden Eigenschaften:

| Eigenschaft | Beschreibung | Erforderlich |
| -------- | ----------- | -------- |
| Typ | Die Typeigenschaft muss auf **AzureSearchIndex** eingestellt sein.| Ja |
| IndexName | Name eines Azure Search-Index. Data Factory erstellt den Index nicht. Der Index muss in Azure Search vorhanden sein. | Ja |


## <a name="copy-activity-properties"></a>Eigenschaften der Kopieraktivität
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

## <a name="json-example-copy-data-from-on-premises-sql-server-to-azure-search-index"></a>JSON-Beispiel: Kopieren von Daten aus einer lokalen SQL Server-Instanz in den Azure Search-Index

Dieses Beispiel zeigt Folgendes:

1.    Einen verknüpften Dienst des Typs [AzureSearch](#linked-service-properties).
2.    Einen verknüpften Dienst des Typs [OnPremisesSqlServer](data-factory-sqlserver-connector.md#linked-service-properties)
3.    Ein [Eingabedataset](data-factory-create-datasets.md) des Typs [SqlServerTable](data-factory-sqlserver-connector.md#dataset-properties).
4.    Ein [Ausgabedataset](data-factory-create-datasets.md) des Typs [AzureSearchIndex](#dataset-properties).
4.    Eine [Pipeline](data-factory-create-pipelines.md) mit Kopieraktivität, die [SqlSource](data-factory-sqlserver-connector.md#copy-activity-properties) und [AzureSearchIndexSink](#copy-activity-properties) verwendet.

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

**Kopieraktivität in einer Pipeline mit SQL-Quelle und dem Azure Search-Index als Senke:**

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

Sie können auch Spalten aus dem Quelldataset in der Definition der Kopieraktivität Spalten im Senkendataset zuordnen. Weitere Informationen finden Sie unter [Zuordnen von Datasetspalten in Azure Data Factory](data-factory-map-columns.md).

## <a name="performance-and-tuning"></a>Leistung und Optimierung  
Im [Handbuch zur Leistung und Optimierung der Kopieraktivität](data-factory-copy-activity-performance.md) werden wichtige Faktoren beschrieben, die sich auf die Leistung der Datenverschiebung (Kopieraktivität) auswirken, sowie verschiedene Möglichkeiten zur Leistungsoptimierung.

## <a name="next-steps"></a>Nächste Schritte
Entsprechende Informationen finden Sie in den folgenden Artikeln:

* [Kopieraktivität-Tutorial](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) für schrittweise Anleitungen zum Erstellen einer Pipeline mit einer Kopieraktivität.

