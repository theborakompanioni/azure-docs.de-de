---
title: " Hochladen von Dateien in ein Media Services-Konto über das Azure-Portal | Microsoft Docs"
description: "In diesem Tutorial erfahren Sie Schritt für Schritt, wie Sie über das Azure-Portal Dateien in ein Media Services-Konto hochladen."
services: media-services
documentationcenter: 
author: Juliako
manager: SyntaxC4
editor: 
ms.assetid: 3ad3dcea-95be-4711-9aae-a455a32434f6
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 08/07/2017
ms.author: juliako
ms.translationtype: HT
ms.sourcegitcommit: f5c887487ab74934cb65f9f3fa512baeb5dcaf2f
ms.openlocfilehash: 8b9a7a266259077d49a38d51d7f801f420cc6315
ms.contentlocale: de-de
ms.lasthandoff: 08/08/2017

---
# <a name="upload-files-into-a-media-services-account-using-the-azure-portal"></a>Hochladen von Dateien in ein Media Services-Konto über das Azure-Portal
> [!div class="op_single_selector"]
> * [Portal](media-services-portal-upload-files.md)
> * [.NET](media-services-dotnet-upload-files.md)
> * [REST](media-services-rest-upload-files.md)
> 
> [!NOTE]
> Sie benötigen ein Azure-Konto, um dieses Lernprogramm auszuführen. Ausführliche Informationen finden Sie unter [Kostenlose Azure-Testversion](https://azure.microsoft.com/pricing/free-trial/). 
> 


In Media Services laden Sie Ihre digitalen Dateien in ein Medienobjekt hoch. Das Medienobjekt kann Videos, Audiodateien, Bilder, Miniaturansichtssammlungen, Texttitel und Untertiteldateien (und die Metadaten zu diesen Dateien) enthalten. Nachdem die Dateien hochgeladen wurden, werden Ihre Inhalte zur weiteren Verarbeitung und zum Streaming sicher in der Cloud gespeichert.


## <a name="upload-files"></a>Hochladen von Dateien

>[!NOTE]
>Bei der Verarbeitung in Media Services werden nur Dateien bis zu einer bestimmten Größe unterstützt. Ausführliche Informationen zur Dateigrößenbeschränkung finden Sie in [diesem Thema](media-services-quotas-and-limitations.md).
>

1. Wählen Sie im [Azure-Portal](https://portal.azure.com/) Ihr Azure Media Services-Konto aus.
2. Klicken Sie auf dem Blatt **Einstellungen** auf **Assets**.
   
    ![Hochladen von Dateien](./media/media-services-portal-vod-get-started/media-services-upload.png)
3. Klicken Sie auf die Schaltfläche **Upload** .
   
    Das Fenster **Upload a video asset** (Videoobjekt hochladen) wird angezeigt.
   
   > [!NOTE]
   > Es gibt in Bezug auf die Dateigröße keinerlei Beschränkung.
   > 
   > 
4. Suchen Sie auf Ihrem Computer nach dem gewünschten Video, wählen Sie es aus, und klicken Sie auf „OK“.  
   
    Der Upload beginnt, und der Status wird unter dem Dateinamen angezeigt.  

Nach Abschluss des Uploads sehen Sie, dass das neue Objekt im Fenster mit den **Medienobjekten** aufgeführt ist. 

## <a name="next-steps"></a>Nächste Schritte
Sie können nun Ihre hochgeladenen Medienobjekte codieren. Weitere Informationen finden Sie unter [Codieren von Medienobjekten](media-services-portal-encode.md).

Sie können auch mithilfe von Azure Functions einen Codierungsauftrag auslösen, wenn eine Datei im konfigurierten Container eingeht. Weitere Informationen finden Sie in [diesem Beispiel](https://azure.microsoft.com/resources/samples/media-services-dotnet-functions-integration/ ).

## <a name="media-services-learning-paths"></a>Media Services-Lernpfade
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Feedback geben
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]


