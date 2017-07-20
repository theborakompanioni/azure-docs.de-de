---
title: Grundlegendes zum Azure IoT Hub-D2C-Messaging | Microsoft-Dokumentation
description: "Entwicklerhandbuch: Verwenden des D2C-Messaging mit IoT Hub. Enthält Informationen zum Senden von Telemetriedaten und anderen Daten und zum Übermitteln von Nachrichten mithilfe von Routing."
services: iot-hub
documentationcenter: .net
author: dominicbetts
manager: timlt
editor: 
ms.service: iot-hub
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/25/2017
ms.author: dobett
ms.translationtype: Human Translation
ms.sourcegitcommit: 5edc47e03ca9319ba2e3285600703d759963e1f3
ms.openlocfilehash: d856e26084ee79386a2e8e0e527804bda86b477b
ms.contentlocale: de-de
ms.lasthandoff: 05/31/2017


---
# <a name="send-device-to-cloud-messages-to-iot-hub"></a>Senden von D2C-Nachrichten an IoT Hub

Zum Senden von Zeitreihen-Telemetrie und Warnungen von Ihren Geräten an Ihr Lösungs-Back-End senden Sie D2C-Nachrichten von Ihrem Gerät an Ihre IoT Hub-Instanz. Eine Erläuterung anderer D2C-Optionen, die von IoT Hub unterstützt werden, finden Sie im [Leitfaden zur D2C-Kommunikation][lnk-d2c-guidance].

Sie senden Gerät-zu-Cloud-Nachrichten (Device-to-Cloud, D2C) von einem geräteseitigen Endpunkt (**/devices/{Geräte-ID}/messages/events**). Mit Routingregeln werden Ihre Nachrichten dann an einen der dienstseitigen Endpunkte auf Ihrem IoT Hub geleitet. Bei Routingregeln werden Header und Text der D2C-Nachrichten, die Ihren Hub durchlaufen, zur Ermittlung des Zielorts für die Weiterleitung verwendet. Standardmäßig werden Nachrichten an den integrierten dienstseitigen Endpunkt (**messages/events**) gesendet, der mit [Event Hubs][lnk-event-hubs] kompatibel ist. Auf diese Weise können Sie standardmäßige [Event Hubs-Integration und -SDKs][lnk-compatible-endpoint] zum Empfangen von D2C-Nachrichten an Ihrem Lösungs-Back-End verwenden.

IoT Hub implementiert das D2C-Messaging anhand eines Streaming-Messagingmusters. Die D2C-Nachrichten von IoT Hub ähneln eher [Event Hubs][lnk-event-hubs] *-Ereignissen* als [Service Bus][lnk-servicebus] *-Nachrichten*, da der Dienst von einem größeren Volumen von Ereignissen durchlaufen wird, die von mehreren Lesern gelesen werden können.

D2C-Messaging mit IoT Hub weist folgende Merkmale auf:

* D2C-Nachrichten sind dauerhafter Art und werden bis zu sieben Tage lang auf dem Standardendpunkt der IoT Hub-Instanz (**messages/events**) aufbewahrt.
* D2C-Nachrichten dürfen maximal 256 KB groß sein. Sie können in Batches gruppiert werden, um den Sendevorgang zu optimieren. Die Batches können maximal 256 KB groß sein.
* Wie im Abschnitt [Verwalten des Zugriffs auf IoT Hub][lnk-devguide-security] erläutert, ermöglicht IoT Hub Authentifizierung und Zugriffssteuerung auf Gerätebasis.
* Mit IoT Hub können Sie bis zu zehn benutzerdefinierte Endpunkte erstellen. Nachrichten werden basierend auf Routen, die auf Ihrem IoT Hub konfiguriert sind, an die Endpunkte gesendet. Weitere Informationen finden Sie unter [Routingregeln](#routing-rules).
* IoT Hub ermöglicht Millionen gleichzeitig verbundener Geräte (siehe [Kontingente und Drosselung][lnk-quotas]).
* IoT Hub erlaubt keine beliebige Partitionierung. D2C-Nachrichten werden gemäß ihrer ursprünglichen **deviceId**partitioniert.

Weitere Informationen zu den Unterschieden zwischen den Diensten IoT Hub und Event Hubs finden Sie unter [Vergleich zwischen Azure IoT Hub und Azure Event Hubs][lnk-comparison].

## <a name="send-non-telemetry-traffic"></a>Senden von Datenverkehr ohne Telemetrie

Häufig senden Geräte nicht nur Telemetriedatenpunkte, sondern auch Nachrichten und Anforderungen, die eine separate Ausführung und Verarbeitung im Lösungs-Back-End erforderlich machen. Ein Beispiel hierfür sind kritische Warnungen, für die eine bestimmte Aktion auf dem Back-End ausgelöst werden muss. Sie können auf einfache Weise eine [Routingregel][lnk-devguide-custom] schreiben, mit der diese Arten von Nachrichten basierend auf einem Header oder einem Wert im Nachrichtentext an einen Endpunkt gesendet werden, der für deren Verarbeitung zuständig ist.

Weitere Informationen zur besten Möglichkeit zum Verarbeiten dieser Art von Nachricht finden Sie im Tutorial [Gewusst wie: Verarbeiten von D2C-Nachrichten mit IoT Hub][lnk-d2c-tutorial].

## <a name="route-device-to-cloud-messages"></a>Weiterleiten von D2C-Nachrichten

Sie haben zwei Möglichkeiten für das Weiterleiten von D2C-Nachrichten an Ihre Back-End-Apps:

* Verwenden Sie den integrierten [Event Hub-kompatiblen Endpunkt][lnk-compatible-endpoint], über den Back-End-Apps vom Hub empfangene D2C-Nachrichten lesen können. Weitere Informationen zum integrierten Event Hub-kompatiblen Endpunkt finden Sie unter [Lesen von D2C-Nachrichten vom integrierten Endpunkt][lnk-devguide-builtin].
* Verwenden Sie Routingregeln zum Senden von Nachrichten an benutzerdefinierte Endpunkte in Ihrer IoT Hub-Instanz. Mit benutzerdefinierten Endpunkten können Ihre Back-End-Apps D2C-Nachrichten mithilfe von Event Hubs, Service Bus-Warteschlangen oder Service Bus-Themen lesen. Weitere Informationen zum Routing und zu benutzerdefinierten Endpunkten finden Sie unter [Verwenden von benutzerdefinierten Endpunkten und Routingregeln für D2C-Nachrichten][lnk-devguide-custom].

## <a name="anti-spoofing-properties"></a>Eigenschaften zum Schutz vor Spoofing

Um ein Gerätespoofing beim D2C-Messaging zu verhindern, versieht IoT Hub alle Nachrichten mit den folgenden Eigenschaften:

* **ConnectionDeviceId**
* **ConnectionDeviceGenerationId**
* **ConnectionAuthMethod**

Die ersten beiden Eigenschaften enthalten die Werte für **deviceId** und **generationId** des ursprünglichen Geräts, wie unter [Geräteidentitätseigenschaften][lnk-device-properties] beschrieben.

Die Eigenschaft **ConnectionAuthMethod** enthält ein serialisiertes JSON-Objekt mit folgenden Eigenschaften:

```json
{
  "scope": "{ hub | device}",
  "type": "{ symkey | sas}",
  "issuer": "iothub"
}
```

## <a name="next-steps"></a>Nächste Schritte

Informationen zu den SDKs, die Sie für das Senden von D2C-Nachrichten verwenden können, finden Sie unter [Azure IoT-SDKs][lnk-sdks].

In den Tutorials mit [ersten Schritten][lnk-get-started] erfahren Sie, wie Sie D2C-Nachrichten von simulierten und physischen Geräten senden. Ausführlichere Informationen finden Sie im Tutorial [Verarbeiten von IoT Hub-D2C-Nachrichten mit Routen][lnk-d2c-tutorial].

[lnk-devguide-builtin]: iot-hub-devguide-messages-read-builtin.md
[lnk-devguide-custom]: iot-hub-devguide-messages-read-custom.md
[lnk-comparison]: iot-hub-compare-event-hubs.md
[lnk-d2c-guidance]: iot-hub-devguide-d2c-guidance.md
[lnk-get-started]: iot-hub-get-started.md

[lnk-event-hubs]: http://azure.microsoft.com/documentation/services/event-hubs/
[lnk-servicebus]: http://azure.microsoft.com/documentation/services/service-bus/
[lnk-quotas]: iot-hub-devguide-quotas-throttling.md
[lnk-sdks]: iot-hub-devguide-sdks.md
[lnk-compatible-endpoint]: iot-hub-devguide-messages-read-builtin.md
[lnk-device-properties]: iot-hub-devguide-identity-registry.md#device-identity-properties
[lnk-devguide-security]: iot-hub-devguide-security.md
[lnk-d2c-tutorial]: iot-hub-csharp-csharp-process-d2c.md

