<properties 
	pageTitle="Arbeiten mit Application Insights in Visual Studio" 
	description="Leistungsanalyse und Diagnose während Debugging und Produktion" 
	services="application-insights" 
    documentationCenter=".net"
	authors="alancameronwills" 
	manager="douge"/>

<tags 
	ms.service="application-insights" 
	ms.workload="tbd" 
	ms.tgt_pltfrm="ibiza" 
	ms.devlang="na" 
	ms.topic="get-started-article" 
	ms.date="06/21/2016" 
	ms.author="awills"/>


# Arbeiten mit Application Insights in Visual Studio

In Visual Studio (2015 und höher) können Sie mithilfe von Telemetriedaten aus [Visual Studio Application Insights](app-insights-overview.md) sowohl beim Debuggen als auch in der Produktion die Leistung analysieren und Probleme diagnostizieren.

[Wenn Sie Application Insights noch nicht in Ihrer App installiert haben](app-insights-asp-net.md), holen Sie das jetzt nach.

## <a name="run"></a> Debuggen des Projekts

Starten Sie Ihre Anwendung mit F5, und probieren Sie es aus: Öffnen Sie verschiedene Seiten, um einige Telemetriedaten zu generieren.

In Visual Studio sehen Sie eine Anzahl der protokollierten Ereignisse.

![In Visual Studio wird die Schaltfläche „Application Insights“ während des Debuggens angezeigt.](./media/app-insights-visual-studio/appinsights-09eventcount.png)

Klicken Sie auf diese Schaltfläche, um die Diagnosesuche zu öffnen.



## Diagnosesuche

Im Suchfenster werden die protokollierten Ereignisse angezeigt. (Wenn Sie sich bei der Einrichtung von Application Insights bei Azure angemeldet haben, können Sie die gleichen Ereignisse im Portal durchsuchen.)

![Klicken Sie mit der rechten Maustaste auf das Projekt, und wählen Sie „Application Insights > Durchsuchen“ aus.](./media/app-insights-visual-studio/34.png)

Die Freitextsuche funktioniert in allen Feldern in den Ereignissen. Suchen Sie z. B. einen Teil der URL einer Seite, den Wert einer Eigenschaft, z. B. Ort des Kunden, oder bestimmte Wörter in einem Ablaufverfolgungsprotokoll.

Klicken Sie auf ein beliebiges Ereignis, um seine detaillierten Eigenschaften anzuzeigen.

Sie können zum Diagnostizieren von fehlerhaften Anforderungen oder von Ausnahmen auch die Registerkarte „Verwandte Elemente“ öffnen.


![](./media/app-insights-visual-studio/41.png)



## Diagnosehub

Der Diagnosehub zeigt (ab Visual Studio 2015) die von Application Insights generierten Servertelemetriedaten. Dies funktioniert auch, wenn Sie sich entschieden haben, nur das SDK zu installieren, ohne es mit einer Ressource im Azure-Portal zu verbinden.

![Öffnen Sie das Fenster „Diagnosetools“, und überprüfen Sie die Application Insights-Ereignisse.](./media/app-insights-visual-studio/31.png)


## Ausnahmen

Wenn Sie [Ausnahmeüberwachung eingerichtet haben](app-insights-asp-net-exceptions.md), werden im Suchfenster Ausnahmeberichte angezeigt.

Klicken Sie auf eine Ausnahme, um eine Stapelüberwachung zu erhalten. Wenn der Code der App in Visual Studio geöffnet ist, können Sie sich durch die Stapelüberwachung bis zur entsprechenden Zeile im Code klicken.


![Ausnahmestapelüberwachung](./media/app-insights-visual-studio/17.png)

Außerdem wird in der Codebereichszeile oberhalb jeder Methode die Anzahl von Ausnahmen angezeigt, die von Application Insights innerhalb der letzten 24 Stunden protokolliert wurden.

![Ausnahmestapelüberwachung](./media/app-insights-visual-studio/21.png)


## Lokale Überwachung



(In Visual Studio 2015 Update 2) Falls Sie das SDK nicht zum Senden von Telemetriedaten an das Application Insights-Portal konfiguriert haben (sodass kein Instrumentationsschlüssel in „ApplicationInsights.config“ enthalten ist), werden im Diagnosefenster Telemetriedaten aus der letzten Debugsitzung angezeigt.

Dies ist wünschenswert, wenn Sie bereits eine frühere Version der Anwendung veröffentlicht haben. Die Telemetriedaten aus den Debugsitzungen und die Telemetriedaten der veröffentlichten App im Application Insights-Portal müssen getrennt behandelt werden.

Dies ist ebenfalls hilfreich, wenn Sie über [benutzerdefinierte Telemetriedaten](app-insights-api-custom-events-metrics.md) verfügen, die Sie vor dem Senden an das Portal debuggen möchten.


* *Zunächst habe ich Application Insights vollständig für das Senden von Telemetriedaten an das Portal konfiguriert. Aber nun möchte ich die Telemetriedaten nur in Visual Studio anzeigen.*

 * In den Einstellungen des Suchfensters steht eine Option zum Durchsuchen der lokalen Diagnosen zur Verfügung, auch wenn Ihre App Telemetriedaten an das Portal sendet.
 * Damit keine Telemetriedaten mehr an das Portal gesendet werden, kommentieren Sie die Zeile `<instrumentationkey>...` in „ApplicationInsights.config“ aus. Wenn Sie bereit sind, Telemetriedaten erneut an das Portal zu senden, heben Sie die Auskommentierung auf.

## Trends

Mit dem Tool Trends können Sie visualisieren, wie sich Ihre App im Laufe der Zeit verhält.

Wählen Sie auf der Application Insights-Symbolleiste oder im Application Insights-Suchfenster die Schaltfläche **Telemetrietrends untersuchen**. Wählen Sie eine der fünf allgemeinen Abfragen aus, um zu beginnen. Sie können unterschiedliche Datasets basierend auf Telemetrietypen, Zeiträumen und anderen Eigenschaften analysieren.

Wählen Sie zum Ermitteln von Anomalien in Ihren Daten in der Dropdownliste „Ansichtstyp“ eine Anomalieoption. Mit den Filteroptionen am unteren Rand des Fensters ist es einfach, bestimmte Teilmengen Ihrer Telemetriedaten anzuzeigen.

![Trends](./media/app-insights-visual-studio/51.png)


## Wie geht es weiter?

||
|---|---
|**[Hinzufügen weiterer Daten](app-insights-asp-net-more.md)**<br/>Überwachen Sie Nutzung, Verfügbarkeit, Abhängigkeiten und Ausnahmen. Integrieren Sie Ablaufverfolgungen aus Protokollierungsframeworks. Schreiben Sie benutzerdefinierte Telemetriedaten. | ![Visual Studio](./media/app-insights-asp-net/64.png)
|**[Navigation und Dashboards im Application Insights-Portal](app-insights-dashboards.md)**<br/> Enthält Informationen zu Dashboards, leistungsstarken Diagnose- und Analysetools, Warnungen, eine Livekarte der Abhängigkeiten Ihrer Anwendung und zum Export von Telemetriedaten. |![Visual Studio](./media/app-insights-asp-net/62.png)


 

<!---HONumber=AcomDC_0622_2016-->