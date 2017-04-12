---
title: Speichern von IoT Hub-Nachrichten im Azure-Datenspeicher | Microsoft-Dokumentation
description: "Verwenden Sie eine Azure-Funktionen-App zum Speichern von IoT Hub-Nachrichten in Azure-Tabellenspeicher. IoT Hub-Nachrichten enthalten Informationen wie Sensordaten, die von Ihrem IoT-Gerät gesendet werden."
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: IoT-Datenspeicher, IoT-Sensordatenspeicher
ms.assetid: 62fd14fd-aaaa-4b3d-8367-75c1111b6269
ms.service: iot-hub
ms.devlang: arduino
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/27/2017
ms.author: xshi
translationtype: Human Translation
ms.sourcegitcommit: 785d3a8920d48e11e80048665e9866f16c514cf7
ms.openlocfilehash: ef5d4af5d5dad2b548ab01be5ba9c4769b21416a
ms.lasthandoff: 04/12/2017


---
# <a name="save-iot-hub-messages-that-contain-information-like-sensor-data-to-azure-table-storage"></a>Speichern von IoT Hub-Nachrichten mit Informationen wie Sensordaten in Azure-Tabellenspeicher

[!INCLUDE [iot-hub-get-started-note](../../includes/iot-hub-get-started-note.md)]

## <a name="what-you-will-learn"></a>Sie lernen Folgendes

Sie erfahren, wie Sie ein Azure-Speicherkonto und eine Azure-Funktionen-App zum Speichern von IoT Hub-Nachrichten in Azure-Tabellenspeicher erstellen.

## <a name="what-you-will-do"></a>Aufgaben

- Erstellen Sie ein Azure-Speicherkonto.
- Bereiten Sie die IoT Hub-Verbindung auf das Lesen von Nachrichten vor.
- Erstellen Sie eine Azure-Funktionen-App, und stellen Sie sie bereit.

## <a name="what-you-will-need"></a>Voraussetzungen

- Sie müssen das Tutorial [Verbinden von ESP8266 mit Azure IoT Hub](iot-hub-arduino-huzzah-esp8266-get-started.md) abgeschlossen haben, in dem die folgenden Voraussetzungen behandelt werden:
  - Ein aktives Azure-Abonnement.
  - Ein Azure IoT Hub in Ihrem Abonnement.
  - Eine ausgeführte Anwendung, die Nachrichten an Ihren Azure IoT Hub sendet.

## <a name="create-an-azure-storage-account"></a>Erstellen eines Azure-Speicherkontos

1. Klicken Sie im Azure-Portal auf **Neu** > **Speicher** > **Speicherkonto**.
1. Geben Sie die erforderlichen Informationen für das Speicherkonto ein:

   ![Erstellen eines Speicherkontos im Azure-Portal](media\iot-hub-store-data-in-azure-table-storage\1_azure-portal-create-storage-account.png)

   **Name**: Der Name des Speicherkontos. Der Name muss global eindeutig sein.

   **Ressourcengruppe**: Verwenden Sie dieselbe Ressourcengruppe wie für Ihren IoT Hub.

   **An Dashboard anheften**: Aktivieren Sie diese Option für den leichteren Zugriff auf Ihren IoT Hub über das Dashboard.
1. Klicken Sie auf **Erstellen**.

## <a name="prepare-for-iot-hub-connection-to-read-messages"></a>Vorbereiten der IoT Hub-Verbindung auf das Lesen von Nachrichten

IoT Hub macht einen integrierten mit Event Hub kompatiblen Endpunkt verfügbar, der einer Anwendung das Lesen von IoT Hub-Nachrichten ermöglicht. In der Zwischenzeit verwenden Anwendungen Consumergruppen zum Lesen von Daten im IoT Hub. Bevor Sie eine Azure-Funktionen-App zum Lesen von Daten in Ihrem IoT Hub erstellen, müssen Sie die folgenden Schritte ausführen:

- Abrufen der Verbindungszeichenfolge Ihres IoT Hub-Endpunkts
- Erstellen einer Consumergruppe für Ihren IoT Hub

### <a name="get-the-connection-string-of-your-iot-hub-endpoint"></a>Abrufen der Verbindungszeichenfolge Ihres IoT Hub-Endpunkts

