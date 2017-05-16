---
title: "Überwachen von Node.js-Diensten per Azure Application Insights | Microsoft-Dokumentation"
description: "Es wird beschrieben, wie Sie die Leistung überwachen und Probleme in Node.js-Diensten mit Application Insights diagnostizieren."
services: application-insights
documentationcenter: nodejs
author: joshgav
manager: carmonm
ms.assetid: 2ec7f809-5e1a-41cf-9fcd-d0ed4bebd08c
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: get-started-article
ms.date: 05/01/2017
ms.author: cfreeman
ms.translationtype: Human Translation
ms.sourcegitcommit: 64bd7f356673b385581c8060b17cba721d0cf8e3
ms.openlocfilehash: 76a8025cd2a67533beb321c88e924517c1977dfc
ms.contentlocale: de-de
ms.lasthandoff: 05/02/2017


---

# <a name="monitor-your-nodejs-services-and-apps-with-application-insights"></a>Überwachen Ihrer Node.js-Dienste und -Apps mit Application Insights

Mit [Azure Application Insights](app-insights-overview.md) werden Ihre Back-End-Dienste und -Komponenten überwacht, nachdem Sie sie bereitgestellt haben, damit Sie [Leistungsprobleme und andere Probleme erkennen und schnell diagnostizieren können](app-insights-detect-triage-diagnose.md). Nutzen Sie die Anwendung für Node.js-Dienste, die an beliebigen Orten gehostet werden: in Ihrem Datencenter, auf Azure-VMs und in Azure-Web-Apps und sogar in anderen öffentlichen Clouds.

Wenn Sie Ihre Überwachungsdaten empfangen, speichern und untersuchen möchten, können Sie die folgende Anleitung befolgen, um einen Agent in Ihren Code einzufügen und in Azure eine entsprechende Application Insights-Ressource festzulegen. Der Agent sendet Daten zur weiteren Analyse und Untersuchung an diese Ressource.

Der Node.js-Agent kann ein- und ausgehende HTTP-Anforderungen, mehrere Systemmetriken sowie Ausnahmen automatisch überwachen. Ab v0.20 können auch einige gängige Drittanbieterpakete überwacht werden, z.B. `mongodb`, `mysql` und `redis`. Alle Ereignisse, die sich auf eine eingehende HTTP-Anforderung beziehen, werden zur Beschleunigung der Problembehandlung korreliert.

Sie können weitere Aspekte Ihrer App und Ihres Systems überwachen, indem Sie diese mit der weiter unten beschriebenen Agent-API instrumentieren.

![Beispiel für Leistungsüberwachungsdiagramm](./media/app-insights-nodejs/10-perf.png)

## <a name="getting-started"></a>Erste Schritte

Hier wird das Einrichten der Überwachung für eine App oder einen Dienst Schritt für Schritt beschrieben.

### <a name="resource"></a> Einrichten einer App Insights-Ressource

Stellen Sie **zuerst** sicher, dass Sie über ein Azure-Abonnement verfügen, oder [beschaffen Sie sich kostenlos ein neues Abonnement][azure-free-offer]. Falls Ihre Organisation bereits über ein Azure-Abonnement verfügt, kann Ihr Administrator Sie anhand [dieser Anleitung][add-aad-user] hinzufügen.

[azure-free-offer]: https://azure.microsoft.com/en-us/free/
[add-aad-user]: https://docs.microsoft.com/en-us/azure/active-directory/active-directory-users-create-azure-portal

Melden Sie sich nun am [Azure-Portal][portal] an, und erstellen Sie wie unten dargestellt eine Application Insights-Ressource: Klicken Sie auf „Neu“ > „Entwicklertools“ > „Application Insights“. Die Ressource enthält u.a. einen Endpunkt zum Empfangen von Telemetriedaten, Speicherplatz für diese Daten, gespeicherte Berichte und Dashboards, eine Regel- und Warnungskonfiguration.

![Erstellen einer App Insights-Ressource](./media/app-insights-nodejs/03-new_appinsights_resource.png)

Wählen Sie auf der Seite für die Ressourcenerstellung in der Dropdownliste für den Anwendungstyp die Option „Node.js-Anwendung“. Der App-Typ bestimmt, welche Dashboards und Berichte standardmäßig für Sie erstellt werden. Es gibt aber keinen Grund zur Sorge. Mit jeder App Insights-Ressource können Daten aller Sprachen und Plattformen gesammelt werden.

