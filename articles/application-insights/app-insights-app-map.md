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
ms.author: cfreeman
ms.translationtype: Human Translation
ms.sourcegitcommit: 71fea4a41b2e3a60f2f610609a14372e678b7ec4
ms.openlocfilehash: e76a31a8eaa249ab064d180bfd7ed158ef32c85a
ms.contentlocale: de-de
ms.lasthandoff: 05/10/2017


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

## <a name="error-pane"></a>Fehlerbereich
Wenn Sie auf einen Knoten in der Zuordnung klicken, wird ein Fehlerbereich auf der rechten Seite angezeigt, in dem die Fehler für diesen Knoten zusammengefasst sind. Fehler werden zunächst nach Vorgangs-ID und dann nach Problem-ID gruppiert.

![Fehlerbereich](./media/app-insights-app-map/error-pane.png)

Durch Klicken auf einen Fehler wird die letzte Instanz dieses Fehlers aufgerufen.

## <a name="resource-health"></a>Ressourcenintegrität
Für einige Ressourcentypen wird die Ressourcenintegrität oben im Fehlerbereich angezeigt. Beispiel: Wenn Sie auf einen SQL-Knoten klicken, werden die Datenbankintegrität und ausgelöste Warnungen angezeigt.

![Ressourcenintegrität](./media/app-insights-app-map/resource-health.png)

Sie können auf den Ressourcennamen klicken, um Standardübersichtsmetriken für diese Ressourcen anzuzeigen.

## <a name="end-to-end-system-app-maps"></a>Lückenlose System-App-Zuordnungen

*SDK-Version 2.3 oder höher erforderlich*

Wenn Ihre Anwendung mehrere Komponenten hat, z.B. zusätzlich zur Web-App einen Back-End-Dienst, können sie alle in einer integrierten App-Zuordnung anzeigen.

![Filter festlegen](./media/app-insights-app-map/multi-component-app-map.png)

Die App-Zuordnung ermittelt Serverknoten, indem sie mit dem Application Insights SDK HTTP-Abhängigkeitsaufrufe zwischen Servern verfolgt. Bei jeder Application Insights-Ressource wird davon ausgegangen, dass sie einen Server enthält.

### <a name="multi-role-app-map-preview"></a>App-Zuordnung mit mehreren Rollen (Vorschau)

Das Vorschaufeature für die App-Zuordnung mit mehreren Rollen ermöglicht es Ihnen, die App-Zuordnung mit mehreren Servern zu nutzen, die Daten an die gleiche Application Insights-Ressource bzw. den gleichen Instrumentierungsschlüssel senden. Server in der Zuordnung sind nach der Eigenschaft „cloud_RoleName“ für Telemetrieelemente segmentiert. Legen Sie auf dem Blatt „Previews“ (Vorschauen) für *Multi-role Application Map* (Anwendungszuordnung mit mehreren Rollen) die Option *Ein* fest, um diese Konfiguration zu aktivieren.

Dieser Ansatz ist unter Umständen in einer Microservices-Anwendung oder anderen Szenarien erwünscht, bei denen Sie Ereignisse über mehrere Server hinweg mit einer einzelnen Application Insights-Ressource korrelieren möchten.

## <a name="video"></a>Video

> [!VIDEO https://channel9.msdn.com/events/Connect/2016/112/player] 

## <a name="feedback"></a>Feedback
Senden Sie über die Feedbackoption im Portal Feedback.

![MapLink-1 (Abbildung)](./media/app-insights-app-map/13.png)


## <a name="next-steps"></a>Nächste Schritte

* [Azure-Portal](https://portal.azure.com)
