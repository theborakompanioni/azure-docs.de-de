---
title: "Verwenden von BI-Tools mit Apache Spark für HDInsight | Microsoft-Dokumentation"
description: "Schritt-für-Schritt-Anleitung zur Verwendung von Notebooks mit Apache Spark zum Erstellen von Schemas für Rohdaten, Speichern als Tabellen und Verwenden von BI-Tools in der Tabelle zur Datenanalyse"
services: hdinsight
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 1448b536-9bc8-46bc-bbc6-d7001623642a
ms.service: hdinsight
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/13/2017
ms.author: nitinme
translationtype: Human Translation
ms.sourcegitcommit: a087df444c5c88ee1dbcf8eb18abf883549a9024
ms.openlocfilehash: ba238a5d18dd83e4adb9e5ba737ec0cff135d34e
ms.lasthandoff: 03/15/2017


---
# <a name="use-bi-tools-with-apache-spark-cluster-on-azure-hdinsight"></a>Verwenden von BI-Tools mit Apache Spark-Cluster unter Azure HDInsight

Es wird beschrieben, wie Sie Apache Spark in Azure HDInsight verwenden, um ein Beispieldataset mit unformatierten Daten zu analysieren, und dann BI-Tools zum Visualisieren der Daten nutzen. In diesem Artikel wird veranschaulicht, wie Sie BI-Tools, z.B. Power BI und Tableau, für HDInsight Spark-Cluster einsetzen. 

> [!NOTE]
> Die in diesem Artikel beschriebenen Verbindungen mit BI-Tools werden unter Spark 2.1 in der Vorschau von Azure HDInsight 3.6 nicht unterstützt. Nur die Spark-Versionen 1.6 und 2.0 (HDInsight 3.4 bzw. 3.5) werden unterstützt.
>

Dieses Tutorial steht auch als Jupyter Notebook für einen Spark-Cluster (Linux) zur Verfügung, den Sie in HDInsight erstellen. In der Notebook-Umgebung können Sie die Python-Ausschnitte direkt im Notebook ausführen. Wenn Sie das Tutorial innerhalb eines Notebooks ausführen möchten, erstellen Sie einen Spark-Cluster, starten Sie ein Jupyter Notebook (`https://CLUSTERNAME.azurehdinsight.net/jupyter`), und führen Sie dann das Notebook **Use BI tools with Apache Spark on HDInsight.ipynb** im Ordner **Python** aus.

## <a name="prerequisites"></a>Voraussetzungen

* Ein Azure-Abonnement. Siehe [How to get Azure Free trial for testing Hadoop in HDInsight](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/)(in englischer Sprache).
* Ein Apache Spark-Cluster unter HDInsight. Eine Anleitung finden Sie unter [Erstellen von Apache Spark-Clustern in Azure HDInsight](hdinsight-apache-spark-jupyter-spark-sql.md).


## <a name="hivetable"></a>Speichern von Rohdaten als Tabelle

