---
title: "Azure Service Fabric-Ressourcenkontrolle für Container und Dienste | Microsoft-Dokumentation"
description: "Azure Service Fabric ermöglicht Ihnen, Ressourcenlimits für Dienste festzulegen, die innerhalb oder außerhalb von Containern ausgeführt werden."
services: service-fabric
documentationcenter: .net
author: masnider
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
ms.translationtype: HT
ms.sourcegitcommit: 47ba7c7004ecf68f4a112ddf391eb645851ca1fb
ms.openlocfilehash: 62ce17533f8c195b873431089e1f1f47cb0bcbec
ms.contentlocale: de-de
ms.lasthandoff: 09/14/2017

---

# <a name="resource-governance"></a>Ressourcenkontrolle 

Werden mehrere Dienste auf dem gleichen Knoten oder Cluster ausgeführt, kann ein Dienst mehr Ressourcen belegen und dadurch die Verfügbarkeit anderer Dienste beeinträchtigen. Dieses Problem wird als „Noisy-Neighbor“-Problem bezeichnet. In Service Fabric können Entwickler Reservierungen und Grenzwerte pro Dienst festlegen, um Ressourcen zu garantieren und die Ressourcennutzung zu beschränken.

>
> Bevor Sie mit diesem Artikel fortfahren, sollten Sie sich mit dem [Service Fabric-Anwendungsmodell](service-fabric-application-model.md) und dem [Service Fabric-Hostingmodell](service-fabric-hosting-model.md) vertraut machen.
>

## <a name="resource-governance-metrics"></a>Ressourcenkontrollmetriken 

Die Ressourcenkontrolle wird in Service Fabric pro [Dienstpaket](service-fabric-application-model.md) unterstützt. Die Ressourcen, die einem Dienstpaket zugewiesen sind, können weiter auf Codepakete unterteilt werden. Die festgelegten Ressourcenlimits haben auch die Reservierung der Ressourcen zur Folge. Service Fabric unterstützt das Festlegen von CPU und Arbeitsspeicher pro Dienstpaket mithilfe von zwei integrierten [Metriken](service-fabric-cluster-resource-manager-metrics.md):

* CPU (Metrikname `servicefabric:/_CpuCores`): Ein Kern ist ein logischer Kern, der auf dem Hostcomputer verfügbar ist, und alle Kerne in allen Knoten werden identisch gewichtet.
* Arbeitsspeicher (Metrikname `servicefabric:/_MemoryInMB`): Arbeitsspeicher wird in MB angegeben und entspricht dem auf dem Computer verfügbaren physischen Arbeitsspeicher.

Für diese beiden Metriken werden vom [Clusterressourcen-Manager](service-fabric-cluster-resource-manager-cluster-description.md) die gesamte Clusterkapazität, die Last auf jedem Knoten im Cluster und die verbleibenden Ressourcen im Cluster nachverfolgt. Diese beiden Metriken entsprechen beliebigen anderen Benutzer- oder benutzerdefinierten Metriken, und alle vorhandenen Features können damit genutzt werden:
* Der Cluster kann gemäß diesen beiden Metriken [ausbalanciert](service-fabric-cluster-resource-manager-balancing.md) werden (Standardverhalten).
* Der Cluster kann gemäß diesen beiden Metriken [defragmentiert](service-fabric-cluster-resource-manager-defragmentation-metrics.md) werden.
* Beim [Beschreiben eines Clusters](service-fabric-cluster-resource-manager-cluster-description.md) kann für diese beiden Metriken gepufferte Kapazität festgelegt werden.

[Dynamische Auslastungsberichte](service-fabric-cluster-resource-manager-metrics.md) werden für diese Metriken nicht unterstützt, und Lasten für diese Metriken werden zum Zeitpunkt der Erstellung definiert.

## <a name="resource-governance-mechanism"></a>Ressourcenkontrollmechanismus

