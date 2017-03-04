---
title: "Resource Balancer – Clusterbeschreibung| Microsoft Docs"
description: "Beschreibung eines Service Fabric-Clusters durch Angabe von Fehlerdomänen, Upgradedomänen, Knoteneigenschaften und Knotenkapazitäten für den Clusterressourcen-Manager."
services: service-fabric
documentationcenter: .net
author: masnider
manager: timlt
editor: 
ms.assetid: 55f8ab37-9399-4c9a-9e6c-d2d859de6766
ms.service: Service-Fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 01/05/2017
ms.author: masnider
translationtype: Human Translation
ms.sourcegitcommit: dafaf29b6827a6f1c043af3d6bfe62d480d31ad5
ms.openlocfilehash: a595a2e24b4c2582998d88f74ca13daba5fe618d
ms.lasthandoff: 02/17/2017

---

# <a name="describing-a-service-fabric-cluster"></a>Beschreiben eines Service Fabric-Clusters
Der Service Fabric-Cluster-Ressourcen-Manager stellt verschiedene Methoden zum Beschreiben eines Clusters bereit. Zur Laufzeit nutzt der Clusterressourcen-Manager diese Informationen, um die hohe Verfügbarkeit der Dienste sicherzustellen, die im Cluster ausgeführt werden. Beim Erzwingen dieser wichtigen Regeln wird auch versucht, den Ressourcenverbrauch des Clusters zu optimieren.

## <a name="key-concepts"></a>Wichtige Begriffe
Der Clusterressourcen-Manager unterstützt verschiedene Funktionen, die einen Cluster beschreiben:

* Fehlerdomänen
* Upgradedomänen
* Knoteneigenschaften
* Knotenkapazitäten

## <a name="fault-domains"></a>Fehlerdomänen
Eine Fehlerdomäne ist ein beliebiger Bereich mit einem koordinierten Ausfall. Ein einzelner Computer ist eine Fehlerdomäne (da er aus vielen unterschiedlichen Gründen ausfallen kann, z.B. infolge eines Stromausfalls oder aufgrund von Laufwerkfehlern oder fehlerhafter NIC-Firmware). Computer, die mit dem gleichen Ethernet-Switch verbunden sind, befinden sich in derselben Fehlerdomäne – genau wie Computer, die eine Stromquelle gemeinsam nutzen. Da sich Hardwarefehler natürlicherweise überschneiden, sind Fehlerdomänen inhärent hierarchisch und werden in Service Fabric als URIs dargestellt.

Wenn Sie Ihren eigenen Cluster einrichten, müssen Sie sich diese unterschiedlichen Fehlerbereiche sorgfältig klarmachen. Es ist wichtig, dass Fehlerdomänen richtig eingerichtet werden, da Service Fabric diese Informationen nutzt, um Dienste sicher zu platzieren. Für Service Fabric soll eine Platzierung von Diensten vermieden werden, bei der der Verlust einer Fehlerdomäne (aufgrund des Ausfalls einer Komponente) dazu führt, dass Dienste nicht mehr verfügbar sind. In der Azure-Umgebung nutzt Service Fabric die von der Umgebung bereitgestellten Fehlerdomäneninformationen, um die Knoten im Cluster in Ihrem Auftrag richtig zu konfigurieren.

In der Grafik unten sind alle Entitäten farbig markiert, die einen Beitrag zu Fehlerdomänen leisten, und alle unterschiedlichen Fehlerdomänen aufgeführt, die sich ergeben. Dieses Beispiel enthält Datencenter („DC“), Racks („R“) und Blades („B“). Wenn jedes Blatt also mehrere virtuelle Computer enthält, kann die Fehlerdomänenhierarchie eine weitere Ebene umfassen.

<center>
![Mithilfe von Fehlerdomänen strukturierte Knoten][Image1]
</center>

Zur Laufzeit berücksichtigt der Clusterressourcen-Manager von Service Fabric die Fehlerdomänen im Cluster und plant ein Layout.  Es wird versucht, die zustandsbehafteten Replikate oder zustandslosen Instanzen für einen bestimmten Dienst zu verteilen, damit sie sich in separaten Fehlerdomänen befinden. Mit diesem Prozess wird sichergestellt, dass bei einem Ausfall einer Fehlerdomäne (auf einer beliebigen Ebene der Hierarchie) die Verfügbarkeit des Diensts nicht gefährdet wird.

Für den Clusterressourcen-Manager von Service Fabric spielt es keine Rolle, wie viele Ebenen die Hierarchie der Fehlerdomänen umfasst. Es wird aber versucht sicherzustellen, dass sich der Verlust eines Teils der Hierarchie nicht auf die darauf ausgeführten Dienste auswirkt. Aus diesem Grund ist es am besten, wenn alle Ebenen auf jeder Tiefe der Fehlerdomänenhierarchie die gleiche Anzahl von Knoten enthält. Wenn die Ebenen ausgeglichen sind, wird verhindert, dass ein Teil der Hierarchie mehr Dienste als andere Teile aufweist. Eine andere Vorgehensweise würde zu Ungleichheiten bei der Auslastung von einzelnen Knoten führen und bewirken, dass der Ausfall bestimmter Domänen kritischer als für andere Domänen ist.

Wenn die „Struktur“ der Fehlerdomänen in Ihrem Cluster nicht ausgeglichen ist, ist es für den Clusterressourcen-Manager schwieriger, die beste Zuordnung für die Dienste zu ermitteln. Fehlerdomänenlayouts mit ungleichmäßiger Verteilung bedeuten, dass sich der Verlust einer bestimmten Domäne mehr als für andere Domänen auf die Verfügbarkeit des Clusters auswirken kann. Daher besteht für den Clusterressourcen-Manager ein Konflikt zwischen seinen beiden Zielen: Die Computer in der stark ausgelasteten Domäne sollen einerseits genutzt werden, indem Dienste darauf platziert werden, und andererseits sollen die Dienste so platziert werden, dass der Verlust einer Domäne nicht zu Problemen führt. Was ist hier zu erkennen?

