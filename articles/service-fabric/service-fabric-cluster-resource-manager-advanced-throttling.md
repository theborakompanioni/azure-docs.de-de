---
title: Drosselung im Clusterressourcen-Manager von Service Fabric | Microsoft Docs
description: "Sie lernen, die Drosselungen zu konfigurieren, die der Clusterressourcen-Manager von Service Fabric ermöglicht."
services: service-fabric
documentationcenter: .net
author: masnider
manager: timlt
editor: 
ms.assetid: 4a44678b-a5aa-4d30-958f-dc4332ebfb63
ms.service: Service-Fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 01/05/2017
ms.author: masnider
translationtype: Human Translation
ms.sourcegitcommit: dafaf29b6827a6f1c043af3d6bfe62d480d31ad5
ms.openlocfilehash: 8a8419497bda3f1df523d6aff28028548abc155a
ms.lasthandoff: 01/07/2017


---

# <a name="throttling-the-behavior-of-the-service-fabric-cluster-resource-manager"></a>Drosselungen für das Verhalten des Clusterressourcen-Managers von Service Fabric
Auch wenn Sie den Clusterressourcen-Manager ordnungsgemäß konfiguriert haben, kann es für den Cluster zu Unterbrechungen kommen. Beispielsweise können Knoten oder Fehlerdomänen gleichzeitig ausfallen. Was wäre, wenn dies während eines Upgrades passieren würde? Der Clusterressourcen-Manager versucht, alle Fehler zu beheben, aber dies kann zu Änderungen im Cluster führen. Drosselungen bieten eine Abfangfunktion, damit der Cluster Ressourcen zur eigenen Stabilisierung verwenden kann. Die Knoten kehren zurück, die Netzwerkpartitionen werden repariert und die korrigierten Komponenten werden bereitgestellt.

Um Sie in Situationen wie diesen zu unterstützen, bietet der Clusterressourcen-Manager von Service Fabric mehrere Drosselungen an. Diese Drosselungen haben relativ umfangreiche Auswirkungen. Diese Einstellungen sollten nur dann von den Standardwerten abweichend geändert werden, wenn Sie sorgfältige mathematische Berechnungen zum Umfang der Arbeit durchgeführt haben, die der Cluster parallel ausführen kann.

Die Drosselungen weisen Standardwerte auf, die das Service Fabric-Team aus Erfahrungswerten gewonnen und für gut befunden hat. Wenn diese geändert werden müssen, sollten Sie sie anhand Ihrer erwarteten tatsächlichen Auslastung optimieren. Sie können zu dem Schluss kommen, dass Sie einige Drosselungen einsetzen müssen, auch wenn der Cluster dadurch in grundlegenden Situationen länger zur Stabilisierung braucht.

## <a name="configuring-the-throttles"></a>Konfigurieren der Drosselungen
Die standardmäßig enthaltenen Drosselungen sind:

* GlobalMovementThrottleThreshold: Diese Einstellung steuert die Gesamtzahl von Bewegungen im Cluster über eine bestimmte Zeit (definiert als das GlobalMovementThrottleCountingInterval, Wert in Sekunden)
* MovementPerPartitionThrottleThreshold: Diese Einstellung steuert die Gesamtzahl von Bewegungen für jede Dienstpartition über eine bestimmte Zeit (definiert als das MovementPerPartitionThrottleCountingInterval, Wert in Sekunden)

``` xml
<Section Name="PlacementAndLoadBalancing">
     <Parameter Name="GlobalMovementThrottleThreshold" Value="1000" />
     <Parameter Name="GlobalMovementThrottleCountingInterval" Value="600" />
     <Parameter Name="MovementPerPartitionThrottleThreshold" Value="50" />
     <Parameter Name="MovementPerPartitionThrottleCountingInterval" Value="600" />
</Section>
```

Über „ClusterConfig.json“ für eigenständige Bereitstellungen bzw. über „Template.json“ für in Azure gehostete Cluster:

```json
"fabricSettings": [
  {
    "name": "PlacementAndLoadBalancing",
    "parameters": [
      {
          "name": "GlobalMovementThrottleThreshold",
          "value": "1000"
      },
      {
          "name": "GlobalMovementThrottleCountingInterval",
          "value": "600"
      },
      {
          "name": "MovementPerPartitionThrottleThreshold",
          "value": "50"
      },
      {
          "name": "MovementPerPartitionThrottleCountingInterval",
          "value": "600"
      }
    ]
  }
]
```

In den meisten Fällen stellen wir fest, dass Kunden diese Drosselungen verwendet haben, weil sie sich ohnehin in einer Umgebung mit eingeschränkten Ressourcen befanden. Beispiele für eine solche Umgebung sind eine begrenzte Netzwerkbandbreite in einzelne Knoten oder Datenträger, die aufgrund von Durchsatzeinschränkungen nicht viele Replikate parallel erstellen können. Diese Arten von Einschränkungen bedeuteten, dass Vorgänge, die als Reaktion auf Fehler ausgelöst wurden, auch ohne die Drosselungen keinen Erfolg hatten oder langsam waren. In diesen Situationen waren sich die Kunden bewusst, dass sie die Zeitspanne erweitern, die der Cluster zum Erreichen eines stabilen Zustands benötigt. Kunden waren sich auch darüber im Klaren, dass sie während der Drosselung möglicherweise eine geringere allgemeine Zuverlässigkeit in Kauf nahmen.

## <a name="next-steps"></a>Nächste Schritte
* Informationen darüber, wie der Clusterressourcen-Manager die Auslastung im Cluster verwaltet und verteilt, finden Sie im Artikel zum [Lastenausgleich](service-fabric-cluster-resource-manager-balancing.md)
* Der Clusterressourcen-Manager bietet zahlreiche Optionen zum Beschreiben des Clusters. Weitere Informationen hierzu finden Sie in diesem Artikel zum [Beschreiben eines Service Fabric-Clusters](service-fabric-cluster-resource-manager-cluster-description.md).

