---
title: "Transformieren von Daten mit dem U-SQL-Skript – Azure | Microsoft-Dokumentation"
description: "Erläutert die Datenverarbeitung oder -transformation durch Ausführen von U-SQL-Skripts für einen Azure Data Lake Analytics-Computedienst."
services: data-factory
documentationcenter: 
author: spelluru
manager: jhubbard
editor: monicar
ms.assetid: e17c1255-62c2-4e2e-bb60-d25274903e80
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/10/2017
ms.author: spelluru
ms.translationtype: Human Translation
ms.sourcegitcommit: 71fea4a41b2e3a60f2f610609a14372e678b7ec4
ms.openlocfilehash: 88628fb2c07ad72c646f7e3ed076e7a4b1519200
ms.contentlocale: de-de
ms.lasthandoff: 07/06/2017


---
<a id="transform-data-by-running-u-sql-scripts-on-azure-data-lake-analytics" class="xliff"></a>
# Transformieren von Daten durch Ausführen von U-SQL-Skripts für Azure Data Lake Analytics 
> [!div class="op_single_selector" title1="Transformation Activities"]
> * [Hive-Aktivität](data-factory-hive-activity.md) 
> * [Pig-Aktivität](data-factory-pig-activity.md)
> * [MapReduce-Aktivität](data-factory-map-reduce.md)
> * [Hadoop-Streamingaktivität](data-factory-hadoop-streaming-activity.md)
> * [Spark-Aktivität](data-factory-spark.md)
> * [Machine Learning-Batchausführungsaktivität](data-factory-azure-ml-batch-execution-activity.md)
> * [Machine Learning-Ressourcenaktualisierungsaktivität](data-factory-azure-ml-update-resource-activity.md)
> * [Aktivität „Gespeicherte Prozedur“](data-factory-stored-proc-activity.md)
> * [U-SQL-Aktivität für Data Lake Analytics](data-factory-usql-activity.md)
> * [Benutzerdefinierte .NET-Aktivität](data-factory-use-custom-activities.md)

Eine Pipeline in einer Azure Data Factory verarbeitet Daten in verknüpften Speicherdiensten mithilfe verknüpfter Compute Services. Sie enthält eine Abfolge von Aktivitäten, wobei jede Aktivität einen bestimmten Verarbeitungsvorgang ausführt. In diesem Kapitel wird die **U-SQL-Aktivität von Data Lake Analytics** beschrieben, die ein **U-SQL**-Skript auf einem mit **Azure Data Lake Analytics** verknüpften Computedienst ausführt. 

> [!NOTE]
> Erstellen Sie ein Azure Data Lake Analytics-Konto, bevor Sie mit einer U-SQL-Aktivität von Data Lake Analytics eine Pipeline erstellen. Weitere Informationen zu Azure Data Lake Analytics finden Sie unter [Erste Schritte mit Azure Data Lake Analytics](../data-lake-analytics/data-lake-analytics-get-started-portal.md).
> 
> Im Tutorial [Erstellen Ihrer ersten Pipeline](data-factory-build-your-first-pipeline.md) finden Sie ausführliche Anweisungen zum Erstellen von Data Factorys, verknüpften Diensten, Datasets und Pipelines. Verwenden Sie JSON-Codeausschnitte mit Data Factory-Editor, Visual Studio oder Azure PowerShell, um Data Factory-Entitäten zu erstellen.


<a id="azure-data-lake-analytics-linked-service" class="xliff"></a>
## Mit Azure Data Lake Analytics verknüpfter Dienst
Sie erstellen einen mit **Azure Data Lake Analytics** verknüpften Dienst, um einen Azure Data Lake Analytics-Computedienst mit einer Azure Data Factory zu verknüpfen. Die Data Lake Analytics-U-SQL-Aktivität in der Pipeline verweist auf diesen verknüpften Dienst. 

Das folgende Beispiel enthält eine JSON-Definition für einen mit Azure Data Lake Analytics verknüpften Dienst. 

```JSON
{
    "name": "AzureDataLakeAnalyticsLinkedService",
    "properties": {
        "type": "AzureDataLakeAnalytics",
        "typeProperties": {
            "accountName": "adftestaccount",
            "dataLakeAnalyticsUri": "datalakeanalyticscompute.net",
            "authorization": "<authcode>",
            "sessionId": "<session ID>", 
            "subscriptionId": "<subscription id>",
            "resourceGroupName": "<resource group name>"
        }
    }
}
```

Die folgende Tabelle enthält Beschreibungen der Eigenschaften, die in der JSON-Definition verwendet werden. 