<center>
![Zwei unterschiedliche Clusterlayouts][Image2]
</center>

In der Abbildung oben sind zwei unterschiedliche Clusterlayouts als Beispiele dargestellt. Im ersten Beispiel werden die Knoten gleichmäßig auf die Fehlerdomänen verteilt. Im anderen Beispiel verfügt eine Fehlerdomäne über deutlich mehr Knoten. Wenn Sie einen eigenen Cluster lokal oder in einer anderen Umgebung einrichten, müssen Sie diese Diskrepanzen berücksichtigen.

In Azure wird die Wahl, welche Fehlerdomäne einen Knoten enthält, für Sie verwaltet. Je nach der Anzahl von Knoten, die Sie bereitstellen, kann es trotzdem dazu kommen, dass einige Fehlerdomänen mehr Knoten als andere enthalten. Angenommen, Sie haben fünf Fehlerdomänen, stellen für einen bestimmten NodeType aber sieben Knoten bereit. In diesem Fall verfügen die ersten beiden Fehlerdomänen über mehr Knoten. Das Problem verschlimmert sich, wenn Sie damit fortfahren, mehr NodeTypes mit nur wenigen Instanzen bereitzustellen.

## <a name="upgrade-domains"></a>Upgradedomänen
Upgradedomänen sind ein weiteres Feature, mit dem der Clusterressourcen-Manager von Service Fabric das Layout des Clusters nachvollziehen kann, sodass er im Voraus für Ausfälle planen kann. Mit Upgradedomänen werden Knotensätze definiert, die gleichzeitig aktualisiert werden.

Upgradedomänen sind Fehlerdomänen nicht unähnlich, es gibt jedoch einige wichtige Unterschiede. Während Fehlerdomänen streng anhand der Bereiche mit koordinierten Hardwarefehlern definiert werden, werden Upgradedomänen anhand einer Richtlinie definiert. Bei Upgradedomänen können Sie entscheiden, wie viele Sie verwenden möchten, und dieser Wert wird nicht von der Umgebung vorgegeben. Ein weiterer Unterschied (zumindest zum jetzigen Zeitpunkt) ist, dass Upgradedomänen nicht hierarchisch aufgebaut sind – sie ähneln eher einem einfachen Tag.

Das folgende Diagramm zeigt drei Upgradedomänen, die auf drei Fehlerdomänen verteilt sind. Außerdem ist eine mögliche Platzierung für drei unterschiedliche Replikate eines zustandsbehafteten Diensts dargestellt, die jeweils in unterschiedlichen Fehler- und Upgradedomänen angeordnet werden. Bei dieser Platzierung können wir den Ausfall einer Fehlerdomäne verkraften, während gerade ein Dienstupgrade durchgeführt wird, und verfügen trotzdem noch über eine Kopie des Codes und der Daten.

<center>
![Platzierung mit Fehler- und Upgradedomänen][Image3]
</center>

Die Verwendung einer größeren Zahl von Upgradedomänen hat Vor- und Nachteile. Mit jeder weiteren Upgradedomäne wird jeder Schritt des Upgrades präziser und wirkt sich daher auf eine kleinere Zahl von Knoten oder Diensten aus. Dies führt dazu, dass weniger Dienste auf einmal verschoben werden müssen, sodass es im System zu weniger Fluktuation kommt. Dies bewirkt meist auch eine Verbesserung der Zuverlässigkeit (da ein geringerer Anteil des Diensts von Problemen betroffen ist, die während des Upgrades auftreten). Eine höhere Zahl von Upgradedomänen bedeutet auch, dass Sie weniger verfügbare Ressourcen auf anderen Knoten vorhalten müssen, um die Auswirkungen des Upgrades verarbeiten zu können. Wenn Sie beispielsweise über fünf Upgradedomänen verfügen, verarbeiten die Knoten jeder Domäne ca. 20% des Datenverkehrs. Wenn Sie diese Upgradedomäne für ein Upgrade außer Betrieb nehmen müssen, muss diese Last verlagert werden können. Eine höhere Zahl von Upgradedomänen bedeutet weniger Mehraufwand, der auf den anderen Knoten im Cluster anfällt.

Der Nachteil bei der Verwendung von vielen Upgradedomänen ist, dass Upgrades länger dauern können. Service Fabric wartet kurz ab, nachdem eine Upgradedomäne abgeschlossen wurde, bevor der Vorgang fortgesetzt wird. Diese Verzögerung soll bewirken, dass Probleme, die mit dem Upgrade verbunden sind, angezeigt und erkannt werden können. Dieser Kompromiss ist akzeptabel, da dadurch verhindert wird, dass sich negative Änderungen jeweils auf einen zu großen Teil des Diensts auswirken.

Eine zu geringe Anzahl von Upgradedomänen hat ebenfalls Nebeneffekte: Während jede einzelne Upgradedomäne außer Betrieb genommen und aktualisiert wird, steht ein großer Teil der Gesamtkapazität nicht zur Verfügung. Beispiel: Wenn Sie nur drei Upgradedomänen besitzen, nehmen Sie jeweils etwa ein Drittel der Dienst- oder Clustergesamtkapazität außer Betrieb. Es ist nicht wünschenswert, dass so ein großer Teil Ihres Diensts auf einmal nicht betriebsbereit ist, weil im restlichen Cluster genügend Kapazität für die Verarbeitung der Workload vorhanden sein muss. Die Verwendung des Puffers bedeutet, dass diese Knoten im Normalfall eine geringere Auslastung als bei einem anderen Ansatz aufweisen. Dies führt zu höheren Kosten für den Betrieb Ihres Diensts.

Es gibt keine tatsächliche Beschränkung bei der Gesamtzahl der Fehler- oder Upgradedomänen in einer Umgebung bzw. keine Einschränkungen in Bezug auf ihre Überlappung. Häufig verwendete Strukturen sind:

* 1:1-Zuordnung von Fehlerdomänen und Upgradedomänen
* Eine Upgradedomäne pro Knoten (physische oder virtuelle Betriebssysteminstanz)
* Ein „Stripeset“- oder „Matrix“-Modell, bei dem die Fehler- und Upgradedomänen eine Matrix bilden, in der die Computer normalerweise entlang der Diagonalen angeordnet sind

<center>
![Layouts von Fehler- und Upgradedomänen][Image4]
</center>

Es gibt keine allgemeingültige Empfehlung für die Auswahl des Layouts, da jedes Layout seine Vor- und Nachteile hat. Das 1FD:1U-Modell ist beispielsweise relativ einfach einzurichten. Mit dem Modell mit einer Upgradedomäne pro Knoten sind die meisten Benutzer wahrscheinlich bereits vertraut, wenn sie in der Vergangenheit kleine Gruppen mit Computern verwaltet haben, die unabhängig voneinander heruntergefahren werden.

Das häufigste (und in Azure verwendete) Modell ist die FD/UD-Matrix, bei der die FDs und UDs eine Tabelle bilden und die Knoten am Anfang der Diagonale angeordnet sind. Ob wenig oder viele Knoten vorhanden sind, richtet sich nach der Gesamtzahl von Knoten im Vergleich zur Anzahl von FDs und UDs. Anders ausgedrückt: Für Cluster mit einer ausreichenden Größe sehen fast alle Anordnungen wie das dichte Matrixmuster aus, das in der obigen Abbildung unten rechts zu sehen ist.

## <a name="fault-and-upgrade-domain-constraints-and-resulting-behavior"></a>Einschränkungen und daraus resultierendes Verhalten von Fehler- und Upgradedomänen
Der Clusterressourcen-Manager behandelt die Anforderung, einen Dienst über Fehler- und Upgradedomänen hinweg auszugleichen, als Einschränkung. Weitere Informationen zu Einschränkungen finden Sie in [diesem Artikel](service-fabric-cluster-resource-manager-management-integration.md). In den Einschränkungen für Fehler- und Upgradedomänen heißt es: „Für jede Dienstpartition sollte der Unterschied zwischen der Anzahl von Dienstobjekten (zustandslose Dienstinstanzen oder zustandsbehaftete Dienstreplikate) zwischen zwei Domänen nie *mehr als eins* betragen.“  Praktisch bedeutet dies, dass bestimmte Verschiebungen oder Anordnungen für einen Dienst unter Umständen nicht gültig sind, da diese die Einschränkung der Fehler- oder Upgradedomäne verletzen würden.

Wir sehen uns nun ein Beispiel an. Angenommen, wir verwenden einen Cluster mit sechs Knoten, für die fünf Fehlerdomänen und fünf Upgradedomänen konfiguriert wurden.

|  | FD0 | FD1 | FD2 | FD3 | FD4 |
| --- |:---:|:---:|:---:|:---:|:---:|
| **UD0** |N1 | | | | |
| **UD1** |N6 |N2 | | | |
| **UD2** | | |N3 | | |
| **UD3** | | | |N4 | |
| **UD4** | | | | |N5 |

Nun erstellen wir einen Dienst mit einem TargetReplicaSetSize-Wert von&5;. Die Replikate (R) werden auf den Knoten N1-N5 gespeichert. N6 wird unabhängig von der Anzahl von erstellten Diensten niemals verwendet. Warum ist das so? Wir betrachten den Unterschied zwischen dem aktuellen Layout und dem Fall, in dem N6 gewählt wird.

Hier sehen Sie das Layout und die Gesamtzahl von Replikaten pro Fehler- und Upgradedomäne:

|  | FD0 | FD1 | FD2 | FD3 | FD4 | UDTotal |
| --- |:---:|:---:|:---:|:---:|:---:|:---:|
| **UD0** |R1 | | | | |1 |
| **UD1** | |R2 | | | |1 |
| **UD2** | | |R3 | | |1 |
| **UD3** | | | |R4 | |1 |
| **UD4** | | | | |R5 |1 |
| **FDTotal** |1 |1 |1 |1 |1 |- |

Dieses Layout ist in Bezug auf die Knoten pro Fehler- und Upgradedomäne ausgeglichen. Außerdem ist es in Bezug auf die Anzahl von Replikaten pro Fehler- und Upgradedomäne ausgeglichen. Jede Domäne verfügt über die gleiche Anzahl von Knoten und die gleiche Anzahl von Replikaten.

Nun sehen wir uns an, was passieren würde, wenn wir N6 anstelle von N2 verwenden würden. Wie wären die Replikate dann verteilt?

|  | FD0 | FD1 | FD2 | FD3 | FD4 | UDTotal |
| --- |:---:|:---:|:---:|:---:|:---:|:---:|
| **UD0** |R1 | | | | |1 |
| **UD1** |R5 | | | | |1 |
| **UD2** | | |R2 | | |1 |
| **UD3** | | | |R3 | |1 |
| **UD4** | | | | |R4 |1 |
| **FDTotal** |2 |0 |1 |1 |1 |- |

Fällt Ihnen etwas auf? Dieses Layout verstößt gegen die Definition der Fehlerdomäneneinschränkung. FD0 hat zwei Replikate, während FD1 keine Replikate hat, sodass der Unterschied zwischen FD0 und FD1 insgesamt&2; beträgt. Der Clusterressourcen-Manager lässt diese Anordnung nicht zu. Wenn wir N2 und N6 (anstelle von N1 und N2) auswählen, erhalten wir Folgendes:

|  | FD0 | FD1 | FD2 | FD3 | FD4 | UDTotal |
| --- |:---:|:---:|:---:|:---:|:---:|:---:|
| **UD0** | | | | | |0 |
| **UD1** |R5 |R1 | | | |2 |
| **UD2** | | |R2 | | |1 |
| **UD3** | | | |R3 | |1 |
| **UD4** | | | | |R4 |1 |
| **FDTotal** |1 |1 |1 |1 |1 |- |

Dieses Layout ist zwar in Bezug auf Fehlerdomänen ausgeglichen, aber es verstößt gegen die Upgradedomäneneinschränkung (da UD0 null Replikate hat, während UD1 zwei hat). Dieses Layout ist ebenfalls ungültig.

