---
title: "Übersicht über die vorkonfigurierte Lösung für vorhersagbaren Wartungsbedarf | Microsoft Docs"
description: "Beschreibung der vorkonfigurierten Azure IoT Suite-Lösung für Predictive Maintenance"
services: 
suite: iot-suite
documentationcenter: 
author: dominicbetts
manager: timlt
editor: 
ms.assetid: b370b3d7-2ce5-4906-9818-3aeedd471ee3
ms.service: iot-suite
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/01/2017
ms.author: dobett
translationtype: Human Translation
ms.sourcegitcommit: caecd0d6e546d4ddef71ebeee6fad3a5c7ba6451
ms.openlocfilehash: 5447dff1ff9aea1b90f7e18499f795e7c173e2b9


---
# <a name="predictive-maintenance-preconfigured-solution-overview"></a>Übersicht über die vorkonfigurierte Lösung für vorhersagbaren Wartungsbedarf

Die [vorkonfigurierte Lösung][lnk_preconfigured_solutions] für *Predictive Maintenance* ist eine der vorkonfigurierten Lösungen von [Microsoft Azure IoT Suite][lnk_iot_suite]. Diese Lösung integriert die Erfassung von Gerätetelemetrie in Echtzeit mit einem Vorhersagemodell, das [Azure Machine Learning][lnk-machine-learning] verwendet.

Mit Azure IoT Suite kann ein Unternehmen schnell und einfach Verbindungen mit Objekten herstellen, diese überwachen und Daten in Echtzeit analysieren. Die vorkonfigurierte Lösung für Predictive Maintenance nutzt diese Daten und verwendet umfangreiche Dashboards und Visualisierungen, um Ihnen neue Einsichten zu bieten, die zur Effizienzsteigerung und zum Ausbau von Einnahmequellen dienen können.

## <a name="the-scenario"></a>Das Szenario
Fabrikam ist eine regionale Fluggesellschaft, deren Schwerpunkt auf hervorragender Kundenzufriedenheit zu wettbewerbsfähigen Preisen liegt. Eine Ursache von Flugverspätungen sind Wartungsprobleme, und insbesondere die Wartung von Flugzeugtriebwerken ist anspruchsvoll. Ein Triebwerksausfall während des Flugs muss unter allen Umständen vermieden werden. Daher setzt Fabrikam auf eine regelmäßige Inspektion und ein Programm zur planmäßigen Wartung. Allerdings nutzen sich nicht alle Flugzeugtriebwerke in gleicher Weise ab. Ein gewisser Anteil der Wartung wird ausgeführt, obwohl er nicht erforderlich ist. Wichtiger noch, es können Umstände auftreten, die ein Flugzeug am Boden festhalten, bis eine Wartung durchgeführt wird. Dieses Problem führt zu teuren Verzögerungen, insbesondere dann, wenn sich das Flugzeug an einem Standort befindet, an dem die benötigten Techniker oder Ersatzteile nicht verfügbar sind.

Die Triebwerke der Fabrikam-Flotte sind mit Sensoren ausgestattet, die den Triebwerkszustand während des Flugs erfassen. Fabrikam verwendet die vorkonfigurierte Lösung für Predictive Maintenance zum Erfassen der Sensordaten während des Flugs. Nach jahrelanger Erfassung der Betriebs- und Fehlerdaten haben die Datenanalysten von Fabrikam ein Modell zur Vorhersage der Restnutzungsdauer (Remaining Useful Life, RUL) eines Flugzeugtriebwerks erstellt. Dieses Modell beruht auf einer gefundenen Korrelation zwischen Daten von vier der Triebwerksensoren und dem Triebwerkverschleiß, der letztlich zum Ausfall führen kann. Obwohl Fabrikam die reguläre Inspektion aus Sicherheitsgründen fortsetzt, kann das Unternehmen jetzt anhand der Modelle nach jedem Flug die Restnutzungsdauer für jedes Triebwerk berechnen. Das Modell verwendet die während des Flugs von den Triebwerken erfassten Telementriedaten. Fabrikam kann jetzt Vorhersagen zu künftigen Fehlerpunkten treffen, und Wartung und Reparatur im Voraus planen.

