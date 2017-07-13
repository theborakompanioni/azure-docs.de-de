---
title: "Verbinden von Arduino (C) mit Azure IoT – Lektion 3: Table Storage | Microsoft-Dokumentation"
description: "Sie überwachen D2C-Nachrichten (Device-to-Cloud, Gerät-zu-Cloud), während diese in Azure Table Storage geschrieben werden."
services: iot-hub
documentationcenter: 
author: shizn
manager: timlt
tags: 
keywords: Daten in der Cloud, Datensammlung in der Cloud, IoT-Clouddienst, IoT-Daten
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-adafruit-feather-m0-wifi-kit-arduino-get-started
ms.assetid: 386083e0-0dbb-48c0-9ac2-4f8fb4590772
ms.service: iot-hub
ms.devlang: arduino
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.translationtype: Human Translation
ms.sourcegitcommit: 64e69df256404e98f6175f77357500b562d74318
ms.openlocfilehash: 7a49a15b00bf25c9416235f41da8cc86e54d0062
ms.contentlocale: de-de
ms.lasthandoff: 01/24/2017

---
<a id="read-messages-persisted-in-azure-storage" class="xliff"></a>

# Lesen von Nachrichten in Azure Storage
<a id="what-you-will-do" class="xliff"></a>

## Aufgaben
Sie überwachen die D2C-Nachrichten, die von Ihrem Arduino-Board Adafruit Feather M0 WiFi an IoT Hub gesendet werden, während diese in Azure Table Storage geschrieben werden.

Problemlösungen finden Sie auf der [Seite zur Problembehandlung][troubleshooting].

<a id="what-you-will-learn" class="xliff"></a>

## Sie lernen Folgendes
In diesem Artikel erfahren Sie, wie Sie den Gulp-Task „read-message“ zum Lesen von Nachrichten verwenden, die in Azure Table Storage gespeichert sind.

<a id="what-you-need" class="xliff"></a>

## Erforderliches Element
Bevor Sie diesen Prozess starten, müssen Sie den Abschnitt [Ausführen der Azure-Beispielanwendung blink auf Ihrem Arduino-Board][run-blink-application] erfolgreich abgeschlossen haben.

<a id="read-new-messages-from-your-storage-account" class="xliff"></a>

## Lesen neuer Nachrichten aus dem Speicherkonto
Im vorherigen Artikel haben Sie eine Beispielanwendung auf dem Arduino-Board ausgeführt. Die Beispielanwendung hat Nachrichten an Ihre Azure IoT Hub-Instanz gesendet. Die an Ihren IoT-Hub gesendeten Nachrichten wurden über die Azure-Funktionen-App in Azure Table Storage gespeichert. Sie benötigen die Azure Storage-Verbindungszeichenfolge für das Lesen von Nachrichten aus Azure Table Storage.

Gehen Sie folgendermaßen vor, um Nachrichten zu lesen, die in Azure Table Storage gespeichert sind:

1. Fragen Sie die Verbindungszeichenfolge ab, indem Sie die folgenden Befehle ausführen:

   ```bash
   az storage account list -g iot-sample --query [].name
   az storage account show-connection-string -g iot-sample -n {storage name}
   ```

   Der erste Befehl ruft den `storage name` ab, der im zweiten Befehl zum Abrufen der Verbindungszeichenfolge verwendet wird. Verwenden Sie `iot-sample` als Wert von `{resource group name}`, sofern Sie ihn nicht geändert haben.
2. Öffnen Sie die Konfigurationsdatei `config-arduino.json` in Visual Studio Code, indem Sie den folgenden Befehl ausführen:

   ```bash
   # For Windows command prompt
   code %USERPROFILE%\.iot-hub-getting-started\config-arduino.json

   # For MacOS or Ubuntu
   code ~/.iot-hub-getting-started/config-arduino.json
   ```
3. Ersetzen Sie `[Azure storage connection string]` durch die Verbindungszeichenfolge, die Sie in Schritt 1 abgerufen haben.
4. Speichern Sie die Datei `config-arduino.json`.
5. Senden Sie Nachrichten erneut, und lesen Sie diese aus Azure Table Storage, indem Sie den folgenden Befehl ausführen:

   ```bash
   gulp run --read-storage

   # You can monitor the serial port by running listen task:
   gulp listen

   # Or you can combine above two gulp tasks into one:
   gulp run --read-storage --listen
   ```

   Die Logik für das Lesen in Azure Table Storage ist in der Datei `azure-table.js` enthalten.

   ![gulp run --read-storage][gulp-run]

<a id="summary" class="xliff"></a>

## Zusammenfassung
Sie haben das Arduino-Board erfolgreich mit Ihrer IoT Hub-Instanz in der Cloud verbunden und die Beispielanwendung blink verwendet, um D2C-Nachrichten zu senden. Sie können die Azure-Funktionen-App auch dazu verwenden, eingehende IoT Hub-Nachrichten in Azure Table Storage zu speichern. Sie können nun C2D-Nachrichten von IoT Hub an das Arduino-Board senden.

<a id="next-steps" class="xliff"></a>

## Nächste Schritte
[Senden von C2D-Nachrichten][send-cloud-to-device-messages]
<!-- Images and links -->

[troubleshooting]: iot-hub-adafruit-feather-m0-wifi-kit-arduino-troubleshooting.md
[run-blink-application]: iot-hub-adafruit-feather-m0-wifi-kit-arduino-lesson3-run-azure-blink.md
[gulp-run]: media/iot-hub-adafruit-feather-m0-wifi-lessons/lesson3/gulp_read_message_arduino.png
[send-cloud-to-device-messages]: iot-hub-adafruit-feather-m0-wifi-kit-arduino-lesson4-send-cloud-to-device-messages.md
