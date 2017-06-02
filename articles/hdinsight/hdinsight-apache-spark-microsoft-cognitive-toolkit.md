---
title: "Microsoft Cognitive Toolkit mit Azure HDInsight Spark für intensives Lernen | Microsoft-Dokumentation"
description: "Erfahren Sie, wie ein trainiertes Microsoft Cognitive Toolkit-Modell für intensives Lernen mithilfe der Spark Python-API in einem Azure HDInsight Spark-Cluster auf ein Dataset angewendet werden kann."
services: hdinsight
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.service: hdinsight
ms.custom: hdinsightactive
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/25/2017
ms.author: nitinme
ms.translationtype: Human Translation
ms.sourcegitcommit: 9ae7e129b381d3034433e29ac1f74cb843cb5aa6
ms.openlocfilehash: b0d57fcef9eda57309d27f214edd184a38a44481
ms.contentlocale: de-de
ms.lasthandoff: 05/08/2017


---
# <a name="use-microsoft-cognitive-toolkit-deep-learning-model-with-azure-hdinsight-spark-cluster"></a>Verwenden des Microsoft Cognitive Toolkit-Modells für intensives Lernen mit einem Azure HDInsight Spark-Cluster

In diesem Artikel führen Sie die folgenden Schritte aus.

1. Führen Sie ein benutzerdefiniertes Skript zum Installieren von Microsoft Cognitive Toolkit in einem Azure HDInsight Spark-Cluster aus.

