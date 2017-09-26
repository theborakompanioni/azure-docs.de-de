---
title: "Ausführen interaktiver Abfragen in einem Azure HDInsight Spark-Cluster | Microsoft-Dokumentation"
description: HDInsight Spark-Schnellstartanleitung zum Erstellen eines Apache Spark-Clusters in HDInsight.
keywords: Spark Schnellstart,interaktiv Spark,interaktive Abfrage,HDInsight Spark,Azure Spark
services: hdinsight
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/22/2017
ms.author: nitinme
ms.translationtype: HT
ms.sourcegitcommit: bfd49ea68c597b109a2c6823b7a8115608fa26c3
ms.openlocfilehash: ada1c3d1482c68834dbbf5eabbd045a7e0c01f9f
ms.contentlocale: de-de
ms.lasthandoff: 07/25/2017

---
# <a name="run-interactive-queries-on-an-hdinsight-spark-cluster"></a>Ausführen interaktiver Abfragen in einem HDInsight Spark-Cluster

In diesem Artikel verwenden Sie ein Jupyter Notebook, um interaktive Spark-SQL-Abfragen für einen Spark-Cluster auszuführen. Jupyter Notebook ist eine browserbasierte Anwendung, die die konsolenbasierte interaktive Oberfläche im Web erweitert. Weitere Informationen finden Sie unter [The Jupyter Notebook (Das Jupyter Notebok)](http://jupyter-notebook.readthedocs.io/en/latest/notebook.html).

Für dieses Tutorial verwenden Sie den **PySpark**-Kernel im Jupyter-Notebook, um eine interaktive Spark-SQL-Abfrage auszuführen. Jupyter-Notebooks auf HDInsight-Clustern unterstützen auch zwei andere Kernels: **PySpark3** und **Spark**. Weitere Informationen zu den Kernels und den Vorteilen von **PySpark**finden Sie unter [Kernel für Jupyter-Notebook in Spark-Clustern in Azure HDInsight](hdinsight-apache-spark-jupyter-notebook-kernels.md).

## <a name="prerequisites"></a>Voraussetzungen

* Ein **Azure HDInsight Spark-Cluster**. Eine Anleitung finden Sie unter [Erstellen eines Apache Spark-Clusters in Azure HDInsight](hdinsight-apache-spark-jupyter-spark-sql.md).

## <a name="create-a-jupyter-notebook-to-run-interactive-queries"></a>Erstellen eines Jupyter-Notebook zum Ausführen interaktiver Abfragen

Zum Ausführen von Abfragen verwenden wir Beispieldaten, die standardmäßig im mit dem Cluster verbundenen Speicher verfügbar sind. Allerdings müssen Sie die Daten zuerst als Datenrahmen in Spark laden. Wenn Sie den Datenrahmen haben, können Sie ihn mithilfe des Jupyter-Notebooks abfragen. In diesem Abschnitt betrachten wir Folgendes:

* Registrieren eines Stichprobendatasets als Spark-Datenrahmen
* Ausführen von Abfragen gegen den Datenrahmen

1. Öffnen Sie das [Azure-Portal](https://portal.azure.com/). Wenn Sie den Cluster am Dashboard angeheftet haben, klicken Sie im Dashboard auf die Clusterkachel , um das Clusterblatt aufzurufen.

    Wenn Sie den Cluster nicht am Dashboard angeheftet haben, klicken Sie im linken Bereich auf **HDInsight-Cluster** und dann auf den von Ihnen erstellten Cluster.

3. Klicken Sie unter **Quicklinks** auf **Clusterdashboards** und dann auf **Jupyter Notebook**. Geben Sie die Administratoranmeldeinformationen für den Cluster ein, wenn Sie dazu aufgefordert werden.

   ![Öffnen von Jupyter Notebook zum Ausführen einer interaktiven Spark SQL-Abfrage](./media/hdinsight-apache-spark-load-data-run-query/hdinsight-spark-start-jupyter-interactive-spark-sql-query.png "Öffnen von Jupyter Notebook zum Ausführen einer interaktiven Spark SQL-Abfrage")

   > [!NOTE]
   > Sie können auch auf das Jupyter Notebook für Ihren Cluster zugreifen, indem Sie in Ihrem Browser die folgende URL öffnen. Ersetzen Sie **CLUSTERNAME** durch den Namen Ihres Clusters:
   >
   > `https://CLUSTERNAME.azurehdinsight.net/jupyter`
   >
   >
3. Erstellen Sie ein Notebook. Klicken Sie auf **Neu** und dann auf **PySpark**.

   ![Erstellen eines Jupyter Notebooks zum Ausführen einer interaktiven Spark SQL-Abfrage](./media/hdinsight-apache-spark-load-data-run-query/hdinsight-spark-create-jupyter-interactive-Spark-SQL-query.png "Erstellen eines Jupyter Notebooks zum Ausführen einer interaktiven Spark SQL-Abfrage")

   Ein neues Notebook mit dem Namen „Untitled“ (Untitled.pynb) wird erstellt und geöffnet.

4. Klicken Sie oben auf den Namen des Notebooks, und geben Sie bei Bedarf einen Anzeigenamen ein.

    ![Eingeben eines Namens für das Jupyter Notebook, aus dem eine interaktive Spark-Abfrage ausgeführt werden soll](./media/hdinsight-apache-spark-load-data-run-query/hdinsight-spark-jupyter-notebook-name.png "Eingeben eines Namens für das Jupyter Notebook, aus dem eine interaktive Spark-Abfrage ausgeführt werden soll")

5. Fügen Sie den folgenden Code in eine leere Zelle ein, und drücken Sie **UMSCHALT+EINGABE** , um den Code auszuführen. Mit dem Code werden die Typen importiert, die für dieses Szenario benötigt werden:

        from pyspark.sql import *
        from pyspark.sql.types import *

    Da Sie ein Notebook mit dem PySpark-Kernel erstellt haben, müssen Sie keine Kontexte explizit erstellen. Die Spark- und Hive-Kontexte werden automatisch für Sie erstellt, wenn Sie die erste Codezelle ausführen.

    ![Status der interaktiven Spark SQL-Abfrage](./media/hdinsight-apache-spark-load-data-run-query/hdinsight-spark-interactive-spark-query-status.png "Status der interaktiven Spark SQL-Abfrage")

    Bei jedem Ausführen einer interaktiven Abfrage in Jupyter wird in der Titelleiste Ihres Webbrowserfensters neben dem Notebooktitel der Status **(Beschäftigt)** angezeigt. Außerdem sehen Sie in der rechten oberen Ecke einen ausgefüllten Kreis neben dem Text **PySpark**. Wenn der Auftrag abgeschlossen ist, wird ein Kreis ohne Füllung angezeigt.

6. Lassen Sie uns vor dem Laden der Daten in ein Spark-Cluster eine Momentaufnahme davon ansehen. Die in diesem Tutorial verwendeten Beispieldaten stehen als CSV-Datei auf allen HDInsight-Spark-Clustern unter **\HdiSamples\HdiSamples\SensorSampleData\hvac\hvac.csv** zur Verfügung. Die Daten erfassen die Temperaturunterschiede in einem Gebäude. Dies sind die ersten Datenzeilen.

    ![Momentaufnahme der Daten für die interaktive Spark-SQL-Abfrage](./media/hdinsight-apache-spark-load-data-run-query/hdinsight-spark-sample-data-interactive-spark-sql-query.png "Momentaufnahme der Daten für die interaktive Spark-SQL-Abfrage")

6. Erstellen Sie einen Datenrahmen und eine temporäre Tabelle (**hvac**), indem Sie den folgenden Code ausführen. Für dieses Tutorial erstellen wir nicht alle Spalten in der temporären Tabelle im Vergleich zu den Spalten in den CSV-Rohdaten. 

        # Create an RDD from sample data
        hvacText = sc.textFile("wasbs:///HdiSamples/HdiSamples/SensorSampleData/hvac/HVAC.csv")

        # Create a schema for our data
        Entry = Row('Date', 'Time', 'TargetTemp', 'ActualTemp', 'BuildingID')

        # Parse the data and create a schema
        hvacParts = hvacText.map(lambda s: s.split(',')).filter(lambda s: s[0] != 'Date')
        hvac = hvacParts.map(lambda p: Entry(str(p[0]), str(p[1]), int(p[2]), int(p[3]), int(p[6])))
        
        # Infer the schema and create a table       
        hvacTable = sqlContext.createDataFrame(hvac)
        hvacTable.registerTempTable('hvactemptable')
        dfw = DataFrameWriter(hvacTable)
        dfw.saveAsTable('hvac')

7. Wenn Sie die Tabelle erstellt haben, führen Sie die interaktive Abfrage gegen die Daten mithilfe des folgenden Codes aus.

        %%sql
        SELECT buildingID, (targettemp - actualtemp) AS temp_diff, date FROM hvac WHERE date = \"6/1/13\"

   Da Sie einen PySpark-Kernel verwenden, können Sie jetzt direkt eine interaktive SQL-Abfrage für die temporäre Tabelle **hvac** ausführen, die Sie mit der Magic `%%sql` erstellt haben. Weitere Informationen zur `%%sql`-Magic sowie anderen für den PySpark-Kernel verfügbaren Magics finden Sie unter [Verfügbare Kernels für Jupyter Notebooks mit Spark-Clustern unter HDInsight](hdinsight-apache-spark-jupyter-notebook-kernels.md#parameters-supported-with-the-sql-magic).

   Die folgende Ausgabe in Tabellenform wird standardmäßig angezeigt.

     ![Tabellenausgabe des Ergebnisses der interaktiven Spark-Abfrage](./media/hdinsight-apache-spark-load-data-run-query/hdinsight-interactive-spark-query-result.png "Tabellenausgabe des Ergebnisses der interaktiven Spark-Abfrage")

    Sie können die Ergebnisse auch in anderen Visualisierungen anzeigen. Beispielsweise würde ein Bereichsdiagramm für dieselbe Ausgabe wie folgt aussehen.

    ![Bereichsdiagramm des Ergebnisses der interaktiven Spark-Abfrage](./media/hdinsight-apache-spark-load-data-run-query/hdinsight-interactive-spark-query-result-area-chart.png "Bereichsdiagramm des Ergebnisses der interaktiven Spark-Abfrage")

9. Fahren Sie das Notebook nach dem Ausführen der Anwendung herunter, um die Clusterressourcen freizugeben. Klicken Sie hierzu im Menü **Datei** des Notebooks auf die Option zum **Schließen und Anhalten**.

## <a name="next-step"></a>Nächster Schritt

In diesem Artikel haben Sie erfahren, wie Sie interaktive Abfragen in Spark mithilfe von Jupyter-Notebook ausführen. Fahren Sie mit dem nächsten Artikel fort, um festzustellen, wie die Daten, die Sie in Spark registriert haben, in ein BI-Analyse-Tool wie Power BI und Tableau gezogen werden können. 

> [!div class="nextstepaction"]
>[Apache Spark BI mit Datenvisualisierungstools unter Azure HDInsight](hdinsight-apache-spark-use-bi-tools.md)





