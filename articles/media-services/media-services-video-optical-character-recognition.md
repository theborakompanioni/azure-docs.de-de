<properties
	pageTitle="Verwenden von Azure Media Analytics zum Konvertieren von Textinhalten in Videodateien in digitalen Text | Microsoft Azure"
	description="Azure Media Analytics OCR (Optical Character Recognition, Optische Zeichenerkennung) dient zum Umwandeln von Textinhalten in Videodateien in bearbeitbaren, durchsuchbaren digitalen Text. Dadurch können Sie die Extraktion aussagekräftiger Metadaten aus dem Videosignal Ihrer Medien automatisieren."
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
	ms.date="09/26/2016"   
	ms.author="juliako"/>
 
#Verwenden von Azure Media Analytics zum Konvertieren von Textinhalten in Videodateien in digitalen Text 

##Übersicht

Wenn Sie Textinhalt aus Ihren Videodateien extrahieren und einen bearbeit- und durchsuchbaren digitalen Text erstellen müssen, sollten Sie Azure Media Analytics OCR (Optical Character Recognition, Optische Zeichenerkennung) verwenden. Dieser Azure-Medienprozessor erkennt Textinhalte in Ihren Videodateien und generiert Textdateien, die Sie nutzen können. Dank der optischen Zeichenerkennung können Sie die Extraktion aussagekräftiger Metadaten aus dem Videosignal Ihrer Medien automatisieren.

Bei Verwendung in Verbindung mit einer Suchmaschine können Sie Ihre Medien problemlos anhand von Text indizieren und die Erkennbarkeit Ihrer Inhalte verbessern. Dies ist äußerst nützlich bei sehr textlastigem Video, z.B. bei einer Video- oder Bildschirmaufnahme einer Diashow. Der Azure OCR-Medienprozessor ist für digitalen Text optimiert.

Der **Azure Media OCR**-Medienprozessor befindet sich derzeit in der Vorschauphase.

