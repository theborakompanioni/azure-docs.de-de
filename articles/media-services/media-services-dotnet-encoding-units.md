---
title: "Skalieren der Medienverarbeitung durch Hinzufügen von Codierungseinheiten – Azure | Microsoft-Dokumentation"
description: "Erfahren Sie, wie Sie mit .NET Codierungseinheiten hinzufügen."
services: media-services
documentationcenter: 
author: juliako
manager: erikre
editor: 
ms.assetid: 33f7625a-966a-4f06-bc09-bccd6e2a42b5
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: juliako;milangada;
translationtype: Human Translation
ms.sourcegitcommit: bdf41edfa6260749a91bc52ec0a2b62fcae99fb0
ms.openlocfilehash: ee40b92c48d91a391fe72582c7ea5e244907747f
ms.lasthandoff: 01/27/2017


---
# <a name="how-to-scale-encoding-with-net-sdk"></a>Skalieren der Codierung mit .NET SDK
> [!div class="op_single_selector"]
> * [Portal](media-services-portal-scale-media-processing.md)
> * [.NET](media-services-dotnet-encoding-units.md)
> * [REST](https://docs.microsoft.com/rest/api/media/operations/encodingreservedunittype)
> * [Java](https://github.com/southworkscom/azure-sdk-for-media-services-java-samples)
> * [PHP](https://github.com/Azure/azure-sdk-for-php/tree/master/examples/MediaServices)
> 
> 

## <a name="overview"></a>Übersicht
> [!IMPORTANT]
> Lesen Sie unbedingt das [Übersichtsthema](media-services-scale-media-processing-overview.md) , um weitere Informationen zum Skalieren der Medienverarbeitung zu erhalten.
> 
> 

Führen Sie folgende Schritte aus, um den Typ reservierter Einheiten und die Anzahl reservierter Einheiten für die Codierung mit dem .NET SDK zu ändern:

    IEncodingReservedUnit encodingS1ReservedUnit = _context.EncodingReservedUnits.FirstOrDefault();
    encodingS1ReservedUnit.ReservedUnitType = ReservedUnitType.Basic; // Corresponds to S1
    encodingS1ReservedUnit.Update();
    Console.WriteLine("Reserved Unit Type: {0}", encodingS1ReservedUnit.ReservedUnitType);

    encodingS1ReservedUnit.CurrentReservedUnits = 2;
    encodingS1ReservedUnit.Update();

    Console.WriteLine("Number of reserved units: {0}", encodingS1ReservedUnit.CurrentReservedUnits);

## <a name="opening-a-support-ticket"></a>Öffnen eines Supporttickets
Standardmäßig kann jedes Media Services-Konto auf bis zu 25 für das Codieren und bis zu 4 für das bedarfsgesteuerte Streaming reservierte Einheiten skaliert werden. Sie können einen höheren Grenzwert durch Öffnen eines Supporttickets anfordern.

### <a name="open-a-support-ticket"></a>Öffnen eines Supporttickets
Um ein Supportticket zu öffnen, gehen Sie folgendermaßen vor:

1. Klicken Sie auf [Support erhalten](https://manage.windowsazure.com/?getsupport=true). Wenn Sie nicht angemeldet sind, werden Sie zur Eingabe Ihrer Anmeldeinformationen aufgefordert.
2. Wählen Sie Ihr Abonnement aus.
3. Wählen Sie unter Supporttyp „Technisch“ aus.
4. Klicken Sie auf "Create Ticket".
5. Wählen Sie „Azure Media Services“ in der Produktliste auf der nächsten Seite aus.
6. Wählen Sie einen Problemtyp aus, der Ihr Problem beschreibt.
7. Klicken Sie auf „Weiter“.
8. Befolgen Sie die Anweisungen auf der nächsten Seite, und geben Sie dann die Details zu Ihrem Problem an.
9. Klicken Sie auf "Senden", um das Ticket zu öffnen.

## <a name="media-services-learning-paths"></a>Media Services-Lernpfade
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Feedback geben
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]


