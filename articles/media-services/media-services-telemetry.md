<properties pageTitle="Azure Media Services-Telemetrie mit .NET | Microsoft Azure" 
	description="In diesem Artikel erfahren Sie, wie Sie die Azure Media Services-Telemetrie verwendet." 
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
	ms.date="07/23/2016"   
	ms.author="juliako"/>

# Azure Media Services-Telemetrie mit .NET
 
## Übersicht

Die Media Services-Telemetrie/-Überwachung ermöglicht Media Services-Kunden den Zugriff auf Metrikdaten für die entsprechenden Dienste. Die aktuelle Version unterstützt Telemetriedaten für die Entitäten „Channel“ und „StreamingEndpoint“. Die Telemetrie kann auf Komponentenebene konfiguriert werden. Dabei stehen zwei Detailstufen zur Verfügung: „Normal“ und „Ausführlich“. Die aktuelle Version unterstützt nur „Normal“.

Die Telemetrie wird in ein vom Kunden angegebenes Azure-Speicherkonto geschrieben. (Ein Speicherkonto muss mit dem Media Services-Konto verknüpft sein.) Die Telemetrie wird in eine Azure Storage-Tabelle unter dem angegebenen Speicherkonto geschrieben. Das Telemetriesystem erstellt für jeden neuen Tag (Grundlage: 00:00 UTC) eine separate Tabelle. Beispiel: „TelemetryMetrics20160321“, wobei „20160321“ das Erstellungsdatum der Tabelle ist. Für jeden Tag wird eine separate Tabelle erstellt.

Das Telemetriesystem bietet weder eine Datenaufbewahrung noch eine automatische Löschung alter Datensätze. Aus diesem Grund müssen Sie die Verwaltung und das Löschen alter Datensätze selbst übernehmen. Die separaten Tabellen für die einzelnen Tage vereinfachen das Löschen alter Datensätze. Sie müssen einfach nur alte Tabellen löschen.

In diesem Thema erfahren Sie, wie Sie die Telemetrie für die angegebenen AMS-Dienste aktivieren und die Metriken mithilfe von .NET abfragen.

## Konfigurieren der Telemetrie für ein Media Services-Konto

Zum Aktivieren der Telemetrie sind folgende Schritte erforderlich:

- Ermitteln Sie die Anmeldeinformationen des mit dem Media Services-Konto verknüpften Speicherkontos.
- Erstellen Sie einen Benachrichtigungsendpunkt, bei dem **EndPointType** auf **AzureTable** festgelegt ist und endPointAddress auf die Speichertabelle verweist.

	    INotificationEndPoint notificationEndPoint = 
	                  _context.NotificationEndPoints.Create("monitoring", 
	                  NotificationEndPointType.AzureTable,
	                  "https://" + _mediaServicesStorageAccountName + ".table.core.windows.net/");

- Erstellen Sie eine Überwachungskonfigurationseinstellung für die zu überwachenden Dienste. Mehrere Überwachungskonfigurationseinstellungen sind nicht zulässig.
  
        IMonitoringConfiguration monitoringConfiguration = _context.MonitoringConfigurations.Create(notificationEndPoint.Id,
            new List<ComponentMonitoringSetting>()
            {
                new ComponentMonitoringSetting(MonitoringComponent.Channel, MonitoringLevel.Normal),
                new ComponentMonitoringSetting(MonitoringComponent.StreamingEndpoint, MonitoringLevel.Normal)
            });

## Verwenden von Telemetriedaten

Telemetriedaten werden in eine Azure-Speichertabelle in dem Speicherkonto geschrieben, das bei der Konfiguration der Telemetrie für das Media Services-Konto angegeben wurde. Das Telemetriesystem erstellt für jeden neuen Tag (Grundlage: 00:00 UTC) eine separate Tabelle. Beispiel: „TelemetryMetrics20160321“, wobei „20160321“ das Erstellungsdatum der Tabelle ist. Für jeden Tag wird eine separate Tabelle erstellt.

Sie können die Tabellen nach den folgenden Metrikinformationen abfragen.

### StreamingEndpoint-Protokoll

