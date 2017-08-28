---
title: Verbinden von Sparkfun ESP8266 Thing Dev mit Azure IoT Hub | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie Sparkfun ESP8266 Thing Dev einrichten und eine Verbindung mit Azure IoT Hub herstellen, um in diesem Tutorial Daten an die Azure-Cloudplattform zu senden.
services: iot-hub
documentationcenter: 
author: shizn
manager: timlt
tags: 
keywords: 
ms.assetid: 587fe292-9602-45b4-95ee-f39bba10e716
ms.service: iot-hub
ms.devlang: arduino
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/16/2017
ms.author: xshi
ms.translationtype: HT
ms.sourcegitcommit: 540180e7d6cd02dfa1f3cac8ccd343e965ded91b
ms.openlocfilehash: 557f0cdf375b345e0dbe0526f5a5bd3c050dec38
ms.contentlocale: de-de
ms.lasthandoff: 08/16/2017

---
# <a name="connect-sparkfun-esp8266-thing-dev-to-azure-iot-hub-in-the-cloud"></a>Verbinden von Sparkfun ESP8266 Thing Dev mit Azure IoT Hub in der Cloud

[!INCLUDE [iot-hub-get-started-device-selector](../../includes/iot-hub-get-started-device-selector.md)]

![Verbindung zwischen DHT22, Thing Dev und IoT Hub](media/iot-hub-sparkfun-thing-dev-get-started/1_connection-hdt22-thing-dev-iot-hub.png)

## <a name="what-you-will-do"></a>Aufgaben

Verbinden Sie Sparkfun ESP8266 Thing Dev mit einem von Ihnen erstellten IoT Hub. Führen Sie anschließend eine Beispielanwendung auf dem ESP8266 aus, um Temperatur- und Feuchtigkeitsdaten aus einem DHT22-Sensor zu erfassen. Senden Sie schließlich die Sensordaten an Ihren IoT Hub.

