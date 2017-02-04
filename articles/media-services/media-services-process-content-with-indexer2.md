---
title: Indizieren von Mediendateien mit der Vorschauversion von Azure Media Indexer 2 | Microsoft Docs
description: "Mit dem Azure Media Indexer können Sie die Inhalte Ihrer Mediendateien durchsuchbar machen und eine Volltext-Aufzeichnung für Untertitel und Schlüsselwörter generieren. In diesem Thema wird die Verwendung von Media Indexer 2 Preview erläutert."
services: media-services
documentationcenter: 
author: Juliako
manager: erikre
editor: 
ms.assetid: 85d25525-a498-44eb-ae3a-2ca5ceb8e53d
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 12/07/2016
ms.author: adsolank;juliako;
translationtype: Human Translation
ms.sourcegitcommit: 7065d5e87dbf47153bc89b37b56af81927b8b92b
ms.openlocfilehash: 902e872b224b267c989f741345efdd8ef3fa8ce3


---
# <a name="indexing-media-files-with-azure-media-indexer-2-preview"></a>Indizieren von Mediendateien mit Azure Media Indexer 2 Preview
## <a name="overview"></a>Übersicht
Mit dem Medienprozessor (MP) **Azure Media Indexer 2 Preview** können Sie Mediendateien und Inhalte durchsuchbar machen sowie Untertitelspuren und Schlüsselwörter generieren. Verglichen mit der vorherigen Version des [Azure Media Indexer](media-services-index-content.md)führt **Azure Media Indexer 2 Preview** eine schnellere Indizierung durch und bietet eine breitere Sprachunterstützung. Zu den unterstützten Sprachen zählen Englisch, Spanisch, Französisch, Deutsch, Italienisch, Chinesisch (Mandarin, vereinfacht), Portugiesisch, Arabisch und Japanisch.

Der Medienprozessor von **Azure Media Indexer 2 Preview** befindet sich derzeit in der Vorschauphase.

In diesem Thema wird dargestellt, wie Sie Indizierungsaufträge mit **Azure Media Indexer 2 Preview**erstellen.

> [!NOTE]
> Es gelten die folgenden Bedingungen:
> 
> Indexer 2 wird in Azure China und Azure Government nicht unterstützt.
> 
> Stellen Sie beim Indizieren von Inhalten sicher, dass Mediendateien verwendet werden, die sehr klare Sprache enthalten (ohne Hintergrundmusik, Lärm, Effekte oder Mikrofonrauschen). Die folgenden Beispiele sind geeignete Inhalte: aufgezeichnete Besprechungen, Vorträge oder Präsentationen. Folgende Inhalte sind für die Indizierung ggf. nicht geeignet: Filme, Fernsehsendungen, Material mit gemischten Audio- und Soundeffekten, schlecht aufgezeichnete Inhalte mit Hintergrundgeräuschen (Rauschen).
> 
> 

Dieses Thema enthält Details zu **Azure Media Indexer 2 Preview** und zeigt, wie diese Version mit dem Media Services SDK für .NET verwendet wird.

## <a name="input-and-output-files"></a>Ein- und Ausgabedateien
### <a name="input-files"></a>Eingabedateien
Audio- oder Videodateien

### <a name="output-files"></a>Ausgabedateien
Ein Indizierungsauftrag kann Untertiteldateien in den folgenden Formaten generieren:  

* **SAMI**
* **TTML**
* **WebVTT**

Untertiteldateien in diesen Formaten können verwendet werden, um Hörgeschädigten Audio- und Videodateien zugänglich zu machen.

## <a name="task-configuration-preset"></a>Aufgabenkonfiguration (Voreinstellung)
Wenn Sie eine Indizierungsaufgabe mit **Azure Media Indexer 2 Preview**erstellen, müssen Sie eine Konfigurationsvoreinstellung angeben.

Die folgende JSON legt verfügbare Parameter fest.

    {
      "version":"1.0",
      "Features":
        [
           {
           "Options": {
                "Formats":["WebVtt","ttml"],
                "Language":"enUs",
                "Type":"RecoOptions"
           },
           "Type":"SpReco"
        }]
    }

## <a name="supported-languages"></a>Unterstützte Sprachen
Azure Media Indexer 2 Preview unterstützt Sprache in Text für die folgenden Sprachen (wenn Sie den Namen der Sprache in der Taskkonfiguration angeben, verwenden Sie einen 4-Zeichen-Code in Klammern, wie unten gezeigt):

* Englisch [EnUs]
* Spanisch [EsEs]
* Chinesisch (Mandarin, vereinfacht) [ZhCn]
* Französisch [FrFr]
* Deutsch [DeDe]
* Italienisch [ItIt]
* Portugiesisch [PtBr]
* Arabisch (ägyptisch) [ArEg]
* Japanisch [JaJp]

## <a name="sample-code"></a>Beispielcode

Das folgende Programm zeigt Ihnen, wie Sie folgendes ausführen:

1. Erstellen eines Assets und Hochladen einer Mediendatei in das Asset.
2. Sie einen Auftrag mit einer Indizierungsaufgabe anhand einer Konfigurationsdatei erstellen, die die folgende JSON-Voreinstellung enthält.
   
        {
          "version":"1.0",
          "Features":
            [
               {
               "Options": {
                    "Formats":["WebVtt","ttml"],
                    "Language":"enUs",
                    "Type":"RecoOptions"
               },
               "Type":"SpReco"
            }]
        }
