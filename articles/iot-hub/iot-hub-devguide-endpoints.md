---
title: 'Entwicklungsleitfaden: IoT Hub-Endpunkte | Microsoft-Dokumentation'
description: "Entwicklungsleitfaden für Azure IoT Hub: Referenzinformationen zu IoT Hub-Endpunkten"
services: iot-hub
documentationcenter: .net
author: dominicbetts
manager: timlt
editor: 
ms.assetid: 57ba52ae-19c6-43e4-bc6c-d8a5c2476e95
ms.service: iot-hub
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/30/2016
ms.author: dobett
translationtype: Human Translation
ms.sourcegitcommit: c18a1b16cb561edabd69f17ecebedf686732ac34
ms.openlocfilehash: f2da46ba3fdf9386ad78ade4d2364a4a8990cd6d


---
# <a name="reference---iot-hub-endpoints"></a>Referenz: IoT Hub-Endpunkte
## <a name="list-of-iot-hub-endpoints"></a>Liste der IoT Hub-Endpunkte
Azure IoT Hub ist ein mehrinstanzenfähiger Dienst, der seine Funktionen für zahlreiche Akteure bereitstellt. Das nachstehende Diagramm zeigt die verschiedenen Endpunkte, die von IoT Hub verfügbar gemacht werden.

![IoT Hub-Endpunkte][img-endpoints]

Im Folgenden finden Sie eine Beschreibung der Endpunkte:

* **Ressourcenanbieter**. Der IoT Hub-Ressourcenanbieter macht eine [Azure Resource Manager][lnk-arm]-Schnittstelle verfügbar, mit der Azure-Abonnementbesitzer IoT-Hubs erstellen und löschen sowie IoT Hub-Eigenschaften aktualisieren können. IoT Hub-Eigenschaften steuern [Sicherheitsrichtlinien auf Hubebene][lnk-accesscontrol] (im Gegensatz zur Zugriffssteuerung auf Geräteebene) und funktionale Optionen für das Messaging zwischen Cloud und Gerät (Cloud-to-Device, C2D) sowie Gerät und Cloud (Device-to-Cloud, D2C). Der IoT Hub-Ressourcenanbieter ermöglicht außerdem das [Exportieren von Geräteidentitäten][lnk-importexport].
* **Geräteidentitätsverwaltung**. Jeder IoT Hub legt einen Satz an HTTP-REST-Endpunkten zum Verwalten von Geräteidentitäten offen (zum Erstellen, Abrufen, Aktualisieren und Löschen). [Geräteidentitäten][lnk-device-identities] werden zur Geräteauthentifizierung und für die Zugriffssteuerung eingesetzt.
* **Verwaltung von Gerätezwillingen**. Jeder IoT-Hub macht eine Reihe von dienstseitigen HTTP-REST-Endpunkten zum Abfragen und Aktualisieren von [Gerätezwillingen][lnk-twins] (Updatetags und Eigenschaften) verfügbar.
* **Auftragsverwaltung**. Jeder IoT-Hub macht eine Reihe von dienstseitigen HTTP-REST-Endpunkten zum Abfragen und Verwalten von [Aufträgen][lnk-jobs] verfügbar.
* **Geräteendpunkte**. Für jedes in der Identitätsregistrierung bereitgestellte Gerät stellt IoT Hub eine Reihe von Endpunkten zur Verfügung, die ein Gerät zum Senden und Empfangen von Nachrichten verwenden kann:
  
  * *Senden von D2C-Nachrichten*. Verwenden Sie diesen Endpunkt, um [D2C-Nachrichten zu senden][lnk-d2c].
  * *Empfangen von C2D-Nachrichten*. Ein Gerät verwendet diesen Endpunkt, um gezielte [C2D-Nachrichten zu empfangen][lnk-c2d].
  * *Einleiten von Dateiuploads*. Ein Gerät verwendet diesen Endpunkt zum Empfangen eines Azure Storage-SAS-URIs von IoT Hub, um [eine Datei hochzuladen][lnk-upload].
  * *Abrufen und Aktualisieren der Eigenschaften von Gerätezwillingen*. Ein Gerät verwendet diese Endpunkte für den Zugriff auf die Eigenschaften seines [Gerätezwillings][lnk-twins].
  * *Empfangen von Anforderungen direkter Methoden*. Ein Gerät verwendet diese Endpunkte zum Lauschen auf Anforderungen [direkter Methoden][lnk-methods].
    
    Diese Endpunkte werden über die Protokolle [MQTT v3.1.1][lnk-mqtt], HTTP 1.1 und [AMQP 1.0][lnk-amqp] verfügbar gemacht. Beachten Sie, dass AMQP auch über [WebSockets][lnk-websockets] an Port 443 verfügbar ist.
    
    Die Zwillings- und Methodenendpunkte sind nur über [MQTT 3.1.1][lnk-mqtt] verfügbar.
