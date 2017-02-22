---
title: Ausgleichen Ihres Azure Service Fabric-Clusters | Microsoft-Dokumentation
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
ms.date: 01/05/2017
ms.author: masnider
translationtype: Human Translation
ms.sourcegitcommit: bb27d279396aa7b670187560cebe2ed074576bad
ms.openlocfilehash: ee77a01b2bb17ab70099e891e00255d1cec676f6


---
# <a name="balancing-your-service-fabric-cluster"></a>Lastenausgleich für Service Fabric-Cluster
Der Clusterressourcen-Manager von Service Fabric unterstützt dynamische Laständerungen, Reaktionen auf hinzugefügte oder entfernte Knoten/Dienste, die Korrektur von Einschränkungsverletzungen sowie den Ausgleich des Clusters. Doch wie häufig werden diese Vorgänge ausgeführt, und wodurch werden sie ausgelöst?

Die ersten Steuerelemente im Zusammenhang mit dem Lastenausgleich sind eine Reihe von Timern. Diese Timer legen fest, wie oft der Clusterressourcen-Manager den Zustand des Clusters überprüft, um Situationen zu ermitteln, die Maßnahmen erfordern. Es gibt drei verschiedene Kategorien von Tasks, für die jeweils ein Timer verfügbar ist. Sie lauten wie folgt:

1. Platzierung – In dieser Phase erfolgt die Platzierung zustandsbehafteter Replikate und zustandsloser Instanzen, die ggf. fehlen. Dies umfasst sowohl neue Dienste als auch die Behandlung von zustandsbehafteten Replikaten oder zustandslosen Instanzen, die ausgefallen sind. Hier werden das Löschen und Verwerfen von Replikaten oder Instanzen behandelt.
2. Einschränkungsüberprüfungen – In dieser Phase erfolgt eine Überprüfung auf und Korrektur bei Verstößen gegen verschiedene Platzierungseinschränkungen (Regeln) im System. Dadurch kann beispielsweise sichergestellt werden, dass Knoten ihre Kapazität nicht überschreiten und die Platzierungseinschränkungen eines Diensts eingehalten werden.
3. Ausgleich – In dieser Phase wird auf der Grundlage des gewünschten Ausgleichsgrads für verschiedene Metriken geprüft, ob ein proaktiver erneuter Ausgleich erforderlich ist. Falls ja, wird versucht, eine ausgeglichenere Anordnung im Cluster zu finden.

## <a name="configuring-cluster-resource-manager-steps-and-timers"></a>Konfigurieren von Schritten und Timern für den Clusterressourcen-Manager
Jede dieser unterschiedlichen Korrekturarten, die der Clusterressourcen-Manager durchführen kann, wird durch einen anderen Timer gesteuert, um festzulegen, wie häufig die Vorgänge ausgeführt werden. Beim Auslösen der einzelnen Timer wird der Task geplant. Der Ressourcen-Manager führt standardmäßig folgende Schritte aus:

* Er überprüft seinen Zustand und wendet jede Zehntelsekunde Updates an (beispielsweise, um zu erfassen, dass ein Knoten inaktiv ist).
* Er legt sekündlich die Kennzeichen für die Platzierungs- und Einschränkungsüberprüfung fest.
* Er legt alle fünf Sekunden das Kennzeichen für den Ausgleich fest.

Dies ist in den folgenden Konfigurationsinformationen zu sehen:

ClusterManifest.xml

``` xml
        <Section Name="PlacementAndLoadBalancing">
            <Parameter Name="PLBRefreshGap" Value="0.1" />
            <Parameter Name="MinPlacementInterval" Value="1.0" />
            <Parameter Name="MinConstraintCheckInterval" Value="1.0" />
            <Parameter Name="MinLoadBalancingInterval" Value="5.0" />
        </Section>
```

Über „ClusterConfig.json“ für eigenständige Bereitstellungen oder „Template.json“ für in Azure gehostet Cluster:

```json
"fabricSettings": [
  {
    "name": "PlacementAndLoadBalancing",
    "parameters": [
      {
          "name": "PLBRefreshGap",
          "value": "0.10"
      },
      {
          "name": "MinPlacementInterval",
          "value": "1.0"
      },
      {
          "name": "MinLoadBalancingInterval",
          "value": "5.0"
      }
    ]
  }
]
```

