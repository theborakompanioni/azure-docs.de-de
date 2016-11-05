---
title: Einführung in Analytics in Application Insights | Microsoft Docs
description: Kurze Beispiele für alle Hauptabfragen in Analytics, dem leistungsfähigen Suchtool für Application Insights.
services: application-insights
documentationcenter: ''
author: alancameronwills
manager: douge

ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 10/03/2016
ms.author: awills

---
# <a name="a-tour-of-analytics-in-application-insights"></a>Einführung in Analytics in Application Insights
[Analytics](app-insights-analytics.md) ist die leistungsfähige Suchfunktion von [Application Insights](app-insights-overview.md). Auf diesen Seiten wird die Analytics-Abfragesprache beschrieben.

* **[Sehen Sie sich das Einführungsvideo an](https://applicationanalytics-media.azureedge.net/home_page_video.mp4)**.
* **[Testen Sie Analytics mit unseren simulierten Daten](https://analytics.applicationinsights.io/demo)**, wenn Ihre App noch keine Daten an Application Insights sendet.

Sehen Sie sich zu Beginn einige grundlegende Abfragen im Detail an.

## <a name="connect-to-your-application-insights-data"></a>Verbinden mit Ihren Application Insights-Daten
Öffnen Sie Analytics über das [Blatt „Übersicht“](app-insights-dashboards.md) Ihrer App in Application Insights:

![Öffnen Sie unter „portal.azure.com“ die Application Insights-Ressource, und wählen Sie „Analytics“.](./media/app-insights-analytics-tour/001.png)

## <a name="[take](app-insights-analytics-reference.md#take-operator):-show-me-n-rows"></a>[take](app-insights-analytics-reference.md#take-operator): Anzeigen von n Zeilen
Datenpunkte, mit denen Benutzervorgänge protokolliert werden (normalerweise von der Web-App empfangene HTTP-Anforderungen), werden in einer Tabelle mit dem Namen `requests`gespeichert. Jede Zeile ist ein Telemetriedatenpunkt, der aus dem Application Insights-SDK in Ihrer App empfangen wird.

Wir beginnen, indem wir einige Beispielzeilen der Tabelle untersuchen:

![results](./media/app-insights-analytics-tour/010.png)

> [!NOTE]
> Positionieren Sie den Cursor an einer beliebigen Stelle in der Anweisung, bevor Sie auf „Start“ klicken. Sie können eine Anweisung auf mehrere Zeilen aufteilen, aber fügen Sie keine leeren Zeilen in eine Anweisung ein. Leere Zeilen sind eine bequeme Möglichkeit, um im Fenster mehrere separate Abfragen zu verwenden.
> 
> 

Wählen Sie Spalten aus, und passen Sie ihre Positionen an:

![Klicken Sie in der oberen rechten Ecke der Ergebnisse auf die Spaltenauswahl.](./media/app-insights-analytics-tour/030.png)

Erweitern Sie ein Element, um die Details anzuzeigen:

![Wählen Sie „Tabelle“ aus, und verwenden Sie „Spalten konfigurieren“.](./media/app-insights-analytics-tour/040.png)

> [!NOTE]
> Klicken Sie auf den Anfang einer Spalte, um die im Webbrowser zur Verfügung stehenden Ergebnisse neu zu ordnen. Beachten Sie aber, dass die Anzahl von in den Browser heruntergeladen Zeilen bei einem großen Resultset beschränkt ist. Daher sollten Sie im Hinterkopf behalten, dass diese Sortierung Ihnen nicht unbedingt die tatsächlichen höchsten oder niedrigsten Elemente anzeigt. Hierfür sollten Sie den Operator `top` oder `sort` verwenden. 
> 
> 

## <a name="[top](app-insights-analytics-reference.md#top-operator)-and-[sort](app-insights-analytics-reference.md#sort-operator)"></a>[Top](app-insights-analytics-reference.md#top-operator) und [Sort](app-insights-analytics-reference.md#sort-operator)
`take` ist hilfreich, um schnell eine Stichprobe abzurufen. Die Zeilen aus der Tabelle werden aber nicht in einer bestimmten Reihenfolge angezeigt. Verwenden Sie für eine sortierte Ansicht `top` (für eine Stichprobe) oder `sort` (für die gesamte Tabelle).

Zeigen Sie die ersten n Zeilen, sortiert nach einer bestimmten Spalte an:

```AIQL

    requests | top 10 by timestamp desc 
```

* *Syntax*: Die meisten Operatoren besitzen Schlüsselwortparameter, z.B. `by`.
* `desc` = absteigende Reihenfolge, `asc` = aufsteigende Reihenfolge.

![](./media/app-insights-analytics-tour/260.png)

`top...` ist eine leistungsstärkere Methode, um `sort ... | take...` auszudrücken. Wir hätten auch Folgendes schreiben können:

```AIQL

    requests | sort by timestamp desc | take 10
```

Das Ergebnis wäre identisch, würde jedoch etwas langsamer ausgeführt. (Sie können auch `order` schreiben, wobei es sich um einen Alias von `sort` handelt.)

Die Spaltenüberschriften in der Tabellenansicht können auch zum Sortieren der Ergebnisse auf dem Bildschirm verwendet werden. Wenn Sie aber `take` oder `top` verwendet haben, um nur einen Teil einer Tabelle abzurufen, ordnen Sie nur die Reihenfolge der abgerufenen Datensätze neu.

## <a name="[project](app-insights-analytics-reference.md#project-operator):-select,-rename-and-compute-columns"></a>[project](app-insights-analytics-reference.md#project-operator): Auswählen, Umbenennen und Berechnen von Spalten
Verwenden Sie [`project`](app-insights-analytics-reference.md#project-operator) , um nur die gewünschten Spalten auszuwählen:

```AIQL

    requests | top 10 by timestamp desc
             | project timestamp, name, resultCode
```

![](./media/app-insights-analytics-tour/240.png)

Sie können auch Spalten umbenennen und neue definieren:

```AIQL

    requests 
      | top 10 by timestamp desc 
      | project  
            name, 
            response = resultCode,
            timestamp, 
            ['time of day'] = floor(timestamp % 1d, 1s)
```

![result](./media/app-insights-analytics-tour/270.png)

* [Spaltennamen](app-insights-analytics-reference.md#names) können Leerzeichen oder Symbole enthalten, wenn diese wie folgt in Klammern stehen: `['...']` oder `["..."]`.
* `%` ist der übliche Modulo-Operator. 
* `1d` (die Ziffer Eins, gefolgt von einem „d“) ist ein Zeitraumliteral für einen Tag. Dies sind einige weitere Zeitraumliterale: `12h`, `30m`, `10s`, `0.01s`.
* `floor` (Alias `bin`) rundet einen Wert auf das nächste Vielfache des von Ihnen angegebenen Basiswerts ab. `floor(aTime, 1s)` rundet demnach eine Zeit auf die nächstniedrigere Sekunde ab.

[Ausdrücke](app-insights-analytics-reference.md#scalars) können alle üblichen Operatoren (`+`,`-` usw.) enthalten. Zudem gibt es zahlreiche nützliche Funktionen.

## <a name="[extend](app-insights-analytics-reference.md#extend-operator):-compute-columns"></a>[extend:](app-insights-analytics-reference.md#extend-operator)Berechnen von Spalten
Wenn Sie nur neue Spalten den vorhandenen hinzufügen möchten, verwenden Sie [`extend`](app-insights-analytics-reference.md#extend-operator):

```AIQL

    requests 
      | top 10 by timestamp desc
      | extend timeOfDay = floor(timestamp % 1d, 1s)
```

[`extend`](app-insights-analytics-reference.md#extend-operator) ist weniger ausführlich als [`project`](app-insights-analytics-reference.md#project-operator), wenn Sie alle vorhandenen Spalten beibehalten möchten.

## <a name="[summarize](app-insights-analytics-reference.md#summarize-operator):-aggregate-groups-of-rows"></a>[summarize](app-insights-analytics-reference.md#summarize-operator): Aggregieren von Zeilengruppen
`Summarize` wendet eine angegebene *Aggregationsfunktion* auf Zeilengruppen an. 

Beispielsweise wird der Zeitraum, den Ihre Web-App zum Antworten auf eine Anforderung benötigt, im Feld `duration`gemeldet. Die durchschnittliche Reaktionszeit für alle Anforderungen lautet:

![](./media/app-insights-analytics-tour/410.png)

Wir können das Ergebnis auch in Anforderungen mit unterschiedlichen Namen unterteilen:

![](./media/app-insights-analytics-tour/420.png)

`Summarize` erfasst die Datenpunkte im Datenstrom und fasst sie zu Gruppen zusammen, für die die `by`-Klausel gleich ausgewertet wird. Jeder Wert im Ausdruck `by` – jeder Vorgangsname im obigen Beispiel – ergibt eine Zeile in der Ergebnistabelle. 

Außerdem können Ergebnisse nach der Tageszeit gruppiert werden:

![](./media/app-insights-analytics-tour/430.png)

Beachten Sie, wie wir die Funktion `bin` (auch `floor`) verwenden. Wenn wir nur `by timestamp` verwenden, wird jede Eingabezeile in einer eigenen kleinen Gruppe angeordnet. Für alle kontinuierlichen Skalare, z.B. Zeiten oder Zahlen, müssen wir den fortlaufenden Bereich in eine verwaltbare Anzahl von diskreten Werten unterteilen. Die Verwendung von `bin`, wobei es sich eigentlich nur um die vertraute `floor`-Funktion zum Abrunden handelt, ist hierfür die einfachste Möglichkeit.

Wir können dasselbe Verfahren anwenden, um Bereiche für Zeichenfolgen zu reduzieren:

![](./media/app-insights-analytics-tour/440.png)

Beachten Sie, dass Sie `name=` zum Festlegen des Namens einer Ergebnisspalte verwenden können, und zwar entweder in den Aggregationsausdrücken oder der by-Klausel.

## <a name="counting-sampled-data"></a>Zählen von Stichprobendaten
`sum(itemCount)` ist die empfohlene Aggregation zum Zählen von Ereignissen. In vielen Fällen gilt „itemCount==1“. Mit der Funktion wird also einfach die Anzahl von Zeilen in der Gruppe gezählt. Wenn jedoch [Stichproben](app-insights-sampling.md) erstellt werden, wird nur ein Bruchteil der ursprünglichen Ereignisse als Datenpunkte in Application Insights beibehalten, sodass für jeden Datenpunkt, den Sie sehen, `itemCount`-Ereignisse vorhanden sind. 

Wenn z.B. bei der Stichprobenerstellung 75 % der ursprünglichen Ereignisse verworfen werden, gilt „itemCount== 4“ in den beibehaltenen Datensätzen, d.h., für jeden beibehaltenen Datensatz gab es ursprünglich vier Datensätze. 

Durch die adaptive Stichprobenerstellung ist itemCount höher, wenn Ihre Anwendung stark ausgelastet ist.

Mit dem Summieren von itemCount erhalten Sie daher eine gute Schätzung der ursprünglichen Anzahl von Ereignissen.

![](./media/app-insights-analytics-tour/510.png)

Zudem ist die Aggregation `count()` (und ein Zählvorgang) für die Fälle vorhanden, in denen Sie wirklich die Anzahl von Zeilen in einer Gruppe ermitteln möchten.

Es gibt eine ganze Reihe von [Aggregationsfunktionen](app-insights-analytics-reference.md#aggregations).

## <a name="charting-the-results"></a>Darstellen der Ergebnisse in Diagrammen
```AIQL

    exceptions 
       | summarize count()  
         by bin(timestamp, 1d)
```

Standardmäßig werden Ergebnisse als Tabelle angezeigt:

![](./media/app-insights-analytics-tour/225.png)

Es gibt jedoch noch eine bessere Ansicht als die Tabelle. Betrachten Sie die Ergebnisse in der Diagrammansicht mit vertikalem Balken:

![Klicken Sie auf „Diagramm“, wählen Sie dann „Vertikales Balkendiagramm“, und weisen Sie x- und y-Achsen zu.](./media/app-insights-analytics-tour/230.png)

Beachten Sie, dass die Diagrammdarstellung die Uhrzeitangaben immer in der richtigen Reihenfolge anzeigt, auch wenn wir die Ergebnisse nicht nach Zeit sortiert haben (wie es in der Tabellenansicht der Fall ist).

## <a name="[where](app-insights-analytics-reference.md#where-operator):-filtering-on-a-condition"></a>[where:](app-insights-analytics-reference.md#where-operator)Filtern nach einer Bedingung
Wenn Sie die Application Insights-Überwachung für die [Client](app-insights-javascript.md) - und Serverseite der App eingerichtet haben, stammen einige der Telemetriedaten in der Datenbank aus Browsern.

Sehen Sie sich nur die vom Browser gemeldeten Ausnahmen an:

```AIQL

    exceptions 
      | where device_Id == "browser" 
      |  summarize count() 
       by device_BrowserVersion, outerExceptionMessage 
```

![](./media/app-insights-analytics-tour/250.png)

Der `where` -Operator akzeptiert einen booleschen Ausdruck. Dazu einige wichtige Punkte:

* `and`, `or`: Boolesche Operatoren
* `==`, `<>`: Gleich und ungleich
* `=~`, `!=`: Zeichenfolge ohne Beachtung der Groß-/Kleinschreibung, gleich und ungleich Es gibt viele weitere Zeichenfolgenvergleichsoperatoren.

Erfahren Sie mehr über [skalare Ausdrücke](app-insights-analytics-reference.md#scalars).

### <a name="filtering-events"></a>Filtern von Ereignissen
Suchen Sie nach nicht erfolgreichen Anforderungen:

```AIQL

    requests 
      | where isnotempty(resultCode) and toint(resultCode) >= 400
```

`responseCode` ist vom Typ „string“, sodass wir das Element für einen numerischen Vergleich [umwandeln](app-insights-analytics-reference.md#casts) müssen.

Fassen Sie die unterschiedlichen Antworten zusammen:

```AIQL

    requests
      | where isnotempty(resultCode) and toint(resultCode) >= 400
      | summarize count() 
      by resultCode
```

## <a name="timecharts"></a>Zeitdiagramme
Zeigen Sie an, wie viele Ereignissen pro Tag stattfinden:

```AIQL

    requests
      | summarize event_count=count()
        by bin(timestamp, 1d)
```

Wählen Sie die Diagrammanzeigeoption aus:

![Zeitdiagramm](./media/app-insights-analytics-tour/080.png)

Die x-Achse für Liniendiagramme muss vom Typ „DateTime“ sein. 

## <a name="multiple-series"></a>Mehrere Reihen
Verwenden Sie mehrere Werte in einer `summarize by` -Klausel, um eine separate Zeile für jede Kombination von Werten zu erstellen:

```AIQL

    requests 
      | summarize event_count=count()   
        by bin(timestamp, 1d), client_StateOrProvince
```

![](./media/app-insights-analytics-tour/090.png)

Um mehrere Linien in einem Diagramm anzuzeigen, klicken Sie auf **Teilen nach** und wählen eine Spalte aus.

![](./media/app-insights-analytics-tour/100.png)

## <a name="daily-average-cycle"></a>Durchschnittlicher Tageszyklus
Inwiefern variiert die Nutzung im Verlauf eines durchschnittlichen Tages?

Zählen Sie die Anforderungen nach Zeitmodule an einem Tag, unterteilt in Stunden:

```AIQL

    requests
      | extend hour = floor(timestamp % 1d , 1h) 
          + datetime("2016-01-01")
      | summarize event_count=count() by hour
```

![Liniendiagramm der Stunden an einem durchschnittlichen Tag](./media/app-insights-analytics-tour/120.png)

> [!NOTE]
> Beachten Sie, dass Zeitdauern derzeit in datetime-Angaben konvertiert werden müssen, um sie im Diagramm anzuzeigen.
> 
> 

## <a name="compare-multiple-daily-series"></a>Vergleichen mehrerer täglicher Reihen
Inwieweit variiert die Nutzung im Verlauf des Tages in unterschiedlichen Staaten?

```AIQL
    requests
     | extend hour= floor( timestamp % 1d , 1h)
           + datetime("2001-01-01")
     | summarize event_count=count() 
       by hour, client_StateOrProvince
```

Teilen Sie das Diagramm in Staaten auf:

![Aufteilen nach „client_StateOrProvince“](./media/app-insights-analytics-tour/130.png)

## <a name="plot-a-distribution"></a>Darstellen einer Verteilung
Wie viele Sitzungen unterschiedlicher Längen gibt es?

```AIQL

    requests 
      | where isnotnull(session_Id) and isnotempty(session_Id) 
      | summarize min(timestamp), max(timestamp) 
      by session_Id 
      | extend sessionDuration = max_timestamp - min_timestamp 
      | where sessionDuration > 1s and sessionDuration < 3m 
      | summarize count() by floor(sessionDuration, 3s) 
      | project d = sessionDuration + datetime("2016-01-01"), count_
```

Die letzte Linie ist erforderlich, um die Konvertierung in datetime-Angaben durchzuführen: Die x-Achse eines Liniendiagramms kann derzeit nur datetime-Angaben enthalten.

Die `where` -Klausel schließt einmalige Sitzungen (sessionDuration==0) aus und legt die Länge der x-Achse fest.

![](./media/app-insights-analytics-tour/290.png)

## <a name="[percentiles](app-insights-analytics-reference.md#percentiles)"></a>[Quantile](app-insights-analytics-reference.md#percentiles)
Welche Bereichsdauern decken verschiedene Prozentsätze von Sitzungen ab?

Verwenden Sie die obige Abfrage, aber ersetzen Sie die letzte Zeile:

```AIQL

    requests 
      | where isnotnull(session_Id) and isnotempty(session_Id) 
      | summarize min(timestamp), max(timestamp) 
      by session_Id 
      | extend sesh = max_timestamp - min_timestamp 
      | where sesh > 1s
      | summarize count() by floor(sesh, 3s) 
      | summarize percentiles(sesh, 5, 20, 50, 80, 95)
```

Wir haben auch die Obergrenze in der Where-Klausel entfernt, um die richtigen Zahlen einschließlich aller Sitzungen mit mehr als einer Anforderung zu ermitteln:

![result](./media/app-insights-analytics-tour/180.png)

Daraus lässt sich Folgendes ablesen:

* 5 % der Sitzungen sind kürzer als 3 Minuten 34 Sekunden. 
* 50 % der Sitzung sind kürzer als 36 Minuten.
* 5 % der Sitzungen dauern länger als 7 Tage.

Um eine separate Aufstellung für jedes Land zu erhalten, müssen Sie die Spalte „client_CountryOrRegion“ separat durch beide summarize-Operatoren bringen:

```AIQL

    requests 
      | where isnotnull(session_Id) and isnotempty(session_Id) 
      | summarize min(timestamp), max(timestamp) 
      by session_Id, client_CountryOrRegion
      | extend sesh = max_timestamp - min_timestamp 
      | where sesh > 1s
      | summarize count() by floor(sesh, 3s), client_CountryOrRegion
      | summarize percentiles(sesh, 5, 20, 50, 80, 95)
      by client_CountryOrRegion
```

![](./media/app-insights-analytics-tour/190.png)

## <a name="[join](app-insights-analytics-reference.md#join)"></a>[Join](app-insights-analytics-reference.md#join)
Wir haben Zugriff auf mehrere Tabellen, einschließlich der Anforderungen und Ausnahmen.

Um Ausnahmen im Zusammenhang mit einer Anforderung zu suchen, die eine Fehlerantwort zurückgegeben hat, können wir die Tabellen anhand von `session_Id`verknüpfen:

```AIQL

    requests 
      | where toint(responseCode) >= 500 
      | join (exceptions) on operation_Id 
      | take 30
```


Es ist üblich, `project` zu verwenden, um vor dem Verknüpfen nur die Spalten auszuwählen, die wir benötigen.
In den gleichen Klauseln benennen wir die Zeitstempelspalte um.

## <a name="[let](app-insights-analytics-reference.md#let-clause):-assign-a-result-to-a-variable"></a>[let](app-insights-analytics-reference.md#let-clause): Zuweisen eines Ergebnisses zu einer Variablen
Verwenden Sie [let](app-insights-analytics-reference.md#let-statements) , um die einzelnen Teile des vorherigen Ausdrucks zu separieren. Die Ergebnisse sind wie folgt unverändert:

```AIQL

    let bad_requests = 
      requests
        | where  toint(resultCode) >= 500  ;
    bad_requests
      | join (exceptions) on session_Id 
      | take 30
```

> Tipp: Fügen Sie im Analytics-Client keine Leerzeilen zwischen diesen Teilen ein. Stellen Sie sicher, dass Sie alles ausführen.
> 
> 

## <a name="accessing-nested-objects"></a>Zugreifen auf geschachtelte Objekte
Auf geschachtelte Objekte kann leicht zugegriffen werden. Beispielsweise finden Sie im Ausnahmedatenstrom strukturierte Objekte wie diese:

![result](./media/app-insights-analytics-tour/520.png)

Sie können sie vereinfachen, indem Sie nur die für Sie interessanten Eigenschaften auswählen.

```AIQL

    exceptions | take 10
      | extend method1 = tostring(details[0].parsedStack[1].method)
```

Beachten Sie, dass eine [Umwandlung](app-insights-analytics-reference.md#casts) in den entsprechenden Typ vorgenommen werden muss.

## <a name="custom-properties-and-measurements"></a>Benutzerdefinierte Eigenschaften und Messungen
Wenn Ihre Anwendung [benutzerdefinierte Dimensionen (Eigenschaften) und benutzerdefinierte Messungen](app-insights-api-custom-events-metrics.md#properties) an Ereignisse anfügt, sehen Sie diese in den Objekten `customDimensions` und `customMeasurements`.

Zum Beispiel kann Ihre App Folgendes einfügen:

```C#

    var dimensions = new Dictionary<string, string> 
                     {{"p1", "v1"},{"p2", "v2"}};
    var measurements = new Dictionary<string, double>
                     {{"m1", 42.0}, {"m2", 43.2}};
    telemetryClient.TrackEvent("myEvent", dimensions, measurements);
```

Und so extrahieren Sie diese Werte in Analytics:

```AIQL

    customEvents
      | extend p1 = customDimensions.p1, 
      m1 = todouble(customMeasurements.m1) // cast to expected type

``` 

## <a name="tables"></a>Tabellen
Sie können über mehrere Tabellen auf den Telemetriedatenstrom aus Ihrer App zugreifen. Das Schema der verfügbaren Eigenschaften für jede Tabelle wird auf der linken Seite des Fensters angezeigt.

### <a name="requests-table"></a>Tabelle „Anforderungen“
Anzahl der HTTP-Anforderungen an Ihre Web-App und Segmente nach Seitenname:

![Anzahl der Anforderungen, nach Namen unterteilt](./media/app-insights-analytics-tour/analytics-count-requests.png)

Suchen der Anforderungen mit den häufigsten Fehlern:

![Anzahl der Anforderungen, nach Namen unterteilt](./media/app-insights-analytics-tour/analytics-failed-requests.png)

### <a name="custom-events-table"></a>Tabelle „Benutzerdefinierte Ereignisse“
Wenn Sie Ihre eigenen Ereignisse über [TrackEvent()](app-insights-api-custom-events-metrics.md#track-event) senden, können Sie sie dieser Tabelle entnehmen. 

Sehen wir uns ein Beispiel, in dem Ihr App-Code diese Zeilen enthält:

```C#

    telemetry.TrackEvent("Query", 
       new Dictionary<string,string> {{"query", sqlCmd}},
       new Dictionary<string,double> {
           {"retry", retryCount},
           {"querytime", totalTime}})
```

Anzeige der Häufigkeit dieser Ereignisse:

![Anzeige der Rate benutzerdefinierter Ereignisse](./media/app-insights-analytics-tour/analytics-custom-events-rate.png)

Extrahieren von Maßen und Dimensionen aus den Ereignissen:

![Anzeige der Rate benutzerdefinierter Ereignisse](./media/app-insights-analytics-tour/analytics-custom-events-dimensions.png)

### <a name="custom-metrics-table"></a>Tabelle „Benutzerdefinierte Metriken“
Wenn Sie eigene Metrikwerte über [TrackMetric()](app-insights-api-custom-events-metrics.md#track-metric) senden, finden Sie die Ergebnisse im Strom **CustomMetrics**. Beispiel:  

![Benutzerdefinierte Metriken in der Application Insights-Analyse](./media/app-insights-analytics-tour/analytics-custom-metrics.png)

> [!NOTE]
> Im [Metrik-Explorer](app-insights-metrics-explorer.md) werden auf dem Blatt „Metriken“ alle benutzerdefinierten und an einen Telemetrietyp angefügten Messungen zusammen mit den Metriken angezeigt, die mithilfe von `TrackMetric()` gesendet wurden. In Analytics hingegen bleiben die benutzerdefinierten Messungen weiterhin an den jeweiligen Telemetrietyp (Ereignisse, Anforderungen etc.) gebunden, und die über TrackMetric gesendeten Metriken werden in ihrem eigenen Datenstrom angezeigt.
> 
> 

### <a name="performance-counters-table"></a>Tabelle „Leistungsindikatoren“
[Leistungsindikatoren](app-insights-web-monitor-performance.md#system-performance-counters) zeigen Ihnen, grundlegende Systemmetriken für Ihre App, wie z. B. CPU, Arbeitsspeicher und Netzwerkauslastung. Sie können das SDK zum Senden von zusätzlicher Indikatoren, z. B. benutzerdefinierten Leistungsindikatoren konfigurieren.

Das Schema **performanceCounters** zeigt die `category`, den `counter`-Namen und `instance`-Namen der einzelnen Leistungsindikatoren. Namen von Leistungsindikatorinstanzen gelten nur für einige Leistungsindikatoren und enthalten in der Regel den Namen des Prozesses, auf den sich die Anzahl bezieht. In den Telemetriedaten jeder Anwendung sehen Sie nur die Indikatoren für diese Anwendung. Beispielsweise, um verfügbare Leistungsindikatoren anzuzeigen: 

![Leistungsindikatoren in der Application Insights-Analyse](./media/app-insights-analytics-tour/analytics-performance-counters.png)

So erhalten Sie ein Diagramm des verfügbaren Arbeitsspeichers im aktuellen Zeitraum: 

![Zeitdiagramm der Speicherauslastung in der Application Insights-Analyse](./media/app-insights-analytics-tour/analytics-available-memory.png)

Wie andere Telemetriedaten umfasst auch **performanceCounters** eine Spalte `cloud_RoleInstance`, die die Identität des Hostcomputers angibt, auf dem Ihre Anwendung ausgeführt wird. Geben Sie beispielsweise Folgendes ein, um die Leistung Ihrer App auf verschiedenen Computern vergleichen: 

![Nach Rolleninstanz in der Application Insights-Analyse segmentierte Leistung](./media/app-insights-analytics-tour/analytics-metrics-role-instance.png)

### <a name="exceptions-table"></a>Tabelle „Ausnahmen“
[Von Ihrer Anwendung gemeldete Ausnahmen](app-insights-asp-net-exceptions.md) sind in dieser Tabelle verfügbar. 

Um die beim Auslösen der Ausnahme von Ihrer Anwendung verarbeitete HTTP-Anforderung zu finden, verknüpfen Sie operation_Id:

![Verknüpfen von Ausnahmen mit Anforderungen über operation_Id](./media/app-insights-analytics-tour/analytics-exception-request.png)

### <a name="browser-timings-table"></a>Tabelle „Browser-Zeitangaben“
`browserTimings` zeigt die in den Browsern Ihrer Benutzer erfassten Seitenladezeiten

[Richten Sie Ihre App für die clientseitige Telemetrie ein](app-insights-javascript.md), um diese Metriken anzuzeigen. 

Das Schema enthält [Metriken zur Dauer der verschiedenen Phasen des Seitenladevorgangs](app-insights-javascript.md#page-load-performance). (Sie geben nicht an, wie lange Ihre Benutzer eine Seite lesen.)  

Anzeige der Beliebtheit verschiedener Seiten und Ladezeiten für jede Seite:

![Seitenladezeiten in Analytics](./media/app-insights-analytics-tour/analytics-page-load.png)

### <a name="availbility-results-table"></a>Tabelle „Verfügbarkeitsergebnisse“
`availabilityResults` zeigt die Ergebnisse Ihrer [Webtests](app-insights-monitor-web-app-availability.md) an. Jede Ausführung Ihrer Tests an jedem Teststandort wird separat gemeldet. 

![Seitenladezeiten in Analytics](./media/app-insights-analytics-tour/analytics-availability.png)

### <a name="dependencies-table"></a>Tabelle „Abhängigkeiten“
Enthält die Ergebnisse der Datenbank- und REST-API-Aufrufe Ihrer Anwendung sowie anderer TrackDependency()-Aufrufe.

### <a name="traces-table"></a>Tabelle „Ablaufverfolgungen“
Enthält die von Ihrer App über TrackTrace() oder [andere Frameworks](app-insights-asp-net-trace-logs.md) gesendeten Telemetriedaten.

## <a name="try-it!"></a>Testen
* **[Testen Sie Analytics mit unseren simulierten Daten](https://analytics.applicationinsights.io/demo)**, wenn Ihre App noch keine Daten an Application Insights sendet.

[!INCLUDE [app-insights-analytics-footer](../../includes/app-insights-analytics-footer.md)]

<!--HONumber=Oct16_HO2-->


