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
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 02/28/2017
ms.author: larryfr
translationtype: Human Translation
ms.sourcegitcommit: 785d3a8920d48e11e80048665e9866f16c514cf7
ms.openlocfilehash: 125531d8d52d4e27951a9e6cae0c50582c5b110e
ms.lasthandoff: 04/12/2017


---
# <a name="run-hive-queries-using-the-hdinsight-tools-for-visual-studio"></a>Ausführen von Hive-Abfragen mit HDInsight Tools für Visual Studio

[!INCLUDE [hive-selector](../../includes/hdinsight-selector-use-hive.md)]

Erfahren Sie, wie Sie mit HDInsight-Tools für Visual Studio Hive-Abfragen an einen HDInsight-Cluster übermitteln.

## <a id="prereq"></a>Voraussetzungen

Zur Ausführung der in diesem Artikel aufgeführten Schritte benötigen Sie Folgendes.

* Einen Azure HDInsight-Cluster (Hadoop in HDInsight)

  > [!IMPORTANT]
  > Linux ist das einzige Betriebssystem, das unter HDInsight Version 3.4 oder höher verwendet wird. Weitere Informationen finden Sie unter [Ende des Lebenszyklus von HDInsight unter Windows](hdinsight-component-versioning.md#hdi-version-33-nearing-deprecation-date).

* Visual Studio (eine der folgenden Versionen):

    * Visual Studio 2013 Community/Professional/Premium/Ultimate mit [Update 4](https://www.microsoft.com/download/details.aspx?id=44921)

    * Visual Studio 2015 (beliebige Edition)

    * Visual Studio 2017 (beliebige Edition)

* HDInsight-Tools für Visual Studio oder Azure Data Lake-Tools für Visual Studio. Informationen zum Installieren und Konfigurieren der Tools finden Sie unter [Erste Schritte bei der Verwendung von Hadoop-Tools für Visual Studio für HDInsight](hdinsight-hadoop-visual-studio-tools-get-started.md) .

## <a id="run"></a> Ausführen von Hive-Abfragen mit Visual Studio

1. Öffnen Sie **Visual Studio**, wählen Sie **Neu** > **Projekt** > **Azure Data Lake** > **HIVE** > **Hive-Anwendung**. Geben Sie einen Namen für dieses Projekt an.

2. Öffnen Sie die Datei **Script.hql** , die mit diesem Projekt erstellt wird, und fügen Sie dann die folgenden HiveQL-Anweisungen ein:

   ```hiveql
   set hive.execution.engine=tez;
   DROP TABLE log4jLogs;
   CREATE EXTERNAL TABLE log4jLogs (t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string)
   ROW FORMAT DELIMITED FIELDS TERMINATED BY ' '
   STORED AS TEXTFILE LOCATION '/example/data/';
   SELECT t4 AS sev, COUNT(*) AS count FROM log4jLogs WHERE t4 = '[ERROR]' AND  INPUT__FILE__NAME LIKE '%.log' GROUP BY t4;
   ```

    Diese Anweisungen führen die folgenden Aktionen aus:

   * `DROP TABLE`: Wenn die Tabelle vorhanden ist, wird sie von dieser Anweisung gelöscht.

   * `CREATE EXTERNAL TABLE`: Erstellt eine neue „externe“ Tabelle in Hive. Externe Tabellen speichern nur die Tabellendefinition in Hive. Die Daten verbleiben am ursprünglichen Speicherort.

     > [!NOTE]
     > Wenn erwartet wird, dass die zugrunde liegenden Daten über eine externe Quelle (z. B. einen automatisierten Prozess zum Hochladen von Daten) oder über einen anderen MapReduce-Vorgang aktualisiert, aber von Hive immer die neuesten Daten verwendet werden, sollten externe Tabellen verwendet werden.
     >
     > Durch das Löschen einer externen Tabelle werden **nicht** die Daten, sondern nur die Tabellendefinitionen gelöscht.

   * `ROW FORMAT`: Teilt Hive mit, wie die Daten formatiert werden. In diesem Fall werden die Felder in den einzelnen Protokollen durch Leerzeichen getrennt.

   * `STORED AS TEXTFILE LOCATION`: Teilt Hive den Speicherort der Daten (das Verzeichnis „example/data“) und die Information mit, dass die Speicherung als Text erfolgt.

   * `SELECT`: Wählt die Anzahl aller Zeilen aus, bei denen die Spalte `t4` den Wert `[ERROR]` enthält. Mit dieser Anweisung sollte der Wert `3` zurückgegeben werden, da dieser Wert in drei Zeilen enthalten ist.

   * `INPUT__FILE__NAME LIKE '%.log'`: Teilt Hive mit, dass nur Daten aus Dateien mit der Erweiterung „.log“ zurückgeben werden sollen. Diese Klausel schränkt die Suche auf die Datei „sample.log“ ein, die die Daten enthält.

3. Wählen Sie auf der Symbolleiste den **HDInsight-Cluster**, den Sie für diese Abfrage verwenden möchten. Wählen Sie **Übermitteln**, um die Anweisungen als Hive-Auftrag auszuführen.

   ![Leiste „Übermitteln“](./media/hdinsight-hadoop-use-hive-visual-studio/toolbar.png)

4. Die **Hive-Auftragszusammenfassung** wird mit Informationen zum aktiven Auftrag angezeigt. Verwenden Sie den Link **Aktualisieren**, um die Auftragsinformationen zu aktualisieren, bis der **Auftragsstatus** zu **Abgeschlossen** wechselt.

   ![Auftragszusammenfassung, die einen abgeschlossenen Auftrag anzeigt](./media/hdinsight-hadoop-use-hive-visual-studio/jobsummary.png)

5. Verwenden Sie den Link **Auftragsausgabe** , um die Ausgabe dieses Auftrags anzuzeigen. `[ERROR] 3` wird angezeigt – dies ist der Wert, der von dieser Abfrage zurückgegeben wird.

6. Sie können Hive-Abfragen auch ohne Erstellung eines Projekts ausführen. Erweitern Sie im **Server-Explorer** die Option **Azure** > **HDInsight**, klicken Sie dann mit der rechten Maustaste auf den HDInsight-Server, und wählen Sie anschließend **Hive-Abfrage schreiben** aus.

7. Fügen Sie im sich daraufhin öffnenden Dokument **temp.hql** die folgenden HiveQL-Anweisungen hinzu:

   ```hiveql
   set hive.execution.engine=tez;
   CREATE TABLE IF NOT EXISTS errorLogs (t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string) STORED AS ORC;
   INSERT OVERWRITE TABLE errorLogs SELECT t1, t2, t3, t4, t5, t6, t7 FROM log4jLogs WHERE t4 = '[ERROR]' AND INPUT__FILE__NAME LIKE '%.log';
   ```

    Diese Anweisungen führen die folgenden Aktionen aus:

   * `CREATE TABLE IF NOT EXISTS`: Erstellt eine Tabelle, sofern diese noch nicht vorhanden ist. Da das `EXTERNAL`-Schlüsselwort nicht verwendet wird, erstellt diese Anweisung eine interne Tabelle. Interne Tabellen werden im Hive-Data Warehouse gespeichert und von Hive verwaltet.

     > [!NOTE]
     > Anders als bei `EXTERNAL` Tabellen werden beim Löschen von internen Tabellen auch die zugrunde liegenden Daten gelöscht.

   * `STORED AS ORC`: Speichert die Daten im ORC-Format (Optimized Row Columnar). ORC ist ein stark optimiertes und effizientes Format zum Speichern von Hive-Daten.

   * `INSERT OVERWRITE ... SELECT`: Wählt die Zeilen aus der `log4jLogs` Tabelle, die `[ERROR]` enthalten, und fügt dann die Daten in die `errorLogs` Tabelle ein.

8. Wählen Sie auf der Symbolleiste **Übermitteln** aus, um den Auftrag auszuführen. Verwenden Sie **Auftragsstatus** , um zu ermitteln, ob der Auftrag erfolgreich abgeschlossen wurde.

9. Verwenden Sie den **Server-Explorer**, und erweitern Sie **Azure** > **HDInsight** > Ihren HDInsight-Cluster > **Hive-Datenbanken** > **Standard**, um zu überprüfen, ob der Auftrag eine neue Tabelle erstellt hat. Die Tabellen **errorLogs** und **log4jLogs** werden aufgelistet.

## <a id="nextsteps"></a>Nächste Schritte

Wie Sie sehen, können Sie mit den HDInsight-Tools für Visual Studio mühelos mit Hive-Abfragen in HDInsight arbeiten.

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

[hdinsight-provision]: hdinsight-hadoop-provision-linux-clusters.md
[hdinsight-submit-jobs]: hdinsight-submit-hadoop-jobs-programmatically.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-get-started]: hdinsight-hadoop-linux-tutorial-get-started.md

[powershell-here-strings]: http://technet.microsoft.com/library/ee692792.aspx

[image-hdi-hive-powershell]: ./media/hdinsight-use-hive/HDI.HIVE.PowerShell.png
[img-hdi-hive-powershell-output]: ./media/hdinsight-use-hive/HDI.Hive.PowerShell.Output.png
[image-hdi-hive-architecture]: ./media/hdinsight-use-hive/HDI.Hive.Architecture.png

