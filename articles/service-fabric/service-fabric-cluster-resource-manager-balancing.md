---
title: "Lastenausgleich für einen Cluster mit dem Clusterressourcen-Manager von Azure Service Fabric | Microsoft Docs"
description: "Eine Einführung in den Lastenausgleich für einen Cluster mit dem Clusterressourcen-Manager von Service Fabric."
services: service-fabric
documentationcenter: .net
author: masnider
manager: timlt
editor: 
ms.assetid: 030b1465-6616-4c0b-8bc7-24ed47d054c0
ms.service: Service-Fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 08/19/2016
ms.author: masnider
translationtype: Human Translation
ms.sourcegitcommit: dcda8b30adde930ab373a087d6955b900365c4cc
ms.openlocfilehash: d278e0125d698306366713a6e11957dad2de10c5


---
# <a name="balancing-your-service-fabric-cluster"></a>Lastenausgleich für Service Fabric-Cluster
Der Clusterressourcen-Manager von Service Fabric ermöglicht Folgendes: Melden dynamischer Lasten, Reagieren auf Änderungen im Cluster, Korrigieren von Einschränkungsverletzungen sowie bei Bedarf das Ausführen eines Lastenausgleichs für den Cluster. Doch wie häufig werden diese Vorgänge ausgeführt, und wodurch werden sie ausgelöst? Hierfür gibt es mehrere Steuerungsmöglichkeiten.

Die ersten Steuerelemente im Zusammenhang mit dem Lastenausgleich sind eine Reihe von Timern. Diese Timer legen fest, wie oft der Clusterressourcen-Manager den Zustand des Clusters überprüft, um Situationen zu ermitteln, die Maßnahmen erfordern. Es gibt drei verschiedene Kategorien von Tasks, für die jeweils ein Timer verfügbar ist. Sie lauten wie folgt:

1. Platzierung – In dieser Phase erfolgt die Platzierung von zustandsbehafteten und zustandslosen Instanzen, die fehlen. Dies umfasst sowohl neue Dienste als auch die Behandlung von zustandsbehafteten Replikaten oder zustandslosen Instanzen, die ausgefallen sind und neu erstellt werden müssen. Das Löschen und Ablegen von Replikaten oder Instanzen erfolgt auch in dieser Phase.
2. Einschränkungsüberprüfungen – In dieser Phase erfolgt eine Überprüfung auf und Korrektur bei Verstößen gegen verschiedene Platzierungseinschränkungen (Regeln) im System. Beispiele hierfür sind z. B. das Sicherstellen, dass Knoten nicht ihre Kapazität überschreiten und die Platzierungseinschränkungen eines Diensts erfüllt werden (mehr dazu weiter unten).
3. Lastenausgleich – In dieser Phase wird geprüft, ob basierend auf dem gewünschten Ausgleichsgrad für verschiedene Metriken ein proaktiver erneuter Ausgleich erforderlich ist. Falls ja, wird versucht, eine ausgeglichenere Anordnung im Cluster zu finden.

## <a name="configuring-cluster-resource-manager-steps-and-timers"></a>Konfigurieren von Schritten und Timern für den Clusterressourcen-Manager
Für all diese Typen von Korrekturen, die der Clusterressourcen-Manager durchführen kann, sind unterschiedliche Timer verfügbar, mit denen festgelegt wird, wie häufig die Vorgänge ausgeführt werden. Wenn Sie z.B. stündlich neue Dienstworkloads im Cluster platzieren möchten (um diese zu Batches zusammenzufassen), jedoch regelmäßig alle paar Sekunden Lastenausgleichsprüfungen wünschen, können Sie dieses Verhalten konfigurieren. Beim Auslösen der einzelnen Timer wird der Task geplant. Standardmäßig überprüft der Ressourcen-Manager jede Zehntelsekunde seinen Zustand und wendet Updates (Zusammenfassung aller Änderungen seit der letzten Prüfung, z.B. das Ermitteln eines ausgefallenen Knotens) ebenfalls mit diesem Intervall an. Die Kennzeichen für die Platzierungs- und Einschränkungsüberprüfung werden auf „Jede Sekunde“, das Kennzeichen für den Ausgleich auf „Alle 5 Sekunden“ festgelegt.

ClusterManifest.xml

``` xml
        <Section Name="PlacementAndLoadBalancing">
            <Parameter Name="PLBRefreshGap" Value="0.1" />
            <Parameter Name="MinPlacementInterval" Value="1.0" />
            <Parameter Name="MinConstraintCheckInterval" Value="1.0" />
            <Parameter Name="MinLoadBalancingInterval" Value="5.0" />
        </Section>
```

