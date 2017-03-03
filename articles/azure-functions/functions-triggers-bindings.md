---
title: Arbeiten mit Triggern und Bindungen in Azure Functions | Microsoft-Dokumentation
description: "Erfahren Sie, wie Sie Trigger und Bindungen in Azure Functions verwenden, um die Verbindung Ihrer Codeausführung mit Onlineereignissen und cloudbasierten Diensten herzustellen."
services: functions
documentationcenter: na
author: christopheranderson
manager: erikre
editor: 
tags: 
keywords: Azure Functions, Functions, Ereignisverarbeitung, Webhooks, dynamisches Compute, serverlose Architektur
ms.assetid: cbc7460a-4d8a-423f-a63e-1cd33fef7252
ms.service: functions
ms.devlang: multiple
ms.topic: reference
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 01/23/2017
ms.author: chrande
translationtype: Human Translation
ms.sourcegitcommit: b7ea1e3a72a9dc6f4f9ca9d2d9d6f8c9b1564972
ms.openlocfilehash: 947f1f5e6d9bebe6708e6d29d3b71efc09573537
ms.lasthandoff: 01/30/2017


---

# <a name="learn-how-to-work-with-triggers-and-bindings-in-azure-functions"></a>Erfahren Sie, wie Sie in Azure Functions mit Triggern und Bindungen arbeiten 
In diesem Thema wird gezeigt, wie Sie Trigger und Bindungen in Azure Functions verwenden, um die Verbindung Ihres Codes mit einer Reihe von Triggern, Azure-Diensten und anderen cloudbasierten Diensten herzustellen. Es werden einige erweiterte Bindungsfeatures und Syntaxelemente beschrieben, die von allen Bindungstypen unterstützt werden.  

Ausführliche Informationen zum Arbeiten mit einem bestimmten Trigger- oder Bindungstyp finden Sie in einem der folgenden Referenzthemen:

| | | | |  
| --- | --- | --- | --- |  
| [HTTP/Webhook](functions-bindings-http-webhook.md) | [Zeitgeber](functions-bindings-timer.md) | [Mobile Apps](functions-bindings-mobile-apps.md) | [Service Bus](functions-bindings-service-bus.md)  |  
| [DocumentDB](functions-bindings-documentdb.md) |  [Storage Blob](functions-bindings-storage-blob.md) | [Speicherwarteschlange](functions-bindings-storage-queue.md) |  [Storage-Tabelle](functions-bindings-storage-table.md) |  
| [Event Hubs](functions-bindings-event-hubs.md) | [Notification Hubs](functions-bindings-notification-hubs.md) | [Twilio](functions-bindings-twilio.md) |   
| | | | |  

