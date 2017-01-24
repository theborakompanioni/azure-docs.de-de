---
title: Service Bus-Trigger und -Bindungen in Azure Functions | Microsoft Docs
description: Erfahren Sie, wie Azure Service Bus-Trigger und -Bindungen in Azure Functions verwendet werden.
services: functions
documentationcenter: na
author: christopheranderson
manager: erikre
editor: 
tags: 
keywords: Azure Functions, Funktionen, Ereignisverarbeitung, dynamisches Compute, serverlose Architektur
ms.assetid: daedacf0-6546-4355-a65c-50873e74f66b
ms.service: functions
ms.devlang: multiple
ms.topic: reference
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 10/31/2016
ms.author: chrande; glenga
translationtype: Human Translation
ms.sourcegitcommit: 7ff4286d6006eb362b6fba705e2afca1fd872f72
ms.openlocfilehash: afefa826577999897a537add7a6c6301144fa38c


---
# <a name="azure-functions-service-bus-bindings"></a>Service Bus-Bindungen von Azure Functions
[!INCLUDE [functions-selector-bindings](../../includes/functions-selector-bindings.md)]

Dieser Artikel erläutert das Konfigurieren und Codieren von Azure Service Bus-Bindungen in Azure Functions. Azure Functions unterstützt Trigger und Ausgabebindungen für Notification Hubs-Warteschlangen und -Themen.

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

<a name="trigger"></a>

## <a name="service-bus-trigger"></a>Service Bus-Trigger
Verwenden Sie den Service Bus-Trigger, um auf Nachrichten von einer Service Bus-Warteschlange oder einem Thema zu reagieren. 

Die Trigger von Notification Hubs-Warteschlange und -Thema zu einer Funktion verwenden die folgenden JSON-Objekte im `bindings`-Array von „function.json“:

* Trigger für *Warteschlange*:

    ```json
    {
        "name" : "<Name of input parameter in function signature>",
        "queueName" : "<Name of the queue>",
        "connection" : "<Name of app setting that has your queue's connection string - see below>",
        "accessRights" : "<Access rights for the connection string - see below>",
        "type" : "serviceBusTrigger",
        "direction" : "in"
    }
    ```

* Trigger für *Thema*:

    ```json
    {
        "name" : "<Name of input parameter in function signature>",
        "topicName" : "<Name of the topic>",
        "subscriptionName" : "<Name of the subscription>",
        "connection" : "<Name of app setting that has your topic's connection string - see below>",
        "accessRights" : "<Access rights for the connection string - see below>",
        "type" : "serviceBusTrigger",
        "direction" : "in"
    }
    ```

Beachten Sie Folgendes:

* [Erstellen Sie eine App-Einstellung in Ihrer Funktions-App]() für `connection`, die die Verbindungszeichenfolge zu Ihrem Service Hub-Namespace enthält, und geben Sie den Namen der App-Einstellung in Ihrem Trigger in der `connection`-Eigenschaft an. Um die Verbindungszeichenfolge zu erhalten, führen Sie die Schritte unter [Abrufen der Verwaltungsanmeldeinformationen](../service-bus-messaging/service-bus-dotnet-get-started-with-queues.md#obtain-the-management-credentials) aus.
  Die Verbindungszeichenfolge muss für einen Service Bus-Namespace gelten und darf nicht auf eine bestimmte Warteschlange oder ein Thema beschränkt sein.
  Wenn Sie `connection` leer lassen, setzt der Trigger voraus, dass eine standardmäßige Service Bus-Verbindungszeichenfolge in einer App-Einstellung mit dem Namen `AzureWebJobsServiceBus` angegeben ist.
* Verfügbare Werte für `accessRights` sind `manage` und `listen`. Die Standardeinstellung ist `manage`, d.h. heißt, dass die `connection` die Berechtigung **Manage** hat. Wenn Sie eine Verbindungszeichenfolge verwenden, die nicht die Berechtigung **Manage** hat, legen Sie `accessRights` auf `listen` fest. Andernfalls versucht die Functions-Laufzeit ggf. erfolglos Vorgänge, die Verwaltungsrechte erfordern.

## <a name="trigger-behavior"></a>Triggerverhalten
* **Single-Threading**: Die Functions-Laufzeit verarbeitet standardmäßig mehrere Nachrichten gleichzeitig. Um die Runtime anzuweisen, jeweils nur eine Warteschlangen- oder Themennachricht zu verarbeiten, legen Sie `serviceBus.maxConcurrrentCalls` in *host.json* auf „1“ fest. 
  Informationen zu *host.json* finden Sie unter [Ordnerstruktur](functions-reference.md#folder-structure) und [host.json](https://github.com/Azure/azure-webjobs-sdk-script/wiki/host.json).
* **Behandlung von nicht verarbeitbaren Nachrichten**: Service Bus kümmert sich selbst um die Handhabung nicht verarbeitbarer Nachrichten, die in Azure Functions-Konfigurationen und -Code nicht gesteuert oder konfiguriert werden kann. 
* **PeekLock-Verhalten**: Die Functions-Runtime empfängt eine Nachricht im [`PeekLock`-Modus](../service-bus-messaging/service-bus-performance-improvements.md#receive-mode) und ruft bei erfolgreicher Ausführung der Funktion `Complete` für die Nachricht auf. Tritt bei der Ausführung ein Fehler auf, wird `Abandon` aufgerufen. 
  Wenn die Funktion länger als im `PeekLock`-Timeout angegeben ausgeführt wird, wird die Sperre automatisch erneuert.

<a name="triggerusage"></a>

## <a name="trigger-usage"></a>Triggerverwendung
Dieser Abschnitt veranschaulicht die Verwendung Ihres Service Hub-Triggers in Ihrem Funktionscode. 

In C# und F# kann die Service Bus-Triggernachricht in alle folgenden Eingabetypen deserialisiert werden:

* `string` – nützlich für Zeichenfolgennachrichten
* `byte[]` – nützlich für binäre Daten
* Beliebiges [Objekt](https://msdn.microsoft.com/library/system.object.aspx) – nützlich für JSON-serialisierte Daten.
  Wenn Sie einen benutzerdefinierten Eingabetyp deklarieren (z.B. `FooType`), versucht Azure Functions, die JSON-Daten in den angegebenen Typ zu deserialisieren.
* `BrokeredMessage` – gibt Ihnen die deserialisierte Nachricht mit der [BrokeredMessage.GetBody<T>()](https://msdn.microsoft.com/library/hh144211.aspx)-Methode zurück.

In Node.js wird die Service Bus-Triggernachricht entweder als Zeichenfolge oder, im Fall der JSON-Nachricht, als JavaScript-Objekt an die Funktion übergeben.

<a name="triggersample"></a>

## <a name="trigger-sample"></a>Triggerbeispiel
Sie haben beispielsweise die folgende „function.json“:

```json
{
"bindings": [
    {
    "queueName": "testqueue",
    "connection": "MyServiceBusConnection",
    "name": "myQueueItem",
    "type": "serviceBusTrigger",
    "direction": "in"
    }
],
"disabled": false
}
```

Beachten Sie das sprachspezifische Beispiel für die Verarbeitung einer Service Bus-Warteschlangennachricht.

* [C#](#triggercsharp)
* [F#](#triggerfsharp)
* [Node.js](#triggernodejs)

<a name="triggercsharp"></a>

### <a name="trigger-sample-in-c"></a>Triggerbeispiel in C# #

```cs
public static void Run(string myQueueItem, TraceWriter log)
{
    log.Info($"C# ServiceBus queue trigger function processed message: {myQueueItem}");
}
```

<a name="triggerfsharp"></a>

### <a name="trigger-sample-in-f"></a>Triggerbeispiel in F# #

```fsharp
let Run(myQueueItem: string, log: TraceWriter) =
    log.Info(sprintf "F# ServiceBus queue trigger function processed message: %s" myQueueItem)
```

<a name="triggernodejs"></a>

### <a name="trigger-sample-in-nodejs"></a>Triggerbeispiel in Node.js

```javascript
module.exports = function(context, myQueueItem) {
    context.log('Node.js ServiceBus queue trigger function processed message', myQueueItem);
    context.done();
};
```

<a name="output"></a>

## <a name="service-bus-output-binding"></a>Service Bus-Ausgabebindung
Die Ausgaben von Notification Hubs-Warteschlange und -Thema für eine Funktion verwenden die folgenden JSON-Objekte im `bindings`-Array von „function.json“:

* Ausgabe der *Warteschlange*:

    ```json
    {
        "name" : "<Name of output parameter in function signature>",
        "queueName" : "<Name of the queue>",
        "connection" : "<Name of app setting that has your queue's connection string - see below>",
        "accessRights" : "<Access rights for the connection string - see below>"
        "type" : "serviceBus",
        "direction" : "out"
    }
    ```
* Ausgabe des *Themas*:

    ```json
    {
        "name" : "<Name of output parameter in function signature>",
        "topicName" : "<Name of the topic>",
        "subscriptionName" : "<Name of the subscription>",
        "connection" : "<Name of app setting that has your topic's connection string - see below>",
        "accessRights" : "<Access rights for the connection string - see below>"
        "type" : "serviceBus",
        "direction" : "out"
    }
    ```

Beachten Sie Folgendes:

* [Erstellen Sie eine App-Einstellung in Ihrer Funktions-App]() für `connection`, die die Verbindungszeichenfolge zu Ihrem Service Hub-Namespace enthält, und geben Sie den Namen der App-Einstellung in Ihrer Ausgabebindung in der `connection`-Eigenschaft an. Um die Verbindungszeichenfolge zu erhalten, führen Sie die Schritte unter [Abrufen der Verwaltungsanmeldeinformationen](../service-bus-messaging/service-bus-dotnet-get-started-with-queues.md#obtain-the-management-credentials) aus.
  Die Verbindungszeichenfolge muss für einen Service Bus-Namespace gelten und darf nicht auf eine bestimmte Warteschlange oder ein Thema beschränkt sein.
  Wenn Sie `connection` leer lassen, setzt die Ausgabebindung voraus, dass eine standardmäßige Service Bus-Verbindungszeichenfolge in einer App-Einstellung mit dem Namen `AzureWebJobsServiceBus` angegeben ist.
* Verfügbare Werte für `accessRights` sind `manage` und `listen`. Die Standardeinstellung ist `manage`, d.h. heißt, dass die `connection` die Berechtigung **Manage** hat. Wenn Sie eine Verbindungszeichenfolge verwenden, die nicht die Berechtigung **Manage** hat, legen Sie `accessRights` auf `listen` fest. Andernfalls versucht die Functions-Laufzeit ggf. erfolglos Vorgänge, die Verwaltungsrechte erfordern.

<a name="outputusage"></a>

## <a name="output-usage"></a>Ausgabeverwendung
In C# und F# kann Azure Functions eine Service Bus-Warteschlangennachricht mit einem der folgenden Typen erstellen:

* Beliebiges [Objekt](https://msdn.microsoft.com/library/system.object.aspx) – Parameterdefinition sieht wie `out T paramName` aus (C#).
  Functions deserialisiert das Objekt in eine JSON-Nachricht. Wenn der Ausgabewert NULL ist, wenn die Funktion beendet wird, erstellt Functions die Nachricht mit einem NULL-Objekt.
* `string` – Parameterdefinition sieht wie `out string paraName` aus (C#). Wenn der Parameterwert nicht NULL ist, sobald die Funktion beendet wird, erstellt Functions eine Nachricht.
* `byte[]` – Parameterdefinition sieht wie `out byte[] paraName` aus (C#). Wenn der Parameterwert nicht NULL ist, sobald die Funktion beendet wird, erstellt Functions eine Nachricht.
* `BrokeredMessage` – Parameterdefinition sieht wie `out BrokeredMessage paraName` aus (C#). Wenn der Parameterwert nicht NULL ist, sobald die Funktion beendet wird, erstellt Functions eine Nachricht.

Für das Erstellen mehrerer Nachrichten in einer C#-Funktion können Sie `ICollector<T>` oder `IAsyncCollector<T>` verwenden. Beim Aufrufen der `Add` -Methode wird eine Nachricht erstellt.

In Node.js können Sie `context.binding.<paramName>` eine Zeichenfolge, ein Bytearray oder ein Javascript-Objekt (deserialisiert in JSON) zuweisen.

<a name="outputsample"></a>

## <a name="output-sample"></a>Ausgabebeispiel
Nehmen wir an, dass Sie über die folgende „function.json“ verfügen, die eine Service Bus-Warteschlangenausgabe definiert:

```json
{
    "bindings": [
        {
            "schedule": "0/15 * * * * *",
            "name": "myTimer",
            "runsOnStartup": true,
            "type": "timerTrigger",
            "direction": "in"
        },
        {
            "name": "outputSbQueue",
            "type": "serviceBus",
            "queueName": "testqueue",
            "connection": "MyServiceBusConnection",
            "direction": "out"
        }
    ],
    "disabled": false
}
```

Beachten Sie das sprachspezifische Beispiel für das Senden einer Nachricht an die Service Bus-Warteschlange.

* [C#](#outcsharp)
* [F#](#outfsharp)
* [Node.js](#outnodejs)

<a name="outcsharp"></a>

### <a name="output-sample-in-c"></a>Ausgabebeispiel in C# #

```cs
public static void Run(TimerInfo myTimer, TraceWriter log, out string outputSbQueue)
{
    string message = $"Service Bus queue message created at: {DateTime.Now}";
    log.Info(message); 
    outputSbQueue = message;
}
```

Alternative zum Erstellen mehrerer Nachrichten:

```cs
public static void Run(TimerInfo myTimer, TraceWriter log, ICollector<string> outputSbQueue)
{
    string message = $"Service Bus queue message created at: {DateTime.Now}";
    log.Info(message); 
    outputSbQueue.Add("1 " + message);
    outputSbQueue.Add("2 " + message);
}
```

<a name="outfsharp"></a>

### <a name="output-sample-in-f"></a>Ausgabebeispiel in F# #

```fsharp
let Run(myTimer: TimerInfo, log: TraceWriter, outputSbQueue: byref<string>) =
    let message = sprintf "Service Bus queue message created at: %s" (DateTime.Now.ToString())
    log.Info(message)
    outputSbQueue = message
```

<a name="outnodejs"></a>

### <a name="output-sample-in-nodejs"></a>Ausgabebeispiel in Node.js

```javascript
module.exports = function (context, myTimer) {
    var message = 'Service Bus queue message created at ' + timeStamp;
    context.log(message);   
    context.bindings.outputSbQueueMsg = message;
    context.done();
};
```

Alternative zum Erstellen mehrerer Nachrichten:

```javascript
module.exports = function (context, myTimer) {
    var message = 'Service Bus queue message created at ' + timeStamp;
    context.log(message);   
    context.bindings.outputSbQueueMsg = [];
    context.bindings.outputSbQueueMsg.push("1 " + message);
    context.bindings.outputSbQueueMsg.push("2 " + message);
    context.done();
};
```

## <a name="next-steps"></a>Nächste Schritte
[!INCLUDE [next steps](../../includes/functions-bindings-next-steps.md)]




<!--HONumber=Jan17_HO2-->


