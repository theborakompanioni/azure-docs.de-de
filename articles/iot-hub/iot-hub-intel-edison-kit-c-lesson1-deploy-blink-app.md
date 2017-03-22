---
title: "Verbinden von Intel Edison (C) mit Azure IoT – Lektion 1: Bereitstellen der Anwendung | Microsoft-Dokumentation"
description: "Klonen Sie die C-Beispielanwendung aus GitHub, und führen Sie Gulp zum Bereitstellen der Anwendung auf dem Intel Edison aus. Diese Beispielanwendung bewirkt, dass die mit dem Pi verbundene LED alle zwei Sekunden blinkt."
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: Arduino LED-Projekte, Arduino blinkende LED, Arduino LED-Blinkcode, Arduino Blinkprogramm, Arduino Blinkbeispiel
ms.assetid: b02dfd3f-28fd-4b52-8775-eb0eaf74d707
ms.service: iot-hub
ms.devlang: c
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
translationtype: Human Translation
ms.sourcegitcommit: 475b25f02715a60493e79ecd2170854019dfc4ac
ms.openlocfilehash: c0589d488be5ec62686551b97d8949e5fed2f0a3
ms.lasthandoff: 01/25/2017


---
# <a name="create-and-deploy-the-blink-application"></a>Erstellen und Bereitstellen der Blinkanwendung
## <a name="what-you-will-do"></a>Aufgaben
Klonen Sie die C-Beispielanwendung aus GitHub, und verwenden Sie das Gulp-Tool, um die Anwendung auf dem Intel Edison bereitzustellen. Die Beispielanwendung bewirkt, dass die mit dem Pi verbundene LED alle zwei Sekunden blinkt. Problemlösungen finden Sie auf der [Seite zur Problembehandlung][troubleshooting].

## <a name="what-you-will-learn"></a>Sie lernen Folgendes
* Bereitstellen und Ausführen der Beispielanwendung auf dem Edison

## <a name="what-you-need"></a>Erforderliches Element
Folgende Schritte müssen bereits erfolgreich ausgeführt worden sein:

* [Konfigurieren des Geräts][configure-your-device]
* [Abrufen der Tools][get-the-tools]

## <a name="open-the-sample-application"></a>Öffnen der Beispielanwendung
Gehen Sie folgendermaßen vor, um die Beispielanwendung zu öffnen:

1. Klonen Sie das Beispielrepository aus GitHub, indem Sie den folgenden Befehl ausführen:

   ```bash
   git clone https://github.com/Azure-Samples/iot-hub-c-edison-getting-started.git
   ```
2. Öffnen Sie die Beispielanwendung in Visual Studio Code, indem Sie die folgenden Befehle ausführen:

   ```bash
   cd iot-hub-c-edison-getting-started
   cd Lesson1
   code .
   ```

   ![Repo structure][repo-structure]

Die Datei im Unterordner `app` ist die Hauptquelldatei, die den Code zum Steuern der LED enthält.

### <a name="install-application-dependencies"></a>Installieren der Anwendungsabhängigkeiten
Führen Sie den folgenden Befehl aus, um die Bibliotheken und anderen Module zu installieren, die Sie für die Beispielanwendung benötigen:

```bash
npm install
```

## <a name="configure-the-device-connection"></a>Konfigurieren der Geräteverbindung
Gehen Sie folgendermaßen vor, um die Geräteverbindung zu konfigurieren:

1. Führen Sie zum Generieren der Gerätekonfigurationsdatei den folgenden Befehl aus:

   ```bash
   gulp init
   ```

   Die Konfigurationsdatei `config-edison.json` enthält die Benutzeranmeldeinformationen, mit denen Sie sich bei Edison anmelden. Um die Anmeldeinformationen schützen, wird die Konfigurationsdatei im Unterordner `.iot-hub-getting-started` des Basisordners auf dem Computer generiert.

2. Öffnen Sie die Gerätekonfigurationsdatei in Visual Studio Code, indem Sie den folgenden Befehl ausführen:

   ```bash
   # For Windows command prompt
   code %USERPROFILE%\.iot-hub-getting-started\config-edison.json

   # For MacOS or Ubuntu
   code ~/.iot-hub-getting-started/config-edison.json
   ```

3. Ersetzen Sie die Platzhalter `[device hostname or IP address]` und `[device password]` durch die IP-Adresse und das Kennwort, die Sie in der vorherigen Lektion notiert haben.

   ![config.json](media/iot-hub-intel-edison-lessons/lesson1/vscode-config-mac.png)

Glückwunsch! Sie haben die erste Beispielanwendung für den Edison erstellt.

## <a name="deploy-and-run-the-sample-application"></a>Bereitstellen und Ausführen der Beispielanwendung
### <a name="install-the-azure-iot-hub-sdk-on-edison"></a>Installieren des Azure IoT Hub SDK auf Edison
Führen Sie den folgenden Befehl aus, um das Azure IoT Hub SDK auf Edison zu installieren:

```bash
gulp install-tools
```

Diese Aufgabe kann je nach Ihrer Netzwerkverbindung viel Zeit in Anspruch nehmen. Sie muss pro Edison nur einmal ausgeführt werden.

### <a name="deploy-and-run-the-sample-app"></a>Bereitstellen und Ausführen der Beispiel-App
Führen Sie den folgenden Befehl aus, um die Beispielanwendung bereitzustellen und auszuführen:

```bash
gulp deploy && gulp run
```

### <a name="verify-the-app-works"></a>Überprüfen, ob die Anwendung ordnungsgemäß ausgeführt wird
Die Beispielanwendung wird nach 20-maligem Blinken der LED automatisch beendet. Wenn die LED nicht blinkt, finden Sie in der [Anleitung zur Problembehandlung][troubleshooting] Lösungen für häufige Probleme.

![LED blinkt][led-blinking]

## <a name="summary"></a>Zusammenfassung
Sie haben die erforderlichen Tools zum Arbeiten mit dem Edison installiert und auf dem Edison eine Beispielanwendung bereitgestellt, die das Blinken der LED bewirkt. Im Anschluss können Sie eine weitere Beispielanwendung erstellen, bereitstellen und ausführen, die den Edison mit Azure IoT Hub verbindet, um Nachrichten zu senden und zu empfangen.

## <a name="next-steps"></a>Nächste Schritte
[Abrufen der Azure-Tools ][get-the-azure-tools]

<!-- Images and links -->

[troubleshooting]: iot-hub-intel-edison-kit-c-troubleshooting.md
[Configure-your-device]: iot-hub-intel-edison-kit-c-lesson1-configure-your-device.md
[get-the-tools]: iot-hub-intel-edison-kit-c-lesson1-get-the-tools-win32.md
[repo-structure]: media/iot-hub-intel-edison-lessons/lesson1/repo_structure_c.png
[led-blinking]: media/iot-hub-intel-edison-lessons/lesson1/led_blinking_c.jpg
[get-the-azure-tools]: iot-hub-intel-edison-kit-c-lesson2-get-azure-tools-win32.md

