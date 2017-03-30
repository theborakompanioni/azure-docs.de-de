---
title: Anwendungszuordnung in Azure Application Insights | Microsoft-Dokumentation
description: "Eine visuelle Darstellung der Abhängigkeiten zwischen App-Komponenten mit KPIs und Warnungen."
services: application-insights
documentationcenter: 
author: SoubhagyaDash
manager: carmonm
ms.assetid: 3bf37fe9-70d7-4229-98d6-4f624d256c36
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 03/14/2017
ms.author: awills
translationtype: Human Translation
ms.sourcegitcommit: fd35f1774ffda3d3751a6fa4b6e17f2132274916
ms.openlocfilehash: 9fc7a41dcca3d9e51b8c67b86ef61443945b6bad
ms.lasthandoff: 03/16/2017


---
# <a name="application-map-in-application-insights"></a>Anwendungszuordnung in Application Insights
Bei der Anwendungszuordnung in [Azure Application Insights](app-insights-overview.md) handelt es sich um ein visuelles Layout der Abhängigkeitsbeziehungen Ihrer Anwendungskomponenten. Jede Komponente zeigt KPIs wie etwa Last, Leistung, Fehler und Warnungen, damit Sie Komponenten ermitteln können, die Leistungsprobleme oder Fehler verursachen. Für jede Komponente können Sie detailliertere Diagnosen anzeigen, z.B. für Application Insights-Ereignisse. Wenn Ihre App Azure-Dienste nutzt, können Sie auch Azure-Diagnosedaten anzeigen, z.B. SQL Database Advisor-Empfehlungen.

Wie bei anderen Diagrammen können Sie eine Anwendungszuordnung an das Azure-Dashboard anheften und alle ihre Funktionen nutzen. 

## <a name="open-the-application-map"></a>Öffnen der Anwendungszuordnung
Öffnen Sie die Zuordnung auf dem Blatt „Übersicht“ für Ihre Anwendung:

![App-Zuordnung öffnen](./media/app-insights-app-map/01.png)

![App-Zuordnung](./media/app-insights-app-map/02.png)

Die Zuordnung zeigt Folgendes:

* Verfügbarkeitstests
* Clientseitige Komponente (mit dem JavaScript SDK überwacht)
* Serverseitige Komponente
* Abhängigkeiten von den Client- und Serverkomponenten

Sie können Abhängigkeitslinkgruppen erweitern oder reduzieren:

![Reduzieren](./media/app-insights-app-map/03.png)

Wenn zahlreiche Abhängigkeiten eines Typs (SQL, HTTP usw.) vorhanden sind, werden sie ggf. gruppiert angezeigt. 

![Gruppierte Abhängigkeiten](./media/app-insights-app-map/03-2.png)

## <a name="spot-problems"></a>Erkennen von Problemen
Jeder Knoten verfügt über relevante Leistungsindikatoren wie Last, Leistung und Fehlerraten für die jeweilige Komponente. 

Warnsymbole weisen auf mögliche Probleme hin. Eine orangefarbene Warnung bedeutet, dass Fehler bei Anforderungen, Seitenaufrufen oder Abhängigkeitsaufrufen vorliegen. Rot gibt eine Fehlerquote von über 5 Prozent an. Wenn Sie diese Schwellenwerte anpassen möchten, öffnen Sie die Optionen.

![Fehlersymbole](./media/app-insights-app-map/04.png)

Aktive Warnungen werden ebenfalls angezeigt: 

![Aktive Warnungen](./media/app-insights-app-map/05.png)

Bei der Verwendung von SQL Azure wird mithilfe eines Symbols darauf hingewiesen, dass Empfehlungen zur Leistungssteigerung vorhanden sind. 

![Azure-Empfehlung](./media/app-insights-app-map/06.png)

Klicken Sie auf ein beliebiges Symbol, um weitere Details anzuzeigen:

![Azure-Empfehlung](./media/app-insights-app-map/07.png)

## <a name="diagnostic-click-through"></a>Anzeigen detaillierter Diagnoseinformationen
Für alle Knoten in der Zuordnung können detaillierte Diagnoseinformationen angezeigt werden. Die Optionen variieren abhängig vom Knotentyp.

![Serveroptionen](./media/app-insights-app-map/09.png)

Für in Azure gehostete Komponenten beinhalten die Optionen direkte Links zu den Komponenten.

## <a name="filters-and-time-range"></a>Filter und Zeitraum
In der Zuordnung werden standardmäßig alle Daten zusammengefasst, die für den ausgewählten Zeitraum zur Verfügung stehen. Sie können sie jedoch nach bestimmten Vorgangsnamen oder Abhängigkeiten filtern.

* Vorgangsname: Dazu gehören Seitenaufrufe und serverseitige Anforderungen. Mit dieser Option zeigt die Zuordnung den KPI für den server-/clientseitigen Knoten nur für die ausgewählten Vorgänge an. Die im Kontext dieser spezifischen Vorgänge aufgerufenen Abhängigkeiten werden angezeigt.
* Basisname der Abhängigkeit: Dazu zählen die browserseitigen AJAX-Abhängigkeiten und die serverseitigen Abhängigkeiten. Wenn Sie benutzerdefinierte Abhängigkeitstelemetriedaten mit der TrackDependency-API erfassen, werden diese hier ebenfalls angezeigt. Sie können die Abhängigkeiten auswählen, die in der Zuordnung angezeigt werden sollen. Derzeit werden die serverseitigen Anforderungen oder die clientseitigen Seitenaufrufe nicht gefiltert.

![Filter festlegen](./media/app-insights-app-map/11.png)

## <a name="save-filters"></a>Speichern von Filtern
Heften Sie zum Speichern der angewendeten Filter die gefilterte Ansicht an ein [Dashboard](app-insights-dashboards.md)an.

![An Dashboard anheften](./media/app-insights-app-map/12.png)

## <a name="end-to-end-system-app-maps"></a>Lückenlose System-App-Zuordnungen

Wenn Ihre Anwendung mehrere Komponenten hat, z.B. zusätzlich zur Web-App einen Back-End-Dienst, können sie alle in einer integrierten App-Zuordnung anzeigen.

![Filter festlegen](./media/app-insights-app-map/multi-component-app-map.png)

Die App-Zuordnung findet Serverknoten, indem alle Application Insights-Ressourcen innerhalb der aktuellen Ressourcengruppe gesucht werden. Sie erkennt auch Serverknoten durch Folgen aller Abhängigkeitsaufrufe, die von Application Insights-Ressourcen in der aktuellen Ressourcengruppe nachverfolgt werden.


### <a name="setting-up"></a>Einrichten

> [!NOTE] 
> Lückenlose System-App-Zuordnungen sind in der Vorschauphase. Sie müssen Ihre Komponenten mit einer speziellen Version des SDK instrumentieren und eine besondere URL nutzen, um die App-Zuordnung anzuzeigen. [Erfahren Sie, wie Sie lückenlose System-App-Zuordnungen einrichten](https://github.com/Microsoft/ApplicationInsights-Home/blob/master/app-insights-app-map-preview.md).

## <a name="video"></a>Video

> [!VIDEO https://channel9.msdn.com/events/Connect/2016/112/player] 

## <a name="feedback"></a>Feedback
[Senden Sie über die Feedbackoption im Portal Feedback](app-insights-get-dev-support.md).

![MapLink-1 (Abbildung)](./media/app-insights-app-map/13.png)


## <a name="next-steps"></a>Nächste Schritte

* [Azure-Portal](https://portal.azure.com)
