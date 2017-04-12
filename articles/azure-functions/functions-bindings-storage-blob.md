---
title: "Azure Functions – Storage-Blobbindungen | Microsoft-Dokumentation"
description: Erfahren Sie, wie Azure Storage-Trigger und -Bindungen in Azure Functions verwendet werden.
services: functions
documentationcenter: na
author: christopheranderson
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
ms.date: 03/06/2017
ms.author: chrande, glenga
translationtype: Human Translation
ms.sourcegitcommit: eeb56316b337c90cc83455be11917674eba898a3
ms.openlocfilehash: 7b4ae9281bca20949c37b2c797e4a1a677665929
ms.lasthandoff: 04/03/2017


---
# <a name="azure-functions-storage-blob-bindings"></a>Azure Functions – Storage-Blobbindungen
[!INCLUDE [functions-selector-bindings](../../includes/functions-selector-bindings.md)]

In diesem Artikel wird das Konfigurieren und Codieren von Azure Storage-Blobbindungen in Azure Functions beschrieben. Azure Functions unterstützt Trigger, Eingabebindungen und Ausgabebindungen für Azure Storage-Blobs.

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

> [!NOTE]
> Ein [Speicherkonto ausschließlich für Blobs](../storage/storage-create-storage-account.md#blob-storage-accounts) wird nicht unterstützt. Für Azure Functions ist ein allgemeines Speicherkonto zur Verwendung mit Blobs erforderlich. 
> 
> 

<a name="trigger"></a>

## <a name="storage-blob-trigger"></a>Storage-Blobtrigger
Mit dem Azure Storage-Blobtrigger können Sie einen Speichercontainer auf neue und geänderte Blobs überwachen und Funktionscode ausführen, wenn Änderungen erkannt werden. 

Der Storage-Blobtrigger zu einer Funktion verwendet die folgenden JSON-Objekte im `bindings`-Array von „function.json“:

```json
{
    "name": "<Name of input parameter in function signature>",
    "type": "blobTrigger",
    "direction": "in",
    "path": "<container to monitor, and optionally a blob name pattern - see below>",
    "connection":"<Name of app setting - see below>"
}
```

Beachten Sie Folgendes:

* Für `path` finden Sie Informationen zur Formatierung von Blobnamensmustern unter [Namensmuster](#pattern).
* `connection` muss den Namen einer App-Einstellung enthalten, die eine Speicherverbindungszeichenfolge enthält. Im Azure-Portal konfiguriert der Standard-Editor auf der Registerkarte **Integrieren** diese App-Einstellung für Sie, wenn Sie ein Speicherkonto erstellen oder ein vorhandenes auswählen. Wie Sie diese App-Einstellung manuell erstellen, erfahren Sie unter [Manuelles Konfigurieren dieser App-Einstellung](functions-how-to-use-azure-function-app-settings.md). 

Wenn bei der Ausführung eines Verbrauchsplans eine Funktionen-App im Leerlauf ist, kann es möglicherweise bis zu 10 Minuten dauern, bis neue Blobs erstellt werden. Sobald die Funktionen-App ausgeführt wird, werden die Blobs schneller verarbeitet. Um diese anfängliche Verzögerung zu vermeiden, verwenden Sie entweder einen regulären App Service-Plan mit aktivierter Always On-Option oder einen anderen Mechanismus, um die Blobverarbeitung auszulösen, z.B. eine Warteschlangennachricht mit dem Blobnamen. 

Weitere Informationen finden Sie auch unter einer der folgenden Unterüberschriften:

* [Namensmuster](#pattern)
* [Blobbelege](#receipts)
* [Verarbeiten von nicht verarbeitbaren Blobs](#poison)

<a name="pattern"></a>

### <a name="name-patterns"></a>Namensmuster
Sie können in der `path` -Eigenschaft ein Blobnamensmuster angeben. Beispiel:

```json
"path": "input/original-{name}",
```

Dieser Pfad findet ein Blob mit dem Namen *original-Blob1.txt* im Container *input*, und der Wert der Variablen `name` im Funktionscode lautet `Blob1`.

Ein weiteres Beispiel:

```json
"path": "input/{blobname}.{blobextension}",
```

Mit diesem Pfad erhalten Sie ein Blob mit dem Namen *original-Blob1.txt*, und die Werte der Variablen `blobname` und `blobextension` im Funktionscode lauten *original-Blob1* und *txt*.

Sie können den Dateityp von Blobs beschränken, indem Sie einen festen Wert als Dateierweiterung verwenden. Beispiel:

```json
"path": "samples/{name}.png",
```

In diesem Fall wird die Funktion nur mit *.png*-Blobs im Container *samples* ausgelöst.

Geschweifte Klammern werden in Namensmustern als Sonderzeichen angesehen. Sie können die geschweiften Klammern verdoppeln, um Blobnamen anzugeben, deren Name geschweifte Klammern enthält. Beispiel:

```json
"path": "images/{{20140101}}-{name}",
```

Dieser Pfad findet ein Blob mit dem Namen *{20140101}-soundfile.mp3* im Container *images*, und der Wert der Variablen `name` im Funktionscode lautet *soundfile.mp3*. 

<a name="receipts"></a>

### <a name="blob-receipts"></a>BLOB-Zugänge
Die Azure Functions-Laufzeit stellt sicher, dass Blobtriggerfunktionen für ein neues oder aktualisiertes Blob nicht mehrmals aufgerufen werden. Zu diesem Zweck wird mittels Verwaltung der *Blobbelege* ermittelt, ob eine bestimmte Blobversion verarbeitet wurde.

Blobbelege werden in einem Container mit dem Namen *azure-webjobs-hosts* im Azure-Speicherkonto für Ihre Funktionen-App gespeichert (per App-Einstellung `AzureWebJobsStorage` angegeben). Ein Blobbeleg enthält die folgenden Informationen:

* Ausgelöste Funktion („*&lt;Funktionen-App-Name>*.Functions.*&lt;Funktionsname>*“, z.B. „functionsf74b96f7.Functions.CopyBlob“)
* Containername
* Blobtyp ("BlockBlob" oder "PageBlob")
* Blobname
* ETag (eine Blobversions-ID. Beispiel: 0x8D1DC6E70A277EF)

Um eine erneute Verarbeitung eines Blobs zu erzwingen, können Sie den Blobbeleg für dieses Blob manuell aus dem Container *azure-webjobs-hosts* löschen.

<a name="poison"></a>

### <a name="handling-poison-blobs"></a>Verarbeiten von nicht verarbeitbaren Blobs
Wenn bei Ausführung einer Blobtrigger-Funktion ein Fehler auftritt, versucht Azure Functions standardmäßig bis zu fünfmal (einschließlich des ersten Versuchs), diese Funktion für ein bestimmtes Blob auszuführen. Wenn alle fünf Versuche nicht gelingen, fügt Functions einer Storage-Warteschlange mit dem Namen *webjobs-blobtrigger-poison* eine Nachricht hinzu. Die Warteschlangennachricht für nicht verarbeitbare Blobs ist ein JSON-Objekt, das die folgenden Eigenschaften enthält:

* FunctionId (im Format *&lt;Funktionen-App-Name>*.Functions.*&lt;Funktionsname>*)
* BlobType ("BlockBlob" oder "PageBlob")
* ContainerName
* BlobName
* ETag (eine Blobversions-ID. Beispiel: 0x8D1DC6E70A277EF)

### <a name="blob-polling-for-large-containers"></a>Abfragen von Blobs für große Container
Wenn der Blobcontainer, der von der Bindung überwacht wird, mehr als 10.000 Blobs enthält, überprüft die Functions-Laufzeit Protokolldateien auf neue oder geänderte Blobs. Dieser Prozess läuft nicht in Echtzeit ab. Eine Funktion wird unter Umständen erst mehrere Minuten nach der Bloberstellung oder noch später ausgelöst. Außerdem erfolgt das [Erstellen von Storage-Protokollen auf bestmögliche Weise](https://msdn.microsoft.com/library/azure/hh343262.aspx). Es gibt aber keine Garantie, dass alle Ereignisse erfasst werden. Unter bestimmten Umständen können Protokolle fehlen. Wenn die Einschränkungen bei der Geschwindigkeit und Zuverlässigkeit von Blobtriggern für große Container für Ihre Anwendung nicht akzeptabel sind, empfiehlt es sich, zusammen mit dem Blob eine [Warteschlangennachricht](../storage/storage-dotnet-how-to-use-queues.md) zu erstellen und für die Verarbeitung des Blobs anstelle des Blobtriggers einen [Warteschlangentrigger](functions-bindings-storage-queue.md) zu verwenden.

<a name="triggerusage"></a>

## <a name="trigger-usage"></a>Triggerverwendung
In C#-Funktionen stellen Sie mit einem benannten Parameter in Ihrer Funktionssignatur, z.B. `<T> <name>`, eine Bindung an Eingabeblobdaten her.
Dabei ist `T` der Datentyp, in den Sie die Daten deserialisieren möchten, und `paramName` ist der Name, den Sie im [JSON-Code des Triggers](#trigger) angegeben haben. In Node.js-Funktionen greifen Sie mit `context.bindings.<name>` auf die Eingabeblobdaten zu.

Das Blob kann in die folgenden Typen deserialisiert werden:

* Beliebiges [Objekt](https://msdn.microsoft.com/library/system.object.aspx) – nützlich für per JSON serialisierte Blobdaten.
  Wenn Sie einen benutzerdefinierten Eingabetyp deklarieren (z.B. `FooType`), versucht Azure Functions, die JSON-Daten in den angegebenen Typ zu deserialisieren.
* Zeichenfolge – nützlich für Textblobdaten.

In C#-Funktionen können Sie auch eine Bindung an folgende Typen einrichten. Die Functions-Laufzeit versucht, die Blobdaten mithilfe dieses Typs zu deserialisieren:

* `TextReader`
* `Stream`
* `ICloudBlob`
* `CloudBlockBlob`
* `CloudPageBlob`
* `CloudBlobContainer`
* `CloudBlobDirectory`
* `IEnumerable<CloudBlockBlob>`
* `IEnumerable<CloudPageBlob>`
* Andere von [ICloudBlobStreamBinder](../app-service-web/websites-dotnet-webjobs-sdk-storage-blobs-how-to.md#icbsb) 

## <a name="trigger-sample"></a>Triggerbeispiel
Nehmen wir an, dass Sie über die folgende Datei „function.json“ verfügen, die einen Storage-Blobtrigger definiert:

```json
{
    "disabled": false,
    "bindings": [
        {
            "name": "myBlob",
            "type": "blobTrigger",
            "direction": "in",
            "path": "samples-workitems",
            "connection":""
        }
    ]
}
```

Beachten Sie das sprachspezifische Beispiel für das Protokollieren der Inhalte jedes Blobs, der dem überwachten Container hinzugefügt wird.

* [C#](#triggercsharp)
* [Node.js](#triggernodejs)

<a name="triggercsharp"></a>

### <a name="trigger-usage-in-c"></a>Triggerverwendung in C# #

```cs
public static void Run(string myBlob, TraceWriter log)
{
    log.Info($"C# Blob trigger function processed: {myBlob}");
}
```

<!--
<a name="triggerfsharp"></a>
### Trigger usage in F# ##
```fsharp

``` 
-->

<a name="triggernodejs"></a>

### <a name="trigger-usage-in-nodejs"></a>Triggerverwendung in Node.js

```javascript
module.exports = function(context) {
    context.log('Node.js Blob trigger function processed', context.bindings.myBlob);
    context.done();
};
```

<a name="input"></a>

## <a name="storage-blob-input-binding"></a>Storage-Blobeingabebindung
Mit der Azure Storage-Blobeingabebindung können Sie ein Blob aus einem Speichercontainer in Ihrer Funktion verwenden. 

Für die Storage-Blobeingabe in eine Funktion werden die folgenden JSON-Objekte im `bindings`-Array von „function.json“ verwendet:

```json
{
  "name": "<Name of input parameter in function signature>",
  "type": "blob",
  "direction": "in"
  "path": "<Path of input blob - see below>",
  "connection":"<Name of app setting - see below>"
},
```

Beachten Sie Folgendes:

* `path` muss den Containernamen und den Blobnamen enthalten. Wenn Sie in Ihrer Funktion beispielsweise über einen [Warteschlangentrigger](functions-bindings-storage-queue.md) verfügen, können Sie mit `"path": "samples-workitems/{queueTrigger}"` auf ein Blob im Container `samples-workitems` mit einem Namen verweisen, der mit dem in der Triggernachricht angegebenen Blobnamen übereinstimmt.   
* `connection` muss den Namen einer App-Einstellung enthalten, die eine Speicherverbindungszeichenfolge enthält. Im Azure-Portal konfiguriert der Standard-Editor auf der Registerkarte **Integrieren** diese App-Einstellung für Sie, wenn Sie ein Speicherkonto erstellen oder ein vorhandenes auswählen. Wie Sie diese App-Einstellung manuell erstellen, erfahren Sie unter [Manuelles Konfigurieren dieser App-Einstellung](functions-how-to-use-azure-function-app-settings.md). 

<a name="inputusage"></a>

## <a name="input-usage"></a>Eingabeverwendung
In C#-Funktionen stellen Sie mit einem benannten Parameter in Ihrer Funktionssignatur, z.B. `<T> <name>`, eine Bindung an Eingabeblobdaten her.
Dabei ist `T` der Datentyp, in den Sie die Daten deserialisieren möchten, und `paramName` ist der Name, den Sie in der [Eingabebindung](#input) angegeben haben. In Node.js-Funktionen greifen Sie mit `context.bindings.<name>` auf die Eingabeblobdaten zu.

Das Blob kann in die folgenden Typen deserialisiert werden:

* Beliebiges [Objekt](https://msdn.microsoft.com/library/system.object.aspx) – nützlich für per JSON serialisierte Blobdaten.
  Wenn Sie einen benutzerdefinierten Eingabetyp deklarieren (z.B. `InputType`), versucht Azure Functions, die JSON-Daten in den angegebenen Typ zu deserialisieren.
* Zeichenfolge – nützlich für Textblobdaten.

In C#-Funktionen können Sie auch eine Bindung an folgende Typen einrichten. Die Functions-Laufzeit versucht, die Blobdaten mithilfe dieses Typs zu deserialisieren:

* `TextReader`
* `Stream`
* `ICloudBlob`
* `CloudBlockBlob` 
* `CloudPageBlob` 

<a name="inputsample"></a>

## <a name="input-sample"></a>Eingabebeispiel
Nehmen wir an, dass Sie über die folgende Datei „function.json“ verfügen, die einen [Storage-Warteschlangentrigger](functions-bindings-storage-queue.md), eine Storage-Blobeingabe und eine Storage-Blobausgabe definiert:

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

### <a name="input-usage-in-c"></a>Eingabeverwendung in C# #

```cs
public static void Run(string myQueueItem, string myInputBlob, out string myOutputBlob, TraceWriter log)
{
    log.Info($"C# Queue trigger function processed: {myQueueItem}");
    myOutputBlob = myInputBlob;
}
```

<!--
<a name="infsharp"></a>
### Input usage in F# ##
```fsharp

``` 
-->

<a name="innodejs"></a>

### <a name="input-usage-in-nodejs"></a>Eingabeverwendung in Node.js

```javascript
module.exports = function(context) {
    context.log('Node.js Queue trigger function processed', context.bindings.myQueueItem);
    context.bindings.myOutputBlob = context.bindings.myInputBlob;
    context.done();
};
```

<a name="output"></a>

## <a name="storage-blob-output-binding"></a>Storage-Blobausgabebindung
Mit der Azure Storage-Blobausgabebindung können Sie Blobs in einen Speichercontainer in Ihrer Funktion schreiben. 

Die Storage-Blobausgabe für eine Funktion verwendet die folgenden JSON-Objekte im `bindings`-Array von „function.json“:

```json
{
  "name": "<Name of output parameter in function signature>",
  "type": "blob",
  "direction": "out",
  "path": "<Path of input blob - see below>",
  "connection": "<Name of app setting - see below>"
}
```

Beachten Sie Folgendes:

* `path` muss den Containernamen und den Blobnamen für den Schreibvorgang enthalten. Wenn Sie in Ihrer Funktion beispielsweise über einen [Warteschlangentrigger](functions-bindings-storage-queue.md) verfügen, können Sie mit `"path": "samples-workitems/{queueTrigger}"` auf ein Blob im Container `samples-workitems` mit einem Namen verweisen, der mit dem in der Triggernachricht angegebenen Blobnamen übereinstimmt.   
* `connection` muss den Namen einer App-Einstellung enthalten, die eine Speicherverbindungszeichenfolge enthält. Im Azure-Portal konfiguriert der Standard-Editor auf der Registerkarte **Integrieren** diese App-Einstellung für Sie, wenn Sie ein Speicherkonto erstellen oder ein vorhandenes auswählen. Wie Sie diese App-Einstellung manuell erstellen, erfahren Sie unter [Manuelles Konfigurieren dieser App-Einstellung](functions-how-to-use-azure-function-app-settings.md). 

<a name="outputusage"></a>

## <a name="output-usage"></a>Ausgabeverwendung
In C#-Funktionen stellen Sie eine Bindung an das Ausgabeblob mithilfe des benannten `out`-Parameters, z.B. `out <T> <name>`, in Ihrer Funktionssignatur her. Hierbei ist `T` der Datentyp, in den Sie die Daten serialisieren möchten, und `paramName` ist der Name, den Sie in der [Ausgabebindung](#output) angegeben haben. In Node.js-Funktionen greifen Sie mit `context.bindings.<name>` auf das Ausgabeblob zu.

Sie können in das Ausgabeblob schreiben, indem Sie einen der folgenden Typen verwenden:

* Beliebiges [Objekt](https://msdn.microsoft.com/library/system.object.aspx) – nützlich für JSON-Serialisierung.
  Wenn Sie einen benutzerdefinierten Ausgabetyp deklarieren (z.B. `out OutputType paramName`), versucht Azure Functions, das Objekt in JSON zu serialisieren. Wenn der Ausgabeparameter bei Beendigung der Funktion NULL ist, erstellt die Functions-Laufzeit ein Blob als NULL-Objekt.
* Zeichenfolge – (`out string paramName`) nützlich für Textblobdaten. Die Functions-Laufzeit erstellt nur dann ein Blob, wenn der Zeichenfolgenparameter bei Rückgabe durch die Funktion ungleich NULL ist.

In C#-Funktionen können Sie auch eine Ausgabe in einen der folgenden Typen erstellen:

* `TextWriter`
* `Stream`
* `CloudBlobStream`
* `ICloudBlob`
* `CloudBlockBlob` 
* `CloudPageBlob` 

<a name="outputsample"></a>

## <a name="output-sample"></a>Ausgabebeispiel
Siehe [Eingabebeispiel](#inputsample).

## <a name="next-steps"></a>Nächste Schritte
[!INCLUDE [next steps](../../includes/functions-bindings-next-steps.md)]


