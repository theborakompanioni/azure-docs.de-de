---
title: Exemplarische Vorgehensweise zur vorbeugenden Wartung | Microsoft Docs
description: "Exemplarische Vorgehensweise zur vorkonfigurierten Azure IoT-Lösung für vorbeugende Wartung | Microsoft Azure"
services: 
suite: iot-suite
documentationcenter: 
author: dominicbetts
manager: timlt
editor: 
ms.assetid: 3c48a716-b805-4c99-8177-414cc4bec3de
ms.service: iot-suite
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/01/2017
ms.author: dobett
translationtype: Human Translation
ms.sourcegitcommit: 719f9810abb12cbe8645651d35cbff936cecd158
ms.openlocfilehash: 57531b609b095359c8bf0ea3685af40124f46311


---
# <a name="predictive-maintenance-preconfigured-solution-walkthrough"></a>Exemplarische Vorgehensweise zur vorkonfigurierten Lösung für vorbeugende Wartung

## <a name="introduction"></a>Einführung

Die vorkonfigurierte IoT Suite-Lösung für vorbeugende Wartung ist eine End-to-End-Lösung für ein Geschäftsszenario, bei dem der Zeitpunkt vorhergesagt wird, zu dem voraussichtlich ein Fehler auftritt. Sie können diese vorkonfigurierte Lösung proaktiv für Aktivitäten nutzen, z.B. zum Optimieren der Wartung. Bei dieser Lösung werden zentrale Azure IoT Suite-Dienste wie IoT Hub, Stream Analytics und ein [Azure Machine Learning][lnk-machine-learning]-Arbeitsbereich kombiniert. Der Arbeitsbereich enthält ein Modell zum Vorhersagen der Restlebensdauer (Remaining Useful Life, RUL) eines Flugzeugtriebwerks auf der Grundlage eines öffentlichen Datasets mit Beispielwerten. Bei der Lösung wird das IoT-Geschäftsszenario vollständig als Ausgangspunkt implementiert, damit Sie eine Lösung planen und implementieren können, die Ihre besonderen Geschäftsanforderungen erfüllt.

## <a name="logical-architecture"></a>Logische Architektur

Das folgende Diagramm beschreibt die logischen Komponenten der vorkonfigurierten Lösung:

![][img-architecture]

Die blauen Elemente sind Azure-Dienste, die in der Region bereitgestellt werden, die Sie beim Bereitstellen der vorkonfigurierten Lösung auswählen. Die Liste mit den Regionen, in denen Sie die vorkonfigurierte Lösung bereitstellen können, wird auf der [Bereitstellungsseite][lnk-azureiotsuite] angezeigt.

Das grüne Element ist ein simuliertes Gerät, das für ein Flugzeugtriebwerk steht. Der folgende Abschnitt enthält weitere Informationen zu diesen simulierten Geräten.

Die grauen Elemente stehen für Komponenten, mit denen Funktionen für die *Geräteverwaltung* implementiert werden. Von der aktuellen Version der vorkonfigurierten Lösung für vorbeugende Wartung werden diese Ressourcen nicht bereitgestellt. Weitere Informationen zur Geräteverwaltung finden Sie in der [vorkonfigurierten Lösung für die Remoteüberwachung][lnk-remote-monitoring].

## <a name="simulated-devices"></a>Simulierte Geräte

In der vorkonfigurierten Lösung steht ein simuliertes Gerät für ein Flugzeugtriebwerk. Die Lösung wird mit zwei Triebwerken bereitgestellt, die einem Flugzeug zugeordnet sind. Jedes Triebwerk gibt vier Arten von Telemetriedaten aus: Sensor 9, Sensor 11, Sensor 14 und Sensor 15. Auf der Grundlage dieser Daten berechnet das Machine Learning-Modell die Restlebensdauer des Triebwerks. Jedes simulierte Gerät sendet die folgenden Telemetriemeldungen an IoT Hub:

*Zyklusanzahl*. Ein Zyklus steht für einen absolvierten Flug mit einer Dauer von zwei bis zehn Stunden. Während des Flugs werden jede halbe Stunde Telemetriedaten erfasst.

