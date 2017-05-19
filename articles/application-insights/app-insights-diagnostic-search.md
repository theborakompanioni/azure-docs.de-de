---
title: Verwenden von Search in Azure Application Insights | Microsoft-Dokumentation
description: Durchsuchen und filtern Sie unformatierte Telemetriedaten, die von Ihrer Web-App gesendet werden.
services: application-insights
documentationcenter: 
author: CFreemanwa
manager: carmonm
ms.assetid: 2a437555-8043-45ec-937a-225c9bf0066b
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 03/14/2017
ms.author: cfreeman
ms.translationtype: Human Translation
ms.sourcegitcommit: 785d3a8920d48e11e80048665e9866f16c514cf7
ms.openlocfilehash: 08390472c6480200751f37028fef594fb055f118
ms.contentlocale: de-de
ms.lasthandoff: 04/12/2017


---
# <a name="using-search-in-application-insights"></a>Verwenden von Search in Application Insights
Search ist eine Funktion in [Application Insights](app-insights-overview.md), mit der Sie einzelne Telemetrieelemente wie Seitenaufrufe, Ausnahmen und Webanforderungen suchen und untersuchen können. Außerdem können Sie Protokollablaufverfolgungen und Ereignisse anzeigen, die Sie programmiert haben.

(Verwenden Sie [Analytics](app-insights-analytics-tour.md) zum Anwenden komplexerer Abfragen auf Ihre Daten.)

## <a name="where-do-you-see-search"></a>Wo steht Search zur Verfügung?
### <a name="in-the-azure-portal"></a>Im Azure-Portal
Sie können die Diagnosesuche explizit auf dem Blatt „Application Insights-Übersicht“ Ihrer Anwendung öffnen:

![Diagnosesuche öffnen](./media/app-insights-diagnostic-search/01-open-Diagnostic.png)

Sie wird auch geöffnet, wenn Sie sich durch einige Diagramme und Rasterelemente klicken. In diesem Fall werden ihre Filter so voreingestellt, dass der Fokus auf dem ausgewählten Elementtyp liegt. 

Auf dem Blatt „Übersicht“ gibt es z.B. ein Balkendiagramm von nach Antwortzeit klassifizierten Anforderungen. Klicken Sie sich durch einen Leistungsbereich, um eine Liste einzelner Anforderungen im jeweiligen Antwortzeitbereich anzuzeigen:

![Klicken durch die Anforderungsleistung](./media/app-insights-diagnostic-search/07-open-from-filters.png)

Den Hauptteil der Diagnosesuche bildet eine Liste von Telemetrieelementen: Serveranforderungen, Seitenaufrufe, benutzerdefinierte Ereignisse, die Sie programmiert haben, usw. Am Anfang der Liste befindet sich ein Übersichtsdiagramm, das die Anzahl der Ereignisse im Zeitablauf zeigt.

Klicken Sie auf „Aktualisieren“, um neue Ereignisse abzurufen.

### <a name="in-visual-studio"></a>In Visual Studio

In Visual Studio gibt es auch das Fenster „Application Insights-Suche“. Es eignet sich am besten für die Anzeige von Telemetrieereignissen, die von der Anwendung generiert wurden, die Sie debuggen. Es kann aber auch die Ereignisse zeigen, die in Ihrer veröffentlichten App im Azure-Portal gesammelt wurden.

Öffnen Sie das Fenster „Search“ in Visual Studio:

![Geöffnetes Visual Studio-Fenster „Application Insights-Suche“](./media/app-insights-diagnostic-search/32.png)

Das Fenster „Search“ bietet ähnliche Features wie das Webportal:

![Visual Studio-Fenster „Application Insights-Suche“](./media/app-insights-diagnostic-search/34.png)

