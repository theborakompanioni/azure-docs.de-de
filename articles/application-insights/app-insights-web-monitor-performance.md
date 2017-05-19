---
title: "Überwachen der Integrität und Nutzung Ihrer Anwendung mit Application Insights"
description: "Erste Schritte mit Application Insights. Analysieren Sie die Auslastung, Verfügbarkeit und Leistung Ihres lokalen oder Microsoft Azure-Anwendungen."
services: application-insights
documentationcenter: 
author: CFreemanwa
manager: carmonm
ms.assetid: 40650472-e860-4c1b-a589-9956245df307
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 11/25/2015
ms.author: cfreeman
ms.translationtype: Human Translation
ms.sourcegitcommit: 538f282b28e5f43f43bf6ef28af20a4d8daea369
ms.openlocfilehash: cbdef43381deac957c0e48b7043273c43b032935
ms.contentlocale: de-de
ms.lasthandoff: 04/07/2017


---
# <a name="monitor-performance-in-web-applications"></a>Leistung in Webanwendungen überwachen


Stellen sie sicher, dass Ihre Anwendung optimal funktioniert, und stellen Sie Fehler umgehend fest. [Application Insights][start] informiert Sie über alle Leistungsprobleme und Ausnahmefälle. So können Sie die Ursachen schnell ermitteln und diagnostizieren.

Application Insights kann Java- und ASP.NET-Webanwendungen und -Dienste sowie WCF-Dienste überwachen. Das Hosting kann lokal, auf virtuellen Computern oder als Microsoft Azure-Websites erfolgen. 

Auf Clientseite kann Application Insights Telemetriedaten von Webseiten und eine Vielzahl von Geräten sammeln, einschließlich iOS-, Android- und Windows Store-Apps.

## <a name="setup"></a>Einrichten der Leistungsüberwachung
Falls Sie Application Insights Ihrem Projekt noch nicht hinzugefügt haben (d. h., wenn es nicht über ApplicationInsights.config verfügt), gehen Sie nach einer der folgenden Methoden vor, um zu beginnen:

* [ASP.NET-Web-Apps](app-insights-asp-net.md)
  * [Ausnahmeüberwachung hinzufügen](app-insights-asp-net-exceptions.md)
  * [Abhängigkeitsüberwachung hinzufügen](app-insights-monitor-performance-live-website-now.md)
* [J2EE-Web-Apps](app-insights-java-get-started.md)
  * [Abhängigkeitsüberwachung hinzufügen](app-insights-java-agent.md)

