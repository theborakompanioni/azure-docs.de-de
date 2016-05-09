<properties
	pageTitle="Verwenden des Application Insights-Portals"
	description="Zeigen Sie Telemetriedaten aus mehreren Komponenten Ihrer Anwendung in einem Dashboard an."
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
	ms.date="04/25/2016"
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

* **ein beliebiges Diagramm oder eine beliebige Kachel**, um weitere Details anzuzeigen.
* **Einstellungen**, um zu vordefinierten Seiten von anderen Metriken zu gelangen.
* [**Metrik-Explorer**](app-insights-metrics-explorer.md), um Metrikseiten Ihrer Wahl zu erstellen.
* [**Suche**](app-insights-diagnostic-search.md), um spezifische Instanzen von Ereignissen zu untersuchen, z.B. Anforderungen, Ausnahmen oder Protokollablaufverfolgungen.
* [**Analytics**](app-insights-analytics.md) für leistungsfähige Abfragen über Ihre Telemetriedaten.


![Hauptwege zum Anzeigen von Telemetriedaten](./media/app-insights-dashboards/010-oview.png)


### Anpassen des Blatts „Übersicht“ 

Legen Sie fest, was in der Übersicht angezeigt werden soll. Unter „Anpassen“ können Sie Abschnittstitel einfügen, Kacheln und Diagramme anordnen, Elemente entfernen und neue Kacheln und Diagramme aus dem Katalog hinzufügen.

![Klicken Sie auf "Bearbeiten". Ziehen Sie Kacheln und Diagramme. Fügen Sie Kacheln aus dem Katalog hinzu. Klicken Sie anschließend auf „Fertig“.](./media/app-insights-dashboards/020-customize.png)

## Dashboards

Nachdem Sie sich beim [Microsoft Azure-Portal](https://portal.azure.com) angemeldet haben, wird Ihnen als Erstes ein Dashboard angezeigt. Hier können Sie für all Ihre Azure-Ressourcen die Diagramme zusammenstellen, die für Sie am wichtigsten sind, einschließlich Telemetriedaten aus [Visual Studio Application Insights](app-insights-overview.md).
 

![Ein angepasstes Dashboard.](./media/app-insights-dashboards/30.png)

1. Sie können jederzeit oben auf die Ecke klicken, um wieder zum Dashboard zu gelangen.
2. Klicken Sie auf ein Diagramm oder eine Kachel auf dem Dashboard, um weitere Details zu den zugehörigen Daten anzuzeigen.
3. Verwenden Sie die Navigationsleiste, um einen vollständigen Überblick über all Ihre Ressourcen zu erhalten.
4. Sie können Dashboards über die Dashboardsymbolleiste erstellen, bearbeiten und freigeben.

## Hinzufügen zu einem Dashboard

Wenn Sie ein Blatt oder Diagramm anzeigen, das Sie besonders interessiert, können Sie eine Kopie des Blatts oder Diagramms an das Dashboard anheften. Es wird dann angezeigt, wenn Sie das nächste Mal zum Dashboard wechseln.

![Um ein Diagramm anzuheften, zeigen Sie darauf, und klicken Sie dann in der Kopfzeile auf „...“.](./media/app-insights-dashboards/33.png)

Beachten Sie, dass Diagramme auf Kacheln gruppiert sind: eine Kachel kann mehr als ein Diagramm enthalten. Sie heften die gesamte Kachel an das Dashboard an.

## Anpassen einer Kachel auf dem Dashboard

Sobald ein Kachel sich auf dem Dashboard befindet, können Sie sie anpassen.

![Bewegen Sie den Mauszeiger über ein Diagramm, um es zu bearbeiten.](./media/app-insights-dashboards/36.png)

1. Fügen Sie der Kachel ein Diagramm hinzu. 
2. Legen Sie die Metrik, die Gruppierungsdimension und den Stil (Tabelle, Graph) eines Diagramms fest.
3. Legen Sie die Zeitraum- und Filtereigenschaften für die Diagramme auf der Kachel fest.
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

<!---HONumber=AcomDC_0427_2016-->