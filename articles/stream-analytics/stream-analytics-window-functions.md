<properties
	pageTitle="Einführung in Stream Analytics-Fensterfunktionen | Microsoft Azure"
	description="Enthält Informationen zu den drei Fensterfunktionen in Stream Analytics (rollierende Fenster, springende Fenster, Schiebefenster)."
	keywords="rollierendes Fenster, Schiebefenster, springendes Fenster"
	documentationCenter=""
	services="stream-analytics"
	authors="jeffstokes72"
	manager="jhubbard"
	editor="cgronlun"
/>

<tags
	ms.service="stream-analytics"
	ms.devlang="na"
	ms.topic="article"
	ms.tgt_pltfrm="na"
	ms.workload="data-services"
	ms.date="08/08/2016"
	ms.author="jeffstok"
/>


# Einführung in Stream Analytics-Fensterfunktionen

In vielen Szenarien mit Echtzeit-Streaming ist es erforderlich, Vorgänge nur für die Daten in temporalen Fenstern durchzuführen. Die native Unterstützung für Fensterfunktionen ist ein Hauptfeature von Azure Stream Analytics, mit dem die Entwicklerproduktivität beim Erstellen komplexer Aufträge mit Datenstromverarbeitung gefördert wird. Stream Analytics ermöglicht Entwicklern die Verwendung von [**rollierenden Fenstern**](https://msdn.microsoft.com/library/dn835055.aspx), [**springenden Fenstern**](https://msdn.microsoft.com/library/dn835041.aspx) und [**Schiebefenstern**](https://msdn.microsoft.com/library/dn835051.aspx), um für Streamingdaten temporale Vorgänge durchzuführen. Beachten Sie, dass die Ergebnisse für alle [Fenster](https://msdn.microsoft.com/library/dn835019.aspx)vorgänge am **Ende** des Fensters ausgegeben werden. Die Ausgabe des Fensters ist ein einzelnes Ereignis, das auf der verwendeten Aggregatfunktion basiert. Das Ereignis verfügt über den Zeitstempel vom Ende des Fensters, und alle Fensterfunktionen werden mit einer festen Länge definiert. Schließlich sollten Sie noch beachten, dass alle Fensterfunktionen in einer [**GROUP BY**](https://msdn.microsoft.com/library/dn835023.aspx)-Klausel verwendet werden sollten.

![Stream Analytics-Fensterfunktionen – Konzepte](media/stream-analytics-window-functions/stream-analytics-window-functions-conceptual.png)

## Rollierendes Fenster

Rollierende Fensterfunktionen werden verwendet, um einen Datenstrom in einzelne Zeitsegmente zu unterteilen und dafür eine Funktion durchzuführen, z.B. wie im Beispiel unten. Die wichtigsten Unterscheidungsmerkmale eines rollierenden Fensters sind, dass sie wiederholt werden, sich nicht überlappen und ein Ereignis nicht zu mehr als einem rollierenden Fenster gehören kann.

![Stream Analytics-Fensterfunktionen – Einführung in rollierende Fenster](media/stream-analytics-window-functions/stream-analytics-window-functions-tumbling-intro.png)

## Springendes Fenster

Bei den Funktionen von springenden Fenstern wird für einen festen Zeitraum ein Sprung nach vorn durchgeführt. Sie können sich dies wie rollierende Fenster vorstellen, die sich überlappen können, damit die Ereignisse mehr als einem Resultset für springende Fenster angehören können. Um ein springendes Fenster an ein rollierendes Fenster anzugleichen, müsste lediglich die Sprunggröße an die Fenstergröße angepasst werden.

![Stream Analytics-Fensterfunktionen – Einführung in springende Fenster](media/stream-analytics-window-functions/stream-analytics-window-functions-hopping-intro.png)

## Schiebefenster

Bei Schiebefensterfunktionen wird im Gegensatz zu rollierenden oder springenden Fenstern **nur** dann ein Ereignis produziert, wenn ein Ereignis eintritt. Jedes Fenster verfügt über mindestens ein Ereignis, und das Fenster wird fortlaufend um „€“ (Epsilon) nach vorn verschoben. Wie bei springenden Fenstern auch, können Ereignisse zu mehr als einem Schiebefenster gehören.

![Stream Analytics-Fensterfunktionen – Einführung in Schiebefenster](media/stream-analytics-window-functions/stream-analytics-window-functions-sliding-intro.png)

## Hilfe zu Fensterfunktionen

Um Hilfe zu erhalten, besuchen Sie unser [Azure Stream Analytics-Forum](https://social.msdn.microsoft.com/Forums/de-DE/home?forum=AzureStreamAnalytics).

## Nächste Schritte

- [Einführung in Azure Stream Analytics](stream-analytics-introduction.md)
- [Erste Schritte mit Azure Stream Analytics](stream-analytics-get-started.md)
- [Skalieren von Azure Stream Analytics-Aufträgen](stream-analytics-scale-jobs.md)
- [Stream Analytics Query Language Reference (in englischer Sprache)](https://msdn.microsoft.com/library/azure/dn834998.aspx)
- [Referenz zur Azure Stream Analytics-Verwaltungs-REST-API](https://msdn.microsoft.com/library/azure/dn835031.aspx)

<!---HONumber=AcomDC_0921_2016-->