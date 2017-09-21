---
title: Was ist Spark-Streaming in Azure HDInsight? | Microsoft-Dokumentation
description: Verwendung von Spark-Streaming-Anwendungen auf HDInsight-Spark-Clustern.
services: hdinsight
documentationcenter: 
tags: azure-portal
author: maxluk
manager: jhubbard
editor: cgronlun
ms.assetid: 
ms.service: hdinsight
ms.custom: hdinsightactive
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/08/2017
ms.author: nitinme
ms.translationtype: HT
ms.sourcegitcommit: 2c6cf0eff812b12ad852e1434e7adf42c5eb7422
ms.openlocfilehash: 7608951ffb79d945ce3271792e7cf1a0d0b254b3
ms.contentlocale: de-de
ms.lasthandoff: 09/13/2017

---
# <a name="overview-of-spark-streaming"></a>Übersicht zu Spark-Streaming

Spark-Streaming bietet eine Datenstromverarbeitung auf HDInsight-Spark-Clustern mit der Garantie, dass alle Eingabeereignisse genau einmal verarbeitet werden, selbst dann, wenn ein Knotenfehler auftritt. Ein Spark-Stream ist ein Auftrag mit langer Ausführungsdauer, der Eingabedaten aus einer Vielzahl von Quellen empfängt, einschließlich Azure Event Hubs, einem Azure IoT Hub, Kafka, Flume, Twitter, ZeroMQ, RAW-TCP-Sockets oder aus der Überwachung von HDFS-Dateisystemen. Im Gegensatz zu einem ausschließlich ereignisgesteuerten Prozess unterteilt ein Spark-Stream Eingabedaten in Zeitfensterbatches, z.B. einen 2-Sekunden-Slice, und transformiert dann jeden Datenbatch mithilfe von Zuordnungs-, Reduzierungs-, Verknüpfungs- und Extrahierungsvorgängen. Er schreibt die transformierten Daten dann in Dateisysteme, Datenbanken, Dashboards und die Konsole.

![Datenstromverarbeitung mit HDInsight und Spark-Streaming](./media/hdinsight-spark-streaming-overview/hdinsight-spark-streaming.png)

Spark-Streaming-Anwendungen müssen einige Sekundenbruchteile warten, bis jeder *Mikrobatch* von Ereignissen eingesammelt ist, bevor dieser Batch zur Verarbeitung gesendet wird. Im Gegensatz dazu verarbeitet eine ereignisgesteuerte Anwendung jedes Ereignis sofort. Die Spark-Streaming-Latenzzeit liegt in der Regel unter wenigen Sekunden. Die Vorteile des Mikrobatchansatzes sind eine effizientere Datenverarbeitung und einfachere Aggregationsberechnungen.

## <a name="introducing-the-dstream"></a>Einführung in DStream

Spark-Streaming stellt einen kontinuierlichen Strom von eingehenden Daten mithilfe eines als DStream bezeichneten *diskretisierten Datenstroms* dar. Ein DStream kann aus Eingabequellen wie Event Hubs oder Kafka oder durch Anwenden von Transformationen auf einen anderen DStream erstellt werden.

Ein DStream stellt eine Abstraktionsebene über den Ereignisrohdaten dar. 

Beginnen wir mit einem einzelnen Ereignis, z.B. einer Temperaturablesung von einem verbundenen Thermostat. Wenn dieses Ereignis Ihre Spark-Streaming-Anwendung erreicht, wird es auf zuverlässige Weise gespeichert, d.h. auf mehreren Knoten repliziert. Diese Fehlertoleranz gewährleistet, dass der Ausfall eines einzelnen Knotens nicht zum Verlust des Ereignisses führt. Der Spark-Kern verwendet eine Datenstruktur, die Daten über mehrere Knoten im Cluster verteilt, wobei jeder Knoten in der Regel zur Leistungsoptimierung seinen eigenen internen Datenarbeitsspeicher verwaltet. Diese Datenstruktur wird als *Resilient Distributed Dataset* (RDD) bezeichnet.

Jedes RDD stellt Ereignisse dar, die in einem als Batchintervall bezeichneten benutzerdefinierten Zeitraum gesammelt wurden. Wenn ein Batchintervall verstreicht, wird ein neues RDD erzeugt, das alle Daten dieses Intervalls enthält. Der kontinuierliche Satz von RDDs wird in einem DStream gesammelt. Wenn das Batchintervall z.B. eine Sekunde lang ist, gibt Ihr DStream jede Sekunde einen Batch aus, der ein RDD mit allen während dieser Sekunde erfassten Daten enthält. Bei der DStream-Verarbeitung tritt das Temperaturereignis in einem dieser Batches auf. Eine Spark-Streaming-Anwendung verarbeitet die Batches, die die Ereignisse enthalten, und führt die endgültigen Aktionen an den Daten durch, die in den einzelnen RDDs gespeichert werden.

