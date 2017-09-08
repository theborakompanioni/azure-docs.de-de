---
title: "Skalieren von Stream Analytics-Aufträgen zur Erhöhung des Durchsatzes | Microsoft Docs"
description: "Erfahren Sie, wie Sie Stream Analytics-Aufträge durch Konfiguration von Eingabepartitionen, Optimierung der Abfragedefinition und Einstellung von Auftrags-Streaming-Einheiten skalieren."
keywords: "Datenstreaming, Datenströme verarbeiten, Analysen optimieren"
services: stream-analytics
documentationcenter: 
author: samacha
manager: jhubbard
editor: cgronlun
ms.assetid: 7e857ddb-71dd-4537-b7ab-4524335d7b35
ms.service: stream-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 06/22/2017
ms.author: samacha
ms.translationtype: HT
ms.sourcegitcommit: 8351217a29af20a10c64feba8ccd015702ff1b4e
ms.openlocfilehash: f1e5e11e82d344508aa4375c42d509f96aaa1d00
ms.contentlocale: de-de
ms.lasthandoff: 08/29/2017

---
# <a name="scale-azure-stream-analytics-jobs-to-increase-stream-data-processing-throughput"></a>Skalieren von Azure Stream Analytics-Aufträgen zur Erhöhung des Durchsatzes bei der Streamingdatenverarbeitung
In diesem Artikel erfahren Sie, wie Sie eine Stream Analytics-Abfrage zur Steigerung des Durchsatzes für Stream Analytics-Aufträge optimieren. Sie erfahren, wie Sie Stream Analytics-Aufträge durch Konfiguration von Eingabepartitionen, Optimierung der Analyseabfragedefinition und die Berechnung und Einstellung von Auftrags-*Streamingeinheiten* (Streaming Units, SUs) skalieren. 

## <a name="what-are-the-parts-of-a-stream-analytics-job"></a>Welche Teile hat ein Stream Analytics-Auftrag?
Eine Stream Analytics-Auftragsdefinition umfasst Eingaben, Abfrage und Ausgabe. Bei Eingaben handelt es sich um Eingaben, aus denen der Auftrag den Datenstrom liest. Bei der Abfrage wird die Datenstromeingabe transformiert, und der Auftrag sendet die Auftragsergebnisse an die Ausgabe.  

Für einen Auftrag wird mindestens eine Eingabequelle für Datenstreaming benötigt. Die Datenstrom-Eingabequelle kann entweder ein Azure Event Hub oder ein Azure Blob Storage sein. Weitere Informationen finden Sie unter [Einführung in Azure Stream Analytics](stream-analytics-introduction.md) und [Erste Schritte mit Azure Stream Analytics](stream-analytics-real-time-fraud-detection.md).

## <a name="partitions-in-event-hubs-and-azure-storage"></a>Partitionen in Event Hubs und in Azure Storage
Bei der Skalierung eines Stream Analytics-Auftrags werden die Partitionen in der Eingabe oder Ausgabe genutzt. Durch die Partitionierung können Daten basierend auf einem Partitionsschlüssel in Teilmengen unterteilt werden. Ein Vorgang, bei dem Daten verbraucht werden (z.B. einem Stream Analytics-Auftrag), können unterschiedliche Partitionen parallel genutzt und in diesen geschrieben werden. Dadurch erhöht sich der Durchsatz. Bei der Arbeit mit Stream Analytics können Sie die Partitionierung in Event Hubs und in Blob Storage nutzen. 

Weitere Informationen zu den Partitionen finden Sie in den folgenden Artikeln:

