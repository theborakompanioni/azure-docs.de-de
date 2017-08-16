---
title: "Automatische Weiterleitung von Azure Service Bus-Messagingentitäten | Microsoft-Dokumentation"
description: So verketten Sie eine Service Bus-Warteschlange oder ein Abonnement mit einer anderen Warteschlange oder einem anderen Thema.
services: service-bus-messaging
documentationcenter: na
author: sethmanheim
manager: timlt
editor: 
ms.assetid: f7060778-3421-402c-97c7-735dbf6a61e8
ms.service: service-bus-messaging
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/07/2017
ms.author: sethm
ms.translationtype: HT
ms.sourcegitcommit: caaf10d385c8df8f09a076d0a392ca0d5df64ed2
ms.openlocfilehash: 2656b3a276c542ca836b3949e4e493d7c7f48f16
ms.contentlocale: de-de
ms.lasthandoff: 08/08/2017

---
# <a name="chaining-service-bus-entities-with-auto-forwarding"></a>Verketten von Service Bus-Entitäten mit automatischer Weiterleitung

Mit dem Service Bus-Feature *Automatische Weiterleitung* können Sie eine Warteschlange oder ein Abonnement mit einer weiteren Warteschlange oder einem Thema aus dem selben Namespace verketten. Wenn die automatische Weiterleitung aktiviert ist, entfernt Service Bus die Nachrichten automatisch, die in der ersten Warteschlange oder dem Abonnement (Quelle) platziert wurden, und fügt sie in die zweite Warteschlange oder das Thema (Ziel) ein. Beachten Sie, dass es weiterhin möglich ist, eine Nachricht direkt an die Zielentität zu senden. Es ist außerdem nicht möglich, eine Unterwarteschlange, z.B. eine Warteschlange für unzustellbare Nachrichten, mit einer anderen Warteschlange oder einem Thema zu verketten.

## <a name="using-auto-forwarding"></a>Verwenden der automatischen Weiterleitung
Sie können die automatische Weiterleitung durch Festlegen der Eigenschaften [QueueDescription.ForwardTo][QueueDescription.ForwardTo] oder [SubscriptionDescription.ForwardTo][SubscriptionDescription.ForwardTo] der Objekte [QueueDescription][QueueDescription] oder [SubscriptionDescription][SubscriptionDescription] für die Quelle aktivieren, wie im folgenden Beispiel veranschaulicht.

```csharp
SubscriptionDescription srcSubscription = new SubscriptionDescription (srcTopic, srcSubscriptionName);
srcSubscription.ForwardTo = destTopic;
namespaceManager.CreateSubscription(srcSubscription));
```

Die Zielentität muss vorhanden sein, wenn die Quellentität erstellt wird. Wenn die Zielentität nicht vorhanden ist, gibt Service Bus eine Ausnahme zurück, wenn die Quellentität erstellt werden soll.

Sie können die automatische Weiterleitung zum Skalieren eines einzelnen Themas verwenden. Service Bus beschränkt die [Anzahl von Abonnements für ein bestimmtes Thema](service-bus-quotas.md) auf 2.000. Sie können weitere Abonnements durch Erstellen von Themen der zweiten Ebene aufnehmen. Hinweis: Selbst wenn Sie nicht durch die Service Bus-Beschränkung für die Anzahl der Abonnements gebunden sind, kann das Hinzufügen einer zweiten Ebene von Themen den Gesamtdurchsatz Ihres Themas verbessern.

![Szenario mit automatischer Weiterleitung][0]

Sie können die automatische Weiterleitung auch verwenden, um Nachrichtenabsender von den Empfängern zu entkoppeln. Betrachten Sie z. B. ein ERP-System, das aus drei Modulen besteht: Auftragsverarbeitung, Bestandsverwaltung und Kundenbeziehungsmanagement. Jedes dieser Module generiert Nachrichten, die in ein entsprechendes Thema aufgenommen werden. Alice und Bob sind Vertriebsmitarbeiter, die an allen Nachrichten interessiert sind, die sich auf ihre Kunden beziehen. Zum Empfangen dieser Nachrichten erstellen Alice und Bob eine persönliche Warteschlange und ein Abonnement für jedes der ERP-Themen, das automatisch alle Nachrichten an ihre Warteschlange weiterleitet.

