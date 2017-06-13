---
title: Verwenden von Azure Service Bus-Themen mit Java | Microsoft-Dokumentation
description: "Erfahren Sie mehr zur Verwendung von Service Bus-Themen und -Abonnements in Azure. Die Codebeispiele wurden für Java-Anwendungen geschrieben."
services: service-bus-messaging
documentationcenter: java
author: sethmanheim
manager: timlt
editor: 
ms.assetid: 63d6c8bd-8a22-4292-befc-545ffb52e8eb
ms.service: service-bus-messaging
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: Java
ms.topic: article
ms.date: 03/23/2017
ms.author: sethm
ms.translationtype: Human Translation
ms.sourcegitcommit: 0bec803e4b49f3ae53f2cc3be6b9cb2d256fe5ea
ms.openlocfilehash: 7132d1e42963d2e419d2bf1b7866ca5888f8719d
ms.contentlocale: de-de
ms.lasthandoff: 03/24/2017


---
# <a name="how-to-use-service-bus-topics-and-subscriptions"></a>Verwenden von Service Bus-Themen und -Abonnements
[!INCLUDE [service-bus-selector-topics](../../includes/service-bus-selector-topics.md)]

In diesem Leitfaden erfahren Sie, wie Sie Service Bus-Themen und -Abonnements verwenden. Die Beispiele wurden in Java geschrieben und verwenden das [Azure-SDK für Java][Azure SDK for Java]. Die behandelten Szenarios umfassen **das Erstellen von Themen und Abonnements**, **das Erstellen von Abonnementfiltern**, **das Senden von Nachrichten an ein Thema**, **das Empfangen von Nachrichten von einem Abonnement** und **das Löschen von Themen und Abonnements**.

## <a name="what-are-service-bus-topics-and-subscriptions"></a>Was sind Service Bus-Themen und -Abonnements?
Service Bus-Themen und -Abonnements unterstützen ein Modell der Messagingkommunikation über das *Veröffentlichen/Abonnieren* . Bei der Verwendung von Themen und Abonnements kommunizieren die Komponenten einer verteilten Anwendung nicht direkt miteinander, sondern tauschen Nachrichten über ein Thema aus, das als Zwischenstufe fungiert.

![TopicConcepts](./media/service-bus-java-how-to-use-topics-subscriptions/sb-topics-01.png)

Anders als bei Service Bus-Warteschlangen, bei denen jede Nachricht von einem einzelnen Consumer verarbeitet wird, bieten Themen und Abonnements eine 1:n-Kommunikationsform mit einem Veröffentlichungs- und Abonnementsmuster. Es ist möglich, mehrere Abonnements zu einem Thema anzumelden. Wenn eine Nachricht an ein Thema gesendet wird, steht sie in jedem Abonnement zur Verfügung, wo sie unabhängig von den anderen Abonnements verarbeitet wird.

Ein Themenabonnement ähnelt einer virtuellen Warteschlange, die Kopien der Nachrichten enthält, die an das Thema gesendet wurden. Sie können optional auch Filterregeln für einzelne Abonnements eines Themas anmelden. Auf diese Weise können Sie filtern/einschränken, welche Nachrichten an ein Thema von welchen Themenabonnements empfangen werden.

Mit Service Bus-Themen und -Abonnements können Sie sehr viele Nachrichten an sehr viele Benutzer und Anwendungen verarbeiten.

## <a name="create-a-service-namespace"></a>Erstellen eines Dienstnamespaces
Um zu Service Bus-Themen und -Abonnements in Azure verwenden zu können, müssen Sie zunächst einen Namespace erstellen, der einen Bereichscontainer zum Adressieren von Service Bus-Ressourcen in Ihrer Anwendung bereitstellt.

So erstellen Sie einen Namespace

[!INCLUDE [service-bus-create-namespace-portal](../../includes/service-bus-create-namespace-portal.md)]

## <a name="configure-your-application-to-use-service-bus"></a>Konfigurieren Ihrer Anwendung für die Verwendung von Service Bus
Stellen Sie vor dem Erstellen dieses Beispiels sicher, dass Sie das [Azure SDK für Java][Azure SDK for Java] installiert haben. Wenn Sie Eclipse verwenden, können Sie das [Azure Toolkit für Eclipse][Azure Toolkit for Eclipse] installieren, das das Azure SDK für Java enthält. Sie können dann Ihrem Projekt die **Microsoft Azure-Bibliotheken für Java** hinzufügen:

![](media/service-bus-java-how-to-use-topics-subscriptions/eclipselibs.png)

Fügen Sie die folgenden `import`-Anweisungen am Anfang der Java-Datei ein:

```java
import com.microsoft.windowsazure.services.servicebus.*;
import com.microsoft.windowsazure.services.servicebus.models.*;
import com.microsoft.windowsazure.core.*;
import javax.xml.datatype.*;
```

Fügen Sie die Azure-Bibliotheken für Java zu Ihrem Buildpfad hinzu und binden Sie die Bibliotheken in die Bereitstellungs-Assembly Ihres Projekts ein.

## <a name="create-a-topic"></a>Erstellen eines Themas
Sie können Verwaltungsvorgänge für Service Bus-Themen über die **ServiceBusContract**-Klasse durchführen. **ServiceBusContract**-Objekte werden mit einer entsprechenden Konfiguration erstellt, die die SAS-Token mit Berechtigungen für deren Verwaltung kapselt. Die **ServiceBusContract**-Klasse ist der einzige Kommunikationspunkt mit Azure.

Die **ServiceBusService**-Klasse enthält Methoden zum Erstellen, Aufzählen und Löschen von Themen. Das folgende Beispiel zeigt, wie Sie mit einem **ServiceBusService**-Objekt ein Thema mit dem Namen `TestTopic` in einem Namespace mit dem Namen `HowToSample` erstellen können:

```java
Configuration config =
    ServiceBusConfiguration.configureWithSASAuthentication(
      "HowToSample",
      "RootManageSharedAccessKey",
      "SAS_key_value",
      ".servicebus.windows.net"
      );

ServiceBusContract service = ServiceBusService.create(config);
TopicInfo topicInfo = new TopicInfo("TestTopic");
try  
{
    CreateTopicResult result = service.createTopic(topicInfo);
}
catch (ServiceException e) {
    System.out.print("ServiceException encountered: ");
    System.out.println(e.getMessage());
    System.exit(-1);
}
```

Mit den Methoden in **TopicInfo** können die Eigenschaften des Themas festgelegt werden (z.B. kann der Standardwert für die Gültigkeitsdauer festgelegt werden, der auf an das Thema gesendete Nachrichten angewendet wird). Das folgende Beispiel zeigt, wie Sie ein Thema mit der Bezeichnung `TestTopic` mit einer maximalen Größe von 5 GB erstellen:

```java
long maxSizeInMegabytes = 5120;  
TopicInfo topicInfo = new TopicInfo("TestTopic");  
topicInfo.setMaxSizeInMegabytes(maxSizeInMegabytes);
CreateTopicResult result = service.createTopic(topicInfo);
```

Sie können mit der **listTopics**-Methode von **ServiceBusContract**-Objekten überprüfen, ob ein Thema mit einem spezifischen Namen bereits innerhalb eines Dienstnamespaces vorhanden ist.

## <a name="create-subscriptions"></a>Erstellen von Abonnements
Abonnements von Themen werden ebenfalls mit der **ServiceBusService**-Klasse erstellt. Abonnements werden benannt und können einen optionalen Filter aufweisen, der die Nachrichten einschränkt, die an die virtuelle Warteschlange des Abonnements übergeben werden.

### <a name="create-a-subscription-with-the-default-matchall-filter"></a>Erstellen eines Abonnements mit dem Standardfilter (MatchAll)
**MatchAll** ist der Standardfilter, der verwendet wird, wenn beim Erstellen eines neuen Abonnements kein Filter angegeben wird. Wenn der Filter **MatchAll** verwendet wird, werden alle für das Thema veröffentlichten Nachrichten in die virtuelle Warteschlange des Abonnements gestellt. Mit dem folgenden Beispiel wird ein Abonnement namens „AllMessages“ erstellt, für das der Standardfilter **MatchAll** verwendet wird.

```java
SubscriptionInfo subInfo = new SubscriptionInfo("AllMessages");
CreateSubscriptionResult result =
    service.createSubscription("TestTopic", subInfo);
```

### <a name="create-subscriptions-with-filters"></a>Erstellen von Abonnements mit Filtern
Sie können auch Filter erstellen, durch die Sie angeben können, welche an ein Thema gesendeten Nachrichten in einem bestimmten Themenabonnement angezeigt werden sollen.

