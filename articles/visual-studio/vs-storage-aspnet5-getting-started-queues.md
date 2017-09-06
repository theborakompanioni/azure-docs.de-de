---
title: Erste Schritte mit Queue Storage und verbundenen Visual Studio-Diensten (ASP.NET Core) | Microsoft-Dokumentation
description: Erste Schritte mit Azure Queue Storage in einem ASP.NET Core-Projekt in Visual Studio
services: storage
documentationcenter: 
author: kraigb
manager: ghogen
editor: 
ms.assetid: 04977069-5b2d-4cba-84ae-9fb2f5eb1006
ms.service: storage
ms.workload: web
ms.tgt_pltfrm: vs-getting-started
ms.devlang: na
ms.topic: article
ms.date: 12/02/2016
ms.author: kraigb
ms.translationtype: HT
ms.sourcegitcommit: 83f19cfdff37ce4bb03eae4d8d69ba3cbcdc42f3
ms.openlocfilehash: 394e2138ed2ba21b949195e473dafeaf6413cccf
ms.contentlocale: de-de
ms.lasthandoff: 08/21/2017

---
# <a name="get-started-with-queue-storage-and-visual-studio-connected-services-aspnet-core"></a>Erste Schritte mit Queue Storage und verbundenen Visual Studio-Diensten (ASP.NET Core)
[!INCLUDE [storage-try-azure-tools-queues](../../includes/storage-try-azure-tools-queues.md)]

## <a name="overview"></a>Übersicht
Dieser Artikel beschreibt, wie die ersten Schritte beim Verwenden von Azure Queue Storage in Visual Studio aussehen, nachdem Sie über das Visual Studio-Dialogfeld **Verbundene Dienste hinzufügen** in einem ASP.NET Core-Projekt ein Azure Storage-Konto erstellt oder auf ein solches Konto verwiesen haben. Bei einem **Verbundene Dienste hinzufügen** -Vorgang werden die entsprechenden NuGet-Pakete installiert, um auf Azure-Speicher in Ihrem Projekt zuzugreifen, und wird die Verbindungszeichenfolge für das Speicherkonto in Ihren Konfigurationsdateien des Projekts hinzugefügt.

Der Azure-Warteschlangenspeicher ist ein Dienst zum Speichern einer großen Anzahl von Nachrichten, auf die weltweit mit authentifizierten Aufrufen mithilfe von HTTP oder HTTPS zugegriffen werden kann. Eine einzelne Warteschlangennachricht kann bis zu 64 Kilobytes (KB) groß sein, und eine Warteschlange kann Millionen von Nachrichten enthalten. Deren Anzahl ist nur durch die Kapazität des Speicherkontos begrenzt.

Damit Sie beginnen können, müssen Sie zuerst eine Azure-Warteschlage in Ihrem Speicherkonto erstellen. Wir zeigen Ihnen, wie Sie eine Warteschlange in Code erstellen. Außerdem wird gezeigt, wie Sie grundlegende Warteschlangenvorgänge, etwa Hinzufügen, Ändern, Lesen und Entfernen von Warteschlangennachrichten ausführen. Die Beispiele sind in C\# geschrieben und greifen auf die Azure Storage-Clientbibliothek für .NET zurück. Weitere Informationen zu ASP.NET finden Sie unter [ASP.NET](http://www.asp.net).

