Es gibt einige Beschränkungen hinsichtlich der Anzahl von Metriken und Ereignissen pro Anwendung (d. h. pro Instrumentationsschlüssel). Die Beschränkungen hängen von dem von Ihnen ausgewählten [Tarif](https://azure.microsoft.com/pricing/details/application-insights/) ab.

| **Ressource** | **Standardlimit** | **Hinweis**
| --- | --- | --- |
| Gesamtdaten pro Tag | 500 GB | Die Datenmenge kann durch Festlegen einer Obergrenze reduziert werden. Sollten Sie mehr benötigen, wenden Sie sich per E-Mail an AIDataCap@microsoft.com.
| Kostenlose Daten pro Monat<br/> (Plan der Grundpreise) | 1 GB | Zusätzliche Daten werden pro GB abgerechnet.
| Drosselung | 32.000 Ereignisse/Sekunde | Das Limit wird eine Minute lang gemessen.
| Beibehaltung von Daten | 90 Tage | Hierbei handelt es sich um eine Ressource für [Suche](../articles/application-insights/app-insights-diagnostic-search.md), [Analyse](../articles/application-insights/app-insights-analytics.md) und [Metrik-Explorer](../articles/application-insights/app-insights-metrics-explorer.md).
| [Webtests in mehreren Schritten](../articles/application-insights/app-insights-monitor-web-app-availability.md#multi-step-web-tests) mit detaillierter Ergebnisaufbewahrung | 90 Tage | Diese Ressource liefert detaillierte Ergebnisse der einzelnen Schritte.
| Maximale Ereignisgröße | 64 K | 
| Eingenschaft und Länge der Namen von Metriken | 150 | Weitere Informationen unten im Kommentar
| Zeichenfolgenlänge des Eigenschaftswerts | 8.192 | Weitere Informationen unten im Kommentar
| Länge von Ablaufverfolgungs- und Ausnahmebenachrichtigungen | 10.000 | Weitere Informationen unten im Kommentar
| [Verfügbarkeitstests](../articles/application-insights/app-insights-monitor-web-app-availability.md) Anzahl pro App  | 10 |

Weitere Informationen zu Preisen und Kontingenten für Application Insights finden Sie [hier](../articles/application-insights/app-insights-pricing.md).

Weitere Informationen zu Grenzwerten für Datenfelder finden Sie unter [Per Type Schemas](https://github.com/Microsoft/ApplicationInsights-Home/blob/master/EndpointSpecs/Schemas/Docs/) (Schemas nach Typ).
