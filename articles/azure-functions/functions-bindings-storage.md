<properties
	pageTitle="Trigger und Bindungen für Azure Storage in Azure Functions | Microsoft Azure"
	description="Erfahren Sie, wie Azure Storage-Trigger und -Bindungen in Azure Functions verwendet werden."
	services="functions"
	documentationCenter="na"
	authors="christopheranderson"
	manager="erikre"
	editor=""
	tags=""
	keywords="Azure Functions, Funktionen, Ereignisverarbeitung, dynamisches Compute, serverlose Architektur"/>

<tags
	ms.service="functions"
	ms.devlang="multiple"
	ms.topic="reference"
	ms.tgt_pltfrm="multiple"
	ms.workload="na"
	ms.date="08/22/2016"
	ms.author="chrande"/>

# Trigger und Bindungen für Azure Storage in Azure Functions

[AZURE.INCLUDE [functions-selector-bindings](../../includes/functions-selector-bindings.md)]

Dieser Artikel erläutert das Konfigurieren und Codieren von Azure Storage-Triggern und -Bindungen in Azure Functions.

[AZURE.INCLUDE [Einführung](../../includes/functions-bindings-intro.md)]

## <a id="storagequeuetrigger"></a> Azure Storage-Warteschlangentrigger

#### „function.json“ für einen Storage-Warteschlangentrigger

Die Datei *function.json* gibt die folgenden Eigenschaften an.