Aktuell werden die Aktionen vom Clusterressourcen-Manager einzeln nacheinander ausgeführt. (Die Timer werden daher als „Mindestintervalle“ bezeichnet.) So verarbeitet der Clusterressourcen-Manager beispielsweise zunächst ausstehende Anforderungen für die Diensterstellung und gleicht erst dann den Cluster aus. Wie anhand der angegebenen Standardintervalle zu sehen, ermittelt und überprüft der Clusterressourcen-Manager in kurzen Abständen, ob etwas zu tun ist. Daher ist die Liste mit den Änderungen, die am Ende der einzelnen Schritte vorgenommen wurden, in der Regel eher kurz. Dank des Konzepts, in kurzen Abständen geringfügige Änderungen vorzunehmen, kann der Clusterressourcen-Manager besser auf Vorgänge im Cluster reagieren. Die Standardtimer ermöglichen eine gewisse Batchverarbeitung, da viele der gleichen Ereignisarten häufig gleichzeitig auftreten. Standardmäßig geht der Clusterressourcen-Manager nicht die Clusteränderungen mehrerer Stunden durch und versucht, alle gleichzeitig zu behandeln. Dies hätte immer wieder kurze Phasen mit hohem Änderungsaufkommen zur Folge.

Der Clusterressourcen-Manager benötigt außerdem einige zusätzliche Informationen, um zu ermitteln, ob der Cluster unausgeglichen ist. Hierfür gibt es zwei weitere Konfigurationseinstellungen: *Ausgleichsschwellenwerte* und *Aktivitätsschwellenwerte*.

## <a name="balancing-thresholds"></a>Ausgleichsschwellenwerte
Ein Ausgleichsschwellenwert ist das Hauptsteuerinstrument für das Auslösen eines proaktiven erneuten Ausgleichs. Der MinLoadBalancingInterval-Timer gibt lediglich an, wie oft der Clusterressourcen-Manager die Überprüfung durchführen soll. Das heißt noch nicht, dass etwas passiert. Der Ausgleichsschwellenwert legt fest, welche Bedingungen für eine bestimmte Metrik erfüllt sein müssen, damit der Clusterressourcen-Manager den Cluster als nicht ausgeglichen einstuft und einen Ausgleich auslöst.

Ausgleichsschwellenwerte werden als Teil der Clusterdefinition metrikbezogen definiert. Weitere Informationen zu Metriken finden Sie in [diesem Artikel](service-fabric-cluster-resource-manager-metrics.md).

ClusterManifest.xml

```xml
    <Section Name="MetricBalancingThresholds">
      <Parameter Name="MetricName1" Value="2"/>
      <Parameter Name="MetricName2" Value="3.5"/>
    </Section>
```

Über „ClusterConfig.json“ für eigenständige Bereitstellungen oder „Template.json“ für in Azure gehostet Cluster:

```json
"fabricSettings": [
  {
    "name": "MetricBalancingThresholds",
    "parameters": [
      {
          "name": "MetricName1",
          "value": "2"
      },
      {
          "name": "MetricName2",
          "value": "3.5"
      }
    ]
  }
]
```

Der Ausgleichsschwellenwert für eine Metrik ist ein Verhältnis. Wenn das Maß der Auslastung auf dem am stärksten ausgelasteten Knoten dividiert durch das Maß der Auslastung auf dem am wenigsten ausgelasteten Knoten diesen Wert überschreitet, gilt der Cluster als unausgeglichen. Bei der nächsten Ausführung des Clusterressourcen-Managers wird deshalb ein Ausgleich ausgelöst.

<center>
![Beispiel mit einem Ausgleichsschwellenwert][Image1]
</center>

In diesem Beispiel verbraucht jeder Dienst eine einzelne Einheit einer bestimmten Metrik. Im oberen Beispiel hat die maximale Auslastung auf einem Knoten den Wert „5“ und die minimale Auslastung den Wert „2“. Angenommen, der Ausgleichsschwellenwert für diese Metrik ist „3“. Da das Verhältnis des Clusters 5/2 (also 2,5) beträgt und damit kleiner als der angegebene Schwellenwert (3) ist, ist der Cluster ausgeglichen. Bei der Überprüfung durch den Clusterressourcen-Manager wird daher kein Ausgleich ausgelöst.

