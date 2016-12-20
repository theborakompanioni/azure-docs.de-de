---
title: "Leistungsüberwachung für mobile Web-Apps mit Developer Analytics | Microsoft Docs"
description: "Anwendungsleistung und Nutzungsüberwachung für Entwickler mobiler Apps. Desktop-, Webdienst- und Back-End-Apps mit HockeyApp und Application Insights."
author: alancameronwills
services: application-insights
documentationcenter: 
manager: douge
ms.assetid: 93e0f108-9605-4d8b-8fce-512bfe8c3f0f
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 09/19/2016
ms.author: awills
translationtype: Human Translation
ms.sourcegitcommit: 41ce9b0e323c0938b6db98b99d8d687d1ed0f0ef
ms.openlocfilehash: 4d394ff22e57d162c5adf39c492ca76f3d1f9d3a


---
# <a name="mobile-analytics-with-hockeyapp-and-application-insights"></a>Mobile Analytics mit HockeyApp und Application Insights
Überwachen Sie die Leistung und Verwendung Ihrer in der Betaversion getesteten und bereitgestellten mobilen und Desktop-Apps mit [HockeyApp](https://hockeyapp.net/). Überwachen Sie die unterstützenden Webdienst- und Back-End-Apps mit [Azure Application Insights](app-insights-overview.md). Analysieren Sie Daten der Client- sowie Server-Apps in Analytics, und zeigen Sie die Diagramme in einem Azure-Dashboard nebeneinander an.

Microsoft Developer Analytics umfasst zwei Lösungen für die Überwachung der Anwendungsleistung:

* **HockeyApp für mobile** und Desktop-Apps
  * Verteilen von Testversionen an ausgewählte Benutzer
  * Absturzanalyse
  * Benutzerdefinierte Telemetriedaten für die Verwendungsanalyse
* **Application Insights für Websites** und Dienste sowie Back-End-Anwendungen
  * Leistungs- und Nutzungsmetriken und Warnungen
  * Ausnahmeerfassung und Diagnoseablaufverfolgung
  * Diagnosesuche mit Filterung und zugehörigen Telemetrieverknüpfungen

Beide Lösungen bieten Folgendes:

* Leistungsstarke **[Analyseabfragesprache](app-insights-analytics.md)** für Diagnosen und Analysen
* **[Export von Daten](app-insights-export-telemetry.md)** in Ihren eigenen Speicher
* **Integrierte Dashboardanzeige** von Analysediagrammen und -tabellen

## <a name="monitor-your-app-components"></a>Überwachen der App-Komponenten
Befolgen Sie die Anweisungen auf den folgenden Seiten zum Installieren des SDK im Code, und beginnen Sie mit der Nachverfolgung der App.

### <a name="web-apps---application-insights"></a>Web-Apps – Application Insights
* [ASP.NET-Web-Apps](app-insights-asp-net.md) 
* [Java-Web-Apps](app-insights-java-get-started.md)
* [Node.js-Web-App](https://github.com/Microsoft/ApplicationInsights-node.js)
* [Azure Cloud Services](app-insights-cloudservices.md)

### <a name="mobile-apps---hockeyapp"></a>Mobile Apps – HockeyApp
* [iOS-Apps](https://support.hockeyapp.net/kb/client-integration-ios-mac-os-x-tvos/hockeyapp-for-ios)
* [Mac OS X-Apps](https://support.hockeyapp.net/kb/client-integration-ios-mac-os-x-tvos/hockeyapp-for-mac-os-x)
* [Android-Apps](https://support.hockeyapp.net/kb/client-integration-android/hockeyapp-for-android-sdk)
* [Universal Windows-Apps](https://support.hockeyapp.net/kb/client-integration-windows-and-windows-phone/how-to-create-an-app-for-uwp)
* [Windows Phone 8- und 8.1-Apps](https://support.hockeyapp.net/kb/client-integration-windows-and-windows-phone/hockeyapp-for-windows-phone-silverlight-apps-80-and-81)
* [Windows Presentation Foundation-Apps](https://support.hockeyapp.net/kb/client-integration-windows-and-windows-phone/hockeyapp-for-windows-wpf-apps)

Für Desktop-Apps unter Windows wird HockeyApp empfohlen. Sie können jedoch auch [Telemetriedaten aus einer Windows-Anwendung an Application Insights senden](app-insights-windows-desktop.md). Auf diese Weise können Sie Application Insights ausprobieren.

## <a name="analytics-and-export-for-hockeyapp-telemetry"></a>Analytics und Export für HockeyApp-Telemetriedaten
Sie können benutzerdefinierte Telemetriedaten und Protokolltelemetriedaten von HockeyApp mithilfe der Funktionen für Analytics und fortlaufenden Export von Application Insights untersuchen. Dazu müssen Sie eine [Brücke einrichten](app-insights-hockeyapp-bridge-app.md).




<!--HONumber=Nov16_HO3-->


