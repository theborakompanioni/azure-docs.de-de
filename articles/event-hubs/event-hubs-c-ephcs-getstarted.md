<properties
	pageTitle="Erste Schritte mit Event Hubs in C und C# | Microsoft Azure"
	description="Befolgen Sie dieses Tutorial für die ersten Schritte bei der Verwendung von Azure Event Hubs für das Senden von Ereignissen in C und das Empfangen in C# mithilfe von EventProcessorHost."
	services="event-hubs"
	documentationCenter=""
	authors="jtaubensee"
	manager="timlt"
	editor=""/>

<tags
	ms.service="event-hubs"
	ms.workload="na"
	ms.tgt_pltfrm="c"
	ms.devlang="csharp"
	ms.topic="article"
	ms.date="08/16/2016"
	ms.author="jotaub;sethm"/>

# Erste Schritte mit Event Hubs

[AZURE.INCLUDE [service-bus-selector-get-started](../../includes/service-bus-selector-get-started.md)]

## Einführung

Event Hubs sind ein hochgradig skalierbares Aufnahmesystem, das Millionen von Ereignissen pro Sekunde aufnehmen kann, wodurch eine Anwendung die Möglichkeit erhält, die von Ihren verbundenen Geräten und Anwendungen erzeugten immensen Datenmengen zu verarbeiten und zu analysieren. Nach der Erfassung in Event Hubs können Sie Daten über einen beliebigen Echtzeit-Analyseanbieter oder ein Speichercluster transformieren und speichern.

Weitere Informationen finden Sie unter [Übersicht über Event Hubs][].

In diesem Tutorial erfahren Sie, wie Nachrichten an einen Event Hub mithilfe einer Konsolenanwendung in C aufgenommen werden können und wie Sie diese parallel mit der C#-[Ereignisprozessorhost-Bibliothek][] abrufen.

Für dieses Tutorial benötigen Sie Folgendes:

+ Eine C-Entwicklungsumgebung. In diesem Tutorial wird vom GCC-Stack auf einem [virtuellen Linux-Computer mit Azure](../virtual-machines/virtual-machines-linux-quick-create-cli.md) mit Ubuntu 14.04 ausgegangen. Anweisungen für andere Umgebungen werden in externen Links bereitgestellt.

+ Microsoft Visual Studio Express für Windows

+ Ein aktives Azure-Konto. Wenn Sie noch kein Konto haben, können Sie in nur wenigen Minuten ein kostenloses Testkonto erstellen. Ausführliche Informationen finden Sie unter [Einen Monat kostenlos testen](https://azure.microsoft.com/pricing/free-trial/).

[AZURE.INCLUDE [event-hubs-create-event-hub](../../includes/event-hubs-create-event-hub.md)]

[AZURE.INCLUDE [service-bus-event-hubs-get-started-send-c](../../includes/service-bus-event-hubs-get-started-send-c.md)]

[AZURE.INCLUDE [service-bus-event-hubs-get-started-receive-ephcs](../../includes/service-bus-event-hubs-get-started-receive-ephcs.md)]

## Ausführen der Anwendungen

Sie können jetzt die Anwendung ausführen.

1.	Führen Sie in Visual Studio das Projekt **Receiver** aus, und warten Sie dann, bis die Empfänger für alle Partitionen gestartet wurden.

	![][21]

2.	Führen Sie das Programm **Sender** aus, und sehen Sie sich die im Empfängerfenster angezeigten Ereignisse an.

	![][24]

## Nächste Schritte

Nachdem Sie eine funktionierende Anwendung erstellt haben, die einen Ereignis-Hub erstellt und Daten sendet und empfängt, können Sie mit den folgenden Szenarios fortfahren:

- Eine vollständige [Beispielanwendung mit Verwendung von Ereignis-Hubs][].
- Das Beispiel [Horizontales Hochskalieren der Ereignisverarbeitung mit Event Hubs][]
- Eine [Messaginglösung mit Warteschlange][] unter Verwendung von Service Bus-Warteschlangen.
- [Übersicht über Event Hubs][]

<!-- Images. -->
[21]: ./media/event-hubs-c-ephcs-getstarted/run-csharp-ephcs1.png
[24]: ./media/event-hubs-c-ephcs-getstarted/receive-eph-c.png

<!-- Links -->
[klassischen Azure-Portal]: https://manage.windowsazure.com/
[Ereignisprozessorhost-Bibliothek]: https://www.nuget.org/packages/Microsoft.Azure.ServiceBus.EventProcessorHost
[Übersicht über Event Hubs]: event-hubs-overview.md
[Übersicht über Event Hubs]: event-hubs-overview.md
[Beispielanwendung mit Verwendung von Ereignis-Hubs]: https://code.msdn.microsoft.com/Service-Bus-Event-Hub-286fd097
[Horizontales Hochskalieren der Ereignisverarbeitung mit Event Hubs]: https://code.msdn.microsoft.com/Service-Bus-Event-Hub-45f43fc3
[Messaginglösung mit Warteschlange]: ../service-bus/service-bus-dotnet-multi-tier-app-using-service-bus-queues.md

<!---HONumber=AcomDC_0928_2016-->