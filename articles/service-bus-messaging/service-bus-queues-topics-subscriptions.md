---
title: "Übersicht über Azure Service Bus-Messagingwarteschlangen, -themen und -abonnements | Microsoft-Dokumentation"
description: "Überblick über Service Bus-Nachrichtenentitäten"
services: service-bus-messaging
documentationcenter: na
author: sethmanheim
manager: timlt
editor: 
ms.assetid: a306ced4-74e9-47c6-990a-d9c47efa31d5
ms.service: service-bus-messaging
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/28/2017
ms.author: sethm
ms.translationtype: HT
ms.sourcegitcommit: 8b857b4a629618d84f66da28d46f79c2b74171df
ms.openlocfilehash: 00f9f38fbae028486270053dedb4df580a3f1a44
ms.contentlocale: de-de
ms.lasthandoff: 08/04/2017

---
# <a name="service-bus-queues-topics-and-subscriptions"></a>Service Bus-Warteschlangen, -Themen und -Abonnements

Microsoft Azure Service Bus unterstützt einen Satz cloudbasierter, nachrichtenorientierter Middlewaretechnologien, darunter zuverlässiges Message Queuing und dauerhaftes Veröffentlichungs-/Abonnementmessaging. Diese Brokermessagingfunktionen kann man sich als asynchrone, entkoppelte Messagingfeatures vorstellen, die unter Verwendung des Service Bus-Messagingfabrics verschiedene Szenarien wie Veröffentlichung/Abonnements, vorübergehende Entkopplung und Lastenausgleich unterstützen. Entkoppelte Kommunikation hat viele Vorteile, beispielsweise können Clients und Server Verbindungen nach Bedarf herstellen und Vorgänge asynchron ausführen.

Den Kern der Messagingfunktionen in Service Bus bilden die folgenden Messagingentitäten: Warteschlangen, Themen und Abonnements sowie Regeln/Aktionen.

## <a name="queues"></a>Warteschlangen

Warteschlangen liefern die Nachrichten im *First In, First Out*-Verfahren (FIFO) an einen oder mehrere Consumer. Dies bedeutet, dass Nachrichten normalerweise von den Empfängern in der Reihenfolge empfangen und verarbeitet werden, in der sie der Warteschlange hinzugefügt wurden, und dass jede Nachricht nur von einem Nachrichtenconsumer empfangen und verarbeitet wird. Als Hauptvorteil ergibt sich bei der Verwendung von Warteschlangen eine "vorübergehende Entkopplung" von Anwendungskomponenten. Anders ausgedrückt, die Producer (Absender) und Consumer (Empfänger) müssen Nachrichten nicht gleichzeitig senden und empfangen, da Nachrichten dauerhaft in der Warteschlange gespeichert werden. Außerdem muss der Producer nicht auf eine Antwort vom Consumer warten, um weiterhin Nachrichten zu verarbeiten und zu senden.

Ein weiterer Vorteil ist der Belastungsausgleich, durch den Producer und Consumer die Möglichkeit haben, Nachrichten mit unterschiedlichen Raten zu senden und zu empfangen. In vielen Anwendungen schwankt die Systemlast mit der Zeit, jedoch ist die erforderliche Verarbeitungsdauer pro Arbeitseinheit üblicherweise konstant. Durch die Zwischenschaltung einer Warteschlange zwischen Nachrichtenproducer und -consumer reicht es aus, wenn die konsumierende Anwendung für die Durchschnittslast anstatt für die Spitzenlast ausgelegt ist. Die Tiefe der Warteschlange erhöht und verringert sich mit der eingehenden Last. Auf diese Weise sparen Sie direkt Geld, da Sie weniger Infrastruktur für Ihre Anwendungslast benötigen. Mit zunehmender Last können weitere Arbeitsprozesse hinzugefügt werden, die Nachrichten aus der Warteschlange abrufen. Jede Nachricht wird nur von einem der Arbeitsprozesse verarbeitet. Außerdem ermöglicht dieser Pull-basierte Lastenausgleich eine optimale Nutzung der Workercomputer, selbst wenn diese über unterschiedliche Leistung verfügen, da jeder von ihnen die Nachrichten in der eigenen Maximalgeschwindigkeit abruft. Dieses Schema wird auch als „Konkurrierende Consumer“ bezeichnet.

