---
title: Verwenden von Zeppelin Notebooks mit Apache Spark-Cluster unter Azure HDInsight | Microsoft-Dokumentation
description: "Enthält eine Schritt-für-Schritt-Anleitung zur Verwendung von Zeppelin Notebooks mit Apache Spark-Clustern unter Azure HDInsight."
services: hdinsight
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
ms.assetid: df489d70-7788-4efa-a089-e5e5006421e2
ms.service: hdinsight
ms.custom: hdinsightactive
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/28/2017
ms.author: nitinme
ms.translationtype: Human Translation
ms.sourcegitcommit: 80be19618bd02895d953f80e5236d1a69d0811af
ms.openlocfilehash: 9a588a28312388a524d91df7363234e0f609660e
ms.contentlocale: de-de
ms.lasthandoff: 06/07/2017

---
# <a name="use-zeppelin-notebooks-with-apache-spark-cluster-on-azure-hdinsight"></a>Verwenden von Zeppelin Notebooks mit Apache Spark-Cluster unter Azure HDInsight

HDInsight Spark-Cluster enthalten Zeppelin Notebooks, die Sie zum Ausführen von Spark-Aufträgen verwenden können. In diesem Artikel wird beschrieben, wie Sie das Zeppelin Notebook in einem HDInsight-Cluster verwenden.

> [!NOTE]
> Zeppelin Notebooks sind nur für Spark 1.6.3 auf HDInsight 3.5 und Spark 2.1.0 auf HDInsight 3.6 verfügbar.
>

**Voraussetzungen:**

* Ein Azure-Abonnement. Siehe [How to get Azure Free trial for testing Hadoop in HDInsight](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/)(in englischer Sprache).
* Ein Apache Spark-Cluster unter HDInsight. Eine Anleitung finden Sie unter [Erstellen von Apache Spark-Clustern in Azure HDInsight](hdinsight-apache-spark-jupyter-spark-sql.md).

## <a name="launch-a-zeppelin-notebook"></a>Starten eines Zeppelin Notebooks
1. Klicken Sie auf dem Blatt für den Spark-Cluster auf **Cluster-Dashboard** und anschließend auf **Zeppelin Notebook**. Geben Sie die Administratoranmeldeinformationen für den Cluster ein, wenn Sie dazu aufgefordert werden.
   
   > [!NOTE]
   > Sie können auch das Zeppelin Notebook für Ihren Cluster aufrufen, indem Sie in Ihrem Browser die folgende URL öffnen. Ersetzen Sie **CLUSTERNAME** durch den Namen Ihres Clusters:
   > 
   > `https://CLUSTERNAME.azurehdinsight.net/zeppelin`
   > 
   > 
2. Erstellen Sie ein neues Notebook. Klicken Sie im Headerbereich auf **Notebook**, und wählen Sie die Option **Neue Notiz erstellen** aus.
   
    ![Erstellen eines neuen Zeppelin Notebooks](./media/hdinsight-apache-spark-zeppelin-notebook/hdinsight-create-zeppelin-notebook.png "Erstellen eines neuen Zeppelin Notebooks")
   
    Geben Sie einen Namen für das Notebook ein, und klicken Sie dann auf **Create Note** (Notiz erstellen).
3. Stellen Sie außerdem sicher, dass im Header des Notebooks der Status „Verbunden“ angezeigt wird. Dies wird durch einen grünen Punkt in der oberen rechten Ecke angezeigt.
   
    ![Zeppelin Notebook-Status](./media/hdinsight-apache-spark-zeppelin-notebook/hdinsight-zeppelin-connected.png "Zeppelin Notebook-Status")
4. Laden Sie Beispieldaten in eine temporäre Tabelle. Wenn Sie einen Spark-Cluster in HDInsight erstellen, wird die Beispieldatei **hvac.csv** in das zugeordnete Speicherkonto unter **\HdiSamples\SensorSampleData\hvac** kopiert.
   
    Fügen Sie in den leeren Absatz, der im neuen Notebook standardmäßig erstellt wird, den folgenden Codeausschnitt ein.
   
        %livy.spark
        //The above magic instructs Zeppelin to use the Livy Scala interpreter
   
        // Create an RDD using the default Spark context, sc
        val hvacText = sc.textFile("wasb:///HdiSamples/HdiSamples/SensorSampleData/hvac/HVAC.csv")
   
        // Define a schema
        case class Hvac(date: String, time: String, targettemp: Integer, actualtemp: Integer, buildingID: String)
   
        // Map the values in the .csv file to the schema
        val hvac = hvacText.map(s => s.split(",")).filter(s => s(0) != "Date").map(
            s => Hvac(s(0), 
                    s(1),
                    s(2).toInt,
                    s(3).toInt,
                    s(6)
            )
        ).toDF()
   
        // Register as a temporary table called "hvac"
        hvac.registerTempTable("hvac")
   
    Drücken Sie **UMSCHALT+EINGABETASTE**, oder klicken Sie auf die Schaltfläche **Wiedergeben** für den Absatz, um den Codeausschnitt auszuführen. Der Status in der rechten Ecke des Absatzes sollte sich entsprechend ändern: BEREIT, AUSSTEHEND, WIRD AUSGEFÜHRT bis zu BEENDET. Die Ausgabe wird unten im Absatz angezeigt. Der Screenshot sieht folgendermaßen aus:
   
    ![Erstellen einer temporären Tabelle auf der Grundlage von Rohdaten](./media/hdinsight-apache-spark-zeppelin-notebook/hdinsight-zeppelin-load-data.png "Erstellen einer temporären Tabelle auf der Grundlage von Rohdaten")
   
    Sie können auch einen Titel für jeden Absatz angeben. Klicken Sie in der rechten Ecke auf das Symbol **Einstellungen** und dann auf **Titel anzeigen**.
