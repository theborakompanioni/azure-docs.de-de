---
title: "Erste Schritte: Apache Spark und interaktive Spark SQL-Abfragen – Azure HDInsight | Microsoft-Dokumentation"
description: "HDInsight Spark-Schnellstart zum Erstellen eines Apache Spark-Clusters in HDInsight und zum Ausführen einer interaktiven Abfrage mithilfe von Jupyter Notebooks."
keywords: Spark Schnellstart,interaktiv Spark,interaktive Abfrage,HDInsight Spark,Azure Spark
services: hdinsight
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 91f41e6a-d463-4eb4-83ef-7bbb1f4556cc
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 05/25/2017
ms.author: nitinme
ms.translationtype: Human Translation
ms.sourcegitcommit: fc4172b27b93a49c613eb915252895e845b96892
ms.openlocfilehash: 0c59792423bfe2848ab5773746db466890228ddc
ms.contentlocale: de-de
ms.lasthandoff: 05/12/2017


---
# <a name="get-started-create-an-apache-spark-cluster-in-hdinsight-and-run-interactive-spark-sql-queries"></a>Erste Schritte: Erstellen eines Apache Spark-Clusters in HDInsight und Ausführen interaktiver Spark SQL-Abfragen

Erfahren Sie, wie Sie einen [Apache Spark](hdinsight-apache-spark-overview.md)-Cluster in HDInsight erstellen und interaktive Spark SQL-Abfragen mit [Jupyter](https://jupyter.org) Notebooks ausführen.

   ![Schnellstartdiagramm mit einer Beschreibung der Schritte zum Erstellen eines Apache Spark-Clusters unter Azure HDInsight](./media/hdinsight-apache-spark-jupyter-spark-sql/hdinsight-spark-quickstart-interactive-spark-query-flow.png "Spark-Schnellstart mit Apache Spark in HDInsight. Dargestellte Schritte: Erstellen eines Clusters; Ausführen einer interaktiven Spark-Abfrage")

## <a name="prerequisites"></a>Voraussetzungen

* **Ein Azure-Abonnement**. Für dieses Tutorial wird ein Azure-Abonnement benötigt. Weitere Informationen finden Sie unter [Erstellen Sie noch heute Ihr kostenloses Azure-Konto](https://azure.microsoft.com/free).

## <a name="create-hdinsight-spark-cluster"></a>Erstellen eines HDInsight Spark-Clusters

In diesem Abschnitt erstellen Sie einen HDInsight Spark-Cluster mit einer [Azure Resource Manager-Vorlage](https://azure.microsoft.com/resources/templates/101-hdinsight-spark-linux/). Andere Methoden zur Erstellung von Clustern finden Sie unter [Erstellen von Linux-basierten Hadoop-Clustern in HDInsight](hdinsight-hadoop-provision-linux-clusters.md).

1. Klicken Sie auf die folgende Abbildung, um die Vorlage im Azure-Portal zu öffnen:         

    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-hdinsight-spark-linux%2Fazuredeploy.json" target="_blank"><img src="./media/hdinsight-apache-spark-jupyter-spark-sql/deploy-to-azure.png" alt="Deploy to Azure"></a>

2. Geben Sie die folgenden Werte ein:

    ![Erstellen eines HDInsight Spark-Clusters mit einer Azure Resource Manager-Vorlage](./media/hdinsight-apache-spark-jupyter-spark-sql/create-spark-cluster-in-hdinsight-using-azure-resource-manager-template.png "Erstellen eines Spark-Clusters in HDInsight mit einer Azure Resource Manager-Vorlage")

    * **Abonnement**: Wählen Sie Ihr Azure-Abonnement für diesen Cluster aus.
    * **Ressourcengruppe**: Erstellen Sie eine Ressourcengruppe, oder wählen Sie eine vorhandene aus. Die Ressourcengruppe wird verwendet, um Azure-Ressourcen für Ihre Projekte zu verwalten.
    * **Standort**: Wählen Sie einen Standort für die Ressourcengruppe aus.  Dieser Standort wird auch für den Standardspeicher des Clusters und den HDInsight-Cluster verwendet.
    * **Clustername**: Geben Sie einen Namen für den Hadoop-Cluster ein, den Sie erstellen.
    * **Spark-Version**: Wählen Sie die Spark-Version aus, die Sie für den Cluster installieren möchten.
    * **Cluster-Benutzername und -Kennwort**: Der Standardname für die Anmeldung lautet „admin“.
    * **SSH-Benutzername und -Kennwort**.

   Notieren Sie diese Werte.  Sie werden später im Lernprogramm benötigt.

3. Aktivieren Sie die Optionen **Ich stimme den oben genannten Geschäftsbedingungen zu** und **An Dashboard anheften**, und klicken Sie anschließend auf **Kaufen**. Daraufhin wird eine neue Kachel mit der Bezeichnung „Bereitstellung für Vorlagenbereitstellung wird gesendet“ angezeigt. Das Erstellen des Clusters dauert ca. 20 Minuten.

> [!NOTE]
> In diesem Artikel wird ein Spark-Cluster erstellt, der [Azure Storage-Blobs als Clusterspeicher](hdinsight-hadoop-use-blob-storage.md) einsetzt. Sie können auch einen Spark-Cluster erstellen, der neben Azure Storage-Blobs als Standardspeicher [Azure Data Lake Store](../data-lake-store/data-lake-store-overview.md) als zusätzlichen Speicher verwendet. Anweisungen hierzu finden Sie unter [Erstellen eines HDInsight-Clusters mit Data Lake-Speicher](../data-lake-store/data-lake-store-hdinsight-hadoop-use-portal.md).
>
>

## <a name="run-an-interactive-spark-sql-query"></a>Ausführen einer interaktiven Spark SQL-Abfrage

In diesem Abschnitt verwenden Sie Jupyter Notebook, um interaktive Spark SQL-Abfragen für den zuvor erstellten Spark-Cluster auszuführen. HDInsight Spark-Cluster bieten drei Kernel, die Sie für Jupyter Notebook verwenden können. Diese sind wie folgt:

* **PySpark** (für in Python geschriebene Anwendungen)
* **PySpark3** (für in Python3 geschriebene Anwendungen)
* **Spark** (für in Scala geschriebene Anwendungen)

In diesem Artikel verwenden Sie den **PySpark**-Kernel in dem Notebook, aus dem Sie die interaktive Spark SQL-Abfrage ausführen. Weitere Informationen zu den Kernels finden Sie unter [Verfügbare Kernels für Jupyter Notebooks mit Apache Spark-Clustern unter HDInsight (Linux)](hdinsight-apache-spark-jupyter-notebook-kernels.md). Zentrale Vorteile des PySpark-Kernels:

* Die Kontexte für Spark und Hive werden automatisch festgelegt.
* Mithilfe von Zellen-Magics wie `%%sql` können Sie interaktive SQL- und Hive-Abfragen direkt (also ohne vorherige Codeausschnitte) ausführen.
* Die Ausgabe der interaktiven Abfragen wird automatisch angezeigt.

### <a name="create-jupyter-notebook-with-pyspark-kernel"></a>Erstellen von Jupyter Notebook mit PySpark-Kernel

1. Öffnen Sie das [Azure-Portal](https://portal.azure.com/).

2. Wenn Sie den Cluster am Dashboard angeheftet haben, klicken Sie im Dashboard auf die Clusterkachel , um das Clusterblatt aufzurufen.

    Wenn Sie den Cluster nicht am Dashboard angeheftet haben, klicken Sie im linken Bereich auf **HDInsight-Cluster** und dann auf den von Ihnen erstellten Cluster.

3. Klicken Sie unter **Quicklinks** auf **Clusterdashboards** und dann auf **Jupyter Notebook**. Geben Sie die Administratoranmeldeinformationen für den Cluster ein, wenn Sie dazu aufgefordert werden.

   ![Öffnen von Jupyter Notebook zum Ausführen einer interaktiven Spark SQL-Abfrage](./media/hdinsight-apache-spark-jupyter-spark-sql/hdinsight-spark-open-jupyter-interactive-spark-sql-query.png "Öffnen von Jupyter Notebook zum Ausführen einer interaktiven Spark SQL-Abfrage")

   > [!NOTE]
   > Sie können auch auf das Jupyter Notebook für Ihren Cluster zugreifen, indem Sie in Ihrem Browser die folgende URL öffnen. Ersetzen Sie **CLUSTERNAME** durch den Namen Ihres Clusters:
   >
   > `https://CLUSTERNAME.azurehdinsight.net/jupyter`
   >
   >
3. Erstellen Sie ein Notebook. Klicken Sie auf **Neu** und dann auf **PySpark**.

   ![Erstellen eines Jupyter Notebooks zum Ausführen einer interaktiven Spark SQL-Abfrage](./media/hdinsight-apache-spark-jupyter-spark-sql/hdinsight-spark-create-jupyter-interactive-Spark-SQL-query.png "Erstellen eines Jupyter Notebooks zum Ausführen einer interaktiven Spark SQL-Abfrage")

   Ein neues Notebook mit dem Namen „Untitled“ (Untitled.pynb) wird erstellt und geöffnet.

4. Klicken Sie oben auf den Namen des Notebooks, und geben Sie bei Bedarf einen Anzeigenamen ein.

    ![Eingeben eines Namens für das Jupyter Notebook, aus dem eine interaktive Spark-Abfrage ausgeführt werden soll](./media/hdinsight-apache-spark-jupyter-spark-sql/hdinsight-spark-jupyter-notebook-name.png "Eingeben eines Namens für das Jupyter Notebook, aus dem eine interaktive Spark-Abfrage ausgeführt werden soll")

5. Fügen Sie den folgenden Code in eine leere Zelle ein, und drücken Sie UMSCHALT+EINGABE****, um den Code auszuführen. Mit dem Code werden die Typen importiert, die für dieses Szenario benötigt werden:

        from pyspark.sql.types import *

    Da Sie ein Notebook mit dem PySpark-Kernel erstellt haben, müssen Sie keine Kontexte explizit erstellen. Die Spark- und Hive-Kontexte werden automatisch für Sie erstellt, wenn Sie die erste Codezelle ausführen.

    ![Status der interaktiven Spark SQL-Abfrage](./media/hdinsight-apache-spark-jupyter-spark-sql/hdinsight-spark-interactive-spark-query-status.png "Status der interaktiven Spark SQL-Abfrage")

    Bei jedem Ausführen einer interaktiven Abfrage in Jupyter wird in der Titelleiste Ihres Webbrowserfensters neben dem Notebooktitel der Status **(Beschäftigt)** angezeigt. Außerdem sehen Sie in der rechten oberen Ecke einen ausgefüllten Kreis neben dem Text **PySpark**. Wenn der Auftrag abgeschlossen ist, wird ein Kreis ohne Füllung angezeigt.

6. Registrieren Sie ein Beispieldataset als temporäre Tabelle (**hvac**), indem Sie den folgenden Code ausführen.

        # Load the data
        hvacText = sc.textFile("wasbs:///HdiSamples/HdiSamples/SensorSampleData/hvac/HVAC.csv")

        # Create the schema
        hvacSchema = StructType([StructField("date", StringType(), False),StructField("time", StringType(), False),StructField("targettemp", IntegerType(), False),StructField("actualtemp", IntegerType(), False),StructField("buildingID", StringType(), False)])

        # Parse the data in hvacText
        hvac = hvacText.map(lambda s: s.split(",")).filter(lambda s: s[0] != "Date").map(lambda s:(str(s[0]), str(s[1]), int(s[2]), int(s[3]), str(s[6]) ))

        # Create a data frame
        hvacdf = sqlContext.createDataFrame(hvac,hvacSchema)

        # Register the data frame as a table to run queries against
        hvacdf.registerTempTable("hvac")

    Spark-Cluster in HDInsight verfügen über die Beispieldatendatei **hvac.csv** unter **\HdiSamples\HdiSamples\SensorSampleData\hvac**.

7. Verwenden Sie zum Ausführen von interaktiven Abfragen für die Daten den folgenden Code:

        %%sql
        SELECT buildingID, (targettemp - actualtemp) AS temp_diff, date FROM hvac WHERE date = \"6/1/13\"

   Da Sie einen PySpark-Kernel verwenden, können Sie jetzt direkt eine interaktive SQL-Abfrage für die temporäre Tabelle **hvac** ausführen, die Sie mit der Magic `%%sql` erstellt haben. Weitere Informationen zur `%%sql`-Magic sowie anderen für den PySpark-Kernel verfügbaren Magics finden Sie unter [Verfügbare Kernels für Jupyter Notebooks mit Spark-Clustern unter HDInsight](hdinsight-apache-spark-jupyter-notebook-kernels.md#parameters-supported-with-the-sql-magic).

   Die folgende Ausgabe in Tabellenform wird standardmäßig angezeigt.

     ![Tabellenausgabe des Ergebnisses der interaktiven Spark-Abfrage](./media/hdinsight-apache-spark-jupyter-spark-sql/hdinsight-interactive-spark-query-result.png "Tabellenausgabe des Ergebnisses der interaktiven Spark-Abfrage")

    Sie können die Ergebnisse auch in anderen Visualisierungen anzeigen. Beispielsweise würde ein Bereichsdiagramm für dieselbe Ausgabe wie folgt aussehen.

    ![Bereichsdiagramm des Ergebnisses der interaktiven Spark-Abfrage](./media/hdinsight-apache-spark-jupyter-spark-sql/hdinsight-interactive-spark-query-result-area-chart.png "Bereichsdiagramm des Ergebnisses der interaktiven Spark-Abfrage")

9. Fahren Sie das Notebook nach dem Ausführen der Anwendung herunter, um die Clusterressourcen freizugeben. Klicken Sie hierzu im Menü **Datei** des Notebooks auf die Option zum **Schließen und Anhalten**.

## <a name="delete-the-cluster"></a>Löschen des Clusters
[!INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

## <a name="troubleshoot-access-control"></a>Problembehandlung für die Zugriffssteuerung

Falls beim Erstellen von HDInsight-Clustern ein Problem auftritt, sehen Sie sich die [Voraussetzungen für die Zugriffssteuerung](hdinsight-administer-use-portal-linux.md#create-clusters) an.

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