2. Laden Sie ein Jupyter Notebook in den Spark-Cluster hoch, um zu erfahren, wie ein trainiertes Microsoft Cognitive Toolkit-Modell für intensives Lernen mit der [Spark Python-API (PySpark)](https://spark.apache.org/docs/0.9.0/python-programming-guide.html) auf Dateien in einem Azure Blob Storage-Konto angewandt wird.

## <a name="prerequisites"></a>Voraussetzungen

* **Ein Azure-Abonnement**. Für dieses Tutorial wird ein Azure-Abonnement benötigt. Weitere Informationen finden Sie unter [Erstellen Sie noch heute Ihr kostenloses Azure-Konto](https://azure.microsoft.com/free).

* **Azure HDInsight Spark-Cluster**. Erstellen Sie für diesen Artikel einen Spark 2.0-Cluster. Eine Anleitung finden Sie unter [Erste Schritte: Erstellen eines Apache Spark-Clusters für Azure HDInsight und Ausführen von interaktiven Abfragen per Spark-SQL](hdinsight-apache-spark-jupyter-spark-sql.md).

## <a name="how-does-this-solution-flow"></a>Wir läuft diese Lösung ab?

Diese Lösung ist auf diesen Artikel und ein Jupyter Notebook aufgeteilt, das Sie als Teil dieses Tutorials hochladen können. In diesem Artikel führen Sie die folgenden Schritte aus:

* Ausführen einer Skriptaktion in einem HDInsight Spark-Cluster zum Installieren von Microsoft Cognitive Toolkit und Python-Paketen.
* Hochladen des Jupyter Notebooks, das die Lösung im HDInsight Spark-Cluster ausführt.

Die folgenden übrigen Schritte werden vom Jupyter Notebook abgedeckt.

- Laden von Beispielimages in ein Spark Resiliant Distributed Dataset oder RDD
   - Laden von Modulen und Definieren von Voreinstellungen
   - Lokales Herunterladen des Datasets in den Spark-Cluster
   - Konvertieren des Datasets in ein RDD
- Bewerten der Images mithilfe eines trainierten Cognitive Toolkit-Modells
   - Herunterladen des trainierten Cognitive Toolkit-Modells in den Spark-Cluster
   - Definieren von Funktionen, die von Workerknoten verwendet werden sollen
   - Bewerten der Images auf Workerknoten
   - Auswerten der Modellgenauigkeit


## <a name="install-microsoft-cognitive-toolkit"></a>Installieren von Microsoft Cognitive Toolkit

Sie können Microsoft Cognitive Toolkit mithilfe einer Skriptaktion in einem Spark-Cluster installieren. Skriptaktionen verwenden benutzerdefinierte Skripts zum Installieren von Komponenten im Cluster, die nicht standardmäßig verfügbar sind. Sie können das benutzerdefinierte Skript aus dem Azure-Portal verwenden. Verwenden Sie dazu das HDInsight .NET SDK oder Azure PowerShell. Sie können das Skript auch verwenden, um das Toolkit entweder als Teil der Clustererstellung zu installieren, oder sobald der Cluster ausgeführt wird. 

In diesem Artikel verwenden wir das Portal, um das Toolkit installieren, nachdem der Cluster erstellt wurde. Andere Möglichkeiten zum Ausführen des benutzerdefinierten Skripts finden Sie unter [Anpassen Linux-basierter HDInsight-Cluster mithilfe von Skriptaktionen](hdinsight-hadoop-customize-cluster-linux.md).

### <a name="using-the-azure-portal"></a>Verwenden des Azure-Portals

Eine Anleitung zur Verwendung des Azure-Portals zum Ausführen der Skriptaktion finden Sie unter [Anpassen von HDInsight-Clustern mithilfe von Skriptaktion](hdinsight-hadoop-customize-cluster-linux.md#use-a-script-action-during-cluster-creation). Stellen Sie sicher, dass Sie die folgenden Eingaben zum Installieren von Microsoft Cognitive Toolkit bereitstellen.

* Geben Sie einen Wert für den Skriptaktionsnamen ein.

* Geben Sie für **Bash-Skript-URI** `https://raw.githubusercontent.com/Azure-Samples/hdinsight-pyspark-cntk-integration/master/cntk-install.sh` ein.

* Sie dürfen das Skript nur auf dem Haupt- und den Workerknoten ausführen. Deaktivieren Sie alle anderen Kontrollkästchen.

* Klicken Sie auf **Erstellen**.

## <a name="upload-the-jupyter-notebook-to-azure-hdinsight-spark-cluster"></a>Hochladen des Jupyter Notebooks in den Azure HDInsight Spark-Cluster

Um das Microsoft Cognitive Toolkit mit dem Azure HDInsight Spark-Cluster zu verwenden, müssen Sie das Jupyter Notebook **CNTK_model_scoring_on_Spark_walkthrough.ipynb** in den Azure HDInsight Spark-Cluster hochladen. Dieses Notebook steht auf GitHub unter [https://github.com/Azure-Samples/hdinsight-pyspark-cntk-integration](https://github.com/Azure-Samples/hdinsight-pyspark-cntk-integration) zur Verfügung.

1. Klonen Sie das GitHub-Repository [https://github.com/Azure-Samples/hdinsight-pyspark-cntk-integration](https://github.com/Azure-Samples/hdinsight-pyspark-cntk-integration). Anleitungen zum Klonen finden Sie unter [Cloning a repository](https://help.github.com/articles/cloning-a-repository/) (Klonen eines Repositorys).

2. Öffnen Sie im Azure-Portal das Blatt des Spark-Clusters, den Sie bereits bereitgestellt haben, und klicken Sie auf **Cluster-Dashboard** und dann auf **Jupyter Notebook**.

    Sie können das Jupyter Notebook auch starten, indem Sie zu der URL `https://<clustername>.azurehdinsight.net/jupyter/` navigieren. Ersetzen Sie \<clustername> durch den Namen Ihres HDInsight-Clusters.

3. Klicken Sie im Jupyter Notebook in der oberen rechten Ecke auf **Hochladen**, und navigieren Sie zu dem Speicherort, in dem Sie das GitHub-Repository geklont haben.

    ![Hochladen des Jupyter Notebooks in den Azure HDInsight Spark-Cluster](./media/hdinsight-apache-spark-microsoft-cognitive-toolkit/hdinsight-microsoft-cognitive-toolkit-load-jupyter-notebook.png "Hochladen des Jupyter Notebooks in den Azure HDInsight Spark-Cluster")

4. Klicken Sie erneut auf **Hochladen**.

5. Sobald das Notebook hochgeladen ist, klicken Sie auf den Namen des Notebooks, folgen Sie den Anweisungen im Notebook zum Laden des Datasets, und führen Sie das Tutorial aus.

## <a name="see-also"></a>Weitere Informationen
* [Übersicht: Apache Spark in Azure HDInsight](hdinsight-apache-spark-overview.md)

### <a name="scenarios"></a>Szenarios
* [Spark mit BI: Durchführen interaktiver Datenanalysen mithilfe von Spark in HDInsight mit BI-Tools](hdinsight-apache-spark-use-bi-tools.md)
* [Spark mit Machine Learning: Analysieren von Gebäudetemperaturen mithilfe von Spark in HDInsight und HVAC-Daten](hdinsight-apache-spark-ipython-notebook-machine-learning.md)
* [Spark mit Machine Learning: Vorhersage von Lebensmittelkontrollergebnissen mithilfe von Spark in HDInsight](hdinsight-apache-spark-machine-learning-mllib-ipython.md)
* [Spark-Streaming: Erstellen von Echtzeitstreaminganwendungen mithilfe von Spark in HDInsight](hdinsight-apache-spark-eventhub-streaming.md)
* [Websiteprotokollanalyse mithilfe von Spark in HDInsight](hdinsight-apache-spark-custom-library-website-log-analysis.md)
* [Application Insight telemetry data analysis using Spark in HDInsight (Application Insight-Telemetriedatenanalyse mithilfe von Spark in HDInsight)](hdinsight-spark-analyze-application-insight-logs.md)

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
* [Verwalten von Ressourcen für den Apache Spark-Cluster in Azure HDInsight](hdinsight-apache-spark-resource-manager.md)
* [Track and debug jobs running on an Apache Spark cluster in HDInsight(Nachverfolgen und Debuggen von Aufträgen in einem Apache Spark-Cluster unter HDInsight)](hdinsight-apache-spark-job-debugging.md)

[hdinsight-versions]: hdinsight-component-versioning.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-storage]: hdinsight-hadoop-use-blob-storage.md

[azure-purchase-options]: http://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: http://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[azure-create-storageaccount]: storage-create-storage-account.md

