---
title: Informationen zu Azure IoT Hub für IT-Experten | Microsoft Docs
description: Informationen, die IT-Experten bei der Verwendung von Azure IoT Hub unterstützen, beispielsweise zu Portanforderungen und zum Sicherheitshintergrund.
services: iot-hub
documentationcenter: ''
author: dominicbetts
manager: timlt
editor: ''

ms.service: iot-hub
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/09/2016
ms.author: dobett

---
# Konfigurieren und Verwalten des Zugriffs auf IoT Hub
Die Informationen in diesem Artikel sollen IT-Experten beim Konfigurieren einer Umgebung unterstützen, in der IoT-Geräte über eine Netzwerkinfrastruktur mit IoT Hub kommunizieren.

## Netzwerkverbindung
Geräte können mit IoT Hub in Azure über verschiedene Protokolle kommunizieren. In der Regel richtet sich die Wahl des Protokolls nach den spezifischen Anforderungen der Lösung. Die folgende Tabelle enthält die ausgehenden Ports, die geöffnet sein müssen, damit ein Gerät ein bestimmtes Protokoll verwenden kann:

| Protocol | Port(s) |
| --- | --- |
| MQTT |8883 |
| AMQP |5671 |
| AMQP über WebSockets |443 |
| HTTPS |443 |
| LWM2M (Geräteverwaltung) |5684 |

Nachdem Sie einen IoT Hub in einer Azure-Region erstellt haben, behält er seine IP-Adresse für die gesamte Lebensdauer. Wenn der IoT Hub von Microsoft jedoch in eine andere Skalierungseinheit verschoben wird, wird ihm eine neue IP-Adresse zugewiesen, um die Dienstqualität zu gewährleisten.

## IoT Hub und Sicherheit
Nur Geräte, die bei einem IoT Hub registriert sind, dürfen mit diesem kommunizieren. Einem registrierten Gerät muss die *DeviceConnect*-Berechtigung gewährt werden. Ein Gerät identifiziert sich durch ein Token, das die eindeutige Geräte-ID in jeder ausgeführten Anforderung kapselt. Der Hub überprüft die Gültigkeit des Tokens und vergewissert sich, dass das Gerät nicht in einer Verweigerungsliste enthalten ist (also die *DeviceConnect*-Berechtigung nicht widerrufen wurde). Informationen zu den von IoT Hub unterstützten Token finden Sie unter [Verwenden von IoT Hub-Sicherheitstoken und X.509-Zertifikaten][lnk-tokens].

Der Zugriff auf andere Verwaltungsendpunkte in einem IoT Hub wird auch über einen Berechtigungssatz gesteuert: *iothubowner*, *service*, *registryRead* und *registryReadWrite*. Jede Clientverwaltungsanwendung, die eine Verbindung mit einem IoT Hub herstellt, muss ein Token mit den entsprechenden Berechtigungen enthalten.

## Nächste Schritte
Dieser Artikel enthält spezifische Informationen für IT-Experten und Entwickler, die ihre Entwicklungs- und Testumgebung konfigurieren. Der [Abschnitt zur Sicherheit im Entwicklungsleitfaden für Azure IoT Hub][lnk-devguide] bietet zusätzliche Informationen zu den Token und dem Berechtigungssystem in IoT Hub.

Weitere Informationen zu den Funktionen von IoT Hub finden Sie unter:

* [Entwerfen Ihrer Lösung][lnk-design]
* [Entwicklerhandbuch][lnk-devguide]
* [Erkunden der Geräteverwaltung mithilfe der Beispielbenutzeroberfläche][lnk-dmui]
* [Simulieren eines Geräts mit dem Gateway SDK][lnk-gateway]

[lnk-devguide]: iot-hub-devguide.md#security

[lnk-design]: iot-hub-guidance.md
[lnk-devguide]: iot-hub-devguide.md
[lnk-dmui]: iot-hub-device-management-ui-sample.md
[lnk-gateway]: iot-hub-linux-gateway-sdk-simulated-device.md
[lnk-tokens]: iot-hub-sas-tokens.md

<!---HONumber=AcomDC_0907_2016-->