---
title: "Verbinden von Arduino (C) mit Azure IoT – Lektion 4: C2D | Microsoft-Dokumentation"
description: "Eine Beispielanwendung wird auf dem Adafruit Feather M0 WiFi ausgeführt und überwacht eingehende Nachrichten von IoT Hub. Ein neuer Gulp-Task sendet Nachrichten von Ihrem IoT Hub an Adafruit Feather M0 WiFi, damit die LED blinkt."
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: "Arduino-Kontroll-LED über Web, Arduino-Kontroll-LED über Web"
ms.assetid: a0bf53fb-29fb-485f-ba4a-6c715057b1a2
ms.service: iot-hub
ms.devlang: arduino
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/13/2016
ms.author: xshi
translationtype: Human Translation
ms.sourcegitcommit: 4f9feb665ac93028023df86889ca53a64ac2ec0e
ms.openlocfilehash: 63113841ca836681232e0aa43b15b444f8bb92e2


---
# <a name="run-a-sample-application-to-receive-cloud-to-device-messages"></a>Ausführen einer Beispielanwendung zum Empfangen von C2D-Nachrichten
In diesem Artikel stellen Sie eine Beispielanwendung auf dem Adafruit Feather M0 WiFi Arduino-Board bereit.

Die Beispielanwendung überwacht eingehende Nachrichten von IoT Hub. Sie führen außerdem auf dem Computer einen Gulp-Task aus, um Nachrichten von Ihrem Arduino-Board an den IoT Hub zu senden. Wenn die Beispielanwendung die Nachrichten empfängt, blinkt die LED. Problemlösungen finden Sie auf der [Seite zur Problembehandlung][troubleshooting].

## <a name="what-you-will-do"></a>Aufgaben
* Verbinden der Beispielanwendung mit IoT Hub
* Bereitstellen und Ausführen der Beispielanwendung
* Senden Sie Nachrichten vom IoT Hub an das Arduino-Board, damit die LED blinkt.

## <a name="what-you-will-learn"></a>Sie lernen Folgendes
In diesem Artikel lernen Sie Folgendes:
* Überwachen von eingehenden Nachrichten von IoT Hub
* Senden von C2D-Nachrichten von Ihrem IoT Hub an das Arduino-Board

## <a name="what-you-need"></a>Erforderliches Element
* Richten Sie Ihr Arduino-Board zur Verwendung ein. Informationen zum Einrichten des Arduino-Board finden Sie unter [Konfigurieren Ihres Geräts][configure-your-device].
* Ein in Ihrem Azure-Abonnement erstellter IoT Hub. Informationen zum Erstellen der IoT Hub-Instanz finden Sie unter [Erstellen der Azure IoT Hub-Instanz][create-your-azure-iot-hub].

## <a name="connect-the-sample-application-to-your-iot-hub"></a>Verbinden der Beispielanwendung mit IoT Hub

1. Stellen Sie sicher, dass Sie sich im Repositoryordner `iot-hub-c-feather-m0-getting-started` befinden.

   Öffnen Sie die Beispielanwendung in Visual Studio Code, indem Sie die folgenden Befehle ausführen:

   ```bash
   cd Lesson4
   code .
   ```

   Beachten Sie, dass sich die Datei `app.ino` im Unterordner `app` befindet. Die Datei `app.ino` ist die Hauptquelldatei, die den Code zum Überwachen von eingehenden Nachrichten von IoT Hub enthält. Die `blinkLED`-Funktion bewirkt das Blinken der LED.

   ![Repositorystruktur in der Beispielanwendung][repo-structure]

2. Rufen Sie den seriellen Port des Geräts mit der Geräteermittlungs-CLI ab:

   ```bash
   devdisco list --usb
   ```

   Sie sollten etwa folgende Ausgabe sehen und den USB-COM-Port für das Arduino-Board finden:

   ![Geräteermittlung][device-discovery]

3. Öffnen Sie die Datei `config.json` im Ordner „Lektion“, und geben Sie den Wert der gefundenen COM-Portnummer ein:

   ```json
   {
       "device_port" : "COM1"
   }
   ```

   ![config.json][config-json]

   > [!NOTE]
   > Für den COM-Port weist sie auf der Windows-Plattform das Format `COM1, COM2, ...` auf. Unter MacOS oder Ubuntu beginnt sie mit `/dev/`.

4. Führen Sie zum Initialisieren der Konfigurationsdatei die folgenden Befehle aus:

   ```bash
   # For Windows command prompt
   npm install
   gulp init
   gulp install-tools
   ```

