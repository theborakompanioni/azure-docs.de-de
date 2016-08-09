<properties 
	pageTitle="Verschieben von Daten aus Salesforce mit Data Factory | Microsoft Azure " 
	description="Erfahren Sie, wie Sie Daten mithilfe von Azure Data Factory aus Salesforce verschieben." 
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
	ms.date="07/07/2016" 
	ms.author="spelluru"/>

# Verschieben von Daten aus Salesforce mithilfe von Azure Data Factory
Dieser Artikel erläutert die Verwendung der Kopieraktivität in einer Azure Data Factory, um Daten aus Salesforce in einen im Abschnitt [Unterstützte Quellen und Senken](data-factory-data-movement-activities.md#supported-data-stores) in der Spalte „Senke“ aufgeführten Datenspeicher zu kopieren. Dieser Artikel baut auf dem Artikel [Datenverschiebungsaktivitäten](data-factory-data-movement-activities.md) auf, der eine allgemeine Übersicht zur Datenverschiebung mit Kopieraktivität und unterstützten Datenspeicherkombinationen bietet.

Data Factory unterstützt derzeit nur das Verschieben von Daten aus Salesforce in [Unterstützte Senkendatenspeicher]((data-factory-data-movement-activities.md#supported-data-stores), aber nicht das Verschieben von Daten aus anderen Datenspeichern nach Salesforce.

## Voraussetzungen
- Sie müssen eine der folgenden Editionen verwenden: Developer Edition, Professional Edition, Enterprise Edition oder Unlimited Edition.
- API-Berechtigungen müssen aktiviert sein. Informationen hierzu finden Sie unter [How do I enable API access in Salesforce by permission set?](https://www.data2crm.com/migration/faqs/enable-api-access-salesforce-permission-set/) (Aktivieren des API-Zugriffs in Salesforce per Berechtigungssatz).
- Um Daten aus Salesforce in lokale Datenspeicher zu kopieren, muss in Ihrer lokalen Umgebung das Datenverwaltungsgateway Version 2.0 oder höher installiert sein.

## Assistent zum Kopieren von Daten
Die einfachste Art, eine Pipeline zu erstellen, die Daten aus Salesforce in einen der unterstützten Senkendatenspeicher kopiert, ist die Verwendung des Assistenten zum Kopieren von Daten. Unter [Tutorial: Erstellen einer Pipeline mit Kopieraktivität mithilfe des Data Factory-Kopier-Assistenten](data-factory-copy-data-wizard-tutorial.md) finden Sie eine kurze exemplarische Vorgehensweise zum Erstellen einer Pipeline mithilfe des Assistenten zum Kopieren von Daten.

Die folgenden Beispiele zeigen JSON-Beispieldefinitionen, die Sie zum Erstellen einer Pipeline mit dem [Azure-Portal](data-factory-copy-activity-tutorial-using-azure-portal.md), mit [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) oder über [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md) verwenden können.

## Beispiel: Kopieren von Daten aus Salesforce in einen Azure-Blob
In diesem Beispiel werden stündlich Daten aus Salesforce in ein Azure-Blob kopiert. Die bei diesen Beispielen verwendeten JSON-Eigenschaften werden in den Abschnitten beschrieben, die auf die Beispiele folgen. Daten können mithilfe der Kopieraktivität in Azure Data Factory direkt in die im Artikel [Datenverschiebungsaktivitäten](data-factory-data-movement-activities.md#supported-data-stores) aufgeführten Senken kopiert werden.

- Ein verknüpfter Dienst des Typs [Salesforce](#salesforce-linked-service-properties)
- Ein verknüpfter Dienst des Typs [AzureStorage](data-factory-azure-blob-connector.md#azure-storage-linked-service-properties)
- Ein [Eingabedataset](data-factory-create-datasets.md) des Typs [RelationalTable](#salesforce-dataset-properties)
- Ein [Ausgabedataset](data-factory-create-datasets.md) des Typs [AzureBlob](data-factory-azure-blob-connector.md#azure-blob-dataset-type-properties)
- Eine [Pipeline](data-factory-create-pipelines.md) mit Kopieraktivität, die [RelationalSource](#relationalsource-type-properties) und [BlobSink](data-factory-azure-blob-connector.md#azure-blob-copy-activity-type-properties) verwendet

**Verknüpfter Salesforce-Dienst**

Dieses Beispiel verwendet den verknüpften **Salesforce**-Dienst. Im Abschnitt [Verknüpfter Salesforce-Dienst](#salesforce-linked-service-properties) finden Sie die Eigenschaften, die von diesem verknüpften Dienst unterstützt werden. Anweisungen zum Abrufen oder Zurücksetzen des Sicherheitstokens finden Sie unter [Get security token](https://help.salesforce.com/apex/HTViewHelpDoc?id=user_security_token.htm) (Abrufen des Sicherheitstokens).

	{
		"name": "SalesforceLinkedService",
		"properties":
		{
			"type": "Salesforce",
			"typeProperties":
			{
				"username": "<user name>",
				"password": "<password>",
				"securityToken": "<security token>"
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

**Salesforce-Eingabedataset:**

	{
		"name": "SalesforceInput",
		"properties": {
			"linkedServiceName": "SalesforceLinkedService",
			"type": "RelationalTable",
			"typeProperties": {
				"tableName": "AllDataType__c"  
			},
			"availability": {
				"frequency": "Hour",
				"interval": 1
			},
			"external": true,
			"policy": {
				"externalData": {
					"retryInterval": "00:01:00",
					"retryTimeout": "00:10:00",
					"maximumRetry": 3
				}
			}
		}
	}

Durch Festlegen von **external** auf **true** wird dem Data Factory-Dienst mitgeteilt, dass das Dataset für die Data Factory extern ist und nicht durch eine Aktivität in der Data Factory erzeugt wird.

> [AZURE.IMPORTANT]  Der Abschnitt „\_\_c“ von „API Name“ wird für benutzerdefinierte Objekte benötigt.

![Data Factory – Salesforce-Verbindung – API-Name](media/data-factory-salesforce-connector/data-factory-salesforce-api-name.png)

**Azure-Blob-Ausgabedataset**

Daten werden stündlich in ein neues Blob geschrieben ("frequency": "hour", "interval": 1).

	{
	    "name": "AzureBlobOutput",
	    "properties":
	    {
	        "type": "AzureBlob",
	        "linkedServiceName": "AzureStorageLinkedService",
	        "typeProperties":
	        {
	            "folderPath": "adfgetstarted/alltypes_c"
	        },
	        "availability":
	        {
	            "frequency": "Hour",
	            "interval": 1
	        }
	    }
	}


**Pipeline mit Kopieraktivität**

Die Pipeline enthält eine Kopieraktivität, die für das Verwenden der oben genannten Ein- und Ausgabedatasets und für eine stündliche Ausführung konfiguriert ist. In der JSON-Definition der Pipeline ist der Typ **source** auf **RelationalSource** und der Typ **sink** auf **BlobSink** festgelegt.

Unter [RelationalSource-Typeigenschaften](#relationalsource-type-properties) finden Sie die Liste der Eigenschaften, die von RelationalSource unterstützt werden.
	
	{  
		"name":"SamplePipeline",
		"properties":{  
			"start":"2016-06-01T18:00:00",
			"end":"2016-06-01T19:00:00",
			"description":"pipeline with copy activity",
			"activities":[  
			{
				"name": "SaleforceToAzureBlob",
				"description": "Copy from Salesforce to an Azure blob",
				"type": "Copy",
				"inputs": [
				{
					"name": "SalesforceInput"
				}
				],
				"outputs": [
				{
					"name": "AzureBlobOutput"
				}
				],
				"typeProperties": {
					"source": {
						"type": "RelationalSource",
						"query": "SELECT Id, Col_AutoNumber__c, Col_Checkbox__c, Col_Currency__c, Col_Date__c, Col_DateTime__c, Col_Email__c, Col_Number__c, Col_Percent__c, Col_Phone__c, Col_Picklist__c, Col_Picklist_MultiSelect__c, Col_Text__c, Col_Text_Area__c, Col_Text_AreaLong__c, Col_Text_AreaRich__c, Col_URL__c, Col_Text_Encrypt__c, Col_Lookup__c FROM AllDataType__c"				
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

> [AZURE.IMPORTANT]  Der Abschnitt „\_\_c“ von „API Name“ wird für benutzerdefinierte Objekte benötigt.

![Data Factory – Salesforce-Verbindung – API-Name](media/data-factory-salesforce-connector/data-factory-salesforce-api-name-2.png)

## Eigenschaften des verknüpften Salesforce-Diensts

Die folgende Tabelle enthält eine Beschreibung der JSON-Elemente, die für den verknüpften Salesforce-Dienst spezifisch sind.

| Eigenschaft | Beschreibung | Erforderlich |
| -------- | ----------- | -------- | 
| Typ | Die type-Eigenschaft muss auf **Salesforce** festgelegt sein. | Ja | 
| username |Geben Sie einen Benutzernamen für das Benutzerkonto an. | Ja |
| password | Geben Sie ein Kennwort für das Benutzerkonto an. | Ja |
| securityToken | Geben Sie ein Sicherheitstoken für das Benutzerkonto an. Anweisungen zum Abrufen oder Zurücksetzen des Sicherheitstokens finden Sie unter [Get security token](https://help.salesforce.com/apex/HTViewHelpDoc?id=user_security_token.htm) (Abrufen des Sicherheitstokens). Allgemeine Informationen zu Sicherheitstoken finden Sie unter [Security and the API](https://developer.salesforce.com/docs/atlas.de-DE.api.meta/api/sforce_api_concepts_security.htm) (Sicherheit und die API). | Ja | 

## Salesforce-Dataseteigenschaften

Eine vollständige Liste der Abschnitte und Eigenschaften, die zum Definieren von Datasets zur Verfügung stehen, finden Sie im Artikel [Erstellen von Datasets](data-factory-create-datasets.md). Abschnitte wie "structure", "availability" und "policy" des JSON-Codes eines Datasets sind bei allen Typen von Datasets (Azure SQL, Azure-Blob, Azure-Tabelle usw.) ähnlich.

Der Abschnitt **typeProperties** unterscheidet sich bei jedem Typ von Dataset und bietet Informationen zum Speicherort der Daten im Datenspeicher. Der Abschnitt „typeProperties“ für ein Dataset vom Typ **RelationalTable** hat die folgenden Eigenschaften.

| Eigenschaft | Beschreibung | Erforderlich |
| -------- | ----------- | -------- |
| tableName | Name der Tabelle in Salesforce. | Nein (wenn **query** von **RelationalSource** angegeben ist) | 

> [AZURE.IMPORTANT]  Der Abschnitt „\_\_c“ von „API Name“ wird für benutzerdefinierte Objekte benötigt.

![Data Factory – Salesforce-Verbindung – API-Name](media/data-factory-salesforce-connector/data-factory-salesforce-api-name.png)

## RelationalSource-Typeigenschaften
Eine vollständige Liste der Abschnitte und Eigenschaften zum Definieren von Aktivitäten finden Sie im Artikel [Erstellen von Pipelines](data-factory-create-pipelines.md). Eigenschaften wie Name, Beschreibung, Eingabe- und Ausgabetabellen, verschiedene Richtlinien usw. sind für alle Arten von Aktivitäten verfügbar.

Im Abschnitt "typeProperties" der Aktivität verfügbare Eigenschaften variieren hingegen bei jedem Aktivitätstyp. Bei der Kopieraktivität variieren sie je nach Typ der Quellen und Senken.

Wenn bei der Kopieraktivität „source“ den Typ **RelationalSource** aufweist (zu dem Salesforce gehört), sind im typeProperties-Abschnitt die folgenden Eigenschaften verfügbar:

| Eigenschaft | Beschreibung | Zulässige Werte | Erforderlich |
| -------- | ----------- | -------------- | -------- |
| query | Verwendet die benutzerdefinierte Abfrage zum Lesen von Daten. | SQL-92-Abfrage oder Abfrage vom Typ [Salesforce Object Query Language (SOQL)](https://developer.salesforce.com/docs/atlas.de-DE.soql_sosl.meta/soql_sosl/sforce_api_calls_soql.htm). Beispiel: select * from MyTable\_\_c. | Nein (wenn **tableName** von **Dataset** angegeben ist) |

> [AZURE.IMPORTANT]  Der Abschnitt „\_\_c“ von „API Name“ wird für benutzerdefinierte Objekte benötigt. <br><br> Verwenden Sie beim Angeben einer Abfrage mit einer WHERE-Klausel für die „datetime“-Spalte eine SQL-Abfrage wie beispielsweise: $$Text.Format('SELECT Id, Name, BillingCity FROM Account WHERE LastModifiedDate >= {0:yyyy-MM-ddTHH:mm:ssZ} AND LastModifiedDate < {1:yyyy-MM-ddTHH:mm:ssZ}', WindowStart, WindowEnd), oder eine SQL-Abfrage wie beispielsweise: $$Text.Format('SELECT * FROM Account WHERE LastModifiedDate >= {{ts'{0:yyyy-MM-dd HH:mm:ss}'}} AND LastModifiedDate < {{ts'{1:yyyy-MM-dd HH:mm:ss}'}}', WindowStart, WindowEnd).

![Data Factory – Salesforce-Verbindung – API-Name](media/data-factory-salesforce-connector/data-factory-salesforce-api-name-2.png)

## Abrufen von Daten aus Salesforce-Bericht
Sie können Daten aus Salesforce-Berichten abrufen, indem Sie die Abfrage als {call "< Berichtsname >"} angeben, z. B. "query": "{call "TestReport"}".

## Anforderungslimits in Salesforce
Salesforce weist Grenzwerte sowohl für die Gesamtanzahl von API-Anforderungen als auch für die Anzahl gleichzeitiger API-Anforderungen auf. Weitere Informationen finden Sie im Abschnitt **API Request Limits** (API-Anforderungslimits) im Artikel [Salesforce API Request Limits](http://resources.docs.salesforce.com/200/20/de-DE/sfdc/pdf/salesforce_app_limits_cheatsheet.pdf) (Salesforce-API-Anforderungslimits).
 
Wenn die Anzahl von gleichzeitigen Anforderungen das Limit überschreitet, setzt eine Drosselung ein, und es werden zufällig generierte Fehler angezeigt. Wenn die Gesamtanzahl von Anforderungen das Limit überschreitet, wird das Salesforce-Konto 24 Stunden lang gesperrt. In beiden Szenarien erhalten Sie möglicherweise auch den Fehler „REQUEST\_LIMIT\_EXCEEDED“.
 

[AZURE.INCLUDE [data-factory-structure-for-rectangular-datasets](../../includes/data-factory-structure-for-rectangualr-datasets.md)]

### Typzuordnung für Salesforce
Salesforce-Typ | .Net-basierter Typ
--------------- | ---------------
Auto Number | String
Kontrollkästchen | Boolean
Currency | Doppelt
Datum | DateTime
Datum/Uhrzeit | DateTime
E-Mail | String
ID | String
Lookup Relationship | String
Multi-Select Picklist | String
Number | Doppelt
Prozent | Doppelt
Phone | String
Picklist | String
Text | String
Text Area | String
Text Area (Long) | String
Text Area (Rich) | String
Text (Encrypted) | String
URL | String

[AZURE.INCLUDE [data-factory-column-mapping](../../includes/data-factory-column-mapping.md)]
[AZURE.INCLUDE [data-factory-structure-for-rectangular-datasets](../../includes/data-factory-structure-for-rectangualr-datasets.md)]

## Leistung und Optimierung  
Der Artikel [Handbuch zur Leistung und Optimierung der Kopieraktivität](data-factory-copy-activity-performance.md) beschreibt wichtige Faktoren, die sich auf die Leistung der Datenverschiebung (Kopieraktivität) in Azure Data Factory auswirken, sowie verschiedene Möglichkeiten zur Leistungsoptimierung.

<!---HONumber=AcomDC_0727_2016-->