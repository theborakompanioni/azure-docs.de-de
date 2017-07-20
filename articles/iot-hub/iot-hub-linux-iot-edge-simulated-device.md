---
title: "Simulieren eines Geräts mit Azure IoT Edge (Linux) | Microsoft-Dokumentation"
description: "Erfahren Sie, wie Sie Azure IoT Edge unter Linux verwenden, um ein simuliertes Gerät zu erstellen, das Telemetriedaten über ein IoT Edge-Gateway an eine IoT Hub-Instanz sendet."
services: iot-hub
documentationcenter: 
author: chipalost
manager: timlt
editor: 
ms.assetid: 11e7bf28-ee3d-48d6-a386-eb506c7a31cf
ms.service: iot-hub
ms.devlang: cpp
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/09/2017
ms.author: andbuc
ms.translationtype: Human Translation
ms.sourcegitcommit: ff2fb126905d2a68c5888514262212010e108a3d
ms.openlocfilehash: 5349960373ae6815862c5f79a69dd6d5d9d624ab
ms.contentlocale: de-de
ms.lasthandoff: 06/17/2017


---

# <a name="use-azure-iot-edge-to-send-device-to-cloud-messages-with-a-simulated-device-linux"></a>Einsatz von Azure IoT Edge zum Senden von D2C-Nachrichten mit einem simulierten Gerät (Linux)

[!INCLUDE [iot-hub-iot-edge-simulated-selector](../../includes/iot-hub-iot-edge-simulated-selector.md)]

[!INCLUDE [iot-hub-iot-edge-install-build-linux](../../includes/iot-hub-iot-edge-install-build-linux.md)]

## <a name="how-to-run-the-sample"></a>Ausführen des Beispiels

Das Skript **build.sh** generiert die Ausgabe im Ordner **build** Ihrer lokalen Kopie des Repositorys **iot-edge**. Diese Ausgabe beinhaltet die vier IoT Edge-Module aus diesem Beispiel.

Das Build-Skript platziert:

* **liblogger.so** im Ordner **build/modules/logger**.
* **libiothub.so** im Ordner **build/modules/iothub**.
* **lib\_identity\_map.so** im Ordner **build/modules/identitymap**.
* **libsimulated\_device.so** im Ordner **build/modules/simulated\_device**.

Verwenden Sie diese Pfade für die Werte von **module path**, wie in der folgenden JSON-Einstellungsdatei gezeigt:

Der Prozess „simulated\_device\_cloud\_upload\_sample“ akzeptiert den Pfad zu einer JSON-Konfigurationsdatei als Befehlszeilenargument. Die folgende JSON-Beispieldatei wird im SDK-Repository unter **samples\\simulated\_device\_cloud\_upload\_sample\\src\\simulated\_device\_cloud\_upload\_sample\_lin.json** bereitgestellt. Diese Konfigurationsdatei kann in der vorliegenden Form verwendet werden – es sei denn, Sie haben das Buildskript geändert, um die IoT Edge-Module oder ausführbare Beispieldateien an benutzerdefinierten Speicherorten zu speichern.

> [!NOTE]
> Die Modulpfade sind relativ zum Verzeichnis, aus dem Sie die ausführbare Datei „simulated\_device\_cloud\_upload\_sample“ ausführen, nicht zum Verzeichnis, in dem sich die ausführbare Datei befindet. Die JSON-Beispielkonfigurationsdatei schreibt standardmäßig in „deviceCloudUploadGatewaylog.log“ im aktuellen Arbeitsverzeichnis.

Öffnen Sie die Datei **samples/simulated\_device\_cloud\_upload\_sample/src/simulated\_device\_cloud\_upload\_lin.json** in Ihrer lokalen Kopie des Repositorys **iot-edge** in einem Text-Editor. Diese Datei konfiguriert die IoT Edge-Module im Beispielgateway:

