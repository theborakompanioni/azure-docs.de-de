---
title: "Azure IoT Hub-Vorgangsüberwachung | Microsoft Docs"
description: "Verwenden der Azure IoT Hub-Vorgangsüberwachung zum Überwachen des Status von Vorgängen auf Ihrem IoT Hub in Echtzeit."
services: iot-hub
documentationcenter: 
author: nberdy
manager: timlt
editor: 
ms.assetid: a299f3a5-b14d-4586-9c3b-44aea14ed013
ms.service: iot-hub
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/24/2017
ms.author: nberdy
ms.translationtype: Human Translation
ms.sourcegitcommit: e7da3c6d4cfad588e8cc6850143112989ff3e481
ms.openlocfilehash: 22ca1b635f3c5d4d349e883d9953b0c126fc715a
ms.contentlocale: de-de
ms.lasthandoff: 05/16/2017


---
# <a name="iot-hub-operations-monitoring"></a>IoT Hub-Vorgangsüberwachung

Die IoT Hub-Vorgangsüberwachung ermöglicht Ihnen das Überwachen des Status von Vorgängen auf Ihrem IoT Hub in Echtzeit. IoT Hub verfolgt Ereignisse in verschiedenen Vorgangskategorien nach. Sie können festlegen, dass Ereignisse aus einer oder mehreren Kategorien zur Verarbeitung an einen Endpunkt Ihres IoT-Hubs gesendet werden. Sie können die Daten auf Fehler überwachen oder eine komplexere auf Datenmustern basierende Verarbeitung einrichten.

IoT Hub überwacht sechs Ereigniskategorien:

* Geräte-Identitätsvorgänge
* Gerätetelemetrie
* C2D-Nachrichten
* Verbindungen
* Dateiuploads
* Nachrichtenweiterleitung

## <a name="how-to-enable-operations-monitoring"></a>Aktivieren der Vorgangsüberwachung

1. Erstellen Sie einen IoT Hub. Anweisungen zum Erstellen eines IoT-Hubs finden Sie in der Anleitung mit den [ersten Schritten][lnk-get-started].

1. Öffnen Sie das Blatt Ihres IoT Hubs. Klicken Sie dort auf **Vorgangsüberwachung**.

    ![Zugriff auf die Konfiguration der Vorgangsüberwachung im Portal][1]

1. Wählen Sie die Kategorien aus, die Sie überwachen möchten, und klicken Sie dann auf **Speichern**. Die Ereignisse können aus dem Event Hub-kompatiblen Endpunkt gelesen werden, der in **Überwachungseinstellungen**aufgelistet ist. Der IoT Hub-Endpunkt heißt `messages/operationsmonitoringevents`.

    ![Konfigurieren der Vorgangsüberwachung in IoT Hub][2]

> [!NOTE]
> Die Auswahl von **Ausführlich** für die Überwachung für die Kategorie **Verbindungen** bewirkt, dass IoT Hub zusätzliche Diagnosemeldungen generiert. Für alle anderen Kategorien ändert die Einstellung **Ausführlich** die Menge der Informationen, die IoT Hub in jede Fehlermeldung einbezieht.

## <a name="event-categories-and-how-to-use-them"></a>Ereigniskategorien und ihre Verwendung

Jede Vorgangsüberwachungskategorie dient der Nachverfolgung eines anderen Interaktionstyps mit dem IoT Hub. Jede Überwachungskategorie hat ein Schema, das festlegt, wie Ereignisse in dieser Kategorie strukturiert sind.

### <a name="device-identity-operations"></a>Geräte-Identitätsvorgänge

Die Kategorie für Geräte-Identitätsvorgänge dient der Nachverfolgung von Fehlern, die auftreten, wenn Sie versuchen, einen Eintrag in der Identitätsregistrierung Ihres IoT Hubs zu erstellen, zu aktualisieren oder zu löschen. Das Nachverfolgen dieser Kategorie ist für Bereitstellungsszenarien nützlich.

```json
{
    "time": "UTC timestamp",
        "operationName": "create",
        "category": "DeviceIdentityOperations",
        "level": "Error",
        "statusCode": 4XX,
        "statusDescription": "MessageDescription",
        "deviceId": "device-ID",
        "durationMs": 1234,
        "userAgent": "userAgent",
        "sharedAccessPolicy": "accessPolicy"
}
```

