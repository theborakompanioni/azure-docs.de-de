---
title: "Clusterressourcen-Manager von Service Fabric – Platzierungsrichtlinien | Microsoft Docs"
description: "Übersicht über zusätzlichen Platzierungsrichtlinien und -regeln für Service Fabric-Dienste"
services: service-fabric
documentationcenter: .net
author: masnider
manager: timlt
editor: 
ms.assetid: 5c2d19c6-dd40-4c4b-abd3-5c5ec0abed38
ms.service: Service-Fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 01/05/2017
ms.author: masnider
translationtype: Human Translation
ms.sourcegitcommit: dafaf29b6827a6f1c043af3d6bfe62d480d31ad5
ms.openlocfilehash: 7fa35de8b99132ad1c10229a9bb2231f11fdbaa0


---
# <a name="placement-policies-for-service-fabric-services"></a>Platzierungsrichtlinien für Service Fabric-Dienste
Es gibt viele verschiedene weitere Regeln, die Sie möglicherweise in einigen seltenen Szenarien konfigurieren müssen. Einige Beispiele für diese Szenarien:
* Ihr Service Fabric-Cluster erstreckt sich über eine geografische Entfernung, beispielsweise mehrere lokale Rechenzentren oder Azure-Regionen.
* Ihre Umgebung erstreckt sich über mehrere geopolitische Grenzen (oder es bestehen rechtliche oder politische Grenzen, die für Sie relevant sind).
* Es sind praktische Aspekte hinsichtlich Leistung/Wartezeit zu beachten, weil bei der Kommunikation im Cluster weite Entfernungen zurückgelegt bzw. langsame oder weniger zuverlässige Netzwerke durchquert werden müssen.

In diesen Situationen ist es möglicherweise für einen bestimmten Dienst entscheidend, dass er in bestimmten Regionen immer ausgeführt werden muss oder niemals ausgeführt werden darf. Auf ähnliche Weise kann es wichtig sein, das primäre Replikat in einer bestimmten Region zu platzieren, um die Wartezeit für Endbenutzer auf ein Mindestmaß zu beschränken.

Die erweiterten Platzierungsrichtlinien sind:

1. Ungültige Domänen
2. Erforderliche Domänen
3. Bevorzugte Domänen
4. Unterbinden einer Replikatüberlastung

Die meisten der folgenden Steuerelemente können mithilfe von Knoteneigenschaften und Platzierungseinschränkungen konfiguriert werden, aber einige sind komplizierter. Zur Vereinfachung bietet der Service Fabric-Clusterressourcen-Manager diese zusätzliche Platzierungsrichtlinien. Wie bei anderen Platzierungseinschränkungen können Platzierungsrichtlinien auf Dienstinstanzbasis namensbezogen konfiguriert und dynamisch aktualisiert werden.

## <a name="specifying-invalid-domains"></a>Angeben ungültiger Domänen
Die Platzierungsrichtlinie „InvalidDomain“ ermöglicht die Angabe, dass eine bestimmte Fehlerdomäne für diese Workload ungültig ist. Diese Richtlinie stellt sicher, dass ein bestimmter Dienst nie in einer bestimmten Region ausgeführt wird, z. B. aus geopolitischen Gründen oder zur Einhaltung von Unternehmensrichtlinien. Über Richtlinien können mehrere ungültige Domänen angegeben werden.

<center>
![Beispiel für eine ungültige Domäne][Image1]
</center>

Code:

```csharp
ServicePlacementInvalidDomainPolicyDescription invalidDomain = new ServicePlacementInvalidDomainPolicyDescription();
invalidDomain.DomainName = "fd:/DCEast"; //regulations prohibit this workload here
serviceDescription.PlacementPolicies.Add(invalidDomain);
```

Powershell:

```posh
New-ServiceFabricService -ApplicationName $applicationName -ServiceName $serviceName -ServiceTypeName $serviceTypeName –Stateful -MinReplicaSetSize 2 -TargetReplicaSetSize 3 -PartitionSchemeSingleton -PlacementPolicy @("InvalidDomain,fd:/DCEast”)
```
## <a name="specifying-required-domains"></a>Angeben erforderlicher Domänen
Die Platzierungsrichtlinie für erforderliche Domänen erfordert, dass alle zustandsbehafteten Replikate oder zustandslosen Dienstinstanzen für den Dienst in der angegebenen Domäne vorhanden sind. Über Richtlinien können mehrere erforderliche Domänen angegeben werden.

