---
title: Azure Application Insights-Telemetriedatenmodell | Microsoft-Dokumentation
description: "Übersicht über das Application Insights-Datenmodell"
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
ms.author: bwren
ms.translationtype: HT
ms.sourcegitcommit: 266b9b7eb228744075627e1e80710e63c27880cc
ms.openlocfilehash: a2487aac712542d86cb3a7f34c45e21c8d035496
ms.contentlocale: de-de
ms.lasthandoff: 09/06/2017

---
# <a name="application-insights-telemetry-data-model"></a>Application Insights-Telemetriedatenmodell

[Azure Application Insights](app-insights-overview.md) sendet Telemetrie von Ihrer Webanwendung zum Azure-Portal, damit Sie die Leistung und Nutzung der Anwendung analysieren können. Der Telemetriemodell ist standardisiert, damit eine plattform- und sprachunabhängige Überwachung erstellt werden kann. 

Die mit Application Insights-Modellen gesammelten Daten ergeben dieses typische Anwendungsausführungsmuster:

![Application Insights-Anwendungsmodell](./media/application-insights-data-model/application-insights-data-model.png)

Folgende Telemetrietypen werden zum Überwachen der Ausführung Ihrer App verwendet. Die folgenden drei Typen werden in der Regel automatisch durch das Application Insights SDK aus dem Webanwendungsframework gesammelt:

* [**Anforderung**](application-insights-data-model-request-telemetry.md) – Zum Protokollieren einer Anforderung generiert, die von Ihrer App empfangen wurde. Das Web-SDK von Application Insights generiert z. B. automatisch einen Eintrag für „Telemetrie anfordern“ für jede HTTP-Anforderung, die Ihre Web-App empfängt. 

    Ein **Vorgang** umfasst den Ausführungsthread, der eine Anforderung verarbeitet. Sie können auch [Code schreiben](app-insights-api-custom-events-metrics.md#trackrequest), um andere Vorgangstypen zu überwachen, z. B. ein „wake up“ (aktivieren) in einem Webauftrag oder einer Funktion, die regelmäßig Daten verarbeitet.  Jeder Vorgang verfügt über eine ID. Mit dieser ID können alle Telemetriedaten [gruppiert](application-insights-correlation.md) werden, die bei der Verarbeitung der Anforderung durch Ihre App generiert werden. Jeder Vorgang wird entweder erfolgreich oder nicht erfolgreich ausgeführt und verfügt über eine bestimmte Dauer.
* [**Ausnahme**](application-insights-data-model-exception-telemetry.md) – Stellt in der Regel eine Ausnahme dar, die zum Fehlschlagen eines Vorgangs führt.
* [**Abhängigkeit**](application-insights-data-model-dependency-telemetry.md) – Stellt einen Aufruf von Ihrer App an einen externen Dienst oder Speicher wie REST-API oder SQL dar. Abhängigkeitsaufrufe von SQL werden in ASP.NET durch `System.Data` definiert. Aufrufe von HTTP-Endpunkten werden durch `System.Net` definiert. 

Application Insights bietet drei zusätzliche Datentypen für benutzerdefinierte Telemetrie:

* [Ablaufverfolgung](application-insights-data-model-trace-telemetry.md) – Dieser Typ wird entweder direkt oder über einen Adapter verwendet, um die Diagnoseprotokollierung über ein Instrumentierungsframework zu implementieren, das Ihnen vertraut ist, z. B. `Log4Net` oder `System.Diagnostics`.
* [Ereignis](application-insights-data-model-event-telemetry.md) – Dieser Typ wird in der Regel dazu verwendet, um Benutzerinteraktionen mit Ihrem Dienst zu erfassen, um Verwendungsmuster zu analysieren.
* [Metrik](application-insights-data-model-metric-telemetry.md) - Dieser Typ wird zum Melden periodischer skalarer Messungen verwendet.

Jedes Telemetrieelement kann die [Kontextinformationen](application-insights-data-model-context.md) (z.B. Anwendungsversion oder Benutzersitzungs-ID) definieren. Beim Kontext handelt es sich um eine Gruppe stark typisierter Felder, die bestimmte Szenarien zulässt. Wenn die Anwendungsversion ordnungsgemäß initialisiert wird, kann Application Insights neue Muster im Anwendungsverhalten in Korrelation mit der erneuten Bereitstellung erkennen. Über die Sitzungs-ID können der Ausfall oder die Auswirkung eines Problems für Benutzer berechnet werden. Die Berechnung des Distinct Count Measure der Werte der Sitzungs-ID für bestimmte fehlerhafte Abhängigkeiten, Fehlerablaufverfolgungen oder kritische Ausnahmen trägt zum besseren Verständnis der Auswirkungen bei.

Mit dem Application Insights-Telemetriedatenmodell wird eine Weise definiert, auf die Telemetriedaten mit dem zugehörigen Vorgang [korreliert](application-insights-correlation.md) werden. Eine Anforderung kann z.B. SQL-Datenbankaufrufe durchführen und Diagnoseinformationen aufzeichnen. Sie können den Korrelationskontext für diese Telemetrieelemente festlegen, über den diese wieder mit der Anforderungstelemetrie verknüpft werden.

## <a name="schema-improvements"></a>Schemaverbesserungen

Mit dem Application Insights-Datenmodell können Sie Ihre Anwendungstelemetriedaten auf einfache, aber leistungsfähige Weise modellieren. Unser Ziel ist und bleibt ein einfaches und schlankes Modell, mit dem wesentliche Szenarien unterstützt werden und die Ausweitung des Schemas für erweiterte Anwendungsfälle ermöglicht wird.

Wenn Sie Probleme mit dem Datenmodell oder dem Schema melden möchten oder Fragen und Anregungen haben, verwenden Sie das GitHub-Repository [ApplicationInsights-Home](https://github.com/Microsoft/ApplicationInsights-Home/labels/schema).

## <a name="next-steps"></a>Nächste Schritte

- [Schreiben benutzerdefinierter Telemetriedaten](app-insights-api-custom-events-metrics.md)
- Informationen zum [Erweitern und Filtern von Telemetriedaten](app-insights-api-filtering-sampling.md).
- Verwenden von [Stichproben](app-insights-sampling.md) zum Minimieren der auf dem Datenmodell basierenden Telemetriedaten.
- Lesen Sie die Informationen zu den von Application Insights unterstützten [Plattformen](app-insights-platforms.md).