Der flexibelste von Abonnements unterstützte Filtertyp ist [SqlFilter][SqlFilter], der eine Teilmenge von SQL92 implementiert. SQL-Filter werden auf die Eigenschaften der Nachrichten angewendet, die für das Thema veröffentlicht werden. Weitere Informationen zu den Ausdrücken, die mit einem SQL-Filter verwendet werden können, finden Sie in der Syntax von [SqlFilter.SqlExpression][SqlFilter.SqlExpression].

Mit dem folgenden Beispiel wird ein Abonnement namens `HighMessages` mit einem [SqlFilter][SqlFilter]-Objekt erstellt, das nur Nachrichten auswählt, deren benutzerdefinierte **MessageNumber**-Eigenschaft größer ist als 3:

```java
// Create a "HighMessages" filtered subscription  
SubscriptionInfo subInfo = new SubscriptionInfo("HighMessages");
CreateSubscriptionResult result = service.createSubscription("TestTopic", subInfo);
RuleInfo ruleInfo = new RuleInfo("myRuleGT3");
ruleInfo = ruleInfo.withSqlExpressionFilter("MessageNumber > 3");
CreateRuleResult ruleResult = service.createRule("TestTopic", "HighMessages", ruleInfo);
// Delete the default rule, otherwise the new rule won't be invoked.
service.deleteRule("TestTopic", "HighMessages", "$Default");
```

Ebenso erstellt das folgende Beispiel ein Abonnement namens `LowMessages` mit einem [SqlFilter][SqlFilter]-Objekt, das nur Nachrichten auswählt, deren benutzerdefinierte **MessageNumber**-Eigenschaft kleiner oder gleich 3 ist:

```java
// Create a "LowMessages" filtered subscription
SubscriptionInfo subInfo = new SubscriptionInfo("LowMessages");
CreateSubscriptionResult result = service.createSubscription("TestTopic", subInfo);
RuleInfo ruleInfo = new RuleInfo("myRuleLE3");
ruleInfo = ruleInfo.withSqlExpressionFilter("MessageNumber <= 3");
CreateRuleResult ruleResult = service.createRule("TestTopic", "LowMessages", ruleInfo);
// Delete the default rule, otherwise the new rule won't be invoked.
service.deleteRule("TestTopic", "LowMessages", "$Default");
```

Wenn jetzt eine Nachricht an `TestTopic` gesendet wird, wird diese stets an Empfänger im `AllMessages`-Abonnement zugestellt. Sie wird selektiv an die Empfänger in den Abonnements `HighMessages` und `LowMessages` zugestellt (je nach Inhalt der Nachricht).

## <a name="send-messages-to-a-topic"></a>Senden von Nachrichten an ein Thema
Um eine Nachricht an ein Service Bus-Thema zu senden, ruft Ihre Anwendung ein **ServiceBusContract**-Objekt ab. Der folgende Code zeigt, wie Sie eine Nachricht an das Thema `TestTopic` senden können, das Sie zuvor im Namespace `HowToSample` erstellt haben:

```java
BrokeredMessage message = new BrokeredMessage("MyMessage");
service.sendTopicMessage("TestTopic", message);
```

An Service Bus-Themen gesendete Nachrichten sind Instanzen der [BrokeredMessage][BrokeredMessage] Klasse. [BrokeredMessage][BrokeredMessage] *Objekte enthalten eine Reihe von Standardmethoden (wie etwa* *setLabel* *und* *TimeToLive**), ein Wörterbuch für die Aufnahme benutzerdefinierter, anwendungsspezifischer Eigenschaften und einen Hauptteil mit beliebigen Anwendungsdaten. Eine Anwendung kann den Haupttext der Nachricht festlegen, indem ein beliebiges serialisierbares Objekt in den Konstruktor der [BrokeredMessage][BrokeredMessage], übergeben wird. Anschließend wird der passende **DataContractSerializer** zum Serialisieren des Objekts verwendet. Alternativ kann ein **java.io.InputStream** bereitgestellt werden.

Das folgende Beispiel veranschaulicht, wie fünf Testnachrichten an den **MessageSender** `TestTopic` gesendet werden, der im vorherigen Codeausschnitt abgerufen wurde.
Beachten Sie, wie der Eigenschaftswert **MessageNumber** jeder Nachricht gemäß der Iteration der Schleife variiert (dadurch wird bestimmt, welche Abonnements die Nachricht erhalten):

```java
for (int i=0; i<5; i++)  {
// Create message, passing a string message for the body
BrokeredMessage message = new BrokeredMessage("Test message " + i);
// Set some additional custom app-specific property
message.setProperty("MessageNumber", i);
// Send message to the topic
service.sendTopicMessage("TestTopic", message);
}
```

