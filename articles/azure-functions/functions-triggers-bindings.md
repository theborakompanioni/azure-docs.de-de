---
title: Arbeiten mit Triggern und Bindungen in Azure Functions | Microsoft-Dokumentation
description: "Erfahren Sie, wie Sie Trigger und Bindungen in Azure Functions verwenden, um die Verbindung Ihrer Codeausführung mit Onlineereignissen und cloudbasierten Diensten herzustellen."
services: functions
documentationcenter: na
author: lindydonna
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
ms.date: 04/14/2017
ms.author: donnam
ms.translationtype: Human Translation
ms.sourcegitcommit: 71fea4a41b2e3a60f2f610609a14372e678b7ec4
ms.openlocfilehash: f7d0ed4f43c68c73c967a401509f1a5ccac7da0b
ms.contentlocale: de-de
ms.lasthandoff: 05/10/2017


---

# <a name="azure-functions-triggers-and-bindings-concepts"></a>Konzepte für Azure Functions-Trigger und -Bindungen
Azure Functions ermöglicht Ihnen das Schreiben von Code, in dem über *Trigger* und *Bindungen* auf Ereignisse in Azure und anderen Diensten reagiert wird. Dieser Artikel ist eine konzeptionelle Übersicht über Trigger und Bindungen für alle unterstützten Programmiersprachen. Hier sind die Funktionen beschrieben, die für alle Bindungen identisch sind.

## <a name="overview"></a>Übersicht

Trigger und Bindungen sind eine deklarative Möglichkeit, zu definieren, wie eine Funktion aufgerufen wird und mit welchen Daten sie arbeitet. Ein *Trigger* definiert, wie eine Funktion aufgerufen wird. Eine Funktion darf nur einen Trigger haben. Trigger haben zugeordnete Daten, in üblicherweise mit der Nutzlast identisch sind, von der die Funktion ausgelöst wurde. 

Eingabe- und Ausgabe-*Bindungen* bieten eine deklarative Möglichkeit, aus Code heraus Verbindungen mit Daten herzustellen. Ähnlich zu Triggern geben Sie Verbindungszeichenfolgen und andere Eigenschaften in der Konfiguration Ihrer jeweiligen Funktion an. Bindungen sind optional, und eine Funktion kann mehrere Eingabe- und Ausgabebindungen haben. 

Durch Verwenden von Triggern und Bindungen können Sie Code schreiben, der allgemeiner ist und in dem die Details der Dienste, mit denen er interagiert, nicht hartcodiert sind. Daten, die von Diensten stammen, werden schlicht Eingabewerte für Ihren Funktionscode. Um Daten an einen anderen Dienst auszugeben (z. B. Erstellen einer neuen Zeile in Azure Table Storage), verwenden Sie den Rückgabewert der Methode. Sie können aber auch, wenn Sie mehrere Werte ausgeben müssen, ein Hilfsobjekt verwenden. Trigger und Bindungen haben die **name**-Eigenschaft, die ein Bezeichner ist, den Sie in Ihrem Code verwenden, um auf die jeweilige die Bindung zuzugreifen.

Sie können Trigger und Bindungen im Azure Functions-Portal auf der Registerkarte **Integrieren** konfigurieren. Im Hintergrund ändert die Benutzeroberfläche eine Datei namens *function.json* im Verzeichnis für Funktionen. Sie können diese Datei bearbeiten, indem Sie zu **Erweiterter Editor** wechseln.

In der folgenden Tabelle sind die Trigger und die Bindungen aufgeführt, die mit Azure Functions unterstützt werden. 

[!INCLUDE [Full bindings table](../../includes/functions-bindings.md)]

### <a name="example-queue-trigger-and-table-output-binding"></a>Beispiel: Triggerwarteschlange und Tabellenausgabebindung

Angenommen, Sie möchten immer dann eine neue Zeile in Azure Table Storage schreiben, wenn in Azure Queue Storage eine neue Nachricht eingetroffen ist. Dieses Szenario kann mit einem Azure-Warteschlangentrigger und einer Tabellenausgabebindung implementiert werden. 

Für einen Warteschlangentrigger sind die folgenden Informationen auf der Registerkarte **Integrieren** erforderlich:

* Der Name der App-Einstellung, die die Speicherkonto-Verbindungszeichenfolge für die Warteschlange enthält
* Der Name der Warteschlange
* Der Bezeichner im Code, um den Inhalt der Warteschlangennachricht zu lesen, beispielsweise `order`

Um in Azure Table Storage zu schreiben, verwenden Sie eine Ausgabebindung mit den folgenden Details:

* Der Name der App-Einstellung, die die Speicherkonto-Verbindungszeichenfolge für die Tabelle enthält
* Der Tabellenname
* Der Bezeichner im Code, um Ausgabeelemente oder den Rückgabewert der Funktion zu erstellen

