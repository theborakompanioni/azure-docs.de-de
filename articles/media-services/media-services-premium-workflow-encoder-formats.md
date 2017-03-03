---
title: Media Encoder Premium Workflow-Formate und -Codecs | Microsoft Docs
description: "Dieses Thema bietet eine Übersicht über Media Encoder Premium Workflow-Formate und -Codecs"
services: media-services
documentationcenter: 
author: juliako
manager: erik43
editor: 
ms.assetid: b197fce8-3b9b-4189-8d08-486810c0426f
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/27/2017
ms.author: juliako;anilmur
translationtype: Human Translation
ms.sourcegitcommit: ef5db062ac320995ff8bfa64f38c89d860957868
ms.openlocfilehash: 8179e955ef0c126acb9dab31226bd1e12278f9b6
ms.lasthandoff: 01/28/2017


---
# <a name="media-encoder-premium-workflow-formats-and-codecs"></a>Media Encoder Premium Workflow-Formate und -Codecs
> [!NOTE]
> Fragen zu Encoder Premium senden Sie per E-Mail an „mepd at Microsoft.com“.
> 
> Der in diesem Thema beschriebene Media Encoder Premium Workflow-Medienprozessor ist in China nicht verfügbar. 
> 
> 

Dieses Dokument enthält eine Liste von Eingabe- und Ausgabedateiformaten sowie Codecs, die von der öffentlichen Vorschauversion des **Medienencoder-Premium-Workflow** -Encoders unterstützt werden.

[Media Encoder Premium Workflow-Eingabeformate und -Codecs](#input_formats)

[Media Encoder Premium Workflow-Ausgabeformate und -Codecs](#output_formats)

**Medienencoder-Premium-Workflow** unterstützt Untertitel, wie in [diesem](#closed_captioning) Abschnitt beschrieben. 

## <a name="a-idinputformatsamedia-encoder-premium-workflow-input-formats-and-codecs"></a><a id="input_formats"></a>Medienencoder-Premium-Workflow-Eingabeformate und -Codecs
Im folgenden Abschnitt werden die Codecs und Formate aufgeführt, die dieser Medienprozessor als Eingabe unterstützt.

### <a name="input-containerfile-formats"></a>Eingabecontainer/Dateiformate
* Adobe® Flash® F4V
* MXF/SMPTE 377M
* GXF
* MPEG-2-Transportdatenstrom
* MPEG-2-Programmdatenstrom
* MPEG-4/MP4
* Windows Media/ASF
* AVI (unkomprimiert, 8-Bit/10-Bit)

### <a name="input-video-codecs"></a>Codecs für Videoeingang
* AVC 8-Bit/10-Bit, bis zu 4:2:2, einschließlich AVCIntra
* Avid DNxHD (in MXF)
* DVCPro/DVCProHD (in MXF)
* JPEG2000
* MPEG-2 (bis zu 422 Profile und High Level; Varianten wie XDCAM, XDCAM HD, XDCAM IMX, CableLabs® und D10 eingeschlossen)
* MPEG-1
* Windows Media Video/VC-1

### <a name="input-audio-codecs"></a>Codecs für Audioeingang
* AES (SMPTE 331M und 302M, AES3-2003)
* Dolby® E
* Dolby® Digital (AC3)
* AAC (AAC-LC, AAC-HE und AAC-HEv2; bis 5.1)
* MPEG Layer 2
* MP3 (MPEG-1 Audio Layer 3)
* Windows Media Audio
* WAV/PCM

## <a name="a-idoutputformatamedia-encoder-premium-workflow-output-formats-and-codecs"></a><a id="output_format"></a>Medienencoder-Premium-Workflow-Ausgabeformate und -Codecs
Im folgenden Abschnitt werden die Codecs und Dateiformate aufgelistet, die als Ausgabe für diesen Medienprozessor unterstützt werden.

### <a name="output-containerfile-formats"></a>Ausgabecontainer/Dateiformate
* Adobe® Flash® F4V
* MXF (OP1a, XDCAM und AS02)
* DPP (einschließlich AS11)
* GXF
* MPEG-4/MP4
* Windows Media/ASF
* AVI (unkomprimiert, 8-Bit/10-Bit)
* Smooth Streaming-Dateiformat (PIFF 1.3)
* MPEG-TS 

### <a name="output-video-codecs"></a>Codecs für Videoausgang
* AVC (H.264; 8-Bit; bis High Profile, Level 5.2; 4K Ultra HD; AVC Intra)
* Avid DNxHD (in MXF)
* DVCPro/DVCProHD (in MXF)
* MPEG-2 (bis zu 422 Profile und High Level; Varianten wie XDCAM, XDCAM HD, XDCAM IMX, CableLabs® und D10 eingeschlossen)
* MPEG-1
* Windows Media Video/VC-1
* Erstellung von JPEG-Miniaturansichten

### <a name="output-audio-codecs"></a>Codecs für Audioausgabe
* AES (SMPTE 331M und 302M, AES3-2003)
* Dolby® Digital (AC3)
* Dolby® Digital Plus (E-AC3) bis 7.1
* AAC (AAC-LC, AAC-HE und AAC-HEv2; bis 5.1)
* MPEG Layer 2
* MP3 (MPEG-1 Audio Layer 3)
* Windows Media Audio

>[!NOTE]
>Wenn Sie in Dolby® Digital (AC3) codieren, kann die Ausgabe nur in eine ISO MP4-Datei geschrieben werden.

## <a name="a-idclosedcaptioningasupport-for-closed-captioning"></a><a id="closed_captioning"></a>Unterstützung für Untertitel
Bei der Erfassung unterstützt **Medienencoder-Premium-Workflow** Folgendes:

1. SCC-Dateien
2. SMPTE-TT-Dateien
3. CEA-608/CEA-708 – als Benutzerdaten (SEI-Nachrichten von H.264-Basisstreams, ATSC/53, SCTE20) oder als Übertragung als Zusatzdaten in MXF-/GXF-Dateien
4. SRT-Untertiteldateien

Bei der Ausgabe sind die folgenden Optionen verfügbar:

1. Übersetzung von CEA-608 nach CEA-708
2. CEA-608/CEA-708-Passthrough (eingebettet in SEI-Nachrichten von H.264-Basisstreams oder Übertragung als Zusatzdaten in MXF-Dateien)
3. SCC
4. SMPTE Timed Text (aus Quelle CEA-608 pro SMPTE RP2052; DFXP-Dateierstellung eingeschlossen)
5. SRT-Untertiteldatei
6. DVB-Untertitelstreams

Hinweis: Nicht alle der oben genannten Ausgabeformate werden zur Übermittlung per Streaming in Azure Media Services unterstützt.

## <a name="known-issues"></a>Bekannte Probleme
Wenn Ihr Eingabevideo keine Untertitel enthält, enthält das Ausgabemedienobjekt trotzdem eine leere TTML-Datei. 

## <a name="media-services-learning-paths"></a>Media Services-Lernpfade
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Feedback geben
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]


