<properties 
	pageTitle="Aufrufen von Spark-Programmen aus Azure Data Factory" 
	description="Erfahren Sie, wie Sie Spark-Programme mithilfe der MapReduce-Aktivität aus einer Azure Data Factory aufrufen." 
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

# Aufrufen von Spark-Programmen aus Data Factory
## Einführung
Sie können die MapReduce-Aktivität in einer Data Factory-Pipeline verwenden, um Spark-Programme in Ihrem HDInsight Spark-Cluster auszuführen. Lesen Sie die detaillierten Informationen zur Verwendung der Aktivität unter [MapReduce-Aktivität](data-factory-map-reduce.md), bevor Sie mit dem vorliegenden Artikel fortfahren.

## Spark-Beispiel auf GitHub
Das [Spark-Beispiel für Data Factory auf GitHub](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/Spark) zeigt, wie Sie die MapReduce-Aktivität verwenden, um ein Spark-Programm aufzurufen. Das Spark-Programm kopiert ganz einfach Daten von einem Azure-Blobcontainer in einen anderen.

## Data Factory-Entitäten
Der Ordner **Spark-ADF/src/ADFJsons** enthält Dateien für Data Factory-Entitäten (verknüpfte Dienste, Datasets, Pipeline).

Dieses Beispiel enthält zwei **verknüpfte Dienste**: Azure Storage und Azure HDInsight. Sie müssen den Namen und die Schlüsselwerte Ihres Azure-Speichers in **StorageLinkedService.json** und Cluster-URI, Benutzernamen sowie Kennwort in **HDInsightLinkedService.json** angeben.

Dieses Beispiel enthält zwei **Datasets**: **input.json** und **output.json**. Diese Dateien befinden sich im Ordner **Datasets**. Diese Dateien stellen die Eingabe- und Ausgabedatasets für die MapReduce-Aktivität dar.

Das Beispiel enthält eine **Pipeline** im Ordner **ADFJsons/Pipeline**. Dies ist die wichtigste Entität, die Sie untersuchen müssen, um zu verstehen, wie ein Spark-Programm mithilfe einer MapReduce-Aktivität aufgerufen wird.

	{
	    "name": "SparkSubmit",
	    "properties": {
	        "description": "Submit a spark job",
	        "activities": [
	            {
	                "type": "HDInsightMapReduce",
	                "typeProperties": {
	                    "className": "com.adf.spark.SparkJob",
	                    "jarFilePath": "libs/spark-adf-job-bin.jar",
	                    "jarLinkedService": "StorageLinkedService",
	                    "arguments": [
	                        "--jarFile",
	                        "libs/sparkdemoapp_2.10-1.0.jar",
	                        "--jars",
	                        "/usr/hdp/current/hadoop-client/hadoop-azure-2.7.1.2.3.3.0-3039.jar,/usr/hdp/current/hadoop-client/lib/azure-storage-2.2.0.jar",
	                        "--mainClass",
	                        "com.adf.spark.demo.Demo",
	                        "--master",
	                        "yarn-cluster",
	                        "--driverMemory",
	                        "2g",
	                        "--driverExtraClasspath",
	                        "/usr/lib/hdinsight-logging/*",
	                        "--executorCores",
	                        "1",
	                        "--executorMemory",
	                        "4g",
	                        "--sparkHome",
	                        "/usr/hdp/current/spark-client",
	                        "--connectionString",
	                        "DefaultEndpointsProtocol=https;AccountName=<YOUR_ACCOUNT>;AccountKey=<YOUR_KEY>",
	                        "input=wasb://input@<YOUR_ACCOUNT>.blob.core.windows.net/data",
	                        "output=wasb://output@<YOUR_ACCOUNT>.blob.core.windows.net/results"
	                    ]
	                },
	                "inputs": [
	                    {
	                        "name": "input"
	                    }
	                ],
	                "outputs": [
	                    {
	                        "name": "output"
	                    }
	                ],
	                "policy": {
	                    "executionPriorityOrder": "OldestFirst",
	                    "timeout": "01:00:00",
	                    "concurrency": 1,
	                    "retry": 1
	                },
	                "scheduler": {
	                    "frequency": "Day",
	                    "interval": 1
	                },
	                "name": "Spark Launcher",
	                "description": "Submits a Spark Job",
	                "linkedServiceName": "HDInsightLinkedService"
	            }
	        ],
	        "start": "2015-11-16T00:00:01Z",
	        "end": "2015-11-16T23:59:00Z",
	        "isPaused": false,
	        "pipelineMode": "Scheduled"
	    }
	}

Wie Sie sehen, ist die MapReduce-Aktivität so konfiguriert, dass sie **spark-adf-job-bin.jar** im **libs**-Container in Ihrem Azure-Speicher aufruft (angegeben in „StorageLinkedService.json“). Der Quellcode für dieses Programm befindet sich im Ordner Spark-ADF/src/main/java/com/adf/spark, ruft „spark-submit“ auf und führt Spark-Aufträge aus.

Dieses MapReduce-Programm (spark-adf-job-bin.jar), das in Ihrem HDInsight-Spark-Cluster ausgeführt wird, ruft das Spark-Programm **sparkdemoapp\_2.10-1.0.jar** auf und übergibt die Argumente, die über die MapReduce-Aktivität empfangen wurden (wie im JSON-Beispiel oben gezeigt), an das Spark-Programm. **sparkdemoapp\_2.10-1.0.jar** enthält Scala-Quellcode, der Daten aus einem Azure-Blobcontainer in einen anderen kopiert. Sie können die JAR-Datei dieser Demo-App durch eine beliebige andere JAR-Datei ersetzen, die einen Auftrag enthält, den Sie über Spark ausführen möchten.

Zusammenfassung: Die **MapReduce-Aktivität** ruft das MapReduce-Programm **spark-adf-job-bin.jar** auf, das wiederum das Spark-Programm **sparkdemoapp\_2.10-1.0.jar** aufruft. Um Ihr eigenes Spark-Programm auszuführen, ersetzen Sie „sparkdemoapp\_2.10-1.0.jar“ durch Ihre eigene JAR-Datei.

> [AZURE.NOTE] Bei dieser Vorgehensweise müssen Sie Ihren eigenen HDInsight-Cluster verwenden, um Spark-Programme mithilfe der MapReduce-Aktivität aufzurufen. Die Verwendung eines bedarfsgesteuerten HDInsight-Clusters wird nicht unterstützt.


## Siehe auch
- [Hive-Aktivität](data-factory-hive-activity.md)
- [Pig-Aktivität](data-factory-pig-activity.md)
- [MapReduce-Aktivität](data-factory-map-reduce.md)
- [Hadoop-Streamingaktivität](data-factory-hadoop-streaming-activity.md)
- [Invoke R scripts](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/RunRScriptUsingADFSample) (Aufrufen von R-Skripts)

<!---HONumber=AcomDC_0420_2016-->