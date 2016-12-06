---
title: "Einrichten der Web-App-Analyse für ASP.NET mit Application Insights | Microsoft Docs"
description: "Konfigurieren Sie Leistung, Verfügbarkeit und Nutzungsanalyse für Ihre lokal oder in Azure gehostete ASP.NET-Website."
services: application-insights
documentationcenter: .net
author: NumberByColors
manager: douge
ms.assetid: d0eee3c0-b328-448f-8123-f478052751db
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: get-started-article
ms.date: 10/13/2016
ms.author: awills
translationtype: Human Translation
ms.sourcegitcommit: dea21a59b189d1d3d474cbc5e67f64df485a1981
ms.openlocfilehash: a97e20b208d92e03bd4458605aaa48ef7c389e32


---
# <a name="set-up-application-insights-for-aspnet"></a>Einrichten von Application Insights für ASP.NET
[Azure Application Insights](app-insights-overview.md) überwacht Ihre Live-Anwendung. So können Sie [Leistungsprobleme und -ausnahmen erkennen und diagnostizieren](app-insights-detect-triage-diagnose.md) und zudem [ermitteln, wie Ihre App verwendet wird](app-insights-overview-usage.md).  Dies funktioniert für Apps, die auf den eigenen lokalen IIS-Servern oder Cloud-VMs gehostet werden, sowie für Azure-Web-Apps.

## <a name="before-you-start"></a>Vorbereitung
Erforderlich:

* Visual Studio 2013, Update 3 oder höher. Eine neuere Version ist besser.
* Ein Abonnement für [Microsoft Azure](http://azure.com). Wenn Ihr Team oder Ihre Organisation über ein Azure-Abonnement verfügt, kann der Besitzer Sie mit Ihrem [Microsoft-Konto](http://live.com)hinzufügen. 

Es gibt noch weitere Artikel, die für Sie unter Umständen von Interesse sind:

* [Installieren des Application Insights-Statusmonitors zum Überwachen der Websiteleistung](app-insights-monitor-performance-live-website-now.md)
* [Azure Cloud Services](app-insights-cloudservices.md)

## <a name="a-nameidea-1-add-application-insights-sdk"></a><a name="ide"></a> 1. Hinzufügen des Application Insights-SDK
### <a name="if-its-a-new-project"></a>Falls es sich um ein neues Projekt handelt ...
Achten Sie darauf, dass Application Insights ausgewählt ist, wenn Sie in Visual Studio ein neues Projekt erstellen. 

![Erstellen eines ASP.NET-Projekts](./media/app-insights-asp-net/appinsights-01-vsnewp1.png)

### <a name="-or-if-its-an-existing-project"></a>… oder falls es sich um ein vorhandenes Projekt handelt
Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste auf das Projekt, und klicken Sie auf **Application Insights-Telemetrie hinzufügen** oder **Application Insights konfigurieren**.

![Application Insights hinzufügen](./media/app-insights-asp-net/appinsights-03-addExisting.png)

* ASP.NET Core-Projekt? – [Führen Sie diese Anleitung zum Ändern einiger Codezeilen aus](https://github.com/Microsoft/ApplicationInsights-aspnetcore/wiki/Getting-Started#add-application-insights-instrumentation-code-to-startupcs). 

## <a name="a-nameruna-2-run-your-app"></a><a name="run"></a> 2. Führen Sie Ihre App aus.
Starten Sie Ihre Anwendung mit F5, und probieren Sie es aus: Öffnen Sie verschiedene Seiten, um einige Telemetriedaten zu generieren.

In Visual Studio wird die Anzahl von protokollierten Ereignissen angezeigt. 

![In Visual Studio wird die Schaltfläche „Application Insights“ während des Debuggens angezeigt.](./media/app-insights-asp-net/54.png)

## <a name="3-see-your-telemetry"></a>3. Anzeigen der Telemetrie:
### <a name="-in-visual-studio"></a>In Visual Studio
Öffnen Sie das Application Insights-Fenster in Visual Studio: Klicken Sie entweder auf die Application Insights-Schaltfläche, oder klicken Sie im Projektmappen-Explorer mit der rechten Maustaste auf das Projekt:

![In Visual Studio wird die Schaltfläche „Application Insights“ während des Debuggens angezeigt.](./media/app-insights-asp-net/55.png)

Diese Ansicht enthält die Telemetriedaten, die auf der Serverseite Ihrer App generiert werden. Experimentieren Sie mit den Filtern, und klicken Sie auf ein beliebiges Ereignis, um weitere Details anzuzeigen.

[Erfahren Sie mehr zu Application Insights-Tools in Visual Studio](app-insights-visual-studio.md).

<a name="monitor"></a> 

### <a name="-in-the-portal"></a>Im Portal
Sie können die Telemetriedaten auch im Application Insights-Webportal anzeigen, es sei denn, Sie wählen die Option *Nur SDK installieren* . 

Das Portal verfügt über eine größere Zahl von Diagrammen, Analysetools und Dashboards als Visual Studio. 

Öffnen Sie die Application Insights-Ressource im [Azure-Portal](https://portal.azure.com/).

![Klicken Sie mit der rechten Maustaste auf Ihr Projekt, und öffnen Sie das Azure-Portal.](./media/app-insights-asp-net/appinsights-04-openPortal.png)

Das Portal wird mit einer Ansicht der Telemetriedaten Ihrer App geöffnet: ![](./media/app-insights-asp-net/66.png)

* Die ersten Telemetriedaten werden unter [Live Metrics Stream](app-insights-metrics-explorer.md#live-metrics-stream)angezeigt.
* Einzelne Ereignisse werden unter **Suche** (1) angezeigt. Es kann einige Minuten dauern, bis Daten angezeigt werden. Klicken Sie auf ein beliebiges Ereignis, um seine Eigenschaften anzuzeigen. 
* Aggregierte Metriken werden in den Diagrammen (2) angezeigt. Es kann ein oder zwei Minuten dauern, bis die Daten hier angezeigt werden. Klicken Sie auf ein beliebiges Diagramm, um ein Blatt mit mehr Details zu öffnen.

[Erfahren Sie mehr über die Verwendung von Application Insights im Azure-Portal](app-insights-dashboards.md).

## <a name="4-publish-your-app"></a>4. Veröffentlichen der App
Veröffentlichen Sie Ihre App auf Ihrem IIS-Server oder in Azure. Sehen Sie sich [Live Metrics Stream](app-insights-metrics-explorer.md#live-metrics-stream) an, um sicherzustellen, dass alles reibungslos funktioniert.

Sie können dann verfolgen, wie Ihre Telemetriedaten im Application Insights-Portal erstellt werden. Darin können Sie Metriken überwachen, die Telemetriedaten durchsuchen und [Dashboards](app-insights-dashboards.md) einrichten. Außerdem können Sie die leistungsfähige [Analytics-Abfragesprache](app-insights-analytics.md) verwenden, um die Nutzung und Leistung zu analysieren oder nach bestimmten Ereignissen zu suchen. 

Sie können Ihre Telemetriedaten auch in [Visual Studio](app-insights-visual-studio.md) mit Tools wie der Diagnosesuche und [Trends](app-insights-visual-studio-trends.md) weiter analysieren.

> [!NOTE]
> Wenn Ihre App so viele Telemetriedaten sendet, dass die [Einschränkungsgrenzwerte](app-insights-pricing.md#limits-summary) bald erreicht werden, wird das automatische [Sampling](app-insights-sampling.md) aktiviert. Mit dem Sampling wird die Menge der von der App gesendeten Telemetriedaten reduziert, während gleichzeitig korrelierte Daten für Diagnosezwecke beibehalten werden.
> 
> 

## <a name="a-namelanda-what-did-add-application-insights-do"></a><a name="land"></a> Welche Funktion hat "Application Insights hinzufügen"?
Application Insights sendet Telemetriedaten aus Ihrer App an das Application Insights-Portal (in Microsoft Azure gehostet):

![](./media/app-insights-asp-net/01-scheme.png)

Mit dem Befehl werden also drei Aufgaben erledigt:

1. Er fügt das NuGet-Paket mit dem Application Insights-Web-SDK dem Projekt hinzu. Um es in Visual Studio anzuzeigen, klicken Sie mit der rechten Maustaste auf Ihr Projekt, und wählen Sie "NuGet-Pakete verwalten" aus.
2. Erstellen Sie im [Azure-Portal](https://portal.azure.com/)eine Application Insights-Ressource. Hier werden Ihre Daten angezeigt. Er ruft den *Instrumentationsschlüssel* ab, der die Ressource identifiziert.
3. Fügt den Instrumentationsschlüssel in `ApplicationInsights.config`ein, damit das SDK Telemetriedaten an das Portal senden kann.

Sie können diese Schritte für [ASP.NET 4](app-insights-windows-services.md) oder [ASP.NET Core](https://github.com/Microsoft/ApplicationInsights-aspnetcore/wiki/Getting-Started) auch manuell ausführen.

### <a name="to-upgrade-to-future-sdk-versions"></a>So upgraden Sie auf zukünftige SDK-Versionen
Für ein Upgrade auf eine [neue Version des SDK](https://github.com/Microsoft/ApplicationInsights-dotnet-server/releases)öffnen Sie wieder den NuGet-Paket-Manager und filtern die Ansicht nach installierten Paketen. Wählen Sie "Microsoft.ApplicationInsights.Web" und dann "Upgrade" aus.

Wenn Sie Anpassungen an der Datei "ApplicationInsights.config" vorgenommen haben, speichern Sie vor dem Upgrade eine Kopie davon. Sie können anschließend die Änderungen in die neue Version übernehmen.

## <a name="add-more-telemetry"></a>Hinzufügen von weiteren Telemetrieelementen
### <a name="web-pages-and-single-page-apps"></a>Webseiten und Einzelseiten-Apps
1. [Fügen Sie Ihren Webseiten den JavaScript-Codeausschnitt hinzu](app-insights-javascript.md), um die Blätter „Browser“ und „Nutzung“ mit Daten zu Seitenansichten, Ladedauern, Browserausnahmen, AJAX-Aufrufleistung und Benutzer- und Sitzungszahlen zu füllen.
2. [Codieren Sie die benutzerdefinierten Ereignisse](app-insights-api-custom-events-metrics.md), um für Benutzeraktionen die Anzahl zu ermitteln, die Dauer zu messen oder Messungen durchzuführen.

### <a name="dependencies-exceptions-and-performance-counters"></a>Abhängigkeiten, Ausnahmen und Leistungsindikatoren
[Installieren Sie den Statusmonitor](app-insights-monitor-performance-live-website-now.md) auf allen Servercomputern, um zusätzliche Telemetriedaten zu Ihrer App zu erhalten. Sie erhalten Folgendes:

* [Leistungsindikatoren](app-insights-performance-counters.md) - 
  CPU-, Arbeitsspeicher, Datenträger- und andere Leistungsindikatoren für Ihre App. 
* [Ausnahmen](app-insights-asp-net-exceptions.md): Ausführlichere Telemetriedaten für einige Ausnahmen.
* [Abhängigkeiten](app-insights-asp-net-dependencies.md): Aufrufe der REST-API oder von SQL-Diensten. Ermitteln Sie, ob langsame Reaktionen externer Komponenten für Ihre App zu Leistungsproblemen führen. (Wenn Ihre App unter .NET 4.6 ausgeführt wird, ist es nicht erforderlich, diese Telemetriedaten mit dem Statusmonitor abzurufen.)

### <a name="diagnostic-code"></a>Diagnosecode
Liegt bei Ihnen ein Problem vor? Sie haben mehrere Möglichkeiten, wenn Sie zur Unterstützung der Diagnose Code in die App einfügen möchten:

* [Erfassen von Protokollablaufverfolgungen](app-insights-asp-net-trace-logs.md): Wenn Sie bereits Log4N, NLog oder System.Diagnostics.Trace zum Protokollieren von Ablaufverfolgungsereignissen verwenden, kann die Ausgabe an Application Insights gesendet werden, damit Sie sie mit Anforderungen verknüpfen, durchsuchen und analysieren können. 
* [Benutzerdefinierte Ereignisse und Metriken](app-insights-api-custom-events-metrics.md): Verwenden Sie „TrackEvent()“ und „TrackMetric()“ entweder in Server- oder Webseitencode.
* [Kennzeichnen von Telemetrie mit zusätzlichen Eigenschaften](app-insights-api-filtering-sampling.md#add-properties)

Verwenden Sie [Search](app-insights-diagnostic-search.md), um bestimmte Ereignisse zu finden und zu korrelieren, und [Analytics](app-insights-analytics.md), um leistungsstärkere Abfragen durchzuführen.

## <a name="alerts"></a>Warnungen
Sie sollten die erste Person sein, die über Probleme mit Ihrer App informiert wird. (Warten Sie nicht, bis Sie von Benutzern darauf aufmerksam gemacht werden!) 

* [Erstellen Sie Webtests](app-insights-monitor-web-app-availability.md), um sicherzustellen, dass Ihre Website im Web sichtbar ist.
* [Proaktive Diagnosen](app-insights-proactive-diagnostics.md) werden automatisch ausgeführt (wenn die App über eine bestimmte Mindestmenge an Datenverkehr verfügt). Für die Einrichtung fällt für Sie kein Aufwand an. Sie werden darüber benachrichtigt, ob für Ihre App eine ungewöhnlich hohe Zahl von Anforderungen mit Fehlern vorliegt.
* [Richten Sie Metrikwarnungen ein](app-insights-alerts.md), damit Sie gewarnt werden, wenn für eine Metrik ein Schwellenwert überschritten wird. Sie können diese für benutzerdefinierte Metriken festlegen, die Sie in Ihrer App codieren.

Standardmäßig werden Warnungsbenachrichtigungen an den Besitzer des Azure-Abonnements gesendet. 

![Beispiel für Warnung per E-Mail](./media/app-insights-asp-net/alert-email.png)

## <a name="version-and-release-tracking"></a>Versionsnachverfolgung
### <a name="track-application-version"></a>Nachverfolgen der Anwendungsversion
Stellen Sie sicher, dass `buildinfo.config` vom MSBuild-Prozess generiert wird. Fügen Sie in Ihrer CSPROJ-Datei Folgendes hinzu:  

```XML

    <PropertyGroup>
      <GenerateBuildInfoConfigFile>true</GenerateBuildInfoConfigFile>    <IncludeServerNameInBuildInfo>true</IncludeServerNameInBuildInfo>
    </PropertyGroup> 
```

Wenn das Webmodul Application Insights über die Buildinformationen verfügt, fügt es jedem Telemetrieelement automatisch die **Anwendungsversion** als Eigenschaft hinzu. Dies ermöglicht es Ihnen, nach der Version zu filtern, wenn Sie [Diagnosesuchen](app-insights-diagnostic-search.md) ausführen oder [Metriken untersuchen](app-insights-metrics-explorer.md). 

Beachten Sie jedoch, dass die Buildversionsnummer nur von MSBuild nicht vom Entwicklerbuild in Visual Studio generiert wird.

### <a name="release-annotations"></a>Versionsanmerkungen
Bei Verwendung von Visual Studio Team Services können Sie Ihren Diagrammen einen [Anmerkungsmarker](app-insights-annotations.md) hinzufügen lassen, wenn Sie eine neue Version veröffentlichen.

![Beispiel für Versionsanmerkungen](./media/app-insights-asp-net/release-annotation.png)

## <a name="next-steps"></a>Nächste Schritte
|  |
| --- | --- |
| **[Arbeiten mit Application Insights in Visual Studio](app-insights-visual-studio.md)**<br/>Enthält Informationen zu den Bereichen Debuggen von Telemetriedaten, Diagnosesuche und Drillthrough zum Code. |
| **[Navigation und Dashboards im Application Insights-Portal](app-insights-dashboards.md)**<br/>Enthält Informationen zu Dashboards, leistungsstarken Diagnose- und Analysetools, Warnungen und zum Export von Telemetriedaten sowie eine Livekarte der Abhängigkeiten Ihrer Anwendung. |
| **[Hinzufügen weiterer Daten](app-insights-asp-net-more.md)**<br/>Überwachen Sie Nutzung, Verfügbarkeit, Abhängigkeiten und Ausnahmen. Integrieren Sie Ablaufverfolgungen aus Protokollierungsframeworks. Schreiben Sie benutzerdefinierte Telemetriedaten. |




<!--HONumber=Nov16_HO3-->


