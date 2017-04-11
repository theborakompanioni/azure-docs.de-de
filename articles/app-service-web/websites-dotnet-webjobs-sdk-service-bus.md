---
title: Verwenden von Azure Service Bus mit dem WebJobs-SDK
description: "Erfahren Sie mehr über die Verwendung von Azure Service Bus-Warteschlangen und -Themen mit dem WebJobs-SDK."
services: app-service\web, service-bus
documentationcenter: .net
author: ggailey777
manager: erikre
editor: jimbe
ms.assetid: 2114a934-135b-42b8-871c-6cc040214e76
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 06/01/2016
ms.author: glenga
translationtype: Human Translation
ms.sourcegitcommit: 2c9877f84873c825f96b62b492f49d1733e6c64e
ms.openlocfilehash: e44199dec6de1e89545f3d122481aa81e97d75ac
ms.lasthandoff: 03/15/2017


---
# <a name="how-to-use-azure-service-bus-with-the-webjobs-sdk"></a>Verwenden von Azure Service Bus mit dem WebJobs-SDK
## <a name="overview"></a>Übersicht
Dieser Leitfaden enthält C#-Codebeispiele, die zeigen, wie Sie einen Prozess auslösen, wenn eine Azure Service Bus-Meldung eingeht. In den Codebeispielen wird Version 1.x des [WebJobs-SDK](websites-dotnet-webjobs-sdk.md) verwendet.

Im Handbuch wird davon ausgegangen, dass Sie wissen, [wie ein WebJobs-Projekt in Visual Studio mit Verbindungszeichenfolgen erstellt wird, die auf Ihr Speicherkonto verweisen](websites-dotnet-webjobs-sdk-get-started.md).

Die Codeausschnitte zeigen nur Funktionen, nicht den Code, mit dem das `JobHost` -Objekt in diesem Beispiel erstellt wird:

```
public class Program
{
   public static void Main()
   {
      JobHostConfiguration config = new JobHostConfiguration();
      config.UseServiceBus();
      JobHost host = new JobHost(config);
      host.RunAndBlock();
   }
}
```

