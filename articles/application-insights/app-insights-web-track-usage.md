---
title: "Verwendungsanalyse für Webanwendungen mit Azure Application Insights | Microsoft-Dokumentation"
description: "Übersicht über die Verwendungsanalyse mit Application Insights"
services: application-insights
documentationcenter: 
author: alancameronwills
manager: carmonm
ms.assetid: bbdb8e58-7115-48d8-93c0-f69a1beeea6e
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: multiple
ms.topic: article
ms.date: 04/12/2017
ms.author: awills
translationtype: Human Translation
ms.sourcegitcommit: 0c4554d6289fb0050998765485d965d1fbc6ab3e
ms.openlocfilehash: 2715207e548fc57b1896f5736731be3881256cbf
ms.lasthandoff: 04/13/2017


---
# <a name="usage-analysis-for-web-applications-with-application-insights"></a>Verwendungsanalyse für Webanwendungen mit Application Insights
Wenn Sie wissen, wie Benutzer Ihre Anwendung verwenden, können Sie Ihre Entwicklungsarbeit auf die Szenarien konzentrieren, die für die Benutzer am wichtigsten sind. Außerdem verschaffen Sie sich Einblick in die Ziele, die für Benutzer einfacher oder schwieriger zu erreichen sind.

[Azure Application Insights](app-insights-overview.md) bietet Nutzungsverfolgung auf zwei Ebenen:

* **Daten zu Benutzern, Sitzungen und Seitenaufrufen** – direkt bereitgestellt.  
* **Benutzerdefinierte Telemetrie** – Sie [schreiben Code](app-insights-api-custom-events-metrics.md), um die Nutzung Ihrer App zu verfolgen. 


## <a name="get-started"></a>Erste Schritte

Sie erzielen optimale Ergebnisse, wenn Sie Application Insights im Servercode der App und in den Webseiten installieren. Die Client- und die Serverkomponente der App senden Telemetriedaten zur Analyse an das Azure-Portal.

1. **Servercode:** Installieren Sie das entsprechende Modul für die [ASP.NET](app-insights-asp-net.md)-, [Azure](app-insights-azure.md)-, [Java](app-insights-java-get-started.md)-, [Node.js](app-insights-nodejs.md)-App oder einen [anderen Typ](app-insights-platforms.md) von App.

    * *Sie möchten keinen Servercode installieren? [Erstellen Sie einfach eine Azure Application Insights-Ressource](app-insights-create-new-resource.md).*

