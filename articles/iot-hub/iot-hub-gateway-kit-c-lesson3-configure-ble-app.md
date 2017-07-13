---
title: "SensorTag-Gerät und Azure IoT Gateway – Lektion 3: Ausführen der Beispiel-App | Microsoft-Dokumentation"
description: "Führen Sie eine BLE-Beispielanwendung aus, um Daten vom BLE SensorTag und von Ihrem IoT-Hub zu erhalten."
services: iot-hub
documentationcenter: 
author: shizn
manager: timlt
tags: 
keywords: "BLE-App, Sensorüberwachungs-App, Sensordatenerfassung, Daten von Sensoren, Sensordaten in die Cloud"
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-gateway-kit-c-lesson1-set-up-nuc
ms.assetid: b33e53a1-1df7-4412-ade1-45185aec5bef
ms.service: iot-hub
ms.devlang: c
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.translationtype: Human Translation
ms.sourcegitcommit: 61e9a9fc7876094c04238c61cfc38efdd97b05f7
ms.openlocfilehash: a9f689c0b231af3cdf9257e2179bf86ff7bc9a31
ms.contentlocale: de-de
ms.lasthandoff: 01/25/2017

---
<a id="configure-and-run-a-ble-sample-application" class="xliff"></a>

# Konfigurieren und Ausführen einer BLE-Beispielanwendung

<a id="what-you-will-do" class="xliff"></a>

## Aufgaben

- Klonen Sie das Beispielrepository. 
- Richten Sie die Verbindung zwischen SensorTag und Intel NUC ein. 
- Verwenden Sie die Azure-CLI zum Abrufen Ihrer IoT Hub- und SensorTag-Informationen für eine BLE-Beispielanwendung (Bluetooth Low Energy). Konfigurieren Sie die BLE-Beispielanwendung, und führen Sie sie aus. 

Problemlösungen finden Sie auf der [Seite zur Problembehandlung](iot-hub-gateway-kit-c-troubleshooting.md).

<a id="what-you-will-learn" class="xliff"></a>

## Sie lernen Folgendes

In diesem Artikel lernen Sie Folgendes:

- Konfigurieren und Ausführen der BLE-Beispielanwendung

<a id="what-you-need" class="xliff"></a>

## Erforderliches Element

Sie müssen Folgendes erfolgreich abgeschlossen haben:

- [Create your Azure IoT hub and register your device](iot-hub-gateway-kit-c-lesson2-register-device.md) (Erstellen einer IoT Hub-Instanz und Registrieren Ihres Geräts)

<a id="clone-the-sample-repository-to-the-host-computer" class="xliff"></a>

## Klonen des Beispielrepositorys auf dem Hostcomputer

Führen Sie diese Schritte auf dem Hostcomputer aus, um das Beispielrepository zu klonen:

1. Öffnen Sie in Windows ein Eingabeaufforderungsfenster, oder öffnen Sie ein Terminal in MacOS oder Ubuntu.
2. Führen Sie die folgenden Befehle aus:

   ```bash
   git clone https://github.com/Azure-samples/iot-hub-c-intel-nuc-gateway-getting-started
   cd iot-hub-c-intel-nuc-gateway-getting-started
   ```

<a id="set-up-the-connectivity-between-sensortag-and-intel-nuc" class="xliff"></a>

## Richten Sie die Verbindung zwischen SensorTag und Intel NUC ein.

Führen Sie diese Schritte auf dem Hostcomputer aus, um die Verbindung einzurichten:

1. Führen Sie zum Initialisieren der Konfigurationsdatei die folgenden Befehle aus:

   ```bash
   cd Lesson3
   npm install
   gulp init
   ```

2. Öffnen Sie `config-gateway.json` in Visual Studio Code, indem Sie den folgenden Befehl ausführen:

   ```bash
   # For Windows command prompt
   code %USERPROFILE%\.iot-hub-getting-started\config-gateway.json
   # For macOS or Ubuntu
   code ~/.iot-hub-getting-started/config-gateway.json
   ```

3. Suchen Sie nach der folgenden Codezeile, und ersetzen Sie `[device hostname or IP address]` durch die IP-Adresse oder den Hostnamen des Intel NUC.
   ![Screenshot der Gatewaykonfiguration](media/iot-hub-gateway-kit-lessons/lesson3/config_gateway.png)

4. Installieren Sie Hilfstools auf dem Intel NUC, indem Sie den folgenden Befehl ausführen:

   ```bash
   gulp install-tools
   ```