Ein [vollständiges Service Bus-Codebeispiel](https://github.com/Azure/azure-webjobs-sdk-samples/blob/master/BasicSamples/ServiceBus/Program.cs) finden Sie im Repository „azure-webjobs-sdk-samples“ auf GitHub.com.

## <a id="prerequisites"></a> Voraussetzungen
Für die Arbeit mit Service Bus müssen Sie neben den weiteren WebJobs-SDK-Paketen das NuGet-Paket [Microsoft.Azure.WebJobs.ServiceBus](https://www.nuget.org/packages/Microsoft.Azure.WebJobs.ServiceBus/) installieren. 

Sie müssen außerdem zusätzlich zu den Speicherverbindungszeichenfolgen die Verbindungszeichenfolge "AzureWebJobsServiceBus" festlegen.  Sie führen dies im Abschnitt `connectionStrings` der Datei "App.config" aus, wie im folgenden Beispiel gezeigt:

        <connectionStrings>
            <add name="AzureWebJobsDashboard" connectionString="DefaultEndpointsProtocol=https;AccountName=[accountname];AccountKey=[accesskey]"/>
            <add name="AzureWebJobsStorage" connectionString="DefaultEndpointsProtocol=https;AccountName=[accountname];AccountKey=[accesskey]"/>
            <add name="AzureWebJobsServiceBus" connectionString="Endpoint=sb://[yourServiceNamespace].servicebus.windows.net/;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=[yourKey]"/>
        </connectionStrings>

Ein Beispielprojekt, das die Einstellung für die Service Bus-Verbindungszeichenfolge in der Datei "App.config" enthält, finden Sie im [Service Bus-Beispiel](https://github.com/Azure/azure-webjobs-sdk-samples/tree/master/BasicSamples/ServiceBus). 

Die Verbindungszeichenfolgen können auch in der Azure-Laufzeitumgebung festgelegt werden, die dann die „App.config“-Einstellungen überschreibt, wenn der WebJob in Azure ausgeführt wird. Weitere Informationen finden Sie unter [Erste Schritte mit dem WebJobs SDK](websites-dotnet-webjobs-sdk-get-started.md#configure-the-web-app-to-use-your-azure-sql-database-and-storage-account).

## <a id="trigger"></a> Auslösen einer Funktion, wenn eine Service Bus-Warteschlangennachricht empfangen wird
Zum Schreiben einer Funktion, die das WebJobs-SDK aufruft, wenn eine Warteschlangennachricht empfangen wird, verwenden Sie das `ServiceBusTrigger` -Attribut. Der Attributkonstruktor verwendet einen Parameter, der den Namen der abzufragenden Warteschlange angibt.

### <a name="how-servicebustrigger-works"></a>Funktionsweise von ServiceBusTrigger
Das SDK empfängt eine Nachricht im `PeekLock`-Modus und ruft bei erfolgreicher Ausführung der Funktion `Complete` für die Nachricht auf. Ist die Ausführung nicht erfolgreich, wird `Abandon` aufgerufen. Wenn die Funktion länger als im `PeekLock`-Timeout angegeben ausgeführt wird, wird die Sperre automatisch erneuert.

Service Bus führt eine eigene Verarbeitung von Warteschlangen für nicht verarbeitbare Nachrichten durch, die nicht mit dem WebJobs-SDK konfiguriert oder gesteuert werden kann. 

### <a name="string-queue-message"></a>Zeichenfolgen-Warteschlangennachrichten
Das folgende Codebeispiel liest eine Warteschlangennachricht, die eine Zeichenfolge enthält, und schreibt diese in das WebJobs-SDK-Dashboard.

        public static void ProcessQueueMessage([ServiceBusTrigger("inputqueue")] string message, 
            TextWriter logger)
        {
            logger.WriteLine(message);
        }

**Hinweis:** Wenn Sie die Warteschlangennachrichten in einer Anwendung erstellen, die das WebJobs-SDK nicht verwendet, legen Sie [BrokeredMessage.ContentType](http://msdn.microsoft.com/library/microsoft.servicebus.messaging.brokeredmessage.contenttype.aspx) auf "text/plain" fest.

### <a name="poco-queue-message"></a>POCO-Warteschlangennachricht
Das SDK führt eine automatische Deserialisierung der Warteschlangennachricht durch, die für einen POCO-Typ [(Plain Old CLR Object)](http://en.wikipedia.org/wiki/Plain_Old_CLR_Object)JSON enthält. Das folgende Codebeispiel liest eine Warteschlangennachricht, die ein `BlobInformation`-Objekt mit einer `BlobName`-Eigenschaft enthält:

        public static void WriteLogPOCO([ServiceBusTrigger("inputqueue")] BlobInformation blobInfo,
            TextWriter logger)
        {
            logger.WriteLine("Queue message refers to blob: " + blobInfo.BlobName);
        }

Codebeispiele, die zeigen, wie POCO-Eigenschaften zum Arbeiten mit Blobs und Tabellen in derselben Funktion verwendet werden, finden Sie in der [Artikelversion zu Speicherwarteschlangen](websites-dotnet-webjobs-sdk-storage-queues-how-to.md#pocoblobs).

Wenn Ihr Code zum Erstellen der Warteschlangennachricht nicht das WebJobs-SDK verwendet, verwenden Sie ähnlichen Code wie im folgenden Beispiel:

        var client = QueueClient.CreateFromConnectionString(ConfigurationManager.ConnectionStrings["AzureWebJobsServiceBus"].ConnectionString, "blobadded");
        BlobInformation blobInformation = new BlobInformation () ;
        var message = new BrokeredMessage(blobInformation);
        client.Send(message);

### <a name="types-servicebustrigger-works-with"></a>Für "ServiceBusTrigger" verwendbare Typen
Neben `string` und POCO-Typen können Sie das `ServiceBusTrigger`-Attribut mit einem Bytearray oder einem `BrokeredMessage`-Objekt verwenden.

## <a id="create"></a> Erstellen von Service Bus-Warteschlangennachrichten
Zum Schreiben einer Funktion, die eine neue Warteschlangennachricht erstellt, verwenden Sie das `ServiceBus` -Attribut und übergeben den Warteschlangennamen an den Attributkonstruktor. 

### <a name="create-a-single-queue-message-in-a-non-async-function"></a>Erstellen einer einzelnen Warteschlangennachricht in einer nicht asynchronen Funktion
Im folgenden Codebeispiel wird ein Ausgabeparameter zum Erstellen einer neuen Nachricht in der Warteschlange "outputqueue" verwendet. Diese hat den gleichen Inhalt wie die Nachricht, die in der Warteschlange "inputqueue" empfangen wird.

        public static void CreateQueueMessage(
            [ServiceBusTrigger("inputqueue")] string queueMessage,
            [ServiceBus("outputqueue")] out string outputQueueMessage)
        {
            outputQueueMessage = queueMessage;
        }

Der Ausgabeparameter zum Erstellen einer einzelnen Warteschlangennachricht kann einen der folgenden Typen verwenden:

* `string`
* `byte[]`
* `BrokeredMessage`
* Ein serialisierbarer POCO-Typ, den Sie definieren. Wird automatisch als JSON serialisiert.

Für Parameter vom POCO-Typ wird bei Beendigung der Funktion immer eine Warteschlangennachricht erstellt. Wenn der Parameter "null" ist, erstellt das SDK eine Warteschlangennachricht, die "null" zurückgibt, wenn die Nachricht empfangen und deserialisiert wird. Bei anderen Typen wird keine Warteschlangennachricht erstellt, wenn der Parameter "null" ist.

### <a name="create-multiple-queue-messages-or-in-async-functions"></a>Erstellen von mehreren Warteschlangennachrichten in asynchronen Funktionen
Verwenden Sie zum Erstellen mehrerer Nachrichten das `ServiceBus`-Attribut mit `ICollector<T>` oder `IAsyncCollector<T>`, wie im folgenden Codebeispiel gezeigt:

        public static void CreateQueueMessages(
            [ServiceBusTrigger("inputqueue")] string queueMessage,
            [ServiceBus("outputqueue")] ICollector<string> outputQueueMessage,
            TextWriter logger)
        {
            logger.WriteLine("Creating 2 messages in outputqueue");
            outputQueueMessage.Add(queueMessage + "1");
            outputQueueMessage.Add(queueMessage + "2");
        }

Jede Warteschlangennachricht wird unmittelbar nach dem Aufruf der `Add` -Methode erstellt.

## <a id="topics"></a>Arbeiten mit Service Bus-Themen
Zum Schreiben einer Funktion, die das SDK aufruft, wenn eine Nachricht zu einem Service Bus-Thema empfangen wird, verwenden Sie das `ServiceBusTrigger` -Attribut mit dem Konstruktor, der Themen- und Abonnementname enthält, wie im folgenden Codebeispiel gezeigt:

        public static void WriteLog([ServiceBusTrigger("outputtopic","subscription1")] string message,
            TextWriter logger)
        {
            logger.WriteLine("Topic message: " + message);
        }

Zum Erstellen einer Nachricht zu einem Thema verwenden Sie das `ServiceBus` -Attribut mit einem Themennamen ebenso wie bei Verwendung eines Warteschlangennamens.

## <a name="features-added-in-release-11"></a>In Version 1.1 hinzugefügte Features
In Version 1.1 wurden die folgenden Features hinzugefügt:

* Möglichkeit zur umfassenden Anpassung der Nachrichtenverarbeitung über `ServiceBusConfiguration.MessagingProvider`.
* `MessagingProvider` unterstützt die Anpassung der Service Bus-Elemente `MessagingFactory` und `NamespaceManager`.
* Ein `MessageProcessor` -Strategiemuster ermöglicht es Ihnen, einen Prozessor pro Warteschlange/Thema anzugeben.
* Die parallele Nachrichtenverarbeitung wird standardmäßig unterstützt. 
* Einfache Anpassung von `OnMessageOptions` über `ServiceBusConfiguration.MessageOptions`.
* [AccessRights](https://github.com/Azure/azure-webjobs-sdk-samples/blob/master/BasicSamples/ServiceBus/Functions.cs#L71) können für `ServiceBusTriggerAttribute`/`ServiceBusAttribute` angegeben werden (für Szenarios, in denen Sie möglicherweise keine Verwaltungsrechte besitzen). Beachten Sie, dass Azure WebJobs ohne das Verwaltungsrecht AccessRights nur existierende Warteschlangen und Themen automatisch verwalten kann.

## <a id="queues"></a>Verwandte Themen, die im Artikel zu Speicherwarteschlangen behandelt werden
Informationen zu WebJobs-SDK-Szenarien, die nicht spezifisch für Service Bus sind, finden Sie unter [Verwenden des Azure-Warteschlangenspeichers mit dem WebJobs-SDK](websites-dotnet-webjobs-sdk-storage-queues-how-to.md). 

In diesem Artikel werden u. a. die folgenden Themen behandelt:

* Asynchrone Funktionen
* Mehrere Instanzen
* Ordnungsgemäßes Herunterfahren
* Verwenden von WebJobs-SDK-Attributen im Hauptteil einer Funktion
* Festlegen der SDK-Verbindungszeichenfolgen im Code
* Festlegen von Werten für WebJobs SDK-Konstruktorparametern im Code
* Manuelles Auslösen einer Funktion
* Schreiben von Protokollen

## <a id="nextsteps"></a> Nächste Schritte
In dieser Anleitung wurde anhand von Codebeispielen veranschaulicht, wie häufige Szenarien für das Arbeiten mit Azure Service Bus behandelt werden. Weitere Informationen zur Verwendung von Azure WebJobs und dem WebJobs-SDK finden Sie unter [Empfohlene Ressourcen für Azure WebJobs](http://go.microsoft.com/fwlink/?linkid=390226).


