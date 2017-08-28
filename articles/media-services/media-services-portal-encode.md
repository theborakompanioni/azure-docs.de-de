---
title: Codieren eines Medienobjekts mit Media Encoder Standard mithilfe des Azure-Portals | Microsoft Docs
description: "Dieses Tutorial führt Sie durch die Schritte zur Codierung eines Medienobjekts mit Media Encoder Standard mithilfe des Azure-Portals."
services: media-services
documentationcenter: 
author: Juliako
manager: SyntaxC4
editor: 
ms.assetid: 107d9e9a-71e9-43e5-b17c-6e00983aceab
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/07/2017
ms.author: juliako
ms.translationtype: HT
ms.sourcegitcommit: f5c887487ab74934cb65f9f3fa512baeb5dcaf2f
ms.openlocfilehash: efe7db8a36273b4755dd057139bb1c673af868d3
ms.contentlocale: de-de
ms.lasthandoff: 08/08/2017

---
# <a name="encode-an-asset-using-media-encoder-standard-with-the-azure-portal"></a>Codieren eines Medienobjekts mit Media Encoder Standard mithilfe des Azure-Portals
> [!NOTE]
> Sie benötigen ein Azure-Konto, um dieses Lernprogramm auszuführen. Weitere Informationen finden Sie unter [Kostenloses Azure-Testkonto](https://azure.microsoft.com/pricing/free-trial/). 
> 
> 

Bei der Arbeit mit Azure Media Services ist eines der häufigsten Szenarios das Streaming mit adaptiver Bitrate an Clients. Media Services unterstützt die folgenden Technologien mit Adaptive Bitrate Streaming: HTTP Live Streaming (HLS), Smooth Streaming und MPEG DASH. Um die Videos für das Adaptive Bitrate Streaming vorzubereiten, müssen Sie das Quellvideo in Dateien mit mehreren Bitraten codieren. Videos sollten mit dem Encoder **Media Encoder Standard** codiert werden.  

Media Services bietet auch eine dynamische Paketerstellung, bei der Sie MP4-Dateien mit Mehrfachbitraten in den folgenden Streamingformaten bereitstellen können: MPEG-DASH, HLS und Smooth Streaming. Hierbei ist es nicht erforderlich, diese Streamingformate neu zu packen. Mit der dynamischen Paketerstellung müssen Sie die Dateien nur in einem Speicherformat speichern und bezahlen. Media Services erstellt und verarbeitet die entsprechende Antwort basierend auf Anforderungen von einem Client.

Um die dynamische Paketerstellung nutzen zu können, müssen Sie Ihre Quelldatei in einen Satz von MP4-Dateien mit Mehrfachbitrate codieren (die Codierungsschritte werden weiter unten in diesem Abschnitt beschrieben).

Informationen zum Skalieren der Medienverarbeitung finden Sie in [diesem](media-services-portal-scale-media-processing.md) Thema.

## <a name="encode-with-the-azure-portal"></a>Codieren mithilfe des Azure-Portals
In diesem Abschnitt werden die Schritte beschrieben, die Sie ausführen können, um Ihre Inhalte mit Media Encoder Standard zu codieren.

1. Wählen Sie im [Azure-Portal](https://portal.azure.com/) Ihr Azure Media Services-Konto aus.
2. Wählen Sie im Fenster **Einstellungen** die Option **Assets**.  
3. Wählen Sie im Fenster **Ressourcen** das Objekt aus, das Sie codieren möchten.
4. Klicken Sie auf die Schaltfläche **Codieren** .
5. Wählen Sie im Fenster **Medienobjekt codieren** den Prozessor „Media Encoder Standard“ und eine Voreinstellung aus. Informationen zu Voreinstellungen finden Sie unter [Verwenden von Azure Media Encoder Standard zum automatischen Generieren einer Reihe von Bitraten](media-services-autogen-bitrate-ladder-with-mes.md) und [Aufgabenvoreinstellungen für MES (Media Encoder Standard)](media-services-mes-presets-overview.md). Wenn Sie steuern möchten, welche Codierungsvoreinstellung verwendet werden soll, beachten Sie Folgendes: Es ist wichtig, die Voreinstellung auszuwählen, die sich am besten für Ihr Eingabevideo eignet. Wenn Sie beispielsweise wissen, dass das Eingabevideo eine Auflösung von 1920x1080 Pixel hat, können Sie die Voreinstellung „H264 Multiple Bitrate 1080p“ wählen. Wenn Sie über ein Video mit niedriger Auflösung (640x360) verfügen, sollten Sie nicht die Voreinstellung „H264 Multiple Bitrate 1080p“ verwenden.
   
   Zur Vereinfachung der Verwaltung besteht die Möglichkeit, den Namen des Ausgabemedienobjekts und den Namen des Auftrags zu bearbeiten.
   
   ![Codieren von Medienobjekten](./media/media-services-portal-vod-get-started/media-services-encode1.png)
6. Klicken Sie auf **Erstellen**.

## <a name="next-step"></a>Nächster Schritt
Sie können den Codierauftragsstatus mithilfe des Azure-Portals überwachen (siehe [diesen](media-services-portal-check-job-progress.md) Artikel).  

## <a name="media-services-learning-paths"></a>Media Services-Lernpfade
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Feedback geben
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]


