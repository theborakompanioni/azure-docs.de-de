---
title: Azure Storage-Tabellenbindungen in Azure Functions | Microsoft Docs
description: Erfahren Sie, wie Azure Storage-Bindungen in Azure Functions verwendet werden.
services: functions
documentationcenter: na
author: christopheranderson
manager: erikre
editor: 
tags: 
keywords: Azure Functions, Funktionen, Ereignisverarbeitung, dynamisches Compute, serverlose Architektur
ms.assetid: 65b3437e-2571-4d3f-a996-61a74b50a1c2
ms.service: functions
ms.devlang: multiple
ms.topic: reference
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 10/28/2016
ms.author: chrande
translationtype: Human Translation
ms.sourcegitcommit: b0c27ca561567ff002bbb864846b7a3ea95d7fa3
ms.openlocfilehash: bb01be3ee044f60376e0c9c2de7b3dd34f3b7aca
ms.lasthandoff: 04/25/2017


---
# <a name="azure-functions-storage-table-bindings"></a>Azure Storage-Tabellenbindungen in Azure Functions
[!INCLUDE [functions-selector-bindings](../../includes/functions-selector-bindings.md)]

Dieser Artikel erläutert das Konfigurieren und Codieren von Azure Storage-Tabellenbindungen in Azure Functions. Azure Functions unterstützt Eingabe- und Ausgabebindungen für Azure Storage-Tabellen.

Die Storage-Tabellenbindung unterstützt die folgenden Szenarien:

* **Lesen einer einzelnen Zeile in einer C#- oder Node.js-Funktion**: Legen Sie `partitionKey` und `rowKey` fest. Die Eigenschaften `filter` und `take` werden in diesem Szenario nicht verwendet.
* **Lesen mehrerer Zeilen in einer C#-Funktion**: Die Functions-Runtime gibt ein an die Tabelle gebundenes `IQueryable<T>`-Objekt an. Der Typ `T` muss von `TableEntity` abgeleitet sein oder `ITableEntity` implementieren. Die Eigenschaften `partitionKey`, `rowKey`, `filter` und `take` werden in diesem Szenario nicht verwendet. Erforderliche Filtervorgänge können über das `IQueryable`-Objekt erfolgen. 
* **Lesen mehrerer Zeilen in einer Node-Funktion**: Legen Sie die Eigenschaften `filter` und `take` fest. Legen Sie `partitionKey` oder `rowKey` nicht fest.
* **Schreiben mindestens einer Zeile in einer C#-Funktion**: Die Functions-Runtime stellt einen an die Tabelle gebundenen `ICollector<T>` oder `IAsyncCollector<T>` bereit, wobei `T` das Schema der Entitäten angibt, die Sie hinzufügen möchten. In der Regel ist der Typ `T` von `TableEntity` abgeleitet oder implementiert `ITableEntity`, dies ist aber nicht zwingend. Die Eigenschaften `partitionKey`, `rowKey`, `filter` und `take` werden in diesem Szenario nicht verwendet.

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

<a name="input"></a>

## <a name="storage-table-input-binding"></a>Storage-Tabelleneingabebindung
Mit der Azure Storage-Tabelleneingabebindung können Sie eine Speichertabelle in Ihrer Funktion verwenden. 

Die Storage-Tabelleneingabe in eine Funktion verwendet die folgenden JSON-Objekte im `bindings`-Array von „function.json“:

```json
{
    "name": "<Name of input parameter in function signature>",
    "type": "table",
    "direction": "in",
    "tableName": "<Name of Storage table>",
    "partitionKey": "<PartitionKey of table entity to read - see below>",
    "rowKey": "<RowKey of table entity to read - see below>",
    "take": "<Maximum number of entities to read in Node.js - optional>",
    "filter": "<OData filter expression for table input in Node.js - optional>",
    "connection": "<Name of app setting - see below>",
}
```

Beachten Sie Folgendes: 

