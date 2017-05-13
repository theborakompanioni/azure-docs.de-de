---
title: "Azure Stream Analytics: Optimieren von Aufträgen zur effizienten Verwendung von Premium-Streamingeinheiten | Microsoft-Dokumentation"
description: "Fragen Sie die bewährten Methoden zur Skalierung und Leistung in Azure Stream Analytics ab."
keywords: Premium-Streamingeinheit, Leistung abfragen
services: stream-analytics
documentationcenter: 
author: jeffstokes72
manager: jhubbard
editor: cgronlun
ms.assetid: 
ms.service: stream-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 04/20/2017
ms.author: jeffstok
ms.translationtype: Human Translation
ms.sourcegitcommit: 7f8b63c22a3f5a6916264acd22a80649ac7cd12f
ms.openlocfilehash: 264331293b16ffb76d037a233eac564e1067f784
ms.contentlocale: de-de
ms.lasthandoff: 05/01/2017


---

# <a name="optimize-your-job-to-use-streaming-units-efficiently"></a>Optimieren von Aufträgen zur effizienten Verwendung von Premium-Streamingeinheiten

Azure Stream Analytics aggregiert die Leistungsgewichtung für die Ausführung eines Auftrags in Premium-Streamingeinheiten. Premium-Streamingeinheiten stellen die Computerressourcen dar, die bei der Ausführung eines Auftrags verbraucht werden. SUs bieten anhand eines kombinierten Maßes aus CPU, Arbeitsspeicher und Schreib- und Leseraten eine Möglichkeit zur Beschreibung der relativen Ereignisverarbeitungskapazität. Mit dieser Kapazität können Sie sich auf die Abfragelogik konzentrieren. Sie müssen keine Überlegungen zur Leistung auf Speicherebene anstellen, nicht manuell Speicher für Ihren Auftrag zuweisen, und nicht die ungefähre CPU-Kernzahl kennen, um Ihren Auftrags rechtzeitig auszuführen.

## <a name="how-many-sus-are-required-for-a-job"></a>Wie viele Premium-Streamingeinheiten sind für einen Auftrag erforderlich?

Die benötigte Anzahl an Premium-Streamingeinheiten für einen bestimmten Auftrag hängt von der Partitionskonfiguration für die Eingaben und der Abfrage ab, die für den Auftrag definiert ist. Auf dem Blatt **Skala** können Sie die richtige Anzahl von Premium-Streamingeinheiten festlegen. Es wird empfohlen, mehr Premium-Streamingeinheiten als erforderlich zuzuweisen. Das Stream Analytics-Verarbeitungsmodul führt zu einer niedrigeren Latenz und einem höheren Durchsatz. Dabei wird zusätzlicher Speicherplatz beansprucht.

Im Allgemeinen wird empfohlen, für Abfragen, die nicht *PARTITIONIEREN NACH* verwenden, mit 6 Premium-Streamingeinheiten zu beginnen. Ermitteln Sie dann die optimale Anzahle mittels Trial-and-Error-Methode. Dabei ändern Sie die Anzahl der Premium-Streamingeinheiten, nachdem Sie eine repräsentative Datenmenge übertragen und die %Auslastungsmetrik der Premium-Streamingeinheiten überprüft haben.

Azure Stream Analytics platziert Ereignisse im Fenster „Puffer für die Neuanordnung“, bevor die Verarbeitung beginnt. Ereignisse werden im Fenster für die Neuanordnung nach Zeit sortiert. Nachfolgende Vorgänge werden für die chronologisch sortierten Ereignisse ausgeführt. Durch die Neuanordnung von Ereignissen nach Zeit wird sichergestellt, dass der Betreiber Einblick in alle Ereignisse im vorgesehenen Zeitraum erhält. Außerdem kann der Betreiber die erforderliche Verarbeitung durchführen und eine Ausgabe erzeugen. Als Nebeneffekt dieses Mechanismus wird die Verarbeitung um die Dauer im Neuanordungsfenster verzögert. Der Speicherbedarf des Auftrags (der sich auf den Verbrauch von Premium-Streamingeinheiten auswirkt) ist eine Funktion mit der Größe dieses Neuanordnungsfensters und der Anzahl der darin enthaltenen Ereignisse.

> [!NOTE]
> Wenn sich die Anzahl der Leser während der Aktualisierung des Auftrags ändert, werden vorübergehende Warnungen in Überwachungsprotokolle geschrieben. Stream Analytics-Aufträge werden automatisch von diesen vorübergehenden Problemen behoben.

