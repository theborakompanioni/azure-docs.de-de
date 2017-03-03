---
title: "Clusterressourcen-Manager von Service Fabric – Integration der Verwaltung | Microsoft Docs"
description: "Übersicht über die Integrationspunkte zwischen dem Clusterressourcen-Manager und der Service Fabric-Verwaltung."
services: service-fabric
documentationcenter: .net
author: masnider
manager: timlt
editor: 
ms.assetid: 956cd0b8-b6e3-4436-a224-8766320e8cd7
ms.service: Service-Fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 01/05/2017
ms.author: masnider
translationtype: Human Translation
ms.sourcegitcommit: b2428f93680c12c76000fa8eb1a7138f72a8efe7
ms.openlocfilehash: 9d67f089f4aba03e846a8fe020a91b6b1ac6ea48
ms.lasthandoff: 02/11/2017


---
# <a name="cluster-resource-manager-integration-with-service-fabric-cluster-management"></a>Integration des Clusterressourcen-Managers in die Service Fabric-Clusterverwaltung
Der Clusterressourcen-Manager von Service Fabric ist zwar nicht die Hauptkomponente von Service Fabric für die Ausführung von Verwaltungsvorgängen (wie z.B. Anwendungsupgrades), jedoch daran beteiligt. Bei der Verwaltung unterstützt Sie der Clusterressourcen-Manager zunächst einmal durch das Verfolgen des gewünschten Clusterstatus und der im Cluster enthaltenen Dienste. Der Clusterressourcen-Manager sendet Integritätsberichte, wenn der Cluster nicht in die gewünschte Konfiguration versetzt werden kann. Dies ist beispielsweise der Fall, wenn nicht genügend Kapazität verfügbar ist oder wenn über das Platzieren eines Diensts widersprüchliche Regeln vorliegen. Ein weiterer Teil der Integration hängt mit der Funktionsweise von Upgrades zusammen. Während eines Upgrades wird das Verhalten des Clusterressourcen-Managers geringfügig verändert. Diese beiden Integrationspunkte werden unten ausgeführt.

## <a name="health-integration"></a>Integration von Integrität
Der Clusterressourcen-Manager überwacht ständig die Regeln, die Sie für Ihre Dienste definiert haben, sowie die Kapazitäten, die auf den Knoten und im Cluster verfügbar sind. Wenn diese Regeln nicht erfüllt werden können oder wenn nicht genügend Kapazität verfügbar ist, werden Integritätswarnungen und -fehler ausgegeben. Wenn beispielsweise ein Knoten über der Kapazitätsgrenze liegt, versucht der Clusterressourcen-Manager die Situation zu beheben, indem er Dienste verschiebt. Wenn das Problem nicht behoben werden kann, gibt er eine Integritätswarnung aus. Diese gibt an, welcher Knoten für welche Metriken über der Kapazitätsgrenze liegt.

Ein weiteres Beispiel für die Integritätswarnungen des Ressourcen-Managers sind Verstöße gegen Platzierungsbeschränkungen. Wenn Sie beispielsweise eine Platzierungseinschränkung definiert haben (z.B. `“NodeColor == Blue”`) und der Ressourcen-Manager einen Verstoß gegen diese Einschränkung erkennt, gibt er eine Integritätswarnung aus. Dies gilt für benutzerdefinierte Einschränkungen und die Standardeinschränkungen (z.B. Einschränkungen für Fehler- und Upgradedomänen).

Hier ein Beispiel eines solchen Integritätsberichts. In diesem Fall gilt der Integritätsbericht für eine der Systemdienstpartitionen. Die Integritätsmeldung zeigt an, dass die Replikate dieser Partition vorübergehend in zu wenigen Upgradedomänen abgelegt werden.