In Bindungen werden App-Einstellungen für Verbindungszeichenfolgen verwendet, damit sichergestellt ist, dass *function.json* keine Geheimnisse eines Diensts enthält.

Verwenden Sie dann in Ihrem Code die Bezeichner, die Sie zum Einbinden von Azure Storage bereitgestellt haben.

```cs
#r "Newtonsoft.Json"

using Newtonsoft.Json.Linq;

// From an incoming queue message that is a JSON object, add fields and write to Table Storage
// The method return value creates a new row in Table Storage
public static Person Run(JObject order, TraceWriter log)
{
    return new Person() { 
            PartitionKey = "Orders", 
            RowKey = Guid.NewGuid().ToString(),  
            Name = order["Name"].ToString(),
            MobileNumber = order["MobileNumber"].ToString() };  
}
 
public class Person
{
    public string PartitionKey { get; set; }
    public string RowKey { get; set; }
    public string Name { get; set; }
    public string MobileNumber { get; set; }
}
```

```javascript
// From an incoming queue message that is a JSON object, add fields and write to Table Storage
// The second parameter to context.done is used as the value for the new row
module.exports = function (context, order) {
    order.PartitionKey = "Orders";
    order.RowKey = generateRandomId(); 

    context.done(null, order);
};

function generateRandomId() {
    return Math.random().toString(36).substring(2, 15) +
        Math.random().toString(36).substring(2, 15);
}
```

Es folgt die *function.json*-Datei, die dem obigen Code entspricht. Sie können jeweils dieselbe Konfiguration verwenden, unabhängig von der Sprache der Funktionsimplementierung.

```json
{
  "bindings": [
    {
      "name": "order",
      "type": "queueTrigger",
      "direction": "in",
      "queueName": "myqueue-items",
      "connection": "MY_STORAGE_ACCT_APP_SETTING"
    },
    {
      "name": "$return",
      "type": "table",
      "direction": "out",
      "tableName": "outTable",
      "connection": "MY_TABLE_STORAGE_ACCT_APP_SETTING"
    }
  ]
}
```
Wenn Sie den Inhalt von *function.json* im Azure-Portal anzeigen und bearbeiten möchten, klicken Sie für die Funktion auf der Registerkarte **Integrieren** auf die Option **Erweiterter Editor**.

Weitere Codebeispiele sowie Informationen zum Einbeziehen von Azure Storage finden Sie unter [Trigger und Bindungen für Azure Storage in Azure Functions](functions-bindings-storage.md).

### <a name="binding-direction"></a>Bindungsrichtung

Alle Trigger und Bindungen haben eine Eigenschaft für die Richtung (`direction`-Eigenschaft):

- Für Trigger ist die Richtung immer gleich `in`.
- Für Eingabe- und Ausgabebindungen werden `in` und `out` verwendet.
- Einige Bindungen unterstützen die spezielle Richtung `inout`. Wenn Sie `inout` verwenden, ist nur **Erweiterter Editor** auf der Registerkarte **Integrieren** verfügbar.

## <a name="using-the-function-return-type-to-return-a-single-output"></a>Verwenden des Funktionsrückgabetyps, um eine einzelne Ausgabe zurückzugeben

