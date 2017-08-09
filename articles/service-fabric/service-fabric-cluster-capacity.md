---
title: "Planen der Service Fabric-Clusterkapazität | Microsoft Docs"
description: "Überlegungen zur Kapazitätsplanung für Service Fabric-Cluster. Knotentypen, Beständigkeit und Zuverlässigkeitsstufen"
services: service-fabric
documentationcenter: .net
author: ChackDan
manager: timlt
editor: 
ms.assetid: 4c584f4a-cb1f-400c-b61f-1f797f11c982
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/24/2017
ms.author: chackdan
ms.translationtype: HT
ms.sourcegitcommit: 141270c353d3fe7341dfad890162ed74495d48ac
ms.openlocfilehash: 9d5a4bef0c22f637a35390c6a8a245967fb02118
ms.contentlocale: de-de
ms.lasthandoff: 07/25/2017

---
# <a name="service-fabric-cluster-capacity-planning-considerations"></a>Überlegungen zur Kapazitätsplanung für Service Fabric-Cluster
Die Kapazitätsplanung ist ein wichtiger Schritt bei jeder Produktionsbereitstellung. Nachfolgend sind einige Aspekte aufgeführt, die Sie dabei berücksichtigen müssen.

* Die Anzahl von Knotentypen, über die Ihr Cluster anfänglich verfügen muss
* Die Eigenschaften der einzelnen Knotentypen (Größe, primärer Knotentyp, Internetzugriff, Anzahl von VMs usw.)
* Die Zuverlässigkeits- und Dauerhaftigkeitsmerkmale des Clusters

Im Folgenden gehen wir kurz auf diese Aspekte ein.

## <a name="the-number-of-node-types-your-cluster-needs-to-start-out-with"></a>Die Anzahl von Knotentypen, über die Ihr Cluster anfänglich verfügen muss
Zunächst müssen Sie herausfinden, zu welchem Zweck der Cluster verwendet werden soll und welche Arten von Anwendungen in diesem Cluster bereitgestellt werden sollen. Wenn Sie bezüglich des Verwendungszwecks des Clusters unsicher sind, ist es vermutlich noch zu früh für die Kapazitätsplanung.

Legen Sie die Anzahl von Knotentypen fest, über die Ihr Cluster anfänglich verfügen muss.  Jeder Knotentyp wird einer VM-Skalierungsgruppe zugeordnet. Jeden Knotentyp kann dann unabhängig zentral hoch- oder herunterskaliert werden, bei jedem Typ können unterschiedliche Portgruppen geöffnet sein, und die Typen können verschiedene Kapazitätsmetriken aufweisen. Wenn Sie die Anzahl von Knotentypen festlegen, sind also die folgenden Aspekte entscheidend:

* Weist Ihre Anwendung mehrere Dienste auf, und müssen einige dieser Dienste öffentlich sein oder über Internetzugriff verfügen? Typische Anwendungen umfassen einen Front-End-Gatewaydienst, der Eingaben von einem Client empfängt, und einen oder mehrere Back-End-Dienste, die mit den Front-End-Diensten kommunizieren. In diesem Fall verfügen Sie also über mindestens zwei Knotentypen.
* Haben Ihre Dienste (aus denen sich Ihre Anwendung zusammensetzt) unterschiedliche Infrastrukturanforderungen, z. B. höhere RAM-Anforderungen oder längere CPU-Zyklen? Nehmen wir z. B. an, dass die Anwendung, die bereitgestellt werden soll, einen Front-End-Dienst und einen Back-End-Dienst umfasst. Der Front-End-Dienst kann auf kleineren virtuellen Computern (VM-Größen wie D2) platziert werden, die über geöffnete Ports für das Internet verfügen.  Der rechenintensive Back-End-Dienst hingegen muss auf größeren virtuellen Computern (mit VM-Größen wie D4, D6, D15) platziert werden, die nicht vom Internet aus zugänglich sind.
  
  Auch wenn beide Dienste in diesem Beispiel auf einem Knotentyp verwendet werden können, wird empfohlen, sie in einem Cluster mit zwei Knotentypen zu platzieren.  Dadurch können für die Knotentypen unterschiedliche Eigenschaften (z. B. Internetkonnektivität oder VM-Größe) festgelegt werden. Außerdem kann die Anzahl von VMs individuell skaliert werden.  
