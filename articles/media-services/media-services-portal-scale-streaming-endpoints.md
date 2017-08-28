---
title: Skalieren von Streamingendpunkten mithilfe des Azure-Portals | Microsoft Docs
description: "Dieses Tutorial führt Sie durch die Schritte zur Skalierung von Streamingendpunkten mithilfe des Azure-Portals."
services: media-services
documentationcenter: 
author: Juliako
manager: cfowler
editor: 
ms.assetid: 1008b3a3-2fa1-4146-85bd-2cf43cd1e00e
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/04/2017
ms.author: juliako
ms.translationtype: Human Translation
ms.sourcegitcommit: bb794ba3b78881c967f0bb8687b1f70e5dd69c71
ms.openlocfilehash: 76de1d40e8c24abda33e99a137f4a6c263f60a8b
ms.contentlocale: de-de
ms.lasthandoff: 07/06/2017

---
# <a name="scale-streaming-endpoints-with-the-azure-portal"></a>Skalieren von Streamingendpunkten mithilfe des Azure-Portals
## <a name="overview"></a>Übersicht

> [!NOTE]
> Sie benötigen ein Azure-Konto, um dieses Lernprogramm auszuführen. Weitere Informationen finden Sie unter [Kostenloses Azure-Testkonto](https://azure.microsoft.com/pricing/free-trial/). 
> 
> 

**Premium**-Streamingendpunkte eignen sich für erweiterte Workloads und bieten eine dedizierte und skalierbare Bandbreitenkapazität. Kunden mit einem **Premium**-Streamingendpunkt erhalten standardmäßig eine Streamingeinheit (Streaming Unit, SU). Der Streamingendpunkt kann durch Hinzufügen von SUs skaliert werden. Jede SU bietet zusätzliche Bandbreitenkapazität für die Anwendung. Weitere Informationen zu Streamingendpunkt-Typen und der CDN-Konfiguration finden Sie im Thema [Übersicht über Streamingendpunkte](media-services-portal-manage-streaming-endpoints.md).
 
In diesem Thema wird gezeigt, wie ein Streamingendpunkt skaliert wird.

Informationen zu den Preisen finden Sie unter [Mediendienste – Preisübersicht](http://go.microsoft.com/fwlink/?LinkId=275107).

## <a name="scale-streaming-endpoints"></a>Skalieren von Streamingendpunkten

Gehen Sie wie folgt vor, um die Anzahl von Streamingeinheiten zu ändern:

1. Wählen Sie im [Azure-Portal](https://portal.azure.com/) Ihr Azure Media Services-Konto aus.
2. Klicken Sie im Fenster **Einstellungen** auf **Streamingendpunkte**.
3. Klicken Sie auf den Streamingendpunkt, den Sie skalieren möchten. 

    [!NOTE] Sie können nur **Premium**-Streamingendpunkte skalieren.

4. Verschieben Sie den Schieberegler, um die Anzahl der Streamingeinheiten anzugeben.

    ![Streamingendpunkt](./media/media-services-portal-manage-streaming-endpoints/media-services-manage-streaming-endpoints3.png)

## <a name="next-steps"></a>Nächste Schritte
Überprüfen Sie die Media Services-Lernpfade.

[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Feedback geben
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]