1. Öffnen Sie Ihren IoT Hub.
1. Klicken Sie im Bereich **IoT Hub** unter **MESSAGING** auf **Endpunkte**.
1. Klicken Sie im rechten Bereich unter **Integrierte Endpunkte** auf **Ereignisse**.
1. Notieren Sie im Bereich **Eigenschaften** die folgenden Werte:
   - Event Hub-kompatibler Endpunkt
   - Event Hub-kompatibler Name

   ![Abrufen der Verbindungszeichenfolge Ihres IoT Hub-Endpunkts im Azure-Portal](media\iot-hub-store-data-in-azure-table-storage\2_azure-portal-iot-hub-endpoint-connection-string.png)

1. Klicken Sie im Bereich **IoT Hub** unter **EINSTELLUNGEN** auf **Freigegebene Zugriffsrichtlinien**.
1. Klicken Sie auf **iothubowner**.
1. Notieren Sie den Wert von **Primärschlüssel**.
1. Bilden Sie Verbindungszeichenfolge Ihres IoT Hub-Endpunkts wie folgt:

   `Endpoint=<Event Hub-compatible endpoint>;SharedAccessKeyName=iothubowner;SharedAccessKey=<Primary key>`

   > [!Note]
   > Ersetzen Sie `<Event Hub-compatible endpoint>` und `<Primary key>` durch die Werte, die Sie notiert haben.

### <a name="create-a-consumer-group-for-your-iot-hub"></a>Erstellen einer Consumergruppe für Ihren IoT Hub

1. Öffnen Sie Ihren IoT Hub.
1. Klicken Sie im Bereich **IoT Hub** unter **MESSAGING** auf **Endpunkte**.
1. Klicken Sie im rechten Bereich unter **Integrierte Endpunkte** auf **Ereignisse**.
1. Geben Sie im Bereich **Eigenschaften** unter **Consumergruppen** einen Namen ein, den Sie sich notieren.
1. Klicken Sie auf **Speichern**.

## <a name="create-and-deploy-an-azure-function-app"></a>Erstellen und Bereitstellen einer Azure-Funktionen-App

