---
title: JavaScript-Entwicklerreferenz zu Azure Functions | Microsoft-Dokumentation
description: Erfahren Sie, wie Azure Functions mithilfe von JavaScript entwickelt wird.
services: functions
documentationcenter: na
author: christopheranderson
manager: erikre
editor: 
tags: 
keywords: Azure Functions, Functions, Ereignisverarbeitung, Webhooks, dynamisches Compute, serverlose Architektur
ms.assetid: 45dedd78-3ff9-411f-bb4b-16d29a11384c
ms.service: functions
ms.devlang: nodejs
ms.topic: reference
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 05/13/2016
ms.author: chrande, glenga
translationtype: Human Translation
ms.sourcegitcommit: e660f3313ba6805356eef30b3a17ac609fc9043e
ms.openlocfilehash: 880fdbd6fc51f16add2f2497402d8b01047d0847


---
# <a name="azure-functions-javascript-developer-guide"></a>JavaScript-Entwicklerhandbuch für Azure Functions
> [!div class="op_single_selector"]
> * [C#-Skript](functions-reference-csharp.md)
> * [F#-Skript](functions-reference-fsharp.md)
> * [JavaScript](functions-reference-node.md)
> 
> 

Mit der JavaScript-Benutzeroberfläche für Azure Functions können Sie ganz einfach eine Funktion exportieren, der ein `context`-Objekt für die Kommunikation mit der Laufzeit sowie für das Empfangen und Senden von Daten über Bindungen übergeben wird.

In diesem Artikel wird davon ausgegangen, dass Sie bereits die [Entwicklerreferenz zu Azure Functions](functions-reference.md)gelesen haben.

## <a name="exporting-a-function"></a>Exportieren einer Funktion
Alle JavaScript-Funktionen müssen über `module.exports` ein einzelnes `function`-Element exportieren, damit die Laufzeit die Funktion finden und ausführen kann. Diese Funktion muss immer ein `context` -Objekt enthalten.

```javascript
// You must include a context, but other arguments are optional
module.exports = function(context) {
    // Additional inputs can be accessed by the arguments property
    if(arguments.length === 4) {
        context.log('This function has 4 inputs');
    }
};
// or you can include additional inputs in your arguments
module.exports = function(context, myTrigger, myInput, myOtherInput) {
    // function logic goes here :)
};
```

Bindungen von `direction === "in"` werden als Funktionsargumente übergeben, sodass Sie mit [`arguments`](https://msdn.microsoft.com/library/87dw3w1k.aspx) neue Eingaben dynamisch verarbeiten können (etwa durch Verwendung von `arguments.length` zum vollständigen Durchlaufen Ihrer Eingaben). Diese Funktion ist sehr praktisch, wenn Sie nur einen Trigger ohne weitere Eingaben verwenden, da Sie zuverlässig auf Ihre Triggerdaten zugreifen können, ohne auf Ihr `context` -Objekt zu verweisen.

Die Argumente werden in der Reihenfolge an die Funktion übergeben, in der sie in *function.json* vorhanden sind. Dies gilt auch, wenn Sie sie in der Exportanweisung nicht angeben. Wenn Sie also beispielsweise `function(context, a, b)` in `function(context, a)` ändern, können Sie dennoch den Wert von `b` im Funktionscode abrufen, indem Sie auf `arguments[3]` verweisen.

Alle Bindungen werden unabhängig von der Richtung auch mit dem `context` -Objekt übergeben (wie weiter unten zu sehen). 

## <a name="context-object"></a>context-Objekt
Die Laufzeit verwendet ein `context`-Objekt, um Daten an Ihre und von Ihrer Funktion zu übergeben und Ihnen die Kommunikation mit der Laufzeit zu ermöglichen.

Das context-Objekt ist immer der erste Parameter in einer Funktion und muss immer angegeben werden, da es Methoden wie `context.done` und `context.log` enthält, die für die ordnungsgemäße Verwendung der Laufzeit benötigt werden. Sie können dem Objekt einen beliebigen Namen geben (also etwa `ctx` oder `c`).

```javascript
// You must include a context, but other arguments are optional
module.exports = function(context) {
    // function logic goes here :)
};
```

## <a name="contextbindings"></a>context.bindings
Das `context.bindings` -Objekt erfasst alle Eingabe- und Ausgabedaten. Die Daten werden dem `context.bindings`-Objekt über die `name`-Eigenschaft der Bindung hinzugefügt. Wenn also beispielsweise in *function.json* die folgende Bindungsdefinition vorliegt, können Sie über `context.bindings.myInput` auf den Inhalt der Warteschlange zugreifen. 

```json
{
    "type":"queue",
    "direction":"in",
    "name":"myInput"
    ...
}
```

```javascript
// myInput contains the input data which may have properties such as "name"
var author = context.bindings.myInput.name;
// Similarly, you can set your output data
context.bindings.myOutput = { 
        some_text: 'hello world', 
        a_number: 1 };
```

## `context.done([err],[propertyBag])`
Die `context.done`-Funktion gibt gegenüber der Laufzeit an, dass die Ausführung beendet ist. Sie sollte unbedingt aufgerufen werden, wenn Sie mit der Funktion fertig sind. Andernfalls ist der Laufzeit nicht bekannt, dass Ihre Funktion abgeschlossen wurde. 

Mit der `context.done`-Funktion können Sie an die Laufzeit einen benutzerdefinierten Fehler sowie einen Eigenschaftenbehälter mit Eigenschaften zurückgeben, die die Eigenschaften des `context.bindings`-Objekts überschreiben.

```javascript
// Even though we set myOutput to have:
//  -> text: hello world, number: 123
context.bindings.myOutput = { text: 'hello world', number: 123 };
// If we pass an object to the done function...
context.done(null, { myOutput: { text: 'hello there, world', noNumber: true }});
// the done method will overwrite the myOutput binding to be: 
//  -> text: hello there, world, noNumber: true
```

## <a name="contextlogmessage"></a>context.log(message)
Mit der `context.log` -Methode können Sie Protokollanweisungen ausgeben, die für die Protokollierung korreliert werden. Bei Verwendung von `console.log`werden Ihre Nachrichten nur für die Protokollierung auf Prozessebene angezeigt, was nicht besonders nützlich ist.

```javascript
/* You can use context.log to log output specific to this 
function. You can access your bindings via context.bindings */
context.log({hello: 'world'}); // logs: { 'hello': 'world' } 
```

Die `context.log` -Methode unterstützt das gleiche Parameterformat, das auch von der Node-Methode [util.format](https://nodejs.org/api/util.html#util_util_format_format) unterstützt wird. Beispielsweise kann folgender Code:

```javascript
context.log('Node.js HTTP trigger function processed a request. RequestUri=' + req.originalUrl);
context.log('Request Headers = ' + JSON.stringify(req.headers));
```

wie folgt geschrieben werden:

```javascript
context.log('Node.js HTTP trigger function processed a request. RequestUri=%s', req.originalUrl);
context.log('Request Headers = ', JSON.stringify(req.headers));
```

## <a name="http-triggers-contextreq-and-contextres"></a>HTTP-Trigger: context.req und context.res
Da es im Fall von HTTP-Triggern gängige Praxis ist, `req` und `res` für die HTTP-Anforderungs- und Antwortobjekte zu verwenden, haben wir uns entschieden, den Zugriff darauf im context-Objekt zu vereinfachen, anstatt die Verwendung des vollständigen `context.bindings.name`-Musters erforderlich zu machen.

```javascript
// You can access your http request off of the context ...
if(context.req.body.emoji === ':pizza:') context.log('Yay!');
// and also set your http response
context.res = { status: 202, body: 'You successfully ordered more coffee!' };   
```

## <a name="node-version--package-management"></a>Node-Version und Paketverwaltung
Die Node-Version ist derzeit unveränderlich auf `6.5.0`festgelegt. Wir untersuchen die Option, Unterstützung für weitere Versionen sowie Konfigurationsmöglichkeiten hinzuzufügen.

Sie können Pakete in Ihre Funktion einschließen, indem Sie die Datei *package.json* in den Ordner Ihrer Funktion im Dateisystem der Funktionen-App hochladen. Anweisungen zum Hochladen von Dateien finden Sie im Abschnitt **Aktualisieren von Funktionen-App-Dateien** der [Referenz für Azure Functions-Entwickler](functions-reference.md#fileupdate). 

Sie können auch `npm install` an der SCM-Befehlszeilenschnittstelle (Kudu) der Funktionen-App verwenden:

1. Navigieren Sie zu `https://<function_app_name>.scm.azurewebsites.net`.
2. Klicken Sie auf **Debugkonsole > CMD**.
3. Navigieren Sie zu `D:\home\site\wwwroot\<function_name>`.
4. Führen Sie `npm install`aus.

Nach der Installation der benötigten Pakete können Sie Ihre Funktion wie gewohnt (also über `require('packagename')`) importieren.

```javascript
// Import the underscore.js library
var _ = require('underscore');
var version = process.version; // version === 'v6.5.0'

module.exports = function(context) {
    // Using our imported underscore.js library
    var matched_names = _
        .where(context.bindings.myInput.names, {first: 'Carla'});
```

## <a name="environment-variables"></a>Umgebungsvariablen
Verwenden Sie `process.env`zum Abrufen einer Umgebungsvariablen oder zum Abrufen des Werts einer App-Einstellung, wie im folgenden Codebeispiel zu sehen:

```javascript
module.exports = function (context, myTimer) {
    var timeStamp = new Date().toISOString();

    context.log('Node.js timer trigger function ran!', timeStamp);   
    context.log(GetEnvironmentVariable("AzureWebJobsStorage"));
    context.log(GetEnvironmentVariable("WEBSITE_SITE_NAME"));

    context.done();
};

function GetEnvironmentVariable(name)
{
    return name + ": " + process.env[name];
}
```

## <a name="typescriptcoffeescript-support"></a>Unterstützung für TypeScript/CoffeeScript
Noch gibt es keine direkte Unterstützung für die automatische Kompilierung von TypeScript/CoffeeScript über die Laufzeit. All dies müsste also außerhalb der Laufzeit zum Zeitpunkt der Bereitstellung geschehen. 

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen finden Sie in den folgenden Ressourcen:

* [Bewährte Methoden für Azure Functions](functions-best-practices.md)
* [Azure Functions developer reference (Azure Functions-Entwicklerreferenz) (Azure Functions-Entwicklerreferenz)](functions-reference.md)
* [C#-Entwicklerreferenz zu Azure Functions](functions-reference-csharp.md)
* [F#-Entwicklerreferenz zu Azure Functions](functions-reference-fsharp.md)
* [Trigger und Bindungen in Azure Functions](functions-triggers-bindings.md)




<!--HONumber=Feb17_HO1-->