Aktuell wird jeweils nur eine dieser Aktionen ausgeführt, sodass die Aktionen nacheinander ausgeführt werden (aus diesem Grund werden diese Konfigurationen als „Mindestintervalle“ bezeichnet). Der Grund dafür ist, dass Sie z.B. zunächst auf Anforderungen zum Erstellen neuer Replikate reagieren, bevor Sie mit einem Ausgleich des Clusters fortfahren. Wie Sie anhand der angegebenen standardmäßigen Zeitintervalle erkennen, können wir sehr schnell eine Überprüfung auf alle erforderlichen Schritte durchführen. Das heißt, dass die Änderungen, die wir am Ende jedes Schritts durchführen, in der Regel weniger umfangreich sind: Wir überprüfen also nicht eine große Menge an Änderungen, die über mehrere Stunden hinweg im Cluster vorgenommen wurden, um dann zu versuchen, alle Änderungen auf einmal zu korrigieren. Stattdessen wird versucht, sofort auf Vorkommnisse zu reagieren. Wenn jedoch viele Änderungen gleichzeitig vorgenommen werden, erfolgt die Korrektur für eine Gruppe von Vorkommnissen. Dadurch kann der Ressourcen-Manager von Service Fabric sehr schnell auf Ereignisse im Cluster reagieren.

Wenngleich die meisten dieser Tasks unkompliziert sind (wenn Einschränkungsverletzungen vorliegen, werden sie korrigiert, wenn Dienste erstellt werden müssen, werden sie erstellt), benötigt der Clusterressourcen-Manager einige zusätzliche Informationen, um zu bestimmen, ob für den Cluster ein Ausgleich vorgenommen werden muss. Hierfür gibt es zwei weitere Konfigurationseinstellungen: *Ausgleichsschwellenwerte* und *Aktivitätsschwellenwerte*.

## <a name="balancing-thresholds"></a>Ausgleichsschwellenwerte
Ein Ausgleichsschwellenwert ist das wesentliche Steuerelement, um einen proaktiven Ausgleich auszulösen (bedenken Sie, dass der Timer lediglich bestimmt, wie oft der Clusterressourcen-Manager den Cluster überprüft. Durch den Timer selbst werden keine Schritte ausgelöst). Der Ausgleichsschwellenwert legt fest, welche Bedingungen für eine bestimmte Metrik erfüllt sein müssen, damit der Clusterressourcen-Manager den Cluster als nicht ausgeglichen einstuft und einen Ausgleich auslöst.

Ausgleichsschwellenwerte werden als Teil der Clusterdefinition metrikbezogen definiert. Weitere Informationen zu Metriken finden Sie in [diesem Artikel](service-fabric-cluster-resource-manager-metrics.md).

ClusterManifest.xml

``` xml
    <Section Name="MetricBalancingThresholds">
      <Parameter Name="MetricName1" Value="2"/>
      <Parameter Name="MetricName2" Value="3.5"/>
    </Section>
```

Der Ausgleichsschwellenwert für eine Metrik ist ein Verhältnis. Wenn die Last auf dem am stärksten ausgelasteten Knoten dividiert durch die Last auf dem am wenigsten ausgelasteten Knoten diesen Wert überschreitet, gilt der Cluster als nicht ausgeglichen. Bei der nächsten Überprüfung durch den Clusterressourcen-Manager (über MinLoadBalancingInterval standardmäßig auf ein Intervall von 5 Sekunden festgelegt, siehe oben) wird ein Ausgleich ausgelöst.

![Beispiel eines Ausgleichsschwellenwerts][Image1]

Bei diesem einfachen Beispiel belegt jeder Dienst eine Einheit einer bestimmten Metrik. Im Beispiel oben ist die maximale Auslastung auf einem Knoten 5 und die minimale Auslastung 2. Angenommen, der Ausgleichsschwellenwert für diese Metrik ist 3. Daher gilt der Cluster im Beispiel oben als ausgeglichen, sodass bei der nächsten Überprüfung durch den Clusterressourcen-Manager kein Lastenausgleich ausgelöst wird (das Verhältnis im Cluster beträgt 5/2 = 2,5, der Wert ist also kleiner als der angegebene Ausgleichsschwellenwert 3).