## <a name="view"></a>Untersuchen von Leistungsmetriken
Navigieren Sie im [Azure-Portal](https://portal.azure.com)zu der Application Insights-Ressource, die Sie für Ihre Anwendung eingerichtet haben. Das Blatt "Übersicht" zeigt grundlegende Leistungsdaten:

Klicken Sie auf ein beliebiges Diagramm, um weitere Details und Ergebnisse über einen längeren Zeitraum anzuzeigen. Klicken Sie beispielsweise auf die Kachel "Requests", und wählen Sie dann einen Zeitraum aus.

![Zu mehr Daten durchklicken und einen Zeitraum auswählen](./media/app-insights-web-monitor-performance/appinsights-48metrics.png)

Klicken Sie auf ein Diagramm, um die anzuzeigenden Metriken auszuwählen, oder fügen Sie ein neues Diagramm hinzu, und wählen Sie dann die Metriken aus:

![Auf Graph klicken, um Metrik auszuwählen](./media/app-insights-web-monitor-performance/appinsights-61perfchoices.png)

> [!NOTE]
> **Deaktivieren Sie alle Metriken**, um die insgesamt verfügbare Auswahl anzuzeigen. Die Metriken werden in Gruppen unterteilt. Wenn ein Mitglied einer Gruppe ausgewählt wird, werden nur die weiteren Mitglieder dieser Gruppe angezeigt.
> 
> 

## <a name="metrics"></a>Was bedeutet was? Leistungskacheln und Berichte
Ihnen steht eine Vielzahl von Leistungsmetriken zur Verfügung. Lassen Sie uns mit denen beginnen, die standardmäßig im Anwendungsblatt angezeigt werden.

### <a name="requests"></a>Requests
Die in einem angegebenen Zeitraum empfangenen HTTP-Anforderungen. Vergleichen Sie den Wert mit den Ergebnissen anderer Berichte, um das Verhalten Ihrer Anwendung mit wechselnder Last zu beurteilen.

HTTP-Anforderungen umfassen alle GET- oder POST-Anforderungen für Seiten, Daten und Bilder.

Klicken Sie auf das Tile, um Zählwerte für bestimmte URLs zu erhalten.

### <a name="average-response-time"></a>Average response time
Misst die Zeit zwischen dem Eingang einer Webanforderung bei Ihrer Anwendung und der zurückgegebenen Antwort.

Die Punkte zeigen einen variablen Durchschnittswert. Falls viele Anforderungen eingehen, gibt es ggf. vom Durchschnitt deutlich nach oben oder unten abweichende Spitzen im Graphen.

Achten Sie auf ungewöhnliche Spitzen. Im Allgemeinen können Sie mit einer längeren Antwortzeit bei steigender Zahl der Anforderungen rechnen. Falls der Anstieg nicht proportional ist, gelangt Ihre Anwendung möglicherweise an Ressourcengrenzen wie die CPU-Leistung oder die Kapazität eines verwendeten Diensts.

Klicken Sie auf die Kachel, um Zeiten für bestimmte URLs zu erhalten.

![](./media/app-insights-web-monitor-performance/appinsights-42reqs.png)

### <a name="slowest-requests"></a>Slowest requests
![](./media/app-insights-web-monitor-performance/appinsights-44slowest.png)

Zeigt, welche Anforderungen möglicherweise eine Leistungsfeinabstimmung erfordern.

### <a name="failed-requests"></a>Failed requests
![](./media/app-insights-web-monitor-performance/appinsights-46failed.png)

Zählwert von Anforderungen, die nicht abgefangene Ausnahmefehler verursacht haben.

Klicken Sie auf das Tile, um Details zu bestimmten Fehlern anzuzeigen, und wählen Sie einzelne Anforderungen aus, um die jeweiligen Details anzuzeigen. 

Nur eine repräsentative Menge an Fehler wird zur individuellen Überprüfung zurückgehalten.

### <a name="other-metrics"></a>Other metrics
Um andere verfügbare Metriken aufzurufen, klicken Sie auf einen Graphen und wählen dann alle Metriken ab. Daraufhin werden alle verfügbaren Metriken angezeigt. Klicken Sie auf (i), um die jeweilige Definition der Metrik anzuzeigen.

![Alle Metriken abwählen, um alle verfügbaren anzuzeigen](./media/app-insights-web-monitor-performance/appinsights-62allchoices.png)

Wenn Sie eine Metrik auswählen, werden alle anderen deaktiviert, die nicht im selben Diagramm angezeigt werden können.

## <a name="set-alerts"></a>Festlegen von Benachrichtigungen
Fügen Sie eine Benachrichtigung hinzu, wenn Sie per E-Mail über ungewöhnliche Werte einer beliebigen Metrik informiert werden möchten. Sie können auswählen, ob die E-Mail an die Kontoadministratoren oder an bestimmte E-Mail-Adressen gesendet wird.

![](./media/app-insights-web-monitor-performance/appinsights-413setMetricAlert.png)

Legen Sie die Ressource vor den anderen Eigenschaften fest. Wählen Sie nicht die Webtestressourcen, wenn Sie Benachrichtigungen für Leistungs- oder Nutzungsmetriken festlegen möchten.

Achten Sie auf die Einheiten, die beim Eingeben des Schwellenwerts gefordert sind.

*Ich sehe keine Schaltfläche zum Hinzufügen von Benachrichtigungen.* – Handelt es sich um ein Gruppenkonto, für das Sie nur über schreibgeschützten Zugriff verfügen? Wenden Sie sich an den Kontoadministrator.

## <a name="diagnosis"></a>Diagnostizieren von Problemen
Im Folgenden finden Sie einige Tipps zum Feststellen und Diagnostizieren von Leistungsproblemen:

* Richten Sie [Webtests][availability] ein, um benachrichtigt zu werden, falls Ihre Website nicht erreichbar ist oder fehlerhaft bzw. langsam reagiert. 
* Vergleichen Sie den Request-Zählwert mit anderen Metriken, um festzustellen, ob Fehler oder langsame Reaktionen mit der Last zusammenhängen.
* [Fügen Sie Ihrem Code Trace-Anweisungen hinzu bzw. suchen Sie diese][diagnostic], um Probleme besser einzugrenzen.

## <a name="next"></a>Nächste Schritte
[Webtests][availability]: Lassen Sie in regelmäßigen Abständen aus aller Welt Webanforderungen an Ihre Anwendung senden.

[Diagnostische Spuren protokollieren und suchen][diagnostic]: Fügen Sie Trace-Aufrufe ein, und durchsuchen Sie die Ergebnisse, um Probleme einzugrenzen.

[Nutzungsnachverfolgung][usage]: Erfahren Sie, wie Ihre Anwendung genutzt wird.

[Problembehandlung][qna] und Fragen und Antworten



<!--Link references-->

[availability]: app-insights-monitor-web-app-availability.md
[diagnostic]: app-insights-diagnostic-search.md
[greenbrown]: app-insights-asp-net.md
[qna]: app-insights-troubleshoot-faq.md
[redfield]: app-insights-monitor-performance-live-website-now.md
[start]: app-insights-overview.md
[usage]: app-insights-web-track-usage.md



