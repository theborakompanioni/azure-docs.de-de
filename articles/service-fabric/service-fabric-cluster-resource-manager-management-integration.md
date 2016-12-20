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
ms.date: 08/19/2016
ms.author: masnider
translationtype: Human Translation
ms.sourcegitcommit: 219dcbfdca145bedb570eb9ef747ee00cc0342eb
ms.openlocfilehash: f13e38b4c01bc718f6f25f92461e332e1aa30136


---
# <a name="cluster-resource-manager-integration-with-service-fabric-cluster-management"></a>Integration des Clusterressourcen-Managers in die Service Fabric-Clusterverwaltung
Der Clusterressourcen-Manager von Service Fabric ist zwar nicht die Hauptkomponente von Service Fabric für die Ausführung von Verwaltungsvorgängen (wie z.B. Anwendungsupgrades), jedoch daran beteiligt. Die erste Unterstützung, die der Clusterressourcen-Manager bei der Verwaltung bietet, ist die Nachverfolgung des gewünschten Status des Clusters und der darin enthaltenen Dienste aus Perspektive der Ressourcen und des Lastenausgleichs. Zudem sendet der Clusterressourcen-Manager Integritätsberichte, wenn die gewünschte Konfiguration für den Cluster nicht umgesetzt werden kann (Beispiel: nicht genügend Kapazität, oder in Konflikt stehende Regeln zur Platzierung eines Diensts). Ein weiterer Teil der Integration hat mit der Funktionsweise von Upgrades zu tun: Während Upgrades ändert der Clusterressourcen-Manager das Verhalten. Darauf gehen wir weiter unten ein.

## <a name="health-integration"></a>Integration von Integrität
Der Clusterressourcen-Manager verfolgt ständig die Regeln, die Sie für Ihre Dienste definiert haben, sowie die verfügbaren Kapazitäten auf den Knoten und im Cluster und gibt Integritätswarnungen und Fehlermeldungen aus, wenn diese Regeln nicht erfüllt werden können oder die Kapazität nicht ausreicht. Wenn z.B. ein Knoten seine Kapazität vollständig ausgeschöpft hat und der Clusterressourcen-Manager die Situation nicht korrigieren kann, wird eine Integritätswarnung ausgegeben, die angibt, welcher Knoten bei welchen Metriken keine Kapazität mehr hat.

Ein weiteres Beispiel der Anzeige von Integritätswarnungen durch den Ressourcen-Manager ist, wenn Sie eine Platzierungseinschränkung (z. B. „NodeColor == Blue“) definiert haben und ein Verstoß gegen diese Einschränkung erkannt wird. Dies erfolgt sowohl für benutzerdefinierte Einschränkungen als auch für die Standardeinschränkungen (z.B. Einschränkungen von Fehler- und Upgradedomänen), die automatisch erzwungen werden.

Hier ein Beispiel eines solchen Integritätsberichts. In diesem Fall gilt der Integritätsbericht einer der Partitionen des Systemdiensts, da die Replikate dieser Partition vorübergehend in zu wenig Upgradedomänen abgelegt sind, was bei einer Kette von Ausfällen passieren kann:

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
4. Den Zeitpunkt des Auftretens des Verstoßes (10.08.2015 19:13:02 Uhr).

Dies sind aussagekräftige Daten für eine Warnung, die in der Produktion ausgelöst wird, um Sie zu informieren, dass etwas falsch gelaufen ist und Sie einen Blick darauf werfen sollten. In diesem Fall möchten wir herausfinden, warum der Ressourcen-Manager keine andere Wahl hatte, als die Replikate in der Upgradedomäne abzulegen. Eine Möglichkeit ist, dass alle Knoten in den anderen Upgradedomänen ausgefallen waren und es nicht genügend Ersatzdomänen gab. Oder wenn es genügend aktive Domänen gab, ist etwas anderes vorgefallen, das bewirkt hat, dass die Knoten in diesen anderen Upgradedomänen ungültig waren (z.B. eine Platzierungsrichtlinie für den Dienst oder zu wenig Kapazität).

Angenommen, Sie möchten einen Dienst erstellen oder der Ressourcen-Manager versucht, einen Ort zu finden, an dem verschiedene Dienste platziert werden können, doch es gibt anscheinend keine Lösungen, die funktionieren. Dafür kann es zahlreiche Gründe geben, doch meist ist eine der beiden folgenden Bedingungen dafür verantwortlich:

1. Eine vorübergehende Bedingung hat es unmöglich gemacht, diese Dienstinstanz bzw. dieses Replikat ordnungsgemäß zu platzieren.
2. Die Anforderungen des Diensts sind auf eine Weise falsch konfiguriert, die das Erfüllen seiner Anforderungen unmöglich machen.

