---
title: Informationen zu Azure IoT Hub-Endpunkten | Microsoft-Dokumentation
description: "Entwicklerhandbuch: Referenzinformationen zu IoT Hub-Endpunkten mit Geräte- und Dienstanbindung"
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
ms.date: 08/08/2017
ms.author: dobett
ms.translationtype: HT
ms.sourcegitcommit: 9b7316a5bffbd689bdb26e9524129ceed06606d5
ms.openlocfilehash: a5753df2ff6874d9574e268953792cac9765cc54
ms.contentlocale: de-de
ms.lasthandoff: 09/08/2017

---
# <a name="reference---iot-hub-endpoints"></a>Referenz: IoT Hub-Endpunkte

## <a name="iot-hub-names"></a>IoT Hub-Namen

Den Namen der IoT Hub-Instanz, die Ihre Endpunkte hostet, finden Sie im Portal auf dem Blatt **Übersicht**. Standardmäßig sieht der DNS-Name einer IoT Hub-Instanz wie folgt aus: `{your iot hub name}.azure-devices.net`.

Mithilfe von Azure DNS können Sie einen benutzerdefinierten DNS-Namen für Ihre IoT Hub-Instanz erstellen. Weitere Informationen finden Sie unter [Bereitstellen von benutzerdefinierten Domäneneinstellungen für einen Azure-Dienst mit Azure DNS](../dns/dns-custom-domain.md#azure-iot).

## <a name="list-of-built-in-iot-hub-endpoints"></a>Liste der integrierten IoT Hub-Endpunkte

Azure IoT Hub ist ein mehrinstanzenfähiger Dienst, der seine Funktionen für zahlreiche Akteure bereitstellt. Das nachstehende Diagramm zeigt die verschiedenen Endpunkte, die von IoT Hub verfügbar gemacht werden.

![IoT Hub-Endpunkte][img-endpoints]

Die Endpunkte werden in der folgende Liste beschrieben:

* **Ressourcenanbieter**. Der IoT Hub-Ressourcenanbieter macht eine [Azure Resource Manager][lnk-arm]-Schnittstelle verfügbar. Über diese Schnittstelle können Azure Abonnementbesitzer IoT Hub-Instanzen erstellen und löschen sowie IoT Hub-Eigenschaften aktualisieren. IoT Hub-Eigenschaften steuern [Sicherheitsrichtlinien auf Hubebene][lnk-accesscontrol] (im Gegensatz zur Zugriffssteuerung auf Geräteebene) und funktionale Optionen für das Messaging zwischen Cloud und Gerät (Cloud-to-Device, C2D) sowie Gerät und Cloud (Device-to-Cloud, D2C). Der IoT Hub-Ressourcenanbieter ermöglicht außerdem das [Exportieren von Geräteidentitäten][lnk-importexport].
* **Geräteidentitätsverwaltung**. Jeder IoT Hub legt einen Satz an HTTP-REST-Endpunkten zum Verwalten von Geräteidentitäten offen (zum Erstellen, Abrufen, Aktualisieren und Löschen). [Geräteidentitäten][lnk-device-identities] werden zur Geräteauthentifizierung und für die Zugriffssteuerung eingesetzt.
* **Verwaltung von Gerätezwillingen**. Jeder IoT-Hub macht eine Reihe von dienstseitigen HTTP-REST-Endpunkten zum Abfragen und Aktualisieren von [Gerätezwillingen][lnk-twins] (Updatetags und Eigenschaften) verfügbar.
* **Auftragsverwaltung**. Jeder IoT-Hub macht eine Reihe von dienstseitigen HTTP-REST-Endpunkten zum Abfragen und Verwalten von [Aufträgen][lnk-jobs] verfügbar.
* **Geräteendpunkte**. Für jedes Gerät in der Identitätsregistrierung macht IoT Hub eine Reihe von Endpunkten verfügbar:

  * *Senden von D2C-Nachrichten*. Ein Gerät verwendet diesen Endpunkt, um [D2C-Nachrichten zu senden][lnk-d2c].
  * *Empfangen von C2D-Nachrichten*. Ein Gerät verwendet diesen Endpunkt, um gezielte [C2D-Nachrichten zu empfangen][lnk-c2d].
  * *Einleiten von Dateiuploads*. Ein Gerät verwendet diesen Endpunkt zum Empfangen eines Azure Storage-SAS-URIs von IoT Hub, um [eine Datei hochzuladen][lnk-upload].
  * *Abrufen und Aktualisieren der Eigenschaften von Gerätezwillingen*. Ein Gerät verwendet diesen Endpunkt für den Zugriff auf die Eigenschaften seines [Gerätezwillings][lnk-twins].
  * *Empfangen von Anforderungen direkter Methoden*. Ein Gerät verwendet diesen Endpunkt zum Lauschen auf Anforderungen [direkter Methoden][lnk-methods].

    Diese Endpunkte werden über die Protokolle [MQTT v3.1.1][lnk-mqtt], HTTP 1.1 und [AMQP 1.0][lnk-amqp] verfügbar gemacht. AMQP ist auch über [WebSockets][lnk-websockets] an Port 443 verfügbar.

* **Dienstendpunkte**. Jede IoT Hub-Instanz macht eine Reihe von Endpunkten verfügbar, über die Ihr Lösungs-Back-End mit Ihren Geräten kommunizieren kann. Diese Endpunkte werden bis auf eine Ausnahme unter Verwendung des [AMQP][lnk-amqp]-Protokolls verfügbar gemacht. Der Endpunkt für den Methodenaufruf wird über das HTTP-Protokoll verfügbar gemacht.
  
  * *Empfangen von D2C-Nachrichten*. Dieser Endpunkt ist kompatibel mit [Azure Event Hubs][lnk-event-hubs]. Ein Back-End-Dienst kann ihn zum Lesen der [D2C-Nachrichten][lnk-d2c] verwenden, die Ihre Geräte senden. Zusätzlich zu diesem integrierten Endpunkt können Sie für Ihren IoT-Hub benutzerdefinierte Endpunkte erstellen.
  * *Senden von C2D-Nachrichten und Empfangen von Übermittlungsbestätigungen*. Diese Endpunkte ermöglichen Ihrem Lösungs-Back-End das Senden von zuverlässigen [C2D-Nachrichten][lnk-c2d] sowie das Empfangen zugehöriger Übermittlungs- oder Ablaufbestätigungen.
  * *Empfangen von Dateibenachrichtigungen*. Dieser Messaging-Endpunkt ermöglicht den Empfang von Benachrichtigungen, wenn Ihre Geräte erfolgreich eine Datei hochgeladen haben. 
  * *Aufruf direkter Methoden*. Dieser Endpunkt ermöglicht einem Back-End-Dienst das Aufrufen einer [direkten Methode][lnk-methods] auf einem Gerät.
  * *Empfangen von Vorgangsüberwachungsereignissen*. Mit diesem Endpunkt können Sie Vorgangsüberwachungsereignisse empfangen, wenn Ihr IoT Hub für deren Ausgabe konfiguriert wurde. Weitere Informationen finden Sie unter [IoT Hub-Vorgangsüberwachung][lnk-operations-mon].

Im Artikel [Azure IoT SDKs][lnk-sdks] werden die verschiedenen Möglichkeiten zum Zugriff auf diese Endpunkte beschrieben.

Alle IoT Hub-Endpunkte verwenden das [TLS][lnk-tls]-Protokoll, und Endpunkte werden niemals für unverschlüsselte/unsichere Kanäle verfügbar gemacht.

## <a name="custom-endpoints"></a>Benutzerdefinierte Endpunkte

Sie können vorhandene Azure-Dienste in Ihrem Abonnement mit Ihrem IoT-Hub verknüpfen, die als Endpunkte für das Nachrichtenrouting fungieren. Diese Endpunkte fungieren als Dienstendpunkte und werden als Senken für Nachrichtenrouten verwendet. Geräte können nicht direkt auf die zusätzlichen Endpunkte schreiben. Weitere Informationen zu Nachrichtenrouten finden Sie im Entwicklerhandbuch im Eintrag zum [Senden und Empfangen von Nachrichten mit IoT Hub][lnk-devguide-messaging].

IoT Hub unterstützt derzeit folgende Azure-Dienste als zusätzliche Endpunkte:

* Event Hubs
* Service Bus-Warteschlangen
* Service Bus-Themen

IoT Hub benötigt Schreibzugriff auf diese Dienstendpunkte, damit das Nachrichtenrouting funktioniert. Wenn Sie Ihre Endpunkte über das Azure-Portal konfigurieren, werden die erforderlichen Berechtigungen für Sie hinzugefügt. Stellen Sie sicher, dass Sie Ihre Dienste zur Unterstützung des erwarteten Durchsatzes konfigurieren. Nach der Erstkonfiguration Ihrer IoT-Lösung müssen Sie möglicherweise Ihre zusätzlichen Endpunkte überwachen und ggf. Anpassungen an die tatsächliche Last vornehmen.

Wenn eine Nachricht mehreren Routen entspricht, die alle auf den gleichen Endpunkt verweisen, übermittelt IoT Hub die Nachricht nur einmal an diesen Endpunkt. Aus diesem Grund müssen Sie nicht die Deduplizierung für Ihre Service Bus-Warteschlange oder ein Thema konfigurieren. In partitionierten Warteschlangen garantiert die Partitionsaffinität die Nachrichtensortierung.

> [!NOTE]
> Für Service Bus-Warteschlangen und -Themen, die als IoT Hub-Endpunkte verwendet werden, dürfen **Sitzungen** oder **Duplikaterkennung** nicht aktiviert werden. Wenn eine dieser Optionen aktiviert ist, wird der Endpunkt im Azure-Portal als **Nicht erreichbar** angezeigt.

Informationen zur Beschränkung der Anzahl von Endpunkten, die Sie hinzufügen können, finden Sie unter [Kontingente und Drosselung][lnk-devguide-quotas].

## <a name="field-gateways"></a>Bereichsgateways

Bei einer IoT-Lösung ist zwischen Ihren Geräten und IoT Hub-Endpunkten ein *Bereichsgateway* angeordnet. Es befindet sich normalerweise in der Nähe Ihrer Geräte. Ihre Geräte kommunizieren direkt mit dem Bereichsgateway, indem sie ein von den Geräten unterstütztes Protokoll nutzen. Das Bereichsgateway verbindet sich mit einem IoT Hub-Endpunkt über ein Protokoll, das von IoT Hub unterstützt wird. Bei einem Bereichsgateway kann es sich um ein dediziertes Hardwaregerät oder um einen energiesparenden Computer mit benutzerdefinierter Gatewaysoftware handeln.

Sie können [Azure IoT Edge][lnk-iot-edge] verwenden, um ein Bereichsgateway zu implementieren. IoT Edge ermöglicht es unter anderem, die Kommunikation mehrerer Geräte im Multiplexverfahren über die gleiche IoT Hub-Verbindung zu übertragen.

## <a name="next-steps"></a>Nächste Schritte

Weitere Referenzthemen in diesem IoT Hub-Entwicklungsleitfaden:

* [IoT Hub-Abfragesprache für Gerätezwillinge, Aufträge und Nachrichtenrouting][lnk-devguide-query]
* [Kontingente und Drosselung][lnk-devguide-quotas]
* [IoT Hub-MQTT-Unterstützung][lnk-devguide-mqtt]

[lnk-iot-edge]: https://github.com/Azure/iot-edge

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
[lnk-d2c]: iot-hub-devguide-messages-d2c.md
[lnk-device-identities]: iot-hub-devguide-identity-registry.md
[lnk-upload]: iot-hub-devguide-file-upload.md
[lnk-c2d]: iot-hub-devguide-messages-c2d.md
[lnk-methods]: iot-hub-devguide-direct-methods.md
[lnk-twins]: iot-hub-devguide-device-twins.md
[lnk-query]: iot-hub-devguide-query-language.md
[lnk-jobs]: iot-hub-devguide-jobs.md

[lnk-devguide-quotas]: iot-hub-devguide-quotas-throttling.md
[lnk-devguide-query]: iot-hub-devguide-query-language.md
[lnk-devguide-mqtt]: iot-hub-mqtt-support.md
[lnk-devguide-messaging]: iot-hub-devguide-messaging.md
[lnk-operations-mon]: iot-hub-operations-monitoring.md

