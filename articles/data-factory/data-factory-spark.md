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
ms.sourcegitcommit: 432752c895fca3721e78fb6eb17b5a3e5c4ca495
ms.openlocfilehash: 3c7d109ec7fd92859cad4ded7422105e8094485c
ms.lasthandoff: 03/30/2017


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

> [!IMPORTANT]
> Wenn Sie mit Azure Data Factory nicht vertraut sind, empfehlen wir Ihnen, das Tutorial [Erstellen der ersten Pipeline](data-factory-build-your-first-pipeline.md) durchzuarbeiten, bevor Sie diesen Artikel lesen. Eine Übersicht zum Data Factory-Dienst finden Sie unter [Einführung in Azure Data Factory](data-factory-introduction.md). 

## <a name="apache-spark-cluster-in-azure-hdinsight"></a>Apache Spark-Cluster unter HDInsight
Erstellen Sie zuerst einen Apache Spark-Cluster in Azure HDInsight, und befolgen Sie dazu die Anweisungen im Tutorial: [Erstellen eines Apache Spark-Clusters in Azure HDInsight](../hdinsight/hdinsight-apache-spark-jupyter-spark-sql.md). 

## <a name="create-data-factory"></a>Erstellen einer Data Factory 
Dies sind die typischen Schritte zum Erstellen einer Data Factory-Pipeline mit einer Spark-Aktivität.  

1. Erstellen einer Data Factory.
2. Erstellen eines verknüpften Diensts, der den Apache Spark-Cluster unter Azure HDInsight mit Ihrer Data Factory verbindet.
3. Derzeit müssen Sie ein Ausgabedataset für eine Aktivität angeben, auch wenn keine Ausgabe erzeugt wird. Um ein Azure-Blob-Dataset zu erstellen, führen Sie die folgenden Schritte aus:
    1. Erstellen Sie einen verknüpften Dienst, der Ihr Azure Storage-Konto mit der Data Factory verbindet.
    2. Erstellen Sie ein Dataset, das auf den mit Azure Storage verknüpften Dienst verweist.  
3. Erstellen Sie eine Pipeline mit Spark-Aktivität, die auf den in Schritt 2 erstellten, mit Apache HDInsight verknüpften Dienst verweist. Die Aktivität wird mit dem Dataset, das Sie im vorherigen Schritt erstellt haben, als Ausgabedataset konfiguriert. Das Ausgabedataset stellt den Treiber des Zeitplans dar (stündlich, täglich usw.). Daher müssen Sie das Augabedataset auch dann eingeben, wenn die Aktivität keine Ausgabe erzeugt.

Detaillierte Schritt-für-Schritt-Anweisungen zum Erstellen einer Data Factory finden Sie im Tutorial: [Erstellen der ersten Pipeline](data-factory-build-your-first-pipeline.md). Dieses Tutorial verwendet eine Hive-Aktivität mit einem HDInsight Hadoop-Cluster, die Schritte zur Verwendung einer Spark-Aktivität mit einem HDInsight Spark-Cluster sind jedoch ähnlich.   

Die folgenden Abschnitte enthalten Informationen zu Data Factory-Entitäten zur Verwendung von Apache Spark-Clustern und Spark-Aktivitäten in Ihrer Data Factory.   

## <a name="hdinsight-linked-service"></a>Verknüpfter HDInsight-Dienst
Bevor Sie eine Spark-Aktivität in einer Data Factory-Pipeline verwenden, erstellen Sie einen verknüpften (Ihren eigenen) HDInsight-Dienst. Der folgende JSON-Codeausschnitt zeigt die Definition eines verknüpften HDInsight-Diensts, der auf einen Azure HDInsight Spark-Cluster zeigt.   

```json
{
    "name": "HDInsightLinkedService",
    "properties": {
        "type": "HDInsight",
        "typeProperties": {
            "clusterUri": "https://<nameofyoursparkcluster>.azurehdinsight.net/",
              "userName": "admin",
              "password": "password",
              "linkedServiceName": "MyHDInsightStoragelinkedService"
        }
    }
}
```

> [!NOTE]
> Zurzeit unterstützt die Spark-Aktivität keine Spark-Cluster, die einen Azure Data Lake Store als primären Speicher oder einen bei Bedarf verknüpften HDInsight-Dienst verwenden. 

Weitere Informationen zu verknüpften HDInsight-Diensten und anderen verknüpften Computediensten finden Sie im Artikel [Verknüpfte Data Factory-Computedienste](data-factory-compute-linked-services.md). 

## <a name="spark-activity-json"></a>JSON für Spark-Aktivität
Hier ist eine einfache JSON-Definition einer Pipeline mit Spark-Aktivität:    

