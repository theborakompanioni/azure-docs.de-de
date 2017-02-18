---
title: "Smart Detection – ungewöhnliche fehlgeschlagene Anforderungen in Application Insights | Microsoft Docs"
description: "Macht Sie auf ungewöhnliche Änderungen bei der Rate fehlgeschlagener Anforderungen an Ihre Web-App aufmerksam und bietet eine Analyse der Diagnose. Es ist keine Konfiguration erforderlich."
services: application-insights
documentationcenter: 
author: yorac
manager: carmonm
ms.assetid: ea2a28ed-4cd9-4006-bd5a-d4c76f4ec20b
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 01/09/2017
ms.author: awills
translationtype: Human Translation
ms.sourcegitcommit: 9a3df0ad2483471023ebb954d613bc5cad8fb7bf
ms.openlocfilehash: 24ca63e69d181f0d2c236b1fb6761984ce89520b


---
# <a name="smart-detection---failure-anomalies"></a>Smart Detection – ungewöhnliche fehlgeschlagene Anforderungen
[Application Insights](app-insights-overview.md) benachrichtigt Sie automatisch und nahezu in Echtzeit, wenn es bei Ihrer Web-App zu einer ungewöhnlichen Zunahme der fehlgeschlagenen Anforderungen kommt. Die Lösung erkennt eine ungewöhnliche Zunahme der Rate fehlerhafter HTTP-Anforderungen oder Abhängigkeitsaufrufen. Bei fehlgeschlagenen Anforderungen handelt es sich in der Regel um Anforderungen mit Antwortcodes von 400 oder höher. Um Sie bei der Selektierung und Diagnose des Problems zu unterstützen, wird in der Benachrichtigung eine Analyse der Merkmale der Fehler und der verknüpften Telemetriedaten angegeben. Außerdem werden Links zum Application Insights-Portal zur weiteren Diagnose bereitgestellt. Diese Funktion muss nicht eingerichtet oder konfiguriert werden, da sie Machine Learning-Algorithmen verwendet, um die normale Fehlerrate zu bestimmen.

