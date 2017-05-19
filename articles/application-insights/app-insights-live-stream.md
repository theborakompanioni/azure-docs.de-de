---
title: Benutzerdefinierte Livemetriken und Diagnose in Azure Application Insights | Microsoft-Dokumentation
description: "Überwachen Sie Ihre Web-App mit benutzerdefinierten Metriken in Echtzeit, und diagnostizieren Sie Probleme mit einem Livefeed zu Fehlern, Ablaufverfolgungen und Ereignissen."
services: application-insights
documentationcenter: 
author: SoubhagyaDash
manager: carmonm
ms.assetid: 1f471176-38f3-40b3-bc6d-3f47d0cbaaa2
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 05/10/2017
ms.author: cfreeman
ms.translationtype: Human Translation
ms.sourcegitcommit: 5e92b1b234e4ceea5e0dd5d09ab3203c4a86f633
ms.openlocfilehash: 86e01cf6cb14334e85da4102610fa7feb66cb543
ms.contentlocale: de-de
ms.lasthandoff: 05/10/2017

---

# <a name="custom-live-metrics-and-events-monitor--diagnose-with-1-second-latency"></a>Benutzerdefinierte Livemetriken und Ereignisse: Überwachung und Diagnose mit einer Sekunde Latenzzeit 
Live Metrics Stream zeigt Ihre [Application Insights](app-insights-overview.md)-Metriken und -Ereignisse nahezu in Echtzeit mit einer Wartezeit von einer Sekunde an. Diese sofortige Überwachung trägt dazu bei, die durchschnittliche Dauer bis zur Erkennung und Diagnose zu reduzieren und SLAs zu erfüllen. Ihre Möglichkeiten:
* Live-Überwachung benutzerdefinierter KPIs: Experimentieren Sie mit Filtern, um Ihre vorhandene Application Insights-Telemetrie aufzugliedern und die wichtigsten KPIs sofort aus dem Portal abzurufen. Code, Konfigurationsänderungen oder Bereitstellungen sind nicht erforderlich. Alle benutzerdefinierten Metriken oder Messungen, die gesendet werden, sind ebenfalls verfügbar.
* Live-Erkennung und -Diagnose: Anforderungs- und Abhängigkeitsfehler werden in Echtzeit zusammen mit ausführlichen Ausnahmeablaufverfolgungen angezeigt. Filtern Sie bekannte Probleme aus, um sich auf echte/neue Probleme zu konzentrieren.
* Live-Debuggen: Reproduzieren Sie ein Problem und zeigen dabei die gesamte zugehörigen Telemetrie in Echtzeit an, indem Sie einen benutzerdefinierten Filter für eine bestimmte Sitzungs-ID (oder benutzerdefinierte Attribute) zur Identifizierung der reproduzierten Interaktion anwenden. Erfassen Sie Informationen von bestimmten oder allen Servern für einen leichten Einstieg in die Behebung des Problems.
* Unmittelbar anzeigen, wie Ihr Ressourcenverbrauch auf Last reagiert: Überwachen Sie in Echtzeit *einen beliebigen* Windows-Leistungsindikator, während Sie Auslastungstests durchführen, oder überwachen Sie die Produktion, um eingreifen zu können, noch bevor es zu einer Beeinträchtigung kommt. Konfigurationsänderungen oder Bereitstellungen sind nicht erforderlich.
* Überprüfen Sie eine veröffentlichte Korrektur. Vergewissern Sie sich während des Vorgangs, dass Ihr Dienst stabil ist. Überprüfen Sie, ob der behobene Fehler nicht mehr auftritt.
* Identifizieren Sie mühelos einen Server mit Problemen, und filtern Sie nach allen KPIs/dem Livefeed nur für diesen Server.

Live Metrics & Events Stream-Daten sind kostenlos, Ihre Rechnung erhöht sich nicht. Die Daten werden bei Bedarf von Ihrem Server gestreamt, wenn Sie die Portalumgebung öffnen. Die Daten werden nur so lange beibehalten, wie sie im Diagramm angezeigt werden. Anschließend werden sie verworfen. Die vollständige Funktionalität steht für klassische ASP.NET-Anwendungen zur Verfügung. .NET Core-Anwendungen bieten derzeit nur einen festen Satz von Live-Metriken und Fehlerbeispielen. Derzeit werden alle unterstützten SDKs überarbeitet, damit sie die Anforderungen der aktuellen Livestreaming-Funktionen erfüllen. 

