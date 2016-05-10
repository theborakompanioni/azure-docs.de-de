<properties
	pageTitle="Trigger und Bindungen in Azure Functions | Microsoft Azure"
	description="Hier wird erläutert, wie Trigger und Bindungen in Azure Functions verwendet werden."
	services="functions"
	documentationCenter="na"
	authors="christopheranderson"
	manager="erikre"
	editor=""
	tags=""
	keywords="Azure Functions, Functions, Ereignisverarbeitung, Webhooks, dynamisches Compute, serverlose Architektur"/>

<tags
	ms.service="functions"
	ms.devlang="multiple"
	ms.topic="reference"
	ms.tgt_pltfrm="multiple"
	ms.workload="na"
	ms.date="04/07/2016"
	ms.author="chrande"/>

# Entwicklerreferenz zu Triggern und Bindungen in Azure Functions

Dieser Artikel erläutert das Konfigurieren und Codieren von Triggern und Bindungen in Azure Functions. Die meisten dieser Bindungen werden einfach über die **Integrieren**-Benutzeroberfläche des Azure-Portals verwaltet, aber im Portal werden nicht alle Funktionen und Optionen für die einzelnen Bindungen erklärt.

In diesem Artikel wird davon ausgegangen, dass Sie bereits die [Entwicklerreferenz zu Azure Functions](functions-reference.md) sowie die [C#](functions-reference-csharp.md)- oder [Node](functions-reference-node.md)-Referenzartikel für Entwickler gelesen haben.

## HTTP- und WebHook-Trigger und -Bindungen

Mit einem HTTP- oder WebHook-Trigger können Sie eine Funktion als Antwort auf eine HTTP-Anforderung aufrufen. Die Anforderung muss einen API-Schlüssel enthalten, der derzeit nur auf der Benutzeroberfläche des Azure-Portals verfügbar ist.

Die Funktions-URL ist eine Kombination aus der URL der Funktions-App und dem Funktionsnamen:

```
 https://{function app name}.azurewebsites.net/api/{function name} 
```

Die Datei *function.json* enthält Eigenschaften, die die HTTP-Anforderung und die Antwort betreffen.

Eigenschaften für die HTTP-Anforderung:

- `name`: Der Variablenname, der im Funktionscode für das Anforderungsobjekt (oder bei Node.js-Funktionen im Hauptteil der Anforderung) verwendet wird.
- `type`: Muss auf *httpTrigger* festgelegt werden.
- `direction`: Muss auf *in* festgelegt werden. 
- `webHookType`: Für WebHook-Trigger sind gültige Werte *github*, *slack* und *genericJson*. Für einen HTTP-Trigger, der kein WebHook ist, legen Sie diese Eigenschaft auf eine leere Zeichenfolge fest. Weitere Informationen zu WebHooks finden Sie im folgenden Abschnitt zu [WebHook-Triggern](#webhook-triggers).
- `authLevel`: Gilt nicht für WebHook-Trigger. Legen Sie den Wert auf „function“ fest, um einen API-Schlüssel zu verlangen, auf „anonymous“, um die Anforderung eines API-Schlüssels aufzuheben, oder auf „admin“, um den Master-API-Schlüssel zu verlangen. Weitere Informationen finden Sie unten unter [API-Schlüssel](#apikeys).

Eigenschaften für die HTTP-Antwort:

- `name`: Variablenname, der im Funktionscode für das Antwortobjekt verwendet wird.
- `type`: Muss auf *http* festgelegt werden.
- `direction`: Muss auf *out* festgelegt werden. 
 
Beispiel für *function.json*:

```json
{
  "bindings": [
    {
      "webHookType": "",
      "name": "req",
      "type": "httpTrigger",
      "direction": "in"
      "authLevel": "function"
    },
    {
      "name": "res",
      "type": "http",
      "direction": "out"
    }
  ],
  "disabled": false
}
```

### WebHook-Trigger

Ein WebHook-Trigger ist ein HTTP-Trigger, der die folgenden für WebHooks entwickelten Features umfasst:

* Für bestimmte WebHook-Anbieter (zurzeit werden GitHub und Slack unterstützt) wird die Signatur des Anbieters durch die Functions-Laufzeit überprüft.
* Für Node.js-Funktionen stellt die Functions-Laufzeit den Hauptteil der Anforderung anstelle des Anforderungsobjekts bereit. Für C#-Funktionen gibt es keine spezielle Verarbeitung, weil Sie durch die Angabe des Parametertyps steuern, was bereitgestellt wird. Bei Angabe von `HttpRequestMessage` erhalten Sie das Anforderungsobjekt. Wenn Sie einen POCO-Typ angeben, versucht die Functions-Laufzeit, ein JSON-Objekt im Hauptteil der Anforderung zu analysieren, um die Objekteigenschaften aufzufüllen.
* Zum Auslösen einer WebHook-Funktion muss die HTTP-Anforderung einen API-Schlüssel enthalten. Für andere HTTP-Trigger als WebHook ist diese Anforderung optional.

Informationen zum Einrichten eines GitHub-WebHooks finden Sie unter [GitHub Developer – Creating WebHooks](http://go.microsoft.com/fwlink/?LinkID=761099&clcid=0x409) (Erstellen von WebHooks).

### API-Schlüssel

Standardmäßig muss eine HTTP-Anforderung einen API-Schlüssel aufweisen, um eine HTTP- oder WebHook-Funktion auszulösen. Der Schlüssel kann in einer Abfragezeichenfolgen-Variablen namens `code` oder in einem `x-functions-key`-HTTP-Header enthalten sein. Für andere Funktionen als WebHook können Sie angeben, dass kein API-Schlüssel erforderlich ist, indem Sie in der Datei *function.json* die `authLevel`-Eigenschaft auf „anonymous“ festlegen.

Sie finden API-Schlüsselwerte im Ordner *D:\\home\\data\\Functions\\secrets* im Dateisystem der Funktions-App. Der Hauptschlüssel und der Funktionsschlüssel werden in der Datei *host.json* festgelegt, wie im folgenden Beispiel gezeigt.

```json
{
  "masterKey": "K6P2VxK6P2VxK6P2VxmuefWzd4ljqeOOZWpgDdHW269P2hb7OSJbDg==",
  "functionKey": "OBmXvc2K6P2VxK6P2VxK6P2VxVvCdB89gChyHbzwTS/YYGWWndAbmA=="
}
```

Der Funktionsschlüssel aus *host.json* kann zum Auslösen einer beliebigen Funktion außer einer deaktivierten Funktion verwendet werden. Der Hauptschlüssel kann zum Auslösen einer beliebigen Funktion eingesetzt werden, selbst wenn diese deaktiviert ist. Sie können eine Funktion so konfigurieren, dass der Hauptschlüssel erforderlich ist, indem Sie die `authLevel`-Eigenschaft auf „admin“ festlegen.

Wenn der Ordner *secrets* eine JSON-Datei mit dem gleichen Namen wie eine Funktion enthält, kann die `key`-Eigenschaft in dieser Datei auch zum Auslösen der Funktion verwendet werden. Dieser Schlüssel funktioniert nur mit der Funktion, auf die er verweist. Beispielsweise ist der API-Schlüssel für eine Funktion namens `HttpTrigger` in der Datei *HttpTrigger.json* im Ordner *secrets* angegeben. Beispiel:

```json
{
  "key":"0t04nmo37hmoir2rwk16skyb9xsug32pdo75oce9r4kg9zfrn93wn4cx0sxo4af0kdcz69a4i"
}
```

> [AZURE.NOTE] Geben Sie beim Einrichten eines WebHook-Triggers nicht den Hauptschlüssel an den WebHook-Anbieter weiter. Verwenden Sie einen Schlüssel, der nur mit der Funktion funktioniert, die den WebHook verarbeitet. Der Hauptschlüssel kann zum Auslösen einer beliebigen Funktion eingesetzt werden, selbst wenn diese deaktiviert ist.

### C#-Beispielcode für eine HTTP-Triggerfunktion 

Der Beispielcode sucht in der Abfragezeichenfolge oder im Text der HTTP-Anforderung nach einem `name`-Parameter.

```csharp
using System.Net;
using System.Threading.Tasks;

public static async Task<HttpResponseMessage> Run(HttpRequestMessage req, TraceWriter log)
{
    log.Verbose($"C# HTTP trigger function processed a request. RequestUri={req.RequestUri}");

    // parse query parameter
    string name = req.GetQueryNameValuePairs()
        .FirstOrDefault(q => string.Compare(q.Key, "name", true) == 0)
        .Value;

    // Get request body
    dynamic data = await req.Content.ReadAsAsync<object>();

    // Set name to query string or body data
    name = name ?? data?.name;

    return name == null
        ? req.CreateResponse(HttpStatusCode.BadRequest, "Please pass a name on the query string or in the request body")
        : req.CreateResponse(HttpStatusCode.OK, "Hello " + name);
}
```

### Node.js-Beispielcode für eine HTTP-Triggerfunktion 

Dieser Beispielcode sucht in der Abfragezeichenfolge oder im Text der HTTP-Anforderung nach einem `name`-Parameter.

```javascript
module.exports = function(context, req) {
    context.log('Node.js HTTP trigger function processed a request. RequestUri=%s', req.originalUrl);

    if (req.query.name || (req.body && req.body.name)) {
        context.res = {
            // status: 200, /* Defaults to 200 */
            body: "Hello " + (req.query.name || req.body.name)
        };
    }
    else {
        context.res = {
            status: 400,
            body: "Please pass a name on the query string or in the request body"
        };
    }
    context.done();
};
```

### C#-Beispielcode für eine GitHub-WebHook-Funktion 

Dieser Beispielcode protokolliert GitHub-Kommentare.

```csharp
#r "Newtonsoft.Json"

using System;
using System.Net;
using System.Threading.Tasks;
using Newtonsoft.Json;

public static async Task<object> Run(HttpRequestMessage req, TraceWriter log)
{
    string jsonContent = await req.Content.ReadAsStringAsync();
    dynamic data = JsonConvert.DeserializeObject(jsonContent);

    log.Verbose($"WebHook was triggered! Comment: {data.comment.body}");

    return req.CreateResponse(HttpStatusCode.OK, new {
        body = $"New GitHub comment: {data.comment.body}"
    });
}
```

### Node.js-Beispielcode für eine GitHub-WebHook-Funktion 

Dieser Beispielcode protokolliert GitHub-Kommentare.

```javascript
module.exports = function (context, data) {
    context.log('GitHub WebHook triggered!', data.comment.body);
    context.res = { body: 'New GitHub comment: ' + data.comment.body };
    context.done();
};
```

## Trigger mit Timer

Die Datei *function.json* enthält einen Zeitplanausdruck und einen Schalter, der angibt, ob die Funktion sofort ausgelöst werden soll.

```json
{
  "bindings": [
    {
      "schedule": "0 * * * * *",
      "runOnStartup": true,
      "name": "myTimer",
      "type": "timerTrigger",
      "direction": "in"
    }
  ],
  "disabled": false
}
```

Der Trigger mit Timer übernimmt automatisch das horizontale Hochskalieren über mehrere Instanzen: Nur eine einzelne Instanz einer bestimmten Timerfunktion wird für alle Instanzen ausgeführt.

### Format des Zeitplanausdrucks

Der Zeitplanausdruck kann ein [CRON-Ausdruck](http://en.wikipedia.org/wiki/Cron#CRON_expression) mit sechs Feldern sein: {Sekunde} {Minute} {Stunde} {Tag} {Monat} {Wochentag}. In vielen Dokumenten mit CRON-Ausdrücken, die online zu finden sind, wird das Feld {Sekunde} ausgelassen. Wenn Sie daher aus einem dieser Dokumente kopieren, müssen Sie das zusätzliche Feld berücksichtigen.

Der Zeitplanausdruck kann auch im Format *hh:mm:ss* vorliegen, um die Verzögerung zwischen den Zeitpunkten der Funktionsauslösung anzugeben.

Hier sind einige Beispiele für Zeitplanausdrücke aufgeführt.

Alle 5 Minuten auslösen:

```json
"schedule": "0 */5 * * * *",
"runOnStartup": false,
```

Sofort und anschließend alle zwei Stunden auslösen:

```json
"schedule": "0 0 */2 * * *",
"runOnStartup": true,
```

Alle 15 Sekunden auslösen:

```json
"schedule": "00:00:15",
"runOnStartup": false,
```

### C#-Codebeispiel für Trigger mit Timer

Dieses C#-Codebeispiel schreibt bei jedem Auslösen der Funktion ein Protokoll.

```csharp
public static void Run(TimerInfo myTimer, TraceWriter log)
{
    log.Verbose($"C# Timer trigger function executed at: {DateTime.Now}");    
}
```

## Azure Storage-Trigger und -Bindungen (Warteschlangen, Blobs, Tabellen)

Dieser Abschnitt enthält die folgenden Unterabschnitte:

* [Azure Storage-Verbindungseigenschaft in „function.json“](#storageconnection)
* [Azure Storage-Warteschlangentrigger](#storagequeuetrigger)
* [Bindung der Azure Storage-Warteschlangenausgabe](#storagequeueoutput)
* [Azure Storage-Blobtrigger](#storageblobtrigger)
* [Azure Storage-Blobeingabe- und -ausgabebindungen](#storageblobbindings)
* [Azure Storage-Tabelleneingabe- und -ausgabebindungen](#storagetablesbindings)

### <a id="storageconnection"></a> Azure Storage-Verbindungseigenschaft in „function.json“

Für alle Azure Storage-Trigger und -Bindungen enthält die Datei *function.json* eine `connection`-Eigenschaft. Beispiel:

```json
{
    "disabled": false,
    "bindings": [
        {
            "name": "myQueueItem",
            "type": "queueTrigger",
            "direction": "in",
            "queueName": "myqueue-items",
            "connection":""
        }
    ]
}
```

Wenn Sie `connection` leer lassen, funktioniert der Trigger bzw. die Bindung mit dem Standardspeicherkonto für die Funktions-App. Wenn der Trigger bzw. die Bindung mit einem anderen Speicherkonto funktionieren soll, erstellen Sie eine App-Einstellung in der Funktions-App, die auf das gewünschte Speicherkonto verweist, und legen Sie `connection` auf den Namen der App-Einstellung fest. Um eine App-Einstellung hinzuzufügen, gehen Sie folgendermaßen vor:

1. Klicken Sie im Azure-Portal auf dem Blatt **Funktions-App** auf **Funktions-App-Einstellungen > Zu App Service-Einstellungen wechseln**.

2. Klicken Sie auf dem Blatt **Einstellungen** auf **Anwendungseinstellungen**.

3. Scrollen Sie zum Abschnitt **App-Einstellungen**, und fügen Sie einen Eintrag mit **Schlüssel** = *{eindeutiger Wert Ihrer Wahl}* und **Wert** = die Verbindungszeichenfolge für das Speicherkonto.

### <a id="storagequeuetrigger"></a> Azure Storage-Warteschlangentrigger

Die Datei *function.json* enthält den Namen der abzufragenden Warteschlange und den Variablennamen für die Warteschlangennachricht. Beispiel:

```json
{
    "disabled": false,
    "bindings": [
        {
            "name": "myQueueItem",
            "type": "queueTrigger",
            "direction": "in",
            "queueName": "myqueue-items",
            "connection":""
        }
    ]
}
```

#### Vom Warteschlangentrigger unterstützte Typen

Die Warteschlangennachricht kann auf alle folgenden Typen deserialisiert werden:

* `string`
* `byte[]`
* JSON-Objekt   
* `CloudQueueMessage`

#### Metadaten für Warteschlangentrigger

Sie können Warteschlangenmetadaten in Ihrer Funktion mithilfe dieser Variablennamen abrufen:

* expirationTime
* insertionTime
* nextVisibleTime
* id
* popReceipt
* dequeueCount
* queueTrigger (eine weitere Möglichkeit, den Text der Warteschlangennachricht als Zeichenfolge abzurufen)

In diesem C#-Codebeispiel werden Warteschlangenmetadaten abgerufen und protokolliert:

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
    log.Verbose($"C# Queue trigger function processed: {myQueueItem}\n" +
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

### <a id="storagequeueoutput"></a> Bindung der Azure Storage-Warteschlangenausgabe

Die Datei *function.json* enthält den Namen der Ausgabewarteschlange und einen Variablennamen für den Inhalt der Nachricht. In diesem Beispiel wird ein Warteschlangentrigger verwendet und eine Warteschlangennachricht geschrieben.

```json
{
  "bindings": [
    {
      "queueName": "myqueue-items",
      "connection": "",
      "name": "myQueueItem",
      "type": "queueTrigger",
      "direction": "in"
    },
    {
      "name": "myQueue",
      "type": "queue",
      "queueName": "samples-workitems-out",
      "connection": "",
      "direction": "out"
    }
  ],
  "disabled": false
}
``` 

#### Von der Bindung der Warteschlangenausgabe unterstützte Typen

Die `queue`-Bindung kann die folgenden Typen in eine Warteschlangennachricht serialisieren:

* `string` (Warteschlangennachricht wird erstellt, wenn der Parameterwert bei Funktionsbeendigung nicht NULL ist)
* `byte[]` (funktioniert wie eine Zeichenfolge) 
* `CloudQueueMessage` (funktioniert wie eine Zeichenfolge) 
* JSON-Objekt (erstellt eine Nachricht mit einem NULL-Objekt, wenn der Parameter bei Funktionsbeendigung NULL ist)

#### Codebeispiel für die Bindung der Warteschlangenausgabe

Dieses C#-Codebeispiel schreibt für jede Nachricht der Eingabewarteschlange eine einzige Nachricht der Ausgabewarteschlange.

```csharp
public static void Run(string myQueueItem, out string myOutputQueueItem, TraceWriter log)
{
    myOutputQueueItem = myQueueItem + "(next step)";
}
```

Dieses C#-Codebeispiel schreibt mehrere Nachrichten mit `ICollector<T>` (verwenden Sie `IAsyncCollector<T>` in einer Async-Funktion):

```csharp
public static void Run(string myQueueItem, ICollector<string> myQueue, TraceWriter log)
{
    myQueue.Add(myQueueItem + "(step 1)");
    myQueue.Add(myQueueItem + "(step 2)");
}
```

### <a id="storageblobtrigger"></a> Azure Storage-Blobtrigger

Die Datei *function.json* stellt einen Pfad bereit, der den zu überwachenden Container und bei Bedarf ein Blobnamensmuster angibt. Dieses Beispiel löst aus, wenn dem Container „samples-workitems“ Blobs hinzugefügt werden.

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

> [AZURE.NOTE] Wenn der Blobcontainer, der vom Trigger überwacht wird, mehr als 10.000 Blobs enthält, überprüft die Functions-Laufzeit Protokolldateien auf neue oder geänderte Blobs. Dieser Vorgang verläuft nicht in Echtzeit. Eine Funktion wird unter Umständen erst mehrere Minuten nach der Bloberstellung oder noch später ausgelöst. Darüber hinaus [werden Speicherprotokolle auf einer „best effort“-Basis](https://msdn.microsoft.com/library/azure/hh343262.aspx) erstellt; es gibt keine Garantie, dass alle Ereignisse erfasst werden. Unter bestimmten Umständen können Protokolle fehlen. Wenn die eingeschränkte Geschwindigkeit und Zuverlässigkeit von Blobtriggern für große Container für Ihre Anwendung nicht akzeptabel sind, empfiehlt es sich, zusammen mit dem Blob eine Warteschlangennachricht zu erstellen und für die Verarbeitung des Blobs anstelle des Blobtriggers einen Warteschlangentrigger zu verwenden.

#### Vom Blobtrigger unterstützte Typen

Blobs können in diese Typen deserialisiert werden:

* string
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

Dieses C#-Codebeispiel protokolliert den Inhalt der einzelnen Blobs, die dem Container hinzugefügt werden.

```csharp
public static void Run(string myBlob, TraceWriter log)
{
    log.Verbose($"C# Blob trigger function processed: {myBlob}");
}
```

#### Namensmuster für Blobtrigger

Sie können ein Blobnamensmuster im `path` angeben. Beispiel:

```json
"path": "input/original-{name}",
```

Dieser Pfad findet ein Blob mit dem Namen *original-Blob1.txt* im Container *input*, und der Wert der Variablen `name` im Funktionscode lautet `Blob1`.

Ein weiteres Beispiel:

```json
"path": "input/{blobname}.{blobextension}",
```

Mit diesem Pfad erhalten Sie ein Blob mit dem Namen *original-Blob1.txt*, und die Werte der Variablen `blobname` und `blobextension` im Funktionscode lauten *original-Blob1* und *txt*.

Sie können die Typen von Blobs einschränken, die die Funktion auslösen, indem Sie ein Muster mit einem festen Wert für die Dateierweiterung angeben. Wenn Sie den `path` auf *samples/{name}.png* festlegen, wird die Funktion nur durch *.png*-Blobs im Container *samples* ausgelöst.

Wenn Sie ein Namensmuster für Blobnamen angeben müssen, die geschweifte Klammern enthalten, verdoppeln Sie die geschweiften Klammern. Wenn Sie beispielsweise Blob im Container *images* suchen, deren Namen wie folgt lauten:

		{20140101}-soundfile.mp3

geben Sie für die `path`-Eigenschaft Folgendes an:

		images/{{20140101}}-{name}

In diesem Beispiel lautet der Wert der `name`-Variablen *soundfile.mp3*.

#### BLOB-Zugänge

Die Azure Functions-Laufzeit stellt sicher, dass Blobtriggerfunktionen für ein neues oder aktualisiertes Blob nicht mehrmals aufgerufen werden. Zu diesem Zweck wird mittels Verwaltung der *Blobbelege* bestimmt, ob eine bestimmte Blobversion verarbeitet wurde.

Blobbelege werden in einem Container mit dem Namen *azure-webjobs-hosts* in dem Azure-Speicherkonto gespeichert, das in der Verbindungszeichenfolge "AzureWebJobsStorage" angegeben ist. Ein Blobbeleg enthält die folgenden Informationen:

* Die Funktion, die für das Blob aufgerufen wurde („ *{Funktions-App-Name}* .Functions. *{Funktionsname}* “, Beispiel: „functionsf74b96f7.Functions.CopyBlob“)
* Der Containername
* Blobtyp ("BlockBlob" oder "PageBlob")
* Blobname
* ETag (eine Blobversions-ID. Beispiel: 0x8D1DC6E70A277EF)

Wenn Sie eine erneute Verarbeitung eines Blobs erzwingen möchten, können Sie den Blobbeleg für dieses Blob manuell aus dem Container *azure-webjobs-hosts* löschen.

#### Verarbeiten von nicht verarbeitbaren Blobs

Wenn bei einer Blobtriggerfunktion ein Fehler auftritt, wird sie für den Fall, dass es sich um einen vorübergehenden Fehler handelt, erneut vom SDK aufgerufen. Wenn der Inhalt des Blobs den Fehler verursacht hat, misslingt die Funktion bei jedem Versuch, das Blob zu verarbeiten. Standardmäßig ruft das SDK eine Funktion bis zu 5 Mal für ein angegebenes Blob auf. Ist auch der fünfte Versuch nicht erfolgreich, fügt das SDK einer Warteschlange namens *webjobs-blobtrigger-poison* eine Nachricht hinzu.

Die Warteschlangennachricht für nicht verarbeitbare Blobs ist ein JSON-Objekt, das die folgenden Eigenschaften enthält:

* FunctionId (im Format *{Funktions-App-Name}*.Functions.*{Funktionsname}*)
* BlobType ("BlockBlob" oder "PageBlob")
* ContainerName
* BlobName
* ETag (eine Blobversions-ID. Beispiel: 0x8D1DC6E70A277EF)

### <a id="storageblobbindings"></a> Azure Storage-Blobeingabe- und -ausgabebindungen

Die Datei *function.json* enthält den Namen des Containers und Variablennamen für Blobname und -inhalt. In diesem Beispiel wird ein Warteschlangentrigger verwendet, um ein Blob zu kopieren:

```json
{
  "bindings": [
    {
      "queueName": "myqueue-items",
      "connection": "",
      "name": "myQueueItem",
      "type": "queueTrigger",
      "direction": "in"
    },
    {
      "name": "myInputBlob",
      "type": "blob",
      "path": "samples-workitems/{queueTrigger}",
      "connection": "",
      "direction": "in"
    },
    {
      "name": "myOutputBlob",
      "type": "blob",
      "path": "samples-workitems/{queueTrigger}-Copy",
      "connection": "",
      "direction": "out"
    }
  ],
  "disabled": false
}
``` 

#### Für Blobeingabe- und Ausgabeparameter unterstützte Typen

Die `blob`-Bindung kann die folgenden Typen serialisieren oder deserialisieren:

* `Stream`
* `TextReader`
* `TextWriter`
* `string` (für Ausgabeblob: Erstellt ein Blob nur dann, wenn der Zeichenfolgenparameter bei Rückgabe der Funktion ungleich NULL ist)
* JSON-Objekt (für Ausgabeblob: Erstellt ein Blob als NULL-Objekt, wenn der Parameterwert bei Funktionsbeendigung NULL ist)
* `CloudBlobStream` (nur Ausgabe)
* `ICloudBlob`
* `CloudBlockBlob` 
* `CloudPageBlob` 

#### C#-Codebeispiel für Blobausgabe

Dieses C#-Codebeispiel kopiert ein Blob, dessen Name in einer Warteschlangennachricht empfangen wird.

```CSHARP
public static void Run(string myQueueItem, string myInputBlob, out string myOutputBlob, TraceWriter log)
{
    log.Verbose($"C# Queue trigger function processed: {myQueueItem}");
    myOutputBlob = myInputBlob;
}
```

### <a id="storagetablesbindings"></a> Azure Storage-Tabelleneingabe- und -ausgabebindungen

Die Datei *function.json* für Speichertabellen stellt mehrere Eigenschaften bereit:

* `name`: Name der Variablen für die Tabellenbindung im Code.
* `tableName`
* `partitionKey` und `rowKey`: Werden zusammen verwendet, um eine einzelne Entität in einer C#- oder Node-Funktion zu lesen oder um eine einzelne Entität in eine Node-Funktion zu schreiben.
* `take`: Maximale Anzahl von Zeilen, die für die Tabelleneingabe in einer Node-Funktion gelesen werden sollen.
* `filter`: OData-Filterausdruck für die Tabelleneingabe in einer Node-Funktion.

Diese Eigenschaften unterstützen die folgenden Szenarien:

* Lesen einer einzelnen Zeile in einer C#- oder Node-Funktion.

	Legen Sie `partitionKey` und `rowKey` fest. Die Eigenschaften `filter` und `take` werden in diesem Szenario nicht verwendet.

* Lesen mehrerer Zeilen in einer C#-Funktion.

	Die Functions-Laufzeit gibt ein an die Tabelle gebundenes `IQueryable<T>`-Objekt an. Der Typ `T` muss von `TableEntity` abgeleitet sein oder `ITableEntity` implementieren. Die Eigenschaften `partitionKey`, `rowKey`, `filter` und `take` werden in diesem Szenario nicht verwendet. Erforderliche Filtervorgänge können über das `IQueryable`-Objekt erfolgen.

* Lesen mehrerer Zeilen in einer Node-Funktion.

	Legen Sie die Eigenschaften `filter` und `take` fest. Legen Sie nicht `partitionKey` oder `rowKey` fest.

* Schreiben einer oder mehrerer Zeilen in eine C#-Funktion.

	Die Functions-Laufzeit stellt einen an die Tabelle gebundenen `ICollector<T>` oder `IAsyncCollector<T>` bereit, wobei `T` das Schema der Entitäten angibt, die Sie hinzufügen möchten. In der Regel ist der Typ `T` von `TableEntity` abgeleitet oder implementiert `ITableEntity`, was aber nicht sein muss. Die Eigenschaften `partitionKey`, `rowKey`, `filter` und `take` werden in diesem Szenario nicht verwendet.

#### Lesen einer einzelnen Tabellenentität in C# oder Node

Dieses Beispiel für *function.json* verwendet einen Warteschlangentrigger zum Lesen einer einzelnen Tabellenzeile mit einem hartcodierten Partitionsschlüsselwert und einem Zeilenschlüssel, der in der Warteschlangennachricht bereitgestellt wird.

```json
{
  "bindings": [
    {
      "queueName": "myqueue-items",
      "connection": "",
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
      "connection": "",
      "direction": "in"
    }
  ],
  "disabled": false
}
```
Das folgende C#-Codebeispiel kann mit der vorhergehenden Datei *function.json* verwendet werden, um eine einzelne Tabellenentität zu lesen. Die Warteschlangennachricht enthält den Zeilenschlüsselwert, und die Tabellenentität wird in einen Typ gelesen, der in der Datei *run.csx* definiert ist. Der Typ umfasst `PartitionKey`- und `RowKey`-Eigenschaften und wird nicht von `TableEntity` abgeleitet.

```csharp
public static void Run(string myQueueItem, Person personEntity, TraceWriter log)
{
    log.Verbose($"C# Queue trigger function processed: {myQueueItem}");
    log.Verbose($"Name in Person entity: {personEntity.Name}");
}

public class Person
{
    public string PartitionKey { get; set; }
    public string RowKey { get; set; }
    public string Name { get; set; }
}
```

Das folgende Node-Codebeispiel kann ebenfalls mit der vorhergehenden Datei *function.json* verwendet werden, um eine einzelne Tabellenentität zu lesen.

```javascript
module.exports = function (context, myQueueItem) {
    context.log('Node.js queue trigger function processed work item', myQueueItem);
    context.log('Person entity name: ' + context.bindings.personEntity.Name);
    context.done();
};
```

#### Lesen mehrerer Tabellenentitäten in C# 

Das folgende *function.json*- und C#-Codebeispiel liest Entitäten für einen Partitionsschlüssel, der in der Warteschlangennachricht angegeben ist.

```json
{
  "bindings": [
    {
      "queueName": "myqueue-items",
      "connection": "",
      "name": "myQueueItem",
      "type": "queueTrigger",
      "direction": "in"
    },
    {
      "name": "tableBinding",
      "type": "table",
      "tableName": "Person",
      "direction": "in"
    }
  ],
  "disabled": false
}
```

Der C#-Code fügt einen Verweis auf das Azure Storage-SDK hinzu, sodass der Entitätstyp von `TableEntity` abgeleitet werden kann.

```csharp
#r "Microsoft.WindowsAzure.Storage"
using Microsoft.WindowsAzure.Storage.Table;

public static void Run(string myQueueItem, IQueryable<Person> tableBinding, TraceWriter log)
{
    log.Verbose($"C# Queue trigger function processed: {myQueueItem}");
    foreach (Person person in tableBinding.Where(p => p.PartitionKey == myQueueItem).ToList())
    {
        log.Verbose($"Name: {person.Name}");
    }
}

public class Person : TableEntity
{
    public string Name { get; set; }
}
``` 

#### Erstellen mehrerer Tabellenentitäten in C# 

Im folgenden *function.json*- und *run.csx*-Beispiel wird veranschaulicht, wie Tabellenentitäten in C# geschrieben werden.

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
      "connection": "",
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
            log.Verbose($"Adding Person entity {i}");
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

#### Erstellen einer Tabellenentität in Node

Im folgenden *function.json*- und *run.csx*-Beispiel wird veranschaulicht, wie eine Tabellenentität in Node geschrieben wird.

```json
{
  "bindings": [
    {
      "queueName": "myqueue-items",
      "connection": "",
      "name": "myQueueItem",
      "type": "queueTrigger",
      "direction": "in"
    },
    {
      "tableName": "Person",
      "partitionKey": "Test",
      "rowKey": "{queueTrigger}",
      "connection": "",
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

## Azure DocumentDB-Bindungen

Dieser Abschnitt enthält die folgenden Unterabschnitte:

* [Azure DocumentDB-Eingabebindung](#docdbinput)
* [Azure DocumentDB-Ausgabebindung](#docdboutput)

### <a id="docdbinput"></a> Azure DocumentDB-Eingabebindung

Eingabebindungen können ein Dokument aus einer DocumentDB-Sammlung laden und direkt an Ihre Bindung übergeben. Die Dokument-ID kann basierend auf dem Trigger ermittelt werden, der die Funktion aufgerufen hat. In einer C#-Funktion werden alle Änderungen am Datensatz automatisch wieder an die Sammlung zurückgesendet, wenn die Funktion erfolgreich beendet wird.

Die Datei „function.json“ enthält die folgenden Eigenschaften zur Verwendung mit der DocumentDB-Eingabebindung:

- `name`: Variablenname, der im Funktionscode für das Dokument verwendet wird.
- `type`: Muss auf „documentdb“ festgelegt werden.
- `databaseName`: Die Datenbank mit dem Dokument.
- `collectionName`: Die Sammlung mit dem Dokument.
- `id`: Die ID des abzurufenden Dokuments. Diese Eigenschaft unterstützt Bindungen wie „{queueTrigger}“, die den Zeichenfolgenwert der Warteschlangennachricht als Dokument-ID verwenden.
- `connection`: Diese Zeichenfolge muss eine Anwendungseinstellung sein, die auf den Endpunkt für Ihr DocumentDB-Konto festgelegt ist. Wenn Sie Ihr Konto auf der Registerkarte „Integrieren“ auswählen, wird eine neue App-Einstellung mit einem Namen für Sie erstellt, der das folgende Format aufweist: IhrKonto\_DOCUMENTDB. Wenn Sie die App-Einstellung manuell erstellen möchten, muss die eigentliche Verbindungszeichenfolge im folgenden Format vorliegen: AccountEndpoint=<Endpoint for your account>;AccountKey=<Your primary access key>;.
- direction: Muss auf *"in"* festgelegt werden.

Beispiel für „function.json“:
 
	{
	  "bindings": [
	    {
	      "name": "document",
	      "type": "documentdb",
	      "databaseName": "MyDatabase",
	      "collectionName": "MyCollection",
	      "id" : "{queueTrigger}",
	      "connection": "MyAccount_DOCUMENTDB",     
	      "direction": "in"
	    }
	  ],
	  "disabled": false
	}

#### Azure DocumentDB-Eingabecodebeispiel für einen C#-Warteschlangentrigger
 
Bei Verwendung des oben genannten Beispiels für „function.json“ ruft die DocumentDB-Eingabebindung das Dokument mit der ID ab, die der Zeichenfolge der Warteschlangennachricht entspricht, und übergibt es an den Parameter „document“. Wenn das Dokument nicht gefunden wird, lautet der Parameter „document“ NULL. Das Dokument wird dann mit dem neuen Textwert aktualisiert, wenn die Funktion beendet wird.
 
	public static void Run(string myQueueItem, dynamic document)
	{   
	    document.text = "This has changed.";
	}

#### Azure DocumentDB-Eingabecodebeispiel für einen Node.js-Warteschlangentrigger
 
Bei Verwendung des oben genannten Beispiels für „function.json“ ruft die DocumentDB-Eingabebindung das Dokument mit der ID ab, die der Zeichenfolge der Warteschlangennachricht entspricht, und übergibt es an die Bindungseigenschaft `documentIn`. In Node.js-Funktionen werden aktualisierte Dokumente nicht an die Sammlung zurückgesendet. Sie können die Eingabebindung jedoch direkt an eine DocumentDB-Ausgabebindung namens `documentOut` übergeben, um Updates zu unterstützen. In diesem Codebeispiel wird die Texteigenschaft des Eingabedokuments aktualisiert und als Ausgabedokument festgelegt.
 
	module.exports = function (context, input) {   
	    context.bindings.documentOut = context.bindings.documentIn;
	    context.bindings.documentOut.text = "This was updated!";
	    context.done();
	};

### <a id="docdboutput"></a> Azure DocumentDB-Ausgabebindungen

Ihre Funktionen können JSON-Dokumente mithilfe der Ausgabebindung **Azure DocumentDB-Dokument** in eine Azure-DocumentDB-Datenbank schreiben. Weitere Informationen zu Azure DocumentDB erhalten Sie unter [Einführung in DocumentDB](../documentdb/documentdb-introduction.md) und im [Tutorial mit den ersten Schritten](../documentdb/documentdb-get-started.md).

Die Datei „function.json“ enthält die folgenden Eigenschaften zur Verwendung mit der DocumentDB-Ausgabebindung:

- `name`: Variablenname, der im Funktionscode für das neue Dokument verwendet wird.
- `type`: Muss auf *"documentdb"* festgelegt werden.
- `databaseName`: Die Datenbank mit der Sammlung, in der das neue Dokument erstellt wird.
- `collectionName`: Die Sammlung, in der das neue Dokument erstellt wird.
- `createIfNotExists`: Dies ist ein boolescher Wert, der angibt, ob die Sammlung erstellt wird, wenn sie nicht vorhanden ist. Die Standardeinstellung ist *false*. Der Grund hierfür ist, dass Sammlungen mit reserviertem Durchsatz erstellt werden, was sich auf den Preis auswirkt. Weitere Informationen finden Sie in der [Preisübersicht](https://azure.microsoft.com/pricing/details/documentdb/).
- `connection`: Diese Zeichenfolge muss eine **Anwendungseinstellung** sein, die auf den Endpunkt für Ihr DocumentDB-Konto festgelegt ist. Wenn Sie Ihr Konto auf der Registerkarte **Integrieren** auswählen, wird eine neue App-Einstellung mit einem Namen für Sie erstellt, der das folgende Format aufweist: `yourAccount_DOCUMENTDB`. Wenn Sie die App-Einstellung manuell erstellen möchten, muss die eigentliche Verbindungszeichenfolge im folgenden Format vorliegen: `AccountEndpoint=<Endpoint for your account>;AccountKey=<Your primary access key>;`. 
- `direction`: Muss auf *"out"* festgelegt werden. 
 
Beispiel für „function.json“:

	{
	  "bindings": [
	    {
	      "name": "document",
	      "type": "documentdb",
	      "databaseName": "MyDatabase",
	      "collectionName": "MyCollection",
	      "createIfNotExists": false,
	      "connection": "MyAccount_DOCUMENTDB",
	      "direction": "out"
	    }
	  ],
	  "disabled": false
	}


#### Azure DocumentDB-Ausgabecodebeispiel für einen Node.js-Warteschlangentrigger

	module.exports = function (context, input) {
	   
	    context.bindings.document = {
	        text : "I'm running in a Node function! Data: '" + input + "'"
	    }   
	 
	    context.done();
	};

Das Ausgabedokument:

	{
	  "text": "I'm running in a Node function! Data: 'example queue data'",
	  "id": "01a817fe-f582-4839-b30c-fb32574ff13f"
	}
 

#### Azure DocumentDB-Ausgabecodebeispiel für einen C#-Warteschlangentrigger


	using System;

	public static void Run(string myQueueItem, out object document, TraceWriter log)
	{
	    log.Verbose($"C# Queue trigger function processed: {myQueueItem}");
	   
	    document = new {
	        text = $"I'm running in a C# function! {myQueueItem}"
	    };
	}


#### Azure DocumentDB-Ausgabecodebeispiel: Festlegen des Dateinamens

Wenn Sie den Namen des Dokuments in der Funktion festlegen möchten, legen Sie einfach den `id`-Wert fest. Beispielsweise wurde JSON-Inhalt für einen Mitarbeiter in etwa wie folgt in der Warteschlange abgelegt:

	{
	  "name" : "John Henry",
      "employeeId" : "123456",
	  "address" : "A town nearby"
	}

Sie könnten den folgenden C#-Code in einer Warteschlangentrigger-Funktion verwenden:
	
	#r "Newtonsoft.Json"
	
	using System;
	using Newtonsoft.Json;
	using Newtonsoft.Json.Linq;
	
	public static void Run(string myQueueItem, out object employeeDocument, TraceWriter log)
	{
	    log.Verbose($"C# Queue trigger function processed: {myQueueItem}");
	    
	    dynamic employee = JObject.Parse(myQueueItem);
	    
	    employeeDocument = new {
	        id = employee.name + "-" + employee.employeeId,
	        name = employee.name,
	        employeeId = employee.employeeId,
	        address = employee.address
	    };
	}

Beispielausgabe:

	{
	  "id": "John Henry-123456",
	  "name": "John Henry",
	  "employeeId": "123456",
	  "address": "A town nearby"
	}

## Einfache Tabellenbindungen in mobilen Azure-Apps

Mit mobilen Azure App Service-Apps können Sie Tabellenendpunkt-Daten auf mobilen Clients verfügbar machen. Dieselben Tabellendaten können sowohl in Eingabe- als auch in Ausgabebindungen mit Azure Functions verwendet werden. Bei einer mobilen Node.js-Back-End-App können Sie über *Einfache Tabellen* mit diesen Tabellendaten im Azure-Portal arbeiten. Einfache Tabellen unterstützen das dynamische Schema, sodass Spalten automatisch entsprechend der Form der eingefügten Daten hinzugefügt werden, um die Schemaentwicklung zu vereinfachen. Das dynamische Schema ist standardmäßig aktiviert und sollte in einer mobilen Produktions-App deaktiviert werden. Weitere Informationen zu einfachen Tabellen in mobilen Apps finden Sie unter [Gewusst wie: Verwenden einfacher Tabellen im Azure-Portal](../app-service-mobile/app-service-mobile-node-backend-how-to-use-server-sdk.md#in-portal-editing). Beachten Sie, dass einfache Tabellen im Portal für mobile .NET-Back-End-Apps derzeit nicht unterstützt werden. Sie können zwar Funktionsbindungen für Tabellenendpunkte in mobilen .NET-Back-End-Apps verwenden, aber das dynamische Schema wird in mobilen .NET-Back-End-Apps nicht unterstützt.

Dieser Abschnitt enthält die folgenden Unterabschnitte:

* [API-Schlüssel für einfache Tabellen in mobilen Azure-Apps](#easytablesapikey)
* [Eingabebindung für einfache Tabellen in mobilen Azure-Apps](#easytablesinput)
* [Ausgabebindung für einfache Tabellen in mobilen Azure-Apps](#easytablesoutput)

### <a id="easytablesapikey"></a> Verwenden Sie einen API-Schlüssel zum Sichern des Zugriffs auf die Endpunkte für einfache Tabellen in Ihren mobilen Apps.

Azure Functions kann derzeit nicht auf Endpunkte zugreifen, die durch die App Service-Authentifizierung gesichert werden. Daher müssen Endpunkte von mobilen Apps, die Sie in Ihren Funktionen mit einfachen Tabellenbindungen verwenden, den anonymen Zugriff zulassen müssen. Dies ist die Standardeinstellung. Bei einfachen Tabellenbindungen können Sie einen API-Schlüssel angeben. Dabei handelt es sich um einen gemeinsamen geheimen Schlüssel, mit dem der unbefugte Zugriff aus anderen Apps als Ihren Funktionen verhindert wird. Mobile Apps bieten keine integrierte Unterstützung für die Authentifizierung von API-Schlüsseln. Sie können jedoch einen API-Schlüssel in der mobilen Node.js-Back-End-App implementieren, indem Sie die Beispiele unter [Azure App Service Mobile Apps backend implementing an API key](https://github.com/Azure/azure-mobile-apps-node/tree/master/samples/api-key) (Back-End mobiler Azure App Service-Apps zum Implementieren eines API-Schlüssels) befolgen.

>[AZURE.IMPORTANT] Dieser API-Schlüssel darf nicht mit Ihren mobilen App-Clients verteilt werden, er sollte nur sicher an dienstseitige Clients wie Azure Functions verteilt werden.

### <a id="easytablesinput"></a> Eingabebindung für einfache Tabellen in mobilen Azure-Apps

Eingabebindungen können einen Datensatz aus einem Tabellenendpunkt von mobilen Apps laden und direkt an Ihre Bindung übergeben. Die Datensatz-ID wird anhand des Triggers ermittelt, der die Funktion aufgerufen hat. In einer C#-Funktion werden alle Änderungen am Datensatz automatisch wieder an die Tabelle zurückgesendet, wenn die Funktion erfolgreich beendet wird.

Die Datei „function.json“ unterstützt die folgenden Eigenschaften für die Verwendung mit Eingabebindungen für einfache Tabellen in mobilen Apps:

- `name`: Variablenname, der im Funktionscode für den neuen Datensatz verwendet wird.
- `type`: Bindungstyp muss auf *easyTable* festgelegt werden.
- `tableName`: Die Tabelle, in der der neue Datensatz erstellt wird.
- `id`: Die ID des abzurufenden Datensatzes. Diese Eigenschaft unterstützt Bindungen wie `{queueTrigger}`, die den Zeichenfolgenwert der Warteschlangennachricht als Datensatz-ID verwenden.
- `apiKey`: Zeichenfolge, die der Anwendungseinstellung entspricht, die den optionalen API-Schlüssel für die mobile App angibt. Dies ist erforderlich, wenn Ihre mobile App einen API-Schlüssel verwendet, um den Clientzugriff einzuschränken.
- `connection`: Zeichenfolge, die der Anwendungseinstellung entspricht, die den URI für Ihre mobile App angibt.
- `direction`: Bindungsrichtung, die auf *in* festgelegt werden muss.

Beispiel für „function.json“:

	{
	  "bindings": [
	    {
	      "name": "record",
	      "type": "easyTable",
	      "tableName": "MyTable",
	      "id" : "{queueTrigger}",
	      "connection": "My_MobileApp_Uri",
	      "apiKey": "My_MobileApp_Key",
	      "direction": "in"
	    }
	  ],
	  "disabled": false
	}

#### Codebeispiel für einfache Tabellen in mobilen Azure-Apps für einen C#-Warteschlangentrigger

Basierend auf dem oben genannten Beispiel für „function.json“ ruft die Eingabebindung den Datensatz mit der ID ab, die der Zeichenfolge der Warteschlangennachricht entspricht, und übergibt ihn an den Parameter *record*. Wenn der Datensatz nicht gefunden wird, ist der Parameter NULL. Der Datensatz wird dann mit dem neuen *Text*-Wert aktualisiert, wenn die Funktion beendet wird.

	#r "Newtonsoft.Json"	
	using Newtonsoft.Json.Linq;
	
	public static void Run(string myQueueItem, JObject record)
	{
	    if (record != null)
	    {
	        record["Text"] = "This has changed.";
	    }    
	}

#### Codebeispiel für einfache Tabellen in mobilen Azure-Apps für einen Node.js-Warteschlangentrigger

Basierend auf dem oben genannten Beispiel für „function.json“ ruft die Eingabebindung den Datensatz mit der ID ab, die der Zeichenfolge der Warteschlangennachricht entspricht, und übergibt ihn an den Parameter *record*. In Node.js-Funktionen werden aktualisierte Datensätze nicht an die Tabelle zurückgesendet. Dieses Codebeispiel schreibt den abgerufenen Datensatz in das Protokoll.

	module.exports = function (context, input) {    
	    context.log(context.bindings.record);
	    context.done();
	};


### <a id="easytablesoutput"></a> Ausgabebindung für einfache Tabellen in mobilen Azure-Apps

Ihre Funktion kann einen Datensatz über eine Ausgabebindung mit einfachen Tabellen in einen Tabellenendpunkt für mobile Apps schreiben.

Die Datei „function.json“ unterstützt die folgenden Eigenschaften für die Verwendung mit Ausgabebindungen für einfache Tabellen:

- `name`: Variablenname, der im Funktionscode für den neuen Datensatz verwendet wird.
- `type`: Bindungstyp, der auf *easyTable* festgelegt werden muss.
- `tableName`: Die Tabelle, in der der neue Datensatz erstellt wird.
- `apiKey`: Zeichenfolge, die der Anwendungseinstellung entspricht, die den optionalen API-Schlüssel für die mobile App angibt. Dies ist erforderlich, wenn Ihre mobile App einen API-Schlüssel verwendet, um den Clientzugriff einzuschränken.
- `connection`: Zeichenfolge, die der Anwendungseinstellung entspricht, die den URI für Ihre mobile App angibt.
- `direction`: Bindungsrichtung, die auf *out* festgelegt werden muss.

Beispiel für „function.json“:

	{
	  "bindings": [
	    {
	      "name": "record",
	      "type": "easyTable",
	      "tableName": "MyTable",
	      "connection": "My_MobileApp_Uri",
	      "apiKey": "My_MobileApp_Key",
	      "direction": "out"
	    }
	  ],
	  "disabled": false
	}

#### Codebeispiel für einfache Tabellen in mobilen Azure-Apps für einen C#-Warteschlangentrigger

Dieses C#-Codebeispiel fügt einen neuen Datensatz mit einer *Text*-Eigenschaft in die Tabelle ein, die in der obigen Bindung angegeben ist.

	public static void Run(string myQueueItem, out object record)
	{
	    record = new {
	        Text = $"I'm running in a C# function! {myQueueItem}"
	    };
	}

#### Codebeispiel für einfache Tabellen in mobilen Azure-Apps für einen Node.js-Warteschlangentrigger

Dieses Node.js-Codebeispiel fügt einen neuen Datensatz mit einer *Text*-Eigenschaft in die Tabelle ein, die in der obigen Bindung angegeben ist.

	module.exports = function (context, input) {
	
	    context.bindings.record = {
	        text : "I'm running in a Node function! Data: '" + input + "'"
	    }   
	
	    context.done();
	};

## Azure Notification Hub-Ausgabebindung

Ihre Funktionen können über einen konfigurierten Azure Notification Hub mit sehr wenigen Codezeilen Pushbenachrichtigungen senden. Allerdings muss der Notification Hub für das PNS (Plattformbenachrichtigungssystem) konfiguriert sein, das Sie verwenden möchten. Weitere Informationen zum Konfigurieren eines Azure Notification Hubs und zum Entwickeln von Clientanwendungen, die sich für Benachrichtigungen registrieren, finden Sie unter [Erste Schritte mit Notification Hubs](../notification-hubs/notification-hubs-windows-store-dotnet-get-started.md), wenn Sie oben auf die Client-Zielplattform klicken.

Die Datei „function.json“ enthält die folgenden Eigenschaften zur Verwendung mit einer Notification Hub-Ausgabebindung:

- `name`: Variablenname, der im Funktionscode für die Notification Hub-Nachricht verwendet wird.
- `type`: Muss auf *"notificationHub"* festgelegt werden.
- `tagExpression`: Mit Tagausdrücken können Sie Benachrichtigungen an eine Reihe von Geräten übermitteln lassen, die sich für den Empfang von Benachrichtigungen registriert haben, die dem Tagausdruck entsprechen. Weitere Informationen finden Sie unter [Weiterleitung und Tagausdrücke](../notification-hubs/notification-hubs-routing-tag-expressions.md).
- `hubName`: Der Name der Notification Hub-Ressource im Azure-Portal.
- `connection`: Diese Verbindungszeichenfolge muss eine Verbindungszeichenfolge für die **Anwendungseinstellung** sein, die auf den *DefaultFullSharedAccessSignature*-Wert für Ihren Notification Hub festgelegt ist.
- `direction`: Muss auf *"out"* festgelegt werden. 
 
Beispiel für „function.json“:

	{
	  "bindings": [
	    {
	      "name": "notification",
	      "type": "notificationHub",
	      "tagExpression": "",
	      "hubName": "my-notification-hub",
	      "connection": "MyHubConnectionString",
	      "direction": "out"
	    }
	  ],
	  "disabled": false
	}

### Einrichten der Azure Notification Hub-Verbindungszeichenfolge

Um eine Notification Hub-Ausgabebindung zu verwenden, müssen Sie die Verbindungszeichenfolge für den Hub konfigurieren. Dies kann auf der Registerkarte *Integrieren* erfolgen, indem Sie einfach Ihren Notification Hub auswählen oder einen neuen erstellen.

Sie können eine Verbindungszeichenfolge für einen vorhandenen Hub auch manuell hinzufügen, indem Sie Ihrem Notification Hub eine Verbindungszeichenfolge für *DefaultFullSharedAccessSignature* hinzufügen. Diese Verbindungszeichenfolge stellt Ihrer Funktion die Zugriffsberechtigungen zum Senden von Nachrichten bereit. Der Wert der *DefaultFullSharedAccessSignature*-Verbindungszeichenfolge ist über die Schaltfläche **Schlüssel** auf dem Hauptblatt der Notification Hub-Ressource im Azure-Portal zugänglich. Um eine Verbindungszeichenfolge für den Hub manuell hinzuzufügen, gehen Sie folgendermaßen vor:

1. Klicken Sie im Azure-Portal auf dem Blatt **Funktions-App** auf **Funktions-App-Einstellungen > Zu App Service-Einstellungen wechseln**.

2. Klicken Sie auf dem Blatt **Einstellungen** auf **Anwendungseinstellungen**.

3. Scrollen Sie nach unten zum Abschnitt **Verbindungszeichenfolgen**, und fügen Sie einen benannten Eintrag für den *DefaultFullSharedAccessSignature*-Wert für Ihren Notification Hub hinzu. Ändern Sie den Typ in **Benutzerdefiniert**.
4. Verweisen Sie in den Ausgabebindungen auf den Namen der Verbindungszeichenfolge, ähnlich wie bei **MyHubConnectionString** im obigen Beispiel.

### Azure Notification Hub-Codebeispiel für einen Node.js-Trigger mit Timer 

In diesem Beispiel wird eine Benachrichtigung für eine [Vorlagenregistrierung](../notification-hubs/notification-hubs-templates.md) gesendet, die `location` und `message` enthält.

	module.exports = function (context, myTimer) {
	    var timeStamp = new Date().toISOString();
	   
	    if(myTimer.isPastDue)
	    {
	        context.log('Node.js is running late!');
	    }
	    context.log('Node.js timer trigger function ran!', timeStamp);  
	    context.bindings.notification = {
	        location: "Redmond",
	        message: "Hello from Node!"
	    };
	    context.done();
	};

### Azure Notification Hub-Codebeispiel für einen C#-Warteschlangentrigger

In diesem Beispiel wird eine Benachrichtigung für eine [Vorlagenregistrierung](../notification-hubs/notification-hubs-templates.md) gesendet, die `message` enthält.


	using System;
	using System.Threading.Tasks;
	using System.Collections.Generic;
	 
	public static void Run(string myQueueItem,  out IDictionary<string, string> notification, TraceWriter log)
	{
	    log.Verbose($"C# Queue trigger function processed: {myQueueItem}");
        notification = GetTemplateProperties(myQueueItem);
	}
	 
	private static IDictionary<string, string> GetTemplateProperties(string message)
	{
	    Dictionary<string, string> templateProperties = new Dictionary<string, string>();
	    templateProperties["message"] = message;
	    return templateProperties;
	}

In diesem Beispiel wird eine Benachrichtigung für eine [Vorlagenregistrierung](../notification-hubs/notification-hubs-templates.md) mit `message` über eine gültige JSON-Zeichenfolge gesendet.

	using System;
	 
	public static void Run(string myQueueItem,  out string notification, TraceWriter log)
	{
		log.Verbose($"C# Queue trigger function processed: {myQueueItem}");
		notification = "{"message":"Hello from C#. Processed a queue item!"}";
	}

### C#-Codebeispiel für einen Azure Notification Hub-Warteschlangentrigger mit Benachrichtigungstyp

Dieses Beispiel zeigt, wie Sie den in der [Bibliothek für Microsoft Azure Notification Hubs](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/) definierten `Notification`-Typ verwenden. Um diesen Typ und die Bibliothek zu verwenden, müssen Sie eine Datei *project.json* für Ihre Funktions-App hochladen. Die Datei „project.json“ ist eine JSON-Textdatei, die in etwa folgendermaßen aussieht:

	{
	  "frameworks": {
	    ".NETFramework,Version=v4.6": {
	      "dependencies": {
	        "Microsoft.Azure.NotificationHubs": "1.0.4"
	      }
	    }
	  }
	}

Weitere Informationen zum Hochladen der Datei „project.json“ finden Sie unter [Hochladen einer Datei „project.json“](http://stackoverflow.com/questions/36411536/how-can-i-use-nuget-packages-in-my-azure-functions).

Beispielcode:

	using System;
	using System.Threading.Tasks;
	using Microsoft.Azure.NotificationHubs;
	 
	public static void Run(string myQueueItem,  out Notification notification, TraceWriter log)
	{
	   log.Verbose($"C# Queue trigger function processed: {myQueueItem}");
	   notification = GetTemplateNotification(myQueueItem);
	}
	private static TemplateNotification GetTemplateNotification(string message)
	{
	    Dictionary<string, string> templateProperties = new Dictionary<string, string>();
	    templateProperties["message"] = message;
	    return new TemplateNotification(templateProperties);
	}

## Nächste Schritte

Weitere Informationen finden Sie in den folgenden Ressourcen:

* [Entwicklerreferenz zu Azure Functions](functions-reference.md)
* [C#-Entwicklerreferenz zu Azure Functions](functions-reference-csharp.md)
* [NodeJS-Entwicklerreferenz zu Azure Functions](functions-reference-node.md)

<!----HONumber=AcomDC_0420_2016-->