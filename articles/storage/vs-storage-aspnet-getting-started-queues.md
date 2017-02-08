---
title: Erste Schritte mit Queue Storage und verbundenen Visual Studio-Diensten (ASP.NET) | Microsoft-Dokumentation
description: "Erfahren Sie mehr über die ersten Schritte mit Azure Queue Storage in einem ASP.NET 5-Projekt in Visual Studio, nachdem Sie mithilfe von verbundenen Visual Studio-Diensten eine Verbindung mit einem Speicherkonto hergestellt haben."
services: storage
documentationcenter: 
author: TomArcher
manager: douge
editor: 
ms.assetid: 94ca3413-5497-433f-abbe-836f83a9de72
ms.service: storage
ms.workload: web
ms.tgt_pltfrm: vs-getting-started
ms.devlang: na
ms.topic: article
ms.date: 12/02/2016
ms.author: tarcher
translationtype: Human Translation
ms.sourcegitcommit: f58c2b522f81dc2dc86f0d2c6bc4872504cf7377
ms.openlocfilehash: 70875287e79aaf49e1b8802cf2953cd5381b97f4


---
# <a name="get-started-with-azure-queue-storage-and-visual-studio-connected-services-aspnet"></a>Erste Schritte mit Azure Queue Storage und verbundenen Visual Studio-Diensten (ASP.NET)
[!INCLUDE [storage-try-azure-tools-queues](../../includes/storage-try-azure-tools-queues.md)]

## <a name="overview"></a>Übersicht

Azure Queue Storage ist ein Dienst zur Speicherung großer Mengen unstrukturierter Daten, auf die über HTTP oder HTTPS zugegriffen werden kann. Eine einzelne Warteschlangennachricht kann bis zu 64 KB groß sein, und eine Warteschlange kann eine unbegrenzte Anzahl von Nachrichten enthalten, die nur durch die Gesamtkapazität des Speicherkontos begrenzt wird.

In diesem Artikel wird beschrieben, wie Sie Azure Queue Storage-Entitäten programmgesteuert verwalten, allgemeine Aufgaben wie das Erstellen einer Azure-Warteschlange ausführen sowie Nachrichten in Warteschlangen hinzufügen, ändern, lesen und entfernen.

> [!NOTE]
> 
> In den Codeabschnitten in diesem Artikel wird davon ausgegangen, dass Sie bereits über Verbundene Dienste eine Verbindung mit einem Azure-Speicherkonto hergestellt haben. Verbundene Dienste wird konfiguriert, indem Sie den Visual Studio Projektmappen-Explorer öffnen, mit der rechten Maustaste auf das Projekt klicken und dann im Kontextmenü **Hinzufügen->Verbundener Dienst** auswählen. Befolgen Sie die Anleitungen im eingeblendeten Dialogfeld zum Herstellen der Verbindung mit dem gewünschten Azure-Speicherkonto.      

## <a name="create-a-queue"></a>Erstellen einer Warteschlange

Die folgenden Schritte beschreiben, wie Sie eine Warteschlange programmgesteuert erstellen. In einer ASP.NET MVC-Anwendung würde sich der Code in einem Controller befinden.

1. Fügen Sie die folgenden *using*-Direktiven hinzu:
   
        using Microsoft.Azure;
        using Microsoft.WindowsAzure.Storage;
        using Microsoft.WindowsAzure.Storage.Queue;

2. Rufen Sie ein **CloudStorageAccount**-Objekt ab, das die Informationen zu Ihrem Speicherkonto enthält. Verwenden Sie den folgenden Code, um die Speicherverbindungszeichenfolge und Speicherkontoinformationen aus der Azure-Dienstkonfiguration abzurufen. (Ändern Sie *storage-account-name>* in den Namen des Azure-Speicherkontos, auf das Sie zugreifen.)

         CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
           CloudConfigurationManager.GetSetting("<storage-account-name>_AzureStorageConnectionString"));

3. Rufen Sie ein **CloudQueueClient**-Objekt ab, das einen Warteschlangendienstclient darstellt.

        CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();

4. Rufen Sie ein **CloudQueue**-Objekt ab, das einen Verweis auf den gewünschten Warteschlangennamen darstellt. (Ändern Sie *<queue-name>* in den Namen der Warteschlange, den Sie erstellen möchten.)

        CloudQueue queue = queueClient.GetQueueReference(<queue-name>);

5. Rufen Sie die **CloudQueue.CreateIfNotExists**-Methode auf, um die Warteschlange zu erstellen, falls noch nicht vorhanden. 

        queue.CreateIfNotExists();


## <a name="add-a-message-to-a-queue"></a>Hinzufügen von Nachrichten zu einer Warteschlange

Die folgenden Schritte beschreiben, wie Sie eine Nachricht einer Warteschlange programmgesteuert hinzufügen. In einer ASP.NET MVC-Anwendung würde sich der Code in einem Controller befinden.

1. Fügen Sie die folgenden *using*-Direktiven hinzu:
   
        using Microsoft.Azure;
        using Microsoft.WindowsAzure.Storage;
        using Microsoft.WindowsAzure.Storage.Queue;

