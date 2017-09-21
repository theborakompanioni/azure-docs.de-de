---
title: "Behandlung von Ereignissen außerhalb der Reihenfolge oder bei Verzögerung in Azure Stream Analytics | Microsoft-Dokumentation"
description: "Erfahren Sie, wie Azure Stream Analytics mit Ereignissen außerhalb der Reihenfolge oder spät empfangenen Ereignissen in Datenströmen umgeht."
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
ms.sourcegitcommit: fda37c1cb0b66a8adb989473f627405ede36ab76
ms.openlocfilehash: cf9a43fbb82a32c92d66f25809916d3ccde1a20d
ms.contentlocale: de-de
ms.lasthandoff: 09/14/2017

---
# <a name="azure-stream-analytics-event-order-handling"></a>Behandlung von Ereignissen außerhalb der Reihenfolge in Azure Stream Analytics

In einem temporalen Datenstrom von Ereignissen wird jedem Ereignis ein Zeitstempel zugewiesen. Azure Stream Analytics weist jedem Ereignis Zeitstempel zu. Dabei wird entweder die Eingangszeit oder die Anwendungszeit verwendet. 

Die Spalte **System.Timestamp** enthält den dem Ereignis zugewiesenen Zeitstempel. Die Eingangszeit wird an der Eingabequelle zugewiesen, wenn das Ereignis die Quelle erreicht. Die Eingangszeit ist **EventEnqueuedTime** (Event Hub-Eingaben) bzw. [blob last modified time](https://docs.microsoft.com/en-us/dotnet/api/microsoft.windowsazure.storage.blob.blobproperties.lastmodified?view=azurestorage-8.1.3) (Blobeingaben). Die Anwendungszeit wird bei Erstellung des Ereignisses zugewiesen und ist Teil der Nutzlast. 

Verwenden Sie zur Verarbeitung von Ereignissen auf der Grundlage der Anwendungszeit die TIMESTAMP BY-Klausel in der SELECT-Abfrage. Ist die TIMESTAMP BY-Klausel nicht vorhanden, werden Ereignisse basierend auf der Eingangszeit verarbeitet. Auf die Eingangszeit kann bei Event Hubs mithilfe der **EventEnqueuedTime**-Eigenschaft und bei Blobeingaben mithilfe der **BlobLastModified**-Eigenschaft zugegriffen werden. 

Azure Stream Analytics erzeugt die Ausgabe in der Reihenfolge der Zeitstempel und bietet einige Einstellungen zur Behandlung von Daten außerhalb der Reihenfolge.

![Behandlung von Ereignissen in Stream Analytics](media/stream-analytics-event-handling/stream-analytics-event-handling.png)

Eingabedatenströmen außerhalb der Reihenfolge weisen eine der folgenden Eigenschaften auf:
* Sie wurden sortiert (und sind daher *verzögert*).
* Sie wurden vom System gemäß einer benutzerdefinierten Richtlinie angepasst.

Stream Analytics toleriert bei der Verarbeitung auf der Grundlage der Anwendungszeit verzögerte Ereignisse und Ereignisse außerhalb der Reihenfolge.

**Toleranz für Eingangsverzögerung**

* Die Einstellung „Toleranz für Eingangsverzögerung“ gilt nur für die Verarbeitung auf der Grundlage der Anwendungszeit. Andernfalls wird sie ignoriert.
* Die Toleranz für Eingangsverzögerung ist die maximale Differenz zwischen der Eingangszeit und der Anwendungszeit. Falls die Anwendungszeit vor *(Eingangszeit - Fenster für Eingangsverzögerung)* liegt, wird sie auf *(Eingangszeit - Fenster für Eingangsverzögerung)* festgelegt.
* Wenn mehrere Teile des gleichen Eingabedatenstroms oder mehrere Eingabedatenströme kombiniert werden, ist die Toleranz für die Eingangsverzögerung der maximale Zeitraum, den jeder Teil auf neue Daten wartet. 

Kurz gesagt: Das Fenster für die Eingangsverzögerung ist die maximale Verzögerung zwischen der Ereigniserstellung und dem Eingang des Ereignisses an der Eingabequelle.
Zuerst erfolgt die Anpassung auf der Grundlage der Toleranz für die Eingangsverzögerung, anschließend die Anpassung auf der Grundlage der falschen Reihenfolge. Die Spalte **System.Timestamp** enthält den dem Ereignis zugewiesenen endgültigen Zeitstempel.

**Toleranz für Fehlordnung**

* Ereignisse, die in falscher Reihenfolge eingehen, sich aber im Toleranzfenster für Fehlordnung befinden, werden *nach dem Zeitstempel neu angeordnet*. 
* Ereignisse, die außerhalb des Toleranzfensters eintreffen, werden *verworfen* oder *angepasst*.
    * **Angepasst**: Ereignisse werden so angepasst, als ob sie zum spätmöglichsten zulässigen Zeitpunkt angekommen wären. 
    * **Verworfen**: Ereignisse werden verworfen.

Um Ereignisse, die innerhalb des Toleranzfensters für Fehlordnung eingegangen sind, neu anzuordnen, wird die Ausgabe der Abfrage *um das Toleranzfenster für Fehlordnung verzögert*.

**Beispiel**

Toleranz für Eingangsverzögerung = 10 Minuten<br/>
Toleranz für Fehlordnung = 3 Minuten<br/>
Verarbeitung basierend auf der Anwendungszeit<br/>

Ereignisse:

Ereignis 1 _Anwendungszeit_ = 00:00:00, _Eingangszeit_ = 00:10:01, _System.Timestamp_ = 00:00:01: Wird angepasst, da (_Eingangszeit_ - _Anwendungszeit_) größer ist als die Toleranz für Eingangsverzögerung.

Ereignis 2 _Anwendungszeit_ = 00:00:01, _Eingangszeit_ = 00:10:01, _System.Timestamp_ = 00:00:01: Wird nicht angepasst, da die Anwendungszeit im Fenster für Eingangsverzögerung liegt.

Ereignis 3 _Anwendungszeit_ = 00:10:00, _Eingangszeit_ = 00:10:02, _System.Timestamp_ = 00:10:00, wird nicht angepasst, da die Anwendungszeit im Fenster für Eingangsverzögerung liegt.

Ereignis 4 _Anwendungszeit_ = 00:09:00, _Eingangszeit_ = 00:10:03, _System.Timestamp_ = 00:09:00: Wird mit dem ursprünglichen Zeitstempel akzeptiert, da die Anwendungszeit innerhalb der Toleranz für Fehlordnung liegt.

Ereignis 5 _Anwendungszeit_ = 00:06:00, _Eingangszeit_ = 00:10:04, _System.Timestamp_ = 00:07:00: Wird angepasst, da die Anwendungszeit vor der Toleranz für Fehlordnung liegt.



## <a name="get-help"></a>Hier erhalten Sie Hilfe
Um weitere Hilfe zu erhalten, nutzen Sie unser [Azure Stream Analytics-Forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics).

## <a name="next-steps"></a>Nächste Schritte
* [Einführung in Azure Stream Analytics](stream-analytics-introduction.md)
* [Erste Schritte mit Azure Stream Analytics](stream-analytics-real-time-fraud-detection.md)
* [Skalieren von Azure Stream Analytics-Aufträgen](stream-analytics-scale-jobs.md)
* [Stream Analytics Query Language Reference](https://msdn.microsoft.com/library/azure/dn834998.aspx) (Referenz zur Stream Analytics-Abfragesprache)
* [Azure Stream Analytics management REST API reference](https://msdn.microsoft.com/library/azure/dn835031.aspx) (Referenz zur Azure Stream Analytics-Verwaltungs-REST-API)

