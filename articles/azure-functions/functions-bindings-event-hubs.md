---
title: "Event Hub-Bindungen für Azure Functions | Microsoft Docs"
description: Erfahren Sie, wie Azure Event Hub-Bindungen in Azure Functions verwendet werden.
services: functions
documentationcenter: na
author: wesmc7777
manager: erikre
editor: 
tags: 
keywords: Azure Functions, Funktionen, Ereignisverarbeitung, dynamisches Compute, serverlose Architektur
ms.assetid: daf81798-7acc-419a-bc32-b5a41c6db56b
ms.service: functions
ms.devlang: multiple
ms.topic: reference
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 11/02/2016
ms.author: wesmc
ms.translationtype: Human Translation
ms.sourcegitcommit: 17c4dc6a72328b613f31407aff8b6c9eacd70d9a
ms.openlocfilehash: 04a8563a0035992cfa4b7d25a4edc14e1db80e44
ms.contentlocale: de-de
ms.lasthandoff: 05/16/2017


---
# <a name="azure-functions-event-hub-bindings"></a>Event Hub-Bindungen für Azure Functions
[!INCLUDE [functions-selector-bindings](../../includes/functions-selector-bindings.md)]

Dieser Artikel erläutert das Konfigurieren und Codieren von [Azure Event Hub](../event-hubs/event-hubs-what-is-event-hubs.md) -Bindungen für Azure Functions.
Azure Functions unterstützt Trigger- und Ausgabebindungen für Event Hubs.

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

Wenn Sie mit Azure Event Hubs noch nicht vertraut sind, lesen Sie die Informationen unter [Übersicht über Event Hubs](../event-hubs/event-hubs-what-is-event-hubs.md).

<a name="trigger"></a>

## <a name="event-hub-trigger"></a>Event Hub-Trigger
Der Event Hub-Trigger kann verwendet werden, um auf ein Ereignis zu reagieren, das an einen Event Hub-Datenstrom gesendet wird. Sie benötigen Lesezugriff auf den Event Hub, um den Trigger einzurichten.

Der Event Hub-Trigger zu einer Funktion verwendet das folgende JSON-Objekt im `bindings`-Array von „function.json“:

```json
{
    "type": "eventHubTrigger",
    "name": "<Name of trigger parameter in function signature>",
    "direction": "in",
    "path": "<Name of the Event Hub>",
    "consumerGroup": "Consumer group to use - see below",
    "connection": "<Name of app setting with connection string - see below>"
}
```