```posh
PS C:\Users\User > Get-WindowsFabricPartitionHealth -PartitionId '00000000-0000-0000-0000-000000000001'


PartitionId           : 00000000-0000-0000-0000-000000000001
AggregatedHealthState : Warning
UnhealthyEvaluations  :
                        Unhealthy event: SourceId='System.PLB', Property='ReplicaConstraintViolation_UpgradeDomain', HealthState='Warning', ConsiderWarningAsError=false.

ReplicaHealthStates   :
                        ReplicaId             : 130766528804733380
                        AggregatedHealthState : Ok

                        ReplicaId             : 130766528804577821
                        AggregatedHealthState : Ok

                        ReplicaId             : 130766528854889931
                        AggregatedHealthState : Ok

                        ReplicaId             : 130766528804577822
                        AggregatedHealthState : Ok

                        ReplicaId             : 130837073190680024
                        AggregatedHealthState : Ok

HealthEvents          :
                        SourceId              : System.PLB
                        Property              : ReplicaConstraintViolation_UpgradeDomain
                        HealthState           : Warning
                        SequenceNumber        : 130837100116930204
                        SentAt                : 8/10/2015 7:53:31 PM
                        ReceivedAt            : 8/10/2015 7:53:33 PM
                        TTL                   : 00:01:05
                        Description           : The Load Balancer has detected a Constraint Violation for this Replica: fabric:/System/FailoverManagerService Secondary Partition 00000000-0000-0000-0000-000000000001 is
                        violating the Constraint: UpgradeDomain Details: Node -- 3d1a4a68b2592f55125328cd0f8ed477  Policy -- Packing
                        RemoveWhenExpired     : True
                        IsExpired             : False
                        Transitions           : Ok->Warning = 8/10/2015 7:13:02 PM, LastError = 1/1/0001 12:00:00 AM
```

Diese Integritätsmeldung gibt Folgendes an:

1. Alle Replikate selbst sind fehlerfrei (dies hat für Service Fabric erste Priorität).
2. Derzeit wird gegen die Einschränkung für die Verteilung von Upgradedomänen verstoßen (was heißt, dass eine bestimmte Upgradedomäne mehr Replikate für diese Partition aufweist, als sie sollte).
3. Den Knoten mit dem Replikat, der den Verstoß verursacht (mit der ID „3d1a4a68b2592f55125328cd0f8ed477“).
4. Wann der Bericht erstellt wurde (10.08.2015 19:13:02 Uhr)

Informationen wie diese ermöglichen Warnungen, die in der Produktion ausgelöst werden, um Sie über ein Problem zu informieren. In diesem Fall möchten wir herausfinden, warum der Ressourcen-Manager die Replikate in der Upgradedomäne ablegen musste. Der Grund könnte beispielsweise sein, dass die Knoten in den anderen Upgradedomänen ausgefallen sind.

Angenommen, Sie möchten einen Dienst erstellen, oder der Ressourcen-Manager versucht, einen Ort zu finden, an dem verschiedene Dienste platziert werden können, es gibt jedoch keine funktionierenden Lösungen. Dafür kann es zahlreiche Gründe geben, doch meist ist eine der beiden folgenden Bedingungen dafür verantwortlich:

1. Eine vorübergehende Bedingung hat es unmöglich gemacht, diese Dienstinstanz bzw. dieses Replikat ordnungsgemäß zu platzieren.
2. Die Anforderungen des Diensts sind auf eine Weise falsch konfiguriert, die das Erfüllen seiner Anforderungen unmöglich machen.

Bei beiden Bedingungen zeigt der Clusterressourcen-Manager einen Integritätsbericht mit Informationen an, mit deren Hilfe Sie ermitteln können, was passiert ist und warum der Dienst nicht platziert werden kann. Wir nennen diesen Prozess „Sequenz zum Entfernen von Einschränkungen“. Im Verlauf dieses Prozesses durchläuft das System die konfigurierten Einschränkungen, die sich auf den Dienst auswirken, und zeichnet auf, was die Einschränkungen entfernen. Wenn Dienste nicht platziert werden können, lässt sich so prüfen, welche Knoten entfernt wurden und warum.

## <a name="constraint-types"></a>Einschränkungstypen
Nun sehen wir uns die verschiedenen Einschränkungen in diesen Integritätsberichten genauer an. In den meisten Fällen werden durch diese Einschränkungen keine Knoten entfernt, da sie standardmäßig als „weiche“ Einschränkungen oder auf Optimierungsebene wirken. Möglicherweise werden aber Integritätsmeldungen zu diesen Einschränkungen angezeigt, wenn sie als „harte“ Einschränkungen konfiguriert wurden oder – in seltenen Fällen – doch zum Entfernen von Knoten führen.

