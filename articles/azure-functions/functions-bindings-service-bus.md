<properties
	pageTitle="Service Bus-Trigger und -Bindungen in Azure Functions | Microsoft Azure"
	description="Erfahren Sie, wie Azure Service Bus-Trigger und -Bindungen in Azure Functions verwendet werden."
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
	ms.author="chrande; glenga"/>

# Service Bus-Trigger und -Bindungen für Warteschlangen und Themen in Azure Functions

[AZURE.INCLUDE [functions-selector-bindings](../../includes/functions-selector-bindings.md)]

Dieser Artikel erläutert das Konfigurieren und Codieren von Azure Service Bus-Triggern und -Bindungen in Azure Functions.

[AZURE.INCLUDE [Einführung](../../includes/functions-bindings-intro.md)]

## <a id="sbtrigger"></a> Service Bus-Trigger für Warteschlangen oder Themen

#### function.json

Die Datei *function.json* gibt die folgenden Eigenschaften an.

- `name`: Variablenname, der im Funktionscode für die Warteschlange oder das Thema bzw. die Warteschlangen- oder Themennachricht verwendet wird.
- `queueName`: Name der abzufragenden Warteschlange (nur für Warteschlangentrigger).
- `topicName`: Name des abzufragenden Themas (nur für Thementrigger).
- `subscriptionName`: Name des Abonnements (nur für Thementrigger).
- `connection`: Name einer App-Einstellung, die eine Service Bus-Verbindungszeichenfolge enthält. Die Verbindungszeichenfolge muss für einen Service Bus-Namespace gelten und darf nicht auf eine bestimmte Warteschlange oder ein Thema beschränkt sein. Wenn die Verbindungszeichenfolge keine Verwaltungsrechte hat, legen Sie die `accessRights`-Eigenschaft fest. Wenn Sie `connection` leer lassen, verwendet der Trigger bzw. die Bindung die standardmäßige Service Bus-Verbindungszeichenfolge für die Funktionen-App, wie in der App-Einstellung AzureWebJobsServiceBus angegeben.
- `accessRights`: gibt die Zugriffsrechte an, die für die Verbindungszeichenfolge zur Verfügung stehen. Der Standardwert ist `manage`. Legen Sie diese Einstellung auf `listen` fest, wenn Sie eine Verbindungszeichenfolge nutzen, die keine Verwaltungsberechtigungen bietet. Andernfalls versucht die Functions-Laufzeit ggf. erfolglos Vorgänge, die Verwaltungsrechte erfordern.
- `type`: muss auf *serviceBusTrigger* festgelegt werden.
- `direction`: Muss auf *in* festgelegt werden.

*function.json*-Beispiel für einen Service Bus-Warteschlangentrigger:

```json
{
  "bindings": [
    {
      "queueName": "testqueue",
      "connection": "MyServiceBusConnection",
      "name": "myQueueItem",
      "type": "serviceBusTrigger",
      "direction": "in"
    }
  ],
  "disabled": false
}
```

#### C#-Codebeispiel, das eine Service Bus-Warteschlangennachricht verarbeitet

```csharp
public static void Run(string myQueueItem, TraceWriter log)
{
    log.Info($"C# ServiceBus queue trigger function processed message: {myQueueItem}");
}
```

#### F#-Codebeispiel, das eine Service Bus-Warteschlangennachricht verarbeitet

```fsharp
let Run(myQueueItem: string, log: TraceWriter) =
    log.Info(sprintf "F# ServiceBus queue trigger function processed message: %s" myQueueItem)
```

#### Node.js-Codebeispiel, das eine Service Bus-Warteschlangennachricht verarbeitet

```javascript
module.exports = function(context, myQueueItem) {
    context.log('Node.js ServiceBus queue trigger function processed message', myQueueItem);
    context.done();
};
```

#### Unterstützte Typen

Die Service Bus-Warteschlangennachricht kann in alle folgenden Typen deserialisiert werden:

