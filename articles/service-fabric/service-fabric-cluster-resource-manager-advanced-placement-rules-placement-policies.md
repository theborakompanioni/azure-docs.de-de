<properties
   pageTitle="Der Clusterressourcen-Manager von Service Fabric – Platzierungsrichtlinien | Microsoft Azure"
   description="Übersicht über zusätzlichen Platzierungsrichtlinien und -regeln für Service Fabric-Dienste"
   services="service-fabric"
   documentationCenter=".net"
   authors="masnider"
   manager="timlt"
   editor=""/>

<tags
   ms.service="Service-Fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="08/19/2016"
   ms.author="masnider"/>

# Platzierungsrichtlinien für Service Fabric-Dienste
Es gibt viele weitere Regeln, die ggf. beachtet werden müssen, wenn sich Ihr Service Fabric-Cluster über größere Entfernungen erstreckt, z.B. über mehrere Rechenzentren oder Azure-Regionen, oder wenn sich Ihre Umgebung über mehrere geopolitische Kontrollbereiche erstreckt (oder eine andere Situation, in der rechtliche oder strategische Grenzen beachtet werden müssen oder die gegebenen Entfernungen tatsächlich Einfluss auf die Leistung/Latenz haben). Die meisten dieser Fälle können mithilfe von Knoteneigenschaften und Platzierungseinschränkungen konfiguriert werden, aber einige sind komplizierter. Zum Vereinfachen der Abläufe bieten wir diese zusätzlichen Befehle. Wie bei anderen Platzierungseinschränkungen können Platzierungsrichtlinien auf Dienstinstanzbasis namensbezogen konfiguriert werden.

## Angeben ungültiger Domänen
Die Platzierungsrichtlinie „InvalidDomain“ ermöglicht die Angabe, dass eine bestimmte Fehlerdomäne für diese Workload ungültig ist. Diese Richtlinie stellt sicher, dass ein bestimmter Dienst nie in einer bestimmten Region ausgeführt wird, z. B. aus geopolitischen Gründen oder zur Einhaltung von Unternehmensrichtlinien. Über Richtlinien können mehrere ungültige Domänen angegeben werden.

![Ungültige Domäne – Beispiel][Image1]

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
## Angeben erforderlicher Domänen
Die Platzierungsrichtlinie für erforderliche Domänen erfordert, dass alle zustandsbehafteten Replikate oder zustandslosen Dienstinstanzen für den Dienst in der angegebenen Domäne vorhanden sind. Über Richtlinien können mehrere erforderliche Domänen angegeben werden.

![Erforderliche Domäne – Beispiel][Image2]

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

## Angeben einer bevorzugten Domäne für die primären Replikate
Die bevorzugte Domäne für primäre Replikate ist ein nützliches Steuerelement, da es die Auswahl der Fehlerdomäne ermöglicht, in der das primäre Replikat nach Möglichkeit platziert werden soll. Wenn alles fehlerfrei ist, befindet sich das primäre Replikat in dieser Domäne. Sollten die Domäne oder das primäre Replikat ausfallen oder aus einem anderen Grund heruntergefahren werden, wird das primäre Replikat zu einem anderen Ort migriert. Wenn dieser Speicherort nicht die bevorzugte Domäne ist, verschiebt der Clusterressourcen-Manager das Replikat nach Möglichkeit erneut in die bevorzugte Domäne. Diese Einstellung ist nur für zustandsbehaftete Dienste sinnvoll. Diese Richtlinie ist besonders nützlich in Clustern, die sich über mehrere Azure-Regionen oder Rechenzentren erstrecken. In diesen Situationen verwenden Sie zu Redundanzzwecken alle Speicherorte, doch die primären Replikate sollen vorzugsweise an einem bestimmten Standort platziert werden, um bei Vorgängen an primäre Replikate niedrigere Latenzzeiten zu erzielen (Schreibvorgänge und standardmäßig auch alle Lesevorgänge werden vom primären Replikat verarbeitet).

![Bevorzugte primäre Domänen und Failover][Image3]

```csharp
ServicePlacementPreferPrimaryDomainPolicyDescription primaryDomain = new ServicePlacementPreferPrimaryDomainPolicyDescription();
primaryDomain.DomainName = "fd:/EastUS/";
serviceDescription.PlacementPolicies.Add(invalidDomain);
```

Powershell:

```posh
New-ServiceFabricService -ApplicationName $applicationName -ServiceName $serviceName -ServiceTypeName $serviceTypeName –Stateful -MinReplicaSetSize 2 -TargetReplicaSetSize 3 -PartitionSchemeSingleton -PlacementPolicy @("PreferredPrimaryDomain,fd:/EastUS")
```

