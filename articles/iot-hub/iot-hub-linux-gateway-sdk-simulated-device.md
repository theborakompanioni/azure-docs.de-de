<properties
	pageTitle="Simulieren eines Geräts mit dem Gateway SDK | Microsoft Azure"
	description="Exemplarische Vorgehensweise für das Azure IoT Hub Gateway SDK unter Verwendung von Linux, um zu veranschaulichen, wie Telemetriedaten mithilfe des Azure IoT Hub Gateway SDK von einem simulierten Gerät gesendet werden."
	services="iot-hub"
	documentationCenter=""
	authors="chipalost"
	manager="timlt"
	editor=""/>

<tags
     ms.service="iot-hub"
     ms.devlang="cpp"
     ms.topic="article"
     ms.tgt_pltfrm="na"
     ms.workload="na"
     ms.date="04/20/2016"
     ms.author="cstreet"/>


# IoT Gateway SDK (Beta) – Senden von D2C-Nachrichten mit einem simulierten Gerät unter Linux

[AZURE.INCLUDE [iot-hub-gateway-sdk-simulated-selector](../../includes/iot-hub-gateway-sdk-simulated-selector.md)]

## Erstellen und Ausführen des Beispiels

Bevor Sie beginnen, müssen Sie Folgendes ausführen:

- [Richten Sie eine Entwicklungsumgebung ein][lnk-setupdevbox], um mit dem SDK unter Linux arbeiten zu können.
- [Erstellen Sie einen IoT Hub][lnk-create-hub] in Ihrem Azure-Abonnement. Sie benötigen den Namen des Hubs, um diese exemplarische Vorgehensweise abzuschließen. Wenn Sie nicht bereits über ein Azure-Abonnement verfügen, können Sie ein [kostenloses Konto erstellen][lnk-free-trial].
- Fügen Sie Ihrem IoT Hub zwei Geräte hinzu, und notieren Sie die IDs und Geräteschlüssel. Sie können die Tools [Geräte-Explorer oder iothub-explorer][lnk-explorer-tools] verwenden, um Geräte zu dem im vorherigen Schritt erstellten IoT Hub hinzuzufügen und die zugehörigen Schlüssel abzurufen.

So erstellen Sie das Beispiel:

1. Öffnen Sie eine Shell.
2. Navigieren Sie in Ihrer lokalen Kopie des Repositorys **azure-iot-gateway-sdk** zum Stammordner.
3. Führen Sie das Skript **tools/build.sh** aus. Dieses Skript verwendet das Hilfsprogramm **cmake**, um einen Ordner namens **build** im Stammordner Ihrer lokalen Kopie des **azure-iot-gateway-sdk**-Repositorys zu erstellen und ein Makefile zu generieren. Das Skript erstellt dann die Lösung und führt die Tests aus.

> [AZURE.NOTE]  Bei jeder Ausführung löscht das Skript **build.sh** den Ordner **build** im Stammordner Ihrer lokalen Kopie des **azure-iot-gateway-sdk**-Repositorys und erstellt ihn neu.

So führen Sie das Beispiel aus:

Öffnen Sie die Datei **samples/simulated\_device\_cloud\_upload/src/simulated\_device\_cloud\_upload\_lin.json** in Ihrer lokalen Kopie des **azure-iot-gateway-sdk**-Repositorys in einem Texteditor. Diese Datei konfiguriert die Module im Beispielgateway:

- Das **IoTHub**-Modul stellt eine Verbindung mit Ihrem IoT Hub her. Sie müssen das Modul zum Senden von Daten an Ihren IoT Hub konfigurieren. Legen Sie insbesondere den **IoTHubName**-Wert auf den Namen Ihres IoT Hubs fest, und legen Sie den Wert von **IoTHubSuffix** auf **azure-devices.net** fest.
- Das **Zuordnungsmodul** ordnet die MAC-Adressen Ihrer simulierten Geräte zu den IoT Hub-Geräte-IDs zu. Stellen Sie sicher, dass die **deviceId**-Werte mit den IDs der beiden Geräte übereinstimmen, die Sie Ihrem IoT Hub hinzugefügt haben, und dass die **deviceKey**-Werte die Schlüssel der beiden Geräte enthalten.
- Bei den Modulen **BLE1** und **BLE2** handelt es sich um die simulierten Geräte. Beachten Sie, dass die MAC-Adressen mit den Werten im **Zuordnungsmodul** übereinstimmen.
- Das **Protokollierungsmodul** protokolliert die Aktivitäten Ihres Gateways in einer Datei.
- Bei den unten gezeigten **module path**-Werten wird angenommen, dass Sie das Beispiel aus dem Stammordner Ihrer lokalen Kopie des **azure-iot-gateway-sdk**-Repositorys ausführen.

