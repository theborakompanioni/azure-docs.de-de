<properties 
	pageTitle="Verwenden von Analytics, das leistungsfähige Suchtool von Application Insights" 
	description="Verwenden von Analytics, das leistungsfähige Diagnosesuchtool von Application Insights" 
	services="application-insights" 
    documentationCenter=""
	authors="alancameronwills" 
	manager="douge"/>

<tags 
	ms.service="application-insights" 
	ms.workload="tbd" 
	ms.tgt_pltfrm="ibiza" 
	ms.devlang="na" 
	ms.topic="article" 
	ms.date="03/30/2016" 
	ms.author="awills"/>




# Verwenden von Analytics in Application Insights


[Analytics](app-insights-analytics.md) ist die leistungsfähige Suchfunktion von [Application Insights](app-insights-overview.md). Auf diesen Seiten wird die Analytics-Abfragesprache beschrieben.

[AZURE.INCLUDE [app-insights-analytics-top-index](../../includes/app-insights-analytics-top-index.md)]

## Öffnen von Analytics

Klicken Sie auf der Startseite Ihrer App in Application Insights auf Analytics.

![Öffnen Sie unter „portal.azure.com“ die Application Insights-Ressource, und wählen Sie „Analytics“.](./media/app-insights-analytics/001.png)

Das Inlinetutorial bietet Ihnen Anregungen, wozu Sie Analytics verwenden können.

Eine [ausführlichere Einführung finden Sie hier](app-insights-analytics-tour.md).

## Schreiben von Abfragen

Schreiben Sie eine Abfrage, indem Sie mit den Namen der Tabellen auf der linken Seite beginnen. Verwenden Sie `|` zum Erstellen einer Pipeline von [Operatoren](app-insights-analytics-queries.md).


![](./media/app-insights-analytics-using/150.png)

* Verwenden Sie keine Leerzeilen in Ihrer Abfrage.
* Einzelne Zeilenumbrüche können Sie in einer Abfrage verwenden.
* Außerdem können Sie mehrere Abfragen im Fenster beibehalten, indem Sie sie durch Leerzeilen trennen.
* Zum Ausführen einer Abfrage **setzen Sie den Cursor in die Mitte oder an das Ende der Abfrage**, und klicken Sie auf „Weiter“.


![](./media/app-insights-analytics-using/130.png)

* Sie können den Inhalt des Abfragefensters speichern und wieder aufrufen.

![](./media/app-insights-analytics-using/140.png)

## Anordnen der Ergebnisse

Sie können die Spalten auswählen, die Sie anzeigen möchten. Erweitern Sie ein beliebiges Element, um alle zurückgegebenen Spaltenwerte anzuzeigen.

![](./media/app-insights-analytics-using/030.png)

> [AZURE.NOTE] Klicken Sie auf den Anfang einer Spalte, um die im Webbrowser zur Verfügung stehenden Ergebnisse schnell neu zu ordnen. Beachten Sie jedoch, dass die Anzahl von in den Browser heruntergeladen Zeilen bei einem großen Resultset beschränkt ist. Diese Sortierung zeigt Ihnen aus diesem Grund nicht unbedingt die tatsächlichen höchsten oder niedrigsten Elemente an. Dazu sollten Sie den [top](app-insights-analytics-queries.md#top-operator)- oder [sort](app-insights-analytics-queries.md#sort-operator)-Operator verwenden.

Es ist allerdings üblich, die [take](app-insights-analytics-queries.md#take-operator)-, [top](app-insights-analytics-queries.md#top-operator)- oder [summarize](app-insights-analytics-queries.md#summarize-operator)-Operatoren zu verwenden, um zu vermeiden, dass riesige Tabellen vom Server heruntergeladen werden. Die automatische Beschränkung liegt ohnehin bei 10.000 Zeilen pro Abfrage.


## Diagramme

Wählen Sie den gewünschten Diagrammtyp:

![](./media/app-insights-analytics-using/230.png)

Wenn Sie über mehrere Spalten mit den richtigen Typen verfügen, können Sie die x- und y-Achsen und eine Spalte mit Dimensionen auswählen, um die Ergebnisse wie folgt aufzuteilen:

![](./media/app-insights-analytics-using/100.png)

Die Ergebnisse werden standardmäßig zunächst als Tabelle angezeigt. Sie wählen die Diagrammdarstellung dann manuell aus. Sie können jedoch die [render-Anweisung](app-insights-analytics-queries.md#render-directive) am Ende einer Abfrage verwenden, um ein Diagramm auszuwählen.

## Exportieren in Excel

Nachdem Sie eine Abfrage ausgeführt haben, können Sie eine CSV-Datei herunterladen. Klicken Sie auf **In Excel Exportieren**.

## Exportieren nach Power BI

1. Positionieren Sie den Cursor in einer Abfrage, und wählen Sie **Nach Power BI Exportieren**.

    ![](./media/app-insights-analytics-using/240.png)

    Dadurch wird eine M-Skriptdatei heruntergeladen.

3. Kopieren Sie das M-Sprachskript in den erweiterten Abfrage-Editor von Power BI Desktop.
 * Öffnen Sie die exportierte Datei.
 * Wählen Sie in Power BI Desktop **Daten abrufen, leere Abfrage, Erweiterter Editor** aus, und fügen Sie das M-Sprachskript ein.

    ![](./media/app-insights-analytics-using/250.png)

4. Bearbeiten Sie die Anmeldeinformationen, falls erforderlich. Nun können Sie den Bericht erstellen.

    ![](./media/app-insights-analytics-using/260.png)



[AZURE.INCLUDE [app-insights-analytics-footer](../../includes/app-insights-analytics-footer.md)]

<!---HONumber=AcomDC_0330_2016-->