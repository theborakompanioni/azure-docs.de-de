---
title: Empfangen von Ereignissen von Azure Event Hubs mithilfe von .NET Framework | Microsoft-Dokumentation
description: In diesem Tutorial erfahren Sie, wie Sie mithilfe von .NET Framework Ereignisse von Azure Event Hubs empfangen.
services: event-hubs
documentationcenter: 
author: sethmanheim
manager: timlt
editor: 
ms.assetid: c4974bd3-2a79-48a1-aa3b-8ee2d6655b28
ms.service: event-hubs
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 06/12/2017
ms.author: sethm
ms.translationtype: Human Translation
ms.sourcegitcommit: fc27849f3309f8a780925e3ceec12f318971872c
ms.openlocfilehash: c97cd41e503b5f2792f55e27038a2e07e254826e
ms.contentlocale: de-de
ms.lasthandoff: 06/14/2017

---
# <a name="receive-events-from-azure-event-hubs-using-the-net-framework"></a>Empfangen von Ereignissen von Azure Event Hubs mithilfe von .NET Framework

## <a name="introduction"></a>Einführung

Event Hubs ist ein Dienst, der große Mengen von Ereignisdaten (Telemetriedaten) von verbundenen Geräten und Anwendungen verarbeiten kann. Nach dem Sammeln von Daten auf Ereignis-Hubs können die Daten mithilfe eines Speicherclusters gespeichert oder mit einem Echtzeitanalyse-Anbieter transformiert werden. Diese umfangreiche Ereignissammlung und -verarbeitung ist eine wichtige Komponente moderner Anwendungsarchitekturen. Hierzu zählt auch das Internet der Dinge (Internet of Things, IoT).

In diesem Tutorial lernen Sie, eine .NET Framework-Konsolenanwendung zum Empfangen von Nachrichten von einem Event Hub mithilfe des **[Ereignisprozessorhosts][EventProcessorHost]** zu schreiben. Informationen zum Senden von Ereignissen mithilfe von .NET Framework finden Sie im Artikel [Senden von Ereignissen an Azure Event Hubs mithilfe von .NET Framework](event-hubs-dotnet-framework-getstarted-send.md). Alternativ können Sie auch links im Inhaltsverzeichnis auf die entsprechende Sendesprache klicken.

Der [Ereignisprozessorhost][EventProcessorHost] ist eine .NET-Klasse, die das Empfangen von Ereignissen von Event Hubs durch die Verwaltung von permanenten Prüfpunkten und parallelen Empfangsvorgängen von diesen Event Hubs vereinfacht. Mit dem [Ereignisprozessorhost][Event Processor Host] können Sie Ereignisse selbst dann auf mehrere Empfänger aufteilen, wenn sie in verschiedenen Knoten gehostet werden. Dieses Beispiel zeigt, wie der [Ereignisprozessorhost][EventProcessorHost] für einen einzelnen Empfänger verwendet wird. Das Beispiel [Skalieren der Ereignisverarbeitung][Scale out Event Processing with Event Hubs] veranschaulicht, wie der [Ereignisprozessorhost][EventProcessorHost] mit mehreren Empfängern verwendet wird.

## <a name="prerequisites"></a>Voraussetzungen

Zum Durchführen dieses Tutorials benötigen Sie Folgendes:

* [Microsoft Visual Studio 2015 oder höher](http://visualstudio.com) Die Screenshots in diesem Tutorial zeigen Visual Studio 2017.
* Ein aktives Azure-Konto. Falls Sie nicht über ein Konto verfügen, können Sie in nur wenigen Minuten ein kostenloses Konto erstellen. Ausführliche Informationen finden Sie unter [Kostenlose Azure-Testversion](https://azure.microsoft.com/free/).

## <a name="create-an-event-hubs-namespace-and-an-event-hub"></a>Erstellen eines Event Hubs-Namespace und eines Event Hubs

Verwenden Sie zunächst das [Azure-Portal](https://portal.azure.com), um einen Namespace vom Typ „Event Hubs“ zu erstellen, und beschaffen Sie die Verwaltungsanmeldeinformationen, die Ihre Anwendung für die Kommunikation mit dem Event Hub benötigt. Folgen Sie dem Ablauf in [diesem Artikel](event-hubs-create.md), um einen Namespace und einen Event Hub zu erstellen, und fahren Sie dann mit den folgenden Schritten in diesem Tutorial fort.

## <a name="create-an-azure-storage-account"></a>Erstellen eines Azure-Speicherkontos

Zur Verwendung des [Ereignisprozessorhosts][EventProcessorHost] benötigen Sie ein [Azure Storage-Konto][Azure Storage account]:

1. Melden Sie sich beim [Azure-Portal][Azure portal] an, und klicken Sie oben links auf dem Bildschirm auf **Neu**.
2. Klicken Sie auf **Storage** und anschließend auf **Speicherkonto**.
   
    ![](./media/event-hubs-dotnet-framework-getstarted-receive-eph/create-storage1.png)
3. Geben Sie auf dem Blatt **Speicherkonto erstellen** einen Namen für das Speicherkonto ein. Wählen Sie ein Azure-Abonnement, eine Ressourcengruppe und einen Standort, an dem die Ressource erstellt werden soll. Klicken Sie dann auf **Erstellen**.
   
    ![](./media/event-hubs-dotnet-framework-getstarted-receive-eph/create-storage2.png)
4. Klicken Sie in der Liste der Speicherkonten auf das neu erstellte Speicherkonto.
5. Klicken Sie auf dem Blatt des Speicherkontos auf **Zugriffsschlüssel**. Kopieren Sie den Wert für **key1** für die spätere Verwendung in diesem Tutorial.
   
    ![](./media/event-hubs-dotnet-framework-getstarted-receive-eph/create-storage3.png)

## <a name="create-a-receiver-console-application"></a>Erstellen einer Receiver-Konsolenanwendung

1. Erstellen Sie in Visual Studio mithilfe der Projektvorlage **Konsolenanwendung** ein neues Visual C#-Desktopanwendungsprojekt. Geben Sie dem Projekt den Namen **Receiver**.
   
    ![](./media/event-hubs-dotnet-framework-getstarted-receive-eph/create-receiver-csharp1.png)
2. Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste auf das **Receiver**-Projekt, und klicken Sie dann auf **NuGet-Pakete für Projektmappe verwalten**.
3. Klicken Sie auf die Registerkarte **Durchsuchen**, und suchen Sie nach `Microsoft Azure Service Bus Event Hub - EventProcessorHost`. Klicken Sie auf **Installieren**, und akzeptieren Sie die Nutzungsbedingungen.
   
    ![](./media/event-hubs-dotnet-framework-getstarted-receive-eph/create-eph-csharp1.png)
   
    Visual Studio lädt das NuGet-Paket [Azure Service Bus Event Hub – EventProcessorHost](https://www.nuget.org/packages/Microsoft.Azure.ServiceBus.EventProcessorHost)mit allen Abhängigkeiten herunter, installiert es und fügt dem Projekt einen Verweis auf das Paket hinzu.
4. Klicken Sie mit der rechten Maustaste auf das Projekt **Receiver**, klicken Sie auf **Hinzufügen** und dann auf **Klasse**. Geben Sie der neuen Klasse den Namen **SimpleEventProcessor**, und klicken Sie auf **Hinzufügen**, um die Klasse zu erstellen.
   
    ![](./media/event-hubs-dotnet-framework-getstarted-receive-eph/create-receiver-csharp2.png)
5. Fügen Sie die folgenden Anweisungen am Anfang der Datei "SimpleEventProcessor.cs" hinzu:
    
  ```csharp
  using Microsoft.ServiceBus.Messaging;
  using System.Diagnostics;
  ```
    
  Ersetzen Sie anschließend den Text der Klasse durch folgenden Code:
    
  ```csharp
  class SimpleEventProcessor : IEventProcessor
  {
    Stopwatch checkpointStopWatch;
    
    async Task IEventProcessor.CloseAsync(PartitionContext context, CloseReason reason)
    {
        Console.WriteLine("Processor Shutting Down. Partition '{0}', Reason: '{1}'.", context.Lease.PartitionId, reason);
        if (reason == CloseReason.Shutdown)
        {
            await context.CheckpointAsync();
        }
    }
    
    Task IEventProcessor.OpenAsync(PartitionContext context)
    {
        Console.WriteLine("SimpleEventProcessor initialized.  Partition: '{0}', Offset: '{1}'", context.Lease.PartitionId, context.Lease.Offset);
        this.checkpointStopWatch = new Stopwatch();
        this.checkpointStopWatch.Start();
        return Task.FromResult<object>(null);
    }
    
    async Task IEventProcessor.ProcessEventsAsync(PartitionContext context, IEnumerable<EventData> messages)
    {
        foreach (EventData eventData in messages)
        {
            string data = Encoding.UTF8.GetString(eventData.GetBytes());
    
            Console.WriteLine(string.Format("Message received.  Partition: '{0}', Data: '{1}'",
                context.Lease.PartitionId, data));
        }
    
        //Call checkpoint every 5 minutes, so that worker can resume processing from 5 minutes back if it restarts.
        if (this.checkpointStopWatch.Elapsed > TimeSpan.FromMinutes(5))
        {
            await context.CheckpointAsync();
            this.checkpointStopWatch.Restart();
        }
    }
  }
  ```
    
  Diese Klasse wird von **EventProcessorHost** zur Verarbeitung der vom Event Hub empfangenen Ereignisse aufgerufen. Die `SimpleEventProcessor`-Klasse verwendet eine Stoppuhr, um in regelmäßigen Abständen die „checkpoint“-Methode für den **EventProcessorHost**-Kontext aufzurufen. Dadurch wird sichergestellt, dass der Empfänger bei einem Neustart maximal nur die Daten verlieren kann, die in den letzten fünf Minuten verarbeitet wurden.
6. Fügen Sie in der **Program**-Klasse die folgende `using`-Anweisungen am Anfang der Datei hinzu:
    
  ```csharp
  using Microsoft.ServiceBus.Messaging;
  ```
    
  Überschreiben Sie dann die `Main`-Methode in der `Program`-Klasse mit dem folgenden Code. Ersetzen Sie dabei den Event Hub-Namen und die zuvor gespeicherte Verbindungszeichenfolge auf Namespace-Ebene sowie das Speicherkonto und den Speicherschlüssel, die Sie in den vorherigen Abschnitten kopiert haben. 
    
  ```csharp
  static void Main(string[] args)
  {
    string eventHubConnectionString = "{Event Hubs namespace connection string}";
    string eventHubName = "{Event Hub name}";
    string storageAccountName = "{storage account name}";
    string storageAccountKey = "{storage account key}";
    string storageConnectionString = string.Format("DefaultEndpointsProtocol=https;AccountName={0};AccountKey={1}", storageAccountName, storageAccountKey);
    
    string eventProcessorHostName = Guid.NewGuid().ToString();
    EventProcessorHost eventProcessorHost = new EventProcessorHost(eventProcessorHostName, eventHubName, EventHubConsumerGroup.DefaultGroupName, eventHubConnectionString, storageConnectionString);
    Console.WriteLine("Registering EventProcessor...");
    var options = new EventProcessorOptions();
    options.ExceptionReceived += (sender, e) => { Console.WriteLine(e.Exception); };
    eventProcessorHost.RegisterEventProcessorAsync<SimpleEventProcessor>(options).Wait();
    
    Console.WriteLine("Receiving. Press enter key to stop worker.");
    Console.ReadLine();
    eventProcessorHost.UnregisterEventProcessorAsync().Wait();
  }
  ```

7. Führen Sie das Programm aus, und stellen Sie sicher, dass keine Fehler auftreten.
  
Glückwunsch! Soeben haben Sie mithilfe eines Ereignisprozessorhosts Nachrichten von einem Event Hub erhalten.


> [!NOTE]
> Dieses Tutorial verwendet eine einzelne Instanz von [EventProcessorHost][EventProcessorHost]. Um den Durchsatz zu erhöhen, sollten Sie mehrere Instanzen von [EventProcessorHost][EventProcessorHost] ausführen, wie im Beispiel [Horizontal hochskalierte Ereignisverarbeitung][Horizontal hochskalierte Ereignisverarbeitung] beschrieben. In diesen Fällen koordinieren sich die verschiedenen Instanzen automatisch untereinander, um die Last der eingegangenen Ereignisse ausgeglichen zu verteilen. Wenn mehrere Empfänger für jeden Prozess *alle* Ereignisse verarbeiten sollen, müssen Sie das **ConsumerGroup** -Konzept verwenden. Wenn Ereignisse von anderen Computern empfangen werden, kann es hilfreich sein, die [EventProcessorHost][EventProcessorHost]-Instanzen nach den Computern (oder Rollen) zu benennen, auf denen sie bereitgestellt werden. Weitere Informationen zu diesen Themen finden Sie unter [Event Hubs – Übersicht][Event Hubs overview] und im [Event Hubs-Programmierhandbuch][Event Hubs Programming Guide].
> 
> 

## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie eine funktionierende Anwendung erstellt haben, die einen Event Hub erstellt und Daten sendet/empfängt, können Sie sich mit den Informationen unter den folgenden Links vertraut machen:

* [Ereignisprozessorhost][Event Processor Host]
* [Übersicht über Event Hubs][Event Hubs overview]
* [Event Hubs – häufig gestellte Fragen](event-hubs-faq.md)

<!-- Images. -->
[19]: ./media/event-hubs-csharp-ephcs-getstarted/create-eh-proj1.png
[20]: ./media/event-hubs-csharp-ephcs-getstarted/create-eh-proj2.png
[21]: ./media/event-hubs-csharp-ephcs-getstarted/run-csharp-ephcs1.png
[22]: ./media/event-hubs-csharp-ephcs-getstarted/run-csharp-ephcs2.png

<!-- Links -->
[EventProcessorHost]: https://www.nuget.org/packages/Microsoft.Azure.ServiceBus.EventProcessorHost
[Event Hubs overview]: event-hubs-what-is-event-hubs.md
[Scale out Event Processing with Event Hubs]: https://code.msdn.microsoft.com/Service-Bus-Event-Hub-45f43fc3
[Event Hubs Programming Guide]: event-hubs-programming-guide.md
[Azure Storage account]:../storage/common/storage-create-storage-account.md
[Event Processor Host]: /dotnet/api/microsoft.servicebus.messaging.eventprocessorhost
[Azure portal]: https://portal.azure.com

