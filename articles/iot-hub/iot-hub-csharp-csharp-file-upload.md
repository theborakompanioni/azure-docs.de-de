<properties
	pageTitle="Hochladen von Dateien von Geräten mit IoT Hub | Microsoft Azure"
	description="In diesem Lernprogramm erfahren Sie, wie Sie mithilfe von Azure IoT Hub mit C# Dateien von Geräten hochladen."
	services="iot-hub"
	documentationCenter=".net"
	authors="fsautomata"
	manager="timlt"
	editor=""/>

<tags
     ms.service="iot-hub"
     ms.devlang="dotnet"
     ms.topic="article"
     ms.tgt_pltfrm="na"
     ms.workload="na"
     ms.date="06/21/2016"
     ms.author="elioda"/>

# Lernprogramm: Hochladen von Dateien von Geräten in die Cloud mit IoT Hub

## Einführung

Azure IoT Hub ist ein vollständig verwalteter Dienst, der eine zuverlässige und sichere bidirektionale Kommunikation zwischen Millionen von IoT-Geräten und einem Anwendungs-Back-End ermöglicht. In vorhergehenden Tutorials ([Erste Schritte mit IoT Hub] und [Senden von C2D-Nachrichten mit IoT Hub]) werden die grundlegenden Funktionen von IoT Hub für Gerät-zu-Cloud- (Device-to-Cloud, D2C) und Cloud-zu-Gerät-Messaging (Cloud-to-Device, C2D) und der Zugriff auf diese Funktionen über Geräte und Cloudkomponenten beschrieben. Im Tutorial [Verarbeiten von Gerät-zu-Cloud-Nachrichten] wird eine Möglichkeit zum zuverlässigen Speichern von D2C-Nachrichten in Azure-Blobspeicher beschrieben. In einigen Szenarien können Sie allerdings nicht einfach die Daten, die Ihre Geräte senden, den relativ kleinen D2C-Nachrichten zuordnen, die IoT Hub akzeptiert. Beispiele sind große Dateien mit Bildern, Videos, Vibrationsdatenerfassungen mit hoher Frequenz oder vorverarbeiteten Daten. Diese Dateien werden normalerweise als Batch in der Cloud mit Tools wie [Azure Data Factory] oder dem [Hadoop]-Stapel verarbeitet. Wenn das Hochladen einer Datei von einem Gerät dem Senden von Ereignissen vorgezogen wird, können dennoch IoT Hub-Sicherheits- und Zuverlässigkeitsfunktionen verwendet werden.

Dieses Tutorial baut auf dem Code im Tutorial [Senden von Nachrichten von Cloud-zu-Gerät mit IoT Hub] auf, um zu zeigen, wie Sie die IoT Hub-Funktionen zum Hochladen von Dateien nutzen. Es zeigt, wie Sie einem Gerät einen Azure-Blob-URI zum Hochladen einer Datei sicher bereitstellen können und wie die IoT Hub-Benachrichtigungen zum Hochladen von Dateien zum Auslösen der Verarbeitung der Datei in Ihrem App-Back-End verwendet werden.

Am Ende dieses Tutorials führen Sie zwei Windows-Konsolenanwendungen aus:

* **SimulatedDevice**, eine geänderte Version der App, die im Tutorial [Senden von Cloud-zu-Gerät-Nachrichten mit IoT Hub] erstellt wurde. Sie dient zum Hochladen einer Datei in Speicher mithilfe eines SAS-URI, den Ihr IoT Hub bereitgestellt hat.
* **ReadFileUploadNotification**, die Benachrichtigungen zum Hochladen von Dateien von Ihrem IoT Hub empfängt.

> [AZURE.NOTE] IoT Hub bietet über Azure IoT-Geräte-SDKs Unterstützung für zahlreiche Geräteplattformen und Sprachen (u.a. C, Java und JavaScript). Im [Azure IoT Developer Center] finden Sie detaillierte Anweisungen zum Verbinden eines Geräts mit dem in diesem Tutorial gezeigten Code sowie allgemeine Informationen zum Verbinden mit Azure IoT Hub.

Zum Durchführen dieses Tutorials benötigen Sie Folgendes:

+ Microsoft Visual Studio 2015

