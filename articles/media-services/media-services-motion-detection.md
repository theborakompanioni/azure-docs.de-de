<properties
	pageTitle="Erkennen von Emotionen mit Azure Media Analytics"
	description="Mit dem Medienprozessor (MP) Azure Media Motion Detector können Sie effizient Ihrem Interesse entsprechende Abschnitte innerhalb eines ansonsten langen und ereignislosen Videos identifizieren."
	services="media-services"
	documentationCenter=""
	authors="juliako"
	manager="erikre"
	editor=""/>

<tags
	ms.service="media-services"
	ms.workload="media"
	ms.tgt_pltfrm="na"
	ms.devlang="dotnet"
	ms.topic="article"
	ms.date="06/22/2016"  
	ms.author="milanga;juliako;"/>
 
# Erkennen von Emotionen mit Azure Media Analytics

##Übersicht

Mit dem Medienprozessor (MP) **Azure Media Motion Detector** können Sie effizient Ihrem Interesse entsprechende Abschnitte innerhalb eines ansonsten langen und ereignislosen Videos identifizieren. Mit Bewegungserkennung können in Aufnahmen statischer Kameras Abschnitte des Videos identifiziert werden, wo Bewegung auftritt. Es wird eine JSON-Datei generiert, die Metadaten mit Zeitstempeln und der Umgebungsregion des Ereignisses enthält.

Diese für Sicherheitsvideoaufnahmen konzipierte Technologie kann Bewegung in relevante Ereignisse und falsch-positive Ergebnisse – z. B. Schatten und Beleuchtungsänderungen – kategorisieren. So können Sie Sicherheitswarnungen aus Kameraaufnahmen generieren, ohne unzählige irrelevante Ereignisse auswerten zu müssen, und sind in der Lage, Momente von Interesse aus sehr langen Überwachungsvideos zu extrahieren.

Der MP **Azure Media Motion Detector** befindet sich derzeit in der Vorschauphase.

Dieses Thema enthält Details zu **Azure Media Motion Detector** und zeigt die Verwendung mit Media Services SDK für .NET.

##Motion Detector-Eingabedateien

Videodateien. Derzeit werden folgende Formate unterstützt: MP4, MOV und WMV.

##Motion Detector-Ausgabedateien

Ein Bewegungserkennungsauftrag gibt eine JSON-Datei in das Ausgabeasset zurück, die die Bewegungswarnungen und ihre Kategorien im Video beschreibt. Die Datei enthält Informationen über Zeit und Dauer der erkannten Bewegung im Video.

Derzeit unterstützt die Bewegungserkennung nur die generische Bewegungskategorie, die in der Ausgabe als ***type 2*** bezeichnet wird.

X- und Y-Koordinaten und Größen werden als normalisierter Gleitkommawert zwischen 0,0 und 1,0 aufgeführt. Multiplizieren Sie dies durch die Höhen- und Breitenauflösung des Videos, um den Begrenzungsrahmen für die Region der erkannten Bewegung zu erhalten.

Jede Ausgabe ist in Fragmente aufgeteilt und in Intervalle zum Definieren der Daten innerhalb des Videos unterteilt. Fragmentlängen müssen nicht gleich sein, und können ausgiebige Längen umfassen, wenn keine Bewegung erkannt wird.

Die Motion Detector-API stellt Indikatoren bereit, sobald bewegte Objekte in einem Video mit unbewegtem Hintergrund (z. B. einem Überwachungsvideo) auftreten. Der Motion Detector wird trainiert, falsche Alarme, die z. B. auf Änderungen von Lichtverhältnissen und Schatten beruhen, zu reduzieren. Die aktuellen Einschränkungen der Algorithmen beziehen sich auf Nachtsichtvideos, halbtransparente und kleine Objekte.

###<a id="output_elements"></a>Elemente der Ausgabe-JSON-Datei

In der folgenden Tabelle werden die Elemente der Ausgabe-JSON-Datei beschrieben.

