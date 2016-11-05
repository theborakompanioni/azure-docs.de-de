## Erhalten einer Benachrichtigung zum Dateiupload
In diesem Abschnitt schreiben Sie eine Windows-Konsolen-App, die Uploadbenachrichtigungen von IoT Hub empfängt.

1. Erstellen Sie in der aktuellen Visual Studio-Projektmappe mithilfe der Projektvorlage **Konsolenanwendung** ein neues Visual C#-Windows-Projekt. Nennen Sie das Projekt **ReadFileUploadNotification**.
   
    ![Neues Projekt in Visual Studio][2]
2. Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste auf das Projekt **ReadFileUploadNotification**, und klicken Sie dann auf **NuGet-Pakete verwalten**.
   
    Daraufhin wird das Fenster "NuGet-Pakete verwalten" angezeigt.
3. Suchen Sie nach `Microsoft.Azure.Devices`, klicken Sie auf **Installieren**, und akzeptieren Sie die Nutzungsbedingungen.
   
    Daraufhin wird das [NuGet-Paket mit dem SDK des Azure IoT-Diensts] heruntergeladen und installiert. Dem Projekt **ReadFileUploadNotification** wird ein Verweis hinzugefügt.
4. Fügen Sie zu Beginn der Datei **Program.cs** die folgenden Anweisungen hinzu:
   
        using Microsoft.Azure.Devices;
5. Fügen Sie der **Program**-Klasse die folgenden Felder hinzu. Ersetzen Sie den Platzhalterwert durch die IoT Hub-Verbindungszeichenfolge aus [Erste Schritte mit IoT Hub]\:
   
        static ServiceClient serviceClient;
        static string connectionString = "{iot hub connection string}";
6. Fügen Sie der **Program**-Klasse die folgende Methode hinzu:
   
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
7. Fügen Sie abschließend der **Main**-Methode die folgenden Zeilen hinzu:
   
        Console.WriteLine("Receive file upload notifications\n");
        serviceClient = ServiceClient.CreateFromConnectionString(connectionString);
        ReceiveFileUploadNotificationAsync().Wait();
        Console.ReadLine();

<!-- Links -->

[IoT Hub Developer Guide - C2D]: ../articles/iot-hub/iot-hub-devguide.md#c2d
[NuGet-Paket mit dem SDK des Azure IoT-Diensts]: https://www.nuget.org/packages/Microsoft.Azure.Devices/
[Transient Fault Handling]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx
[Erste Schritte mit IoT Hub]: ../articles/iot-hub/iot-hub-csharp-csharp-getstarted.md

<!-- Images -->
[2]: ./media/iot-hub-c2d-cloud-csharp/create-identity-csharp1.png

<!---HONumber=AcomDC_0622_2016-->