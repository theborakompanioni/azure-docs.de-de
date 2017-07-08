---
title: "ESP8266 in der Cloud – Verbinden von Feather HUZZAH ESP8266 mit Azure IoT Hub | Microsoft-Dokumentation"
description: "Erläuterungen zum Herstellen einer Verbindung des Arduino-Geräts Adafruit Feather HUZZAH ESP8266 mit Azure IoT Hub, einem Microsoft-Clouddienst, der bei der Verwaltung Ihrer IoT-Objekte hilft."
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: 
ms.assetid: c505aacf-89a8-40ed-a853-493b75bec524
ms.service: iot-hub
ms.devlang: arduino
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/15/2017
ms.author: xshi
ms.translationtype: Human Translation
ms.sourcegitcommit: 857267f46f6a2d545fc402ebf3a12f21c62ecd21
ms.openlocfilehash: 0969e78a69c73c29ecfddcf0de0ebeeeed8acd60
ms.contentlocale: de-de
ms.lasthandoff: 06/28/2017


---
# <a name="connect-adafruit-feather-huzzah-esp8266-to-azure-iot-hub-in-the-cloud"></a>Verbinden von Adafruit Feather HUZZAH ESP8266 mit dem Azure IoT Hub in der Cloud

[!INCLUDE [iot-hub-get-started-device-selector](../../includes/iot-hub-get-started-device-selector.md)]

![Verbindung zwischen DHT22, Feather HUZZAH ESP8266 und dem IoT Hub](media/iot-hub-arduino-huzzah-esp8266-get-started/1_connection-hdt22-feather-huzzah-iot-hub.png)

## <a name="what-you-do"></a>Aufgaben


Verbinden Sie Adafruit Feather HUZZAH ESP8266 mit einem IoT Hub, den Sie erstellen. Führen Sie anschließend eine Beispielanwendung auf dem ESP8266 aus, um die Temperatur- und Feuchtigkeitsdaten eines DHT22-Sensors zu erfassen. Senden Sie abschließend die Sensordaten an Ihren IoT Hub.

