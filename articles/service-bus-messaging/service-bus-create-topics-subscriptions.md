---
title: Erstellen von Anwendungen, die Azure Service Bus-Themen und -Abonnements verwenden | Microsoft-Dokumentation
description: "Einführung in die Funktionen zum Veröffentlichen/Abonnieren von Service Bus-Themen und -Abonnements"
services: service-bus-messaging
documentationcenter: na
author: sethmanheim
manager: timlt
editor: 
ms.assetid: a48fc9b0-b7b0-464e-8187-a517bf8b4eb4
ms.service: service-bus-messaging
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/07/2017
ms.author: sethm
ms.translationtype: Human Translation
ms.sourcegitcommit: 0c4554d6289fb0050998765485d965d1fbc6ab3e
ms.openlocfilehash: 75dbbff8e666ed6c9b11fcfc1aa5be0eb759a0bb
ms.contentlocale: de-de
ms.lasthandoff: 04/13/2017

---
# <a name="create-applications-that-use-service-bus-topics-and-subscriptions"></a>Erstellen von Anwendungen, die Service Bus-Themen und -Abonnements verwenden
Azure Service Bus unterstützt einen Satz cloudbasierter, nachrichtenorientierter Middlewaretechnologien, darunter zuverlässiges Message Queuing und dauerhaftes Veröffentlichungs-/Abonnementmessaging. Dieser Artikel baut auf den Informationen des Artikels [Erstellen von Anwendungen, die Service Bus-Warteschlangen verwenden](service-bus-create-queues.md) auf und enthält eine Einführung in die Funktionen zum Veröffentlichen/Abonnieren, die von Service Bus-Themen bereitgestellt werden.

## <a name="evolving-retail-scenario"></a>Weiterentwickeln des Einzelhandelsszenarios
In diesem Artikel wird das Einzelhandelsszenario fortgeführt, das bereits unter [Erstellen von Anwendungen, die Service Bus-Warteschlangen verwenden](service-bus-create-queues.md) verwendet wurde. Wie Sie sich sicherlich erinnern, müssen Verkaufsdaten aus einzelnen Point-of-Sale (POS)-Terminals an ein Lagerverwaltungssystem weitergeleitet werden, in dem anhand dieser Daten festgestellt wird, ob Bestände aufgefüllt werden müssen. Jedes POS-Terminal meldet seine Verkaufsdaten durch Senden von Nachrichten an die **DataCollectionQueue**-Warteschlange, in der die Nachrichten verbleiben, bis sie vom Lagerverwaltungssystem empfangen werden (siehe Abbildung):

![Service Bus 1](./media/service-bus-create-topics-subscriptions/IC657161.gif)

Zur Weiterentwicklung dieses Szenarios wurde dem System eine neue Anforderung hinzugefügt: Der Ladenbesitzer möchte die Möglichkeit haben, die Geschäftsentwicklung in Echtzeit zu überwachen.

Damit diese Anforderung umgesetzt werden kann, muss das System den Datenstrom der Verkaufsdaten "überwachen". Nach wie vor soll jede von den POS-Terminals gesendete Nachricht an das Lagerverwaltungssystem gesendet werden. Von jeder Nachricht soll jedoch auch eine Kopie erstellt werden, die dazu verwendet werden kann, dem Ladenbesitzer die Dashboardansicht zu präsentieren.

In einem solchen Fall, bei dem jede Nachricht von mehreren Komponenten genutzt werden soll, können Sie Service Bus-*Themen* verwenden. Themen umfassen ein Veröffentlichungs- und Abonnementmuster, mit dem jede veröffentlichte Nachricht für die Abonnements zur Verfügung gestellt wird, die für das entsprechende Thema registriert sind. Im Gegensatz dazu wird bei Warteschlangen jede Nachricht von einem einzigen Consumer empfangen.

Nachrichten werden auf die gleiche Weise an ein Thema gesendet wie an eine Warteschlange. Die Nachrichten werden jedoch nicht direkt vom jeweiligen Thema, sondern stattdessen von Abonnements empfangen. Sie können sich ein Abonnement für ein Thema als virtuelle Warteschlange vorstellen, die Kopien der Nachrichten empfängt, die an das Thema gesendet wurden. Nachrichten werden von einem Abonnement auf die gleiche Weise empfangen wie von einer Warteschlange.

Zurück zum Einzelhandelsszenario: Die Warteschlange wird durch ein Thema ersetzt. Zudem wird ein Abonnement hinzugefügt, das vom Lagerverwaltungssystem verwendet werden kann. Das System sieht nun folgendermaßen aus:

![Service Bus 2](./media/service-bus-create-topics-subscriptions/IC657165.gif)

