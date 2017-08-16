---
title: "Analysieren von Application Insight-Protokollen mit Spark – Azure HDInsight | Microsoft-Dokumentation"
description: "Erfahren Sie, wie Sie Application Insights-Protokolle in Blobspeicher exportieren und die Protokolle anschließend mit Spark in HDInsight analysieren."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
ms.assetid: 883beae6-9839-45b5-94f7-7eb0f4534ad5
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 08/15/2017
ms.author: larryfr
ms.translationtype: Human Translation
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.openlocfilehash: 8ccb94ea1b415c93d9dc3faefab3cbf8fb075b89
ms.contentlocale: de-de
ms.lasthandoff: 07/08/2017

---
# <a name="analyze-application-insights-telemetry-logs-with-spark-on-hdinsight"></a>Analysieren von Application Insights-Telemetrieprotokollen mit Spark in HDInsight

Erfahren Sie, wie Sie mit Spark in HDInsight Telemetriedaten von Application Insight analysieren können.

[Visual Studio Application Insights](../application-insights/app-insights-overview.md) ist ein Analysedienst, der Ihre Webanwendungen überwacht. Von Application Insights generierte Telemetriedaten können nach Azure Storage exportiert werden. Sobald sich die Daten in Azure Storage befinden, können sie mit HDInsight analysiert werden.

## <a name="prerequisites"></a>Voraussetzungen

* Eine Anwendung, die zur Verwendung von Application Insights konfiguriert ist.