Im Beispiel unten ist die maximale Auslastung auf einem Knoten 10, während die Mindestauslastung 2 ist (was ein Verhältnis von 5 ergibt). Der Cluster überschreitet also den vorgesehenen Ausgleichsschwellenwert 3 für diese Metrik. Folglich wird beim nächsten Auslösen des Ausgleichstimers ein globaler Ausgleich geplant. Beachten Sie, dass allein durch die Suche zum Durchführen eines Ausgleichs noch keine Elemente verschoben werden. In einigen Fällen ist der Cluster nicht ausgeglichen, ohne dass die Situation verbessert werden kann. In einer Situation wie dieser hier wird jedoch (zumindest per Standardkonfiguration) mit ziemlicher Sicherheit ein Teil der Last auf Node3 verschoben. Da wir keinen „gierigen“ Ansatz befolgen, könnte ein Teil der Last auch auf Node2 verteilt werden, da dies zu einer Minimierung des allgemeinen Ungleichgewichts zwischen Knoten führen würde. Wir würden jedoch davon ausgehen, dass der größte Teil der Last auf Node3 platziert würde.

![Ausgleichsschwellenwert – Beispielaktionen][Image2]

Beachten Sie, dass die Unterschreitung des Ausgleichsschwellenwerts kein explizites Ziel ist – Ausgleichsschwellenwerte sind lediglich *Trigger*, die den Clusterressourcen-Manager darüber informieren, dass der Cluster geprüft werden sollte, um ggf. Verbesserungen vorzunehmen.

## <a name="activity-thresholds"></a>Aktivitätsschwellenwerte
Mitunter ist die *Gesamtlast* des Clusters niedrig, obwohl Knoten relativ unausgeglichen sind. Der Grund hierfür kann bloß die Tageszeit sein oder dass der Cluster neu ist und einem Bootstrapping unterzogen wird. In beiden Fällen sollten Sie auf einen Lastenausgleich für den Cluster verzichten, da der Nutzen sehr gering ist und Sie lediglich viel Zeit für das Verschieben von Netzwerk- und Computeressourcen aufwenden, ohne dass dies zu einer wirklichen Veränderung führt. Um dies zu verhindern, gibt es im Ressourcen-Manager mit Aktivitätsschwellenwerten ein weiteres Steuerelement, mit dem Sie eine absolute Untergrenze für eine Aktivität angeben können. Wenn ein Knoten nicht mindestens diese Last aufweist, wird kein Ausgleich ausgelöst, selbst wenn der Ausgleichsschwellenwert erreicht wird.

Nehmen wir als Beispiel Berichte mit den folgenden Summen für die Nutzung auf diesen Knoten. Nehmen wir außerdem an, dass wir unseren Ausgleichsschwellenwert 3 für diese Metrik beibehalten, aber zusätzlich auch den Aktivitätsschwellenwert 1536 festgelegt haben. Obgleich der Cluster im ersten Fall laut Ausgleichsschwellenwert unausgeglichen ist, erreicht kein Knoten den Mindestaktivitätsschwellenwert, sodass wir nicht eingreifen. Im unteren Beispiel wird der Aktivitätsschwellenwert von Node1 erheblich überschritten, sodass ein Ausgleich durchgeführt wird (sowohl der Ausgleichsschwellenwert als auch der Aktivitätsschwellenwert für die Metrik werden überschritten).

![Beispiel eines Aktivitätsschwellenwerts][Image3]

Aktivitätsschwellenwerte werden wie Ausgleichsschwellenwerte metrikbezogen in der Clusterdefinition definiert:

ClusterManifest.xml

``` xml
    <Section Name="MetricActivityThresholds">
      <Parameter Name="Memory" Value="1536"/>
    </Section>
```

Sowohl der Ausgleichs- als auch der Aktivitätsschwellenwert sind mit der Metrik verknüpft. Der Ausgleich wird nur ausgelöst, wenn sowohl der Ausgleichs- als auch der Aktivitätsschwellenwert für die gleiche Metrik überschritten werden. Folglich gilt: Wenn der Ausgleichsschwellenwert für den Arbeitsspeicher und der Aktivitätsschwellenwert für die CPU überschritten werden, wird kein Ausgleich mehr ausgelöst, solange die verbleibenden Schwellenwerte (Ausgleichsschwellenwert für die CPU und Aktivitätsschwellenwert für den Arbeitsspeicher) nicht überschritten werden.

## <a name="balancing-services-together"></a>Gemeinsamer Lastenausgleich von Diensten
Ein interessanter Hinweis ist, dass die Entscheidung, ob der Cluster unausgeglichen ist oder nicht, clusterweit gefällt wird. Doch die Korrektur darin besteht, einzelne Dienstreplikate und -instanzen zu verschieben. Das ist einleuchtend, nicht wahr? Wenn auf einem Knoten ein zu hoher Arbeitsspeicherwert erreicht wird, können mehrere Replikate oder Instanzen dazu beitragen. Daher kann es erforderlich sein, zustandsbehaftete Replikate oder zustandslose Instanzen zu verschieben, die die betroffene, unausgeglichene Metrik nutzen.