## <a name="configuring-fault-and-upgrade-domains"></a>Konfigurieren von Fehler- und Upgradedomänen
Das Definieren von Fehler- und Upgradedomänen erfolgt in unter Azure gehosteten Service Fabric-Bereitstellungen automatisch. Service Fabric wählt die Umgebungsinformationen von Azure und verwendet sie.

Wenn Sie Ihren eigenen Cluster erstellen (oder eine bestimmte Topologie ausführen möchten, die sich in der Entwicklung befindet), geben Sie die Informationen für die Fehler- und Upgradedomänen selbst an. In diesem Beispiel definieren wir einen lokalen Entwicklungscluster mit neun Knoten, der drei „Datencenter“ (mit jeweils drei Racks) umfasst. Dieser Cluster verfügt auch über drei Upgradedomänen, die auf diese drei Datencenter verteilt sind. Dies sieht in der Clustermanifestvorlage etwa wie folgt aus:

ClusterManifest.xml

```xml
  <Infrastructure>
    <!-- IsScaleMin indicates that this cluster runs on one-box /one single server -->
    <WindowsServer IsScaleMin="true">
      <NodeList>
        <Node NodeName="Node01" IPAddressOrFQDN="localhost" NodeTypeRef="NodeType01" FaultDomain="fd:/DC01/Rack01" UpgradeDomain="UpgradeDomain1" IsSeedNode="true" />
        <Node NodeName="Node02" IPAddressOrFQDN="localhost" NodeTypeRef="NodeType02" FaultDomain="fd:/DC01/Rack02" UpgradeDomain="UpgradeDomain2" IsSeedNode="true" />
        <Node NodeName="Node03" IPAddressOrFQDN="localhost" NodeTypeRef="NodeType03" FaultDomain="fd:/DC01/Rack03" UpgradeDomain="UpgradeDomain3" IsSeedNode="true" />
        <Node NodeName="Node04" IPAddressOrFQDN="localhost" NodeTypeRef="NodeType04" FaultDomain="fd:/DC02/Rack01" UpgradeDomain="UpgradeDomain1" IsSeedNode="true" />
        <Node NodeName="Node05" IPAddressOrFQDN="localhost" NodeTypeRef="NodeType05" FaultDomain="fd:/DC02/Rack02" UpgradeDomain="UpgradeDomain2" IsSeedNode="true" />
        <Node NodeName="Node06" IPAddressOrFQDN="localhost" NodeTypeRef="NodeType06" FaultDomain="fd:/DC02/Rack03" UpgradeDomain="UpgradeDomain3" IsSeedNode="true" />
        <Node NodeName="Node07" IPAddressOrFQDN="localhost" NodeTypeRef="NodeType07" FaultDomain="fd:/DC03/Rack01" UpgradeDomain="UpgradeDomain1" IsSeedNode="true" />
        <Node NodeName="Node08" IPAddressOrFQDN="localhost" NodeTypeRef="NodeType08" FaultDomain="fd:/DC03/Rack02" UpgradeDomain="UpgradeDomain2" IsSeedNode="true" />
        <Node NodeName="Node09" IPAddressOrFQDN="localhost" NodeTypeRef="NodeType09" FaultDomain="fd:/DC03/Rack03" UpgradeDomain="UpgradeDomain3" IsSeedNode="true" />
      </NodeList>
    </WindowsServer>
  </Infrastructure>
```

über „ClusterConfig.json“ für eigenständige Bereitstellungen

```json
"nodes": [
  {
    "nodeName": "vm1",
    "iPAddress": "localhost",
    "nodeTypeRef": "NodeType0",
    "faultDomain": "fd:/dc1/r0",
    "upgradeDomain": "UD1"
  },
  {
    "nodeName": "vm2",
    "iPAddress": "localhost",
    "nodeTypeRef": "NodeType0",
    "faultDomain": "fd:/dc1/r0",
    "upgradeDomain": "UD2"
  },
  {
    "nodeName": "vm3",
    "iPAddress": "localhost",
    "nodeTypeRef": "NodeType0",
    "faultDomain": "fd:/dc1/r0",
    "upgradeDomain": "UD3"
  },
  {
    "nodeName": "vm4",
    "iPAddress": "localhost",
    "nodeTypeRef": "NodeType0",
    "faultDomain": "fd:/dc2/r0",
    "upgradeDomain": "UD1"
  },
  {
    "nodeName": "vm5",
    "iPAddress": "localhost",
    "nodeTypeRef": "NodeType0",
    "faultDomain": "fd:/dc2/r0",
    "upgradeDomain": "UD2"
  },
  {
    "nodeName": "vm6",
    "iPAddress": "localhost",
    "nodeTypeRef": "NodeType0",
    "faultDomain": "fd:/dc2/r0",
    "upgradeDomain": "UD3"
  },
  {
    "nodeName": "vm7",
    "iPAddress": "localhost",
    "nodeTypeRef": "NodeType0",
    "faultDomain": "fd:/dc3/r0",
    "upgradeDomain": "UD1"
  },
  {
    "nodeName": "vm8",
    "iPAddress": "localhost",
    "nodeTypeRef": "NodeType0",
    "faultDomain": "fd:/dc3/r0",
    "upgradeDomain": "UD2"
  },
  {
    "nodeName": "vm9",
    "iPAddress": "localhost",
    "nodeTypeRef": "NodeType0",
    "faultDomain": "fd:/dc3/r0",
    "upgradeDomain": "UD3"
  }
],
```

> [!NOTE]
> In Azure-Bereitstellungen werden Fehlerdomänen und Upgradedomänen von Azure zugewiesen. Daher enthält die Definition Ihrer Knoten und Rollen unter der Infrastrukturoption für Azure keine Informationen zu Fehler- oder Upgradedomänen.
>
>

