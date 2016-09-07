<properties
   pageTitle="Drosselung im Clusterressourcen-Manager von Service Fabric | Microsoft Azure"
   description="Sie lernen, die Drosselungen zu konfigurieren, die der Clusterressourcen-Manager von Service Fabric ermöglicht."
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


# Drosselungen für das Verhalten des Clusterressourcen-Managers von Service Fabric
Auch wenn Sie den Clusterressourcen-Manager ordnungsgemäß konfiguriert haben, kann es für den Cluster zu Unterbrechungen kommen. Beispielsweise können Knoten oder Fehlerdomänen gleichzeitig ausfallen. Was wäre, wenn dies während eines Upgrades passieren würde? Der Clusterressourcen-Manager versucht, alle Fehler zu beheben. In diesen Fällen sollten Sie über den Einsatz eines Backstops nachdenken, sodass sich der Cluster selbst stabilisiert (die Knoten, die wieder verfügbar sein sollen, werden erneut bereitgestellt, die Netzwerkbedingungen korrigieren sich von selbst und die korrigierten Bits werden bereitgestellt). Um Sie in Situationen wie diesen zu unterstützen, bietet der Clusterressourcen-Manager von Service Fabric mehrere Drosselungen an. Beachten Sie, dass diese Drosselungen erhebliche Unterbrechungen zur Folge haben. Sie sollten nicht verwendet werden, solange nicht berechnet wurde, wie viel Vorgänge der Cluster parallel tatsächlich ausführen kann. Zudem sollte häufig die Notwendigkeit bestehen, auf diese ungeplanten, makroskopischen Ereignisse (auch „sehr schlechte Tage“ genannt) zu reagieren, die eine Neukonfiguration erforderlich machen.

Wie empfehlen gemeinhin, „sehr schlechte Tage“ mithilfe anderer Optionen zu vermeiden (z. B. durch reguläre Codeaktualisierungen oder generell durch das Vermeiden einer Überlastung des Clusters), statt den Cluster zu drosseln und daran zu hindern, Ressourcen zu nutzen, während er gleichzeitig versucht, sich selbst zu reparieren. Die Drosselungen verfügen über Standardwerte, die basierend auf unseren Erfahrungen festgelegt wurden. Dennoch sollten Sie diese Werte überprüfen und an Ihre erwartete tatsächliche Last anpassen. Wenngleich allgemein empfohlen wird, für einen Cluster keine zu strikten Einschränkungen festzulegen und keine zu große Last zuzuweisen, können Drosselungen in bestimmten Situationen sinnvoll sein (bis Sie die problematische Situation beheben können), auch wenn der Cluster dadurch länger zum Stabilisieren braucht.

##Konfigurieren der Drosselungen
Die standardmäßig enthaltenen Drosselungen sind:

-	GlobalMovementThrottleThreshold – steuert die Gesamtzahl der Bewegungen im Cluster über eine bestimmte Zeit (definiert als das GlobalMovementThrottleCountingInterval, Wert in Sekunden)
-	MovementPerPartitionThrottleThreshold – steuert die Gesamtzahl der Bewegungen für jede Dienstpartition über eine bestimmte Zeit (definiert als das MovementPerPartitionThrottleCountingInterval, Wert in Sekunden)

``` xml
<Section Name="PlacementAndLoadBalancing">
     <Parameter Name="GlobalMovementThrottleThreshold" Value="1000" />
     <Parameter Name="GlobalMovementThrottleCountingInterval" Value="600" />
     <Parameter Name="MovementPerPartitionThrottleThreshold" Value="50" />
     <Parameter Name="MovementPerPartitionThrottleCountingInterval" Value="600" />
</Section>
```

Beachten Sie, dass Kunden diese Drosselungen in der Vergangenheit meist in Umgebungen verwendet haben, für die bereits Ressourceneinschränkungen galten (z.B. eine eingeschränkte Netzwerkbandbreite für einzelne Knoten oder Datenträger, die die Anforderungen einer parallelen Replikaterstellung nicht unterstützten). Diese Vorgänge konnten also ohnehin nicht bzw. nur langsam ausgeführt werden. In diesen Situationen waren Kunden sich bewusst, dass die Drosselungen möglicherweise die Zeitspanne verlängern, bis der Cluster sich wieder stabilisiert hat, und die Prozesse während des Drosselns möglicherweise mit einer niedrigeren allgemeinen Zuverlässigkeit ausgeführt werden.

## Nächste Schritte
- Informationen darüber, wie der Clusterressourcen-Manager die Auslastung im Cluster verwaltet und verteilt, finden Sie im Artikel zum [Lastenausgleich](service-fabric-cluster-resource-manager-balancing.md).
- Der Clusterressourcen-Manager bietet viele Optionen für die Beschreibung des Clusters. Weitere Informationen hierzu finden Sie in diesem Artikel zum [Beschreiben eines Service Fabric-Clusters](service-fabric-cluster-resource-manager-cluster-description.md).

<!---HONumber=AcomDC_0824_2016-->