| Eigenschaft | Beschreibung | Erforderlich |
| --- | --- | --- |
| Typ |Legen Sie die Typeigenschaft auf **AzureDataLakeAnalytics**fest. |Ja |
| accountName |Name des Azure Data Lake Analytics-Kontos. |Ja |
| dataLakeAnalyticsUri |URI des Azure Data Lake Analytics-Kontos. |Nein |
| Autorisierung |Der Autorisierungscode wird automatisch abgerufen, nachdem Sie im Data Factory-Editor auf die Schaltfläche **Autorisieren** geklickt und die OAuth-Anmeldung abgeschlossen haben. |Ja |
| subscriptionId |Azure-Abonnement-ID |Nein (falls nicht angegeben, wird das Abonnement der Data Factory verwendet). |
| ResourceGroupName |Azure-Ressourcengruppenname |Nein (falls nicht angegeben, wird die Ressourcengruppe der Data Factory verwendet). |
| sessionId |Sitzungs-ID aus der OAuth-Autorisierungssitzung. Jede Sitzungs-ID ist eindeutig und darf nur einmal verwendet werden. Die Sitzungs-ID wird im Data Factory-Editor automatisch generiert. |Ja |

Der von Ihnen mithilfe der Schaltfläche **Autorisieren** generierte Autorisierungscode läuft nach einer gewissen Zeit ab. Die Zeiten bis zum Ablaufen der Autorisierungscodes für die verschiedenen Benutzerkonten finden Sie in der folgenden Tabelle. Unter Umständen wird Ihnen nach dem **Ablauf des Tokens** folgende Fehlermeldung angezeigt: „Fehler beim Anmeldevorgang: invalid_grant – AADSTS70002: Fehler beim Überprüfen der Anmeldeinformationen. AADSTS70008: Die angegebene Zugriffserteilung ist abgelaufen oder wurde widerrufen. Ablaufverfolgungs-ID: d18629e8-af88-43c5-88e3-d8419eb1fca1 Korrelations-ID: fac30a0c-6be6-4e02-8d69-a776d2ffefd7 Zeitstempel: 2015-12-15 21:09:31Z“

| Benutzertyp | Läuft ab nach |
|:--- |:--- |
| Benutzerkonten, die NICHT von Azure Active Directory verwaltet werden (@hotmail.com, @live.com usw.) |12 Stunden |
| Benutzerkonten, die von Azure Active Directory (AAD) verwaltet werden |14 Tage nach der letzten Sliceausführung. <br/><br/>90 Tage, wenn ein Slice, das auf einem verknüpften OAuth-Dienst basiert, mindestens einmal alle 14 Tage ausgeführt wird. |

Um diesen Fehler zu vermeiden oder zu beheben, autorisieren Sie sich durch Klicken auf die Schaltfläche **Autorisieren** erneut, wenn das **Token abläuft**, und stellen den verknüpften Dienst anschließend erneut bereit. Sie können auch programmgesteuert Werte für die Eigenschaften **sessionId** und **authorization** generieren. Verwenden Sie hierzu den im folgenden Abschnitt bereitgestellten Code:

<a id="to-programmatically-generate-sessionid-and-authorization-values" class="xliff"></a>
### So generieren Sie programmgesteuert Werte für „sessionId“ und „authorization“

```csharp
if (linkedService.Properties.TypeProperties is AzureDataLakeStoreLinkedService ||
    linkedService.Properties.TypeProperties is AzureDataLakeAnalyticsLinkedService)
{
    AuthorizationSessionGetResponse authorizationSession = this.Client.OAuth.Get(this.ResourceGroupName, this.DataFactoryName, linkedService.Properties.Type);

    WindowsFormsWebAuthenticationDialog authenticationDialog = new WindowsFormsWebAuthenticationDialog(null);
    string authorization = authenticationDialog.AuthenticateAAD(authorizationSession.AuthorizationSession.Endpoint, new Uri("urn:ietf:wg:oauth:2.0:oob"));

    AzureDataLakeStoreLinkedService azureDataLakeStoreProperties = linkedService.Properties.TypeProperties as AzureDataLakeStoreLinkedService;
    if (azureDataLakeStoreProperties != null)
    {
        azureDataLakeStoreProperties.SessionId = authorizationSession.AuthorizationSession.SessionId;
        azureDataLakeStoreProperties.Authorization = authorization;
    }

    AzureDataLakeAnalyticsLinkedService azureDataLakeAnalyticsProperties = linkedService.Properties.TypeProperties as AzureDataLakeAnalyticsLinkedService;
    if (azureDataLakeAnalyticsProperties != null)
    {
        azureDataLakeAnalyticsProperties.SessionId = authorizationSession.AuthorizationSession.SessionId;
        azureDataLakeAnalyticsProperties.Authorization = authorization;
    }
}
```