## <a name="placement-constraints-and-node-properties"></a>Platzierungseinschränkungen und Knoteneigenschaften
Manchmal (eigentlich in den meisten Fällen) möchten Sie sicherstellen, dass bestimmte Workloads nur auf bestimmten Knoten oder bestimmten Knotengruppen im Cluster ausgeführt werden. Für einige Workloads sind beispielsweise GPUs oder SSDs erforderlich, für andere hingegen nicht. Ein gutes Beispiel für die Ausrichtung von Hardware auf bestimmte Workloads ist nahezu jede vorstellbare N-Tier-Architektur. Bei diesen Architekturen dienen bestimmte Computer als Front-End/Schnittstellenseite der Anwendung (und sind somit meist über das Internet verfügbar). Unterschiedliche Gruppen von Computern (häufig mit unterschiedlichen Hardwareressourcen) verarbeiten die Workload der Compute- oder Speicherebenen (und sind normalerweise nicht über das Internet zugänglich). Service Fabric erwartet, dass es selbst in der Welt der Microservices Fälle gibt, in denen bestimmte Workloads in bestimmten Hardwarekonfigurationen, ausgeführt werden müssen, z.B.:

* Eine vorhandene n-Ebenen-Anwendung wurde in eine Service Fabric-Umgebung verschoben.
* Eine Workload soll aus Leistungs-, Skalierbarkeits- oder Sicherheitsisolationsgründen auf bestimmter Hardware ausgeführt werden.
* Eine Workload sollte aufgrund von Richtlinien oder des Ressourcenverbrauchs von anderen Workloads isoliert werden.

Zur Unterstützung dieser Arten von Konfigurationen verfügt Service Fabric über ein erstklassiges Tagkonzept, bei dem die Tags auf Knoten angewendet werden können. Diese werden als Platzierungseinschränkungen bezeichnet. Mit Platzierungseinschränkungen kann angegeben werden, wo bestimmte Dienste ausgeführt werden sollen. Die Gruppe der Einschränkungen ist erweiterbar. Alle Schlüssel-Wert-Paare können funktionieren.

<center>
![Clusterlayout bei verschiedenen Workloads][Image5]
</center>

Die unterschiedlichen Schlüssel/Wert-Tags auf Knoten werden als *Eigenschaften* der Knotenplatzierung (oder kurz Knoteneigenschaften) bezeichnet. Der Wert in der Knoteneigenschaft kann als Zeichenfolge, boolescher Wert oder als Wert vom Typ „signed long“ angegeben werden. Die Anweisung im Dienst wird als Platzierungs*einschränkung* bezeichnet, weil damit eingeschränkt wird, wo der Dienst im Cluster ausgeführt werden kann. Bei der Einschränkung kann es sich um eine boolesche Anweisung handeln, die für die verschiedenen Knoteneigenschaften im Cluster ausgeführt wird. Die gültigen Selektoren in diesen booleschen Anweisungen lauten:

1) Bedingungsüberprüfungen zum Erstellen bestimmter Anweisungen

| Anweisung | Syntax |
| --- |:---:|
| "equal to" (ist gleich) | "==" |
| "not equal to" (ungleich) | "!=" |
| "greater than" (größer als) | ">" |
| "greater than or equal to" (größer als oder gleich) | ">=" |
| "less than" (kleiner als) | "<" |
| "less than or equal to" (kleiner als oder gleich) | "<=" |

2) Boolesche Anweisungen für Gruppierungen und logische Vorgänge

| Anweisung | Syntax |
| --- |:---:|
| "and" (und) | "&&" |
| "or" (oder) | "&#124;&#124;" |
| "not" (nicht) | "!" |
| "group as single statement" (Als einzelne Anweisung gruppieren) | "()" |

Hier sind einige Beispiele für einfache Anweisungen für Einschränkungen angegeben.

  * `"Value >= 5"`
  * `"NodeColor != green"`
  * `"((OneProperty < 100) || ((AnotherProperty == false) && (OneProperty >= 100)))"`

Nur auf Knoten, bei denen die Anweisung insgesamt als „True“ ausgewertet wird, kann ein Dienst platziert werden. Knoten, für die keine Eigenschaft definiert ist, entsprechen keiner Platzierungseinschränkung mit dieser Eigenschaft.

Service Fabric definiert einige Standardknoteneigenschaften, die automatisch verwendet werden können, ohne dass sie vom Benutzer definiert werden müssen. Zum Zeitpunkt der Erstellung dieses Dokuments sind für jeden Knoten die Standardeigenschaften **NodeType** und **NodeName** definiert. Sie können beispielsweise folgende Platzierungseinschränkung schreiben: `"(NodeType == NodeType03)"`. Es hat sich gezeigt, dass NodeType eine der am häufigsten verwendeten Eigenschaften ist. Sie ist nützlich, da es sich um eine 1:1-Entsprechung eines Computertyps handelt, der wiederum einem Workloadtyp einer herkömmlichen N-Tier-Anwendungsarchitektur entspricht.

<center>
![Platzierungseinschränkungen und Knoteneigenschaften][Image6]
</center>

Angenommen, die folgenden Knoteneigenschaften wurden für einen bestimmten Knotentyp definiert:

ClusterManifest.xml

```xml
    <NodeType Name="NodeType01">
      <PlacementProperties>
        <Property Name="HasSSD" Value="true"/>
        <Property Name="NodeColor" Value="green"/>
        <Property Name="SomeProperty" Value="5"/>
      </PlacementProperties>
    </NodeType>
```

Über „ClusterConfig.json“ für eigenständige Bereitstellungen bzw. über „Template.json“ für in Azure gehostete Cluster. In Ihrer Azure Resource Manager-Vorlage sind für einen Cluster Dinge wie beispielsweise der Knotentypname meist parametrisiert und lauten eher „[parameters('vmNodeType1Name')]“ als „NodeType01“.

```json
"nodeTypes": [
    {
        "name": "NodeType01",
        "placementProperties": {
            "HasSSD": "true",
            "NodeColor": "green",
            "SomeProperty": "5"
        },
    }
],
```

Sie können Dienstplatzierungs*einschränkungen* für einen Dienst wie folgt erstellen:

C#