**HINWEIS**: Einige der APIs, die Aufrufe des Azure Storage in ASP.NET Core ausführen, arbeiten asynchron. Unter [Asynchrone Programmierung mit Async und Await](http://msdn.microsoft.com/library/hh191443.aspx) finden Sie weitere Informationen. Im folgenden Code wird die Programmierung mit Async-Methoden angenommen.

* Weitere Informationen zum programmgesteuerten Bearbeiten von Warteschlangen finden Sie unter [Erste Schritte mit Azure Queue Storage mit .NET](../storage/queues/storage-dotnet-how-to-use-queues.md) .
* Allgemeine Informationen zum Azure-Speicher finden Sie unter [Speicherdokumentation](https://azure.microsoft.com/documentation/services/storage/) .
* Allgemeine Informationen zu Azure-Clouddiensten finden Sie unter [Cloud Services-Dokumentation](https://azure.microsoft.com/documentation/services/cloud-services/) .
* Weitere Informationen über das Programmieren von ASP.NET-Anwendungen finden Sie unter [ASP.NET](http://www.asp.net) .

## <a name="access-queues-in-code"></a>Zugriff auf Warteschlangen in Code
Um auf Warteschlangen in ASP.NET Core-Projekten zuzugreifen, müssen Sie jeder C#-Quelldatei, in der auf Azure Queue Storage zugegriffen wird, die folgenden Elemente hinzufügen.

1. Vergewissern Sie sich, dass die Namespacedeklarationen am Anfang der C#-Datei diese **using** -Anweisungen enthalten.
   
        using Microsoft.Framework.Configuration;
        using Microsoft.WindowsAzure.Storage;
        using Microsoft.WindowsAzure.Storage.Queue;
        using System.Threading.Tasks;
        using LogLevel = Microsoft.Framework.Logging.LogLevel;
2. Rufen Sie ein **CloudStorageAccount** -Objekt ab, das die Informationen zu Ihrem Speicherkonto enthält. Verwenden Sie den folgenden Code, um Ihre Speicherverbindungszeichenfolge und Speicherkontoinformationen aus der Azure-Dienstkonfiguration abzurufen.
   
         CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
           CloudConfigurationManager.GetSetting("<storage-account-name>_AzureStorageConnectionString"));
3. Rufen Sie ein **CloudQueueClient** -Objekt ab, um auf die Warteschlangenobjekte in Ihrem Speicherkonto zu verweisen.  
   
        // Create the CloudQueueClient object for the storage account.
        CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();
4. Abrufen eines **CloudQueue** -Objekts, das auf eine bestimmte Warteschlange verweist.
   
        // Get a reference to the CloudQueue named "messageQueue"
        CloudQueue messageQueue = queueClient.GetQueueReference("messageQueue");

**HINWEIS:** Verwenden Sie den gesamten obigen Code vor dem Code in den folgenden Beispielen.

### <a name="create-a-queue-in-code"></a>Erstellen einer Warteschlange in Code
Fügen Sie zum Erstellen der Azure-Warteschlange im Code einfach einen Aufruf von **CreateIfNotExists**hinzu.

    // Create the CloudQueue if it does not exist.
    await messageQueue.CreateIfNotExistsAsync();

## <a name="add-a-message-to-a-queue"></a>Hinzufügen von Nachrichten zu einer Warteschlange
Wenn Sie eine Nachricht in eine vorhandene Warteschlange einfügen möchten, erstellen Sie zuerst ein neues **CloudQueueMessage**-Objekt, und rufen Sie dann die **AddMessageAsync**-Methode auf.

Ein **CloudQueueMessage** -Objekt kann aus einer Zeichenfolge (im UTF-8-Format) oder aus einem Bytearray erstellt werden.

Hier ist ein Beispiel, dass die Nachricht "Hello, World" eingefügt.

    // Create a message and add it to the queue.
    CloudQueueMessage message = new CloudQueueMessage("Hello, World");
    await messageQueue.AddMessageAsync(message);

## <a name="read-a-message-in-a-queue"></a>Lesen von Nachrichten in einer Warteschlange
Sie können einen Blick auf die Nachricht am Anfang einer Warteschlange werfen, ohne sie aus der Warteschlange zu entfernen, indem Sie die **PeekMessageAsync()** -Methode aufrufen.

    // Peek the next message in the queue. 
    CloudQueueMessage peekedMessage = await messageQueue.PeekMessageAsync();


## <a name="read-and-remove-a-message-in-a-queue"></a>Lesen und Entfernen von Nachrichten in einer Warteschlange
Ihr Code kann eine Nachricht in zwei Schritten aus der Warteschlange entfernen.

1. Rufen Sie **GetMessageAsync()** auf, um die nächste Nachricht in einer Warteschlange abzurufen. Eine von **GetMessageAsync()** zurückgegebene Nachricht ist für anderen Code nicht mehr sichtbar, der Nachrichten aus dieser Warteschlange liest. Standardmäßig bleibt die Nachricht 30 Sekunden lang unsichtbar.
2. Wenn Sie die Nachricht endgültig aus der Warteschlange entfernen möchten, rufen Sie **DeleteMessageAsync()**auf.

Dieser zweistufige Prozess zum Entfernen von Nachrichten stellt sicher, dass eine andere Codeinstanz dieselbe Nachricht erneut abrufen kann, falls die Verarbeitung aufgrund eines Hardware- oder Softwarefehlers fehlschlägt. Der folgende Code ruft **DeleteMessageAsync()** direkt nach der Verarbeitung der Nachricht auf.

    // Get the next message in the queue.
    CloudQueueMessage retrievedMessage = await messageQueue.GetMessageAsync();

    // Process the message in less than 30 seconds.

    // Then delete the message.
    await messageQueue.DeleteMessageAsync(retrievedMessage);

## <a name="leverage-additional-options-for-dequeuing-messages"></a>Zusätzliche Optionen für das Entfernen von Nachrichten aus der Warteschlange
Es gibt zwei Möglichkeiten, wie Sie das Abrufen von Nachrichten aus der Warteschlange anpassen können.
Erstens können Sie einen Nachrichtenstapel abrufen (bis zu 32). Zweitens können Sie das Unsichtbarkeits-Zeitlimit verkürzen oder verlängern, sodass der Code mehr oder weniger Zeit zur vollständigen Verarbeitung jeder Nachricht benötigt. Im folgenden Codebeispiel wird **GetMessages** verwendet, um 20 Nachrichten mit einem Aufruf abzurufen. Anschließend wird jede Nachricht mithilfe einer **foreach** -Schleife verarbeitet. Außerdem wird das Unsichtbarkeits-Zeitlimit auf 5 Minuten pro Nachricht festgelegt. Beachten Sie, dass die fünf Minuten für alle Nachrichten gleichzeitig beginnen, sodass fünf Minuten nach dem Aufruf von **GetMessages**alle Nachrichten, die nicht gelöscht wurden, wieder sichtbar werden.

    // Retrieve 20 messages at a time, keeping those messages invisible for 5 minutes, 
    //   delete each message after processing.

    foreach (CloudQueueMessage message in messageQueue.GetMessages(20, TimeSpan.FromMinutes(5)))
    {
        // Process all messages in less than 5 minutes, deleting each message after processing.
        queue.DeleteMessage(message);
    }

## <a name="get-the-queue-length"></a>Abrufen der Warteschlangenlänge
Sie können die Anzahl der Nachrichten in einer Warteschlange schätzen lassen. Die **FetchAttributes** -Methode fordert den Warteschlangendienst auf, die Warteschlangenattribute einschließlich der Nachrichtenanzahl abzurufen. Die **ApproximateMethodCount**-Eigenschaft gibt den letzten von der **FetchAttributes**-Methode abgerufenen Wert zurück, ohne den Warteschlangendienst aufzurufen.

    // Fetch the queue attributes.
    messageQueue.FetchAttributes();

    // Retrieve the cached approximate message count.
    int? cachedMessageCount = messageQueue.ApproximateMessageCount;

    // Display the number of messages.
    Console.WriteLine("Number of messages in queue: " + cachedMessageCount);

## <a name="use-the-async-await-pattern-with-common-queue-apis"></a>Verwenden des Async-Await-Musters mit allgemeinen Warteschlangen-APIs
In diesem Beispiel wird veranschaulicht, wie das Async-Await-Muster mit allgemeinen Warteschlangen-API verwendet wird. Im Beispiel werden jeweils die asynchronen Versionen der angegebenen Methoden aufgerufen Dies ist am Postfix "Async" der einzelnen Methoden erkennbar. Wenn eine asynchrone Methode verwendet wird, hält das Async-Await-Muster die lokale Ausführung an, bis der Aufruf abgeschlossen ist. Durch dieses Verhalten kann der aktuelle Thread eine andere Aktion ausführen, wodurch Leistungsengpässe vermieden werden und die allgemeine Reaktionsfähigkeit der Anwendung verbessert wird. Weitere Informationen zur Verwendung des Async-Await-Musters in .NET finden Sie unter [Async und Await (C# und Visual Basic)](https://msdn.microsoft.com/library/hh191443.aspx).

    // Create a message to add to the queue.
    CloudQueueMessage cloudQueueMessage = new CloudQueueMessage("My message");

    // Async enqueue the message.
    await messageQueue.AddMessageAsync(cloudQueueMessage);
    Console.WriteLine("Message added");

    // Async dequeue the message.
    CloudQueueMessage retrievedMessage = await messageQueue.GetMessageAsync();
    Console.WriteLine("Retrieved message with content '{0}'", retrievedMessage.AsString);

    // Async delete the message.
    await messageQueue.DeleteMessageAsync(retrievedMessage);
    Console.WriteLine("Deleted message");
## <a name="delete-a-queue"></a>Löschen einer Warteschlange
Zum Löschen einer Warteschlange und aller darin enthaltenen Nachrichten rufen Sie die Methode **Delete** für das Warteschlangenobjekt auf.

    // Delete the queue.
    messageQueue.Delete();


## <a name="next-steps"></a>Nächste Schritte
[!INCLUDE [vs-storage-dotnet-queues-next-steps](../../includes/vs-storage-dotnet-queues-next-steps.md)]


