---
title: Cosmos DB-Bindungen in Azure Functions | Microsoft-Dokumentation
description: Erfahren Sie, wie Azure Cosmos DB-Bindungen in Azure Functions verwendet werden.
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
ms.date: 04/18/2016
ms.author: chrande; glenga
ms.translationtype: Human Translation
ms.sourcegitcommit: 71fea4a41b2e3a60f2f610609a14372e678b7ec4
ms.openlocfilehash: 2c0cb8ee1690f9b36b76c87247e3c7223876b269
ms.contentlocale: de-de
ms.lasthandoff: 05/10/2017


---
# <a name="azure-functions-cosmos-db-bindings"></a>Cosmos DB-Bindungen in Azure Functions
[!INCLUDE [functions-selector-bindings](../../includes/functions-selector-bindings.md)]

Dieser Artikel erläutert das Konfigurieren und Codieren von Azure Cosmos DB-Bindungen in Azure Functions. Azure Functions unterstützt Eingabe- und Ausgabebindungen für Cosmos DB.

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

Weitere Informationen zu Cosmos DB finden Sie unter [Einführung in Cosmos DB](../documentdb/documentdb-introduction.md) und [Erstellen einer Cosmos DB-Konsolenanwendung](../documentdb/documentdb-get-started.md).

<a id="docdbinput"></a>

## <a name="documentdb-api-input-binding"></a>DocumentDB-API-Eingabebindung
Die DocumentDB-API-Eingabebindung ruft ein Cosmos DB-Dokument ab und übergibt es an den benannten Eingabeparameter der Funktion. Die Dokument-ID kann basierend auf dem Trigger ermittelt werden, der die Funktion aufruft. 

Die DocumentDB-API-Eingabebindung hat in *function.json* die folgenden Eigenschaften:

- `name`: Bezeichnername, der im Funktionscode für das Dokument verwendet wird.
- `type`: Muss auf „documentdb“ festgelegt werden.
- `databaseName`: Die Datenbank mit dem Dokument.
- `collectionName`: Die Sammlung mit dem Dokument.
- `id`: Die ID des abzurufenden Dokuments. Diese Eigenschaft unterstützt Bindungsparameter. Siehe [Binden an benutzerdefinierten Eingabeeigenschaften in einem Bindungsausdruck](functions-triggers-bindings.md#bind-to-custom-input-properties-in-a-binding-expression) im Artikel [Konzepte für Azure Functions-Trigger und -Bindungen](functions-triggers-bindings.md).
- `sqlQuery`: Eine SQL-Abfrage in Cosmos DB zum Abrufen mehrerer Dokumente. Die Abfrage unterstützt Bindungen zur Laufzeit. Beispiel: `SELECT * FROM c where c.departmentId = {departmentId}`
- `connection`: Der Name der App-Einstellung mit Ihrer Cosmos DB-Verbindungszeichenfolge.
- `direction`: Muss auf `"in"` festgelegt werden.

Die Eigenschaften `id` und `sqlQuery` können nicht beide angegeben werden. Wenn weder `id` noch `sqlQuery` festgelegt ist, wird die gesamte Sammlung abgerufen.

## <a name="using-a-documentdb-api-input-binding"></a>Verwenden einer DocumentDB-API-Eingabebindung

* Wenn in C#- und F#-Funktionen die Funktion erfolgreich beendet wird, werden alle Änderungen am Eingabedokument mithilfe benannter Eingabeparameter automatisch beibehalten. 
* In JavaScript-Funktionen erfolgen Aktualisierungen bei Beenden der Funktion nicht automatisch. Verwenden Sie stattdessen `context.bindings.<documentName>In` und `context.bindings.<documentName>Out`, um Aktualisierungen vorzunehmen. Siehe das [JavaScript-Beispiel](#injavascript).

<a name="inputsample"></a>

## <a name="input-sample-for-single-document"></a>Eingabebeispiel für einzelnes Dokument
Angenommen, die folgende DocumentDB-API-Eingabebindung befindet sich im `bindings`-Array von „function.json“:

```json
{
  "name": "inputDocument",
  "type": "documentDB",
  "databaseName": "MyDatabase",
  "collectionName": "MyCollection",
  "id" : "{queueTrigger}",
  "connection": "MyAccount_COSMOSDB",     
  "direction": "in"
}
```

Sehen Sie sich das sprachspezifische Beispiel an, in dem diese Eingabebindung zum Aktualisieren des Textwerts des Dokuments verwendet wird.

* [C#](#incsharp)
* [F#](#infsharp)
* [JavaScript](#injavascript)

<a name="incsharp"></a>
### <a name="input-sample-in-c"></a>Eingabebeispiel in C# #

```cs
// Change input document contents using DocumentDB API input binding 
public static void Run(string myQueueItem, dynamic inputDocument)
{   
  inputDocument.text = "This has changed.";
}
```
<a name="infsharp"></a>

### <a name="input-sample-in-f"></a>Eingabebeispiel in F# #

```fsharp
(* Change input document contents using DocumentDB API input binding *)
open FSharp.Interop.Dynamic
let Run(myQueueItem: string, inputDocument: obj) =
  inputDocument?text <- "This has changed."
```

Dieses Beispiel erfordert die Datei `project.json`, die die NuGet-Abhängigkeiten `FSharp.Interop.Dynamic` und `Dynamitey` angibt:

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

<a name="injavascript"></a>

### <a name="input-sample-in-javascript"></a>Eingabebeispiel in JavaScript

```javascript
// Change input document contents using DocumentDB API input binding, using context.bindings.inputDocumentOut
module.exports = function (context) {   
  context.bindings.inputDocumentOut = context.bindings.inputDocumentIn;
  context.bindings.inputDocumentOut.text = "This was updated!";
  context.done();
};
```

## <a name="input-sample-with-multiple-documents"></a>Eingabebeispiel mit mehreren Dokumenten

Angenommen, Sie möchten mehrere von einer SQL-Abfrage angegebene Dokumente mithilfe eines Warteschlangentriggers abrufen, um die Abfrageparameter anzupassen. 

In diesem Beispiel stellt der Warteschlangentrigger den Parameter `departmentId` bereit. Die Warteschlangennachricht `{ "departmentId" : "Finance" }` gibt dann alle Datensätze für die Finanzabteilung zurück. Verwenden Sie in *function.json* Folgendes:

```
{
    "name": "documents",
    "type": "documentdb",
    "direction": "in",
    "databaseName": "MyDb",
    "collectionName": "MyCollection",
    "sqlQuery": "SELECT * from c where c.departmentId = {departmentId}"
    "connection": "CosmosDBConnection"
}
```

### <a name="input-sample-with-multiple-documents-in-c"></a>Eingabebeispiel mit mehreren Dokumenten in C#

```csharp
public static void Run(QueuePayload myQueueItem, IEnumerable<dynamic> documents)
{   
    foreach (var doc in documents)
    {
        // operate on each document
    }    
}

public class QueuePayload
{
    public string departmentId { get; set; }
}
```

### <a name="input-sample-with-multiple-documents-in-javascript"></a>Eingabebeispiel mit mehreren Dokumenten in JavaScript

```javascript
module.exports = function (context, input) {    
    var documents = context.bindings.documents;
    for (var i = 0; i < documents.length; i++) {
        var document = documents[i];
        // operate on each document
    }        
    context.done();
};
```

## <a id="docdboutput"></a>DocumentDB-API-Ausgabebindung
Die DocumentDB-API-Ausgabebindung ermöglicht das Schreiben eines neuen Dokuments in eine Azure Cosmos DB-Datenbank. Sie hat in *function.json* die folgenden Eigenschaften:

- `name`: Bezeichner, der im Funktionscode für das neue Dokument verwendet wird.
- `type`: Muss auf `"documentdb"` festgelegt werden.
- `databaseName`: Datenbank mit der Sammlung, in der das neue Dokument erstellt wird.
- `collectionName`: Sammlung, in der das neue Dokument erstellt wird.
- `createIfNotExists`: Boolescher Wert, der angibt, ob die Sammlung erstellt werden soll, wenn sie nicht vorhanden ist. Die Standardeinstellung ist *false*. Der Grund hierfür ist, dass Sammlungen mit reserviertem Durchsatz erstellt werden, was sich auf den Preis auswirkt. Weitere Informationen finden Sie in der [Preisübersicht](https://azure.microsoft.com/pricing/details/documentdb/).
- `connection`: Der Name der App-Einstellung mit Ihrer Cosmos DB-Verbindungszeichenfolge.
- `direction`: Muss auf `"out"` festgelegt werden.

## <a name="using-a-documentdb-api-output-binding"></a>Verwenden einer DocumentDB-API-Ausgabebindung
Dieser Abschnitt veranschaulicht die Verwendung Ihrer DocumentDB-API-Ausgabebindung in Ihrem Funktionscode.

Beim Schreiben in den Ausgabeparameter in Ihrer Funktion wird standardmäßig ein neues Dokument mit einer automatisch generierten GUID als Dokument-ID in Ihrer Datenbank erstellt. Sie können die Dokument-ID des Ausgabedokument angeben, indem Sie im Ausgabeparameter die JSON-Eigenschaft `id` festlegen. 

>[!Note]  
>Wenn Sie die ID eines vorhandenen Dokuments angeben, wird dieses vom neuen Ausgabedokument überschrieben. 

Für die Ausgabe mehrerer Dokumente können Sie auch eine Bindung mit `ICollector<T>` oder `IAsyncCollector<T>` erstellen, wobei `T` einer der unterstützten Typen ist.

<a name="outputsample"></a>

## <a name="documentdb-api-output-binding-sample"></a>Beispiel einer DocumentDB-API-Ausgabebindung
Angenommen, die folgende DocumentDB-API-Ausgabebindung befindet sich im `bindings`-Array von „function.json“:

```json
{
  "name": "employeeDocument",
  "type": "documentDB",
  "databaseName": "MyDatabase",
  "collectionName": "MyCollection",
  "createIfNotExists": true,
  "connection": "MyAccount_COSMOSDB",     
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

Sie möchten nun Cosmos DB-Dokumente im folgenden Format für die einzelnen Datensätze erstellen:

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
* [JavaScript](#outjavascript)

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

Dieses Beispiel erfordert die Datei `project.json`, die die NuGet-Abhängigkeiten `FSharp.Interop.Dynamic` und `Dynamitey` angibt:

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

<a name="outjavascript"></a>

### <a name="output-sample-in-javascript"></a>Ausgabebeispiel in JavaScript

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

