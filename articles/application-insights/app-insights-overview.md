---
title: Was ist Azure Application Insights? | Microsoft Docs
description: Anwendungsleistungsverwaltung und Nachverfolgen der Nutzung Ihrer aktiven Webanwendung.  Erkennung, Triage und Diagnose von Problemen, Analyse der App-Nutzung.
services: application-insights
documentationcenter: 
author: alancameronwills
manager: carmonm
ms.assetid: 379721d1-0f82-445a-b416-45b94cb969ec
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: get-started-article
ms.date: 02/07/2017
ms.author: awills
translationtype: Human Translation
ms.sourcegitcommit: 785d3a8920d48e11e80048665e9866f16c514cf7
ms.openlocfilehash: 4c47041bb729a3ee1f3fb4c7baf7f988db226677
ms.lasthandoff: 04/12/2017


---
# <a name="what-is-application-insights"></a>Was ist Application Insights?
Application Insights ist ein erweiterbarer, für Webentwickler konzipierter Dienst zur Verwaltung der Anwendungsleistung (Application Performance Management, APM) auf mehreren Plattformen. Verwenden Sie ihn, um Ihre aktiven Webanwendung zu überwachen. Der Dienst erkennt automatisch Leistungsanomalien. Er verfügt über leistungsstarke Analysetools, mit denen Sie Probleme diagnostizieren und nachvollziehen können, wie Ihre App von den Benutzern verwendet wird.  Der Dienst unterstützt Sie bei der kontinuierlichen Verbesserung der Leistung und Benutzerfreundlichkeit Ihrer App. Er lässt sich für Apps auf einer Vielzahl von Plattformen einsetzen, wie z.B. .NET, Node.js oder J2EE – lokal gehostet oder in der Cloud. Der Dienst lässt sich in Ihren DevOps-Prozess integrieren und verfügt über Verbindungspunkte mit einer Vielzahl von Entwicklungstools.

![Erstellen Sie ein Diagramm mit der Statistik zur Benutzeraktivität, oder führen Sie eine Detailsuche bei bestimmten Ereignissen aus.](./media/app-insights-overview/00-sample.png)

