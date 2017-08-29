---
title: Verwenden von .NET-Klassenbibliotheken mit Azure Functions | Microsoft-Dokumentation
description: "Erfahren Sie, wie Sie .NET-Klassenbibliotheken für die Verwendung mit Azure Functions erstellen."
services: functions
documentationcenter: na
author: lindydonna
manager: cfowler
editor: 
tags: 
keywords: Azure Functions, Funktionen, Ereignisverarbeitung, dynamisches Compute, serverlose Architektur
ms.assetid: 9f5db0c2-a88e-4fa8-9b59-37a7096fc828
ms.service: functions
ms.devlang: multiple
ms.topic: reference
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 06/09/2017
ms.author: donnam
ms.translationtype: HT
ms.sourcegitcommit: 137671152878e6e1ee5ba398dd5267feefc435b7
ms.openlocfilehash: 0613bb96d3afb85ff7e684246b128e4eef518d23
ms.contentlocale: de-de
ms.lasthandoff: 07/28/2017

---
# <a name="using-net-class-libraries-with-azure-functions"></a>Verwenden von .NET-Klassenbibliotheken mit Azure Functions

Neben Skriptdateien unterstützt Azure Functions die Veröffentlichung einer Klassenbibliothek wie die Implementierung für eine oder mehrere Funktionen. Wir empfehlen Ihnen die Verwendung der [Azure Functions Visual Studio 2017 Tools](https://blogs.msdn.microsoft.com/webdev/2017/05/10/azure-function-tools-for-visual-studio-2017/).

## <a name="prerequisites"></a>Voraussetzungen 

Für diesen Artikel ist Folgendes erforderlich:

- [Visual Studio 2017 15.3 – Vorschau](https://www.visualstudio.com/vs/preview/). Installieren Sie die Workloads **ASP.NET und Webentwicklung** sowie **Azure-Entwicklung**.
- [Azure Functions-Tools für Visual Studio 2017](https://marketplace.visualstudio.com/items?itemName=AndrewBHall-MSFT.AzureFunctionToolsforVisualStudio2017)

## <a name="functions-class-library-project"></a>Funktionsklassenbibliotheks-Projekt

Erstellen Sie in Visual Studio ein Azure Functions-Projekt. Die neue Projektvorlage erstellt die Dateien *host.json* und *local.settings.json*. Sie können [Azure Functions-Laufzeiteinstellungen in „host.json“ anpassen](https://github.com/Azure/azure-webjobs-sdk-script/wiki/host.json). 

Die Datei *local.settings.json* speichert App-Einstellungen, Verbindungszeichenfolgen und Einstellungen für Azure Functions Core Tools. Weitere Informationen zu ihrer Struktur finden Sie unter [Lokales Codieren und Testen von Azure-Funktionen](functions-run-local.md#local-settings).

### <a name="functionname-attribute"></a>FunctionName-Attribut

Das Attribut [`FunctionNameAttribute`](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/FunctionNameAttribute.cs) kennzeichnet eine Methode als Funktionseinstiegspunkt. Es muss mit genau einem Trigger und 0 oder mehr Ein- und Ausgabebindungen verwendet werden.

### <a name="conversion-to-functionjson"></a>Konvertierung in „function.json“

Beim Erstellen eines Azure Functions-Projekts wird eine Datei `function.json` in dem Verzeichnis erzeugt, das dem in `[FunctionName]` definierten Funktionsnamen entspricht. Sie gibt Trigger, Bindungen und Punkte für die Projektassemblydatei an.

Das NuGet-Paket [Microsoft\.NET\.Sdk\.Functions](http://www.nuget.org/packages/Microsoft.NET.Sdk.Functions) führt diese Konvertierung aus. Die Quelle ist im GitHub-Repository [azure\-functions\-vs\-build\-sdk](https://github.com/Azure/azure-functions-vs-build-sdk) verfügbar.

## <a name="triggers-and-bindings"></a>Trigger und Bindungen

Die folgende Tabelle enthält die Trigger und Bindungen, die in einem Azure Functions-Klassenbibliotheksprojekt verfügbar sind. Alle Attribute sind im Namespace `Microsoft.Azure.WebJobs`.

| Bindung | Attribut | NuGet-Paket |
|------   | ------    | ------        |
| [Blobspeichertrigger, Eingabe, Ausgabe](#blob-storage) | [BlobAttribute], [StorageAccountAttribute] | [Microsoft.Azure.WebJobs] | [Blobspeicher] |
| [Cosmos DB-Eingabe- und Ausgabebindung](#cosmos-db) | [DocumentDBAttribute] | [Microsoft.Azure.WebJobs.Extensions.DocumentDB] | 
| [Event Hubs: Trigger und Ausgabe](#event-hub) | [EventHubTriggerAttribute], [EventHubAttribute] | [Microsoft.Azure.WebJobs.ServiceBus] |
| [Externe Datei: Ein- und Ausgabe](#api-hub) | [ApiHubFileAttribute] | [Microsoft.Azure.WebJobs.Extensions.ApiHub] |
| [HTTP- und Webhooktrigger](#http) | [HttpTriggerAttribute] | [Microsoft.Azure.WebJobs.Extensions.Http] |
| [Mobile Apps: Ein- und Ausgabe](#mobile-apps) | [MobileTableAttribute] | [Microsoft.Azure.WebJobs.Extensions.MobileApps] | 
| [Notification Hubs: Ausgabe](#nh) | [NotificationHubAttribute] | [Microsoft.Azure.WebJobs.Extensions.NotificationHubs] | 
| [Warteschlangenspeicher: Trigger und Ausgabe](#queue) | [QueueAttribute], [StorageAccountAttribute] | [Microsoft.Azure.WebJobs] | 
| [SendGrid-Ausgabe](#sendgrid) | [SendGridAttribute] | [Microsoft.Azure.WebJobs.Extensions.SendGrid] | 
| [Service Bus: Trigger und Ausgabe](#service-bus) | [ServiceBusAttribute], [ServiceBusAccountAttribute] | [Microsoft.Azure.WebJobs.ServiceBus]
| [Tabellenspeicher: Ein- und Ausgabe](#table) | [TableAttribute], [StorageAccountAttribute] | [Microsoft.Azure.WebJobs] | 
| [Trigger mit Timer](#timer) | [TimerTriggerAttribute] | [Microsoft.Azure.WebJobs.Extensions] | 
| [Twilio: Ausgabe](#twilio) | [TwilioSmsAttribute] | [Microsoft.Azure.WebJobs.Extensions.Twilio] | 

<a name="blob-storage"></a>

### <a name="blob-storage-trigger-input-and-output-bindings"></a>Blobspeicher: Trigger, Eingabe- und Ausgabebindungen

Azure Functions unterstützt Trigger, Ein- und Ausgabebindungen für Azure Blob Storage. Weitere Informationen zu Bindungsausdrücken und Metadaten finden Sie unter [Azure Functions – Blob Storage-Bindungen](functions-bindings-storage-blob.md).

Ein Blobtrigger wird mit dem `[BlobTrigger]`-Attribut definiert. Sie können mit dem Attribut `[StorageAccount]` das Speicherkonto definieren, das von einer gesamten Funktion oder Klasse verwendet wird.

```csharp
[StorageAccount("AzureWebJobsStorage")]
[FunctionName("BlobTriggerCSharp")]        
public static void Run([BlobTrigger("samples-workitems/{name}")] Stream myBlob, string name, TraceWriter log)
{
    log.Info($"C# Blob trigger function Processed blob\n Name:{name} \n Size: {myBlob.Length} Bytes");
}
```

Blobeingabe und -ausgabe werden mit dem `[Blob]`-Attribut definiert, zusammen mit einem `FileAccess`-Parameter, der das Lese- oder Schreibrecht anzeigt. Im folgenden Beispiel werden ein Blobtrigger und eine Blobausgabebindung verwendet.

```csharp
[FunctionName("ResizeImage")]
[StorageAccount("AzureWebJobsStorage")]
public static void Run(
    [BlobTrigger("sample-images/{name}")] Stream image, 
    [Blob("sample-images-sm/{name}", FileAccess.Write)] Stream imageSmall, 
    [Blob("sample-images-md/{name}", FileAccess.Write)] Stream imageMedium)
{
    var imageBuilder = ImageResizer.ImageBuilder.Current;
    var size = imageDimensionsTable[ImageSize.Small];

    imageBuilder.Build(image, imageSmall,
        new ResizeSettings(size.Item1, size.Item2, FitMode.Max, null), false);

    image.Position = 0;
    size = imageDimensionsTable[ImageSize.Medium];

    imageBuilder.Build(image, imageMedium,
        new ResizeSettings(size.Item1, size.Item2, FitMode.Max, null), false);
}

public enum ImageSize { ExtraSmall, Small, Medium }

private static Dictionary<ImageSize, (int, int)> imageDimensionsTable = new Dictionary<ImageSize, (int, int)>() {
    { ImageSize.ExtraSmall, (320, 200) },
    { ImageSize.Small,      (640, 400) },
    { ImageSize.Medium,     (800, 600) }
};
```        

<a name="cosmos-db"></a>

### <a name="cosmos-db-input-and-output-bindings"></a>Cosmos DB-Eingabe- und Ausgabebindungen

Azure Functions unterstützt Eingabe- und Ausgabebindungen für Cosmos DB. Weitere Informationen zu den Funktionen der Cosmos-DB-Bindung finden Sie unter [Cosmos DB-Bindungen in Azure Functions](functions-bindings-documentdb.md).

Verwenden Sie zur Bindung an ein Cosmos-DB-Dokument das Attribut `[DocumentDB]` im NuGet-Paket [Microsoft.Azure.WebJobs.Extensions.DocumentDB]. Das folgende Beispiel enthält einen Warteschlangentrigger und eine DocumentDB-API-Ausgabebindung:

```csharp
[FunctionName("QueueToDocDB")]        
public static void Run(
    [QueueTrigger("myqueue-items", Connection = "AzureWebJobsStorage")] string myQueueItem, 
    [DocumentDB("ToDoList", "Items", ConnectionStringSetting = "DocDBConnection")] out dynamic document)
{
    document = new { Text = myQueueItem, id = Guid.NewGuid() };
}

```

<a name="event-hub"></a>

### <a name="event-hubs-trigger-and-output"></a>Event Hubs: Trigger und Ausgabe

Azure Functions unterstützt Trigger- und Ausgabebindungen für Event Hubs. Weitere Informationen finden Sie unter [Event Hub-Bindungen für Azure Functions](functions-bindings-event-hubs.md).

Die Typen `[Microsoft.Azure.WebJobs.ServiceBus.EventHubTriggerAttribute]` und `[Microsoft.Azure.WebJobs.ServiceBus.EventHubAttribute]` werden im NuGet-Paket [Microsoft.Azure.WebJobs.ServiceBus] definiert. 

Im folgenden Beispiel wird ein Event Hub-Trigger verwendet:

```csharp
[FunctionName("EventHubTriggerCSharp")]
public static void Run([EventHubTrigger("samples-workitems", Connection = "EventHubConnection")] string myEventHubMessage, TraceWriter log)
{
    log.Info($"C# Event Hub trigger function processed a message: {myEventHubMessage}");
}
```

Das folgende Beispiel enthält eine Event Hub-Ausgabe, wobei der Methodenrückgabewert als Ausgabe verwendet wird:

```csharp
[FunctionName("EventHubOutput")]
[return: EventHub("outputEventHubMessage", Connection = "EventHubConnection")]
public static string Run([TimerTrigger("0 */5 * * * *")] TimerInfo myTimer, TraceWriter log)
{
    log.Info($"C# Timer trigger function executed at: {DateTime.Now}");
    return $"{DateTime.Now}";
}
```

<a name="api-hub"></a>

### <a name="external-file-input-and-output"></a>Externe Datei: Ein- und Ausgabe

Azure Functions unterstützt Trigger-, Eingabe- und Ausgabebindungen für externe Dateien wie Google Drive, Dropbox und OneDrive. Weitere Informationen finden Sie unter [Azure Functions – Bindungen zu externen Dateien (Vorschau)](functions-bindings-external-file.md). Die Attribute `[ExternalFileTrigger]` und `[ExternalFile]` werden im NuGet-Paket [Microsoft.Azure.WebJobs.Extensions.ApiHub] definiert.

Das folgende C#-Beispiel veranschaulicht eine Eingabe- und Ausgabebindung zu einer externen Datei. Der Code kopiert die Eingabedatei in die Ausgabedatei.

```csharp
[StorageAccount("MyStorageConnection")]
[FunctionName("ExternalFile")]
[return: ApiHubFile("MyFileConnection", "samples-workitems/{queueTrigger}-Copy", FileAccess.Write)]
public static string Run([QueueTrigger("myqueue-items")] string myQueueItem, 
    [ApiHubFile("MyFileConnection", "samples-workitems/{queueTrigger}", FileAccess.Read)] string myInputFile, 
    TraceWriter log)
{
    log.Info($"C# Queue trigger function processed: {myQueueItem}");
    return myInputFile;
}
```

<a name="http"></a>

### <a name="http-and-webhooks"></a>HTTP und Webhooks

Verwenden Sie das `HttpTrigger`-Attribut zum Definieren eines HTTP-Triggers oder Webhooks. Dieses Attribut wird im NuGet-Paket [Microsoft.Azure.WebJobs.Extensions.Http] definiert. Sie können Autorisierungsstufe, Webhooktyp, Route und Methoden anpassen. Das folgende Beispiel definiert einen HTTP-Trigger mit anonymer Authentifizierung und Webhooktyp _genericJson_.

```csharp
[FunctionName("HttpTriggerCSharp")]
public static HttpResponseMessage Run([HttpTrigger(AuthorizationLevel.Anonymous, WebHookType = "genericJson")] HttpRequestMessage req)
{
    return req.CreateResponse(HttpStatusCode.OK);
}
```

<a name="mobile-apps"></a>

### <a name="mobile-apps-input-and-output"></a>Mobile Apps: Ein- und Ausgabe

Azure Functions unterstützt Eingabe- und Ausgabebindungen für Mobile Apps. Weitere Informationen finden Sie unter [Mobile Apps-Bindungen in Azure Functions](functions-bindings-mobile-apps.md). Das Attribut `[MobileTable]` wird im NuGet-Paket [Microsoft.Azure.WebJobs.Extensions.MobileApps] definiert.

Das folgende Beispiel zeigt eine Mobile Apps-Ausgabebindung:

```csharp
[FunctionName("MobileAppsOutput")]        
[return: MobileTable(ApiKeySetting = "MyMobileAppKey", TableName = "MyTable", MobileAppUriSetting = "MyMobileAppUri")]
public static object Run([QueueTrigger("myqueue-items", Connection = "AzureWebJobsStorage")] string myQueueItem, TraceWriter log)
{
    return new { Text = $"I'm running in a C# function! {myQueueItem}" };
}
```

<a name="nh"></a>

### <a name="notification-hubs-output"></a>Notification Hubs: Ausgabe

Azure Functions unterstützt eine Ausgabebindung für Notification Hubs. Weitere Informationen finden Sie unter [Azure Functions Notification Hub-Ausgabebindung](functions-bindings-notification-hubs.md). Das Attribut `[NotificationHub]` wird im NuGet-Paket [Microsoft.Azure.WebJobs.Extensions.NotificationHubs] definiert.

<a name="queue"></a>

### <a name="queue-storage-trigger-and-output"></a>Warteschlangenspeicher: Trigger und Ausgabe

Azure Functions unterstützt Trigger- und Ausgabebindungen für Azure-Warteschlangen. Weitere Informationen finden Sie unter [Azure Storage-Warteschlangenbindungen in Azure Functions](functions-bindings-storage-queue.md).

Im folgenden Beispiel wird gezeigt, wie der Funktionsrückgabetyp mithilfe des `[Queue]`-Attributs mit der Warteschlangenausgaben-Bindung verwendet wird. Verwenden Sie zum Definieren eines Triggers für die Warteschlange das `[QueueTrigger]`-Attribut.

```csharp
[StorageAccount("AzureWebJobsStorage")]
public static class QueueFunctions
{
    // HTTP trigger with queue output binding
    [FunctionName("QueueOutput")]
    [return: Queue("myqueue-items")]
    public static string QueueOutput([HttpTrigger] dynamic input,  TraceWriter log)
    {
        log.Info($"C# function processed: {input.Text}");
        return input.Text;
    }

    // Queue trigger
    [FunctionName("QueueTrigger")]
    [StorageAccount("AzureWebJobsStorage")]
    public static void QueueTrigger([QueueTrigger("myqueue-items")] string myQueueItem, TraceWriter log)
    {
        log.Info($"C# function processed: {myQueueItem}");
    }
}

```

<a name="sendgrid"></a>

### <a name="sendgrid-output"></a>SendGrid-Ausgabe

Azure Functions unterstützt eine SendGrid-Ausgabebindung für das programmgesteuerte Senden von E-Mail. Weitere Informationen finden Sie unter [SendGrid-Bindungen in Azure Functions](functions-bindings-sendgrid.md).

Das Attribut `[SendGrid]` wird im NuGet-Paket [Microsoft.Azure.WebJobs.Extensions.SendGrid] definiert.

Das folgende Beispiel zeigt die Verwendung eines Service Bus-Warteschlangentriggers und einer SendGrid-Ausgabebindung mit `SendGridMessage`:

```csharp
[FunctionName("SendEmail")]
public static void Run(
    [ServiceBusTrigger("myqueue", AccessRights.Manage, Connection = "ServiceBusConnection")] OutgoingEmail email,
    [SendGrid] out SendGridMessage message)
{
    message = new SendGridMessage();
    message.AddTo(email.To);
    message.AddContent("text/html", email.Body);
    message.SetFrom(new EmailAddress(email.From));
    message.SetSubject(email.Subject);
}

public class OutgoingEmail
{
    public string To { get; set; }
    public string From { get; set; }
    public string Subject { get; set; }
    public string Body { get; set; }
}
```

<a name="service-bus"></a>

### <a name="service-bus-trigger-and-output"></a>Service Bus: Trigger und Ausgabe

Azure Functions unterstützt Trigger und Ausgabebindungen für Service Bus-Warteschlangen und -Themen. Weitere Informationen zum Konfigurieren von Bindungen finden Sie unter [Service Bus-Bindungen von Azure Functions](functions-bindings-service-bus.md).

Die Attribute `[ServiceBusTrigger]` und `[ServiceBus]` werden im NuGet-Paket [Microsoft.Azure.WebJobs.ServiceBus] definiert. 

Im Folgenden ist ein Beispiel für einen Service Bus-Warteschlangentrigger angegeben:

```csharp
[FunctionName("ServiceBusQueueTriggerCSharp")]                    
public static void Run([ServiceBusTrigger("myqueue", AccessRights.Manage, Connection = "ServiceBusConnection")] string myQueueItem, TraceWriter log)
{
    log.Info($"C# ServiceBus queue trigger function processed message: {myQueueItem}");
}
```

Im folgenden Beispiel einer Service Bus-Ausgabebindung wird der Methodenrückgabetyp als Ausgabe verwendet:

```csharp
[FunctionName("ServiceBusOutput")]
[return: ServiceBus("myqueue", Connection = "ServiceBusConnection")]
public static string ServiceBusOutput([HttpTrigger] dynamic input, TraceWriter log)
{
    log.Info($"C# function processed: {input.Text}");
    return input.Text;
}
```        

<a name="table"></a>

### <a name="table-storage-input-and-output"></a>Tabellenspeicher: Ein- und Ausgabe

Azure Functions unterstützt Eingabe- und Ausgabebindungen für Azure-Tabellenspeicher. Weitere Informationen finden Sie unter [Azure Storage-Tabellenbindungen in Azure Functions](functions-bindings-storage-table.md).

Im folgenden Beispiel werden anhand einer Klasse mit zwei Funktionen die Ausgabe- und Eingabebindungen des Tabellenspeichers demonstriert. 

```csharp
[StorageAccount("AzureWebJobsStorage")]
public class TableStorage
{
    public class MyPoco
    {
        public string PartitionKey { get; set; }
        public string RowKey { get; set; }
        public string Text { get; set; }
    }

    [FunctionName("TableOutput")]
    [return: Table("MyTable")]
    public static MyPoco TableOutput([HttpTrigger] dynamic input, TraceWriter log)
    {
        log.Info($"C# http trigger function processed: {input.Text}");
        return new MyPoco { PartitionKey = "Http", RowKey = Guid.NewGuid().ToString(), Text = input.Text };
    }

    // use the metadata parameter "queueTrigger" to bind the queue payload
    [FunctionName("TableInput")]
    public static void TableInput([QueueTrigger("table-items")] string input, [Table("MyTable", "Http", "{queueTrigger}")] MyPoco poco, TraceWriter log)
    {
        log.Info($"C# function processed: {poco.Text}");
    }
}

```

<a name="timer"></a>

### <a name="timer-trigger"></a>Trigger mit Timer

Azure Functions hat eine Timertriggerbindung, mit der Sie Ihren Funktionscode auf Basis eines definierten Zeitplans ausführen können. Weitere Informationen zu den Funktionen der Bindung finden Sie unter [Planen der Ausführung von Code mit Azure Functions](functions-bindings-timer.md).

Im Verbrauchsplan können Sie Zeitpläne mit einem [CRON-Ausdruck](http://en.wikipedia.org/wiki/Cron#CRON_expression) definieren. Wenn Sie einen App Service-Plan verwenden, können Sie auch eine TimeSpan-Zeichenfolge einsetzen. 

Das folgende Beispiel definiert einen Timertrigger, der alle 5 Minuten ausgeführt wird:

```csharp
[FunctionName("TimerTriggerCSharp")]
public static void Run([TimerTrigger("0 */5 * * * *")]TimerInfo myTimer, TraceWriter log)
{
    log.Info($"C# Timer trigger function executed at: {DateTime.Now}");
}
```

<a name="twilio"></a>

### <a name="twilio-output"></a>Twilio: Ausgabe

Azure Functions unterstützt Twilio-Ausgabebindungen, mit denen Sie in Ihren Funktionen das Senden von SMS-Textnachrichten aktivieren können. Weitere Informationen finden Sie unter [Senden von SMS-Nachrichten mit Azure Functions mithilfe der Twilio-Ausgabebindung](functions-bindings-twilio.md). 

Das Attribut `[TwilioSms]` wird im Paket [Microsoft.Azure.WebJobs.Extensions.Twilio] definiert.

Im folgenden C#-Beispiel werden ein Warteschlangentrigger und eine Twilio-Ausgabebindung verwendet:

```csharp
[FunctionName("QueueTwilio")]
[return: TwilioSms(AccountSidSetting = "TwilioAccountSid", AuthTokenSetting = "TwilioAuthToken", From = "+1425XXXXXXX" )]
public static SMSMessage Run([QueueTrigger("myqueue-items", Connection = "AzureWebJobsStorage")] JObject order, TraceWriter log)
{
    log.Info($"C# Queue trigger function processed: {order}");

    var message = new SMSMessage()
    {
        Body = $"Hello {order["name"]}, thanks for your order!",
        To = order["mobileNumber"].ToString()
    };

    return message;
}
```

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zum Verwenden von Azure Functions in C#-Skripts finden Sie unter [C\#-Entwicklerreferenz zu Azure Functions](functions-reference-csharp.md).

[!INCLUDE [next steps](../../includes/functions-bindings-next-steps.md)]


<!-- NuGet packages --> 
[Microsoft.Azure.WebJobs]: http://www.nuget.org/packages/Microsoft.Azure.WebJobs/2.1.0-beta1
[Microsoft.Azure.WebJobs.Extensions.DocumentDB]: http://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.DocumentDB/1.1.0-beta1
[Microsoft.Azure.WebJobs.ServiceBus]: http://www.nuget.org/packages/Microsoft.Azure.WebJobs.ServiceBus/2.1.0-beta1
[Microsoft.Azure.WebJobs.Extensions.MobileApps]: http://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.MobileApps/1.1.0-beta1
[Microsoft.Azure.WebJobs.Extensions.NotificationHubs]: http://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.NotificationHubs/1.1.0-beta1
[Microsoft.Azure.WebJobs.ServiceBus]: http://www.nuget.org/packages/Microsoft.Azure.WebJobs.ServiceBus/2.1.0-beta1
[Microsoft.Azure.WebJobs.Extensions.SendGrid]: http://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.SendGrid/2.1.0-beta1
[Microsoft.Azure.WebJobs.Extensions.Http]: http://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.Http/1.0.0-beta1
[Microsoft.Azure.WebJobs.Extensions.BotFramework]: http://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.BotFramework/1.0.15-beta
[Microsoft.Azure.WebJobs.Extensions.ApiHub]: http://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.ApiHub/1.0.0-beta4
[Microsoft.Azure.WebJobs.Extensions.Twilio]: http://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.Twilio/1.1.0-beta1
[Microsoft.Azure.WebJobs.Extensions]: http://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions/2.1.0-beta1


<!-- Links to source --> 
[DocumentDBAttribute]: https://github.com/Azure/azure-webjobs-sdk-extensions/blob/master/src/WebJobs.Extensions.DocumentDB/DocumentDBAttribute.cs
[EventHubAttribute]: https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs.ServiceBus/EventHubs/EventHubAttribute.cs
[EventHubTriggerAttribute]: https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs.ServiceBus/EventHubs/EventHubTriggerAttribute.cs
[MobileTableAttribute]: https://github.com/Azure/azure-webjobs-sdk-extensions/blob/master/src/WebJobs.Extensions.MobileApps/MobileTableAttribute.cs
[NotificationHubAttribute]: https://github.com/Azure/azure-webjobs-sdk-extensions/blob/master/src/WebJobs.Extensions.NotificationHubs/NotificationHubAttribute.cs 
[ServiceBusAttribute]: https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs.ServiceBus/ServiceBusAttribute.cs
[ServiceBusAccountAttribute]: https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs.ServiceBus/ServiceBusAccountAttribute.cs
[QueueAttribute]: https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/QueueAttribute.cs
[StorageAccountAttribute]: https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/StorageAccountAttribute.cs
[BlobAttribute]: https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/BlobAttribute.cs
[TableAttribute]: https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/TableAttribute.cs
[TwilioSmsAttribute]: https://github.com/Azure/azure-webjobs-sdk-extensions/blob/master/src/WebJobs.Extensions.Twilio/TwilioSMSAttribute.cs
[SendGridAttribute]: https://github.com/Azure/azure-webjobs-sdk-extensions/blob/master/src/WebJobs.Extensions.SendGrid/SendGridAttribute.cs
[HttpTriggerAttribute]: https://github.com/Azure/azure-webjobs-sdk-extensions/blob/dev/src/WebJobs.Extensions.Http/HttpTriggerAttribute.cs
[ApiHubFileAttribute]: https://github.com/Azure/azure-webjobs-sdk-extensions/blob/master/src/WebJobs.Extensions.ApiHub/ApiHubFileAttribute.cs
[TimerTriggerAttribute]: https://github.com/Azure/azure-webjobs-sdk-extensions/blob/master/src/WebJobs.Extensions/Extensions/Timers/TimerTriggerAttribute.cs

