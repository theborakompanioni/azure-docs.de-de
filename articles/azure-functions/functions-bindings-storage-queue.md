---
title: Azure Storage-Warteschlangenbindungen in Azure Functions | Microsoft-Dokumentation
description: Erfahren Sie, wie Azure Storage-Trigger und -Bindungen in Azure Functions verwendet werden.
services: functions
documentationcenter: na
author: christopheranderson
manager: erikre
editor: 
tags: 
keywords: Azure Functions, Funktionen, Ereignisverarbeitung, dynamisches Compute, serverlose Architektur
ms.assetid: 4e6a837d-e64f-45a0-87b7-aa02688a75f3
ms.service: functions
ms.devlang: multiple
ms.topic: reference
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 01/18/2017
ms.author: chrande, glenga
translationtype: Human Translation
ms.sourcegitcommit: 770cac8809ab9f3d6261140333ec789ee1390daf
ms.openlocfilehash: bf9bd2a1b5acdf5a4a4f862bef693f8c60c63a33


---
# <a name="azure-functions-storage-queue-bindings"></a>Azure Storage-Warteschlangenbindungen in Azure Functions
[!INCLUDE [functions-selector-bindings](../../includes/functions-selector-bindings.md)]

Dieser Artikel erläutert das Konfigurieren und Codieren von Azure Storage-Warteschlangenbindungen in Azure Functions. Azure Functions unterstützt Trigger und Ausgabebindungen für Azure Storage-Warteschlangen.

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

<a name="trigger"></a>

## <a name="storage-queue-trigger"></a>Storage-Warteschlangentrigger
Mit dem Azure Storage-Warteschlangentrigger können Sie eine Speicherwarteschlange auf neue Nachrichten überwachen und darauf reagieren. 

Der Storage-Warteschlangentrigger zu einer Funktion verwendet die folgenden JSON-Objekte im `bindings`-Array von „function.json“:

```json
{
    "name": "<Name of input parameter in function signature>",
    "queueName": "<Name of queue to poll>",
    "connection":"<Name of app setting - see below>",
    "type": "queueTrigger",
    "direction": "in"
}
```

