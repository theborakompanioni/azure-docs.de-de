---
title: Verwenden von Hadoop Pig in HDInsight | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie Pig mit Hadoop in HDInsight verwenden.
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: acfeb52b-4b81-4a7d-af77-3e9908407404
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 05/26/2017
ms.author: larryfr
ms.translationtype: Human Translation
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.openlocfilehash: 9c19dc502acd1af4c3193e033d3caefe895624be
ms.contentlocale: de-de
ms.lasthandoff: 07/08/2017

---
# <a name="use-pig-with-hadoop-on-hdinsight"></a>Verwenden von Pig mit Hadoop in HDInsight

Informationen zum Verwenden von [Apache Pig](http://pig.apache.org/) mit HDInsight.

Pig ist eine Plattform zum Erstellen von Programmen für Hadoop mithilfe der prozeduralen Sprache *Pig Latin*. Pig ist eine Alternative zu Java für das Erstellen von *MapReduce* -Lösungen, und es ist in Azure HDInsight enthalten. In der folgenden Tabelle finden Sie verschiedene Möglichkeiten der Verwendung von Pig mit HDInsight:

| **Verwenden Sie dies**, wenn Sie Folgendes wünschen: | ... eine **interaktive** Shell | ...**Batchverarbeitung** | ...mit diesem **Clusterbetriebssystem** | ...von diesem **Clientbetriebssystem** |
|:--- |:---:|:---:|:--- |:--- |
| [SSH](hdinsight-hadoop-use-pig-ssh.md) |✔ |✔ |Linux |Linux, Unix, Mac OS X oder Windows |
| [REST-API](hdinsight-hadoop-use-pig-curl.md) |&nbsp; |✔ |Linux oder Windows |Linux, Unix, Mac OS X oder Windows |
| [.NET SDK für Hadoop](hdinsight-hadoop-use-pig-dotnet-sdk.md) |&nbsp; |✔ |Linux oder Windows |Windows (vorläufig) |
| [Windows PowerShell](hdinsight-hadoop-use-pig-powershell.md) |&nbsp; |✔ |Linux oder Windows |Windows |
| [Remotedesktop](hdinsight-hadoop-use-pig-remote-desktop.md) (HDInsight 3.2 und 3.3) |✔ |✔ |Windows |Windows |

> [!IMPORTANT]
> Linux ist das einzige Betriebssystem, das unter HDInsight Version 3.4 oder höher verwendet wird. Weitere Informationen finden Sie unter [Welche Hadoop-Komponenten und -Versionen sind in HDInsight verfügbar?](hdinsight-component-versioning.md#hdinsight-windows-retirement).

## <a id="why"></a>Warum Pig?

Eine der Herausforderungen der Verarbeitung von Daten mithilfe von MapReduce in Hadoop ist die Implementierung der Verarbeitungslogik mit nur einer Karte und einer Reduce-Funktion. Für die komplexe Verarbeitung muss die diese häufig in mehrere MapReduce-Vorgänge unterteilt werden, die miteinander verkettet sind, um das gewünschte Ergebnis zu erzielen.

Pig ermöglicht Ihnen, die Verarbeitung als eine Reihe von Transformationen zu definieren, die die Daten durchlaufen, um die gewünschte Ausgabe zu erzeugen.

Mit der Pig Latin-Sprache können Sie den Datenfluss von Rohdaten durch eine oder mehrere Transformationen beschreiben, um die gewünschte Ausgabe zu erzeugen. Pig Latin-Programme folgen diesem allgemeinen Muster:

* **Laden**: Die zu verändernden Daten werden aus dem Dateisystem gelesen

* **Transformieren**: Manipulation der Daten

* **Ablage oder Speicherung**: Ausgabe der Daten auf dem Bildschirm oder Speicherung zur Weiterverarbeitung

### <a name="user-defined-functions"></a>Benutzerdefinierte Funktionen

Pig Latin unterstützt auch benutzerdefinierte Funktionen (UDF), wodurch Sie externe Komponenten aufrufen können, die in Pig Latin schwer zu modellierende Logik implementieren.

Weitere Informationen zu Pig Latin finden Sie unter [Pig Latin-Referenzhandbuch 1](http://pig.apache.org/docs/r0.7.0/piglatin_ref1.html) und [Pig Latin-Referenzhandbuch 2](http://pig.apache.org/docs/r0.7.0/piglatin_ref2.html).

Ein Beispiel für benutzerdefinierte Funktionen mit Pig finden Sie in den folgenden Dokumenten:

* [Verwenden von DataFu mit Pig in HDInsight](hdinsight-hadoop-use-pig-datafu-udf.md) – DataFu ist eine Sammlung nützlicher UDFs, die von Apache verwaltet werden.
* [Verwenden von Python mit Pig und Hive in HDInsight](hdinsight-python.md)
* [Verwenden von C# mit Hive und Pig in HDInsight](hdinsight-hadoop-hive-pig-udf-dotnet-csharp.md)

## <a id="data"></a>Beispieldaten

HDInsight enthält verschiedene Beispieldatasets, die in den Verzeichnissen `/example/data` und `/HdiSamples` gespeichert sind. Diese Verzeichnisse befinden sich im Standardspeicher für den Cluster. Im Pig-Beispiel in diesem Dokument wird die Datei *log4j* aus `/example/data/sample.log` verwendet.

Jedes Protokoll innerhalb der Datei besteht aus einer Reihe von Feldern, unter denen sich ein Feld namens `[LOG LEVEL]` befindet, das die Art und den Schweregrad des jeweiligen Fehlers anzeigt, beispielsweise:

    2012-02-03 20:26:41 SampleClass3 [ERROR] verbose detail for id 1527353937

Die Protokollebene im vorherigen Beispiel ist ERROR.

> [!NOTE]
> Sie können auch eine log4j-Datei generieren, indem Sie das [Apache Log4j](http://en.wikipedia.org/wiki/Log4j) -Protokollierungstool verwenden und die Datei dann in Ihren Blob hochladen. Anweisungen hierzu finden Sie unter [Hochladen von Daten in HDInsight](hdinsight-upload-data.md) . Weitere Informationen zur Verwendung von Blobs im Azure-Speicher mit HDInsight finden Sie unter [Verwenden des Azure Blobspeichers mit HDInsight](hdinsight-hadoop-use-blob-storage.md).

## <a id="job"></a>Beispielauftrag

Mit dem folgenden Pig Latin-Auftrag wird die Datei `sample.log` aus dem Standardspeicher für den HDInsight-Cluster geladen. Er führt dann eine Reihe von Transformationen durch, die als Ergebnis anzeigen, wie oft jede Protokollebene in den Eingabedaten aufgetreten ist. Die Ergebnisse werden an STDOUT geschrieben.

    LOGS = LOAD 'wasb:///example/data/sample.log';
    LEVELS = foreach LOGS generate REGEX_EXTRACT($0, '(TRACE|DEBUG|INFO|WARN|ERROR|FATAL)', 1)  as LOGLEVEL;
    FILTEREDLEVELS = FILTER LEVELS by LOGLEVEL is not null;
    GROUPEDLEVELS = GROUP FILTEREDLEVELS by LOGLEVEL;
    FREQUENCIES = foreach GROUPEDLEVELS generate group as LOGLEVEL, COUNT(FILTEREDLEVELS.LOGLEVEL) as COUNT;
    RESULT = order FREQUENCIES by COUNT desc;
    DUMP RESULT;

In der folgenden Abbildung ist zusammengefasst, wie sich die einzelnen Transformationen auf die Daten auswirken.

![Grafische Darstellung der Transformationen][image-hdi-pig-data-transformation]

## <a id="run"></a>Pig Latin-Auftrag ausführen

HDInsight kann Pig Latin-Aufträge mithilfe verschiedener Methoden ausführen. Die folgende Tabelle hilft Ihnen bei der Entscheidung, welche Methode für Sie geeignet ist. Folgen Sie anschließend dem Link für eine exemplarische Vorgehensweise.

| **Verwenden Sie dies**, wenn Sie Folgendes wünschen: | ... eine **interaktive** Shell | ...**Batchverarbeitung** | ...mit diesem **Clusterbetriebssystem** | ... von diesem **Client** |
|:--- |:---:|:---:|:--- |:--- |
| [SSH](hdinsight-hadoop-use-pig-ssh.md) |✔ |✔ |Linux |Linux, Unix, Mac OS X oder Windows |
| [Curl](hdinsight-hadoop-use-pig-curl.md) |&nbsp; |✔ |Linux oder Windows |Linux, Unix, Mac OS X oder Windows |
| [.NET SDK für Hadoop](hdinsight-hadoop-use-pig-dotnet-sdk.md) |&nbsp; |✔ |Linux oder Windows |Windows (vorläufig) |
| [Windows PowerShell](hdinsight-hadoop-use-pig-powershell.md) |&nbsp; |✔ |Linux oder Windows |Windows |
| [Remotedesktop](hdinsight-hadoop-use-pig-remote-desktop.md) (HDInsight 3.2 und 3.3) |✔ |✔ |Windows |Windows |

> [!IMPORTANT]
> Linux ist das einzige Betriebssystem, das unter HDInsight Version 3.4 oder höher verwendet wird. Weitere Informationen finden Sie unter [Welche Hadoop-Komponenten und -Versionen sind in HDInsight verfügbar?](hdinsight-component-versioning.md#hdinsight-windows-retirement).

## <a name="pig-and-sql-server-integration-services"></a>Pig und SQL Server Integration Services

Sie können mit SQL Server Integration Services (SSIS) einen Pig-Auftrag ausführen. Das Azure Feature Pack für SSIS bietet die folgenden Komponenten, die mit Pig-Aufträgen in HDInsight funktionieren.

* [Pig-Aufgabe in Azure HDInsight][pigtask]

* [Verbindungs-Manager für Azure-Abonnements][connectionmanager]

[Hier][ssispack] erfahren Sie mehr zum Azure Feature Pack für SSIS.

## <a id="nextsteps"></a>Nächste Schritte
Nachdem Sie erfahren haben, wie Sie Pig mit HDInsight verwenden, können Sie mithilfe der nachfolgenden Links andere Möglichkeiten für die Arbeit mit Azure HDInsight untersuchen.

* [Hochladen von Daten in HDInsight][hdinsight-upload-data]
* [Verwenden von Hive mit HDInsight][hdinsight-use-hive]
* [Verwenden von Sqoop mit HDInsight](hdinsight-use-sqoop.md)
* [Verwenden von Oozie mit HDInsight](hdinsight-use-oozie.md)
* [Verwenden von MapReduce-Aufträgen mit HDInsight][hdinsight-use-mapreduce]

[apachepig-home]: http://pig.apache.org/
[putty]: http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html
[curl]: http://curl.haxx.se/
[pigtask]: http://msdn.microsoft.com/library/mt146781(v=sql.120).aspx
[connectionmanager]: http://msdn.microsoft.com/library/mt146773(v=sql.120).aspx
[ssispack]: http://msdn.microsoft.com/library/mt146770(v=sql.120).aspx


[hdinsight-upload-data]: hdinsight-upload-data.md

[hdinsight-admin-powershell]: hdinsight-administer-use-powershell.md

[hdinsight-use-hive]: hdinsight-use-hive.md
[hdinsight-use-mapreduce]: hdinsight-use-mapreduce.md

[hdinsight-provision]: hdinsight-hadoop-provision-linux-clusters.md
[hdinsight-submit-jobs]: hdinsight-submit-hadoop-jobs-programmatically.md#mapreduce-sdk

[Powershell-install-configure]: /powershell/azureps-cmdlets-docs

[powershell-start]: http://technet.microsoft.com/library/hh847889.aspx


[image-hdi-pig-data-transformation]: ./media/hdinsight-use-pig/HDI.DataTransformation.gif