Wir erfassen den Livestream von Ihren Anwendungsinstanzen, bevor Stichproben genommen oder benutzerdefinierte Telemetrieprozessoren angewendet werden. 

![Video zu Live Metrics Stream](./media/app-insights-live-stream/youtube.png) [Video zu Live Metrics Stream](https://www.youtube.com/watch?v=zqfHf1Oi5PY)

Greifen Sie auf Live-Metriken und -Ereignisse zu, indem Sie entweder auf die Option links oder die Schaltfläche auf dem Blatt „Übersicht“ klicken:

![Klicken Sie auf dem Blatt „Übersicht“ auf „Livedatenstrom“.](./media/app-insights-live-stream/live-stream-2.png)

## <a name="custom-live-kpi"></a>Benutzerdefinierte Live-KPIs
Sie können benutzerdefinierte KPIs live überwachen, indem Sie im Portal beliebige Filter auf Application Insights-Telemetrie anwenden. Klicken Sie auf das Filtersteuerelement, das angezeigt wird, wenn Sie den Mauszeiger über eines der Diagramme bewegen. Das folgende Diagramm stellt eine benutzerdefinierte KPI für die Anforderungsanzahl mit Filtern für die Attribute „URL“ und „Dauer“ dar. Überprüfen Sie Ihre Filter in der Streamvorschau, die jederzeit einen Livefeed von Telemetriedaten anzeigt, die den von Ihnen angegeben Kriterien entsprechen. 

![Benutzerdefinierte Anforderungs-KPI](./media/app-insights-live-stream/live-stream-filteredMetric.png)

Sie können einen anderen Wert als die Anzahl überwachen. Die Optionen hängen von der Typ des Streams ab. Bei diesem kann es sich um eine beliebige Application Insights-Telemetrie handeln: Anforderungen, Abhängigkeiten, Ausnahmen, Ablaufverfolgungen, Ereignisse oder Metriken. Sie können auch eine eigene [benutzerdefinierte Messung](app-insights-api-custom-events-metrics.md#properties) verwenden:

![Wertoptionen](./media/app-insights-live-stream/live-stream-valueoptions.png)

Neben Application Insights-Telemetrie können Sie auch einen beliebigen Windows-Leistungsindikator überwachen, indem Sie diese Streamoptionen auswählen und den Namen des Leistungsindikators angeben.

Livemetriken werden an zwei Punkten aggregiert: lokal auf jedem Server und übergreifend auf allen Servern. Sie können die Standardeinstellung durch die Auswahl anderer Optionen in der entsprechenden Dropdownliste ändern.

## <a name="sample-telemetry-custom-live-diagnostic-events"></a>Telemetriebeispiel: Benutzerdefinierte Live-Diagnoseereignisse
Der Livefeed von Ereignissen zeigt standardmäßig Beispiele für fehlgeschlagene Anforderungen und Abhängigkeitsaufrufe, Ausnahmen, Ereignisse und Ablaufverfolgungen an. Klicken Sie jederzeit auf das Filtersymbol, um die angewendeten Kriterien anzuzeigen. 

![Standard-Livefeed](./media/app-insights-live-stream/live-stream-eventsdefault.png)

Wie bei Metriken können Sie beliebige Kriterien für alle Application Insights-Telemetrietypen angeben. In diesem Beispiel wählen wir bestimmte Anforderungsfehler, Ablaufverfolgungen und Ereignisse aus. Wir wählen außerdem alle Ausnahmen und Abhängigkeitsfehler aus.

![Benutzerdefinierter Livefeed](./media/app-insights-live-stream/live-stream-events.png)

Hinweis: Verwenden Sie derzeit für meldungsbasierte Ausnahmekriterien die Meldung für äußere Ausnahmen. Verwenden Sie im vorangehenden Beispiel zum Herausfiltern mit der Meldung für innere Ausnahmen (folgt auf das Trennzeichen „<--“) „Der Client wurde getrennt.“ eine Meldung, die keine Kriterien vom Typ „Fehler beim Lesen des Anforderungsinhalts“ enthält.

Klicken Sie auf ein Element im Livefeed, um die Details dazu anzuzeigen. Sie können den Feed anhalten, indem Sie auf **Anhalten** klicken, einfach nach unten scrollen oder auf ein Element klicken. Der Livefeed wird fortgesetzt, wenn Sie wieder nach oben scrollen oder auf den Zähler der Elemente klicken, die im angehaltenen Zustand erfasst wurden.

![Auswahl der Live-Fehler](./media/app-insights-live-stream/live-metrics-eventdetail.png)

## <a name="filter-by-server-instance"></a>Filtern nach Serverinstanz

Wenn Sie eine bestimmte Serverrolleninstanz überwachen möchten, können Sie nach Server filtern.

![Auswahl der Live-Fehler](./media/app-insights-live-stream/live-stream-filter.png)

## <a name="sdk-requirements"></a>SDK-Anforderungen
Die benutzerdefinierte Live Metrics Stream-Instanz steht mit der Webversion 2.4.0-beta2 oder höher von [Application Insights SDK](https://www.nuget.org/packages/Microsoft.ApplicationInsights.Web/) zur Verfügung. Denken Sie daran, im NuGet-Paket-Manager die Option „Vorabversion einbeziehen“ auszuwählen.

## <a name="authenticated-channel"></a>Authentifizierter Kanal
Die von Ihnen angegebenen benutzerdefinierten Filterkriterien werden an die Livemetrikkomponente des Application Insights SDK zurückgesendet. Der Filter können potenziell vertrauliche Informationen wie z.B. Kunden-IDs enthalten. Zum Sichern des Kanals können Sie neben dem Instrumentierungsschlüssel auch einen geheimen API-Schlüssel verwenden.
### <a name="create-an-api-key"></a>Erstellen eines API-Schlüssels

![API-Schlüssel erstellen](./media/app-insights-live-stream/live-metrics-apikeycreate.png)

### <a name="add-api-key-to-configuration"></a>Hinzufügen eines API-Schlüssels zur Konfiguration
Fügen Sie den API-Authentifizierungsschlüssel in der Datei „applicationinsights.config“ dem QuickPulseTelemetryModule-Element hinzu:
``` XML

<Add Type="Microsoft.ApplicationInsights.Extensibility.PerfCounterCollector.QuickPulse.QuickPulseTelemetryModule, Microsoft.AI.PerfCounterCollector">
      <AuthenticationApiKey>YOUR-API-KEY-HERE</AuthenticationApiKey>
</Add> 

```
Oder legen Sie ihn im Code auf dem QuickPulseTelemetryModule-Element fest:

``` C#

    module.AuthenticationApiKey = "YOUR-API-KEY-HERE";

```

Wenn Sie jedoch alle verbundenen Server erkennen und ihnen vertrauen, können Sie die benutzerdefinierte Filter ohne den authentifizierten Kanal ausprobieren. Diese Option steht sechs Monate lang zur Verfügung. Diese Überschreibung muss einmal für jede neue Sitzung, oder wenn ein neuer Server online geschaltet wird, durchgeführt werden.

![Authentifizierungsoptionen für Livemetriken](./media/app-insights-live-stream/live-stream-auth.png)

>[!NOTE]
>Es wird dringend empfohlen, dass Sie den authentifizierten Kanal vor dem Eingeben von potenziell vertraulichen Informationen wie der Kunden-ID in den Filterkriterien einrichten.
>

## <a name="troubleshooting"></a>Problembehandlung

Sie sehen keine Daten? Wenn sich Ihre Anwendung in einem geschützten Netzwerk befindet: Live Metrics Stream verwendet andere IP-Adressen als andere Application Insights-Telemetriedaten. Stellen Sie sicher, dass [diese IP-Adressen](app-insights-ip-addresses.md) in Ihrer Firewall geöffnet sind.



## <a name="next-steps"></a>Nächste Schritte
* [Überwachen der Verwendung mit Application Insights](app-insights-web-track-usage.md)
* [Verwenden der Diagnosesuche](app-insights-diagnostic-search.md)


