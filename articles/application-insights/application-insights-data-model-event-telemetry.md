---
title: 'Azure Application Insights-Telemetriedatenmodell: Ereignistelemetrie | Microsoft-Dokumentation'
description: "Application Insights-Datenmodell für Ereignistelemetrie"
services: application-insights
documentationcenter: .net
author: SergeyKanzhelev
manager: carmonm
ms.service: application-insights
ms.workload: TBD
ms.tgt_pltfrm: ibiza
ms.devlang: multiple
ms.topic: article
ms.date: 04/25/2017
ms.author: cfreeman
ms.translationtype: Human Translation
ms.sourcegitcommit: aaf97d26c982c1592230096588e0b0c3ee516a73
ms.openlocfilehash: 1cad3594be32e59ea6bd3d3ba2289d391bd92c0b
ms.contentlocale: de-de
ms.lasthandoff: 04/27/2017


---
# <a name="event-telemetry-application-insights-data-model"></a>Ereignistelemetrie: Application Insights-Datenmodell

Sie können Ereignistelemetrieelemente erstellen (in [Application Insights](app-insights-overview.md)), um ein Ereignis darzustellen, das in Ihrer Anwendung aufgetreten ist. In der Regel handelt es sich um eine Benutzerinteraktion wie ein Klicken auf eine Schaltfläche oder das Zahlen eines Warenkorbs. Es kann sich auch um ein Ereignis im Lebenszyklus einer Anwendung handeln, wie z.B. eine Initialisierung oder Konfigurationsaktualisierung. 

Semantisch können Ereignisse nun mit Anforderungen in Korrelation stehen. Bei ordnungsgemäßer Verwendung ist Ereignistelemetrie jedoch wichtiger als Anforderungen oder Ablaufverfolgungen. Ereignisse stellen Geschäftstelemetrie dar, für die eine getrennte, weniger aggressive [Stichprobennahme](app-insights-api-filtering-sampling.md) erfolgen muss.

## <a name="name"></a>Name

Ereignisname. Um eine ordnungsgemäße Gruppierung und nützliche Metriken zu ermöglichen, schränken Sie Ihre Anwendung so ein, dass sie eine geringe Anzahl von separaten Ereignisnamen generiert. Verwenden Sie z. B. nicht für jede generierte Instanz eines Ereignisses einen separaten Namen.

Maximale Länge: 512 Zeichen

## <a name="custom-properties"></a>Benutzerdefinierte Eigenschaften

[!INCLUDE [application-insights-data-model-properties](../../includes/application-insights-data-model-properties.md)]

## <a name="custom-measurements"></a>Benutzerdefinierte Messungen

[!INCLUDE [application-insights-data-model-measurements](../../includes/application-insights-data-model-measurements.md)]

## <a name="next-steps"></a>Nächste Schritte

- Informationen zu den Application Insights-Typen und zum Datenmodell finden Sie unter [Datenmodell](application-insights-data-model.md).
- [Schreiben benutzerdefinierter Ereignistelemetriedaten](app-insights-api-custom-events-metrics.md#trackevent)
- Überprüfen der von Application Insights unterstützten [Plattformen](app-insights-platforms.md).

