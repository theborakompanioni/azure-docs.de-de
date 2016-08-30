<properties 
	pageTitle="Verschieben von Daten aus MongoDB mit Data Factory | Microsoft Azure" 
	description="Erfahren Sie, wie Sie Daten aus der MongoDB-Datenbank mithilfe von Azure Data Factory verschieben." 
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
	ms.date="08/04/2016" 
	ms.author="spelluru"/>

# Verschieben von Daten aus MongoDB mithilfe von Azure Data Factory

Dieser Artikel beschreibt, wie Sie die Kopieraktivität in einer Azure Data Factory verwenden können, um Daten aus einer lokalen MongoDB-Datenbank in einen anderen Datenspeicher zu verschieben. Dieser Artikel baut auf dem Artikel zu [Datenverschiebungsaktivitäten](data-factory-data-movement-activities.md) auf, der eine allgemeine Übersicht zur Datenverschiebung mit der Kopieraktivität und den unterstützten Datenspeicherkombinationen aus Quelle und Senke bietet.

Der Data Factory-Dienst unterstützt das Herstellen einer Verbindung mit lokalen MongoDB-Datenquellen mithilfe des Datenverwaltungsgateways. Im Artikel [Datenverwaltungsgateway](data-factory-data-management-gateway.md) erfahren Sie etwas über das Datenverwaltungsgateway, und unter [Verschieben von Daten zwischen lokalen Quellen und der Cloud](data-factory-move-data-between-onprem-and-cloud.md) finden Sie schrittweise Anleitungen für das Einrichten des Gateways für eine Datenpipeline zum Verschieben von Daten.

> [AZURE.NOTE] Sie müssen das Gateway nutzen, um eine Verbindung mit MongoDB herzustellen, auch wenn das Hosting in Azure-IaaS-VMs erfolgt. Wenn Sie versuchen, eine Verbindung mit einer MongoDB-Instanz herzustellen, die in der Cloud gehostet wird, können Sie auch die Gatewayinstanz in der IaaS-VM installieren.

Data Factory unterstützt derzeit nur das Verschieben von Daten aus MongoDB in andere Datenspeicher und nicht das Verschieben aus anderen Datenspeichern in MongoDB.

## Voraussetzungen
Damit der Azure Data Factory-Dienst eine Verbindung mit Ihrer lokalen MongoDB-Datenbank herstellen kann, muss Folgendes installiert werden:

- Datenverwaltungsgateway 2.0 oder höher auf dem Computer, der die Datenbank hostet, oder auf einem separaten Computer, um zu vermeiden, dass der Computer mit der Datenbank um Ressourcen konkurriert. Datenverwaltungsgateway ist eine Software, die lokale Datenquellen mit Cloud-Diensten auf sichere, verwaltete Weise verbindet. Im Artikel [Datenverwaltungsgateway](data-factory-data-management-gateway.md) finden Sie Einzelheiten zum Datenverwaltungsgateway.
  
	Beim Installieren des Gateways wird automatisch ein Microsoft MongoDB-ODBC-Treiber installiert, der zum Herstellen einer Verbindung mit MongoDB verwendet wird.

## Assistent zum Kopieren von Daten
Die einfachste Art, eine Pipeline zu erstellen, mit der Daten aus einer MongoDB-Datenbank in einen der unterstützten Senkendatenspeicher kopiert werden, ist die Verwendung des Assistenten zum Kopieren von Daten. Unter [Tutorial: Erstellen einer Pipeline mit dem Assistenten zum Kopieren](data-factory-copy-data-wizard-tutorial.md) finden Sie eine kurze exemplarische Vorgehensweise zum Erstellen einer Pipeline mithilfe des Assistenten zum Kopieren von Daten.