Die Service Fabric-Runtime ermöglicht derzeit keine Reservierungen für Ressourcen. Wenn ein Prozess oder ein Container geöffnet ist, legt die Runtime die Ressourcengrenzwerte auf die Lasten fest, die zum Zeitpunkt der Erstellung definiert wurden. Darüber hinaus verweigert die Runtime das Öffnen neuer Dienstpakete, wenn keine Ressourcen mehr verfügbar sind. Um die Funktionsweise dieses Prozesses besser zu verstehen, sehen Sie sich ein Beispiel für einen Knoten mit 2 CPU-Kernen an (der Mechanismus für die Speicherüberwachung ist identisch):

1. Zunächst wird ein Container auf dem Knoten platziert, der einen CPU-Kern anfordert. Die Runtime öffnet den Container und legt den CPU-Grenzwert auf einen Kern fest. Der Container kann maximal einen Kern verwenden.
2. Anschließend wird ein Replikat des Diensts im Knoten platziert, und das zugehörige Dienstpaket gibt als Grenzwert einen CPU-Kern an. Die Runtime öffnet das Codepaket und legt den CPU-Grenzwert auf einen Kern fest.

An diesem Punkt ist die Summe der Grenzwerte für die Kapazität des Knotens gleich, und ein Prozess und ein Container werden mit jeweils einem Kern ausgeführt, sodass sie sich nicht gegenseitig beeinträchtigen. Service Fabric platziert keine weiteren Container oder Replikate, wenn diese einen CPU-Grenzwert angeben. Es gibt jedoch zwei Situationen, in denen andere Prozesse CPU-Leistung anfordern, sodass der eine Prozess und der eine Container aus unserem Beispiel beeinträchtigt werden könnten („Noisy-Neighbor-Problem“):

* Kombinieren von gesteuerten und nicht gesteuerten Diensten und Containern: Wenn ein Benutzer einen Dienst erstellt, ohne eine Ressourcenkontrolle anzugeben, behandelt die Runtime diesen so, als würde er keine Ressourcen in Anspruch nehmen, und kann ihn in unserem Beispiel auf dem Knoten platzieren. In diesem Fall verbraucht dieser neue Prozess einen Teil der CPU-Leistung, der den bereits auf dem Knoten ausgeführten Diensten dann fehlt. Zum Beheben dieses Problems können Sie entweder vermeiden, nicht gesteuerte und gesteuerte Dienste im selben Cluster zu verwenden, oder Sie verwenden [Platzierungseinschränkungen](service-fabric-cluster-resource-manager-advanced-placement-rules-placement-policies.md), damit sie nicht im selben Knotensatz platziert werden.
* Für den Fall, dass ein anderer Prozess auf dem Knoten außerhalb von Service Fabric gestartet wird (z.B. ein OS-Dienst), konkurriert dieser Prozess auch mit den vorhandenen Diensten um CPU-Leistung. Zum Beheben dieses Problems können Sie Knotenkapazitäten einrichten, um den Mehraufwand des Betriebssystems abzufangen. Dieses Vorgehen wird im nächsten Abschnitt beschrieben.

## <a name="cluster-set-up-for-enabling-resource-governance"></a>Clustereinrichtung zum Aktivieren der Ressourcenkontrolle

Wenn der Knoten gestartet wird und dem Cluster beitritt, erkennt Service Fabric den verfügbaren Arbeitsspeicher und die verfügbare Anzahl an Kernen und legt die Knotenkapazitäten für diese beiden Ressourcen fest. Um etwas Puffer für das Betriebssystem und für andere Prozesse, die auf dem Knoten ausgeführt werden könnten, zu behalten, nutzt Service Fabric nur 80 % der verfügbaren Ressourcen auf dem Knoten. Dieser Prozentsatz ist konfigurierbar und kann im Clustermanifest geändert werden. Mit dem folgenden Beispiel weisen Sie Service Fabric an, 50 % der verfügbaren CPU-Leistung und 70 % des verfügbaren Arbeitsspeichers zu verwenden: 

```xml
<Section Name="PlacementAndLoadBalancing">
    <!-- 0.0 means 0%, and 1.0 means 100%-->
    <Parameter Name="CpuPercentageNodeCapacity" Value="0.5" />
    <Parameter Name="MemoryPercentageNodeCapacity" Value="0.7" />
</Section>
```