5. Sie können jetzt Spark-SQL-Anweisungen für die **hvac** -Tabelle ausführen. Fügen Sie die folgende Abfrage in einen neuen Absatz ein. Mit der Abfrage werden die Gebäude-ID und der Unterschied zwischen den Ziel- und Ist-Temperaturen für jedes Gebäude an einem bestimmten Datum abgerufen. Drücken Sie **UMSCHALT+EINGABETASTE**.
   
        %sql
        select buildingID, (targettemp - actualtemp) as temp_diff, date from hvac where date = "6/1/13" 
   
    Mit der **%sql**-Anweisung am Anfang wird das Notebook angewiesen, den Livy Scala-Interpreter zu verwenden.
   
    Im folgenden Screenshot ist die Ausgabe dargestellt.
   
    ![Ausführen einer Spark-SQL-Anweisung mit dem Notebook](./media/hdinsight-apache-spark-zeppelin-notebook/hdinsight-zeppelin-spark-query-1.png "Ausführen einer Spark SQL-Anweisung mit dem Notebook")
   
     Klicken Sie auf die Anzeigeoptionen (im Rechteck hervorgehoben), um für eine Ausgabe zwischen unterschiedlichen Darstellungen zu wechseln. Klicken Sie auf **Einstellungen** , um auszuwählen, worum es sich bei den Schlüsseln und Werten in der Ausgabe handelt. Im obigen Screenshot werden **buildingID** als Schlüssel und der Mittelwert von **temp_diff** als Wert verwendet.
6. Sie können auch Spark-SQL-Anweisungen ausführen, indem Sie die Variablen in der Abfrage verwenden. Der nächste Codeausschnitt zeigt, wie Sie eine Variable ( **Temp**) in der Abfrage mit den möglichen Werten definieren, die für die Abfrage verwendet werden sollen. Beim ersten Ausführen der Abfrage wird automatisch eine Dropdownliste mit den Werten aufgefüllt, die Sie für die Variable angegeben haben.
   
        %sql
        select buildingID, date, targettemp, (targettemp - actualtemp) as temp_diff from hvac where targettemp > "${Temp = 65,65|75|85}" 
   
    Fügen Sie diesen Codeausschnitt in einen neuen Absatz ein, und drücken Sie **UMSCHALT+EINGABETASTE**. Im folgenden Screenshot ist die Ausgabe dargestellt.
   
    ![Ausführen einer Spark-SQL-Anweisung mit dem Notebook](./media/hdinsight-apache-spark-zeppelin-notebook/hdinsight-zeppelin-spark-query-2.png "Ausführen einer Spark SQL-Anweisung mit dem Notebook")
   
    Für nachfolgende Abfragen können Sie einen neuen Wert aus der Dropdownliste auswählen und die Abfrage erneut ausführen. Klicken Sie auf **Einstellungen** , um auszuwählen, worum es sich bei den Schlüsseln und Werten in der Ausgabe handelt. Im obigen Screenshot werden **buildingID** als Schlüssel, der Mittelwert von **temp_diff** als Wert und **targettemp** als Gruppe verwendet.
7. Starten Sie den Livy-Interpreter neu, um die Anwendung zu beenden. Öffnen Sie zu diesem Zweck die Einstellungen des Interpreters, indem Sie oben rechts auf den Namen des angemeldeten Benutzers und dann auf **Interpreter** klicken.
   
    ![Starten des Interpreters](./media/hdinsight-apache-spark-zeppelin-notebook/zeppelin-launch-interpreter.png "Hive-Ausgabe")
8. Scrollen Sie zu den Einstellungen des Livy-Interpreters, und klicken Sie dann auf **Neu starten**.
   
    ![Neustarten des Livy-Interpreters](./media/hdinsight-apache-spark-zeppelin-notebook/hdinsight-zeppelin-restart-interpreter.png "Neustarten des Zeppelin-Interpreters")

