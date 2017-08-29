---
title: JavaScript-Entwicklerreferenz zu Azure Functions | Microsoft-Dokumentation
description: "Erfahren Sie, wie Sie mithilfe von JavaScript Funktionen entwickeln können."
services: functions
documentationcenter: na
author: christopheranderson
manager: cfowler
editor: 
tags: 
keywords: Azure Functions, Functions, Ereignisverarbeitung, Webhooks, dynamisches Compute, serverlose Architektur
ms.assetid: 45dedd78-3ff9-411f-bb4b-16d29a11384c
ms.service: functions
ms.devlang: nodejs
ms.topic: reference
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 05/25/2017
ms.author: glenga
ms.translationtype: HT
ms.sourcegitcommit: 74b75232b4b1c14dbb81151cdab5856a1e4da28c
ms.openlocfilehash: 641afd78aae145c5e1b16a08567a22c1aafe59a8
ms.contentlocale: de-de
ms.lasthandoff: 07/26/2017

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

Bindungen von `direction === "in"` werden als Funktionsargumente übergeben, sodass Sie mit [`arguments`](https://msdn.microsoft.com/library/87dw3w1k.aspx) neue Eingaben dynamisch verarbeiten können (etwa durch Verwendung von `arguments.length` zum vollständigen Durchlaufen Ihrer Eingaben). Diese Funktion ist praktisch, wenn Sie nur einen Trigger ohne weitere Eingaben verwenden, da Sie zuverlässig auf Ihre Triggerdaten zugreifen können, ohne auf Ihr `context`-Objekt zu verweisen.

Die Argumente werden in der Reihenfolge an die Funktion übergeben, in der sie in *function.json* vorhanden sind. Dies gilt auch, wenn Sie sie in der Exportanweisung nicht angeben. Wenn Sie also beispielsweise `function(context, a, b)` in `function(context, a)` ändern, können Sie dennoch den Wert von `b` im Funktionscode abrufen, indem Sie auf `arguments[3]` verweisen.

Alle Bindungen werden unabhängig von der Richtung auch mit dem `context`-Objekt übergeben (siehe folgendes Skript). 

## <a name="context-object"></a>context-Objekt
Die Laufzeit verwendet ein `context`-Objekt, um Daten an Ihre und von Ihrer Funktion zu übergeben und Ihnen die Kommunikation mit der Laufzeit zu ermöglichen.

Das context-Objekt ist immer der erste Parameter in einer Funktion und muss immer angegeben werden, da es Methoden wie `context.done` und `context.log` enthält, die für die ordnungsgemäße Verwendung der Laufzeit benötigt werden. Sie können dem Objekt einen beliebigen Namen geben (also etwa `ctx` oder `c`).

```javascript
// You must include a context, but other arguments are optional
module.exports = function(context) {
    // function logic goes here :)
};
```

### <a name="contextbindings-property"></a>context.bindings-Eigenschaft

```
context.bindings
```
Gibt ein benanntes Objekt zurück, das alle Eingabe- und Ausgabedaten enthält. Wenn also beispielsweise in *function.json* die folgende Bindungsdefinition vorliegt, können Sie über das `context.bindings.myInput`-Objekt auf den Inhalt der Warteschlange zugreifen. 

```json
{
    "type":"queue",
    "direction":"in",
    "name":"myInput"
    ...
}
```

```javascript
// myInput contains the input data, which may have properties such as "name"
var author = context.bindings.myInput.name;
// Similarly, you can set your output data
context.bindings.myOutput = { 
        some_text: 'hello world', 
        a_number: 1 };
```

### <a name="contextdone-method"></a>context.Done-Methode
```
context.done([err],[propertyBag])
```

Teilt der Laufzeit mit, dass Ihr Code beendet ist. Sie müssen `context.done` aufrufen, da die Laufzeit sonst nie erfährt, dass Ihre Funktion abgeschlossen ist, und die Ausführung durch einen Timeout beendet wird. 

Mit der `context.done`-Methode können Sie einen benutzerdefinierten Fehler sowie einen Eigenschaftenbehälter mit Eigenschaften, die die Eigenschaften des `context.bindings`-Objekts überschreiben, an die Laufzeit zurückgeben.

```javascript
// Even though we set myOutput to have:
//  -> text: hello world, number: 123
context.bindings.myOutput = { text: 'hello world', number: 123 };
// If we pass an object to the done function...
context.done(null, { myOutput: { text: 'hello there, world', noNumber: true }});
// the done method will overwrite the myOutput binding to be: 
//  -> text: hello there, world, noNumber: true
```

### <a name="contextlog-method"></a>context.log-Methode  

```
context.log(message)
```
Ermöglicht das Schreiben in die Streamingkonsolenprotokolle auf Standard-Ablaufverfolgungsebene. Es sind zusätzliche Protokollierungsmethoden in `context.log` verfügbar, mit denen Sie auf anderen Ablaufverfolgungsebenen in das Konsolenprotokoll schreiben können:


| Methode                 | Beschreibung                                |
| ---------------------- | ------------------------------------------ |
| **Fehler(_Meldung_)**   | Schreibt in Protokollierung auf Fehlerebene oder niedriger.   |
| **warn(_Meldung_)**    | Schreibt in Protokollierung auf Warnungsebene oder niedriger. |
| **info(_Meldung_)**    | Schreibt in Protokollierung auf Informationsebene oder niedriger.    |
| **verbose(_Meldung_)** | Schreibt in Protokollierung auf ausführlicher Ebene.           |

Das folgende Beispiel schreibt auf Warnungs-Ablaufverfolgungsebene in die Konsole:

```javascript
context.log.warn("Something has happened."); 
```
Sie können den Ablaufverfolgungsebenen-Schwellenwert für die Protokollierung in der Datei „host.json“ festlegen oder ihn deaktivieren.  Weitere Informationen zum Schreiben von Protokollen finden Sie im nächsten Abschnitt.

## <a name="binding-data-type"></a>Datentyp für Bindungen

Verwenden Sie zum Definieren des Datentyps für eine Eingabebindung die `dataType`-Eigenschaft in der Bindungsdefinition. Um z.B. den Inhalt einer HTTP-Anforderung im Binärformat zu lesen, verwenden Sie den Typ `binary`:

```json
{
    "type": "httpTrigger",
    "name": "req",
    "direction": "in",
    "dataType": "binary"
}
```

Andere Optionen für `dataType` sind `stream` und `string`.

## <a name="writing-trace-output-to-the-console"></a>Schreiben der Ablaufverfolgungsausgabe in die Konsole 

In Functions verwenden Sie die `context.log`-Methoden, um die Ablaufverfolgungsausgabe in die Konsole zu schreiben. An diesem Punkt können Sie `console.log` nicht verwenden, um in die Konsole zu schreiben.

Beim Aufruf von `context.log()` wird Ihre Meldung auf der Standard-Ablaufverfolgungsebene, also der _info_-Ablaufverfolgungsebene, in die Konsole geschrieben. Das folgende Beispiel schreibt auf der „info“-Ablaufverfolgungsebene in die Konsole:

```javascript
context.log({hello: 'world'});  
```

Der obige Code ist gleichwertig mit folgendem Code:

```javascript
context.log.info({hello: 'world'});  
```

Der folgende Code schreibt auf der „error“-Ebene in die Konsole:

```javascript
context.log.error("An error has occurred.");  
```

Da _error_ die höchste Ablaufverfolgungsebene ist, wird diese Ablaufverfolgung auf allen Ablaufverfolgungsebenen in die Ausgabe geschrieben, solange die Protokollierung aktiviert ist.  


Alle `context.log`-Methoden unterstützen das gleiche Parameterformat, das auch von der [util.format](https://nodejs.org/api/util.html#util_util_format_format)-Methode in Node.js unterstützt wird. Beachten Sie den folgenden Code, der auf der standardmäßigen Ablaufverfolgungsebene in die Konsole schreibt:

```javascript
context.log('Node.js HTTP trigger function processed a request. RequestUri=' + req.originalUrl);
context.log('Request Headers = ' + JSON.stringify(req.headers));
```

Sie können den gleichen Code auch im folgenden Format schreiben:

```javascript
context.log('Node.js HTTP trigger function processed a request. RequestUri=%s', req.originalUrl);
context.log('Request Headers = ', JSON.stringify(req.headers));
```

### <a name="configure-the-trace-level-for-console-logging"></a>Konfigurieren der Ablaufverfolgungsebene für die Konsolenprotokollierung

Mit Functions können Sie den Ablaufverfolgungsebenen-Schwellenwert zum Schreiben in die Konsole definieren, sodass Sie mühelos mit Ihren Funktionen steuern können, wie Ablaufverfolgungen in die Konsole geschrieben werden. Legen Sie mit der `tracing.consoleLevel`-Eigenschaft in der Datei „host.json“ den Schwellenwert für alle Ablaufverfolgungen fest, die in die Konsole geschrieben werden. Diese Einstellung gilt für alle Funktionen in Ihrer Funktionen-App. Im folgenden Beispiel wird der Schwellenwert für die Ablaufverfolgung festgelegt, um die ausführliche Protokollierung zu aktivieren:

```json
{ 
    "tracing": {      
        "consoleLevel": "verbose"     
    }
}  
```

Die Werte von **consoleLevel** entsprechen den Namen der `context.log`-Methoden. Um die gesamte Ablaufverfolgungsprotokollierung in der Konsole zu deaktivieren, setzen Sie **consoleLevel** auf _off_. Weitere Informationen über die host.json-Datei finden Sie im [host.json-Referenzthema](https://github.com/Azure/azure-webjobs-sdk-script/wiki/host.json).

## <a name="http-triggers-and-bindings"></a>HTTP: Trigger und Bindungen

HTTP- und Webhooktrigger und HTTP-Ausgabebindungen verwenden Request- und Response-Objekte, um das HTTP-Messaging darzustellen.  

### <a name="request-object"></a>Anforderungsobjekt

Das `request`-Objekt weist die folgenden Eigenschaften auf:

| Eigenschaft      | Beschreibung                                                    |
| ------------- | -------------------------------------------------------------- |
| _body_        | Ein Objekt, das den Hauptteil der Anforderung enthält.               |
| _headers_     | Ein Objekt, das die Header der Anforderung enthält.                   |
| _method_      | Die HTTP-Methode der Anforderung.                                |
| _originalUrl_ | Die URL der Anforderung.                                        |
| _params_      | Ein Objekt, das die Routingparameter der Anforderung enthält. |
| _query_       | Ein Objekt, das die Abfrageparameter enthält.                  |
| _rawBody_     | Der Hauptteil der Meldung als Zeichenfolge.                           |


### <a name="response-object"></a>Antwortobjekt

Das `response`-Objekt weist die folgenden Eigenschaften auf:

| Eigenschaft  | Beschreibung                                               |
| --------- | --------------------------------------------------------- |
| _body_    | Ein Objekt, das den Hauptteil der Antwort enthält.         |
| _headers_ | Ein Objekt, das die Header der Antwort enthält.             |
| _isRaw_   | Gibt an, dass die Formatierung für die Antwort übersprungen wird.    |
| _status_  | Der HTTP-Statuscode der Antwort.                     |

### <a name="accessing-the-request-and-response"></a>Zugreifen auf Anforderung und Antwort 

Bei der Arbeit mit HTTP-Triggern haben Sie drei Möglichkeiten, auf HTTP-Anforderungsobjekt und -Antwortobjekt zuzugreifen:

+ Über die benannten Eingabe- und Ausgabebindungen. Hierbei funktionieren die HTTP-Trigger und -Bindungen genauso wie jede andere Bindung. Im folgenden Beispiel wird das Antwortobjekt mit einer als `response` benannten Bindung festgelegt: 

    ```javascript
    context.bindings.response = { status: 201, body: "Insert succeeded." };
    ```

+ Über die `req`- und `res`-Eigenschaft des `context`-Objekts. Auf diese Weise können Sie die herkömmlichen Muster für den Zugriff auf HTTP-Daten über das context-Objekt verwenden, anstatt das gesamte `context.bindings.name`-Muster verwenden zu müssen. Das folgende Beispiel veranschaulicht den Zugriff auf das `req`- und `res`-Objekt des `context`-Objekts:

    ```javascript
    // You can access your http request off the context ...
    if(context.req.body.emoji === ':pizza:') context.log('Yay!');
    // and also set your http response
    context.res = { status: 202, body: 'You successfully ordered more coffee!' }; 
    ```

+ Durch Aufrufen von `context.done()`. Eine besondere Art der HTTP-Bindung gibt die Antwort zurück, die der `context.done()`-Methode übergeben wird. Die folgende HTTP-Ausgabebindung definiert einen `$return`-Ausgabeparameter:

    ```json
    {
      "type": "http",
      "direction": "out",
      "name": "$return"
    }
    ``` 
    Diese Ausgabebindung erwartet, dass Sie die Antwort angeben, wenn Sie `done()` aufrufen, wie folgt:

    ```javascript
     // Define a valid response object.
    res = { status: 201, body: "Insert succeeded." };
    context.done(null, res);   
    ```  

## <a name="node-version-and-package-management"></a>Node-Version und Paketverwaltung
Die Node-Version ist derzeit unveränderlich auf `6.5.0`festgelegt. Wir untersuchen die Option, Unterstützung für weitere Versionen sowie Konfigurationsmöglichkeiten hinzuzufügen.

Mit den folgenden Schritten können Sie Pakete in Ihre Funktionen-App einbeziehen: 

1. Wechseln Sie zur Adresse `https://<function_app_name>.scm.azurewebsites.net`.

2. Klicken Sie auf **Debugkonsole** > **CMD**.

3. Gehen Sie zu `D:\home\site\wwwroot`, und ziehen Sie dann die Datei „package.json“ auf den **wwwroot**-Ordner in der oberen Hälfte der Seite.  
    Es gibt auch andere Möglichkeiten, Dateien in Ihre Funktionen-App hochzuladen. Weitere Informationen finden Sie unter [Aktualisieren von Funktionen-App-Dateien](functions-reference.md#fileupdate). 

4. Sobald die Datei „package.json“ hochgeladen ist, führen Sie den `npm install`-Befehl in der **Kudu-Remoteausführungskonsole** aus.  
    Mit dieser Aktion werden die in der Datei „package.json“ angegebenen Pakete heruntergeladen und die Funktionen-App neu gestartet.

Nach der Installation der benötigten Pakete importieren Sie sie durch Aufrufen von `require('packagename')` in Ihre Funktion, wie im folgenden Beispiel gezeigt:

```javascript
// Import the underscore.js library
var _ = require('underscore');
var version = process.version; // version === 'v6.5.0'

module.exports = function(context) {
    // Using our imported underscore.js library
    var matched_names = _
        .where(context.bindings.myInput.names, {first: 'Carla'});
```

Sie sollten eine `package.json`-Datei im Stammverzeichnis Ihrer Funktionen-App definieren. Wenn Sie die Datei definieren, nutzen alle Funktionen in der App gemeinsam die gleichen zwischengespeicherten Pakete, was die Leistung optimiert. Wenn ein Versionskonflikt auftritt, können Sie ihn beheben, indem Sie eine `package.json`-Datei im Ordner einer bestimmten Funktion hinzufügen.  

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
## <a name="considerations-for-javascript-functions"></a>Überlegungen zu JavaScript-Funktionen

Beachten Sie bei der Arbeit mit JavaScript-Funktionen die Überlegungen in den folgenden beiden Abschnitten.

### <a name="choose-single-core-app-service-plans"></a>Auswählen von Einzelkern-App Service-Plänen

Wenn Sie eine Funktionen-App erstellen, die einen App Service-Plan verwendet, sollten Sie statt eines Plans mit mehreren Kernen einen Einzelkernplan auswählen. Derzeit führt Functions JavaScript-Funktionen auf Einzelkern-VMs effizienter aus; die Verwendung größerer VMs führt nicht zu den erwarteten Leistungsverbesserungen. Bei Bedarf können Sie manuell horizontal hochskalieren, indem Sie weitere Instanzen von virtuellen Computern mit einem Kern hinzufügen, oder Sie können die automatische Skalierung aktivieren. Weitere Informationen finden Sie unter [Manuelles oder automatisches Skalieren der Instanzenzahl](../monitoring-and-diagnostics/insights-how-to-scale.md?toc=%2fazure%2fapp-service-web%2ftoc.json).    

### <a name="typescript-and-coffeescript-support"></a>TypeScript- und CoffeeScript-Unterstützung
Weil es noch keine direkte Unterstützung für die automatische Kompilierung von TypeScript bzw. CoffeeScript über die Laufzeit gibt, muss eine solche Unterstützung außerhalb der Laufzeit zum Zeitpunkt der Bereitstellung geschehen. 

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen finden Sie in den folgenden Ressourcen:

* [Bewährte Methoden für Azure Functions](functions-best-practices.md)
* [Azure Functions developer reference (Azure Functions-Entwicklerreferenz) (Azure Functions-Entwicklerreferenz)](functions-reference.md)
* [C#-Entwicklerreferenz zu Azure Functions](functions-reference-csharp.md)
* [F#-Entwicklerreferenz zu Azure Functions](functions-reference-fsharp.md)
* [Trigger und Bindungen in Azure Functions](functions-triggers-bindings.md)


