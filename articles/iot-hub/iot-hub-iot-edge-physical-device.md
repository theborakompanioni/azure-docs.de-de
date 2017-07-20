---
title: "Verwenden eines physischen Geräts mit dem Azure IoT Edge | Microsoft-Dokumentation"
description: "Hier erfahren Sie, wie Sie mithilfe eines SensorTag-Geräts von Texas Instruments Daten über ein auf einem Raspberry Pi 3 ausgeführtes IoT Edge-Gateway an eine IoT Hub-Instanz senden. Das Gateway wird mit Azure IoT Edge erstellt."
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
ms.date: 06/12/2017
ms.author: andbuc
ms.translationtype: Human Translation
ms.sourcegitcommit: cb4d075d283059d613e3e9d8f0a6f9448310d96b
ms.openlocfilehash: 02962a91c739a53dfcf947bcc736e5c293b9384f
ms.contentlocale: de-de
ms.lasthandoff: 06/26/2017


---
# <a name="use-azure-iot-edge-on-a-raspberry-pi-to-forward-device-to-cloud-messages-to-iot-hub"></a>Verwenden Sie Azure IoT Edge auf einem Raspberry Pi, um Gerät-zu-Cloud-Nachrichten an IoT Hub weiterzuleiten

In dieser exemplarischen Vorgehensweise zum [Bluetooth-Beispiel mit niedrigem Energieverbrauch][lnk-ble-samplecode] wird gezeigt, wie Sie mit [Azure IoT Edge][lnk-sdk] folgende Aufgaben durchführen:

* Weiterleiten von D2C-Telemetriedaten von einem physischen Gerät an IoT Hub
* Senden von Befehlen von IoT Hub an ein physisches Gerät

Diese Anleitung umfasst:

* **Architektur**: wichtige Informationen zur Architektur zu dem Bluetooth-Beispiel mit niedrigem Energieverbrauch.
* **Erstellen und Ausführen**: die zum Erstellen und Ausführen des Beispiels erforderlichen Schritte.

## <a name="architecture"></a>Architektur

In der exemplarischen Vorgehensweise wird veranschaulicht, wie Sie ein IoT Edge-Gateway auf einem Raspberry Pi 3 unter Raspbian Linux erstellen und ausführen. Das Gateway wird mit IoT Edge erstellt. Für das Beispiel wird ein Texas Instruments SensorTag Bluetooth Low Energy (BLE)-Gerät zum Erfassen von Temperaturdaten verwendet.

Folgendes geschieht, wenn Sie das IoT Edge-Gateway ausführen:

* Eine Verbindung mit einem SensorTag-Gerät wird mithilfe des BLE-Protokolls (Bluetooth Low Energy) hergestellt.
* Eine Verbindung mit IoT Hub wird mithilfe des Protokolls HTTP hergestellt.
* Telemetriedaten werden aus dem SensorTag-Gerät an IoT Hub weitergeleitet.
* Befehle werden aus IoT Hub an das SensorTag-Gerät gesendet.

Das Gateway enthält die folgenden IoT Edge-Module:

* Ein *BLE-Modul* , das über eine Schnittstelle mit einem BLE-Gerät verbunden ist, um Temperaturdaten von dem Gerät zu empfangen und Befehle an das Gerät zu senden.
* Ein *BLE-Cloud-zu-Gerät-Modul*, das die vom IoT Hub gesendeten JSON-Nachrichten für das *BLE-Modul* in BLE-Anweisungen übersetzt.
* Ein *Protokollierungsmodul*, das alle Gatewaynachrichten in einer lokalen Datei protokolliert.
* Ein *Identitätszuordnungsmodul* , das zwischen BLE-Gerät-MAC-Adressen und Azure IoT Hub-Geräteidentitäten übersetzt.
* Ein *IoT Hub-Modul* , das die Telemetriedaten in einen IoT Hub hochlädt und Gerätebefehle von einem IoT Hub empfängt.
* Ein *BLE-Druckermodul* , das die Telemetriedaten von dem BLE-Gerät interpretiert und formatierte Daten an die Konsole ausgibt, um Problembehandlung und Debuggen zu aktivieren.

### <a name="how-data-flows-through-the-gateway"></a>Datenfluss über das Gateway

Das folgende Blockdiagramm zeigt die Datenflusspipeline beim Hochladen der Telemetriedaten:

![Gatewaypipeline beim Hochladen der Telemetriedaten](media/iot-hub-iot-edge-physical-device/gateway_ble_upload_data_flow.png)

Folgende Schritte legt ein Telemetrieelement auf dem Weg von einem BLE-Gerät zu IoT Hub zurück:

1. Das BLE-Gerät generiert eine Temperaturstichprobe und sendet sie über Bluetooth an das BLE-Modul im Gateway.
1. Das BLE-Modul empfängt die Stichprobe und übergibt sie zusammen mit der MAC-Adresse des Geräts an den Broker.
1. Das Identitätszuordnungsmodul nimmt diese Nachricht an und verwendet eine interne Tabelle, um die MAC-Adresse des Geräts in eine IoT Hub-Geräteidentität zu übersetzen. Eine IoT Hub Geräteidentität besteht aus einer Geräte-ID und einem Schlüssel des Geräts.
1. Das Identitätszuordnungsmodul veröffentlicht eine neue Nachricht, die die Temperaturbeispieldaten, die MAC-Adresse des Geräts, die Geräte-ID und den Schlüssel des Geräts enthält.
1. Das IoT Hub-Modul empfängt diese (vom Identitätszuordnungsmodul generierte) neue Nachricht und veröffentlicht sie im IoT Hub.
1. Das Modul für die Protokollierung protokolliert alle Nachrichten vom Broker in eine lokale Datei.

Das folgende Blockdiagramm zeigt die Datenflusspipeline des Gerätebefehls:

![Gatewaypipeline des Gerätebefehls](media/iot-hub-iot-edge-physical-device/gateway_ble_command_data_flow.png)

1. Das IoT Hub-Modul fragt den IoT Hub in regelmäßigen Abständen nach neuen Befehlsnachrichten ab.
1. Wenn das IoT Hub-Modul eine neue Befehlsnachricht empfängt, veröffentlicht es sie im Broker.
1. Das Identitätszuordnungsmodul nimmt diese Nachricht an und verwendet eine interne Tabelle, um die IoT Hub-Geräte-ID in eine MAC-Adresse des Geräts zu übersetzen. Dann veröffentlicht es eine neue Nachricht, die die MAC-Adresse des Zielgeräts in der Eigenschaftenzuordnung der Nachricht enthält.
1. Das BLE-Cloud-zu-Gerät-Modul übernimmt diese Nachricht und übersetzt sie in die richtige BLE-Anweisung für das BLE-Modul. Anschließend wird eine neue Nachricht veröffentlicht.
1. Das BLE-Modul nimmt diese Nachricht auf und führt die E/A-Anweisung durch Kommunikation mit dem BLE-Gerät aus.
1. Das Modul für die Protokollierung protokolliert alle Nachrichten vom Broker in einer Datenträgerdatei.

## <a name="prerequisites"></a>Voraussetzungen

Um dieses Tutorial abzuschließen, benötigen Sie ein aktives Azure-Abonnement.

> [!NOTE]
> Wenn Sie über kein Konto verfügen, können Sie in nur wenigen Minuten ein kostenloses Testkonto erstellen. Ausführliche Informationen finden Sie unter [Kostenlose Azure-Testversion][lnk-free-trial].

Sie benötigen einen SSH-Client auf Ihrem Desktopcomputer, um per Remotezugriff auf die Befehlszeile des Raspberry Pi zugreifen zu können.

