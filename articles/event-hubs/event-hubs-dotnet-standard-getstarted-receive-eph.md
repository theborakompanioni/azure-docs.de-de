---
title: Empfangen von Ereignissen von Azure Event Hubs mithilfe von .NET Standard | Microsoft-Dokumentation
description: "Erste Schritte zum Empfangen von Nachrichten mit „EventProcessorHost“ in .NET Standard"
services: event-hubs
documentationcenter: na
author: jtaubensee
manager: timlt
editor: 
ms.assetid: 
ms.service: event-hubs
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/27/2017
ms.author: jotaub;sethm
translationtype: Human Translation
ms.sourcegitcommit: 6e0ad6b5bec11c5197dd7bded64168a1b8cc2fdd
ms.openlocfilehash: c86a1feee02bbf8580a40119ac140528217e435d
ms.lasthandoff: 03/28/2017

---

# <a name="get-started-receiving-messages-with-the-event-processor-host-in-net-standard"></a>Erste Schritte zum Empfangen von Nachrichten mit dem Ereignisprozessorhost (EventProcessorHost) in .NET Standard

> [!NOTE]
> Dieses Beispiel ist auf [GitHub](https://github.com/Azure/azure-event-hubs/tree/master/samples/SampleEphReceiver) verfügbar.

In diesem Tutorial lernen Sie, wie Sie eine .NET Core-Konsolenanwendung zum Empfangen von Nachrichten von einer Event Hubs-Instanz mithilfe von **EventProcessorHost** schreiben können. Sie können die [GitHub](https://github.com/Azure/azure-event-hubs/tree/master/samples/SampleEphReceiver)-Projektmappe ohne Änderung ausführen, indem Sie die Zeichenfolgen durch Ihre Event Hub- und Speicherkontowerte ersetzen. Sie können auch die Schritte in diesem Tutorial ausführen, um eine eigene zu erstellen.

## <a name="prerequisites"></a>Voraussetzungen

* [Microsoft Visual Studio 2015 oder 2017](http://www.visualstudio.com) In den Beispielen in diesem Tutorial wird Visual Studio 2017 verwendet, aber Visual Studio 2015 wird ebenfalls unterstützt.
* [.NET Core Visual Studio 2015- oder 2017-Tools](http://www.microsoft.com/net/core).
* Ein Azure-Abonnement.
* Ein Azure Event Hubs-Namespace
* Ein Azure-Speicherkonto.

## <a name="create-an-event-hubs-namespace-and-an-event-hub"></a>Erstellen eines Event Hubs-Namespace und eines Event Hubs  

Verwenden Sie zunächst das [Azure-Portal](https://portal.azure.com), um einen Namespace vom Typ „Event Hubs“ zu erstellen, und rufen Sie damit die Verwaltungsanmeldeinformationen ab, die Ihre Anwendung benötigt, um mit der Event Hubs-Instanz zu kommunizieren. Folgen Sie dem Ablauf in [diesem Artikel](event-hubs-create.md), um einen Namespace und eine Event Hubs-Instanz zu erstellen, und fahren Sie dann mit folgenden Schritten fort.  

## <a name="create-an-azure-storage-account"></a>Erstellen eines Azure-Speicherkontos  

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com)an.  
2. Klicken Sie im Portal im Navigationsbereich auf der linken Seite auf **Neu**, **Storage** und **Speicherkonto**.  
3. Füllen Sie die Felder auf dem Blatt „Speicherkonto“ aus, und klicken Sie dann auf **Erstellen**.

    ![Speicherkonto erstellen][1]

4. Klicken Sie nach dem Anzeigen der Meldung **Die Bereitstellungen waren erfolgreich** auf den Namen des neuen Speicherkontos. Klicken Sie auf dem Blatt **Zusammenfassung** auf **Blobs**. Klicken Sie oben auf **+ Container**, nachdem das Blatt **Blob-Dienst** geöffnet wurde. Benennen Sie den Container, und schließen Sie das Blatt **Blob-Dienst**.  
5. Klicken Sie auf dem linken Blatt auf **Zugriffsschlüssel**, und kopieren Sie den Namen des Speichercontainers, des Speicherkontos und den Wert von **key1**. Speichern Sie diese Werte im Editor oder an einem anderen temporären Speicherort.  

## <a name="create-a-console-application"></a>Erstellen einer Konsolenanwendung

Starten Sie Visual Studio. Klicken Sie im Menü **Datei** auf **Neu** und dann auf **Projekt**. Erstellen Sie eine .NET Core-Konsolenanwendung.

![Neues Projekt][2]

## <a name="add-the-event-hubs-nuget-package"></a>Hinzufügen des Event Hubs-NuGet-Pakets

Fügen Sie Ihrem Projekt die folgenden NuGet-Pakete hinzu:
* [`Microsoft.Azure.EventHubs`](https://www.nuget.org/packages/Microsoft.Azure.EventHubs/)
* [`Microsoft.Azure.EventHubs.Processor`](https://www.nuget.org/packages/Microsoft.Azure.EventHubs.Processor/)

## <a name="implement-the-ieventprocessor-interface"></a>Implementieren der IEventProcessor-Schnittstelle

1. Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste auf das Projekt, und klicken Sie auf **Hinzufügen** und anschließend auf **Klasse**. Nennen Sie die neue Klasse **SimpleEventProcessor**.

2. Öffnen Sie die Datei „SimpleEventProcessor.cs“, und fügen Sie folgende `using`-Anweisung am Anfang der Datei ein.

    ```csharp
    using Microsoft.Azure.EventHubs;
    using Microsoft.Azure.EventHubs.Processor;
    using System.Threading.Tasks;
    ```

3. Implementieren Sie die `IEventProcessor`-Schnittstelle. Ersetzen Sie den gesamten Inhalt der Klasse `SimpleEventProcessor` durch folgenden Code:

    ```csharp
    public class SimpleEventProcessor : IEventProcessor
    {
        public Task CloseAsync(PartitionContext context, CloseReason reason)
        {
            Console.WriteLine($"Processor Shutting Down. Partition '{context.PartitionId}', Reason: '{reason}'.");
            return Task.CompletedTask;
        }

        public Task OpenAsync(PartitionContext context)
        {
            Console.WriteLine($"SimpleEventProcessor initialized. Partition: '{context.PartitionId}'");
            return Task.CompletedTask;
        }

        public Task ProcessErrorAsync(PartitionContext context, Exception error)
        {
            Console.WriteLine($"Error on Partition: {context.PartitionId}, Error: {error.Message}");
            return Task.CompletedTask;
        }

        public Task ProcessEventsAsync(PartitionContext context, IEnumerable<EventData> messages)
        {
            foreach (var eventData in messages)
            {
                var data = Encoding.UTF8.GetString(eventData.Body.Array, eventData.Body.Offset, eventData.Body.Count);
                Console.WriteLine($"Message received. Partition: '{context.PartitionId}', Data: '{data}'");
            }

            return context.CheckpointAsync();
        }
    }
    ```

## <a name="write-a-main-console-method-that-uses-the-simpleeventprocessor-class-to-receive-messages"></a>Schreiben Sie eine Hauptkonsolenmethode, die die Klasse „SimpleEventProcessor“ verwendet, um Nachrichten zu erhalten

1. Fügen Sie am Anfang der Datei „Program.cs“ die folgenden `using`-Anweisungen hinzu:

    ```csharp
    using Microsoft.Azure.EventHubs;
    using Microsoft.Azure.EventHubs.Processor;
    using System.Threading.Tasks;
    ```

2. Fügen Sie der `Program`-Klasse Konstanten für die Event Hubs-Verbindungszeichenfolge, den Event Hub-Namen, den Speicherkonto-Containernamen, den Speicherkontonamen und den Speicherkontoschlüssel hinzu. Fügen Sie folgenden Code ein, sodass die Platzhalter durch die entsprechenden Werte ersetzt werden.

    ```csharp
    private const string EhConnectionString = "{Event Hubs connection string}";
    private const string EhEntityPath = "{Event Hub path/name}";
    private const string StorageContainerName = "{Storage account container name}";
    private const string StorageAccountName = "{Storage account name}";
    private const string StorageAccountKey = "{Storage account key}";

    private static readonly string StorageConnectionString = string.Format("DefaultEndpointsProtocol=https;AccountName={0};AccountKey={1}", StorageAccountName, StorageAccountKey);
    ```   

3. Fügen Sie wie folgt eine neue Methode mit dem Namen `MainAsync` in die `Program`-Klasse ein:

    ```csharp
    private static async Task MainAsync(string[] args)
    {
        Console.WriteLine("Registering EventProcessor...");

        var eventProcessorHost = new EventProcessorHost(
            EhEntityPath,
            PartitionReceiver.DefaultConsumerGroupName,
            EhConnectionString,
            StorageConnectionString,
            StorageContainerName);

        // Registers the Event Processor Host and starts receiving messages
        await eventProcessorHost.RegisterEventProcessorAsync<SimpleEventProcessor>();

        Console.WriteLine("Receiving. Press ENTER to stop worker.");
        Console.ReadLine();

        // Disposes of the Event Processor Host
        await eventProcessorHost.UnregisterEventProcessorAsync();
    }
    ```

3. Fügen Sie der `Main`-Methode die folgende Codezeile hinzu:

    ```csharp
    MainAsync(args).GetAwaiter().GetResult();
    ```

    Die Datei „Program.cs“ sollte nun wie folgt aussehen:

    ```csharp
    namespace SampleEphReceiver
    {

        public class Program
        {
            private const string EhConnectionString = "{Event Hubs connection string}";
            private const string EhEntityPath = "{Event Hub path/name}";
            private const string StorageContainerName = "{Storage account container name}";
            private const string StorageAccountName = "{Storage account name}";
            private const string StorageAccountKey = "{Storage account key}";

            private static readonly string StorageConnectionString = string.Format("DefaultEndpointsProtocol=https;AccountName={0};AccountKey={1}", StorageAccountName, StorageAccountKey);

            public static void Main(string[] args)
            {
                MainAsync(args).GetAwaiter().GetResult();
            }

            private static async Task MainAsync(string[] args)
            {
                Console.WriteLine("Registering EventProcessor...");

                var eventProcessorHost = new EventProcessorHost(
                    EhEntityPath,
                    PartitionReceiver.DefaultConsumerGroupName,
                    EhConnectionString,
                    StorageConnectionString,
                    StorageContainerName);

                // Registers the Event Processor Host and starts receiving messages
                await eventProcessorHost.RegisterEventProcessorAsync<SimpleEventProcessor>();

                Console.WriteLine("Receiving. Press ENTER to stop worker.");
                Console.ReadLine();

                // Disposes of the Event Processor Host
                await eventProcessorHost.UnregisterEventProcessorAsync();
            }
        }
    }
    ```

4. Führen Sie das Programm aus, und stellen Sie sicher, dass keine Fehler auftreten.

Glückwunsch! Soeben haben Sie Nachrichten von einer Event Hubs-Instanz mithilfe eines Ereignisprozessorhosts erhalten.

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen zu Event Hubs finden Sie unter den folgenden Links:

* [Übersicht über Event Hubs](event-hubs-what-is-event-hubs.md)
* [Erstellen eines Event Hubs](event-hubs-create.md)
* [Event Hubs – häufig gestellte Fragen](event-hubs-faq.md)

[1]: ./media/event-hubs-dotnet-standard-getstarted-receive-eph/event-hubs-python1.png
[2]: ./media/event-hubs-dotnet-standard-getstarted-receive-eph/netcore.png

