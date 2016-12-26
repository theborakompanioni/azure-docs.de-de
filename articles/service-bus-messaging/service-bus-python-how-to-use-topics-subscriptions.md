---
title: Verwenden von Service Bus-Themen mit Python | Microsoft Docs
description: "Erfahren Sie mehr über die Verwendung von Azure Service Bus-Themen und -Abonnements über Python."
services: service-bus-messaging
documentationcenter: python
author: sethmanheim
manager: timlt
editor: 
ms.assetid: c4f1d76c-7567-4b33-9193-3788f82934e4
ms.service: service-bus-messaging
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: python
ms.topic: article
ms.date: 10/04/2016
ms.author: sethm
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: 86fa1e1cc5db31bdbec216e1c1f20c2b07cf68d9


---
# <a name="how-to-use-service-bus-topics-and-subscriptions"></a>Verwenden von Service Bus-Themen und -Abonnements
[!INCLUDE [service-bus-selector-topics](../../includes/service-bus-selector-topics.md)]

In diesem Artikel erfahren Sie, wie Sie Service Bus-Themen und -Abonnements verwenden. Die Beispiele sind in Python geschrieben und verwenden das [Python Azure-Paket][Python Azure-Paket]. Die behandelten Szenarios umfassen **das Erstellen von Themen und Abonnements**, **das Erstellen von Abonnementfiltern**, **das Senden von Nachrichten an ein Thema**, **das Empfangen von Nachrichten von einem Abonnement** und **das Löschen von Themen und Abonnements**. Weitere Informationen zu Themen und Abonnements finden Sie im Abschnitt [Nächste Schritte](#next-steps).

[!INCLUDE [howto-service-bus-topics](../../includes/howto-service-bus-topics.md)]

**Hinweis:** Informationen zur Installation von Python bzw. des [Python Azure-Pakets][Python Azure-Paket] finden Sie in den [Installationsanweisungen für Python](../python-how-to-install.md).

## <a name="create-a-topic"></a>Erstellen eines Themas
Das **ServiceBusService**-Objekt ermöglicht es Ihnen, mit Themen zu arbeiten. Fügen Sie am Anfang jeder Python-Datei, in der Sie programmgesteuert auf Service Bus zugreifen möchten, den folgenden Code hinzu:

```
from azure.servicebus import ServiceBusService, Message, Topic, Rule, DEFAULT_RULE_NAME
```

Der folgende Code erstellt ein **ServiceBusService**-Objekt. Ersetzen Sie `mynamespace`, `sharedaccesskeyname` und `sharedaccesskey` durch Ihren tatsächlichen Namespace, Shared Access Signature (SAS)-Schlüsselnamen und Schlüsselwert.

```
bus_service = ServiceBusService(
    service_namespace='mynamespace',
    shared_access_key_name='sharedaccesskeyname',
    shared_access_key_value='sharedaccesskey')
```

Sie können die Werte für den Namen und Wert des SAS-Schlüssels im [Azure-Portal][Azure-Portal] abrufen.

```
bus_service.create_topic('mytopic')
```

**create\_topic** unterstützt zudem weitere Optionen, mit denen Sie die Standardthemeneinstellungen überschreiben können, wie zum Beispiel die Nachrichtenlebensdauer oder maximale Themengröße. Das folgende Beispiel legt die maximale Themengröße auf 5 GB bei einer Gültigkeitsdauer (Time-to-live, TTL) von 1 Minute fest:

```
topic_options = Topic()
topic_options.max_size_in_megabytes = '5120'
topic_options.default_message_time_to_live = 'PT1M'

bus_service.create_topic('mytopic', topic_options)
```

## <a name="create-subscriptions"></a>Erstellen von Abonnements
Abonnements von Themen werden ebenfalls mit dem **ServiceBusService**-Objekt erstellt. Abonnements werden benannt und können einen optionalen Filter aufweisen, der die Nachrichten einschränkt, die an die virtuelle Warteschlange des Abonnements übermittelt werden.

> [!NOTE]
> Abonnements sind dauerhaft und existieren solange, bis sie oder das Thema, für das sie abonniert sind, gelöscht werden.
> 
> 

### <a name="create-a-subscription-with-the-default-matchall-filter"></a>Erstellen eines Abonnements mit dem Standardfilter (MatchAll)
**MatchAll** ist der Standardfilter, der verwendet wird, wenn beim Erstellen eines neuen Abonnements kein Filter angegeben wird. Wenn der Filter **MatchAll** verwendet wird, werden alle für das Thema veröffentlichten Nachrichten in die virtuelle Warteschlange des Abonnements gestellt. Mit dem folgenden Beispiel wird ein Abonnement namens „AllMessages“ erstellt, für das der Standardfilter **MatchAll** verwendet wird.

```
bus_service.create_subscription('mytopic', 'AllMessages')
```

### <a name="create-subscriptions-with-filters"></a>Erstellen von Abonnements mit Filtern
Sie können auch Filter definieren, durch die Sie angeben können, welche an ein Thema gesendeten Nachrichten in einem bestimmten Themenabonnement angezeigt werden sollen.

Der flexibelste, von Abonnements unterstützte Filtertyp ist **SqlFilter**, der eine Teilmenge von SQL92 implementiert. SQL-Filter werden auf die Eigenschaften der Nachrichten angewendet, die für das Thema veröffentlicht werden. Weitere Informationen zu den Ausdrücken, die mit einem SQL-Filter verwendet werden können, finden Sie in der Syntax zu [SqlFilter.SqlExpression][SqlFilter.SqlExpression].

Sie können einem Abonnement mithilfe der Methode **create\_rule** des **ServiceBusService**-Objekts Filter hinzufügen. Durch diese Methode können Sie neue Filter zu einem vorhandenen Abonnement hinzufügen.

> [!NOTE]
> Da der Standardfilter automatisch auf alle neuen Abonnements angewendet wird, müssen Sie zuerst den Standardfilter entfernen. Ansonsten überschreibt **MatchAll** alle Filter, die Sie angeben. Sie können die Standardregel mithilfe der Methode **delete\_rule** des **ServiceBusService**-Objekts entfernen.
> 
> 

Mit dem folgenden Beispiel wird ein Abonnement namens `HighMessages` mit einem **SqlFilter** erstellt, der nur Nachrichten auswählt, deren benutzerdefinierte Eigenschaft **messagenumber** größer ist als 3:

```
bus_service.create_subscription('mytopic', 'HighMessages')

rule = Rule()
rule.filter_type = 'SqlFilter'
rule.filter_expression = 'messagenumber > 3'

bus_service.create_rule('mytopic', 'HighMessages', 'HighMessageFilter', rule)
bus_service.delete_rule('mytopic', 'HighMessages', DEFAULT_RULE_NAME)
```

Ebenso erstellt das folgende Beispiel ein Abonnement namens `LowMessages` mit einem **SqlFilter**-Element, das nur Nachrichten auswählt, deren benutzerdefinierte Eigenschaft **messagenumber** kleiner oder gleich 3 ist:

```
bus_service.create_subscription('mytopic', 'LowMessages')

rule = Rule()
rule.filter_type = 'SqlFilter'
rule.filter_expression = 'messagenumber <= 3'

bus_service.create_rule('mytopic', 'LowMessages', 'LowMessageFilter', rule)
bus_service.delete_rule('mytopic', 'LowMessages', DEFAULT_RULE_NAME)
```

Wenn eine Nachricht an `mytopic` gesendet wird, wird diese nun stets an die Empfänger des Themenabonnements **AllMessages** zugestellt. Sie wird selektiv an die Empfänger der Themenabonnements **HighMessages** und **LowMessages** zugestellt (je nach Inhalt der Nachricht).

## <a name="send-messages-to-a-topic"></a>Senden von Nachrichten an ein Thema
Um eine Nachricht an ein Service Bus-Thema zu senden, muss die Anwendung die **send\_topic\_message**-Methode des **ServiceBusService**-Objekts verwenden.

Das folgende Beispiel zeigt, wie Sie fünf Testnachrichten an `mytopic` senden. Beachten Sie, dass der Eigenschaftswert **messagenumber** jeder Nachricht gemäß der Iteration der Schleife variiert (auf diese Weise wird bestimmt, welche Abonnements die Nachricht erhalten):

```
for i in range(5):
    msg = Message('Msg {0}'.format(i).encode('utf-8'), custom_properties={'messagenumber':i})
    bus_service.send_topic_message('mytopic', msg)
```

Service Bus-Themen unterstützen eine maximale Nachrichtengröße von 256 KB im [Standard-Tarif](service-bus-premium-messaging.md) und 1 MB im [Premium-Tarif](service-bus-premium-messaging.md). Der Header, der die standardmäßigen und benutzerdefinierten Anwendungseigenschaften enthält, kann eine maximale Größe von 64 KB haben. Es gibt keine Beschränkung für die Anzahl der Nachrichten, die ein Thema enthält. Es gibt jedoch eine Obergrenze für die Gesamtgröße der Nachrichten eines Themas. Die Themengröße wird bei der Erstellung definiert. Die Obergrenze beträgt 5 GB. Weitere Informationen zu Kontingenten finden Sie unter [Service Bus-Kontingente][Service Bus-Kontingente].

## <a name="receive-messages-from-a-subscription"></a>Empfangen von Nachrichten aus einem Abonnement
Nachrichten werden von einem Abonnement über die Methode **receive\_subscription\_message** auf dem **ServiceBusService**-Objekt empfangen:

```
msg = bus_service.receive_subscription_message('mytopic', 'LowMessages', peek_lock=False)
print(msg.body)
```

Wenn der Parameter **peek\_lock** auf **FALSE** festgelegt ist, werden Nachrichten nach dem Lesen aus dem Abonnement gelöscht. Sie können die Nachricht lesen (peek) und sperren, ohne sie aus der Warteschlange zu löschen, indem Sie den Parameter **peek\_lock** auf **TRUE** festlegen.

Das Verhalten für das Lesen und Löschen der Nachricht als Teil des Empfangsvorgangs ist das einfachste Modell. Es wird am besten für Szenarien eingesetzt, bei denen es eine Anwendung tolerieren kann, wenn eine Nachricht bei Auftreten eines Fehlers nicht verarbeitet wird. Um dieses Verfahren zu verstehen, stellen Sie sich ein Szenario vor, in dem der Consumer die Empfangsanforderung ausstellt und dann abstürzt, bevor diese verarbeitet wird. Da Service Bus die Nachricht als verwendet markiert hat, wird er jene Nachricht auslassen, die vor dem Absturz verwendet wurde, wenn die Anwendung neu startet und erneut mit der Verwendung von Nachrichten beginnt.

Wenn der Parameter **peek\_lock** auf **TRUE** festgelegt ist, wird der Empfangsvorgang zu einem zweistufigen Vorgang. Dadurch können Anwendungen unterstützt werden, die das Auslassen bzw. Fehlen von Nachrichten nicht zulassen können. Wenn Service Bus eine Anfrage erhält, ermittelt der Dienst die nächste zu verarbeitende Nachricht, sperrt diese, um zu verhindern, dass andere Consumer sie erhalten, und sendet sie dann zurück an die Anwendung. Nachdem die Anwendung die Verarbeitung der Nachricht abgeschlossen hat (oder sie zwecks zukünftiger Verarbeitung zuverlässig gespeichert hat), führt Sie die zweite Phase des Empfangsprozesses durch Aufrufen der Methode **delete** für das **Message**-Objekt aus. Die Methode **delete** markiert die Nachricht als verarbeitet und entfernt sie aus dem Abonnement.

```
msg = bus_service.receive_subscription_message('mytopic', 'LowMessages', peek_lock=True)
print(msg.body)

msg.delete()
```

## <a name="how-to-handle-application-crashes-and-unreadable-messages"></a>Behandeln von Anwendungsabstürzen und nicht lesbaren Nachrichten
Service Bus stellt Funktionen zur Verfügung, die Sie bei der ordnungsgemäßen Behandlung von Fehlern in der Anwendung oder bei Problemen beim Verarbeiten einer Nachricht unterstützen. Wenn eine Empfängeranwendung die Nachricht aus einem bestimmten Grund nicht verarbeiten kann, so kann sie die Methode **unlock** für das **Message**-Objekt aufrufen. Dies führt dazu, dass Service Bus die Nachricht innerhalb des Abonnements entsperrt und zur Verfügung stellt, damit sie erneut empfangen werden kann – durch die gleiche verarbeitende Anwendung oder durch eine andere verarbeitenden Anwendung.

Zudem wird einer im Abonnement gesperrten Anwendung ein Zeitlimit zugeordnet. Wenn die Anwendung die Nachricht vor Ablauf des Sperrzeitlimits nicht verarbeiten kann (zum Beispiel wenn die Anwendung abstürzt) entsperrt Service Bus die Nachricht automatisch und macht sie verfügbar, um erneut empfangen zu werden.

Falls die Anwendung nach der Verarbeitung der Nachricht, aber vor Abrufen der Methode **delete** abstürzt, wird die Nachricht wieder an die Anwendung zugestellt, wenn diese neu gestartet wird. Dies wird häufig als **At Least Once Processing** (Mindestens einmal verarbeiten) bezeichnet und bedeutet, dass jede Nachricht mindestens einmal verarbeitet wird, wobei dieselbe Nachricht in bestimmten Situationen möglicherweise erneut zugestellt wird. Wenn eine doppelte Verarbeitung im betreffenden Szenario nicht geeignet ist, sollten Anwendungsentwickler ihrer Anwendung zusätzliche Logik für den Umgang mit der Übermittlung doppelter Nachrichten hinzufügen. Dies wird häufig durch die Verwendung der **MessageId**-Eigenschaft der Nachricht erzielt, die über mehrere Zustellungsversuche hinweg konstant bleibt.

## <a name="delete-topics-and-subscriptions"></a>Löschen von Themen und Abonnements
Themen und Abonnements sind persistent und müssen über das [Azure-Portal][Azure-Portal]oder programmgesteuert explizit gelöscht werden. Im folgenden Beispiel wird das Thema `mytopic` gelöscht:

```
bus_service.delete_topic('mytopic')
```

Durch das Löschen eines Themas werden auch alle Abonnements gelöscht, die mit dem Thema registriert sind. Abonnements können auch unabhängig gelöscht werden. Der folgende Code zeigt, wie das Abonnement `HighMessages` aus dem Thema `mytopic` gelöscht wird:

```
bus_service.delete_subscription('mytopic', 'HighMessages')
```

## <a name="next-steps"></a>Nächste Schritte
Nachdem Sie nun mit den Grundlagen der Servicebus-Themen vertraut sind, finden Sie unter den folgenden Links weitere Informationen.

* Siehe [Warteschlangen, Themen und Abonnements][Warteschlangen, Themen und Abonnements].
* Referenz für [SqlFilter.SqlExpression][SqlFilter.SqlExpression].

[Azure-Portal]: https://portal.azure.com
[Python Azure-Paket]: https://pypi.python.org/pypi/azure  
[Warteschlangen, Themen und Abonnements]: service-bus-queues-topics-subscriptions.md
[SqlFilter.SqlExpression]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.sqlfilter.sqlexpression.aspx
[Service Bus-Kontingente]: service-bus-quotas.md 



<!--HONumber=Nov16_HO3-->


