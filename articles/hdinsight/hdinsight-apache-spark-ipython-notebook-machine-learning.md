---
title: "Erstellen von Apache Spark-Machine Learning-Anwendungen für Azure HDInsight | Microsoft-Dokumentation"
description: Schrittweise Anleitung zum Erstellen einer Apache Spark-Machine Learning-Anwendung in HDInsight Spark-Clustern mit Jupyter-Notebook
services: hdinsight
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: f584ca5e-abee-4b7c-ae58-2e45dfc56bf4
ms.service: hdinsight
ms.custom: hdinsightactive
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/25/2017
ms.author: nitinme
ms.translationtype: Human Translation
ms.sourcegitcommit: 80be19618bd02895d953f80e5236d1a69d0811af
ms.openlocfilehash: abb5bcf91a0155f1311bc28657b6208c00e945d1
ms.contentlocale: de-de
ms.lasthandoff: 06/07/2017

---
# <a name="build-apache-spark-machine-learning-applications-on-azure-hdinsight"></a>Erstellen von Apache Spark-Machine Learning-Anwendungen für Azure HDInsight

Erfahren Sie, wie Sie eine Apache Spark-Machine Learning-Anwendung mit einem Spark-Cluster unter HDInsight erstellen. In diesem Artikel wird beschrieben, wie Sie das Jupyter-Notebook für den Cluster zum Erstellen und Testen dieser Anwendung verwenden. Für die Anwendung werden die HVAC.csv-Beispieldaten genutzt, die standardmäßig auf allen Clustern verfügbar sind.

**Voraussetzungen:**

Sie benötigen Folgendes:

* Ein Apache Spark-Cluster unter HDInsight. Eine Anleitung finden Sie unter [Erstellen von Apache Spark-Clustern in Azure HDInsight](hdinsight-apache-spark-jupyter-spark-sql.md). 

## <a name="data"></a>Grundlegendes zum Dataset
Bevor wir mit dem Erstellen der Anwendung beginnen, müssen wir uns zuerst mit der Struktur der Daten, für die wir die Anwendung erstellen, und der Art der Analyse, die wir für die Daten durchführen, vertraut machen. 

In diesem Artikel verwenden wir die Beispieldatendatei **HVAC.csv** , die im Azure Storage-Konto verfügbar ist, das Sie dem HDInsight-Cluster zugeordnet haben. Im Speicherkonto finden Sie die Datei unter **\HdiSamples\HdiSamples\SensorSampleData\hvac**. Laden Sie die CSV-Datei herunter, und öffnen Sie sie, um eine Momentaufnahme der Daten zu erhalten.  

![Momentaufnahme der für das Spark-Machine Learning-Beispiel verwendeten Daten](./media/hdinsight-apache-spark-ipython-notebook-machine-learning/spark-machine-learning-understand-data.png "Momentaufnahme der für das Spark-Machine Learning-Beispiel verwendeten Daten")

Die Daten zeigen die Zieltemperatur und die Ist-Temperatur eines Gebäudes an, in dem HVAC-Systeme installiert sind. Angenommen, die Spalte **System** enthält die System-ID und die Spalte **SystemAge** eine Angabe in Jahren, wie lange das HVAC-System im Gebäude bereits verwendet wird.

Wir sagen anhand dieser Daten vorher, ob es in einem Gebäude basierend auf der Zieltemperatur zu warm oder zu kalt ist. Hierfür werden die System-ID und das Alter des Systems verwendet.

## <a name="app"></a>Schreiben einer Spark-Machine Learning-Anwendung mit Spark MLlib
In dieser Anwendung verwenden wir eine Spark ML-Pipeline, um eine Dokumentklassifizierung durchzuführen. In der Pipeline teilen wir das Dokument in Wörter auf, konvertieren die Wörter in einen numerischen Featurevektor und erstellen dann mit den Featurevektoren und Beschriftungen ein Vorhersagemodell. Führen Sie die folgenden Schritte aus, um die Anwendung zu erstellen:

1. Klicken Sie im [Azure-Portal](https://portal.azure.com/)im Startmenü auf die Kachel für Ihren Spark-Cluster (sofern Sie die Kachel ans Startmenü angeheftet haben). Sie können auch unter **Alle durchsuchen** > **HDInsight-Cluster** zu Ihrem Cluster navigieren.   
2. Klicken Sie auf dem Blatt für den Spark-Cluster auf **Cluster-Dashboard** und anschließend auf **Jupyter Notebook**. Geben Sie die Administratoranmeldeinformationen für den Cluster ein, wenn Sie dazu aufgefordert werden.
   
   > [!NOTE]
   > Sie können auch das Jupyter Notebook für Ihren Cluster aufrufen, indem Sie in Ihrem Browser die folgende URL öffnen. Ersetzen Sie **CLUSTERNAME** durch den Namen Ihres Clusters:
   > 
   > `https://CLUSTERNAME.azurehdinsight.net/jupyter`
   > 
   > 
3. Erstellen Sie ein neues Notebook. Klicken Sie auf **Neu** und dann auf **PySpark**.
   
    ![Erstellen eines Jupyter-Notebooks für das Spark-Machine Learning-Beispiel](./media/hdinsight-apache-spark-ipython-notebook-machine-learning/spark-machine-learning-create-notebook.png "Erstellen eines Jupyter-Notebooks für das Spark-Machine Learning-Beispiel")
4. Ein neues Notebook mit dem Namen „Untitled.pynb“ wird erstellt und geöffnet. Klicken Sie oben auf den Namen des Notebooks, und geben Sie einen Anzeigenamen ein.
   
    ![Angeben eines Notebooknamens für das Spark-Machine Learning-Beispiel](./media/hdinsight-apache-spark-ipython-notebook-machine-learning/spark-machine-learning-notebook-name.png "Angeben eines Notebooknamens für das Spark-Machine Learning-Beispiel")
5. Da Sie ein Notebook mit dem PySpark-Kernel erstellt haben, müssen Sie keine Kontexte explizit erstellen. Die Spark- und Hive-Kontexte werden automatisch für Sie erstellt, wenn Sie die erste Codezelle ausführen. Sie können zunächst die Typen importieren, die für dieses Szenario erforderlich sind. Fügen Sie den folgenden Codeausschnitt in eine leere Zelle ein, und drücken Sie **UMSCHALT+EINGABETASTE**. 
   
        from pyspark.ml import Pipeline
        from pyspark.ml.classification import LogisticRegression
        from pyspark.ml.feature import HashingTF, Tokenizer
        from pyspark.sql import Row
   
        import os
        import sys
        from pyspark.sql.types import *
   
        from pyspark.mllib.classification import LogisticRegressionWithSGD
        from pyspark.mllib.regression import LabeledPoint
        from numpy import array
6. Sie müssen die Daten (hvac.csv) jetzt laden, analysieren und zum Trainieren des Modells verwenden. Definieren Sie hierzu eine Funktion, mit der überprüft wird, ob die Ist-Temperatur des Gebäudes höher als die Zieltemperatur ist. Wenn die Ist-Temperatur höher ist, wird angegeben, dass es in dem Gebäude zu warm ist (Wert **1,0**). Wenn die Ist-Temperatur niedriger ist, wird angegeben, dass es in dem Gebäude zu kalt ist (Wert **0,0**). 
   
    Fügen Sie den folgenden Codeausschnitt in eine leere Zelle ein, und drücken Sie **UMSCHALT+EINGABE**.

        # List the structure of data for better understanding. Because the data will be
        # loaded as an array, this structure makes it easy to understand what each element
        # in the array corresponds to

        # 0 Date
        # 1 Time
        # 2 TargetTemp
        # 3 ActualTemp
        # 4 System
        # 5 SystemAge
        # 6 BuildingID

        LabeledDocument = Row("BuildingID", "SystemInfo", "label")

        # Define a function that parses the raw CSV file and returns an object of type LabeledDocument

        def parseDocument(line):
            values = [str(x) for x in line.split(',')]
            if (values[3] > values[2]):
                hot = 1.0
            else:
                hot = 0.0        

            textValue = str(values[4]) + " " + str(values[5])

            return LabeledDocument((values[6]), textValue, hot)

        # Load the raw HVAC.csv file, parse it using the function
        data = sc.textFile("wasb:///HdiSamples/HdiSamples/SensorSampleData/hvac/HVAC.csv")

        documents = data.filter(lambda s: "Date" not in s).map(parseDocument)
        training = documents.toDF()


1. Konfigurieren Sie die Spark Machine Learning-Pipeline, die drei Phasen umfasst: Tokenizer, hashingTF und lr. Weitere Informationen dazu, was eine Pipeline ist und wie sie funktioniert, finden Sie unter <a href="http://spark.apache.org/docs/latest/ml-guide.html#how-it-works" target="_blank">Spark Machine Learning-Pipeline</a>.
   
    Fügen Sie den folgenden Codeausschnitt in eine leere Zelle ein, und drücken Sie **UMSCHALT+EINGABE**.
   
        tokenizer = Tokenizer(inputCol="SystemInfo", outputCol="words")
        hashingTF = HashingTF(inputCol=tokenizer.getOutputCol(), outputCol="features")
        lr = LogisticRegression(maxIter=10, regParam=0.01)
        pipeline = Pipeline(stages=[tokenizer, hashingTF, lr])
2. Passen Sie die Pipeline an das Schulungsdokument an. Fügen Sie den folgenden Codeausschnitt in eine leere Zelle ein, und drücken Sie **UMSCHALT+EINGABE**.
   
        model = pipeline.fit(training)
3. Überprüfen Sie das Schulungsdokument, um Ihre Fortschritte in Bezug auf die Anwendung zu ermitteln. Fügen Sie den folgenden Codeausschnitt in eine leere Zelle ein, und drücken Sie **UMSCHALT+EINGABE**.
   
        training.show()
   
    Sie sollten eine Ausgabe erhalten, die Folgendem ähnelt:
   
        +----------+----------+-----+
        |BuildingID|SystemInfo|label|
        +----------+----------+-----+
        |         4|     13 20|  0.0|
        |        17|      3 20|  0.0|
        |        18|     17 20|  1.0|
        |        15|      2 23|  0.0|
        |         3|      16 9|  1.0|
        |         4|     13 28|  0.0|
        |         2|     12 24|  0.0|
        |        16|     20 26|  1.0|
        |         9|      16 9|  1.0|
        |        12|       6 5|  0.0|
        |        15|     10 17|  1.0|
        |         7|      2 11|  0.0|
        |        15|      14 2|  1.0|
        |         6|       3 2|  0.0|
        |        20|     19 22|  0.0|
        |         8|     19 11|  0.0|
        |         6|      15 7|  0.0|
        |        13|      12 5|  0.0|
        |         4|      8 22|  0.0|
        |         7|      17 5|  0.0|
        +----------+----------+-----+

    Wechseln Sie zurück, und überprüfen Sie die Ausgabe gegenüber der CSV-Rohdatei. Die erste Zeile der CSV-Datei enthält beispielsweise folgende Daten:

    ![Momentaufnahme der Ausgabedaten für das Spark-Machine Learning-Beispiel](./media/hdinsight-apache-spark-ipython-notebook-machine-learning/spark-machine-learning-output-data.png "Momentaufnahme der Ausgabedaten für das Spark-Machine Learning-Beispiel")

    Beachten Sie, dass die Ist-Temperatur unterhalb der Zieltemperatur liegt. Im Gebäude ist es also zu kalt. In der Schulungsausgabe lautet der Wert **label** in der ersten Zeile daher **0,0**. Dies bedeutet, dass es im Gebäude nicht zu warm ist.

1. Bereiten Sie ein Dataset vor, für das das Schulungsmodell ausgeführt werden kann. Hierzu übergeben wir eine System-ID und ein Systemalter (in der Schulungsausgabe als **SystemInfo** bezeichnet). Mit dem Modell wird dann vorhergesagt, ob es im Gebäude mit der jeweiligen System-ID und dem Systemalter wärmer (1,0) oder kälter (0,0) wäre.
   
   Fügen Sie den folgenden Codeausschnitt in eine leere Zelle ein, und drücken Sie **UMSCHALT+EINGABE**.
   
       # SystemInfo here is a combination of system ID followed by system age
       Document = Row("id", "SystemInfo")
       test = sc.parallelize([(1L, "20 25"),
                     (2L, "4 15"),
                     (3L, "16 9"),
                     (4L, "9 22"),
                     (5L, "17 10"),
                     (6L, "7 22")]) \
           .map(lambda x: Document(*x)).toDF() 
2. Treffen Sie als Letztes die Vorhersagen für die Testdaten. Fügen Sie den folgenden Codeausschnitt in eine leere Zelle ein, und drücken Sie **UMSCHALT+EINGABE**.
   
        # Make predictions on test documents and print columns of interest
        prediction = model.transform(test)
        selected = prediction.select("SystemInfo", "prediction", "probability")
        for row in selected.collect():
            print row
3. Es sollte eine Ausgabe angezeigt werden, die Folgendem ähnelt:
   
       Row(SystemInfo=u'20 25', prediction=1.0, probability=DenseVector([0.4999, 0.5001]))
       Row(SystemInfo=u'4 15', prediction=0.0, probability=DenseVector([0.5016, 0.4984]))
       Row(SystemInfo=u'16 9', prediction=1.0, probability=DenseVector([0.4785, 0.5215]))
       Row(SystemInfo=u'9 22', prediction=1.0, probability=DenseVector([0.4549, 0.5451]))
       Row(SystemInfo=u'17 10', prediction=1.0, probability=DenseVector([0.4925, 0.5075]))
       Row(SystemInfo=u'7 22', prediction=0.0, probability=DenseVector([0.5015, 0.4985]))
   
   In der ersten Zeile der Vorhersage können Sie sehen, dass das Gebäude für ein HVAC-System mit ID 20 und einem Systemalter von 25 Jahren zu warm ist (**prediction=1.0**). Der erste Wert für DenseVector (0,49999) entspricht der Vorhersage 0,0, und der zweite Wert (0,5001) entspricht der Vorhersage 1,0. Obwohl der zweite Wert in der Ausgabe nur unwesentlich höher ist, zeigt das Modell **prediction=1.0**an.
4. Nach Ausführen der Anwendung empfiehlt es sich, das Notebook herunterzufahren, um die Ressourcen freizugeben. Klicken Sie hierzu im Menü **Datei** des Notebooks auf die Option zum **Schließen und Anhalten**. Dadurch wird das Notebook heruntergefahren und geschlossen.

## <a name="anaconda"></a>Verwenden der Anaconda-scikit-learn-Bibliothek für Spark-Machine Learning
Apache Spark-Cluster unter HDInsight enthalten Anaconda-Bibliotheken. Dazu gehört auch die **scikit-learn** -Bibliothek für Machine Learning. Außerdem enthält die Bibliothek verschiedene Datasets, mit denen Sie Beispielanwendungen direkt über ein Jupyter Notebook erstellen können. Beispiele zur Verwendung der scikit-learn-Bibliothek finden Sie unter [http://scikit-learn.org/stable/auto_examples/index.html](http://scikit-learn.org/stable/auto_examples/index.html).

## <a name="seealso"></a>Weitere Informationen
* [Übersicht: Apache Spark in Azure HDInsight](hdinsight-apache-spark-overview.md)

### <a name="scenarios"></a>Szenarios
* [Spark mit BI: Durchführen interaktiver Datenanalysen mithilfe von Spark in HDInsight mit BI-Tools](hdinsight-apache-spark-use-bi-tools.md)
* [Spark mit Machine Learning: Vorhersage von Lebensmittelkontrollergebnissen mithilfe von Spark in HDInsight](hdinsight-apache-spark-machine-learning-mllib-ipython.md)
* [Spark-Streaming: Erstellen von Echtzeitstreaminganwendungen mithilfe von Spark in HDInsight](hdinsight-apache-spark-eventhub-streaming.md)
* [Websiteprotokollanalyse mithilfe von Spark in HDInsight](hdinsight-apache-spark-custom-library-website-log-analysis.md)

### <a name="create-and-run-applications"></a>Erstellen und Ausführen von Anwendungen
* [Erstellen einer eigenständigen Anwendung mit Scala](hdinsight-apache-spark-create-standalone-application.md)
* [Remoteausführung von Aufträgen in einem Spark-Cluster mithilfe von Livy](hdinsight-apache-spark-livy-rest-interface.md)

### <a name="tools-and-extensions"></a>Tools und Erweiterungen
* [Verwenden des HDInsight-Tools-Plug-Ins für IntelliJ IDEA zum Erstellen und Übermitteln von Spark Scala-Anwendungen](hdinsight-apache-spark-intellij-tool-plugin.md)
* [Use HDInsight Tools Plugin for IntelliJ IDEA to debug Spark applications remotely (Verwenden von HDInsight-Tools-Plug-Ins für IntelliJ IDEA zum Remotedebuggen von Spark-Anwendungen)](hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely.md)
* [Verwenden von Zeppelin-Notebooks mit einem Spark-Cluster in HDInsight](hdinsight-apache-spark-zeppelin-notebook.md)
* [Verfügbare Kernels für Jupyter-Notebook im Spark-Cluster für HDInsight](hdinsight-apache-spark-jupyter-notebook-kernels.md)
* [Verwenden von externen Paketen mit Jupyter Notebooks](hdinsight-apache-spark-jupyter-notebook-use-external-packages.md)
* [Installieren von Jupyter Notebook auf Ihrem Computer und Herstellen einer Verbindung zum Apache Spark-Cluster in Azure HDInsight (Vorschau)](hdinsight-apache-spark-jupyter-notebook-install-locally.md)

### <a name="manage-resources"></a>Verwalten von Ressourcen
* [Verwalten von Ressourcen für den Apache Spark-Cluster in Azure HDInsight](hdinsight-apache-spark-resource-manager.md)
* [Track and debug jobs running on an Apache Spark cluster in HDInsight(Nachverfolgen und Debuggen von Aufträgen in einem Apache Spark-Cluster unter HDInsight)](hdinsight-apache-spark-job-debugging.md)

[hdinsight-versions]: hdinsight-component-versioning.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-storage]: hdinsight-hadoop-use-blob-storage.md

[hdinsight-weblogs-sample]: hdinsight-hive-analyze-website-log.md
[hdinsight-sensor-data-sample]: hdinsight-hive-analyze-sensor-data.md

[azure-purchase-options]: http://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: http://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[azure-create-storageaccount]:../storage/common/storage-create-storage-account.md

