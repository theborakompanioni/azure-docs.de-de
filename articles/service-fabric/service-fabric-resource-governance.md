---
title: "Azure Service Fabric-Ressourcenkontrolle für Container und Dienste | Microsoft-Dokumentation"
description: "Azure Service Fabric ermöglicht Ihnen, Ressourcenlimits für Dienste festzulegen, die innerhalb oder außerhalb von Containern ausgeführt werden."
services: service-fabric
documentationcenter: .net
author: mani-ramaswamy
manager: timlt
editor: 
ms.assetid: ab49c4b9-74a8-4907-b75b-8d2ee84c6d90
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 8/9/2017
ms.author: subramar
ms.translationtype: Human Translation
ms.sourcegitcommit: 71fea4a41b2e3a60f2f610609a14372e678b7ec4
ms.openlocfilehash: e490dfba28b8a270bf0e6022333f148c965bc6dc
ms.contentlocale: de-de
ms.lasthandoff: 05/10/2017

---

# <a name="resource-governance"></a>Ressourcenkontrolle 

Werden mehrere Dienste auf dem gleichen Knoten oder Cluster ausgeführt, kann ein Dienst mehr Ressourcen belegen und dadurch die Verfügbarkeit anderer Dienste beeinträchtigen. Dieses Problem wird als „Noisy-Neighbor“-Problem bezeichnet. In Service Fabric können Entwickler Reservierungen und Grenzwerte pro Dienst festlegen, um Ressourcen zu garantieren und die Ressourcennutzung zu beschränken. 

## <a name="resource-governance-metrics"></a>Ressourcenkontrollmetriken 

Die Ressourcenkontrolle wird in Service Fabric pro [Dienstpaket](service-fabric-application-model.md) unterstützt. Die Ressourcen, die einem Dienstpaket zugewiesen sind, können weiter auf Codepakete unterteilt werden. Die festgelegten Ressourcenlimits haben auch die Reservierung der Ressourcen zur Folge. Service Fabric unterstützt das Festlegen von CPU und Arbeitsspeicher pro Dienstpaket mithilfe von zwei integrierten [Metriken](service-fabric-cluster-resource-manager-metrics.md):

* CPU (Metrikname `ServiceFabric:/_CpuCores`): Ein Kern ist ein logischer Kern, der auf dem Hostcomputer verfügbar ist, und alle Kerne in allen Knoten werden identisch gewichtet.
* Arbeitsspeicher (Metrikname `ServiceFabric:/_MemoryInMB`): Arbeitsspeicher wird in MB angegeben und entspricht dem auf dem Computer verfügbaren physischen Arbeitsspeicher.

Es werden nur weiche Reservierungsgarantien bereitgestellt: die Laufzeit lehnt das Öffnen neuer Dienstpakete ab, wenn die verfügbaren Ressourcen überschritten werden. Wenn jedoch eine andere ausführbare Datei oder ein anderer Container auf dem Knoten platziert wird, kann dies die ursprünglichen Reservierungsgarantien verletzen.

Für diese beiden Metriken werden vom [Clusterressourcen-Manager](service-fabric-cluster-resource-manager-cluster-description.md) die gesamte Clusterkapazität, die Last auf jedem Knoten im Cluster und die verbleibenden Ressourcen im Cluster nachverfolgt. Diese beiden Metriken entsprechen beliebigen anderen Benutzer- oder benutzerdefinierten Metriken, und alle vorhandenen Features können damit genutzt werden:
* Der Cluster kann gemäß diesen beiden Metriken [ausbalanciert](service-fabric-cluster-resource-manager-balancing.md) werden (Standardverhalten).
* Der Cluster kann gemäß diesen beiden Metriken [defragmentiert](service-fabric-cluster-resource-manager-defragmentation-metrics.md) werden.
* Beim [Beschreiben eines Clusters](service-fabric-cluster-resource-manager-cluster-description.md) kann für diese beiden Metriken gepufferte Kapazität festgelegt werden.

[Dynamische Auslastungsberichte](service-fabric-cluster-resource-manager-metrics.md) werden für diese Metriken nicht unterstützt, und Lasten für diese Metriken werden zum Zeitpunkt der Erstellung definiert.

## <a name="cluster-set-up-for-enabling-resource-governance"></a>Clustereinrichtung zum Aktivieren der Ressourcenkontrolle

