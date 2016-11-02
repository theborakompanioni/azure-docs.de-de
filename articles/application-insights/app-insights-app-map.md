<properties 
	pageTitle="Anwendungszuordnung in Application Insights | Microsoft Azure" 
	description="Eine visuelle Darstellung der Abhängigkeiten zwischen App-Komponenten mit KPIs und Warnungen." 
	services="application-insights" 
    documentationCenter=""
	authors="SoubhagyaDash" 
	manager="douge"/>

<tags 
	ms.service="application-insights" 
	ms.workload="tbd" 
	ms.tgt_pltfrm="ibiza" 
	ms.devlang="na" 
	ms.topic="article" 
	ms.date="06/15/2016" 
	ms.author="awills"/>
 
# Anwendungszuordnung in Application Insights

Bei der Anwendungszuordnung in [Visual Studio Application Insights](app-insights-overview.md) handelt es sich um ein visuelles Layout der Abhängigkeitsbeziehungen Ihrer Anwendungskomponenten. Jede Komponente zeigt KPIs wie etwa Last, Leistung, Fehler und Warnungen, damit Sie Komponenten ermitteln können, die Leistungsprobleme oder Fehler verursachen. Sie können für jede Komponente ausführlichere Diagnoseinformationen anzeigen. Die Daten können aus Application Insights und (sofern Ihre App Azure-Dienste verwendet) aus der Azure-Diagnose stammen – wie etwa Empfehlungen des SQL-Datenbank-Ratgebers.

Wie bei anderen Diagrammen können Sie eine Anwendungszuordnung an das Azure-Dashboard anheften und alle ihre Funktionen nutzen.

## Öffnen der Anwendungszuordnung

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
 
Wenn zahlreiche Abhängigkeiten eines Typs (SQL, HTTP usw.) vorhanden sind, werden sie unter Umständen gruppiert angezeigt.


![Gruppierte Abhängigkeiten](./media/app-insights-app-map/03-2.png)
 
 
## Erkennen von Problemen

Jeder Knoten verfügt über relevante Leistungsindikatoren wie Last, Leistung und Fehlerraten für die jeweilige Komponente.

Warnsymbole weisen auf mögliche Probleme hin. Eine orangefarbene Warnung bedeutet, dass Fehler bei Anforderungen, Seitenaufrufen oder Abhängigkeitsaufrufen vorliegen. Rot gibt eine Fehlerquote von über 5 Prozent an.


![Fehlersymbole](./media/app-insights-app-map/04.png)

 
Aktive Warnungen werden ebenfalls angezeigt:


![Aktive Warnungen](./media/app-insights-app-map/05.png)
 
Bei der Verwendung von SQL Azure wird mithilfe eines Symbols darauf hingewiesen, dass Empfehlungen zur Leistungssteigerung vorhanden sind.


![Azure-Empfehlung](./media/app-insights-app-map/06.png)

Klicken Sie auf ein beliebiges Symbol, um weitere Details anzuzeigen:


![Azure-Empfehlung](./media/app-insights-app-map/07.png)
 
 
## Anzeigen detaillierter Diagnoseinformationen

Für alle Knoten in der Zuordnung können detaillierte Diagnoseinformationen angezeigt werden. Die Optionen variieren abhängig vom Knotentyp.

![Serveroptionen](./media/app-insights-app-map/09.png)

 
Für in Azure gehostete Komponenten beinhalten die Optionen direkte Links zu den Komponenten.


## Filter und Zeitraum

In der Zuordnung werden standardmäßig alle Daten zusammengefasst, die für den ausgewählten Zeitraum zur Verfügung stehen. Sie können sie jedoch nach bestimmten Vorgangsnamen oder Abhängigkeiten filtern.

* Vorgangsname: Dazu gehören Seitenaufrufe und serverseitige Anforderungen. Mit dieser Option zeigt die Zuordnung den KPI auf dem server-/clientseitigen Knoten nur für die ausgewählten Vorgänge an. Die im Kontext dieser spezifischen Vorgänge aufgerufenen Abhängigkeiten werden angezeigt.
* Basisname der Abhängigkeit: Dazu zählen die browserseitigen AJAX-Abhängigkeiten und die serverseitigen Abhängigkeiten. Wenn Sie benutzerdefinierte Abhängigkeitstelemetriedaten mit der TrackDependency-API erfassen, werden diese hier ebenfalls angezeigt. Sie können die Abhängigkeiten auswählen, die in der Zuordnung angezeigt werden sollen. Beachten Sie, dass zu diesem Zeitpunkt die serverseitigen Anforderungen oder die clientseitigen Seitenaufrufe nicht gefiltert werden.


![Filter festlegen](./media/app-insights-app-map/11.png)

 
 
## Speichern von Filtern

Heften Sie zum Speichern der angewendeten Filter die gefilterte Ansicht an ein [Dashboard](app-insights-dashboards.md) an.


![An das Dashboard anheften](./media/app-insights-app-map/12.png)
 


## Feedback

[Senden Sie über die Feedbackoption im Portal Feedback](app-insights-get-dev-support.md).


![MapLink-1 (Abbildung)](./media/app-insights-app-map/13.png)

<!---HONumber=AcomDC_0622_2016-->