---
title: "Analysieren von Benutzernavigationsmustern mit Benutzerabläufen in Azure Application Insights | Microsoft-Dokumention"
description: Analysieren Sie, wie Benutzer zwischen den Seiten und Features Ihrer Web-App navigieren.
services: application-insights
documentationcenter: 
author: numberbycolors
manager: carmonm
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: multiple
ms.topic: article
ms.date: 08/02/2017
ms.author: cfreeman
ms.translationtype: HT
ms.sourcegitcommit: 1e6fb68d239ee3a66899f520a91702419461c02b
ms.openlocfilehash: d17ed3dff08f00a1d6a2108608e42b29f95fbd84
ms.contentlocale: de-de
ms.lasthandoff: 08/16/2017

---

# <a name="analyze-user-navigation-patterns-with-user-flows-in-application-insights"></a>Analysieren von Benutzernavigationsmustern mit Benutzerabläufen in Application Insights

![Application Insights-Tool für Benutzerabläufe](./media/app-insights-usage-flows/flows.png)

Das Tool für Benutzerabläufe visualisiert die Navigation von Benutzern zwischen den Seiten und Features Ihrer Website. Dadurch liefert es beispielsweise Antworten auf folgende Fragen:
* Wie verlassen Benutzer eine Seite auf Ihrer Website?
* Worauf klicken Benutzer auf einer Seite Ihrer Website?
* An welchen Stellen verlassen Benutzer Ihre Website am häufigsten?
* Gibt es Bereiche, in denen Benutzer immer wieder die gleiche Aktion wiederholen?

Das Tool für Benutzerabläufe startet bei einem Seitenaufruf oder Ereignis Ihrer Wahl. Auf der Grundlage dieses Seitenaufrufs oder benutzerdefinierten Ereignisses zeigen die Benutzerabläufe die Seitenaufrufe und benutzerdefinierten Ereignisse, die Benutzer während einer Sitzung unmittelbar im Anschluss, zwei Schritte später usw. senden. Linien mit unterschiedlicher Stärke geben Aufschluss darüber, wie oft Benutzer den einzelnen Pfaden folgen. Spezielle Sitzungsbeendigungsknoten zeigen, wie viele Benutzer nach dem vorherigen Knoten keine Seitenaufrufe oder benutzerdefinierten Ereignisse mehr gesendet haben. Dadurch wird deutlich, wo Benutzer Ihre Website wahrscheinlich verlassen haben.



> [!NOTE]
> Ihre Application Insights-Ressource muss Seitenaufrufe oder benutzerdefinierte Ereignisse enthalten, um das Tool für Benutzerabläufe verwenden zu können. [Informieren Sie sich darüber, wie Sie Ihre App so einrichten können, dass Seitenansichten automatisch mit dem Application Insights-JavaScript-SDK erfasst werden](app-insights-javascript.md).
> 
> 

## <a name="start-by-choosing-an-initial-page-view-or-custom-event"></a>Wählen Sie zunächst einen Seitenaufruf oder ein benutzerdefiniertes Ereignis als Ausgangspunkt aus.

![Auswählen eines Ausgangsereignisses für Benutzerabläufe](./media/app-insights-usage-flows/flows-initial-event.png)

Wählen Sie zur Beantwortung von Fragen mit dem Tool für Benutzerabläufe einen Seitenaufruf oder ein benutzerdefiniertes Ereignis als Ausgangspunkt für die Visualisierung aus:
1. Klicken Sie auf den Link unter dem Titel „What do users do after...?“ (Was tun Benutzer nach Folgendem:...?), oder klicken Sie auf die Schaltfläche „Bearbeiten“. 
2. Wählen Sie im Dropdownmenü „Initial event“ (Ausgangsereignis) einen Seitenaufruf oder ein benutzerdefiniertes Ereignis aus.
3. Klicken Sie auf „Create graph“ (Graph erstellen).

Die Spalte „Step 1“ (Schritt 1) der Visualisierung zeigt, welche Aktion Benutzer direkt nach dem Ausgangsereignis am häufigsten ausgeführt haben (absteigend sortiert nach Häufigkeit). Die Spalte „Step 2“ (Schritt 2) und die nachfolgenden Spalten zeigen, welche Aktionen Benutzer danach ausgeführt haben. So können Sie sich einen Überblick über die Navigation sämtlicher Benutzer auf Ihrer Website verschaffen.

