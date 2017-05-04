---
title: "Intelligente Diagnose von Web-App-Leistungsänderungen in Azure Application Insights| Microsoft-Dokumentation"
description: Automatische Diagnose von Spitzen oder Schritten in der Leistungstelemetrie Ihrer Web-App
services: application-insights
documentationcenter: 
author: alancameronwills
manager: carmonm
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 04/16/2017
ms.author: awills
translationtype: Human Translation
ms.sourcegitcommit: db7cb109a0131beee9beae4958232e1ec5a1d730
ms.openlocfilehash: 74878cd8a5a7f1aa82281d3dc1118089f134bedc
ms.lasthandoff: 04/18/2017


---
# <a name="diagnose-sudden-changes-in-your-app-telemetry"></a>Diagnose von plötzlichen Änderungen in der App-Telemetrie

*Dieses Feature befindet sich in der Vorschau.*

Diagnostizieren Sie plötzliche Änderungen der Web-App-Leistung oder -Nutzung mit einem einzigen Mausklick! Die intelligente Diagnosefunktion ist immer verfügbar, wenn Sie in [Analytics](app-insights-analytics.md) in [Application Insights](app-insights-overview.md) ein Zeitdiagramm erstellen. Bei jeder ungewöhnlichen Abweichung vom Trend der Ergebnisse, z.B. einer Spitze oder einer Abnahme, identifiziert die intelligente Diagnose ein Muster aus Dimensionen und verwandten Werten, mit denen die Änderung möglicherweise erklärt werden kann. Auf diese Weise können Sie das Problem schnell diagnostizieren. 

In diesem Beispiel hat die intelligente Diagnose ein Muster von Eigenschaftswerten erkannt, die mit der Änderung zusammenhängen, und hebt die Unterschiede zwischen den Ergebnissen mit diesem Muster und ohne dieses Muster hervor:

![Beispiel für ein Analyse-Diagnoseergebnis](./media/app-insights-analytics-diagnostics/analytics-result.png)
 

## <a name="diagnose-data-changes"></a>Diagnostizieren von Datenänderungen

1.    Führen Sie eine Abfrage in Analytics aus, und rendern Sie sie als Zeitdiagramm. 
2.    Klicken Sie auf einen beliebigen hervorgehobenen Spitzenpunkt (sofern vorhanden).
 
    ![Spitzenpunkt](./media/app-insights-analytics-diagnostics/peak.png)

    Die Diagnose braucht einige Sekunden, um ein Muster zu erkennen.

3. Auf der Registerkarte „Diagnoseergebnisse“ wird ein Muster angezeigt, das die Diskontinuität der Daten erklären könnte.

    ![result](./media/app-insights-analytics-diagnostics/result.png)
 
    Der Text zeigt die Dimensionswerte, die offenbar mit der Veränderung korrelieren. In diesem Beispiel hängt dies mit einer bestimmten Anforderung und einer bestimmten Browserversion zusammen.

    Beachten Sie auch die beiden Komponenten des Diagramms mit den Filtern „true“ und „false“. Die Komponente „false“ zeigt einen unveränderten Trend. Anders ausgedrückt, es liegt keine Änderung in den Telemetrieergebnissen vor, wenn die von der Diagnose als problematisch erkannte Kombination von Dimensionen ausgeschlossen wird. Im Gegensatz dazu zeigen die Ergebnisse innerhalb dieser Kombination eine erhebliche Änderung im hervorgehobenen Bereich der Untersuchung. Dies zeigt, dass die Diagnose eine Kombination von Eigenschaften gefunden hat, die die Änderung erklärt.

4.    Bei einem komplexen Muster müssen Sie mit dem Mauszeiger auf **Alle anzeigen** zeigen, um die Dimensionen einzublenden.

    ![Alle anzeigen](./media/app-insights-analytics-diagnostics/show-all.png)
 
5.    Falls die Diagnose kein signifikantes Muster findet, wird die Seite „Keine Ergebnisse“ angezeigt. An diesem Punkt sollten Sie die Abfrage unter Umständen ändern. Für eine weitere Analyse und möglicherweise bessere Ergebnisse könnten Sie beispielsweise den Zeitbereich und die Klassifizierung in der Analytics-Abfrage einschränken.

