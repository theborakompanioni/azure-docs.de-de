---
title: Gesichts- und Emotionenerkennung mit Azure Media Analytics | Microsoft-Dokumentation
description: Dieses Thema veranschaulicht, wie Azure Media Analytics Gesichter und Emotionen erkennt.
services: media-services
documentationcenter: 
author: juliako
manager: erikre
editor: 
ms.assetid: 5ca4692c-23f1-451d-9d82-cbc8bf0fd707
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 04/17/2017
ms.author: milanga;juliako;
translationtype: Human Translation
ms.sourcegitcommit: abdbb9a43f6f01303844677d900d11d984150df0
ms.openlocfilehash: 5a604f3538a0749f7f951926f451cc91504255d6
ms.lasthandoff: 04/20/2017


---
# <a name="detect-face-and-emotion-with-azure-media-analytics"></a>Gesichts- und Emotionenerkennung mit Azure Media Analytics
## <a name="overview"></a>Übersicht
Mit dem Medienprozessor (MP) **Azure Media Face Detector** können Sie Bewegungen zählen und nachverfolgen und sogar Teilnahme sowie Reaktion der Zielgruppe anhand von Gesichtsausdrücken einschätzen. Dieser Dienst enthält zwei Funktionen: 

* **Gesichtserkennung**
  
    Die Gesichtserkennung sucht und verfolgt menschliche Gesichter in einem Video. Mehrere Gesichter können erkannt und anschließend nachverfolgt werden, während sie sich bewegen, wobei Metadaten zu Zeit und Ort in einer JSON-Datei zurückgegeben werden. Während der Nachverfolgung wird versucht, dem gleichen Gesicht eine konsistente ID zuzuweisen, während die Person sich auf dem Bildschirm bewegt, auch wenn sie verdeckt wird oder kurzzeitig nicht im Frame ist.
  
  > [!NOTE]
  > Diese Dienste führen keine Gesichtserkennung durch. Eine Person, die zu lange nicht im Frame ist oder verdeckt wird, erhält bei ihrer Rückkehr eine neue ID.
  > 
  > 
* **Emotionenerkennung**
  
    Emotionenerkennung ist eine optionale Komponente des Gesichtserkennungs-Medienprozessors, die eine Analyse verschiedener emotionaler Attribute in den Gesichtern liefert, einschließlich Glück, Trauer, Angst, Wut und anderer Emotionen. 

Der MP **Azure Media Face Detector** befindet sich derzeit in der Vorschauphase.

Dieses Thema enthält Details zur **Azure Media-Gesichtserkennung** und zeigt die Verwendung mit dem Media Services SDK für .NET.

## <a name="face-detector-input-files"></a>Face Detector-Eingabedateien
Videodateien. Derzeit werden folgende Formate unterstützt: MP4, MOV und WMV.

## <a name="face-detector-output-files"></a>Face Detector-Ausgabedateien
Die API zur Gesichtserkennung und -nachverfolgung ermöglicht Gesichtspositionserkennung und -nachverfolgung mit hoher Genauigkeit, sodass bis zu 64 menschliche Gesichter in einem Video erkannt werden können. Frontalansichten von Gesichtern bieten die besten Ergebnisse, während Seitenansichten und kleine Gesichter (höchstens 24 x 24 Pixel) möglicherweise nicht so genau erkannt werden.

Die erkannten und nachverfolgten Gesichter werden mit den Koordinaten (links, oben, Breite und Höhe) zurückgegeben, die die Position der Gesichter im Bild in Pixeln sowie eine Gesichts-ID angeben, die die Nachverfolgung einer einzelnen Person kenntlich macht. Gesichts-IDs können zurückgesetzt werden, wenn die Frontalansicht des Gesichts verloren geht oder im Frame überlappt wird, sodass einigen Personen möglicherweise mehrere IDs zugewiesen werden.

### <a id="output_elements"></a>Elemente der Ausgabe-JSON-Datei
Für die Gesichtserkennung und -nachverfolgung liegen als Ausgabeergebnis die Metadaten aus den Gesichtern in der jeweiligen Datei im JSON-Format vor.

Die JSON-Datei für Gesichtserkennung und -nachverfolgung umfasst die folgenden Attribute:

