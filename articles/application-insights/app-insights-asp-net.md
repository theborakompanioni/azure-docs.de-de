<properties 
	pageTitle="Einrichten der Web-App-Analyse für ASP.NET mit Application Insights" 
	description="Konfigurieren Sie Leistung, Verfügbarkeit und Nutzungsanalyse für Ihre lokal oder in Azure gehostete ASP.NET-Website." 
	services="application-insights" 
    documentationCenter=".net"
	authors="NumberByColors" 
	manager="douge"/>

<tags 
	ms.service="application-insights" 
	ms.workload="tbd" 
	ms.tgt_pltfrm="ibiza" 
	ms.devlang="na" 
	ms.topic="get-started-article" 
	ms.date="08/09/2016" 
	ms.author="daviste"/>


# Einrichten von Application Insights für ASP.NET

[Visual Studio Application Insights](app-insights-overview.md) überwacht Ihre Live-Anwendung. So können Sie [Leistungsprobleme und -ausnahmen erkennen und diagnostizieren](app-insights-detect-triage-diagnose.md) und zudem [ermitteln, wie Ihre App verwendet wird](app-insights-overview-usage.md). Dies funktioniert für Apps, die auf den eigenen lokalen IIS-Servern oder Cloud-VMs gehostet werden, sowie für Azure-Web-Apps.


## Vorbereitung

Erforderlich:

* Visual Studio 2013, Update 3 oder höher. Eine neuere Version ist besser.
* Ein Abonnement für [Microsoft Azure](http://azure.com) Wenn Ihr Team oder Ihre Organisation über ein Azure-Abonnement verfügt, kann der Besitzer Sie mit Ihrem [Microsoft-Konto](http://live.com) hinzufügen.

Es gibt noch weitere Artikel, die für Sie unter Umständen von Interesse sind:

* [Installieren des Application Insights-Statusmonitors zum Überwachen der Websiteleistung](app-insights-monitor-performance-live-website-now.md) (zum Thema: Instrumentieren einer Web-App zur Laufzeit)
* [Azure Cloud Services](app-insights-cloudservices.md)

## <a name="ide"></a> 1. Hinzufügen des Application Insights-SDK


### Falls es sich um ein neues Projekt handelt ...

Achten Sie darauf, dass Application Insights ausgewählt ist, wenn Sie in Visual Studio ein neues Projekt erstellen.


![Erstellen eines ASP.NET-Projekts](./media/app-insights-asp-net/appinsights-01-vsnewp1.png)


### … oder falls es sich um ein vorhandenes Projekt handelt

Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste auf das Projekt, und klicken Sie auf **Application Insights-Telemetrie hinzufügen** oder **Application Insights konfigurieren**.

![Application Insights hinzufügen](./media/app-insights-asp-net/appinsights-03-addExisting.png)

* ASP.NET Core-Projekt? – [Führen Sie diese Anleitung zum Ändern einiger Codezeilen aus](https://github.com/Microsoft/ApplicationInsights-aspnetcore/wiki/Getting-Started#add-application-insights-instrumentation-code-to-startupcs).



## <a name="run"></a> 2. Führen Sie Ihre App aus.

Starten Sie Ihre Anwendung mit F5, und probieren Sie es aus: Öffnen Sie verschiedene Seiten, um einige Telemetriedaten zu generieren.

In Visual Studio wird die Anzahl von protokollierten Ereignissen angezeigt.

![In Visual Studio wird die Schaltfläche „Application Insights“ während des Debuggens angezeigt.](./media/app-insights-asp-net/54.png)

## 3\. Anzeigen der Telemetrie:

### In Visual Studio

Öffnen Sie das Application Insights-Fenster in Visual Studio: Klicken Sie entweder auf die Application Insights-Schaltfläche, oder klicken Sie im Projektmappen-Explorer mit der rechten Maustaste auf das Projekt:

![In Visual Studio wird die Schaltfläche „Application Insights“ während des Debuggens angezeigt.](./media/app-insights-asp-net/55.png)

Diese Ansicht enthält die Telemetriedaten, die auf der Serverseite Ihrer App generiert werden. Experimentieren Sie mit den Filtern, und klicken Sie auf ein beliebiges Ereignis, um weitere Details anzuzeigen.

[Erfahren Sie mehr zu Application Insights-Tools in Visual Studio](app-insights-visual-studio.md).

<a name="monitor"></a>
### Im Portal

Sie können die Telemetriedaten auch im Application Insights-Webportal anzeigen, es sei denn, Sie wählen die Option *Nur SDK installieren*.

Das Portal verfügt über eine größere Zahl von Diagrammen, Analysetools und Dashboards als Visual Studio.


Öffnen Sie die Application Insights-Ressource im [Azure-Portal](https://portal.azure.com/).

![Klicken Sie mit der rechten Maustaste auf Ihr Projekt, und öffnen Sie das Azure-Portal.](./media/app-insights-asp-net/appinsights-04-openPortal.png)

Das Portal wird mit einer Ansicht der Telemetriedaten Ihrer App geöffnet: ![](./media/app-insights-asp-net/66.png)

* Die ersten Telemetriedaten werden unter [Live Metrics Stream](app-insights-metrics-explorer.md#live-metrics-stream) angezeigt.
* Einzelne Ereignisse werden unter **Suche** (1) angezeigt. Es kann einige Minuten dauern, bis Daten angezeigt werden. Klicken Sie auf ein beliebiges Ereignis, um seine Eigenschaften anzuzeigen.
* Aggregierte Metriken werden in den Diagrammen (2) angezeigt. Es kann ein oder zwei Minuten dauern, bis die Daten hier angezeigt werden. Klicken Sie auf ein beliebiges Diagramm, um ein Blatt mit mehr Details zu öffnen.

[Erfahren Sie mehr über die Verwendung von Application Insights im Azure-Portal](app-insights-dashboards.md).

## 4\. Veröffentlichen der App

Veröffentlichen Sie Ihre App auf Ihrem IIS-Server oder in Azure. Sehen Sie sich [Live Metrics Stream](app-insights-metrics-explorer.md#live-metrics-stream) an, um sicherzustellen, dass alles reibungslos funktioniert.

Sie können dann verfolgen, wie Ihre Telemetriedaten im Application Insights-Portal erstellt werden. Darin können Sie Metriken überwachen, die Telemetriedaten durchsuchen und [Dashboards](app-insights-dashboards.md) einrichten. Außerdem können Sie die leistungsfähige [Analytics-Abfragesprache](app-insights-analytics.md) verwenden, um die Nutzung und Leistung zu analysieren oder nach bestimmten Ereignissen zu suchen.

Sie können Ihre Telemetriedaten auch in [Visual Studio](app-insights-visual-studio.md) mit Tools wie der Diagnosesuche und [Trends](app-insights-visual-studio-trends.md) weiter analysieren.

> [AZURE.NOTE] Wenn Ihre App so viele Telemetriedaten sendet, dass die [Einschränkungsgrenzwerte](app-insights-pricing.md#limits-summary) bald erreicht werden, wird das automatische [Sampling](app-insights-sampling.md) aktiviert. Mit dem Sampling wird die Menge der von der App gesendeten Telemetriedaten reduziert, während gleichzeitig korrelierte Daten für Diagnosezwecke beibehalten werden.


##<a name="land"></a> Welche Funktion hat "Application Insights hinzufügen"?

Application Insights sendet Telemetriedaten aus Ihrer App an das Application Insights-Portal (in Microsoft Azure gehostet):

![](./media/app-insights-asp-net/01-scheme.png)

Mit dem Befehl werden also drei Aufgaben erledigt:

1. Er fügt das NuGet-Paket mit dem Application Insights-Web-SDK dem Projekt hinzu. Um es in Visual Studio anzuzeigen, klicken Sie mit der rechten Maustaste auf Ihr Projekt, und wählen Sie "NuGet-Pakete verwalten" aus.
2. Erstellen Sie im [Azure-Portal](https://portal.azure.com/) eine Application Insights-Ressource. Hier werden Ihre Daten angezeigt. Er ruft den *Instrumentationsschlüssel* ab, der die Ressource identifiziert.
3. Fügt den Instrumentationsschlüssel in `ApplicationInsights.config` ein, damit das SDK Telemetriedaten an das Portal senden kann.

Sie können diese Schritte für [ASP.NET 4](app-insights-asp-net-manual.md) oder [ASP.NET Core](https://github.com/Microsoft/ApplicationInsights-aspnetcore/wiki/Getting-Started) auch manuell ausführen.

### So upgraden Sie auf zukünftige SDK-Versionen

Für ein Upgrade auf eine [neue Version des SDK](app-insights-release-notes-dotnet.md) öffnen Sie wieder den NuGet-Paket-Manager und filtern die Ansicht nach installierten Paketen. Wählen Sie "Microsoft.ApplicationInsights.Web" und dann "Upgrade" aus.

Wenn Sie Anpassungen an der Datei "ApplicationInsights.config" vorgenommen haben, speichern Sie vor dem Upgrade eine Kopie davon. Sie können anschließend die Änderungen in die neue Version übernehmen.



## Wie geht es weiter?

| | 
|---|---
|**[Arbeiten mit Application Insights in Visual Studio](app-insights-visual-studio.md)**<br/>Enthält Informationen zu den Bereichen Debuggen von Telemetriedaten, Diagnosesuche und Drillthrough zum Code.|![Visual Studio](./media/app-insights-asp-net/61.png)
|**[Navigation und Dashboards im Application Insights-Portal](app-insights-dashboards.md)**<br/>Enthält Informationen zu Dashboards, leistungsstarken Diagnose- und Analysetools, Warnungen und zum Export von Telemetriedaten sowie eine Livekarte der Abhängigkeiten Ihrer Anwendung. |![Visual Studio](./media/app-insights-asp-net/62.png)
|**[Hinzufügen weiterer Daten](app-insights-asp-net-more.md)**<br/>Überwachen Sie Nutzung, Verfügbarkeit, Abhängigkeiten und Ausnahmen. Integrieren Sie Ablaufverfolgungen aus Protokollierungsframeworks. Schreiben Sie benutzerdefinierte Telemetriedaten. | ![Visual Studio](./media/app-insights-asp-net/64.png)

<!---HONumber=AcomDC_0810_2016-->