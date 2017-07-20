---
title: C#-Skriptentwicklerreferenz zu Azure Functions | Microsoft-Dokumentation
description: Erfahren Sie, wie Azure Functions mithilfe von C# entwickelt wird.
services: functions
documentationcenter: na
author: lindydonna
manager: erikre
editor: 
tags: 
keywords: Azure Functions, Functions, Ereignisverarbeitung, Webhooks, dynamisches Compute, serverlose Architektur
ms.assetid: f28cda01-15f3-4047-83f3-e89d5728301c
ms.service: functions
ms.devlang: dotnet
ms.topic: reference
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 06/07/2017
ms.author: donnam
ms.translationtype: Human Translation
ms.sourcegitcommit: bb794ba3b78881c967f0bb8687b1f70e5dd69c71
ms.openlocfilehash: 1d0143d72ab18deeba7a32cc732445cc7ba64019
ms.contentlocale: de-de
ms.lasthandoff: 07/06/2017


---
# <a name="azure-functions-c-script-developer-reference"></a>C#-Skriptentwicklerreferenz zu Azure Functions
> [!div class="op_single_selector"]
> * [C#-Skript](functions-reference-csharp.md)
> * [F#-Skript](functions-reference-fsharp.md)
> * [Node.js](functions-reference-node.md)
>
>

Die C#-Skriptoberfläche für Azure Functions basiert auf dem Azure WebJobs SDK. Daten fließen über Methodenargumente in Ihre C#-Funktion ein. Argumentnamen werden in `function.json`angegeben, und es gibt vordefinierte Namen für den Zugriff auf Elemente wie die Funktionsprotokollierung und Abbruchtoken.

In diesem Artikel wird davon ausgegangen, dass Sie bereits die [Entwicklerreferenz zu Azure Functions](functions-reference.md)gelesen haben.

Weitere Informationen zur Verwendung von C#-Klassenbibliotheken finden Sie unter [Verwenden von .NET-Klassenbibliotheken mit Azure Functions](functions-dotnet-class-library.md).

## <a name="how-csx-works"></a>Funktionsweise von CSX
Dank des `.csx`-Formats müssen Sie weniger Textbausteine schreiben und können sich ganz auf das Schreiben einer C#-Funktion konzentrieren. Schließen Sie wie gewohnt alle Assemblyverweise und Namespaces am Anfang der Datei ein. Anstatt sämtliche Informationen in einem Namespace und einer Klasse zu umschließen, definieren Sie einfach eine `Run`-Methode. Wenn Sie Klassen einbeziehen müssen, um beispielsweise POCO-Objekte (Plain Old CLR Object) zu definieren, können Sie eine Klasse innerhalb derselben Datei einschließen.   

## <a name="binding-to-arguments"></a>Binden an Argumente
Die verschiedenen Bindungen sind über die `name`-Eigenschaft in der *function.json*-Konfiguration an eine C#-Funktion gebunden. Jede Bindung weist eigene unterstützte Typen auf. So kann ein Blobtrigger z.B. eine Zeichenfolge, ein POCO oder ein CloudBlockBlob unterstützen. Die unterstützten Typen werden für jede Bindung im Verweis dokumentiert. Bei einem POCO-Objekt müssen für jede Eigenschaft ein Getter und ein Setter definiert werden.

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

[!INCLUDE [HTTP client best practices](../../includes/functions-http-client-best-practices.md)]

## <a name="using-method-return-value-for-output-binding"></a>Verwenden von Rückgabewerten bestimmter Methoden für Ausgabebindungen

Sie können für eine Ausgabebindung den Rückgabewert einer Methode verwenden, indem Sie den Namen `$return` in der Datei *function.json* verwenden:

```json
{
    "type": "queue",
    "direction": "out",
    "name": "$return",
    "queueName": "outqueue",
    "connection": "MyStorageConnectionString",
}
```

```csharp
public static string Run(string input, TraceWriter log)
{
    return input;
}
```

## <a name="writing-multiple-output-values"></a>Schreiben von mehreren Ausgabewerten

Um mehrere Werte in eine Ausgabebindung zu schreiben, verwenden Sie die Typen [`ICollector`](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/ICollector.cs) oder [`IAsyncCollector`](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/IAsyncCollector.cs). Diese Typen stellen lesegeschützte Sammlungen dar, die nach Durchführung der Methode in die Ausgabebindung geschrieben werden.

In diesem Beispiel werden mehrere Warteschlangennachrichten mit `ICollector` geschrieben:

```csharp
public static void Run(ICollector<string> myQueueItem, TraceWriter log)
{
    myQueueItem.Add("Hello");
    myQueueItem.Add("World!");
}
```

## <a name="logging"></a>Protokollierung
Um eine Ausgabe in C# in Ihren Streamingprotokollen zu dokumentieren, fügen Sie ein Argument vom Typ `TraceWriter` ein. Verwenden Sie hierzu am besten den Namen `log`. Vermeiden Sie die Verwendung von `Console.Write` in Azure Functions. 

`TraceWriter` ist im [Azure WebJobs SDK](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs.Host/TraceWriter.cs) definiert. Die Protokollebene für `TraceWriter` kann in [host\.json] konfiguriert werden.

```csharp
public static void Run(string myBlob, TraceWriter log)
{
    log.Info($"C# Blob trigger function processed: {myBlob}");
}
```

## <a name="async"></a>Async
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

## <a name="cancellation-token"></a>Abbruchtoken
Einige Vorgänge erfordern ein ordnungsgemäßes Herunterfahren. Es empfiehlt sich grundsätzlich, Codes zu schreiben, die Abstürze behandeln können. In Fällen, in denen Anforderungen zum ordnungsgemäßen Herunterfahren verarbeitet werden sollen, definieren Sie ein typisiertes [`CancellationToken`](https://msdn.microsoft.com/library/system.threading.cancellationtoken.aspx)-Argument.  Ein `CancellationToken` wird angegeben, um zu signalisieren, dass das Herunterfahren des Hosts ausgelöst wird.

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

## <a name="importing-namespaces"></a>Importieren von Namespaces
Wenn Sie Namespaces importieren müssen, ist dies wie üblich über die `using` -Klausel möglich.

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

## <a name="referencing-external-assemblies"></a>Verweise auf externe Assemblys
Bei Frameworkassemblys können Sie Verweise über die `#r "AssemblyName"` -Anweisung hinzufügen.

```csharp
#r "System.Web.Http"

using System.Net;
using System.Net.Http;
using System.Threading.Tasks;

public static Task<HttpResponseMessage> Run(HttpRequestMessage req, TraceWriter log)
```

Die folgenden Assemblys werden automatisch von der Azure Functions-Hostumgebung hinzugefügt:

* `mscorlib`
* `System`
* `System.Core`
* `System.Xml`
* `System.Net.Http`
* `Microsoft.Azure.WebJobs`
* `Microsoft.Azure.WebJobs.Host`
* `Microsoft.Azure.WebJobs.Extensions`
* `System.Web.Http`
* `System.Net.Http.Formatting`.

Von einem einfachen Namen kann auf folgende Assemblys verwiesen (z.B. `#r "AssemblyName"`):

* `Newtonsoft.Json`
* `Microsoft.WindowsAzure.Storage`
* `Microsoft.ServiceBus`
* `Microsoft.AspNet.WebHooks.Receivers`
* `Microsoft.AspNet.WebHooks.Common`
* `Microsoft.Azure.NotificationHubs`

## <a name="referencing-custom-assemblies"></a>Verweisen von benutzerdefinierten Assemblys

Um eine benutzerdefinierte Assembly zu verweisen, können Sie entweder eine *freigegebene* Assembly oder eine *private* Assembly verwenden:
- Freigegebene Assemblys werden für alle Funktionen innerhalb einer Funktionen-App freigegeben. Um eine benutzerdefinierte Assembly zu verweisen, laden Sie die Assembly in Ihrer Funktionen-App hoch, wie etwa in einen `bin`-Ordner im Stammverzeichnis der Funktionen-App. 
- Private Assemblys sind Bestandteil eines Kontexts einer bestimmten Funktion und unterstützen das Querladen von verschiedenen Versionen. Private Assemblys sollten in einen `bin`-Ordner im Funktionsverzeichnis hochgeladen werden. Verweisen Sie auf den Dateinamen, wie etwa `#r "MyAssembly.dll"`. 

Informationen zum Hochladen von Dateien in Ihren Funktionenordner finden Sie im folgenden Abschnitt zur Paketverwaltung.

### <a name="watched-directories"></a>Überwachte Verzeichnisse

Das Verzeichnis, das die Skriptdatei für die Funktion enthält, wird automatisch im Hinblick auf Änderungen an Assemblys überwacht. Um Änderungen an Assemblys in anderen Verzeichnissen zu überwachen, fügen Sie sie der Liste `watchDirectories` in der Datei [host\.json] hinzu.

## <a name="using-nuget-packages"></a>Verwenden von NuGet-Paketen
Um NuGet-Pakete in einer C#-Funktion zu verwenden, laden Sie die Datei *project.json* in den Ordner der Funktion im Dateisystem der Funktionen-App hoch. Hier sehen Sie ein Beispiel der Datei *project.json* , die einen Verweis auf Microsoft.ProjectOxford.Face (Version 1.1.0) hinzufügt:

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

Nur .NET Framework 4.6 wird unterstützt. Stellen Sie also sicher, dass die Datei *project.json* wie hier gezeigt `net46` angibt.

Beim Hochladen der Datei *project.json* erhält die Laufzeit die Pakete und fügt automatisch Verweise auf die Paketassemblys hinzu. Sie müssen keine `#r "AssemblyName"` -Direktiven hinzufügen. Um die in den NuGet-Paketen definierten Typen zu verwenden, fügen Sie der Datei *run.csx* die erforderlichen `using`-Anweisungen hinzu. 

In der Functions-Laufzeit erfolgt die NuGet-Wiederherstellung durch Vergleichen von `project.json` und `project.lock.json`. Wenn die Datums- und Zeitstempel der Dateien **nicht übereinstimmen**, wird eine NuGet-Wiederherstellung ausgeführt, und NuGet lädt aktualisierte Pakete herunter. Wenn die Datums- und Zeitstempel der Dateien dagegen **übereinstimmen**, wird keine NuGet-Wiederherstellung ausgeführt. Aus diesem Grund sollte `project.lock.json` nicht bereitgestellt werden, da diese dazu führt, dass NuGet die Wiederherstellung von Paketen überspringt. Um die Bereitstellung der Sperrdatei zu verhindern, fügen Sie `project.lock.json` der Datei `.gitignore` hinzu.

Um einen benutzerdefinierten NuGet-Feed zu verwenden, geben Sie den Feed in der Datei *NuGet.config* im Stammverzeichnis der Funktionen-App an. Weitere Informationen finden Sie unter [Konfigurieren des NuGet-Verhaltens](/nuget/consume-packages/configuring-nuget-behavior).

### <a name="using-a-projectjson-file"></a>Verwenden der Datei „project.json“
1. Öffnen Sie die Funktion im Azure-Portal. Auf der Registerkarte „Protokolle“ wird die Ausgabe der Paketinstallation angezeigt.
2. Verwenden Sie zum Hochladen der Datei „project.json“ eine der Methoden, die im Abschnitt [Vorgehensweise zum Aktualisieren von Funktionen-App-Dateien](functions-reference.md#fileupdate) des Themas „Azure Functions-Entwicklerreferenz“ beschrieben werden.
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

## <a name="environment-variables"></a>Umgebungsvariablen
Verwenden Sie `System.Environment.GetEnvironmentVariable`zum Abrufen einer Umgebungsvariablen oder zum Abrufen des Werts einer App-Einstellung, wie im folgenden Codebeispiel zu sehen:

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

## <a name="reusing-csx-code"></a>Wiederverwenden von CSX-Code
Sie können in der Datei *run.csx* Klassen und Methoden verwenden, die in anderen *CSX* -Dateien definiert sind. Verwenden Sie zu diesem Zweck `#load`-Anweisungen in der Datei *run.csx*. Im folgenden Beispiel wird die Protokollierungsroutine `MyLogger` in *myLogger.csx* freigegeben und mit der `#load`-Anweisung in *run.csx* geladen:

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

Die Verwendung einer freigegebenen *CSX*-Datei ist ein häufiges Verfahren, wenn die Argumente zwischen Funktionen mit einem POCO-Objekt stark typisiert werden sollen. Im folgenden vereinfachten Beispiel verwenden ein HTTP-Trigger und ein Warteschlangentrigger gemeinsam das POCO-Objekt `Order`, um die Bestelldaten stark zu typisieren:

Beispiel: *run.csx* für HTTP-Trigger:

```cs
#load "..\shared\order.csx"

using System.Net;

public static async Task<HttpResponseMessage> Run(Order req, IAsyncCollector<Order> outputQueueItem, TraceWriter log)
{
    log.Info("C# HTTP trigger function received an order.");
    log.Info(req.ToString());
    log.Info("Submitting to processing queue.");

    if (req.orderId == null)
    {
        return new HttpResponseMessage(HttpStatusCode.BadRequest);
    }
    else
    {
        await outputQueueItem.AddAsync(req);
        return new HttpResponseMessage(HttpStatusCode.OK);
    }
}
```

Beispiel: *run.csx* für Warteschlangentrigger:

```cs
#load "..\shared\order.csx"

using System;

public static void Run(Order myQueueItem, out Order outputQueueItem,TraceWriter log)
{
    log.Info($"C# Queue trigger function processed order...");
    log.Info(myQueueItem.ToString());

    outputQueueItem = myQueueItem;
}
```

Beispiel: *order.csx*:

```cs
public class Order
{
    public string orderId {get; set; }
    public string custName {get; set;}
    public string custAddress {get; set;}
    public string custEmail {get; set;}
    public string cartId {get; set; }

    public override String ToString()
    {
        return "\n{\n\torderId : " + orderId +
                  "\n\tcustName : " + custName +             
                  "\n\tcustAddress : " + custAddress +             
                  "\n\tcustEmail : " + custEmail +             
                  "\n\tcartId : " + cartId + "\n}";             
    }
}
```

Mit der `#load` -Direktive können Sie einen relativen Pfad verwenden:

* `#load "mylogger.csx"` : Lädt eine Datei, die sich im Funktionsordner befindet.
* `#load "loadedfiles\mylogger.csx"` : Lädt eine Datei, die sich in einem Ordner im Funktionsordner befindet.
* `#load "..\shared\mylogger.csx"` : Lädt eine Datei, die sich in einem Ordner auf der gleichen Ebene befindet wie der Funktionsordner (also direkt unter *wwwroot*).

Die `#load` -Direktive kann nur mit *CSX* -Dateien (C#-Skript) verwendet werden, nicht mit *CS* -Dateien.

<a name="imperative-bindings"></a> 

## <a name="binding-at-runtime-via-imperative-bindings"></a>Bindung zur Runtime durch imperative Bindungen

In C# und anderen .NET-Sprachen können Sie ein [imperatives](https://en.wikipedia.org/wiki/Imperative_programming) Bindungsmuster verwenden, im Gegensatz zu den [ *deklarativen* ](https://en.wikipedia.org/wiki/Declarative_programming) Bindungen in *function.json*. Imperative Bindung eignet sich, wenn Bindungsparameter zur Laufzeit statt zur Entwurfszeit berechnet werden müssen. Mit diesem Muster ist die Bindung an unterstützten Eingabe- und Ausgabebindungen direkt im Funktionscode möglich.

Definieren Sie eine imperative Bindung wie folgt:

- Schließen Sie für die gewünschten imperativen Bindungen **keinen** Eintrag in *function.json* ein.
- Übergeben Sie den Eingabeparameter [`Binder binder`](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs.Host/Bindings/Runtime/Binder.cs) oder [`IBinder binder`](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/IBinder.cs).
- Verwenden Sie das folgende C#-Muster, um die Datenbindung auszuführen.

```cs
using (var output = await binder.BindAsync<T>(new BindingTypeAttribute(...)))
{
    ...
}
```

Dabei ist `BindingTypeAttribute` das .NET-Attribut, das die Bindung definiert, und `T` ist der Eingabe- oder Ausgabetyp, der von diesem Bindungstyp unterstützt wird. `T` darf auch kein `out`-Parametertyp sein (wie `out JObject`). Die ausgehende Bindung für die Tabelle „Mobile Apps“ unterstützt z.B. [sechs Ausgabetypen](https://github.com/Azure/azure-webjobs-sdk-extensions/blob/master/src/WebJobs.Extensions.MobileApps/MobileTableAttribute.cs#L17-L22), Sie können aber nur [ICollector<T>](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/ICollector.cs) oder [IAsyncCollector<T>](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/IAsyncCollector.cs) für `T` verwenden.

Mit dem folgenden Beispielcode wird eine [ausgehende Speicherblob-Bindung](functions-bindings-storage-blob.md#using-a-blob-output-binding) mit einem Blobpfad erstellt, der zur Laufzeit definiert wird. Dann wird eine Zeichenfolge in das Blob geschrieben.

```cs
using Microsoft.Azure.WebJobs;
using Microsoft.Azure.WebJobs.Host.Bindings.Runtime;

public static async Task Run(string input, Binder binder)
{
    using (var writer = await binder.BindAsync<TextWriter>(new BlobAttribute("samples-output/path")))
    {
        writer.Write("Hello World!!");
    }
}
```

[BlobAttribute](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/BlobAttribute.cs) definiert die Eingabe- oder Ausgabebindung für den [Speicherblob](functions-bindings-storage-blob.md), und [TextWriter](https://msdn.microsoft.com/library/system.io.textwriter.aspx) ist ein unterstützter Ausgabenbindungstyp.
So wie gezeigt ruft der Code die Standard-App-Einstellung für die Speicherkonto-Verbindungszeichenfolge ab (also `AzureWebJobsStorage`). Sie können eine zu verwendende benutzerdefinierte App-Einstellung angeben, indem Sie [StorageAccountAttribute](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/StorageAccountAttribute.cs) hinzufügen und das Attributarray an `BindAsync<T>()` übergeben. Beispiel:

```cs
using Microsoft.Azure.WebJobs;
using Microsoft.Azure.WebJobs.Host.Bindings.Runtime;

public static async Task Run(string input, Binder binder)
{
    var attributes = new Attribute[]
    {    
        new BlobAttribute("samples-output/path"),
        new StorageAccountAttribute("MyStorageAccount")
    };

    using (var writer = await binder.BindAsync<TextWriter>(attributes))
    {
        writer.Write("Hello World!");
    }
}
```

In der folgenden Tabelle sind die .NET-Attribute für jeden Bindungstyp und die Pakete aufgelistet, in denen sie definiert sind.

> [!div class="mx-codeBreakAll"]
| Bindung | Attribut | Hinzuzufügender Verweis |
|------|------|------|
| Cosmos DB | [`Microsoft.Azure.WebJobs.DocumentDBAttribute`](https://github.com/Azure/azure-webjobs-sdk-extensions/blob/master/src/WebJobs.Extensions.DocumentDB/DocumentDBAttribute.cs) | `#r "Microsoft.Azure.WebJobs.Extensions.DocumentDB"` |
| Event Hubs | [`Microsoft.Azure.WebJobs.ServiceBus.EventHubAttribute`](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs.ServiceBus/EventHubs/EventHubAttribute.cs), [`Microsoft.Azure.WebJobs.ServiceBusAccountAttribute`](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs.ServiceBus/ServiceBusAccountAttribute.cs) | `#r "Microsoft.Azure.Jobs.ServiceBus"` |
| Mobile Apps | [`Microsoft.Azure.WebJobs.MobileTableAttribute`](https://github.com/Azure/azure-webjobs-sdk-extensions/blob/master/src/WebJobs.Extensions.MobileApps/MobileTableAttribute.cs) | `#r "Microsoft.Azure.WebJobs.Extensions.MobileApps"` |
| Notification Hubs | [`Microsoft.Azure.WebJobs.NotificationHubAttribute`](https://github.com/Azure/azure-webjobs-sdk-extensions/blob/master/src/WebJobs.Extensions.NotificationHubs/NotificationHubAttribute.cs) | `#r "Microsoft.Azure.WebJobs.Extensions.NotificationHubs"` |
| Service Bus | [`Microsoft.Azure.WebJobs.ServiceBusAttribute`](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs.ServiceBus/ServiceBusAttribute.cs), [`Microsoft.Azure.WebJobs.ServiceBusAccountAttribute`](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs.ServiceBus/ServiceBusAccountAttribute.cs) | `#r "Microsoft.Azure.WebJobs.ServiceBus"` |
| Speicherwarteschlange | [`Microsoft.Azure.WebJobs.QueueAttribute`](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/QueueAttribute.cs), [`Microsoft.Azure.WebJobs.StorageAccountAttribute`](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/StorageAccountAttribute.cs) | |
| Speicherblob | [`Microsoft.Azure.WebJobs.BlobAttribute`](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/BlobAttribute.cs), [`Microsoft.Azure.WebJobs.StorageAccountAttribute`](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/StorageAccountAttribute.cs) | |
| Speichertabelle | [`Microsoft.Azure.WebJobs.TableAttribute`](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/TableAttribute.cs), [`Microsoft.Azure.WebJobs.StorageAccountAttribute`](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/StorageAccountAttribute.cs) | |
| Twilio | [`Microsoft.Azure.WebJobs.TwilioSmsAttribute`](https://github.com/Azure/azure-webjobs-sdk-extensions/blob/master/src/WebJobs.Extensions.Twilio/TwilioSMSAttribute.cs) | `#r "Microsoft.Azure.WebJobs.Extensions.Twilio"` |



## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen finden Sie in den folgenden Ressourcen:

* [Bewährte Methoden für Azure Functions](functions-best-practices.md)
* [Azure Functions developer reference (Azure Functions-Entwicklerreferenz) (Azure Functions-Entwicklerreferenz)](functions-reference.md)
* [F#-Entwicklerreferenz zu Azure Functions](functions-reference-fsharp.md)
* [NodeJS-Entwicklerreferenz zu Azure Functions](functions-reference-node.md)
* [Trigger und Bindungen in Azure Functions](functions-triggers-bindings.md)

[host\.json]: https://github.com/Azure/azure-webjobs-sdk-script/wiki/host.json
