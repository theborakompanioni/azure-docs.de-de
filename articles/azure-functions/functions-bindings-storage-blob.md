---
title: "Azure Functions – Blob Storage-Bindungen | Microsoft-Dokumentation"
description: Erfahren Sie, wie Azure Storage-Trigger und -Bindungen in Azure Functions verwendet werden.
services: functions
documentationcenter: na
author: lindydonna
manager: erikre
editor: 
tags: 
keywords: Azure Functions, Funktionen, Ereignisverarbeitung, dynamisches Compute, serverlose Architektur
ms.assetid: aba8976c-6568-4ec7-86f5-410efd6b0fb9
ms.service: functions
ms.devlang: multiple
ms.topic: reference
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 05/25/2017
ms.author: donnam, glenga
ms.translationtype: Human Translation
ms.sourcegitcommit: a643f139be40b9b11f865d528622bafbe7dec939
ms.openlocfilehash: b819bf4461f14033dd2c00331e3c3e4d0fbafde6
ms.contentlocale: de-de
ms.lasthandoff: 05/31/2017


---
# <a name="azure-functions-blob-storage-bindings"></a>Azure Functions – Blob Storage-Bindungen
[!INCLUDE [functions-selector-bindings](../../includes/functions-selector-bindings.md)]

Dieser Artikel erläutert das Konfigurieren von und Arbeiten mit Azure Blob Storage-Bindungen in Azure Functions. Azure Functions unterstützt Trigger, Eingabebindungen und Ausgabebindungen für Azure Blob Storage. Features, die in allen Bindungen verfügbar sind, finden Sie unter [Konzepte für Azure Functions-Trigger und -Bindungen](functions-triggers-bindings.md).

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

