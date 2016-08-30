<properties 
	pageTitle="Verwenden von Analytics, dem leistungsfähigen Suchtool von Application Insights | Microsoft Azure" 
	description="Verwenden von Analytics, dem leistungsfähigen Diagnosesuchtool von Application Insights " 
	services="application-insights" 
    documentationCenter=""
	authors="danhadari" 
	manager="douge"/>

<tags 
	ms.service="application-insights" 
	ms.workload="tbd" 
	ms.tgt_pltfrm="ibiza" 
	ms.devlang="na" 
	ms.topic="article" 
	ms.date="07/26/2016" 
	ms.author="danha"/>


# Verwenden von Analytics in Application Insights


[Analytics](app-insights-analytics.md) ist die leistungsfähige Suchfunktion von [Application Insights](app-insights-overview.md). Auf diesen Seiten wird die Analytics-Abfragesprache beschrieben.

* **[Sehen Sie sich das Einführungsvideo an](https://applicationanalytics-media.azureedge.net/home_page_video.mp4)**.
* **[Testen Sie Analytics mit unseren simulierten Daten](https://analytics.applicationinsights.io/demo)**, wenn Ihre App noch keine Daten an Application Insights sendet.

## Öffnen von Analytics

Klicken Sie auf der Startseite Ihrer App in Application Insights auf Analytics.

![Öffnen Sie unter „portal.azure.com“ die Application Insights-Ressource, und wählen Sie „Analytics“.](./media/app-insights-analytics-using/001.png)

Das Inlinetutorial bietet Ihnen Anregungen, wozu Sie Analytics verwenden können.

Eine [ausführlichere Einführung finden Sie hier](app-insights-analytics-tour.md).

## Abfragen der Telemetrie

### Schreiben Sie eine Abfrage.

![Schemaanzeige](./media/app-insights-analytics-using/150.png)

Beginnen Sie mit den Namen der Tabellen auf der linken Seite (oder mit den Operatoren [range](app-insights-analytics-reference.md#range-operator) oder [union](app-insights-analytics-reference.md#union-operator)). Verwenden Sie `|`, um eine Pipeline von [Operatoren](app-insights-analytics-reference.md#queries-and-operators) zu erstellen. IntelliSense schlägt Ihnen die Operatoren und einige der Ausdruckselemente vor, die Sie verwenden können.

Weitere Informationen finden Sie in der [Einführung in Analytics in Application Insights](app-insights-analytics-tour.md) sowie in der [Sprachreferenz](app-insights-analytics-reference.md).

### Ausführen einer Abfrage

![Ausführen einer Abfrage](./media/app-insights-analytics-using/130.png)

1. Einzelne Zeilenumbrüche können Sie in einer Abfrage verwenden.
2. Platzieren Sie den Cursor in oder am Ende der Abfrage, die Sie ausführen möchten.
3. Klicken Sie auf „Los“, um die Abfrage auszuführen.
4. Verwenden Sie keine Leerzeilen in Ihrer Abfrage. Mehrere separate Abfragen können auf einer Abfrageregisterkarte durch Leerzeilen getrennt werden. Nur das Element mit dem Cursor wird ausgeführt.

### Speichern einer Abfrage

![Speichern einer Abfrage](./media/app-insights-analytics-using/140.png)

1. Aktuelle Abfragedatei speichern
2. Gespeicherte Abfragedatei öffnen
3. Neue Abfragedatei erstellen


## Details

Erweitern Sie in den Ergebnissen eine beliebige Zeile, um eine umfassende Liste mit Eigenschaften anzuzeigen. Eigenschaften, bei denen es sich um einen strukturierten Wert handelt, können nochmals erweitert werden. Beispiele wären etwa benutzerdefinierte Dimensionen oder die Stapellisten in einer Ausnahme.

![Erweitern einer Zeile](./media/app-insights-analytics-using/070.png)

 

## Anordnen der Ergebnisse

Die zurückgegebenen Abfrageergebnisse können sortiert, gefiltert, umgebrochen und gruppiert werden.

> [AZURE.NOTE] Beim Sortieren, Gruppieren und Filtern im Browser wird Ihre Abfrage nicht erneut ausgeführt. Hierbei werden lediglich die Ergebnisse Ihrer letzten Abfrage neu angeordnet.
> 
> Wenn diese Aufgaben auf dem Server ausgeführt werden sollen, bevor die Ergebnisse zurückgegeben werden, verwenden Sie in Ihrer Abfrage die Operatoren [sort](app-insights-analytics-reference.md#sort-operator), [summarize](app-insights-analytics-reference.md#summarize-operator) und [where](app-insights-analytics-reference.md#where-operator).

Wählen Sie die anzuzeigenden Spalten aus, ordnen Sie Spaltenüberschriften durch Ziehen neu an, um passen Sie die Spaltengröße durch Ziehen des jeweiligen Rahmens an.

![Anordnen von Spalten](./media/app-insights-analytics-using/030.png)

### Sortieren und Filtern von Elementen

Sortieren Sie Ihre Ergebnisse durch Klicken auf eine Spaltenüberschrift. Klicken Sie erneut, um die Sortierung umzukehren. Wenn Sie ein drittes Mal klicken, erhalten Sie wieder die ursprüngliche, von der Abfrage zurückgegebene Reihenfolge.

Über das Filtersymbol können Sie Ihre Suche eingrenzen.

![Sortieren und Filtern von Spalten](./media/app-insights-analytics-using/040.png)



### Gruppieren von Elementen

Wenn Sie nach mehreren Spalten sortieren möchten, verwenden Sie eine Gruppierung. Aktivieren Sie dazu zunächst die Funktion, und ziehen Sie dann Spaltenüberschriften in den Bereich über der Tabelle.

![Group](./media/app-insights-analytics-using/060.png)



### Werden nicht alle Ergebnisse angezeigt?

Die Anzahl von Ergebniszeilen, die vom Portal zurückgegeben werden können, ist auf ungefähr 10.000 Zeilen beschränkt. Bei einer Überschreitung des Grenzwerts wird eine Warnung angezeigt. In diesem Fall werden beim Sortieren der Ergebnisse in der Tabelle nicht immer alle ersten oder letzten Ergebnisse angezeigt.

Daher empfiehlt es sich, eine Überschreitung des Grenzwerts zu vermeiden. Verwenden beispielsweise folgende Operatoren:

* [where timestamp > ago(3d)](app-insights-analytics-reference.md#where-operator)
* [top 100 by timestamp](app-insights-analytics-reference.md#top-operator)
* [take 100](app-insights-analytics-reference.md#take-operator)
* [summarize](app-insights-analytics-reference.md#summarize-operator)



## Diagramme

Wählen Sie den gewünschten Diagrammtyp:

![Diagrammtyp auswählen](./media/app-insights-analytics-using/230.png)

Wenn Sie über mehrere Spalten mit den richtigen Typen verfügen, können Sie die X- und Y-Achse sowie eine Spalte mit Dimensionen auswählen, um die Ergebnisse aufzuteilen.

Die Ergebnisse werden standardmäßig zunächst als Tabelle angezeigt. Sie wählen die Diagrammdarstellung dann manuell aus. Sie können jedoch die [render-Anweisung](app-insights-analytics-reference.md#render-directive) am Ende einer Abfrage verwenden, um ein Diagramm auszuwählen.

## An das Dashboard anheften

Sie können ein Diagramm an eines Ihrer [freigegebenen Dashboards](app-insights-dashboards.md) anheften – klicken Sie einfach auf die Stecknadel. (Unter Umständen müssen Sie [das Tarifpaket für Ihre App aktualisieren](app-insights-pricing.md), um diese Funktion zu aktivieren.)

![Auf Stecknadel klicken](./media/app-insights-analytics-using/pin-01.png)

Wenn Sie ein Dashboard zum Überwachen der Leistung oder Nutzung Ihrer Webdienste erstellen, können Sie so zusätzlich zu den anderen Metriken komplexe Analysen hinzufügen.

#### Dashboard-Aktualisierung

Das an das Dashboard angeheftete Diagramm wird automatisch aktualisiert, indem Sie die Abfrage etwa jede halbe Stunde erneut ausführen.

#### Automatische Vereinfachungen

In manchen Fällen werden auf ein Diagramm bestimmte Vereinfachungen angewendet, wenn Sie es an ein Dashboard anheften.

Wenn Sie ein Diagramm mit zahlreichen separaten Containern (in der Regel ein Balkendiagramm) anheften, werden die Container mit wenig Daten automatisch in einem einzelnen Container zusammengefasst. Die folgende Abfrage:

    requests | summarize count_search = count() by client_CountryOrRegion

sieht in Analytics wie folgt aus:


![Ausführliches Diagramm](./media/app-insights-analytics-using/pin-07.png)

Wenn Sie sie an ein Dashboard anheften, sieht das folgendermaßen aus:


![Diagramm mit begrenzter Anzahl von Containern](./media/app-insights-analytics-using/pin-08.png)




## Exportieren in Excel

Nachdem Sie eine Abfrage ausgeführt haben, können Sie eine CSV-Datei herunterladen. Klicken Sie auf **Nach Excel exportieren**.

## Exportieren nach Power BI

1. Positionieren Sie den Cursor in einer Abfrage, und wählen Sie **Nach Power BI exportieren**.

    ![](./media/app-insights-analytics-using/240.png)

    Dadurch wird eine M-Skriptdatei heruntergeladen.

3. Kopieren Sie das M-Sprachskript in den erweiterten Abfrage-Editor von Power BI Desktop.
 * Öffnen Sie die exportierte Datei.
 * Wählen Sie in Power BI Desktop **Daten abrufen > leere Abfrage > Erweiterter Editor** aus, und fügen Sie das M-Sprachskript ein.

    ![](./media/app-insights-analytics-using/250.png)

4. Bearbeiten Sie die Anmeldeinformationen, falls erforderlich. Nun können Sie den Bericht erstellen.

    ![](./media/app-insights-analytics-using/260.png)




[AZURE.INCLUDE [app-insights-analytics-footer](../../includes/app-insights-analytics-footer.md)]

<!---HONumber=AcomDC_0817_2016-->