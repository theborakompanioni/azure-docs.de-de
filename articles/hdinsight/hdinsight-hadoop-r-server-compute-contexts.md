<properties
   pageTitle="Rechenkontextoptionen für R Server in HDInsight Premium | Azure"
   description="Lernen Sie die verschiedenen Rechenkontextoptionen kennen, die Benutzern mit R Server in HDInsight Premium zur Verfügung stehen."
   services="HDInsight"
   documentationCenter=""
   authors="jeffstokes72"
   manager="paulettem"
   editor="cgronlun"
/>

<tags
   ms.service="HDInsight"
   ms.devlang="R"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="data-services"
   ms.date="03/28/2016"
   ms.author="jeffstok"
/>

# Rechenkontextoptionen für R Server in HDInsight Premium

R Server auf HDI Spark stellt die neuesten Funktionen für die R-basierte Analyse mithilfe von in HDFS gespeicherten Daten bereit, die in einem Container in Ihrem lokalen [Azure-Blobspeicherkonto](../storage/storage-introduction.md "Azure-Blobspeicher") oder auf dem lokalen Linux-Dateisystem enthalten sind. Da R Server auf der Open Source-Sprache R basiert, profitieren Ihre Anwendungen von mehr als 8.000 Open Source-Paketen für R und von den Routinen im Big Data-Analysepaket [ScaleR](http://www.revolutionanalytics.com/revolution-r-enterprise-scaler "Revolution Analytics ScaleR") von Microsoft, das Bestandteil von R Server ist. Der Edgeknoten der Premium-Cluster stellt eine praktische Möglichkeit für die Verbindung mit dem Cluster und die Ausführung Ihrer R-Skripts bereit. Mit einem Edgeknoten können Sie die parallelisierten verteilten Funktionen von ScaleR entweder auf den Kernen des Edgeknotenservers oder auf den Knoten des Clusters ausführen. Hierfür müssen Sie die ScaleR-Rechenkontexte Hadoop MapReduce oder Spark verwenden.

## Rechenkontexte für einen Edgeknoten

Im Allgemeinen wird ein R-Skript, das in R Server auf dem Edgeknoten ausgeführt wird, innerhalb des R-Interpreters auf diesem Knoten ausgeführt. Ausgenommen hiervon sind die Schritte, die eine ScaleR-Funktion aufrufen. Die ScaleR-Aufrufe werden in einer Rechenumgebung ausgeführt, die dadurch bestimmt wird, wie Sie den ScaleR-Rechenkontext festlegen. Mögliche Werte für den Rechenkontext beim Ausführen Ihres R-Skripts auf einem Edgeknoten sind: lokal sequenziell („local“), lokal parallel („localpar“), Map Reduce und Spark. Hierbei gilt Folgendes:

| Rechenkontext | Festlegung | Ausführungskontext |
|------------------|---------------------------------|---------------------------------------------------------------------------------------|
| Lokal sequenziell | rxSetComputeContext(‘local’) | Sequenzielle (nicht parallelisierte) Ausführung auf dem Edgeknotenserver |
| Lokal parallel | rxSetComputeContext(‘localpar’) | Parallelisierte Ausführung über die Kerne des Edgeknotenservers hinweg |
| Spark | RxSpark() | Parallelisierte verteilte Ausführung mit Spark über die Knoten des HDI-Clusters hinweg |
| Map Reduce | RxHadoopMR() | Parallelisierte verteilte Ausführung mit Map Reduce über die Knoten des HDI-Clusters hinweg |


Davon ausgehend, dass Sie aus Gründen der Leistung eine parallelisierte Ausführung bevorzugen, stehen drei Optionen zur Verfügung. Für welche Option Sie sich entscheiden, richtet sich nach der Art Ihrer Analysen sowie nach der Größe und dem Speicherort Ihrer Daten.

## Entscheidung für einen Rechenkontext

Es ist keine Formel vorhanden, mit deren Hilfe Sie problemlos entscheiden können, welcher Rechenkontext verwendet werden sollte. Stattdessen gibt es einige grundlegende Richtlinien, die Sie bei Ihrer Entscheidung unterstützen können. Wenn eine optimale Auswahl erforderlich ist, sollten Sie einen Vergleichstest durchführen. Zu diesen Richtlinien gehören die folgenden:

1.	Das lokale Linux-Dateisystem ist schneller als HDFS.
2.	Wiederholte Analysen können schneller ausgeführt werden, wenn die Daten lokal und in XDF vorliegen. 
3.	Führen Sie für kleine Datenmengen ein Streaming von einer Textdatenquelle durch, oder konvertieren Sie die Daten andernfalls vor der Analyse nach XDF. 
4.	Der Mehraufwand zum Kopieren/Streamen der zu analysierenden Daten auf die Edgeknoten ist für sehr große Datenmengen nicht vertretbar. 
5.	Spark ist für Analysen in Hadoop schneller als Map Reduce, solange die Datenmengen nicht zu groß sind, um in den verteilten Arbeitsspeicher aufgenommen zu werden.

Neben diesen Richtlinien sollten bei der Auswahl des Rechenkontexts die folgenden allgemeinen Regeln berücksichtigt werden:

### Lokal parallel

- Wenn die zu analysierende Datenmenge klein ist und keine wiederholte Analyse erfordert, sollten Sie die Daten direkt in die Analyseroutine streamen und „localpar“ verwenden. 
- Wenn die zu analysierende Datenmenge klein ist und eine wiederholte Analyse erfordert oder wenn eine Datenmenge mittleren Umfangs analysiert werden muss, dann kopieren Sie die Daten in das lokale Dateisystem, importieren Sie nach XDF, und führen Sie die Analyse mit „localpar“ durch. 

### Hadoop Spark

- Wenn eine große Datenmenge analysiert werden muss, importieren Sie die Daten nach XDF in HDFS (sofern der Speicherplatz kein Problem darstellt), und führen Sie die Analyse mit „Spark“ durch. 

### Hadoop Map Reduce

- Wenn eine sehr große Datenmenge analysiert werden muss und Spark keine ausreichende Leistung bietet, versuchen Sie eine Analyse mit „Map Reduce“.

## Integrierte Hilfe zu rxSetComputeContext

Weitere Informationen und Beispiele zu ScaleR-Rechenkontexten finden Sie in der integrierten Hilfe in R zur rxSetComputeContext-Methode:

    > ?rxSetComputeContext 

Alternativ finden Sie weitere Informationen im „ScaleR Distributed Computing Guide“ (Handbuch zum verteilten Computing mit ScaleR) in der [R Server MSDN](https://msdn.microsoft.com/library/mt674634.aspx "R Server auf MSDN")-Bibliothek.


## Nächste Schritte

In diesem Artikel haben Sie erfahren, wie Sie einen neuen HDInsight-Cluster mit Verwendung von R Server erstellen, und Sie haben die Grundlagen der Verwendung der R-Konsole aus einer SSH-Sitzung kennengelernt. Unter den folgenden Links erhalten Sie Informationen zu weiteren Möglichkeiten für die Arbeit mit R Server in HDInsight.

- [Übersicht über R Server in Hadoop](hdinsight-hadoop-r-server-overview.md)
- [Erste Schritte mit R Server in Hadoop](hdinsight-hadoop-r-server-get-started.md)
- [Hinzufügen von RStudio Server zu HDInsight Premium](hdinsight-hadoop-r-server-install-r-studio.md)
- [Azure Storage-Optionen für R Server in HDInsight Premium](hdinsight-hadoop-r-server-storage.md)

<!---HONumber=AcomDC_0330_2016-->