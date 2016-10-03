<properties 
	pageTitle="Verschieben von Daten in/aus Azure-Tabellen | Microsoft Azure" 
	description="Erfahren Sie, wie Daten mithilfe von Azure Data Factory in einen und aus einem Azure-Tabellenspeicher verschoben werden." 
	services="data-factory" 
	documentationCenter="" 
	authors="spelluru" 
	manager="jhubbard" 
	editor="monicar"/>

<tags 
	ms.service="data-factory" 
	ms.workload="data-services" 
	ms.tgt_pltfrm="na" 
	ms.devlang="na" 
	ms.topic="article" 
	ms.date="09/13/2016" 
	ms.author="spelluru"/>

# Verschieben von Daten in eine und aus einer Azure-Tabelle mithilfe von Azure Data Factory

Dieser Artikel beschreibt, wie Sie die Kopieraktivität in einer Azure Data Factory verwenden können, um Daten aus der Azure-Tabelle in einen anderen Datenspeicher zu verschieben oder umgekehrt. Dieser Artikel baut auf dem Artikel [Datenverschiebungsaktivitäten](data-factory-data-movement-activities.md) auf, der eine allgemeine Übersicht zur Datenverschiebung und unterstützten Datenspeicherkombinationen mit Kopieraktivität bietet.

## Assistent zum Kopieren von Daten
Die einfachste Möglichkeit zum Erstellen einer Pipeline, die Daten in und aus Azure Table Storage kopiert, ist die Verwendung des Assistenten zum Kopieren von Daten. Unter [Tutorial: Erstellen einer Pipeline mit dem Assistenten zum Kopieren](data-factory-copy-data-wizard-tutorial.md) finden Sie eine kurze exemplarische Vorgehensweise zum Erstellen einer Pipeline mithilfe des Assistenten zum Kopieren von Daten.

