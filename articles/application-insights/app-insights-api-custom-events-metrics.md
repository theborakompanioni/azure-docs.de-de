---
title: "Application Insights-API für benutzerdefinierte Ereignisse und Metriken | Microsoft Docs"
description: "Fügen Sie einige Codezeilen in Ihrer Geräte- oder Desktop-App, Webseite oder dem Webdienst ein, um Nutzungs- und Diagnoseprobleme nachzuverfolgen."
services: application-insights
documentationcenter: 
author: alancameronwills
manager: douge
ms.assetid: 80400495-c67b-4468-a92e-abf49793a54d
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: multiple
ms.topic: article
ms.date: 11/16/2016
ms.author: awills
translationtype: Human Translation
ms.sourcegitcommit: 7bd26ffdec185a1ebd71fb88383c2ae4cd6d504f
ms.openlocfilehash: 460b9e3cf2b4c7a85895c2db872344a1a5bc0ff9


---
# <a name="application-insights-api-for-custom-events-and-metrics"></a>Application Insights-API für benutzerdefinierte Ereignisse und Metriken


Fügen Sie einige Codezeilen in Ihre Anwendung ein, um herauszufinden, wie sie von Benutzern eingesetzt wird, oder um Probleme zu diagnostizieren. Sie können Telemetriedaten von Geräte- und Desktop-Apps, Webclients und Webservern senden. Mit der [Azure Application Insights](app-insights-overview.md)-Kerntelemetrie-API können Sie benutzerdefinierte Ereignisse und Metriken und Ihre eigenen Versionen der standardmäßigen Telemetrie senden. Dies ist die gleiche API, die von den standardmäßigen Application Insights-Datensammlern verwendet wird.

## <a name="api-summary"></a>API-Zusammenfassung
Die API ist, abgesehen von ein paar kleinen Variationen, auf allen Plattformen einheitlich.

