---
title: "Behandlung von Ereignissen außerhalb der Reihenfolge oder bei Verzögerung in Azure Stream Analytics | Microsoft-Dokumentation"
description: "Erfahren Sie, wie Stream Analytics mit Ereignissen außerhalb der Reihenfolge oder spät empfangenen Ereignissen in Datenströmen umgeht."
keywords: "außerhalb der Reihenfolge, spät empfangen, Ereignisse"
documentationcenter: 
services: stream-analytics
author: samacha
manager: jhubbard
editor: cgronlun
ms.assetid: 
ms.service: stream-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 04/20/2017
ms.author: samacha
ms.translationtype: HT
ms.sourcegitcommit: 8351217a29af20a10c64feba8ccd015702ff1b4e
ms.openlocfilehash: 5089dda48ea829902663ef9d09fe83177df6f220
ms.contentlocale: de-de
ms.lasthandoff: 08/29/2017

---
# <a name="azure-stream-analytics-event-order-handling"></a>Behandlung von Ereignissen außerhalb der Reihenfolge in Azure Stream Analytics

In einem temporären Datenstrom von Ereignissen wird jedes Ereignis mit dem Zeitpunkt erfasst, an dem das Ereignis empfangen wurde. Einige Bedingungen können dazu führen, dass Ereignisdatenströme Ereignisse mitunter in einer anderen Reihenfolge empfangen als in der Reihenfolge, in der sie gesendet wurden. Ursache hierfür kann eine einfache TCP-Neuübertragung oder sogar eine Uhrabweichung zwischen dem sendenden Gerät und dem empfangenden Event Hub sein. Interpunktionsereignisse werden ebenfalls zu empfangenen Ereignisdatenströmen hinzugefügt, um die Zeit ohne Ereignisankunft fortzuführen. Diese sind in Szenarien wie bei folgender Option erforderlich: „Benachrichtigung erhalten, wenn 3 Minuten lang keine Anmeldungen stattfinden.“

Eingabedatenströmen außerhalb der Reihenfolge weisen eine der folgenden Eigenschaften auf:
* Sie wurden sortiert (und sind daher **verzögert**).
* Sie wurden vom System gemäß einer benutzerdefinierten Richtlinie angepasst.


## <a name="lateness-tolerance"></a>Verzögerungstoleranz
Stream Analytics toleriert folgende Typen von Szenarien. Stream Analytics bietet verschiedene Möglichkeiten zur Behandlung von Ereignissen außerhalb der Reihenfolge und spät empfangenen Ereignissen. Derartige Ereignisse werden wie folgt behandelt:

* Ereignisse mit falscher Reihenfolge, die jedoch innerhalb des festgelegten Toleranzfensters eingehen, werden **nach dem Zeitstempel neu angeordnet**.
* Ereignisse, die außerhalb des Toleranzfensters eintreffen, werden **verworfen oder angepasst**.
    * **Angepasst**: Ereignisse werden so angepasst, als ob sie zum spätmöglichsten zulässigen Zeitpunkt angekommen wären.
    * **Verworfen**: Ereignisse werden verworfen.

![Behandlung von Ereignissen in Stream Analytics](media/stream-analytics-event-handling/stream-analytics-event-handling.png)

## <a name="reduce-the-number-of-out-of-order-events"></a>Reduzieren der Anzahl von Ereignissen außerhalb der Reihenfolge

Da Stream Analytics eine temporale Transformation bei der Verarbeitung von eingehenden Ereignissen (z.B. für Fensteraggregate oder temporale Verknüpfungen) anwendet, sortiert Stream Analytics eingehende Ereignisse in der Reihenfolge der Zeitstempel.

Wenn das Schlüsselwort „Zeitstempel von“ **nicht** verwendet wird, wird standardmäßig die Zeit zum Einreihen von Azure Event Hub-Ereignissen in die Warteschlange verwendet. Event Hubs gewährleisten die Monotonie des Zeitstempels in jeder Partition des Event Hubs. Außerdem wird sichergestellt, dass Ereignisse von allen Partitionen gemäß der Reihenfolge der Zeitstempel zusammengeführt werden. Durch diese beiden Event Hub-Eigenschaften wird sichergestellt, dass keine Ereignisse außerhalb der Reihenfolge auftreten.

Gelegentlich müssen Sie möglicherweise den Zeitstempel des Absenders verwenden. In diesem Fall wird mithilfe von „Zeitstempel von“ ein Zeitstempel aus der Ereignisnutzlast ausgewählt. Bei diesen Szenarien kann eine falsche Ereignisreihenfolge auf eine oder mehrere Ursachen zurückgeführt werden.

* Ereignisproducer weisen Uhrabweichungen auf. Dies ist häufig der Fall, wenn die Producer von unterschiedlichen Computern stammen, wodurch die Uhrabweichung entsteht.
* Es liegt eine Netzwerkverzögerung von der Quelle der Ereignisse zum Ziel-Event Hub vor.
* Es liegen Uhrabweichungen zwischen Event Hub-Partitionen vor. Stream Analytics sortiert zuerst Ereignisse von allen Event Hub-Partitionen nach der Zeit zum Einreihen von Ereignissen in die Warteschlange. Anschließend wird der Datenstrom auf Ereignisse mit falscher Reihenfolge geprüft.

Auf der Registerkarte „Konfiguration“ werden folgende Standardwerte angezeigt:

![Behandlung von Ereignissen außerhalb der Reihenfolge bei Stream Analytics](media/stream-analytics-event-handling/stream-analytics-out-of-order-handling.png)

Wenn Sie als Toleranzfenster für Ereignisse außerhalb der Reihenfolge einen Wert von 0 Sekunden festlegen, bestätigen Sie, dass alle Ereignisse stets die Reihenfolge einhalten. Angesichts der drei Ursachen, die zum Auftreten von Ereignissen mit falscher Reihenfolge führen können, gilt ein solcher Fall als unwahrscheinlich. 

Um Stream Analytics die Korrektur von Ereignissen mit falscher Reihenfolge zu ermöglichen, können Sie ein Toleranzfenster für Ereignisse außerhalb der Reihenfolge ungleich 0 festlegen. Stream Analytics puffert Ereignisse bis zu diesem Fenster und ordnet diese mithilfe des von Ihnen gewählten Zeitstempels neu an. Anschließend wird die temporale Transformation angewendet. Sie können mit einem 3-Sekunden-Fenster beginnen und den Wert anpassen, um die Anzahl der Ereignisse, deren Zeit angepasst wird, zu reduzieren. 

Ein Nebeneffekt der Pufferung ist, dass die Ausgabe **um denselben Zeitraum verzögert wird**. Sie können den Wert anpassen, um die Anzahl von Ereignissen außerhalb der Reihenfolge zu verringern und die Auftragslatenz niedrig zu halten.

## <a name="get-help"></a>Hier erhalten Sie Hilfe
Um weitere Hilfe zu erhalten, nutzen Sie unser [Azure Stream Analytics-Forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics).

## <a name="next-steps"></a>Nächste Schritte
* [Einführung in Azure Stream Analytics](stream-analytics-introduction.md)
* [Erste Schritte mit Stream Analytics](stream-analytics-real-time-fraud-detection.md)
* [Skalieren von Stream Analytics-Aufträgen](stream-analytics-scale-jobs.md)
* [Referenz zur Stream Analytics-Abfragesprache](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [Referenz zur REST-API für die Stream Analytics-Verwaltung](https://msdn.microsoft.com/library/azure/dn835031.aspx)
