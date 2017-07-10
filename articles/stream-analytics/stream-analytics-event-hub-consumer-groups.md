---
title: "Debuggen von Azure Stream Analytics mit Event Hub-Empfängern | Microsoft-Dokumentation"
description: "Bewährte Methoden für Abfragen zum Berücksichtigen von Event Hubs-Consumergruppen in Stream Analytics-Aufträgen."
keywords: Event Hub-Gruppe, Consumergruppe
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
ms.sourcegitcommit: 6dbb88577733d5ec0dc17acf7243b2ba7b829b38
ms.openlocfilehash: d1c099dd39fd6a58bf4d0679f536499d6dd8d29a
ms.contentlocale: de-de
ms.lasthandoff: 07/04/2017


---

# <a name="debug-azure-stream-analytics-with-event-hub-receivers"></a>Debuggen von Azure Stream Analytics mit Event Hub-Empfängern

Sie können Azure Event Hubs in Azure Stream Analytics nutzen, um Daten in einem Auftrag zu erfassen oder aus diesem auszugeben. Eine bewährte Methode beim Arbeiten mit Event Hubs ist die Verwendung mehrerer Consumergruppen, um die Skalierbarkeit von Aufträgen sicherzustellen. Ein Grund hierfür ist, dass die Anzahl der Leser im Stream Analytics-Auftrag für eine bestimmte Eingabe sich auf die Anzahl der Leser in einer einzelnen Consumergruppe auswirkt. Die genaue Anzahl der Empfänger basiert auf internen Implementierungsdetails für die Logik der horizontalen Skalierungstopologie. Die Anzahl der Empfänger wird nicht extern verfügbar gemacht. Die Anzahl der Leser kann sich entweder zur Startzeit des Auftrags oder im Verlauf von Auftragsaktualisierungen ändern.

> [!NOTE]
> Wenn sich die Anzahl der Leser während der Aktualisierung des Auftrags ändert, werden Warnungen zu vorübergehenden Problemen in Überwachungsprotokolle geschrieben. Diese vorübergehenden Probleme werden in Stream Analytics-Aufträgen automatisch behoben.

## <a name="number-of-readers-per-partition-exceeds-event-hubs-limit-of-five"></a>Die Anzahl der Leser pro Partition überschreitet den Event Hubs-Grenzwert (5)

Es folgen Szenarien, in denen die Anzahl der Leser pro Partition den Event Hubs-Grenzwert 5 überschreitet:

* Mehrere SELECT-Anweisungen: Bei Verwendung mehrerer SELECT-Anweisungen, die auf die **gleiche** Event Hub-Eingabe verweisen, bewirkt jede SELECT-Anweisung, dass ein neuer Empfänger erstellt wird.
* UNION: Wenn Sie UNION verwenden, es ist möglich, dass mehrere Eingaben auf den **gleichen** Event Hub und die gleiche Consumergruppe verweisen.
* SELF JOIN: Wenn Sie einen SELF JOIN-Vorgang verwenden, es ist möglich, dass auf den **gleichen** Event Hub mehrmals verwiesen wird.

## <a name="solution"></a>Lösung

Es folgen bewährte Methoden zum Vermeiden von Szenarien, in denen die Anzahl der Leser pro Partition den Event Hubs-Grenzwert 5 überschreitet.

### <a name="split-your-query-into-multiple-steps-by-using-a-with-clause"></a>Unterteilen der Abfrage mithilfe einer WITH-Klausel in mehrere Schritte

Die WITH-Klausel gibt ein temporäres benanntes Resultset an, auf das in der Abfrage in einer FROM-Klausel verwiesen werden kann. Sie definieren die WITH-Klausel im Ausführungsbereich einer einzelnen SELECT-Anweisung.

Verwenden Sie beispielsweise anstelle dieser Abfrage:

```
SELECT foo 
INTO output1
FROM inputEventHub

SELECT bar
INTO output2
FROM inputEventHub 
…
```

Diese Abfrage:

```
WITH input (
   SELECT * FROM inputEventHub
) as data

SELECT foo
INTO output1
FROM data

SELECT bar
INTO output2
FROM data
…
```

### <a name="ensure-that-inputs-bind-to-different-consumer-groups"></a>Sicherstellen, dass Eingaben an verschiedene Consumergruppen gebunden werden

Erstellen Sie für Abfragen, bei denen drei oder mehr Eingaben mit der gleichen Event Hubs-Consumergruppe verbunden sind, separate Consumergruppen. Hierfür ist die Erstellung zusätzlicher Stream Analytics-Eingaben erforderlich.


## <a name="get-help"></a>Hier erhalten Sie Hilfe
Um weitere Hilfe zu erhalten, nutzen Sie unser [Azure Stream Analytics-Forum](https://social.msdn.microsoft.com/Forums/home?forum=AzureStreamAnalytics).

## <a name="next-steps"></a>Nächste Schritte
* [Einführung in Azure Stream Analytics](stream-analytics-introduction.md)
* [Erste Schritte mit Stream Analytics](stream-analytics-real-time-fraud-detection.md)
* [Skalieren von Stream Analytics-Aufträgen](stream-analytics-scale-jobs.md)
* [Referenz zur Stream Analytics-Abfragesprache](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [Referenz zur REST-API für die Stream Analytics-Verwaltung](https://msdn.microsoft.com/library/azure/dn835031.aspx)