* **ReplicaExclusionStatic** und **ReplicaExclusionDynamic**: Diese Einschränkung weist darauf hin, dass zwei zustandsbehaftete Replikate oder zustandslose Instanzen aus der gleichen Partition auf einem Knoten platziert werden müssten (was nicht zulässig ist). Die Regel für „ReplicaExclusionStatic“ und die für „ReplicaExclusionDynamic“ sind fast identisch. Die Einschränkung „ReplicaExclusionDynamic“ besagt, dass dieses Replikat hier nicht platzieren werden konnte, da die einzige vorgeschlagene Lösung hier bereits ein Replikat platziert hat. Dies unterscheidet sich von der Einschränkung „ReplicaExclusionStatic“, die nicht auf einen vermeintlichen, sondern auf einen tatsächlichen Konflikt hinweist. In diesem Fall ist bereits ein Replikat auf dem Knoten vorhanden. Ist das verwirrend? Ja. Ist das wirklich ein Problem? Nein. Wenn Ihnen eine Sequenz zum Entfernen von Einschränkungen angezeigt wird, die entweder die Einschränkung „ReplicaExclusionStatic“ oder „ReplicaExclusionDynamic“ enthält, geht der Clusterressourcen-Manager davon aus, dass nicht genügend Knoten vorhanden sind. Die weiteren Einschränkungen geben uns normalerweise einen Hinweis, wie es dazu kommt, dass zu wenige Knoten vorhanden sind.
* **PlacementConstraint**: Wenn diese Meldung angezeigt wird, bedeutet dies, dass wir einige Knoten entfernt haben, da sie nicht den Platzierungseinschränkungen des Diensts entsprachen. Als Teil dieser Nachricht finden wir die derzeit konfigurierten Platzierungseinschränkungen. Dies ist normal, wenn Sie Platzierungsbeschränkungen definiert haben. Wenn in der Platzierungseinschränkung jedoch ein Fehler vorliegt, durch den zu viele Knoten entfernt werden, würde es Ihnen an dieser Stelle auffallen.
* **NodeCapacity**: Diese Einschränkung bedeutet, dass der Clusterressourcen-Manager die Replikate nicht auf den angegebenen Knoten platzieren konnte, da dadurch die Kapazität des Knotens überschritten würde.
* **Affinity**: Diese Einschränkung gibt an, dass wir das Replikat nicht auf den betroffenen Knoten platzieren konnten, da dies zu einem Verstoß gegen die Affinitätseinschränkung führen würde. Weitere Informationen zur Affinität finden Sie in [diesem Artikel](service-fabric-cluster-resource-manager-advanced-placement-rules-affinity.md).
* **FaultDomain** und **UpgradeDomain**: Diese Einschränkung entfernt Knoten, wenn das Platzieren des Replikats auf den angegebenen Knoten eine Überlastung in einer bestimmten Fehler- oder Upgradedomäne bewirken würde. Sie finden verschiedene Beispiele zu dieser Einschränkung im Thema zu [Einschränkungen und daraus resultierendes Verhalten von Fehler- und Upgradedomänen](service-fabric-cluster-resource-manager-cluster-description.md)
* **PreferredLocation**: Diese Einschränkung führt normalerweise nicht dazu, dass Knoten aus der Lösung entfernt werden, da sie standardmäßig auf der Optimierungsebene festgelegt wird. Darüber hinaus ist während Upgrades in der Regel die Einschränkung für den bevorzugten Standort vorhanden. Während eines Upgrades dient sie dazu, Replikate an die Stelle zurück zu verschieben, an der sie sich bei Beginn des Upgrades befanden.

### <a name="constraint-priorities"></a>Einschränkungsprioritäten
Bei all diesen Einschränkungen sind Sie vielleicht zu folgendem Schluss gekommen: „Platzierungseinschränkungen sind in meinem System das Wichtigste. Ich bin bereit, andere Einschränkungen zu verletzen, auch Dinge wie Affinität und Kapazität, wenn dies sicherstellt, dass die Platzierungseinschränkungen niemals verletzt werden.“

Es stellt sich heraus, dass wir dies tun können! Einschränkungen können mit einigen anderen Erzwingungsebenen konfiguriert werden, aber sie laufen auf „hart“ (0), „weich“ (1), „Optimierung“ (2) und „deaktiviert“ (-1) hinaus. Die meisten Einschränkungen haben wir standardmäßig als „hart“ definiert. Die meisten Benutzer betrachten die Kapazität als Einschränkung, die nicht verletzt werden darf. Fast alle Einschränkungen sind entweder hart oder weich.

