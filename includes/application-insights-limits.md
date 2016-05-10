Es gibt einige Beschränkungen hinsichtlich der Anzahl von Metriken und Ereignissen pro Anwendung (d. h. pro Instrumentationsschlüssel).

Die Beschränkungen hängen von dem von Ihnen ausgewählten [Tarif](https://azure.microsoft.com/pricing/details/application-insights/) ab.

**Ressource** | **Standardlimit** | **Maximales Limit**
-------- | ------------- | -------------
Sitzungsdatenpunkte<sup>1</sup> pro Monat | Unbegrenzt | 
Andere Datenpunkte pro Monat | 5 Mio. | 50 Mio.<sup>2</sup>
Datenrate für [Ablaufverfolgung oder Protokolle](../articles/application-insights/app-insights-search-diagnostic-logs.md) | 200 DP/s | 500 DP/s
Datenrate für [Ausnahmen](../articles/application-insights/app-insights-asp-net-exceptions.md) | 50 DP/s | 50 DP/s
Sonstige Telemetriedatenrate | 200 DP/s | 500 DP/s
Aufbewahrung von [Rohdaten](../articles/application-insights/app-insights-diagnostic-search.md) | 7 Tage
Aufbewahrung von [aggregierten Daten](../articles/application-insights/app-insights-metrics-explorer.md) | 90 Tage
Anzahl der Namen von [Eigenschaften](../articles/application-insights/app-insights-api-custom-events-metrics.md#properties) | 100 |
Länge von Eigenschaftennamen | 100 | 
Länge von Eigenschaftswerten | 1000 | 
Länge von Ablaufverfolgungs- und Ausnahmebenachrichtigungen | 10000 |
Anzahl der Namen von [Metriken](../articles/application-insights/app-insights-api-custom-events-metrics.md#properties) | 100 |
Länge von Metriknamen | 100 | 
[Verfügbarkeitstests](../articles/application-insights/app-insights-monitor-web-app-availability.md) | 10 | 

<sup>1</sup> Ein Datenpunkt ist ein einzelner Metrikwert oder ein einzelnes Ereignis mit den zugehörigen Eigenschaften und Messwerten.

<sup>2</sup> Sie können zusätzliche Kapazitäten über 50 Mio. erwerben.
 
[Informationen zu Preisen und Kontingenten für Application Insights](../articles/application-insights/app-insights-pricing.md)

<!---HONumber=AcomDC_0427_2016-->