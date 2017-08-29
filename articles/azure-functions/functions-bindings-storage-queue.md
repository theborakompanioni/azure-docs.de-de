---
title: "Azure Functions – Queue Storage-Bindungen | Microsoft-Dokumentation"
description: Erfahren Sie, wie Azure Storage-Trigger und -Bindungen in Azure Functions verwendet werden.
services: functions
documentationcenter: na
author: lindydonna
manager: cfowler
editor: 
tags: 
keywords: Azure Functions, Funktionen, Ereignisverarbeitung, dynamisches Compute, serverlose Architektur
ms.assetid: 4e6a837d-e64f-45a0-87b7-aa02688a75f3
ms.service: functions
ms.devlang: multiple
ms.topic: reference
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 05/30/2017
ms.author: glenga
ms.translationtype: HT
ms.sourcegitcommit: 94d1d4c243bede354ae3deba7fbf5da0652567cb
ms.openlocfilehash: f39f674bf576a2661a0e03710b9005b0515b3aa5
ms.contentlocale: de-de
ms.lasthandoff: 07/18/2017

---
# <a name="azure-functions-queue-storage-bindings"></a>Azure Functions – Queue Storage-Bindungen
[!INCLUDE [functions-selector-bindings](../../includes/functions-selector-bindings.md)]

Dieser Artikel erläutert das Konfigurieren und Codieren von Azure Queue Storage-Bindungen in Azure Functions. Azure Functions unterstützt Trigger- und Ausgabebindungen für Azure-Warteschlangen. Features, die in allen Bindungen verfügbar sind, finden Sie unter [Konzepte für Azure Functions-Trigger und -Bindungen](functions-triggers-bindings.md).

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

<a name="trigger"></a>

## <a name="queue-storage-trigger"></a>Queue Storage-Trigger
Mit dem Azure Queue Storage-Trigger können Sie eine Queue Storage-Instanz auf neue Nachrichten überwachen und darauf reagieren. 

Sie definieren einen Warteschlangentrigger auf der Registerkarte **Integrieren** im Functions-Portal. Das Portal erstellt die folgende Definition im Abschnitt **Bindungen** der Datei *function.json*:

```json
{
    "type": "queueTrigger",
    "direction": "in",
    "name": "<The name used to identify the trigger data in your code>",
    "queueName": "<Name of queue to poll>",
    "connection":"<Name of app setting - see below>"
}
```

* Die `connection`-Eigenschaft muss den Namen einer App-Einstellung enthalten, die eine Speicherverbindungszeichenfolge enthält. Im Azure-Portal konfiguriert der Standard-Editor auf der Registerkarte **Integrieren** diese App-Einstellung für Sie, wenn Sie ein Speicherkonto auswählen.

