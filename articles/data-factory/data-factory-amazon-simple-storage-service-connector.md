---
title: Verschieben von Daten aus Amazon Simple Storage Service mit Data Factory | Microsoft Docs
description: "Informationen über das Verschieben von Daten aus Amazon Simple Storage Service (S3) mit Azure Data Factory."
services: data-factory
documentationcenter: 
author: linda33wj
manager: jhubbard
editor: monicar
ms.assetid: 636d3179-eba8-4841-bcb4-3563f6822a26
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/22/2017
ms.author: jingwang
translationtype: Human Translation
ms.sourcegitcommit: b4802009a8512cb4dcb49602545c7a31969e0a25
ms.openlocfilehash: 23d0092915c98da54ed486aed22afba4839befd1
ms.lasthandoff: 03/29/2017


---
# <a name="move-data-from-amazon-simple-storage-service-using-azure-data-factory"></a>Verschieben von Daten aus Amazon Simple Storage Service mit Azure Data Factory
Dieser Artikel beschreibt, wie Sie die Kopieraktivität in Azure Data Factory verwenden, um Daten aus Amazon Simple Storage Service (S3) zu verschieben. Dieser Artikel baut auf dem Artikel zu [Datenverschiebungsaktivitäten](data-factory-data-movement-activities.md) auf, der eine allgemeine Übersicht zur Datenverschiebung mit der Kopieraktivität bietet. 

