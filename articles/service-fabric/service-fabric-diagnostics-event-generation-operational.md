---
title: "Betriebskanal für Azure Service Fabric | Microsoft-Dokumentation"
description: Eine umfassende Liste der im Betriebskanal von Azure Service Fabric-Clustern generierten Protokolle.
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
ms.date: 07/19/2017
ms.author: dekapur
ms.translationtype: HT
ms.sourcegitcommit: 0425da20f3f0abcfa3ed5c04cec32184210546bb
ms.openlocfilehash: 934719868ab9968db352db2b440014d35dbc0274
ms.contentlocale: de-de
ms.lasthandoff: 07/20/2017

---

# <a name="operational-channel"></a>Betriebskanal 

Der Betriebskanal besteht aus Protokollen aus Aktionen auf hoher Ebene, die von Service Fabric für Ihre Knoten und Ihren Cluster ausgeführt werden. Wenn für einen Cluster „Diagnose“ aktiviert ist, wird der Azure-Diagnose-Agent in Ihrem Cluster bereitgestellt und standardmäßig so konfiguriert, dass er Protokolle aus dem Betriebskanal einliest. Erfahren Sie mehr dazu, wie Sie den [Azure-Diagnose-Agent](service-fabric-diagnostics-event-aggregation-wad.md) so konfigurieren, dass er die Diagnosekonfiguration des Clusters ändert, um mehr Protokolle oder Leistungsindikatoren aufzunehmen. 

## <a name="operational-channel-logs"></a>Betriebskanalprotokolle 

Hier sehen Sie eine umfassende Liste der von Service Fabric im Betriebskanal bereitgestellten Protokolle: 

| EventId | Name | Quelle (Aufgabe) | Ebene |
| --- | --- | --- | --- |
| 25620 | NodeOpening | FabricNode | Information |
| 25621 | NodeOpenedSuccess | FabricNode | Information |
| 25622 | NodeOpenedFailed | FabricNode | Information |
| 25623 | NodeClosing | FabricNode | Information |
| 25624 | NodeClosed | FabricNode | Information |
| 25625 | NodeAborting | FabricNode | Information |
| 25626 | NodeAborted | FabricNode | Information |
| 29627 | ClusterUpgradeStart | CM | Information |
| 29628 | ClusterUpgradeComplete | CM | Information |
| 29629 | ClusterUpgradeRollback | CM | Information |
| 29630 | ClusterUpgradeRollbackComplete | CM | Information |
| 29631 | ClusterUpgradeDomainComplete | CM | Information |
| 23074 | ContainerActivated | Hosting | Information |
| 23075 | ContainerDeactivated | Hosting | Information |
| 29620 | ApplicationCreated | CM | Information |
| 29621 | ApplicationUpgradeStart | CM | Information |
| 29622 | ApplicationUpgradeComplete | CM | Information |
| 29623 | ApplicationUpgradeRollback | CM | Information |
| 29624 | ApplicationUpgradeRollbackComplete | CM | Information |
| 29625 | ApplicationDeleted | CM | Information |
| 29626 | ApplicationUpgradeDomainComplete | CM | Information |
| 18566 | ServiceCreated | FM | Information |
| 18567 | ServiceDeleted | FM | Information |

## <a name="next-steps"></a>Nächste Schritte

* Erfahren Sie mehr über die gesamte [Ereignisgenerierung auf Plattformebene](service-fabric-diagnostics-event-generation-infra.md) in Service Fabric.
* Ändern Ihrer [Azure-Diagnose](service-fabric-diagnostics-event-aggregation-wad.md)-Konfiguration zum Sammeln weiterer Protokolle
* [Einrichten von Application Insights](service-fabric-diagnostics-event-analysis-appinsights.md) zum Anzeigen der Betriebskanalprotokolle