```csharp
FabricClient fabricClient = new FabricClient();
StatefulServiceDescription serviceDescription = new StatefulServiceDescription();
serviceDescription.PlacementConstraints = "(HasSSD == true && SomeProperty >= 4)";
// add other required servicedescription fields
//...
await fabricClient.ServiceManager.CreateServiceAsync(serviceDescription);
```

Powershell:

```posh
New-ServiceFabricService -ApplicationName $applicationName -ServiceName $serviceName -ServiceTypeName $serviceType -Stateful -MinReplicaSetSize 2 -TargetReplicaSetSize 3 -PartitionSchemeSingleton -PlacementConstraint "HasSSD == true && SomeProperty >= 4"
```

Wenn Sie sicher sind, dass alle Knoten vom Typ „NodeType01“ gültig sind, können Sie auch diesen Knotentyp wählen.

Das Praktische an den Platzierungseinschränkungen eines Diensts ist, dass sie während der Laufzeit dynamisch aktualisiert werden können. Falls erforderlich, können Sie also einen Dienst im Cluster verschieben, Anforderungen hinzufügen und entfernen usw. Service Fabric stellt sicher, dass der Dienst einsatzbereit und verfügbar bleibt, selbst wenn diese Art von Änderungen fortlaufend ausgeführt wird.

C#:

```csharp
StatefulServiceUpdateDescription updateDescription = new StatefulServiceUpdateDescription();
updateDescription.PlacementConstraints = "NodeType == NodeType01";
await fabricClient.ServiceManager.UpdateServiceAsync(new Uri("fabric:/app/service"), updateDescription);
```

Powershell:

```posh
Update-ServiceFabricService -Stateful -ServiceName $serviceName -PlacementConstraints "NodeType == NodeType01"
```

Platzierungseinschränkungen (und zahlreiche weitere Orchestratormechanismen, über die wir sprechen werden) werden für jede unterschiedliche benannte Dienstinstanz angegeben. Updates ersetzen (überschreiben) immer die vorherige Angabe.

Die Eigenschaften auf einem Knoten werden über die Clusterdefinition definiert und können daher nicht ohne ein Upgrade des Clusters aktualisiert werden. Für das Upgrade der Eigenschaften eines Knotens muss jeder betroffene Knoten herunter- und dann wieder hochgefahren werden.

## <a name="capacity"></a>Kapazität
Eine der wichtigsten Aufgaben eines Orchestrators besteht in der Verwaltung des Ressourcenverbrauchs im Cluster. Was Sie keineswegs möchten, ist folgender Fall: Sie versuchen, Dienste auf effiziente Weise auszuführen, und eine größere Zahl von Knoten ist sehr stark ausgelastet, während andere nur eine geringe Auslastung aufweisen. Knoten mit hoher Auslastung führen zu Ressourcenkonflikten und schlechter Leistung, und Knoten mit sehr geringer Auslastung bedeuten verschwendete Ressourcen und höhere Kosten. Bevor wir über die gleichmäßige Verteilung sprechen, sollten wir zuerst sicherstellen, dass den Knoten nicht die Ressourcen ausgehen.

Service Fabric stellt Ressourcen als `Metrics` (Metriken) dar. Bei Metriken handelt es sich um alle logischen oder physischen Ressourcen, die Sie für Service Fabric beschreiben möchten. Beispiele für Metriken: WorkQueueDepth oder MemoryInMb. Informationen zur Konfiguration von Metriken und ihrer Verwendung finden Sie in [diesem Artikel](service-fabric-cluster-resource-manager-metrics.md).

Metriken unterscheiden sich von Platzierungseinschränkungen und Knoteneigenschaften. Knoteneigenschaften sind statische Deskriptoren der eigentlichen Knoten, während es bei Metriken um Ressourcen geht, über die Knoten verfügen und die von Diensten genutzt werden, wenn sie auf einem Knoten ausgeführt werden. Eine Knoteneigenschaft könnte beispielsweise „HasSSD“ lauten und auf „true“ oder „false“ festgelegt sein. Die Menge des verfügbaren Speicherplatzes auf dieser SSD (der von den Diensten genutzt wird) kann eine Metrik wie „DriveSpaceInMb“ sein. Der Knoten verfügt dann über die Kapazität für „DriveSpaceInMb“ bis zu der auf dem Laufwerk nicht reservierten Speicherplatzmenge. Die Dienste melden, welchen Anteil der Metrik sie zur Laufzeit verwendet haben.

Es ist wichtig zu beachten, dass der Clusterressourcen-Manager von Service Fabric – genau wie bei Platzierungseinschränkungen und Knoteneigenschaften – nicht versteht, was die Namen der Metriken bedeuten. Bei Namen von Metriken handelt es sich lediglich um Zeichenfolgen. Es ist ratsam, Einheiten als Teil der von Ihnen erstellten Namen von Metriken zu deklarieren, falls die Gefahr von Mehrdeutigkeiten besteht.

Wenn Sie den *Lastenausgleich*für alle Ressourcen deaktivieren, kann der Clusterressourcen-Manager von Service Fabric dennoch versuchen sicherzustellen, dass kein Knoten überlastet wird. Im Allgemeinen ist dies möglich, sofern der gesamte Cluster nicht zu voll ist. Die Kapazität ist eine weitere *Einschränkung*, anhand der der Clusterressourcen-Manager ermittelt, wie viele Ressourcen auf einem Knoten verbraucht werden. Auch die verbleibende Kapazität wird für den gesamten Cluster nachverfolgt. Sowohl die Kapazität als auch der Verbrauch auf Dienstebene werden als Metrik ausgedrückt. Die Metrik kann beispielsweise „MemoryInMb“ lauten, und ein Knoten kann für „MemoryInMb“ eine Kapazität von „2048“ aufweisen. Eine Dienst, der auf diesem Knoten ausgeführt wird, kann beispielsweise melden, dass sein Verbrauch für „MemoryInMb“ gerade „64“ beträgt.