Nähere Informationen zu den im Code verwendeten Data Factory-Klassen finden Sie in den Themen [AzureDataLakeStoreLinkedService-Klasse](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.azuredatalakestorelinkedservice.aspx), [AzureDataLakeAnalyticsLinkedService-Klasse](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.azuredatalakeanalyticslinkedservice.aspx) und [AuthorizationSessionGetResponse-Klasse](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.authorizationsessiongetresponse.aspx). Fügen Sie für die WindowsFormsWebAuthenticationDialog-Klasse einen Verweis auf Microsoft.IdentityModel.Clients.ActiveDirectory.WindowsForms.dll hinzu. 

<a id="data-lake-analytics-u-sql-activity" class="xliff"></a>
## U-SQL-Aktivität für Data Lake Analytics
Der folgende JSON-Ausschnitt definiert eine Pipeline mit einer U-SQL-Aktivität für Data Lake Analytics. Die Aktivitätsdefinition verwendet einen Verweis auf den zuvor erstellten mit Azure Data Lake Analytics verknüpften Dienst.   

```JSON
{
    "name": "ComputeEventsByRegionPipeline",
    "properties": {
        "description": "This is a pipeline to compute events for en-gb locale and date less than 2012/02/19.",
        "activities": 
        [
            {
                "type": "DataLakeAnalyticsU-SQL",
                "typeProperties": {
                    "scriptPath": "scripts\\kona\\SearchLogProcessing.txt",
                    "scriptLinkedService": "StorageLinkedService",
                    "degreeOfParallelism": 3,
                    "priority": 100,
                    "parameters": {
                        "in": "/datalake/input/SearchLog.tsv",
                        "out": "/datalake/output/Result.tsv"
                    }
                },
                "inputs": [
                    {
                        "name": "DataLakeTable"
                    }
                ],
                "outputs": 
                [
                    {
                        "name": "EventsByRegionTable"
                    }
                ],
                "policy": {
                    "timeout": "06:00:00",
                    "concurrency": 1,
                    "executionPriorityOrder": "NewestFirst",
                    "retry": 1
                },
                "scheduler": {
                    "frequency": "Day",
                    "interval": 1
                },
                "name": "EventsByRegion",
                "linkedServiceName": "AzureDataLakeAnalyticsLinkedService"
            }
        ],
        "start": "2015-08-08T00:00:00Z",
        "end": "2015-08-08T01:00:00Z",
        "isPaused": false
    }
}
```

Die folgende Tabelle beschreibt die Namen und Eigenschaften, die für diese Aktivität spezifisch sind. 

| Eigenschaft | Beschreibung | Erforderlich |
|:--- |:--- |:--- |
| Typ |Die type-Eigenschaft muss auf **DataLakeAnalyticsU-SQL**festgelegt werden. |Ja |
| scriptPath |Der Pfad zum Ordner, der das U-SQL-Skript enthält. Beim Dateinamen wird Groß-/Kleinschreibung unterschieden. |Nein (wenn script verwendet wird) |
| scriptLinkedService |Verknüpfter Dienst, der den Speicher, der das Skript enthält, mit der Data Factory verknüpft. |Nein (wenn script verwendet wird) |
| script |Geben Sie anstelle von scriptPath und scriptLinkedService ein Inlineskript an. Beispiel: `"script": "CREATE DATABASE test"`. |Nein (wenn scriptPath and scriptLinkedService verwendet werden) |
| degreeOfParallelism |Die maximale Anzahl von Knoten, die zum Ausführen des Auftrags gleichzeitig verwendet werden. |Nein |
| priority |Bestimmt, welche der in der Warteschlange befindlichen Aufträge als erstes ausgeführt werden. Je niedriger die Zahl, desto höher die Priorität. |Nein |
| parameters |Parameter für das U-SQL-Skript |Nein |
| runtimeVersion | Die Runtime-Version des zu verwendenden U-SQL-Moduls | Nein | 
| compilationMode | <p>Der Kompilierungsmodus von U-SQL. Muss einen der folgenden Werte aufweisen:</p> <ul><li>**Semantic:** Es werden nur Semantiküberprüfungen und erforderliche Integritätsprüfungen ausgeführt.</li><li>**Full:** Es wird die vollständige Kompilierung ausgeführt, einschließlich Syntaxprüfung, Optimierung, Codegenerierung usw.</li><li>**SingleBox:** Es wird die vollständige Kompilierung ausgeführt, wobei die TargetType-Einstellung auf „SingleBox“ festgelegt ist.</li></ul><p>Wenn Sie für diese Eigenschaft keinen Wert angeben, bestimmt der Server den optimalen Kompilierungsmodus. </p>| Nein | 

