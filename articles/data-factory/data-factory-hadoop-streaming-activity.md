---
title: "Transformieren von Daten mit der Hadoop-Streamingaktivität – Azure | Microsoft-Dokumentation"
description: "Erfahren Sie, wie Sie die Hadoop-Streamingaktivität in Azure Data Factory verwenden können, um Daten durch die Ausführung von Hadoop-Streamingprogrammen in einem bedarfsgesteuerten oder einem eigenen HDInsight-Cluster zu transformieren."
services: data-factory
documentationcenter: 
author: sharonlo101
manager: jhubbard
editor: monicar
ms.assetid: 4c3ff8f2-2c00-434e-a416-06dfca2c41ec
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/14/2017
ms.author: shlo
ms.translationtype: Human Translation
ms.sourcegitcommit: 138f04f8e9f0a9a4f71e43e73593b03386e7e5a9
ms.openlocfilehash: 1946fba8dd94d0b2815550e3825b092c1c6289ec
ms.contentlocale: de-de
ms.lasthandoff: 06/29/2017

---
# <a name="transform-data-using-hadoop-streaming-activity-in-azure-data-factory"></a>Transformieren von Daten mit der Hadoop-Streamingaktivität in Azure Data Factory
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

Mit der Aktivität „HDInsightStreamingActivity“ können Sie einen Hadoop-Streamingauftrag über eine Azure Data Factory-Pipeline aufrufen. Der folgende JSON-Codeausschnitt zeigt die Syntax für die Verwendung von HDInsightStreamingActivity in einer JSON-Pipelinedatei. 

