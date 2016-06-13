<properties
	pageTitle="Entwicklerreferenz zu Azure Functions | Microsoft Azure"
	description="Erfahren Sie, wie Azure Functions mithilfe von C# entwickelt wird."
	services="functions"
	documentationCenter="na"
	authors="christopheranderson"
	manager="erikre"
	editor=""
	tags=""
	keywords="Azure Functions, Functions, Ereignisverarbeitung, Webhooks, dynamisches Compute, serverlose Architektur"/>

<tags
	ms.service="functions"
	ms.devlang="dotnet"
	ms.topic="reference"
	ms.tgt_pltfrm="multiple"
	ms.workload="na"
	ms.date="05/13/2016"
	ms.author="chrande"/>

# C#-Entwicklerreferenz zu Azure Functions

Die C#-Benutzeroberfläche für Azure Functions basiert auf dem Azure WebJobs-SDK. Daten fließen über Methodenargumente in Ihre C#-Funktion ein. Argumentnamen werden in `function.json` angegeben, und es gibt vordefinierte Namen für den Zugriff auf Elemente wie die Funktionsprotokollierung und Abbruchtoken.

In diesem Artikel wird davon ausgegangen, dass Sie bereits die [Entwicklerreferenz zu Azure Functions](functions-reference.md) gelesen haben.

## Funktionsweise von CSX

Durch das `.csx`-Format müssen Sie weniger „Textbausteine“ schreiben, sondern können sich auf das Schreiben einer C#-Funktion konzentrieren. Für Azure Functions schließen Sie wie üblich alle benötigten Assemblyverweise und Namespaces ein, und statt alles in einem Namespace und in einer Klasse zu umschließen, können Sie einfach Ihre `Run`-Methode definieren. Wenn Sie Klassen einbeziehen müssen, um beispielsweise POCO-Objekte zu definieren, können Sie eine Klasse innerhalb derselben Datei einschließen.

## Binden an Argumente

Die verschiedenen Bindungen sind über die `name`-Eigenschaft in der *function.json*-Konfiguration an eine C#-Funktion gebunden. Jede Bindung verfügt über eigene unterstützte Typen, die pro Bindung dokumentiert sind. Ein Blobtrigger kann z. B. eine Zeichenfolge, ein POCO oder verschiedene andere Typen unterstützen. Sie können den Typ verwenden, der Ihren Anforderungen am besten entspricht.

```csharp
public static void Run(string myBlob, out MyClass myQueueItem)
{
    log.Verbose($"C# Blob trigger function processed: {myBlob}");
    myQueueItem = new MyClass() { Id = "myid" };
}

public class MyClass
{
    public string Id { get; set; }
}
```

## Protokollierung

Zum Protokollieren der Ausgabe in Ihren Streamingprotokollen in C# können Sie ein Argument vom Typ `TraceWriter` angeben. Es wird empfohlen, dafür den Namen `log` zu verwenden. Es wird davon abgeraten, `Console.Write` in Azure Functions zu verwenden.

```csharp
public static void Run(string myBlob, TraceWriter log)
{
    log.Verbose($"C# Blob trigger function processed: {myBlob}");
}
```

## Async

Um eine Funktion asynchron auszuführen, verwenden Sie das `async`-Schlüsselwort, und geben Sie ein `Task`-Objekt zurück.

```csharp
public async static Task ProcessQueueMessageAsync(
        string blobName, 
        Stream blobInput,
        Stream blobOutput)
    {
        await blobInput.CopyToAsync(blobOutput, 4096, token);
    }
```

## Abbruchtoken

