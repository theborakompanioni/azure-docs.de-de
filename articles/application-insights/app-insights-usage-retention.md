---
title: "Benutzerbindungsanalyse für Webanwendungen mit Azure Application Insights | Microsoft-Dokumentation"
description: "Wie viele Benutzer kehren zu Ihrer App zurück?"
services: application-insights
documentationcenter: 
author: botatoes
manager: carmonm
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: multiple
ms.topic: article
ms.date: 05/03/2017
ms.author: cfreeman
ms.translationtype: Human Translation
ms.sourcegitcommit: 71fea4a41b2e3a60f2f610609a14372e678b7ec4
ms.openlocfilehash: 17f6062537714fbef7c8509261ac4875f8a44b6e
ms.contentlocale: de-de
ms.lasthandoff: 05/10/2017

---

# <a name="user-retention-analysis-for-web-applications-with-application-insights"></a>Benutzerbindungsanalyse für Webanwendungen mit Azure Application Insights

Das Blatt „Vermerkdauer“ in [Azure Application Insights](app-insights-overview.md) hilft Ihnen beim Analysieren, wie viele Benutzer zu Ihrer App zurückkehren und wie oft sie bestimmte Aufgaben durchführen oder Ziele erreichen. Beispiel: Wenn Sie eine Spielewebsite betreiben, können Sie die Anzahl der Benutzer, die nach einem verlorenen Spiel zurückkehren, mit der Anzahl vergleichen, die nach einem gewonnenen Spiel zurückkehren. Mit diesem Wissen können Sie sowohl die Benutzererfahrung als auch Ihre Geschäftsstrategie verbessern.

## <a name="get-started"></a>Erste Schritte

Wenn Sie im Application Insights-Portal auf dem Blatt „Vermerkdauer“ noch keine Daten sehen, [erfahren Sie mehr zum Einstieg in die Nutzungstools](app-insights-usage-overview.md).

## <a name="the-retention-tool"></a>Das Vermerkdauer-Tool

![Vermerkdauer-Tool](./media/app-insights-usage-retention/retention.png)

a. Die Seite „Übersicht“ ist immer vorhanden. Außerdem können Sie benannte Berichte mit verschiedenen Parametereinstellungen speichern. Speichern Sie Berichte in der Freigabe, um sie für andere Benutzer sichtbar zu machen, die Zugriff auf die Ressource haben.

b. Das Diagramm zählt standardmäßig alle Benutzer, die benutzerdefinierte Ereignisse oder Seitenansichten ausgeführt haben, die von Ihrer App empfangen wurden. Wählen Sie einen oder mehrere aus, um sich auf eine bestimmte Benutzeraktivität zu konzentrieren. 

c. Wenden Sie einen oder mehrere Filter auf Eigenschaften an. Beispielsweise können Sie sich auf Benutzer in einem bestimmten Land oder einer bestimmten Region konzentrieren. Klicken Sie nach dem Festlegen der Filter auf **Aktualisieren**.

d. Mit **Standardeinstellungen wiederherstellen** werden der benutzerdefinierte Filter und die Ereignisfilter immer gelöscht.

e. Das Zusammenfassungsdiagramm zeigt Gesamtwerte für den ausgewählten Zeitraum an.

f. Das Raster zeigt die Anzahl der Benutzer, die zurückgekehrt sind, um die ausgewählten Aktionen innerhalb eines bestimmten Zeitraums zu wiederholen. Jede Zeile stellt eine Kohorte von Benutzern dar, die eine der ausgewählten Aktionen während des angezeigten Zeitraums ausgeführt haben. Jede Zelle in der Zeile zeigt an, wie viele Benutzer dieser Kohorte mindestens einmal in einem späteren Zeitraum zurückgekehrt sind. Einige Benutzer kehren möglicherweise in mehreren Zeiträumen zurück.

Jeder Benutzer, der die App während des Zeitbereichs des Diagramms verwendet hat, wird genau in einer Zeile des Diagramms dargestellt. Jeder Benutzer wird in dem Zeitraum gezählt, in dem er die ausgewählte Aktion erstmals innerhalb des Zeitbereichs des Diagramms ausgeführt hat. Daher enthält die erste Zeile tendenziell eine größere Anzahl.


## <a name="use-business-events-to-track-retention"></a>Nachverfolgen der Vermerkdauer mithilfe von Geschäftsereignissen

Um die nützlichste Vermerkdaueranalyse zu erhalten, messen Sie Ereignisse, die wichtige Geschäftsaktivitäten darstellen. 

Beispielsweise können viele Benutzer eine Seite in Ihrer App öffnen, ohne das darauf angezeigte Spiel zu spielen. Die Nachverfolgung der Seitenansichten an sich würde daher eine ungenaue Schätzung der Anzahl der Personen ergeben, die zurückkehren, um das Spiel erneut zu spielen, weil es ihnen gefallen hat. Um ein klares Bild der zurückkehrenden Spieler zu erhalten, sollte Ihre App ein benutzerdefiniertes Ereignis senden, wenn ein Benutzer tatsächlich spielt.  

Es empfiehlt sich, benutzerdefinierte Ereignisse zu codieren, die wichtige geschäftliche Aktionen darstellen, und diese für die Vermerkdaueranalyse zu verwenden. Zum Erfassen des Spielergebnisses müssen Sie eine Codezeile zum Senden eines benutzerdefinierten Ereignisses an Application Insights schreiben. Wenn Sie diese im Webseitencode oder in Node.JS schreiben, sieht sie wie folgt aus:

```JavaScript
    appinsights.trackEvent("won game");
```

Oder im ASP.NET-Servercode:

```C#
   telemetry.TrackEvent("won game");
```

[Erfahren Sie mehr über das Schreiben von benutzerdefinierten Ereignissen](app-insights-api-custom-events-metrics.md#trackevent).


## <a name="next-steps"></a>Nächste Schritte

* [Nutzungsübersicht](app-insights-usage-overview.md)
* [Benutzer und Sitzungen](app-insights-usage-segmentation.md)
* [Codierung benutzerdefinierte Ereignisse](app-insights-api-custom-events-metrics.md)