![Formular für neue App Insights-Ressource](./media/app-insights-nodejs/04-create_appinsights_resource.png)

### <a name="agent"></a> Einrichten des Node.js-Agents

Als Nächstes binden Sie den Agent in Ihre App ein, damit er zum Sammeln von Daten genutzt werden kann.
Beginnen Sie, indem Sie den Instrumentierungsschlüssel Ihrer Ressource (im Folgenden als `ikey` bezeichnet) wie unten gezeigt aus dem Portal kopieren. Im App Insights-System wird dieser Schlüssel zum Zuordnen von Daten zu Ihrer Azure-Ressource verwendet. Sie müssen ihn also in einer Umgebungsvariablen oder im Code angeben, damit er vom Agent genutzt werden kann.  

![Kopieren des Instrumentierungsschlüssels](./media/app-insights-nodejs/05-appinsights_ikey_portal.png)

Fügen Sie als Nächstes die Node.js-Agent-Bibliothek per „package.json“ den Abhängigkeiten Ihrer App hinzu. Führen Sie im Stammordner Ihrer App Folgendes aus:

```bash
npm install applicationinsights --save
```

Nun ist es erforderlich, die Bibliothek explizit im Code zu laden. Da der Agent die Instrumentierung in viele andere Bibliotheken einbettet, sollten Sie sie so früh wie möglich laden – noch vor anderen `require`-Anweisungen. Fügen Sie oben in Ihrer ersten JS-Datei Folgendes hinzu, um zu beginnen:

```javascript
const appInsights = require("applicationinsights");
appInsights.setup("<instrumentation_key>");
appInsights.start();
```

Mit der `setup`-Methode wird der Instrumentierungsschlüssel (und somit die Azure-Ressource) konfiguriert, der standardmäßig für alle nachverfolgten Elemente verwendet werden soll. Rufen Sie `start` nach Abschluss der Konfiguration auf, um mit dem Sammeln und Senden von Telemetriedaten zu beginnen.

Sie können einen „ikey“ auch über die Umgebungsvariable „APPINSIGHTS\_INSTRUMENTATIONKEY“ bereitstellen, anstatt ihn manuell an `setup()` oder `getClient()` zu übergeben. Mit dieser Vorgehensweise können Sie „ikeys“ aus festgelegtem Quellcode heraushalten und unterschiedliche „ikeys“ für unterschiedliche Umgebungen angeben.

Weitere Konfigurationsoptionen sind weiter unten beschrieben.

Sie können den Agent ausprobieren, ohne Telemetriedaten zu senden, indem Sie den Instrumentierungsschlüssel auf eine beliebige Zeichenfolge festlegen, die nicht leer ist.

### <a name="monitor"></a> Überwachen Ihrer App

Der Agent sammelt automatisch Telemetriedaten zur Node.js-Laufzeit und zu einigen Drittanbietermodulen. Verwenden Sie Ihre Anwendung jetzt, um einige dieser Daten zu generieren.

Navigieren Sie anschließend im [Azure-Portal][portal] zu der Application Insights-Ressource, die Sie zuvor erstellt haben, und suchen Sie in der Zeitachse „Übersicht“ nach Ihren ersten Daten. Dies ist in der folgenden Abbildung dargestellt. Klicken Sie durch die Diagramme, um weitere Details anzuzeigen.

![Erste Datenpunkte](./media/app-insights-nodejs/12-first-perf.png)

Klicken Sie auf die Schaltfläche „Anwendungszuordnung“, um die Topologie anzuzeigen, die für Ihre App ermittelt wurde. Dies ist in der folgenden Abbildung dargestellt. Klicken Sie in der Zuordnungsübersicht auf die Komponenten, um weitere Details anzuzeigen.

![Einfache App-Zuordnung](./media/app-insights-nodejs/06-appinsights_appmap.png)

Sie können weitere Informationen zu Ihrer App anzeigen und Probleme beheben, indem Sie die anderen Ansichten verwenden, die im Abschnitt „Untersuchen“ verfügbar sind.

![Abschnitt „Untersuchen“](./media/app-insights-nodejs/07-appinsights_investigate_blades.png)

#### <a name="no-data"></a>Sie sehen keine Daten?