<center>
![Beispiel für eine erforderliche Domäne][Image2]
</center>

Code:

```csharp
ServicePlacementRequiredDomainPolicyDescription requiredDomain = new ServicePlacementRequiredDomainPolicyDescription();
requiredDomain.DomainName = "fd:/DC01/RK03/BL2";
serviceDescription.PlacementPolicies.Add(requiredDomain);
```

Powershell:

```posh
New-ServiceFabricService -ApplicationName $applicationName -ServiceName $serviceName -ServiceTypeName $serviceTypeName –Stateful -MinReplicaSetSize 2 -TargetReplicaSetSize 3 -PartitionSchemeSingleton -PlacementPolicy @("RequiredDomain,fd:/DC01/RK03/BL2")
```

## <a name="specifying-a-preferred-domain-for-the-primary-replicas"></a>Angeben einer bevorzugten Domäne für die primären Replikate
Die bevorzugte Domäne für primäre Replikate ist ein nützliches Steuerelement, da es die Auswahl der Fehlerdomäne ermöglicht, in der das primäre Replikat nach Möglichkeit platziert werden soll. Wenn alles fehlerfrei ist, befindet sich das primäre Replikat in dieser Domäne. Sollte die Domäne oder das primäre Replikat ausfallen oder aus irgendeinem Grund heruntergefahren werden, wird das primäre Replikat zu einem anderen Ort migriert. Wenn dieser neue Ort nicht die bevorzugte Domäne ist, verschiebt der Clusterressourcen-Manager das Replikat so bald wie möglich in die bevorzugte Domäne zurück. Diese Einstellung ist nur für zustandsbehaftete Dienste sinnvoll. Diese Richtlinie ist besonders hilfreich in Clustern, die sich über Azure-Regionen oder mehrere Rechenzentren erstrecken, in denen die primären Replikate jedoch bevorzugt an einem bestimmten Ort platziert werden sollen. Durch das Platzieren der primären Replikate in der Nähe ihrer Benutzer wird vor allem für Lesevorgänge eine geringere Latenz erzielt.

<center>
![Bevorzugte primäre Domänen und Failover][Image3]
</center>

```csharp
ServicePlacementPreferPrimaryDomainPolicyDescription primaryDomain = new ServicePlacementPreferPrimaryDomainPolicyDescription();
primaryDomain.DomainName = "fd:/EastUS/";
serviceDescription.PlacementPolicies.Add(invalidDomain);
```

Powershell:

```posh
New-ServiceFabricService -ApplicationName $applicationName -ServiceName $serviceName -ServiceTypeName $serviceTypeName –Stateful -MinReplicaSetSize 2 -TargetReplicaSetSize 3 -PartitionSchemeSingleton -PlacementPolicy @("PreferredPrimaryDomain,fd:/EastUS")
```

