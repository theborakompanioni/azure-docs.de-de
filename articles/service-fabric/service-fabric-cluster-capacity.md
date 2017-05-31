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
ms.date: 05/05/2017
ms.author: chackdan
ms.translationtype: Human Translation
ms.sourcegitcommit: 2db2ba16c06f49fd851581a1088df21f5a87a911
ms.openlocfilehash: c38b337d67b518f68be6dc8255fa97b78ba89dae
ms.contentlocale: de-de
ms.lasthandoff: 05/08/2017


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

Der Cluster kann über mehrere Knotentypen verfügen. Der primäre Knotentyp (der erste, den Sie im Portal definieren) muss bei Clustern, die für Produktionsworkloads eingesetzt werden, jedoch mindestens fünf VMs aufweisen (für Testcluster sind mindestens drei VMs erforderlich). Wenn Sie den Cluster anhand einer Resource Manager-Vorlage erstellen, enthält die Knotentypdefinition ein Attribut **is Primary**. Der primäre Knotentyp ist der Knotentyp mit den Service Fabric-Systemdiensten.  

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

* Gold: Die Infrastrukturaufträge können für eine Dauer von 2 Stunden pro UD angehalten werden. Die Dauerhaftigkeit „Gold“ kann nur für VM-SKUs mit vollständigen Knoten wie D15_V2, G5 usw. aktiviert werden.
* Silber: Die Infrastrukturaufträge können für eine Dauer von 30 Minuten pro UD angehalten werden. (Diese Option steht derzeit nicht zur Verfügung. Sobald aktiviert, wird diese Option für alle Standard-VMs ab einem Kern verfügbar sein.)
* Bronze: Keine Berechtigungen Dies ist die Standardoption.

## <a name="the-reliability-characteristics-of-the-cluster"></a>Die Zuverlässigkeitsmerkmale des Clusters
Über die Zuverlässigkeitsstufe wird die Anzahl von Replikaten der Systemdienste festgelegt, die in diesem Cluster auf dem primären Knotentyp ausgeführt werden sollen. Je mehr Replikate vorhanden sind, desto größer ist die Zuverlässigkeit der Systemdienste in Ihrem Cluster.  

Für die Zuverlässigkeitsstufe können folgende Werte festgelegt werden:

* Platin: Systemdienste mit einer Replikatgruppen-Zielanzahl von 9 ausführen
* Gold: Systemdienste mit einer Replikatgruppen-Zielanzahl von 7 ausführen
* Silber: Systemdienste mit einer Replikatgruppen-Zielanzahl von 5 ausführen
* Bronze: Systemdienste mit einer Replikatgruppen-Zielanzahl von 3 ausführen

> [!NOTE]
> Die gewählte Zuverlässigkeitsstufe bestimmt die Mindestanzahl von Knoten, über die Ihr primärer Knotentyp verfügen muss. Die Zuverlässigkeitsstufe hat keinen Einfluss auf die maximale Größe des Clusters. Sie können also einen Cluster mit 20 Knoten ausführen, der über die Zuverlässigkeitsstufe „Bronze“ verfügt.
> 
> 

 Sie können die Zuverlässigkeitsstufe Ihres Clusters jederzeit ändern. Durch diesen Vorgang werden die erforderlichen Clusterupgrades ausgelöst, um die Replikatgruppenanzahl der Systemdienste zu ändern. Warten Sie, bis das laufende Upgrade abgeschlossen ist, ehe Sie Änderungen am Cluster vornehmen, beispielsweise Knoten hinzufügen.  Sie können den Fortschritt des Upgrades im Service Fabric Explorer oder durch Ausführen von [Get-ServiceFabricClusterUpgrade](/powershell/module/servicefabric/get-servicefabricclusterupgrade?view=azureservicefabricps) verfolgen.


## <a name="primary-node-type---capacity-guidance"></a>Primärer Knotentyp: Kapazitätsleitfaden

Nutzen Sie diesen Leitfaden, um die Kapazität Ihres primären Knotentyps zu planen.

