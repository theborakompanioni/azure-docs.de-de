---
title: 'Azure Application Insights-Telemetriedatenmodell: Ereignistelemetrie | Microsoft-Dokumentation'
description: "Application Insights-Datenmodell für Ereignistelemetrie"
services: application-insights
documentationcenter: .net
author: SergeyKanzhelev
manager: azakonov-ms
ms.service: application-insights
ms.workload: TBD
ms.tgt_pltfrm: ibiza
ms.devlang: multiple
ms.topic: article
ms.date: 04/17/2017
ms.author: sergkanz
translationtype: Human Translation
ms.sourcegitcommit: 9eafbc2ffc3319cbca9d8933235f87964a98f588
ms.openlocfilehash: 5b8b803f26dca82b5112568f486e5c347602a409
ms.lasthandoff: 04/22/2017


---
# <a name="event-telemetry-application-insights-data-model"></a>Ereignistelemetrie: Application Insights-Datenmodell

Ereignisse stellen eine Aktion in einer Anwendung dar, die zu einem bestimmten Zeitpunkt erfolgt ist. In der Regel ist es eine Benutzerinteraktion wie ein Klicken auf eine Schaltfläche oder das Zahlen eines Warenkorbs. Es kann sich auch um ein Ereignis im Lebenszyklus einer Anwendung handeln, wie z.B. eine Initialisierung oder Konfigurationsaktualisierung. Der Ereignisname soll eine kurze Zeichenfolge mit niedriger Kardinalität sein. 

Semantisch können Ereignisse nun mit Anforderungen in Korrelation stehen. Bei ordnungsgemäßer Verwendung ist Ereignistelemetrie jedoch wichtiger als Anforderungen oder Ablaufverfolgungen. Ereignisse stellen Geschäftstelemetrie dar, für die eine getrennte, weniger aggressive Stichprobennahme erfolgen muss.

## <a name="name"></a>Name

Ereignisname. Behalten Sie eine niedrige Kardinalität bei, um eine ordnungsgemäße Gruppierung und nützliche Metriken zu ermöglichen.

Maximale Länge: 512 Zeichen

## <a name="custom-properties"></a>Benutzerdefinierte Eigenschaften

[!INCLUDE [application-insights-data-model-properties](../../includes/application-insights-data-model-properties.md)]

## <a name="custom-measurements"></a>Benutzerdefinierte Messungen

[!INCLUDE [application-insights-data-model-measurements](../../includes/application-insights-data-model-measurements.md)]

## <a name="next-steps"></a>Nächste Schritte

- Informationen zu den Application Insights-Typen und zum Datenmodell finden Sie unter [Datenmodell](/application-insights-data-model.md).
- Erfahren Sie, wie Sie die [Application Insights-API für benutzerdefinierte Ereignisse und Metriken](/app-insights-asp-net-dependencies.md) nutzen.
- Überprüfen der von Application Insights unterstützten [Plattformen](/app-insights-platforms.md).