5. Nehmen Sie in der Datei `config-arduino.json` die folgenden Änderungen vor:

   Wenn Sie die Schritte unter [Erstellen einer Azure-Funktionen-App und eines Speicherkontos][create-an-azure-function-app-and-storage-account] auf diesem Computer ausgeführt haben, werden alle Konfigurationen vererbt. Sie können daher diesen Schritt überspringen und mit dem Bereitstellen und Ausführen der Beispielanwendung fortfahren. Wenn Sie die Schritte unter [Erstellen einer Azure-Funktionen-App und eines Speicherkontos][create-an-azure-function-app-and-storage-account] auf einem anderen Computer ausgeführt haben, müssen Sie die Platzhalter in der Datei `config-arduino.json` ersetzen. Die Datei `config-arduino.json` befindet sich im Unterordner des Basisordners.

   ![Inhalt der Datei „config-arduino.json“][config-arduino-json]

   * Ersetzen Sie **[Wi-Fi SSID]** durch die WLAN-SSID, die eine Verbindung mit dem Internet hergestellt hat.
   * Ersetzen Sie **[Wi-Fi password]** durch das WLAN-Kennwort. Entfernen Sie die Zeichenfolge, wenn das WLAN kein Kennwort erfordert.
   * Ersetzen Sie **[IoT device connection string]** durch die Geräteverbindungszeichenfolge, die Sie mit dem Befehl `az iot device show-connection-string --hub-name {my hub name} --device-id {device id}` abrufen.
   * Ersetzen Sie **[IoT hub connection string]** durch die IoT Hub-Verbindungszeichenfolge, die Sie mit dem Befehl `az iot hub show-connection-string --name {my hub name}` abrufen.

## <a name="deploy-and-run-the-sample-application"></a>Bereitstellen und Ausführen der Beispielanwendung
Führen Sie die folgenden Befehle aus, um die Beispielanwendung auf dem Arduino-Board bereitzustellen und auszuführen:

```bash
gulp run
# You can monitor the serial port by running listen task:
gulp listen

# Or you can combine above two gulp tasks into one:
gulp run --listen
```

Mit dem Gulp-Befehl wird die Beispielanwendung auf Ihrem Arduino-Board bereitgestellt. Dann führt er die Anwendung auf dem Arduino-Board und einen gesonderten Task auf dem Hostcomputer aus, um von Ihrem IoT Hub aus 20 Blinknachrichten an das Arduino-Board zu senden.

Nachdem die Beispielanwendung ausgeführt wurde, beginnt sie mit dem Lauschen auf Nachrichten von IoT Hub. In der Zwischenzeit sendet der Gulp-Task mehrere Blinknachrichten von Ihrem IoT Hub aus an das Arduino-Board. Für jede Blinknachricht, die auf dem Board eingeht, ruft die Beispielanwendung die Funktion `blinkLED` auf, um die LED blinken zu lassen.

Die LED sollte alle zwei Sekunden blinken, da der Gulp-Task 20 Nachrichten von Ihrem IoT Hub aus an das Arduino-Board sendet. Die letzte Nachricht ist eine Stoppnachricht, mit der die Anwendung beendet wird.

![Beispielanwendung mit gulp-Befehl und blink-Nachrichten][sample-application]

## <a name="summary"></a>Zusammenfassung
Sie haben erfolgreich Nachrichten von Ihrem IoT Hub aus an das Arduino-Board gesendet, damit die LED blinkt. Die nächste Aufgabe ist optional: das Ändern des Ein- und Ausschaltverhaltens der LED.

## <a name="next-steps"></a>Nächste Schritte
[Ändern des Ein- und Aus-Verhaltens der LED][change-the-on-and-off-led-behavior]


<!-- Images and links -->

[troubleshooting]: iot-hub-adafruit-feather-m0-wifi-kit-arduino-troubleshooting.md
[configure-your-device]: iot-hub-adafruit-feather-m0-wifi-kit-arduino-lesson1-configure-your-device.md
[create-your-azure-iot-hub]: iot-hub-adafruit-feather-m0-wifi-kit-arduino-lesson2-prepare-azure-iot-hub.md
[repo-structure]: media/iot-hub-adafruit-feather-m0-wifi-lessons/lesson4/repo_structure_arduino.png
[device-discovery]: media/iot-hub-adafruit-feather-m0-wifi-lessons/lesson1/device_discovery.png
[config-json]: media/iot-hub-adafruit-feather-m0-wifi-lessons/lesson1/vscode-config-mac.png
[create-an-azure-function-app-and-storage-account]: iot-hub-adafruit-feather-m0-wifi-kit-arduino-lesson3-deploy-resource-manager-template.md
[config-arduino-json]: media/iot-hub-adafruit-feather-m0-wifi-lessons/lesson4/config-arduino.png
[sample-application]: media/iot-hub-adafruit-feather-m0-wifi-lessons/lesson4/gulp_blink_arduino.png
[change-the-on-and-off-led-behavior]: iot-hub-adafruit-feather-m0-wifi-kit-arduino-lesson4-change-led-behavior.md


<!--HONumber=Jan17_HO4-->


