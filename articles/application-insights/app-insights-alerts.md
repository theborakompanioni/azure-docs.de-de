---
title: Einrichten von Warnungen in Azure Application Insights | Microsoft Docs
description: "Legen Sie fest, dass Sie über längere Reaktionszeiten, Ausnahmen und andere Leistungs- oder Nutzungsänderungen in Ihrer Web-App informiert werden."
services: application-insights
documentationcenter: 
author: alancameronwills
manager: carmonm
ms.assetid: f8ebde72-f819-4ba5-afa2-31dbd49509a5
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 03/14/2017
ms.author: cfreeman
ms.translationtype: Human Translation
ms.sourcegitcommit: 0d8472cb3b0d891d2b184621d62830d1ccd5e2e7
ms.openlocfilehash: 04965375fc94fc1aa8b1c48deb743bb1d0cf1c26
ms.contentlocale: de-de
ms.lasthandoff: 03/21/2017


---
# <a name="set-alerts-in-application-insights"></a>Einrichten von Warnungen in Application Insights
[Azure Application Insights][start] kann Sie bei Änderungen der Leistung oder von Nutzungsmetriken in Ihrer Web-App benachrichtigen. 

Application Insights überwacht die Live-App auf einer [Vielzahl von Plattformen][platforms], um Sie bei der Diagnose von Leistungsproblemen und beim Auswerten von Nutzungsmustern zu unterstützen.

Es gibt drei Arten von Warnungen:

* **Metrikwarnungen** informieren Sie darüber, wenn eine Metrik für einen bestimmten Zeitraum einen Schwellenwert überschreitet – z.B. in Bezug auf Reaktionszeiten, Anzahl von Ausnahmen, CPU-Nutzung oder Seitenaufrufe. 
* [**Webtests**][availability] informieren Sie, wenn Ihre Website im Internet nicht verfügbar ist oder langsam reagiert. [Weitere Informationen][availability].
* Die [**Proaktive Diagnose**](app-insights-proactive-diagnostics.md) wird automatisch konfiguriert, damit Benachrichtigungen über ungewöhnliche Leistungsmuster gesendet werden.

In diesem Artikel werden Metrikwarnungen beschrieben.

## <a name="set-a-metric-alert"></a>Festlegen einer Metrikwarnung
Öffnen Sie das Blatt „Warnungsregeln“, und verwenden Sie dann die Schaltfläche „Hinzufügen“. 

![Klicken Sie auf dem Blatt "Warnungsregeln" auf "Warnung hinzufügen". Legen Sie Ihre App als zu messende Ressource fest, geben Sie einen Namen für die Warnung ein, und wählen Sie eine Metrik.](./media/app-insights-alerts/01-set-metric.png)

* Legen Sie die Ressource vor den anderen Eigenschaften fest. **Wählen Sie die Ressource "(Komponenten)" aus** , wenn Sie Benachrichtigungen für Leistungs- oder Nutzungsmetriken festlegen möchten.
* Der Name, den Sie der Warnung zuweisen, muss innerhalb der Ressourcengruppe (nicht nur in Ihrer Anwendung) eindeutig sein.
* Achten Sie auf die Einheiten, die beim Eingeben des Schwellenwerts gefordert sind.
* Wenn Sie das Feld „E-Mail an Besitzer...“ aktivieren, werden Warnungen per E-Mail an alle Benutzer gesendet, die Zugriff auf diese Ressourcengruppe haben. Wenn Sie diese Personengruppe erweitern möchten, fügen Sie die entsprechenden Benutzer der [Ressourcengruppe oder dem Abonnement](app-insights-resources-roles-access-control.md) (nicht der Ressource) hinzu.
* Wenn Sie „Weitere E-Mail-Adressen“ angeben, werden Warnungen an diese Einzelpersonen oder Gruppen gesendet (unabhängig davon, ob Sie das Kontrollkästchen „E-Mail an Besitzer“ aktiviert haben). 
* Legen Sie eine [Webhookadresse](../monitoring-and-diagnostics/insights-webhooks-alerts.md) fest, wenn Sie eine Web-App eingerichtet haben, die auf Warnungen reagiert. Der Aufruf erfolgt, wenn die Warnung aktiviert (ausgelöst) und wenn sie gelöst wird. (Beachten Sie aber, dass Abfrageparameter derzeit nicht als Webhook-Eigenschaften übergeben werden.)
* Sie können die Warnung deaktivieren oder aktivieren: Die zugehörigen Schaltflächen finden Sie oben auf dem Blatt.