* Das **IoTHub** -Modul stellt eine Verbindung mit Ihrem IoT Hub her. Sie konfigurieren das Modul zum Senden von Daten an Ihren IoT Hub. Legen Sie insbesondere den Wert **IoTHubName** auf den Namen Ihres IoT Hubs und den Wert von **IoTHubSuffix** auf **azure-devices.net** fest. Legen Sie den Wert für **Transport** auf eine der folgenden Optionen fest: **HTTP**, **AMQP** oder **MQTT**. Derzeit wird nur bei **HTTP** eine TCP-Verbindung für alle Gerätenachrichten gemeinsam genutzt. Wenn Sie den Wert auf **AMQP** oder **MQTT** festlegen, richtet das Gateway für jedes Gerät eine eigene TCP-Verbindung mit IoT Hub ein.
* Das Modul **Mapping** ordnet die MAC-Adressen Ihrer simulierten Geräte den IoT Hub-Geräte-IDs zu. Stellen Sie sicher, dass die **deviceId**-Werte mit den IDs der beiden Geräte übereinstimmen, die Sie Ihrem IoT Hub hinzugefügt haben, und dass die **deviceKey**-Werte die Schlüssel der beiden Geräte enthalten.
* Bei den Modulen **BLE1** und **BLE2** handelt es sich um die simulierten Geräte. Beachten Sie, dass ihre MAC-Adressen mit den Adressen im Modul **Mapping** übereinstimmen.
* Das Modul **Logger** protokolliert die Aktivitäten Ihres Gateways in einer Datei.
* Bei den im Beispiel gezeigten Werten für **module path** wird angenommen, dass Sie das Beispiel aus dem Ordner **build** in Ihrer lokalen Kopie des Repositorys **iot-edge** ausführen.
* Das Array **Links** unten in der JSON-Datei verbindet die Module **BLE1** und **BLE2** mit dem Modul **Mapping** und das Modul **Mapping** mit dem Modul **IoTHub**. Zusätzlich wird gewährleistet, dass alle Nachrichten vom Modul **Logger** protokolliert werden.

```json
{
    "modules": [
        {
            "name": "IotHub",
          "loader": {
            "name": "native",
            "entrypoint": {
              "module.path": "./modules/iothub/libiothub.so"
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
              "module.path": "./modules/identitymap/libidentity_map.so"
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
              "module.path": "./modules/simulated_device/libsimulated_device.so"
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
              "module.path": "./modules/simulated_device/libsimulated_device.so"
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
              "module.path": "./modules/logger/liblogger.so"
            }
            },
            "args": {
              "filename": "deviceCloudUploadGatewaylog.log"
            }
          }
    ],
    "links": [
        {
            "source": "*",
            "sink": "Logger"
        },
        {
            "source": "BLE1",
            "sink": "mapping"
        },
        {
            "source": "BLE2",
            "sink": "mapping"
        },
        {
            "source": "mapping",
            "sink": "IotHub"
        }
    ]
}
```

Speichern Sie die an der Konfigurationsdatei vorgenommenen Änderungen.

So führen Sie das Beispiel aus:

1. Navigieren Sie in der Shell zum Ordner **iot-edge/build**.
2. Führen Sie den folgenden Befehl aus:
   
    ```sh
    ./samples/simulated_device_cloud_upload/simulated_device_cloud_upload_sample ../samples/simulated_device_cloud_upload/src/simulated_device_cloud_upload_lin.json
    ```
3. Sie können die Tools [Device Explorer][lnk-device-explorer] oder [iothub-explorer][lnk-iothub-explorer] verwenden, um die Nachrichten zu überwachen, die IoT Hub vom Gateway empfängt. Mithilfe von „iothub-explorer“ können Sie beispielsweise D2C-Nachrichten mit dem folgenden Befehl überwachen:

    ```sh
    iothub-explorer monitor-events --login "HostName={Your iot hub name}.azure-devices.net;SharedAccessKeyName=iothubowner;SharedAccessKey={Your IoT Hub key}"
    ```

## <a name="next-steps"></a>Nächste Schritte

Wenn Sie noch mehr über Azure IoT Edge erfahren und mit einigen Codebeispielen experimentieren möchten, sehen Sie sich die folgenden Tutorials und Ressourcen für Entwickler an:

* [Senden von D2C-Nachrichten von einem physischen Gerät mit Azure IoT Edge][lnk-physical-device]
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

