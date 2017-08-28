---
title: "Erste Schritte zum Bereitstellen von VoD-Inhalten über das Azure-Portal | Microsoft-Dokumentation"
description: "In diesem Tutorial erfahren Sie Schritt für Schritt, wie Sie mithilfe des Azure-Portals einen einfachen Dienst zur Übermittlung von VoD-Inhalten (Video-on-Demand) mit der AMS-Anwendung (Azure Media Services) implementieren."
services: media-services
documentationcenter: 
author: Juliako
manager: cfowler
editor: 
ms.assetid: 6c98fcfa-39e6-43a5-83a5-d4954788f8a4
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 08/07/2017
ms.author: juliako
ms.translationtype: HT
ms.sourcegitcommit: f5c887487ab74934cb65f9f3fa512baeb5dcaf2f
ms.openlocfilehash: cbb67ef92386a6288b3317bf77ebb67f15ce7fb2
ms.contentlocale: de-de
ms.lasthandoff: 08/08/2017

---
# <a name="get-started-with-delivering-content-on-demand-using-the-azure-portal"></a>Erste Schritte zum Bereitstellen von Inhalten nach Bedarf mit dem Azure-Portal
[!INCLUDE [media-services-selector-get-started](../../includes/media-services-selector-get-started.md)]

In diesem Tutorial erfahren Sie Schritt für Schritt, wie Sie mithilfe des Azure-Portals einen einfachen Dienst zur Übermittlung von VoD-Inhalten (Video-on-Demand) mit der AMS-Anwendung (Azure Media Services) implementieren.

## <a name="prerequisites"></a>Voraussetzungen
Zum Abschließen dieses Lernprogramms müssen folgende Voraussetzungen erfüllt sein:

* Ein Azure-Konto. Ausführliche Informationen finden Sie unter [Einen Monat kostenlos testen](https://azure.microsoft.com/pricing/free-trial/). 
* Media Services-Konto. Informationen zum Erstellen eines Media Services-Kontos finden Sie unter [Gewusst wie: Erstellen eines Media Services Kontos](media-services-portal-create-account.md).

Dieses Lernprogramm enthält die folgenden Aufgaben:

1. Starten des Streamingendpunkts
2. Hochladen einer Videodatei
3. Codieren der Quelldatei in einen Satz von MP4-Dateien mit adaptiver Bitrate
4. Veröffentlichen des Medienobjekts und Abrufen von URLs für Streaming und progressiven Download  
5. Wiedergeben Ihrer Inhalte

## <a name="start-streaming-endpoints"></a>Starten von Streamingendpunkten 

Wenn Sie mit Azure Media Services arbeiten, besteht eines der häufigsten Szenarien darin, Videos per Adaptive Bitrate Streaming zu übermitteln. Media Services bietet dynamische Paketerstellung für die Just-in-Time-Übermittlung von Daten vom Typ „MP4-codierte Inhalte mit adaptiver Bitrate“ in Streamingformaten, die von Media Services unterstützt werden (MPEG DASH, HLS, Smooth Streaming), ohne dass Sie jeweils vorab verpackte Versionen dieser Streamingformate speichern müssen.

>[!NOTE]
>Beim Erstellen Ihres AMS-Kontos wird dem Konto ein **Standard**-Streamingendpunkt mit dem Status **Beendet** hinzugefügt. Um mit dem Streamen der Inhalte zu beginnen und die dynamische Paketerstellung und dynamische Verschlüsselung zu nutzen, muss der Streamingendpunkt, von dem Sie Inhalte streamen möchten, den Status **Wird ausgeführt** aufweisen. 

Führen Sie folgende Schritte aus, um den Streamingendpunkt zu starten:

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com/)an.
2. Klicken Sie im Fenster „Einstellungen“ auf „Streamingendpunkte“. 
3. Klicken Sie auf den standardmäßigen Streamingendpunkt. 

    Das Fenster DEFAULT STREAMING ENDPOINT DETAILS (DETAILS ZUM STANDARD-STREAMINGENDPUNKT) wird angezeigt.

4. Klicken Sie auf das Symbol „Start“.
5. Klicken Sie auf die Schaltfläche „Speichern“, um die Änderungen zu speichern.

## <a name="upload-files"></a>Hochladen von Dateien
Zum Streamen von Videos mit Azure Media Services müssen Sie die Quellvideos hochladen, in mehreren Bitraten codieren und das Ergebnis veröffentlichen. Der erste Schritt ist in diesem Abschnitt beschrieben. 

