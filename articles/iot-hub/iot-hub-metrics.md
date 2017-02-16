---
title: Azure IoT Hub-Metriken | Microsoft-Dokumentation
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
ms.sourcegitcommit: a243e4f64b6cd0bf7b0776e938150a352d424ad1
ms.openlocfilehash: 689e3a07fd9bdd82d8c57cbe714562a780a14714


---
# <a name="iot-hub-metrics"></a>IoT Hub-Metriken
IoT Hub-Metriken liefern bessere Daten zum Zustand der Azure IoT-Ressourcen in Ihrem Azure-Abonnement. Auf der Grundlage von IoT Hub-Metriken können Sie die allgemeine Integrität des IoT Hub-Diensts und der mit ihm verbundenen Geräte beurteilen. Benutzerorientierte Statistiken spielen eine wichtige Rolle. Sie bieten die Basis, um die Vorgänge in Ihrem IoT Hub zu erkennen und Probleme zu beheben, ohne den Azure-Support zu kontaktieren.

IoT Hub-Metriken können über das Azure-Portal aktiviert werden.

## <a name="how-to-enable-iot-hub-metrics"></a>Aktivieren von IoT Hub-Metriken
1. Erstellen Sie einen IoT Hub. Anweisungen zum Erstellen eines IoT-Hubs finden Sie in der Anleitung mit den [ersten Schritten][lnk-get-started].
2. Öffnen Sie das Blatt Ihres IoT Hubs. Klicken Sie dort auf **Diagnose**.
   
    ![][1]
3. Konfigurieren Sie die Diagnose durch Festlegen des Status auf **Ein** und Auswählen eines Azure Storage-Kontos zum Speichern der Diagnosedaten. Aktivieren Sie **Metriken**, und klicken Sie dann auf **Speichern**. Beachten Sie, dass das Azure Storage-Konto vorab erstellt werden muss und die Speicherung separat in Rechnung gestellt wird. Als weitere Möglichkeit können Sie die Diagnosedaten auch an einen Event Hubs-Endpunkt senden.
   
    ![][2]
4. Nachdem Sie die Diagnose eingerichtet haben, kehren Sie zum Blatt **Übersicht** des IoT Hubs zurück. Metrikdaten werden im Abschnitt **Überwachung** des Blatts dargestellt. Durch Klicken auf das Diagramm wird der Metrikbereich geöffnet, in dem Sie eine Übersicht der Metrikdaten für Ihren IoT Hub anzeigen können. Sie können die Auswahl der im Diagramm gezeigten Metriken bearbeiten und basierend auf Metrikwerten Warnungen konfigurieren.
   
    ![][3]

## <a name="iot-hub-metrics-and-how-to-use-them"></a>IoT Hub-Metriken und ihre Verwendung
IoT Hub bietet mehrere Metriken, um Ihnen einen Überblick über die Integrität Ihres Hubs und die Gesamtzahl der verbundenen Geräte zu verschaffen. Sie können Informationen aus mehreren Metriken kombinieren, um sich ein umfassenderes Bild des Zustands des IoT Hubs zu machen. In der folgenden Tabelle werden die Metriken beschrieben, die für IoT Hubs nachverfolgt werden. Außerdem erfahren Sie, in welchem Bezug die einzelnen Metriken zum allgemeinen Status des IoT Hubs stehen.

| Metrik | Beschreibung der Metrik | Zweck der Metrik |
| --- | --- | --- |
| d2c.telemetry.Ingress.allProtocol |Anzahl der Nachrichten, die an alle Geräte gesendet wurden |Übersichtsdaten zu gesendeten Nachrichten |
| d2c.telemetry.ingress.success |Anzahl aller erfolgreichen Nachrichten an den IoT-Hub |Übersicht über den erfolgreichen Nachrichteneingang in den IoT-Hub |
| c2d.commands.egress.complete.success |Anzahl aller Befehlsnachrichten, die vom empfangenden Gerät auf allen Geräten abgeschlossen wurden |Bietet zusammen mit den Metriken zu Abbrüchen und Zurückweisungen eine Übersicht über die allgemeine Erfolgsquote bei C2D-Nachrichten |
| c2d.commands.egress.abandon.success |Anzahl aller Nachrichten, die vom empfangenden Gerät auf allen Geräten erfolgreich abgebrochen wurden |Hinweis auf potenzielle Probleme, wenn Nachrichten häufiger als erwartet abgebrochen werden |
| c2d.commands.egress.reject.success |Anzahl aller Nachrichten, die vom empfangenden Gerät auf allen Geräten erfolgreich zurückgewiesen wurden |Hinweis auf potenzielle Probleme, wenn Nachrichten häufiger als erwartet zurückgewiesen werden |
| devices.totalDevices |Durchschnittliche, minimale und maximale Anzahl von Geräten, die beim IoT Hub registriert sind |Die Anzahl der Geräte, die beim IoT-Hub registriert sind |
| devices.connectedDevices.allProtocol |Durchschnittliche, minimale und maximale Anzahl gleichzeitig verbundener Geräte |Übersicht über die Anzahl der Geräte, die mit dem IoT-Hub verbunden sind |

## <a name="next-steps"></a>Nächste Schritte
Nachdem Sie sich nun einen Überblick über IoT Hub-Metriken verschafft haben, folgen Sie dem folgenden Link, um mehr über die Verwaltung von IoT Hub zu erfahren:

* [Vorgangsüberwachung][lnk-monitor]

Weitere Informationen zu den Funktionen von IoT Hub finden Sie unter:

* [Entwicklungsleitfaden für IoT Hub][lnk-devguide]
* [Simulieren eines Geräts mit dem IoT Gateway SDK][lnk-gateway]

<!-- Links and images -->
[1]: media/iot-hub-metrics/enable-metrics-1.png
[2]: media/iot-hub-metrics/enable-metrics-2.png
[3]: media/iot-hub-metrics/enable-metrics-3.png

[lnk-get-started]: iot-hub-csharp-csharp-getstarted.md
[lnk-operations-monitoring]: iot-hub-operations-monitoring.md
[lnk-scaling]: iot-hub-scaling.md
[lnk-dr]: iot-hub-ha-dr.md

[lnk-monitor]: iot-hub-operations-monitoring.md

[lnk-devguide]: iot-hub-devguide.md
[lnk-gateway]: iot-hub-linux-gateway-sdk-simulated-device.md



<!--HONumber=Dec16_HO1-->


