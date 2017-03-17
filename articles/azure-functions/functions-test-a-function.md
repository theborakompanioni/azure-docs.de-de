---
title: Testen von Azure Functions | Microsoft Docs
description: Testen Sie Ihre Azure-Funktionen mithilfe von Postman, cURL und Node.js.
services: functions
documentationcenter: na
author: wesmc7777
manager: erikre
editor: 
tags: 
keywords: Azure Functions, Functions, Ereignisverarbeitung, Webhooks, dynamisches Compute, serverlose Architektur, Tests
ms.assetid: c00f3082-30d2-46b3-96ea-34faf2f15f77
ms.service: functions
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 02/02/2017
ms.author: wesmc
ms.custom: H1Hack27Feb2017
translationtype: Human Translation
ms.sourcegitcommit: 2fd12dd32ed3c8479c7460cbc0a1cac3330ff4f4
ms.openlocfilehash: a58bf41ec11b5826b60c1fc999240ea655be6d9d
ms.lasthandoff: 03/01/2017


---
# <a name="strategies-for-testing-your-code-in-azure-functions"></a>Strategien zum Testen Ihres Codes in Azure Functions

Dieses Thema veranschaulicht die verschiedenen Möglichkeiten zum Testen von Funktionen einschließlich Nutzung der folgenden allgemeinen Ansätze:

+ HTTP-basierte Tools, z.B. cURL, Postman und sogar einen Webbrowser für webbasierte Trigger
+ Azure-Speicher-Explorer zum Testen von Triggern, die auf Azure Storage basieren
+ Testregisterkarte im Azure Functions-Portal
+ Per Timer ausgelöste Funktion
+ Testen der Anwendung oder des Frameworks

Alle diese Testmethoden verwenden eine HTTP-Trigger-Funktion, die Eingaben entweder über einen Abfragezeichenfolgen-Parameter oder über den Anforderungstext akzeptiert. Sie erstellen diese Funktion im ersten Abschnitt.

## <a name="create-a-function-for-testing"></a>Erstellen einer Funktion zum Testen
In diesem Tutorial verwenden wir hauptsächlich eine leicht abgeänderte Version der Vorlage für die JavaScript-Funktion HttpTrigger, die beim Erstellen einer Funktion verfügbar ist. Wenn Sie Hilfe zum Erstellen einer Funktion benötigen, nutzen Sie dieses [Tutorial](functions-create-first-azure-function.md). Wählen Sie beim Erstellen der Testfunktion im [Azure-Portal] die **HttpTrigger- JavaScript**-Vorlage.

Die Standardvorlage für die Funktion ist im Grunde eine „Hello World“-Funktion, die den Namen aus dem Anforderungstext oder dem Abfragezeichenfolgen-Parameter `name=<your name>` zurückgibt.  Wir aktualisieren den Code, sodass Sie auch den Namen und eine Adresse als JSON-Inhalte im Anforderungstext bereitstellen können. Die Funktion gibt diese Informationen dann an den Client zurück, wenn sie verfügbar sind.   

Aktualisieren Sie die Funktion mit dem folgenden Code, der für Tests verwendet wird:

```javascript
module.exports = function (context, req) {
    context.log("HTTP trigger function processed a request. RequestUri=%s", req.originalUrl);
    context.log("Request Headers = " + JSON.stringify(req.headers));
    var res;

    if (req.query.name || (req.body && req.body.name)) {
        if (typeof req.query.name != "undefined") {
            context.log("Name was provided as a query string param...");
            res = ProcessNewUserInformation(context, req.query.name);
        }
        else {
            context.log("Processing user info from request body...");
            res = ProcessNewUserInformation(context, req.body.name, req.body.address);
        }
    }
    else {
        res = {
            status: 400,
            body: "Please pass a name on the query string or in the request body"
        };
    }
    context.done(null, res);
};
function ProcessNewUserInformation(context, name, address) {
    context.log("Processing user information...");
    context.log("name = " + name);
    var echoString = "Hello " + name;
    var res;

    if (typeof address != "undefined") {
        echoString += "\n" + "The address you provided is " + address;
        context.log("address = " + address);
    }
    res = {
        // status: 200, /* Defaults to 200 */
        body: echoString
    };
    return res;
}
```

