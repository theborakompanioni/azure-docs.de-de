<properties 
	pageTitle="Verschieben von Daten in/aus Azure SQL Data Warehouse | Microsoft Azure" 
	description="Erfahren Sie, wie Daten mithilfe von Azure Data Factory in und aus Azure SQL Data Warehouse verschoben werden." 
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
	ms.date="09/20/2016" 
	ms.author="spelluru"/>

# Verschieben von Daten in und aus Azure SQL Data Warehouse mithilfe von Azure Data Factory

In diesem Artikel erfahren Sie, wie Sie die Kopieraktivität in Azure Data Factory verwenden, um Daten aus Azure SQL Data Warehouse in einen anderen Datenspeicher zu verschieben (und umgekehrt).

Sie können angeben, ob Sie PolyBase beim Laden von Daten in Azure SQL Data Warehouse verwenden möchten. Es wird empfohlen, dass Sie PolyBase verwenden, um beim Laden von Daten in Azure SQL Data Warehouse eine optimale Leistung zu erzielen. Details finden Sie im Abschnitt [Daten unter Verwendung von PolyBase in Azure SQL Data Warehouse laden](data-factory-azure-sql-data-warehouse-connector.md#use-polybase-to-load-data-into-azure-sql-data-warehouse).


## Assistent zum Kopieren von Daten
Die einfachste Möglichkeit zum Erstellen einer Pipeline, die Daten in und aus Azure SQL Data Warehouse kopiert, ist die Verwendung des Assistenten zum Kopieren von Daten. Unter [Tutorial: Erstellen einer Pipeline mit dem Assistenten zum Kopieren](data-factory-copy-data-wizard-tutorial.md) finden Sie eine kurze exemplarische Vorgehensweise zum Erstellen einer Pipeline mithilfe des Assistenten zum Kopieren von Daten.

Die folgenden Beispiele zeigen JSON-Beispieldefinitionen, die Sie zum Erstellen einer Pipeline mit dem [Azure-Portal](data-factory-copy-activity-tutorial-using-azure-portal.md), mit [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) oder [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md) verwenden können. Sie zeigen Ihnen das Kopieren von Daten in und aus Azure SQL Data Warehouse und Azure Blob Storage. Daten können jedoch mithilfe der Kopieraktivität in Azure Data Factory **direkt** aus beliebigen Quellen in die [hier](data-factory-data-movement-activities.md#supported-data-stores) aufgeführten Senken kopiert werden.


> [AZURE.NOTE] 
Eine Übersicht über den Azure Data Factory-Dienst finden Sie unter [Einführung in Azure Data Factory](data-factory-introduction.md).
> 
> Dieser Artikel bietet JSON-Beispiele, aber keine detaillierten Anleitungen zum Erstellen einer Data Factory. Unter [Kopieren von Daten aus Blob Storage in SQL-Datenbank mithilfe von Data Factory](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) finden Sie eine kurze exemplarische Vorgehensweise mit detaillierten Anleitungen zur Verwendung der Kopieraktivität in Azure Data Factory.


## Beispiel: Kopieren von Daten aus Azure SQL Data Warehouse in ein Azure-Blob

Das Beispiel definiert folgende Data Factory-Entitäten:

1. Einen verknüpften Dienst des Typs [AzureSqlDW](#azure-sql-data-warehouse-linked-service-properties)
2. Einen verknüpften Dienst des Typs [AzureStorage](data-factory-azure-blob-connector.md#azure-storage-linked-service-properties)
3. Ein [Eingabedataset](data-factory-create-datasets.md) des Typs [AzureSqlDWTable](#azure-sql-data-warehouse-dataset-type-properties)
4. Ein [Ausgabedataset](data-factory-create-datasets.md) des Typs [AzureBlob](data-factory-azure-blob-connector.md#azure-blob-dataset-type-properties)
4. Eine [Pipeline](data-factory-create-pipelines.md) mit Kopieraktivität, die [SqlDWSource](#azure-sql-data-warehouse-copy-activity-type-properties) und [BlobSink](data-factory-azure-blob-connector.md#azure-blob-copy-activity-type-properties) verwendet

In dem Beispiel werden jede Stunde Zeitreihendaten (stündlich, täglich usw.) aus einer Tabelle in einer Azure SQL Data Warehouse-Datenbank in ein Blob kopiert. Die bei diesen Beispielen verwendeten JSON-Eigenschaften werden in den Abschnitten beschrieben, die auf die Beispiele folgen.

**Mit Azure SQL Data Warehouse verknüpfter Dienst:**

	{
	  "name": "AzureSqlDWLinkedService",
	  "properties": {
	    "type": "AzureSqlDW",
	    "typeProperties": {
	      "connectionString": "Server=tcp:<servername>.database.windows.net,1433;Database=<databasename>;User ID=<username>@<servername>;Password=<password>;Trusted_Connection=False;Encrypt=True;Connection Timeout=30"
	    }
	  }
	}

**Mit Azure-Blobspeicher verknüpfter Dienst:**

	{
	  "name": "StorageLinkedService",
	  "properties": {
	    "type": "AzureStorage",
	    "typeProperties": {
	      "connectionString": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"
	    }
	  }
	}

**Azure SQL Data Warehouse-Eingabedataset:**

Im Beispiel wird vorausgesetzt, dass Sie die Tabelle "MyTable" in Azure SQL Data Warehouse erstellt haben, die für Zeitreihendaten eine Spalte namens "timestampcolumn" enthält.
 
Durch Festlegen von „external“ auf „true“ wird dem Data Factory-Dienst mitgeteilt, dass das Dataset für die Data Factory extern ist und nicht durch eine Aktivität in der Data Factory erzeugt wird.

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

Die Pipeline enthält eine Kopieraktivität, die für die Verwendung der Ein- und Ausgabedatasets und für eine stündliche Ausführung konfiguriert ist. In der JSON-Definition der Pipeline ist der Typ **source** auf **SqlDWSource** und der Typ **sink** auf **BlobSink** festgelegt. Die für die **SqlReaderQuery**-Eigenschaft angegebene SQL-Abfrage wählt die Daten der letzten Stunde zum Kopieren aus.

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

> [AZURE.NOTE] In dem Beispiel wird **sqlReaderQuery** für „SqlSWSource“ angegeben. Mit der Kopieraktivität wird diese Abfrage für die Azure SQL Data Warehouse-Quelle ausgeführt, um die Daten abzurufen.
>  
> Alternativ dazu können Sie eine gespeicherte Prozedur angeben, indem Sie **sqlReaderStoredProcedureName** und **storedProcedureParameters** angeben (sofern die gespeicherten Prozeduren Parameter verwenden).
>  
> Wenn Sie "sqlReaderQuery" oder "sqlReaderStoredProcedureName" nicht angeben, werden die im Strukturabschnitt des DataSet-JSON-Bereichs definierten Spalten verwendet, um eine Abfrage ("select column1, column2 from mytable") zur Ausführung für das SQL Data Warehouse zu erstellen. Falls die DataSet-Definition nicht über die Struktur verfügt, werden alle Spalten der Tabelle ausgewählt.

## Beispiel: Kopieren von Daten aus einem Azure-Blob in Azure SQL Data Warehouse

Das Beispiel definiert folgende Data Factory-Entitäten:

1.	Einen verknüpften Dienst des Typs [AzureSqlDW](#azure-sql-data-warehouse-linked-service-properties)
2.	Einen verknüpften Dienst des Typs [AzureStorage](data-factory-azure-blob-connector.md#azure-storage-linked-service-properties)
3.	Ein [Eingabedataset](data-factory-create-datasets.md) des Typs [AzureBlob](data-factory-azure-blob-connector.md#azure-blob-dataset-type-properties)
4.	Ein [Ausgabedataset](data-factory-create-datasets.md) des Typs [AzureSqlDWTable](#azure-sql-data-warehouse-dataset-type-properties)
4.	Eine [Pipeline](data-factory-create-pipelines.md) mit Kopieraktivität, die [BlobSource](data-factory-azure-blob-connector.md#azure-blob-copy-activity-type-properties) und [SqlDWSink](#azure-sql-data-warehouse-copy-activity-type-properties) verwendet


In dem Beispiel werden jede Stunde Zeitreihendaten (stündlich, täglich usw.) aus einem Azure-Blob in eine Tabelle in einer Azure SQL Data Warehouse-Datenbank kopiert. Die bei diesen Beispielen verwendeten JSON-Eigenschaften werden in den Abschnitten beschrieben, die auf die Beispiele folgen.

**Mit Azure SQL Data Warehouse verknüpfter Dienst:**

	{
	  "name": "AzureSqlDWLinkedService",
	  "properties": {
	    "type": "AzureSqlDW",
	    "typeProperties": {
	      "connectionString": "Server=tcp:<servername>.database.windows.net,1433;Database=<databasename>;User ID=<username>@<servername>;Password=<password>;Trusted_Connection=False;Encrypt=True;Connection Timeout=30"
	    }
	  }
	}

**Mit Azure-Blobspeicher verknüpfter Dienst:**

	{
	  "name": "StorageLinkedService",
	  "properties": {
	    "type": "AzureStorage",
	    "typeProperties": {
	      "connectionString": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"
	    }
	  }
	}

**Azure-Blob-Eingabedataset:**

Daten werden stündlich aus einem neuen Blob übernommen ("frequency": "hour", "interval": 1). Ordnerpfad und Dateiname des Blobs werden basierend auf der Startzeit des Slices, der verarbeitet wird, dynamisch ausgewertet. Der Ordnerpfad enthält das Jahr, den Monat und den Tag der Startzeit, der Dateiname enthält die Stunde der Startzeit. Die Festlegung von „external“ auf „true“ teilt dem Data Factory-Dienst mit, dass diese Tabelle für die Data Factory extern ist und nicht durch eine Aktivität in der Data Factory erzeugt wird.

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

**Azure SQL Data Warehouse-Ausgabedataset:**

Das Beispiel kopiert Daten in eine Tabelle namens "MyTable" in einer Azure SQL Data Warehouse-Datenbank. Erstellen Sie die Tabelle in Azure SQL Data Warehouse mit der gleichen Anzahl von Spalten, die Sie auch in der Blob-CSV-Datei erwarten. Neue Zeilen werden der Tabelle stündlich hinzugefügt.

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

**Pipeline mit Kopieraktivität**

Die Pipeline enthält eine Kopieraktivität, die für die Verwendung der Ein- und Ausgabedatasets und für eine stündliche Ausführung konfiguriert ist. In der JSON-Definition der Pipeline ist der Typ **source** auf **BlobSource** und der Typ **sink** auf **SqlDWSink** festgelegt.

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
	            "type": "SqlDWSink"
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

Eine exemplarische Vorgehensweise finden Sie im Artikel [Laden von Daten mit Azure Data Factory](../sql-data-warehouse/sql-data-warehouse-get-started-load-with-azure-data-factory.md) der Azure SQL Data Warehouse-Dokumentation.

## Eigenschaften des mit Azure SQL Data Warehouse verknüpften Diensts

Die folgende Tabelle enthält eine Beschreibung der JSON-Elemente, die für den mit Azure SQL Data Warehouse verknüpften Dienst spezifisch sind.

Eigenschaft | Beschreibung | Erforderlich
-------- | ----------- | --------
Typ | Die "type"-Eigenschaft muss auf **AzureSqlDW** festgelegt sein. | Ja
**connectionString** | Geben Sie Informationen, die zur Verbindung mit der Azure SQL Data Warehouse-Instanz erforderlich sind, für die Eigenschaft "connectionString" ein. | Ja

> [AZURE.IMPORTANT] Konfigurieren Sie die [Azure SQL-Datenbankfirewall](https://msdn.microsoft.com/library/azure/ee621782.aspx#ConnectingFromAzure) und den Datenbankserver, um [Azure-Diensten den Zugriff auf den Server zu ermöglichen](https://msdn.microsoft.com/library/azure/ee621782.aspx#ConnectingFromAzure). Wenn Sie nicht aus Azure stammende Daten in Azure SQL Data Warehouse kopieren, müssen Sie außerdem den entsprechenden IP-Adressbereich für den Computer konfigurieren, der Daten an Azure SQL Data Warehouse sendet. Dies gilt auch beim Kopieren von Daten aus lokalen Datenquellen mit Data Factory-Gateway.

## Eigenschaften des Dataset-Typs „Azure SQL Data Warehouse“

Eine vollständige Liste der Abschnitte und Eigenschaften, die zum Definieren von Datasets zur Verfügung stehen, finden Sie im Artikel [Erstellen von Datasets](data-factory-create-datasets.md). Abschnitte wie „structure“, „availability“ und „policy“ des JSON-Codes eines Datasets sind bei allen Dataset-Typen (Azure SQL, Azure-Blob, Azure-Tabelle usw.) ähnlich.

Der Abschnitt "typeProperties" unterscheidet sich bei jedem Typ von Dataset und bietet Informationen zum Speicherort der Daten im Datenspeicher. Der Abschnitt **typeProperties** für ein DataSet des Typs **AzureSqlDWTable** weist die folgenden Eigenschaften auf.

| Eigenschaft | Beschreibung | Erforderlich |
| -------- | ----------- | -------- |
| tableName | Name der Tabelle in der Azure SQL Data Warehouse-Datenbank, auf die der verknüpfte Dienst verweist. | Ja |

## Eigenschaften des Kopieraktivitätstyps „Azure SQL Data Warehouse“

Eine vollständige Liste der Abschnitte und Eigenschaften zum Definieren von Aktivitäten finden Sie im Artikel [Erstellen von Pipelines](data-factory-create-pipelines.md). Eigenschaften wie Name, Beschreibung, Eingabe- und Ausgabetabellen und Richtlinie sind für alle Arten von Aktivitäten verfügbar.

> [AZURE.NOTE]
Die Kopieraktivität verwendet nur eine Eingabe und erzeugt nur eine Ausgabe.

Eigenschaften im Abschnitt „typeProperties“ der Aktivität können dagegen je nach Aktivitätstyp variieren. Für die Kopieraktivität variieren die Eigenschaften je nach Art der Quellen und Senken.

### SqlDWSource

Bei einer Quelle vom Typ **SqlDWSource** sind im Abschnitt **typeProperties** folgende Eigenschaften verfügbar:

| Eigenschaft | Beschreibung | Zulässige Werte | Erforderlich |
| -------- | ----------- | -------------- | -------- |
| sqlReaderQuery | Verwendet die benutzerdefinierte Abfrage zum Lesen von Daten. | SQL-Abfragezeichenfolge. Beispiel: select * from MyTable. | Nein |
| sqlReaderStoredProcedureName | Der Name der gespeicherten Prozedur, die Daten aus der Quelltabelle liest. | Name der gespeicherten Prozedur. | Nein |
| storedProcedureParameters | Parameter für die gespeicherte Prozedur. | Name-Wert-Paare. Die Namen und die Groß-/Kleinschreibung von Parametern müssen denen der Parameter der gespeicherten Prozedur entsprechen. | Nein |

Wenn **sqlReaderQuery** für "SqlDWSource" angegeben ist, führt die Kopieraktivität diese Abfrage für die Azure SQL Data Warehouse-Quelle aus, um die Daten abzurufen.

Alternativ dazu können Sie eine gespeicherte Prozedur angeben, indem Sie **sqlReaderStoredProcedureName** und **storedProcedureParameters** angeben (sofern die gespeicherten Prozeduren Parameter verwenden).

Ohne Angabe von „sqlReaderQuery“ oder „sqlReaderStoredProcedureName“ werden die im Strukturabschnitt des DataSet-JSON-Bereichs definierten Spalten verwendet, um eine Abfrage für SQL Data Warehouse zu erstellen. Beispiel: `select column1, column2 from mytable`. Falls die DataSet-Definition nicht über die Struktur verfügt, werden alle Spalten der Tabelle ausgewählt.

#### Beispiel für "SqlDWSource"

    "source": {
        "type": "SqlDWSource",
        "sqlReaderStoredProcedureName": "CopyTestSrcStoredProcedureWithParameters",
        "storedProcedureParameters": {
            "stringData": { "value": "str3" },
            "id": { "value": "$$Text.Format('{0:yyyy}', SliceStart)", "type": "Int"}
        }
    }

**Die Definition der gespeicherten Prozedur:**

	CREATE PROCEDURE CopyTestSrcStoredProcedureWithParameters
	(
		@stringData varchar(20),
		@id int
	)
	AS
	SET NOCOUNT ON;
	BEGIN
	     select *
	     from dbo.UnitTestSrcTable
	     where dbo.UnitTestSrcTable.stringData != stringData
	    and dbo.UnitTestSrcTable.id != id
	END
	GO
 

### SqlDWSink
**SqlDWSink** unterstützt die folgenden Eigenschaften:

| Eigenschaft | Beschreibung | Zulässige Werte | Erforderlich |
| -------- | ----------- | -------------- | -------- |
| writeBatchSize | Fügt Daten in die SQL-Tabelle ein, wenn die Puffergröße "writeBatchSize" erreicht. | Integer (Gesamtanzahl von Zeilen) | Nein (Standard = 10000) |
| writeBatchTimeout | Die Wartezeit für den Abschluss der Batcheinfügung, bis das Timeout wirksam wird. | Zeitraum<br/><br/> Beispiel: 00:30:00 (30 Minuten) | Nein | 
| sqlWriterCleanupScript | Geben Sie eine Abfrage für die Kopieraktivität an, bei deren Ausführung die Daten eines bestimmten Slice bereinigt werden. Ausführlichere Informationen finden Sie im [Abschnitt zur Wiederholbarkeit](#repeatability-during-copy). | Eine Abfrageanweisung. | Nein |
| allowPolyBase | Gibt an, ob (falls zutreffend) PolyBase anstelle des BULKINSERT-Mechanismus zum Laden von Daten in Azure SQL Data Warehouse verwendet werden soll. <br/><br/>Derzeit kann nur ein Quelldataset vom Typ **Azure-Blob** verwendet werden, bei dem **format** auf **TextFormat** festgelegt ist. <br/><br/>Einschränkungen und Einzelheiten finden Sie im Abschnitt [Daten unter Verwendung von PolyBase in Azure SQL Data Warehouse laden](#use-polybase-to-load-data-into-azure-sql-data-warehouse). | True <br/>False (Standardwert) | Nein |  
| polyBaseSettings | Eine Gruppe von Eigenschaften, die angegeben werden können, wenn die **allowPolybase**-Eigenschaft auf **true** festgelegt ist. | &nbsp; | Nein |  
| rejectValue | Gibt die Anzahl oder den Prozentsatz von Zeilen an, die abgelehnt werden können, bevor für die Abfrage ein Fehler auftritt. <br/><br/>Weitere Informationen zu den PolyBase-Ablehnungsoptionen finden Sie im Abschnitt **Argumente** des Themas [CREATE EXTERNAL TABLE (Transact-SQL)](https://msdn.microsoft.com/library/dn935021.aspx). | 0 (Standardwert), 1, 2, … | Nein |  
| rejectType | Gibt an, ob die rejectValue-Option als Literalwert oder Prozentsatz angegeben ist. | Value (Standardwert), Percentage | Nein |   
| rejectSampleValue | Gibt die Anzahl von Zeilen an, die abgerufen werden, bevor PolyBase den Prozentsatz der abgelehnten Zeilen neu berechnet. | 1, 2, … | Ja, wenn für **rejectType** der Wert **percentage** festgelegt ist. |  
| useTypeDefault | Gibt an, wie fehlende Werte in durch Trennzeichen getrennten Textdateien verarbeitet werden sollen, wenn PolyBase Daten aus der Textdatei abruft.<br/><br/>Weitere Informationen zu dieser Eigenschaft finden Sie im Abschnitt zu Argumenten im Thema [CREATE EXTERNAL FILE FORMAT (Transact-SQL)](https://msdn.microsoft.com/library/dn935026.aspx). | True/False (Standardwert) | Nein | 


#### Beispiel für "SqlDWSink"


    "sink": {
        "type": "SqlDWSink",
        "writeBatchSize": 1000000,
        "writeBatchTimeout": "00:05:00"
    }

## Daten unter Verwendung von PolyBase in Azure SQL Data Warehouse laden
Mit **PolyBase** lassen sich große Datenmengen effizient und mit hohem Durchsatz in Azure SQL Data Warehouse laden. Wenn Sie PolyBase anstelle des standardmäßigen BULKINSERT-Mechanismus verwenden, wird der Durchsatz erheblich gesteigert.

Legen Sie für die **allowPolyBase**-Eigenschaft den Wert **true** fest, wie im folgenden Beispiel für Azure Data Factory gezeigt, um Daten unter Verwendung von PolyBase in Azure SQL Data Warehouse zu kopieren. Wenn Sie „allowPolyBase“ auf „true“ festlegen, können Sie über die **polyBaseSettings**-Eigenschaftengruppe PolyBase-spezifische Eigenschaften festlegen. Im Abschnitt [SqlDWSink](#SqlDWSink) finden Sie Einzelheiten zu den Eigenschaften, die mit „polyBaseSettings“ verwendet werden können.


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

### Direktes Kopieren mithilfe von PolyBase
Wenn Ihre Daten die in diesem Abschnitt beschriebenen Kriterien erfüllen, können Sie mithilfe von PolyBase direkt aus dem Quelldatenspeicher in Azure SQL Data Warehouse kopieren. Andernfalls können Sie [gestaffeltes Kopieren mit PolyBase](#staged-copy-using-polybase) verwenden.

Falls die Anforderungen nicht erfüllt werden, überprüft Azure Data Factory die Einstellungen und greift bei der Datenverschiebung automatisch auf den BULKINSERT-Mechanismus zurück.

1.	**Der mit der Quelle verknüpfte Dienst** ist vom Typ **Azure Storage**. Außerdem ist dieser Dienst nicht für die Verwendung der SAS-Authentifizierung (Shared Access Signature) konfiguriert. Ausführliche Informationen finden Sie unter [Mit Azure Storage verknüpfter Dienst](data-factory-azure-blob-connector.md#azure-storage-linked-service).
2. Das **Eingabedataset** ist vom Typ **Azure-Blob**, und der Formattyp unter Typeigenschaften lautet **OrcFormat** oder **TextFormat** mit folgenden Konfigurationen:
	1. **rowDelimiter** muss **\\n** sein.
	2. **nullValue** ist auf eine leere Zeichenfolge ("") festgelegt.
	3. **encodingName** ist auf **utf-8** festgelegt. Dies ist der **Standardwert**. Legen Sie keinen anderen Wert fest.
	4. **escapeChar** und **quoteChar** sind nicht angegeben.
	5. **Compression** lautet nicht **BZIP2**.
	 
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
3.	Unter **BlobSource** ist keine **skipHeaderLineCount**-Einstellung für die Kopieraktivität in der Pipeline vorhanden.
4.	Unter **SqlDWSink** ist keine **sliceIdentifierColumnName**-Einstellung für die Kopieraktivität in der Pipeline vorhanden. (PolyBase stellt sicher, dass in einem Durchlauf entweder alle oder keine Daten aktualisiert werden). Um **Wiederholbarkeit** zu erreichen, kann **sqlWriterCleanupScript** verwendet werden.
5.	In der zugehörigen Kopieraktivität wird **columnMapping** nicht verwendet.

### Gestaffeltes Kopieren mit PolyBase
Falls Ihre Quelldaten die Kriterien aus dem vorherigen Abschnitt nicht erfüllen, können Sie die Daten über einen zwischengeschalteten Azure-Stagingblobspeicher kopieren. In diesem Fall führt Azure Data Factory die erforderlichen Transformationen für die Daten durch, um die Datenformatanforderungen von PolyBase zu erfüllen, und die Daten werden anschließend mithilfe von PolyBase in SQL Data Warehouse geladen. Unter [Gestaffeltes Kopieren](data-factory-copy-activity-performance.md#staged-copy) finden Sie ausführliche Informationen zur allgemeinen Funktionsweise des Kopierens von Daten über ein Azure-Stagingblob.

> [AZURE.IMPORTANT] Beim Kopieren von Daten aus einem lokalen Datenspeicher in Azure SQL Data Warehouse mithilfe von PolyBase und einem Stagingverfahren müssen Sie die JRE 8 (Java Runtime Environment) auf dem Gatewaycomputer installieren, mit dem die Quelldaten in das richtige Format transformiert werden. Für ein 64-Bit-Gateway ist die 64-Bit-JRE erforderlich, für ein 32-Bit-Gateway eine 32-Bit-JRE. Die passende Version können Sie [hier](http://go.microsoft.com/fwlink/?LinkId=808605) herunterladen.

Erstellen Sie zum Verwenden dieses Features einen [verknüpften Azure Storage-Dienst](data-factory-azure-blob-connector.md#azure-storage-linked-service), der auf das Azure Storage-Konto mit dem zwischengeschalteten Blobspeicher verweist. Geben Sie dann die Eigenschaften **enableStaging** und **stagingSettings** für die Kopieraktivität an, wie im folgenden Code zu sehen:

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


### Bewährte Methoden bei Verwendung von PolyBase

#### Zeilengrößeneinschränkung
Polybase unterstützt eine maximale Zeilengröße von 32 KB. Beim Versuch, eine Tabelle mit Zeilen zu laden, die größer sind als 32 KB, tritt der folgende Fehler auf:

	Type=System.Data.SqlClient.SqlException,Message=107093;Row size exceeds the defined Maximum DMS row size: [35328 bytes] is larger than the limit of [32768 bytes],Source=.Net SqlClient

Wenn Sie über Quelldaten mit Zeilen verfügen, deren Größe über 32 KB liegt, sollten Sie die Quelltabellen vertikal in kleinere Tabellen unterteilen, sodass die Zeilengröße der einzelnen Tabellen den Höchstwert nicht überschreitet. Die kleineren Tabellen können dann mit PolyBase geladen und in Azure SQL Data Warehouse zusammengeführt werden.

#### „tableName“ in Azure SQL Data Warehouse
Die folgende Tabelle enthält Beispiele für das Angeben der **tableName**-Eigenschaft in Dataset-JSON für diverse Kombinationen aus Schema und Tabellenname:

| Datenbankschema | Tabellenname | JSON-Eigenschaft „tableName“ |
| --------- | -----------| ----------------------- | 
| dbo | MyTable | MyTable oder dbo.MyTable oder [dbo].[MyTable] |
| dbo1 | MyTable | dbo1.MyTable oder [dbo1].[MyTable] |
| dbo | My.Table | [My.Table] oder [dbo].[My.Table] |
| dbo1 | My.Table | [dbo1].[My.Table] |

Sollte der folgende Fehler auftreten, liegt dies unter Umständen am Wert für die tableName-Eigenschaft. Informationen zur korrekten Angabe von Werten für die JSON-Eigenschaft „tableName“ finden Sie in der Tabelle.

	Type=System.Data.SqlClient.SqlException,Message=Invalid object name 'stg.Account_test'.,Source=.Net SqlClient Data Provider

#### Spalten mit Standardwerten
Das PolyBase-Feature in Data Factory akzeptiert aktuell lediglich dieselbe Anzahl von Spalten wie in der Zieltabelle. Angenommen, Sie verfügen über eine Tabelle mit vier Spalten, von denen eine mit einem Standardwert definiert ist. Die Eingabedaten müssen trotzdem vier Spalten umfassen. Bei Bereitstellung eines Eingabedatasets mit drei Spalten tritt ein Fehler wie der folgende auf:

	All columns of the table must be specified in the INSERT BULK statement.

Der NULL-Wert ist eine Sonderform eines Standardwerts. Wenn die Spalte NULL-Werte zulässt, könnten die Eingabedaten (im Blob) für diese Spalte leer sein (sie dürfen im Eingabedataset nicht fehlen). PolyBase fügt für diese Daten in Azure SQL Data Warehouse den Wert „NULL“ ein.


[AZURE.INCLUDE [data-factory-type-repeatability-for-sql-sources](../../includes/data-factory-type-repeatability-for-sql-sources.md)]

[AZURE.INCLUDE [data-factory-structure-for-rectangular-datasets](../../includes/data-factory-structure-for-rectangualr-datasets.md)]

### Typzuordnung für Azure SQL Data Warehouse

Wie im Artikel [Datenverschiebungsaktivitäten](data-factory-data-movement-activities.md) beschrieben, führt die Kopieraktivität mithilfe der beiden folgenden Schritte automatische Typkonvertierungen von Quelltypen in Senkentypen durch:

1. Konvertieren von systemeigenen Quelltypen in den .NET-Typ
2. Konvertieren vom .NET-Typ in systemeigenen Senkentyp

Beim Verschieben von Daten in und aus Azure SQL, SQL Server und Sybase werden die folgenden Zuordnungen zwischen SQL-Typ und .NET-Typ und umgekehrt verwendet.

Die Zuordnung ist mit der [SQL Server-Datentypzuordnung für ADO.NET](https://msdn.microsoft.com/library/cc716729.aspx) identisch.

| Typ "SQL Server-Datenbankmodul" | Typ ".NET Framework" |
| ------------------------------- | ------------------- |
| bigint | Int64 |
| binary | Byte |
| Bit | Boolean |
| char | String, Char |
| date | DateTime |
| Datetime | DateTime |
| datetime2 | DateTime |
| Datetimeoffset | DateTimeOffset |
| Decimal | Decimal |
| FILESTREAM-Attribut (varbinary(max)) | Byte |
| Float | Doppelt |
| image | Byte | 
| int | Int32 | 
| money | Decimal |
| nchar | String, Char |
| ntext | String, Char |
| numeric | Decimal |
| nvarchar | String, Char |
| real | Single |
| rowversion | Byte |
| smalldatetime | DateTime |
| smallint | Int16 |
| smallmoney | Decimal | 
| sql\_variant | Object * |
| Text | String, Char |
| in | TimeSpan |
| timestamp | Byte |
| tinyint | Byte |
| uniqueidentifier | GUID |
| varbinary | Byte |
| varchar | String, Char |
| xml | Xml |



[AZURE.INCLUDE [data-factory-type-conversion-sample](../../includes/data-factory-type-conversion-sample.md)]

[AZURE.INCLUDE [data-factory-column-mapping](../../includes/data-factory-column-mapping.md)]

## Leistung und Optimierung  
Im Artikel [Handbuch zur Leistung und Optimierung der Kopieraktivität](data-factory-copy-activity-performance.md) werden wichtige Faktoren beschrieben, die sich auf die Leistung der Datenverschiebung (Kopieraktivität) in Azure Data Factory auswirken, sowie verschiedene Möglichkeiten zur Leistungsoptimierung.

<!---HONumber=AcomDC_0921_2016-->