* [Event Hubs-Features im Überblick](../event-hubs/event-hubs-features.md#partitions)
* [Datenpartitionierung](https://docs.microsoft.com/azure/architecture/best-practices/data-partitioning#partitioning-azure-blob-storage)


## <a name="streaming-units-sus"></a>Streamingeinheiten (SUs)
Streamingeinheiten (Streaming Units, SUs) sind eine abstrakte Repräsentation der Ressourcen und Computeleistung, die zum Ausführen eines Azure Stream Analytics-Auftrags benötigt werden. SUs bieten anhand eines kombinierten Maßes aus CPU, Arbeitsspeicher und Schreib- und Leseraten eine Möglichkeit zur Beschreibung der relativen Ereignisverarbeitungskapazität. Jede SU entspricht etwa einem Durchsatz von 1 MB/s. 

Die benötigte SU-Anzahl für einen bestimmten Auftrag hängt von der Partitionskonfiguration für die Eingaben und der für den Auftrag definierten Abfrage ab. Sie können die für Ihr Kontingent maximal festgelegte Anzahl von SUs für einen Auftrag auswählen. Jedes Azure-Abonnement verfügt standardmäßig über ein Kontingent von höchstens 50 SUs für alle Analyseaufträge in einer bestimmten Region. Wenn Sie die SUs für Ihre Abonnements über dieses Kontingent hinaus erhöhen möchten, wenden Sie sich an den [Microsoft-Support](http://support.microsoft.com). Die gültigen Werte für SUs pro Auftrag sind 1, 3, 6 und danach Werte in Schritten von 6.

## <a name="embarrassingly-parallel-jobs"></a>Hochgradig parallele Aufträge
Ein *hochgradig paralleler* Auftrag stellt das am stärksten skalierbare Szenario dar, das in Azure Stream Analytics zur Verfügung steht. Er verbindet eine Partition der Eingabe mit einer Instanz der Abfrage und einer Partition der Ausgabe. Für eine solche Parallelität gelten folgende Anforderungen:

1. Wenn Ihre Abfragelogik davon abhängig ist, dass derselbe Schlüssel durch dieselbe Abfrageinstanz verarbeitet wird, müssen Sie sicherstellen, dass die Ereignisse in derselben Partition Ihrer Eingabe aufgenommen werden. Bei Event Hubs bedeutet dies, dass für die Ereignisdaten der Wert **PartitionKey** festgelegt sein muss. Alternativ können Sie partitionierte Absender verwenden. Bei Blob Storage bedeutet dies, dass die Ereignisse an denselben Partitionsordner gesendet werden. Wenn es für Ihre Abfragelogik nicht erforderlich ist, dass derselbe Schlüssel von derselben Abfrageinstanz verarbeitet wird, können Sie diese Anforderung ignorieren. Ein Beispiel für diese Logik wäre eine einfache Auswahl-, Projekt- oder Filterabfrage.  

2. Nachdem die Daten auf der Eingabeseite angeordnet wurden, müssen Sie sicherstellen, dass die Abfrage partitioniert wird. Dazu müssen Sie in allen Schritten **Partition By** verwenden. Es sind mehrere Schritte zulässig, aber sie müssen alle durch denselben Schlüssel partitioniert werden. Derzeit muss der Partitionierungsschlüssel auf **PartitionId** festgelegt werden, damit der Auftrag vollständige Parallelität aufweist.  

3. Derzeit unterstützen nur Event Hubs und Blob Storage partitionierte Ausgaben. Für die Event Hub-Ausgabe muss der Partitionsschlüssel für **PartitionId** konfiguriert sein. Für die Blob Storage-Ausgabe müssen keine Einstellungen vorgenommen werden.  

4. Die Anzahl von Eingabepartitionen muss mit der Anzahl von Ausgabepartitionen identisch sein. Bei der Blob Storage-Ausgabe werden derzeit keine Partitionen unterstützt. Dies ist jedoch kein Problem, da diese das Partitionierungsschema der vorgeschalteten Abfrage erbt. Im Folgenden werden Beispiele für Partitionswerte vorgestellt, die einen vollständig parallelen Auftrag ermöglichen:  

   * 8 Event Hub-Eingabepartitionen und 8 Event Hub-Ausgabepartitionen
   * 8 Event Hub-Eingabepartitionen und Blob Storage-Ausgabe  
   * 8 Blob Storage-Eingabepartitionen und Blob Storage-Ausgabe  
   * 8 Blob Storage-Eingabepartitionen und 8 Event Hub-Ausgabepartitionen  

In den folgenden Abschnitten werden einige Beispielszenarien für hochgradige Parallelität behandelt.

### <a name="simple-query"></a>Einfache Abfrage

* Eingabe: Event Hub mit 8 Partitionen
* Ausgabe: Event Hub mit 8 Partitionen

Abfrage:

    SELECT TollBoothId
    FROM Input1 Partition By PartitionId
    WHERE TollBoothId > 100

Diese Abfrage stellt einen einfachen Filter dar. Daher ist keine Partitionierung der Eingabe erforderlich, die an den Event Hub gesendet wird. Beachten Sie, dass die Abfrage **Partition By PartitionId** enthält, sodass sie die zweite zuvor genannte Anforderung erfüllt. Für die Ausgabe muss die Event Hub-Ausgabe im Auftrag so konfiguriert werden, dass der Partitionsschlüssel auf **PartitionId** festgelegt ist. Eine letzte Prüfung besteht darin, sicherzustellen, dass die Anzahl der Eingabepartitionen mit der Anzahl der Ausgabepartitionen identisch ist.

### <a name="query-with-a-grouping-key"></a>Abfrage mit einem Gruppierungsschlüssel

* Eingabe: Event Hub mit 8 Partitionen
* Ausgabe: Blob Storage

Abfrage:

    SELECT COUNT(*) AS Count, TollBoothId
    FROM Input1 Partition By PartitionId
    GROUP BY TumblingWindow(minute, 3), TollBoothId, PartitionId

Diese Abfrage enthält einen Gruppierungsschlüssel. Deshalb muss der Schlüssel von derselben Abfrageinstanz verarbeitet werden, d.h., dass Ereignisse auf partitionierter Weise an den Event Hub gesendet werden müssen. Doch welcher Schlüssel sollte verwendet werden? **PartitionId** ist ein Konzept mit Auftragslogik. Der für uns tatsächlich relevante Schlüssel ist **TollBoothId**. Daher muss der Wert **PartitionKey** der Ereignisdaten auf **TollBoothId** festgelegt sein. Hierfür legen wir in der Abfrage **Partition By** auf **PartitionId** fest. Da es sich bei der Ausgabe um Blob Storage handelt, muss laut der vierten Anforderung kein Wert für den Partitionsschlüssel konfiguriert werden.

### <a name="multi-step-query-with-a-grouping-key"></a>Mehrstufige Abfrage mit einem Gruppierungsschlüssel
* Eingabe: Event Hub mit 8 Partitionen
* Ausgabe: Event Hub-Instanz mit 8 Partitionen

Abfrage:

    WITH Step1 AS (
    SELECT COUNT(*) AS Count, TollBoothId, PartitionId
    FROM Input1 Partition By PartitionId
    GROUP BY TumblingWindow(minute, 3), TollBoothId, PartitionId
    )

    SELECT SUM(Count) AS Count, TollBoothId
    FROM Step1 Partition By PartitionId
    GROUP BY TumblingWindow(minute, 3), TollBoothId, PartitionId

Diese Abfrage enthält einen Gruppierungsschlüssel. Hier muss der Schlüssel von derselben Abfrageinstanz verarbeitet werden. Wir können dieselbe Strategie wie im vorherigen Beispiel verwenden. In diesem Fall umfasst die Abfrage mehrere Schritte. Ist für jeden Schritt **Partition By PartitionId** festgelegt? Ja, damit die Abfrage die dritte Anforderung erfüllt. Für die Ausgabe müssen wir den Partitionsschlüssel auf **PartitionId** festlegen, wie bereits erwähnt wurde. Zudem können wir erkennen, dass sie dieselbe Anzahl von Partitionen wie die Eingabe enthält.

## <a name="example-scenarios-that-are-not-embarrassingly-parallel"></a>Beispielszenarien *ohne* hochgradige Parallelität

Im vorherigen Abschnitt haben wir einige Szenarien mit hochgradiger Parallelität vorgestellt. In diesem Abschnitt werden Szenarien erläutert, in denen nicht alle Anforderungen hinsichtlich hochgradiger Parallelität erfüllt werden. 

### <a name="mismatched-partition-count"></a>Ungleiche Anzahl von Partitionen
* Eingabe: Event Hub mit 8 Partitionen
* Ausgabe: Event Hub mit 32 Partitionen

In diesem Fall spielt es keine Rolle, um welche Abfrage es sich handelt. Wenn die Anzahl der Eingabepartitionen nicht der Anzahl der Ausgabepartitionen entspricht, weist die Topologie keine hochgradige Parallelität auf.

### <a name="not-using-event-hubs-or-blob-storage-as-output"></a>Keine Verwendung von Event Hubs oder Blob Storage als Ausgabe
* Eingabe: Event Hub mit 8 Partitionen
* Ausgabe: Power BI

Bei der Power BI-Ausgabe wird derzeit keine Partitionierung unterstützt. Daher besteht in diesem Szenario keine hochgradige Parallelität.

### <a name="multi-step-query-with-different-partition-by-values"></a>Mehrstufige Abfrage mit unterschiedlichen Werten für „Partition By“
* Eingabe: Event Hub mit 8 Partitionen
* Ausgabe: Event Hub mit 8 Partitionen

Abfrage:

    WITH Step1 AS (
    SELECT COUNT(*) AS Count, TollBoothId, PartitionId
    FROM Input1 Partition By PartitionId
    GROUP BY TumblingWindow(minute, 3), TollBoothId, PartitionId
    )

    SELECT SUM(Count) AS Count, TollBoothId
    FROM Step1 Partition By TollBoothId
    GROUP BY TumblingWindow(minute, 3), TollBoothId

Wie Sie sehen, wird im zweiten Schritt **TollBoothId** als Partitionierungsschlüssel verwendet. Dieser Schritt entspricht nicht dem ersten Schritt, und deshalb muss eine Umschichtung durchgeführt werden. 

In den vorherigen Beispielen werden einige Stream Analytics-Aufträge gezeigt, die einer hochgradig parallelen Topologie entsprechen (oder nicht entsprechen). Liegt eine Entsprechung mit dieser Topologie vor, können sie maximal skaliert werden. Für Aufträge, für die keines dieser Profile geeignet ist, werden bei zukünftigen Updates Leitfäden bezüglich Skalierungen zur Verfügung gestellt. Verwenden Sie bis dahin die allgemeinen Hinweise in den folgenden Abschnitten.

## <a name="calculate-the-maximum-streaming-units-of-a-job"></a>Berechnen der maximal möglichen Streaming-Einheiten für einen Auftrag
Die Gesamtzahl der von einem Stream Analytics-Auftrag verwendbaren Streaming-Einheiten hängt von der Anzahl an Schritten in der für den Auftrag definierten Abfrage und der Anzahl an Partitionen für die einzelnen Schritte ab.

### <a name="steps-in-a-query"></a>Schritte einer Abfrage
Eine Abfrage kann einen oder mehrere Schritte umfassen. Jeder Schritt besteht aus einer Unterabfrage, die mit dem Schlüsselwort **WITH** definiert wird. Die Abfrage, die sich außerhalb des Schlüsselworts **WITH** befindet (nur eine Abfrage), wird ebenfalls als Schritt gezählt, beispielsweise die **SELECT**-Anweisung in der folgenden Abfrage:

    WITH Step1 AS (
        SELECT COUNT(*) AS Count, TollBoothId
        FROM Input1 Partition By PartitionId
        GROUP BY TumblingWindow(minute, 3), TollBoothId, PartitionId
    )

    SELECT SUM(Count) AS Count, TollBoothId
    FROM Step1
    GROUP BY TumblingWindow(minute,3), TollBoothId

Die Abfrage umfasst zwei Schritte.

> [!NOTE]
> Diese Abfrage wird später im vorliegenden Artikel ausführlicher erläutert.
>  

### <a name="partition-a-step"></a>Partitionieren eines Schritts
Für die Partitionierung eines Schrittes gelten die folgenden Voraussetzungen:

* Die Eingabequelle muss partitioniert sein. 
* Die **SELECT** -Anweisung der Abfrage muss aus einer partitionierten Eingabequelle lesen.
* Die Abfrage innerhalb des Schritts muss das Schlüsselwort **Partition By** aufweisen.

Wenn eine Abfrage partitioniert ist, werden die Eingabeereignisse verarbeitet und in separaten Partitionsgruppen aggregiert, und für jede einzelne Gruppe werden Ausgabeereignisse generiert. Wenn Sie ein kombiniertes Aggregat bevorzugen, müssen Sie einen zweiten, nicht partitionierten Schritt zum Aggregieren erstellen.

### <a name="calculate-the-max-streaming-units-for-a-job"></a>Berechnen der maximal möglichen Streaming-Einheiten für einen Auftrag
Alle nicht partitionierten Schritte zusammen können auf bis zu sechs Streamingeinheiten (SUs) für einen Stream Analytics-Auftrag zentral hochskaliert werden. Zum Hinzufügen von SUs muss ein Schritt partitioniert werden. Jede Partition kann sechs SUs aufweisen.

<table border="1">
<tr><th>Abfrage</th><th>Max. Anzahl von SUs für den Auftrag</th></td>

<tr><td>
<ul>
<li>Die Abfrage umfasst einen Schritt.</li>
<li>Der Schritt ist nicht partitioniert.</li>
</ul>
</td>
<td>6</td></tr>

<tr><td>
<ul>
<li>Der Eingabedatenstrom ist in 3 partitioniert.</li>
<li>Die Abfrage umfasst einen Schritt.</li>
<li>Der Schritt ist partitioniert.</li>
</ul>
</td>
<td>18</td></tr>

<tr><td>
<ul>
<li>Die Abfrage umfasst zwei Schritte.</li>
<li>Keiner der Schritte ist partitioniert.</li>
</ul>
</td>
<td>6</td></tr>

<tr><td>
<ul>
<li>Der Eingabedatenstrom ist in 3 partitioniert.</li>
<li>Die Abfrage umfasst zwei Schritte. Der Eingabeschritt ist partitioniert, nicht jedoch der zweite.</li>
<li>Die <strong>SELECT</strong>-Anweisung liest aus der partitionierten Eingabe.</li>
</ul>
</td>
<td>24 (18 für partitionierte Schritte + 6 für nicht partitionierte Schritte)</td></tr>
</table>

### <a name="examples-of-scaling"></a>Beispiele für eine Skalierung

Mit der folgenden Abfrage wird die Anzahl der Fahrzeuge berechnet, die in einem Zeitraum von drei Minuten eine Mautstation mit drei Mautstellen passieren. Diese Abfrage kann auf bis zu sechs SUs skaliert werden.

    SELECT COUNT(*) AS Count, TollBoothId
    FROM Input1
    GROUP BY TumblingWindow(minute, 3), TollBoothId, PartitionId

Um weitere SUs für die Abfrage zu verwenden, müssen sowohl der Eingabedatenstrom als auch die Abfrage partitioniert werden. Da die Datenstrompartition auf „3“ festgelegt ist, kann die folgende geänderte Abfrage auf bis zu 18 SUs skaliert werden:

    SELECT COUNT(*) AS Count, TollBoothId
    FROM Input1 Partition By PartitionId
    GROUP BY TumblingWindow(minute, 3), TollBoothId, PartitionId

Wenn eine Abfrage partitioniert ist, werden die Eingabeereignisse verarbeitet und in separaten Partitionsgruppen aggregiert. Zudem werden für die einzelnen Gruppen Ausgabeereignisse generiert. Die Partitionierung kann zu unerwarteten Ergebnissen führen, wenn das Feld **GROUP BY** nicht den Partitionsschlüssel im Eingabedatenstrom enthält. Beispielsweise enthält das Feld **TollBoothId** in der vorherigen Abfrage nicht den Partitionsschlüssel von **Input1**. Das Ergebnis: Die Daten aus der Mautstation 1 können auf mehrere Partitionen verteilt werden.

Alle Partitionen von **Input1** werden separat von Stream Analytics verarbeitet. Folglich werden im selben rollierenden Fenster mehrere Einträge von der Anzahl der Autos für dieselbe Mautstation erstellt. Falls der Eingabepartitionsschlüssel nicht geändert werden kann, kann dieses Problem durch Hinzufügen eines nicht partitionierten Schritts behoben werden, wie im folgenden Beispiel gezeigt wird:

    WITH Step1 AS (
        SELECT COUNT(*) AS Count, TollBoothId
        FROM Input1 Partition By PartitionId
        GROUP BY TumblingWindow(minute, 3), TollBoothId, PartitionId
    )

    SELECT SUM(Count) AS Count, TollBoothId
    FROM Step1
    GROUP BY TumblingWindow(minute, 3), TollBoothId

Diese Abfrage kann auf bis zu 24 SUs skaliert werden.

> [!NOTE]
> Wenn Sie zwei Datenströme verknüpfen, stellen Sie sicher, dass die Datenströme anhand des Partitionsschlüssels der Spalte partitioniert werden, in der Sie die Verknüpfungen vornehmen. Stellen Sie außerdem sicher, dass in beiden Datenströmen dieselbe Anzahl von Partitionen vorliegt.
> 
> 

## <a name="configure-stream-analytics-streaming-units"></a>Konfigurieren von Streamingeinheiten für Stream Analytics-Aufträge

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com)an.
2. Suchen Sie in der Liste der Ressourcen nach dem zu skalierenden Stream Analytics-Auftrag, und öffnen Sie ihn anschließend.
3. Klicken Sie auf dem Blatt „Auftrag“ unter **Konfigurieren** auf **Skalieren**.

    ![Konfiguration von Stream Analytics-Aufträgen im Azure-Portal][img.stream.analytics.preview.portal.settings.scale]

4. Verwenden Sie den Schieberegler, um die SUs für den Auftrag festzulegen. Beachten Sie, dass Sie auf die jeweiligen SU-Einstellungen beschränkt sind.


## <a name="monitor-job-performance"></a>Überwachen der Auftragsleistung
Über das Azure-Portal können Sie den Durchsatz eines Auftrags nachverfolgen:

![Überwachen von Aufträgen in Azure Stream Analytics][img.stream.analytics.monitor.job]

Berechnen Sie den erwarteten Durchsatz der Workload. Für den Fall, dass der Durchsatz kleiner als erwartet ist, optimieren Sie die Eingabepartition und die Abfrage, und fügen Sie dem Auftrag zusätzliche SUs hinzu.


## <a name="visualize-stream-analytics-throughput-at-scale-the-raspberry-pi-scenario"></a>Visualisieren des skalierten Stream Analytics-Durchsatzes – das Raspberry Pi-Szenario
Um die Skalierung von Stream Analytics-Aufträgen zu veranschaulichen, haben wir basierend auf der Eingabe eines Raspberry Pi-Geräts ein Experiment durchgeführt. Dieses Experiment führt vor Augen, welche Auswirkungen sich für den Durchsatz mehrerer Streamingeinheiten und Partitionen ergeben.

In diesem Szenario sendet das Gerät Sensordaten (Clients) an einen Event Hub. Stream Analytics verarbeitet die Daten und sendet eine Warnung oder Statistiken als Ausgabe an einen anderen Event Hub. 

Der Client sendet Sensordaten im JSON-Format. Die Datenausgabe erfolgt ebenfalls im JSON-Format. Die Daten sehen wie folgt aus:

    {"devicetime":"2014-12-11T02:24:56.8850110Z","hmdt":42.7,"temp":72.6,"prss":98187.75,"lght":0.38,"dspl":"R-PI Olivier's Office"}

Mit der folgenden Abfrage wird eine Warnung gesendet, wenn ein Licht ausgeschaltet wird:

    SELECT AVG(lght),
     "LightOff" as AlertText
    FROM input TIMESTAMP
    BY devicetime
     WHERE
        lght< 0.05 GROUP BY TumblingWindow(second, 1)

### <a name="measure-throughput"></a>Messen des Durchsatzes

In diesem Kontext ist der Durchsatz die Menge der von Stream Analytics in einem festen Zeitraum verarbeiteten Eingabedaten. (Die Messung dauerte 10 Minuten.) Um den optimalen Verarbeitungsdurchsatz für die Eingabedaten zu erzielen, wurden sowohl die Datenstromeingabe als auch die Abfrage partitioniert. In die Abfrage wurde **COUNT()** eingefügt, um zu messen, wie viele Eingabeereignisse verarbeitet wurden. Um sicherzustellen, dass der Auftrag nicht einfach auf eingehende Eingabeereignisse wartet, wurde jede Event Hub-Partition der Eingabe im Voraus mit ca. 300 MB Eingabedaten geladen.

Die folgende Tabelle enthält die Ergebnisse, die sich ergeben haben, nachdem die Anzahl der Streamingeinheiten und die entsprechende Anzahl von Partitionen in Event Hubs erhöht wurden.  

<table border="1">
<tr><th>Eingabepartitionen</th><th>Ausgabepartitionen</th><th>Streaming-Einheiten</th><th>Anhaltender Durchsatz
</th></td>

<tr><td>12</td>
<td>12</td>
<td>6</td>
<td>4,06 MB/s</td>
</tr>

<tr><td>12</td>
<td>12</td>
<td>12</td>
<td>8,06 MB/s</td>
</tr>

<tr><td>48</td>
<td>48</td>
<td>48</td>
<td>38,32 MB/s</td>
</tr>

<tr><td>192</td>
<td>192</td>
<td>192</td>
<td>172,67 MB/s</td>
</tr>

<tr><td>480</td>
<td>480</td>
<td>480</td>
<td>454,27 MB/s</td>
</tr>

<tr><td>720</td>
<td>720</td>
<td>720</td>
<td>609,69 MB/s</td>
</tr>
</table>

Im folgenden Diagramm wird die Beziehung zwischen SUs und dem Durchsatz visualisiert.

![img.stream.analytics.perfgraph][img.stream.analytics.perfgraph]

## <a name="get-help"></a>Hier erhalten Sie Hilfe
Um Hilfe zu erhalten, nutzen Sie unser [Azure Stream Analytics-Forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics).

## <a name="next-steps"></a>Nächste Schritte
* [Einführung in Azure Stream Analytics](stream-analytics-introduction.md)
* [Erste Schritte mit Azure Stream Analytics](stream-analytics-real-time-fraud-detection.md)
* [Stream Analytics Query Language Reference (in englischer Sprache)](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [Referenz zur Azure Stream Analytics-Verwaltungs-REST-API](https://msdn.microsoft.com/library/azure/dn835031.aspx)

<!--Image references-->

[img.stream.analytics.monitor.job]: ./media/stream-analytics-scale-jobs/StreamAnalytics.job.monitor-NewPortal.png
[img.stream.analytics.configure.scale]: ./media/stream-analytics-scale-jobs/StreamAnalytics.configure.scale.png
[img.stream.analytics.perfgraph]: ./media/stream-analytics-scale-jobs/perf.png
[img.stream.analytics.streaming.units.scale]: ./media/stream-analytics-scale-jobs/StreamAnalyticsStreamingUnitsExample.jpg
[img.stream.analytics.preview.portal.settings.scale]: ./media/stream-analytics-scale-jobs/StreamAnalyticsPreviewPortalJobSettings-NewPortal.png   

<!--Link references-->

[microsoft.support]: http://support.microsoft.com
[azure.management.portal]: http://manage.windowsazure.com
[azure.event.hubs.developer.guide]: http://msdn.microsoft.com/library/azure/dn789972.aspx

[stream.analytics.introduction]: stream-analytics-introduction.md
[stream.analytics.get.started]: stream-analytics-real-time-fraud-detection.md
[stream.analytics.query.language.reference]: http://go.microsoft.com/fwlink/?LinkID=513299
[stream.analytics.rest.api.reference]: http://go.microsoft.com/fwlink/?LinkId=517301