In diesem Abschnitt verwenden wir das [Jupyter](https://jupyter.org) Notebook aus einem HDInsight Spark-Cluster zum Ausführen von Aufträgen, bei denen Ihre Beispielrohdaten verarbeitet und dann als Tabelle gespeichert werden. Die Beispieldaten sind in einer CSV-Datei (hvac.csv) enthalten, die standardmäßig auf allen Clustern verfügbar ist.

Nachdem Ihre Daten als Tabelle gespeichert wurden, verwenden wir im nächsten Abschnitt BI-Tools, um eine Verbindung mit der Tabelle herzustellen und weitere Visualisierungen durchzuführen.

1. Klicken Sie im [Azure-Portal](https://portal.azure.com/)im Startmenü auf die Kachel für Ihren Spark-Cluster (sofern Sie die Kachel ans Startmenü angeheftet haben). Sie können auch unter **Alle durchsuchen** > **HDInsight-Cluster** zu Ihrem Cluster navigieren.   
2. Klicken Sie auf dem Blatt für den Spark-Cluster auf **Cluster-Dashboard** und anschließend auf **Jupyter Notebook**. Geben Sie die Administratoranmeldeinformationen für den Cluster ein, wenn Sie dazu aufgefordert werden.

   > [!NOTE]
   > Sie können auch das Jupyter Notebook für Ihren Cluster aufrufen, indem Sie in Ihrem Browser die folgende URL öffnen. Ersetzen Sie **CLUSTERNAME** durch den Namen Ihres Clusters:
   >
   > `https://CLUSTERNAME.azurehdinsight.net/jupyter`
   >
   >

3. Erstellen Sie ein Notebook. Klicken Sie auf **Neu** und dann auf **PySpark**.

    ![Erstellen eines Jupyter Notebooks](./media/hdinsight-apache-spark-use-bi-tools/hdispark.note.jupyter.createnotebook.png "Erstellen eines Jupyter Notebooks")

4. Ein neues Notebook mit dem Namen „Untitled.pynb“ wird erstellt und geöffnet. Klicken Sie oben auf den Namen des Notebooks, und geben Sie einen Anzeigenamen ein.

    ![Angeben eines neuen Namens für das Notebook](./media/hdinsight-apache-spark-use-bi-tools/hdispark.note.jupyter.notebook.name.png "Angeben eines neuen Namens für das Notebook")

5. Da Sie ein Notebook mit dem PySpark-Kernel erstellt haben, müssen Sie keine Kontexte explizit erstellen. Die Spark- und Hive-Kontexte werden automatisch für Sie erstellt, wenn Sie die erste Codezelle ausführen. Sie können zunächst die Typen importieren, die für dieses Szenario erforderlich sind. Setzen Sie dazu den Cursor in die Zelle, und drücken Sie **UMSCHALT- + EINGABETASTE**.

        from pyspark.sql import *

6. Laden Sie Beispieldaten in eine temporäre Tabelle. Wenn Sie einen Spark-Cluster in HDInsight erstellen, wird die Beispieldatendatei **hvac.csv** in das zugeordnete Speicherkonto unter **\HdiSamples\HdiSamples\SensorSampleData\hvac** kopiert.

    Fügen Sie den folgenden Codeausschnitt in eine leere Zelle ein, und drücken Sie UMSCHALT+EINGABE****. Mit diesem Codeausschnitt werden die Daten in einer Tabelle mit dem Namen **hvac** registriert.

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

7. Vergewissern Sie sich, dass die Tabelle erstellt wurde. Sie können die `%%sql` -Magic verwenden, um Hive-Abfragen direkt auszuführen. Weitere Informationen zur `%%sql`-Magic sowie anderen für den PySpark-Kernel verfügbaren Magics finden Sie unter [Verfügbare Kernels für Jupyter Notebooks mit Spark-Clustern unter HDInsight](hdinsight-apache-spark-jupyter-notebook-kernels.md#choose-between-the-kernels).

        %%sql
        SHOW TABLES

    Die Ausgabe sieht etwa wie folgt aus:

        +---------------+-------------+
        |tableName      |isTemporary  |
        +---------------+-------------+
        |hvactemptable  |true         |
        |hivesampletable|false        |
        |hvac           |false        |
        +---------------+-------------+

    Nur die Tabellen, für die in der Spalte **isTemporary** die Option „false“ angegeben ist, sind Hive-Tabellen, die im Metastore gespeichert werden und auf die mit den BI-Tools zugegriffen werden kann. In diesem Tutorial stellen wir eine Verbindung mit der erstellten **hvac**-Tabelle her.

8. Stellen Sie sicher, dass die Tabelle die gewünschten Daten enthält. Kopieren Sie den folgenden Codeausschnitt in eine leere Zelle im Notebook, und drücken Sie UMSCHALT+EINGABETASTE ****.

        %%sql
        SELECT * FROM hvac LIMIT 10

9. Fahren Sie das Notebook herunter, um die Ressourcen freizugeben. Klicken Sie hierzu im Menü **Datei** des Notebooks auf die Option zum **Schließen und Anhalten**.

## <a name="powerbi"></a>Verwenden von Power BI

Nachdem Sie die Daten als Tabelle gespeichert haben, können Sie Power BI verwenden, um eine Verbindung mit den Daten herzustellen und die Daten zu visualisieren. So können Sie Berichte, Dashboards usw. erstellen.

1. Stellen Sie sicher, dass Sie über Zugriff auf Power BI verfügen. Eine kostenlose Preview-Version von Power BI können Sie unter [http://www.powerbi.com/](http://www.powerbi.com/)abonnieren.

2. Melden Sie sich bei [Power BI](http://www.powerbi.com/) an.

3. Klicken Sie unten links im Bereich auf **Daten abrufen**.

4. Klicken Sie auf der Seite **Daten abrufen** unter **Daten importieren oder mit Daten verbinden** für **Datenbanken** auf **Abrufen**.

    ![Abrufen von Daten in Power BI](./media/hdinsight-apache-spark-use-bi-tools/hdispark.powerbi.get.data.png "Abrufen von Daten in Power BI")

5. Klicken Sie auf dem nächsten Bildschirm auf **Spark on Azure HDInsight** und dann auf **Verbinden**. Geben Sie nach Aufforderung die Cluster-URL (`mysparkcluster.azurehdinsight.net`) und die Anmeldeinformationen für die Verbindung mit dem Cluster ein.

    ![Herstellen einer Verbindung mit Spark](./media/hdinsight-apache-spark-use-bi-tools/power-bi-connect-to-spark.png "Abrufen von Daten in Power BI")

    Nachdem die Verbindung hergestellt wurde, beginnt Power BI mit dem Importieren der Daten aus dem Spark-Cluster unter HDInsight.

6. Power BI importiert die Daten und fügt ein **Spark**-Dataset unter der Überschrift **Datasets** hinzu. Klicken Sie auf das Dataset, um ein neues Arbeitsblatt zum Visualisieren der Daten zu öffnen. Sie können das Arbeitsblatt auch als Bericht speichern. Klicken Sie im Menü **Datei** auf **Speichern**, um ein Arbeitsblatt zu speichern.

    ![Spark-Kachel im Power BI-Dashboard](./media/hdinsight-apache-spark-use-bi-tools/hdispark.powerbi.tile.png "Spark-Kachel im Power BI-Dashboard")
7. Beachten Sie, dass in der Liste **Felder** auf der rechten Seite die **hvac**-Tabelle aufgeführt ist, die Sie vorhin erstellt haben. Erweitern Sie die Tabelle, um die Felder in der Tabelle anzuzeigen, die Sie im Notebook definiert haben.

      ![Auflisten der Hive-Tabellen](./media/hdinsight-apache-spark-use-bi-tools/hdispark.powerbi.display.tables.png "Auflisten der Hive-Tabellen")

8. Erstellen Sie eine Visualisierung, um die Abweichung zwischen Zieltemperatur und Ist-Temperatur für jedes Gebäude anzuzeigen. Wählen Sie die Option **Flächendiagramm** (rot umrandet dargestellt), um Ihre Daten zu visualisieren. Zum Definieren der Achse verschieben Sie das Feld **BuildingID** per Drag & Drop unter **Achse** und die Felder **ActualTemp**/**TargetTemp** unter **Wert**.

    ![Erstellen von Visualisierungen](./media/hdinsight-apache-spark-use-bi-tools/hdispark.powerbi.visual1.png "Erstellen von Visualisierungen")

9. Standardmäßig werden in der Visualisierung die Summen für **ActualTemp** und **TargetTemp** angezeigt. Wählen Sie für beide Felder in der Dropdownliste die Option **Mittelwert** aus, um für beide Gebäude den Mittelwert der Ist- und Zieltemperaturen zu erhalten.

    ![Erstellen von Visualisierungen](./media/hdinsight-apache-spark-use-bi-tools/hdispark.powerbi.visual2.png)

10. Ihre Datenvisualisierung sollte in etwa wie im Screenshot aussehen. Bewegen Sie den Cursor über die Visualisierung, um QuickInfos mit relevanten Daten abzurufen.

    ![Erstellen von Visualisierungen](./media/hdinsight-apache-spark-use-bi-tools/hdispark.powerbi.visual3.png)

11. Klicken Sie im oberen Menü auf **Speichern** , und geben Sie einen Berichtsnamen an. Sie können die Visualisierung auch anheften. Wenn Sie eine Visualisierung anheften, wird sie in Ihrem Dashboard gespeichert, damit Sie jeweils den aktuellen Wert im Blick haben.

   Sie können für ein Dataset beliebig viele Visualisierungen hinzufügen und im Dashboard anheften, um eine Momentaufnahme Ihrer Daten zu erhalten. Außerdem sind Spark-Cluster unter HDInsight per Direktverbindung mit Power BI verbunden. So wird sichergestellt, dass Power BI immer über die aktuellsten Daten aus Ihrem Cluster verfügt, damit Sie keine Aktualisierungen für das Dataset einplanen müssen.

## <a name="tableau"></a>Verwenden von Tableau Desktop zum Analysieren von Daten in der Tabelle

> [!NOTE]
> Dieser Abschnitt gilt nur für Spark 1.5.2-Cluster, die in Azure HDInsight erstellt wurden.
>
>

1. Installieren Sie [Tableau Desktop](http://www.tableau.com/products/desktop) auf dem Computer, auf dem Sie dieses Tutorial durchführen. 

2. Stellen Sie sicher, dass auf dem Computer auch der Microsoft Spark ODBC-Treiber installiert ist. Sie können den Treiber [hier](http://go.microsoft.com/fwlink/?LinkId=616229)installieren.

1. Starten Sie Tableau Desktop. Klicken Sie im linken Bereich in der Liste mit den verfügbaren Servern auf **Spark SQL**. Wenn Spark-SQL nicht standardmäßig im linken Bereich aufgeführt ist, finden Sie es durch Klicken auf **Mehr Server**.
2. Geben Sie im Dialogfeld für die Spark-SQL-Verbindung die Werte wie im Screenshot dargestellt ein, und klicken Sie dann auf **OK**.

    ![Herstellen einer Verbindung mit einem Spark-Cluster](./media/hdinsight-apache-spark-use-bi-tools/hdispark.tableau.connect.png "Herstellen einer Verbindung mit einem Spark-Cluster")

    In der Dropdownliste für die Authentifizierung wird der **Microsoft Azure HDInsight-Dienst** nur dann als Option aufgeführt, wenn Sie auf dem Computer den [Microsoft Spark-ODBC-Treiber](http://go.microsoft.com/fwlink/?LinkId=616229) installiert haben.
3. Klicken Sie auf dem nächsten Bildschirm in der Dropdownliste **Schema** auf das Symbol **Suchen** und dann auf **Standard**.

    ![Suchen nach Schemas](./media/hdinsight-apache-spark-use-bi-tools/hdispark.tableau.find.schema.png "Suchen nach Schemas")
4. Klicken Sie für das Feld **Tabelle** erneut auf das Symbol **Suchen**, um alle Hive-Tabellen aufzulisten, die im Cluster verfügbar sind. Daraufhin sollte die **hvac** -Tabelle angezeigt werden, die Sie zuvor mit dem Notebook erstellt haben.

    ![Suchen nach Tabellen](./media/hdinsight-apache-spark-use-bi-tools/hdispark.tableau.find.table.png "Suchen nach Tabellen")
5. Legen Sie die Tabelle im oberen Feld auf der rechten Seite ab. Tableau importiert die Daten und zeigt das Schema wie in der Hervorhebung mit dem roten Kasten an.

    ![Hinzufügen von Tabellen zu Tableau](./media/hdinsight-apache-spark-use-bi-tools/hdispark.tableau.drag.table.png "Hinzufügen von Tabellen zu Tableau")
6. Klicken Sie unten links auf die Registerkarte **Blatt1** . Erstellen Sie eine Visualisierung, in der die durchschnittlichen Ziel- und Ist-Temperaturen für alle Gebäude und jedes Datum angezeigt werden. Ziehen Sie **Datum** und **Building ID** (Gebäude-ID) auf **Spalten** und **Actual Temp**/**Target Temp** (Tatsächliche Temperatur/Zieltemperatur) auf **Zeilen**. Wählen Sie unter **Markierungen** die Option **Bereich**, um eine Bereichskartenvisualisierung zu verwenden.

     ![Hinzufügen von Feldern für die Visualisierung](./media/hdinsight-apache-spark-use-bi-tools/hdispark.tableau.drag.fields.png "Hinzufügen von Feldern für die Visualisierung")
7. Standardmäßig werden die Temperaturfelder als Aggregatwerte angezeigt. Wenn Sie stattdessen die durchschnittlichen Temperaturen anzeigen möchten, können Sie die Dropdownliste verwenden, wie unten dargestellt.

    ![Bestimmen des Mittelwerts für die Temperatur](./media/hdinsight-apache-spark-use-bi-tools/hdispark.tableau.temp.avg.png "Bestimmen des Mittelwerts für die Temperatur")
8. Sie können auch eine Temperaturkarte auf die andere Temperaturkarte legen, um ein besseres Gefühl für den Unterschied zwischen Ziel- und Ist-Temperaturen zu erhalten. Bewegen Sie den Mauszeiger in die Ecke der unteren Bereichskarte, bis das Griffsymbol mit einem roten Kreis markiert wird. Ziehen Sie die Karte in die andere Karte, die darüber liegt, und lassen Sie die Maustaste los, wenn die Form im roten Rechteck hervorgehoben wird.

    ![Zusammenführen von Karten](./media/hdinsight-apache-spark-use-bi-tools/hdispark.tableau.merge.png "Zusammenführen von Karten")

     Die Visualisierung der Daten sollte sich wie im Screenshot dargestellt ändern:

    ![Visualisierung](./media/hdinsight-apache-spark-use-bi-tools/hdispark.tableau.final.visual.png "Visualisierung")
9. Klicken Sie auf **Speichern** , um das Arbeitsblatt zu speichern. Sie können Dashboards erstellen und ein oder mehrere Blätter hinzufügen.

## <a name="seealso"></a>Weitere Informationen
* [Übersicht: Apache Spark in Azure HDInsight](hdinsight-apache-spark-overview.md)

### <a name="scenarios"></a>Szenarios
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
* [Verwalten von Ressourcen für den Apache Spark-Cluster in Azure HDInsight](hdinsight-apache-spark-resource-manager.md)
* [Track and debug jobs running on an Apache Spark cluster in HDInsight(Nachverfolgen und Debuggen von Aufträgen in einem Apache Spark-Cluster unter HDInsight)](hdinsight-apache-spark-job-debugging.md)

[hdinsight-versions]: hdinsight-component-versioning.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-storage]: hdinsight-hadoop-use-blob-storage.md


[azure-purchase-options]: http://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: http://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[azure-management-portal]: https://manage.windowsazure.com/
[azure-create-storageaccount]: storage-create-storage-account.md