Im vorhergehenden Beispiel wird gezeigt, wie der Funktionsrückgabewert verwendet wird, um Ausgabe für eine Bindung bereitzustellen, wozu der spezielle Namensparameter `$return` verwendet wird. (Dies wird nur in Sprachen unterstützt, die Rückgabewerte haben, z. B. C#, JavaScript und F#.) Hat eine Funktion mehrere Ausgabebindungen, verwenden Sie `$return` nur für eine der Ausgabebindungen. 

```json
// excerpt of function.json
{
    "name": "$return",
    "type": "blob",
    "direction": "out",
    "path": "output-container/{id}"
}
```

In den folgenden Beispielen wird gezeigt, wie Rückgabetypen mit Ausgabebindungen in C#, JavaScript und F# verwendet werden.

```cs
// C# example: use method return value for output binding
public static string Run(WorkItem input, TraceWriter log)
{
    string json = string.Format("{{ \"id\": \"{0}\" }}", input.Id);
    log.Info($"C# script processed queue message. Item={json}");
    return json;
}
```

```cs
// C# example: async method, using return value for output binding
public static Task<string> Run(WorkItem input, TraceWriter log)
{
    string json = string.Format("{{ \"id\": \"{0}\" }}", input.Id);
    log.Info($"C# script processed queue message. Item={json}");
    return json;
}
```

```javascript
// JavaScript: return a value in the second parameter to context.done
module.exports = function (context, input) {
    var json = JSON.stringify(input);
    context.log('Node.js script processed queue message', json);
    context.done(null, json);
}
```

```fsharp
// F# example: use return value for output binding
let Run(input: WorkItem, log: TraceWriter) =
    let json = String.Format("{{ \"id\": \"{0}\" }}", input.Id)   
    log.Info(sprintf "F# script processed queue message '%s'" json)
    json
```

## <a name="resolving-app-settings"></a>Auflösen von App-Einstellungen
Es hat sich bewährt, Geheimnisse und Verbindungszeichenfolgen nicht in Konfigurationsdateien, sondern über App-Einstellungen zu verwalten. Dies schränkt den Zugriff auf diese Geheimnisse ein und bewirkt, dass es hinsichtlich der Sicherheit unbedenklich ist, *function.json* in einem öffentlichen Quellcodeverwaltungs-Repository zu speichern.

App-Einstellungen sind auch nützlich, wenn Sie die jeweilige Konfiguration entsprechend der Umgebung ändern möchten. Beispielsweise kann es sein, dass Sie in einer Testumgebung eine andere Warteschlange oder einen anderen Blob Storage-Container überwachen möchten.

App-Einstellungen werden immer dann aufgelöst, wenn ein Wert in Prozentzeichen steht, etwa `%MyAppSetting%`. Die `connection`Eigenschaft von Triggern und Bindungen ist ein Sonderfall, denn für sie werden Werte automatisch als App-Einstellungen aufgelöst. 

Im folgenden Beispiel ist ein Warteschlangentrigger gezeigt, in dem die App-Einstellung `%input-queue-name%` verwendet wird, um die Warteschlange anzugeben, für die ausgelöst werden soll.

```json
{
  "bindings": [
    {
      "name": "order",
      "type": "queueTrigger",
      "direction": "in",
      "queueName": "%input-queue-name%",
      "connection": "MY_STORAGE_ACCT_APP_SETTING"
    }
  ]
}
```

## <a name="trigger-metadata-properties"></a>Metadateneigenschaften von Triggern

Viele Trigger stellen zusätzlich zur Datennutzlast (z. B. die Warteschlangennachricht, von der eine Funktion ausgelöst wurde) weitere Metadatenwerte bereit. Diese Werte können als Eingabeparameter in C# und F# oder als Eigenschaften für das `context.bindings`-Objekt in JavaScript verwendet werden. 

Beispielsweise unterstützt ein Warteschlangentrigger die folgenden Eigenschaften:

* QueueTrigger – Auslösen von Nachrichteninhalt, wenn gültige Zeichenfolge
* DequeueCount
* ExpirationTime
* ID
* InsertionTime
* NextVisibleTime
* PopReceipt

Details der Metadateneigenschaften für jeden Trigger sind im entsprechenden Referenzthema beschrieben. Dokumentation ist auch im Portal auf der Registerkarte **Integrieren** im Abschnitt **Dokumentation** verfügbar, der sich unter dem Bereich für Bindungskonfigurationen befindet.  

Beispielsweise können Sie, da Blobtrigger einige Verzögerungen haben, einen Warteschlangentrigger verwenden, um Ihre Funktion auszuführen (weitere Informationen finden Sie unter [Storage-Blobtrigger](functions-bindings-storage-blob.md#storage-blob-trigger). Die Warteschlangennachricht würde den Namen der Blobdatei enthalten, für die ausgelöst werden soll. Durch Verwenden der `queueTrigger`-Metadateneigenschaft können Sie dieses Verhalten vollständig in Ihrer Konfiguration statt in Ihrem Code angeben.

```json
  "bindings": [
    {
      "name": "myQueueItem",
      "type": "queueTrigger",
      "queueName": "myqueue-items",
      "connection": "MyStorageConnection",
    },
    {
      "name": "myInputBlob",
      "type": "blob",
      "path": "samples-workitems/{queueTrigger}",
      "direction": "in",
      "connection": "MyStorageConnection"
    }
  ]
```

Metadateneigenschaften von einem Trigger können auch in einem *Bindungsausdruck* für eine andere Bindung verwendet werden, wie dies im folgenden Abschnitt beschrieben ist.

## <a name="binding-expressions-and-patterns"></a>Bindungsausdrücke und Muster

Eines der leistungsstärksten Merkmale von Triggern und Bindungen sind *Bindungsausdrücke*. In einer Bindung können Sie Musterausdrücke definieren, die dann in anderen Bindungen oder in Code verwendet werden können. Triggermetadaten können auch in Bindungsausdrücken verwendet werden, wie dies im Beispiel im vorherigen Abschnitt gezeigt ist.

Angenommen, Sie möchten die Größe der Bilder in einem bestimmte Blob Storage-Container ähnlich ändern wie mit der Vorlage für Bildgrößenänderung (**Image Resizer**) auf der Seite **Neue Funktion**. Wechseln Sie zu **Neue Funktion** -> Sprache **C#** -> Szenario **Beispiele** -> **ImageResizer-CSharp**. 

So sieht die *function.json*-Definition aus:

```json
{
  "bindings": [
    {
      "name": "image",
      "type": "blobTrigger",
      "path": "sample-images/{filename}",
      "direction": "in",
      "connection": "MyStorageConnection"
    },
    {
      "name": "imageSmall",
      "type": "blob",
      "path": "sample-images-sm/{filename}",
      "direction": "out",
      "connection": "MyStorageConnection"
    }
  ],
}
```

Beachten Sie, dass der `filename`-Parameter sowohl in der Definition des Blobtriggers als auch in der Blob-Ausgabebindung verwendet wird. Dieser Parameter kann auch in Funktionscode verwendet werden.

```csharp
// C# example of binding to {filename}
public static void Run(Stream image, string filename, Stream imageSmall, TraceWriter log)  
{
    log.Info($"Blob trigger processing: {filename}");
    // ...
} 
```

<!--TODO: add JavaScript example -->
<!-- Blocked by bug https://github.com/Azure/Azure-Functions/issues/248 -->


### <a name="random-guids"></a>Zufällige GUIDs
Azure Functions bietet über den `{rand-guid}`-Bindungsausdruck eine benutzerfreundliche Syntax zum Generieren von GUIDs. Im folgenden Beispiel wird dies verwendet, um einen eindeutigen Blobnamen zu generieren: 

```json
{
  "type": "blob",
  "name": "blobOutput",
  "direction": "out",
  "path": "my-output-container/{rand-guid}"
}
```

## <a name="bind-to-custom-input-properties-in-a-binding-expression"></a>Binden an benutzerdefinierte Eingabeeigenschaften in einem Bindungsausdruck

In Bindungsausdrücken kann auch auf Eigenschaften verwiesen werden, die in der Triggernutzlast definiert sind. Beispielsweise könnte es sein, dass Sie über einen Dateinamen, der in einem Webhook bereitgestellt wird, ein dynamisches Binden an eine Blob Storage-Datei vornehmen möchten.

In der folgenden *function.json*-Datei wird beispielsweise eine Eigenschaft namens `BlobName` aus der Triggernutzlast verwendet:

```json
{
  "bindings": [
    {
      "name": "info",
      "type": "httpTrigger",
      "direction": "in",
      "webHookType": "genericJson",
    },
    {
      "name": "blobContents",
      "type": "blob",
      "direction": "in",
      "path": "strings/{BlobName}",
      "connection": "AzureWebJobsStorage"
    },
    {
      "name": "res",
      "type": "http",
      "direction": "out"
    }
  ]
}
```

Um dies in C# und F# zu implementieren, müssen Sie eine POCO-Entität definieren, in der die Felder definiert sind, die in der Triggernutzlast deserialisiert werden.

```csharp
using System.Net;

public class BlobInfo
{
    public string BlobName { get; set; }
}
  
public static HttpResponseMessage Run(HttpRequestMessage req, BlobInfo info, string blobContents)
{
    if (blobContents == null) {
        return req.CreateResponse(HttpStatusCode.NotFound);
    } 

    return req.CreateResponse(HttpStatusCode.OK, new {
        data = $"{blobContents}"
    });
}
```

In JavaScript wird JSON-Deserialisierung automatisch ausgeführt, und Sie können die Eigenschaften direkt verwenden.

```javascript
module.exports = function (context, info) {
    if ('BlobName' in info) {
        context.res = {
            body: { 'data': context.bindings.blobContents }
        }
    }
    else {
        context.res = {
            status: 404
        };
    }
    context.done();
}
```

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen zu einer bestimmten Bindung finden Sie in den folgenden Artikeln:

- [HTTP und Webhooks](functions-bindings-http-webhook.md)
- [Zeitgeber](functions-bindings-timer.md)
- [Queue Storage](functions-bindings-storage-queue.md)
- [Blob Storage](functions-bindings-storage-blob.md)
- [Table Storage](functions-bindings-storage-table.md)
- [Event Hub](functions-bindings-event-hubs.md)
- [Service Bus](functions-bindings-service-bus.md)
- [Cosmos DB](functions-bindings-documentdb.md)
- [SendGrid](functions-bindings-sendgrid.md)
- [Twilio](functions-bindings-twilio.md)
- [Notification Hubs](functions-bindings-notification-hubs.md)
- [Mobile Apps](functions-bindings-mobile-apps.md)
- [Externe Datei](functions-bindings-external-file.md)

