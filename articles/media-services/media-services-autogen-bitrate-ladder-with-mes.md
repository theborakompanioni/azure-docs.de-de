---
title: "Verwenden von Azure Media Encoder Standard zum automatischen Generieren einer Reihe von Bitraten/Auflösungs-Paaren| Microsoft-Dokumentation"
description: "In diesem Thema wird gezeigt, wie Sie mit Media Encoder Standard (MES) automatisch eine Reihe von Bitraten/Auflösungs-Paaren auf Basis der eingegebenen Auflösung und Bitrate generieren können. Eingegebene Auflösung und Bitrate werden nie überschritten. Ist die Eingabe beispielsweise 720p bei 3MBit/s, bleibt die Ausgabe bestenfalls bei 720p, und es beginnt mit niedrigeren Raten als 3MBit/s."
services: media-services
documentationcenter: 
author: juliako
manager: erikre
editor: 
ms.assetid: 63ed95da-1b82-44b0-b8ff-eebd535bc5c7
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/29/2016
ms.author: juliako
translationtype: Human Translation
ms.sourcegitcommit: d4be6bf16527fa1a08ad758cbccf1329122a5e65
ms.openlocfilehash: 5cb44681f9bbd07a95eb2eed1d8de08243a18d6d
ms.lasthandoff: 02/28/2017


---
#  <a name="use-azure-media-standard-to-auto-generate-a-bitrate-ladder"></a>Verwenden von Azure Media Standard zum automatischen Generieren einer Reihe von Bitraten/Auflösungs-Paaren

## <a name="overview"></a>Übersicht

In diesem Thema wird gezeigt, wie Sie mit Media Encoder Standard (MES) automatisch eine Reihe von Bitraten/Auflösungs-Paaren auf Basis der eingegebenen Auflösung und Bitrate generieren können. Die automatisch generierte Voreinstellung wird nie die eingegebene Auflösung und Bitrate übersteigen. Ist die Eingabe beispielsweise 720p bei 3MBit/s, bleibt die Ausgabe bestenfalls bei 720p, und es beginnt mit niedrigeren Raten als 3MBit/s.

Um dieses Feature verwenden zu können, müssen Sie beim Erstellen einer Codierungsaufgabe die Voreinstellung **Adaptives Streaming** angeben. Bei Verwendung der Voreinstellung **Adaptives Streaming** begrenzt der MES-Encoder eine Reihe von Bitraten/Auflösungs-Paaren intelligent. Allerdings können Sie die Codierungskosten nicht steuern, da der Dienst bestimmt, wie viele Ebenen bei welcher Auflösung verwendet werden. Am [Ende](#output) dieses Themas finden Sie Beispiele von Ausgabeebenen, die von MES als Ergebnis der Codierung mit der Voreinstellung **Adaptives Streaming** erzeugt wurden.

## <a id="encoding_with_dotnet"></a>Codierung mit dem Media Services .NET SDK

Im folgenden Codebeispiel wird das Media Services-.NET-SDK verwendet, um die folgenden Aufgaben auszuführen:

- Erstellen eines Codierungsauftrags.
- Abrufen eines Verweises auf den Media Encoder Standard-Encoder
- Fügen Sie dem Auftrag eine Codierungsaufgabe hinzu, und geben Sie die Verwendung der Voreinstellung **Adaptives Streaming** an. 
- Erstellen Sie ein Ausgabeasset, das das codierte Asset enthalten soll.
- Fügen Sie einen Ereignishandler hinzu, um den Auftragsstatus zu überprüfen.
- Übermitteln des Auftrags.

        using System;
        using System.Configuration;
        using System.IO;
        using System.Linq;
        using Microsoft.WindowsAzure.MediaServices.Client;
        using System.Threading;

        namespace AdaptiveStreamingMESPresest
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
                // Used the chached credentials to create CloudMediaContext.
                _context = new CloudMediaContext(_cachedCredentials);

                // Get an uploaded asset.
                var asset = _context.Assets.FirstOrDefault();

                // Encode and generate the output using the "Adaptive Streaming" preset.
                EncodeToAdaptiveBitrateMP4Set(asset);

                Console.ReadLine();
            }

            static public IAsset EncodeToAdaptiveBitrateMP4Set(IAsset asset)
            {
                // Declare a new job.
                IJob job = _context.Jobs.Create("Media Encoder Standard Job");

                // Get a media processor reference, and pass to it the name of the 
                // processor to use for the specific task.
                IMediaProcessor processor = GetLatestMediaProcessorByName("Media Encoder Standard");

                // Create a task
                ITask task = job.Tasks.AddNew("Media Encoder Standard encoding task",
                processor,
                "Adaptive Streaming",
                TaskOptions.None);

                // Specify the input asset to be encoded.
                task.InputAssets.Add(asset);
                // Add an output asset to contain the results of the job. 
                // This output is specified as AssetCreationOptions.None, which 
                // means the output asset is not encrypted. 
                task.OutputAssets.AddNew("Output asset",
                AssetCreationOptions.None);

                job.StateChanged += new EventHandler<JobStateChangedEventArgs>(JobStateChanged);
                job.Submit();
                job.GetExecutionProgressTask(CancellationToken.None).Wait();

                return job.OutputMediaAssets[0];
            }
            private static void JobStateChanged(object sender, JobStateChangedEventArgs e)
            {
                Console.WriteLine("Job state changed event:");
                Console.WriteLine("  Previous state: " + e.PreviousState);
                Console.WriteLine("  Current state: " + e.CurrentState);
                switch (e.CurrentState)
                {
                case JobState.Finished:
                    Console.WriteLine();
                    Console.WriteLine("Job is finished. Please wait while local tasks or downloads complete...");
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
                    break;
                default:
                    break;
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

            }

        }

## <a id="output"></a>Ausgabe

Dieser Abschnitt zeigt drei Beispiele von Ausgabeebenen, die von MES als Ergebnis der Codierung mit der Voreinstellung **Adaptives Streaming** erzeugt wurden. 

### <a name="example-1"></a>Beispiel 1
Quelle mit Höhe „1080“ und Bildrate „29.970“ produziert 6 Videoebenen:

|Ebene|Höhe|Breite|Bitrate(KBit/s)|
|---|---|---|---|
|1|1080|1920|6780|
|2|720|1280|3520|
|3|540|960|2210|
|4|360|640|1150|
|5|270|480|720|
|6|180|320|380|

### <a name="example-2"></a>Beispiel 2
Quelle mit Höhe „720“ und Bildrate „23.970“ produziert 5 Videoebenen:

|Ebene|Höhe|Breite|Bitrate(KBit/s)|
|---|---|---|---|
|1|720|1280|2940|
|2|540|960|1850|
|3|360|640|960|
|4|270|480|600|
|5|180|320|320|

### <a name="example-3"></a>Beispiel 3
Quelle mit Höhe „360“ und Bildrate „29.970“ produziert 3 Videoebenen:

|Ebene|Höhe|Breite|Bitrate(KBit/s)|
|---|---|---|---|
|1|360|640|700|
|2|270|480|440|
|3|180|320|230|
## <a name="media-services-learning-paths"></a>Media Services-Lernpfade
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Feedback geben
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="see-also"></a>Weitere Informationen
[Media Services-Codierung (Übersicht)](media-services-encode-asset.md)


