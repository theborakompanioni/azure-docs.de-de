---
title: "Verbinden von Arduino mit Azure IoT – Lektion 1: Bereitstellen der App | Microsoft-Dokumentation"
description: "Klonen Sie die Arduino-Beispielanwendung von GitHub, und führen Sie Gulp aus, um diese Anwendung für Adafruit Feather M0 WiFi bereitzustellen. Bei dieser Beispielanwendung blinkt die GPIO-LED."
services: iot-hub
documentationcenter: 
author: shizn
manager: timlt
tags: 
keywords: Arduino LED-Projekte, Arduino blinkende LED, Arduino LED-Blinkcode, Arduino Blinkprogramm, Arduino Blinkbeispiel
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-adafruit-feather-m0-wifi-kit-arduino-get-started
ms.assetid: b0a7d076-d580-4686-9f7d-c0712750b615
ms.service: iot-hub
ms.devlang: arduino
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.translationtype: Human Translation
ms.sourcegitcommit: 64e69df256404e98f6175f77357500b562d74318
ms.openlocfilehash: a2058611cecb33eab04de9249ad73b1c4ec6c2a5
ms.contentlocale: de-de
ms.lasthandoff: 01/24/2017

---
<a id="create-and-deploy-the-blink-application" class="xliff"></a>

# Erstellen und Bereitstellen der Blinkanwendung
<a id="what-you-will-do" class="xliff"></a>

## Aufgaben
Klonen Sie die Arduino-Beispielanwendung von GitHub, und verwenden Sie das Tool „Gulp“, um die Beispielanwendung für Ihr Adafruit Feather M0 WiFi-Arduino-Board bereitzustellen. Bei der Beispielanwendung blinkt die integrierte GPIO #13-LED alle zwei Sekunden.

Problemlösungen finden Sie auf der [Seite zur Problembehandlung][troubleshooting-page].

<a id="what-you-will-learn" class="xliff"></a>

## Sie lernen Folgendes
* Bereitstellen und Ausführen der Beispielanwendung auf dem Arduino-Board

<a id="what-you-need" class="xliff"></a>

## Erforderliches Element
Folgende Schritte müssen bereits erfolgreich ausgeführt worden sein:

* [Konfigurieren des Geräts][configure-your-device]
* [Abrufen der Tools][get-the-tools]

<a id="open-the-sample-application" class="xliff"></a>

## Öffnen der Beispielanwendung
Gehen Sie folgendermaßen vor, um die Beispielanwendung zu öffnen:

1. Klonen Sie das Beispielrepository aus GitHub, indem Sie den folgenden Befehl ausführen:

   ```bash
   git clone https://github.com/Azure-Samples/iot-hub-c-feather-m0-getting-started.git
   ```
2. Öffnen Sie die Beispielanwendung in Visual Studio Code, indem Sie die folgenden Befehle ausführen:

   ```bash
   cd iot-hub-c-feather-m0-getting-started
   cd Lesson1
   code .
   ```

   ![Repo structure][repo-structure]

Die Datei `app.ino` im Unterordner `app` ist die Hauptquelldatei, die den Code zum Steuern der LED enthält.

<a id="install-application-dependencies" class="xliff"></a>

### Installieren der Anwendungsabhängigkeiten
Führen Sie den folgenden Befehl aus, um die Bibliotheken und anderen Module zu installieren, die Sie für die Beispielanwendung benötigen:

```bash
npm install
```

<a id="configure-the-device-connection" class="xliff"></a>

## Konfigurieren der Geräteverbindung
Gehen Sie folgendermaßen vor, um die Geräteverbindung zu konfigurieren:

1. Rufen Sie den seriellen Port des Geräts mit der Geräteermittlungs-CLI ab:

   ```bash
   devdisco list --usb
   ```

   Sie sollten etwa folgende Ausgabe sehen und den USB-COM-Port für das Arduino-Board finden: ![Geräteermittlung][device-discovery]

2. Öffnen Sie die Datei `config.json` im Ordner „Lektion“, und fügen Sie den Wert der gefundenen COM-Portnummer hinzu:

   ```json
   {
       "device_port" : "COM1"
   }
   ```
   ![config.json][config-json]
   > [!NOTE]
   > Für den COM-Port weist sie auf der Windows-Plattform das Format `COM1, COM2, ...` auf. Unter MacOS oder Ubuntu beginnt sie mit `/dev/`.

<a id="deploy-and-run-the-sample-application" class="xliff"></a>

## Bereitstellen und Ausführen der Beispielanwendung
<a id="install-the-required-tools-for-your-arduino-board" class="xliff"></a>

### Installieren der erforderlichen Tools für Ihr Arduino-Board

Installieren Sie das Azure IoT Hub SDK für das Arduino-Board, indem Sie den folgenden Befehl ausführen:

```bash
gulp install-tools
```

Diese Aufgabe kann je nach Ihrer Netzwerkverbindung viel Zeit in Anspruch nehmen.

> [!NOTE]
> Beenden Sie die aktive Arduino-IDE-Instanz, wenn Sie Gulp-Aufgaben ausführen: `install-tools`, `run`.

<a id="deploy-and-run-the-sample-app" class="xliff"></a>

### Bereitstellen und Ausführen der Beispiel-App
Führen Sie den folgenden Befehl aus, um die Beispielanwendung bereitzustellen und auszuführen:

```bash
gulp run

# You can monitor the serial port by running listen task:
gulp listen

# Or you can combine above two gulp tasks into one:
gulp run --listen
```

<a id="verify-the-app-works" class="xliff"></a>

### Überprüfen, ob die Anwendung ordnungsgemäß ausgeführt wird
Wenn die LED nicht blinkt, helfen Ihnen die Lösungen für häufige Probleme in der [Anleitung zur Problembehandlung][troubleshooting-page] weiter.

![LED blinkt][led-blinking]

<a id="summary" class="xliff"></a>

## Zusammenfassung
Sie haben die erforderlichen Tools zum Arbeiten mit dem Arduino-Board installiert und darauf eine Beispielanwendung bereitgestellt, die das Blinken der LED bewirkt. Im Anschluss können Sie eine weitere Beispielanwendung erstellen, bereitstellen und ausführen, die das Arduino-Board mit Azure IoT Hub verbindet, um Nachrichten zu senden und zu empfangen.

<a id="next-steps" class="xliff"></a>

## Nächste Schritte
[Abrufen der Azure-Tools ][get-the-azure-tools]

<!-- Images and links -->

[troubleshooting-page]: iot-hub-adafruit-feather-m0-wifi-kit-arduino-troubleshooting.md
[configure-your-device]: iot-hub-adafruit-feather-m0-wifi-kit-arduino-lesson1-configure-your-device.md
[get-the-tools]: iot-hub-adafruit-feather-m0-wifi-kit-arduino-lesson1-get-the-tools-win32.md
[repo-structure]: media/iot-hub-adafruit-feather-m0-wifi-lessons/lesson1/vscode-blink-arduino-mac.png
[device-discovery]: media/iot-hub-adafruit-feather-m0-wifi-lessons/lesson1/device_discovery.png
[config-json]: media/iot-hub-adafruit-feather-m0-wifi-lessons/lesson1/vscode-config-mac.png
[led-blinking]: media/iot-hub-adafruit-feather-m0-wifi-lessons/lesson1/led_blinking.png
[get-the-azure-tools]: iot-hub-adafruit-feather-m0-wifi-kit-arduino-lesson2-get-azure-tools-win32.md