Da der Agent Daten zur Übermittlung in Batches zusammenfasst, kann es eine Weile dauern, bis die Elemente im Portal angezeigt werden. Falls in Ihrer Ressource keine Daten angezeigt werden, können Sie folgende Lösungsmöglichkeiten ausprobieren:

* Nutzen Sie die Anwendung, und führen Sie weitere Aktionen durch, um mehr Telemetriedaten zu generieren.
* Klicken Sie in der Ressourcenansicht des Portals auf **Aktualisieren**. Diagramme werden regelmäßig automatisch aktualisiert, aber durch das Klicken auf die entsprechende Schaltfläche wird dieser Vorgang sofort ausgelöst.
* Stellen Sie sicher, dass die [erforderlichen ausgehenden Ports](app-insights-ip-addresses.md) geöffnet sind.
* Öffnen Sie die Kachel [Suche](app-insights-diagnostic-search.md), und suchen Sie nach bestimmten einzelnen Ereignissen.
* Lesen Sie die [häufig gestellten Fragen][].


## <a name="agent-configuration"></a>Agent-Konfiguration

Hier sind die Konfigurationsmethoden des Agents und die dazugehörigen Standardwerte angegeben.

Legen Sie `.setAutoDependencyCorrelation(true)` fest, um die Ereignisse für einen Dienst vollständig zu korrelieren. Auf diese Weise kann der Agent den Kontext in Node.js über asynchrone Rückrufe hinweg nachverfolgen.

```javascript
const appInsights = require("applicationinsights");
appInsights.setup("<instrumentation_key>")
    .setAutoDependencyCorrelation(false)
    .setAutoCollectRequests(true)
    .setAutoCollectPerformance(true)
    .setAutoCollectExceptions(true)
    .setAutoCollectDependencies(true)
    .start();
```

## <a name="agent-api"></a>Agent-API

<!-- TODO: Fully document agent API. -->

Eine vollständige Beschreibung der .NET-Agent-API finden Sie [hier](app-insights-api-custom-events-metrics.md).

Sie können alle Anforderungen, Ereignisse, Metriken oder Ausnahmen mit dem Application Insights-Node.js-Client nachverfolgen. Im folgenden Beispiel werden einige verfügbare APIs veranschaulicht.

```javascript
let appInsights = require("applicationinsights");
appInsights.setup().start(); // assuming ikey in env var
let client = appInsights.getClient();

client.trackEvent("my custom event", {customProperty: "custom property value"});
client.trackException(new Error("handled exceptions can be logged with this method"));
client.trackMetric("custom metric", 3);
client.trackTrace("trace message");

let http = require("http");
http.createServer( (req, res) => {
  client.trackRequest(req, res); // Place at the beginning of your request handler
});
```

### <a name="track-your-dependencies"></a>Nachverfolgen Ihrer Abhängigkeiten

```javascript
let appInsights = require("applicationinsights");
let client = appInsights.getClient();

var success = false;
let startTime = Date.now();
// execute dependency call here....
let duration = Date.now() - startTime;
success = true;

client.trackDependency("dependency name", "command name", duration, success);
```

### <a name="add-a-custom-property-to-all-events"></a>Hinzufügen einer benutzerdefinierten Eigenschaft zu allen Ereignissen

```javascript
appInsights.client.commonProperties = {
    environment: process.env.SOME_ENV_VARIABLE
};
```

### <a name="track-http-get-requests"></a>Nachverfolgen von HTTP GET-Anforderungen

```javascript
var server = http.createServer((req, res) => {
    if ( req.method === "GET" ) {
            appInsights.client.trackRequest(req, res);
    }
    // other work here....
    res.end();
});
```

### <a name="track-server-startup-time"></a>Nachverfolgen der Serverstartzeit

```javascript
let start = Date.now();
server.on("listening", () => {
    let duration = Date.now() - start;
    appInsights.client.trackMetric("server startup time", duration);
});
```

## <a name="more-resources"></a>Weitere Ressourcen

* [Überwachen Ihrer Telemetriedaten im Portal](app-insights-dashboards.md)
* [Schreiben von Analytics-Abfragen über Ihre Telemetriedaten](app-insights-analytics-tour.md)

<!--references-->

[portal]: https://portal.azure.com/
[häufig gestellten Fragen]: app-insights-troubleshoot-faq.md