![Beispiel-DStream mit Temperaturereignissen ](./media/hdinsight-spark-streaming-overview/hdinsight-spark-streaming-example.png)

## <a name="structure-of-a-spark-streaming-application"></a>Struktur einer Spark-Streaming-Anwendung
Eine Spark-Streaming-Anwendung ist eine Anwendung mit langer Ausführungsdauer, die Daten von Erfassungsquellen empfängt, Transformationen zum Verarbeiten der Daten anwendet und die Daten dann an ein oder mehrere Ziele sendet. Die Struktur einer Spark-Streaming-Anwendung besteht aus zwei Teilen. Zuerst definieren Sie, woher die Daten kommen, welcher Verarbeitung sie unterzogen werden sollen, und wohin die Ergebnisse gesendet werden sollen. Zweitens führen Sie die Anwendung auf unbestimmte Zeit aus und warten auf ein Stoppsignal.

Hier ist z.B. eine einfache Anwendung, die eine Textzeile über einen TCP-Socket empfängt und zählt, wie oft die einzelnen Wörter vorkommen.

### <a name="define-the-application"></a>Definieren der Anwendung

Die Definition der Anwendungslogik besteht aus vier Schritten:

1. Erstellen eines StreamingContext.
2. Erstellen eines DStream aus dem StreamingContext.
3. Anwenden von Transformationen auf den DStream.
4. Ausgeben der Ergebnisse.

Diese Definition ist statisch, und Daten werden erst dann verarbeitet, wenn Sie die Anwendung ausführen.

#### <a name="create-a-streamingcontext"></a>Erstellen eines StreamingContext
Erstellen Sie einen StreamingContext aus dem SparkContext, der auf Ihren Cluster verweist. Beim Erstellen eines StreamingContext geben Sie z.B. die Größe des Batches in Sekunden an:

    val ssc = new StreamingContext(spark, Seconds(1))

#### <a name="create-a-dstream"></a>Erstellen eines DStream
Mithilfe der StreamingContext-Instanz, die Sie erstellt haben, erstellen Sie einen Eingabe-DStream für Ihre Eingabequelle. In diesem Fall überwachen wir das Vorkommen neuer Dateien im Standardspeicher, der dem HDInsight-Cluster angefügt ist.

    val lines = ssc.textFileStream("/uploads/2017/01/")

#### <a name="apply-transformations"></a>Anwenden von Transformationen
Sie implementieren die Verarbeitung durch Anwenden von Transformationen auf den DStream. Die Anwendung empfängt jeweils eine Textzeile aus der Datei, unterteilt jede Zeile in Wörter und verwendet dann ein MapReduce-Muster, um zu zählen, wie oft jedes Wort vorkommt.

    val words = lines.flatMap(_.split(" "))
    val pairs = words.map(word => (word, 1))
    val wordCounts = pairs.reduceByKey(_ + _)

#### <a name="output-results"></a>Ausgeben von Ergebnissen
Senden Sie die Transformationsergebnisse durch Anwenden von Ausgabevorgängen an die Zielsysteme. In diesem Fall wird das Ergebnis jedes Berechnungsdurchlaufs an der Konsole ausgegeben.

    wordCounts.print()

### <a name="run-the-application"></a>Ausführen der Anwendung
Starten Sie die Streaminganwendung, und führen Sie sie aus, bis ein Beendigungssignal empfangen wird.

    ssc.start()            
    ssc.awaitTermination()