* **Dienstendpunkte**. Jeder IoT Hub legt eine Reihe von Endpunkten offen, die Ihr Anwendungs-Back-End zur Kommunikation mit Ihren Geräten verwenden kann. Diese Endpunkte werden derzeit nur über das Protokoll [AMQP][lnk-amqp] verfügbar gemacht. Eine Ausnahme ist der Endpunkt für den Methodenaufruf, der über HTTP 1.1 verfügbar gemacht wird.
  
  * *Empfangen von D2C-Nachrichten*. Dieser Endpunkt ist kompatibel mit [Azure Event Hubs][lnk-event-hubs]. Ein Back-End-Dienst kann ihn zum Lesen aller [D2C-Nachrichten][lnk-d2c] verwenden, die Ihre Geräte senden.
  * *Senden von C2D-Nachrichten und Empfangen von Übermittlungsbestätigungen*. Diese Endpunkte ermöglichen Ihrem Anwendungs-Back-End das Senden von zuverlässigen [C2D-Nachrichten][lnk-c2d] sowie das Empfangen zugehöriger Übermittlungs- oder Ablaufbestätigungen.
  * *Empfangen von Dateibenachrichtigungen*. Dieser Messaging-Endpunkt ermöglicht den Empfang von Benachrichtigungen, wenn Ihre Geräte erfolgreich eine Datei hochgeladen haben. 
  * *Aufruf direkter Methoden*. Dieser Endpunkt ermöglicht einem Back-End-Dienst das Aufrufen einer [direkten Methode][lnk-methods] auf einem Gerät.

Im Artikel [Azure IoT SDKs][lnk-sdks] werden die verschiedenen Möglichkeiten zum Zugriff auf diese Endpunkte beschrieben.

Schließlich ist es wichtig zu wissen, dass alle IoT Hub-Endpunkte das [TLS][lnk-tls]-Protokoll nutzen und Endpunkte niemals auf unverschlüsselten/unsicheren Kanälen verfügbar gemacht werden.

## <a name="field-gateways"></a>Bereichsgateways
Bei einer IoT-Lösung ist zwischen Ihren Geräten und IoT Hub-Endpunkten ein *Bereichsgateway* angeordnet. Es befindet sich normalerweise in der Nähe Ihrer Geräte. Ihre Geräte kommunizieren direkt mit dem Bereichsgateway, indem sie ein von den Geräten unterstütztes Protokoll nutzen. Das Bereichsgateway verbindet sich mit einem IoT Hub-Endpunkt über ein Protokoll, das von IoT Hub unterstützt wird. Bei einem Bereichsgateway kann es sich um hochspezialisierte Hardware oder einen Computer mit niedriger Leistung handeln, auf dem Software für das End-to-End-Szenario ausgeführt wird, für das das Gateway vorgesehen ist.

Sie können das [Azure IoT Gateway SDK][lnk-gateway-sdk] verwenden, um ein Bereichsgateway zu implementieren. Dieses SDK bietet bestimmte Funktionen, z.B. die Möglichkeit, die Kommunikation von mehreren Geräten über eine Verbindung mit IoT Hub im Multiplexverfahren zu übertragen.

## <a name="next-steps"></a>Nächste Schritte
Weitere Referenzthemen in diesem IoT Hub-Entwicklungsleitfaden:

* [IoT Hub-Abfragesprache für Gerätezwillinge und Aufträge][lnk-devguide-query]
* [Kontingente und Drosselung][lnk-devguide-quotas]
* [IoT Hub-MQTT-Unterstützung][lnk-devguide-mqtt]

[lnk-gateway-sdk]: https://github.com/Azure/azure-iot-gateway-sdk

[img-endpoints]: ./media/iot-hub-devguide-endpoints/endpoints.png
[lnk-amqp]: https://www.amqp.org/
[lnk-mqtt]: http://mqtt.org/
[lnk-websockets]: https://tools.ietf.org/html/rfc6455
[lnk-arm]: ../azure-resource-manager/resource-group-overview.md
[lnk-event-hubs]: http://azure.microsoft.com/documentation/services/event-hubs/

[lnk-tls]: https://tools.ietf.org/html/rfc5246


[lnk-sdks]: iot-hub-devguide-sdks.md
[lnk-accesscontrol]: iot-hub-devguide-security.md#access-control-and-permissions
[lnk-importexport]: iot-hub-devguide-identity-registry.md#import-and-export-device-identities
[lnk-d2c]: iot-hub-devguide-messaging.md#device-to-cloud-messages
[lnk-device-identities]: iot-hub-devguide-identity-registry.md
[lnk-upload]: iot-hub-devguide-file-upload.md
[lnk-c2d]: iot-hub-devguide-messaging.md#cloud-to-device-messages
[lnk-methods]: iot-hub-devguide-direct-methods.md
[lnk-twins]: iot-hub-devguide-device-twins.md
[lnk-query]: iot-hub-devguide-query-language.md
[lnk-jobs]: iot-hub-devguide-jobs.md

[lnk-devguide-quotas]: iot-hub-devguide-quotas-throttling.md
[lnk-devguide-query]: iot-hub-devguide-query-language.md
[lnk-devguide-mqtt]: iot-hub-mqtt-support.md



<!--HONumber=Nov16_HO5-->


