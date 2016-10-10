<properties
	pageTitle="Verwenden eines realen Geräts mit dem Gateway SDK | Microsoft Azure"
	description="Exemplarische Vorgehensweise zum Azure IoT Hub Gateway SDK mit einem SensorTag-Gerät von Texas Instruments zum Senden von Daten an IoT Hub über ein Gateway, das auf einem Intel Edison Compute Modul ausgeführt wird."
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


# IoT Gateway SDK (Beta) – Senden von D2C-Nachrichten mit einem realen Gerät unter Linux

In dieser exemplarischen Vorgehensweise eines [Bluetooth-Beispiels mit niedrigem Energieverbrauch][lnk-ble-samplecode] erfahren Sie, wie Sie mit dem [Microsoft Azure IoT Gateway SDK][lnk-sdk] D2C-Telemetriedaten von einem physischen Gerät an IoT Hub weiterleiten, und wie Sie Befehle aus IoT Hub an ein physisches Gerät senden.

Diese Anleitung umfasst:

* **Architektur**: wichtige Informationen zur Architektur zu dem Bluetooth-Beispiel mit niedrigem Energieverbrauch.

* **Erstellen und Ausführen**: die zum Erstellen und Ausführen des Beispiels erforderlichen Schritte.

## Architektur

Die Vorgehensweise zeigt Ihnen, wie ein IoT-Gateway auf einem Intel Edison Compute Modul, das Linux ausführt, erstellt und ausgeführt wird. Das Gateway wird mit dem IoT Gateway SDK erstellt. Für das Beispiel wird ein Texas Instruments SensorTag Bluetooth Low Energy (BLE)-Gerät zum Erfassen von Temperaturdaten verwendet.

Folgendes geschieht, wenn Sie das Gateway ausführen:

- Eine Verbindung mit einem SensorTag-Gerät wird mithilfe des BLE-Protokolls (Bluetooth Low Energy) hergestellt.
- Eine Verbindung mit IoT Hub wird mithilfe des AMQP-Protokolls hergestellt.
- Telemetriedaten werden aus dem SensorTag-Gerät an IoT Hub weitergeleitet.
- Befehle werden aus IoT Hub an das SensorTag-Gerät gesendet.

Das Gateway enthält die folgenden Module:

- Ein *BLE-Modul*, das über eine Schnittstelle mit einem BLE-Gerät verbunden ist, um Temperaturdaten von dem Gerät zu empfangen und Befehle an das Gerät zu senden.
- Ein *BLE-Cloud-zu-Gerät-Modul*, das die aus der Cloud stammenden JSON-Nachrichten für das *BLE-Modul* in BLE-Anweisungen übersetzt.
- Ein *Protokollierungsmodul*, das alle Gatewaynachrichten protokolliert.
- Ein *Identitätszuordnungsmodul*, das zwischen BLE-Gerät-MAC-Adressen und Azure IoT Hub-Geräteidentitäten übersetzt.
- Ein *IoT Hub-Modul*, das die Telemetriedaten in einen IoT Hub hochlädt und Gerätebefehle von einem IoT Hub empfängt.
- Ein *BLE-Druckermodul*, das die Telemetriedaten von dem BLE-Gerät interpretiert und formatierte Daten an die Konsole ausgibt, um Problembehandlung und Debuggen zu aktivieren.

### Datenfluss über das Gateway

Das folgende Blockdiagramm zeigt die Datenflusspipeline beim Hochladen der Telemetriedaten:

![](media/iot-hub-gateway-sdk-physical-device/gateway_ble_upload_data_flow.png)

Folgende Schritte legt ein Telemetrieelement auf dem Weg von einem BLE-Gerät zu IoT Hub zurück:

1. Das BLE-Gerät generiert eine Temperaturstichprobe und sendet sie über Bluetooth an das BLE-Modul im Gateway.
2. Das BLE-Modul empfängt die Stichprobe und übergibt sie zusammen mit der MAC-Adresse des Geräts an den Broker.
3. Das Identitätszuordnungsmodul nimmt diese Nachricht an und verwendet eine interne Tabelle, um die MAC-Adresse des Geräts in eine IoT Hub-Geräteidentität (Geräte-ID und Schlüssel des Geräts) zu übersetzen. Anschließend wird eine neue Nachricht veröffentlicht, die die Temperaturbeispieldaten, die MAC-Adresse des Geräts, die Geräte-ID und den Schlüssel des Geräts enthält.
4. Das IoT Hub-Modul empfängt diese (vom Identitätszuordnungsmodul generierte) neue Nachricht und veröffentlicht sie im IoT Hub.
5. Das Modul für die Protokollierung protokolliert alle Nachrichten vom Broker in einer Datenträgerdatei.

Das folgende Blockdiagramm zeigt die Datenflusspipeline des Gerätebefehls:

![](media/iot-hub-gateway-sdk-physical-device/gateway_ble_command_data_flow.png)

1. Das IoT Hub-Modul fragt den IoT Hub in regelmäßigen Abständen nach neuen Befehlsnachrichten ab.
2. Wenn das IoT Hub-Modul eine neue Befehlsnachricht empfängt, veröffentlicht es sie im Broker.
3. Das Identitätszuordnungsmodul nimmt diese Nachricht an und verwendet eine interne Tabelle, um die IoT Hub-Geräte-ID in eine MAC-Adresse des Geräts zu übersetzen. Dann veröffentlicht es eine neue Nachricht, die die MAC-Adresse des Zielgeräts in der Eigenschaftenzuordnung der Nachricht enthält.
4. Das BLE-Cloud-zu-Gerät-Modul übernimmt diese Nachricht und übersetzt sie in die ordnungsgemäße BLE-Anweisung für das BLE-Modul. Anschließend wird eine neue Nachricht veröffentlicht.
5. Das BLE-Modul nimmt diese Nachricht auf und führt die E/A-Anweisung durch Kommunikation mit dem BLE-Gerät aus.
6. Das Modul für die Protokollierung protokolliert alle Nachrichten vom Broker in einer Datenträgerdatei.

## Vorbereiten der Hardware

