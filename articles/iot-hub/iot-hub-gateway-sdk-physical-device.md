---
title: "Verwenden eines physischen Geräts mit dem Azure IoT Gateway SDK | Microsoft-Dokumentation"
description: "Hier erfahren Sie, wie Sie mithilfe eines SensorTag-Geräts von Texas Instruments Daten über ein auf einem Raspberry Pi 3 ausgeführtes Gateway an eine IoT Hub-Instanz senden. Das Gateway wird mithilfe des Azure IoT Gateway SDKs erstellt."
services: iot-hub
documentationcenter: 
author: chipalost
manager: timlt
editor: 
ms.assetid: 212dacbf-e5e9-48b2-9c8a-1c14d9e7b913
ms.service: iot-hub
ms.devlang: cpp
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/14/2016
ms.author: andbuc
translationtype: Human Translation
ms.sourcegitcommit: dcda8b30adde930ab373a087d6955b900365c4cc
ms.openlocfilehash: 05c82a87e839a0a95e7050092d6f6867e76fb316


---
# <a name="use-the-azure-iot-gateway-sdk-to-send-device-to-cloud-messages-with-a-physical-device-linux"></a>Senden von D2C-Nachrichten mit einem physischen Gerät mithilfe des Azure IoT Gateway SDKs (Linux)
In dieser exemplarischen Vorgehensweise eines [Bluetooth-Beispiels mit niedrigem Energieverbrauch][lnk-ble-samplecode] erfahren Sie, wie Sie mit dem [Azure IoT Gateway SDK][lnk-sdk] D2C-Telemetriedaten von einem physischen Gerät an IoT Hub weiterleiten und wie Sie Befehle aus IoT Hub an ein physisches Gerät senden.

Diese Anleitung umfasst:

* **Architektur**: wichtige Informationen zur Architektur zu dem Bluetooth-Beispiel mit niedrigem Energieverbrauch.
* **Erstellen und Ausführen**: die zum Erstellen und Ausführen des Beispiels erforderlichen Schritte.

## <a name="architecture"></a>Architektur
In der exemplarischen Vorgehensweise wird veranschaulicht, wie Sie ein IoT Gateway auf einem Raspberry Pi 3 unter Raspbian Linux erstellen und ausführen. Das Gateway wird mit dem IoT Gateway SDK erstellt. Für das Beispiel wird ein Texas Instruments SensorTag Bluetooth Low Energy (BLE)-Gerät zum Erfassen von Temperaturdaten verwendet.

Folgendes geschieht, wenn Sie das Gateway ausführen:

* Eine Verbindung mit einem SensorTag-Gerät wird mithilfe des BLE-Protokolls (Bluetooth Low Energy) hergestellt.
* Eine Verbindung mit IoT Hub wird mithilfe des Protokolls HTTP hergestellt.
* Telemetriedaten werden aus dem SensorTag-Gerät an IoT Hub weitergeleitet.
* Befehle werden aus IoT Hub an das SensorTag-Gerät gesendet.

Das Gateway enthält die folgenden Module:

* Ein *BLE-Modul* , das über eine Schnittstelle mit einem BLE-Gerät verbunden ist, um Temperaturdaten von dem Gerät zu empfangen und Befehle an das Gerät zu senden.
* Ein *BLE-Cloud-zu-Gerät-Modul*, das die aus der Cloud stammenden JSON-Nachrichten für das *BLE-Modul* in BLE-Anweisungen übersetzt.
* Ein *Protokollierungsmodul*, das alle Gatewaynachrichten in einer lokalen Datei protokolliert.
* Ein *Identitätszuordnungsmodul* , das zwischen BLE-Gerät-MAC-Adressen und Azure IoT Hub-Geräteidentitäten übersetzt.
* Ein *IoT Hub-Modul* , das die Telemetriedaten in einen IoT Hub hochlädt und Gerätebefehle von einem IoT Hub empfängt.
* Ein *BLE-Druckermodul* , das die Telemetriedaten von dem BLE-Gerät interpretiert und formatierte Daten an die Konsole ausgibt, um Problembehandlung und Debuggen zu aktivieren.