* Objekt (aus JSON)
* string
* Bytearray
* `BrokeredMessage` (C#)

#### <a id="sbpeeklock"></a> PeekLock-Verhalten

Die Functions-Runtime empfängt eine Nachricht im `PeekLock`-Modus und ruft bei erfolgreicher Ausführung der Funktion `Complete` für die Nachricht auf. Tritt bei der Ausführung ein Fehler auf, wird `Abandon` aufgerufen. Wenn die Funktion länger als im `PeekLock`-Timeout angegeben ausgeführt wird, wird die Sperre automatisch erneuert.

#### <a id="sbpoison"></a> Behandlung von nicht verarbeitbaren Nachrichten

Service Bus kümmert sich selbst um die Handhabung nicht verarbeitbarer Nachrichten, die in Azure Functions-Konfigurationen und -Code nicht gesteuert oder konfiguriert werden kann.

#### <a id="sbsinglethread"></a> Single-Threading

Die Functions-Laufzeit verarbeitet standardmäßig mehrere Warteschlangennachrichten gleichzeitig. Um die Runtime anzuweisen, immer nur eine Warteschlangen- oder Themennachricht gleichzeitig zu verarbeiten, legen Sie `serviceBus.maxConcurrrentCalls` in der Datei *host.json* auf „1“ fest. Informationen zur Datei *host.json* finden Sie in der [Ordnerstruktur](functions-reference.md#folder-structure) im Artikel zur Referenz für Entwickler und unter [host.json](https://github.com/Azure/azure-webjobs-sdk-script/wiki/host.json) im Wiki des WebJobs.Script-Repositorys.

## <a id="sboutput"></a> Service Bus-Ausgabebindung für Warteschlangen oder Themen

#### function.json

Die Datei *function.json* gibt die folgenden Eigenschaften an.

- `name`: Variablenname, der im Funktionscode für die Warteschlange oder Warteschlangennachricht verwendet wird.
- `queueName`: Name der abzufragenden Warteschlange (nur für Warteschlangentrigger).
- `topicName`: Name des abzufragenden Themas (nur für Thementrigger).
- `subscriptionName`: Name des Abonnements (nur für Thementrigger).
- `connection`: ebenso wie für den Service Bus-Trigger.
- `accessRights`: gibt die Zugriffsrechte an, die für die Verbindungszeichenfolge zur Verfügung stehen. Der Standardwert ist `manage`. Legen Sie diese Einstellung auf `send` fest, wenn Sie eine Verbindungszeichenfolge nutzen, die keine Verwaltungsberechtigungen bietet. Andernfalls versucht die Functions-Laufzeit ggf. erfolglos Vorgänge, die Verwaltungsrechte erfordern, z. B. das Erstellen von Warteschlangen.
- `type`: muss auf *serviceBus* festgelegt werden.
- `direction`: Muss auf *out* festgelegt werden.

*function.json*-Beispiel für die Verwendung eines Zeitgebertriggers zum Schreiben von Service Bus-Warteschlangennachrichten:

```JSON
{
  "bindings": [
    {
      "schedule": "0/15 * * * * *",
      "name": "myTimer",
      "runsOnStartup": true,
      "type": "timerTrigger",
      "direction": "in"
    },
    {
      "name": "outputSbQueue",
      "type": "serviceBus",
      "queueName": "testqueue",
      "connection": "MyServiceBusConnection",
      "direction": "out"
    }
  ],
  "disabled": false
}
``` 

#### Unterstützte Typen

Azure Functions kann eine Service Bus-Warteschlangennachricht mit einem der folgenden Typen erstellen.

* Objekt (erstellt immer eine JSON-Nachricht; erstellt die Nachricht mit einem NULL-Objekt, wenn der Wert bei Funktionsbeendigung NULL ist)
* Zeichenfolge (eine Nachricht wird erstellt, wenn der Wert bei Funktionsbeendigung nicht NULL ist)
* Bytearray (funktioniert wie Zeichenfolge)
* `BrokeredMessage` (C#, funktioniert wie Zeichenfolge)

Für das Erstellen mehrerer Nachrichten in einer C#-Funktion können Sie `ICollector<T>` oder `IAsyncCollector<T>` verwenden. Beim Aufrufen der `Add`-Methode wird eine Nachricht erstellt.

#### C#-Codebeispiele, die Service Bus-Warteschlangennachrichten erstellen

```csharp
public static void Run(TimerInfo myTimer, TraceWriter log, out string outputSbQueue)
{
	string message = $"Service Bus queue message created at: {DateTime.Now}";
    log.Info(message); 
    outputSbQueue = message;
}
```

```csharp
public static void Run(TimerInfo myTimer, TraceWriter log, ICollector<string> outputSbQueue)
{
	string message = $"Service Bus queue message created at: {DateTime.Now}";
    log.Info(message); 
    outputSbQueue.Add("1 " + message);
    outputSbQueue.Add("2 " + message);
}
```

#### F#-Codebeispiel, das eine Service Bus-Warteschlangennachricht erstellt

```fsharp
let Run(myTimer: TimerInfo, log: TraceWriter, outputSbQueue: byref<string>) =
    let message = sprintf "Service Bus queue message created at: %s" (DateTime.Now.ToString())
    log.Info(message)
    outputSbQueue = message
```

#### Node.js-Codebeispiel, das eine Service Bus-Warteschlangennachricht erstellt

```javascript
module.exports = function (context, myTimer) {
    var timeStamp = new Date().toISOString();
    
    if(myTimer.isPastDue)
    {
        context.log('Node.js is running late!');
    }
    var message = 'Service Bus queue message created at ' + timeStamp;
    context.log(message);   
    context.bindings.outputSbQueueMsg = message;
    context.done();
};
```

## Nächste Schritte

[AZURE.INCLUDE [Nächste Schritte](../../includes/functions-bindings-next-steps.md)]

<!---HONumber=AcomDC_0921_2016-->