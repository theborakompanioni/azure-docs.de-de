<properties 
	pageTitle="Verschieben von Daten aus Amazon Simple Storage Service mit Data Factory | Microsoft Azure" 
	description="Informationen über das Verschieben von Daten aus Amazon Simple Storage Service (S3) mit Azure Data Factory." 
	services="data-factory" 
	documentationCenter="" 
	authors="linda33wj" 
	manager="jhubbard" 
	editor="monicar"/>

<tags 
	ms.service="data-factory" 
	ms.workload="data-services" 
	ms.tgt_pltfrm="na" 
	ms.devlang="na" 
	ms.topic="article" 
	ms.date="08/25/2016" 
	ms.author="jingwang"/>

# Verschieben von Daten aus Amazon Simple Storage Service mit Azure Data Factory

Dieser Artikel beschreibt, wie Sie die Kopieraktivität in einer Azure Data Factory verwenden können, um Daten aus Amazon Simple Storage Service (S3) in einen anderen Datenspeicher zu verschieben. Dieser Artikel baut auf dem Artikel [Datenverschiebungsaktivitäten](data-factory-data-movement-activities.md) auf, der eine allgemeine Übersicht zur Datenverschiebung und eine Liste unterstützter Quellen-/Senkendatenspeichern mit Kopieraktivität bietet.

Data Factory unterstützt derzeit nur das Verschieben von Daten aus Amazon S3 in andere Datenspeicher und nicht das Verschieben aus anderen Datenspeichern in Amazon S3.

## Assistent zum Kopieren von Daten
Die einfachste Möglichkeit zum Erstellen einer Pipeline, die Daten aus Amazon S3 kopiert, ist die Verwendung des Assistenten zum Kopieren von Daten. Unter [Tutorial: Erstellen einer Pipeline mit dem Assistenten zum Kopieren](data-factory-copy-data-wizard-tutorial.md) finden Sie eine kurze exemplarische Vorgehensweise zum Erstellen einer Pipeline mithilfe des Assistenten zum Kopieren von Daten.

