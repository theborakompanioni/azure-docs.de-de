---
title: Verwenden von Ambari-Ansichten zum Arbeiten mit Hive in HDInsight (Hadoop) | Microsoft Docs
description: "Erfahren Sie, wie Sie die Hive-Ansicht im Webbrowser zum Übermitteln von Hive-Abfragen verwenden. Die Hive-Ansicht ist Teil der Ambari-Webbenutzeroberfläche, die mit dem Linux-basierten HDInsight-Cluster bereitgestellt wird."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 1abe9104-f4b2-41b9-9161-abbc43de8294
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 02/08/2017
ms.author: larryfr
translationtype: Human Translation
ms.sourcegitcommit: 5ec4b964066687b506686709c3dc5ed5b402fbaf
ms.openlocfilehash: a846d5a70451ed3082b90d87b90bef0eb6da5993
ms.lasthandoff: 02/09/2017


---
# <a name="use-the-hive-view-with-hadoop-in-hdinsight"></a>Verwenden der Hive-Ansicht mit Hadoop in HDInsight

[!INCLUDE [hive-selector](../../includes/hdinsight-selector-use-hive.md)]

Ambari ist ein Verwaltungs- und Überwachungsdienstprogramm, das mit Linux-basierten HDInsight-Clustern bereitgestellt wird. Eines der in Ambari bereitgestellten Features ist eine Webbenutzeroberfläche, die zum Ausführen von Hive-Abfragen verwendet werden kann. Dies ist die **Hive-Ansicht**, die Teil der Ambari-Ansichten des HDInsight-Clusters ist.

> [!NOTE]
> Ambari verfügt über viele Funktionen, die in diesem Dokument nicht beschrieben werden. Weitere Informationen finden Sie unter [Verwalten von HDInsight-Clustern mithilfe der Ambari-Webbenutzeroberfläche](hdinsight-hadoop-manage-ambari.md).

## <a name="prerequisites"></a>Voraussetzungen

* Ein Linux-basierter HDInsight-Cluster. Informationen zum Erstellen eines Clusters finden Sie unter [Erste Schritte mit Linux-basiertem HDInsight](hdinsight-hadoop-linux-tutorial-get-started.md).

