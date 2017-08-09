---
title: "Debuggen von Apache Spark-Aufträgen, die in HDInsight ausgeführt werden | Microsoft-Dokumentation"
description: "Verwenden Sie die YARN-Benutzeroberfläche, die Spark-Benutzeroberfläche und den Spark-Verlaufsserver, um auf einem Spark-Cluster in Azure HDInsight ausgeführte Aufträge nachzuverfolgen und zu debuggen."
services: hdinsight
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 59af05a7-2bd9-44b0-b55f-2438d294198b
ms.service: hdinsight
ms.custom: hdinsightactive
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/21/2017
ms.author: nitinme
ms.translationtype: HT
ms.sourcegitcommit: 22aa82e5cbce5b00f733f72209318c901079b665
ms.openlocfilehash: bf66757cc9439a969c9f28abc0b95055ff697c3b
ms.contentlocale: de-de
ms.lasthandoff: 07/24/2017

---
# <a name="debug-apache-spark-jobs-running-on-azure-hdinsight"></a>Debuggen von Apache Spark-Aufträgen, die in HDInsight ausgeführt werden

In diesem Artikel erfahren Sie, wie Sie Spark-Aufträge, die in HDInsight ausgeführt werden, mit der YARN-Benutzeroberfläche, der Spark-Benutzeroberfläche und dem Spark-Verlaufsserver nachverfolgen und debuggen. In diesem Artikel starten wir einen Spark-Auftrag über das Notebook **Machine Learning: Vorhersageanalyse von Lebensmittelkontrolldaten mithilfe von MLlib**, das im Spark-Cluster verfügbar ist. Sie können die folgenden Schritte auch ausführen, um eine Anwendung nachzuverfolgen, die Sie mit einer anderen Methode wie z.B. **spark-submit** übermittelt haben.

## <a name="prerequisites"></a>Voraussetzungen
Sie benötigen Folgendes:

* Ein Azure-Abonnement. Siehe [How to get Azure Free trial for testing Hadoop in HDInsight](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/)(in englischer Sprache).
* Ein Apache Spark-Cluster unter HDInsight. Eine Anleitung finden Sie unter [Erstellen von Apache Spark-Clustern in Azure HDInsight](hdinsight-apache-spark-jupyter-spark-sql.md).
* Sie sollten mit der Ausführung des Notebooks **[Machine Learning: Vorhersageanalyse von Lebensmittelkontrolldaten mithilfe von MLlib](hdinsight-apache-spark-machine-learning-mllib-ipython.md)** begonnen haben. Für eine Anleitung zum Ausführen dieses Notebooks folgen Sie dem Link.  

## <a name="track-an-application-in-the-yarn-ui"></a>Nachverfolgen einer Anwendung auf der YARN-Benutzeroberfläche
1. Starten Sie die YARN-Benutzeroberfläche. Klicken Sie auf dem Clusterblatt auf **Cluster-Dashboard** und dann auf **YARN**.
   
    ![YARN-Benutzeroberfläche starten](./media/hdinsight-apache-spark-job-debugging/launch-yarn-ui.png)
   
   > [!TIP]
   > Alternativ können Sie die YARN-Benutzeroberfläche auch über die Ambari-Benutzeroberfläche starten. Klicken Sie zum Starten der Ambari-Benutzeroberfläche auf dem Clusterblatt auf **Cluster-Dashboard** und dann auf **HDInsight-Cluster-Dashboard**. Klicken Sie auf der Ambari-Benutzeroberfläche auf **YARN**, auf **Quick Links**, auf den aktiven Ressourcen-Manager und dann auf **ResourceManager UI** (ResourceManager-UI).    
   > 
   > 
2. Da Sie den Spark-Auftrag mit Jupyter-Notebooks gestartet haben, hat die Anwendung den Namen **remotesparkmagics** (dies ist der Name für alle Anwendungen, die über die Notebooks gestartet werden). Klicken Sie auf die Anwendungs-ID für den Anwendungsnamen, um weitere Informationen zum Auftrag abzurufen. Dadurch wird die Anwendungsansicht geöffnet.
   
    ![Suchen der Spark-Anwendungs-ID](./media/hdinsight-apache-spark-job-debugging/find-application-id.png)
   
    Für Anwendungen, die über die Jupyter-Notebooks gestartet werden, ist der Status immer **WIRD AUSGEFÜHRT** , bis Sie das Notebook beenden.
