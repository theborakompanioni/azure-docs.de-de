---
title: Verwenden von Ambari-Ansichten zum Arbeiten mit Hive in HDInsight (Hadoop) | Microsoft Docs
description: Erfahren Sie, wie Sie die Hive-Ansicht im Webbrowser zum Übermitteln von Hive-Abfragen verwenden. Die Hive-Ansicht ist Teil der Ambari-Webbenutzeroberfläche, die mit dem Linux-basierten HDInsight-Cluster bereitgestellt wird.
services: hdinsight
documentationcenter: ''
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal

ms.service: hdinsight
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 08/09/2016
ms.author: larryfr

---
# Verwenden der Hive-Ansicht mit Hadoop in HDInsight
[!INCLUDE [Hive-Selektor](../../includes/hdinsight-selector-use-hive.md)]

Ambari ist ein Verwaltungs- und Überwachungsdienstprogramm, das mit Linux-basierten HDInsight-Clustern bereitgestellt wird. Eines der in Ambari bereitgestellten Features ist eine Webbenutzeroberfläche, die zum Ausführen von Hive-Abfragen verwendet werden kann. Dies ist die **Hive-Ansicht**, die Teil der Ambari-Ansichten des HDInsight-Clusters ist.

> [!NOTE]
> Ambari verfügt über viele Funktionen, die in diesem Dokument nicht beschrieben werden. Weitere Informationen finden Sie unter [Verwalten von HDInsight-Clustern mithilfe der Ambari-Webbenutzeroberfläche](hdinsight-hadoop-manage-ambari.md).
> 
> 

## Voraussetzungen
* Ein Linux-basierter HDInsight-Cluster. Informationen zum Erstellen eines neuen Clusters finden Sie unter [Erste Schritte mit Linux-basiertem HDInsight](hdinsight-hadoop-linux-tutorial-get-started.md).

## Öffnen der Hive-Ansicht
Ambari-Ansichten sind im Azure-Portal verfügbar. Wählen Sie Ihren HDInsight-Cluster, und wählen Sie dann **Ambari-Ansichten** im Abschnitt **Quicklinks**.

![Quicklinks-Abschnitt](./media/hdinsight-hadoop-use-hive-ambari-view/quicklinks.png)

Sie können auch direkt zu Ambari navigieren, indem Sie in einem Webbrowser https://CLUSTERNAME.azurehdinsight.net aufrufen (wobei **CLUSTERNAME** der Name Ihres HDInsight-Clusters ist) und dann den Satz von Quadraten im Seitenmenü auswählen (neben dem **Admin**-Link und der Schaltfläche links auf der Seite), um verfügbare Ansichten aufzulisten. Wählen Sie die **Hive-Ansicht** aus.

![Ambari-Ansichten auswählen](./media/hdinsight-hadoop-use-hive-ambari-view/selecthiveview.png).

> [!NOTE]
> Beim Zugriff auf Ambari werden Sie aufgefordert, sich bei der Website zu authentifizieren. Geben Sie den Administratorkontonamen (Standard-`admin`) und das Kennwort ein, den bzw. das Sie beim Erstellen des Clusters verwendet haben.
> 
> 

Eine Seite ähnlich der folgenden wird angezeigt:

![Bild der Hive-Ansichtenseite, die einen Abfrage-Editor-Abschnitt enthält](./media/hdinsight-hadoop-use-hive-ambari-view/hiveview.png)

## Anzeigen von Tabellen
Wählen Sie im Abschnitt **Datenbank-Explorer** der Seite auf der Registerkarte **Datenbanken** den Eintrag **Standard** aus. Eine Liste mit den Tabellen in der Standarddatenbank wird angezeigt. Für einen neuen HDInsight-Cluster sollte nur eine Tabelle vorhanden sein: **hivesampletable**.

![Datenbank-Explorer mit erweiterter Standarddatenbank](./media/hdinsight-hadoop-use-hive-ambari-view/databaseexplorer.png)

Wenn mit den Schritten in diesem Dokument neue Tabellen hinzugefügt werden, können Sie das Symbol „Aktualisieren“ oben rechts im Datenbank-Explorer verwenden, um die Liste mit den verfügbaren Tabellen zu aktualisieren.

