---
title: "Übersicht über die .NET Standard-APIs in Azure Event Hubs | Microsoft-Dokumentation"
description: ".NET Standard-API – Übersicht"
services: event-hubs
documentationcenter: na
author: sethmanheim
manager: timlt
editor: 
ms.assetid: a173f8e4-556c-42b8-b856-838189f7e636
ms.service: event-hubs
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/09/2017
ms.author: sethm
ms.translationtype: Human Translation
ms.sourcegitcommit: db7cb109a0131beee9beae4958232e1ec5a1d730
ms.openlocfilehash: 4e05b1cc41038b2239f9314c17b93d20eed33844
ms.contentlocale: de-de
ms.lasthandoff: 04/18/2017


---

# <a name="event-hubs-net-standard-api-overview"></a>Event Hubs .NET Standard-API – Übersicht
In diesem Artikel werden einige der wichtigsten .NET Standard-Client-APIs von Event Hubs zusammengefasst. Derzeit gibt es zwei .NET Standard-Clientbibliotheken:
* [Microsoft.Azure.EventHubs](/dotnet/api/microsoft.azure.eventhubs)
  *  Diese Bibliothek stellt alle grundlegenden Laufzeitvorgänge bereit.
* [Microsoft.Azure.EventHubs.Processor](/dotnet/api/microsoft.azure.eventhubs.processor)
  * Diese Bibliothek fügt zusätzliche Funktionen hinzu, die es ermöglichen, verarbeitete Ereignisse nachzuverfolgen. Dies ist die einfachste Möglichkeit zum Lesen aus einem Event Hub.

## <a name="event-hub-client"></a>Event Hub-Client
[**EventHubClient**](/dotnet/api/microsoft.azure.eventhubs.eventhubclient) ist das primäre Objekt zum Versenden von Ereignissen, zum Erstellen von Empfängern und zum Abrufen von Laufzeitinformationen. Dieser Client ist mit einem bestimmten Event Hub verknüpft und erstellt eine neue Verbindung mit dem Event Hubs-Endpunkt.

### <a name="create-an-event-hub-client"></a>Erstellen eines Event Hub-Clients
Ein [**EventHubClient**](/dotnet/api/microsoft.azure.eventhubs.eventhubclient)-Objekt wird aus einer Verbindungszeichenfolge erstellt. Die einfachste Möglichkeit zum Instanziieren eines neuen Clients sehen Sie im folgenden Beispiel:

```csharp
var eventHubClient = EventHubClient.CreateFromConnectionString("{Event Hub connection string}");
```

