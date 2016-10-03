<properties
	pageTitle="DocumentDB-Bindungen in Azure Functions | Microsoft Azure"
	description="Erfahren Sie, wie Azure DocumentDB-Bindungen in Azure Functions verwendet werden."
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

# DocumentDB-Bindungen in Azure Functions

[AZURE.INCLUDE [functions-selector-bindings](../../includes/functions-selector-bindings.md)]

Dieser Artikel erläutert das Konfigurieren und Codieren von Azure DocumentDB-Bindungen in Azure Functions.

[AZURE.INCLUDE [Einführung](../../includes/functions-bindings-intro.md)]

## <a id="docdbinput"></a> Azure DocumentDB-Eingabebindung

Eingabebindungen können ein Dokument aus einer DocumentDB-Sammlung laden und direkt an Ihre Bindung übergeben. Die Dokument-ID kann basierend auf dem Trigger ermittelt werden, der die Funktion aufgerufen hat. In einer C#-Funktion werden alle Änderungen am Datensatz automatisch wieder an die Sammlung zurückgesendet, wenn die Funktion erfolgreich beendet wird.

#### „function.json“ für DocumentDB-Eingabebindung

Die Datei *function.json* stellt die folgenden Eigenschaften bereit:

- `name`: Variablenname, der im Funktionscode für das Dokument verwendet wird.
- `type`: Muss auf "documentdb" festgelegt werden.
- `databaseName`: Die Datenbank mit dem Dokument.
- `collectionName`: Die Sammlung mit dem Dokument.
- `id`: Die ID des abzurufenden Dokuments. Diese Eigenschaft unterstützt Bindungen wie „{queueTrigger}“, die den Zeichenfolgenwert der Warteschlangennachricht als Dokument-ID verwenden.
- `connection`: Diese Zeichenfolge muss eine Anwendungseinstellung sein, die auf den Endpunkt Ihres DocumentDB-Kontos festgelegt ist. Wenn Sie Ihr Konto auf der Registerkarte „Integrieren“ auswählen, wird eine neue App-Einstellung mit einem Namen für Sie erstellt, der das folgende Format aufweist: IhrKonto\_DOCUMENTDB. Wenn Sie die App-Einstellung manuell erstellen möchten, muss die eigentliche Verbindungszeichenfolge das folgende Format besitzen: AccountEndpoint=<Endpunkt für Ihr Konto>;AccountKey=<Ihr primärer Zugriffsschlüssel>;.
- direction: Muss auf *"in"* festgelegt werden.

Beispiel für *function.json*:
 
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

#### Azure DocumentDB-Eingabecodebeispiel für einen F#-Warteschlangentrigger

Bei Verwendung des oben genannten Beispiels für „function.json“ ruft die DocumentDB-Eingabebindung das Dokument mit der ID ab, die der Zeichenfolge der Warteschlangennachricht entspricht, und übergibt es an den Parameter „document“. Wenn das Dokument nicht gefunden wird, lautet der Parameter „document“ NULL. Das Dokument wird dann mit dem neuen Textwert aktualisiert, wenn die Funktion beendet wird.

	open FSharp.Interop.Dynamic
	let Run(myQueueItem: string, document: obj) =
	    document?text <- "This has changed."

Sie benötigen eine `project.json`-Datei, die etwa wie folgt mithilfe von NuGet die Pakete `FSharp.Interop.Dynamic` und `Dynamitey` als Paketabhängigkeiten angibt:

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

Dabei wird NuGet zum Abrufen der Abhängigkeiten verwendet, auf die dann in Ihrem Skript verwiesen wird.

#### Azure DocumentDB-Eingabecodebeispiel für einen Node.js-Warteschlangentrigger
 
Bei Verwendung des oben genannten Beispiels für „function.json“ ruft die DocumentDB-Eingabebindung das Dokument mit der ID ab, die der Zeichenfolge der Warteschlangennachricht entspricht, und übergibt es an die `documentIn`-Bindungseigenschaft. In Node.js-Funktionen werden aktualisierte Dokumente nicht an die Sammlung zurückgesendet. Sie können die Eingabebindung jedoch direkt an eine DocumentDB-Ausgabebindung namens `documentOut` übergeben, um Updates zu unterstützen. In diesem Codebeispiel wird die Texteigenschaft des Eingabedokuments aktualisiert und als Ausgabedokument festgelegt.
 
	module.exports = function (context, input) {   
	    context.bindings.documentOut = context.bindings.documentIn;
	    context.bindings.documentOut.text = "This was updated!";
	    context.done();
	};