## <a name="hivequery"></a>Abfrage-Editor
Führen Sie in der Hive-Ansicht folgende Schritte aus, um eine Hive-Abfrage an Daten auszuführen, die im Cluster enthalten sind.

1. Fügen Sie im Abschnitt **Abfrage-Editor** der Seite die folgenden HiveQL-Anweisungen in das Arbeitsblatt ein:
   
        DROP TABLE log4jLogs;
        CREATE EXTERNAL TABLE log4jLogs(t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string)
        ROW FORMAT DELIMITED FIELDS TERMINATED BY ' '
        STORED AS TEXTFILE LOCATION 'wasbs:///example/data/';
        SELECT t4 AS sev, COUNT(*) AS cnt FROM log4jLogs WHERE t4 = '[ERROR]' GROUP BY t4;
   
    Diese Anweisungen führen die folgenden Aktionen aus:
   
   * **DROP TABLE** löscht die Tabelle und Datendatei, falls die Tabelle bereits vorhanden ist.
   * **CREATE EXTERNAL TABLE** erstellt eine neue externe Tabelle in Hive. Externe Tabellen dienen nur zum Speichern der Tabellendefinition in Hive. Die Daten verbleiben am ursprünglichen Speicherort.
   * **ROW FORMAT** teilt Hive mit, wie die Daten formatiert werden sollen. In diesem Fall werden die Felder in den einzelnen Protokollen durch Leerzeichen getrennt.
   * **SPEICHERORT DER TEXTDATEI** – Teilt Hive den Speicherort der Daten (das Verzeichnis "example/data") und die Information mit, dass die Speicherung als Text erfolgt.
   * **AUSWÄHLEN** – Wählt die Anzahl aller Zeilen aus, bei denen die Spalte "t4" den Wert "FEHLER" enthält.
     
     > [!NOTE]
     > Wenn erwartet wird, dass die zugrunde liegenden Daten über eine externe Quelle, z. B. einen automatisierten Prozess zum Hochladen von Daten, oder über einen anderen MapReduce-Vorgang aktualisiert, aber von Hive immer die neuesten Daten verwendet werden, sollten externe Tabellen verwendet werden. Durch das Löschen einer externen Tabelle werden *nicht* die Daten, sondern nur die Tabellendefinitionen gelöscht.
     > 
     > 
2. Starten Sie die Abfrage mit der Schaltfläche **Ausführen** am unteren Rand des Abfrage-Editors. Deren Farbe sollte zu Orange wechseln, und der Text sollte sich in **Ausführung beenden** ändern. Der Abschnitt **Abfrageprozessergebnisse** sollte unterhalb des Abfrage-Editors angezeigt werden und Informationen über den Auftrag enthalten.
   
   > [!IMPORTANT]
   > In einigen Browsern werden das Protokoll oder die Ergebnisdaten möglicherweise nicht korrekt aktualisiert. Wenn Sie einen Auftrag ausführen und eine scheinbar endlose Ausführung ohne Aktualisierung des Protokolls oder Rückgabe von Ergebnissen feststellen, sollten Sie es noch einmal mit Mozilla Firefox oder Google Chrome versuchen.
   > 
   > 
3. Nach Abschluss der Abfrage werden im Abschnitt **Abfrageprozessergebnisse** die Ergebnisse des Vorgangs angezeigt. Die Schaltfläche **Ausführung beenden** ändert sich auch wieder in die grüne Schaltfläche **Ausführen**. Die Registerkarte **Ergebnisse** sollte folgende Informationen enthalten:
   
        sev       cnt
        [ERROR]   3
   
    Auf der Registerkarte **Protokolle** können die vom Auftrag erstellten Protokollinformationen angezeigt werden. Sie können sie zur Problembehandlung verwenden, wenn Probleme mit einer Abfrage auftreten.
   
   > [!TIP]
   > Beachten Sie das Dropdown-Dialogfeld **Ergebnisse speichern** in der oberen linken Ecke des Abschnitts **Abfrageprozessergebnisse**. Hiermit können Sie die Ergebnisse entweder herunterladen oder im HDInsight-Speicher als CSV-Datei speichern.
   > 
   > 