1. Klicken Sie im [Azure-Portal](https://portal.azure.com/)auf **Neu** > **Berechnen** > **Funktionen-App**.
1. Geben Sie die erforderlichen Informationen für die Funktionen-App ein.

   ![Erstellen einer Funktionen-App im Azure-Portal](media\iot-hub-store-data-in-azure-table-storage\3_azure-portal-create-function-app.png)

   **App-Name**: Der Name der Funktionen-App. Der Name muss global eindeutig sein.

   **Ressourcengruppe**: Verwenden Sie dieselbe Ressourcengruppe wie für Ihren IoT Hub.

   **Speicherkonto**: Das Speicherkonto, das Sie erstellt haben.

   **An Dashboard anheften**: Aktivieren Sie diese Option für den leichteren Zugriff auf die Funktionen-App über das Dashboard.
1. Klicken Sie auf **Erstellen**.
1. Öffnen Sie die erstellte Funktionen-App.
1. Erstellen Sie in der Funktionen-App eine neue Funktion.
   1. Klicken Sie auf **Neue Funktion**.
   1. Wählen Sie **JavaScript** für **Sprache** und **Datenverarbeitung** für **Szenario** aus.
   1. Klicken Sie auf die Vorlage **EventHubTrigger-JavaScript**.
   1. Geben Sie die erforderlichen Informationen für die Vorlage ein.

      **Funktion benennen**: Der Name der Funktion.

      **Event Hub-Name**: Der Event Hub-kompatible Name, den Sie notiert haben.

      **Event Hub-Verbindung**: Klicken Sie auf „Neu“, um die Verbindungszeichenfolge Ihren IoT Hub-Endpunkts hinzuzufügen, den Sie erstellt haben.
   1. Klicken Sie auf **Erstellen**.
1. Konfigurieren Sie eine Ausgabe der Funktion.
   1. Klicken Sie auf **Integrieren** > **Neue Ausgabe** > **Azure Table Storage** > **Auswählen**.

      ![Hinzufügen eines Tabellenspeichers zu Ihrer Funktionen-App im Azure-Portal](media\iot-hub-store-data-in-azure-table-storage\4_azure-portal-function-app-add-output-table-storage.png)
   1. Geben Sie die erforderlichen Informationen ein.

      **Tabellenname**: Verwenden Sie `deviceData` als Namen.

      **Speicherkontoverbindung**: Klicken Sie auf **Neu**, und wählen Sie Ihr Speicherkonto aus.
   1. Klicken Sie auf **Speichern**.
1. Klicken Sie unter **Trigger** auf **Azure Event Hub (myEventHubTrigger)**.
1. **Event Hub-Consumergruppe**: Geben Sie den Namen der Consumergruppe ein, die Sie zuvor erstellt haben, und klicken Sie auf **Speichern**.
1. Klicken Sie auf **Entwickeln** und dann auf **Dateien anzeigen**.
1. Klicken Sie auf **Hinzufügen**, um die neue Datei `package.json` hinzuzufügen. Fügen Sie die folgenden Informationen ein, und klicken Sie dann auf **Speichern**.

   ```json
   {
      "name": "iothub_save_message_to_table",
      "version": "0.0.1",
      "private": true,
      "main": "index.js",
      "author": "Microsoft Corp.",
      "dependencies": {
         "azure-iothub": "1.0.9",
         "azure-iot-common": "1.0.7",
         "moment": "2.14.1"
      }
   }
   ```
1. Ersetzen Sie den Code in `index.js` durch den folgenden Code. Klicken Sie dann auf **Speichern**.

   ```javascript
   'use strict';

   // This function is triggered each time a message is revieved in the IoTHub.
   // The message payload is persisted in an Azure Storage Table
   var moment = require('moment');

   module.exports = function (context, iotHubMessage) {
      context.log('Message received: ' + JSON.stringify(iotHubMessage));
      context.bindings.outputTable = {
      "partitionKey": moment.utc().format('YYYYMMDD'),
         "rowKey": moment.utc().format('hhmmss') + process.hrtime()[1] + '',
         "message": JSON.stringify(iotHubMessage)
      };
      context.done();
   };
   ```
1. Klicken Sie auf **Funktionen-App-Einstellungen** > **Entwicklerkonsole öffnen**.

   Sie sollten sich im Ordner `wwwroot` der Funktionen-App befinden.
1. Wechseln Sie zum Ordner der Funktion, indem Sie den folgenden Befehl ausführen:

   ```bash
   cd <your function name>
   ```
1. Installieren Sie das npm-Paket, indem Sie den folgenden Befehl ausführen:

   ```bash
   npm install
   ```

   > [!Note]
   > Die Installation kann einige Zeit in Anspruch nehmen.

Sie haben soeben die Funktionen-App erstellt. In ihr werden Nachrichten gespeichert, die Ihr IoT Hub in Ihrem Azure-Tabellenspeicher empfängt.

> [!Note]
> Sie können auf die Schaltfläche **Ausführen** klicken, um die Funktionen-App zu testen. Bei Klicken auf **Ausführen** wird die Testnachricht an Ihren IoT Hub gesendet. Der Eingang der Nachricht löst den Start der Funktionen-App aus, die die Nachricht anschließend in Ihrem Azure-Tabellenspeicher speichert. Im Bereich **Protokolle** werden die Details des Prozesses aufgezeichnet.

## <a name="verify-your-message-in-your-table-storage"></a>Überprüfen der Nachricht in Ihrem Tabellenspeicher

1. Führen Sie die Beispielanwendung auf Ihrem Gerät aus, um Nachrichten an Ihren IoT Hub zu senden.
1. [Laden Sie den Microsoft Azure Storage Explorer herunter, und installieren Sie ihn](http://storageexplorer.com/).
1. Öffnen Sie den Microsoft Azure Storage Explorer. Klicken Sie auf **Azure-Konto hinzufügen** > **Anmelden**, und melden Sie sich bei Ihrem Azure-Konto an.
1. Klicken Sie auf Ihr Azure-Abonnement > **Speicherkonten** > Ihr Speicherkonto > **Tabellen** > **deviceData**.

   Die Tabelle `deviceData` sollte protokollierte Nachrichten enthalten, die von Ihrem Gerät an Ihren IoT Hub gesendet wurden.

## <a name="next-steps"></a>Nächste Schritte

Sie haben Ihr Azure-Speicherkonto und die Azure-Funktionen-App erfolgreich zum Speichern von Nachrichten in Ihrem Azure-Tabellenspeicher erstellt, die Ihr IoT Hub empfängt.

[!INCLUDE [iot-hub-get-started-next-steps](../../includes/iot-hub-get-started-next-steps.md)]