Die HDInsight-Streamingaktivität in einer Data Factory-[Pipeline](data-factory-create-pipelines.md) wendet Hadoop-Streamingprogramme auf [Ihren eigenen](data-factory-compute-linked-services.md#azure-hdinsight-linked-service) oder [bedarfsgesteuerten](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service) Windows-/Linux-basierten HDInsight-Cluster an. Dieser Artikel baut auf dem Artikel zu [Datentransformationsaktivitäten](data-factory-data-transformation-activities.md) auf, der eine allgemeine Übersicht über die Datentransformation und die unterstützten Transformationsaktivitäten bietet.

> [!NOTE] 
> Wenn Sie mit Azure Data Factory nicht vertraut sind, lesen Sie zunächst den Artikel [Einführung in Azure Data Factory](data-factory-introduction.md), und führen Sie das Tutorial durch: [Erstellen Ihrer ersten Pipeline zur Transformierung von Daten mithilfe eines Hadoop-Clusters](data-factory-build-your-first-pipeline.md), bevor Sie diesen Artikel lesen. 

## <a name="json-sample"></a>JSON-Beispiel
Der HDInsight-Cluster wird automatisch mit Beispielprogrammen („wc.exe“ und „cat.exe“) und Daten („davinci.txt“) aufgefüllt. Standardmäßig entspricht der Name des Containers, der im HDInsight-Cluster verwendet wird, dem Namen des Clusters. Wenn der Name Ihres Clusters beispielsweise „myhdicluster“ lautet, lautet der Name des zugeordneten Blob-Containers auch „myhdicluster“. 

```JSON
{
    "name": "HadoopStreamingPipeline",
    "properties": {
        "description": "Hadoop Streaming Demo",
        "activities": [
            {
                "type": "HDInsightStreaming",
                "typeProperties": {
                    "mapper": "cat.exe",
                    "reducer": "wc.exe",
                    "input": "wasb://<nameofthecluster>@spestore.blob.core.windows.net/example/data/gutenberg/davinci.txt",
                    "output": "wasb://<nameofthecluster>@spestore.blob.core.windows.net/example/data/StreamingOutput/wc.txt",
                    "filePaths": [
                        "<nameofthecluster>/example/apps/wc.exe",
                        "<nameofthecluster>/example/apps/cat.exe"
                    ],
                    "fileLinkedService": "AzureStorageLinkedService",
                    "getDebugInfo": "Failure"
                },
                "outputs": [
                    {
                        "name": "StreamingOutputDataset"
                    }
                ],
                "policy": {
                    "timeout": "01:00:00",
                    "concurrency": 1,
                    "executionPriorityOrder": "NewestFirst",
                    "retry": 1
                },
                "scheduler": {
                    "frequency": "Day",
                    "interval": 1
                },
                "name": "RunHadoopStreamingJob",
                "description": "Run a Hadoop streaming job",
                "linkedServiceName": "HDInsightLinkedService"
            }
        ],
        "start": "2014-01-04T00:00:00Z",
        "end": "2014-01-05T00:00:00Z"
    }
}
```

Beachten Sie folgende Punkte:

1. Legen Sie **linkedServiceName** auf den Namen des verknüpften Diensts fest, der auf Ihren HDInsight-Cluster verweist, auf dem der MapReduce-Streamingauftrag ausgeführt wird.
2. Legen Sie den Typ der Aktivität auf **HDInsightStreaming**fest.
3. Geben Sie für die **mapper** -Eigenschaft den Namen der ausführbaren Zuordnungsdatei an. Im Beispiel ist „cat.exe“ die ausführbare Zuordnungsdatei.
4. Geben Sie für die **reducer** -Eigenschaft den Namen der ausführbaren Reduzierungsdatei an. Im Beispiel ist „wc.exe“ die ausführbare Reduzierungsdatei.
5. Geben Sie für die **input** -Eigenschaft die Eingabedatei (einschließlich Speicherort) für die Zuordnung an. Im Beispiel „wasb://adfsample@<account name>.blob.core.windows.net/example/data/gutenberg/davinci.txt“ ist „adfsample“ der Blobcontainer, „example/data/Gutenberg“ der Ordner und „davinci.txt“ das Blob.
6. Geben Sie für die **output** -Eigenschaft die Ausgabedatei (einschließlich Speicherort) für die Reduzierung an. Die Ausgabe des Hadoop-Streamingauftrags wird an dem für diese Eigenschaft angegebenen Speicherort geschrieben.
7. Geben Sie im Abschnitt **filePaths** die Pfade für die ausführbare Zuordnungs- und Reduzierungsdatei an. Im Beispiel „adfsample/example/apps/wc.exe“ ist „adfsample“ der Blob-Container, „example/apps“ der Ordner und „wc.exe“ die ausführbare Datei.
8. Geben Sie für die **fileLinkedService** -Eigenschaft den mit Azure Storage verknüpften Dienst an, der den Azure-Speicher mit den im Abschnitt „filePaths“ angegebenen Dateien darstellt.
9. Geben Sie für die **arguments** -Eigenschaft die Argumente für den Streamingauftrag an.
10. Die **getDebugInfo** -Eigenschaft ist ein optionales Element. Wenn die Eigenschaft auf "Failure" festgelegt wird, werden die Protokolle nur bei Fehlern heruntergeladen. Wenn die Eigenschaft auf „Immer“ festgelegt wird, werden Protokolle immer heruntergeladen, unabhängig vom Ausführungsstatus.

> [!NOTE]
> Wie im Beispiel gezeigt, legen Sie in Bezug auf die Hadoop-Streamingaktivität für die **outputs** -Eigenschaft ein Ausgabedataset fest. Dieses Dataset ist nur ein für die Pipeline erforderliches Dummydataset. Sie müssen in Bezug auf die Aktivität kein Eingabedataset für die **inputs** -Eigenschaft festlegen.  
> 
> 

## <a name="example"></a>Beispiel
In der Pipeline in dieser exemplarischen Vorgehensweise wird das Zuordnungs-/Reduzierungsprogramm für das Streaming der Wortzählung auf Ihrem Azure HDInsight-Cluster ausgeführt. 

### <a name="linked-services"></a>Verknüpfte Dienste
#### <a name="azure-storage-linked-service"></a>Mit Azure Storage verknüpfter Dienst
Erstellen Sie zunächst einen verknüpften Dienst, um die vom Azure HDInsight-Cluster verwendete Azure Storage-Instanz mit der Azure Data Factory zu verknüpfen. Denken Sie beim Kopieren und Einfügen des folgenden Codes daran, „account name“ (Kontoname) und „account key“ (Kontoschlüssel) durch den Namen und Schlüssel Ihrer Azure Storage-Instanz zu ersetzen. 

```JSON
{
    "name": "StorageLinkedService",
    "properties": {
        "type": "AzureStorage",
        "typeProperties": {
            "connectionString": "DefaultEndpointsProtocol=https;AccountName=<account name>;AccountKey=<account key>"
        }
    }
}
```

#### <a name="azure-hdinsight-linked-service"></a>Mit Azure HDInsight verknüpfter Dienst
Erstellen Sie anschließend einen verknüpften Dienst, um Ihren Azure HDInsight-Cluster mit der Azure Data Factory zu verknüpfen. Ersetzen Sie beim Kopieren und Einfügen des folgenden Codes „HDInsight cluster name“ durch den Namen des HDInsight-Clusters, und ändern Sie die Werte für Benutzername und Kennwort. 

```JSON
{
    "name": "HDInsightLinkedService",
    "properties": {
        "type": "HDInsight",
        "typeProperties": {
            "clusterUri": "https://<HDInsight cluster name>.azurehdinsight.net",
            "userName": "admin",
            "password": "**********",
            "linkedServiceName": "StorageLinkedService"
        }
    }
}
```

### <a name="datasets"></a>Datasets
#### <a name="output-dataset"></a>Ausgabedataset
Die Pipeline in diesem Beispiel akzeptiert keine Eingaben. Für die HDInsight-Streamingaktivität geben Sie ein Ausgabedataset an. Dieses Dataset ist nur ein für die Pipeline erforderliches Dummydataset. 

```JSON
{
    "name": "StreamingOutputDataset",
    "properties": {
        "published": false,
        "type": "AzureBlob",
        "linkedServiceName": "StorageLinkedService",
        "typeProperties": {
            "folderPath": "adftutorial/streamingdata/",
            "format": {
                "type": "TextFormat",
                "columnDelimiter": ","
            },
        },
        "availability": {
            "frequency": "Day",
            "interval": 1
        }
    }
}
```

### <a name="pipeline"></a>Pipeline
Die Pipeline in diesem Beispiel weist nur eine einzelne Aktivität vom Typ **HDInsightStreaming**auf. 

Der HDInsight-Cluster wird automatisch mit Beispielprogrammen („wc.exe“ und „cat.exe“) und Daten („davinci.txt“) aufgefüllt. Standardmäßig entspricht der Name des Containers, der im HDInsight-Cluster verwendet wird, dem Namen des Clusters. Wenn der Name Ihres Clusters beispielsweise „myhdicluster“ lautet, lautet der Name des zugeordneten Blob-Containers auch „myhdicluster“.  

```JSON
{
    "name": "HadoopStreamingPipeline",
    "properties": {
        "description": "Hadoop Streaming Demo",
        "activities": [
            {
                "type": "HDInsightStreaming",
                "typeProperties": {
                    "mapper": "cat.exe",
                    "reducer": "wc.exe",
                    "input": "wasb://<blobcontainer>@spestore.blob.core.windows.net/example/data/gutenberg/davinci.txt",
                    "output": "wasb://<blobcontainer>@spestore.blob.core.windows.net/example/data/StreamingOutput/wc.txt",
                    "filePaths": [
                        "<blobcontainer>/example/apps/wc.exe",
                        "<blobcontainer>/example/apps/cat.exe"
                    ],
                    "fileLinkedService": "StorageLinkedService"
                },
                "outputs": [
                    {
                        "name": "StreamingOutputDataset"
                    }
                ],
                "policy": {
                    "timeout": "01:00:00",
                    "concurrency": 1,
                    "executionPriorityOrder": "NewestFirst",
                    "retry": 1
                },
                "scheduler": {
                    "frequency": "Day",
                    "interval": 1
                },
                "name": "RunHadoopStreamingJob",
                "description": "Run a Hadoop streaming job",
                "linkedServiceName": "HDInsightLinkedService"
            }
        ],
        "start": "2017-01-03T00:00:00Z",
        "end": "2017-01-04T00:00:00Z"
    }
}
```
## <a name="see-also"></a>Weitere Informationen
* [Hive-Aktivität](data-factory-hive-activity.md)
* [Pig-Aktivität](data-factory-pig-activity.md)
* [MapReduce-Aktivität](data-factory-map-reduce.md)
* [Invoke Spark programs (Aufrufen von Spark-Programmen)](data-factory-spark.md)
* [Invoke R scripts (Aufrufen von R-Skripts)](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/RunRScriptUsingADFSample)


