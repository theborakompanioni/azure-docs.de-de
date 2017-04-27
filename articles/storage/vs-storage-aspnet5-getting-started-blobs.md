---
title: Erste Schritte mit Blob Storage und verbundenen Visual Studio-Diensten (ASP.NET Core) | Microsoft-Dokumentation
description: "Informieren Sie sich über die ersten Schritte mit Azure Blob Storage in einem ASP.NET Core-Projekt in Visual Studio, nachdem Sie mithilfe von verbundenen Visual Studio-Diensten ein Speicherkonto erstellt haben."
services: storage
documentationcenter: 
author: TomArcher
manager: douge
editor: 
ms.assetid: 094b596a-c92c-40c4-a0f5-86407ae79672
ms.service: storage
ms.workload: web
ms.tgt_pltfrm: vs-getting-started
ms.devlang: na
ms.topic: article
ms.date: 12/02/2016
ms.author: tarcher
translationtype: Human Translation
ms.sourcegitcommit: 988e7fe2ae9f837b661b0c11cf30a90644085e16
ms.openlocfilehash: e725015c8be7ecfa908f0ae75986b73f218fa3ae
ms.lasthandoff: 04/06/2017


---
# <a name="get-started-with-azure-blob-storage-and-visual-studio-connected-services-aspnet-core"></a>Erste Schritte mit Azure Blob Storage und verbundenen Visual Studio-Diensten (ASP.NET Core)
[!INCLUDE [storage-try-azure-tools-blobs](../../includes/storage-try-azure-tools-blobs.md)]

## <a name="overview"></a>Übersicht
Dieser Artikel beschreibt die ersten Schritte beim Verwenden von Azure Blob Storage in Visual Studio, nachdem Sie über das Visual Studio-Dialogfeld „Verbundene Dienste hinzufügen“ in einem ASP.NET Core-Projekt ein Azure Storage-Konto erstellt oder auf ein solches Konto verwiesen haben.

Die BLOB-Speicherung in Azure ist ein Dienst zur Speicherung großer Mengen unstrukturierter Daten, auf die von überall auf der Welt über HTTP oder HTTPS zugegriffen werden kann. Ein einzelner BLOB kann eine beliebige Größe aufweisen. BLOBs können Bilder, Audio- und Videodateien, Rohdaten und Dokumentdateien enthalten. Dieser Artikel beschreibt die ersten Schritte mit dem Blob Storage, nachdem Sie über das Visual Studio-Dialogfeld **Verbundene Dienste hinzufügen** ein Azure Storage-Konto in einem ASP.NET Core-Projekt erstellt haben.

Dateien werden in Ordnern gespeichert, Speicher-BLOBs analog dazu in Containern. Nachdem Sie einen Speicher erstellt haben, erstellen Sie mindestens einen Container in dem Speicher. Beispielsweise können Sie in einem Speicher namens „Notizbuch“ Container im Speicher namens „Bilder“ erstellen, um Bilder zu speichern, und einen anderen namens „Audio“ erstellen, um Audiodateien zu speichern. Nachdem Sie die Container erstellt haben, können Sie einzelne BLOB-Dateien in diese hochladen. Weitere Informationen zum programmgesteuerten Bearbeiten von Blobs finden Sie unter [Erste Schritte mit Azure Blob Storage mit .NET](storage-dotnet-how-to-use-blobs.md) .

## <a name="access-blob-containers-in-code"></a>Zugreifen auf BLOB-Container in Code
Für den programmgesteuerten Zugriff auf Blobs in ASP.NET Core-Projekten müssen Sie die folgenden Elemente hinzuzufügen, wenn sie nicht bereits vorhanden sind.

