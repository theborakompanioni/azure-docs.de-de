---
title: Verwenden von Hadoop Hive auf der Abfragekonsole in HDInsight | Microsoft-Dokumentation
description: "In diesem Artikel erfahren Sie, wie Sie mit der HDInsight-Abfragekonsole Hive-Abfragen auf einem Hadoop-Cluster in HDInsight in Ihrem Browser ausführen."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 5ae074b0-f55e-472d-94a7-005b0e79f779
ms.service: hdinsight
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 01/12/2017
ms.author: larryfr
ROBOTS: NOINDEX
ms.translationtype: Human Translation
ms.sourcegitcommit: 8f987d079b8658d591994ce678f4a09239270181
ms.openlocfilehash: 3000f8db8357793c68f49b47cb5a1df16bb8df51
ms.contentlocale: de-de
ms.lasthandoff: 05/18/2017


---
# <a name="run-hive-queries-using-the-query-console"></a>Ausführen von Hive-Abfragen mithilfe der Abfragekonsole
[!INCLUDE [hive-selector](../../includes/hdinsight-selector-use-hive.md)]

In diesem Artikel erfahren Sie, wie Sie mit der HDInsight-Abfragekonsole Hive-Abfragen auf einem Hadoop-Cluster in HDInsight in Ihrem Browser ausführen.

> [!IMPORTANT]
> Die HDInsight-Abfragekonsole ist nur auf Windows-basierten HDInsight-Clustern verfügbar. Linux ist das einzige Betriebssystem, das unter HDInsight Version 3.4 oder höher verwendet wird. Weitere Informationen finden Sie unter [Welche Hadoop-Komponenten und -Versionen sind in HDInsight verfügbar?](hdinsight-component-versioning.md#hdi-version-33-nearing-retirement-date).
>
> Informationen zum Ausführen von Hive-Abfragen über einen Webbrowser für HDInsight 3.4 oder höher finden Sie unter [Ausführen von Hive-Abfragen in Ambari Hive View](hdinsight-hadoop-use-hive-ambari-view.md).

## <a id="prereq"></a>Voraussetzungen
Damit Sie die in dieser Artikel aufgeführten Schritte ausführen können, benötigen Sie Folgendes:

* Einen Windows-basierten Hadoop-Cluster in HDInsight
* Einen zeitgemäßen Webbrowser

## <a id="run"></a> Ausführen von Hive-Abfragen mithilfe der Abfragekonsole
1. Öffnen Sie einen Webbrowser, und navigieren Sie zu **https://CLUSTERNAME.azurehdinsight.net**. Dabei steht **CLUSTERNAME** für den Namen des HDInsight-Clusters. Geben Sie nach der entsprechenden Aufforderung den Benutzernamen samt Kennwort ein, den Sie beim Erstellen des Clusters eingegeben haben.
2. Wählen Sie am Seitenanfang in den Links die Option **Hive-Editor**aus. Dadurch wird ein Formular angezeigt, mit dem Sie HiveQL-Anweisungen eingeben können, die Sie auf dem HDInsight-Cluster ausführen möchten.

    ![im Hive-Editor](./media/hdinsight-hadoop-use-hive-query-console/queryconsole.png)

    Ersetzen Sie den Text `Select * from hivesampletable` durch die folgenden HiveQL-Anweisungen:

        set hive.execution.engine=tez;
        DROP TABLE log4jLogs;
        CREATE EXTERNAL TABLE log4jLogs (t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string)
        ROW FORMAT DELIMITED FIELDS TERMINATED BY ' '
        STORED AS TEXTFILE LOCATION 'wasbs:///example/data/';
        SELECT t4 AS sev, COUNT(*) AS count FROM log4jLogs WHERE t4 = '[ERROR]' AND INPUT__FILE__NAME LIKE '%.log' GROUP BY t4;

    Diese Anweisungen führen die folgenden Aktionen aus:

   * **TABELLE LÖSCHEN**: Löscht Tabelle und Datendatei, falls die Tabelle bereits vorhanden ist.
   * **CREATE EXTERNAL TABLE**: Erstellt eine neue „externe“ Tabelle in Hive. Externe Tabellen dienen nur zum Speichern der Tabellendefinition in Hive. Die Daten verbleiben am ursprünglichen Speicherort.

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
3. Klicken Sie auf **Senden**. Die **Auftragssitzung** am unteren Seitenrand sollte Details für den Auftrag anzeigen.
4. Sobald sich die Angabe im Feld **Status** in **Abgeschlossen** ändert, wählen Sie **Details anzeigen** für den Auftrag aus. Auf der Detailseite enthält die **Auftragsausgabe** die Zeichenfolge `[ERROR]    3`. Sie können die Schaltfläche **Herunterladen** unter diesem Feld verwenden, um eine Datei herunterzuladen, die die Ausgabe des Auftrags enthält.

## <a id="summary"></a>Zusammenfassung
Wie Sie sehen können, bietet die Abfragekonsole eine einfache Möglichkeit, um Hive-Abfragen auf einem HDInsight-Cluster auszuführen, den Auftragsstatus zu überwachen und die Ausgabe abzurufen.

Um weitere Informationen zu Hive mithilfe der Abfragekonsole zu erhalten, wählen Sie am oberen Rand der Abfragekonsole **Erste Schritte** aus, und verwenden Sie dann die Beispiele. In jedem Beispiel werden Sie durch die Datenanalyse mit Hive geführt, einschließlich der Erläuterungen zu HiveQL-Anweisungen, die in diesem Beispiel verwendet werden.

## <a id="nextsteps"></a>Nächste Schritte
Allgemeine Informationen zu Hive in HDInsight:

* [Verwenden von Hive mit Hadoop in HDInsight](hdinsight-use-hive.md)

Informationen zu anderen Möglichkeiten, wie Sie mit Hadoop in HDInsight arbeiten können:

* [Verwenden von Pig mit Hadoop in HDInsight](hdinsight-use-pig.md)
* [Verwenden von MapReduce mit Hadoop in HDInsight](hdinsight-use-mapreduce.md)

Wenn Sie mit Tez mit Hive verwenden, finden Sie in den folgenden Dokumenten Informationen zum Debuggen:

* [Verwenden der Tez-Benutzeroberfläche in Windows-basiertem HDInsight](hdinsight-debug-tez-ui.md)
* [Verwenden der Ambari-Tez-Ansicht in Linux-basiertem HDInsight](hdinsight-debug-ambari-tez-view.md)

[1]: ../HDInsight/hdinsight-hadoop-visual-studio-tools-get-started.md

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

[Powershell-install-configure]: /powershell/azureps-cmdlets-docs
[powershell-here-strings]: http://technet.microsoft.com/library/ee692792.aspx


[img-hdi-hive-powershell-output]: ./media/hdinsight-use-hive/HDI.Hive.PowerShell.Output.png

