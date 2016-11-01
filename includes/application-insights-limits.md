Es gibt einige Beschränkungen hinsichtlich der Anzahl von Metriken und Ereignissen pro Anwendung (d. h. pro Instrumentationsschlüssel).

Die Beschränkungen hängen von dem von Ihnen ausgewählten [Tarif](https://azure.microsoft.com/pricing/details/application-insights/) ab.

**Ressource** | **Standardlimit** | **Maximales Limit**
-------- | ------------- | -------------
Sitzungsdatenpunkte<sup>1, 2</sup> pro Monat | Unbegrenzt | 
Datenpunkte insgesamt pro Monat für Anforderungen, Ereignisse, Abhängigkeiten, Ablaufverfolgungen, Ausnahmen und Seitenansichten | 5 Mio. | 50 Mio.<sup>3</sup>
Datenrate für [Ablaufverfolgung und Protokolle](../articles/application-insights/app-insights-search-diagnostic-logs.md) | 200 DP/s | 500 DP/s
Datenrate für [Ausnahmen](../articles/application-insights/app-insights-asp-net-exceptions.md) | 50 DP/s | 50 DP/s
Gesamtdatenrate für Anforderungen, Ereignisse, Abhängigkeiten, Seitenansichten und Telemetrie | 200 DP/s | 500 DP/s
Aufbewahrung von Rohdaten für [Search](../articles/application-insights/app-insights-diagnostic-search.md) und [Analytics](../articles/application-insights/app-insights-analytics.md) | 7 Tage
Aufbewahrung von aggregierten Daten für [Metrik-Explorer](../articles/application-insights/app-insights-metrics-explorer.md) | 90 Tage
Anzahl der Namen von [Eigenschaften](../articles/application-insights/app-insights-api-custom-events-metrics.md#properties) | 100 |
Länge von Eigenschaftennamen | 150 | 
Länge von Eigenschaftswerten | 8192 | 
Länge von Ablaufverfolgungs- und Ausnahmebenachrichtigungen | 10000 |
Anzahl der Namen von [Metriken](../articles/application-insights/app-insights-api-custom-events-metrics.md#properties) | 100 |
Länge von Metriknamen | 150 | 
[Verfügbarkeitstests](../articles/application-insights/app-insights-monitor-web-app-availability.md) | 10 | 

<sup>1</sup> Ein Datenpunkt ist ein einzelner Metrikwert oder ein einzelnes Ereignis mit den zugehörigen Eigenschaften und Messwerten.

<sup>2</sup> Ein Sitzungsdatenpunkt protokolliert Anfang oder Ende einer Sitzung sowie die Benutzeridentität.

<sup>3</sup> Sie können zusätzliche Kapazitäten (über 50 Mio. hinaus) erwerben.
 
[Informationen zu Preisen und Kontingenten für Application Insights](../articles/application-insights/app-insights-pricing.md)

<!---HONumber=AcomDC_0803_2016-->