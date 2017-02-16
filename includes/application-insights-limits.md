Es gibt einige Beschränkungen hinsichtlich der Anzahl von Metriken und Ereignissen pro Anwendung (d. h. pro Instrumentationsschlüssel). 

Die Beschränkungen hängen von dem von Ihnen ausgewählten [Tarif](https://azure.microsoft.com/pricing/details/application-insights/) ab.

| **Ressource** | **Standardlimit** | **Hinweis**
| --- | --- | --- |
| Gesamtdaten pro Tag | 500 GB | Diese können Sie durch Festlegen einer Obergrenze reduzieren. Wenn Sie mehr benötigen, wenden Sie sich per E-Mail an AIDataCap@microsoft.com 
| Kostenlose Daten pro Monat<br/> (Plan der Grundpreise) | 1 GB | Zusätzliche Daten, die pro GB abgerechnet werden
| Drosselung | 16.000 Ereignisse/Sekunde | Messdauer: Eine Minute. 
| Beibehaltung von Daten | 90 Tage | für die [Suche](../articles/application-insights/app-insights-diagnostic-search.md), die [Analyse](../articles/application-insights/app-insights-analytics.md) und [Metriken-Explorer](../articles/application-insights/app-insights-metrics-explorer.md).
| [Webtests in mehreren Schritten](../articles/application-insights/app-insights-monitor-web-app-availability.md#multi-step-web-tests) mit detaillierter Ergebnisaufbewahrung | 90 Tage | Detaillierte Ergebnisse der einzelnen Schritte
| [Eigenschaft](../articles/application-insights/app-insights-api-custom-events-metrics.md#properties) und Anzahl der Namen von [Metriken](../articles/application-insights/app-insights-api-custom-events-metrics.md#properties)<sup>2</sup> | 200 | 
| Eingenschaft und Länge der Namen von Metriken | 150 |
| Zeichenfolgenlänge des Eigenschaftswerts | 8192 |
| Unterschiedliche Werte für Eigenschaften<sup>3,4</sup> | 100 | > 100 => eine Eigenschaft kann nicht als Filter im Metrik-Explorer verwendet werden
| Länge von Ablaufverfolgungs- und Ausnahmebenachrichtigungen | 10000 |
| [Verfügbarkeitstests](../articles/application-insights/app-insights-monitor-web-app-availability.md) Anzahl pro App  | 10 |

1. All diese Zahlen gelten pro Instrumentierungsschlüssel.
2. Metriknamen werden jeweils im TrackMetric- und Messungsparameter anderer Track*()-Aufrufe definiert. Metriknamen sind pro Instrumentierungsschlüssel global.
3. Eigenschaften können nur zur Filterung und zur Gruppierung verwendet werden, solange sie weniger als 100 eindeutige Werte für jede Eigenschaft aufweisen. Sind mehr als 100 eindeutige Werte vorhanden, können Sie die Eigenschaft zwar weiterhin für Suchvorgänge, aber nicht mehr zum Filtern oder Gruppieren verwenden.
4. Standardeigenschaften wie z. B. RequestName und die Seiten-URL, sind auf 1000 eindeutige Werte pro Woche beschränkt. Nach 1000 eindeutigen Werten werden zusätzliche Werte als „Andere Werte“ gekennzeichnet. Die ursprünglichen Werte können nach wie vor für die Volltextsuche und die Filterung verwendet werden.


[Informationen zu Preisen und Kontingenten für Application Insights](../articles/application-insights/app-insights-pricing.md)

<!--HONumber=Jan17_HO3-->


