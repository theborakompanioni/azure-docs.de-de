<properties 
	pageTitle="Optimale Nutzung von Application Insights | Microsoft Azure" 
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
	ms.date="08/30/2016" 
	ms.author="awills"/>

# Weitere Telemetriedaten aus Application Insights

Nachdem Sie [Application Insights zu Ihrem .ASP NET-Code hinzugefügt haben](app-insights-asp-net.md), können Sie einige weitere Einstellungen vornehmen, um noch mehr Telemetriedaten zu erhalten.

## Wenn Ihre App auf Ihrem IIS-Server ausgeführt wird ...

Wenn Ihre App auf von Ihnen gesteuerten IIS-Servern gehostet wird, installieren Sie den Application Insights-Statusmonitor auf diesen Servern. Wenn der Monitor bereits installiert ist, müssen Sie nichts weiter tun.

1. Melden Sie sich mit Administratorrechten auf jedem IIS-Webserver an.
2. Laden Sie den [Statusmonitor-Installer](http://go.microsoft.com/fwlink/?LinkId=506648) herunter, und starten Sie ihn.
3. Melden Sie sich im Installations-Assistenten bei Microsoft Azure an.

Sie müssen nichts weiter tun, aber Sie können bestätigen, dass die Überwachung für Ihre App aktiviert ist.

![In Azure erweitern](./media/app-insights-asp-net-more/025.png)

(Sie können den Statusmonitor auch verwenden, um die [Überwachung zur Laufzeit zu aktivieren](app-insights-monitor-performance-live-website-now.md), auch wenn Sie Ihre Apps nicht in Visual Studio instrumentiert haben.)

### Was erhalten Sie?

Wenn der Statusmonitor auf Ihren Servercomputern installiert ist, erhalten Sie zusätzliche Telemetriedaten:

* Telemetriedaten zu Abhängigkeiten (SQL-Aufrufe und REST-Aufrufe von Ihrer App) für .NET 4.5-Apps. (Bei höheren .NET-Versionen ist der Statusmonitor nicht erforderlich, um Telemetriedaten zu Abhängigkeiten abzurufen.)
* Ausnahmestapelüberwachungen zeigen mehr Details.
* Leistungsindikatoren. In Application Insights werden diese Indikatoren auf dem Blatt „Server“ angezeigt.

![In Azure erweitern](./media/app-insights-asp-net-more/070.png)

Um mehr oder weniger Indikatoren anzuzeigen, [bearbeiten Sie die Diagramme](app-insights-metrics-explorer.md). Wenn der gewünschte Leistungsindikator sich nicht im Satz verfügbarer Indikatoren befindet, können Sie ihn [zu dem Indikatorsatz hinzufügen, dessen Daten vom Leistungsindikatorenmodul gesammelt werden](app-insights-web-monitor-performance.md#system-performance-counters).

## Wenn es sich um eine Azure-Web-App handelt ...

Wenn Ihre App als Azure-Web-App ausgeführt wird, wechseln Sie zur Azure-Systemsteuerung für die App oder den virtuellen Computer, und fügen Sie die Application Insights-Erweiterung hinzu. Öffnen Sie im Menü **Extras** die Option **Leistungsüberwachung**, und konfigurieren Sie **Application Insights**. Wenn Sie dazu aufgefordert werden, wählen Sie die Application Insights-Ressource, die Sie bereits erstellt haben.

![In Azure erweitern](./media/app-insights-asp-net-more/05-extend.png)

### Was erhalten Sie?

* Ausnahmestapelüberwachungen zeigen mehr Details.
* Telemetriedaten zu Abhängigkeiten (SQL-Aufrufe und REST-Aufrufe von Ihrer App) für .NET 4.5-Apps. (Bei höheren .NET-Versionen ist die Erweiterung nicht erforderlich, um Telemetriedaten zu Abhängigkeiten abzurufen.)

![In Azure erweitern](./media/app-insights-asp-net-more/080.png)

(Sie können diese Methode auch verwenden, um die [Leistungsüberwachung zur Laufzeit zu aktivieren](app-insights-monitor-performance-live-website-now.md), auch wenn Sie Ihre App nicht in Visual Studio instrumentiert haben.)

## Clientseitige Überwachung

Sie haben das SDK installiert, das Telemetriedaten vom Server (Back-End) Ihrer Anwendung sendet. Nun können Sie die clientseitige Überwachung hinzufügen. Dadurch erhalten Sie Daten zu Benutzern, Sitzungen, Seitenaufrufen und allen Ausnahmen oder Abstürzen, die im Browser auftreten. Sie können zudem eigenen Code schreiben, um nachzuverfolgen, wie Ihre Benutzer Ihre App verwenden – bis zur Detailebene der Mausklicks und Tastaturanschläge.

Fügen Sie den JavaScript-Codeausschnitt für Application Insights zu jeder Webseite hinzu, um Telemetriedaten aus Clientbrowsern zu erhalten.

1. Öffnen Sie in Azure die Application Insights-Ressource für Ihre App.
2. Öffnen Sie „Schnellstart“ und dann die Überwachung für die Clientseite, und kopieren Sie den Codeausschnitt.
3. Fügen Sie den Ausschnitt so ein, dass er im Header jeder Webseite angezeigt wird – am besten, indem Sie den Ausschnitt auf der Masterlayoutseite einfügen.

![In Azure erweitern](./media/app-insights-asp-net-more/100.png)

Beachten Sie, dass der Code den Instrumentationsschlüssel enthält, der Ihre Anwendungsressource identifiziert.

### Was erhalten Sie?

* Sie können JavaScript-Code schreiben, um [benutzerdefinierte Telemetriedaten von Ihren Webseiten](app-insights-api-custom-events-metrics.md) zu senden, z.B. zum Nachverfolgen von Schaltflächenklicks.
* In [Analytics](app-insights-analytics.md) erhalten Sie Daten in `pageViews` und AJAX-Daten in `dependencies`.
* [Leistungs- und Nutzungsdaten für Clients](app-insights-javascript.md) im Blatt „Browser“.

![In Azure erweitern](./media/app-insights-asp-net-more/090.png)


[Weitere Informationen zur Nachverfolgung von Webseiten.](app-insights-web-track-usage.md)



## Nachverfolgen der Anwendungsversion

Stellen Sie sicher, dass `buildinfo.config` vom MSBuild-Prozess generiert wird. Fügen Sie in Ihrer CSPROJ-Datei Folgendes hinzu:

```XML

    <PropertyGroup>
      <GenerateBuildInfoConfigFile>true</GenerateBuildInfoConfigFile>    <IncludeServerNameInBuildInfo>true</IncludeServerNameInBuildInfo>
    </PropertyGroup> 
```

Wenn das Webmodul Application Insights über die Buildinformationen verfügt, fügt es jedem Telemetrieelement automatisch die **Anwendungsversion** als Eigenschaft hinzu. Dies ermöglicht es Ihnen, nach Version zu filtern, wenn Sie [Diagnosesuchen](app-insights-diagnostic-search.md) ausführen oder [Metriken untersuchen](app-insights-metrics-explorer.md).

Beachten Sie jedoch, dass die Buildversionsnummer nur von MSBuild nicht vom Entwicklerbuild in Visual Studio generiert wird.


## Verfügbarkeitswebtests

Senden Sie der Web-App in regelmäßigen Abständen HTTP-Anforderungen aus aller Welt. Wir benachrichtigen Sie, wenn die Reaktion langsam oder unzuverlässig ist.

Klicken Sie in der Application Insights-Ressource für Ihre App auf die Kachel „Verfügbarkeit“, um Webtests hinzuzufügen, zu bearbeiten und anzuzeigen.

Sie können mehrere Tests an mehreren Standorten ausführen.

![In Azure erweitern](./media/app-insights-asp-net-more/110.png)

[Weitere Informationen](app-insights-monitor-web-app-availability.md)

## Benutzerdefinierte Telemetrie und Protokollierung

Die Application Insights-Pakete, die Sie zu Ihrem Code hinzugefügt haben, stellen eine API bereit, die Sie aus Ihrer Anwendung aufrufen können.

* [Generieren Sie eigene Ereignisse und Metriken](app-insights-api-custom-events-metrics.md), z.B. zum Erfassen von Geschäftsereignissen oder zum Überwachen der Leistung.
* [Erfassen Sie Protokollablaufverfolgungen](app-insights-asp-net-trace-logs.md) aus Log4Net, NLog, oder System.Diagnostics.Trace.
* [Filtern, ändern, oder ergänzen](app-insights-api-filtering-sampling.md) Sie die standardmäßigen Telemetriedaten, die von Ihrer App gesendet werden, indem Sie Telemetrieprozessoren schreiben.


## Leistungsfähige Analysen und Präsentationen

Es gibt zahlreiche Möglichkeiten, Ihre Daten zu untersuchen. Wenn Sie erst vor Kurzem begonnen haben, mit Application Insights zu arbeiten, lesen Sie diese Artikel:

||
|---|---
|[**Diagnosesuche nach Instanzdaten**](app-insights-visual-studio.md)<br/>Suchen und filtern Sie Ereignisse wie Anforderungen, Ausnahmen, Abhängigkeitsaufrufe, Protokollablaufverfolgungen und Seitenaufrufe. Wechseln Sie in Visual Studio von Stapelüberwachungen zum Code.|![Visual Studio](./media/app-insights-asp-net/61.png)
|[**Metrik-Explorer für aggregierte Daten**](app-insights-metrics-explorer.md)<br/>Durchsuchen, filtern und segmentieren Sie aggregierte Daten wie z.B. Anforderungs-, Fehler- und Ausnahmeraten, Antwortzeiten, Seitenladezeiten.|![Visual Studio](./media/app-insights-asp-net-more/060.png)
|[**Dashboards**](app-insights-dashboards.md#dashboards)<br/>Kombinieren Sie Daten aus mehreren Ressourcen, und geben Sie sie für andere frei. Dies ist sehr gut für Anwendungen mit mehreren Komponenten und für die kontinuierliche Anzeige im Teamraum geeignet. |![Beispiel für Dashboards](./media/app-insights-asp-net/62.png)
|[**Live Metrics Stream**](app-insights-metrics-explorer.md#live-metrics-stream)<br/>Wenn Sie einen neuen Build bereitstellen, sehen Sie sich diese beinahe in Echtzeit verfügbaren Leistungsindikatoren an, um sicherzustellen, dass alles wie erwartet funktioniert.|![Beispiel für Analysen](./media/app-insights-asp-net-more/050.png)
|[**Analytics**](app-insights-analytics.md)<br/>Beantworten Sie schwierige Fragen zur Leistung und Nutzung Ihrer App mithilfe dieser leistungsstarken Abfragesprache.|![Beispiel für Analysen](./media/app-insights-asp-net-more/010.png)
|[**Automatische und manuelle Warnungen**](app-insights-alerts.md)<br/>Automatische Warnungen sind an die normalen Telemetriemuster Ihrer App angepasst und werden ausgelöst, wenn etwas nicht den üblichen Mustern entspricht. Sie können auch Warnungen auf bestimmten Ebenen von benutzerdefinierten oder standardmäßigen Metriken festlegen.|![Beispiel für Warnungen](./media/app-insights-asp-net-more/020.png)

## Datenverwaltung

|||
|---|---|
|[**Fortlaufender Export**](app-insights-export-telemetry.md)<br/>Kopieren Sie alle Telemetriedaten in den Speicher, sodass Sie sie auf Ihre eigene Weise analysieren können.|
|**API für den Datenzugriff**<br/>In Kürze verfügbar.|
|[**Stichprobenerstellung**](app-insights-sampling.md)<br/>Reduziert die Datenrate und hilft Ihnen dabei, innerhalb Ihres Tariflimits zu bleiben.|![Kachel für Stichproben](./media/app-insights-asp-net-more/030.png)

<!---HONumber=AcomDC_0907_2016-->