### <a name="how-data-flows-through-the-gateway"></a>Datenfluss über das Gateway
Das folgende Blockdiagramm zeigt die Datenflusspipeline beim Hochladen der Telemetriedaten:

![](media/iot-hub-gateway-sdk-physical-device/gateway_ble_upload_data_flow.png)

Folgende Schritte legt ein Telemetrieelement auf dem Weg von einem BLE-Gerät zu IoT Hub zurück:

1. Das BLE-Gerät generiert eine Temperaturstichprobe und sendet sie über Bluetooth an das BLE-Modul im Gateway.
2. Das BLE-Modul empfängt die Stichprobe und übergibt sie zusammen mit der MAC-Adresse des Geräts an den Broker.
3. Das Identitätszuordnungsmodul nimmt diese Nachricht an und verwendet eine interne Tabelle, um die MAC-Adresse des Geräts in eine IoT Hub-Geräteidentität (Geräte-ID und Schlüssel des Geräts) zu übersetzen. Anschließend wird eine neue Nachricht veröffentlicht, die die Temperaturbeispieldaten, die MAC-Adresse des Geräts, die Geräte-ID und den Schlüssel des Geräts enthält.
4. Das IoT Hub-Modul empfängt diese (vom Identitätszuordnungsmodul generierte) neue Nachricht und veröffentlicht sie im IoT Hub.
5. Das Modul für die Protokollierung protokolliert alle Nachrichten vom Broker in eine lokale Datei.

Das folgende Blockdiagramm zeigt die Datenflusspipeline des Gerätebefehls:

![](media/iot-hub-gateway-sdk-physical-device/gateway_ble_command_data_flow.png)

1. Das IoT Hub-Modul fragt den IoT Hub in regelmäßigen Abständen nach neuen Befehlsnachrichten ab.
2. Wenn das IoT Hub-Modul eine neue Befehlsnachricht empfängt, veröffentlicht es sie im Broker.
3. Das Identitätszuordnungsmodul nimmt diese Nachricht an und verwendet eine interne Tabelle, um die IoT Hub-Geräte-ID in eine MAC-Adresse des Geräts zu übersetzen. Dann veröffentlicht es eine neue Nachricht, die die MAC-Adresse des Zielgeräts in der Eigenschaftenzuordnung der Nachricht enthält.
4. Das BLE-Cloud-zu-Gerät-Modul übernimmt diese Nachricht und übersetzt sie in die richtige BLE-Anweisung für das BLE-Modul. Anschließend wird eine neue Nachricht veröffentlicht.
5. Das BLE-Modul nimmt diese Nachricht auf und führt die E/A-Anweisung durch Kommunikation mit dem BLE-Gerät aus.
6. Das Modul für die Protokollierung protokolliert alle Nachrichten vom Broker in einer Datenträgerdatei.

