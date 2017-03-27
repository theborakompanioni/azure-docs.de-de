---
title: Aufrufen von Spark-Programmen aus Azure Data Factory
description: "Erfahren Sie, wie Sie Spark-Programme mithilfe der MapReduce-Aktivität aus einer Azure Data Factory aufrufen."
services: data-factory
documentationcenter: 
author: spelluru
manager: jhubbard
editor: monicar
ms.assetid: fd98931c-cab5-4d66-97cb-4c947861255c
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/21/2017
ms.author: spelluru
translationtype: Human Translation
ms.sourcegitcommit: bb1ca3189e6c39b46eaa5151bf0c74dbf4a35228
ms.openlocfilehash: 8be5e1525a7c481de5cb02edd26da305af2d4798
ms.lasthandoff: 03/18/2017


---
# <a name="invoke-spark-programs-from-data-factory"></a>Aufrufen von Spark-Programmen aus Data Factory
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

## <a name="introduction"></a>Einführung
Die HDInsight Spark-Aktivität in einer Data Factory-[Pipeline](data-factory-create-pipelines.md) führt Spark-Programme in [Ihrem eigenen](data-factory-compute-linked-services.md#azure-hdinsight-linked-service) HDInsight-Cluster aus. Dieser Artikel baut auf dem Artikel zu [Datentransformationsaktivitäten](data-factory-data-transformation-activities.md) auf, der eine allgemeine Übersicht über die Datentransformation und die unterstützten Transformationsaktivitäten bietet.

## <a name="hdinsight-linked-service"></a>Verknüpfter HDInsight-Dienst
Bevor Sie eine Spark-Aktivität in einer Data Factory-Pipeline verwenden, erstellen Sie einen verknüpften (Ihren eigenen) HDInsight-Dienst. Der folgende JSON-Codeausschnitt zeigt die Definition eines verknüpften HDInsight-Diensts, der auf Ihren eigenen Azure HDInsight Spark-Cluster zeigt.   

```json
{
    "name": "HDInsightLinkedService",
    "properties": {
        "type": "HDInsight",
        "typeProperties": {
            "clusterUri": "https://MyHdinsightSparkcluster.azurehdinsight.net/",
              "userName": "admin",
              "password": "password",
              "linkedServiceName": "MyHDInsightStoragelinkedService"
        }
    }
}
```

> [!NOTE]
> Zurzeit unterstützt die Spark-Aktivität keine Spark-Cluster, die Data Lake Store als primären Speicher oder einen bedarfsbasierten verknüpften HDInsight-Dienst verwenden. 

Weitere Informationen zu verknüpften HDInsight-Diensten und anderen verknüpften Computediensten finden Sie im Artikel [Verknüpfte Data Factory-Computedienste](data-factory-compute-linked-services.md). 

## <a name="spark-activity-json"></a>JSON für Spark-Aktivität
Dies ist die JSON-Beispieldefinition einer Spark-Aktivität:    

```json
{
    "name": "MySparkActivity",
    "description": "This activity invokes the Spark program",
    "type": "HDInsightSpark",
    "outputs": [
        {
            "name": "PlaceholderDataset"
        }
    ],
    "linkedServiceName": "HDInsightLinkedService",
    "typeProperties": {
        "rootPath": "mycontainer\\myfolder",
        "entryFilePath": "main.py",
        "arguments": [ "arg1", "arg2" ],
        "sparkConfig": {
              "spark.python.worker.memory": "512m"
        }
    }
}
```
Die folgende Tabelle beschreibt die JSON-Eigenschaften, die in der JSON-Definition verwendet werden: 

| Eigenschaft | Beschreibung | Erforderlich |
| -------- | ----------- | -------- |
| name | Der Name der Aktivität in der Pipeline. | Ja |
| Beschreibung | Ein Text, der beschreibt, was mit der Aktivität ausgeführt wird. | Nein |
| Typ | Diese Eigenschaft muss auf „HDInsightSpark“ festgelegt werden. | Ja |
| linkedServiceName | Verweis auf einen verknüpften HDInsight-Dienst, in dem das Spark-Programm ausgeführt wird. | Ja |
| rootPath | Der Azure-Blobcontainer und -ordner mit der Spark-Datei. Beim Dateinamen muss die Groß-/Kleinschreibung beachtet werden. | Ja |
| entryFilePath | Der relative Pfad zum Stammordner des Spark-Codes bzw. -Pakets. | Ja |
| className | Die Java-/Spark-Hauptklasse der Anwendung. | Nein | 
| arguments | Eine Liste der Befehlszeilenargumente für das Spark-Programm. | Nein | 
| proxyUser | Das Benutzerkonto, dessen Identität angenommen werden soll, um das Spark-Programm auszuführen. | Nein | 
| sparkConfig | Eigenschaften der Spark-Konfiguration. | Nein | 
| getDebugInfo | Gibt an, ob die Spark-Protokolldateien in den Azure-Speicher kopiert werden, der vom HDInsight-Cluster verwendet (oder) von sparkJobLinkedService angegeben wird. Zulässige Werte: Keine, Immer oder Fehler. Standardwert: Keine | Nein | 
| sparkJobLinkedService | Der verknüpfte Azure Storage-Dienst, der die Datei sowie die Abhängigkeiten und Protokolle für den Spark-Auftrag enthält.  Wenn Sie für diese Eigenschaft keinen Wert angeben, wird der Speicher verwendet, der dem HDInsight-Cluster zugeordnet ist. | Nein |

## <a name="folder-structure"></a>Ordnerstruktur
Die Spark-Aktivität unterstützt im Gegensatz zu Pig- und Hive-Aktivitäten keine Inlineskripts. Spark-Aufträge lassen sich zudem besser erweitern als Pig- oder Hive-Aufträge. Bei Spark-Aufträgen können Sie mehrere Abhängigkeiten wie z.B. jar-Pakete (im Java-CLASSPATH platziert), Python-Dateien (im PYTHONPATH platziert) sowie beliebige andere Dateien bereitstellen.

Erstellen Sie folgende Ordnerstruktur in dem Azure-Blobspeicher, auf den der verknüpfte HDInsight-Dienst verweist. Laden Sie dann abhängige Dateien in die entsprechenden Unterordner in dem Stammordner hoch, der von **entryFilePath** repräsentiert wird. Python-Dateien werden beispielsweise in den Unterordner „pyFiles“ und jar-Dateien in den Unterordner „jars“ des Stammordners hochgeladen. Zur Laufzeit erwartet der Data Factory-Dienst die folgende Ordnerstruktur im Azure-Blobspeicher:     

| path | Beschreibung | Erforderlich | Typ |
| ---- | ----------- | -------- | ---- | 
| verfügbar.    | Der Stammpfad des Spark-Auftrags im verknüpften Speicherdienst.    | Ja | Ordner |
| &lt;benutzerdefiniert&gt; | Der Pfad, der auf die Eingabedatei des Spark-Auftrags zeigt. | Ja | File | 
| ./jars | Alle Dateien in diesem Ordner werden hochgeladen und im Java-CLASSPATH des Clusters platziert. | Nein | Ordner |
| ./pyFiles | Alle Dateien in diesem Ordner werden hochgeladen und im PYTHONPATH des Clusters platziert. | Nein | Ordner |
| ./files | Alle Dateien in diesem Ordner werden hochgeladen und im Executor-Arbeitsverzeichnis platziert. | Nein | Ordner |
| ./archives | Alle Dateien in diesem Ordner sind nicht komprimiert. | Nein | Ordner |
| ./logs | Der Ordner, in dem Protokolle aus dem Spark-Cluster gespeichert werden.| Nein | Ordner |

Hier finden Sie ein Beispiel für einen Speicher mit zwei Spark-Auftragsdateien in dem Azure-Blobspeicher, auf den der verknüpfte HDInsight-Dienst verweist. 

```
SparkJob1
    main.jar
    files
        input1.txt
        input2.txt
    jars
        package1.jar
        package2.jar
    logs

SparkJob2
    main.py
    pyFiles
        scrip1.py
        script2.py
    logs    
```

> [!IMPORTANT]
> Eine vollständige exemplarische Vorgehensweise zum Erstellen einer Pipeline mit einer Transformationsaktivität finden Sie unter [Erstellen einer Pipeline zum Transformieren von Daten](data-factory-build-your-first-pipeline-using-editor.md). 



## <a name="see-also"></a>Weitere Informationen
* [Hive-Aktivität](data-factory-hive-activity.md)
* [Pig-Aktivität](data-factory-pig-activity.md)
* [MapReduce-Aktivität](data-factory-map-reduce.md)
* [Hadoop-Streamingaktivität](data-factory-hadoop-streaming-activity.md)
* [Invoke R scripts (Aufrufen von R-Skripts)](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/RunRScriptUsingADFSample)