*Telemetrie*. Es sind vier Sensoren vorhanden, die für die Triebwerkattribute stehen. Die Sensoren tragen die generischen Bezeichnungen Sensor 9, Sensor 11, Sensor 14 und Sensor 15. Diese vier Sensoren liefern genügend Telemetriedaten, um auf der Grundlage des Restlebensdauer-Modells sinnvolle Ergebnisse zu erhalten. Das in der vorkonfigurierten Lösung verwendete Modell basiert auf einem öffentlichen Dataset mit echten Daten von Triebwerksensoren. Weitere Informationen dazu, wie das Modell aus dem ursprünglichen Dataset erstellt wurde, finden Sie unter [Cortana Intelligence-Katalog: Vorlage für die vorbeugende Wartung][lnk-cortana-analytics].

Außerdem können die simulierten Geräte auch die folgenden Befehle verarbeiten, die vom IoT-Hub in der Lösung gesendet werden:

| Befehl | Beschreibung |
| --- | --- |
| StartTelemetry |Steuert den Status der Simulation.<br/>Startet das Senden der Telemetriedaten vom Gerät. |
| StopTelemetry |Steuert den Status der Simulation.<br/>Beendet das Senden der Telemetriedaten vom Gerät. |

IoT Hub führt die Bestätigung der Gerätebefehle durch.

## <a name="azure-stream-analytics-job"></a>Azure Stream Analytics-Auftrag
**Auftrag: Telemetrie** verarbeitet den eingehenden Datenstrom mit Gerätetelemetriedaten mit zwei Anweisungen. Mit der ersten Anweisung werden alle Telemetriedaten von den Geräten ausgewählt und an eine Blob Storage-Einheit gesendet, von wo aus sie in der Web-App visualisiert werden. Mit der zweiten Anweisung werden die durchschnittlichen Sensorwerte für ein gleitendes Fenster von zwei Minuten berechnet und über den Event Hub an einen **Ereignisprozessor**gesendet.

## <a name="event-processor"></a>Ereignisprozessor
Der **Ereignisprozessorhost** wird im Rahmen eines Azure-Webauftrags ausgeführt. Der **Ereignisprozessor** verwendet die durchschnittlichen Sensorwerte eines abgeschlossenen Zyklus. Diese Werte übergibt er anschließend an eine API, die das trainierte Modell zum Berechnen der Restlebensdauer eines Triebwerks verfügbar macht. Die API wird von einem Machine Learning-Arbeitsbereich verfügbar gemacht, der als Teil der Lösung bereitgestellt wird.

## <a name="machine-learning"></a>Machine Learning
Die Machine Learning-Komponente verwendet ein Modell, das sich von Daten ableitet, die von echten Flugzeugtriebwerken gesammelt wurden. Sie können über die Kachel auf der Seite [azureiotsuite.com][lnk-azureiotsuite] Ihrer bereitgestellten Lösung zum Machine Learning-Arbeitsbereich navigieren, wenn der Status **Bereit** lautet.


## <a name="next-steps"></a>Nächste Schritte
Nachdem Sie nun die zentralen Komponenten der vorkonfigurierten Lösung für vorbeugende Wartung kennen, können Sie die Lösung anpassen. Weitere Informationen finden Sie unter [Anpassen vorkonfigurierter Lösungen][lnk-customize].

Sie können auch einige andere Features und Funktionen der vorkonfigurierten IoT Suite-Lösungen ausprobieren:

* [Häufig gestellte Fragen zu IoT Suite][lnk-faq]
* [Sicherheit im Internet der Dinge von Anfang an][lnk-security-groundup]

[img-architecture]: media/iot-suite-predictive-walkthrough/architecture.png

[lnk-remote-monitoring]: iot-suite-remote-monitoring-sample-walkthrough.md
[lnk-cortana-analytics]: http://gallery.cortanaintelligence.com/Collection/Predictive-Maintenance-Template-3
[lnk-azureiotsuite]: https://www.azureiotsuite.com/
[lnk-customize]: iot-suite-guidance-on-customizing-preconfigured-solutions.md
[lnk-faq]: iot-suite-faq.md
[lnk-security-groundup]: securing-iot-ground-up.md
[lnk-machine-learning]: https://azure.microsoft.com/services/machine-learning/


<!--HONumber=Feb17_HO1-->