Sie können die Verbindungszeichenfolge programmgesteuert bearbeiten, indem Sie die Klasse [**EventHubsConnectionStringBuilder**](/dotnet/api/microsoft.azure.eventhubs.eventhubsconnectionstringbuilder) verwenden und die Verbindungszeichenfolge als Parameter an [**EventHubClient.CreateFromConnectionString**](/dotnet/api/microsoft.azure.eventhubs.eventhubclient#Microsoft_Azure_EventHubs_EventHubClient_CreateFromConnectionString_System_String_) weitergeben.

```csharp
var connectionStringBuilder = new EventHubsConnectionStringBuilder("{Event Hub connection string}")
{
    EntityPath = EhEntityPath
};

var eventHubClient = EventHubClient.CreateFromConnectionString(connectionStringBuilder.ToString());
```

### <a name="send-events"></a>Senden von Ereignisse
Mithilfe der [**EventData**](/dotnet/api/microsoft.azure.eventhubs.eventdata)-Klasse können Sie Ereignisse an einen Event Hub senden. Der Text muss ein `byte`-Array oder ein `byte`-Arraysegment sein.

```csharp
// Create a new EventData object by encoding a string as a byte array
var data = new EventData(Encoding.UTF8.GetBytes("This is my message..."));
// Set user properties if needed
data.Properties.Add("Type", "Informational");
// Send single message async
await eventHubClient.SendAsync(data);
```

### <a name="receive-events"></a>Empfangen von Ereignissen
Zum Empfangen von Ereignissen von Event Hubs empfiehlt sich die Verwendung von [**EventProcessorHost**](##Event-Processor-Host-APIs). Dabei ist eine Funktion zum automatischen Nachverfolgen von Offset und Partitionsinformationen verfügbar. In bestimmten Situationen bevorzugen Sie jedoch unter Umständen die Flexibilität der Event Hubs-Kernbibliothek, um Ereignisse zu empfangen.

#### <a name="create-a-receiver"></a>Erstellen eines Receivers
Receiver sind an bestimmte Partitionen gebunden. Damit alle Ereignisse in einem Event Hub empfangen werden können, müssen Sie mehrere Instanzen erstellen. Im Allgemeinen empfiehlt es sich, die Partitionsinformationen programmgesteuert abzurufen und nicht die Partitions-IDs fest zu programmieren. Hierfür können Sie die Methode [**GetRuntimeInformationAsync**](/dotnet/api/microsoft.azure.eventhubs.eventhubclient#Microsoft_Azure_EventHubs_EventHubClient_GetRuntimeInformationAsync) ausführen.

```csharp

// Create a list to keep track of the receivers
var receivers = new List<PartitionReceiver>();
// Use the eventHubClient created above to get the runtime information
var runTimeInformation = await eventHubClient.GetRuntimeInformationAsync();
// Loop over the resulting partition ids
foreach (var partitionId in runTimeInformation.PartitionIds)
{
    // Create the receiver
    var receiver = eventHubClient.CreateReceiver(PartitionReceiver.DefaultConsumerGroupName, partitionId, PartitionReceiver.EndOfStream);
    // Add the receiver to the list
    receivers.Add(receiver);
}
```

Da Ereignisse niemals aus einem Event Hub entfernt werden, sondern nur ablaufen, müssen Sie den korrekten Ausgangspunkt festlegen. Das folgende Beispiel zeigt mögliche Kombinationen.

```csharp
// partitionId is assumed to come from GetRuntimeInformationAsync()

// Using the constant 'PartitionReceiver.EndOfStream' will only receive all messages from this point forward.
var receiver = eventHubClient.CreateReceiver(PartitionReceiver.DefaultConsumerGroupName, partitionId, PartitionReceiver.EndOfStream);

// All messages available
var receiver = eventHubClient.CreateReceiver(PartitionReceiver.DefaultConsumerGroupName, partitionId, "-1");

// From one day ago
var receiver = eventHubClient.CreateReceiver(PartitionReceiver.DefaultConsumerGroupName, partitionId, DateTime.Now.AddDays(-1));
```

#### <a name="consume-an-event"></a>Nutzen eines Ereignisses
```csharp
// Receive a maximum of 100 messages in this call to ReceiveAsync
var ehEvents = await receiver.ReceiveAsync(100);
// ReceiveAsync can return null if there are no messages
if (ehEvents != null)
{
    // Since ReceiveAsync can return more than a single event you will need a loop to process
    foreach (var ehEvent in ehEvents)
    {
        // Decode the byte array segment
        var message = UnicodeEncoding.UTF8.GetString(ehEvent.Body.Array);
        // Load the custom property that we set in the send example
        var customType = ehEvent.Properties["Type"];
        // Implement processing logic here
    }
}        
```

## <a name="event-processor-host-apis"></a>Ereignisprozessorhost-APIs
Diese APIs bieten Flexibilität für Workerprozesse, die möglicherweise nicht mehr verfügbar sind, indem Partitionen auf verfügbare Worker aufgeteilt werden.

```csharp
// Checkpointing is done within the SimpleEventProcessor and on a per-consumerGroup per-partition basis, workers resume from where they last left off.

// Read these connection strings from a secure location
var ehConnectionString = "{Event Hubs connection string}";
var ehEntityPath = "{Event Hub path/name}";
var storageConnectionString = "{Storage connection string}";
var storageContainerName = "{Storage account container name}";

var eventProcessorHost = new EventProcessorHost(
    ehEntityPath,
    PartitionReceiver.DefaultConsumerGroupName,
    ehConnectionString,
    storageConnectionString,
    storageContainerName);

// Start/register an EventProcessorHost
await eventProcessorHost.RegisterEventProcessorAsync<SimpleEventProcessor>();

// Disposes of the Event Processor Host
await eventProcessorHost.UnregisterEventProcessorAsync();
```

Im Folgenden finden Sie eine Beispielimplementierung von [IEventProcessor](/dotnet/api/microsoft.azure.eventhubs.processor.ieventprocessor).

```csharp
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
```

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen zu Event Hubs-Szenarien finden Sie unter diesen Links:

* [Was ist Azure Event Hubs?](event-hubs-what-is-event-hubs.md)
* [Verfügbare Event Hubs-APIs](event-hubs-api-overview.md)

Die .NET-API-Referenzen sind hier verfügbar:

* [Microsoft.Azure.EventHubs](/dotnet/api/microsoft.azure.eventhubs)
* [Microsoft.Azure.EventHubs.Processor](/dotnet/api/microsoft.azure.eventhubs.processor)