```
{
    "modules" :
    [ 
        {
            "module name" : "IoTHub",
            "module path" : "./build/modules/iothubhttp/libiothubhttp_hl.so",
            "args" : 
            {
                "IoTHubName" : "{Your IoT hub name}",
                "IoTHubSuffix" : "azure-devices.net"
            }
        },
        {
            "module name" : "mapping",
            "module path" : "./build/modules/identitymap/libidentitymap_hl.so",
            "args" : 
            [
                {
                    "macAddress" : "01-01-01-01-01-01",
                    "deviceId"   : "GW-ble1-demo",
                    "deviceKey"  : "{Device key}"
                },
                {
                    "macAddress" : "02-02-02-02-02-02",
                    "deviceId"   : "GW-ble2-demo",
                    "deviceKey"  : "{Device key}"
                }
            ]
        },
        {
            "module name":"BLE1",
            "module path" : "./build/modules/simulated_device/libsimulated_device_hl.so",
            "args":
            {
                "macAddress" : "01-01-01-01-01-01"
            }
        },
        {
            "module name":"BLE2",
            "module path" : "./build/modules/simulated_device/libsimulated_device_hl.so",
            "args":
            {
                "macAddress" : "02-02-02-02-02-02"
            }
        },
        {
            "module name":"Logger",
            "module path" : "./build/modules/logger/liblogger_hl.so",
            "args":
            {
                "filename":"./deviceCloudUploadGatewaylog.log"
            }
        }
    ]
}

```

Speichern Sie alle Änderungen, die Sie an der Konfigurationsdatei vorgenommen haben.

So führen Sie das Beispiel aus:

1. Navigieren Sie in der Shell zum Stammordner in Ihrer lokalen Kopie des **azure-iot-gateway-sdk**-Repositorys.
2. Führen Sie den folgenden Befehl aus:

    ```
    ./build/samples/simulated_device_cloud_upload/simulated_device_cloud_upload_sample ./samples/simulated_device_cloud_upload/src/simulated_device_cloud_upload_lin.json
    ```

3. Sie können die Tools [Geräte-Explorer oder iothub-explorer][lnk-explorer-tools] verwenden, um die Nachrichten zu überwachen, die IoT Hub aus dem Gateway empfängt.

## Nächste Schritte

Wenn Sie noch mehr über das Gateway SDK wissen und mit einigen Codebeispielen experimentieren möchten, sehen Sie sich die folgenden Tutorials und Ressourcen für Entwickler an:

- [Senden von Gerät-zu-Cloud-Nachrichten von einem echten Gerät mit dem Gateway SDK][lnk-physical-device]
- [Verwalten eines Gatewaygeräts][lnk-manage-devices]
- [Azure IoT Gateway SDK][lnk-gateway-sdk]

Weitere Informationen zu den Funktionen von IoT Hub finden Sie unter:

- [Entwerfen Ihrer Lösung][lnk-design]
- [Entwicklerhandbuch][lnk-devguide]
- [Erkunden der Geräteverwaltung mithilfe der Beispielbenutzeroberfläche][lnk-dmui]
- [Verwenden des Azure-Portals zur Verwaltung von IoT Hub][lnk-portal]
- [Schützen Ihrer IoT-Lösung von Grund auf][lnk-securing]

<!-- Links -->
[lnk-setupdevbox]: https://github.com/Azure/azure-iot-gateway-sdk/blob/master/doc/devbox_setup.md
[lnk-create-hub]: iot-hub-manage-through-portal.md
[lnk-free-trial]: https://azure.microsoft.com/pricing/free-trial/
[lnk-explorer-tools]: https://github.com/Azure/azure-iot-sdks/blob/master/doc/manage_iot_hub.md
[lnk-gateway-sdk]: https://github.com/Azure/azure-iot-gateway-sdk/

[lnk-physical-device]: iot-hub-gateway-sdk-physical-device.md
[lnk-manage-devices]: iot-hub-gateway-sdk-device-management.md

[lnk-design]: iot-hub-guidance.md
[lnk-devguide]: iot-hub-devguide.md
[lnk-dmui]: iot-hub-device-management-ui-sample.md
[lnk-portal]: iot-hub-manage-through-portal.md
[lnk-securing]: iot-hub-security-ground-up.md

<!---HONumber=AcomDC_0727_2016-->