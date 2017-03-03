---
title: Schreiben von Anwendungen, die Azure Service Bus-Warteschlangen verwenden | Microsoft Docs
description: Erfahren Sie, wie Sie eine einfache warteschlangenbasierte Anwendung schreiben, bei der Azure Service Bus verwendet wird.
services: service-bus-messaging
documentationcenter: na
author: sethmanheim
manager: timlt
editor: 
ms.assetid: 754d91b3-1426-405e-84b4-fd36d65b114a
ms.service: service-bus-messaging
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/15/2017
ms.author: sethm
translationtype: Human Translation
ms.sourcegitcommit: d987aa22379ede44da1b791f034d713a49ad486a
ms.openlocfilehash: 40414edebcd76fc93136cbc14d7a6436fc7f6da5
ms.lasthandoff: 02/16/2017


---
# <a name="create-applications-that-use-service-bus-queues"></a>Erstellen von Anwendungen, die Service Bus-Warteschlangen verwenden
In diesem Thema werden Service Bus-Warteschlangen beschrieben. Zudem wird erläutert, wie Sie eine einfache warteschlangenbasierte Anwendung schreiben können, die Service Bus verwendet.

Nehmen wir ein Szenario aus dem Einzelhandel, bei dem Verkaufsdaten aus einzelnen Point-of-Sale (POS)-Terminals an ein Lagerverwaltungssystem weitergeleitet werden müssen, in dem anhand dieser Daten ermittelt wird, ob Bestände aufgefüllt werden müssen. Bei dieser Lösung wird Service Bus-Messaging für die Kommunikation zwischen den Terminals und dem Lagerverwaltungssystem eingesetzt, wie in der folgenden Abbildung dargestellt:

![Service Bus-Warteschlangen Bild 1](./media/service-bus-create-queues/IC657161.gif)

Jedes POS-Terminal meldet seine Verkaufsdaten durch Senden von Nachrichten an **DataCollectionQueue**. Diese Nachrichten verbleiben in dieser Warteschlange, bis sie vom Lagerverwaltungssystem abgerufen werden. Dieses Muster wird auch als *asynchrones Messaging* bezeichnet, da die Verarbeitung des POS-Terminals auch ohne Antwort vom Lagerverwaltungssystem fortgesetzt werden kann.

## <a name="why-queuing"></a>Warum Warteschlangen?
Bevor wir uns den Code für diese Anwendung ansehen, soll zunächst auf die Vorteile der Verwendung einer Warteschlange in diesem Szenario gegenüber der direkten (synchronen) Kommunikation zwischen den POS-Terminals und dem Lagerverwaltungssystem eingegangen werden.

### <a name="temporal-decoupling"></a>Zeitliche Entkopplung
Dank des asynchronen Nachrichtenmusters müssen Producer und Consumer nicht gleichzeitig online sein. Die Messaginginfrastruktur speichert Nachrichten zuverlässig, bis die Empfängerseite für den Empfang bereit ist. Auf diese Weise können die Komponenten verteilter Anwendungen ohne Beeinträchtigung des Gesamtsystems voneinander entkoppelt werden (etwa zu Wartungszwecken oder bei einem Komponentenausfall). Außerdem genügt es unter Umständen, wenn die konsumierende Anwendung nur zu bestimmten Tageszeiten online ist. In diesem Einzelhandelsszenario kann das Lagerverwaltungssystem beispielsweise nur am Ende des Geschäftstages online geschaltet werden.

### <a name="load-leveling"></a>Belastungsausgleich
In vielen Anwendungen schwankt die Systemlast mit der Zeit, während die Bearbeitungszeit pro Arbeitseinheit normalerweise konstant ist. Durch die Zwischenschaltung einer Warteschlange zwischen Nachrichtenproducer und -consumer muss der Consumer (Worker) anstelle der Spitzenlast nur die durchschnittliche Last verarbeiten können. Die Länge der Warteschlange schwankt ebenso wie die eingehende Last. Auf diese Weise sparen Sie direkt Geld, da Sie weniger Infrastruktur für Ihre Anwendungslast benötigen.