Die Kapazität sollte auf jedem Knotentyp im Cluster wie folgt manuell definiert werden:

```xml
    <NodeType Name="MyNodeType">
      <Capacities>
        <Capacity Name="ServiceFabric:/_CpuCores" Value="4"/>
        <Capacity Name="ServiceFabric:/_MemoryInMB" Value="2048"/>
      </Capacities>
    </NodeType>
```
 
Die Ressourcenkontrolle ist nur für Benutzerdienste und nicht für Systemdienste zulässig. Beim Festlegen einer Kapazität dürfen bestimmte Kerne und Arbeitsspeicher nicht zugeordnet werden und müssen für Systemdienste verfügbar bleiben. Für eine optimale Leistung sollte auch die folgende Einstellung im Clustermanifest aktiviert werden: 

```xml
<Section Name="PlacementAndLoadBalancing">
    <Parameter Name="PreventTransientOvercommit" Value="true" /> 
    <Parameter Name="AllowConstraintCheckFixesDuringApplicationUpgrade" Value="true" />
</Section>
```


## <a name="specifying-resource-governance"></a>Festlegen der Ressourcenkontrolle 

Ressourcenkontrollgrenzwerte werden im Anwendungsmanifest (Abschnitt „ServiceManifestImport“) festgelegt, wie im folgenden Beispiel gezeigt:

```xml
<?xml version='1.0' encoding='UTF-8'?>
<ApplicationManifest ApplicationTypeName='TestAppTC1' ApplicationTypeVersion='vTC1' xsi:schemaLocation='http://schemas.microsoft.com/2011/01/fabric ServiceFabricServiceModel.xsd' xmlns='http://schemas.microsoft.com/2011/01/fabric' xmlns:xsi='http://www.w3.org/2001/XMLSchema-instance'>
  <Parameters>
  </Parameters>
  <!--
  ServicePackageA has the number of CPU cores defined, but doesn't have the MemoryInMB defined.
  In this case, Service Fabric will sum the limits on code packages and uses the sum as 
  the overall ServicePackage limit.
  -->
  <ServiceManifestImport>
    <ServiceManifestRef ServiceManifestName='ServicePackageA' ServiceManifestVersion='v1'/>
    <Policies>
      <ServicePackageResourceGovernancePolicy CpuCores="1"/>
      <ResourceGovernancePolicy CodePackageRef="CodeA1" CpuShares="512" MemoryInMB="1000" />
      <ResourceGovernancePolicy CodePackageRef="CodeA2" CpuShares="256" MemoryInMB="1000" />
    </Policies>
  </ServiceManifestImport>
```
  
In diesem Beispiel erhält das Dienstpaket ServicePackageA einen Kern auf den Knoten, iauf denen es platziert wurde. Dieses Dienstpaket enthält zwei Codepakete („CodeA1“ und „CodeA2“), und in beiden ist der Parameter `CpuShares` festgelegt. Der Anteil von CpuShares 512:256 teilt den Kern auf die zwei Codepakete auf. Daher erhält „CodeA1“ in diesem Beispiel zwei Drittel eines Kerns, und „CodeA2“ erhält ein Drittel eines Kerns (sowie eine weiche Reservierungsgarantie desselben). Für den Fall, dass für Codepakete keine CpuShares angegeben sind, teilt Service Fabric die Kerne gleichmäßig auf die Codepakete auf.

Die Arbeitsspeicherlimits sind absolut, daher sind beide Codepakete auf 1024 MB Arbeitsspeicher (sowie eine weiche Reservierungsgarantie desselben) beschränkt. Codepakete (Container oder Prozesse) können nicht mehr Arbeitsspeicher zuzuweisen, als dieser Grenzwert zulässt, ein entsprechender Versuch führt daher zu einer Ausnahme „Nicht genügend Arbeitsspeicher“. Damit die Erzwingung des Ressourcenlimits funktioniert, sollten für alle Codepakete innerhalb eines Dienstpakets Arbeitsspeicherlimits festgelegt sein.


## <a name="next-steps"></a>Nächste Schritte
* Weitere Informationen zum Clusterressourcen-Manager finden Sie in diesem [Artikel](service-fabric-cluster-resource-manager-introduction.md).
* Weitere Informationen zu Anwendungsmodell, Dienstpaketen, Codepaketen und dazu, wie diesen Replikate zugewiesen werden, finden Sie in diesem [Artikel](service-fabric-application-model.md).

