<properties
	pageTitle="Was ist Azure Event Hubs? | Microsoft Azure"
	description="Übersicht über und Beschreibung der Azure Event Hubs"
	services="event-hubs"
	documentationCenter=".net"
	authors="sethmanheim"
	manager="timlt"
	editor=""/>

<tags
	ms.service="event-hubs"
	ms.workload="na"
	ms.tgt_pltfrm="na"
	ms.devlang="na"
	ms.topic="get-started-article"
	ms.date="04/12/2016"
	ms.author="sethm;nberdy"/>

# Was ist Azure Event Hubs?

Azure Event Hubs ist ein hochgradig skalierbarer Dateneingangsdienst, der Millionen von Ereignissen pro Sekunde erfassen kann. Auf diese Weise können Sie riesige Datenmengen verarbeiten und analysieren, die von vernetzten Geräten und Anwendungen erzeugt werden. Event Hubs fungiert als „Eingangstür“ für eine Ereignispipeline. Nach der Erfassung in Event Hubs können Sie Daten mit einem beliebigen Echtzeit-Analyseanbieter oder mit Batchverarbeitungs-/Speicheradaptern umwandeln und speichern. Event Hubs entkoppelt die Erzeugung eines Datenstroms von Ereignissen von der Nutzung dieser Ereignisse, sodass Ereignisconsumer nach einem eigenen Zeitplan auf Ereignisse zugreifen können.

## Funktionen von Event Hubs

Event Hubs ist ein Dienst zur Ereignisverarbeitung, der riesige Mengen an Ereignis- und Telemetriedaten in der Cloud erfassen kann und gleichzeitig eine niedrige Latenz und hohe Zuverlässigkeit bietet. Dieser Dienst ist besonders nützlich für:

- Anwendungsinstrumentation
- Benutzerumgebung oder Workflowverarbeitung
- Internet der Dinge (IoT)-Szenarien

Weitere Beispiele für die Funktionen von Event Hubs sind Verhaltensanalysen in mobilen Apps, Verkehrsinformationen von Webfarmen, Ereignisaufzeichnungen in Konsolenspielen oder von Industriemaschinen oder verbundenen Fahrzeugen erfasste Telemetriedaten.

Im Gegensatz zu [Service Bus-Warteschlangen und -Themen](../service-bus/service-bus-messaging-overview.md) konzentriert sich Event Hubs auf das Bereitstellen von skalierter Verarbeitung von Nachrichtendatenströmen. Event Hubs-Funktionen unterscheiden sich von Service Bus-Themen beispielsweise darin, dass sie für Szenarios mit hohem Durchsatz und hoher Ereignisverarbeitung optimiert sind. Daher sind in Event Hubs einige der Nachrichtenfunktionen, die für [Themen](../service-bus/service-bus-fundamentals-hybrid-solutions.md#topics) verfügbar sind, nicht direkt implementiert. Wenn Sie diese Funktionen benötigen, sind Themen weiterhin die optimale Wahl.

## Nächste Schritte

Ausführliche Informationen zu Event Hubs finden Sie unter den folgenden Themen.

- [Übersicht über Event Hubs](event-hubs-overview.md)
- [Programmierleitfaden für Event Hubs](event-hubs-programming-guide.md)
- [Event Hubs Verfügbarkeit und Support – häufig gestellte Fragen](event-hubs-availability-and-support-faq.md)
- Erste Schritte mit einem [Lernprogramm zu Event Hubs][].
- Eine vollständige [Beispielanwendung zur Verwendung von Event Hubs][]

[Lernprogramm zu Event Hubs]: event-hubs-csharp-ephcs-getstarted.md
[Beispielanwendung zur Verwendung von Event Hubs]: https://code.msdn.microsoft.com/Service-Bus-Event-Hub-286fd097

<!---HONumber=AcomDC_0413_2016-->