In diesen Artikeln wird davon ausgegangen, dass Sie die [Entwicklerreferenz zu Azure Functions](functions-reference.md) sowie die Referenzartikel zu [C#](functions-reference-csharp.md), [F#](functions-reference-fsharp.md) bzw. [Node.js](functions-reference-node.md) für Entwickler gelesen haben.

## <a name="overview"></a>Übersicht
Bei Triggern handelt es sich um Ereignisantworten, die zum Auslösen Ihres benutzerdefinierten Codes verwendet werden. Sie können damit auf Ereignisse auf der gesamten Azure-Plattform oder lokale Ereignisse reagieren. Bindungen sind die erforderlichen Metadaten, die zum Herstellen der Verbindung für Ihren Code mit dem gewünschten Trigger oder den dazugehörigen Eingabe- oder Ausgabedaten verwendet werden. Die Datei *function.json* einer Funktion enthält jeweils alle zugehörigen Bindungen. Es gibt keine Beschränkung der Anzahl der Eingabe- und Ausgabebindungen, die eine Funktion enthalten kann. Es wird jedoch nur eine Trigger-Bindung pro Funktion unterstützt.  

Sehen Sie sich die folgende Tabelle an, um sich über die verschiedenen Bindungen zu informieren, die Sie in Ihre Azure-Funktionen-App integrieren können.

[!INCLUDE [dynamic compute](../../includes/functions-bindings.md)]

Stellen Sie sich zum besseren allgemeinen Verständnis von Triggern und Bindungen beispielsweise vor, dass Sie Code ausführen möchten, um ein neues Element aus einer Azure Storage-Warteschlange zu verarbeiten. Azure Functions verfügt über einen Azure-Warteschlangentrigger zur Unterstützung dieses Vorgangs. Zum Überwachen der Warteschlange benötigen Sie die folgenden Informationen:

* Das Speicherkonto, unter dem sich die Warteschlange befindet.
* Den Namen der Warteschlange.
* Einen Variablennamen, der von Ihrem Code genutzt wird, um auf das neue Element zu verweisen, das in die Warteschlange eingereiht wurde.  

Eine Warteschlangentrigger-Bindung enthält diese Informationen für eine Azure-Funktion. Hier ist ein Beispiel für die Datei *function.json* mit einer Warteschlangentrigger-Bindung angegeben. 

```json
{
  "bindings": [
    {
      "name": "myNewUserQueueItem",
      "type": "queueTrigger",
      "direction": "in",
      "queueName": "queue-newusers",
      "connection": "MY_STORAGE_ACCT_APP_SETTING"
    }
  ],
  "disabled": false
}
```

Ihr Code sendet unter Umständen unterschiedliche Arten von Ausgaben. Dies richtet sich danach, wie das neue Warteschlangenelement verarbeitet wird. Es kann beispielsweise sein, dass Sie einen neuen Datensatz in eine Azure Storage-Tabelle schreiben möchten.  Hierzu erstellen Sie eine Ausgabebindung an eine Azure Storage-Tabelle. Hier ist ein Beispiel für die Datei *function.json* mit einer Speichertabellen-Ausgabebindung angegeben, die mit einem Warteschlangentrigger verwendet werden kann. 

```json
{
  "bindings": [
    {
      "name": "myNewUserQueueItem",
      "type": "queueTrigger",
      "direction": "in",
      "queueName": "queue-newusers",
      "connection": "MY_STORAGE_ACCT_APP_SETTING"
    },
    {
      "type": "table",
      "name": "myNewUserTableBinding",
      "tableName": "newUserTable",
      "connection": "MY_TABLE_STORAGE_ACCT_APP_SETTING",
      "direction": "out"
    }
  ],
  "disabled": false
}
```

Die folgende C#-Funktion antwortet auf ein neues Element, das in die Warteschlange eingereiht wird, und schreibt einen neuen Benutzereintrag in eine Azure Storage-Tabelle.

```cs
#r "Newtonsoft.Json"

using System;
using Newtonsoft.Json;

public static async Task Run(string myNewUserQueueItem, IAsyncCollector<Person> myNewUserTableBinding, 
                                TraceWriter log)
{
    // In this example the queue item is a JSON string representing an order that contains the name, 
    // address and mobile number of the new customer.
    dynamic order = JsonConvert.DeserializeObject(myNewUserQueueItem);

    await myNewUserTableBinding.AddAsync(
        new Person() { 
            PartitionKey = "Test", 
            RowKey = Guid.NewGuid().ToString(), 
            Name = order.name,
            Address = order.address,
            MobileNumber = order.mobileNumber }
        );
}

public class Person
{
    public string PartitionKey { get; set; }
    public string RowKey { get; set; }
    public string Name { get; set; }
    public string Address { get; set; }
    public string MobileNumber { get; set; }
}
```

Weitere Codebeispiele und spezifischere Informationen zu den unterstützten Azure-Speichertypen finden Sie unter [Trigger und Bindungen für Azure Storage in Azure Functions](functions-bindings-storage.md).

Klicken Sie zum Verwenden der erweiterten Bindungsfunktionen im Azure-Portal auf der Registerkarte **Integrieren** Ihrer Funktion auf **Erweiterter Editor**. Mit dem erweiterten Editor können Sie die Datei *function.json* direkt im Portal bearbeiten.

## <a name="random-guids"></a>Zufällige GUIDs
Azure Functions enthält Syntax zum Generieren von zufälligen GUIDs mit Ihren Bindungen. Mit der folgenden Bindungssyntax wird die Ausgabe in ein neues Blob mit einem eindeutigen Namen in einem Speichercontainer geschrieben: 

```json
{
  "type": "blob",
  "name": "blobOutput",
  "direction": "out",
  "path": "my-output-container/{rand-guid}"
}
```


## <a name="returning-a-single-output"></a>Zurückgeben einer einzelnen Ausgabe
In Fällen, in denen Ihr Funktionscode eine einzelne Ausgabe zurückgibt, können Sie eine Ausgabebindung mit dem Namen `$return` verwenden, um in Ihrem Code eine natürlichere Funktionssignatur zu nutzen. Sie kann nur für Sprachen verwendet werden, die einen Rückgabewert unterstützen (C#, Node.js, F#). Die Bindung ähnelt der folgenden Blobausgabebindung, die für einen Warteschlangentrigger verwendet wird.

```json
{
  "bindings": [
    {
      "type": "queueTrigger",
      "name": "input",
      "direction": "in",
      "queueName": "test-input-node"
    },
    {
      "type": "blob",
      "name": "$return",
      "direction": "out",
      "path": "test-output-node/{id}"
    }
  ]
}
```

Mit dem folgenden C#-Code wird die Ausgabe auf natürlichere Weise zurückgegeben, ohne dass in der Funktionssignatur der Parameter `out` verwendet werden muss.

```cs
public static string Run(WorkItem input, TraceWriter log)
{
    string json = string.Format("{{ \"id\": \"{0}\" }}", input.Id);
    log.Info($"C# script processed queue message. Item={json}");
    return json;
}
```

Asynchrones Beispiel:

```cs
public static Task<string> Run(WorkItem input, TraceWriter log)
{
    string json = string.Format("{{ \"id\": \"{0}\" }}", input.Id);
    log.Info($"C# script processed queue message. Item={json}");
    return json;
}
```


Diese Vorgehensweise wird auch wie folgt mit Node.js veranschaulicht:

```javascript
module.exports = function (context, input) {
    var json = JSON.stringify(input);
    context.log('Node.js script processed queue message', json);
    context.done(null, json);
}
```

Es folgt ein F#-Beispiel:

```fsharp
let Run(input: WorkItem, log: TraceWriter) =
    let json = String.Format("{{ \"id\": \"{0}\" }}", input.Id)   
    log.Info(sprintf "F# script processed queue message '%s'" json)
    json
```

Hierfür können auch mehrere Ausgabeparameter verwendet werden, indem eine einzelne Ausgabe mit `$return` festgelegt wird.

## <a name="resolving-app-settings"></a>Auflösen von App-Einstellungen
Als bewährte Methode sollten sensible Informationen mit App-Einstellungen als Teil der Laufzeitumgebung gespeichert werden. Indem sensible Informationen nicht in den Konfigurationsdateien der App enthalten sind, wird die Offenlegung beim Verwenden eines öffentlichen Repositorys zum Speichern von App-Dateien eingeschränkt.  

Die Azure Functions-Laufzeit löst App-Einstellungen in Werte auf, wenn der Name der App-Einstellung in Prozentzeichen eingeschlossen wird (`%your app setting%`). Die folgende [Twilio-Bindung](functions-bindings-twilio.md) verwendet die App-Einstellung `TWILIO_ACCT_PHONE` für das `from`-Feld der Bindung. 

```json
{
  "type": "twilioSms",
  "name": "$return",
  "accountSid": "TwilioAccountSid",
  "authToken": "TwilioAuthToken",
  "to": "{mobileNumber}",
  "from": "%TWILIO_ACCT_PHONE%",
  "body": "Thank you {name}, your order was received Node.js",
  "direction": "out"
},
```



## <a name="parameter-binding"></a>Parameterbindung
Anstelle einer statischen Konfigurationseinstellung für Ihre Ausgabebindungseigenschaften können Sie die Einstellungen auch so konfigurieren, dass sie dynamisch an die Daten gebunden werden, die Teil der Eingabebindung Ihres Triggers sind. Stellen Sie sich ein Szenario vor, bei dem neue Aufträge mit einer Azure Storage-Warteschlange verarbeitet werden. Jedes neue Warteschlangenelement ist eine JSON-Zeichenfolge, die mindestens die folgenden Eigenschaften enthält:

```json
{
  "name" : "Customer Name",
  "address" : "Customer's Address",
  "mobileNumber" : "Customer's mobile number in the format - +1XXXYYYZZZZ."
}
```

Sie können dem Kunden über Ihr Twilio-Konto eine SMS mit dem Hinweis senden, dass die Bestellung eingegangen ist.  Sie können `body` und das Feld `to` Ihrer Twilio-Ausgabebindung wie folgt so konfigurieren, dass eine dynamische Bindung mit den Elementen `name` und `mobileNumber` besteht, die Teil der Eingabe waren.

```json
{
  "name": "myNewOrderItem",
  "type": "queueTrigger",
  "direction": "in",
  "queueName": "queue-newOrders",
  "connection": "orders_STORAGE"
},
{
  "type": "twilioSms",
  "name": "$return",
  "accountSid": "TwilioAccountSid",
  "authToken": "TwilioAuthToken",
  "to": "{mobileNumber}",
  "from": "%TWILIO_ACCT_PHONE%",
  "body": "Thank you {name}, your order was received",
  "direction": "out"
},
```

Jetzt muss mit Ihrem Funktionscode nur noch der Ausgabeparameter wie folgt initialisiert werden. Während der Ausführung werden die Ausgabeeigenschaften an die gewünschten Eingabedaten gebunden.

```cs
#r "Newtonsoft.Json"
#r "Twilio.Api"

using System;
using System.Threading.Tasks;
using Newtonsoft.Json;
using Twilio;

public static async Task<SMSMessage> Run(string myNewOrderItem, TraceWriter log)
{
    log.Info($"C# Queue trigger function processed: {myNewOrderItem}");

    dynamic order = JsonConvert.DeserializeObject(myNewOrderItem);    

    // Even if you want to use a hard coded message and number in the binding, you must at least 
    // initialize the SMSMessage variable.
    SMSMessage smsText = new SMSMessage();

    // The following isn't needed since we use parameter binding for this
    //string msg = "Hello " + order.name + ", thank you for your order.";
    //smsText.Body = msg;
    //smsText.To = order.mobileNumber;

    return smsText;
}
```

Node.js:

```javascript
module.exports = function (context, myNewOrderItem) {    
    context.log('Node.js queue trigger function processed work item', myNewOrderItem);    

    // No need to set the properties of the text, we use parameters in the binding. We do need to 
    // initialize the object.
    var smsText = {};    

    context.done(null, smsText);
}
```

## <a name="advanced-binding-at-runtime-imperative-binding"></a>Erweiterte Bindung zur Laufzeit (imperative Bindung)

Das Standardmuster für Eingabe- und Ausgabebindungen mit *function.json* wird als [*deklarative*](https://en.wikipedia.org/wiki/Declarative_programming) Bindung bezeichnet. Dabei wird die Bindung von der JSON-Deklaration definiert. Sie können jedoch auch [imperative](https://en.wikipedia.org/wiki/Imperative_programming) Bindungen verwenden. Mit diesem Muster ist die Bindung an eine beliebige Anzahl von unterstützten Eingabe- und Ausgabebindungen direkt im Funktionscode möglich.
Möglicherweise benötigen Sie imperative Bindungen in Fällen, in denen die Berechnung des Bindungspfads oder anderer Eingaben zur Laufzeit in Ihrer Funktion erfolgen muss und nicht zur Entwurfszeit. 

Definieren Sie eine imperative Bindung wie folgt:

- Schließen Sie für die gewünschten imperativen Bindungen **keinen** Eintrag in *function.json* ein.
- Übergeben Sie den Eingabeparameter [`Binder binder`](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs.Host/Bindings/Runtime/Binder.cs) oder [`IBinder binder`](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/IBinder.cs). 
- Verwenden Sie das folgende C#-Muster, um die Datenbindung auszuführen.

```cs
using (var output = await binder.BindAsync<T>(new BindingTypeAttribute(...)))
{
    ...
}
```

Dabei ist `BindingTypeAttribute` das .NET-Attribut, das die Bindung definiert, und `T` ist der Eingabe- oder Ausgabetyp, der von diesem Bindungstyp unterstützt wird. `T` darf auch kein `out`-Parametertyp sein (wie `out JObject`). Die ausgehende Bindung für die Tabelle „Mobile Apps“ unterstützt z.B. [sechs Ausgabetypen](https://github.com/Azure/azure-webjobs-sdk-extensions/blob/master/src/WebJobs.Extensions.MobileApps/MobileTableAttribute.cs#L17-L22), Sie können aber nur [ICollector<T>](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/ICollector.cs) oder [IAsyncCollector<T>](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/IAsyncCollector.cs) für `T` verwenden.
    
Mit dem folgenden Beispielcode wird eine [ausgehende Speicherblob-Bindung](functions-bindings-storage-blob.md#storage-blob-output-binding) mit einem Blobpfad erstellt, der zur Laufzeit definiert wird. Dann wird eine Zeichenfolge in das Blob geschrieben.

```cs
using Microsoft.Azure.WebJobs;
using Microsoft.Azure.WebJobs.Host.Bindings.Runtime;

public static async Task Run(string input, Binder binder)
{
    using (var writer = await binder.BindAsync<TextWriter>(new BlobAttribute("samples-output/path")))
    {
        writer.Write("Hello World!!");
    }
}
```

[BlobAttribute](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/BlobAttribute.cs) definiert die Eingabe- oder Ausgabebindung für den [Speicherblob](functions-bindings-storage-blob.md), und [TextWriter](https://msdn.microsoft.com/library/system.io.textwriter.aspx) ist ein unterstützter Ausgabenbindungstyp.
So wie gezeigt ruft der Code die Standard-App-Einstellung für die Speicherkonto-Verbindungszeichenfolge ab (also `AzureWebJobsStorage`). Sie können eine zu verwendende benutzerdefinierte App-Einstellung angeben, indem Sie [StorageAccountAttribute](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/StorageAccountAttribute.cs) hinzufügen und das Attributarray an `BindAsync<T>()` übergeben. Beispiel:

```cs
using Microsoft.Azure.WebJobs;
using Microsoft.Azure.WebJobs.Host.Bindings.Runtime;

public static async Task Run(string input, Binder binder)
{
    var attributes = new Attribute[]
    {    
        new BlobAttribute("samples-output/path"),
        new StorageAccountAttribute("MyStorageAccount")
    };

    using (var writer = await binder.BindAsync<TextWriter>(attributes))
    {
        writer.Write("Hello World!");
    }
}
```

Die folgende Tabelle zeigt, welches .NET-Attribut für die einzelnen Bindungstypen verwendet und auf welches Paket verwiesen wird.

> [!div class="mx-codeBreakAll"]
| Bindung | Attribut | Hinzuzufügender Verweis |
|------|------|------|
| DocumentDB | [`Microsoft.Azure.WebJobs.DocumentDBAttribute`](https://github.com/Azure/azure-webjobs-sdk-extensions/blob/master/src/WebJobs.Extensions.DocumentDB/DocumentDBAttribute.cs) | `#r "Microsoft.Azure.WebJobs.Extensions.DocumentDB"` |
| Event Hubs | [`Microsoft.Azure.WebJobs.ServiceBus.EventHubAttribute`](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs.ServiceBus/EventHubs/EventHubAttribute.cs), [`Microsoft.Azure.WebJobs.ServiceBusAccountAttribute`](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs.ServiceBus/ServiceBusAccountAttribute.cs) | `#r "Microsoft.Azure.Jobs.ServiceBus"` |
| Mobile Apps | [`Microsoft.Azure.WebJobs.MobileTableAttribute`](https://github.com/Azure/azure-webjobs-sdk-extensions/blob/master/src/WebJobs.Extensions.MobileApps/MobileTableAttribute.cs) | `#r "Microsoft.Azure.WebJobs.Extensions.MobileApps"` |
| Notification Hubs | [`Microsoft.Azure.WebJobs.NotificationHubAttribute`](https://github.com/Azure/azure-webjobs-sdk-extensions/blob/master/src/WebJobs.Extensions.NotificationHubs/NotificationHubAttribute.cs) | `#r "Microsoft.Azure.WebJobs.Extensions.NotificationHubs"` |
| Service Bus | [`Microsoft.Azure.WebJobs.ServiceBusAttribute`](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs.ServiceBus/ServiceBusAttribute.cs), [`Microsoft.Azure.WebJobs.ServiceBusAccountAttribute`](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs.ServiceBus/ServiceBusAccountAttribute.cs) | `#r "Microsoft.Azure.WebJobs.ServiceBus"` |
| Speicherwarteschlange | [`Microsoft.Azure.WebJobs.QueueAttribute`](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/QueueAttribute.cs), [`Microsoft.Azure.WebJobs.StorageAccountAttribute`](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/StorageAccountAttribute.cs) | |
| Speicherblob | [`Microsoft.Azure.WebJobs.BlobAttribute`](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/BlobAttribute.cs), [`Microsoft.Azure.WebJobs.StorageAccountAttribute`](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/StorageAccountAttribute.cs) | |
| Speichertabelle | [`Microsoft.Azure.WebJobs.TableAttribute`](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/TableAttribute.cs), [`Microsoft.Azure.WebJobs.StorageAccountAttribute`](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/StorageAccountAttribute.cs) | |
| Twilio | [`Microsoft.Azure.WebJobs.TwilioSmsAttribute`](https://github.com/Azure/azure-webjobs-sdk-extensions/blob/master/src/WebJobs.Extensions.Twilio/TwilioSMSAttribute.cs) | `#r "Microsoft.Azure.WebJobs.Extensions.Twilio"` |



## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen finden Sie in den folgenden Ressourcen:

* [Testen einer Funktion](functions-test-a-function.md)
* [Skalieren einer Funktion](functions-scale.md)


