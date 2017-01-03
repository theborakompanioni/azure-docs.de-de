---
title: HTTP- und Webhookbindungen in Azure Functions | Microsoft Docs
description: Erfahren Sie, wie HTTP- und Webhooktrigger und -Bindungen in Azure Functions verwendet werden.
services: functions
documentationcenter: na
author: mattchenderson
manager: erikre
editor: 
tags: 
keywords: Azure Functions, Functions, Ereignisverarbeitung, Webhooks, dynamisches Computing, serverlose Architektur, HTTP, API, REST
ms.assetid: 2b12200d-63d8-4ec1-9da8-39831d5a51b1
ms.service: functions
ms.devlang: multiple
ms.topic: reference
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 11/18/2016
ms.author: mahender
translationtype: Human Translation
ms.sourcegitcommit: 412640c0c53ca85dbdc234783ba94afaa807a22a
ms.openlocfilehash: f1e4ecfd91e161f71115bed31cd41684ed514b5a


---
# <a name="azure-functions-http-and-webhook-bindings"></a>HTTP- und Webhookbindungen in Azure Functions
[!INCLUDE [functions-selector-bindings](../../includes/functions-selector-bindings.md)]

Dieser Artikel erläutert das Konfigurieren von und Arbeiten mit HTTP-Triggern und -Bindungen in Azure Functions.
Damit können Sie Azure Functions zum Erstellen von serverlosen APIs und zum Antworten auf Webhooks verwenden.