Diese Konfiguration entspricht in der Ausführung dem vorherigen warteschlangenbasierten Entwurf. Das heißt, die an das Thema gesendeten Nachrichten werden an das Abonnement **Inventory** weitergeleitet, aus dem das **Lagerverwaltungssystem** die Nachrichten abruft.

Damit das Management-Dashboard unterstützt wird, muss wie im Folgenden gezeigt ein zweites Abonnement für das Thema erstellt werden:

![Service Bus 3](./media/service-bus-create-topics-subscriptions/IC657166.gif)

Bei dieser Konfiguration wird jede Nachricht von den POS-Terminals sowohl dem Abonnement **Dashboard** als auch dem Abonnement **Inventory** zur Verfügung gestellt.

## <a name="show-me-the-code"></a>Anwendungscode
Unter [Erstellen von Anwendungen, die Service Bus-Warteschlangen verwenden](service-bus-create-queues.md) wird beschrieben, wie Sie sich für ein Azure-Konto registrieren und einen Dienstnamespace erstellen. Die einfachste Möglichkeit zum Verweisen auf Service Bus-Abhängigkeiten besteht darin, das [NuGet-Paket](https://www.nuget.org/packages/WindowsAzure.ServiceBus/) „Service Bus“ zu installieren. Die Service Bus-Bibliotheken sind auch Bestandteil des Azure SDK. und kann auf der [Azure SDK-Downloadseite](https://azure.microsoft.com/downloads/) heruntergeladen werden.

### <a name="create-the-topic-and-subscriptions"></a>Erstellen des Themas und der Abonnements
Verwaltungsvorgänge für Service Bus-Nachrichtenentitäten (Warteschlangen und Veröffentlichen/Abonnieren von Themen) werden über die [NamespaceManager](/dotnet/api/microsoft.servicebus.namespacemanager#microsoft_servicebus_namespacemanager)-Klasse ausgeführt. Für die Erstellung einer **NamespaceManager**-Instanz für einen bestimmten Dienstnamespace werden gültige Anmeldeinformationen benötigt. Service Bus verwendet ein [SAS (Shared Access Signature)](service-bus-sas.md)-basiertes Sicherheitsmodell. Die [TokenProvider](/dotnet/api/microsoft.servicebus.tokenprovider#microsoft_servicebus_tokenprovider)-Klasse stellt einen Sicherheitstokenanbieter mit integrierten Factorymethoden dar, die einige bekannte Tokenanbieter zurückgeben. Wir verwenden die [CreateSharedAccessSignatureTokenProvider](/dotnet/api/microsoft.servicebus.tokenprovider#Microsoft_ServiceBus_TokenProvider_CreateSharedAccessSignatureTokenProvider_System_String_)-Methode, um die SAS-Anmeldeinformationen zu speichern. Die [NamespaceManager](/dotnet/api/microsoft.servicebus.namespacemanager#microsoft_servicebus_namespacemanager)-Instanz wird dann mit der Basisadresse des Service Bus-Namespaces und dem Tokenanbieter erstellt.

Die [NamespaceManager](/dotnet/api/microsoft.servicebus.namespacemanager#microsoft_servicebus_namespacemanager)-Klasse enthält Methoden zum Erstellen, Aufzählen und Löschen von Nachrichtenentitäten. Mit dem folgenden Code wird gezeigt, wie die **NamespaceManager**-Instanz erstellt und zum Erstellen des Themas **DataCollectionTopic** verwendet wird.

```csharp
Uri uri = ServiceBusEnvironment.CreateServiceUri("sb", "test-blog", string.Empty);
string name = "RootManageSharedAccessKey";
string key = "abcdefghijklmopqrstuvwxyz";

TokenProvider tokenProvider = TokenProvider.CreateSharedAccessSignatureTokenProvider(name, key);
NamespaceManager namespaceManager = new NamespaceManager(uri, tokenProvider);

namespaceManager.CreateTopic("DataCollectionTopic");
```

Beachten Sie die Überladungen der [CreateTopic](/dotnet/api/microsoft.servicebus.namespacemanager#Microsoft_ServiceBus_NamespaceManager_CreateTopic_System_String_)-Methode, mit denen die Eigenschaften des Themas festgelegt werden können. So können Sie beispielsweise den Standardwert für die Gültigkeitsdauer (TTL) für an das Thema gesendete Nachrichten festlegen. Fügen Sie als Nächstes die Abonnements **Inventory** und **Dashboard** hinzu.

```csharp
namespaceManager.CreateSubscription("DataCollectionTopic", "Inventory");
namespaceManager.CreateSubscription("DataCollectionTopic", "Dashboard");
```

### <a name="send-messages-to-the-topic"></a>Senden von Nachrichten an das Thema
Für Vorgänge zur Laufzeit für Service Bus-Entitäten, z.B. Senden und Empfangen von Nachrichten, muss eine Anwendung zunächst ein [MessagingFactory](/dotnet/api/microsoft.servicebus.messaging.messagingfactory#microsoft_servicebus_messaging_messagingfactory)-Objekt erstellen. Ähnlich wie die [NamespaceManager](/dotnet/api/microsoft.servicebus.namespacemanager#microsoft_servicebus_namespacemanager)-Klasse wird die **MessagingFactory**-Instanz aus der Basisadresse des Dienstnamespaces und dem Tokenanbieter erstellt.

```
MessagingFactory factory = MessagingFactory.Create(uri, tokenProvider);
```

Nachrichten, die an die Service Bus-Themen gesendet und von diesen empfangen werden, sind Instanzen der [BrokeredMessage](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage)-Klasse. Diese Klasse besteht aus einem Satz von Standardeigenschaften (z.B. [Label](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage.label?view=azureservicebus-4.0.0#Microsoft_ServiceBus_Messaging_BrokeredMessage_Label) und [TimeToLive](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage.timetolive?view=azureservicebus-4.0.0#Microsoft_ServiceBus_Messaging_BrokeredMessage_TimeToLive)), einem Wörterbuch, in dem Anwendungseigenschaften enthalten sind, und einem Bestand beliebiger Anwendungsdaten. Eine Anwendung kann durch Übergeben eines beliebigen serialisierbaren Objekts den Bestand festlegen (im folgenden Beispiel wird ein **SalesData**-Objekt übergeben, das die Verkaufsdaten vom POS-Terminal darstellt), der mithilfe von [DataContractSerializer](https://msdn.microsoft.com/library/system.runtime.serialization.datacontractserializer.aspx) das Objekt serialisiert. Alternativ kann ein [Stream](https://msdn.microsoft.com/library/system.io.stream.aspx)-Objekt angegeben werden.

```csharp
BrokeredMessage bm = new BrokeredMessage(salesData);
bm.Label = "SalesReport";
bm.Properties["StoreName"] = "Redmond";
bm.Properties["MachineID"] = "POS_1";
```

Die einfachste Möglichkeit zum Senden von Nachrichten an das Thema ist die Verwendung von [CreateMessageSender](/dotnet/api/microsoft.servicebus.messaging.messagingfactory#Microsoft_ServiceBus_Messaging_MessagingFactory_CreateMessageSender_System_String_) zum Erstellen eines [MessageSender](/dotnet/api/microsoft.servicebus.messaging.messagesender)-Objekts direkt aus der [MessagingFactory](/dotnet/api/microsoft.servicebus.messaging.messagingfactory)-Instanz:

```csharp
MessageSender sender = factory.CreateMessageSender("DataCollectionTopic");
sender.Send(bm);
```

### <a name="receive-messages-from-a-subscription"></a>Empfangen von Nachrichten aus einem Abonnement
Wie beim Verwenden von Warteschlangen können Sie zum Empfangen von Nachrichten aus einem Abonnement ein [MessageReceiver](/dotnet/api/microsoft.servicebus.messaging.messagereceiver)-Objekt verwenden, das Sie unter Verwendung von [CreateMessageReceiver](/dotnet/api/microsoft.servicebus.messaging.messagingfactory) direkt aus [MessagingFactory](/dotnet/api/microsoft.servicebus.messaging.messagingfactory#Microsoft_ServiceBus_Messaging_MessagingFactory_CreateMessageReceiver_System_String_) erstellen. Sie können einen der beiden Empfangsmodi (**ReceiveAndDelete** und **PeekLock**) verwenden. Die entsprechenden Informationen finden Sie unter [Erstellen von Anwendungen, die Service Bus-Warteschlangen verwenden](service-bus-create-queues.md).

Wenn Sie ein **MessageReceiver**-Objekt für Abonnements erstellen, hat der *entityPath*-Parameter das Format `topicPath/subscriptions/subscriptionName`. Zum Erstellen einer **MessageReceiver**-Instanz für das Abonnement **Inventory** des Themas **DataCollectionTopic** muss *entityPath* daher auf `DataCollectionTopic/subscriptions/Inventory` festgelegt werden. Der Code sieht wie folgt aus:

```csharp
MessageReceiver receiver = factory.CreateMessageReceiver("DataCollectionTopic/subscriptions/Inventory");
BrokeredMessage receivedMessage = receiver.Receive();
try
{
    ProcessMessage(receivedMessage);
    receivedMessage.Complete();
}
catch (Exception e)
{
    receivedMessage.Abandon();
}
```

## <a name="subscription-filters"></a>Abonnementfilter
Bislang werden in diesem Szenario alle Nachrichten, die an das Thema gesendet wurden, allen registrierten Abonnements zur Verfügung gestellt. Der Kernpunkt lautet "zur Verfügung gestellt". Auch wenn für Service Bus-Abonnements alle Nachrichten angezeigt werden, die an das Thema gesendet wurden, können Sie nur eine Teilmenge dieser Nachrichten in die virtuelle Abonnementwarteschlange kopieren. Dies erfolgt mithilfe von *Abonnementfiltern*. Wenn Sie ein Abonnement erstellen, können Sie einen Filterausdruck angeben, der das Format eines SQL92-Prädikats hat und auf die Eigenschaften der Nachricht angewendet wird, und zwar sowohl auf die Systemeigenschaften (z.B. [Label](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage#Microsoft_ServiceBus_Messaging_BrokeredMessage_Label)) als auch auf die Anwendungseigenschaften (z.B. **StoreName** aus dem vorherigen Beispiel).

Das Szenario soll erweitert werden, um dies zu veranschaulichen. Dazu wird dem Einzelhandelsszenario ein zweites Geschäft hinzugefügt. Weiterhin müssen die Verkaufsdaten von allen POS-Terminals aus beiden Geschäften an das zentrale Lagerverwaltungssystem weitergeleitet werden, aber ein Geschäftsführer, der mit dem Dashboardtool arbeitet, ist nur an der Geschäftsentwicklung des zweiten Ladens interessiert. Dies können Sie durch Filterung der Abonnements erreichen. Wenn die POS-Terminals Nachrichten veröffentlichen, wird die **StoreName**-Eigenschaft der Anwendung für die Nachrichten festgelegt. Bei zwei Läden, beispielsweise **Redmond** und **Seattle**, stempeln die POS-Terminals im Laden in Redmond ihre Verkaufsdatennachrichten in **StoreName** mit dem Namen **Redmond**, während die POS-Terminals im Laden in Seattle für **StoreName** den Namen **Seattle** verwenden. Der Geschäftsführer des Ladens in Redmond möchte nur Daten von seinen POS-Terminals sehen. Das System sieht nun wie folgt aus:

![Service-Bus4](./media/service-bus-create-topics-subscriptions/IC657167.gif)

Zum Einrichten dieser Weiterleitung erstellen Sie das Abonnement **Dashboard** wie folgt:

```csharp
SqlFilter dashboardFilter = new SqlFilter("StoreName = 'Redmond'");
namespaceManager.CreateSubscription("DataCollectionTopic", "Dashboard", dashboardFilter);
```

Mit diesem [Abonnementfilter](/dotnet/api/microsoft.servicebus.messaging.sqlfilter) werden nur Nachrichten, deren **StoreName**-Eigenschaft auf **Redmond** festgelegt ist, in die virtuelle Warteschlange für das Abonnement **Dashboard** kopiert. Mit der Abonnementfilterung eröffnen sich jedoch auch noch weitere Möglichkeiten. Für Anwendungen können mehrere Filterregeln pro Abonnement festgelegt werden. Zudem können die Eigenschaften einer Nachricht geändert werden, während diese in die virtuelle Warteschlange eines Abonnements weitergeleitet wird.

## <a name="summary"></a>Zusammenfassung
Alle Gründe, die für die Verwendung von Warteschlangen sprechen und die unter [Erstellen von Anwendungen, die Service Bus-Warteschlangen verwenden](service-bus-create-queues.md) beschrieben sind, gelten auch für die Verwendung von Themen. Insbesondere sind dies die folgenden Gründe:

* Zeitliche Entkopplung: Producer (Sender von Nachrichten) und Consumer (Empfänger von Nachrichten) müssen nicht gleichzeitig online sein.
* Belastungsausgleich: Lastspitzen werden vom Thema geglättet, sodass die Consumer-Anwendungen nur für die durchschnittliche Last anstelle der Spitzenlast bereitgestellt werden müssen.
* Lastenausgleich: Ähnlich wie bei einer Warteschlange können Sie mehrere Consumer festlegen, die ein einzelnes Abonnement überwachen, wobei jede Nachricht nur an einen der Consumer gesendet wird und so die Last ausgeglichen wird.
* Lose Kopplung: Sie können das Messagingnetzwerk ohne Auswirkung auf die vorhandenen Endpunkte erweitern, z.B. durch Hinzufügen von Abonnements zu einem Thema oder Ändern der Filtern, damit neue Consumer berücksichtigt werden können.

## <a name="next-steps"></a>Nächste Schritte

Informationen zur Verwendung von Warteschlangen im POS-Einzelhandelsszenario finden Sie unter [Erstellen von Anwendungen, die Service Bus-Warteschlangen verwenden](service-bus-create-queues.md).


