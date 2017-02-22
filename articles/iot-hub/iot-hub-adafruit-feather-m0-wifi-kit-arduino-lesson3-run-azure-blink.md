---
title: "Verbinden von Arduino (C) mit Azure IoT – Lektion 3: Ausführen des Beispiels | Microsoft-Dokumentation"
description: "Stellen Sie eine Beispielanwendung auf Adafruit Feather M0 WiFi bereit, die Nachrichten an IoT Hub sendet und ein Blinken der LED auslöst, und führen Sie sie aus."
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: IoT-Clouddienst, Arduino-Daten an Cloud senden
ms.assetid: 92cce319-2b17-4c9b-889d-deac959e3e7c
ms.service: iot-hub
ms.devlang: arduino
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/13/2016
ms.author: xshi
translationtype: Human Translation
ms.sourcegitcommit: 64e69df256404e98f6175f77357500b562d74318
ms.openlocfilehash: 631a6677e4801b1c3475b9e8f2726219dd101132


---
# <a name="run-a-sample-application-to-send-device-to-cloud-messages"></a>Ausführen einer Beispielanwendung zum Senden von D2C-Nachrichten
## <a name="what-you-will-do"></a>Aufgaben
Dieser Artikel zeigt, wie Sie eine Beispielanwendung auf dem Adafruit Feather M0 WiFi Arduino-Board bereitstellen und ausführen, das Nachrichten an den IoT-Hub sendet.

Sollten Probleme auftreten, finden Sie Lösungen auf der [Seite zur Problembehandlung][troubleshooting].

## <a name="what-you-will-learn"></a>Sie lernen Folgendes
Sie erfahren, wie Sie das Gulp-Tool zum Bereitstellen und Ausführen der Arduino-Beispielanwendung auf dem Arduino-Board verwenden.

## <a name="what-you-need"></a>Erforderliches Element
* Bevor Sie mit dieser Aufgabe beginnen, müssen Sie [Create an Azure function app and a storage account to process and store IoT hub messages][process-and-store-iot-hub-messages] (Erstellen einer Azure-Funktionen-App und eines Azure-Speicherkontos zum Verarbeiten und Speichern von IoT Hub-Nachrichten) erfolgreich abgeschlossen haben.

## <a name="get-your-iot-hub-and-device-connection-strings"></a>Abrufen der IoT Hub- und Geräteverbindungszeichenfolgen
Die Geräteverbindungszeichenfolge dient zum Verbinden des Arduino-Boards mit dem IoT Hub. Die IoT Hub-Verbindungszeichenfolge dient zum Verbinden des IoT Hub mit der Geräteidentität, die das Arduino-Board im IoT Hub darstellt.

* Listen Sie alle IoT Hubs in der Ressourcengruppe auf, indem Sie den folgenden Azure-CLI-Befehl ausführen:

```bash
az iot hub list -g iot-sample --query [].name
```

Verwenden Sie `iot-sample` als Wert von `{resource group name}`, sofern Sie ihn nicht geändert haben.

* Führen Sie zum Abrufen der IoT Hub-Verbindungszeichenfolge den folgenden Azure-CLI-Befehl aus:

```bash
az iot hub show-connection-string --name {my hub name}
```

`{my hub name}` ist der Name, den Sie beim Erstellen des IoT Hubs und Registrieren des Arduino-Boards angegeben haben.

* Führen Sie zum Abrufen der Geräteverbindungszeichenfolge den folgenden Befehl aus:

```bash
az iot device show-connection-string --hub-name {my hub name} --device-id mym0wifi
```

Verwenden Sie `mym0wifi` als Wert von `{device id}`, sofern Sie ihn nicht geändert haben.
## <a name="configure-the-device-connection"></a>Konfigurieren der Geräteverbindung
Gehen Sie folgendermaßen vor, um die Geräteverbindung zu konfigurieren:

1. Rufen Sie den seriellen Port des Geräts mit der Geräteermittlungs-CLI ab:

   ```bash
   devdisco list --usb
   ```

   Sie sollten etwa folgende Ausgabe sehen und den USB-COM-Port für das Arduino-Board finden:

   ![Geräteermittlung][device-discovery]

2. Öffnen Sie die Datei `config.json` im Ordner „Lektion“, und fügen Sie den Wert der gefundenen COM-Portnummer hinzu:

   ```json
   {
       "device_port" : "COM1"
   }
   ```

   ![config.json][config-json]

   > [!NOTE]
   > Für den COM-Port weist sie auf der Windows-Plattform das Format `COM1, COM2, ...` auf. Unter MacOS oder Ubuntu beginnt sie mit `/dev/`.