Bei beiden Bedingungen zeigt der Clusterressourcen-Manager einen Integritätsbericht an, mit dessen Informationen Sie ermitteln können, was passiert ist und warum der Dienst nicht platziert werden kann. Wir nennen diesen Prozess „Sequenz zum Entfernen von Einschränkungen“. Im Verlauf dieses Prozesses durchläuft das System die konfigurierten Einschränkungen, die sich auf den Dienst auswirken, und zeichnet auf, was die Einschränkungen entfernen. Wenn Dienste nicht platziert werden können, lässt sich so prüfen, welche Knoten entfernt wurden und warum.

## <a name="constraint-types"></a>Einschränkungstypen
Nun wollen wir uns die verschiedenen Einschränkungen und ihre Prüfungen in diesen Integritätsberichten genauer ansehen. Im Allgemeinen werden Sie nicht feststellen, dass diese Einschränkungen Knoten entfernen, da sich die Einschränkungen standardmäßig auf den Stufen „Soft“ oder „Optimize“ befinden (im weiteren Verlauf dieses Artikels erfahren Sie mehr über Einschränkungsprioritäten). Möglicherweise werden aber Integritätsmeldungen zu diesen Einschränkungen angezeigt, wenn sie als „harte“ Einschränkungen konfiguriert wurden oder in den seltenen Fällen, in denen sie doch dafür sorgen, dass Knoten entfernt werden. Daher führen wir sie der Vollständigkeit halber hier auf:

* „ReplicaExclusionStatic“ und „ReplicaExclusionDynamic“: Dies ist eine interne Einschränkung, die darauf hinweist, dass während der Suche festgestellt wurde, dass zwei zustandsbehaftete Replikate oder zustandslose Instanzen aus der gleichen Partition auf einem Knoten platziert werden müssten (was nicht zulässig ist). Die Regel für „ReplicaExclusionStatic“ und „ReplicaExclusionDynamic“ ist fast identisch. Die Einschränkung „ReplicaExclusionDynamic“ besagt, dass wir dieses Replikat hier nicht platzieren können, da die einzige vorgeschlagene Lösung hier bereits ein Replikat platziert hat. Dies unterscheidet sich vom Ausschluss „ReplicaExclusionStatic“, der nicht auf einen vermeintlichen Konflikt, sondern auf einen tatsächlichen verweist, denn es gibt bereits ein Replikat auf dem Knoten. Ist das verwirrend? Ja. Ist das wirklich ein Problem? Nein. Wenn Sie eine Sequenz zum Entfernen von Einschränkungen sehen, die entweder die Einschränkung „ReplicaExclusionStatic“ oder „ReplicaExclusionDynamic“ aufweist, geht der Clusterressourcen-Manager davon aus, dass nicht genügend Knoten zur Platzierung aller Replikate vorhanden sind. Die weiteren Einschränkungen informieren uns meist, wie wir enden, wenn von Anfang an zu wenig vorhanden sind.
* PlacementConstraint: Wenn diese Meldung angezeigt wird, bedeutet dies, dass wir einige Knoten entfernt haben, da sie nicht mit den Platzierungseinschränkungen des Diensts übereinstimmen. Als Teil dieser Nachricht finden wir die derzeit konfigurierten Platzierungseinschränkungen. Dies ist in der Regel normal, wenn Sie Platzierungseinschränkungen bereitgestellt haben. Wenn eine Platzierungseinschränkung jedoch einen Fehler aufweist, aufgrund dessen zu viele Knoten entfernt werden, könnte dieses Ergebnis angezeigt werden.
* NodeCapacity: Wenn Sie diese Einschränkung sehen, bedeutet dies, dass wir die Replikate nicht auf den angegebenen Knoten platzieren konnten, da dadurch die Kapazität des Knotens überschritten würde.
* Affinity: Diese Einschränkung gibt an, dass wir das Replikat nicht auf den betroffenen Knoten platzieren konnten, da dies zu einem Verstoß gegen die Affinitätseinschränkung führen würde.
* FaultDomain und UpgradeDomain: Diese Einschränkung entfernt Knoten, wenn das Platzieren des Replikats auf den angegebenen Knoten eine Überlastung in einer bestimmten Fehler- oder Upgradedomäne bewirken würde. Sie finden verschiedene Beispiele zu dieser Einschränkung im Thema zu [Einschränkungen und daraus resultierendes Verhalten von Fehler- und Upgradedomänen](service-fabric-cluster-resource-manager-cluster-description.md)
* PreferredLocation: Diese Einschränkung wird normalerweise nicht angezeigt. Sie bewirkt, dass Knoten aus der Lösung entfernt werden, da sie standardmäßig nur auf Optimierung ausgelegt ist. Darüber hinaus ist die Einschränkung „PreferredLocation“ meist nur während Upgrades vorhanden (wenn sie verwendet wird, um Replikate dorthin zurück zu verschieben, wo sie bei Beginn des Upgrades waren). Sie wird jedoch möglicherweise angezeigt.

