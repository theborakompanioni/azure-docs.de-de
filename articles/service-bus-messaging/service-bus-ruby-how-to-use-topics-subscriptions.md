---
title: Verwenden von Service Bus-Themen (Ruby) | Microsoft Docs
description: "Erfahren Sie mehr zur Verwendung von Service Bus-Themen und -Abonnements in Azure. Die Codebeispiele wurden für Ruby-Anwendungen geschrieben."
services: service-bus-messaging
documentationcenter: ruby
author: sethmanheim
manager: timlt
editor: 
ms.assetid: 3ef2295e-7c5f-4c54-a13b-a69c8045d4b6
ms.service: service-bus-messaging
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: ruby
ms.topic: article
ms.date: 08/10/2017
ms.author: sethm
ms.translationtype: HT
ms.sourcegitcommit: 398efef3efd6b47c76967563251613381ee547e9
ms.openlocfilehash: 4a4c9949843b16ae6be2f516de4fd1e3f7415959
ms.contentlocale: de-de
ms.lasthandoff: 08/11/2017

---
# <a name="how-to-use-service-bus-topics-and-subscriptions-with-ruby"></a>Verwenden von Service Bus-Themen und -Abonnements mit Ruby
 
[!INCLUDE [service-bus-selector-topics](../../includes/service-bus-selector-topics.md)]

