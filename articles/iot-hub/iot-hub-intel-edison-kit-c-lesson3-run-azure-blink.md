---
title: "Ausführen einer Beispielanwendung zum Senden von D2C-Nachrichten an Azure IoT Hub | Microsoft Docs"
description: "Stellen Sie eine Beispielanwendung auf dem Intel Edison bereit, die Nachrichten an IoT Hub sendet und ein Blinken der LED auslöst, und führen Sie sie aus."
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: IoT-Clouddienst, Arduino Daten an Cloud senden
ms.assetid: 12672b64-795a-4dfc-86fd-df53ed3eeef7
ms.service: iot-hub
ms.devlang: c
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/8/2016
ms.author: xshi
translationtype: Human Translation
ms.sourcegitcommit: cb18052f74528e245264bb2f400b625fe550ed85
ms.openlocfilehash: 59db4ca0afcfd2757a725f5d56c0810f6547c924


---
# <a name="run-a-sample-application-to-send-device-to-cloud-messages"></a>Ausführen einer Beispielanwendung zum Senden von D2C-Nachrichten
## <a name="what-you-will-do"></a>Aufgaben
In diesem Artikel wird erläutert, wie Sie eine Beispielanwendung auf dem Intel Edison bereitstellen und ausführen, die Nachrichten an IoT Hub sendet. Problemlösungen finden Sie auf der [Seite zur Problembehandlung][troubleshooting].

## <a name="what-you-will-learn"></a>Sie lernen Folgendes
Sie erfahren, wie Sie das Gulp-Tool zum Bereitstellen und Ausführen der C-Beispielanwendung auf Edison verwenden.

## <a name="what-you-need"></a>Erforderliches Element
* Bevor Sie mit dieser Aufgabe beginnen, müssen Sie [Create an Azure function app and a storage account to process and store IoT hub messages][process-and-store-iot-hub-messages] (Erstellen einer Azure-Funktionen-App und eines Azure-Speicherkontos zum Verarbeiten und Speichern von IoT Hub-Nachrichten) erfolgreich abgeschlossen haben.

## <a name="get-your-iot-hub-and-device-connection-strings"></a>Abrufen der IoT Hub- und Geräteverbindungszeichenfolgen
Die Geräteverbindungszeichenfolge dient zum Verbinden des Edison mit IoT Hub. Die IoT Hub-Verbindungszeichenfolge dient zum Verbinden von IoT Hub mit der Geräteidentität, die der Edison in IoT Hub darstellt.

* Listen Sie alle IoT Hubs der Ressourcengruppe auf, indem Sie den folgenden Azure-Befehlszeilenschnittstellen-Befehl ausführen:

```bash
az iot hub list -g iot-sample --query [].name
```

Verwenden Sie `iot-sample` als Wert von `{resource group name}`, sofern Sie ihn nicht geändert haben.

* Führen Sie zum Abrufen der IoT Hub-Verbindungszeichenfolge den folgenden Azure-CLI-Befehl aus:

```bash
az iot hub show-connection-string --name {my hub name}
```

`{my hub name}` ist der Name, den Sie beim Erstellen Ihres IoT Hubs und Registrieren des Edison angegeben haben.

* Führen Sie zum Abrufen der Geräteverbindungszeichenfolge den folgenden Befehl aus:

```bash
az iot device show-connection-string --hub-name {my hub name} --device-id myinteledison
```

Verwenden Sie `myinteledison` als Wert von `{device id}`, sofern Sie ihn nicht geändert haben.

## <a name="configure-the-device-connection"></a>Konfigurieren der Geräteverbindung
1. Führen Sie zum Initialisieren der Konfigurationsdatei die folgenden Befehle aus:

   ```bash
   npm install
   gulp init
   ```
   > [!NOTE]
   > Führen Sie außerdem **gulp install-tools** aus, falls Sie dies in Lektion 1 noch nicht getan haben.

2. Öffnen Sie die Gerätekonfigurationsdatei `config-edison.json` in Visual Studio Code, indem Sie den folgenden Befehl ausführen:

   ```bash
   # For Windows command prompt
   code %USERPROFILE%\.iot-hub-getting-started\config-edison.json

   # For MacOS or Ubuntu
   code ~/.iot-hub-getting-started/config-edison.json
   ```

   ![config.json](media/iot-hub-intel-edison-lessons/lesson3/config.png)

3. Nehmen Sie in der Datei `config-edison.json` die folgenden Änderungen vor:

   * Ersetzen Sie **[device hostname or IP address]** durch die IP-Adresse des Geräts, die Sie bei der Konfiguration Ihres Geräts notiert haben.
   * Ersetzen Sie **[IoT device connection string]** durch den `device connection string`, den Sie abgerufen haben.
   * Ersetzen Sie **[IoT hub connection string]** durch den `iot hub connection string`, den Sie abgerufen haben.

   > [!NOTE]
   > `azure_storage_connection_string` benötigen Sie in diesem Artikel nicht. Nehmen Sie keine Änderungen vor.

## <a name="deploy-and-run-the-sample-application"></a>Bereitstellen und Ausführen der Beispielanwendung
Führen Sie den folgenden Befehl aus, um die Beispielanwendung auf dem Edison bereitzustellen und auszuführen:

```bash
gulp deploy && gulp run
```

## <a name="verify-that-the-sample-application-works"></a>Überprüfen, ob die Beispielanwendung ordnungsgemäß ausgeführt wird
Die mit dem Edison verbundene LED sollte alle zwei Sekunden blinken. Bei jedem Blinken der LED sendet die Beispielanwendung eine Nachricht an IoT Hub und überprüft, ob die Nachricht erfolgreich an IoT Hub gesendet wurde. Darüber hinaus wird jede von IoT Hub empfangene Nachricht im Konsolenfenster ausgegeben. Die Beispielanwendung wird automatisch beendet, nachdem 20 Nachrichten gesendet wurden.

![Beispielanwendung mit gesendeten und empfangenen Nachrichten][sample-application-with-sent-and-received-messages]

## <a name="summary"></a>Zusammenfassung
Sie haben die neue Blinkbeispielanwendung auf dem Edison bereitgestellt und ausgeführt, um D2C-Nachrichten an IoT Hub zu senden. Sie können nun Nachrichten überwachen, wenn sie in das Speicherkonto geschrieben werden.

## <a name="next-steps"></a>Nächste Schritte
[Lesen von Nachrichten in Azure Storage][read-messages-persisted-in-azure-storage]
<!-- Images and links -->

[troubleshooting]: iot-hub-intel-edison-kit-c-troubleshooting.md
[process-and-store-iot-hub-messages]: iot-hub-intel-edison-kit-c-lesson3-deploy-resource-manager-template.md
[sample-application-with-sent-and-received-messages]: media/iot-hub-intel-edison-lessons/lesson3/gulp_run_c.png
[read-messages-persisted-in-azure-storage]: iot-hub-intel-edison-kit-c-lesson3-read-table-storage.md


<!--HONumber=Dec16_HO2-->


