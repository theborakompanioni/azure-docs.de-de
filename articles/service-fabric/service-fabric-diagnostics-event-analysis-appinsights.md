---
title: Azure Service Fabric-Ereignisanalyse mit Application Insights | Microsoft-Dokumentation
description: "Erfahren Sie, wie Ereignisse unter Verwendung von Application Insights zur Überwachung und Diagnose von Azure Service Fabric-Clustern visualisiert und analysiert werden."
services: service-fabric
documentationcenter: .net
author: dkkapur
manager: timlt
editor: 
ms.assetid: 
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 05/26/2017
ms.author: dekapur
ms.translationtype: Human Translation
ms.sourcegitcommit: 5edc47e03ca9319ba2e3285600703d759963e1f3
ms.openlocfilehash: c88b2b5cff70e7a4b3642c53502f348f2e789e81
ms.contentlocale: de-de
ms.lasthandoff: 05/31/2017


---

# <a name="event-analysis-and-visualization-with-application-insights"></a>Ereignisanalyse und Visualisierung mit Application Insights

Azure Application Insights ist eine erweiterbare Plattform für die Überwachung und Diagnose von Anwendungen. Sie enthält ein leistungsfähiges Analyse- und Abfragetool, ein anpassbares Dashboard und Visualisierungen sowie weitere Optionen, z.B. automatisierte Warnungen. Sie ist die empfohlene Plattform für die Überwachung und Diagnose für Service Fabric-Anwendungen und -Dienste.

## <a name="setting-up-application-insights"></a>Einrichten von Application Insights

### <a name="creating-an-ai-resource"></a>Erstellen einer AI-Ressource

Navigieren Sie zum Erstellen einer AI-Ressource zum Azure Marketplace, und suchen Sie nach „Application Insights“. Dies sollte als erste Lösung angezeigt werden (in der Kategorie „Web und mobil“). Klicken Sie auf **Erstellen**, wenn die richtige Ressource angezeigt wird (überprüfen Sie, ob der Pfad der Abbildung unten entspricht).

![Neue Application Insights-Ressource](media/service-fabric-diagnostics-event-analysis-appinsights/create-new-ai-resource.png)

Sie müssen einige Informationen eingeben, damit die Ressource ordnungsgemäß bereitgestellt wird. Verwenden Sie im Feld *Anwendungstyp* die Option „ASP.NET-Webanwendung“, wenn Sie Service Fabric-Programmiermodelle verwenden oder eine .NET-Anwendung im Cluster veröffentlichen möchten. Verwenden Sie „Allgemein“, wenn Sie ausführbare Gastanwendungsdateien und Gastcontainer bereitstellen möchten. Im Allgemeinen sollten Sie standardmäßig „ASP.NET-Webanwendung“ verwenden, um sich alle Optionen offen zu halten. Den Namen können Sie nach Belieben auswählen, und die Ressourcengruppe und das Abonnement können nach der Bereitstellung der Ressource geändert werden. Es empfiehlt sich, dass die AI-Ressource sich in derselben Ressourcengruppe wie Ihr Cluster befindet. Weitere Informationen finden Sie unter [Erstellen einer Application Insights-Ressource](../application-insights/app-insights-create-new-resource.md).

Zum Konfigurieren von AI mit Ihrem Ereignisaggregationstool benötigen Sie den AI-Instrumentierungsschlüssel. Nachdem die AI-Ressource eingerichtet ist (dauert einige Minuten nach der Überprüfung der Bereitstellung), navigieren Sie zu dieser Ressource, und suchen Sie auf der linken Navigationsleiste den Abschnitt **Eigenschaften**. In dem dann geöffneten neuen Blatt wird ein *INSTRUMENTIERUNGSSCHLÜSSEL* angezeigt. Wenn Sie das Abonnement oder die Ressourcengruppe ändern möchten, ist dies ebenfalls hier möglich.

### <a name="configuring-ai-with-wad"></a>Konfigurieren von AI mit WAD

