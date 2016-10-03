<properties
	pageTitle="HTTP- und Webhookbindungen in Azure Functions | Microsoft Azure"
	description="Erfahren Sie, wie HTTP- und Webhooktrigger und -Bindungen in Azure Functions verwendet werden."
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
	ms.date="08/22/2016"
	ms.author="chrande"/>

# HTTP- und Webhookbindungen in Azure Functions

[AZURE.INCLUDE [functions-selector-bindings](../../includes/functions-selector-bindings.md)]

Dieser Artikel erläutert das Konfigurieren und Codieren von HTTP- und Webhooktriggern und -Bindungen in Azure Functions.

[AZURE.INCLUDE [Einführung](../../includes/functions-bindings-intro.md)]

## „function.json“ für HTTP und Webhookbindungen

Die Datei *function.json* enthält Eigenschaften, die die Anforderung und die Antwort betreffen.

Eigenschaften für die HTTP-Anforderung:

- `name`: Der Variablenname, der im Funktionscode für das Anforderungsobjekt (oder bei Node.js-Funktionen im Hauptteil der Anforderung) verwendet wird.
- `type`: Muss auf *httpTrigger* festgelegt werden.
- `direction`: Muss auf *in* festgelegt werden.
- `webHookType`: Gültige Werte für Webhook-Trigger sind *github*, *slack* und *genericJson*. Für einen HTTP-Trigger, der kein WebHook ist, legen Sie diese Eigenschaft auf eine leere Zeichenfolge fest. Weitere Informationen zu Webhooks finden Sie weiter unten im Abschnitt [WebHook-Trigger](#webhook-triggers).
- `authLevel`: Für Webhook-Trigger nicht relevant. Legen Sie den Wert auf „function“ fest, um einen API-Schlüssel zu verlangen, auf „anonymous“, um die Anforderung eines API-Schlüssels aufzuheben, oder auf „admin“, um den Master-API-Schlüssel zu verlangen. Weitere Informationen finden Sie weiter unten unter [API-Schlüssel](#apikeys).

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
      "direction": "in",
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

## WebHook-Trigger

Ein WebHook-Trigger ist ein HTTP-Trigger, der die folgenden für WebHooks entwickelten Features umfasst:

* Für bestimmte WebHook-Anbieter (zurzeit werden GitHub und Slack unterstützt) wird die Signatur des Anbieters durch die Functions-Laufzeit überprüft.
* Für Node.js-Funktionen stellt die Functions-Laufzeit den Anforderungstext anstelle des Anforderungsobjekts bereit. Für C#-Funktionen gibt es keine spezielle Verarbeitung, weil Sie durch die Angabe des Parametertyps steuern, was bereitgestellt wird. Bei Angabe von `HttpRequestMessage` erhalten Sie das Anforderungsobjekt. Wenn Sie einen POCO-Typ angeben, versucht die Functions-Laufzeit, ein JSON-Objekt im Anforderungstext zu analysieren, um die Objekteigenschaften aufzufüllen.
* Zum Auslösen einer WebHook-Funktion muss die HTTP-Anforderung einen API-Schlüssel enthalten. Für andere HTTP-Trigger als WebHook ist diese Anforderung optional.

Informationen zum Einrichten eines GitHub-Webhooks finden Sie unter [GitHub Developer – Creating Webhooks](http://go.microsoft.com/fwlink/?LinkID=761099&clcid=0x409) (Erstellen von Webhooks).

## URL zum Auslösen der Funktion

Zum Auslösen einer Funktion senden Sie eine HTTP-Anforderung an eine URL, die eine Kombination aus der Funktionen-App-URL und dem Funktionsnamen:

```
 https://{function app name}.azurewebsites.net/api/{function name} 
```

## API-Schlüssel

Standardmäßig muss eine HTTP-Anforderung einen API-Schlüssel aufweisen, um eine HTTP- oder WebHook-Funktion auszulösen. Der Schlüssel kann in einer Abfragezeichenfolgen-Variablen namens `code` oder in einem `x-functions-key`-HTTP-Header enthalten sein. Für andere Funktionen als WebHook können Sie angeben, dass kein API-Schlüssel erforderlich ist, indem Sie in der Datei *function.json* die `authLevel`-Eigenschaft auf „anonymous“ festlegen.

Sie finden API-Schlüsselwerte im Ordner *D:\\home\\data\\Functions\\secrets* im Dateisystem der Funktionen-App. Der Hauptschlüssel und der Funktionsschlüssel werden in der Datei *host.json* festgelegt, wie im folgenden Beispiel gezeigt.

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

## C#-Beispielcode für eine HTTP-Triggerfunktion 

Der Beispielcode sucht in der Abfragezeichenfolge oder im Text der HTTP-Anforderung nach einem `name`-Parameter.

```csharp
using System.Net;
using System.Threading.Tasks;

public static async Task<HttpResponseMessage> Run(HttpRequestMessage req, TraceWriter log)
{
    log.Info($"C# HTTP trigger function processed a request. RequestUri={req.RequestUri}");

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

## F#-Beispielcode für eine HTTP-Triggerfunktion

Der Beispielcode sucht in der Abfragezeichenfolge oder im Text der HTTP-Anforderung nach einem `name`-Parameter.

```fsharp
open System.Net
open System.Net.Http
open FSharp.Interop.Dynamic

let Run(req: HttpRequestMessage) =
    async {
        let q =
            req.GetQueryNameValuePairs()
                |> Seq.tryFind (fun kv -> kv.Key = "name")
        match q with
        | Some kv ->
            return req.CreateResponse(HttpStatusCode.OK, "Hello " + kv.Value)
        | None ->
            let! data = Async.AwaitTask(req.Content.ReadAsAsync<obj>())
            try
                return req.CreateResponse(HttpStatusCode.OK, "Hello " + data?name)
            with e ->
                return req.CreateErrorResponse(HttpStatusCode.BadRequest, "Please pass a name on the query string or in the request body")
    } |> Async.StartAsTask
```

Sie benötigen eine `project.json`-Datei, die wie folgt mithilfe von NuGet auf die Assemblys `FSharp.Interop.Dynamic` und `Dynamitey` verweist:

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

Dabei wird NuGet zum Abrufen der Abhängigkeiten verwendet, auf die dann in Ihrem Skript verwiesen wird.

## Node.js-Beispielcode für eine HTTP-Triggerfunktion 

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

## C#-Beispielcode für eine GitHub-WebHook-Funktion 

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

    log.Info($"WebHook was triggered! Comment: {data.comment.body}");

    return req.CreateResponse(HttpStatusCode.OK, new {
        body = $"New GitHub comment: {data.comment.body}"
    });
}
```

## F#-Beispielcode für eine GitHub-WebHook-Funktion

Dieser Beispielcode protokolliert GitHub-Kommentare.

```fsharp
open System.Net
open System.Net.Http
open FSharp.Interop.Dynamic
open Newtonsoft.Json

type Response = {
    body: string
}

let Run(req: HttpRequestMessage, log: TraceWriter) =
    async {
        let! content = req.Content.ReadAsStringAsync() |> Async.AwaitTask
        let data = content |> JsonConvert.DeserializeObject
        log.Info(sprintf "GitHub WebHook triggered! %s" data?comment?body)
        return req.CreateResponse(
            HttpStatusCode.OK,
            { body = sprintf "New GitHub comment: %s" data?comment?body })
    } |> Async.StartAsTask
```

## Node.js-Beispielcode für eine GitHub-WebHook-Funktion 

Dieser Beispielcode protokolliert GitHub-Kommentare.

```javascript
module.exports = function (context, data) {
    context.log('GitHub WebHook triggered!', data.comment.body);
    context.res = { body: 'New GitHub comment: ' + data.comment.body };
    context.done();
};
```

## Nächste Schritte

[AZURE.INCLUDE [Nächste Schritte](../../includes/functions-bindings-next-steps.md)]

<!---HONumber=AcomDC_0921_2016-->