> [!NOTE]
> Das Lösungsmodell verwendet die Daten zur tatsächlichen Maschinennutzung.

Durch die Vorhersage des geeigneten Wartungszeitpunkts kann Fabrikam eine Optimierung durchführen und die Betriebskosten senken. Die Wartungskoordinatoren arbeiten mit Schedulern:

- Um die Wartungsarbeiten so zu planen, dass sie zeitlich mit dem Eintreffen des Flugzeugs an einem bestimmten Standort übereinstimmen
- Um sicherzustellen, dass für die Außerbetriebnahme des Flugzeugs genügend Zeit zur Verfügung steht, ohne dass der Flugplan dadurch beeinträchtigt wird
- Um den Einsatz von Technikern so zu planen, dass die Flugzeuge effizient und ohne Leerlauf gewartet werden

Bestandskontrollmanager erhalten Wartungspläne, damit Sie den Bestellvorgang und den Ersatzteilbestand optimieren können. All diese Faktoren ermöglichen es Fabrikam, die Bodenzeiten der Flugzeuge zu minimieren und die Betriebskosten bei unveränderter Sicherheit für Passagiere und Crew zu senken.

Informationen dazu, wie [Azure IoT Suite][lnk_iot_suite] die vom Kunden benötigten Fähigkeiten bereitstellt, um das Potenzial von Predictive Maintenance zu erschließen, finden Sie in dieser [Infografik][lnk_infographic].

## <a name="how-the-predictive-maintenance-solution-is-built"></a>Aufbau der Lösung für vorhersagbaren Wartungsbedarf

Die Lösung nutzt ein vorhandenes Azure Machine Learning-Modell, das als Vorlage zur Verfügung steht, um die Anwendung dieser Funktionen auf Gerätetelemetriedaten zu veranschaulichen, die über IoT Suite-Dienste erfasst wurden. Microsoft hat anhand öffentlich zugänglicher Daten <sup>\[1\]</sup> ein [Regressionsmodell][lnk_regression_model] eines Flugzeugtriebwerks erstellt und eine schrittweise Anleitung zur Verwendung des Modells veröffentlicht.

Die Azure IoT-Lösung für Predictive Maintenance verwendet das mit dieser Vorlage erstellte Regressionsmodell. Das Modell wird in Ihrem Azure-Abonnement bereitgestellt und über eine automatisch erstellte API verfügbar gemacht. Die Lösung umfasst eine Teilmenge der Testdaten mit vier (von insgesamt 100) Triebwerken und vier (von insgesamt 21) Sensordatenströmen. Diese Daten sind ausreichend, um ein genaues Ergebnis aus dem trainierten Modell bereitzustellen.

