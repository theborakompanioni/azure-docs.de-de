---
title: "Service Bus-Warteschlangen für unzustellbare Nachrichten | Microsoft Docs"
description: "Übersicht über Azure Service Bus-Warteschlangen für unzustellbare Nachrichten"
services: service-bus-messaging
documentationcenter: .net
author: sethmanheim
manager: timlt
editor: 
ms.assetid: 68b2aa38-dba7-491a-9c26-0289bc15d397
ms.service: service-bus-messaging
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/17/2017
ms.author: clemensv;sethm
ms.translationtype: Human Translation
ms.sourcegitcommit: 95b8c100246815f72570d898b4a5555e6196a1a0
ms.openlocfilehash: c16bcf30ab96f79e59404a41852e4cd227e28b08
ms.contentlocale: de-de
ms.lasthandoff: 05/18/2017


---
# <a name="overview-of-service-bus-dead-letter-queues"></a>Übersicht über Service Bus-Warteschlangen für unzustellbare Nachrichten

Service Bus-Warteschlangen und -Themenabonnements bieten eine sekundäre Unterwarteschlange, die als *Warteschlange für unzustellbare Nachrichten* (dead letter queue, DLQ) bezeichnet wird. Die Warteschlange für unzustellbare Nachrichten muss nicht explizit erstellt und kann nicht gelöscht oder anderweitig unabhängig von der Hauptentität verwaltet werden.

