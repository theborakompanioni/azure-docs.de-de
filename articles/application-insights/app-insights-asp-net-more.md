<properties 
	pageTitle="Optimale Nutzung von Application Insights" 
	description="Nachdem Sie die ersten Schritte mit Application Insights gemacht haben, finden Sie hier eine Zusammenfassung der Funktionen, mit denen Sie sich beschäftigen können." 
	services="application-insights" 
    documentationCenter=".net"
	authors="alancameronwills" 
	manager="douge"/>

<tags 
	ms.service="application-insights" 
	ms.workload="tbd" 
	ms.tgt_pltfrm="ibiza" 
	ms.devlang="na" 
	ms.topic="article" 
	ms.date="06/02/2016" 
	ms.author="awills"/>

# Weitere Telemetriedaten aus Application Insights

Im Folgenden finden Sie eine Zusammenfassung der Funktionen, die Sie möglicherweise in [Visual Studio Application Insights](app-insights-overview.md) noch nicht ausprobiert haben. Wir gehen davon aus, dass Sie bereits [die ersten Schritte ausgeführt haben](app-insights-asp-net.md). Mit Application Insights können Sie Ihre Webanwendung auf Verfügbarkeit, Leistung und Nutzung überwachen. Mit dem Feedback zur Leistung und Effektivität der App in der Praxis können Sie in jedem Entwicklungslebenszyklus eine fundierte Entscheidung für die Richtung des Entwurfs treffen.

## Mehr Telemetrie

||
|---|---
|[**Verfügbarkeitstests**](app-insights-monitor-web-app-availability.md)<br/>Senden Sie der Web-App in regelmäßigen Abständen HTTP-Anforderungen aus aller Welt. Wir werden Sie benachrichtigen, wenn die Reaktion langsam oder unzuverlässig ist.| 
|[**Abhängigkeit und AJAX-Aufrufe**](app-insights-asp-net-dependencies.md)<br/>Überwachen Sie SQL-Abfragen und Aufrufe von REST oder anderen Ressourcen.|
|[**Ausnahmen**](app-insights-asp-net-exceptions.md)<br/>Zählen Sie behandelte und nicht behandelte Ausnahmen, rufen Sie Stapelüberwachungen ab, und klicken Sie sich durch den Code.|
|[**Webseiten**](app-insights-javascript.md)<br/>Überwachen Sie die Seitennutzung und die Leistung durch Instrumentieren der Webseiten.
|**Hostleistung: [Azure-Diagnose](app-insights-azure-diagnostics.md), [Windows-Leistungsindikatoren](app-insights-web-monitor-performance.md)**<br/>Sehen Sie sich die Metriken zur CPU-Auslastung und andere Kontextmetriken an. |![](./media/app-insights-asp-net-more/04.png)
|[**SDK-API – benutzerdefinierte Telemetriedaten**](app-insights-api-custom-events-metrics.md)<br/>Senden Sie Ihre eigenen Ereignisse und Metriken für eine detailliertere Ansicht der Leistung und Nutzung Ihrer App im Server- und Clientcode.|
|[**Protokollintegration**](app-insights-asp-net-trace-logs.md)<br/>Wenn Sie ein Protokollierungsframework wie Log4Net, NLog oder System.Diagnostics.Trace verwenden, gibt es einen Adapter, der diese Ablaufverfolgungen zusammen mit anderen Telemetriedaten an Application Insights sendet.|
|[**Telemetrieprozessoren**](app-insights-api-filtering-sampling.md)<br/>Filtern, ändern oder erweitern Sie die Telemetriedaten, die vom SDK in Ihrer App gesendet werden. |


## Leistungsfähige Analysen und Präsentationen

||
|---|---
|[**Diagnosesuche nach Instanzdaten**](app-insights-visual-studio.md)<br/>Suchen und filtern Sie Ereignisse wie Anforderungen, Ausnahmen, Abhängigkeitsaufrufe, Protokollablaufverfolgungen und Seitenaufrufe. Wechseln Sie in Visual Studio von Stapelüberwachungen zum Code.|![Visual Studio](./media/app-insights-asp-net/61.png)
|[**Metrik-Explorer für aggregierte Daten**](app-insights-metrics-explorer.md)<br/>Durchsuchen, filtern und segmentieren Sie aggregierte Daten wie z.B. Raten von Anforderungen, Fehlern und Ausnahmen, Antwortzeiten, Seitenladezeiten.|![Visual Studio](./media/app-insights-asp-net-more/060.png)
|[**Dashboards**](app-insights-dashboards.md#dashboards)<br/>Kombinieren Sie Daten aus mehreren Ressourcen, und geben Sie sie für andere frei. Dies ist sehr gut für Anwendungen mit mehreren Komponenten und für die kontinuierliche Anzeige im Teamraum geeignet. |![Beispiel für Dashboards](./media/app-insights-asp-net/62.png)
|[**Livedatenstrom**](app-insights-metrics-explorer.md#live-stream)<br/>Wenn Sie einen neuen Build bereitstellen, sehen Sie sich diese beinahe in Echtzeit verfügbaren Leistungsindikatoren an, um sicherzustellen, dass alles wie erwartet funktioniert.|![Beispiel für Analysen](./media/app-insights-asp-net-more/050.png)
|[**Analytics**](app-insights-analytics.md)<br/>Beantworten Sie schwierige Fragen zur Leistung und Nutzung Ihrer App mithilfe dieser leistungsstarken Abfragesprache.|![Beispiel für Analysen](./media/app-insights-asp-net-more/010.png)
|[**Automatische und manuelle Warnungen**](app-insights-alerts.md)<br/>Automatische Warnungen sind an die normalen Telemetriemuster Ihrer App angepasst und werden ausgelöst, wenn etwas nicht den üblichen Mustern entspricht. Sie können auch Warnungen auf bestimmten Ebenen von benutzerdefinierten oder standardmäßigen Metriken festlegen.|![Beispiel für Warnungen](./media/app-insights-asp-net-more/020.png)

## Datenverwaltung

|||
|---|---|
|[**Fortlaufender Export**](app-insights-export-telemetry.md)<br/>Kopieren Sie alle Telemetriedaten in den Speicher, sodass Sie sie auf Ihre eigene Weise analysieren können.|
|**API für den Datenzugriff**<br/>In Kürze verfügbar.|
|[**Stichprobenerstellung**](app-insights-sampling.md)<br/>Reduziert die Datenrate und hilft Ihnen dabei, innerhalb der Grenzwerte Ihres Tarifs zu bleiben.|![Kachel für Stichproben](./media/app-insights-asp-net-more/030.png)

<!---HONumber=AcomDC_0608_2016-->