Dieses Thema enthält Details zu **Azure Media OCR** und veranschaulicht die Verwendung zusammen mit dem Media Services SDK für .NET. Weitere Informationen und Beispiele finden Sie in [diesem Blog](https://azure.microsoft.com/blog/announcing-video-ocr-public-preview-new-config/).

##OCR-Eingabedateien

Videodateien. Derzeit werden folgende Formate unterstützt: MP4, MOV und WMV.

##Aufgabenkonfiguration 

Aufgabenkonfiguration (Voreinstellung). Wenn Sie eine Aufgabe mit **Azure Media OCR** erstellen, müssen Sie mithilfe von JSON oder XML eine Konfigurationsvoreinstellung angeben.

###Beschreibungen der Attribute

Attributname|Beschreibung
---|---
Language|(Optional) Beschreibt die Sprache des Texts, nach dem gesucht wird. Eine der folgenden: AutoDetect (Standard), Arabisch, Chinesisch vereinfacht, Chinesisch traditionell, Dänisch, Deutsch, Englisch, Finnisch, Französisch, Griechisch, Italienisch, Japanisch, Koreanisch, Niederländisch, Norwegisch, Polnisch, Portugiesisch, Rumänisch, Russisch, Schwedisch, Serbisch (lateinisch), Serbisch (kyrillisch), Slowakisch, Spanisch, Tschechisch, Türkisch, Ungarisch.
TextOrientation|(Optional) Beschreibt die Ausrichtung des Texts, nach dem gesucht wird. „Left“ bedeutet, dass alle Buchstaben nach links ausgerichtet sind. Standardtext (z. B. wie in einem Buch) hat die Ausrichtung „Up“. Eine der folgenden Optionen: AutoDetect (Standard), Up, Right, Down, Left.
TimeInterval|(Optional) Beschreibt die Abtastrate. Standard ist alle 0,5 Sekunden.<br/>JSON-Format: HH:mm:ss.SSS (Standard 00:00:00.500)<br/>XML-Format – W3C XSD-Dauer, primitiv (Standard PT0.5)
DetectRegions|(Optional) Ein Array von „DetectRegion“-Objekten zur Angabe von Bereichen innerhalb des Videoframes, in denen Text erkannt werden soll.<br/>Ein „DetectRegion“-Objekt besteht aus den folgenden vier ganzzahligen Werten:<br/>Left – Pixel vom linken Rand<br/>Top – Pixel vom oberen Rand<br/>Width – Breite des Bereichs in Pixel<br/>Height – Höhe des Bereichs in Pixel

####Beispiel für JSON-Voreinstellung
	 	
	{
	    'Version':'1.0', 
	    'Options': 
	    {
	    'Language':'English', 
	        'TimeInterval':'00:00:01.5',
	        'DetectRegions': 
	         [
	            {'Left':'1','Top':'1','Width':'1','Height':'1'},
	            {'Left':'2','Top':'2','Width':'2','Height':'2'}
	         ],
	        'TextOrientation':'Up'
	    }
	}

####Beispiel für XML-Voreinstellung

	<?xml version=""1.0"" encoding=""utf-16""?>
	<VideoOcrPreset xmlns:xsi=""http://www.w3.org/2001/XMLSchema-instance"" xmlns:xsd=""http://www.w3.org/2001/XMLSchema"" Version=""1.0"" xmlns=""http://www.windowsazure.com/media/encoding/Preset/2014/03"">
	  <Options>
	     <Language>English</Language>
	     <TimeInterval>PT1.5S</TimeInterval>
	     <DetectRegions>
	         <DetectRegion>
	               <Left>1</Left>
	               <Top>1</Top>
	               <Width>1</Width>
	               <Height>1</Height>
	        </DetectRegion>
	   </DetectRegions>
	   <TextOrientation>Up</TextOrientation>
	  </Options>
	</VideoOcrPreset>

##OCR-Ausgabedateien

Die Ausgabe des OCR-Medienprozessors ist eine JSON-Datei.

###Elemente der JSON-Ausgabedatei

Die Video OCR-Ausgabe liefert zeitsegmentierte Daten zu in Ihrem Video gefundenen Zeichen. Sie können Attribute wie Sprache oder Ausrichtung verwenden, um sich exakt auf die Wörter zu konzentrieren, die Sie analysieren möchten.

Die Ausgabe enthält die folgenden Attribute:

Element|Beschreibung
---|---
Zeitskala|„Teilstriche“ pro Sekunde des Videos
Offset|Zeitoffset für Zeitstempel In Version 1.0 von Video-APIs wird dies immer 0 sein.
Framerate|Frames pro Sekunde des Videos
width|Breite des Videos in Pixel
height|Höhe des Videos in Pixel
Fragments|Array zeitbasierter Segmente des Videos, in die die Metadaten aufgeteilt werden
Start|Startzeit eines Fragments in Teilstrichen
duration|Dauer eines Fragments in Teilstrichen
interval|Intervall jedes Ereignisses innerhalb des gegebenen Fragments
events|Array mit Bereichen
region|Objekt, das die erkannten Wörter oder Ausdrücke darstellt
language|Sprache des Texts innerhalb eines Bereichs
orientation|Ausrichtung des Texts innerhalb eines Bereichs
lines|Array von Zeilen des Texts innerhalb eines Bereichs
Text|Der tatsächliche Text

###Beispiel für die JSON-Ausgabe

Das folgende Ausgabebeispiel enthält die allgemeinen Videoinformationen und mehrere Videofragmente. Es enthält in jedem Videofragment alle Bereiche, die vom OCR-Medienprozessor samt Sprache und Textausrichtung erkannt werden. Der Bereich enthält auch alle Zeilen mit Wörtern in diesem Bereich samt Zeilentext, Zeilenposition und Informationen zu jedem Wort (Inhalt, Position und Konfidenz) in der jeweiligen Zeile. Es folgt ein Beispiel mit von mir hinzugefügten Inlinekommentaren.

	{
		"version": 1, 
		"timescale": 90000, 
		"offset": 0, 
		"framerate": 30, 
		"width": 640, 
		"height": 480,  // general video information
		"fragments": [
		    {
		        "start": 0, 
		        "duration": 180000, 
		        "interval": 90000,  // the time information about this fragment
		        "events": [
		            [
		               { 
		                    "region": { // the detected region array in this fragment 
		                        "language": "English",  // region language
		                        "orientation": "Up",  // text orientation
		                        "lines": [  // line information array in this region, including the text and the position
		                            {
		                                "text": "One Two", 
		                                "left": 10, 
		                                "top": 10, 
		                                "right": 210, 
		                                "bottom": 110, 
		                                "word": [  // word information array in this line
		                                    {
		                                        "text": "One", 
		                                        "left": 10, 
		                                        "top": 10, 
		                                        "right": 110, 
		                                        "bottom": 110, 
		                                        "confidence": 900
		                                    }, 
		                                    {
		                                        "text": "Two", 
		                                        "left": 110, 
		                                        "top": 10, 
		                                        "right": 210, 
		                                        "bottom": 110, 
		                                        "confidence": 910
		                                    }
		                                ]
		                            }
		                        ]
		                    }
		                }
		            ]
		        ]
		    }
		]
	}

## Beispielcode

Das folgende Programm zeigt Ihnen, wie Sie folgendes ausführen:

1. Erstellen eines Assets und Hochladen einer Mediendatei in das Asset.
1. Erstellt einen Auftrag mit einer OCR-Konfigurations-/Voreinstellungsdatei.
1. Herunterladen der JSON-Ausgabedateien.
		 
        using System;
		using System.Configuration;
		using System.IO;
		using System.Linq;
		using Microsoft.WindowsAzure.MediaServices.Client;
		using System.Threading;
		using System.Threading.Tasks;
		
		namespace OCR
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
		
		            // Run the OCR job.
		            var asset = RunOCRJob(@"C:\supportFiles\OCR\presentation.mp4",
		                                        @"C:\supportFiles\OCR\config.json");
		
		            // Download the job output asset.
		            DownloadAsset(asset, @"C:\supportFiles\OCR\Output");
		        }
		
		        static IAsset RunOCRJob(string inputMediaFilePath, string configurationFile)
		        {
		            // Create an asset and upload the input media file to storage.
		            IAsset asset = CreateAssetAndUploadSingleFile(inputMediaFilePath,
		                "My OCR Input Asset",
		                AssetCreationOptions.None);
		
		            // Declare a new job.
		            IJob job = _context.Jobs.Create("My OCR Job");
		
		            // Get a reference to Azure Media OCR.
		            string MediaProcessorName = "Azure Media OCR";
		
		            var processor = GetLatestMediaProcessorByName(MediaProcessorName);
		
		            // Read configuration from the specified file.
		            string configuration = File.ReadAllText(configurationFile);
		
		            // Create a task with the encoding details, using a string preset.
		            ITask task = job.Tasks.AddNew("My OCR Task",
		                processor,
		                configuration,
		                TaskOptions.None);
		
		            // Specify the input asset.
		            task.InputAssets.Add(asset);
		
		            // Add an output asset to contain the results of the job.
		            task.OutputAssets.AddNew("My OCR Output Asset", AssetCreationOptions.None);
		
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

<!---HONumber=AcomDC_0928_2016-->