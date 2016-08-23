<properties
	pageTitle="Hinzufügen des Application Insights SDK zur Überwachung der Node.js-App | Microsoft Azure"
	description="Analysieren Sie die Auslastung, Verfügbarkeit und Leistung Ihrer lokalen oder Microsoft Azure-Webanwendung mit Application Insights."
	services="application-insights"
    documentationCenter=""
	authors="alancameronwills"
	manager="douge"/>

<tags
	ms.service="application-insights"
	ms.workload="tbd"
	ms.tgt_pltfrm="ibiza"
	ms.devlang="na"
	ms.topic="get-started-article"
	ms.date="05/25/2016"
	ms.author="awills"/>


# Hinzufügen des Application Insights SDK zur Überwachung der Node.js-App

*Application Insights befindet sich in der Vorschau.*

[Visual Studio Application Insights](app-insights-overview.md) überwacht Ihre Live-Anwendung. So können Sie [Leistungsprobleme und -ausnahmen erkennen und diagnostizieren](app-insights-detect-triage-diagnose.md) und zudem [ermitteln, wie Ihre App verwendet wird](app-insights-overview-usage.md). Dies funktioniert für Apps, die auf den eigenen lokalen IIS-Servern oder Azure-VMs gehostet werden, sowie für Azure-Web-Apps.



Das SDK ermöglicht die automatische Erfassung der Raten eingehender HTTP-Anforderungen sowie von Antworten, Leistungsindikatoren (CPU, Speicher, RPS) und Ausnahmefehlern. Darüber hinaus können Sie benutzerdefinierte Aufrufe zum Nachverfolgen von Abhängigkeiten, Metriken und anderen Ereignisse hinzufügen.

![Beispiel für Leistungsüberwachungsdiagramm](./media/app-insights-asp-net-manual/10-perf.png)


#### Vorbereitung

Erforderlich:

* Visual Studio 2013 oder höher Eine neuere Version ist besser.
* Ein Abonnement für [Microsoft Azure](http://azure.com). Wenn Ihr Team oder Ihre Organisation über ein Azure-Abonnement verfügt, kann der Besitzer Sie mit Ihrem [Microsoft-Konto](http://live.com) hinzufügen.

## <a name="add"></a>Erstellen einer Application Insights-Ressource

Melden Sie sich beim [Azure-Portal][portal] an, und erstellen Sie eine neue Application Insights-Ressource. Eine [Ressource][roles] in Azure ist eine Instanz eines Diensts. In dieser Ressource werden Telemetriedaten aus Ihrer App analysiert und Ihnen angezeigt.

![Klicken Sie auf "Neu > Application Insights"](./media/app-insights-asp-net-manual/01-new-asp.png)

Wählen Sie als Anwendungstyp „Sonstiges“ aus. Durch Auswahl des Anwendungstyps werden der Standardinhalt der Ressourcenblätter und die im [Metrik-Explorer][metrics] sichtbaren Eigenschaften festgelegt.

#### Kopieren des Instrumentationsschlüssels

Der Schlüssel identifiziert die Ressource, den Sie bald im SDK installieren können, um die Daten an die Ressource zu leiten.

![Klicken Sie auf "Eigenschaften", wählen Sie den Schlüssel aus, und drücken Sie STRG+C](./media/app-insights-asp-net-manual/02-props-asp.png)


## <a name="sdk"></a> Installieren des SDK in Ihrer Anwendung

```
npm install applicationinsights --save
```

## Verwendung

Dadurch werden die Anforderungsüberwachung, die Nachverfolgung von Ausnahmefehlern und die Systemleistungsüberwachung (CPU/Speicher/RPS) ermöglicht.

```javascript

var appInsights = require("applicationinsights");
appInsights.setup("<instrumentation_key>").start();
```

Der Instrumentierungsschlüssel kann auch in der Umgebungsvariablen APPINSIGHTS\_INSTRUMENTATIONKEY festgelegt werden. In diesem Fall ist beim Aufrufen von `appInsights.setup()` oder `appInsights.getClient()` kein Argument erforderlich.

Sie können das SDK testen, ohne Telemetriedaten zu senden: Legen Sie als Instrumentierungsschlüssel eine nicht leere Zeichenfolge fest.


## <a name="run"></a> Ausführen des Projekts

Starten Sie Ihre Anwendung, und testen Sie sie: Öffnen Sie verschiedene Seiten, um einige Telemetriedaten zu generieren.


## <a name="monitor"></a> Anzeigen der Telemetrie

Kehren Sie zum [Azure-Portal](https://portal.azure.com) zurück, und navigieren Sie zur Application Insights-Ressource.


Suchen Sie nach Daten auf der Übersichtsseite. Zuerst sehen Sie lediglich einen oder zwei Punkte. Zum Beispiel:

![Klicken Sie, um weitere Daten anzuzeigen.](./media/app-insights-asp-net-manual/12-first-perf.png)

Klicken Sie sich durch ein beliebiges Diagramm, um ausführlichere Metriken anzuzeigen. [Weitere Informationen zu Metriken.][perf]

#### Sie sehen keine Daten?

* Verwenden Sie die Anwendung, und öffnen Sie verschiedene Seiten, damit einige Telemetriedaten generiert werden.
* Öffnen Sie die Kachel [Suche](app-insights-diagnostic-search.md), um einzelne Ereignisse anzuzeigen. Manchmal dauert es eine Weile, bis Ereignisse über die Metrikpipeline übertragen werden.
* Warten Sie einige Sekunden, und klicken Sie auf **Aktualisieren**. Diagramme aktualisieren sich in regelmäßigen Abständen selbst, doch Sie können sie auch manuell aktualisieren, wenn Sie auf anzuzeigende Daten warten.
* Informationen hierzu finden Sie unter [Problembehandlung][qna].

## Veröffentlichen der App

Stellen Sie jetzt Ihre Anwendung für IIS bereit, und beobachten Sie, wie die Daten gesammelt werden.


#### Keine Daten nach dem Veröffentlichen auf Ihrem Server?

Öffnen Sie diese Ports für den ausgehenden Datenverkehr in der Firewall des Servers:

+ `dc.services.visualstudio.com:443`
+ `f5.services.visualstudio.com:443`


#### Probleme auf dem Buildserver?

Weitere Informationen finden Sie in [diesem Artikel zur Problembehandlung](app-insights-asp-net-troubleshoot-no-data.md#NuGetBuild).



## Benutzerdefinierte Verwendung 

### Deaktivieren der automatischen Erfassung

```javascript
import appInsights = require("applicationinsights");
appInsights.setup("<instrumentation_key>")
    .setAutoCollectRequests(false)
    .setAutoCollectPerformance(false)
    .setAutoCollectExceptions(false)
    // no telemetry will be sent until .start() is called
    .start();
```

### Benutzerdefinierte Überwachung

```javascript
import appInsights = require("applicationinsights");
var client = appInsights.getClient();

client.trackEvent("custom event", {customProperty: "custom property value"});
client.trackException(new Error("handled exceptions can be logged with this method"));
client.trackMetric("custom metric", 3);
client.trackTrace("trace message");
```

Erfahren Sie mehr über die [Telemetrie-API](app-insights-api-custom-events-metrics.md).

### Verwenden mehrerer Instrumentierungsschlüssel

```javascript
import appInsights = require("applicationinsights");

// configure auto-collection with one instrumentation key
appInsights.setup("<instrumentation_key>").start();

// get a client for another instrumentation key
var otherClient = appInsights.getClient("<other_instrumentation_key>");
otherClient.trackEvent("custom event");
```

## Beispiele

### Nachverfolgen von Abhängigkeiten

```javascript
import appInsights = require("applicationinsights");
var client = appInsights.getClient();

var startTime = Date.now();
// execute dependency call
var endTime = Date.now();

var elapsedTime = endTime - startTime;
var success = true;
client.trackDependency("dependency name", "command name", elapsedTime, success);
```



### Manuelle Anforderungsnachverfolgung aller GET-Anforderungen

```javascript
var http = require("http");
var appInsights = require("applicationinsights");
appInsights.setup("<instrumentation_key>")
    .setAutoCollectRequests(false) // disable auto-collection of requests for this example
    .start();

// assign common properties to all telemetry sent from the default client
appInsights.client.commonProperties = {
    environment: process.env.SOME_ENV_VARIABLE
};

// track a system startup event
appInsights.client.trackEvent("server start");

// create server
var port = process.env.port || 1337
var server = http.createServer(function (req, res) {
    // track all "GET" requests
    if(req.method === "GET") {
        appInsights.client.trackRequest(req, res);
    }

    res.writeHead(200, { "Content-Type": "text/plain" });
    res.end("Hello World\n");
}).listen(port);

// track startup time of the server as a custom metric
var start = +new Date;
server.on("listening", () => {
    var end = +new Date;
    var duration = end - start;
    appInsights.client.trackMetric("StartupTime", duration);
});
```




<!--Link references-->

[knowUsers]: app-insights-overview-usage.md
[metrics]: app-insights-metrics-explorer.md
[perf]: app-insights-web-monitor-performance.md
[portal]: http://portal.azure.com/
[qna]: app-insights-troubleshoot-faq.md
[roles]: app-insights-resources-roles-access-control.md

<!---HONumber=AcomDC_0817_2016-->