Die Registerkarte „Nachverfolgungsvorgang“ ist verfügbar, wenn Sie eine Anforderung oder einen Seitenaufruf öffnen. Ein „Vorgang“ ist eine Folge von Ereignissen, die einer einzelnen Anforderung oder einem einzelnem Seitenaufruf zugeordnet sind. Abhängigkeitsaufrufe, Ausnahmen, Ablaufverfolgungsprotokolle und benutzerdefinierte Ereignisse können beispielsweise Teil eines einzelnen Vorgangs sein. Die Registerkarte "Nachverfolgungsvorgang" zeigt den Zeitpunkt und die Dauer dieser Ereignisse in Bezug auf die Anforderung oder den Seitenaufruf. 

## <a name="inspect-individual-items"></a>Überprüfen einzelner Elemente
Wählen Sie ein beliebiges Telemetrieelement aus, um Schlüsselfelder und verwandte Elemente anzuzeigen. Wenn Sie alle Felder anzeigen möchten, klicken Sie auf "...". 

![Klicken Sie auf „Neues Arbeitselement“, bearbeiten Sie die Felder, und klicken Sie dann auf „OK“.](./media/app-insights-diagnostic-search/10-detail.png)

## <a name="filter-event-types"></a>Filtern von Ereignistypen
Öffnen Sie das Blatt "Filter", und wählen Sie die Ereignistypen, die Sie anzeigen möchten. (Wenn Sie später die Filter wiederherstellen möchten, mit denen Sie das Blatt geöffnet haben, klicken Sie auf "Zurücksetzen".)

![Wählen Sie "Filter" und dann Telemetrietypen](./media/app-insights-diagnostic-search/02-filter-req.png)

Die Ereignistypen sind:

* **Ablaufverfolgung** - [Diagnoseprotokolle](app-insights-asp-net-trace-logs.md), einschließlich TrackTrace, log4Net, NLog und Aufrufe von „System.Diagnostic.Trace“.
* **Anforderung**: Von Ihrer Serveranwendung empfangene HTTP-Anforderungen, einschließlich Seiten, Skripts, Bilder, Formatdateien und Daten. Diese Ereignisse werden verwendet, um die Übersichtsdiagramme für Anforderungen und Antworten zu erstellen.
* **Seitenaufruf** - [Vom Webclient gesendete Telemetriedaten](app-insights-javascript.md), die zum Erstellen von Seitenaufrufberichten verwendet werden. 
* **Benutzerdefiniertes Ereignis**: Wenn Sie Aufrufe von „TrackEvent()“ zum [Überwachen der Nutzung](app-insights-api-custom-events-metrics.md) eingefügt haben, können Sie diese hier suchen.
* **Ausnahme**: Nicht vom [Server abgefangene Ausnahmen](app-insights-asp-net-exceptions.md) und Ausnahmen, die Sie mit „TrackException()“ protokollieren können.
* **Abhängigkeit** - [Aufrufe aus Ihrer Serveranwendung](app-insights-asp-net-dependencies.md) an andere Dienste wie REST-APIs oder Datenbanken und AJAX-Aufrufe aus Ihrem [Clientcode](app-insights-javascript.md).
* **Verfügbarkeit**: Ergebnisse von [Verfügbarkeitstests](app-insights-monitor-web-app-availability.md).

## <a name="filter-on-property-values"></a>Filtern von Eigenschaftswerten
Sie können Ereignisse anhand der Werte ihrer Eigenschaften filtern. Die verfügbaren Eigenschaften hängen von den ausgewählten Ereignistypen ab. 

Wählen Sie z. B. Anforderungen mit einem bestimmten Antwortcode aus. 

![Erweitern Sie eine Eigenschaft, und wählen Sie einen Wert](./media/app-insights-diagnostic-search/03-response500.png)

Das Auswählen keiner Werte einer bestimmten Eigenschaft hat dieselbe Wirkung wie das Auswählen aller Werte, nämlich dass das Filtern anhand dieser Eigenschaft deaktiviert wird.

### <a name="narrow-your-search"></a>Eingrenzen der Suche
Beachten Sie, dass die Zahlen rechts neben den Filterwerten anzeigen, wie viele Vorkommen es in der aktuell gefilterten Gruppe gibt. 