| Methode | Verwendung |
| --- | --- |
| [`TrackPageView`](#page-views) |Seiten, Bildschirme, Blätter oder Formulare. |
| [`TrackEvent`](#track-event) |Benutzeraktionen und andere Ereignisse. Wird zum Verfolgen des Benutzerverhaltens oder zur Leistungsüberwachung eingesetzt. |
| [`TrackMetric`](#track-metric) |Leistungsmessungen wie Warteschlangenlängen, die nicht im Zusammenhang mit bestimmten Ereignissen stehen. |
| [`TrackException`](#track-exception) |Protokollieren von Ausnahmen für die Diagnose. Verfolgen Sie, wo diese in Bezug auf andere Ereignisse auftreten, und untersuchen Sie die Stapelüberwachung. |
| [`TrackRequest`](#track-request) |Protokollieren Sie die Häufigkeit und Dauer der Serveranforderungen für die Leistungsanalyse. |
| [`TrackTrace`](#track-trace) |Diagnoseprotokollnachrichten. Sie können auch Drittanbieterprotokolle erfassen. |
| [`TrackDependency`](#track-dependency) |Protokollieren Sie die Dauer und die Häufigkeit der Aufrufe von externen Komponenten, von denen Ihre Anwendung abhängt. |

Sie können den meisten dieser Telemetrieaufrufe [Eigenschaften und Metriken anfügen](#properties) .

## <a name="a-nameprepabefore-you-start"></a><a name="prep"></a>Vorbereitung
Falls noch nicht geschehen:

* Fügen Sie Ihrem Projekt das Application Insights-SDK hinzu:

  * [ASP.NET-Projekt][greenbrown]
  * [Java-Projekt][java]
  * [JavaScript auf jeder Webseite][client]   
* Schließen Sie Folgendes in den Geräte- oder Webservercode ein:

    *C#:* `using Microsoft.ApplicationInsights;`

    *VB:* `Imports Microsoft.ApplicationInsights`

    *Java:* `import com.microsoft.applicationinsights.TelemetryClient;`

## <a name="construct-a-telemetryclient"></a>Erstellen eines TelemetryClient
Erstellen Sie eine Instanz von TelemetryClient (außer in JavaScript auf Webseiten):

*C#:*

    private TelemetryClient telemetry = new TelemetryClient();

*VB:*

    Private Dim telemetry As New TelemetryClient

*Java*

    private TelemetryClient telemetry = new TelemetryClient();

TelemetryClient ist threadsicher.

Wir empfehlen Ihnen, für jedes Modul Ihrer App eine Instanz des `TelemetryClient` zu verwenden. Zum Beispiel können Sie einen `TelemetryClient` in Ihrem Webdienst haben, der eingehende HTTP-Anforderungen meldet, und einen anderen in einer Middleware-Klasse, der Geschäftslogikereignisse meldet. Sie können Eigenschaften festlegen wie `TelemetryClient.Context.User.Id` zum Nachverfolgen von Benutzern und Sitzungen oder `TelemetryClient.Context.Device.Id` zum Identifizieren des Computers. Diese Informationen werden allen Ereignissen angefügt, die von der Instanz gesendet werden.

## <a name="track-event"></a>Nachverfolgen von Ereignissen
In Application Insights handelt es sich bei einem *benutzerdefinierten Ereignis* um einen Datenpunkt, den Sie im [Metrik-Explorer][metrics] als aggregierte Anzahl und in der [Diagnosesuche][diagnostic] als einzelne Vorkommen anzeigen können. (Er gehört nicht zu MVC oder anderen Ereignissen des Frameworks.)

Fügen Sie TrackEvent-Aufrufe in Ihren Code ein, um zu zählen, wie oft Benutzer ein bestimmtes Feature auswählen, bestimmte Ziele erreichen oder vielleicht bestimmte Arten von Fehlern machen.

Senden Sie z. B. in einer Spiele-App ein Ereignis, sobald ein Benutzer das Spiel gewinnt:

*JavaScript*

    appInsights.trackEvent("WinGame");

*C#*

    telemetry.TrackEvent("WinGame");

*VB*

    telemetry.TrackEvent("WinGame")

*Java*

    telemetry.trackEvent("WinGame");


### <a name="view-your-events-in-the-azure-portal"></a>Anzeigen der Ereignisse im Azure-Portal
Um die Anzahl der Ereignisse anzuzeigen, öffnen Sie das Blatt [Metrik-Explorer](app-insights-metrics-explorer.md) , fügen Sie ein neues Diagramm hinzu und wählen Sie die Ereignisse aus.  

![](./media/app-insights-api-custom-events-metrics/01-custom.png)

Um die Anzahl der verschiedene Ereignisse zu vergleichen, legen Sie den Diagrammtyp auf „Raster“ fest und gruppieren Sie nach Ereignisnamen:

![](./media/app-insights-api-custom-events-metrics/07-grid.png)

Klicken Sie im Raster auf einen Ereignisnamen, um einzelne Vorkommen dieses Ereignisses anzuzeigen.

![Führen Sie ein Drillthrough für die Ereignisse aus.](./media/app-insights-api-custom-events-metrics/03-instances.png)

Klicken Sie auf ein beliebiges Vorkommen, um weitere Details anzuzeigen.

Um sich entweder unter Search oder im Metrik-Explorer bestimmte Ereignisse zu konzentrieren, legen Sie den Filter des Blattes auf die Ereignisnamen fest, an denen Sie interessiert sind:

![Öffnen Sie Filter, erweitern Sie den Ereignisnamen und wählen Sie einen oder mehrere Werte.](./media/app-insights-api-custom-events-metrics/06-filter.png)

## <a name="track-metric"></a>TrackMetric
Verwenden Sie TrackMetric zum Senden von Metriken, die keinen bestimmten Ereignissen zugeordnet sind. Beispielsweise könnten Sie die Länge einer Warteschlange in regelmäßigen Abständen überwachen.

Metriken werden als statistische Diagramme im Metrik-Explorer angezeigt. Im Gegensatz zu Ereignissen können Sie jedoch nicht nach einzelnen Vorkommen in der Diagnosesuche suchen.

Metrikwerte müssen >=0 sein, um richtig angezeigt zu werden.

*JavaScript*

    appInsights.trackMetric("Queue", queue.Length);

*C#*

    telemetry.TrackMetric("Queue", queue.Length);

*VB*

    telemetry.TrackMetric("Queue", queue.Length)

*Java*

    telemetry.trackMetric("Queue", queue.Length);

Tatsächlich können Sie dies in einem Hintergrundthread ausführen:

*C#*

    private void Run() {
     var appInsights = new TelemetryClient();
     while (true) {
      Thread.Sleep(60000);
      appInsights.TrackMetric("Queue", queue.Length);
     }
    }


Um die Ergebnisse anzuzeigen, öffnen Sie den Metrik-Explorer, und fügen Sie ein neues Diagramm hinzu. Legen Sie es für die Anzeige Ihrer Metrik fest.

![Fügen Sie ein neues Diagramm hinzu, oder wählen Sie ein Diagramm aus, und wählen Sie Ihre Metrik unter "Benutzerdefiniert" aus.](./media/app-insights-api-custom-events-metrics/03-track-custom.png)

Es gibt einige [Beschränkungen hinsichtlich der Anzahl von Metriken](#limits) , die verwendet werden können.

## <a name="page-views"></a>Seitenaufrufe
In einer Geräte- oder Webseiten-App werden die Telemetriedaten zu den Seitenaufrufen standardmäßig gesendet, wenn die einzelnen Bildschirme oder Seiten geladen werden. Sie können dies jedoch so ändern, dass Seitenaufrufe zu zusätzlichen oder anderen verfolgt werden. Angenommen, Sie möchten in einer App mit Registerkarten oder Blättern eine "Seite" nachverfolgen, sobald der Benutzer ein neues Blatt öffnet.

![Lupe für Nutzung auf dem Blatt "Übersicht"](./media/app-insights-api-custom-events-metrics/appinsights-47usage-2.png)

Benutzer- und Sitzungsdaten werden als Eigenschaften zusammen mit Seitenaufrufen gesendet, damit die Benutzer- und Sitzungsdiagramme aktiv werden, sobald Telemetriedaten zu den Seitenaufrufen vorliegen.

#### <a name="custom-page-views"></a>Benutzerdefinierte Seitenaufrufe
*JavaScript*

    appInsights.trackPageView("tab1");

*C#*

    telemetry.TrackPageView("GameReviewPage");

*VB*

    telemetry.TrackPageView("GameReviewPage")


Wenn Sie mehrere Registerkarten in anderen HTML-Seiten haben, können Sie ebenfalls die URL angeben:

    appInsights.trackPageView("tab1", "http://fabrikam.com/page1.htm");

#### <a name="timing-page-views"></a>Zeitliches Einteilen von Seitenaufrufen
In der Standardeinstellung werden die gemeldeten Zeiten für „Ladezeit der Seitenansicht“ von dem Zeitpunkt, zu dem der Browser die Anforderung sendet, bis zum Aufruf des Seitenladeereignisses im Browser gemessen.

Sie haben stattdessen noch folgende Möglichkeiten:

* Legen Sie im [trackPageView](https://github.com/Microsoft/ApplicationInsights-JS/blob/master/API-reference.md#trackpageview) -Aufruf eine explizite Dauer fest.
  * `appInsights.trackPageView("tab1", null, null, null, durationInMilliseconds);`
* Verwenden Sie die Zeitsteuerungsaufrufe `startTrackPage` und `stopTrackPage` für die Seitenansicht.

*JavaScript*

    // To start timing a page:
    appInsights.startTrackPage("Page1");

...

    // To stop timing and log the page:
    appInsights.stopTrackPage("Page1", url, properties, measurements);

Der Name, den Sie für den ersten Parameter verwenden, ordnet die Start- und Stoppaufrufe zu. Der Standardwert ist der Name der aktuellen Seite.

Die resultierenden Zeiten für das Laden der Seite, die im Metrik-Explorer angezeigt werden, leiten sich vom Intervall zwischen den Start- und Stoppaufrufen ab. Sie können entscheiden, welches Intervall Sie tatsächlich verwenden.

## <a name="track-request"></a>TrackRequest
Wird vom Server-SDK zum Protokollieren von HTTP-Anforderungen verwendet.

Sie können diese Methode auch selbst aufrufen, wenn Sie Anforderungen in einem Kontext simulieren möchten, in dem das Webdienstmodul nicht ausgeführt wird.

*C#*

    // At start of processing this request:

    // Operation Id and Name are attached to all telemetry and help you identify
    // telemetry associated with one request:
    telemetry.Context.Operation.Id = Guid.NewGuid().ToString();
    telemetry.Context.Operation.Name = requestName;

    var stopwatch = System.Diagnostics.Stopwatch.StartNew();

    // ... process the request ...

    stopwatch.Stop();
    telemetry.TrackRequest(requestName, DateTime.Now,
       stopwatch.Elapsed,
       "200", true);  // Response code, success



## <a name="operation-context"></a>Vorgangskontext
Telemetrieelemente können einander zugeordnet werden, indem sie einer gemeinsamen Vorgangs-ID angefügt werden. Das standardmäßige Anforderungsnachverfolgungs-Modul führt dies für Ausnahmen und andere Ereignisse aus, die beim Verarbeiten einer HTTP-Anforderung gesendet werden. In [Suche](app-insights-diagnostic-search.md) und [Analyse](app-insights-analytics.md) können Sie anhand der ID mühelos der Anforderung zugeordnete Ereignisse finden.

Die einfachste Möglichkeit, die ID festzulegen, ist die Festlegung eines Vorgangskontexts mit diesem Muster:

    // Establish an operation context and associated telemetry item:
    using (var operation = telemetry.StartOperation<RequestTelemetry>("operationName"))
    {
        // Telemetry sent in here will use the same operation ID.
        ...
        telemetry.TrackEvent(...); // or other Track* calls
        ...
        // Set properties of containing telemetry item - for example:
        operation.Telemetry.ResponseCode = "200";

        // Optional: explicitly send telemetry item:
        telemetry.StopOperation(operation);

    } // When operation is disposed, telemetry item is sent.

`StartOperation` legt einen Vorgangskontext fest, erstellt ein Telemetrieelement des von Ihnen angegebenen Typs und sendet dieses, wenn Sie den Vorgang löschen oder explizit `StopOperation` aufrufen. Wenn Sie `RequestTelemetry` als Telemetrietyp verwenden, wird die Dauer („Duration“) auf das Zeitintervall zwischen Start und Stopp festgelegt.

Vorgangskontexte können nicht geschachtelt werden. Wenn bereits ein Vorgangskontext vorhanden ist, wird die Kontext-ID mit allen enthaltenen Elementen verknüpft, einschließlich des mit StartOperation erstellten Elements.

In der Suche wird mit dem Vorgangskontext die Liste „Verwandte Elemente“ erstellt:

![Verwandte Elemente](./media/app-insights-api-custom-events-metrics/21.png)

## <a name="track-exception"></a>TrackException
Senden Sie Ausnahmen an Application Insights, um sie als Hinweis auf die Häufigkeit eines Problems [zu zählen][metrics] und [einzelne Vorkommen zu untersuchen][diagnostic]. Die Berichte enthalten die Stapelüberwachung.

*C#*

    try
    {
        ...
    }
    catch (Exception ex)
    {
       telemetry.TrackException(ex);
    }

*JavaScript*

    try
    {
       ...
    }
    catch (ex)
    {
       appInsights.trackException(ex);
    }

Die SDKs viele Ausnahmen automatisch abfangen, müssen Sie TrackException nicht immer explizit aufrufen.

* ASP.NET: [Schreiben Sie Code zum Abfangen von Ausnahmen](app-insights-asp-net-exceptions.md)
* J2EE: [Ausnahmen werden automatisch abgefangen](app-insights-java-get-started.md#exceptions-and-request-failures)
* JavaScript: Wird automatisch abgefangen. Wenn Sie die automatische Erfassung deaktivieren möchten, fügen Sie eine Zeile dem Codeausschnitt hinzu, den Sie in Ihren Webseiten einfügen:

    ```
    ({
      instrumentationKey: "your key"
      , disableExceptionTracking: true
    })
    ```

## <a name="track-trace"></a>TrackTrace
Verwenden Sie diese Methode zur Diagnose von Problemen, indem Sie eine "Brotkrümelnavigation" an Application Insights senden. Sie können Blöcke von Diagnosedaten senden und sie in der [Diagnosesuche][diagnostic] überprüfen.

[Protokolladapter][trace] verwenden diese API, um Drittanbieterprotokolle an das Portal zu senden.

*C#*

    telemetry.TrackTrace(message, SeverityLevel.Warning, properties);


Sie können nach Nachrichteninhalt suchen, aber (anders als bei Eigenschaftswerten) nicht danach filtern.

Die Größenbeschränkung für `message` liegt wesentlich höher als der Grenzwert für Eigenschaften.
Ein Vorteil von TrackTrace ist, dass relativ lange Daten in die Nachricht eingefügt werden können. Sie können dort z. B. POST-Daten codieren.  

Darüber hinaus können Sie Ihrer Nachricht einen Schweregrad hinzufügen. Wie andere Telemetriedaten können Sie Eigenschaftswerte hinzufügen, die Sie verwenden können, um zu filtern oder nach verschiedene Spuren zu suchen. Beispiel:

    var telemetry = new Microsoft.ApplicationInsights.TelemetryClient();
    telemetry.TrackTrace("Slow database response",
                   SeverityLevel.Warning,
                   new Dictionary<string,string> { {"database", db.ID} });

Auf diese Weise können Sie in der [Suche][diagnostic] problemlos alle Nachrichten eines bestimmten Schweregrads herausfiltern, die im Zusammenhang mit einer bestimmten Datenbank stehen.

## <a name="track-dependency"></a>TrackDependency
Verwenden Sie diesen Aufruf zum Verfolgen der Antwortzeiten und der Erfolgsraten der Aufrufe an eine externe Codepassage. Die Ergebnisse werden in den Abhängigkeitsdiagrammen im Portal angezeigt.

```C#

            var success = false;
            var startTime = DateTime.UtcNow;
            var timer = System.Diagnostics.Stopwatch.StartNew();
            try
            {
                success = dependency.Call();
            }
            finally
            {
                timer.Stop();
                telemetry.TrackDependency("myDependency", "myCall", startTime, timer.Elapsed, success);
            }
```

Beachten Sie, dass die Server-SDKs ein [Abhängigkeitsmodul](app-insights-asp-net-dependencies.md) enthalten, das bestimmte Abhängigkeitsaufrufe automatisch ermittelt und nachverfolgt (etwa an Datenbanken und REST-APIs). Sie müssen einen Agent auf dem Server installieren, damit das Modul funktioniert. Sie können diesen Aufruf verwenden, um Aufrufe zu verfolgen, die durch die automatisierte Nachverfolgung nicht angezeigt werden oder wenn Sie den Agent nicht installieren möchten.

Um das Standardmodul zum Nachverfolgen von Abhängigkeiten zu deaktivieren, bearbeiten Sie [ApplicationInsights.config](app-insights-configuration-with-applicationinsights-config.md), und löschen Sie den Verweis auf `DependencyCollector.DependencyTrackingTelemetryModule`.

## <a name="flushing-data"></a>Leeren von Daten
Normalerweise sendet das SDK Daten zu ausgewählten Zeiten, um den Benutzer möglichst wenig zu beeinträchtigen. In einigen Fällen empfiehlt es sich jedoch, den Puffer zu leeren – wenn Sie das SDK beispielsweise in einer Anwendung verwenden, die heruntergefahren wird.

*C#*

    telemetry.Flush();

    // Allow some time for flushing before shutdown.
    System.Threading.Thread.Sleep(1000);

Beachten Sie, dass diese Funktion für den [Servertelemetriekanal](https://www.nuget.org/packages/Microsoft.ApplicationInsights.WindowsServer.TelemetryChannel/) asynchron ist.

## <a name="authenticated-users"></a>Authentifizierte Benutzer
In einer Web-App werden Benutzer standardmäßig durch Cookies identifiziert. Benutzer werden möglicherweise mehr als einmal gezählt, wenn sie über verschiedene Computer oder Browser auf Ihre App zugreifen oder Cookies löschen.

Wenn sich Benutzer bei Ihrer App anmelden, erhalten Sie einen genaueren Zählwert durch Festlegen der ID des authentifizierten Benutzers im Browsercode:

*JavaScript*

```JS
    // Called when my app has identified the user.
    function Authenticated(signInId) {
      var validatedId = signInId.replace(/[,;=| ]+/g, "_");
      appInsights.setAuthenticatedUserContext(validatedId);
      ...
    }
```

In einer ASP.NET-MVC-Webanwendung, beispielsweise:

*Razor*

        @if (Request.IsAuthenticated)
        {
            <script>
                appInsights.setAuthenticatedUserContext("@User.Identity.Name
                   .Replace("\\", "\\\\")"
                   .replace(/[,;=| ]+/g, "_"));
            </script>
        }

Es ist nicht notwendig, den tatsächlichen Anmeldenamen des Benutzers zu verwenden. Es muss sich lediglich um eine ID handeln, die für diesen Benutzer eindeutig ist. Sie darf keine Leerzeichen und keines der folgenden Zeichen enthalten: `,;=|`.

Die Benutzer-ID wird zudem in einem Sitzungscookie festgelegt und an den Server gesendet. Wenn das Server-SDK installiert ist, wird die ID des authentifizierten Benutzers als Teil der Kontexteigenschaften der Client- und Servertelemetrie gesendet, sodass Sie sie filtern und durchsuchen können.

Wenn bei Ihrer App Benutzer in Konten gruppiert werden, können Sie auch einen Bezeichner für das Konto (mit den gleichen Zeicheneinschränkungen) übergeben.

      appInsights.setAuthenticatedUserContext(validatedId, accountId);

Im [Metrik-Explorer](app-insights-metrics-explorer.md) können Sie ein Diagramm erstellen, das **authentifizierte Benutzer** und **Benutzerkonten** zählt.

Sie können auch nach Clientdatenpunkten mit bestimmten Benutzernamen und Konten [suchen][diagnostic].

## <a name="a-namepropertiesafilter-search-and-segment-your-data-with-properties"></a><a name="properties"></a>Filtern, Suchen und Segmentieren der Daten mit Eigenschaften
Sie können Ihren Ereignissen (und auch Metriken, Seitenaufrufen, Ausnahmen und anderen Telemetriedaten) Eigenschaften und Messungen anfügen.

**Eigenschaften** sind die Zeichenfolgenwerte, die Sie zum Filtern der Telemetriedaten in den Nutzungsberichten verwenden können. Wenn zum Beispiel die Anwendung mehrere Spiele bereitstellt, sollten Sie den Namen des Spiels an jedes Ereignis anfügen, damit Sie sehen können, welche Spiele immer populärer werden.

Es gibt eine Beschränkung von ca. 1 KB für die Länge der Zeichenfolge. (Wenn Sie große Datenblöcke senden möchten, verwenden Sie den message-Parameter von [TrackTrace](#track-trace).)

**Metriken** sind numerische Werte, die grafisch dargestellt werden können. Beispiel: Sie möchten überprüfen, ob die von den Spielern erreichten Punktzahlen stetig zunehmen. Die Diagramme können anhand der mit dem Ereignis gesendeten Eigenschaften unterteilt werden, sodass Sie für verschiedene Spiele separate oder gestapelte Diagramme erhalten.

Metrikwerte müssen >=0 sein, um richtig angezeigt zu werden.

Es gibt einige [Beschränkungen hinsichtlich der Anzahl von Eigenschaften, Eigenschaftswerten und Metriken](#limits) , die Sie verwenden können.

*JavaScript*

    appInsights.trackEvent
      ("WinGame",
         // String properties:
         {Game: currentGame.name, Difficulty: currentGame.difficulty},
         // Numeric metrics:
         {Score: currentGame.score, Opponents: currentGame.opponentCount}
         );

    appInsights.trackPageView
        ("page name", "http://fabrikam.com/pageurl.html",
          // String properties:
         {Game: currentGame.name, Difficulty: currentGame.difficulty},
         // Numeric metrics:
         {Score: currentGame.score, Opponents: currentGame.opponentCount}
         );


*C#*

    // Set up some properties and metrics:
    var properties = new Dictionary <string, string>
       {{"game", currentGame.Name}, {"difficulty", currentGame.Difficulty}};
    var metrics = new Dictionary <string, double>
       {{"Score", currentGame.Score}, {"Opponents", currentGame.OpponentCount}};

    // Send the event:
    telemetry.TrackEvent("WinGame", properties, metrics);


*VB*

    ' Set up some properties:
    Dim properties = New Dictionary (Of String, String)
    properties.Add("game", currentGame.Name)
    properties.Add("difficulty", currentGame.Difficulty)

    Dim metrics = New Dictionary (Of String, Double)
    metrics.Add("Score", currentGame.Score)
    metrics.Add("Opponents", currentGame.OpponentCount)

    ' Send the event:
    telemetry.TrackEvent("WinGame", properties, metrics)


*Java*

    Map<String, String> properties = new HashMap<String, String>();
    properties.put("game", currentGame.getName());
    properties.put("difficulty", currentGame.getDifficulty());

    Map<String, Double> metrics = new HashMap<String, Double>();
    metrics.put("Score", currentGame.getScore());
    metrics.put("Opponents", currentGame.getOpponentCount());

    telemetry.trackEvent("WinGame", properties, metrics);


> [!NOTE]
> Achten Sie darauf, keine persönlich identifizierbaren Informationen in den Eigenschaften zu protokollieren.
>
>

**Wenn Sie Metriken verwenden**, öffnen Sie den Metrik-Explorer, und wählen Sie die Metrik aus der benutzerdefinierten Gruppe aus:

![Öffnen Sie den Metrik-Explorer, wählen Sie das Diagramm aus, und wählen Sie die Metrik aus.](./media/app-insights-api-custom-events-metrics/03-track-custom.png)

*Wenn die Metrik nicht angezeigt wird oder die benutzerdefinierte Überschrift nicht vorhanden ist, schließen Sie das Blatt „Auswahl“, und versuchen Sie es später erneut. Manchmal dauert das Aggregieren der Metriken über die Pipeline eine Stunde.*

**Wenn Sie Eigenschaften und Metriken verwendet haben**, unterteilen Sie die Metrik nach der Eigenschaft:

![Legen Sie die Gruppierung fest, und wählen Sie dann unter "Gruppieren nach" die Eigenschaft aus.](./media/app-insights-api-custom-events-metrics/04-segment-metric-event.png)

**In der Diagnosesuche**können Sie die Eigenschaften und Metriken einzelner Vorkommen eines Ereignisses anzeigen.

![Wählen Sie eine Instanz und anschließend "...".](./media/app-insights-api-custom-events-metrics/appinsights-23-customevents-4.png)

Verwenden Sie das Suchfeld, um Ereignisvorkommen mit einem bestimmten Eigenschaftswert anzuzeigen.

![Geben Sie einen Begriff in das Suchfeld ein.](./media/app-insights-api-custom-events-metrics/appinsights-23-customevents-5.png)

Weitere Informationen zu Suchausdrücken finden Sie [hier][diagnostic].

#### <a name="alternative-way-to-set-properties-and-metrics"></a>Alternative Methode zum Festlegen von Eigenschaften und Metriken
Wenn es für Sie praktischer ist, können Sie die Parameter eines Ereignisses in einem separaten Objekt sammeln:

    var event = new EventTelemetry();

    event.Name = "WinGame";
    event.Metrics["processingTime"] = stopwatch.Elapsed.TotalMilliseconds;
    event.Properties["game"] = currentGame.Name;
    event.Properties["difficulty"] = currentGame.Difficulty;
    event.Metrics["Score"] = currentGame.Score;
    event.Metrics["Opponents"] = currentGame.Opponents.Length;

    telemetry.TrackEvent(event);

> [!WARNING]
> Verwenden Sie nicht dieselbe Telemetrieelementinstanz (in diesem Beispiel `event`), um „Track*()“ mehrfach aufzurufen. Dies kann dazu führen, dass Telemetriedaten mit einer falschen Konfiguration gesendet werden.
>
>

## <a name="a-nametimeda-timing-events"></a><a name="timed"></a> Zeitmessung bei Ereignissen
Manchmal möchten Sie im Diagramm darstellen, wie lange es dauert, eine Aktion auszuführen. Beispielsweise möchten Sie wissen, wie lange Benutzer brauchen, um die Auswahl in einem Spiel zu erwägen. Dies ist ein nützliches Beispiel für die Verwendungszwecke des Messparameters.

*C#*

    var stopwatch = System.Diagnostics.Stopwatch.StartNew();

    // ... perform the timed action ...

    stopwatch.Stop();

    var metrics = new Dictionary <string, double>
       {{"processingTime", stopwatch.Elapsed.TotalMilliseconds}};

    // Set up some properties:
    var properties = new Dictionary <string, string>
       {{"signalSource", currentSignalSource.Name}};

    // Send the event:
    telemetry.TrackEvent("SignalProcessed", properties, metrics);



## <a name="a-namedefaultsadefault-properties-for-custom-telemetry"></a><a name="defaults"></a>Standardeigenschaften für benutzerdefinierte Telemetriedaten
Wenn Sie die Standardeigenschaftswerte für einige Ihrer benutzerdefinierten Ereignisse festlegen möchten, können Sie diese in einem „TelemetryClient“ einstellen. Sie werden jedem Telemetrieelement zugeordnet, das von diesem Client gesendet wird..

*C#*

    using Microsoft.ApplicationInsights.DataContracts;

    var gameTelemetry = new TelemetryClient();
    gameTelemetry.Context.Properties["Game"] = currentGame.Name;
    // Now all telemetry will automatically be sent with the context property:
    gameTelemetry.TrackEvent("WinGame");

*VB*

    Dim gameTelemetry = New TelemetryClient()
    gameTelemetry.Context.Properties("Game") = currentGame.Name
    ' Now all telemetry will automatically be sent with the context property:
    gameTelemetry.TrackEvent("WinGame")

*Java*

    import com.microsoft.applicationinsights.TelemetryClient;
    import com.microsoft.applicationinsights.TelemetryContext;
    ...


    TelemetryClient gameTelemetry = new TelemetryClient();
    TelemetryContext context = gameTelemetry.getContext();
    context.getProperties().put("Game", currentGame.Name);

    gameTelemetry.TrackEvent("WinGame");



Einzelne Telemetrieaufrufe können die Standardwerte in ihren Eigenschaftenwörterbüchern überschreiben.

**Für JavaScript-Webclients**verwenden Sie [JavaScript-Telemetrieinitialisierer](#js-initializer).

Wenn Sie **allen Telemetriedaten Eigenschaften hinzufügen** möchten (einschließlich Daten aus Standardsammlungsmodulen), [implementieren Sie `ITelemetryInitializer`](app-insights-api-filtering-sampling.md#add-properties).

## <a name="sampling-filtering-and-processing-telemetry"></a>Stichprobenerstellung, Filterung und Verarbeitung von Telemetriedaten
Sie können Code zum Verarbeiten Ihrer Telemetriedaten schreiben, bevor sie vom SDK gesendet werden. Die Verarbeitung umfasst Daten, die von den standardmäßigen Telemetriemodulen gesendet werden, z. B. die HTTP-Anforderungsauflistung und Abhängigkeitsauflistung.

* [Fügen Sie Telemetriedaten Eigenschaften hinzu](app-insights-api-filtering-sampling.md#add-properties) (beispielsweise Versionsnummern oder auf der Grundlage anderer Eigenschaften berechnete Werte), indem Sie `ITelemetryInitializer` implementieren.
* Mittels [Filterung](app-insights-api-filtering-sampling.md#filtering) können Sie Telemetriedaten ändern oder verwerfen, bevor sie vom SDK gesendet werden. Implementieren Sie hierzu `ITelemetryProcesor`. Sie steuern, was gesendet oder verworfen wird, aber Sie müssen die Auswirkung auf Ihre Metriken im Auge behalten. Je nach Vorgehensweise beim Verwerfen der Elemente kann es sein, dass Sie nicht mehr zwischen verwandten Elementen navigieren können.
* [Erstellung von Stichproben](app-insights-api-filtering-sampling.md) ist eine sofort einsetzbare Methode, um das von Ihrer App an das Portal gesendete Datenvolumen zu reduzieren. Dies hat keinerlei Auswirkungen auf die angezeigten Metriken oder die Fähigkeit, Probleme durch Navigieren zwischen verwandten Elementen wie Ausnahmen, Anforderungen und Seitenansichten zu diagnostizieren.

[Weitere Informationen](app-insights-api-filtering-sampling.md)

## <a name="disabling-telemetry"></a>Deaktivieren der Telemetrie
So können Sie die Sammlung und Übermittlung von Telemetriedaten **dynamisch beenden und starten** :

*C#*

```C#

    using  Microsoft.ApplicationInsights.Extensibility;

    TelemetryConfiguration.Active.DisableTelemetry = true;
```

Wenn Sie **bestimmte Standardsammlungsmodule deaktivieren** möchten (etwa Leistungsindikatoren, HTTP-Anforderungen oder Abhängigkeiten), löschen Sie in [ApplicationInsights.config][config] die entsprechenden Zeilen, oder kommentieren Sie sie aus. Diese Vorgehensweise bietet sich z. B. an, wenn Sie Ihre eigenen TrackRequest-Daten senden möchten.

## <a name="a-namedebugadeveloper-mode"></a><a name="debug"></a>Entwicklermodus
Während des Debuggens ist es sinnvoll, die Telemetriedaten beschleunigt über die Pipeline zu senden, damit die Ergebnisse sofort angezeigt werden. Sie erhalten außerdem zusätzliche Meldungen, mit denen Sie alle Probleme mit der Telemetrie verfolgen können. Schalten Sie ihn in der Produktion aus, da er Ihre App beeinträchtigen kann.

*C#*

    TelemetryConfiguration.Active.TelemetryChannel.DeveloperMode = true;

*VB*

    TelemetryConfiguration.Active.TelemetryChannel.DeveloperMode = True


## <a name="a-nameikeya-set-the-instrumentation-key-for-selected-custom-telemetry"></a><a name="ikey"></a> Festlegen des Instrumentationsschlüssels für ausgewählte benutzerdefinierte Telemetriedaten
*C#*

    var telemetry = new TelemetryClient();
    telemetry.InstrumentationKey = "---my key---";
    // ...


## <a name="a-namedynamic-ikeya-dynamic-instrumentation-key"></a><a name="dynamic-ikey"></a> Dynamischer Instrumentierungsschlüssel
Um das Vermischen von Telemetriedaten aus Entwicklungs-, Test- und Produktionsumgebungen zu vermeiden, können Sie [separate Application Insights-Ressourcen erstellen][erstellen] und ihre Schlüssel abhängig von der Umgebung ändern.

Statt den Instrumentationsschlüssel aus der Konfigurationsdatei abzurufen, können Sie ihn im Code festlegen. Legen Sie den Schlüssel in einer Initialisierungsmethode fest, wie z. B. "global.aspx.cs" in einem ASP.NET-Dienst:

*C#*

    protected void Application_Start()
    {
      Microsoft.ApplicationInsights.Extensibility.
        TelemetryConfiguration.Active.InstrumentationKey =
          // - for example -
          WebConfigurationManager.Settings["ikey"];
      ...

*JavaScript*

    appInsights.config.instrumentationKey = myKey;



Auf Webseiten empfiehlt es sich, ihn über den Zustand des Webservers festzulegen, anstatt ihn im Skript zu codieren. Hier zum Beispiel auf einer Webseite, die von einer ASP.NET-App generiert wird:

*JavaScript in Razor*

    <script type="text/javascript">
    // Standard Application Insights web page script:
    var appInsights = window.appInsights || function(config){ ...
    // Modify this part:
    }({instrumentationKey:  
      // Generate from server property:
      @Microsoft.ApplicationInsights.Extensibility.
         TelemetryConfiguration.Active.InstrumentationKey"
    }) // ...


## <a name="telemetrycontext"></a>TelemetryContext
TelemetryClient besitzt eine Eigenschaft "Context", die eine Reihe von Werten enthält, die zusammen mit allen Telemetriedaten gesendet werden. Sie werden normalerweise von den Standardtelemetriemodulen festgelegt, aber Sie können sie auch selbst einstellen. Beispiel:

    telemetry.Context.Operation.Name = "MyOperationName";

Wenn Sie diese Werte selbst festlegen, empfiehlt es sich, die entsprechende Zeile aus [ApplicationInsights.config][config] zu entfernen, damit kein Konflikt zwischen Ihren Werten und den Standardwerten entsteht.

* **Component** Identifiziert die App und deren Version.
* **Device** Daten zu dem Gerät, auf dem die App ausgeführt wird (in Web-Apps der Server oder das Clientgerät, von dem die Telemetrie gesendet wird).
* **InstrumentationKey** Identifiziert die Application Insights-Ressource in Azure, in der die Telemetrie angezeigt wird. In der Regel wird diese aus der Datei "ApplicationInsights.config" übernommen.
* **Location** Identifiziert den geografischen Standort des Geräts.
* **Operation** In Web-Apps die aktuelle HTTP-Anforderung. In anderen App-Typen können Sie dies zur Gruppierung von Ereignissen festlegen.
  * **Id**: Ein generierter Wert, der verschiedene Ereignisse korreliert, sodass Sie beim Untersuchen eines Ereignisses in der Diagnosesuche "Verwandte Elemente" finden können.
  * **Name**: Ein Bezeichner, in der Regel die URL der HTTP-Anforderung.
  * **SyntheticSource**: Wenn sie nicht null oder leer ist, gibt diese Zeichenfolge an, dass die Quelle der Anforderung als Roboter oder Webtest identifiziert wurde. Standardmäßig wird sie von Berechnungen im Metrik-Explorer ausgeschlossen.
* **Properties** Eigenschaften, die mit allen Telemetriedaten gesendet werden. Kann in einzelnen Track*-Aufrufen außer Kraft gesetzt werden.
* **Session** Identifiziert die Sitzung des Benutzers. Die ID wird auf einen generierten Wert festgelegt, der geändert wird, wenn der Benutzer für eine Weile nicht aktiv ist.
* **User** : Benutzerinformationen.

## <a name="limits"></a>Grenzen
[!INCLUDE [application-insights-limits](../../includes/application-insights-limits.md)]

*Wie kann ich das Erreichen der Obergrenze vermeiden?*

* Verwenden Sie [Stichproben](app-insights-sampling.md).

*Wie lange werden Daten aufbewahrt?*

* Informationen hierzu finden Sie unter [Datenaufbewahrung und Datenschutz][Daten].

## <a name="reference-docs"></a>Referenz
* [ASP.NET-Referenz](https://msdn.microsoft.com/library/dn817570.aspx)
* [Java-Referenz](http://dl.windowsazure.com/applicationinsights/javadoc/)
* [JavaScript-Referenz](https://github.com/Microsoft/ApplicationInsights-JS/blob/master/API-reference.md)
* [Android-SDK](https://github.com/Microsoft/ApplicationInsights-Android)
* [iOS SDK](https://github.com/Microsoft/ApplicationInsights-iOS)

## <a name="sdk-code"></a>SDK-Code
* [ASP.NET Core SDK](https://github.com/Microsoft/ApplicationInsights-dotnet)
* [ASP.NET 5](https://github.com/Microsoft/ApplicationInsights-aspnet5)
* [Windows Server-Pakete](https://github.com/Microsoft/applicationInsights-dotnet-server)
* [Java SDK](https://github.com/Microsoft/ApplicationInsights-Java)
* [JavaScript SDK](https://github.com/Microsoft/ApplicationInsights-JS)
* [Alle Plattformen](https://github.com/Microsoft?utf8=%E2%9C%93&query=applicationInsights)

## <a name="questions"></a>Fragen
* *Welche Ausnahmen werden möglicherweise von Aufrufen vom Typ „Track_()“ ausgelöst?*

    Keine Sie müssen sie nicht mit try/catch-Klauseln umschließen. Wenn beim SDK Probleme auftreten, werden Meldungen protokolliert, die in der Debugkonsolenausgabe und – sofern die Meldungen ankommen – in der Diagnosesuche angezeigt werden.
* *Gibt es eine REST-API zum Abrufen von Daten aus dem Portal?*

    Ja, die [Datenzugriffs-API](https://dev.applicationinsights.io/). Weitere Methoden zum Extrahieren von Daten stellen das [Exportieren aus Analytics in Power BI](app-insights-export-power-bi.md) und der [fortlaufende Export](app-insights-export-telemetry.md) dar.

## <a name="a-namenextanext-steps"></a><a name="next"></a>Nächste Schritte
[Durchsuchen von Ereignissen und Protokollen][diagnostic]

[Beispiele und exemplarische Vorgehensweisen](app-insights-code-samples.md)

[Problembehandlung][qna]

<!--Link references-->

[client]: app-insights-javascript.md
[config]: app-insights-configuration-with-applicationinsights-config.md
[erstellen]: app-insights-create-new-resource.md
[Daten]: app-insights-data-retention-privacy.md
[diagnostic]: app-insights-diagnostic-search.md
[exceptions]: app-insights-asp-net-exceptions.md
[greenbrown]: app-insights-asp-net.md
[java]: app-insights-java-get-started.md
[metrics]: app-insights-metrics-explorer.md
[qna]: app-insights-troubleshoot-faq.md
[trace]: app-insights-search-diagnostic-logs.md



<!--HONumber=Nov16_HO3-->