Da Sie nun wissen, dass eine spezifische Seite Ihrer Website zu Problemen in einem bestimmten Browser führt, können Sie direkt zu dieser problematischen Seite wechseln und die letzten Änderungen untersuchen.

## <a name="try-the-demo"></a>Testen Sie die Demo

[Klicken Sie hier zum Anzeigen einer Demo](https://analytics.applicationinsights.io/demo?q=H4sIAAAAAAAAA3VSTY%2FTQAy991dYPXWlLf0QIO2KIiGWA3duiMPsxEnMzhe2p6WIH48nVUsuGylRNPOe3%2FOzN5vFZgPfRhL4VZHPIGM%2BCdgHdESgpMjOKx0RnsgNKYuSF%2BjRaWUE7xKMGIoBgTpMSv2Z0jBxOWc1QBWEPjM4EMUCP2uc0A3x8E5HKMi%2BEQNC7oHRbIgKdJWdUk5vmr9PvdkArildit%2Fcrk0lBDjnyhBzk%2FKVxdTy0QhNY6RhDPYqdlCy9XMV96NjBZc68IH8y6Tzuf01iZxeIZ%2FI5DqMOYmaQQRXNUdz6qGb5WOdSKEXnOozHtEFK%2Bh0qnq5YQzGF9DcoinoqbcigkO0NOZRNGOZaaBkMuat5xznFOtULKhG%2BdrGlVDhy%2B8SMlsETV8dD6gTd0YrbsBrFq6U1v%2Filv4C%2FsJpRJuwUrQTZ0P7eIDOHLeD1X67e7%2Fe7dbbB9htH%2Ffbu4vQDfvhFez%2B8a1h%2F1f3VSy%2BJ4Ol1oN8X4qN0qMZWv44HJanzKFLeJIltKcRpcbomP7gbHNkdV2Xe1uqO3g%2BwzOl1c3PvbmMlC7KjKlry2GX0w4s%2FgFoo5%2BhBAMAAA%3D%3D&timespan=PT24H) zu Beispieldaten.

## <a name="how-it-works"></a>So funktioniert's

Die intelligente Diagnose verwendet einen erweiterten, nicht überwachten Machine Learning-Algorithmus auf Basis des Vorgangs [DiffPatterns](app-insights-analytics-reference.md#evaluate-diffpatterns). Sie sucht nach möglichen Mustern, die die Datenänderung erklären könnten. Analysiert werden die Auswirkungen der einzelnen Kandidaten auf die Metrik, und das Muster, das am besten mit der Änderung korreliert, wird angezeigt.

## <a name="no-diagnostic-points"></a>Keine Diagnosepunkte?

Die intelligente Diagnose funktioniert nur, wenn die folgenden Kriterien erfüllt sind:

 * Die intelligente Diagnose ist aktiviert. Überprüfen Sie dies unter dem Symbol „Einstellungen“ in Analytics.
 * Die Option „Intelligente Diagnose“ ist in den Analytics-Einstellungen ausgewählt. 
 * Zeitachse: Die X-Achse des Diagramms muss vom Typ `datetime` sein.
 * Linien- oder Bereichsdiagramm: Die Diagnose funktioniert nur mit diesen Diagrammtypen. Verwenden Sie `| render timechart` oder `| render areachart` am Ende der Abfrage, oder wählen Sie in der Dropdownauswahl das Linien- oder Bereichsdiagramm aus.
 * Diskontinuität: Es muss eine deutliche Diskontinuität in den Daten vorliegen.
 * Es sind genügend Punkte für eine Analyse vorhanden.
 * Die Abfrage enthält höchstens eine Zusammenfassungsklausel.
 * Es ist keine Projektklausel vorhanden, die eine Namensdefinition vor der Zusammenfassungsklausel enthält.

 
 ## <a name="related-articles"></a>Verwandte Artikel

 * [Analytics-Tutorial](app-insights-analytics-tour.md)
 * Die [intelligente Erkennung](app-insights-proactive-diagnostics.md) benachrichtigt Sie automatisch über Leistungsprobleme.
S