### <a name="device-telemetry"></a>Gerätetelemetrie

Die Kategorie für Gerätetelemetrie dient der Nachverfolgung von Fehlern, die auf dem IoT Hub auftreten und in Beziehung zur Telemetriepipeline stehen. Zu dieser Kategorie gehören Fehler, die beim Senden von Telemetrie-Ereignissen (z.B. Drosselung) und Empfangen von Telemetrie-Ereignissen (z.B. nicht autorisierte Leser) auftreten. Diese Kategorie kann keine Fehler abfangen, die vom Code verursacht werden, der auf dem Gerät selbst ausgeführt wird.

```json
{
        "messageSizeInBytes": 1234,
        "batching": 0,
        "protocol": "Amqp",
        "authType": "{\"scope\":\"device\",\"type\":\"sas\",\"issuer\":\"iothub\"}",
        "time": "UTC timestamp",
        "operationName": "ingress",
        "category": "DeviceTelemetry",
        "level": "Error",
        "statusCode": 4XX,
        "statusType": 4XX001,
        "statusDescription": "MessageDescription",
        "deviceId": "device-ID",
        "EventProcessedUtcTime": "UTC timestamp",
        "PartitionId": 1,
        "EventEnqueuedUtcTime": "UTC timestamp"
}
```

### <a name="cloud-to-device-commands"></a>C2D-Befehle

Die Kategorie der C2D-Befehle dient der Nachverfolgung von Fehlern, die auf dem IoT Hub auftreten und in Beziehung zur C2D-Nachrichtenpipeline stehen. Zu dieser Kategorie gehören Fehler, die beim Senden von C2D-Nachrichten (z.B. nicht autorisierter Absender), Empfangen von C2D-Nachrichten (z.B. Zustellungsanzahl überschritten) und C2D-Nachrichtenfeedback (z.B. Feedback abgelaufen) auftreten. Diese Kategorie fängt keine Fehler eines Geräts ab, das eine C2D-Nachricht nicht ordnungsgemäß verarbeitet, wenn die C2D-Nachricht erfolgreich übermittelt wurde.

```json
{
    "messageSizeInBytes": 1234,
    "authType": "{\"scope\":\"hub\",\"type\":\"sas\",\"issuer\":\"iothub\"}",
    "deliveryAcknowledgement": 0,
    "protocol": "Amqp",
    "time": " UTC timestamp",
    "operationName": "ingress",
    "category": "C2DCommands",
    "level": "Error",
    "statusCode": 4XX,
    "statusType": 4XX001,
    "statusDescription": "MessageDescription",
    "deviceId": "device-ID",
    "EventProcessedUtcTime": "UTC timestamp",
    "PartitionId": 1,
    "EventEnqueuedUtcTime": “UTC timestamp"
}
```

### <a name="connections"></a>Verbindungen

Die Kategorie für Verbindungen verfolgt Fehler nach, die auftreten, wenn Geräte Verbindungen mit dem IoT Hub herstellen oder trennen. Das Nachverfolgen dieser Kategorie ist nützlich zum Bestimmen nicht autorisierter Verbindungsversuche und zum Nachverfolgen, wann eine Verbindung mit Geräten in Bereichen mit schlechter Konnektivität unterbrochen wird.

```json
{
    "durationMs": 1234,
    "authType": "{\"scope\":\"hub\",\"type\":\"sas\",\"issuer\":\"iothub\"}",
    "protocol": "Amqp",
    "time": " UTC timestamp",
    "operationName": "deviceConnect",
    "category": "Connections",
    "level": "Error",
    "statusCode": 4XX,
    "statusType": 4XX001,
    "statusDescription": "MessageDescription",
    "deviceId": "device-ID"
}
```

### <a name="file-uploads"></a>Dateiuploads

Die Dateiuploadkategorie dient der Nachverfolgung von Fehlern, die auf dem IoT Hub auftreten und in Beziehung zur Dateiuploadfunktionalität stehen. In diese Kategorie fällt Folgendes:

* Fehler, die beim SAS-URI auftreten, beispielsweise wenn er abläuft, bevor ein Gerät den Hub über einen abgeschlossenen Upload informiert
* Vom Gerät gemeldete Uploadfehler
* Fehler, die auftreten, wenn während der IoT Hub-Benachrichtigungserstellung eine Datei nicht im Speicher gefunden wird

Diese Kategorie kann keine Fehler abfangen, die auftreten, während das Gerät eine Datei in den Speicher hochlädt.

```json
{
    "authType": "{\"scope\":\"hub\",\"type\":\"sas\",\"issuer\":\"iothub\"}",
    "protocol": "HTTP",
    "time": " UTC timestamp",
    "operationName": "ingress",
    "category": "fileUpload",
    "level": "Error",
    "statusCode": 4XX,
    "statusType": 4XX001,
    "statusDescription": "MessageDescription",
    "deviceId": "device-ID",
    "blobUri": "http//bloburi.com",
    "durationMs": 1234
}
```

### <a name="message-routing"></a>Nachrichtenweiterleitung

Die Kategorie „Nachrichtenweiterleitung“ verfolgt Fehler, die während der Auswertung der Nachrichtenweiterleitung auftreten, und die durch IoT Hub wahrgenommene Endpunktintegrität. Diese Kategorie umfasst z.B. folgende Ereignisse: Eine Regel wird als „nicht definiert“ ausgewertet, IoT Hub markiert einen Endpunkt als inaktiv und alle anderen Fehler, die von einem Endpunkt empfangen werden. Diese Kategorie umfasst keine bestimmten Fehler zu den Nachrichten selbst (z.B. Gerätedrosselungsfehler), die in der Kategorie „Gerätetelemetrie“ gemeldet werden.

```json
{
    "messageSizeInBytes": 1234,
    "time": "UTC timestamp",
    "operationName": "ingress",
    "category": "routes",
    "level": "Error",
    "deviceId": "device-ID",
    "messageId": "ID of message",
    "routeName": "myroute",
    "endpointName": "myendpoint",
    "details": "ExternalEndpointDisabled"
}
```

## <a name="view-events"></a>Ereignisse anzeigen

Sie können das Tool *iothub-explorer* verwenden, um schnell zu überprüfen, ob Ihr IoT Hub Überwachungsereignisse generiert. Anweisungen zum Installieren dieses Tools finden Sie im [iothub-explorer][lnk-iothub-explorer] GitHub-Repository.

1. Stellen Sie sicher, dass die Überwachungskategorie **Verbindungen** im Portal auf **Ausführlich** festgelegt ist.

1. Führen Sie an einer Befehlszeile den folgenden Befehl aus, um aus dem Überwachungsendpunkt zu lesen.

    ```
    iothub-explorer monitor-ops --login {your iothubowner connection string}
    ```

1. Führen Sie in einer anderen Eingabeaufforderung den folgenden Befehl zum Simulieren eines Geräts, das Gerät-zu-Cloud-Nachrichten sendet, aus:

    ```
    iothub-explorer simulate-device {your device name} --send "My test message" --login {your iothubowner connection string}
    ```

1. Die erste Befehlszeile zeigt die Überwachungsereignisse an, wenn das simulierte Gerät eine Verbindung mit Ihrem IoT Hub herstellt.

## <a name="connect-to-the-monitoring-endpoint"></a>Herstellen einer Verbindung mit dem Überwachungsendpunkt

Der Überwachungsendpunkt in IoT Hub ist ein mit Event Hub kompatibler Endpunkt. Sie können einen beliebigen für Event Hubs geeigneten Mechanismus zum Lesen von Überwachungsnachrichten von diesem Endpunkt verwenden. Im folgenden Beispiel wird ein einfacher Reader erstellt, der für eine Bereitstellung mit hohem Durchsatz nicht geeignet ist. Weitere Informationen zum Verarbeiten von Nachrichten von Event Hubs finden Sie im Tutorial [Erste Schritte mit Event Hubs][lnk-eventhubs-tutorial].

Zum Herstellen einer Verbindung mit dem Überwachungsendpunkt benötigen Sie eine Verbindungszeichenfolge und den Endpunktnamen. Führen Sie die folgenden Schritte aus, um die erforderlichen Werte im Portal zu ermitteln:

1. Navigieren Sie im Portal zum Blatt für die IoT Hub-Ressource.

