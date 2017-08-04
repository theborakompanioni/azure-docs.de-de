---
title: "Nachverfolgen von benutzerdefinierten Vorgängen mit dem Azure Application Insights .NET-SDK | Microsoft-Dokumentation"
description: "Nachverfolgen von benutzerdefinierten Vorgängen mit dem Azure Application Insights .NET-SDK"
services: application-insights
documentationcenter: .net
author: SergeyKanzhelev
manager: carmonm
ms.service: application-insights
ms.workload: TBD
ms.tgt_pltfrm: ibiza
ms.devlang: multiple
ms.topic: article
ms.date: 06/31/2017
ms.author: sergkanz
ms.translationtype: HT
ms.sourcegitcommit: 54454e98a2c37736407bdac953fdfe74e9e24d37
ms.openlocfilehash: 883750c939aa3bd605189f513e5ce74642f91709
ms.contentlocale: de-de
ms.lasthandoff: 07/13/2017

---

# <a name="tracking-custom-operations-with-application-insights-net-sdk"></a>Nachverfolgen von benutzerdefinierten Vorgängen mit dem Application Insights .NET-SDK

Mit Application Insights-SDKs werden eingehende HTTP-Anforderungen und Aufrufe abhängiger Dienste, z.B. HTTP-Anforderungen, SQL-Abfragen usw., automatisch nachverfolgt. Stellen Sie sich eine Anwendung vor, die aus mehreren Microservices besteht. Dank der Nachverfolgung und Korrelation von Anforderungen und Abhängigkeiten erhalten Sie Einblicke in die Reaktionsfähigkeit und Zuverlässigkeit der gesamten Anwendung über alle Microservices hinweg. Wir werden die Liste nach und nach erweitern und eine automatische Sammlung mit anderen bekannten Plattformen und Frameworks bereitstellen. 

Es gibt eine Klasse von Anwendungsmustern, die nicht generisch unterstützt werden kann. Für die richtige Überwachung dieser Muster ist eine manuelle Codeinstrumentierung erforderlich. In diesem Artikel werden einige Muster beschrieben, für die unter Umständen eine manuelle Instrumentierung benötigt wird. Beispiele hierfür sind die Verarbeitung einer benutzerdefinierten Warteschlange oder die Ausführung von Hintergrundaufgaben mit langer Ausführungsdauer.

Dieses Dokument enthält eine Anleitung zum Nachverfolgen von benutzerdefinierten Vorgängen per ApplicationInsights-SDK.

Das Dokument ist für folgende Versionen relevant:
- Application Insights für .NET (Basis-SDK), Version `2.4+`
- Application Insights für Webanwendungen (mit Ausführung von ASP.NET), Version `2.4+`
- Application Insights für AspNetCore, Version `2.1+`

## <a name="overview"></a>Übersicht
Mit einem Vorgang ist eine logische Arbeitsaufgabe gemeint, die von einer Anwendung durchgeführt wird. Sie verfügt über Name, Startzeit/Dauer und Ausführungskontext, z.B. Benutzername, Eigenschaften und Ergebnis. Wenn Vorgang `A` von Vorgang `B` initiiert wurde, wird Vorgang `B` als übergeordneter Vorgang von `A` festgelegt.  Ein Vorgang kann nur über einen übergeordneten Vorgang, aber über viele untergeordnete Vorgänge verfügen. Weitere Informationen zu Vorgängen und zur Telemetriekorrelation finden Sie [hier](application-insights-correlation.md).