2. Rufen Sie ein **CloudStorageAccount** -Objekt ab, das die Informationen zu Ihrem Speicherkonto enthält. Verwenden Sie den folgenden Code, um die Speicherverbindungszeichenfolge und Speicherkontoinformationen aus der Azure-Dienstkonfiguration abzurufen. (Ändern Sie *storage-account-name>* in den Namen des Azure-Speicherkontos, auf das Sie zugreifen.)

         CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
           CloudConfigurationManager.GetSetting("<storage-account-name>_AzureStorageConnectionString"));

3. Rufen Sie ein **CloudQueueClient**-Objekt ab, das einen Warteschlangendienstclient darstellt.

        CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();

4. Rufen Sie ein **CloudQueue**-Objekt ab, das einen Verweis auf den gewünschten Warteschlangennamen darstellt. (Ändern Sie *<queue-name>* in den Namen der Warteschlange, der Sie die Nachricht hinzufügen möchten.)

        CloudQueue queue = queueClient.GetQueueReference(<queue-name>);

5. Erstellen Sie das **CloudQueueMessage**-Objekt, das die Nachricht darstellt, die Sie der Warteschlange hinzufügen möchten. Ein **CloudQueueMessage** -Objekt kann aus einer Zeichenfolge (im UTF-8-Format) oder aus einem Bytearray erstellt werden. (Ändern Sie *<queue-message>* in die Nachricht, die Sie hinzufügen möchten.

        CloudQueueMessage message = new CloudQueueMessage(<queue-message>);

6. Rufen Sie die **CloudQueue.AddMessage**-Methode auf, um die Nachricht der Warteschlange hinzuzufügen.

        queue.AddMessage(message);

## <a name="read-a-message-from-a-queue-without-removing-it"></a>Lesen einer Nachricht in einer Warteschlange, ohne sie zu entfernen

Die folgenden Schritte veranschaulichen, wie Sie programmgesteuert einen Blick auf die Nachricht in der Warteschlange werfen (die Nachricht lesen, ohne sie zu entfernen). In einer ASP.NET MVC-Anwendung würde sich der Code in einem Controller befinden. 

1. Fügen Sie die folgenden *using*-Direktiven hinzu:
   
        using Microsoft.Azure;
        using Microsoft.WindowsAzure.Storage;
        using Microsoft.WindowsAzure.Storage.Queue;

2. Rufen Sie ein **CloudStorageAccount** -Objekt ab, das die Informationen zu Ihrem Speicherkonto enthält. Verwenden Sie den folgenden Code, um die Speicherverbindungszeichenfolge und Speicherkontoinformationen aus der Azure-Dienstkonfiguration abzurufen. (Ändern Sie *storage-account-name>* in den Namen des Azure-Speicherkontos, auf das Sie zugreifen.)

         CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
           CloudConfigurationManager.GetSetting("<storage-account-name>_AzureStorageConnectionString"));

3. Rufen Sie ein **CloudQueueClient**-Objekt ab, das einen Warteschlangendienstclient darstellt.

        CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();

4. Rufen Sie ein **CloudQueue**-Objekt ab, das einen Verweis auf die Warteschlange darstellt. (Ändern Sie *<queue-name>* in den Namen der Warteschlange, in der Sie die Nachricht lesen möchten.)

        CloudQueue queue = queueClient.GetQueueReference(<queue-name>);

5. Rufen Sie die **CloudQueue.PeekMessage**-Methode auf, um die Nachricht am Anfang einer Warteschlange zu lesen, ohne sie daraus zu entfernen.

        CloudQueueMessage message = queue.PeekMessage();

6. Greifen Sie auf den Wert des **CloudQueueMessage**-Objekts mithilfe der **CloudQueueMessage.AsBytes**- oder **CloudQueueMessage.AsString**-Eigenschaft zu.

        string messageAsString = message.AsString;
        byte[] messageAsBytes = message.AsBytes;

## <a name="read-and-remove-a-message-from-a-queue"></a>Lesen und Entfernen von Nachrichten aus einer Warteschlange

Die folgenden Schritte beschreiben, wie Sie eine Nachricht in einer Warteschlange programmgesteuert lesen und anschließend löschen. In einer ASP.NET MVC-Anwendung würde sich der Code in einem Controller befinden. 

1. Fügen Sie die folgenden *using*-Direktiven hinzu:
   
        using Microsoft.Azure;
        using Microsoft.WindowsAzure.Storage;
        using Microsoft.WindowsAzure.Storage.Queue;

2. Rufen Sie ein **CloudStorageAccount** -Objekt ab, das die Informationen zu Ihrem Speicherkonto enthält. Verwenden Sie den folgenden Code, um die Speicherverbindungszeichenfolge und Speicherkontoinformationen aus der Azure-Dienstkonfiguration abzurufen. (Ändern Sie *storage-account-name>* in den Namen des Azure-Speicherkontos, auf das Sie zugreifen.)

         CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
           CloudConfigurationManager.GetSetting("<storage-account-name>_AzureStorageConnectionString"));

