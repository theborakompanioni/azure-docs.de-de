---

title: Verwalten von Streamingendpunkten mit .NET SDK | Microsoft Docs
description: Diese Thema zeigt, wie Sie Streamingendpunkte mithilfe des Azure-Portals verwalten.
services: media-services
documentationcenter: 
author: Juliako
writer: juliako
manager: erikre
editor: 
ms.assetid: 0da34a97-f36c-48d0-8ea2-ec12584a2215
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/05/2017
ms.author: juliako
translationtype: Human Translation
ms.sourcegitcommit: 4f61f7c0fd50065d341ae1ce182a033395857579
ms.openlocfilehash: 68011ef634b1f3bdeb7c219a46e1307698a12f7e


---


# <a name="manage-streaming-endpoints-with-net-sdk"></a>Verwalten von Streamingendpunkten mit .NET SDK

>[!NOTE]
>Sehen Sie sich zunächst das Thema [Übersicht](media-services-streaming-endpoints-overview.md) an. Lesen Sie außerdem [StreamingEndpoint](https://docs.microsoft.com/rest/api/media/operations/streamingendpoint).

Der Code in diesem Thema zeigt, wie die folgenden Aufgaben mithilfe des Azure Media Services .NET SDK ausgeführt werden:

- Untersuchen des Standard-Streamingendpunkts.
- Erstellen/Hinzufügen eines neuen Streamingendpunkts.

    Es empfehlen sich ggf. mehrere Streamingendpunkte, wenn Sie planen, mit verschiedenen CDNs oder mit einem CDN und Direktzugriff zu arbeiten.

    > [!NOTE]
    > Abgerechnet werden nur ausgeführte Streamingendpunkte.
    
- Aktualisieren des Streamingendpunkts.
    
    Rufen Sie unbedingt die Update()-Funktion auf.

- Löschen des Streamingendpunkts.

    >[!NOTE]
    >Der standardmäßige Streamingendpunkt kann nicht gelöscht werden.

Weitere Informationen zum Skalieren des Streamingendpunkts finden Sie in [diesem](media-services-portal-scale-streaming-endpoints.md) Thema.


###<a name="set-up-the-visual-studio-project"></a>Einrichten des Visual Studio-Projekts

1. Erstellen Sie eine neue C#-Konsolenanwendung in Visual Studio 2015.  
2. Erstellen Sie die Projektmappe.
3. Verwenden Sie **NuGet**, um das [aktuelle Azure Media Services .NET SDK-Paket](https://www.nuget.org/packages/windowsazure.mediaservices/) zu installieren.   
4. Fügen Sie der CONFIG-Datei den appSettings-Abschnitt hinzu, und aktualisieren Sie den Media Services-Namen und die Schlüsselwerte. 
    
        <appSettings>
          <add key="MediaServicesAccountName" value="Media-Services-Account-Name"/>
          <add key="MediaServicesAccountKey" value="Media-Services-Account-Key"/>
        </appSettings>

###<a name="add-code-that-manages-streaming-endpoints"></a>Hinzufügen von Code, der Streamingendpunkte verwaltet
    
Ersetzen Sie den Code in „Program.cs“ durch den folgenden Code:

    using System;
    using System.Collections.Generic;
    using System.Configuration;
    using System.Linq;
    using Microsoft.WindowsAzure.MediaServices.Client;
    using Microsoft.WindowsAzure.MediaServices.Client.Live;
    
    namespace AMSStreamingEndpoint
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
    
                var defaultStreamingEndpoint = _context.StreamingEndpoints.Where(s=>s.Name.Contains("default")).FirstOrDefault();
                ExamineStreamingEndpoint(defaultStreamingEndpoint);
    
                IStreamingEndpoint newStreamingEndpoint = AddStreamingEndpoint();
                UpdateStreamingEndpoint(newStreamingEndpoint);
                DeleteStreamingEndpoint(newStreamingEndpoint);
            }
    
            static public void ExamineStreamingEndpoint(IStreamingEndpoint streamingEndpoint)
            {
                Console.WriteLine(streamingEndpoint.Name);
                Console.WriteLine(streamingEndpoint.StreamingEndpointVersion);
                Console.WriteLine(streamingEndpoint.FreeTrialEndTime);
                Console.WriteLine(streamingEndpoint.ScaleUnits);
                Console.WriteLine(streamingEndpoint.CdnProvider);
                Console.WriteLine(streamingEndpoint.CdnProfile);
                Console.WriteLine(streamingEndpoint.CdnEnabled);
            }
    
            static public IStreamingEndpoint AddStreamingEndpoint()
            {
                var name = "StreamingEndpoint" + DateTime.UtcNow.ToString("hhmmss");
                var option = new StreamingEndpointCreationOptions(name, 1)
                {
                    StreamingEndpointVersion = new Version("2.0"),
                    CdnEnabled = true,
                    CdnProfile = "CdnProfile",
                    CdnProvider = CdnProviderType.PremiumVerizon
                };
    
                var streamingEndpoint = _context.StreamingEndpoints.Create(option);
    
                return streamingEndpoint;
            }
    
            static public void UpdateStreamingEndpoint(IStreamingEndpoint streamingEndpoint)
            {
                if(streamingEndpoint.StreamingEndpointVersion == "1.0")
                    streamingEndpoint.StreamingEndpointVersion = "2.0";
    
                streamingEndpoint.CdnEnabled = false;
                streamingEndpoint.Update();
            }
    
            static public void DeleteStreamingEndpoint(IStreamingEndpoint streamingEndpoint)
            {
                streamingEndpoint.Delete();
            }
        }
    }
    

## <a name="next-steps"></a>Nächste Schritte
Überprüfen Sie die Media Services-Lernpfade.

[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Feedback geben
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]




<!--HONumber=Jan17_HO2-->


