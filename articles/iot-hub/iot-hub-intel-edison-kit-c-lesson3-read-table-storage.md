---
title: Lesen von Nachrichten in Azure Storage | Microsoft-Dokumentation
description: "Sie überwachen D2C-Nachrichten (Device-to-Cloud, Gerät-zu-Cloud), während diese in Azure Table Storage geschrieben werden."
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: Daten in der Cloud, Datensammlung in der Cloud, IoT-Clouddienst, IoT-Daten
ms.assetid: cad545c3-dd88-486c-a663-d587a924ccd4
ms.service: iot-hub
ms.devlang: c
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/8/2016
ms.author: xshi
translationtype: Human Translation
ms.sourcegitcommit: fc9090bd9d2dd621767c23f3cfb76dcc88b66a71
ms.openlocfilehash: c798669a111417cd4cf719a59a15221e734c7ff3


---
# <a name="read-messages-persisted-in-azure-storage"></a>Lesen von Nachrichten in Azure Storage
## <a name="what-you-will-do"></a>Aufgaben
Sie überwachen die D2C-Nachrichten, die vom Intel Edison an IoT Hub gesendet werden, während diese in Azure Table Storage geschrieben werden. Problemlösungen finden Sie auf der [Seite zur Problembehandlung][troubleshooting].

## <a name="what-you-will-learn"></a>Sie lernen Folgendes
In diesem Artikel erfahren Sie, wie Sie den Gulp-Task „read-message“ zum Lesen von Nachrichten verwenden, die in Azure Table Storage gespeichert sind.

## <a name="what-you-need"></a>Erforderliches Element
Bevor Sie diesen Prozess starten, müssen Sie den Abschnitt [Ausführen der Azure-Beispielanwendung blink auf einem Intel Edison][run-the-azure-blink-sample-application-on-intel-edison] erfolgreich abgeschlossen haben.

## <a name="read-new-messages-from-your-storage-account"></a>Lesen neuer Nachrichten aus dem Speicherkonto
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

## <a name="summary"></a>Zusammenfassung
Sie haben den Edison erfolgreich mit Ihrer IoT Hub-Instanz in der Cloud verbunden und die Beispielanwendung blink verwendet, um D2C-Nachrichten zu senden. Sie können die Azure-Funktionen-App auch dazu verwenden, eingehende IoT Hub-Nachrichten in Azure Table Storage zu speichern. Sie können nun C2D-Nachrichten von IoT Hub an den Edison senden.

## <a name="next-steps"></a>Nächste Schritte
[Ausführen einer Beispielanwendung, um C2D-Nachrichten zu empfangen][receive-cloud-to-device-messages]
<!-- Images and links -->

[troubleshooting]: iot-hub-intel-edison-kit-c-troubleshooting.md
[run-the-azure-blink-sample-application-on-intel-edison]: iot-hub-intel-edison-kit-c-lesson3-run-azure-blink.md
[gulp run]: media/iot-hub-intel-edison-lessons/lesson3/gulp_read_message_c.png
[receive-cloud-to-device-messages]: iot-hub-intel-edison-kit-c-lesson4-send-cloud-to-device-messages.md


<!--HONumber=Dec16_HO2-->


