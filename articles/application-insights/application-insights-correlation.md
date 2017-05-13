---
title: Korrelation der Azure Application Insights-Telemetrie | Microsoft-Dokumentation
description: Korrelation der Application Insights-Telemetrie
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
ms.author: sergkanz
ms.translationtype: Human Translation
ms.sourcegitcommit: aaf97d26c982c1592230096588e0b0c3ee516a73
ms.openlocfilehash: c48dc5cb5dd6dfa09ff9718e78f8d560886851be
ms.contentlocale: de-de
ms.lasthandoff: 04/27/2017


---
# <a name="telemetry-correlation-in-application-insights"></a>Telemetriekorrelation in Application Insights

In der Welt der Mikroservices müssen für jeden logischen Vorgang Einstellungen an den verschiedenen Dienstkomponenten vorgenommen werden. Jede dieser Komponenten kann separat von [Application Insights](app-insights-overview.md) überwacht werden. Die Web-App-Komponente kommuniziert mit der Authentifizierungsanbieterkomponente, um die Anmeldeinformationen des Benutzers zu überprüfen, und mit der API-Komponente, um Daten zur Visualisierung abzurufen. Die API-Komponente wiederum kann Daten von anderen Diensten abfragen, Cacheanbieterkomponenten nutzen und die Abrechnungskomponente über diesen Aufruf benachrichtigen. Application Insights unterstützt die verteilte Telemetriekorrelation. Damit können Sie ermitteln, welche Komponente für Fehler oder Leistungsbeeinträchtigungen verantwortlich ist.

In diesem Artikel wird das von Application Insights verwendete Datenmodell erläutert, mit dem die von mehreren Komponenten gesendeten Telemetriedaten korreliert werden. Darin werden die Methoden und Protokolle zur Kontextpropagierung erläutert. Zudem wird auch die Implementierung der Korrelationskonzepte in verschiedenen Sprachen und auf verschiedenen Plattformen behandelt.

## <a name="telemetry-correlation-data-model"></a>Telemetriekorrelations-Datenmodell

Application Insights definiert ein [Datenmodell](application-insights-data-model.md) für die verteilte Telemetriekorrelation. Um die Telemetrie mit dem logischen Vorgang zu verknüpfen, ist jedes Telemetrieelement mit einem Kontextfeld namens `operation_Id` versehen. Dieser Bezeichner wird von jedem Telemetrieelement in der verteilten Ablaufverfolgung gemeinsam genutzt. Selbst bei einem Verlust von Telemetriedaten von einer einzigen Ebene können Sie dennoch Telemetriedaten zuordnen, die von anderen Komponenten gemeldet wurden.

Der verteilte logische Vorgang besteht in der Regel aus einem Satz von kleineren Vorgängen, wobei die Anforderungen von einer der Komponenten verarbeitet werden. Diese Vorgänge werden von einer [Anforderungstelemetrie](application-insights-data-model-request-telemetry.md) definiert. Jede Anforderungstelemetrie verfügt über eine eigene `id`, die diese eindeutig global identifiziert. Für sämtliche Telemetriedaten (Ablaufverfolgungen, Ausnahmen usw.), die dieser Anforderung zugeordnet sind, sollte die `operation_parentId` auf den Wert der Anforderungs-`id` festgelegt werden.

Alle ausgehenden Vorgänge wie HTTP-Aufrufe an andere Komponenten werden durch eine [Abhängigkeitstelemetrie](application-insights-data-model-dependency-telemetry.md) dargestellt. Abhängigkeitstelemetrien definieren zudem ihre eigene `id`, die global eindeutig ist. Anforderungsabhängigkeiten, die durch diese Anforderungstelemetrie initiiert werden, verwenden diese als `operation_parentId`.

Sie können die Ansicht des verteilten logischen Vorgangs mit `operation_Id`, `operation_parentId` und `request.id` mit `dependency.id` erstellen. Diese Felder definieren auch die Kausalitätsreihenfolge der Telemetrieaufrufe.

In Mikroserviceumgebungen können Ablaufverfolgungen von Komponenten an unterschiedliche Speicher weitergeleitet werden. Jede Komponente kann einen eigenen Instrumentierungsschlüssel in Application Insights aufweisen. Um Telemetriedaten für den logischen Vorgang abzurufen, müssen Sie Daten von jedem Speicher abfragen. Bei einer immensen Anzahl von Speichern benötigen Sie einen Anhaltspunkt, um zu wissen, wo Sie als Nächstes suchen müssen.

