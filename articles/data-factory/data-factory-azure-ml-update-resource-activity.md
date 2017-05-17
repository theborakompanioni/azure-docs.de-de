---
title: Aktualisieren von Machine Learning-Modellen mithilfe von Azure Data Factory | Microsoft Docs
description: Beschreibt das Erstellen von Vorhersagepipelines mithilfe von Azure Data Factory und Azure Machine Learning
services: data-factory
documentationcenter: 
author: sharonlo101
manager: jhubbard
editor: monicar
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/16/2017
ms.author: shlo
ms.translationtype: Human Translation
ms.sourcegitcommit: c1cd1450d5921cf51f720017b746ff9498e85537
ms.openlocfilehash: f78c735ec9a0bb34921e2ce03a4b96478480bcd0
ms.contentlocale: de-de
ms.lasthandoff: 03/14/2017


---
# <a name="updating-azure-machine-learning-models-using-update-resource-activity"></a>Aktualisieren von Azure Machine Learning-Modellen mithilfe der Ressourcenaktualisierungsaktivität

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

Dieser Artikel stellt eine Ergänzung zum Hauptartikel zur Integration von Azure Data Factory und Azure Machine Learning dar: [Erstellen von Vorhersagepipelines mithilfe von Azure Machine Learning und Azure Data Factory](data-factory-azure-ml-batch-execution-activity.md). Wenn Sie dies noch nicht getan haben, lesen Sie zunächst den Hauptartikel, bevor Sie diesen Artikel lesen. 

## <a name="overview"></a>Übersicht
Im Laufe der Zeit müssen die Vorhersagemodelle in den Azure ML-Bewertungsexperimenten mit neuen Eingabedatasets neu trainiert werden. Wenn Sie mit dem erneuten Trainieren fertig sind, sollten Sie den Bewertungswebdienst mit dem neu trainierten ML-Modell aktualisieren. Typische Schritte, um das erneute Trainieren und das Aktualisieren von Azure ML-Modellen über Webdienste zu ermöglichen:

1. Erstellen Sie ein Experiment in [Azure ML Studio](https://studio.azureml.net).
2. Wenn Sie mit dem Modell zufrieden sind, verwenden Sie Azure ML Studio, um Webdienste für das **Trainingsexperiment** und das Bewertungs-/**Vorhersageexperiment** zu veröffentlichen.

In der folgenden Tabelle werden die in diesem Beispiel verwendeten Webdienste beschrieben.  Details finden Sie unter [Programmgesteuertes erneutes Trainieren von Machine Learning-Modellen](../machine-learning/machine-learning-retrain-models-programmatically.md) .

- **Trainingswebdienst**: Empfängt Trainingsdaten und erzeugt trainierte Modelle. Die Ausgabe des erneuten Trainierens ist eine ILEARNER-Datei in einem Azure-Blobspeicher. Der **Standardendpunkt** wird automatisch erstellt, wenn Sie das Trainingsexperiment als Webdienst veröffentlichen. Sie können weitere Endpunkte erstellen, aber im Beispiel wird nur der Standardendpunkt verwendet.
- **Bewertungswebdienst**: Empfängt Datenbeispiele ohne Bezeichnung und macht Vorhersagen. Die Ausgabe der Vorhersage kann verschiedene Formen aufweisen, z. B. eine CSV-Datei oder Zeilen in einer Azure SQL-Datenbank, dies ist abhängig von der Konfiguration des Experiments. Der Standardendpunkt wird automatisch erstellt, wenn Sie das Vorhersageexperiment als Webdienst veröffentlichen. 

Die folgende Abbildung zeigt die Beziehung zwischen Trainings- und Bewertungsendpunkten in Azure ML.

![WEB SERVICES](./media/data-factory-azure-ml-batch-execution-activity/web-services.png)

Sie können die **Azure ML-Batchausführungsaktivität** verwenden, um den **Trainingswebdienst** aufzurufen. Das Aufrufen eines Trainingswebdiensts entspricht dem Aufrufen eines Azure ML-Webdiensts (Bewertungswebdienst) für Bewertungsdaten. In den oben aufgeführten Abschnitten wird detailliert beschrieben, wie ein Azure ML-Webdienst über eine Azure Data Factory-Pipeline aufgerufen wird. 

Sie können den **Bewertungswebdienst** aufrufen, indem Sie die **Azure ML-Ressourcenaktualisierungsaktivität** verwenden, um den Webdienst mit dem neu trainierten Modell zu aktualisieren. Die folgenden Beispiele enthalten Definitionen von verknüpften Diensten: 

## <a name="scoring-web-service-is-a-classic-web-service"></a>Der Bewertungswebdienst ist ein klassischer Webdienst.
Wenn der Bewertungswebdienst ein **klassischer Webdienst** ist, erstellen Sie den zweiten **nicht standardmäßigen und aktualisierbaren Endpunkt** mithilfe des [Azure-Portals](https://manage.windowsazure.com). Die erforderlichen Schritte finden Sie im Artikel [Erstellen von Endpunkten](../machine-learning/machine-learning-create-endpoint.md). Führen Sie folgende Schritte aus, nachdem Sie den nicht standardmäßigen aktualisierbaren Endpunkt erstellt haben:

* Klicken Sie auf **BATCHAUSFÜHRUNG**, um den URI-Wert für die **mlEndpoint**-JSON-Eigenschaft zu erhalten.
* Klicken Sie auf den Link **RESSOURCE AKTUALISIEREN**, um den URI-Wert für die **updateResourceEndpoint**-JSON-Eigenschaft abzurufen. Den API-Schlüssel finden Sie auf der Seite des Endpunkts (unten rechts).

![Aktualisierbarer Endpunkt](./media/data-factory-azure-ml-batch-execution-activity/updatable-endpoint.png)

Das folgende Beispiel enthält eine JSON-Beispieldefinition für den mit AzureML verknüpften Dienst. Der verknüpfte Dienst verwendet die apiKey-Variable für die Authentifizierung.  

```json
{
    "name": "updatableScoringEndpoint2",
    "properties": {
        "type": "AzureML",
        "typeProperties": {
            "mlEndpoint": "https://ussouthcentral.services.azureml.net/workspaces/xxx/services/--scoring experiment--/jobs",
            "apiKey": "endpoint2Key",
            "updateResourceEndpoint": "https://management.azureml.net/workspaces/xxx/webservices/--scoring experiment--/endpoints/endpoint2"
        }
    }
}
```

## <a name="scoring-web-service-is-azure-resource-manager-web-service"></a>Der Bewertungswebdienst ist ein Azure Resource Manager-Webdienst. 
Wenn der Webdienst der neue Webdiensttyp ist, der einen Azure Resource Manager-Endpunkt verfügbar macht, müssen Sie den zweiten **nicht standardmäßigen** Endpunkt nicht hinzufügen. Das **updateResourceEndpoint**-Element im verknüpften Dienst hat das Format: 

```
https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resource-group-name}/providers/Microsoft.MachineLearning/webServices/{web-service-name}?api-version=2016-05-01-preview. 
```

Beim Abfragen des Webdiensts im [Azure Machine Learning Web Services-Portal](https://services.azureml.net/) können Sie Werte für Platzhalter in der URL abrufen. Der neue Typ der Ressourcenendpunktaktualisierung erfordert ein AAD (Azure Active Directory)-Token. Geben Sie **servicePrincipalId** und **servicePrincipalKey** im verknüpften AzureML-Dienst an. Informationen dazu finden Sie im Artikel zum [Erstellen eines Dienstprinzipals und Zuweisen von Berechtigungen zum Verwalten einer Azure-Ressource](../azure-resource-manager/resource-group-create-service-principal-portal.md). Hier sehen Sie ein Beispiel für eine Definition des verknüpften AzureML-Diensts: 

```json
{
    "name": "AzureMLLinkedService",
    "properties": {
        "type": "AzureML",
        "description": "The linked service for AML web service.",
        "typeProperties": {
            "mlEndpoint": "https://ussouthcentral.services.azureml.net/workspaces/0000000000000000000000000000000000000/services/0000000000000000000000000000000000000/jobs?api-version=2.0",
            "apiKey": "xxxxxxxxxxxx",
            "updateResourceEndpoint": "https://management.azure.com/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myRG/providers/Microsoft.MachineLearning/webServices/myWebService?api-version=2016-05-01-preview",
            "servicePrincipalId": "000000000-0000-0000-0000-0000000000000",
            "servicePrincipalKey": "xxxxx",
            "tenant": "mycompany.com"
        }
    }
}
```

Das folgende Szenario enthält weitere Details hierzu. Es enthält ein Beispiel für das erneute Trainieren und Aktualisieren von Azure ML-Modellen aus einer Azure Data Factory-Pipeline.

## <a name="scenario-retraining-and-updating-an-azure-ml-model"></a>Szenario: Erneutes Trainieren und Aktualisieren eines Azure ML-Modells
Dieser Abschnitt enthält eine Beispielpipeline, bei der die **Azure ML-Batchausführungsaktivität** zum erneuten Trainieren des Modells verwendet wird. Für die Pipeline wird außerdem die **Azure ML-Aktivität zum Aktualisieren von Ressourcen** verwendet, um das Modell im Bewertungswebdienst zu aktualisieren. Darüber hinaus enthält der Abschnitt JSON-Codeausschnitte für alle verknüpften Dienste, Datasets und Pipelines im Beispiel.

Im Folgenden ist die Diagrammansicht der Beispielpipeline dargestellt. Wie Sie sehen können, übernimmt die Azure ML-Batchausführungsaktivität die Trainingseingabe und erzeugt eine Trainingsausgabe (iLearner-Datei). Die Azure ML-Ressourcenaktualisierungsaktivität verwendet die Trainingsausgabe und aktualisiert das Modell im Bewertungswebdienst-Endpunkt. Die Ressourcenaktualisierungsaktivität erzeugt keine Ausgabe. „placeholderBlob“ ist nur ein Platzhalter für ein Ausgabedataset, das für den Azure Data Factory-Dienst zum Ausführen der Pipeline erforderlich ist.

![Pipelinediagramm](./media/data-factory-azure-ml-batch-execution-activity/update-activity-pipeline-diagram.png)

### <a name="azure-blob-storage-linked-service"></a>Mit Azure-Blobspeicher verknüpfter Dienst:
Der Azure-Speicher enthält die folgenden Daten:

* Trainingsdaten: Die Eingabedaten für den Azure ML-Trainingswebdienst.  
* iLearner-Datei: Die Ausgabe des Azure ML-Trainingswebdiensts. Die Datei dient darüber hinaus als Eingabe für die Aktivität zur Ressourcenaktualisierung.  

Dies ist die JSON-Beispieldefinition des verknüpften-Diensts:

```JSON
{
    "name": "StorageLinkedService",
      "properties": {
        "type": "AzureStorage",
        "typeProperties": {
            "connectionString": "DefaultEndpointsProtocol=https;AccountName=name;AccountKey=key"
        }
    }
}
```

### <a name="training-input-dataset"></a>Trainingseingabedataset:
Das folgende Dataset stellt die Trainingseingabedaten für den Azure ML-Trainingswebdienst dar. Die Azure ML-Batchausführungsaktivität verwendet dieses Dataset als Eingabe.

```JSON
{
    "name": "trainingData",
    "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "StorageLinkedService",
        "typeProperties": {
            "folderPath": "labeledexamples",
            "fileName": "labeledexamples.arff",
            "format": {
                "type": "TextFormat"
            }
        },
        "availability": {
            "frequency": "Week",
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

### <a name="training-output-dataset"></a>Trainingsausgabedataset:
Das folgende Dataset stellt die iLearner-Ausgabedatei des Azure ML-Trainingswebdiensts dar. Die Azure ML-Batchausführungsaktivität erzeugt dieses Dataset. Dieses Dataset ist darüber hinaus die Eingabe für die Azure ML-Ressourcenaktualisierungsaktivität.

```JSON
{
    "name": "trainedModelBlob",
    "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "StorageLinkedService",
        "typeProperties": {
            "folderPath": "trainingoutput",
            "fileName": "model.ilearner",
            "format": {
                "type": "TextFormat"
            }
        },
        "availability": {
            "frequency": "Week",
            "interval": 1
        }
    }
}
```

### <a name="linked-service-for-azure-ml-training-endpoint"></a>Verknüpfter Dienst für den Azure ML-Trainingsendpunkt
Der folgende JSON-Codeausschnitt definiert einen mit Azure Machine Learning verknüpften Dienst, der auf den Standardendpunkt des Trainingswebdiensts verweist.

```JSON
{    
    "name": "trainingEndpoint",
      "properties": {
        "type": "AzureML",
        "typeProperties": {
            "mlEndpoint": "https://ussouthcentral.services.azureml.net/workspaces/xxx/services/--training experiment--/jobs",
              "apiKey": "myKey"
        }
      }
}
```

Führen Sie in **Azure ML Studio** folgende Schritte aus, um Werte für **mlEndpoint** und **apiKey** abzurufen:

1. Klicken Sie im linken Menü auf **WEB SERVICES** .
2. Klicken Sie in der Liste der Webdienste auf den **Trainingswebdienst** .
3. Klicken Sie neben dem Textfeld **API-Schlüssel** auf „Kopieren“. Fügen Sie den Schlüssel aus der Zwischenablage in den Data Factory-JSON-Editor ein.
4. Klicken Sie in **Azure ML Studio** auf den Link **BATCHAUSFÜHRUNG**.
5. Kopieren Sie den **Anforderungs-URI** im Abschnitt **Anforderung**, und fügen Sie ihn in den Data Factory-JSON-Editor ein.   

### <a name="linked-service-for-azure-ml-updatable-scoring-endpoint"></a>Verknüpfter Dienst für den aktualisierbaren Azure ML-Bewertungsendpunkt
Der folgende JSON-Codeausschnitt definiert einen mit Azure Machine Learning verknüpften Dienst, der auf den nicht standardmäßigen aktualisierbaren Endpunkt des Bewertungswebdiensts verweist.  

```JSON
{
    "name": "updatableScoringEndpoint2",
    "properties": {
        "type": "AzureML",
        "typeProperties": {
            "mlEndpoint": "https://ussouthcentral.services.azureml.net/workspaces/00000000eb0abe4d6bbb1d7886062747d7/services/00000000026734a5889e02fbb1f65cefd/jobs?api-version=2.0",
            "apiKey": "sooooooooooh3WvG1hBfKS2BNNcfwSO7hhY6dY98noLfOdqQydYDIXyf2KoIaN3JpALu/AKtflHWMOCuicm/Q==",
            "updateResourceEndpoint": "https://management.azure.com/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/Default-MachineLearning-SouthCentralUS/providers/Microsoft.MachineLearning/webServices/myWebService?api-version=2016-05-01-preview",
            "servicePrincipalId": "fe200044-c008-4008-a005-94000000731",
            "servicePrincipalKey": "zWa0000000000Tp6FjtZOspK/WMA2tQ08c8U+gZRBlw=",
            "tenant": "mycompany.com"
        }
    }
}
```

### <a name="placeholder-output-dataset"></a>Platzhalter-Ausgabedataset:
Die Azure ML-Aktivität zur Ressourcenaktualisierung generiert keine Ausgabe. Für Azure Data Factory ist aber ein Ausgabedataset für den Zeitplan einer Pipeline erforderlich. Aus diesem Grund verwenden wir in diesem Beispiel ein Dummy- bzw. Platzhalter-Dataset.  

```JSON
{
    "name": "placeholderBlob",
    "properties": {
        "availability": {
            "frequency": "Week",
            "interval": 1
        },
        "type": "AzureBlob",
        "linkedServiceName": "StorageLinkedService",
        "typeProperties": {
            "folderPath": "any",
            "format": {
                "type": "TextFormat"
            }
        }
    }
}
```

### <a name="pipeline"></a>Pipeline
Die Pipeline weist zwei Aktivitäten auf: **AzureMLBatchExecution** und **AzureMLUpdateResource**. Die Azure ML-Batchausführungsaktivität verwendet die Trainingsdaten als Eingabe und erzeugt eine iLearner-Datei als Ausgabe. Die Aktivität ruft den Trainingswebdienst (das als Webdienst bereitgestellte Trainingsexperiment) mit den Trainingseingabedaten auf und empfängt die iLearner-Datei vom Webdienst. „placeholderBlob“ ist nur ein Platzhalter für ein Ausgabedataset, das für den Azure Data Factory-Dienst zum Ausführen der Pipeline erforderlich ist.

![Pipelinediagramm](./media/data-factory-azure-ml-batch-execution-activity/update-activity-pipeline-diagram.png)

```JSON
{
    "name": "pipeline",
    "properties": {
        "activities": [
            {
                "name": "retraining",
                "type": "AzureMLBatchExecution",
                "inputs": [
                    {
                        "name": "trainingData"
                    }
                ],
                "outputs": [
                    {
                        "name": "trainedModelBlob"
                    }
                ],
                "typeProperties": {
                    "webServiceInput": "trainingData",
                    "webServiceOutputs": {
                        "output1": "trainedModelBlob"
                    }              
                 },
                "linkedServiceName": "trainingEndpoint",
                "policy": {
                    "concurrency": 1,
                    "executionPriorityOrder": "NewestFirst",
                    "retry": 1,
                    "timeout": "02:00:00"
                }
            },
            {
                "type": "AzureMLUpdateResource",
                "typeProperties": {
                    "trainedModelName": "Training Exp for ADF ML [trained model]",
                    "trainedModelDatasetName" :  "trainedModelBlob"
                },
                "inputs": [
                    {
                        "name": "trainedModelBlob"
                    }
                ],
                "outputs": [
                    {
                        "name": "placeholderBlob"
                    }
                ],
                "policy": {
                    "timeout": "01:00:00",
                    "concurrency": 1,
                    "retry": 3
                },
                "name": "AzureML Update Resource",
                "linkedServiceName": "updatableScoringEndpoint2"
            }
        ],
        "start": "2016-02-13T00:00:00Z",
           "end": "2016-02-14T00:00:00Z"
    }
}
```

