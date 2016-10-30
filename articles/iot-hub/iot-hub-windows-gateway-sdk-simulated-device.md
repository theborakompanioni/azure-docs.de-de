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
     ms.date="08/29/2016"
     ms.author="andbuc"/>



# <a name="iot-gateway-sdk-(beta)-–-send-device-to-cloud-messages-with-a-simulated-device-using-windows"></a>IoT Gateway SDK (Beta) – Senden von D2C-Nachrichten mit einem simulierten Gerät unter Windows

[AZURE.INCLUDE [iot-hub-gateway-sdk-simulated-selector](../../includes/iot-hub-gateway-sdk-simulated-selector.md)]

## <a name="build-and-run-the-sample"></a>Erstellen und Ausführen des Beispiels

Bevor Sie beginnen, müssen Sie Folgendes ausführen:

- [Richten Sie eine Entwicklungsumgebung ein][lnk-setupdevbox], um mit dem SDK unter Windows arbeiten zu können.
- [Erstellen Sie einen IoT Hub][lnk-create-hub] in Ihrem Azure-Abonnement. Der Name des Hubs wird später in dieser exemplarischen Vorgehensweise benötigt. Wenn Sie nicht bereits über ein Azure-Abonnement verfügen, können Sie ein [kostenloses Konto][lnk-free-trial] erstellen.
- Fügen Sie Ihrem IoT Hub zwei Geräte hinzu, und notieren Sie die IDs und Geräteschlüssel. Sie können die Tools [Geräte-Explorer oder iothub-explorer][lnk-explorer-tools] verwenden, um Geräte zu dem im vorherigen Schritt erstellten IoT Hub hinzuzufügen und die zugehörigen Schlüssel abzurufen.

So erstellen Sie das Beispiel:

1. Öffnen Sie eine **Developer-Eingabeaufforderung für VS2015** .
2. Navigieren Sie in Ihrer lokalen Kopie des Repositorys **azure-iot-gateway-sdk** zum Stammordner.
3. Führen Sie das Skript **tools\\build.cmd** aus. Dieses Skript erstellt eine Visual Studio-Projektmappendatei, erstellt die Projektmappe und führt die Tests aus. Sie finden die Visual Studio-Projektmappe im **build**-Ordner Ihrer lokalen Kopie des **azure-iot-gateway-sdk**-Repositorys.

So führen Sie das Beispiel aus:

Öffnen Sie die Datei **samples\\simulated_device_cloud_upload\\src\\simulated_device_cloud_upload_win.json** in Ihrer lokalen Kopie des **azure-iot-gateway-sdk**-Repositorys in einem Text-Editor. Diese Datei konfiguriert die Module im Beispielgateway:

- Das **IoTHub** -Modul stellt eine Verbindung mit Ihrem IoT Hub her. Sie müssen das Modul zum Senden von Daten an Ihren IoT Hub konfigurieren. Legen Sie insbesondere den Wert **IoTHubName** auf den Namen Ihres IoT Hubs und den Wert von **IoTHubSuffix** auf **azure-devices.net** fest. Legen Sie den Wert **Transport** auf eine der folgenden Optionen fest: HTTP, AMQP oder MQTT. Beachten Sie, dass derzeit nur bei „HTTP“ eine TCP-Verbindung für alle Gerätenachrichten gemeinsam genutzt wird. Wenn Sie den Wert auf „AMQP“ oder „MQTT“ festlegen, richtet das Gateway für jedes Gerät eine eigene TCP-Verbindung mit dem IoT Hub ein.
- Das Modul **Mapping** ordnet die MAC-Adressen Ihrer simulierten Geräte den IoT Hub-Geräte-IDs zu. Stellen Sie sicher, dass die **deviceId**-Werte mit den IDs der beiden Geräte übereinstimmen, die Sie Ihrem IoT Hub hinzugefügt haben, und dass die **deviceKey**-Werte die Schlüssel der beiden Geräte enthalten.
- Bei den Modulen **BLE1** und **BLE2** handelt es sich um die simulierten Geräte. Beachten Sie, dass die MAC-Adressen mit den Werten im Modul **Mapping** übereinstimmen.
- Das Modul **Logger** protokolliert die Aktivitäten Ihres Gateways in einer Datei.
- Bei den unten gezeigten **module path**-Werten wird davon ausgegangen, dass Sie das Gateway SDK-Repository in den Stammordner Ihres Laufwerks **C:** geklont haben. Wenn Sie das Repository an einen anderen Speicherort heruntergeladen haben, müssen Sie die **module path** -Werte entsprechend anpassen.
- Das Array **Links** unten in der JSON-Datei verbindet die Module **BLE1** und **BLE2** mit dem Modul **Mapping** und das Modul **Mapping** mit dem Modul **IoTHub**. Zusätzlich wird gewährleistet, dass alle Nachrichten vom Modul **Logger** protokolliert werden.