In diesem Tutorial wird vorausgesetzt, dass Sie ein [Texas Instruments SensorTag](http://www.ti.com/ww/en/wireless_connectivity/sensortag2015/index.html)-Gerät verwenden, das mit einer Intel Edison-Platine verbunden ist.

### Einrichten der Edison-Platine

Bevor Sie beginnen, sollten Sie sicherstellen, dass Sie Ihr Edison-Gerät mit dem Drahtlosnetzwerk verbinden können. Um Ihr Edison-Gerät einzurichten, müssen Sie es mit einem Hostcomputer verbinden. Intel stellt Erste-Schritte-Handbücher für die folgenden Betriebssysteme bereit:

- [Get Started with the Intel Edison Development Board on Windows 64-bit][lnk-setup-win64] (Erste Schritte mit dem Intel Edison Development Board unter Windows 64-Bit).
- [Get Started with the Intel Edison Development Board on Windows 32-bit][lnk-setup-win32] (Erste Schritte mit dem Intel Edison Development Board unter Windows 32-Bit).
- [Get Started with the Intel Edison Development Board on Mac OS X][lnk-setup-osx] (Erste Schritte mit dem Intel Edison Development Board unter Mac OS X).
- [Getting Started with the Intel® Edison Board on Linux][lnk-setup-linux] (Erste Schritte mit dem Intel® Edison Board unter Linux).

Um Ihr Edison-Gerät einzurichten und sich damit vertraut zu machen, sollten Sie alle Schritte in diesen „Erste Schritte“-Artikeln mit Ausnahme des letzten Schritts, „Choose IDE“ (IDE auswählen), ausführen, der für das aktuelle Tutorial nicht erforderlich ist. Am Ende des Edison-Installationsvorgangs haben Sie:

- Ihr Edison-Gerät mit der neuesten Firmware aktualisiert.
- Eine serielle Verbindung zwischen Ihrem Host und dem Edison-Gerät eingerichtet.
- Führen Sie das Skript **configure\_edison** aus, um ein Kennwort festzulegen und WLAN auf Ihrem Edison-Gerät zu aktivieren.

### Aktivieren der Konnektivität mit dem SensorTag-Gerät von Ihrer Edison-Platine aus

Bevor Sie das Beispiel ausführen, müssen Sie überprüfen, ob Ihre Edison-Platine sich mit dem SensorTag-Gerät verbinden kann.

Zuerst müssen Sie überprüfen, ob Ihr Edison-Gerät eine Verbindung mit dem SensorTag-Gerät herstellen kann.

1. Geben Sie Bluetooth auf dem Edison-Gerät frei, und überprüfen Sie, ob die Versionsnummer **5.37** ist.
    
    ```
    rfkill unblock bluetooth
    bluetoothctl --version
    ```

2. Führen Sie den **Bluetoothctl**-Befehl aus. Sie befinden sich nun in einer interaktiven Bluetooth-Shell.

3. Geben Sie den Befehl **power on** ein, um den Bluetooth-Controller einzuschalten. Eine ähnliche Ausgabe wie die folgende sollte angezeigt werden:
    
    ```
    [NEW] Controller 98:4F:EE:04:1F:DF edison [default]
    ```

4. Während Sie sich noch in der interaktiven Bluetooth-Shell befinden, geben Sie den Befehl **scan on** zum Scannen nach Bluetooth-Geräten ein. Eine ähnliche Ausgabe wie die folgende sollte angezeigt werden:
    
    ```
    Discovery started
    [CHG] Controller 98:4F:EE:04:1F:DF Discovering: yes
    ```

5. Machen Sie das SensorTag-Gerät erkennbar, indem Sie auf die kleine Taste drücken (die grüne LED sollte blinken). Das Edison-Gerät sollte das SensorTag-Gerät erkennen:
    
    ```
    [NEW] Device A0:E6:F8:B5:F6:00 CC2650 SensorTag
    [CHG] Device A0:E6:F8:B5:F6:00 TxPower: 0
    [CHG] Device A0:E6:F8:B5:F6:00 RSSI: -43
    ```
    
    In diesem Beispiel können Sie sehen, dass die MAC-Adresse des SensorTag-Geräts **A0:E6:F8:B5:F6:00** ist.

6. Deaktivieren Sie das Scannen durch Eingabe des Befehls **scan off**.
    
    ```
    [CHG] Controller 98:4F:EE:04:1F:DF Discovering: no
    Discovery stopped
    ```

7. Stellen Sie die Verbindung mit Ihrem SensorTag-Gerät über dessen MAC-Adresse her, indem Sie **connect <MAC-Adresse>** eingeben. Beachten Sie, dass die folgende Beispielausgabe abgekürzt ist:
    
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
    
    Hinweis: Mit dem Befehl **list-attributes** können Sie die GATT-Merkmale des Geräts erneut auflisten.

8. Sie können nun mithilfe des Befehls **disconnect** die Verbindung mit dem Gerät trennen und dann mithilfe des Befehl **quit** die Bluetooth-Shell verlassen:
    
    ```
    Attempting to disconnect from A0:E6:F8:B5:F6:00
    Successful disconnected
    [CHG] Device A0:E6:F8:B5:F6:00 Connected: no
    ```

Nun können Sie das BLE Gateway-Beispiel auf Ihrem Edison-Gerät ausführen.

## Ausführen des BLE Gateway-Beispiels

Um das BLE-Beispiel auf Ihrem Edison-Gerät auszuführen, müssen Sie drei Aufgaben abschließen:

- Konfigurieren Sie zwei Beispielgeräte in Ihrem IoT Hub.
- Erstellen Sie das Gateway SDK auf Ihrem Edison-Gerät.
- Konfigurieren Sie das BLE-Beispiel auf Ihrem Edison-Gerät, und führen Sie es dort aus.

Zum Zeitpunkt der Abfassung dieses Artikels unterstützt das Gateway SDK nur Gateways, die BLE-Module unter Linux verwenden.

### Konfigurieren von zwei Beispielgeräten in Ihrem IoT Hub

- [Erstellen Sie einen IoT Hub][lnk-create-hub] in Ihrem Azure-Abonnement. Der Name des Hubs wird später in dieser exemplarischen Vorgehensweise benötigt. Wenn Sie nicht bereits über ein Azure-Abonnement verfügen, können Sie ein [kostenloses Konto erstellen][lnk-free-trial].
- Fügen Sie Ihrer IoT Hub-Instanz ein Gerät namens **SensorTag\_01** hinzu, und notieren Sie sich die ID und den Geräteschlüssel. Sie können die Tools [Geräte-Explorer oder iothub-explorer][lnk-explorer-tools] verwenden, um der im vorherigen Schritt erstellten IoT Hub-Instanz Geräte hinzuzufügen und die zugehörigen Schlüssel abzurufen. Ordnen Sie beim Konfigurieren des Gateways dieses Gerät dem SensorTag-Gerät zu.

### Erstellen des Gateway SDK auf Ihrem Edison-Gerät

Die Version von **git** auf dem Edison-Gerät unterstützt keine Untermodule. Sie haben zwei Optionen, die vollständige Quelle für das Gateway SDK auf das Edison-Gerät herunterzuladen:

- Option 1: Klonen des [Microsoft Azure IoT Gateway SDK][lnk-sdk]-Repositorys auf Ihrem Edison-Gerät und anschließendes manuelles Klonen des Repositorys für die einzelnen Untermodule.
- Option 2: Klonen des [Microsoft Azure IoT Gateway SDK][lnk-sdk]-Repositorys auf einem Desktopgerät, auf dem **git** Untermodule unterstützt, und anschließendes Kopieren des vollständigen Repositorys mit Untermodulen auf Ihr Edison-Gerät.

Verwenden Sie bei Option 2 die folgenden **git**-Befehle, um das Gateway SDK und alle seine Untermodule zu klonen:

```
git clone --recursive https://github.com/Azure/azure-iot-gateway-sdk.git 
git submodule update --init --recursive
```

Sie sollten dann das gesamte lokale Repository in einer einzigen Archivdatei komprimieren, bevor Sie es auf das Edison-Gerät kopieren. Sie können ein Dienstprogramm wie **pscp** (in **Putty** enthalten) verwenden, um die Archivdatei auf das Edison-Gerät zu kopieren. Beispiel:

```
pscp .\gatewaysdk.zip root@192.168.0.45:/home/root
```

Wenn Sie auf Ihrem Edison-Gerät über eine vollständige Kopie des Gateway SDK-Repositorys verfügen, können Sie es mit dem folgenden Befehl aus dem Ordner erstellen, der das SDK enthält:

```
./tools/build.sh
```

### Konfigurieren Sie das BLE-Beispiel auf Ihrem Edison-Gerät, und führen Sie es dort aus.

Zum Starten und Ausführen des Beispiels müssen Sie jedes Modul konfigurieren, das am Gateway beteiligt ist. Diese Konfiguration wird in einer JSON-Datei bereitgestellt, und Sie müssen alle fünf beteiligten Module konfigurieren. Das Repository enthält eine JSON-Beispieldatei namens **gateway\_sample.json**, die Sie als Grundlage für die Erstellung einer eigenen Konfigurationsdatei verwenden können. Diese Datei befindet sich in der lokalen Kopie des Gateway SDK-Repositorys im Ordner **samples/ble\_gateway\_hl/src** .

In den folgenden Abschnitten wird beschrieben, wie diese Konfigurationsdatei für das BLE-Beispiel bearbeitet wird. Dabei wird vorausgesetzt, dass sich das Gateway SDK-Repository auf Ihrem Edison-Gerät im Ordner **/home/root/azure-iot-gateway-sdk/** befindet. Wenn das Repository sich an anderer Stelle befindet, müssen Sie die Pfade entsprechend anpassen:

#### Protokollierungskonfiguration

Sofern sich das Gatewayrepository im Ordner **/home/root/azure-iot-gateway-sdk /** befindet, konfigurieren Sie das Modul für die Protokollierung wie folgt:

```json
{
    "module name": "logger",
    "module path": "/home/root/azure-iot-gateway-sdk/build/modules/logger/liblogger_hl.so",
    "args":
    {
        "filename":"/home/root/gw_logger.log"
    }
}
```

#### BLE-Modulkonfiguration

Die Beispielkonfiguration für das BLE-Gerät setzt ein Texas Instruments SensorTag-Gerät voraus. Alle BLE-Standardgeräte, die als GATT-Peripheriegeräte eingesetzt werden können, sollten funktionieren, aber Sie müssen die GATT-Merkmal-IDs und die Daten (für Schreibanweisungen) aktualisieren. Fügen Sie die MAC-Adresse des SensorTag-Geräts hinzu:

```json
{
  "module name": "SensorTag",
  "module path": "/home/root/azure-iot-gateway-sdk/build/modules/ble/libble_hl.so",
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

#### IoT Hub-Modul

Fügen Sie den Namen Ihres IoT Hubs hinzu. Der Suffixwert lautet in der Regel **azure-devices.net**:

```json
{
  "module name": "IoTHub",
  "module path": "/home/root/azure-iot-gateway-sdk/build/modules/iothub/libiothub_hl.so",
  "args": {
    "IoTHubName": "<<Azure IoT Hub Name>>",
    "IoTHubSuffix": "<<Azure IoT Hub Suffix>>",
    "Transport": "HTTP"
  }
}
```

#### Konfiguration des Identitätszuordnungsmoduls

Fügen Sie die MAC-Adresse Ihres SensorTag-Geräts sowie die Geräte-ID und den Schlüssel des Geräts **SensorTag\_01** hinzu, das Sie Ihrer IoT Hub-Instanz hinzugefügt haben:

```json
{
  "module name": "mapping",
  "module path": "/home/root/azure-iot-gateway-sdk/build/modules/identitymap/libidentity_map_hl.so",
  "args": [
    {
      "macAddress": "<<AA:BB:CC:DD:EE:FF>>",
      "deviceId": "<<Azure IoT Hub Device ID>>",
      "deviceKey": "<<Azure IoT Hub Device Key>>"
    }
  ]
}
```

#### BLE-Druckermodulkonfiguration

```json
{
    "module name": "BLE Printer",
    "module path": "/home/root/azure-iot-gateway-sdk/build/samples/ble_gateway_hl/ble_printer/libble_printer.so",
    "args": null
}
```

#### Routingkonfiguration

Die folgende Konfiguration stellt Folgendes sicher:
- Das Modul **Logger** empfängt und protokolliert alle Nachrichten.
- Das Modul **SensorTag** sendet Nachrichten an die Module **Mapping** und **BLE Printer**.
- Das Modul **Mapping** sendet Nachrichten an das Modul **IoTHub**, die zu Ihrem IoT Hub gesendet werden sollen.
- Das Modul **IoTHub** sendet Nachrichten zurück an das Modul **Mapping**.
- Das Modul **Mapping** sendet Nachrichten zurück an das Modul **SensorTag**.

```json
"links" : [
    {"source" : "*", "sink" : "Logger" },
    {"source" : "SensorTag", "sink" : "mapping" },
    {"source" : "SensorTag", "sink" : "BLE Printer" },
    {"source" : "mapping", "sink" : "IoTHub" },
    {"source" : "IoTHub", "sink" : "mapping" },
    {"source" : "mapping", "sink" : "SensorTag" }
  ]
```

Führen Sie zum Ausführen des Beispiels die Binärdatei **ble\_gateway\_hl** aus, um den Pfad der JSON-Konfigurationsdatei zu übergeben. Wenn Sie die Datei **gateway\_sample.json** verwendet haben, sieht der auszuführende Befehl folgendermaßen aus:

```
./build/samples/ble_gateway_hl/ble_gateway_hl ./samples/ble_gateway_hl/src/gateway_sample.json
```

Sie müssen die kleine Taste auf dem SensorTag-Gerät vor dem Ausführen des Beispiels drücken, damit es erkannt werden kann.

Wenn Sie das Beispiel ausführen, können Sie die Tools [Geräte-Explorer oder iothub-explorer][lnk-explorer-tools] verwenden, um die Nachrichten zu überwachen, die das Gateway vom SensorTag-Gerät weiterleitet.

## Senden von C2D-Nachrichten.

Das BLE-Modul unterstützt auch das Senden von Anweisungen aus dem Azure IoT Hub an das Gerät. Sie können den [Azure IoT Hub-Geräte-Explorer](https://github.com/Azure/azure-iot-sdks/blob/master/tools/DeviceExplorer/doc/how_to_use_device_explorer.md) oder den [IoT Hub-Explorer](https://github.com/Azure/azure-iot-sdks/tree/master/tools/iothub-explorer) zum Senden von JSON-Nachrichten verwenden, die das BLE-Gatewaymodul an das BLE-Gerät übergibt. Wenn Sie z.B. das SensorTag-Gerät von Texas Instruments verwenden, können Sie die folgenden JSON-Nachrichten aus IoT Hub an das Gerät senden.

- Zurücksetzen aller LEDs und des Summers (Ausschalten)

    ```json
    {
      "type": "write_once",
      "characteristic_uuid": "F000AA65-0451-4000-B000-000000000000",
      "data": "AA=="
    }
    ```

- Konfigurieren von E/A als „Remote“

    ```json
    {
      "type": "write_once",
      "characteristic_uuid": "F000AA66-0451-4000-B000-000000000000",
      "data": "AQ=="
    }
    ```

- Einschalten der roten LED

    ```json
    {
      "type": "write_once",
      "characteristic_uuid": "F000AA65-0451-4000-B000-000000000000",
      "data": "AQ=="
    }
    ```

- Einschalten der grünen LED

    ```json
    {
      "type": "write_once",
      "characteristic_uuid": "F000AA65-0451-4000-B000-000000000000",
      "data": "Ag=="
    }
    ```

- Einschalten des Summers

    ```json
    {
      "type": "write_once",
      "characteristic_uuid": "F000AA65-0451-4000-B000-000000000000",
      "data": "BA=="
    }
    ```

Standardmäßig prüft ein Gerät, das über das HTTP-Protokoll mit IoT Hub verbunden ist, alle 25 Minuten, ob ein neuer Befehl vorliegt. Wenn Sie also mehrere separate Befehle senden, müssen Sie 25 Minuten warten, bis das Gerät jeden Befehl erhält.

> [AZURE.NOTE] Das Gateway prüft auch bei jedem Start, ob neue Befehle vorliegen, sodass Sie durch Beenden und Starten des Gateways die Verarbeitung eines Befehls erzwingen können.

## Nächste Schritte

Wenn Sie noch mehr über das Gateway SDK wissen und mit einigen Codebeispielen experimentieren möchten, sehen Sie sich die folgenden Tutorials und Ressourcen für Entwickler an:

- [Verwalten eines Gatewaygeräts][lnk-manage-devices]
- [Azure IoT Gateway SDK][lnk-gateway-sdk]

Weitere Informationen zu den Funktionen von IoT Hub finden Sie unter:

- [Entwerfen Ihrer Lösung][lnk-design]
- [Entwicklerhandbuch][lnk-devguide]
- [Erkunden der Geräteverwaltung mithilfe der Beispielbenutzeroberfläche][lnk-dmui]
- [Verwenden des Azure-Portals zur Verwaltung von IoT Hub][lnk-portal]

<!-- Links -->
[lnk-ble-samplecode]: https://github.com/Azure/azure-iot-gateway-sdk/blob/master/samples/ble_gateway_hl
[lnk-setupdevbox]: https://github.com/Azure/azure-iot-gateway-sdk/blob/master/doc/devbox_setup.md
[lnk-create-hub]: iot-hub-manage-through-portal.md
[lnk-free-trial]: https://azure.microsoft.com/pricing/free-trial/
[lnk-explorer-tools]: https://github.com/Azure/azure-iot-sdks/blob/master/doc/manage_iot_hub.md
[lnk-gateway-sdk]: https://github.com/Azure/azure-iot-gateway-sdk/
[lnk-setup-win64]: https://software.intel.com/get-started-edison-windows
[lnk-setup-win32]: https://software.intel.com/get-started-edison-windows-32
[lnk-setup-osx]: https://software.intel.com/get-started-edison-osx
[lnk-setup-linux]: https://software.intel.com/get-started-edison-linux
[lnk-sdk]: https://github.com/Azure/azure-iot-gateway-sdk/

[lnk-manage-devices]: iot-hub-gateway-sdk-device-management.md

[lnk-design]: iot-hub-guidance.md
[lnk-devguide]: iot-hub-devguide.md
[lnk-dmui]: iot-hub-device-management-ui-sample.md
[lnk-portal]: iot-hub-manage-through-portal.md

<!---HONumber=AcomDC_0928_2016-->