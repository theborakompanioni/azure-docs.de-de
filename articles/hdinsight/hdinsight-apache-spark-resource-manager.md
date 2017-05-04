---
title: "Verwalten von Ressourcen für den Apache Spark-Cluster unter Azure HDInsight | Microsoft-Dokumentation"
description: "Hier erfahren Sie, wie Sie Ressourcen für Spark-Cluster unter Azure HDInsight verwalten, um die Leistung zu optimieren."
services: hdinsight
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 9da7d4e3-458e-4296-a628-77b14643f7e4
ms.service: hdinsight
ms.custom: hdinsightactive
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/03/2017
ms.author: nitinme
translationtype: Human Translation
ms.sourcegitcommit: a939a0845d7577185ff32edd542bcb2082543a26
ms.openlocfilehash: 68972d9422dd0861318b0f26a4543f7596d9a680
ms.lasthandoff: 01/24/2017


---
# <a name="manage-resources-for-apache-spark-cluster-on-azure-hdinsight"></a>Verwalten von Ressourcen für den Apache Spark-Cluster unter Azure HDInsight 

In diesem Artikel erfahren Sie, wie Sie auf die mit Ihrem Spark-Cluster verknüpften Benutzeroberflächen wie die Ambari-Benutzeroberfläche, die YARN-Benutzeroberfläche und den Spark-Verlaufsserver zugreifen können. Sie erfahren auch, wie Sie die Clusterkonfiguration für eine optimale Leistung optimieren können.

**Voraussetzungen:**

Sie benötigen Folgendes:

* Ein Azure-Abonnement. Siehe [How to get Azure Free trial for testing Hadoop in HDInsight](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/)(in englischer Sprache).
* Einen Apache Spark-Cluster unter HDInsight. Eine Anleitung finden Sie unter [Erstellen von Apache Spark-Clustern in Azure HDInsight](hdinsight-apache-spark-jupyter-spark-sql.md).

