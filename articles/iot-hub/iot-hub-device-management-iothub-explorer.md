---
title: "Azure IoT-Geräteverwaltung mit iothub-explorer | Microsoft-Dokumentation"
description: "Verwenden Sie das CLI-Tool iothub-explorer mit direkten Methoden und Optionen für die Verwaltung gewünschter Eigenschaften von Gerätezwillingen für die Azure IoT Hub-Geräteverwaltung."
services: iot-hub
documentationcenter: 
author: shizn
manager: timlt
tags: 
keywords: "Azure Iot-Geräteverwaltung, Azure IoT Hub-Geräteverwaltung, IoT-Geräteverwaltung, IoT Hub-Geräteverwaltung"
ms.assetid: b34f799a-fc14-41b9-bf45-54751163fffe
ms.service: iot-hub
ms.devlang: arduino
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/15/2017
ms.author: xshi
ms.translationtype: Human Translation
ms.sourcegitcommit: ff2fb126905d2a68c5888514262212010e108a3d
ms.openlocfilehash: 2234924251447bb32d7cca0623a4e8d4f8d4e7ff
ms.contentlocale: de-de
ms.lasthandoff: 06/17/2017

---
# <a name="use-iothub-explorer-for-azure-iot-hub-device-management"></a>Verwenden von iothub-explorer für die Azure IoT Hub-Geräteverwaltung

![Lückenloses Diagramm](media/iot-hub-get-started-e2e-diagram/2.png)

[!INCLUDE [iot-hub-get-started-note](../../includes/iot-hub-get-started-note.md)]

