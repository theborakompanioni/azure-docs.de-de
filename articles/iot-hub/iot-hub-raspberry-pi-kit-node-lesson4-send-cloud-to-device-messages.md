---
featureFlags:
- usabilla
title: "Verbinden von Raspberry Pi (Node) mit Azure IoT – Lektion 4: C2D | Microsoft-Dokumentation"
description: "Die Beispielanwendung wird auf dem Pi ausgeführt und überwacht eingehende Nachrichten von IoT Hub. Ein neuer Gulp-Task sendet Nachrichten von IoT Hub an den Raspberry Pi, damit die LED blinkt."
services: iot-hub
documentationcenter: 
author: shizn
manager: timlt
tags: 
keywords: "Cloud-zu-Gerät, Nachricht aus der Cloud"
ms.assetid: 6ae6539e-1163-4490-8d72-fdf7803e3054
ms.service: iot-hub
ms.devlang: node
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
translationtype: Human Translation
ms.sourcegitcommit: 64e69df256404e98f6175f77357500b562d74318
ms.openlocfilehash: 92ee9d6faae9f539c663395e47714609a146f2df
ms.lasthandoff: 01/24/2017


---
# <a name="run-the-sample-application-to-receive-cloud-to-device-messages"></a>Ausführen der Beispielanwendung, um C2D-Nachrichten zu empfangen
In diesem Artikel stellen Sie eine Beispielanwendung auf dem Raspberry Pi 3 bereit. Die Beispielanwendung überwacht eingehende Nachrichten von IoT Hub. Sie führen außerdem auf dem Computer einen Gulp-Task aus, um Nachrichten vom Pi an IoT Hub zu senden. Wenn die Beispielanwendung die Nachrichten empfängt, blinkt die LED. Bei Problemen können Sie sich die Lösungen auf der [Seite zur Problembehandlung](iot-hub-raspberry-pi-kit-node-troubleshooting.md) ansehen.

## <a name="what-you-will-do"></a>Aufgaben
* Verbinden der Beispielanwendung mit IoT Hub
* Bereitstellen und Ausführen der Beispielanwendung
* Senden von Nachrichten von IoT Hub an den Pi, damit die LED blinkt

## <a name="what-you-will-learn"></a>Sie lernen Folgendes
In diesem Artikel lernen Sie Folgendes:
* Überwachen von eingehenden Nachrichten von IoT Hub
* Senden von C2D-Nachrichten von IoT Hub zum Pi

## <a name="what-you-need"></a>Erforderliche Komponenten
* Raspberry Pi 3, bereit zur Verwendung. Informationen zum Einrichten des Pi finden Sie unter [Konfigurieren Ihres Geräts](iot-hub-raspberry-pi-kit-node-lesson1-configure-your-device.md).
* Ein in Ihrem Azure-Abonnement erstellter IoT Hub. Informationen zur Erstellung von IoT Hub finden Sie unter [Create your IoT hub and register Raspberry Pi 3](iot-hub-raspberry-pi-kit-node-lesson2-prepare-azure-iot-hub.md) (Erstellen von IoT Hub und Registrieren des Raspberry Pi 3).

## <a name="connect-the-sample-application-to-your-iot-hub"></a>Verbinden der Beispielanwendung mit IoT Hub
1. Stellen Sie sicher, dass Sie sich im Repositoryordner `iot-hub-node-raspberrypi-getting-started` befinden. Öffnen Sie die Beispielanwendung in Visual Studio Code, indem Sie die folgenden Befehle ausführen:
   
   ```bash
   cd Lesson4
   code .
   ```
   
   Beachten Sie, dass sich die Datei `app.js` im Unterordner `app` befindet. Die Datei `app.js` ist die Hauptquelldatei, die den Code zum Überwachen von eingehenden Nachrichten von IoT Hub enthält. Die `blinkLED`-Funktion bewirkt das Blinken der LED.
   
   ![Repositorystruktur in der Beispielanwendung](media/iot-hub-raspberry-pi-lessons/lesson4/repo_structure.png)
