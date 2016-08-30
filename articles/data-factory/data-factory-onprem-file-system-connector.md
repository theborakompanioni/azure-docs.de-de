<properties 
	pageTitle="Verschieben von Daten in und aus dem Dateisystem | Azure Data Factory" 
	description="Erfahren Sie, wie Daten mithilfe von Azure Data Factory in das und aus dem lokalen Dateisystem verschoben werden." 
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
	ms.date="07/13/2016" 
	ms.author="spelluru"/>

# Verschieben von Daten in das lokale und aus dem lokalen Dateisystem mithilfe von Azure Data Factory

Dieser Artikel beschreibt, wie Sie die Data Factory-Kopieraktivität zum Verschieben von Daten in das und aus dem lokalen Dateisystem verwenden können. Eine Liste mit Datenspeichern, die als Quellen oder Senken mit dem lokalen Dateisystem verwendet werden können, finden Sie unter [Unterstützte Datenspeicher](data-factory-data-movement-activities.md#supported-data-stores). Dieser Artikel baut auf dem Artikel [Datenverschiebungsaktivitäten](data-factory-data-movement-activities.md) auf, der eine allgemeine Übersicht zur Datenverschiebung mit Kopieraktivität und unterstützten Datenspeicherkombinationen bietet.

Data Factory unterstützt das Herstellen einer Verbindung mit dem lokalen Dateisystem mithilfe des Datenverwaltungsgateways. Im Artikel [Verschieben von Daten zwischen lokalen Standorten und Cloud](data-factory-move-data-between-onprem-and-cloud.md) erfahren mehr zum Datenverwaltungsgateway und erhalten eine schrittweise Anleitung zum Einrichten des Gateways.

> [AZURE.NOTE] 
Abgesehen vom Datenverwaltungsgateway müssen keine anderen Binärdateien installiert werden, um die Kommunikation mit dem lokalen Dateisystem zu ermöglichen.
> 
> Unter [Problembehandlung bei Gateways](data-factory-data-management-gateway.md#troubleshoot-gateway-issues) finden Sie Tipps zur Behandlung von Verbindungs- bzw. Gatewayproblemen.

## Linux-Dateifreigabe 

Führen Sie die folgenden beiden Schritte aus, um eine Linux-Dateifreigabe mit dem Dienst zu verwenden, der mit dem Dateiserver verknüpft ist:

- Installieren Sie [Samba](https://www.samba.org/) auf dem Linux-Server.
- Installieren und konfigurieren Sie das Datenverwaltungsgateway auf einem Windows-Server. Das Installieren des Gateways auf einem Linux-Server wird nicht unterstützt.
 
## Assistent zum Kopieren von Daten
Die einfachste Möglichkeit zum Erstellen einer Pipeline, die Daten aus einem und in ein lokales Dateisystem kopiert, ist die Verwendung des Assistenten zum Kopieren von Daten. Unter [Tutorial: Erstellen einer Pipeline mit dem Assistenten zum Kopieren](data-factory-copy-data-wizard-tutorial.md) finden Sie eine kurze exemplarische Vorgehensweise zum Erstellen einer Pipeline mithilfe des Assistenten zum Kopieren von Daten.

Die folgenden Beispiele zeigen JSON-Beispieldefinitionen, die Sie zum Erstellen einer Pipeline mit dem [Azure-Portal](data-factory-copy-activity-tutorial-using-azure-portal.md), mit [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) oder [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md) verwenden können. Sie zeigen, wie Sie Daten in und aus einem lokalen Dateisystem und Azure Blob Storage kopieren. Daten können jedoch auch mithilfe der Kopieraktivität in Azure Data Factory **direkt** aus beliebigen Quellen in die [hier](data-factory-data-movement-activities.md#supported-data-stores) aufgeführten Senken kopiert werden.


## Beispiel: Kopieren von Daten aus dem lokalen Dateisystem in ein Azure-Blob

In diesem Beispiel wird gezeigt, wie Sie Daten aus einem lokalen Dateisystem in einen Azure-BLOB-Speicher kopieren. Daten können jedoch mithilfe der Kopieraktivität in Azure Data Factory **direkt** in die [hier](data-factory-data-movement-activities.md#supported-data-stores) aufgeführten Senken kopiert werden.
 
Das Beispiel enthält die folgenden Data Factory-Entitäten:

- Einen verknüpften Dienst des Typs [OnPremisesFileServer](data-factory-onprem-file-system-connector.md#onpremisesfileserver-linked-service-properties)
- Einen verknüpften Dienst des Typs [AzureStorage](data-factory-azure-blob-connector.md#azure-storage-linked-service-properties)
- Ein [Eingabedataset](data-factory-create-datasets.md) des Typs [FileShare](data-factory-onprem-file-system-connector.md#on-premises-file-system-dataset-type-properties)
- Ein [Ausgabedataset](data-factory-create-datasets.md) des Typs [AzureBlob](data-factory-azure-blob-connector.md#azure-blob-dataset-type-properties)
- Eine [Pipeline](data-factory-create-pipelines.md) mit Kopieraktivität, die [FileSystemSource](data-factory-onprem-file-system-connector.md#file-share-copy-activity-type-properties) und [BlobSink](data-factory-azure-blob-connector.md#azure-blob-copy-activity-type-properties) verwendet

Im folgenden Beispiel werden Daten, die zu einer Zeitreihe gehören, aus dem lokalen Dateisystem stündlich in ein Azure-Blob kopiert. Die bei diesen Beispielen verwendeten JSON-Eigenschaften werden in den Abschnitten beschrieben, die auf die Beispiele folgen.

Als ersten Schritt richten Sie das Datenverwaltungsgateway gemäß den Anweisungen im Artikel [Verschieben von Daten zwischen lokalen Quellen und der Cloud](data-factory-move-data-between-onprem-and-cloud.md) ein.

**Mit lokalem Dateiserver verknüpfter Dienst:**

	{
	  "Name": "OnPremisesFileServerLinkedService",
	  "properties": {
	    "type": "OnPremisesFileServer",
	    "typeProperties": {
	      "host": "\\\Contosogame-Asia.<region>.corp.<company>.com",
	      "userid": "Admin",
	      "password": "123456",
	      "gatewayName": "mygateway"
	    }
	  }
	}

Für „host“ können Sie **Local** oder **localhost** angeben, wenn die Dateifreigabe auf dem Gatewaycomputer selbst erfolgt. Außerdem sollten Sie die Eigenschaft **encryptedCredential** anstelle der Eigenschaften **userid** und **password** verwenden. Weitere Informationen zu diesem verknüpften Dienst finden Sie unter [Eigenschaften des verknüpften Diensts „OnPremisesFileServer“](#onpremisesfileserver-linked-service-properties) .

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

**Eingabedataset aus dem lokalem Dateisystem:**

Daten werden stündlich in eine neue Datei kopiert, wobei der Pfad und Dateiname jeweils Datum und Uhrzeit mit stündlicher Genauigkeit angeben.

Durch Festlegen von "external" auf "true" und Angeben der Richtlinie "externalData" wird dem Azure Data Factory-Dienst angegeben, dass dies eine Tabelle ist, die für die Data Factory extern ist und nicht durch eine Aktivität in der Data Factory erzeugt wird.

	{
	  "name": "OnpremisesFileSystemInput",
	  "properties": {
	    "type": " FileShare",
	    "linkedServiceName": " OnPremisesFileServerLinkedService ",
	    "typeProperties": {
	      "folderPath": "mysharedfolder/yearno={Year}/monthno={Month}/dayno={Day}",
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
	      ]
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
	      "folderPath": "mycontainer/yearno={Year}/monthno={Month}/dayno={Day}/hourno={Hour}",
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

**Kopieraktivität:**

Die Pipeline enthält eine Kopieraktivität, die für das Verwenden der oben genannten Ein- und Ausgabedatasets und für eine stündliche Ausführung konfiguriert ist. In der JSON-Definition der Pipeline ist der Typ **source** auf **FileSystemSource** und der Typ **sink** auf **BlobSink** festgelegt.
	
	{  
	    "name":"SamplePipeline",
	    "properties":{  
	    "start":"2015-06-01T18:00:00",
	    "end":"2015-06-01T19:00:00",
	    "description":"Pipeline for copy activity",
	    "activities":[  
	      {
	        "name": "OnpremisesFileSystemtoBlob",
	        "description": "copy activity",
	        "type": "Copy",
	        "inputs": [
	          {
	            "name": "OnpremisesFileSystemInput"
	          }
	        ],
	        "outputs": [
	          {
	            "name": "AzureBlobOutput"
	          }
	        ],
	        "typeProperties": {
	          "source": {
	            "type": "FileSystemSource"
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

##Beispiel: Kopieren von Daten aus Azure SQL in ein lokales Dateisystem 

Das nachstehende Beispiel zeigt Folgendes:

- Einen verknüpften Dienst des Typs "AzureSqlDatabase"
- Einen verknüpften Dienst des Typs "OnPremisesFileServer"
- Ein Eingabedataset des Typs "AzureSqlTable"
- Ein Ausgabedataset des Typs "FileShare"
- Eine Pipeline mit Kopieraktivität, die "SqlSource" und "FileSystemSink" verwendet

Im Beispiel werden Daten, die zu einer Zeitreihe gehören, stündlich aus einer Tabelle in einer Azure SQL-Datenbank in das lokale Dateisystem kopiert. Die bei diesen Beispielen verwendeten JSON-Eigenschaften werden in den Abschnitten beschrieben, die auf die Beispiele folgen.

**Mit Azure SQL verknüpfter Dienst:**

	{
	  "name": "AzureSqlLinkedService",
	  "properties": {
	    "type": "AzureSqlDatabase",
	    "typeProperties": {
	      "connectionString": "Server=tcp:<servername>.database.windows.net,1433;Database=<databasename>;User ID=<username>@<servername>;Password=<password>;Trusted_Connection=False;Encrypt=True;Connection Timeout=30"
	    }
	  }
	}

**Mit lokalem Dateiserver verknüpfter Dienst:**

	{
	  "Name": "OnPremisesFileServerLinkedService",
	  "properties": {
	    "type": "OnPremisesFileServer",
	    "typeProperties": {
	      "host": "\\\Contosogame-Asia.<region>.corp.<company>.com",
	      "userid": "Admin",
	      "password": "123456",
	      "gatewayName": "mygateway"
	    }
	  }
	}

Für „host“ können Sie **Local** oder **localhost** angeben, wenn die Dateifreigabe auf dem Gatewaycomputer selbst erfolgt. Außerdem sollten Sie die Eigenschaft **encryptedCredential** anstelle der Eigenschaften **userid** und **password** verwenden. Weitere Informationen zu diesem verknüpften Dienst finden Sie unter [Eigenschaften des verknüpften Diensts „OnPremisesFileServer“](#onpremisesfileserver-linked-service-properties) .

**Azure SQL-Eingabedataset:**

Im Beispiel wird vorausgesetzt, dass Sie die Tabelle "MyTable" in Azure SQL erstellt haben, die für Zeitreihendaten eine Spalte namens "timestampcolumn" enthält.

Durch Festlegen von "external" auf "true" und Angeben der Richtlinie "externalData" wird dem Data Factory-Dienst angegeben, dass dies eine Tabelle ist, die für die Data Factory extern ist und nicht durch eine Aktivität in der Data Factory erzeugt wird.

	{
	  "name": "AzureSqlInput",
	  "properties": {
	    "type": "AzureSqlTable",
	    "linkedServiceName": "AzureSqlLinkedService",
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

**Ausgabedataset aus dem lokalem Dateisystem:**

Daten werden stündlich in eine neue Datei kopiert, wobei der Pfad des Blobs jeweils Datum und Uhrzeit mit stündlicher Genauigkeit angibt.

	{
	  "name": "OnpremisesFileSystemOutput",
	  "properties": {
	    "type": "FileShare",
	    "linkedServiceName": " OnPremisesFileServerLinkedService ",
	    "typeProperties": {
	      "folderPath": "mysharedfolder/yearno={Year}/monthno={Month}/dayno={Day}",
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
	      ]
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

**Pipeline mit Kopieraktivität**: Die Pipeline enthält eine Kopieraktivität, die für das Verwenden der oben genannten Ein- und Ausgabedatasets und für eine stündliche Ausführung konfiguriert ist. In der JSON-Definition der Pipeline ist der Typ **source** auf **SqlSource** und der Typ **sink** auf **FileSystemSink** festgelegt. Die für die **SqlReaderQuery**-Eigenschaft angegebene SQL-Abfrage wählt die Daten der letzten Stunde zum Kopieren aus.

	
	{  
	    "name":"SamplePipeline",
	    "properties":{  
	    "start":"2015-06-01T18:00:00",
	    "end":"2015-06-01T20:00:00",
	    "description":"pipeline for copy activity",
	    "activities":[  
	      {
	        "name": "AzureSQLtoOnPremisesFile",
	        "description": "copy activity",
	        "type": "Copy",
	        "inputs": [
	          {
	            "name": "AzureSQLInput"
	          }
	        ],
	        "outputs": [
	          {
	            "name": "OnpremisesFileSystemOutput"
	          }
	        ],
	        "typeProperties": {
	          "source": {
	            "type": "SqlSource",
	            "SqlReaderQuery": "$$Text.Format('select * from MyTable where timestampcolumn >= \\'{0:yyyy-MM-dd}\\' AND timestampcolumn < \\'{1:yyyy-MM-dd}\\'', WindowStart, WindowEnd)"
	          },
	          "sink": {
	            "type": "FileSystemSink"
	          }
	        },
	       "scheduler": {
	          "frequency": "Hour",
	          "interval": 1
	        },
	        "policy": {
	          "concurrency": 1,
	          "executionPriorityOrder": "OldestFirst",
	          "retry": 3,
	          "timeout": "01:00:00"
	        }
	      }
	     ]
	   }
	}

## Eigenschaften des verknüpften Diensts „OnPremisesFileServer“

Sie können ein lokales Dateisystem mithilfe eines verknüpften Dienst des Typs "Lokaler Dateiserver" mit einer Azure Data Factory verknüpfen. Die folgende Tabelle enthält eine Beschreibung der JSON-Elemente, die für den mit dem lokalen Dateiserver verknüpften Dienst spezifisch sind.

Eigenschaft | Beschreibung | Erforderlich
-------- | ----------- | --------
type | Die "type"-Eigenschaft muss auf **OnPremisesFileServer** festgelegt sein. | Ja 
host | Stammpfad des Ordners, den Sie kopieren möchten. Verwenden Sie für Sonderzeichen in der Zeichenfolge das Escapezeichen „\\“. Beispiele finden Sie unter [Beispieldefinitionen für verknüpfte Dienste und Datasets](#sample-linked-service-and-dataset-definitions). | Ja
userid | Geben Sie die ID des Benutzers an, der auf dem Server zugreifen darf. | Nein (wenn Sie "encryptedCredential" auswählen)
password | Geben Sie das Kennwort für das Benutzerkonto (userid) an. | Nein (wenn Sie "encryptedCredential" auswählen) 
encryptedCredential | Geben Sie die verschlüsselten Anmeldeinformationen an. Diese können Sie durch Ausführen des Cmdlets „New-AzureRmDataFactoryEncryptValue“ abrufen.<br/><br/>**Hinweis:** Zur Verwendung von Cmdlets wie „New-AzureRmDataFactoryEncryptValue“, bei denen der type-Parameter auf „OnPremisesFileSystemLinkedService“ festgelegt ist, muss mindestens die Azure PowerShell-Version 0.8.14 verwendet werden. | Nein (wenn Sie "userid" und "password" unverschlüsselt angeben)
gatewayName | Der Name des Gateways, das der Data Factory-Dienst zum Verbinden mit dem lokalen Dateiserver verwenden soll. | Ja

Ausführliche Informationen zum Festlegen von Anmeldeinformationen für eine Datenquelle des lokalen Dateisystems finden Sie unter [Festlegen von Anmeldeinformationen und Sicherheit](data-factory-move-data-between-onprem-and-cloud.md#set-credentials-and-security).

### Beispieldefinitionen für verknüpfte Dienste und Datasets 
Szenario | Host in der Definition des verknüpften Diensts | folderPath in der Datasetdefinition
-------- | --------------------------------- | --------------------- |
Lokaler Ordner auf dem Datenverwaltungsgateway-Computer: <br/><br/>z.B. D:\\* oder D:\\folder\\subfolder\\* | D:\\\ (für Gatewayversion 2.0 und höher) <br/><br/> localhost (für Gatewayversionen unter 2.0) | .\\\ oder folder\\\subfolder (für Gatewayversion 2.0 und höher) <br/><br/>D:\\\ oder D:\\\folder\\\subfolder (für Gatewayversionen unter 2.0)
Freigegebener Remoteordner: <br/><br/>z.B. \\\myserver\\share\\* oder \\\myserver\\share\\folder\\subfolder\\* | \\\\\\myserver\\\share | .\\\ oder folder\\\subfolder

Die **Version** des installierten Gateways ermitteln Sie, indem Sie den [Datenverwaltungsgateway-Konfigurations-Manager](data-factory-data-management-gateway.md#data-management-gateway-configuration-manager) auf Ihrem Computer starten und zur Registerkarte **Hilfe** wechseln.

> [AZURE.NOTE] Wenn Sie in einem Szenario mit lokalem Ordner die host-Eigenschaft als „localhost“ angeben, funktioniert die Kopieraktivität weiterhin mit jeder Gatewayversion, aber Sie können den Kopier-Assistenten nicht zum Einrichten der Kopie verwenden. Es empfiehlt sich, [das Gateway auf Version 2.0 oder höher zu aktualisieren](data-factory-data-management-gateway.md#update-data-management-gateway). Dann können Sie die oben genannten neuen Konfigurationen sowohl in JSON als auch im Kopier-Assistenten verwenden, damit das Szenario funktioniert.

**Beispiel: Mit "username" und "password" im Nur-Text-Format**
	
	{
	  "Name": "OnPremisesFileServerLinkedService",
	  "properties": {
	    "type": "OnPremisesFileServer",
	    "typeProperties": {
	      "host": "\\\Contosogame-Asia",
	      "userid": "Admin",
	      "password": "123456",
	      "gatewayName": "mygateway"
	    }
	  }
	}
	
**Beispiel: Verwenden von "encryptedcredential"**

	{
	  "Name": " OnPremisesFileServerLinkedService ",
	  "properties": {
	    "type": "OnPremisesFileServer",
	    "typeProperties": {
	      "host": "localhost",
	      "encryptedCredential": "WFuIGlzIGRpc3Rpbmd1aXNoZWQsIG5vdCBvbmx5IGJ5xxxxxxxxxxxxxxxxx",
	      "gatewayName": "mygateway"
	    }
	  }
	}

## Eigenschaften des Dataset-Typs „Lokales Dateisystem“

Eine vollständige Liste der Abschnitte und Eigenschaften, die zum Definieren von Datasets zur Verfügung stehen, finden Sie im Artikel [Erstellen von Datasets](data-factory-create-datasets.md). Abschnitte wie "structure", "availability" und "policy" des JSON-Codes eines Datasets sind bei allen Typen von Datasets (Azure SQL, Azure-Blob, Azure-Tabelle, lokales Dateisystem usw.) ähnlich.

Der Abschnitt "typeProperties" unterscheidet sich bei jedem Typ von Dataset und bietet Informationen zum Speicherort, Format usw. der Daten im Datenspeicher. Der Abschnitt "typeProperties" für ein Dataset vom Typ **FileShare** hat die folgenden Eigenschaften.

Eigenschaft | Beschreibung | Erforderlich
-------- | ----------- | --------
folderPath | Unterpfad zum Ordner. Verwenden Sie für Sonderzeichen in der Zeichenfolge das Escapezeichen „\\“. Beispiele finden Sie unter [Beispieldefinitionen für verknüpfte Dienste und Datasets](#sample-linked-service-and-dataset-definitions).<br/><br/>Sie können dies mit **partitionBy** kombinieren, um Ordnerpfade basierend auf Datum und Uhrzeit für Start und Ende des Slices zu erhalten. | Ja
fileName | Geben Sie den Namen der Datei in **folderPath** an, wenn die Tabelle auf eine bestimmte Datei im Ordner verweisen soll. Wenn Sie keine Werte für diese Eigenschaft angeben, verweist die Tabelle auf alle Dateien im Ordner.<br/><br/>Wenn fileName für ein Ausgabedataset nicht angegeben ist, hat der Name der generierten Datei das folgende Format: <br/><br/>Data.<GUID>.txt (Beispiel: Data.0a405f8a-93ff-4c6f-b3be-f69616f1df7a.txt). | Nein
partitionedBy | "partitionedBy" kann genutzt werden, um einen dynamischen Wert für "folderPath" oder "filename" für Zeitreihendaten anzugeben. Beispiel: "folderPath" als Parameter für jedes Stunde mit Daten. | Nein
Format | Die folgenden Formattypen werden unterstützt: **TextFormat**, **AvroFormat**, **JsonFormat** und **OrcFormat**. Sie müssen die **type**-Eigenschaft unter „format“ auf einen dieser Werte festlegen. Weitere Informationen finden Sie in den Abschnitten [Angeben von TextFormat](#specifying-textformat), [Angeben von AvroFormat](#specifying-avroformat), [Angeben von JsonFormat](#specifying-jsonformat) und [Angeben von OrcFormat](#specifying-orcformat). Wenn Sie Dateien unverändert zwischen dateibasierten Speichern kopieren möchten (binäre Kopie), können Sie den Formatabschnitt bei den Definitionen von Eingabe- und Ausgabedatasets überspringen. | Nein
fileFilter | Geben Sie einen Filter zur Auswahl einer Teilmenge der Dateien in "folderPath" statt alle Dateien an. <br/><br/>Zulässige Werte: * (mehrere Zeichen) und ? (einzelnes Zeichen).<br/><br/>Beispiel 1: „fileFilter“: „*.log“<br/>Beispiel 2: „fileFilter“: „2014-1-?.txt“<br/><br/>**Hinweis:** fileFilter eignet sich für ein FileShare-Eingabedataset. | Nein
| Komprimierung | Geben Sie den Typ und den Grad der Komprimierung für die Daten an. Folgende Typen werden unterstützt: **GZip**, **Deflate** und **BZip2**. Folgende Komprimierungsstufen werden unterstützt: **Optimal** und **Schnellste**. Beachten Sie, dass für Daten mit **AvroFormat** oder **OrcFormat** derzeit keine Komprimierungseinstellungen unterstützt werden. Weitere Einzelheiten finden Sie im Abschnitt [Komprimierungsunterstützung](#compression-support). | Nein |

> [AZURE.NOTE] "filename" und "fileFilter" können nicht gleichzeitig verwendet werden.

### Nutzen der "partionedBy"-Eigenschaft

Wie zuvor erwähnt, kann "partitionedBy" genutzt werden, um einen dynamischen Wert für "folderPath" oder "filename" für Zeitreihendaten anzugeben. Sie können dazu die Data Factory-Makros und Systemvariablen "SliceStart" und "SliceEnd" verwenden, die den logischen Zeitraum für einen bestimmten Datenslice angeben.

In den Artikeln [Erstellen von Datasets](data-factory-create-datasets.md), [Planung und Ausführung](data-factory-scheduling-and-execution.md) und [Erstellen von Pipelines](data-factory-create-pipelines.md) finden Sie weitere Details zu Zeitreihendatasets, Planung und Slices.

#### Beispiel 1:

	"folderPath": "wikidatagateway/wikisampledataout/{Slice}",
	"partitionedBy": 
	[
	    { "name": "Slice", "value": { "type": "DateTime", "date": "SliceStart", "format": "yyyyMMddHH" } },
	],

Im obigen Beispiel wird {Slice} durch den Wert der Data Factory-Systemvariablen "SliceStart" ersetzt, die im Format (JJJJMMTTHH) angegeben wird. "SliceStart" bezieht sich auf die Startzeit des Slices. "folderPath" unterscheidet sich für jeden Slice. Beispiel: "wikidatagateway/wikisampledataout/2014100103" oder "wikidatagateway/wikisampledataout/2014100104".

#### Beispiel 2:

	"folderPath": "wikidatagateway/wikisampledataout/{Year}/{Month}/{Day}",
	"fileName": "{Hour}.csv",
	"partitionedBy": 
	 [
	    { "name": "Year", "value": { "type": "DateTime", "date": "SliceStart", "format": "yyyy" } },
	    { "name": "Month", "value": { "type": "DateTime", "date": "SliceStart", "format": "MM" } }, 
	    { "name": "Day", "value": { "type": "DateTime", "date": "SliceStart", "format": "dd" } }, 
	    { "name": "Hour", "value": { "type": "DateTime", "date": "SliceStart", "format": "hh" } } 
	],

Im Beispiel oben werden Jahr, Monat, Tag und Uhrzeit von "SliceStart" in separate Variablen extrahiert, die von den Eigenschaften "folderPath" und "fileName" verwendet werden.

[AZURE.INCLUDE [data-factory-file-format](../../includes/data-factory-file-format.md)]  
[AZURE.INCLUDE [data-factory-compression](../../includes/data-factory-compression.md)]

## Eigenschaften des Kopieraktivitätstyps „Dateifreigabe“

**FileSystemSource** unterstützt die folgenden Eigenschaften:

| Eigenschaft | Beschreibung | Zulässige Werte | Erforderlich |
| -------- | ----------- | -------------- | -------- |
| recursive | Gibt an, ob die Daten rekursiv aus den Unterordnern oder nur aus dem angegebenen Ordner gelesen werden. | True/False (Standardwert)| Nein | 

**FileSystemSink** unterstützt die folgenden Eigenschaften:

| Eigenschaft | Beschreibung | Zulässige Werte | Erforderlich |
| -------- | ----------- | -------------- | -------- |
| copyBehavior | Definiert das Verhalten beim Kopieren, wenn die Quelle "BlobSource" oder "FileSystem" ist. | **PreserveHierarchy**: Behält die Dateihierarchie im Zielordner bei. Der relative Pfad der Quelldatei zum Quellordner entspricht also dem relativen Pfad der Zieldatei zum Zielordner.<br/><br/>**FlattenHierarchy**: Alle Dateien aus dem Quellordner befinden sich auf der ersten Ebene des Zielordners. Die Namen der Zieldateien werden automatisch generiert.<br/><br/>**MergeFiles**: Führt alle Dateien aus dem Quellordner in einer Datei zusammen. Wenn der Datei-/Blob-Name angegeben wurde, entspricht der Name dem angegebenen Namen, andernfalls dem automatisch generierten Dateinamen. | Nein |

### Beispiele für "recursive" und "copyBehavior"
Dieser Abschnitt beschreibt das resultierende Verhalten des Kopiervorgangs für verschiedene Kombinationen von rekursiven und CopyBehavior-Werten.

recursive | copyBehavior | Resultierendes Verhalten
--------- | ------------ | --------
true | preserveHierarchy | Bei einem Quellordner namens „Ordner1“ mit der folgenden Struktur:<br/><br/>Ordner1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Datei1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Datei2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Unterordner1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Datei3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Datei4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Datei5<br/><br/>hat der Zielordner „Ordner1“ die gleiche Struktur wie die Quelle<br/><br/>>Ordner1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Datei1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Datei2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Unterordner1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Datei3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Datei4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Datei5.  
true | flattenHierarchy | Für einen Quellordner „Ordner1“ mit der folgenden Struktur:<br/><br/>Ordner1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Datei1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Datei2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Unterordner1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Datei3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Datei4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Datei5<br/><br/>hat der Zielordner „Ordner1“ folgende Struktur: <br/><br/>Ordner1<br/>&nbsp;&nbsp;&nbsp;&nbsp;automatisch generierter Name für Datei1<br/>&nbsp;&nbsp;&nbsp;&nbsp;automatisch generierter Name für Datei2<br/>&nbsp;&nbsp;&nbsp;&nbsp;automatisch generierter Name für Datei3<br/>&nbsp;&nbsp;&nbsp;&nbsp;automatisch generierter Name für Datei4<br/>&nbsp;&nbsp;&nbsp;&nbsp;automatisch generierter Name für Datei5
true | mergeFiles | Für einen Quellordner „Ordner1“ mit der folgenden Struktur:<br/><br/>Ordner1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Datei1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Datei2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Unterordner1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Datei3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Datei4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Datei5<br/><br/>hat der Zielordner „Ordner1“ folgende Struktur: <br/><br/>Ordner1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Inhalte von Datei1 + Datei2 + Datei3 + Datei4 + Datei5 werden in einer Datei mit einem automatisch generierten Namen zusammengeführt<
false | preserveHierarchy | Für einen Quellordner „Ordner1“ mit der folgenden Struktur:<br/><br/>Ordner1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Datei1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Datei2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Unterordner1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Datei3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Datei4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Datei5<br/><br/>hat der Zielordner „Ordner1“ folgende Struktur:<br/><br/>Ordner1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Datei1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Datei2<br/><br/>Unterordner1 mit Datei3, Datei4 und Datei5 wird nicht ausgewählt.
false | flattenHierarchy | Für einen Quellordner „Ordner1“ mit der folgenden Struktur:<br/><br/>Ordner1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Datei1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Datei2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Unterordner1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Datei3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Datei4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Datei5<br/><br/>hat der Zielordner „Ordner1“ folgende Struktur:<br/><br/>Ordner1<br/>&nbsp;&nbsp;&nbsp;&nbsp;automatisch generierter Name für Datei1<br/>&nbsp;&nbsp;&nbsp;&nbsp;automatisch generierter Name für Datei2<br/><br/>Unterordner1 mit Datei3, Datei4 und Datei5 wird nicht ausgewählt.<
false | mergeFiles | Für einen Quellordner „Ordner1“ mit der folgenden Struktur:<br/><br/>Ordner1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Datei1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Datei2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Unterordner1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Datei3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Datei4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Datei5<br/><br/>hat der Zielordner „Ordner1“ folgende Struktur:<br/><br/>Ordner1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Inhalte von Datei1 + Datei2 werden zu einer Datei mit einem automatisch generierten Namen zusammengeführt. Automatisch generierter Name für Datei1<br/><br/>Unterordner1 mit Datei3, Datei4 und Datei5 wird nicht ausgewählt.


[AZURE.INCLUDE [data-factory-structure-for-rectangular-datasets](../../includes/data-factory-structure-for-rectangualr-datasets.md)]

[AZURE.INCLUDE [data-factory-column-mapping](../../includes/data-factory-column-mapping.md)]

## Leistung und Optimierung  
Im Artikel [Handbuch zur Leistung und Optimierung der Kopieraktivität](data-factory-copy-activity-performance.md) werden wichtige Faktoren beschrieben, die sich auf die Leistung der Datenverschiebung (Kopieraktivität) in Azure Data Factory auswirken, sowie verschiedene Möglichkeiten zur Leistungsoptimierung.






 

<!---HONumber=AcomDC_0817_2016-->