---
title: "Rechenkontextoptionen für R Server in HDInsight – Azure | Microsoft-Dokumentation"
description: "Informieren Sie sich über die verschiedenen Rechenkontextoptionen, die Benutzern mit R Server in HDInsight (Vorschau) zur Verfügung stehen."
services: HDInsight
documentationcenter: 
author: bradsev
manager: jhubbard
editor: cgronlun
ms.assetid: 0deb0b1c-4094-459b-94fc-ec9b774c1f8a
ms.service: HDInsight
ms.custom: hdinsightactive
ms.devlang: R
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 06/19/2017
ms.author: bradsev
ms.translationtype: Human Translation
ms.sourcegitcommit: 7948c99b7b60d77a927743c7869d74147634ddbf
ms.openlocfilehash: 09b65ccd77f6e0898f07c4262940e517a8f913fa
ms.contentlocale: de-de
ms.lasthandoff: 06/20/2017


---
# <a name="compute-context-options-for-r-server-on-hdinsight"></a>Rechenkontextoptionen für R Server in HDInsight

Microsoft R Server für Azure HDInsight bestimmt durch Festlegen des Computekontexts, wie Aufrufe ausgeführt werden. In diesem Artikel werden die Optionen beschrieben, mit denen festgelegt werden kann, ob und wie die Ausführung in allen Kernen des Edgeknotens oder im HDInsight-Cluster parallelisiert werden.

Der Edgeknoten eines Clusters ist ein praktischer Ort für die Verbindungsherstellung mit dem Cluster und die Ausführung Ihrer R-Skripts. Mit einem Edgeknoten haben Sie die Möglichkeit, die parallelisierten verteilten Funktionen von ScaleR in allen Kernen der Edgeknotenserver auszuführen. Außerdem können Sie sie über die Knoten des Clusters hinweg ausführen, indem Sie Hadoop MapReduce von ScaleR oder Spark-Rechenkontexte verwenden.

