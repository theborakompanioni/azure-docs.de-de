---
title: "Azure On-Demand Media Encoder – Überblick und Vergleich | Microsoft-Dokumentation"
description: "Dieses Thema bietet eine Übersicht und einen Vergleich über Azure On-Demand-Media Encoder."
services: media-services
documentationcenter: 
author: juliako
manager: erikre
editor: 
ms.assetid: e6bfc068-fa46-4d68-b1ce-9092c8f3a3c9
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/19/2016
ms.author: juliako
translationtype: Human Translation
ms.sourcegitcommit: ff663f40507547ba561053b5c9a7a8ce93fbf213
ms.openlocfilehash: ea3d67f7e73f9e1fb716e9c5cdd4873b0b7dddd4


---
# <a name="overview-and-comparison-of-azure-on-demand-media-encoders"></a>Azure On-Demand Media Encoder – Überblick und Vergleich
## <a name="encoding-overview"></a>Übersicht über die Codierung
Azure Media Services bietet verschiedene Optionen für die Codierung von Medien in der Cloud.

Bei Verwendung von Media Services ist es wichtig, den Unterschied zwischen Codecs und Dateiformaten zu kennen.
Bei Codecs handelt es sich um die Software, die die Algorithmen für die Komprimierung/Dekomprimierung implementiert. Dateiformate dagegen sind die Container, die das komprimierte Video enthalten.

Media Services bietet dynamische Paketerstellung zum Übermitteln Ihrer MP4-Dateien mit adaptiver Bitrate oder Smooth Streaming-codierten Inhalte in Streamingformaten, die von Media Services unterstützt werden (MPEG-DASH, HLS, Smooth Streaming), ohne dass Sie diese Streamingformate erneut packen müssen.

Um die [dynamische Paketerstellung](media-services-dynamic-packaging-overview.md)nutzen zu können, müssen Sie folgende Schritte ausführen:

* Codieren Ihrer Zwischendatei (Quelle) in einen Satz von MP4-Dateien oder Smooth Streaming-Dateien mit adaptiver Bitrate (die Codierungsschritte werden weiter unten in diesem Lernprogramm beschrieben)
* Abrufen von mindestens einer On-Demand-Streamingeinheit für den Streamingendpunkt, von dem aus Sie die Bereitstellung Ihrer Inhalte planen. Weitere Informationen finden Sie unter [Skalieren von reservierten Einheiten für bedarfsgesteuertes Streaming](media-services-portal-manage-streaming-endpoints.md).

Media Services unterstützt die folgenden On-Demand-Encoder, die in diesem Artikel beschrieben werden:

* [Media Encoder Standard](media-services-encode-asset.md#media-encoder-standard)
* [Media Encoder Premium Workflow](media-services-encode-asset.md#media-encoder-premium-workflow)

Dieser Artikel enthält eine kurze Übersicht über On-Demand-Media Encoder und stellt Links zu Artikeln bereit, die detailliertere Informationen bieten. Das Thema enthält auch einen Encodervergleich.

Beachten Sie, dass jedes Media Services-Konto standardmäßig je eine aktive Codierungsaufgabe gleichzeitig aufweisen kann. Sie können Einheiten für die Codierung reservieren, mit denen Sie mehrere Codierungsaufgaben gleichzeitig ausführen kennen – jeweils eine für jede reservierte Einheit für die Codierung, die Sie erwerben. Weitere Informationen finden Sie unter [Skalieren der Codierung](media-services-scale-media-processing-overview.md).

## <a name="media-encoder-standard"></a>Media Encoder Standard
### <a name="how-to-use"></a>Gewusst wie
[Gewusst wie: Codieren mit Media Encoder Standard](media-services-dotnet-encode-with-media-encoder-standard.md)

### <a name="formats"></a>Formate
[Formate und Codecs](media-services-media-encoder-standard-formats.md)

### <a name="presets"></a>Voreinstellungen
Media Encoder Standard wird mithilfe von Encoder-Voreinstellungen konfiguriert, die [hier](http://go.microsoft.com/fwlink/?linkid=618336&clcid=0x409)beschrieben sind.

### <a name="input-and-output-metadata"></a>Eingabe- und Ausgabemetadaten
Die Eingabemetadaten für den Encoder werden [hier](http://msdn.microsoft.com/library/azure/dn783120.aspx)beschrieben.

Die Ausgabemetadaten für den Encoder werden [hier](http://msdn.microsoft.com/library/azure/dn783217.aspx)beschrieben.

### <a name="generate-thumbnails"></a>Generieren von Miniaturansichten
Informationen finden Sie unter [Generieren von Miniaturansichten mithilfe von Media Encoder Standard](media-services-advanced-encoding-with-mes.md#thumbnails).

### <a name="trim-videos-clipping"></a>Kürzen von Videos (Clipping)
Informationen finden Sie unter [Kürzen von Videos mithilfe von Media Encoder Standard](media-services-advanced-encoding-with-mes.md#trim_video).

### <a name="create-overlays"></a>Erstellen von Überlagerungen
Informationen finden Sie unter [Erstellen von Überlagerungen mithilfe von Media Encoder Standard](media-services-advanced-encoding-with-mes.md#overlay).

### <a name="see-also"></a>Siehe auch
[Media Services-Blog](https://azure.microsoft.com/blog/2015/07/16/announcing-the-general-availability-of-media-encoder-standard/)

## <a name="media-encoder-premium-workflow"></a>Media Encoder Premium Workflow
### <a name="overview"></a>Übersicht
[Introducing Premium Encoding in Azure Media Services (in englischer Sprache)](https://azure.microsoft.com/blog/2015/03/05/introducing-premium-encoding-in-azure-media-services/)

### <a name="how-to-use"></a>Gewusst wie
Media Encoder Premium Workflow ist mit komplexen Workflows konfiguriert. Workflowdateien können mit dem Tool [Workflow-Designer](media-services-workflow-designer.md) erstellt und aktualisiert werden.

[How to Use Premium Encoding in Azure Media Services (in englischer Sprache)](https://azure.microsoft.com/blog/2015/03/06/how-to-use-premium-encoding-in-azure-media-services/)

### <a name="known-issues"></a>Bekannte Probleme
Wenn Ihr Eingabevideo keine Untertitel enthält, enthält das Ausgabemedienobjekt trotzdem eine leere TTML-Datei.


## <a name="media-services-learning-paths"></a>Media Services-Lernpfade
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Feedback geben
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="related-articles"></a>Verwandte Artikel
* [Ausführen von Aufgaben für die erweiterte Codierung durch Anpassen der Media Encoder Standard-Voreinstellungen](media-services-custom-mes-presets-with-dotnet.md)
* [Kontingente und Einschränkungen](media-services-quotas-and-limitations.md)

<!--Reference links in article-->
[1]: http://azure.microsoft.com/pricing/details/media-services/



<!--HONumber=Dec16_HO2-->