Im unteren Beispiel hat die maximale Auslastung auf einem Knoten den Wert „10“ und die minimale Auslastung den Wert „2“. Für das Verhältnis ergibt sich also der Wert „5“. Der Wert „5“ übersteigt den für die Metrik festgelegten Ausgleichsschwellenwert (3). Folglich wird beim nächsten Auslösen des Ausgleichstimers ein Ausgleich geplant. In einer derartigen Situation wird ein Teil der Auslastung mit hoher Wahrscheinlichkeit an „Node3“ verteilt. Da der Clusterressourcen-Manager von Service Fabric keinen „gierigen“ Ansatz verfolgt, wird ein Teil der Auslastung unter Umständen auch an „Node2“ verteilt. Dies führt insgesamt zu einer Minimierung der Unterschiede zwischen Knoten, was ja eines der Ziele des Clusterressourcen-Managers ist.

<center>
![Ausgleichsschwellenwert – Beispielaktionen][Image2]
</center>

Die Unterschreitung des Ausgleichsschwellenwerts ist kein explizites Ziel. Ausgleichsschwellenwerte sind lediglich *Trigger*, die den Clusterressourcen-Manager darüber informieren, dass der Cluster überprüft werden sollte, um ggf. Verbesserungen vorzunehmen. Allein durch den Suchvorgang für einen Ausgleich werden allerdings noch keine Elemente verschoben. Manchmal ist der Cluster nicht ausgeglichen, ohne dass die Situation verbessert werden kann.

## <a name="activity-thresholds"></a>Aktivitätsschwellenwerte
Mitunter ist die *Gesamtlast* des Clusters niedrig, obwohl Knoten relativ unausgeglichen sind. Dies kann auf einen vorübergehenden Rückgang zurückzuführen sein oder daran liegen, dass der Cluster neu ist und einem Bootstrapping-Vorgang unterzogen wird. In beiden Fällen empfiehlt es sich unter Umständen, von einem Ausgleich des Clusters abzusehen, da sich damit nur wenig erreichen lässt. Wenn der Cluster ausgeglichen wurde, würden Sie lediglich Netzwerk- und Computeressourcen beanspruchen, um Elemente zu verschieben, ohne jedoch einen *absoluten* Unterschied zu erzielen. Dies lässt sich durch so genannte Aktivitätsschwellenwerte vermeiden. Mithilfe von Aktivitätsschwellenwerten können Sie eine absolute Untergrenze für Aktivitäten angeben. Wird der Schwellenwert von keinem Knoten überschritten, wird auch bei Erreichen des Ausgleichsschwellenwerts kein Ausgleich ausgelöst.

Sehen Sie sich zur Veranschaulichung das Beispieldiagramm weiter unten an. Angenommen, wir behalten für diese Metrik den Ausgleichsschwellenwert „3“ bei, verwenden aber zusätzlich den Aktivitätsschwellenwert „1536“. Im ersten Fall ist der Cluster zwar laut Ausgleichsschwellenwert unausgeglichen, der Aktivitätsschwellenwert wird jedoch von keinem Knoten erreicht, sodass keine Maßnahmen ergriffen werden. Im unteren Beispiel überschreitet „Node1“ den Aktivitätsschwellenwert deutlich. Da sowohl der Ausgleichsschwellenwert als auch der Aktivitätsschwellenwert für die Metrik überschritten werden, wird ein Ausgleich geplant.

<center>
![Beispiel mit einem Aktivitätsschwellenwert][Image3]
</center>

Aktivitätsschwellenwerte werden wie Ausgleichsschwellenwerte metrikbezogen in der Clusterdefinition definiert:

ClusterManifest.xml

``` xml
    <Section Name="MetricActivityThresholds">
      <Parameter Name="Memory" Value="1536"/>
    </Section>
```

Über „ClusterConfig.json“ für eigenständige Bereitstellungen oder „Template.json“ für in Azure gehostet Cluster:

```json
"fabricSettings": [
  {
    "name": "MetricActivityThresholds",
    "parameters": [
      {
          "name": "Memory",
          "value": "1536"
      }
    ]
  }
]
```

Sowohl der Ausgleichs- als auch der Aktivitätsschwellenwert sind mit einer bestimmten Metrik verknüpft. Der Ausgleich wird nur ausgelöst, wenn sowohl der Ausgleichs- als auch der Aktivitätsschwellenwert für die gleiche Metrik überschritten werden.

## <a name="balancing-services-together"></a>Gemeinsamer Lastenausgleich von Diensten
Ob der Cluster unausgeglichen ist oder nicht, wird auf der Clusterebene entschieden. Die Korrektur besteht jedoch darin, einzelne Dienstreplikate und -instanzen zu verschieben. Das ist einleuchtend, nicht wahr? Wenn auf einem Knoten ein hoher Arbeitsspeicherwert erreicht wird, kann dies auf mehrere Replikate oder Instanzen zurückzuführen sein. Daher kann es erforderlich sein, zustandsbehaftete Replikate oder zustandslose Instanzen zu verschieben, die die betroffene, unausgeglichene Metrik nutzen.