- In Windows ist kein SSH-Client enthalten. Wir empfehlen die Verwendung von [PuTTY](http://www.putty.org/).
- In den meisten Linux-Distributionen sowie in Mac OS ist ein SSH-Befehlszeilenprogramm enthalten. Weitere Informationen finden Sie unter [SSH Using Linux or Mac OS](https://www.raspberrypi.org/documentation/remote-access/ssh/unix.md) (SSH unter Linux oder Mac OS).

## <a name="prepare-your-hardware"></a>Vorbereiten der Hardware

In diesem Tutorial wird vorausgesetzt, dass Sie ein [Texas Instruments SensorTag](http://www.ti.com/ww/en/wireless_connectivity/sensortag2015/index.html)-Gerät verwenden, das mit einem Raspberry Pi 3 mit Raspbian-Betriebssystem verbunden ist.

### <a name="install-raspbian"></a>Installieren von Raspbian

Sie können eine der folgenden Optionen verwenden, um Raspbian auf dem Raspberry Pi 3-Gerät zu installieren.

* Verwenden Sie zum Installieren der aktuellen Version von Raspbian die grafische Benutzeroberfläche [NOOBS][lnk-noobs].
* [Laden Sie das aktuelle Image des Raspbian-Betriebssystems manuell herunter][lnk-raspbian], und schreiben Sie es auf eine SD-Karte.

### <a name="sign-in-and-access-the-terminal"></a>Anmelden und Zugreifen auf das Terminal

Sie können auf Ihrem Raspberry Pi auf zwei Arten auf eine Terminalumgebung zuzugreifen:

* Wenn Sie eine Tastatur und einen Monitor an Ihren Raspberry Pi angeschlossen haben, können Sie über die grafische Raspbian-Benutzeroberfläche auf ein Terminalfenster zugreifen.

* Sie können aber auch auf Ihrem Desktopcomputer per SSH auf die Befehlszeile Ihres Raspberry Pi zugreifen.

#### <a name="use-a-terminal-window-in-the-gui"></a>Verwenden eines Terminalfensters auf der grafischen Benutzeroberfläche

Die Anmeldeinformationen für Raspbian lauten standardmäßig **pi** (Benutzername) und **raspberry** (Kennwort). Auf der Taskleiste der grafischen Benutzeroberfläche können Sie über das Monitorsymbol das Hilfsprogramm **Terminal** starten.

#### <a name="sign-in-with-ssh"></a>Anmelden mit SSH

Sie können über SSH auf die Befehlszeile Ihres Raspberry Pi zugreifen. Im Artikel [SSH (Secure Shell)][lnk-pi-ssh] erfahren Sie, wie Sie SSH auf Ihrem Raspberry Pi konfigurieren und unter [Windows][lnk-ssh-windows] oder unter [Linux und Mac OS][lnk-ssh-linux] eine Verbindung herstellen.

Melden Sie sich mit dem Benutzernamen **pi** und dem Kennwort **raspberry** an.

### <a name="install-bluez-537"></a>Installieren von BlueZ 5.37

Die BLE-Module kommunizieren mit der Bluetooth-Hardware über den BlueZ-Stapel. Sie benötigen Version 5.37 von BlueZ, damit die Module richtig funktionieren. In dieser Anleitung wird sichergestellt, dass die richtige Version von BlueZ installiert wird.

1. Beenden Sie den aktuellen Bluetooth-Daemon:

    ```sh
    sudo systemctl stop bluetooth
    ```

1. Installieren Sie die BlueZ-Abhängigkeiten:

    ```sh
    sudo apt-get update
    sudo apt-get install bluetooth bluez-tools build-essential autoconf glib2.0 libglib2.0-dev libdbus-1-dev libudev-dev libical-dev libreadline-dev
    ```

1. Laden Sie den BlueZ-Quellcode von „bluez.org“ herunter:

    ```sh
    wget http://www.kernel.org/pub/linux/bluetooth/bluez-5.37.tar.xz
    ```

1. Entzippen Sie den Quellcode:

    ```sh
    tar -xvf bluez-5.37.tar.xz
    ```

1. Wechseln Sie zum neu erstellten Ordner:

    ```sh
    cd bluez-5.37
    ```

1. Konfigurieren Sie den BlueZ-Code für den Buildvorgang:

    ```sh
    ./configure --disable-udev --disable-systemd --enable-experimental
    ```

1. Führen Sie den Buildvorgang für BlueZ durch:

    ```sh
    make
    ```

1. Installieren Sie BlueZ, nachdem der Buildvorgang abgeschlossen ist:

    ```sh
    sudo make install
    ```

1. Ändern Sie die systemd-Dienstkonfiguration für Bluetooth so, dass auf den neuen Bluetooth-Daemon in der Datei `/lib/systemd/system/bluetooth.service` verwiesen wird. Ersetzen Sie die Zeile „ExecStart“ durch den folgenden Text:

    ```conf
    ExecStart=/usr/local/libexec/bluetooth/bluetoothd -E
    ```

### <a name="enable-connectivity-to-the-sensortag-device-from-your-raspberry-pi-3-device"></a>Aktivieren der Konnektivität mit dem SensorTag-Gerät vom Raspberry Pi 3-Gerät aus

Bevor Sie das Beispiel ausführen, müssen Sie überprüfen, ob für den Raspberry Pi 3 eine Verbindung mit dem SensorTag-Gerät hergestellt werden kann.

1. Stellen Sie sicher, dass das Hilfsprogramm `rfkill` installiert ist:

    ```sh
    sudo apt-get install rfkill
    ```

1. Geben Sie Bluetooth auf dem Raspberry Pi 3-Computer frei, und überprüfen Sie, ob die Versionsnummer **5.37** lautet:

    ```sh
    sudo rfkill unblock bluetooth
    bluetoothctl --version
    ```

1. Um die interaktive Bluetooth-Befehlsshell einzugeben, starten Sie den Bluetooth-Dienst, und führen Sie den Befehl **bluetoothctl** aus:

    ```sh
    sudo systemctl start bluetooth
    bluetoothctl
    ```

1. Geben Sie den Befehl **power on** ein, um den Bluetooth-Controller einzuschalten. Der Befehl gibt eine Ausgabe wie die folgende zurück:

    ```sh
    [NEW] Controller 98:4F:EE:04:1F:DF C3 raspberrypi [default]
    ```

1. Geben Sie in der interaktiven Bluetooth-Shell den Befehl **scan on** zum Scannen nach Bluetooth-Geräten ein. Der Befehl gibt eine Ausgabe wie die folgende zurück:

    ```sh
    Discovery started
    [CHG] Controller 98:4F:EE:04:1F:DF Discovering: yes
    ```

1. Machen Sie das SensorTag-Gerät erkennbar, indem Sie auf die kleine Taste drücken (die grüne LED sollte blinken). Der Raspberry Pi 3 sollte das SensorTag-Gerät ermitteln können:

    ```sh
    [NEW] Device A0:E6:F8:B5:F6:00 CC2650 SensorTag
    [CHG] Device A0:E6:F8:B5:F6:00 TxPower: 0
    [CHG] Device A0:E6:F8:B5:F6:00 RSSI: -43
    ```

    In diesem Beispiel können Sie sehen, dass die MAC-Adresse des SensorTag-Geräts **A0:E6:F8:B5:F6:00**ist.

1. Deaktivieren Sie das Scannen durch Eingabe des Befehls **scan off**:

    ```sh
    [CHG] Controller 98:4F:EE:04:1F:DF Discovering: no
    Discovery stopped
    ```

1. Stellen Sie die Verbindung mit Ihrem SensorTag-Gerät über dessen MAC-Adresse her, indem Sie **connect \<MAC-Adresse\>** eingeben. Die folgende Beispielausgabe ist aus Gründen der Übersichtlichkeit abgekürzt:

    ```sh
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

1. Sie können nun mithilfe des Befehls **disconnect** die Verbindung mit dem Gerät trennen und dann mithilfe des Befehl **quit** die Bluetooth-Shell verlassen:

    ```sh
    Attempting to disconnect from A0:E6:F8:B5:F6:00
    Successful disconnected
    [CHG] Device A0:E6:F8:B5:F6:00 Connected: no
    ```

Nun können Sie das BLE IoT Edge-Beispiel auf dem Raspberry Pi 3-Computer ausführen.

## <a name="run-the-iot-edge-ble-sample"></a>Ausführen des IoT Edge-BLE-Beispiels

Um das IoT Edge-BLE-Beispiel auszuführen, müssen Sie drei Aufgaben durchführen:

* Konfigurieren Sie zwei Beispielgeräte in Ihrem IoT Hub.
* Erstellen Sie IoT Edge auf Ihrem Raspberry Pi 3-Gerät.
* Konfigurieren Sie das BLE-Beispiel auf dem Raspberry Pi 3-Gerät, und führen Sie es aus.

Zum Zeitpunkt der Abfassung dieses Artikels unterstützt IoT Edge nur BLE-Module in Gateways, die unter Linux laufen.

### <a name="configure-two-sample-devices-in-your-iot-hub"></a>Konfigurieren von zwei Beispielgeräten in Ihrem IoT Hub

* [Erstellen Sie einen IoT Hub][lnk-create-hub] in Ihrem Azure-Abonnement. Der Name des Hubs wird später in dieser exemplarischen Vorgehensweise benötigt. Wenn Sie nicht über ein Konto verfügen, können Sie in nur wenigen Minuten ein [kostenloses Konto][lnk-free-trial] erstellen.
* Fügen Sie Ihrer IoT Hub-Instanz ein Gerät namens **SensorTag_01** hinzu, und notieren Sie sich die ID und den Geräteschlüssel. Sie können die Tools [Geräte-Explorer oder iothub-explorer][lnk-explorer-tools] verwenden, um der im vorherigen Schritt erstellten IoT Hub-Instanz Geräte hinzuzufügen und die zugehörigen Schlüssel abzurufen. Ordnen Sie beim Konfigurieren des Gateways dieses Gerät dem SensorTag-Gerät zu.

### <a name="build-azure-iot-edge-on-your-raspberry-pi-3"></a>Erstellen von Azure IoT Edge auf Ihrem Raspberry Pi 3

Installieren Sie die Abhängigkeiten für Azure IoT Edge:

```sh
sudo apt-get install cmake uuid-dev curl libcurl4-openssl-dev libssl-dev
```

Verwenden Sie die folgenden Befehle, um IoT Edge und alle Untermodule im Stammverzeichnis zu klonen:

```sh
cd ~
git clone https://github.com/Azure/iot-edge.git
```

Wenn Sie auf Ihrem Raspberry Pi 3-Gerät über eine vollständige Kopie des IoT Edge-Repositorys verfügen, können Sie es mit dem folgenden Befehl aus dem Ordner erstellen, der das SDK enthält:

```sh
cd ~/iot-edge
./tools/build.sh  --disable-native-remote-modules
```

### <a name="configure-and-run-the-ble-sample-on-your-raspberry-pi-3"></a>Konfigurieren und Ausführen des BLE-Beispiels auf dem Raspberry Pi 3

Zum Starten und Ausführen des Beispiels müssen Sie jedes IoT Edge-Modul konfigurieren, das am Gateway beteiligt ist. Diese Konfiguration wird in einer JSON-Datei bereitgestellt, und Sie müssen alle fünf beteiligten IoT Edge-Module konfigurieren. Das Repository enthält eine JSON-Beispieldatei namens **gateway\_sample.json**, die Sie als Grundlage für die Erstellung einer eigenen Konfigurationsdatei verwenden können. Diese Datei befindet sich in der lokalen Kopie des IoT Edge-Repositorys im Ordner **samples/ble_gateway/src**.

In den folgenden Abschnitten wird beschrieben, wie diese Konfigurationsdatei für das BLE-Beispiel bearbeitet wird. Dabei wird vorausgesetzt, dass sich das IoT Edge-Repository auf Ihrem Raspberry Pi 3-Gerät im Ordner **/home/pi/iot-edge/** befindet. Wenn das Repository sich an anderer Stelle befindet, passen Sie die Pfade entsprechend an.

#### <a name="logger-configuration"></a>Protokollierungskonfiguration

Sofern sich das Gatewayrepository im Ordner **/home/pi/iot-edge/** befindet, konfigurieren Sie das Modul für die Protokollierung wie folgt:

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

Die Beispielkonfiguration für das BLE-Gerät setzt ein Texas Instruments SensorTag-Gerät voraus. Alle BLE-Standardgeräte, die als GATT-Peripheriegeräte eingesetzt werden können, sollten funktionieren, aber Sie müssen die GATT-Merkmal-IDs und die Daten aktualisieren. Fügen Sie die MAC-Adresse des SensorTag-Geräts hinzu:

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

Wenn Sie kein SensorTag-Gerät verwenden, überprüfen Sie die Dokumentation für Ihr BLE-Gerät, um festzustellen, ob Sie die GATT-Merkmal-IDs und die -Datenwerte aktualisieren müssen.

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
      "macAddress": "<<AA:BB:CC:DD:EE:FF>>",
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

Die nachstehende Konfiguration stellt folgendes Routing zwischen IoT Edge-Modulen sicher:

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

Übergeben Sie zum Ausführen des Beispiels den Pfad zur JSON-Konfigurationsdatei als Parameter an die **ble\_gateway**-Binärdatei. Beim folgenden Befehl wird die Verwendung der Konfigurationsdatei **gateway_sample.json** vorausgesetzt. Führen Sie diesen Befehl über den Ordner **iot-edge** auf dem Raspberry Pi aus:

```sh
./build/samples/ble_gateway/ble_gateway ./samples/ble_gateway/src/gateway_sample.json
```

Sie müssen die kleine Taste auf dem SensorTag-Gerät vor dem Ausführen des Beispiels drücken, damit es erkannt werden kann.

Wenn Sie das Beispiel ausführen, können Sie das Tool [Geräte-Explorer](https://github.com/Azure/azure-iot-sdk-csharp/blob/master/tools/DeviceExplorer) oder [iothub-explorer](https://github.com/Azure/iothub-explorer) verwenden, um die Nachrichten zu überwachen, die das IoT Edge-Gateway vom SensorTag-Gerät weiterleitet. Mithilfe von „iothub-explorer“ können Sie beispielsweise Gerät-zu-Cloud-Nachrichten mit dem folgenden Befehl überwachen:

```sh
iothub-explorer monitor-events --login "HostName={Your iot hub name}.azure-devices.net;SharedAccessKeyName=iothubowner;SharedAccessKey={Your IoT Hub key}"
```

## <a name="send-cloud-to-device-messages"></a>Senden von C2D-Nachrichten.

Das BLE-Modul unterstützt auch das Senden von Befehlen von IoT Hub an das Gerät. Sie können den [Geräte-Explorer](https://github.com/Azure/azure-iot-sdk-csharp/blob/master/tools/DeviceExplorer) oder das Tool [iothub-explorer](https://github.com/Azure/iothub-explorer) verwenden, um JSON-Nachrichten zu senden, die das BLE-Gatewaymodul an das BLE-Gerät weiterleitet.

Bei Verwendung des SensorTag-Geräts von Texas Instruments können Sie die rote LED, die grüne LED oder den Summer einschalten, indem Sie Befehle von IoT Hub senden. Senden Sie zunächst nacheinander die beiden folgenden JSON-Nachrichten, bevor Sie Befehle von IoT Hub senden. Anschließend können Sie die entsprechenden Befehle senden, um die LEDs oder den Summer einzuschalten.

1. Zurücksetzen aller LEDs und des Summers (Ausschalten):

    ```json
    {
      "type": "write_once",
      "characteristic_uuid": "F000AA65-0451-4000-B000-000000000000",
      "data": "AA=="
    }
    ```

1. Konfigurieren von E/A als „remote“:

    ```json
    {
      "type": "write_once",
      "characteristic_uuid": "F000AA66-0451-4000-B000-000000000000",
      "data": "AQ=="
    }
    ```

Jetzt können Sie die folgenden Befehle senden, um die LEDs oder den Summer am SensorTag-Gerät einzuschalten:

* Einschalten der roten LED:

    ```json
    {
      "type": "write_once",
      "characteristic_uuid": "F000AA65-0451-4000-B000-000000000000",
      "data": "AQ=="
    }
    ```

* Einschalten der grünen LED:

    ```json
    {
      "type": "write_once",
      "characteristic_uuid": "F000AA65-0451-4000-B000-000000000000",
      "data": "Ag=="
    }
    ```

* Einschalten des Summers:

    ```json
    {
      "type": "write_once",
      "characteristic_uuid": "F000AA65-0451-4000-B000-000000000000",
      "data": "BA=="
    }
    ```

## <a name="next-steps"></a>Nächste Schritte

Wenn Sie noch mehr über IoT Edge wissen und mit einigen Codebeispielen experimentieren möchten, sehen Sie sich die folgenden Tutorials und Ressourcen für Entwickler an:

* [Azure IoT Edge][lnk-sdk]

Weitere Informationen zu den Funktionen von IoT Hub finden Sie unter:

* [IoT Hub-Entwicklerhandbuch][lnk-devguide]

<!-- Links -->
[lnk-ble-samplecode]: https://github.com/Azure/iot-edge/tree/master/samples/ble_gateway
[lnk-free-trial]: https://azure.microsoft.com/pricing/free-trial/
[lnk-explorer-tools]: https://github.com/Azure/azure-iot-sdks/blob/master/doc/manage_iot_hub.md
[lnk-sdk]: https://github.com/Azure/iot-edge/
[lnk-noobs]: https://www.raspberrypi.org/documentation/installation/noobs.md
[lnk-raspbian]: https://www.raspberrypi.org/downloads/raspbian/
[lnk-devguide]: iot-hub-devguide.md
[lnk-create-hub]: iot-hub-create-through-portal.md 
[lnk-pi-ssh]: https://www.raspberrypi.org/documentation/remote-access/ssh/README.md
[lnk-ssh-windows]: https://www.raspberrypi.org/documentation/remote-access/ssh/windows.md
[lnk-ssh-linux]: https://www.raspberrypi.org/documentation/remote-access/ssh/unix.md