[iothub-explorer](https://github.com/azure/iothub-explorer) ist ein CLI-Tool, das Sie auf einem Hostcomputer ausführen, um Geräteidentitäten in der IoT Hub-Registrierung zu verwalten. Es bietet Verwaltungsoptionen, die Sie zum Ausführen verschiedener Aufgaben verwenden können.

| Verwaltungsoption          | Aufgabe                                                                                                                            |
|----------------------------|---------------------------------------------------------------------------------------------------------------------------------|
| Direkte Methoden             | Lassen Sie ein Gerät beispielsweise mit dem Senden von Nachrichten beginnen oder dies beenden, oder starten Sie es neu.                                        |
| Gewünschte Eigenschaften von Gerätezwillingen    | Setzen Sie ein Gerät in bestimmte Status, stellen Sie z.B. das Leuchten einer grünen LED ein, oder legen Sie das Telemetriesendeintervall auf 30 Minuten fest.         |
| Berichtete Eigenschaften von Gerätezwillingen   | Rufen Sie den berichteten Status eines Geräts ab. Das Gerät meldet z.B., das die LED jetzt blinkt.                                    |
| Gerätezwillingstags                  | Speichern gerätespezifischer Metadaten in der Cloud – beispielsweise den Aufstellungsort eines Automaten.                         |
| C2D-Nachrichten   | Senden Sie Benachrichtigungen an ein Gerät. Beispiel: „Heute wird es sehr wahrscheinlich regnen. Vergessen Sie Ihren Schirm nicht.“              |
| Gerätezwillingabfragen        | Fragen Sie alle Gerätezwillinge ab, um diejenigen abzurufen, die beliebige Bedingungen erfüllen; identifizieren Sie z.B. die zur Verwendung verfügbaren Geräte. |

Eine ausführlichere Erläuterung zu den Unterschieden und Anweisungen zur Verwendung dieser Optionen finden Sie im [Leitfaden zur D2C-Kommunikation](iot-hub-devguide-d2c-guidance.md) und [Leitfaden zur C2D-Kommunikation](iot-hub-devguide-c2d-guidance.md).

> [!NOTE]
> Gerätezwillinge sind JSON-Dokumente, in denen Gerätestatusinformationen (Metadaten, Konfigurationen und Bedingungen) gespeichert werden. Von IoT Hub wird für jedes Gerät, das eine Verbindung herstellt, dauerhaft ein Gerätezwilling gespeichert. Weitere Informationen zu Gerätezwillingen finden Sie unter [Erste Schritte mit Gerätezwillingen (Node)](iot-hub-node-node-twin-getstarted.md).

## <a name="what-you-learn"></a>Lerninhalt

Sie lernen, iothub-explorer mit verschiedenen Verwaltungsoptionen auf Ihrem Entwicklungscomputer zu verwenden.

## <a name="what-you-do"></a>Aufgaben

Führen Sie iothub-explorer mit verschiedenen Verwaltungsoptionen aus.

## <a name="what-you-need"></a>Voraussetzungen

- Abschluss des Tutorials [Einrichten Ihres Geräts](iot-hub-raspberry-pi-kit-node-get-started.md), in dem die folgenden Anforderungen behandelt werden:
  - Ein aktives Azure-Abonnement.
  - Ein Azure IoT Hub in Ihrem Abonnement.
  - Eine Clientanwendung, die Nachrichten an Ihren Azure IoT Hub sendet.
- iothub-explorer. ([Installieren von iothub-explorer ](https://github.com/azure/iothub-explorer) auf dem Entwicklungscomputer)

## <a name="connect-to-your-iot-hub"></a>Herstellen einer Verbindung mit Ihrem IoT Hub

Stellen Sie mithilfe des folgenden Befehls eine Verbindung mit Ihrem IoT Hub her:

```bash
iothub-explorer login <your IoT hub connection string>
```

## <a name="use-iothub-explorer-with-direct-methods"></a>Verwenden von iothub-explorer mit direkten Methoden

Rufen Sie mithilfe des folgenden Befehls die `start`-Methode in der Geräte-App auf, um Nachrichten an Ihren IoT Hub zu senden:

```bash
iothub-explorer device-method <your device Id> start
```

Rufen Sie mithilfe des folgenden Befehls die `stop`-Methode in der Geräte-App auf, um das Senden von Nachrichten an Ihren IoT Hub zu stoppen:

```bash
iothub-explorer device-method <your device Id> stop
```

## <a name="use-iothub-explorer-with-twins-desired-properties"></a>Verwenden von iothub-explorer mit den gewünschten Eigenschaften von Gerätezwillingen

Legen Sie mit folgendem Befehl für die gewünschte Eigenschaft ein Intervall von 3.000 fest:

```bash
iothub-explorer update-twin <your device id> {\"properties\":{\"desired\":{\"interval\":3000}}}
```

Diese Eigenschaft kann von Ihrem Gerät gelesen werden.

## <a name="use-iothub-explorer-with-twins-reported-properties"></a>Verwenden von iothub-explorer mit den berichteten Eigenschaften von Gerätezwillingen

Zeigen Sie mithilfe des folgenden Befehls die berichteten Eigenschaften des Geräts an:

```bash
iothub-explorer get-twin <your device id>
```

Eine der Eigenschaften ist „$metadata.$lastUpdated“. Sie gibt an, wann dieses Geräts zuletzt eine Nachricht gesendet oder empfangen hat.

## <a name="use-iothub-explorer-with-twins-tags"></a>Verwenden von iothub-explorer mit Gerätezwillingstags

Zeigen Sie mithilfe des folgenden Befehls die Tags und Eigenschaften des Geräts an:

```bash
iothub-explorer get-twin <your device id>
```

Fügen Sie mit folgendem Befehl dem Gerät eine Feldrolle „temperature&humidity“ hinzu:

```bash
iothub-explorer update-twin <your device id> {\"tags\":{\"role\":\"temperature&humidity\"}}
```

## <a name="use-iothub-explorer-with-cloud-to-device-messages"></a>Verwenden von iothub-explorer mit C2D-Nachrichten

Senden Sie mithilfe des folgenden Befehls eine „Hello World“-Nachricht an Ihr Gerät:

```bash
iothub-explorer send <device-id> "Hello World"
```

In [Verwenden von iothub-explorer zum Senden und Empfangen von Nachrichten zwischen Ihrem Gerät und IoT Hub](iot-hub-explorer-cloud-device-messaging.md) finden Sie ein reales Szenario zur Verwendung dieses Befehls.

## <a name="use-iothub-explorer-with-device-twins-queries"></a>Verwenden von iothub-explorer mit Abfragen von Gerätezwillingen

Fragen Sie mit folgendem Befehl Geräte mit einem Rollentag „temperature&humidity“ ab:

```bash
iothub-explorer query-twin "SELECT * FROM devices WHERE tags.role = 'temperature&humidity'"
```

Fragen Sie mit folgendem Befehl alle Geräte außer denen mit einem Rollentag „temperature&humidity“ ab:

```bash
iothub-explorer query-twin "SELECT * FROM devices WHERE tags.role != 'temperature&humidity'"
```

## <a name="next-steps"></a>Nächste Schritte

Sie haben jetzt gelernt, iothub-explorer mit verschiedenen Verwaltungsoptionen zu verwenden.

[!INCLUDE [iot-hub-get-started-next-steps](../../includes/iot-hub-get-started-next-steps.md)]

