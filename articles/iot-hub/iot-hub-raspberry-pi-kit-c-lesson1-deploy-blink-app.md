---
title: "Verbinden von Raspberry Pi (C) mit Azure IoT – Lektion 1: Bereitstellen der App | Microsoft-Dokumentation"
description: Klonen Sie die C-Beispielanwendung aus GitHub, und verwenden Sie Gulp, um die Anwendung auf dem Raspberry Pi 3 bereitzustellen. Diese Beispielanwendung bewirkt, dass die mit dem Pi verbundene LED alle zwei Sekunden blinkt.
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: Raspberry Pi blinkende LED, blinkende LED mit Raspberry Pi
ms.assetid: f601d1e1-2bc3-4cc5-a6b1-0467e5304dcf
ms.service: iot-hub
ms.devlang: c
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
translationtype: Human Translation
ms.sourcegitcommit: 64e69df256404e98f6175f77357500b562d74318
ms.openlocfilehash: 5b16230178fa82c7f227e3e3f8742fa16b00c719
ms.lasthandoff: 01/24/2017


---
# <a name="create-and-deploy-the-blink-application"></a>Erstellen und Bereitstellen der Blinkanwendung
## <a name="what-you-will-do"></a>Aufgaben
Klonen Sie die C-Beispielanwendung aus GitHub, und verwenden Sie das Gulp-Tool, um die Anwendung auf dem Raspberry Pi 3 bereitzustellen. Die Beispielanwendung bewirkt, dass die mit dem Pi verbundene LED alle zwei Sekunden blinkt. Problemlösungen finden Sie auf der [Seite zur Problembehandlung](iot-hub-raspberry-pi-kit-c-troubleshooting.md).

## <a name="what-you-will-learn"></a>Sie lernen Folgendes
In diesem Artikel lernen Sie Folgendes:

* Verwenden des Tools `device-discover-cli` zum Abrufen der Netzwerkinformationen zu Pi
* Bereitstellen und Ausführen der Beispielanwendung auf Pi
* Bereitstellen und Debuggen von Anwendungen, die remote auf Pi ausgeführt werden

## <a name="what-you-need"></a>Voraussetzungen
Folgende Schritte müssen bereits erfolgreich ausgeführt worden sein:

* [Konfigurieren des Geräts](iot-hub-raspberry-pi-kit-c-lesson1-configure-your-device.md)
* [Abrufen der Tools](iot-hub-raspberry-pi-kit-c-lesson1-get-the-tools-win32.md)

## <a name="obtain-the-ip-address-and-host-name-of-pi"></a>Abrufen der IP-Adresse und des Hostnamens für Pi
Öffnen Sie eine Eingabeaufforderung (unter Windows) oder ein Terminal (unter macOS oder Ubuntu), und führen Sie dann den folgenden Befehl aus:

```bash
devdisco list --eth
```

Anschließend sollte die folgende Ausgabe angezeigt werden:

![Geräteermittlung](media/iot-hub-raspberry-pi-lessons/lesson1/device_discovery.png)

Notieren Sie sich die Werte für `IP address` und `hostname` von Pi. Diese Informationen werden später in diesem Artikel benötigt.

> [!NOTE]
> Vergewissern Sie sich, dass Pi mit dem gleichen Netzwerk verbunden ist wie der Computer. Wenn der Computer beispielsweise mit einem Drahtlosnetzwerk und Pi mit einem verkabelten Netzwerk verbunden ist, wird in der Ausgabe des devdisco-Befehls die IP-Adresse möglicherweise nicht angezeigt.

## <a name="open-the-sample-application"></a>Öffnen der Beispielanwendung
Gehen Sie folgendermaßen vor, um die Beispielanwendung zu öffnen:

1. Klonen Sie das Beispielrepository aus GitHub, indem Sie den folgenden Befehl ausführen:
   
    ```bash
    git clone https://github.com/Azure-Samples/iot-hub-c-raspberrypi-getting-started.git
    ```
2. Öffnen Sie die Beispielanwendung in Visual Studio Code, indem Sie die folgenden Befehle ausführen:
   
    ```bash
    cd iot-hub-c-raspberrypi-getting-started
    cd Lesson1
    code .
    ```

