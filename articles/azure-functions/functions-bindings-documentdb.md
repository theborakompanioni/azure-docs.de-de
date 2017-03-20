---
title: DocumentDB-Bindungen in Azure Functions | Microsoft-Dokumentation
description: Erfahren Sie, wie Azure DocumentDB-Bindungen in Azure Functions verwendet werden.
services: functions
documentationcenter: na
author: christopheranderson
manager: erikre
editor: 
tags: 
keywords: Azure Functions, Funktionen, Ereignisverarbeitung, dynamisches Compute, serverlose Architektur
ms.assetid: 3d8497f0-21f3-437d-ba24-5ece8c90ac85
ms.service: functions
ms.devlang: multiple
ms.topic: reference
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 11/10/2016
ms.author: chrande; glenga
translationtype: Human Translation
ms.sourcegitcommit: d9dad6cff80c1f6ac206e7fa3184ce037900fc6b
ms.openlocfilehash: e476a80a3846b8c80c35d6803d5518727f008824
ms.lasthandoff: 03/06/2017


---
# <a name="azure-functions-documentdb-bindings"></a>DocumentDB-Bindungen in Azure Functions
[!INCLUDE [functions-selector-bindings](../../includes/functions-selector-bindings.md)]

Dieser Artikel erläutert das Konfigurieren und Codieren von Azure DocumentDB-Bindungen in Azure Functions. Azure Functions unterstützt Eingabe- und Ausgabebindungen für DocumentDB.

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

Weitere Informationen zu DocumentDB finden Sie in der [Einführung in DocumentDB](../documentdb/documentdb-introduction.md) und im Thema zum [Erstellen einer DocumentDB-Konsolenanwendung](../documentdb/documentdb-get-started.md).

<a id="docdbinput"></a>

## <a name="documentdb-input-binding"></a>DocumentDB-Eingabebindung
Die DocumentDB-Eingabebindung ruft ein DocumentDB-Dokument ab und übergibt es an den benannten Eingabeparameter der Funktion. Die Dokument-ID kann basierend auf dem Trigger ermittelt werden, der die Funktion aufruft. 

Die DocumentDB-Eingabe zu einer Funktion verwendet das folgende JSON-Objekt im `bindings`-Array von „function.json“:

```json
{
  "name": "<Name of input parameter in function signature>",
  "type": "documentDB",
  "databaseName": "<Name of the DocumentDB database>",
  "collectionName": "<Name of the DocumentDB collection>",
  "id": "<Id of the DocumentDB document - see below>",
  "connection": "<Name of app setting with connection string - see below>",
  "direction": "in"
},
```

Beachten Sie Folgendes:

* `id` unterstützt Bindungen wie `{queueTrigger}`, die den Zeichenfolgenwert der Warteschlangennachricht als Dokument-ID verwendet.
* `connection` muss der Name einer App-Einstellung sein, die auf den Endpunkt für Ihr DocumentDB-Konto verweist (Wert: `AccountEndpoint=<Endpoint for your account>;AccountKey=<Your primary access key>`). Wenn Sie über die Functions-Portal-UI ein DocumentDB-Konto erstellen, wird bei der Kontoerstellung eine App-Einstellung für Sie erstellt. Damit Sie ein vorhandenes DocumentDB-Konto verwenden können, müssen Sie [diese App-Einstellung manuell konfigurieren](functions-how-to-use-azure-function-app-settings.md). 
* Wenn das angegebene Dokument nicht gefunden wird, wird der benannte Eingabeparameter zur Funktion auf `null` festgelegt. 

## <a name="input-usage"></a>Eingabeverwendung
Dieser Abschnitt veranschaulicht die Verwendung Ihrer DocumentDB-Eingabebindung in Ihrem Funktionscode.

In C#- und F#-Funktionen werden alle Änderungen am Eingabedokument (benannter Eingabeparameter) automatisch wieder an die Sammlung zurückgesendet, wenn die Funktion erfolgreich beendet wird. In Node.js-Funktionen werden Dokumentaktualisierungen in der Eingabebindung nicht an die Sammlung zurückgesendet. Sie können jedoch mithilfe von `context.bindings.<documentName>In` und `context.bindings.<documentName>Out` Aktualisierungen an Eingabedokumenten vornehmen. Informationen zur Vorgehensweise finden Sie im [Node.js-Beispiel](#innodejs).

<a name="inputsample"></a>

## <a name="input-sample"></a>Eingabebeispiel
Angenommen, die folgende DocumentDB-Eingabebindung befindet sich im `bindings`-Array von „function.json“:

```json
{
  "name": "inputDocument",
  "type": "documentDB",
  "databaseName": "MyDatabase",
  "collectionName": "MyCollection",
  "id" : "{queueTrigger}",
  "connection": "MyAccount_DOCUMENTDB",     
  "direction": "in"
}
```

Sehen Sie sich das sprachspezifische Beispiel an, in dem diese Eingabebindung zum Aktualisieren des Textwerts des Dokuments verwendet wird.

* [C#](#incsharp)
* [F#](#infsharp)
* [Node.js](#innodejs)

<a name="incsharp"></a>
### <a name="input-sample-in-c"></a>Eingabebeispiel in C# #

```cs
public static void Run(string myQueueItem, dynamic inputDocument)
{   
  inputDocument.text = "This has changed.";
}
```
<a name="infsharp"></a>

### <a name="input-sample-in-f"></a>Eingabebeispiel in F# #

```fsharp
open FSharp.Interop.Dynamic
let Run(myQueueItem: string, inputDocument: obj) =
  inputDocument?text <- "This has changed."
```

Sie müssen eine `project.json`-Datei hinzufügen, die die NuGet-Abhängigkeiten `FSharp.Interop.Dynamic` und `Dynamitey` angibt:

```json
{
  "frameworks": {
    "net46": {
      "dependencies": {
        "Dynamitey": "1.0.2",
        "FSharp.Interop.Dynamic": "3.0.0"
      }
    }
  }
}
```

Informationen zum Hinzufügen einer `project.json`-Datei finden Sie unter [Paketverwaltung](functions-reference-fsharp.md#package).

<a name="innodejs"></a>

### <a name="input-sample-in-nodejs"></a>Eingabebeispiel in Node.js

```javascript
module.exports = function (context) {   
  context.bindings.inputDocumentOut = context.bindings.inputDocumentIn;
  context.bindings.inputDocumentOut.text = "This was updated!";
  context.done();
};
```

## <a id="docdboutput"></a>DocumentDB-Ausgabebindung
Die DocumentDB-Ausgabebindung ermöglicht das Schreiben eines neuen Dokuments in eine Azure DocumentDB-Datenbank. 

Die Ausgabebindung verwendet das folgende JSON-Objekte im `bindings`-Array von „function.json“: 

```json
{
  "name": "<Name of output parameter in function signature>",
  "type": "documentDB",
  "databaseName": "<Name of the DocumentDB database>",
  "collectionName": "<Name of the DocumentDB collection>",
  "createIfNotExists": <true or false - see below>,
  "connection": "<Value of AccountEndpoint in Application Setting - see below>",
  "direction": "out"
}
```

Beachten Sie Folgendes:

* Legen Sie `createIfNotExists` auf `true` fest, um die Datenbank und die Sammlung zu erstellen, wenn diese nicht vorhanden sind. Standardwert: `false`. Neue Sammlungen werden mit reserviertem Durchsatz erstellt. Dies wirkt sich auf den Preis aus. Weitere Informationen finden Sie unter [DocumentDB-Preise](https://azure.microsoft.com/pricing/details/documentdb/).
* `connection` muss der Name einer App-Einstellung sein, die auf den Endpunkt für Ihr DocumentDB-Konto verweist (Wert: `AccountEndpoint=<Endpoint for your account>;AccountKey=<Your primary access key>`). Wenn Sie über die Functions-Portal-UI ein DocumentDB-Konto erstellen, wird bei der Kontoerstellung eine neue App-Einstellung für Sie erstellt. Damit Sie ein vorhandenes DocumentDB-Konto verwenden können, müssen Sie [diese App-Einstellung manuell konfigurieren](). 

## <a name="output-usage"></a>Ausgabeverwendung
Dieser Abschnitt veranschaulicht die Verwendung Ihrer DocumentDB-Ausgabebindung in Ihrem Funktionscode.

Beim Schreiben in den Ausgabeparameter in Ihrer Funktion wird standardmäßig ein neues Dokument mit einer automatisch generierten GUID als Dokument-ID in Ihrer Datenbank erstellt. Sie können die Dokument-ID des Ausgabedokument angeben, indem Sie im Ausgabeparameter die JSON-Eigenschaft `id` festlegen. 

>[!Note]  
>Wenn Sie die ID eines vorhandenen Dokuments angeben, wird dieses vom neuen Ausgabedokument überschrieben. 

Sie können in die Ausgabe schreiben, indem Sie einen der folgenden Typen verwenden:

* Beliebiges [Objekt](https://msdn.microsoft.com/library/system.object.aspx) – nützlich für JSON-Serialisierung.
  Wenn Sie einen benutzerdefinierten Ausgabetyp deklarieren (z.B. `out FooType paramName`), versucht Azure Functions, das Objekt in JSON zu serialisieren. Wenn der Ausgabeparameter bei Beendigung der Funktion NULL ist, erstellt die Functions-Laufzeit ein Blob als NULL-Objekt.
* Zeichenfolge – (`out string paramName`) nützlich für Textblobdaten. Die Functions-Laufzeit erstellt nur dann ein Blob, wenn der Zeichenfolgenparameter bei Rückgabe durch die Funktion ungleich NULL ist.

In C#-Funktionen können Sie auch eine Ausgabe in einen der folgenden Typen erstellen:

* `TextWriter`
* `Stream`
* `CloudBlobStream`
* `ICloudBlob`
* `CloudBlockBlob` 
* `CloudPageBlob` 

Für die Ausgabe mehrerer Dokumente können Sie auch eine Bindung mit `ICollector<T>` oder `IAsyncCollector<T>` erstellen, wobei `T` einer der unterstützten Typen ist.


<a name="outputsample"></a>

## <a name="output-sample"></a>Ausgabebeispiel
Angenommen, die folgende DocumentDB-Ausgabebindung befindet sich im `bindings`-Array von „function.json“:

```json
{
  "name": "employeeDocument",
  "type": "documentDB",
  "databaseName": "MyDatabase",
  "collectionName": "MyCollection",
  "createIfNotExists": true,
  "connection": "MyAccount_DOCUMENTDB",     
  "direction": "out"
}
```

Darüber hinaus liegt eine Warteschlangeneingabebindung für eine Warteschlange vor, die JSON-Code im folgenden Format empfängt:

```json
{
  "name": "John Henry",
  "employeeId": "123456",
  "address": "A town nearby"
}
```

Sie möchten nun DocumentDB-Dokumente im folgenden Format für die einzelnen Datensätze erstellen:

```json
{
  "id": "John Henry-123456",
  "name": "John Henry",
  "employeeId": "123456",
  "address": "A town nearby"
}
```

Sehen Sie sich das sprachspezifische Beispiel an, in dem diese Ausgabebindung zum Hinzufügen von Dokumenten zur Datenbank verwendet wird.

* [C#](#outcsharp)
* [F#](#outfsharp)
* [Node.js](#outnodejs)

<a name="outcsharp"></a>

### <a name="output-sample-in-c"></a>Ausgabebeispiel in C# #

```cs
#r "Newtonsoft.Json"

using System;
using Newtonsoft.Json;
using Newtonsoft.Json.Linq;

public static void Run(string myQueueItem, out object employeeDocument, TraceWriter log)
{
  log.Info($"C# Queue trigger function processed: {myQueueItem}");

  dynamic employee = JObject.Parse(myQueueItem);

  employeeDocument = new {
    id = employee.name + "-" + employee.employeeId,
    name = employee.name,
    employeeId = employee.employeeId,
    address = employee.address
  };
}
```

<a name="outfsharp"></a>

### <a name="output-sample-in-f"></a>Ausgabebeispiel in F# #

```fsharp
open FSharp.Interop.Dynamic
open Newtonsoft.Json

type Employee = {
  id: string
  name: string
  employeeId: string
  address: string
}

let Run(myQueueItem: string, employeeDocument: byref<obj>, log: TraceWriter) =
  log.Info(sprintf "F# Queue trigger function processed: %s" myQueueItem)
  let employee = JObject.Parse(myQueueItem)
  employeeDocument <-
    { id = sprintf "%s-%s" employee?name employee?employeeId
      name = employee?name
      employeeId = employee?employeeId
      address = employee?address }
```

Sie müssen eine `project.json`-Datei hinzufügen, die die NuGet-Abhängigkeiten `FSharp.Interop.Dynamic` und `Dynamitey` angibt:

```json
{
  "frameworks": {
    "net46": {
      "dependencies": {
        "Dynamitey": "1.0.2",
        "FSharp.Interop.Dynamic": "3.0.0"
      }
    }
  }
}
```

Informationen zum Hinzufügen einer `project.json`-Datei finden Sie unter [Paketverwaltung](functions-reference-fsharp.md#package).

<a name="outnodejs"></a>

### <a name="output-sample-in-nodejs"></a>Ausgabebeispiel in Node.js

```javascript
module.exports = function (context) {

  context.bindings.employeeDocument = JSON.stringify({ 
    id: context.bindings.myQueueItem.name + "-" + context.bindings.myQueueItem.employeeId,
    name: context.bindings.myQueueItem.name,
    employeeId: context.bindings.myQueueItem.employeeId,
    address: context.bindings.myQueueItem.address
  });

  context.done();
};
```