## Anfordern einer Verteilung von Replikaten auf alle Domänen und Unterbinden einer Überlastung
Eine weitere Richtlinie, die Sie angeben können, legt fest, dass Replikate immer auf die verfügbaren Fehlerdomänen verteilt werden. Diese Vorgehensweise wird standardmäßig in den meisten Fällen angewendet, wenn der Cluster fehlerfrei ausgeführt wird. In Situationen, in denen eine ordnungsgemäße Funktionsweise nicht sichergestellt ist, werden die Replikate für eine bestimmte Partition jedoch möglicherweise vorübergehend in einer einzigen Fehler- oder Upgradedomäne platziert. Beispiel: Der Cluster verfügt über 9 Knoten in 3 Fehlerdomänen (0, 1 und 2), und Ihr Dienst verfügt über 3 Replikate. Die Knoten, die für diese Replikate in Fehlerdomäne 1 und 2 verwendet wurden, sind ausgefallen. Aufgrund von Kapazitätsproblemen konnte kein anderer Knoten in diesen Domänen verwendet werden. Wenn Service Fabric Ersatzreplikate erstellen würde, würden diese vom Clusterressourcen-Manager in Fehlerdomäne 0 platziert. Dadurch entsteht jedoch eine Situation, durch die die Einschränkung für Fehlerdomänen verletzt wird. Außerdem steigt das Risiko eines Verlusts der gesamten Replikatgruppe (wenn Fehlerdomäne 0 dauerhaft ausfällt). (Weitere Informationen zu Einschränkungen und Einschränkungsprioritäten im Allgemeinen finden Sie in [diesem Thema](service-fabric-cluster-resource-manager-management-integration.md#constraint-priorities). )

Wenn eine Warnung zur Integrität wie „Der Load Balancer hat für dieses Replikat eine Einschränkungsverletzung festgestellt:Fabric:/<Dienstname> Sekundäre Partition <Partitions-ID> verletzt die Einschränkung: Fehlerdomäne“ ist diese oder eine ähnliche Bedingung erfüllt. Diese Situationen treten üblicherweise vorübergehend ein (die Knoten fallen nur für kurze Zeit aus, oder in den richtigen Fehlerdomänen sind andere gültige Knoten vorhanden, wenn bei einem längeren Ausfall Ersatzreplikate erstellt werden müssen), bei einigen Workloads ist es jedoch sinnvoll, eher Einbußen bei der Verfügbarkeit als den Verlust aller Replikate in Kauf zu nehmen. Zu diesem Zweck kann die Richtlinie RequireDomainDistribution angegeben werden, mit der sichergestellt wird, dass zu keinem Zeitpunkt zwei Replikate derselben Partition in derselben Fehler- oder Upgradedomäne platziert werden.

Bei einigen Workloads kann es sinnvoller sein, immer über die gewünschte Anzahl von Replikaten (Kopien des Zustands) zu verfügen (und nicht von einem vollständigen Ausfall der Domänen auszugeben bzw. zu wissen, dass der lokale Zustand üblicherweise wiederhergestellt werden kann), während bei anderen Workloads eher eine Downtime in Kauf genommen wird als das Risiko von fehlerhaften Daten oder Datenverlust. Da die meisten Produktionsworkloads mit mehr als 3 Replikaten arbeiten, wird standardmäßig keine Domänenverteilung erzwungen. Stattdessen werden Lastenausgleich und Failover normal ausgeführt, auch wenn dies bedeutet, dass in einer Domäne vorübergehend mehrere Replikate platziert werden.

Code:

```csharp
ServicePlacementRequireDomainDistributionPolicyDescription distributeDomain = new ServicePlacementRequireDomainDistributionPolicyDescription();
serviceDescription.PlacementPolicies.Add(distributeDomain);
```

Powershell:

```posh
New-ServiceFabricService -ApplicationName $applicationName -ServiceName $serviceName -ServiceTypeName $serviceTypeName –Stateful -MinReplicaSetSize 2 -TargetReplicaSetSize 3 -PartitionSchemeSingleton -PlacementPolicy @("RequiredDomainDistribution")
```

Ist es auch möglich, diese Konfigurationen für Dienste in einem Cluster zu verwenden, der sich nicht über größere Entfernungen erstreckt? Das ist möglich! Diese Konfiguration ist jedoch nicht erforderlich. Insbesondere die Konfigurationen für erforderliche, ungültige und bevorzugte Domänen sollten nur dann verwendet werden, wenn Ihr Cluster sich tatsächlich auf mehrere geografische Regionen erstreckt. Es ist nicht sinnvoll, die Ausführung einer bestimmten Workload in einem einzigen Rack zu erzwingen oder ein bestimmtes Segment Ihres lokalen Clusters einem anderen vorzuziehen, sofern nicht unterschiedliche Hardwaretypen verwendet oder eine Workloadsegmentierung vorgenommen wird. Und in diesen Fällen können die normalen Platzierungseinschränkungen verwendet werden.

## Nächste Schritte
- Informationen zu weiteren Optionen, die für die Konfiguration von Diensten zur Verfügung stehen, finden Sie im Thema zu den weiteren verfügbaren Clusterressourcen-Manager-Konfigurationen: [Konfigurieren von Diensten](service-fabric-cluster-resource-manager-configure-services.md)

[Image1]: ./media/service-fabric-cluster-resource-manager-advanced-placement-rules-placement-policies/cluster-invalid-placement-domain.png
[Image2]: ./media/service-fabric-cluster-resource-manager-advanced-placement-rules-placement-policies/cluster-required-placement-domain.png
[Image3]: ./media/service-fabric-cluster-resource-manager-advanced-placement-rules-placement-policies/cluster-preferred-primary-domain.png

<!---HONumber=AcomDC_0824_2016-->