`consumerGroup` ist eine optionale Eigenschaft, die zum Festlegen der [Consumergruppe](../event-hubs/event-hubs-features.md#event-consumers) verwendet wird, mit der Ereignisse im Hub abonniert werden. Wird sie nicht angegeben, wird die Consumergruppe `$Default` verwendet.  
`connection` muss der Name einer App-Einstellung sein, die die Zeichenfolge für die Verbindung mit dem Namespace des Event Hubs enthält.
Kopieren Sie diese Verbindungszeichenfolge, indem Sie für den *Namespace* (nicht für den eigentlichen Event Hub) auf die Schaltfläche **Verbindungsinformationen** klicken. Diese Verbindungszeichenfolge muss mindestens über Leseberechtigungen verfügen, um den Trigger zu aktivieren.

[Zusätzliche Einstellungen](https://github.com/Azure/azure-webjobs-sdk-script/wiki/host.json) können in einer host.json-Datei zur weiteren Feinabstimmung von Event Hub-Triggern angegeben werden.  

<a name="triggerusage"></a>

## <a name="trigger-usage"></a>Triggerverwendung
Wenn eine Event Hub-Triggerfunktion ausgelöst wird, wird die Meldung, die sie auslöst, als Zeichenfolge an die Funktion übergeben.

<a name="triggersample"></a>

## <a name="trigger-sample"></a>Triggerbeispiel
Angenommen, der folgende Event Hub-Trigger befindet sich im `bindings`-Array von „function.json“:

```json
{
  "type": "eventHubTrigger",
  "name": "myEventHubMessage",
  "direction": "in",
  "path": "MyEventHub",
  "connection": "myEventHubReadConnectionString"
}
```

Weitere Informationen finden Sie im sprachspezifischen Beispiel, in dem der Nachrichtentext des Event Hub-Triggers protokolliert wird.

* [C#](#triggercsharp)
* [F#](#triggerfsharp)
* [Node.js](#triggernodejs)

<a name="triggercsharp"></a>

### <a name="trigger-sample-in-c"></a>Triggerbeispiel in C# #

```cs
using System;

public static void Run(string myEventHubMessage, TraceWriter log)
{
    log.Info($"C# Event Hub trigger function processed a message: {myEventHubMessage}");
}
```

<a name="triggerfsharp"></a>

### <a name="trigger-sample-in-f"></a>Triggerbeispiel in F# #

```fsharp
let Run(myEventHubMessage: string, log: TraceWriter) =
    log.Info(sprintf "F# eventhub trigger function processed work item: %s" myEventHubMessage)
```

<a name="triggernodejs"></a>

### <a name="trigger-sample-in-nodejs"></a>Triggerbeispiel in Node.js

```javascript
module.exports = function (context, myEventHubMessage) {
    context.log('Node.js eventhub trigger function processed work item', myEventHubMessage);    
    context.done();
};
```

<a name="output"></a>

## <a name="event-hub-output-binding"></a>Event Hub-Ausgabebindung
Mit der Event Hub-Ausgabebindung werden Ereignisse in einen Event Hub-Ereignisstrom geschrieben. Um Ereignisse in einen Event Hub schreiben zu können, müssen Sie über eine Sendeberechtigung verfügen.

Die Ausgabebindung verwendet das folgende JSON-Objekte im `bindings`-Array von „function.json“:

```json
{
    "type": "eventHub",
    "name": "<Name of output parameter in function signature>",
    "path": "<Name of event hub>",
    "connection": "<Name of app setting with connection string - see below>"
    "direction": "out"
}
```

`connection` muss der Name einer App-Einstellung sein, die die Zeichenfolge für die Verbindung mit dem Namespace des Event Hubs enthält.
Kopieren Sie diese Verbindungszeichenfolge, indem Sie für den *Namespace* (nicht für den eigentlichen Event Hub) auf die Schaltfläche **Verbindungsinformationen** klicken. Diese Verbindungszeichenfolge muss über Sendeberechtigungen zum Senden der Nachricht an den Ereignisstrom verfügen.

## <a name="output-usage"></a>Ausgabeverwendung
Dieser Abschnitt veranschaulicht die Verwendung Ihrer Event Hub-Ausgabebindung in Ihrem Funktionscode.

Sie können Nachrichten an die konfigurierte Event Hub-Instanz mit den folgenden Parametertypen ausgeben:

* `out string`
* `ICollector<string>` (um mehrere Entitäten auszugeben)
* `IAsyncCollector<string>` (asynchrone Version von `ICollector<T>`)

<a name="outputsample"></a>

## <a name="output-sample"></a>Ausgabebeispiel
Angenommen, die folgende Event Hub-Ausgabebindung befindet sich im `bindings`-Array von „function.json“:

```json
{
    "type": "eventHub",
    "name": "outputEventHubMessage",
    "path": "myeventhub",
    "connection": "MyEventHubSend",
    "direction": "out"
}
```

Weitere Informationen finden Sie im sprachspezifischen Beispiel, in dem ein Ereignis in den Ereignisstrom geschrieben wird.

* [C#](#outcsharp)
* [F#](#outfsharp)
* [Node.js](#outnodejs)

<a name="outcsharp"></a>

### <a name="output-sample-in-c"></a>Ausgabebeispiel in C# #

```cs
using System;

public static void Run(TimerInfo myTimer, out string outputEventHubMessage, TraceWriter log)
{
    String msg = $"TimerTriggerCSharp1 executed at: {DateTime.Now}";
    log.Verbose(msg);   
    outputEventHubMessage = msg;
}
```

Alternative zum Erstellen mehrerer Nachrichten:

```cs
public static void Run(TimerInfo myTimer, ICollector<string> outputEventHubMessage, TraceWriter log)
{
    string message = $"Event Hub message created at: {DateTime.Now}";
    log.Info(message);
    outputEventHubMessage.Add("1 " + message);
    outputEventHubMessage.Add("2 " + message);
}
```

<a name="outfsharp"></a>

### <a name="output-sample-in-f"></a>Ausgabebeispiel in F# #

```fsharp
let Run(myTimer: TimerInfo, outputEventHubMessage: byref<string>, log: TraceWriter) =
    let msg = sprintf "TimerTriggerFSharp1 executed at: %s" DateTime.Now.ToString()
    log.Verbose(msg);
    outputEventHubMessage <- msg;
```

<a name="outnodejs"></a>

### <a name="output-sample-for-nodejs"></a>Ausgabebeispiel in Node.js

```javascript
module.exports = function (context, myTimer) {
    var timeStamp = new Date().toISOString();
    context.log('Event Hub message created at: ', timeStamp);   
    context.bindings.outputEventHubMessage = "Event Hub message created at: " + timeStamp;
    context.done();
};
```

Alternative zum Senden mehrerer Nachrichten:

```javascript
module.exports = function(context) {
    var timeStamp = new Date().toISOString();
    var message = 'Event Hub message created at: ' + timeStamp;

    context.bindings.outputEventHubMessage = [];

    context.bindings.outputEventHubMessage.push("1 " + message);
    context.bindings.outputEventHubMessage.push("2 " + message);
    context.done();
};
```

## <a name="next-steps"></a>Nächste Schritte
[!INCLUDE [next steps](../../includes/functions-bindings-next-steps.md)]

