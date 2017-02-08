---
title: MapReduce und Remotedesktop mit Hadoop in HDInsight | Microsoft-Dokumentation
description: "Erfahren Sie, wie Sie mithilfe des Remotedesktops eine Verbindung mit Hadoop in HDInsight herstellen und MapReduce-Aufträge ausführen."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 9d3a7b34-7def-4c2e-bb6c-52682d30dee8
ms.service: hdinsight
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 09/27/2016
ms.author: larryfr
translationtype: Human Translation
ms.sourcegitcommit: cc59d7785975e3f9acd574b516d20cd782c22dac
ms.openlocfilehash: a79f10cfb0ca882b00eb4a969fbf1d397ebdcf2f


---
# <a name="use-mapreduce-in-hadoop-on-hdinsight-with-remote-desktop"></a>Verwenden von MapReduce in Hadoop in HDInsight über den Remotedesktop
[!INCLUDE [mapreduce-selector](../../includes/hdinsight-selector-use-mapreduce.md)]

In diesem Artikel erfahren Sie, wie Sie mithilfe des Remotedesktops eine Verbindung mit einem Hadoop-Cluster in HDInsight herstellen und dann MapReduce-Aufträge mit dem Hadoop-Befehl ausführen.

## <a name="a-idprereqaprerequisites"></a><a id="prereq"></a>Voraussetzungen
Um die in diesem Artikel aufgeführten Schritte auszuführen, benötigen Sie Folgendes:

* Einen Windows-basierten HDInsight-Cluster (Hadoop in HDInsight)
* Ein Clientcomputer unter Windows 10, Windows 8 oder Windows 7

## <a name="a-idconnectaconnect-with-remote-desktop"></a><a id="connect"></a>Verbinden mit dem Remotedesktop
Aktivieren Sie Remotedesktop für den HDInsight-Cluster, und stellen Sie anschließend mithilfe der Anweisungen unter [Verbinden mit HDInsight-Clustern über RDP](hdinsight-administer-use-management-portal.md#connect-to-clusters-using-rdp)eine Verbindung her.

## <a name="a-idhadoopause-the-hadoop-command"></a><a id="hadoop"></a>Verwenden des Hadoop-Befehls
Wenn Sie mit dem Desktop des HDInsight-Clusters verbunden sind, gehen Sie wie folgt vor, um einen MapReduce-Auftrag mithilfe des Hadoop-Befehls auszuführen:

1. Starten Sie auf dem HDInsight-Desktop die **Hadoop-Befehlszeile**. Dadurch wird eine neue Eingabeaufforderung im Verzeichnis **c:\apps\dist\hadoop-&lt;Versionsnummer>** geöffnet.

   > [!NOTE]
   > Die Versionsnummer wechselt, wenn Hadoop aktualisiert wird. Die Umgebungsvariable **HADOOP_HOME** kann für die Suche nach dem Pfad verwendet werden. Mithilfe von `cd %HADOOP_HOME%` können Sie z. B. in das Hadoop-Verzeichnis wechseln, ohne die Versionsnummer von Hadoop kennen zu müssen.
   >
   >
2. Verwenden Sie den folgenden Befehl, um mit dem **Hadoop** -Befehl einen MapReduce-Beispielauftrag auszuführen.

        hadoop jar hadoop-mapreduce-examples.jar wordcount wasbs:///example/data/gutenberg/davinci.txt wasbs:///example/data/WordCountOutput

    Dadurch wird die Klasse **wordcount** gestartet, die in der Datei **hadoop-mapreduce-examples.jar** im aktuellen Verzeichnis enthalten ist. Sie verwendet als Eingabe das Dokument **wasbs://example/data/gutenberg/davinci.txt**, und die Ausgabe wird im Verzeichnis **wasbs:///example/data/WordCountOutput** gespeichert.

   > [!NOTE]
   > Weitere Informationen über diesen MapReduce-Auftrag und die Beispieldaten finden Sie unter <a href="hdinsight-use-mapreduce.md">Verwenden von MapReduce in HDInsight Hadoop</a>.
   >
   >
3. Der Auftrag gibt während der Verarbeitung Details aus und gibt ähnliche Informationen wie die folgenden zurück, wenn der Auftrag abgeschlossen ist:

        File Input Format Counters
        Bytes Read=1395666
        File Output Format Counters
        Bytes Written=337623
4. Sobald der Auftrag abgeschlossen ist, verwenden Sie den folgenden Befehl zum Auflisten der Ausgabedateien, die unter **wasbs://example/data/WordCountOutput** gespeichert sind:

        hadoop fs -ls wasbs:///example/data/WordCountOutput

    Daraufhin sollten zwei Dateien angezeigt werden: **_SUCCESS** und **part-r-00000**. Die Datei **part-r-00000** enthält die Ausgabe für diesen Auftrag.

   > [!NOTE]
   > Einige MapReduce-Aufträge teilen die Ergebnisse möglicherweise auf mehrere **part-r-#####** -Dateien auf. Verwenden Sie in diesem Fall das Suffix "#####", um die Reihenfolge der Dateien anzugeben.
   >
   >
5. Verwenden Sie den folgenden Befehl, um die Ausgabe anzuzeigen:

        hadoop fs -cat wasbs:///example/data/WordCountOutput/part-r-00000

    Dadurch wird eine Liste mit in der Datei **wasbs://example/data/gutenberg/davinci.txt** enthaltenen Wörtern zusammen mit der Häufigkeit ihres jeweiligen Auftretens angezeigt. Im Folgenden finden Sie ein Beispiel für die in der Datei enthaltenen Daten:

        wreathed        3
        wreathing       1
        wreaths         1
        wrecked         3
        wrenching       1
        wretched        6
        wriggling       1

## <a name="a-idsummaryasummary"></a><a id="summary"></a>Zusammenfassung
Wie Sie sehen können, bietet der Hadoop-Befehl eine einfache Möglichkeit zum Ausführen von MapReduce-Aufträgen auf einen HDInsight-Cluster und dem anschließenden Anzeigen der Auftragsausgabe.

## <a name="a-idnextstepsanext-steps"></a><a id="nextsteps"></a>Nächste Schritte
Allgemeine Informationen zu MapReduce-Aufträgen in HDInsight:

* [Verwenden von MapReduce mit Hadoop in HDInsight](hdinsight-use-mapreduce.md)

Informationen zu anderen Möglichkeiten, wie Sie mit Hadoop in HDInsight arbeiten können:

* [Verwenden von Hive mit Hadoop in HDInsight](hdinsight-use-hive.md)
* [Verwenden von Pig mit Hadoop in HDInsight](hdinsight-use-pig.md)



<!--HONumber=Nov16_HO3-->