3. In der Anwendungsansicht können Sie weitere Details anzeigen, um die der Anwendung zugeordneten Container und die Protokolle (stdout/stderr) zu finden. Sie können die Spark-Benutzeroberfläche auch starten, indem Sie auf die Verknüpfung für die **Nachverfolgungs-URL**klicken, wie unten dargestellt. 
   
    ![Herunterladen von Containerprotokollen](./media/hdinsight-apache-spark-job-debugging/download-container-logs.png)

## <a name="track-an-application-in-the-spark-ui"></a>Nachverfolgen einer Anwendung auf der Spark-Benutzeroberfläche
Auf der Spark-Benutzeroberfläche können Sie Details der Spark-Aufträge anzeigen, die von der Anwendung erzeugt werden, die Sie zuvor gestartet haben.

1. Um die Spark-Benutzeroberfläche zu starten, klicken Sie in der Anwendungsansicht auf den Link für die **Nachverfolgungs-URL**, wie im Screenshot oben dargestellt. Es werden alle Spark-Aufträge angezeigt, die von der Anwendung, die im Jupyter-Notebook ausgeführt wird, gestartet werden.
   
    ![Anzeigen von Spark-Aufträgen](./media/hdinsight-apache-spark-job-debugging/view-spark-jobs.png)
2. Klicken Sie auf die Registerkarte **Executors** , um Informationen zur Verarbeitung und Speicherung für jeden Executor anzuzeigen. Sie können auch die Aufrufliste abrufen, indem Sie auf den Link **Thread Dump** klicken.
   
    ![Anzeigen von Spark-Executors](./media/hdinsight-apache-spark-job-debugging/view-spark-executors.png)
3. Klicken Sie auf die Registerkarte **Stages** , um die der Anwendung zugeordneten Phasen anzuzeigen.
   
    ![Anzeigen von Spark-Phasen](./media/hdinsight-apache-spark-job-debugging/view-spark-stages.png)
   
    Jede Phase kann mehrere Aufgaben aufweisen, für die Sie Ausführungsstatistiken anzeigen können, wie unten dargestellt.
   
    ![Anzeigen von Spark-Phasen](./media/hdinsight-apache-spark-job-debugging/view-spark-stages-details.png) 
4. Auf der Seiten mit Phasendetails können Sie die DAG-Visualisierung starten. Erweitern Sie den Link **DAG Visualization** oben auf der Seite, wie unten dargestellt.
   
    ![Anzeigen der DAG-Visualisierung von Spark-Phasen](./media/hdinsight-apache-spark-job-debugging/view-spark-stages-dag-visualization.png)
   
    Mit DAG (Direct Aclyic Graph, gerichteter azyklischer Graph) werden die verschiedenen Phasen in der Anwendung dargestellt. Jedes blaue Feld im Diagramm stellt einen Spark-Vorgang dar, der von der Anwendung aufgerufen wurde.
5. Auf der Seite mit den Phasendetails können Sie auch die Zeitachsenansicht für die Anwendung starten. Erweitern Sie den Link **Event Timeline** oben auf der Seite, wie unten dargestellt.
   
    ![Anzeigen der Ereigniszeitachse von Spark-Phasen](./media/hdinsight-apache-spark-job-debugging/view-spark-stages-event-timeline.png)
   
    Dadurch werden die Spark-Ereignisse in Form einer Zeitachse angezeigt. Die Zeitachsenansicht ist auf drei Ebenen verfügbar: für Aufträge, innerhalb eines Auftrags und innerhalb einer Phase. In der Abbildung oben ist die Zeitachsenansicht für eine bestimmte Phase dargestellt.
   
   > [!TIP]
   > Wenn Sie das Kontrollkästchen **Enable zooming** aktivieren, können Sie in der Zeitachsenansicht einen Bildlauf nach links und rechts ausführen.
   > 
   > 
6. Andere Registerkarten der Spark-Benutzeroberfläche enthalten ebenfalls nützliche Informationen über die Spark-Instanz.
   
   * Registerkarte „Storage“: Wenn Ihre Anwendung RDDs erstellt, finden Sie auf der Registerkarte „Storage“ Informationen dazu.
   * Registerkarte „Environment“: Diese Registerkarte enthält viele nützliche Informationen zu Ihrer Spark-Instanz, z.B. die folgenden 
     * Scala-Version
     * Ereignisprotokollverzeichnis, das dem Cluster zugeordnet ist
     * Anzahl der Executorkerne für die Anwendung
     * und weitere

