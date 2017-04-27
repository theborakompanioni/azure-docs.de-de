---
title: Entwickeln von Azure Functions mit Media Services
description: In diesem Thema wird gezeigt, wie Sie im Azure-Portal mit dem Entwickeln von Azure Functions mit Media Services beginnen.
services: media-services
documentationcenter: 
author: juliako
manager: erikre
editor: 
ms.assetid: 51bdcb01-1846-4e1f-bd90-70020ab471b0
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 03/21/2017
ms.author: juliako
translationtype: Human Translation
ms.sourcegitcommit: 785d3a8920d48e11e80048665e9866f16c514cf7
ms.openlocfilehash: ca7c89dc5f56f1bf4c300874111e65048e88abcf
ms.lasthandoff: 04/12/2017


---
#<a name="develop-azure-functions-with-media-services"></a>Entwickeln von Azure Functions mit Media Services

In diesem Thema wird beschrieben, wie Sie im Azure-Portal mit dem Entwickeln von Azure Functions mit Media Services beginnen. 

Sie können auch vorhandene [Media Services Azure Functions](https://github.com/Azure-Samples/media-services-dotnet-functions-integration) bereitstellen, indem Sie die Schaltfläche zum Bereitstellen in Azure (**Deploy to Azure**) verwenden. Dieses Repository enthält Beispiele für Azure Functions, in denen Azure Media Services verwendet werden. Hiermit werden Workflows veranschaulicht, bei denen es um das Erfassen von Inhalten direkt aus dem Blobspeicher, die Codierung und das Rückschreiben von Inhalt in den Blobspeicher geht. Außerdem sind Beispiele dafür vorhanden, wie Sie Auftragsbenachrichtigungen über Webhooks und Azure-Warteschlangen überwachen. Sie können Ihre Funktionen auch basierend auf den Beispielen im Repository der [Media Services Azure Functions](https://github.com/Azure-Samples/media-services-dotnet-functions-integration) entwickeln. 

In diesem Thema wird veranschaulicht, wie Sie mit der Erstellung von Azure Functions beginnen, für die Media Services verwendet werden. Die in diesem Thema definierte Azure Function überwacht einen Speicherkontocontainer mit dem Namen **input** für neue MP4-Dateien. Sobald eine Datei in den Speichercontainer abgelegt wird, führt der BLOB-Trigger die Funktion aus.

## <a name="prerequisites"></a>Voraussetzungen

- Bevor Sie Ihre erste Funktion erstellen können, müssen Sie über ein aktives Azure-Konto verfügen. Wenn Sie noch kein Azure-Konto haben, [erstellen Sie ein kostenloses Konto](https://azure.microsoft.com/free/).
- Es ist ratsam, wie [hier](media-services-portal-create-account.md) beschrieben ein AMS-Konto zu erstellen, wenn Sie Azure Functions erstellen möchten, mit denen Aktionen in Ihrem AMS-Konto (Azure Media Services) durchgeführt werden oder auf von Media Services gesendete Ereignisse gelauscht wird.
- Erfahrung beim [Verwenden von Azure Functions](../azure-functions/functions-overview.md). Lesen Sie auch folgende Artikel:
    - [HTTP- und Webhookbindungen in Azure Functions](../azure-functions/functions-triggers-bindings.md)
    - [Konfigurieren von Azure Funktionen-App-Einstellungen](../azure-functions/functions-how-to-use-azure-function-app-settings.md)
    
## <a name="considerations"></a>Überlegungen

-  Wird Azure Functions unter dem Verbrauchsplan ausgeführt, gilt ein Timeout von 5 Minuten.

## <a name="create-a-function-app"></a>Erstellen einer Funktionen-App

1. Wechseln Sie zum [Azure-Portal](http://portal.azure.com) , und melden Sie sich mit Ihrem Azure-Konto an.
2. Erstellen Sie wie [hier](../azure-functions/functions-create-function-app-portal.md) beschrieben eine Funktionen-App.

>[!NOTE]
> Ein Speicherkonto, das Sie in der Umgebungsvariablen **StorageConnection** angeben (siehe nächster Schritt), sollte sich in der gleichen Region wie Ihre App befinden.

## <a name="configure-function-app-settings"></a>Konfigurieren von Einstellungen der Funktionen-App

Beim Entwickeln von Media Services-Funktionen ist es nützlich, Umgebungsvariablen hinzuzufügen, die in Ihren Funktionen verwendet werden. Klicken Sie zum Konfigurieren von App-Einstellungen auf den Link „App-Einstellungen konfigurieren“. Weitere Informationen finden Sie unter [Konfigurieren der Einstellungen der Azure Function-App](../azure-functions/functions-how-to-use-azure-function-app-settings.md). 

Beispiel:

![Einstellungen](./media/media-services-azure-functions/media-services-azure-functions001.png)

Bei der in diesem Artikel definierte Funktion wird davon ausgegangen, dass Sie die folgenden Umgebungsvariablen in den App-Einstellungen definiert haben:

**AMSAccount** : *AMS-Kontoname* (z.B. testams)

**AMSKey** : *AMS-Kontoschlüssel* (z.B. IHOySnH+XX3LGPfraE5fKPl0EnzvEPKkOPKCr59aiMM=)

**MediaServicesStorageAccountName** : *Speicherkontoname* (z.B. testamsstorage)

**MediaServicesStorageAccountKey** : *Speicherkontoschlüssel* (z.B. xx7RN7mvpcipkuXvn5g7jwxnKh5MwYQ/awZAzkSIxQA8tmCtn93rqobjgjt41Wb0zwTZWeWQHY5kSZF0XXXXXX==)

**StorageConnection** : *Speicherverbindung* (z.B. DefaultEndpointsProtocol=https;AccountName=testamsstorage;AccountKey=xx7RN7mvpcipkuXvn5g7jwxnKh5MwYQ/awZAzkSIxQA8tmCtn93rqobjgjt41Wb0zwTZWeWQHY5kSZF0XXXXX==)

## <a name="create-a-function"></a>Erstellen einer Funktion

Nachdem die Funktionen-App bereitgestellt wurde, wird sie unter den Azure Functions von **App Services** aufgeführt.

1. Wählen Sie Ihre Funktionen-App aus, und klicken Sie auf **Neue Funktion**.
2. Wählen Sie die Sprache **C#** und das Szenario **Datenverarbeitung** aus.
3. Wählen Sie die Vorlage **BlobTrigger** aus. Diese Funktion wird ausgelöst, wenn ein BLOB in den **input**-Container hochgeladen wird. Der Name **input** wird unter **Path** im nächsten Schritt festgelegt.

    ![files](./media/media-services-azure-functions/media-services-azure-functions004.png)

4. Nachdem Sie **BlobTrigger** ausgewählt haben, werden auf der Seite einige weitere Steuerelemente angezeigt.

    ![files](./media/media-services-azure-functions/media-services-azure-functions005.png)

4. Klicken Sie auf **Erstellen**. 


## <a name="files"></a>Dateien

Ihre Azure-Funktion ist Codedateien und anderen Dateien zugeordnet, die in diesem Abschnitt beschrieben werden. Standardmäßig ist eine Funktion den Dateien **function.json** und **run.csx** (C#) zugeordnet. Sie müssen die Datei **project.json** hinzufügen. Im restlichen Teil dieses Abschnitts werden die Definitionen für diese Dateien angezeigt.

![files](./media/media-services-azure-functions/media-services-azure-functions003.png)

### <a name="functionjson"></a>function.json

Die Datei „function.json“ definiert die Funktionsbindungen und weitere Konfigurationseinstellungen. Die Laufzeit verwendet diese Datei, um zu ermitteln, welche Ereignisse überwacht werden sollen und wie Daten in die Funktionsausführung übergeben und aus dieser zurückgegeben werden. Weitere Informationen finden Sie unter [HTTP- und Webhookbindungen in Azure Functions](../azure-functions/functions-reference.md#function-code).

>[!NOTE]
>Legen Sie die **disabled**-Eigenschaft auf **true** fest, um die Ausführung der Funktion zu verhindern. 


Hier ist ein Beispiel für die Datei **function.json** angegeben.

    {
    "bindings": [
      {
        "name": "myBlob",
        "type": "blobTrigger",
        "direction": "in",
        "path": "input/{fileName}.mp4",
        "connection": "StorageConnection"
      }
    ],
    "disabled": false
    }

### <a name="projectjson"></a>project.json

Die Datei „project.json“ enthält die Abhängigkeiten. Hier sehen Sie ein Beispiel für die Datei **project.json**, das die erforderlichen .NET Azure Media Services-Pakete aus Nuget enthält. Beachten Sie, dass sich die Versionsnummern mit den aktuellen Updates für die Pakete ändert, daher sollten Sie überprüfen, ob die aktuellste Version vorliegt. 

    {
      "frameworks": {
        "net46":{
          "dependencies": {
        "windowsazure.mediaservices": "3.8.0.5",
        "windowsazure.mediaservices.extensions": "3.8.0.3"
          }
        }
       }
    }
    
### <a name="runcsx"></a>run.csx

Dies ist der C#-Code für Ihre Funktion.  Die unten definierte Funktion überwacht einen Speicherkontocontainer mit dem Namen **input** (so wie im Pfad angegeben) für neue MP4-Dateien. Sobald eine Datei in den Speichercontainer abgelegt wird, führt der BLOB-Trigger die Funktion aus.
    
Das in diesem Abschnitt definierte Beispiel veranschaulicht Folgendes: 

1. Erfassen eines Medienobjekts in einem Media Services-Konto (durch Kopieren eines Blobs in ein AMS-Medienobjekt) und 
2. Übermitteln eines Codierungsauftrags, der die Voreinstellung „Adaptives Streaming“ von Media Encoder Standard verwendet

In der Praxis möchten Sie wahrscheinlich den Auftragsstatus nachverfolgen und Ihr codiertes Medienobjekt dann veröffentlichen. Weitere Informationen finden Sie unter [Verwenden von Azure-Webhooks zum Überwachen von Media Services-Auftragsbenachrichtigungen](media-services-dotnet-check-job-progress-with-webhooks.md). Weitere Beispiele finden Sie unter [Media Services Azure Functions](https://github.com/Azure-Samples/media-services-dotnet-functions-integration).  

Klicken Sie auf **Speichern und ausführen**, nachdem Sie die Funktion definiert haben.

    #r "Microsoft.WindowsAzure.Storage"
    #r "Newtonsoft.Json"
    #r "System.Web"

    using System;
    using Microsoft.WindowsAzure.MediaServices.Client;
    using System.Collections.Generic;
    using System.Linq;
    using System.Text;
    using System.Net;
    using System.Threading;
    using System.Threading.Tasks;
    using System.IO;
    using Microsoft.WindowsAzure.Storage;
    using Microsoft.WindowsAzure.Storage.Blob;
    using Microsoft.WindowsAzure.Storage.Auth;

    private static readonly string _mediaServicesAccountName = Environment.GetEnvironmentVariable("AMSAccount");
    private static readonly string _mediaServicesAccountKey = Environment.GetEnvironmentVariable("AMSKey");

    static string _storageAccountName = Environment.GetEnvironmentVariable("MediaServicesStorageAccountName");
    static string _storageAccountKey = Environment.GetEnvironmentVariable("MediaServicesStorageAccountKey");

    private static CloudStorageAccount _destinationStorageAccount = null;

    // Field for service context.
    private static CloudMediaContext _context = null;
    private static MediaServicesCredentials _cachedCredentials = null;

    public static void Run(CloudBlockBlob myBlob, string fileName, TraceWriter log)
    {
        // NOTE that the variables {fileName} here come from the path setting in function.json
        // and are passed into the  Run method signature above. We can use this to make decisions on what type of file
        // was dropped into the input container for the function. 

        // No need to do any Retry strategy in this function, By default, the SDK calls a function up to 5 times for a 
        // given blob. If the fifth try fails, the SDK adds a message to a queue named webjobs-blobtrigger-poison.

        log.Info($"C# Blob trigger function processed: {fileName}.mp4");
        log.Info($"Using Azure Media Services account : {_mediaServicesAccountName}");


        try
        {
        // Create and cache the Media Services credentials in a static class variable.
        _cachedCredentials = new MediaServicesCredentials(
                _mediaServicesAccountName,
                _mediaServicesAccountKey);

        // Used the chached credentials to create CloudMediaContext.
        _context = new CloudMediaContext(_cachedCredentials);

        // Step 1:  Copy the Blob into a new Input Asset for the Job
        // ***NOTE: Ideally we would have a method to ingest a Blob directly here somehow. 
        // using code from this sample - https://azure.microsoft.com/en-us/documentation/articles/media-services-copying-existing-blob/

        StorageCredentials mediaServicesStorageCredentials =
            new StorageCredentials(_storageAccountName, _storageAccountKey);

        IAsset newAsset = CreateAssetFromBlob(myBlob, fileName, log).GetAwaiter().GetResult();

        // Step 2: Create an Encoding Job

        // Declare a new encoding job with the Standard encoder
        IJob job = _context.Jobs.Create("Azure Function - MES Job");

        // Get a media processor reference, and pass to it the name of the 
        // processor to use for the specific task.
        IMediaProcessor processor = GetLatestMediaProcessorByName("Media Encoder Standard");

        // Create a task with the encoding details, using a custom preset
        ITask task = job.Tasks.AddNew("Encode with Adaptive Streaming",
            processor,
            "Adaptive Streaming",
            TaskOptions.None); 

        // Specify the input asset to be encoded.
        task.InputAssets.Add(newAsset);

        // Add an output asset to contain the results of the job. 
        // This output is specified as AssetCreationOptions.None, which 
        // means the output asset is not encrypted. 
        task.OutputAssets.AddNew(fileName, AssetCreationOptions.None);

        job.Submit();
        log.Info("Job Submitted");

        }
        catch (Exception ex)
        {
        log.Error("ERROR: failed.");
        log.Info($"StackTrace : {ex.StackTrace}");
        throw ex;
        }
    }

    private static IMediaProcessor GetLatestMediaProcessorByName(string mediaProcessorName)
    {
        var processor = _context.MediaProcessors.Where(p => p.Name == mediaProcessorName).
        ToList().OrderBy(p => new Version(p.Version)).LastOrDefault();

        if (processor == null)
        throw new ArgumentException(string.Format("Unknown media processor", mediaProcessorName));

        return processor;
    }


    public static async Task<IAsset> CreateAssetFromBlob(CloudBlockBlob blob, string assetName, TraceWriter log){
        IAsset newAsset = null;

        try{
            Task<IAsset> copyAssetTask = CreateAssetFromBlobAsync(blob, assetName, log);
            newAsset = await copyAssetTask;
            log.Info($"Asset Copied : {newAsset.Id}");
        }
        catch(Exception ex){
            log.Info("Copy Failed");
            log.Info($"ERROR : {ex.Message}");
            throw ex;
        }

        return newAsset;
    }

    /// <summary>
    /// Creates a new asset and copies blobs from the specifed storage account.
    /// </summary>
    /// <param name="blob">The specified blob.</param>
    /// <returns>The new asset.</returns>
    public static async Task<IAsset> CreateAssetFromBlobAsync(CloudBlockBlob blob, string assetName, TraceWriter log)
    {
         //Get a reference to the storage account that is associated with the Media Services account. 
        StorageCredentials mediaServicesStorageCredentials =
        new StorageCredentials(_storageAccountName, _storageAccountKey);
        _destinationStorageAccount = new CloudStorageAccount(mediaServicesStorageCredentials, false);

        // Create a new asset. 
        var asset = _context.Assets.Create(blob.Name, AssetCreationOptions.None);
        log.Info($"Created new asset {asset.Name}");

        IAccessPolicy writePolicy = _context.AccessPolicies.Create("writePolicy",
        TimeSpan.FromHours(4), AccessPermissions.Write);
        ILocator destinationLocator = _context.Locators.CreateLocator(LocatorType.Sas, asset, writePolicy);
        CloudBlobClient destBlobStorage = _destinationStorageAccount.CreateCloudBlobClient();

        // Get the destination asset container reference
        string destinationContainerName = (new Uri(destinationLocator.Path)).Segments[1];
        CloudBlobContainer assetContainer = destBlobStorage.GetContainerReference(destinationContainerName);

        try{
        assetContainer.CreateIfNotExists();
        }
        catch (Exception ex)
        {
        log.Error ("ERROR:" + ex.Message);
        }

        log.Info("Created asset.");

        // Get hold of the destination blob
        CloudBlockBlob destinationBlob = assetContainer.GetBlockBlobReference(blob.Name);

        // Copy Blob
        try
        {
        using (var stream = await blob.OpenReadAsync()) 
        {            
            await destinationBlob.UploadFromStreamAsync(stream);          
        }

        log.Info("Copy Complete.");

        var assetFile = asset.AssetFiles.Create(blob.Name);
        assetFile.ContentFileSize = blob.Properties.Length;
        assetFile.IsPrimary = true;
        assetFile.Update();
        asset.Update();
        }
        catch (Exception ex)
        {
        log.Error(ex.Message);
        log.Info (ex.StackTrace);
        log.Info ("Copy Failed.");
        throw;
        }

        destinationLocator.Delete();
        writePolicy.Delete();

        return asset;
    }
##<a name="test-your-function"></a>Testen der Funktion

Um die Funktion zu testen, müssen Sie eine MP4-Datei in den **input**-Container des Speicherkontos hochladen, das Sie in der Verbindungszeichenfolge angegeben haben.  

## <a name="next-step"></a>Nächster Schritt

Nun können Sie mit der Entwicklung einer Mediendienste-Anwendung beginnen. 
 
Weitere Details und umfassende Beispiele/Lösungen zur Verwendung von Azure Functions und Logic Apps mit Azure Media Services zum Erstellen von benutzerdefinierten Inhaltserstellungsworkflows finden Sie unter [Media Services .NET Functions Integraiton Sample on GitHub](https://github.com/Azure-Samples/media-services-dotnet-functions-integration) (Beispiel zur Integration von Media Services .NET-Funktionen in GitHub).

Lesen Sie auch [Verwenden von Azure-Webhooks zum Überwachen von Media Services-Auftragsbenachrichtigungen mit .NET](media-services-dotnet-check-job-progress-with-webhooks.md). 

## <a name="media-services-learning-paths"></a>Media Services-Lernpfade
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Feedback geben
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]


