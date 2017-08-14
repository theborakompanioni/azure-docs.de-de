---
title: Erste Schritte mit Azure IoT Hub (.NET) | Microsoft-Dokumentation
description: "Hier erfahren Sie, wie Sie mithilfe der IoT-SDKs für .NET D2C-Nachrichten an Azure IoT Hub senden. Erstellen Sie simulierte Geräte- und Dienst-Apps zum Registrieren Ihres Geräts, zum Senden von Nachrichten und zum Lesen von Nachrichten von IoT Hub."
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
ms.date: 08/08/2017
ms.author: dobett
ms.custom: H1Hack27Feb2017
ms.translationtype: HT
ms.sourcegitcommit: f9003c65d1818952c6a019f81080d595791f63bf
ms.openlocfilehash: 69296eb9ac2a74a97b632d27733a6a06500b4abd
ms.contentlocale: de-de
ms.lasthandoff: 08/09/2017

---
# <a name="connect-your-device-to-your-iot-hub-using-net"></a>Herstellen einer Verbindung zwischen Ihrem Gerät und Ihrem IoT Hub mit .NET

[!INCLUDE [iot-hub-selector-get-started](../../includes/iot-hub-selector-get-started.md)]

Am Ende dieses Tutorials verfügen Sie über drei .NET-Konsolen-Apps:

* **CreateDeviceIdentity**: Hiermit werden eine Geräteidentität und ein zugehöriger Sicherheitsschlüssel zum Verbinden mit Ihrer Geräte-App erstellt.
* **ReadDeviceToCloudMessages**: Hiermit wird die Telemetrie angezeigt, die Ihre Geräte-App sendet.
* **SimulatedDevice**: Hiermit wird mithilfe der zuvor erstellten Geräteidentität eine Verbindung mit Ihrem IoT Hub hergestellt und jede Sekunde unter Verwendung des MQTT-Protokolls eine Telemetrienachricht gesendet.

Sie können die Visual Studio-Projektmappe mit den drei Apps von Github herunterladen und klonen.

```bash
git clone https://github.com/Azure-Samples/iot-hub-dotnet-simulated-device-client-app.git
```

> [!NOTE]
> Informationen zu den verschiedenen Azure IoT SDKs, mit denen Sie sowohl Anwendungen für Geräte als auch das zugehörige Lösungs-Back-End erstellen können, finden Sie unter [Azure IoT SDKs][lnk-hub-sdks].

Für dieses Tutorial benötigen Sie Folgendes:

* Visual Studio 2015 oder Visual Studio 2017
* Ein aktives Azure-Konto. (Wenn Sie über kein Konto verfügen, können Sie in nur wenigen Minuten ein [kostenloses Konto][lnk-free-trial] erstellen.)

