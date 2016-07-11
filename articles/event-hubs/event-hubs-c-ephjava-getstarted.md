<properties
	pageTitle="Erste Schritte mit Event Hubs in C | Microsoft Azure"
	description="Befolgen Sie dieses Tutorial für die ersten Schritte bei der Verwendung von Azure Event Hubs und für das Senden von Ereignissen in C und das Empfangen in Java mithilfe von EventProcessorHost."
	services="event-hubs"
	documentationCenter=""
	authors="fsautomata"
	manager="timlt"
	editor=""/>

<tags
	ms.service="event-hubs"
	ms.workload="na"
	ms.tgt_pltfrm="c"
	ms.devlang="csharp"
	ms.topic="article"
	ms.date="06/16/2016"
	ms.author="jtaubensee"/>

# Erste Schritte mit Event Hubs

[AZURE.INCLUDE [service-bus-selector-get-started](../../includes/service-bus-selector-get-started.md)]

## Einführung

Event Hubs sind ein hochgradig skalierbares Aufnahmesystem, das Millionen von Ereignissen pro Sekunde aufnehmen kann, wodurch eine Anwendung die Möglichkeit erhält, die von Ihren verbundenen Geräten und Anwendungen erzeugten immensen Datenmengen zu verarbeiten und zu analysieren. Nach der Erfassung in Event Hubs können Sie Daten über einen beliebigen Echtzeit-Analyseanbieter oder ein Speichercluster transformieren und speichern.

Weitere Informationen finden Sie unter [Übersicht über Event Hubs][].

In diesem Lernprogramm erfahren Sie, wie Nachrichten an einen Event Hub mithilfe einer Konsolenanwendung in C aufgenommen werden können und wie Sie diese parallel mit der C#-[Ereignisprozessorhost-Bibliothek][] abrufen.

Zum Durchführen dieses Lernprogramms benötigen Sie Folgendes:

+ Eine C-Entwicklungsumgebung. In diesem Lernprogramm wird vom GCC-Stack auf einem [virtuellen Linux-Computer mit Azure](../virtual-machines/virtual-machines-linux-quick-create-cli.md) mit Ubuntu 14.04 ausgegangen. Anweisungen für andere Umgebungen werden in externen Links bereitgestellt.

+ Microsoft Visual Studio Express für Windows

+ Ein aktives Azure-Konto. <br/>Wenn Sie noch kein Konto haben, können Sie in nur wenigen Minuten ein kostenloses Testkonto erstellen. Ausführliche Informationen finden Sie unter <a href="http://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fde-DE%2Fdevelop%2Fmobile%2Ftutorials%2Fget-started%2F" target="_blank">Kostenlose Azure-Testversion</a>.

[AZURE.INCLUDE [event-hubs-create-event-hub](../../includes/event-hubs-create-event-hub.md)]

[AZURE.INCLUDE [service-bus-event-hubs-get-started-send-c](../../includes/service-bus-event-hubs-get-started-send-c.md)]

[AZURE.INCLUDE [service-bus-event-hubs-get-started-receive-ephjava](../../includes/service-bus-event-hubs-get-started-receive-ephjava.md)]

## Ausführen der Anwendungen

Sie können jetzt die Anwendung ausführen.

1.	Führen Sie das Projekt **Receiver** aus.

	![][21]

2.	Führen Sie das Programm **sender** aus, und sehen Sie sich die im Empfängerfenster angezeigten Ereignisse an.

	![][24]

## Nächste Schritte

Nachdem Sie eine funktionierende Anwendung erstellt haben, die einen Ereignis-Hub erstellt und Daten sendet und empfängt, können Sie mit den folgenden Szenarios fortfahren:

- Eine vollständige [Beispielanwendung mit Verwendung von Ereignis-Hubs][].
- Das Beispiel [Horizontales Hochskalieren der Ereignisverarbeitung mit Event Hubs][]
- Eine [Messaginglösung mit Warteschlange][] unter Verwendung von Service Bus-Warteschlangen.
- [Übersicht über Event Hubs][]

<!-- Images. -->
[21]: ./media/event-hubs-c-ephjava-getstarted/ephjava.png
[24]: ./media/event-hubs-c-ephjava-getstarted/receive-eph-c.png

<!-- Links -->
[Azure classic portal]: https://manage.windowsazure.com/
[Ereignisprozessorhost-Bibliothek]: https://www.nuget.org/packages/Microsoft.Azure.ServiceBus.EventProcessorHost
[Übersicht über Event Hubs]: event-hubs-overview.md
[Übersicht über Event Hubs]: event-hubs-overview.md
[Beispielanwendung mit Verwendung von Ereignis-Hubs]: https://code.msdn.microsoft.com/Service-Bus-Event-Hub-286fd097
[Horizontales Hochskalieren der Ereignisverarbeitung mit Event Hubs]: https://code.msdn.microsoft.com/Service-Bus-Event-Hub-45f43fc3
[Messaginglösung mit Warteschlange]: ../service-bus/service-bus-dotnet-multi-tier-app-using-service-bus-queues.md

<!---HONumber=AcomDC_0629_2016-->