---
title: " Skalieren von Streamingendpunkten mithilfe des Azure-Portals | Microsoft Docs"
description: "Dieses Tutorial führt Sie durch die Schritte zur Skalierung von Streamingendpunkten mithilfe des Azure-Portals."
services: media-services
documentationcenter: 
author: Juliako
manager: erikre
editor: 
ms.assetid: 1008b3a3-2fa1-4146-85bd-2cf43cd1e00e
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/24/2016
ms.author: juliako
translationtype: Human Translation
ms.sourcegitcommit: ff663f40507547ba561053b5c9a7a8ce93fbf213
ms.openlocfilehash: f9f7872eab6b61afcbdc2d8eb23f1dc8ada82829


---
# <a name="scale-streaming-endpoints-with-the-azure-portal"></a>Skalieren von Streamingendpunkten mithilfe des Azure-Portals
## <a name="overview"></a>Übersicht
> [!NOTE]
> Sie benötigen ein Azure-Konto, um dieses Lernprogramm auszuführen. Weitere Informationen finden Sie unter [Kostenloses Azure-Testkonto](https://azure.microsoft.com/pricing/free-trial/). 
> 
> 

Wenn Sie mit Azure Media Services arbeiten, besteht eines der häufigsten Szenarien darin, Videos per Adaptive Bitrate Streaming an Ihre Clients zu übermitteln. Media Services unterstützt die folgenden Technologien mit Adaptive Bitrate Streaming: HTTP Live Streaming (HLS), Smooth Streaming und MPEG DASH.

Media Services bietet dynamische Paketerstellung für die Just-in-Time-Übermittlung von Daten vom Typ „MP4-codierte Inhalte mit adaptiver Bitrate“ in Streamingformaten, die von Media Services unterstützt werden (MPEG-DASH, HLS, Smooth Streaming), ohne dass Sie jeweils vorab gepackte Versionen dieser Streamingformate speichern müssen.

Um die dynamische Paketerstellung nutzen zu können, müssen Sie folgende Schritte ausführen:

* Codieren Ihrer Zwischendatei (Quelle) in einen Satz von MP4-Dateien mit adaptiver Bitrate (die Codierungsschritte werden weiter unten in diesem Tutorial beschrieben)  
* Erstellen von mindestens einer Streamingeinheit für den *Streamingendpunkt*, von dem aus Sie die Bereitstellung Ihrer Inhalte planen In den folgenden Schritten wird gezeigt, wie Sie die Anzahl von Streamingeinheiten ändern.

Mit der dynamischen Paketerstellung müssen Sie die Dateien nur in einem Speicherformat speichern und bezahlen. Media Services erstellt und verarbeitet die entsprechende Antwort basierend auf Anforderungen von einem Client.

Darüber hinaus können Sie die Kapazität des Streamingendpunktdiensts für die Verarbeitung wachsender Bandbreitenanforderungen steuern, indem Sie die Streamingeinheiten anpassen. Es wird empfohlen, für Anwendungen in der Produktionsumgebung eine oder mehrere Skalierungseinheiten zu reservieren. Streamingeinheiten stellen dedizierte Ausgangskapazitäten bereit, die in Schritten zu jeweils 200 Mbit/s erworben werden können, sowie zusätzliche Funktionen, die Folgendes umfassen: [dynamische Paketerstellung](media-services-dynamic-packaging-overview.md), CDN-Integration und erweiterte Konfiguration. Weitere Informationen finden Sie unter [Verwalten von Streamingendpunkten mithilfe des Azure-Portals](media-services-portal-manage-streaming-endpoints.md).

## <a name="scale-streaming-endpoints"></a>Skalieren von Streamingendpunkten
Gehen Sie wie folgt vor, um die Anzahl von Einheiten zu erstellen und zu ändern, die für das Streaming reserviert sind:

1. Wählen Sie im [Azure-Portal](https://portal.azure.com/) Ihr Azure Media Services-Konto aus.
2. Klicken Sie im Fenster **Einstellungen** auf **Streamingendpunkte**.
3. Klicken Sie auf den Streamingendpunkt, den Sie skalieren möchten. 
4. Verschieben Sie den Schieberegler, um die Anzahl der Streamingeinheiten anzugeben.

![Streamingendpunkt](./media/media-services-portal-manage-streaming-endpoints/media-services-manage-streaming-endpoints3.png)

Es gelten die folgenden Bedingungen:

* Die Zuordnung neuer Streamingeinheiten dauert ca. 20 Minuten. 
* Aktuell kann das bedarfsgesteuerte Streaming für bis zu eine Stunde deaktiviert werden, wenn Sie einen positiven Wert für die Streaming-Einheiten zurück auf null setzen.
* Die höchste für den 24-Stunden-Zeitraum angegebene Anzahl an Einheiten wird zum Berechnen der Kosten verwendet. Informationen zu den Preisen finden Sie unter [Mediendienste – Preisübersicht](http://go.microsoft.com/fwlink/?LinkId=275107).

## <a name="next-steps"></a>Nächste Schritte
Überprüfen Sie die Media Services-Lernpfade.

[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Feedback geben
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]




<!--HONumber=Dec16_HO2-->


