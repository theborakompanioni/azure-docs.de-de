---
title: "Veröffentlichen von Azure Media Services-Inhalten mit .NET | Microsoft-Dokumentation"
description: "Erfahren Sie, wie Sie einen Locator erstellen, der zum Generieren einer Streaming-URL verwendet wird. Die Codebeispiele sind in C# geschrieben und verwenden das Media Services SDK für .NET."
author: juliako
manager: erikre
editor: 
services: media-services
documentationcenter: 
ms.assetid: c53b1f83-4cb1-4b09-840f-9c145b7d6f8d
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/09/2017
ms.author: juliako
ms.translationtype: Human Translation
ms.sourcegitcommit: aaf97d26c982c1592230096588e0b0c3ee516a73
ms.openlocfilehash: 37f6cd3a25c36fe27c9c711a430a2fc11e50906e
ms.contentlocale: de-de
ms.lasthandoff: 04/27/2017

---
# <a name="publish-azure-media-services-content-using-net"></a>Veröffentlichen von Azure Media Services-Inhalten mit .NET
> [!div class="op_single_selector"]
> * [REST](media-services-rest-deliver-streaming-content.md)
> * [.NET](media-services-deliver-streaming-content.md)
> * [Portal](media-services-portal-publish.md)
> 
> 

## <a name="overview"></a>Übersicht
Sie können einen MP4-Satz mit adaptiver Bitrate streamen, indem Sie einen OnDemand-Streaminglocator und eine Streaming-URL erstellen. Im Thema [Codieren eines Medienobjekts](media-services-encode-asset.md) wird die Codierung in einen MP4-Satz mit adaptiver Bitrate erläutert. 

> [!NOTE]
> Wenn Ihr Inhalt verschlüsselt ist, konfigurieren Sie vor dem Erstellen eines Locators die Bereitstellungsrichtlinie für Medienobjekte (wie in [diesem Thema](media-services-dotnet-configure-asset-delivery-policy.md) beschrieben). 
> 
> 

Sie können auch einen OnDemand-Streaminglocator zum Erstellen von URLs verwenden, die auf MP4-Dateien verweisen, die progressiv heruntergeladen werden können.  

In diesem Thema wird erläutert, wie Sie einen OnDemand-Streaminglocator erstellen, um Ihr Medienobjekt zu veröffentlichen und Smooth-, MPEG-DASH- sowie HLS-Streaming-URLs zu erstellen. Außerdem wird veranschaulicht, wie Sie URLs für progressive Downloads erstellen. 

## <a name="create-an-ondemand-streaming-locator"></a>Erstellen eines OnDemand-Streaminglocators
Verfahren Sie zum Erstellen des OnDemand-Streaminglocators und Abrufen von URLs wie folgt:

1. Wenn der Inhalt verschlüsselt ist, definieren Sie eine Zugriffsrichtlinie.
2. Erstellen Sie einen OnDemand-Streaminglocator.
3. Wenn Sie ein Streaming planen, rufen Sie die Streaming-Manifestdatei (ISM) im Medienobjekt ab. 
   
   Wenn Sie einen progressiven Download ausführen möchten, rufen Sie die Namen der MP4-Dateien im Medienobjekt ab.  
4. Erstellen Sie URLs für die Manifestdatei oder MP4-Dateien. 


