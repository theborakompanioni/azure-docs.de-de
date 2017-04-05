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
ms.date: 02/08/2017
ms.author: larryfr
translationtype: Human Translation
ms.sourcegitcommit: 4f2230ea0cc5b3e258a1a26a39e99433b04ffe18
ms.openlocfilehash: e696906adf604ebdb665a7a29fd1bba25095e842
ms.lasthandoff: 03/25/2017


---
# <a name="use-mapreduce-with-hadoop-on-hdinsight-with-ssh"></a>Verwenden von MapReduce mit Hadoop in HDInsight über SSH

[!INCLUDE [mapreduce-selector](../../includes/hdinsight-selector-use-mapreduce.md)]

In diesem Artikel erfahren Sie, wie Sie mit Secure Shell (SSH) eine Verbindung mit einem Hadoop-Cluster in HDInsight herstellen und anschließend MapReduce-Aufträge mithilfe von Hadoop-Befehlen übermitteln.

> [!NOTE]
> Wenn Sie bereits mit der Verwendung von Linux-basierten Hadoop-Servern vertraut sind, Ihnen HDInsight jedoch neu ist, finden Sie weitere Informationen unter [Tipps zu Linux-basiertem HDInsight](hdinsight-hadoop-linux-information.md).

## <a id="prereq"></a>Voraussetzungen

Damit Sie die in diesem Artikel aufgeführten Schritte ausführen können, benötigen Sie Folgendes:

* Einen Linux-basierten HDInsight-Cluster (Hadoop in HDInsight)

  > [!IMPORTANT]
  > Linux ist das einzige Betriebssystem, das unter HDInsight Version 3.4 oder höher verwendet wird. Weitere Informationen finden Sie unter [Ende des Lebenszyklus von HDInsight unter Windows](hdinsight-component-versioning.md#hdi-version-32-and-33-nearing-deprecation-date).

* Einen SSH-Client. Windows-Benutzer müssen einen Client herunterladen, z. B [PuTTY](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html).

## <a id="ssh"></a>Verbinden mit SSH

Stellen Sie mithilfe des SSH-Befehls eine Verbindung zum vollqualifizierten Domänennamen (FQDN) des HDInsight-Clusters her. Der FQDN entspricht dem Namen, den Sie dem Cluster zugewiesen haben, gefolgt von **.azurehdinsight.net**. Mithilfe des folgenden Befehls wird z. B. eine Verbindung mit einem Cluster namens **myhdinsight** hergestellt:

    ssh admin@myhdinsight-ssh.azurehdinsight.net

**Wenn Sie beim Erstellen des HDInsight-Clusters einen Zertifikatschlüssel für die SSH-Authentifizierung** angegeben haben, müssen Sie möglicherweise den Speicherort des privaten Schlüssels im Clientsystem angeben. Beispiel:

    ssh -i ~/mykey.key admin@myhdinsight-ssh.azurehdinsight.net

**Wenn Sie beim Erstellen des HDInsight-Clusters ein Kennwort für die SSH-Authentifizierung** angegeben haben, müssen Sie nach der entsprechenden Aufforderung das Kennwort angeben.

Weitere Informationen zum Verwenden von SSH mit HDInsight finden Sie unter [Verwenden von SSH mit HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).

## <a id="hadoop"></a>Verwenden von Hadoop-Befehlen

1. Nachdem die Verbindung mit dem HDInsight-Cluster hergestellt wurde, verwenden Sie den folgenden **Hadoop** -Befehl, um einen MapReduce-Auftrag zu starten:
   
    ```
    yarn jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-mapreduce-examples.jar wordcount /example/data/gutenberg/davinci.txt /example/data/WordCountOutput
    ```

    Dadurch wird die Klasse **wordcount** gestartet, die in der Datei **hadoop-mapreduce-examples.jar** enthalten ist. Sie verwendet als Eingabe das Dokument **/example/data/gutenberg/davinci.txt**, und die Ausgabe wird im Verzeichnis **/example/data/WordCountOutput** gespeichert.
   
    > [!NOTE]
    > Weitere Informationen über diesen MapReduce-Auftrag und die Beispieldaten finden Sie unter [Verwenden von MapReduce mit Hadoop in HDInsight](hdinsight-use-mapreduce.md).

2. Der Auftrag gibt während der Verarbeitung Details aus und gibt ähnliche Informationen wie die folgenden zurück, wenn der Auftrag abgeschlossen ist:
   
        File Input Format Counters
        Bytes Read=1395666
        File Output Format Counters
        Bytes Written=337623

3. Sobald der Auftrag abgeschlossen ist, verwenden Sie den folgenden Befehl zum Auflisten der Ausgabedateien, die unter **wasbs://example/data/WordCountOutput** gespeichert sind:
   
    ```
    hdfs dfs -ls /example/data/WordCountOutput
    ```
   
    Daraufhin sollten zwei Dateien angezeigt werden: **_SUCCESS** und **part-r-00000**. Die Datei **part-r-00000** enthält die Ausgabe für diesen Auftrag.
   
    > [!NOTE]
    > Einige MapReduce-Aufträge teilen die Ergebnisse möglicherweise auf mehrere **part-r-#####** -Dateien auf. Verwenden Sie in diesem Fall das Suffix "#####", um die Reihenfolge der Dateien anzugeben.

4. Verwenden Sie den folgenden Befehl, um die Ausgabe anzuzeigen:
   
    ```
    hdfs dfs -cat /example/data/WordCountOutput/part-r-00000
    ```
   
    Dadurch werden eine Liste mit in der Datei **wasbs://example/data/gutenberg/davinci.txt** enthaltenen Wörtern und die Häufigkeit ihres jeweiligen Auftretens angezeigt. Im Folgenden finden Sie ein Beispiel für die in der Datei enthaltenen Daten:
   
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