Folgende StreamingEndPoint-Metriken können abgefragt werden:

Eigenschaft|Beschreibung|Beispielwert
---|---|---
**PartitionKey**|Ruft den Partitionsschlüssel des Datensatzes ab.|60b71b0f6a0e4d869eb0645c16d708e1\_6efed125eef44fb5b61916edc80e6e23
**Zeilenschlüssel**|Ruft den Zeilenschlüssel des Datensatzes ab.|00959\_00000
**AccountId**|Ruft die Media Services-Konto-ID ab.|6efed125-eef4-4fb5-b619-16edc80e6e23
**StreamingEndpointId**|Ruft die Media Services-Streamingendpunkt-ID ab.|d17ec9e4-a5d4-033d-0c36-def70229f06f
**ObservedTime**|Ruft den beobachteten Zeitraum der Metrik ab.|1/20/16 23:44:01
**HostName**|Ruft den Hostnamen des Streamingendpunkts ab.|builddemoserver.origin.mediaservices.windows.net
**StatusCode**|Ruft den Statuscode ab.|200
**ResultCode**|Ruft den Ergebniscode ab.|S\_OK
**RequestCount**|Ruft die Anzahl von Anforderungen ab.|3
**BytesSent**|Ruft die gesendeten Bytes ab.|2987358
**ServerLatency**|Ruft die Serverlatenz ab (einschließlich Speicher).|129
**EndToEndLatency**|Ruft die Anforderungszeit insgesamt ab.|250


### Livekanaltakt

Folgende Livekanalmetriken können abgefragt werden:

Eigenschaft|Beschreibung|Beispielwert
---|---|---
**PartitionKey**|Ruft den Partitionsschlüssel des Datensatzes ab.|60b71b0f6a0e4d869eb0645c16d708e1\_0625cc45918e4f98acfc9a33e8066628
**Zeilenschlüssel**|Ruft den Zeilenschlüssel des Datensatzes ab.|13872\_00005
**AccountId**|Ruft die Media Services-Konto-ID ab.|6efed125-eef4-4fb5-b619-16edc80e6e23
**ChannelId**|Ruft die Media Services-Kanal-ID ab.|
**ObservedTime**|Ruft den beobachteten Zeitraum der Metrik ab.|1/21/2016 20:08:49
**CustomAttributes**|Ruft die benutzerdefinierten Attribute ab.|
**TrackType**|Ruft den Nachverfolgungstyp ab.|video
**TrackName**|Ruft den Nachverfolgungsnamen ab.|video
**Bitrate**|Ruft die Bitrate ab.|785000
**IncomingBitrate**|Ruft die eingehende Bitrate ab.|784548
**OverlapCount**|Ruft die Anzahl von Überlappungen ab.|0
**DiscontinuityCount**|Ruft die Anzahl von Unterbrechungen ab.|0
**LastTimestamp**|Ruft den letzten Zeitstempel ab.|1800488800
 