2. **Webseitencode:** Öffnen Sie das [Azure-Portal](https://portal.azure.com), öffnen Sie die Application Insights-Ressource für die App, und öffnen Sie dann **Erste Schritte > Überwachung und Diagnose der clientseitigen Anwendung**. 

    ![Kopieren Sie das Skript in die Kopfzeile der Masterwebseite.](./media/app-insights-web-track-usage/02-monitor-web-page.png)


3. **Abrufen von Telemetriedaten:** Führen Sie das Projekt einige Minuten lang im Debugmodus aus, und suchen Sie dann im Blatt „Übersicht“ von Application Insights nach Ergebnissen.

    Veröffentlichen Sie die App, um die Leistung der App zu überwachen und zu ermitteln, was die Benutzer mit Ihrer App tun.

## <a name="usage-analysis-out-of-the-box"></a>Vorkonfigurierte Verwendungsanalyse
Öffnen Sie das Blatt „Nutzung“.

![Diagramme zu Benutzern, Sitzungen und Seitenaufrufen](./media/app-insights-web-track-usage/14-usage.png)

Bewegen Sie den Mauszeiger im dem leeren Bereich über einem Diagramm, um die Anzahlen an einem bestimmten Punkt anzuzeigen. Andernfalls weisen die Zahlen den Wert aus, der über den Zeitraum aggregiert wurde, z. B. einen Mittelwert, eine Summe oder Anzahl eindeutiger Benutzer im jeweiligen Zeitraum.

Was wird in diesen Diagrammen angezeigt?

* **Benutzer:** Die Anzahl der unterschiedlichen aktiven Benutzer über den Zeitbereich des Diagramms. In Webanwendungen werden Benutzer mithilfe von Cookies gezählt. Eine Person, die mehrere Browser nutzt, Cookies löscht oder die Datenschutzfunktion nutzt, wird mehrere Male gezählt.
* **Authentifizierte Benutzer** werden gezählt, wenn Sie im Code einen Aufruf von [setAuthenticatedUser()](app-insights-api-custom-events-metrics.md#authenticated-users) eingefügt haben.
* **Sitzungen:** Die Anzahl der aktiven Sitzungen. Eine Web-Sitzung wird nach 30 Minuten Inaktivität gezählt. 
* **Seitenaufrufe** Zählt die Anzahl der Aufrufe von trackPageView(), wird in der Regel einmal auf jeder Webseite aufgerufen.

Klicken Sie auf ein Diagramm, um weitere Details anzuzeigen. Beachten Sie, dass Sie den Zeitraum der Diagramme ändern können.

### <a name="where-do-my-users-live"></a>Wo wohnen meine Benutzer?
Klicken Sie auf einem leeren Bereich des Diagramms „Benutzer“, um ein weiteres Blatt zu öffnen, in dem weitere Details angezeigt werden:

![Klicken Sie auf dem Blatt „Verwendung“ auf das Diagramm „Benutzer“.](./media/app-insights-web-track-usage/02-sessions.png)

### <a name="what-browsers-or-operating-systems-do-they-use"></a>Welche Browser oder Betriebssysteme verwenden sie?

Klicken Sie im Diagramm „Benutzer“ auf **Bearbeiten**, und gruppieren (segmentieren) Sie Daten anhand einer Eigenschaft, z.B. Browser, Betriebssystem oder Ort: 

![Wählen Sie ein Diagramm mit einer einzelnen Metrik, schalten Sie "Gruppierung" ein, und wählen Sie eine Eigenschaft](./media/app-insights-web-track-usage/03-browsers.png)

Um den vollständigen Satz von Zahlen anzuzeigen, ändern Sie den Diagrammtyp in **Raster**. Sie können auch zusätzliche Metriken anzeigen:

![Mehrspaltiges Rasterdiagramm](./media/app-insights-web-track-usage/multi-column-grid.png)

Sie können für grafische Diagrammtypen entweder nach einer Eigenschaft gruppieren oder mehrere Metriken auswählen, aber nicht beide Optionen gleichzeitig nutzen. In diesem Diagramm werden die beiden Metriken „Benutzer“ und [„authentifizierte Benutzer“](app-insights-api-custom-events-metrics.md#authenticated-users) verglichen. 

![Wählen Sie ein Diagramm aus, suchen Sie Metriken, und aktivieren oder deaktivieren Sie diese.](./media/app-insights-web-track-usage/031-dual.png)



## <a name="page-views"></a>Seitenaufrufe
Klicken Sie sich auf dem Blatt „Nutzung“ durch das Diagramm der Seitenaufrufe, um eine detaillierte Version sowie eine Aufschlüsselung Ihrer beliebtesten Seiten zu erhalten:

![Klicken Sie auf dem Blatt "Übersicht" auf das Diagramm "Seitenaufrufe"](./media/app-insights-web-track-usage/05-games.png)

Das vorherige Beispiel gehört zu einer Spielewebsite. Anhand der Diagramme lässt sich sofort Folgendes erkennen:

* Die Nutzung hat sich in der Vorwoche nicht erhöht. Vielleicht sollten wir über eine Suchmaschinenoptimierung nachdenken?
* „Tennis“ ist die beliebteste Spieleseite. Konzentrieren wir uns auf weitere Verbesserungen dieser Seite.
* Die Seite „Tennis“ wird im Durchschnitt dreimal pro Woche besucht. (Es gibt ungefähr dreimal mehr Sitzungen als Benutzer.)
* Die meisten Benutzer besuchen die Website an Arbeitstagen in den USA, und zwar während der Arbeitszeit. Vielleicht sollten wir auf der Website eine Schaltfläche für schnelles Ausblenden bereitstellen.
* Die [Anmerkungen](app-insights-annotations.md) im Diagramm geben an, wenn neue Versionen der Website bereitgestellt wurden. Keine der neuesten Bereitstellungen hatte eine spürbare Auswirkung auf die Nutzung.

## <a name="custom-tracking"></a>Benutzerdefinierte Nachverfolgung
Lassen Sie uns einmal Folgendes annehmen. Anstatt jedes Spiel in einer eigenen Webseite zu implementieren, gestalten Sie den Code so um, dass alle in der App auf einer Seite angezeigt werden, wobei der Großteil der Funktionalität als JavaScript in die Website programmiert wird. Dies ermöglicht Benutzern ein schnelles Wechseln zwischen Spielen oder sogar die Anzeige mehrerer Spiele auf einer Seite.

Aber dennoch soll Application Insights weiter protokollieren, wie oft jedes Spiel geöffnet wird, und zwar auf dieselbe Weise wie bei den getrennten Webseiten. Das ist ganz einfach: Fügen Sie einfach in Ihr JavaScript einen Aufruf des Telemetriemoduls an der Stelle ein, an der aufgezeichnet werden soll, dass eine neue Seite geöffnet wurde:

```JavaScript
    telemetryClient.trackPageView(game.Name);
```
Dieser Aufruf simuliert die Telemetriefunktion zum Protokollieren eines Seitenaufrufs.  Sie möchten jedoch Nachrichten und Seitenaufrufe auseinanderhalten können. Verwenden Sie stattdessen benutzerdefinierte Ereignisse. Sie können diese von Webseiten oder einem Webserver senden:


```JavaScript

    telemetryClient.trackEvent("GameEnd");
```

```C#
    var tc = new Microsoft.ApplicationInsights.TelemetryClient();
    tc.TrackEvent("GameEnd");
```

```VB

    Dim tc = New Microsoft.ApplicationInsights.TelemetryClient()
    tc.TrackEvent("GameEnd")
```

[Benutzerdefinierte Telemetrie, die in den Web- oder Servercode eingefügt wird](app-insights-api-custom-events-metrics.md#trackevent), kann auf vielfältige Weise verwendet werden, um zu ermitteln, wie Ihre Anwendung verwendet wird.

So zeigen Sie von „TrackEvent()“ gesendete Ereignisse an: Öffnen Sie den Metrik-Explorer, fügen Sie ein neues Diagramm hinzu, und bearbeiten Sie es. Die Metriken werden unter „Benutzerdefinierte Metriken“ angezeigt. 

![Ein Diagramm, in dem benutzerdefinierte Ereignisse angezeigt werden.](./media/app-insights-web-track-usage/06-eventsSegment.png)

Wenn Sie in den Ereignissen [Eigenschaftswerte](app-insights-api-custom-events-metrics.md#properties) (auch als Dimensionen bezeichnet) festlegen, können Sie nach diesen gruppieren und filtern.

Erstellen Sie mehrere Diagramme, um Änderungen in anderen Metriken und Ereignissen zu korrelieren. Zu Zeiten, in denen mehr Spiele gespielt werden, erwarten Sie z. B. auch einen Anstieg abgebrochener Spiele. Aber wenn der Anstieg abgebrochener Spiele unverhältnismäßig ist, möchten Sie herausfinden, ob die hohe Auslastung Probleme verursacht, die Benutzer nicht akzeptabel finden.

## <a name="drill-into-specific-events"></a>Detailsuche bei bestimmten Ereignissen
Um besser zu verstehen, wie eine typische Sitzung abläuft, wechselt zu erhalten, wird empfohlen, sich auf eine bestimmte Benutzersitzung konzentrieren, die einen bestimmten Typ von Ereignis aufweist.

Klicken Sie sich in einem bestimmten Raster von Ereignissen durch das Ereignis von Interesse, und wählen Sie ein aktuelles spezifisches Vorkommen aus:

![Klicken Sie in der Liste unter dem Übersichtsdiagramm auf ein Ereignis](./media/app-insights-web-track-usage/08-searchEvents.png)

Lassen Sie uns nun alle Telemetriedaten für die Sitzung anschauen, in der dieses bestimmte "NoGame"-Ereignis aufgetreten ist.

![Klicken Sie auf "Gesamte Telemetrie für Sitzung"](./media/app-insights-web-track-usage/09-relatedTelemetry.png)

Es gab keine Ausnahmen, sodass der Benutzer nicht durch einen Fehler am Spielen gehindert wurde.

Wir können bis auf Seitenaufrufe alle Arten von Telemetriedaten für diese Sitzung herausfiltern:

![](./media/app-insights-web-track-usage/10-filter.png)

Jetzt können wir erkennen, dass sich dieser Benutzer nur angemeldet, um die neuesten Punktstände zu überprüfen. Vielleicht sollten wir eine User Story entwickeln, die dies vereinfacht. (Und wir sollten ein benutzerdefiniertes Ereignis implementieren, das meldet, wenn diese bestimmte User Story auftritt.)

## <a name="filter-search-and-segment-your-data-with-properties"></a>Filtern, Suchen und Segmentieren der Daten mit Eigenschaften
Sie können beliebige Tags und numerische Werte an Ereignisse anfügen.

*JavaScript in Webseiten*

```JavaScript

    appInsights.trackEvent("WinGame",
        // String properties:
        {Game: currentGame.name, Difficulty: currentGame.difficulty},
        // Numeric measurements:
        {Score: currentGame.score, Opponents: currentGame.opponentCount}
    );
```

*C# auf Server*

```C#

    // Set up some properties:
    var properties = new Dictionary <string, string>
        {{"game", currentGame.Name}, {"difficulty", currentGame.Difficulty}};
    var measurements = new Dictionary <string, double>
        {{"Score", currentGame.Score}, {"Opponents", currentGame.OpponentCount}};

    // Send the event:
    telemetry.TrackEvent("WinGame", properties, measurements);
```

*VB auf Server*

```VB

    ' Set up some properties:
    Dim properties = New Dictionary (Of String, String)
    properties.Add("game", currentGame.Name)
    properties.Add("difficulty", currentGame.Difficulty)

    Dim measurements = New Dictionary (Of String, Double)
    measurements.Add("Score", currentGame.Score)
    measurements.Add("Opponents", currentGame.OpponentCount)

    ' Send the event:
    telemetry.TrackEvent("WinGame", properties, measurements)
```

Fügen Sie Eigenschaften an Seitenaufrufe auf die gleiche Weise an:

*JavaScript in Webseiten*

```JS

    appInsights.trackPageView("Win", 
        url,
        {Game: currentGame.Name}, 
        {Score: currentGame.Score});
```

Zeigen Sie auf dem Blatt "Diagnosesuche" die Eigenschaften an, indem Sie sich durch ein einzelnes Vorkommen eines Ereignisses klicken.

![Öffnen Sie in der Liste der Ereignisse ein Ereignis, und klicken Sie dann auf „...“, um weitere Eigenschaften anzuzeigen.](./media/app-insights-web-track-usage/11-details.png)

Verwenden Sie das Suchfeld, um Ereignisvorkommen mit einem bestimmten Eigenschaftswert anzuzeigen.

![Geben Sie einen Wert in das Suchfeld ein](./media/app-insights-web-track-usage/12-searchEvents.png)

## <a name="edit-and-create-queries-over-your-telemetry"></a>Bearbeiten und Erstellen von Abfragen über Ihre Telemetriedaten

Klicken Sie in einem Diagramm auf das Symbol „Azure Analytics“, um eine entsprechende Abfrage zu öffnen, die Sie bearbeiten können.
Scrollen Sie nach unten, um zu überprüfen, ob mehrere generierte Abfragen vorhanden sind. Setzen Sie den Cursor in eine Abfrage, und klicken Sie auf **Gehe zu**. 

Alternativ können Sie Analytics über das Symbol auf dem Blatt „Übersicht“ öffnen und eigene Abfragen schreiben, oder führen Sie einige der Beispielabfragen auf der Registerkarte „Start“ von Analytics aus.


![Analytics-Fenster mit generierter Abfrage](./media/app-insights-web-track-usage/open-analytics.png)

[Weitere Informationen über die Azure Analytics-Abfragesprache](app-insights-analytics.md).

Für die Verwendungsanalyse sind die folgenden Tabellen möglicherweise hilfreich:

* `customEvents` – Ergebnisse von [TrackEvent()](app-insights-api-custom-events-metrics.md#trackevent)-Aufrufen.
* `pageViews` – Anzahl der in Clientbrowsern geöffneten Seiten oder Aufrufe von [trackPageView()](app-insights-api-custom-events-metrics.md#page-views).


## <a name="a--b-testing"></a>A-B-Tests
Wenn Sie nicht wissen, welche Variante eines Features erfolgreicher sein wird, veröffentlichten Sie beide, und ermöglichen Sie verschiedenen Benutzern den Zugriff darauf. Messen Sie den jeweiligen Erfolg, und erstellen Sie anschließend eine vereinheitlichte Version.

Für dieses Verfahren fügen Sie unterschiedliche Tags an alle Telemetriedaten an, die von jeder Version Ihrer App gesendet wird. Dazu definieren Sie Eigenschaften im aktiven "TelemetryContext"-Element. Diese Standardeigenschaften werden jeder Telemetrienachricht hinzugefügt, die die Anwendung sendet, nicht nur Ihren benutzerdefinierten Nachrichten, sondern auch den Standardtelemetriedaten.

Im Application Insights-Portal können Sie anschließend Ihre Daten anhand der Tags filtern und gruppieren (segmentieren), um die verschiedenen Versionen zu vergleichen.


```C#

    using Microsoft.ApplicationInsights.DataContracts;

    var context = new TelemetryContext();
    context.Properties["Game"] = currentGame.Name;
    var telemetry = new TelemetryClient(context);
    // Now all telemetry will automatically be sent with the context property:
    telemetry.TrackEvent("WinGame");
```


```VB

    Dim context = New TelemetryContext
    context.Properties("Game") = currentGame.Name
    Dim telemetry = New TelemetryClient(context)
    ' Now all telemetry will automatically be sent with the context property:
    telemetry.TrackEvent("WinGame")
```

Einzelne Telemetriedaten können die Standardwerte außer Kraft setzen.

Sie können einen universellen Initialisierer einrichten, damit alle neuen "TelemetryClient"-Elemente automatisch Ihren Kontext verwenden.

```C#


    // Telemetry initializer class
    public class MyTelemetryInitializer : ITelemetryInitializer
    {
        public void Initialize (ITelemetry telemetry)
        {
            telemetry.Properties["AppVersion"] = "v2.1";
        }
    }
```

Gehen Sie im App-Initialisierer wie z. B. "Global.asax.cs" so vor:

```C#

    protected void Application_Start()
    {
        // ...
        TelemetryConfiguration.Active.TelemetryInitializers
        .Add(new MyTelemetryInitializer());
    }
```


## <a name="build---measure---learn"></a>Erstellen – Messen – Lernen
Wenn Sie mit Analysen arbeiten, werden diese zu einem integrierten Bestandteil Ihres Entwicklungszyklus und nicht bloß zu etwas, an das Sie denken, wenn es gilt, Probleme zu lösen. Hier einige Tipps:

* Bestimmen Sie die wichtigste Metrik Ihrer Anwendung. Wünschen Sie sich so viele Benutzer wie möglich oder lieber eine kleine Gruppe sehr zufriedener Benutzer? Möchten Sie Websitebesuche oder den Umsatz maximieren?
* Planen Sie eine Messung für jede User Story. Wenn Sie eine neue User Story oder Funktion entwerfen oder eine vorhandene aktualisieren wollen, denken Sie stets daran, wie Sie den Erfolg der Änderung messen möchten. Fragen Sie sich vor Beginn der Programmierung "Welche Auswirkungen hat dies auf unsere Metriken, wenn es funktioniert? Sollten wir neue Ereignisse nachverfolgen?"
  Und wenn das Feature dann online ist, sehen Sie sich die Analyse an, und reagieren Sie auf die Ergebnisse.
* Setzen Sie die anderen Metriken in Beziehung zur Hauptmetrik. Wenn Sie z. B. das Feature "Favoriten" hinzufügen, möchten Sie bestimmt wissen, wie oft Benutzer Favoriten hinzufügen. Aber vielleicht ist es noch interessanter zu wissen, wie oft sie zu ihren Favoriten zurückkehren. Und am wichtigsten ist, ob Benutzer, die Favoriten nutzen, letztendlich mehr von Ihrem Produkt kaufen.
* Testen Sie mit begrenzten Benutzergruppen. Richten Sie einen Feature-Umschalter ein, der es Ihnen ermöglicht, ein neues Feature nur für einige Benutzer sichtbar zu machen. Verwenden Sie Application Insights, um festzustellen, ob das neue Feature so wie vorgesehen genutzt wird. Nehmen Sie Anpassungen vor, und veröffentlichen Sie es für ein breiteres Publikum.
* Sprechen Sie Ihre Benutzer an! Analysen allein genügen nicht, sondern dienen zur Aufrechterhaltung einer guten Kundenbeziehung.

## <a name="learn-more"></a>Weitere Informationen
* [Erkennung, Eingrenzung und Diagnose von Abstürzen und Leistungsproblemen in Ihrer App](app-insights-detect-triage-diagnose.md)
* [Erste Schritte mit Application Insights auf vielen Plattformen](app-insights-detect-triage-diagnose.md)
* [Verwenden der API – Übersicht](app-insights-api-custom-events-metrics.md)
* [JavaScript-API-Referenz](https://github.com/Microsoft/ApplicationInsights-JS/blob/master/API-reference.md)