## <a name="test-a-function-with-tools"></a>Testen einer Funktion mit Tools
Außerhalb des Azure-Portals stehen Ihnen verschiedene Tools zur Verfügung, die Sie zum Auslösen Ihrer Funktionen zum Testen verwenden können. Dazu gehören auf UI und auf Befehlszeilen basierende HTTP-Testtools, Azure-Speicherzugriffstools und sogar ein einfacher Webbrowser.

### <a name="test-with-a-browser"></a>Testen mit einem Browser
Der Webbrowser ist eine einfache Möglichkeit zum Auslösen von Funktionen über HTTP. Sie können einen Browser für die GET-Anforderungen verwenden, die keine Textnutzlast erfordern und nur Abfragezeichenfolgen-Parameter verwenden.

Kopieren Sie zum Testen der bereits definierten Funktion die **Funktions-URL** aus dem Portal. Sie hat folgendes Format:

    https://<Your Function App>.azurewebsites.net/api/<Your Function Name>?code=<your access code>

Fügen Sie den `name`-Parameter an die Abfragezeichenfolge. Verwenden Sie einen tatsächlichen Namen für den `<Enter a name here>`-Platzhalter.

    https://<Your Function App>.azurewebsites.net/api/<Your Function Name>?code=<your access code>&name=<Enter a name here>

Fügen Sie die URL in Ihren Browser ein, und Sie sollten eine Antwort ähnlich der folgenden erhalten.

![Screenshot der Chrome-Browserregisterkarte mit der Testantwort](./media/functions-test-a-function/browser-test.png)

In diesem Beispiel umschließt der Chrome-Browser die zurückgegebene Zeichenfolge in XML. Andere Browser zeigen einfach den Zeichenfolgenwert an.

Im Portalfenster **Protokolle** wird während der Ausführung der Funktion eine Ausgabe ähnlich der folgenden protokolliert:

    2016-03-23T07:34:59  Welcome, you are now connected to log-streaming service.
    2016-03-23T07:35:09.195 Function started (Id=61a8c5a9-5e44-4da0-909d-91d293f20445)
    2016-03-23T07:35:10.338 Node.js HTTP trigger function processed a request. RequestUri=https://functionsExample.azurewebsites.net/api/WesmcHttpTriggerNodeJS1?code=XXXXXXXXXX==&name=Glenn from a browser
    2016-03-23T07:35:10.338 Request Headers = {"cache-control":"max-age=0","connection":"Keep-Alive","accept":"text/html","accept-encoding":"gzip","accept-language":"en-US"}
    2016-03-23T07:35:10.338 Name was provided as a query string param.
    2016-03-23T07:35:10.338 Processing User Information...
    2016-03-23T07:35:10.369 Function completed (Success, Id=61a8c5a9-5e44-4da0-909d-91d293f20445)