Gelegentlich aber ruft uns ein Kunde an oder schickt uns ein Ticket, das besagt, dass ein nicht unausgeglichener Dienst verschoben wurde. Wie kann es passieren, dass ein Dienst verschoben wird, obwohl alle Metriken dieses Diensts zum Zeitpunkt der anderen Diskrepanz (geradezu perfekt) ausgeglichen waren? Das wollen wir uns einmal ansehen.

Sehen wir uns folgendes Beispiel mit den Diensten Service1, Service2, Service3 und Service4 an. Service1 meldet Werte für die Metriken Metric1 und Metric2, Service2 für die Metriken Metric2 und Metric3, Service3 für die Metriken Metric3 und Metric4 und Service4 für die Metrik Metric99. Sicherlich können Sie erkennen, in welche Richtung wir hier gehen. Wir haben eine Kette! Aus Sicht des Clusterressourcen-Managers verfügen wir nicht wirklich über vier unabhängige Dienste, sondern vielmehr über eine Reihe von Diensten, die in Bezug zueinander stehen (Service1, Service2 und Service3), sowie über einen weiteren Dienst, der unabhängig ist.

![Gemeinsamer Lastenausgleich von Diensten][Image4]

Daher ist es möglich, dass ein Ungleichgewicht bei Metric1 bewirken kann, dass Replikate oder Instanzen, die zu Service3 gehören, verschoben werden. In der Regel sind diese Verschiebungen eher begrenzt, können aber umfassender sein, was genau davon abhängt, wie unausgeglichen Metrik 1 geworden ist und welche Änderungen im Cluster für die Korrektur erforderlich waren. Wir können auch mit Sicherheit sagen, dass ein Ungleichgewicht bei den Metriken 1, 2 oder 3 nie Verschiebungen in Dienst 4 bewirkt. Dies wäre sinnlos, da das Verschieben von Replikaten oder Instanzen, die zu Dienst 4 gehören, keinerlei Auswirkung auf die Ausgeglichenheit der Metriken 1, 2 oder 3 hat.

Der Clusterressourcen-Manager ermittelt automatisch, welche Dienste in Beziehung stehen, da Dienste ggf. hinzugefügt oder entfernt werden oder sich die Konfiguration ihrer Metriken geändert haben könnte. Zwischen zwei Ausführungen des Lastenausgleichs könnte Service2 z.B. so neu konfiguriert worden sein, dass Metric2 entfernt wurde. Dadurch wird die Kette zwischen Dienst 1 und Dienst 2 unterbrochen. Dann haben Sie anstelle von zwei Dienstgruppen drei:

![Gemeinsamer Lastenausgleich von Diensten][Image5]

## <a name="next-steps"></a>Nächste Schritte
* Metriken bestimmen, wie der Clusterressourcen-Manager von Service Fabric den Ressourcenverbrauch und die Kapazität im Cluster verwaltet. Weitere Informationen zu Metriken und deren Konfiguration finden Sie in [diesem Artikel](service-fabric-cluster-resource-manager-metrics.md)
* Bewegungskosten sind eine Möglichkeit, dem Clusterressourcen-Manager mitzuteilen, dass bestimmte Dienste teurer zu bewegen sind als andere. Weitere Informationen zu Bewegungskosten finden Sie in [diesem Artikel](service-fabric-cluster-resource-manager-movement-cost.md)
* Der Clusterressourcen-Manager bietet mehrere Drosselungen, die Sie konfigurieren können, um Änderungen im Cluster zu verlangsamen. Sie sind normalerweise nicht erforderlich, aber bei Bedarf finden Sie [hier](service-fabric-cluster-resource-manager-advanced-throttling.md)

[Image1]:./media/service-fabric-cluster-resource-manager-balancing/cluster-resrouce-manager-balancing-thresholds.png
[Image2]:./media/service-fabric-cluster-resource-manager-balancing/cluster-resource-manager-balancing-threshold-triggered-results.png
[Image3]:./media/service-fabric-cluster-resource-manager-balancing/cluster-resource-manager-activity-thresholds.png
[Image4]:./media/service-fabric-cluster-resource-manager-balancing/cluster-resource-manager-balancing-services-together1.png
[Image5]:./media/service-fabric-cluster-resource-manager-balancing/cluster-resource-manager-balancing-services-together2.png



<!--HONumber=Dec16_HO2-->


