<properties 
	pageTitle="Verschieben von Daten aus einem lokalem HDFS mithilfe von Azure Data Factory | Azure Data Factory" 
	description="Erfahren Sie, wie Sie Daten aus einem lokalem HDFS mithilfe von Azure Data Factory verschieben." 
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
	ms.date="04/05/2016" 
	ms.author="spelluru"/>

# Verschieben von Daten aus einem lokalem HDFS mithilfe von Azure Data Factory
Dieser Artikel beschreibt, wie Sie die Kopieraktivität in einer Azure Data Factory verwenden können, um Daten aus einem lokalen HDFS (Hadoop Distributed File System) in einen anderen Datenspeicher zu verschieben. Dieser Artikel baut auf dem Artikel [Datenverschiebungsaktivitäten](data-factory-data-movement-activities.md) auf, der eine allgemeine Übersicht zur Datenverschiebung mit Kopieraktivität und unterstützten Datenspeicherkombinationen bietet.

Data Factory unterstützt derzeit nur das Verschieben von Daten aus einem lokalen HDFS in andere Datenspeicher, aber nicht das Verschieben aus anderen Datenspeichern in ein lokales HDFS.


## Herstellen der Verbindung
Der Data Factory-Dienst unterstützt das Herstellen einer Verbindung mit einem lokalen HDFS über das Datenverwaltungsgateway. Im Artikel [Verschieben von Daten zwischen lokalen Standorten und Cloud](data-factory-move-data-between-onprem-and-cloud.md) erfahren mehr zum Datenverwaltungsgateway und erhalten eine schrittweise Anleitung zum Einrichten des Gateways. Sie müssen das Gateway nutzen, um eine Verbindung mit dem HDFS herzustellen, auch wenn das HDFS einer Azure-IaaS-VM gehostet wird.

Grundsätzlich ist es zwar möglich, das Gateway auf dem gleichen lokalen Computer bzw. auf dem gleichen virtuellen Azure-Computer zu installieren, auf dem sich auch das HDFS befindet. Aus Leistungsgründen und zur Vermeidung von Ressourcenkonflikten wird jedoch empfohlen, das Gateway auf einem separaten Computer oder auf einer separaten Azure-IaaS-VM zu installieren. Wenn Sie das Gateway auf einem separaten Computer installieren, muss dieser Computer auf den Computer mit dem HDFS zugreifen können.

## Beispiel: Kopieren von Daten aus einem lokalen HDFS in ein Azure-Blob