![Service Bus-Warteschlangen Bild 2](./media/service-bus-create-queues/IC657162.gif)

### <a name="load-balancing"></a>Lastenausgleich
Mit zunehmender Last können weitere Workerprozesse hinzugefügt werden, die Nachrichten aus der Workerwarteschlange abrufen. Jede Nachricht wird nur von einem der Workerprozesse verarbeitet. Außerdem ermöglicht dieser Pull-basierte Lastenausgleich eine optimale Nutzung der Workercomputer, selbst wenn diese über unterschiedliche Leistung verfügen, da jeder von ihnen die Nachrichten in der eigenen Maximalgeschwindigkeit abruft. Dieses Schema nennt man auch das "Konkurrierende Consumer"-Schema.

![Service Bus-Warteschlangen Bild 3](./media/service-bus-create-queues/IC657163.gif)

### <a name="loose-coupling"></a>Lose Kopplung
Die Verwendung von Nachrichtenwarteschlangen als Zwischenglied zwischen Nachrichtenproducern und -consumern sorgt für eine inhärente lose Kopplung zwischen den Komponenten. Da Producer und Consumer voneinander unabhängig sind, kann ein Upgrade für einen Consumer ohne Auswirkungen auf den Producer durchgeführt werden. Darüber hinaus kann die Nachrichtentopologie ohne Auswirkung auf die vorhandenen Endpunkte erweitert werden. Dies wird im Abschnitt zum Veröffentlichen/Abonnieren eingehender behandelt.

## <a name="show-me-the-code"></a>Anwendungscode
In den folgenden Abschnitten wird erläutert, wie diese Anwendung mithilfe von Service Bus erstellt werden kann.

### <a name="sign-up-for-an-azure-account"></a>Registrieren für ein Azure-Konto
Zur Verwendung von Service Bus benötigen Sie ein Azure-Konto. Wenn Sie über kein Abonnement verfügen, können Sie sich [hier](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A85619ABF) für ein kostenloses Konto registrieren.

### <a name="create-a-namespace"></a>Erstellen eines Namespace
Sobald Sie über ein Abonnement verfügen, können Sie [einen Dienstnamespace erstellen](service-bus-create-namespace-portal.md). Jeder Namespace ist ein Bereichscontainer für eine Gruppe von Service Bus-Entitäten. Versehen Sie den neuen Namespace mit einem Namen, der für alle Service Bus-Konten eindeutig ist. 