### <a name="constraint-priorities"></a>Einschränkungsprioritäten
Bei allen diesen Einschränkungen haben Sie vielleicht gedacht: „Hallo – ich glaube, dass Platzierungseinschränkungen in meinem System das Wichtigste sind. Ich bin bereit, andere Einschränkungen zu verletzen, auch Dinge wie Affinität und Kapazität, wenn dies sicherstellt, dass die Platzierungseinschränkungen niemals verletzt werden.“

Es stellt sich heraus, dass wir dies tun können! Einschränkungen können mit einigen anderen Erzwingungsebenen konfiguriert werden, aber sie laufen auf „hart“ (0), „weich“ (1), „Optimierung“ (2) und „deaktiviert“ (-1) hinaus. Die meisten Einschränkungen haben wir standardmäßig als hart definiert (da die meisten Benutzer z.B. die Kapazität normalerweise nicht als etwas betrachten, das sie lockern möchten), und fast alle sind entweder hart oder weich. Jedoch können sie in komplexeren Situationen geändert werden. Was ist beispielsweise, wenn Sie sicherstellen möchten, dass die Affinität immer verletzt werden darf, um Knotenkapazitätsprobleme zu lösen? Sie könnten die Priorität für die Affinitätseinschränkung auf „weich“ (1) festlegen und die Kapazitätseinschränkung bei „hart“ (0) belassen. Die verschiedenen Einschränkungsprioritäten sind auch der Grund dafür, dass Sie einige Einschränkungsverletzungswarnungen häufiger sehen als andere – weil wir bereit sind, bestimmte Einschränkungen vorübergehend zu lockern (verletzen). Beachten Sie, dass diese Ebenen nicht wirklich bedeuten, dass eine gegebene Einschränkung verletzt oder nicht immer verletzt wird, sondern nur, dass es eine bestimmte Reihenfolge gibt, in der sie vorzugsweise erzwungen werden, damit wir die richtigen Kompromisse treffen können, wenn es unmöglich ist, alle von ihnen zu erfüllen.

Die Konfigurations- und Standardprioritätswerte für die verschiedenen Einschränkungen sind nachfolgend aufgeführt:

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

Sie sehen hier, dass Einschränkungen für Upgrade-und Fehlerdomänen definiert sind, und auch, dass die Einschränkung für die Upgradedomäne „weich“ ist. Und es gibt auch diese seltsame „PreferredLocation“-Einschränkung mit einer Priorität. Was soll das alles?

Vor allem möchten wir über Fehler- und Upgradedomänen verteilte Dienste als Einschränkung innerhalb des Ressourcen-Manager-Moduls verwalten. In der Vergangenheit mussten wir oft entweder im Hinblick auf Fehler- und Upgradedomänen auf eine sehr strenge Platzierung achten, und in ein paar Fällen trat auch das Problem auf, dass wir sie (wenn auch nur kurz!) vollständig ignorieren mussten, sodass wir normalerweise über diese Entwurfsoption und die Flexibilität der Einschränkungsinfrastruktur froh waren. In den meisten Fällen liegt die Einschränkung für die Upgradedomäne jedoch als weiche Einschränkung vor, d.h. wenn der Resource Manager vorübergehend einige Replikate in eine Upgradedomäne packen muss, weil z.B. gerade ein Upgrade ausgeführt wird oder eine Reihe gleichzeitiger Ausfälle oder sonstiger Einschränkungsverletzungen (bei den harten Einschränkungen) auftritt, dann ist das in Ordnung. Normalerweise geschieht dies nicht, solange nicht viele Fehler oder andere Änderungen im System die korrekte Platzierung verhindern, und wenn die Umgebung ordnungsgemäß konfiguriert ist, wird die Upgradedomäne im stabilen Zustand immer vollständig respektiert.

Die Einschränkung PreferredLocation ist ein wenig anders und daher als einzige Einschränkung auf „Optimierung“ festgelegt. Wir verwenden diese Einschränkung während der Durchführung von Upgrades, um zu versuchen, Dienste vorzugsweise wieder dort zu platzieren, wo wir sie vor dem Upgrade angetroffen haben. Aus verschiedenen Gründen funktioniert dies in der Praxis möglicherweise nicht, aber es ist nun mal eine nette Optimierung. Sie erfahren mehr darüber, wenn wir erörtern, wie der Clusterressourcen-Manager Upgrades unterstützt.

