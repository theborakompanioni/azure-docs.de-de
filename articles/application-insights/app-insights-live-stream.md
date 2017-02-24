---
title: Verwenden von Live Metrics Stream in Azure Application Insights | Microsoft-Dokumentation
description: "Überwachen Sie die Leistung Ihrer Web-App in Echtzeit, um die Auswirkungen einer Veröffentlichung oder einer anderen Änderung zu beobachten."
services: application-insights
documentationcenter: 
author: alancameronwills
manager: carmonm
ms.assetid: 1f471176-38f3-40b3-bc6d-3f47d0cbaaa2
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 02/08/2017
ms.author: awills
translationtype: Human Translation
ms.sourcegitcommit: c453774b04c7005ce5948617beea8770b499b88d
ms.openlocfilehash: 2da6d02055616db5f9854481054ff12c69dc4801


---

# <a name="live-metrics-stream-instant-metrics-for-close-monitoring"></a>Live Metrics Stream: sofortige Metriken für eine genaue Überwachung
Live Metrics Stream zeigt Ihnen Ihre [Application Insights](app-insights-overview.md)-Metriken für den aktuellen Zeitpunkt an – nahezu in Echtzeit, mit einer Latenz von einer Sekunde. Diese unmittelbare Überwachung ist äußerst nützlich, wenn Sie einen neuen Build veröffentlichen und sicherstellen möchten, dass alles wie erwartet funktioniert, oder wenn Sie einen Incident in Echtzeit untersuchen möchten.

![Klicken Sie auf dem Blatt „Übersicht“ auf „Livedatenstrom“.](./media/app-insights-live-stream/live-stream.png)

Im Gegensatz zum [Metrik-Explorer](app-insights-metrics-explorer.md) zeigt Live Metrics Stream einen festen Satz von Metriken an. Die Daten werden nur so lange beibehalten, wie sie im Diagramm angezeigt werden. Anschließend werden sie verworfen.

Live Metrics Stream-Daten sind kostenlos, Ihre Rechnung erhöht sich nicht.

## <a name="live-failures"></a>Live-Fehler

Wenn Fehler oder Ausnahmen protokolliert werden, wählt Live Stream eine Auswahl daraus aus. Klicken Sie auf **Anhalten**, um ein bestimmtes Beispiel anzuzeigen, und wählen Sie ein Ereignis, um dessen Details anzuzeigen.

![Auswahl der Live-Fehler](./media/app-insights-live-stream/live-stream-failures.png)


Live Metrics Stream steht mit der neuesten Webversion von [Application Insights SDK](https://www.nuget.org/packages/Microsoft.ApplicationInsights.Web/) zur Verfügung.



## <a name="troubleshooting"></a>Problembehandlung

Sie sehen keine Daten? Live Metrics Stream verwendet einen anderen Port als andere Application Insights-Telemetriedaten. Stellen Sie sicher, dass [diese Ports](app-insights-ip-addresses.md) in Ihrer Firewall geöffnet sind.



## <a name="next-steps"></a>Nächste Schritte
* [Überwachen der Verwendung mit Application Insights](app-insights-overview-usage.md)
* [Verwenden der Diagnosesuche](app-insights-diagnostic-search.md)




<!--HONumber=Feb17_HO2-->