3. Führen Sie zum Initialisieren der Konfigurationsdatei die folgenden Befehle aus:

   ```bash
   npm install
   gulp init
   gulp install-tools
   ```
4. Öffnen Sie die Gerätekonfigurationsdatei `config-arduino.json` in Visual Studio Code, indem Sie den folgenden Befehl ausführen:

   ```bash
   # For Windows command prompt
   code %USERPROFILE%\.iot-hub-getting-started\config-arduino.json

   # For MacOS or Ubuntu
   code ~/.iot-hub-getting-started/config-arduino.json
   ```

   ![config-arduino.json][config-arduino-json]

5. Nehmen Sie in der Datei `config-arduino.json` die folgenden Änderungen vor:

   * Ersetzen Sie **[Wi-Fi SSID]** durch die WLAN-SSID, die eine Verbindung mit dem Internet hergestellt hat.
   * Ersetzen Sie **[Wi-Fi password]** durch das WLAN-Kennwort. Entfernen Sie die Zeichenfolge, wenn das WLAN kein Kennwort erfordert.
   * Ersetzen Sie **[IoT device connection string]** durch den `device connection string`, den Sie abgerufen haben.
   * Ersetzen Sie **[IoT hub connection string]** durch den `iot hub connection string`, den Sie abgerufen haben.

   > [!NOTE]
   > `azure_storage_connection_string` benötigen Sie in diesem Artikel nicht. Nehmen Sie keine Änderungen vor.

## <a name="deploy-and-run-the-sample-application"></a>Bereitstellen und Ausführen der Beispielanwendung
Führen Sie den folgenden Befehl aus, um die Beispielanwendung auf dem Arduino-Board bereitzustellen und auszuführen:

```bash
gulp run
# You can monitor the serial port by running listen task:
gulp listen

# Or you can combine above two gulp tasks into one:
gulp run --listen
```

> [!NOTE]
> Der Gulp-Standardtask führt nacheinander die Tasks `install-tools` und `run` aus. Als Sie die [Blink-App bereitgestellt haben][deployed-the-blink-app], haben Sie diese Tasks separat durchgeführt.

## <a name="verify-that-the-sample-application-works"></a>Überprüfen, ob die Beispielanwendung ordnungsgemäß ausgeführt wird
Die integrierte GPIO #0-LED sollte alle zwei Sekunden blinken. Bei jedem Blinken der LED sendet die Beispielanwendung eine Nachricht an IoT Hub und überprüft, ob die Nachricht erfolgreich an IoT Hub gesendet wurde. Darüber hinaus wird jede von IoT Hub empfangene Nachricht im Konsolenfenster ausgegeben. Die Beispielanwendung wird automatisch beendet, nachdem 20 Nachrichten gesendet wurden.

![Beispielanwendung mit gesendeten und empfangenen Nachrichten][sample-application-with-sent-and-received-messages]

## <a name="summary"></a>Zusammenfassung
Sie haben die neue Blinkbeispielanwendung auf dem Arduino-Board bereitgestellt und ausgeführt, um D2C-Nachrichten an IoT Hub zu senden. Sie können nun Nachrichten überwachen, wenn sie in das Speicherkonto geschrieben werden.

## <a name="next-steps"></a>Nächste Schritte
[Lesen von Nachrichten in Azure Storage][read-messages-persisted-in-azure-storage]
<!-- Images and links -->

[troubleshooting]: iot-hub-adafruit-feather-m0-wifi-kit-arduino-troubleshooting.md
[process-and-store-iot-hub-messages]: iot-hub-adafruit-feather-m0-wifi-kit-arduino-lesson3-deploy-resource-manager-template.md
[device-discovery]: media/iot-hub-adafruit-feather-m0-wifi-lessons/lesson1/device_discovery.png
[config-json]: media/iot-hub-adafruit-feather-m0-wifi-lessons/lesson1/vscode-config-mac.png
[config-arduino-json]: media/iot-hub-adafruit-feather-m0-wifi-lessons/lesson3/config-arduino.png
[deployed-the-blink-app]: iot-hub-adafruit-feather-m0-wifi-kit-arduino-lesson1-deploy-blink-app.md
[sample-application-with-sent-and-received-messages]: media/iot-hub-adafruit-feather-m0-wifi-lessons/lesson3/gulp_run_arduino.png
[read-messages-persisted-in-azure-storage]: iot-hub-adafruit-feather-m0-wifi-kit-arduino-lesson3-read-table-storage.md


<!--HONumber=Jan17_HO4-->