## <a name="upgrades"></a>Upgrades
Der Clusterressourcen-Manager hilft auch bei Anwendungs- und Clusterupgrades, um sicherzustellen, dass das Upgrade reibungslos verläuft. Er soll aber auch sicherstellen, dass die Regeln und Leistung des Clusters nicht beeinträchtigt werden.

### <a name="keep-enforcing-the-rules"></a>Fortsetzen des Erzwingens der Regeln
Ein überaus wichtiger Aspekt ist, dass die Regeln – d.h. die strikten Einschränkungen z.B. hinsichtlich der Platzierung – auch im Verlauf von Upgrades weiter erzwungen werden. Sie meinen, dass sei wohl selbstverständlich, was es auch ist und wir hiermit noch einmal betonen möchten. Der positive Aspekt ist dabei: Wenn Sie sicher sein möchten, dass bestimmte Workloads auf bestimmten Knoten nicht ausgeführt werden, können Sie sicherstellen, dass diese Regeln auch während des Upgrades automatisch erzwungen werden. Wenn Ihre Umgebung zahlreiche Einschränkungen aufweist, können Upgrades lange dauern, da es nur einige wenige Optionen dafür gibt, was mit einem Dienst (bzw. dem Knoten, auf dem er sich befindet) passieren kann, wenn er für ein Update heruntergefahren werden muss.

### <a name="smart-replacements"></a>Intelligenter Ersatz
Wenn ein Upgrade gestartet wird, erstellt der Ressourcen-Manager eine Momentaufnahme der aktuellen Anordnung des Clusters und versucht, nach Abschluss des Upgrades diesen Status wiederherzustellen. Die Gründe dafür sind einfach: Erstens wird sichergestellt, dass im Rahmen des Upgrades nur wenige Übergänge für jedes Dienstreplikat bzw. jede Dienstinstanz notwendig sind (die Verschiebung aus dem betroffenen Knoten und wieder zurück). Zweitens wird sichergestellt, dass das Upgrade selbst keine große Auswirkung auf das Layout des Clusters hat. Wenn der Cluster vor dem Upgrade gut angeordnet war, wird dies auch im Anschluss daran der Fall sein, oder zumindest nicht schlechter.

### <a name="reduced-churn"></a>Weniger Änderungen
Wichtig ist der Hinweis, dass der Clusterressourcen-Manager während des Upgrades den Lastenausgleich für die Entität deaktiviert, für die das Upgrade erfolgt. Wenn Sie zwei verschiedene Anwendungsinstanzen haben und auf einer davon ein Upgrade starten, wird der Lastenausgleich für diese Anwendungsinstanz, aber nicht für die andere angehalten. Reaktiven Lastenausgleich zu verhindern, verhindert unnötige Reaktionen auf das Upgrade selbst („Oh nein! Ein leerer Knoten! Da muss doch was drauf!“) sowie viele zusätzliche Verschiebungen für Dienste im Cluster, die wieder rückgängig gemacht werden müssen, wenn die Dienste nach Abschluss des Upgrades wieder auf die Knoten verschoben werden müssen. Wenn das betreffende Upgrade ein Clusterupgrade ist, wird der Lastenausgleich für die Dauer des Upgrades im gesamten Cluster angehalten (Einschränkungsüberprüfungen – Sicherstellen der Erzwingung von Regeln – bleiben aktiv, es wird nur der proaktive erneute Ausgleich deaktiviert).

### <a name="relaxed-rules"></a>Gelockerte Regeln
Einer der Aspekte, der allgemein für Upgrades gilt, ist, dass Sie wollen, dass das Upgrade abgeschlossen wird, auch wenn der Cluster insgesamt einschränkt oder voll ist. Während Upgrades ist es sogar noch wichtiger als sonst, die Kapazität des Clusters verwalten zu können, da üblicherweise zwischen 5 und 20 Prozent der Kapazität nicht verfügbar sind, während das Upgrade nach und nach im gesamten Cluster ausgeführt wird. Die betreffenden Workloads müssen also anderweitig untergebracht werden. Hier kommt das Konzept der [gepufferten Kapazitäten](service-fabric-cluster-resource-manager-cluster-description.md#buffered-capacity) zum Tragen. Während die gepufferte Kapazität während des Normalbetriebs berücksichtigt wird (und für Mehraufwand sorgt), füllt der Clusterressourcen-Manager diese im Verlauf von Upgrades bis zur Gesamtkapazität auf (und verbraucht dabei den Puffer).

## <a name="next-steps"></a>Nächste Schritte
* Starten Sie mit einer [Einführung in den Clusterressourcen-Manager von Service Fabric](service-fabric-cluster-resource-manager-introduction.md)




<!--HONumber=Nov16_HO3-->