Zur Behebung dieses Problems definiert das Datenmodell von Application Insights zwei Felder: `request.source` und `dependency.target`. Das erste Feld identifiziert die Komponente, die die Abhängigkeitsanforderung initiiert hat, und das zweite Feld identifiziert die Komponente, die die Antwort des Abhängigkeitsaufrufs zurückgegeben hat.


## <a name="example"></a>Beispiel

Veranschaulichen wir uns dies anhand eines Beispiels für eine STOCK PRICES-Anwendung, die den aktuellen Marktpreis eines Aktienkurses mithilfe der externen STOCKS API angibt. Die Anwendung STOCK PRICES enthält eine `Stock page`-Seite, die vom Clientwebbrowser mit `GET /Home/Stock` geöffnet wird. Die Anwendung fragt die STOCK API mit dem HTTP-Aufruf `GET /api/stock/value` ab.

Sie können die daraus resultierenden Telemetriedaten durch Ausführung einer Abfrage analysieren:

```
(requests | union dependencies | union pageViews) 
| where operation_Id == "STYz"
| project timestamp, itemType, name, id, operation_ParentId, operation_Id
```

Beachten Sie, dass in der Ergebnisansicht alle Telemetrieelemente das Stammverzeichnis `operation_Id` nutzen. Wenn ein Ajax-Aufruf über die Seite vorgenommen wird, wird die neue eindeutige ID `qJSXU` der Abhängigkeitstelemetrie zugewiesen und die Seitenansichts-ID dient als `operation_ParentId`. Die Serveranforderung verwendet im Gegenzug die Ajax-ID als `operation_ParentId` etc.

| itemType   | name                      | id           | operation_ParentId | operation_Id |
|------------|---------------------------|--------------|--------------------|--------------|
| pageView   | Stock page                |              | STYz               | STYz         |
| dependency | GET /Home/Stock           | qJSXU        | STYz               | STYz         |
| request    | GET Home/Stock            | KqKwlrSt9PA= | qJSXU              | STYz         |
| dependency | GET /api/stock/value      | bBrf2L7mm2g= | KqKwlrSt9PA=       | STYz         |

Nachdem Sie den `GET /api/stock/value`-Aufruf an einen externen Dienst getätigt haben, sollten Sie die Identität dieses Servers herausfinden. So können Sie das Feld `dependency.target` entsprechend festlegen. Wenn der externe Dienst keine Überwachung unterstützt, wird `target` auf den Hostnamen des Diensts, z.B. `stock-prices-api.com`, festgelegt. Wenn sich dieser Dienst jedoch durch Zurückgeben eines vordefinierten HTTP-Header selbst identifiziert, enthält `target` die Dienstidentität, mit dem Application Insights verteilte Ablaufverfolgungen durch Abfrage von Telemetriedaten von diesem Dienst erstellen kann. 

## <a name="correlation-headers"></a>Korrelations-Header