*\[1\] A. Saxena und K. Goebel (2008). „Turbofan Engine Degradation Simulation Data Set“, NASA Ames Prognostics Data Repository (http://ti.arc.nasa.gov/tech/dash/pcoe/prognostic-data-repository/), NASA Ames Research Center, Moffett Field, CA*

## <a name="get-started-with-predictive-maintenance"></a>Erste Schritte mit Predictive Maintenance

In diesem Tutorial erfahren Sie, wie Sie die Lösung für Predictive Maintenance bereitstellen. Außerdem lernen Sie die grundlegenden Funktionen der Lösung für Predictive Maintenance kennen. Sie können auf einen Großteil dieser Funktionen über das Lösungsdashboard zugreifen, das zusammen mit der vorkonfigurierten Lösung bereitgestellt wird.

Um dieses Tutorial abzuschließen, benötigen Sie ein aktives Azure-Abonnement.

> [!NOTE]
> Wenn Sie über kein Konto verfügen, können Sie in nur wenigen Minuten ein kostenloses Testkonto erstellen. Ausführliche Informationen finden Sie unter [Kostenlose Azure-Testversion][lnk_free_trial].

1. Melden Sie sich mit den Anmeldeinformationen für Ihr Azure-Konto bei [azureiotsuite.com][lnk-azureiotsuite] an, und klicken Sie auf **+**, um eine Lösung zu erstellen.
1. Klicken Sie auf der Kachel **Predictive maintenance** auf **Auswählen**.
1. Geben Sie einen **Lösungsnamen** für Ihre vorkonfigurierte Lösung für Predictive Maintenance ein.
1. Wählen Sie die gewünschten Angaben für **Region** und **Abonnement**, um die Lösung bereitzustellen.
1. Klicken Sie auf **Lösung erstellen** , um den Bereitstellungsprozess zu beginnen. Das dauert in der Regel einige Minuten.

### <a name="wait-for-the-provisioning-process-to-complete"></a>Warten auf den Abschluss des Bereitstellungsvorgangs

1. Klicken Sie auf die Kachel für Ihre Lösung mit dem Status **Bereitstellung** .
1. Beachten Sie die **Bereitstellungsstatus** während der Bereitstellung von Azure-Diensten im Azure-Abonnement.
1. Nach Abschluss der Bereitstellung ändert sich der Status in **Bereit**.
1. Klicken Sie auf die Kachel, um im rechten Bereich Details zur Lösung anzuzeigen. Aus diesem Bereich können Sie das Lösungsdashboard starten und auf den Machine Learning-Arbeitsbereich zugreifen.

> [!NOTE]
> Falls beim Bereitstellen der vorkonfigurierten Lösung Probleme auftreten, helfen Ihnen die Informationen unter [Berechtigungen für die Website „azureiotsuite.com“][lnk-permissions] und die [häufig gestellten Fragen][lnk-faq] weiter. Sollten die Probleme weiterhin auftreten, können Sie im [Portal][lnk-portal] ein Dienstticket erstellen.

Erwarten Sie Informationen zu Ihrer Lösung, die nicht aufgeführt werden? Senden Sie uns Vorschläge für neue Funktionen über den [Feedbackbereich](https://feedback.azure.com/forums/321918-azure-iot)zu.

## <a name="view-the-solution"></a>Anzeigen der Lösung

In diesem Abschnitt erhalten Sie Informationen zur Benutzeroberfläche der Lösung.

### <a name="predictive-maintenance-dashboard"></a>Dashboard für vorbeugende Wartung
Auf dieser Seite in der Webanwendung werden PowerBI-JavaScript-Steuerelemente (siehe [Repository mit visuellen PowerBI-Elementen][lnk-powerbi]) zum Visualisieren verwendet:

* Ausgabedaten von Stream Analytics-Aufträgen in der Blob Storage-Einheit
* Restlebensdauer und Zyklusanzahl pro Flugzeugtriebwerk

### <a name="observing-the-behavior-of-the-cloud-solution"></a>Beobachten des Verhaltens der Cloudlösung
Navigieren Sie im Azure-Portal zur Ressourcengruppe mit dem von Ihnen gewählten Lösungsnamen, um Ihre bereitgestellten Ressourcen anzuzeigen.

![][img-resource-group]

Beim Bereitstellen der vorkonfigurierten Lösung erhalten Sie eine E-Mail mit einem Link zum Machine Learning-Arbeitsbereich. Sie können auch über die Kachel auf der Seite [azureiotsuite.com][lnk-azureiotsuite] Ihrer bereitgestellten Lösung zum Machine Learning-Arbeitsbereich navigieren, wenn der Status **Bereit** lautet.

![][img-machine-learning]

Im Lösungsportal sehen Sie, dass für das Beispiel vier simulierte Geräte bereitgestellt werden, die für zwei Flugzeuge mit zwei Triebwerken pro Flugzeug und jeweils vier Sensoren stehen. Wenn Sie zum ersten Mal zum Lösungsportal navigieren, wird die Simulation angehalten.

![][img-simulation-stopped]

Klicken Sie auf **Simulation starten**, um die Simulation zu starten. Das Dashboard wird dann mit dem Sensorverlauf, der Restlebensdauer, den Zyklen und dem Verlauf der Restlebensdauer gefüllt.

![][img-simulation-running]

Wenn der Wert für die Restlebensdauer kleiner als 160 ist (zu Demonstrationszwecken willkürlich gewählter Schwellenwert), wird im Lösungsportal ein Warnsymbol neben der Anzeige der Restlebensdauer eingeblendet, und das Flugzeugtriebwerk wird in gelber Farbe hervorgehoben. Beachten Sie, dass die Werte für die Restlebensdauer im Allgemeinen einen Abwärtstrend aufweisen, aber trotzdem relativ stark variieren. Der Grund für dieses Verhalten sind die variierenden Zykluslängen und die Modellgenauigkeit.

![][img-simulation-warning]

Die vollständige Simulation dauert ca. 35 Minuten, und es werden 148 Zyklen durchgeführt. Der Schwellenwert der Restlebensdauer von 160 wird zum ersten Mal nach ca. fünf Minuten erreicht, und für beide Triebwerke ist der Schwellenwert nach ca. acht Minuten erreicht.

Die Simulation durchläuft das vollständige Dataset für 148 Zyklen, und die endgültigen Werte für die Restlebensdauer und Zyklen werden festgelegt.

Sie können die Simulation jederzeit stoppen. Wenn Sie auf **Simulation starten** klicken, wird die Simulation aber ab dem Anfang des Datasets wiedergegeben.

## <a name="next-steps"></a>Nächste Schritte

Informationen dazu, wie Azure IoT Szenarios mit vorbeugender Wartung ermöglicht, finden Sie unter [Capture value from the Internet of Things][lnk_capture_value] (Nutzen der Vorteile des Internets der Dinge).

Sehen Sie sich die [exemplarische Vorgehensweise][lnk-predictive-walkthrough] zur vorkonfigurierten Lösung für die vorbeugende Wartung an.

Sie können auch einige andere Features und Funktionen der vorkonfigurierten IoT Suite-Lösungen ausprobieren:

* [Häufig gestellte Fragen zu IoT Suite][lnk-faq]
* [Sicherheit im Internet der Dinge von Anfang an][lnk-security-groundup]

[img-resource-group]: media/iot-suite-predictive-overview/resource-group.png
[img-simulation-stopped]: media/iot-suite-predictive-overview/simulation-stopped.png
[img-simulation-running]: media/iot-suite-predictive-overview/simulation-running.png
[img-simulation-warning]: media/iot-suite-predictive-overview/simulation-warning.png
[img-machine-learning]: media/iot-suite-predictive-overview/machine-learning.png

[lnk-powerbi]: https://www.github.com/Microsoft/PowerBI-visuals
[lnk-predictive-walkthrough]: iot-suite-predictive-walkthrough.md
[lnk_preconfigured_solutions]: iot-suite-what-are-preconfigured-solutions.md
[lnk_iot_suite]: iot-suite-overview.md
[lnk_infographic]: https://www.microsoft.com/server-cloud/predictivemaintenance/Index.html
[lnk_regression_model]: http://gallery.cortanaanalytics.com/Collection/Predictive-Maintenance-Template-3

[lnk_capture_value]: http://download.microsoft.com/download/0/7/D/07D394CE-185D-4B96-AC3C-9B61179F7080/Capture_value_from_the_Internet%20of%20Things_with_Predictive_Maintenance.PDF
[lnk-faq]: iot-suite-faq.md
[lnk-security-groundup]: securing-iot-ground-up.md
[lnk-azureiotsuite]: https://www.azureiotsuite.com/
[lnk_free_trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-azureiotsuite]: https://www.azureiotsuite.com
[lnk-permissions]: iot-suite-permissions.md
[lnk-portal]: http://portal.azure.com/
[lnk-machine-learning]: https://azure.microsoft.com/services/machine-learning/


<!--HONumber=Feb17_HO1-->