In diesem Beispiel ist es klar, dass die Anforderung „Rpt/Employees“ die Mehrzahl der 500-er Fehler ausmacht:

![Erweitern Sie eine Eigenschaft, und wählen Sie einen Wert](./media/app-insights-diagnostic-search/04-failingReq.png)




## <a name="find-events-with-the-same-property"></a>Suchen von Ereignissen mit der gleichen Eigenschaft
Suchen Sie alle Elemente mit dem gleichen Eigenschaftswert:

![Klicken Sie mit der rechten Maustaste auf eine Eigenschaft ](./media/app-insights-diagnostic-search/12-samevalue.png)


## <a name="search-the-data"></a>Durchsuchen der Daten

> [!NOTE]
> Um komplexere Abfragen zu stellen, öffnen Sie oben auf dem Blatt „Suche“ das Feld [**Analytics**](app-insights-analytics-tour.md).
> 

Sie können alle Eigenschaftswerte nach Ausdrücken durchsuchen. Dies ist besonders nützlich, wenn Sie [benutzerdefinierte Ereignisse](app-insights-api-custom-events-metrics.md) mit Eigenschaftswerten geschrieben haben. 

Sie können auch einen Zeitbereich festlegen, da die Suche in einem kürzeren Bereich schneller erfolgt. 

![Diagnosesuche öffnen](./media/app-insights-diagnostic-search/appinsights-311search.png)

Suchen Sie nach vollständigen Wörtern, nicht nach Teilzeichenfolgen. Verwenden Sie Anführungszeichen, um Sonderzeichen zu umschließen.

| string | wurde *nicht* gefunden von | wird aber von diesen gefunden |
| --- | --- | --- |
| HomeController.Info |home<br/>controller<br/>out | homecontroller<br/>about<br/>"homecontroller.about"|
|USA|Uni<br/>ted|united<br/>states<br/>united AND states<br/>"united states"

Dies sind einige Suchausdrücke, die Sie verwenden können:

| Beispielabfrage | Effekt |
| --- | --- |
| `apple` |Findet alle Ereignisse im Uhrzeitbereich, deren Felder den Begriff „apple“ enthalten. |
| `apple AND banana` |Findet Ereignisse, die beide Wörter enthalten. Verwenden Sie „AND“ in Großbuchstaben, nicht „and“. |
| `apple OR banana`<br/>`apple banana` |Findet Ereignisse, die eines der beiden Wörter enthalten. Verwenden Sie „OR“ in Großbuchstaben, nicht „or“.<br/>Kurzform. |
| `apple NOT banana` |Findet Ereignisse, die das eine Wort enthalten, aber nicht das andere. |



## <a name="sampling"></a>Stichproben
Wenn die Anwendung viele Telemetriedaten generiert (und Sie die ASP.NET SDK-Version 2.0.0-beta3 oder höher verwenden), reduziert das adaptive Stichprobenmodul automatisch die an das Portal gesendete Datenmenge, indem nur ein repräsentativer Bruchteil der Ereignisse gesendet wird. Ereignisse, die mit derselben Anforderung im Zusammenhang stehen, werden als Gruppe aus- oder abgewählt, sodass Sie zwischen verwandten Ereignissen navigieren können. 

[Erfahren Sie mehr über das Erstellen von Stichproben](app-insights-sampling.md).



## <a name="create-work-item"></a>Erstellen eines Arbeitselements
Sie können einen Fehler mit den Details aus einem beliebigen Telemetrieelement in GitHub oder Visual Studio Team Services erstellen. 

![Klicken Sie auf „Neues Arbeitselement“, bearbeiten Sie die Felder, und klicken Sie dann auf „OK“.](./media/app-insights-diagnostic-search/42.png)

Wenn Sie diesen Vorgang zum ersten Mal ausführen, werden Sie aufgefordert, einen Link zu Ihrem Team Services-Konto und -Projekt zu konfigurieren.

