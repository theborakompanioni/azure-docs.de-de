---
title: "Verbinden von Intel Edison (C) mit Azure IoT – Lektion 4: Empfangen von Nachrichten | Microsoft-Dokumentation"
description: "Eine Beispielanwendung wird auf Edison ausgeführt und überwacht eingehende Nachrichten von IoT Hub. Ein neuer Gulp-Task sendet Nachrichten vom IoT Hub an Edison, damit die LED blinkt."
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: "Arduino-Kontroll-LED über Web, Arduino-Kontroll-LED über Web"
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-intel-edison-kit-c-get-started
ms.assetid: 820d38f3-d3b8-4249-9e2b-f1b9b771e62f
ms.service: iot-hub
ms.devlang: c
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
translationtype: Human Translation
ms.sourcegitcommit: 475b25f02715a60493e79ecd2170854019dfc4ac
ms.openlocfilehash: b4d76eeb1bc0b406800c89c4952900350d8eafa8
ms.lasthandoff: 01/25/2017


---
# <a name="run-a-sample-application-to-receive-cloud-to-device-messages"></a>Ausführen einer Beispielanwendung zum Empfangen von C2D-Nachrichten
In diesem Artikel stellen Sie eine Beispielanwendung auf Intel Edison bereit. Die Beispielanwendung überwacht eingehende Nachrichten von IoT Hub. Sie führen außerdem auf dem Computer einen Gulp-Task aus, um Nachrichten vom IoT Hub an Edison zu senden. Wenn die Beispielanwendung die Nachrichten empfängt, blinkt die LED. Problemlösungen finden Sie auf der [Seite zur Problembehandlung][troubleshooting].

## <a name="what-you-will-do"></a>Aufgaben
* Verbinden der Beispielanwendung mit IoT Hub
* Bereitstellen und Ausführen der Beispielanwendung
* Senden von Nachrichten vom IoT Hub an Edison, damit die LED blinkt

## <a name="what-you-will-learn"></a>Sie lernen Folgendes
In diesem Artikel lernen Sie Folgendes:
* Überwachen von eingehenden Nachrichten von IoT Hub
* Senden von C2D-Nachrichten von IoT Hub an Edison

## <a name="what-you-need"></a>Erforderliches Element
* Intel Edison, bereit zur Verwendung. Informationen zum Einrichten von Edison finden Sie unter [Konfigurieren Ihres Geräts][configure-your-device].
* Ein in Ihrem Azure-Abonnement erstellter IoT Hub. Informationen zum Erstellen der IoT Hub-Instanz finden Sie unter [Erstellen der Azure IoT Hub-Instanz][create-your-azure-iot-hub].

## <a name="connect-the-sample-application-to-your-iot-hub"></a>Verbinden der Beispielanwendung mit IoT Hub
1. Stellen Sie sicher, dass Sie sich im Repositoryordner `iot-hub-c-edison-getting-started` befinden. Öffnen Sie die Beispielanwendung in Visual Studio Code, indem Sie die folgenden Befehle ausführen:

   ```bash
   cd Lesson4
   code .
   ```

   Die Datei im Unterordner `app` ist die Hauptquelldatei, die den Code zum Überwachen von eingehenden Nachrichten von IoT Hub enthält. Die `blinkLED`-Funktion bewirkt das Blinken der LED.

   ![Repositorystruktur in der Beispielanwendung][repo-structure]