Während der Laufzeit verfolgt der Clusterressourcen-Manager nach, welcher Anteil jeder Ressource auf den einzelnen Knoten vorhanden ist und wie groß der Rest ist. Hierzu wird der deklarierte Verbrauchswert der einzelnen Dienste, die auf dem Knoten ausgeführt werden, von der Kapazität des Knotens abgezogen. Mit diesen Informationen kann der Clusterressourcen-Manager von Service Fabric ermitteln, wo Replikate platziert oder wohin diese verschoben werden sollen, um die Kapazität der Knoten nicht zu überschreiten.

C#:

```csharp
StatefulServiceDescription serviceDescription = new StatefulServiceDescription();
ServiceLoadMetricDescription metric = new ServiceLoadMetricDescription();
metric.Name = "MemoryInMb";
metric.PrimaryDefaultLoad = 64;
metric.SecondaryDefaultLoad = 64;
metric.Weight = ServiceLoadMetricWeight.High;
serviceDescription.Metrics.Add(metric);
await fabricClient.ServiceManager.CreateServiceAsync(serviceDescription);
```

Powershell:

```posh
New-ServiceFabricService -ApplicationName $applicationName -ServiceName $serviceName -ServiceTypeName $serviceTypeName –Stateful -MinReplicaSetSize 2 -TargetReplicaSetSize 3 -PartitionSchemeSingleton –Metric @("Memory,High,64,64)
```
<center>
![Clusterknoten und -kapazität][Image7]
</center>

Sie sehen die definierten Kapazitäten im Clustermanifest:

ClusterManifest.xml

```xml
    <NodeType Name="NodeType02">
      <Capacities>
        <Capacity Name="MemoryInMb" Value="2048"/>
        <Capacity Name="DiskInMb" Value="512000"/>
      </Capacities>
    </NodeType>
```

Über „ClusterConfig.json“ für eigenständige Bereitstellungen bzw. über „Template.json“ für in Azure gehostete Cluster. In Ihrer Azure Resource Manager-Vorlage sind für einen Cluster Dinge wie beispielsweise der Knotentypname meist parametrisiert und lauten eher „[parameters('vmNodeType2Name')]“ als „NodeType02“.

```json
"nodeTypes": [
    {
        "name": "NodeType02",
        "capacities": {
            "MemoryInMb": "2048",
            "DiskInMb": "512000"
        }
    }
],
```

Unter Umständen ändert sich die Last eines Diensts auch dynamisch. (Dies kann recht häufig der Fall sein.) Angenommen, die Last eines Replikats ändert sich von 64 in 1.024, aber der Rest auf dem Knoten, auf dem das Replikat ausgeführt wird, beträgt nur noch 512 (Metrik „MemoryInMb“). Die Platzierung dieses Replikats bzw. der Instanz ist ungültig, da auf dem Knoten nicht genügend Platz ist. Dies kann auch vorkommen, wenn die kombinierte Nutzung der Replikate und Instanzen auf diesem Knoten die Kapazität dieses Knotens übersteigt. In beiden Fällen muss der Clusterressourcen-Manager eingreifen und für den Knoten wieder die Einhaltung der Kapazitätsgrenze sicherstellen. Hierzu werden ein oder mehrere Replikate oder Instanzen dieses Knotens auf andere Knoten verschoben. Beim Verschieben von Replikaten versucht der Clusterressouren-Manager, die Kosten dieser Verschiebungen möglichst gering zu halten. Die Kosten von Verschiebungen werden in [diesem Artikel](service-fabric-cluster-resource-manager-movement-cost.md) beschrieben.

## <a name="cluster-capacity"></a>Clusterkapazität
Wie verhindern wir eine Überlastung des Clusters? Bei dynamischer Auslastung gibt es für den Clusterressourcen-Manager nicht viele Möglichkeiten. Dienste können unabhängig von Aktionen, die vom Clusterressourcen-Manager durchgeführt werden, Lastspitzen aufweisen. Dies kann dazu führen, dass Ihr Cluster, der heute noch über ausreichend Spielraum verfügt, nicht mehr ausreicht, wenn Sie morgen großen Erfolg haben. Es sind aber einige Kontrollmechanismen eingebaut, um die üblichen Probleme zu verhindern. Zunächst einmal können wir die Erstellung neuer Workloads verhindern, die eine Überlastung des Clusters verursachen.

Angenommen, Sie erstellen einen zustandslosen Dienst, dem eine bestimmte Last zugeordnet ist (weitere Informationen zur Meldung standardmäßiger und dynamischer Auslastung finden Sie weiter unten). Angenommen, für den Dienst ist die Metrik „DiskSpaceInMb“ wichtig. Wir nehmen weiterhin an, dass für jede Instanz des Diensts fünf Einheiten von „DiskSpaceInMb“ verbraucht werden. Sie möchten drei Instanzen des Diensts erstellen. Prima. Dies bedeutet, dass im Cluster 15 Einheiten von „DiskSpaceInMb“ vorhanden sein müssen, damit wir diese Dienstinstanzen überhaupt erstellen können. Der Clusterressourcen-Manager berechnet ständig die Gesamtkapazität und den Gesamtverbrauch jeder Metrik, damit leicht ermittelt werden kann, ob im Cluster ausreichend Speicherplatz vorhanden ist. Falls nicht genügend Speicherplatz vorhanden ist, lehnt der Clusterressourcen-Manager den Aufruf zum Erstellen des Diensts ab.

Da die Anforderung lediglich darin besteht, dass 15 Einheiten verfügbar sind, kann dieser Speicherplatz auf vielerlei Weise zugeordnet werden. Beispielweise kann eine verbleibende Einheit mit 15 unterschiedlichen Knoten verwendet werden, oder drei verbleibende Einheiten mit einer Kapazität von fünf unterschiedlichen Knoten. Solange der Clusterressourcen-Manager die Anordnung so ändern kann, dass auf drei Knoten fünf Einheiten verfügbar sind, kann der Dienst platziert werden. Eine Neuanordnung dieser Art ist fast immer möglich, sofern der Cluster als Ganzes nicht fast vollständig voll ist oder alle Dienste „umfangreich“ sind (oder beides zutrifft).

