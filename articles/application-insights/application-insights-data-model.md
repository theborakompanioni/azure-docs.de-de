---
title: Azure Application Insights-Telemetriedatenmodell | Microsoft-Dokumentation
description: "Übersicht über das Application Insights-Datenmodell"
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
ms.openlocfilehash: 52f700bd18da87a9a38e0a791d0ec3496f201e0a
ms.lasthandoff: 04/22/2017


---
# <a name="application-insights-telemetry-data-model"></a>Application Insights-Telemetriedatenmodell

Application Insights definiert das Telemetriedatenmodell für Application Performance Management (APM). Dieses Modell standardisiert die Datensammlung und ermöglicht die Erstellung plattform- und sprachunabhängiger Überwachungsszenarien. Die mit Application Insights-Modellen gesammelten Daten ergeben das typische Anwendungsausführungsmuster:

![Application Insights-Anwendungsmodell](./media/application-insights-data-model/application-insights-data-model.png)

Zwei Arten von Anwendungen sind verfügbar: Anwendungen mit einem Endpunkt, die externe ***Anforderungen*** empfangen – Webanwendungen, und Anwendungen, die zum Verarbeiten von an bestimmten Speicherorten gespeicherten Daten regelmäßig aktiviert werden – WebJobs oder Funktionen. In beiden Fällen wird die eindeutige Ausführung eines ***Vorgangs*** aufgerufen. Der Vorgang wird erfolgreich durchgeführt oder schlägt mit einer ***Ausnahme*** fehl. Oder er hängt davon ab, dass andere Dienste/Speicher die zugehörige Geschäftslogik ausführen. Zur Berücksichtigung dieser Konzepte werden mit dem Application Insights-Datenmodell drei Telemetrietypen definiert: [Anforderung](./application-insights-data-model-request-telemetry.md), [Ausnahme](/application-insights-data-model-exception-telemetry.md) und [Abhängigkeit](/application-insights-data-model-dependency-telemetry.md).

Diese Typen werden normalerweise durch das Anwendungsframework definiert und automatisch durch das SDK gesammelt. `ASP.NET MVC` definiert das Konzept einer Anforderungsausführung in der zugehörigen Modell-Anzeige-Controller-Struktur und markiert den Anfang und das Ende einer Anforderung. Abhängigkeitsaufrufe von SQL werden durch `System.Data` definiert. Aufrufe von HTTP-Endpunkten werden durch `System.Net` definiert. Sie können die von einer bestimmten Plattform und einem bestimmten Framework gesammelten Telemetrietypen durch Verwendung benutzerdefinierter Eigenschaften und Messungen erweitern. In manchen Fällen möchten Sie jedoch unter Umständen benutzerdefinierte Telemetriedaten erfassen. Möglicherweise möchten Sie die Diagnoseprotokollierung in einem Ihnen vertrauten Instrumentierungsframework, z.B. `Log4Net` oder `System.Diagnostics`, implementieren. Oder Sie möchten Benutzerinteraktionen mit Ihrem Dienst erfassen, um Verwendungsmuster zu analysieren. In Application Insights werden zur Modellierung dieser Szenarien drei weitere Datentypen erkannt: [Ablaufverfolgung](/application-insights-data-model-trace-telemetry.md), [Ereignis](/application-insights-data-model-event-telemetry.md) und [Metrik](/application-insights-data-model-metric-telemetry.md).

Mit dem Application Insights-Telemetriedatenmodell wird die Weise definiert, auf die Telemetriedaten mit dem zugehörigen Vorgang [korreliert](/correlation.md) werden. Eine Anforderung kann z.B. SQL-Datenbankaufrufe durchführen und Diagnoseinformationen aufzeichnen. Sie können den Korrelationskontext für diese Telemetrieelemente festlegen, über den diese wieder mit der Anforderungstelemetrie verknüpft werden.

## <a name="schema-improvements"></a>Schemaverbesserungen

Mit dem Application Insights-Datenmodell können Sie Ihre Anwendungstelemetriedaten auf einfache, aber leistungsfähige Weise modellieren. Unser Ziel ist und bleibt ein einfaches und schlankes Modell, mit dem wesentliche Szenarien unterstützt werden und die Ausweitung des Schemas für erweiterte Anwendungsfälle ermöglicht wird.

Wenn Sie Probleme mit dem Datenmodell oder dem Schema melden möchten oder Fragen und Anregungen haben, verwenden Sie das GitHub-Repository [ApplicationInsights-Home](https://github.com/Microsoft/ApplicationInsights-Home/labels/schema).

## <a name="next-steps"></a>Nächste Schritte

- Lesen Sie die Informationen zu den von Application Insights unterstützten [Plattformen](/app-insights-platforms.md).
- Informationen zum [Erweitern und Filtern von Telemetriedaten](/app-insights-api-filtering-sampling.md).
- Verwenden von [Stichproben](/app-insights-sampling.md) zum Minimieren der auf dem Datenmodell basierenden Telemetriedaten.

