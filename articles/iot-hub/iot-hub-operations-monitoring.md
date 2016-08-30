<properties
 pageTitle="IoT Hub-Vorgangsüberwachung"
 description="Eine Übersicht über die IoT Hub-Vorgangsüberwachung, die Ihnen das Überwachen des Status von Vorgängen auf Ihrem IoT Hub in Echtzeit ermöglicht"
 services="iot-hub"
 documentationCenter=""
 authors="nberdy"
 manager="timlt"
 editor=""/>

<tags
 ms.service="iot-hub"
 ms.devlang="na"
 ms.topic="article"
 ms.tgt_pltfrm="na"
 ms.workload="na"
 ms.date="08/11/2016"
 ms.author="nberdy"/>

# Einführung in Vorgangsüberwachung

Die IoT Hub-Vorgangsüberwachung ermöglicht Ihnen das Überwachen des Status von Vorgängen auf Ihrem IoT Hub in Echtzeit. IoT Hub verfolgt Ereignisse in mehreren Vorgangskategorien. Sie können festlegen, dass Ereignisse aus einer oder mehreren Kategorien zur Verarbeitung an einen Endpunkt Ihres IoT Hubs gesendet werden. Sie können die Daten auf Fehler überwachen oder eine komplexere auf Datenmustern basierende Verarbeitung einrichten.

IoT Hub überwacht fünf Ereigniskategorien:

- Geräte-Identitätsvorgänge
- Gerätetelemetrie
- C2D-Befehle
- Verbindungen
- Dateiuploads

## Aktivieren der Vorgangsüberwachung

1. Erstellen Sie einen IoT Hub. Anweisungen zum Erstellen eines IoT Hubs finden Sie in der Anleitung mit den [ersten Schritten][lnk-get-started].

2. Öffnen Sie das Blatt Ihres IoT Hubs. Klicken Sie dort auf **Vorgangsüberwachung**.

    ![][1]

3. Wählen Sie die Kategorien aus, die Sie überwachen möchten, und klicken Sie dann auf **Speichern**. Die Ereignisse können aus dem Event Hub-kompatiblen Endpunkt gelesen werden, der in **Überwachungseinstellungen** aufgelistet ist. Der IoT Hub-Endpunkt heißt `messages/operationsmonitoringevents`.

    ![][2]

## Ereigniskategorien und ihre Verwendung

Jede Vorgangsüberwachungskategorie dient der Nachverfolgung eines anderen Interaktionstyps mit dem IoT Hub. Jede Überwachungskategorie hat ein Schema, das festlegt, wie Ereignisse in dieser Kategorie strukturiert sind.

### Geräte-Identitätsvorgänge

Die Kategorie für Geräte-Identitätsvorgänge dient der Nachverfolgung von Fehlern, die auftreten, wenn Sie versuchen, einen Eintrag in der Identitätsregistrierung Ihres IoT Hubs zu erstellen, zu aktualisieren oder zu löschen. Das Nachverfolgen dieser Kategorie ist für Bereitstellungsszenarien nützlich.

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

### Gerätetelemetrie

Die Kategorie für Gerätetelemetrie dient der Nachverfolgung von Fehlern, die auf dem IoT Hub auftreten und in Beziehung zur Telemetriepipeline stehen. Zu dieser Kategorie gehören Fehler, die beim Senden von Telemetrie-Ereignissen (z.B. Drosselung) und Empfangen von Telemetrie-Ereignissen (z.B. nicht autorisierte Leser) auftreten. Beachten Sie, dass diese Kategorie keine Fehler abfangen kann, die vom Code verursacht werden, der auf dem Gerät selbst ausgeführt wird.

    {
         "messageSizeInBytes": 1234,
         "batching": 0,
         "protocol": "Amqp",
         "authType": "{"scope":"device","type":"sas","issuer":"iothub"}",
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

### C2D-Befehle

Die Kategorie der C2D-Befehle dient der Nachverfolgung von Fehlern, die auf dem IoT Hub auftreten und in Beziehung zur Gerätebefehlspipeline stehen. Zu dieser Kategorie gehören Fehler, die beim Senden von Befehlen (z.B. nicht autorisierter Absender), Empfangen von Befehlen (Zustellungsanzahl überschritten) und Empfangen von Feedback zu Befehlen (z.B. Feedback abgelaufen) auftreten. Diese Kategorie fängt keine Fehler eines Geräts ab, das einen Befehl nicht ordnungsgemäß verarbeitet, wenn der Befehl erfolgreich übermittelt wurde.

    {
         "messageSizeInBytes": 1234,
         "authType": "{"scope":"hub","type":"sas","issuer":"iothub"}",
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

### Verbindungen

Die Kategorie für Verbindungen verfolgt Fehler nach, die auftreten, wenn Geräte Verbindungen mit dem IoT Hub herstellen oder trennen. Das Nachverfolgen dieser Kategorie ist nützlich zum Bestimmen nicht autorisierter Verbindungsversuche und zum Nachverfolgen, wann eine Verbindung mit Geräten in Bereichen mit schlechter Konnektivität unterbrochen wird.

    {
         "durationMs": 1234,
         "authType": "{"scope":"hub","type":"sas","issuer":"iothub"}",
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

### Dateiuploads

Die Dateiuploadkategorie dient der Nachverfolgung von Fehlern, die auf dem IoT Hub auftreten und in Beziehung zur Dateiuploadfunktionalität stehen. Zu dieser Kategorie gehören Fehler, die in Verbindung mit dem SAS-URI auftreten (z.B. wenn dieser abläuft, bevor ein Gerät den Hub über einen abgeschlossenen Upload unterrichtet), vom Gerät gemeldete fehlerhafte Uploads, und wenn eine Datei während der Erstellung der IoT Hub-Nachricht nicht im Speicher gefunden wird. Beachten Sie, dass diese Kategorie keine Fehler abfangen kann, die auftreten, während das Gerät eine Datei in den Speicher hochlädt.

    {
         "authType": "{"scope":"hub","type":"sas","issuer":"iothub"}",
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

## Nächste Schritte

Nachdem Sie sich nun einen Überblick über die Überwachung von Vorgängen verschafft haben, finden Sie unter [Verwalten des Zugriffs auf IoT Hub][lnk-itpro] zusätzliche Informationen zum Verwalten von IoT Hub.

Weitere Informationen zu den Funktionen von IoT Hub finden Sie unter:

- [Entwerfen Ihrer Lösung][lnk-design]
- [Entwicklerhandbuch][lnk-devguide]
- [Erkunden der Geräteverwaltung mithilfe der Beispielbenutzeroberfläche][lnk-dmui]
- [Simulieren eines Geräts mit dem Gateway SDK][lnk-gateway]

<!-- Links and images -->
[1]: media/iot-hub-operations-monitoring/enable-OM-1.png
[2]: media/iot-hub-operations-monitoring/enable-OM-2.png

[lnk-get-started]: iot-hub-csharp-csharp-getstarted.md
[lnk-diagnostic-metrics]: iot-hub-metrics.md
[lnk-scaling]: iot-hub-scaling.md
[lnk-dr]: iot-hub-ha-dr.md

[lnk-itpro]: iot-hub-itpro-info.md

[lnk-design]: iot-hub-guidance.md
[lnk-devguide]: iot-hub-devguide.md
[lnk-dmui]: iot-hub-device-management-ui-sample.md
[lnk-gateway]: iot-hub-linux-gateway-sdk-simulated-device.md

<!---HONumber=AcomDC_0817_2016-->