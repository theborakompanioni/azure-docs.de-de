---
title: Empfangen von Ereignissen von Azure Event Hubs mithilfe von .NET Standard | Microsoft-Dokumentation
description: "Erste Schritte zum Empfangen von Nachrichten mit „EventProcessorHost“ in .NET Standard"
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
ms.date: 02/01/2017
ms.author: jotaub
translationtype: Human Translation
ms.sourcegitcommit: f92909e0098a543f99baf3df3197a799bc9f1edc
ms.openlocfilehash: 8f0870c98f625a56cfddacd91b401206293122f1
ms.lasthandoff: 03/01/2017

---

# <a name="get-started-receiving-messages-with-the-eventprocessorhost-in-net-standard"></a>Erste Schritte zum Empfangen von Nachrichten mit „EventProcessorHost“ in .NET Standard

> [!NOTE]
> Dieses Beispiel ist auf [GitHub](https://github.com/Azure/azure-event-hubs-dotnet/tree/master/samples/SampleEphReceiver) verfügbar.

## <a name="what-will-be-accomplished"></a>Ziele

In diesem Tutorial wird gezeigt, wie die vorhandene Projektmappe **SampleEphReceiver** (in diesem Ordner) erstellt wird. Sie können die Projektmappe ohne Änderung ausführen. Ersetzen Sie die Zeichenfolgen `EhConnectionString`, `EhEntityPath` und `StorageAccount` durch Ihre Event Hub- und Speicherkontowerte, oder führen Sie die Schritte in diesem Tutorial aus, um eine eigene Projektmappe zu erstellen.

In diesem Tutorial schreiben Sie eine .NET Core-Konsolenanwendung zum Empfangen von Nachrichten von einem Event Hub mithilfe von **EventProcessorHost**.

## <a name="prerequisites"></a>Voraussetzungen

1. [Microsoft Visual Studio 2015 oder 2017](http://www.visualstudio.com)

2. [.NET Core Visual Studio 2015- oder 2017-Tools](http://www.microsoft.com/net/core)

3. Ein Azure-Abonnement.

4. Ein Event Hubs-Namespace
    
## <a name="receive-messages-from-the-event-hub"></a>Empfangen von Nachrichten vom Event Hub

### <a name="create-a-console-application"></a>Erstellen einer Konsolenanwendung

1. Starten Sie Visual Studio, und erstellen Sie eine neue .NET Core-Konsolenanwendung.

### <a name="add-the-event-hubs-nuget-package"></a>Hinzufügen des Event Hubs-NuGet-Pakets

* Fügen Sie Ihrem Projekt die folgenden NuGet-Pakete hinzu:
  * [`Microsoft.Azure.EventHubs`](https://www.nuget.org/packages/Microsoft.Azure.EventHubs/)
  * [`Microsoft.Azure.EventHubs.Processor`](https://www.nuget.org/packages/Microsoft.Azure.EventHubs.Processor/)

### <a name="implement-the-ieventprocessor-interface"></a>Implementieren der IEventProcessor-Schnittstelle

1. Erstellen Sie eine neue Klasse namens „SimpleEventProcessor“.

2. Fügen Sie die folgenden `using`-Anweisungen am Anfang der Datei „SimpleEventProcessor.cs“ hinzu:

    ```cs
    using Microsoft.Azure.EventHubs;
    using Microsoft.Azure.EventHubs.Processor;
    ```

3. Implementieren Sie die `IEventProcessor`-Schnittstelle. Die Klasse sollte wie folgt aussehen:

    ```cs
    namespace SampleEphReceiver
    {
        using System;
        using System.Collections.Generic;
        using System.Text;
        using System.Threading.Tasks;
        using Microsoft.Azure.EventHubs;
        using Microsoft.Azure.EventHubs.Processor;
    
        public class SimpleEventProcessor : IEventProcessor
        {
            public Task CloseAsync(PartitionContext context, CloseReason reason)
            {
                Console.WriteLine($"Processor Shutting Down. Partition '{context.PartitionId}', Reason: '{reason}'.");
                return Task.CompletedTask;
            }
    
            public Task OpenAsync(PartitionContext context)
            {
                Console.WriteLine($"SimpleEventProcessor initialized. Partition: '{context.PartitionId}'");
                return Task.CompletedTask;
            }
    
            public Task ProcessErrorAsync(PartitionContext context, Exception error)
            {
                Console.WriteLine($"Error on Partition: {context.PartitionId}, Error: {error.Message}");
                return Task.CompletedTask;
            }
    
            public Task ProcessEventsAsync(PartitionContext context, IEnumerable<EventData> messages)
            {
                foreach (var eventData in messages)
                {
                    var data = Encoding.UTF8.GetString(eventData.Body.Array, eventData.Body.Offset, eventData.Body.Count);
                    Console.WriteLine($"Message received. Partition: '{context.PartitionId}', Data: '{data}'");
                }
    
                return context.CheckpointAsync();
            }
        }
    }
    ```

### <a name="write-a-main-console-method-that-uses-simpleeventprocessor-to-receive-messages-from-an-event-hub"></a>Schreiben einer Hauptkonsolenmethode, die `SimpleEventProcessor` zum Empfangen von Nachrichten von einem Event Hub verwendet

1. Fügen Sie am Anfang der Datei „Program.cs“ die folgenden `using`-Anweisungen hinzu:
  
    ```cs
    using Microsoft.Azure.EventHubs;
    using Microsoft.Azure.EventHubs.Processor;
    ```

2. Fügen Sie der `Program`-Klasse Konstanten für die Event Hubs-Verbindungszeichenfolge, den Event Hub-Pfad, den Speichercontainernamen, den Speicherkontonamen und den Speicherkontoschlüssel hinzu. Ersetzen Sie Platzhalter durch die entsprechenden Werte.

    ```cs
    private const string EhConnectionString = "{Event Hubs connection string}";
    private const string EhEntityPath = "{Event Hub path/name}";
    private const string StorageContainerName = "{Storage account container name}";
    private const string StorageAccountName = "{Storage account name}";
    private const string StorageAccountKey = "{Storage account key}";

    private static readonly string StorageConnectionString = string.Format("DefaultEndpointsProtocol=https;AccountName={0};AccountKey={1}", StorageAccountName, StorageAccountKey);
    ```   

3. Fügen Sie der `Program`-Klasse folgendermaßen eine neue Methode namens `MainAsync` hinzu:
    ```cs
    private static async Task MainAsync(string[] args)
    {
        Console.WriteLine("Registering EventProcessor...");

        var eventProcessorHost = new EventProcessorHost(
            EhEntityPath,
            PartitionReceiver.DefaultConsumerGroupName,
            EhConnectionString,
            StorageConnectionString,
            StorageContainerName);

        // Registers the Event Processor Host and starts receiving messages
        await eventProcessorHost.RegisterEventProcessorAsync<SimpleEventProcessor>();

        Console.WriteLine("Receiving. Press enter key to stop worker.");
        Console.ReadLine();

        // Disposes of the Event Processor Host
        await eventProcessorHost.UnregisterEventProcessorAsync();
    }
    ```

3. Fügen Sie der `Main`-Methode die folgende Codezeile hinzu:

    ```cs
    MainAsync(args).GetAwaiter().GetResult();
    ```

    Die Datei „Program.cs“ sollte nun wie folgt aussehen:

    ```cs
    namespace SampleEphReceiver
    {
        using System;
        using System.Threading.Tasks;
        using Microsoft.Azure.EventHubs;
        using Microsoft.Azure.EventHubs.Processor;
    
        public class Program
        {
            private const string EhConnectionString = "{Event Hubs connection string}";
            private const string EhEntityPath = "{Event Hub path/name}";
            private const string StorageContainerName = "{Storage account container name}";
            private const string StorageAccountName = "{Storage account name}";
            private const string StorageAccountKey = "{Storage account key}";
    
            private static readonly string StorageConnectionString = string.Format("DefaultEndpointsProtocol=https;AccountName={0};AccountKey={1}", StorageAccountName, StorageAccountKey);
    
            public static void Main(string[] args)
            {
                MainAsync(args).GetAwaiter().GetResult();
            }
    
            private static async Task MainAsync(string[] args)
            {
                Console.WriteLine("Registering EventProcessor...");
    
                var eventProcessorHost = new EventProcessorHost(
                    EhEntityPath,
                    PartitionReceiver.DefaultConsumerGroupName,
                    EhConnectionString,
                    StorageConnectionString,
                    StorageContainerName);
    
                // Registers the Event Processor Host and starts receiving messages
                await eventProcessorHost.RegisterEventProcessorAsync<SimpleEventProcessor>();
    
                Console.WriteLine("Receiving. Press enter key to stop worker.");
                Console.ReadLine();
    
                // Disposes of the Event Processor Host
                await eventProcessorHost.UnregisterEventProcessorAsync();
            }
        }
    }
    ```
  
4. Führen Sie das Programm aus, und stellen Sie sicher, dass keine Fehler auftreten.
  
Glückwunsch! Sie haben jetzt Nachrichten von einem Event Hub empfangen.

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen zu Event Hubs finden Sie unter den folgenden Links:

* [Übersicht über Event Hubs](event-hubs-what-is-event-hubs.md)
* [Erstellen eines Event Hubs](event-hubs-create.md)
* [Event Hubs – häufig gestellte Fragen](event-hubs-faq.md)