Service Bus-Themen unterstützen eine maximale Nachrichtengröße von 256 KB im [Standard-Tarif](service-bus-premium-messaging.md) und 1 MB im [Premium-Tarif](service-bus-premium-messaging.md). Der Header, der die standardmäßigen und benutzerdefinierten Anwendungseigenschaften enthält, kann eine maximale Größe von 64 KB haben. Es gibt keine Beschränkung für die Anzahl der Nachrichten, die ein Thema enthält. Es gibt jedoch eine Obergrenze für die Gesamtgröße der Nachrichten eines Themas. Die Themengröße wird bei der Erstellung definiert. Die Obergrenze beträgt 5 GB.

## <a name="how-to-receive-messages-from-a-subscription"></a>Empfangen von Nachrichten aus einem Abonnement
Verwenden Sie ein **ServiceBusContract**-Objekt, um Nachrichten aus einem Abonnement zu empfangen. Empfangene Nachrichten können in zwei unterschiedlichen Modi funktionieren: **ReceiveAndDelete** und **PeekLock**.

Bei Verwendung des **ReceiveAndDelete**-Modus ist der Nachrichtenempfang ein Single-Shot-Vorgang. Das heißt, wenn Service Bus eine Leseanforderung für eine Nachricht erhält, wird die Nachricht als verarbeitet gekennzeichnet und an die Anwendung zurückgesendet. Der **ReceiveAndDelete**-Modus ist das einfachste Modell. Er wird am besten für Szenarios eingesetzt, bei denen es eine Anwendung tolerieren kann, wenn eine Nachricht bei Auftreten eines Fehlers nicht verarbeitet wird. Um dieses Verfahren zu verstehen, stellen Sie sich ein Szenario vor, in dem der Consumer die Empfangsanforderung ausstellt und dann abstürzt, bevor diese verarbeitet wird. Da Service Bus die Nachricht als verwendet markiert hat, wird er jene Nachricht auslassen, die vor dem Absturz verwendet wurde, wenn die Anwendung neu startet und erneut mit der Verwendung von Nachrichten beginnt.

Im **PeekLock**-Modus ist der Empfangsvorgang zweistufig. Dadurch können Anwendungen unterstützt werden, die das Auslassen bzw. Fehlen von Nachrichten nicht zulassen können. Wenn Service Bus eine Anfrage erhält, ermittelt der Dienst die nächste zu verarbeitende Nachricht, sperrt diese, um zu verhindern, dass andere Consumer sie erhalten, und sendet sie dann zurück an die Anwendung. Nachdem die Anwendung die Verarbeitung der Nachricht abgeschlossen hat (oder sie zwecks zukünftiger Verarbeitung zuverlässig gespeichert hat), führt sie die zweite Phase des Empfangsprozesses durch Aufrufen von **Delete** für die empfangene Nachricht durch. Wenn Service Bus den **Delete**-Aufruf erkennt, wird die Nachricht als verwendet markiert und aus dem Thema entfernt.

Das folgende Beispiel zeigt, wie Nachrichten im nicht standardmäßig verwendeten **PeekLock**-Modus empfangen und verarbeitet werden können. Das folgende Beispiel verarbeitet Nachrichten aus dem "HighMessages"-Abonnement in einer Schleife und endet, wenn keine Nachrichten mehr vorhanden sind (alternativ könnte der Code auf neue Nachrichten warten).

```java
try
{
    ReceiveMessageOptions opts = ReceiveMessageOptions.DEFAULT;
    opts.setReceiveMode(ReceiveMode.PEEK_LOCK);

    while(true)  {
        ReceiveSubscriptionMessageResult  resultSubMsg =
            service.receiveSubscriptionMessage("TestTopic", "HighMessages", opts);
        BrokeredMessage message = resultSubMsg.getValue();
        if (message != null && message.getMessageId() != null)
        {
            System.out.println("MessageID: " + message.getMessageId());
            // Display the topic message.
            System.out.print("From topic: ");
            byte[] b = new byte[200];
            String s = null;
            int numRead = message.getBody().read(b);
            while (-1 != numRead)
            {
                s = new String(b);
                s = s.trim();
                System.out.print(s);
                numRead = message.getBody().read(b);
            }
            System.out.println();
            System.out.println("Custom Property: " +
                message.getProperty("MessageNumber"));
            // Delete message.
            System.out.println("Deleting this message.");
            service.deleteMessage(message);
        }  
        else  
        {
            System.out.println("Finishing up - no more messages.");
            break;
            // Added to handle no more messages.
            // Could instead wait for more messages to be added.
        }
    }
}
catch (ServiceException e) {
    System.out.print("ServiceException encountered: ");
    System.out.println(e.getMessage());
    System.exit(-1);
}
catch (Exception e) {
    System.out.print("Generic exception encountered: ");
    System.out.println(e.getMessage());
    System.exit(-1);
}
```