Die Skriptdefinition finden Sie unter [Skriptdefinition „SearchLogProcessing.txt“](#sample-u-sql-script) . 

<a id="sample-input-and-output-datasets" class="xliff"></a>
## Eingabe- und Ausgabedatasets als Beispiel
<a id="input-dataset" class="xliff"></a>
### Eingabedataset
In diesem Beispiel befinden sich die Eingabedaten in einem Azure Data Lake-Speicher (Datei SearchLog.tsv im Ordner datalake/input). 

```JSON
{
    "name": "DataLakeTable",
    "properties": {
        "type": "AzureDataLakeStore",
        "linkedServiceName": "AzureDataLakeStoreLinkedService",
        "typeProperties": {
            "folderPath": "datalake/input/",
            "fileName": "SearchLog.tsv",
            "format": {
                "type": "TextFormat",
                "rowDelimiter": "\n",
                "columnDelimiter": "\t"
            }
        },
        "availability": {
            "frequency": "Day",
            "interval": 1
        }
    }
}    
```

<a id="output-dataset" class="xliff"></a>
### Ausgabedataset
In diesem Beispiel werden die von dem U-SQL-Skript erzeugten Ausgabedaten in einem Azure Data Lake-Speicher abgelegt (im Ordner „datalake/output“). 

```JSON
{
    "name": "EventsByRegionTable",
    "properties": {
        "type": "AzureDataLakeStore",
        "linkedServiceName": "AzureDataLakeStoreLinkedService",
        "typeProperties": {
            "folderPath": "datalake/output/"
        },
        "availability": {
            "frequency": "Day",
            "interval": 1
        }
    }
}
```

<a id="sample-data-lake-store-linked-service" class="xliff"></a>
### Beispiel für einen mit Azure Data Lake Store verknüpften Dienst
Hier finden Sie die Definition des mit Azure Data Lake Store verknüpften Beispieldiensts, der von den Eingabe-/Ausgabedatasets verwendet wird. 

```JSON
{
    "name": "AzureDataLakeStoreLinkedService",
    "properties": {
        "type": "AzureDataLakeStore",
        "typeProperties": {
            "dataLakeUri": "https://<accountname>.azuredatalakestore.net/webhdfs/v1",
            "sessionId": "<session ID>",
            "authorization": "<authorization URL>"
        }
    }
}
```

Im Artikel [Verschieben von Daten in und aus Azure Data Lake Store mithilfe von Azure Data Factory](data-factory-azure-datalake-connector.md) finden Sie Beschreibungen der JSON-Eigenschaften. 

<a id="sample-u-sql-script" class="xliff"></a>
## Beispiel-U-SQL-Skript

```
@searchlog =
    EXTRACT UserId          int,
            Start           DateTime,
            Region          string,
            Query           string,
            Duration        int?,
            Urls            string,
            ClickedUrls     string
    FROM @in
    USING Extractors.Tsv(nullEscape:"#NULL#");

@rs1 =
    SELECT Start, Region, Duration
    FROM @searchlog
WHERE Region == "en-gb";

@rs1 =
    SELECT Start, Region, Duration
    FROM @rs1
    WHERE Start <= DateTime.Parse("2012/02/19");

OUTPUT @rs1   
    TO @out
      USING Outputters.Tsv(quoting:false, dateTimeFormat:null);
```

Die Werte für die Parameter **@in** und **@out** im U-SQL-Skript werden von ADF dynamisch mithilfe des Abschnitts „parameters“ übergeben. Informationen finden Sie in der Pipelinedefinition im Abschnitt „parameters“.

Sie können in Ihrer Pipelinedefinition auch andere Eigenschaften wie etwa degreeOfParallelism oder „priority“ für die Aufträge angeben, die im Azure Data Lake Analytics-Dienst ausgeführt werden.

<a id="dynamic-parameters" class="xliff"></a>
## Dynamische Parameter
In der beispielhaften Pipelinedefinition werden die Eingabe- und Ausgabeparameter mit hartcodierten Werten zugewiesen. 

```JSON
"parameters": {
    "in": "/datalake/input/SearchLog.tsv",
    "out": "/datalake/output/Result.tsv"
}
```

Anstelle von hartcodierten Werten können dynamische Parameter verwendet werden. Beispiel: 

```JSON
"parameters": {
    "in": "$$Text.Format('/datalake/input/{0:yyyy-MM-dd HH:mm:ss}.tsv', SliceStart)",
    "out": "$$Text.Format('/datalake/output/{0:yyyy-MM-dd HH:mm:ss}.tsv', SliceStart)"
}
```

In diesem Fall werden die Eingabedateien weiterhin aus dem Ordner „/datalake/input“ abgerufen und die Ausgabedateien im Ordner „datalake/output“ generiert. Die Dateinamen sind dynamisch und basieren auf der Startzeit des Slices.  