Wir arbeiten an RFC-Vorschlägen für das [Korrelations-HTTP-Protokoll](https://github.com/lmolkova/correlation/blob/master/http_protocol_proposal_v1.md). Diese Vorschläge definieren zwei Header:

- `Request-Id` enthält die global eindeutige ID des Aufrufs.
- `Correlation-Context` enthält die Sammlung an Name/Wert-Paaren der Eigenschaften von verteilten Ablaufverfolgungen.

Der Standard definiert außerdem zwei Schemata an `Request-Id`-Generierungen, nämlich flache und hierarchische Schemata. Beim flachen Schema ist ein bekannter `Id`-Schlüssel für die Sammlung `Correlation-Context` definiert.

Application Insights definiert die [Erweiterung](https://github.com/lmolkova/correlation/blob/master/http_protocol_proposal_v2.md) für das Korrelations-HTTP-Protokoll. Er verwendet `Request-Context`-Name/Wert-Paare, die die vom unmittelbaren Aufrufer oder Aufgerufenen verwendete Sammlung von Eigenschaften propagieren. Das Application Insights SDK legt mithilfe dieses Header die Felder `dependency.target` und `request.source` fest.

## <a name="open-tracing-and-application-insights"></a>OpenTracing und Application Insights

Aufbau von [OpenTracing](http://opentracing.io/)- und Application Insights-Datenmodellen 

- `request`, `pageView` entspricht **Span** mit `span.kind = server`
- `dependency` entspricht **Span** mit `span.kind = client`
- `id` einer `request` und `dependency` entspricht **Span.Id**
- `operation_Id` entspricht **TraceId**
- `operation_ParentId` entspricht **Reference** vom Typ `ChileOf`

Informationen zu den Application Insights-Typen und zum Datenmodell finden Sie unter [Datenmodell](application-insights-data-model.md).

Definitionen von OpenTracing-Konzepten finden Sie unter [Spezifikation](https://github.com/opentracing/specification/blob/master/specification.md) und [Semantic_conventions](https://github.com/opentracing/specification/blob/master/semantic_conventions.md).


## <a name="telemetry-correlation-in-net"></a>Telemetriekorrelation in .NET

Im Laufe der Zeit hat .NET mehrere Möglichkeiten zur Korrelation von Telemetrie- und Diagnoseprotokollen definiert. `System.Diagnostics.CorrelationManager` ermöglicht die Nachverfolgung von [LogicalOperationStack und ActivityId](https://msdn.microsoft.com/library/system.diagnostics.correlationmanager.aspx). `System.Diagnostics.Tracing.EventSource` und die Ereignisablaufverfolgung für Windows (ETW) definieren die Methode [SetCurrentThreadActivityId](https://msdn.microsoft.com/library/system.diagnostics.tracing.eventsource.setcurrentthreadactivityid.aspx). `ILogger` verwendet [Protokollbereiche](https://docs.microsoft.com/aspnet/core/fundamentals/logging#log-scopes). WCF und HTTP richten die „aktuelle“ Kontextpropagierung ein.

Allerdings wurde durch diese Methoden nicht die Unterstützung für automatische verteilte Ablaufverfolgung aktiviert. `DiagnosticsSource` ist eine Möglichkeit zur Unterstützung von automatischen computerübergreifenden Korrelationen. .NET-Bibliotheken unterstützen Diagnosequellen und ermöglichen die automatische computerübergreifende Propagierung des Korrelationskontexts durch Übertragungsmöglichkeiten wie HTTP.

Im [Leitfaden zu Aktivitäten](https://github.com/dotnet/corefx/blob/master/src/System.Diagnostics.DiagnosticSource/src/ActivityUserGuide.md) in der Diagnosequelle werden die Grundlagen von Nachverfolgungsaktivitäten erläutert. 

ASP.NET Core 2.0 unterstützt die Extraktion von HTTP-Headern und das Starten einer neuen Aktivität. 

`System.Net.HttpClient` mit Startversion `<fill in>` unterstützt die automatische Injektion der Korrelations-HTTP-Header und die Nachverfolgung des HTTP-Aufrufs als Aktivität.

Für das klassische ASP.NET ist das neue HTTP-Modul [Microsoft.AspNet.TelemetryCorrelation](https://www.nuget.org/packages/Microsoft.AspNet.TelemetryCorrelation/) verfügbar. Dieses Modul implementiert Telemetriekorrelationen mithilfe von DiagnosticsSource. Es startet Aktivitäten basierend auf den eingehenden Anforderungs-Headern. Zudem werden Telemetriedaten aus den verschiedenen Phasen der Anforderungsverarbeitung korreliert. Dies gilt selbst für Fälle, in denen jede Phase der Verarbeitung in IIS in verschiedenen Verwaltungsthreads ausgeführt wird.

Das Application Insights SDK mit der Startversion `2.4.0-beta1` verwendet DiagnosticsSource und Activity, um Telemetriedaten zu sammeln und diese mit der aktuellen Aktivität zu verknüpfen. 

## <a name="next-steps"></a>Nächste Schritte

- [Schreiben benutzerdefinierter Telemetriedaten](app-insights-api-custom-events-metrics.md)
- Integrieren Sie alle Komponenten Ihres Mikroservices in Application Insights. Überprüfen Sie die [unterstützten Plattformen](app-insights-platforms.md).
- Informationen zu den Application Insights-Typen und zum Datenmodell finden Sie unter [Datenmodell](application-insights-data-model.md).
- Informationen zum Erweitern und Filtern von Telemetriedaten finden Sie [hier](app-insights-api-filtering-sampling.md).

