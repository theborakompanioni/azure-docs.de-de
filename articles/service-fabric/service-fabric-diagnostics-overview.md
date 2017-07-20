---
title: "Übersicht über Azure Service Fabric-Überwachung und -Diagnose | Microsoft-Dokumentation"
description: "Erfahren Sie mehr über die Überwachung und Diagnose für Cluster, Anwendungen und Dienste von Azure Service Fabric."
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
ms.openlocfilehash: 571c31b9e6514b44d6a8a69fe8a6a0806e4b80e3
ms.contentlocale: de-de
ms.lasthandoff: 05/31/2017


---

# <a name="monitoring-and-diagnostics-for-azure-service-fabric"></a>Überwachung und Diagnose für Azure Service Fabric

Überwachung und Diagnose sind wichtig für Entwicklung und Tests sowie die Bereitstellung von Anwendungen und Diensten in allen Umgebungen. Service Fabric-Lösungen funktionieren am besten, wenn Sie die Überwachung und Diagnose planen und implementieren. So stellen Sie sicher, dass Anwendungen und Dienste in einer lokalen Entwicklungsumgebung oder in der Produktionsumgebung wie erwartet ausgeführt werden.

Die Hauptziele der Überwachung und Diagnose sind:
* Erkennen und Diagnostizieren von Hardware- und Infrastrukturproblemen
* Erkennen von Software- und App-Problemen, Verringern von Dienstausfallzeiten
* Verstehen des Ressourcenverbrauchs und Hilfe bei betrieblichen Entscheidungen
* Optimieren der Leistung von Anwendung, Dienst und Infrastruktur
* Generieren von geschäftlichen Einblicken und Identifizieren von Verbesserungspotenzialen


Der allgemeine Workflow für die Überwachung und Diagnose besteht aus drei Schritten:

1. **Ereignisgenerierung:** Dies schließt Ereignisse (Protokolle, Ablaufverfolgungen, benutzerdefinierte Ereignisse) auf Infrastruktur- (Cluster-) und Anwendungs-/Dienstebene ein.
2. **Ereignisaggregation:** Die generierten Ereignisse müssen gesammelt und aggregiert werden, bevor sie angezeigt werden können.
3. **Analyse:** Die Ereignisse müssen visualisiert und in einem Format verfügbar sein, das die Analyse und Anzeige je nach Bedarf ermöglicht.

Mehrere Produkte sind erhältlich, die alle drei Bereiche abdecken. Zudem können Sie für jeden Bereich unterschiedliche Technologien auswählen. Dabei muss sichergestellt werden, dass diese jeweils zusammenpassen, um eine umfassende Überwachungslösung für den Cluster bereitzustellen.

## <a name="event-generation"></a>Ereignisgenerierung

Der erste Schritt im Workflow für die Überwachung und Diagnose besteht in der Erstellung und Generierung von Ereignissen und Protokollen. Diese Ereignisse, Protokolle und Ablaufverfolgungen werden auf zwei Ebenen generiert: auf Infrastrukturebene (Cluster, Computer oder Service Fabric-Aktionen) oder auf Anwendungsebene (Instrumentierungen in Apps und Diensten, die im Cluster bereitgestellt wurden). Ereignisse auf jeder dieser Ebenen können angepasst werden, Service Fabric umfasst jedoch auch standardmäßig einige Instrumentierungen.

Weitere Informationen zu den enthaltenen Instrumentierungen und zum Hinzufügen weiterer Instrumentierungen finden Sie unter [Ereignisse auf Infrastrukturebene](service-fabric-diagnostics-event-generation-infra.md) und [Ereignisse auf Anwendungsebene](service-fabric-diagnostics-event-generation-app.md).

Nachdem Sie festgelegt haben, welchen Protokollanbieter Sie verwenden möchten, müssen Sie sicherstellen, dass die Protokolle ordnungsgemäß aggregiert und gespeichert werden.

## <a name="event-aggregation"></a>Ereignisaggregation

Für das Sammeln der Protokolle und Ereignisse, die von Ihren Anwendungen und Ihrem Cluster generiert werden, empfehlen wir normalerweise [Azure-Diagnose](service-fabric-diagnostics-event-aggregation-wad.md) (ähnelt eher der Agent-basierten Protokollsammlung) oder [EventFlow](service-fabric-diagnostics-event-aggregation-eventflow.md) (In-Process-Protokollsammlung).