1. Fügen Sie die folgenden Namespace-Deklarationen zum Anfang aller C#-Dateien hinzu, in denen Sie programmgesteuert auf den Azure-Speicher zugreifen möchten:
   
        using Microsoft.Extensions.Configuration;
        using Microsoft.WindowsAzure.Storage;
        using Microsoft.WindowsAzure.Storage.Blob;
        using System.Threading.Tasks;
        using LogLevel = Microsoft.Extensions.Logging.LogLevel;
2. Rufen Sie ein **CloudStorageAccount** -Objekt ab, das die Informationen zu Ihrem Speicherkonto enthält. Verwenden Sie den folgenden Code, um Ihre Speicherverbindungszeichenfolge und Speicherkontoinformationen aus der Azure-Dienstkonfiguration abzurufen.
   
         CloudStorageAccount storageAccount = new CloudStorageAccount(
            new Microsoft.WindowsAzure.Storage.Auth.StorageCredentials(
            "<storage-account-name>",
            "<access-key>"), true);
   
    **HINWEIS:** Verwenden Sie den gesamten obigen Code vor dem Code in den folgenden Abschnitten.
3. Verwenden Sie ein **CloudBlobClient**-Objekt zum Abrufen eines **CloudBlobContainer**-Verweises auf einen Container, der in Ihrem Speicherkonto vorhanden ist.
   
        // Create a blob client.
        CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();
   
        // Get a reference to a container named "mycontainer."
        CloudBlobContainer container = blobClient.GetContainerReference("mycontainer");

## <a name="create-a-container-in-code"></a>Erstellen eines Containers in Code
Sie können den **CloudBlobClient** auch zum Erstellen eines Containers in Ihrem Speicherkonto verwenden. Dazu müssen Sie lediglich wie im folgenden Code einen Aufruf von **CreateIfNotExistsAsync** hinzufügen:

    // Create a blob client.
    CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

    // Get a reference to a container named "my-new-container."
    CloudBlobContainer container = blobClient.GetContainerReference("my-new-container");

    // If "mycontainer" doesn't exist, create it.
    await container.CreateIfNotExistsAsync();