*Ich sehe keine Schaltfläche zum Hinzufügen von Benachrichtigungen.* 

* Verwenden Sie ein Organisationskonto? Sie können Warnungen festlegen, wenn Sie für diese Anwendungsressource über Zugriffsberechtigungen für Besitzer oder Mitwirkende verfügen. Sehen Sie sich das Blatt „Access Control“ an. [Erfahren Sie mehr über Access Control][roles].

> [!NOTE]
> Auf dem Blatt „Warnungen“ sehen Sie, dass bereits eine Warnung festgelegt ist: [Proaktive Diagnose](app-insights-proactive-failure-diagnostics.md). Dies ist eine automatische Benachrichtigung, die eine bestimmte Metrik überwacht, nämlich die Anforderungsfehlerrate. Sofern Sie nicht entscheiden, diese proaktive Warnung zu deaktivieren, müssen Sie also keine eigene Warnung für die Anforderungsfehlerrate festlegen. 
> 
> 

## <a name="see-your-alerts"></a>Anzeigen Ihrer Warnungen
Sie erhalten eine E-Mail, wenn sich ein Warnungsstatus von "Inaktiv" in "Aktiv" ändert und umgekehrt. 

Der aktuelle Status jeder Warnung wird im Fenster "Warnungsregeln" angezeigt.

Die Dropdownliste "Warnungen" enthält einen Überblick über die letzten Aktivitäten:

![Dropdownliste „Warnungen“](./media/app-insights-alerts/010-alert-drop.png)

Der Verlauf von Statusänderungen befindet sich im Aktivitätsprotokoll:

![Klicken Sie auf dem Blatt „Übersicht“ auf „Einstellungen“, „Überwachungsprotokolle“.](./media/app-insights-alerts/09-alerts.png)

## <a name="how-alerts-work"></a>Funktionsweise von Warnungen
* Eine Warnung kann drei Zustände annehmen: „Nie aktiviert“, „Aktiviert“ und „Gelöst“. „Aktiviert“ bedeutet, dass die angegebene Bedingung bei der letzten Auswertung erfüllt wurde.
* Wenn sich der Status einer Warnung ändert, wird eine Benachrichtigung generiert. (Wenn die Bedingung für die Warnung bereits erfüllt war, als Sie die Warnung erstellt haben, erhalten Sie möglicherweise erst dann eine Benachrichtigung, wenn die Bedingung nicht mehr erfüllt wird.)
* Jede Benachrichtigung generiert eine E-Mail-Nachricht, wenn Sie das E-Mail-Feld aktiviert oder E-Mail-Adressen angegeben haben. Sie können auch die Dropdownliste „Benachrichtigungen“ überprüfen.
* Eine Warnung wird jedes Mal ausgewertet, wenn eine Metrik empfangen wird, sonst aber aus keinem anderen Grund.
* Durch die Auswertung wird die Metrik über den vorangegangenen Zeitraum aggregiert und mit dem Schwellenwert verglichen, um den neuen Zustand zu ermitteln.
* Der von Ihnen ausgewählte Zeitraum gibt das Intervall an, über das Metriken aggregiert werden. Der Zeitraum hat keinen Einfluss darauf, wie häufig die Warnung ausgewertet wird: Dies hängt davon ab, wie häufig Metriken empfangen werden.
* Wenn eine Zeit lang kein Daten für eine bestimmte Metrik empfangen wurden, wirkt sich diese Lücke unterschiedlich auf die Auswertung von Warnungen und die Diagramme im Metrik-Explorer aus. Wenn länger als durch das Samplingintervall des Diagramms vorgegeben keine Daten empfangen werden, hat das Diagramm im Metrik-Explorer den Wert 0. Eine auf derselben Metrik basierende Warnung wird aber nicht erneut ausgewertet, und der Zustand der Warnung bleibt unverändert. 
  
    Sobald dann Daten empfangen werden, springt das Diagramm auf einen Wert ungleich 0 (null) zurück. Die Warnung wird auf Grundlage der Daten ausgewertet, die für den angegebenen Zeitraum zur Verfügung stehen. Wenn der neue Datenpunkt der einzige in diesem Zeitraum verfügbare Datenpunkt ist, basiert das Aggregat auf diesem Datenpunkt.
