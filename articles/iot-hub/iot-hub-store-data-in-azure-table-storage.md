---
title: Speichern Ihrer IoT Hub-Nachrichten im Azure-Datenspeicher | Microsoft-Dokumentation
description: "Verwenden einer Azure-Funktions-App zum Speichern Ihrer IoT Hub-Nachrichten in Ihrem Azure-Tabellenspeicher. IoT Hub-Nachrichten enthalten Informationen wie Sensordaten, die von Ihrem IoT-Gerät gesendet werden."
services: iot-hub
documentationcenter: 
author: shizn
manager: timlt
tags: 
keywords: IoT-Datenspeicher, IoT-Sensordatenspeicher
ms.assetid: 62fd14fd-aaaa-4b3d-8367-75c1111b6269
ms.service: iot-hub
ms.devlang: arduino
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/16/2017
ms.author: xshi
ms.translationtype: HT
ms.sourcegitcommit: 540180e7d6cd02dfa1f3cac8ccd343e965ded91b
ms.openlocfilehash: 06503f9564e00ef62587d02f2da4778974e246c5
ms.contentlocale: de-de
ms.lasthandoff: 08/16/2017

---
# <a name="save-iot-hub-messages-that-contain-sensor-data-to-your-azure-table-storage"></a>Speichern von IoT Hub-Nachrichten mit Sensordaten in Ihrem Azure-Tabellenspeicher

![Lückenloses Diagramm](media/iot-hub-get-started-e2e-diagram/3.png)

[!INCLUDE [iot-hub-get-started-note](../../includes/iot-hub-get-started-note.md)]

## <a name="what-you-learn"></a>Lerninhalt

Sie erfahren, wie Sie ein Azure-Speicherkonto und eine Azure-Funktions-App zum Speichern von IoT Hub-Nachrichten in Ihrem Tabellenspeicher erstellen.

## <a name="what-you-do"></a>Aufgaben

- Erstellen Sie ein Azure-Speicherkonto.
- Vorbereiten Ihrer IoT Hub-Verbindung auf das Lesen von Nachrichten.
- Erstellen und Bereitstellen einer Azure-Funktions-App.

## <a name="what-you-need"></a>Erforderliches Element

- [Richten Sie Ihr Gerät ein](iot-hub-raspberry-pi-kit-node-get-started.md), um folgende Anforderungen zu erfüllen:
  - Ein aktives Azure-Abonnement
  - Ein IoT Hub in Ihrem Abonnement 
  - Eine ausgeführte Anwendung, die Nachrichten an Ihren IoT Hub sendet

## <a name="create-an-azure-storage-account"></a>Erstellen eines Azure-Speicherkontos

