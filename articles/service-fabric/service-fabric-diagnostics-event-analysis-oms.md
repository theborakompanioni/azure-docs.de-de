---
title: Azure Service Fabric-Ereignisanalyse mit OMS | Microsoft-Dokumentation
description: "Erfahren Sie, wie Sie Ereignisse unter Verwendung der OMS zur Überwachung und Diagnose von Azure Service Fabric-Clustern visualisieren und analysieren."
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
ms.openlocfilehash: d7857821524de3d1dbdac9e8c8d0da89678670db
ms.contentlocale: de-de
ms.lasthandoff: 05/31/2017


---

# <a name="event-analysis-and-visualization-with-oms"></a>Ereignisanalyse und Visualisierung mit OMS

Die Operations Management Suite (OMS) ist eine Sammlung von Verwaltungsdiensten, die die Überwachung und Diagnose für in der Cloud gehostete Anwendungen und Dienste unterstützen. Eine ausführlichere Übersicht über die OMS und ihre Optionen finden Sie unter [Was ist die OMS?](../operations-management-suite/operations-management-suite-overview.md).

## <a name="log-analytics-and-the-oms-workspace"></a>Log Analytics und der OMS-Arbeitsbereich

Log Analytics sammelt Daten von verwalteten Ressourcen, z.B. von einer Azure Storage-Tabelle oder einem Agent, und verwaltet sie in einem zentralen Repository. Die Daten können dann für Analyse, Warnungen und Visualisierung oder für den weiteren Export verwendet werden. Log Analytics unterstützt Ereignisse, Leistungsdaten oder andere benutzerdefinierten Daten.

Wenn die OMS konfiguriert ist, haben Sie Zugriff auf einen bestimmten *OMS-Arbeitsbereich*, über den Daten abgefragt oder in Dashboards visualisiert werden können.

