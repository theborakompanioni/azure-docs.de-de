---
title: "Einführung in Analytics in Azure Application Insights | Microsoft Docs"
description: "Kurze Beispiele für alle Hauptabfragen in Analytics, dem leistungsfähigen Suchtool für Application Insights."
services: application-insights
documentationcenter: 
author: alancameronwills
manager: carmonm
ms.assetid: bddf4a6d-ea8d-4607-8531-1fe197cc57ad
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 02/07/2017
ms.author: awills
translationtype: Human Translation
ms.sourcegitcommit: 13c524cdc5ef0d9e70820cc3dac8d747e5bb5845
ms.openlocfilehash: 12e832b8e0d0509f5b59d588b43f062fb07ddcde
ms.lasthandoff: 02/11/2017


---
# <a name="a-tour-of-analytics-in-application-insights"></a>Einführung in Analytics in Application Insights
[Analytics](app-insights-analytics.md) ist die leistungsfähige Suchfunktion von [Application Insights](app-insights-overview.md). Auf diesen Seiten wird die Analytics-Abfragesprache beschrieben.

* **[Sehen Sie sich das Einführungsvideo an.](https://applicationanalytics-media.azureedge.net/home_page_video.mp4)**
* **[Testen Sie Analytics mit unseren simulierten Daten](https://analytics.applicationinsights.io/demo)**, wenn Ihre App noch keine Daten an Application Insights sendet.
* In der **[Kurzübersicht für SQL-Benutzer](https://aka.ms/sql-analytics)** finden Sie eine Übersetzung der gängigsten Sprachen.

Sehen Sie sich zu Beginn einige grundlegende Abfragen im Detail an.

## <a name="connect-to-your-application-insights-data"></a>Verbinden mit Ihren Application Insights-Daten
Öffnen Sie Analytics über das [Blatt „Übersicht“](app-insights-dashboards.md) Ihrer App in Application Insights:

![Öffnen Sie unter „portal.azure.com“ die Application Insights-Ressource, und wählen Sie „Analytics“.](./media/app-insights-analytics-tour/001.png)

## <a name="takeapp-insights-analytics-referencemdtake-operator-show-me-n-rows"></a>[take](app-insights-analytics-reference.md#take-operator): Anzeigen von n Zeilen
Datenpunkte, mit denen Benutzervorgänge protokolliert werden (normalerweise von der Web-App empfangene HTTP-Anforderungen), werden in einer Tabelle mit dem Namen `requests`gespeichert. Jede Zeile ist ein Telemetriedatenpunkt, der aus dem Application Insights-SDK in Ihrer App empfangen wird.

Wir beginnen, indem wir einige Beispielzeilen der Tabelle untersuchen:

![results](./media/app-insights-analytics-tour/010.png)

> [!NOTE]
> Positionieren Sie den Cursor an einer beliebigen Stelle in der Anweisung, bevor Sie auf „Start“ klicken. Sie können eine Anweisung auf mehrere Zeilen aufteilen, aber fügen Sie keine leeren Zeilen in eine Anweisung ein. Leere Zeilen sind eine bequeme Möglichkeit, um im Fenster mehrere separate Abfragen zu verwenden.
>
>

Wählen Sie Spalten aus, verschieben Sie sie durch Ziehen und Ablegen, gruppieren Sie nach Spalten, und wenden Sie Filter an:

![Klicken Sie in der oberen rechten Ecke der Ergebnisse auf die Spaltenauswahl.](./media/app-insights-analytics-tour/030.png)

Erweitern Sie ein Element, um die Details anzuzeigen:

![Wählen Sie „Tabelle“ aus, und verwenden Sie „Spalten konfigurieren“.](./media/app-insights-analytics-tour/040.png)

> [!NOTE]
> Klicken Sie auf den Anfang einer Spalte, um die im Webbrowser zur Verfügung stehenden Ergebnisse neu zu ordnen. Beachten Sie aber, dass die Anzahl von in den Browser heruntergeladen Zeilen bei einem großen Resultset beschränkt ist. Diese Sortierung zeigt Ihnen nicht unbedingt die tatsächlichen höchsten oder niedrigsten Elemente an. Verwenden Sie den Operator `top` oder `sort`, um die Elemente zuverlässig zu sortieren.
>
>

## <a name="topapp-insights-analytics-referencemdtop-operator-and-sortapp-insights-analytics-referencemdsort-operator"></a>[Top](app-insights-analytics-reference.md#top-operator) und [Sort](app-insights-analytics-reference.md#sort-operator)
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

## <a name="whereapp-insights-analytics-referencemdwhere-operator-filtering-on-a-condition"></a>[where:](app-insights-analytics-reference.md#where-operator)Filtern nach einer Bedingung

Lassen Sie uns nur Anforderungen prüfen, die einen bestimmten Ergebniscode zurückgegeben haben:

```AIQL

    requests
    | where resultCode  == "404"
    | take 10
```

![](./media/app-insights-analytics-tour/250.png)

Der `where` -Operator akzeptiert einen booleschen Ausdruck. Dazu einige wichtige Punkte:

* `and`, `or`: Boolesche Operatoren
* `==`, `<>`: Gleich und ungleich
* `=~`, `!=`: Zeichenfolge ohne Beachtung der Groß-/Kleinschreibung, gleich und ungleich Es gibt viele weitere Zeichenfolgenvergleichsoperatoren.

Erfahren Sie mehr über [skalare Ausdrücke](app-insights-analytics-reference.md#scalars).

### <a name="getting-the-right-type"></a>Abrufen des richtigen Typs
Suchen Sie nach nicht erfolgreichen Anforderungen:

```AIQL

    requests
    | where isnotempty(resultCode) and toint(resultCode) >= 400
```

`resultCode` ist vom Typ „string“, sodass wir das Element für einen numerischen Vergleich [umwandeln](app-insights-analytics-reference.md#casts) müssen.

## <a name="time-range"></a>Zeitbereich

Standardmäßig sind Ihre Abfragen auf die letzten 24 Stunden beschränkt. Doch Sie können diesen Wert ändern:

![](./media/app-insights-analytics-tour/change-time-range.png)

Überschreiben Sie den Zeitraum durch Schreiben einer Abfrage, in der `timestamp` in einer WHERE-Klausel erwähnt wird. Beispiel:

```AIQL

    // What were the slowest requests over the past 3 days?
    requests
    | where timestamp > ago(3d)  // Override the time range
    | top 5 by duration
```

Das Feature „Zeitbereich“ ist gleichbedeutend mit einer WHERE-Klausel, die nach jeder Erwähnung einer der Quelltabellen eingefügt wird.

`ago(3d)` bedeutet „vor drei Tagen“. Andere Zeiteinheiten sind Stunden (`2h`, `2.5h`), Minuten (`25m`) und Sekunden (`10s`).

Weitere Beispiele:

```AIQL

    // Last calendar week:
    requests
    | where timestamp > startofweek(now()-7d)
        and timestamp < startofweek(now())
    | top 5 by duration

    // First hour of every day in past seven days:
    requests
    | where timestamp > ago(7d) and timestamp % 1d < 1h
    | top 5 by duration

    // Specific dates:
    requests
    | where timestamp > datetime(2016-11-19) and timestamp < datetime(2016-11-21)
    | top 5 by duration

```

[Verweis auf Datum und Uhrzeit](app-insights-analytics-reference.md#date-and-time).


## <a name="projectapp-insights-analytics-referencemdproject-operator-select-rename-and-compute-columns"></a>[project](app-insights-analytics-reference.md#project-operator): Auswählen, Umbenennen und Berechnen von Spalten
Verwenden Sie [`project`](app-insights-analytics-reference.md#project-operator), um nur die gewünschten Spalten auszuwählen:

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

## <a name="extendapp-insights-analytics-referencemdextend-operator-compute-columns"></a>[extend:](app-insights-analytics-reference.md#extend-operator)Berechnen von Spalten
Wenn Sie nur neue Spalten den vorhandenen hinzufügen möchten, verwenden Sie [`extend`](app-insights-analytics-reference.md#extend-operator):

```AIQL

    requests
    | top 10 by timestamp desc
    | extend timeOfDay = floor(timestamp % 1d, 1s)
```

[`extend`](app-insights-analytics-reference.md#extend-operator) ist weniger ausführlich als [`project`](app-insights-analytics-reference.md#project-operator), wenn Sie alle vorhandenen Spalten beibehalten möchten.

### <a name="convert-to-local-time"></a>Konvertieren in die Ortszeit

Zeitstempel werden stets in UTC angegeben. Für die Pazifikküste der USA gilt im Winter beispielsweise Folgendes:

```AIQL

    requests
    | top 10 by timestamp desc
    | extend localTime = timestamp - 8h
```


## <a name="summarizeapp-insights-analytics-referencemdsummarize-operator-aggregate-groups-of-rows"></a>[summarize](app-insights-analytics-reference.md#summarize-operator): Aggregieren von Zeilengruppen
`Summarize` wendet eine angegebene *Aggregationsfunktion* auf Zeilengruppen an.

Beispielsweise wird der Zeitraum, den Ihre Web-App zum Antworten auf eine Anforderung benötigt, im Feld `duration`gemeldet. Die durchschnittliche Reaktionszeit für alle Anforderungen lautet:

![](./media/app-insights-analytics-tour/410.png)

Wir können das Ergebnis auch in Anforderungen mit unterschiedlichen Namen unterteilen:

![](./media/app-insights-analytics-tour/420.png)

`Summarize` erfasst die Datenpunkte im Datenstrom und fasst sie zu Gruppen zusammen, für die die `by`-Klausel gleich ausgewertet wird. Jeder Wert im Ausdruck `by` – jeder Vorgangsname im obigen Beispiel – ergibt eine Zeile in der Ergebnistabelle.

Außerdem können Ergebnisse nach der Tageszeit gruppiert werden:

![](./media/app-insights-analytics-tour/430.png)

Beachten Sie, wie wir die Funktion `bin` (auch `floor`) verwenden. Wenn wir nur `by timestamp` verwenden, wird jede Eingabezeile in einer eigenen kleinen Gruppe angeordnet. Für alle kontinuierlichen Skalare (beispielsweise Zeitangaben oder Zahlen) müssen wir den fortlaufenden Bereich in eine verwaltbare Anzahl von diskreten Werten unterteilen. `bin` (die vertraute `floor`-Funktion zum Abrunden) ist hierfür die einfachste Möglichkeit.

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
       | summarize count=sum(itemCount)  
         by bin(timestamp, 1h)
```

Standardmäßig werden Ergebnisse als Tabelle angezeigt:

![](./media/app-insights-analytics-tour/225.png)

Es gibt jedoch noch eine bessere Ansicht als die Tabelle. Betrachten Sie die Ergebnisse in der Diagrammansicht mit vertikalem Balken:

![Klicken Sie auf „Diagramm“, wählen Sie dann „Vertikales Balkendiagramm“, und weisen Sie x- und y-Achsen zu.](./media/app-insights-analytics-tour/230.png)

Beachten Sie, dass die Diagrammdarstellung die Uhrzeitangaben immer in der richtigen Reihenfolge anzeigt, auch wenn wir die Ergebnisse nicht nach Zeit sortiert haben (wie es in der Tabellenansicht der Fall ist).


## <a name="timecharts"></a>Zeitdiagramme
Zeigen Sie an, wie viele Ereignisse pro Stunde erfolgen:

```AIQL

    requests
      | summarize event_count=sum(itemCount)
        by bin(timestamp, 1h)
```

Wählen Sie die Diagrammanzeigeoption aus:

![Zeitdiagramm](./media/app-insights-analytics-tour/080.png)

## <a name="multiple-series"></a>Mehrere Reihen
Mit mehreren Ausdrücken in der `summarize`-Klausel werden mehrere Spalten erstellt.

Mit mehreren Ausdrücken in der `by`-Klausel werden mehrere Zeilen erstellt, und zwar eine für jede Kombination der Werte.

```AIQL

    requests
    | summarize count_=sum(itemCount), avg(duration)
      by bin(timestamp, 1h), client_StateOrProvince, client_City
    | order by timestamp asc, client_StateOrProvince, client_City
```

![Tabelle der Anforderungen nach Stunde und Standort](./media/app-insights-analytics-tour/090.png)

### <a name="segment-a-chart-by-dimensions"></a>Segmentieren eines Diagramms nach Dimensionen
Wenn Sie aus einer Tabelle mit einer Zeichenfolgenspalte und einer Zahlenspalte ein Diagramm erstellen, kann die Zeichenfolge verwendet werden, um die numerischen Daten in separate Datenpunktreihen zu unterteilen. Falls mehr als eine Zeichenfolgenspalte vorhanden ist, können Sie wählen, welche Spalte als Diskriminator verwendet werden soll.

![Segmentieren eines Analysediagramms](./media/app-insights-analytics-tour/100.png)

#### <a name="bounce-rate"></a>Bounce-Rate

Konvertieren Sie einen booleschen Wert in eine Zeichenfolge, um sie als Diskriminator zu verwenden:

```AIQL

    // Bounce rate: sessions with only one page view
    requests
    | where notempty(session_Id)
    | where tostring(operation_SyntheticSource) == "" // real users
    | summarize pagesInSession=sum(itemCount), sessionEnd=max(timestamp)
               by session_Id
    | extend isbounce= pagesInSession == 1
    | summarize count()
               by tostring(isbounce), bin (sessionEnd, 1h)
    | render timechart
```

### <a name="display-multiple-metrics"></a>Anzeigen mehrerer Metriken
Wenn Sie aus einer Tabelle mit mehr als einer Zahlenspalte ein Diagramm erstellen, können Sie zusätzlich zum Zeitstempel beliebige mögliche Kombinationen anzeigen.

![Segmentieren eines Analysediagramms](./media/app-insights-analytics-tour/110.png)

Sie müssen die Option **Don't Split** (Nicht trennen) aktivieren, um mehrere Zahlenspalten auswählen zu können. Wenn mehrere Zahlenspalten angezeigt werden, ist keine Trennung nach einer Zeichenfolgenspalte möglich.

## <a name="daily-average-cycle"></a>Durchschnittlicher Tageszyklus
Inwiefern variiert die Nutzung im Verlauf eines durchschnittlichen Tages?

Zählen Sie die Anforderungen nach Zeitmodule an einem Tag, unterteilt in Stunden:

```AIQL

    requests
    | where timestamp > ago(30d)  // Override "Last 24h"
    | where tostring(operation_SyntheticSource) == "" // real users
    | extend hour = bin(timestamp % 1d , 1h)
          + datetime("2016-01-01") // Allow render on line chart
    | summarize event_count=sum(itemCount) by hour
```

![Liniendiagramm der Stunden an einem durchschnittlichen Tag](./media/app-insights-analytics-tour/120.png)

> [!NOTE]
> Beachten Sie, dass Dauern derzeit in „datetime“-Angaben konvertiert werden müssen, um sie in einem Liniendiagramm anzuzeigen.
>
>

## <a name="compare-multiple-daily-series"></a>Vergleichen mehrerer täglicher Reihen
Inwieweit variiert die Nutzung im Verlauf des Tages in unterschiedlichen Ländern?

```AIQL

     requests  
     | where timestamp > ago(30d)  // Override "Last 24h"
     | where tostring(operation_SyntheticSource) == "" // real users
     | extend hour= floor( timestamp % 1d , 1h)
           + datetime("2001-01-01")
     | summarize event_count=sum(itemCount)
       by hour, client_CountryOrRegion
     | render timechart
```

![Trennen nach „client_CountryOrRegion“](./media/app-insights-analytics-tour/130.png)

## <a name="plot-a-distribution"></a>Darstellen einer Verteilung
Wie viele Sitzungen unterschiedlicher Längen gibt es?

```AIQL

    requests
    | where timestamp > ago(30d) // override "Last 24h"
    | where isnotnull(session_Id) and isnotempty(session_Id)
    | summarize min(timestamp), max(timestamp)
      by session_Id
    | extend sessionDuration = max_timestamp - min_timestamp
    | where sessionDuration > 1s and sessionDuration < 3m
    | summarize count() by floor(sessionDuration, 3s)
    | project d = sessionDuration + datetime("2016-01-01"), count_
```

Die letzte Zeile wird zum Konvertieren von Datum und Uhrzeit benötigt. Derzeit wird die x-Achse eines Diagramms nur als Skalar angezeigt, wenn sie das Datum/Uhrzeit-Format aufweist.

Die `where` -Klausel schließt einmalige Sitzungen (sessionDuration==0) aus und legt die Länge der x-Achse fest.

![](./media/app-insights-analytics-tour/290.png)

## <a name="percentilesapp-insights-analytics-referencemdpercentiles"></a>[Quantile](app-insights-analytics-reference.md#percentiles)
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

Wir haben auch die Obergrenze in der Where-Klausel entfernt, um korrekte Werte zu erhalten (einschließlich aller Sitzungen mit mehreren Anforderungen):

![result](./media/app-insights-analytics-tour/180.png)

Daraus lässt sich Folgendes ablesen:

* 5 % der Sitzungen sind kürzer als 3 Minuten 34 Sekunden.
* 50 % der Sitzungen sind kürzer als 36 Minuten.
* 5 % der Sitzungen dauern länger als 7 Tage.

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

## <a name="join"></a>Join
Wir haben Zugriff auf mehrere Tabellen, einschließlich der Anforderungen und Ausnahmen.

Um Ausnahmen im Zusammenhang mit einer Anforderung zu suchen, die eine Fehlerantwort zurückgegeben hat, können wir die Tabellen anhand von `session_Id`verknüpfen:

```AIQL

    requests
    | where toint(resultCode) >= 500
    | join (exceptions) on operation_Id
    | take 30
```


Es ist üblich, `project` zu verwenden, um vor dem Verknüpfen nur die Spalten auszuwählen, die wir benötigen.
In den gleichen Klauseln benennen wir die Zeitstempelspalte um.

## <a name="letapp-insights-analytics-referencemdlet-clause-assign-a-result-to-a-variable"></a>[let](app-insights-analytics-reference.md#let-clause): Zuweisen eines Ergebnisses zu einer Variablen

Verwenden Sie `let`, um die einzelnen Teile des vorherigen Ausdrucks zu trennen. Die Ergebnisse sind wie folgt unverändert:

```AIQL

    let bad_requests =
      requests
        | where  toint(resultCode) >= 500  ;
    bad_requests
    | join (exceptions) on session_Id
    | take 30
```

> [!Tip] 
> Fügen Sie im Analytics-Client keine Leerzeilen zwischen den Teilen der Abfrage ein. Stellen Sie sicher, dass Sie alles ausführen.
>

Konvertieren Sie mithilfe von `toscalar` eine einzelne Tabellenzelle in einen Wert:

```AIQL
let topCities =  toscalar (
   requests
   | summarize count() by client_City 
   | top n by count_ 
   | summarize makeset(client_City));
requests
| where client_City in (topCities(3)) 
| summarize count() by client_City;
```


### <a name="functions"></a>Functions

Verwenden Sie *Let*, um eine Funktion zu definieren:

```AIQL

    let usdate = (t:datetime)
    {
      strcat(getmonth(t), "/", dayofmonth(t),"/", getyear(t), " ",
      bin((t-1h)%12h+1h,1s), iff(t%24h<12h, "AM", "PM"))
    };
    requests  
    | extend PST = usdate(timestamp-8h)
```

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

So überprüfen Sie, ob eine benutzerdefinierte Dimension einen bestimmten Typ hat

```AIQL

    customEvents
    | extend p1 = customDimensions.p1,
      iff(notnull(todouble(customMeasurements.m1)), ...
```

## <a name="dashboards"></a>Dashboards
Sie können Ihre Ergebnisse an ein Dashboard anheften, um eine Übersicht über Ihre wichtigsten Diagramme und Tabellen zu erhalten.

* [Freigegebenes Azure-Dashboard](app-insights-dashboards.md#share-dashboards): Klicken Sie auf das Stecknadelsymbol. Bevor Sie dies tun, müssen Sie über ein freigegebenes Dashboard verfügen. Öffnen Sie im Azure-Portal ein Dashboard (oder erstellen Sie es), und klicken Sie auf „Freigeben“.
* [Power BI-Dashboard](app-insights-export-power-bi.md): Klicken Sie auf „Export“ > „Power BI Query“ (Exportieren > Power BI-Abfrage). Ein Vorteil dieser alternativen Vorgehensweise besteht darin, dass Sie Ihre Abfrage neben anderen Ergebnissen aus unterschiedlichen Quellen anzeigen können.

## <a name="combine-with-imported-data"></a>Kombinieren mit importierten Daten

Analytics-Berichte sehen im Dashboard zwar gut aus, doch mitunter möchten Sie die Daten in ein übersichtlicheres Format übersetzen. Angenommen, Ihre authentifizierten Benutzer werden in den Telemetriedaten mittels eines Alias identifiziert. Sie möchten jedoch, dass in den Ergebnissen ihre echten Namen angezeigt werden. Zu diesem Zweck benötigen Sie eine CSV-Datei, mit deren Hilfe die Aliase den tatsächlichen Namen zugeordnet werden.

Sie können eine Datendatei importieren und genau wie Standardtabellen (Anforderungen, Ausnahmen usw.) nutzen. Fragen Sie sie entweder einzeln ab, oder verknüpfen Sie sie mit anderen Tabellen. Angenommen, Sie haben eine Tabelle mit dem Namen „usermap“ mit den Spalten `realName` und `userId`. Diese können Sie verwenden, um sie in das Feld `user_AuthenticatedId` in den Anforderungstelemetriedaten zu übersetzen:

```AIQL

    requests
    | where notempty(user_AuthenticatedId)
    | project userId = user_AuthenticatedId
      // get the realName field from the usermap table:
    | join kind=leftouter ( usermap ) on userId
      // count transactions by name:
    | summarize count() by realName
```

Wenn Sie eine Tabelle importieren möchten, halten Sie sich auf dem Schemablatt unter **Other Data Sources** (Andere Datenquellen) an die Anweisungen zum Hinzufügen einer neuen Datenquelle, und laden Sie eine Stichprobe Ihrer Daten hoch. Diese Definition können Sie dann zum Hochladen von Tabellen verwenden.

Das Importfeature befindet sich momentan in der Vorschauphase, daher wird unter „Other Data Sources“ (Andere Datenquellen) zunächst ein Kontaktlink angezeigt. Verwenden Sie diesen Link, um sich für das Vorschauprogramm zu registrieren. Danach wird der Link durch eine Schaltfläche zum Hinzufügen neuer Datenquellen ersetzt.


## <a name="tables"></a>Tabellen
Sie können über mehrere Tabellen auf den Telemetriedatenstrom aus Ihrer App zugreifen. Das Schema der verfügbaren Eigenschaften für jede Tabelle wird auf der linken Seite des Fensters angezeigt.

### <a name="requests-table"></a>Tabelle „Anforderungen“
Anzahl der HTTP-Anforderungen an Ihre Web-App und Segmente nach Seitenname:

![Anzahl der Anforderungen, nach Namen unterteilt](./media/app-insights-analytics-tour/analytics-count-requests.png)

Suchen der Anforderungen mit den häufigsten Fehlern:

![Anzahl der Anforderungen, nach Namen unterteilt](./media/app-insights-analytics-tour/analytics-failed-requests.png)

### <a name="custom-events-table"></a>Tabelle „Benutzerdefinierte Ereignisse“
Wenn Sie Ihre eigenen Ereignisse über [TrackEvent()](app-insights-api-custom-events-metrics.md#trackevent) senden, können Sie sie dieser Tabelle entnehmen.

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
Wenn Sie eigene Metrikwerte über [TrackMetric()](app-insights-api-custom-events-metrics.md#trackmetric) senden, finden Sie die Ergebnisse im Strom **CustomMetrics**. Beispiel:  

![Benutzerdefinierte Metriken in der Application Insights-Analyse](./media/app-insights-analytics-tour/analytics-custom-metrics.png)

> [!NOTE]
> Im [Metrik-Explorer](app-insights-metrics-explorer.md) werden auf dem Blatt „Metriken“ alle benutzerdefinierten und an einen Telemetrietyp angefügten Messungen zusammen mit den Metriken angezeigt, die mithilfe von `TrackMetric()` gesendet wurden. In Analytics hingegen bleiben die benutzerdefinierten Messungen weiterhin an den jeweiligen Telemetrietyp (Ereignisse, Anforderungen etc.) gebunden, und die über TrackMetric gesendeten Metriken werden in ihrem eigenen Datenstrom angezeigt.
>
>

### <a name="performance-counters-table"></a>Tabelle „Leistungsindikatoren“
[Leistungsindikatoren](app-insights-performance-counters.md) zeigen Ihnen, grundlegende Systemmetriken für Ihre App, wie z. B. CPU, Arbeitsspeicher und Netzwerkauslastung. Sie können das SDK zum Senden von zusätzlicher Indikatoren, z. B. benutzerdefinierten Leistungsindikatoren konfigurieren.

Das Schema **performanceCounters** zeigt die `category`, den `counter`-Namen und `instance`-Namen der einzelnen Leistungsindikatoren. Namen von Leistungsindikatorinstanzen gelten nur für einige Leistungsindikatoren und enthalten in der Regel den Namen des Prozesses, auf den sich die Anzahl bezieht. In den Telemetriedaten jeder Anwendung sehen Sie nur die Indikatoren für diese Anwendung. Beispielsweise, um verfügbare Leistungsindikatoren anzuzeigen:

![Leistungsindikatoren in der Application Insights-Analyse](./media/app-insights-analytics-tour/analytics-performance-counters.png)

So erhalten Sie ein Diagramm des verfügbaren Arbeitsspeichers im ausgewählten Zeitraum

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

### <a name="availability-results-table"></a>Tabelle „Verfügbarkeitsergebnisse“
`availabilityResults` zeigt die Ergebnisse Ihrer [Webtests](app-insights-monitor-web-app-availability.md) an. Jede Ausführung Ihrer Tests an jedem Teststandort wird separat gemeldet.

![Seitenladezeiten in Analytics](./media/app-insights-analytics-tour/analytics-availability.png)

### <a name="dependencies-table"></a>Tabelle „Abhängigkeiten“
Enthält die Ergebnisse der Datenbank- und REST-API-Aufrufe Ihrer Anwendung sowie anderer TrackDependency()-Aufrufe. Umfasst auch AJAX-Aufrufe aus dem Browser.

AJAX-Aufrufe aus dem Browser:

```AIQL

    dependencies | where client_Type == "Browser"
    | take 10
```

Abhängigkeitsaufrufe vom Server:

```AIQL

    dependencies | where client_Type == "PC"
    | take 10
```

Serverseitige Abhängigkeitsergebnisse enthalten immer `success==False`, wenn der Application Insights-Agent nicht installiert ist. Die anderen Daten sind allerdings richtig.

### <a name="traces-table"></a>Tabelle „Ablaufverfolgungen“
Enthält die von Ihrer App über TrackTrace() oder [andere Frameworks](app-insights-asp-net-trace-logs.md) gesendeten Telemetriedaten.



## <a name="next-steps"></a>Nächste Schritte
* [Analytics-Sprachreferenz](app-insights-analytics-reference.md)
* In der [Kurzübersicht für SQL-Benutzer](https://aka.ms/sql-analytics) finden Sie eine Übersetzung der gängigsten Sprachen.

[!INCLUDE [app-insights-analytics-footer](../../includes/app-insights-analytics-footer.md)]