Es gibt zwei bevorzugte Möglichkeiten zum Senden von Daten von WAD an Azure AI. Dies erfolgt durch Hinzufügen einer AI-Senke in der WAD-Konfiguration, wie in [diesem Artikel](../monitoring-and-diagnostics/azure-diagnostics-configure-application-insights.md) ausführlich beschrieben.

#### <a name="add-an-ai-instrumentation-key-when-creating-a-cluster-in-azure-portal"></a>Hinzufügen eines AI-Instrumentierungsschlüssels beim Erstellen eines Clusters im Azure-Portal

![Hinzufügen eines AI-Schlüssels](media/service-fabric-diagnostics-event-analysis-appinsights/azure-enable-diagnostics.png)

Wenn beim Erstellen eines Clusters die Diagnose aktiviert ist („Ein“), wird ein optionales Feld angezeigt, in dem ein Application Insights-Instrumentierungsschlüssel eingegeben werden kann. Wenn Sie Ihren AI-IKey hier einfügen, wird die AI-Senke automatisch in der Resource Manager-Vorlage konfiguriert, die zum Bereitstellen Ihres Clusters verwendet wird.

#### <a name="add-the-ai-sink-to-the-resource-manager-template"></a>Hinzufügen der AI-Senke zur Resource Manager-Vorlage

Fügen Sie in „WadCfg“ der Resource Manager-Vorlage durch Einfügen der folgenden beiden Änderungen eine Senke („Sink“) hinzu:

1. Fügen Sie die Senkenkonfiguration hinzu:

    ```json
    "SinksConfig": {
        "Sink": [
            {
                "name": "applicationInsights",
                "ApplicationInsights": "***ADD INSTRUMENTATION KEY HERE***"
            }
        ]
    }

    ```

2. Fügen Sie die Senke in „DiagnosticMonitorConfiguration“ ein, indem Sie die folgende Zeile in „DiagnosticMonitorConfiguration“ von „WadCfg“ hinzufügen:

    ```json
    "sinks": "applicationInsights"
    ```

In beiden Codeausschnitten oben wurde der Name „applicationInsights“ für die Senke verwendet. Dieser Name muss nicht verwendet werden. Solange der Name der Senke in „sinks“ eingefügt wird, können Sie eine beliebige Zeichenfolge als Name festlegen.

Derzeit werden Protokolle vom Cluster als Ablaufverfolgungen in der AI-Protokollanzeige angezeigt. Da die meisten Ablaufverfolgungen aus der Infrastrukturebene den Typ „Informationen“ aufweisen, haben Sie auch die Möglichkeit, die Senkenkonfiguration so zu ändern, dass nur Protokolle der Typen „Kritisch“ und „Fehler“ gesendet werden. Dies kann durch Hinzufügen von „Kanälen“ zur Senke erfolgen, wie in [diesem Artikel](../monitoring-and-diagnostics/azure-diagnostics-configure-application-insights.md) erläutert.

>[!NOTE]
>Wenn Sie im Portal oder in der Resource Manager-Vorlage einen falschen AI-IKey verwenden, müssen Sie ihn manuell ändern und den Cluster aktualisieren und erneut bereitstellen. 

### <a name="configuring-ai-with-eventflow"></a>Konfigurieren von AI mit EventFlow

Wenn Sie EventFlow zum Aggregieren von Ereignissen verwenden, müssen Sie das NuGet-Paket `Microsoft.Diagnostics.EventFlow.Output.ApplicationInsights` importieren. Folgendes muss im Abschnitt *outputs* der Datei *eventFlowConfig.json* eingefügt werden:

```json
"outputs": [
    {
        "type": "ApplicationInsights",
        // (replace the following value with your AI resource's instrumentation key)
        "instrumentationKey": "00000000-0000-0000-0000-000000000000"
    }
]
```

Stellen Sie sicher, dass Sie die erforderlichen Änderungen in Ihren Filtern vornehmen und alle anderen Eingaben (zusammen mit den jeweiligen NuGet-Paketen) einfügen.

## <a name="aisdk"></a>AI.SDK

