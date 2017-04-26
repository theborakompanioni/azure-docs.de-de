---
title: "M0 in der Cloud – Verbinden von Feather M0 WiFi mit Azure IoT Hub | Microsoft-Dokumentation"
description: "Erläuterungen zum Herstellen einer Verbindung des Arduino-Geräts Adafruit Feather M0 WiFi mit Azure IoT Hub, einem Microsoft-Clouddienst, der bei der Verwaltung Ihrer IoT-Objekte hilft."
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: 
ms.assetid: 51befcdb-332b-416f-a6a1-8aabdb67f283
ms.service: iot-hub
ms.devlang: arduino
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
translationtype: Human Translation
ms.sourcegitcommit: 0c4554d6289fb0050998765485d965d1fbc6ab3e
ms.openlocfilehash: e2d44f821635ce9d91b67ecdc0653e2ba9c99b01
ms.lasthandoff: 04/13/2017


---

# <a name="connect-adafruit-feather-m0-wifi-to-azure-iot-hub-in-the-cloud"></a>Verbinden von Adafruit Feather M0 WiFi mit Azure IoT Hub in der Cloud
[!INCLUDE [iot-hub-get-started-device-selector](../../includes/iot-hub-get-started-device-selector.md)]

![Verbindung zwischen BME280, Feather M0 WiFi und IoT Hub](media/iot-hub-adafruit-feather-m0-wifi-get-started/1_connection-m0-feather-m0-iot-hub.png)

In diesem Tutorial machen Sie sich zuerst mit den Grundlagen der Verwendung des Arduino-Boards vertraut. Anschließend erfahren Sie, wie Sie Ihre Geräte mithilfe von [Azure IoT Hub](iot-hub-what-is-iot-hub.md) nahtlos mit der Cloud verbinden.

## <a name="what-you-do"></a>Aufgaben

Verbinden Sie Adafruit Feather M0 WiFi mit einem von Ihnen erstellten IoT Hub. Führen Sie anschließend eine Beispielanwendung auf M0 WiFi aus, um die Temperatur- und Feuchtigkeitsdaten eines BME280-Sensors zu erfassen. Senden Sie abschließend die Sensordaten an Ihren IoT Hub.



## <a name="what-you-learn"></a>Lerninhalt

* Erstellen eines IoT Hubs und Registrieren eines Geräts für Feather M0 WiFi
* Herstellen einer Verbindung von Feather M0 WiFi mit dem Sensor und Ihrem Computer
* Erfassen von Sensordaten durch Ausführen einer Beispielanwendung auf Feather M0 WiFi
* Senden der Sensordaten an Ihren IoT Hub

## <a name="what-you-need"></a>Erforderliches Element

![Für das Tutorial erforderliche Teile](media/iot-hub-adafruit-feather-m0-wifi-get-started/2_parts-needed-for-the-tutorial.png)

Für diesen Vorgang benötigen Sie die folgenden Teile aus dem Feather M0 WiFi Starter Kit:

* Die Feather M0 WiFi-Platine
* Ein Micro-USB-Kabel (Typ A)

Sie benötigen für die Entwicklungsumgebung außerdem Folgendes:

* Mac oder PC, auf dem Windows oder Ubuntu ausgeführt wird
* WLAN, mit dem Feather M0 Wifi verbunden werden kann
* Internetverbindung zum Herunterladen des Konfigurationstools
* [Arduino IDE](https://www.arduino.cc/en/main/software)-Version 1.6.8 oder höher. Frühere Versionen funktionieren nicht mit der AzureIoT-Bibliothek.


Die folgenden Elemente sind optional, falls Sie über keinen Sensor verfügen. Sie haben auch die Möglichkeit der Verwendung von simulierten Sensordaten.

* BME280-Temperatur- und Feuchtigkeitssensor
* Steckplatine
* M/M-Jumperdrähte

[!INCLUDE [iot-hub-get-started-create-hub-and-device](../../includes/iot-hub-get-started-create-hub-and-device.md)]

## <a name="connect-feather-m0-wifi-with-the-sensor-and-your-computer"></a>Herstellen einer Verbindung von Feather M0 WiFi mit dem Sensor und Ihrem Computer
In diesem Abschnitt verbinden Sie die Sensoren mit Ihrer Platine. Anschließend schließen Sie Ihr Gerät zur weiteren Nutzung an Ihren Computer an.
### <a name="connect-a-dht22-temperature-and-humidity-sensor-to-feather-m0-wifi"></a>Herstellen einer Verbindung eines DHT22-Temperatur- und Feuchtigkeitssensors mit Feather M0 Wifi

Verwenden Sie die Steckplatinen- und Jumperdrähte zum Herstellen der Verbindung. Falls Sie über keinen Sensor verfügen, überspringen Sie diesen Abschnitt, da Sie stattdessen simulierte Sensordaten verwenden können.

![Referenz der Verbindungen](media/iot-hub-adafruit-feather-m0-wifi-get-started/3_connections_on_breadboard.png)


Für Sensorstifte verwenden Sie die folgende Verkabelung:


| Start (Sensor)           | Ende (Board)            | Kabelfarbe   |
| -----------------------  | ---------------------- | ------------: |
| VDD (Stift 27A)            | 3V (Stift 3A)            | Rotes Kabel     |
| GND (Stift 29A)            | GND (Stift 6A)           | Schwarzes Kabel   |
| SCK (Stift 30A)            | SCK (Stift 12A)          | Gelbes Kabel  |
| SDO (Stift 31A)            | MI (Stift 14A)           | Weißes Kabel   |
| SDI (Stift 32A)            | M0 (Stift 13A)           | Blaues Kabel    |
| CS (Stift 33A)             | GPIO 5 (Stift 15J)       | Oranges Kabel  |

Weitere Informationen finden Sie unter [Adafruit BME280 Humidity + Barometric Pressure + Temperature Sensor Breakout](https://learn.adafruit.com/adafruit-bme280-humidity-barometric-pressure-temperature-sensor-breakout/wiring-and-test?view=all) und [Adafruit Feather M0 WiFi Pinouts](https://learn.adafruit.com/adafruit-feather-m0-wifi-atwinc1500/pinouts).



Jetzt muss Ihr Feather M0 Wifi mit einem funktionierenden Sensor verbunden werden.

![Verbinden von BME280 mit Feather M0 Wifi](media/iot-hub-adafruit-feather-m0-wifi-get-started/4_connect-bme280-feather-m0-wifi.png)

### <a name="connect-feather-m0-wifi-to-your-computer"></a>Verbinden von Feather M0 WiFi mit Ihrem Computer

Verwenden Sie (wie nachstehend gezeigt) das Micro-USB-Kabel (Typ A), um Feather M0 Wifi mit Ihrem Computer zu verbinden.

![Verbinden von Feather M0 Wifi mit Ihrem Computer](media/iot-hub-adafruit-feather-m0-wifi-get-started/5_connect-feather-m0-wifi-computer.png)

### <a name="add-serial-port-permissions-ubuntu-only"></a>Hinzufügen von Berechtigungen für den seriellen Anschluss (nur Ubuntu)

Bei Verwendung von Ubuntu stellen Sie sicher, dass Sie die Berechtigungen für die Verwendung des USB-Anschlusses von Feather M0 Wifi haben. Gehen Sie folgendermaßen vor, um Berechtigungen für den seriellen Anschluss hinzuzufügen:


1. Führen Sie an einem Terminal die folgenden Befehle aus:

   ```bash
   ls -l /dev/ttyUSB*
   ls -l /dev/ttyACM*
   ```

   Sie erhalten eine der folgenden Ausgaben:

   * crw-rw---- 1 root uucp xxxxxxxx
   * crw-rw---- 1 root dialout xxxxxxxx

   Beachten Sie, dass in der Ausgabe `uucp` oder `dialout` der Name des Gruppenbesitzers des USB-Anschlusses ist.

1. Fügen Sie der Gruppe den Benutzer anhand des folgenden Befehls hinzu:

   ```bash
   sudo usermod -a -G <group-owner-name> <username>
   ```

   `<group-owner-name>` ist der Name des Gruppenbesitzers, den Sie im vorherigen Schritt abgerufen haben. `<username>` ist Ihr Ubuntu-Benutzername.

1. Melden Sie von Ubuntu ab, und melden Sie sich erneut an, damit die Änderung angezeigt wird.

## <a name="collect-sensor-data-and-send-it-to-your-iot-hub"></a>Erfassen von Sensordaten und Senden an Ihren IoT Hub

In diesem Abschnitt stellen Sie eine Beispielanwendung auf Feather M0 Wifi bereit und führen sie aus. Die Beispielanwendung veranlasst die LED auf Feather M0 Wifi zu blinken und sendet die erfassten Temperatur- und Luftfeuchtigkeitsdaten vom BME280-Sensor an Ihren IoT Hub.

### <a name="get-the-sample-application-from-github-and-prepare-arduino-ide"></a>Abrufen der Beispielanwendung von GitHub und Vorbereiten der Arduino-IDE

Die Beispielanwendung wird auf GitHub gehostet. Klonen Sie das Beispielrepository, das die Beispielanwendung aus GitHub enthält. Gehen Sie folgendermaßen vor, um das Beispielrepository zu klonen:

1. Öffnen Sie eine Eingabeaufforderung oder ein Terminalfenster.
1. Wechseln Sie zu einem Ordner, in dem die Beispielanwendung gespeichert werden soll.
1. Führen Sie den folgenden Befehl aus:

   ```bash
   git clone https://github.com/Azure-Samples/iot-hub-Feather-M0-WiFi-client-app.git
   ```

Installieren Sie das Paket für Feather M0 Wifi in der Arduino-IDE:

1. Öffnen Sie den Ordner, in dem die Beispielanwendung gespeichert ist.
1. Öffnen Sie in der Arduino-IDE im Ordner „App“ die Datei „app.ino“.

   ![Öffnen der Beispielanwendung in der Arduino-IDE](media/iot-hub-adafruit-feather-m0-wifi-get-started/6_arduino-ide-open-sample-app.png)

1. Klicken Sie auf **Tools** > **Board** > **Boards Manager**, und installieren Sie dann die `Arduino SAMD Boards`-Version `1.6.2` oder höher. 

   Der Boards-Manager gibt an, dass `Arduino SAMD Boards` mit der Version `1.6.2` oder höher installiert ist.

   ![Das esp8266-Paket ist installiert](media/iot-hub-adafruit-feather-m0-wifi-get-started/7_arduino-ide-package-url.png)

1. Klicken Sie auf **Tools** > **Board** > **Adafruit M0 WiFi**.

1. Installieren von Treibern (nur Windows): Wenn Sie das Feather-Gerät anschließen, müssen Sie möglicherweise einen Treiber installieren. Klicken Sie [hier](https://github.com/adafruit/Adafruit_Windows_Drivers/releases/download/1.1/adafruit_drivers.exe), um das Treiberinstallationsprogramm herunterzuladen.
   Führen Sie die Schritte zum Installieren der Treiber aus, die Sie installieren möchten.

### <a name="install-necessary-libraries"></a>Installieren der erforderlichen Bibliotheken

1. Klicken Sie in Arduino IDE auf **Sketch (Skizze)** > **Include Library (Bibliothek einbeziehen)** > **Manage Libraries** (Bibliotheken verwalten).
1. Suchen Sie die folgenden Bibliotheknamen einzeln. Klicken Sie für jede gefundene Bibliothek auf **Install** (Installieren).
   * `Adafruit_WINC1500`
   * `RTCZero`
   * `NTPClient`
   * `AzureIoTHub`
   * `AzureIoTUtility`
   * `AzureIoTProtocol_HTTP`
   * `ArduinoJson`
   * `Adafruit BME280 Library`
   * `Adafruit Unified Sensor`

### <a name="dont-have-a-real-bme280-sensor"></a>Haben Sie keinen echten BME280-Sensor?

Die Beispielanwendung kann Temperatur- und Feuchtigkeitsdaten simulieren, falls Sie über keinen echten BME280-Sensor verfügen. Gehen Sie folgendermaßen vor, um in der Beispielanwendung die Nutzung simulierter Daten einzurichten:

1. Öffnen Sie im `app`-Ordner die `config.h`-Datei.
1. Suchen Sie die folgende Codezeile, und ändern Sie den Wert `false` in `true`:
   ```c
   define SIMULATED_DATA true
   ```
   ![Konfigurieren der Beispielanwendung für die Verwendung simulierter Daten](media/iot-hub-adafruit-feather-m0-wifi-get-started/8_arduino-ide-configure-app-use-simulated-data.png)

1. Speichern Sie die Datei als `Control-s`.

### <a name="deploy-the-sample-application-to-feather-m0-wifi"></a>Bereitstellen der Beispielanwendung in Feather M0 Wifi

1. Klicken Sie in der Arduino-IDE auf **Tool** > **Port**, und klicken Sie dann auf den seriellen Anschluss für Feather M0 Wifi.
1. Klicken Sie auf **Sketch** > **Upload**, um die Beispielanwendung zu erstellen und auf Feather M0 Wifi bereitzustellen.

### <a name="enter-your-credentials"></a>Eingeben Ihrer Anmeldeinformationen

Nach erfolgreichem Abschluss des Uploads führen Sie diese Schritte zur Eingabe Ihrer Anmeldeinformationen aus:

1. Klicken Sie in Arduino IDE auf **Tools** > **Serial Monitor** (Serieller Monitor).
1. Beachten Sie im Fenster des seriellen Monitors die beiden Dropdownlisten in der rechten unteren Ecke.
1. Wählen Sie für die linke Dropdownliste **No line ending** (Kein Zeilenende) aus.
1. Wählen Sie für die rechte Dropdownliste **115200 baud** (115200 Baud) aus.
1. Geben Sie die folgenden Informationen in das Eingabefeld am oberen Rand des Fensters für den seriellen Monitor ein, wenn Sie aufgefordert werden, diese bereitzustellen, und klicken Sie dann auf **Send** (Senden).
   * WLAN-SSID
   * WLAN-Kennwort
   * Geräte-Verbindungszeichenfolge

> [!Note]
> Die Anmeldeinformationen werden im EEPROM von Feather M0 Wifi gespeichert. Wenn Sie auf dem Feather M0 Wifi-Board auf die Schaltfläche „Zurücksetzen“ klicken, werden Sie von der Beispielanwendung gefragt, ob Sie die Informationen löschen möchten. Geben Sie `Y` ein, damit die Informationen gelöscht werden. Sie werden aufgefordert, die Anmeldeinformationen ein zweites Mal bereitzustellen.

### <a name="verify-the-sample-application-is-running-successfully"></a>Stellen Sie sicher, dass die Beispielanwendung erfolgreich ausgeführt wird

Wenn Sie die folgende Ausgabe im Fenster des seriellen Monitors und die blinkende LED auf Feather M0 Wifi sehen, wird die Beispielanwendung erfolgreich ausgeführt.

![Endgültige Ausgabe in der Arduino-IDE](media/iot-hub-adafruit-feather-m0-wifi-get-started/9_arduino-ide-final-output.png)

## <a name="next-steps"></a>Nächste Schritte

Sie haben ein Feather M0 WiFi-Gerät erfolgreich mit Ihrem IoT Hub verbunden und die erfassten Sensordaten an Ihren IoT Hub gesendet. 

[!INCLUDE [iot-hub-get-started-next-steps](../../includes/iot-hub-get-started-next-steps.md)]