Manche Vorgänge reagieren empfindlich auf das Herunterfahren. Es empfiehlt sich grundsätzlich, Code zu schreiben, der Abstürze verarbeiten kann. In Fällen, in denen Anforderungen zum ordnungsgemäßen Herunterfahren verarbeitet werden sollen, definieren Sie ein Argument vom Typ [`CancellationToken`](https://msdn.microsoft.com/library/system.threading.cancellationtoken.aspx). Ein `CancellationToken` wird bereitgestellt, wenn ein Herunterfahren des Hosts ausgelöst wird.

```csharp
public async static Task ProcessQueueMessageAsyncCancellationToken(
        string blobName, 
        Stream blobInput,
        Stream blobOutput,
        CancellationToken token)
    {
        await blobInput.CopyToAsync(blobOutput, 4096, token);
    }
```

## Importieren von Namespaces

Wenn Sie Namespaces importieren müssen, ist dies wie üblich über die `using`-Klausel möglich.

```csharp
using System.Net;
using System.Threading.Tasks;

public static Task<HttpResponseMessage> Run(HttpRequestMessage req, TraceWriter log)
```

Die folgenden Namespaces werden automatisch importiert und sind daher optional:

* `System`
* `System.Collections.Generic`
* `System.IO`
* `System.Linq`
* `System.Net.Http`
* `System.Threading.Tasks`
* `Microsoft.Azure.WebJobs`
* `Microsoft.Azure.WebJobs.Host`.

## Verweise auf externe Assemblys

Fügen Sie bei Frameworkassemblys über die `#r "AssemblyName"`-Anweisung Verweise hinzu.

```csharp
#r "System.Web.Http"

using System.Net;
using System.Net.Http;
using System.Threading.Tasks;

public static Task<HttpResponseMessage> Run(HttpRequestMessage req, TraceWriter log)
```

Die folgenden Assemblys werden automatisch von der Azure Functions-Hostumgebung hinzugefügt:

* `mscorlib`,
* `System`
* `System.Core`
* `System.Xml`
* `System.Net.Http`
* `Microsoft.Azure.WebJobs`
* `Microsoft.Azure.WebJobs.Host`
* `Microsoft.Azure.WebJobs.Extensions`
* `System.Web.Http`
* `System.Net.Http.Formatting`.

Darüber hinaus sind die folgenden Assemblys besondere Fälle, die über „SimpleName“ referenziert werden können (z. B. `#r "AssemblyName"`):

* `Newtonsoft.Json`
* `Microsoft.WindowsAzure.Storage`
* `Microsoft.ServiceBus`
* `Microsoft.AspNet.WebHooks.Receivers`
* `Microsoft.AspNEt.WebHooks.Common`.

Wenn Sie auf eine private Assembly verweisen müssen, können Sie die Assemblydatei in einen `bin`-Ordner relativ zu Ihrer Funktion hochladen und anhand des Dateinamens (z. B. `#r "MyAssembly.dll"`) darauf verweisen. Informationen zum Hochladen von Dateien in Ihren Funktionenordner finden Sie im folgenden Abschnitt zur Paketverwaltung.

## Paketverwaltung

Um NuGet-Pakete in einer C#-Funktion zu verwenden, laden Sie die Datei *project.json* in den Ordner der Funktion im Dateisystem der Funktionen-App hoch. Hier sehen Sie ein Beispiel der Datei *project.json*, die einen Verweis auf Microsoft.ProjectOxford.Face, Version 1.1.0, hinzufügt:

```json
{
  "frameworks": {
    "net46":{
      "dependencies": {
        "Microsoft.ProjectOxford.Face": "1.1.0"
      }
    }
   }
}
```

Nur .NET Framework 4.6 wird unterstützt. Stellen Sie also sicher, dass Ihre Datei *project.json* `net46` wie hier gezeigt angibt.

Beim Hochladen der Datei *project.json* erhält die Laufzeit die Pakete und fügt automatisch Verweise auf die Paketassemblys hinzu. Sie müssen keine `#r "AssemblyName"`-Direktiven hinzufügen. Fügen Sie Ihrer Datei *run.csx* einfach die erforderlichen `using`-Anweisungen hinzu, um die in den NuGet-Paketen definierten Typen zu verwenden.


### Hochladen einer Datei „project.json“

1. Stellen Sie zunächst sicher, dass Ihre Funktionen-App ausgeführt wird. Öffnen Sie zu diesem Zweck Ihre Funktion im Azure-Portal. 

	Dies ermöglicht auch Zugriff auf die Streamingprotokolle, in denen die Ausgabe der Paketinstallation angezeigt wird.

2. Verwenden Sie zum Hochladen der Datei „project.json“ eine der Methoden, die im Abschnitt **Aktualisieren von Funktionen-App-Dateien** im entsprechenden Thema der [Referenz für Azure Functions-Entwickler](functions-reference.md#fileupdate) beschrieben werden.

3. Nach dem Hochladen der Datei *project.json* wird im Streamingprotokoll Ihrer Funktion in etwa folgende Ausgabe angezeigt:

```
2016-04-04T19:02:48.745 Restoring packages.
2016-04-04T19:02:48.745 Starting NuGet restore
2016-04-04T19:02:50.183 MSBuild auto-detection: using msbuild version '14.0' from 'D:\Program Files (x86)\MSBuild\14.0\bin'.
2016-04-04T19:02:50.261 Feeds used:
2016-04-04T19:02:50.261 C:\DWASFiles\Sites\facavalfunctest\LocalAppData\NuGet\Cache
2016-04-04T19:02:50.261 https://api.nuget.org/v3/index.json
2016-04-04T19:02:50.261 
2016-04-04T19:02:50.511 Restoring packages for D:\home\site\wwwroot\HttpTriggerCSharp1\Project.json...
2016-04-04T19:02:52.800 Installing Newtonsoft.Json 6.0.8.
2016-04-04T19:02:52.800 Installing Microsoft.ProjectOxford.Face 1.1.0.
2016-04-04T19:02:57.095 All packages are compatible with .NETFramework,Version=v4.6.
2016-04-04T19:02:57.189 
2016-04-04T19:02:57.189 
2016-04-04T19:02:57.455 Packages restored.
```

## Umgebungsvariablen

Verwenden Sie `System.Environment.GetEnvironmentVariable` zum Abrufen einer Umgebungsvariable oder des Werts einer App-Einstellung (siehe das folgende Codebeispiel):

```csharp
public static void Run(TimerInfo myTimer, TraceWriter log)
{
    log.Info($"C# Timer trigger function executed at: {DateTime.Now}");
    log.Info(GetEnvironmentVariable("AzureWebJobsStorage"));
    log.Info(GetEnvironmentVariable("WEBSITE_SITE_NAME"));
}

public static string GetEnvironmentVariable(string name)
{
    return name + ": " + 
        System.Environment.GetEnvironmentVariable(name, EnvironmentVariableTarget.Process);
}
```

## Wiederverwenden von CSX-Code

Sie können in Ihrer Datei *run.csx* Klassen und Methoden verwenden, die in anderen *CSX*-Dateien definiert sind. Verwenden Sie hierzu `#load`-Direktiven in Ihrer Datei *run.csx*, wie im folgenden Beispiel gezeigt.

Beispiel für *run.csx*:

```csharp
#load "mylogger.csx"

public static void Run(TimerInfo myTimer, TraceWriter log)
{
    log.Verbose($"Log by run.csx: {DateTime.Now}"); 
    MyLogger(log, $"Log by MyLogger: {DateTime.Now}");
}
```

Beispiel für *mylogger.csx*:

```csharp
public static void MyLogger(TraceWriter log, string logtext)
{
    log.Verbose(logtext); 
}
```

Sie können mit der `#load`-Direktive einen relativen Pfad verwenden:

* `#load "mylogger.csx"` lädt eine Datei, die sich im Funktionsordner befindet.

* `#load "loadedfiles\mylogger.csx"` lädt eine Datei, die sich in einem Ordner im Funktionsordner befindet.

* `#load "..\shared\mylogger.csx"` lädt eine Datei, die sich in einem Ordner auf derselben Ebene wie der Funktionsordner direkt unter *wwwroot* befindet.
 
Die `#load`-Direktive kann nur mit *CSX*-Dateien (C#-Skript), nicht mit *CS*-Dateien verwendet werden.

## Nächste Schritte

Weitere Informationen finden Sie in den folgenden Ressourcen:

* [Entwicklerreferenz zu Azure Functions](functions-reference.md)
* [NodeJS-Entwicklerreferenz zu Azure Functions](functions-reference-node.md)
* [Trigger und Bindungen in Azure Functions](functions-triggers-bindings.md)

<!---HONumber=AcomDC_0601_2016-->