Das folgende Beispiel stellt JSON-Beispieldefinitionen bereit, die Sie zum Erstellen einer Pipeline mit dem [Azure-Portal](data-factory-copy-activity-tutorial-using-azure-portal.md), mit [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) oder mit [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md) verwenden können. Hier erfahren Sie, wie Sie Daten aus Amazon S3 in Azure-Blobspeicher kopieren. Allerdings können Daten in jede der [hier](data-factory-data-movement-activities.md#supported-data-stores) angegebenen Senken kopiert werden.

## Beispiel: Kopieren von Daten aus Amazon S3 in ein Azure-Blob
Dieses Beispiel zeigt, wie Sie Daten aus Amazon S3 in Azure-Blobspeicher kopieren. Daten können jedoch mithilfe der Kopieraktivität in Azure Data Factory **direkt** in die [hier](data-factory-data-movement-activities.md#supported-data-stores) aufgeführten Senken kopiert werden.
 
Das Beispiel enthält die folgenden Data Factory-Entitäten:

- Einen verknüpften Dienst des Typs [AwsAccessKey](#linked-service-properties).
- Einen verknüpften Dienst des Typs [AzureStorage](data-factory-azure-blob-connector.md#azure-storage-linked-service-properties)
- Ein [Eingabedataset](data-factory-create-datasets.md) des Typs [AmazonS3](#dataset-type-properties).
- Ein [Ausgabedataset](data-factory-create-datasets.md) des Typs [AzureBlob](data-factory-azure-blob-connector.md#azure-blob-dataset-type-properties)
- Eine [Pipeline](data-factory-create-pipelines.md) mit Kopieraktivität, die [FileSystemSource](#copy-activity-type-properties) und [BlobSink](data-factory-azure-blob-connector.md#azure-blob-copy-activity-type-properties) verwendet

Im Beispiel werden stündlich Daten aus Amazon S3 in ein Azure-Blob kopiert. Die bei diesen Beispielen verwendeten JSON-Eigenschaften werden in den Abschnitten beschrieben, die auf die Beispiele folgen.

**Mit Amazon S3 verknüpfter Dienst**

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

**Mit Azure Storage verknüpfter Dienst**

	{
	  "name": "AzureStorageLinkedService",
	  "properties": {
	    "type": "AzureStorage",
	    "typeProperties": {
	      "connectionString": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"
	    }
	  }
	}

**Amazon S3-Eingabedataset**

Mit der Festlegung **"external": true** wird dem Data Factory-Dienst mitgeteilt, dass das Dataset für die Data Factory extern ist und nicht durch eine Aktivität in der Data Factory erzeugt wird. Legen Sie diese Eigenschaft für ein Eingabedataset, das nicht durch eine Aktivität in der Pipeline erstellt wird, auf „true“ fest.

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



**Azure-Blob-Ausgabedataset**

Daten werden stündlich in ein neues Blob geschrieben ("frequency": "hour", "interval": 1). Der Ordnerpfad des Blobs wird basierend auf der Startzeit des Slices, der verarbeitet wird, dynamisch ausgewertet. Im Ordnerpfad werden Jahr, Monat, Tag und die Stundenteile der Startzeit verwendet.

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



**Pipeline mit Kopieraktivität**

Die Pipeline enthält eine Kopieraktivität, die für die Verwendung der Ein- und Ausgabedatasets und für eine stündliche Ausführung konfiguriert ist. In der JSON-Definition der Pipeline ist der Typ **source** auf **FileSystemSource** und der Typ **sink** auf **BlobSink** festgelegt.
	
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



## Eigenschaften des verknüpften Diensts

Die folgende Tabelle enthält eine Beschreibung der JSON-Elemente, die für den mit Amazon S3 (**AwsAccessKey**) verknüpften Dienst spezifisch sind.

| Eigenschaft | Beschreibung | Zulässige Werte | Erforderlich |
| -------- | ----------- | -------- | ------- |  
| accessKeyID | ID des geheimen Zugriffsschlüssels. | string | Ja |
| secretAccessKey | Der geheime Zugriffsschlüssel selbst. | Verschlüsselte geheime Zeichenfolge | Ja | 


## Eigenschaften des Datasettyps

Eine vollständige Liste der Abschnitte und Eigenschaften, die zum Definieren von Datasets zur Verfügung stehen, finden Sie im Artikel [Erstellen von Datasets](data-factory-create-datasets.md). Abschnitte wie „structure“, „availability“ und „policy“ sind bei allen Dataset-Typen (Azure SQL, Azure-Blob, Azure-Tabelle usw.) ähnlich.

Der Abschnitt **typeProperties** unterscheidet sich bei jedem Typ von Dataset und bietet Informationen zum Speicherort der Daten im Datenspeicher. Der Abschnitt typeProperties für ein Dataset vom Typ **AmazonS3** (mit Amazon S3-Dataset) hat die folgenden Eigenschaften:

| Eigenschaft | Beschreibung | Zulässige Werte | Erforderlich |
| -------- | ----------- | -------- | ------ | 
| bucketName | Der Name des S3-Buckets. | String | Ja |
| key | Der S3-Objektschlüssel. | String | Nein | 
| prefix | Präfix für den S3-Objektschlüssel. Objekte, deren Schlüssel mit diesem Präfix beginnen, werden ausgewählt. Gilt nur, wenn der Schlüssel leer ist. | String | Nein | 
| Version | Die Version des S3-Objekts, wenn S3-Versionierung aktiviert ist. | String | Nein |  
| format | Die folgenden Formattypen werden unterstützt: **TextFormat**, **AvroFormat**, **JsonFormat**, **OrcFormat**, **ParquetFormat**. Sie müssen die **type**-Eigenschaft unter „format“ auf einen dieser Werte festlegen. Weitere Informationen finden Sie in den Abschnitten [Angeben von TextFormat](#specifying-textformat), [Angeben von AvroFormat](#specifying-avroformat), [Angeben von JsonFormat](#specifying-jsonformat), [Angeben von OrcFormat](#specifying-orcformat) und [Angeben von ParquetFormat](#specifying-parquetformat). Wenn Sie Dateien unverändert zwischen dateibasierten Speichern kopieren möchten (binäre Kopie), können Sie den Formatabschnitt bei den Definitionen von Eingabe- und Ausgabedatasets überspringen.| Nein
| Komprimierung | Geben Sie den Typ und den Grad der Komprimierung für die Daten an. Folgende Typen werden unterstützt: **GZip**, **Deflate** und **BZip2**. Folgende Komprimierungsstufen werden unterstützt: **Optimal** und **Schnellste**. Beachten Sie, dass für Daten mit **AvroFormat** oder **OrcFormat** derzeit keine Komprimierungseinstellungen unterstützt werden. Weitere Informationen finden Sie im Abschnitt [Komprimierungsunterstützung](#compression-support). | Nein |

> [AZURE.NOTE] „bucketName + key“ gibt den Speicherort des S3-Objekts an, wobei bucketName der Name des Stammcontainers für S3-Objekte und „key“ der vollständige Pfad zum S3-Objekt ist.

### Beispieldataset mit Präfix

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

### Beispieldataset (mit Version)

	{
	    "name": "dataset-s3",
	    "properties": {
	        "type": "AmazonS3",
	        "linkedServiceName": "link- testS3",
	        "typeProperties": {
	            "key": "testFolder/test.orc",
	            "bucketName": "testbucket",
	            "version": "WBeMIxQkJczm0CJajYkHf0_k6LhBmkcL",
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


### Dynamische Pfade für S3

In diesem Beispiel verwenden wir feste Werte für die Eigenschaften „key“ und bucketName im Amazon S3-Dataset.

	"key": "testFolder/test.orc",
	"bucketName": "testbucket",

Mithilfe von Systemvariablen wie SliceStart können Sie „key“ und bucketName dynamisch zur Laufzeit durch Data Factory berechnen lassen.

	"key": "$$Text.Format('{0:MM}/{0:dd}/test.orc', SliceStart)"
	"bucketName": "$$Text.Format('{0:yyyy}', SliceStart)"

Dies können Sie auch für die prefix-Eigenschaft eines Amazon S3-Datasets vornehmen. Unter [Data Factory – Funktionen und Systemvariablen](data-factory-functions-variables.md) finden Sie eine Liste unterstützter Funktionen und Variablen.


[AZURE.INCLUDE [data-factory-file-format](../../includes/data-factory-file-format.md)]
[AZURE.INCLUDE [data-factory-compression](../../includes/data-factory-compression.md)]


## Eigenschaften des Kopieraktivitätstyps

Eine vollständige Liste der Abschnitte und Eigenschaften zum Definieren von Aktivitäten finden Sie im Artikel [Erstellen von Pipelines](data-factory-create-pipelines.md). Eigenschaften wie Name, Beschreibung, Eingabe- und Ausgabetabellen und Richtlinien sind für alle Arten von Aktivitäten verfügbar.

Eigenschaften im Abschnitt **typeProperties** der Aktivität können dagegen je nach Aktivitätstyp variieren. Für die Kopieraktivität variieren die Eigenschaften je nach Art der Quellen und Senken.

Wenn die Quelle in Kopieraktivität vom Typ **FileSystemSource** ist (zu dem Amazon S3 gehört), sind im Abschnitt typeProperties die folgenden Eigenschaften verfügbar:

| Eigenschaft | Beschreibung | Zulässige Werte | Erforderlich |
| -------- | ----------- | -------------- | -------- | 
| recursive | Gibt an, ob S3-Objekte rekursiv im Verzeichnis aufgelistet werden. | True/False | Nein | 

[AZURE.INCLUDE [data-factory-structure-for-rectangular-datasets](../../includes/data-factory-structure-for-rectangualr-datasets.md)]

[AZURE.INCLUDE [data-factory-column-mapping](../../includes/data-factory-column-mapping.md)]

[AZURE.INCLUDE [data-factory-type-repeatability-for-relational-sources](../../includes/data-factory-type-repeatability-for-relational-sources.md)]

## Leistung und Optimierung  
Im Artikel [Handbuch zur Leistung und Optimierung der Kopieraktivität](data-factory-copy-activity-performance.md) werden wichtige Faktoren beschrieben, die sich auf die Leistung der Datenverschiebung (Kopieraktivität) in Azure Data Factory auswirken, sowie verschiedene Möglichkeiten zur Leistungsoptimierung.

## Nächste Schritte
Entsprechende Informationen finden Sie in den folgenden Artikeln:
- [Kopieraktivität-Tutorial](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) für schrittweise Anleitungen zum Erstellen einer Pipeline mit einer Kopieraktivität.

<!---HONumber=AcomDC_0928_2016-->