## <a name="how-do-i-launch-the-ambari-web-ui"></a>Wie wird die Ambari-Webbenutzeroberfläche gestartet?
1. Klicken Sie im [Azure-Portal](https://portal.azure.com/)im Startmenü auf die Kachel für Ihren Spark-Cluster (sofern Sie die Kachel ans Startmenü angeheftet haben). Sie können auch unter **Alle durchsuchen** > **HDInsight-Cluster** zu Ihrem Cluster navigieren.
2. Klicken Sie auf dem Blatt für den Spark-Cluster auf **Dashboard**. Geben Sie bei Aufforderung die Anmeldeinformationen für den Spark-Cluster ein.

    ![Starten von Ambari](./media/hdinsight-apache-spark-resource-manager/hdispark.cluster.launch.dashboard.png "Starten des Ressourcen-Managers")
3. Damit sollte wie unten dargestellt die Ambari-Webbenutzeroberfläche gestartet werden.

    ![Ambari-Webbenutzeroberfläche](./media/hdinsight-apache-spark-resource-manager/ambari-web-ui.png "Ambari-Webbenutzeroberfläche")   

## <a name="how-do-i-launch-the-spark-history-server"></a>Wie starte ich den Spark-Verlaufsserver?
1. Klicken Sie im [Azure-Portal](https://portal.azure.com/)im Startmenü auf die Kachel für Ihren Spark-Cluster (sofern Sie die Kachel ans Startmenü angeheftet haben).
2. Klicken Sie auf dem Clusterblatt unter **Quicklinks** auf **Cluster-Dashboard**. Klicken Sie auf dem Blatt **Cluster-Dashboard** auf **Spark-Verlaufsserver**.

    ![Spark-Verlaufsserver](./media/hdinsight-apache-spark-resource-manager/launch-history-server.png "Spark-Verlaufsserver")

    Geben Sie bei Aufforderung die Anmeldeinformationen für den Spark-Cluster ein.

## <a name="how-do-i-launch-the-yarn-ui"></a>Wie wird die Yarn-Benutzeroberfläche gestartet?
Auf der YARN-Benutzeroberfläche können Anwendungen überwachen, die derzeit auf dem Spark-Cluster ausgeführt werden.

1. Klicken Sie auf dem Clusterblatt auf **Cluster-Dashboard** und dann auf **YARN**.

    ![YARN-Benutzeroberfläche starten](./media/hdinsight-apache-spark-resource-manager/launch-yarn-ui.png)

   > [!TIP]
   > Alternativ können Sie die YARN-Benutzeroberfläche auch über die Ambari-Benutzeroberfläche starten. Klicken Sie zum Starten der Ambari-Benutzeroberfläche auf dem Clusterblatt auf **Cluster-Dashboard** und dann auf **HDInsight-Cluster-Dashboard**. Klicken Sie auf der Ambari-Benutzeroberfläche auf **YARN**, auf **Quick Links**, auf den aktiven Ressourcen-Manager und dann auf **ResourceManager UI** (ResourceManager-UI).
   >
   >

## <a name="what-is-the-optimum-cluster-configuration-to-run-spark-applications"></a>Was ist die optimale Clusterkonfiguration für die Ausführung von Spark-Anwendungen?
Die drei wichtigsten Parameter, die je nach Anwendungsanforderungen für die Spark-Konfiguration verwendet werden können, sind `spark.executor.instances`, `spark.executor.cores` und `spark.executor.memory`. Ein Executor ist ein Prozess, der für eine Spark-Anwendung gestartet wird. Er wird auf dem Workerknoten ausgeführt und ist für die Ausführung der Aufgaben für die Anwendung zuständig. Die Standardanzahl von Executors und die Executorgrößen für jeden Cluster werden basierend auf der Anzahl von Workerknoten und der Größe der Workerknoten berechnet. Diese werden in `spark-defaults.conf` im Clusterhauptknoten gespeichert.

Die drei Konfigurationsparameter können auf Clusterebene (für alle Anwendungen, die im Cluster ausgeführt werden) konfiguriert werden oder für jede einzelne Anwendung angegeben werden.

### <a name="change-the-parameters-using-ambari-ui"></a>Ändern der Parameter mit der Ambari-Benutzeroberfläche
1. Klicken Sie auf der Ambari-Benutzeroberfläche auf **Spark** und auf **Configs** (Konfigurationen), und erweitern Sie dann **Custom spark-defaults**.

    ![Festlegen von Parametern mit Ambari](./media/hdinsight-apache-spark-resource-manager/set-parameters-using-ambari.png)
2. Die Standardwerte sind gut geeignet, um vier Spark-Anwendungen gleichzeitig im Cluster auszuführen. Sie können diese Werte über die Benutzeroberfläche ändern, wie unten dargestellt.

    ![Festlegen von Parametern mit Ambari](./media/hdinsight-apache-spark-resource-manager/set-executor-parameters.png)
3. Klicken Sie zum Speichern der Konfigurationsänderungen auf **Save** . Oben auf der Seite werden Sie aufgefordert, alle betroffenen Dienste neu zu starten. Klicken Sie auf **Restart**.

    ![Neustarten von Diensten](./media/hdinsight-apache-spark-resource-manager/restart-services.png)

### <a name="change-the-parameters-for-an-application-running-in-jupyter-notebook"></a>Ändern der Parameter für eine Anwendung, die im Jupyter-Notebook ausgeführt wird
Für Anwendungen, die im Jupyter-Notebook ausgeführt werden, können Sie die `%%configure` -Magic für Konfigurationsänderungen verwenden. Im Idealfall nehmen Sie diese Änderungen am Anfang der Anwendung vor, bevor Sie die erste Codezelle ausführen. Dadurch wird sichergestellt, dass die Konfiguration auf die Livy-Sitzung angewendet wird, wenn sie erstellt wird. Wenn Sie die Konfiguration zu einem späteren Zeitpunkt in der Anwendung ändern möchten, müssen Sie den Parameter `-f` verwenden. Allerdings geht dadurch der gesamte Fortschritt in der Anwendung verloren.

Der folgende Codeausschnitt zeigt, wie die Konfiguration für eine Anwendung geändert wird, die in Jupyter ausgeführt wird.

    %%configure
    {"executorMemory": "3072M", "executorCores": 4, "numExecutors":10}

Konfigurationsparameter müssen als JSON-Zeichenfolge übergeben werden und in der nächsten Zeile nach dem Magic-Befehl stehen, wie in der Beispielspalte gezeigt.

### <a name="change-the-parameters-for-an-application-submitted-using-spark-submit"></a>Ändern der Parameter für eine Anwendung, die mit „spark-submit“ übermittelt wird
Der folgende Befehl ist ein Beispiel dafür, wie die Konfigurationsparameter für eine Batchanwendung geändert werden, die mithilfe von `spark-submit`übermittelt wird.

    spark-submit --class <the application class to execute> --executor-memory 3072M --executor-cores 4 –-num-executors 10 <location of application jar file> <application parameters>

### <a name="change-the-parameters-for-an-application-submitted-using-curl"></a>Ändern der Parameter für eine Anwendung, die mit „cURL“ übermittelt wird
Der folgende Befehl ist ein Beispiel dafür, wie die Konfigurationsparameter für eine Batchanwendung geändert werden, die mithilfe von „cURL“ übermittelt wird.

    curl -k -v -H 'Content-Type: application/json' -X POST -d '{"file":"<location of application jar file>", "className":"<the application class to execute>", "args":[<application parameters>], "numExecutors":10, "executorMemory":"2G", "executorCores":5' localhost:8998/batches

### <a name="how-do-i-change-these-parameters-on-a-spark-thrift-server"></a>Wie ändere ich diese Parameter auf einem Spark Thrift-Server?
Der Spark Thrift-Server bietet JDBC/ODBC-Zugriff auf einen Spark-Cluster und wird verwendet, um Spark-SQL-Abfragen zu verarbeiten. Tools wie Power BI, Tableau usw. verwenden das ODBC-Protokoll zur Kommunikation mit dem Spark Thrift-Server, um Spark-SQL-Abfragen als Spark-Anwendung auszuführen. Wenn ein Spark-Cluster erstellt wird, werden zwei Instanzen des Spark Thrift-Servers gestartet, eine auf jedem Hauptknoten. Jeder Spark Thrift-Server wird als Spark-Anwendung auf der YARN-Benutzeroberfläche angezeigt.

Der Spark Thrift-Server nutzt die dynamische Executorzuordnung in Spark und somit wird `spark.executor.instances` nicht verwendet. Der Spark Thrift-Server verwendet stattdessen `spark.dynamicAllocation.minExecutors` und `spark.dynamicAllocation.maxExecutors`, um die Executoranzahl anzugeben. Die Konfigurationsparameter `spark.executor.cores` und `spark.executor.memory` werden verwendet, um die Executorgröße zu ändern. Sie können diese Parameter ändern, wie unten dargestellt.

* Erweitern Sie die Kategorie **Advanced spark-thrift-sparkconf**, um die Parameter `spark.dynamicAllocation.minExecutors`, `spark.dynamicAllocation.maxExecutors` und `spark.executor.memory` zu aktualisieren.

    ![Konfigurieren des Spark Thrift-Servers](./media/hdinsight-apache-spark-resource-manager/spark-thrift-server-1.png)    
* Erweitern Sie die Kategorie **Custom spark-thrift-sparkconf**, um den Parameter `spark.executor.cores` zu aktualisieren.

    ![Konfigurieren des Spark Thrift-Servers](./media/hdinsight-apache-spark-resource-manager/spark-thrift-server-2.png)

### <a name="how-do-i-change-the-driver-memory-of-the-spark-thrift-server"></a>Wie ändere ich den Treiberspeicher des Spark Thrift-Servers?
Der Treiberspeicher des Spark Thrift-Servers ist so konfiguriert, dass er 25 % der RAM-Größe des Hauptknotens umfasst, vorausgesetzt, dass die RAM-Gesamtgröße des Hauptknotens über 14 GB liegt. Sie können mit der Ambari-Benutzeroberfläche die Treiberspeicherkonfiguration ändern, wie unten dargestellt.

* Klicken Sie auf der Ambari-Benutzeroberfläche auf **Spark**, klicken Sie auf **Configs** (Konfigurationen), erweitern Sie **Advanced spark-env**, und geben Sie dann den Wert für **spark_thrift_cmd_opts** an.

    ![Konfigurieren des RAM für den Spark Thrift-Server](./media/hdinsight-apache-spark-resource-manager/spark-thrift-server-ram.png)

## <a name="i-do-not-use-bi-with-spark-cluster-how-do-i-take-the-resources-back"></a>Ich verwende BI mit Spark-Cluster nicht. Wie nehme ich die Ressourcen zurück?
Da wir die dynamische Zuordnung von Spark verwenden, werden nur die Ressourcen für die beiden Anwendungsmaster vom Thrift-Server genutzt. Um diese Ressourcen freizugeben, müssen Sie die Dienste des Thrift-Servers im Cluster beenden.

1. Klicken Sie im linken Bereich der Ambari-Benutzeroberfläche auf **Spark**.
2. Klicken Sie auf der nächsten Seite auf **Spark Thrift Servers**.

    ![Neustarten des Thrift-Servers](./media/hdinsight-apache-spark-resource-manager/restart-thrift-server-1.png)
3. Sie sollte zwei Hauptknoten sehen, auf denen der Spark Thrift-Server ausgeführt wird. Klicken Sie auf einen der Hauptknoten.

    ![Neustarten des Thrift-Servers](./media/hdinsight-apache-spark-resource-manager/restart-thrift-server-2.png)
4. Auf der nächsten Seite werden alle auf diesem Hauptknoten ausgeführten Dienste aufgeführt. Klicken Sie in der Liste auf die Dropdownschaltfläche neben „Spark Thrift Server“ und dann auf **Stop**.

    ![Neustarten des Thrift-Servers](./media/hdinsight-apache-spark-resource-manager/restart-thrift-server-3.png)
5. Wiederholen Sie diese Schritte auch auf dem anderen Hauptknoten.

## <a name="my-jupyter-notebooks-are-not-running-as-expected-how-can-i-restart-the-service"></a>Meine Jupyter-Notebooks werden nicht wie erwartet ausgeführt. Wie kann ich den Dienst neu starten?
1. Starten Sie wie oben gezeigt die Ambari-Webbenutzeroberfläche. Klicken Sie im linken Navigationsbereich auf **Jupyter**, auf **Dienstaktionen** und dann auf **Alle neu starten**. Dadurch wird der Jupyter-Dienst auf allen Stammknoten gestartet.

    ![Neustarten von Jupyter](./media/hdinsight-apache-spark-resource-manager/restart-jupyter.png "Neustarten von Jupyter")

## <a name="how-do-i-know-if-i-am-running-out-of-resource"></a>Wie erkenne ich, ob mir die Ressourcen ausgehen?
1. Starten Sie die Benutzeroberfläche von Yarn, wie oben gezeigt. Überprüfen Sie im oberen Bildschirmbereich in der Tabelle mit den Clustermetriken die Werte in den Spalten **Memory Used** (Verwendeter Arbeitsspeicher) und **Memory Total** (Arbeitsspeicher gesamt). Wenn die beiden Werte sehr dicht beieinander liegen, sind unter Umständen nicht genügend Ressourcen für den Start der nächsten Anwendung vorhanden. Gleiches gilt für die Spalten **VCores Used** (Verwendete virtuelle Kerne) und **VCores Total** (Virtuelle Kerne gesamt). Falls in der Hauptansicht eine Anwendung mit dem Zustand **Akzeptiert** nicht in den Zustand **Ausgeführt** oder **Fehler** übergeht, kann dies ebenfalls ein Hinweis darauf sein, dass nicht genügend Ressourcen zum Starten der Anwendung zur Verfügung stehen.

    ![Ressourcenlimit](./media/hdinsight-apache-spark-resource-manager/resource-limit.png "Ressourcenlimit")

## <a name="how-do-i-kill-a-running-application-to-free-up-resource"></a>Wie kann ich eine ausgeführte Anwendung beenden, um Ressourcen freizugeben?
1. Klicken Sie im linken Bereich der Benutzeroberfläche von Yarn auf **Running** (Ausgeführt). Suchen Sie in der Liste mit den ausgeführten Anwendungen nach der Anwendung, die Sie beenden möchten, und klicken auf die **ID**.

    ![Beenden einer App (1)](./media/hdinsight-apache-spark-resource-manager/kill-app1.png "Beenden einer App (1)")

2. Klicken Sie rechts oben auf **Kill Application** (Anwendung beenden) und anschließend auf **OK**.

    ![Beenden einer App (2)](./media/hdinsight-apache-spark-resource-manager/kill-app2.png "Beenden einer App (2)")

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
* [Verwenden von Zeppelin-Notebooks mit einem Spark-Cluster in HDInsight](hdinsight-apache-spark-use-zeppelin-notebook.md)
* [Verfügbare Kernels für Jupyter-Notebook im Spark-Cluster für HDInsight](hdinsight-apache-spark-jupyter-notebook-kernels.md)
* [Verwenden von externen Paketen mit Jupyter Notebooks](hdinsight-apache-spark-jupyter-notebook-use-external-packages.md)
* [Installieren von Jupyter Notebook auf Ihrem Computer und Herstellen einer Verbindung zum Apache Spark-Cluster in Azure HDInsight (Vorschau)](hdinsight-apache-spark-jupyter-notebook-install-locally.md)

### <a name="manage-resources"></a>Verwalten von Ressourcen
* [Track and debug jobs running on an Apache Spark cluster in HDInsight (Nachverfolgen und Debuggen von Aufträgen in einem Apache Spark-Cluster unter HDInsight)](hdinsight-apache-spark-job-debugging.md)

[hdinsight-versions]: hdinsight-component-versioning.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-storage]: hdinsight-hadoop-use-blob-storage.md


[azure-purchase-options]: http://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: http://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[azure-management-portal]: https://manage.windowsazure.com/
[azure-create-storageaccount]: storage-create-storage-account.md

