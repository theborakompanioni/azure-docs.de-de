---
title: Analysieren von Application Insight-Protokollen mit Spark in HDInsight | Microsoft-Dokumentation
description: "Erfahren Sie, wie Sie Application Insights-Protokolle in Blobspeicher exportieren und die Protokolle anschließend mit Spark in HDInsight analysieren."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
ms.assetid: 883beae6-9839-45b5-94f7-7eb0f4534ad5
ms.service: hdinsight
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 02/10/2017
ms.author: larryfr
translationtype: Human Translation
ms.sourcegitcommit: 8c07f0da21eab0c90ad9608dfaeb29dd4a01a6b7
ms.openlocfilehash: b3ff0e93ee144e98dec69c0678c3b467db1e0bc0


---
# <a name="analyze-application-insights-telemetry-logs-with-spark-on-hdinsight"></a>Analysieren von Application Insights-Telemetrieprotokollen mit Spark in HDInsight

[Visual Studio Application Insights](../application-insights/app-insights-overview.md) ist ein Analysedienst, der Ihre Webanwendungen überwacht. Von Application Insights generierte Telemetriedaten können nach Azure Storage exportiert und von dort aus über HDInsight analysiert werden.

In diesem Dokument erfahren Sie, wie Sie HDInsight zum Analysieren von Application Insights-Telemetriedaten unter Verwendung von Apache Spark nutzen.

## <a name="prerequisites"></a>Voraussetzungen

* Ein Azure-Abonnement.

* Eine Anwendung, die zur Verwendung von Application Insights konfiguriert ist. 