* Da Sie nicht in die Zukunft blicken können, sollten Sie sich auf die Ihnen bekannten Fakten verlassen und die Anzahl von Knotentypen entsprechend festlegen, über die Ihre Anwendungen anfänglich verfügen müssen. Später können Knotentypen hinzugefügt oder entfernt werden. Ein Service Fabric-Cluster muss über mindestens einen Knotentyp verfügen.

## <a name="the-properties-of-each-node-type"></a>Die Eigenschaften der einzelnen Knotentypen
Der **Knotentyp** kann als Äquivalent zu Rollen in Cloud Services betrachtet werden. Knotentypen definieren die Größe, die Anzahl und die Eigenschaften der virtuellen Computer. Jeder Knotentyp, der in einem Service Fabric-Cluster definiert ist, wird als separate Skalierungsgruppe eines virtuellen Computers eingerichtet. Eine Skalierungsgruppe eines virtuellen computers ist eine Azure-Computeressource, mit der Sie eine Sammlung von virtuellen Computern bereitstellen und verwalten können. Da die einzelnen Knotentypen als separate Skalierungsgruppen von virtuellen Computern definiert werden, können sie unabhängig voneinander zentral hoch- oder herunterskaliert werden. Außerdem können bei den verschiedenen Typen unterschiedliche Portgruppen geöffnet sein, und die Typen können unterschiedliche Kapazitätsmetriken aufweisen.

Lesen Sie [dieses Dokument](service-fabric-cluster-nodetypes.md), um detaillierte Informationen zur Beziehung zwischen Knotentypen und Skalierungsgruppen von virtuellen Computern zu erhalten und zu erfahren, wie Sie per Remotedesktopprotokoll eine Verbindung mit einer der Instanzen herstellen, neue Ports öffnen und vieles mehr.

Der Cluster kann über mehrere Knotentypen verfügen. Der primäre Knotentyp (der erste, den Sie im Portal definieren) muss bei Clustern, die für Produktionsworkloads eingesetzt werden, jedoch mindestens fünf VMs aufweisen (für Testcluster sind mindestens drei VMs erforderlich). Wenn Sie den Cluster anhand einer Resource Manager-Vorlage erstellen, suchen Sie unter der Knotentypdefinition nach dem Attribut **is Primary**. Der primäre Knotentyp ist der Knotentyp mit den Service Fabric-Systemdiensten.  

### <a name="primary-node-type"></a>Primärer Knotentyp
Bei Clustern mit mehreren Knotentypen muss ein Knotentyp als primärer Knotentyp festgelegt werden. Nachfolgend sind die Merkmale eines primären Knotentyps aufgeführt:

* Die **Mindestgröße von VMs** für den primären Knotentyp hängt von der gewählten **Dauerhaftigkeitsstufe** ab. Der Standardwert für die Dauerhaftigkeitsstufe ist „Bronze“. Scrollen Sie nach unten, um Einzelheiten zur Dauerhaftigkeitsstufe und den möglichen Werten anzuzeigen.  
* Die **Mindestanzahl von VMs** für den primären Knotentyp hängt von der gewählten **Zuverlässigkeitsstufe** ab. Der Standardwert für die Zuverlässigkeitsstufe ist „Silber“. Scrollen Sie nach unten, um Einzelheiten zur Zuverlässigkeitsstufe und den möglichen Werten anzuzeigen. 

 

* Die Service Fabric-Systemdienste (z. B. der Cluster-Manager-Dienst oder der Imagespeicherdienst) werden auf dem primären Knotentyp platziert. Die Zuverlässigkeit und die Dauerhaftigkeit des Clusters hängt also von der Zuverlässigkeitsstufe und der Dauerhaftigkeitsstufe ab, die Sie für den primären Knotentyp auswählen.

![Screenshot eines Clusters mit zwei Knotentypen ][SystemServices]