## <a name="requiring-replicas-to-be-distributed-among-all-domains-and-disallowing-packing"></a>Anfordern einer Verteilung von Replikaten auf alle Domänen und Unterbinden einer Überlastung
Replikate werden _normalerweise_ über die Domänen verteilt, wenn der Cluster fehlerfrei ist, aber es gibt Fälle, in denen Replikate für eine bestimmte Partition vorübergehend in einer einzigen Domäne gruppiert werden. Nehmen wir beispielsweise an, dass der Cluster neun Knoten in drei Fehlerdomänen aufweist (fd:/0, fd:/1 und fd:/2) und dass der Dienst über drei Replikate verfügt. Nehmen wir ferner an, dass die Knoten, die für diese Replikate in fd:/1 und fd:/2 verwendet wurden, ausgefallen sind. Jetzt würde der Clusterressourcen-Manager normalerweise andere Knoten in diesen Fehlerdomänen bevorzugen. In diesem Fall ist jedoch, zum Beispiel aufgrund von Kapazitätsproblemen, keiner der anderen Knoten in diesen Domänen gültig. Wenn der Clusterressourcen-Manager einen Ersatz für diese Replikate erstellt, müsste er die Knoten in fd:/0 auswählen. Allerdings entsteht _dadurch_ eine Situation, in der die Fehlerdomäneneinschränkung verletzt wird. Außerdem steigt das Risiko eines Ausfalls oder Verlusts der gesamten Replikatgruppe (wenn Fehlerdomäne 0 dauerhaft ausfällt). Weitere Informationen zu Einschränkungen und Einschränkungsprioritäten im Allgemeinen finden Sie in [diesem Thema](service-fabric-cluster-resource-manager-management-integration.md#constraint-priorities).

Wenn Sie jemals eine Warnung wie die Folgende gesehen haben: `The Load Balancer has detected a Constraint Violation for this Replica:fabric:/<some service name> Secondary Partition <some partition ID> is violating the Constraint: FaultDomain`, ist diese oder eine ähnliche Bedingung erfüllt. Dieser Fall ist selten, kann aber auftreten. Dieser Zustand ist in der Regel vorübergehend, weil die Knoten wieder online gehen. Wenn die Knoten dauerhaft ausfallen und der Clusterressourcen-Manager diese ersetzen muss, stehen in idealen Fehlerdomänen normalerweise andere Knoten zur Verfügung.

Einige Workloads sorgen stattdessen immer dafür, dass die Zielanzahl von Replikaten verfügbar ist, selbst wenn diese auf weniger Domänen verteilt sind. Durch diese Workloads werden gleichzeitige dauerhafte Totalausfälle von Domänen riskiert, und in der Regel kann der lokale Zustand wiederhergestellt werden. Bei anderen Workloads wird stattdessen früher die Downtime in Kauf genommen, als die Richtigkeit oder den Verlust von Daten zu riskieren. Da die meisten Produktionsworkloads mit mehr als drei Replikaten, mehr als drei Fehlerdomänen und vielen gültigen Knoten pro Fehlerdomäne ausgeführt werden, ist standardmäßig keine Domänenverteilung erforderlich. Dadurch können diese Fälle durch normalen Lastenausgleich und Failover abgedeckt werden, selbst wenn dies bedeutet, dass möglicherweise mehrere Replikate vorübergehend in einer Domäne platziert werden.

Wenn Sie eine solche Überlastung für eine bestimmte Workload deaktivieren möchten, können Sie die Richtlinie „RequireDomainDistribution“ für den Dienst angeben. Wenn diese Richtlinie festgelegt ist, stellt der Clusterressourcen-Manager sicher, dass keine zwei Replikate aus derselben Partition in derselben Fehler- oder Upgradedomäne platziert werden.

Code:

```csharp
ServicePlacementRequireDomainDistributionPolicyDescription distributeDomain = new ServicePlacementRequireDomainDistributionPolicyDescription();
serviceDescription.PlacementPolicies.Add(distributeDomain);
```

Powershell:

```posh
New-ServiceFabricService -ApplicationName $applicationName -ServiceName $serviceName -ServiceTypeName $serviceTypeName –Stateful -MinReplicaSetSize 2 -TargetReplicaSetSize 3 -PartitionSchemeSingleton -PlacementPolicy @("RequiredDomainDistribution")
```

Wäre es auch möglich, diese Konfigurationen für Dienste in einem Cluster zu verwenden, der sich nicht über größere Entfernungen erstreckt? Das ist möglich! Es gibt dafür jedoch keinen triftigen Grund. Insbesondere die Konfigurationen erforderlicher, ungültiger und bevorzugter Domänen sollten vermieden werden, solange Sie keinen Cluster betreiben, der sich über weite Entfernungen erstreckt. Es ist nicht sinnvoll, die Ausführung einer bestimmten Workload in einem einzigen Rack zu erzwingen oder ein bestimmtes Segment Ihres Clusters einem anderen gegenüber zu bevorzugen. Unterschiedliche Hardwarekonfigurationen sollten über Domänen hinweg verteilt werden, und diese sollten über normale Platzierungseinschränkungen und Knoteneigenschaften verwaltet werden.

## <a name="next-steps"></a>Nächste Schritte
* Weitere Informationen zu den anderen verfügbaren Optionen zum Konfigurieren von Diensten finden Sie unter [Informationen zur Konfiguration von Diensten](service-fabric-cluster-resource-manager-configure-services.md).

[Image1]:./media/service-fabric-cluster-resource-manager-advanced-placement-rules-placement-policies/cluster-invalid-placement-domain.png
[Image2]:./media/service-fabric-cluster-resource-manager-advanced-placement-rules-placement-policies/cluster-required-placement-domain.png
[Image3]:./media/service-fabric-cluster-resource-manager-advanced-placement-rules-placement-policies/cluster-preferred-primary-domain.png



<!--HONumber=Jan17_HO1-->