![Szenario mit automatischer Weiterleitung][1]

Wenn Alice im Urlaub ist, wird ihre persönliche Warteschlange gefüllt, nicht das ERP-Thema. Da in diesem Szenario kein Vertriebsmitarbeiter Nachrichten empfangen hat, erreicht keines der ERP-Themen je das Kontingent.

## <a name="auto-forwarding-considerations"></a>Überlegungen zur automatischen Weiterleitung

Wenn die Zielentität zu viele Nachrichten gesammelt hat und das Kontingent überschreitet oder wenn die Zielentität deaktiviert ist, fügt die Quellentität die Nachrichten zur eigenen [Warteschlange für unzustellbare Nachrichten](service-bus-dead-letter-queues.md) hinzu, bis Speicherplatz im Ziel vorhanden ist (oder die Entität wieder aktiviert wird). Diese Nachrichten sind weiterhin in der Warteschlange für unzustellbare Nachrichten aktiv, daher müssen Sie sie explizit aus der Warteschlange für unzustellbare Nachrichten empfangen und verarbeiten.

Beim Verketteten einzelner Themen, um ein kombiniertes Thema mit vielen Abonnements zu erhalten, wird empfohlen, dass Sie eine moderate Anzahl von Abonnements für das Thema der ersten Ebene und viele Abonnements für Themen der zweiten Ebene nutzen. Beispiel: Ein Thema der ersten Ebene mit 20 Abonnements, von denen jedes mit einem Thema der zweiten Ebene mit 200 Abonnements verkettet ist, ermöglicht einen höheren Durchsatz als ein Thema der ersten Ebene mit 200 Abonnements, von denen jedes mit einem Thema der zweiten Ebene mit 20 Abonnements verkettet ist.

Service Bus rechnet einen Vorgang für jede weitergeleitete Nachricht ab. Beispiel: Das Senden einer Nachricht an ein Thema mit 20 Abonnements, von denen jedes so konfiguriert ist, dass Nachrichten automatisch an eine andere Warteschlange oder ein Thema weitergeleitet werden, wird als 21 Vorgänge abgerechnet, wenn alle Abonnements der ersten Ebene eine Kopie der Nachricht erhalten.

Um ein Abonnement zu erstellen, das mit einer anderen Warteschlange oder einem Thema verkettet ist, muss der Ersteller des Abonnements über die Berechtigung **Verwalten** für die Quell- und die Zielentität verfügen. Das Senden von Nachrichten an das Quellthema erfordert nur die Berechtigung **Senden** für das Quellthema.

## <a name="next-steps"></a>Nächste Schritte

Detaillierte Informationen über die automatische Weiterleitung finden Sie in den folgenden Referenzthemen:

* [ForwardTo][QueueDescription.ForwardTo]
* [QueueDescription][QueueDescription]
* [SubscriptionDescription][SubscriptionDescription]

Weitere Informationen zu Service Bus-Leistungssteigerungen finden Sie unter 

* [Bewährte Methoden für Leistungsoptimierungen mithilfe von Service Bus Messaging](service-bus-performance-improvements.md)
* [Partitionierte Messagingentitäten][Partitioned messaging entities]

[QueueDescription.ForwardTo]: /dotnet/api/microsoft.servicebus.messaging.queuedescription.forwardto#Microsoft_ServiceBus_Messaging_QueueDescription_ForwardTo
[SubscriptionDescription.ForwardTo]: /dotnet/api/microsoft.servicebus.messaging.subscriptiondescription.forwardto#Microsoft_ServiceBus_Messaging_SubscriptionDescription_ForwardTo
[QueueDescription]: /dotnet/api/microsoft.servicebus.messaging.queuedescription
[SubscriptionDescription]: /dotnet/api/microsoft.servicebus.messaging.queuedescription
[0]: ./media/service-bus-auto-forwarding/IC628631.gif
[1]: ./media/service-bus-auto-forwarding/IC628632.gif
[Partitioned messaging entities]: service-bus-partitioning.md