**HINWEIS:** Die APIs, die Aufrufe zum Azure Storage in ASP.NET Core ausführen, sind asynchron. Unter [Asynchrone Programmierung mit Async und Await](http://msdn.microsoft.com/library/hh191443.aspx) finden Sie weitere Informationen. Im folgenden Code wird die Programmierung mit Async-Methoden angenommen.

Um die Dateien im Container für alle Benutzer zur Verfügung zu stellen, können Sie den Container mithilfe des folgenden Codes als öffentlich festlegen.

    await container.SetPermissionsAsync(new BlobContainerPermissions
    {
        PublicAccess = BlobContainerPublicAccessType.Blob
    });

## <a name="upload-a-blob-into-a-container"></a>Hochladen eines Blobs in einen Container
Rufen Sie einen Containerverweis ab, und verwenden Sie diesen dann zum Abrufen eines BLOB-Verweises, um eine BLOB-Datei in einen Container hochzuladen. Sobald Sie über einen BLOB-Verweis verfügen, können Sie jeden Datenstrom in diesen hochladen, indem Sie die **UploadFromStreamAsync()** -Methode aufrufen. Dieser Vorgang erstellt den Blob, wenn dieser noch nicht vorhanden ist, oder überschreibt ihn, wenn er vorhanden ist. Im folgenden Beispiel wird gezeigt, wie ein Blob in einen bereits erstellten Container hochgeladen wird.

    // Get a reference to a blob named "myblob".
    CloudBlockBlob blockBlob = container.GetBlockBlobReference("myblob");

    // Create or overwrite the "myblob" blob with the contents of a local file
    // named "myfile".
    using (var fileStream = System.IO.File.OpenRead(@"path\myfile"))
    {
        await blockBlob.UploadFromStreamAsync(fileStream);
    }

## <a name="list-the-blobs-in-a-container"></a>Auflisten der Blobs in einem Container
Um die Blobs in einem Container aufzuführen, müssen Sie zuerst einen Containerverweis abrufen. Anschließend können Sie die **ListBlobsSegmentedAsync()** -Methode des Containers aufrufen, um die darin enthaltenen BLOBs und/oder Verzeichnisse abzurufen. Damit Sie auf die zahlreichen Eigenschaften und Methoden für ein zurückgegebenes **IListBlobItem**-Objekt zugreifen können, müssen Sie es in ein **CloudBlockBlob**-, **CloudPageBlob**- oder **CloudBlobDirectory**-Objekt umwandeln. Wenn der Blob-Typ unbekannt ist, können Sie eine Typüberprüfung ausführen, um zu bestimmen, in welchen Typ die Umwandlung erfolgen soll. Im folgenden Code wird gezeigt, wie der URI der einzelnen Elemente in einem Container abgerufen und ausgegeben wird:

    BlobContinuationToken token = null;
    do
    {
        BlobResultSegment resultSegment = await container.ListBlobsSegmentedAsync(token);
        token = resultSegment.ContinuationToken;

        foreach (IListBlobItem item in resultSegment.Results)
        {
            if (item.GetType() == typeof(CloudBlockBlob))
            {
                CloudBlockBlob blob = (CloudBlockBlob)item;
                Console.WriteLine("Block blob of length {0}: {1}", blob.Properties.Length, blob.Uri);
            }

            else if (item.GetType() == typeof(CloudPageBlob))
            {
                CloudPageBlob pageBlob = (CloudPageBlob)item;

                Console.WriteLine("Page blob of length {0}: {1}", pageBlob.Properties.Length, pageBlob.Uri);
            }

            else if (item.GetType() == typeof(CloudBlobDirectory))
            {
                CloudBlobDirectory directory = (CloudBlobDirectory)item;

                Console.WriteLine("Directory: {0}", directory.Uri);
            }
        }
    } while (token != null);

Es gibt auch andere Möglichkeiten, den Inhalt eines BLOB-Containers aufzulisten. Weitere Informationen finden Sie unter [Erste Schritte mit Azure Blob Storage mit .NET](storage-dotnet-how-to-use-blobs.md#list-the-blobs-in-a-container) .

## <a name="download-a-blob"></a>Herunterladen eines Blobs
Wenn Sie ein Blob herunterladen möchten, rufen Sie zuerst einen Verweis auf das Blob ab, und rufen Sie dann die **DownloadToStreamAsync()** -Methode auf. Im folgenden Beispiel wird die **DownloadToStreamAsync()** -Methode verwendet, um den Inhalt des BLOBs in ein Datenstromobjekt zu übertragen, das anschließend als eine lokale Datei gespeichert werden kann.

    // Get a reference to a blob named "photo1.jpg".
    CloudBlockBlob blockBlob = container.GetBlockBlobReference("photo1.jpg");

    // Save the blob contents to a file named "myfile".
    using (var fileStream = System.IO.File.OpenWrite(@"path\myfile"))
    {
        await blockBlob.DownloadToStreamAsync(fileStream);
    }

Es gibt auch andere Möglichkeiten zum Speichern von BLOBs als Dateien. Weitere Informationen finden Sie unter [Erste Schritte mit Azure Blob Storage mit .NET](storage-dotnet-how-to-use-blobs.md#download-blobs) .

## <a name="delete-a-blob"></a>Löschen eines Blobs
Wenn Sie einen BLOB löschen möchten, rufen Sie zuerst einen Verweis auf den BLOB ab, und rufen Sie dann die **DeleteAsync()** -Methode auf.

    // Get a reference to a blob named "myblob.txt".
    CloudBlockBlob blockBlob = container.GetBlockBlobReference("myblob.txt");

    // Delete the blob.
    await blockBlob.DeleteAsync();

## <a name="next-steps"></a>Nächste Schritte
[!INCLUDE [vs-storage-dotnet-blobs-next-steps](../../includes/vs-storage-dotnet-blobs-next-steps.md)]


