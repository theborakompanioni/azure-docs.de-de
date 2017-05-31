---
title: "Simulieren eines Geräts mit Azure IoT Edge (Windows) | Microsoft-Dokumentation"
description: "Erfahren Sie, wie Sie Azure IoT Edge unter Windows verwenden, um ein simuliertes Gerät zu erstellen, das Telemetriedaten über ein IoT Edge-Gateway an eine IoT Hub-Instanz sendet."
services: iot-hub
documentationcenter: 
author: chipalost
manager: timlt
editor: 
ms.assetid: 6a2aeda0-696a-4732-90e1-595d2e2fadc6
ms.service: iot-hub
ms.devlang: cpp
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/29/2017
ms.author: andbuc
ms.translationtype: Human Translation
ms.sourcegitcommit: e7da3c6d4cfad588e8cc6850143112989ff3e481
ms.openlocfilehash: 0ea8483e19ddec447642a33c24c9ecdd9937ea11
ms.contentlocale: de-de
ms.lasthandoff: 05/16/2017


---
# <a name="use-azure-iot-edge-to-send-device-to-cloud-messages-with-a-simulated-device-windows"></a>Einsatz von Azure IoT Edge zum Senden von D2C-Nachrichten mit einem simulierten Gerät (Windows)
[!INCLUDE [iot-hub-gateway-sdk-simulated-selector](../../includes/iot-hub-gateway-sdk-simulated-selector.md)]

## <a name="build-and-run-the-sample"></a>Erstellen und Ausführen des Beispiels
Bevor Sie beginnen, müssen Sie Folgendes ausführen:

* [Richten Sie eine Entwicklungsumgebung ein][lnk-setupdevbox], um mit dem SDK unter Windows arbeiten zu können.
* [Erstellen Sie einen IoT Hub][lnk-create-hub] in Ihrem Azure-Abonnement. Der Name des Hubs wird später in dieser exemplarischen Vorgehensweise benötigt. Wenn Sie nicht über ein Konto verfügen, können Sie in nur wenigen Minuten ein [kostenloses Konto][lnk-free-trial] erstellen.
* Fügen Sie Ihrem IoT Hub zwei Geräte hinzu, und notieren Sie die IDs und Geräteschlüssel. Sie können die Tools [Device Explorer][lnk-device-explorer] oder [iothub-explorer][lnk-iothub-explorer] verwenden, um Geräte zu dem im vorherigen Schritt erstellten IoT-Hub hinzuzufügen und die zugehörigen Schlüssel abzurufen.

So erstellen Sie das Beispiel:

1. Öffnen Sie eine **Entwicklereingabeaufforderung für VS 2015** oder eine **Entwicklereingabeaufforderung für VS 2017**.
2. Navigieren Sie zum Stammordner in Ihrer lokalen Kopie des Repositorys **iot-edge**.
3. Führen Sie das Skript **tools\\build.cmd** aus. Dieses Skript erstellt eine Visual Studio-Projektmappendatei und erstellt die Projektmappe. Sie finden die Visual Studio-Projektmappe im **build**-Ordner Ihrer lokalen Kopie des Repositorys **iot-edge**. Dem Skript können zur Erstellung und Ausführung von Komponenten- und End-to-End-Tests weitere Parameter hinzugefügt werden. Diese Parameter lauten **--run-unittests** bzw. **--run-e2e-tests**.

So führen Sie das Beispiel aus:

Öffnen Sie die Datei **samples\\simulated_device_cloud_upload\\src\\simulated_device_cloud_upload_win.json** in Ihrer lokalen Kopie des Repositorys **iot-edge** in einem Text-Editor. Diese Datei konfiguriert die IoT Edge-Module im Beispielgateway:

* Das **IoTHub** -Modul stellt eine Verbindung mit Ihrem IoT Hub her. Sie konfigurieren das Modul zum Senden von Daten an Ihren IoT Hub. Legen Sie insbesondere den Wert **IoTHubName** auf den Namen Ihres IoT Hubs und den Wert von **IoTHubSuffix** auf **azure-devices.net** fest. Legen Sie den Wert für **Transport** auf eine der folgenden Optionen fest: „HTTP“, „AMQP“ oder „MQTT“. Derzeit wird nur bei „HTTP“ eine TCP-Verbindung für alle Gerätenachrichten gemeinsam genutzt. Wenn Sie den Wert auf „AMQP“ oder „MQTT“ festlegen, richtet das Gateway für jedes Gerät eine eigene TCP-Verbindung mit IoT Hub ein.
* Das Modul **Mapping** ordnet die MAC-Adressen Ihrer simulierten Geräte den IoT Hub-Geräte-IDs zu. Stellen Sie sicher, dass die **deviceId**-Werte mit den IDs der beiden Geräte übereinstimmen, die Sie Ihrem IoT Hub hinzugefügt haben, und dass die **deviceKey**-Werte die Schlüssel der beiden Geräte enthalten.
* Bei den Modulen **BLE1** und **BLE2** handelt es sich um die simulierten Geräte. Beachten Sie, dass die MAC-Adressen des Moduls mit den Adressen im Modul **Mapping** übereinstimmen.
* Das Modul **Logger** protokolliert die Aktivitäten Ihres Gateways in einer Datei.
* Bei den im folgenden Beispiel gezeigten **module path**-Werten wird davon ausgegangen, dass Sie das IoT Edge-Repository in den Stammordner des Laufwerks **C:** geklont haben. Wenn Sie das Repository an einen anderen Speicherort heruntergeladen haben, müssen Sie die **module path** -Werte entsprechend anpassen.
* Das Array **Links** unten in der JSON-Datei verbindet die Module **BLE1** und **BLE2** mit dem Modul **Mapping** und das Modul **Mapping** mit dem Modul **IoTHub**. Zusätzlich wird gewährleistet, dass alle Nachrichten vom Modul **Logger** protokolliert werden.