## <a name="how-do-i-use-external-packages-with-the-notebook"></a>Wie verwende ich externe Pakete mit dem Notebook?
Sie können das Zeppelin Notebook in einem Apache Spark-Cluster in HDInsight (Linux) konfigurieren, um externe, von der Community bereitgestellte Pakete zu verwenden, die nicht im Lieferumfang des Clusters enthalten sind. Sie können das [Maven Repository](http://search.maven.org/) nach einer vollständigen Liste der verfügbaren Pakete durchsuchen. Sie können die Liste der verfügbaren Pakete auch aus anderen Quellen abrufen. Beispielsweise steht eine vollständige Liste der von der Community bereitgestellten Pakete auf [Spark-Pakete](http://spark-packages.org/)zur Verfügung.

In diesem Artikel wird beschrieben, wie Sie das Paket [spark-csv](http://search.maven.org/#artifactdetails%7Ccom.databricks%7Cspark-csv_2.10%7C1.4.0%7Cjar) mit Jupyter Notebook verwenden.

1. Öffnen Sie die Einstellungen des Interpreters. Klicken Sie oben rechts auf den Namen des angemeldeten Benutzers, und klicken Sie dann auf **Interpreter**.
   
    ![Starten des Interpreters](./media/hdinsight-apache-spark-zeppelin-notebook/zeppelin-launch-interpreter.png "Hive-Ausgabe")
2. Scrollen Sie zu den Einstellungen des Livy-Interpreters, und klicken Sie dann auf **Bearbeiten**.
   
    ![Ändern der Interpreter-Einstellungen](./media/hdinsight-apache-spark-zeppelin-notebook/zeppelin-use-external-package-1.png "Ändern der Interpreter-Einstellungen")
3. Fügen Sie den neuen Schlüssel **livy.spark.jars.packages** hinzu, und legen Sie seinen Wert im Format `group:id:version` fest. Wenn Sie das Paket [spark-csv](http://search.maven.org/#artifactdetails%7Ccom.databricks%7Cspark-csv_2.10%7C1.4.0%7Cjar) verwenden möchten, müssen Sie den Wert des Schlüssels auf `com.databricks:spark-csv_2.10:1.4.0` festlegen.
   
    ![Ändern der Interpreter-Einstellungen](./media/hdinsight-apache-spark-zeppelin-notebook/zeppelin-use-external-package-2.png "Ändern der Interpreter-Einstellungen")
   
    Klicken Sie auf **Speichern**, und starten Sie dann den Livy-Interpreter neu.
4. **Tipp**: Hier ist angegeben, wie Sie zum Wert des oben eingegebenen Schlüssels gelangen, falls dies für Sie interessant ist.
   
    a. Suchen Sie das Paket im Maven-Repository. In diesem Tutorial haben wir [spark-csv](http://search.maven.org/#artifactdetails%7Ccom.databricks%7Cspark-csv_2.10%7C1.4.0%7Cjar) verwendet.
   
    b. Sammeln Sie im Repository die Werte für **GroupId**, **ArtifactId** und **Version**.
   
    ![Verwenden externer Pakete mit Jupyter Notebooks](./media/hdinsight-apache-spark-zeppelin-notebook/use-external-packages-with-jupyter.png "Verwenden externer Pakete mit Jupyter Notebooks")
   
    c. Verketten Sie die drei Werte, getrennt durch einen Doppelpunkt (**:**).
   
        com.databricks:spark-csv_2.10:1.4.0

## <a name="where-are-the-zeppelin-notebooks-saved"></a>Wo werden Zeppelin Notebooks gespeichert?
Die Zeppelin Notebooks werden in den Clusterhauptknoten gespeichert. Wenn Sie den Cluster löschen, werden also auch die Notebooks gelöscht. Falls Sie Ihre Notebooks zur späteren Verwendung auf anderen Clustern beibehalten möchten, müssen Sie sie exportieren, nachdem Sie die Ausführung der Aufträge abgeschlossen haben. Klicken Sie zum Exportieren eines Notebooks auf das Symbol **Exportieren**. Dies ist in der Abbildung unten dargestellt.

![Herunterladen des Notebooks](./media/hdinsight-apache-spark-zeppelin-notebook/zeppelin-download-notebook.png "Herunterladen des Notebooks")

Das Notebook wird als JSON-Datei in Ihrem Downloadverzeichnis gespeichert.

## <a name="livy-session-management"></a>Livy-Sitzungsverwaltung
Wenn Sie den ersten Codeabsatz in Ihrem Zeppelin-Notebook ausführen, wird in Ihrem HDInsight Spark-Cluster eine neue Livy-Sitzung erstellt. Diese Sitzung kann für alle Zeppelin Notebooks gemeinsam verwendet werden, die Sie erstellen. Falls die Livy-Sitzung aus irgendeinem Grund beendet wird (Clusterneustart usw.), können Sie keine Aufträge über das Zeppelin Notebook ausführen.

In diesem Fall müssen Sie die folgenden Schritte ausführen, bevor Sie mit dem Ausführen von Aufträgen über ein Zeppelin Notebook beginnen können. 

1. Starten Sie den Livy-Interpreter über das Zeppelin Notebook neu. Öffnen Sie zu diesem Zweck die Einstellungen des Interpreters, indem Sie oben rechts auf den Namen des angemeldeten Benutzers und dann auf **Interpreter** klicken.
   
    ![Starten des Interpreters](./media/hdinsight-apache-spark-zeppelin-notebook/zeppelin-launch-interpreter.png "Hive-Ausgabe")
2. Scrollen Sie zu den Einstellungen des Livy-Interpreters, und klicken Sie dann auf **Neu starten**.
   
    ![Neustarten des Livy-Interpreters](./media/hdinsight-apache-spark-zeppelin-notebook/hdinsight-zeppelin-restart-interpreter.png "Neustarten des Zeppelin-Interpreters")
3. Führen Sie eine Codezelle über ein vorhandenes Zeppelin Notebook aus. Im HDInsight-Cluster wird eine neue Livy-Sitzung erstellt.

## <a name="seealso"></a>Weitere Informationen
* [Übersicht: Apache Spark in Azure HDInsight](hdinsight-apache-spark-overview.md)

### <a name="scenarios"></a>Szenarios
* [Spark mit BI: Durchführen interaktiver Datenanalysen mithilfe von Spark in HDInsight mit BI-Tools](hdinsight-apache-spark-use-bi-tools.md)
* [Spark mit Machine Learning: Analysieren von Gebäudetemperaturen mithilfe von Spark in HDInsight und HVAC-Daten](hdinsight-apache-spark-ipython-notebook-machine-learning.md)
* [Spark mit Machine Learning: Vorhersage von Lebensmittelkontrollergebnissen mithilfe von Spark in HDInsight](hdinsight-apache-spark-machine-learning-mllib-ipython.md)
* [Spark-Streaming: Erstellen von Echtzeitstreaminganwendungen mithilfe von Spark in HDInsight](hdinsight-apache-spark-eventhub-streaming.md)
* [Websiteprotokollanalyse mithilfe von Spark in HDInsight](hdinsight-apache-spark-custom-library-website-log-analysis.md)

### <a name="create-and-run-applications"></a>Erstellen und Ausführen von Anwendungen
* [Erstellen einer eigenständigen Anwendung mit Scala](hdinsight-apache-spark-create-standalone-application.md)
* [Remoteausführung von Aufträgen in einem Spark-Cluster mithilfe von Livy](hdinsight-apache-spark-livy-rest-interface.md)

### <a name="tools-and-extensions"></a>Tools und Erweiterungen
* [Verwenden des HDInsight-Tools-Plug-Ins für IntelliJ IDEA zum Erstellen und Übermitteln von Spark Scala-Anwendungen](hdinsight-apache-spark-intellij-tool-plugin.md)
* [Use HDInsight Tools Plugin for IntelliJ IDEA to debug Spark applications remotely (Verwenden von HDInsight-Tools-Plug-Ins für IntelliJ IDEA zum Remotedebuggen von Spark-Anwendungen)](hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely.md)
* [Verfügbare Kernels für Jupyter-Notebook im Spark-Cluster für HDInsight](hdinsight-apache-spark-jupyter-notebook-kernels.md)
* [Verwenden von externen Paketen mit Jupyter Notebooks](hdinsight-apache-spark-jupyter-notebook-use-external-packages.md)
* [Installieren von Jupyter Notebook auf Ihrem Computer und Herstellen einer Verbindung zum Apache Spark-Cluster in Azure HDInsight (Vorschau)](hdinsight-apache-spark-jupyter-notebook-install-locally.md)

### <a name="manage-resources"></a>Verwalten von Ressourcen
* [Verwalten von Ressourcen für den Apache Spark-Cluster in Azure HDInsight](hdinsight-apache-spark-resource-manager.md)
* [Track and debug jobs running on an Apache Spark cluster in HDInsight(Nachverfolgen und Debuggen von Aufträgen in einem Apache Spark-Cluster unter HDInsight)](hdinsight-apache-spark-job-debugging.md)

[hdinsight-versions]: hdinsight-component-versioning.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-storage]: hdinsight-hadoop-use-blob-storage.md

[azure-purchase-options]: http://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: http://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[azure-create-storageaccount]:../storage/common/storage-create-storage-account.md 








