---
title: "Überwachung auf Azure Service Fabric-Plattformebene | Microsoft-Dokumentation"
description: "In diesem Artikel erfahren Sie mehr über Ereignisse und Protokolle auf Plattformebene, die zum Überwachen und Diagnostizieren von Azure Service Fabric-Clustern verwendet werden."
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
ms.date: 07/17/2017
ms.author: dekapur
ms.translationtype: HT
ms.sourcegitcommit: 0425da20f3f0abcfa3ed5c04cec32184210546bb
ms.openlocfilehash: 2e320339f60b593c1cff68ca047c95f9cb7b33e2
ms.contentlocale: de-de
ms.lasthandoff: 07/20/2017

---

# <a name="platform-level-event-and-log-generation"></a>Ereignis- und Protokollgenerierung auf Plattformebene

## <a name="monitoring-the-cluster"></a>Überwachen des Clusters

Die Überwachung auf Plattformebene ist wichtig, um festzustellen, ob sich Ihre Hardware und Ihr Cluster erwartungsgemäß verhalten. Durch Service Fabric können Anwendungen bei Hardwareausfällen weiter ausgeführt werden. Sie müssen aber nach wie vor diagnostizieren, ob ein Fehler in einer Anwendung oder in der zugrunde liegenden Infrastruktur auftritt. Zudem sollten Sie zur besseren Kapazitätsplanung Ihre Cluster überwachen, um besser entscheiden zu können, ob Hardware hinzugefügt oder entfernt werden soll.

Service Fabric stellt fünf verschiedene vorkonfigurierte Protokollkanäle bereit, die die folgenden Ereignisse generieren:

* Betriebskanal: von Service Fabric und im Cluster ausgeführten Vorgänge auf höchster Ebene, einschließlich Ereignissen für einen gestarteten Knoten, eine neu bereitgestellte Anwendung oder ein Rollback für ein SF-Upgrade usw.
* Kundeninformationskanal: Integritätsberichte und Entscheidungen zum Lastenausgleich
* [Reliable Services-Ereignisse](service-fabric-reliable-services-diagnostics.md): spezifische Ereignisse für das Programmierungsmodell
* [Reliable Actors-Ereignisse](service-fabric-reliable-actors-diagnostics.md): spezifische Ereignisse für das Programmierungsmodell und Leistungsindikatoren
* Supportprotokolle: Systemprotokolle, die von Service Fabric nur für unseren Support generiert werden

Über diese verschiedenen Kanäle wird der Großteil der empfohlenen Protokollierung auf Plattformebene abgewickelt. Um die Protokollierung auf Plattformebene zu verbessern, sollten Sie sich einen besseren Überblick über das Integritätsmodell verschaffen und benutzerdefinierte Integritätsberichte hinzufügen. Fügen Sie außerdem benutzerdefinierte **Leistungsindikatoren** hinzu, um herauszufinden, welche Auswirkungen sich für Ihre Dienste und Anwendungen im Cluster in Echtzeit ergeben.

### <a name="azure-service-fabric-health-and-load-reporting"></a>Integritäts- und Auslastungsberichte für Azure Service Fabric

Service Fabric weist ein eigenes Integritätsmodell auf, das in diesen Artikeln ausführlich beschrieben wird:
- [Einführung in die Service Fabric-Integritätsüberwachung](service-fabric-health-introduction.md)
- [Melden und Überprüfen der Dienstintegrität](service-fabric-diagnostics-how-to-report-and-check-service-health.md)
- [Hinzufügen von benutzerdefinierten Service Fabric-Integritätsberichten](service-fabric-report-health.md)
- [Anzeigen von Service Fabric-Integritätsberichten](service-fabric-view-entities-aggregated-health.md)

Die Systemüberwachung ist entscheidend für mehrere Aspekte beim Betrieb eines Diensts. Die Systemüberwachung ist besonders wichtig, wenn Service Fabric ein Upgrade für eine benannte Anwendung durchführt. Nachdem jede Upgradedomäne des Diensts aktualisiert wurde und für Ihre Kunden verfügbar ist, muss die Upgradedomäne Integritätsprüfungen bestehen, bevor die Bereitstellung mit der nächsten Upgradedomäne fortfährt. Wenn kein ausreichender Integritätsstatus erreicht werden kann, wird für die Bereitstellung ein Rollback durchgeführt, damit sich die Anwendung wieder in einem bekannten guten Zustand befindet. Es können sich zwar Auswirkungen für einige Kunden ergeben, bis das Rollback für die Dienste abgeschlossen ist, aber die meisten Kunden bemerken keine Fehler. Darüber hinaus wird relativ schnell für eine Lösung gesorgt, ohne dass auf eine Aktion von einem menschlichen Bediener gewartet werden muss. Je mehr Integritätsprüfungen in Ihren Code eingebunden sind, desto widerstandsfähiger ist Ihr Dienst gegenüber Problemen bei der Bereitstellung.

