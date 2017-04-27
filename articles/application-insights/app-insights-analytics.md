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
ms.date: 03/14/2017
ms.author: awills
translationtype: Human Translation
ms.sourcegitcommit: 785d3a8920d48e11e80048665e9866f16c514cf7
ms.openlocfilehash: 717269a2c0e0b1a3d332e627154eacd2d2c5001e
ms.lasthandoff: 04/12/2017


---
# <a name="analytics-in-application-insights"></a>Analytics in Application Insights
[Analytics](app-insights-analytics.md) ist die leistungsfähige Suchfunktion von [Application Insights](app-insights-overview.md). Auf diesen Seiten wird die Analytics-Abfragesprache beschrieben. 

* **[Sehen Sie sich das Einführungsvideo an.](https://applicationanalytics-media.azureedge.net/home_page_video.mp4)**
* **[Testen Sie Analytics mit unseren simulierten Daten](https://analytics.applicationinsights.io/demo)**, wenn Ihre App noch keine Daten an Application Insights sendet.
* In der **[Kurzübersicht für SQL-Benutzer](https://aka.ms/sql-analytics)** finden Sie eine Übersetzung der gängigsten Sprachen.
* **[Sprachreferenz](app-insights-analytics-reference.md)** Erfahren Sie, wie die leistungsstarken Funktionen der Analytics-Abfragesprache verwenden.


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


## <a name="video"></a>Video

> [!VIDEO https://channel9.msdn.com/events/Connect/2016/123/player] 


[!INCLUDE [app-insights-analytics-footer](../../includes/app-insights-analytics-footer.md)]


## <a name="next-steps"></a>Nächste Schritte
* Es wird empfohlen, mit der [Einführung in die Abfragesprache](app-insights-analytics-tour.md)zu beginnen. 

### <a name="query-examples"></a>Abfragebeispiele

* Arbeiten Sie diese exemplarischen Vorgehensweisen durch, die die Vorteile der Verwendung von Analytics veranschaulichen:
 1.    [Automatic diagnostics of spikes and step jumps in requests durations](https://analytics.applicationinsights.io/demo#/discover/query/results/chart?title=Automatic%20diagnostics%20of%20sudden%20spikes%20or%20step%20jumps%20in%20requests%20duration&shared=true) (Automatische Diagnose von Spitzen und Schrittsprüngen in der Anfragedauer)
 2.    [Analyzing performance degradations with time series analysis](https://analytics.applicationinsights.io/demo#/discover/query/main?title=Analyzing%20performance%20degradations%20with%20time%20series%20analysis&shared=true) (Analysieren der Leistungsbeeinträchtigung mithilfe der Zeitreihenanalyse)
 3.    [Analyzing application failures with autocluster and diffpatterns](https://analytics.applicationinsights.io/demo#/discover/query/main?title=Analyzing%20application%20failures%20with%20autocluster%20and%20diffpatterns&shared=true) (Analysieren von Anwendungsausfällen mit automatischem Clustering und Differenzmustern)
 4.    [Advanced shape detections with time series analysis](https://analytics.applicationinsights.io/demo#/discover/query/main?title=Advanced%20shape%20detection%20with%20time%20series%20analysis&shared=true) (Erkennung erweiterter Formen mithilfe der Zeitreihenanalyse)
 5.    [Using sliding window operations to analyze application usage (rolling MAU/DAU etc)](https://analytics.applicationinsights.io/demo#/discover/query/main?title=Using%20sliding%20window%20calculations%20to%20analyze%20usage%20metrics:%20rolling%20MAU~2FDAU%20and%20cohorts&shared=true) (Analysieren der Anwendungsverwendung (gleitendes MAU/DAU) mithilfe von Schiebefenstervorgängen)
 6.    [Detection of service disruptions based on analysis of debug logs](https://analytics.applicationinsights.io/demo#/discover/query/main?title=Detection%20of%20service%20disruptions%20based%20on%20regression%20analysis%20of%20trace%20logs&shared=true) (Erkennen von Dienstunterbrechungen durch Analyse von Debugprotokollen); ein entsprechender Blogbeitrag findet sich [hier](https://maximshklar.wordpress.com/2017/02/16/finding-trends-in-traces-with-smart-data-analytics).
 7.    [Profiling applications’ performance using simple debug logs](https://analytics.applicationinsights.io/demo#/discover/query/main?title=Profiling%20applications'%20performance%20with%20simple%20debug%20logs&shared=true) (Erstellen von Leistungsprofilen für Anwendungen mithilfe einfacher Debugprotokolle); ein entsprechender Blogbeitrag findet sich [hier](https://yossiattasblog.wordpress.com/2017/03/13/first-blog-post/)
 8.    [Measuring the duration for each step in your code flow using simple debug logs](https://analytics.applicationinsights.io/demo#/discover/query/main?title=Measuring%20the%20duration%20of%20each%20step%20in%20your%20code%20flow%20using%20simple%20debug%20logs&shared=true) (Messen der Dauer der einzelnen Schritte im Codefluss mithilfe einfacher Debugprotokolle); ein entsprechender Blogbeitrag findet sich [hier](https://yossiattasblog.wordpress.com/2017/03/14/measuring-the-duration-of-each-step-in-your-code-flow-using-simple-debug-logs/)
 9.    [Analyzing concurrency using simple debug logs](https://analytics.applicationinsights.io/demo#/discover/query/results/chart?title=Analyzing%20concurrency%20with%20simple%20debug%20logs&shared=true) (Analysieren der Parallelität mithilfe einfacher Debugprotokolle); ein entsprechender Blogbeitrag findet sich [hier](https://yossiattasblog.wordpress.com/2017/03/23/analyzing-concurrency-using-simple-debug-logs/)