5. Aktivieren Sie SensorTag, indem Sie wie in der folgenden Abbildung gezeigt den Ein/Aus-Schalter betätigen. Danach sollte die grüne LED blinken.

   ![SensorTag einschalten](media/iot-hub-gateway-kit-lessons/lesson3/turn on_off sensortag.jpg)

6. Scannen Sie SensorTag-Geräte, indem Sie die folgenden Befehle ausführen:

   ```bash
   gulp discover-sensortag
   ```

7. Testen Sie die Verbindung zwischen SensorTag und Intel NUC, indem Sie den folgenden Befehl ausführen:

   ```bash
   gulp test-connectivity --mac {mac address}
   ```

   Ersetzen Sie `{mac address}` durch die MAC-Adresse, die Sie im vorherigen Schritt ermittelt haben.

<a id="get-the-connection-string-of-sensortag" class="xliff"></a>

## Abrufen der Verbindungszeichenfolge des SensorTag

Führen Sie den folgenden Befehl auf dem Hostcomputer aus, um die Azure IoT Hub-Verbindungszeichenfolge von SensorTag abzurufen:

```bash
az iot device show-connection-string --hub-name {IoT hub name} --device-id mydevice --resource-group iot-gateway
```

`{IoT hub name}` ist der IoT Hub-Name, den Sie verwendet haben. Verwenden Sie „iot-gateway“ als Wert von `{resource group name}` und „mydevice“ als Wert von `{device id}`, wenn Sie den Wert in Lektion 2 nicht geändert haben.

<a id="configure-the-ble-sample-application" class="xliff"></a>

## Konfigurieren der BLE-Beispielanwendung

Führen Sie diese Schritte auf dem Hostcomputer aus, um die BLE-Beispielanwendung zu konfigurieren und auszuführen:

1. Öffnen Sie `config-sensortag.json` in Visual Studio Code, indem Sie den folgenden Befehl ausführen:

   ```bash
   # For Windows command prompt
   code %USERPROFILE%\.iot-hub-getting-started\config-sensortag.json
   # For macOS or Ubuntu
   code ~/.iot-hub-getting-started/config-sensortag.json
   ```

   ![Screenshot der SensorTag-Konfiguration](media/iot-hub-gateway-kit-lessons/lesson3/config_sensortag.png)

2. Nehmen Sie am Code die folgenden Änderungen vor:
   - Ersetzen Sie `[IoT hub name]` durch den verwendeten IoT Hub-Namen.
   - Ersetzen Sie `[IoT device connection string]` durch die Verbindungszeichenfolge von SensorTag, die Sie ermittelt haben.
   - Ersetzen Sie `[device_mac_address]` durch die MAC-Adresse des SensorTag, die Sie ermittelt haben.

3. Führen Sie die BLE-Beispielanwendung aus.

   Führen Sie diese Schritte auf dem Hostcomputer aus, um die BLE-Beispielanwendung auszuführen:

   1. Schalten Sie SensorTag ein.

   2. Führen Sie den folgenden Befehl aus, um die BLE-Beispielanwendung auf dem Intel NUC-Gerät bereitzustellen und auszuführen:
   
      ```bash
      gulp run
      ```

<a id="verify-that-the-ble-sample-application-works" class="xliff"></a>

## Überprüfen, ob die BLE-Beispielanwendung richtig ausgeführt wird

Folgendes sollte angezeigt werden:

![Ausgabe der BLE-Beispielanwendung](media/iot-hub-gateway-kit-lessons/lesson3/BLE_running.png)

Die Beispielanwendung sammelt weiter Temperaturdaten und sendet sie an Ihren IoT Hub. Die Beispielanwendung wird nach 40 Sekunden automatisch beendet.

<a id="summary" class="xliff"></a>

## Zusammenfassung

Sie haben die Verbindung zwischen SensorTag und Intel NUC erfolgreich eingerichtet und eine BLE-Beispielanwendung ausgeführt, mit der Daten vom SensorTag-Gerät erfasst und an Ihren IoT Hub gesendet werden. Als Nächstes können Sie sich nun damit vertraut machen, wie Sie überprüfen, ob der IoT Hub die Daten empfangen hat.

<a id="next-steps" class="xliff"></a>

## Nächste Schritte
[Read messages from your IoT hub](iot-hub-gateway-kit-c-lesson3-read-messages-from-hub.md) (Lesen von Nachrichten von Ihrem IoT Hub)
