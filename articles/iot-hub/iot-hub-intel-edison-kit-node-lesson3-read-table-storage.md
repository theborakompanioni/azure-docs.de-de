---
title: "Verbinden von Intel Edison (Node) mit Azure IoT – Lektion 3: Überwachen von Nachrichten | Microsoft-Dokumentation"
description: "Sie überwachen D2C-Nachrichten (Device-to-Cloud, Gerät-zu-Cloud), während diese in Azure Table Storage geschrieben werden."
services: iot-hub
documentationcenter: 
author: shizn
manager: timlt
tags: 
keywords: Daten in der Cloud, Datensammlung in der Cloud, IoT-Clouddienst, IoT-Daten
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-intel-edison-kit-node-get-started
ms.assetid: fa2c7efe-7e34-4e39-bb70-015c15ac69ed
ms.service: iot-hub
ms.devlang: nodejs
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.translationtype: Human Translation
ms.sourcegitcommit: adf5b10721a28432e6b37ef73c6a7e7ec9f93cdd
ms.openlocfilehash: 5100e6dbd74095f419c1017a3784fac547bf200b
ms.contentlocale: de-de
ms.lasthandoff: 01/25/2017

---
<a id="read-messages-persisted-in-azure-storage" class="xliff"></a>

# Lesen von Nachrichten in Azure Storage
<a id="what-you-will-do" class="xliff"></a>

## Aufgaben
Sie überwachen die D2C-Nachrichten, die vom Intel Edison an IoT Hub gesendet werden, während diese in Azure Table Storage geschrieben werden. Problemlösungen finden Sie auf der [Seite zur Problembehandlung][troubleshooting].

<a id="what-you-will-learn" class="xliff"></a>

## Sie lernen Folgendes
In diesem Artikel erfahren Sie, wie Sie den Gulp-Task „read-message“ zum Lesen von Nachrichten verwenden, die in Azure Table Storage gespeichert sind.

<a id="what-you-need" class="xliff"></a>

## Erforderliches Element
Bevor Sie diesen Prozess starten, müssen Sie den Abschnitt [Ausführen der Azure-Beispielanwendung blink auf einem Intel Edison][run-the-azure-blink-sample-application-on-intel-edison] erfolgreich abgeschlossen haben.

<a id="read-new-messages-from-your-storage-account" class="xliff"></a>

## Lesen neuer Nachrichten aus dem Speicherkonto
Im vorherigen Artikel haben Sie eine Beispielanwendung auf dem Edison ausgeführt. Die Beispielanwendung hat Nachrichten an Ihre Azure IoT Hub-Instanz gesendet. Die an Ihren IoT-Hub gesendeten Nachrichten wurden über die Azure-Funktionen-App in Azure Table Storage gespeichert. Sie benötigen die Azure Storage-Verbindungszeichenfolge für das Lesen von Nachrichten aus Azure Table Storage.

Gehen Sie folgendermaßen vor, um Nachrichten zu lesen, die in Azure Table Storage gespeichert sind:

1. Fragen Sie die Verbindungszeichenfolge ab, indem Sie die folgenden Befehle ausführen:

   ```bash
   az storage account list -g iot-sample --query [].name
   az storage account show-connection-string -g iot-sample -n {storage name}
   ```

   Der erste Befehl ruft den `storage name` ab, der im zweiten Befehl zum Abrufen der Verbindungszeichenfolge verwendet wird. Verwenden Sie `iot-sample` als Wert von `{resource group name}`, sofern Sie ihn nicht geändert haben.
2. Öffnen Sie die Konfigurationsdatei `config-edison.json` in Visual Studio Code, indem Sie den folgenden Befehl ausführen:

   ```bash
   # For Windows command prompt
   code %USERPROFILE%\.iot-hub-getting-started\config-edison.json

   # For MacOS or Ubuntu
   code ~/.iot-hub-getting-started/config-edison.json
   ```
3. Ersetzen Sie `[Azure storage connection string]` durch die Verbindungszeichenfolge, die Sie in Schritt 1 abgerufen haben.
4. Speichern Sie die Datei `config-edison.json`.
5. Senden Sie Nachrichten erneut, und lesen Sie diese aus Azure Table Storage, indem Sie den folgenden Befehl ausführen:

   ```bash
   gulp run --read-storage
   ```

   Die Logik für das Lesen in Azure Table Storage ist in der Datei `azure-table.js` enthalten.

   ![gulp run --read-storage][gulp run]

<a id="summary" class="xliff"></a>

## Zusammenfassung
Sie haben den Edison erfolgreich mit Ihrer IoT Hub-Instanz in der Cloud verbunden und die Beispielanwendung blink verwendet, um D2C-Nachrichten zu senden. Sie können die Azure-Funktionen-App auch dazu verwenden, eingehende IoT Hub-Nachrichten in Azure Table Storage zu speichern. Sie können nun C2D-Nachrichten von IoT Hub an den Edison senden.

<a id="next-steps" class="xliff"></a>

## Nächste Schritte
[Ausführen einer Beispielanwendung, um C2D-Nachrichten zu empfangen][receive-cloud-to-device-messages]
<!-- Images and links -->

[troubleshooting]: iot-hub-intel-edison-kit-node-troubleshooting.md
[run-the-azure-blink-sample-application-on-intel-edison]: iot-hub-intel-edison-kit-node-lesson3-run-azure-blink.md
[gulp run]: media/iot-hub-intel-edison-lessons/lesson3/gulp_read_message.png
[receive-cloud-to-device-messages]: iot-hub-intel-edison-kit-node-lesson4-send-cloud-to-device-messages.md