```
{
    "modules" :
    [ 
        {
            "module name" : "IoTHub",
            "module path" : "C:\\azure-iot-gateway-sdk\\build\\modules\\iothub\\Debug\\iothub_hl.dll",
            "args" : 
            {
                "IoTHubName" : "{Your IoT hub name}",
                "IoTHubSuffix" : "azure-devices.net",
                "Transport": "HTTP"
            }
        },
        {
            "module name" : "mapping",
            "module path" : "C:\\azure-iot-gateway-sdk\\build\\modules\\identitymap\\Debug\\identity_map_hl.dll",
            "args" : 
            [
                {
                    "macAddress" : "01-01-01-01-01-01",
                    "deviceId"   : "{Device ID 1}",
                    "deviceKey"  : "{Device key 1}"
                },
                {
                    "macAddress" : "02-02-02-02-02-02",
                    "deviceId"   : "{Device ID 2}",
                    "deviceKey"  : "{Device key 2}"
                }
            ]
        },
        {
            "module name":"BLE1",
            "module path" : "C:\\azure-iot-gateway-sdk\\build\\modules\\simulated_device\\Debug\\simulated_device_hl.dll",
            "args":
            {
                "macAddress" : "01-01-01-01-01-01"
            }
        },
        {
            "module name":"BLE2",
            "module path" : "C:\\azure-iot-gateway-sdk\\build\\modules\\simulated_device\\Debug\\simulated_device_hl.dll",
            "args":
            {
                "macAddress" : "02-02-02-02-02-02"
            }
        },
        {
            "module name":"Logger",
            "module path" : "C:\\azure-iot-gateway-sdk\\build\\modules\\logger\\Debug\\logger_hl.dll",
            "args":
            {
                "filename":"C:\\azure-iot-gateway-sdk\\deviceCloudUploadGatewaylog.log"
            }
        }
    ],
    "links" : [
        { "source" : "*", "sink" : "Logger" },
        { "source" : "BLE1", "sink" : "mapping" },
        { "source" : "BLE2", "sink" : "mapping" },
        { "source" : "mapping", "sink" : "IoTHub" }
    ]
}
```

Speichern Sie alle Änderungen, die Sie an der Konfigurationsdatei vorgenommen haben.

So führen Sie das Beispiel aus:

1. Navigieren Sie an einer Eingabeaufforderung zum Stammordner in Ihrer lokalen Kopie des Repositorys **azure-iot-gateway-sdk** .
2. Führen Sie den folgenden Befehl aus:
  
    ```
    build\samples\simulated_device_cloud_upload\Debug\simulated_device_cloud_upload_sample.exe samples\simulated_device_cloud_upload\src\simulated_device_cloud_upload_win.json
    ```

3. Sie können die Tools [Geräte-Explorer oder iothub-explorer][lnk-explorer-tools] verwenden, um die Nachrichten zu überwachen, die IoT Hub vom Gateway empfängt.


## <a name="next-steps"></a>Nächste Schritte

Wenn Sie noch mehr über das Gateway SDK wissen und mit einigen Codebeispielen experimentieren möchten, sehen Sie sich die folgenden Tutorials und Ressourcen für Entwickler an:

- [Senden von Device-to-Cloud-Nachrichten von einem echten Gerät mit dem Gateway-SDK][lnk-physical-device]
- [Azure IoT-Gateway-SDK][lnk-gateway-sdk]

Weitere Informationen zu den Funktionen von IoT Hub finden Sie unter:

- [Entwicklerhandbuch][lnk-devguide]

<!-- Links -->
[lnk-setupdevbox]: https://github.com/Azure/azure-iot-gateway-sdk/blob/master/doc/devbox_setup.md
[lnk-free-trial]: https://azure.microsoft.com/pricing/free-trial/
[lnk-explorer-tools]: https://github.com/Azure/azure-iot-sdks/blob/master/doc/manage_iot_hub.md
[lnk-gateway-sdk]: https://github.com/Azure/azure-iot-gateway-sdk/

[lnk-physical-device]: iot-hub-gateway-sdk-physical-device.md

[lnk-devguide]: ./iot-hub-devguide.md
[lnk-create-hub]: iot-hub-create-through-portal.md 


<!--HONumber=Oct16_HO2-->