2. Verwenden Sie zum Initialisieren der Konfigurationsdatei die folgenden Befehle:
   
   ```bash
   npm install
   gulp init
   ```
   
   Wenn Sie die Schritte unter [Erstellen einer Azure-Funktionen-App und eines Speicherkontos](iot-hub-raspberry-pi-kit-node-lesson3-deploy-resource-manager-template.md) auf diesem Computer ausgeführt haben, werden alle Konfigurationen vererbt. Sie können den Schritt zum Bereitstellen und Ausführen der Beispielanwendung also überspringen. Falls Sie die Schritte unter [Erstellen einer Azure-Funktionen-App und eines Speicherkontos](iot-hub-raspberry-pi-kit-node-lesson3-deploy-resource-manager-template.md) auf einem anderen Computer ausgeführt haben, müssen Sie die Platzhalter in der Datei `config-raspberrypi.json` ersetzen. Die Datei `config-raspberrypi.json` befindet sich im Unterordner des Basisordners.
   
   ![Inhalt der Datei „config-raspberrypi.json“](media/iot-hub-raspberry-pi-lessons/lesson4/config_raspberrypi.png)

* Ersetzen Sie **[device hostname or IP address]** durch die IP-Adresse des Pi oder den Hostnamen, den Sie durch das Ausführen des Befehls `devdisco list --eth` erhalten.
* Ersetzen Sie **[IoT device connection string]** durch die Geräteverbindungszeichenfolge, die Sie mit dem Befehl `az iot device show-connection-string --hub-name {my hub name} --device-id {device id} -g iot-sample {resource group name}` abrufen.
* Ersetzen Sie **[IoT hub connection string]** durch die IoT Hub-Verbindungszeichenfolge, die Sie mit dem Befehl `az iot hub show-connection-string --name {my hub name} -g iot-sample {resource group name}` abrufen.

> [!NOTE]
> Führen Sie außerdem **gulp install-tools** aus, falls Sie dies in Lektion 1 noch nicht getan haben.

## <a name="deploy-and-run-the-sample-application"></a>Bereitstellen und Ausführen der Beispielanwendung
Führen Sie den folgenden Befehl aus, um die Beispielanwendung auf dem Pi bereitzustellen und auszuführen:

```bash
gulp deploy && gulp run
```

Mit dem Befehl wird die Beispielanwendung auf dem Pi bereitgestellt. Anschließend führt er die Anwendung auf dem Pi und einen gesonderten Task auf dem Hostcomputer aus, um von IoT Hub 20 blink-Nachrichten an den Pi zu senden.

Nachdem die Beispielanwendung ausgeführt wurde, beginnt sie mit dem Lauschen auf Nachrichten von IoT Hub. In der Zwischenzeit sendet der Gulp-Task mehrere blink-Nachrichten von IoT Hub an den Pi. Für jede blink-Nachricht, die auf dem Pi eingeht, ruft die Beispielanwendung die Funktion `blinkLED` auf, um die LED blinken zu lassen.

Die LED sollte alle zwei Sekunden blinken, da der Gulp-Task 20 Nachrichten von IoT Hub an den Pi sendet. Die letzte Nachricht ist eine stop-Nachricht, mit der die Anwendung beendet wird.

![Beispielanwendung mit gulp-Befehl und blink-Nachrichten](media/iot-hub-raspberry-pi-lessons/lesson4/gulp_blink.png)

## <a name="summary"></a>Zusammenfassung
Sie haben erfolgreich Nachrichten von IoT Hub an den Pi gesendet, damit die LED blinkt. Die nächste Aufgabe ist optional: das Ändern des Ein- und Ausschaltverhaltens der LED.

## <a name="next-steps"></a>Nächste Schritte
[Change the on and off behavior of the LED](iot-hub-raspberry-pi-kit-node-lesson4-change-led-behavior.md) (Ändern des Ein- und Aus-Verhaltens der LED)