```
{
    "modules" :
    [
      {
        "name": "IotHub",
        "loader": {
          "name": "native",
          "entrypoint": {
            "module.path": "..\\..\\..\\modules\\iothub\\Debug\\iothub.dll"
          }
          },
          "args": {
            "IoTHubName": "<<insert here IoTHubName>>",
            "IoTHubSuffix": "<<insert here IoTHubSuffix>>",
            "Transport": "HTTP"
          }
        },
      {
        "name": "mapping",
        "loader": {
          "name": "native",
          "entrypoint": {
            "module.path": "..\\..\\..\\modules\\identitymap\\Debug\\identity_map.dll"
          }
          },
          "args": [
            {
              "macAddress": "01:01:01:01:01:01",
              "deviceId": "<<insert here deviceId>>",
              "deviceKey": "<<insert here deviceKey>>"
            },
            {
              "macAddress": "02:02:02:02:02:02",
              "deviceId": "<<insert here deviceId>>",
              "deviceKey": "<<insert here deviceKey>>"
            }
          ]
        },
      {
        "name": "BLE1",
        "loader": {
          "name": "native",
          "entrypoint": {
            "module.path": "..\\..\\..\\modules\\simulated_device\\Debug\\simulated_device.dll"
          }
          },
          "args": {
            "macAddress": "01:01:01:01:01:01"
          }
        },
      {
        "name": "BLE2",
        "loader": {
          "name": "native",
          "entrypoint": {
            "module.path": "..\\..\\..\\modules\\simulated_device\\Debug\\simulated_device.dll"
          }
          },
          "args": {
            "macAddress": "02:02:02:02:02:02"
          }
        },
      {
        "name": "Logger",
        "loader": {
          "name": "native",
          "entrypoint": {
            "module.path": "..\\..\\..\\modules\\logger\\Debug\\logger.dll"
          }
        },
        "args": {
          "filename": "deviceCloudUploadGatewaylog.log"
        }
      }
    ],
    "links" : [
        { "source" : "*", "sink" : "Logger" },
        { "source" : "BLE1", "sink" : "mapping" },
        { "source" : "BLE2", "sink" : "mapping" },
        { "source" : "mapping", "sink" : "IotHub" }
    ]
}
```

Speichern Sie alle Änderungen, die Sie an der Konfigurationsdatei vorgenommen haben.

So führen Sie das Beispiel aus:

1. Navigieren Sie an einer Eingabeaufforderung zum Stammordner Ihrer lokalen Kopie des Repositorys **iot-edge**.
2. Führen Sie den folgenden Befehl aus:
   
    ```
    build\samples\simulated_device_cloud_upload\Debug\simulated_device_cloud_upload_sample.exe samples\simulated_device_cloud_upload\src\simulated_device_cloud_upload_win.json
    ```
3. Sie können die Tools [Device Explorer][lnk-device-explorer] oder [iothub-explorer][lnk-iothub-explorer] verwenden, um die Nachrichten zu überwachen, die IoT Hub vom Gateway empfängt.

## <a name="next-steps"></a>Nächste Schritte
Wenn Sie noch mehr über IoT Edge wissen und mit einigen Codebeispielen experimentieren möchten, sehen Sie sich die folgenden Tutorials und Ressourcen für Entwickler an:

* [Senden von D2C-Nachrichten von einem physischen Gerät mit IoT Edge][lnk-physical-device]
* [Azure IoT Edge][lnk-gateway-sdk]

Weitere Informationen zu den Funktionen von IoT Hub finden Sie unter:

* [IoT Hub-Entwicklerhandbuch][lnk-devguide]
* [Schützen Ihrer IoT-Lösung von Grund auf][lnk-securing]

<!-- Links -->
[lnk-setupdevbox]: https://github.com/Azure/iot-edge/blob/master/doc/devbox_setup.md
[lnk-free-trial]: https://azure.microsoft.com/pricing/free-trial/
[lnk-device-explorer]: https://github.com/Azure/azure-iot-sdk-csharp/tree/master/tools/DeviceExplorer
[lnk-iothub-explorer]: https://github.com/Azure/iothub-explorer/blob/master/readme.md
[lnk-gateway-sdk]: https://github.com/Azure/iot-edge/

[lnk-physical-device]: iot-hub-gateway-sdk-physical-device.md

[lnk-devguide]: iot-hub-devguide.md
[lnk-create-hub]: iot-hub-create-through-portal.md