* Sie müssen mit der Erstellung eines Linux-basierten HDInsight-Clusters vertraut sein. Wenn Sie nicht mit dem Vorgang zum Erstellen eines Clusters vertraut sind, finden Sie unter [Erstellen von Spark in HDInsight-Clustern](hdinsight-apache-spark-jupyter-spark-sql.md) weitere Informationen.
  
  > [!IMPORTANT]
  > Die Schritte in diesem Dokument erfordern einen HDInsight-Cluster mit Linux. Linux ist das einzige Betriebssystem, das unter HDInsight Version 3.4 oder höher verwendet wird. Weitere Informationen finden Sie unter [Ende des Lebenszyklus von HDInsight unter Windows](hdinsight-component-versioning.md#hdi-version-32-and-33-nearing-deprecation-date).

* Ein Webbrowser. Dieser wird verwendet, um eine interaktive Analyse über ein Jupyter Notebook auszuführen.

Für Entwicklung und Test dieses Dokuments wurden die folgenden Komponenten eingesetzt:

* Die Application Insights-Telemetriedaten wurden mit einer [Node.js-Web-App generiert, die zur Verwendung von Application Insights konfiguriert wurde](../application-insights/app-insights-nodejs.md).

* Zur Analyse der Daten wurde ein Linux-basierter Spark in HDInsight-Cluster der Version 3.4 verwendet.

## <a name="architecture-and-planning"></a>Architektur und Planung
Das folgende Diagramm zeigt die grundlegende Dienstarchitektur für dieses Beispiel:

![Diagramm zum Datenfluss von Application Insights zum Blobspeicher und anschließende Verarbeitung durch Spark in HDInsight](./media/hdinsight-spark-analyze-application-insight-logs/appinsightshdinsight.png)

### <a name="azure-storage"></a>Azure-Speicher

Ein HDInsight-Cluster kann direkt auf Blockblobs eines Azure Storage-Kontos zugreifen, und Application Insights kann zum fortlaufenden Export von Telemetriedaten in Blobs in Azure Storage konfiguriert werden. Es gelten jedoch einige Anforderungen, die erfüllt werden müssen:

* **Standort**: Das Speicherkonto muss sich in derselben Region wie HDInsight befinden. Dies verringert die Latenz beim Zugriff auf die Daten und verhindert Kosten für ausgehende Datenübertragungen, die beim Verschieben von Daten zwischen Regionen anfallen.
* **Blobtyp**: HDInsight unterstützt nur Blockblobs. Application Insights verwendet standardmäßig Blockblobs, deshalb sollten Sie standardmäßig mit HDInsight arbeiten.
* **Zugriffsberechtigungen**: Wenn Sie dasselbe Speicherkonto für den fortlaufenden Application Insights-Export und den HDInsight-Standardspeicher verwenden, verfügt HDInsight über Vollzugriff auf die Application Insights-Telemetriedaten. Dies bedeutet, dass die Telemetriedaten aus dem HDInsight-Cluster gelöscht werden können.
  
    Stattdessen wird empfohlen, separate Speicherkonten für HDInsight und Application Insights-Telemetriedaten zu verwenden und [Shared Access Signatures (SAS) zu verwenden, um den Datenzugriff aus HDInsight zu beschränken](hdinsight-storage-sharedaccesssignature-permissions.md). Mithilfe einer SAS können Sie HDInsight Lesezugriff auf die Telemetriedaten gewähren.

### <a name="data-schema"></a>Datenschema

Application Insights bietet Informationen zum [Exportdatenmodell](../application-insights/app-insights-export-data-model.md) für das Telemetriedatenformat, das in Blobs exportiert wird. In diesem Dokument wird Spark SQL für die Arbeit mit den Daten verwendet. Spark SQL kann automatisch ein Schema für die JSON-Datenstruktur generieren, das von Application Insights protokolliert wird, deshalb müssen Sie das Schema beim Ausführen einer Analyse nicht manuell definieren.

## <a name="export-telemetry-data"></a>Exportieren von Telemetriedaten
Führen Sie die Schritte unter [Einrichten des fortlaufenden Exports](../application-insights/app-insights-export-telemetry.md) aus, um Application Insights für den Export von Telemetriedaten in einen Azure-Speicherblob zu konfigurieren.

## <a name="configure-hdinsight-to-access-the-data"></a>Konfigurieren von HDInsight für den Datenzugriff
Verwenden Sie die Informationen unter [Verwenden von Azure Storage Shared Access Signatures zum Einschränken des Zugriffs auf Daten mit HDInsight](hdinsight-storage-sharedaccesssignature-permissions.md) , um eine SAS für den Blobcontainer zu erstellen, der die exportierten Telemetriedaten enthält. Die SAS sollte Lesezugriff auf die Daten gewähren.

Das SAS-Dokument bietet Informationen dazu, wie Sie den SAS-Speicher zu einem vorhandenen Linux-basierten HDInsight-Cluster hinzufügen. Darüber hinaus werden Informationen dazu bereitgestellt, wie Sie den SAS-Speicher beim Erstellen eines neuen HDInsight-Clusters hinzufügen.

## <a name="analyze-the-data-using-python-pyspark"></a>Analysieren der Daten mithilfe von Python (PySpark)
1. Wählen Sie im [Azure-Portal](https://portal.azure.com)Ihren Spark in HDInsight-Cluster aus. Wählen Sie im Abschnitt zunächst **Quick Links** und dann **Cluster-Dashboards** aus,und wählen Sie dann auf dem Blatt für Cluster-Dashboards **Jupyter Notebook** aus.
   
    ![Clusterdashboards](./media/hdinsight-spark-analyze-application-insight-logs/clusterdashboards.png)
2. Wählen Sie oben rechts auf der Jupyter-Seite **Neu** und anschließend **PySpark** aus. Daraufhin wird eine neue Browserregisterkarte geöffnet, die ein Python-basiertes Jupyter Notebook enthält.
3. Geben Sie in das erste Feld (als **Zelle**bezeichnet) auf der Seite Folgendes ein:
   
        sc._jsc.hadoopConfiguration().set('mapreduce.input.fileinputformat.input.dir.recursive', 'true')
   
    Auf diese Weise kann Spark rekursiv auf die Verzeichnisstruktur für die Eingabedaten zugreifen. Die Application Insights-Telemetriedaten werden in einer Verzeichnisstruktur ähnlich der folgenden protokolliert:
   
        /{telemetry type}/YYYY-MM-DD/{##}/
4. Verwenden Sie **UMSCHALT+EINGABE**, um den Code auszuführen. Auf der linken Seite der Zelle wird ein \* in Klammern angezeigt. Dies weist darauf hin, dass der Code in dieser Zelle ausgeführt wird. Nach Abschluss der Ausführung wird \* in eine Zahl geändert, und eine Ausgabe ähnlich der folgenden wird unterhalb der Zelle angezeigt:
   
        Creating SparkContext as 'sc'
   
        ID    YARN Application ID    Kind    State    Spark UI    Driver log    Current session?
        3    application_1468969497124_0001    pyspark    idle    Link    Link    ✔
   
        Creating HiveContext as 'sqlContext'
        SparkContext and HiveContext created. Executing user code ...
5. Unterhalb der ersten Zelle wird eine neu erstellte Zelle angezeigt. Geben Sie Folgendes in die neue Zelle ein. Ersetzen Sie hierbei **CONTAINER** und **STORAGEACCOUNT** durch den Azure Storage-Kontonamen und den Namen des Blobcontainers, die Sie beim Konfigurieren des fortlaufenden Application Insights-Exports verwendet haben.
   
        %%bash
        hdfs dfs -ls wasb://CONTAINER@STORAGEACCOUNT.blob.core.windows.net/
   
    Verwenden Sie **UMSCHALT+EINGABETASTE**, um diese Zelle auszuführen. Ein Ergebnis ähnlich dem folgenden wird angezeigt:
   
        Found 1 items
        drwxrwxrwx   -          0 1970-01-01 00:00 wasb://appinsights@contosostore.blob.core.windows.net/contosoappinsights_2bededa61bc741fbdee6b556571a4831
   
    Der zurückgegebene WASB-Pfad ist der Speicherort der Application Insights-Telemetriedaten. Ändern Sie die Zeile `hdfs dfs -ls` in der Zelle so ab, dass der zurückgegebene WASB-Pfad verwendet wird, und führen Sie dann über **UMSCHALT+EINGABE** die Zelle erneut aus. Jetzt sollten die Ergebnisse die Verzeichnisse anzeigen, die Telemetriedaten enthalten.
   
   > [!NOTE]
   > Für die verbleibenden Schritte in diesem Abschnitt wurde das Verzeichnis `wasb://appinsights@contosostore.blob.core.windows.net/contosoappinsights_{ID}/Requests` verwendet. Dieses Verzeichnis ist nur vorhanden, wenn Ihre Telemetriedaten aus einer Web-App stammen. Wenn Sie Telemetriedaten verwendet, die kein Anforderungsverzeichnis umfassen, wählen Sie ein anderes Verzeichnis aus, und passen Sie die übrigen Schritte auf die Verwendung dieses Verzeichnisses und des Schemas für die darin gespeicherten Daten an.
   > 
   > 
6. Geben Sie in die nächste Zelle Folgendes ein. Ersetzen Sie **WASB\_PATH** durch den Pfad aus dem vorherigen Schritt.
   
        jsonFiles = sc.textFile('WASB_PATH')
        jsonData = sqlContext.read.json(jsonFiles)
   
    Hierdurch wird ein neuer Datenrahmenaus den JSON-Dateien erstellt, die über den fortlaufenden Exportprozess exportiert wurden. Verwenden Sie **UMSCHALT+EINGABETASTE**, um diese Zelle auszuführen.
7. Geben Sie in der nächsten Zelle Folgendes ein, und führen Sie die Zelle aus, um das Schema anzuzeigen, das Spark für die JSON-Dateien erstellt hat:
   
        jsonData.printSchema()
   
    Das Schema ist für die verschiedenen Telemetriedatentypen unterschiedlich. Das nachfolgende Schema wird für Webanforderungen generiert (Daten sind im Unterverzeichnis `Requests` gespeichert):
   
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
   
        jsonData.registerTempTable("requests")
        sqlContext.sql("select context.location.city from requests where context.location.city is not null")
   
    Über diese Abfrage werden Informationen zur Stadt für die ersten 20 Datensätze zurückgegeben, bei denen „context.location.city“ nicht NULL ist.
   
   > [!NOTE]
   > Die Kontextstruktur ist in allen Telemetriedaten vorhanden, die von Application Insights protokolliert werden. Möglicherweise wird das Element „city“ jedoch in Ihren Protokollen nicht aufgefüllt. Verwenden Sie das Schema, um weitere abfragbare Elemente zu identifizieren, die für Ihre Protokolle möglicherweise Daten enthalten.
   > 
   > 
   
    Diese Abfrage gibt ähnliche Informationen wie diese zurück:
   
        +---------+
        |     city|
        +---------+
        | Bellevue|
        |  Redmond|
        |  Seattle|
        |Charlotte|
        ...
        +---------+

## <a name="analyze-the-data-using-scala"></a>Analysieren der Daten mit Scala
1. Wählen Sie im [Azure-Portal](https://portal.azure.com)Ihren Spark in HDInsight-Cluster aus. Wählen Sie im Abschnitt zunächst **Quick Links** und dann **Cluster-Dashboards** aus,und wählen Sie dann auf dem Blatt für Cluster-Dashboards **Jupyter Notebook** aus.
   
    ![Clusterdashboards](./media/hdinsight-spark-analyze-application-insight-logs/clusterdashboards.png)
2. Wählen Sie oben rechts auf der Jupyter-Seite **Neu** und anschließend **Scala** aus. Daraufhin wird eine neue Browserregisterkarte geöffnet, die ein Scala-basiertes Jupyter Notebook enthält.
3. Geben Sie in das erste Feld (als **Zelle**bezeichnet) auf der Seite Folgendes ein:
   
        sc.hadoopConfiguration.set("mapreduce.input.fileinputformat.input.dir.recursive", "true")
   
    Auf diese Weise kann Spark rekursiv auf die Verzeichnisstruktur für die Eingabedaten zugreifen. Die Application Insights-Telemetriedaten werden in einer Verzeichnisstruktur ähnlich der folgenden protokolliert:
   
        /{telemetry type}/YYYY-MM-DD/{##}/
4. Verwenden Sie **UMSCHALT+EINGABE**, um den Code auszuführen. Auf der linken Seite der Zelle wird ein \* in Klammern angezeigt. Dies weist darauf hin, dass der Code in dieser Zelle ausgeführt wird. Nach Abschluss der Ausführung wird \* in eine Zahl geändert, und eine Ausgabe ähnlich der folgenden wird unterhalb der Zelle angezeigt:
   
        Creating SparkContext as 'sc'
   
        ID    YARN Application ID    Kind    State    Spark UI    Driver log    Current session?
        3    application_1468969497124_0001    spark    idle    Link    Link    ✔
   
        Creating HiveContext as 'sqlContext'
        SparkContext and HiveContext created. Executing user code ...
5. Unterhalb der ersten Zelle wird eine neu erstellte Zelle angezeigt. Geben Sie Folgendes in die neue Zelle ein. Ersetzen Sie hierbei **CONTAINER** und **STORAGEACCOUNT** durch den Azure Storage-Kontonamen und den Namen des Blobcontainers, die Sie beim Konfigurieren des fortlaufenden Application Insights-Exports verwendet haben.
   
        %%bash
        hdfs dfs -ls wasb://CONTAINER@STORAGEACCOUNT.blob.core.windows.net/
   
    Verwenden Sie **UMSCHALT+EINGABETASTE**, um diese Zelle auszuführen. Ein Ergebnis ähnlich dem folgenden wird angezeigt:
   
        Found 1 items
        drwxrwxrwx   -          0 1970-01-01 00:00 wasb://appinsights@contosostore.blob.core.windows.net/contosoappinsights_2bededa61bc741fbdee6b556571a4831
   
    Der zurückgegebene WASB-Pfad ist der Speicherort der Application Insights-Telemetriedaten. Ändern Sie die Zeile `hdfs dfs -ls` in der Zelle so ab, dass der zurückgegebene WASB-Pfad verwendet wird, und führen Sie dann über **UMSCHALT+EINGABE** die Zelle erneut aus. Jetzt sollten die Ergebnisse die Verzeichnisse anzeigen, die Telemetriedaten enthalten.
   
   > [!NOTE]
   > Für die verbleibenden Schritte in diesem Abschnitt wurde das Verzeichnis `wasb://appinsights@contosostore.blob.core.windows.net/contosoappinsights_{ID}/Requests` verwendet. Dieses Verzeichnis ist nur vorhanden, wenn Ihre Telemetriedaten aus einer Web-App stammen. Wenn Sie Telemetriedaten verwendet, die kein Anforderungsverzeichnis umfassen, wählen Sie ein anderes Verzeichnis aus, und passen Sie die übrigen Schritte auf die Verwendung dieses Verzeichnisses und des Schemas für die darin gespeicherten Daten an.
   > 
   > 
6. Geben Sie in die nächste Zelle Folgendes ein. Ersetzen Sie **WASB\_PATH** durch den Pfad aus dem vorherigen Schritt.
   
        jsonFiles = sc.textFile('WASB_PATH')
        jsonData = sqlContext.read.json(jsonFiles)
   
    Hierdurch wird ein neuer Datenrahmenaus den JSON-Dateien erstellt, die über den fortlaufenden Exportprozess exportiert wurden. Verwenden Sie **UMSCHALT+EINGABETASTE**, um diese Zelle auszuführen.
7. Geben Sie in der nächsten Zelle Folgendes ein, und führen Sie die Zelle aus, um das Schema anzuzeigen, das Spark für die JSON-Dateien erstellt hat:
   
        jsonData.printSchema
   
    Das Schema ist für die verschiedenen Telemetriedatentypen unterschiedlich. Das nachfolgende Schema wird für Webanforderungen generiert (Daten sind im Unterverzeichnis `Requests` gespeichert):
   
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
   
        jsonData.registerTempTable("requests")
        var city = sqlContext.sql("select context.location.city from requests where context.location.city is not null limit 10").show()
   
    Über diese Abfrage werden Informationen zur Stadt für die ersten 20 Datensätze zurückgegeben, bei denen „context.location.city“ nicht NULL ist.
   
   > [!NOTE]
   > Die Kontextstruktur ist in allen Telemetriedaten vorhanden, die von Application Insights protokolliert werden. Möglicherweise wird das Element „city“ jedoch in Ihren Protokollen nicht aufgefüllt. Verwenden Sie das Schema, um weitere abfragbare Elemente zu identifizieren, die für Ihre Protokolle möglicherweise Daten enthalten.
   > 
   > 
   
    Diese Abfrage gibt ähnliche Informationen wie diese zurück:
   
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
* [Spark-Streaming: Erstellen von Echtzeitstreaminganwendungen mithilfe von Spark in HDInsight](hdinsight-apache-spark-eventhub-streaming.md)
* [Websiteprotokollanalyse mithilfe von Spark in HDInsight](hdinsight-apache-spark-custom-library-website-log-analysis.md)

Informationen zum Erstellen und Ausführen von Spark-Anwendungen finden Sie in den folgenden Dokumenten:

* [Erstellen einer eigenständigen Anwendung mit Scala](hdinsight-apache-spark-create-standalone-application.md)
* [Remoteausführung von Aufträgen in einem Spark-Cluster mithilfe von Livy](hdinsight-apache-spark-livy-rest-interface.md)




<!--HONumber=Jan17_HO3-->


