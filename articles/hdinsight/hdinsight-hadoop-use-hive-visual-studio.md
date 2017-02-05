---
title: "Hive-Abfrage mit Hadoop-Tools für Visual Studio | Microsoft-Dokumentation"
description: "Erfahren Sie, wie Sie Hive mit Hadoop in HDInsight mit den Hadoop-Tools für Visual Studio verwenden."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 2b3e672a-1195-4fa5-afb7-b7b73937bfbe
ms.service: hdinsight
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 09/06/2016
ms.author: larryfr
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: c14547e54d7f09267b12f3bbe22e45e51ff08744


---
# <a name="run-hive-queries-using-the-hdinsight-tools-for-visual-studio"></a>Ausführen von Hive-Abfragen mit HDInsight Tools für Visual Studio
[!INCLUDE [hive-selector](../../includes/hdinsight-selector-use-hive.md)]

In diesem Artikel erfahren Sie, wie Sie mit HDInsight Tools für Visual Studio Hive-Abfragen an einen HDInsight-Cluster übermitteln.

> [!NOTE]
> Dieses Dokument bietet keine detaillierte Beschreibung dazu, wie die in diesem Beispiel verwendeten HiveQL-Anweisungen vorgehen. Informationen zu der HiveQL, die in diesem Beispiel verwendet wird, finden Sie unter [Verwenden von Hive mit Hadoop in HDInsight](hdinsight-use-hive.md).
> 
> 

## <a name="a-idprereqaprerequisites"></a><a id="prereq"></a>Voraussetzungen
Damit Sie die in dieser Artikel aufgeführten Schritte ausführen können, benötigen Sie Folgendes:

* Einen Azure HDInsight-Cluster (Hadoop in HDInsight), der auf Linux oder Windows basiert
* Visual Studio (eine der folgenden Versionen):
  
    Visual Studio 2013 Community/Professional/Premium/Ultimate mit [Update 4](https://www.microsoft.com/download/details.aspx?id=44921)
  
    Visual Studio 2015 (Community/Enterprise)
* HDInsight Tools für Visual Studio. Informationen zum Installieren und Konfigurieren der Tools finden Sie unter [Erste Schritte bei der Verwendung von Hadoop-Tools für Visual Studio für HDInsight](hdinsight-hadoop-visual-studio-tools-get-started.md) .

## <a name="a-idruna-run-hive-queries-using-the-hdinsight-tools-for-visual-studio"></a><a id="run"></a> Ausführen von Hive-Abfragen mit HDInsight Tools für Visual Studio
1. Öffnen Sie **Visual Studio**, wählen Sie **Neu** > **Projekt** > **HDInsight** > **Hive-Anwendung**. Geben Sie einen Namen für dieses Projekt an.
2. Öffnen Sie die Datei **Script.hql** , die mit diesem Projekt erstellt wird, und fügen Sie dann die folgenden HiveQL-Anweisungen ein:
   
        set hive.execution.engine=tez;
        DROP TABLE log4jLogs;
        CREATE EXTERNAL TABLE log4jLogs (t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string)
        ROW FORMAT DELIMITED FIELDS TERMINATED BY ' '
        STORED AS TEXTFILE LOCATION 'wasbs:///example/data/';
        SELECT t4 AS sev, COUNT(*) AS count FROM log4jLogs WHERE t4 = '[ERROR]' AND INPUT__FILE__NAME LIKE '%.log' GROUP BY t4;
   
    Diese Anweisungen führen die folgenden Aktionen aus:
   
   * **TABELLE LÖSCHEN**: Löscht Tabelle und Datendatei, falls die Tabelle bereits vorhanden ist.
   * **CREATE EXTERNAL TABLE**: Erstellt eine neue „externe“ Tabelle in Hive. Externe Tabellen speichern nur die Tabellendefinition in Hive. Die Daten verbleiben am ursprünglichen Speicherort.
     
     > [!NOTE]
     > Wenn erwartet wird, dass die zugrunde liegenden Daten über eine externe Quelle (z. B. einen automatisierten Prozess zum Hochladen von Daten) oder über einen anderen MapReduce-Vorgang aktualisiert, aber von Hive immer die neuesten Daten verwendet werden, sollten externe Tabellen verwendet werden.
     > 
     > Durch das Löschen einer externen Tabelle werden **nicht** die Daten, sondern nur die Tabellendefinitionen gelöscht.
     > 
     > 
   * **ZEILENFORMAT**: Teilt Hive mit, wie die Daten formatiert werden. In diesem Fall werden die Felder in den einzelnen Protokollen durch Leerzeichen getrennt.
   * **SPEICHERORT DER TEXTDATEI**: Teilt Hive den Speicherort der Daten (das Verzeichnis "Beispiel/Daten") und die Information mit, dass die Speicherung als Text erfolgt.
   * **SELECT**: Wählt die Anzahl aller Zeilen aus, bei denen die Spalte **t4** den Wert **[ERROR]** enthält. Dadurch sollte der Wert **3** zurückgegeben werden, da dieser Wert in drei Zeilen enthalten ist.
   * **INPUT__FILE__NAME LIKE '%.log'**: Teilt Hive mit, dass nur Daten aus Dateien mit der Erweiterung „.log“ zurückgeben werden sollen. Dies schränkt die Suche auf die Datei "sample.log" ein, die die Daten enthält, und verhindert, dass Daten aus anderen Beispieldatendateien zurückgegeben werden, die nicht dem von uns definierten Schema entsprechen.
3. Wählen Sie auf der Symbolleiste den **HDInsight-Cluster** aus, den Sie für diese Abfrage verwenden möchten, und wählen Sie dann **An WebHCat übermitteln**, um die Anweisungen als Hive-Auftrag mit WebHCat auszuführen. Wenn HiveServer2 in Ihrer Clusterversion verfügbar ist, können Sie den Auftrag auch über die Schaltfläche **Über HiveServer2 ausführen** übermitteln. Die **Hive-Auftragszusammenfassung** wird mit Informationen zum aktiven Auftrag angezeigt. Verwenden Sie den Link **Aktualisieren**, um die Auftragsinformationen zu aktualisieren, bis der **Auftragsstatus** zu **Abgeschlossen** wechselt.
4. Verwenden Sie den Link **Auftragsausgabe** , um die Ausgabe dieses Auftrags anzuzeigen. Es sollte `[ERROR] 3`angezeigt werden, was dem Wert entspricht, der von der Anweisung AUSWÄHLEN zurückgegeben wurde.
5. Sie können Hive-Abfragen auch ohne Erstellung eines Projekts ausführen. Erweitern Sie im **Server-Explorer** die Option **Azure** > **HDInsight**, klicken Sie dann mit der rechten Maustaste auf den HDInsight-Server, und wählen Sie anschließend **Hive-Abfrage schreiben** aus.
6. Fügen Sie im sich daraufhin öffnenden Dokument **temp.hql** die folgenden HiveQL-Anweisungen hinzu:
   
        set hive.execution.engine=tez;
        CREATE TABLE IF NOT EXISTS errorLogs (t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string) STORED AS ORC;
        INSERT OVERWRITE TABLE errorLogs SELECT t1, t2, t3, t4, t5, t6, t7 FROM log4jLogs WHERE t4 = '[ERROR]' AND INPUT__FILE__NAME LIKE '%.log';
   
    Diese Anweisungen führen die folgenden Aktionen aus:
   
   * **TABELLE ERSTELLEN, FALLS NICHT VORHANDEN**: Erstellt eine Tabelle, sofern diese noch nicht vorhanden ist. Da das Schlüsselwort **EXTERN** nicht verwendet wird, ist dies eine interne Tabelle, die im Hive-Data-Warehouse gespeichert und vollständig von Hive verwaltet wird.
     
     > [!NOTE]
     > Anders als bei **EXTERNEN** Tabellen werden beim Löschen von internen Tabellen auch die zugrunde liegenden Daten gelöscht.
     > 
     > 
   * **ALS ORC GESPEICHERT**: Speichert die Daten im ORC-Format (Optimized Row Columnar). Dies ist ein stark optimiertes und effizientes Format zum Speichern von Hive-Daten.
   * **ÜBERSCHREIBEN EINFÜGEN ... SELECT**: Wählt die Zeilen aus der Tabelle **log4jLogs** aus, die den Wert **[ERROR]** enthalten. Dann werden die Daten in die Tabelle **errorLogs** eingefügt.
7. Wählen Sie auf der Symbolleiste **Übermitteln** aus, um den Auftrag auszuführen. Verwenden Sie **Auftragsstatus** , um zu ermitteln, ob der Auftrag erfolgreich abgeschlossen wurde.
8. Verwenden Sie den **Server-Explorer**, und erweitern Sie **Azure** > **HDInsight** > Ihren HDInsight-Cluster > **Hive-Datenbanken** > **Standard**, um zu überprüfen, ob der Auftrag abgeschlossen und eine neue Tabelle erstellt wurde. Es sollten die beiden Tabellen **errorLogs** und **log4jLogs** angezeigt werden.

## <a name="a-idsummaryasummary"></a><a id="summary"></a>Zusammenfassung
Wie Sie sehen können, bieten die HDInsight Tools für Visual Studio eine einfache Möglichkeit, um Hive-Abfragen auf einem HDInsight-Cluster auszuführen, den Auftragsstatus zu überwachen und die Ausgabe abzurufen.

## <a name="a-idnextstepsanext-steps"></a><a id="nextsteps"></a>Nächste Schritte
Allgemeine Informationen zu Hive in HDInsight:

* [Verwenden von Hive mit Hadoop in HDInsight](hdinsight-use-hive.md)

Informationen zu anderen Möglichkeiten, wie Sie mit Hadoop in HDInsight arbeiten können:

* [Verwenden von Pig mit Hadoop in HDInsight](hdinsight-use-pig.md)
* [Verwenden von MapReduce mit Hadoop in HDInsight](hdinsight-use-mapreduce.md)

Weitere Informationen zu den HDInsight Tools für Visual Studio:

* [Erste Schritte mit HDInsight Tools für Visual Studio](hdinsight-hadoop-visual-studio-tools-get-started.md)

[hdinsight-sdk-documentation]: http://msdnstage.redmond.corp.microsoft.com/library/dn479185.aspx

[azure-purchase-options]: http://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: http://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: http://azure.microsoft.com/pricing/free-trial/

[apache-tez]: http://tez.apache.org
[apache-hive]: http://hive.apache.org/
[apache-log4j]: http://en.wikipedia.org/wiki/Log4j
[hive-on-tez-wiki]: https://cwiki.apache.org/confluence/display/Hive/Hive+on+Tez
[import-to-excel]: http://azure.microsoft.com/documentation/articles/hdinsight-connect-excel-power-query/


[hdinsight-use-oozie]: hdinsight-use-oozie.md
[hdinsight-analyze-flight-data]: hdinsight-analyze-flight-delay-data.md



[hdinsight-storage]: hdinsight-hadoop-use-blob-storage.md

[hdinsight-provision]: hdinsight-provision-clusters.md
[hdinsight-submit-jobs]: hdinsight-submit-hadoop-jobs-programmatically.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-get-started]: hdinsight-hadoop-linux-tutorial-get-started.md

[powershell-here-strings]: http://technet.microsoft.com/library/ee692792.aspx

[image-hdi-hive-powershell]: ./media/hdinsight-use-hive/HDI.HIVE.PowerShell.png
[img-hdi-hive-powershell-output]: ./media/hdinsight-use-hive/HDI.Hive.PowerShell.Output.png
[image-hdi-hive-architecture]: ./media/hdinsight-use-hive/HDI.Hive.Architecture.png



<!--HONumber=Nov16_HO3-->


