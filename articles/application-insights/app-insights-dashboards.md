<properties
	pageTitle="Verwenden des Application Insights-Portals"
	description="Metriken, Suche, Dashboards und Einstellungen im Application Insights-Portal."
	services="application-insights"
    documentationCenter=""
	authors="alancameronwills"
	manager="douge"/>

<tags
	ms.service="application-insights"
	ms.workload="tbd"
	ms.tgt_pltfrm="ibiza"
	ms.devlang="multiple"
	ms.topic="article" 
	ms.date="07/30/2016"
	ms.author="awills"/>

# Navigation und Dashboards im Application Insights-Portal

Nachdem Sie [Application Insights für Ihr Projekt eingerichtet haben](app-insights-overview.md), werden Telemetriedaten zur Leistung und Nutzung Ihrer App in der Application Insights-Ressource Ihres Projekts im [Azure-Portal](https://portal.azure.com) angezeigt.

## Finden der Telemetriedaten

Melden Sie sich am [Azure-Portal](https://portal.azure.com) an, und wechseln Sie zur Application Insights-Ressource, die Sie für Ihre App erstellt haben.

![Wählen Sie „Durchsuchen“, „Application Insights“ und dann Ihre App aus.](./media/app-insights-dashboards/00-start.png)

Auf der Übersichtsseite finden Sie grundlegende Telemetriedaten sowie Links zu weiteren Daten. Der Inhalt hängt vom Typ Ihrer App ab und kann angepasst werden.


## Das Blatt für die App-Übersicht

Das Übersichtsblatt (bzw. die Seite) für Ihre App zeigt eine Zusammenfassung der wichtigen Diagnosemetriken für Ihre App an und ist das Tor zu den anderen Features des Portals.

Klicken Sie auf:

* **Ein Diagramm oder eine Kachel**, um weitere Details darüber anzuzeigen, was im Diagramm dargestellt wird.
* **Einstellungen**, um vordefinierte Blätter anderer Metriken sowie Konfigurationsseiten anzuzeigen.
* [**Metrik-Explorer**](app-insights-metrics-explorer.md), um Metrikseiten Ihrer Wahl zu erstellen.
* [**Suche**](app-insights-diagnostic-search.md), um spezifische Instanzen von Ereignissen zu untersuchen, z.B. Anforderungen, Ausnahmen oder Protokollablaufverfolgungen.
* [**Analytics**](app-insights-analytics.md) für leistungsfähige Abfragen über Ihre Telemetriedaten.
* [**Livedatenströme**](app-insights-metrics-explorer.md#live-stream) für einen festen Satz zeitnaher Metriken, die nützlich sind, wenn Sie einen neuen Build bereitstellen oder debuggen.


![Hauptwege zum Anzeigen von Telemetriedaten](./media/app-insights-dashboards/010-oview.png)


### Anpassen des Blatts „Übersicht“ 

Legen Sie fest, was in der Übersicht angezeigt werden soll. Unter „Anpassen“ können Sie Abschnittstitel einfügen, Kacheln und Diagramme anordnen, Elemente entfernen und neue Kacheln und Diagramme aus dem Katalog hinzufügen.

![Klicken Sie auf "Bearbeiten". Ziehen Sie Kacheln und Diagramme. Fügen Sie Kacheln aus dem Katalog hinzu. Klicken Sie anschließend auf „Fertig“.](./media/app-insights-dashboards/020-customize.png)

## Dashboards

Nachdem Sie sich beim [Microsoft Azure-Portal](https://portal.azure.com) angemeldet haben, wird Ihnen als Erstes ein Dashboard angezeigt. Hier können Sie für all Ihre Azure-Ressourcen die Diagramme zusammenstellen, die für Sie am wichtigsten sind, einschließlich Telemetriedaten aus [Visual Studio Application Insights](app-insights-overview.md).
 

![Ein angepasstes Dashboard.](./media/app-insights-dashboards/31.png)


1. Sie können Dashboards über die Dashboardsymbolleiste erstellen, bearbeiten und freigeben.
2. Kehren Sie zum aktuellen Dashboard zurück, oder wechseln Sie zu anderen kürzlich verwendeten Ansichten.
3. Wechseln Sie die Dashboards.
4. Sie können Dashboards erstellen, bearbeiten und freigeben.
5. Zeigen Sie auf eine Kachel, und verwenden Sie die obere Leiste, um sie zu verschieben, anzupassen oder zu entfernen.

## Hinzufügen zu einem Dashboard

Wenn Sie ein Blatt oder Diagramm anzeigen, das Sie besonders interessiert, können Sie eine Kopie des Blatts oder Diagramms an das Dashboard anheften. Es wird dann angezeigt, wenn Sie das nächste Mal zum Dashboard wechseln.

![Um ein Diagramm anzuheften, zeigen Sie darauf, und klicken Sie dann in der Kopfzeile auf „...“.](./media/app-insights-dashboards/33.png)

1. Heften Sie ein Diagramm an ein Dashboard an. Eine Kopie des Diagramms wird auf dem Dashboard angezeigt.
2. Heften Sie das ganze Blatt an das Dashboard an. Es wird auf dem Dashboard als Kachel angezeigt, mit der Sie interagieren können.
3. Klicken Sie in die obere linke Ecke, um zurück zum aktuellen Dashboard zu wechseln. Über das Dropdownmenü können Sie zur aktuellen Ansicht zurückkehren.

Beachten Sie, dass Diagramme auf Kacheln gruppiert sind: eine Kachel kann mehr als ein Diagramm enthalten. Sie heften die gesamte Kachel an das Dashboard an.

## Anpassen einer Kachel auf dem Dashboard

Sobald ein Kachel sich auf dem Dashboard befindet, können Sie sie anpassen.

![Bewegen Sie den Mauszeiger über ein Diagramm, um es zu bearbeiten.](./media/app-insights-dashboards/36.png)

1. Fügen Sie der Kachel ein Diagramm hinzu.
2. Legen Sie die Metrik, die Gruppierungsdimension und den Stil (Tabelle, Graph) eines Diagramms fest.
3. Legen Sie die Zeitraum- und Filtereigenschaften für die Diagramme auf der Kachel fest. Sie können den Zeitraum des Diagramms direkt angeben oder festlegen, dass er aus dem Zeitraum des Dashboards geerbt wird.
4. Legen Sie den Titel für die Kachel fest.

Für Kacheln, die aus Metrik-Explorer-Blättern angeheftet wurden, stehen mehr Bearbeitungsoptionen zur Verfügung als für Kacheln, die aus Übersichtsblättern angeheftet wurden.

Bearbeitungen wirken sich nicht auf die ursprüngliche Kachel aus, die Sie angeheftet haben,


## Wechsel zwischen Dashboards

Sie können mehrere Dashboards speichern und zwischen diesen wechseln. Wenn Sie ein Diagramm oder Blatt anheften, werden diese dem aktuellen Dashboard hinzugefügt.

![Um zwischen Dashboards zu wechseln, klicken Sie auf ein Dashboard, und wählen Sie dann ein gespeichertes Dashboard aus. Um ein neues Dashboard zu erstellen und zu speichern, klicken Sie auf „Neu“. Um die Anordnung zu ändern, klicken Sie auf „Bearbeiten“.](./media/app-insights-dashboards/32.png)

Sie könnten beispielsweise ein Dashboard besitzen, das Sie im Teamraum im Vollbildmodus anzeigen, sowie ein weiteres für die allgemeine Entwicklung.


Auf dem Dashboard wird ein Blatt als Kachel angezeigt: Klicken Sie darauf, um das Blatt anzuzeigen. Ein Diagramm repliziert das Diagramm an seinem ursprünglichen Speicherort.

![Klicken Sie auf eine Kachel, um das Blatt anzuzeigen, das von der Kachel repräsentiert wird.](./media/app-insights-dashboards/35.png)


## Freigeben von Dashboards für Ihr Team

Wenn Sie ein Dashboard erstellt haben, können Sie es für andere Benutzer freigeben.


![Klicken Sie in der Dashboardüberschrift auf „Freigeben“.](./media/app-insights-dashboards/41.png)

Erfahren Sie mehr über [Rollen und Zugriffssteuerung](app-insights-resources-roles-access-control.md).

## Einstellungen

Über die Schaltfläche „Einstellungen“ auf dem Blatt „Übersicht“ können Sie nicht nur auf die Konfiguration, sondern auch auf verschiedene Diagrammtypen zugreifen.

* **Überwachungsprotokolle**: Überprüfen Sie Warnungen und Aktionen, die in Ihrer App-Ressource ausgeführt wurden.
* [Neue Supportanforderung](app-insights-get-dev-support.md): Hilfeoptionen für alle Aspekte von Azure. Für Application Insights können Sie auch Hilfe erhalten, indem Sie auf die Smiley-Schaltfläche in der oberen rechten Ecke des Fensters klicken.
* [Anwendungszuordnung](app-insights-dependencies.md#application-map): Aktive Zuordnung, die die Komponenten Ihrer Anwendung anzeigt, abgeleitet aus den Abhängigkeitsinformationen.
* [Live Metrics Stream](app-insights-metrics-explorer.md#live-metrics-stream): Wichtige Metriken mit einer Latenz von etwa 1 Sekunde, zur Überwachung einer neuen Version.
* **Fehler**: Anforderungen und Abhängigkeitsaufrufe mit Fehlern sowie Ausnahmen. [Blatt mit bearbeitbaren Metriken](app-insights-metrics-explorer.md).
* **Leistung**: Reaktionszeit, Reaktionszeiten von Abhängigkeiten. [Blatt mit bearbeitbaren Metriken](app-insights-metrics-explorer.md).
* [Server](app-insights-web-monitor-performance.md): Leistungsindikatoren. Verfügbar, wenn Sie den [Statusmonitor installieren](app-insights-monitor-performance-live-website-now.md).
* **Browser**: Leistung von Seitenansichten und AJAX. Verfügbar, wenn Sie [Ihre Webseiten instrumentieren](app-insights-javascript.md).
* **Nutzung**: Seitenansichts-, Benutzer- und Sitzungszähler. Verfügbar, wenn Sie [Ihre Webseiten instrumentieren](app-insights-javascript.md).
* **Erste Schritte**: Inlinetutorial.
* **Eigenschaften**: Instrumentierungsschlüssel, Abonnement und Ressourcen-ID.
* [Verfügbarkeit](app-insights-monitor-web-app-availability.md): Konfiguration und Ergebnisse von Webtests.
* [Warnungen](app-insights-alerts.md): Konfiguration von Warnungen zu Metriken.
* [Fortlaufender Export](app-insights-export-telemetry.md): Konfigurieren Sie den Export von Telemetriedaten in Azure Storage.
* [Leistungstests](app-insights-monitor-web-app-availability.md#performance-tests): Richten Sie eine synthetische Last auf Ihrer Website ein.
* [Kontingent und Preise](app-insights-pricing.md) und [Erfassungs-Stichprobenerstellung](app-insights-sampling.md).
* **API-Zugriff**: Zurzeit zum Erstellen von [Versionsanmerkungen](app-insights-annotations.md) und für die Datenzugriffs-API verwendet.
* [**Arbeitselemente**](app-insights-diagnostic-search.md#create-work-item): Stellen Sie eine Verbindung mit einem System für die Aufgabennachverfolgung her, um Fehler erstellen zu können, während Sie die Telemetrie überprüfen.
* [**Benutzer**](app-insights-resources-roles-access-control.md): Verwalten Sie die Benutzer, die auf Ihre App-Ressource zugreifen dürfen.
* [**Rollen**](app-insights-resources-roles-access-control.md): Verwalten Sie die Aktionen, die Benutzer ausführen können.
* [**Tags**](..\resource-group-using-tags.md): Organisieren Sie Azure-Ressourcen.
* [**Sperren**](..\resource-group-lock-resources.md): Sperren Sie Azure-Ressourcen.
* [**Exportieren einer Vorlage**](app-insights-powershell.md): Exportieren Sie eine Definition der Azure-Ressource, um diese als Vorlage für die Erstellung neuer Ressourcen zu verwenden.

## Wie geht es weiter?

||
|---|---
|[Metrik-Explorer](app-insights-metrics-explorer.md)<br/>Filtern und Segmentieren von Metriken|![Suchbeispiel](./media/app-insights-dashboards/64.png)
|[Diagnosesuche](app-insights-diagnostic-search.md)<br/>Suchen und Untersuchen von Ereignissen bzw. zugehörigen Ereignissen und Erstellen von Fehlern |![Suchbeispiel](./media/app-insights-dashboards/61.png)
|[Analytics](app-insights-analytics.md)<br/>Leistungsfähige Abfragesprache| ![Suchbeispiel](./media/app-insights-dashboards/63.png)

<!---HONumber=AcomDC_0803_2016-->