Die verschiedenen Einschränkungsprioritäten sind der Grund dafür, dass Sie einige Einschränkungsverletzungswarnungen häufiger sehen als andere: weil wir bereit sind, bestimmte Einschränkungen vorübergehend zu lockern (zu verletzen). Diese Ebenen bedeuten nicht unbedingt, dass eine bestimmte Einschränkung in jedem Fall verletzt wird, sondern nur, dass es eine Reihenfolge gibt, in der die Einstellungen nach Bedarf erzwungen werden. So kann der Clusterressourcen-Manager die richtigen Kompromisse eingehen, wenn nicht alle Einschränkungen erfüllt werden können.

In komplexeren Situationen können die Einschränkungsprioritäten geändert werden. Angenommen, Sie möchten sicherstellen, dass die Affinität ggf. verletzt wird, um Probleme mit der Knotenkapazität zu beheben. Um dies zu erreichen, könnten Sie die Priorität für die Affinitätseinschränkung auf „weich“ (1) einstellen und die Kapazitätseinschränkung auf „hart“ (0) festgelegt lassen.

Die Standardprioritätswerte für die verschiedenen Einschränkungen sind in der Konfiguration aufgeführt:

ClusterManifest.xml

```xml
        <Section Name="PlacementAndLoadBalancing">
            <Parameter Name="PlacementConstraintPriority" Value="0" />
            <Parameter Name="CapacityConstraintPriority" Value="0" />
            <Parameter Name="AffinityConstraintPriority" Value="0" />
            <Parameter Name="FaultDomainConstraintPriority" Value="0" />
            <Parameter Name="UpgradeDomainConstraintPriority" Value="1" />
            <Parameter Name="PreferredLocationConstraintPriority" Value="2" />
        </Section>
```

Über „ClusterConfig.json“ für eigenständige Bereitstellungen bzw. über „Template.json“ für in Azure gehostete Cluster:

```json
"fabricSettings": [
  {
    "name": "PlacementAndLoadBalancing",
    "parameters": [
      {
          "name": "PlacementConstraintPriority",
          "value": "0"
      },
      {
          "name": "CapacityConstraintPriority",
          "value": "0"
      },
      {
          "name": "AffinityConstraintPriority",
          "value": "0"
      },
      {
          "name": "FaultDomainConstraintPriority",
          "value": "0"
      },
      {
          "name": "UpgradeDomainConstraintPriority",
          "value": "1"
      },
      {
          "name": "PreferredLocationConstraintPriority",
          "value": "2"
      }
    ]
  }
]
```

## <a name="fault-domain-and-upgrade-domain-constraints"></a>Einschränkungen für Fehlerdomänen und Upgradedomänen
Der Clusterressourcen-Manager setzt die Anforderung um, Dienste über Fehler- und Upgradedomänen zu verteilen und dies als Einschränkung innerhalb des Ressourcen-Manager-Moduls zu verwalten. Weitere Informationen zu ihrer Verwendung finden Sie im Artikel zur [Clusterkonfiguration](service-fabric-cluster-resource-manager-cluster-description.md).

Es gibt Zeiten, in denen wir Fehler- und Upgradedomänen streng beachten müssen, um schwerwiegende Fehler zu vermeiden. Es gibt jedoch auch Fälle, in denen wir sie vollständig ignorieren mussten (wenn auch nur für kurze Zeit). Die Flexibilität der Infrastruktur mit Einschränkungsprioritäten funktioniert in der Regel sehr gut, ist jedoch nicht häufig erforderlich. In den meisten Fällen gelten die Standardprioritäten. Upgradedomänen bleiben eine weiche Einschränkung. Der Clusterressourcen-Manager muss möglicherweise einige Replikate in einer Upgradedomäne platzieren, um ein Upgrade, Fehler oder Verstöße gegen Einschränkungen zu verarbeiten. Dies geschieht normalerweise nur, wenn mehrere Fehler oder andere Änderungen im System eine richtige Platzierung verhindern. Wenn die Umgebung ordnungsgemäß konfiguriert ist, werden alle Einschränkungen, einschließlich Fehler- und Upgradeeinschränkungen, selbst während der Upgrades vollständig eingehalten. Der wichtigste Punkt ist, dass der Clusterressourcen-Manager die Einschränkungen überwacht und sofort meldet, wenn Verstöße erkannt werden.

## <a name="the-preferred-location-constraint"></a>Die Einschränkung für den bevorzugten Standort
Die Einschränkung PreferredLocation ist ein wenig anders und daher als einzige Einschränkung auf „Optimierung“ festgelegt. Wir verwenden diese Einschränkung während der Durchführung von Upgrades, um zu versuchen, Dienste vorzugsweise wieder dort zu platzieren, wo wir sie vor dem Upgrade angetroffen haben. Aus verschiedenen Gründen funktioniert dies in der Praxis möglicherweise nicht, aber es ist dennoch eine nette Optimierung.

