---
title: "Übersicht über Service Bus Premium- und Standard-Preisstufen für Messaging | Microsoft Docs"
description: "Service Bus Premium- und Standard-Tarif für Messaging"
services: service-bus-messaging
documentationcenter: .net
author: djrosanova
manager: timlt
editor: 
ms.assetid: e211774d-821c-4d79-8563-57472d746c58
ms.service: service-bus-messaging
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 04/19/2017
ms.author: darosa;sethm;jotaub
translationtype: Human Translation
ms.sourcegitcommit: e0c999b2bf1dd38d8a0c99c6cdd4976cc896dd99
ms.openlocfilehash: 606838dbeec5e9762f67159ece1bbc8a78769420
ms.lasthandoff: 04/20/2017


---
# <a name="service-bus-premium-and-standard-messaging-tiers"></a>Service Bus Premium- und Standard-Preisstufe für Messaging

Service Bus-Messaging, das Entitäten wie Warteschlangen und Themen umfasst, kombiniert Messagingfunktionen für Unternehmen mit umfangreicher Veröffentlichen/Abonnieren-Semantik auf Cloudebene. Service Bus-Messaging wird als Kommunikationsbackbone für zahlreiche komplexe Cloudlösungen verwendet.

Der *Premium*-Tarif von Service Bus-Messaging ist für allgemeine Kundenanfragen zur Skalierung, Leistung und Verfügbarkeit für unternehmenswichtige Anwendungen vorgesehen. Obwohl die Funktionssätze fast identisch sind, sind diese zwei Stufen von Service Bus-Messaging für verschiedene Anwendungsfälle vorgesehen.

In der folgenden Tabelle sind einige allgemeine Unterschiede hervorgehoben:

| Premium | Standard |
| --- | --- |
| Hoher Durchsatz |Variabler Durchsatz |
| Vorhersagbare Leistung |Variable Latenzzeit |
| Feste Preise |Variable Preisgestaltung (nutzungsbasierte Bezahlung) |
| Möglichkeit zur Herauf- und Herunterskalierung der Workload |N/V |
| Nachrichtengröße bis 1 MB |Nachrichtengröße bis 256 KB |

**Service Bus Premium-Messaging** bietet Ressourcenisolierung auf CPU- und Arbeitsspeicherebene, sodass die Workloads der einzelnen Kunden isoliert ausgeführt werden. Dieser Ressourcencontainer wird als *Messaging-Einheit* bezeichnet. Jedem Premium-Namespace wird mindestens eine Messaging-Einheit zugeordnet. Sie können 1, 2 oder 4 Messaging-Einheiten für jeden Service Bus Premium-Namespace erwerben. Eine einzelne Workload oder Entität kann mehrere Messaging-Einheiten umfassen, und die Anzahl der Einheiten kann beliebig geändert werden. Gebühren werden jedoch im 24-Stunden- bzw. Tagesrhythmus berechnet. Das Ergebnis ist eine vorhersehbare und wiederholbare Leistung Ihrer Service Bus-basierten Lösung.

Diese Leistung ist nicht nur besser vorhersehbar und verfügbar, sondern auch schneller. Service Bus Premium-Messaging basiert auf dem in [Azure Event Hubs](https://azure.microsoft.com/services/event-hubs/) eingeführten Speichermodul. Mit Premium-Messaging wird bei Spitzenleistung eine viel höhere Geschwindigkeit als beim Standard-Tarif erzielt.

## <a name="premium-messaging-technical-differences"></a>Premium-Messaging – technische Unterschiede

In den folgenden Abschnitten werden einige Unterschiede zwischen der Premium- und der Standard-Messagingstufe erläutert.

### <a name="partitioned-queues-and-topics"></a>Partitionierte Warteschlangen und Themen

Partitionierte Warteschlangen und Themen werden beim Premium-Messaging unterstützt, funktionieren allerdings anders als in den Standard- und Basic-Tarifen des Service Bus-Messaging. Bei Premium-Messaging wird nicht SQL als Datenspeicher verwendet, und im Gegensatz zu einer gemeinsam verwendeten Plattform können Ressourcen nicht mehr gleichzeitig verwendet werden. Daher ist eine Partitionierung zur Steigerung der Leistung nicht erforderlich. Darüber hinaus wurde die Anzahl der Partitionen von 16 bei Standard-Messaging in zwei Partitionen bei Premium-Messaging geändert. Bei zwei Partitionen ist die Verfügbarkeit sichergestellt, und diese Anzahl eignet sich besser für die Premium-Laufzeitumgebung. Weitere Informationen zur Partitionierung finden Sie unter [Partitionierte Warteschlangen und Themen](service-bus-partitioning.md).

### <a name="express-entities"></a>Expressentitäten

Da Premium-Messaging in einer vollständig isolierten Laufzeitumgebung ausgeführt wird, werden Expressentitäten in Premium-Namespaces nicht unterstützt. Weitere Informationen zur Expressfunktion finden Sie in der [QueueDescription.EnableExpress](/dotnet/api/microsoft.servicebus.messaging.queuedescription.enableexpress?view=azureservicebus-4.0.0#Microsoft_ServiceBus_Messaging_QueueDescription_EnableExpress)-Eigenschaft.

## <a name="get-started-with-premium-messaging"></a>Erste Schritte mit Premium-Messaging

Die ersten Schritte mit Premium-Messaging sind einfach, und der Prozess ähnelt der Vorgehensweise für Standard-Messaging. Beginnen Sie, indem Sie [einen Namespace erstellen](service-bus-create-namespace-portal.md). Stellen Sie sicher, dass Sie unter **Tarif** die Option **Premium** wählen.

![create-premium-namespace][create-premium-namespace]

Sie können auch [Premium-Namespaces mit Azure Resource Manager-Vorlagen erstellen](https://azure.microsoft.com/en-us/resources/templates/101-servicebus-pn-ar/).


## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu Service Bus Messaging finden Sie in folgenden Themen:

* [Introducing Azure Service Bus Premium Messaging (Blogbeitrag)](http://azure.microsoft.com/blog/introducing-azure-service-bus-premium-messaging/) (Einführung in Azure Service Bus Premium-Messaging)
* [Introducing Azure Service Bus Premium Messaging (Channel9)](https://channel9.msdn.com/Blogs/Subscribe/Introducing-Azure-Service-Bus-Premium-Messaging) (Einführung in Azure Service Bus Premium-Messaging)
* [Service Bus Messaging: Flexible Datenübermittlung in der Cloud](service-bus-messaging-overview.md)
* [Verwenden von Service Bus-Warteschlangen](service-bus-dotnet-get-started-with-queues.md)

<!--Image references-->

[create-premium-namespace]: ./media/service-bus-premium-messaging/select-premium-tier.png