## StreamingEndpoint-Metrikbeispiel
		
	using System;
	using System.Collections.Generic;
	using System.Configuration;
	using System.Linq;
	using Microsoft.WindowsAzure.MediaServices.Client;
	
	namespace AMSMetrics
	{
	    class Program
	    {
	        // Read values from the App.config file.
	        private static readonly string _mediaServicesAccountName =
	            ConfigurationManager.AppSettings["MediaServicesAccountName"];
	        private static readonly string _mediaServicesAccountKey =
	            ConfigurationManager.AppSettings["MediaServicesAccountKey"];
	        private static readonly string _mediaServicesAccountID =
	            ConfigurationManager.AppSettings["MediaServicesAccountID"];
	        private static readonly string _streamingEndpointID =
	            ConfigurationManager.AppSettings["StreamingEndpointID"];
	        private static readonly string _mediaServicesStorageAccountName =
	            ConfigurationManager.AppSettings["StorageAccountName"];
	        private static readonly string _mediaServicesStorageAccountKey =
	            ConfigurationManager.AppSettings["StorageAccountKey"];
	
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
	

	
	            var monitoringConfigurations = _context.MonitoringConfigurations;
	            IMonitoringConfiguration monitoringConfiguration = null;
	
	            // No more than one monitoring configuration settings is allowed.
	            if (monitoringConfigurations.ToArray().Length != 0)
	            {
	                monitoringConfiguration = _context.MonitoringConfigurations.FirstOrDefault();
	            }
	            else
	            {
		            INotificationEndPoint notificationEndPoint = 
		                          _context.NotificationEndPoints.Create("monitoring", 
								  NotificationEndPointType.AzureTable, GetTableEndPoint());

	                monitoringConfiguration = _context.MonitoringConfigurations.Create(notificationEndPoint.Id,
	                    new List<ComponentMonitoringSetting>()
	                    {
	                        new ComponentMonitoringSetting(MonitoringComponent.StreamingEndpoint, MonitoringLevel.Normal)
	                    });
	            }
	
	            //Print metrics for a Streaming Endpoint.
	            PrintStreamingEndpointMetrics();
	
	            Console.ReadLine();
	        }
	
	        private static string GetTableEndPoint()
	        {
	            return "https://" + _mediaServicesStorageAccountName + ".table.core.windows.net/";
	        }
	
	        private static void PrintStreamingEndpointMetrics()
	        {
	            var end = DateTime.UtcNow;
	            var start = DateTime.UtcNow.AddHours(-5);
	
	            // Get some streaming endpoint metrics.
	            var res = _context.StreamingEndPointRequestLogs.GetStreamingEndPointMetrics(
	                    GetTableEndPoint(),
	                    _mediaServicesStorageAccountKey,
	                    _mediaServicesAccountID,
	                    _streamingEndpointID,
	                    start,
	                    end);
	
	
	            Console.Title = "Streaming endpoint metrics:";
	
	            foreach (var log in res)
	            {
	                Console.WriteLine("AccountId: {0}", log.AccountId);
	                Console.WriteLine("BytesSent: {0}", log.BytesSent);
	                Console.WriteLine("EndToEndLatency: {0}", log.EndToEndLatency);
	                Console.WriteLine("HostName: {0}", log.HostName);
	                Console.WriteLine("ObservedTime: {0}", log.ObservedTime);
	                Console.WriteLine("PartitionKey: {0}", log.PartitionKey);
	                Console.WriteLine("RequestCount: {0}", log.RequestCount);
	                Console.WriteLine("ResultCode: {0}", log.ResultCode);
	                Console.WriteLine("RowKey: {0}", log.RowKey);
	                Console.WriteLine("ServerLatency: {0}", log.ServerLatency);
	                Console.WriteLine("StatusCode: {0}", log.StatusCode);
	                Console.WriteLine("StreamingEndpointId: {0}", log.StreamingEndpointId);
	                Console.WriteLine();
	            }
	
	            Console.WriteLine();
	        }

	        private static void PrintChannelMetrics()
	        {
	            var end = DateTime.UtcNow;
	            var start = DateTime.UtcNow.AddHours(-5);
	
	            // Get some channel metrics.
	            var channelMetrics = _context.ChannelMetrics.GetChannelMetrics(
	                GetTableEndPoint(),
	                _mediaServicesStorageAccountKey,
	                _mediaServicesAccountName,
	                _context.Channels.FirstOrDefault().Id,
	                start,
	                end);
	
	            // Print the channel metrics.
	            Console.WriteLine("Channel metrics:");
	
	            foreach (var channelHeartbeat in channelMetrics.OrderBy(x => x.ObservedTime))
	            {
	                Console.WriteLine(
	                    "    Observed time: {0}, Last timestamp: {1}, Incoming bitrate: {2}",
	                    channelHeartbeat.ObservedTime,
	                    channelHeartbeat.LastTimestamp,
	                    channelHeartbeat.IncomingBitrate);
	            }
	
	            Console.WriteLine();
	        }
	    }
	}


##Feedback geben

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

##Nächster Schritt
 
Unter den Azure Media Services-Lernpfaden finden Sie Informationen zu weiteren AMS-Features.

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

<!---HONumber=AcomDC_0727_2016-->