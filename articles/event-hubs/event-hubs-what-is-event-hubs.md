---
title: Definition und Vorteile von Azure Event Hubs | Microsoft-Dokumentation
description: "Übersicht und Einführung in Azure Event Hubs – Telemetrieeingang auf Cloudebene von Websites, Apps und Geräten"
services: event-hubs
documentationcenter: .net
author: sethmanheim
manager: timlt
editor: 
ms.assetid: 
ms.service: event-hubs
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/28/2017
ms.author: sethm; babanisa
ms.translationtype: Human Translation
ms.sourcegitcommit: 857267f46f6a2d545fc402ebf3a12f21c62ecd21
ms.openlocfilehash: 1a6bf0a0352e6d9e3a22586ac825558d12e1307a
ms.contentlocale: de-de
ms.lasthandoff: 06/28/2017


---
<a id="what-is-event-hubs" class="xliff"></a>

# Was ist Event Hubs?

Azure Event Hubs ist eine hochgradig skalierbare Datenstreamingplattform und ein Dienst zur Erfassung von Ereignissen, der bzw. die Millionen Ereignisse pro Sekunde empfangen und verarbeiten kann. Event Hubs kann Ereignisse, Daten oder Telemetriedaten, die von verteilter Software und verteilten Geräten erzeugt wurden, verarbeiten und speichern. An einen Event Hub gesendete Daten können transformiert und mit einem beliebigen Echtzeitanalyse-Anbieter oder Batchverarbeitungs-/Speicheradapter gespeichert werden. Durch die Möglichkeit, [Funktionen zum Veröffentlichen/Abonnieren](https://msdn.microsoft.com/library/aa560414.aspx) mit niedriger Latenz und umfangreicher Skalierung anzubieten, fungiert Event Hubs als Einstiegspunkt für Big Data.

<a id="why-use-event-hubs" class="xliff"></a>

## Vorteile von Event Hubs

Die Ereignis- und Telemetrieverarbeitungsfunktionen von Event Hubs machen ihn besonders hilfreich für:

* Anwendungsinstrumentation
* Benutzerumgebung oder Workflowverarbeitung
* Internet der Dinge (IoT)-Szenarien

Beispielsweise bietet Event Hubs die Nachverfolgung von Verhaltensweisen in mobilen Apps, Informationen zum Datenverkehr von Webfarmen, Ereigniserfassung in Konsolenspielen oder von Industriemaschinen, vernetzten Fahrzeugen oder anderen Geräten erfasste Telemetriedaten.

<a id="azure-event-hubs-overview" class="xliff"></a>

## Übersicht über Event Hubs

In Lösungsarchitekturen übernimmt Event Hubs im Allgemeinen eine Funktion als „Eingangstür“ für eine Ereignispipeline, häufig als *Ereigniserfassung* bezeichnet. Ein Ereigniserfasser ist eine Komponente oder ein Dienst zwischen Ereignisherausgeber und Ereignisverarbeitung zum Entkoppeln der Erzeugung eines Ereignisstreams von der Verarbeitung dieser Ereignisse. Diese Architektur ist in der folgenden Abbildung dargestellt:

![Event Hubs](./media/event-hubs-what-is-event-hubs/event_hubs_full_pipeline.png)

Event Hubs bietet eine Funktion zur Abwicklung von Nachrichtendatenströmen, verfügt jedoch über Eigenschaften, die sich von herkömmlichen Diensten zur Unternehmenskommunikation unterscheiden. Event Hubs-Funktionen werden für Szenarien mit hohem Durchsatz und Ereignisverarbeitung erstellt. Infolgedessen unterscheidet sich Event Hubs von [Azure Service Bus](https://azure.microsoft.com/services/service-bus/)-Messaging. Zudem werden von Event Hubs einige der Funktionen, die für [Service Bus-Messaging-Entitäten](/azure/service-bus-messaging/) wie Themen verfügbar sind, nicht implementiert.

<a id="event-hubs-features" class="xliff"></a>

## Event Hubs-Funktionen

Event Hubs besteht aus folgenden Hauptelementen:

- [**Ereignisersteller/-herausgeber**](event-hubs-features.md#event-publishers): Eine Entität, die Daten an einen Event Hub sendet. Ein Ereignis wird über AMQP 1.0 oder HTTPS veröffentlicht.
- [**Erfassung**](event-hubs-features.md#capture): Ermöglicht Ihnen die Erfassung von Event Hubs-Streamingdaten und ihre Speicherung in einem Azure Blob-Speicherkonto.
- [**Partitionen**](event-hubs-features.md#partitions): Ermöglicht jedem Consumer, nur eine bestimmte Teilmenge oder Partition des Ereignisdatenstroms zu lesen.
- [**SAS-Token**](event-hubs-features.md#sas-tokens): Identifizieren und Authentifizieren den Ereignisherausgeber.
- [**Ereignisconsumer**](event-hubs-features.md#event-consumers): Eine Entität, die Ereignisdaten von einem Event Hub liest. Event Hubs-Consumer stellen Verbindungen über AMQP 1.0 her. 
- [**Consumergruppen**](event-hubs-features.md#consumer-groups): Stellen eine separate Ansicht vom Ereignisdatenstrom für jede aus mehreren Consumeranwendungen bestehende Gruppe bereit, wodurch diese Consumer unabhängig Vorgänge durchführen können.
- [**Durchsatzeinheiten**](event-hubs-features.md#capacity): Werden vorab als Kapazitätseinheiten erworben. Eine einzelne Partition weist eine maximale Skalierung von einer Durchsatzeinheit auf.

Technische Informationen zu diesen und anderen Event Hubs-Funktionen finden Sie im [Überblick über Event Hubs-Funktionen](event-hubs-features.md). 

<a id="next-steps" class="xliff"></a>

## Nächste Schritte

Ausführliche Informationen zu Event Hubs-Preisen finden Sie unter [Event Hubs-Preise](https://azure.microsoft.com/pricing/details/event-hubs/).

Weitere Informationen zu Event Hubs erhalten Sie unter den folgenden Links:

* Erste Schritte mit einem [Event Hubs-Tutorial](event-hubs-dotnet-standard-getstarted-send.md)
* [Event Hubs – häufig gestellte Fragen](event-hubs-faq.md)
* [Sample applications that use Event Hubs (Beispielanwendung mit Verwendung von Event Hubs)](https://github.com/Azure/azure-event-hubs/tree/master/samples)
 
 


