---
title: "Rechenkontextoptionen für R Server in HDInsight | Microsoft Docs"
description: "Informieren Sie sich über die verschiedenen Rechenkontextoptionen, die Benutzern mit R Server in HDInsight (Vorschau) zur Verfügung stehen."
services: HDInsight
documentationcenter: 
author: jeffstokes72
manager: jhubbard
editor: cgronlun
ms.assetid: 0deb0b1c-4094-459b-94fc-ec9b774c1f8a
ms.service: HDInsight
ms.devlang: R
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 11/15/2016
ms.author: jeffstok
translationtype: Human Translation
ms.sourcegitcommit: 3743a1c9539f6e0077448aff4470cb8f5363ef2f
ms.openlocfilehash: ac1e2c269d59d9e3262c385450a4912de0c9debe


---
# <a name="compute-context-options-for-r-server-on-hdinsight"></a>Rechenkontextoptionen für R Server in HDInsight
Microsoft R Server für Azure HDInsight bietet die neuesten Funktionen für die R-basierte Analyse. Die verwendeten Daten sind in HDFS in einem Container in Ihrem [Azure-Blobspeicherkonto](../storage/storage-introduction.md "Azure-Blobspeicherkonto storage") oder im lokalen Dateisystem von Linux gespeichert. Da R Server auf Open Source R basiert, stehen Ihnen bei der Erstellung R-basierter Anwendungen alle über 8000 Open Source R-Pakete zur Verfügung. Auch die Routinen in [ScaleR](http://www.revolutionanalytics.com/revolution-r-enterprise-scaler "Revolution Analytics ScaleR")– dem in R Server enthaltenen Big Data-Analysepaket von Microsoft – können genutzt werden.  

Der Edgeknoten eines Clusters ist ein praktischer Ort für die Verbindungsherstellung mit dem Cluster und die Ausführung Ihrer R-Skripts. Mit einem Edgeknoten haben Sie die Möglichkeit, die parallelisierten verteilten Funktionen von ScaleR über die Kerne der Edgeknotenserver hinweg auszuführen. Außerdem können Sie sie über die Knoten des Clusters hinweg ausführen, indem Sie Hadoop MapReduce von ScaleR oder Spark-Berechnungskontexte verwenden.

## <a name="compute-contexts-for-an-edge-node"></a>Computekontexte für einen Edgeknoten
Im Allgemeinen wird ein R-Skript, das in R Server auf dem Edgeknoten ausgeführt wird, im R-Interpreter auf diesem Knoten ausgeführt. Bei Schritten, in denen eine ScaleR-Funktion aufgerufen wird, ist dies nicht der Fall. Die ScaleR-Aufrufe werden in einer Computeumgebung ausgeführt, die dadurch bestimmt wird, wie Sie den ScaleR-Computekontext festlegen.  Wenn Sie Ihr R-Skript auf einem Edgeknoten ausführen, sind für den Computekontext folgende Werte möglich: lokal sequenziell („local“), lokal parallel („localpar“), Map Reduce und Spark.

Die Optionen „local“ und „localpar“ unterscheiden sich nur darin, wie rxExec-Aufrufe ausgeführt werden. Beide führen andere „rx-function“-Aufrufe auf allen verfügbaren Knoten parallel aus, es sei denn, über die ScaleR-Option „numCoresToUse“ ist etwas anderes angegeben. Beispiel: rxOptions(numCoresToUse=6). Im folgenden werden die verschiedenen Optionen für den Computekontext zusammengefasst.

| Computekontext | Festlegung | Ausführungskontext |
| --- | --- | --- |
| Lokal sequenziell |rxSetComputeContext(‘local’) |Parallele Ausführung auf den Kernen des Edgeknotenservers außer für rxExec-Aufrufe, die seriell ausgeführt werden |
| Lokal parallel |rxSetComputeContext(‘localpar’) |Parallele Ausführung auf den Kernen des Edgeknotenservers |
| Spark |RxSpark() |Parallelisierte verteilte Ausführung mit Spark über die Knoten des HDI-Clusters hinweg  |
| Map Reduce |RxHadoopMR() |Parallelisierte verteilte Ausführung mit Map Reduce über die Knoten des HDI-Clusters hinweg  |

Falls Sie aus Leistungsgründen eine parallele Ausführung bevorzugen, stehen drei Optionen zur Verfügung. Für welche Option Sie sich entscheiden, richtet sich nach der Art Ihrer Analysen sowie nach der Größe und dem Speicherort Ihrer Daten.

## <a name="guidelines-for-deciding-on-a-compute-context"></a>Entscheidungsrichtlinien für die Wahl eines Computekontexts
Der zu verwendende Kontext lässt sich derzeit nicht anhand einer Formel ermitteln. Es gibt jedoch einige grundlegende Prinzipien, von denen Sie sich bei Ihrer Entscheidung leiten lassen können (oder mit denen sich die Optionen zumindest eingrenzen lassen), bevor Sie einen Benchmarktest ausführen. Zu diesen Richtlinien gehören die folgenden:

1. Das lokale Linux-Dateisystem ist schneller als HDFS.
2. Wiederholte Analysen können schneller ausgeführt werden, wenn die Daten lokal und in XDF vorliegen.
3. Kleinere Datenmengen sollten aus einer Textdatenquelle gestreamt werden. Bei größeren Datenmengen empfiehlt es sich, die Daten vor der Analyse in XDF zu konvertieren.
4. Bei sehr großen Datenmengen ist der Mehraufwand durch das Kopieren oder Streamen der zu analysierenden Daten an den Edgeknoten zu groß.
5. Für Analysen in Hadoop ist Spark schneller als MapReduce.

Neben diesen Richtlinien sollten bei der Auswahl des Rechenkontexts die folgenden allgemeinen Regeln berücksichtigt werden:

### <a name="local"></a>Lokal
* Wenn die zu analysierende Datenmenge klein und keine wiederholte Analyse erforderlich ist, sollten Sie die Daten direkt in die Analyseroutine streamen und „local“ oder „localpar“ verwenden.
* Wenn die zu analysierende Datenmenge klein oder mittelgroß und eine wiederholte Analyse erforderlich ist, kopieren Sie die Daten in das lokale Dateisystem, importieren Sie sie in XDF, und führen Sie die Analyse mit „local“ oder „localpar“ durch.

### <a name="hadoop-spark"></a>Hadoop Spark
* Wenn eine große Datenmenge analysiert werden muss, importieren Sie die Daten in HDFS in XDF (sofern der Speicherplatz kein Problem darstellt), und führen Sie die Analyse mit „Spark“ durch.

### <a name="hadoop-map-reduce"></a>Hadoop Map Reduce
* Nur verwenden, wenn bei einem Spark-Rechenkontext ein unüberwindliches Problem auftritt, da MapReduce meist langsamer ist.  

## <a name="inline-help-on-rxsetcomputecontext"></a>Integrierte Hilfe zu rxSetComputeContext
Weitere Informationen und Beispiele zu ScaleR-Computekontexten finden Sie in der integrierten Hilfe von R unter der rxSetComputeContext-Methode. Beispiel:

    > ?rxSetComputeContext

Informationen finden Sie auch im „ScaleR Distributed Computing Guide“ (Handbuch zum verteilten Computing mit ScaleR) in der [MSDN-Bibliothek zu R Server](https://msdn.microsoft.com/library/mt674634.aspx "R Server auf MSDN").

## <a name="next-steps"></a>Nächste Schritte
In diesem Artikel haben Sie gelernt, wie Sie einen neuen HDInsight-Cluster mit R Server erstellen. Außerdem haben Sie sich mit der grundlegenden Verwendung der R-Konsole im Rahmen einer SSH-Sitzung vertraut gemacht. In den folgenden Artikeln werden weitere Methoden für die Verwendung von R Server in HDInsight erläutert:

* [Übersicht über R Server in HDInsight](hdinsight-hadoop-r-server-overview.md)
* [Erste Schritte mit R Server für Hadoop](hdinsight-hadoop-r-server-get-started.md)
* [Hinzufügen von RStudio Server zu HDInsight](hdinsight-hadoop-r-server-install-r-studio.md)
* [Azure Storage-Optionen für R Server in HDInsight](hdinsight-hadoop-r-server-storage.md)




<!--HONumber=Nov16_HO3-->