![Geben Sie die URL Ihres Team Services-Servers und den Projektnamen ein, und klicken Sie auf „Autorisieren“.](./media/app-insights-diagnostic-search/41.png)

(Sie können den Link auch auf dem Blatt „Arbeitsaufgaben“ konfigurieren.)

## <a name="save-your-search"></a>Speichern der Suche
Wenn Sie alle gewünschten Filter festgelegt haben, können Sie die Suche als Favoriten speichern. Wenn Sie mit einem Organisationskonto arbeiten, können Sie wählen, ob Sie sie für andere Teammitglieder freigeben.

![Klicken Sie auf "Favoriten", legen Sie den Namen fest, und klicken Sie auf "Speichern"](./media/app-insights-diagnostic-search/08-favorite-save.png)

Um die Suche erneut zu verwenden, **wechseln Sie zum Blatt "Übersicht"** und öffnen "Favoriten":

![Kachel "Favoriten"](./media/app-insights-diagnostic-search/09-favorite-get.png)

Wenn Sie mit dem Zeitraum "Relativ" gespeichert haben, enthält das neu geöffnete Blatt die neuesten Daten. Wenn Sie mit dem Zeitraum "Absolut" gespeichert haben, werden jedes Mal dieselben Daten gezeigt. (Sollte „Relativ“ nicht verfügbar sein, wenn Sie einen Favoriten speichern möchten, klicken Sie in der Kopfzeile auf „Zeitbereich“ und legen einen Zeitbereich fest, der kein benutzerdefinierter Bereich ist.)

## <a name="send-more-telemetry-to-application-insights"></a>Senden weiterer Telemetriedaten an Application Insights
Neben der standardmäßig vom Application Insights SDK gesendeten Telemetriedaten können Sie folgende Aktionen ausführen:

* Erfassen von Protokollablaufverfolgungen mithilfe Ihres bevorzugten Protokollierungsframeworks in [.NET](app-insights-asp-net-trace-logs.md) oder [Java](app-insights-java-trace-logs.md). Das heißt, dass Sie Ihre Protokollablaufverfolgungen durchsuchen und mit Seitenaufrufen, Ausnahmen und anderen Ereignissen korrelieren können. 
* [Schreiben von Code](app-insights-api-custom-events-metrics.md) zum Senden benutzerdefinierter Ereignisse, Seitenaufrufe und Ausnahmen. 

[Erfahren Sie, wie Sie Protokolle und benutzerdefinierte Telemetrie an Application Insights senden können](app-insights-asp-net-trace-logs.md).

## <a name="questions"></a>FRAGEN UND ANTWORTEN
### <a name="limits"></a>Wie viele Daten werden beibehalten?

Entsprechende Informationen finden Sie unter [Zusammenfassung der Grenzwerte](app-insights-pricing.md#limits-summary).

### <a name="how-can-i-see-post-data-in-my-server-requests"></a>Wie kann ich die POST-Daten in meinen Serveranforderungen anzeigen?
POST-Daten werden nicht automatisch protokolliert. Sie können jedoch [TrackTrace oder Protokollaufrufe](app-insights-asp-net-trace-logs.md) verwenden. Fügen Sie die POST-Daten in den "message"-Parameter ein. Sie können nicht anhand der Nachricht wie anhand von Eigenschaften filtern, aber dafür ist der Größengrenzwert höher.

## <a name="video"></a>Video

> [!VIDEO https://channel9.msdn.com/events/Connect/2016/112/player]

## <a name="add"></a>Nächste Schritte
* [Stellen von komplexeren Abfragen mit Analytics](app-insights-analytics-tour.md)
* [Senden von Protokollen und benutzerdefinierter Telemetrie an Application Insights](app-insights-asp-net-trace-logs.md)
* [Einrichten von Tests der Verfügbarkeit und Reaktionsfähigkeit](app-insights-monitor-web-app-availability.md)
* [Problembehandlung](app-insights-troubleshoot-faq.md)