Sie können Daten von Amazon Simple Storage Service (S3) in beliebige unterstützte Senkendatenspeicher kopieren. Eine Liste der Datenspeicher, die als Senken für die Kopieraktivität unterstützt werden, finden Sie in der Tabelle [Unterstützte Datenspeicher](data-factory-data-movement-activities.md#supported-data-stores-and-formats). Data Factory unterstützt derzeit nur das Verschieben von Daten aus Amazon S3 in andere Datenspeicher und nicht das Verschieben aus anderen Datenspeichern in Amazon S3.

## <a name="required-permissions"></a>Erforderliche Berechtigungen
Um Daten von Amazon S3 zu kopieren, müssen Sie sicherstellen, dass Ihnen die folgenden Berechtigungen erteilt wurden:

* `s3:GetObject` und `s3:GetObjectVersion` für Amazon S3-Objektvorgänge
* `s3:ListBucket` für Amazon S3-Bucketvorgänge. Wenn Sie den Kopier-Assistenten verwenden, ist außerdem `s3:ListAllMyBuckets` erforderlich.

Sie finden die vollständige Liste der Amazon S3-Berechtigungen mit entsprechenden Details unter [Specifying Permissions in a Policy](http://docs.aws.amazon.com/AmazonS3/latest/dev/using-with-s3-actions.html) (Angeben von Berechtigungen in einer Richtlinie).

## <a name="getting-started"></a>Erste Schritte
Sie können eine Pipeline mit einer Kopieraktivität erstellen, die Daten mithilfe verschiedener Tools/APIs aus einer Amazon S3-Quelle verschiebt.

Am einfachsten erstellen Sie eine Pipeline mit dem **Kopier-Assistenten**. Unter [Tutorial: Erstellen einer Pipeline mit dem Assistenten zum Kopieren](data-factory-copy-data-wizard-tutorial.md) finden Sie eine kurze exemplarische Vorgehensweise zum Erstellen einer Pipeline mithilfe des Assistenten zum Kopieren von Daten.

Sie können auch die folgenden Tools für das Erstellen einer Pipeline verwenden: **Azure-Portal**, **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager-Vorlagen**, **.NET-API** und **REST-API**. Im [Tutorial zur Kopieraktivität](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) finden Sie detaillierte Anweisungen, wie Sie eine Pipeline mit einer Kopieraktivität erstellen können. 

Unabhängig davon, ob Sie Tools oder APIs verwenden, führen Sie die folgenden Schritte aus, um eine Pipeline zu erstellen, die Daten aus einem Quelldatenspeicher in einen Senkendatenspeicher verschiebt: 

1. Erstellen Sie **verknüpfte Dienste** zum Verknüpfen von Eingabe- und Ausgabedatenspeichern mit Ihrer Data Factory.
2. Erstellen Sie **Datasets** zur Darstellung von Eingabe- und Ausgabedaten für den Kopiervorgang. 
3. Erstellen Sie eine **Pipeline** mit einer Kopieraktivität, die ein Dataset als Eingabe und ein Dataset als Ausgabe akzeptiert. 

Wenn Sie den Assistenten verwenden, werden automatisch JSON-Definitionen für diese Data Factory-Entitäten (verknüpfte Dienste, Datasets und die Pipeline) erstellt. Bei Verwendung von Tools und APIs (mit Ausnahme der .NET-API) definieren Sie diese Data Factory-Entitäten im JSON-Format.  Ein Beispiel mit JSON-Definitionen für Data Factory-Entitäten, die zum Kopieren von Daten aus einem Amazon S3-Datenspeicher verwendet werden, finden Sie in diesem Artikel im Abschnitt [JSON-Beispiel: Kopieren von Daten aus Amazon S3 in ein Azure-Blob](#json-example-copy-data-from-amazon-s3-to-azure-blob). 

Die folgenden Abschnitte enthalten Details zu JSON-Eigenschaften, die zum Definieren von Data Factory-Entitäten speziell für Amazon S3 verwendet werden: 

## <a name="linked-service-properties"></a>Eigenschaften des verknüpften Diensts
Ein verknüpfter Dienst verbindet einen Data Store mit einer Data Factory. Sie erstellen einen verknüpften Dienst vom Typ **AwsAccessKey**, um Ihren Amazon S3-Datenspeicher mit ihrer Data Factory zu verbinden. Die folgende Tabelle enthält eine Beschreibung der JSON-Elemente, die für den mit Amazon S3 (AwsAccessKey) verknüpften Dienst spezifisch sind.

| Eigenschaft | Beschreibung | Zulässige Werte | Erforderlich |
| --- | --- | --- | --- |
| accessKeyID |ID des geheimen Zugriffsschlüssels. |string |Ja |
| secretAccessKey |Der geheime Zugriffsschlüssel selbst. |Verschlüsselte geheime Zeichenfolge |Ja |

Beispiel: 

```json
{
    "name": "AmazonS3LinkedService",
    "properties": {
        "type": "AwsAccessKey",
        "typeProperties": {
            "accessKeyId": "<access key id>",
            "secretAccessKey": "<secret access key>"
        }
    }
}
```

## <a name="dataset-properties"></a>Dataset-Eigenschaften
Um ein Dataset zur Darstellung von Eingabedaten in einem Azure Blob Storage anzugeben, legen Sie die Typeigenschaft des Datasets auf **AmazonS3** fest. Legen Sie die **linkedServiceName**-Eigenschaft des Datasets auf den Namen des mit Amazon S3 verknüpften Diensts fest.  Eine vollständige Liste der Abschnitte und Eigenschaften, die zum Definieren von Datasets zur Verfügung stehen, finden Sie im Artikel [Erstellen von Datasets](data-factory-create-datasets.md). Abschnitte wie „structure“, „availability“ und „policy“ sind bei allen Dataset-Typen (Azure SQL, Azure-Blob, Azure-Tabelle usw.) ähnlich. Der Abschnitt **typeProperties** unterscheidet sich bei jedem Typ von Dataset und bietet Informationen zum Speicherort der Daten im Datenspeicher. Der Abschnitt typeProperties für ein Dataset vom Typ **AmazonS3** (mit Amazon S3-Dataset) hat die folgenden Eigenschaften:

| Eigenschaft | Beschreibung | Zulässige Werte | Erforderlich |
| --- | --- | --- | --- |
| bucketName |Der Name des S3-Buckets. |string |Ja |
| key |Der S3-Objektschlüssel. |string |Nein |
| prefix |Präfix für den S3-Objektschlüssel. Objekte, deren Schlüssel mit diesem Präfix beginnen, werden ausgewählt. Gilt nur, wenn der Schlüssel leer ist. |string |Nein |
| Version |Die Version des S3-Objekts, wenn S3-Versionierung aktiviert ist. |string |Nein |
| format | Die folgenden Formattypen werden unterstützt: **TextFormat**, **JsonFormat**, **AvroFormat**, **OrcFormat**, **ParquetFormat**. Sie müssen die **type** -Eigenschaft unter „format“ auf einen dieser Werte festlegen. Weitere Informationen finden Sie in den Abschnitten [Textformat](data-factory-supported-file-and-compression-formats.md#text-format), [JSON-Format](data-factory-supported-file-and-compression-formats.md#json-format), [Avro-Format](data-factory-supported-file-and-compression-formats.md#avro-format), [Orc-Format](data-factory-supported-file-and-compression-formats.md#orc-format) und [Parquet-Format](data-factory-supported-file-and-compression-formats.md#parquet-format). <br><br> Wenn Sie **Dateien unverändert zwischen dateibasierten Speichern kopieren** möchten (binäre Kopie), können Sie den Formatabschnitt bei den Definitionen von Eingabe- und Ausgabedatasets überspringen. |Nein | |
| Komprimierung | Geben Sie den Typ und den Grad der Komprimierung für die Daten an. Unterstützte Typen sind: **Gzip**, **Deflate**, **bzip2** und **ZipDeflate**. Die unterstützten Grade sind: **Optimal** und **Schnellste**. Weitere Informationen finden Sie unter [Datei- und Komprimierungsformate in Azure Data Factory](data-factory-supported-file-and-compression-formats.md#compression-support). |Nein | |


> [!NOTE]
> „bucketName + key“ gibt den Speicherort des S3-Objekts an, wobei bucketName der Name des Stammcontainers für S3-Objekte und „key“ der vollständige Pfad zum S3-Objekt ist.

### <a name="sample-dataset-with-prefix"></a>Beispieldataset mit Präfix

```json
{
    "name": "dataset-s3",
    "properties": {
        "type": "AmazonS3",
        "linkedServiceName": "link- testS3",
        "typeProperties": {
            "prefix": "testFolder/test",
            "bucketName": "testbucket",
            "format": {
                "type": "OrcFormat"
            }
        },
        "availability": {
            "frequency": "Hour",
            "interval": 1
        },
        "external": true
    }
}
```
### <a name="sample-data-set-with-version"></a>Beispieldataset (mit Version)

```json
{
    "name": "dataset-s3",
    "properties": {
        "type": "AmazonS3",
        "linkedServiceName": "link- testS3",
        "typeProperties": {
            "key": "testFolder/test.orc",
            "bucketName": "testbucket",
            "version": "XXXXXXXXXczm0CJajYkHf0_k6LhBmkcL",
            "format": {
                "type": "OrcFormat"
            }
        },
        "availability": {
            "frequency": "Hour",
            "interval": 1
        },
        "external": true
    }
}
```

### <a name="dynamic-paths-for-s3"></a>Dynamische Pfade für S3
In diesem Beispiel verwenden wir feste Werte für die Eigenschaften „key“ und bucketName im Amazon S3-Dataset.

```json
"key": "testFolder/test.orc",
"bucketName": "testbucket",
```

Mithilfe von Systemvariablen wie SliceStart können Sie „key“ und bucketName dynamisch zur Laufzeit durch Data Factory berechnen lassen.

```json
"key": "$$Text.Format('{0:MM}/{0:dd}/test.orc', SliceStart)"
"bucketName": "$$Text.Format('{0:yyyy}', SliceStart)"
```

Dies können Sie auch für die prefix-Eigenschaft eines Amazon S3-Datasets vornehmen. Unter [Data Factory – Funktionen und Systemvariablen](data-factory-functions-variables.md) finden Sie eine Liste unterstützter Funktionen und Variablen.

## <a name="copy-activity-properties"></a>Eigenschaften der Kopieraktivität
Eine vollständige Liste der Abschnitte und Eigenschaften zum Definieren von Aktivitäten finden Sie im Artikel [Erstellen von Pipelines](data-factory-create-pipelines.md). Eigenschaften wie Name, Beschreibung, Eingabe- und Ausgabetabellen und Richtlinien sind für alle Arten von Aktivitäten verfügbar. Eigenschaften im Abschnitt **typeProperties** der Aktivität können dagegen je nach Aktivitätstyp variieren. Für die Kopieraktivität variieren die Eigenschaften je nach Art der Quellen und Senken. Wenn die Quelle in Kopieraktivität vom Typ **FileSystemSource** ist (zu dem Amazon S3 gehört), sind im Abschnitt typeProperties die folgenden Eigenschaften verfügbar:

| Eigenschaft | Beschreibung | Zulässige Werte | Erforderlich |
| --- | --- | --- | --- |
| recursive |Gibt an, ob S3-Objekte rekursiv im Verzeichnis aufgelistet werden. |True/False |Nein |


## <a name="json-example-copy-data-from-amazon-s3-to-azure-blob"></a>JSON-Beispiel: Kopieren von Daten aus Amazon S3 in ein Azure-Blob
Dieses Beispiel zeigt, wie Sie Daten aus Amazon S3 in Azure-Blobspeicher kopieren. Daten können jedoch mithilfe der Kopieraktivität in Azure Data Factory **direkt** in die [hier](data-factory-data-movement-activities.md#supported-data-stores-and-formats) aufgeführten Senken kopiert werden. 

Das Beispiel enthält JSON-Definitionen für die folgenden Data Factory-Entitäten. Sie können diese Definitionen zur Erstellung einer Pipeline zum Kopieren von Daten aus Amazon S3 in einen Azure Blob Storage mithilfe des [Azure-Portals](data-factory-copy-activity-tutorial-using-azure-portal.md) oder von [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) oder [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md) verwenden.   

* Einen verknüpften Dienst des Typs [AwsAccessKey](#linked-service-properties).
* Einen verknüpften Dienst des Typs [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties)
* Ein [Eingabedataset](data-factory-create-datasets.md) des Typs [AmazonS3](#dataset-properties)
* Ein [Ausgabedataset](data-factory-create-datasets.md) des Typs [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties)
* Eine [Pipeline](data-factory-create-pipelines.md) mit Kopieraktivität, die [FileSystemSource](#copy-activity-properties) und [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties) verwendet

Im Beispiel werden stündlich Daten aus Amazon S3 in ein Azure-Blob kopiert. Die bei diesen Beispielen verwendeten JSON-Eigenschaften werden in den Abschnitten beschrieben, die auf die Beispiele folgen.

**Mit Amazon S3 verknüpfter Dienst:**

```json
{
    "name": "AmazonS3LinkedService",
    "properties": {
        "type": "AwsAccessKey",
        "typeProperties": {
            "accessKeyId": "<access key id>",
            "secretAccessKey": "<secret access key>"
        }
    }
}
```

**Mit Azure Storage verknüpfter Dienst:**

```json
{
  "name": "AzureStorageLinkedService",
  "properties": {
    "type": "AzureStorage",
    "typeProperties": {
      "connectionString": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"
    }
  }
}
```

**Amazon S3-Eingabedataset:**

Mit der Festlegung **"external": true** wird dem Data Factory-Dienst mitgeteilt, dass das Dataset für die Data Factory extern ist und nicht durch eine Aktivität in der Data Factory erzeugt wird. Legen Sie diese Eigenschaft für ein Eingabedataset, das nicht durch eine Aktivität in der Pipeline erstellt wird, auf „true“ fest.

```json
    {
        "name": "AmazonS3InputDataset",
        "properties": {
            "type": "AmazonS3",
            "linkedServiceName": "AmazonS3LinkedService",
            "typeProperties": {
                "key": "testFolder/test.orc",
                "bucketName": "testbucket",
                "format": {
                    "type": "OrcFormat"
                }
            },
            "availability": {
                "frequency": "Hour",
                "interval": 1
            },
            "external": true
        }
    }
```


**Azure-Blob-Ausgabedataset:**

Daten werden stündlich in ein neues Blob geschrieben ("frequency": "hour", "interval": 1). Der Ordnerpfad des Blobs wird basierend auf der Startzeit des Slices, der verarbeitet wird, dynamisch ausgewertet. Im Ordnerpfad werden Jahr, Monat, Tag und die Stundenteile der Startzeit verwendet.

```json
{
    "name": "AzureBlobOutputDataSet",
    "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "AzureStorageLinkedService",
        "typeProperties": {
            "folderPath": "mycontainer/fromamazons3/yearno={Year}/monthno={Month}/dayno={Day}/hourno={Hour}",
            "format": {
                "type": "TextFormat",
                "rowDelimiter": "\n",
                "columnDelimiter": "\t"
            },
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
            ]
        },
        "availability": {
            "frequency": "Hour",
            "interval": 1
        }
    }
}
```


**Kopieraktivität in einer Pipeline mit einer Amazon S3-Quelle (Dateisystemquelle) und einer Blobsenke:**

Die Pipeline enthält eine Kopieraktivität, die für die Verwendung der Ein- und Ausgabedatasets und für eine stündliche Ausführung konfiguriert ist. In der JSON-Definition der Pipeline ist der Typ **source** auf **FileSystemSource** und der Typ **sink** auf **BlobSink** festgelegt.

```json
{
    "name": "CopyAmazonS3ToBlob",
    "properties": {
        "description": "pipeline for copy activity",
        "activities": [
            {
                "type": "Copy",
                "typeProperties": {
                    "source": {
                        "type": "FileSystemSource",
                        "recursive": true
                    },
                    "sink": {
                        "type": "BlobSink",
                        "writeBatchSize": 0,
                        "writeBatchTimeout": "00:00:00"
                    }
                },
                "inputs": [
                    {
                        "name": "AmazonS3InputDataset"
                    }
                ],
                "outputs": [
                    {
                        "name": "AzureBlobOutputDataSet"
                    }
                ],
                "policy": {
                    "timeout": "01:00:00",
                    "concurrency": 1
                },
                "scheduler": {
                    "frequency": "Hour",
                    "interval": 1
                },
                "name": "AmazonS3ToBlob"
            }
        ],
        "start": "2014-08-08T18:00:00Z",
        "end": "2014-08-08T19:00:00Z"
    }
}
```
> [!NOTE]
> Weitere Informationen zum Zuordnen von Spalten im Quelldataset zu Spalten im Senkendataset finden Sie unter [Zuordnen von Datasetspalten in Azure Data Factory](data-factory-map-columns.md).


## <a name="performance-and-tuning"></a>Leistung und Optimierung
Der Artikel [Handbuch zur Leistung und Optimierung der Kopieraktivität](data-factory-copy-activity-performance.md) beschreibt wichtige Faktoren, die sich auf die Leistung der Datenverschiebung (Kopieraktivität) in Azure Data Factory auswirken, sowie verschiedene Möglichkeiten zur Leistungsoptimierung.

## <a name="next-steps"></a>Nächste Schritte
Entsprechende Informationen finden Sie in den folgenden Artikeln:

* [Kopieraktivität-Tutorial](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) für schrittweise Anleitungen zum Erstellen einer Pipeline mit einer Kopieraktivität.

