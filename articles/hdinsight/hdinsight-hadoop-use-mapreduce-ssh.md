---
title: MapReduce und SSH-Verbindung mit Hadoop in HDInsight | Microsoft-Dokumentation
description: "Erfahren Sie, wie MapReduce-Aufträge mithilfe von SSH mit Hadoop in HDInsight ausgeführt werden."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 844678ba-1e1f-4fda-b9ef-34df4035d547
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 05/03/2017
ms.author: larryfr
ms.translationtype: Human Translation
ms.sourcegitcommit: 7c4d5e161c9f7af33609be53e7b82f156bb0e33f
ms.openlocfilehash: 04d5b7ea9835e3196bbe300384e9097d7c667e3f
ms.contentlocale: de-de
ms.lasthandoff: 05/04/2017


---
# <a name="use-mapreduce-with-hadoop-on-hdinsight-with-ssh"></a>Verwenden von MapReduce mit Hadoop in HDInsight über SSH

[!INCLUDE [mapreduce-selector](../../includes/hdinsight-selector-use-mapreduce.md)]

In diesem Artikel wird erläutert, wie MapReduce-Jobs aus einer Secure Shell-Verbindung (SSH) an HDInsight übermittelt werden können.

> [!NOTE]
> Wenn Sie bereits mit der Verwendung von Linux-basierten Hadoop-Servern vertraut sind, Ihnen HDInsight jedoch neu ist, finden Sie weitere Informationen unter [Tipps zu Linux-basiertem HDInsight](hdinsight-hadoop-linux-information.md).

## <a id="prereq"></a>Voraussetzungen

* Einen Linux-basierten HDInsight-Cluster (Hadoop in HDInsight)

  > [!IMPORTANT]
  > Linux ist das einzige Betriebssystem, das unter HDInsight Version 3.4 oder höher verwendet wird. Weitere Informationen finden Sie unter [HDInsight-Komponentenversionen](hdinsight-component-versioning.md#hdi-version-33-nearing-deprecation-date).

* Einen SSH-Client. Weitere Informationen finden Sie unter [Verwenden von SSH mit Linux-basiertem Hadoop in HDInsight unter Linux, Unix oder OS X](hdinsight-hadoop-linux-use-ssh-unix.md)

## <a id="ssh"></a>Verbinden mit SSH

Stellen Sie mithilfe von SSH eine Verbindung zum Cluster her. Mithilfe des folgenden Befehls wird z.B. eine Verbindung mit einem Cluster namens **myhdinsight** hergestellt:

    ssh admin@myhdinsight-ssh.azurehdinsight.net

**Wenn Sie einen Zertifikatschlüssel für die SSH-Authentifizierung** verwenden, müssen Sie möglicherweise den Speicherort des privaten Schlüssels im Clientsystem angeben. Beispiel:

    ssh -i ~/mykey.key admin@myhdinsight-ssh.azurehdinsight.net

**Wenn Sie ein Kennwort für die SSH-Authentifizierung verwenden**, müssen Sie dieses angeben, wenn Sie dazu aufgefordert werden.

Weitere Informationen zum Verwenden von SSH mit HDInsight finden Sie unter [Verwenden von SSH mit HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).

## <a id="hadoop"></a>Verwenden von Hadoop-Befehlen

1. Nachdem die Verbindung mit dem HDInsight-Cluster hergestellt wurde, verwenden Sie den folgenden Befehl, um einen MapReduce-Auftrag zu starten:

    ```bash
    yarn jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-mapreduce-examples.jar wordcount /example/data/gutenberg/davinci.txt /example/data/WordCountOutput
    ```

    Dieser Befehl ruft die `wordcount`-Klasse auf, die sich in der Datei `hadoop-mapreduce-examples.jar` befindet. Der Befehl verwendet das Dokument `/example/data/gutenberg/davinci.txt` als Eingabe, und die Ausgabe wird in `/example/data/WordCountOutput` gespeichert.

    > [!NOTE]
    > Weitere Informationen über diesen MapReduce-Auftrag und die Beispieldaten finden Sie unter [Verwenden von MapReduce mit Hadoop in HDInsight](hdinsight-use-mapreduce.md).

2. Der Auftrag gibt während der Verarbeitung Details aus und gibt ähnliche Informationen wie den folgenden Text zurück, wenn der Auftrag abgeschlossen ist:

        File Input Format Counters
        Bytes Read=1395666
        File Output Format Counters
        Bytes Written=337623

3. Wenn der Auftrag abgeschlossen ist, verwenden Sie den folgenden Befehl zum Auflisten der Ausgabedateien:

    ```bash
    hdfs dfs -ls /example/data/WordCountOutput
    ```

    Dieser Befehl zeigt die beiden Dateien, `_SUCCESS` und `part-r-00000` an. Die Datei `part-r-00000` enthält die Ausgabe für diesen Auftrag.

    > [!NOTE]
    > Einige MapReduce-Aufträge teilen die Ergebnisse möglicherweise auf mehrere **part-r-#####** -Dateien auf. Verwenden Sie in diesem Fall das Suffix "#####", um die Reihenfolge der Dateien anzugeben.

4. Verwenden Sie den folgenden Befehl, um die Ausgabe anzuzeigen:

    ```bash
    hdfs dfs -cat /example/data/WordCountOutput/part-r-00000
    ```

    Dieser Befehl zeigt eine Liste mit in der Datei **wasbs://example/data/gutenberg/davinci.txt** enthaltenen Wörtern und die Häufigkeit ihres jeweiligen Vorkommens an. Im folgenden Text finden Sie ein Beispiel für die in der Datei enthaltenen Daten:

        wreathed        3
        wreathing       1
        wreaths         1
        wrecked         3
        wrenching       1
        wretched        6
        wriggling       1

## <a id="summary"></a>Zusammenfassung

Wie Sie sehen können, bieten Hadoop-Befehle eine einfache Möglichkeit zum Ausführen von MapReduce-Aufträgen in einem HDInsight-Cluster und dem anschließenden Anzeigen der Auftragsausgabe.

## <a id="nextsteps"></a>Nächste Schritte

Allgemeine Informationen zu MapReduce-Aufträgen in HDInsight:

* [Verwenden von MapReduce mit Hadoop in HDInsight](hdinsight-use-mapreduce.md)

Informationen zu anderen Möglichkeiten, wie Sie mit Hadoop in HDInsight arbeiten können:

* [Verwenden von Hive mit Hadoop in HDInsight](hdinsight-use-hive.md)
* [Verwenden von Pig mit Hadoop in HDInsight](hdinsight-use-pig.md)