### <a name="non-primary-node-type"></a>Nicht primärer Knotentyp
Cluster mit mehreren Knotentypen verfügen über einen primären Knotentyp. Die übrigen Knotentypen sind keine primären Knotentypen. Nachfolgend sind die Merkmale eines nicht primären Knotentyps aufgeführt:

* Die Mindestgröße von VMs für diesen Knotentyp hängt von der gewählten Dauerhaftigkeitsstufe ab. Der Standardwert für die Dauerhaftigkeitsstufe ist „Bronze“. Scrollen Sie nach unten, um Einzelheiten zur Dauerhaftigkeitsstufe und den möglichen Werten anzuzeigen.  
* Die Mindestanzahl von VMs für diesen Knotentyp beträgt 1. Diese Anzahl sollte jedoch basierend auf der Anzahl von Replikaten der Anwendung/Dienste gewählt werden, die auf diesem Knotentyp ausgeführt werden soll bzw. sollen. Die Anzahl von VMs auf einem Knotentyp kann nach der Bereitstellung des Clusters erhöht werden.

## <a name="the-durability-characteristics-of-the-cluster"></a>Die Dauerhaftigkeitsmerkmale des Clusters
Über die Dauerhaftigkeitsstufe wird dem System angezeigt, über welche Berechtigungen Ihre VMs für die zugrunde liegende Azure-Infrastruktur verfügen. Auf dem primären Knotentyp kann Service Fabric mit dieser Berechtigung Infrastrukturanforderungen auf VM-Ebene anhalten (z. B. einen VM-Neustart, ein VM-Reimaging oder eine VM-Migration), die sich auf die Quorumanforderungen für die Systemdienste und Ihre zustandsbehafteten Dienste auswirken. Auf den nicht primären Knotentypen kann Service Fabric mit dieser Berechtigung Infrastrukturanforderungen auf VM-Ebene (z.B. einen VM-Neustart, ein VM-Reimaging oder eine VM-Migration) anhalten, die sich auf die Quorumanforderungen für Ihre zustandsbehafteten Dienste auf diesem Knoten auswirken.

Für diese Berechtigung können die folgenden Werte festgelegt werden:

* Gold: Die Infrastrukturaufträge können für eine Dauer von zwei Stunden pro UD angehalten werden. Die Dauerhaftigkeit „Gold“ kann nur für VM-SKUs mit vollständigen Knoten wie D15_V2, G5 usw. aktiviert werden.
* Silber – Die Infrastrukturaufträge können für eine Dauer von 10 Minuten pro UD angehalten werden. Diese Option ist für alle virtuellen Standardcomputer ab einem Kern verfügbar.
* Bronze: Keine Berechtigungen Dies ist die Standardeinstellung und wird empfohlen, wenn Sie nur zustandslose Workloads in Ihrem Cluster ausführen.

Sie können die Dauerhaftigkeitsstufe für jeden Ihrer Knotentypen auswählen. Für einen Knotentyp kann die Dauerhaftigkeitsstufe „Gold“ oder „Silber“ und für einen anderen Knotentypen im selben Cluster die Stufe „Bronze“ festgelegt werden.**Sie müssen mindestens 5 Knoten für jeden Knotentyp mit der Dauerhaftigkeitsstufe „Gold“ oder „Silber“ verwalten**. 

**Vorteile der Verwendung der Dauerhaftigkeitsstufe „Silber“ oder „Gold“**
 
1. Reduziert die Anzahl der erforderlichen Schritte in einem Vorgang zur horizontalen Herunterskalierung (d.h., die Knotendeaktivierung und der Befehl „Remove-ServiceFabricNodeState“ werden automatisch ausgeführt).
2. Reduziert das Risiko eines Datenverlusts aufgrund eines vom Kunden initiierten direkten VM-SKU-Änderungsvorgangs oder von Azure-Infrastruktur-Vorgängen.
     
**Nachteile der Verwendung der Dauerhaftigkeitsstufe „Silber“ oder „Gold“**
 
