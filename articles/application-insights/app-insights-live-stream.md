---
title: Live Metrics Stream mit benutzerdefinierten Metriken und Diagnosen in Azure Application Insights | Microsoft-Dokumentation
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
ms.date: 05/24/2017
ms.author: cfreeman
ms.translationtype: Human Translation
ms.sourcegitcommit: c785ad8dbfa427d69501f5f142ef40a2d3530f9e
ms.openlocfilehash: 68820f9c018b1076bae8dc0195906d0f34590748
ms.contentlocale: de-de
ms.lasthandoff: 05/26/2017

---

# <a name="live-metrics-stream-monitor--diagnose-with-1-second-latency"></a>Live Metrics Stream: Überwachung und Diagnose mit einer Sekunde Latenzzeit 

Überprüfen Sie die Funktionsfähigkeit Ihrer Livewebanwendung in der Produktion mithilfe von Live Metrics Stream von [Application Insights](app-insights-overview.md). Wählen und filtern Sie Metriken und Leistungsindikatoren für die Überwachung in Echtzeit, ohne dass der Dienst beeinträchtigt wird. Überprüfen Sie Stapelüberwachungen von fehlerhaften Anforderungen und Ausnahmen. In Kombination mit [Profiler](app-insights-profiler.md), [Snapshot Debugger](app-insights-snapshot-debugger.md) und [Leistungstests](app-insights-monitor-web-app-availability.md#performance-tests) bietet Live Metrics Stream ein leistungsfähiges und nicht invasives Diagnosetool für Ihre Livewebsite.

Mit Live Metrics Stream haben Sie folgende Möglichkeiten:

* Überprüfen Sie eine veröffentlichte Korrektur durch Überwachen von Leistungs- und Fehlerindikatoren.
* Überwachen Sie die Auswirkungen von Testauslastungen, und diagnostizieren Sie Probleme im laufenden Betrieb. 
* Konzentrieren Sie sich auf bestimmte Testsitzungen, oder filtern Sie bekannte Probleme heraus, indem Sie die zu überwachenden Metriken auswählen und filtern.
* Rufen Sie Ausnahmeablaufverfolgungen in Echtzeit ab.
* Experimentieren Sie mit Filtern, um die wichtigsten KPIs zu ermitteln.
* Überwachen Sie Windows-Leistungsindikatoren live.
* Identifizieren Sie mühelos einen Server mit Problemen, und filtern Sie nach allen KPIs/dem Livefeed nur für diesen Server.

[![Video zu Live Metrics Stream](./media/app-insights-live-stream/youtube.png)](https://www.youtube.com/watch?v=zqfHf1Oi5PY)

Live Metrics Stream ist derzeit für ASP.NET-Apps verfügbar, die lokal oder in der Cloud ausgeführt werden. 

## <a name="get-started"></a>Erste Schritte

1. Wenn Sie [Application Insights](app-insights-asp-net.md) noch nicht in Ihrer ASP.NET-Web-App oder [Windows-Server-App](app-insights-windows-services.md) installiert haben, holen Sie dies jetzt nach. 
2. Führen Sie ein **Update auf die neueste Version** des Application Insights-Pakets durch. Klicken Sie in Visual Studio mit der rechten Maustaste auf Ihr Projekt, und wählen Sie **NuGet-Pakete verwalten** aus. Öffnen Sie die Registerkarte **Updates**, aktivieren Sie **Vorabversion einbeziehen**, und wählen Sie alle Microsoft.ApplicationInsights.*-Pakete aus.

    Stellen Sie Ihre App erneut bereit.

3. Öffnen Sie im [Azure-Portal](https://portal.azure.com) die Application Insights-Ressource für Ihre App und anschließend Live Stream.

4. [Sichern Sie den Steuerkanal](#secure-the-control-channel), wenn Sie sensible Daten (z.B. Kundennamen) in Ihren Filtern verwenden möchten.


![Klicken Sie auf dem Blatt „Übersicht“ auf „Livedatenstrom“.](./media/app-insights-live-stream/live-stream-2.png)

### <a name="no-data-check-your-server-firewall"></a>Sie sehen keine Daten? Überprüfen Sie die Serverfirewall

Überprüfen Sie, ob die [ausgehenden Ports für Live Metrics Stream](app-insights-ip-addresses.md#outgoing-ports) in der Firewall Ihrer Server geöffnet sind. 


## <a name="how-does-live-metrics-stream-differ-from-metrics-explorer-and-analytics"></a>Wie unterscheidet sich Live Metrics Stream von Metrik-Explorer und Analytics?

| |Live Stream | Metrik-Explorer und Analytics |
|---|---|---|
|Latenz|Daten werden innerhalb einer Sekunde angezeigt.|Aggregation innerhalb mehrerer Minuten|
|Keine Beibehaltung|Daten werden beibehalten, solange sie im Diagramm angezeigt werden, und dann verworfen.|[Daten werden 90 Tage lang beibehalten.](app-insights-data-retention-privacy.md#how-long-is-the-data-kept)|
|On-Demand-Streaming|Daten werden beim Öffnen von Live Metrics gestreamt.|Daten werden gesendet, sobald das SDK installiert und aktiviert wird.|
|Kostenlos|Keine Gebühren für Live Stream-Daten|[Gebührenpflichtig](app-insights-pricing.md)
|Stichproben|Alle ausgewählten Metriken und Indikatoren werden übertragen. Für Fehler und Stapelüberwachungen werden Stichproben erstellt. Telemetrieprozessoren werden nicht angewendet.|Für Ereignisse können [Stichproben](app-insights-api-filtering-sampling.md) erstellt werden.|
|Steuerkanal|Filtersteuersignale werden an das SDK gesendet. Es empfiehlt sich, [diesen Kanal zu sichern](#secure-channel).|Unidirektionale Kommunikation zum Portal|


## <a name="select-and-filter-your-metrics"></a>Auswählen und Filtern der Metriken

(Für klassische ASP.NET-Apps mit dem aktuellen SDK verfügbar.)

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

## <a name="secure-the-control-channel"></a>Sichern des Steuerkanals
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

## <a name="generating-a-performance-test-load"></a>Generieren einer Leistungstestauslastung

Verwenden Sie das Blatt „Leistungstest“, wenn Sie die Auswirkungen einer zunehmenden Auslastung überwachen möchten. Beim Leistungstest werden Anforderungen von mehreren gleichzeitigen Benutzern simuliert. Sie können entweder „manuelle Tests“ (Ping-Tests) einer einzelnen URL oder einen [Webleistungstest mit mehreren Schritten](app-insights-monitor-web-app-availability.md#multi-step-web-tests) ausführen, den Sie hochladen (auf dieselbe Weise wie einen Verfügbarkeitstest).

> [!TIP]
> Öffnen Sie nach dem Erstellen des Leistungstests den Test und das Live Stream-Blatt in separaten Fenstern. So können Sie sehen, wann der Leistungstest in der Warteschlange gestartet wird, und gleichzeitig den Livestream überwachen.
>


## <a name="troubleshooting"></a>Problembehandlung

Sie sehen keine Daten? Wenn sich Ihre Anwendung in einem geschützten Netzwerk befindet: Live Metrics Stream verwendet andere IP-Adressen als andere Application Insights-Telemetriedaten. Stellen Sie sicher, dass [diese IP-Adressen](app-insights-ip-addresses.md) in Ihrer Firewall geöffnet sind.



## <a name="next-steps"></a>Nächste Schritte
* [Überwachen der Verwendung mit Application Insights](app-insights-web-track-usage.md)
* [Verwenden der Diagnosesuche](app-insights-diagnostic-search.md)
* [Profiler](app-insights-profiler.md)
* [Snapshot Debugger](app-insights-snapshot-debugger.md)
