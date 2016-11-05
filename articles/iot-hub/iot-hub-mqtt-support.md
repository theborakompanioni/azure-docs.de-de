---
title: IoT Hub MQTT-Unterstützung | Microsoft Docs
description: Beschreibung der MQTT-Unterstützung auf IoT Hub-Ebene
services: iot-hub
documentationcenter: .net
author: dominicbetts
manager: timlt
editor: ''

ms.service: iot-hub
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/05/2016
ms.author: dobett

---
# <a name="iot-hub-mqtt-support"></a>IoT Hub MQTT-Unterstützung
IoT Hub ermöglicht Geräten mithilfe des Protokolls [MQTT v3.1.1][lnk-mqtt-org] die Kommunikation mit den IoT Hub-Geräteendpunkten an Port 8883. IoT Hub setzt voraus, dass die gesamte Gerätekommunikation mithilfe von TLS/SSL gesichert wird.

## <a name="connecting-to-iot-hub"></a>Herstellen einer Verbindung mit einem IoT Hub
Ein Gerät kann sich entweder über die Bibliotheken in den [Microsoft Azure IoT-SDKs][lnk-device-sdks] oder direkt über das Protokoll MQTT mit IoT Hub verbinden.

## <a name="using-the-device-client-sdks"></a>Verwenden der SDKs für Geräteclients
[SDKs für Geräteclients][lnk-device-sdks], die das MQTT-Protokoll unterstützen, stehen für Java, Node.js, C und C# zur Verfügung. Die SDKs für Geräteclients verwenden die standardmäßige IoT Hub-Verbindungszeichenfolge zum Herstellen einer Verbindung mit einem IoT Hub. Um das MQTT-Protokoll verwenden zu können, muss der Clientprotokollparameter auf **MQTT**festgelegt werden. Standardmäßig verbinden sich die SDKs von Geräteclients mit einem IoT Hub, indem das **CleanSession**-Flag auf **0** festgelegt und **QoS-1** für den Nachrichtenaustausch mit dem IoT Hub verwendet wird.

Wenn ein Gerät mit einem IoT Hub verbunden ist, bieten die SDKs von Geräteclients Methoden, die dem Gerät das Senden von Nachrichten an und Empfangen von Nachrichten von einem IoT Hub ermöglichen.

Die folgende Tabelle enthält Links zu Codebeispielen für jede unterstützte Sprache und gibt die Parameter zum Herstellen einer Verbindung mit einem IoT Hub über das Protokoll MQTT an.