[!INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

Sie haben nun Ihren IoT-Hub erstellt und verfügen über den Hostnamen und die IoT Hub-Verbindungszeichenfolge, die Sie für die weiteren Schritte in diesem Tutorial benötigen.

<a id="DeviceIdentity_csharp"></a>
[!INCLUDE [iot-hub-get-started-create-device-identity-csharp](../../includes/iot-hub-get-started-create-device-identity-csharp.md)]

<a id="D2C_csharp"></a>
## <a name="receive-device-to-cloud-messages"></a>Empfangen von Gerät-an-Cloud-Nachrichten
In diesem Abschnitt erstellen Sie eine .NET-Konsolen-App, die D2C-Nachrichten (Device-to-Cloud) aus IoT Hub liest. Ein IoT-Hub macht einen mit [Azure Event Hubs][lnk-event-hubs-overview] kompatiblen Endpunkt verfügbar, der Ihnen das Lesen von D2C-Nachrichten ermöglicht. Zur Vereinfachung wird in diesem Tutorial ein einfacher Reader erstellt, der für eine Bereitstellung mit hohem Durchsatz nicht geeignet ist. Informationen dazu, wie Sie eine Verarbeitung von D2C-Nachrichten mit Skalierbarkeit erzielen, finden Sie im Tutorial [Verarbeiten von D2C-Nachrichten][lnk-process-d2c-tutorial]. Weitere Informationen zum Verarbeiten von Nachrichten von Event Hubs finden Sie im Tutorial [Erste Schritte mit Event Hubs][lnk-eventhubs-tutorial]. (Dieses Tutorial gilt für die mit IoT Hub Event Hubs kompatiblen Endpunkte.)

> [!NOTE]
> Der mit Event Hubs kompatible Endpunkt zum Lesen von D2C-Nachrichten verwendet immer das AMQP-Protokoll.

1. Fügen Sie in Visual Studio in der aktuellen Projektmappe mithilfe der Projektvorlage **Konsolenanwendung (.NET Framework** ein Visual C#-Projekt für den klassischen Windows-Desktop hinzu. Stellen Sie sicher, dass .NET-Framework-Version 4.5.1 oder höher verwendet wird. Nennen Sie das Projekt **ReadDeviceToCloudMessages**.

    ![Neues Visual C#-Projekt für den klassischen Windows-Desktop][10a]

2. Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste auf das Projekt **ReadDeviceToCloudMessages**, und klicken Sie dann auf **NuGet-Pakete verwalten**.

3. Suchen Sie im Fenster **NuGet-Paket-Manager** nach **WindowsAzure.ServiceBus**, wählen Sie die Option **Installieren** aus, und akzeptieren Sie die Nutzungsbedingungen. Bei diesem Verfahren wird [Azure Service Bus][lnk-servicebus-nuget] mit allen Abhängigkeiten heruntergeladen, installiert und mit einem Verweis versehen. Dieses Paket ermöglicht der Anwendung das Herstellen einer Verbindung mit dem Event Hub-kompatiblen Endpunkt des IoT Hubs.

4. Fügen Sie am Anfang der Datei **Program.cs** die folgenden `using`-Anweisungen hinzu:

    ```csharp
    using Microsoft.ServiceBus.Messaging;
    using System.Threading;
    ```

5. Fügen Sie der **Program** -Klasse die folgenden Felder hinzu. Ersetzen Sie den Platzhalterwert durch die IoT Hub-Verbindungszeichenfolge für den Hub, den Sie im Abschnitt zur Erstellung eines IoT-Hubs erstellt haben.

    ```csharp
    static string connectionString = "{iothub connection string}";
    static string iotHubD2cEndpoint = "messages/events";
    static EventHubClient eventHubClient;
    ```

6. Fügen Sie der **Program** -Klasse die folgende Methode hinzu:

    ```csharp
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
    ```

    Diese Methode verwendet eine **EventHubReceiver** -Instanz, um Nachrichten von allen D2C-Empfangspartitionen des IoT Hubs zu empfangen. Beachten Sie, wie beim Erstellen des **EventHubReceiver**-Objekts ein `DateTime.Now`-Parameter übergeben wird, damit nur die Nachrichten empfangen werden, die nach seinem Start gesendet wurden. Dieser Filter ist in einer Testumgebung hilfreich, damit Sie die aktuelle Gruppe der Nachrichten anzeigen können. In einer Produktionsumgebung sollte mit Ihrem Code sichergestellt werden, dass alle Nachrichten verarbeitet werden. Weitere Informationen finden Sie im Tutorial [Verarbeiten von D2C-Nachrichten mit IoT Hub][lnk-process-d2c-tutorial].

7. Fügen Sie abschließend der **Main**-Methode die folgenden Zeilen hinzu:

    ```csharp
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
    ```

## <a name="create-a-device-app"></a>Erstellen einer Geräte-App

In diesem Abschnitt erstellen Sie eine .NET-Konsolenanwendung, die ein Gerät simuliert, das D2C-Nachrichten (Device to Cloud, Gerät zu Cloud) an einen IoT-Hub sendet.

1. Fügen Sie in Visual Studio in der aktuellen Projektmappe mithilfe der Projektvorlage **Konsolenanwendung (.NET Framework** ein Visual C#-Projekt für den klassischen Windows-Desktop hinzu. Stellen Sie sicher, dass .NET-Framework-Version 4.5.1 oder höher verwendet wird. Nennen Sie das Projekt **SimulatedDevice**.

    ![Neues Visual C#-Projekt für den klassischen Windows-Desktop][10b]

2. Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste auf das Projekt **SimulatedDevice**, und klicken Sie dann auf **NuGet-Pakete verwalten**.

3. Wählen Sie im Fenster **NuGet-Paket-Manager** die Option **Durchsuchen** aus, suchen Sie nach **Microsoft.Azure.Devices.Client**, wählen Sie zum Installieren des Pakets **Microsoft.Azure.Devices** die Option **Installieren** aus, und akzeptieren Sie die Nutzungsbedingungen. Bei diesem Verfahren wird das [NuGet-Paket mit dem Azure IoT-Geräte-SDK][lnk-device-nuget] heruntergeladen und installiert und ein Verweis auf das Paket und seine Abhängigkeiten hinzugefügt.

4. Fügen Sie am Anfang der Datei **Program.cs** die folgende `using`-Anweisung hinzu:

    ```csharp
    using Microsoft.Azure.Devices.Client;
    using Newtonsoft.Json;
    ```

5. Fügen Sie der **Program** -Klasse die folgenden Felder hinzu. Ersetzen Sie `{iot hub hostname}` durch den im Abschnitt „Erstellen eines IoT Hubs“ abgerufenen Hostnamen des IoT-Hubs. Ersetzen Sie `{device key}` durch den Geräteschlüssel, den Sie im Abschnitt „Erstellen einer Geräteidentität“ abgerufen haben.

    ```csharp
    static DeviceClient deviceClient;
    static string iotHubUri = "{iot hub hostname}";
    static string deviceKey = "{device key}";
    ```

6. Fügen Sie der **Program** -Klasse die folgende Methode hinzu:

    ```csharp
    private static async void SendDeviceToCloudMessagesAsync()
    {
        double minTemperature = 20;
        double minHumidity = 60;
        int messageId = 1;
        Random rand = new Random();

        while (true)
        {
            double currentTemperature = minTemperature + rand.NextDouble() * 15;
            double currentHumidity = minHumidity + rand.NextDouble() * 20;

            var telemetryDataPoint = new
            {
                messageId = messageId++,
                deviceId = "myFirstDevice",
                temperature = currentTemperature,
                humidity = currentHumidity
            };
            var messageString = JsonConvert.SerializeObject(telemetryDataPoint);
            var message = new Message(Encoding.ASCII.GetBytes(messageString));
            message.Properties.Add("temperatureAlert", (currentTemperature > 30) ? "true" : "false");

            await deviceClient.SendEventAsync(message);
            Console.WriteLine("{0} > Sending message: {1}", DateTime.Now, messageString);

            await Task.Delay(1000);
        }
    }
    ```

    Diese Methode sendet jede Sekunde eine neue D2C-Nachricht. Die Nachricht enthält ein JSON-serialisiertes Objekt mit der Geräte-ID sowie zufällig generierten Zahlen, um einen Temperatur- und Feuchtigkeitssensor zu simulieren.

7. Fügen Sie abschließend der **Main**-Methode die folgenden Zeilen hinzu:

    ```csharp
    Console.WriteLine("Simulated device\n");
    deviceClient = DeviceClient.Create(iotHubUri, new DeviceAuthenticationWithRegistrySymmetricKey ("myFirstDevice", deviceKey), TransportType.Mqtt);

    SendDeviceToCloudMessagesAsync();
    Console.ReadLine();
    ```

    In der Standardeinstellung erstellt die **Create**-Methode in einer .NET Framework-App eine **DeviceClient**-Instanz, die das AMQP-Protokoll für die Kommunikation mit dem IoT Hub verwendet. Um das MQTT- oder HTTP-Protokoll zu verwenden, nutzen Sie die Überschreibung der **Create**-Methode, bei der Sie das Protokoll angeben können. UWP- und PCL-Clients nutzen standardmäßig HTTP. Wenn Sie das HTTP-Protokoll verwenden, müssen Sie Ihrem Projekt auch das NuGet-Paket **Microsoft.AspNet.WebApi.Client** hinzufügen, um den **System.Net.Http.Formatting**-Namespace einzuschließen.

In diesem Tutorial werden die Schritte zum Erstellen eines IoT Hub-Geräteclients beschrieben. Sie können auch die Visual Studio-Erweiterung für den [verknüpften Dienst für Azure IoT Hub][lnk-connected-service] verwenden, um Ihrer Geräte-App den erforderlichen Code hinzuzufügen.

> [!NOTE]
> Der Einfachheit halber wird in diesem Tutorial keine Wiederholungsrichtlinie implementiert. Im Produktionscode sollten Sie Wiederholungsrichtlinien implementieren (z.B. einen exponentiellen Backoff), wie im MSDN-Artikel zum [Transient Fault Handling (Behandeln vorübergehender Fehler)][lnk-transient-faults] beschrieben.

## <a name="run-the-apps"></a>Ausführen der Apps

Sie können die Apps nun ausführen.

1. Klicken Sie in Visual Studio im Projektmappen-Explorer mit der rechten Maustaste auf Ihre Projektmappe, und klicken Sie dann auf **Startprojekte festlegen**. Wählen Sie **Mehrere Startprojekte**, und wählen Sie dann für die Projekte **ReadDeviceToCloudMessages** und **SimulatedDevice** als Aktion die Option **Starten** aus.

    ![Startprojekteigenschaften][41]

2. Drücken Sie **F5** , um die Ausführung der beiden Apps zu starten. Die Konsolenausgabe der **SimulatedDevice**-App zeigt die Nachrichten an, die Ihre Geräte-App an Ihren IoT Hub sendet. Die Konsolenausgabe der **ReadDeviceToCloudMessages** -App zeigt die Nachrichten an, die vom IoT Hub empfangen werden.

    ![Konsolenausgabe von Apps][42]

3. Über die Kachel **Nutzung** im [Azure-Portal][lnk-portal] wird die Anzahl der an den IoT-Hub gesendeten Nachrichten angezeigt:

    ![Kachel „Verwendung“ im Azure-Portal][43]

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie im Azure-Portal einen IoT-Hub konfiguriert und anschließend in der Identitätsregistrierung des IoT-Hubs eine Geräteidentität erstellt. Sie haben diese Geräteidentität verwendet, um die Geräte-App für das Senden von D2C-Nachrichten an den IoT Hub zu aktivieren. Sie haben außerdem eine App erstellt, mit der die vom IoT-Hub empfangenen Nachrichten angezeigt werden.

Informationen zu den weiteren ersten Schritten mit IoT Hub und zum Kennenlernen anderer IoT-Szenarien finden Sie in den folgenden Artikeln:

* [Verbinden Ihres Geräts][lnk-connect-device]
* [Erste Schritte mit der Geräteverwaltung][lnk-device-management]
* [Erste Schritte mit IoT Edge][lnk-iot-edge]

Informationen dazu, wie Sie Ihre IoT-Lösung erweitern und Ihre D2C-Nachrichten mit Skalierbarkeit verarbeiten, finden Sie im Tutorial [Verarbeiten von D2C-Nachrichten mit IoT Hub][lnk-process-d2c-tutorial].

[!INCLUDE [iot-hub-get-started-next-steps](../../includes/iot-hub-get-started-next-steps.md)]

<!-- Images. -->
[41]: ./media/iot-hub-csharp-csharp-getstarted/run-apps1.png
[42]: ./media/iot-hub-csharp-csharp-getstarted/run-apps2.png
[43]: ./media/iot-hub-csharp-csharp-getstarted/usage.png
[10a]: ./media/iot-hub-csharp-csharp-getstarted/create-receive-csharp1.png
[10b]: ./media/iot-hub-csharp-csharp-getstarted/create-device-csharp1.png


<!-- Links -->
[lnk-process-d2c-tutorial]: iot-hub-csharp-csharp-process-d2c.md

[lnk-hub-sdks]: iot-hub-devguide-sdks.md
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-portal]: https://portal.azure.com/

[lnk-eventhubs-tutorial]: ../event-hubs/event-hubs-csharp-ephcs-getstarted.md
[lnk-servicebus-nuget]: https://www.nuget.org/packages/WindowsAzure.ServiceBus
[lnk-event-hubs-overview]: ../event-hubs/event-hubs-overview.md

[lnk-device-nuget]: https://www.nuget.org/packages/Microsoft.Azure.Devices.Client/
[lnk-transient-faults]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx
[lnk-connected-service]: https://visualstudiogallery.msdn.microsoft.com/e254a3a5-d72e-488e-9bd3-8fee8e0cd1d6
[lnk-device-management]: iot-hub-node-node-device-management-get-started.md
[lnk-iot-edge]: iot-hub-linux-iot-edge-get-started.md
[lnk-connect-device]: https://azure.microsoft.com/develop/iot/

