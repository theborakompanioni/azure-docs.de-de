<properties 
    pageTitle="Service Bus-Transaktionen | Microsoft Azure" 
    description="Übersicht über atomare Azure Service Bus-Transaktionen und „send via“" 
    services="service-bus" 
    documentationCenter=".net" 
    authors="sethmanheim" 
    manager="timlt" 
    editor=""/>

<tags
    ms.service="service-bus"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="na" 
    ms.date="05/16/2016"
    ms.author="clemensv;sethm"/>

# Übersicht über die Service Bus-Transaktionsverarbeitung

Dieser Artikel beschreibt die Transaktionsfunktionen von Azure Service Bus. Ein Großteil der Diskussion wird in dem Beispiel [Atomic Transactions with Service Bus sample](https://github.com/Azure-Samples/azure-servicebus-messaging-samples/tree/master/AtomicTransactions) (Beispiel: Atomare Transaktionen mit Service Bus) veranschaulicht. Dieser Artikel bietet lediglich eine Übersicht über die Transaktionsverarbeitung in Service Bus und die Service Bus-Funktion *send via* (Senden über), während das Beispiel zu den atomaren Transaktionen umfangreicher und komplexer ist.

## Transaktionen in Service Bus

Eine [Transaktion](https://github.com/Azure-Samples/azure-servicebus-messaging-samples/tree/master/AtomicTransactions#what-are-transactions) gruppiert zwei oder mehr Vorgänge in einem *Ausführungsbereich*. Solch eine Transaktion muss von sich aus sicherstellen, dass alle Vorgänge, die zu einer bestimmten Gruppe von Vorgängen gehören, entweder gemeinsam gelingen oder gemeinsam fehlschlagen. In dieser Hinsicht agieren Transaktionen als eine Einheit. Dieses Verhalten wird häufig als *Unteilbarkeit* bezeichnet.

Service Bus ist ein Transaktionsnachrichtenbroker und gewährleistet die Transaktionsintegrität für alle internen Vorgänge in seinen Nachrichtenspeichern. Alle Nachrichtenübertragungen innerhalb von Service Bus wie das Verschieben von Nachrichten in eine [Warteschlange für unzustellbare Nachrichten ](service-bus-dead-letter-queues.md) oder die [automatische Weiterleitung](service-bus-auto-forwarding.md) von Nachrichten zwischen Entitäten werden als Transaktionen bezeichnet. Wenn Service Bus also eine Meldung akzeptiert, wurde diese bereits gespeichert und mit einer Sequenznummer versehen. Alle darauf folgenden Nachrichtenübermittlungen innerhalb von Service Bus sind koordinierte Vorgänge zwischen Entitäten und führen weder zum Verlust (Quelle erfolgreich, Ziel schlägt fehl) noch zur Duplizierung (Quelle schlägt fehl, Ziel erfolgreich) der Nachricht.

Service Bus unterstützt Gruppierungsvorgänge für eine einzelne Nachrichtenentität (Warteschlange, Thema, Abonnement) innerhalb eines Transaktionsbereichs. Sie können beispielsweise mehrere Nachrichten aus einem Transaktionsbereich an eine Warteschlange senden. Die Nachrichten werden allerdings erst im Warteschlangenprotokoll committet, wenn die Transaktion erfolgreich abgeschlossen wurde.

## Vorgänge innerhalb eines Transaktionsbereichs 

Die innerhalb eines Transaktionsbereichs ausführbaren Vorgänge sind:

- **[QueueClient](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queueclient.aspx), [MessageSender](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagesender.aspx), [TopicClient](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.topicclient.aspx)**: Send, SendAsync, SendBatch, SendBatchAsync 

- **[BrokeredMessage](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.aspx)**: Complete, CompleteAsync, Abandon, AbandonAsync, Deadletter, DeadletterAsync, Defer, DeferAsync, RenewLock, RenewLockAsync

Empfangsvorgänge sind nicht enthalten, da davon ausgegangen wird, dass die Anwendung Nachrichten mithilfe des Modus [ReceiveMode.PeekLock](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.receivemode.aspx) in einer Empfangsschleife oder über einen [OnMessage](https://msdn.microsoft.com/library/azure/dn369601.aspx)-Rückruf erhält. Erst dann öffnet die Anwendung einen Transaktionsbereich für die Verarbeitung der Nachricht.

Die Disposition der Nachricht (vollständig, verworfen, unzustellbar, zurückgestellt) tritt innerhalb des Bereichs und abhängig von dem Gesamtergebnis der Transaktion auf.

## Übertragungen und „send via“

Service Bus unterstützt *Übertragungen*, um Transaktionsübergaben von Daten aus einer Warteschlange an einen Prozessor und von dem Prozessor an eine weitere Warteschlange zu ermöglichen. Bei einem Übertragungsvorgang sendet ein Sender eine Nachricht zuerst an eine Übertragungswarteschlange. Die Übertragungswarteschlange verschiebt die Nachricht sofort zu der gewünschten Zielwarteschlange und verwendet dazu dieselbe stabile Übertragungsimplementierung, auf der auch die automatische Weiterleitung basiert. Die Nachricht wird im Protokoll der Übertragungswarteschlange niemals so committet, dass sie für die Consumer der Übertragungswarteschlange sichtbar wird.

Wenn die Übertragungswarteschlange selbst die Quelle der eingehenden Nachrichten des Absenders darstellt, offenbart sich die Leistungsfähigkeit dieser Transaktionsfunktion. Das bedeutet, dass Service Bus die Nachricht „via“ Übertragungswarteschlange an die Zielwarteschlange übermitteln kann. Gleichzeitig führt Service Bus einen vollständigen (zurückgestellten oder unzustellbaren) Vorgang für die Eingabenachricht aus, alles in einer atomaren Operation.

### Codebeispiel

Erstellen Sie einen Nachrichtenabsender, der sich über die Übertragungswarteschlange an die Zielwarteschlange richtet, um solche Übertragungen einzurichten. Sie benötigen außerdem einen Empfänger, der Nachrichten aus derselben Warteschlange abruft. Beispiel:

```
var sender = this.messagingFactory.CreateMessageSender(destinationQueue, myQueueName);
var receiver = this.messagingFactory.CreateMessageReceiver(myQueueName);
```

Eine einfache Transaktion verwendet diese Elemente anschließend wie im folgenden Beispiel:

```
var msg = receiver.Receive();

using (scope = new TransactionScope())
{
    // Do whatever work is required 

    var newmsg = ... // package the result 

    msg.Complete(); // mark the message as done
    sender.Send(newmsg); // forward the result

    scope.Complete(); // declare the transaction done
} 
```

## Nächste Schritte

Weitere Informationen zu Service Bus-Warteschlangen finden Sie in den folgenden Artikeln:

- [Verketten von Service Bus-Entitäten mit automatischer Weiterleitung](service-bus-auto-forwarding.md)
- [Auto Forward](https://github.com/Azure-Samples/azure-servicebus-messaging-samples/tree/master/AutoForward) (Automatische Weiterleitung)
- [Atomic Transactions with Service Bus](https://github.com/Azure-Samples/azure-servicebus-messaging-samples/tree/master/AtomicTransactions) (Atomare Transaktionen mit Service Bus)
- [Vergleich von Azure-Warteschlangen und Service Bus-Warteschlangen](service-bus-azure-and-service-bus-queues-compared-contrasted.md)
- [Verwenden von Service Bus-Warteschlangen](service-bus-dotnet-get-started-with-queues.md)

<!---HONumber=AcomDC_0608_2016-->