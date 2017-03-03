---
title: Gekoppelte Azure Service Bus-Namespaces | Microsoft Docs
description: Details und Kosten zur Implementierung von gekoppelten Namespaces
services: service-bus-messaging
documentationcenter: na
author: sethmanheim
manager: timlt
editor: 
ms.assetid: 2440c8d3-ed2e-47e0-93cf-ab7fbb855d2e
ms.service: service-bus-messaging
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/16/2017
ms.author: sethm
translationtype: Human Translation
ms.sourcegitcommit: d987aa22379ede44da1b791f034d713a49ad486a
ms.openlocfilehash: 84e125dffcac3f3a54250587c5238b50d3a6cb95
ms.lasthandoff: 02/16/2017


---
# <a name="paired-namespace-implementation-details-and-cost-implications"></a>Details zur Implementierung von gekoppelten Namespaces und Kostenaspekte
Mit der [PairNamespaceAsync][PairNamespaceAsync]-Methode unter Verwendung einer [SendAvailabilityPairedNamespaceOptions][SendAvailabilityPairedNamespaceOptions]-Instanz werden sichtbare Aufgaben in Ihrem Namen ausgeführt. Da beim Verwenden des Features Kostenaspekte zu beachten sind, ist es ratsam, sich mit diesen Aufgaben vertraut zu machen. So sind Sie auf das Auftreten dieses Verhaltens vorbereitet. Die API veranlasst in Ihrem Namen das folgende automatische Verhalten:

* Erstellung von Backlog-Warteschlangen
* Erstellung eines [MessageSender][MessageSender]-Objekts, das mit Warteschlangen oder Themen kommuniziert.
* Wenn eine Messagingentitätt nicht mehr verfügbar ist, werden Ping-Nachrichten an die Entität gesendet, um nach Möglichkeit zu ermitteln, wann die Entität wieder verfügbar sein wird
* Optionale Erstellung einer Gruppe von „Nachrichtensystemen”, mit denen Nachrichten aus den Backlog-Warteschlangen in die primären Warteschlangen verschoben werden
* Koordinierung des Schließens bzw. der Fehlerbehandlung der primären und sekundären [MessagingFactory][MessagingFactory]-Instanzen.

Einfach ausgedrückt funktioniert das Feature wie folgt: Wenn die primäre Entität fehlerfrei ist, werden keine Verhaltensänderungen vorgenommen. Wenn der [FailoverInterval][FailoverInterval]-Zeitraum abläuft und die primäre Entität nach einer nicht vorübergehenden [MessagingException][MessagingException] oder [TimeoutException][TimeoutException] keine erfolgreichen Sendevorgänge erkennt, tritt das folgende Verhalten auf:

1. Sendevorgänge an die primäre Entität werden deaktiviert, und das System sendet Ping-Nachrichten an die primäre Entität, bis diese erfolgreich zugestellt werden können.
2. Es wird eine zufällige Backlog-Warteschlange ausgewählt.
3. [BrokeredMessage][BrokeredMessage]-Objekte werden an die ausgewählte Backlog-Warteschlange geleitet.
4. Wenn für einen Sendevorgang an die gewählte Backlog-Warteschlange ein Fehler auftritt, wird diese Warteschlange aus der Rotation herausgenommen und eine neue Warteschlange ausgewählt. Alle Absender der [MessagingFactory][MessagingFactory]-Instanz werden über den Fehler informiert.

Die Sequenz ist in den folgenden Abbildungen dargestellt. Zuerst werden vom Absender die Nachrichten gesendet.

![Gekoppelte Namespaces][0]

Wenn das Senden an die primäre Warteschlange nicht erfolgreich ist, beginnt der Absender damit, Nachrichten an eine zufällig ausgewählte Backlog-Warteschlange zu senden. Gleichzeitig wird eine Ping-Aufgabe gestartet.

![Gekoppelte Namespaces][1]