> [!IMPORTANT]
> Die Schritte in diesem Dokument erfordern einen HDInsight-Cluster mit Linux. Linux ist das einzige Betriebssystem, das unter HDInsight Version 3.4 oder höher verwendet wird. Weitere Informationen finden Sie unter [Ende des Lebenszyklus von HDInsight unter Windows](hdinsight-component-versioning.md#hdi-version-32-and-33-nearing-deprecation-date).

## <a name="open-the-hive-view"></a>Öffnen der Hive-Ansicht

Ambari-Ansichten sind im Azure-Portal verfügbar. Wählen Sie Ihren HDInsight-Cluster, und wählen Sie dann im Abschnitt **Quicklinks** die Option **Ambari-Ansichten**.

![Quicklinks-Abschnitt](./media/hdinsight-hadoop-use-hive-ambari-view/quicklinks.png)

Sie können auch direkt zu Ambari navigieren, indem Sie in einem Webbrowser „https://CLUSTERNAME.azurehdinsight.net“ aufrufen. Ersetzen Sie **CLUSTERNAME** durch den Namen Ihres HDInsight-Clusters. Klicken Sie im Seitenmenü auf die Gruppe von Quadraten neben dem Link für **Admin**, um verfügbare Ansichten aufzulisten. Wählen Sie die Ansicht **Hive** aus.

![Ambari-Ansichten auswählen](./media/hdinsight-hadoop-use-hive-ambari-view/selecthiveview.png).

> [!NOTE]
> Beim Zugriff auf Ambari werden Sie aufgefordert, sich bei der Website zu authentifizieren. Geben Sie den Administratorkontonamen (Standard-`admin`) und das Kennwort ein, den bzw. das Sie beim Erstellen des Clusters verwendet haben.

Eine Seite ähnlich der folgenden wird angezeigt:

![Bild der Hive-Ansichtenseite, die einen Abfrage-Editor-Abschnitt enthält](./media/hdinsight-hadoop-use-hive-ambari-view/hiveview.png)

## <a name="view-tables"></a>Anzeigen von Tabellen
Wählen Sie im Abschnitt **Datenbank-Explorer** der Seite auf der Registerkarte **Datenbanken** den Eintrag **Standard** aus. Eine Liste mit den Tabellen in der Standarddatenbank wird angezeigt. Für einen neuen HDInsight-Cluster sollte nur eine Tabelle vorhanden sein: **hivesampletable**.

![Datenbank-Explorer mit erweiterter Standarddatenbank](./media/hdinsight-hadoop-use-hive-ambari-view/databaseexplorer.png)

Wenn mit den Schritten in diesem Dokument neue Tabellen hinzugefügt werden, können Sie das Symbol „Aktualisieren“ oben rechts im Datenbank-Explorer verwenden, um die Liste zu aktualisieren.

## <a name="hivequery"></a>Abfrage-Editor

Verwenden Sie die folgenden Schritte aus der Hive-Ansicht, um eine Hive-Abfrage auszuführen.

1. Fügen Sie im Abschnitt **Abfrage-Editor** der Seite die folgenden HiveQL-Anweisungen in das Arbeitsblatt ein:
   
    ```hiveql
    DROP TABLE log4jLogs;
    CREATE EXTERNAL TABLE log4jLogs(t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string)
    ROW FORMAT DELIMITED FIELDS TERMINATED BY ' '
    STORED AS TEXTFILE LOCATION '/example/data/';
    SELECT t4 AS sev, COUNT(*) AS cnt FROM log4jLogs WHERE t4 = '[ERROR]' GROUP BY t4;
    ```
   
    Diese Anweisungen führen die folgenden Aktionen aus:
   
   * **TABELLE LÖSCHEN** : Löscht Tabelle und Datendatei, falls die Tabelle bereits existiert.

   * **CREATE EXTERNAL TABLE**: Erstellt eine neue „externe“ Tabelle in Hive. 
   Externe Tabellen speichern nur die Tabellendefinition in Hive. Die Daten verbleiben an ihrem ursprünglichen Speicherort.

   * **ZEILENFORMAT** : Teilt Hive mit, wie die Daten formatiert werden. In diesem Fall werden die Felder in den einzelnen Protokollen durch Leerzeichen getrennt.

   * **SPEICHERORT DER TEXTDATEI** – Teilt Hive den Speicherort der Daten (das Verzeichnis "example/data") und die Information mit, dass die Speicherung als Text erfolgt.

   * **AUSWÄHLEN** – Wählt die Anzahl aller Zeilen aus, bei denen die Spalte "t4" den Wert "FEHLER" enthält.
     
     > [!NOTE]
     > Externe Tabellen sollten Sie verwenden, wenn Sie erwarten, dass die zugrunde liegenden Daten aus einer externen Quelle aktualisiert werden. Das könnte z.B. ein automatisierter Datenupload oder ein anderer MapReduce-Vorgang sein. Durch das Löschen einer externen Tabelle werden *nicht* die Daten, sondern nur die Tabellendefinitionen gelöscht.

2. Starten Sie die Abfrage mit der Schaltfläche **Ausführen** am unteren Rand des Abfrage-Editors. Sie wird nun orange angezeigt, und der Text ändert sich in **Ausführung beenden**. Der Abschnitt **Abfrageprozessergebnisse** sollte unterhalb des Abfrage-Editors angezeigt werden und Informationen über den Auftrag enthalten.
   
   > [!IMPORTANT]
   > In einigen Browsern werden das Protokoll oder die Ergebnisdaten möglicherweise nicht korrekt aktualisiert. Wenn Sie einen Auftrag ausführen und eine scheinbar endlose Ausführung ohne Aktualisierung des Protokolls oder Rückgabe von Ergebnissen feststellen, sollten Sie es noch einmal mit Mozilla Firefox oder Google Chrome versuchen.
 
3. Nach Abschluss der Abfrage werden im Abschnitt **Abfrageprozessergebnisse** die Ergebnisse des Vorgangs angezeigt. Die Schaltfläche **Ausführung beenden** wird auch wieder durch die grüne Schaltfläche **Ausführen** ersetzt, wenn die Abfrage ausgeführt ist. Die Registerkarte **Ergebnisse** sollte folgende Informationen enthalten:
   
        sev       cnt
        [ERROR]   3
   
    Auf der Registerkarte **Protokolle** können die vom Auftrag erstellten Protokollinformationen angezeigt werden.
   
   > [!TIP]
   > Im Dropdown-Dialogfeld **Ergebnisse speichern** in der oberen linken Ecke des Abschnitts **Abfrageprozessergebnisse** können Sie Ergebnisse herunterladen oder speichern.

4. Wählen Sie die ersten vier Zeilen dieser Abfrage aus, und klicken Sie auf **Ausführen**. Beachten Sie, dass keine Ergebnisse vorliegen, wenn der Auftrag abgeschlossen ist. Wenn ein Teil der Abfrage ausgewählt ist, werden bei Verwendung der Schaltfläche **Ausführen** nur die ausgewählten Anweisungen ausgeführt. In diesem Fall enthielt die Auswahl nicht die letzte Anweisung, die Zeilen aus der Tabelle abruft. Wenn Sie nur diese Zeile auswählen und **Ausführen** verwenden, sollten die erwarteten Ergebnisse angezeigt werden.

5. Um ein neues Arbeitsblatt hinzuzufügen, verwenden Sie die Schaltfläche **Neues Arbeitsblatt** am unteren Rand des **Abfrage-Editors**. Geben Sie in das neue Arbeitsblatt die folgenden HiveQL-Anweisungen ein:
   
    ```hiveql
    CREATE TABLE IF NOT EXISTS errorLogs (t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string) STORED AS ORC;
    INSERT OVERWRITE TABLE errorLogs SELECT t1, t2, t3, t4, t5, t6, t7 FROM log4jLogs WHERE t4 = '[ERROR]';
    ```
   
    These statements perform the following actions:
   
   * **TABELLE ERSTELLEN, FALLS NICHT VORHANDEN** – Erstellt eine Tabelle, sofern diese noch nicht vorhanden ist. Da das Schlüsselwort **EXTERN** nicht verwendet wird, wird eine interne Tabelle erstellt. Eine interne Tabelle wird im Hive-Data Warehouse gespeichert und vollständig von Hive verwaltet. Anders als bei externen Tabellen werden beim Löschen von internen Tabellen auch die zugrunde liegenden Daten gelöscht.

   * **ALS ORC GESPEICHERT** – Speichert die Daten im ORC-Format (Optimized Row Columnar). Dies ist ein stark optimiertes und effizientes Format zum Speichern von Hive-Daten.

   * **ÜBERSCHREIBEN EINFÜGEN ... AUSWÄHLEN**: Wählt Zeilen in der Tabelle **log4jLogs** aus, die [ERROR] enthalten, und fügt die Daten dann in die Tabelle **errorLogs** ein.
     
     Verwenden Sie die Schaltfläche **Ausführen**, um diese Abfrage auszuführen. Die Registerkarte **Ergebnisse** enthält keine Informationen, wenn die Abfrage keine Zeilen zurückgibt. Sobald die Abfrage abgeschlossen ist, sollte der Status **ERFOLGREICH** angezeigt werden.

### <a name="hive-settings"></a>Hive-Einstellungen

Wählen Sie rechts vom Editor das Symbol **Einstellungen**.

![Symbol „Einstellungen“ für Hive-Ansicht](./media/hdinsight-hadoop-use-hive-ambari-view/hive-view-settings-icon.png)

Die Einstellungen können verwendet werden, um verschiedene Hive-Einstellungen zu ändern, z.B. das Ausführungsmodul für Hive von Tez (Standard) in MapReduce.

### <a name="visualization"></a>Visualisierung

Wählen Sie rechts vom Editor das Symbol __Visuelle Erläuterung__.

![Symbol „Visuelle Erläuterung“ für Hive-Ansicht](./media/hdinsight-hadoop-use-hive-ambari-view/hive-view-visualization-icon.png)

Daraufhin wird die Benutzeroberfläche für visuelle Erläuterungen angezeigt, wo Sie visuelle Erläuterungen der von der Abfrage zurückgegebenen Daten erstellen können. Im Folgenden finden Sie ein Beispiel einer visuellen Erläuterung mit Daten aus der in HDInsight enthaltenen `hivesampletable`.

![Beispiel einer visuellen Erläuterung](./media/hdinsight-hadoop-use-hive-ambari-view/hive-view-visualization.png)

### <a name="visual-explain"></a>Visuelle Erläuterung

Wählen Sie rechts vom Editor das Symbol **Visuelle Erläuterung**.

![Symbol „Visuelle Erläuterung“ für Hive-Ansicht](./media/hdinsight-hadoop-use-hive-ambari-view/hive-view-visual-explain-icon.png)

Dies ist die Ansicht **Visuelle Erläuterung** der Abfrage, die das Verständnis des Ablaufs komplexer Abfragen erleichtern kann. Sie können eine Textentsprechung dieser Ansicht mit der Schaltfläche **Erklären** im Abfrage-Editor anzeigen.

![Bild zur visuellen Erläuterung](./media/hdinsight-hadoop-use-hive-ambari-view/visualexplain.png)

### <a name="tez"></a>Tez

Wählen Sie rechts vom Editor das Symbol **Tez**.

![Tez-Symbol für Hive-Ansicht](./media/hdinsight-hadoop-use-hive-ambari-view/hive-view-tez-icon.png)

Falls verfügbar, wird der von Tez für diese Abfrage verwendete gerichtete azyklische Graph (Directed Acyclic Graph, DAG) angezeigt. Wenn Sie den DAG für Abfragen anzeigen möchten, die Sie in der Vergangenheit ausgeführt haben, oder den Tez-Prozess debuggen möchten, verwenden Sie stattdessen die [Tez-Ansicht](hdinsight-debug-ambari-tez-view.md) .

### <a name="notifications"></a>Benachrichtigungen

Wählen Sie rechts vom Editor das Symbol **Benachrichtigungen** aus.

![Benachrichtigungensymbol für Hive-Ansicht](./media/hdinsight-hadoop-use-hive-ambari-view/hive-view-notifications-icon.png)

Benachrichtigungen sind Nachrichten, die beim Ausführen von Abfragen generiert werden. Beispielsweise erhalten Sie eine Benachrichtigung, wenn eine Abfrage gesendet wird oder ein Fehler auftritt.

## <a name="saved-queries"></a>Gespeicherte Abfragen

1. Erstellen Sie im Abfrage-Editor ein Arbeitsblatt, und geben Sie die folgende Abfrage ein:
   
    ```hiveql
    SELECT * from errorLogs;
    ```
   
    Führen Sie die Abfrage aus, um zu überprüfen, ob sie funktioniert. Die Ergebnisse entsprechen etwa folgendem Beispiel:
   
        errorlogs.t1     errorlogs.t2     errorlogs.t3     errorlogs.t4     errorlogs.t5     errorlogs.t6     errorlogs.t7
        2012-02-03     18:35:34     SampleClass0     [ERROR]     incorrect     id     
        2012-02-03     18:55:54     SampleClass1     [ERROR]     incorrect     id     
        2012-02-03     19:25:27     SampleClass4     [ERROR]     incorrect     id

2. Klicken Sie unten im Editor auf die Schaltfläche **Speichern unter**. Geben Sie der Abfrage den Namen **Errorlogs**, und wählen Sie **OK**. Der Name des Arbeitsblatts ändert sich in **Errorlogs**.

3. Wählen Sie oben auf der Seite mit der Hive-Ansicht die Registerkarte **Gespeicherte Abfragen** aus. **Errorlogs** wird jetzt als gespeicherte Abfrage aufgeführt. Sie bleibt in dieser Liste enthalten, bis Sie sie entfernen. Bei Auswahl des Namens wird die Abfrage im Abfrage-Editor geöffnet.

## <a name="query-history"></a>Abfrageverlauf

Mit der Schaltfläche **Verlauf** oben in der Hive-Ansicht können Sie Abfragen anzeigen, die Sie bereits ausgeführt haben. Verwenden Sie diese Schaltfläche, und wählen Sie einige Abfragen aus, die Sie bereits ausgeführt haben. Wenn Sie eine Abfrage auswählen, wird sie im Abfrage-Editor geöffnet.

## <a name="user-defined-functions-udf"></a>Benutzerdefinierte Funktionen (User Defined Functions, UDFs)

Hive kann auch über **benutzerdefinierte Funktionen (UDF)**erweitert werden. Mit einer UDF-Datei können Sie Funktionen oder Logik implementieren, die sich nicht einfach in HiveQL modellieren lässt.

Mit der Registerkarte „UDF“ oben in der Hive-Ansicht können Sie auch eine Gruppe von UDFs deklarieren und speichern, die mit dem **Abfrage-Editor** verwendet werden können.

Nachdem Sie der Hive-Ansicht eine benutzerdefinierte Funktion hinzugefügt haben, wird unten im **Abfrage-Editor** die Schaltfläche **UDFs einfügen** angezeigt. Wenn Sie diese Schaltfläche wählen, wird eine Dropdownliste mit den UDFs angezeigt, die in der Hive-Ansicht definiert sind. Durch das Auswählen einer UDF werden Ihrer Abfrage HiveQL-Anweisungen hinzugefügt, um die UDF zu aktivieren.

Beispielsweise können Sie eine UDF mit den folgenden Eigenschaften definieren:

* Ressourcenname: myudfs

* Ressourcenpfad: /myudfs.jar

* UDF-Name: myawesomeudf

* UDF-Klassenname: com.myudfs.Awesome

Mit der Schaltfläche **UDFs einfügen** wird ein Eintrag mit dem Namen **myudfs** angezeigt, der über eine weitere Dropdownliste für jede UDF verfügt, die für diese Ressource definiert ist. In diesem Fall ist das **myawesomeudf**. Wenn Sie diesen Eintrag auswählen, wird am Anfang der Abfrage Folgendes hinzugefügt:

```hiveql
add jar /myudfs.jar;
create temporary function myawesomeudf as 'com.myudfs.Awesome';
```

Sie können die UDF dann in Ihrer Abfrage verwenden. Beispiel: `SELECT myawesomeudf(name) FROM people;`.

Weitere Informationen zur Verwendung von UDFs mit Hive unter HDInsight finden Sie in den folgenden Artikeln:

* [Verwenden von Python mit Hive und Pig in HDInsight](hdinsight-python.md)
* [How to add a custom Hive UDF to HDInsight (in englischer Sprache)](http://blogs.msdn.com/b/bigdatasupport/archive/2014/01/14/how-to-add-custom-hive-udfs-to-hdinsight.aspx)

## <a id="nextsteps"></a>Nächste Schritte
Allgemeine Informationen zu Hive in HDInsight:

* [Verwenden von Hive mit Hadoop in HDInsight](hdinsight-use-hive.md)

Informationen zu anderen Möglichkeiten, wie Sie mit Hadoop in HDInsight arbeiten können:

* [Verwenden von Pig mit Hadoop in HDInsight](hdinsight-use-pig.md)
* [Verwenden von MapReduce mit Hadoop in HDInsight](hdinsight-use-mapreduce.md)