```json
{
    "name": "SparkPipeline",
    "properties": {
        "activities": [
            {
                "type": "HDInsightSpark",
                "typeProperties": {
                    "rootPath": "adfspark\\pyFiles",
                    "entryFilePath": "test.py",
                    "arguments": [ "arg1", "arg2" ],
                    "sparkConfig": {
                        "spark.python.worker.memory": "512m"
                    }
                    "getDebugInfo": "Always"
                },
                "outputs": [
                    {
                        "name": "OutputDataset"
                    }
                ],
                "name": "MySparkActivity",
                "description": "This activity invokes the Spark program",
                "linkedServiceName": "HDInsightLinkedService"
            }
        ],
        "start": "2017-02-01T00:00:00Z",
        "end": "2017-02-02T00:00:00Z"
    }
}
```

Beachten Sie folgende Punkte: 
- Die „Type“-Eigenschaft ist auf „HDInsightSpark“ festgelegt. 
- Der rootPath ist auf „adfspark\\pyFiles“ festgelegt, wobei „adfspark“ den Azure-Blobcontainer und „pyFiles“ einen Dateiordner in diesem Container darstellt. In diesem Beispiel ist Azure Blob Storage der dem Spark-Cluster zugeordnete Speicher. Sie können die Datei auf einen anderen Azure-Speicher hochladen. Erstellen Sie in diesem Fall einen verknüpften Azure Storage-Dienst, der das Speicherkonto mit der Data Factory verknüpft. Geben Sie dann den Namen des verknüpften Diensts als Wert für die Eigenschaft „sparkJobLinkedService“ an. Details zu dieser und anderen von der Spark-Aktivität unterstützten Eigenschaften finden Sie unter [Eigenschaften von Spark-Aktivitäten](#spark-activity-properties).  
- „entryFilePath“ ist auf „test.py“ festgelegt, das ist die Python-Datei. 
- Die Werte für Parameter für das Spark-Programm werden mithilfe der Eigenschaft „arguments“ übergeben. In diesem Beispiel gibt es zwei Argumente: „arg1“ und „arg2“. 
- Die Eigenschaft „getDebugInfo“ ist auf „Always“ festgelegt, das heißt, Protokolldateien werden in jedem Fall (Erfolg oder Fehler) erstellt. 
- Im Abschnitt „sparkConfig“ ist eine Python-Umgebungseinstellung festgelegt: „worker.memory“. 
- Der Abschnitt „outputs“ weist ein Ausgabedataset auf. Sie müssen in jedem Fall ein Ausgabedataset angeben, selbst wenn das Spark-Programm keine Ausgabe erzeugt. Das Ausgabedataset stellt den Treiber des Zeitplans für die Pipeline dar (stündlich, täglich usw.).     

Die Typeigenschaften (im Abschnitt „typeProperties“) werden weiter unten in diesem Artikel im Abschnitt [Eigenschaften von Spark-Aktivitäten](#spark-activity-properties) beschrieben. 

Wie bereits erwähnt, müssen Sie ein Ausgabedataset für die Aktivität angeben, da es den Treiber für den Zeitplan der Pipeline (stündlich, täglich usw.) darstellt. In diesem Beispiel wird ein Azure-Blobdataset verwendet. Um ein Azure-Blobdataset zu erstellen, müssen Sie zuerst einen verknüpften Azure Storage-Dienst erstellen. 

Dies sind die Beispieldefinitionen des verknüpften Azure Storage-Diensts und des Azure-Blobdatasets: 

**Mit Azure Storage verknüpfter Dienst:**
```json
{
    "name": "AzureStorageLinkedService",
    "properties": {
        "type": "AzureStorage",
        "typeProperties": {
            "connectionString": "DefaultEndpointsProtocol=https;AccountName=<storageaccountname>;AccountKey=<storageaccountkey>"
        }
    }
}
```
 

**Azure-Blobdataset** 
```json
{
    "name": "OutputDataset",
    "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "AzureStorageLinkedService",
        "typeProperties": {
            "fileName": "sparkoutput.txt",
            "folderPath": "spark/output/",
            "format": {
                "type": "TextFormat",
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

Dieses Dataset stellt eher eine Datasetattrappe dar. Data Factory verwendet die Einstellungen für Häufigkeit und Intervall und führt die Pipeline täglich innerhalb der Start- und Endzeiten der Pipeline aus. In der Definition der Beispielpipeline liegen Start- und Endzeitpunkt nur einen Tag auseinander, sodass die Pipeline nur einmal ausgeführt wird. 

## <a name="spark-activity-properties"></a>Eigenschaften von Spark-Aktivitäten

Die folgende Tabelle beschreibt die JSON-Eigenschaften, die in der JSON-Definition verwendet werden: 

| Eigenschaft | Beschreibung | Erforderlich |
| -------- | ----------- | -------- |
| Name | Der Name der Aktivität in der Pipeline. | Ja |
| Beschreibung | Ein Text, der beschreibt, was mit der Aktivität ausgeführt wird. | Nein |
| Typ | Diese Eigenschaft muss auf „HDInsightSpark“ festgelegt werden. | Ja |
| linkedServiceName | Name des mit HDInsight verknüpften Diensts, in dem das Spark-Programm ausgeführt wird. | Ja |
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