* Verwenden Sie `partitionKey` und `rowKey` zusammen, um eine einzelne Entität zu lesen. Diese Eigenschaften sind optional. 
* `connection` muss den Namen einer App-Einstellung enthalten, die eine Speicherverbindungs-Zeichenfolge enthält. Im Azure-Portal konfiguriert der Standard-Editor auf der Registerkarte **Integrieren** diese App-Einstellung für Sie, wenn Sie ein Speicherkonto erstellen oder ein vorhandenes auswählen. Sie können [diese App-Einstellung auch manuell konfigurieren](functions-how-to-use-azure-function-app-settings.md#settings).  

<a name="inputusage"></a>

## <a name="input-usage"></a>Eingabeverwendung
In C#-Funktionen stellen Sie mit einem benannten Parameter in Ihrer Funktionssignatur, z.B. `<T> <name>`, eine Bindung an die Eingabetabellenentität (bzw. Entitäten) her.
Dabei ist `T` der Datentyp, in den Sie die Daten deserialisieren möchten, und `paramName` ist der Name, den Sie in der [Eingabebindung](#input) angegeben haben. In Node.js-Funktionen greifen Sie mit `context.bindings.<name>` auf die Eingabetabellenentität (oder die Entitäten) zu.

Die Eingabedaten können in Node.js- oder C#-Funktionen deserialisiert werden. Die deserialisierten Objekte verfügen über die Eigenschaften `RowKey` und `PartitionKey`.

In C#-Funktionen können Sie auch eine Bindung an beliebige der folgenden Typen herstellen, und die Functions-Laufzeit versucht, die Tabellendaten mithilfe dieses Typs zu deserialisieren:

* Jeder Typ, der `ITableEntity` implementiert
* `IQueryable<T>`

<a name="inputsample"></a>

## <a name="input-sample"></a>Eingabebeispiel
Stellen Sie sich vor, Sie haben die folgende „function.json“, die einen Warteschlangentrigger zum Lesen einer einzelnen Tabellenzeile verwendet. Die JSON-Funktion gibt `PartitionKey` 
`RowKey` an. `"rowKey": "{queueTrigger}"` gibt an, dass der Zeilenschlüssel aus der Warteschlangennachricht-Zeichenfolge stammt.

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
      "name": "personEntity",
      "type": "table",
      "tableName": "Person",
      "partitionKey": "Test",
      "rowKey": "{queueTrigger}",
      "connection": "MyStorageConnection",
      "direction": "in"
    }
  ],
  "disabled": false
}
```

Beachten Sie das sprachspezifische Beispiel für das Lesen einer einzelnen Tabellenentität.

* [C#](#inputcsharp)
* [F#](#inputfsharp)
* [Node.js](#inputnodejs)

<a name="inputcsharp"></a>

### <a name="input-sample-in-c"></a>Eingabebeispiel in C# #
```csharp
public static void Run(string myQueueItem, Person personEntity, TraceWriter log)
{
    log.Info($"C# Queue trigger function processed: {myQueueItem}");
    log.Info($"Name in Person entity: {personEntity.Name}");
}

public class Person
{
    public string PartitionKey { get; set; }
    public string RowKey { get; set; }
    public string Name { get; set; }
}
```

<a name="inputfsharp"></a>

### <a name="input-sample-in-f"></a>Eingabebeispiel in F# #
```fsharp
[<CLIMutable>]
type Person = {
  PartitionKey: string
  RowKey: string
  Name: string
}

let Run(myQueueItem: string, personEntity: Person) =
    log.Info(sprintf "F# Queue trigger function processed: %s" myQueueItem)
    log.Info(sprintf "Name in Person entity: %s" personEntity.Name)