Im Application Insights .NET-SDK wird ein Vorgang mit der abstrakten [OperationTelemetry](https://github.com/Microsoft/ApplicationInsights-dotnet/blob/develop/src/Core/Managed/Shared/Extensibility/Implementation/OperationTelemetry.cs)-Klasse und ihren Nachfolgerelementen [RequestTelemetry](https://github.com/Microsoft/ApplicationInsights-dotnet/blob/develop/src/Core/Managed/Shared/DataContracts/RequestTelemetry.cs) und [DependencyTelemetry](https://github.com/Microsoft/ApplicationInsights-dotnet/blob/develop/src/Core/Managed/Shared/DataContracts/DependencyTelemetry.cs) beschrieben.

## <a name="incoming-operations-tracking"></a>Nachverfolgen von eingehenden Vorgängen 
Das Application Insights-Web-SDK sammelt automatisch HTTP-Anforderungen für ASP.NET-Anwendungen, die in der IIS-Pipeline und allen ASP.NET Core-Anwendungen ausgeführt werden. Es sind auch Lösungen mit Community-Support für andere Plattformen und Frameworks vorhanden. Falls die Anwendung von keiner Standardlösung oder Lösung mit Community-Support unterstützt wird, können Sie sie manuell instrumentieren.

Ein weiteres Beispiel, für das die benutzerdefinierte Nachverfolgung erforderlich ist, ist ein Worker, der Elemente aus der Warteschlange erhält. Für einige Warteschlangen wird der Aufruf zum Hinzufügen einer Nachricht zur Warteschlange als Abhängigkeit nachverfolgt. Ein allgemeiner Vorgang, der die Nachrichtenverarbeitung beschreibt, wird aber nicht automatisch erfasst.

Wir sehen uns nun an, wie diese Vorgänge nachverfolgt werden können.

Im Grunde genommen besteht die Aufgabe darin, `RequestTelemetry` zu erstellen und bekannte Eigenschaften festzulegen. Nachdem der Vorgang abgeschlossen ist, können Sie die Telemetriedaten nachverfolgen. Dies wird im folgenden Beispiel veranschaulicht.

### <a name="http-request-in-owin-self-hosted-app"></a>HTTP-Anforderung in selbstgehosteter Owin-App
Wir halten uns an das [HTTP-Protokoll für die Korrelation](https://github.com/dotnet/corefx/blob/master/src/System.Diagnostics.DiagnosticSource/src/HttpCorrelationProtocol.md). Sie können also damit rechnen, die hier beschriebenen Header zu empfangen.

``` C#
public class ApplicationInsightsMiddleware : OwinMiddleware
{
    private readonly TelemetryClient telemetryClient = new TelemetryClient(TelemetryConfiguration.Active);
    
    public ApplicationInsightsMiddleware(OwinMiddleware next) : base(next) {}

    public override async Task Invoke(IOwinContext context)
    {
        // Let's create and start RequestTelemetry
        var requestTelemetry = new RequestTelemetry
        {
            Name = $"{context.Request.Method} {context.Request.Uri.GetLeftPart(UriPartial.Path)}"
        };

        // if there is Request-Id recevied from the upstream service, set telemetry context accordingly
        if (context.Request.Headers.ContainsKey("Request-Id"))
        {
            var requestId = context.Request.Headers.Get("Request-Id");
            // get the operation id from the Request-Id (if you follow the 'Http Protocol for Correlation')
            requestTelemetry.Context.Operation.Id = GetOperationId(requestId);
            requestTelemetry.Context.Operation.ParentId = requestId;
        }

        // StartOperation is a helper method that allows to correlate 
        // current operations with nested operations/telemetry
        // and initializes start time and duration on telemetry item
        var operation = telemetryClient.StartOperation(requestTelemetry);

        // process request
        try
        {
            await Next.Invoke(context);
        }
        catch (Exception e)
        {
            requestTelemetry.Success = false;
            telemetryClient.TrackException(e);
            throw;
        }
        finally
        {
            // update status code and success as appropriate
            if (context.Response != null)
            {
                requestTelemetry.ResponseCode = context.Response.StatusCode.ToString();
                requestTelemetry.Success = context.Response.StatusCode >= 200 && context.Response.StatusCode <= 299;
            }
            else
            {
                requestTelemetry.Success = false;
            }

            // now it's time to stop operation (and track telemetry)
            telemetryClient.StopOperation(operation);
        }
    }
    
    public static string GetOperationId(string id)
    {
        // returns root Id from the '|' to first '.' if any
        int rootEnd = id.IndexOf('.');
        if (rootEnd < 0)
            rootEnd = id.Length;

        int rootStart = id[0] == '|' ? 1 : 0;
        return id.Substring(rootStart, rootEnd - rootStart);
    }
}
```

Mit dem Protokoll für die HTTP-Korrelation wird auch ein `Correlation-Context`-Header deklariert, der aber hier weggelassen wird, um die Komplexität zu verringern.

## <a name="queue-instrumentation"></a>Warteschlangeninstrumentierung
Für die HTTP-Kommunikation haben wir ein Protokoll zum Übergeben von Korrelationsdetails erstellt. Mit einigen Warteschlangenprotokollen können Sie zusammen mit der Nachricht zusätzliche Metadaten übergeben, während dies für andere nicht möglich ist.

### <a name="service-bus-queue"></a>Service Bus-Warteschlange
Bei einer [ServiceBus-Warteschlange](../service-bus-messaging/index.md) können Sie die Eigenschaftensammlung zusammen mit der Nachricht übergeben. Wir nutzen sie zum Übergeben der Korrelations-ID.

Für die ServiceBus-Warteschlange werden TCP-basierte Protokolle verwendet. Da Warteschlangenvorgänge von Application Insights nicht automatisch nachverfolgt werden, führen wir die Nachverfolgung manuell durch. Der Vorgang zum Entfernen aus der Warteschlange ist eine API im Push-Stil, und es ist keinerlei Nachverfolgung möglich.

#### <a name="enqueue"></a>Einreihen in die Warteschlange

```C#
public async Task Enqueue(string payload)
{
    // StartOperation is a helper method that initializes telemetry item
    // and allows to correlate this operation with its parent and children
    var operation = telemetryClient.StartOperation<DependencyTelemetry>("enqueue " + queueName);
    operation.Telemetry.Type = "Queue";
    operation.Telemetry.Data = "Enqueue " + queueName;

    var message = new BrokeredMessage(payload);
    // ServiceBus Queue allows to pass property bag along with the message
    // we will use them to pass our correlation identifiers (and other context)
    // to the consumer
    message.Properties.Add("ParentId", operation.Telemetry.Id);
    message.Properties.Add("RootId", operation.Telemetry.Context.Operation.Id);

    try
    {
        await queue.SendAsync(message);
        
        // set operation.Telemetry Success and ResponseCode here
        operation.Telemetry.Success = true;
    }
    catch (Exception e)
    {
        telemetryClient.TrackException(e);
        // set operation.Telemetry Success and ResponseCode here
        operation.Telemetry.Success = false;
        throw;
    }
    finally
    {
        telemetryClient.StopOperation(operation);
    }
}
```

#### <a name="process"></a>Prozess
```C#
public async Task Process(BrokeredMessage message)
{
    // once the message is taken from the queue, create ReqeustTelemetry to track its processing
    // it may also make sense to get name from the message
    RequestTelemetry requestTelemetry = new RequestTelemetry { Name = "Dequeue " + queueName };

    var rootId = message.Properties["RootId"].ToString();
    var parentId = message.Properties["ParentId"].ToString();
    // get the operation id from the Request-Id (if you follow the 'Http Protocol for Correlation')
    requestTelemetry.Context.Operation.Id = rootId;
    requestTelemetry.Context.Operation.ParentId = parentId;

    var operation = telemetryClient.StartOperation(requestTelemetry);

    try
    {
        await ProcessMessage();
    }
    catch (Exception e)
    {
        telemetryClient.TrackException(e);
        throw;
    }
    finally
    {
        // update status code and success as appropriate
        telemetryClient.StopOperation(operation);
    }
}
```

### <a name="azure-storage-queue"></a>Azure Storage-Warteschlange
Im folgenden Beispiel wird veranschaulicht, wie Sie Vorgänge der [Azure Storage-Warteschlange](../storage/storage-dotnet-how-to-use-queues.md) nachverfolgen und die Telemetrie zwischen „Producer“, „Consumer“ und Azure Storage korrelieren. 

Die Azure Storage-Warteschlange verfügt über eine HTTP-API, und alle Aufrufe der Warteschlange werden vom ApplicationInsights DependencyCollector in Bezug auf HTTP-Anforderungen nachverfolgt.
Stellen Sie sicher, dass `Microsoft.ApplicationInsights.DependencyCollector.HttpDependenciesParsingTelemetryInitializer` in `applicationInsights.config` enthalten ist, oder fügen Sie dieses Element wie [hier](app-insights-api-filtering-sampling.md) beschrieben programmgesteuert hinzu.

Stellen Sie beim manuellen Konfigurieren von ApplicationInsights sicher, dass Sie `Microsoft.ApplicationInsights.DependencyCollector.DependencyTrackingTelemetryModule` in etwa wie folgt erstellen und initialisieren:
 
``` C#
DependencyTrackingTelemetryModule module = new DependencyTrackingTelemetryModule();

// you can prevent correlation header injection to some domains by adding it to the excluded list.
// Make sure you add Azure Storage endpoint, otherwise you may experience request signature validation issues on the Storage service side
module.ExcludeComponentCorrelationHttpHeadersOnDomains.Add("core.windows.net");
module.Initialize(TelemetryConfiguration.Active);

// do not forget to dispose module during application shutdown
```

Es kann auch ratsam sein, die ID des ApplicationInsights-Vorgangs mit der Azure Storage RequestId zu korrelieren. Informationen dazu, wie Sie den Speicheranforderungsclient und die Serveranforderungs-ID festlegen und abrufen, finden Sie in [diesem Artikel](../storage/storage-monitoring-diagnosing-troubleshooting.md#end-to-end-tracing).

#### <a name="enqueue"></a>Einreihen in die Warteschlange
Da Azure Storage-Warteschlangen die HTTP-API unterstützen, werden alle Vorgänge der Warteschlange von ApplicationInsights automatisch nachverfolgt. In vielen Fällen sollte diese Instrumentierung ausreichend sein.
Zum Korrelieren von Ablaufverfolgungen auf Consumerseite mit Ablaufverfolgungen für Producer müssen Sie aber auf ähnliche Weise Korrelationskontext übergeben, wie wir dies für das „HTTP-Protokoll für die Korrelation“ getan haben. 

Im Beispiel unten wird der optionale `Enqueue`-Vorgang nachverfolgt. Dies ermöglicht Folgendes:
 - Korrelieren von erneuten Versuchen (falls vorhanden): Alle verfügen über ein gemeinsames übergeordnetes Element, und zwar den `Enqueue`-Vorgang. Andernfalls werden sie als untergeordnete Elemente der eingehenden Anforderung nachverfolgt. Wenn also mehrere logische Anforderungen für die Warteschlange vorhanden sind, lässt sich unter Umständen nur schwer ermitteln, welcher Aufruf zu den erneuten Versuchen geführt hat.
 - Korrelieren von Azure Storage-Protokollen (falls erforderlich) mit ApplicationInsights-Telemetrie.

Der `Enqueue`-Vorgang ist das untergeordnete Element eines übergeordneten Vorgangs (z.B. eingehende HTTP-Anforderung), und der HTTP-Abhängigkeitsaufruf ist das untergeordnete Element des `Enqueue`-Vorgangs und das auf zweiter Ebene untergeordnete Element der eingehenden Anforderung.

```C#
public async Task Enqueue(CloudQueue queue, string message)
{
    var operation = telemetryClient.StartOperation<DependencyTelemetry>("enqueue " + queue.Name);
    operation.Telemetry.Type = "Queue";
    operation.Telemetry.Data = "Enqueue " + queue.Name;

    // MessagePayload represents your custom message and also serializes correlation identifiers into payload
    // e.g. if you choose to pass payload serialized to json, it may look like
    // {'RootId' : 'some-id', 'ParentId' : '|some-id.1.2.3.', 'message' : 'your message to process'}
    var jsonPayload = JsonConvert.SerializeObject(new MessagePayload
    {
        RootId = operation.Telemetry.Context.Operation.Id,
        ParentId = operation.Telemetry.Id,
        Payload = message
    });
    
    CloudQueueMessage queueMessage = new CloudQueueMessage(jsonPayload);

    // Add operation.Telemetry.Id to the OperationContext to correlate Azure Storage logs and ApplciationInsights telemetry
    OperationContext context = new OperationContext { ClientRequestID = operation.Telemetry.Id};

    try
    {
        await queue.AddMessageAsync(queueMessage, null, null, new QueueRequestOptions(), context);
    }
    catch (StorageException e)
    {
        operation.Telemetry.Properties.Add("AzureServiceRequestID", e.RequestInformation.ServiceRequestID);
        operation.Telemetry.Success = false;
        operation.Telemetry.ResultCode = e.RequestInformation.HttpStatusCode.ToString();
        telemetryClient.TrackException(e);
    }
    finally
    {
        // update status code and success as appropriate
        telemetryClient.StopOperation(operation);
    }
}  
```

Wenn Sie die Menge an Telemetriedaten, die von Ihrer Anwendung gemeldet werden, reduzieren oder den `enqueue`-Vorgang aus anderen Gründen nachverfolgen möchten, können Sie die `Activity`-API direkt verwenden:

- Erstellen (und starten) Sie eine neue `Activity`, anstatt den ApplicationInsights-Vorgang zu starten (bis auf den Vorgangsnamen müssen Sie KEINE weiteren Eigenschaften zuweisen).
- Serialisieren Sie `yourActivity.Id` anstelle von `operation.Telemetry.Id` für die Nachrichtennutzlast. Sie können auch `Activity.Current.Id` verwenden.


#### <a name="dequeue"></a>Entfernen aus der Warteschlange
Ähnlich wie bei `Enqueue` werden die tatsächlichen HTTP-Anforderungen für die Azure Storage-Warteschlange von ApplicationInsights automatisch nachverfolgt. Der `Enqueue`-Vorgang wird normalerweise aber im Kontext des übergeordneten Elements durchgeführt, z.B. im Kontext von „eingehenden“ Anforderungen. Application Insights-SDKs korrelieren Vorgänge dieser Art (und den dazugehörigen HTTP-Teil) mit der übergeordneten Anforderung und anderen Telemetriedaten, die für denselben Bereich gemeldet werden.

Der `Dequeue`-Vorgang ist nicht ganz einfach: Das Application Insights-SDK führt die automatische Nachverfolgung von HTTP-Anforderungen durch, aber der Korrelationskontext ist erst bekannt, nachdem die Nachricht analysiert wurde. Es ist nicht möglich, die HTTP-Anforderung zu korrelieren, um die Nachricht mit den restlichen Telemetriedaten zu erhalten.

In vielen Fällen kann es nützlich sein, die HTTP-Anforderung zusammen mit anderen Ablaufverfolgungen mit der Warteschlange zu korrelieren. Im nächsten Beispiel wird veranschaulicht, wie Sie dies erreichen.

``` C#
public async Task<MessagePayload> Dequeue(CloudQueue queue)
{
    var telemetry = new DependencyTelemetry
    {
        Type = "Queue",
        Name = "Dequeue " + queue.Name
    };

    telemetry.Start();

    try
    {
        var message = await queue.GetMessageAsync();

        if (message != null)
        {
            var payload = JsonConvert.DeserializeObject<MessagePayload>(message.AsString);

            // if there is a message, we want to correlate Dequeue operation with processing
            // However we will only know what correlation Id to use AFTER we will get it from the message
            // So we will report telemetry once we know the Ids.
            telemetry.Context.Operation.Id = payload.RootId;
            telemetry.Context.Operation.ParentId = payload.ParentId;

            // delete message
            return payload;
        }
    }
    catch (StorageException e)
    {
        telemetry.Properties.Add("AzureServiceRequestID", e.RequestInformation.ServiceRequestID);
        telemetry.Success = false;
        telemetry.ResultCode = e.RequestInformation.HttpStatusCode.ToString();
        telemetryClient.TrackException(e);
    }
    finally
    {
        // update status code and success as appropriate
        telemetry.Stop();
        telemetryClient.Track(telemetry);
    }

    return null;
}
```

#### <a name="process"></a>Prozess

Im folgenden Beispiel wird die „eingehende“ Nachricht auf ähnliche Weise wie bei eingehenden HTTP-Anforderungen nachverfolgt.

```C#
public async Task Process(MessagePayload message)
{
    // once the message is dequeued from the queue, create ReqeustTelemetry to track it's processing
    RequestTelemetry requestTelemetry = new RequestTelemetry { Name = "Dequeue " + queueName };
    // it may also make sense to get name from the message
    requestTelemetry.Context.Operation.Id = message.RootId;
    requestTelemetry.Context.Operation.ParentId = message.ParentId;

    var operation = telemetryClient.StartOperation(requestTelemetry);

    try
    {
        await ProcessMessage();
    }
    catch (Exception e)
    {
        telemetryClient.TrackException(e);
        throw;
    }
    finally
    {
        // update status code and success as appropriate
        telemetryClient.StopOperation(operation);
    }
}
```

Auch andere Warteschlangenvorgänge können so instrumentiert werden. Der Peekvorgang sollte ähnlich wie bei der Entfernung aus der Warteschlange instrumentiert werden. Das Instrumentieren von Vorgängen der Warteschlangenverwaltung ist nicht erforderlich. Mit Application Insights werden Vorgänge als HTTP-Vorgänge nachverfolgt, und in den meisten Fällen ist dies ausreichend.

Stellen Sie beim Instrumentieren der Nachrichtenlöschung sicher, dass Sie die Vorgangsbezeichner festlegen (für die Korrelation). Alternativ hierzu können Sie auch die `Activity`-API nutzen (das Festlegen von Vorgangsbezeichnern für die Telemetrieelemente ist dann nicht erforderlich, da ApplicationInsights diese Aufgabe für Sie übernimmt):

- Erstellen Sie nach Erhalt eines Elements aus der Warteschlange eine neue `Activity`.
- Verwenden Sie `Activity.SetParentId(message.ParentId)` zum Korrelieren von Consumer- und Producerprotokollen.
- Starten Sie die `Activity`.
- Verfolgen Sie Vorgänge vom Typ „Dequeue“, „Process“ und „Delete“ nach, indem Sie `Start/StopOperation`-Hilfsprogramme verwenden. Führen Sie dies über die gleiche asynchrone Ablaufsteuerung (Ausführungskontext) durch. Auf diese Weise wird alles richtig korreliert.
- Beenden Sie anschließend die `Activity`.
- Verwenden Sie `Start/StopOperation`, oder rufen Sie die Nachverfolgung der Telemetrie manuell auf. 

### <a name="batch-processing"></a>Batchverarbeitung
Einige Warteschlangen ermöglichen das Entfernen von mehreren Nachrichten aus der Warteschlange mit einer Anforderung, aber die Verarbeitung dieser Nachrichten erfolgt voraussichtlich unabhängig und gehört zu den unterschiedlichen logischen Vorgängen.
In diesem Fall ist es nicht möglich, den `Dequeue`-Vorgang mit einer bestimmten Nachrichtenverarbeitung zu korrelieren.

Jede Nachrichtenverarbeitung sollte in einer eigenen asynchronen Ablaufsteuerung durchgeführt werden. Weitere Details hierzu finden Sie im Abschnitt [Nachverfolgung von ausgehenden Abhängigkeiten](#outgoing-dependencies-tracking).

## <a name="long-running-background-tasks"></a>Hintergrundaufgaben mit langer Ausführungsdauer
Einige Anwendungen starten einen Vorgang mit langer Ausführungsdauer, der unter Umständen durch Benutzeranforderungen verursacht wird. Aus Sicht der Nachverfolgung bzw. Instrumentierung unterscheidet sich dies nicht von der Instrumentierung von Anforderungen oder Abhängigkeiten. 

``` C#
async Task BackgroundTask()
{
    var operation = telemetryClient.StartOperation<RequestTelemetry>(taskName);
    operation.Telemetry.Type = "Background";
    try
    {
        int progress = 0;
        while (progress < 100)
        {
            // process task
            telemetryClient.TrackTrace($"done {progress++}%");
        }
        // update status code and success as appropriate
    }
    catch (Exception e)
    {
        telemetryClient.TrackException(e);
        // update status code and success as appropriate
        throw;
    }
    finally
    {
        telemetryClient.StopOperation(operation);
    }
}
```

In diesem Beispiel verwenden wir `telemetryClient.StartOperation`, um `RequestTelemetry` zu erstellen und den Korrelationskontext zu füllen. Angenommen, Sie verfügen über einen übergeordneten Vorgang, der beispielsweise von eingehenden Anforderungen für die Planung des Vorgangs erstellt wurde. Sofern `BackgroundTask` in derselben asynchronen Ablaufsteuerung wie eine eingehende Anforderung gestartet wird, wird die Korrelation mit diesem übergeordneten Vorgang durchgeführt. `BackgroundTask` und alle geschachtelten Telemetriedatenelemente werden automatisch mit der Anforderung korreliert, die der Auslöser war. Dies gilt auch nach Abschluss der Anforderung.

Wenn die Aufgabe über den Hintergrundthread gestartet wird, dem kein Vorgang (Aktivität) zugeordnet ist, weist `BackgroundTask` kein übergeordnetes Element auf. Sie kann aber geschachtelte Vorgänge enthalten, und alle Telemetriedatenelemente, die von der Aufgabe gemeldet werden, werden mit dem in der `BackgroundTask` erstellten `RequestTelemetry`-Element korreliert.

## <a name="outgoing-dependencies-tracking"></a>Nachverfolgung von ausgehenden Abhängigkeiten
Es kann sein, dass Sie Ihre eigene Art von „Abhängigkeit“ oder einen Vorgang nachverfolgen möchten, der nicht von ApplicationInsights unterstützt wird.

Die `Enqueue`-Methode in der ServiceBus-Warteschlange oder Azure Storage-Warteschlange kann als Beispiel für diese benutzerdefinierte Nachverfolgung dienen.

Allgemeiner Ansatz für die benutzerdefinierte Nachverfolgung von Abhängigkeiten:
- Rufen Sie die `TelemetryClient.StartOperation`-Methode (Erweiterungsmethode) auf, mit der erforderliche `DependencyTelemetry`-Eigenschaften für die Korrelation und einige andere Eigenschaften (Startzeitstempel, Dauer) aufgefüllt werden.
- Legen Sie weitere benutzerdefinierte Eigenschaften für `DependencyTelemetry` fest: Name und anderen benötigten Kontext.
- Führen Sie einen Abhängigkeitsaufruf durch, und warten Sie auf das Ergebnis.
- Beenden Sie den Vorgang `StopOperation`, wenn Sie fertig sind.
- Behandeln von Ausnahmen

Mit `StopOperation` wird nur der Vorgang beendet, der gestartet wurde: Wenn der derzeit ausgeführte Vorgang nicht mit dem Vorgang übereinstimmt, den Sie beenden möchten, hat StopOperation keinerlei Auswirkung. Dies kann der Fall sein, wenn Sie mehrere Vorgänge parallel in demselben Ausführungskontext starten:

```C#
var firstOperation = telemetryClient.StartOperation<DependencyTelemetry>("task 1");
var firstOperation = telemetryClient.StartOperation<DependencyTelemetry>("task 1");
var firstTask = RunMyTaskAsync();

var secondOperation = telemetryClient.StartOperation<DependencyTelemetry>("task 2");
var secondTask = RunMyTaskAsync();

await firstTask;

// this will do nothing and will not report telemetry for the first operation
// as currently secondOperation is active
telemetryClient.StopOperation(firstOperation); 

await secondTask;
```

Sie müssen also sicherstellen, dass Sie immer `StartOperation` aufrufen und Ihre Aufgabe in ihrem eigenen Kontext ausführen:
```C#
public async Task RunMyTaskAsync()
{
    var operation = telemetryClient.StartOperation<DependencyTelemetry>("task 1");
    try 
    {
        var myTask = await StartMyTaskAsync();
        // update status code and success as appropriate
    }
    catch(...) 
    {
        // update status code and success as appropriate
    }
    finally 
    {
        telemetryClient.StopOperation(operation);
    }
}
```

## <a name="next-steps"></a>Nächste Schritte

- Informieren Sie sich über die Grundlagen der [Telemetriekorrelation](application-insights-correlation.md) in Application Insights.
- Lesen Sie die Informationen zu den Application Insights-Typen und zum Datenmodell unter [Datenmodell](application-insights-data-model.md).
- Informieren Sie sich über das Melden von benutzerdefinierten [Ereignissen und Metriken](app-insights-api-custom-events-metrics.md) an Application Insights.
- Informationen zur [Konfiguration](app-insights-configuration-with-applicationinsights-config.md#telemetry-initializers-aspnet) der Sammlung von Standardkontexteigenschaften.
- Lesen Sie den [System.Diagnostics.Activity User Guide](https://github.com/dotnet/corefx/blob/master/src/System.Diagnostics.DiagnosticSource/src/ActivityUserGuide.md) (System.Diagnostics.Activity-Leitfaden), um zu erfahren, wie wir Telemetriedaten korrelieren.