1. Für Bereitstellungen in Ihrer VM-Skalierungsgruppe (und anderen verwandten Azure-Ressourcen) kann eine Verzögerung bzw. ein Timeout auftreten, oder sie können durch Probleme in Ihrem Cluster oder auf der Infrastrukturebene vollständig blockiert werden. 
2. Erhöht die Anzahl der [Lebenszyklusereignisse für Replikate](service-fabric-reliable-services-advanced-usage.md#stateful-service-replica-lifecycle ) (z.B. Tausch des primären Replikats) aufgrund von automatischen Knotendeaktivierungen während Azure-Infrastrukturvorgängen.



### <a name="recommendations-on-when-to-use-silver-or-gold-durability-levels"></a>Empfehlungen, wann die Dauerhaftigkeitsstufe „Silber“ oder „Gold“ verwendet werden soll

Verwenden Sie die Silber- oder Gold-Dauerhaftigkeit für alle Knotentypen, die zustandsbehaftete Dienste hosten, für die Sie eine häufige horizontale Herunterskalierung (Reduzierung der VM-Instanzanzahl) erwarten und Sie bevorzugen, dass sich Bereitstellungsvorgänge zugunsten einer Vereinfachung dieser Herunterskalierungsvorgänge verzögern. Die Szenarien mit horizontaler Hochskalierung (Hinzufügen von virtuellen Computerinstanzen) haben keinen Einfluss auf Ihre Wahl der Dauerhaftigkeitsstufe, dies ist nur bei horizontaler Hochskalierung der Fall.

### <a name="operational-recommendations-for-the-node-type-that-you-have-set-to-silver-or-gold-durability-level"></a>Betriebsempfehlungen für den Knotentyp, dessen Dauerhaftigkeitsstufe Sie auf „Silber“ oder „Gold“ festgelegt haben.

1. Halten Sie Ihren Cluster und Anwendungen jederzeit fehlerfrei, und stellen Sie sicher, dass Anwendungen rechtzeitig auf alle [Lebenszyklus-Dienstereignisse für Replikate](service-fabric-reliable-services-advanced-usage.md#stateful-service-replica-lifecycle) (z.B. Unterbrechung der Replikaterstellung) reagieren.
2. Führen Sie sicherere Methoden für VM-SKU-Änderungen (zentrales Hoch-/Herunterskalieren) ein: Das Ändern der VM-SKU einer VM-Skalierungsgruppe ist grundsätzlich ein unsicherer Vorgang und sollte möglichst vermieden werden. Mit dieser Vorgehensweise vermeiden Sie gängige Probleme.
    - **Für nicht primäre Knotentypen:** Es wird empfohlen, eine neue VM-Skalierungsgruppe zu erstellen, die Dienstplatzierungseinschränkungen so zu ändern, dass die neue VM-Skalierungsgruppe/der neue Knotentyp enthalten sind, und dann die Instanzenanzahl der alten VM-Skalierungsgruppe auf 0 zu reduzieren (nacheinander für alle Knoten, um sicherzustellen, dass das Entfernen der Knoten die Zuverlässigkeit des Clusters nicht beeinträchtigt).
    - **Für den primären Knotentyp:** Eine Änderung der VM-SKU des primären Knotentyps wird nicht empfohlen. Ist Kapazität der Grund für die neue SKU, wird empfohlen, weitere Instanzen hinzuzufügen oder wenn möglich einen neuen Cluster zu erstellen. Wenn es keine andere Möglichkeit gibt, nehmen Sie Änderungen an der Modelldefinition der VM-Skalierungsgruppe vor, um die neue SKU zu übernehmen. Wenn der Cluster nur einen Knotentyp enthält, stellen Sie sicher, dass alle Ihre zustandsbehafteten Anwendungen rechtzeitig auf alle [Lebenszyklus-Dienstereignisse für Replikate](service-fabric-reliable-services-advanced-usage.md#stateful-service-replica-lifecycle) (z.B. Unterbrechung der Replikaterstellung) reagieren und dass die Dauer für die Neuerstellung des Dienstreplikats weniger als fünf Minuten beträgt (für die Dauerhaftigkeitsstufe „Silber“). 
3. Verwalten Sie mindestens fünf Knoten für alle VM-Skalierungsgruppen, bei denen MR aktiviert ist.
4. Löschen Sie keine zufälligen VM-Instanzen, verwenden Sie immer die Funktion zum zentralen Herunterskalieren für VM-Skalierungsgruppen. Das Löschen von zufälligen VM-Instanzen kann zu Ungleichheiten in der auf UD und FD verteilten VM-Instanz führen. Eine solche Ungleichheit kann die Fähigkeit des Systems zum ordnungsgemäßen Lastenausgleich zwischen den Dienstinstanzen/Dienstreplikaten beeinträchtigen.
6. Wenn Sie die automatische Skalierung verwenden, legen Sie die Regeln so fest, dass die horizontale Herunterskalierung (Entfernung von VM-Instanzen) jeweils nur für einen Knoten ausgeführt wird. Es ist nicht sicher, mehrere Instanzen gleichzeitig herunterzuskalieren.
7. Wenn Sie einen primären Knotentyp herunterskalieren, sollten Sie ihn nie weiter herunterskalieren, als für die Zuverlässigkeitsstufe zulässig ist.


## <a name="the-reliability-characteristics-of-the-cluster"></a>Die Zuverlässigkeitsmerkmale des Clusters
Über die Zuverlässigkeitsstufe wird die Anzahl von Replikaten der Systemdienste festgelegt, die in diesem Cluster auf dem primären Knotentyp ausgeführt werden sollen. Je mehr Replikate vorhanden sind, desto größer ist die Zuverlässigkeit der Systemdienste in Ihrem Cluster.  

Für die Zuverlässigkeitsstufe können folgende Werte festgelegt werden:

* Platin: Systemdienste mit einer Replikatgruppen-Zielanzahl von 9 ausführen
* Gold: Systemdienste mit einer Replikatgruppen-Zielanzahl von 7 ausführen
* Silber: Systemdienste mit einer Replikatgruppen-Zielanzahl von 5 ausführen 
* Bronze: Systemdienste mit einer Replikatgruppen-Zielanzahl von 3 ausführen

> [!NOTE]
> Die gewählte Zuverlässigkeitsstufe bestimmt die Mindestanzahl von Knoten, über die Ihr primärer Knotentyp verfügen muss. 
> 
> 


### <a name="recommendations-for-the-reliability-tier"></a>Empfehlungen für die Zuverlässigkeitsstufe.

 Wenn Sie den Cluster vergrößern oder verkleinern (die Summe der VM-Instanzen in allen Knotentypen), müssen Sie die Zuverlässigkeit des Clusters von einer Stufe auf eine andere aktualisieren. Durch diesen Vorgang werden die erforderlichen Clusterupgrades ausgelöst, um die Replikatgruppenanzahl der Systemdienste zu ändern. Warten Sie, bis das laufende Upgrade abgeschlossen ist, ehe Sie Änderungen am Cluster vornehmen, beispielsweise Knoten hinzufügen.  Sie können den Fortschritt des Upgrades im Service Fabric Explorer oder durch Ausführen von [Get-ServiceFabricClusterUpgrade](/powershell/module/servicefabric/get-servicefabricclusterupgrade?view=azureservicefabricps) verfolgen.

Dies ist die Empfehlung für die Auswahl der Zuverlässigkeitsstufe.

| **Clustergröße** | **Zuverlässigkeitsstufe** |
| --- | --- |
| 1 |Geben Sie den Parameter für die Zuverlässigkeitsstufe nicht an, er wird vom System berechnet |
| 3 |Bronze |
| 5 oder 6|Silber |
| 7 oder 8 |Gold |
| 9 und höher |Platin |




## <a name="primary-node-type---capacity-guidance"></a>Primärer Knotentyp: Kapazitätsleitfaden

Nutzen Sie diesen Leitfaden, um die Kapazität Ihres primären Knotentyps zu planen.

1. **Anzahl von VM-Instanzen, um eine Produktionsworkload in Azure auszuführen:** Sie müssen die Mindestgröße 5 für den primären Knotentyp angeben. 
2. **Anzahl von VM-Instanzen, um eine Testworkload in Azure auszuführen:** Sie können die Mindestgröße 1 oder 3 für den primären Knotentyp angeben. Knotencluster 1 setzt eine spezifische Konfiguration voraus, daher wird bei diesem Cluster horizontales Skalieren nicht unterstützt. Knotencluster 1 besitzt keine Zuverlässigkeit. Daher müssen Sie in Ihrer Resource Manager-Vorlage diese Konfiguration entfernen bzw. nicht angeben (es reicht nicht, den Konfigurationswert nicht einzustellen). Wenn Sie über das Portal den Knotencluster 1 eingerichtet haben, wird die Konfiguration automatisch bearbeitet. Bei den Knotenclustern 1 und 3 wird die Ausführung von Produktionsworkloads nicht unterstützt. 
3. **VM-SKU**: Der primäre Knotentyp dient zur Ausführung der Systemdienste, daher muss die ausgewählte VM-SKU die gesamte Spitzenlast verarbeiten können, die Sie für den Cluster planen. Eine Analogie zur Veranschaulichung: Stellen Sie sich den primären Knotentyp als Ihre Lunge vor, die Ihr Gehirn mit Sauerstoff versorgt. Wenn Ihr Gehirn nicht genügend Sauerstoff erhält, funktioniert Ihr Körper nicht richtig. 

Da die Kapazitätsanforderungen eines Clusters von der Workload abhängig sind, die Sie für die Ausführung im Cluster planen, können wir Ihnen keine detaillierten Anleitungen für Ihre spezifische Workload bereitstellen. Im Folgenden finden Sie eine allgemeine Übersicht, um Ihnen beim Einstieg zu helfen

Für Produktionsworkloads 


- Die empfohlene VM-SKU ist D3 Standard oder D3_V2 Standard oder eine entsprechende SKU mit einer lokalen SSD-Kapazität von mindestens 14 GB.
- Die minimal unterstützte VM-SKU ist D1 Standard oder D1_V2 Standard oder eine entsprechende SKU mit einer lokalen SSD-Kapazität von mindestens 14 GB. 
- VM-SKUs mit partiellem Kern wie A0 Standard werden für Produktionsworkloads nicht unterstützt.
- A1 Standard-SKUs werden aus Leistungsgründen für Produktionsworkloads nicht unterstützt.


## <a name="non-primary-node-type---capacity-guidance-for-stateful-workloads"></a>Nicht primärer Knotentyp: Kapazitätsleitfaden für zustandsbehaftete Workloads

Diese Anleitung gilt für zustandsbehaftete Workloads mit [zuverlässigen Sammlungen oder Reliable Actors](service-fabric-choose-framework.md) von Service Fabric, die Sie auf dem nicht primären Knotentyp ausführen.


**Anzahl von VM-Instanzen**: Es wird empfohlen, zustandsbehaftete Produktionsworkloads mit mindestens 5 Zielreplikaten auszuführen. Dies bedeutet, dass Sie im stabilen Zustand in jeder Fehlerdomäne und jeder Upgradedomäne ein Replikat (aus einer Replikatgruppe) erhalten. Das gesamte Konzept der Zuverlässigkeitsstufen für den primären Knotentyp ist eine Möglichkeit, diese Einstellung für Systemdienste anzugeben. Daher gilt die gleiche Überlegung auch für Ihre zustandsbehafteten Dienste.

Für Produktionsworkloads wird die Mindestgröße 5 für den primären Knotentyp empfohlen, wenn Sie zustandsbehaftete Workloads auf dem Knoten ausführen.


**VM-SKU**: Dies ist der Knotentyp, in dem Ihre Anwendungsdienste ausgeführt werden. Daher muss die ausgewählte VM-SKU die Spitzenlast verarbeiten können, die Sie für jeden Knoten planen. Die Kapazitätsanforderungen des Knotentyps sind von der Workload abhängig, die Sie für die Ausführung im Cluster planen. Daher können wir Ihnen keine detaillierten Anleitungen für Ihre spezifische Workload bereitstellen. Im Folgenden finden Sie eine allgemeine Übersicht, um Ihnen beim Einstieg zu helfen

Für Produktionsworkloads 

- Die empfohlene VM-SKU ist D3 Standard oder D3_V2 Standard oder eine entsprechende SKU mit einer lokalen SSD-Kapazität von mindestens 14 GB.
- Die minimal unterstützte VM-SKU ist D1 Standard oder D1_V2 Standard oder eine entsprechende SKU mit einer lokalen SSD-Kapazität von mindestens 14 GB. 
- VM-SKUs mit partiellem Kern wie A0 Standard werden für Produktionsworkloads nicht unterstützt.
- A1 Standard-SKUs werden aus Leistungsgründen für Produktionsworkloads ausdrücklich nicht unterstützt.


## <a name="non-primary-node-type---capacity-guidance-for-stateless-workloads"></a>Nicht primärer Knotentyp: Kapazitätsleitfaden für zustandslose Workloads

Diese Anleitung gilt für zustandslose Workloads, die Sie auf dem nicht primären Knotentyp ausführen.

**Anzahl von VM-Instanzen:** Für zustandslose Produktionsworkloads wird die Mindestgröße 2 für den nicht primären Knotentyp unterstützt. So können Sie zwei zustandslose Instanzen Ihrer Anwendung ausführen und stellen sicher, dass Ihr Dienst auch beim Ausfall einer VM-Instanz weiter funktioniert. 

> [!NOTE]
> Wenn Ihr Cluster in einer niedrigeren Service Fabric-Version als 5.6 ausgeführt wird, führt ein Fehler während der Laufzeit (der mit 5.6 behoben ist) dazu, dass nach dem Herunterskalieren eines nicht primären Knotentyps auf weniger als 5 Knoten die Clusterintegrität verloren geht. Dies lässt sich nur beheben, indem Sie den Befehl [Remove-ServiceFabricNodeState](https://docs.microsoft.com/powershell/servicefabric/vlatest/Remove-ServiceFabricNodeState) mit dem entsprechenden Knotennamen aufrufen. Unter [Zentrales Hoch- oder Herunterskalieren eines Service Fabric-Clusters](service-fabric-cluster-scale-up-down.md) finden Sie weitere Informationen.
> 
>

**VM-SKU**: Dies ist der Knotentyp, in dem Ihre Anwendungsdienste ausgeführt werden. Daher muss die ausgewählte VM-SKU die Spitzenlast verarbeiten können, die Sie für jeden Knoten planen. Die Kapazitätsanforderungen des Knotentyps sind von der Workload abhängig, die Sie für die Ausführung im Cluster planen. Daher können wir Ihnen keine detaillierten Anleitungen für Ihre spezifische Workload bereitstellen. Im Folgenden finden Sie eine allgemeine Übersicht, um Ihnen beim Einstieg zu helfen

Für Produktionsworkloads 


- Die empfohlene VM-SKU ist D3 Standard oder D3_V2 Standard oder eine entsprechende SKU. 
- Die minimal unterstützte VM-SKU ist D1 Standard oder D1_V2 Standard oder eine entsprechende SKU. 
- VM-SKUs mit partiellem Kern wie A0 Standard werden für Produktionsworkloads nicht unterstützt.
- A1 Standard-SKUs werden aus Leistungsgründen für Produktionsworkloads nicht unterstützt.

<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->

## <a name="next-steps"></a>Nächste Schritte
Wenn Sie die Kapazitätsplanung abgeschlossen haben und einen Cluster einrichten, sollten Sie folgende Artikel lesen:

* [Service Fabric-Clustersicherheit](service-fabric-cluster-security.md)
* [Einführung in das Service Fabric-Integritätsmodell](service-fabric-health-introduction.md)
* [Beziehung zwischen Knotentypen und der VM-Skalierungsgruppe](service-fabric-cluster-nodetypes.md)

<!--Image references-->
[SystemServices]: ./media/service-fabric-cluster-capacity/SystemServices.png