>[!NOTE]
>Es gilt ein Grenzwert von 1.000.000 Richtlinien für verschiedene AMS-Richtlinien (z.B. für die Locator-Richtlinie oder für ContentKeyAuthorizationPolicy). Verwenden Sie stets die gleiche Richtlinien-ID, wenn Sie immer die gleichen Tage und Zugriffsberechtigungen verwenden. Verwenden Sie z.B. für Richtlinien für Locators, die für einen längeren Zeitraum vorgesehen sind (Richtlinien ohne Upload). Weitere Informationen finden Sie in [diesem](media-services-dotnet-manage-entities.md#limit-access-policies) Thema.

### <a name="use-media-services-net-sdk"></a>Verwenden des Media Services .NET SDKs
Erstellen von Streaming-URLs 

    private static void BuildStreamingURLs(IAsset asset)
    {

        // Create a 30-day readonly access policy. 
          // You cannot create a streaming locator using an AccessPolicy that includes write or delete permissions.
        IAccessPolicy policy = _context.AccessPolicies.Create("Streaming policy",
            TimeSpan.FromDays(30),
            AccessPermissions.Read);

        // Create a locator to the streaming content on an origin. 
        ILocator originLocator = _context.Locators.CreateLocator(LocatorType.OnDemandOrigin, asset,
            policy,
            DateTime.UtcNow.AddMinutes(-5));

        // Display some useful values based on the locator.
        Console.WriteLine("Streaming asset base path on origin: ");
        Console.WriteLine(originLocator.Path);
        Console.WriteLine();

        // Get a reference to the streaming manifest file from the  
        // collection of files in the asset. 
        var manifestFile = asset.AssetFiles.Where(f => f.Name.ToLower().
                                    EndsWith(".ism")).
                                    FirstOrDefault();

        // Create a full URL to the manifest file. Use this for playback
        // in streaming media clients. 
        string urlForClientStreaming = originLocator.Path + manifestFile.Name + "/manifest";
        Console.WriteLine("URL to manifest for client streaming using Smooth Streaming protocol: ");
        Console.WriteLine(urlForClientStreaming);
        Console.WriteLine("URL to manifest for client streaming using HLS protocol: ");
        Console.WriteLine(urlForClientStreaming + "(format=m3u8-aapl)");
        Console.WriteLine("URL to manifest for client streaming using MPEG DASH protocol: ");
        Console.WriteLine(urlForClientStreaming + "(format=mpd-time-csf)"); 
        Console.WriteLine();
    }

Die Ausgaben:

    URL to manifest for client streaming using Smooth Streaming protocol:
    http://amstest1.streaming.mediaservices.windows.net/3c5fe676-199c-4620-9b03-ba014900f214/BigBuckBunny.ism/manifest
    URL to manifest for client streaming using HLS protocol:
    http://amstest1.streaming.mediaservices.windows.net/3c5fe676-199c-4620-9b03-ba014900f214/BigBuckBunny.ism/manifest(format=m3u8-aapl)
    URL to manifest for client streaming using MPEG DASH protocol:
    http://amstest1.streaming.mediaservices.windows.net/3c5fe676-199c-4620-9b03-ba014900f214/BigBuckBunny.ism/manifest(format=mpd-time-csf)


> [!NOTE]
> Sie können auch den Inhalt über eine SSL-Verbindung streamen. Zu diesem Zweck stellen Sie sicher, dass die Streaming-URLs mit HTTPS beginnen. AMS unterstützt derzeit SSL mit benutzerdefinierten Domänen nicht.
> 
> 

Erstellen von URLs für progressive Downloads 

    private static void BuildProgressiveDownloadURLs(IAsset asset)
    {
        // Create a 30-day readonly access policy. 
        IAccessPolicy policy = _context.AccessPolicies.Create("Streaming policy",
            TimeSpan.FromDays(30),
            AccessPermissions.Read);

        // Create an OnDemandOrigin locator to the asset. 
        ILocator originLocator = _context.Locators.CreateLocator(LocatorType.OnDemandOrigin, asset,
            policy,
            DateTime.UtcNow.AddMinutes(-5));

        // Display some useful values based on the locator.
        Console.WriteLine("Streaming asset base path on origin: ");
        Console.WriteLine(originLocator.Path);
        Console.WriteLine();

        // Get MP4 files.
        IEnumerable<IAssetFile> mp4AssetFiles = asset
            .AssetFiles
            .ToList()
            .Where(af => af.Name.EndsWith(".mp4", StringComparison.OrdinalIgnoreCase));

        // Create a full URL to the MP4 files. Use this to progressively download files.
        foreach (var pd in mp4AssetFiles)
            Console.WriteLine(originLocator.Path + pd.Name);
    }

Die Ausgaben:

    http://amstest1.streaming.mediaservices.windows.net/3c5fe676-199c-4620-9b03-ba014900f214/BigBuckBunny_H264_650kbps_AAC_und_ch2_96kbps.mp4
    http://amstest1.streaming.mediaservices.windows.net/3c5fe676-199c-4620-9b03-ba014900f214/BigBuckBunny_H264_400kbps_AAC_und_ch2_96kbps.mp4
    http://amstest1.streaming.mediaservices.windows.net/3c5fe676-199c-4620-9b03-ba014900f214/BigBuckBunny_H264_3400kbps_AAC_und_ch2_96kbps.mp4
    http://amstest1.streaming.mediaservices.windows.net/3c5fe676-199c-4620-9b03-ba014900f214/BigBuckBunny_H264_2250kbps_AAC_und_ch2_96kbps.mp4

    . . . 

### <a name="use-media-services-net-sdk-extensions"></a>Verwenden von Media Services .NET SDK-Erweiterungen
Im folgenden Code werden .NET SDK-Erweiterungsmethoden aufgerufen, durch die ein Locator erstellt und die Smooth Streaming-, HLS- und MPEG-DASH-URLs für adaptives Streaming generiert werden.

    // Create a loctor.
    _context.Locators.Create(
        LocatorType.OnDemandOrigin,
        inputAsset,
        AccessPermissions.Read,
        TimeSpan.FromDays(30));

    // Get the streaming URLs.
    Uri smoothStreamingUri = inputAsset.GetSmoothStreamingUri();
    Uri hlsUri = inputAsset.GetHlsUri();
    Uri mpegDashUri = inputAsset.GetMpegDashUri();

    Console.WriteLine(smoothStreamingUri);
    Console.WriteLine(hlsUri);
    Console.WriteLine(mpegDashUri);


## <a name="media-services-learning-paths"></a>Media Services-Lernpfade
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Feedback geben
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="next-steps"></a>Nächste Schritte
* [Herunterladen von Medienobjekten](media-services-deliver-asset-download.md)
* [Konfigurieren der Übermittlungsrichtlinie für Medienobjekte](media-services-dotnet-configure-asset-delivery-policy.md)