1. **Anzahl von VM-Instanzen, um eine Produktionsworkload in Azure auszuführen: **Sie müssen Mindestgröße 5 für den primären Knotentyp angeben.
2. **Anzahl von VM-Instanzen, um eine Testworkload in Azure** auszuführen: Sie können Mindestgröße 1 oder 3 für den primären Knotentyp angeben. Knotencluster 1 setzt eine spezifische Konfiguration voraus, daher wird bei diesem Cluster horizontales Skalieren nicht unterstützt. Knotencluster 1 besitzt keine Zuverlässigkeit. Daher müssen Sie in Ihrer Resource Manager-Vorlage diese Konfiguration entfernen bzw. nicht angeben (es reicht nicht, den Konfigurationswert nicht einzustellen). Wenn Sie über das Portal den Knotencluster 1 eingerichtet haben, wird die Konfiguration automatisch bearbeitet. Bei den Knotenclustern 1 und 3 wird die Ausführung von Produktionsworkloads nicht unterstützt. 
3. **VM-SKU**: Der primäre Knotentyp dient zur Ausführung der Systemdienste, daher muss die ausgewählte VM-SKU die gesamte Spitzenlast verarbeiten können, die Sie für den Cluster planen. Eine Analogie zur Veranschaulichung: Stellen Sie sich den primären Knotentyp als Ihre Lunge vor, die Ihr Gehirn mit Sauerstoff versorgt. Wenn Ihr Gehirn nicht genügend Sauerstoff erhält, funktioniert Ihr Körper nicht richtig. 

Die Kapazitätsanforderungen eines Clusters sind von der Workload abhängig, die Sie für die Ausführung im Cluster planen. Daher können wir Ihnen keine detaillierten Anleitungen für Ihre spezifische Workload bereitstellen. Im Folgenden finden Sie eine allgemeine Übersicht, um Ihnen beim Einstieg zu helfen

Für Produktionsworkloads 


- Die empfohlene VM-SKU ist D3 Standard oder D3_V2 Standard oder eine entsprechende SKU mit einer lokalen SSD-Kapazität von mindestens 14 GB.
- Die minimal unterstützte VM-SKU ist D1 Standard oder D1_V2 Standard oder eine entsprechende SKU mit einer lokalen SSD-Kapazität von mindestens 14 GB. 
- VM-SKUs mit partiellem Kern wie A0 Standard werden für Produktionsworkloads nicht unterstützt.
- A1 Standard-SKUs werden aus Leistungsgründen für Produktionsworkloads nicht unterstützt.


## <a name="non-primary-node-type---capacity-guidance-for-stateful-workloads"></a>Nicht primärer Knotentyp: Kapazitätsleitfaden für zustandsbehaftete Workloads

Lesen Sie folgende Informationen für Workloads, die zuverlässige Service Fabric-Sammlungen oder Reliable Actors verwenden. Erfahren Sie mehr zu [Programmiermodellen](service-fabric-choose-framework.md).

1. **Anzahl von VM-Instanzen**: Es wird empfohlen, zustandsbehaftete Produktionsworkloads mit mindestens 5 Zielreplikaten auszuführen. Dies bedeutet, dass Sie im stabilen Zustand in jeder Fehlerdomäne und jeder Upgradedomäne ein Replikat (aus einer Replikatgruppe) erhalten. Das gesamte Konzept der Zuverlässigkeitsstufen für den primären Knotentyp ist eine Möglichkeit, diese Einstellung für Systemdienste anzugeben.

Für Produktionsworkloads wird die Mindestgröße 5 für den primären Knotentyp empfohlen, wenn Sie zustandsbehaftete Workloads auf dem Knoten ausführen.

2. **VM-SKU**: Dies ist der Knotentyp, in dem Ihre Anwendungsdienste ausgeführt werden. Daher muss die ausgewählte VM-SKU die Spitzenlast verarbeiten können, die Sie für jeden Knoten planen. Die Kapazitätsanforderungen des Knotentyps sind von der Workload abhängig, die Sie für die Ausführung im Cluster planen. Daher können wir Ihnen keine detaillierten Anleitungen für Ihre spezifische Workload bereitstellen. Im Folgenden finden Sie eine allgemeine Übersicht, um Ihnen beim Einstieg zu helfen

