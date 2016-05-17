<properties
	pageTitle="Erstellen eines benutzerdefinierten Dashboards in Log Analytics | Microsoft Azure"
	description="In diesem Leitfaden wird beschrieben, wie in Log Analytics-Dashboards alle gespeicherten Protokollsuchvorgänge visualisiert werden können, um Ihnen einen zentralen Überblick über Ihre Umgebung zu ermöglichen."
	services="log-analytics"
	documentationCenter=""
	authors="bandersmsft"
	manager="jwhit"
	editor=""/>

<tags
	ms.service="log-analytics"
	ms.workload="na"
	ms.tgt_pltfrm="na"
	ms.devlang="na"
	ms.topic="article"
	ms.date="04/28/2016"
	ms.author="banders"/>

# Erstellen eines benutzerdefinierten Dashboards in Log Analytics

In diesem Leitfaden wird beschrieben, wie in Log Analytics-Dashboards alle gespeicherten Protokollsuchvorgänge visualisiert werden können, um Ihnen einen zentralen Überblick über Ihre Umgebung zu ermöglichen.

![Beispiel-Dashboard](./media/log-analytics-dashboards/oms-dashboards-example-dash.png)

## Wie erstelle ich mein Dashboard?

Wechseln Sie zuerst zur OMS-Übersicht, indem Sie im linken Navigationsbereich auf die Schaltfläche „Übersicht“ klicken. Auf der linken Seite sehen Sie die Kachel „Mein Dashboard“. Klicken Sie darauf, um einen Drilldown in das Dashboard durchzuführen.

![Übersicht](./media/log-analytics-dashboards/oms-dashboards-overview.png)



## Hinzufügen einer Kachel

In Dashboards werden Kacheln über Ihre gespeicherten Protokollsuchen mit Informationen versorgt. OMS verfügt über viele vorgefertigte Protokollsuchvorgänge, damit Sie sofort beginnen können. Die folgende Abbildung mit Startinformationen wird angezeigt.

![Abbildung](./media/log-analytics-dashboards/oms-dashboards-pictorial.png)

Klicken Sie in der Ansicht „Mein Dashboard“ einfach unten auf der Seite auf das Zahnrad „Anpassen“, um den Anpassungsmodus zu aktivieren. Im Bereich, der rechts auf der Seite geöffnet wird, werden alle gespeicherten Protokollsuchen Ihres Arbeitsbereichs angezeigt.

![Hinzufügen von Kacheln 1](./media/log-analytics-dashboards/oms-dashboards-add-tile1.png)

Sie können eine gespeicherte Protokollsuche als Kachel visualisieren, indem Sie sie einfach links in den leeren Bereich ziehen. Beim Ziehen wird die Umwandlung in eine Kachel vollzogen.

![Hinzufügen von Kacheln 2](./media/log-analytics-dashboards/oms-dashboards-add-tile2.png)

![Hinzufügen von Kacheln 3](./media/log-analytics-dashboards/oms-dashboards-add-tile3.png)


## Bearbeiten einer Kachel

Klicken Sie in der Ansicht „Mein Dashboard“ einfach unten auf der Seite auf das Zahnrad „Anpassen“, um den Anpassungsmodus zu aktivieren. Klicken Sie auf die Kachel, die Sie bearbeiten möchten. Der rechte Bereich wechselt in den Bearbeitungsmodus, und es werden verschiedene Optionen angezeigt:

![Bearbeiten der Kachel](./media/log-analytics-dashboards/oms-dashboards-edit-tile.png)

### Kachelvisualisierungen#
Es gibt zwei Arten von Kachelvisualisierungen:

|Diagrammtyp|Funktionsweise|
|---|---|
|![Balkendiagramm](./media/log-analytics-dashboards/oms-dashboards-bar-chart.png)|Zeigt eine Zeitachse mit den Ergebnissen Ihrer gespeicherten Protokollsuchvorgänge oder eine Liste mit auf einem Feld basierenden Ergebnissen an. Dies richtet sich danach, ob bei Ihrer Protokollsuche Ergebnisse basierend auf einem Feld zusammengefasst werden.
|![Metrik](./media/log-analytics-dashboards/oms-dashboards-metric.png)|Zeigt die Gesamtanzahl der Ergebnistreffer für Ihre Protokollsuche als Zahl in einer Kachel an. Bei Kacheln vom Typ „Metrik“ können Sie einen Schwellenwert festlegen, bei dem die Kachel hervorgehoben wird, wenn der Schwellenwert erreicht ist.|

### Schwellenwert
Mit der Visualisierung „Metrik“ können Sie einen Schwellenwert für eine Kachel erstellen. Wählen Sie „Ein“, um auf der Kachel einen Schwellenwert zu erstellen. Geben Sie an, ob die Kachel hervorgehoben werden soll, wenn der Wert oberhalb oder unterhalb des gewählten Schwellenwerts liegt, und legen Sie darunter dann den Schwellenwert fest.

## Organisieren des Dashboards
Navigieren Sie zum Organisieren des Dashboards zur Ansicht „Mein Dashboard“, und klicken Sie unten auf der Seite auf das Zahnrand „Anpassen“, um den Anpassungsmodus zu aktivieren. Klicken Sie auf die Kachel, die Sie verschieben möchten, und ziehen Sie sie an die gewünschte Position.

![Organisieren des Dashboards](./media/log-analytics-dashboards/oms-dashboards-organize.png)

## Entfernen einer Kachel
Navigieren Sie zum Entfernen einer Kachel zur Ansicht „Mein Dashboard“, und klicken Sie unten auf der Seite auf das Zahnrand **Anpassen**, um den Anpassungsmodus zu aktivieren. Wählen Sie die Kachel aus, die Sie entfernen möchten, und wählen Sie im rechten Bereich die Option **Kachel entfernen**.

![Entfernen einer Kachel](./media/log-analytics-dashboards/oms-dashboards-remove-tile.png)

## Nächste Schritte

- Erstellen Sie Warnungen in Log Analytics, um Benachrichtigungen zu generieren und Probleme zu beheben.

<!---HONumber=AcomDC_0504_2016-->