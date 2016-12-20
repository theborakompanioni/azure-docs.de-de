---
title: Service Fabric-Knotentypen und VM-Skalierungsgruppen | Microsoft Docs
description: Beschreibt, wie Service Fabric-Knotentypen mit VM-Skalierungsgruppen in Zusammenhang stehen, und wie die Remoteverbindung mit einer VM-Skalierungsgruppeninstanz oder einem Clusterknoten hergestellt wird.
services: service-fabric
documentationcenter: .net
author: ChackDan
manager: timlt
editor: 
ms.assetid: 5441e7e0-d842-4398-b060-8c9d34b07c48
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 09/09/2016
ms.author: chackdan
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: 04acc5a82e658c216e25b96ecbfa42c88177506f


---
# <a name="the-relationship-between-service-fabric-node-types-and-virtual-machine-scale-sets"></a>Die Beziehung zwischen Service Fabric-Knotentypen und VM-Skalierungsgruppen
VM-Skalierungsgruppen sind eine Azure-Computeressource, mit der Sie eine Sammlung von virtuellen Computern bereitstellen und verwalten können. Jeder Knotentyp, der in einem Service Fabric-Cluster definiert ist, wird als separate VM-Skalierungsgruppe festgelegt. Jeden Knotentyp kann dann unabhängig zentral hoch- oder herunterskaliert werden, bei jedem Typ können unterschiedliche Portgruppen geöffnet sein, und die Typen können verschiedene Kapazitätsmetriken aufweisen.

Der folgende Screenshot zeigt einen Cluster mit zwei Knotentypen: FrontEnd und BackEnd.  Jeder Knotentyp verfügt über fünf Knoten.

![Screenshot eines Clusters mit zwei Knotentypen][NodeTypes]

## <a name="mapping-vm-scale-set-instances-to-nodes"></a>Zuordnen von VM-Skalierungsgruppeninstanzen zu Knoten
Wie Sie oben sehen können, beginnen VM-Skalierungsgruppeninstanzen bei Instanz 0 und werden dann heraufgezählt. Die Namen spiegeln die Nummerierung wider. Beispiel: „BackEnd_0“ ist Instanz 0 der VM-Skalierungsgruppe „BackEnd“. Diese bestimmte VM-Skalierungsgruppe hat fünf Instanzen mit den Namen „BackEnd_0“, „BackEnd_1“, „BackEnd_2“, „BackEnd_3“ und „BackEnd_4“.

Wenn Sie eine VM-Skalierungsgruppe zentral hochskalieren, wird eine neue Instanz erstellt. Der neue VM-Skalierungsgruppen-Instanzname hat in der Regel folgende Struktur: VM-Skalierungsgruppenname + Nummer der nächsten Instanz. In diesem Beispiel lautet er „BackEnd_5“.

## <a name="mapping-vm-scale-set-load-balancers-to-each-node-typevm-scale-set"></a>Zuordnen des VM-Skalierungsgruppen-Lastenausgleichs für jeden Knotentyp/jede VM-Skalierungsgruppe
Wenn Sie Ihren Cluster über das Portal bereitgestellt oder die bereitgestellte Resource Manager-Vorlage verwendet haben und dann eine Liste aller Ressourcen in einer Ressourcengruppe abrufen, wird der Lastenausgleich für jede VM-Skalierungsgruppe oder jeden Knotentyp angezeigt.

Der Name würde etwa folgendermaßen lauten: **LB-&lt;Knotentypname&gt;**. Z. B. „LB-sfcluster4doc-0“, wie in diesem Screenshot gezeigt:

![Ressourcen][Resources]

## <a name="remote-connect-to-a-vm-scale-set-instance-or-a-cluster-node"></a>Herstellen einer Remoteverbindung mit einer VM-Skalierungsgruppeninstanz oder einem Clusterknoten
Jeder Knotentyp, der in einem Cluster definiert ist, wird als separate VM-Skalierungsgruppe eingerichtet.  Dies bedeutet, dass die Knotentypen unabhängig zentral hoch- bzw. herunterskaliert und aus verschiedenen VM-SKUs erstellt werden können. Im Gegensatz zu Einzelinstanz-VMs erhalten die VM-Skalierungsgruppeninstanzen keine eigene virtuelle IP-Adresse. Daher kann es für Sie eine gewisse Herausforderung sein, nach einer IP-Adresse und einem Port zum Herstellen einer Remoteverbindung mit einer bestimmten Instanz zu suchen.

Mit den folgenden Schritten können Sie diese ermitteln.

### <a name="step-1-find-out-the-virtual-ip-address-for-the-node-type-and-then-inbound-nat-rules-for-rdp"></a>Schritt 1: Suchen der virtuellen IP-Adresse für den Knotentyp und dann der Regeln für eingehenden NAT-Datenverkehr für RDP
Hierzu müssen Sie die Werte der Regeln für eingehenden NAT-Datenverkehr abrufen, die als Teil der Ressourcendefinition für **Microsoft.Network/loadBalancers**definiert wurden.

Wechseln Sie in das Portal zum Blatt „Load Balancer“ und dann zu **Einstellungen**.

![LBBlade][LBBlade]