Nachdem Daten von Log Analytics empfangen wurden, verfügt die OMS über mehrere *Verwaltungslösungen*. Dabei handelt es sich um vorkonfigurierte Lösungen zum Überwachen eingehender Daten, die an verschiedene Szenarien angepasst sind. Dazu gehören eine *Service Fabric-Analyselösung* und eine *Containerlösung*. Dies sind die beiden wichtigsten Lösungen für die Diagnose und Überwachung bei Verwendung von Service Fabric-Clustern. Es gibt aber auch verschiedene andere Lösungen, die in Betracht gezogen werden können. Außerdem ermöglicht die OMS die Erstellung benutzerdefinierter Lösungen. Entsprechende Informationen finden Sie [hier](https://docs.microsoft.com/azure/operations-management-suite/operations-management-suite-solutions). Jede Lösung, für deren Verwendung Sie sich für einen Cluster entscheiden, wird mit Log Analytics im gleichen OMS-Arbeitsbereich konfiguriert. Arbeitsbereiche ermöglichen benutzerdefinierte Dashboards und die Visualisierung von Daten sowie Änderungen an den Daten, die Sie sammeln, verarbeiten und analysieren möchten.

## <a name="setting-up-an-oms-workspace-with-the-service-fabric-solution"></a>Einrichten eines OMS-Arbeitsbereichs mit der Service Fabric-Lösung

Es wird empfohlen, dass Sie die Service Fabric-Lösung in Ihrem OMS-Arbeitsbereich einfügen, da sie ein nützliches Dashboard umfasst, über das die verschiedenen eingehenden Protokollkanäle von der Infrastruktur- und Anwendungsebene angezeigt werden und die Service Fabric-spezifischen Protokolle abgefragt werden können. Es folgt ein Beispiel für eine relativ einfache Service Fabric-Lösung, mit der eine einzelne Anwendung im Cluster bereitgestellt wird:

![OMS – SF-Lösung](media/service-fabric-diagnostics-event-analysis-oms/service-fabric-solution.png)

Es gibt zwei Möglichkeiten zum Bereitstellen und Konfigurieren eines OMS-Arbeitsbereichs: entweder über eine Resource Manager-Vorlage oder direkt über den Azure Marketplace. Verwenden Sie die erste Option, wenn Sie einen Cluster bereitstellen, und die zweite, wenn Sie bereits einen Cluster mit aktivierter Diagnose bereitgestellt haben.

### <a name="deploying-oms-using-a-resource-management-template"></a>Bereitstellen der OMS über eine Resource Manager-Vorlage

Dies erfolgt in der Phase der Clustererstellung: Beim Bereitstellen eines Clusters mithilfe einer Resource Manager-Vorlage kann mit der Vorlage auch ein neuer OMS-Arbeitsbereich erstellt, diesem die Service Fabric-Lösung hinzugefügt und der Arbeitsbereich so konfiguriert werden, dass Daten aus den entsprechenden Speichertabellen gelesen werden.

>[!NOTE]
>Dazu muss Diagnose aktiviert werden, sodass die Azure Storage-Tabelle für die OMS und Log Analytics vorhanden sind und Daten daraus gelesen werden können.

[Hier](https://azure.microsoft.com/resources/templates/service-fabric-oms/) finden Sie eine Beispielvorlage, die Sie verwenden und je nach Anforderung ändern können und über die die oben genannten Vorgänge durchgeführt werden. Für den Fall, dass Sie mehr Optionen benötigen, gibt es einige weitere Vorlagen, die verschiedene Optionen enthalten, abhängig davon, wo genau im Prozess Sie einen OMS-Arbeitsbereich einrichten. Diese finden Sie unter [Service Fabric und OMS-Vorlagen](https://azure.microsoft.com/resources/templates/?term=service+fabric+OMS).

### <a name="deploying-oms-using-through-azure-marketplace"></a>Bereitstellen der OMS über den Azure Marketplace

Wenn Sie einen OMS-Arbeitsbereich nach dem Bereitstellen eines Clusters hinzufügen möchten, navigieren Sie zum Azure Marketplace, und suchen Sie *Service Fabric-Analyse*. In der Kategorie „Überwachung und Verwaltung“ sollte nur eine Ressource angezeigt werden (siehe Abbildung unten):

![OMS – SF-Analyse im Marketplace](media/service-fabric-diagnostics-event-analysis-oms/service-fabric-analytics.png)

Wenn Sie auf **Erstellen** klicken, werden Sie nach einem OMS-Arbeitsbereich gefragt. Klicken Sie auf **Arbeitsbereich auswählen** und dann auf **Neuen Arbeitsbereich erstellen**. Nehmen Sie die erforderlichen Eingaben vor. Einzige Anforderung hier ist, dass für den Service Fabric-Cluster und den OMS-Arbeitsbereich dasselbe Abonnement verwendet werden muss. Nachdem die Eingaben überprüft wurden, wird Ihr OMS-Arbeitsbereich in wenigen Minuten bereitgestellt. Die Bereitstellung wird abgeschlossen, die Erstellung des Blatts für die Service Fabric-Lösung steht jedoch noch aus. Stellen Sie sicher, dass unter *OMS-Arbeitsbereich* derselbe Arbeitsbereich angezeigt wird, und klicken Sie unten auf **Erstellen**, um dem Arbeitsbereich die Service Fabric-Lösung hinzuzufügen.

## <a name="using-the-oms-agent"></a>Verwenden des OMS-Agents

Es wird empfohlen, EventFlow und WAD als Aggregationslösungen zu verwenden, da diese einen modularen Ansatz für die Diagnose und Überwachung bieten. Wenn Sie beispielsweise die Ausgaben von EventFlow ändern möchten, ist keine Änderung an der eigentlichen Instrumentierung, sondern lediglich eine einfache Änderung an der Konfigurationsdatei erforderlich. Wenn Sie sich jedoch für die Verwendung der OMS entscheiden und sie auch für die Ereignisanalyse verwenden möchten (sie muss nicht die einzige Plattform sein, die Sie verwenden, sondern mindestens eine der Plattformen sein), wird empfohlen, dass Sie sich mit der Einrichtung des [OMS-Agents](https://docs.microsoft.com/azure/log-analytics/log-analytics-windows-agents) vertraut machen.

Die dazu erforderlichen Schritte sind relativ einfach, da Sie den Agent lediglich als VM-Skalierungsgruppenerweiterung der Resource Manager-Vorlage hinzufügen und sicherstellen müssen, dass er auf einem Ihrer Knoten installiert wird. Eine Resource Manager-Beispielvorlage, über die der OMS-Arbeitsbereich mit der Service Fabric-Lösung bereitgestellt (wie oben erläutert) und der Agent den Knoten hinzugefügt wird, finden Sie [hier](https://github.com/ChackDan/Service-Fabric/tree/master/ARM%20Templates/SF%20OMS%20Sample).

Dies bietet die folgenden Vorteile:

* Umfangreichere Daten für die Leistungsindikatoren und Metriken
* Einfache Konfiguration der vom Cluster gesammelten Daten und Änderungen ohne erneute Bereitstellung Ihrer Anwendungen oder des Clusters, da Änderungen an den Einstellungen des Agents über den OMS-Arbeitsbereich erfolgen können und der Agent einfach automatisch zurückgesetzt wird. Zum Konfigurieren des OMS-Agents für das Abrufen spezifischer Leistungsindikatoren wechseln Sie zu **Startseite des Arbeitsbereichs > Einstellungen > Daten > Windows-Leistungsindikatoren**, und wählen Sie die zu sammelnden Daten aus.
* Daten werden schneller angezeigt, da sie nicht gespeichert werden müssen, um dann von OMS und Log Analytics abgerufen zu werden.
* Die Überwachung von Containern ist sehr viel einfacher, da Docker-Protokolle (stdout, stderror) und Docker-Statistiken (Leistungsmetriken auf Container- und Knotenebene) abgerufen werden können.

Die Hauptüberlegung hierbei ist, dass es sich um einen Agent handelt und dieser daher zusammen mit allen Ihren Anwendungen im Cluster bereitgestellt wird, sodass dadurch die Leistung der Anwendungen im Cluster minimal beeinträchtigt wird.

## <a name="monitoring-containers"></a>Überwachen von Containern

Bei der Bereitstellung von Containern in einem Service Fabric-Cluster wird empfohlen, dass der Cluster mit dem OMS-Agent eingerichtet und Ihrem OMS-Arbeitsbereich die Container-Lösung hinzugefügt wird, um die Überwachung und Diagnose zu ermöglichen. Hier ein Beispiel für die Container-Lösung in einem Arbeitsbereich:

![Einfaches OMS-Dashboard](./media/service-fabric-diagnostics-event-analysis-oms/oms-containers-dashboard.png)

Der Agent ermöglicht das Sammeln verschiedener containerspezifischer Protokolle, die in der OMS abgefragt oder zur Visualisierung von Leistungsindikatoren verwendet werden können. Folgende Protokolltypen werden gesammelt:

* ContainerInventory: zeigt Informationen über Containerspeicherort, -name und -images an
* ContainerImageInventory: zeigt Informationen zu bereitgestellten Images an, einschließlich IDs und Größen
* ContainerLog: zeigt bestimmte Fehlerprotokolle, Docker-Protokolle (stdout usw.) und andere Einträge an
* ContainerServiceLog: zeigt die Docker-Daemon-Befehle an, die ausgeführt wurden
* Perf: zeigt Leistungsindikatoren einschließlich Container-CPU, Arbeitsspeicher, Netzwerkdatenverkehr, Datenträger-E/A und benutzerdefinierter Metriken von den Hostcomputern an

In diesem Artikel werden die erforderlichen Schritte zum Einrichten der Containerüberwachung für Ihren Cluster beschrieben. Weitere Informationen zur OMS-Containerlösung finden Sie in der [Dokumentation](../log-analytics/log-analytics-containers.md).

Stellen Sie zum Einrichten der Container-Lösung in Ihrem Arbeitsbereich sicher, dass der OMS-Agent auf den Knoten Ihres Clusters bereitgestellt ist. Führen Sie dazu die oben genannten Schritte aus. Nachdem der Cluster eingerichtet wurde, können Sie einen Container in ihm bereitstellen. Bedenken Sie, dass es bei der ersten Bereitstellung eines Containerimages in einem Cluster mehrere Minuten dauern kann, bis das Image heruntergeladen wurde. Dies hängt von der Größe des Images ab.

Suchen Sie im Azure Marketplace nach *Container*, und erstellen Sie eine Containerressource (in der Kategorie „Überwachung und Verwaltung“).

![Hinzufügen der Containerlösung](./media/service-fabric-diagnostics-event-analysis-oms/containers-solution.png)

Im Erstellungsschritt wird ein OMS-Arbeitsbereich angefordert. Wählen Sie den Arbeitsbereich aus, der mit der Bereitstellung oben erstellt wurde. Dieser Schritt fügt die Containerlösung Ihrem OMS-Arbeitsbereich hinzu. Sie wird dann automatisch durch den OMS-Agent erkannt, der von der Vorlage bereitgestellt wurde. Der Agent beginnt das Sammeln von Daten für Containerprozesse im Cluster, und nach weniger als 15 Minuten sollten in der Lösung Daten wie in der Abbildung des Dashboards oben angezeigt werden.


## <a name="next-steps"></a>Nächste Schritte

Zum Anpassen eines Arbeitsbereichs entsprechend Ihren Anforderungen können Sie die folgenden Tools und Optionen der OMS verwenden:

* Für lokale Cluster bietet die OMS ein Gateway (HTTP-Weiterleitungsproxy), über das Daten an die OMS gesendet werden können. Weitere Informationen dazu finden Sie unter [Verbinden von Computern ohne Internetzugriff mit der OMS über das OMS-Gateway](../log-analytics/log-analytics-oms-gateway.md).
* Konfigurieren Sie die OMS für die Einrichtung von [automatisierten Warnungen](../log-analytics/log-analytics-alerts.md) bei der Erkennung und Diagnose.
* Machen Sie sich mit den Features zur [Protokollsuche und -abfrage](../log-analytics/log-analytics-log-searches.md) in Log Analytics vertraut.