In diesem Artikel erfahren Sie, wie Sie Service Bus-Themen und -Abonnements aus Ruby-Anwendungen verwenden. Die behandelten Szenarios umfassen **das Erstellen von Themen und Abonnements, das Erstellen von Abonnementfiltern, das Senden von Nachrichten** an ein Thema, **das Empfangen von Nachrichten von einem Abonnement** und **das Löschen von Themen und Abonnements**. Weitere Informationen zu Themen und Abonnements finden Sie im Abschnitt [Nächste Schritte](#next-steps).

[!INCLUDE [howto-service-bus-topics](../../includes/howto-service-bus-topics.md)]

[!INCLUDE [service-bus-create-namespace-portal](../../includes/service-bus-create-namespace-portal.md)]

[!INCLUDE [service-bus-ruby-setup](../../includes/service-bus-ruby-setup.md)]

## <a name="create-a-topic"></a>Erstellen eines Themas
Das **Azure::ServiceBusService**-Objekt ermöglicht es Ihnen, mit Themen zu arbeiten. Der folgende Code erstellt ein **Azure::ServiceBusService**-Objekt. Verwenden Sie die Methode `create_topic()`, um ein Thema zu erstellen. Im folgenden Beispiel wird ein Thema erstellt oder ggf. ein Fehler ausgegeben.

```ruby
azure_service_bus_service = Azure::ServiceBus::ServiceBusService.new(sb_host, { signer: signer})
begin
  topic = azure_service_bus_service.create_queue("test-topic")
rescue
  puts $!
end
```

Sie können außerdem ein Objekt vom Typ **Azure::ServiceBus::Topic** mit weiteren Optionen übergeben, mit denen Sie die Standardthemeneinstellungen (etwa die Nachrichtenlebensdauer oder die maximale Warteschlangengröße) überschreiben können. Das folgende Beispiel zeigt, wie Sie die maximale Warteschlangengröße auf 5 GB und die Gültigkeitsdauer auf eine Minute festlegen:

```ruby
topic = Azure::ServiceBus::Topic.new("test-topic")
topic.max_size_in_megabytes = 5120
topic.default_message_time_to_live = "PT1M"

topic = azure_service_bus_service.create_topic(topic)
```

## <a name="create-subscriptions"></a>Erstellen von Abonnements
Themenabonnements werden ebenfalls mit dem **Azure::ServiceBusService**-Objekt erstellt. Abonnements werden benannt und können einen optionalen Filter aufweisen, der die Nachrichten einschränkt, die an die virtuelle Warteschlange des Abonnements übermittelt werden.

Abonnements sind persistent und bleiben erhalten, bis sie selbst oder die mit ihnen verbundenen Themen gelöscht werden. Wenn Ihre Anwendung Logik beinhaltet, sollte sie bei der Erstellung eines Abonnements zuerst mithilfe der getSubscription-Methode überprüfen, ob das Abonnement bereits vorhanden ist.

### <a name="create-a-subscription-with-the-default-matchall-filter"></a>Erstellen eines Abonnements mit dem Standardfilter (MatchAll)
**MatchAll** ist der Standardfilter, der verwendet wird, wenn beim Erstellen eines neuen Abonnements kein Filter angegeben wird. Wenn der Filter **MatchAll** verwendet wird, werden alle für das Thema veröffentlichten Nachrichten in die virtuelle Warteschlange des Abonnements gestellt. Mit dem folgenden Beispiel wird ein Abonnement namens „all-messages“ erstellt, für das der Standardfilter **MatchAll** verwendet wird.

```ruby
subscription = azure_service_bus_service.create_subscription("test-topic", "all-messages")
```

### <a name="create-subscriptions-with-filters"></a>Erstellen von Abonnements mit Filtern
Sie können auch Filter definieren, durch die Sie angeben können, welche an ein Thema gesendeten Nachrichten in einem bestimmten Abonnement angezeigt werden sollen.

Der von Abonnements unterstützte flexibelste Filtertyp ist **Azure::ServiceBus::SqlFilter**, der eine Teilmenge von SQL92 implementiert. SQL-Filter werden auf die Eigenschaften der Nachrichten angewendet, die für das Thema veröffentlicht werden. Weitere Informationen zu den Ausdrücken, die mit einem SQL-Filter verwendet werden können, finden Sie in der Syntax von [SqlFilter](service-bus-messaging-sql-filter.md).

Sie können einem Abonnement mithilfe der Methode `create_rule()` des Objekts **Azure::ServiceBusService** Filter hinzufügen. Durch diese Methode können Sie neue Filter einem vorhandenen Abonnement hinzufügen.

Da der Standardfilter automatisch auf alle neuen Abonnements angewendet wird, müssen Sie zuerst den Standardfilter entfernen. Ansonsten überschreibt **MatchAll** alle Filter, die Sie angeben. Sie können die Standardregel mithilfe der Methode `delete_rule()` des Objekts **Azure::ServiceBusService** entfernen.

Mit dem folgenden Beispiel wird ein Abonnement namens „high-messages“ mit einem Filter vom Typ **Azure::ServiceBus::SqlFilter** erstellt, der nur Nachrichten auswählt, deren benutzerdefinierte Eigenschaft `message_number` größer „3“ ist:

```ruby
subscription = azure_service_bus_service.create_subscription("test-topic", "high-messages")
azure_service_bus_service.delete_rule("test-topic", "high-messages", "$Default")

rule = Azure::ServiceBus::Rule.new("high-messages-rule")
rule.topic = "test-topic"
rule.subscription = "high-messages"
rule.filter = Azure::ServiceBus::SqlFilter.new({
  :sql_expression => "message_number > 3" })
rule = azure_service_bus_service.create_rule(rule)
```

Analog dazu erstellt das folgende Beispiel ein Abonnement namens `low-messages` mit einem Filter vom Typ **Azure::ServiceBus::SqlFilter**, der nur Nachrichten auswählt, deren benutzerdefinierte Eigenschaft `message_number` kleiner oder gleich „3“ ist:

```ruby
subscription = azure_service_bus_service.create_subscription("test-topic", "low-messages")
azure_service_bus_service.delete_rule("test-topic", "low-messages", "$Default")

rule = Azure::ServiceBus::Rule.new("low-messages-rule")
rule.topic = "test-topic"
rule.subscription = "low-messages"
rule.filter = Azure::ServiceBus::SqlFilter.new({
  :sql_expression => "message_number <= 3" })
rule = azure_service_bus_service.create_rule(rule)
```

Wenn nun eine Nachricht an `test-topic` gesendet wird, wird diese stets an das `all-messages`-Themenabonnement und selektiv an die Empfänger der Abonnements `high-messages` und `low-messages` zugestellt (je nach Inhalt der Nachricht).

## <a name="send-messages-to-a-topic"></a>Senden von Nachrichten an ein Thema
Um eine Nachricht an ein Service Bus-Thema zu senden, muss die Anwendung die Methode `send_topic_message()` des Objekts **Azure::ServiceBusService** verwenden. Nachrichten, die an Service Bus-Themen gesendet werden, sind **Azure::ServiceBus::BrokeredMessage**-Objekte. Objekte vom Typ **Azure::ServiceBus::BrokeredMessage** besitzen eine Reihe von Standardeigenschaften (wie etwa `label` und `time_to_live`), ein Wörterbuch zur Speicherung benutzerdefinierter, anwendungsspezifischer Eigenschaften und einen aus Zeichenfolgendaten bestehenden Nachrichtentext. Eine Anwendung kann den Nachrichtentext festlegen, indem sie einen Zeichenfolgenwert an die Methode `send_topic_message()` übergibt. Erforderliche Standardeigenschaften werden mit Standardwerten gefüllt.

Das folgende Beispiel zeigt, wie Sie fünf Testnachrichten an `test-topic` senden. Beachten Sie, dass der benutzerdefinierte Eigenschaftswert `message_number` jeder Nachricht gemäß der Iteration der Schleife variiert. (Dadurch wird bestimmt, welches Abonnement die Nachricht erhält.)

```ruby
5.times do |i|
  message = Azure::ServiceBus::BrokeredMessage.new("test message " + i,
    { :message_number => i })
  azure_service_bus_service.send_topic_message("test-topic", message)
end
```

Service Bus-Themen unterstützen eine maximale Nachrichtengröße von 256 KB im [Standard-Tarif](service-bus-premium-messaging.md) und 1 MB im [Premium-Tarif](service-bus-premium-messaging.md). Der Header, der die standardmäßigen und benutzerdefinierten Anwendungseigenschaften enthält, kann eine maximale Größe von 64 KB haben. Es gibt keine Beschränkung für die Anzahl der Nachrichten, die ein Thema enthält. Es gibt jedoch eine Obergrenze für die Gesamtgröße der Nachrichten eines Themas. Die Themengröße wird bei der Erstellung definiert. Die Obergrenze beträgt 5 GB.

## <a name="receive-messages-from-a-subscription"></a>Empfangen von Nachrichten aus einem Abonnement
Nachrichten werden von einem Abonnement über die Methode `receive_subscription_message()` des Objekts **Azure::ServiceBusService** empfangen. Standardmäßig werden Nachrichten gelesen(peak) und gesperrt, ohne aus dem Abonnement gelöscht zu werden. Sie können die Nachricht aus dem Abonnement lesen und löschen, indem Sie die Option `peek_lock` auf **false** festlegen.

Im Standardverhalten sind Empfang und Löschung von Nachrichten zweistufig. Dadurch können Anwendungen unterstützt werden, die das Auslassen bzw. Fehlen von Nachrichten nicht zulassen können. Wenn Service Bus eine Anfrage erhält, ermittelt der Dienst die nächste zu verarbeitende Nachricht, sperrt diese, um zu verhindern, dass andere Consumer sie erhalten, und sendet sie dann zurück an die Anwendung. Nachdem die Anwendung die Verarbeitung der Nachricht abgeschlossen hat (oder sie zwecks zukünftiger Verarbeitung zuverlässig gespeichert hat), führt sie die zweite Phase des Empfangsprozesses durch: In dieser Phase wird die Methode `delete_subscription_message()` aufgerufen und die zu löschende Nachricht als Parameter angegeben. Die Methode `delete_subscription_message()` markiert die Nachricht als verarbeitet und entfernt sie aus dem Abonnement.

Wenn der Parameter `:peek_lock` auf **false** festgelegt ist, wird das Lesen und Löschen der Nachricht zum einfachsten Modell. Es eignet sich am besten für Szenarien, in denen es kein Problem ist, wenn Nachrichten bei Auftreten eines Fehlers nicht verarbeitet werden. Um dieses Verfahren zu verstehen, stellen Sie sich ein Szenario vor, in dem der Consumer die Empfangsanforderung ausstellt und dann abstürzt, bevor diese verarbeitet wird. Da Service Bus die Nachricht als verwendet markiert hat, wird er jene Nachricht auslassen, die vor dem Absturz verwendet wurde, wenn die Anwendung neu startet und erneut mit der Verwendung von Nachrichten beginnt.

Das folgende Beispiel zeigt, wie Nachrichten mit `receive_subscription_message()` empfangen und verarbeitet werden können. In diesem Beispiel wird zunächst eine Nachricht aus dem Abonnement `low-messages` empfangen und gelöscht, wobei `:peek_lock` auf **false** festgelegt ist. Anschließend wird eine Nachricht von `high-messages` empfangen und mit `delete_subscription_message()` gelöscht:

```ruby
message = azure_service_bus_service.receive_subscription_message(
  "test-topic", "low-messages", { :peek_lock => false })
message = azure_service_bus_service.receive_subscription_message(
  "test-topic", "high-messages")
azure_service_bus_service.delete_subscription_message(message)
```

## <a name="how-to-handle-application-crashes-and-unreadable-messages"></a>Behandeln von Anwendungsabstürzen und nicht lesbaren Nachrichten
Service Bus stellt Funktionen zur Verfügung, die Sie bei der ordnungsgemäßen Behandlung von Fehlern in der Anwendung oder bei Problemen beim Verarbeiten einer Nachricht unterstützen. Wenn eine Empfängeranwendung die Nachricht aus einem bestimmten Grund nicht verarbeiten kann, kann sie die Methode `unlock_subscription_message()` für das Objekt **Azure::ServiceBusService** aufrufen. Dies führt dazu, dass Service Bus die Nachricht innerhalb des Abonnements entsperrt und verfügbar macht, damit sie erneut empfangen werden kann, und zwar entweder durch dieselbe verarbeitende Anwendung oder durch eine andere verarbeitende Anwendung.

Zudem wird der im Abonnement gesperrten Anwendung ein Timeout zugeordnet. Wenn die Anwendung die Nachricht vor Ablauf des Timeouts nicht verarbeiten kann (zum Beispiel, wenn die Anwendung abstürzt), entsperrt Service Bus die Nachricht automatisch und macht sie verfügbar, um erneut empfangen zu werden.

Falls die Anwendung nach der Verarbeitung der Nachricht, aber vor dem Aufruf der Methode `delete_subscription_message()` abstürzt, wird die Nachricht wieder an die Anwendung zugestellt, wenn diese neu gestartet wird. Dies wird häufig als *At Least Once Processing* (Mindestens einmal verarbeiten) bezeichnet und bedeutet, dass jede Nachricht mindestens einmal verarbeitet wird, wobei dieselbe Nachricht in bestimmten Situationen möglicherweise erneut zugestellt wird. Wenn eine doppelte Verarbeitung im betreffenden Szenario nicht geeignet ist, sollten Anwendungsentwickler ihrer Anwendung zusätzliche Logik für den Umgang mit der Übermittlung doppelter Nachrichten hinzufügen. Diese Logik wird häufig durch die Verwendung der Eigenschaft `message_id` der Nachricht erzielt, die über mehrere Zustellungsversuche hinweg konstant bleibt.

## <a name="delete-topics-and-subscriptions"></a>Löschen von Themen und Abonnements
Themen und Abonnements sind persistent und müssen über das [Azure-Portal][Azure portal] oder programmgesteuert explizit gelöscht werden. Das folgende Beispiel zeigt, wie Sie das Thema namens `test-topic` löschen.

```ruby
azure_service_bus_service.delete_topic("test-topic")
```

Durch das Löschen eines Themas werden auch alle Abonnements gelöscht, die mit dem Thema registriert sind. Abonnements können auch unabhängig gelöscht werden. Der folgende Code zeigt, wie das Abonnement `high-messages` aus dem Thema `test-topic` gelöscht wird:

```ruby
azure_service_bus_service.delete_subscription("test-topic", "high-messages")
```

## <a name="next-steps"></a>Nächste Schritte
Nachdem Sie nun mit den Grundlagen der Servicebus-Themen vertraut sind, finden Sie unter den folgenden Links weitere Informationen.

* Siehe [Service Bus-Warteschlangen, -Themen und -Abonnements](service-bus-queues-topics-subscriptions.md).
* API-Referenz für [SqlFilter](/dotnet/api/microsoft.servicebus.messaging.sqlfilter#microsoft_servicebus_messaging_sqlfilter).
* Besuchen Sie das [Azure SDK for Ruby](https://github.com/Azure/azure-sdk-for-ruby)-Repository auf GitHub.

[Azure portal]: https://portal.azure.com