Element|Beschreibung
---|---
Version|Dies bezieht sich auf die Version der Video-API.
Zeitskala|„Ticks“ pro Sekunde des Videos.
Offset|Dies ist der Zeitoffset für Zeitstempel in „Ticks“. In Version 1.0 von Video-APIs wird dies immer 0 sein. In zukünftigen Szenarien, die wir unterstützen, kann sich dieser Wert ändern.
Framerate|Frames des Videos pro Sekunde.
Width, Height|Bezieht sich auf die Breite und Höhe des Videos in Pixeln.
Starten|Der Zeitstempel für den Start in „Ticks“.
Dauer|Die Länge des Ereignisses in „Ticks“.
Intervall|Das Intervall für jeden Eintrag im Ereignis in „Ticks“.
Ereignisse|Jedes Ereignisfragment enthält die innerhalb dieser Zeitspanne erkannte Bewegung.
Typ|Dies ist in der aktuellen Version immer ‘2’ für generische Bewegung. Diese Bezeichnung gibt Video-APIs die Flexibilität zum Kategorisieren der Bewegung in zukünftigen Versionen.
RegionID|Wie bereits dargelegt, wird dies in dieser Version immer 0 sein. Diese Bezeichnung gibt Video-APIs die Flexibilität, in zukünftigen Versionen Bewegung in verschiedenen Regionen zu suchen.
Regionen|Bezieht sich auf den Bereich in Ihrem Video, in dem Bewegung Sie interessiert. In der aktuellen Version von Video-APIs können Sie keine Region angeben, stattdessen wird Bewegung auf der gesamten Oberfläche des Videos erkannt.<br/>– Die ID steht für die Region – in dieser Version gibt es nur die ID 0. <br/>– „Rectangle“ (Rechteck) repräsentiert die Form der Region, in der Bewegung Sie interessiert. In dieser Version ist es immer ein Rechteck. <br/>– Die Region hat die Dimensionen X, Y, Breite und Höhe. Die X- und Y-Koordinaten stellen die linken oberen XY-Koordinaten der Region in einem normalisierten Maßstab von 0,0 bis 1,0 dar. Die Breite und Höhe stellen die Größe der Region in einem normalisierten Maßstab von 0,0 bis 1,0 dar. In der aktuellen Version werden X, Y, Breite und Höhe stets bei 0,0 und 1,1 fixiert.<br/>– „Fragments“: Die Metadaten werden in verschiedene Segmente segmentiert, die als Fragmente bezeichnet werden. Jedes Fragment enthält Startzeitpunkt, Dauer, Intervallnummer und Ereignis(se). Ein Fragment ohne Ereignisse bedeutet, dass zu diesem Startzeitpunkt und während dieser Dauer keine Bewegung erkannt wurde.
Klammern|Jede Klammer stellt ein Intervall im Ereignis dar. Leere Klammern für ein Intervall bedeuten, dass keine Bewegung erkannt wurde.
 

##Aufgabenkonfiguration (Voreinstellung)

Wenn Sie eine Aufgabe mit **Azure Media Motion Detector** erstellen, müssen Sie eine Konfigurationsvoreinstellung angeben. Derzeit können Sie nicht alle Optionen in der Konfigurationsvoreinstellung von Azure Media Motion Detector festlegen. Die folgende Mindestkonfigurationsvoreinstellung müssen Sie festlegen.

	{"version":"1.0"}

##Beispielvideos und Motion Detector-Ausgaben

###Beispiel mit tatsächlicher Bewegung

