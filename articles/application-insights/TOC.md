# Übersicht
## [Was ist Application Insights?](app-insights-overview.md)
## [So funktioniert's](app-insights-detect-triage-diagnose.md)

# Erste Schritte
## Überwachen von Azure
### [Azure-Web-Apps](app-insights-azure-web-apps.md)
### [Azure Cloud Services](app-insights-cloudservices.md)

## Überwachen von ASP.NET-Apps
### [Web-Apps](app-insights-asp-net.md)
### [Web-Apps, die bereits live sind](app-insights-monitor-performance-live-website-now.md)
### [Windows-Dienste](app-insights-windows-services.md)
### [Windows Desktop](app-insights-windows-desktop.md)

## Überwachen von Java-Apps
### [Web-Apps](app-insights-java-get-started.md)
### [Web-Apps – Laufzeit](app-insights-java-live.md)
### [Docker-Apps](app-insights-docker.md)

## Überwachen von Webseiten
### [JavaScript](app-insights-javascript.md)

## Überwachen von anderen Plattformen
### [Node.js-Apps](app-insights-nodejs.md)
### [SharePoint-Websites](app-insights-sharepoint.md)
### [Weitere Plattformen](app-insights-platforms.md)

## [Häufig gestellte Fragen zu ASP.NET](app-insights-troubleshoot-faq.md)

# Anleitung
## Planen und Entwerfen

### [Eingehende Diagnose für Web-Apps und Dienste](app-insights-devops.md)
### [Developer Analytics mit Application Insights und HockeyApp](app-insights-developer-analytics.md)
### [Leistungsüberwachung in Webanwendungen](app-insights-web-monitor-performance.md)
### [Verwendungsanalyse mit Application Insights](app-insights-overview-usage.md)
### [Trennen von Application Insights-Ressourcen](app-insights-separate-resources.md)
### [Wie führe ich in Application Insights ... aus?](app-insights-how-do-i.md)

## Konfigurieren

### Azure
#### [Diagnose](app-insights-azure-diagnostics.md)

### ASP.NET
#### [Erweitern der Telemetriedaten](app-insights-asp-net-more.md)
#### [Exceptions](app-insights-asp-net-exceptions.md)
#### [Protokollablaufverfolgungen](app-insights-asp-net-trace-logs.md)
#### [Leistungsindikatoren](app-insights-performance-counters.md)
#### [Abhängigkeiten](app-insights-asp-net-dependencies.md)
#### [Versionsanmerkungen](app-insights-annotations.md)


### J2EE
#### [Protokollablaufverfolgungen](app-insights-java-trace-logs.md)
#### [UNIX-Metriken](app-insights-java-collectd.md)
#### [Abhängigkeiten](app-insights-java-agent.md)

### Warnungen

#### [Availability](app-insights-monitor-web-app-availability.md)
#### [Metrikwarnungen](app-insights-alerts.md)

### [Intelligente Erkennung](app-insights-proactive-diagnostics.md)
#### [Ausfallerkennung](app-insights-proactive-failure-diagnostics.md)
#### [Anomalieerkennung](app-insights-proactive-performance-diagnostics.md)

## Analysieren

### Application Insights-Portal

#### [Dashboards](app-insights-dashboards.md)
#### [Suchen,](app-insights-diagnostic-search.md)
#### [Metriken](app-insights-metrics-explorer.md)
#### Analyse

##### [Analyse](app-insights-analytics.md)
##### [Überblick über Analytics](app-insights-analytics-tour.md)
##### [Verwenden von Analytics](app-insights-analytics-using.md)

#### [Anwendungszuordnung](app-insights-app-map.md)
#### [HockeyApp-Daten](app-insights-hockeyapp-bridge-app.md)
#### [Erstellen einer Ressource](app-insights-create-new-resource.md)

### Visual Studio

#### [Erkenntnisse mit F5](app-insights-visual-studio.md)
#### [Trends](app-insights-visual-studio-trends.md)
#### [CodeLens](app-insights-visual-studio-codelens.md)

