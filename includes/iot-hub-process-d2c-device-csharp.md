## Senden interaktiver Nachrichten von einem simulierten Gerät
In diesem Abschnitt ändern Sie die simulierte Geräteanwendung, die Sie im Tutorial [Erste Schritte mit IoT Hub] erstellt haben, sodass sie interaktive D2C-Nachrichten an IoT Hub sendet.

1. Fügen Sie in Visual Studio im **SimulatedDevice**-Projekt der **Program**-Klasse folgende Methode hinzu.
   
    ```
    private static async void SendDeviceToCloudInteractiveMessagesAsync()
    {
      while (true)
      {
        var interactiveMessageString = "Alert message!";
        var interactiveMessage = new Message(Encoding.ASCII.GetBytes(interactiveMessageString));
        interactiveMessage.Properties["messageType"] = "interactive";
        interactiveMessage.MessageId = Guid.NewGuid().ToString();
   
        await deviceClient.SendEventAsync(interactiveMessage);
        Console.WriteLine("{0} > Sending interactive message: {1}", DateTime.Now, interactiveMessageString);
   
        Task.Delay(10000).Wait();
      }
    }
    ```
   
    Diese Methode ähnelt der **SendDeviceToCloudMessagesAsync**-Methode im Projekt **SimulatedDevice**. Die einzigen Unterschiede bestehen darin, dass Sie jetzt die Systemeigenschaft **MessageId** und eine Benutzereigenschaft **messageType** festlegen. Der Code weist einen global eindeutigen Bezeichner (GUID) der Eigenschaft **MessageId** zu. Der Service Bus kann damit Nachrichten, die er empfängt, deduplizieren. Im Beispiel wird die Eigenschaft **messageType** verwendet, um interaktive von Datenpunktnachrichten zu unterscheiden. Die Anwendung übergibt diese Informationen in Nachrichteneigenschaften anstatt im Nachrichtentext. Auf diese Weise muss der Ereignisprozessor zum Nachrichtenrouting nicht die gesamte Nachricht deserialisieren.
   
   > [!NOTE]
   > Es ist wichtig, die **MessageId**, die zum Deduplizieren interaktiver Nachrichten verwendet wird, im Gerätecode zu erstellen. Zeitweise Netzwerkkommunikation oder andere Fehler könnten dazu führen, dass mehrere Neuübertragungen der gleichen Nachricht von diesem Gerät erfolgen. Sie können anstelle einer GUID auch eine semantische Nachrichten-ID verwenden, z.B. einen Hash der relevanten Nachrichtendatenfelder.
   > 
   > 
2. Fügen Sie in der **Main**-Methode unmittelbar vor der Zeile `Console.ReadLine()` folgende Methode hinzu:
   
    ````
    SendDeviceToCloudInteractiveMessagesAsync();
    ````
   
   > [!NOTE]
   > Der Einfachheit halber wird in diesem Tutorial keine Wiederholungsrichtlinie implementiert. Im Produktionscode sollten Sie eine Wiederholungsrichtlinie implementieren (z. B. Exponential Backoff), wie im MSDN-Artikel zum [Behandeln vorübergehender Fehler] beschrieben.
   > 
   > 

<!-- Links -->
[Behandeln vorübergehender Fehler]: https://msdn.microsoft.com/library/hh675232.aspx
[Erste Schritte mit IoT Hub]: ../articles/iot-hub/iot-hub-csharp-csharp-getstarted.md

<!---HONumber=AcomDC_0608_2016-->