2. Führen Sie zum Initialisieren der Konfigurationsdatei die folgenden Befehle aus:

   ```bash
   npm install
   gulp init
   ```

   Wenn Sie die Schritte unter [Erstellen einer Azure-Funktionen-App und eines Speicherkontos][create-an-azure-function-app-and-storage-account] auf diesem Computer ausgeführt haben, werden alle Konfigurationen vererbt. Sie können daher diesen Schritt überspringen und mit dem Bereitstellen und Ausführen der Beispielanwendung fortfahren. Wenn Sie die Schritte unter [Erstellen einer Azure-Funktionen-App und eines Speicherkontos][create-an-azure-function-app-and-storage-account] auf einem anderen Computer ausgeführt haben, müssen Sie die Platzhalter in der Datei `config-edison.json` ersetzen. Die Datei `config-edison.json` befindet sich im Unterordner des Basisordners.

   ![Inhalt der Datei „config-edison.json“](media/iot-hub-intel-edison-lessons/lesson4/config-edison.png)

   * Ersetzen Sie **[device hostname or IP address]** durch die IP-Adresse des Geräts, die Sie bei der Konfiguration Ihres Geräts notiert haben.
   * Ersetzen Sie **[IoT device connection string]** durch die Geräteverbindungszeichenfolge, die Sie mit dem Befehl `az iot device show-connection-string --hub-name {my hub name} --device-id {device id}` abrufen.
   * Ersetzen Sie **[IoT hub connection string]** durch die IoT Hub-Verbindungszeichenfolge, die Sie mit dem Befehl `az iot hub show-connection-string --name {my hub name}` abrufen.

   > [!NOTE]
   > Führen Sie außerdem **gulp install-tools** aus, falls Sie dies in Lektion 1 noch nicht getan haben.

## <a name="deploy-and-run-the-sample-application"></a>Bereitstellen und Ausführen der Beispielanwendung
Führen Sie die folgenden Befehle aus, um die Beispielanwendung auf Edison bereitzustellen und auszuführen:

```bash
gulp deploy && gulp run
```

Mit dem Gulp-Befehl wird die Beispielanwendung auf Edison bereitgestellt. Anschließend werden die Anwendung auf Edison und ein gesonderter Task auf dem Hostcomputer ausgeführt, um von IoT Hub 20 Blink-Nachrichten an Edison zu senden.

Nachdem die Beispielanwendung ausgeführt wurde, beginnt sie mit dem Lauschen auf Nachrichten von IoT Hub. In der Zwischenzeit sendet der Gulp-Task mehrere Blink-Nachrichten von IoT Hub an Edison. Für jede Blink-Nachricht, die auf Edison eingeht, ruft die Beispielanwendung die Funktion `blinkLED` auf, um die LED blinken zu lassen.

Die LED sollte alle zwei Sekunden blinken, da der Gulp-Task 20 Nachrichten von IoT Hub an Edison sendet. Die letzte Nachricht ist eine Stoppnachricht, mit der die Anwendung beendet wird.

![Beispielanwendung mit gulp-Befehl und blink-Nachrichten][gulp-command-and-blink-messages]

## <a name="summary"></a>Zusammenfassung
Sie haben erfolgreich Nachrichten von IoT Hub an Edison gesendet, damit die LED blinkt. Die nächste Aufgabe ist optional: das Ändern des Ein- und Ausschaltverhaltens der LED.

## <a name="next-steps"></a>Nächste Schritte
[Ändern des Ein- und Aus-Verhaltens der LED][change-the-on-and-off-behavior-of-the-led]

<!-- Images and links -->

[troubleshooting]: iot-hub-intel-edison-kit-c-troubleshooting.md
[configure-your-device]: iot-hub-intel-edison-kit-c-lesson1-configure-your-device.md
[create-your-azure-iot-hub]: iot-hub-intel-edison-kit-c-lesson2-prepare-azure-iot-hub.md
[repo-structure]: media/iot-hub-intel-edison-lessons/lesson4/repo_structure_c.png
[create-an-azure-function-app-and-storage-account]: iot-hub-intel-edison-kit-c-lesson3-deploy-resource-manager-template.md
[gulp-command-and-blink-messages]: media/iot-hub-intel-edison-lessons/lesson4/gulp_blink_c.png
[change-the-on-and-off-behavior-of-the-led]: iot-hub-intel-edison-kit-c-lesson4-change-led-behavior.md
