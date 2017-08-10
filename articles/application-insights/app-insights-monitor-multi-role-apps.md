---
title: "Azure Application Insights-Unterstützung für mehrere Komponenten, Microservices und Container | Microsoft-Dokumentation"
description: "Überwachen der Leistung und Nutzung von Apps, die aus mehreren Komponenten oder Rollen bestehen"
services: application-insights
documentationcenter: 
author: CFreemanwa
manager: carmonm
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 05/17/2017
ms.author: sewhee
ms.translationtype: HT
ms.sourcegitcommit: 141270c353d3fe7341dfad890162ed74495d48ac
ms.openlocfilehash: d8b466caba7201a5bb8612e773ad61943f6d1cf2
ms.contentlocale: de-de
ms.lasthandoff: 07/25/2017

---
# <a name="monitor-multi-component-applications-with-application-insights-preview"></a>Überwachen von Anwendungen mit mehreren Komponenten mit Application Insights (Vorschau)

Sie können Apps, die aus mehreren Serverkomponenten, Rollen oder Diensten bestehen, mit [Azure Application Insights](app-insights-overview.md) überwachen. Die Integrität der Komponenten und die Beziehungen zwischen ihnen werden in einer einzelnen Anwendungszuordnung angezeigt. Sie können einzelne Vorgänge über mehrere Komponenten mit automatischer HTTP-Korrelation verfolgen. Eine Containerdiagnose kann integriert und mit Anwendungstelemetriedaten korreliert werden. Verwenden Sie eine einzelne Application Insights-Ressource für alle Komponenten Ihrer Anwendung. 

![Anwendungszuordnung mit mehreren Komponenten](./media/app-insights-monitor-multi-role-apps/app-map.png)

Wir verwenden „Komponente“ hier für jegliches funktionierende Teil einer umfangreichen Anwendung. Eine typische Geschäftsanwendung kann beispielsweise aus Clientcode bestehen, der in Webbrowsern ausgeführt wird, die mit einem oder mehreren Web-App-Diensten kommunizieren, die wiederum Back-End-Dienste verwenden. Serverkomponenten können lokal oder in der Cloud gehostet werden, Azure-Webrollen und Azure-Workerrollen sein oder in Containern wie Docker oder Service Fabric ausgeführt werden. 

### <a name="sharing-a-single-application-insights-resource"></a>Gemeinsames Verwenden einer einzelnen Application Insights-Ressource 

Die wichtigste Technologie ist das Senden von Telemetriedaten von jeder Komponente in der Anwendung an dieselbe Application Insights-Ressource. Verwenden Sie gegebenenfalls die `cloud_RoleName`-Eigenschaft, um zwischen den Komponenten zu unterscheiden. Das Application Insights SDK fügt den von Komponenten ausgegebenen Telemetriedaten die Eigenschaft `cloud_RoleName` hinzu. So fügt das SDK der Eigenschaft `cloud_RoleName` beispielsweise einen Websitenamen oder einen Dienstrollennamen hinzu. Dieser Wert kann mit einem Telemetrie-Initialisierer überschrieben werden. Die Anwendungszuordnung verwendet die Eigenschaft `cloud_RoleName`, um die Komponenten in der Zuordnung zu identifizieren.

