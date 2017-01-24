---
title: Wiedergabe von Inhalten | Microsoft Docs
description: "In diesem Thema finden Sie vorhandene Player, die Sie zum Wiedergeben Ihrer Inhalte verwenden können."
services: media-services
documentationcenter: 
author: Juliako
manager: erikre
editor: 
ms.assetid: 7e9fcf89-0fb6-4fa4-96cb-666320684d69
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/12/2016
ms.author: juliako
translationtype: Human Translation
ms.sourcegitcommit: 0c77ee0f612c1cbef551a129a22cf3f125e6f29d
ms.openlocfilehash: 41049570b1858010c338c149c5d3545e58a197b2


---
# <a name="playing-your-content-with-existing-players"></a>Wiedergabe Ihrer Inhalte mit vorhandenen Playern
Azure Media Services unterstützt zahlreiche gängige Streamingformate wie Smooth Streaming, HTTP Live Streaming und MPEG-Dash. In diesem Thema finden Sie Verweise auf vorhandene Player, die Sie zum Testen von Datenströmen verwenden können.

### <a name="the-azure-portal-media-services-content-player"></a>Der Media Services-Inhaltsplayer im Azure-Portal
Im **Azure** -Portal wird ein Inhaltsplayer bereitgestellt, mit dem Sie Ihre Videos testen können.

Klicken Sie auf den gewünschten Videoinhalt (vergewissern Sie sich, dass er [veröffentlicht](media-services-portal-publish.md)wurde), und klicken Sie auf die Schaltfläche **Abspielen** am unteren Rand des Portals.

Folgende Überlegungen sollten berücksichtigt werden:

* Der **MEDIA SERVICES-INHALTSPLAYER** gibt die Inhalt vom standardmäßigen Streamingendpunkt wieder. Wenn Sie die Wiedergabe von einem anderen Streamingendpunkt starten möchten, verwenden Sie einen anderen Player. Beispiel: [Azure Media Player](http://amsplayer.azurewebsites.net/azuremediaplayer.html).

![AMSPlayer][AMSPlayer]

### <a name="azure-media-player"></a>Azure Media Player
Verwenden Sie [Azure Media Player](http://amsplayer.azurewebsites.net/azuremediaplayer.html) zum Wiedergeben Ihrer Inhalte (offen oder geschützt) in den folgenden Formaten:

* Smooth Streaming
* MPEG DASH
* HLS
* Progressive MP4

### <a name="flash-player"></a>Flash Player
#### <a name="aes-encrypted-with-token"></a>AES-verschlüsselt mit Token
[http://aestoken.azurewebsites.net](http://aestoken.azurewebsites.net)

### <a name="silverlight-players"></a>Silverlight-Player
#### <a name="monitoring"></a>Überwachung
[http://smf.cloudapp.net/healthmonitor](http://smf.cloudapp.net/healthmonitor)

#### <a name="playready-with-token"></a>PlayReady mit Token
[http://sltoken.azurewebsites.net](http://sltoken.azurewebsites.net)

### <a name="dash-players"></a>DASH-Player
[http://dashplayer.azurewebsites.net](http://dashplayer.azurewebsites.net)

[http://dashif.org](http://dashif.org)

### <a name="other"></a>Andere
Sie können zum Testen von HLS-URLs auch Folgendes verwenden:

* **Safari** auf einem iOS-Gerät oder
* **3ivx HLS Player** unter Windows.

## <a name="developing-video-players"></a>Entwickeln von Videoplayern
Informationen zum Entwickeln eigener Player finden Sie unter [Entwickeln von Videoplayern](media-services-develop-video-players.md)

## <a name="media-services-learning-paths"></a>Media Services-Lernpfade
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Feedback geben
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

[AMSPlayer]: ./media/media-services-playback-content-with-existing-players/media-services-portal-player.png



<!--HONumber=Jan17_HO2-->


