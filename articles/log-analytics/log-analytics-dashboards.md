---
title: Erstellen eines benutzerdefinierten Dashboards in Azure Log Analytics | Microsoft Docs
description: "In diesem Leitfaden wird beschrieben, wie in Log Analytics-Dashboards alle gespeicherten Protokollsuchvorgänge visualisiert werden können, um Ihnen eine zentrale Übersicht über Ihre Umgebung zu ermöglichen."
services: log-analytics
documentationcenter: 
author: bandersmsft
manager: carmonm
editor: 
ms.assetid: abb07f6c-b356-4f15-85f5-60e4415d0ba2
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/27/2017
ms.author: banders
ms.custom: H1Hack27Feb2017
translationtype: Human Translation
ms.sourcegitcommit: a0c8af30fbed064001c3fd393bf0440aa1cb2835
ms.openlocfilehash: a8c9766bf066a7f0dfd28ebb4e41bf0eaf3f05bd
ms.lasthandoff: 02/28/2017


---
# <a name="create-a-custom-dashboard-for-use-in-log-analytics"></a>Erstellen eines benutzerdefinierten Dashboards für die Verwendung in Log Analytics
In diesem Leitfaden wird beschrieben, wie in Log Analytics-Dashboards alle gespeicherten Protokollsuchvorgänge visualisiert werden können, um Ihnen eine zentrale Übersicht über Ihre Umgebung zu ermöglichen.

![Beispiel-Dashboard](./media/log-analytics-dashboards/oms-dashboards-example-dash.png)

Alle benutzerdefinierten Dashboards, die Sie im OMS-Portal erstellen, sind auch in der mobilen OMS-App verfügbar. Auf den folgenden Seiten erhalten Sie weitere Informationen zu den Apps.

* [Mobile OMS-App im Microsoft Store](http://www.windowsphone.com/store/app/operational-insights/4823b935-83ce-466c-82bb-bd0a3f58d865)
* [Mobile OMS-App bei Apple iTunes](https://itunes.apple.com/app/microsoft-operations-management/id1042424859?mt=8)

!["mobile dashboard" ("mobiles Dashboard")](./media/log-analytics-dashboards/oms-search-mobile.png)

## <a name="how-do-i-create-my-dashboard"></a>Wie erstelle ich mein Dashboard?
Navigieren Sie zu Beginn zur Übersichtsseite von OMS. Auf der linken Seite sehen Sie die Kachel **Mein Dashboard**. Klicken Sie darauf, um einen Drilldown in das Dashboard durchzuführen.

![Übersicht](./media/log-analytics-dashboards/oms-dashboards-overview.png)

## <a name="adding-a-tile"></a>Hinzufügen einer Kachel
In Dashboards werden Kacheln über Ihre gespeicherten Protokollsuchen mit Informationen versorgt. OMS verfügt über viele vorgefertigte Protokollsuchvorgänge, damit Sie sofort beginnen können. In den folgenden Schritten wird beschrieben, wie Sie beginnen.

Klicken Sie in der Ansicht „Mein Dashboard“ einfach auf **Anpassen**, um in den Anpassungsmodus zu wechseln.

![Abbildung](./media/log-analytics-dashboards/oms-dashboards-pictorial01.png)

 Im Bereich, der rechts auf der Seite geöffnet wird, werden alle gespeicherten Protokollsuchen Ihres Arbeitsbereichs angezeigt. Um eine gespeicherte Protokollsuche als Kachel zu visualisieren, zeigen Sie auf eine gespeicherte Suche, und klicken Sie dann auf das **Plussymbol**.

![Hinzufügen von Kacheln 1](./media/log-analytics-dashboards/oms-dashboards-pictorial02.png)

Wenn Sie auf das **Plussymbol** klicken, wird eine neue Kachel in der Ansicht „Mein Dashboard“ angezeigt.

![Hinzufügen von Kacheln 2](./media/log-analytics-dashboards/oms-dashboards-pictorial03.png)

## <a name="edit-a-tile"></a>Bearbeiten einer Kachel
Klicken Sie in der Ansicht „Mein Dashboard“ einfach auf **Anpassen**, um in den Anpassungsmodus zu wechseln. Klicken Sie auf die Kachel, die Sie bearbeiten möchten. Der rechte Bereich wechselt in den Bearbeitungsmodus, und es werden verschiedene Optionen angezeigt:

![Bearbeiten der Kachel](./media/log-analytics-dashboards/oms-dashboards-pictorial04.png)

![Bearbeiten der Kachel](./media/log-analytics-dashboards/oms-dashboards-pictorial05.png)

### <a name="tile-visualizations"></a>Kachelvisualisierungen
Es stehen drei Arten von Kachelvisualisierungen zur Auswahl:

| Diagrammtyp | Funktionsweise |
| --- | --- |
| ![Balkendiagramm](./media/log-analytics-dashboards/oms-dashboards-bar-chart.png) |Zeigt eine Zeitachse mit den Ergebnissen Ihrer gespeicherten Protokollsuchvorgänge als Balkendiagramm oder eine Liste mit auf einem Feld basierenden Ergebnissen an. Dies richtet sich danach, ob bei Ihrer Protokollsuche Ergebnisse basierend auf einem Feld zusammengefasst werden. |
| ![Metrik](./media/log-analytics-dashboards/oms-dashboards-metric.png) |Zeigt die Gesamtanzahl der Ergebnistreffer für Ihre Protokollsuche als Zahl in einer Kachel an. Bei Kacheln vom Typ „Metrik“ können Sie einen Schwellenwert festlegen, bei dem die Kachel hervorgehoben wird, wenn der Schwellenwert erreicht ist. |
| ![line](./media/log-analytics-dashboards/oms-dashboards-line.png) |Zeigt eine Zeitachse mit den Ergebnissen Ihrer gespeicherten Protokollsuchvorgänge mit Werten als Liniendiagramm an. |

### <a name="threshold"></a>Schwellenwert
Mit der Visualisierung „Metrik“ können Sie einen Schwellenwert für eine Kachel erstellen. Wählen Sie „Ein“, um auf der Kachel einen Schwellenwert zu erstellen. Geben Sie an, ob die Kachel hervorgehoben werden soll, wenn der Wert oberhalb oder unterhalb des gewählten Schwellenwerts liegt, und legen Sie darunter dann den Schwellenwert fest.

## <a name="organizing-the-dashboard"></a>Organisieren des Dashboards
Navigieren Sie zum Organisieren des Dashboards zur Ansicht „Mein Dashboard“, und klicken Sie auf **Anpassen**, um in den Anpassungsmodus zu wechseln. Klicken Sie auf die Kachel, die Sie verschieben möchten, und ziehen Sie sie an die gewünschte Position.

![Organisieren des Dashboards](./media/log-analytics-dashboards/oms-dashboards-organize.png)

## <a name="remove-a-tile"></a>Entfernen einer Kachel
Navigieren Sie zum Entfernen einer Kachel zur Ansicht „Mein Dashboard“, und klicken Sie auf **Anpassen**, um in den Anpassungsmodus zu wechseln. Wählen Sie die Kachel aus, die Sie entfernen möchten, und wählen Sie im rechten Bereich die Option **Kachel entfernen**.

![Entfernen einer Kachel](./media/log-analytics-dashboards/oms-dashboards-remove-tile.png)

## <a name="next-steps"></a>Nächste Schritte
* Erstellen Sie [Warnungen](log-analytics-alerts.md) in Log Analytics, um Benachrichtigungen zu generieren und Probleme zu beheben.