* Sie müssen mit der Erstellung eines Linux-basierten HDInsight-Clusters vertraut sein. Weitere Informationen finden Sie unter [Erste Schritte: Erstellen eines Apache Spark-Clusters für Azure HDInsight und Ausführen von interaktiven Abfragen per Spark-SQL](hdinsight-apache-spark-jupyter-spark-sql.md).

  > [!IMPORTANT]
  > Die Schritte in diesem Dokument erfordern einen HDInsight-Cluster mit Linux. Linux ist das einzige Betriebssystem, das unter HDInsight Version 3.4 oder höher verwendet wird. Weitere Informationen finden Sie unter [Welche Hadoop-Komponenten und -Versionen sind in HDInsight verfügbar?](hdinsight-component-versioning.md#hdinsight-windows-retirement).

* Ein Webbrowser.

Für Entwicklung und Test dieses Dokuments wurden die folgenden Ressourcen eingesetzt:

* Die Application Insights-Telemetriedaten wurden mit einer [Node.js-Web-App generiert, die zur Verwendung von Application Insights konfiguriert wurde](../application-insights/app-insights-nodejs.md).

* Zur Analyse der Daten wurde ein Linux-basierter Spark in HDInsight-Cluster der Version 3.5 verwendet.

## <a name="architecture-and-planning"></a>Architektur und Planung

Das folgende Diagramm zeigt die grundlegende Dienstarchitektur für dieses Beispiel:

![Diagramm zum Datenfluss von Application Insights zum Blobspeicher und anschließende Verarbeitung durch Spark in HDInsight](./media/hdinsight-spark-analyze-application-insight-logs/appinsightshdinsight.png)

### <a name="azure-storage"></a>Azure-Speicher

Application Insights kann für den fortlaufenden Export von Telemetriedaten in Blobs konfiguriert werden. HDInsight kann dann in den Blobs gespeicherte Daten lesen. Es gelten jedoch einige Anforderungen, die erfüllt werden müssen:

* **Speicherort**: Wenn das Speicherkonto und HDInsight sich an verschiedenen Standorten befinden, kann dies die Latenz erhöhen. Gleichzeitig steigen die Kosten, da beim Verschieben von Daten zwischen Regionen Ausgangsgebühren anfallen.

    > [!WARNING]
    > Die Verwendung eines Speicherkontos an einem anderen Speicherort als HDInsight wird nicht unterstützt.

* **Blobtyp**: HDInsight unterstützt nur Blockblobs. Application Insights verwendet standardmäßig Blockblobs, deshalb sollten Sie standardmäßig mit HDInsight arbeiten.

Informationen zum Hinzufügen von zusätzlichem Speicher zu einem vorhandenen HDInsight-Cluster finden Sie im Dokument [Hinzufügen zusätzlicher Speicherkonten](hdinsight-hadoop-add-storage.md).

### <a name="data-schema"></a>Datenschema

Application Insights bietet Informationen zum [Exportdatenmodell](../application-insights/app-insights-export-data-model.md) für das Telemetriedatenformat, das in Blobs exportiert wird. In diesem Dokument wird Spark SQL für die Arbeit mit den Daten verwendet. Spark SQL kann automatisch ein Schema für die von Application Insights protokollierte JSON-Datenstruktur generieren.

## <a name="export-telemetry-data"></a>Exportieren von Telemetriedaten

Führen Sie die Schritte unter [Einrichten des fortlaufenden Exports](../application-insights/app-insights-export-telemetry.md) aus, um Application Insights für den Export von Telemetriedaten in einen Azure-Speicherblob zu konfigurieren.

## <a name="configure-hdinsight-to-access-the-data"></a>Konfigurieren von HDInsight für den Datenzugriff

Wenn Sie einen HDInsight-Cluster erstellen, fügen Sie das Speicherkonto während der Clustererstellung hinzu.

Verwenden Sie zum Hinzufügen des Azure-Speicherkontos zu einem vorhandenen Cluster die Informationen im Dokument [Hinzufügen zusätzlicher Speicherkonten](hdinsight-hadoop-add-storage.md).

## <a name="analyze-the-data-pyspark"></a>Analysieren der Daten: PySpark

1. Wählen Sie im [Azure-Portal](https://portal.azure.com)Ihren Spark in HDInsight-Cluster aus. Wählen Sie im Abschnitt zunächst **Quick Links** und dann **Cluster-Dashboards** aus,und wählen Sie dann auf dem Blatt für Cluster-Dashboards **Jupyter Notebook** aus.

    ![Clusterdashboards](./media/hdinsight-spark-analyze-application-insight-logs/clusterdashboards.png)

2. Wählen Sie oben rechts auf der Jupyter-Seite **Neu** und anschließend **PySpark** aus. Eine neue Browserregisterkarte wird geöffnet, die ein Python-basiertes Jupyter Notebook enthält.

3. Geben Sie in das erste (als **Zelle** bezeichnete) Feld auf der Seite folgenden Text ein:

   ```python
   sc._jsc.hadoopConfiguration().set('mapreduce.input.fileinputformat.input.dir.recursive', 'true')
   ```

    Dieser Code konfiguriert Spark für den rekursiven Zugriff auf die Verzeichnisstruktur für die Eingabedaten. Die Application Insights-Telemetriedaten werden in einer Verzeichnisstruktur protokolliert, die `/{telemetry type}/YYYY-MM-DD/{##}/` ähnelt.

4. Verwenden Sie **UMSCHALT+EINGABE**, um den Code auszuführen. Auf der linken Seite der Zelle wird ein „\*“ in Klammern angezeigt. Dies weist darauf hin, dass der Code in dieser Zelle ausgeführt wird. Nach Abschluss der Ausführung wird „\*“ in eine Zahl geändert, und eine Ausgabe, die dem folgenden Text ähnelt, wird unterhalb der Zelle angezeigt:

        Creating SparkContext as 'sc'

        ID    YARN Application ID    Kind    State    Spark UI    Driver log    Current session?
        3    application_1468969497124_0001    pyspark    idle    Link    Link    ✔

        Creating HiveContext as 'sqlContext'
        SparkContext and HiveContext created. Executing user code ...
5. Unterhalb der ersten Zelle wird eine neu erstellte Zelle angezeigt. Geben Sie den folgenden Text in die neue Zelle ein. Ersetzen Sie `CONTAINER` und `STORAGEACCOUNT` durch den Namen des Azure-Speicherkontos und des BLOB-Containers, der Application Insights-Daten enthält.

   ```python
   %%bash
   hdfs dfs -ls wasb://CONTAINER@STORAGEACCOUNT.blob.core.windows.net/
   ```

    Verwenden Sie **UMSCHALT+EINGABETASTE**, um diese Zelle auszuführen. Ein Ergebnis ähnlich dem folgenden Text wird angezeigt:

        Found 1 items
        drwxrwxrwx   -          0 1970-01-01 00:00 wasb://appinsights@contosostore.blob.core.windows.net/contosoappinsights_2bededa61bc741fbdee6b556571a4831

    Der zurückgegebene WASB-Pfad ist der Speicherort der Application Insights-Telemetriedaten. Ändern Sie die Zeile `hdfs dfs -ls` in der Zelle so ab, dass der zurückgegebene WASB-Pfad verwendet wird, und führen Sie dann über **UMSCHALT+EINGABE** die Zelle erneut aus. Jetzt sollten die Ergebnisse die Verzeichnisse anzeigen, die Telemetriedaten enthalten.

   > [!NOTE]
   > Für die verbleibenden Schritte in diesem Abschnitt wurde das Verzeichnis `wasb://appinsights@contosostore.blob.core.windows.net/contosoappinsights_{ID}/Requests` verwendet. Ihre Verzeichnisstruktur kann anders sein.

6. Geben Sie in der nächsten Zelle den folgenden Code ein: Ersetzen Sie `WASB_PATH` durch den Pfad aus dem vorherigen Schritt.

   ```python
   jsonFiles = sc.textFile('WASB_PATH')
   jsonData = sqlContext.read.json(jsonFiles)
   ```

    Dieser Code erstellt einen Datenrahmen aus den JSON-Dateien, die über den fortlaufenden Exportprozess exportiert wurden. Verwenden Sie **UMSCHALT+EINGABETASTE**, um diese Zelle auszuführen.
7. Geben Sie in der nächsten Zelle Folgendes ein, und führen Sie die Zelle aus, um das Schema anzuzeigen, das Spark für die JSON-Dateien erstellt hat:

   ```python
   jsonData.printSchema()
   ```

    Das Schema ist für die verschiedenen Telemetriedatentypen unterschiedlich. Das nachfolgende Beispiel ist das Schema, das für Webanforderungen generiert wird (Daten sind im Unterverzeichnis `Requests` gespeichert):

        root
        |-- context: struct (nullable = true)
        |    |-- application: struct (nullable = true)
        |    |    |-- version: string (nullable = true)
        |    |-- custom: struct (nullable = true)
        |    |    |-- dimensions: array (nullable = true)
        |    |    |    |-- element: string (containsNull = true)
        |    |    |-- metrics: array (nullable = true)
        |    |    |    |-- element: string (containsNull = true)
        |    |-- data: struct (nullable = true)
        |    |    |-- eventTime: string (nullable = true)
        |    |    |-- isSynthetic: boolean (nullable = true)
        |    |    |-- samplingRate: double (nullable = true)
        |    |    |-- syntheticSource: string (nullable = true)
        |    |-- device: struct (nullable = true)
        |    |    |-- browser: string (nullable = true)
        |    |    |-- browserVersion: string (nullable = true)
        |    |    |-- deviceModel: string (nullable = true)
        |    |    |-- deviceName: string (nullable = true)
        |    |    |-- id: string (nullable = true)
        |    |    |-- osVersion: string (nullable = true)
        |    |    |-- type: string (nullable = true)
        |    |-- location: struct (nullable = true)
        |    |    |-- city: string (nullable = true)
        |    |    |-- clientip: string (nullable = true)
        |    |    |-- continent: string (nullable = true)
        |    |    |-- country: string (nullable = true)
        |    |    |-- province: string (nullable = true)
        |    |-- operation: struct (nullable = true)
        |    |    |-- name: string (nullable = true)
        |    |-- session: struct (nullable = true)
        |    |    |-- id: string (nullable = true)
        |    |    |-- isFirst: boolean (nullable = true)
        |    |-- user: struct (nullable = true)
        |    |    |-- anonId: string (nullable = true)
        |    |    |-- isAuthenticated: boolean (nullable = true)
        |-- internal: struct (nullable = true)
        |    |-- data: struct (nullable = true)
        |    |    |-- documentVersion: string (nullable = true)
        |    |    |-- id: string (nullable = true)
        |-- request: array (nullable = true)
        |    |-- element: struct (containsNull = true)
        |    |    |-- count: long (nullable = true)
        |    |    |-- durationMetric: struct (nullable = true)
        |    |    |    |-- count: double (nullable = true)
        |    |    |    |-- max: double (nullable = true)
        |    |    |    |-- min: double (nullable = true)
        |    |    |    |-- sampledValue: double (nullable = true)
        |    |    |    |-- stdDev: double (nullable = true)
        |    |    |    |-- value: double (nullable = true)
        |    |    |-- id: string (nullable = true)
        |    |    |-- name: string (nullable = true)
        |    |    |-- responseCode: long (nullable = true)
        |    |    |-- success: boolean (nullable = true)
        |    |    |-- url: string (nullable = true)
        |    |    |-- urlData: struct (nullable = true)
        |    |    |    |-- base: string (nullable = true)
        |    |    |    |-- hashTag: string (nullable = true)
        |    |    |    |-- host: string (nullable = true)
        |    |    |    |-- protocol: string (nullable = true)
8. Verwenden Sie den folgenden Code, um den Datenrahmen als temporäre Tabelle zu registrieren und eine Abfrage für die Daten auszuführen:

   ```python
   jsonData.registerTempTable("requests")
   df = sqlContext.sql("select context.location.city from requests where context.location.city is not null")
   df.show()
   ```

    Über diese Abfrage werden Informationen zur Stadt für die ersten 20 Datensätze zurückgegeben, bei denen „context.location.city“ nicht NULL ist.

   > [!NOTE]
   > Die context-Struktur ist in allen von Application Insights generierten Telemetriedaten vorhanden. Das city-Element wurde in Ihren Protokolle möglicherweise nicht gefüllt. Verwenden Sie das Schema, um weitere abfragbare Elemente zu identifizieren, die für Ihre Protokolle möglicherweise Daten enthalten.

    Die Abfrage gibt Informationen zurück, die folgendem Text ähneln:

        +---------+
        |     city|
        +---------+
        | Bellevue|
        |  Redmond|
        |  Seattle|
        |Charlotte|
        ...
        +---------+

## <a name="analyze-the-data-scala"></a>Analysieren der Daten: Scala

1. Wählen Sie im [Azure-Portal](https://portal.azure.com)Ihren Spark in HDInsight-Cluster aus. Wählen Sie im Abschnitt zunächst **Quick Links** und dann **Cluster-Dashboards** aus,und wählen Sie dann auf dem Blatt für Cluster-Dashboards **Jupyter Notebook** aus.

    ![Clusterdashboards](./media/hdinsight-spark-analyze-application-insight-logs/clusterdashboards.png)
2. Wählen Sie oben rechts auf der Jupyter-Seite **Neu** und anschließend **Scala** aus. Eine neue Browserregisterkarte, die ein Scala-basiertes Jupyter-Notebook enthält, wird angezeigt.
3. Geben Sie in das erste (als **Zelle** bezeichnete) Feld auf der Seite folgenden Text ein:

   ```scala
   sc.hadoopConfiguration.set("mapreduce.input.fileinputformat.input.dir.recursive", "true")
   ```

    Dieser Code konfiguriert Spark für den rekursiven Zugriff auf die Verzeichnisstruktur für die Eingabedaten. Die Application Insights-Telemetriedaten werden in einer Verzeichnisstruktur protokolliert, die `/{telemetry type}/YYYY-MM-DD/{##}/` ähnelt.

4. Verwenden Sie **UMSCHALT+EINGABE**, um den Code auszuführen. Auf der linken Seite der Zelle wird ein „\*“ in Klammern angezeigt. Dies weist darauf hin, dass der Code in dieser Zelle ausgeführt wird. Nach Abschluss der Ausführung wird „\*“ in eine Zahl geändert, und eine Ausgabe, die dem folgenden Text ähnelt, wird unterhalb der Zelle angezeigt:

        Creating SparkContext as 'sc'

        ID    YARN Application ID    Kind    State    Spark UI    Driver log    Current session?
        3    application_1468969497124_0001    spark    idle    Link    Link    ✔

        Creating HiveContext as 'sqlContext'
        SparkContext and HiveContext created. Executing user code ...
5. Unterhalb der ersten Zelle wird eine neu erstellte Zelle angezeigt. Geben Sie den folgenden Text in die neue Zelle ein. Ersetzen Sie `CONTAINER` und `STORAGEACCOUNT` durch den Namen des Azure-Speicherkontos und des BLOB-Containers, der Application Insights-Protokolle enthält.

   ```scala
   %%bash
   hdfs dfs -ls wasb://CONTAINER@STORAGEACCOUNT.blob.core.windows.net/
   ```

    Verwenden Sie **UMSCHALT+EINGABETASTE**, um diese Zelle auszuführen. Ein Ergebnis ähnlich dem folgenden Text wird angezeigt:

        Found 1 items
        drwxrwxrwx   -          0 1970-01-01 00:00 wasb://appinsights@contosostore.blob.core.windows.net/contosoappinsights_2bededa61bc741fbdee6b556571a4831

    Der zurückgegebene WASB-Pfad ist der Speicherort der Application Insights-Telemetriedaten. Ändern Sie die Zeile `hdfs dfs -ls` in der Zelle so ab, dass der zurückgegebene WASB-Pfad verwendet wird, und führen Sie dann über **UMSCHALT+EINGABE** die Zelle erneut aus. Jetzt sollten die Ergebnisse die Verzeichnisse anzeigen, die Telemetriedaten enthalten.

   > [!NOTE]
   > Für die verbleibenden Schritte in diesem Abschnitt wurde das Verzeichnis `wasb://appinsights@contosostore.blob.core.windows.net/contosoappinsights_{ID}/Requests` verwendet. Dieses Verzeichnis ist nur vorhanden, wenn Ihre Telemetriedaten aus einer Web-App stammen.

6. Geben Sie in der nächsten Zelle den folgenden Code ein: Ersetzen Sie `WASB\_PATH` durch den Pfad aus dem vorherigen Schritt.

   ```scala
   var jsonFiles = sc.textFile('WASB_PATH')
   val sqlContext = new org.apache.spark.sql.SQLContext(sc)
   var jsonData = sqlContext.read.json(jsonFiles)
   ```

    Dieser Code erstellt einen Datenrahmen aus den JSON-Dateien, die über den fortlaufenden Exportprozess exportiert wurden. Verwenden Sie **UMSCHALT+EINGABETASTE**, um diese Zelle auszuführen.

7. Geben Sie in der nächsten Zelle Folgendes ein, und führen Sie die Zelle aus, um das Schema anzuzeigen, das Spark für die JSON-Dateien erstellt hat:

   ```scala
   jsonData.printSchema
   ```

    Das Schema ist für die verschiedenen Telemetriedatentypen unterschiedlich. Das nachfolgende Beispiel ist das Schema, das für Webanforderungen generiert wird (Daten sind im Unterverzeichnis `Requests` gespeichert):

        root
        |-- context: struct (nullable = true)
        |    |-- application: struct (nullable = true)
        |    |    |-- version: string (nullable = true)
        |    |-- custom: struct (nullable = true)
        |    |    |-- dimensions: array (nullable = true)
        |    |    |    |-- element: string (containsNull = true)
        |    |    |-- metrics: array (nullable = true)
        |    |    |    |-- element: string (containsNull = true)
        |    |-- data: struct (nullable = true)
        |    |    |-- eventTime: string (nullable = true)
        |    |    |-- isSynthetic: boolean (nullable = true)
        |    |    |-- samplingRate: double (nullable = true)
        |    |    |-- syntheticSource: string (nullable = true)
        |    |-- device: struct (nullable = true)
        |    |    |-- browser: string (nullable = true)
        |    |    |-- browserVersion: string (nullable = true)
        |    |    |-- deviceModel: string (nullable = true)
        |    |    |-- deviceName: string (nullable = true)
        |    |    |-- id: string (nullable = true)
        |    |    |-- osVersion: string (nullable = true)
        |    |    |-- type: string (nullable = true)
        |    |-- location: struct (nullable = true)
        |    |    |-- city: string (nullable = true)
        |    |    |-- clientip: string (nullable = true)
        |    |    |-- continent: string (nullable = true)
        |    |    |-- country: string (nullable = true)
        |    |    |-- province: string (nullable = true)
        |    |-- operation: struct (nullable = true)
        |    |    |-- name: string (nullable = true)
        |    |-- session: struct (nullable = true)
        |    |    |-- id: string (nullable = true)
        |    |    |-- isFirst: boolean (nullable = true)
        |    |-- user: struct (nullable = true)
        |    |    |-- anonId: string (nullable = true)
        |    |    |-- isAuthenticated: boolean (nullable = true)
        |-- internal: struct (nullable = true)
        |    |-- data: struct (nullable = true)
        |    |    |-- documentVersion: string (nullable = true)
        |    |    |-- id: string (nullable = true)
        |-- request: array (nullable = true)
        |    |-- element: struct (containsNull = true)
        |    |    |-- count: long (nullable = true)
        |    |    |-- durationMetric: struct (nullable = true)
        |    |    |    |-- count: double (nullable = true)
        |    |    |    |-- max: double (nullable = true)
        |    |    |    |-- min: double (nullable = true)
        |    |    |    |-- sampledValue: double (nullable = true)
        |    |    |    |-- stdDev: double (nullable = true)
        |    |    |    |-- value: double (nullable = true)
        |    |    |-- id: string (nullable = true)
        |    |    |-- name: string (nullable = true)
        |    |    |-- responseCode: long (nullable = true)
        |    |    |-- success: boolean (nullable = true)
        |    |    |-- url: string (nullable = true)
        |    |    |-- urlData: struct (nullable = true)
        |    |    |    |-- base: string (nullable = true)
        |    |    |    |-- hashTag: string (nullable = true)
        |    |    |    |-- host: string (nullable = true)
        |    |    |    |-- protocol: string (nullable = true)

8. Verwenden Sie den folgenden Code, um den Datenrahmen als temporäre Tabelle zu registrieren und eine Abfrage für die Daten auszuführen:

   ```scala
   jsonData.registerTempTable("requests")
   var city = sqlContext.sql("select context.location.city from requests where context.location.city is not null limit 10").show()
   ```

    Über diese Abfrage werden Informationen zur Stadt für die ersten 20 Datensätze zurückgegeben, bei denen „context.location.city“ nicht NULL ist.

   > [!NOTE]
   > Die context-Struktur ist in allen von Application Insights generierten Telemetriedaten vorhanden. Das city-Element wurde in Ihren Protokolle möglicherweise nicht gefüllt. Verwenden Sie das Schema, um weitere abfragbare Elemente zu identifizieren, die für Ihre Protokolle möglicherweise Daten enthalten.
   >
   >

    Die Abfrage gibt Informationen zurück, die folgendem Text ähneln:

        +---------+
        |     city|
        +---------+
        | Bellevue|
        |  Redmond|
        |  Seattle|
        |Charlotte|
        ...
        +---------+

## <a name="next-steps"></a>Nächste Schritte

Weitere Beispiele zur Verwendung von Spark für die Arbeit mit Daten und Diensten in Azure finden Sie in den folgenden Dokumenten:

* [Spark mit BI: Durchführen interaktiver Datenanalysen mithilfe von Spark in HDInsight mit BI-Tools](hdinsight-apache-spark-use-bi-tools.md)
* [Spark mit Machine Learning: Analysieren von Gebäudetemperaturen mithilfe von Spark in HDInsight und HVAC-Daten](hdinsight-apache-spark-ipython-notebook-machine-learning.md)
* [Spark mit Machine Learning: Vorhersage von Lebensmittelkontrollergebnissen mithilfe von Spark in HDInsight](hdinsight-apache-spark-machine-learning-mllib-ipython.md)
* [Spark-Streaming: Erstellen von Streaminganwendungen mithilfe von Spark in HDInsight](hdinsight-apache-spark-eventhub-streaming.md)
* [Websiteprotokollanalyse mithilfe von Spark in HDInsight](hdinsight-apache-spark-custom-library-website-log-analysis.md)

Informationen zum Erstellen und Ausführen von Spark-Anwendungen finden Sie in den folgenden Dokumenten:

* [Erstellen einer eigenständigen Anwendung mit Scala](hdinsight-apache-spark-create-standalone-application.md)
* [Remoteausführung von Aufträgen in einem Spark-Cluster mithilfe von Livy](hdinsight-apache-spark-livy-rest-interface.md)