An diesem Punkt befinden sich die Nachrichten weiterhin in der sekundären Warteschlange und wurden nicht an die primäre Warteschlange übermittelt. Sobald die primäre Warteschlange wieder fehlerfrei ist, sollte mindestens ein Prozess den „Siphon“ ausführen. Über den Siphon werden die Nachrichten aus den verschiedenen Backlog-Warteschlangen an die richtigen Zielentitäten (Warteschlangen und Themen) zugestellt.

![Gekoppelte Namespaces][2]

Im weiteren Verlauf dieses Themas wird näher beschrieben, wie die einzelnen Elemente funktionieren.

## <a name="creation-of-backlog-queues"></a>Erstellen von Backlog-Warteschlangen
Das [SendAvailabilityPairedNamespaceOptions][SendAvailabilityPairedNamespaceOptions]-Objekt, das an die [PairNamespaceAsync][PairNamespaceAsync]-Methode übergeben wird, gibt die Anzahl von Backlog-Warteschlangen an, die Sie verwenden möchten. Jede Backlog-Warteschlange wird dann erstellt, und die folgenden Eigenschaften werden explizit festgelegt (alle anderen Werte werden auf die [QueueDescription][QueueDescription]-Standardeinstellungen festgelegt):

| Pfad | [primärer Namespace]/x-servicebus-transfer/[Index], wobei [Index] ein Wert in [0, BacklogQueueCount) ist |
| --- | --- |
| MaxSizeInMegabytes |5120 |
| MaxDeliveryCount |int.MaxValue |
| DefaultMessageTimeToLive |TimeSpan.MaxValue |
| AutoDeleteOnIdle |TimeSpan.MaxValue |
| LockDuration |1 Minute |
| EnableDeadLetteringOnMessageExpiration |true |
| EnableBatchedOperations |true |

Die erste Backlog-Warteschlange, die für den Namespace **contoso** erstellt wird, hat beispielsweise den Namen `contoso/x-servicebus-transfer/0`.

Bei der Erstellung der Warteschlangen überprüft der Code zuerst, ob eine Warteschlange bereits vorhanden ist. Wenn die Warteschlange nicht vorhanden ist, wird die Warteschlange erstellt. Vom Code werden „zusätzliche“ Backlog-Warteschlangen nicht bereinigt. Falls die Anwendung mit dem primären Namespace **contoso** beispielsweise fünf Backlog-Warteschlangen anfordert, aber bereits eine Backlog-Warteschlange mit dem Pfad `contoso/x-servicebus-transfer/7` vorhanden ist, existiert diese zusätzliche Backlog-Warteschlange zwar, wird aber nicht verwendet. Im System sind explizit zusätzliche Backlog-Warteschlangen zulässig, die nicht verwendet werden. Als Namespace-Besitzer sind Sie für die Bereinigung aller nicht verwendeten oder unerwünschten Backlog-Warteschlangen verantwortlich. Der Grund für diese Entscheidung ist, dass Service Bus nicht wissen kann, welchen Zweck sämtliche Warteschlangen in Ihrem Namespace haben. Wenn eine Warteschlange mit dem jeweiligen Namen vorhanden ist, die vorausgesetzte [QueueDescription][QueueDescription] aber nicht erfüllt, gelten in Bezug auf die Änderung des Standardverhaltens zudem Ihre eigenen Gründe. Für Änderungen, die von Ihrem Code an den Backlog-Warteschlangen vorgenommen werden, wird keine Garantie übernommen. Stellen Sie sicher, dass Sie Ihre Änderungen gründlich testen.

## <a name="custom-messagesender"></a>Benutzerdefiniertes MessageSender-Objekt
Beim Senden durchlaufen alle Nachrichten ein internes [MessageSender][MessageSender]-Objekt, das ein normales Verhalten zeigt, wenn alles funktioniert. Falls Fehler auftreten, wird die Umleitung an die Backlog-Warteschlangen genutzt. Nach dem Empfang eines nicht vorübergehenden Fehlers wird ein Zeitgeber gestartet. Nach einem [TimeSpan][TimeSpan]-Zeitraum, der aus dem [FailoverInterval][FailoverInterval]-Eigenschaftswert besteht und in dem keine Nachrichten erfolgreich gesendet werden, wird das Failover aktiviert. An diesem Punkt werden für jede Entität die folgenden Schritte ausgeführt:

* Eine Ping-Aufgabe führt alle [PingPrimaryInterval][PingPrimaryInterval]-Vorkommen aus, um zu prüfen, ob die Entität verfügbar ist. Nachdem diese Aufgabe erfolgreich abgeschlossen wurde, beginnt der gesamte Clientcode, von dem die Entität verwendet wird, sofort mit dem Senden neuer Nachrichten an den primären Namespace.
* Zukünftige Anforderungen zum Senden an dieselbe Entität von anderen Absendern führen dazu, dass die [BrokeredMessage][BrokeredMessage], die zum Vornehmen von Änderungen gesendet wird, in der Backlog-Warteschlange enthalten ist. Durch die Änderung werden einige Eigenschaften aus dem [BrokeredMessage][BrokeredMessage]-Objekt entfernt und an einem anderen Ort gespeichert. Die folgenden Eigenschaften werden gelöscht und unter einem neuen Alias hinzugefügt, sodass Service Bus und das SDK Nachrichten einheitlich verarbeiten können:

| Alter Eigenschaftenname | Neuer Eigenschaftenname |
| --- | --- |
| SessionId |x-ms-sessionid |
| TimeToLive |x-ms-timetolive |
| ScheduledEnqueueTimeUtc |x-ms-path |

Der ursprüngliche Zielpfad wird ebenfalls in der Nachricht als Eigenschaft mit dem Namen „x-ms-path“ gespeichert. Bei diesem Aufbau können Nachrichten für viele Entitäten in einer einzigen Backlog-Warteschlange gemeinsam vorhanden sein. Die Eigenschaften werden durch den Siphon zurückübersetzt.

Für das benutzerdefinierte [MessageSender][MessageSender]-Objekt können Probleme auftreten, wenn Nachrichten in die Nähe das Grenzwerts von 256 KB eingehen und das Failover aktiviert wird. Das benutzerdefinierte [MessageSender][MessageSender]-Objekt speichert Nachrichten für alle Warteschlangen und Themen zusammen in den Backlog-Warteschlangen. Bei diesem Objekt werden Nachrichten von vielen primären Elementen in den Backlog-Warteschlangen gemischt. Zum Behandeln des Lastenausgleichs für viele Clients, die einander nicht kennen, wählt das SDK zufällig eine Backlog-Warteschlange für jedes [QueueClient][QueueClient]- oder [TopicClient][TopicClient]-Element, das Sie im Code erstellen.

## <a name="pings"></a>Ping-Nachrichten
Eine Ping-Nachricht ist eine leere [BrokeredMessage][BrokeredMessage], bei der die [ContentType][ContentType]-Eigenschaft auf „application/vnd.ms-servicebus-ping“ festgelegt ist und die über einen [TimeToLive][TimeToLive]-Wert von 1 Sekunde verfügt. Diese Ping-Nachricht verfügt in Service Bus über ein besonderes Merkmal: Der Server stellt keine Ping-Nachricht zu, wenn ein Aufrufer eine [BrokeredMessage][BrokeredMessage] anfordert. Sie müssen also nicht lernen, wie Sie diese Nachrichten empfangen und ignorieren. An jede Entität (eindeutige Warteschlange oder Thema) pro [MessagingFactory][MessagingFactory]-Instanz pro Client wird eine Ping-Nachricht gesendet, wenn diese als nicht verfügbar angesehen werden. Dies geschieht standardmäßig einmal pro Minute. Ping-Nachrichten gelten als reguläre Service Bus-Nachrichten und können Kosten für Bandbreite und Nachrichtentransport verursachen. Sobald die Clients erkennen, dass das System verfügbar ist, werden die Nachrichten gestoppt.

