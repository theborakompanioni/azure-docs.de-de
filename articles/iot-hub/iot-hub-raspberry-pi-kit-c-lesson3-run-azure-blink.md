---
title: "Verbinden von Raspberry Pi (C) mit Azure IoT – Lektion 3: Ausführen des Beispiels | Microsoft-Dokumentation"
description: "Stellen Sie eine Beispielanwendung auf Raspberry Pi 3 bereit, die Nachrichten an IoT Hub sendet und ein Blinken der LED auslöst, und führen Sie sie aus."
services: iot-hub
documentationcenter: 
author: shizn
manager: timlt
tags: 
keywords: Blinken LED Cloud Pi, LED blinken aus Cloud
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-raspberry-pi-kit-c-get-started
ms.assetid: e38df29f-f77f-435f-9add-46814297564f
ms.service: iot-hub
ms.devlang: c
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.translationtype: Human Translation
ms.sourcegitcommit: 64e69df256404e98f6175f77357500b562d74318
ms.openlocfilehash: 985ba895bfcad1611bebe80a6c2fc8ffed780da5
ms.contentlocale: de-de
ms.lasthandoff: 01/24/2017

---
<a id="run-a-sample-application-to-send-device-to-cloud-messages" class="xliff"></a>

# Ausführen einer Beispielanwendung zum Senden von D2C-Nachrichten
<a id="what-you-will-do" class="xliff"></a>

## Aufgaben
In diesem Artikel wird erläutert, wie Sie eine Beispielanwendung auf dem Pi 3 bereitstellen und ausführen, die Nachrichten an IoT Hub sendet. Problemlösungen finden Sie auf der [Seite zur Problembehandlung](iot-hub-raspberry-pi-kit-c-troubleshooting.md).

<a id="what-you-will-learn" class="xliff"></a>

## Sie lernen Folgendes
Sie erfahren, wie Sie das Gulp-Tool zum Bereitstellen und Ausführen der Node.js-Beispielanwendung auf Pi verwenden.

<a id="what-you-need" class="xliff"></a>

## Erforderliches Element
* Bevor Sie mit dieser Aufgabe beginnen, müssen Sie [Create an Azure function app and a storage account to process and store IoT hub messages](iot-hub-raspberry-pi-kit-c-lesson3-deploy-resource-manager-template.md) (Erstellen einer Azure-Funktionen-App und eines Speicherkontos zum Verarbeiten und Speichern von IoT Hub-Nachrichten) erfolgreich abgeschlossen haben.

<a id="get-your-iot-hub-and-device-connection-strings" class="xliff"></a>

## Abrufen der IoT Hub- und Geräteverbindungszeichenfolgen
Die Geräteverbindungszeichenfolge dient zum Verbinden des Pi mit IoT Hub. Die IoT Hub-Verbindungszeichenfolge wird verwendet, um die Identitätsregistrierung in Ihrem IoT Hub zum Verwalten der Geräte zu verwenden, die für das Herstellen der Verbindung mit Ihrem IoT Hub zulässig sind. 

* Listen Sie alle IoT Hubs der Ressourcengruppe auf, indem Sie den folgenden Azure-Befehlszeilenschnittstellen-Befehl ausführen:

```bash
az iot hub list -g iot-sample --query [].name
```

Verwenden Sie `iot-sample` als Wert von `{resource group name}`, sofern Sie ihn nicht geändert haben.

* Führen Sie zum Abrufen der IoT Hub-Verbindungszeichenfolge den folgenden Azure-CLI-Befehl aus:

```bash
az iot hub show-connection-string --name {my hub name} -g iot-sample
```

`{my hub name}` ist der Name, den Sie beim Erstellen Ihres IoT-Hubs und Registrieren von Pi angegeben haben.

* Führen Sie zum Abrufen der Geräteverbindungszeichenfolge den folgenden Befehl aus:

```bash
az iot device show-connection-string --hub-name {my hub name} --device-id myraspberrypi -g iot-sample
```

Verwenden Sie `myraspberrypi` als Wert von `{device id}`, sofern Sie ihn nicht geändert haben.

<a id="configure-the-device-connection" class="xliff"></a>

## Konfigurieren der Geräteverbindung
1. Führen Sie zum Initialisieren der Konfigurationsdatei die folgenden Befehle aus:
   
   ```bash
   npm install
   gulp init
   ```

> [!NOTE]
> Führen Sie außerdem **gulp install-tools** aus, falls Sie dies in Lektion 1 noch nicht getan haben.

2. Öffnen Sie die Gerätekonfigurationsdatei `config-raspberrypi.json` in Visual Studio Code, indem Sie den folgenden Befehl ausführen:
   
   ```bash
   # For Windows command prompt
   code %USERPROFILE%\.iot-hub-getting-started\config-raspberrypi.json
   
   # For MacOS or Ubuntu
   code ~/.iot-hub-getting-started/config-raspberrypi.json
   ```
   
   ![config.json](media/iot-hub-raspberry-pi-lessons/lesson3/config.png)
3. Nehmen Sie in der Datei `config-raspberrypi.json` die folgenden Änderungen vor:
   
   * Ersetzen Sie **[device hostname or IP address]** durch die IP-Adresse oder den Hostnamen des Geräts, den Sie mit `device-discovery-cli` abgerufen haben, oder durch den Wert, den Sie bei der Konfiguration Ihres Geräts übernommen haben.
   * Ersetzen Sie **[IoT device connection string]** durch den `device connection string`, den Sie abgerufen haben.
   * Ersetzen Sie **[IoT hub connection string]** durch den `iot hub connection string`, den Sie abgerufen haben.

> [!NOTE]
> `azure_storage_connection_string` benötigen Sie in diesem Artikel nicht. Nehmen Sie keine Änderungen vor.

Aktualisieren Sie die Datei `config-raspberrypi.json` so, dass Sie die Beispielanwendung auf Ihrem Computer bereitstellen können.

<a id="deploy-and-run-the-sample-application" class="xliff"></a>

## Bereitstellen und Ausführen der Beispielanwendung
Führen Sie den folgenden Befehl aus, um die Beispielanwendung auf dem Pi bereitzustellen und auszuführen:

```bash
gulp deploy && gulp run
```

<a id="verify-that-the-sample-application-works" class="xliff"></a>

## Überprüfen, ob die Beispielanwendung ordnungsgemäß ausgeführt wird
Die mit dem Pi verbundene LED sollte alle zwei Sekunden blinken. Bei jedem Blinken der LED sendet die Beispielanwendung eine Nachricht an IoT Hub und überprüft, ob die Nachricht erfolgreich an IoT Hub gesendet wurde. Darüber hinaus wird jede von IoT Hub empfangene Nachricht im Konsolenfenster ausgegeben. Die Beispielanwendung wird automatisch beendet, nachdem 20 Nachrichten gesendet wurden.

![Beispielanwendung mit gesendeten und empfangenen Nachrichten](media/iot-hub-raspberry-pi-lessons/lesson3/gulp_run_c.png)

<a id="summary" class="xliff"></a>

## Zusammenfassung
Sie haben die neue Beispielanwendung blink auf dem Pi bereitgestellt und ausgeführt, um D2C-Nachrichten an IoT Hub zu senden. Sie können nun Nachrichten überwachen, wenn sie in das Speicherkonto geschrieben werden.

<a id="next-steps" class="xliff"></a>

## Nächste Schritte
[Lesen von Nachrichten in Azure Storage](iot-hub-raspberry-pi-kit-c-lesson3-read-table-storage.md)