## <a name="common-high-memory-causes-for-high-su-usage-for-running-jobs"></a>Häufige Ursachen für die hohe Auslastung von Premium-Streamingeinheiten bei der Ausführung von Aufträgen

### <a name="high-cardinality-for-group-by"></a>Hohe Kardinalität für „GRUPPIEREN NACH“

Die Kardinalität der eingehenden Ereignisse bestimmt die Speicherauslastung für den Auftrag.

In `SELECT count(*) from input group by clustered, tumblingwindow (minutes, 5)` ist die mit dem **Cluster** verknüpfte Zahl die Kardinalität der Abfrage.

Um durch eine hohe Kardinalität verursachte Probleme zu verhindern, skalieren Sie die Abfrage horizontal hoch, indem Sie die Partitionen durch **PARTITIONIEREN NACH** erhöhen.

```
Select count(*) from input
Partition By clusterid
GROUP BY clustered tumblingwindow (minutes, 5)
```

Bei der Anzahl von *Cluster* handelt es sich in diesem Fall um die Kardinalität von „GRUPPIEREN NACH“.

Nach dem Partitionieren der Abfrage wird sie auf mehrere Knoten verteilt. Infolgedessen verringert sich die Anzahl eingehender Ereignisse auf den einzelnen Knoten, wodurch wiederum die Größe des Puffers für die Neuanordnung reduziert wird. Sie sollten auch Event Hub-Partitionen nach Partitionid partitionieren.

### <a name="high-unmatched-event-count-for-join"></a>Hohe Anzahl von nicht abgeglichenen Ereignissen für JOIN-Vorgänge

Die Anzahl nicht abgeglichener Ereignisse im JOIN-Vorgang wirkt sich auf die Arbeitsspeicherauslastung für die Abfrage aus. Nehmen Sie beispielsweise eine Abfrage, mit der die Anzahl der Anzeigenaufrufe, die Klicks generieren, gesucht wird:

```
SELECT id from clicks INNER JOIN,
impressions on impressions.id = clicks.id AND DATEDIFF(hour, impressions, clicks) between 0 AND 10
```

In diesem Szenario ist es möglich, dass viele Anzeigen angezeigt werden und wenige Mausklicks generiert werden. Bei einem derartigen Ergebnis müssten alle Ereignisse des Auftrags im selben Zeitfenster verarbeitet werden. Die Menge des belegten Arbeitsspeichers ist proportional zur Fenstergröße und Ereignisrate. 

Um diese Situation zu beheben, skalieren Sie die Abfrage horizontal hoch, indem Sie die Partitionen mithilfe von „PARTITIONIEREN NACH“ erhöhen. 

Nach dem Partitionieren der Abfrage wird sie auf mehrere Verarbeitungsknoten verteilt. Infolgedessen verringert sich die Anzahl eingehender Ereignisse auf den einzelnen Knoten, wodurch wiederum die Größe des Puffers für die Neuanordnung reduziert wird.

### <a name="large-number-of-out-of-order-events"></a>Große Anzahl von Ereignissen mit falscher Reihenfolge 

Eine hohe Anzahl von Ereignissen mit falscher Reihenfolge in einem großen Zeitfenster hat einen größeren Puffer für die Neuanordnung zur Folge. Um diese Situation zu beheben, skalieren Sie die Abfrage, indem Sie die Partitionen mithilfe von „PARTITIONIEREN NACH“ erhöhen. Nach dem Partitionieren der Abfrage wird sie auf mehrere Knoten verteilt. Infolgedessen verringert sich die Anzahl eingehender Ereignisse auf den einzelnen Knoten, wodurch wiederum die Größe des Puffers für die Neuanordnung reduziert wird. 


## <a name="get-help"></a>Hier erhalten Sie Hilfe
Um Hilfe zu erhalten, nutzen Sie unser [Azure Stream Analytics-Forum](https://social.msdn.microsoft.com/Forums/home?forum=AzureStreamAnalytics).

## <a name="next-steps"></a>Nächste Schritte
* [Einführung in Azure Stream Analytics](stream-analytics-introduction.md)
* [Erste Schritte mit Azure Stream Analytics](stream-analytics-get-started.md)
* [Skalieren von Azure Stream Analytics-Aufträgen](stream-analytics-scale-jobs.md)
* [Stream Analytics Query Language Reference](https://msdn.microsoft.com/library/azure/dn834998.aspx) (Referenz zur Stream Analytics-Abfragesprache)
* [Azure Stream Analytics Management REST API reference](https://msdn.microsoft.com/library/azure/dn835031.aspx) (Referenz zur Azure Stream Analytics-Verwaltungs-REST-API)