1. Klicken Sie im Fenster **Einstellungen** auf **Assets**.
   
    ![Hochladen von Dateien](./media/media-services-portal-vod-get-started/media-services-upload.png)
2. Klicken Sie auf die Schaltfläche **Upload** .
   
    Das Fenster **Upload a video asset** (Videoobjekt hochladen) wird angezeigt.
   
   > [!NOTE]
   > Es gibt in Bezug auf die Dateigröße keinerlei Beschränkung.
   > 
   > 
3. Suchen Sie auf Ihrem Computer nach dem gewünschten Video, wählen Sie es aus, und klicken Sie auf „OK“.  
   
    Der Upload beginnt, und der Status wird unter dem Dateinamen angezeigt.  

Nach Abschluss des Uploadvorgangs wird das neue Objekt im Fenster **Ressourcen** angezeigt. 

## <a name="encode-assets"></a>Codieren von Medienobjekten

Bei der Arbeit mit Azure Media Services ist eines der häufigsten Szenarios das Streaming mit adaptiver Bitrate an Clients. Media Services unterstützt die folgenden Technologien mit Adaptive Bitrate Streaming: HTTP Live Streaming (HLS), Smooth Streaming und MPEG DASH. Um die Videos für das Adaptive Bitrate Streaming vorzubereiten, müssen Sie das Quellvideo in Dateien mit mehreren Bitraten codieren. Videos sollten mit dem Encoder **Media Encoder Standard** codiert werden.  

Media Services bietet auch eine dynamische Paketerstellung, bei der Sie MP4-Dateien mit variablen Bitraten in den folgenden Streamingformaten bereitstellen können: MPEG DASH, HLS und Smooth Streaming. Hierbei ist es nicht erforderlich, diese Streamingformate neu zu verpacken. Mit der dynamischen Paketerstellung müssen Sie die Dateien nur in einem einzelnen Speicherformat speichern und bezahlen. Media Services erstellt und verarbeitet die entsprechende Antwort basierend auf Anforderungen von einem Client.

Um die dynamische Paketerstellung nutzen zu können, müssen Sie Ihre Quelldatei in einen Satz von MP4-Dateien mit Mehrfachbitrate codieren (die Codierungsschritte werden weiter unten in diesem Abschnitt beschrieben).

### <a name="to-use-the-portal-to-encode"></a>So verwenden Sie das Portal zum Codieren
In diesem Abschnitt werden die Schritte beschrieben, die Sie ausführen können, um Ihre Inhalte mit Media Encoder Standard zu codieren.

1. Wählen Sie im Fenster **Einstellungen** die Option **Assets** aus.  
2. Wählen Sie im Fenster **Ressourcen** das Objekt aus, das Sie codieren möchten.
3. Klicken Sie auf die Schaltfläche **Codieren** .
4. Wählen Sie im Fenster **Medienobjekt codieren** den Prozessor „Media Encoder Standard“ und eine Voreinstellung aus. Informationen zu Voreinstellungen finden Sie unter [Verwenden von Azure Media Encoder Standard zum automatischen Generieren einer Reihe von Bitraten](media-services-autogen-bitrate-ladder-with-mes.md) und [Aufgabenvoreinstellungen für MES (Media Encoder Standard)](media-services-mes-presets-overview.md). Wenn Sie steuern möchten, welche Codierungsvoreinstellung verwendet werden soll, beachten Sie Folgendes: Es ist wichtig, die Voreinstellung auszuwählen, die sich am besten für Ihr Eingabevideo eignet. Wenn Sie beispielsweise wissen, dass das Eingabevideo eine Auflösung von 1920x1080 Pixel hat, können Sie die Voreinstellung „H264 Multiple Bitrate 1080p“ wählen. Wenn Sie über ein Video mit niedriger Auflösung (640x360) verfügen, sollten Sie nicht die Voreinstellung „H264 Multiple Bitrate 1080p“ verwenden.
   
   Zur Vereinfachung der Verwaltung besteht die Möglichkeit, den Namen des Ausgabemedienobjekts und den Namen des Auftrags zu bearbeiten.
   
   ![Codieren von Medienobjekten](./media/media-services-portal-vod-get-started/media-services-encode1.png)
5. Klicken Sie auf **Erstellen**.

