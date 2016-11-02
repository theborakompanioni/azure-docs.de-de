<properties
	pageTitle="Überwachen der Leistung der Azure-Web-App | Microsoft Azure"
	description="Überwachung der Anwendungsleistung für Azure-Web-Apps. Ladezeit für Diagramme und Antwortzeit, Informationen zu den Abhängigkeiten und Festlegen von Benachrichtigungen zur Leistung."
	services="application-insights"
    documentationCenter=".net"
	authors="alancameronwills"
	manager="douge"/>

<tags
	ms.service="azure-portal"
	ms.workload="na"
	ms.tgt_pltfrm="na"
	ms.devlang="na"
	ms.topic="article"
	ms.date="09/01/2016"
	ms.author="awills"/>

# Überwachen der Leistung der Azure-Web-App

Im [Azure-Portal](https://portal.azure.com) können Sie eine Anwendungsleistungsüberwachung für Ihre [Azure-Web-Apps](../app-service-web/app-service-web-overview.md) einrichten. [Visual Studio Application Insights](app-insights-overview.md) instrumentiert Ihre App, um Telemetriedaten über ihre Aktivitäten an den Application Insights-Dienst zu senden, wo sie gespeichert und analysiert werden. Hier können Sie dann mithilfe von Metrikdiagrammen und Suchtools Probleme diagnostizieren, die Leistung verbessern und die Nutzung untersuchen.

## Laufzeit oder Buildzeit

Zum Konfigurieren der Überwachung kann die App auf zwei Arten instrumentiert werden:

* **Laufzeit:** Sie können eine Erweiterung für die Leistungsüberwachung auswählen, wenn Ihre Web-App bereits live geschaltet wurde. Die App muss dazu nicht neu erstellt oder erneut installiert werden. Sie erhalten einen Standardsatz von Paketen zur Überwachung von Antwortzeiten, Erfolgsraten, Ausnahmen, Abhängigkeiten und Ähnlichem.

    **Application Insights** und **New Relic** sind zwei der zur Verfügung stehenden Erweiterungen für die Leistungsüberwachung zur Laufzeit.
 
* **Buildzeit:** Sie können ein Paket in der App installieren, die Sie gerade entwickeln. Diese Option ist wesentlich flexibler. Neben den gleichen Standardpaketen können Sie auch Code schreiben, um die Telemetrie anzupassen oder Ihre eigenen Telemetriedaten zu senden. Sie können bestimmte Aktivitäten protokollieren oder Ereignisse gemäß der Semantik Ihrer App-Domäne erfassen.

    **Application Insights** stellt Buildzeitpakete bereit.


## Erstellen der App mit dem Application Insights-Paket

Application Insights kann durch Installieren eines SDK in Ihrer App eine detailliertere Telemetrie bereitstellen.

Fügen Sie in Visual Studio (2013 Update 2 oder höher) Ihrem Projekt das Application Insights SDK hinzu.

![Klicken Sie mit der rechten Maustaste auf das Webprojekt, und wählen Sie "Application Insights hinzufügen".](./media/app-insights-azure-web-apps/03-add.png)

Wenn Sie zur Anmeldung aufgefordert werden, verwenden Sie die Anmeldeinformationen für Ihr Azure-Konto.

Der Vorgang hat zwei Auswirkungen:

1. Es wird eine Application Insights-Ressource in Azure erstellt, unter der die Telemetriedaten gespeichert, analysiert und angezeigt werden.
2. Das Application Insights-NuGet-Paket wird dem Code hinzugefügt und so konfiguriert, dass Telemetriedaten an die Azure-Ressource gesendet werden.

Sie können die Telemetrie testen, indem Sie die App auf dem Entwicklungscomputer ausführen (F5), oder Sie können einfach fortfahren und die App erneut veröffentlichen.

Das SDK stellt eine API bereit, sodass Sie eine [benutzerdefinierte Telemetrie schreiben](../application-insights/app-insights-api-custom-events-metrics.md) können, um die Verwendung nachzuverfolgen.

### Manuelles Einrichten einer Ressource

Wenn Sie das SDK nicht in Visual Studio hinzugefügt haben, müssen Sie in Azure eine Application Insights-Ressource einrichten, unter der die Telemetriedaten gespeichert, analysiert und angezeigt werden.

![Klicken Sie auf „Hinzufügen“ > „Entwicklerdienste“ > „Application Insights“. Wählen Sie den App-Typ „ASP.NET“ aus.](./media/app-insights-azure-web-apps/01-new.png)


## Aktivieren einer Erweiterung

1. Navigieren Sie zum Steuerungsblatt der Web-App oder VM, die instrumentiert werden soll.

2. Fügen Sie die Application Insights- oder die New Relic-Erweiterung hinzu.

    Wenn Sie eine Web-App instrumentieren möchten:

![Einstellungen, Erweiterungen, Hinzufügen, Application Insights](./media/app-insights-azure-web-apps/05-extend.png)

Oder wenn Sie einen virtuellen Computer verwenden:

![Klicken Sie auf die Kachel „Analytics“.](./media/app-insights-azure-web-apps/10-vm1.png)



## Untersuchen der Daten

1. Öffnen Sie die Application Insights-Ressource (entweder direkt über „Durchsuchen“ oder über das Tool „Leistungsüberwachung“ der Web-App).

2. Klicken Sie sich durch ein beliebiges Diagramm, um weitere Details anzuzeigen:

    ![Klicken Sie auf dem Blatt mit der Application Insights-Übersicht auf ein Diagramm.](./media/app-insights-azure-web-apps/07-dependency.png)

    Sie können [Blätter mit Metriken anpassen](../application-insights/app-insights-metrics-explorer.md).

3. Klicken Sie weiter, um einzelne Ereignisse und die zugehörigen Eigenschaften anzuzeigen:

    ![Klicken Sie auf einen Ereignistyp, um eine nach diesem Typ gefilterte Suche zu öffnen.](./media/app-insights-azure-web-apps/08-requests.png)

    Beachten Sie den Link „...“ zum Öffnen aller Eigenschaften.

    Sie können [Suchvorgänge anpassen](../application-insights/app-insights-diagnostic-search.md).

Verwenden Sie die [Analytics-Abfragesprache](../application-insights/app-insights-analytics-tour.md), um Telemetriedaten noch effizienter zu durchsuchen.


## Fragen und Antworten

Wie erreiche ich, dass Daten an eine andere Application Insights-Ressource gesendet werden?

* *Vorgehensweise, wenn Sie Application Insights Ihrem Code in Visual Studio hinzugefügt haben:* Klicken Sie mit der rechten Maustaste auf das Projekt, und wählen Sie **Application Insights > Konfigurieren** und dann die gewünschte Ressource aus. Sie erhalten die Möglichkeit, eine neue Ressource zu erstellen. Führen Sie die Neuerstellung und dann die erneute Bereitstellung durch.
* *Vorgehensweise in anderen Fällen:* Öffnen Sie in Azure das Steuerungsblatt der Web-App und dann **Extras > Erweiterungen**. Löschen Sie die Application Insights-Erweiterung. Öffnen Sie **Extras > Leistung**, und wählen Sie „Hier klicken“ > „Application Insights“ und dann die gewünschte Ressource aus. (Wenn Sie eine neue Application Insights-Ressource erstellen möchten, sollten Sie dies zuerst durchführen.)


## Nächste Schritte

* [Ermöglichen des Sendens von Azure-Diagnosedaten an Application Insights](app-insights-azure-diagnostics.md)
* [Überwachen von Dienstintegritätsmetriken](../azure-portal/insights-how-to-customize-monitoring.md), um sicherzustellen, dass Ihr Dienst verfügbar und reaktionsfähig ist
* [Empfangen von Warnbenachrichtigungen](../azure-portal/insights-receive-alert-notifications.md), wenn ein Vorgangsereignis auftritt oder Metriken einen Schwellenwert überschreiten
* Verwenden von [Application Insights für JavaScript-Apps und Webseiten](app-insights-web-track-usage.md), um Clienttelemetriedaten von den Browsern zu erhalten, mit denen auf eine Webseite zugegriffen wird
* [Einrichten von Verfügbarkeitswebtests](app-insights-monitor-web-app-availability.md), um benachrichtigt zu werden, wenn Ihre Website nicht verfügbar ist

<!---HONumber=AcomDC_0907_2016-->