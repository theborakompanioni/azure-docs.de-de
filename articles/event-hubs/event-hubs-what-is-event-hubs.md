---
title: Was ist Azure Event Hubs? | Microsoft Docs
description: "Übersicht über und Beschreibung der Azure Event Hubs"
services: event-hubs
documentationcenter: .net
author: sethmanheim
manager: timlt
editor: 
ms.assetid: 4391d750-5bbe-456d-9091-b416127bc754
ms.service: event-hubs
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 11/30/2016
ms.author: sethm
translationtype: Human Translation
ms.sourcegitcommit: 05ca343cfdfc602759eb3ea30a7186a0bb47bd74
ms.openlocfilehash: eddc3e7c4914936a8a83a042dc0f7d528b91f059


---
# <a name="what-is-azure-event-hubs"></a>Was ist Azure Event Hubs?
Azure Event Hubs ist ein hochgradig skalierbarer Dateneingangsdienst, der Millionen von Ereignissen pro Sekunde erfassen kann. Auf diese Weise können Sie riesige Datenmengen verarbeiten und analysieren, die von vernetzten Geräten und Anwendungen erzeugt werden. Event Hubs fungiert als „Eingangstür“ für eine Ereignispipeline. Nach der Erfassung in Event Hubs können Sie Daten mit einem beliebigen Echtzeit-Analyseanbieter oder mit Batchverarbeitungs-/Speicheradaptern umwandeln und speichern. Event Hubs entkoppelt die Erzeugung eines Datenstroms von Ereignissen von der Nutzung dieser Ereignisse, sodass  Ereignisconsumer nach einem eigenen Zeitplan auf Ereignisse zugreifen können. Weitere Informationen und technische Details finden Sie in der [Übersicht über Event Hubs](event-hubs-overview.md).

## <a name="event-hubs-capabilities"></a>Funktionen von Event Hubs
Event Hubs ist ein Dienst zur Ereignisverarbeitung, der große Mengen an Ereignis- und Telemetriedaten in der Cloud erfassen kann und gleichzeitig eine niedrige Latenz und hohe Zuverlässigkeit bietet. Dieser Dienst ist besonders nützlich für:

* Anwendungsinstrumentation
* Benutzerumgebung oder Workflowverarbeitung
* Internet der Dinge (IoT)-Szenarien

Weitere Beispiele für die Funktionen von Event Hubs sind Verhaltensanalysen in mobilen Apps, Verkehrsinformationen von Webfarmen, Ereignisaufzeichnungen in Konsolenspielen oder von Industriemaschinen oder verbundenen Fahrzeugen erfasste Telemetriedaten.

## <a name="next-steps"></a>Nächste Schritte
Ausführliche Informationen zu Event Hubs finden Sie unter den folgenden Themen:

* [Übersicht über Event Hubs](event-hubs-overview.md)
* [Programmierleitfaden für Event Hubs](event-hubs-programming-guide.md)
* [Event Hubs Verfügbarkeit und Support – häufig gestellte Fragen](event-hubs-availability-and-support-faq.md)
* Beginnen Sie mit einem [Event Hubs-Tutorial][Event Hubs tutorial].
* Eine vollständige [Beispielanwendung mit Verwendung von Event Hubs][sample application that uses Event Hubs]

[Event Hubs tutorial]: event-hubs-csharp-ephcs-getstarted.md
[sample application that uses Event Hubs]: https://code.msdn.microsoft.com/Service-Bus-Event-Hub-286fd097



<!--HONumber=Dec16_HO1-->