Azure Functions bietet die folgenden Bindungen:
- Mit einem [HTTP-Trigger](#httptrigger) können Sie eine Funktion mit einer HTTP-Anforderung aufrufen. Die Funktion kann so angepasst werden, dass sie auf [Webhooks](#hooktrigger) antwortet.
- Mit einer [HTTP-Ausgabebindung](#output) können Sie auf die Anforderung antworten.

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

<a name="httptrigger"></a>

## <a name="http-trigger"></a>HTTP-Trigger
Der HTTP-Trigger führt Ihre Funktion als Antwort auf eine HTTP-Anforderung aus. Die Funktion kann so angepasst werden, dass sie auf eine bestimmte URL oder einen HTTP-Methodensatz antwortet. Sie können auch einen HTTP-Trigger zum Antworten auf Webhooks konfigurieren. 

Wenn Sie das Functions-Portal verwenden, können Sie mit einer vorbereiteten Vorlage sofort loslegen. Wählen Sie **Neue Funktion** aus, und wählen Sie aus der Dropdownliste **Szenario** die Option „API + Webhooks“ aus. Wählen Sie eine der Vorlagen aus, und klicken Sie auf **Erstellen**.

Standardmäßig antwortet ein HTTP-Trigger auf die Anforderung mit dem Statuscode „HTTP 200 OK“ und leerem Haupttext. Um diese Antwort zu ändern, konfigurieren Sie eine [HTTP-Ausgabebindung](#output).

### <a name="configuring-an-http-trigger"></a>Konfigurieren eines HTTP-Triggers
Ein HTTP-Trigger wird definiert, indem ein JSON-Objekt ähnlich dem folgenden in das `bindings`-Array von „function.json“ eingefügt wird:

```json
{
    "name": "req",
    "type": "httpTrigger",
    "direction": "in",
    "authLevel": "function",
    "methods": [ "GET" ],
    "route": "values/{id}"
},
```
Die Bindung unterstützt die folgenden Eigenschaften:

* **name**: Erforderlich – der Variablenname, der im Funktionscode für die Anforderung oder den Anforderungstext verwendet wird. Siehe [Arbeiten mit einem HTTP-Trigger aus dem Code](#httptriggerusage).
* **type**: Erforderlich, muss auf „httpTrigger“ festgelegt sein.
* **direction**: Erforderlich, muss auf „in“ festgelegt sein.
* _authLevel_: Legt fest, welche Schlüssel (sofern welche benötigt werden) in der Anforderung vorhanden sein müssen, um die Funktion aufzurufen. Siehe [Arbeiten mit Schlüsseln](#keys) weiter unten. Der Wert kann einer der folgenden sein:
    * _anonymous_: Es ist kein API-Schlüssel erforderlich.
    * _function_: Es ist ein funktionsspezifischer API-Schlüssel erforderlich. Dies ist der Standardwert, wenn kein anderer Wert angegeben wird.
    * _admin_: Der Hauptschlüssel ist erforderlich.
* **methods**: Ein Array der HTTP-Methoden, denen die Funktion antwortet. Wenn kein Wert angegeben ist, antwortet die Funktion auf alle HTTP-Methoden. Siehe [Anpassen des HTTP-Endpunkts](#url).
* **route**: Definiert die Routenvorlage, mit der gesteuert wird, auf welche Anforderungs-URLs Ihre Funktion antwortet. Wenn kein anderer Wert angegeben wird, lautet der Standardwert `<functionname>`. Siehe [Anpassen des HTTP-Endpunkts](#url).
* **webHookType**: Konfiguriert den HTTP-Trigger so, dass dieser als Webhookempfänger für den angegebenen Anbieter fungiert. Wenn dieser Wert ausgewählt ist, sollte die _methods_-Eigenschaft nicht festgelegt werden. Siehe [Antworten auf Webhooks](#hooktrigger). Der Wert kann einer der folgenden sein:
    * _genericJson_: Ein Webhookendpunkt für allgemeine Zwecke ohne Logik für einen bestimmten Anbieter.
    * _github_: Die Funktion antwortet auf GitHub-Webhooks. Wenn dieser Wert ausgewählt ist, sollte die _authLevel_-Eigenschaft nicht festgelegt werden.
    * _slack_: Die Funktion antwortet auf Slack-Webhooks. Wenn dieser Wert ausgewählt ist, sollte die _authLevel_-Eigenschaft nicht festgelegt werden.

<a name="httptriggerusage"></a>
### <a name="working-with-an-http-trigger-from-code"></a>Arbeiten mit einem HTTP-Trigger aus dem Code
Bei C#- und F#-Funktionen können Sie den Typ der Triggereingabe als `HttpRequestMessage` oder als benutzerdefinierten Typ deklarieren. Wenn Sie `HttpRequestMessage` auswählen, erhalten Sie Vollzugriff auf das Anforderungsobjekt. Bei benutzerdefinierten Typen (z.B. POCO) versucht Functions, den Haupttext als JSON zu analysieren, um die Objekteigenschaften aufzufüllen.

Für Node.js-Funktionen stellt die Functions-Laufzeit den Anforderungstext anstelle des Anforderungsobjekts bereit.

Verwendungsbeispiele finden Sie unter [Beispiele für HTTP-Trigger](#httptriggersample).


<a name="output"></a>
## <a name="http-response-output-binding"></a>HTTP-Ausgabebindung bei Antwort
Verwenden Sie die HTTP-Ausgabebindung, um eine Antwort an den Absender der HTTP-Anforderung zu senden. Diese Bindung erfordert einen HTTP-Trigger und ermöglicht Ihnen, die Antwort, die der Anforderung des Triggers zugeordnet ist, benutzerdefiniert anzupassen. Wenn keine HTTP-Ausgabebindung angegeben wird, gibt ein HTTP-Trigger den Statuscode „HTTP 200 OK“ mit leerem Haupttext zurück. 

### <a name="configuring-an-http-output-binding"></a>Konfigurieren einer HTTP-Ausgabebindung
Die HTTP-Ausgabebindung wird definiert, indem ein JSON-Objekt ähnlich dem folgenden in das `bindings`-Array von „function.json“ eingefügt wird:

```json
{
    "name": "res",
    "type": "http",
    "direction": "out"
}
```
Die Bindung enthält die folgenden Eigenschaften:

* **name**: Erforderlich – der Variablenname, der im Funktionscode für die Antwort verwendet wird. Siehe [Arbeiten mit einer HTTP-Ausgabebindung aus dem Code](#outputusage).
* **type**: Erforderlich, muss auf „http“ festgelegt sein.
* **direction**: Erforderlich, muss auf „out“ festgelegt sein.

<a name="outputusage"></a>
### <a name="working-with-an-http-output-binding-from-code"></a>Arbeiten mit einer HTTP-Ausgabebindung aus dem Code
Sie können den Ausgabeparameter (z.B. „res“) für die Antwort an den HTTP- oder Webhookaufrufer verwenden. Alternativ dazu können Sie die standardmäßigen Muster `Request.CreateResponse()` (C#) oder `context.res` (Node.js) zur Rückgabe der Antwort verwenden. Beispiele für die letztgenannte Methode finden Sie unter [Beispiele für HTTP-Trigger](#httptriggersample) und [Beispiele für Webhooktrigger](#hooktriggersample).


<a name="hooktrigger"></a>
## <a name="responding-to-webhooks"></a>Antworten auf Webhooks
Ein HTTP-Trigger mit der Eigenschaft _webHookType_ wird so konfiguriert, dass er auf [Webhooks](https://en.wikipedia.org/wiki/Webhook) antwortet. Die grundlegende Konfiguration verwendet die Einstellung „genericJson“. Diese beschränkt Anforderungen auf solche, die HTTP POST verwenden und den Inhaltstyp `application/json` aufweisen.

Der Trigger kann automatisch an einen bestimmten Webhookanbieter angepasst werden (z.B. [GitHub](https://developer.github.com/webhooks/) und [Slack](https://api.slack.com/outgoing-webhooks)). Wenn ein Anbieter angegeben wurde, kümmert sich die Functions-Laufzeit um die Überprüfungslogik des Anbieters.  

### <a name="configuring-github-as-a-webhook-provider"></a>Konfigurieren von GitHub als Webhookanbieter
Um auf GitHub-Webhooks zu antworten, erstellen Sie zuerst eine Funktion mit einem HTTP-Trigger, und legen Sie die Eigenschaft _webHookType_ auf „github“ fest. Kopieren Sie dann die [URL](#url) und den [API-Schlüssel](#keys) auf die Seite **Webhook hinzufügen** Ihres GitHub-Repositorys. Weitere Informationen finden Sie in der GitHub-Dokumentation [Creating Webhooks](http://go.microsoft.com/fwlink/?LinkID=761099&clcid=0x409) (Erstellen von Webhooks).

![](./media/functions-bindings-http-webhook/github-add-webhook.png)

### <a name="configuring-slack-as-a-webhook-provider"></a>Konfigurieren von Slack als Webhookanbieter
Der Slack-Webhook lässt Sie das Token nicht angeben, sondern generiert es für Sie. Daher müssen Sie einen funktionsspezifischen Schlüssel mit dem Token aus Slack konfigurieren. Siehe [Arbeiten mit Schlüsseln](#keys).

<a name="url"></a>
## <a name="customizing-the-http-endpoint"></a>Anpassen des HTTP-Endpunkts
Wenn Sie eine Funktion für einen HTTP-Trigger oder einen Webhook erstellen, ist die Funktion mit einer Route der folgenden Form erreichbar:

    http://<yourapp>.azurewebsites.net/api/<funcname> 

Sie können diese Route mit der optionalen `route`-Eigenschaft in der Eingabebindung des HTTP-Triggers anpassen. In diesem Beispiel wird mit der Datei *function.json* eine `route`-Eigenschaft für einen HTTP-Trigger definiert:

```json
    {
      "bindings": [
        {
          "type": "httpTrigger",
          "name": "req",
          "direction": "in",
          "methods": [ "get" ],
          "route": "products/{category:alpha}/{id:int?}"
        },
        {
          "type": "http",
          "name": "res",
          "direction": "out"
        }
      ]
    }
```

Mit dieser Konfiguration ist die Funktion jetzt über die folgende Route erreichbar, anstatt über die ursprüngliche Route.

    http://<yourapp>.azurewebsites.net/api/products/electronics/357

So kann der Funktionscode zwei Parameter in der Adresse unterstützen: „category“ und „id“. Sie können für Ihre Parameter eine beliebige [Web-API-Routeneinschränkung](https://www.asp.net/web-api/overview/web-api-routing-and-actions/attribute-routing-in-web-api-2#constraints) verwenden. Im folgenden C#-Funktionscode werden beide Parameter verwendet.

```csharp
    public static Task<HttpResponseMessage> Run(HttpRequestMessage request, string category, int? id, 
                                                    TraceWriter log)
    {
        if (id == null)
           return  req.CreateResponse(HttpStatusCode.OK, $"All {category} items were requested.");
        else
           return  req.CreateResponse(HttpStatusCode.OK, $"{category} item with id = {id} has been requested.");
    }
```

Dies ist der Node.js-Funktionscode zur Verwendung der gleichen Routenparameter.

```javascript
    module.exports = function (context, req) {

        var category = context.bindingData.category;
        var id = context.bindingData.id;

        if (!id) {
            context.res = {
                // status: 200, /* Defaults to 200 */
                body: "All " + category + " items were requested."
            };
        }
        else {
            context.res = {
                // status: 200, /* Defaults to 200 */
                body: category + " item with id = " + id + " was requested."
            };
        }

        context.done();
    } 
```

Standardmäßig verfügen alle Funktionsrouten über das Präfix *api*. Sie können das Präfix auch mit der `http.routePrefix`-Eigenschaft in der Datei *host.json* anpassen oder entfernen. Im folgenden Beispiel wird das Routenpräfix *api* entfernt, indem in der Datei *host.json* eine leere Zeichenfolge als Präfix verwendet wird.

```json
    {
      "http": {
        "routePrefix": ""
      }
    }
```

Ausführliche Informationen zur Aktualisierung der Datei *host.json* für Ihre Funktion finden Sie unter [Aktualisieren von Funktionen-App-Dateien](functions-reference.md#fileupdate). 

Informationen zu anderen Eigenschaften, die Sie in der Datei *host.json* konfigurieren können, finden Sie in der [Referenz zu „host.json“](https://github.com/Azure/azure-webjobs-sdk-script/wiki/host.json).


<a name="keys"></a>
## <a name="working-with-keys"></a>Arbeiten mit Schlüsseln
HTTP-Trigger können Schlüssel nutzen, um für mehr Sicherheit zu sorgen. Ein standardmäßiger HTTP-Trigger kann diese als API-Schlüssel verwenden, sodass der Schlüssel in der Anforderung vorhanden sein muss. Webhooks können Schlüssel verwenden, um Anforderungen auf unterschiedliche Arten zu autorisieren, je nachdem, welche Methode vom Anbieter unterstützt wird.

Schlüssel werden als Teil Ihrer Funktionen-App in Azure gespeichert und sind im Ruhezustand verschlüsselt. Um vorhandene Schlüssel anzuzeigen, neue Schlüssel zu erstellen oder Schlüsseln neue Werte zuzuweisen, navigieren Sie im Portal zu einer Ihrer Funktion, und wählen Sie „Verwalten“ aus. 

Es gibt zwei Arten von Schlüsseln:
- **Administratorschlüssel**: Diese Schlüssel werden von allen Funktionen innerhalb der Funktionen-App gemeinsam genutzt. Bei Verwendung als API-Schlüssel ermöglichen diese Zugriff auf jede Funktion in der Funktionen-App.
- **Funktionsschlüssel**: Diese Schlüssel gelten nur für die Funktionen, für die sie definiert sind. Bei Verwendung als API-Schlüssel ermöglichen diese nur Zugriff auf diese spezielle Funktion.

Jeder Schlüssel ist zu Referenzzwecken benannt. Auf Funktions- und Administratorebene gibt es einen Standardschlüssel (mit dem Namen „default“). Der **Hauptschlüssel** ist ein Standardadministratorschlüssel namens „_master“, der für jede Funktionen-App definiert ist und nicht widerrufen werden kann. Er bietet administrativen Zugriff auf die Laufzeit-APIs. Bei Verwendung von `"authLevel": "admin"` in der Bindungs-JSON muss dieser Schlüssel in der Anforderung bereitgestellt werden. Jeder andere Schlüssel führt zu einem Autorisierungsfehler.

> [!NOTE]
> Aufgrund der erhöhten Berechtigungen, die der Hauptschlüssel gewährt, sollten Sie diesen Schlüssel nicht für Dritte freigeben oder in nativen Clientanwendungen verteilen. Gehen Sie sehr umsichtig vor, wenn Sie die Autorisierungsstufe „Administrator“ auswählen.
> 
> 

### <a name="api-key-authorization"></a>Autorisierung per API-Schlüssel
Ein HTTP-Trigger erfordert standardmäßig einen API-Schlüssel in der HTTP-Anforderung. Ihre HTTP-Anforderung sieht normalerweise folgendermaßen aus:

    https://<yourapp>.azurewebsites.net/api/<function>?code=<ApiKey>

Der Schlüssel kann in einer Abfragezeichenfolgenvariablen namens `code` (wie oben zu sehen) oder in einem `x-functions-key`-HTTP-Header enthalten sein. Der Wert des Schlüssels kann ein beliebiger für die Funktion definierter Funktionsschlüssel oder ein beliebiger Administratorschlüssel sein.

Sie können Anforderungen ohne Schlüssel zulassen oder festlegen, dass der Hauptschlüssel verwendet werden muss. Ändern Sie zu diesem Zweck die `authLevel`-Eigenschaft in der Bindungs-JSON (siehe [HTTP-Trigger](#httptrigger)).

### <a name="keys-and-webhooks"></a>Schlüssel und Webhooks
Die Webhookautorisierung wird von der Empfangskomponente für Webhooks verarbeitet, die zum HTTP-Trigger gehört. Der Mechanismus variiert je nach Webhooktyp. Für jeden Mechanismus wird allerdings ein Schlüssel benötigt. Standardmäßig wird der Funktionsschlüssel namens „default“ verwendet. Wenn Sie einen anderen Schlüssel verwenden möchten, müssen Sie den Webhookanbieter so konfigurieren, dass der Schlüsselname auf eine der folgenden Arten mit der Anforderung gesendet wird:

- **Abfragezeichenfolge**: Der Anbieter übergibt den Schlüsselnamen im `clientid`-Abfragezeichenfolgenparameter (z.B. `https://<yourapp>.azurewebsites.net/api/<funcname>?clientid=<keyname>`).
- **Anforderungsheader**: Der Anbieter übergibt den Schlüsselnamen im `x-functions-clientid`-Header.

> [!NOTE]
> Funktionsschlüssel haben Vorrang vor Administratorschlüsseln. Wenn zwei Schlüssel mit dem gleichen Namen definiert wurden, wird der Funktionsschlüssel verwendet.
> 
> 


<a name="httptriggersample"></a>
## <a name="http-trigger-samples"></a>Beispiele für HTTP-Trigger
Angenommen, der folgende HTTP-Trigger befindet sich im `bindings`-Array von „function.json“:

```json
{
    "name": "req",
    "type": "httpTrigger",
    "direction": "in",
    "authLevel": "function"
},
```

Informationen dazu finden Sie im sprachspezifischen Beispiel, das in der Abfragezeichenfolge oder im Text der HTTP-Anforderung nach einem `name`-Parameter sucht.

* [C#](#httptriggercsharp)
* [F#](#httptriggerfsharp)
* [Node.js](#httptriggernodejs)


<a name="httptriggercsharp"></a>
### <a name="http-trigger-sample-in-c"></a>Beispiel für HTTP-Trigger in C# #
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

<a name="httptriggerfsharp"></a>
### <a name="http-trigger-sample-in-f"></a>Beispiel für HTTP-Trigger in F# #
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

<a name="httptriggernodejs"></a>
### <a name="http-trigger-sample-in-nodejs"></a>Beispiel für HTTP-Trigger in Node.js
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



<a name="hooktriggersample"></a>
## <a name="webhook-samples"></a>Beispiele für Webhooks
Angenommen, der folgende Webhooktrigger befindet sich im `bindings`-Array von „function.json“:

```json
{
    "webHookType": "github",
    "name": "req",
    "type": "httpTrigger",
    "direction": "in",
},
```

Informationen dazu finden Sie im sprachspezifischen Beispiel, das Kommentare zu GitHub-Problemen protokolliert.

* [C#](#hooktriggercsharp)
* [F#](#hooktriggerfsharp)
* [Node.js](#hooktriggernodejs)

<a name="hooktriggercsharp"></a>

### <a name="webhook-sample-in-c"></a>Webhookbeispiel in C# #
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

<a name="hooktriggerfsharp"></a>

### <a name="webhook-sample-in-f"></a>Webhookbeispiel in F# #
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

<a name="hooktriggernodejs"></a>

### <a name="webhook-sample-in-nodejs"></a>Webhookbeispiel in Node.js
```javascript
module.exports = function (context, data) {
    context.log('GitHub WebHook triggered!', data.comment.body);
    context.res = { body: 'New GitHub comment: ' + data.comment.body };
    context.done();
};
```


## <a name="next-steps"></a>Nächste Schritte
[!INCLUDE [next steps](../../includes/functions-bindings-next-steps.md)]




<!--HONumber=Nov16_HO4-->


