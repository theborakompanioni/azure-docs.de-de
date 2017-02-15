---
title: Azure Storage-Warteschlangenbindungen in Azure Functions | Microsoft Docs
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
ms.date: 11/02/2016
ms.author: chrande
translationtype: Human Translation
ms.sourcegitcommit: 96f253f14395ffaf647645176b81e7dfc4c08935
ms.openlocfilehash: 36cf563a8318acb9371c48ba7d29e24694446e45


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

`connection` muss den Namen einer App-Einstellung enthalten, die eine Speicherverbindungs-Zeichenfolge enthält. Im Azure-Portal konfiguriert der Standard-Editor auf der Registerkarte **Integrieren** diese App-Einstellung für Sie, wenn Sie ein Storage-Konto erstellen oder ein vorhandenes auswählen. Wie Sie diese App-Einstellung manuell erstellen, erfahren Sie unter [Manuelles Konfigurieren dieser App-Einstellung]().

[Zusätzliche Einstellungen](https://github.com/Azure/azure-webjobs-sdk-script/wiki/host.json) können in einer host.json-Datei zur weiteren Feinabstimmung von Storage-Warteschlangentriggern angegeben werden.  

### <a name="handling-poison-queue-messages"></a>Behandeln von Nachrichten in der Warteschlange für nicht verarbeitbare Nachrichten
Wenn bei Ausführung einer Warteschlangentrigger-Funktion ein Fehler auftritt, versucht Azure Functions standardmäßig bis zu 5 Mal (einschließlich des ersten Versuchs), diese Funktion für eine bestimmte Warteschlangennachricht auszuführen. Wenn alle 5 Versuche nicht gelingen, fügt Functions einer Storage-Warteschlange eine Nachricht mit dem Namen *&lt;Originalwarteschlangename>-poison* hinzu. Sie können eine Funktion schreiben, um Nachrichten aus der Warteschlange für nicht verarbeitete Nachrichten zu verarbeiten, indem Sie diese protokollieren oder eine Benachrichtigung senden, dass ein manueller Eingriff erforderlich ist. 

Wenn Sie nicht verarbeitbare Nachrichten manuell verarbeiten möchten, können Sie durch Überprüfen von `dequeueCount` abrufen, wie oft eine Nachricht zur Verarbeitung entnommen wurde (siehe [Metadaten für Warteschlangentrigger](#meta)).

<a name="triggerusage"></a>

## <a name="trigger-usage"></a>Triggerverwendung
In C#-Funktionen stellen Sie mit einem benannten Parameter in Ihrer Funktionssignatur, z.B. `<T> <name>`, eine Bindung an die Eingabenachricht her.
Dabei ist `T` der Datentyp, in den Sie die Daten deserialisieren möchten, und `paramName` ist der Name, den Sie in der [Triggerbindung](#trigger) angegeben haben. In Node.js-Funktionen greifen Sie mit `context.bindings.<name>` auf die Eingabeblobdaten zu.

Die Warteschlangennachricht kann in alle folgenden Typen deserialisiert werden:

* Beliebiges [Objekt](https://msdn.microsoft.com/library/system.object.aspx) – nützlich für JSON-serialisierte Nachrichten.
  Wenn Sie einen benutzerdefinierten Eingabetyp deklarieren (z.B. `FooType`), versucht Azure Functions, die JSON-Daten in den angegebenen Typ zu deserialisieren.
* String
* Bytearray 
* `CloudQueueMessage` (C#) 

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
    context.log('Node.js queue trigger function processed work item' context.bindings.myQueueItem);
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

`connection` muss den Namen einer App-Einstellung enthalten, die eine Speicherverbindungs-Zeichenfolge enthält. Im Azure-Portal konfiguriert der Standard-Editor auf der Registerkarte **Integrieren** diese App-Einstellung für Sie, wenn Sie ein Storage-Konto erstellen oder ein vorhandenes auswählen. Wie Sie diese App-Einstellung manuell erstellen, erfahren Sie unter [Manuelles Konfigurieren dieser App-Einstellung]().

<a name="outputusage"></a>

## <a name="output-usage"></a>Ausgabeverwendung
In C#-Funktionen schreiben Sie eine Warteschlangennachricht mithilfe des benannten `out`-Parameters wie `out <T> <name>` in Ihrer Funktionssignatur, wobei `T` der Datentyp ist, in den Sie die Nachricht serialisieren möchten, und `paramName` der Name, den Sie in der [Ausgabebindung](#output) angegeben haben. In Node.js-Funktionen greifen Sie mit `context.bindings.<name>` auf die Ausgabe zu.

Sie können eine Warteschlangennachricht mit einem beliebigen Datentyp in Ihrem Code ausgeben:

* Beliebiges [Objekt](https://msdn.microsoft.com/library/system.object.aspx) – nützlich für JSON-Serialisierung.
  Wenn Sie einen benutzerdefinierten Ausgabetyp deklarieren (z.B. `out FooType paramName`), versucht Azure Functions, das Objekt in JSON zu serialisieren. Wenn der Ausgabeparameter NULL ist, wenn die Funktion beendet wird, erstellt die Functions-Laufzeit eine Warteschlangennachricht als NULL-Objekt.
* Zeichenfolge – (`out string paramName`) nützlich für Testnachrichten. Die Functions-Laufzeit erstellt nur dann eine Nachricht, wenn der Zeichenfolgenparameter bei Rückgabe durch die Funktion ungleich NULL ist.
* Bytearray – (`out byte[]`) 
* `out CloudQueueMessage` – nur C# 

In C# können Sie auch eine Bindung mit `ICollector<T>` oder `IAsyncCollector<T>` erstellen, wobei `T` einer der unterstützten Typen ist.

<a name="outputsample"></a>

## <a name="output-sample"></a>Ausgabebeispiel
Nehmen wir an, dass Sie über die folgende „function.json“ verfügen, die einen [Storage-Warteschlangentrigger](functions-bindings-storage-queue.md), eine Storage-Blobeingabe und eine Storage-Blobausgabe definiert:

Beispiel von *function.json* für eine Storage-Warteschlangen-Ausgabebindung, die einen Warteschlangentrigger verwendet und eine Warteschlangennachricht schreibt:

```json
{
  "bindings": [
    {
      "name": "myQueueItem",
      "queueName": "myqueue-items",
      "connection": "MyStorageConnection",
      "type": "queueTrigger",
      "direction": "in"
    },
    {
      "name": "myQueue",
      "queueName": "samples-workitems-out",
      "connection": "MyStorageConnection",
      "type": "queue",
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
public static void Run(string myQueueItem, out string myQueue, TraceWriter log)
{
    myQueue = myQueueItem + "(next step)";
}
```

Alternative zum Senden mehrerer Nachrichten:

```cs
public static void Run(string myQueueItem, ICollector<string> myQueue, TraceWriter log)
{
    myQueue.Add(myQueueItem + "(step 1)");
    myQueue.Add(myQueueItem + "(step 2)");
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
    context.bindings.myQueue = context.bindings.myQueueItem + "(next step)";
    context.done();
};
```

Alternative zum Senden mehrerer Nachrichten:

```javascript
module.exports = function(context) {
    context.bindings.myQueue = [];

    context.bindings.myQueueItem.push("(step 1)");
    context.bindings.myQueueItem.push("(step 2)");
    context.done();
};
```

## <a name="next-steps"></a>Nächste Schritte
[!INCLUDE [next steps](../../includes/functions-bindings-next-steps.md)]




<!--HONumber=Nov16_HO3-->


