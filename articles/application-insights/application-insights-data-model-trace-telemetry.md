---
title: 'Azure Application Insights-Telemetriedatenmodell: Ablaufverfolgungstelemetrie | Microsoft-Dokumentation'
description: "Application Insights-Datenmodell für Ablaufverfolgungstelemetrie"
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
ms.openlocfilehash: 178b1ec0edce5a3fb20dc5c591e12cfe6e721cf2
ms.contentlocale: de-de
ms.lasthandoff: 04/27/2017

---
# <a name="trace-telemetry-application-insights-data-model"></a>Ablaufverfolgungstelemetrie: Application Insights-Datenmodell

Ablaufverfolgungstelemetrie stellt (in [Application Insights](app-insights-overview.md)) Ablaufverfolgungsanweisungen im Format `printf` dar, die sich für eine Textsuche eignen. `Log4Net`, `NLog` und andere textbasierte Protokolldateieinträge werden in Instanzen dieses Typs übersetzt. Die Ablaufverfolgung weist für die Erweiterbarkeit keine Messungen auf.

## <a name="message"></a>Nachricht

Ablaufverfolgungsmeldung.

Maximale Länge: 32.768 Zeichen

## <a name="severity-level"></a>Schweregrad

Schweregrad der Ablaufverfolgung. Möglicher Wert: `Verbose`, `Information`, `Warning`, `Error`, `Critical`.

## <a name="custom-properties"></a>Benutzerdefinierte Eigenschaften

[!INCLUDE [application-insights-data-model-properties](../../includes/application-insights-data-model-properties.md)]

## <a name="next-steps"></a>Nächste Schritte

- [Untersuchen von .NET-Ablaufverfolgungsprotokollen in Application Insights](app-insights-asp-net-trace-logs.md).
- [Untersuchen von Java-Ablaufverfolgungsprotokollen in Application Insights](app-insights-java-trace-logs.md).
- Informationen zu den Application Insights-Typen und zum Datenmodell finden Sie unter [Datenmodell](application-insights-data-model.md).
- [Schreiben benutzerdefinierter Telemetriedaten für die Ablaufverfolgung](app-insights-api-custom-events-metrics.md#tracktrace)
- Überprüfen der von Application Insights unterstützten [Plattformen](app-insights-platforms.md).

