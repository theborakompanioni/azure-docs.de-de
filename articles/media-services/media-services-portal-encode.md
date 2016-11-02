<properties
	pageTitle="Codieren eines Medienobjekts mit Media Encoder Standard mithilfe des Azure-Portals | Microsoft Azure"
	description="Dieses Tutorial führt Sie durch die Schritte zur Codierung eines Medienobjekts mit Media Encoder Standard mithilfe des Azure-Portals."
	services="media-services"
	documentationCenter=""
	authors="Juliako"
	manager="erikre"
	editor=""/>

<tags
	ms.service="media-services"
	ms.workload="media"
	ms.tgt_pltfrm="na"
	ms.devlang="na"
	ms.topic="article"
	ms.date="08/29/2016"
	ms.author="juliako"/>


# Codieren eines Medienobjekts mit Media Encoder Standard mithilfe des Azure-Portals

> [AZURE.NOTE] Sie benötigen ein Azure-Konto, um dieses Lernprogramm auszuführen. Weitere Informationen finden Sie unter [Kostenloses Azure-Testkonto](https://azure.microsoft.com/pricing/free-trial/).

Bei der Arbeit mit Azure Media Services ist eines der häufigsten Szenarios das Streaming mit adaptiver Bitrate an Clients. Von Media Services werden die folgenden Streamingtechnologien mit adaptiver Bitrate unterstützt: HTTP Live Streaming (HLS), Smooth Streaming, MPEG DASH und HDS (nur mit Adobe PrimeTime/Access-Lizenz). Um die Videos für das Adaptive Bitrate Streaming vorzubereiten, müssen Sie das Quellvideo in Dateien mit mehreren Bitraten codieren. Videos sollten mit dem Encoder **Media Encoder Standard** codiert werden.

Media Services umfasst auch die dynamische Paketerstellung, bei der Sie MP4-Dateien mit variablen Bitraten in den folgenden Streamingformaten bereitstellen können: MPEG DASH, HLS, Smooth Streaming oder HDS. Hierbei ist es nicht erforderlich, diese Streamingformate erneut zu verpacken. Mit der dynamischen Paketerstellung müssen Sie die Dateien nur in einem Speicherformat speichern und bezahlen. Media Services erstellt und verarbeitet die entsprechende Antwort basierend auf Anforderungen von einem Client.

Um die dynamische Paketerstellung nutzen zu können, müssen Sie folgende Schritte ausführen:

- Codieren Ihrer Quelldatei in einen Satz von MP4-Dateien mit variablen Bitraten (die Codierungsschritte werden weiter unten in diesem Abschnitt beschrieben)
- Abrufen von mindestens einer Streamingeinheit für den Streamingendpunkt, von dem aus Sie die Bereitstellung Ihrer Inhalte planen Weitere Informationen finden Sie unter [Konfigurieren von Streamingendpunkten](media-services-portal-vod-get-started.md#configure-streaming-endpoints).

Informationen zum Skalieren der Medienverarbeitung finden Sie in [diesem](media-services-portal-scale-media-processing.md) Thema.

## Codieren mithilfe des Azure-Portals

In diesem Abschnitt werden die Schritte beschrieben, die Sie ausführen können, um Ihre Inhalte mit Media Encoder Standard zu codieren.

1.  Wählen Sie im Fenster **Einstellungen** die Option **Ressourcen** aus.
2.  Wählen Sie im Fenster **Ressourcen** das Objekt aus, das Sie codieren möchten.
3.  Klicken Sie auf die Schaltfläche **Codieren**.
4.  Wählen Sie im Fenster **Encode an asset** (Ressource codieren) den Prozessor „Media Encoder Standard“ und eine Voreinstellung aus. Wenn Sie beispielsweise wissen, dass das Eingabevideo eine Auflösung von 1920x1080 Pixel hat, können Sie die Voreinstellung „H264 Multiple Bitrate 1080p“ wählen. Weitere Informationen zu Voreinstellungen finden Sie in [diesem Artikel](https://msdn.microsoft.com/library/azure/mt269960.aspx). Es ist wichtig, dass Sie die Voreinstellung auswählen, die für Ihr Eingabevideo am besten geeignet ist. Wenn Sie über ein Video mit niedriger Auflösung (640x360) verfügen, sollten Sie nicht die standardmäßige Voreinstellung „H264 Multiple Bitrate 1080p“ verwenden.
	
	Zur Vereinfachung der Verwaltung besteht die Möglichkeit, den Namen des Ausgabemedienobjekts und den Namen des Auftrags zu bearbeiten.
		
	![Codieren von Medienobjekten](./media/media-services-portal-vod-get-started/media-services-encode1.png)
5. Klicken Sie auf **Erstellen**.


##Nächster Schritt

Sie können den Codierauftragsstatus mithilfe des Azure-Portals überwachen (siehe [diesen](media-services-portal-check-job-progress.md) Artikel).

##Media Services-Lernpfade

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##Feedback geben

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

<!---HONumber=AcomDC_0831_2016-->