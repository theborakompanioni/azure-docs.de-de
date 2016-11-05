---
title: Hinzufügen von Codierungseinheiten
description: Erfahren Sie, wie Sie mit .NET Codierungseinheiten hinzufügen.
services: media-services
documentationcenter: ''
author: juliako
manager: erikre
editor: ''

ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/01/2016
ms.author: juliako;milangada;gtrifonov

---
# Skalieren der Codierung mit .NET SDK
> [!div class="op_single_selector"]
> * [Portal](media-services-portal-scale-media-processing.md)
> * [.NET](media-services-dotnet-encoding-units.md)
> * [REST](https://msdn.microsoft.com/library/azure/dn859236.aspx)
> * [Java](https://github.com/southworkscom/azure-sdk-for-media-services-java-samples)
> * [PHP](https://github.com/Azure/azure-sdk-for-php/tree/master/examples/MediaServices)
> 
> 

## Übersicht
> [!IMPORTANT]
> Lesen Sie unbedingt das [Übersichtsthema](media-services-scale-media-processing-overview.md), um weitere Informationen zum Skalieren der Medienverarbeitung zu erhalten.
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

## Öffnen eines Supporttickets
Standardmäßig kann jedes Media Services-Konto auf bis zu 25 für das Codieren und bis zu 4 für das bedarfsgesteuerte Streaming reservierte Einheiten skaliert werden. Sie können einen höheren Grenzwert durch Öffnen eines Supporttickets anfordern.

### Öffnen eines Supporttickets
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

## Media Services-Lernpfade
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## Feedback geben
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

<!---HONumber=AcomDC_0907_2016-->