Zusätzliche Einstellungen können in der Datei [host.json](https://github.com/Azure/azure-webjobs-sdk-script/wiki/host.json) zur weiteren Feinabstimmung von Queue Storage-Triggern angegeben werden. Sie können beispielsweise das Abrufintervall für die Warteschlange in „host.json“ ändern.

<a name="triggerusage"></a>

## <a name="using-a-queue-trigger"></a>Verwenden von Warteschlangentriggern
In Node.js-Funktionen greifen Sie mit `context.bindings.<name>` auf die Warteschlangendaten zu.


In .NET-Funktionen greifen Sie über einen Methodenparameter wie `CloudQueueMessage paramName` auf die Warteschlangennutzlast zu. Hier ist `paramName` der Wert, den Sie bei der [Triggerkonfiguration](#trigger) angegeben haben. Die Warteschlangennachricht kann in alle folgenden Typen deserialisiert werden:

* POCO-Objekt. Verwenden Sie dieses, wenn die Warteschlangennutzlast ein JSON-Objekt ist. Die Functions-Runtime deserialisiert die Nutzlast im POCO-Objekt. 
* `string`
* `byte[]`
* [`CloudQueueMessage`]

<a name="meta"></a>

### <a name="queue-trigger-metadata"></a>Metadaten für Warteschlangentrigger
Der Warteschlangentrigger stellt mehrere Metadateneigenschaften bereit. Diese Eigenschaften können als Teil der Bindungsausdrücke in anderen Bindungen oder als Parameter im Code verwendet werden. Diese Werte haben die gleiche Semantik wie [`CloudQueueMessage`].

* **QueueTrigger** – die Warteschlangennutzlast (bei einer gültigen Zeichenfolge)
* **DequeueCount** – Typ `int`. Gibt an, wie oft diese Nachricht aus der Warteschlange entfernt wurde.
* **ExpirationTime** – Typ `DateTimeOffset?`. Die Zeit, zu der die Nachricht abläuft.
* **Id** – Typ `string`. ID der Warteschlangennachricht
* **InsertionTime** – Typ `DateTimeOffset?`. Die Zeit, zu der die Nachricht der Warteschlange hinzugefügt wurde.
* **NextVisibleTime** – Typ `DateTimeOffset?`. Die Zeit, zu der die Nachricht als Nächstes sichtbar wird.
* **PopReceipt** – Typ `string`. Die POP-Bestätigung der Nachricht.

Weitere Informationen dazu, wie Sie die Metadaten von Warteschlangen verwenden, finden Sie im [Triggerbeispiel](#triggersample).

<a name="triggersample"></a>

## <a name="trigger-sample"></a>Triggerbeispiel
Angenommen, Sie verfügen über die folgende „function.json“, die einen Warteschlangentrigger definiert:

```json
{
    "disabled": false,
    "bindings": [
        {
            "type": "queueTrigger",
            "direction": "in",
            "name": "myQueueItem",
            "queueName": "myqueue-items",
            "connection":"MyStorageConnectionString"
        }
    ]
}
```

Beachten Sie das sprachspezifische Beispiel für das Abrufen und Protokollieren von Warteschlangen-Metadaten.

* [C#](#triggercsharp)
* [Node.js](#triggernodejs)

<a name="triggercsharp"></a>

### <a name="trigger-sample-in-c"></a>Triggerbeispiel in C# #
```csharp
#r "Microsoft.WindowsAzure.Storage"

using Microsoft.WindowsAzure.Storage.Queue;
using System;

public static void Run(CloudQueueMessage myQueueItem, 
    DateTimeOffset expirationTime, 
    DateTimeOffset insertionTime, 
    DateTimeOffset nextVisibleTime,
    string queueTrigger,
    string id,
    string popReceipt,
    int dequeueCount,
    TraceWriter log)
{
    log.Info($"C# Queue trigger function processed: {myQueueItem.AsString}\n" +
        $"queueTrigger={queueTrigger}\n" +
        $"expirationTime={expirationTime}\n" +
        $"insertionTime={insertionTime}\n" +
        $"nextVisibleTime={nextVisibleTime}\n" +
        $"id={id}\n" +
        $"popReceipt={popReceipt}\n" + 
        $"dequeueCount={dequeueCount}");
}
```

<!--
<a name="triggerfsharp"></a>
### Trigger sample in F# ## 
```fsharp

```
-->

<a name="triggernodejs"></a>

### <a name="trigger-sample-in-nodejs"></a>Triggerbeispiel in Node.js

```javascript
module.exports = function (context) {
    context.log('Node.js queue trigger function processed work item', context.bindings.myQueueItem);
    context.log('queueTrigger =', context.bindingData.queueTrigger);
    context.log('expirationTime =', context.bindingData.expirationTime);
    context.log('insertionTime =', context.bindingData.insertionTime);
    context.log('nextVisibleTime =', context.bindingData.nextVisibleTime);
    context.log('id=', context.bindingData.id);
    context.log('popReceipt =', context.bindingData.popReceipt);
    context.log('dequeueCount =', context.bindingData.dequeueCount);
    context.done();
};
```

### <a name="handling-poison-queue-messages"></a>Behandeln von Nachrichten in der Warteschlange für nicht verarbeitbare Nachrichten
Wenn eine Warteschlangentrigger-Funktion fehlschlägt, versucht Azure Functions bis zu fünf Mal (einschließlich des ersten Versuchs), diese Funktion für eine bestimmte Warteschlangennachricht auszuführen. Wenn bei allen fünf Versuchen Fehler auftreten, fügt die Functions-Runtime der Queue Storage-Instanz *&lt;originalqueuename>-poison* eine Nachricht hinzu. Sie können eine Funktion schreiben, um Nachrichten aus der Warteschlange für nicht verarbeitete Nachrichten zu verarbeiten, indem Sie diese protokollieren oder eine Benachrichtigung senden, dass ein manueller Eingriff erforderlich ist. 

Für die manuelle Behandlung nicht verarbeitbarer Nachrichten überprüfen Sie den `dequeueCount` der Warteschlangennachricht (siehe [Metadaten für Warteschlangentrigger](#meta)).

<a name="output"></a>

## <a name="queue-storage-output-binding"></a>Queue Storage-Ausgabebindung
Die Azure Queue Storage-Ausgabebindung ermöglicht Ihnen das Schreiben von Nachrichten in eine Warteschlange. 

Sie definieren eine Warteschlangen-Ausgabebindung auf der Registerkarte **Integrieren** im Functions-Portal. Das Portal erstellt die folgende Definition im Abschnitt **Bindungen** der Datei *function.json*:

```json
{
   "type": "queue",
   "direction": "out",
   "name": "<The name used to identify the trigger data in your code>",
   "queueName": "<Name of queue to write to>",
   "connection":"<Name of app setting - see below>"
}
```

* Die `connection`-Eigenschaft muss den Namen einer App-Einstellung enthalten, die eine Speicherverbindungszeichenfolge enthält. Im Azure-Portal konfiguriert der Standard-Editor auf der Registerkarte **Integrieren** diese App-Einstellung für Sie, wenn Sie ein Speicherkonto auswählen.

<a name="outputusage"></a>

## <a name="using-a-queue-output-binding"></a>Verwenden von Warteschlangen-Ausgabebindungen
In Node.js-Funktionen greifen Sie mit `context.bindings.<name>` auf die Ausgabewarteschlange zu.

In .NET-Funktionen können Sie eine Ausgabe in einen der folgenden Typen erstellen. Beim Typparameter `T` muss `T` einer der unterstützten Ausgabetypen sein, z.B. `string` oder ein POCO-Objekt.

* `out T` (serialisiert as JSON)
* `out string`
* `out byte[]`
* `out` [`CloudQueueMessage`] 
* `ICollector<T>`
* `IAsyncCollector<T>`
* [`CloudQueue`](/dotnet/api/microsoft.windowsazure.storage.queue.cloudqueue)

Sie können auch den Rückgabetyp der Methode als Ausgabebindung verwenden.

<a name="outputsample"></a>

## <a name="queue-output-sample"></a>Beispiel für eine Warteschlangenausgabe
Die folgende Datei *function.json* definiert einen HTTP-Trigger mit einer Warteschlangen-Ausgabebindung:

```json
{
  "bindings": [
    {
      "type": "httpTrigger",
      "direction": "in",
      "authLevel": "function",
      "name": "input"
    },
    {
      "type": "http",
      "direction": "out",
      "name": "return"
    },
    {
      "type": "queue",
      "direction": "out",
      "name": "$return",
      "queueName": "outqueue",
      "connection": "MyStorageConnectionString",
    }
  ]
}
``` 

Weitere Informationen finden Sie im sprachspezifischen Beispiel, das eine Warteschlangennachricht mit der eingehenden HTTP-Nutzlast ausgibt.

* [C#](#outcsharp)
* [Node.js](#outnodejs)

<a name="outcsharp"></a>

### <a name="queue-output-sample-in-c"></a>Beispiel für eine Warteschlangenausgabe in C# #

```cs
// C# example of HTTP trigger binding to a custom POCO, with a queue output binding
public class CustomQueueMessage
{
    public string PersonName { get; set; }
    public string Title { get; set; }
}

public static CustomQueueMessage Run(CustomQueueMessage input, TraceWriter log)
{
    return input;
}
```

Verwenden Sie zum Senden mehrerer Nachrichten einen `ICollector`:

```cs
public static void Run(CustomQueueMessage input, ICollector<CustomQueueMessage> myQueueItem, TraceWriter log)
{
    myQueueItem.Add(input);
    myQueueItem.Add(new CustomQueueMessage { PersonName = "You", Title = "None" });
}
```

<a name="outnodejs"></a>

### <a name="queue-output-sample-in-nodejs"></a>Beispiel für eine Warteschlangenausgabe in Node.js

```javascript
module.exports = function (context, input) {
    context.done(null, input.body);
};
```

Alternative zum Senden mehrerer Nachrichten:

```javascript
module.exports = function(context) {
    // Define a message array for the myQueueItem output binding. 
    context.bindings.myQueueItem = ["message 1","message 2"];
    context.done();
};
```

## <a name="next-steps"></a>Nächste Schritte

Ein Beispiel für eine Funktion, die Queue Storage-Trigger und -Bindungen verwendet, finden Sie unter [Erstellen einer Azure-Funktion, die mit einem Azure-Dienst verbunden ist](functions-create-an-azure-connected-function.md).

[!INCLUDE [next steps](../../includes/functions-bindings-next-steps.md)]

<!-- LINKS -->

[`CloudQueueMessage`]: /dotnet/api/microsoft.windowsazure.storage.queue.cloudqueuemessage

