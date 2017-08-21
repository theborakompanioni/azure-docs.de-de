---
title: Konfigurieren von Azure Media Services-Telemetrie mit .NET | Microsoft-Dokumentation
description: In diesem Artikel erfahren Sie, wie Sie die Azure Media Services-Telemetrie mithilfe des .NET SDK verwendet.
services: media-services
documentationcenter: 
author: Juliako
manager: erikre
editor: 
ms.assetid: f8f55e37-0714-49ea-bf4a-e6c1319bec44
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/18/2017
ms.author: juliako
ms.translationtype: HT
ms.sourcegitcommit: c3ea7cfba9fbf1064e2bd58344a7a00dc81eb148
ms.openlocfilehash: b346b32bd5580d25ac24786fce5aff932b3c15ae
ms.contentlocale: de-de
ms.lasthandoff: 07/19/2017

---

# <a name="configuring-azure-media-services-telemetry-with-net"></a>Konfigurieren der Azure Media Services-Telemetrie mit .NET

Dieses Thema beschreibt allgemeine Schritte, die Sie zum Konfigurieren der Azure Media Services-Telemetrie (AMS) mit .NET SDK ausführen können. 

>[!NOTE]
>Ausführliche Erläuterungen dazu, worum es sich bei AMS-Telemetrie handelt und wie sie genutzt wird, finden Sie im Thema mit der [Übersicht](media-services-telemetry-overview.md).

Sie können Telemetriedaten in einer der folgenden Arten verwenden:

- Lesen Sie Daten direkt aus dem Azure-Tabellenspeicher (z.B. mithilfe des Speicher-SDK). Eine Beschreibung der Telemetriespeichertabellen finden Sie in [diesem](https://msdn.microsoft.com/library/mt742089.aspx) Thema unter **Consuming telemetry information** (Nutzung von Telemetrieinformationen).

oder

- Verwenden Sie die Unterstützung für das Lesen von Speicherdaten im .NET SDK der Media Services. In diesem Thema erfahren Sie, wie Sie die Telemetrie für das angegebene AMS-Konto aktivieren und die Metriken mithilfe des .NET SDK der Azure Media Services abfragen.  

## <a name="configuring-telemetry-for-a-media-services-account"></a>Konfigurieren der Telemetrie für ein Media Services-Konto

Zum Aktivieren der Telemetrie sind folgende Schritte erforderlich:

- Ermitteln Sie die Anmeldeinformationen des mit dem Media Services-Konto verknüpften Speicherkontos. 
- Erstellen Sie einen Benachrichtigungsendpunkt, bei dem **EndPointType** auf **AzureTable** festgelegt ist, und „endPointAddress“ auf die Speichertabelle verweist.

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

## <a name="consuming-telemetry-information"></a>Verwenden von Telemetriedaten

Informationen zur Nutzung von Telemetrieinformationen finden Sie in [diesem](media-services-telemetry-overview.md) Thema.

## <a name="create-and-configure-a-visual-studio-project"></a>Erstellen und Konfigurieren eines Visual Studio-Projekts

1. Richten Sie Ihre Entwicklungsumgebung ein, und füllen Sie die Datei „app.config“ mit Verbindungsinformationen, wie unter [Media Services-Entwicklung mit .NET](media-services-dotnet-how-to-use.md) beschrieben. 

2. Fügen Sie den **appSettings** in Ihrer Datei „app.config“ das folgende Element hinzu:

    <add key="StorageAccountName" value="storage_name" />
 
## <a name="example"></a>Beispiel  
    
Im folgenden Thema erfahren Sie, wie Sie die Telemetrie für das angegebene AMS-Konto aktivieren und die Metriken mithilfe des .NET SDK der Azure Media Services abfragen.  

    using System;
    using System.Collections.Generic;
    using System.Configuration;
    using System.Linq;
    using Microsoft.WindowsAzure.MediaServices.Client;

    namespace AMSMetrics
    {
        class Program
        {
        private static readonly string _AADTenantDomain =
            ConfigurationManager.AppSettings["AADTenantDomain"];
        private static readonly string _RESTAPIEndpoint =
            ConfigurationManager.AppSettings["MediaServiceRESTAPIEndpoint"];

        private static readonly string _mediaServicesStorageAccountName =
            ConfigurationManager.AppSettings["StorageAccountName"];

        // Field for service context.
        private static CloudMediaContext _context = null;

        private static IStreamingEndpoint _streamingEndpoint = null;
        private static IChannel _channel = null;

        static void Main(string[] args)
        {
            var tokenCredentials = new AzureAdTokenCredentials(_AADTenantDomain, AzureEnvironments.AzureCloudEnvironment);
            var tokenProvider = new AzureAdTokenProvider(tokenCredentials);

            _context = new CloudMediaContext(new Uri(_RESTAPIEndpoint), tokenProvider);

            _streamingEndpoint = _context.StreamingEndpoints.FirstOrDefault();
            _channel = _context.Channels.FirstOrDefault();

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
                    new ComponentMonitoringSetting(MonitoringComponent.Channel, MonitoringLevel.Normal),
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
            Console.WriteLine(string.Format("Telemetry for streaming endpoint '{0}'", _streamingEndpoint.Name));

            DateTime timerangeEnd = DateTime.UtcNow;
            DateTime timerangeStart = DateTime.UtcNow.AddHours(-5);

            // Get some streaming endpoint metrics.
            var telemetry = _streamingEndpoint.GetTelemetry();

            var res = telemetry.GetStreamingEndpointRequestLogs(timerangeStart, timerangeEnd);

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
            if (_channel == null)
            {
            Console.WriteLine("There are no channels in this AMS account");
            return;
            }

            Console.WriteLine(string.Format("Telemetry for channel '{0}'", _channel.Name));

            DateTime timerangeEnd = DateTime.UtcNow; 
            DateTime timerangeStart = DateTime.UtcNow.AddHours(-5);

            // Get some channel metrics.
            var telemetry = _channel.GetTelemetry();

            var channelMetrics = telemetry.GetChannelHeartbeats(timerangeStart, timerangeEnd);

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


## <a name="next-steps"></a>Nächste Schritte

[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Feedback geben

[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

