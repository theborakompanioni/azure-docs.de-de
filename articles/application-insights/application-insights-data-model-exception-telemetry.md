---
title: 'Azure Application Insights-Telemetriedatenmodell: Ausnahmentelemetrie | Microsoft-Dokumentation'
description: "Application Insights-Datenmodell für Ausnahmentelemetrie"
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
ms.openlocfilehash: 17a39660fce598610ff9a95e886282e6b3faffe4
ms.lasthandoff: 04/22/2017


---
# <a name="exception-telemetry-application-insights-data-model"></a>Ausnahmentelemetrie: Application Insights-Datenmodell

Eine Instanz einer Ausnahme stellt eine behandelte oder nicht behandelte Ausnahme dar, die während der Ausführung der überwachten Anwendung aufgetreten ist.

## <a name="problem-id"></a>Problem-ID

Bezeichner der Stelle, an der die Ausnahme im Code ausgelöst wurde. Dient zum Gruppieren von Ausnahmen. In der Regel eine Kombination von Ausnahmetyp und einer Funktion in der Aufrufliste.

Maximale Länge: 1024 Zeichen

## <a name="severity-level"></a>Schweregrad

Schweregrad der Ablaufverfolgung. Möglicher Wert: `Verbose`, `Information`, `Warning`, `Error`, `Critical`.

## <a name="exception-details"></a>Ausnahmedetails

Dieser Abschnitt ist in Bearbeitung...

## <a name="custom-properties"></a>Benutzerdefinierte Eigenschaften

[!INCLUDE [application-insights-data-model-properties](../../includes/application-insights-data-model-properties.md)]

## <a name="custom-measurements"></a>Benutzerdefinierte Messungen

[!INCLUDE [application-insights-data-model-measurements](../../includes/application-insights-data-model-measurements.md)]

## <a name="next-steps"></a>Nächste Schritte

- Informationen zu den Application Insights-Typen und zum Datenmodell finden Sie unter [Datenmodell](/application-insights-data-model.md).
- Erfahren Sie mehr zum [Diagnostizieren von Ausnahmen in Ihren Web-Apps mit Application Insights](/app-insights-asp-net-exceptions.md).
- Überprüfen der von Application Insights unterstützten [Plattformen](/app-insights-platforms.md).

