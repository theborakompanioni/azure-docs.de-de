---
title: "Übersicht über die Azure Event Hubs-APIs | Microsoft Docs"
description: "Eine Zusammenfassung einiger wichtiger .NET-Client-APIs für Event Hubs."
services: event-hubs
documentationcenter: na
author: sethmanheim
manager: timlt
editor: 
ms.assetid: 7f3b6cc0-9600-417f-9e80-2345411bd036
ms.service: event-hubs
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/18/2016
ms.author: sethm
translationtype: Human Translation
ms.sourcegitcommit: c68b5cfadc3e98e15c119b3717c8098887ca67d0
ms.openlocfilehash: e8c97689f84d3bedb312ec4443427181a542e95b


---
# <a name="event-hubs-api-overview"></a>Übersicht über die Event Hubs-API
In diesem Artikel werden einige der wichtigsten .NET-Client-APIs von Event Hubs zusammengefasst. Es gibt zwei Kategorien: Verwaltungs- und Laufzeit-APIs. Laufzeit-APIs umfassen alle Vorgänge, die zum Senden und Empfangen einer Nachricht erforderlich sind. Verwaltungsvorgänge ermöglichen das Verwalten des Entitätszustands von Event Hubs durch das Erstellen, Aktualisieren und Löschen von Entitäten.

Überwachungsszenarien umfassen sowohl die Verwaltung als auch Laufzeit. Eine ausführliche Referenzdokumentation zu den .NET-APIs finden Sie in den Referenzen zu [Service Bus .NET](/dotnet/api) und zur [EventProcessorHost-API](/dotnet/api).

## <a name="management-apis"></a>Verwaltungs-APIs
Um die folgenden Verwaltungsvorgänge auszuführen, benötigen Sie **Verwaltungsberechtigungen** für den Event Hubs-Namespace:

### <a name="create"></a>Erstellen
```csharp
// Create the Event Hub
EventHubDescription ehd = new EventHubDescription(eventHubName);
ehd.PartitionCount = SampleManager.numPartitions;
namespaceManager.CreateEventHubAsync(ehd).Wait();
```

### <a name="update"></a>Aktualisieren
```csharp
EventHubDescription ehd = await namespaceManager.GetEventHubAsync(eventHubName);

// Create a customer SAS rule with Manage permissions
ehd.UserMetadata = "Some updated info";
string ruleName = "myeventhubmanagerule";
string ruleKey = SharedAccessAuthorizationRule.GenerateRandomKey();
ehd.Authorization.Add(new SharedAccessAuthorizationRule(ruleName, ruleKey, new AccessRights[] {AccessRights.Manage, AccessRights.Listen, AccessRights.Send} )); 
namespaceManager.UpdateEventHubAsync(ehd).Wait();
```

### <a name="delete"></a>Löschen
```csharp
namespaceManager.DeleteEventHubAsync("Event Hub name").Wait();
```

## <a name="run-time-apis"></a>Laufzeit-APIs
### <a name="create-publisher"></a>Herausgeber erstellen
```csharp
// EventHubClient model (uses implicit factory instance, so all links on same connection)
EventHubClient eventHubClient = EventHubClient.Create("Event Hub name");
```

### <a name="publish-message"></a>Meldung veröffentlichen
```csharp
// Create the device/temperature metric
MetricEvent info = new MetricEvent() { DeviceId = random.Next(SampleManager.NumDevices), Temperature = random.Next(100) };
EventData data = new EventData(new byte[10]); // Byte array
EventData data = new EventData(Stream); // Stream 
EventData data = new EventData(info, serializer) //Object and serializer 
    {
       PartitionKey = info.DeviceId.ToString()
    };

// Set user properties if needed
data.Properties.Add("Type", "Telemetry_" + DateTime.Now.ToLongTimeString());

// Send single message async
await client.SendAsync(data);
```