Die folgenden Beispiele zeigen JSON-Beispieldefinitionen, die Sie zum Erstellen einer Pipeline mit dem [Azure-Portal](data-factory-copy-activity-tutorial-using-azure-portal.md), mit [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) oder [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md) verwenden können. Sie zeigen Ihnen das Kopieren von Daten aus Azure Table Storge und der Azure-Blobdatenbank (und umgekehrt). Daten können jedoch mithilfe der Kopieraktivität in Azure Data Factory **direkt** aus beliebigen Quellen in die [hier](data-factory-data-movement-activities.md#supported-data-stores) aufgeführten Senken kopiert werden.

## Beispiel: Kopieren von Daten aus einer Azure-Tabelle in ein Azure-Blob

Dieses Beispiel zeigt Folgendes:

1.	Einen verknüpften Dienst des Typs [AzureStorage](data-factory-azure-blob-connector.md#azure-storage-linked-service-properties) (wird für Tabelle und Blob verwendet)
2.	Ein [Eingabedataset](data-factory-create-datasets.md) des Typs [AzureTable](#azure-table-dataset-type-properties)
3.	Ein [Ausgabedataset](data-factory-create-datasets.md) des Typs [AzureBlob](data-factory-azure-blob-connector.md#azure-blob-dataset-type-properties)
3.	Die [Pipeline](data-factory-create-pipelines.md) mit Kopieraktivität, die [AzureTableSource](#azure-table-copy-activity-type-properties) und [BlobSink](data-factory-azure-blob-connector.md#azure-blob-copy-activity-type-properties) verwendet

Im Beispiel werden Daten, die zur Standardpartition in einer Azure-Tabelle gehören, stündlich in ein Blob kopiert. Die bei diesen Beispielen verwendeten JSON-Eigenschaften werden in den Abschnitten beschrieben, die auf die Beispiele folgen.

**Mit Azure Storage verknüpfter Dienst:**

	{
	  "name": "StorageLinkedService",
	  "properties": {
	    "type": "AzureStorage",
	    "typeProperties": {
	      "connectionString": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"
	    }
	  }
	}

Azure Data Factory unterstützt zwei Arten von mit Azure Storage verknüpften Diensten: **AzureStorage** und **AzureStorageSas**. Für den ersten geben Sie die Verbindungszeichenfolge an, die den Kontoschlüssel enthält. Für den zweiten geben Sie den SAS-URI (Shared Access Signature) an. Weitere Informationen finden Sie unter [Verknüpfte Dienste](#linked-services).

**Azure-Tabellen-Eingabedataset:**

Im Beispiel wird davon ausgegangen, dass Sie eine Tabelle "MyTable" in einer Azure-Tabelle erstellt haben.
 
Durch Festlegen von „external“ auf „true“ wird dem Data Factory-Dienst mitgeteilt, dass das Dataset für die Data Factory extern ist und nicht durch eine Aktivität in der Data Factory erzeugt wird.

	{
	  "name": "AzureTableInput",
	  "properties": {
	    "type": "AzureTable",
	    "linkedServiceName": "StorageLinkedService",
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

**Azure-Blob-Ausgabedataset:**

Daten werden stündlich in ein neues Blob geschrieben ("frequency": "hour", "interval": 1). Der Ordnerpfad des Blobs wird basierend auf der Startzeit des Slices, der verarbeitet wird, dynamisch ausgewertet. Im Ordnerpfad werden Jahr, Monat, Tag und die Stundenteile der Startzeit verwendet.

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

**Pipeline mit Kopieraktivität:**

Die Pipeline enthält eine Kopieraktivität, die für die Verwendung der Ein- und Ausgabedatasets und für eine stündliche Ausführung konfiguriert ist. In der JSON-Definition der Pipeline ist der Typ **source** auf **AzureTableSource** und der Typ **sink** auf **BlobSink** festgelegt. Die mit der Eigenschaft **AzureTableSourceQuery** angegebene SQL-Abfrage wählt stündlich aus der Standardpartition die zu kopierenden Daten aus.

	{  
	    "name":"SamplePipeline",
	    "properties":{  
	    	"start":"2014-06-01T18:00:00",
		    "end":"2014-06-01T19:00:00",
	    	"description":"pipeline for copy activity",
	    	"activities":[  
				{
	        		"name": "AzureTabletoBlob",
	        		"description": "copy activity",
	        		"type": "Copy",
	        		"inputs": [
	          			{
		            		"name": "AzureTableInput"
		        		}
	        		],
	        		"outputs": [
	          			{
	          	  			"name": "AzureBlobOutput"
		          		}
		        	],
		        	"typeProperties": {
		          		"source": {
		            		"type": "AzureTableSource",
				            "AzureTableSourceQuery": "PartitionKey eq 'DefaultPartitionKey'"
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

## Beispiel: Kopieren von Daten aus einem Azure-Blob in eine Azure-Tabelle

Dieses Beispiel zeigt Folgendes:

1.	Einen verknüpften Dienst des Typs [AzureStorage](data-factory-azure-blob-connector.md#azure-storage-linked-service-properties) (wird für Tabelle und Blob verwendet)
3.	Ein [Eingabedataset](data-factory-create-datasets.md) des Typs [AzureBlob](data-factory-azure-blob-connector.md#azure-blob-dataset-type-properties)
4.	Ein [Ausgabedataset](data-factory-create-datasets.md) des Typs [AzureTable](#azure-table-dataset-type-properties)
4.	Die [Pipeline](data-factory-create-pipelines.md) mit Kopieraktivität, die [BlobSource](data-factory-azure-blob-connector.md#azure-blob-copy-activity-type-properties) und [AzureTableSink](#azure-table-copy-activity-type-properties) verwendet


Im Beispiel werden Zeitreihendaten aus einem Azure-Blob stündlich in eine Azure-Tabelle kopiert. Die bei diesen Beispielen verwendeten JSON-Eigenschaften werden in den Abschnitten beschrieben, die auf die Beispiele folgen.

**Mit Azure Storage (für Azure-Tabelle und -Blob) verknüpfter Dienst:**

	{
	  "name": "StorageLinkedService",
	  "properties": {
	    "type": "AzureStorage",
	    "typeProperties": {
	      "connectionString": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"
	    }
	  }
	}

Azure Data Factory unterstützt zwei Arten von mit Azure Storage verknüpften Diensten: **AzureStorage** und **AzureStorageSas**. Für den ersten geben Sie die Verbindungszeichenfolge an, die den Kontoschlüssel enthält. Für den zweiten geben Sie den SAS-URI (Shared Access Signature) an. Weitere Informationen finden Sie unter [Verknüpfte Dienste](#linked-services).

**Azure-Blob-Eingabedataset:**

Daten werden stündlich aus einem neuen Blob übernommen ("frequency": "hour", "interval": 1). Ordnerpfad und Dateiname des Blobs werden basierend auf der Startzeit des Slices, der verarbeitet wird, dynamisch ausgewertet. Der Ordnerpfad verwendet die Bestandteile Jahr, Monat und Tag der Startzeit, und der Dateiname verwendet die Stunde der Startzeit. Die Festlegung von „external“ auf „true“ teilt dem Data Factory-Dienst mit, dass dieses Dataset für die Data Factory extern ist und nicht durch eine Aktivität in der Data Factory erzeugt wird.
	
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

**Azure-Tabellen-Ausgabedataset:**

Das Beispiel kopiert Daten in eine Tabelle namens "MyTable" in einer Azure-Tabelle. Erstellen Sie eine Azure-Tabelle mit der gleichen Anzahl von Spalten, die Sie in der Blob-CSV-Datei erwarten. Neue Zeilen werden der Tabelle stündlich hinzugefügt.

	{
	  "name": "AzureTableOutput",
	  "properties": {
	    "type": "AzureTable",
	    "linkedServiceName": "StorageLinkedService",
	    "typeProperties": {
	      "tableName": "MyOutputTable"
	    },
	    "availability": {
	      "frequency": "Hour",
	      "interval": 1
	    }
	  }
	}

**Pipeline mit Kopieraktivität:**

Die Pipeline enthält eine Kopieraktivität, die für die Verwendung der Ein- und Ausgabedatasets und für eine stündliche Ausführung konfiguriert ist. In der JSON-Definition der Pipeline ist der Typ **source** auf **BlobSource** und der Typ **sink** auf **AzureTableSink** festgelegt.


	{  
	    "name":"SamplePipeline",
	    "properties":{  
	    "start":"2014-06-01T18:00:00",
	    "end":"2014-06-01T19:00:00",
	    "description":"pipeline with copy activity",
	    "activities":[  
	      {
	        "name": "AzureBlobtoTable",
	        "description": "Copy Activity",
	        "type": "Copy",
	        "inputs": [
	          {
	            "name": "AzureBlobInput"
	          }
	        ],
	        "outputs": [
	          {
	            "name": "AzureTableOutput"
	          }
	        ],
	        "typeProperties": {
	          "source": {
	            "type": "BlobSource"
	          },
	          "sink": {
	            "type": "AzureTableSink",
	            "writeBatchSize": 100,
	            "writeBatchTimeout": "01:00:00"
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

## Verknüpfte Dienste
Es gibt zwei Arten von verknüpften Diensten, die Sie verwenden können, um einen Azure-Blobspeicher mit einer Azure Data Factory zu verknüpfen: **AzureStorage** und **AzureStorageSas**. Der mit Azure Storage verknüpfte Dienst bietet der Data Factory einen globalen Zugriff auf Azure-Speicher. Dagegen bietet der mit Azure Storage SAS (Shared Access Signature) verknüpfte Dienst der Data Factory einen eingeschränkten bzw. zeitgebundenen Zugriff auf Azure-Speicher. Es gibt keine weitere Unterschiede zwischen diesen beiden verknüpften Diensten. Wählen Sie den verknüpften Dienst, der Ihren Anforderungen entspricht. Die folgenden Abschnitte bieten weitere Informationen zu diesen beiden verknüpften Diensten.

[AZURE.INCLUDE [data-factory-azure-storage-linked-services](../../includes/data-factory-azure-storage-linked-services.md)]

## Eigenschaften des Dataset-Typs "Azure-Tabelle"

Eine vollständige Liste der Abschnitte und Eigenschaften, die zum Definieren von Datasets zur Verfügung stehen, finden Sie im Artikel [Erstellen von Datasets](data-factory-create-datasets.md). Abschnitte wie „structure“, „availability“ und „policy“ des JSON-Codes eines Datasets sind bei allen Dataset-Typen (Azure SQL, Azure-Blob, Azure-Tabelle usw.) ähnlich.

Der Abschnitt "typeProperties" unterscheidet sich bei jedem Typ von Dataset und bietet Informationen zum Speicherort der Daten im Datenspeicher. Der Abschnitt **typeProperties** für ein Dataset des Typs **AzureTable** hat die folgenden Eigenschaften.

| Eigenschaft | Beschreibung | Erforderlich |
| -------- | ----------- | -------- |
| tableName | Name der Tabelle in der Azure-Tabellendatenbankinstanz, auf die der verknüpfte Dienst verweist. | Ja. Wenn ein Tabellenname ohne „AzureTableSourceQuery“ angegeben wird, werden alle Datensätze aus der Tabelle an das Ziel kopiert. Bei Angabe von „AzureTableSourceQuery“ werden nur Datensätze, die der Abfrage entsprechen, aus der Tabelle an das Ziel kopiert. |

### Schema per Data Factory
Bei schemafreien Datenspeichern, z. B. Azure-Tabellen, leitet der Data Factory-Dienst das Schema auf eine der folgenden Weisen ab:

1.	Wenn Sie die Struktur der Daten mithilfe der **structure**-Eigenschaft in der Datasetdefinition angeben, berücksichtigt der Data Factory-Dienst diese Struktur als das Schema. Wenn in diesem Fall eine Zeile keinen Wert für eine Spalte enthält, wird ein NULL-Wert für sie angegeben.
2. Wenn Sie die Struktur der Daten nicht mithilfe der **structure**-Eigenschaft in der Datasetdefinition angeben, leitet Data Factory das Schema unter Verwendung der ersten Zeile in den Daten ab. Wenn in diesem Fall die erste Zeile nicht das vollständige Schema enthält, fehlen im Ergebnis des Kopiervorgangs einige Spalten.

Daher empfiehlt es sich bei schemafreien Datenquellen, die Struktur der Daten mithilfe der **structure**-Eigenschaft anzugeben.

## Eigenschaften von Azure-Tabellen-Kopieraktivitätstypen

Eine vollständige Liste der Abschnitte und Eigenschaften zum Definieren von Aktivitäten finden Sie im Artikel [Erstellen von Pipelines](data-factory-create-pipelines.md). Eigenschaften wie Name, Beschreibung, Eingabe- und Ausgabedatasets und Richtlinien sind für alle Arten von Aktivitäten verfügbar.

Eigenschaften im Abschnitt „typeProperties“ der Aktivität können dagegen je nach Aktivitätstyp variieren. Für die Kopieraktivität variieren die Eigenschaften je nach Art der Quellen und Senken.

**AzureTableSource** unterstützt die folgenden Eigenschaften im Abschnitt "typeProperties":

Eigenschaft | Beschreibung | Zulässige Werte | Erforderlich
-------- | ----------- | -------------- | -------- 
azureTableSourceQuery | Verwendet die benutzerdefinierte Abfrage zum Lesen von Daten. | Abfragezeichenfolge für Azure-Tabelle. Siehe Beispiele im nächsten Abschnitt. | Nein. Wenn ein Tabellenname ohne „AzureTableSourceQuery“ angegeben wird, werden alle Datensätze aus der Tabelle an das Ziel kopiert. Bei Angabe von „AzureTableSourceQuery“ werden nur Datensätze, die der Abfrage entsprechen, aus der Tabelle an das Ziel kopiert.
azureTableSourceIgnoreTableNotFound | Gibt an, ob der Ausnahmefall, dass die Tabelle nicht vorhanden ist, ignoriert werden soll. | TRUE<br/>FALSE | Nein |

### Beispiele für azureTableSourceQuery

Wenn die Spalte für die Azure-Tabelle vom Typ „Zeichenfolge“ ist:

	azureTableSourceQuery": "$$Text.Format('PartitionKey ge \\'{0:yyyyMMddHH00_0000}\\' and PartitionKey le \\'{0:yyyyMMddHH00_9999}\\'', SliceStart)"

Wenn die Spalte für die Azure-Tabelle vom Typ „datetime“ ist:

	"azureTableSourceQuery": "$$Text.Format('DeploymentEndTime gt datetime\\'{0:yyyy-MM-ddTHH:mm:ssZ}\\' and DeploymentEndTime le datetime\\'{1:yyyy-MM-ddTHH:mm:ssZ}\\'', SliceStart, SliceEnd)"


**AzureTableSink** unterstützt die folgenden Eigenschaften im Abschnitt "typeProperties":


Eigenschaft | Beschreibung | Zulässige Werte | Erforderlich  
-------- | ----------- | -------------- | -------- 
azureTableDefaultPartitionKeyValue | Standardmäßiger Partitionsschlüsselwert, der von der Senke verwendet werden kann. | Ein Zeichenfolgenwert. | Nein 
azureTablePartitionKeyName | Geben Sie den Namen der Spalte an, deren Werte als Partitionsschlüssel verwendet werden. Wenn dieser nicht angegeben ist, wird "AzureTableDefaultPartitionKeyValue" als Partitionsschlüssel verwendet. | Ein Spaltenname. | Nein |
azureTableRowKeyName | Geben Sie den Namen der Spalte an, deren Werte als Zeilenschlüssel verwendet werden. Wenn nicht angegeben, verwenden Sie für jede Zeile eine GUID. | Ein Spaltenname. | Nein  
azureTableInsertType | Der Modus zum Einfügen von Daten in eine Azure-Tabelle.<br/><br/>Diese Eigenschaft steuert, ob die Werte von vorhandenen Zeilen in der Ausgabetabelle, deren Partitions- und Zeilenschlüssel übereinstimmen, ersetzt oder zusammengeführt werden. <br/><br/>Informationen zur Funktionsweise dieser Einstellungen (Zusammenführen und Ersetzen) finden Sie in den Themen [Insert or Merge Entity](https://msdn.microsoft.com/library/azure/hh452241.aspx) (Entität einfügen oder zusammenführen) und [Insert or Replace Entity](https://msdn.microsoft.com/library/azure/hh452242.aspx) (Entität einfügen oder ersetzen). <br/><br> Diese Einstellung gilt auf Zeilenebene, nicht auf Tabellenebene, und keine der beiden Optionen löscht Zeilen in der Ausgabetabelle, die in der Eingabe nicht vorhanden sind. | merge (default)<br/>replace | Nein 
writeBatchSize | Fügt Daten in die Azure-Tabelle ein, wenn "writeBatchSize" oder "writeBatchTimeout" erreicht wird. | Integer (Gesamtanzahl von Zeilen)| Nein (Standard = 10000) 
writeBatchTimeout | Fügt Daten in die Azure-Tabelle ein, wenn "writeBatchSize" oder "writeBatchTimeout" erreicht wird. | Zeitraum<br/><br/>Beispiel: 00:20:00 (20 Minuten) | Nein (Standardmäßiger Timeoutwert von 90 Sekunden für Speicherclient)

### azureTablePartitionKeyName
Ordnen Sie einer Zielspalte mithilfe der JSON-Eigenschaft „translator“ eine Quellspalte zu, bevor Sie die Zielspalte als azureTablePartitionKeyName verwenden können.

Im folgenden Beispiel wird die Quellspalte "DivisionID" der Zielspalte "DivisionID" zugeordnet.

	"translator": {
		"type": "TabularTranslator",
		"columnMappings": "DivisionID: DivisionID, FirstName: FirstName, LastName: LastName"
	} 

„DivisionID“ ist als Partitionsschlüssel angegeben.

	"sink": {
		"type": "AzureTableSink",
		"azureTablePartitionKeyName": "DivisionID",
		"writeBatchSize": 100,
		"writeBatchTimeout": "01:00:00"
	}


[AZURE.INCLUDE [data-factory-structure-for-rectangular-datasets](../../includes/data-factory-structure-for-rectangualr-datasets.md)]

### Typzuordnung für Azure-Tabelle

Wie im Artikel [Datenverschiebungsaktivitäten](data-factory-data-movement-activities.md) beschrieben, führt die Kopieraktivität automatische Typkonvertierungen von Quelltypen in Senkentypen mithilfe des folgenden aus zwei Schritten bestehenden Ansatzes durch:

1. Konvertieren von systemeigenen Quelltypen in den .NET-Typ
2. Konvertieren vom .NET-Typ in systemeigenen Senkentyp

Beim Verschieben von Daten in die und aus der Azure-Tabelle werden die folgenden [vom Azure-Tabellenspeicherdienst definierten Zuordnungen](https://msdn.microsoft.com/library/azure/dd179338.aspx) bei Verschiebungen von Azure-Tabellen-OData-Typen in den .NET-Typ und umgekehrt verwendet.

| OData-Datentyp | .NET-Typ | Details |
| --------------- | --------- | ------- |
| Edm.Binary | byte | Ein Array von Bytes mit einer Größe bis zu 64KB. |
| Edm.Boolean | bool | Ein boolescher Wert. |
| Edm.DateTime | DateTime | Ein 64-Bit-Wert, ausgedrückt als koordinierte Weltzeit (UTC). Der unterstützte DateTime-Bereich beginnt um 00:00 Uhr, Mitternacht, 1. Januar, 1601 n. Chr. (unsere Zeitrechnung), UTC Der Bereich endet am 31. Dezember 9999. |
| Edm.Double | double | Ein 64-Bit-Gleitkommawert. |
| Edm.Guid | GUID | Ein 128-Bit-GUID. |
| Edm.Int32 | Int32 oder int | Eine 32-Bit-Ganzzahl. |
| Edm.Int64 | Int64 oder long | Eine 64-Bit-Ganzzahl. |
| Edm.String | String | Ein UTF-16-codierter Wert. Zeichenfolgenwerte können bis zu 64KB groß sein. |

### Beispiel für Typkonvertierung

Im folgenden Beispiel wird das Kopieren von Daten aus einem Azure-Blob in eine Azure-Tabelle mit Typumwandlungen gezeigt.

Es wird vorausgesetzt, dass das Blobdataset im CSV-Format vorliegt und drei Spalten enthält. Eine davon ist eine datetime-Spalte mit einem benutzerdefinierten datetime-Format mit abgekürzten französischen Namen für die Wochentage.

Definieren Sie das Blob-Quelldataset wie folgt zusammen mit Typdefinitionen für die Spalten.
	
	{
	    "name": " AzureBlobInput",
	    "properties":
	    {
	         "structure": 
	          [
	                { "name": "userid", "type": "Int64"},
	                { "name": "name", "type": "String"},
	                { "name": "lastlogindate", "type": "Datetime", "culture": "fr-fr", "format": "ddd-MM-YYYY"}
	          ],
	        "type": "AzureBlob",
	        "linkedServiceName": "StorageLinkedService",
	        "typeProperties": {
	            "folderPath": "mycontainer/myfolder",
	            "fileName":"myfile.csv",
	            "format":
	            {
	                "type": "TextFormat",
	                "columnDelimiter": ","
	            }
	        },
	        "external": true,
	        "availability":
	        {
	            "frequency": "Hour",
	            "interval": 1,
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

Zur Zuordnung des Azure-Tabellen-OData-Typs zum obigen .NET-Typ würden Sie die Tabelle in der Azure-Tabelle mit dem folgenden Schema definieren.

**Azure-Tabellenschema:**

Spaltenname | Typ
----------- | --------
userid | Edm.Int64
name | Edm.String 
lastlogindate | Edm.DateTime

Als Nächstes definieren Sie das Azure-Tabellendataset wie folgt. Sie müssen keinen Abschnitt "structure" mit den Typinformationen angeben, da die Typinformationen bereits im zugrunde liegenden Datenspeicher angegeben werden.

	{
	  "name": "AzureTableOutput",
	  "properties": {
	    "type": "AzureTable",
	    "linkedServiceName": "StorageLinkedService",
	    "typeProperties": {
	      "tableName": "MyOutputTable"
	    },
	    "availability": {
	      "frequency": "Hour",
	      "interval": 1
	    }
	  }
	}

In diesem Fall führt Data Factory die Typkonvertierungen automatisch einschließlich des Datetime-Felds mit dem benutzerdefinierten Datetime-Format aus. Dabei wird die Kultur „fr-fr“ beim Verschieben von Daten aus dem Blob in die Azure-Tabelle verwendet.



[AZURE.INCLUDE [data-factory-column-mapping](../../includes/data-factory-column-mapping.md)]

## Leistung und Optimierung  
Wichtige Faktoren, die sich auf die Leistung der Datenverschiebung (Kopieraktivität) in Azure Data Factory auswirken, sowie verschiedene Möglichkeiten zur Leistungsoptimierung werden im Artikel [Handbuch zur Leistung und Optimierung der Kopieraktivität](data-factory-copy-activity-performance.md) beschrieben.

<!---HONumber=AcomDC_0921_2016-->