Standardmäßig zieht das Tool nur willkürliche Stichproben von Seitenaufrufen und benutzerdefinierten Ereignissen auf Ihrer Website aus den letzten 24 Stunden heran. Über das Bearbeitungsmenü können Sie den Zeitbereich vergrößern und das Verhältnis zwischen Leistung und Genauigkeit für willkürliche Stichproben verändern.

Sollten einige der Seitenaufrufe und benutzerdefinierten Ereignisse für Sie nicht relevant sein, klicken Sie für die Knoten, die Sie ausblenden möchten, auf „X“. Klicken Sie nach der Wahl der auszublendenden Knoten unter der Visualisierung auf die Schaltfläche „Create graph“ (Graph erstellen). Wenn Sie alle ausgeblendeten Knoten anzeigen möchten, klicken Sie auf die Schaltfläche „Bearbeiten“, und sehen Sie sich den Abschnitt „Excluded events“ (Ausgeschlossene Ereignisse) an.

Sollten bestimmte Seitenaufrufe oder benutzerdefinierte Ereignisse nicht wie erwartet in der Visualisierung angezeigt werden, gehen Sie wie folgt vor:
* Überprüfen Sie im Bearbeitungsmenü den Abschnitt mit den ausgeschlossenen Ereignissen.
* Verwenden Sie die Detailstufensteuerung im Bearbeitungsmenü, um seltenere Ereignisse in die Visualisierung einzubeziehen.
* Wenn der erwartete Seitenaufruf oder das erwartete benutzerdefinierte Ereignis nur selten von Benutzern gesendet wird, vergrößern Sie den Zeitbereich der Visualisierung über das Bearbeitungsmenü.
* Vergewissern Sie sich, dass der erwartete Seitenaufruf oder das erwartete benutzerdefinierte Ereignis im Quellcode Ihrer Website so eingerichtet ist, dass er bzw. es vom Application Insights SDK erfasst wird. Informationen zum Erfassen benutzerdefinierter Ereignisse finden Sie [hier](app-insights-api-custom-events-metrics.md).

Mit dem Schieberegler „Anzahl der Schritte“ im Bearbeitungsmenü können Sie in der Visualisierung weitere Schritte anzeigen.

## <a name="after-visiting-a-page-or-feature-where-do-users-go-and-what-do-they-click"></a>Wohin wechseln Besucher nach dem Besuch einer Seite oder nach der Verwendung eines Features, und worauf klicken sie?

![Verwenden von Benutzerabläufen, um nachzuvollziehen, worauf Benutzer klicken](./media/app-insights-usage-flows/flows-one-step.png)

Wenn es sich bei Ihrem Ausgangsereignis um einen Seitenaufruf handelt, können Sie über die erste Spalte der Visualisierung (Schritt 1) schnell ermitteln, welche Aktion Benutzer direkt im Anschluss an den Seitenbesuch ausgeführt haben. Öffnen Sie Ihre Website in einem Fenster neben der Visualisierung von Benutzerabläufen. Vergleichen Sie Ihre Erwartungen hinsichtlich der Benutzerinteraktion mit der Liste von Ereignissen aus der Spalte „Step 1“ (Schritt 1). Es kommt immer wieder vor, dass ein Seitenelement, dem Ihr Team keine besondere Bedeutung beimisst, zu den meistgenutzten Elementen der Seite gehört. Dies kann ein guter Ausgangspunkt für die Optimierung des Websitedesigns sein.

Wenn es sich bei Ihrem Ausgangsereignis um ein benutzerdefiniertes Ereignis handelt, zeigt die erste Spalte,welche Aktion Benutzer direkt im Anschluss an diese Aktion ausgeführt haben. Überlegen Sie sich auch hier, ob das beobachtete Verhalten der Benutzer den Zielen und Erwartungen Ihres Teams entspricht. Wenn Sie als Ausgangsereignis also beispielsweise „Artikel in den Warenkorb gelegt“ ausgewählt haben, prüfen Sie in der Visualisierung, ob kurz darauf „Zur Kasse“ und „Kauf abgeschlossen“ erscheinen. Falls das Benutzerverhalten stark von Ihren Erwartungen abweicht, können Sie anhand der Visualisierung nachvollziehen, inwiefern dies auf das aktuelle Design Ihrer Website zurückzuführen ist.

