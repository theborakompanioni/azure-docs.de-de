---
title: Kopieren von Blobs aus einem Speicherkonto in ein Azure Media Services-Medienobjekt | Microsoft-Dokumentation
description: "In diesem Thema wird erläutert, wie ein vorhandener Blob in ein Media Services-Medienobjekt kopiert wird. Im folgenden Beispiel werden Azure Media Services .NET SDK-Erweiterungen verwendet."
services: media-services
documentationcenter: 
author: Juliako
manager: erikre
editor: 
ms.assetid: 6a63823f-f3c9-424c-91b8-566f70bec346
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: ne
ms.topic: article
ms.date: 01/31/2017
ms.author: juliako
translationtype: Human Translation
ms.sourcegitcommit: c1cd1450d5921cf51f720017b746ff9498e85537
ms.openlocfilehash: 14bdbe593d8c6ec5297ce37ce57b759deb533cec
ms.lasthandoff: 03/14/2017


---
# <a name="copying-existing-blobs-into-a-media-services-asset"></a>Kopieren vorhandener Blobs in ein Media Services-Medienobjekt
In diesem Thema wird erläutert, wie Blobs mithilfe von [Azure Media Services .NET SDK-Erweiterungen](https://github.com/Azure/azure-sdk-for-media-services-extensions/) aus einem Speicherkonto in ein neues Azure Media Services-Medienobjekt (AMS) kopiert werden.

Die Erweiterungsmethoden können mit Folgendem verwendet werden:

- Normalen Medienobjekten
- Livearchivobjekten (FragBlob-Format).
- Quell- und Zielmedienobjekten, die zu unterschiedlichen Media Services-Konten (sogar in verschiedenen Rechenzentren) gehören Allerdings fallen dabei möglicherweise Kosten an. Die Preisübersicht und weitere Informationen finden Sie unter [Azure-Preise](https://azure.microsoft.com/pricing/#header-11).

> [!NOTE]
> Sie sollten nicht versuchen, den Inhalt von BLOB-Containern, die mit Media Services generiert wurden, ohne die Verwendung von Media Service-APIs zu ändern.
> 

In diesem Thema werden zwei Codebeispiele gezeigt:

1. Kopieren von Blobs aus einem Medienobjekt in einem AMS-Konto in ein neues Medienobjekt in einem anderen AMS-Konto
2. Kopieren von Blobs aus einem Speicherkonto in ein neues Medienobjekt in einem AMS-Konto

## <a name="copy-blobs-between-two-ams-accounts"></a>Kopieren von Blobs zwischen zwei AMS-Konten  

### <a name="prerequisites"></a>Voraussetzungen

Zwei Media Services-Konten Weitere Informationen finden Sie unter [Erstellen eines Media Services-Kontos](media-services-portal-create-account.md).

### <a name="download-sample"></a>Beispiel herunterladen
Sie können die Schritte in diesem Artikel befolgen oder ein Beispiel mit dem in diesem Artikel beschriebenen Code [hier](https://azure.microsoft.com/documentation/samples/media-services-dotnet-copy-blob-into-asset/) herunterladen.

### <a name="set-up-your-project"></a>Einrichten des Projekts

1. Verwenden Sie Visual Studio, um eine neue Projektmappe zu erstellen, die das C#-Konsolenanwendungsprojekt enthält. 
3. Verwenden Sie [NuGet](https://www.nuget.org/packages/windowsazure.mediaservices.extensions), um die Azure Media Services .NET SDK-Erweiterungen (windowsazure.mediaservices.extensions) zu installieren. Durch Installieren dieses Pakets werden auch das Media Services .NET SDK installiert und alle anderen erforderlichen Abhängigkeiten hinzugefügt.
4. Fügen Sie andere Verweise hinzu, die für dieses Projekt erforderlich sind: System.Configuration.
6. Fügen Sie der CONFIG-Datei den Abschnitt „appSettings“ hinzu, und ändern Sie die Werte entsprechend Ihren Media Services-Konten, dem Zielspeicherkonto und der Quellmedienobjekt-ID. 
   
        <appSettings>
              <add key="MediaServicesSourceAccountName" value="name"/>
              <add key="MediaServicesSourceAccountKey" value="key"/>
              <add key="MediaServicesDestAccountName" value="name"/>
              <add key="MediaServicesDestAccountKey" value="key"/>
              <add key="DestStorageAccountName" value="name"/>
              <add key="DestStorageAccountKey" value="key"/>
              <add key="SourceAssetID" value="nb:cid:UUID:assetID"/>       
        </appSettings>

### <a name="copy-blobs-from-an-asset-in-one-ams-account-into-an-asset-in-another-ams-account"></a>Kopieren von Blobs aus einem Medienobjekt in einem AMS-Konto in ein Medienobjekt in einem anderen AMS-Konto

Der folgende Code verwendet die Erweiterungsmethode **IAsset.Copy**, um alle Dateien im Quellmedienobjekt mithilfe einer einzelnen Erweiterung in das Zielmedienobjekt zu kopieren. Bei asynchroner Unterstützung fällt zusätzlicher Verarbeitungsaufwand an.

    using System;
    using System.Linq;
    using System.Configuration;
    using Microsoft.WindowsAzure.MediaServices.Client;
    using Microsoft.WindowsAzure.Storage.Auth;
    
    namespace CopyExistingBlobsIntoAsset
    {
        class Program
        {
            static string _sourceAaccountName = ConfigurationManager.AppSettings["MediaServicesSourceAccountName"];
            static string _sourceAccountKey = ConfigurationManager.AppSettings["MediaServicesSourceAccountKey"];
            static string _destAccountName = ConfigurationManager.AppSettings["MediaServicesDestAccountName"];
            static string _destAccountKey = ConfigurationManager.AppSettings["MediaServicesDestAccountKey"];
            static string _destStorageAccountName = ConfigurationManager.AppSettings["DestStorageAccountName"];
            static string _destStorageAccountKey = ConfigurationManager.AppSettings["DestStorageAccountKey"];
            static string _sourceAssetID = ConfigurationManager.AppSettings["SourceAssetID"];
            
    
            private static CloudMediaContext _sourceContext = null;
            private static CloudMediaContext _destContext = null;
    
            static void Main(string[] args)
            {
                // Create the context for your source Media Services account.
    
                // Use the cached credentials to create CloudMediaContext.
                _sourceContext = new CloudMediaContext(new MediaServicesCredentials(
                    _sourceAaccountName,
                    _sourceAccountKey));

                // Create the context for your destination Media Services account.
                _destContext = new CloudMediaContext(new MediaServicesCredentials(
                    _destAccountName,
                    _destAccountKey));
    
                // Get the credentials of the default Storage account bound to your destination Media Services account.
                StorageCredentials destinationStorageCredentials =
                    new StorageCredentials(_destStorageAccountName, _destStorageAccountKey);
    
                // Get a reference to the source asset in the source context.
                IAsset sourceAsset = _sourceContext.Assets.Where(a => a.Id == _sourceAssetID).First();
    
                // Create an empty destination asset in the destination context.
                IAsset destinationAsset = _destContext.Assets.Create(sourceAsset.Name, AssetCreationOptions.None);
    
                // Copy the files in the source asset instance into the destination asset instance.
                // There is an additional overload with async support.
                sourceAsset.Copy(destinationAsset, destinationStorageCredentials);

                
            }
        }
    }


## <a name="copy-blobs-from-a-storage-account-into-an-ams-account"></a>Kopieren von Blobs aus einem Speicherkonto in ein AMS-Konto 

### <a name="prerequisites"></a>Voraussetzungen

- Ein Speicherkonto, aus dem Blobs kopiert werden sollen
- Ein AMS-Konto, in das Blobs kopiert werden sollen

### <a name="set-up-your-project"></a>Einrichten des Projekts

1. Verwenden Sie Visual Studio, um eine neue Projektmappe zu erstellen, die das C#-Konsolenanwendungsprojekt enthält. 
3. Verwenden Sie [NuGet](https://www.nuget.org/packages/windowsazure.mediaservices.extensions), um die Azure Media Services .NET SDK-Erweiterungen (windowsazure.mediaservices.extensions) zu installieren. Durch Installieren dieses Pakets werden auch das Media Services .NET SDK installiert und alle anderen erforderlichen Abhängigkeiten hinzugefügt.
4. Fügen Sie andere Verweise hinzu, die für dieses Projekt erforderlich sind: System.Configuration.
6. Fügen Sie der CONFIG-Datei den appSettings-Abschnitt hinzu, und ändern Sie die Werte basierend auf dem Quellspeicher und den AMS-Zielkonten.
   
          <appSettings>
            <add key="MediaServicesAccountName" value="name" />
            <add key="MediaServicesAccountKey" value="key" />
            <add key="MediaServicesStorageAccountName" value="name" />
            <add key="MediaServicesStorageAccountKey" value="key" />
            <add key="SourceStorageAccountName" value="name" />
            <add key="SourceStorageAccountKey" value="key" />
          </appSettings>

### <a name="copy-blobs-from-some-storage-account-into-a-new-asset-in-a-ams-account"></a>Kopieren von Blobs aus einem Speicherkonto in ein neues Medienobjekt in einem AMS-Konto

Der folgende Code kopiert Blobs aus einem Speicherkonto in ein Media Services-Medienobjekt. 

>[!NOTE]
>Es gilt ein Grenzwert von 1.000.000 Richtlinien für verschiedene AMS-Richtlinien (z.B. für die Locator-Richtlinie oder für ContentKeyAuthorizationPolicy). Wenn Sie immer die gleichen Tage/Zugriffsberechtigungen verwenden, z.B. Richtlinien für Locator, die für einen längeren Zeitraum vorgesehen sind (Richtlinien ohne Upload), sollten Sie dieselbe Richtlinien-ID verwenden. Weitere Informationen finden Sie in [diesem](media-services-dotnet-manage-entities.md#limit-access-policies) Thema.

    using System;
    using System.Configuration;
    using System.Linq;
    using Microsoft.WindowsAzure.MediaServices.Client;
    using System.Threading;
    using Microsoft.WindowsAzure.Storage.Auth;
    using Microsoft.WindowsAzure.Storage;
    using Microsoft.WindowsAzure.Storage.Blob;
    using System.Threading.Tasks;
    
    namespace CopyExistingBlobsIntoAsset
    {
        class Program
        {
            // Read values from the App.config file.
            private static readonly string _mediaServicesAccountName =
                ConfigurationManager.AppSettings["MediaServicesAccountName"];
            private static readonly string _mediaServicesAccountKey =
                ConfigurationManager.AppSettings["MediaServicesAccountKey"];
            private static readonly string _mediaServicesStorageAccountName =
                ConfigurationManager.AppSettings["MediaServicesStorageAccountName"];
            private static readonly string _mediaServicesStorageAccountKey =
                ConfigurationManager.AppSettings["MediaServicesStorageAccountKey"];
            private static readonly string _sourceStorageAccountName =
         ConfigurationManager.AppSettings["SourceStorageAccountName"];
            private static readonly string _sourceStorageAccountKey =
                ConfigurationManager.AppSettings["SourceStorageAccountKey"];
    
            // Field for service context.
            private static CloudMediaContext _context = null;
            private static CloudStorageAccount _sourceStorageAccount = null;
            private static CloudStorageAccount _destinationStorageAccount = null;
    
            static void Main(string[] args)
            {
    
                // Used the cached credentials to create CloudMediaContext.
                _context = new CloudMediaContext(new MediaServicesCredentials(
                                _mediaServicesAccountName,
                                _mediaServicesAccountKey));
    
                _sourceStorageAccount = 
                    new CloudStorageAccount(new StorageCredentials(_sourceStorageAccountName, 
                        _sourceStorageAccountKey), true);
    
                _destinationStorageAccount = 
                    new CloudStorageAccount(new StorageCredentials(_mediaServicesStorageAccountName, 
                        _mediaServicesStorageAccountKey), true);
                
                CloudBlobClient sourceCloudBlobClient = 
                    _sourceStorageAccount.CreateCloudBlobClient();
                CloudBlobContainer sourceContainer = 
                    sourceCloudBlobClient.GetContainerReference("NameOfBlobContainerYouWantToCopy");
    
                CreateAssetFromExistingBlobs(sourceContainer);
            }
            
            static public IAsset CreateAssetFromExistingBlobs(CloudBlobContainer sourceBlobContainer)
            {
                CloudBlobClient destBlobStorage = _destinationStorageAccount.CreateCloudBlobClient();
    
                // Create a new asset. 
                IAsset asset = _context.Assets.Create("NewAsset_" + Guid.NewGuid(), AssetCreationOptions.None);
    
                IAccessPolicy writePolicy = _context.AccessPolicies.Create("writePolicy",
                    TimeSpan.FromHours(24), AccessPermissions.Write);
    
                ILocator destinationLocator = 
                    _context.Locators.CreateLocator(LocatorType.Sas, asset, writePolicy);
    
                // Get the asset container URI and Blob copy from mediaContainer to assetContainer. 
                CloudBlobContainer destAssetContainer =
                    destBlobStorage.GetContainerReference((new Uri(destinationLocator.Path)).Segments[1]);
    
                if (destAssetContainer.CreateIfNotExists())
                {
                    destAssetContainer.SetPermissions(new BlobContainerPermissions
                    {
                        PublicAccess = BlobContainerPublicAccessType.Blob
                    });
                }
    
                var blobList = sourceBlobContainer.ListBlobs();
    
                foreach (var sourceBlob in blobList)
                {
                    var assetFile = asset.AssetFiles.Create((sourceBlob as ICloudBlob).Name);
    
                    ICloudBlob destinationBlob = destAssetContainer.GetBlockBlobReference(assetFile.Name);
    
                    // Call the CopyBlobHelpers.CopyBlobAsync extension method to copy blobs.
                    using (Task task = 
                        CopyBlobHelpers.CopyBlobAsync((CloudBlockBlob)sourceBlob, 
                            (CloudBlockBlob)destinationBlob, 
                            new BlobRequestOptions(), 
                            CancellationToken.None))
                    {
                        task.Wait();
                    }
    
                    assetFile.ContentFileSize = (sourceBlob as ICloudBlob).Properties.Length;
                    assetFile.Update();
                    Console.WriteLine("File {0} is of {1} size", assetFile.Name, assetFile.ContentFileSize);
                }
    
                asset.Update();
    
                destinationLocator.Delete();
                writePolicy.Delete();
    
                // Set the primary asset file.
                // If, for example, we copied a set of Smooth Streaming files, 
                // set the .ism file to be the primary file. 
                // If we, for example, copied an .mp4, then the mp4 would be the primary file. 
                var ismAssetFiles = asset.AssetFiles.ToList().
                    Where(f => f.Name.EndsWith(".ism", StringComparison.OrdinalIgnoreCase)).ToArray();
    
                // The following code assigns the first .ism file as the primary file in the asset.
                // An asset should have one .ism file.  
                ismAssetFiles.First().IsPrimary = true;
                ismAssetFiles.First().Update();
    
                return asset;
            }
        }
    }
    
## <a name="next-steps"></a>Nächste Schritte

Sie können nun Ihre hochgeladenen Medienobjekte codieren. Weitere Informationen finden Sie unter [Codieren von Medienobjekten](media-services-portal-encode.md).

Sie können auch mithilfe von Azure Functions einen Codierungsauftrag auslösen, wenn eine Datei im konfigurierten Container eingeht. Weitere Informationen finden Sie in [diesem Beispiel](https://azure.microsoft.com/resources/samples/media-services-dotnet-functions-integration/ ).

## <a name="media-services-learning-paths"></a>Media Services-Lernpfade
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Feedback geben
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]


