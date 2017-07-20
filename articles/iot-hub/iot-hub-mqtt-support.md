---
title: "Grundlegendes zur Azure IoT Hub MQTT-Unterstützung | Microsoft-Dokumentation"
description: "Entwicklerhandbuch: Unterstützung für Geräte, die unter Verwendung des MQTT-Protokolls eine Verbindung mit einem geräteseitigen IoT Hub-Endpunkt herstellen. Enthält Informationen zur integrierten MQTT-Unterstützung der Azure IoT-Geräte-SDKs."
services: iot-hub
documentationcenter: .net
author: kdotchkoff
manager: timlt
editor: 
ms.assetid: 1d71c27c-b466-4a40-b95b-d6550cf85144
ms.service: iot-hub
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/11/2017
ms.author: kdotchko
ms.custom: H1Hack27Feb2017
ms.translationtype: Human Translation
ms.sourcegitcommit: 5edc47e03ca9319ba2e3285600703d759963e1f3
ms.openlocfilehash: 886bf3ce3979b7ef52ca29b7731562c5768596a2
ms.contentlocale: de-de
ms.lasthandoff: 05/31/2017

---
# <a name="communicate-with-your-iot-hub-using-the-mqtt-protocol"></a>Kommunikation mit Ihrem IoT Hub mithilfe des Protokolls MQTT

IoT Hub ermöglicht Geräten mithilfe des Protokolls [MQTT v3.1.1][lnk-mqtt-org] an Port 8883 oder MQTT v3.1.1 über WebSocket an Port 443 die Kommunikation mit den IoT Hub-Geräteendpunkten. IoT Hub setzt voraus, dass die gesamte Gerätekommunikation mithilfe von TLS/SSL geschützt wird (weshalb IoT Hub keine ungeschützten Verbindungen über Port 1883 unterstützt).

## <a name="connecting-to-iot-hub"></a>Herstellen einer Verbindung mit einem IoT Hub

Ein Gerät kann sich entweder über die Bibliotheken in den [Azure IoT SDKs][lnk-device-sdks] oder direkt über das Protokoll MQTT mit IoT Hub verbinden.

## <a name="using-the-device-sdks"></a>Verwenden der Geräte-SDKs

[Geräte-SDKs][lnk-device-sdks], die das MQTT-Protokoll unterstützen, stehen für Java, Node.js, C, C# und Python zur Verfügung. Die SDKs für Geräte verwenden die IoT Hub-Standard-Verbindungszeichenfolge zum Herstellen einer Verbindung mit einem IoT Hub. Um das MQTT-Protokoll verwenden zu können, muss der Clientprotokollparameter auf **MQTT**festgelegt werden. Standardmäßig verbinden sich die SDKs von Geräten mit einem IoT Hub, indem das **CleanSession**-Flag auf **0** festgelegt und **QoS 1** für den Nachrichtenaustausch mit dem IoT Hub verwendet wird.

Wenn ein Gerät mit einem IoT Hub verbunden ist, werden mit den SDKs von Geräten Methoden bereitgestellt, die dem Gerät das Senden von Nachrichten an und Empfangen von Nachrichten von einem IoT Hub ermöglichen.

Die folgende Tabelle enthält Links zu Codebeispielen für jede unterstützte Sprache und gibt die Parameter zum Herstellen einer Verbindung mit einem IoT Hub über das Protokoll MQTT an.