## <a name="buffered-capacity"></a>Gepufferte Kapazität
Ein weiteres Feature des Clusterressourcen-Managers, mit dem Benutzer bei der Verwaltung der gesamten Clusterkapazität unterstützt werden, ist die Angabe eines reservierten Puffers für die angegebene Kapazität jedes Knotens. Die gepufferte Kapazität ermöglicht die Reservierung eines Teils der gesamten Knotenkapazität, damit sie nur verwendet wird, um bei Upgrades und Knotenausfällen Dienste zu platzieren. Heute wird der Puffer global pro Metrik für alle Knoten über die Clusterdefinition angegeben. Der Wert, den Sie für die reservierte Kapazität auswählen, ist eine Funktion aus der Anzahl von Fehler- und Upgradedomänen im Cluster und des gewünschten Overheads. Eine höhere Zahl von Fehler- und Upgradedomänen bedeutet, dass Sie für Ihre gepufferte Kapazität einen niedrigeren Wert wählen können. Bei einer größeren Zahl von Domänen können Sie erwarten, dass geringere Mengen Ihres Clusters bei Upgrades und Ausfällen nicht verfügbar sind. Das Angeben des Pufferprozentsatzes ist nur sinnvoll, wenn Sie auch die Knotenkapazität für eine Metrik angegeben haben.

Hier finden Sie ein Beispiel für die Angabe der gepufferten Kapazität:

ClusterManifest.xml

```xml
        <Section Name="NodeBufferPercentage">
            <Parameter Name="DiskSpace" Value="0.10" />
            <Parameter Name="Memory" Value="0.15" />
            <Parameter Name="SomeOtherMetric" Value="0.20" />
        </Section>
```

Über „ClusterConfig.json“ für eigenständige Bereitstellungen bzw. über „Template.json“ für in Azure gehostete Cluster:

```json
"fabricSettings": [
  {
    "name": "NodeBufferPercentage",
    "parameters": [
      {
          "name": "DiskSpace",
          "value": "0.10"
      },
      {
          "name": "Memory",
          "value": "0.15"
      },
      {
          "name": "SomeOtherMetric",
          "value": "0.20"
      }
    ]
  }
]
```

Die Erstellung neuer Dienste ist nicht erfolgreich, wenn der Cluster für eine Metrik keine gepufferte Kapazität mehr aufweist. So wird sichergestellt, dass der Cluster genügend Overhead als Ersatz aufweist, damit es bei Upgrades und Ausfällen nicht passiert, dass die Kapazität überschritten wird. Die gepufferte Kapazität ist optional, aber sie wird für alle Cluster empfohlen, bei denen für eine Metrik eine Kapazität definiert wird.

Der Clusterressourcen-Manager macht diese Informationen über PowerShell und die Abfrage-APIs verfügbar. So können Sie die Einstellungen für die gepufferte Kapazität, die gesamte Kapazität und den aktuellen Verbrauch für jede Metrik anzeigen, die im Cluster verwendet wird. Hier sehen Sie ein Beispiel für diese Ausgabe:

```posh
PS C:\Users\user> Get-ServiceFabricClusterLoadInformation
LastBalancingStartTimeUtc : 9/1/2016 12:54:59 AM
LastBalancingEndTimeUtc   : 9/1/2016 12:54:59 AM
LoadMetricInformation     :
                            LoadMetricName        : Metric1
                            IsBalancedBefore      : False
                            IsBalancedAfter       : False
                            DeviationBefore       : 0.192450089729875
                            DeviationAfter        : 0.192450089729875
                            BalancingThreshold    : 1
                            Action                : NoActionNeeded
                            ActivityThreshold     : 0
                            ClusterCapacity       : 189
                            ClusterLoad           : 45
                            ClusterRemainingCapacity : 144
                            NodeBufferPercentage  : 10
                            ClusterBufferedCapacity : 170
                            ClusterRemainingBufferedCapacity : 125
                            ClusterCapacityViolation : False
                            MinNodeLoadValue      : 0
                            MinNodeLoadNodeId     : 3ea71e8e01f4b0999b121abcbf27d74d
                            MaxNodeLoadValue      : 15
                            MaxNodeLoadNodeId     : 2cc648b6770be1bc9824fa995d5b68b1
```

## <a name="next-steps"></a>Nächste Schritte
* Informationen über die Architektur und den Informationsfluss innerhalb des Clusterressourcen-Managers finden Sie in [diesem Artikel](service-fabric-cluster-resource-manager-architecture.md).
* Das Definieren von Defragmentierungsmetriken ist eine Möglichkeit, die Last auf Knoten zu konsolidieren, statt sie auszubreiten. Informationen zum Konfigurieren der Defragmentierung finden Sie in [diesem Artikel](service-fabric-cluster-resource-manager-defragmentation-metrics.md)
* Starten Sie mit einer [Einführung in den Clusterressourcen-Manager von Service Fabric](service-fabric-cluster-resource-manager-introduction.md)
* Informationen darüber, wie der Clusterressourcen-Manager die Auslastung im Cluster verwaltet und verteilt, finden Sie im Artikel zum [Lastenausgleich](service-fabric-cluster-resource-manager-balancing.md)

[Image1]:./media/service-fabric-cluster-resource-manager-cluster-description/cluster-fault-domains.png
[Image2]:./media/service-fabric-cluster-resource-manager-cluster-description/cluster-uneven-fault-domain-layout.png
[Image3]:./media/service-fabric-cluster-resource-manager-cluster-description/cluster-fault-and-upgrade-domains-with-placement.png
[Image4]:./media/service-fabric-cluster-resource-manager-cluster-description/cluster-fault-and-upgrade-domain-layout-strategies.png
[Image5]:./media/service-fabric-cluster-resource-manager-cluster-description/cluster-layout-different-workloads.png
[Image6]:./media/service-fabric-cluster-resource-manager-cluster-description/cluster-placement-constraints-node-properties.png
[Image7]:./media/service-fabric-cluster-resource-manager-cluster-description/cluster-nodes-and-capacity.png