### <a name="test-with-postman"></a>Testen mit Postman
Das empfohlene Tool zum Testen der meisten Funktionen ist Postman, das in den Chrome-Browser integriert ist. Informationen zum Installieren von Postman finden Sie unter [Get Postman (Abrufen von Postman)](https://www.getpostman.com/). Postman ermöglicht die Steuerung zahlreicher weiterer Attribute einer HTTP-Anforderung.

> [!TIP]
> Verwenden Sie das HTTP-Testtool, mit dem Sie am besten vertraut sind. Hier sind einige Alternativen zu Postman:  
>
> * [Fiddler](http://www.telerik.com/fiddler)  
> * [Paw](https://luckymarmot.com/paw)  
>
>

So testen die Funktion mit einem Anforderungstext in Postman:

1. Starten Sie Postman über die Schaltfläche **Apps** links oben in einem Chrome-Browserfenster.
2. Kopieren Sie Ihre **Funktions-URL**, und fügen Sie sie in Postman ein. Sie enthält den Abfragezeichenfolgen-Parameter für den Zugriffscode.
3. Ändern Sie die HTTP-Methode zu **POST**.
4. Klicken Sie auf **Text** > **unformatiert**, und fügen Sie einen JSON-Anforderungstext ähnlich dem folgenden hinzu:

    ```json
    {
        "name" : "Wes testing with Postman",
        "address" : "Seattle, WA 98101"
    }
    ```
5. Klicken Sie auf **Send**.

Die folgende Abbildung zeigt den Test der einfachen Rückgabefunktion in diesem Tutorial.

![Screenshot der Postman-Benutzeroberfläche](./media/functions-test-a-function/postman-test.png)

Im Portalfenster **Protokolle** wird während der Ausführung der Funktion eine Ausgabe ähnlich der folgenden protokolliert:

    2016-03-23T08:04:51  Welcome, you are now connected to log-streaming service.
    2016-03-23T08:04:57.107 Function started (Id=dc5db8b1-6f1c-4117-b5c4-f6b602d538f7)
    2016-03-23T08:04:57.763 HTTP trigger function processed a request. RequestUri=https://functions841def78.azurewebsites.net/api/WesmcHttpTriggerNodeJS1?code=XXXXXXXXXX==
    2016-03-23T08:04:57.763 Request Headers = {"cache-control":"no-cache","connection":"Keep-Alive","accept":"*/*","accept-encoding":"gzip","accept-language":"en-US"}
    2016-03-23T08:04:57.763 Processing user info from request body...
    2016-03-23T08:04:57.763 Processing User Information...
    2016-03-23T08:04:57.763 name = Wes testing with Postman
    2016-03-23T08:04:57.763 address = Seattle, W.A. 98101
    2016-03-23T08:04:57.795 Function completed (Success, Id=dc5db8b1-6f1c-4117-b5c4-f6b602d538f7)

### <a name="test-with-curl-from-the-command-line"></a>Testen mit cURL über die Befehlszeile
Beim Testen von Software muss häufig nur die Befehlszeile berücksichtigt werden, um Ihre Anwendung zu debuggen. Dies gilt auch für das Testen von Funktionen. Beachten Sie, dass cURL auf Linux-basierten Systemen standardmäßig verfügbar ist. Unter Windows müssen Sie das [cURL-Tool](https://curl.haxx.se/) erst herunterladen und installieren.

Kopieren Sie zum Testen der bereits definierten Funktion die **Funktions-URL** aus dem Portal. Sie hat folgendes Format:

    https://<Your Function App>.azurewebsites.net/api/<Your Function Name>?code=<your access code>

Dies ist die URL zum Auslösen Ihrer Funktion. Testen Sie dies, indem Sie mithilfe des cURL-Befehls in der Befehlszeile eine GET-Anforderung (`-G` oder `--get`) für die Funktion senden:

    curl -G https://<Your Function App>.azurewebsites.net/api/<Your Function Name>?code=<your access code>

Dieses spezielle Beispiel erfordert einen Abfragezeichenfolgen-Parameter, der dem cURL-Befehl in Form von Daten (`-d`) übergeben werden kann:

    curl -G https://<Your Function App>.azurewebsites.net/api/<Your Function Name>?code=<your access code> -d name=<Enter a name here>

Führen Sie den Befehl aus, und Sie erhalten die folgende Ausgabe der Funktion in der Befehlszeile:

![Screenshot der Ausgabe in der Befehlszeile](./media/functions-test-a-function/curl-test.png)

Im Portalfenster **Protokolle** wird während der Ausführung der Funktion eine Ausgabe ähnlich der folgenden protokolliert:

    2016-04-05T21:55:09  Welcome, you are now connected to log-streaming service.
    2016-04-05T21:55:30.738 Function started (Id=ae6955da-29db-401a-b706-482fcd1b8f7a)
    2016-04-05T21:55:30.738 Node.js HTTP trigger function processed a request. RequestUri=https://functionsExample.azurewebsites.net/api/HttpTriggerNodeJS1?code=XXXXXXX&name=Azure Functions
    2016-04-05T21:55:30.738 Function completed (Success, Id=ae6955da-29db-401a-b706-482fcd1b8f7a)

### <a name="test-a-blob-trigger-by-using-storage-explorer"></a>Testen eines Blobtriggers mit dem Speicher-Explorer
Sie können eine Blobtriggerfunktion mit dem [Azure-Speicher-Explorer](http://storageexplorer.com/) testen.

1. Erstellen Sie im [Azure-Portal] für Ihre Funktionen-App eine C#-, F#- oder JavaScript-Blobtriggerfunktion. Legen Sie den zu überwachenden Pfad auf den Namen Ihres Blobcontainers fest. Beispiel:

        files
2. Klicken Sie auf die Schaltfläche **+** , um das zu verwendende Speicherkonto auszuwählen oder zu erstellen. Klicken Sie dann auf **Erstellen**.
3. Erstellen und speichern Sie eine Textdatei mit folgendem Inhalt:

        A text file for blob trigger function testing.
4. Führen Sie den [Azure-Speicher-Explorer](http://storageexplorer.com/) aus, und stellen Sie eine Verbindung mit dem Blobcontainer im überwachten Speicherkonto her.
5. Klicken Sie auf **Hochladen**, um die Textdatei hochzuladen.

    ![Screenshot des Speicher-Explorers](./media/functions-test-a-function/azure-storage-explorer-test.png)

Der Standardcode der Blobtriggerfunktion meldet die Verarbeitung des Blobs in den Protokollen:

    2016-03-24T11:30:10  Welcome, you are now connected to log-streaming service.
    2016-03-24T11:30:34.472 Function started (Id=739ebc07-ff9e-4ec4-a444-e479cec2e460)
    2016-03-24T11:30:34.472 C# Blob trigger function processed: A text file for blob trigger function testing.
    2016-03-24T11:30:34.472 Function completed (Success, Id=739ebc07-ff9e-4ec4-a444-e479cec2e460)

## <a name="test-a-function-within-functions"></a>Testen einer Funktion innerhalb von Funktionen
Das Azure Functions-Portal ist so gestaltet, dass Sie per HTTP und per Timer ausgelöste Funktionen testen können. Sie können auch Funktionen zum Auslösen anderer Funktionen erstellen, die Sie testen.

### <a name="test-with-the-functions-portal-run-button"></a>Testen mit der Schaltfläche „Ausführen“ des Functions-Portals
Mit der Schaltfläche **Ausführen** des Portals können Sie einige eingeschränkte Tests durchführen. Sie können mit der Schaltfläche einen Anforderungstext bereitstellen, das Bereitstellen eines Abfragezeichenfolgen-Parameters oder das Aktualisieren von Anforderungsheadern sind jedoch nicht möglich.

Testen Sie die zuvor erstellte HTTP-Triggerfunktion durch das Hinzufügen einer ähnlichen JSON-Zeichenfolge wie der folgenden im Feld **Anforderungstext**. Klicken Sie dann auf die Schaltfläche **Ausführen**.

```json
{
    "name" : "Wes testing Run button",
    "address" : "USA"
}
```

Im Portalfenster **Protokolle** wird während der Ausführung der Funktion eine Ausgabe ähnlich der folgenden protokolliert:

    2016-03-23T08:03:12  Welcome, you are now connected to log-streaming service.
    2016-03-23T08:03:17.357 Function started (Id=753a01b0-45a8-4125-a030-3ad543a89409)
    2016-03-23T08:03:18.697 HTTP trigger function processed a request. RequestUri=https://functions841def78.azurewebsites.net/api/wesmchttptriggernodejs1
    2016-03-23T08:03:18.697 Request Headers = {"connection":"Keep-Alive","accept":"*/*","accept-encoding":"gzip","accept-language":"en-US"}
    2016-03-23T08:03:18.697 Processing user info from request body...
    2016-03-23T08:03:18.697 Processing User Information...
    2016-03-23T08:03:18.697 name = Wes testing Run button
    2016-03-23T08:03:18.697 address = USA
    2016-03-23T08:03:18.744 Function completed (Success, Id=753a01b0-45a8-4125-a030-3ad543a89409)


### <a name="test-with-a-timer-trigger"></a>Testen mit einem Trigger mit Timer
Einige Funktionen können mit den zuvor erwähnten Tools nicht angemessen getestet werden. Dies gilt beispielsweise für eine Warteschlangen-Triggerfunktion, die ausgeführt wird, wenn eine Nachricht im [Azure-Warteschlangenspeicher](../storage/storage-dotnet-how-to-use-queues.md) abgelegt wird. Sie können jederzeit Code schreiben, um eine Nachricht in der Warteschlange zu platzieren; ein Beispiel dafür in einem Konsolenprojekt wird später in diesem Artikel präsentiert. Zum direkten Testen von Funktionen ist jedoch ein anderer Ansatz möglich.  

Sie können einen Trigger mit Timer verwenden, der mit einer Warteschlangen-Ausgabenbindung konfiguriert ist. Der Code für den Trigger mit Timer kann die Testnachrichten dann in die Warteschlange schreiben. In diesem Abschnitt wird ein Beispiel beschrieben.

Ausführlichere Informationen zur Verwendung von Bindungen mit Azure Functions finden Sie in der [Entwicklerreferenz zu Azure Functions](functions-reference.md).

#### <a name="create-a-queue-trigger-for-testing"></a>Erstellen eines Warteschlangentriggers für den Test
Um diesen Ansatz zu veranschaulichen, erstellen wir zunächst eine Warteschlangen-Triggerfunktion, die für eine Warteschlange mit dem Namen `queue-newusers` getestet werden soll. Diese Funktion verarbeitet Informationen zu Name und Adresse für einen neuen Benutzer, die im Warteschlangenspeicher abgelegt werden.

> [!NOTE]
> Wenn Sie den Namen einer anderen Warteschlange verwenden, stellen Sie sicher, dass der angegebene Name den Regeln für das [Benennen von Warteschlangen und Metadaten](https://msdn.microsoft.com/library/dd179349.aspx) entspricht. Andernfalls erhalten Sie eine Fehlermeldung.
>
>

1. Klicken Sie im [Azure-Portal] für Ihre Funktionen-App auf **Neue Funktion** > **QueueTrigger - C#**.
2. Geben Sie den Namen der Warteschlange ein, die von der Warteschlangenfunktion überwacht werden soll:

        queue-newusers
3. Klicken Sie auf die Schaltfläche **+** , um das zu verwendende Speicherkonto auszuwählen oder zu erstellen. Klicken Sie dann auf **Erstellen**.
4. Lassen Sie dieses Portalbrowserfenster geöffnet, damit Sie die Protokolleinträge für den Standardvorlagencode der Warteschlangenfunktion überwachen können.

#### <a name="create-a-timer-trigger-to-drop-a-message-in-the-queue"></a>Erstellen eines Triggers mit Timer, um eine Nachricht in der Warteschlange abzulegen
1. Öffnen Sie das [Azure-Portal] in einem neuen Browserfenster, und navigieren Sie zu Ihrer Funktionen-App.
2. Klicken Sie auf **Neue Funktion** > **TimerTrigger – C#**. Geben Sie einen Cron-Ausdruck ein, um festzulegen, wie oft der Timercode Ihre Warteschlangenfunktion testet. Klicken Sie dann auf **Erstellen**. Wenn Sie möchten, dass der Test alle 30 Sekunden ausgeführt wird, können Sie den folgenden [CRON-Ausdruck](https://wikipedia.org/wiki/Cron#CRON_expression)verwenden:

        */30 * * * * *
3. Klicken Sie auf die Registerkarte **Integrieren** für Ihren neuen Trigger mit Timer.
4. Klicken Sie unter **Ausgabe** auf **+ Neue Ausgabe**. Klicken Sie dann auf **Warteschlange** und **Auswählen**.
5. Notieren Sie den Namen, den Sie für das **Warteschlangennachrichtenobjekt** verwenden. Sie verwenden ihn im Timerfunktionscode.

        myQueue
6. Geben Sie den Namen der Warteschlange ein, an die die Nachricht gesendet wird:

        queue-newusers
7. Klicken Sie auf die Schaltfläche **+**, um das Speicherkonto auszuwählen, das Sie zuvor mit dem Warteschlangentrigger verwendet haben. Klicken Sie anschließend auf **Save**.
8. Klicken Sie auf die Registerkarte **Entwickeln** für Ihren Trigger mit Timer.
9. Sie können den folgenden Code für die C#-Timerfunktion verwenden, sofern Sie den gleichen Warteschlangennachricht-Objektnamen wie bereits gezeigt verwendet haben. Klicken Sie anschließend auf **Save**.

    ```cs
    using System;

    public static void Run(TimerInfo myTimer, out String myQueue, TraceWriter log)
    {
        String newUser =
        "{\"name\":\"User testing from C# timer function\",\"address\":\"XYZ\"}";

        log.Verbose($"C# Timer trigger function executed at: {DateTime.Now}");   
        log.Verbose($"{newUser}");   

        myQueue = newUser;
    }
    ```

Die C#-Timerfunktion wird zu diesem Zeitpunkt alle 30 Sekunden ausgeführt, wenn Sie den Cron-Beispielausdruck verwendet haben. Das Protokoll für die Timerfunktion meldet jede Ausführung:

    2016-03-24T10:27:02  Welcome, you are now connected to log-streaming service.
    2016-03-24T10:27:30.004 Function started (Id=04061790-974f-4043-b851-48bd4ac424d1)
    2016-03-24T10:27:30.004 C# Timer trigger function executed at: 3/24/2016 10:27:30 AM
    2016-03-24T10:27:30.004 {"name":"User testing from C# timer function","address":"XYZ"}
    2016-03-24T10:27:30.004 Function completed (Success, Id=04061790-974f-4043-b851-48bd4ac424d1)

Im Browserfenster für die Warteschlangenfunktion wird die Verarbeitung der einzelnen Nachrichten angezeigt:

    2016-03-24T10:27:06  Welcome, you are now connected to log-streaming service.
    2016-03-24T10:27:30.607 Function started (Id=e304450c-ff48-44dc-ba2e-1df7209a9d22)
    2016-03-24T10:27:30.607 C# Queue trigger function processed: {"name":"User testing from C# timer function","address":"XYZ"}
    2016-03-24T10:27:30.607 Function completed (Success, Id=e304450c-ff48-44dc-ba2e-1df7209a9d22)

## <a name="test-a-function-with-code"></a>Testen einer Funktion mit Code
Vielleicht müssen Sie eine externe Anwendung oder ein Framework erstellen, um Ihre Funktionen zu testen.

### <a name="test-an-http-trigger-function-with-code-nodejs"></a>Testen einer HTTP-Triggerfunktion mit Code: Node.js
Sie können eine Node.js-App verwenden, um eine HTTP-Anforderung zum Testen Ihrer Funktion auszuführen.
Legen Sie fest:

* Den Host Ihrer Funktionen-App in den Anforderungsoptionen als `host`.
* Den Namen Ihrer Funktion im `path`
* Ihren Zugriffscode (`<your code>`) im `path`

Codebeispiel:

```javascript
var http = require("http");

var nameQueryString = "name=Wes%20Query%20String%20Test%20From%20Node.js";

var nameBodyJSON = {
    name : "Wes testing with Node.JS code",
    address : "Dallas, T.X. 75201"
};

var bodyString = JSON.stringify(nameBodyJSON);

var options = {
  host: "functions841def78.azurewebsites.net",
  //path: "/api/HttpTriggerNodeJS2?code=sc1wt62opn7k9buhrm8jpds4ikxvvj42m5ojdt0p91lz5jnhfr2c74ipoujyq26wab3wk5gkfbt9&" + nameQueryString,
  path: "/api/HttpTriggerNodeJS2?code=sc1wt62opn7k9buhrm8jpds4ikxvvj42m5ojdt0p91lz5jnhfr2c74ipoujyq26wab3wk5gkfbt9",
  method: "POST",
  headers : {
      "Content-Type":"application/json",
      "Content-Length": Buffer.byteLength(bodyString)
    }    
};

callback = function(response) {
  var str = ""
  response.on("data", function (chunk) {
    str += chunk;
  });

  response.on("end", function () {
    console.log(str);
  });
}

var req = http.request(options, callback);
console.log("*** Sending name and address in body ***");
console.log(bodyString);
req.end(bodyString);
```


Ausgabe:

    C:\Users\Wesley\testing\Node.js>node testHttpTriggerExample.js
    *** Sending name and address in body ***
    {"name" : "Wes testing with Node.JS code","address" : "Dallas, T.X. 75201"}
    Hello Wes testing with Node.JS code
    The address you provided is Dallas, T.X. 75201

Im Portalfenster **Protokolle** wird während der Ausführung der Funktion eine Ausgabe ähnlich der folgenden protokolliert:

    2016-03-23T08:08:55  Welcome, you are now connected to log-streaming service.
    2016-03-23T08:08:59.736 Function started (Id=607b891c-08a1-427f-910c-af64ae4f7f9c)
    2016-03-23T08:09:01.153 HTTP trigger function processed a request. RequestUri=http://functionsExample.azurewebsites.net/api/WesmcHttpTriggerNodeJS1/?code=XXXXXXXXXX==
    2016-03-23T08:09:01.153 Request Headers = {"connection":"Keep-Alive","host":"functionsExample.azurewebsites.net"}
    2016-03-23T08:09:01.153 Name not provided as query string param. Checking body...
    2016-03-23T08:09:01.153 Request Body Type = object
    2016-03-23T08:09:01.153 Request Body = [object Object]
    2016-03-23T08:09:01.153 Processing User Information...
    2016-03-23T08:09:01.215 Function completed (Success, Id=607b891c-08a1-427f-910c-af64ae4f7f9c)


### <a name="test-a-queue-trigger-function-with-code-c"></a>Testen einer Warteschlangentrigger-Funktion mit Code: C# #
Wie bereits erwähnt, können Sie einen Warteschlangentrigger testen, indem Sie mithilfe von Code eine Nachricht in ihrer Warteschlange ablegen. Der folgende Beispielcode basiert auf dem C#-Code im Tutorial [Erste Schritte mit Azure-Warteschlangenspeicher](../storage/storage-dotnet-how-to-use-queues.md) . Code für andere Sprachen ist ebenfalls über diesen Link verfügbar.

Zum Testen dieses Codes in einer Konsolen-App müssen Sie folgende Schritte durchführen:

* [Konfigurieren Sie die Speicherverbindungszeichenfolge in der Datei „app.config“](../storage/storage-dotnet-how-to-use-queues.md).
* Übergeben Sie einen `name` und eine `address` als Parameter an die App. Beispiel: `C:\myQueueConsoleApp\test.exe "Wes testing queues" "in a console app"`. (Dieser Code akzeptiert Name und Adresse für einen neuen Benutzer während der Laufzeit als Befehlszeilenargumente.)

C#-Beispielcode:

```cs
static void Main(string[] args)
{
    string name = null;
    string address = null;
    string queueName = "queue-newusers";
    string JSON = null;

    if (args.Length > 0)
    {
        name = args[0];
    }
    if (args.Length > 1)
    {
        address = args[1];
    }

    // Retrieve storage account from connection string
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(ConfigurationManager.AppSettings["StorageConnectionString"]);

    // Create the queue client
    CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();

    // Retrieve a reference to a queue
    CloudQueue queue = queueClient.GetQueueReference(queueName);

    // Create the queue if it doesn't already exist
    queue.CreateIfNotExists();

    // Create a message and add it to the queue.
    if (name != null)
    {
        if (address != null)
            JSON = String.Format("{{\"name\":\"{0}\",\"address\":\"{1}\"}}", name, address);
        else
            JSON = String.Format("{{\"name\":\"{0}\"}}", name);
    }

    Console.WriteLine("Adding message to " + queueName + "...");
    Console.WriteLine(JSON);

    CloudQueueMessage message = new CloudQueueMessage(JSON);
    queue.AddMessage(message);
}
```

Im Browserfenster für die Warteschlangenfunktion wird die Verarbeitung der einzelnen Nachrichten angezeigt:

    2016-03-24T10:27:06  Welcome, you are now connected to log-streaming service.
    2016-03-24T10:27:30.607 Function started (Id=e304450c-ff48-44dc-ba2e-1df7209a9d22)
    2016-03-24T10:27:30.607 C# Queue trigger function processed: {"name":"Wes testing queues","address":"in a console app"}
    2016-03-24T10:27:30.607 Function completed (Success, Id=e304450c-ff48-44dc-ba2e-1df7209a9d22)


<!-- URLs. -->

[Azure-Portal]: https://portal.azure.com

