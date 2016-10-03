<properties
	pageTitle="Event Hub-Bindungen für Azure Functions | Microsoft Azure"
	description="Erfahren Sie, wie Azure Event Hub-Bindungen in Azure Functions verwendet werden."
	services="functions"
	documentationCenter="na"
	authors="wesmc7777"
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
	ms.author="wesmc"/>

# Event Hub-Bindungen für Azure Functions

[AZURE.INCLUDE [functions-selector-bindings](../../includes/functions-selector-bindings.md)]

Dieser Artikel erläutert das Konfigurieren und Codieren von [Azure Event Hub](../event-hubs/event-hubs-overview.md)-Bindungen für Azure Functions. Azure Functions unterstützt Auslöse- und Ausgabebindungen für Azure Event Hubs.

[AZURE.INCLUDE [Einführung](../../includes/functions-bindings-intro.md)]


## Azure Event Hub-Triggerbindung

Ein Azure Event Hub-Trigger kann verwendet werden, um auf ein Ereignis zu reagieren, das an einen Event Hub-Datenstrom gesendet wird. Sie benötigen Lesezugriff auf den Event Hub, um eine Triggerbindung einzurichten.

#### function.json für Event Hub-Triggerbindung

Die Datei *function.json* für einen Azure Event Hub-Trigger gibt die folgenden Eigenschaften an:

- `type`: Muss auf *eventHubTrigger* festgelegt werden.
- `name`: Variablenname, der im Funktionscode für die Event Hub-Nachricht verwendet wird.
- `direction`: Muss auf *in* festgelegt werden.
- `path`: Name des Event Hubs.
- `connection`: Name einer App-Einstellung, die die Zeichenfolge für die Verbindung mit dem Namespace enthält, in dem sich der Event Hub befindet. Kopieren Sie diese Verbindungszeichenfolge, indem Sie für den Namespace (nicht für den eigentlichen Event Hub) auf die Schaltfläche **Verbindungsinformationen** klicken. Diese Verbindungszeichenfolge muss mindestens über Leseberechtigungen verfügen, um den Trigger zu aktivieren.

		{
		  "bindings": [
		    {
		      "type": "eventHubTrigger",
		      "name": "myEventHubMessage",
		      "direction": "in",
		      "path": "MyEventHub",
		      "connection": "myEventHubReadConnectionString"
		    }
		  ],
		  "disabled": false
		}

#### C#-Beispiel für Azure Event Hub-Trigger
 
Mit dem oben genannten Beispiel function.json wird der Text der Ereignismeldung mit dem folgenden C#-Funktionscode protokolliert:
 
	using System;
	
	public static void Run(string myEventHubMessage, TraceWriter log)
	{
	    log.Info($"C# Event Hub trigger function processed a message: {myEventHubMessage}");
	}

#### F#-Beispiel für Azure Event Hub-Trigger

Mit dem oben genannten Beispiel für „function.json“ wird der Text der Ereignismeldung mit dem folgenden F#-Funktionscode protokolliert:

	let Run(myEventHubMessage: string, log: TraceWriter) =
	    log.Info(sprintf "F# eventhub trigger function processed work item: %s" myEventHubMessage)

#### Node.js-Beispiel für Azure Event Hub-Trigger
 
Mit dem oben genannten Beispiel function.json wird der Text der Ereignismeldung mit dem folgenden Node.js-Funktionscode protokolliert:
 
	module.exports = function (context, myEventHubMessage) {
	    context.log('Node.js eventhub trigger function processed work item', myEventHubMessage);	
	    context.done();
	};


## Azure Event Hub-Ausgabebindung

Mit einer Azure Event Hub-Ausgabebindung werden Ereignisse in einen Event Hub-Ereignisdatenstrom geschrieben. Um Ereignisse in einen Event Hub schreiben zu können, müssen Sie über eine Sendeberechtigung verfügen.

#### function.json für Event Hub-Ausgabebindung

Die Datei *function.json* für eine Azure Event Hub-Ausgabebindung gibt die folgenden Eigenschaften an:

- `type`: Muss auf *eventHub* festgelegt werden.
- `name`: Variablenname, der im Funktionscode für die Event Hub-Nachricht verwendet wird.
- `path`: Name des Event Hubs.
- `connection`: Name einer App-Einstellung, die die Zeichenfolge für die Verbindung mit dem Namespace enthält, in dem sich der Event Hub befindet. Kopieren Sie diese Verbindungszeichenfolge, indem Sie für den Namespace (nicht für den eigentlichen Event Hub) auf die Schaltfläche **Verbindungsinformationen** klicken. Diese Verbindungszeichenfolge muss über Sendeberechtigungen zum Senden der Nachricht an den Event Hub-Datenstrom verfügen.
- `direction`: Muss auf *out* festgelegt werden.

	    {
	      "type": "eventHub",
	      "name": "outputEventHubMessage",
	      "path": "myeventhub",
	      "connection": "MyEventHubSend",
	      "direction": "out"
	    }


#### C#-Codebeispiel für die Azure Event Hub-Ausgabebindung
 
Der folgende C#-Beispielfunktionscode veranschaulicht das Schreiben eines Ereignisses in einen Event Hub-Datenstrom. Dieses Beispiel zeigt die Anwendung der oben dargestellten Event Hub-Ausgabebindung auf einen C#-Trigger mit Timer.
 
	using System;
	
	public static void Run(TimerInfo myTimer, out string outputEventHubMessage, TraceWriter log)
	{
	    String msg = $"TimerTriggerCSharp1 executed at: {DateTime.Now}";
	
	    log.Verbose(msg);   
	    
	    outputEventHubMessage = msg;
	}

#### F#-Codebeispiel für die Azure Event Hub-Ausgabebindung

Der folgende F#-Beispielfunktionscode veranschaulicht das Schreiben eines Ereignisses in einen Event Hub-Datenstrom. Dieses Beispiel zeigt die Anwendung der oben dargestellten Event Hub-Ausgabebindung auf einen C#-Trigger mit Timer.

	let Run(myTimer: TimerInfo, outputEventHubMessage: byref<string>, log: TraceWriter) =
	    let msg = sprintf "TimerTriggerFSharp1 executed at: %s" DateTime.Now.ToString()
	    log.Verbose(msg);
	    outputEventHubMessage <- msg;

#### Node.js-Codebeispiel für die Azure Event Hub-Ausgabebindung
 
Der folgende Node.js-Beispielfunktionscode veranschaulicht das Schreiben eines Ereignisses in einen Event Hub-Datenstrom. Dieses Beispiel zeigt die Anwendung der oben dargestellten Event Hub-Ausgabebindung auf einen Node.js-Trigger mit Timer.
 
	module.exports = function (context, myTimer) {
	    var timeStamp = new Date().toISOString();
	    
	    if(myTimer.isPastDue)
	    {
	        context.log('TimerTriggerNodeJS1 is running late!');
	    }

	    context.log('TimerTriggerNodeJS1 function ran!', timeStamp);   
	    
	    context.bindings.outputEventHubMessage = "TimerTriggerNodeJS1 ran at : " + timeStamp;
	
	    context.done();
	};

## Nächste Schritte

[AZURE.INCLUDE [Nächste Schritte](../../includes/functions-bindings-next-steps.md)]

<!---HONumber=AcomDC_0921_2016-->