## <a name="microsoft-r-server-on-azure-hdinsight"></a>Microsoft R Server für Azure HDInsight
[Microsoft R Server für Azure HDInsight](hdinsight-hadoop-r-server-overview.md) bietet die neuesten Funktionen für die R-basierte Analyse. Die verwendeten Daten sind in einem HDFS-Container in Ihrem [Azure Blob Storage-Konto](../storage/storage-introduction.md "Azure Blob Storage"), einem Data Lake Store oder im lokalen Dateisystem von Linux gespeichert. Da R Server auf Open Source R basiert, stehen Ihnen bei der Erstellung R-basierter Anwendungen alle über 8000 Open Source R-Pakete zur Verfügung. Auch die Routinen in [RevoScaleR](https://msdn.microsoft.com/microsoft-r/scaler/scaler) – dem in R Server enthaltenen Big Data-Analysepaket von Microsoft – können genutzt werden.  

## <a name="compute-contexts-for-an-edge-node"></a>Computekontexte für einen Edgeknoten
Im Allgemeinen wird ein R-Skript, das in R Server auf dem Edgeknoten ausgeführt wird, im R-Interpreter auf diesem Knoten ausgeführt. Bei Schritten, in denen eine ScaleR-Funktion aufgerufen wird, ist dies nicht der Fall. Die ScaleR-Aufrufe werden in einer Computeumgebung ausgeführt, die dadurch bestimmt wird, wie Sie den ScaleR-Computekontext festlegen.  Wenn Sie Ihr R-Skript auf einem Edgeknoten ausführen, sind für den Computekontext folgende Werte möglich:

- lokal sequenziell (*„local“*)
- lokal parallel (*„localpar“*)
- Map Reduce
- Spark

Die Optionen *„local“* und *„localpar“* unterscheiden sich nur darin, wie **rxExec**-Aufrufe ausgeführt werden. Beide führen andere „rx-function“-Aufrufe auf allen verfügbaren Knoten parallel aus, es sei denn, über die ScaleR-Option **numCoresToUse** ist etwas anderes angegeben. Beispiel: `rxOptions(numCoresToUse=6)`. Optionen für die parallele Ausführung ermöglichen eine optimale Leistung.

In der folgenden Tabelle werden die verschiedenen Optionen für den Computekontext zur Ausführung von Aufrufen zusammengefasst:

| Computekontext  | Festlegung                      | Ausführungskontext                        |
| ---------------- | ------------------------------- | ---------------------------------------- |
| Lokal sequenziell | rxSetComputeContext(‘local’)    | Parallele Ausführung auf den Kernen des Edgeknotenservers außer für rxExec-Aufrufe, die seriell ausgeführt werden |
| Lokal parallel   | rxSetComputeContext(‘localpar’) | Parallele Ausführung auf den Kernen des Edgeknotenservers |
| Spark            | RxSpark()                       | Parallelisierte verteilte Ausführung mit Spark über die Knoten des HDI-Clusters hinweg  |
| Map Reduce       | RxHadoopMR()                    | Parallelisierte verteilte Ausführung mit Map Reduce über die Knoten des HDI-Clusters hinweg  |

## <a name="guidelines-for-deciding-on-a-compute-context"></a>Entscheidungsrichtlinien für die Wahl eines Computekontexts

Welche der drei Optionen für die parallelisierte Ausführung ausgewählt werden, hängt von der Art der Analysen sowie vom Umfang und vom Speicherort der Daten ab. Der zu verwendende Kontext lässt sich nicht anhand einer einfachen Formel ermitteln. Es gibt jedoch einige grundlegende Prinzipien, von denen Sie sich bei Ihrer Entscheidung leiten lassen können (oder mit denen sich die Optionen zumindest eingrenzen lassen), bevor Sie einen Benchmarktest ausführen. Zu diesen Richtlinien gehören die folgenden:

- Das lokale Linux-Dateisystem ist schneller als HDFS.
- Wiederholte Analysen können schneller ausgeführt werden, wenn die Daten lokal und in XDF vorliegen.
- Kleine Datenmengen sollten aus einer Textdatenquelle gestreamt werden. Bei größeren Datenmengen empfiehlt es sich, die Daten vor der Analyse in XDF zu konvertieren.
- Bei sehr großen Datenmengen ist der Mehraufwand durch das Kopieren oder Streamen der zu analysierenden Daten an den Edgeknoten zu groß.
- Für Analysen in Hadoop ist Spark schneller als MapReduce.

Neben diesen Richtlinien sollten bei der Auswahl des Rechenkontexts die allgemeinen Regeln in den folgenden Abschnitten berücksichtigt werden.

### <a name="local"></a>Lokal
* Wenn die zu analysierende Datenmenge klein und keine wiederholte Analyse erforderlich ist, sollten Sie die Daten direkt in die Analyseroutine streamen und *„local“* oder *„localpar“* verwenden.
* Wenn die zu analysierende Datenmenge klein oder mittelgroß und eine wiederholte Analyse erforderlich ist, kopieren Sie die Daten in das lokale Dateisystem, importieren Sie sie in XDF, und führen Sie die Analyse mit *„local“* oder *„localpar“* durch.

### <a name="hadoop-spark"></a>Hadoop Spark
* Wenn eine große Datenmenge analysiert werden muss, importieren Sie die Daten mit **RxHiveData** oder **RxParquetData** in einen Spark DataFrame, oder in XDF in HDFS (sofern der Speicherplatz kein Problem darstellt), und führen Sie die Analyse mit dem Spark-Computekontext durch.

### <a name="hadoop-map-reduce"></a>Hadoop Map Reduce
* Verwenden Sie den MapReduce-Computekontext nur, wenn bei einem Spark-Computekontext ein unüberwindliches Problem auftritt, da MapReduce meist langsamer ist.  

## <a name="inline-help-on-rxsetcomputecontext"></a>Integrierte Hilfe zu rxSetComputeContext
Weitere Informationen und Beispiele zu ScaleR-Computekontexten finden Sie in der integrierten Hilfe von R unter der rxSetComputeContext-Methode. Beispiel:

    > ?rxSetComputeContext

Informationen finden Sie auch im „[ScaleR Distributed Computing Guide](https://msdn.microsoft.com/microsoft-r/scaler-distributed-computing)“ (Handbuch zum verteilten Computing mit ScaleR) in der [MSDN-Bibliothek zu R](https://msdn.microsoft.com/library/mt674634.aspx "R Server on MSDN").

## <a name="next-steps"></a>Nächste Schritte
In diesem Artikel haben Sie die Optionen kennengelernt, mit denen festgelegt werden kann, ob und wie die Ausführung in allen Kernen des Edgeknotens oder im HDInsight-Cluster parallelisiert werden. Weitere Informationen zur Verwendung von R Server mit HDInsight-Clustern finden Sie unter den folgenden Themen:

* [Übersicht über R Server in HDInsight](hdinsight-hadoop-r-server-overview.md)
* [Erste Schritte mit R Server für Hadoop](hdinsight-hadoop-r-server-get-started.md)
* [Hinzufügen von RStudio Server zu HDInsight (falls nicht bereits während der Clustererstellung hinzugefügt)](hdinsight-hadoop-r-server-install-r-studio.md)
* [Azure Storage-Optionen für R Server in HDInsight](hdinsight-hadoop-r-server-storage.md)