Die folgenden Beispiele zeigen JSON-Beispieldefinitionen, die Sie zum Erstellen einer Pipeline mit dem [Azure-Portal](data-factory-copy-activity-tutorial-using-azure-portal.md), mit [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) oder [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md) verwenden können. Darin wird veranschaulicht, wie Sie Daten aus einer MongoDB-Datenbank in Azure Blob Storage kopieren. Daten können jedoch auch mithilfe der Kopieraktivität in Azure Data Factory in eine beliebige der [hier](data-factory-data-movement-activities.md#supported-data-stores) aufgeführten Senken kopiert werden.

## Beispiel: Kopieren von Daten aus MongoDB in Azure-Blob
In diesem Beispiel wird gezeigt, wie Sie Daten aus einer lokalen MongoDB-Datenbank in einen Azure Blob Storage kopieren. Daten können jedoch mithilfe der Kopieraktivität in Azure Data Factory **direkt** in die [hier](data-factory-data-movement-activities.md#supported-data-stores) aufgeführten Senken kopiert werden.
 
Das Beispiel enthält die folgenden Data Factory-Entitäten:

1.	Einen verknüpften Dienst des Typs [OnPremisesMongoDb](#linked-service-properties)
2.	Einen verknüpften Dienst des Typs [AzureStorage](data-factory-azure-blob-connector.md#azure-storage-linked-service-properties)
3.	Ein [Eingabedataset](data-factory-create-datasets.md) des Typs [MongoDbCollection](#dataset-type-properties)
4.	Ein [Ausgabedataset](data-factory-create-datasets.md) des Typs [AzureBlob](data-factory-azure-blob-connector.md#azure-blob-dataset-type-properties)
4.	Eine [Pipeline](data-factory-create-pipelines.md) mit Kopieraktivität, die [MongoDbSource](#copy-activity-type-properties) und [BlobSink](data-factory-azure-blob-connector.md#azure-blob-copy-activity-type-properties) verwendet

Das Beispiel kopiert Daten stündlich aus einem Abfrageergebnis in einer MongoDB-Datenbank in ein Blob. Die bei diesen Beispielen verwendeten JSON-Eigenschaften werden in den Abschnitten beschrieben, die auf die Beispiele folgen.

Als ersten Schritt richten Sie das Datenverwaltungsgateway gemäß den Anweisungen im Artikel [Datenverwaltungsgateway](data-factory-data-management-gateway.md) ein.

**Mit MongoDB verknüpfter Dienst**

	{ 
	    "name": "OnPremisesMongoDbLinkedService", 
	    "properties": 
	    { 
	        "type": "OnPremisesMongoDb", 
	        "typeProperties": 
	        { 
	            "authenticationType": "<Basic or Anonymous>", 
	            "server": "< The IP address or host name of the MongoDB server >",  
				"port": "<The number of the TCP port that the MongoDB server uses to listen for client connections.>", 
	            "username": "<username>", 
	            "password": "<password>",
	           "authSource": "< The database that you want to use to check your credentials for authentication. >",
	            "databaseName": "<database name>",
	            "gatewayName": "<mygateway>"
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

**MongoDB-Eingabedataset**. Durch Festlegen von „external“ auf TRUE wird dem Data Factory-Dienst mitgeteilt, dass das Dataset für die Data Factory extern ist und nicht durch eine Aktivität in der Data Factory erzeugt wird.
	
	{
	     "name":  "MongoDbInputDataset",
	    "properties": { 
	        "type": "MongoDbCollection", 
	        "linkedServiceName": "OnPremisesMongoDbLinkedService", 
	        "typeProperties": { 
	            "collectionName": "<Collection name>"	
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
	            "folderPath": "mycontainer/frommongodb/yearno={Year}/monthno={Month}/dayno={Day}/hourno={Hour}",
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

Die Pipeline enthält eine Kopieraktivität, die für das Verwenden der oben genannten Ein- und Ausgabedatasets und für eine stündliche Ausführung konfiguriert ist. In der JSON-Definition der Pipeline ist der Typ **source** auf **MongoDbSource** und der Typ **sink** auf **BlobSink** festgelegt. Die für die **query**-Eigenschaft angegebene SQL-Abfrage wählt die aus der letzten Stunde zu kopierenden Daten aus.
	
	{
	    "name": "CopyMongoDBToBlob",
	    "properties": {
	        "description": "pipeline for copy activity",
	        "activities": [
	            {
	                "type": "Copy",
	                "typeProperties": {
	                    "source": {
	                        "type": "MongoDbSource",
	                        "query": "$$Text.Format('select * from  MyTable where LastModifiedDate >= {{ts\'{0:yyyy-MM-dd HH:mm:ss}\'}} AND LastModifiedDate < {{ts\'{1:yyyy-MM-dd HH:mm:ss}\'}}', WindowStart, WindowEnd)"
	                    },
	                    "sink": {
	                        "type": "BlobSink",
	                        "writeBatchSize": 0,
	                        "writeBatchTimeout": "00:00:00"
	                    }
	                },
	                "inputs": [
	                    {
	                        "name": "MongoDbInputDataset"
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
	                "name": "MongoDBToAzureBlob"
	            }
	        ],
	        "start": "2016-06-01T18:00:00Z",
	        "end": "2016-06-01T19:00:00Z"
	    }
	}


## Eigenschaften des verknüpften Diensts

Die folgende Tabelle enthält eine Beschreibung der JSON-Elemente, die für den verknüpften Dienst **OnPremisesMongoDb** spezifisch sind.

| Eigenschaft | Beschreibung | Erforderlich |
| -------- | ----------- | -------- | 
| Typ | Die type-Eigenschaft muss auf **OnPremisesMongoDb** festgelegt sein. | Ja |
| server | IP-Adresse oder Hostname des MongoDB-Servers | Ja | 
| port | Der TCP-Port, den der MongoDB-Server verwendet, um auf Clientverbindungen zu lauschen | Optional, Standardwert: 27017 |
| authenticationType | Basic oder Anonymous | Ja | 
| username | Benutzerkonto für den Zugriff auf MongoDB | Ja (wenn die Standardauthentifizierung verwendet wird)|
| password | Kennwort für den Benutzer |	Ja (wenn die Standardauthentifizierung verwendet wird) | 
| authSource | Der Name der MongoDB-Datenbank, die Sie zum Überprüfen Ihrer Anmeldeinformationen zur Authentifizierung verwenden möchten | Optional (wenn die Standardauthentifizierung verwendet wird). Standardwert: verwendet das Administratorkonto und die Datenbank, die mit der databaseName-Eigenschaft angegeben wird |  
| databaseName | Der Name der MongoDB-Datenbank, auf die Sie zugreifen möchten | Ja |
| gatewayName | Der Name des Gateways, das auf den Datenspeicher zugreift | Ja | 
| encryptedCredential | Anmeldeinformationen, die vom Gateway verschlüsselt werden | Optional |


Ausführliche Informationen zum Festlegen von Anmeldeinformationen für eine lokale MongoDB-Datenquelle finden Sie unter [Festlegen von Anmeldeinformationen und Sicherheit](data-factory-move-data-between-onprem-and-cloud.md#set-credentials-and-security).

## Eigenschaften des Datasettyps

Eine vollständige Liste der Abschnitte und Eigenschaften, die zum Definieren von Datasets zur Verfügung stehen, finden Sie im Artikel [Erstellen von Datasets](data-factory-create-datasets.md). Abschnitte wie "structure", "availability" und "policy" des JSON-Codes eines Datasets sind bei allen Typen von Datasets (Azure SQL, Azure-Blob, Azure-Tabelle usw.) ähnlich.

Der Abschnitt **typeProperties** unterscheidet sich bei jedem Typ von Dataset und bietet Informationen zum Speicherort der Daten im Datenspeicher. Der Abschnitt „typeProperties“ für ein Dataset vom Typ **MongoDbCollection** hat die folgenden Eigenschaften:

| Eigenschaft | Beschreibung | Erforderlich |
| -------- | ----------- | -------- |
| collectionName | Der Name der Sammlung in der MongoDB-Datenbank | Ja | 

## Eigenschaften des Kopieraktivitätstyps

Eine vollständige Liste der Abschnitte und Eigenschaften zum Definieren von Aktivitäten finden Sie im Artikel [Erstellen von Pipelines](data-factory-create-pipelines.md). Eigenschaften wie Name, Beschreibung, Eingabe- und Ausgabetabellen, verschiedene Richtlinien usw. sind für alle Arten von Aktivitäten verfügbar.

Die im Abschnitt **typeProperties** der Aktivität verfügbaren Eigenschaften variieren hingegen bei jedem Aktivitätstyp. Bei der Kopieraktivität variieren sie je nach Typ der Quellen und Senken.

Wenn bei der Kopieraktivität „source“ den Typ **MongoDbSource** hat, sind im Abschnitt „typeProperties“ die folgenden Eigenschaften verfügbar:

| Eigenschaft | Beschreibung | Zulässige Werte | Erforderlich |
| -------- | ----------- | -------------- | -------- |
| query | Verwendet die benutzerdefinierte Abfrage zum Lesen von Daten. | SQL-92-Abfragezeichenfolge. Beispiel: select * from MyTable. | Nein (wenn **collectionName** von **dataset** angegeben ist) | 

## Schema per Data Factory
Der Azure Data Factory-Dienst leitet ein Schema aus einer MongoDB-Sammlung mithilfe der letzten 100 Dokumente in der Sammlung ab. Wenn diese 100 Dokumente kein vollständiges Schema enthalten, können einige Spalten während des Kopiervorgangs ignoriert werden.

## Typzuordnung für MongoDB

Wie im Artikel [Datenverschiebungsaktivitäten](data-factory-data-movement-activities.md) beschrieben, führt die Kopieraktivität automatische Typkonvertierungen von Quelltypen in Senkentypen mithilfe des folgenden aus zwei Schritten bestehenden Ansatzes durch:

1. Konvertieren von systemeigenen Quelltypen in den .NET-Typ
2. Konvertieren vom .NET-Typ in systemeigenen Senkentyp

Beim Verschieben von Daten in MongoDB werden die folgenden Zuordnungen zwischen MongoDB-Typ und .NET-Typ verwendet.

| MongoDB-Typ | Typ ".NET Framework" |
| ------------------- | ------------------- | 
| Binary | Byte |
| Boolean | Boolean |
| Datum | DateTime |
| NumberDouble | Doppelt |
| NumberInt | Int32 |
| NumberLong | Int64 |
| ObjectID | String |
| String | String |
| UUID | GUID | 
| Objekt | Renormalisiert in vereinfachte Spalten mit dem geschachtelten Trennzeichen „\_“ |

> [AZURE.NOTE]  
Weitere Informationen zur Unterstützung für Arrays mit virtuellen Tabellen finden Sie im Abschnitt [Unterstützung für komplexe Typen mit virtuellen Tabellen](#support-for-complex-types-using-virtual-tables) unten.

Die folgenden MongoDB-Datentypen werden zurzeit nicht unterstützt: DBPointer, JavaScript, MaxKey/MinKey, Regular Expression, Symbol, Timestamp, Undefined

## Unterstützung für komplexe Typen mit virtuellen Tabellen
Azure Data Factory verwendet einen integrierten ODBC-Treiber, um eine Verbindung mit der MongoDB-Datenbank herzustellen und Daten daraus zu kopieren. Für komplexe Typen wie Arrays oder Objekte mit unterschiedlichen Typen in den Dokumenten renormalisiert der Treiber die Daten in die entsprechenden virtuellen Tabellen. Wenn eine Tabelle solche Spalten enthält, generiert der Treiber die folgenden virtuellen Tabellen:

-	Eine **Basistabelle**, die die gleichen Daten wie die echte Tabelle enthält, mit Ausnahme der Spalten mit komplexen Typen. Für die Basistabelle wird der gleiche Name wie für die echte Tabelle verwendet, die sie repräsentiert.
-	Eine **virtuelle Tabelle** für jede Spalte mit komplexen Typen (Erweiterung der geschachtelten Daten). Die virtuellen Tabellen werden mit dem Namen der echten Tabelle benannt und erhalten zusätzlich das Trennzeichen „\_“ und den Namen der des Arrays oder Objekts.

Virtuelle Tabellen beziehen sich auf die Daten in der echten Tabelle, sodass der Treiber auf die denormalisierten Daten zugreifen kann. Details finden Sie unten im Abschnitt mit dem Beispiel. Sie können auf den Inhalt von MongoDB-Arrays zugreifen, indem Sie die virtuellen Tabellen abfragen und verknüpfen.

Sie können den [Assistenten zum Kopieren](data-factory-data-movement-activities.md#data-factory-copy-wizard) verwenden, um die Liste mit den Tabellen in der MongoDB-Datenbank, einschließlich der virtuellen Tabellen, intuitiv anzuzeigen. Sie können im Kopier-Assistenten auch eine Abfrage erstellen und eine Überprüfung durchführen, um das Ergebnis anzuzeigen.

### Beispiel

Beispielsweise ist „ExampleTable“ unten eine MongoDB-Tabelle, die eine Spalte (Invoices) mit einem Array von Objekten in jeder Zelle enthält sowie eine Spalte (Ratings) mit einem Array von skalaren Typen.

\_id | Customer Name | Invoices | Service Level | Ratings
--- | ------------- | -------- | ------------- | -------
1111 | ABC | [{invoice\_id:”123”, item:”toaster”, price:”456”, discount:”0.2”}, {invoice\_id:”124”, item:”oven”,price: ”1235”,discount: ”0.2”}] | Silber | [5,6]
2222 | XYZ | [{invoice\_id:”135”, item:”fridge”,price: ”12543”,discount: ”0.0”}] | Gold | [1,2]

Der Treiber erzeugt mehrere virtuelle Tabellen, um diese einzelne Tabelle zu repräsentieren. Die erste virtuelle Tabelle ist die unten dargestellte Basistabelle mit dem Namen „ExampleTable“. Die Basistabelle enthält alle Daten der ursprünglichen Tabelle, aber die Daten aus den Arrays wurden ausgelassen und werden in den virtuellen Tabellen erweitert.

\_id | Customer Name | Service Level
--- | ------------- | -------------
1111 | ABC | Silber
2222 | XYZ | Gold

Die folgenden Tabellen enthalten die virtuellen Tabellen, die die ursprünglichen Arrays im Beispiel darstellen. Alle Tabellen enthalten Folgendes:

- Einen Verweis zurück auf die ursprüngliche Primärschlüsselspalte, die der Zeile des ursprünglichen Arrays entspricht (über die Spalte „\_id“)
- Einen Hinweis auf die Position der Daten im ursprünglichen Array
- Die erweiterten Daten für jedes Element innerhalb des Arrays

Tabelle „ExampleTable\_Invoices“:

\_id | ExampleTable\_Invoices\_dim1\_idx | invoice\_id | item | price | Discount
--- | -------------- | ---------- | ---- | ----- | --------
1111 | 0 | 123 | toaster | 456 | 0,2
1111 | 1 | 124 | oven | 1235 | 0,2
2222 | 0 | 135 | fridge | 12543 | 0\.0

Tabelle „ExampleTable\_Ratings“:

\_id | ExampleTable\_Ratings\_dim1\_idx | ExampleTable\_Ratings
--- | ------------- | -------------
1111 | 0 | 5
1111 | 1 | 6
2222 | 0 | 1
2222 | 1 | 2

[AZURE.INCLUDE [data-factory-column-mapping](../../includes/data-factory-column-mapping.md)]

[AZURE.INCLUDE [data-factory-structure-for-rectangular-datasets](../../includes/data-factory-structure-for-rectangualr-datasets.md)]

## Leistung und Optimierung  
Im Artikel [Handbuch zur Leistung und Optimierung der Kopieraktivität](data-factory-copy-activity-performance.md) werden wichtige Faktoren beschrieben, die sich auf die Leistung der Datenverschiebung (Kopieraktivität) in Azure Data Factory auswirken, sowie verschiedene Möglichkeiten zur Leistungsoptimierung.

## Nächste Schritte
Im Artikel [Verschieben von Daten zwischen lokalen Quellen und der Cloud](data-factory-move-data-between-onprem-and-cloud.md) finden Sie schrittweise Anleitungen zum Erstellen einer Datenpipeline für das Verschieben von Daten aus einem lokalen Datenspeicher in einen Azure-Datenspeicher.

<!---HONumber=AcomDC_0817_2016-->