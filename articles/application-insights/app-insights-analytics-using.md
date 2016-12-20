---
title: "Verwenden von Analytics, dem leistungsfähigen Suchtool von Application Insights | Microsoft Docs"
description: "Verwenden von Analytics, dem leistungsfähigen Diagnosesuchtool von Application Insights "
services: application-insights
documentationcenter: 
author: danhadari
manager: douge
ms.assetid: c3b34430-f592-4c32-b900-e9f50ca096b3
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 11/16/2016
ms.author: awills
translationtype: Human Translation
ms.sourcegitcommit: 7bd26ffdec185a1ebd71fb88383c2ae4cd6d504f
ms.openlocfilehash: f9c02c11c6f0143f8da7a329f23033120f31ba59


---
# <a name="using-analytics-in-application-insights"></a>Verwenden von Analytics in Application Insights
[Analytics](app-insights-analytics.md) ist die leistungsfähige Suchfunktion von [Application Insights](app-insights-overview.md). Auf diesen Seiten wird die Analytics-Abfragesprache beschrieben.

* **[Sehen Sie sich das Einführungsvideo an.](https://applicationanalytics-media.azureedge.net/home_page_video.mp4)**
* **[Testen Sie Analytics mit unseren simulierten Daten](https://analytics.applicationinsights.io/demo)**, wenn Ihre App noch keine Daten an Application Insights sendet.

## <a name="open-analytics"></a>Öffnen von Analytics
Klicken Sie auf der Startseite Ihrer App in Application Insights auf Analytics.

![Öffnen Sie unter „portal.azure.com“ die Application Insights-Ressource, und wählen Sie „Analytics“.](./media/app-insights-analytics-using/001.png)

Das Inlinetutorial bietet Ihnen Anregungen, wozu Sie Analytics verwenden können.

Eine [ausführlichere Einführung finden Sie hier](app-insights-analytics-tour.md).

## <a name="query-your-telemetry"></a>Abfragen der Telemetrie
### <a name="write-a-query"></a>Schreiben Sie eine Abfrage.
![Schemaanzeige](./media/app-insights-analytics-using/150.png)

Beginnen Sie mit den Namen der Tabellen auf der linken Seite (oder mit den Operatoren [range](app-insights-analytics-reference.md#range-operator) oder [union](app-insights-analytics-reference.md#union-operator)). Verwenden Sie `|` , um eine Pipeline von [Operatoren](app-insights-analytics-reference.md#queries-and-operators)zu erstellen. 

IntelliSense schlägt Ihnen die Operatoren und Ausdruckselemente vor, die Sie verwenden können. Klicken Sie auf das Informationssymbol (oder drücken Sie STRG+LEERTASTE), um eine längere Beschreibung und Beispiele zum Verwenden der einzelnen Elemente anzuzeigen.

Weitere Informationen finden Sie in der [Einführung in Analytics in Application Insights](app-insights-analytics-tour.md) sowie in der [Sprachreferenz](app-insights-analytics-reference.md).

### <a name="run-a-query"></a>Ausführen einer Abfrage
![Ausführen einer Abfrage](./media/app-insights-analytics-using/130.png)

1. Einzelne Zeilenumbrüche können Sie in einer Abfrage verwenden.
2. Platzieren Sie den Cursor in oder am Ende der Abfrage, die Sie ausführen möchten.
3. Überprüfen Sie den Zeitbereich der Abfrage. (Diesen können Sie ändern oder überschreiben, indem Sie eine eigene [`where...timestamp...`](app-insights-analytics-tour.md#time-range)-Klausel in der Abfrage einfügen.)
3. Klicken Sie auf „Los“, um die Abfrage auszuführen.
4. Verwenden Sie keine Leerzeilen in Ihrer Abfrage. Mehrere separate Abfragen können auf einer Abfrageregisterkarte durch Leerzeilen getrennt werden. Es wird nur die Abfrage ausgeführt, in der der Cursor platziert ist.

### <a name="save-a-query"></a>Speichern einer Abfrage
![Speichern einer Abfrage](./media/app-insights-analytics-using/140.png)

1. Aktuelle Abfragedatei speichern
2. Gespeicherte Abfragedatei öffnen
3. Neue Abfragedatei erstellen

## <a name="see-the-details"></a>Details
Erweitern Sie in den Ergebnissen eine beliebige Zeile, um eine umfassende Liste mit Eigenschaften anzuzeigen. Eigenschaften, bei denen es sich um einen strukturierten Wert handelt, können nochmals erweitert werden. Beispiele wären etwa benutzerdefinierte Dimensionen oder die Stapellisten in einer Ausnahme.

![Erweitern einer Zeile](./media/app-insights-analytics-using/070.png)

## <a name="arrange-the-results"></a>Anordnen der Ergebnisse
Die zurückgegebenen Abfrageergebnisse können sortiert, gefiltert, umgebrochen und gruppiert werden.

> [!NOTE]
> Beim Sortieren, Gruppieren und Filtern im Browser wird Ihre Abfrage nicht erneut ausgeführt. Hierbei werden lediglich die Ergebnisse Ihrer letzten Abfrage neu angeordnet. 
> 
> Wenn diese Aufgaben auf dem Server ausgeführt werden sollen, bevor die Ergebnisse zurückgegeben werden, verwenden Sie in Ihrer Abfrage die Operatoren [sort](app-insights-analytics-reference.md#sort-operator), [summarize](app-insights-analytics-reference.md#summarize-operator) und [where](app-insights-analytics-reference.md#where-operator).
> 
> 

Wählen Sie die anzuzeigenden Spalten aus, ordnen Sie Spaltenüberschriften durch Ziehen neu an, um passen Sie die Spaltengröße durch Ziehen des jeweiligen Rahmens an.

![Anordnen von Spalten](./media/app-insights-analytics-using/030.png)

### <a name="sort-and-filter-items"></a>Sortieren und Filtern von Elementen
Sortieren Sie Ihre Ergebnisse durch Klicken auf eine Spaltenüberschrift. Klicken Sie erneut, um die Sortierung umzukehren. Wenn Sie ein drittes Mal klicken, erhalten Sie wieder die ursprüngliche, von der Abfrage zurückgegebene Reihenfolge.

Über das Filtersymbol können Sie Ihre Suche eingrenzen.

![Sortieren und Filtern von Spalten](./media/app-insights-analytics-using/040.png)

### <a name="group-items"></a>Gruppieren von Elementen
Wenn Sie nach mehreren Spalten sortieren möchten, verwenden Sie eine Gruppierung. Aktivieren Sie dazu zunächst die Funktion, und ziehen Sie dann Spaltenüberschriften in den Bereich über der Tabelle.

![Group](./media/app-insights-analytics-using/060.png)

### <a name="missing-some-results"></a>Werden nicht alle Ergebnisse angezeigt?

Wenn Sie denken, dass nicht alle erwarteten Ergebnisse angezeigt werden, kann dies mehrere mögliche Gründe haben.

* **Zeitbereichsfilter:** Standardmäßig werden nur die Ergebnisse der letzten 24 Stunden angezeigt. Durch einen automatischen Filter wird der Bereich der Ergebnisse beschränkt, die aus den Quelltabellen abgerufen werden. 

    Den Zeitbereichsfilter können Sie im entsprechenden Dropdownmenü jedoch ändern.

    Oder Sie können den automatischen Bereich überschreiben, indem Sie eine eigene [`where  ... timestamp ...`-Klausel](app-insights-analytics-reference.md#where-operator) in die Abfrage einfügen. Beispiel:

    `requests | where timestamp > ago('2d')`

* **Ergebnislimit:** Die Anzahl von Ergebniszeilen, die vom Portal zurückgegeben werden können, ist auf ungefähr 10.000 Zeilen beschränkt. Bei einer Überschreitung des Grenzwerts wird eine Warnung angezeigt. In diesem Fall werden beim Sortieren der Ergebnisse in der Tabelle nicht immer alle ersten oder letzten Ergebnisse angezeigt. 

    Daher empfiehlt es sich, eine Überschreitung des Grenzwerts zu vermeiden. Verwenden Sie den Zeitbereichsfilter oder beispielsweise die folgenden Operatoren:

  * [top 100 by timestamp](app-insights-analytics-reference.md#top-operator) 
  * [take 100](app-insights-analytics-reference.md#take-operator)
  * [summarize ](app-insights-analytics-reference.md#summarize-operator) 
  * [where timestamp > ago(3d)](app-insights-analytics-reference.md#where-operator)

(Sollen mehr als 10.000 Zeilen zurückgegeben werden? Stattdessen können Sie den [fortlaufenden Export](app-insights-export-telemetry.md) verwenden. Analytics ist für Analysen und nicht zum Abrufen von Rohdaten vorgesehen.)

## <a name="diagrams"></a>Diagramme
Wählen Sie den gewünschten Diagrammtyp:

![Diagrammtyp auswählen](./media/app-insights-analytics-using/230.png)

Wenn Sie über mehrere Spalten mit den richtigen Typen verfügen, können Sie die X- und Y-Achse sowie eine Spalte mit Dimensionen auswählen, um die Ergebnisse aufzuteilen.

Die Ergebnisse werden standardmäßig zunächst als Tabelle angezeigt. Sie wählen die Diagrammdarstellung dann manuell aus. Sie können jedoch die [render-Anweisung](app-insights-analytics-reference.md#render-directive) am Ende einer Abfrage verwenden, um ein Diagramm auszuwählen.

## <a name="pin-to-dashboard"></a>An Dashboard anheften
Sie können ein Diagramm oder eine Tabelle an eines Ihrer [freigegebenen Dashboards](app-insights-dashboards.md) anheften – klicken Sie einfach auf die Stecknadel. (Unter Umständen müssen Sie [das Tarifpaket für Ihre App aktualisieren](app-insights-pricing.md) , um diese Funktion zu aktivieren.) 

![Auf Stecknadel klicken](./media/app-insights-analytics-using/pin-01.png)

Wenn Sie ein Dashboard zum Überwachen der Leistung oder Nutzung Ihrer Webdienste erstellen, können Sie so zusätzlich zu den anderen Metriken komplexe Analysen hinzufügen. 

Wenn eine Tabelle maximal vier Spalten enthält, können Sie sie an das Dashboard anheften. Nur die ersten sieben Zeilen werden angezeigt.

### <a name="dashboard-refresh"></a>Dashboard-Aktualisierung
Das an das Dashboard angeheftete Diagramm wird automatisch aktualisiert, indem die Abfrage etwa alle zwei Stunden erneut ausgeführt wird.

### <a name="automatic-simplifications"></a>Automatische Vereinfachungen

Auf ein Diagramm werden bestimmte Vereinfachungen angewendet, wenn Sie es an ein Dashboard anheften.

**Zeiteinschränkung:** Abfragen sind automatisch auf die letzten 14 Tage begrenzt. Dies wirkt sich so aus, als enthielte die Abfrage `where timestamp > ago(14d)`.

**Maximale Anzahl von Containern:** Wenn Sie ein Diagramm mit zahlreichen separaten Containern (in der Regel ein Balkendiagramm) anzeigen, werden die Container mit wenig Daten automatisch in einem einzelnen Container zusammengefasst. Die folgende Abfrage:

    requests | summarize count_search = count() by client_CountryOrRegion

sieht in Analytics wie folgt aus:

![Ausführliches Diagramm](./media/app-insights-analytics-using/pin-07.png)

Wenn Sie sie an ein Dashboard anheften, sieht das folgendermaßen aus:

![Diagramm mit begrenzter Anzahl von Containern](./media/app-insights-analytics-using/pin-08.png)

## <a name="export-to-excel"></a>Exportieren in Excel
Nachdem Sie eine Abfrage ausgeführt haben, können Sie eine CSV-Datei herunterladen. Klicken Sie auf **Nach Excel exportieren**.

## <a name="export-to-power-bi"></a>Exportieren nach Power BI
Positionieren Sie den Cursor in einer Abfrage, und wählen Sie **Nach Power BI exportieren**.

![Exportieren von Analytics nach Power BI](./media/app-insights-analytics-using/240.png)

Sie führen die Abfrage in Power BI aus. Sie können sie auf der Grundlage eines Zeitplans aktualisieren.

Mit Power BI können Sie Dashboards erstellen, auf denen Daten aus verschiedenen Quellen zusammengefasst werden.

[Weitere Informationen zum Exportieren nach Power BI](app-insights-export-power-bi.md)


## <a name="automation"></a>Automation

Sie können Analytics-Abfragen über die [Data Access-REST-API](https://dev.applicationinsights.io/) ausführen, z.B. mithilfe von PowerShell.



## <a name="import-data"></a>Daten importieren

Sie können Daten aus einer CSV-Datei importieren. Typischerweise werden statische Daten importiert, die Sie mit Tabellen aus der Telemetrie verknüpfen können. 

Wenn authentifizierte Benutzer in der Telemetrie beispielsweise durch einen Alias oder eine verborgene ID identifiziert werden, können Sie eine Tabelle importieren, in der Aliase den tatsächlichen Namen zugeordnet sind. Durch Ausführen einer Verknüpfung der Anforderungstelemetriedaten können Sie Benutzer in den Analytics-Berichten durch ihre tatsächlichen Namen identifizieren.

### <a name="define-your-data-schema"></a>Definieren des Datenschemas

1. Klicken Sie auf **Einstellungen** (oben links) und dann auf **Datenquellen**. 
2. Fügen Sie entsprechend den Anweisungen eine Datenquelle hinzu. Sie werden aufgefordert, einen Teil der Daten anzugeben, der mindestens zehn Zeilen enthalten soll. Anschließend korrigieren Sie das Schema.

Damit wird eine Datenquelle definiert, die Sie dann zum Importieren einzelner Tabellen verwenden können.

### <a name="import-a-table"></a>Importieren einer Tabelle

1. Öffnen Sie die Datenquellendefinition in der Liste.
2. Klicken Sie auf „Hochladen“, und befolgen Sie die Anweisungen zum Hochladen der Tabelle. Dies umfasst einen Aufruf einer REST-API, sodass sich dieser Vorgang leicht automatisieren lässt. 

Die Tabelle steht nun zur Verwendung in Analytics-Abfragen zur Verfügung. Sie wird in Analytics angezeigt. 

### <a name="use-the-table"></a>Verwenden der Tabelle

Angenommen, die Datenquellendefinition heißt `usermap` und umfasst die beiden Felder `realName` und `user_AuthenticatedId`. Die Tabelle `requests` enthält zudem das Feld `user_AuthenticatedId`, sodass sie einfach verknüpft werden können:

```AIQL

    requests
    | where notempty(user_AuthenticatedId) | take 10
    | join kind=leftouter ( usermap ) on user_AuthenticatedId 
```
Die resultierende Tabelle der Anforderungen enthält die zusätzliche Spalte `realName`.

### <a name="import-from-logstash"></a>Importieren aus LogStash

Bei Verwendung von [LogStash](https://www.elastic.co/guide/en/logstash/current/getting-started-with-logstash.html) können Sie Protokolle mithilfe von Analytics abfragen. Verwenden Sie das [Plug-In, mit dem Daten an Analytics weitergeleitet werden](https://github.com/Microsoft/logstash-output-application-insights). 



[!INCLUDE [app-insights-analytics-footer](../../includes/app-insights-analytics-footer.md)]




<!--HONumber=Nov16_HO3-->