Gelegentlich wird jedoch auch ein Dienst verschoben, der gar nicht unausgeglichen war. Wie kann es passieren, dass ein Dienst verschoben wird, obwohl alle Metriken dieses Diensts zum Zeitpunkt der anderen Diskrepanz (geradezu perfekt) ausgeglichen waren? Das wollen wir uns einmal ansehen.

Sehen wir uns folgendes Beispiel mit den Diensten Service1, Service2, Service3 und Service4 an. Service1 meldet Werte für die Metriken Metric1 und Metric2, Service2 für die Metriken Metric2 und Metric3, Service3 für die Metriken Metric3 und Metric4 und Service4 für die Metrik Metric99. Sicherlich können Sie erkennen, in welche Richtung wir hier gehen. Wir haben eine Kette! Wir verfügen nicht wirklich über vier unabhängige Dienste, sondern vielmehr über eine Reihe von Diensten, die in Bezug zueinander stehen („Service1“, „Service2“ und „Service3“), sowie über einen weiteren Dienst, der unabhängig ist.

<center>
![Gemeinsames Ausgleichen von Diensten][Image4]
</center>

Ein Ungleichgewicht bei „Metric1“ kann daher zu einer Verschiebung von Replikaten oder Instanzen führen, die zu „Service3“ gehören (der „Metric1“ nicht meldet). In der Regel sind diese Verschiebungen begrenzt, sie können aber auch umfassender ausfallen. Dies hängt davon ab, wie unausgeglichen „Metric1“ geworden ist und welche Korrekturmaßnahmen im Cluster erforderlich waren. Wir können auch mit Sicherheit sagen, dass ein Ungleichgewicht bei der Metrik 1, 2 oder 3 nie Verschiebungen in „Service4“ bewirkt. Das wäre sinnlos, da das Verschieben von Replikaten oder Instanzen, die zu „Service4“ gehören, keinerlei Auswirkung auf die Ausgeglichenheit der Metrik 1, 2 oder 3 hat.

Der Clusterressourcen-Manager ermittelt automatisch, welche Dienste in Beziehung stehen, da Dienste ggf. hinzugefügt oder entfernt werden oder sich die Konfiguration ihrer Metriken geändert hat. Zwischen zwei Ausführungen des Ausgleichs kann beispielsweise „Service2“ so konfiguriert worden sein, dass „Metric2“ entfernt wird. Dadurch wird die Kette zwischen „Service1“ und „Service2“ unterbrochen. Anstelle von zwei zusammenhängenden Dienstgruppen haben Sie nun drei:

<center>
![Gemeinsames Ausgleichen von Diensten][Image5]
</center>

## <a name="next-steps"></a>Nächste Schritte
* Metriken bestimmen, wie der Clusterressourcen-Manager von Service Fabric den Ressourcenverbrauch und die Kapazität im Cluster verwaltet. Weitere Informationen zu Metriken und deren Konfiguration finden Sie in [diesem Artikel](service-fabric-cluster-resource-manager-metrics.md).
* Bewegungskosten sind eine Möglichkeit, dem Clusterressourcen-Manager mitzuteilen, dass bestimmte Dienste teurer zu bewegen sind als andere. Weitere Informationen zu Bewegungskosten finden Sie in [diesem Artikel](service-fabric-cluster-resource-manager-movement-cost.md).
* Der Clusterressourcen-Manager bietet mehrere Drosselungen, die Sie konfigurieren können, um Änderungen im Cluster zu verlangsamen. Sie sind normalerweise nicht erforderlich, aber bei Bedarf finden Sie [hier](service-fabric-cluster-resource-manager-advanced-throttling.md)

[Image1]:./media/service-fabric-cluster-resource-manager-balancing/cluster-resrouce-manager-balancing-thresholds.png
[Image2]:./media/service-fabric-cluster-resource-manager-balancing/cluster-resource-manager-balancing-threshold-triggered-results.png
[Image3]:./media/service-fabric-cluster-resource-manager-balancing/cluster-resource-manager-activity-thresholds.png
[Image4]:./media/service-fabric-cluster-resource-manager-balancing/cluster-resource-manager-balancing-services-together1.png
[Image5]:./media/service-fabric-cluster-resource-manager-balancing/cluster-resource-manager-balancing-services-together2.png



<!--HONumber=Jan17_HO4-->


