## Zuweisen eines Azure Storage-Kontos zu IoT Hub

Da vom simulierten Gerät eine Datei in ein Azure Storage-Blob hochgeladen wird, müssen Sie über ein Azure-Speicherkonto verfügen, das IoT Hub zugeordnet ist. Sie können ein vorhandenes Speicherkonto verwenden oder mithilfe der Anweisungen unter [Informationen zu Azure Storage] ein neues Konto erstellen. Sie können IoT Hub ein Azure Storage-Konto zuordnen, indem Sie die Anweisungen unter [Verwalten von IoT Hubs über das Azure-Portal] befolgen.

## Hochladen einer Datei von einem simulierten Gerät

In diesem Abschnitt ändern Sie die simulierte Geräteanwendung, die Sie in [Senden von Cloud-zu-Gerät-Nachrichten mit IoT Hub] erstellt haben, um Cloud-zu-Gerät-Nachrichten von IoT Hub zu empfangen.

1. Klicken Sie in Visual Studio mit der rechten Maustaste auf das Projekt **SimulatedDevice**. Klicken Sie auf **Hinzufügen** und dann auf **Vorhandenes Element**. Navigieren Sie zu einer Bilddatei, und fügen Sie sie Ihrem Projekt hinzu. In diesem Tutorial wird vorausgesetzt, dass das Bild `image.jpg` heißt.

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

    Die `UploadToBlobAsync`-Methode verwendet den Dateinamen und die Datenstromquelle der hochzuladenden Datei und verarbeitet das Hochladen in Speicher. Die Konsolenanwendung zeigt den Zeitaufwand für das Hochladen der Datei an.

5. Fügen Sie in der **Main**-Methode unmittelbar vor der Zeile `Console.ReadLine()` die folgende Methode hinzu:

        SendToBlobAsync();

> [AZURE.NOTE] Der Einfachheit halber wird in diesem Lernprogramm keine Wiederholungsrichtlinie implementiert. Im Produktionscode sollten Sie Wiederholungsrichtlinien implementieren (z.B. einen exponentiellen Backoff), wie im MSDN-Artikel zum [Behandeln vorübergehender Fehler] beschrieben.

<!-- Links -->
[IoT Hub Developer Guide - C2D]: iot-hub-devguide.md#c2d
[Behandeln vorübergehender Fehler]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx
[Informationen zu Azure Storage]: ../storage/storage-create-storage-account.md#create-a-storage-account
[Verwalten von IoT Hubs über das Azure-Portal]: ../articles/iot-hub-manage-through-portal/#file-upload

<!-- Images -->
[1]: ../articles/iot-hub/media/iot-hub-csharp-csharp-file-upload/image-properties.png

<!---HONumber=AcomDC_0622_2016-->