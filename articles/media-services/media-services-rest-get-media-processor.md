---
title: Erstellen eines Medienprozessors mithilfe von REST | Microsoft-Dokumentation
description: "Erfahren Sie, wie Sie eine Medienprozessorkomponente erstellen können, um Medieninhalte für Azure Media Services zu codieren, zu ver- oder entschlüsseln, und um Formate zu konvertieren."
services: media-services
documentationcenter: 
author: Juliako
manager: erikre
editor: 
ms.assetid: f9ff1997-0da6-4528-aaed-792837e5be41
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/31/2017
ms.author: juliako
ms.translationtype: HT
ms.sourcegitcommit: fff84ee45818e4699df380e1536f71b2a4003c71
ms.openlocfilehash: 4983069924f0edaeee7ffc91131bb83d9f9f4508
ms.contentlocale: de-de
ms.lasthandoff: 08/01/2017

---
# <a name="how-to-get-a-media-processor-instance"></a>Gewusst wie: Abrufen einer Media Processor-Instanz
> [!div class="op_single_selector"]
> * [.NET](media-services-get-media-processor.md)
> * [REST](media-services-rest-get-media-processor.md)
> 
> 

## <a name="overview"></a>Übersicht
Der Medienprozessor in Media Services ist für bestimmte Verarbeitungsaufgaben wie z. B. Codierung, Formatumwandlungen, Verschlüsselung oder Entschlüsselung von Medieninhalten zuständig. Normalerweise erstellen Sie einen Medienprozessor, wenn Sie eine Aufgabe zur Codierung, Verschlüsselung oder Formatumwandlung von Medieninhalten erstellen.

## <a name="azure-media-processors"></a>Azure-Medienprozessoren 

Das folgende Thema enthält Listen von Medienprozessoren:

* [Codieren von Medienprozessoren](scenarios-and-availability.md#encoding-media-processors)
* [Analytics-Medienprozessoren](scenarios-and-availability.md#analytics-media-processors)

## <a name="get-mediaprocessor"></a>Abrufen von MediaProcessor
> [!NOTE]
> Beim Verwenden der Media Services REST-API gelten die folgenden Überlegungen:
> 
> Wenn Sie in Media Services auf Entitäten zugreifen, müssen Sie bestimmte Headerfelder und Werte in Ihren HTTP-Anforderungen festlegen. Weitere Informationen finden Sie unter [Installation für die Entwicklung mit der Media Services-REST-API](media-services-rest-how-to-use.md).
> 
> Nach der erfolgreichen Verbindung mit „https://media.windows.net“ erhalten Sie eine 301 Redirect-Antwort, in der ein anderer Media Services-URI angegeben ist. Nachfolgende Aufrufe müssen an den neuen URI gesendet werden. Informationen zum Herstellen einer Verbindung mit der AMS-API finden Sie unter [Zugreifen auf die Azure Media Services-API per Azure AD-Authentifizierung](media-services-use-aad-auth-to-access-ams-api.md).
> 

Der folgende REST-Aufruf zeigt, wie Sie eine Medienprozessorinstanz nach Namen (in diesem Fall **Media Encoder Standard**) abrufen. 

Anforderung:

    GET https://media.windows.net/api/MediaProcessors()?$filter=Name%20eq%20'Media%20Encoder%20Standard' HTTP/1.1
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    User-Agent: Microsoft ADO.NET Data Services
    Authorization: Bearer <token>
    x-ms-version: 2.11
    Host: media.windows.net

Antwort:

    . . .

    {  
       "odata.metadata":"https://media.windows.net/api/$metadata#MediaProcessors",
       "value":[  
          {  
             "Id":"nb:mpid:UUID:ff4df607-d419-42f0-bc17-a481b1331e56",
             "Description":"Media Encoder Standard",
             "Name":"Media Encoder Standard",
             "Sku":"",
             "Vendor":"Microsoft",
             "Version":"1.1"
          }
       ]
    }


## <a name="media-services-learning-paths"></a>Media Services-Lernpfade
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Feedback geben
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="next-steps"></a>Nächste Schritte
Sie sind nun in der Lage, eine Medienprozessorinstanz zu erstellen, und können mit dem Thema [Codieren von Medienobjekten](media-services-rest-get-started.md) fortfahren. Dort lernen Sie, wie Sie Medienobjekte mit dem Media Encoder Standard codieren können.


