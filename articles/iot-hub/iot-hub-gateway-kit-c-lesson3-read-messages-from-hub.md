---
title: "SensorTag-Gerät und Azure IoT Gateway – Lektion 3: Lesen von Nachrichten | Microsoft-Dokumentation"
description: "Führen Sie Beispielcode auf Ihrem Hostcomputer aus, um die Nachrichten vom IoT Hub zu lesen."
services: iot-hub
documentationcenter: 
author: shizn
manager: timlt
tags: 
keywords: Daten in der Cloud, Datensammlung in der Cloud, IoT-Clouddienst, IoT-Daten
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-gateway-kit-c-lesson1-set-up-nuc
ms.assetid: cc88be24-b5c0-4ef2-ba21-4e8f77f3e167
ms.service: iot-hub
ms.devlang: c
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.translationtype: Human Translation
ms.sourcegitcommit: 61e9a9fc7876094c04238c61cfc38efdd97b05f7
ms.openlocfilehash: 0a5dd7e010ba46b724bb6a2526dc2a2730acc10f
ms.contentlocale: de-de
ms.lasthandoff: 01/25/2017

---

<a id="read-messages-from-your-iot-hub" class="xliff"></a>

# Lesen von Nachrichten von Ihrem IoT Hub

<a id="what-you-will-do" class="xliff"></a>

## Aufgaben

- Führen Sie Beispielcode auf Ihrem Hostcomputer aus, um Nachrichten vom IoT Hub zu lesen.

Problemlösungen finden Sie auf der [Seite zur Problembehandlung](iot-hub-gateway-kit-c-troubleshooting.md).

<a id="what-you-will-learn" class="xliff"></a>

## Sie lernen Folgendes

Verwenden des Gulp-Tools zum Lesen von Nachrichten vom IoT Hub

<a id="what-you-need" class="xliff"></a>

## Erforderliches Element

- Die BLE-Beispielanwendung, die Sie in Lektion 3 erfolgreich ausgeführt haben.

<a id="get-your-iot-hub-and-device-connection-strings" class="xliff"></a>

## Abrufen der IoT Hub- und Geräteverbindungszeichenfolgen

Die Geräteverbindungszeichenfolge wird von Ihrem Gerät (TI SensorTag oder simuliertes Gerät) zum Herstellen der Verbindung mit Ihrem IoT Hub verwendet. Die IoT Hub-Verbindungszeichenfolge wird verwendet, um die Identitätsregistrierung in Ihrem IoT Hub zum Verwalten der Geräte zu verwenden, die für das Herstellen der Verbindung mit Ihrem IoT Hub zulässig sind.

- Listen Sie alle IoT Hubs der Ressourcengruppe auf, indem Sie den folgenden Befehl verwenden:

   ```bash
   az iot hub list -g iot-gateway --query [].name
   ```

   Verwenden Sie `iot-gateway` als Wert von `{resource group name}`, sofern Sie ihn nicht geändert haben.
- Führen Sie zum Abrufen der IoT Hub-Verbindungszeichenfolge den folgenden Befehl aus:

   ```bash
   az iot hub show-connection-string --name {my hub name} -g iot-gateway
   ```

   `{my hub name}` ist der Name, den Sie in Lektion 2 angegeben haben.

<a id="configure-the-device-connection-for-the-sample-code" class="xliff"></a>

## Konfigurieren der Geräteverbindung für den Beispielcode

Aktualisieren Sie die Gerätekonfigurationsdatei `config-azure.json`, damit Sie Nachrichten vom IoT Hub auf Ihrem Hostcomputer lesen können. Gehen Sie dazu folgendermaßen vor:

1. Öffnen Sie `config-azure.json` in Visual Studio Code, indem Sie den folgenden Befehl in einem Konsolenfenster ausführen:

   ```bash
   # For Windows command prompt
   code %USERPROFILE%\.iot-hub-getting-started\config-azure.json
   # For MacOS or Ubuntu
   code ~/.iot-hub-getting-started/config-azure.json
   ```

2. Nehmen Sie in der Datei `config-azure.json` die folgenden Änderungen vor:

   ![Screenshot der Azure-Konfiguration](media/iot-hub-gateway-kit-lessons/lesson3/config_azure.png)

   Ersetzen Sie `[IoT hub connection string]` durch die IoT Hub-Verbindungszeichenfolge, die Sie abgerufen haben.

<a id="read-messages-from-your-iot-hub" class="xliff"></a>

## Lesen von Nachrichten von Ihrem IoT Hub

Stellen Sie bei Verwendung eines TI SensorTag sicher, dass Sie das SensorTag-Gerät bereits eingeschaltet haben. Führen Sie die Gatewaybeispielanwendung aus, und lesen Sie die IoT Hub-Nachrichten mit dem folgenden Befehl:

```bash
gulp run --iot-hub
```

Mit dem Befehl wird die BLE-Beispielanwendung ausgeführt, mit der Temperaturdaten von Ihrem SensorTag oder simulierten Gerät verpackt werden und die Nachricht alle zwei Sekunden an Ihren IoT Hub gesendet wird. Außerdem wird ein untergeordneter Prozess zum Empfangen der Nachricht erzeugt.

Alle Nachrichten, die gesendet und empfangen werden, werden auf dem Hostcomputer sofort in demselben Konsolenfenster angezeigt. Die Instanz der Beispielanwendung endet automatisch nach 40 Sekunden.

![BLE-Beispielanwendung mit gesendeten und empfangenen Nachrichten](media/iot-hub-gateway-kit-lessons/lesson3/gulp_run_read_hub.png)

<a id="summary" class="xliff"></a>

## Zusammenfassung

Sie haben einen Beispielcode ausgeführt, um Nachrichten von Ihrem IoT Hub zu lesen. Sie können jetzt die Nachrichten lesen, die in Ihrem Azure-Tabellenspeicher gespeichert sind.

<a id="next-steps" class="xliff"></a>

## Nächste Schritte
[Erstellen einer Azure-Funktionen-App und eines Azure Storage-Kontos](iot-hub-gateway-kit-c-lesson4-deploy-resource-manager-template.md)