Für Produktionsworkloads 

- Die empfohlene VM-SKU ist D3 Standard oder D3_V2 Standard oder eine entsprechende SKU mit einer lokalen SSD-Kapazität von mindestens 14 GB.
- Die minimal unterstützte VM-SKU ist D1 Standard oder D1_V2 Standard oder eine entsprechende SKU mit einer lokalen SSD-Kapazität von mindestens 14 GB. 
- VM-SKUs mit partiellem Kern wie A0 Standard werden für Produktionsworkloads nicht unterstützt.
- A1 Standard-SKUs werden aus Leistungsgründen für Produktionsworkloads ausdrücklich nicht unterstützt.


## <a name="non-primary-node-type---capacity-guidance-for-stateless-workloads"></a>Nicht primärer Knotentyp: Kapazitätsleitfaden für zustandslose Workloads

Lesen Sie folgende Informationen für zustandslose Workloads

**Anzahl von VM-Instanzen:** Für zustandslose Produktionsworkloads wird die Mindestgröße 2 für den nicht primären Knotentyp unterstützt. So können Sie zwei zustandslose Instanzen Ihrer Anwendung ausführen und stellen sicher, dass Ihr Dienst auch beim Ausfall einer VM-Instanz weiter funktioniert. 

> [!NOTE]
> Wenn Ihr Cluster in einer niedrigeren Service Fabric-Version als 5.6 ausgeführt wird, führt ein Fehler während der Laufzeit (der mit 5.6 behoben ist) dazu, dass nach dem Herunterskalieren eines nicht primären Knotentyps auf weniger als 5 Knoten die Clusterintegrität verloren geht. Dies lässt sich nur beheben, indem Sie den Befehl [Remove-ServiceFabricNodeState](https://docs.microsoft.com/powershell/servicefabric/vlatest/Remove-ServiceFabricNodeState) mit dem entsprechenden Knotennamen aufrufen. Unter [Zentrales Hoch- oder Herunterskalieren eines Service Fabric-Clusters](service-fabric-cluster-scale-up-down.md) finden Sie weitere Informationen.
> 
>

**VM-SKU**: Dies ist der Knotentyp, in dem Ihre Anwendungsdienste ausgeführt werden. Daher muss die ausgewählte VM-SKU die Spitzenlast verarbeiten können, die Sie für jeden Knoten planen. Die Kapazitätsanforderungen des Knotentyps sind von der Workload abhängig, die Sie für die Ausführung im Cluster planen. Daher können wir Ihnen keine detaillierten Anleitungen für Ihre spezifische Workload bereitstellen. Im Folgenden finden Sie eine allgemeine Übersicht, um Ihnen beim Einstieg zu helfen.

Für Produktionsworkloads 


- Die empfohlene VM-SKU ist D3 Standard oder D3_V2 Standard oder eine entsprechende SKU. 
- Die minimal unterstützte VM-SKU ist D1 Standard oder D1_V2 Standard oder eine entsprechende SKU. 
- VM-SKUs mit partiellem Kern wie A0 Standard werden für Produktionsworkloads nicht unterstützt.
- A1 Standard-SKUs werden aus Leistungsgründen für Produktionsworkloads ausdrücklich nicht unterstützt.

<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->

## <a name="next-steps"></a>Nächste Schritte
Wenn Sie die Kapazitätsplanung abgeschlossen haben und einen Cluster einrichten, sollten Sie folgende Artikel lesen:

* [Service Fabric-Clustersicherheit](service-fabric-cluster-security.md)
* [Einführung in das Service Fabric-Integritätsmodell](service-fabric-health-introduction.md)
* [Beziehung zwischen Knotentypen und der VM-Skalierungsgruppe](service-fabric-cluster-nodetypes.md)

<!--Image references-->
[SystemServices]: ./media/service-fabric-cluster-capacity/SystemServices.png