[Sehen Sie sich die Introanimation an](https://www.youtube.com/watch?v=fX2NtGrh-Y0).

## <a name="how-does-application-insights-work"></a>Funktionsweise von Application Insights
Sie installieren ein kleines Instrumentierungspaket in Ihrer Anwendung und richten eine Application Insights-Ressource im Microsoft Azure-Portal ein. Die Instrumentierung überwacht Ihre App und sendet Telemetriedaten an das Portal. (Die Anwendung kann überall ausgeführt werden, sie muss nicht in Azure gehostet werden.)

Sie können nicht nur die Webdienstanwendung instrumentieren, sondern auch Hintergrundkomponenten und den JavaScript-Code in den Webseiten selbst. 

![Die Application Insights-Instrumentierung in Ihrer App sendet Telemetriedaten an Ihre Application Insights-Ressource.](./media/app-insights-overview/01-scheme.png)


Darüber hinaus können Sie aus Hostumgebungen Telemetriedaten abrufen, wie z.B. Leistungsindikatoren, Azure-Diagnosen oder Docker-Protokolle. Sie können auch Webtests einrichten, die in regelmäßigen Abständen synthetische Anforderungen an den Webdienst senden.

Alle diese Telemetriedatenströme werden in das Azure-Portal integriert, wo Sie auf die Rohdaten leistungsstarke Analysen und Suchtools anwenden können.


### <a name="whats-the-overhead"></a>Wie sieht der Aufwand aus?
Die Auswirkungen auf die Leistung Ihrer App sind sehr gering. Aufrufe zur Nachverfolgung sind nicht blockierend und werden zusammengefasst und in einem separaten Thread gesendet.

## <a name="what-does-application-insights-monitor"></a>Was wird von Application Insights überwacht?

Application Insights ist für Entwicklerteams konzipiert und hilft Ihnen dabei, die Leistung und Verwendung Ihrer App nachzuvollziehen. Der Dienst überwacht:

* **Anforderungsraten, Antwortzeiten und Fehlerraten**: Finden Sie heraus, welche Seiten zu welchen Tageszeiten am häufigsten verwendet werden und wo Ihre Benutzer sind. Stellen Sie fest, welche Seiten die beste Leistung aufweisen. Wenn die Antwortzeiten und Fehlerraten bei mehr Anforderungen ansteigen, haben Sie möglicherweise ein Problem mit den Ressourcen. 
* **Abhängigkeitsraten, Antwortzeiten und Fehlerraten**: Finden Sie heraus, ob Sie von externen Diensten verlangsamt werden.
* **Ausnahmen**: Analysieren Sie die aggregierten Statistiken, oder wählen Sie bestimmte Instanzen aus, und untersuchen Sie die Stapelüberwachung und die zugehörigen Anforderungen. Sowohl die Server- als auch die Browserausnahmen werden gemeldet.
* **Seitenansichten und Ladeleistung**: Von den Browsern der Benutzer gemeldet.
* **AJAX-Aufrufe** von Webseiten: Raten, Antwortzeiten und Fehlerraten.
* **Anzahl von Benutzern und Sitzungen**.
* **Leistungsindikatoren** von Ihren Windows- oder Linux-Servercomputern, z.B. CPU, Arbeitsspeicher und Netzwerkverwendung. 
* **Hostdiagnose** von Docker oder Azure. 
* **Diagnose-Ablaufverfolgungsprotokolle** aus Ihrer App, sodass Sie Ablaufverfolgungsereignisse mit Anforderungen korrelieren können.
* **Benutzerdefinierte Ereignisse und Metriken**, die Sie selbst im Client- oder Servercode schreiben, um Geschäftsereignisse zu verfolgen, wie z.B. verkaufte oder gewonnene Spiele.

## <a name="where-do-i-see-my-telemetry"></a>Wo finde ich meine Telemetriedaten?

Es gibt zahlreiche Möglichkeiten, Ihre Daten zu untersuchen. Informationen finden Sie in den folgenden Artikeln:

|  |  |
| --- | --- |
| [**Anwendungszuordnung**](app-insights-app-map.md)<br/>Die Komponenten der App mit wichtigen Metriken und Warnungen. |![Anwendungszuordnung](./media/app-insights-overview/appmap-tn.png)  |
| [**Diagnosesuche für Instanzdaten**](app-insights-diagnostic-search.md)<br/>Suchen und filtern Sie Ereignisse wie Anforderungen, Ausnahmen, Abhängigkeitsaufrufe, Protokollablaufverfolgungen und Seitenaufrufe.  |![Suchen von Telemetriedaten](./media/app-insights-overview/search-tn.png) |
| [**Metrik-Explorer für aggregierte Daten**](app-insights-metrics-explorer.md)<br/>Durchsuchen, filtern und segmentieren Sie aggregierte Daten wie z.B. Anforderungs-, Fehler- und Ausnahmeraten, Antwortzeiten und Seitenladezeiten. |![Metriken](./media/app-insights-overview/metrics-tn.png) |
| [**Dashboards**](app-insights-dashboards.md#dashboards)<br/>Kombinieren Sie Daten aus mehreren Ressourcen, und geben Sie sie für andere frei. Dies ist sehr gut für Anwendungen mit mehreren Komponenten und für die kontinuierliche Anzeige im Teamraum geeignet. |![Beispiel für Dashboards](./media/app-insights-overview/dashboard-tn.png) |
| [**Live Metrics Stream**](app-insights-live-stream.md)<br/>Wenn Sie einen neuen Build bereitstellen, sehen Sie sich diese beinahe in Echtzeit verfügbaren Leistungsindikatoren an, um sicherzustellen, dass alles wie erwartet funktioniert. |![Beispiel für Livemetriken](./media/app-insights-overview/live-metrics-tn.png) |
| [**Analyse**](app-insights-analytics.md)<br/>Beantworten Sie schwierige Fragen zur Leistung und Nutzung Ihrer App mithilfe dieser leistungsstarken Abfragesprache. |![Beispiel für Analysen](./media/app-insights-overview/analytics-tn.png) |
| [**Automatische und manuelle Warnungen**](app-insights-alerts.md)<br/>Automatische Warnungen sind an die normalen Telemetriemuster Ihrer App angepasst und werden ausgelöst, wenn etwas nicht den üblichen Mustern entspricht. Sie können auch Warnungen auf bestimmten Ebenen von benutzerdefinierten oder standardmäßigen Metriken festlegen. |![Beispiel für Warnungen](./media/app-insights-overview/alerts-tn.png) |
| [**Visual Studio**](app-insights-visual-studio.md)<br/>Zeigen Sie Leistungsdaten im Code an. Wechseln Sie von Stapelüberwachungen zum Code.|![Visual Studio](./media/app-insights-overview/visual-studio-tn.png) |
| [**Power BI**](app-insights-export-power-bi.md)<br/>Integrieren Sie Nutzungsmetriken und andere Business Intelligence-Daten.| ![Power BI](./media/app-insights-overview/power-bi.png)|
| [**REST-API**](https://dev.applicationinsights.io/)<br/>Schreiben Sie Code zum Ausführen von Abfragen für Ihre Metriken und Rohdaten.| ![REST-API](./media/app-insights-overview/rest-tn.png) |
| [**Fortlaufender Export**](app-insights-export-telemetry.md)<br/>Exportieren Sie große Mengen von Rohdaten in den Speicher, sobald sie eintreffen. |![Export](./media/app-insights-overview/export-tn.png) |

## <a name="how-do-i-use-application-insights"></a>Wie verwende ich Application Insights?

### <a name="monitor"></a>Überwachen
Installieren Sie Application Insights in Ihrer App, richten Sie [Webtests zur Verfügbarkeit](app-insights-monitor-web-app-availability.md) ein, und gehen Sie dann wie folgt vor:

* Richten Sie ein [Dashboard](app-insights-dashboards.md) für Ihren Teamraum ein, damit Aspekte wie die Auslastung, Reaktionsfähigkeit und Leistung Ihrer Abhängigkeiten, Seitenladevorgänge und AJAX-Aufrufe immer im Blick behalten werden können.
* Ermitteln Sie, welche Anforderungen am langsamsten sind und am häufigsten zu Ausfällen führen.
* Verfolgen Sie den [Livestream](app-insights-live-stream.md), wenn Sie eine neue Version bereitstellen, damit Sie über Leistungsabfälle sofort informiert sind.

### <a name="detect-diagnose"></a>Erkennen, Diagnostizieren
Gehen Sie wie folgt vor, wenn Sie eine Warnung erhalten oder ein Problem auftritt:

* Bewerten Sie, wie viele Benutzer betroffen sind.
* Korrelieren Sie Ausfälle mit Ausnahmen, Abhängigkeitsaufrufen und Nachverfolgungen.
* Untersuchen Sie Stapelabbilder und Ablaufverfolgungsprotokolle.

### <a name="build-measure-learn"></a>Erstellen, Messen, Lernen
Messen Sie die Effektivität jeder neuen Funktion, die Sie bereitstellen.

* Planen Sie eine Messung, um zu ermitteln, wie Kunden neue Funktionen der Benutzeroberfläche bzw. des Geschäftsablaufs verwenden.
* Schreiben Sie benutzerdefinierte Telemetriedaten in Ihren Code.
* Sorgen Sie dafür, dass der nächste Entwicklungszyklus auf belastbaren Informationen aus Ihren Telemetriedaten basiert.

## <a name="get-started"></a>Erste Schritte
Application Insights ist einer der vielen in Microsoft Azure gehosteten Dienste, und Telemetriedaten werden zur Analyse und Darstellung an Azure gesendet. Als Erstes benötigen Sie also ein Abonnement für [Microsoft Azure](http://azure.com). Die Registrierung ist kostenlos. Wenn Sie den [Basistarif](https://azure.microsoft.com/pricing/details/application-insights/) von Application Insights wählen, fallen Gebühren erst an, sobald Ihre Anwendung umfassender genutzt wird. Falls Ihre Organisation bereits über ein Abonnement verfügt, kann sie diesem Ihr Microsoft-Konto hinzufügen.

Es gibt mehrere Möglichkeiten für den Einstieg. Wählen Sie die Methode aus, die sich am besten für Sie eignet. Die anderen können später hinzugefügt werden.

* **Zur Laufzeit: Instrumentieren Sie Ihre Web-App auf dem Server.** Bei dieser Vorgehensweise sind keine Codeaktualisierungen erforderlich. Sie benötigen Administratorzugriff für Ihren Server.
  * [**IIS (lokal oder auf einem virtuellen Computer)**](app-insights-monitor-performance-live-website-now.md)
  * [**Azure-Web-App oder -VM**](app-insights-monitor-performance-live-website-now.md)
  * [**J2EE**](app-insights-java-live.md)
* **Bei der Entwicklung: Fügen Sie Ihrem Code Application Insights hinzu.** Ermöglicht das Schreiben von benutzerdefinierter Telemetrie sowie das Instrumentieren von Back-End- und Desktop-Apps.
  * [Visual Studio](app-insights-asp-net.md) 2013, Update 2 oder höher.
  * Java in [Eclipse](app-insights-java-eclipse.md) oder [anderen Tools](app-insights-java-get-started.md)
  * [Node.js](app-insights-nodejs.md)
  * [Andere Plattformen](app-insights-platforms.md)
* **[Instrumentieren Sie Ihre Webseiten](app-insights-javascript.md)** für Seitenansicht, AJAX und andere clientseitige Telemetrie.
* **[Verfügbarkeitstests](app-insights-monitor-web-app-availability.md)** : Pingen Sie Ihre Website regelmäßig über unsere Server an.


## <a name="next-steps"></a>Nächste Schritte
Beginnen mit der Laufzeitmethode mit:

* [IIS-Server](app-insights-monitor-performance-live-website-now.md)
* [J2EE-Server](app-insights-java-live.md)

Beginnen mit der Entwicklungszeitmethode mit:

* [ASP.NET](app-insights-asp-net.md)
* [Java](app-insights-java-get-started.md)
* [Node.js](app-insights-nodejs.md)

## <a name="support-and-feedback"></a>Support und Feedback
* Fragen und Probleme:
  * [Behandeln von Problemen][qna]
  * [MSDN-Forum](https://social.msdn.microsoft.com/Forums/vstudio/home?forum=ApplicationInsights)
  * [StackOverflow](http://stackoverflow.com/questions/tagged/ms-application-insights)
* Ihre Vorschläge:
  * [UserVoice](https://visualstudio.uservoice.com/forums/357324)
* Blog:
  * [Application Insights-Blog](https://azure.microsoft.com/blog/tag/application-insights)

## <a name="videos"></a>Videos

[![Animierte Einführung](./media/app-insights-overview/video-front-1.png)](https://www.youtube.com/watch?v=fX2NtGrh-Y0)

> [!VIDEO https://channel9.msdn.com/events/Connect/2016/100/player] 

<!--Link references-->

[android]: https://github.com/Microsoft/ApplicationInsights-Android
[azure]: ../insights-perf-analytics.md
[client]: app-insights-javascript.md
[desktop]: app-insights-windows-desktop.md
[detect]: app-insights-detect-triage-diagnose.md
[greenbrown]: app-insights-asp-net.md
[ios]: https://github.com/Microsoft/ApplicationInsights-iOS
[java]: app-insights-java-get-started.md
[knowUsers]: app-insights-overview-usage.md
[platforms]: app-insights-platforms.md
[portal]: http://portal.azure.com/
[qna]: app-insights-troubleshoot-faq.md
[redfield]: app-insights-monitor-performance-live-website-now.md

