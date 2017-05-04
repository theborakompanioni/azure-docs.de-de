---
title: 'Azure Application Insights-Telemetriedatenmodell: Ablaufverfolgungstelemetrie | Microsoft-Dokumentation'
description: "Application Insights-Datenmodell für Ablaufverfolgungstelemetrie"
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
ms.openlocfilehash: e7825b26c47a80debf92be1ad166e5a18bae4a61
ms.lasthandoff: 04/22/2017


---
# <a name="trace-telemetry-application-insights-data-model"></a>Ablaufverfolgungstelemetrie: Application Insights-Datenmodell

Ablaufverfolgungstelemetrie stellt Ablaufverfolgungsanweisungen im Format `printf` dar, die sich für eine Textsuche eignen. `Log4Net`, `NLog` und andere textbasierte Protokolldateieinträge werden in Instanzen dieses Typs übersetzt. Die Ablaufverfolgung weist für die Erweiterbarkeit keine Messungen auf.

## <a name="message"></a>Nachricht

Ablaufverfolgungsmeldung.

Maximale Länge: 32.768 Zeichen

## <a name="severity-level"></a>Schweregrad

Schweregrad der Ablaufverfolgung. Möglicher Wert: `Verbose`, `Information`, `Warning`, `Error`, `Critical`.

## <a name="custom-properties"></a>Benutzerdefinierte Eigenschaften

[!INCLUDE [application-insights-data-model-properties](../../includes/application-insights-data-model-properties.md)]

## <a name="next-steps"></a>Nächste Schritte

- Informationen zu den Application Insights-Typen und zum Datenmodell finden Sie unter [Datenmodell](/application-insights-data-model.md).
- [Untersuchen von .NET-Ablaufverfolgungsprotokollen in Application Insights](/app-insights-asp-net-trace-logs.md).
- [Untersuchen von Java-Ablaufverfolgungsprotokollen in Application Insights](/app-insights-java-trace-logs.md).
- Überprüfen der von Application Insights unterstützten [Plattformen](/app-insights-platforms.md).

