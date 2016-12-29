---
title: "Analytics: Das leistungsfähige Suchtool von Azure Application Insights | Microsoft Docs"
description: "Übersicht über Analytics, das leistungsfähige Diagnosesuchtool von Application Insights. "
services: application-insights
documentationcenter: 
author: alancameronwills
manager: carmonm
ms.assetid: 0a2f6011-5bcf-47b7-8450-40f284274b24
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 11/23/2016
ms.author: awills
translationtype: Human Translation
ms.sourcegitcommit: f760494cbe7341391f0ce51bb1161cb1395cbe5c
ms.openlocfilehash: b56ac6e0b75492e8d784c5fc67e555f8f450b65f


---
# <a name="analytics-in-application-insights"></a>Analytics in Application Insights
[Analytics](app-insights-analytics.md) ist die leistungsfähige Suchfunktion von [Application Insights](app-insights-overview.md). Auf diesen Seiten wird die Analytics-Abfragesprache beschrieben. 

* **[Sehen Sie sich das Einführungsvideo an](https://applicationanalytics-media.azureedge.net/home_page_video.mp4)**.
* **[Testen Sie Analytics mit unseren simulierten Daten](https://analytics.applicationinsights.io/demo)**, wenn Ihre App noch keine Daten an Application Insights sendet.
* In der **[Kurzübersicht für SQL-Benutzer](https://aka.ms/sql-analytics)** finden Sie eine Übersetzung der gängigsten Sprachen.

## <a name="queries-in-analytics"></a>Abfragen in Analytics
Eine typische Abfrage ist eine *Quelltabelle*, gefolgt von einer Reihe von *Operatoren*, die durch `|` getrennt sind. 

Finden wir also heraus, zu welcher Tageszeit die Einwohner von Hyderabad unsere Web-App testen. Und während wir damit beschäftigt sind, sehen wir, welche Ergebniscodes an ihre HTTP-Anfragen zurückgegeben werden. 

```AIQL

    requests      // Table of events that log HTTP requests.
    | where timestamp > ago(7d) and client_City == "Hyderabad"
    | summarize clients = dcount(client_IP) 
      by tod_UTC=bin(timestamp % 1d, 1h), resultCode
    | extend local_hour = (tod_UTC + 5h + 30min) % 24h + datetime("2001-01-01") 
```

Wir zählen verschiedene Client-IP-Adressen der letzten 7 Tage auf und gruppieren sie nach Uhrzeit. 

Wir zeigen die Ergebnisse mit der Balkendiagramm-Präsentation und listen die Ergebnisse von verschiedenen Antwortcodes auf.

![Wählen Sie das Balkendiagramm, die X-Achsen und Y-Achsen, dann die Segmentierung](./media/app-insights-analytics/020.png)

Anscheinend ist unserer App zur Mittagszeit und zur Schlafenszeit in Hyderabad am beliebtesten. (Und wir sollten wir diese 500 Codes untersuchen.)

Es gibt auch leistungsstarke statistische Vorgänge:

![](./media/app-insights-analytics/025.png)

Die Sprache verfügt über viele attraktive Features:

* [Filtern](app-insights-analytics-reference.md#where-operator) der Rohdaten Ihrer App-Telemetrie nach beliebigen Feldern, einschließlich Ihrer benutzerdefinierten Eigenschaften und Metriken.
* [Verbinden](app-insights-analytics-reference.md#join-operator) mehrerer Tabellen – Korrelation von Anforderungen mit Seitenansichten, Aufrufen von Abhängigkeiten, Ausnahmen und Protokollablaufverfolgungen.
* Leistungsstarke statistische [Aggregationen](app-insights-analytics-reference.md#aggregations).
* Genauso leistungsstark wie SQL, aber viel einfacher für komplexe Abfragen: anstelle der Schachtelung von Anweisungen übergeben Sie die Daten aus einem elementaren Vorgang an den nächsten.
* Sofortige und leistungsfähige Visualisierungen.
* [Anheften von Diagrammen an Azure-Dashboards](app-insights-analytics-using.md#pin-to-dashboard).
* [Exportieren von Abfragen in Power BI](app-insights-analytics-using.md#export-to-power-bi).
* Es gibt eine [REST-API](https://dev.applicationinsights.io/),die Sie verwenden können, um Abfragen programmgesteuert auszuführen, z.B. über PowerShell.


## <a name="connect-to-your-application-insights-data"></a>Verbinden mit Ihren Application Insights-Daten
Öffnen Sie Analytics über das [Blatt „Übersicht“](app-insights-dashboards.md) Ihrer App in Application Insights: 

![Öffnen Sie unter „portal.azure.com“ die Application Insights-Ressource, und wählen Sie „Analytics“.](./media/app-insights-analytics/001.png)



[!INCLUDE [app-insights-analytics-footer](../../includes/app-insights-analytics-footer.md)]

## <a name="next-steps"></a>Nächste Schritte
* Es wird empfohlen, mit der [Einführung in die Abfragesprache](app-insights-analytics-tour.md)zu beginnen.





<!--HONumber=Nov16_HO4-->