Ein weiterer Aspekt der Dienstintegrität sind Berichte zu Metriken des Diensts. Metriken sind in Service Fabric wichtig, da sie zum Ausgleichen der Ressourcenverwendung genutzt werden. Außerdem können Metriken ein Indikator für die Systemintegrität sein. Sie können beispielsweise über eine Anwendung mit vielen Diensten verfügen, und jede Instanz meldet eine RPS-Metrik (Requests per Second, Anforderungen pro Sekunde). Wenn ein Dienst mehr Ressourcen als ein anderer Dienst nutzt, verschiebt Service Fabric Dienstinstanzen im Cluster, um eine gleichmäßige Ressourcenverwendung zu erreichen. Eine ausführlichere Beschreibung der Ressourcenverwendung finden Sie unter [Verwalten von Ressourcenverbrauch und Auslastung in Service Fabric mit Metriken](service-fabric-cluster-resource-manager-metrics.md).

Mit Metriken können Sie auch Erkenntnisse zur Leistung Ihres Diensts gewinnen. Im Laufe der Zeit können Sie Metriken verwenden, um zu überprüfen, ob der Dienst innerhalb der erwarteten Parameter betrieben wird. Wenn Trends beispielsweise anzeigen, dass der RPS-Durchschnittswert am Montag um 9:00 Uhr bei 1.000 liegt, können Sie einen Integritätsbericht einrichten, mit dem Sie gewarnt werden, wenn der RPS-Wert unter 500 oder über 1.500 liegt. Unter Umständen ist alles in Ordnung, aber eine Überprüfung könnte nicht schaden, um sicherzustellen, dass die Benutzererfahrung für Ihre Kunden optimal ist. Ihr Dienst kann eine Gruppe von Metriken definieren, die für Integritätsprüfungen gemeldet werden können, sich aber nicht auf den Lastenausgleich von Ressourcen des Clusters auswirken. Legen Sie die Metrikgewichtung hierfür auf null fest. Es wird empfohlen, bei allen Metriken mit einer Gewichtung von null zu beginnen und die Gewichtung erst zu erhöhen, wenn Sie sicher sind, wie sich die Gewichtung der Metriken auf den Ressourcenausgleich des Clusters auswirkt.

> [!TIP]
> Verwenden Sie nicht zu viele gewichtete Metriken. Es kann schwierig zu verstehen sein, warum Dienstinstanzen aus Gründen des Lastenausgleichs verschoben werden. Wenige Metriken können eine große Auswirkung haben!

Alle Informationen, mit denen die Integrität und Leistung Ihrer Anwendung angegeben wird, sind Kandidaten für Metriken und Integritätsberichte. Ein CPU-Leistungsindikator kann darauf hinweisen, wie ausgelastet der Knoten ist. Er informiert Sie aber nicht unbedingt darüber, ob ein bestimmter Dienst fehlerfrei ist, da ggf. mehrere Dienste auf einem einzelnen Knoten ausgeführt werden können. Aber mit Metriken wie „RPS“, „Verarbeitete Elemente“ und „Anforderungswartezeit“ kann die Integrität eines bestimmten Diensts angegeben werden.

Verwenden Sie Code wie diesen, um die Integrität zu melden:

  ```csharp
    if (!result.HasValue)
    {
        HealthInformation healthInformation = new HealthInformation("ServiceCode", "StateDictionary", HealthState.Error);
        this.Partition.ReportInstanceHealth(healthInformation);
    }
  ```

Verwenden Sie Code wie diesen, um eine Metrik zu melden:

  ```csharp
    this.Partition.ReportLoad(new List<LoadMetric> { new LoadMetric("MemoryInMb", 1234), new LoadMetric("metric1", 42) });
  ```

### <a name="service-fabric-support-logs"></a>Service Fabric-Supportprotokolle

Wenn Sie vom Microsoft-Support Hilfe zu Ihrem Azure Service Fabric-Cluster benötigen, sind fast immer Supportprotokolle erforderlich. Wenn Ihr Cluster in Azure gehostet wird, werden Supportprotokolle beim Erstellen eines Clusters automatisch konfiguriert und gesammelt. Die Protokolle werden in einem dedizierten Speicherkonto in der Ressourcengruppe des Clusters gespeichert. Das Speicherkonto hat keinen feststehenden Namen, aber im Konto werden Blobcontainer und -tabellen angezeigt, deren Namen mit *fabric* beginnen. Informationen zum Einrichten von Protokollsammlungen für einen eigenständigen Cluster finden Sie unter [Erstellen und Verwalten eines eigenständigen Azure Service Fabric-Clusters](service-fabric-cluster-creation-for-windows-server.md) und [Konfigurationseinstellungen für eigenständige Windows-Cluster](service-fabric-cluster-manifest.md). Für eigenständige Service Fabric-Instanzen sollten die Protokolle an eine lokale Dateifreigabe gesendet werden. Sie **benötigen** diese Protokolle für den Support, allerdings sind sie für Personen außerhalb des Microsoft-Supportteams nicht nützlich.