`connection` muss den Namen einer App-Einstellung enthalten, die eine Speicherverbindungszeichenfolge enthält. Im Azure-Portal können Sie diese App-Einstellung auf der Registerkarte **Integrieren** konfigurieren, wenn Sie ein Speicherkonto erstellen oder ein vorhandenes auswählen. Informationen zum manuellen Erstellen dieser App-Einstellung finden Sie unter [App Service-Einstellungen verwalten](functions-how-to-use-azure-function-app-settings.md#manage-app-service-settings).

[Zusätzliche Einstellungen](https://github.com/Azure/azure-webjobs-sdk-script/wiki/host.json) können in einer host.json-Datei zur weiteren Feinabstimmung von Storage-Warteschlangentriggern angegeben werden.  

### <a name="handling-poison-queue-messages"></a>Behandeln von Nachrichten in der Warteschlange für nicht verarbeitbare Nachrichten
Wenn eine Warteschlangentrigger-Funktion fehlschlägt, versucht Azure Functions bis zu fünf Mal (einschließlich des ersten Versuchs), diese Funktion für eine bestimmte Warteschlangennachricht auszuführen. Wenn alle fünf Versuche misslingen, fügt Functions einer Storage-Warteschlange eine Nachricht mit dem Namen *&lt;originalqueuename>-poison* hinzu. Sie können eine Funktion schreiben, um Nachrichten aus der Warteschlange für nicht verarbeitete Nachrichten zu verarbeiten, indem Sie diese protokollieren oder eine Benachrichtigung senden, dass ein manueller Eingriff erforderlich ist. 

Zur manuellen Verarbeitung von nicht verarbeitbaren Nachrichten können Sie durch Überprüfen von `dequeueCount` abrufen, wie oft eine Nachricht zur Verarbeitung entnommen wurde (siehe [Metadaten für Warteschlangentrigger](#meta)).

<a name="triggerusage"></a>

## <a name="trigger-usage"></a>Triggerverwendung
In C#-Funktionen stellen Sie mit einem benannten Parameter in Ihrer Funktionssignatur, z.B. `<T> <name>`, eine Bindung an die Eingabenachricht her.
Dabei ist `T` der Datentyp, in den Sie die Daten deserialisieren möchten, und `paramName` ist der Name, den Sie in der [Triggerbindung](#trigger) angegeben haben. In Node.js-Funktionen greifen Sie mit `context.bindings.<name>` auf die Eingabeblobdaten zu.

Die Warteschlangennachricht kann in alle folgenden Typen deserialisiert werden:

* [Objekt](https://msdn.microsoft.com/library/system.object.aspx): Wird für serialisierte JSON-Nachrichten verwendet. Wenn Sie einen benutzerdefinierten Eingabetyp deklarieren, versucht die Laufzeit, das JSON-Objekt zu deserialisieren. 
* String
* Bytearray
* [CloudQueueMessage](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.queue.cloudqueuemessage.aspx) (nur C#)

<a name="meta"></a>

### <a name="queue-trigger-metadata"></a>Metadaten für Warteschlangentrigger
Sie können Warteschlangen-Metadaten in Ihrer Funktion mithilfe dieser Variablennamen abrufen:

* expirationTime
* insertionTime
* nextVisibleTime
* id
* popReceipt
* dequeueCount
* queueTrigger (eine weitere Möglichkeit, den Text der Warteschlangennachricht als Zeichenfolge abzurufen)

Wie Sie Warteschlangen-Metadaten verwenden, erfahren Sie unter [Triggerbeispiel](#triggersample).

<a name="triggersample"></a>

## <a name="trigger-sample"></a>Triggerbeispiel
Nehmen wir an, dass Sie über die folgende „function.json“ verfügen, die einen Storage-Warteschlangentrigger definiert:

```json
{
    "disabled": false,
    "bindings": [
        {
            "name": "myQueueItem",
            "queueName": "myqueue-items",
            "connection":"",
            "type": "queueTrigger",
            "direction": "in"
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
public static void Run(string myQueueItem, 
    DateTimeOffset expirationTime, 
    DateTimeOffset insertionTime, 
    DateTimeOffset nextVisibleTime,
    string queueTrigger,
    string id,
    string popReceipt,
    int dequeueCount,
    TraceWriter log)
{
    log.Info($"C# Queue trigger function processed: {myQueueItem}\n" +
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

<a name="output"></a>

## <a name="storage-queue-output-binding"></a>Storage-Warteschlangen-Ausgabebindung
Die Azure Storage-Warteschlangen-Ausgabebindung ermöglicht Ihnen, in Ihrer Funktion Nachrichten in eine Storage-Warteschlange zu schreiben. 

Die Storage-Warteschlangenausgabe für eine Funktion verwendet die folgenden JSON-Objekte im `bindings`-Array von „function.json“:

```json
{
  "name": "<Name of output parameter in function signature>",
    "queueName": "<Name of queue to write to>",
    "connection":"<Name of app setting - see below>",
  "type": "queue",
  "direction": "out"
}
```

`connection` muss den Namen einer App-Einstellung enthalten, die eine Speicherverbindungs-Zeichenfolge enthält. Im Azure-Portal konfiguriert der Standard-Editor auf der Registerkarte **Integrieren** diese App-Einstellung für Sie, wenn Sie ein Speicherkonto erstellen oder ein vorhandenes auswählen. Informationen zum manuellen Erstellen dieser App-Einstellung finden Sie unter [App Service-Einstellungen verwalten](functions-how-to-use-azure-function-app-settings.md#manage-app-service-settings).

<a name="outputusage"></a>

## <a name="output-usage"></a>Ausgabeverwendung
In C#-Funktionen schreiben Sie eine Warteschlangennachricht mithilfe des benannten `out`-Parameters in Ihrer Funktionssignatur, z.B. `out <T> <name>`. In diesem Fall ist `T` der Datentyp, in den Sie die Nachricht serialisieren möchten, und `paramName` ist der Name, den Sie in der [Ausgabebindung](#output) angegeben haben. In Node.js-Funktionen greifen Sie mit `context.bindings.<name>` auf die Ausgabe zu.

Sie können eine Warteschlangennachricht mit einem beliebigen Datentyp in Ihrem Code ausgeben:

* Beliebiges [Objekt](https://msdn.microsoft.com/library/system.object.aspx): `out MyCustomType paramName`  
Für die JSON-Serialisierung verwendet.  Wenn Sie einen benutzerdefinierten Ausgabetyp deklarieren, versucht die Laufzeit, das Objekt in JSON zu serialisieren. Wenn der Ausgabeparameter beim Beenden der Funktion NULL ist, erstellt die Laufzeit eine Warteschlangennachricht als NULL-Objekt.
* Zeichenfolge: `out string paramName`  
Für Testnachrichten verwendet. Die Laufzeit erstellt nur dann eine Nachricht, wenn der Zeichenfolgenparameter bei Rückgabe durch die Funktion ungleich NULL ist.
* Bytearray: `out byte[]` 

Diese zusätzliche Ausgabetypen werden von einer C#-Funktion unterstützt:

* [CloudQueueMessage](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.queue.cloudqueuemessage.aspx): `out CloudQueueMessage` 
* `ICollector<T>` oder `IAsyncCollector<T>`, wobei `T` einer der unterstützten Typen ist.

<a name="outputsample"></a>

## <a name="output-sample"></a>Ausgabebeispiel
Nehmen wir an, dass Sie über die folgende Datei „function.json“ verfügen, die einen [Storage-Warteschlangentrigger](functions-bindings-storage-queue.md), eine Storage-Blobeingabe und eine Storage-Blobausgabe definiert:

*function.json*-Beispiel für eine Storage-Warteschlangen-Ausgabebindung, die einen manuellen Trigger verwendet und die Eingabe in eine Warteschlangennachricht schreibt:

```json
{
  "bindings": [
    {
      "type": "manualTrigger",
      "direction": "in",
      "name": "input"
    },
    {
      "type": "queue",
      "name": "myQueueItem",
      "queueName": "myqueue",
      "connection": "my_storage_connection",
      "direction": "out"
    }
  ],
  "disabled": false
}
``` 

Beachten Sie das sprachspezifische Beispiel für das Schreiben einer Ausgabewarteschlangen-Nachricht für jede Eingabewarteschlangen-Nachricht.

* [C#](#outcsharp)
* [Node.js](#outnodejs)

<a name="outcsharp"></a>

### <a name="output-sample-in-c"></a>Ausgabebeispiel in C# #

```cs
public static void Run(string input, out string myQueueItem, TraceWriter log)
{
    myQueueItem = "New message: " + input;
}
```

Alternative zum Senden mehrerer Nachrichten:

```cs
public static void Run(string input, ICollector<string> myQueueItem, TraceWriter log)
{
    myQueueItem.Add("Message 1: " + input);
    myQueueItem.Add("Message 2: " + "Some other message.");
}
```

<!--
<a name="outfsharp"></a>
### Output sample in F# ## 
```fsharp

```
-->

<a name="outnodejs"></a>

### <a name="output-sample-in-nodejs"></a>Ausgabebeispiel in Node.js

```javascript
module.exports = function(context) {
    // Define a new message for the myQueueItem output binding.
    context.bindings.myQueueItem = "new message";
    context.done();
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

Ein Beispiel für eine Funktion, die Storage-Warteschlangentrigger und Bindungen verwendet, finden Sie unter [Erstellen einer Azure Functions-Funktion, die mit einem Azure-Dienst verbunden ist](functions-create-an-azure-connected-function.md).

[!INCLUDE [next steps](../../includes/functions-bindings-next-steps.md)]




<!--HONumber=Jan17_HO3-->