## <a name="the-syphon"></a>Siphon
Mindestens ein ausführbares Programm in der Anwendung sollte den Siphon aktiv ausführen. Der Siphon führt einen langen Abfrage- und Empfangsvorgang durch, der 15 Minuten dauert. Wenn alle Entitäten verfügbar sind und Sie über zehn Backlog-Warteschlangen verfügen, ruft die Anwendung, die den Siphon hostet, den Empfangsvorgang 40-mal pro Stunde, 960-mal pro Tag und 28.800-mal in 30 Tagen auf. Wenn der Siphon aktiv Nachrichten aus der Backlog-Warteschlange in die primäre Warteschlange verschiebt, fallen für jede Nachricht die folgenden Gebühren an (Standardgebühren für Nachrichtengröße und Bandbreite gelten in allen Phasen):

1. Senden an die Backlog-Warteschlange
2. Empfangen von der Backlog-Warteschlange
3. Senden an die primäre Warteschlange
4. Empfangen von der primären Warteschlange

## <a name="closefault-behavior"></a>Schließ-/Fehlerverhalten
Innerhalb einer Anwendung, die den Siphon hostet, wird in folgenden Fällen der Siphon aktiviert: Sobald für das primäre oder sekundäre [MessagingFactory][MessagingFactory]-Element ein Fehler auftritt oder sobald es geschlossen wird, ohne dass für das Partnerelement auch einer dieser Vorgänge eintritt, und dieser Zustand vom Siphon erkannt wird. Wenn das andere [MessagingFactory][MessagingFactory]-Element nicht innerhalb von fünf Sekunden geschlossen wird, generiert der Siphon für das noch geöffnete [MessagingFactory][MessagingFactory]-Element einen Fehler.

## <a name="next-steps"></a>Nächste Schritte
Ausführliche Informationen zum asynchronen Service Bus-Messaging finden Sie unter [Asynchrone Messagingmuster und hohe Verfügbarkeit][Asynchronous messaging patterns and high availability]. 

[PairNamespaceAsync]: /dotnet/api/microsoft.servicebus.messaging.messagingfactory#Microsoft_ServiceBus_Messaging_MessagingFactory_PairNamespaceAsync_Microsoft_ServiceBus_Messaging_PairedNamespaceOptions_
[SendAvailabilityPairedNamespaceOptions]: /dotnet/api/microsoft.servicebus.messaging.sendavailabilitypairednamespaceoptions
[MessageSender]: /dotnet/api/microsoft.servicebus.messaging.messagesender
[MessagingFactory]: /dotnet/api/microsoft.servicebus.messaging.messagingfactory
[FailoverInterval]: /dotnet/api/microsoft.servicebus.messaging.pairednamespaceoptions#Microsoft_ServiceBus_Messaging_PairedNamespaceOptions_FailoverInterval
[MessagingException]: /dotnet/api/microsoft.servicebus.messaging.messagingexception
[TimeoutException]: https://msdn.microsoft.com/library/azure/system.timeoutexception.aspx
[BrokeredMessage]: /dotnet/api/microsoft.servicebus.messaging.brokeredmessage
[QueueDescription]: /dotnet/api/microsoft.servicebus.messaging.queuedescription
[TimeSpan]: https://msdn.microsoft.com/library/azure/system.timespan.aspx
[PingPrimaryInterval]: /dotnet/api/microsoft.servicebus.messaging.sendavailabilitypairednamespaceoptions#Microsoft_ServiceBus_Messaging_SendAvailabilityPairedNamespaceOptions_PingPrimaryInterval
[QueueClient]: /dotnet/api/microsoft.servicebus.messaging.queueclient
[TopicClient]: /dotnet/api/microsoft.servicebus.messaging.topicclient
[ContentType]: /dotnet/api/microsoft.servicebus.messaging.brokeredmessage#Microsoft_ServiceBus_Messaging_BrokeredMessage_ContentType
[TimeToLive]: /dotnet/api/microsoft.servicebus.messaging.brokeredmessage#Microsoft_ServiceBus_Messaging_BrokeredMessage_TimeToLive
[Asynchronous messaging patterns and high availability]: service-bus-async-messaging.md
[0]: ./media/service-bus-paired-namespaces/IC673405.png
[1]: ./media/service-bus-paired-namespaces/IC673406.png
[2]: ./media/service-bus-paired-namespaces/IC673407.png