> [!NOTE]
> Bei Verwendung von anderen ESP8266-Boards können Sie diese Schritte dennoch durchführen, um es mit Ihrem IoT Hub zu verbinden. Je nach verwendetem ESP8266-Board müssen Sie möglicherweise das `LED_PIN`-Objekt neu konfigurieren. Beispiel: Bei Verwendung von ESP8266 aus AI-Thinker können Sie es von `0` in `2` ändern. Wenn Sie noch über kein Kit verfügen: Klicken Sie [hier](http://azure.com/iotstarterkits).

## <a name="what-you-will-learn"></a>Sie lernen Folgendes

* Erstellen eines IoT Hubs und Registrieren eines Geräts für Thing Dev
* Herstellen einer Verbindung von Thing Dev mit dem Sensor und Ihrem Computer
* Erfassen von Sensordaten durch Ausführen einer Beispielanwendung auf Thing Dev
* Senden der Sensordaten an Ihren IoT Hub

## <a name="what-you-will-need"></a>Sie benötigen Folgendes

![Für das Tutorial erforderliche Teile](media/iot-hub-sparkfun-thing-dev-get-started/2_parts-needed-for-the-tutorial.png)

Für diesen Vorgang benötigen Sie die folgenden Teile aus dem Thing Dev Starter Kit:

* Das Sparkfun ESP8266 Thing Dev-Board
* Ein Micro-USB-Kabel (Typ A).

Sie benötigen auch Folgendes für die Entwicklungsumgebung:

* Ein aktives Azure-Abonnement. Wenn Sie kein Azure-Konto besitzen, können Sie in nur wenigen Minuten ein [kostenloses Azure-Testkonto](https://azure.microsoft.com/free/) erstellen.
* Mac oder PC, auf dem Windows oder Ubuntu ausgeführt wird
* WLAN, mit dem Sparkfun ESP8266 Sache Dev die Verbindung herstellt
* Internetverbindung zum Herunterladen des Konfigurationstools
* [Arduino IDE](https://www.arduino.cc/en/main/software) Version 1.6.8 (oder höher). Frühere Versionen funktionieren nicht mit der AzureIoT-Bibliothek.

Die folgenden Elemente sind optional, falls Sie über keinen Sensor verfügen. Sie haben auch die Möglichkeit der Verwendung von simulierten Sensordaten.

* Ein Adafruit DHT22-Temperatur- und Feuchtigkeitssensor
* Eine Steckplatine
* M/M-Jumperdrähte

[!INCLUDE [iot-hub-get-started-create-hub-and-device](../../includes/iot-hub-get-started-create-hub-and-device.md)]

## <a name="connect-esp8266-thing-dev-with-the-sensor-and-your-computer"></a>Herstellen einer Verbindung von ESP8266 Thing Dev mit dem Sensor und Ihrem Computer

### <a name="connect-a-dht22-temperature-and-humidity-sensor-to-esp8266-thing-dev"></a>Herstellen einer Verbindung eines DHT22-Temperatur- und Feuchtigkeitssensors mit ESP8266 Thing Dev

Verwenden Sie die Steckplatinen- und Jumperdrähte zum Herstellen der Verbindung. Falls Sie über keinen Sensor verfügen, überspringen Sie diesen Abschnitt, da Sie stattdessen simulierte Sensordaten verwenden können.

![Verbindungsverweis](media/iot-hub-sparkfun-thing-dev-get-started/15_connections_on_breadboard.png)

Für Sensorpins verwenden wir die folgenden Verkabelungen:

| Start (Sensor)           | Ende (Board)           | Kabelfarbe   |
| -----------------------  | ---------------------- | ------------: |
| VDD (Stift 27F)            | 3V (Stift 8A)           | Rotes Kabel     |
| DATA (Stift 28F)           | GPIO 2 (Stift 9A)       | Weißes Kabel    |
| GND (Stift 30F)            | GND (Stift 7J)          | Schwarzes Kabel   |


- Weitere Informationen finden Sie in den Artikeln zur [Einrichtung des DHT22-Sensors](http://cdn.sparkfun.com/datasheets/Sensors/Weather/RHT03.pdf) und mit den [technischen Daten zu Sparkfun ESP8266 Thing Dev](https://www.sparkfun.com/products/13711).

Jetzt muss Ihr Sparkfun ESP8266 Thing Dev mit einem funktionierenden Sensor verbunden werden.

![Verbinden von dht22 mit ESP8266 Thing Dev](media/iot-hub-sparkfun-thing-dev-get-started/8_connect-dht22-thing-dev.png)

### <a name="connect-sparkfun-esp8266-thing-dev-to-your-computer"></a>Verbinden von Sparkfun ESP8266 Thing Dev mit Ihrem Computer

Verwenden Sie das Micro-USB-Kabel (Typ A), um Sparkfun ESP8266 Thing Dev wie folgt mit Ihrem Computer zu verbinden.

![Thing Dev mit Ihrem Computer verbinden](media/iot-hub-sparkfun-thing-dev-get-started/9_connect-thing-dev-computer.png)

### <a name="add-serial-port-permissions--ubuntu-only"></a>Hinzufügen von Berechtigungen für den seriellen Port – nur für Ubuntu

Bei Verwendung von Ubuntu stellen Sie sicher, dass ein normaler Benutzer die Berechtigungen für die Verwendung des USB-Anschlusses von Sparkfun ESP8266 Thing Dev hat. Gehen Sie folgendermaßen vor, um einem normalen Benutzer Berechtigungen für den seriellen Port zu erteilen:

1. Führen Sie an einem Terminal die folgenden Befehle aus:

   ```bash
   ls -l /dev/ttyUSB*
   ls -l /dev/ttyACM*
   ```

   Sie erhalten eine der folgenden Ausgaben:

   * crw-rw---- 1 root uucp xxxxxxxx
   * crw-rw---- 1 root dialout xxxxxxxx

   Beachten Sie in der Ausgabe `uucp` oder `dialout`, den Namen des Gruppenbesitzers des USB-Ports.

1. Fügen Sie der Gruppe den Benutzer anhand des folgenden Befehls hinzu:

   ```bash
   sudo usermod -a -G <group-owner-name> <username>
   ```

   `<group-owner-name>` ist der Name des Gruppenbesitzers, den Sie im vorherigen Schritt abgerufen haben. `<username>` ist Ihr Ubuntu-Benutzername.

1. Melden Sie sich bei Ubuntu ab und wieder an, damit die Änderung wirksam wird.

## <a name="collect-sensor-data-and-send-it-to-your-iot-hub"></a>Erfassen von Sensordaten und Senden an Ihren IoT Hub

In diesem Abschnitt stellen Sie eine Beispielanwendung auf Sparkfun ESP8266 Thing Dev bereit und führen sie aus. Die Beispielanwendung veranlasst die LED auf Sparkfun ESP8266 Thing Dev zu blinken und sendet die erfassten Temperatur- und Luftfeuchtigkeitsdaten vom DHT22-Sensor an Ihren IoT Hub.

### <a name="get-the-sample-application-from-github"></a>Abrufen der Beispielanwendung von GitHub

Die Beispielanwendung wird auf GitHub gehostet. Klonen Sie das Beispielrepository, das die Beispielanwendung aus GitHub enthält. Gehen Sie folgendermaßen vor, um das Beispielrepository zu klonen:

1. Öffnen Sie eine Eingabeaufforderung oder ein Terminalfenster.
1. Wechseln Sie zu einem Ordner, in dem die Beispielanwendung gespeichert werden soll.
1. Führen Sie den folgenden Befehl aus:

   ```bash
   git clone https://github.com/Azure-Samples/iot-hub-SparkFun-ThingDev-client-app.git
   ```

Installieren Sie das Paket für Sparkfun ESP8266 Thing Dev in Arduino IDE:

1. Öffnen Sie den Ordner, in dem die Beispielanwendung gespeichert ist.
1. Öffnen Sie in Arduino IDE im Ordner „App“ die app.ino-Datei.

   ![Beispielanwendung in Arduino IDE öffnen](media/iot-hub-sparkfun-thing-dev-get-started/10_arduino-ide-open-sample-app.png)

1. Klicken Sie in Arduino IDE auf **File (Datei)** > **Preferences (Einstellungen)**.
1. Klicken Sie im Dialogfeld **Preferences** (Einstellungen) auf das Symbol neben dem Textfeld **Additional Boards Manager URLs** (Zusätzliche Board-Manager-URLs).
1. Geben Sie im Popup-Fenster die folgende URL ein, und klicken Sie dann auf **OK**.

   `http://arduino.esp8266.com/stable/package_esp8266com_index.json`

   ![In Arduino IDE auf eine Paket-URL zeigen](media/iot-hub-sparkfun-thing-dev-get-started/11_arduino-ide-package-url.png)

1. Klicken Sie im Dialogfeld **Preference** (Einstellung) auf **OK**.
1. Klicken Sie auf **Tools** > **Board** > **Boards Manager** (Boards-Manager), und suchen Sie dann nach esp8266.
   Installieren Sie ESP8266 Version 2.2.0 oder höher.

   ![Das esp8266-Paket ist installiert](media/iot-hub-sparkfun-thing-dev-get-started/12_arduino-ide-esp8266-installed.png)

1. Klicken Sie auf **Tools** > **Board** > **Sparkfun ESP8266 Thing Dev**.

### <a name="install-necessary-libraries"></a>Installieren der erforderlichen Bibliotheken

1. Klicken Sie in Arduino IDE auf **Sketch (Skizze)** > **Include Library (Bibliothek einbeziehen)** > **Manage Libraries** (Bibliotheken verwalten).
1. Suchen Sie die folgenden Bibliotheknamen einzeln. Klicken Sie für jede gefundene Bibliothek auf **Install** (Installieren).
   * `AzureIoTHub`
   * `AzureIoTUtility`
   * `AzureIoTProtocol_MQTT`
   * `ArduinoJson`
   * `DHT sensor library`
   * `Adafruit Unified Sensor`

### <a name="dont-have-a-real-dht22-sensor"></a>Sie haben keinen echten DHT22-Sensor?

Die Beispielanwendung kann Temperatur- und Feuchtigkeitsdaten simulieren, falls Sie über keinen echten DHT22-Sensor verfügen. Gehen Sie folgendermaßen vor, um in der Beispielanwendung die Nutzung simulierter Daten zu aktivieren:

1. Öffnen Sie im `app`-Ordner die `config.h`-Datei.
1. Suchen Sie die folgende Codezeile, und ändern Sie den Wert `false` in `true`:
   ```c
   define SIMULATED_DATA true
   ```
   ![Beispielanwendung für die Verwendung von simulierten Daten konfigurieren](media/iot-hub-sparkfun-thing-dev-get-started/13_arduino-ide-configure-app-use-simulated-data.png)
   
1. Speichern Sie mit `Control-s`.

### <a name="deploy-the-sample-application-to-sparkfun-esp8266-thing-dev"></a>Bereitstellen der Beispielanwendung in Sparkfun ESP8266 Thing Dev

1. Klicken Sie in der Arduino IDE auf **Tool** > **Port** und dann auf den seriellen Anschluss für Sparkfun ESP8266 Thing Dev.
1. Klicken Sie auf **Sketch (Skizze)** > **Upload** (Hochladen), um die Beispielanwendung zu erstellen und in Sparkfun ESP8266 Thing Dev bereitzustellen.

> [!Note]
> Wenn Sie macOS verwenden, wurden während des Uploads wahrscheinlich folgende Meldungen angezeigt. `warning: espcomm_sync failed`,`error: espcomm_open failed`. Um dieses Problem zu beheben, öffnen Sie das Terminalfenster, und führen Sie die unten genannten Aktionen durch.
> ```bash
> cd /System/Library/Extensions/IOUSBFamily.kext/Contents/PlugIns
> sudo mv AppleUSBFTDI.kext AppleUSBFTDI.disabled
> sudo touch /System/Library/Extensions
> ```

### <a name="enter-your-credentials"></a>Eingeben Ihrer Anmeldeinformationen

Nach erfolgreichem Abschluss des Uploads führen Sie die Schritte zur Eingabe Ihrer Anmeldeinformationen aus:

1. Klicken Sie in Arduino IDE auf **Tools** > **Serial Monitor** (Serieller Monitor).
1. Beachten Sie im Fenster des seriellen Monitors die beiden Dropdownlisten in der unteren rechten Ecke.
1. Wählen Sie für die linke Dropdownliste **No line ending** (Kein Zeilenende) aus.
1. Wählen Sie für die rechte Dropdownliste **115200 baud** (115200 Baud) aus.
1. Geben Sie die folgenden Informationen in das Eingabefeld am oberen Rand des Fensters für den seriellen Monitor ein, wenn Sie aufgefordert werden, diese bereitzustellen, und klicken Sie dann auf **Send** (Senden).
   * WLAN-SSID
   * WLAN-Kennwort
   * Geräte-Verbindungszeichenfolge

> [!Note]
> Die Anmeldeinformationen werden im EEPROM von Sparkfun ESP8266 Thing Dev gespeichert. Wenn Sie auf dem Sparkfun ESP8266 Thing Dev-Board auf die Schaltfläche „Zurücksetzen“ klicken, werden Sie von der Beispielanwendung gefragt, ob Sie die Informationen löschen möchten. Geben Sie `Y` ein, um die Informationen zu löschen, und Sie werden aufgefordert, die Informationen erneut bereitzustellen.

### <a name="verify-the-sample-application-is-running-successfully"></a>Stellen Sie sicher, dass die Beispielanwendung erfolgreich ausgeführt wird

Wenn Sie die folgende Ausgabe im Fenster des seriellen Monitors und die blinkende LED auf Sparkfun ESP8266 Thing Dev sehen, wird die Beispielanwendung erfolgreich ausgeführt.

![Endgültige Ausgabe in Arduino IDE](media/iot-hub-sparkfun-thing-dev-get-started/14_arduino-ide-final-output.png)

## <a name="next-steps"></a>Nächste Schritte

Sie haben Sparkfun ESP8266 Thing Dev erfolgreich mit Ihrem IoT Hub verbunden und die erfassten Sensordaten an Ihren IoT Hub gesendet. 

[!INCLUDE [iot-hub-get-started-next-steps](../../includes/iot-hub-get-started-next-steps.md)]

