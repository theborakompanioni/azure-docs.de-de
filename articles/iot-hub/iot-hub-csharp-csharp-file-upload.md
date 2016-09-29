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

+ Ein aktives Azure-Konto. (Falls Sie nicht über ein Konto verfügen, können Sie in nur wenigen Minuten ein kostenloses Testkonto erstellen. Ausführliche Informationen finden Sie unter [Kostenlose Azure-Testversion][lnk-free-trial].)

## Zuweisen eines Azure Storage-Kontos zu IoT Hub

Da vom simulierten Gerät eine Datei in ein Azure Storage-Blob hochgeladen wird, müssen Sie über ein [Azure Storage]-Konto verfügen, das IoT Hub zugeordnet ist. Wenn Sie ein Speicherkonto mit einer IoT Hub-Instanz verknüpfen, kann der Hub einen SAS-URI generieren, den ein Gerät zum sicheren Hochladen einer Datei an einen Blobcontainer verwenden kann. Der IoT Hub-Dienst und die Geräte-SDKs koordinieren den Prozess, der die SAS-URI generiert und für ein Gerät verfügbar macht, sodass er beim Hochladen einer Datei verwendet werden kann.

Gehen Sie wie unter [Verwalten von IoT Hubs über das Azure-Portal] vor, um ein Azure Storage-Konto mit Ihrer IoT Hub-Instanz zu verknüpfen.

## Hochladen einer Datei von einem simulierten Gerät

In diesem Abschnitt ändern Sie die simulierte Geräteanwendung, die Sie in [Senden von Cloud-zu-Gerät-Nachrichten mit IoT Hub] erstellt haben, um Cloud-zu-Gerät-Nachrichten von IoT Hub zu empfangen.

1. Klicken Sie in Visual Studio mit der rechten Maustaste auf das Projekt **SimulatedDevice**. Klicken Sie auf **Hinzufügen** und dann auf **Vorhandenes Element**. Navigieren Sie zu einer Bilddatei, und fügen Sie sie Ihrem Projekt hinzu. In diesem Tutorial wird davon ausgegangen, dass das Bild `image.jpg` heißt.

2. Klicken Sie mit der rechten Maustaste auf das Bild, und klicken Sie dann auf **Eigenschaften**. Stellen Sie sicher, dass **In Ausgabeverzeichnis kopieren** auf **Immer kopieren** festgelegt ist.

    ![][1]

3. Fügen Sie zu Beginn der Datei **Program.cs** die folgenden Anweisungen hinzu:

        using System.IO;

4. Fügen Sie der **Program**-Klasse die folgende Methode hinzu:
         
        private static async void SendToBlobAsync()
        {
            string fileName = "image.jpg";
            Console.WriteLine("Uploading file: {0}", fileName);
            var watch = System.Diagnostics.Stopwatch.StartNew();

            using (var sourceData = new FileStream(@"image.jpg", FileMode.Open))
            {
                await deviceClient.UploadToBlobAsync(fileName, sourceData);
            }

            watch.Stop();
            Console.WriteLine("Time to upload file: {0}ms\n", watch.ElapsedMilliseconds);
        }

    Die `UploadToBlobAsync`-Methode verwendet den Dateinamen und die Datenstromquelle der hochzuladenden Datei und behandelt das Hochladen in den Speicher. Die Konsolenanwendung zeigt den Zeitaufwand für das Hochladen der Datei an.

5. Fügen Sie in der **Main**-Methode unmittelbar vor der Zeile `Console.ReadLine()` die folgende Methode hinzu:

        SendToBlobAsync();

> [AZURE.NOTE] Der Einfachheit halber wird in diesem Lernprogramm keine Wiederholungsrichtlinie implementiert. Im Produktionscode sollten Sie Wiederholungsrichtlinien implementieren (etwa einen exponentiellen Backoff), wie im MSDN-Artikel zum [Behandeln vorübergehender Fehler] beschrieben.

## Erhalten einer Benachrichtigung zum Dateiupload

In diesem Abschnitt schreiben Sie eine Windows-Konsolen-App, die Uploadbenachrichtigungen von IoT Hub empfängt.

1. Erstellen Sie in der aktuellen Visual Studio-Projektmappe mithilfe der Projektvorlage **Konsolenanwendung** ein neues Visual C#-Windows-Projekt. Nennen Sie das Projekt **ReadFileUploadNotification**.

    ![Neues Projekt in Visual Studio][2]

2. Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste auf das Projekt **ReadFileUploadNotification**, und klicken Sie dann auf **NuGet-Pakete verwalten**.

    Daraufhin wird das Fenster "NuGet-Pakete verwalten" angezeigt.

2. Suchen Sie nach `Microsoft.Azure.Devices`, klicken Sie auf **Installieren**, und akzeptieren Sie die Nutzungsbedingungen.

	Daraufhin wird das [NuGet-Paket mit dem SDK des Azure IoT-Diensts] heruntergeladen und installiert. Dem Projekt **ReadFileUploadNotification** wird ein Verweis hinzugefügt.

3. Fügen Sie zu Beginn der Datei **Program.cs** die folgenden Anweisungen hinzu:

        using Microsoft.Azure.Devices;

4. Fügen Sie der **Program**-Klasse die folgenden Felder hinzu. Ersetzen Sie den Platzhalterwert durch die IoT Hub-Verbindungszeichenfolge aus [Erste Schritte mit IoT Hub]\:

		static ServiceClient serviceClient;
        static string connectionString = "{iot hub connection string}";
        
