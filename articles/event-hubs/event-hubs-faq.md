---
title: "Häufig gestellte Fragen (FAQ) zu Event Hubs | Microsoft Docs"
description: "Event Hubs – häufig gestellte Fragen"
services: event-hubs
documentationcenter: na
author: sethmanheim
manager: timlt
editor: 
ms.assetid: bfa10984-eb22-4671-861a-f377a90d9372
ms.service: event-hubs
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/01/2016
ms.author: sethm
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: eb8a1f5b46ed5bfbdc61789ffc48a79927d9c19d


---
# <a name="event-hubs-faq"></a>Event Hubs – häufig gestellte Fragen
Event Hubs bietet umfangreiche Erfassung, Persistenz und Verarbeitung von Datenereignissen aus Datenquellen mit hohem Durchsatz und/oder Millionen von Geräten. Zusammen mit Service Bus-Warteschlangen und -Themen ermöglicht Event Hubs persistente Befehlszeilen- und Steuerelement-Bereitstellungen für [Internet der Dinge](https://azure.microsoft.com/services/iot-hub/) -Szenarien (IoT).

Dieser Artikel enthält Preisinformationen und beantwortet einige häufig gestellte Fragen zu Event Hubs:

## <a name="pricing-information"></a>Preisinformationen
Umfassende Informationen zu den Preisen von Event Hubs finden Sie unter [Event Hubs – Preisübersicht](https://azure.microsoft.com/pricing/details/event-hubs/).

## <a name="how-are-event-hubs-ingress-events-calculated"></a>Wie werden Eingangsereignisse von Event Hubs berechnet?
Jedes an einen Event Hub gesendete Ereignis wird als abrechenbare Nachricht gezählt. Ein *Eingangsereignis* ist als eine Dateneinheit definiert, die kleiner oder gleich 64 KB ist. Jedes Ereignis, das kleiner als oder gleich 64 KB ist, gilt als kostenpflichtig. Wenn das Ereignis größer als 64 KB ist, wird die Anzahl der abzurechnenden Ereignisse gemäß der Ereignisgröße in Vielfachen von 64 KB berechnet. Beispielsweise wird ein an ein Event Hub gesendetes 8-KB-Ereignis als ein Ereignis abgerechnet, aber eine Nachricht in der Größe von 96 KB, die an den Event Hub gesendet wird, wird als zwei Ereignisse in Rechnung gestellt.

Von einem Event Hub genutzte Ereignisse sowie Verwaltungsvorgänge und Kontrollaufrufe wie etwa Checkpoints werden nicht als abzurechnende Eingangsereignisse gezählt, sondern bis zur erlaubten Durchsatzeinheit angesammelt.

## <a name="what-are-event-hubs-throughput-units"></a>Was sind Event Hubs-Durchsatzeinheiten?
Sie wählen die Event Hubs-Durchsatzeinheiten explizit aus, entweder über das Azure-Portal oder mithilfe von Event Hubs Resource Manager-Vorlagen. Durchsatzeinheiten betreffen alle Event Hubs in einem Event Hubs-Namespace, und durch jede Durchsatzeinheit erhält der Namespace die Berechtigung für folgende Funktionen:

* Eingangsereignisse bis zu 1 MB pro Sekunde (Ereignisse, die an einen Event Hub gesendet werden), aber nicht mehr als 1.000 Eingangsereignisse, Verwaltungsvorgänge oder Steuer-API-Aufrufe pro Sekunde.
* Eingangsereignisse (Ereignisse, die von einem Ereignis-Hub genutzt werden) mit bis zu 2 MB pro Sekunde.
* Bis zu 84 GB Ereignisspeicher (ausreichend für die Standardbeibehaltungsdauer im 24-Stunden-Format).

Event Hubs-Durchsatzeinheiten werden stündlich abgerechnet, basierend auf der maximalen Anzahl an ausgewählten Einheiten in der betreffenden Stunde.

## <a name="how-are-event-hubs-throughput-unit-limits-enforced"></a>Wie werden Beschränkungen für Event Hubs-Durchsatzeinheiten durchgesetzt?
Wenn der gesamte eingehende Durchsatz oder die gesamte eingehende Ereignisrate über alle Event Hubs hinweg in einem Namespace das Einheitenkontingent für den aggregierten Durchsatz überschreitet, werden Absender gedrosselt und erhalten die Fehlermeldung, dass das Eingangskontingent überschritten wurde.

Wenn der gesamte ausgehende Durchsatz oder die gesamte ausgehende Ereignisrate über alle Event Hubs hinweg in einem Namespace das Einheitenkontingent für den aggregierten Durchsatz überschreitet, werden Empfänger gedrosselt und erhalten die Fehlermeldung, dass das Ausgangskontingent überschritten wurde. Eingangs-und Ausgangskontingente werden separat durchgesetzt, sodass kein Absender die Verlangsamung der Nutzung herbeiführen kann und kein Empfänger verhindern kann, dass Ereignisse an einen Event Hub gesendet werden.

Beachten Sie, dass die Auswahl der Durchsatzeinheiten unabhängig von der Anzahl der Event Hubs-Partitionen ist. Während jede Partition einen maximalen Durchsatz von 1 MB pro Sekunde eingehend (bei einem Maximum von 1.000 Ereignissen pro Sekunde) und 2 MB pro Sekunde ausgehend bietet, gibt es keine festen Gebühren für die Partitionen selbst. Die Gebühr gilt für die aggregierten Durchsatzeinheiten in allen Event Hubs in einem Event Hubs-Namespace. Mit diesem Muster können Sie genügend Partitionen erstellen, um die erwartete maximale Belastung für die Systeme ohne weitere Kosten pro Durchsatzeinheit zu unterstützen, bis die Ereignisladung im System tatsächlich höhere Durchsatzzahlen erfordert, ohne die Struktur und Architektur Ihrer Systeme ändern zu müssen, während die Belastung des Systems erhöht wird.

## <a name="is-there-a-limit-on-the-number-of-throughput-units-that-can-be-selected"></a>Gibt es eine Beschränkung für die Anzahl der Durchsatzeinheiten, die ausgewählt werden können?
Es gibt ein Kontingent von 20 Durchsatzeinheiten pro Namespace. Sie können ein größeres Kontingent an Durchsatzeinheiten anfordern, indem Sie ein Supportticket ausfüllen. Über das Limit von 20 Durchsatzeinheiten hinaus sind Pakete in 20 bis 100 Durchsatzeinheiten verfügbar. Beachten Sie, dass es bei Verwendung von mehr als 20 Durchsatzeinheiten nicht die Möglichkeit gibt, die Anzahl der Durchsatzeinheiten zu ändern, ohne ein Supportticket auszufüllen.

## <a name="is-there-a-charge-for-retaining-event-hubs-events-for-more-than-24-hours"></a>Gibt es eine Gebühr für die Beibehaltung von Event Hubs-Ereignissen für mehr als 24 Stunden?
Die Standard-Ebene für Event Hubs erlaubt eine Aufbewahrung von Nachrichten länger als 24 Stunden, maximal 30 Tage. Wenn die Größe der Gesamtanzahl der gespeicherten Ereignisse das Speicherkontingent für die Anzahl der ausgewählten Durchsatzeinheiten (84 GB pro Durchsatzeinheit) überschreitet, wird die überschrittene Größe zur veröffentlichten Rate für den Azure Blob Storage in Rechnung gestellt. Das Speicherkontingent in allen Durchsatzeinheiten deckt alle Speicherkosten für die Aufbewahrungsdauer von 24 Stunden (Standard), selbst, wenn die Durchsatzeinheit bis zum maximal erlaubten Eingang verbraucht ist.

## <a name="what-is-the-maximum-retention-period"></a>Was ist die maximale Beibehaltungsdauer?
Die Standard-Ebene für Event Hubs unterstützt derzeit einen maximalen Aufbewahrungszeitraum von 7 Tagen. Beachten Sie, dass Event Hubs nicht als dauerhafter Datenspeicher vorgesehen sind. Beibehaltungsdauern größer als 24 Stunden sind für Szenarien vorgesehen, in denen es praktisch ist, einen Stream von Ereignissen in die gleichen Systeme wiederzugeben, wenn Sie beispielsweise ein neues Computerlernmodell für vorhandene Daten trainieren oder überprüfen wollen.

## <a name="how-is-the-event-hubs-storage-size-calculated-and-charged"></a>Wie wird die Event Hubs-Speichergröße berechnet und in Rechnung gestellt?
Die Gesamtgröße aller gespeicherten Ereignisse, einschließlich des gesamten internen Mehraufwands für Ereignisheader oder Speicherstrukturen auf Datenträgern in allen Event Hubs, wird im Laufe des Tages gemessen. Am Ende des Tages wird die maximale Speichergröße berechnet. Das tägliche Speicherkontingent wird auf Grundlage der Mindestanzahl der Durchsatzeinheiten berechnet, die im Laufe des Tages ausgewählt wurden (jede Durchsatzeinheit bietet ein Kontingent von 84 GB). Wenn die Gesamtgröße das berechnete tägliche Speicherkontingent überschreitet, wird der überschüssige Speicher zu Azure-Blob-Speichersätzen in Rechnung gestellt (auf der **lokal redundanter Speicher** Rate).

## <a name="can-i-use-a-single-amqp-connection-to-send-and-receive-from-event-hubs-and-service-bus-queuestopics"></a>Kann ich eine einzelne AMQP-Verbindung zum Senden und Empfangen von Event Hubs und Service Bus-Warteschlangen/-Themen verwenden?
Ja, solange sich alle Event Hubs, Warteschlangen und Themen im selben Namespace befinden. Daher können Sie bidirektionale und vermittelte Verbindungen auf zahlreichen Geräten implementieren – mit einer Latenz von Sekundenbruchteilen, kostengünstig und hochgradig skalierbar.

## <a name="do-brokered-connection-charges-apply-to-event-hubs"></a>Gelten vermittelte Verbindungsgebühren für Event Hubs?
Für Absender fallen Verbindungsgebühren nur an, wenn das AMQP-Protokoll verwendet wird. Es gibt keine Verbindungsgebühren für das Senden von Ereignissen über HTTP, unabhängig von der Anzahl der sendenden Systeme oder Geräte. Wenn Sie AMQP verwenden möchten (z. B. um effizienteres Ereignisstreaming zu erreichen oder bidirektionale Kommunikation in Befehls- und -Steuerungsszenarien von IoT zu ermöglichen), lesen Sie sich bitte die Seite [Service Bus-Preisinformationen](https://azure.microsoft.com/pricing/details/service-bus/) durch, um mehr über vermittelte Verbindungen und deren Taktung zu erfahren.

## <a name="what-is-the-difference-between-event-hubs-basic-and-standard-tiers"></a>Was ist der Unterschied zwischen den Event Hubs-Ebenen Basic und Standard?
Event Hubs im Standard-Tarif bieten mehr Features als der Basic-Tarif sowie einige vergleichbare Systeme. Zu diesen Features gehören Aufbewahrungszeiträume von mehr als 24 Stunden und die Fähigkeit, eine einzelne AMQP-Verbindung zum Senden von Befehlen an eine große Anzahl von Geräten mit einer Latenz von Sekundenbruchteilen sowie Telemetriedaten von diesen Geräten an Event Hubs zu senden. Die Liste der Funktionen finden Sie unter [Event Hubs – Preisdetails](https://azure.microsoft.com/pricing/details/event-hubs/)

## <a name="geographic-availability"></a>Geografische Verfügbarkeit
Event Hubs ist in den folgenden Regionen verfügbar:

| geografischer Raum | Regionen |
| --- | --- |
| USA |„USA, Mitte“, „USA, Osten“, „USA, Osten 2“, „USA, Mitte-Süden“, „USA, Westen“ |
| Europa |„Europa, Norden“, „Europa, Westen“ |
| Asien-Pazifik |„Asien, Osten“, „Asien, Südosten“ |
| Japan |„Japan, Osten“, „Japan, Westen“ |
| Brasilien |Brasilien, Süden |
| Australien |„Australien, Osten“, „Australien, Südosten“ |

## <a name="support-and-sla"></a>Support und SLA
Technischer Support für Event Hubs steht über die [Communityforen](https://social.msdn.microsoft.com/forums/azure/home)bereit. Der Support für die Abrechnungs- und Abonnementverwaltung wird kostenlos bereitgestellt.

Weitere Informationen zu unserem SLA finden Sie auf der Seite [Vereinbarungen zum Servicelevel](https://azure.microsoft.com/support/legal/sla/) .

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen zu Event Hubs finden Sie in den folgenden Artikeln:

* [Übersicht über Event Hubs][Übersicht über Event Hubs].
* Eine vollständige [Beispielanwendung zur Verwendung von Event Hubs][Beispielanwendung zur Verwendung von Event Hubs].

[Übersicht über Event Hubs]: event-hubs-overview.md
[Beispielanwendung zur Verwendung von Event Hubs]: https://code.msdn.microsoft.com/Service-Bus-Event-Hub-286fd097



<!--HONumber=Nov16_HO3-->