4. Wählen Sie die ersten vier Zeilen dieser Abfrage aus, und klicken Sie auf **Ausführen**. Beachten Sie, dass keine Ergebnisse vorliegen, wenn der Auftrag abgeschlossen ist. Der Grund ist, dass die Schaltfläche **Ausführen**, wenn ein Teil der Abfrage ausgewählt ist, nur die ausgewählten Anweisungen ausführt. In diesem Fall enthielt die Auswahl nicht die letzte Anweisung, die Zeilen aus der Tabelle abruft. Wenn Sie nur diese Zeile auswählen und **Ausführen** verwenden, sollten die erwarteten Ergebnisse angezeigt werden.
5. Verwenden Sie die Schaltfläche **Neues Arbeitsblatt** am unteren Rand des **Abfrage-Editors**, um ein neues Arbeitsblatt zu erstellen. Geben Sie in das neue Arbeitsblatt die folgenden HiveQL-Anweisungen ein:
   
        CREATE TABLE IF NOT EXISTS errorLogs (t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string) STORED AS ORC;
        INSERT OVERWRITE TABLE errorLogs SELECT t1, t2, t3, t4, t5, t6, t7 FROM log4jLogs WHERE t4 = '[ERROR]';
   
    Diese Anweisungen führen die folgenden Aktionen aus:
   
   * **TABELLE ERSTELLEN, FALLS NICHT VORHANDEN** – Erstellt eine Tabelle, sofern diese noch nicht vorhanden ist. Da das Schlüsselwort **EXTERN** nicht verwendet wird, ist dies eine "interne" Tabelle, die im Hive-Data Warehouse gespeichert und vollständig von Hive verwaltet wird. Im Gegensatz zu externen Tabellen werden beim Ablegen von internen Tabellen auch die zugrunde liegenden Daten gelöscht.
   * **ALS ORC GESPEICHERT** – Speichert die Daten im ORC-Format (Optimized Row Columnar) Dies ist ein stark optimiertes und effizientes Format zum Speichern von Hive-Daten.
   * **ÜBERSCHREIBEN EINFÜGEN ... AUSWÄHLEN** – Wählt Zeilen in der Tabelle **log4jLogs** aus, die [FEHLER] enthalten, und fügt die Daten dann in die Tabelle **errorLogs** ein.
     
     Verwenden Sie die Schaltfläche **Ausführen**, um diese Abfrage auszuführen. Die Registerkarte **Ergebnisse** enthält keine Informationen, da diese Abfrage keine Zeilen zurückgibt, aber der Status **ERFOLGREICH** sollte angezeigt werden.

### Hive-Einstellungen
Wählen Sie rechts vom Editor das Symbol **Einstellungen**.

![Symbole](./media/hdinsight-hadoop-use-hive-ambari-view/settings.png)

Die Einstellungen können verwendet werden, um verschiedene Hive-Einstellungen zu ändern, z. B. das Ausführungsmodul für Hive von Tez (Standard) in MapReduce.

### Visuelle Erläuterung
Wählen Sie rechts vom Editor das Symbol **Visuelle Erläuterung**.

![Symbole](./media/hdinsight-hadoop-use-hive-ambari-view/visualexplainicon.png)

Dies ist die Ansicht **Visuelle Erläuterung** der Abfrage, die das Verständnis des Ablaufs komplexer Abfragen erleichtern kann. Sie können eine Textentsprechung dieser Ansicht mit der Schaltfläche **Erklären** im Abfrage-Editor anzeigen.

![Bild zur visuellen Erläuterung](./media/hdinsight-hadoop-use-hive-ambari-view/visualexplain.png)

### Tez
Wählen Sie rechts vom Editor das Symbol **Tez**.

![Symbole](./media/hdinsight-hadoop-use-hive-ambari-view/tez.png)

Falls verfügbar, wird der von Tez für diese Abfrage verwendete gerichtete azyklische Graph (Directed Acyclic Graph, DAG) angezeigt. Wenn Sie den DAG für Abfragen anzeigen möchten, die Sie in der Vergangenheit ausgeführt haben, oder den Tez-Prozess debuggen möchten, verwenden Sie stattdessen die [Tez-Ansicht](hdinsight-debug-ambari-tez-view.md).

### Benachrichtigungen
Wählen Sie rechts vom Editor das Symbol **Benachrichtigungen** aus.

