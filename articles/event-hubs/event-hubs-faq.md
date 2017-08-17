---
title: "Azure Event Hubs – häufig gestellte Fragen (FAQ) | Microsoft-Dokumentation"
description: "Häufig gestellte Fragen (FAQ) zu Azure Event Hubs"
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
ms.date: 06/28/2017
ms.author: sethm;shvija
ms.translationtype: Human Translation
ms.sourcegitcommit: 857267f46f6a2d545fc402ebf3a12f21c62ecd21
ms.openlocfilehash: e0e33882acc90130ef93cd66c3d57bb90f78ccee
ms.contentlocale: de-de
ms.lasthandoff: 06/28/2017


---

# <a name="event-hubs-frequently-asked-questions"></a>Häufig gestellte Fragen zu Event Hubs

## <a name="general"></a>Allgemein

### <a name="what-is-the-difference-between-event-hubs-basic-and-standard-tiers"></a>Was ist der Unterschied zwischen den Event Hubs-Ebenen Basic und Standard?

Im Standard-Tarif von Azure Event Hubs stehen Ihnen mehr Funktionen als im Basic-Tarif zur Verfügung. Die folgenden Features sind in Standard enthalten:
* Längere Aufbewahrung von Ereignissen
* Zusätzliche Brokerverbindungen (Bei Überschreitung der enthaltenen Anzahl fällt eine Zusatzgebühr an.)
* Mehr als eine Consumergruppe
* [Capture](https://docs.microsoft.com/azure/event-hubs/event-hubs-archive-overview)

Weitere Informationen zu Tarifen, u.a. für Event Hubs Dedicated, finden Sie unter [Event Hubs – Preisdetails](https://azure.microsoft.com/pricing/details/event-hubs/).

### <a name="what-are-event-hubs-throughput-units"></a>Was sind Event Hubs-Durchsatzeinheiten?

Sie wählen die Event Hubs-Durchsatzeinheiten explizit aus, entweder über das Azure-Portal oder mithilfe von Event Hubs Resource Manager-Vorlagen. Durchsatzeinheiten betreffen alle Event Hubs in einem Event Hubs-Namespace, und durch jede Durchsatzeinheit erhält der Namespace die Berechtigung für folgende Funktionen:

* Eingangsereignisse bis zu 1 MB pro Sekunde (Ereignisse, die an einen Event Hub gesendet werden), aber nicht mehr als 1.000 Eingangsereignisse, Verwaltungsvorgänge oder Steuer-API-Aufrufe pro Sekunde.
* Eingangsereignisse (Ereignisse, die von einem Ereignis-Hub genutzt werden) mit bis zu 2 MB pro Sekunde.
* Bis zu 84 GB Ereignisspeicher (ausreichend für die Standardbeibehaltungsdauer im 24-Stunden-Format).

Event Hubs-Durchsatzeinheiten werden stündlich abgerechnet, basierend auf der maximalen Anzahl an ausgewählten Einheiten in der betreffenden Stunde.

### <a name="how-are-event-hubs-throughput-unit-limits-enforced"></a>Wie werden Beschränkungen für Event Hubs-Durchsatzeinheiten durchgesetzt?
Wenn der gesamte eingehende Durchsatz oder die gesamte eingehende Ereignisrate über alle Event Hubs hinweg in einem Namespace das Einheitenkontingent für den aggregierten Durchsatz überschreitet, werden Absender gedrosselt und erhalten die Fehlermeldung, dass das Eingangskontingent überschritten wurde.

Wenn der gesamte ausgehende Durchsatz oder die gesamte ausgehende Ereignisrate über alle Event Hubs hinweg in einem Namespace das Einheitenkontingent für den aggregierten Durchsatz überschreitet, werden Empfänger gedrosselt und erhalten die Fehlermeldung, dass das Ausgangskontingent überschritten wurde. Eingangs-und Ausgangskontingente werden separat durchgesetzt, sodass kein Absender die Verlangsamung der Nutzung herbeiführen kann und kein Empfänger verhindern kann, dass Ereignisse an einen Event Hub gesendet werden.

### <a name="is-there-a-limit-on-the-number-of-throughput-units-that-can-be-selected"></a>Gibt es eine Beschränkung für die Anzahl der Durchsatzeinheiten, die ausgewählt werden können?
Es gibt ein Kontingent von 20 Durchsatzeinheiten pro Namespace. Sie können ein größeres Kontingent an Durchsatzeinheiten anfordern, indem Sie ein Supportticket ausfüllen. Über das Limit von 20 Durchsatzeinheiten hinaus sind Pakete in 20 bis 100 Durchsatzeinheiten verfügbar. Beachten Sie, dass es bei Verwendung von mehr als 20 Durchsatzeinheiten nicht die Möglichkeit gibt, die Anzahl der Durchsatzeinheiten zu ändern, ohne ein Supportticket auszufüllen.

### <a name="can-i-use-a-single-amqp-connection-to-send-and-receive-from-multiple-event-hubs"></a>Kann ich eine einzelne AMQP-Verbindung zum Senden und Empfangen von mehreren Event Hubs verwenden?
Ja, solange sich alle Event Hubs im gleichen Namespace befinden.

### <a name="what-is-the-maximum-retention-period-for-events"></a>Was ist die maximale Beibehaltungsdauer für Ereignisse?
Die Standard-Ebene für Event Hubs unterstützt derzeit einen maximalen Aufbewahrungszeitraum von 7 Tagen. Beachten Sie, dass Event Hubs nicht als dauerhafter Datenspeicher vorgesehen sind. Beibehaltungsdauern größer als 24 Stunden sind für Szenarien vorgesehen, in denen es praktisch ist, einen Stream von Ereignissen in die gleichen Systeme wiederzugeben, wenn Sie beispielsweise ein neues Computerlernmodell für vorhandene Daten trainieren oder überprüfen wollen. Wenn Sie Nachrichten länger als 7 Tage aufbewahren möchten, werden die Daten durch Aktivieren von [Event Hubs Capture](https://docs.microsoft.com/azure/event-hubs/event-hubs-archive-overview) in Ihrem Event Hub von Ihrem Event Hub per Pull in den von Ihnen gewünschten Speicher übertragen. Abhängig von Ihrer Durchsatzeinheit entstehen durch das aktivieren von „Capture“ Kosten.

### <a name="where-is-azure-event-hubs-available"></a>Wo steht Azure Event Hubs zur Verfügung?
Azure Event Hubs ist in allen unterstützten Azure-Regionen verfügbar. Eine Liste finden Sie auf der Seite [Azure-Regionen](https://azure.microsoft.com/regions/).  

## <a name="best-practices"></a>Bewährte Methoden

### <a name="how-many-partitions-do-i-need"></a>Wie viele Partitionen benötige ich?
Denken Sie daran, dass die Partitionszahl auf einem Event Hub nach der Installation nicht geändert werden kann. Vor diesem Hintergrund ist es wichtig, dass Sie vor Beginn genau überlegen, wie viele Partitionen Sie benötigen. 

Event Hubs ist für einen einzelnen Partitionsleser pro Verbrauchergruppe ausgelegt. In den meisten Fällen reicht die Standardeinstellung von vier Partitionen aus. Wenn Sie Ihre Ereignisverarbeitung skalieren möchten, möchten Sie vielleicht die Möglichkeit haben, das Hinzufügen weiterer Partitionen zu erwägen. Es gibt keine bestimmte Durchsatzbegrenzung für eine Partition, aber der aggregierte Durchsatz in Ihrem Namespace ist durch die Anzahl der Durchsatzeinheiten beschränkt. Wenn Sie die Anzahl der Durchsatzeinheiten in Ihrem Namespace erhöhen, wünschen Sie vielleicht zusätzliche Partitionen, um gleichzeitigen Lesern zu ermöglichen, ihren eigenen maximalen Durchsatz zu erzielen.

Aber wenn Sie über ein Modell verfügen, in dem die Anwendung eine bestimmte Partition bevorzugt, ist eine höhere Anzahl von Partitionen für Sie nicht unbedingt von Vorteil. Weitere Informationen hierzu finden Sie unter [Verfügbarkeit und Konsistenz](event-hubs-availability-and-consistency.md).

## <a name="pricing"></a>Preise

### <a name="where-can-i-find-more-pricing-information"></a>Wo finde ich weitere Preisinformationen?
Umfassende Informationen zu den Preisen von Event Hubs finden Sie unter [Event Hubs – Preisübersicht](https://azure.microsoft.com/pricing/details/event-hubs/).

### <a name="is-there-a-charge-for-retaining-event-hubs-events-for-more-than-24-hours"></a>Gibt es eine Gebühr für die Beibehaltung von Event Hubs-Ereignissen für mehr als 24 Stunden?
Der Standard-Tarif für Event Hubs erlaubt eine Aufbewahrung von Nachrichten länger als 24 Stunden, maximal 7 Tage. Wenn die Größe der Gesamtanzahl der gespeicherten Ereignisse das Speicherkontingent für die Anzahl der ausgewählten Durchsatzeinheiten (84 GB pro Durchsatzeinheit) überschreitet, wird die überschrittene Größe zur veröffentlichten Rate für den Azure Blob Storage in Rechnung gestellt. Das Speicherkontingent in allen Durchsatzeinheiten deckt alle Speicherkosten für die Aufbewahrungsdauer von 24 Stunden (Standard), selbst, wenn die Durchsatzeinheit bis zum maximal erlaubten Eingang verbraucht ist.

### <a name="how-is-the-event-hubs-storage-size-calculated-and-charged"></a>Wie wird die Event Hubs-Speichergröße berechnet und in Rechnung gestellt?
Die Gesamtgröße aller gespeicherten Ereignisse, einschließlich des gesamten internen Mehraufwands für Ereignisheader oder Speicherstrukturen auf Datenträgern in allen Event Hubs, wird im Laufe des Tages gemessen. Am Ende des Tages wird die maximale Speichergröße berechnet. Das tägliche Speicherkontingent wird auf Grundlage der Mindestanzahl der Durchsatzeinheiten berechnet, die im Laufe des Tages ausgewählt wurden (jede Durchsatzeinheit bietet ein Kontingent von 84 GB). Wenn die Gesamtgröße das berechnete tägliche Speicherkontingent überschreitet, wird der überschüssige Speicher zu Azure-Blob-Speichersätzen in Rechnung gestellt (auf der **lokal redundanter Speicher** Rate).

### <a name="how-are-event-hubs-ingress-events-calculated"></a>Wie werden Eingangsereignisse von Event Hubs berechnet?
Jedes an einen Event Hub gesendete Ereignis wird als abrechenbare Nachricht gezählt. Ein *Eingangsereignis* ist als eine Dateneinheit definiert, die kleiner oder gleich 64 KB ist. Jedes Ereignis, das kleiner als oder gleich 64 KB ist, gilt als kostenpflichtig. Wenn das Ereignis größer als 64 KB ist, wird die Anzahl der abzurechnenden Ereignisse gemäß der Ereignisgröße in Vielfachen von 64 KB berechnet. Beispielsweise wird ein an ein Event Hub gesendetes 8-KB-Ereignis als ein Ereignis abgerechnet, aber eine Nachricht in der Größe von 96 KB, die an den Event Hub gesendet wird, wird als zwei Ereignisse in Rechnung gestellt.

Von einem Event Hub genutzte Ereignisse sowie Verwaltungsvorgänge und Kontrollaufrufe wie etwa Checkpoints werden nicht als abzurechnende Eingangsereignisse gezählt, sondern bis zur erlaubten Durchsatzeinheit angesammelt.

### <a name="do-brokered-connection-charges-apply-to-event-hubs"></a>Gelten vermittelte Verbindungsgebühren für Event Hubs?
Verbindungsgebühren fallen nur an, wenn das AMQP-Protokoll verwendet wird. Es gibt keine Verbindungsgebühren für das Senden von Ereignissen über HTTP, unabhängig von der Anzahl der sendenden Systeme oder Geräte. Wenn Sie AMQP verwenden möchten (z.B. um effizienteres Ereignisstreaming zu erreichen oder bidirektionale Kommunikation in Befehls- und Steuerungsszenarios von IoT zu ermöglichen), lesen Sie sich bitte die Seite [Event Hubs Preise](https://azure.microsoft.com/pricing/details/event-hubs/) durch, um Näheres darüber zu erfahren, wie viele Verbindungen in jeder Dienstebene enthalten sind.

### <a name="how-is-event-hubs-capture-billed"></a>Wie erfolgt die Abrechnung für Event Hubs Capture?
Capture wird aktiviert, wenn für eine Event Hub-Instanz im Namespace die Capture-Option aktiviert wird. Event Hubs Capture wird stundenweise pro erworbener Durchsatzeinheit abgerechnet. Wenn die Anzahl von Durchsatzeinheiten erhöht oder verringert wird, werden diese Änderungen bei der Abrechnung für Event Hubs Capture in Schritten ganzer Stunden übernommen. Weitere Informationen zur Abrechnung von Event Hubs Capture finden Sie unter [Event Hubs Preise](https://azure.microsoft.com/pricing/details/event-hubs/).

### <a name="will-i-be-billed-for-the-storage-account-i-select-for-event-hubs-capture"></a>Werden Speichergebühren für das für Event Hubs Capture ausgewählte Speicherkonto abgerechnet?
Capture verwendet ein Speicherkonto, das Sie angeben, wenn es auf einem Event Hub aktiviert ist. Da es sich um Ihr Speicherkonto handelt, werden alle Änderungen an dieser Konfiguration über Ihr Azure-Abonnement abgerechnet.

## <a name="quotas"></a>Kontingente

### <a name="are-there-any-quotas-associated-with-event-hubs"></a>Sind Event Hubs bestimmte Kontingente zugeordnet?
Eine Liste aller Kontingente für Event Hubs finden Sie unter [Event Hubs-Kontingente](event-hubs-quotas.md).

## <a name="troubleshooting"></a>Problembehandlung

### <a name="what-are-some-of-the-exceptions-generated-by-event-hubs-and-their-suggested-actions"></a>Welche Beispiele gibt es für Ausnahmen, die von Event Hubs generiert werden, und die vorgeschlagenen Aktionen?
Eine Liste der möglichen Event Hubs-Ausnahmen finden Sie unter [Event Hubs-Messagingausnahmen](event-hubs-messaging-exceptions.md).

### <a name="diagnostic-logs"></a>Diagnoseprotokolle
Event Hubs unterstützt zwei Arten von [Diagnoseprotokollen](event-hubs-diagnostic-logs.md): Erfassungsfehlerprotokolle und Betriebsprotokolle. Beide werden im JSON-Format dargestellt und können über das Azure-Portal aktiviert werden.

### <a name="support-and-sla"></a>Support und SLA
Technischer Support für Event Hubs steht über die [Communityforen](https://social.msdn.microsoft.com/forums/azure/home)bereit. Der Support für die Abrechnungs- und Abonnementverwaltung wird kostenlos bereitgestellt.

Weitere Informationen zu unserem SLA finden Sie auf der Seite [Vereinbarungen zum Servicelevel](https://azure.microsoft.com/support/legal/sla/) .

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen zu Event Hubs finden Sie unter den folgenden Links:

* [Übersicht über Event Hubs](event-hubs-what-is-event-hubs.md)
* [Erstellen eines Event Hubs](event-hubs-create.md)

