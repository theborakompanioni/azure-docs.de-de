---
title: Vergleich zwischen Azure IoT Hub und Azure Event Hubs | Microsoft Docs
description: Hier finden Sie einen Vergleich der Azure IoT Hub- und Azure Event Hubs-Dienste, wobei besonders auf Unterschiede bei den Funktionen und Anwendungsbeispiele eingegangen wird.
services: iot-hub
documentationcenter: 
author: fsautomata
manager: timlt
editor: 
ms.assetid: aeddea62-8302-48e2-9aad-c5a0e5f5abe9
ms.service: iot-hub
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/16/2016
ms.author: elioda
translationtype: Human Translation
ms.sourcegitcommit: ce514e19370d2b42fb16b4e96b66f212d5fa999c
ms.openlocfilehash: 9c7e33ceebb28f6a263d2c92e0c0208434e64ffc


---
# <a name="comparison-of-azure-iot-hub-and-azure-event-hubs"></a>Vergleich zwischen Azure IoT Hub und Azure Event Hubs
Eine der wichtigsten Anwendungsmöglichkeiten für IoT Hub ist die Erfassung der Telemetriedaten von Geräten. Aus diesem Grund wird IoT Hub häufig mit [Azure Event Hubs][Azure Event Hubs] verglichen. Ebenso wie IoT Hub ist Event Hubs ein Dienst zur Ereignisverarbeitung, der riesige Mengen von Ereignis- und Telemetriedaten erfassen kann und gleichzeitig eine niedrige Latenz und hohe Zuverlässigkeit bietet.

Allerdings bestehen bei den Diensten viele Unterschiede, die in der folgenden Tabelle beschrieben werden.

| Bereich | IoT Hub | Event Hubs |
| --- | --- | --- |
| Kommunikationsmuster | Ermöglicht [Gerät-zu-Cloud-Kommunikation][lnk-d2c-guidance] (Messaging, Dateiuploads und gemeldete Eigenschaften) und [Cloud-zu-Gerät-Kommunikation][lnk-c2d-guidance] (direkte Methoden, gewünschte Eigenschaften, Messaging). |Ermöglicht nur Ereigniseingang (meist für D2C-Szenarien). |
| Gerätestatusinformationen | [Gerätezwillinge][lnk-twins] können Gerätestatusinformationen speichern und abfragen. | Gerätestatusinformationen können nicht gespeichert werden. |
| Unterstützung für Geräteprotokolle |Unterstützt MQTT, MQTT über WebSockets, AMQP, AMQP über WebSockets und HTTP. Darüber hinaus arbeitet IoT Hub mit dem [Azure IoT-Protokollgateway][lnk-azure-protocol-gateway] zusammen, einer anpassbaren Protokollgateway-Implementierung, die benutzerdefinierte Protokolle unterstützt. |Unterstützt AMQP, AMQP über WebSockets und HTTP. |
| Sicherheit |Ermöglicht Identität pro Gerät und widerrufbare Zugriffssteuerung. Siehe [Abschnitt über Sicherheit im IoT Hub-Entwicklerleitfaden]. |Ermöglicht Event Hub-weite [SAS-Richtlinien][Event Hubs - security] mit begrenzter Widerrufsunterstützung über [Richtlinien des Herausgebers][Event Hubs publisher policies]. Bei IoT-Lösungen ist es häufig erforderlich, eine benutzerdefinierte Lösung zur Unterstützung von Anmeldeinformationen pro Gerät und Maßnahmen zum Schutz vor Spoofing zu implementieren. |
| Vorgangsüberwachung |Ermöglicht IoT-Lösungen, einen umfangreichen Satz von Geräte-Identitätsverwaltungs- und -verbindungsereignissen zu abonnieren, z. B. individuelle Authentifizierungsfehler, Drosselung und Ausnahmen bei ungültigen Formaten. Diese Ereignisse ermöglichen Ihnen das rasche Bestimmen von Verbindungsproblemen auf Einzelgerätebene. |Macht nur aggregierte Metriken verfügbar. |
| Skalieren |Ist für die Unterstützung von Millionen von gleichzeitig verbundenen Geräten optimiert. |Kann eine eingeschränktere Anzahl von gleichzeitigen Verbindungen unterstützen – bis zu 5.000 AMQP-Verbindungen gemäß [Azure Service Bus-Kontingenten][Azure Service Bus quotas]. Jedoch können Sie mit Event Hubs die Partition für jede gesendete Nachricht angeben. |
| Geräte-SDKs |Bietet [Geräte-SDKs][Azure IoT SDKs] für eine Vielzahl von Plattformen und Sprachen, zusätzlich zu direkten MQTT-, AMQP- und HTTP-APIs. |Wird unter .NET, Java und C unterstützt und verfügt außerdem über AMQP- und HTTP-Sendeschnittstellen. |
| Dateiupload |Ermöglicht IoT-Lösungen zum Hochladen von Dateien von Geräten in die Cloud. Enthält einen Dateibenachrichtigungs-Endpunkt für die Workflowintegration und eine Vorgangsüberwachungskategorie für die Debugunterstützung. | Nicht unterstützt. |

Zusammengefasst bedeutet dies: Auch wenn nur der D2C-Telemetrieeingang als Anwendungsfall vorhanden ist, bietet IoT Hub einen Dienst, der für die Verbindung von IoT-Geräten ausgelegt ist. Außerdem werden die Wertbeiträge für diese Szenarien mit IoT-spezifischen Features weiter ausgebaut. Event Hubs sind für den Ereigniseingang in großem Umfang ausgelegt, sowohl im Kontext von Szenarien im Rechenzentrum als auch außerhalb davon.

Es ist nicht ungewöhnlich, IoT Hub und Event Hubs in der gleichen Lösung zu verwenden. IoT Hub wird für die D2C-Kommunikation eingesetzt und mit Event Hubs der nachgeschaltete Ereigniseingang in Echtzeit-Verarbeitungsmodulen geregelt.

### <a name="next-steps"></a>Nächste Schritte
Weitere Informationen zum Planen Ihrer IoT Hub-Bereitstellung finden Sie unter [Skalierung, hohe Verfügbarkeit und Notfallwiederherstellung][lnk-scaling].

Weitere Informationen zu den Funktionen von IoT Hub finden Sie unter:

* [Entwicklerhandbuch][lnk-devguide]
* [Simulieren eines Geräts mit dem IoT Gateway SDK][lnk-gateway]

[lnk-twins]: iot-hub-devguide-device-twins.md
[lnk-c2d-guidance]: iot-hub-devguide-c2d-guidance.md
[lnk-d2c-guidance]: iot-hub-devguide-d2c-guidance.md

[Azure Event Hubs]: ../event-hubs/event-hubs-what-is-event-hubs.md
[Abschnitt über Sicherheit im IoT Hub-Entwicklerleitfaden]: iot-hub-devguide-security.md
[Event Hubs - security]: ../event-hubs/event-hubs-authentication-and-security-model-overview.md
[Event Hubs publisher policies]: ../event-hubs/event-hubs-overview.md#common-publisher-tasks
[Azure Service Bus quotas]: ../service-bus-messaging/service-bus-quotas.md
[Azure IoT SDKs]: https://github.com/Azure/azure-iot-sdks/blob/master/readme.md
[lnk-azure-protocol-gateway]: iot-hub-protocol-gateway.md

[lnk-scaling]: iot-hub-scaling.md
[lnk-devguide]: iot-hub-devguide.md
[lnk-gateway]: iot-hub-linux-gateway-sdk-simulated-device.md



<!--HONumber=Nov16_HO5-->


