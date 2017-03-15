---
title: Senden von Ereignissen an Azure Event Hubs mithilfe von .NET Standard | Microsoft-Dokumentation
description: Erste Schritte beim Senden von Ereignissen an Event Hubs in .NET Standard
services: event-hubs
documentationcenter: na
author: jtaubensee
manager: timlt
editor: 
ms.assetid: 
ms.service: event-hubs
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/31/2017
ms.author: jotaub
translationtype: Human Translation
ms.sourcegitcommit: f92909e0098a543f99baf3df3197a799bc9f1edc
ms.openlocfilehash: dcb8bb1dcf287ecb2d91e19bf4df38765fe79114
ms.lasthandoff: 03/01/2017

---

# <a name="get-started-sending-messages-to-event-hubs-in-net-standard"></a>Erste Schritte beim Senden von Nachrichten an Event Hubs in .NET Standard

> [!NOTE]
> Dieses Beispiel ist auf [GitHub](https://github.com/Azure/azure-event-hubs-dotnet/tree/master/samples/SampleSender) verfügbar.

## <a name="what-will-be-accomplished"></a>Ziele

In diesem Tutorial wird gezeigt, wie die vorhandene Projektmappe **SampleSender** (in diesem Ordner) erstellt wird. Sie können die Projektmappe ohne Änderung ausführen. Ersetzen Sie die Zeichenfolgen `EhConnectionString`, `EhEntityPath` und `StorageAccount` durch Ihre Event Hub-Werte, oder führen Sie die Schritte in diesem Tutorial aus, um eine eigene Projektmappe zu erstellen.

In diesem Tutorial schreiben wir eine .NET Core-Konsolenanwendung zum Senden von Nachrichten an einen Event Hub.

## <a name="prerequisites"></a>Voraussetzungen

1. [Microsoft Visual Studio 2015 oder 2017](http://www.visualstudio.com)

2. [.NET Core Visual Studio 2015- oder 2017-Tools](http://www.microsoft.com/net/core)

3. Ein Azure-Abonnement.

4. Ein Event Hubs-Namespace

## <a name="send-messages-to-an-event-hub"></a>Senden von Nachrichten an einen Ereignis Hub

Um Nachrichten an einen Event Hub senden zu können, erstellen wir mithilfe von Visual Studio eine C#-Konsolenanwendung.

### <a name="create-a-console-application"></a>Erstellen einer Konsolenanwendung

* Starten Sie Visual Studio, und erstellen Sie eine neue .NET Core-Konsolenanwendung.

### <a name="add-the-event-hubs-nuget-package"></a>Hinzufügen des Event Hubs-NuGet-Pakets

* Fügen Sie Ihrem Projekt das NuGet-Paket [`Microsoft.Azure.EventHubs`](https://www.nuget.org/packages/Microsoft.Azure.EventHubs/) hinzu.

### <a name="write-some-code-to-send-messages-to-the-event-hub"></a>Schreiben von Code zum Senden von Nachrichten an den Event Hub

1. Fügen Sie am Anfang der Datei „Program.cs“ die folgende `using`-Anweisung hinzu:

    ```cs
    using Microsoft.Azure.EventHubs;
    ```

2. Fügen Sie der `Program`-Klasse Konstanten für die Event Hubs-Verbindungszeichenfolge und den Event Hubs-Entitätspfad (einzelner Event Hub-Name) hinzu. Ersetzen Sie die Platzhalter in Klammern durch die entsprechenden Werte, die beim Erstellen des Event Hubs abgerufen wurden.

    ```cs
    private static EventHubClient eventHubClient;
    private const string EhConnectionString = "{Event Hubs connection string}";
    private const string EhEntityPath = "{Event Hub path/name}";
    ```

3. Fügen Sie der `Program`-Klasse folgendermaßen eine neue Methode namens `MainAsync` hinzu:

    ```cs
    private static async Task MainAsync(string[] args)
    {
        // Creates an EventHubsConnectionStringBuilder object from a the connection string, and sets the EntityPath.
        // Typically the connection string should have the Entity Path in it, but for the sake of this simple scenario
        // we are using the connection string from the namespace.
        var connectionStringBuilder = new EventHubsConnectionStringBuilder(EhConnectionString)
        {
            EntityPath = EhEntityPath
        };

        eventHubClient = EventHubClient.CreateFromConnectionString(connectionStringBuilder.ToString());

        await SendMessagesToEventHub(100);

        await eventHubClient.CloseAsync();

        Console.WriteLine("Press any key to exit.");
        Console.ReadLine();
    }
    ```
    
4. Fügen Sie der `Program`-Klasse folgendermaßen eine neue Methode namens `SendMessagesToEventHub` hinzu:

    ```cs
    // Creates an Event Hub client and sends 100 messages to the event hub.
    private static async Task SendMessagesToEventHub(int numMessagesToSend)
    {
        for (var i = 0; i < numMessagesToSend; i++)
        {
            try
            {
                var message = $"Message {i}";
                Console.WriteLine($"Sending message: {message}");
                await eventHubClient.SendAsync(new EventData(Encoding.UTF8.GetBytes(message)));
            }
            catch (Exception exception)
            {
                Console.WriteLine($"{DateTime.Now} > Exception: {exception.Message}");
            }

            await Task.Delay(10);
        }

        Console.WriteLine($"{numMessagesToSend} messages sent.");
    }
    ```

5. Fügen Sie der `Main`-Methode in der `Program`-Klasse den folgenden Code hinzu.

    ```cs
    MainAsync(args).GetAwaiter().GetResult();
    ```

    „Program.cs“ sollte nun wie folgt aussehen:

    ```cs
    namespace SampleSender
    {
        using System;
        using System.Text;
        using System.Threading.Tasks;
        using Microsoft.Azure.EventHubs;
    
        public class Program
        {
            private static EventHubClient eventHubClient;
            private const string EhConnectionString = "{Event Hubs connection string}";
            private const string EhEntityPath = "{Event Hub path/name}";
    
            public static void Main(string[] args)
            {
                MainAsync(args).GetAwaiter().GetResult();
            }
    
            private static async Task MainAsync(string[] args)
            {
                // Creates an EventHubsConnectionStringBuilder object from a the connection string, and sets the EntityPath.
                // Typically the connection string should have the Entity Path in it, but for the sake of this simple scenario
                // we are using the connection string from the namespace.
                var connectionStringBuilder = new EventHubsConnectionStringBuilder(EhConnectionString)
                {
                    EntityPath = EhEntityPath
                };
    
                eventHubClient = EventHubClient.CreateFromConnectionString(connectionStringBuilder.ToString());
    
                await SendMessagesToEventHub(100);
    
                await eventHubClient.CloseAsync();
    
                Console.WriteLine("Press any key to exit.");
                Console.ReadLine();
            }
    
            // Creates an Event Hub client and sends 100 messages to the event hub.
            private static async Task SendMessagesToEventHub(int numMessagesToSend)
            {
                for (var i = 0; i < numMessagesToSend; i++)
                {
                    try
                    {
                        var message = $"Message {i}";
                        Console.WriteLine($"Sending message: {message}");
                        await eventHubClient.SendAsync(new EventData(Encoding.UTF8.GetBytes(message)));
                    }
                    catch (Exception exception)
                    {
                        Console.WriteLine($"{DateTime.Now} > Exception: {exception.Message}");
                    }
    
                    await Task.Delay(10);
                }
    
                Console.WriteLine($"{numMessagesToSend} messages sent.");
            }
        }
    }
    ```
  
6. Führen Sie das Programm aus, und stellen Sie sicher, dass keine Fehler ausgelöst werden.
  
Glückwunsch! Sie haben jetzt Nachrichten an einen Event Hub gesendet.

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen zu Event Hubs finden Sie unter den folgenden Links:

* [Empfangen von Ereignissen von Event Hubs](event-hubs-dotnet-standard-getstarted-receive-eph.md)
* [Übersicht über Event Hubs](event-hubs-what-is-event-hubs.md)
* [Erstellen eines Event Hubs](event-hubs-create.md)
* [Event Hubs – häufig gestellte Fragen](event-hubs-faq.md)