Dieser Artikel erläutert Warteschlangen für unzustellbare Nachrichten in Azure Service Bus. Ein Großteil der Informationen wird durch das Beispiel [Dead-Letter Queues](https://github.com/Azure/azure-service-bus/tree/master/samples/DotNet/Microsoft.ServiceBus.Messaging/DeadletterQueue) auf GitHub veranschaulicht.
 
## <a name="the-dead-letter-queue"></a>Die Warteschlange für unzustellbare Nachrichten

Die Warteschlange für unzustellbare Nachrichten dient zum Speichern von Nachrichten, die an keinen Empfänger übermittelt oder nicht verarbeitet werden können. Nachrichten können dann aus dieser Warteschlange entfernt und überprüft werden. Eine Anwendung kann mithilfe eines Operators Probleme beheben und die Nachricht erneut senden, den Fehler protokollieren und korrigierende Maßnahmen ergreifen. 

Im Hinblick auf API und Protokoll ähnelt die Warteschlange für unzustellbare Nachrichten weitgehend anderen Warteschlangen, mit der Ausnahme, dass Nachrichten nur durch die übergeordnete Entität über den Vorgang für unzustellbare Nachrichten gesendet werden können. Darüber hinaus ist die Gültigkeitsdauer nicht relevant, und eine Nachricht kann nicht von einer Warteschlange für unzustellbare Nachrichten aus als unzustellbar gekennzeichnet werden. Die Warteschlange für unzustellbare Nachrichten bietet vollständige Unterstützung für die Peek/Lock-Übermittlung und Transaktionsvorgänge.

Beachten Sie, dass die Warteschlange für unzustellbare Nachrichten nicht automatisch bereinigt wird. Nachrichten verbleiben in dieser Warteschlange, bis Sie sie explizit daraus abrufen und für die unzustellbare Nachricht [Complete()](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage#Microsoft_ServiceBus_Messaging_BrokeredMessage_CompleteAsync) aufrufen.

## <a name="moving-messages-to-the-dlq"></a>Verschieben von Nachrichten in die Warteschlange für unzustellbare Nachrichten

Es gibt verschiedene Aktivitäten in Service Bus, durch welche Nachrichten aus dem Messagingmodul selbst per Push in die Warteschlange für unzustellbare Nachrichten übertragen werden. Eine Anwendung kann Nachrichten auch explizit in die Warteschlange für unzustellbare Nachrichten verschieben. 

Während die Nachricht vom Broker verschoben wird, werden ihr zwei Eigenschaften hinzugefügt, wenn der Broker die interne Version der [DeadLetter](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage#Microsoft_ServiceBus_Messaging_BrokeredMessage_DeadLetter_System_String_System_String_)-Methode für die Nachricht aufruft: `DeadLetterReason` und `DeadLetterErrorDescription`.

Anwendungen können ihre eigenen Codes für die `DeadLetterReason`-Eigenschaft definieren, aber das System legt die folgenden Werte fest.

| Bedingung | DeadLetterReason | DeadLetterErrorDescription |
| --- | --- | --- |
| Always |HeaderSizeExceeded |Das Größenkontingent für diesen Datenstrom wurde überschritten. |
| !TopicDescription.<br />EnableFilteringMessagesBeforePublishing und SubscriptionDescription.<br />EnableDeadLetteringOnFilterEvaluationExceptions |exception.GetType().Name |exception.Message |
| EnableDeadLetteringOnMessageExpiration |TTLExpiredException |Die Nachricht ist abgelaufen und wurde in die Warteschlange für unzustellbare Nachrichten verschoben. |
| SubscriptionDescription.RequiresSession |Sitzungs-ID ist NULL. |Die für die Sitzung aktivierte Entität erlaubt keine Nachricht, deren Sitzungsbezeichner null ist. |
| !dead letter queue |MaxTransferHopCountExceeded |Null |
| Explizites Markieren von Nachrichten als unzustellbar durch Anwendung |Gemäß Anwendung |Gemäß Anwendung |

## <a name="exceeding-maxdeliverycount"></a>Überschreiten von MaxDeliveryCount
Warteschlangen und Abonnements haben die Eigenschaft [QueueDescription.MaxDeliveryCount](/dotnet/api/microsoft.servicebus.messaging.queuedescription#Microsoft_ServiceBus_Messaging_QueueDescription_MaxDeliveryCount) bzw. [SubscriptionDescription.MaxDeliveryCount](/dotnet/api/microsoft.servicebus.messaging.subscriptiondescription#Microsoft_ServiceBus_Messaging_SubscriptionDescription_MaxDeliveryCount). Der jeweilige Standardwert ist 10. Jedes Mal, wenn eine Nachricht unter einer Sperre übermittelt wurde ([ReceiveMode.PeekLock](/dotnet/api/microsoft.servicebus.messaging.receivemode)), aber entweder explizit abgebrochen wurde oder die Sperre abgelaufen ist, wird der Wert [BrokeredMessage.DeliveryCount](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage#Microsoft_ServiceBus_Messaging_BrokeredMessage_DeliveryCount) für die Nachricht erhöht. Wenn [DeliveryCount](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage#Microsoft_ServiceBus_Messaging_BrokeredMessage_DeliveryCount) den Wert [MaxDeliveryCount](/dotnet/api/microsoft.servicebus.messaging.queuedescription#Microsoft_ServiceBus_Messaging_QueueDescription_MaxDeliveryCount) überschreitet, wird die Nachricht in die Warteschlange für unzustellbare Nachrichten verschoben, und der Ursachencode `MaxDeliveryCountExceeded` wird angegeben.

Dieses Verhalten kann nicht deaktiviert werden, aber Sie können [MaxDeliveryCount](/dotnet/api/microsoft.servicebus.messaging.queuedescription#Microsoft_ServiceBus_Messaging_QueueDescription_MaxDeliveryCount) auf eine sehr hohe Zahl festlegen.

## <a name="exceeding-timetolive"></a>Überschreiten von TimeToLive
Wenn die Eigenschaft [QueueDescription.EnableDeadLetteringOnMessageExpiration](/dotnet/api/microsoft.servicebus.messaging.queuedescription#Microsoft_ServiceBus_Messaging_QueueDescription_EnableDeadLetteringOnMessageExpiration) bzw. [SubscriptionDescription.EnableDeadLetteringOnMessageExpiration](/dotnet/api/microsoft.servicebus.messaging.subscriptiondescription#Microsoft_ServiceBus_Messaging_SubscriptionDescription_EnableDeadLetteringOnMessageExpiration) auf **TRUE** festgelegt ist (die Standardeinstellung lautet **FALSE**), werden alle ablaufenden Nachrichten in die Warteschlange für unzustellbare Nachrichten verschoben, und der Ursachencode `TTLExpiredException` wird angegeben.

Beachten Sie, dass abgelaufene Nachrichten nur bereinigt und daher in die Warteschlange für unzustellbare Nachrichten verschoben werden, wenn mindestens ein aktiver Empfänger per Pull aus der Hauptwarteschlange oder dem Abonnement abruft. Dieses Verhalten ist beabsichtigt.

## <a name="errors-while-processing-subscription-rules"></a>Fehler beim Verarbeiten von Regeln für Abonnements
Wenn die [SubscriptionDescription.EnableDeadLetteringOnFilterEvaluationExceptions](/dotnet/api/microsoft.servicebus.messaging.subscriptiondescription#Microsoft_ServiceBus_Messaging_SubscriptionDescription_EnableDeadLetteringOnFilterEvaluationExceptions)-Eigenschaft für ein Abonnement aktiviert ist, werden Fehler, die während der Ausführung der SQL-Filterregel eines Abonnements auftreten, zusammen mit der betreffenden Nachricht in der Warteschlange für unzustellbare Nachrichten erfasst.

## <a name="application-level-dead-lettering"></a>Unzustellbare Nachrichten auf Anwendungsebene
Zusätzlich zu den vom System bereitgestellten Features für unzustellbare Nachrichten können Anwendungen die Warteschlange für unzustellbare Nachrichten verwenden, um nicht annehmbare Nachrichten explizit abzulehnen. Dazu zählen beispielsweise Nachrichten, die aufgrund eines beliebigen Systemproblems nicht ordnungsgemäß verarbeitet werden können, Nachrichten mit fehlerhaften Nutzlasten oder Nachrichten, bei denen Authentifizierungsfehler auftreten, wenn ein Sicherheitsschema auf Nachrichtenebene verwendet wird.

## <a name="dead-lettering-in-forwardto-or-sendvia-scenarios"></a>Unzustellbare Nachrichten bei ForwardTo- oder SendVia-Szenarien

Nachrichten werden in den folgenden Situationen an die Warteschlange für unzustellbare Nachrichten übermittelt:

- Eine Meldung durchläuft mehr als drei Warteschlangen oder Themen, die [miteinander verkettet](service-bus-auto-forwarding.md) sind.
- Die Zielwarteschlange oder das Zielthema wurde deaktiviert oder gelöscht.
- Die Zielwarteschlange oder das Zielthema überschreitet die maximale Entitätsgröße.

Zum Abrufen dieser unzustellbaren Nachrichten können Sie mithilfe der [FormatTransferDeadletterPath](/dotnet/api/microsoft.servicebus.messaging.queueclient#Microsoft_ServiceBus_Messaging_QueueClient_FormatTransferDeadLetterPath_System_String_)-Hilfsmethode einen Empfänger erstellen.

## <a name="example"></a>Beispiel
Der folgende Codeausschnitt erstellt einen Nachrichtenempfänger. In der Nachrichtenempfangsschleife für die Hauptwarteschlange ruft der Code die Nachricht mit [Receive(TimeSpan.Zero)](/dotnet/api/microsoft.servicebus.messaging.messagereceiver#Microsoft_ServiceBus_Messaging_MessageReceiver_Receive_System_TimeSpan_) ab. Dadurch wird der Broker aufgefordert, sofort alle bereitstehenden Nachrichten bzw. kein Ergebnis zurückzugeben. Wenn der Code eine Nachricht empfängt, wird diese sofort abgebrochen und `DeliveryCount` wird erhöht. Sobald das System die Nachricht in die Warteschlange für unzustellbare Nachrichten verschiebt, ist die Hauptwarteschlange leer und die Schleife wird beendet, da [ReceiveAsync](/dotnet/api/microsoft.servicebus.messaging.messagereceiver#Microsoft_ServiceBus_Messaging_MessageReceiver_ReceiveAsync_System_TimeSpan_) den Wert **null** zurückgibt.

```csharp
var receiver = await receiverFactory.CreateMessageReceiverAsync(queueName, ReceiveMode.PeekLock);
while(true)
{
    var msg = await receiver.ReceiveAsync(TimeSpan.Zero);
    if (msg != null)
    {
        Console.WriteLine("Picked up message; DeliveryCount {0}", msg.DeliveryCount);
        await msg.AbandonAsync();
    }
    else
    {
        break;
    }
}
```

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen zu Service Bus-Warteschlangen finden Sie in den folgenden Artikeln:

* [Erste Schritte mit Service Bus-Warteschlangen](service-bus-dotnet-get-started-with-queues.md)
* [Vergleich von Azure-Warteschlangen und Service Bus-Warteschlangen](service-bus-azure-and-service-bus-queues-compared-contrasted.md)