- `name`: Variablenname, der im Funktionscode für die Warteschlange oder Warteschlangennachricht verwendet wird.
- `queueName`: Name der abzufragenden Warteschlange. Benennungsregeln für Warteschlangen finden Sie unter [Benennen von Warteschlangen und Metadaten](https://msdn.microsoft.com/library/dd179349.aspx).
- `connection`: Name einer App-Einstellung, die eine Storage-Verbindungszeichenfolge enthält. Wenn Sie `connection` leer lassen, verwendet der Trigger für die Funktionen-App die standardmäßige Storage-Verbindungszeichenfolge, die von der App-Einstellung „AzureWebJobsStorage“ angegeben wird.
- `type`: Muss auf *queueTrigger* festgelegt werden.
- `direction`: Muss auf *in* festgelegt werden.

Beispiel von *function.json* für einen Storage-Warteschlangentrigger:

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

#### Vom Warteschlangentrigger unterstützte Typen

Die Warteschlangennachricht kann in alle folgenden Typen deserialisiert werden:

* Objekt (aus JSON)
* String
* Bytearray
* `CloudQueueMessage` (C#)

#### Metadaten für Warteschlangentrigger

Sie können Warteschlangen-Metadaten in Ihrer Funktion mithilfe dieser Variablennamen abrufen:

* expirationTime
* insertionTime
* nextVisibleTime
* id
* popReceipt
* dequeueCount
* queueTrigger (eine weitere Möglichkeit, den Text der Warteschlangennachricht als Zeichenfolge abzurufen)

In diesem C#-Codebeispiel werden Warteschlangen-Metadaten abgerufen und protokolliert:

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

#### Behandeln von Nachrichten in der Warteschlange für nicht verarbeitbare Nachrichten

Nachrichten, deren Inhalt bewirkt, dass bei einer Funktion ein Fehler auftritt, werden als *nicht verarbeitbare Nachrichten* bezeichnet. Wenn bei der Funktion ein Fehler auftritt, wird die Warteschlangennachricht nicht gelöscht, sondern schließlich erneut aufgegriffen, wodurch verursacht wird, dass der Zyklus wiederholt wird. Das SDK kann den Zyklus nach einer begrenzten Anzahl von Iterationen automatisch unterbrechen, oder Sie können dies manuell vornehmen.

Das SDK ruft eine Funktion bis zu fünfmal auf, um eine Warteschlangennachricht zu verarbeiten. Wenn der fünfte Versuch fehlschlägt, wird die Nachricht in eine Warteschlange für nicht verarbeitete Nachrichten verschoben.

Die Warteschlange für nicht verarbeitete Nachrichten weist den Namen "*{UrsprünglicherWarteschlangenname}*-poison" auf. Sie können eine Funktion schreiben, um Nachrichten aus der Warteschlange für nicht verarbeitete Nachrichten zu verarbeiten, indem Sie diese protokollieren oder eine Benachrichtigung senden, dass ein manueller Eingriff erforderlich ist.

Wenn Sie nicht verarbeitbare Nachrichten manuell verarbeiten möchten, können Sie durch Überprüfen von `dequeueCount` abrufen, wie oft eine Nachricht zur Verarbeitung entnommen wurde.

## <a id="storagequeueoutput"></a> Azure Storage-Warteschlangen-Ausgabebindung

#### Beispiel von „function.json“ für eine Storage-Warteschlangen-Ausgabebindung

Die Datei *function.json* gibt die folgenden Eigenschaften an.

- `name`: Variablenname, der im Funktionscode für die Warteschlange oder Warteschlangennachricht verwendet wird.
- `queueName`: Name der Warteschlange. Benennungsregeln für Warteschlangen finden Sie unter [Benennen von Warteschlangen und Metadaten](https://msdn.microsoft.com/library/dd179349.aspx).
- `connection`: Name einer App-Einstellung, die eine Storage-Verbindungszeichenfolge enthält. Wenn Sie `connection` leer lassen, verwendet der Trigger für die Funktionen-App die standardmäßige Storage-Verbindungszeichenfolge, die von der App-Einstellung „AzureWebJobsStorage“ angegeben wird.
- `type`: Muss auf *queue* festgelegt werden.
- `direction`: Muss auf *out* festgelegt werden.

Beispiel von *function.json* für eine Storage-Warteschlangen-Ausgabebindung, die einen Warteschlangentrigger verwendet und eine Warteschlangennachricht schreibt:

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

#### Von der Bindung der Warteschlangenausgabe unterstützte Typen

Die `queue`-Bindung kann die folgenden Typen in eine Warteschlangennachricht serialisieren:

* Objekt (`out T` in C#, erstellt eine Nachricht mit einem NULL-Objekt, wenn der Parameter bei Funktionsbeendigung NULL ist)
* Zeichenfolge (`out string` in C#, erstellt eine Warteschlangennachricht, wenn der Parameterwert nicht NULL ist, sobald die Funktion beendet wird)
* Bytearray (`out byte[]` in C#, funktioniert wie Zeichenfolge)
* `out CloudQueueMessage` (C#, funktioniert wie Zeichenfolge)

In C# können Sie auch eine Bindung mit `ICollector<T>` oder `IAsyncCollector<T>` erstellen, wobei `T` einer der unterstützten Typen ist.

#### Codebeispiel für die Bindung der Warteschlangenausgabe

Dieses C#-Codebeispiel schreibt für jede Nachricht der Eingabewarteschlange eine einzige Nachricht der Ausgabewarteschlange.

```csharp
public static void Run(string myQueueItem, out string myOutputQueueItem, TraceWriter log)
{
    myOutputQueueItem = myQueueItem + "(next step)";
}
```

Dieses C#-Codebeispiel schreibt mehrere Nachrichten mit `ICollector<T>` (verwenden Sie `IAsyncCollector<T>` in einer async-Funktion):

```csharp
public static void Run(string myQueueItem, ICollector<string> myQueue, TraceWriter log)
{
    myQueue.Add(myQueueItem + "(step 1)");
    myQueue.Add(myQueueItem + "(step 2)");
}
```

## <a id="storageblobtrigger"></a> Azure Storage-Blobtrigger

#### „function.json“ für einen Storage-Blobtrigger

Die Datei *function.json* gibt die folgenden Eigenschaften an.

- `name`: Variablenname, der im Funktionscode für das Blob verwendet wird.
- `path`: Pfad, der den zu überwachenden Container und bei Bedarf ein Blobnamensmuster angibt.
- `connection`: Name einer App-Einstellung, die eine Storage-Verbindungszeichenfolge enthält. Wenn Sie `connection` leer lassen, verwendet der Trigger für die Funktionen-App die standardmäßige Storage-Verbindungszeichenfolge, die von der App-Einstellung „AzureWebJobsStorage“ angegeben wird.
- `type`: Muss auf *blobTrigger* festgelegt werden.
- `direction`: Muss auf *in* festgelegt werden.

Beispiel von *function.json* für einen Storage-Blobtrigger, der Blobs überwacht, die dem Container „samples-workitems“ hinzugefügt werden:

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

#### Vom Blobtrigger unterstützte Typen

Das Blob kann in einen der folgenden Typen in Node- oder C#-Funktionen deserialisiert werden:

* Objekt (aus JSON)
* String

In C#-Funktionen können Sie auch eine Bindung mit einem der folgenden Typen herstellen:

* `TextReader`
* `Stream`
* `ICloudBlob`
* `CloudBlockBlob`
* `CloudPageBlob`
* `CloudBlobContainer`
* `CloudBlobDirectory`
* `IEnumerable<CloudBlockBlob>`
* `IEnumerable<CloudPageBlob>`
* Andere von [ICloudBlobStreamBinder](../app-service-web/websites-dotnet-webjobs-sdk-storage-blobs-how-to.md#icbsb) deserialisierte Typen

#### C#-Codebeispiel für Blobtrigger

Dieses C#-Codebeispiel protokolliert den Inhalt der einzelnen Blobs, die dem überwachten Container hinzugefügt werden.

```csharp
public static void Run(string myBlob, TraceWriter log)
{
    log.Info($"C# Blob trigger function processed: {myBlob}");
}
```

#### Namensmuster für Blobtrigger

Sie können in der `path`-Eigenschaft ein Blobnamensmuster angeben. Beispiel:

```json
"path": "input/original-{name}",
```

Dieser Pfad findet ein Blob mit dem Namen *original-Blob1.txt* im Container *input*, und der Wert der Variablen `name` im Funktionscode lautet `Blob1`.

Ein weiteres Beispiel:

```json
"path": "input/{blobname}.{blobextension}",
```

Mit diesem Pfad erhalten Sie ein Blob mit dem Namen *original-Blob1.txt*, und die Werte der Variablen `blobname` und `blobextension` im Funktionscode lauten *original-Blob1* und *txt*.

Sie können die Typen von Blobs einschränken, die die Funktion auslösen, indem Sie ein Muster mit einem festen Wert für die Dateierweiterung angeben. Wenn Sie den `path` auf *samples/{Name}.png* festlegen, wird die Funktion nur durch *PNG*-Blobs im Container *samples* ausgelöst.

Wenn Sie ein Namensmuster für Blobnamen angeben müssen, die geschweifte Klammern enthalten, verdoppeln Sie die geschweiften Klammern. Wenn Sie beispielsweise Blob im Container *images* suchen, deren Namen wie folgt lauten:

		{20140101}-soundfile.mp3

Geben Sie für die `path`-Eigenschaft Folgendes an:

		images/{{20140101}}-{name}

In diesem Beispiel lautet der Wert der Variablen `name` *soundfile.mp3*.

#### BLOB-Zugänge

Die Azure Functions-Laufzeit stellt sicher, dass Blobtriggerfunktionen für ein neues oder aktualisiertes Blob nicht mehrmals aufgerufen werden. Zu diesem Zweck wird mittels Verwaltung der *Blobbelege* bestimmt, ob eine bestimmte Blobversion verarbeitet wurde.

Blobbelege werden in einem Container mit dem Namen *azure-webjobs-hosts* in dem Azure-Speicherkonto gespeichert, das in der Verbindungszeichenfolge "AzureWebJobsStorage" angegeben ist. Ein Blobbeleg enthält die folgenden Informationen:

* Die Funktion, die für das Blob aufgerufen wurde („ *{Funktionen-App-Name}* .Functions. *{Funktionsname}* “, Beispiel: „functionsf74b96f7.Functions.CopyBlob“)
* Der Containername
* Blobtyp ("BlockBlob" oder "PageBlob")
* Blobname
* ETag (eine Blobversions-ID. Beispiel: 0x8D1DC6E70A277EF)

Wenn Sie eine erneute Verarbeitung eines Blobs erzwingen möchten, können Sie den Blobbeleg für dieses Blob manuell aus dem Container *azure-webjobs-hosts* löschen.

#### Verarbeiten von nicht verarbeitbaren Blobs

Wenn bei einer Blobtriggerfunktion ein Fehler auftritt, wird sie für den Fall, dass es sich um einen vorübergehenden Fehler handelt, erneut vom SDK aufgerufen. Wenn der Inhalt des Blobs den Fehler verursacht hat, misslingt die Funktion bei jedem Versuch, das Blob zu verarbeiten. Standardmäßig ruft das SDK eine Funktion bis zu 5 Mal für ein angegebenes Blob auf. Ist auch der fünfte Versuch nicht erfolgreich, fügt das SDK einer Warteschlange namens *webjobs-blobtrigger-poison* eine Nachricht hinzu.

Die Warteschlangennachricht für nicht verarbeitbare Blobs ist ein JSON-Objekt, das die folgenden Eigenschaften enthält:

* FunctionId (im Format *{Funktionen-App-Name}*.Functions.*{Funktionsname}*)
* BlobType ("BlockBlob" oder "PageBlob")
* ContainerName
* BlobName
* ETag (eine Blobversions-ID. Beispiel: 0x8D1DC6E70A277EF)

#### Abfragen von Blobs für große Container

Wenn der Blobcontainer, der vom Trigger überwacht wird, mehr als 10.000 Blobs enthält, überprüft die Functions-Laufzeit Protokolldateien auf neue oder geänderte Blobs. Dieser Vorgang verläuft nicht in Echtzeit. Eine Funktion wird unter Umständen erst mehrere Minuten nach der Bloberstellung oder noch später ausgelöst. Darüber hinaus erfolgt das [Erstellen von Storage-Protokollen auf bestmögliche Weise](https://msdn.microsoft.com/library/azure/hh343262.aspx); es gibt jedoch keine Garantie, dass alle Ereignisse erfasst werden. Unter bestimmten Umständen können Protokolle fehlen. Wenn die eingeschränkte Geschwindigkeit und Zuverlässigkeit von Blobtriggern für große Container für Ihre Anwendung nicht akzeptabel sind, empfiehlt es sich, zusammen mit dem Blob eine Warteschlangennachricht zu erstellen und für die Verarbeitung des Blobs anstelle des Blobtriggers einen Warteschlangentrigger zu verwenden.
 
## <a id="storageblobbindings"></a> Azure Storage-Blobeingabe- und -ausgabebindungen

#### Beispiel von „function.json“ für eine Storage-Blobeingabe- oder -ausgabebindung

Die Datei *function.json* gibt die folgenden Eigenschaften an.

- `name`: Variablenname, der im Funktionscode für das Blob verwendet wird.
- `path`: Pfad, der den Container angibt, aus dem das Blob gelesen bzw. in den das Blob geschrieben wird, und ggf. ein Blobnamensmuster angibt.
- `connection`: Name einer App-Einstellung, die eine Storage-Verbindungszeichenfolge enthält. Wenn Sie `connection` leer lassen, verwendet die Bindung für die Funktionen-App die standardmäßige Storage-Verbindungszeichenfolge, die von der App-Einstellung „AzureWebJobsStorage“ angegeben wird.
- `type`: Muss auf *blob* festgelegt werden.
- `direction`: Muss auf *in* oder *out* festgelegt werden.

Beispiel von *function.json* für eine Storage-Blobeingabe- oder -ausgabebindung mit einem Warteschlangentrigger zum Kopieren eines Blobs:

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

#### Für Blobeingabe- und Ausgabeparameter unterstützte Typen

Die `blob`-Bindung kann in Node.js- oder C#-Funktionen die folgenden Typen serialisieren oder deserialisieren:

* Objekt (`out T` in C# für Ausgabeblob: erstellt ein Blob als NULL-Objekt, wenn der Parameterwert bei Funktionsbeendigung NULL ist)
* Zeichenfolge (`out string` in C# für Ausgabeblob: erstellt ein Blob nur dann, wenn der Zeichenfolgenparameter bei Rückgabe der Funktion ungleich NULL ist)

In C#-Funktionen können Sie auch eine Bindung mit einem der folgenden Typen herstellen:

* `TextReader` (nur Eingabe)
* `TextWriter` (nur Ausgabe)
* `Stream`
* `CloudBlobStream` (nur Ausgabe)
* `ICloudBlob`
* `CloudBlockBlob`
* `CloudPageBlob`

#### C#-Codebeispiel für Blobausgabe

Dieses C#-Codebeispiel kopiert ein Blob, dessen Name in einer Warteschlangennachricht empfangen wird.

```csharp
public static void Run(string myQueueItem, string myInputBlob, out string myOutputBlob, TraceWriter log)
{
    log.Info($"C# Queue trigger function processed: {myQueueItem}");
    myOutputBlob = myInputBlob;
}
```

## <a id="storagetablesbindings"></a> Azure Storage-Tabelleneingabe- und -ausgabebindungen

#### „function.json“ für Storage-Tabellen

Die Datei *function.json* gibt die folgenden Eigenschaften an.

- `name`: Variablenname, der im Funktionscode für die Tabellenbindung verwendet wird.
- `tableName`: Name der Tabelle.
- `partitionKey` und `rowKey`: Werden zusammen verwendet, um eine einzelne Entität in einer C#- oder Node.js-Funktion zu lesen oder um eine einzelne Entität in eine Node.js-Funktion zu schreiben.
- `take`: Maximale Anzahl von Zeilen, die für die Tabelleneingabe in einer Node.js-Funktion gelesen werden sollen.
- `filter`: OData-Filterausdruck für die Tabelleneingabe in einer Node.js-Funktion.
- `connection`: Name einer App-Einstellung, die eine Storage-Verbindungszeichenfolge enthält. Wenn Sie `connection` leer lassen, verwendet die Bindung für die Funktionen-App die standardmäßige Storage-Verbindungszeichenfolge, die von der App-Einstellung „AzureWebJobsStorage“ angegeben wird.
- `type`: Muss auf *table* festgelegt werden.
- `direction`: Muss auf *in* oder *out* festgelegt werden.

Das folgende Beispiel von *function.json* verwendet einen Warteschlangentrigger zum Lesen einer einzelnen Tabellenzeile. JSON stellt einen hartcodierten Partitionsschlüsselwert bereit und gibt an, dass der Zeilenschlüssel aus der Warteschlangennachricht stammt.

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

#### Für Speichertabelleneingabe und -ausgabe unterstützte Typen

Die `table`-Bindung kann Objekte in Node.js- oder C#-Funktionen serialisieren oder deserialisieren. Die Objekte haben „RowKey“- und „PartitionKey“-Eigenschaften.

In C#-Funktionen können Sie auch eine Bindung mit einem der folgenden Typen herstellen:

* `T`, wobei `ITableEntity` von `T` implementiert wird
* `IQueryable<T>` (nur Eingabe)
* `ICollector<T>` (nur Ausgabe)
* `IAsyncCollector<T>` (nur Ausgabe)

#### Szenarien für die Speichertabellenbindung

Die Tabellenbindung unterstützt die folgenden Szenarien:

* Lesen einer einzelnen Zeile in einer C#- oder Node-Funktion.

	Legen Sie `partitionKey` und `rowKey` fest. Die Eigenschaften `filter` und `take` werden in diesem Szenario nicht verwendet.

* Lesen mehrerer Zeilen in einer C#-Funktion.

	Die Functions-Runtime gibt ein an die Tabelle gebundenes `IQueryable<T>`-Objekt an. Der Typ `T` muss von `TableEntity` abgeleitet sein oder `ITableEntity` implementieren. Die Eigenschaften `partitionKey`, `rowKey`, `filter` und `take` werden in diesem Szenario nicht verwendet. Erforderliche Filtervorgänge können über das `IQueryable`-Objekt erfolgen.

* Lesen mehrerer Zeilen in einer Node-Funktion.

	Legen Sie die Eigenschaften `filter` und `take` fest. Legen Sie `partitionKey` oder `rowKey` nicht fest.

* Schreiben einer oder mehrerer Zeilen in eine C#-Funktion.

	Die Functions-Runtime stellt einen an die Tabelle gebundenen `ICollector<T>` oder `IAsyncCollector<T>` bereit, wobei `T` das Schema der Entitäten angibt, die Sie hinzufügen möchten. In der Regel ist der Typ `T` von `TableEntity` abgeleitet oder implementiert `ITableEntity`, dies ist aber nicht zwingend. Die Eigenschaften `partitionKey`, `rowKey`, `filter` und `take` werden in diesem Szenario nicht verwendet.

#### Beispiel für Speichertabellen: Lesen einer einzelnen Tabellenentität in C# oder Node

Das folgende C#-Codebeispiel kann mit der vorhergehenden Datei *function.json* verwendet werden, um eine einzelne Tabellenentität zu lesen. Die Warteschlangennachricht enthält den Zeilenschlüsselwert, und die Tabellenentität wird in einen Typ gelesen, der in der Datei *run.csx* definiert ist. Der Typ umfasst `PartitionKey`- und `RowKey`-Eigenschaften und ist nicht von `TableEntity` abgeleitet.

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

Das folgende F#-Codebeispiel kann ebenfalls mit der vorhergehenden Datei *function.json* verwendet werden, um eine einzelne Tabellenentität zu lesen.

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

Das folgende Node.js-Codebeispiel kann ebenfalls mit der vorhergehenden Datei *function.json* verwendet werden, um eine einzelne Tabellenentität zu lesen.

```javascript
module.exports = function (context, myQueueItem) {
    context.log('Node.js queue trigger function processed work item', myQueueItem);
    context.log('Person entity name: ' + context.bindings.personEntity.Name);
    context.done();
};
```

#### Beispiel für Speichertabellen: Lesen mehrerer Tabellenentitäten in C# 

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

Der C#-Code fügt einen Verweis auf das Azure Storage SDK hinzu, sodass der Entitätstyp von `TableEntity` abgeleitet werden kann.

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

#### Beispiel für Speichertabellen: Erstellen mehrerer Tabellenentitäten in C# 

Im folgenden Beispiel von *function.json* und *run.csx* wird veranschaulicht, wie Tabellenentitäten in C# geschrieben werden.

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

#### Beispiel für Speichertabellen: Erstellen mehrerer Tabellenentitäten in F#

Im folgenden Beispiel von *function.json* und *run.fsx* wird veranschaulicht, wie Tabellenentitäten in F# geschrieben werden.

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

#### Beispiel für Speichertabellen: Erstellen einer Tabellenentität in Node

Im folgenden Beispiel von *function.json* und *run.csx* wird veranschaulicht, wie eine Tabellenentität in Node.js geschrieben wird.

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
      "tableName": "Person",
      "partitionKey": "Test",
      "rowKey": "{queueTrigger}",
      "connection": "MyStorageConnection",
      "name": "personEntity",
      "type": "table",
      "direction": "out"
    }
  ],
  "disabled": true
}
```

```javascript
module.exports = function (context, myQueueItem) {
    context.log('Node.js queue trigger function processed work item', myQueueItem);
    context.bindings.personEntity = {"Name": "Name" + myQueueItem }
    context.done();
};
```

## Nächste Schritte

[AZURE.INCLUDE [Nächste Schritte](../../includes/functions-bindings-next-steps.md)]

<!---HONumber=AcomDC_0921_2016-->