<properties
	pageTitle="Erste Schritte mit Event Hubs in C# | Microsoft Azure"
	description="Befolgen Sie dieses Lernprogramm für die ersten Schritte bei der Verwendung von Azure Event Hubs mit C# und EventProcessorHost."
	services="event-hubs"
	documentationCenter=""
	authors="jtaubensee"
	manager="timlt"
	editor=""/>

<tags
	ms.service="event-hubs"
	ms.workload="na"
	ms.tgt_pltfrm="na"
	ms.devlang="na"
	ms.topic="hero-article"
	ms.date="09/02/2016"
	ms.author="jotaub;sethm"/>

# Erste Schritte mit Event Hubs

[AZURE.INCLUDE [service-bus-selector-get-started](../../includes/service-bus-selector-get-started.md)]

## Einführung

Event Hubs ist ein Dienst, der große Mengen von Ereignisdaten (Telemetriedaten) von verbundenen Geräten und Anwendungen verarbeiten kann. Nach dem Sammeln von Daten auf Ereignis-Hubs können die Daten mithilfe eines Speicherclusters gespeichert oder mit einem Echtzeitanalyse-Anbieter transformiert werden. Diese umfangreiche Ereignissammlung und -verarbeitung ist eine wichtige Komponente moderner Anwendungsarchitekturen. Hierzu zählt auch das Internet der Dinge (Internet of Things, IoT).

Dieses Tutorial zeigt, wie Sie mit dem klassischen Azure-Portal einen Event Hub erstellen. Sie erfahren in diesem Lernprogramm außerdem, wie Nachrichten an einen Event Hub mithilfe einer in C# geschriebenen Konsolenanwendung aufgenommen werden können und wie Sie diese parallel mit der C#-[Ereignisprozessorhost][]-Bibliothek abrufen.

Für dieses Tutorial benötigen Sie Folgendes:

+ [Microsoft Visual Studio](http://visualstudio.com)

+ Ein aktives Azure-Konto. Falls Sie nicht über ein Konto verfügen, können Sie in nur wenigen Minuten ein kostenloses Konto erstellen. Ausführliche Informationen finden Sie unter [Einen Monat kostenlos testen](https://azure.microsoft.com/free/).

[AZURE.INCLUDE [event-hubs-create-event-hub](../../includes/event-hubs-create-event-hub.md)]

[AZURE.INCLUDE [service-bus-event-hubs-get-started-send-csharp](../../includes/service-bus-event-hubs-get-started-send-csharp.md)]

[AZURE.INCLUDE [service-bus-event-hubs-get-started-receive-ephcs](../../includes/service-bus-event-hubs-get-started-receive-ephcs.md)]

## Ausführen der Anwendungen

Sie können jetzt die Anwendung ausführen.

1. Öffnen Sie in Visual Studio das zuvor erstellte Projekt **Receiver**.

2. Klicken Sie mit der rechten Maustaste auf die Projektmappe **Receiver**, und klicken Sie dann auf **Hinzufügen** und **Vorhandenes Projekt**.
 
3. Suchen Sie nach der vorhandenen Datei „Sender.csproj“, und doppelklicken Sie dann auf die Datei, um sie der Projektmappe hinzuzufügen.
 
4. Klicken Sie erneut mit der rechten Maustaste auf die Projektmappe **Receiver**, und klicken Sie dann auf **Eigenschaften**. Die Eigenschaftenseite für **Receiver** wird angezeigt.

5. Klicken Sie auf **Startprojekt** und dann auf die Schaltfläche **Mehrere Startprojekte**. Legen Sie das Feld **Aktion** für das Projekt **Receiver** und **Sender** jeweils auf **Start** fest.

	![][19]

6. Klicken Sie auf **Projektabhängigkeiten**. Klicken Sie im Feld **Projekte** auf **Sender**. Stellen Sie im Feld **Abhängigkeiten** sicher, dass **Receiver** aktiviert ist.

	![][20]

7. Klicken Sie auf **OK**, um das Dialogfeld mit den **Eigenschaften** zu schließen.

1.	Drücken Sie F5, um das Projekt **Receiver** in Visual Studio auszuführen, und warten Sie dann, bis die Empfänger für alle Partitionen gestartet wurden.

	![][21]

2.	Das Projekt **Sender** wird automatisch ausgeführt. Drücken Sie im Konsolenfenster die EINGABETASTE, und sehen Sie sich die im Empfängerfenster angezeigten Ereignisse an.

	![][22]

Drücken Sie im Fenster **Sender** die Tastenkombination STRG+C, um die Anwendung „Sender“ zu beenden. Drücken Sie anschließend im Fenster „Receiver“ die EINGABETASTE, um die Anwendung herunterzufahren.

## Nächste Schritte

Nachdem Sie eine funktionierende Anwendung erstellt haben, die einen Ereignis-Hub erstellt und Daten sendet und empfängt, können Sie mit den folgenden Szenarios fortfahren:

- Eine vollständige [Beispielanwendung mit Verwendung von Ereignis-Hubs][].
- Das Beispiel [Horizontales Hochskalieren der Ereignisverarbeitung mit Event Hubs][]
- Eine [Messaginglösung mit Warteschlange][] unter Verwendung von Service Bus-Warteschlangen.
- [Übersicht über Event Hubs][]

<!-- Images. -->
[19]: ./media/event-hubs-csharp-ephcs-getstarted/create-eh-proj1.png
[20]: ./media/event-hubs-csharp-ephcs-getstarted/create-eh-proj2.png
[21]: ./media/event-hubs-csharp-ephcs-getstarted/run-csharp-ephcs1.png
[22]: ./media/event-hubs-csharp-ephcs-getstarted/run-csharp-ephcs2.png

<!-- Links -->
[Azure classic portal]: https://manage.windowsazure.com/
[Ereignisprozessorhost]: https://www.nuget.org/packages/Microsoft.Azure.ServiceBus.EventProcessorHost
[Übersicht über Event Hubs]: event-hubs-overview.md
[Beispielanwendung mit Verwendung von Ereignis-Hubs]: https://code.msdn.microsoft.com/Service-Bus-Event-Hub-286fd097
[Horizontales Hochskalieren der Ereignisverarbeitung mit Event Hubs]: https://code.msdn.microsoft.com/Service-Bus-Event-Hub-45f43fc3
[Messaginglösung mit Warteschlange]: ../service-bus/service-bus-dotnet-multi-tier-app-using-service-bus-queues.md
 

<!---HONumber=AcomDC_0928_2016-->