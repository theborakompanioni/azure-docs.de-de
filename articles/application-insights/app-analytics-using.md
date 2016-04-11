<properties 
	pageTitle="Verwenden von Analytics, dem leistungsfähigen Suchtool von Application Insights" 
	description="Verwenden von Analytics, dem leistungsfähigen Diagnosesuchtool von Application Insights" 
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
	ms.date="03/23/2016" 
	ms.author="awills"/>




# Verwenden von Analytics in Application Insights

[Analytics](app-analytics.md) ermöglicht die Ausführung leistungsstarker Abfragen der von [Application Insights](app-insights-overview.md) gesammelten Telemetriedaten. Auf diesen Seiten wird die Analytics-Abfragesprache beschrieben.

[AZURE.INCLUDE [app-analytics-top-index](../../includes/app-analytics-top-index.md)]

## Öffnen von Analytics

Klicken Sie in der Start-Ressource Ihrer App in Application Insights auf Analytics.

![Öffnen Sie unter „portal.azure.com“ die Application Insights-Ressource, und wählen Sie „Analytics“.](./media/app-analytics/001.png)

Das Inlinetutorial wird Ihnen Anregungen geben, was Sie damit machen können.

Eine [ausführlichere Einführung finden Sie hier](app-analytics-tour.md).

## Schreiben von Abfragen

Schreiben Sie eine Abfrage, indem Sie mit den Namen der Tabellen auf der linken Seite beginnen. Verwenden Sie `|` zum Erstellen einer Pipeline von [Operatoren](app-analytics-queries.md).


![](./media/app-analytics-using/150.png)

* Verwenden Sie keine Leerzeilen in Ihrer Abfrage.
* Einzelne Zeilenumbrüche können Sie in einer Abfrage verwenden.
* Außerdem können Sie mehrere Abfragen im Fenster beibehalten, indem Sie sie durch Leerzeilen trennen.
* Zum Ausführen einer Abfrage **setzen Sie den Cursor in die Mitte oder an das Ende der abfrage**, und klicken Sie auf „Weiter“.


![](./media/app-analytics-using/130.png)

* Sie können den Inhalt des Abfragefensters speichern und wiederaufrufen.

![](./media/app-analytics-using/140.png)

## Anordnen der Ergebnisse

Sie können die Spalten auswählen, die Sie anzeigen möchten. Erweitern Sie ein beliebiges Element, um alle zurückgegebenen Spaltenwerte anzuzeigen.

![](./media/app-analytics-using/030.png)

> [AZURE.NOTE] Klicken Sie auf den Anfang einer Spalte, um die im Webbrowser zur Verfügung stehenden Ergebnisse schnell neu zu ordnen. Beachten Sie, dass die Anzahl der in den Browser heruntergeladen Zeilen bei einem großen Resultset jedoch beschränkt ist. Diese Sortierung zeigt Ihnen aus diesem Grund nicht unbedingt die tatsächlichen höchsten oder niedrigsten Elemente an. Dazu sollten Sie den [top](app-analytics-queries.md#top-operator)- oder [sort](app-analytics-queries.md#sort-operator)-Operator verwenden.

Allerdings ist es üblich, die [take](app-analytics-queries.md#take-operator)-, [top](app-analytics-queries.md#top-operator)- oder [summerize](app-analytics-queries.md#summarize-operator)-Operatoren zu verwenden, um zu vermeiden, dass riesige Tabellen vom Server heruntergeladen werden. Die automatische Beschränkung liegt ohnehin bei 10.000 Zeilen pro Abfrage.


## Diagramme

Wählen Sie den gewünschten Diagrammtyp:

![](./media/app-analytics-using/230.png)

Wenn Sie über mehrere Spalten mit den richtigen Typen verfügen, können Sie die x- und y-Achsen und eine Spalte der Dimensionen auswählen, um die Ergebnisse wie folgt aufzuteilen:

![](./media/app-analytics-using/100.png)

Die Ergebnisse werden standardmäßig zunächst als Tabelle angezeigt. Sie wählen die Diagrammdarstellung dann manuell aus. Sie können jedoch die [render-Anweisung](app-analytics-queries.md#render-directive) am Ende einer Abfrage verwenden, um ein Diagramm auszuwählen.

## Exportieren in Excel

Nachdem Sie eine Abfrage ausgeführt haben, können Sie eine CSV-Datei herunterladen. Klicken Sie auf **In Excel Exportieren**.

## Exportieren nach Power BI

1. Positionieren Sie den Cursor in einer Abfrage, und wählen Sie **Nach Power BI Exportieren**.

    ![](./media/app-analytics-using/240.png)

    Dadurch wird eine Skriptdatei M heruntergeladen.

3. Kopieren Sie das M-Sprachskript in den erweiterten Abfrage-Editor von Power BI Desktop.
 * Öffnen Sie die exportierte Datei.
 * Wählen Sie in Power BI Desktop **Daten abrufen, leere Abfrage, Erweiterter Editor** aus, und fügen Sie das M-Sprachskript ein.

    ![](./media/app-analytics-using/250.png)

4. Bearbeiten Sie die Anmeldeinformationen, falls erforderlich, und nun können Sie den Bericht erstellen.

    ![](./media/app-analytics-using/260.png)



[AZURE.INCLUDE [app-analytics-footer](../../includes/app-analytics-footer.md)]

<!---HONumber=AcomDC_0330_2016-->