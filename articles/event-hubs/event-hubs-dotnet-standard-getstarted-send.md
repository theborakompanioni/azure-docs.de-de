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
ms.date: 03/27/2017
ms.author: jotaub;sethm
translationtype: Human Translation
ms.sourcegitcommit: 1cc1ee946d8eb2214fd05701b495bbce6d471a49
ms.openlocfilehash: 12d4282082b3318bd14535a49e25c8d606d1bd7f
ms.lasthandoff: 04/26/2017

---

# <a name="get-started-sending-messages-to-azure-event-hubs-in-net-standard"></a>Erste Schritte beim Senden von Nachrichten an Azure Event Hubs in .NET Standard

> [!NOTE]
> Dieses Beispiel ist auf [GitHub](https://github.com/Azure/azure-event-hubs/tree/master/samples/DotNet/Microsoft.Azure.EventHubs/SampleSender) verfügbar.

Dieses Tutorial zeigt, wie Sie eine .NET Core-Konsolenanwendung schreiben können, die eine Reihe von Nachrichten an einen Event Hub sendet. Sie können die [GitHub](https://github.com/Azure/azure-event-hubs/tree/master/samples/DotNet/Microsoft.Azure.EventHubs/SampleSender)-Projektmappe ohne Änderung ausführen, indem Sie die Zeichenfolgen `EhConnectionString` und `EhEntityPath` durch Ihre Event Hub-Werte ersetzen. Sie können auch die Schritte in diesem Tutorial ausführen, um eine eigene zu erstellen.

## <a name="prerequisites"></a>Voraussetzungen

* [Microsoft Visual Studio 2015 oder 2017](http://www.visualstudio.com) In den Beispielen in diesem Tutorial wird Visual Studio 2017 verwendet, aber Visual Studio 2015 wird ebenfalls unterstützt.
* [.NET Core Visual Studio 2015- oder 2017-Tools](http://www.microsoft.com/net/core).
* Ein Azure-Abonnement.
* Ein Event Hub-Namespace.

Um Nachrichten an eine Event Hubs-Instanz senden zu können, erstellen wir mithilfe von Visual Studio eine C#-Konsolenanwendung.

## <a name="create-an-event-hubs-namespace-and-an-event-hub"></a>Erstellen eines Event Hubs-Namespace und eines Event Hubs

Verwenden Sie zunächst das [Azure-Portal](https://portal.azure.com), um einen Namespace für den Event Hub-Typ zu erstellen, und rufen Sie die Verwaltungsanmeldeinformationen ab, die Ihre Anwendung benötigt, um mit dem Event Hub zu kommunizieren. Folgen Sie dem Ablauf in [diesem Artikel](event-hubs-create.md), um einen Namespace und einen Event Hub zu erstellen, und fahren Sie dann mit folgenden Schritten fort.

## <a name="create-a-console-application"></a>Erstellen einer Konsolenanwendung

Starten Sie Visual Studio. Klicken Sie im Menü **Datei** auf **Neu** und dann auf **Projekt**. Erstellen Sie eine .NET Core-Konsolenanwendung.

![Neues Projekt][1]

## <a name="add-the-event-hubs-nuget-package"></a>Hinzufügen des Event Hubs-NuGet-Pakets

Fügen Sie Ihrem Projekt das NuGet-Paket [`Microsoft.Azure.EventHubs`](https://www.nuget.org/packages/Microsoft.Azure.EventHubs/) hinzu.

## <a name="write-some-code-to-send-messages-to-the-event-hub"></a>Schreiben von Code zum Senden von Nachrichten an den Event Hub

1. Fügen Sie am Anfang der Datei „Program.cs“ die folgenden `using`-Anweisungen hinzu:

    ```csharp
    using Microsoft.Azure.EventHubs;
    using System.Text;
    using System.Threading.Tasks;
    ```

2. Fügen Sie der `Program`-Klasse Konstanten für die Event Hubs-Verbindungszeichenfolge und den Event Hubs-Entitätspfad (einzelner Event Hub-Name) hinzu. Ersetzen Sie die Platzhalter in Klammern durch die entsprechenden Werte, die beim Erstellen des Event Hubs abgerufen wurden.

    ```csharp
    private static EventHubClient eventHubClient;
    private const string EhConnectionString = "{Event Hubs connection string}";
    private const string EhEntityPath = "{Event Hub path/name}";
    ```

3. Fügen Sie wie folgt eine neue Methode mit dem Namen `MainAsync` in die `Program`-Klasse ein:

    ```csharp
    private static async Task MainAsync(string[] args)
    {
        // Creates an EventHubsConnectionStringBuilder object from the connection string, and sets the EntityPath.
        // Typically, the connection string should have the entity path in it, but for the sake of this simple scenario
        // we are using the connection string from the namespace.
        var connectionStringBuilder = new EventHubsConnectionStringBuilder(EhConnectionString)
        {
            EntityPath = EhEntityPath
        };

        eventHubClient = EventHubClient.CreateFromConnectionString(connectionStringBuilder.ToString());

        await SendMessagesToEventHub(100);

        await eventHubClient.CloseAsync();

        Console.WriteLine("Press ENTER to exit.");
        Console.ReadLine();
    }
    ```

4. Fügen Sie wie folgt eine neue Methode mit dem Namen `SendMessagesToEventHub` in die `Program`-Klasse ein:

    ```csharp
    // Creates an event hub client and sends 100 messages to the event hub.
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

    ```csharp
    MainAsync(args).GetAwaiter().GetResult();
    ```

   „Program.cs“ sollte nun wie folgt aussehen:

    ```csharp
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
                // Creates an EventHubsConnectionStringBuilder object from the connection string, and sets the EntityPath.
                // Typically, the connection string should have the entity path in it, but for the sake of this simple scenario
                // we are using the connection string from the namespace.
                var connectionStringBuilder = new EventHubsConnectionStringBuilder(EhConnectionString)
                {
                    EntityPath = EhEntityPath
                };

                eventHubClient = EventHubClient.CreateFromConnectionString(connectionStringBuilder.ToString());

                await SendMessagesToEventHub(100);

                await eventHubClient.CloseAsync();

                Console.WriteLine("Press ENTER to exit.");
                Console.ReadLine();
            }

            // Creates an event hub client and sends 100 messages to the event hub.
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

6. Führen Sie das Programm aus, und stellen Sie sicher, dass keine Fehler auftreten.

Glückwunsch! Sie haben jetzt Nachrichten an einen Event Hub gesendet.

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen zu Event Hubs finden Sie unter den folgenden Links:

* [Empfangen von Ereignissen von Event Hubs](event-hubs-dotnet-standard-getstarted-receive-eph.md)
* [Übersicht über Event Hubs](event-hubs-what-is-event-hubs.md)
* [Erstellen eines Event Hubs](event-hubs-create.md)
* [Event Hubs – häufig gestellte Fragen](event-hubs-faq.md)

[1]: ./media/event-hubs-dotnet-standard-getstarted-send/netcore.png