| Sprache | „Protocol“-Parameter |
| --- | --- |
| [Node.js][lnk-sample-node] |azure-iot-device-mqtt |
| [Java][lnk-sample-java] |IotHubClientProtocol.MQTT |
| [C][lnk-sample-c] |MQTT_Protocol |
| [C#][lnk-sample-csharp] |TransportType.Mqtt |
| [Python][lnk-sample-python] |IoTHubTransportProvider.MQTT |

### <a name="migrate-a-device-app-from-amqp-to-mqtt"></a>Migrieren einer Geräte-App von AMQP zu MQTT
Bei Verwendung der [SDKs für Geräteclients][lnk-device-sdks] muss bei einem Wechsel von AMQP zu MQTT der Protokollparameter, wie oben erwähnt, in der Clientinitialisierung geändert werden.

Dabei sollten Sie die folgenden Punkte beachten:

* Bei AMQP werden Fehler für viele Bedingungen zurückgegeben, bei MQTT wird dagegen die Verbindung beendet. Daher müssen an der Ausnahmebehandlungslogik möglicherweise einige Änderungen vorgenommen werden.
* MQTT unterstützt beim Empfang von [C2D-Nachrichten][lnk-messaging] keine *reject*-Vorgänge. Wenn das Back-End eine Antwort von der Geräte-App erhalten muss, können Sie [direkte Methoden][lnk-method] verwenden.
* Derzeit ist MQTT über WebSockets nicht verfügbar.

## <a name="using-the-mqtt-protocol-directly"></a>Direktes Verwenden des Protokolls MQTT
Wenn ein Gerät die SDKs von Geräteclients nicht verwenden kann, lässt es sich dennoch mithilfe des Protokolls MQTT mit den öffentlichen Geräteendpunkten verbinden. Das Gerät muss im **CONNECT** -Paket die folgenden Werte verwenden:

* Verwenden Sie für das Feld **ClientId** die **deviceId**-Eigenschaft. 
* Verwenden Sie `{iothubhostname}/{device_id}` für das Feld **Benutzername**, wobei {iothubhostname} der vollständige CNAME für den IoT Hub ist.
  
    Beispiel: Wenn der Name für den IoT Hub **contoso.azure devices.net** und der Name des Geräts **MyDevice01** lautet, sollte das vollständige Feld **Benutzername** den Namen `contoso.azure-devices.net/MyDevice01` enthalten.
* Verwenden Sie im Feld **Kennwort** ein SAS-Token. Das Format des SAS-Tokens ist das gleiche wie für die Protokolle HTTP und AMQP: <br/>`SharedAccessSignature sig={signature-string}&se={expiry}&sr={URL-encoded-resourceURI}`festgelegt werdenfestgelegt werden.
  
    Weitere Informationen zum Generieren von SAS-Token finden Sie unter [Verwenden von IoT Hub-Sicherheitstoken][lnk-sas-tokens] im Abschnitt zu Geräten.
  
    Beim Testen können Sie auch das Tool [Geräte-Explorer][lnk-device-explorer] nutzen, um schnell ein SAS-Token zu generieren, das Sie kopieren und in Ihren eigenen Code einfügen können:
  
  1. Klicken Sie in Device Explorer auf die Registerkarte **Management** .
  2. Klicken Sie auf **SAS Token** (oben rechts).
  3. Wählen Sie unter **SASTokenForm** Ihr Gerät in der Dropdownliste **DeviceID** aus. Legen Sie Ihre **TTL**fest.
  4. Klicken Sie auf **Generieren** , um Ihr Token zu erstellen.
     
     Das generierte SAS-Token weist die folgende Struktur auf:   `HostName={your hub name}.azure-devices.net;DeviceId=javadevice;SharedAccessSignature=SharedAccessSignature sr={your hub name}.azure-devices.net%2fdevices%2fMyDevice01&sig=vSgHBMUG.....Ntg%3d&se=1456481802`.
     
     Der folgende Teil des Tokens wird im Feld **Kennwort** verwendet, um über MQTT eine Verbindung herzustellen:   `SharedAccessSignature sr={your hub name}.azure-devices.net%2fdevices%2fyDevice01&sig=vSgHBMUG.....Ntg%3d&se=1456481802g%3d&se=1456481802`.

Für die MQTT-Pakete CONNECT und DISCONNECT löst IoT Hub ein Ereignis im Kanal **Vorgangsüberwachung** aus.

### <a name="sending-messages-to-iot-hub"></a>Senden von Nachrichten an einen IoT Hub
Nachdem Sie erfolgreich eine Verbindung hergestellt haben, kann ein Gerät Nachrichten mithilfe von `devices/{device_id}/messages/events/` oder `devices/{device_id}/messages/events/{property_bag}` als **Themenname** an IoT Hub senden. Das `{property_bag}` -Element ermöglicht dem Gerät das Senden von Nachrichten mit zusätzlichen Eigenschaften in einem URL-codierten Format. Beispiel:

```
RFC 2396-encoded(<PropertyName1>)=RFC 2396-encoded(<PropertyValue1>)&RFC 2396-encoded(<PropertyName2>)=RFC 2396-encoded(<PropertyValue2>)…
```

> [!NOTE]
> Dieses `{property_bag}`-Element verwendet die gleiche Codierung wie bei Abfragezeichenfolgen im HTTP-Protokoll.
> 
> 

Die Geräteclientanwendung kann auch `devices/{device_id}/messages/events/{property_bag}` als **Will-Themennamen** verwenden, um festzulegen, dass *Will-Nachrichten* als Telemetrienachricht weitergeleitet werden.

### <a name="receiving-messages"></a>Empfangen von Nachrichten
Zum Empfangen von Nachrichten von einem IoT Hub muss ein Gerät ein Abonnement unter Verwendung von `devices/{device_id}/messages/devicebound/#”` als **Themenfilter** einrichten. IoT Hub sendet Nachrichten mit dem **Themennamen** `devices/{device_id}/messages/devicebound/` oder `devices/{device_id}/messages/devicebound/{property_bag}`, wenn Nachrichteneigenschaften vorhanden sind. `{property_bag}` enthält URL-codierte Schlüssel-Wert-Paare von Nachrichteneigenschaften. Nur Anwendungseigenschaften und vom Benutzer festlegbare Systemeigenschaften (z.B. **messageId** oder **correlationId**) sind im Eigenschaftenbehälter enthalten. Systemeigenschaftennamen haben das Präfix **$**, Anwendungseigenschaften verwenden den ursprünglichen Eigenschaftennamen ohne Präfix.

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen finden Sie unter [Hinweise zur MQTT-Unterstützung][lnk-mqtt-devguide] im Entwicklerhandbuch für Azure IoT Hub.

Weitere Informationen zum Protokoll MQTT finden Sie in der [MQTT-Dokumentation][lnk-mqtt-docs].

Weitere Informationen zum Planen Ihrer IoT Hub-Bereitstellung finden Sie unter:

* [Unterstützte Geräte][lnk-devices]
* [Unterstützen zusätzlicher Protokolle][lnk-protocols]
* [Vergleich mit Event Hubs][lnk-compare]
* [Skalierung, hohe Verfügbarkeit und Notfallwiederherstellung][lnk-scaling]

Weitere Informationen zu den Funktionen von IoT Hub finden Sie unter:

* [Entwicklerhandbuch][lnk-devguide]
* [Simulieren eines Geräts mit dem Gateway SDK][lnk-gateway]

[lnk-device-sdks]: https://github.com/Azure/azure-iot-sdks/blob/master/readme.md
[lnk-mqtt-org]: http://mqtt.org/
[lnk-mqtt-docs]: http://mqtt.org/documentation
[lnk-sample-node]: https://github.com/Azure/azure-iot-sdks/blob/develop/node/device/samples/simple_sample_device.js
[lnk-sample-java]: https://github.com/Azure/azure-iot-sdks/blob/develop/java/device/samples/send-receive-sample/src/main/java/samples/com/microsoft/azure/iothub/SendReceive.java
[lnk-sample-c]: https://github.com/Azure/azure-iot-sdks/tree/master/c/iothub_client/samples/iothub_client_sample_mqtt
[lnk-sample-csharp]: https://github.com/Azure/azure-iot-sdks/tree/master/csharp/device/samples
[lnk-sample-python]: https://github.com/Azure/azure-iot-sdks/tree/master/python/device/samples
[lnk-device-explorer]: https://github.com/Azure/azure-iot-sdks/blob/master/tools/DeviceExplorer/readme.md
[lnk-sas-tokens]: iot-hub-devguide-security.md#using-sas-tokens-as-a-device
[lnk-mqtt-devguide]: iot-hub-devguide-messaging.md#notes-on-mqtt-support

[lnk-devices]: iot-hub-tested-configurations.md
[lnk-protocols]: iot-hub-protocol-gateway.md
[lnk-compare]: iot-hub-compare-event-hubs.md
[lnk-scaling]: iot-hub-scaling.md
[lnk-devguide]: iot-hub-devguide.md
[lnk-gateway]: iot-hub-linux-gateway-sdk-simulated-device.md

[lnk-methods]: iot-hub-devguide-direct-methods.md
[lnk-messaging]: iot-hub-devguide-messaging.md



<!--HONumber=Oct16_HO2-->


