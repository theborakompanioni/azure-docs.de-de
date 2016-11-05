---
title: Aufrufen von Spark-Programmen aus Azure Data Factory
description: Erfahren Sie, wie Sie Spark-Programme mithilfe der MapReduce-Aktivität aus einer Azure Data Factory aufrufen.
services: data-factory
documentationcenter: ''
author: spelluru
manager: jhubbard
editor: monicar

ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/25/2016
ms.author: spelluru

---
# Aufrufen von Spark-Programmen aus Data Factory
## Einführung
Sie können die MapReduce-Aktivität in einer Data Factory-Pipeline verwenden, um Spark-Programme in Ihrem HDInsight Spark-Cluster auszuführen. Lesen Sie die detaillierten Informationen zur Verwendung der Aktivität unter [MapReduce-Aktivität](data-factory-map-reduce.md), bevor Sie mit dem vorliegenden Artikel fortfahren.

## Spark-Beispiel auf GitHub
Das [Spark-Beispiel für Data Factory auf GitHub](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/Spark) zeigt, wie Sie die MapReduce-Aktivität verwenden, um ein Spark-Programm aufzurufen. Das Spark-Programm kopiert ganz einfach Daten von einem Azure-Blobcontainer in einen anderen.

## Data Factory-Entitäten
Der Ordner **Spark-ADF/src/ADFJsons** enthält Dateien für Data Factory-Entitäten (verknüpfte Dienste, Datasets, Pipeline).

Dieses Beispiel enthält zwei **verknüpfte Dienste**: Azure Storage und Azure HDInsight. Geben Sie den Namen und die Schlüsselwerte Ihres Azure-Speichers in **StorageLinkedService.json** an, und geben Sie Cluster-URI, Benutzernamen sowie Kennwort in **HDInsightLinkedService.json** an.

Dieses Beispiel enthält zwei **Datasets**: **input.json** und **output.json**. Diese Dateien befinden sich im Ordner **Datasets**. Diese Dateien stellen die Eingabe- und Ausgabedatasets für die MapReduce-Aktivität dar.

Beispielpipelines finden Sie im Ordner **ADFJsons/Pipeline**. Sehen Sie sich eine Pipeline an, um zu verstehen, wie Sie mithilfe der MapReduce-Aktivität ein Spark-Programm aufrufen.

Die MapReduce-Aktivität ist so konfiguriert, dass sie **com.adf.sparklauncher.jar** im **adflibs**-Container in Ihrem Azure-Speicher aufruft (angegeben in „StorageLinkedService.json“). Der Quellcode für dieses Programm befindet sich im Ordner „Spark-ADF/src/main/java/com/adf/“, ruft „spark-submit“ auf und führt Spark-Aufträge aus.

> [!IMPORTANT]
> Lesen Sie die [README.TXT](https://github.com/Azure/Azure-DataFactory/blob/master/Samples/Spark/README.txt)-Datei, um neue und zusätzliche Informationen zu erhalten, bevor Sie das Beispiel verwenden.
> 
> Bei dieser Vorgehensweise verwenden Sie Ihren eigenen HDInsight-Cluster, um Spark-Programme mithilfe der MapReduce-Aktivität aufzurufen. Die Verwendung eines bedarfsgesteuerten HDInsight-Clusters wird nicht unterstützt.
> 
> 

## Weitere Informationen
* [Hive-Aktivität](data-factory-hive-activity.md)
* [Pig-Aktivität](data-factory-pig-activity.md)
* [MapReduce-Aktivität](data-factory-map-reduce.md)
* [Hadoop-Streamingaktivität](data-factory-hadoop-streaming-activity.md)
* [Invoke R scripts (Aufrufen von R-Skripts)](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/RunRScriptUsingADFSample)

<!---HONumber=AcomDC_0831_2016-->