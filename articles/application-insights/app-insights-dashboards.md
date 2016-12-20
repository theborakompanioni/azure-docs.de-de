---
title: Dashboards und Navigation im Application Insights-Portal | Microsoft Docs
description: "Es wird beschrieben, wie Sie Ansichten für Ihre wichtigsten APM-Diagramme und -Abfragen erstellen."
services: application-insights
documentationcenter: 
author: alancameronwills
manager: douge
ms.assetid: 39b0701b-2fec-4683-842a-8a19424f67bd
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: multiple
ms.topic: article
ms.date: 10/18/2016
ms.author: awills
translationtype: Human Translation
ms.sourcegitcommit: dea21a59b189d1d3d474cbc5e67f64df485a1981
ms.openlocfilehash: 369feec4d8d5234ffcde9d4f75811f3847d88834


---
# <a name="navigation-and-dashboards-in-the-application-insights-portal"></a>Navigation und Dashboards im Application Insights-Portal
Nachdem Sie [Application Insights für Ihr Projekt eingerichtet haben](app-insights-overview.md), werden Telemetriedaten zur Leistung und Nutzung Ihrer App in der Application Insights-Ressource Ihres Projekts im [Azure-Portal](https://portal.azure.com) angezeigt.

## <a name="find-your-telemetry"></a>Finden der Telemetriedaten
Melden Sie sich am [Azure-Portal](https://portal.azure.com) an, und navigieren Sie zur Application Insights-Ressource, die Sie für Ihre App erstellt haben.

![Wählen Sie „Durchsuchen“, „Application Insights“ und dann Ihre App aus.](./media/app-insights-dashboards/00-start.png)

Das Übersichtsblatt (bzw. die Seite) für Ihre App zeigt eine Zusammenfassung der wichtigen Diagnosemetriken für Ihre App an und ist das Tor zu den anderen Features des Portals.

![Hauptwege zum Anzeigen von Telemetriedaten](./media/app-insights-dashboards/010-oview.png)

Sie können alle anderen Diagramme anpassen und an ein Dashboard anheften. Auf diese Weise können Sie die wichtigen Diagramme aus verschiedenen Apps zusammenführen.

## <a name="dashboards"></a>Dashboards
Nachdem Sie sich beim [Microsoft Azure-Portal](https://portal.azure.com) angemeldet haben, wird Ihnen als Erstes ein Dashboard angezeigt. Hier können Sie für all Ihre Azure-Ressourcen die Diagramme zusammenstellen, die für Sie am wichtigsten sind, einschließlich Telemetriedaten aus [Azure Application Insights](app-insights-overview.md).

![Ein angepasstes Dashboard.](./media/app-insights-dashboards/31.png)

1. **Navigieren zu bestimmten Ressourcen**, z.B. zu Ihrer App in Application Insights: Verwenden Sie die linke Leiste.
2. **Zurückwechseln zum aktuellen Dashboard** oder zu anderen zuletzt verwendeten Ansichten: Verwenden Sie das Dropdownmenü oben links.
3. **Wechseln von Dashboards**: Verwenden Sie das Dropdownmenü in der Titelleiste des Dashboards.
4. **Erstellen, Bearbeiten und Freigeben von Dashboards**: Verwenden Sie die Symbolleiste des Dashboards.
5. **Bearbeiten des Dashboards**: Zeigen Sie auf eine Kachel, und verwenden Sie die obere Leiste, um sie zu verschieben, anzupassen oder zu entfernen.

## <a name="add-to-a-dashboard"></a>Hinzufügen zu einem Dashboard
Wenn Sie ein Blatt oder Diagramm anzeigen, das Sie besonders interessiert, können Sie eine Kopie des Blatts oder Diagramms an das Dashboard anheften. Es wird dann angezeigt, wenn Sie das nächste Mal zum Dashboard wechseln.

![Um ein Diagramm anzuheften, zeigen Sie darauf, und klicken Sie dann in der Kopfzeile auf „...“.](./media/app-insights-dashboards/33.png)

1. Heften Sie ein Diagramm an ein Dashboard an. Eine Kopie des Diagramms wird auf dem Dashboard angezeigt.
2. Heften Sie das ganze Blatt an das Dashboard an. Es wird auf dem Dashboard als Kachel angezeigt, mit der Sie interagieren können.
3. Klicken Sie in die obere linke Ecke, um zurück zum aktuellen Dashboard zu wechseln. Über das Dropdownmenü können Sie zur aktuellen Ansicht zurückkehren.

Beachten Sie, dass Diagramme auf Kacheln gruppiert sind: eine Kachel kann mehr als ein Diagramm enthalten. Sie heften die gesamte Kachel an das Dashboard an.

### <a name="pin-any-query-in-analytics"></a>Anheften von beliebigen Abfragen in Analytics
Sie können Analysediagramme auch an ein [freigegebenes](#share-dashboards-with-your-team) Dashboard [anheften](app-insights-analytics-using.md#pin-to-dashboard). Dadurch können Sie neben Standardmetriken Diagramme einer beliebigen Abfrage hinzufügen. (Für dieses Feature wird eine Gebühr berechnet.)

## <a name="adjust-a-tile-on-the-dashboard"></a>Anpassen einer Kachel auf dem Dashboard
Sobald ein Kachel sich auf dem Dashboard befindet, können Sie sie anpassen.

![Bewegen Sie den Mauszeiger über ein Diagramm, um es zu bearbeiten.](./media/app-insights-dashboards/36.png)

1. Fügen Sie der Kachel ein Diagramm hinzu.
2. Legen Sie die Metrik, die Gruppierungsdimension und den Stil (Tabelle, Graph) eines Diagramms fest.
3. Ziehen Sie den Mauszeiger über das Diagramm, um die Ansicht zu vergrößern. Klicken Sie auf die Schaltfläche „Rückgängig“, um den Zeitraum zurückzusetzen. Legen Sie die Filtereigenschaften für die Diagramme auf der Kachel fest.
4. Legen Sie den Titel für die Kachel fest.

Für Kacheln, die aus Metrik-Explorer-Blättern angeheftet wurden, stehen mehr Bearbeitungsoptionen zur Verfügung als für Kacheln, die aus Übersichtsblättern angeheftet wurden.

Bearbeitungen wirken sich nicht auf die ursprüngliche Kachel aus, die Sie angeheftet haben,

## <a name="switch-between-dashboards"></a>Wechsel zwischen Dashboards
Sie können mehrere Dashboards speichern und zwischen diesen wechseln. Wenn Sie ein Diagramm oder Blatt anheften, werden diese dem aktuellen Dashboard hinzugefügt.

![Um zwischen Dashboards zu wechseln, klicken Sie auf ein Dashboard, und wählen Sie dann ein gespeichertes Dashboard aus. Um ein neues Dashboard zu erstellen und zu speichern, klicken Sie auf „Neu“. Um die Anordnung zu ändern, klicken Sie auf „Bearbeiten“.](./media/app-insights-dashboards/32.png)

Sie könnten beispielsweise ein Dashboard besitzen, das Sie im Teamraum im Vollbildmodus anzeigen, sowie ein weiteres für die allgemeine Entwicklung.

Auf dem Dashboard wird ein Blatt als Kachel angezeigt: Klicken Sie darauf, um das Blatt anzuzeigen. Ein Diagramm repliziert das Diagramm an seinem ursprünglichen Speicherort.

![Klicken Sie auf eine Kachel, um das Blatt anzuzeigen, das von der Kachel repräsentiert wird.](./media/app-insights-dashboards/35.png)

## <a name="share-dashboards"></a>Freigeben von Dashboards
Wenn Sie ein Dashboard erstellt haben, können Sie es für andere Benutzer freigeben.

![Klicken Sie in der Dashboardüberschrift auf „Freigeben“.](./media/app-insights-dashboards/41.png)

Erfahren Sie mehr über [Rollen und Zugriffssteuerung](app-insights-resources-roles-access-control.md).

## <a name="app-navigation"></a>App-Navigation
Das Blatt „Übersicht“ ist der Ausgangspunkt für weitere Informationen zu Ihrer App.

* **Diagramm oder Kachel**: Klicken Sie auf eine beliebige Kachel oder ein Diagramm, um weitere Details zur Anzeige einzublenden.

### <a name="overview-blade-buttons"></a>Schaltflächen auf dem Blatt „Übersicht“
![Obere Navigationsleiste auf dem Blatt „Übersicht“](./media/app-insights-dashboards/app-overview-top-nav.png)

* [**Metrik-Explorer**](app-insights-metrics-explorer.md): Erstellen Sie Ihre eigenen Diagramme für Leistung und Nutzung.
* [**Suche**](app-insights-diagnostic-search.md): Untersuchen Sie spezifische Instanzen von Ereignissen, z.B. Anforderungen, Ausnahmen oder Protokollablaufverfolgungen.
* [**Analytics**](app-insights-analytics.md): Leistungsfähige Abfragen Ihrer Telemetriedaten.
* **Zeitbereich**: Passen Sie den Bereich an, der für alle Diagramme des Blatts angezeigt wird.
* **Löschen**: Löschen Sie die Application Insights-Ressource für diese App. Sie sollten außerdem entweder die Application Insights-Pakete aus Ihrem App-Code entfernen oder den [Instrumentationsschlüssel](app-insights-create-new-resource.md#copy-the-instrumentation-key) in Ihrer App bearbeiten, um die Telemetriedaten an eine andere Application Insights-Ressource zu leiten.

### <a name="essentials-tab"></a>Registerkarte „Zusammenfassung“
* [Instrumentationsschlüssels](app-insights-create-new-resource.md#copy-the-instrumentation-key): Identifiziert diese App-Ressource.
* Preise: Hier können Sie Features verfügbar machen und Obergrenzen für das Volumen festlegen.

### <a name="app-navigation-bar"></a>Leiste für App-Navigation
![Linke Navigationsleiste](./media/app-insights-dashboards/app-left-nav-bar.png)

* **Übersicht**: Dient zum Zurückwechseln zum Blatt mit der App-Übersicht.
* **Aktivitätsprotokoll**: Enthält Warnungen und Azure-Verwaltungsereignisse.
* [**Zugriffssteuerung**](app-insights-resources-roles-access-control.md): Ermöglicht den Zugriff für Teammitglieder und andere Personen.
* [**Tags**](../resource-group-using-tags.md): Verwenden Sie Tags zum Gruppieren Ihrer App mit anderen Apps.

UNTERSUCHEN

* [**Anwendungszuordnung**](app-insights-app-map.md): Aktive Zuordnung, die die Komponenten Ihrer Anwendung anzeigt, abgeleitet aus den Abhängigkeitsinformationen.
* [**Proaktive Diagnose**](app-insights-proactive-diagnostics.md): Überprüfung der letzten Leistungswarnungen.
* [**Livedatenströme**](app-insights-metrics-explorer.md#live-metrics-stream): Ein fester Satz zeitnaher Metriken, die nützlich sind, wenn Sie einen neuen Build bereitstellen oder debuggen.
* [**Verfügbarkeit/Webtests**](app-insights-monitor-web-app-availability.md): Ermöglicht das weltweite Senden von regelmäßigen Anforderungen an Ihre Web-App.*
* [**Fehler, Leistung**](app-insights-web-monitor-performance.md): Ausnahmen, Fehlerraten und Antwortzeiten für Anforderungen an Ihre App und für Anforderungen von Ihrer App an [Abhängigkeiten](app-insights-asp-net-dependencies.md).
* [**Leistung**](app-insights-web-monitor-performance.md): Reaktionszeit, Reaktionszeiten von Abhängigkeiten.
* [Server:](app-insights-web-monitor-performance.md) Leistungsindikatoren. Verfügbar, wenn Sie den [Statusmonitor installieren](app-insights-monitor-performance-live-website-now.md).
* **Browser:** Leistung von Seitenansichten und AJAX. Verfügbar, wenn Sie [Ihre Webseiten instrumentieren](app-insights-javascript.md).
* **Nutzung:** Seitenansichts-, Benutzer- und Sitzungszähler. Verfügbar, wenn Sie [Ihre Webseiten instrumentieren](app-insights-javascript.md).

KONFIGURIEREN

* **Erste Schritte:** Inlinetutorial.
* **Eigenschaften:** Instrumentierungsschlüssel, Abonnement und Ressourcen-ID.
* [Warnungen:](app-insights-alerts.md) Konfiguration von Warnungen zu Metriken.
* [Fortlaufender Export:](app-insights-export-telemetry.md) Konfigurieren Sie den Export von Telemetriedaten in Azure Storage.
* [Leistungstests:](app-insights-monitor-web-app-availability.md#performance-tests) Richten Sie eine synthetische Last auf Ihrer Website ein.
* [Kontingent und Preise](app-insights-pricing.md) und [Erfassungs-Stichprobenerstellung](app-insights-sampling.md).
* **API-Zugriff**: Dient zum Erstellen von [Versionsanmerkungen](app-insights-annotations.md) und wird für die Datenzugriffs-API verwendet.
* [**Arbeitselemente**](app-insights-diagnostic-search.md#create-work-item): Stellen Sie eine Verbindung mit einem System für die Aufgabennachverfolgung her, um Fehler erstellen zu können, während Sie die Telemetrie überprüfen.

EINSTELLUNGEN

* [**Sperren**](../resource-group-lock-resources.md): Sperren Sie Azure-Ressourcen.
* [**Automatisierungsskript**](app-insights-powershell.md): Exportieren Sie eine Definition der Azure-Ressource, um diese als Vorlage für die Erstellung neuer Ressourcen zu verwenden.

SUPPORT

* **Supportanfrage**: Hierfür ist ein kostenpflichtiges Abonnement erforderlich. Siehe auch [Hilfe](app-insights-get-dev-support.md).

## <a name="whats-next"></a>Wie geht es weiter?
|  |  |
| --- | --- |
| [Metrik-Explorer](app-insights-metrics-explorer.md)<br/>Filtern und Segmentieren von Metriken |![Suchbeispiel](./media/app-insights-dashboards/64.png) |
| [Diagnosesuche](app-insights-diagnostic-search.md)<br/>Suchen und Untersuchen von Ereignissen bzw. zugehörigen Ereignissen und Erstellen von Fehlern |![Suchbeispiel](./media/app-insights-dashboards/61.png) |
| [Analyse](app-insights-analytics.md)<br/>Leistungsfähige Abfragesprache |![Suchbeispiel](./media/app-insights-dashboards/63.png) |



<!--HONumber=Nov16_HO3-->


