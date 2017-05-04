---
title: "Einführung in Stream Analytics-Fensterfunktionen | Microsoft Docs"
description: "Enthält Informationen zu den drei Fensterfunktionen in Stream Analytics (rollierende Fenster, springende Fenster, Schiebefenster)."
keywords: rollierendes Fenster, Schiebefenster, springendes Fenster
documentationcenter: 
services: stream-analytics
author: jeffstokes72
manager: jhubbard
editor: cgronlun
ms.assetid: 0d8d8717-5d23-43f0-b475-af078ab4627d
ms.service: stream-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 03/28/2017
ms.author: jeffstok
translationtype: Human Translation
ms.sourcegitcommit: 7f8b63c22a3f5a6916264acd22a80649ac7cd12f
ms.openlocfilehash: 8f814708c4536e709fbe85b8a3a7bfb44e44e81f
ms.lasthandoff: 05/01/2017


---
# <a name="introduction-to-stream-analytics-window-functions"></a>Einführung in Stream Analytics-Fensterfunktionen
In vielen Szenarien mit Echtzeit-Streaming ist es erforderlich, Vorgänge nur für die Daten in temporalen Fenstern durchzuführen. Die native Unterstützung für Fensterfunktionen ist ein Hauptfeature von Azure Stream Analytics, mit dem die Entwicklerproduktivität beim Erstellen komplexer Aufträge mit Datenstromverarbeitung gefördert wird. Stream Analytics ermöglicht Entwicklern die Verwendung von [**rollierenden Fenstern**](https://msdn.microsoft.com/library/dn835055.aspx), [**springenden Fenstern**](https://msdn.microsoft.com/library/dn835041.aspx) und [**Schiebefenstern**](https://msdn.microsoft.com/library/dn835051.aspx), um für Streamingdaten temporale Vorgänge durchzuführen. Beachten Sie, dass die Ergebnisse für alle [Fenster](https://msdn.microsoft.com/library/dn835019.aspx) vorgänge am **Ende** des Fensters ausgegeben werden. Die Ausgabe des Fensters ist ein einzelnes Ereignis, das auf der verwendeten Aggregatfunktion basiert. Das Ereignis verfügt über den Zeitstempel vom Ende des Fensters, und alle Fensterfunktionen werden mit einer festen Länge definiert. Schließlich sollten Sie noch beachten, dass alle Fensterfunktionen in einer [**GROUP BY**](https://msdn.microsoft.com/library/dn835023.aspx)-Klausel verwendet werden sollten.

![Stream Analytics-Fensterfunktionen – Konzepte](media/stream-analytics-window-functions/stream-analytics-window-functions-conceptual.png)

## <a name="tumbling-window"></a>Rollierendes Fenster
Rollierende Fensterfunktionen werden verwendet, um einen Datenstrom in einzelne Zeitsegmente zu unterteilen und dafür eine Funktion durchzuführen, z.B. wie im Beispiel unten. Die wichtigsten Unterscheidungsmerkmale eines rollierenden Fensters sind, dass sie wiederholt werden, sich nicht überlappen und ein Ereignis nicht zu mehr als einem rollierenden Fenster gehören kann.

![Stream Analytics-Fensterfunktionen – Einführung in rollierende Fenster](media/stream-analytics-window-functions/stream-analytics-window-functions-tumbling-intro.png)

## <a name="hopping-window"></a>Springendes Fenster
Bei den Funktionen von springenden Fenstern wird für einen festen Zeitraum ein Sprung nach vorn durchgeführt. Sie können sich dies wie rollierende Fenster vorstellen, die sich überlappen können, damit die Ereignisse mehr als einem Resultset für springende Fenster angehören können. Um ein springendes Fenster an ein rollierendes Fenster anzugleichen, müsste lediglich die Sprunggröße an die Fenstergröße angepasst werden. 

![Stream Analytics-Fensterfunktionen – Einführung in springende Fenster](media/stream-analytics-window-functions/stream-analytics-window-functions-hopping-intro.png)

## <a name="sliding-window"></a>Schiebefenster
Bei Schiebefensterfunktionen wird im Gegensatz zu rollierenden oder springenden Fenstern **nur** dann ein Ereignis produziert, wenn ein Ereignis eintritt. Jedes Fenster verfügt über mindestens ein Ereignis, und das Fenster wird fortlaufend um „€“ (Epsilon) nach vorn verschoben. Wie bei springenden Fenstern auch, können Ereignisse zu mehr als einem Schiebefenster gehören.

![Stream Analytics-Fensterfunktionen – Einführung in Schiebefenster](media/stream-analytics-window-functions/stream-analytics-window-functions-sliding-intro.png)

## <a name="getting-help-with-window-functions"></a>Hilfe zu Fensterfunktionen
Um Hilfe zu erhalten, besuchen Sie unser [Azure Stream Analytics-Forum](https://social.msdn.microsoft.com/Forums/home?forum=AzureStreamAnalytics)

## <a name="next-steps"></a>Nächste Schritte
* [Einführung in Azure Stream Analytics](stream-analytics-introduction.md)
* [Erste Schritte mit Azure Stream Analytics](stream-analytics-get-started.md)
* [Skalieren von Azure Stream Analytics-Aufträgen](stream-analytics-scale-jobs.md)
* [Stream Analytics Query Language Reference (in englischer Sprache)](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [Referenz zur Azure Stream Analytics-Verwaltungs-REST-API](https://msdn.microsoft.com/library/azure/dn835031.aspx)