Das Sammeln von Anwendungsprotokollen per Azure-Diagnose-Erweiterung ist eine gute Option für Service Fabric-Dienste, wenn die Gruppe von Protokollquellen und -zielen sich nicht häufig ändert und eine einfache Zuordnung zwischen den Quellen und ihren Zielen besteht. Der Grund hierfür ist, dass die Konfiguration von Azure-Diagnose auf der Resource Manager-Ebene erfolgt, sodass umfassende Änderungen an der Konfiguration die Aktualisierung und erneute Bereitstellung des Clusters erforderlich machen. Zudem lässt sich die Azure-Diagnose optimal nutzen, wenn sichergestellt ist, dass die Protokolle an einem permanenteren Ort gespeichert werden, über den verschiedene Analyseplattformen auf sie zugreifen können. Das bedeutet, dass die Pipeline letztlich weniger effizient ist als bei einer Option wie EventFlow.

Bei Verwendung von [EventFlow](https://github.com/Azure/diagnostics-eventflow) ist es möglich, dass Dienste ihre Protokolle direkt an eine Analyse- und Visualisierungsplattform und/oder einen Speicher senden. Für den gleichen Zweck können auch andere Bibliotheken (ILogger, Serilog usw.) verwendet werden, aber EventFlow hat den Vorteil, dass die Anwendung speziell für die In-Process-Protokollsammlung und die Unterstützung von Service Fabric-Diensten konzipiert wurde. Dies hat mehrere potenzielle Vorteile:

* Einfache Konfiguration und Bereitstellung
    * Die Konfiguration der Erfassung von Diagnosedaten ist Teil der Dienstkonfiguration. Es ist einfach, sie stets mit dem Rest der Anwendung „synchron“ zu halten.
    * Eine anwendungs- oder dienstspezifische Konfiguration ist problemlos möglich.
    * Bei der Konfiguration von Datenzielen über EventFlow müssen lediglich das entsprechende NuGet-Paket hinzugefügt und die Datei *eventFlowConfig.json* geändert werden.
* Flexibilität
    * Die Anwendung kann die Daten an ein beliebiges Ziel senden – vorausgesetzt, es ist eine Clientbibliothek vorhanden, die das gewünschte Datenspeichersystem unterstützt. Neue Ziele können je nach Bedarf hinzugefügt werden.
    * Komplexe Erfassungs-, Filter- und Datenaggregierungsregeln können implementiert werden.
* Zugriff auf interne Anwendungsdaten und Kontext
    * Das Diagnosesubsystem innerhalb des Anwendungs-/Dienstprozesses kann die Ablaufverfolgungen problemlos mit Kontextinformationen ergänzen.

Dabei ist zu beachten, dass diese beiden Optionen sich nicht gegenseitig ausschließen. Und obwohl sich ähnliche Aufgaben unter Verwendung der einen oder der anderen Option durchführen lassen, kann es auch sinnvoll sein, beide Optionen einzurichten. In den meisten Fällen kann die Kombination eines Agents mit der In-Process-Sammlung zu einem zuverlässigeren Überwachungsworkflow führen. Sie können die Azure-Diagnose-Erweiterung (Agent) beispielsweise als Pfad für Protokolle auf Infrastrukturebene auswählen und EventFlow (In-Process-Sammlung) für Protokolle auf Anwendungsebene verwenden. Wenn Sie herausgefunden haben, welche Option sich am besten für Sie eignet, müssen Sie sich damit befassen, wie Ihre Daten angezeigt und analysiert werden sollen.

## <a name="event-analysis"></a>Ereignisanalyse

Für die Analyse und Visualisierung von Überwachungs- und Diagnosedaten sind mehrere hervorragende Plattformen auf dem Markt verfügbar. Aufgrund der besseren Integration in Service Fabric empfehlen wir zwei davon: [OMS](service-fabric-diagnostics-event-analysis-oms.md) und [Application Insights](service-fabric-diagnostics-event-analysis-appinsights.md). Sehen Sie sich aber auch [Elastic Stack](https://www.elastic.co/products) (vor allem, wenn Sie einen Cluster in einer Offlineumgebung ausführen möchten), [Splunk](https://www.splunk.com/) oder eine andere Plattform Ihrer Wahl an.

Wichtige Punkte für die Auswahl einer Plattform sollten u.a. sein, wie Sie mit der Benutzeroberfläche und den Abfrageoptionen zurechtkommen, welche Optionen zum Visualisieren von Daten und Erstellen übersichtlicher Dashboards enthalten sind und welche zusätzlichen Tools zur Optimierung der Überwachung zur Verfügung stehen (z.B. automatische Warnungen).

Zusätzlich zu der ausgewählten Plattform erhalten Sie beim Einrichten eines Service Fabric-Clusters als Azure-Ressource auch Zugriff auf die vordefinierten Überwachungsoptionen von Azure für Computer, die nützlich sein können für eine spezifische Überwachung der Leistung und Metriken.

### <a name="azure-monitor"></a>Azure Monitor

Sie können [Azure Monitor](../monitoring-and-diagnostics/monitoring-overview.md) zum Überwachen von vielen Azure-Ressourcen verwenden, auf denen ein Service Fabric-Cluster erstellt wird. Ein Satz von Metriken für die [VM-Skalierungsgruppe](../monitoring-and-diagnostics/monitoring-supported-metrics.md#microsoftcomputevirtualmachinescalesets) (Virtual Machine Scale Set, VMSS) und einzelne [virtuelle Computer](../monitoring-and-diagnostics/monitoring-supported-metrics.md#microsoftcomputevirtualmachinescalesetsvirtualmachines) wird automatisch gesammelt und im Azure-Portal angezeigt. Wählen Sie zum Anzeigen der gesammelten Informationen im Azure-Portal die Ressourcengruppe aus, die den Service Fabric-Cluster enthält. Wählen Sie anschließend die VM-Skalierungsgruppe aus, die Sie anzeigen möchten. Wählen Sie im Abschnitt **Überwachung** die Option **Metriken**, um einen Graphen mit den Werten anzuzeigen.

![Ansicht des Azure-Portals mit gesammelten Metrikinformationen](media/service-fabric-diagnostics-overview/azure-monitoring-metrics.png)

Führen Sie die Schritte der Anleitung unter [Überblick über Metriken in Microsoft Azure](../monitoring-and-diagnostics/insights-how-to-customize-monitoring.md) aus, um die Diagramme anzupassen. Sie können basierend auf diesen Metriken auch Warnungen erstellen, wie unter [Erstellen von Warnungen in Azure Monitor für Azure-Dienste](../monitoring-and-diagnostics/insights-alerts-portal.md) beschrieben. Sie können Warnungen an einen Benachrichtigungsdienst senden, indem Sie Webhooks verwenden. Dies ist unter [Konfigurieren eines Webhooks für eine Azure-Metrikwarnung](../monitoring-and-diagnostics/insights-webhooks-alerts.md) beschrieben. Für Azure Monitor wird nur ein Abonnement unterstützt. Wenn Sie mehrere Abonnements überwachen müssen oder zusätzliche Features benötigen, können Sie [Log Analytics](https://azure.microsoft.com/documentation/services/log-analytics/) verwenden. Log Analytics ist Teil der Microsoft Operations Management Suite und stellt eine umfassende IT-Verwaltungslösung für lokale und cloudbasierte Infrastrukturen bereit. Sie können Daten von Azure Monitor direkt an Log Analytics weiterleiten, damit sich die Metriken und Protokolle für Ihre gesamte Umgebung an einem Ort befinden.

## <a name="next-steps"></a>Nächste Schritte

### <a name="watchdogs"></a>Watchdogs

Ein Watchdog ist ein separater Dienst, der die Integrität und die Auslastung von Diensten überwachen und die Integrität für alle Elemente in der Hierarchie des Integritätsmodells melden kann. So können Fehler verhindert werden, die nicht erkannt werden, wenn nur ein einzelner Dienst betrachtet wird. Watchdogs sind auch ein guter Platz zum Hosten von Code, der Aktionen zur Problembehebung ohne Benutzeraktion ausführt (z.B. Bereinigen von Protokolldateien im Speicher in bestimmten Zeitintervallen). Ein Beispiel für eine Watchdog-Dienstimplementierung finden Sie [hier](https://github.com/Azure-Samples/service-fabric-watchdog-service).

Machen Sie sich damit vertraut, wie Ereignisse und Protokolle auf der [Infrastrukturebene](service-fabric-diagnostics-event-generation-infra.md) und der [Anwendungsebene](service-fabric-diagnostics-event-generation-app.md) generiert werden.