Weitere Informationen zum Überschreiben der Eigenschaft `cloud_RoleName` finden Sie unter [Hinzufügen von Eigenschaften: ITelemetryInitializer](app-insights-api-filtering-sampling.md#add-properties-itelemetryinitializer).  

In einigen Fällen ist dies eventuell nicht möglich, sodass Sie bevorzugt unterschiedliche Ressourcen für verschiedene Gruppen von Komponenten verwenden sollten. Möglicherweise müssen Sie z.B. verschiedene Ressourcen für Verwaltungs- und Abrechnungszwecke verwenden. Die Verwendung unterschiedlicher Ressourcen bedeutet, dass nicht alle Komponenten in einer Anwendungszuordnung angezeigt werden. Außerdem können Sie in [Analytics](app-insights-analytics.md) keine Abfragen über mehrere Komponenten durchführen. Darüber hinaus müssen Sie auch separate Ressourcen einrichten.

Trotz dieser Einschränkung gehen wir im verbleibenden Teil dieses Dokuments davon aus, dass Sie Daten von mehreren Komponenten an eine Application Insights-Ressource senden möchten.

## <a name="configure-multi-component-applications"></a>Konfigurieren von Anwendungen mit mehreren Komponenten

Um eine Anwendungszuordnung mit mehreren Komponenten zu erhalten, müssen Sie folgende Aktionen durchführen:

* **Installieren Sie die aktuelle Vorabversion** des Application Insights-Pakets in jeder Komponente der Anwendung. 
* **Verwenden Sie eine einzelne Application Insights-Ressource** für alle Komponenten Ihrer Anwendung.
* **Aktivieren Sie die Anwendungszuordnung mit mehreren Rollen** auf dem Blatt „Vorschauversionen“.

Konfigurieren Sie jede Komponente Ihrer Anwendung mit der entsprechenden Methode für den jeweiligen Typ. ([ASP.NET](app-insights-asp-net.md), [Java](app-insights-java-get-started.md), [Node.js](app-insights-nodejs.md), [JavaScript](app-insights-javascript.md)).

### <a name="1-install-the-latest-pre-release-package"></a>1. Installieren des neuesten Vorabversionspakets

Aktualisieren oder installieren Sie die Application Insights-Pakete im Projekt zu jeder Serverkomponente. Wenn Sie Visual Studio verwenden:

1. Klicken Sie mit der rechten Maustaste auf das Projekt, und wählen Sie **NuGet-Pakete verwalten** aus. 
2. Wählen Sie **Vorabversion einbeziehen** aus.
3. Wenn Application Insights-Pakete in den Updates angezeigt werden, wählen Sie sie aus. 

    Suchen Sie andernfalls nach dem entsprechenden Paket, und installieren Sie es:
    
    * Microsoft.ApplicationInsights.WindowsServer
    * Microsoft.ApplicationInsights.ServiceFabric: für Komponenten, die als Gast ausgeführt werden, und Docker-Container, die in einer Service Fabric-Anwendung ausgeführt werden
    * Microsoft.ApplicationInsights.ServiceFabric.Native: für Reliable Services in ServiceFabric-Anwendungen
    * Microsoft.ApplicationInsights.Kubernetes: für Komponenten, die in Docker auf Kubernetes ausgeführt werden

### <a name="2-share-a-single-application-insights-resource"></a>2. Gemeinsames Verwenden einer einzelnen Application Insights-Ressource

* Klicken Sie in Visual Studio mit der rechten Maustaste auf ein Projekt, und wählen Sie **Application Insights konfigurieren** oder **Application Insights > Konfigurieren** aus. Verwenden Sie für das erste Projekt den Assistenten, um eine Application Insights-Ressource zu erstellen. Wählen Sie bei nachfolgenden Projekten dieselbe Ressource aus.
* Wenn kein Application Insights-Menü vorhanden ist, führen Sie die Konfiguration manuell durch:

   1. Öffnen Sie im [Azure-Portal](https://portal,azure.com) die Application Insights-Ressource, die Sie bereits für eine andere Komponente erstellt haben.
   2. Öffnen Sie auf dem Blatt „Übersicht“ die Dropdownliste „Zusammenfassung“, und kopieren Sie den **Instrumentierungsschlüssel**.
   3. Öffnen Sie in Ihrem Projekt die Datei „ApplicationInsights.config“, und fügen Sie Folgendes ein: `<InstrumentationKey>your copied key</InstrumentationKey>`

![Kopieren des Instrumentierungsschlüssels in die CONFIG-Datei](./media/app-insights-monitor-multi-role-apps/copy-instrumentation-key.png)


### <a name="3-enable-multi-role-application-map"></a>3. Aktivieren der Anwendungszuordnung mit mehreren Rollen

Öffnen Sie im Azure-Portal die Ressource für Ihre Anwendung. Aktivieren Sie auf dem Blatt „Vorschauversionen“ die *Anwendungszuordnung mit mehreren Rollen*.

### <a name="4-enable-docker-metrics-optional"></a>4. Aktivieren von Docker-Metriken (optional) 

Wenn eine Komponente in einer in Docker gehosteten Windows-VM in Azure ausgeführt wird, können Sie zusätzliche Metriken aus dem Container erfassen. Fügen Sie diese in die Konfigurationsdatei von [Azure-Diagnose](../monitoring-and-diagnostics/azure-diagnostics.md) ein:

```
"DiagnosticMonitorConfiguration": {
        ...
        "sinks": "applicationInsights",
        "DockerSources": {
                "Stats": {
                    "enabled": true,
                    "sampleRate": "PT1M"
                }
            },
        ...
    }
    ...   
    "SinksConfig": {
        "Sink": [{
            "name": "applicationInsights",
            "ApplicationInsights": "<your instrumentation key here>"
        }]
    }
    ...
}

```

## <a name="use-cloudrolename-to-separate-components"></a>Verwenden von cloud_RoleName zum Trennen von Komponenten

Die `cloud_RoleName`-Eigenschaft ist an alle Telemetriedaten angefügt. Sie identifiziert die Komponente – die Rolle oder den Dienst –, von der die Telemetriedaten stammen. (Sie ist nicht identisch mit cloud_RoleInstance, mit der identische Rollen getrennt werden, die gleichzeitig in mehreren Serverprozessen oder auf mehreren Computern ausgeführt werden.)

Sie können Ihrer Telemetriedaten mithilfe dieser Eigenschaft im Portal filtern oder segmentieren. In diesem Beispiel wird das Blatt „Fehler“ gefiltert, sodass nur Informationen vom Front-End-Webdienst angezeigt und Fehler vom CRM-API-Back-End herausgefiltert werden:

![Metrikdiagramm segmentiert nach Cloudrollenname](./media/app-insights-monitor-multi-role-apps/cloud-role-name.png)

## <a name="trace-operations-between-components"></a>Ablaufverfolgungsvorgänge zwischen Komponenten

Sie können die Aufrufe, die während der Verarbeitung eines einzelnen Vorgangs erfolgen, von einer Komponente an eine andere nachverfolgen.


![Anzeigen der Telemetrie für einen Vorgang](./media/app-insights-monitor-multi-role-apps/show-telemetry-for-operation.png)

Klicken Sie durch eine korrelierte Liste von Telemetriedaten für diesen Vorgang auf dem Front-End-Webserver und der Back-End-API:

![Suchen in Komponenten](./media/app-insights-monitor-multi-role-apps/search-across-components.png)


## <a name="next-steps"></a>Nächste Schritte

* [Trennen von Telemetriedaten zwischen Entwicklung, Test und Produktion](app-insights-separate-resources.md)