> [!NOTE]
> Ein [Speicherkonto ausschließlich für Blobs](../storage/storage-create-storage-account.md#blob-storage-accounts) wird nicht unterstützt. Blob Storage-Trigger und -Bindungen erfordern ein allgemeines Speicherkonto. 
> 

<a name="trigger"></a>
<a name="storage-blob-trigger"></a>
## <a name="blob-storage-triggers-and-bindings"></a>Blob Storage-Trigger und -Bindungen

Mit dem Azure Blob Storage-Trigger wird Ihr Funktionscode aufgerufen, wenn ein neuer oder aktualisierter Blob erkannt wird. Der Blob-Inhalt wird als Eingabe für die Funktion bereitgestellt.

Sie definieren einen Blob Storage-Trigger auf der Registerkarte **Integrieren** im Functions-Portal. Das Portal erstellt die folgende Definition im Abschnitt **Bindungen** der Datei *function.json*:

```json
{
    "name": "<The name used to identify the trigger data in your code>",
    "type": "blobTrigger",
    "direction": "in",
    "path": "<container to monitor, and optionally a blob name pattern - see below>",
    "connection": "<Name of app setting - see below>"
}
```

Die Bindungen für Blob-Eingabe und -Ausgabe werden mit `blob` als Bindungstyp definiert:

```json
{
  "name": "<The name used to identify the blob input in your code>",
  "type": "blob",
  "direction": "in", // other supported directions are "inout" and "out"
  "path": "<Path of input blob - see below>",
  "connection":"<Name of app setting - see below>"
},
```

* Die `path`-Eigenschaft unterstützt Bindungsausdrücke und Filterparameter. Weitere Informationen finden Sie unter [Namensmuster](#pattern).
* Die `connection`-Eigenschaft muss den Namen einer App-Einstellung enthalten, die eine Speicherverbindungszeichenfolge enthält. Im Azure-Portal konfiguriert der Standard-Editor auf der Registerkarte **Integrieren** diese App-Einstellung für Sie, wenn Sie ein Speicherkonto auswählen.

> [!NOTE]
> Bei Verwendung eines Blobtriggers in einem Verbrauchsplan kann es bis zu 10 Minuten dauern, bis neue Blobs verarbeitet werden, nachdem eine Funktions-App in den Leerlauf gewechselt ist. Sobald die Funktions-App ausgeführt wird, werden die Blobs sofort verarbeitet. Um diese anfängliche Verzögerung zu vermeiden, sollten Sie eine der folgenden Möglichkeiten erwägen:
> - Verwenden Sie einen App Service-Plan mit aktivierter Option „Always On“.
> - Verwenden Sie einen anderen Mechanismus zum Auslösen der Blobverarbeitung, z.B. eine Warteschlangennachricht, die den Blobnamen enthält. Ein Beispiel finden Sie unter [Warteschlangentrigger mit Blobeingabebindung](#input-sample).

<a name="pattern"></a>

### <a name="name-patterns"></a>Namensmuster
Sie können ein Blobnamensmuster in der `path`-Eigenschaft angeben, bei der es sich um einen Filter oder einen Bindungsausdruck handeln kann. Weitere Informationen finden Sie unter [Bindungsausdrücke und Muster](functions-triggers-bindings.md#binding-expressions-and-patterns).

Um z.B. Blobs zu filtern, die mit der Zeichenfolge „original“ beginnen, verwenden Sie die folgende Definition. Dieser Pfad findet ein Blob mit dem Namen *original-Blob1.txt* im Container *input*, und der Wert der Variablen `name` im Funktionscode lautet `Blob1`.

```json
"path": "input/original-{name}",
```

Um den Blobdateinamen und die Erweiterung separat zu binden, verwenden Sie zwei Muster. Mit diesem Pfad erhalten Sie ein Blob mit dem Namen *original-Blob1.txt*, und die Werte der Variablen `blobname` und `blobextension` im Funktionscode lauten *original-Blob1* und *txt*.

```json
"path": "input/{blobname}.{blobextension}",
```

Sie können den Dateityp von Blobs beschränken, indem Sie einen festen Wert als Dateierweiterung verwenden. Um nur bei PNG-Dateien auszulösen, verwenden Sie z.B. folgendes Muster:

```json
"path": "samples/{name}.png",
```

Geschweifte Klammern werden in Namensmustern als Sonderzeichen angesehen. Um Blobnamen anzugeben, deren Namen geschweifte Klammern enthalten, können Sie die geschweiften Klammern als Escapezeichen verdoppeln. Das folgende Beispiel findet das Blob *{20140101}-soundfile.mp3* im Container *images*, und der Wert der Variablen `name` im Funktionscode lautet *soundfile.mp3*. 

```json
"path": "images/{{20140101}}-{name}",
```

### <a name="trigger-metadata"></a>Metadaten für Trigger

Der Blobtrigger stellt mehrere Metadateneigenschaften bereit. Diese Eigenschaften können als Teil der Bindungsausdrücke in anderen Bindungen oder als Parameter im Code verwendet werden. Diese Werte haben die gleiche Semantik wie [CloudBlob](https://docs.microsoft.com/en-us/dotnet/api/microsoft.windowsazure.storage.blob.cloudblob?view=azure-dotnet).

- **BlobTrigger**. Geben Sie `string`ein. Der auslösende Blobpfad
- **Uri**. Geben Sie `System.Uri`ein. Der Blob-URI für den primären Speicherort
- **Properties**. Geben Sie `Microsoft.WindowsAzure.Storage.Blob.BlobProperties`ein. Die Systemeigenschaften des Blobs
- **Metadata**. Geben Sie `IDictionary<string,string>`ein. Die benutzerdefinierten Metadaten für das Blob

<a name="receipts"></a>

### <a name="blob-receipts"></a>BLOB-Zugänge
Die Azure Functions-Runtime stellt sicher, dass Blobtriggerfunktionen für ein neues oder aktualisiertes Blob nicht mehrmals aufgerufen werden. Zu diesem Zweck wird mittels Verwaltung der *Blobbelege* ermittelt, ob eine bestimmte Blobversion verarbeitet wurde.

Azure Functions speichert Blobbelege in einem Container mit dem Namen *azure-webjobs-hosts* im Azure Storage-Konto für Ihre Funktions-App (per App-Einstellung `AzureWebJobsStorage` definiert). Ein Blobbeleg enthält die folgenden Informationen:

* Die ausgelöste Funktion („*&lt;Funktions-App-Name>*.Functions.*&lt;Funktionsname>*“, z.B. „MyFunctionApp.Functions.CopyBlob“)
* Containername
* Blobtyp ("BlockBlob" oder "PageBlob")
* Blobname
* ETag (eine Blobversions-ID. Beispiel: 0x8D1DC6E70A277EF)

Um eine erneute Verarbeitung eines Blobs zu erzwingen, können Sie den Blobbeleg für dieses Blob manuell aus dem Container *azure-webjobs-hosts* löschen.

<a name="poison"></a>

### <a name="handling-poison-blobs"></a>Verarbeiten von nicht verarbeitbaren Blobs
Wenn bei Ausführung einer Blobtriggerfunktion für ein Blob ein Fehler auftritt, versucht Azure Functions standardmäßig bis zu fünfmal, diese Funktion für ein bestimmtes Blob auszuführen. 

Wenn bei allen fünf Versuchen Fehler auftreten, fügt Azure Functions der Storage-Warteschlange *webjobs-blobtrigger-poison* eine Nachricht hinzu. Die Warteschlangennachricht für nicht verarbeitbare Blobs ist ein JSON-Objekt, das die folgenden Eigenschaften enthält:

* FunctionId (im Format *&lt;Funktionen-App-Name>*.Functions.*&lt;Funktionsname>*)
* BlobType ("BlockBlob" oder "PageBlob")
* ContainerName
* BlobName
* ETag (eine Blobversions-ID. Beispiel: 0x8D1DC6E70A277EF)

### <a name="blob-polling-for-large-containers"></a>Abfragen von Blobs für große Container
Wenn der überwachte Blobcontainer mehr als 10.000 Blobs enthält, überprüft die Functions-Runtime Protokolldateien auf neue oder geänderte Blobs. Dieser Prozess läuft nicht in Echtzeit ab. Eine Funktion wird unter Umständen erst mehrere Minuten nach der Bloberstellung oder noch später ausgelöst. Außerdem erfolgt das [Erstellen von Storage-Protokollen auf bestmögliche Weise](/rest/api/storageservices/About-Storage-Analytics-Logging). Es gibt aber keine Garantie, dass alle Ereignisse erfasst werden. Unter bestimmten Umständen können Protokolle fehlen. Wenn eine schnellere oder zuverlässigere Blobverarbeitung erforderlich ist, sollten Sie beim Erstellen des Blobs eine [Warteschlangennachricht](../storage/storage-dotnet-how-to-use-queues.md) 
 erstellen. Verwenden Sie dann einen [Warteschlangentrigger](functions-bindings-storage-queue.md) anstelle eines Blobtriggers für die Verarbeitung des Blobs.

<a name="triggerusage"></a>

## <a name="using-a-blob-trigger-and-input-binding"></a>Verwenden eines Blobtriggers und einer Eingabebindung
In .NET Funktionen greifen Sie über einen Methodenparameter wie `Stream paramName` auf die Blobdaten zu. Hier ist `paramName` der Wert, den Sie bei der [Triggerkonfiguration](#trigger) angegeben haben. In Node.js-Funktionen greifen Sie mit `context.bindings.<name>` auf die Eingabeblobdaten zu.

Sie können in .NET Bindungen mit beliebigen Typen in der Liste unten erstellen. Bei der Verwendung als Eingabebindung erfordern einige dieser Typen in *function.json* als Bindungsrichtung `inout`. Diese Richtung wird vom Standard-Editor nicht unterstützt, sodass Sie den erweiterten Editor verwenden müssen.

* `TextReader`
* `Stream`
* `ICloudBlob` (erfordert die Bindungsrichtung „inout“)
* `CloudBlockBlob` (erfordert die Bindungsrichtung „inout“)
* `CloudPageBlob` (erfordert die Bindungsrichtung „inout“)
* `CloudAppendBlob` (erfordert die Bindungsrichtung „inout“)

Wenn Textblobs zu erwarten sind, können Sie auch an den .NET-Typ `string` binden. Dies wird nur empfohlen, wenn die Blobgröße klein ist, da der gesamte Blobinhalt in den Arbeitsspeicher geladen wird. Im Allgemeinen ist es günstiger, die Typen `Stream` oder `CloudBlockBlob` zu verwenden.

## <a name="trigger-sample"></a>Triggerbeispiel
Angenommen, Sie verfügen über die folgende Datei „function.json“, die einen Blob Storage-Trigger definiert:

```json
{
    "disabled": false,
    "bindings": [
        {
            "name": "myBlob",
            "type": "blobTrigger",
            "direction": "in",
            "path": "samples-workitems",
            "connection":"MyStorageAccount"
        }
    ]
}
```

Beachten Sie das sprachspezifische Beispiel für das Protokollieren der Inhalte jedes Blobs, der dem überwachten Container hinzugefügt wird.

* [C#](#triggercsharp)
* [Node.js](#triggernodejs)

<a name="triggercsharp"></a>

### <a name="blob-trigger-examples-in-c"></a>Beispiele für Blobtrigger in C# #

```cs
// Blob trigger sample using a Stream binding
public static void Run(Stream myBlob, TraceWriter log)
{
   log.Info($"C# Blob trigger function Processed blob\n Name:{name} \n Size: {myBlob.Length} Bytes");
}
```

```cs
// Blob trigger binding to a CloudBlockBlob
#r "Microsoft.WindowsAzure.Storage"

using Microsoft.WindowsAzure.Storage.Blob;

public static void Run(CloudBlockBlob myBlob, string name, TraceWriter log)
{
    log.Info($"C# Blob trigger function Processed blob\n Name:{name}\nURI:{myBlob.StorageUri}");
}
```

<a name="triggernodejs"></a>

### <a name="trigger-example-in-nodejs"></a>Triggerbeispiel in Node.js

```javascript
module.exports = function(context) {
    context.log('Node.js Blob trigger function processed', context.bindings.myBlob);
    context.done();
};
```
<a name="outputusage"></a> <a name="storage-blob-output-binding"></a>

## <a name="using-a-blob-output-binding"></a>Verwenden einer Blobausgabebindung

In .NET-Funktionen sollten Sie entweder einen `out string`-Parameter in Ihrer Funktionssignatur oder einen der Typen in der folgenden Liste verwenden. In Node.js-Funktionen greifen Sie mit `context.bindings.<name>` auf das Ausgabeblob zu.

In .NET-Funktionen können Sie eine Ausgabe in einen der folgenden Typen erstellen:

* `out string`
* `TextWriter`
* `Stream`
* `CloudBlobStream`
* `ICloudBlob`
* `CloudBlockBlob` 
* `CloudPageBlob` 

<a name="input-sample"></a>

## <a name="queue-trigger-with-blob-input-and-output-sample"></a>Warteschlangentrigger mit Beispiel für Blobeingabe und -ausgabe
Angenommen, Sie verfügen über die folgende Datei „function.json“, die einen [Queue Storage-Trigger](functions-bindings-storage-queue.md), eine Blob Storage-Eingabe und eine Blob Storage-Ausgabe definiert. Beachten Sie die Verwendung der Metadateneigenschaft `queueTrigger` in den `path`-Eigenschaften für Blobeingabe und -ausgabe:

```json
{
  "bindings": [
    {
      "queueName": "myqueue-items",
      "connection": "MyStorageConnection",
      "name": "myQueueItem",
      "type": "queueTrigger",
      "direction": "in"
    },
    {
      "name": "myInputBlob",
      "type": "blob",
      "path": "samples-workitems/{queueTrigger}",
      "connection": "MyStorageConnection",
      "direction": "in"
    },
    {
      "name": "myOutputBlob",
      "type": "blob",
      "path": "samples-workitems/{queueTrigger}-Copy",
      "connection": "MyStorageConnection",
      "direction": "out"
    }
  ],
  "disabled": false
}
``` 

Beachten Sie das sprachspezifische Beispiel für das Kopieren des Eingabeblobs in das Ausgabeblob.

* [C#](#incsharp)
* [Node.js](#innodejs)

<a name="incsharp"></a>

### <a name="blob-binding-example-in-c"></a>Beispiel für Blobbindung in C# #

```cs
// Copy blob from input to output, based on a queue trigger
public static void Run(string myQueueItem, Stream myInputBlob, out string myOutputBlob, TraceWriter log)
{
    log.Info($"C# Queue trigger function processed: {myQueueItem}");
    myOutputBlob = myInputBlob;
}
```

<a name="innodejs"></a>

### <a name="blob-binding-example-in-nodejs"></a>Beispiel für Blobbindung in Node.js

```javascript
// Copy blob from input to output, based on a queue trigger
module.exports = function(context) {
    context.log('Node.js Queue trigger function processed', context.bindings.myQueueItem);
    context.bindings.myOutputBlob = context.bindings.myInputBlob;
    context.done();
};
```

## <a name="next-steps"></a>Nächste Schritte
[!INCLUDE [next steps](../../includes/functions-bindings-next-steps.md)]