[Beispiel mit tatsächlicher Bewegung](http://ampdemo.azureedge.net/azuremediaplayer.html?url=https%3A%2F%2Freferencestream-samplestream.streaming.mediaservices.windows.net%2Fd54876c6-89a5-41de-b1f4-f45b6e10a94f%2FGarage.ism%2Fmanifest)

###JSON-Ausgabe

	 {
	 "version": "1.0",
	 "timescale": 60000,
	 "offset": 0,
	 "framerate": 30,
	 "width": 1920,
	 "height": 1080,
	 "regions": [
	   {
	     "id": 0,
	     "type": "rectangle",
	     "x": 0,
	     "y": 0,
	     "width": 1,
	     "height": 1
	   }
	 ],
	 "fragments": [
	   {
	     "start": 0,
	     "duration": 68510
	   },
	   {
	     "start": 68510,
	     "duration": 969999,
	     "interval": 969999,
	     "events": [
	       [
	         {
	           "type": 2,
	           "regionId": 0
	         }
	       ]
	     ]
	   },
	   {
	     "start": 1038509,
	     "duration": 41489
	   }
	 ]
	}

###Beispiel mit falsch-positiven Ergebnissen

[Beispiel mit falsch-positiven Ergebnissen (leichte Änderungen):](http://ampdemo.azureedge.net/azuremediaplayer.html?url=https%3A%2F%2Freferencestream-samplestream.streaming.mediaservices.windows.net%2Ffdc6656b-1c10-4f3f-aa7c-07ba073c1615%2FLivingRoomLight.ism%2Fmanifest&tech=flash)

###JSON-Ausgabe

	{
	    "version": "1.0",
	    "timescale": 30000,
	    "offset": 0,
	    "framerate": 29.97,
	    "width": 1920,
	    "height": 1080,
	    "regions": [
	    {
	        "id": 0,
	        "type": "rectangle",
	        "x": 0,
	        "y": 0,
	        "width": 1,
	        "height": 1
	    }
	    ],
	    "fragments": [
	    {
	        "start": 0,
	        "duration": 320320
	    }
	    ]
	}


##Einschränkungen

- Zu den unterstützten Eingabevideoformaten zählen MP4, MOV und WMV.
- Die Bewegungserkennung ist für Videos mit unbewegtem Hintergrund optimiert. Der Algorithmus konzentriert sich auf die Reduzierung falscher Alarme, wie Änderungen von Licht und Schatten.
- Manche Bewegung wird möglicherweise aufgrund technischer Probleme nicht erkannt; z. B. Nachtsichtvideos, halbtransparente und kleine Objekte.


## Beispielcode

Das folgende Programm zeigt Ihnen, wie Sie folgendes ausführen:

1. Erstellen eines Assets und Hochladen einer Mediendatei in das Asset.
1. Erstellen eines Auftrags mit einer Bewegungserkennungsaufgabe auf Basis einer Konfigurationsdatei, die die folgende JSON-Voreinstellung enthält.
					
		{
		    "version": "1.0"
		}

1. Herunterladen der JSON-Ausgabedateien.
		 
        using System;
		using System.Configuration;
		using System.IO;
		using System.Linq;
		using Microsoft.WindowsAzure.MediaServices.Client;
		using System.Threading;
		using System.Threading.Tasks;
		
		namespace VideoMotionDetection
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
		
		            // Run the VideoMotionDetection job.
		            var asset = RunVideoMotionDetectionJob(@"C:\supportFiles\VideoMotionDetection\BigBuckBunny.mp4",
		                                        @"C:\supportFiles\VideoMotionDetection\config.json");
		
		            // Download the job output asset.
		            DownloadAsset(asset, @"C:\supportFiles\VideoMotionDetection\Output");
		        }
		
		        static IAsset RunVideoMotionDetectionJob(string inputMediaFilePath, string configurationFile)
		        {
		            // Create an asset and upload the input media file to storage.
		            IAsset asset = CreateAssetAndUploadSingleFile(inputMediaFilePath,
		                "My Video Motion Detection Input Asset",
		                AssetCreationOptions.None);
		
		            // Declare a new job.
		            IJob job = _context.Jobs.Create("My Video Motion Detection Job");
		
		            // Get a reference to Azure Media Motion Detector.
		            string MediaProcessorName = "Azure Media Motion Detector";
		
		            var processor = GetLatestMediaProcessorByName(MediaProcessorName);
		
		            // Read configuration from the specified file.
		            string configuration = File.ReadAllText(configurationFile);
		
		            // Create a task with the encoding details, using a string preset.
		            ITask task = job.Tasks.AddNew("My Video Motion Detection Task",
		                processor,
		                configuration,
		                TaskOptions.None);
		
		            // Specify the input asset.
		            task.InputAssets.Add(asset);
		
		            // Add an output asset to contain the results of the job.
		            task.OutputAssets.AddNew("My Video Motion Detectoion Output Asset", AssetCreationOptions.None);
		
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


##Media Services-Lernpfade

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##Feedback geben

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

##Verwandte Links

[Azure Media Services Analytics – Übersicht](media-services-analytics-overview.md)

[Azure Media Analytics-Demos](http://azuremedialabs.azurewebsites.net/demos/Analytics.html)

<!---HONumber=AcomDC_0629_2016-->