| Element | Beschreibung |
| --- | --- |
| Version |Dies bezieht sich auf die Version der Video-API. |
| Zeitskala |„Ticks“ pro Sekunde des Videos. |
| Offset |Dies ist der Zeitoffset für Zeitstempel. In Version 1.0 von Video-APIs wird dies immer 0 sein. In zukünftigen Szenarien, die wir unterstützen, kann sich dieser Wert ändern. |
| Framerate |Frames des Videos pro Sekunde. |
| Fragments |Die Metadaten werden in verschiedene, als Fragmente bezeichnete Segmente aufgeteilt. Jedes Fragment enthält Startzeitpunkt, Dauer, Intervallnummer und Ereignis(se). |
| Start |Die Startzeit des ersten Ereignisses in „Ticks“. |
| Duration |Die Länge des Fragments in „Ticks“. |
| Intervall |Das Intervall jedes Ereigniseintrags innerhalb des Fragments in „Ticks“. |
| Ereignisse |Jedes Ereignis enthält die innerhalb dieser Zeitspanne erkannten und nachverfolgten Gesichter. Es ist ein Array eines Arrays von Ereignissen. Das äußere Array stellt ein Zeitintervall dar. Das innere Array besteht aus 0 oder mehr Ereignissen, die zu diesem Zeitpunkt aufgetreten sind. Eine leere Klammer ( [] ) bedeutet, dass keine Gesichter erkannt wurden. |
| ID |Die ID des Gesichts, das nachverfolgt wird. Diese Nummer kann sich unbeabsichtigt ändern, wenn ein Gesicht nicht mehr erkannt wird. Eine bestimmte Person sollte während des gesamten Videos die gleiche ID aufweisen, doch dies kann aufgrund von Einschränkungen im Erkennungsalgorithmus (Verdecken usw.) nicht garantiert werden. |
| X, Y |Die oberen linken X- und Y-Koordinaten des Rahmens, der das Gesicht umgibt, in einem normalisierten Maßstab von 0,0 bis 1,0. <br/>X- und Y-Koordinaten sind immer relativ zum Querformat, d.h. bei einem Video im Hochformat (oder, im Fall von iOS, einem kopfstehenden) müssen Sie die Koordinaten entsprechend vertauschen. |
| Width, Height |Die Breite und Höhe des Rahmens, der das Gesicht umgibt, in einem normalisierten Maßstab von 0,0 bis 1,0. |
| facesDetected |Dieses Element befindet sich am Ende der JSON-Ergebnisse und fasst die Anzahl der Gesichter zusammen, die der Algorithmus während des Videos erkannt hat. Da die IDs unabsichtlich zurückgesetzt werden können, wenn ein Gesicht nicht mehr erkannt wird (z. B. wenn das Gesicht sich aus dem Bild bewegt, die Person sich abwendet), entspricht diese Zahl möglicherweise nicht immer der tatsächlichen Anzahl der Gesichter im Video. |

Face Detector verwendet Techniken der Fragmentierung (wobei die Metadaten in zeitbasierte Blöcke unterteilt werden können, sodass Sie nichts herunterladen müssen, was Sie nicht benötigen) und Segmentierung (wobei die Ereignisse aufgeteilt werden, falls sie zu groß werden). Einige einfache Berechnungen können Ihnen helfen, die Daten zu transformieren. Wenn ein Ereignis beispielsweise mit 6.300 (Ticks), einer Zeitskala von 2.997 (Ticks/s) und Framerate von 29,97 (Bilder/s) begonnen hat, dann gilt:

* Start/Zeitskala = 2,1 Sekunden
* Sekunden × Framerate = 63 Frames

