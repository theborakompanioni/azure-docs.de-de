---
title: "Simuliertes Gerät und Azure IoT Gateway – Lektion 3: Ausführen der Beispiel-App | Microsoft-Dokumentation"
description: "Ausführen einer Beispiel-App für ein simuliertes Gerät zum Senden von Temperaturdaten an Ihren IoT Hub"
services: iot-hub
documentationcenter: 
author: shizn
manager: timlt
tags: 
keywords: Daten in die Cloud
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-gateway-kit-c-lesson1-set-up-nuc
ms.assetid: 5d051d99-9749-4150-b3c8-573b0bda9c52
ms.service: iot-hub
ms.devlang: c
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.translationtype: Human Translation
ms.sourcegitcommit: 61e9a9fc7876094c04238c61cfc38efdd97b05f7
ms.openlocfilehash: 873f3cef8cb1d115f77f0d3fa2c4b50391f66d91
ms.contentlocale: de-de
ms.lasthandoff: 01/25/2017

---
<a id="configure-and-run-a-simulated-device-sample-app" class="xliff"></a>

# Konfigurieren und Ausführen einer Beispiel-App für ein simuliertes Gerät

<a id="what-you-will-do" class="xliff"></a>

## Aufgaben

- Klonen Sie das Beispielrepository.
- Verwenden Sie die Azure-CLI, um Ihre IoT Hub-Informationen und die Informationen zum logischen Gerät für die Beispiel-App für ein simuliertes Gerät abzurufen. Konfigurieren Sie die Beispielanwendung für das simulierte Gerät, und führen Sie sie aus.

Problemlösungen finden Sie auf der [Seite zur Problembehandlung](iot-hub-gateway-kit-c-sim-troubleshooting.md).

<a id="what-you-will-learn" class="xliff"></a>

## Sie lernen Folgendes

In diesem Artikel lernen Sie Folgendes:

- Konfigurieren und Ausführen der Beispielanwendung für das simulierte Gerät

<a id="what-you-need" class="xliff"></a>

## Erforderliches Element

Sie müssen Folgendes erfolgreich abgeschlossen haben:

- [Create an IoT hub and register your device](iot-hub-gateway-kit-c-sim-lesson2-register-device.md) (Erstellen eines IoT Hub und Registrieren Ihres Geräts)

<a id="clone-the-sample-repository-to-the-host-computer" class="xliff"></a>

## Klonen des Beispielrepositorys auf dem Hostcomputer

Führen Sie diese Schritte auf dem Hostcomputer aus, um das Beispielrepository zu klonen:

1. Öffnen Sie in Windows eine Eingabeaufforderung, oder öffnen Sie ein Terminal in MacOS oder Ubuntu.
2. Führen Sie die folgenden Befehle aus:

   ```bash
   git clone https://github.com/Azure-samples/iot-hub-c-intel-nuc-gateway-getting-started
   cd iot-hub-c-intel-nuc-gateway-getting-started
   ```

<a id="configure-the-simulated-device-and-your-nuc" class="xliff"></a>

## Konfigurieren des simulierten Geräts und Ihres NUC

1. Öffnen Sie die Konfigurationsdatei `config.json` in Visual Studio Code, indem Sie den folgenden Befehl ausführen:

   ```bash
   code config.json
   ```

2. Ersetzen Sie `"has_sensortag": true` durch `"has_sensortag": false`.

   ![Konfiguration bei Nichtvorhandensein eines TI SensorTag-Geräts](media/iot-hub-gateway-kit-lessons/lesson3/config_no_sensortag.png)

3. Führen Sie zum Initialisieren der Konfigurationsdatei die folgenden Befehle aus:

   ```bash
   cd Lesson3
   npm install
   gulp init
   ```

4. Öffnen Sie `config-gateway.json` in Visual Studio Code, indem Sie den folgenden Befehl ausführen:

   ```bash
   # For Windows command prompt
   code %USERPROFILE%\.iot-hub-getting-started\config-gateway.json
   # For macOS or Ubuntu
   code ~/.iot-hub-getting-started/config-gateway.json
   ```

5. Suchen Sie nach der folgenden Codezeile, und ersetzen Sie `[device hostname or IP address]` durch die IP-Adresse oder den Hostnamen des Intel NUC.
   ![Screenshot der Gatewaykonfiguration](media/iot-hub-gateway-kit-lessons/lesson3/config_gateway.png)

<a id="get-the-connection-string-of-your-iot-hub-logical-device" class="xliff"></a>

## Abrufen der Verbindungszeichenfolge des logischen IoT Hub-Geräts

Führen Sie den folgenden Befehl auf dem Hostcomputer aus, um die Azure IoT Hub-Verbindungszeichenfolge des logischen Geräts abzurufen:

```bash
az iot device show-connection-string --hub-name {IoT hub name} --device-id mydevice --resource-group iot-gateway
```

`{IoT hub name}` ist der IoT Hub-Name, den Sie verwendet haben. Verwenden Sie „iot-gateway“ als Wert von `{resource group name}` und „mydevice“ als Wert von `{device id}`, wenn Sie den Wert in Lektion 2 nicht geändert haben.

<a id="configure-the-simulated-device-cloud-upload-sample-application" class="xliff"></a>

## Konfigurieren der Cloudupload-Beispielanwendung für das simulierte Gerät

Führen Sie diese Schritte auf dem Hostcomputer aus, um die Cloudupload-Beispielanwendung für das simulierte Gerät zu konfigurieren und auszuführen:

1. Öffnen Sie `config-sensortag.json` in Visual Studio Code, indem Sie den folgenden Befehl ausführen:

   ```bash
   # For Windows command prompt
   code %USERPROFILE%\.iot-hub-getting-started\config-sensortag.json
   # For macOS or Ubuntu
   code ~/.iot-hub-getting-started/config-sensortag.json
   ```

   ![Screenshot der SensorTag-Konfiguration](media/iot-hub-gateway-kit-lessons/lesson3/config_simulated_device.png)

2. Nehmen Sie am Code die folgenden Änderungen vor:
   - Ersetzen Sie `[IoT hub name]` durch den IoT Hub-Namen.
   - Ersetzen Sie `[IoT device connection string]` durch die Verbindungszeichenfolge Ihres logischen IoT Hub-Geräts.

3. Führen Sie die Anwendung aus.

   Führen Sie den folgenden Befehl aus, um die Anwendung bereitzustellen und auszuführen:

   ```bash
   gulp run
   ```

<a id="verify-the-sample-application-works" class="xliff"></a>

## Überprüfen, ob die Beispielanwendung richtig ausgeführt wird

Die Ausgabe sollte in etwa wie folgt aussehen:

![Ausgabe der Beispielanwendung für das simulierte Gerät](media/iot-hub-gateway-kit-lessons/lesson3/gulp_run_simudev.png)

Die Anwendung sendet Temperaturdaten an Ihren IoT Hub. Der Vorgang ist 40 Sekunden lang aktiv.

<a id="summary" class="xliff"></a>

## Zusammenfassung

Sie haben die Cloudupload-Beispielanwendung, mit der Daten an Ihren IoT Hub mit dem simulierten Gerät gesendet werden, erfolgreich konfiguriert und ausgeführt.

<a id="next-steps" class="xliff"></a>

## Nächste Schritte
[Read messages from your IoT hub](iot-hub-gateway-kit-c-sim-lesson3-read-messages-from-hub.md) (Lesen von Nachrichten von Ihrem IoT Hub)
