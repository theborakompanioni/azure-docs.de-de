---
title: 'Azure Application Insights-Telemetriedatenmodell: Ausnahmentelemetrie | Microsoft-Dokumentation'
description: "Application Insights-Datenmodell für Ausnahmentelemetrie"
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
ms.author: sewhee
ms.translationtype: Human Translation
ms.sourcegitcommit: aaf97d26c982c1592230096588e0b0c3ee516a73
ms.openlocfilehash: 49d2429c01fe3fde2e51e4344904f55e43eea1cd
ms.contentlocale: de-de
ms.lasthandoff: 04/27/2017

---
# <a name="exception-telemetry-application-insights-data-model"></a>Ausnahmentelemetrie: Application Insights-Datenmodell

In [Application Insights](app-insights-overview.md) stellt eine Instanz einer Ausnahme eine behandelte oder nicht behandelte Ausnahme dar, die während der Ausführung der überwachten Anwendung aufgetreten ist.

## <a name="problem-id"></a>Problem-ID

Bezeichner der Stelle, an der die Ausnahme im Code ausgelöst wurde. Dient zum Gruppieren von Ausnahmen. In der Regel eine Kombination von Ausnahmetyp und einer Funktion in der Aufrufliste.

Maximale Länge: 1024 Zeichen

## <a name="severity-level"></a>Schweregrad

Schweregrad der Ablaufverfolgung. Möglicher Wert: `Verbose`, `Information`, `Warning`, `Error`, `Critical`.

## <a name="exception-details"></a>Ausnahmedetails

(Erweiterung folgt)

## <a name="custom-properties"></a>Benutzerdefinierte Eigenschaften

[!INCLUDE [application-insights-data-model-properties](../../includes/application-insights-data-model-properties.md)]

## <a name="custom-measurements"></a>Benutzerdefinierte Messungen

[!INCLUDE [application-insights-data-model-measurements](../../includes/application-insights-data-model-measurements.md)]

## <a name="next-steps"></a>Nächste Schritte

- Informationen zu den Application Insights-Typen und zum Datenmodell finden Sie unter [Datenmodell](application-insights-data-model.md).
- Erfahren Sie mehr zum [Diagnostizieren von Ausnahmen in Ihren Web-Apps mit Application Insights](app-insights-asp-net-exceptions.md).
- Überprüfen der von Application Insights unterstützten [Plattformen](app-insights-platforms.md).