| Sprache | „Protocol“-Parameter |
| --- | --- |
| [Node.js][lnk-sample-node] |azure-iot-device-mqtt |
| [Java][lnk-sample-java] |IotHubClientProtocol.MQTT |
| [C][lnk-sample-c] |MQTT_Protocol |
| [C#][lnk-sample-csharp] |TransportType.Mqtt |
| [Python][lnk-sample-python] |IoTHubTransportProvider.MQTT |

### <a name="migrating-a-device-app-from-amqp-to-mqtt"></a>Migrieren einer Geräte-App von AMQP zu MQTT

Bei Verwendung der [Geräte-SDKs][lnk-device-sdks] muss bei einem Wechsel von AMQP zu MQTT der Protokollparameter, wie bereits erwähnt, in der Clientinitialisierung geändert werden.

Dabei sollten Sie die folgenden Punkte beachten:

* Bei AMQP werden Fehler für viele Bedingungen zurückgegeben, bei MQTT wird dagegen die Verbindung beendet. Daher müssen an der Ausnahmebehandlungslogik möglicherweise einige Änderungen vorgenommen werden.
* MQTT unterstützt beim Empfang von [Cloud-zu-Gerät-Nachrichten][lnk-messaging] keine *reject*-Vorgänge. Wenn Ihre Back-End-App eine Antwort von der Geräte-App erhalten muss, können Sie [direkte Methoden][lnk-methods] verwenden.

## <a name="using-the-mqtt-protocol-directly"></a>Direktes Verwenden des Protokolls MQTT
Wenn ein Gerät die SDKs von Geräten nicht verwenden kann, lässt es sich dennoch mithilfe des Protokolls MQTT über den Port 8883 mit den öffentlichen Geräteendpunkten verbinden. Das Gerät muss im **CONNECT** -Paket die folgenden Werte verwenden:

* Verwenden Sie für das Feld **ClientId** die **deviceId**-Eigenschaft.
* Verwenden Sie `{iothubhostname}/{device_id}/api-version=2016-11-14` für das Feld **Benutzername**, wobei {iothubhostname} der vollständige CNAME für den IoT Hub ist.

    Beispiel: Wenn der Name für den IoT Hub **contoso.azure devices.net** und der Name des Geräts **MyDevice01** lautet, sollte das vollständige Feld **Benutzername** den Namen `contoso.azure-devices.net/MyDevice01/api-version=2016-11-14` enthalten.
* Verwenden Sie im Feld **Kennwort** ein SAS-Token. Das Format des SAS-Tokens ist das gleiche wie für die Protokolle HTTP und AMQP: <br/>`SharedAccessSignature sig={signature-string}&se={expiry}&sr={URL-encoded-resourceURI}`.

    Weitere Informationen zum Generieren von SAS-Token finden Sie unter [Verwenden von IoT Hub-Sicherheitstoken][lnk-sas-tokens] im Abschnitt zu Geräten.

    Beim Testen können Sie auch das Tool [Device Explorer][lnk-device-explorer] nutzen, um schnell ein SAS-Token zu generieren, das Sie kopieren und in Ihren eigenen Code einfügen können:

  1. Klicken Sie in **Device Explorer** auf die Registerkarte **Verwaltung**.
  2. Klicken Sie auf **SAS Token** (oben rechts).
  3. Wählen Sie unter **SASTokenForm** Ihr Gerät in der Dropdownliste **DeviceID** aus. Legen Sie Ihre **TTL**fest.
  4. Klicken Sie auf **Generieren** , um Ihr Token zu erstellen.

     Das generierte SAS-Token weist die folgende Struktur auf: `HostName={your hub name}.azure-devices.net;DeviceId=javadevice;SharedAccessSignature=SharedAccessSignature sr={your hub name}.azure-devices.net%2Fdevices%2FMyDevice01%2Fapi-version%3D2016-11-14&sig=vSgHBMUG.....Ntg%3d&se=1456481802`.

     Der folgende Teil des Tokens wird im Feld **Kennwort** verwendet, um über MQTT eine Verbindung herzustellen: `SharedAccessSignature sr={your hub name}.azure-devices.net%2Fdevices%2FMyDevice01%2Fapi-version%3D2016-11-14&sig=vSgHBMUG.....Ntg%3d&se=1456481802`.

Für die MQTT-Pakete CONNECT und DISCONNECT löst IoT Hub ein Ereignis im Kanal **Vorgangsüberwachung** aus, das zusätzliche Informationen liefert, mit deren Hilfe Sie Verbindungsprobleme beheben können.

### <a name="sending-device-to-cloud-messages"></a>Senden von D2C-Nachrichten

Nachdem Sie erfolgreich eine Verbindung hergestellt haben, kann ein Gerät Nachrichten mithilfe von `devices/{device_id}/messages/events/` oder `devices/{device_id}/messages/events/{property_bag}` als **Themenname** an IoT Hub senden. Das `{property_bag}` -Element ermöglicht dem Gerät das Senden von Nachrichten mit zusätzlichen Eigenschaften in einem URL-codierten Format. Beispiel:

```
RFC 2396-encoded(<PropertyName1>)=RFC 2396-encoded(<PropertyValue1>)&RFC 2396-encoded(<PropertyName2>)=RFC 2396-encoded(<PropertyValue2>)…
```

> [!NOTE]
> Dieses `{property_bag}`-Element verwendet die gleiche Codierung wie bei Abfragezeichenfolgen im HTTP-Protokoll.
>
>

Die Geräte-App kann auch `devices/{device_id}/messages/events/{property_bag}` als **Will-Themennamen** verwenden, um festzulegen, dass *Will-Nachrichten* als Telemetrienachricht weitergeleitet werden sollen.

- IoT Hub unterstützt keine QoS 2-Nachrichten. Wenn eine Geräte-App eine Nachricht mit **QoS 2**veröffentlicht, schließt IoT Hub die Netzwerkverbindung.
- IoT Hub speichert Beibehaltungsnachrichten („Retain“) nicht beständig. Wenn ein Geräte eine Nachricht mit auf 1 festgelegtem **RETAIN**-Flag sendet, fügt IoT Hub der Nachricht die Anwendungseigenschaft **x-opt-retain** hinzu. In diesem Fall speichert IoT Hub die Beibehaltungsnachricht nicht beständig, sondern übergibt sie an die Back-End-App.
- IoT Hub unterstützt nur eine aktive MQTT-Verbindung pro Gerät. Jede neue MQTT-Verbindung für die gleiche Geräte-ID bewirkt, dass IoT Hub die vorhandene Verbindung löscht.

Weitere Informationen finden Sie im [Entwicklerhandbuch zum Messaging][lnk-messaging].

### <a name="receiving-cloud-to-device-messages"></a>Empfangen von C2D-Nachrichten

Zum Empfangen von Nachrichten von einem IoT Hub muss ein Gerät ein Abonnement unter Verwendung von `devices/{device_id}/messages/devicebound/#` als **Themenfilter** einrichten. Der Platzhalter mit mehreren Ebenen **#** im Themenfilter wird nur verwendet, um dem Gerät das Empfangen zusätzlicher Eigenschaften im Themennamen zu erlauben. IoT Hub lässt sich nicht die Verwendung der Platzhalter **#** und **?** für die Filterung von Unterthemen zu. Da IoT Hub kein allgemeiner Nachrichtenbrokerdienst für das Veröffentlichen und Abonnieren ist, werden nur die dokumentierten Themennamen und -filter unterstützt.

Das Gerät empfängt erst Nachrichten von der IoT Hub-Instanz, nachdem es deren gerätespezifischen Endpunkt erfolgreich abonniert hat, der vom Themenfilter `devices/{device_id}/messages/devicebound/#` dargestellt wird. Nachdem das Abonnement erfolgreich eingerichtet wurde, beginnt das Gerät mit dem Empfangen von ausschließlich C2D-Nachrichten, die nach der Zeit des Abonnements an das Gerät gesendet wurden. Wenn das Gerät eine Verbindung mit auf **0** festgelegtem **CleanSession**-Flag herstellt, behält das Abonnement verschiedene Sitzungen übergreifend bei. In diesem Fall empfängt das Gerät beim nächsten Verbindungsaufbau mit **CleanSession 0** ausstehende Nachrichten, die ihm gesendet wurden, als es vom Netzwerk getrennt war. Wenn das Gerät das auf **1** festgelegte **CleanSession**-Flag verwendet, empfängt es erst dann Nachrichten von der IoT Hub-Instanz, wenn es deren Geräteendpunkt abonniert.

IoT Hub sendet Nachrichten mit dem **Themennamen** `devices/{device_id}/messages/devicebound/` oder `devices/{device_id}/messages/devicebound/{property_bag}`, wenn Nachrichteneigenschaften vorhanden sind. `{property_bag}` enthält URL-codierte Schlüssel-Wert-Paare von Nachrichteneigenschaften. Nur Anwendungseigenschaften und vom Benutzer festlegbare Systemeigenschaften (z.B. **messageId** oder **correlationId**) sind im Eigenschaftenbehälter enthalten. Systemeigenschaftennamen haben das Präfix **$**, Anwendungseigenschaften verwenden den ursprünglichen Eigenschaftennamen ohne Präfix.

Wenn eine Geräte-App ein Thema mit **QoS 2** abonniert, gewährt IoT Hub im **SUBACK**-Paket maximal die QoS-Ebene 1. Danach übermittelt IoT Hub mithilfe von QoS 1 Nachrichten an das Gerät.

### <a name="retrieving-a-device-twins-properties"></a>Abrufen der Eigenschaften eines Gerätezwillings

Als Erstes abonniert ein Gerät `$iothub/twin/res/#`, um die Antworten des Vorgangs zu erhalten. Anschließend wird eine leere Nachricht an das Thema `$iothub/twin/GET/?$rid={request id}` gesendet, wobei der Wert für **request id** ausgefüllt ist. Als Nächstes sendet der Dienst eine Antwortnachricht mit den Daten des Gerätezwillings im Thema `$iothub/twin/res/{status}/?$rid={request id}`, indem die gleiche **request id** wie für die Anforderung verwendet wird.

Die Anforderungs-ID (request id) kann ein beliebiger gültiger Wert für den Eigenschaftswert einer Nachricht sein (siehe [Entwicklerhandbuch zum IoT Hub-Messaging][lnk-messaging]). Der Status wird als ganze Zahl validiert.
Der Text der Antwort enthält den Abschnitt mit den Eigenschaften des Gerätezwillings:

Der Text des Eintrags in der Identitätsregistrierung ist auf den Member „properties“ beschränkt, z.B.:

        {
            "properties": {
                "desired": {
                    "telemetrySendFrequency": "5m",
                    "$version": 12
                },
                "reported": {
                    "telemetrySendFrequency": "5m",
                    "batteryLevel": 55,
                    "$version": 123
                }
            }
        }

Die möglichen Statuscodes lauten:

|Status | Beschreibung |
| ----- | ----------- |
| 200 | Erfolgreich |
| 429 | Zu viele Anforderungen (gedrosselt), siehe [IoT Hub-Drosselung][lnk-quotas] |
| 5** | Serverfehler |

Weitere Informationen finden Sie im [Entwicklerhandbuch zu Gerätezwillingen][lnk-devguide-twin].

### <a name="update-device-twins-reported-properties"></a>Aktualisieren der gemeldeten Eigenschaften des Gerätezwillings

Die folgende Sequenz beschreibt, wie ein Gerät die gemeldeten Eigenschaften in dem Gerätezwilling in IoT Hub aktualisiert:

1. Ein Gerät muss zuerst das Thema `$iothub/twin/res/#` abonnieren, um die Antworten des Vorgangs von IoT Hub zu empfangen.

1. Ein Gerät sendet eine Nachricht, die das Gerätezwillingsupdate enthält, an das Thema `$iothub/twin/PATCH/properties/reported/?$rid={request id}`. Diese Nachricht enthält einen **Anforderungs-ID**-Wert.

1. Anschließend sendet der Dienst eine Antwortnachricht, die den neuen ETag-Wert für die Sammlung der gemeldeten Eigenschaften enthält, im Thema `$iothub/twin/res/{status}/?$rid={request id}`. Diese Antwortnachricht verwendet die gleiche **Anforderungs-ID** wie die Anforderung.

Der Text der Antwortnachricht enthält ein JSON-Dokument, mit dem neue Werte für die gemeldeten Eigenschaften bereitgestellt werden (keine anderen Eigenschaftsdaten oder Metadaten können geändert werden).
Jeder Member im JSON-Dokument wird aktualisiert, oder der entsprechende Member wird im Dokument des Gerätezwillings hinzugefügt. Wenn ein Member auf `null` festgelegt ist, wird der Member aus dem enthaltenden Objekt gelöscht. Beispiel:

        {
            "telemetrySendFrequency": "35m",
            "batteryLevel": 60
        }

Die möglichen Statuscodes lauten:

|Status | Beschreibung |
| ----- | ----------- |
| 200 | Erfolgreich |
| 400 | Ungültige Anforderung; falsch formatierter JSON-Code |
| 429 | Zu viele Anforderungen (gedrosselt), siehe [IoT Hub-Drosselung][lnk-quotas] |
| 5** | Serverfehler |

Weitere Informationen finden Sie im [Entwicklerhandbuch zu Gerätezwillingen][lnk-devguide-twin].

### <a name="receiving-desired-properties-update-notifications"></a>Empfangen von Aktualisierungsbenachrichtigungen für gewünschte Eigenschaften

Wenn die Verbindung für ein Gerät hergestellt wird, sendet IoT Hub Benachrichtigungen an das Thema `$iothub/twin/PATCH/properties/desired/?$version={new version}`. Die Benachrichtigungen enthalten den Inhalt der Aktualisierung, die vom Lösungs-Back-End durchgeführt wird. Beispiel:

        {
            "telemetrySendFrequency": "5m",
            "route": null
        }

Für Aktualisierungen von Eigenschaften bedeutet die Angabe von `null` für die Werte, dass der JSON-Objektmember gelöscht wird.


> [!IMPORTANT] 
> IoT Hub generiert Änderungsbenachrichtigungen nur, wenn Geräte verbunden sind. Achten Sie darauf, den [Ablauf zur Wiederherstellung der Geräteverbindung][lnk-devguide-twin-reconnection] zu implementieren, um für die gewünschten Eigenschaften die Synchronisierung zwischen IoT Hub und der Geräte-App aufrechtzuerhalten.

Weitere Informationen finden Sie im [Entwicklerhandbuch zu Gerätezwillingen][lnk-devguide-twin].

### <a name="respond-to-a-direct-method"></a>Antworten auf eine direkte Methode

Als Erstes muss ein Gerät `$iothub/methods/POST/#` abonnieren. IoT Hub sendet Methodenanforderungen an das Thema `$iothub/methods/POST/{method name}/?$rid={request id}`, die entweder gültigen JSON-Code oder leeren Text enthalten.

Als Antwort sendet das Gerät eine Nachricht mit gültigem JSON-Code oder leerem Text an das Thema `$iothub/methods/res/{status}/?$rid={request id}`, wobei die **request id** mit der ID in der Anforderungsnachricht übereinstimmen und **status** eine ganze Zahl sein muss.

Weitere Informationen finden Sie im [Entwicklerhandbuch zu direkten Methoden][lnk-methods].

### <a name="additional-considerations"></a>Zusätzliche Überlegungen

Wenn Sie das Verhalten des Protokolls MQTT auf Cloudseite anpassen möchten, sollte schließlich noch der Einsatz eines [Azure IoT-Protokollgateways][lnk-azure-protocol-gateway] erwogen werden, das die Bereitstellung eines hochleistungsfähigen benutzerdefinierten Protokollgateways mit einer direkten Schnittstelle zu IoT Hub ermöglicht. Das Azure IoT-Protokollgateway dient zum Anpassen des Geräteprotokolls zum Unterstützen von Brownfield MQTT-Bereitstellungen oder anderer benutzerdefinierter Protokolle. Dieser Ansatz setzt jedoch voraus, dass Sie ein benutzerdefiniertes Protokollgateway ausführen und betreiben.

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zum Protokoll MQTT finden Sie in der [MQTT-Dokumentation][lnk-mqtt-docs].

Weitere Informationen zum Planen Ihrer IoT Hub-Bereitstellung finden Sie unter:

* [Katalog mit Azure Certified for IoT-Geräten][lnk-devices]
* [Unterstützen zusätzlicher Protokolle][lnk-protocols]
* [Vergleich mit Event Hubs][lnk-compare]
* [Skalierung, hohe Verfügbarkeit und Notfallwiederherstellung][lnk-scaling]

Weitere Informationen zu den Funktionen von IoT Hub finden Sie unter:

* [IoT Hub-Entwicklerhandbuch][lnk-devguide]
* [Simulieren eines Geräts mit Azure IoT Edge][lnk-iotedge]

[lnk-device-sdks]: https://github.com/Azure/azure-iot-sdks
[lnk-mqtt-org]: http://mqtt.org/
[lnk-mqtt-docs]: http://mqtt.org/documentation
[lnk-sample-node]: https://github.com/Azure/azure-iot-sdk-node/blob/master/device/samples/simple_sample_device.js
[lnk-sample-java]: https://github.com/Azure/azure-iot-sdk-java/tree/master/device/samples/send-receive-sample/src/main/java/samples/com/microsoft/azure/iothub/SendReceive.java
[lnk-sample-c]: https://github.com/Azure/azure-iot-sdk-c/tree/master/iothub_client/samples/iothub_client_sample_mqtt
[lnk-sample-csharp]: https://github.com/Azure/azure-iot-sdk-csharp/tree/master/device/samples
[lnk-sample-python]: https://github.com/Azure/azure-iot-sdk-python/tree/master/device/samples
[lnk-device-explorer]: https://github.com/Azure/azure-iot-sdk-csharp/blob/master/tools/DeviceExplorer
[lnk-sas-tokens]: iot-hub-devguide-security.md#use-sas-tokens-in-a-device-app
[lnk-azure-protocol-gateway]: iot-hub-protocol-gateway.md

[lnk-devices]: https://catalog.azureiotsuite.com/
[lnk-protocols]: iot-hub-protocol-gateway.md
[lnk-compare]: iot-hub-compare-event-hubs.md
[lnk-scaling]: iot-hub-scaling.md
[lnk-devguide]: iot-hub-devguide.md
[lnk-iotedge]: iot-hub-linux-iot-edge-simulated-device.md

[lnk-methods]: iot-hub-devguide-direct-methods.md
[lnk-messaging]: iot-hub-devguide-messaging.md

[lnk-quotas]: iot-hub-devguide-quotas-throttling.md
[lnk-devguide-twin-reconnection]: iot-hub-devguide-device-twins.md#device-reconnection-flow
[lnk-devguide-twin]: iot-hub-devguide-device-twins.md

