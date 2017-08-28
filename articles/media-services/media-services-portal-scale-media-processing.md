---
title: Skalieren der Medienverarbeitung mithilfe des Azure-Portals | Microsoft-Dokumentation
description: "Dieses Tutorial führt Sie durch die Schritte zur Skalierung der Medienverarbeitung mithilfe des Azure-Portals."
services: media-services
documentationcenter: 
author: Juliako
manager: cfowler
editor: 
ms.assetid: e500f733-68aa-450c-b212-cf717c0d15da
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/04/2017
ms.author: juliako
ms.translationtype: Human Translation
ms.sourcegitcommit: bb794ba3b78881c967f0bb8687b1f70e5dd69c71
ms.openlocfilehash: e6ef52f7aee8b2a0d0dd6ebc99ad7a8c5c11f525
ms.contentlocale: de-de
ms.lasthandoff: 07/06/2017

---
# <a name="change-the-reserved-unit-type"></a>Ändern Sie den Typ reservierter Einheiten
> [!div class="op_single_selector"]
> * [.NET](media-services-dotnet-encoding-units.md)
> * [Portal](media-services-portal-scale-media-processing.md)
> * [REST](https://docs.microsoft.com/rest/api/media/operations/encodingreservedunittype)
> * [Java](https://github.com/southworkscom/azure-sdk-for-media-services-java-samples)
> * [PHP](https://github.com/Azure/azure-sdk-for-php/tree/master/examples/MediaServices)
> 
> 

## <a name="overview"></a>Übersicht

Ein Media Services-Konto ist dem Typ einer reservierten Einheit zugeordnet, der die Geschwindigkeit bestimmt, mit der Medienverarbeitungsaufgaben erfolgen. Sie können zwischen den folgenden reservierten Einheitentypen wählen: **S1**, **S2** oder **S3**. Derselbe Codierungsauftrag wird bei Verwendung der reservierten Einheit **S2** beispielsweise schneller ausgeführt als mit dem Typ **S1**.

Zusätzlich zum Typ reservierter Einheiten können Sie angeben, dass für Ihr Konto **reservierte Einheiten** (Reserved Units, RUs) bereitgestellt werden sollen. Die Anzahl der bereitgestellten RUs bestimmt die Anzahl der Medienaufgaben, die gleichzeitig unter einem bestimmten Konto verarbeitet werden können.

>[!NOTE]
>RUs dienen der Parallelisierung der gesamten Medienverarbeitung, einschließlich der Indizierung von Aufträgen mit Azure Media Indexer. Allerdings erfolgt die Indizierung von Aufträgen im Gegensatz zur Codierung mit schnelleren reservierten Einheiten nicht schneller.

> [!IMPORTANT]
> Lesen Sie unbedingt das [Übersichtsthema](media-services-scale-media-processing-overview.md) , um weitere Informationen zum Skalieren der Medienverarbeitung zu erhalten.
> 
> 

## <a name="scale-media-processing"></a>Skalieren der Medienverarbeitung
Führen Sie folgende Schritte aus, um den Typ reservierter Einheiten und die Anzahl reservierter Einheiten zu ändern:

1. Wählen Sie im [Azure-Portal](https://portal.azure.com/) Ihr Azure Media Services-Konto aus.
2. Wählen Sie im Fenster **Einstellungen** die Option **Reservierte Einheiten für Medien** aus.
   
    Verwenden Sie zum Ändern der Anzahl reservierter Einheiten für den ausgewählten Typ den Schieberegler unter **Reservierte Einheiten für Medien** .
   
    Um den **TYP RESERVIERTER EINHEITEN**zu ändern, wählen Sie S1, S2 oder S3 aus.
   
    ![Prozessorenseite](./media/media-services-portal-scale-media-processing/media-services-scale-media-processing.png)
3. Klicken Sie auf die Schaltfläche "Speichern", um die Änderungen zu speichern.
   
    Die neuen reservierten Einheiten werden zugewiesen, wenn Sie auf SPEICHERN klicken.

## <a name="next-steps"></a>Nächste Schritte
Überprüfen Sie die Media Services-Lernpfade.

[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Feedback geben
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]


