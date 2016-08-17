<properties
	pageTitle="Überwachen der Leistung der Azure-Web-App | Microsoft Azure"
	description="Ladezeit für Diagramme und Antwortzeit, Informationen zu den Abhängigkeiten und Festlegen von Benachrichtigungen zur Leistung."
	services="azure-portal"
    documentationCenter="na"
	authors="alancameronwills"
	manager="douge"/>

<tags
	ms.service="azure-portal"
	ms.workload="na"
	ms.tgt_pltfrm="na"
	ms.devlang="na"
	ms.topic="article"
	ms.date="07/28/2016"
	ms.author="awills"/>

# Überwachen der Leistung der Azure-Web-App

Im [Azure-Portal](https://portal.azure.com) können Sie festlegen, dass Statistiken und Details zu den Anwendungsabhängigkeiten in Ihren [Azure-Web-Apps](../app-service-web/app-service-web-overview.md) oder [virtuellen Computern](../virtual-machines/virtual-machines-linux-about.md) gesammelt werden.

Azure unterstützt die Überwachung der Anwendungsleistung (Application Performance Monitoring, APM) durch die Nutzung von Erweiterungen. Diese Erweiterungen werden in Ihrer Anwendung installiert und geben die gesammelten Daten an die Überwachungsdienste zurück.

**Application Insights** und **New Relic** sind zwei der zur Verfügung stehenden Erweiterungen zur Leistungsüberwachung. Um sie zu verwenden, müssen Sie einen Agent zur Laufzeit installieren. Application Insights bietet auch die Option zum Erstellen des Codes mit einem SDK. Mit dem SDK können Sie Code schreiben, um die Auslastung und Leistung Ihrer Anwendung genauer zu überwachen.

## Application Insights

### (Optional) Neuerstellen der App mit dem SDK...

Application Insights kann durch Installieren eines SDK in Ihrer App eine detailliertere Telemetrie bereitstellen.

Fügen Sie in Visual Studio (2013 Update 2 oder höher) Ihrem Projekt das Application Insights SDK hinzu.

![Klicken Sie mit der rechten Maustaste auf das Webprojekt, und wählen Sie "Application Insights hinzufügen".](./media/insights-perf-analytics/03-add.png)

Wenn Sie zur Anmeldung aufgefordert werden, verwenden Sie die Anmeldeinformationen für Ihr Azure-Konto.

Der Vorgang hat zwei Auswirkungen:

1. Es wird eine Application Insights-Ressource in Azure erstellt, unter der die Telemetriedaten gespeichert, analysiert und angezeigt werden.
2. Das Application Insights-NuGet-Paket wird dem Code hinzugefügt und so konfiguriert, dass Telemetriedaten an die Azure-Ressource gesendet werden.

Sie können die Telemetrie testen, indem Sie die App auf dem Entwicklungscomputer ausführen (F5), oder Sie können einfach fortfahren und die App erneut veröffentlichen.

Das SDK stellt eine API bereit, sodass Sie eine [benutzerdefinierte Telemetrie schreiben](../application-insights/app-insights-api-custom-events-metrics.md) können, um die Verwendung nachzuverfolgen.

### ...oder manuelles Einrichten einer Ressource

Wenn Sie das SDK nicht in Visual Studio hinzugefügt haben, müssen Sie die Application Insights-Ressource in Azure einrichten, unter der die Telemetriedaten gespeichert, analysiert und angezeigt werden.

![Klicken Sie auf „Hinzufügen“ > „Entwicklerdienste“ > „Application Insights“. Wählen Sie den App-Typ „ASP.NET“ aus.](./media/insights-perf-analytics/01-new.png)


## Aktivieren einer Erweiterung

1. Navigieren Sie zum Steuerungsblatt der Web-App oder VM, die instrumentiert werden soll.

2. Fügen Sie die Application Insights- oder die New Relic-Erweiterung hinzu.

    Wenn Sie eine Web-App instrumentieren möchten:

![Einstellungen, Erweiterungen, Hinzufügen, Application Insights](./media/insights-perf-analytics/05-extend.png)

Oder wenn Sie einen virtuellen Computer verwenden:

![Klicken Sie auf die Kachel „Analytics“.](./media/insights-perf-analytics/10-vm1.png)



## Untersuchen der Daten

1. Öffnen Sie die Application Insights-Ressource (entweder direkt über „Durchsuchen“ oder über das Tool „Leistungsüberwachung“ der Web-App).

2. Klicken Sie sich durch ein beliebiges Diagramm, um weitere Details anzuzeigen:

    ![Klicken Sie auf dem Blatt mit der Application Insights-Übersicht auf ein Diagramm.](./media/insights-perf-analytics/07-dependency.png)

    Sie können [Blätter mit Metriken anpassen](../application-insights/app-insights-metrics-explorer.md).

3. Klicken Sie weiter, um einzelne Ereignisse und die zugehörigen Eigenschaften anzuzeigen:

    ![Klicken Sie auf einen Ereignistyp, um eine nach diesem Typ gefilterte Suche zu öffnen.](./media/insights-perf-analytics/08-requests.png)

    Beachten Sie den Link „...“ zum Öffnen aller Eigenschaften.

    Sie können [Suchen anpassen](../application-insights/app-insights-diagnostic-search.md).

Verwenden Sie für das leistungsfähigere Durchsuchen von Telemetriedaten die [Analytics-Abfragesprache](../application-insights/app-insights-analytics-tour.md).


## Fragen und Antworten

Wie erreiche ich, dass Daten an eine andere Application Insights-Ressource gesendet werden?

* *Bei Hinzufügung von Application Insights zum Code in Visual Studio:* Klicken Sie mit der rechten Maustaste auf das Projekt, und wählen Sie **Application Insights > Konfigurieren** und dann die gewünschte Ressource. Sie erhalten die Möglichkeit, eine neue Ressource zu erstellen. Führen Sie die Neuerstellung und dann die erneute Bereitstellung durch.
* *Alternative:* Öffnen Sie in Azure das Steuerungsblatt der Web-App und dann **Extras > Erweiterungen**. Löschen Sie die Application Insights-Erweiterung. Öffnen Sie **Extras > Leistung**, und wählen Sie „Hier klicken“, „Application Insights“ und dann die gewünschte Ressource. (Wenn Sie eine neue Application Insights-Ressource erstellen möchten, sollten Sie dies zuerst durchführen.)


## Nächste Schritte

* [Überwachen von Dienstintegritätsmetriken](insights-how-to-customize-monitoring.md), um sicherzustellen, dass Ihr Dienst verfügbar und reaktionsfähig ist.
* [Aktivieren von Überwachung und Diagnose](insights-how-to-use-diagnostics.md), um detaillierte Hochfrequenzmetriken zu Ihrem Dienst zu sammeln.
* [Empfangen von Warnbenachrichtigungen](insights-receive-alert-notifications.md), wenn ein Vorgangsereignis auftritt oder Metriken einen Schwellenwert überschreiten.
* Verwenden von [Application Insights für JavaScript-Apps und Webseiten](../application-insights/app-insights-web-track-usage.md), um die Clienttelemetriedaten über die Browser zu erhalten, mit denen auf eine Webseite zugegriffen wird.
* [Einrichten von Verfügbarkeitswebtests](../application-insights/app-insights-monitor-web-app-availability.md), um benachrichtigt zu werden, wenn Ihre Website nicht verfügbar ist.

<!---HONumber=AcomDC_0803_2016-->