1. Wählen Sie **Vorgangsüberwachung** aus, und notieren Sie sich die Werte für **Event Hub-kompatibler Name** und **Event Hub-kompatibler Endpunkt**:

    ![Werte für Event Hub-kompatiblen Endpunkt][img-endpoints]

1. Wählen Sie **Freigegebene Zugriffsrichtlinien** und dann **Dienst** aus. Notieren Sie sich den Wert von **Primärschlüssel**:

    ![Primärschlüssel des Diensts unter „SAS-Richtlinie“][img-service-key]

Das folgende C#-Codebeispiel stammt aus einer Visual Studio C#-Konsolenanwendung für den **klassischen Windows-Desktop**. Im Projekt ist das NuGet-Paket **WindowsAzure.ServiceBus** installiert.

* Ersetzen Sie den Platzhalter für die Verbindungszeichenfolge durch eine Verbindungszeichenfolge, in der die Werte für **Event Hub-kompatibler Endpunkt** und Dienst-**Primärschlüssel** verwendet werden, die Sie sich zuvor notiert haben, wie im folgenden Beispiel gezeigt:

    ```cs
    "Endpoint={your Event Hub-compatible endpoint};SharedAccessKeyName=service;SharedAccessKey={your service primary key value}"
    ```

* Ersetzen Sie den Platzhalter für den Namen des Überwachungsendpunkts durch den Wert von **Event Hub-kompatibler Name**, den Sie sich zuvor notiert haben.

```cs
class Program
{
    static string connectionString = "{your monitoring endpoint connection string}";
    static string monitoringEndpointName = "{your monitoring endpoint name}";
    static EventHubClient eventHubClient;

    static void Main(string[] args)
    {
        Console.WriteLine("Monitoring. Press Enter key to exit.\n");

        eventHubClient = EventHubClient.CreateFromConnectionString(connectionString, monitoringEndpointName);
        var d2cPartitions = eventHubClient.GetRuntimeInformation().PartitionIds;
        CancellationTokenSource cts = new CancellationTokenSource();
        var tasks = new List<Task>();

        foreach (string partition in d2cPartitions)
        {
            tasks.Add(ReceiveMessagesFromDeviceAsync(partition, cts.Token));
        }

        Console.ReadLine();
        Console.WriteLine("Exiting...");
        cts.Cancel();
        Task.WaitAll(tasks.ToArray());
    }

    private static async Task ReceiveMessagesFromDeviceAsync(string partition, CancellationToken ct)
    {
        var eventHubReceiver = eventHubClient.GetDefaultConsumerGroup().CreateReceiver(partition, DateTime.UtcNow);
        while (true)
        {
            if (ct.IsCancellationRequested)
            {
                await eventHubReceiver.CloseAsync();
                break;
            }

            EventData eventData = await eventHubReceiver.ReceiveAsync(new TimeSpan(0,0,10));

            if (eventData != null)
            {
                string data = Encoding.UTF8.GetString(eventData.GetBytes());
                Console.WriteLine("Message received. Partition: {0} Data: '{1}'", partition, data);
            }
        }
    }
}
```

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen zu den Funktionen von IoT Hub finden Sie unter:

* [IoT Hub-Entwicklerhandbuch][lnk-devguide]
* [Simulieren eines Geräts mit Azure IoT Edge][lnk-iotedge]

<!-- Links and images -->
[1]: media/iot-hub-operations-monitoring/enable-OM-1.png
[2]: media/iot-hub-operations-monitoring/enable-OM-2.png
[img-endpoints]: media/iot-hub-operations-monitoring/monitoring-endpoint.png
[img-service-key]: media/iot-hub-operations-monitoring/service-key.png

[lnk-get-started]: iot-hub-csharp-csharp-getstarted.md
[lnk-diagnostic-metrics]: iot-hub-metrics.md
[lnk-scaling]: iot-hub-scaling.md
[lnk-dr]: iot-hub-ha-dr.md

[lnk-devguide]: iot-hub-devguide.md
[lnk-iotedge]: iot-hub-linux-iot-edge-simulated-device.md
[lnk-iothub-explorer]: https://github.com/azure/iothub-explorer
[lnk-eventhubs-tutorial]: ../event-hubs/event-hubs-csharp-ephcs-getstarted.md