1. Klicken Sie im [Azure-Portal](https://portal.azure.com/) auf **Neu** > **Speicher** > **Speicherkonto** > **Erstellen**.

2. Geben Sie die erforderlichen Informationen für das Speicherkonto ein:

   ![Erstellen eines Speicherkontos im Azure-Portal](media\iot-hub-store-data-in-azure-table-storage\1_azure-portal-create-storage-account.png)

   * **Name**: Der Name des Speicherkontos. Der Name muss global eindeutig sein.

   * **Ressourcengruppe**: Verwenden Sie dieselbe Ressourcengruppe wie für Ihren IoT Hub.

   * **An Dashboard anheften**: Aktivieren Sie diese Option für den leichteren Zugriff auf Ihren IoT Hub über das Dashboard.

3. Klicken Sie auf **Erstellen**.

## <a name="prepare-your-iot-hub-connection-to-read-messages"></a>Vorbereiten Ihrer IoT Hub-Verbindung auf das Lesen von Nachrichten

IoT Hub macht einen integrierten, mit Event Hub kompatiblen Endpunkt verfügbar, der Anwendungen das Lesen von IoT Hub-Nachrichten ermöglicht. In der Zwischenzeit verwenden Anwendungen Consumergruppen zum Lesen von Daten in Ihrem IoT Hub. Bevor Sie eine Azure-Funktions-App zum Lesen von Daten von Ihrem IoT Hub erstellen, tun Sie Folgendes:

- Abrufen der Verbindungszeichenfolge Ihres IoT Hub-Endpunkts
- Erstellen einer Consumergruppe für Ihren IoT Hub

### <a name="get-the-connection-string-of-your-iot-hub-endpoint"></a>Abrufen der Verbindungszeichenfolge Ihres IoT Hub-Endpunkts

1. Öffnen Sie Ihren IoT Hub.

2. Klicken Sie im Bereich **IoT Hub** unter **Messaging** auf **Endpunkte**.

3. Klicken Sie im rechten Bereich unter **Integrierte Endpunkte** auf **Ereignisse**.

4. Notieren Sie im Bereich **Eigenschaften** die folgenden Werte:
   - Event Hub-kompatibler Endpunkt
   - Event Hub-kompatibler Name

   ![Abrufen der Verbindungszeichenfolge Ihres IoT Hub-Endpunkts im Azure-Portal](media\iot-hub-store-data-in-azure-table-storage\2_azure-portal-iot-hub-endpoint-connection-string.png)

5. Klicken Sie im Bereich **IoT Hub** unter **Einstellungen** auf **Freigegebene Zugriffsrichtlinien**.

6. Klicken Sie auf **iothubowner**.

7. Notieren Sie den Wert **Primärschlüssel**.

8. Erstellen Sie die Verbindungszeichenfolge Ihres IoT Hub-Endpunkts wie folgt:

   `Endpoint=<Event Hub-compatible endpoint>;SharedAccessKeyName=iothubowner;SharedAccessKey=<Primary key>`

   > [!NOTE]
   > Ersetzen Sie `<Event Hub-compatible endpoint>` und `<Primary key>` durch die Werte, die Sie notiert haben.

### <a name="create-a-consumer-group-for-your-iot-hub"></a>Erstellen einer Consumergruppe für Ihren IoT Hub

1. Öffnen Sie Ihren IoT Hub.

2. Klicken Sie im Bereich **IoT Hub** unter **Messaging** auf **Endpunkte**.

3. Klicken Sie im rechten Bereich unter **Integrierte Endpunkte** auf **Ereignisse**.

4. Geben Sie im Bereich **Eigenschaften** unter **Consumergruppen** einen Namen ein, den Sie sich notieren.

5. Klicken Sie auf **Speichern**.

## <a name="create-and-deploy-an-azure-function-app"></a>Erstellen und Bereitstellen einer Azure-Funktions-App

1. Klicken Sie im [Azure-Portal](https://portal.azure.com/) auf **Neu** > **Berechnen** > **Funktionen-App** > **Erstellen**.

2. Geben Sie die erforderlichen Informationen für die Funktions-App ein.

   ![Erstellen einer Funktions-App im Azure-Portal](media\iot-hub-store-data-in-azure-table-storage\3_azure-portal-create-function-app.png)

   * **App-Name**: Der Name der Funktions-App. Der Name muss global eindeutig sein.

   * **Ressourcengruppe**: Verwenden Sie dieselbe Ressourcengruppe wie für Ihren IoT Hub.

   * **Speicherkonto**: Das Speicherkonto, das Sie erstellt haben.

   * **An Dashboard anheften**: Aktivieren Sie diese Option für den leichteren Zugriff auf die Funktions-App über das Dashboard.

3. Klicken Sie auf **Erstellen**.

4. Sobald die Funktions-App erstellt ist, öffnen Sie sie.

5. Erstellen Sie in der Funktions-App wie folgt eine neue Funktion:

   a. Klicken Sie auf **Neue Funktion**.

   b. Wählen Sie **JavaScript** für **Sprache** und **Datenverarbeitung** für **Szenario** aus.

   c. Klicken Sie auf **Diese Funktion erstellen** und dann auf **Neue Funktion**.

   d. Wählen Sie **JavaScript** als Sprache und **Datenverarbeitung** als Szenario aus.

   e. Klicken Sie auf die Vorlage **EventHubTrigger-JavaScript**.

   f. Geben Sie die erforderlichen Informationen für die Vorlage ein.

      * **Funktion benennen**: Der Name der Funktion.

      * **Event Hub-Name**: Der Event Hub-kompatible Name, den Sie vorher notiert haben.

      * **Event Hub-Verbindung**: Klicken Sie auf **Neu**, um die Verbindungszeichenfolge des IoT Hub-Endpunkts hinzuzufügen, den Sie erstellt haben.

   g. Klicken Sie auf **Erstellen**.

6. Konfigurieren Sie wie folgt eine Ausgabe der Funktion:

   a. Klicken Sie auf **Integrieren** > **Neue Ausgabe** > **Azure Table Storage** > **Auswählen**.

      ![Hinzufügen eines Tabellenspeichers zu Ihrer Funktions-App im Azure-Portal](media\iot-hub-store-data-in-azure-table-storage\4_azure-portal-function-app-add-output-table-storage.png)

   b. Geben Sie die erforderlichen Informationen ein.

      * **Tabellenparametername**: Wählen Sie `outputTable`, der im Code der Azure-Funktion verwendet wird.
      
      * **Tabellenname**: Verwenden Sie `deviceData`.

      * **Speicherkontoverbindung**: Klicken Sie auf **Neu**, wählen Sie Ihr Speicherkonto aus, oder geben Sie es ein. Wenn das Speicherkonto nicht angezeigt wird, siehe [Anforderungen an das Speicherkonto](https://docs.microsoft.com/azure/azure-functions/functions-create-function-app-portal#storage-account-requirements).
      
   c. Klicken Sie auf **Speichern**.

7. Klicken Sie unter **Trigger** auf **Azure Event Hub (eventHubMessages)**.

8. **Event Hub-Consumergruppe**: Geben Sie den Namen der Consumergruppe ein, die Sie zuvor erstellt haben, und klicken Sie auf **Speichern**.

9. Klicken Sie auf der linken Seite auf die von Ihnen erstellte Funktion und auf der rechten Seite dann auf **Dateien anzeigen**.

10. Ersetzen Sie den Code in `index.js` durch den folgenden Code:

   ```javascript
   'use strict';

   // This function is triggered each time a message is received in the IoT hub.
   // The message payload is persisted in an Azure storage table
 
   module.exports = function (context, iotHubMessage) {
    context.log('Message received: ' + JSON.stringify(iotHubMessage));
    var date = Date.now();
    var partitionKey = Math.floor(date / (24 * 60 * 60 * 1000)) + '';
    var rowKey = date + '';
    context.bindings.outputTable = {
     "partitionKey": partitionKey,
     "rowKey": rowKey,
     "message": JSON.stringify(iotHubMessage)
    };
    context.done();
   };
   ```

11. Klicken Sie auf **Speichern**.

Sie haben soeben die Funktions-App erstellt. In ihr werden Nachrichten gespeichert, die Ihr IoT Hub in Ihrem Tabellenspeicher empfängt.

> [!NOTE]
> Sie können auf die Schaltfläche **Ausführen** klicken, um die Funktions-App zu testen. Bei Klicken auf **Ausführen** wird die Testnachricht an Ihren IoT Hub gesendet. Der Eingang der Nachricht löst den Start der Funktions-App aus, die die Nachricht anschließend in Ihrem Tabellenspeicher speichert. Im Bereich **Protokolle** werden die Details des Prozesses aufgezeichnet.

## <a name="verify-your-message-in-your-table-storage"></a>Überprüfen der Nachricht in Ihrem Tabellenspeicher

1. Führen Sie die Beispielanwendung auf Ihrem Gerät aus, um Nachrichten an Ihren IoT Hub zu senden.

2. [Laden Sie den Azure Storage Explorer herunter, und installieren Sie ihn](http://storageexplorer.com/).

3. Öffnen Sie den Storage Explorer. Klicken Sie auf **Azure-Konto hinzufügen** > **Anmelden**, und melden Sie sich bei Ihrem Azure-Konto an.

4. Klicken Sie auf Ihr Azure-Abonnement > **Speicherkonten** > Ihr Speicherkonto > **Tabellen** > **deviceData**.

   Die Tabelle `deviceData` sollte protokollierte Nachrichten enthalten, die von Ihrem Gerät an Ihren IoT Hub gesendet wurden.

## <a name="next-steps"></a>Nächste Schritte

Sie haben Ihr Azure-Speicherkonto und die Azure-Funktions-App erfolgreich zum Speichern von Nachrichten in Ihrem Tabellenspeicher erstellt, die Ihr IoT Hub empfängt.

[!INCLUDE [iot-hub-get-started-next-steps](../../includes/iot-hub-get-started-next-steps.md)]

