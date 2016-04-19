## Erstellen einer simulierten Geräte-App

In diesem Abschnitt erstellen Sie eine Windows-Konsolenanwendung, die ein Gerät simuliert, das D2C-Nachrichten (Device-to-Cloud, Gerät-an-Cloud) an einen IoT Hub sendet.

1. Fügen Sie in Visual Studio in der aktuellen Projektmappe mithilfe der Projektvorlage **Konsolenanwendung** ein neues Visual C#-Projekt für den klassischen Windows-Desktop hinzu. Stellen Sie sicher, dass mindestens .NET-Framework-Version 4.5.1 verwendet wird. Nennen Sie das Projekt **SimulatedDevice**.

   	![][30]

2. Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste auf das Projekt **SimulatedDevice**, und klicken Sie dann auf **NuGet-Pakete verwalten**.

3. Wählen Sie im Fenster **NuGet-Paket-Manager** die Option **Durchsuchen**, suchen Sie nach **Microsoft.Azure.Devices.Client**, klicken Sie zum Installieren des Pakets **Microsoft.Azure.Devices** auf **Installieren**, und akzeptieren Sie die Nutzungsbedingungen.

	Daraufhin wird das [NuGet-Paket mit dem Geräte-SDK für Azure IoT][lnk-device-nuget] heruntergeladen und installiert und ein Verweis auf das Paket und seine Abhängigkeiten hinzugefügt.

4. Fügen Sie am Anfang der Datei **Program.cs** die folgenden `using`-Anweisungen hinzu:

		using Microsoft.Azure.Devices.Client;
        using Newtonsoft.Json;


5. Fügen Sie der **Program**-Klasse die folgenden Felder hinzu, und ersetzen Sie dabei die Platzhalterwerte durch den IoT-Hostnamen, den Sie im Abschnitt *Erstellen eines IoT Hubs* abgerufen haben, und durch den Geräteschlüssel, den Sie im Abschnitt *Erstellen einer Geräteidentität* abgerufen haben:

		static DeviceClient deviceClient;
        static string iotHubUri = "{iot hub hostname}";
        static string deviceKey = "{device key}";

6. Fügen Sie der **Program**-Klasse die folgende Methode hinzu:

		private static async void SendDeviceToCloudMessagesAsync()
        {
            double avgWindSpeed = 10; // m/s
            Random rand = new Random();

            while (true)
            {
                double currentWindSpeed = avgWindSpeed + rand.NextDouble() * 4 - 2;

                var telemetryDataPoint = new
                {
                    deviceId = "myFirstDevice",
                    windSpeed = currentWindSpeed
                };
                var messageString = JsonConvert.SerializeObject(telemetryDataPoint);
                var message = new Message(Encoding.ASCII.GetBytes(messageString));

                await deviceClient.SendEventAsync(message);
                Console.WriteLine("{0} > Sending message: {1}", DateTime.Now, messageString);

                Task.Delay(1000).Wait();
            }
        }

	Diese Methode sendet jede Sekunde eine neue D2C-Nachricht. Die Nachricht enthält ein JSON-serialisiertes Objekt mit der Geräte-ID sowie eine zufällig generierte Zahl, um einen Windgeschwindigkeitssensor zu simulieren.

7. Fügen Sie abschließend der **Main**-Methode die folgenden Zeilen hinzu:

        Console.WriteLine("Simulated device\n");
        deviceClient = DeviceClient.Create(iotHubUri, new DeviceAuthenticationWithRegistrySymmetricKey("myFirstDevice", deviceKey));

        SendDeviceToCloudMessagesAsync();
        Console.ReadLine();

  In der Standardeinstellung erstellt die **Create**-Methode eine **DeviceClient**-Instanz, die das AMQP-Protokoll für die Kommunikation mit dem IoT Hub verwendet. Um das HTTPS-Protokoll zu verwenden, nutzen Sie die Überschreibung der **Create**-Methode, bei der Sie das Protokoll angeben können. Wenn Sie das HTTPS-Protokoll verwenden möchten, müssen Sie Ihrem Projekt auch das NuGet-Paket **Microsoft.AspNet.WebApi.Client** hinzufügen, um den **System.Net.Http.Formatting**-Namespace einzuschließen.

Dieses Tutorial führt Sie durch die Schritte zum Erstellen eines IoT Hub-Geräteclients. Als Alternative können Sie die Visual Studio-Erweiterung für den [verbundenen Dienst für Azure IoT Hub][lnk-connected-service] verwenden, um den erforderlichen Code zu Ihrer Geräteclientanwendung hinzuzufügen.


> [AZURE.NOTE] Der Einfachheit halber wird in diesem Tutorial keine Wiederholungsrichtlinie implementiert. Im Produktionscode sollten Sie Wiederholungsrichtlinien implementieren (z. B. einen exponentiellen Backoff), wie im MSDN-Artikel zum [Behandeln vorübergehender Fehler][lnk-transient-faults] beschrieben.

<!-- Links -->

[lnk-device-nuget]: https://www.nuget.org/packages/Microsoft.Azure.Devices.Client/
[lnk-transient-faults]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx
[lnk-connected-service]: https://visualstudiogallery.msdn.microsoft.com/e254a3a5-d72e-488e-9bd3-8fee8e0cd1d6

<!-- Images -->
[30]: ./media/iot-hub-getstarted-device-csharp/create-identity-csharp1.png

<!---HONumber=AcomDC_0413_2016-->