---
title: "Erste Schritte mit Azure IoT Hub für C# | Microsoft Docs"
description: "Tutorial für den Einstieg in Azure IoT Hub mit C#. Verwenden Sie Azure IoT Hub und C# mit den Microsoft Azure IoT SDKs, um eine IoT-Lösung zu implementieren."
services: iot-hub
documentationcenter: .net
author: dominicbetts
manager: timlt
editor: 
ms.assetid: f40604ff-8fd6-4969-9e99-8574fbcf036c
ms.service: iot-hub
ms.devlang: dotnet
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/12/2016
ms.author: dobett
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: 97c5d3c3e78ce6ec23f260bd26a0b41dc7c7197a


---
# <a name="get-started-with-azure-iot-hub-for-net"></a>Erste Schritte mit Azure IoT Hub für .NET
[!INCLUDE [iot-hub-selector-get-started](../../includes/iot-hub-selector-get-started.md)]

Am Ende dieses Tutorials verfügen Sie über drei Windows-Konsolenanwendungen:

* **CreateDeviceIdentity**: Hiermit werden eine Geräteidentität und ein zugehöriger Sicherheitsschlüssel zum Verbinden mit Ihrem simulierten Gerät erstellt.
* **ReadDeviceToCloudMessages**: Hiermit wird die Telemetrie angezeigt, die Ihr simuliertes Gerät sendet.
* **SimulatedDevice**: Hiermit wird mithilfe der zuvor erstellten Geräteidentität eine Verbindung mit Ihrem IoT Hub hergestellt und jede Sekunde unter Verwendung des AMQP-Protokolls eine Telemetrienachricht gesendet.

> [!NOTE]
> Unter [IoT Hub-SDKs][lnk-hub-sdks] finden Sie Informationen zu den verschiedenen SDKs, mit denen Sie sowohl Anwendungen für Geräte als auch das zugehörige Lösungs-Back-End erstellen können.
> 
> 

Für dieses Lernprogramm benötigen Sie Folgendes:

* Microsoft Visual Studio 2015.
* Ein aktives Azure-Konto. (Wenn Sie noch kein Konto haben, können Sie in nur wenigen Minuten ein [kostenloses Testkonto][lnk-free-trial] erstellen.)

