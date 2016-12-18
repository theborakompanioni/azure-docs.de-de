---
title: "Überwachen der Leistung der Azure-Web-App | Microsoft Docs"
description: "Überwachung der Anwendungsleistung für Azure-Web-Apps. Ladezeit für Diagramme und Antwortzeit, Informationen zu den Abhängigkeiten und Festlegen von Benachrichtigungen zur Leistung."
services: application-insights
documentationcenter: .net
author: alancameronwills
manager: douge
ms.assetid: 0b2deb30-6ea8-4bc4-8ed0-26765b85149f
ms.service: azure-portal
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/24/2016
ms.author: awills
translationtype: Human Translation
ms.sourcegitcommit: 41ce9b0e323c0938b6db98b99d8d687d1ed0f0ef
ms.openlocfilehash: c5869c2f4f593d8ffd1992ec2a7dbc473898f3ad


---
# <a name="monitor-azure-web-app-performance"></a>Überwachen der Leistung der Azure-Web-App
Im [Azure-Portal](https://portal.azure.com) können Sie eine Anwendungsleistungsüberwachung für Ihre [Azure-Web-Apps](../app-service-web/app-service-web-overview.md) einrichten. [Azure Application Insights](app-insights-overview.md) instrumentiert Ihre App, um Telemetriedaten über ihre Aktivitäten an den Application Insights-Dienst zu senden, wo sie gespeichert und analysiert werden. Hier können Sie dann mithilfe von Metrikdiagrammen und Suchtools Probleme diagnostizieren, die Leistung verbessern und die Nutzung untersuchen.

## <a name="run-time-or-build-time"></a>Laufzeit oder Buildzeit
Zum Konfigurieren der Überwachung kann die App auf zwei Arten instrumentiert werden:

* **Laufzeit:** Sie können eine Erweiterung für die Leistungsüberwachung auswählen, wenn Ihre Web-App bereits live geschaltet wurde. Die App muss dazu nicht neu erstellt oder erneut installiert werden. Sie erhalten einen Standardsatz von Paketen zur Überwachung von Antwortzeiten, Erfolgsraten, Ausnahmen, Abhängigkeiten und Ähnlichem. 
* **Buildzeit:** Sie können ein Paket in der App installieren, die Sie gerade entwickeln. Diese Option ist wesentlich flexibler. Neben den gleichen Standardpaketen können Sie auch Code schreiben, um die Telemetrie anzupassen oder Ihre eigenen Telemetriedaten zu senden. Sie können bestimmte Aktivitäten protokollieren oder Ereignisse gemäß der Semantik Ihrer App-Domäne erfassen. 

## <a name="run-time-instrumentation-with-application-insights"></a>Laufzeitinstrumentierung mit Application Insights
Wenn Sie bereits eine Web-App in Azure ausführen, verfügen Sie schon über eine Art der Überwachung, und zwar von Anforderungs- und Fehlerraten. Fügen Sie Application Insights hinzu, um die Überwachung zu erweitern, z.B. auf Reaktionszeiten, Überwachung beim Aufrufen von Abhängigkeiten, intelligente Erkennung und leistungsstarke Analytics-Abfragesprache. 

1. In der Azure-Systemsteuerung für Ihre Web-App können Sie **Application Insights auswählen**.
   
    ![Wählen Sie unter „Überwachung“ die Option „Application Insights“.](./media/app-insights-azure-web-apps/05-extend.png)
   
   * Wählen Sie die Option zum Erstellen einer neuen Ressource, sofern Sie nicht bereits auf einem anderen Weg eine Application Insights-Ressource für diese App eingerichtet haben.
2. **Instrumentieren Sie Ihre Web-App**, nachdem Application Insights installiert wurde. 
   
    ![Instrumentieren Ihrer Web-App](./media/app-insights-azure-web-apps/restart-web-app-for-insights.png)
3. **Überwachen Sie Ihre App**.  [Untersuchen Sie die Daten](#explore-the-data).

Später können Sie die App bei Bedarf mit Application Insights erstellen und erneut bereitstellen.

*Wie entferne ich Application Insights oder stelle auf das Senden an eine andere Ressource um?*

* Öffnen Sie in Azure das Steuerungsblatt der Web-App und unter „Entwicklungstools“ dann **Erweiterungen**. Löschen Sie die Application Insights-Erweiterung. Wählen Sie anschließend unter „Überwachung“ die Option „Application Insights“, und erstellen Sie die gewünschte Ressource bzw. wählen Sie sie aus.

## <a name="build-the-app-with-application-insights"></a>Erstellen der App mit Application Insights
Application Insights kann durch Installieren eines SDK in Ihrer App eine detailliertere Telemetrie bereitstellen. Beispielsweise können Sie Ablaufverfolgungsprotokolle erfassen, [benutzerdefinierte Telemetriedaten schreiben](app-insights-api-custom-events-metrics.md) und ausführlichere Berichte zu Ausnahmen erhalten.

1. Fügen Sie in **Visual Studio** (2013 Update 2 oder höher) Ihrem Projekt das Application Insights SDK hinzu.
   
    ![Klicken Sie mit der rechten Maustaste auf das Webprojekt, und wählen Sie "Application Insights hinzufügen".](./media/app-insights-azure-web-apps/03-add.png)
   
    Wenn Sie zur Anmeldung aufgefordert werden, verwenden Sie die Anmeldeinformationen für Ihr Azure-Konto.
   
    Der Vorgang hat zwei Auswirkungen:
   
   1. Es wird eine Application Insights-Ressource in Azure erstellt, unter der die Telemetriedaten gespeichert, analysiert und angezeigt werden.
   2. Das Application Insights-NuGet-Paket wird dem Code hinzugefügt und so konfiguriert, dass Telemetriedaten an die Azure-Ressource gesendet werden.
2. **Testen Sie die Telemetrie**, indem Sie die App auf Ihrem Entwicklungscomputer (F5) ausführen.
3. **Veröffentlichen Sie die App** wie gewohnt in Azure. 

*Wie führe ich die Umstellung durch, damit Daten an eine andere Application Insights-Ressource gesendet werden?*

* Klicken Sie in Visual Studio mit der rechten Maustaste auf das Projekt, und wählen Sie **Application Insights > Konfigurieren** und dann die gewünschte Ressource aus. Sie erhalten die Möglichkeit, eine neue Ressource zu erstellen. Führen Sie die Neuerstellung und dann die erneute Bereitstellung durch.

## <a name="explore-the-data"></a>Untersuchen der Daten
1. Das Blatt „Application Insights“ Ihrer Web-App-Systemsteuerung enthält Livemetriken. Anforderungen und Fehler werden darüber innerhalb von ein oder zwei Sekunden nach dem Auftreten eingeblendet. Dies ist eine sehr nützliche Anzeige beim erneuten Veröffentlichen Ihrer App, weil Ihnen Probleme sofort auffallen.
2. Klicken Sie weiter, bis Sie zur vollständigen Application Insights-Ressource gelangen.

    ![Navigieren per Klick](./media/app-insights-azure-web-apps/view-in-application-insights.png)

    Sie gelangen auch direkt über die Azure-Ressourcennavigation dorthin.

1. Klicken Sie sich durch ein beliebiges Diagramm, um weitere Details anzuzeigen:
   
    ![Klicken Sie auf dem Blatt mit der Application Insights-Übersicht auf ein Diagramm.](./media/app-insights-azure-web-apps/07-dependency.png)
   
    Sie können [Blätter mit Metriken anpassen](app-insights-metrics-explorer.md).
2. Klicken Sie weiter, um einzelne Ereignisse und die zugehörigen Eigenschaften anzuzeigen:
   
    ![Klicken Sie auf einen Ereignistyp, um eine nach diesem Typ gefilterte Suche zu öffnen.](./media/app-insights-azure-web-apps/08-requests.png)
   
    Beachten Sie den Link „...“ zum Öffnen aller Eigenschaften.
   
    Sie können [Suchvorgänge anpassen](app-insights-diagnostic-search.md).

Verwenden Sie die [Analytics-Abfragesprache](app-insights-analytics-tour.md), um Telemetriedaten noch effizienter zu durchsuchen.

## <a name="next-steps"></a>Nächste Schritte
* [Ermöglichen des Sendens von Azure-Diagnosedaten an Application Insights](app-insights-azure-diagnostics.md)
* [Überwachen von Dienstintegritätsmetriken](../monitoring-and-diagnostics/insights-how-to-customize-monitoring.md), um sicherzustellen, dass Ihr Dienst verfügbar und reaktionsfähig ist.
* [Empfangen von Warnbenachrichtigungen](../monitoring-and-diagnostics/insights-receive-alert-notifications.md) , wenn ein Vorgangsereignis auftritt oder Metriken einen Schwellenwert überschreiten.
* Verwenden von [Application Insights für JavaScript-Apps und Webseiten](app-insights-web-track-usage.md), um Clienttelemetriedaten von den Browsern zu erhalten, mit denen auf eine Webseite zugegriffen wird
* [Einrichten von Verfügbarkeitswebtests](app-insights-monitor-web-app-availability.md), um benachrichtigt zu werden, wenn Ihre Website nicht verfügbar ist




<!--HONumber=Nov16_HO3-->


