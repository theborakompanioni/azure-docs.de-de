---
title: Intelligente Erkennung in Azure Application Insights | Microsoft Docs
description: "Application Insights führt eine automatische umfassende Analyse Ihrer App-Telemetrie durch und warnt Sie vor potenziellen Problemen."
services: application-insights
documentationcenter: windows
author: rakefetj
manager: carmonm
ms.assetid: 2eeb4a35-c7a1-49f7-9b68-4f4b860938b2
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 10/31/2016
ms.author: bwren
ms.translationtype: HT
ms.sourcegitcommit: b6c65c53d96f4adb8719c27ed270e973b5a7ff23
ms.openlocfilehash: 66c4a82153dccd82977cf42c5bb9de2a8a8d6255
ms.contentlocale: de-de
ms.lasthandoff: 08/17/2017

---
# <a name="smart-detection-in-application-insights"></a>Intelligente Erkennung in Application Insights
 Die intelligente Erkennung warnt Sie automatisch vor potenziellen Leistungsproblemen in Ihrer Webanwendung. Sie führt eine proaktive Analyse der Telemetriedaten durch, die Ihre App an [Application Insights](app-insights-overview.md) sendet. Bei einem plötzlichen Anstieg der Fehlerraten oder bei ungewöhnlichen Mustern in der Client- oder Serverleistung erhalten Sie eine Warnung. Diese Funktion muss nicht konfiguriert werden. Sie wird ausgeführt, wenn Ihre Anwendung genügend Telemetriedaten sendet.

Sie können auf Warnungen der intelligenten Erkennung über die erhaltenen E-Mails oder das Blatt für die intelligenten Erkennung zugreifen.

## <a name="review-your-smart-detections"></a>Überprüfen der Ergebnisse der intelligenten Erkennung
Sie haben zwei Möglichkeiten, Erkennungen anzuzeigen:

* **Sie erhalten eine E-Mail** von Application Insights. Hier sehen Sie ein typisches Beispiel:
  
    ![E-Mail-Warnung](./media/app-insights-proactive-diagnostics/03.png)
  
    Klicken Sie auf die große Schaltfläche, um ausführlichere Informationen im Portal zu öffnen.
* Die Kachel für die **intelligente Erkennung** auf dem Blatt „Übersicht“ Ihrer App zeigt die Anzahl aktueller Warnungen an. Klicken Sie auf die Kachel, um eine Liste der aktuellen Warnungen anzuzeigen.

![Aktuelle Erkennungen anzeigen](./media/app-insights-proactive-diagnostics/04.png)

Wählen Sie eine Warnung aus, um Details anzuzeigen.

## <a name="what-problems-are-detected"></a>Welche Probleme werden erkannt?
Es gibt drei Arten der Erkennung:

* [Intelligente Erkennung – Fehlerabweichungen](app-insights-proactive-failure-diagnostics.md). Wir nutzen Machine Learning, um die voraussichtliche Rate fehlerhafter Anforderungen für Ihre App (korreliert mit Lastangaben und anderen Faktoren) festzulegen. Falls die Fehlerrate den erwarteten Rahmen überschreitet, wird eine Warnung gesendet.
* [Intelligente Erkennung – Leistungsabweichungen](app-insights-proactive-performance-diagnostics.md). Sie erhalten Benachrichtigungen, wenn sich die Reaktionszeit eines Vorgangs oder die Abhängigkeitsdauer im Vergleich zu den typischen Verlaufsdaten verschlechtert oder wenn für die Reaktionszeit oder die Seitenladezeit ein anomales Muster erkannt wird.   
* [Intelligente Erkennung – Azure-Clouddienstprobleme](https://azure.microsoft.com/blog/proactive-notifications-on-cloud-service-issues-with-azure-diagnostics-and-application-insights/). Sie erhalten Warnungen, wenn Ihre App in Azure Cloud Services gehostet wird und bei einer Rolleninstanz Startfehler, häufige Wiederverwendungen oder Abstürze zur Laufzeit auftreten.

(Über die Hilfelinks in den jeweiligen Benachrichtigungen gelangen Sie zu den relevanten Artikeln.)

## <a name="video"></a>Video

> [!VIDEO https://channel9.msdn.com/events/Connect/2016/112/player]

## <a name="next-steps"></a>Nächste Schritte
Mit den folgenden Diagnosetools können Sie die Telemetrie Ihrer App untersuchen:

* [Metrik-Explorer](app-insights-metrics-explorer.md)
* [Suchexplorer](app-insights-diagnostic-search.md)
* [Analytics: Leistungsfähige Abfragesprache](app-insights-analytics-tour.md)

Die intelligente Erkennungen erfolgt vollkommen automatisch. Vielleicht möchten Sie aber weitere Warnungen einrichten?

* [Einrichten von Warnungen in Application Insights](app-insights-alerts.md)
* [Verfügbarkeitswebtests](app-insights-monitor-web-app-availability.md) 


