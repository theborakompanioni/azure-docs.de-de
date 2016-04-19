<properties
	pageTitle="Erste Schritte mit Azure IoT Hub für C# | Microsoft Azure"
	description="Tutorial für den Einstieg in Azure IoT Hub mit C#. Verwenden Sie Azure IoT Hub und C# mit den Microsoft Azure IoT SDKs, um eine IoT-Lösung zu implementieren."
	services="iot-hub"
	documentationCenter=".net"
	authors="dominicbetts"
	manager="timlt"
	editor=""/>

<tags
     ms.service="iot-hub"
     ms.devlang="dotnet"
     ms.topic="hero-article"
     ms.tgt_pltfrm="na"
     ms.workload="na"
     ms.date="03/22/2016"
     ms.author="dobett"/>

# Erste Schritte mit Azure IoT Hub für .NET

[AZURE.INCLUDE [iot-hub-selector-get-started](../../includes/iot-hub-selector-get-started.md)]

## Einführung

Azure IoT Hub ist ein vollständig verwalteter Dienst, der eine zuverlässige und sichere bidirektionale Kommunikation zwischen Millionen von IoT-Geräten und einem Lösungs-Back-End ermöglicht. Eines der größten Probleme im Zusammenhang mit IoT-Projekten ist die sichere und zuverlässige Verbindung von Geräten mit dem Lösungs-Back-End. Um diese Herausforderungen zu meistern, bietet IoT Hub:

- Ein zuverlässiges, hyperskalierbares Messaging zwischen Geräten und Cloud (Device-to-Cloud, D2C) sowie zwischen Cloud und Geräten (Cloud-to-Device, C2D)
- Eine sichere Kommunikation unter Verwendung von Zugriffssteuerung und Sicherheitsanmeldeinformationen auf Gerätebasis
- Gerätebibliotheken für die gängigsten Sprachen und Plattformen

Dieses Tutorial veranschaulicht folgende Vorgehensweisen:

- Erstellen eines IoT Hubs mit dem Azure-Portal
- Erstellen einer Geräteidentität im IoT Hub
- Erstellen eines simulierten Geräts, das Telemetrie an das Cloud-Back-End sendet und vom Cloud-Back-End Befehle empfängt

Am Ende dieses Tutorials verfügen Sie über drei Windows-Konsolenanwendungen:

* **CreateDeviceIdentity**. Hiermit werden eine Geräteidentität und ein zugehöriger Sicherheitsschlüssel zum Verbinden mit Ihrem simulierten Gerät erstellt.
* **ReadDeviceToCloudMessages**. Hiermit wird die Telemetrie angezeigt, die Ihr simuliertes Gerät sendet.
* **SimulatedDevice**. Hiermit wird mithilfe der zuvor erstellten Geräteidentität eine Verbindung mit Ihrem IoT Hub hergestellt und jede Sekunde unter Verwendung des AMQPS-Protokolls eine Telemetrienachricht gesendet.

> [AZURE.NOTE] Im Artikel [IoT Hub-SDKs][lnk-hub-sdks] finden Sie Informationen über die verschiedenen SDKs, mit denen Sie sowohl Anwendungen für Geräte als auch das zugehörige Lösungs-Back-End erstellen können.

Für dieses Tutorial benötigen Sie Folgendes:

+ Microsoft Visual Studio 2015.

+ Ein aktives Azure-Konto. <br/>Wenn Sie noch kein Konto haben, können Sie in nur wenigen Minuten ein kostenloses Testkonto erstellen. Einzelheiten finden Sie unter [Kostenlose Azure-Testversion][lnk-free-trial].

[AZURE.INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

Sie haben nun Ihren IoT Hub erstellt und verfügen über den Hostnamen und die Verbindungszeichenfolge, die Sie für die weiteren Schritte in diesem Tutorial benötigen.

[AZURE.INCLUDE [iot-hub-get-started-cloud-csharp](../../includes/iot-hub-get-started-cloud-csharp.md)]


[AZURE.INCLUDE [iot-hub-get-started-device-csharp](../../includes/iot-hub-get-started-device-csharp.md)]

## Ausführen der Anwendungen

Sie können nun die Anwendungen ausführen.

1.	Klicken Sie in Visual Studio im Projektmappen-Explorer mit der rechten Maustaste auf Ihre Projektmappe, und klicken Sie dann auf **Startprojekte festlegen**. Wählen Sie **Mehrere Startprojekte** aus, und wählen Sie dann für die Projekte **ReadDeviceToCloudMessages** und **SimulatedDevice** als **Aktion** die Option **Starten** aus.

   	![][41]

2.	Drücken Sie **F5**, um die Ausführung der beiden Apps zu starten. Die Konsolenausgabe der App **SimulatedDevice** zeigt die Nachrichten, die Ihr simuliertes Gerät an Ihren IoT Hub sendet, und die Konsolenausgabe der App **ReadDeviceToCloudMessages** zeigt die Nachrichten, die Ihr IoT Hub empfängt.

   	![][42]

3. Über die Kachel **Nutzung** im [Azure-Portal][lnk-portal] wird die Anzahl der an den Hub gesendeten Nachrichten angezeigt:

    ![][43]


## Nächste Schritte

In diesem Tutorial haben Sie im Portal einen neuen IoT Hub konfiguriert und anschließend in der Identitätsregistrierung des Hubs eine Geräteidentität erstellt. Sie haben diese Geräteidentität verwendet, um der simulierten Geräte-App das Senden von D2C-Nachrichten an den Hub zu ermöglichen, und Sie haben eine App erstellt, die die vom Hub empfangenen Nachrichten anzeigt. In den folgenden Tutorials werden weitere Features und Szenarien für IoT Hubs vorgestellt:

- Unter [Senden von C2D-Nachrichten mit IoT Hub][lnk-c2d-tutorial] wird erläutert, wie Sie Nachrichten an Geräte senden und das von IoT Hub generierte Feedback zur Übermittlung verarbeiten.
- In [Verarbeiten von D2C-Nachrichten][lnk-process-d2c-tutorial] wird erläutert, wie Sie zuverlässig Telemetriedaten und interaktive Nachrichten von Geräten verarbeiten können.
- In [Hochladen von Dateien von Geräten][lnk-upload-tutorial] wird beschrieben, wie mithilfe von C2D-Nachrichten Dateiuploads von Geräten erleichtert werden.

<!-- Images. -->
[41]: ./media/iot-hub-csharp-csharp-getstarted/run-apps1.png
[42]: ./media/iot-hub-csharp-csharp-getstarted/run-apps2.png
[43]: ./media/iot-hub-csharp-csharp-getstarted/usage.png

<!-- Links -->
[lnk-c2d-tutorial]: iot-hub-csharp-csharp-c2d.md
[lnk-process-d2c-tutorial]: iot-hub-csharp-csharp-process-d2c.md
[lnk-upload-tutorial]: iot-hub-csharp-csharp-file-upload.md

[lnk-hub-sdks]: iot-hub-sdks-summary.md
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-portal]: https://portal.azure.com/

<!---HONumber=AcomDC_0413_2016-->