---
title: "Übersicht über die dynamische Paketerstellung mit Azure Media Services | Microsoft-Dokumentation"
description: "In diesem Thema finden Sie eine Übersicht über die dynamische Paketerstellung."
author: Juliako
manager: erikre
editor: 
services: media-services
documentationcenter: 
ms.assetid: 0d9e4f54-5daa-45c1-bfaa-cf09ca89b812
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/29/2017
ms.author: juliako
ms.translationtype: Human Translation
ms.sourcegitcommit: da455a350f61e17425cd308d0fdc8bb5294a0b76
ms.openlocfilehash: 574921fdecdadaa48c572685f7486d4e7b1d25f4
ms.contentlocale: de-de
ms.lasthandoff: 01/27/2017


---
# <a name="dynamic-packaging"></a>Dynamische Paketerstellung
## <a name="overview"></a>Übersicht
Microsoft Azure Media Services kann verwendet werden, um zahlreiche Medien-Quelldateiformate, Medienstreamingformate und Inhaltsschutzformate in verschiedenen Clienttechnologien wie iOS, XBOX, Silverlight oder Windows 8 bereitzustellen. Von den Clients werden verschiedene Protokolle verarbeitet. So ist für iOS ein HLS-V4-Format (HTTP Live Streaming) erforderlich, für Silverlight und Xbox dagegen Smooth Streaming. Wenn Sie über einen Satz MP4-Dateien (ISO Base Media 14496-12) mit adaptiver Bitrate (Multi-Bitrate) oder einen Satz von Smooth Streaming-Dateien mit adaptiver Bitrate verfügen, die Sie Clients bereitstellen möchten, von denen MPEG DASH, HLS oder Smooth Streaming verarbeitet wird, sollten Sie die Vorteile der dynamischen Paketerstellung von Media Services nutzen.

Bei der dynamischen Paketerstellung müssen Sie lediglich ein Medienobjekt erstellen, das einen Satz von MP4- oder Smooth Streaming-Dateien mit adaptiver Bitrate enthält. Dann wird durch den bedarfsgesteuerten Streamingserver auf Basis des in der Manifest- oder Fragmentanforderung angegebenen Formats sichergestellt, dass Sie den Datenstrom im ausgewählten Protokoll erhalten. So müssen Sie die Dateien nur in einem Speicherformat speichern und bezahlen. Die entsprechende Antwort wird von Media Services basierend auf Clientanforderungen erstellt und verfügbar gemacht.

Im folgenden Diagramm wird der herkömmliche Workflow zur Codierung und statischen Paketerstellung dargestellt.

![Statische Codierung](./media/media-services-dynamic-packaging-overview/media-services-static-packaging.png)

Im folgenden Diagramm wird der Workflow zur dynamischen Paketerstellung dargestellt.

![Dynamische Codierung](./media/media-services-dynamic-packaging-overview/media-services-dynamic-packaging.png)


## <a name="common-scenario"></a>Allgemeines Szenario
1. Laden Sie eine Eingabedatei (auch Mezzaninedatei genannt) hoch. Beispielformate: H.264, MP4 oder WMV (eine Liste unterstützter Formate finden Sie unter [Von Media Encoder Standard unterstützte Formate](media-services-media-encoder-standard-formats.md)).
2. Codieren Sie Ihre Mezzaninedatei zu H.264-MP4-Sätzen mit adaptiver Bitrate.
3. Veröffentlichen Sie das Medienobjekt, das den MP4-Satz mit adaptiver Bitrate enthält, indem Sie den On-Demand-Locator erstellen.
4. Erstellen Sie die Streaming-URLs zum Zugreifen und Streamen Ihrer Inhalte.

## <a name="preparing-assets-for-dynamic-streaming"></a>Vorbereiten von Medienobjekten auf dynamisches Streaming
Zum Vorbereiten Ihres Medienobjekts auf dynamisches Streaming haben Sie zwei Möglichkeiten:

1. [Laden Sie eine Masterdatei hoch](media-services-dotnet-upload-files.md).
2. [Verwenden Sie den Media Encoder Standard-Encoder zum Erzeugen von H.264-MP4-Sätzen mit adaptiver Bitrate](media-services-dotnet-encode-with-media-encoder-standard.md).
3. [Streamen Sie Ihre Inhalte](media-services-deliver-content-overview.md).

## <a id="unsupported_formats"></a>Formate, die nicht von der dynamischen Paketerstellung unterstützt werden
Die folgenden Quelldateiformate werden von der dynamischen Paketerstellung nicht unterstützt:

* MP4-Dateien in Dolby Digital
* Smooth-Dateien in Dolby Digital

## <a name="media-services-learning-paths"></a>Media Services-Lernpfade
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Feedback geben
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]