### <a name="create-consumer"></a>Consumer erstellen
```csharp
// Create the Event Hubs client
EventHubClient eventHubClient = EventHubClient.Create(EventHubName);

// Get the default consumer group
EventHubConsumerGroup defaultConsumerGroup = eventHubClient.GetDefaultConsumerGroup();

// All messages
EventHubReceiver consumer = await defaultConsumerGroup.CreateReceiverAsync(shardId: index);

// From one day ago
EventHubReceiver consumer = await defaultConsumerGroup.CreateReceiverAsync(shardId: index, startingDateTimeUtc:DateTime.Now.AddDays(-1));

// From specific offset, -1 means oldest
EventHubReceiver consumer = await defaultConsumerGroup.CreateReceiverAsync(shardId: index,startingOffset:-1); 
```

### <a name="consume-message"></a>Meldung verarbeiten
```csharp
var message = await consumer.ReceiveAsync();

// Provide a serializer
var info = message.GetBody<Type>(Serializer)

// Get a byte[]
var info = message.GetBytes(); 
msg = UnicodeEncoding.UTF8.GetString(info);
```

## <a name="event-processor-host-apis"></a>Ereignisprozessorhost-APIs
Diese APIs bieten Flexibilität für Arbeitsprozesse, die möglicherweise nicht mehr verfügbar sind, indem Shards auf verfügbare Worker aufgeteilt werden.

```csharp
// Checkpointing is done within the SimpleEventProcessor and on a per-consumerGroup per-partition basis, workers resume from where they last left off.
// Use the EventData.Offset value for checkpointing yourself, this value is unique per partition.

string eventHubConnectionString = System.Configuration.ConfigurationManager.AppSettings["Microsoft.ServiceBus.ConnectionString"];
string blobConnectionString = System.Configuration.ConfigurationManager.AppSettings["AzureStorageConnectionString"]; // Required for checkpoint/state

EventHubDescription eventHubDescription = new EventHubDescription(EventHubName);
EventProcessorHost host = new EventProcessorHost(WorkerName, EventHubName, defaultConsumerGroup.GroupName, eventHubConnectionString, blobConnectionString);
            host.RegisterEventProcessorAsync<SimpleEventProcessor>();

// To close
host.UnregisterEventProcessorAsync().Wait();   
```

Die [IEventProcessor](https://docs.microsoft.com/en-us/dotnet/api/microsoft.servicebus.messaging.ieventprocessor)-Schnittstelle wird wie folgt definiert:

```csharp
public class SimpleEventProcessor : IEventProcessor
{
    IDictionary<string, string> map;
    PartitionContext partitionContext;

    public SimpleEventProcessor()
    {
        this.map = new Dictionary<string, string>();
    }

    public Task OpenAsync(PartitionContext context)
    {
        this.partitionContext = context;

        return Task.FromResult<object>(null);
    }

    public async Task ProcessEventsAsync(PartitionContext context, IEnumerable<EventData> messages)
    {
        foreach (EventData message in messages)
        {
            Process messages here
        }

        // Checkpoint when appropriate
        await context.CheckpointAsync();

    }


    public async Task CloseAsync(PartitionContext context, CloseReason reason)
    {
        if (reason == CloseReason.Shutdown)
        {
            await context.CheckpointAsync();
        }
    }
}
```

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen zu Event Hubs-Szenarien finden Sie unter diesen Links:

* [Was ist Azure Event Hubs?](event-hubs-what-is-event-hubs.md)
* [Übersicht über Event Hubs](event-hubs-overview.md)
* [Programmierleitfaden für Event Hubs](event-hubs-programming-guide.md)
* [Event Hubs-Codebeispiele](http://code.msdn.microsoft.com/site/search?query=event hub&f\[0\].Value=event hubs&f\[0\].Type=SearchText&ac=5)

Die .NET-API-Referenzen sind hier verfügbar:

* [.NET-API-Referenzen zu Service Bus und Event Hubs](/dotnet/api)
* [Referenz zur Ereignisprozessorhost-API](/dotnet/api)




<!--HONumber=Jan17_HO1-->