Falls eine vollständig manuelle Einrichtung der Knotenkapazitäten erforderlich ist, können auch reguläre Mechanismen zum Beschreiben der Knoten im Cluster angewendet werden. Im Folgenden finden Sie ein Beispiel für das Einrichten des Knotens mit vier Kernen und 2 GB Arbeitsspeicher: 

```xml
    <NodeType Name="MyNodeType">
      <Capacities>
        <Capacity Name="servicefabric:/_CpuCores" Value="4"/>
        <Capacity Name="servicefabric:/_MemoryInMB" Value="2048"/>
      </Capacities>
    </NodeType>
```

Wenn die automatische Erkennung der verfügbaren Ressourcen aktiviert ist und die Knotenkapazitäten manuell im Clustermanifest definiert werden, überprüft Service Fabric, ob der Knoten über genügend Ressourcen für die vom Benutzer definierten Kapazität verfügt:
* Wenn die im Manifest definierten Knotenkapazitäten geringer oder gleich den verfügbaren Ressourcen auf dem Knoten sind, verwendet Service Fabric die im Manifest angegebenen Kapazitäten.
* Wenn die im Manifest definierten Knotenkapazitäten größer als die verfügbaren Ressourcen sind, verwendet Service Fabric die verfügbaren Ressourcen als Knotenkapazitäten.

Die automatische Erkennung der verfügbaren Ressourcen kann vollständig deaktiviert werden, falls sie nicht erforderlich ist. Ändern Sie dazu die folgende Einstellung:

```xml
<Section Name="PlacementAndLoadBalancing">
    <Parameter Name="AutoDetectAvailableResources" Value="false" />
</Section>
```

Für eine optimale Leistung sollte auch die folgende Einstellung im Clustermanifest aktiviert werden: 

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

## <a name="other-resources-for-containers"></a>Weitere Ressourcen für Container
Neben CPU und Arbeitsspeicher ist es möglich, noch andere Ressourcengrenzwerte für Container anzugeben. Diese Grenzwerte werden auf Codepaketebene angegeben und angewendet, wenn der Container gestartet wird. Anders als bei CPU und Arbeitsspeicher sind diese Ressourcen im Cluster Resource Manager nicht bekannt, sodass dieser weder die Kapazität überprüft noch einen Lastenausgleich für sie ausführt. 

* MemorySwapInMB: die Menge an Auslagerungsspeicher, die ein Container nutzen kann
* MemoryReservationInMB: weicher Grenzwert für die Arbeitsspeicherüberwachung, die nur erzwungen wird, wenn auf dem Knoten Arbeitsspeicherkonflikte erkannt werden
* CpuPercent: CPU-Prozentsatz, den der Container verwenden kann. Wenn CPU-Grenzwerte für das Dienstpaket angegeben wurden, wird dieser Parameter ignoriert.
* MaximumIOps: maximale IOPS, die ein Container verwenden kann (lesen und schreiben)
* MaximumIOBytesps: maximale E/A (Byte/s), die ein Container verwenden kann (lesen und schreiben)
* BlockIOWeight: Block-E/A-Gewichtung relativ zu anderen Containern

Diese Ressourcen können mit CPU und Arbeitsspeicher kombiniert werden. Hier ist ein Beispiel dafür, wie zusätzliche Ressourcen für Container angegeben werden:

```xml
    <ServiceManifestImport>
        <ServiceManifestRef ServiceManifestName="FrontendServicePackage" ServiceManifestVersion="1.0"/>
        <Policies>
            <ResourceGovernancePolicy CodePackageRef="FrontendService.Code" CpuPercent="5"
            MemorySwapInMB="4084" MemoryReservationInMB="1024" MaximumIOPS="20" />
        </Policies>
    </ServiceManifestImport>
```

## <a name="next-steps"></a>Nächste Schritte
* Weitere Informationen zum Clusterressourcen-Manager finden Sie in diesem [Artikel](service-fabric-cluster-resource-manager-introduction.md).
* Weitere Informationen zu Anwendungsmodell, Dienstpaketen, Codepaketen und dazu, wie diesen Replikate zugewiesen werden, finden Sie in diesem [Artikel](service-fabric-application-model.md).