> [!NOTE]
> Bei Verwendung von anderen ESP8266-Boards können Sie diese Schritte dennoch durchführen, um es mit Ihrem IoT Hub zu verbinden. Je nach verwendetem ESP8266-Board müssen Sie möglicherweise das `LED_PIN`-Objekt neu konfigurieren. Beispiel: Bei Verwendung von ESP8266 aus AI-Thinker können Sie es von `0` in `2` ändern. Sie haben noch kein Kit? Sie erhalten es auf der [Azure-Website](http://azure.com/iotstarterkits).




## <a name="what-you-learn"></a>Lerninhalt

* Erstellen eines IoT Hubs und Registrieren eines Geräts für Feather HUZZAH ESP8266
* Herstellen einer Verbindung von Feather HUZZAH ESP8266 mit dem Sensor und Ihrem Computer
* Erfassen von Sensordaten durch Ausführen einer Beispielanwendung auf Feather HUZZAH ESP8266
* Senden der Sensordaten an Ihren IoT Hub

## <a name="what-you-need"></a>Erforderliches Element

![Für das Tutorial erforderliche Teile](media/iot-hub-arduino-huzzah-esp8266-get-started/2_parts-needed-for-the-tutorial.png)

Für diesen Vorgang benötigen Sie die folgenden Teile aus dem Feather HUZZAH ESP8266 Starter Kit:

* Das Feather HUZZAH ESP8266-Board
* Ein Micro-USB-Kabel (Typ A)

Sie benötigen für die Entwicklungsumgebung außerdem Folgendes:

* Ein aktives Azure-Abonnement. Wenn Sie kein Azure-Konto besitzen, können Sie in nur wenigen Minuten ein [kostenloses Azure-Testkonto](https://azure.microsoft.com/free/) erstellen.
* Mac oder PC, auf dem Windows oder Ubuntu ausgeführt wird
* Drahtloses Netzwerk, mit dem Feather HUZZAH ESP8266 verbunden werden kann
* Internetverbindung zum Herunterladen des Konfigurationstools
* [Arduino-IDE](https://www.arduino.cc/en/main/software) Version 1.6.8 oder höher. Frühere Versionen funktionieren nicht mit der AzureIoT-Bibliothek.

Die folgenden Elemente sind optional, falls Sie über keinen Sensor verfügen. Sie haben auch die Möglichkeit der Verwendung von simulierten Sensordaten.

* Ein Adafruit DHT22-Temperatur- und Feuchtigkeitssensor
* Eine Steckplatine
* M/M-Jumperdrähte


[!INCLUDE [iot-hub-get-started-create-hub-and-device](../../includes/iot-hub-get-started-create-hub-and-device.md)]

## <a name="connect-feather-huzzah-esp8266-with-the-sensor-and-your-computer"></a>Herstellen einer Verbindung von Feather HUZZAH ESP8266 mit dem Sensor und Ihrem Computer
In diesem Abschnitt verbinden Sie die Sensoren mit Ihrer Platine. Anschließend schließen Sie Ihr Gerät zur weiteren Nutzung an Ihren Computer an.
### <a name="connect-a-dht22-temperature-and-humidity-sensor-to-feather-huzzah-esp8266"></a>Herstellen einer Verbindung eines DHT22-Temperatur- und Feuchtigkeitssensors mit Feather HUZZAH ESP8266

Verwenden Sie die Steckplatinen- und Jumperdrähte zum Herstellen der Verbindung. Falls Sie über keinen Sensor verfügen, überspringen Sie diesen Abschnitt, da Sie stattdessen simulierte Sensordaten verwenden können.

![Referenz der Verbindungen](media/iot-hub-arduino-huzzah-esp8266-get-started/15_connections_on_breadboard.png)


Verwenden Sie für Sensorstifte die folgende Verkabelung:


| Start (Sensor)           | Ende (Board)           | Kabelfarbe   |
| -----------------------  | ---------------------- | ------------: |
| VDD (Pin 31F)            | 3V (Pin 58H)           | Rotes Kabel     |
| DATA (Pin 32F)           | GPIO 2 (Pin 46A)       | Blaues Kabel    |
| GND (Pin 34F)            | GND (PIn 56I)          | Schwarzes Kabel   |

Weitere Informationen finden Sie unter [Adafruit DHT22 sensor setup](https://learn.adafruit.com/dht/connecting-to-a-dhtxx-sensor) (Adafruit DHT22-Sensoreinrichtung) und [Adafruit Feather HUZZAH Esp8266 Pinouts](https://learn.adafruit.com/adafruit-feather-huzzah-esp8266/using-arduino-ide?view=all#pinouts) (Stiftbelegungen bei Adafruit Feather HUZZAH Esp8266).



Jetzt muss Ihr Feather Huzzah ESP8266 mit einem funktionierenden Sensor verbunden werden.

![Herstellen der Verbindung von DHT22 mit Feather Huzzah](media/iot-hub-arduino-huzzah-esp8266-get-started/8_connect-dht22-feather-huzzah.png)

### <a name="connect-feather-huzzah-esp8266-to-your-computer"></a>Verbinden von Feather HUZZAH ESP8266 mit Ihrem Computer

Verwenden Sie, wie im Folgenden gezeigt, das Micro-USB-Kabel (Typ A), um Feather HUZZAH ESP8266 mit Ihrem Computer zu verbinden.

![Herstellen der Verbindung von Feather Huzzah mit Ihrem Computer](media/iot-hub-arduino-huzzah-esp8266-get-started/9_connect-feather-huzzah-computer.png)

### <a name="add-serial-port-permissions-ubuntu-only"></a>Hinzufügen von Berechtigungen für den seriellen Port (nur für Ubuntu)


Bei Verwendung von Ubuntu stellen Sie sicher, dass Sie die Berechtigungen für die Verwendung des USB-Ports von Feather HUZZAH ESP8266 haben. Gehen Sie folgendermaßen vor, um Berechtigungen für den seriellen Anschluss hinzuzufügen:


1. Führen Sie an einem Terminal die folgenden Befehle aus:

   ```bash
   ls -l /dev/ttyUSB*
   ls -l /dev/ttyACM*
   ```

   Sie erhalten eine der folgenden Ausgaben:

   * crw-rw---- 1 root uucp xxxxxxxx
   * crw-rw---- 1 root dialout xxxxxxxx

   Beachten Sie, dass in der Ausgabe `uucp` oder `dialout` der Name des Gruppenbesitzers des USB-Ports ist.

1. Fügen Sie der Gruppe den Benutzer anhand des folgenden Befehls hinzu:

   ```bash
   sudo usermod -a -G <group-owner-name> <username>
   ```

   `<group-owner-name>` ist der Name des Gruppenbesitzers, den Sie im vorherigen Schritt abgerufen haben. `<username>` ist Ihr Ubuntu-Benutzername.

1. Melden Sie sich bei Ubuntu ab, und melden Sie sich erneut an, damit die Änderung angezeigt wird.

## <a name="collect-sensor-data-and-send-it-to-your-iot-hub"></a>Erfassen von Sensordaten und Senden an Ihren IoT Hub

In diesem Abschnitt stellen Sie eine Beispielanwendung auf Feather HUZZAH ESP8266 bereit und führen sie aus. Die Beispielanwendung lässt die LED auf Feather HUZZAH ESP8266 blinken und sendet die erfassten Temperatur- und Luftfeuchtigkeitsdaten vom DHT22-Sensor an Ihren IoT Hub.

### <a name="get-the-sample-application-from-github"></a>Abrufen der Beispielanwendung von GitHub

Die Beispielanwendung wird auf GitHub gehostet. Klonen Sie das Beispielrepository, das die Beispielanwendung aus GitHub enthält. Gehen Sie folgendermaßen vor, um das Beispielrepository zu klonen:

1. Öffnen Sie eine Eingabeaufforderung oder ein Terminalfenster.
1. Wechseln Sie zu einem Ordner, in dem die Beispielanwendung gespeichert werden soll.
1. Führen Sie den folgenden Befehl aus:

   ```bash
   git clone https://github.com/Azure-Samples/iot-hub-feather-huzzah-client-app.git
   ```

Installieren Sie das Paket für Feather HUZZAH ESP8266 in der Arduino-IDE:

1. Öffnen Sie den Ordner, in dem die Beispielanwendung gespeichert ist.
1. Öffnen Sie in der Arduino-IDE im Ordner „app“ die Datei „app.ino“.

   ![Öffnen der Beispielanwendung in der Arduino-IDE](media/iot-hub-arduino-huzzah-esp8266-get-started/10_arduino-ide-open-sample-app.png)

1. Klicken Sie in Arduino IDE auf **File (Datei)** > **Preferences (Einstellungen)**.
1. Klicken Sie im Dialogfeld **Preferences** (Einstellungen) auf das Symbol neben dem Feld **Additional Boards Manager URLs** (Zusätzliche Board-Manager-URLs).
1. Geben Sie im Popup-Fenster die folgende URL ein, und klicken Sie dann auf **OK**.

   `http://arduino.esp8266.com/stable/package_esp8266com_index.json`

   ![Zeigen auf eine Paket-URL in der Arduino-IDE](media/iot-hub-arduino-huzzah-esp8266-get-started/11_arduino-ide-package-url.png)

1. Klicken Sie im Dialogfeld **Preference** (Einstellung) auf **OK**.
1. Klicken Sie auf **Tools** > **Board** > **Boards Manager** (Boards-Manager), und suchen Sie dann nach esp8266.

   Der Boards-Manager gibt an, dass ESP8266 mit der Version 2.2.0 oder höher installiert ist.

   ![Das esp8266-Paket ist installiert.](media/iot-hub-arduino-huzzah-esp8266-get-started/12_arduino-ide-esp8266-installed.png)

1. Klicken Sie auf **Tools** > **Board** > **Adafruit HUZZAH ESP8266**.

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

Die Beispielanwendung kann Temperatur- und Feuchtigkeitsdaten simulieren, falls Sie über keinen echten DHT22-Sensor verfügen. Gehen Sie folgendermaßen vor, um in der Beispielanwendung die Nutzung simulierter Daten einzurichten:

1. Öffnen Sie im `app`-Ordner die `config.h`-Datei.
1. Suchen Sie die folgende Codezeile, und ändern Sie den Wert `false` in `true`:
   ```c
   define SIMULATED_DATA true
   ```
   ![Konfigurieren der Beispielanwendung für die Verwendung simulierter Daten](media/iot-hub-arduino-huzzah-esp8266-get-started/13_arduino-ide-configure-app-use-simulated-data.png)

1. Speichern Sie die Datei als `Control-s`.

### <a name="deploy-the-sample-application-to-feather-huzzah-esp8266"></a>Bereitstellen der Beispielanwendung in Feather HUZZAH ESP8266

1. Klicken Sie in Arduino IDE auf **Tool** > **Port**, und klicken Sie dann auf die seriellen Ports für Feather HUZZAH ESP8266.
1. Klicken Sie auf **Sketch (Skizze)** > **Upload** (Hochladen), um die Beispielanwendung zu erstellen und in Feather HUZZAH ESP8266 bereitzustellen.

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
> Die Anmeldeinformationen werden in EEPROM von Feather HUZZAH ESP8266 gespeichert. Wenn Sie auf dem Feather HUZZAH ESP8266-Board auf die Schaltfläche „Zurücksetzen“ klicken, werden Sie von der Beispielanwendung gefragt, ob Sie die Informationen löschen möchten. Geben Sie `Y` ein, damit die Informationen gelöscht werden. Sie werden aufgefordert, die Anmeldeinformationen ein zweites Mal bereitzustellen.

### <a name="verify-the-sample-application-is-running-successfully"></a>Stellen Sie sicher, dass die Beispielanwendung erfolgreich ausgeführt wird

Wenn Sie die folgende Ausgabe im Fenster des seriellen Monitors und die blinkende LED auf Feather HUZZAH ESP8266 sehen, wird die Beispielanwendung erfolgreich ausgeführt.

![Endgültige Ausgabe in der Arduino-IDE](media/iot-hub-arduino-huzzah-esp8266-get-started/14_arduino-ide-final-output.png)

## <a name="next-steps"></a>Nächste Schritte

Sie haben ein Feather HUZZAH ESP8266 erfolgreich mit Ihrem IoT Hub verbunden und die erfassten Sensordaten an Ihren IoT Hub gesendet. 

[!INCLUDE [iot-hub-get-started-next-steps](../../includes/iot-hub-get-started-next-steps.md)]