Klicken Sie in **Einstellungen** auf **NAT-Eingangsregeln**. Jetzt erhalten Sie die IP-Adresse und den Port zum Herstellen einer Remoteverbindung mit der ersten VM-Skalierungsgruppeninstanz. Im folgenden Screenshot sind dies **104.42.106.156** und **3389**.

![NATRules][NATRules]

### <a name="step-2-find-out-the-port-that-you-can-use-to-remote-connect-to-the-specific-vm-scale-set-instancenode"></a>Schritt 2: Suchen des Ports, mit dem Sie eine Remoteverbindung mit der bestimmten VM-Skalierungsgruppeninstanz/dem bestimmten Knoten herstellen können
Weiter oben in diesem Dokument habe ich die Zuordnung der VM-Skalierungsgruppeninstanzen zu den Knoten erörtert. Wir verwenden diese, um den richtigen Port zu ermitteln.

Die Zuweisung der Ports erfolgt in der aufsteigenden Reihenfolge der VM-Skalierungsgruppeninstanz. In meinem Beispiel für den Knotentyp „FrontEnd“ erhalten die fünf Instanzen also die folgenden Ports. Jetzt müssen Sie die gleiche Zuordnung für Ihre VM-Skalierungsgruppeninstanz vornehmen.

| **VM-Skalierungsgruppeninstanz** | **Port** |
| --- | --- |
| FrontEnd_0 |3389 |
| FrontEnd_1 |3390 |
| FrontEnd_2 |3391 |
| FrontEnd_3 |3392 |
| FrontEnd_4 |3393 |
| FrontEnd_5 |3394 |

### <a name="step-3-remote-connect-to-the-specific-vm-scale-set-instance"></a>Schritt 3: Herstellen der Remoteverbindung mit der bestimmten VM-Skalierungsgruppeninstanz
Im folgenden Screenshot wird die Remotedesktopverbindung zum Herstellen der Verbindung mit „FrontEnd_1“ verwendet:

![RDP][RDP]

## <a name="how-to-change-the-rdp-port-range-values"></a>Ändern der Werte des RDP-Portbereichs
### <a name="before-cluster-deployment"></a>Vor der Clusterbereitstellung
Wenn Sie den Cluster mithilfe einer Resource Manager-Vorlage einrichten, können Sie den Bereich in **inboundNatPools**angeben.

Wechseln Sie zur Ressourcendefinition für **Microsoft.Network/loadBalancers**. Darunter finden Sie die Beschreibung für **inboundNatPools**.  Ersetzen Sie die Werte *frontendPortRangeStart* und *frontendPortRangeEnd*.

![inboundNatPools][InboundNatPools]

### <a name="after-cluster-deployment"></a>Nach der Clusterbereitstellung
Dies ist etwas komplizierter und kann dazu führen, dass die virtuellen Computern neu gestartet werden. Sie müssen nun neue Werte mit Azure PowerShell festlegen. Stellen Sie sicher, dass Azure PowerShell 1.0 oder höher auf Ihrem Computer installiert ist. Falls noch nicht erfolgt, sollten Sie unbedingt die Schritte unter [Installieren und Konfigurieren von Azure PowerShell](../powershell-install-configure.md)

Melden Sie sich beim Azure-Konto an. Wenn dieser PowerShell-Befehl aus irgendeinem Grund Fehler verursacht, sollten Sie prüfen, ob Azure PowerShell ordnungsgemäß installiert ist.

```
Login-AzureRmAccount
```

Wenn Sie Folgendes ausführen, um die Details für den Lastenausgleich abzurufen, sehen Sie die Werte, denn Sie finden die Beschreibung für **inboundNatPools**:

```
Get-AzureRmResource -ResourceGroupName <RGname> -ResourceType Microsoft.Network/loadBalancers -ResourceName <load balancer name>
```

Legen Sie jetzt *frontendPortRangeEnd* und *frontendPortRangeStart* auf die gewünschten Werte fest.

```
$PropertiesObject = @{
    #Property = value;
}
Set-AzureRmResource -PropertyObject $PropertiesObject -ResourceGroupName <RG name> -ResourceType Microsoft.Network/loadBalancers -ResourceName <load Balancer name> -ApiVersion <use the API version that get returned> -Force
```


## <a name="next-steps"></a>Nächste Schritte
* [Übersicht über das Feature „Deploy Anywhere“ (umgebungsunabhängige Bereitstellung) und ein Vergleich mit Clustern, die von Azure verwaltet werden](service-fabric-deploy-anywhere.md)
* [Clustersicherheit](service-fabric-cluster-security.md)
* [ Service Fabric-SDK und erste Schritte](service-fabric-get-started.md)

<!--Image references-->
[NodeTypes]: ./media/service-fabric-cluster-nodetypes/NodeTypes.png
[Ressourcen]: ./media/service-fabric-cluster-nodetypes/Resources.png
[InboundNatPools]: ./media/service-fabric-cluster-nodetypes/InboundNatPools.png
[LBBlade]: ./media/service-fabric-cluster-nodetypes/LBBlade.png
[NATRules]: ./media/service-fabric-cluster-nodetypes/NATRules.png
[RDP]: ./media/service-fabric-cluster-nodetypes/RDP.png



<!--HONumber=Nov16_HO3-->