### <a name="monitor-encoding-job-progress"></a>Überwachen des Codierauftragsstatus
Klicken Sie zum Überwachen des Codierauftragsstatus auf **Einstellungen** (oben auf der Seite), und wählen Sie dann **Aufträge** aus.

![Aufträge](./media/media-services-portal-vod-get-started/media-services-jobs.png)

## <a name="publish-content"></a>Veröffentlichen von Inhalten
Um Ihren Benutzern eine URL für das Streaming bzw. Herunterladen des Inhalts angeben zu können, müssen Sie zunächst das Medienobjekt veröffentlichen, indem Sie einen Locator erstellen. Locators ermöglichen den Zugriff auf Dateien im Medienobjekt. Media Services unterstützt zwei Arten von Locatorobjekten: 

* Streaminglocators (OnDemandOrigin), die für das adaptive Streaming verwendet werden (z.B. zum Streamen von MPEG DASH, HLS oder Smooth Streaming). Für die Erstellung eines Streaminglocators muss das Objekt eine ISM-Datei enthalten. 
* Progressive Locators (SAS), die für die Bereitstellung von Videos per progressivem Download verwendet werden.

Eine Streaming-URL weist das folgende Format auf. Mit dieser URL können Sie Smooth Streaming-Medienobjekte wiedergeben.

    {streaming endpoint name-media services account name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest

Um eine HLS-Streaming-URL zu erstellen, fügen Sie "(format=m3u8-aapl)" an die URL an.

    {streaming endpoint name-media services account name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=m3u8-aapl)

Um eine MPEG DASH-Streaming-URL zu erstellen, fügen Sie „(format=mpd-time-csf)“ an die URL an.

    {streaming endpoint name-media services account name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=mpd-time-csf)


Eine SAS-URL weist das folgende Format auf.

    {blob container name}/{asset name}/{file name}/{SAS signature}

> [!NOTE]
> Wenn Sie das Portal vor März 2015 zum Erstellen von Locators verwendet haben, wurden diese mit einem Ablaufdatum von zwei Jahren erstellt.  
> 
> 

Verwenden Sie zum Aktualisieren eines Ablaufdatums für einen Locator die [REST](https://docs.microsoft.com/rest/api/media/operations/locator#update_a_locator)- oder [.NET](http://go.microsoft.com/fwlink/?LinkID=533259)-APIs. Wenn Sie das Ablaufdatum eines SAS-Locators aktualisieren, ändert sich auch die URL.

### <a name="to-use-the-portal-to-publish-an-asset"></a>So verwenden Sie das Portal zum Veröffentlichen eines Objekts
So veröffentlichen Sie ein Medienobjekt über das Portal:

1. Wählen Sie **Einstellungen** > **Ressourcen**.
2. Wählen Sie das Objekt aus, das Sie veröffentlichen möchten.
3. Klicken Sie auf die Schaltfläche **Veröffentlichen** .
4. Wählen Sie den Locatortyp aus.
5. Klicken Sie auf **Hinzufügen**.
   
    ![Veröffentlichen](./media/media-services-portal-vod-get-started/media-services-publish1.png)

Die URL wird der Liste mit den **veröffentlichten URLs** hinzugefügt.

## <a name="play-content-from-the-portal"></a>Wiedergeben von Inhalten im Portal
Im Azure-Portal wird ein Inhaltsplayer bereitgestellt, mit dem Sie Ihre Videos testen können.

Klicken Sie auf das gewünschte Video und dann auf die Schaltfläche **Wiedergeben** .

![Veröffentlichen](./media/media-services-portal-vod-get-started/media-services-play.png)

Folgende Überlegungen sollten berücksichtigt werden:

* Starten Sie die Ausführung des Streamingendpunkts **Standard**, um das Streaming zu starten.
* Vergewissern Sie sich, dass das Video veröffentlicht wurde.
* Dieser **Media Player** verwendet für die Wiedergabe den standardmäßigen Streamingendpunkt. Wenn Sie die Wiedergabe von einem anderen Streamingendpunkt starten möchten, können Sie die URL per Klick kopieren und einen anderen Player verwenden. Sie können beispielsweise den [Azure Media Services-Player](http://amsplayer.azurewebsites.net/azuremediaplayer.html)nutzen.

## <a name="next-steps"></a>Nächste Schritte
Überprüfen Sie die Media Services-Lernpfade.

[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Feedback geben
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]