In diesem Beispiel wird gezeigt, wie Sie Daten aus einem lokalen HDFS in einen Azure-BLOB-Speicher kopieren. Daten können jedoch mithilfe der Kopieraktivität in Azure Data Factory **direkt** in die [hier](data-factory-data-movement-activities.md#supported-data-stores) aufgeführten Senken kopiert werden.
 
Das Beispiel enthält die folgenden Data Factory-Entitäten:

1.	Einen verknüpften Dienst des Typs [OnPremisesHdfs](#hdfs-linked-service-properties)
2.	Einen verknüpften Dienst des Typs [AzureStorage](data-factory-azure-blob-connector.md#azure-storage-linked-service-properties)
3.	Ein [Eingabedataset](data-factory-create-datasets.md) des Typs [FileShare](#hdfs-dataset-type-properties)
4.	Ein [Ausgabedataset](data-factory-create-datasets.md) des Typs [AzureBlob](data-factory-azure-blob-connector.md#azure-blob-dataset-type-properties)
4.	Eine [Pipeline](data-factory-create-pipelines.md) mit Kopieraktivität, die [FileSystemSource](#hdfs-copy-activity-type-properties) und [BlobSink](data-factory-azure-blob-connector.md#azure-blob-copy-activity-type-properties) verwendet

Das Beispiel kopiert stündlich Daten aus einem Abfrageergebnis in einem lokalen HDFS in ein Blob. Die bei diesen Beispielen verwendeten JSON-Eigenschaften werden in den Abschnitten beschrieben, die auf die Beispiele folgen.

Als ersten Schritt richten Sie das Datenverwaltungsgateway gemäß den Anweisungen im Artikel [Verschieben von Daten zwischen lokalen Standorten und Cloud](data-factory-move-data-between-onprem-and-cloud.md) ein.

**Mit HDFS verknüpfter Dienst**. In diesem Beispiel wird die Windows-Authentifizierung verwendet. Informationen zu den verwendbaren Authentifizierungstypen finden Sie im Abschnitt [Mit HDFS verknüpfter Dienst](#hdfs-linked-service-properties).

	{
	    "name": "HDFSLinkedService",
	    "properties":
	    {
	        "type": "Hdfs",
	        "typeProperties":
	        {
	            "authenticationType": "Windows",
	            "userName": "Administrator",
	            "password": "password",
	            "url" : "http://<machine>:50070/webhdfs/v1/",
	            "gatewayName": "mygateway"
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

**HDFS-Eingabedataset**. Dieses Dataset verweist auf den HDFS-Ordner „DataTransfer/UnitTest/“. Die Pipeline kopiert alle Dateien in diesem Ordner an das Ziel.

Durch Festlegen von „external“ auf „true“ und (optionales) Angeben der Richtlinie „externalData“ wird dem Data Factory-Dienst mitgeteilt, dass dies eine Tabelle ist, die für die Data Factory extern ist und die nicht durch eine Aktivität in der Data Factory erzeugt wird.
	
	{
	    "name": "InputDataset",
	    "properties": {
	        "type": "FileShare",
	        "linkedServiceName": "HDFSLinkedService",
	        "typeProperties": {
	            "folderPath": "DataTransfer/UnitTest/"
	        },
	        "external": true,
	        "availability": {
	            "frequency": "Hour",
	            "interval":  1
	        }
	    }
	}




**Azure-Blob-Ausgabedataset**

Daten werden stündlich in ein neues Blob geschrieben ("frequency": "hour", "interval": 1). Der Ordnerpfad des Blobs wird basierend auf der Startzeit des Slices, der verarbeitet wird, dynamisch ausgewertet. Im Ordnerpfad werden Jahr, Monat, Tag und die Stundenteile der Startzeit verwendet.

	{
	    "name": "OutputDataset",
	    "properties": {
	        "type": "AzureBlob",
	        "linkedServiceName": "AzureStorageLinkedService",
	        "typeProperties": {
	            "folderPath": "mycontainer/hdfs/yearno={Year}/monthno={Month}/dayno={Day}/hourno={Hour}",
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
	                        "format": "%M"
	                    }
	                },
	                {
	                    "name": "Day",
	                    "value": {
	                        "type": "DateTime",
	                        "date": "SliceStart",
	                        "format": "%d"
	                    }
	                },
	                {
	                    "name": "Hour",
	                    "value": {
	                        "type": "DateTime",
	                        "date": "SliceStart",
	                        "format": "%H"
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

Die Pipeline enthält eine Kopieraktivität, die für das Verwenden der oben genannten Ein- und Ausgabedatasets und für eine stündliche Ausführung konfiguriert ist. In der JSON-Definition der Pipeline ist der Typ **source** auf **FileSystemSource** und der Typ **sink** auf **BlobSink** festgelegt. Die für die **query**-Eigenschaft angegebene SQL-Abfrage wählt die aus der letzten Stunde zu kopierenden Daten aus.
	
	{
	    "name": "pipeline",
	    "properties":
	    {
	        "activities":
	        [
	            {
	                "name": "HdfsToBlobCopy",
	                "inputs": [ {"name": "InputDataset"} ],
	                "outputs": [ {"name": "OutputDataset"} ],
	                "type": "Copy",
	                "typeProperties":
	                {
	                    "source":
	                    {
	                        "type": "FileSystemSource"
	                    },
	                    "sink":
	                    {
	                        "type": "BlobSink"
	                    }
	                },
	                "policy":
	                {
	                    "concurrency": 1,
	                    "executionPriorityOrder": "NewestFirst",
	                    "retry": 1,
	                    "timeout": "00:05:00"
	                }
	            }
	        ],
	        "start": "2014-06-01T18:00:00Z",
	        "end": "2014-06-01T19:00:00Z"
	    }
	}



## Eigenschaften des mit HDFS verknüpften Diensts

Die folgende Tabelle enthält eine Beschreibung der JSON-Elemente, die für den mit HDFS verknüpften Dienst spezifisch sind.

| Eigenschaft | Beschreibung | Erforderlich |
| -------- | ----------- | -------- | 
| type | Die type-Eigenschaft muss auf **Hdfs** festgelegt sein. | Ja | 
| Url | Die URL für das HDFS. | Ja |
| encryptedCredential | Die Ausgabe der Anmeldeinformationen von [New-AzureRMDataFactoryEncryptValue](https://msdn.microsoft.com/library/mt603802.aspx) für den Zugriff | Nein |
| userName | Der Benutzername für die Windows-Authentifizierung. | Ja (für die Windows-Authentifizierung)
| password | Das Kennwort für die Windows-Authentifizierung. | Ja (für die Windows-Authentifizierung)
| authenticationType | Windows oder Anonymous. | Ja |
| gatewayName | Der Name des Gateways, das der Data Factory-Dienst zum Verbinden mit dem HDFS verwenden soll. | Ja |   

Ausführliche Informationen zum Festlegen von Anmeldeinformationen für ein lokales HDFS finden Sie unter [Festlegen von Anmeldeinformationen und Sicherheit](data-factory-move-data-between-onprem-and-cloud.md#set-credentials-and-security).

### Verwenden der anonymen Authentifizierung

	{
	    "name": "hdfs",
	    "properties":
	    {
	        "type": "Hdfs",
	        "typeProperties":
	        {
	            "authenticationType": "Anonymous",
	            "userName": "hadoop",
	            "url" : "http://<machine>:50070/webhdfs/v1/",
	            "gatewayName": "mygateway"
	        }
	    }
	}


### Verwenden der Windows-Authentifizierung
	
	{
	    "name": "hdfs",
	    "properties":
	    {
	        "type": "Hdfs",
	        "typeProperties":
	        {
	            "authenticationType": "Windows",
	            "userName": "Administrator",
	            "password": "password",
	            "url" : "http://<machine>:50070/webhdfs/v1/",
	            "gatewayName": "mygateway"
	        }
	    }
	}
 


## Eigenschaften des Dataset-Typs „HDFS“

Eine vollständige Liste der Abschnitte und Eigenschaften, die zum Definieren von Datasets zur Verfügung stehen, finden Sie im Artikel [Erstellen von Datasets](data-factory-create-datasets.md). Abschnitte wie "structure", "availability" und "policy" des JSON-Codes eines Datasets sind bei allen Typen von Datasets (Azure SQL, Azure-Blob, Azure-Tabelle usw.) ähnlich.

Der Abschnitt **typeProperties** unterscheidet sich bei jedem Typ von Dataset und bietet Informationen zum Speicherort der Daten im Datenspeicher. Der Abschnitt „typeProperties“ für ein Dataset vom Typ **FileShare** (wozu ein HDFS-Dataset gehört) hat die folgenden Eigenschaften:

Eigenschaft | Beschreibung | Erforderlich
-------- | ----------- | --------
folderPath | Pfad zum Ordner. Beispiel: myfolder<br/><br/>Verwenden Sie für Sonderzeichen in der Zeichenfolge das Escapezeichen "\". Geben Sie für "folder\\subfolder" z. B. "folder\\\subfolder" und für "d:\\samplefolder" die Syntax "d:\\\samplefolder" an.<br/><br/>Sie können dies mit **partitionBy** kombinieren, damit Ordnerpfade auf der Datum-/Uhrzeitangabe für Anfangs- und Endwert von Slices basieren. | Ja
fileName | Geben Sie den Namen der Datei in **folderPath** an, wenn die Tabelle auf eine bestimmte Datei im Ordner verweisen soll. Wenn Sie keine Werte für diese Eigenschaft angeben, verweist die Tabelle auf alle Dateien im Ordner.<br/><br/>Wenn "fileName" für ein Ausgabedataset nicht angegeben wird, hat der Name der generierten Datei das folgende Format: <br/><br/>Data.<Guid>.txt (Beispiel: Data.0a405f8a-93ff-4c6f-b3be-f69616f1df7a.txt) | Nein
partitionedBy | "partitionedBy" kann genutzt werden, um einen dynamischen Wert für "folderPath" oder "filename" für Zeitreihendaten anzugeben. Beispiel: "folderPath" als Parameter für jedes Stunde mit Daten. | Nein
fileFilter | Geben Sie einen Filter zur Auswahl einer Teilmenge der Dateien in "folderPath" statt alle Dateien an. <br/><br/>Zulässige Werte sind: * (mehrere Zeichen) und ? (einzelnes Zeichen).<br/><br/>Beispiel 1: "fileFilter": "*.log"<br/>Beispiel 2: "fileFilter": 2014-1-?.txt"<br/><br/>** Hinweis**: "fileFilter" eignet sich für ein Eingabedataset des Typs "FileShare". | Nein
| Komprimierung | Geben Sie den Typ und den Grad der Komprimierung für die Daten an. Folgende Typen werden unterstützt: **GZip**, **Deflate** und **BZip2**. Folgende Komprimierungsgrade werden unterstützt: **Optimal** und **Schnellste**. Beachten Sie, dass für Daten im **AvroFormat** oder **OrcFormat** derzeit keine Komprimierungseinstellungen unterstützt werden. Weitere Einzelheiten finden Sie im Abschnitt [Komprimierungsunterstützung](#compression-support). | Nein |
| Format | Die folgenden Formattypen werden unterstützt: **TextFormat**, **AvroFormat**, **JsonFormat**, **OrcFormat**. Sie müssen die **type**-Eigenschaft unter „Format“ auf einen dieser Werte festlegen. Weitere Informationen finden Sie in den Abschnitten [Angeben von TextFormat](#specifying-textformat), [Angeben von AvroFormat](#specifying-avroformat), [Angeben von JsonFormat](#specifying-jsonformat) und [Angeben von OrcFormat](#specifying-orcformat). Wenn Sie Dateien unverändert zwischen dateibasierten Speichern kopieren möchten (binäre Kopie), können Sie den Formatabschnitt bei den Definitionen von Eingabe- und Ausgabedatasets überspringen. | Nein 


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

## Eigenschaften von HDFS-Kopieraktivitätstyp

Eine vollständige Liste der Abschnitte und Eigenschaften zum Definieren von Aktivitäten finden Sie im Artikel [Erstellen von Pipelines](data-factory-create-pipelines.md). Eigenschaften wie Name, Beschreibung, Eingabe- und Ausgabetabellen, verschiedene Richtlinien usw. sind für alle Arten von Aktivitäten verfügbar.

Im Abschnitt "typeProperties" der Aktivität verfügbare Eigenschaften variieren hingegen bei jedem Aktivitätstyp. Bei der Kopieraktivität variieren sie je nach Typ der Quellen und Senken.

Wenn die Quelle bei der Kopieraktivität vom Typ **FileSystemSource** ist, sind im Abschnitt „typeProperties“ die folgenden Eigenschaften verfügbar:

**FileSystemSource** unterstützt die folgenden Eigenschaften:

| Eigenschaft | Beschreibung | Zulässige Werte | Erforderlich |
| -------- | ----------- | -------------- | -------- |
| recursive | Gibt an, ob die Daten rekursiv aus den Unterordnern oder nur aus dem angegebenen Ordner gelesen werden. | True/False (Standardwert)| Nein |

[AZURE.INCLUDE [data-factory-column-mapping](../../includes/data-factory-column-mapping.md)]

[AZURE.INCLUDE [data-factory-structure-for-rectangular-datasets](../../includes/data-factory-structure-for-rectangualr-datasets.md)]

## Leistung und Optimierung  
Der Artikel [Handbuch zur Leistung und Optimierung der Kopieraktivität](data-factory-copy-activity-performance.md) beschreibt wichtige Faktoren, die sich auf die Leistung der Datenverschiebung (Kopieraktivität) in Azure Data Factory auswirken, sowie verschiedene Möglichkeiten zur Leistungsoptimierung.

<!---HONumber=AcomDC_0608_2016-->