## <a name="upgrades"></a>Upgrades
Der Clusterressourcen-Manager ist auch bei Anwendungs- und Clusterupgrades hilfreich, bei denen er zwei Aufgaben erfüllt:

* Sicherstellen, dass die Regeln und die Leistung des Clusters nicht gefährdet sind
* Unterstützen einer reibungslosen Durchführung des Upgrades

### <a name="keep-enforcing-the-rules"></a>Fortsetzen des Erzwingens der Regeln
Ein überaus wichtiger Aspekt ist, dass die Regeln – d.h. die strikten Einschränkungen z.B. hinsichtlich der Platzierung – auch im Verlauf von Upgrades weiter erzwungen werden. Platzierungsbeschränkungen stellen sicher, dass Ihre Arbeitsauslastungen auch während der Upgrades nur an erlaubter Stelle ausgeführt werden. Wenn Ihre Umgebung stark eingeschränkt ist, können Upgrades sehr lange dauern. Der Grund dafür ist, dass es möglicherweise nur wenige Optionen für die Platzierung eines Diensts gibt, wenn er (oder der Knoten, auf dem er sich befindet) für ein Update heruntergefahren werden muss.

### <a name="smart-replacements"></a>Intelligenter Ersatz
Zu Beginn eines Upgrades erfasst der Ressourcen-Manager eine Momentaufnahme der aktuellen Clusteranordnung. Während die einzelnen Upgradedomänen abgeschlossen werden, versucht er, die ursprüngliche Anordnung wiederherzustellen. Auf diese Weise gibt es während des Upgrades höchstens zwei Übergänge (die Verschiebung aus dem betroffenen Knoten und die Verschiebung zurück auf den Knoten). Durch die Wiederherstellung des Clusters in den Zustand vor dem Upgrade wird außerdem sichergestellt, dass das Layout des Clusters nicht durch das Upgrade beeinträchtigt wird. Wenn der Cluster vor dem Upgrade passend angeordnet war, ist er auch nach dem Upgrade passend angeordnet. Zumindest gibt es keine Verschlechterung.

### <a name="reduced-churn"></a>Weniger Änderungen
Wichtig ist der Hinweis, dass der Clusterressourcen-Manager während des Upgrades den Lastenausgleich für die Entität deaktiviert, für die das Upgrade erfolgt. Wenn Sie daher zwei verschiedene Anwendungsinstanzen verwenden und auf einer davon ein Upgrade durchführen, wird der Lastenausgleich für diese Anwendungsinstanz, aber nicht für die andere angehalten. Durch das Verhindern des Lastenausgleichs werden unnötige Reaktionen auf das Upgrade selbst verhindert, beispielsweise das Verschieben von Diensten auf Knoten, die für das Upgrade geleert wurden. Wenn es sich beim betreffenden Upgrade um ein Clusterupgrade handelt, findet für den gesamten Cluster während des Upgrades kein Lastenausgleich statt. Einschränkungsüberprüfungen – die sicherstellen, dass die Regeln erzwungen werden – bleiben aktiv, nur die Umverteilung wird deaktiviert.

### <a name="relaxed-rules"></a>Gelockerte Regeln
Im Allgemeinen möchten Sie, dass das Upgrade auch dann abgeschlossen wird, wenn der Cluster eingeschränkt oder voll belegt ist. Während eines Upgrades ist die Möglichkeit zum Verwalten der Clusterkapazität noch wichtiger als sonst. Der Grund dafür ist, dass normalerweise zwischen 5 und 20 Prozent der Kapazität ausfällt, während das Upgrade den Cluster durchläuft. Diese Arbeit muss normalerweise irgendwo aufgefangen werden. Hier kommt das Konzept der [gepufferten Kapazitäten](service-fabric-cluster-resource-manager-cluster-description.md#buffered-capacity) zum Tragen. Während die gepufferte Kapazität während des Normalbetriebs berücksichtigt wird (und für Mehraufwand sorgt), füllt der Clusterressourcen-Manager diese im Verlauf von Upgrades bis zur Gesamtkapazität auf (und verbraucht dabei den Puffer).

## <a name="next-steps"></a>Nächste Schritte
* Starten Sie mit einer [Einführung in den Clusterressourcen-Manager von Service Fabric](service-fabric-cluster-resource-manager-introduction.md)