## <a id="docdboutput"></a> Azure DocumentDB-Ausgabebindungen

Ihre Funktionen können JSON-Dokumente mithilfe der Ausgabebindung **Azure DocumentDB-Dokument** in eine Azure DocumentDB-Datenbank schreiben. Weitere Informationen zu Azure DocumentDB erhalten Sie unter [Einführung in DocumentDB](../documentdb/documentdb-introduction.md) sowie im [Tutorial mit den ersten Schritten](../documentdb/documentdb-get-started.md).

#### „function.json“ für DocumentDB-Ausgabebindung

Die Datei „function.json“ stellt die folgenden Eigenschaften bereit:

- `name`: Variablenname, der im Funktionscode für das neue Dokument verwendet wird.
- `type`: Muss auf *"documentdb"* festgelegt werden.
- `databaseName`: Datenbank mit der Sammlung, in der das neue Dokument erstellt wird.
- `collectionName`: Sammlung, in der das neue Dokument erstellt wird.
- `createIfNotExists`: Boolescher Wert, der angibt, ob die Sammlung erstellt werden soll, wenn sie nicht vorhanden ist. Die Standardeinstellung ist *false*. Der Grund hierfür ist, dass Sammlungen mit reserviertem Durchsatz erstellt werden, was sich auf den Preis auswirkt. Weitere Informationen finden Sie in der [Preisübersicht](https://azure.microsoft.com/pricing/details/documentdb/).
- `connection`: Diese Zeichenfolge muss eine **Anwendungseinstellung** sein, die auf den Endpunkt Ihres DocumentDB-Kontos festgelegt ist. Wenn Sie Ihr Konto auf der Registerkarte **Integrieren** auswählen, wird eine neue App-Einstellung mit einem Namen im folgenden Format erstellt: `yourAccount_DOCUMENTDB`. Wenn Sie die App-Einstellung manuell erstellen möchten, muss die eigentliche Verbindungszeichenfolge das folgende Format besitzen: `AccountEndpoint=<Endpoint for your account>;AccountKey=<Your primary access key>;`.
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
 

#### Azure DocumentDB-Ausgabecodebeispiel für einen F#-Warteschlangentrigger

	open FSharp.Interop.Dynamic
	let Run(myQueueItem: string, document: obj) =
	    document?text <- (sprintf "I'm running in an F# function! %s" myQueueItem)

#### Azure DocumentDB-Ausgabecodebeispiel für einen C#-Warteschlangentrigger


	using System;

	public static void Run(string myQueueItem, out object document, TraceWriter log)
	{
	    log.Info($"C# Queue trigger function processed: {myQueueItem}");
	   
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
	    log.Info($"C# Queue trigger function processed: {myQueueItem}");
	    
	    dynamic employee = JObject.Parse(myQueueItem);
	    
	    employeeDocument = new {
	        id = employee.name + "-" + employee.employeeId,
	        name = employee.name,
	        employeeId = employee.employeeId,
	        address = employee.address
	    };
	}

Alternativ können Sie den entsprechenden F#-Code verwenden:

	open FSharp.Interop.Dynamic
	open Newtonsoft.Json

	type Employee = {
	    id: string
	    name: string
	    employeeId: string
	    address: string
	}

	let Run(myQueueItem: string, employeeDocument: byref<obj>, log: TraceWriter) =
	    log.Info(sprintf "F# Queue trigger function processed: %s" myQueueItem)
	    let employee = JObject.Parse(myQueueItem)
	    employeeDocument <-
	        { id = sprintf "%s-%s" employee?name employee?employeeId
	          name = employee?name
	          employeeId = employee?id
	          address = employee?address }

Beispielausgabe:

	{
	  "id": "John Henry-123456",
	  "name": "John Henry",
	  "employeeId": "123456",
	  "address": "A town nearby"
	}

## Nächste Schritte

[AZURE.INCLUDE [Nächste Schritte](../../includes/functions-bindings-next-steps.md)]

<!---HONumber=AcomDC_0921_2016-->