3. Sie die Ausgabedateien herunterladen. 
   
        using System;
        using System.Configuration;
        using System.IO;
        using System.Linq;
        using Microsoft.WindowsAzure.MediaServices.Client;
        using System.Threading;
        using System.Threading.Tasks;
   
        namespace IndexContent
        {
            class Program
            {
                // Read values from the App.config file.
                private static readonly string _mediaServicesAccountName =
                    ConfigurationManager.AppSettings["MediaServicesAccountName"];
                private static readonly string _mediaServicesAccountKey =
                    ConfigurationManager.AppSettings["MediaServicesAccountKey"];
   
                // Field for service context.
                private static CloudMediaContext _context = null;
                private static MediaServicesCredentials _cachedCredentials = null;
   
                static void Main(string[] args)
                {
   
                    // Create and cache the Media Services credentials in a static class variable.
                    _cachedCredentials = new MediaServicesCredentials(
                                    _mediaServicesAccountName,
                                    _mediaServicesAccountKey);
                    // Used the cached credentials to create CloudMediaContext.
                    _context = new CloudMediaContext(_cachedCredentials);
   
                    // Run indexing job.
                    var asset = RunIndexingJob(@"C:\supportFiles\Indexer\BigBuckBunny.mp4",
                                                @"C:\supportFiles\Indexer\config.json");
   
                    // Download the job output asset.
                    DownloadAsset(asset, @"C:\supportFiles\Indexer\Output");
                }
   
                static IAsset RunIndexingJob(string inputMediaFilePath, string configurationFile)
                {
                    // Create an asset and upload the input media file to storage.
                    IAsset asset = CreateAssetAndUploadSingleFile(inputMediaFilePath,
                        "My Indexing Input Asset",
                        AssetCreationOptions.None);
   
                    // Declare a new job.
                    IJob job = _context.Jobs.Create("My Indexing Job");
   
                    // Get a reference to Azure Media Indexer 2 Preview.
                    string MediaProcessorName = "Azure Media Indexer 2 Preview";
   
                    var processor = GetLatestMediaProcessorByName(MediaProcessorName);
   
                    // Read configuration from the specified file.
                    string configuration = File.ReadAllText(configurationFile);
   
                    // Create a task with the encoding details, using a string preset.
                    ITask task = job.Tasks.AddNew("My Indexing Task",
                        processor,
                        configuration,
                        TaskOptions.None);
   
                    // Specify the input asset to be indexed.
                    task.InputAssets.Add(asset);
   
                    // Add an output asset to contain the results of the job.
                    task.OutputAssets.AddNew("My Indexing Output Asset", AssetCreationOptions.None);
   
                    // Use the following event handler to check job progress.  
                    job.StateChanged += new EventHandler<JobStateChangedEventArgs>(StateChanged);
   
                    // Launch the job.
                    job.Submit();
   
                    // Check job execution and wait for job to finish.
                    Task progressJobTask = job.GetExecutionProgressTask(CancellationToken.None);
   
                    progressJobTask.Wait();
   
                    // If job state is Error, the event handling
                    // method for job progress should log errors.  Here we check
                    // for error state and exit if needed.
                    if (job.State == JobState.Error)
                    {
                        ErrorDetail error = job.Tasks.First().ErrorDetails.First();
                        Console.WriteLine(string.Format("Error: {0}. {1}",
                                                        error.Code,
                                                        error.Message));
                        return null;
                    }
   
                    return job.OutputMediaAssets[0];
                }
   
                static IAsset CreateAssetAndUploadSingleFile(string filePath, string assetName, AssetCreationOptions options)
                {
                    IAsset asset = _context.Assets.Create(assetName, options);
   
                    var assetFile = asset.AssetFiles.Create(Path.GetFileName(filePath));
                    assetFile.Upload(filePath);
   
                    return asset;
                }
   
                static void DownloadAsset(IAsset asset, string outputDirectory)
                {
                    foreach (IAssetFile file in asset.AssetFiles)
                    {
                        file.Download(Path.Combine(outputDirectory, file.Name));
                    }
                }
   
                static IMediaProcessor GetLatestMediaProcessorByName(string mediaProcessorName)
                {
                    var processor = _context.MediaProcessors
                        .Where(p => p.Name == mediaProcessorName)
                        .ToList()
                        .OrderBy(p => new Version(p.Version))
                        .LastOrDefault();
   
                    if (processor == null)
                        throw new ArgumentException(string.Format("Unknown media processor",
                                                                   mediaProcessorName));
   
                    return processor;
                }
   
                static private void StateChanged(object sender, JobStateChangedEventArgs e)
                {
                    Console.WriteLine("Job state changed event:");
                    Console.WriteLine("  Previous state: " + e.PreviousState);
                    Console.WriteLine("  Current state: " + e.CurrentState);
   
                    switch (e.CurrentState)
                    {
                        case JobState.Finished:
                            Console.WriteLine();
                            Console.WriteLine("Job is finished.");
                            Console.WriteLine();
                            break;
                        case JobState.Canceling:
                        case JobState.Queued:
                        case JobState.Scheduled:
                        case JobState.Processing:
                            Console.WriteLine("Please wait...\n");
                            break;
                        case JobState.Canceled:
                        case JobState.Error:
                            // Cast sender as a job.
                            IJob job = (IJob)sender;
                            // Display or log error details as needed.
                            // LogJobStop(job.Id);
                            break;
                        default:
                            break;
                    }
                }
            }
        }

## <a name="media-services-learning-paths"></a>Media Services-Lernpfade
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Feedback geben
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="related-links"></a>Verwandte Links
[Azure Media Services Analytics – Übersicht](media-services-analytics-overview.md)

[Azure Media Analytics-Demos](http://azuremedialabs.azurewebsites.net/demos/Analytics.html)




<!--HONumber=Dec16_HO2-->