### <a name="install-the-nuget-package"></a>Installieren des NuGet-Pakets
Zur Verwendung eines Service Bus-Namespaces muss eine Anwendung auf die Service Bus-Assembly (genauer: auf „Microsoft.ServiceBus.dll“) verweisen. Diese Assembly ist Bestandteil des Microsoft Azure SDK und kann auf der [Azure SDK-Downloadseite](https://azure.microsoft.com/downloads/) heruntergeladen werden. Das [NuGet-Paket für Service Bus](https://www.nuget.org/packages/WindowsAzure.ServiceBus) stellt jedoch die einfachste Möglichkeit zum Abrufen der Service Bus-API und Konfigurieren der Anwendung mit allen Service Bus-Abhängigkeiten dar.

### <a name="create-the-queue"></a>Erstellen der Warteschlange
Verwaltungsvorgänge für Service Bus-Nachrichtenentitäten (Warteschlangen und Veröffentlichen/Abonnieren von Themen) werden über die [NamespaceManager](/dotnet/api/microsoft.servicebus.namespacemanager)-Klasse ausgeführt. Service Bus verwendet ein [SAS (Shared Access Signature)](service-bus-sas.md)-basiertes Sicherheitsmodell. Die [TokenProvider](/dotnet/api/microsoft.servicebus.tokenprovider)-Klasse stellt einen Sicherheitstokenanbieter mit integrierten Factorymethoden dar, die einige bekannte Tokenanbieter zurückgeben. Wir verwenden die [CreateSharedAccessSignatureTokenProvider](/dotnet/api/microsoft.servicebus.tokenprovider#Microsoft_ServiceBus_TokenProvider_CreateSharedAccessSignatureTokenProvider_System_String_)-Methode, um die SAS-Anmeldeinformationen zu speichern. Die [NamespaceManager](/dotnet/api/microsoft.servicebus.namespacemanager)-Instanz wird dann mit der Basisadresse des Service Bus-Namespaces und dem Tokenanbieter erstellt.

Die [NamespaceManager](/dotnet/api/microsoft.servicebus.namespacemanager)-Klasse enthält Methoden zum Erstellen, Aufzählen und Löschen von Nachrichtenentitäten. Mit dem folgenden Code wird gezeigt, wie die [NamespaceManager](/dotnet/api/microsoft.servicebus.namespacemanager)-Instanz erstellt und zum Erstellen der **DataCollectionQueue**-Warteschlange verwendet wird.

```csharp
Uri uri = ServiceBusEnvironment.CreateServiceUri("sb", 
                "test-blog", string.Empty);
string name = "RootManageSharedAccessKey";
string key = "abcdefghijklmopqrstuvwxyz";

TokenProvider tokenProvider = 
    TokenProvider.CreateSharedAccessSignatureTokenProvider(name, key);
NamespaceManager namespaceManager = 
    new NamespaceManager(uri, tokenProvider);
namespaceManager.CreateQueue("DataCollectionQueue");
```

Beachten Sie die Überladungen der [CreateQueue](/dotnet/api/microsoft.servicebus.namespacemanager#Microsoft_ServiceBus_NamespaceManager_CreateQueue_System_String_)-Methode, mit denen die Eigenschaften der Warteschlange optimiert werden können. So können Sie beispielsweise den Standardwert für die Gültigkeitsdauer (TTL) für an die Warteschlange gesendete Nachrichten festlegen.

### <a name="send-messages-to-the-queue"></a>Senden von Nachrichten an die Warteschlange
Für Vorgänge zur Laufzeit für Service Bus-Entitäten, z.B. Senden und Empfangen von Nachrichten, muss eine Anwendung zunächst ein [MessagingFactory](/dotnet/api/microsoft.servicebus.messaging.messagingfactory)-Objekt erstellen. Ähnlich wie die [NamespaceManager](/dotnet/api/microsoft.servicebus.namespacemanager)-Klasse wird die [MessagingFactory](/dotnet/api/microsoft.servicebus.messaging.messagingfactory)-Instanz aus der Basisadresse des Dienstnamespaces und dem Tokenanbieter erstellt.

```csharp
 BrokeredMessage bm = new BrokeredMessage(salesData);
 bm.Label = "SalesReport";
 bm.Properties["StoreName"] = "Redmond";
 bm.Properties["MachineID"] = "POS_1";
```

Die Nachrichten, die an die Service Bus-Warteschlangen gesendet werden (und von diesen empfangen werden), sind Instanzen der Klasse [BrokeredMessage](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage). Diese Klasse besteht aus einem Satz von Standardeigenschaften (z.B. [Label](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage#Microsoft_ServiceBus_Messaging_BrokeredMessage_Label) und [TimeToLive](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage#Microsoft_ServiceBus_Messaging_BrokeredMessage_TimeToLive)), einem Wörterbuch, in dem Anwendungseigenschaften enthalten sind, und einem Bestand beliebiger Anwendungsdaten. Eine Anwendung kann durch Übergeben eines beliebigen serialisierbaren Objekts den Bestand festlegen (im folgenden Beispiel wird ein **SalesData**-Objekt übergeben, das die Verkaufsdaten vom POS-Terminal darstellt), der mithilfe von [DataContractSerializer](https://msdn.microsoft.com/library/system.runtime.serialization.datacontractserializer.aspx) das Objekt serialisiert. Alternativ kann ein [Stream](https://msdn.microsoft.com/library/system.io.stream.aspx)-Objekt angegeben werden.

Die einfachste Möglichkeit zum Senden von Nachrichten an eine bestimmte Warteschlange, in unserem Fall die **DataCollectionQueue**-Warteschlange, ist die Verwendung von [CreateMessageSender](/dotnet/api/microsoft.servicebus.messaging.messagingfactory#Microsoft_ServiceBus_Messaging_MessagingFactory_CreateMessageSender_System_String_) zum Erstellen eines [MessageSender](/dotnet/api/microsoft.servicebus.messaging.messagesender)-Objekts direkt aus der [MessagingFactory](/dotnet/api/microsoft.servicebus.messaging.messagingfactory)-Instanz.

```csharp
MessageSender sender = factory.CreateMessageSender("DataCollectionQueue");
sender.Send(bm);
```

### <a name="receiving-messages-from-the-queue"></a>Empfangen von Nachrichten aus der Warteschlange
Zum Empfangen von Nachrichten aus der Warteschlange können Sie ein [MessageReceiver](/dotnet/api/microsoft.servicebus.messaging.messagereceiver)-Objekt verwenden, das Sie mithilfe von [CreateMessageReceiver](/dotnet/api/microsoft.servicebus.messaging.messagingfactory) direkt aus [MessagingFactory](/dotnet/api/microsoft.servicebus.messaging.messagingfactory#Microsoft_ServiceBus_Messaging_MessagingFactory_CreateMessageReceiver_System_String_) erstellen können. Nachrichtenempfänger können zwei unterschiedliche Modi verwenden: **ReceiveAndDelete** und **PeekLock**. [ReceiveMode](/dotnet/api/microsoft.servicebus.messaging.receivemode) wird beim Erstellen des Nachrichtenempfängers als Parameter für den [CreateMessageReceiver](/dotnet/api/microsoft.servicebus.messaging.messagingfactory?redirectedfrom=MSDN#Microsoft_ServiceBus_Messaging_MessagingFactory_CreateMessageReceiver_System_String_Microsoft_ServiceBus_Messaging_ReceiveMode_)-Aufruf festgelegt.

Bei Verwendung des **ReceiveAndDelete**-Modus ist der Nachrichtenempfang ein einstufiger Vorgang. Das heißt, wenn Service Bus die Anforderung erhält, wird die Nachricht als verarbeitet gekennzeichnet und an die Anwendung zurückgesendet. Der **ReceiveAndDelete**-Modus ist das einfachste Modell. Er wird am besten für Szenarios eingesetzt, bei denen die Anwendung es tolerieren kann, wenn eine Nachricht beim Auftreten eines Fehlers nicht verarbeitet wird. Um dieses Verfahren zu verstehen, stellen Sie sich ein Szenario vor, in dem der Consumer die Empfangsanforderung ausstellt und dann abstürzt, bevor diese verarbeitet wird. Da die Nachricht von Service Bus als verarbeitet gekennzeichnet wurde, wird die vor dem Absturz verarbeitete Nachricht nicht berücksichtigt, wenn die Anwendung neu gestartet und die Verarbeitung von Nachrichten wieder aufgenommen wird.

Im **PeekLock**-Modus ist der Nachrichtenempfang zweistufig. Dadurch können Anwendungen unterstützt werden, die fehlende Nachrichten nicht tolerieren können. Wenn Service Bus die Anforderung erhält, ermittelt der Dienst die nächste zu verarbeitende Nachricht, sperrt diese, um zu verhindern, dass andere Consumer sie erhalten, und sendet sie dann an die Anwendung zurück. Nachdem die Anwendung die Verarbeitung der Nachricht abgeschlossen hat (oder sie zwecks zukünftiger Verarbeitung zuverlässig gespeichert hat), führt sie die zweite Phase des Empfangsprozesses per Aufruf von [Complete](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage#Microsoft_ServiceBus_Messaging_BrokeredMessage_Complete) für die empfangene Nachricht durch. Wenn Service Bus den [Complete](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage#Microsoft_ServiceBus_Messaging_BrokeredMessage_Complete)-Aufruf erkennt, wird die Nachricht als verarbeitet markiert.

Zwei weitere Abläufe sind möglich. Wenn die Anwendung die Nachricht aus einem bestimmten Grund nicht verarbeiten kann, kann sie erstens [Abandon](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage#Microsoft_ServiceBus_Messaging_BrokeredMessage_Abandon) für die empfangene Nachricht aufrufen (anstelle von [Complete](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage#Microsoft_ServiceBus_Messaging_BrokeredMessage_Complete)). Dies führt dazu, dass Service Bus die Nachricht entsperrt und verfügbar macht, damit sie erneut empfangen werden kann, und zwar entweder von demselben Consumer oder von einem anderen den Vorgang abschließenden Consumer. Der Sperre ist zweitens ein Timeout zugeordnet. Wenn die Anwendung die Nachricht nicht vor Ablauf des Timeouts verarbeiten kann (z.B. wenn die Anwendung abstürzt), entsperrt Service Bus die Nachricht und macht sie verfügbar, sodass sie erneut empfangen werden kann (im Wesentlichen erfolgt standardmäßig der Vorgang [Verwerfen](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage#Microsoft_ServiceBus_Messaging_BrokeredMessage_Abandon)).

Beachten Sie, dass falls die Anwendung nach der Verarbeitung der Nachricht, aber vor der Ausgabe der [Complete](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage#Microsoft_ServiceBus_Messaging_BrokeredMessage_Complete)-Anforderung abstürzt, wird die Nachricht erneut an die Anwendung übermittelt, wenn diese neu gestartet wird. Dies wird häufig als *At Least Once*-Verarbeitung bezeichnet. Das bedeutet, dass jede Nachricht mindestens einmal verarbeitet wird, wobei dieselbe Nachricht in bestimmten Fällen möglicherweise erneut übermittelt wird. Wenn eine doppelte Verarbeitung in dem Szenario nicht zulässig ist, ist in der Anwendung zusätzliche Logik für die Erkennung doppelter Nachrichten erforderlich. Dies kann durch Verwendung der [MessageId](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage#Microsoft_ServiceBus_Messaging_BrokeredMessage_MessageId)-Eigenschaft der Nachricht erreicht werden. Der Wert dieser Eigenschaft bleibt über mehrere Übermittlungsversuche konstant. Dies wird als *Exactly Once*-Verarbeitung (genau einmal) bezeichnet.

Mit dem folgenden Code wird eine Nachricht unter Verwendung des **PeekLock**-Modus empfangen und verarbeitet. Dies ist der Standardmodus, wenn für [ReceiveMode](/dotnet/api/microsoft.servicebus.messaging.receivemode) nicht ausdrücklich ein Wert angegeben wird.

```csharp
MessageReceiver receiver = factory.CreateMessageReceiver("DataCollectionQueue");
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

### <a name="use-the-queue-client"></a>Verwenden des Warteschlangenclients
In den Beispielen weiter oben wurden [MessageSender](/dotnet/api/microsoft.servicebus.messaging.messagesender)- und [MessageReceiver](/dotnet/api/microsoft.servicebus.messaging.messagereceiver)-Objekte direkt aus [MessagingFactory](/dotnet/api/microsoft.servicebus.messaging.messagingfactory) erstellt, um Nachrichten aus der Warteschlange zu senden bzw. zu empfangen. Eine andere Möglichkeit besteht in der Verwendung eines [QueueClient](/dotnet/api/microsoft.servicebus.messaging.queueclient)-Objekts, das sowohl Sende- als auch Empfangsvorgänge sowie erweiterte Funktionen (z.B. Sitzungen) unterstützt.

```csharp
QueueClient queueClient = factory.CreateQueueClient("DataCollectionQueue");
queueClient.Send(bm);

BrokeredMessage message = queueClient.Receive();

try
{
    ProcessMessage(message);
    message.Complete();
}
catch (Exception e)
{
    message.Abandon();
} 
```

## <a name="next-steps"></a>Nächste Schritte
Nachdem Sie sich mit den Grundlagen in Bezug auf Warteschlangen vertraut gemacht haben, finden Sie unter [Erstellen von Anwendungen, die Service Bus-Themen und -Abonnements verwenden](service-bus-create-topics-subscriptions.md) weiterführende Informationen zur Verwendung der Funktionen zum Veröffentlichen/Abonnieren der Service Bus-Themen und -Abonnements.