+ Ein aktives Azure-Konto. <br/>Wenn Sie kein Konto haben, können Sie in nur wenigen Minuten ein kostenloses Konto erstellen. Weitere Informationen finden Sie unter [Kostenlose Azure-Testversion](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fde-DE%2Fdevelop%2Fiot%2Ftutorials%2Ffile-upload%2F target="\_blank").

[AZURE.INCLUDE [iot-hub-file-upload-device-csharp](../../includes/iot-hub-file-upload-device-csharp.md)]


[AZURE.INCLUDE [iot-hub-file-upload-cloud-csharp](../../includes/iot-hub-file-upload-cloud-csharp.md)]

## Ausführen der Anwendungen

Sie können jetzt die Anwendung ausführen.

1. Klicken Sie in Visual Studio mit der rechten Maustaste auf Ihre Projektmappe, und wählen Sie **Startprojekte festlegen** aus. Wählen Sie **Mehrere Startprojekte** und dann die Aktion **Starten** für **ReadFileUploadNotification** und **SimulatedDevice** aus.

2. Drücken Sie **F5**. Beide Anwendungen sollten gestartet werden. In der einen Konsolen-App sollte eine Nachricht zum Abschluss des Uploads und in der anderen eine Nachricht zum Empfang der Benachrichtigung zum Upload angezeigt werden. Sie können im [Azure-Portal] oder in Visual Studio Server-Explorer überprüfen, ob die hochgeladene Datei in Ihrem Speicherkonto vorhanden ist.

  ![][50]


## Nächste Schritte

In diesem Tutorial haben Sie gelernt, wie Sie die IoT Hub-Funktionen zum Hochladen von Dateien nutzen, um Dateiuploads zu vereinfachen. Im folgenden Lernprogramm werden weitere Funktionen und Szenarios für IoT Hubs vorgestellt:

- In [Verarbeiten von D2C-Nachrichten] wird erläutert, wie Sie zuverlässig Telemetriedaten und interaktive Nachrichten von Geräten verarbeiten können.

Weitere Informationen zu IoT Hub:

* [Übersicht zu IoT Hub]
* [IoT Hub-Entwicklerhandbuch]
* [Anleitungen zu IoT Hub]
* [Unterstützte Geräteplattformen und Sprachen][Supported devices]
* [Azure IoT Developer Center]

<!-- Images. -->

[50]: ./media/iot-hub-csharp-csharp-file-upload/run-apps1.png

<!-- Links -->

[Send Cloud-to-Device messages with IoT Hub]: iot-hub-csharp-csharp-c2d.md

[Azure-Portal]: https://portal.azure.com/

[Azure Data Factory]: https://azure.microsoft.com/documentation/services/data-factory/
[Hadoop]: https://azure.microsoft.com/documentation/services/hdinsight/

[Get started with IoT Hub]: iot-hub-csharp-csharp-getstarted.md
[Senden von C2D-Nachrichten mit IoT Hub]: iot-hub-csharp-csharp-c2d.md
[Senden von Cloud-zu-Gerät-Nachrichten mit IoT Hub]: iot-hub-csharp-csharp-c2d.md
[Senden von Nachrichten von Cloud-zu-Gerät mit IoT Hub]: iot-hub-csharp-csharp-c2d.md
[Verarbeiten von D2C-Nachrichten]: iot-hub-csharp-csharp-process-d2c.md
[Verarbeiten von Gerät-zu-Cloud-Nachrichten]: iot-hub-csharp-csharp-process-d2c.md
[Uploading files from devices]: iot-hub-csharp-csharp-file-upload.md

[Übersicht zu IoT Hub]: iot-hub-what-is-iot-hub.md
[Anleitungen zu IoT Hub]: iot-hub-guidance.md
[IoT Hub-Entwicklerhandbuch]: iot-hub-devguide.md
[IoT Hub Supported Devices]: iot-hub-supported-devices.md
[Erste Schritte mit IoT Hub]: iot-hub-csharp-csharp-getstarted.md
[Supported devices]: https://github.com/Azure/azure-iot-sdks/blob/master/doc/tested_configurations.md
[Azure IoT Developer Center]: http://www.azure.com/develop/iot

<!---HONumber=AcomDC_0622_2016-->