```

<a name="inputnodejs"></a>

### <a name="input-sample-in-nodejs"></a>Eingabebeispiel in Node.js
```javascript
module.exports = function (context, myQueueItem) {
    context.log('Node.js queue trigger function processed work item', myQueueItem);
    context.log('Person entity name: ' + context.bindings.personEntity.Name);
    context.done();
};
```

<a name="output"></a>

## <a name="storage-table-output-binding"></a>Storage-Tabellenausgabebindung
Die Azure Storage-Tabellenausgabebindung ermöglicht Ihnen, in Ihrer Funktion Entitäten in eine Storage-Tabelle zu schreiben. 

Die Storage-Tabellenausgabe für eine Funktion verwendet die folgenden JSON-Objekte im `bindings`-Array von „function.json“:

```json
{
    "name": "<Name of input parameter in function signature>",
    "type": "table",
    "direction": "out",
    "tableName": "<Name of Storage table>",
    "partitionKey": "<PartitionKey of table entity to write - see below>",
    "rowKey": "<RowKey of table entity to write - see below>",
    "connection": "<Name of app setting - see below>",
}
```

Beachten Sie Folgendes: 

* Verwenden Sie `partitionKey` und `rowKey` zusammen, um eine einzelne Entität zu schreiben. Diese Eigenschaften sind optional. Sie können beim Erstellen der Entitätsobjekte in Ihrem Funktionscode auch `PartitionKey` und `RowKey` angeben.
* `connection` muss den Namen einer App-Einstellung enthalten, die eine Speicherverbindungs-Zeichenfolge enthält. Im Azure-Portal konfiguriert der Standard-Editor auf der Registerkarte **Integrieren** diese App-Einstellung für Sie, wenn Sie ein Speicherkonto erstellen oder ein vorhandenes auswählen. Sie können [diese App-Einstellung auch manuell konfigurieren](functions-how-to-use-azure-function-app-settings.md#settings). 

<a name="outputusage"></a>

## <a name="output-usage"></a>Ausgabeverwendung
In C#-Funktionen stellen Sie eine Bindung an die Tabellenausgabe mithilfe des benannten `out`-Parameters wie `out <T> <name>` in Ihrer Funktionssignatur her, wobei `T` der Datentyp ist, in den Sie die Daten serialisieren möchten, und `paramName` der Name, den Sie in der [Ausgabebindung](#output) angegeben haben. In Node.js-Funktionen greifen Sie mit `context.bindings.<name>` auf die Tabellenausgabe zu.

Sie können Objekte in Node.js- oder C#-Funktionen serialisieren. In C#-Funktionen können Sie auch eine Bindung mit einem der folgenden Typen herstellen:

* Jeder Typ, der `ITableEntity` implementiert
* `ICollector<T>` (Um mehrere Entitäten auszugeben. Siehe [Beispiel](#outcsharp).)
* `IAsyncCollector<T>` (asynchrone Version von `ICollector<T>`)
* `CloudTable` (mithilfe des Azure Storage-SDK. Siehe [Beispiel](#readmulti).)

<a name="outputsample"></a>

## <a name="output-sample"></a>Ausgabebeispiel
Im folgenden Beispiel von *function.json* und *run.csx* wird veranschaulicht, wie mehrere Tabellenentitäten geschrieben werden.

```json
{
  "bindings": [
    {
      "name": "input",
      "type": "manualTrigger",
      "direction": "in"
    },
    {
      "tableName": "Person",
      "connection": "MyStorageConnection",
      "name": "tableBinding",
      "type": "table",
      "direction": "out"
    }
  ],
  "disabled": false
}
```

Beachten Sie das sprachspezifische Beispiel für das Erstellen mehrerer Tabellenentitäten.

* [C#](#outcsharp)
* [F#](#outfsharp)
* [Node.js](#outnodejs)

<a name="outcsharp"></a>

### <a name="output-sample-in-c"></a>Ausgabebeispiel in C# #
```csharp
public static void Run(string input, ICollector<Person> tableBinding, TraceWriter log)
{
    for (int i = 1; i < 10; i++)
        {
            log.Info($"Adding Person entity {i}");
            tableBinding.Add(
                new Person() { 
                    PartitionKey = "Test", 
                    RowKey = i.ToString(), 
                    Name = "Name" + i.ToString() }
                );
        }

}

public class Person
{
    public string PartitionKey { get; set; }
    public string RowKey { get; set; }
    public string Name { get; set; }
}

```
<a name="outfsharp"></a>

### <a name="output-sample-in-f"></a>Ausgabebeispiel in F# #
```fsharp
[<CLIMutable>]
type Person = {
  PartitionKey: string
  RowKey: string
  Name: string
}

let Run(input: string, tableBinding: ICollector<Person>, log: TraceWriter) =
    for i = 1 to 10 do
        log.Info(sprintf "Adding Person entity %d" i)
        tableBinding.Add(
            { PartitionKey = "Test"
              RowKey = i.ToString()
              Name = "Name" + i.ToString() })
```

<a name="outnodejs"></a>

### <a name="output-sample-in-nodejs"></a>Ausgabebeispiel in Node.js
```javascript
module.exports = function (context) {

    context.bindings.tableBinding = [];

    for (var i = 1; i < 10; i++) {
        context.bindings.tableBinding.push({
            PartitionKey: "Test",
            RowKey: i.toString(),
            Name: "Name " + i
        });
    }
    
    context.done();
};
```

<a name="readmulti"></a>

## <a name="sample-read-multiple-table-entities-in-c"></a>Beispiel: Lesen mehrerer Tabellenentitäten in C#  #
Das folgende Beispiel für *function.json* und C#-Code liest Entitäten für einen Partitionsschlüssel, der in der Warteschlangennachricht angegeben ist.

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
      "name": "tableBinding",
      "type": "table",
      "connection": "MyStorageConnection",
      "tableName": "Person",
      "direction": "in"
    }
  ],
  "disabled": false
}
```

Der C#-Code fügt einen Verweis auf das Azure Storage SDK hinzu, sodass der Entitätstyp von `TableEntity`abgeleitet werden kann.

```csharp
#r "Microsoft.WindowsAzure.Storage"
using Microsoft.WindowsAzure.Storage.Table;

public static void Run(string myQueueItem, IQueryable<Person> tableBinding, TraceWriter log)
{
    log.Info($"C# Queue trigger function processed: {myQueueItem}");
    foreach (Person person in tableBinding.Where(p => p.PartitionKey == myQueueItem).ToList())
    {
        log.Info($"Name: {person.Name}");
    }
}

public class Person : TableEntity
{
    public string Name { get; set; }
}
```

## <a name="next-steps"></a>Nächste Schritte
[!INCLUDE [next steps](../../includes/functions-bindings-next-steps.md)]