## Automatisieren

### [PowerShell-Konfiguration](app-insights-powershell.md)
### [Erstellen von Ressourcen](app-insights-powershell-script-create-resource.md)
### [Festlegen von Benachrichtigungen](app-insights-powershell-alerts.md)
### [Holen Sie sich Azure-Diagnose](app-insights-powershell-azure-diagnostics.md)


## Integrieren

### [Fortlaufendem Export](app-insights-export-telemetry.md)
### [Exportieren nach Power BI](app-insights-export-power-bi.md)


## Entwickeln

### [API für benutzerdefinierte Ereignisse und Metriken](app-insights-api-custom-events-metrics.md)
### [Filterung und Vorverarbeitung von Telemetriedaten](app-insights-api-filtering-sampling.md)
### [ASP.NET Core](app-insights-asp-net-core.md)


## Verwalten

### [Verwalten von Preisen und Kontingenten](app-insights-pricing.md)
### [Überwachung der Anwendungsleistung mithilfe von Application Insights für SCOM](app-insights-scom.md)


## Schützen

### [Datensammlung, -vermerkdauer und -speicherung](app-insights-data-retention-privacy.md)
### [Ressourcen, Rollen und Zugriffssteuerung](app-insights-resources-roles-access-control.md)


## Problembehandlung
### [Keine Daten für .NET](app-insights-asp-net-troubleshoot-no-data.md)
### [Analyse](app-insights-analytics-troubleshooting.md)
### [Java](app-insights-java-troubleshoot.md)

# Referenz
## [Java](http://dl.windowsazure.com/applicationinsights/javadoc/)
## [.NET API](https://docs.microsoft.com/dotnet/api)
## [JavaScript-API](https://github.com/Microsoft/ApplicationInsights-JS/blob/master/API-reference.md)
## [Analyse](app-insights-analytics-reference.md)
## [Stichproben](app-insights-sampling.md)
## [IP-Adressen](app-insights-ip-addresses.md)
## [ApplicationInsights.config](app-insights-configuration-with-applicationinsights-config.md)
## [Exportdatenmodell](app-insights-export-data-model.md)
## [Migrieren von Azure-Endpunktüberwachung zu Verfügbarkeitstests](app-insights-migrate-azure-endpoint-tests.md)
## [Entwickleranalysen: Sprachen, Plattformen und Integrationsmöglichkeiten](app-insights-platforms.md)
### [Beispiele und exemplarische Vorgehensweisen](app-insights-code-samples.md)
#### [Exemplarische Vorgehensweise: Aktivieren von Telemetriedaten für Microsoft Dynamics CRM Online](app-insights-sample-mscrm.md)
#### [Exemplarische Vorgehensweise: Exportieren in SQL mit Stream Analytics](app-insights-code-sample-export-sql-stream-analytics.md)
#### [Codebeispiel: Analysieren exportierter Daten](app-insights-code-sample-export-telemetry-sql-database.md)
## [Anmerkungen zu dieser Version des Application Insights SDK für Windows Phone und Store](app-insights-release-notes-windows.md)
## [Anmerkungen zur Version von Developer Analytics Tools](app-insights-release-notes-vsix.md)
## [Anmerkungen zu dieser Version des Application Insights SDKs](app-insights-release-notes.md)
## [REST-API](https://dev.applicationinsights.io/)

# Ressourcen
## [Preise](https://azure.microsoft.com/pricing/details/application-insights/)  
## [MSDN-Forum](https://social.msdn.microsoft.com/Forums/azure/en-US/home?forum=ApplicationInsights)  
## [Stack Overflow](http://stackoverflow.com/questions/tagged/az-application-insights)
## [Videos](https://azure.microsoft.com/documentation/videos/index/?services=application-insights) 
## [Dienstupdates](https://azure.microsoft.com/en-us/updates/?product=application-insights) 
## [Unterstützung](app-insights-get-dev-support.md)



<!--HONumber=Nov16_HO2-->


