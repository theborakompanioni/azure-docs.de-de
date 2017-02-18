---
title: "Überwachen von Azure IoT Hub mithilfe von Metriken | Microsoft-Dokumentation"
description: "Hier erfahren Sie, wie Sie mithilfe von Azure IoT Hub-Metriken die allgemeine Integrität Ihrer IoT Hub-Instanzen bewerten und überwachen."
services: iot-hub
documentationcenter: 
author: nberdy
manager: timlt
editor: 
ms.assetid: a47108fd-f994-4105-b21d-5b8f697b699c
ms.service: iot-hub
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/16/2016
ms.author: nberdy
translationtype: Human Translation
ms.sourcegitcommit: 64e69df256404e98f6175f77357500b562d74318
ms.openlocfilehash: e8cac4af4b971320429cc4c76b8d806e314e1143


---
# <a name="iot-hub-metrics"></a>IoT Hub-Metriken
IoT Hub-Metriken liefern bessere Daten zum Zustand der Azure IoT-Ressourcen in Ihrem Azure-Abonnement. Auf der Grundlage von IoT Hub-Metriken können Sie die allgemeine Integrität des IoT Hub-Diensts und der mit ihm verbundenen Geräte beurteilen. Benutzerorientierte Statistiken spielen eine wichtige Rolle. Sie bieten die Basis, um die Vorgänge in Ihrem IoT Hub zu erkennen und Probleme zu beheben, ohne den Azure-Support zu kontaktieren.

Metriken sind standardmäßig aktiviert. IoT Hub-Metriken können über das Azure-Portal angezeigt werden.

## <a name="how-to-view-iot-hub-metrics"></a>Anzeigen von IoT Hub-Metriken
1. Erstellen Sie einen IoT Hub. Anweisungen zum Erstellen eines IoT-Hubs finden Sie in der Anleitung mit den [ersten Schritten][lnk-get-started].
2. Öffnen Sie das Blatt Ihres IoT Hubs. Klicken Sie dort auf **Metriken**.
   
    ![][1]
3. Auf dem Blatt „Metriken“ können Sie die Metriken für Ihren IoT-Hub anzeigen und benutzerdefinierte Metrikansichten erstellen. Wenn Sie auf **Diagnoseeinstellungen** klicken, können Sie Ihre Metrikdaten an einen Event Hubs-Endpunkt oder an ein Azure Storage-Konto senden.
   
    ![][2]

## <a name="iot-hub-metrics-and-how-to-use-them"></a>IoT Hub-Metriken und ihre Verwendung
IoT Hub bietet mehrere Metriken, um Ihnen einen Überblick über die Integrität Ihres Hubs und die Gesamtzahl der verbundenen Geräte zu verschaffen. Sie können Informationen aus mehreren Metriken kombinieren, um sich ein umfassenderes Bild des Zustands des IoT Hubs zu machen. In der folgenden Tabelle werden die Metriken beschrieben, die für IoT Hubs nachverfolgt werden. Außerdem erfahren Sie, in welchem Bezug die einzelnen Metriken zum allgemeinen Status des IoT Hubs stehen.

