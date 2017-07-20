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
ms.date: 06/09/2017
ms.author: andbuc
ms.translationtype: Human Translation
ms.sourcegitcommit: ff2fb126905d2a68c5888514262212010e108a3d
ms.openlocfilehash: e7eb2931993daf3f0aecbd4a43d27ebd5adc10b0
ms.contentlocale: de-de
ms.lasthandoff: 06/17/2017


---

# <a name="use-azure-iot-edge-to-send-device-to-cloud-messages-with-a-simulated-device-windows"></a>Einsatz von Azure IoT Edge zum Senden von D2C-Nachrichten mit einem simulierten Gerät (Windows)

[!INCLUDE [iot-hub-iot-edge-simulated-selector](../../includes/iot-hub-iot-edge-simulated-selector.md)]

[!INCLUDE [iot-hub-iot-edge-install-build-windows](../../includes/iot-hub-iot-edge-install-build-windows.md)]

## <a name="how-to-run-the-sample"></a>Ausführen des Beispiels

Das Skript **build.cmd** generiert die Ausgabe im Ordner **build** Ihrer lokalen Kopie des Repositorys **iot-edge**. Diese Ausgabe beinhaltet die beiden IoT Edge-Module aus diesem Beispiel.

Das Build-Skript platziert:

* **logger.dll** in dem Ordner **build\\modules\\logger\\Debug**.
* **iothub.dll** in dem Ordner **build\\modules\\iothub\\Debug**.
* **identity\_map.dll** in dem Ordner **build\\modules\\identitymap\\Debug**.
* **simulated\_device.dll** in dem Ordner **build\\modules\\simulated\_device\\Debug**.

Verwenden Sie diese Pfade für die Werte von **Modulpfad**, wie in der folgenden JSON-Einstellungsdatei gezeigt:

Der Prozess „simulated\_device\_cloud\_upload\_sample“ akzeptiert den Pfad zu einer JSON-Konfigurationsdatei als Befehlszeilenargument. Die folgende JSON-Beispieldatei wird im SDK-Repository unter **samples\\simulated\_device\_cloud\_upload\_sample\\src\\simulated\_device\_cloud\_upload\_sample\_win.json** bereitgestellt. Diese Konfigurationsdatei kann in der vorliegenden Form verwendet werden, wenn Sie das Buildskript nicht geändert haben, um die IoT Edge-Module oder ausführbare Beispieldateien an benutzerdefinierten Speicherorten zu speichern.

> [!NOTE]
> Die Modulpfade gelten relativ zu dem Verzeichnis, in dem sich die Datei simulated\_device\_cloud\_upload\_sample.exe befindet. Die JSON-Beispielkonfigurationsdatei schreibt standardmäßig in „deviceCloudUploadGatewaylog.log“ im aktuellen Arbeitsverzeichnis.

Öffnen Sie die Datei **samples\\simulated\_device\_cloud\_upload\_sample\\src\\simulated\_device\_cloud\_upload\_win.json** in Ihrer lokalen Kopie des Repositorys **iot-edge** in einem Texteditor. Diese Datei konfiguriert die IoT Edge-Module im Beispielgateway:

* Das **IoTHub** -Modul stellt eine Verbindung mit Ihrem IoT Hub her. Sie konfigurieren das Modul zum Senden von Daten an Ihren IoT Hub. Legen Sie insbesondere den Wert **IoTHubName** auf den Namen Ihres IoT Hubs und den Wert von **IoTHubSuffix** auf **azure-devices.net** fest. Legen Sie den Wert für den **Transport** auf eine der folgenden Optionen fest: **HTTP**, **AMQP** oder **MQTT**. Derzeit wird nur bei **HTTP** eine TCP-Verbindung für alle Gerätenachrichten gemeinsam genutzt. Wenn Sie den Wert auf **AMQP** oder **MQTT** festlegen, richtet das Gateway für jedes Gerät eine eigene TCP-Verbindung mit IoT Hub ein.
* Das Modul **Mapping** ordnet die MAC-Adressen Ihrer simulierten Geräte den IoT Hub-Geräte-IDs zu. Stellen Sie sicher, dass die **deviceId**-Werte mit den IDs der beiden Geräte übereinstimmen, die Sie Ihrem IoT Hub hinzugefügt haben, und dass die **deviceKey**-Werte die Schlüssel der beiden Geräte enthalten.
* Bei den Modulen **BLE1** und **BLE2** handelt es sich um die simulierten Geräte. Beachten Sie, dass die MAC-Adressen des Moduls mit den Adressen im Modul **Mapping** übereinstimmen.
* Das Modul **Logger** protokolliert die Aktivitäten Ihres Gateways in einer Datei.
* Die Werte für den **Modulpfad**, die im folgenden Beispiel dargestellt werden, gelten relativ zu dem Verzeichnis, in dem sich die Datei \_device\_cloud\_upload\_sample.exe befindet.
* Das Array **Links** unten in der JSON-Datei verbindet die Module **BLE1** und **BLE2** mit dem Modul **Mapping** und das Modul **Mapping** mit dem Modul **IoTHub**. Zusätzlich wird gewährleistet, dass alle Nachrichten vom Modul **Logger** protokolliert werden.

```json
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

Speichern Sie die Änderungen, die Sie an der Konfigurationsdatei vorgenommen haben.

So führen Sie das Beispiel aus:

1. Navigieren Sie an einer Eingabeaufforderung zum Ordner **build** in Ihrer lokalen Kopie des Repositorys **iot-edge**.
2. Führen Sie den folgenden Befehl aus:
   
    ```cmd
    samples\simulated_device_cloud_upload\Debug\simulated_device_cloud_upload_sample.exe ..\samples\simulated_device_cloud_upload\src\simulated_device_cloud_upload_win.json
    ```
3. Sie können die Tools [Device Explorer][lnk-device-explorer] oder [iothub-explorer][lnk-iothub-explorer] verwenden, um die Nachrichten zu überwachen, die IoT Hub vom Gateway empfängt. Mithilfe von „iothub-explorer“ können Sie beispielsweise Gerät-zu-Cloud-Nachrichten mit dem folgenden Befehl überwachen:

    ```cmd
    iothub-explorer monitor-events --login "HostName={Your iot hub name}.azure-devices.net;SharedAccessKeyName=iothubowner;SharedAccessKey={Your IoT Hub key}"
    ```

## <a name="next-steps"></a>Nächste Schritte

Sehen Sie sich die folgenden Tutorials und Ressourcen für Entwickler an, wenn Sie noch mehr über IoT Edge wissen und mit einigen Codebeispielen experimentieren möchten:

* [Senden von D2C-Nachrichten von einem physischen Gerät mit IoT Edge][lnk-physical-device]
* [Azure IoT Edge][lnk-iot-edge]

Weitere Informationen zu den Funktionen von IoT Hub finden Sie unter:

* [IoT Hub-Entwicklerhandbuch][lnk-devguide]
* [Schützen Ihrer IoT-Lösung von Grund auf][lnk-securing]

<!-- Links -->
[lnk-iot-edge]: https://github.com/Azure/iot-edge/
[lnk-physical-device]: iot-hub-iot-edge-physical-device.md
[lnk-devguide]: iot-hub-devguide.md
[lnk-securing]: iot-hub-security-ground-up.md
[lnk-device-explorer]: https://github.com/Azure/azure-iot-sdk-csharp/tree/master/tools/DeviceExplorer
[lnk-iothub-explorer]: https://github.com/Azure/iothub-explorer/blob/master/readme.md
