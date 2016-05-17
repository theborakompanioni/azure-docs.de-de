<properties
	pageTitle="Simulieren eines Geräts mit dem Gateway SDK | Microsoft Azure"
	description="Exemplarische Vorgehensweise für das Azure IoT Hub Gateway SDK unter Verwendung von Windows, um zu veranschaulichen, wie Telemetriedaten mithilfe des Azure IoT Hub Gateway SDK von einem simulierten Gerät gesendet werden."
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


# IoT Gateway SDK (Beta) – Senden von D2C-Nachrichten mit einem simulierten Gerät unter Windows

[AZURE.INCLUDE [iot-hub-gateway-sdk-simulated-selector](../../includes/iot-hub-gateway-sdk-simulated-selector.md)]

## Erstellen und Ausführen des Beispiels

Bevor Sie beginnen, müssen Sie Folgendes ausführen:

- [Richten Sie eine Entwicklungsumgebung ein][lnk-setupdevbox], um mit dem SDK unter Windows arbeiten zu können.
- [Erstellen Sie einen IoT Hub][lnk-create-hub] in Ihrem Azure-Abonnement. Sie benötigen den Namen des Hubs, um diese exemplarische Vorgehensweise abzuschließen. Wenn Sie nicht bereits über ein Azure-Abonnement verfügen, können Sie ein [kostenloses Konto erstellen][lnk-free-trial].
- Fügen Sie Ihrem IoT Hub zwei Geräte hinzu, und notieren Sie die IDs und Geräteschlüssel. Sie können die Tools [Geräte-Explorer oder iothub-explorer][lnk-explorer-tools] verwenden, um Geräte zu dem im vorherigen Schritt erstellten IoT Hub hinzuzufügen und die zugehörigen Schlüssel abzurufen.

So erstellen Sie das Beispiel:

1. Öffnen Sie eine **Developer-Eingabeaufforderung für VS2015**.
2. Navigieren Sie in Ihrer lokalen Kopie des Repositorys **azure-iot-gateway-sdk** zum Stammordner.
3. Führen Sie das Skript **tools\\build.cmd** aus. Dieses Skript erstellt eine Visual Studio-Projektmappendatei, erstellt die Projektmappe und führt die Tests aus. Sie finden die Visual Studio-Projektmappe im **build**-Ordner Ihrer lokalen Kopie des **azure-iot-gateway-sdk**-Repositorys.

So führen Sie das Beispiel aus:

Öffnen Sie die Datei **samples\\simulated\_device\_cloud\_upload\\src\\simulated\_device\_cloud\_upload\_win.json** in Ihrer lokalen Kopie des **azure-iot-gateway-sdk**-Repositorys in einem Texteditor. Diese Datei konfiguriert die Module im Beispielgateway:

- Das **IoTHub**-Modul stellt eine Verbindung mit Ihrem IoT Hub her. Sie müssen das Modul zum Senden von Daten an Ihren IoT Hub konfigurieren. Legen Sie insbesondere den **IoTHubName**-Wert auf den Namen Ihres IoT Hubs fest, und legen Sie den Wert von **IoTHubSuffix** auf **azure-devices.net** fest.
- Das **Zuordnungsmodul** ordnet die MAC-Adressen Ihrer simulierten Geräte zu den IoT Hub-Geräte-IDs zu. Stellen Sie sicher, dass die **deviceId**-Werte mit den IDs der beiden Geräte übereinstimmen, die Sie Ihrem IoT Hub hinzugefügt haben, und dass die **deviceKey**-Werte die Schlüssel der beiden Geräte enthalten.
- Bei den Modulen **BLE1** und **BLE2** handelt es sich um die simulierten Geräte. Beachten Sie, dass die MAC-Adressen mit den Werten im **Zuordnungsmodul** übereinstimmen.
- Das **Protokollierungsmodul** protokolliert die Aktivitäten Ihres Gateways in einer Datei.
- Bei den unten gezeigten **module path**-Werten wird angenommen, dass Sie das Gateway SDK-Repository in den Stammordner Ihres Laufwerks **C:** geklont haben. Wenn Sie das Repository in einen anderen Speicherort heruntergeladen haben, müssen Sie die **module path**-Werte entsprechend anpassen.

```
{
    "modules" :
    [ 
        {
            "module name" : "IoTHub",
            "module path" : "C:\\azure-iot-gateway-sdk\\modules\\iothub_http\\Debug\\iothub_http_hl.dll",
            "args" : 
            {
                "IoTHubName" : "{Your IoT hub name}",
                "IoTHubSuffix" : "azure-devices.net"
            }
        },
        {
            "module name" : "mapping",
            "module path" : "C:\\azure-iot-gateway-sdk\\modules\\mac_iothub_id_map\\Debug\\mac_iothub_id_map_hl.dll",
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
            "module path" : "C:\\azure-iot-gateway-sdk\\modules\\ble_fake\\Debug\\ble_fake_hl.dll",
            "args":
            {
                "macAddress" : "01-01-01-01-01-01"
            }
        },
        {
            "module name":"BLE2",
            "module path" : "C:\\azure-iot-gateway-sdk\\modules\\ble_fake\\Debug\\ble_fake_hl.dll",
            "args":
            {
                "macAddress" : "02-02-02-02-02-02"
            }
        },
        {
            "module name":"Logger",
            "module path" : "C:\\azure-iot-gateway-sdk\\modules\\logger\\Debug\\logger_hl.dll",
            "args":
            {
                "filename":"C:\\azure-iot-gateway-sdk\\deviceCloudUploadGatewaylog.log"
            }
        }
    ]
}
```

Speichern Sie alle Änderungen, die Sie an der Konfigurationsdatei vorgenommen haben.

So führen Sie das Beispiel aus:

1. Navigieren Sie an einer Eingabeaufforderung zum Stammordner in Ihrer lokalen Kopie des **azure-iot-gateway-sdk**-Repositorys.
2. Führen Sie den folgenden Befehl aus:
  
    ```
    build\samples\simulated_device_cloud_upload\Debug\simulated_device_cloud_upload_sample.exe samples\simulated_device_cloud_upload\src\simulated_device_cloud_upload_win.json
    ```

3. Sie können die Tools [Geräte-Explorer oder iothub-explorer][lnk-explorer-tools] verwenden, um die Nachrichten zu überwachen, die IoT Hub aus dem Gateway empfängt.


## Nächste Schritte

Informationen zur Verwendung des Gateway SDK finden Sie unter [Azure IoT Gateway SDK][lnk-gateway-sdk] auf GitHub.

<!-- Links -->
[lnk-setupdevbox]: https://github.com/Azure/azure-iot-gateway-sdk/blob/master/doc/devbox_setup.md
[lnk-create-hub]: iot-hub-manage-through-portal.md
[lnk-free-trial]: https://azure.microsoft.com/pricing/free-trial/
[lnk-explorer-tools]: https://github.com/Azure/azure-iot-sdks/blob/master/doc/manage_iot_hub.md
[lnk-gateway-sdk]: https://github.com/Azure/azure-iot-gateway-sdk/

<!---HONumber=AcomDC_0504_2016-->