Im Allgemeinen wird empfohlen, EventFlow und WAD als Aggregationslösungen zu verwenden, da diese einen modularen Ansatz für die Diagnose und Überwachung bieten. Das heißt, wenn Sie die Ausgaben von EventFlow ändern möchten, ist keine Änderung an der eigentlichen Instrumentierung, sondern lediglich eine einfache Änderung an der Konfigurationsdatei erforderlich. Wenn Sie sich jedoch für die Verwendung von Application Insights entscheiden und voraussichtlich die Plattform nicht ändern werden, sollten Sie in Betracht ziehen, das neue SDK von AI einzusetzen, um Ereignisse zu aggregieren und an AI zu senden. Das bedeutet, dass Sie nicht mehr EventFlow zum Senden der Daten an AI konfigurieren müssen, sondern stattdessen das Service Fabric-NuGet-Paket von Application Insights installieren. Details zu diesem Paket finden Sie [hier](https://github.com/Microsoft/ApplicationInsights-ServiceFabric).

Unter [Application Insights-Unterstützung für Microservices und Container](https://azure.microsoft.com/app-insights-microservices/) werden einige der neuen Features beschrieben, an denen gearbeitet wird (derzeit noch in der Betaversion) und mit denen Sie über umfangreichere vordefinierte Überwachungsoptionen mit AI verfügen. Dazu gehören die Abhängigkeitsnachverfolgung (wird beim Erstellen einer AppMap aller Ihrer Dienste und Anwendungen in einem Cluster und der zugehörigen Kommunikation verwendet) und eine bessere Korrelation der Ablaufverfolgungen, die von Ihren Diensten stammen (trägt zur besseren genauen Erkennung eines Problems im Workflow einer App oder eines Diensts bei).

Wenn Sie beim Entwickeln in .NET voraussichtlich einige der Service Fabric-Programmiermodelle verwenden und AI als Plattform zum Visualisieren und Analysieren von Ereignis- und Protokolldaten verwenden möchten, wird empfohlen, das AI SDK für den Workflow der Überwachung und Diagnose zu verwenden. Informationen für den Einstieg in die Verwendung von AI zum Sammeln und Anzeigen Ihrer Protokolle finden Sie [hier](../application-insights/app-insights-asp-net-more.md) und [hier](../application-insights/app-insights-asp-net-trace-logs.md).

## <a name="navigating-the-ai-resource-in-azure-portal"></a>Navigieren zu der AI-Ressource im Azure-Portal

Nachdem Sie AI als Ausgabe für die Ereignisse und Protokolle konfiguriert haben, sollten nach wenigen Minuten Informationen in der AI-Ressource angezeigt werden. Navigieren Sie zu der AI-Ressource, um das Dashboard für die AI-Ressource anzuzeigen. Klicken Sie in der AI-Taskleiste auf **Suchen**, um die zuletzt empfangenen Ablaufverfolgungen anzuzeigen und sie filtern zu können.

*Metrik-Explorer* ist ein nützliches Tool zum Erstellen von benutzerdefinierten Dashboards basierend auf den Metriken, die Ihre Anwendungen, Dienste und Cluster melden. Informationen zum Einrichten einiger benutzerdefinierter Diagramme basierend auf Ihren gesammelten Daten finden Sie unter [Untersuchen von Metriken in Application Insights](../application-insights/app-insights-metrics-explorer.md).

Durch Klicken auf **Analytics** gelangen Sie zum Application Insights Analytics-Portal, in dem Sie Ereignisse und Ablaufverfolgungen in größerem Umfang und mit mehr Optionen abfragen können. Weitere Informationen hierzu finden Sie unter [Analytics in Application Insights](../application-insights/app-insights-analytics.md).

## <a name="next-steps"></a>Nächste Schritte

* [Richten Sie Warnungen in AI ein](../application-insights/app-insights-alerts.md), um Benachrichtigungen zu Änderungen der Leistung oder Nutzung zu erhalten.
* Die [intelligente Erkennung in Application Insights](../application-insights/app-insights-proactive-diagnostics.md) führt eine proaktive Analyse der an AI gesendeten Telemetriedaten aus, um Sie vor potenziellen Leistungsproblemen zu warnen.
