<properties
	pageTitle="F#-Entwicklerreferenz zu Azure Functions | Microsoft Azure"
	description="Es wird beschrieben, wie die Azure Functions-Entwicklung mit F# durchgeführt wird."
	services="functions"
	documentationCenter="fsharp"
	authors="sylvanc"
	manager="jbronsk"
	editor=""
	tags=""
	keywords="Azure Functions, Functions, Ereignisverarbeitung, Webhooks, dynamisches Compute, serverlose Architektur, F#"/>

<tags
	ms.service="functions"
	ms.devlang="fsharp"
	ms.topic="reference"
	ms.tgt_pltfrm="multiple"
	ms.workload="na"
	ms.date="09/09/2016"
	ms.author="syclebsc"/>

# F#-Entwicklerreferenz zu Azure Functions

> [AZURE.SELECTOR]
- [C#-Skript](../articles/azure-functions/functions-reference-csharp.md)
- [F#-Skript](../articles/azure-functions/functions-reference-fsharp.md)
- [Node.js](../articles/azure-functions/functions-reference-node.md)

F# für Azure Functions ist eine Lösung, mit der Sie ganz einfach kleinere Codeelemente (Funktionen) in der Cloud ausführen können. Daten fließen über Funktionsargumente in Ihre F#-Funktion. Argumentnamen werden in `function.json` angegeben, und es gibt vordefinierte Namen für den Zugriff auf Elemente wie die Funktionsprotokollierung und Abbruchtoken.

In diesem Artikel wird davon ausgegangen, dass Sie bereits die [Entwicklerreferenz zu Azure Functions](functions-reference.md) gelesen haben.

## Funktionsweise von „.fsx“

Eine `.fsx`-Datei ist ein F#-Skript. Sie können sich dies wie ein F#-Projekt vorstellen, das in einer einzelnen Datei enthalten ist. Die Datei enthält sowohl den Code für Ihr Programm (in diesem Fall Ihre Azure-Funktion) als auch Direktiven zum Verwalten von Abhängigkeiten.

Bei Verwendung der Endung `.fsx` für eine Azure-Funktion werden die üblicherweise erforderlichen Assemblys automatisch für Sie eingebunden, sodass Sie sich auf die Funktion konzentrieren können und sich nicht mit Codebausteinen beschäftigen müssen.

## Binden an Argumente

Jede Bindung unterstützt eine Gruppe von Argumenten. Dies ist in der [Entwicklerreferenz zu Triggern und Bindungen in Azure Functions](functions-triggers-bindings.md) ausführlich beschrieben. Eine Argumentbindung, die von einem Blobtrigger beispielsweise unterstützt wird, ist ein POCO-Element. Dies kann mit einem F#-Eintrag ausgedrückt werden. Beispiel:

```fsharp
type Item = { Id: string }

let Run(blob: string, output: byref<Item>) =
    let item = { Id = "Some ID" }
    output <- item
```

Für Ihre F#-Azure-Funktion wird mindestens ein Argument verwendet. Wenn wir von Azure Functions-Argumenten sprechen, geht es um Argumente für die _Eingabe_ und Argumente für die _Ausgabe_. Bei einem Eingabeargument geht es genau um das, was der Name vermuten lässt: die Eingabe in Ihre F#-Azure-Funktion. Bei einem _Ausgabe_argument geht es um änderbare Daten oder ein `byref<>`-Argument, das als Möglichkeit zum Zurückgeben von Daten _aus_ Ihrer Funktion dient.

Im obigen Beispiel ist `blob` ein Eingabeargument und `output` ein Ausgabeargument. Beachten Sie, dass wir `byref<>` für `output` verwendet haben (das Hinzufügen der Anmerkung `[<Out>]` ist nicht erforderlich). Mit dem Typ `byref<>` kann Ihre Funktion ändern, auf welchen Eintrag oder welches Objekt das Argument verweist.

Wenn ein F#-Eintrag als Eingabetyp verwendet wird, muss die Eintragsdefinition mit `[<CLIMutable>]` gekennzeichnet werden. So kann das Azure Functions-Framework die Felder richtig festlegen, bevor der Eintrag an Ihre Funktion übergeben wird. Im Hintergrund generiert `[<CLIMutable>]` Setter für die Eintragseigenschaften. Beispiel:

```fsharp
[<CLIMutable>]
type TestObject =
    { SenderName : string
      Greeting : string }

let Run(req: TestObject, log: TraceWriter) =
    { req with Greeting = sprintf "Hello, %s" req.SenderName }
```

Eine F#-Klasse kann auch für in- und out-Argumente verwendet werden. Für eine Klasse benötigen Eigenschaften normalerweise „Getter“ und „Setter“. Beispiel:

```fsharp
type Item() =
    member val Id = "" with get,set
    member val Text = "" with get,set

let Run(input: string, item: byref<Item>) =
    let result = Item(Id = input, Text = "Hello from F#!")
    item <- result
```

## Protokollierung

Zum Protokollieren der Ausgabe in Ihren [Streamingprotokollen](../app-service-web/web-sites-streaming-logs-and-console.md) in F# sollte Ihre Funktion ein Argument vom Typ `TraceWriter` verwenden. Der Einheitlichkeit halber empfehlen wir Ihnen, diesem Argument den Namen `log` zu geben. Beispiel:

```fsharp
let Run(blob: string, output: byref<string>, log: TraceWriter) =
    log.Verbose(sprintf "F# Azure Function processed a blob: %s" blob)
    output <- input
```

## Async

Der `async`-Workflow kann verwendet werden, aber vom Ergebnis muss ein `Task`-Element zurückgegeben werden. Hierfür können Sie `Async.StartAsTask` verwenden, z.B.:

```fsharp
let Run(req: HttpRequestMessage) =
    async {
        return new HttpResponseMessage(HttpStatusCode.OK)
    } |> Async.StartAsTask
```

## Abbruchtoken

Wenn Ihre Funktion das ordnungsgemäße Herunterfahren durchführen soll, können Sie sie mit dem Argument [`CancellationToken`](https://msdn.microsoft.com/library/system.threading.cancellationtoken.aspx) versehen. Dies können Sie mit `async` kombinieren, z.B.:

```fsharp
let Run(req: HttpRequestMessage, token: CancellationToken)
    let f = async {
        do! Async.Sleep(10)
        return new HttpResponseMessage(HttpStatusCode.OK)
    }
    Async.StartAsTask(f, token)
```

## Importieren von Namespaces

Namespaces können auf die übliche Weise geöffnet werden:

```fsharp
open System.Net
open System.Threading.Tasks

let Run(req: HttpRequestMessage, log: TraceWriter) =
    ...
```

Die folgenden Namespaces werden automatisch geöffnet:

* `System`
* `System.Collections.Generic`
* `System.IO`
* `System.Linq`
* `System.Net.Http`
* `System.Threading.Tasks`
* `Microsoft.Azure.WebJobs`
* `Microsoft.Azure.WebJobs.Host`.

## Verweise auf externe Assemblys

Auf ähnliche Weise werden Framework-Assemblyverweise mit der `#r "AssemblyName"`-Direktive hinzugefügt.

```fsharp
#r "System.Web.Http"

open System.Net
open System.Net.Http
open System.Threading.Tasks

let Run(req: HttpRequestMessage, log: TraceWriter) =
    ...
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

Darüber hinaus sind die folgenden Assemblys besondere Fälle, die über „SimpleName“ referenziert werden können (z.B. `#r "AssemblyName"`):

* `Newtonsoft.Json`
* `Microsoft.WindowsAzure.Storage`
* `Microsoft.ServiceBus`
* `Microsoft.AspNet.WebHooks.Receivers`
* `Microsoft.AspNEt.WebHooks.Common`.

Wenn Sie auf eine private Assembly verweisen müssen, können Sie die Assemblydatei in einen `bin`-Ordner relativ zu Ihrer Funktion hochladen und anhand des Dateinamens (z.B. `#r "MyAssembly.dll"`) darauf verweisen. Informationen zum Hochladen von Dateien in Ihren Funktionenordner finden Sie im folgenden Abschnitt zur Paketverwaltung.

## Editor-Einleitung

Ein Editor, der F#-Compilerdienste unterstützt, besitzt keine Informationen zu den Namespaces und Assemblys, die in Azure Functions automatisch enthalten sind. Daher kann es nützlich sein, eine Einleitung einzufügen, die dem Editor als Unterstützung beim Suchen nach den von Ihnen verwendeten Assemblys und beim expliziten Öffnen von Namespaces dient. Beispiel:

```fsharp
#if !COMPILED
#I "../../bin/Binaries/WebJobs.Script.Host"
#r "Microsoft.Azure.WebJobs.Host.dll"
#endif

open Sytem
open Microsoft.Azure.WebJobs.Host

let Run(blob: string, output: byref<string>, log: TraceWriter) =
    ...
```

Wenn Ihr Code von Azure Functions ausgeführt wird, wird die Quelle mit der Definition `COMPILED` verarbeitet, und die Editor-Einleitung wird ignoriert.

## Paketverwaltung

Um NuGet-Pakete in einer F#-Funktion zu verwenden, fügen Sie im Funktionsordner im Dateisystem der Funktionen-App die Datei `project.json` hinzu. Hier ist eine `project.json`-Beispieldatei angegeben, mit der ein NuGet-Paketverweis auf `Microsoft.ProjectOxford.Face` Version 1.1.0 hinzugefügt wird:

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

Nur .NET Framework 4.6 wird unterstützt. Stellen Sie also sicher, dass für Ihre Datei `project.json` wie hier gezeigt `net46` angegeben wird.

Beim Hochladen der Datei `project.json` erhält die Laufzeit die Pakete und fügt automatisch Verweise auf die Paketassemblys hinzu. Sie müssen keine `#r "AssemblyName"`-Direktiven hinzufügen. Fügen Sie die erforderlichen `open`-Anweisungen einfach der `.fsx`-Datei hinzu.

Es kann ratsam sein, automatische Verweisassemblys in die Editor-Einleitung einzufügen, um die Interaktion des Editors mit den F#-Kompilierdiensten zu verbessern.

### Hinzufügen der Datei `project.json` zur Azure-Funktion

1. Stellen Sie zunächst sicher, dass Ihre Funktionen-App ausgeführt wird. Öffnen Sie zu diesem Zweck Ihre Funktion im Azure-Portal. Dies ermöglicht auch Zugriff auf die Streamingprotokolle, in denen die Ausgabe der Paketinstallation angezeigt wird.

2. Verwenden Sie zum Hochladen der Datei `project.json` eines der Verfahren, die unter [Aktualisieren von Funktionen-App-Dateien](functions-reference.md#fileupdate) beschrieben sind. Wenn Sie [Continuous Deployment für Azure Functions](functions-continuous-deployment.md) verwenden, können Sie Ihrer Stagingverzweigung die Datei `project.json` hinzufügen, um damit zu experimentieren, bevor Sie sie der Bereitstellungsverzweigung hinzufügen.

3. Nach dem Hinzufügen der Datei `project.json` wird eine Ausgabe angezeigt, die dem folgenden Beispiel im Streamingprotokoll Ihrer Funktion ähnelt:

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

Verwenden Sie `System.Environment.GetEnvironmentVariable`, um eine Umgebungsvariable oder einen App-Einstellungswert abzurufen, z.B.:

```fsharp
open System.Environment

let Run(timer: TimerInfo, log: TraceWriter) =
    log.Info("Storage = " + GetEnvironmentVariable("AzureWebJobsStorage"))
    log.Info("Site = " + GetEnvironmentVariable("WEBSITE_SITE_NAME"))
```

## Wiederverwenden von .fsx-Code

Sie können Code aus anderen `.fsx`-Dateien verwenden, indem Sie eine `#load`-Direktive nutzen. Beispiel:

`run.fsx`

```fsharp
#load "logger.fsx"

let Run(timer: TimerInfo, log: TraceWriter) =
    mylog log (sprintf "Timer: %s" DateTime.Now.ToString())
```

`logger.fsx`

```fsharp
let mylog(log: TraceWriter, text: string) =
    log.Verbose(text);
```

Die Pfade zur `#load`-Direktive gelten relativ zum Speicherort Ihrer `.fsx`-Datei.

* `#load "logger.fsx"` lädt eine Datei, die sich im Funktionsordner befindet.

* `#load "package\logger.fsx"` lädt eine Datei, die sich im Ordner `package` im Funktionsordner befindet.

* `#load "..\shared\mylogger.fsx"` lädt eine Datei, die sich im Ordner `shared` auf derselben Ebene wie der Funktionsordner direkt unter `wwwroot` befindet.

Die `#load`-Direktive funktioniert nur mit `.fsx`-Dateien (F#-Skripts) und nicht mit `.fs`-Dateien.

## Nächste Schritte

Weitere Informationen finden Sie in den folgenden Ressourcen:

* [F# Guide](https://docs.microsoft.com/de-DE/dotnet/articles/fsharp/index) (F#-Leitfaden)
* [Entwicklerreferenz zu Azure Functions](functions-reference.md)
* [C#-Entwicklerreferenz zu Azure Functions](functions-reference-csharp.md)
* [NodeJS-Entwicklerreferenz zu Azure Functions](functions-reference-node.md)
* [Trigger und Bindungen in Azure Functions](functions-triggers-bindings.md)
* [Azure Functions-Tests](functions-test-a-function.md)
* [Azure Functions-Skalierung](functions-scale.md)

<!---HONumber=AcomDC_0921_2016-->