![Repo structure](media/iot-hub-raspberry-pi-lessons/lesson1/vscode-blink-c-mac.png)

Die Datei `main.c` im Unterordner `app` ist die Hauptquelldatei, die den Code zum Steuern der LED enthält.

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
   
   Die Konfigurationsdatei `config-raspberrypi.json` enthält die Benutzeranmeldeinformationen, mit denen Sie sich bei Pi anmelden. Um die Anmeldeinformationen schützen, wird die Konfigurationsdatei im Unterordner `.iot-hub-getting-started` des Basisordners auf dem Computer generiert.

2. Öffnen Sie die Gerätekonfigurationsdatei in Visual Studio Code, indem Sie den folgenden Befehl ausführen:
   
   ```bash
   # For Windows command prompt
   code %USERPROFILE%\.iot-hub-getting-started\config-raspberrypi.json
   
   # For MacOS or Ubuntu
   code ~/.iot-hub-getting-started/config-raspberrypi.json
   ```

3. Ersetzen Sie den Platzhalter `[device hostname or IP address]` durch die IP-Adresse oder den Hostnamen, die bzw. den Sie zuvor im Schritt „Abrufen der IP-Adresse und des Hostnamens für Pi“ ermittelt haben.
   
   ![config.json](media/iot-hub-raspberry-pi-lessons/lesson1/vscode-config-mac.png)

> [!NOTE]
> Beim Herstellen der Verbindung mit Raspberry Pi können Sie anstelle von Benutzername und Kennwort den SSH-Schlüssel verwenden. Hierzu müssen Sie den Schlüssel mithilfe von **ssh-keygen** und **ssh-copy-id pi@\<Geräteadresse\>** generieren.
>
> Unter Windows stehen diese Befehle in **Git Bash** zur Verfügung.
>
> Unter macOS müssen Sie **brew install ssh-copy-id** ausführen.
>
> Ersetzen Sie **device_password** in **Config raspberrypi.json** durch die Eigenschaft **device_key_path**, nachdem Sie den Schlüssel erfolgreich an Raspberry Pi hochgeladen haben.
>
> Die aktualisierten Zeilen sollten in etwa wie folgt aussehen:
> ```javascript
> "device_user_name": "pi",
> "device_key_path": "id_rsa",
> ```

Glückwunsch! Sie haben die erste Beispielanwendung für Pi erstellt.

## <a name="deploy-and-run-the-sample-application"></a>Bereitstellen und Ausführen der Beispielanwendung
### <a name="install-the-azure-iot-hub-sdk-on-pi"></a>Installieren des Azure IoT Hub SDK auf Pi
Führen Sie den folgenden Befehl aus, um das Azure IoT Hub SDK auf dem Pi zu installieren:

```bash
gulp install-tools
```

Die erstmalige Ausführung kann einige Minuten dauern.

### <a name="deploy-and-run-the-sample-app"></a>Bereitstellen und Ausführen der Beispiel-App
Führen Sie den folgenden Befehl aus, um die Beispielanwendung bereitzustellen und auszuführen:

```bash
gulp deploy && gulp run
```

### <a name="verify-the-app-works"></a>Überprüfen, ob die Anwendung ordnungsgemäß ausgeführt wird
Die Beispielanwendung wird nach 20-maligem Blinken der LED automatisch beendet. Wenn die LED nicht blinkt, suchen Sie im [Handbuch zur Problembehandlung](iot-hub-raspberry-pi-kit-c-troubleshooting.md) Lösungen für häufige Probleme.
![LED blinkt](media/iot-hub-raspberry-pi-lessons/lesson1/led_blinking.jpg)

## <a name="summary"></a>Zusammenfassung
Sie haben die erforderlichen Tools für die Arbeit mit Pi installiert und auf Pi eine Beispielanwendung bereitgestellt, die das Blinken der LED bewirkt. Im Anschluss können Sie eine weitere Beispielanwendung erstellen, bereitstellen und ausführen, die Pi mit Azure IoT Hub verbindet, um Nachrichten zu senden und zu empfangen.

## <a name="next-steps"></a>Nächste Schritte
[Abrufen der Azure-Tools ](iot-hub-raspberry-pi-kit-c-lesson2-get-azure-tools-win32.md)