* Eine Warnung kann häufig zwischen den Zuständen "Warnung" und "Fehlerfrei" hin und her wechseln, auch wenn Sie einen langen Zeitraum festlegen. Dies kann vorkommen, wenn sich der metrische Wert um den Schwellenwert herum bewegt. Der Schwellenwert weist keine Hysterese auf: Der Übergang zur Warnung erfolgt beim selben Wert wie der Übergang zum fehlerfreien Zustand.

## <a name="what-are-good-alerts-to-set"></a>Welche Warnungen sollten festgelegt werden?
Das hängt von Ihrer Anwendung ab. Zunächst einmal sollten Sie nicht zu viele Metriken festlegen. Beobachten Sie Ihre Metrikdiagramme eine Zeitlang, während die App ausgeführt wird, um ein Gefühl für das Normalverhalten zu erhalten. So finden Sie leichter Möglichkeiten zum Verbessern der Leistung. Richten Sie dann Warnungen ein, die Ihnen mitteilen, wenn die Metriken den Normalbereich verlassen. 

Zu den gängigen Warnungen zählen Folgende:

* [Browsermetriken][client], insbesondere **Browser-Seitenladezeiten**, eignen sich für Webanwendungen. Wenn Ihre Seite viele Skripts enthält, sollten Sie auf **Browserausnahmen**achten. Um diese Metriken und Warnungen zu erhalten, müssen Sie die [Webseitenüberwachung][client] einrichten.
* **Serverantwortzeit** für die Serverseite von Webanwendungen. Achten Sie neben der Einrichtung von Warnungen auf diese Metrik, um festzustellen, ob sie bei hohen Anforderungsraten unverhältnismäßig stark variiert: Dies kann darauf hindeuten, dass für Ihre App nicht genügend Systemressourcen vorhanden sind. 
* **Serverausnahmen** erfordern ein [zusätzliches Setup](app-insights-asp-net-exceptions.md), damit sie angezeigt werden.

Vergessen Sie nicht, dass bei der [proaktiven Fehlerquotendiagnose](app-insights-proactive-failure-diagnostics.md) automatisch die Rate überwacht wird, mit der Ihre App auf Anforderungen mit Fehlercodes reagiert. 

## <a name="automation"></a>Automation
* [Verwenden von PowerShell zum Automatisieren der Einrichtung von Warnungen](app-insights-powershell-alerts.md)
* [Verwenden von Webhooks zum Automatisieren der Reaktion auf Warnungen](../monitoring-and-diagnostics/insights-webhooks-alerts.md)

## <a name="video"></a>Video

> [!VIDEO https://channel9.msdn.com/events/Connect/2016/112/player]

## <a name="see-also"></a>Weitere Informationen
* [Verfügbarkeitswebtests](app-insights-monitor-web-app-availability.md)
* [Automatisieren der Einrichtung von Warnungen](app-insights-powershell-alerts.md)
* [Proaktive Diagnose](app-insights-proactive-diagnostics.md) 

<!--Link references-->

[availability]: app-insights-monitor-web-app-availability.md
[client]: app-insights-javascript.md
[platforms]: app-insights-platforms.md
[roles]: app-insights-resources-roles-access-control.md
[start]: app-insights-overview.md