Weitere Informationen zur Spark-Stream-API sowie den unterstützten Ereignisquellen, Transformationen und Ausgabevorgängen finden Sie im [Programmierhandbuch zu Spark-Streaming](https://people.apache.org/~pwendell/spark-releases/latest/streaming-programming-guide.html).

Die folgende Beispielanwendung ist eigenständig, sodass Sie sie in einem [Jupyter Notebook](hdinsight-apache-spark-jupyter-notebook-kernels.md) ausführen können. In diesem Beispiel erstellen wir eine simulierte Datenquelle in der Klasse DummySource, die alle fünf Sekunden den Wert eines Zählers und die aktuelle Zeit in Millisekunden ausgibt. Wir erstellen ein neues StreamingContext-Objekt mit einem Batchintervall von 30 Sekunden. Jedes Mal, wenn ein Batch erstellt wird, überprüft es das erzeugte RDD, konvertiert es in einen Spark-DataFrame und erstellt eine temporäre Tabelle über dem DataFrame.

    class DummySource extends org.apache.spark.streaming.receiver.Receiver[(Int, Long)](org.apache.spark.storage.StorageLevel.MEMORY_AND_DISK_2) {

        /** Start the thread that simulates receiving data */
        def onStart() {
            new Thread("Dummy Source") { override def run() { receive() } }.start()
        }

        def onStop() {  }

        /** Periodically generate a random number from 0 to 9, and the timestamp */
        private def receive() {
            var counter = 0  
            while(!isStopped()) {
            store(Iterator((counter, System.currentTimeMillis)))
            counter += 1
            Thread.sleep(5000)
            }
        }
    }

    // A batch is created every 30 seconds
    val ssc = new org.apache.spark.streaming.StreamingContext(spark.sparkContext, org.apache.spark.streaming.Seconds(30))

    // Set the active SQLContext so that we can access it statically within the foreachRDD
    org.apache.spark.sql.SQLContext.setActive(spark.sqlContext)

    // Create the stream
    val stream = ssc.receiverStream(new DummySource())

    // Process RDDs in the batch
    stream.foreachRDD { rdd =>

        // Access the SQLContext and create a table called demo_numbers we can query
        val _sqlContext = org.apache.spark.sql.SQLContext.getOrCreate(rdd.sparkContext)
        _sqlContext.createDataFrame(rdd).toDF("value", "time")
            .registerTempTable("demo_numbers")
    } 

    // Start the stream processing
    ssc.start()

Wir können dann den DataFrame in regelmäßigen Abständen abfragen, um den im Batch vorhandenen aktuellen Satz von Werten anzuzeigen. In diesem Fall verwenden wir folgende SQL-Abfrage:

    %%sql
    SELECT * FROM demo_numbers

Daraus ergibt sich folgende Ausgabe:

| value | in |
| --- | --- |
|10 | 1497314465256 |
|11 | 1497314470272 |
|12 | 1497314475289 |
|13 | 1497314480310 |
|14 | 1497314485327 |
|15 | 1497314490346 |

Wir erwarten, dass sechs Werte angezeigt werden, da die DummySource alle 5 Sekunden einen Wert erstellt, und wir alle 30 Sekunden einen Batch ausgeben.

## <a name="sliding-windows"></a>Gleitende Fenster
Wenn Sie über einen bestimmten Zeitraum Aggregationsberechnungen für Ihren DStream durchführen möchten, um z.B. die Durchschnittstemperatur der letzten 2 Sekunden abzurufen, können Sie die in Spark-Streaming verfügbaren „Gleitendes Fenster“-Vorgänge verwenden. Ein gleitendes Fenster wird definiert durch seine Dauer (Fensterlänge) und das Intervall, in dem der Inhalt des Fensters ausgewertet wird (Gleitintervall).

Diese gleitenden Fenster können sich überschneiden, sodass Sie z.B. ein Fenster mit einer Länge von 2 Sekunden definieren können, das einmal pro Sekunde gleitet. Das bedeutet, dass das Fenster jedes Mal, wenn Sie eine Aggregationsberechnung ausführen, Daten aus der letzten Sekunde des vorherigen Fensters sowie neue Daten aus der nächsten Sekunde enthält.

![Beispiel für das erste Fenster mit Temperaturereignissen](./media/hdinsight-spark-streaming-overview/hdinsight-spark-streaming-window-01.png)

![Beispiel für ein Fenster mit Temperaturereignissen nach dem Gleiten](./media/hdinsight-spark-streaming-overview/hdinsight-spark-streaming-window-02.png)

Im folgenden Beispiel aktualisieren wir den Code, der die DummySource verwendet, um die Batches in einem Fenster mit einer Dauer von einer Minute und einem Gleiten in einer Minute zu sammeln.

    // A batch is created every 30 seconds
    val ssc = new org.apache.spark.streaming.StreamingContext(spark.sparkContext, org.apache.spark.streaming.Seconds(30))

    // Set the active SQLContext so that we can access it statically within the foreachRDD
    org.apache.spark.sql.SQLContext.setActive(spark.sqlContext)

    // Create the stream
    val stream = ssc.receiverStream(new DummySource())

    // Process batches in 1 minute windows
    stream.window(org.apache.spark.streaming.Minutes(1)).foreachRDD { rdd =>

        // Access the SQLContext and create a table called demo_numbers we can query
        val _sqlContext = org.apache.spark.sql.SQLContext.getOrCreate(rdd.sparkContext)
        _sqlContext.createDataFrame(rdd).toDF("value", "time")
        .registerTempTable("demo_numbers")
    } 

    // Start the stream processing
    ssc.start()

Nach der ersten Minute ergibt dies 12 Einträge – sechs Einträge aus jedem der beiden im Fenster gesammelten Batches.

| value | in |
| --- | --- |
| 1 | 1497316294139 |
| 2 | 1497316299158
| 3 | 1497316304178
| 4 | 1497316309204
| 5 | 1497316314224
| 6 | 1497316319243
| 7 | 1497316324260
| 8 | 1497316329278
| 9 | 1497316334293
| 10 | 1497316339314
| 11 | 1497316344339
| 12 | 1497316349361

Zu den in der Spark-Streaming-API verfügbaren „Gleitendes Fenster“-Funktionen zählen „window“, countByWindow, reduceByWindow und countByValueAndWindow. Weitere Informationen zu diesen Funktionen finden Sie unter [Transformations on DStreams](https://people.apache.org/~pwendell/spark-releases/latest/streaming-programming-guide.html#transformations-on-dstreams) (DStream-Transformationen).

## <a name="checkpointing"></a>Setzen von Prüfpunkten
Um Resilienz und Fehlertoleranz zu gewährleisten, stellt Spark-Streaming mit Prüfpunkten sicher, dass die Datenstromverarbeitung auch bei Knotenfehlern kontinuierlich durchgeführt werden kann. In HDInsight erstellt Spark Prüfpunkte in permanentem Speicher (Azure Storage oder Data Lake Store). Diese Prüfpunkte speichern die Metadaten der Streaminganwendung, z.B. die Konfiguration, die von der Anwendung definierten Vorgänge und alle Batches, die in die Warteschlange gestellt, aber noch nicht verarbeitet wurden. In einigen Fällen beinhalten die Prüfpunkte auch das Speichern der Daten in den RDDs, um die Zeit zum Neuerstellen des Zustands der Daten aus dem, was in den von Spark verwalteten RDDs vorhanden ist, zu verkürzen.

## <a name="deploying-spark-streaming-applications"></a>Bereitstellen von Spark-Streaming-Anwendungen
Sie erstellen die Spark-Streaming-Anwendung in der Regel lokal, und stellen Sie dann unter HDInsight für Spark bereit, indem Sie die JAR-Datei, die Ihre Anwendung enthält, in den Standardspeicher kopieren, der Ihrem HDInsight-Cluster angefügt ist. Dann können Sie Ihre Anwendung mithilfe der LIVY-REST-APIs starten, die in Ihrem Cluster verfügbar sind. Dies ist ein POST-Vorgang, wobei der POST-Textkörper ein JSON-Dokument enthält, das Folgendes angibt: den Pfad zu Ihrer JAR-Datei, den Namen der Klasse, deren Hauptmethode die Streaminganwendung definiert und ausführt, und optional die Ressourcenanforderungen des Auftrags (z.B. die Anzahl der Executors, Speicher und Kerne) sowie alle Konfigurationseinstellungen, die Ihr Anwendungscode erfordert.

![Beispiel für ein Fenster mit Temperaturereignissen nach dem Gleiten](./media/hdinsight-spark-streaming-overview/hdinsight-spark-streaming-livy.png)

Der Status aller Anwendungen kann auch mit einer GET-Anforderung an einem LIVY-Endpunkt überprüft werden. Schließlich können Sie eine ausgeführte Anwendung durch Senden einer DELETE-Anforderung an den LIVE-Endpunkt beenden. Einzelheiten zur LIVY-API finden Sie unter [Übermitteln von Remoteaufträgen an einen HDInsight Spark-Cluster mithilfe der Apache Spark-REST-API](hdinsight-apache-spark-livy-rest-interface.md)

## <a name="next-steps"></a>Nächste Schritte

* [Erstellen von Linux-basierten Clustern in HDInsight mithilfe des Azure-Portals](hdinsight-hadoop-create-linux-clusters-portal.md)
* [Spark-Streaming-Programmierhandbuch](https://people.apache.org/~pwendell/spark-releases/latest/streaming-programming-guide.html)
* [Übermitteln von Remoteaufträgen an einen HDInsight Spark-Cluster mithilfe der Apache Spark-REST-API](hdinsight-apache-spark-livy-rest-interface.md)