## <a name="prepare-your-hardware"></a>Vorbereiten der Hardware
In diesem Tutorial wird vorausgesetzt, dass Sie ein [Texas Instruments SensorTag](http://www.ti.com/ww/en/wireless_connectivity/sensortag2015/index.html)-Gerät verwenden, das mit einem Raspberry Pi 3 mit Raspbian-Betriebssystem verbunden ist.

### <a name="install-raspbian"></a>Installieren von Raspbian
Sie können eine der folgenden Optionen verwenden, um Raspbian auf dem Raspberry Pi 3-Gerät zu installieren. 

* Verwenden Sie [NOOBS][lnk-noobs], eine grafische Benutzeroberfläche, um die aktuelle Version von Raspbian zu installieren. 
* [Laden Sie das aktuelle Image des Raspbian-Betriebssystems manuell herunter][lnk-raspbian], und schreiben Sie es auf eine SD-Karte. 

### <a name="install-bluez-537"></a>Installieren von BlueZ 5.37
Die BLE-Module kommunizieren mit der Bluetooth-Hardware über den BlueZ-Stapel. Sie benötigen Version 5.37 von BlueZ, damit die Module richtig funktionieren. In dieser Anleitung wird sichergestellt, dass die richtige Version von BlueZ installiert wird.

1. Beenden Sie den aktuellen Bluetooth-Daemon:
   
    ```
    sudo systemctl stop bluetooth
    ```
2. Installieren Sie die BlueZ-Abhängigkeiten. 
   
    ```
    sudo apt-get update
    sudo apt-get install bluetooth bluez-tools build-essential autoconf glib2.0 libglib2.0-dev libdbus-1-dev libudev-dev libical-dev libreadline-dev
    ```
3. Laden Sie den BlueZ-Quellcode von „bluez.org“ herunter. 
   
    ```
    wget http://www.kernel.org/pub/linux/bluetooth/bluez-5.37.tar.xz
    ```
4. Entzippen Sie den Quellcode.
   
    ```
    tar -xvf bluez-5.37.tar.xz
    ```
5. Wechseln Sie zum neu erstellten Ordner.
   
    ```
    cd bluez-5.37
    ```
6. Konfigurieren Sie den BlueZ-Code für den Buildvorgang.
   
    ```
    ./configure --disable-udev --disable-systemd --enable-experimental
    ```
7. Führen Sie den Buildvorgang für BlueZ durch.
   
    ```
    make
    ```
8. Installieren Sie BlueZ, nachdem der Buildvorgang abgeschlossen ist.
   
    ```
    sudo make install
    ```
9. Ändern Sie die systemd-Dienstkonfiguration für Bluetooth so, dass auf den neuen Bluetooth-Daemon in der Datei `/lib/systemd/system/bluetooth.service` verwiesen wird. Ersetzen Sie die Zeile „ExecStart“ durch den folgenden Text: 
    
    ```
    ExecStart=/usr/local/libexec/bluetooth/bluetoothd -E
    ```

### <a name="enable-connectivity-to-the-sensortag-device-from-your-raspberry-pi-3-device"></a>Aktivieren der Konnektivität mit dem SensorTag-Gerät vom Raspberry Pi 3-Gerät aus
Bevor Sie das Beispiel ausführen, müssen Sie überprüfen, ob für den Raspberry Pi 3 eine Verbindung mit dem SensorTag-Gerät hergestellt werden kann.


1. Stellen Sie sicher, dass das Hilfsprogramm `rfkill` installiert ist.
   
    ```
    sudo apt-get install rfkill
    ```
2. Geben Sie Bluetooth auf dem Raspberry Pi 3-Computer frei, und überprüfen Sie, ob die Versionsnummer **5.37** lautet.
   
    ```
    sudo rfkill unblock bluetooth
    bluetoothctl --version
    ```
3. Starten Sie den Bluetooth-Dienst, und führen Sie den Befehl **bluetoothctl** aus, um eine interaktive Bluetooth-Shell einzugeben. 
   
    ```
    sudo systemctl start bluetooth
    bluetoothctl
    ```
4. Geben Sie den Befehl **power on** ein, um den Bluetooth-Controller einzuschalten. Eine ähnliche Ausgabe wie die folgende sollte angezeigt werden:
   
    ```
    [NEW] Controller 98:4F:EE:04:1F:DF C3 raspberrypi [default]
    ```
5. Während Sie sich noch in der interaktiven Bluetooth-Shell befinden, geben Sie den Befehl **scan on** zum Scannen nach Bluetooth-Geräten ein. Eine ähnliche Ausgabe wie die folgende sollte angezeigt werden:
   
    ```
    Discovery started
    [CHG] Controller 98:4F:EE:04:1F:DF Discovering: yes
    ```
6. Machen Sie das SensorTag-Gerät erkennbar, indem Sie auf die kleine Taste drücken (die grüne LED sollte blinken). Der Raspberry Pi 3 sollte das SensorTag-Gerät ermitteln können:
   
    ```
    [NEW] Device A0:E6:F8:B5:F6:00 CC2650 SensorTag
    [CHG] Device A0:E6:F8:B5:F6:00 TxPower: 0
    [CHG] Device A0:E6:F8:B5:F6:00 RSSI: -43
    ```
   
    In diesem Beispiel können Sie sehen, dass die MAC-Adresse des SensorTag-Geräts **A0:E6:F8:B5:F6:00**ist.
7. Deaktivieren Sie das Scannen durch Eingabe des Befehls **scan off** .
   
    ```
    [CHG] Controller 98:4F:EE:04:1F:DF Discovering: no
    Discovery stopped
    ```
8. Stellen Sie die Verbindung mit Ihrem SensorTag-Gerät über dessen MAC-Adresse her, indem Sie **connect \<MAC-Adresse>** eingeben. Beachten Sie, dass die folgende Beispielausgabe abgekürzt ist:
   
    ```
    Attempting to connect to A0:E6:F8:B5:F6:00
    [CHG] Device A0:E6:F8:B5:F6:00 Connected: yes
    Connection successful
    [CHG] Device A0:E6:F8:B5:F6:00 UUIDs: 00001800-0000-1000-8000-00805f9b34fb
    ...
    [NEW] Primary Service
            /org/bluez/hci0/dev_A0_E6_F8_B5_F6_00/service000c
            Device Information
    ...
    [CHG] Device A0:E6:F8:B5:F6:00 GattServices: /org/bluez/hci0/dev_A0_E6_F8_B5_F6_00/service000c
    ...
    [CHG] Device A0:E6:F8:B5:F6:00 Name: SensorTag 2.0
    [CHG] Device A0:E6:F8:B5:F6:00 Alias: SensorTag 2.0
    [CHG] Device A0:E6:F8:B5:F6:00 Modalias: bluetooth:v000Dp0000d0110
    ```
   
    > Mit dem Befehl **list-attributes** können Sie die GATT-Merkmale des Geräts erneut auflisten.
9. Sie können nun mithilfe des Befehls **disconnect** die Verbindung mit dem Gerät trennen und dann mithilfe des Befehl **quit** die Bluetooth-Shell verlassen:
   
    ```
    Attempting to disconnect from A0:E6:F8:B5:F6:00
    Successful disconnected
    [CHG] Device A0:E6:F8:B5:F6:00 Connected: no
    ```

Nun können Sie das BLE Gateway-Beispiel auf dem Raspberry Pi 3-Computer ausführen.

## <a name="run-the-ble-gateway-sample"></a>Ausführen des BLE Gateway-Beispiels
Um das BLE-Beispiel auszuführen, müssen Sie drei Aufgaben durchführen:

* Konfigurieren Sie zwei Beispielgeräte in Ihrem IoT Hub.
* Erstellen Sie das IoT Gateway SDK auf dem Raspberry Pi 3-Gerät.
* Konfigurieren Sie das BLE-Beispiel auf dem Raspberry Pi 3-Gerät, und führen Sie es aus.

Zum Zeitpunkt der Abfassung dieses Artikels unterstützt das IoT Gateway SDK nur Gateways, die BLE-Module unter Linux verwenden.

### <a name="configure-two-sample-devices-in-your-iot-hub"></a>Konfigurieren von zwei Beispielgeräten in Ihrem IoT Hub
* [Erstellen Sie einen IoT Hub][lnk-create-hub] in Ihrem Azure-Abonnement. Der Name des Hubs wird später in dieser exemplarischen Vorgehensweise benötigt. Wenn Sie kein Konto haben, können Sie in nur wenigen Minuten ein [kostenloses Konto][lnk-free-trial] erstellen.
* Fügen Sie Ihrer IoT Hub-Instanz ein Gerät namens **SensorTag_01** hinzu, und notieren Sie sich die ID und den Geräteschlüssel. Sie können die Tools [Geräte-Explorer oder iothub-explorer][lnk-explorer-tools] verwenden, um der im vorherigen Schritt erstellten IoT Hub-Instanz Geräte hinzuzufügen und die zugehörigen Schlüssel abzurufen. Ordnen Sie beim Konfigurieren des Gateways dieses Gerät dem SensorTag-Gerät zu.

### <a name="build-the-azure-iot-gateway-sdk-on-your-raspberry-pi-3"></a>Erstellen des Azure IoT Gateway SDK auf dem Raspberry Pi 3

Installieren Sie die Abhängigkeiten für das Azure IoT Gateway SDK.

``` 
sudo apt-get install cmake uuid-dev curl libcurl4-openssl-dev libssl-dev
```
Verwenden Sie die folgenden Befehle, um das IoT Gateway SDK und alle Untermodule im Stammverzeichnis zu klonen:

```
cd ~
git clone --recursive https://github.com/Azure/azure-iot-gateway-sdk.git 
cd azure-iot-gateway-sdk
git submodule update --init --recursive
```

Wenn Sie auf Ihrem Raspberry Pi 3-Gerät über eine vollständige Kopie des IoT Gateway SDK-Repositorys verfügen, können Sie es mit dem folgenden Befehl aus dem Ordner erstellen, der das SDK enthält:

```
./tools/build.sh --skip-unittests
```

### <a name="configure-and-run-the-ble-sample-on-your-raspberry-pi-3"></a>Konfigurieren und Ausführen des BLE-Beispiels auf dem Raspberry Pi 3
Zum Starten und Ausführen des Beispiels müssen Sie jedes Modul konfigurieren, das am Gateway beteiligt ist. Diese Konfiguration wird in einer JSON-Datei bereitgestellt, und Sie müssen alle fünf beteiligten Module konfigurieren. Das Repository enthält eine JSON-Beispieldatei namens **gateway_sample.json**, die Sie als Grundlage für die Erstellung einer eigenen Konfigurationsdatei verwenden können. Diese Datei befindet sich in der lokalen Kopie des IoT Gateway SDK-Repositorys im Ordner **samples/ble_gateway/src**.

In den folgenden Abschnitten wird beschrieben, wie diese Konfigurationsdatei für das BLE-Beispiel bearbeitet wird. Dabei wird vorausgesetzt, dass sich das IoT Gateway SDK-Repository auf Ihrem Raspberry Pi 3-Gerät im Ordner **/home/pi/azure-iot-gateway-sdk/** befindet. Wenn das Repository sich an anderer Stelle befindet, müssen Sie die Pfade entsprechend anpassen:

#### <a name="logger-configuration"></a>Protokollierungskonfiguration
Sofern sich das Gatewayrepository im Ordner **/home/pi/azure-iot-gateway-sdk/** befindet, konfigurieren Sie das Modul für die Protokollierung wie folgt:

```json
{
  "name": "Logger",
  "loader": {
    "name" : "native",
    "entrypoint" : {
      "module.path" : "build/modules/logger/liblogger.so"
    }
  },
  "args":
  {
    "filename": "<</path/to/log-file.log>>"
  }
}
```

#### <a name="ble-module-configuration"></a>BLE-Modulkonfiguration
Die Beispielkonfiguration für das BLE-Gerät setzt ein Texas Instruments SensorTag-Gerät voraus. Alle BLE-Standardgeräte, die als GATT-Peripheriegeräte eingesetzt werden können, sollten funktionieren, aber Sie müssen die GATT-Merkmal-IDs und die Daten (für Schreibanweisungen) aktualisieren. Fügen Sie die MAC-Adresse des SensorTag-Geräts hinzu: 

```json
{
  "name": "SensorTag",
  "loader": {
    "name" : "native",
    "entrypoint" : {
      "module.path": "build/modules/ble/libble.so"
    }
  },
  "args": {
    "controller_index": 0,
    "device_mac_address": "<<AA:BB:CC:DD:EE:FF>>",
    "instructions": [
      {
        "type": "read_once",
        "characteristic_uuid": "00002A24-0000-1000-8000-00805F9B34FB"
      },
      {
        "type": "read_once",
        "characteristic_uuid": "00002A25-0000-1000-8000-00805F9B34FB"
      },
      {
        "type": "read_once",
        "characteristic_uuid": "00002A26-0000-1000-8000-00805F9B34FB"
      },
      {
        "type": "read_once",
        "characteristic_uuid": "00002A27-0000-1000-8000-00805F9B34FB"
      },
      {
        "type": "read_once",
        "characteristic_uuid": "00002A28-0000-1000-8000-00805F9B34FB"
      },
      {
        "type": "read_once",
        "characteristic_uuid": "00002A29-0000-1000-8000-00805F9B34FB"
      },
      {
        "type": "write_at_init",
        "characteristic_uuid": "F000AA02-0451-4000-B000-000000000000",
        "data": "AQ=="
      },
      {
        "type": "read_periodic",
        "characteristic_uuid": "F000AA01-0451-4000-B000-000000000000",
        "interval_in_ms": 1000
      },
      {
        "type": "write_at_exit",
        "characteristic_uuid": "F000AA02-0451-4000-B000-000000000000",
        "data": "AA=="
      }
    ]
  }
}
```

#### <a name="iot-hub-module"></a>IoT Hub-Modul
Fügen Sie den Namen Ihres IoT Hubs hinzu. Der Suffixwert lautet in der Regel **azure-devices.net**:

```json
{
  "name": "IoTHub",
  "loader": {
    "name" : "native",
    "entrypoint" : {
      "module.path": "build/modules/iothub/libiothub.so"
    }
  },
  "args": {
    "IoTHubName": "<<Azure IoT Hub Name>>",
    "IoTHubSuffix": "<<Azure IoT Hub Suffix>>",
    "Transport" : "amqp"
  }
}
```

#### <a name="identity-mapping-module-configuration"></a>Konfiguration des Identitätszuordnungsmoduls
Fügen Sie die MAC-Adresse Ihres SensorTag-Geräts sowie die Geräte-ID und den Schlüssel des Geräts **SensorTag_01** hinzu, das Sie Ihrer IoT Hub-Instanz hinzugefügt haben:

```json
{
  "name": "mapping",
  "loader": {
    "name" : "native",
    "entrypoint" : {
      "module.path": "build/modules/identitymap/libidentity_map.so"
    }
  },
  "args": [
    {
      "macAddress": "AA:BB:CC:DD:EE:FF",
      "deviceId": "<<Azure IoT Hub Device ID>>",
      "deviceKey": "<<Azure IoT Hub Device Key>>"
    }
  ]
}
```

#### <a name="ble-printer-module-configuration"></a>BLE-Druckermodulkonfiguration
```json
{
  "name": "BLE Printer",
  "loader": {
    "name" : "native",
    "entrypoint" : {
      "module.path": "build/samples/ble_gateway/ble_printer/libble_printer.so"
    }
  },
  "args": null
}
```

#### <a name="blec2d-module-configuration"></a>BLEC2D-Modulkonfiguration
```json
{
  "name": "BLEC2D",
  "loader": {
    "name" : "native",
    "entrypoint" : {
      "module.path": "build/modules/ble/libble_c2d.so"
    }
  },
  "args": null
}
```

#### <a name="routing-configuration"></a>Routingkonfiguration
Die folgende Konfiguration stellt Folgendes sicher:

* Das Modul **Logger** empfängt und protokolliert alle Nachrichten.
* Das Modul **SensorTag** sendet Nachrichten an die Module **apping** und **BLE Printer**.
* Das Modul **mapping** sendet Nachrichten an das Modul **IoTHub**, die an Ihren IoT Hub gesendet werden sollen.
* Das Modul **IoTHub** sendet Nachrichten zurück an das Modul **mapping**.
* Das Modul **mapping** sendet Nachrichten an das Modul **BLEC2D**.
* Das Modul **BLEC2D** sendet Nachrichten zurück an das Modul **SensorTag**.

```json
"links" : [
    {"source" : "*", "sink" : "Logger" },
    {"source" : "SensorTag", "sink" : "mapping" },
    {"source" : "SensorTag", "sink" : "BLE Printer" },
    {"source" : "mapping", "sink" : "IoTHub" },
    {"source" : "IoTHub", "sink" : "mapping" },
    {"source" : "mapping", "sink" : "BLEC2D" },
    {"source" : "BLEC2D", "sink" : "SensorTag"}
 ]
```

Übergeben Sie zum Ausführen des Beispiels den Pfad zur JSON-Konfigurationsdatei an die **ble_gateway**-Binärdatei. Der Befehl, der bei Verwendung der Datei **gateway_sample.json** gilt, ist unten angegeben. Führen Sie diesen Befehl über das Verzeichnis „azure-iot-gateway-sdk“ aus.

```
./build/samples/ble_gateway/ble_gateway ./samples/ble_gateway/src/gateway_sample.json
```

Sie müssen die kleine Taste auf dem SensorTag-Gerät vor dem Ausführen des Beispiels drücken, damit es erkannt werden kann.

Wenn Sie das Beispiel ausführen, können Sie die Tools [Geräte-Explorer oder iothub-explorer][lnk-explorer-tools] verwenden, um die Nachrichten zu überwachen, die das Gateway vom SensorTag-Gerät weiterleitet.

## <a name="send-cloud-to-device-messages"></a>Senden von C2D-Nachrichten.
Das BLE-Modul unterstützt auch das Senden von Anweisungen vom Azure IoT Hub an das Gerät. Sie können den [Geräte-Explorer](https://github.com/Azure/azure-iot-sdks/blob/master/tools/DeviceExplorer/doc/how_to_use_device_explorer.md) oder das Tool [iothub-explorer](https://github.com/Azure/azure-iot-sdks/tree/master/tools/iothub-explorer) verwenden, um JSON-Nachrichten zu senden, die das BLE-Gatewaymodul an das BLE-Gerät übergibt.
Bei Verwendung des SensorTag-Geräts von Texas Instruments können Sie die rote LED, grüne LED oder den Summer einschalten, indem Sie Befehle vom IoT Hub senden. Senden Sie hierzu die folgenden beiden JSON-Nachrichten in der richtigen Reihenfolge. Anschließend können Sie die entsprechenden Befehle senden, um die LEDs oder den Summer einzuschalten.

1. Zurücksetzen aller LEDs und des Summers (Ausschalten)
  
    ```json
    {
      "type": "write_once",
      "characteristic_uuid": "F000AA65-0451-4000-B000-000000000000",
      "data": "AA=="
    }
    ```
2. Konfigurieren von E/A als „remote“
  
    ```json
    {
      "type": "write_once",
      "characteristic_uuid": "F000AA66-0451-4000-B000-000000000000",
      "data": "AQ=="
    }
    ```
* Einschalten der roten LED
  
    ```json
    {
      "type": "write_once",
      "characteristic_uuid": "F000AA65-0451-4000-B000-000000000000",
      "data": "AQ=="
    }
    ```
* Einschalten der grünen LED
  
    ```json
    {
      "type": "write_once",
      "characteristic_uuid": "F000AA65-0451-4000-B000-000000000000",
      "data": "Ag=="
    }
    ```
* Einschalten des Summers
  
    ```json
    {
      "type": "write_once",
      "characteristic_uuid": "F000AA65-0451-4000-B000-000000000000",
      "data": "BA=="
    }
    ```

## <a name="next-steps"></a>Nächste Schritte
Wenn Sie noch mehr über das IoT Gateway SDK wissen und mit einigen Codebeispielen experimentieren möchten, sehen Sie sich die folgenden Tutorials und Ressourcen für Entwickler an:

* [Azure IoT Gateway SDK][lnk-sdk]

Weitere Informationen zu den Funktionen von IoT Hub finden Sie unter:

* [IoT Hub-Entwicklerhandbuch][lnk-devguide]

<!-- Links -->
[lnk-ble-samplecode]: https://github.com/Azure/azure-iot-gateway-sdk/tree/master/samples/ble_gateway
[lnk-free-trial]: https://azure.microsoft.com/pricing/free-trial/
[lnk-explorer-tools]: https://github.com/Azure/azure-iot-sdks/blob/master/doc/manage_iot_hub.md
[lnk-sdk]: https://github.com/Azure/azure-iot-gateway-sdk/
[lnk-noobs]: https://www.raspberrypi.org/documentation/installation/noobs.md
[lnk-raspbian]: https://www.raspberrypi.org/downloads/raspbian/


[lnk-devguide]: iot-hub-devguide.md
[lnk-create-hub]: iot-hub-create-through-portal.md 



<!--HONumber=Dec16_HO2-->