## <a name="how-to-handle-application-crashes-and-unreadable-messages"></a>Behandeln von Anwendungsabstürzen und nicht lesbaren Nachrichten
Service Bus stellt Funktionen zur Verfügung, die Sie bei der ordnungsgemäßen Behandlung von Fehlern in der Anwendung oder bei Problemen beim Verarbeiten einer Nachricht unterstützen. Wenn eine empfangene Anwendung eine Nachricht aus einem beliebigen Grund nicht verarbeiten kann, kann sie die **unlockMessage**-Methode für die empfangene Nachricht aufrufen (anstelle der **deleteMessage**-Methode). Dies führt dazu, dass Service Bus die Nachricht innerhalb des Themas entsperrt und verfügbar macht, damit sie erneut empfangen werden kann, und zwar entweder durch dieselbe verarbeitende Anwendung oder durch eine andere verarbeitende Anwendung.

Darüber hinaus ist eine im Thema gesperrte Nachricht mit einem Timeout verknüpft. Wenn die Anwendung die Nachricht nicht vor Ablauf dieses Zeitlimits verarbeitet (z.B. wenn die Anwendung abstürzt), entsperrt Service Bus die Nachricht automatisch und macht sie für den erneuten Empfang verfügbar.

Falls die Anwendung nach der Verarbeitung der Nachricht, aber vor Ausgabe der **deleteMessage**-Anforderung abstürzt, wird die Nachricht wieder an die Anwendung zugestellt, wenn diese neu gestartet wird. Dies wird häufig als **At Least Once Processing** bezeichnet und bedeutet, dass jede Nachricht mindestens einmal verarbeitet wird, wobei dieselbe Nachricht in bestimmten Situationen möglicherweise erneut zugestellt wird. Wenn eine doppelte Verarbeitung im betreffenden Szenario nicht geeignet ist, sollten Anwendungsentwickler ihrer Anwendung zusätzliche Logik für den Umgang mit der Übermittlung doppelter Nachrichten hinzufügen. Dies wird häufig durch die Verwendung der **getMessageId**-Methode der Nachricht erzielt, die über mehrere Zustellungsversuche hinweg konstant bleibt.

## <a name="delete-topics-and-subscriptions"></a>Löschen von Themen und Abonnements
Der einfachste Weg zum Löschen von Themen und Abonnements sind **ServiceBusContract**-Objekte. Durch das Löschen eines Themas werden auch alle Abonnements gelöscht, die mit dem Thema registriert sind. Abonnements können auch unabhängig gelöscht werden.

```java
// Delete subscriptions
service.deleteSubscription("TestTopic", "AllMessages");
service.deleteSubscription("TestTopic", "HighMessages");
service.deleteSubscription("TestTopic", "LowMessages");

// Delete a topic
service.deleteTopic("TestTopic");
```

## <a name="next-steps"></a>Nächste Schritte
Nachdem Sie nun mit den Grundlagen von Service Bus-Warteschlangen vertraut sind, finden Sie weitere Informationen unter [Service Bus-Warteschlangen, -Themen und -Abonnements][Service Bus queues, topics, and subscriptions].

[Azure SDK for Java]: http://azure.microsoft.com/develop/java/
[Azure Toolkit for Eclipse]: ../azure-toolkit-for-eclipse.md
[Service Bus queues, topics, and subscriptions]: service-bus-queues-topics-subscriptions.md
[SqlFilter]: /dotnet/api/microsoft.servicebus.messaging.sqlfilter 
[SqlFilter.SqlExpression]: /dotnet/api/microsoft.servicebus.messaging.sqlfilter#Microsoft_ServiceBus_Messaging_SqlFilter_SqlExpression
[BrokeredMessage]: /dotnet/api/microsoft.servicebus.messaging.brokeredmessage

[0]: ./media/service-bus-java-how-to-use-topics-subscriptions/sb-queues-13.png
[2]: ./media/service-bus-java-how-to-use-topics-subscriptions/sb-queues-04.png
[3]: ./media/service-bus-java-how-to-use-topics-subscriptions/sb-queues-09.png