## <a name="enabling-diagnostics-for-a-cluster"></a>Aktivieren der Diagnose für einen Cluster

Um diese Protokolle nutzen zu können, wird dringend empfohlen, die „Diagnose“ bei der Clustererstellung zu aktivieren. Durch Aktivieren der Diagnose kann die Microsoft Azure-Diagnose bei der Bereitstellung des Clusters Betriebs-, Reliable Services- und Reliable Actors-Kanäle bestätigen, und die Daten speichern, wie **hier** ausführlich erläutert wird.

Wie oben zu sehen, gibt es außerdem ein optionales Feld für das Hinzufügen eines Instrumentierungsschlüssels von Application Insights (AppInsights). Wenn Sie AppInsights für eine Ereignisanalyse verwenden (weitere Informationen finden Sie **hier**), fügen Sie hier den instrumentationKey für die AppInsights-Ressource (GUID) ein.


Wenn Sie Container in Ihrem Cluster bereitstellen, aktivieren Sie WAD zum Abrufen von Docker-Statistiken, indem Sie sie zu Ihrer Konfiguration unter „WadCfg > DiagnosticMonitorConfiguration“ hinzufügen:

```json
"DockerSources": {
    "Stats": {
        "enabled": true,
        "sampleRate": "PT1M"
    }
},

```

## <a name="measuring-performance"></a>Messen der Leistung

Durch die Leistungsmessung Ihres Clusters können Sie sich einen Eindruck davon verschaffen, wie Lasten verarbeitet werden, und so Entscheidungen zur Skalierung Ihres Clusters vorantreiben (weitere Informationen finden Sie in den Artikeln zur Skalierung eines Clusters [in Azure](service-fabric-cluster-scale-up-down.md) oder [lokal](service-fabric-cluster-windows-server-add-remove-nodes.md)). Leistungsdaten sind auch bei der zukünftigen Analyse von Protokollen hilfreich im Vergleich zu den Aktionen, die Sie oder Ihre Anwendungen und Dienste eventuell getroffen haben. 

Eine Liste der Leistungsindikatordaten, die bei der Verwendung von Service Fabric gesammelt werden sollten, finden Sie unter [Leistungsindikatoren in Service Fabric](service-fabric-diagnostics-event-generation-perf.md)

Im Folgenden werden zwei allgemeine Verfahren vorgestellt, mit denen Sie die Sammlung von Leistungsdaten für Ihren Cluster einrichten können:

* Mithilfe eines Agents: Dies ist die bevorzugte Methode zur Sammlung von Leistungsdaten von einem Computer, da Agents in der Regel über eine Liste möglicher Leistungsmetriken verfügen, die gesammelt werden können. Zudem ist es ein relativ einfacher Vorgang, um die zu sammelnden oder zu ändernden Metriken auszuwählen. Lesen Sie die Artikel [Vorgehensweise zum Konfigurieren der OMS für Service Fabric](service-fabric-diagnostics-event-analysis-oms.md) und [Einrichten des Windows-Agents für die OMS](../log-analytics/log-analytics-windows-agents.md), um mehr über den OMS-Agent zu erfahren. Dabei handelt es sich um einen Überwachungs-Agent, der Leistungsdaten für Cluster-VMs und bereitgestellte Container abrufen kann.

* Konfigurieren von Diagnosen zum Schreiben von Leistungsindikatoren in eine Tabelle: Für Cluster in Azure bedeutet dies, dass die Konfiguration der Azure-Diagnose geändert wird, um die entsprechenden Leistungsindikatoren von den VMs in Ihrem Cluster abzurufen, und dass diese aktiviert wird, um bei der Bereitstellung von Containern Docker-Statistiken abzurufen. Weitere Informationen zum Einrichten der Sammlung von Leistungsindikatoren finden Sie unter [Konfigurieren von Leistungsindikatoren in WAD](service-fabric-diagnostics-event-aggregation-wad.md).

## <a name="next-steps"></a>Nächste Schritte

Ihre Protokolle und Ereignisse müssen aggregiert werden, bevor sie an eine beliebige Analyseplattform gesendet werden können. Lesen Sie die Abschnitte [EventFlow](service-fabric-diagnostics-event-aggregation-eventflow.md) und [WAD](service-fabric-diagnostics-event-aggregation-wad.md), um einige der empfohlenen Optionen besser zu verstehen.