## <a name="where-are-the-places-that-users-churn-most-from-your-site"></a>An welchen Stellen verlassen Benutzer Ihre Website am häufigsten?

Achten Sie auf Sitzungsbeendigungsknoten, die weit oben in einer Visualisierungsspalte erscheinen (insbesondere zu Beginn eines Ablaufs). Das bedeutet, dass viele Benutzer Ihre Website im Anschluss an die zuvor besuchten Seiten und ausgeführten Benutzeroberflächeninteraktionen wahrscheinlich verlassen haben. Dies ist manchmal zu erwarten (beispielsweise nach Abschluss eines Einkaufs auf einer E-Commerce-Seite). In der Regel deutet das Verlassen von Benutzern jedoch auf Designprobleme, Leistungsdefizite oder andere korrigierbare Probleme Ihrer Website hin.

Beachten Sie, dass Sitzungsbeendigungsknoten nur auf Telemetriedaten basieren, die durch diese Application Insights-Ressource gesammelt wurden. Wenn Application Insights für bestimmte Benutzerinteraktionen keine Telemetriedaten erhält, haben Benutzer möglicherweise trotzdem auf diese Weise mit Ihrer Website interagiert, obwohl die Sitzung laut dem Tool für Benutzerabläufe beendet wurde.

## <a name="are-there-places-where-users-repeat-the-same-action-over-and-over"></a>Gibt es Bereiche, in denen Benutzer immer wieder die gleiche Aktion wiederholen?

Halten Sie die Augen nach Seitenaufrufen oder benutzerdefinierten Ereignissen offen, die von vielen Benutzern über mehrere Visualisierungsschritte hinweg wiederholt werden. Das bedeutet in der Regel, dass Benutzer auf Ihrer Website repetitive Aktionen ausführen. Überlegen Sie sich in diesem Fall, wie Sie das Design der Website ändern oder neue Funktionen hinzufügen können, um Wiederholungen zu reduzieren. Ein Beispiel wäre etwa das Hinzufügen einer Massenbearbeitungsfunktion, wenn Sie feststellen, dass Benutzer repetitive Aktionen für jede Zeile eines Tabellenelements ausführen.

## <a name="common-questions"></a>Häufig gestellte Fragen

### <a name="why-do-steps-appear-disconnected"></a>Warum werden Schritte ohne Verbindung angezeigt?

![Benutzerabläufe mit Schritten ohne Verbindung](./media/app-insights-usage-flows/flows-disconnected.png)

Wenn in einer Benutzerablaufvisualisierung Schritte (Spalten) ohne Verbindung angezeigt werden, war keiner der Pfade, die Benutzer zwischen den einzelnen Schritten verwendet haben, häufig genug, um angezeigt zu werden. Wenn in der Visualisierung seltenere Ereignisse angezeigt werden sollen, damit die Schritte mit einer Verbindung versehen werden, passen Sie im Bearbeitungsmenü den Schieberegler für die Detailstufe an.

### <a name="does-the-initial-event-represent-the-first-time-the-event-appears-in-a-session-or-any-time-it-appears-in-a-session"></a>Stellt das Ausgangsereignis das erste Vorkommen des Ereignisses in einer Sitzung oder ein beliebiges Vorkommen in einer Sitzung dar?

Das Ausgangsereignis in der Visualisierung stellt nur die erste Instanz eines Seitenaufrufs oder benutzerdefinierten Ereignisses dar, den bzw. das ein Benutzer während einer Sitzung gesendet hat. Wenn Benutzer das Ausgangsereignis mehrmals in einer Sitzung senden können, zeigt die Spalte „Step 1“ (Schritt 1) nur das Verhalten von Benutzern nach der *ersten* Instanz des Ausgangsereignisses (nicht alle Instanzen).

## <a name="next-steps"></a>Nächste Schritte

* [Nutzungsübersicht](app-insights-usage-overview.md)
* [Benutzer-, Sitzungs- und Ereignisanalyse in Azure Application Insights](app-insights-usage-segmentation.md)
* [Bindung](app-insights-usage-retention.md)
* [Application Insights-API für benutzerdefinierte Ereignisse und Metriken](app-insights-api-custom-events-metrics.md)
