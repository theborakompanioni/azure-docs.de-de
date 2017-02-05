---
title: "  Veröffentlichen von Inhalten mithilfe des Azure-Portals | Microsoft Docs"
description: "Dieses Tutorial führt Sie durch die Schritte zur Veröffentlichung Ihrer Inhalte mithilfe des Azure-Portals."
services: media-services
documentationcenter: 
author: Juliako
manager: erikre
editor: 
ms.assetid: 92c364eb-5a5f-4f4e-8816-b162c031bb40
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/24/2016
ms.author: juliako
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: 96a8342dc782a9270a6de5b67387c35ea45b860e


---
# <a name="publish-content-with-the-azure-portal"></a>Veröffentlichen von Inhalten mithilfe des Azure-Portals
> [!div class="op_single_selector"]
> * [Portal](media-services-portal-publish.md)
> * [.NET](media-services-deliver-streaming-content.md)
> * [REST](media-services-rest-deliver-streaming-content.md)
> 
> 

## <a name="overview"></a>Übersicht
> [!NOTE]
> Sie benötigen ein Azure-Konto, um dieses Lernprogramm auszuführen. Ausführliche Informationen finden Sie unter [Kostenlose Azure-Testversion](https://azure.microsoft.com/pricing/free-trial/). 
> 
> 

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

Weitere Informationen finden Sie unter [Inhaltsbereitstellung – Übersicht](media-services-deliver-content-overview.md).

> [!NOTE]
> Die vor März 2015 über das Portal erstellten Locator weisen ein Ablaufdatum von zwei Jahren auf.  
> 
> 

Verwenden Sie zum Aktualisieren eines Ablaufdatums für einen Locator die [REST](http://msdn.microsoft.com/library/azure/hh974308.aspx#update_a_locator)- oder [.NET](http://go.microsoft.com/fwlink/?LinkID=533259)-APIs. Wenn Sie das Ablaufdatum eines SAS-Locators aktualisieren, ändert sich auch die URL.

### <a name="to-use-the-portal-to-publish-an-asset"></a>So verwenden Sie das Portal zum Veröffentlichen eines Objekts
So veröffentlichen Sie ein Medienobjekt über das Portal:

1. Wählen Sie im [Azure-Portal](https://portal.azure.com/) Ihr Azure Media Services-Konto aus.
2. Wählen Sie **Einstellungen** > **Ressourcen**.
3. Wählen Sie das Objekt aus, das Sie veröffentlichen möchten.
4. Klicken Sie auf die Schaltfläche **Veröffentlichen** .
5. Wählen Sie den Locatortyp aus.
6. Klicken Sie auf **Hinzufügen**.
   
    ![Veröffentlichen](./media/media-services-portal-vod-get-started/media-services-publish1.png)

Die URL wird der Liste mit den **veröffentlichten URLs**hinzugefügt.

## <a name="play-content-from-the-portal"></a>Wiedergeben von Inhalten im Portal
Im Azure-Portal wird ein Inhaltsplayer bereitgestellt, mit dem Sie Ihre Videos testen können.

Klicken Sie auf das gewünschte Video und dann auf die Schaltfläche **Wiedergeben** .

![Veröffentlichen](./media/media-services-portal-vod-get-started/media-services-play.png)

Folgende Überlegungen sollten berücksichtigt werden:

* Vergewissern Sie sich, dass das Video veröffentlicht wurde.
* Dieser **Media Player** verwendet für die Wiedergabe den standardmäßigen Streamingendpunkt. Wenn Sie die Wiedergabe von einem anderen Streamingendpunkt starten möchten, können Sie die URL per Klick kopieren und einen anderen Player verwenden. Sie können beispielsweise den [Azure Media Services-Player](http://amsplayer.azurewebsites.net/azuremediaplayer.html)nutzen.
* Der Streamingendpunkt, von dem aus das Streaming erfolgen soll.  
* Zum Streamen von einem Streamingendpunkt müssen Sie mindestens eine Streamingeinheit hinzufügen. Weitere Informationen finden Sie in [diesem](media-services-portal-scale-streaming-endpoints.md) Thema.   

## <a name="next-steps"></a>Nächste Schritte
Überprüfen Sie die Media Services-Lernpfade.

[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Feedback geben
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]




<!--HONumber=Nov16_HO3-->