## <a name="face-detection-input-and-output-example"></a>Eingabe- und Ausgabebeispiel für Gesichtserkennung
### <a name="input-video"></a>Videoeingang
[Videoeingang](http://ampdemo.azureedge.net/azuremediaplayer.html?url=https%3A%2F%2Freferencestream-samplestream.streaming.mediaservices.windows.net%2Fc8834d9f-0b49-4b38-bcaf-ece2746f1972%2FMicrosoft%20Convergence%202015%20%20Keynote%20Highlights.ism%2Fmanifest&amp;autoplay=false)

### <a name="task-configuration-preset"></a>Aufgabenkonfiguration (Voreinstellung)
Wenn Sie eine Aufgabe mit **Azure Media Face Detector**erstellen, müssen Sie eine Konfigurationsvoreinstellung angeben. Die folgende Konfigurationsvoreinstellung ist nur zur Gesichtserkennung bestimmt.

    {
      "version":"1.0",
      "options":{
          "TrackingMode": "Fast"
      }
    }

#### <a name="attribute-descriptions"></a>Beschreibungen der Attribute
| Attributname | Beschreibung |
| --- | --- |
| Mode |Schneller: höhere Verarbeitungsgeschwindigkeit, aber weniger Genauigkeit (Standard).|


### <a name="json-output"></a>JSON-Ausgabe
Das folgende Beispiel einer JSON-Ausgabe wurde abgeschnitten.

    {
    "version": 1,
    "timescale": 30000,
    "offset": 0,
    "framerate": 29.97,
    "width": 1280,
    "height": 720,
    "fragments": [
        {
        "start": 0,
        "duration": 60060
        },
        {
        "start": 60060,
        "duration": 60060,
        "interval": 1001,
        "events": [
            [
            {
                "id": 0,
                "x": 0.519531,
                "y": 0.180556,
                "width": 0.0867188,
                "height": 0.154167
            }
            ],
            [
            {
                "id": 0,
                "x": 0.517969,
                "y": 0.181944,
                "width": 0.0867188,
                "height": 0.154167
            }
            ],
            [
            {
                "id": 0,
                "x": 0.517187,
                "y": 0.183333,
                "width": 0.0851562,
                "height": 0.151389
            }
            ],

        . . . 

## <a name="emotion-detection-input-and-output-example"></a>Eingabe- und Ausgabebeispiel für Emotionenerkennung
### <a name="input-video"></a>Videoeingang
[Videoeingang](http://ampdemo.azureedge.net/azuremediaplayer.html?url=https%3A%2F%2Freferencestream-samplestream.streaming.mediaservices.windows.net%2Fc8834d9f-0b49-4b38-bcaf-ece2746f1972%2FMicrosoft%20Convergence%202015%20%20Keynote%20Highlights.ism%2Fmanifest&amp;autoplay=false)

### <a name="task-configuration-preset"></a>Aufgabenkonfiguration (Voreinstellung)
Wenn Sie eine Aufgabe mit **Azure Media Face Detector**erstellen, müssen Sie eine Konfigurationsvoreinstellung angeben. Die folgende Konfigurationsvoreinstellung gibt an, eine JSON-Datei auf Basis der Emotionenerkennung zu erstellen.

    {
      "version": "1.0",
      "options": {
        "aggregateEmotionWindowMs": "987",
        "mode": "aggregateEmotion",
        "aggregateEmotionIntervalMs": "342"
      }
    }


#### <a name="attribute-descriptions"></a>Beschreibungen der Attribute
| Attributname | Beschreibung |
| --- | --- |
| Mode |Faces: nur Gesichtserkennung.<br/>PerFaceEmotion: unabhängige Rückgabe der Emotionen für jede Gesichtserkennung.<br/>AggregateEmotion: Rückgabe der durchschnittlichen Emotionswerte für alle Gesichter im Frame. |
| AggregateEmotionWindowMs |Bei Auswahl des Modus „AggregateEmotion“ verwenden. Gibt die Länge des Videos, das jeweils zum Produzieren eines Aggregierergebnisses verwendet wird, in Millisekunden an. |
| AggregateEmotionIntervalMs |Bei Auswahl des Modus „AggregateEmotion“ verwenden. Gibt an, mit welcher Häufigkeit Aggregierergebnisse erzeugt werden. |

#### <a name="aggregate-defaults"></a>Aggregierstandardwerte
Die folgenden Werte werden für Aggregierfenster und Intervalleinstellungen empfohlen. AggregateEmotionWindowMs sollte länger als AggregateEmotionIntervalMs sein.

|| Standardwerte (s) | Minimum (s) | Maximum (s) |
|--- | --- | --- | --- |
| AggregateEmotionWindowMs |0,5 |2 |0,25|
| AggregateEmotionIntervalMs |0,5 |1 |0,25|

### <a name="json-output"></a>JSON-Ausgabe
JSON-Ausgabe für Emotionsaggregierung (abgeschnitten):

    {
     "version": 1,
     "timescale": 30000,
     "offset": 0,
     "framerate": 29.97,
     "width": 1280,
     "height": 720,
     "fragments": [
       {
         "start": 0,
         "duration": 60060,
         "interval": 15015,
         "events": [
           [
             {
               "windowFaceDistribution": {
                 "neutral": 0,
                 "happiness": 0,
                 "surprise": 0,
                 "sadness": 0,
                 "anger": 0,
                 "disgust": 0,
                 "fear": 0,
                 "contempt": 0
               },
               "windowMeanScores": {
                 "neutral": 0,
                 "happiness": 0,
                 "surprise": 0,
                 "sadness": 0,
                 "anger": 0,
                 "disgust": 0,
                 "fear": 0,
                 "contempt": 0
               }
             }
           ],
           [
             {
               "windowFaceDistribution": {
                 "neutral": 0,
                 "happiness": 0,
                 "surprise": 0,
                 "sadness": 0,
                 "anger": 0,
                 "disgust": 0,
                 "fear": 0,
                 "contempt": 0
               },
               "windowMeanScores": {
                 "neutral": 0,
                 "happiness": 0,
                 "surprise": 0,
                 "sadness": 0,
                 "anger": 0,
                 "disgust": 0,
                 "fear": 0,
                 "contempt": 0
               }
             }
           ],
           [
             {
               "windowFaceDistribution": {
                 "neutral": 0,
                 "happiness": 0,
                 "surprise": 0,
                 "sadness": 0,
                 "anger": 0,
                 "disgust": 0,
                 "fear": 0,
                 "contempt": 0
               },
               "windowMeanScores": {
                 "neutral": 0,
                 "happiness": 0,
                 "surprise": 0,
                 "sadness": 0,
                 "anger": 0,
                 "disgust": 0,
                 "fear": 0,
                 "contempt": 0
               }
             }
           ],
           [
             {
               "windowFaceDistribution": {
                 "neutral": 0,
                 "happiness": 0,
                 "surprise": 0,
                 "sadness": 0,
                 "anger": 0,
                 "disgust": 0,
                 "fear": 0,
                 "contempt": 0
               },
               "windowMeanScores": {
                 "neutral": 0,
                 "happiness": 0,
                 "surprise": 0,
                 "sadness": 0,
                 "anger": 0,
                 "disgust": 0,
                 "fear": 0,
                 "contempt": 0
               }
             }
           ]
         ]
       },
       {
         "start": 60060,
         "duration": 60060,
         "interval": 15015,
         "events": [
           [
             {
               "windowFaceDistribution": {
                 "neutral": 1,
                 "happiness": 0,
                 "surprise": 0,
                 "sadness": 0,
                 "anger": 0,
                 "disgust": 0,
                 "fear": 0,
                 "contempt": 0
               },
               "windowMeanScores": {
                 "neutral": 0.688541,
                 "happiness": 0.0586323,
                 "surprise": 0.227184,
                 "sadness": 0.00945675,
                 "anger": 0.00592107,
                 "disgust": 0.00154993,
                 "fear": 0.00450447,
                 "contempt": 0.0042109
               }
             }
           ],
           [
             {
               "windowFaceDistribution": {
                 "neutral": 1,
                 "happiness": 0,
                 "surprise": 0,
                 "sadness": 0,
                 "anger": 0,
                 "disgust": 0,
                 "fear": 0,


## <a name="limitations"></a>Einschränkungen
* Zu den unterstützten Eingabevideoformaten zählen MP4, MOV und WMV.
* Die erkennbare Gesichtsgröße reicht von 24 x 24 bis 2.048 x 2.048 Pixel. Außerhalb dieses Bereichs liegende Gesichter werden nicht erkannt.
* Für jedes Video werden maximal 64 Gesichter zurückgegeben.
* Einige Gesichter werden möglicherweise aufgrund technischer Probleme nicht erkannt: z. B. sehr große Gesichtswinkel (Kopfpose) und starke Verdeckung. Frontalansichten und nahezu der Frontalansicht entsprechende Ansichten von Gesichtern führen zu den besten Ergebnissen.

## <a name="sample-code"></a>Beispielcode
Das folgende Programm zeigt Ihnen, wie Sie folgendes ausführen:

1. Erstellen eines Assets und Hochladen einer Mediendatei in das Asset.
2. Erstellen eines Auftrags mit einer Gesichtserkennungsaufgabe auf Basis einer Konfigurationsdatei, die die folgende JSON-Voreinstellung enthält. 
   
        {
            "version": "1.0"
        }
3. Herunterladen der JSON-Ausgabedateien. 
   
        using System;
        using System.Configuration;
        using System.IO;
        using System.Linq;
        using Microsoft.WindowsAzure.MediaServices.Client;
        using System.Threading;
        using System.Threading.Tasks;
   
        namespace FaceDetection
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
   
                    // Run the FaceDetection job.
                    var asset = RunFaceDetectionJob(@"C:\supportFiles\FaceDetection\BigBuckBunny.mp4",
                                                @"C:\supportFiles\FaceDetection\config.json");
   
                    // Download the job output asset.
                    DownloadAsset(asset, @"C:\supportFiles\FaceDetection\Output");
                }
   
                static IAsset RunFaceDetectionJob(string inputMediaFilePath, string configurationFile)
                {
                    // Create an asset and upload the input media file to storage.
                    IAsset asset = CreateAssetAndUploadSingleFile(inputMediaFilePath,
                        "My Face Detection Input Asset",
                        AssetCreationOptions.None);
   
                    // Declare a new job.
                    IJob job = _context.Jobs.Create("My Face Detection Job");
   
                    // Get a reference to Azure Media Face Detector.
                    string MediaProcessorName = "Azure Media Face Detector";
   
                    var processor = GetLatestMediaProcessorByName(MediaProcessorName);
   
                    // Read configuration from the specified file.
                    string configuration = File.ReadAllText(configurationFile);
   
                    // Create a task with the encoding details, using a string preset.
                    ITask task = job.Tasks.AddNew("My Face Detection Task",
                        processor,
                        configuration,
                        TaskOptions.None);
   
                    // Specify the input asset.
                    task.InputAssets.Add(asset);
   
                    // Add an output asset to contain the results of the job.
                    task.OutputAssets.AddNew("My Face Detectoion Output Asset", AssetCreationOptions.None);
   
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