| Metrik | Beschreibung der Metrik | Zweck der Metrik |
| --- | --- | --- |
| d2c.telemetry.Ingress.allProtocol | Anzahl der Nachrichten, die an alle Geräte gesendet wurden | Übersichtsdaten zu gesendeten Nachrichten |
| d2c.telemetry.ingress.success | Anzahl aller erfolgreichen Nachrichten an den Hub | Übersicht über den erfolgreichen Nachrichteneingang in den Hub |
| d2c.telemetry.egress.success | Anzahl aller erfolgreichen Schreibvorgänge an einen Endpunkt | Übersicht über die Nachrichtenauffächerung auf der Grundlage der Routen eines Benutzers |
| d2c.telemetry.egress.invalid | Anzahl von Nachrichten, die aufgrund von Inkompatibilität mit dem Endpunkt nicht übermittelt wurden | Übersicht über die Anzahl von Schreibfehlern für die Gruppe von Endpunkten des Benutzers. Hohe Werte deuten unter Umständen auf nicht ordnungsgemäß konfigurierte Endpunkte hin. |
| d2c.telemetry.egress.dropped | Anzahl von Nachrichten, die verworfen wurden, weil ein Endpunkt fehlerhaft war | Übersicht über die Anzahl verworfener Nachrichten in Anbetracht der aktuellen Konfiguration des IoT-Hubs |
| d2c.telemetry.egress.fallback | Anzahl von Nachrichten, die der Fallbackroute entsprechen | Für Benutzer, die alle Nachrichten nicht an den integrierten Endpunkt, sondern an andere Endpunkte umleiten, zeigt diese Metrik Lücken in den Routingeinstellungen auf. |
| d2c.telemetry.egress.orphaned | Anzahl von Nachrichten, die keiner Route entsprechen (einschließlich der Fallbackroute) | Übersicht über die Anzahl verwaister Nachrichten in Anbetracht der aktuellen Konfiguration der IoT Hub-Instanz |
| d2c.endpoints.latency.eventHubs | Durchschnittliche Wartezeit zwischen dem Eingang der Nachricht beim IoT-Hub und dem Eingang der Nachricht bei einem Event Hub-Endpunkt in Millisekunden | Anhand der Differenz können Benutzer Endpunktkonfigurationen mit Optimierungsbedarf erkennen. |
| d2c.endpoints.latency.serviceBusQueues | Durchschnittliche Wartezeit zwischen dem Eingang der Nachricht beim IoT-Hub und dem Eingang der Nachricht bei einem Service Bus-Warteschlangenendpunkt in Millisekunden | Anhand der Differenz können Benutzer Endpunktkonfigurationen mit Optimierungsbedarf erkennen. |
| d2c.endpoints.latency.serviceBusTopic | Durchschnittliche Wartezeit zwischen dem Eingang der Nachricht beim IoT-Hub und dem Eingang der Nachricht bei einem Service Bus-Themenendpunkt in Millisekunden | Anhand der Differenz können Benutzer Endpunktkonfigurationen mit Optimierungsbedarf erkennen. |
| d2c.endpoints.latency.builtIn.events | Durchschnittliche Wartezeit zwischen dem Eingang der Nachricht beim IoT-Hub und dem Eingang der Nachricht beim integrierten Endpunkt (Nachrichten/Ereignisse) in Millisekunden | Anhand der Differenz können Benutzer Endpunktkonfigurationen mit Optimierungsbedarf erkennen. |
| c2d.commands.egress.complete.success | Anzahl aller Befehlsnachrichten, die vom empfangenden Gerät auf allen Geräten abgeschlossen wurden |Bietet zusammen mit den Metriken zu Abbrüchen und Zurückweisungen eine Übersicht über die allgemeine C2D-Quote erfolgreicher Befehle |
| c2d.commands.egress.abandon.success | Anzahl aller Nachrichten, die vom empfangenden Gerät auf allen Geräten erfolgreich abgebrochen wurden |Hinweis auf potenzielle Probleme, wenn Nachrichten häufiger als erwartet abgebrochen werden |
| c2d.commands.egress.reject.success | Anzahl aller Nachrichten, die vom empfangenden Gerät auf allen Geräten erfolgreich zurückgewiesen wurden |Hinweis auf potenzielle Probleme, wenn Nachrichten häufiger als erwartet zurückgewiesen werden |
| devices.totalDevices | Die Anzahl der Geräte, die beim IoT-Hub registriert sind |Die Anzahl der Geräte, die beim Hub registriert sind |
| devices.connectedDevices.allProtocol | Die durchschnittliche Anzahl gleichzeitig verbundener Geräte |Übersicht über die Anzahl der Geräte, die mit dem Hub verbunden sind |

## <a name="next-steps"></a>Nächste Schritte
Nachdem Sie sich nun einen Überblick über IoT Hub-Metriken verschafft haben, folgen Sie dem folgenden Link, um mehr über die Verwaltung von IoT Hub zu erfahren:

* [Vorgangsüberwachung][lnk-monitor]

Weitere Informationen zu den Funktionen von IoT Hub finden Sie unter:

* [IoT Hub-Entwicklerhandbuch][lnk-devguide]
* [Simulieren eines Geräts mit dem IoT Gateway SDK][lnk-gateway]

<!-- Links and images -->
[1]: media/iot-hub-metrics/enable-metrics-1.png
[2]: media/iot-hub-metrics/enable-metrics-2.png

[lnk-get-started]: iot-hub-csharp-csharp-getstarted.md
[lnk-operations-monitoring]: iot-hub-operations-monitoring.md
[lnk-scaling]: iot-hub-scaling.md
[lnk-dr]: iot-hub-ha-dr.md

[lnk-monitor]: iot-hub-operations-monitoring.md

[lnk-devguide]: iot-hub-devguide.md
[lnk-gateway]: iot-hub-linux-gateway-sdk-simulated-device.md



<!--HONumber=Jan17_HO4-->