Dieses Feature funktioniert mit Java- und ASP.NET-Web-Apps, die in der Cloud oder auf Ihren eigenen Servern gehostet werden. Es funktioniert auch mit allen Apps, die Daten zur Anforderungs- oder Abhängigkeitstelemetrie generieren, z.B. mit einer Workerrolle, mit der [TrackRequest()](app-insights-api-custom-events-metrics.md#trackrequest) oder [TrackDependency()](app-insights-api-custom-events-metrics.md#trackdependency) aufgerufen wird.

Nachdem Sie [Application Insights für Ihr Projekt](app-insights-overview.md)eingerichtet haben und Ihre App eine bestimmte Mindestmenge von Telemetriedaten generiert hat, benötigt Smart Detection 24 Stunden, um das normale Verhalten Ihrer App zu ermitteln, bevor die Erkennung aktiviert wird und Warnungen senden kann.

Hier sehen Sie eine Beispielwarnung:

![Beispiel für eine intelligente Erkennung mit Clusteranalyse im Umfeld des Fehlers](./media/app-insights-proactive-failure-diagnostics/013.png)

> [!NOTE]
> Standardmäßig erhalten Sie eine kürzere E-Mail als in diesem Beispiel. Sie können jedoch [zu diesem Detailformat wechseln](#configure-alerts).
>
>

Sie enthält folgende Angaben:

* Die Fehlerrate im Vergleich zum normalen App-Verhalten
* Anzahl der betroffenen Benutzer – so wissen Sie, wie ernst die Lage ist.
* Ein charakteristisches Muster für die Fehler. In diesem Beispiel gibt es einen bestimmten Antwortcode, einen Anforderungsnamen (Vorgang) und eine App-Version. So wissen Sie sofort, wo im Code Sie suchen müssen. Weitere Möglichkeiten sind beispielsweise ein bestimmter Browser oder Clientbetriebssystem.
* Die Ausnahme, Protokollablaufverfolgungen und Abhängigkeitsfehler (Datenbanken oder andere externe Komponenten), die den charakterisierten Fehlern zugeordnet zu sein scheinen.
* Direkte Links zu relevanten Suchvorgängen in den Telemetriedaten in Application Insights.

## <a name="benefits-of-smart-detection"></a>Vorteile von Smart Detection
Normale [Metrikwarnungen](app-insights-alerts.md) informieren Sie, dass möglicherweise ein Problem vorliegt. Mit Smart Detection wird dagegen die Diagnose für Sie gestartet und ein großer Teil der Analyse durchgeführt, die Sie ansonsten selbst durchzuführen hätten. Sie erhalten die Ergebnisse fein säuberlich verpackt und können so schnell zur Ursache des Problems vordringen.

## <a name="how-it-works"></a>So funktioniert's
Bei Smart Detection werden die von Ihrer App erhaltenen Telemetriedaten und insbesondere die Rate von Fehlern überwacht. Diese Regel ermittelt die Anzahl von Anforderungen, bei denen die `Successful request`-Eigenschaft auf „false“ festgelegt ist, sowie die Anzahl von Abhängigkeitsaufrufen, bei denen die `Successful call`-Eigenschaft auf „false“ festgelegt ist. Für Anforderungen gilt standardmäßig `Successful request == (resultCode < 400)` (es sei denn, Sie haben benutzerdefinierten Code geschrieben, um Ihre eigenen [TrackRequest](app-insights-api-custom-events-metrics.md#trackrequest)-Aufrufe zu [filtern](app-insights-api-filtering-sampling.md#filtering) oder zu erstellen). 

Die Leistung Ihrer App weist ein typisches Verhaltensmuster auf. Einige Anforderungen oder Abhängigkeitsaufrufe sind anfälliger für Fehler als andere, und die gesamte Fehlerrate kann mit zunehmender Auslastung steigen. Bei Smart Detection werden diese Anomalien mithilfe von maschinellem Lernen ermittelt.

Wenn Telemetriedaten aus Ihrer Web-App in Application Insights eingehen, vergleicht Smart Detection das aktuelle Verhalten mit den Mustern der letzten Tage. Wenn ein ungewöhnlicher Anstieg der Fehlerrate im Vergleich zur vorherigen Leistung beobachtet wird, wird eine Analyse ausgelöst.

Wenn eine Analyse ausgelöst wird, führt der Dienst eine Clusteranalyse für die fehlgeschlagene Anforderung aus, um in den Werten ein Muster zu ermitteln, durch die sich die Fehler charakterisieren lassen. Im obigen Beispiel hat die Analyse ermittelt, dass die meisten Fehler zu einem bestimmten Ergebniscode, Anforderungsnamen, Server-URL-Host und einer Rolleninstanz gehören. Im Gegensatz dazu hat die Analyse ermittelt, dass die Clientbetriebssystem-Eigenschaft über mehrere Werte verteilt wird und daher nicht aufgeführt wird.

Wenn Ihr Dienst mit diesen Telemetrieaufrufen instruiert wurde, ermittelt der Analyzer eine Ausnahme und einen Abhängigkeitsfehler, die Anforderungen im identifizierten Cluster zuzuordnen sind, sowie ein Beispiel für Ablaufverfolgungsprotokolle, die diesen Anforderungen zugeordnet sind.

Das Analyseergebnis wird Ihnen als Warnung gesendet, sofern Sie nichts anderes konfiguriert haben.

Wie bei [manuell festgelegten Warnungen](app-insights-alerts.md)können Sie den Status der Warnung prüfen und die Warnung auf dem Blatt „Warnungen“ Ihrer Application Insights-Ressource konfigurieren. Im Gegensatz zu anderen Warnungen müssen Sie Smart Detection jedoch nicht einrichten oder konfigurieren. Wenn Sie möchten, können Sie sie deaktivieren oder die Ziel-E-Mail-Adressen ändern.

## <a name="configure-alerts"></a>Konfigurieren von Warnungen
Sie können Smart Detection deaktivieren, die E-Mail-Empfänger ändern, einen Webhook erstellen oder ausführlichere Warnmeldungen festlegen.

Öffnen Sie die Seite „Warnungen“. Failure Anomalies wird zusammen mit allen Warnungen aufgeführt, die Sie eventuell manuell festgelegt haben, und Sie können sehen, ob sie sich derzeit im Status „Warnung“ befindet.

![Klicken Sie auf der Seite „Übersicht“ auf die Kachel „Warnungen“. Klicken Sie alternativ dazu auf einer beliebigen Seite „Metriken“ auf die Schaltfläche „Warnungen“.](./media/app-insights-proactive-failure-diagnostics/021.png)

Klicken Sie auf die Warnung, um sie zu konfigurieren.

![Konfiguration](./media/app-insights-proactive-failure-diagnostics/032.png)

Beachten Sie, dass Sie Smart Detection deaktivieren, aber nicht löschen (oder neu erstellen) können.

#### <a name="detailed-alerts"></a>Detaillierte Warnungen
Bei Auswahl von „Detaillierte Diagnose erhalten“ enthält die E-Mail weitere Diagnoseinformationen. Manchmal werden Sie das Problem allein anhand der Daten in der E-Mail diagnostizieren können.

Es gibt ein geringes Risiko, dass die ausführlichere Warnung vertrauliche Informationen enthält, weil sie Ausnahme- und Ablaufverfolgungsmeldungen umfasst. Dies geschieht jedoch nur, wenn Ihr Code vertrauliche Informationen in diesen Nachrichten zulassen könnte.

## <a name="triaging-and-diagnosing-an-alert"></a>Selektierung und Diagnose einer Warnung
Eine Warnung gibt an, dass ein ungewöhnlicher Anstieg bei der Rate der Anforderungsfehler erkannt wurde. Wahrscheinlich liegt ein Problem mit Ihrer App oder deren Umgebung vor.

Anhand des Prozentsatzes der Anforderungen und der Anzahl der betroffenen Benutzer können Sie entscheiden, wie dringend das Problem ist. Im Beispiel oben zeigt die Fehlerrate von 22,5 %, die einer normalen Fehlerrate von 1 % gegenübersteht, dass ganz offensichtlich Probleme vorliegen. Auf der anderen Seite sind nur 11 Benutzer betroffen. Wenn es sich dabei um Ihre App handeln würde, könnten Sie beurteilen, wie schwerwiegend die Fehler sind.

In vielen Fällen können Sie das Problem über den Anforderungsnamen, die Ausnahme, den Abhängigkeitsfehler und die Ablaufverfolgungsdaten schnell diagnostizieren.

Es gibt einige andere Anhaltspunkte. Beispielsweise entspricht die Abhängigkeitsfehlerrate in diesem Beispiel der Ausnahmerate (89,3 %). Dies legt nahe, dass die Ausnahme sich direkt aus dem Abhängigkeitsfehler ergibt, sodass Sie genau wissen, wo im Code Sie suchen müssen.

Über die Links in den einzelnen Abschnitten gelangen Sie direkt zu einer [Suchseite](app-insights-diagnostic-search.md), die nach den entsprechenden Anforderungen, Ausnahmen, Abhängigkeiten oder Ablaufverfolgungen gefiltert ist. Dort können Sie weitere Nachforschungen anstellen. Alternativ können Sie das [Azure-Portal](https://portal.azure.com) öffnen, zur Application Insights-Ressource für Ihre App wechseln und das Blatt „Fehler“ öffnen.

In diesem Beispiel wird durch Klicken den Link zum Anzeigen der Details von Abhängigkeitsfehlern das Application Insights-Blatt „Suchen“ geöffnet. Es zeigt die SQL-Anweisung mit einem Beispiel für die zugrunde liegende Ursache: In Pflichtfeldern wurden NULL-Werte angegeben, und die Überprüfung während des Speichervorgangs ist negativ ausgefallen.

![Diagnosesuche](./media/app-insights-proactive-failure-diagnostics/051.png)

## <a name="review-recent-alerts"></a>Überprüfen aktueller Warnungen

Klicken Sie auf **Smart Detection**, um zur letzten Warnung zu wechseln:

![Zusammenfassung von Warnungen](./media/app-insights-proactive-failure-diagnostics/070.png)


## <a name="whats-the-difference-"></a>Wo liegt der Unterschied?
Smart Detection für ungewöhnliche fehlgeschlagene Anforderungen ergänzt andere ähnliche, aber doch verschiedene Features von Application Insights.

* [Metrikwarnungen](app-insights-alerts.md) werden von Ihnen festgelegt und können eine Vielzahl von Metriken überwachen, z.B. die CPU-Belegung, Anforderungsraten, Seitenladezeiten usw. Sie können sie z. B. einsetzen, um rechtzeitig benachrichtigt zu werden, wenn weitere Ressourcen hinzugefügt werden müssen. Im Gegensatz dazu deckt Smart Detection einen kleinen Bereich kritischer Metriken ab (zurzeit nur die Rate von Anforderungsfehlern), durch die Sie nahezu in Echtzeit benachrichtigt werden, sobald die Rate der Anforderungsfehler für Ihre Web-App im Vergleich zum normalen Verhalten der Web-App drastisch ansteigt.

    Bei Smart Detection wird der Schwellenwert automatisch an die aktuellen Bedingungen angepasst.

    Smart Detection beginnt mit der Erledigung Ihrer Diagnosearbeit
* [Smart Detection für Leistungsprobleme](app-insights-proactive-performance-diagnostics.md) verwendet zudem intelligente Funktionen, um ungewöhnliche Muster in Ihren Metriken zu ermitteln, und muss nicht von Ihnen konfiguriert werden. Im Gegensatz zu Smart Detection für ungewöhnliche fehlgeschlagene Anforderungen besteht der Zweck von Smart Detection für Leistungsprobleme jedoch in der Ermittlung von Nutzungssegmenten, die beispielsweise durch bestimmte Seiten in einem bestimmten Browsertyp nicht zufriedenstellend verarbeitet werden. Die Analyse wird täglich ausgeführt, und falls ein Ergebnis gefunden wird, ist dies wahrscheinlich wesentlich weniger dringend als eine Warnung. Im Gegensatz dazu wird die Analyse fehlgeschlagener Anforderungen kontinuierlich für eingehende Telemetriedaten ausgeführt, und Sie werden innerhalb weniger Minuten benachrichtigt, wenn die Serverfehlerraten höher ausfallen als erwartet.

## <a name="if-you-receive-a-smart-detection-alert"></a>Wenn Sie eine Smart Detection-Warnung erhalten
*Warum habe ich diese Warnung erhalten?*

* Im Vergleich zu den normalen Grundwerten des vorhergehenden Zeitraums wurde ein ungewöhnlicher Anstieg der Anforderungsfehlerrate festgestellt. Nach der Analyse der Fehler und zugeordneten Telemetriedaten ist davon auszugehen, dass ein Problem vorliegt, das Sie untersuchen sollten.

*Bedeutet die Benachrichtigung, dass ich definitiv ein Problem habe?*

* Wir versuchen, bei Störungen oder Beeinträchtigungen der App eine Warnung zu senden, aber nur Sie können die Semantik oder die Auswirkungen auf die App oder Benutzer vollständig einschätzen.

*Meine Daten werden also angesehen?*

* Nein. Der Dienst ist vollständig automatisch. Nur Sie erhalten die Benachrichtigungen. Ihre Daten sind [privat](app-insights-data-retention-privacy.md).

*Muss ich diese Benachrichtigung abonnieren?*

* Nein. Jeder Anwendung, die Telemetrieanforderungen sendet, verfügt über die Smart Detection-Warnungsregel.

*Kann ich das Abonnement abbestellen oder die Benachrichtigungen stattdessen an meine Kollegen senden lassen?*

* Ja, klicken Sie unter „Warnungsregeln“ auf die Regel für Smart Detection, um sie zu konfigurieren. Sie können die Warnung deaktivieren oder die Empfänger für die Warnung ändern.

*Ich habe die E-Mail verloren. Wo finde ich die Benachrichtigungen im Portal?*

* In den Aktivitätsprotokollen. Öffnen Sie in Azure die Application Insights-Ressource für Ihre App, und wählen Sie dann Aktivitätsprotokolle aus.

*Einige Warnungen betreffen bekannte Probleme. Diese Warnungen möchte ich nicht erhalten.*

* Wir arbeiten an einer Funktion zur Warnungsunterdrückung.

## <a name="next-steps"></a>Nächste Schritte
Mit den folgenden Diagnosetools können Sie die Telemetrie Ihrer App untersuchen:

* [Metrik-Explorer](app-insights-metrics-explorer.md)
* [Suchexplorer](app-insights-diagnostic-search.md)
* [Analytics: Leistungsfähige Abfragesprache](app-insights-analytics-tour.md)

Intelligente Erkennungen sind vollkommen automatisch. Vielleicht möchten Sie aber weitere Warnungen einrichten?

* [Einrichten von Warnungen in Application Insights](app-insights-alerts.md)
* [Verfügbarkeitswebtests](app-insights-monitor-web-app-availability.md)



<!--HONumber=Feb17_HO1-->