![Symbole](./media/hdinsight-hadoop-use-hive-ambari-view/notifications.png)

Benachrichtigungen sind Nachrichten, die beim Ausführen von Abfragen generiert werden. Beispielsweise erhalten Sie eine Benachrichtigung, wenn eine Abfrage gesendet wird oder ein Fehler auftritt.

## Gespeicherte Abfragen
1. Erstellen Sie im Abfrage-Editor ein neues Arbeitsblatt, und geben Sie die folgende Abfrage ein:
   
        SELECT * from errorLogs;
   
    Führen Sie die Abfrage aus, um zu überprüfen, ob sie funktioniert. So sehen die Ergebnisse aus:
   
        errorlogs.t1     errorlogs.t2     errorlogs.t3     errorlogs.t4     errorlogs.t5     errorlogs.t6     errorlogs.t7
        2012-02-03     18:35:34     SampleClass0     [ERROR]     incorrect     id     
        2012-02-03     18:55:54     SampleClass1     [ERROR]     incorrect     id     
        2012-02-03     19:25:27     SampleClass4     [ERROR]     incorrect     id
2. Klicken Sie unten im Editor auf die Schaltfläche **Speichern unter**. Geben Sie der Abfrage den Namen **Errorlogs**, und wählen Sie **OK**. Beachten Sie, dass sich der Name des Arbeitsblatts in **Errorlogs** ändert.
3. Wählen Sie oben auf der Seite mit der Hive-Ansicht die Registerkarte **Gespeicherte Abfragen** aus. Beachten Sie, dass **Errorlogs** jetzt als gespeicherte Abfrage aufgeführt ist. Sie bleibt in dieser Liste enthalten, bis Sie sie entfernen. Bei Auswahl des Namens wird die Abfrage im Abfrage-Editor geöffnet.

## Abfrageverlauf
Mit der Schaltfläche **Verlauf** oben in der Hive-Ansicht können Sie Abfragen anzeigen, die Sie bereits ausgeführt haben. Verwenden Sie diese Schaltfläche, und wählen Sie einige Abfragen aus, die Sie bereits ausgeführt haben. Wenn Sie eine Abfrage auswählen, wird sie im Abfrage-Editor geöffnet.

## Benutzerdefinierte Funktionen (User Defined Functions, UDFs)
Hive kann auch über **benutzerdefinierte Funktionen (UDF)** erweitert werden. Mit einer UDF-Datei können Sie Funktionen oder Logik implementieren, die sich nicht einfach in HiveQL modellieren lässt.

Sie können eine benutzerdefinierte Funktion als Teil der HiveQL-Anweisungen in der Abfrage hinzufügen. Mit der Registerkarte „UDF“ oben in der Hive-Ansicht können Sie auch eine Gruppe von UDFs deklarieren und speichern, die mit dem **Abfrage-Editor** verwendet werden können.

Nachdem Sie der Hive-Ansicht eine benutzerdefinierte Funktion hinzugefügt haben, wird unten im **Abfrage-Editor** die Schaltfläche **UDFs einfügen** angezeigt. Wenn Sie diese Schaltfläche wählen, wird eine Dropdownliste mit den UDFs angezeigt, die in der Hive-Ansicht definiert sind. Durch das Auswählen einer UDF werden Ihrer Abfrage HiveQL-Anweisungen hinzugefügt, um die UDF zu aktivieren.

Beispielsweise können Sie eine UDF mit den folgenden Eigenschaften definieren:

* Ressourcenname: myudfs
* Ressourcenpfad: wasbs:///myudfs.jar
* UDF-Name: myawesomeudf
* UDF-Klassenname: com.myudfs.Awesome

Mit der Schaltfläche **UDFs einfügen** wird ein Eintrag mit dem Namen **myudfs** angezeigt, der über eine weitere Dropdownliste für jede UDF verfügt, die für diese Ressource definiert ist. In diesem Fall ist dies **myawesomeudf**. Wenn Sie diesen Eintrag auswählen, wird am Anfang der Abfrage Folgendes hinzugefügt:

    add jar wasbs:///myudfs.jar;

    create temporary function myawesomeudf as 'com.myudfs.Awesome';

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

<!---HONumber=AcomDC_0914_2016-->