[!INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

Sie haben nun Ihren IoT Hub erstellt und verfügen über den Hostnamen und die Verbindungszeichenfolge, die Sie für die weiteren Schritte in diesem Tutorial benötigen.

## <a name="create-a-device-identity"></a>Erstellen einer Geräteidentität
In diesem Abschnitt erstellen Sie eine Windows-Konsolen-App, die eine Geräteidentität in der Identitätsregistrierung Ihres IoT Hubs erstellt. Ein Gerät kann nur eine Verbindung mit dem IoT Hub herstellen, wenn in der Geräteidentitätsregistrierung ein Eintrag für dieses Gerät vorhanden ist. Weitere Informationen finden Sie im Abschnitt „Geräteidentitätsregistrierung“ des [Entwicklungsleitfadens für IoT Hub][lnk-devguide-identity]. Beim Ausführen dieser Konsolen-App werden eine eindeutige Geräte-ID und ein Schlüssel erstellt, mit denen sich das Gerät beim Senden von D2C-Nachrichten (Device-to-Cloud, Gerät-an-Cloud) beim IoT Hub identifizieren kann.

1. Fügen Sie in Visual Studio in der aktuellen Projektmappe mithilfe der Projektvorlage **Konsolenanwendung** ein Visual C#-Projekt für den klassischen Windows-Desktop hinzu. Stellen Sie sicher, dass .NET-Framework-Version 4.5.1 oder höher verwendet wird. Nennen Sie das Projekt **CreateDeviceIdentity**.
   
    ![Neues Visual C#-Projekt für den klassischen Windows-Desktop][10]
2. Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste auf das Projekt **CreateDeviceIdentity**, und klicken Sie dann auf **NuGet-Pakete verwalten**.
3. Wählen Sie im Fenster **NuGet-Paket-Manager** die Option **Durchsuchen**, suchen Sie nach **microsoft.azure.devices**, wählen Sie zum Installieren des Pakets **Microsoft.Azure.Devices** die Option **Installieren**, und akzeptieren Sie die Nutzungsbedingungen. Bei diesem Verfahren werden das NuGet-Paket für das [Dienst-SDK für Microsoft Azure IoT][lnk-nuget-service-sdk] sowie die dazugehörigen Abhängigkeiten heruntergeladen, installiert und mit einem entsprechenden Verweis versehen.
   
    ![Fenster „NuGet-Paket-Manager“][11]
4. Fügen Sie am Anfang der Datei **Program.cs** die folgenden `using`-Anweisungen hinzu:
   
        using Microsoft.Azure.Devices;
        using Microsoft.Azure.Devices.Common.Exceptions;
5. Fügen Sie der **Program** -Klasse die folgenden Felder hinzu. Ersetzen Sie den Platzhalterwert durch die Verbindungszeichenfolge für den IoT Hub, den Sie im vorherigen Abschnitt erstellt haben.
   
        static RegistryManager registryManager;
        static string connectionString = "{iot hub connection string}";
6. Fügen Sie der **Program** -Klasse die folgende Methode hinzu:
   
        private static async Task AddDeviceAsync()
        {
            string deviceId = "myFirstDevice";
            Device device;
            try
            {
                device = await registryManager.AddDeviceAsync(new Device(deviceId));
            }
            catch (DeviceAlreadyExistsException)
            {
                device = await registryManager.GetDeviceAsync(deviceId);
            }
            Console.WriteLine("Generated device key: {0}", device.Authentication.SymmetricKey.PrimaryKey);
        }
   
    Mit dieser Methode wird eine Geräteidentität mit der ID **myFirstDevice** erstellt. (Wenn diese Geräte-ID in der Registrierung bereits vorhanden ist, werden mit dem Code lediglich die vorhandenen Geräteinformationen abgerufen.) Anschließend zeigt die App den Primärschlüssel für diese Identität an. Sie verwenden diesen Schlüssel im simulierten Gerät, um eine Verbindung mit dem IoT Hub herzustellen.
7. Fügen Sie abschließend der **Main** -Methode die folgenden Zeilen hinzu:
   
        registryManager = RegistryManager.CreateFromConnectionString(connectionString);
        AddDeviceAsync().Wait();
        Console.ReadLine();
8. Führen Sie diese Anwendung aus, und notieren Sie den Geräteschlüssel.
   
    ![Von der Anwendung generierter Geräteschlüssel][12]

> [!NOTE]
> Die Identitätsregistrierung im IoT Hub speichert nur Geräteidentitäten, um einen sicheren Zugriff auf den Hub zu ermöglichen. Sie speichert Geräte-IDs und Schlüssel, die als Sicherheitsanmeldeinformationen verwendet werden, sowie ein Aktiviert/Deaktiviert-Kennzeichen, mit dem Sie den Zugriff für ein einzelnes Gerät deaktivieren können. Wenn Ihre Anwendung das Speichern weiterer gerätespezifischer Metadaten erfordert, sollte dafür ein anwendungsspezifischer Speicher verwendet werden. Weitere Informationen finden Sie im [IoT Hub-Entwicklerhandbuch][lnk-devguide-identity].
> 
> 

## <a name="receive-devicetocloud-messages"></a>Empfangen von Gerät-an-Cloud-Nachrichten
In diesem Abschnitt erstellen Sie eine Windows-Konsolen-App, die D2C-Nachrichten (Device-to-Cloud) aus dem IoT Hub liest. Ein IoT Hub macht einen [Azure Event Hubs][lnk-event-hubs-overview]-kompatiblen Endpunkt verfügbar, der Ihnen das Lesen von D2C-Nachrichten ermöglicht. Zur Vereinfachung wird in diesem Tutorial ein einfacher Reader erstellt, der für eine Bereitstellung mit hohem Durchsatz nicht geeignet ist. Im Tutorial [Verarbeiten von D2C-Nachrichten mit IoT Hub][lnk-process-d2c-tutorial] wird gezeigt, wie Sie D2C-Nachrichten bedarfsorientiert verarbeiten. Weitere Informationen zum Verarbeiten von Nachrichten von Event Hubs finden Sie im Tutorial [Erste Schritte mit Event Hubs][lnk-eventhubs-tutorial]. (Dieses Tutorial gilt für die mit IoT Hub Event Hubs kompatiblen Endpunkte.)

> [!NOTE]
> Der mit Event Hubs kompatible Endpunkt zum Lesen von D2C-Nachrichten verwendet immer das AMQP-Protokoll.
> 
> 

1. Fügen Sie in Visual Studio in der aktuellen Projektmappe mithilfe der Projektvorlage **Konsolenanwendung** ein neues Visual C#-Projekt für den klassischen Windows-Desktop hinzu. Stellen Sie sicher, dass .NET-Framework-Version 4.5.1 oder höher verwendet wird. Nennen Sie das Projekt **ReadDeviceToCloudMessages**.
   
    ![Neues Visual C#-Projekt für den klassischen Windows-Desktop][10]
2. Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste auf das Projekt **ReadDeviceToCloudMessages**, und klicken Sie dann auf **NuGet-Pakete verwalten**.
3. Suchen Sie im Fenster **NuGet-Paket-Manager** nach **WindowsAzure.ServiceBus**, wählen Sie die Option **Installieren**, und akzeptieren Sie die Nutzungsbedingungen. Bei diesem Verfahren wird [Azure Service Bus][lnk-servicebus-nuget] mit allen Abhängigkeiten heruntergeladen, installiert und mit einem entsprechenden Verweis versehen. Dieses Paket ermöglicht der Anwendung das Herstellen einer Verbindung mit dem Event Hub-kompatiblen Endpunkt des IoT Hubs.
4. Fügen Sie am Anfang der Datei **Program.cs** die folgenden `using`-Anweisungen hinzu:
   
        using Microsoft.ServiceBus.Messaging;
        using System.Threading;
5. Fügen Sie der **Program** -Klasse die folgenden Felder hinzu. Ersetzen Sie den Platzhalterwert durch die Verbindungszeichenfolge für den IoT Hub, den Sie im Abschnitt „Erstellen eines IoT Hubs“ erstellt haben.
   
        static string connectionString = "{iothub connection string}";
        static string iotHubD2cEndpoint = "messages/events";
        static EventHubClient eventHubClient;
6. Fügen Sie der **Program** -Klasse die folgende Methode hinzu:
   
        private static async Task ReceiveMessagesFromDeviceAsync(string partition, CancellationToken ct)
        {
            var eventHubReceiver = eventHubClient.GetDefaultConsumerGroup().CreateReceiver(partition, DateTime.UtcNow);
            while (true)
            {
                if (ct.IsCancellationRequested) break;
                EventData eventData = await eventHubReceiver.ReceiveAsync();
                if (eventData == null) continue;
   
                string data = Encoding.UTF8.GetString(eventData.GetBytes());
                Console.WriteLine("Message received. Partition: {0} Data: '{1}'", partition, data);
            }
        }
   
    Diese Methode verwendet eine **EventHubReceiver** -Instanz, um Nachrichten von allen D2C-Empfangspartitionen des IoT Hubs zu empfangen. Beachten Sie, wie beim Erstellen des **EventHubReceiver**-Objekts ein `DateTime.Now`-Parameter übergeben wird, damit nur die Nachrichten empfangen werden, die nach seinem Start gesendet wurden. Dieser Filter ist in einer Testumgebung hilfreich, damit Sie die aktuelle Gruppe der Nachrichten anzeigen können. In einer Produktionsumgebung sollte mit Ihrem Code sichergestellt werden, dass alle Nachrichten verarbeitet werden. Weitere Informationen finden Sie im Tutorial [Verarbeiten von D2C-Nachrichten mit IoT Hub][lnk-process-d2c-tutorial].
7. Fügen Sie abschließend der **Main** -Methode die folgenden Zeilen hinzu:
   
        Console.WriteLine("Receive messages. Ctrl-C to exit.\n");
        eventHubClient = EventHubClient.CreateFromConnectionString(connectionString, iotHubD2cEndpoint);
   
        var d2cPartitions = eventHubClient.GetRuntimeInformation().PartitionIds;
   
        CancellationTokenSource cts = new CancellationTokenSource();
   
        System.Console.CancelKeyPress += (s, e) =>
        {
          e.Cancel = true;
          cts.Cancel();
          Console.WriteLine("Exiting...");
        };
   
        var tasks = new List<Task>();
        foreach (string partition in d2cPartitions)
        {
           tasks.Add(ReceiveMessagesFromDeviceAsync(partition, cts.Token));
        }  
        Task.WaitAll(tasks.ToArray());

## <a name="create-a-simulated-device-app"></a>Erstellen einer simulierten Geräte-App
In diesem Abschnitt erstellen Sie eine Windows-Konsolenanwendung, die ein Gerät simuliert, das D2C-Nachrichten (Device-to-Cloud) an einen IoT Hub sendet.

1. Fügen Sie in Visual Studio in der aktuellen Projektmappe mithilfe der Projektvorlage **Konsolenanwendung** ein neues Visual C#-Projekt für den klassischen Windows-Desktop hinzu. Stellen Sie sicher, dass .NET-Framework-Version 4.5.1 oder höher verwendet wird. Nennen Sie das Projekt **SimulatedDevice**.
   
    ![Neues Visual C#-Projekt für den klassischen Windows-Desktop][10]
2. Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste auf das Projekt **SimulatedDevice**, und klicken Sie dann auf **NuGet-Pakete verwalten**.
3. Wählen Sie im Fenster **NuGet-Paket-Manager** die Option **Durchsuchen**, suchen Sie nach **Microsoft.Azure.Devices.Client**, wählen Sie zum Installieren des Pakets **Microsoft.Azure.Devices** die Option **Installieren**, und akzeptieren Sie die Nutzungsbedingungen. Bei diesem Verfahren wird das [NuGet-Paket mit dem Geräte-SDK für Azure IoT][lnk-device-nuget] heruntergeladen und installiert und ein Verweis auf das Paket und seine Abhängigkeiten hinzugefügt.
4. Fügen Sie am Anfang der Datei **Program.cs** die folgende `using`-Anweisung hinzu:
   
        using Microsoft.Azure.Devices.Client;
        using Newtonsoft.Json;
5. Fügen Sie der **Program** -Klasse die folgenden Felder hinzu. Ersetzen Sie die Platzhalterwerte durch den IoT Hub-Hostnamen, den Sie im Abschnitt „Erstellen eines IoT Hubs“ abgerufen haben, und durch den Geräteschlüssel, den Sie im Abschnitt „Erstellen einer Geräteidentität“ abgerufen haben.
   
        static DeviceClient deviceClient;
        static string iotHubUri = "{iot hub hostname}";
        static string deviceKey = "{device key}";
6. Fügen Sie der **Program** -Klasse die folgende Methode hinzu:
   
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
7. Fügen Sie abschließend der **Main** -Methode die folgenden Zeilen hinzu:
   
        Console.WriteLine("Simulated device\n");
        deviceClient = DeviceClient.Create(iotHubUri, new DeviceAuthenticationWithRegistrySymmetricKey("myFirstDevice", deviceKey));
   
        SendDeviceToCloudMessagesAsync();
        Console.ReadLine();
   
   In der Standardeinstellung erstellt die **Create**-Methode eine **DeviceClient**-Instanz, die das AMQP-Protokoll für die Kommunikation mit dem IoT Hub verwendet. Um das HTTP-Protokoll zu verwenden, nutzen Sie die Überschreibung der **Create**-Methode, bei der Sie das Protokoll angeben können. Wenn Sie das HTTP-Protokoll verwenden, müssen Sie Ihrem Projekt auch das NuGet-Paket **Microsoft.AspNet.WebApi.Client** hinzufügen, um den **System.Net.Http.Formatting**-Namespace einzuschließen.

Dieses Tutorial führt Sie durch die Schritte zum Erstellen eines IoT Hub-Geräteclients. Sie können auch die Visual Studio-Erweiterung für den [verbundenen Dienst für Azure IoT Hub][lnk-connected-service] verwenden, um Ihrer Geräteclientanwendung den erforderlichen Code hinzuzufügen.

> [!NOTE]
> Der Einfachheit halber wird in diesem Tutorial keine Wiederholungsrichtlinie implementiert. Im Produktionscode sollten Sie Wiederholungsrichtlinien implementieren (z.B. einen exponentiellen Backoff), wie im MSDN-Artikel zum [Behandeln vorübergehender Fehler][lnk-transient-faults] beschrieben.
> 
> 

## <a name="run-the-applications"></a>Ausführen der Anwendungen
Sie können nun die Anwendungen ausführen.

1. Klicken Sie in Visual Studio im Projektmappen-Explorer mit der rechten Maustaste auf Ihre Projektmappe, und klicken Sie dann auf **Startprojekte festlegen**. Wählen Sie **Mehrere Startprojekte**, und wählen Sie dann für die Projekte **ReadDeviceToCloudMessages** und **SimulatedDevice** als Aktion die Option **Starten** aus.
   
    ![Startprojekteigenschaften][41]
2. Drücken Sie **F5** , um die Ausführung der beiden Apps zu starten. Die Konsolenausgabe der **SimulatedDevice** -App zeigt die Nachrichten an, die vom simulierten Gerät an Ihren IoT Hub gesendet werden. Die Konsolenausgabe der **ReadDeviceToCloudMessages** -App zeigt die Nachrichten an, die vom IoT Hub empfangen werden.
   
    ![Konsolenausgabe von Apps][42]
3. Über die Kachel **Nutzung** im [Azure-Portal][lnk-portal] wird die Anzahl der an den Hub gesendeten Nachrichten angezeigt:
   
    ![Kachel „Verwendung“ im Azure-Portal][43]

## <a name="next-steps"></a>Nächste Schritte
In diesem Tutorial haben Sie im Azure-Portal einen IoT Hub konfiguriert und anschließend in der Identitätsregistrierung des Hubs eine Geräteidentität erstellt. Sie haben diese Geräteidentität verwendet, um die SimulatedDevice-App für das Senden von D2C-Nachrichten zu aktivieren. Sie haben außerdem eine App erstellt, mit der die vom Hub empfangenen Nachrichten angezeigt werden. 

Informationen zu den weiteren ersten Schritten mit IoT Hub und zum Kennenlernen anderer IoT-Szenarien finden Sie in den folgenden Artikeln:

* [Verbinden des Geräts][lnk-connect-device]
* [Erste Schritte mit der Geräteverwaltung][lnk-device-management]
* [Erste Schritte mit dem IoT Gateway-SDK][lnk-gateway-SDK]

Informationen dazu, wie Sie Ihre IoT-Lösung erweitern und eine Verarbeitung von D2C-Nachrichten mit guter Skalierbarkeit erzielen, finden Sie im Tutorial [Verarbeiten von Gerät-zu-Cloud-Nachrichten][lnk-process-d2c-tutorial].

<!-- Images. -->
[41]: ./media/iot-hub-csharp-csharp-getstarted/run-apps1.png
[42]: ./media/iot-hub-csharp-csharp-getstarted/run-apps2.png
[43]: ./media/iot-hub-csharp-csharp-getstarted/usage.png
[10]: ./media/iot-hub-csharp-csharp-getstarted/create-identity-csharp1.png
[11]: ./media/iot-hub-csharp-csharp-getstarted/create-identity-csharp2.png
[12]: ./media/iot-hub-csharp-csharp-getstarted/create-identity-csharp3.png

<!-- Links -->
[lnk-process-d2c-tutorial]: iot-hub-csharp-csharp-process-d2c.md

[lnk-hub-sdks]: iot-hub-devguide-sdks.md
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-portal]: https://portal.azure.com/

[lnk-eventhubs-tutorial]: ../event-hubs/event-hubs-csharp-ephcs-getstarted.md
[lnk-devguide-identity]: iot-hub-devguide-identity-registry.md
[lnk-servicebus-nuget]: https://www.nuget.org/packages/WindowsAzure.ServiceBus
[lnk-event-hubs-overview]: ../event-hubs/event-hubs-overview.md

[lnk-nuget-service-sdk]: https://www.nuget.org/packages/Microsoft.Azure.Devices/
[lnk-device-nuget]: https://www.nuget.org/packages/Microsoft.Azure.Devices.Client/
[lnk-transient-faults]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx
[lnk-connected-service]: https://visualstudiogallery.msdn.microsoft.com/e254a3a5-d72e-488e-9bd3-8fee8e0cd1d6
[lnk-device-management]: iot-hub-device-management-get-started.md
[lnk-gateway-SDK]: iot-hub-linux-gateway-sdk-get-started.md
[lnk-connect-device]: https://azure.microsoft.com/develop/iot/



<!--HONumber=Nov16_HO2-->