5. Fügen Sie der **Program**-Klasse die folgende Methode hinzu:
   
        private async static Task ReceiveFileUploadNotificationAsync()
        {
            var notificationReceiver = serviceClient.GetFileNotificationReceiver();

            Console.WriteLine("\nReceiving file upload notification from service");
            while (true)
            {
                var fileUploadNotification = await notificationReceiver.ReceiveAsync();
                if (fileUploadNotification == null) continue;

                Console.ForegroundColor = ConsoleColor.Yellow;
                Console.WriteLine("Received file upload noticiation: {0}", string.Join(", ", fileUploadNotification.BlobName));
                Console.ResetColor();

                await notificationReceiver.CompleteAsync(fileUploadNotification);
            }
        }

    Beachten Sie, dass das Empfangsmuster mit dem Muster zum Empfangen von Cloud-zu-Gerät-Nachrichten von der Geräte-App identisch ist.

6. Fügen Sie abschließend der **Main**-Methode die folgenden Zeilen hinzu:

        Console.WriteLine("Receive file upload notifications\n");
        serviceClient = ServiceClient.CreateFromConnectionString(connectionString);
        ReceiveFileUploadNotificationAsync().Wait();
        Console.ReadLine();

## Ausführen der Anwendungen

Sie können jetzt die Anwendung ausführen.

1. Klicken Sie in Visual Studio mit der rechten Maustaste auf Ihre Projektmappe, und wählen Sie **Startprojekte festlegen** aus. Wählen Sie **Mehrere Startprojekte** und dann die Aktion **Starten** für **ReadFileUploadNotification** und **SimulatedDevice** aus.

2. Drücken Sie F5. Beide Anwendungen sollten gestartet werden. In der einen Konsolen-App sollte eine Nachricht zum Abschluss des Uploads und in der anderen eine Nachricht zum Empfang der Benachrichtigung zum Upload angezeigt werden. Sie können im [Azure-Portal] oder in Visual Studio Server-Explorer überprüfen, ob die hochgeladene Datei in Ihrem Speicherkonto vorhanden ist.

  ![][50]


## Nächste Schritte

In diesem Tutorial haben Sie gelernt, wie Sie die IoT Hub-Funktionen zum Hochladen von Dateien nutzen, um Dateiuploads zu vereinfachen. In den folgenden Artikeln werden weitere IoT Hub-Features und -Szenarien vorgestellt:

- [Programmgesteuertes Erstellen eines IoT Hubs][lnk-create-hub]
- [Einführung in das C SDK][lnk-c-sdk]
- [IoT Hub-SDKs][lnk-sdks]

Weitere Informationen zu den Funktionen von IoT Hub finden Sie unter:

- [Entwerfen Ihrer Lösung][lnk-design]
- [Erkunden der Geräteverwaltung mithilfe der Beispielbenutzeroberfläche][lnk-dmui]
- [Simulieren eines Geräts mit dem Gateway SDK][lnk-gateway]
- [Verwenden des Azure-Portals zur Verwaltung von IoT Hub][lnk-portal]

<!-- Images. -->

[50]: ./media/iot-hub-csharp-csharp-file-upload/run-apps1.png
[1]: ./media/iot-hub-csharp-csharp-file-upload/image-properties.png
[2]: ./media/iot-hub-csharp-csharp-file-upload/create-identity-csharp1.png

<!-- Links -->

[Azure-Portal]: https://portal.azure.com/

[Azure Data Factory]: https://azure.microsoft.com/documentation/services/data-factory/
[Hadoop]: https://azure.microsoft.com/documentation/services/hdinsight/

[Senden von C2D-Nachrichten mit IoT Hub]: iot-hub-csharp-csharp-c2d.md
[Senden von Cloud-zu-Gerät-Nachrichten mit IoT Hub]: iot-hub-csharp-csharp-c2d.md
[Senden von Nachrichten von Cloud-zu-Gerät mit IoT Hub]: iot-hub-csharp-csharp-c2d.md
[Verarbeiten von Gerät-zu-Cloud-Nachrichten]: iot-hub-csharp-csharp-process-d2c.md
[Erste Schritte mit IoT Hub]: iot-hub-csharp-csharp-getstarted.md
[Azure IoT Developer Center]: http://www.azure.com/develop/iot

[Behandeln vorübergehender Fehler]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx
[Azure Storage]: ../storage/storage-create-storage-account.md#create-a-storage-account
[Verwalten von IoT Hubs über das Azure-Portal]: iot-hub-manage-through-portal.md#file-upload
[NuGet-Paket mit dem SDK des Azure IoT-Diensts]: https://www.nuget.org/packages/Microsoft.Azure.Devices/
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/

[lnk-create-hub]: iot-hub-rm-template-powershell.md
[lnk-c-sdk]: iot-hub-device-sdk-c-intro.md
[lnk-sdks]: iot-hub-sdks-summary.md

[lnk-design]: iot-hub-guidance.md
[lnk-dmui]: iot-hub-device-management-ui-sample.md
[lnk-gateway]: iot-hub-linux-gateway-sdk-simulated-device.md
[lnk-portal]: iot-hub-manage-through-portal.md

<!---HONumber=AcomDC_0914_2016-->