## <a name="find-information-about-completed-jobs-using-the-spark-history-server"></a>Anzeigen von Informationen zu abgeschlossenen Aufträgen mithilfe des Spark-Verlaufsservers
Wenn ein Auftrag abgeschlossen ist, werden die Informationen zum Auftrag auf dem Spark-Verlaufsserver beibehalten.

1. Klicken Sie zum Starten des Spark-Verlaufsservers auf dem Clusterblatt auf **Cluster-Dashboard** und dann auf **Spark-Verlaufsserver**.
   
    ![Starten des Spark-Verlaufsservers](./media/hdinsight-apache-spark-job-debugging/launch-spark-history-server.png)
   
   > [!TIP]
   > Alternativ können Sie die Benutzeroberfläche des Spark-Verlaufsservers auch über die Ambari-Benutzeroberfläche starten. Klicken Sie zum Starten der Ambari-Benutzeroberfläche auf dem Clusterblatt auf **Cluster-Dashboard** und dann auf **HDInsight-Cluster-Dashboard**. Klicken Sie auf der Ambari-Benutzeroberfläche auf **Spark**, klicken Sie auf **Quick Links**, und klicken Sie dann auf **Spark History Server UI** (UI des Spark-Verlaufsservers).
   > 
   > 
2. Eine Liste der abgeschlossenen Anwendungen wird angezeigt. Klicken Sie auf eine Anwendungs-ID, um weitere Informationen zur Anwendung anzuzeigen.
   
    ![Starten des Spark-Verlaufsservers](./media/hdinsight-apache-spark-job-debugging/view-completed-applications.png)

## <a name="see-also"></a>Weitere Informationen
*  [Verwalten von Ressourcen für den Apache Spark-Cluster in Azure HDInsight](hdinsight-apache-spark-resource-manager.md)

### <a name="for-data-analysts"></a>Für Datenanalysten

* [Spark mit Machine Learning: Analysieren von Gebäudetemperaturen mithilfe von Spark in HDInsight und HVAC-Daten](hdinsight-apache-spark-ipython-notebook-machine-learning.md)
* [Spark mit Machine Learning: Vorhersage von Lebensmittelkontrollergebnissen mithilfe von Spark in HDInsight](hdinsight-apache-spark-machine-learning-mllib-ipython.md)
* [Websiteprotokollanalyse mithilfe von Spark in HDInsight](hdinsight-apache-spark-custom-library-website-log-analysis.md)
* [Application Insight telemetry data analysis using Spark in HDInsight (Application Insight-Telemetriedatenanalyse mithilfe von Spark in HDInsight)](hdinsight-spark-analyze-application-insight-logs.md)
* [Verwenden von Caffe in Azure HDInsight Spark für verteiltes Deep Learning](hdinsight-deep-learning-caffe-spark.md)

### <a name="for-spark-developers"></a>Für Spark-Entwickler

* [Erstellen einer eigenständigen Anwendung mit Scala](hdinsight-apache-spark-create-standalone-application.md)
* [Remoteausführung von Aufträgen in einem Spark-Cluster mithilfe von Livy](hdinsight-apache-spark-livy-rest-interface.md)
* [Verwenden des HDInsight-Tools-Plug-Ins für IntelliJ IDEA zum Erstellen und Übermitteln von Spark Scala-Anwendungen](hdinsight-apache-spark-intellij-tool-plugin.md)
* [Spark-Streaming: Erstellen von Echtzeit-Streaminganwendungen mithilfe von Spark in HDInsight](hdinsight-apache-spark-eventhub-streaming.md)
* [Use HDInsight Tools Plugin for IntelliJ IDEA to debug Spark applications remotely (Verwenden von HDInsight-Tools-Plug-Ins für IntelliJ IDEA zum Remotedebuggen von Spark-Anwendungen)](hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely.md)
* [Verwenden von Zeppelin-Notebooks mit einem Spark-Cluster in HDInsight](hdinsight-apache-spark-zeppelin-notebook.md)
* [Verfügbare Kernels für Jupyter-Notebook im Spark-Cluster für HDInsight](hdinsight-apache-spark-jupyter-notebook-kernels.md)
* [Verwenden von externen Paketen mit Jupyter Notebooks](hdinsight-apache-spark-jupyter-notebook-use-external-packages.md)
* [Installieren von Jupyter Notebook auf Ihrem Computer und Herstellen einer Verbindung zum Apache Spark-Cluster in Azure HDInsight (Vorschau)](hdinsight-apache-spark-jupyter-notebook-install-locally.md)



