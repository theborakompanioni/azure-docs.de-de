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
	ms.date="08/17/2016"
	ms.author="sethm"/>

# Was ist Azure Event Hubs?

Azure Event Hubs ist ein hochgradig skalierbarer Dateneingangsdienst, der Millionen von Ereignissen pro Sekunde erfassen kann. Auf diese Weise können Sie riesige Datenmengen verarbeiten und analysieren, die von vernetzten Geräten und Anwendungen erzeugt werden. Event Hubs fungiert als „Eingangstür“ für eine Ereignispipeline. Nach der Erfassung in Event Hubs können Sie Daten mit einem beliebigen Echtzeit-Analyseanbieter oder mit Batchverarbeitungs-/Speicheradaptern umwandeln und speichern. Event Hubs entkoppelt die Erzeugung eines Datenstroms von Ereignissen von der Nutzung dieser Ereignisse, sodass Ereignisconsumer nach einem eigenen Zeitplan auf Ereignisse zugreifen können. Weitere Informationen und technische Details finden Sie in der [Übersicht über Event Hubs](event-hubs-overview.md).

## Funktionen von Event Hubs

Event Hubs ist ein Dienst zur Ereignisverarbeitung, der große Mengen an Ereignis- und Telemetriedaten in der Cloud erfassen kann und gleichzeitig eine niedrige Latenz und hohe Zuverlässigkeit bietet. Dieser Dienst ist besonders nützlich für:

- Anwendungsinstrumentation
- Benutzerumgebung oder Workflowverarbeitung
- Internet der Dinge (IoT)-Szenarien

Weitere Beispiele für die Funktionen von Event Hubs sind Verhaltensanalysen in mobilen Apps, Verkehrsinformationen von Webfarmen, Ereignisaufzeichnungen in Konsolenspielen oder von Industriemaschinen oder verbundenen Fahrzeugen erfasste Telemetriedaten.

## Nächste Schritte

Ausführliche Informationen zu Event Hubs finden Sie unter den folgenden Themen:

- [Übersicht über Event Hubs](event-hubs-overview.md)
- [Programmierleitfaden für Event Hubs](event-hubs-programming-guide.md)
- [Event Hubs Verfügbarkeit und Support – häufig gestellte Fragen](event-hubs-availability-and-support-faq.md)
- Erste Schritte mit einem [Lernprogramm zu Event Hubs][].
- Eine vollständige [Beispielanwendung zur Verwendung von Event Hubs][]

[Lernprogramm zu Event Hubs]: event-hubs-csharp-ephcs-getstarted.md
[Beispielanwendung zur Verwendung von Event Hubs]: https://code.msdn.microsoft.com/Service-Bus-Event-Hub-286fd097

<!---HONumber=AcomDC_0907_2016-->