3. Rufen Sie ein **CloudQueueClient**-Objekt ab, das einen Warteschlangendienstclient darstellt.

        CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();

4. Rufen Sie ein **CloudQueue**-Objekt ab, das einen Verweis auf die Warteschlange darstellt. (Ändern Sie *<queue-name>* in den Namen der Warteschlange, in der Sie die Nachricht lesen möchten.)

        CloudQueue queue = queueClient.GetQueueReference(<queue-name>);

5. Rufen Sie die **CloudQueue.GetMessage**-Methode auf, um die erste Nachricht in der Warteschlange zu lesen. Die **CloudQueue.GetMessage**-Methode macht die Nachricht (standardmäßig) für anderen Code zum Lesen von Nachrichten für 30 Sekunden unsichtbar, sodass kein anderer Code die Nachricht ändern oder lesen kann, während Sie sie verarbeiten. Um die Zeitspanne zu ändern, in der die Nachricht nicht sichtbar ist, ändern Sie den **VisibilityTimeout**-Parameter, der an die **CloudQueue.GetMessage**-Methode übergeben wurde.

        // This message will be invisible to other code for 30 seconds.
        CloudQueueMessage message = queue.GetMessage();     

6. Rufen Sie die **CloudQueueMessage.Delete**-Methode auf, um die Nachricht aus der Warteschlange zu löschen.

        queue.DeleteMessage(message);

## <a name="get-the-queue-length"></a>Abrufen der Warteschlangenlänge

Die folgenden Schritte beschreiben, wie Sie die Länge einer Warteschlange (Anzahl der Nachrichten) programmgesteuert abrufen. In einer ASP.NET MVC-Anwendung würde sich der Code in einem Controller befinden. 

1. Fügen Sie die folgenden *using*-Direktiven hinzu:
   
        using Microsoft.Azure;
        using Microsoft.WindowsAzure.Storage;
        using Microsoft.WindowsAzure.Storage.Queue;

2. Rufen Sie ein **CloudStorageAccount** -Objekt ab, das die Informationen zu Ihrem Speicherkonto enthält. Verwenden Sie den folgenden Code, um die Speicherverbindungszeichenfolge und Speicherkontoinformationen aus der Azure-Dienstkonfiguration abzurufen. (Ändern Sie *storage-account-name>* in den Namen des Azure-Speicherkontos, auf das Sie zugreifen.)

         CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
           CloudConfigurationManager.GetSetting("<storage-account-name>_AzureStorageConnectionString"));

3. Rufen Sie ein **CloudQueueClient**-Objekt ab, das einen Warteschlangendienstclient darstellt.

        CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();

4. Rufen Sie ein **CloudQueue**-Objekt ab, das einen Verweis auf die Warteschlange darstellt. (Ändern Sie *<queue-name>* in den Namen der Warteschlange, deren Länge Sie abfragen.)

        CloudQueue queue = queueClient.GetQueueReference(<queue-name>);

5. Rufen Sie die **CloudQueue.FetchAttributes**-Methode auf, um die Attribute der Warteschlange (einschließlich Länge) abzurufen. 

        queue.FetchAttributes();

6. Greifen Sie auf die **CloudQueue.ApproximateMessageCount**-Eigenschaft zu, um die Länge der Warteschlange abzurufen.
 
        int? nMessages = queue.ApproximateMessageCount;

## <a name="delete-a-queue"></a>Löschen einer Warteschlange
Die folgenden Schritte beschreiben, wie Sie eine Warteschlange programmgesteuert löschen. 

1. Fügen Sie die folgenden *using*-Direktiven hinzu:
   
        using Microsoft.Azure;
        using Microsoft.WindowsAzure.Storage;
        using Microsoft.WindowsAzure.Storage.Queue;

2. Rufen Sie ein **CloudStorageAccount** -Objekt ab, das die Informationen zu Ihrem Speicherkonto enthält. Verwenden Sie den folgenden Code, um die Speicherverbindungszeichenfolge und Speicherkontoinformationen aus der Azure-Dienstkonfiguration abzurufen. (Ändern Sie *storage-account-name>* in den Namen des Azure-Speicherkontos, auf das Sie zugreifen.)

         CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
           CloudConfigurationManager.GetSetting("<storage-account-name>_AzureStorageConnectionString"));

3. Rufen Sie ein **CloudQueueClient**-Objekt ab, das einen Warteschlangendienstclient darstellt.

        CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();

4. Rufen Sie ein **CloudQueue**-Objekt ab, das einen Verweis auf die Warteschlange darstellt. (Ändern Sie *<queue-name>* in den Namen der Warteschlange, deren Länge Sie abfragen.)

        CloudQueue queue = queueClient.GetQueueReference(<queue-name>);

5. Rufen Sie die **CloudQueue.Delete**-Methode zum Löschen der Warteschlange auf, die vom **CloudQueue**-Objekt dargestellt wird.

        messageQueue.Delete();

## <a name="next-steps"></a>Nächste Schritte
[!INCLUDE [vs-storage-dotnet-queues-next-steps](../../includes/vs-storage-dotnet-queues-next-steps.md)]




<!--HONumber=Dec16_HO2-->