Die Verwendung von Warteschlangen als Zwischenglied zwischen Nachrichtenproducern und -consumern sorgt für eine inhärente lockere Kopplung zwischen den Komponenten. Da Producer und Consumer voneinander unabhängig sind, kann ein Upgrade für einen Consumer ohne Auswirkungen auf den Producer durchgeführt werden.

Die Erstellung einer Warteschlange erfolgt in mehreren Schritten. Verwaltungsvorgänge für Service Bus-Messagingentitäten (sowohl Warteschlangen als auch Themen) werden über die [Microsoft.ServiceBus.NamespaceManager](/dotnet/api/microsoft.servicebus.namespacemanager#microsoft_servicebus_namespacemanager)-Klasse durchgeführt, die durch Angabe der Basisadresse des Service Bus-Namespace und der Benutzeranmeldeinformationen erstellt wird. [NamespaceManager](/dotnet/api/microsoft.servicebus.namespacemanager#microsoft_servicebus_namespacemanager) enthält Methoden zum Erstellen, Aufzählen und Löschen von Messagingentitäten. Nachdem Sie ein [Microsoft.ServiceBus.TokenProvider](/dotnet/api/microsoft.servicebus.tokenprovider#microsoft_servicebus_tokenprovider)-Objekt aus dem SAS-Namen und -Schlüssel sowie ein Dienstnamespace-Verwaltungsobjekt erstellt haben, können Sie die Warteschlange mit der [Microsoft.ServiceBus.NamespaceManager.CreateQueue](/dotnet/api/microsoft.servicebus.namespacemanager#Microsoft_ServiceBus_NamespaceManager_CreateQueue_System_String_)-Methode erstellen. Beispiel:

```csharp
// Create management credentials
TokenProvider credentials = TokenProvider.CreateSharedAccessSignatureTokenProvider(sasKeyName,sasKeyValue);
// Create namespace client
NamespaceManager namespaceClient = new NamespaceManager(ServiceBusEnvironment.CreateServiceUri("sb", ServiceNamespace, string.Empty), credentials);
```

Anschließend können Sie ein Warteschlangenobjekt und eine Messagingfactory mit dem Service Bus-URI als Argument erstellen. Zum Beispiel:

```csharp
QueueDescription myQueue;
myQueue = namespaceClient.CreateQueue("TestQueue");
MessagingFactory factory = MessagingFactory.Create(ServiceBusEnvironment.CreateServiceUri("sb", ServiceNamespace, string.Empty), credentials); 
QueueClient myQueueClient = factory.CreateQueueClient("TestQueue");
```

Dann können Sie Nachrichten an die Warteschlange senden. Im Fall einer Liste von Brokernachrichten namens `MessageList` sieht der Code beispielsweise ungefähr folgendermaßen aus:

```csharp
for (int count = 0; count < 6; count++)
{
    var issue = MessageList[count];
    issue.Label = issue.Properties["IssueTitle"].ToString();
    myQueueClient.Send(issue);
}
```

Sie empfangen dann Nachrichten aus der Warteschlange wie folgt:

```csharp
while ((message = myQueueClient.Receive(new TimeSpan(hours: 0, minutes: 0, seconds: 5))) != null)
    {
        Console.WriteLine(string.Format("Message received: {0}, {1}, {2}", message.SequenceNumber, message.Label, message.MessageId));
        message.Complete();

        Console.WriteLine("Processing message (sleeping...)");
        Thread.Sleep(1000);
    }
```

Im [ReceiveAndDelete](/dotnet/api/microsoft.servicebus.messaging.receivemode)-Modus ist der Nachrichtenempfang ein einstufiger Vorgang. Das heißt, wenn Service Bus die Anforderung erhält, wird die Nachricht als verarbeitet gekennzeichnet und an die Anwendung zurückgesendet. Der Modus **ReceiveAndDelete** ist das einfachste Modell. Er wird am besten für Szenarios eingesetzt, bei denen die Anwendung es tolerieren kann, wenn eine Nachricht bei Auftreten eines Fehlers nicht verarbeitet wird. Um dieses Verfahren zu verstehen, stellen Sie sich ein Szenario vor, in dem der Consumer die Empfangsanforderung ausstellt und dann abstürzt, bevor diese verarbeitet wird. Wenn die Anwendung neu gestartet wird und das Konsumieren von Nachrichten fortsetzt, entgeht ihr die vor dem Absturz konsumierte Nachricht, da Service Bus die Nachricht als konsumiert markiert hat.

Im [PeekLock](/dotnet/api/microsoft.servicebus.messaging.receivemode)-Modus ist der Empfangsvorgang zweistufig. Dadurch können Anwendungen unterstützt werden, die das Fehlen von Nachrichten nicht tolerieren können. Wenn Service Bus die Anforderung erhält, ermittelt der Dienst die nächste zu verarbeitende Nachricht, sperrt diese, um zu verhindern, dass andere Consumer sie erhalten, und sendet sie dann an die Anwendung zurück. Nachdem die Anwendung die Verarbeitung der Nachricht abgeschlossen hat (oder sie zwecks zukünftiger Verarbeitung zuverlässig gespeichert hat), führt sie die zweite Phase des Empfangsprozesses per Aufruf von [Complete](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage#Microsoft_ServiceBus_Messaging_BrokeredMessage_Complete) für die empfangene Nachricht durch. Wenn Service Bus den **Complete**-Aufruf erkennt, wird die Nachricht als verarbeitet markiert.

Wenn die Anwendung die Nachricht aus einem bestimmten Grund nicht verarbeiten kann, kann sie die [Abandon](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage#Microsoft_ServiceBus_Messaging_BrokeredMessage_Abandon)-Methode für die empfangene Nachricht aufrufen (anstelle von [Complete](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage#Microsoft_ServiceBus_Messaging_BrokeredMessage_Complete)). Dadurch kann Service Bus die Nachricht entsperren und verfügbar machen, damit sie erneut empfangen werden kann, und zwar entweder von demselben Consumer oder von einem anderen konkurrierenden Consumer. Der Sperre ist außerdem ein Timeout zugeordnet. Wenn die Anwendung die Nachricht nicht vor Ablauf des Timeouts verarbeiten kann (z.B. wenn die Anwendung abstürzt), entsperrt Service Bus die Nachricht und macht sie verfügbar, sodass sie wieder empfangen werden kann (im Wesentlichen erfolgt standardmäßig der Vorgang [Abandon](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage#Microsoft_ServiceBus_Messaging_BrokeredMessage_Abandon)).

Falls die Anwendung nach der Verarbeitung der Nachricht, jedoch vor Ausgabe der **Complete**-Anforderung abstürzt, wird die Nachricht der Anwendung bei ihrem Neustart erneut zugestellt. Dies wird häufig als *At Least Once*-Processing bezeichnet, d.h., jede Nachricht wird mindestens einmal verarbeitet. In bestimmten Situationen wird dieselbe Nachricht jedoch möglicherweise erneut zugestellt. Wenn das Szenario keine doppelte Verarbeitung toleriert, muss in der Anwendung zusätzliche Logik zum Erkennen von Duplikaten vorhanden sein; hierzu kann die Eigenschaft **MessageId** der Nachricht verwendet werden, die über alle Zustellversuche hinweg konstant bleibt. Dies wird auch als *Exactly Once*-Verarbeitung (genau einmal) bezeichnet.

## <a name="topics-and-subscriptions"></a>Themen und Abonnements
Im Gegensatz zu Warteschlangen, bei denen jede Nachricht von einem einzigen Consumer verarbeitet wird, bieten *Themen* und *Abonnements* eine 1:n-Kommunikationsform, die nach dem Muster *Veröffentlichen/Abonnieren* abläuft. Jede veröffentlichte Nachricht wird für jedes beim Thema registrierte Abonnement verfügbar gemacht, was für die Skalierung in Szenarien mit sehr vielen Benutzern hilfreich ist. Nachrichten werden an ein Thema gesendet und an ein oder mehrere zugeordnete Abonnements übermittelt, abhängig von Filterregeln, die für jedes Abonnement einzeln festlegt werden können. In Abonnements können die zu empfangenden Nachrichten mithilfe zusätzlicher Filter eingeschränkt werden. Nachrichten werden auf die gleiche Weise an ein Thema gesendet wie an eine Warteschlange, sie werden jedoch nicht direkt vom Thema empfangen. Stattdessen werden sie von Abonnements empfangen. Ein Themenabonnement ist mit einer virtuellen Warteschlange vergleichbar, die Kopien der an das Thema gesendeten Nachrichten empfängt. Nachrichten werden von einem Abonnement auf die gleiche Weise empfangen wie von einer Warteschlange.

Zum Vergleich: Die Sendefunktionalität einer Warteschlange ist direkt einem Thema zuzuordnen, und die Empfangsfunktionalität ist einem Abonnement zuzuordnen. Dies bedeutet unter anderem, dass Abonnements das weiter oben in diesem Abschnitt im Hinblick auf Warteschlangen beschriebene Muster unterstützen: konkurrierender Consumer, vorübergehende Entkopplung, Belastungsausgleich und Lastenausgleich.

Ein Thema wird mit einem ähnlichen Verfahren erstellt wie eine Warteschlange, wie im Beispiel im vorherigen Abschnitt gezeigt. Erstellen Sie den Dienst-URI, und erstellen Sie dann mithilfe der [NamespaceManager](/dotnet/api/microsoft.servicebus.namespacemanager)-Klasse den Namespaceclient. Anschließend können Sie ein Thema mit der [CreateTopic](/dotnet/api/microsoft.servicebus.namespacemanager#Microsoft_ServiceBus_NamespaceManager_CreateTopic_System_String_)-Methode erstellen. Beispiel:

```csharp
TopicDescription dataCollectionTopic = namespaceClient.CreateTopic("DataCollectionTopic");
```

Fügen Sie dann Abonnements nach Bedarf hinzu:

```csharp
SubscriptionDescription myAgentSubscription = namespaceClient.CreateSubscription(myTopic.Path, "Inventory");
SubscriptionDescription myAuditSubscription = namespaceClient.CreateSubscription(myTopic.Path, "Dashboard");
```

Anschließend erstellen Sie einen Themenclient. Beispiel:

```csharp
MessagingFactory factory = MessagingFactory.Create(serviceUri, tokenProvider);
TopicClient myTopicClient = factory.CreateTopicClient(myTopic.Path)
```

Mit dem Nachrichtenabsender können Sie Nachrichten an ein Thema senden und von diesem empfangen, wie im vorherigen Abschnitt gezeigt. Beispiel:

```csharp
foreach (BrokeredMessage message in messageList)
{
    myTopicClient.Send(message);
    Console.WriteLine(
    string.Format("Message sent: Id = {0}, Body = {1}", message.MessageId, message.GetBody<string>()));
}
```

Ähnlich wie bei Warteschlangen werden Nachrichten von einem Abonnement nicht mit einem [QueueClient](/dotnet/api/microsoft.servicebus.messaging.subscriptionclient)-Objekt, sondern mit einem [SubscriptionClient](/dotnet/api/microsoft.servicebus.messaging.queueclient)-Objekt empfangen. Erstellen Sie den Abonnementclient, und übergeben Sie dabei den Namen des Themas, den Namen des Abonnements und (optional) den Empfangsmodus als Parameter. Beispiel für das Abonnement **Inventory**:

```csharp
// Create the subscription client
MessagingFactory factory = MessagingFactory.Create(serviceUri, tokenProvider); 

SubscriptionClient agentSubscriptionClient = factory.CreateSubscriptionClient("IssueTrackingTopic", "Inventory", ReceiveMode.PeekLock);
SubscriptionClient auditSubscriptionClient = factory.CreateSubscriptionClient("IssueTrackingTopic", "Dashboard", ReceiveMode.ReceiveAndDelete); 

while ((message = agentSubscriptionClient.Receive(TimeSpan.FromSeconds(5))) != null)
{
    Console.WriteLine("\nReceiving message from Inventory...");
    Console.WriteLine(string.Format("Message received: Id = {0}, Body = {1}", message.MessageId, message.GetBody<string>()));
    message.Complete();
}          

// Create a receiver using ReceiveAndDelete mode
while ((message = auditSubscriptionClient.Receive(TimeSpan.FromSeconds(5))) != null)
{
    Console.WriteLine("\nReceiving message from Dashboard...");
    Console.WriteLine(string.Format("Message received: Id = {0}, Body = {1}", message.MessageId, message.GetBody<string>()));
}
```

### <a name="rules-and-actions"></a>Regeln und Aktionen
In vielen Fällen müssen Nachrichten mit bestimmten Merkmalen auf unterschiedliche Weise verarbeitet werden. Damit dies möglich ist, können Sie Abonnements so konfigurieren, dass sie nach Nachrichten mit bestimmten Eigenschaften suchen und dann bestimmte Änderungen an diesen Eigenschaften vornehmen. Auch wenn Service Bus-Abonnements alle Nachrichten angezeigt werden, die an das Thema gesendet wurden, können Sie nur eine Teilmenge dieser Nachrichten in die virtuelle Abonnementwarteschlange kopieren. Dies wird mithilfe von Abonnementfiltern erreicht. Solche Änderungen werden als *Filteraktionen* bezeichnet. Beim Erstellen eines Abonnements können Sie einen Filterausdruck angeben, der auf die Eigenschaften der Nachricht angewendet wird, und zwar sowohl auf die Systemeigenschaften (z.B. **Label**) als auch auf benutzerdefinierte Anwendungseigenschaften (z.B. **StoreName**.) Der SQL-Filterausdruck ist in diesem Fall optional. Ohne SQL-Filterausdruck wird jede für ein Abonnement definierte Filteraktion für alle Nachrichten in diesem Abonnement ausgeführt.

Wenn Sie im vorherigen Beispiel nur Nachrichten von **Store1** herausfiltern möchten, erstellen Sie das Dashboard-Abonnement wie folgt:

```csharp
namespaceManager.CreateSubscription("IssueTrackingTopic", "Dashboard", new SqlFilter("StoreName = 'Store1'"));
```

Mit diesem Abonnementfilter werden nur Nachrichten, deren Eigenschaft `StoreName` auf `Store1` festgelegt ist, in die virtuelle Warteschlange für das Abonnement `Dashboard` kopiert.

Weitere Informationen über mögliche Filterwerte finden Sie in der Dokumentation zu den Klassen [SqlFilter](/dotnet/api/microsoft.servicebus.messaging.sqlfilter) und [SqlRuleAction](/dotnet/api/microsoft.servicebus.messaging.sqlruleaction). Weitere Informationen finden Sie außerdem in den Beispielen [Brokermessaging: Erweiterte Filter](http://code.msdn.microsoft.com/Brokered-Messaging-6b0d2749) und [Themenfilter](https://github.com/Azure-Samples/azure-servicebus-messaging-samples/tree/master/TopicFilters).

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen und Beispiele für die Verwendung von Messaging mit Service Bus finden Sie in den folgenden erweiterten Themen.

* [Übersicht über Service Bus-Messaging](service-bus-messaging-overview.md)
* [.NET-Tutorial zu Service Bus-Brokermessaging](service-bus-brokered-tutorial-dotnet.md)
* [REST-Tutorial zu Service Bus-Brokermessaging](service-bus-brokered-tutorial-rest.md)
* [Themenfilter – Beispiel](https://github.com/Azure/azure-service-bus/tree/master/samples/DotNet/Microsoft.ServiceBus.Messaging/TopicFilters)
* [Brokermessaging: Beispiel für erweiterte Filter](http://code.msdn.microsoft.com/Brokered-Messaging-6b0d2749)


