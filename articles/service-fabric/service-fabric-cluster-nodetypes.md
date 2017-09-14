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
ms.date: 06/05/2017
ms.author: chackdan
ms.translationtype: HT
ms.sourcegitcommit: ce0189706a3493908422df948c4fe5329ea61a32
ms.openlocfilehash: 8c9e91d122591a19d34d944e2d9aaeb327cdafe4
ms.contentlocale: de-de
ms.lasthandoff: 09/05/2017

---
# <a name="the-relationship-between-service-fabric-node-types-and-virtual-machine-scale-sets"></a>Die Beziehung zwischen Service Fabric-Knotentypen und VM-Skalierungsgruppen
VM-Skalierungsgruppen sind eine Azure-Compute-Ressource. Sie können verwendet werden, um eine Sammlung virtueller Computer als Gruppe bereitzustellen und zu verwalten. Jeder Knotentyp, der in einem Service Fabric-Cluster definiert ist, wird als separate Skalierungsgruppe eines virtuellen Computers eingerichtet. Jeden Knotentyp kann dann unabhängig zentral hoch- oder herunterskaliert werden, bei jedem Typ können unterschiedliche Portgruppen geöffnet sein, und die Typen können verschiedene Kapazitätsmetriken aufweisen.

Der folgende Screenshot zeigt einen Cluster mit zwei Knotentypen: FrontEnd und BackEnd.  Jeder Knotentyp verfügt über fünf Knoten.

![Screenshot eines Clusters mit zwei Knotentypen][NodeTypes]

## <a name="mapping-virtual-machine-scale-set-instances-to-nodes"></a>Zuordnen von VM-Skalierungsgruppeninstanzen zu Knoten
Wie Sie oben sehen können, beginnen VM-Skalierungsgruppeninstanzen bei Instanz 0 und werden dann heraufgezählt. Die Namen spiegeln die Nummerierung wider. Beispiel: BackEnd_0 ist Instanz 0 der VM-Skalierungsgruppe BackEnd. Diese bestimmte VM-Skalierungsgruppe hat fünf Instanzen mit den Namen BackEnd_0, BackEnd_1, BackEnd_2, BackEnd_3 und BackEnd_4.

Wenn Sie eine VM-Skalierungsgruppe zentral hochskalieren, wird eine neue Instanz erstellt. Der neue VM-Skalierungsgruppen-Instanzname hat in der Regel folgende Struktur: VM-Skalierungsgruppenname + Nummer der nächsten Instanz. In diesem Beispiel lautet er „BackEnd_5“.

## <a name="mapping-virtual-machine-scale-set-load-balancers-to-each-node-typevm-scale-set"></a>Zuordnen des VM-Skalierungsgruppen-Lastenausgleichs zu jedem Knotentyp/jeder VM-Skalierungsgruppe
Wenn Sie Ihren Cluster aus dem Portal bereitgestellt oder die Resource Manager-Beispielvorlage verwendet haben, erhalten Sie eine Liste aller Ressourcen, die zu einer Ressourcengruppe gehören. Der Lastenausgleich jeder VM-Skalierungsgruppe oder jedes Knotentyps wird angezeigt.

Der Name würde etwa folgendermaßen lauten: **LB-&lt;Knotentypname&gt;**. Z. B. „LB-sfcluster4doc-0“, wie in diesem Screenshot gezeigt:

![Ressourcen][Resources]

## <a name="remote-connect-to-a-virtual-machine-scale-set-instance-or-a-cluster-node"></a>Herstellen einer Remoteverbindung mit einer VM-Skalierungsgruppeninstanz oder einem Clusterknoten
Jeder Knotentyp, der in einem Cluster definiert ist, wird als separate VM-Skalierungsgruppe eingerichtet.  Dies bedeutet, dass die Knotentypen unabhängig zentral hoch- bzw. herunterskaliert werden können. Darüber hinaus können sie aus unterschiedlichen VM-SKUs erstellt werden. Im Gegensatz zu Einzelinstanz-VMs erhalten die VM-Skalierungsgruppeninstanzen keine eigene virtuelle IP-Adresse. Daher kann es für Sie eine gewisse Herausforderung sein, nach einer IP-Adresse und einem Port zum Herstellen einer Remoteverbindung mit einer bestimmten Instanz zu suchen.

Mit den folgenden Schritten können Sie diese ermitteln.

### <a name="step-1-find-out-the-virtual-ip-address-for-the-node-type-and-then-inbound-nat-rules-for-rdp"></a>Schritt 1: Suchen der virtuellen IP-Adresse für den Knotentyp und dann der Regeln für eingehenden NAT-Datenverkehr für RDP
Hierzu müssen Sie die Werte der Regeln für eingehenden NAT-Datenverkehr abrufen, die als Teil der Ressourcendefinition für **Microsoft.Network/loadBalancers**definiert wurden.

Wechseln Sie in das Portal zum Blatt „Load Balancer“ und dann zu **Einstellungen**.

![LBBlade][LBBlade]

Klicken Sie in **Einstellungen** auf **NAT-Eingangsregeln**. Jetzt erhalten Sie die IP-Adresse und den Port zum Herstellen einer Remoteverbindung mit der ersten VM-Skalierungsgruppeninstanz. Im folgenden Screenshot sind dies **104.42.106.156** und **3389**.

![NATRules][NATRules]

### <a name="step-2-find-out-the-port-that-you-can-use-to-remote-connect-to-the-specific-virtual-machine-scale-set-instancenode"></a>Schritt 2: Suchen des Ports, mit dem Sie eine Remoteverbindung mit der bestimmten VM-Skalierungsgruppeninstanz/dem bestimmten Knoten herstellen können
Weiter oben in diesem Dokument wurde die Zuordnung der VM-Skalierungsgruppeninstanzen zu den Knoten erörtert. Anhand dessen wird der richtige Port ermittelt.

Die Zuweisung der Ports erfolgt in der aufsteigenden Reihenfolge der VM-Skalierungsgruppeninstanz. In meinem Beispiel für den Knotentyp „FrontEnd“ erhalten die fünf Instanzen also die folgenden Ports. Jetzt müssen Sie die gleiche Zuordnung für Ihre VM-Skalierungsgruppeninstanz vornehmen.

| **VM-Skalierungsgruppeninstanz** | **Port** |
| --- | --- |
| FrontEnd_0 |3389 |
| FrontEnd_1 |3390 |
| FrontEnd_2 |3391 |
| FrontEnd_3 |3392 |
| FrontEnd_4 |3393 |
| FrontEnd_5 |3394 |

### <a name="step-3-remote-connect-to-the-specific-virtual-machine-scale-set-instance"></a>Schritt 3: Herstellen der Remoteverbindung mit der bestimmten VM-Skalierungsgruppeninstanz
Im folgenden Screenshot wird die Remotedesktopverbindung zum Herstellen der Verbindung mit „FrontEnd_1“ verwendet:

![RDP][RDP]

## <a name="how-to-change-the-rdp-port-range-values"></a>Ändern der Werte des RDP-Portbereichs
### <a name="before-cluster-deployment"></a>Vor der Clusterbereitstellung
Wenn Sie den Cluster mithilfe einer Resource Manager-Vorlage einrichten, können Sie den Bereich in **inboundNatPools** angeben.

Wechseln Sie zur Ressourcendefinition für **Microsoft.Network/loadBalancers**. Darunter finden Sie die Beschreibung für **inboundNatPools**.  Ersetzen Sie die Werte *frontendPortRangeStart* und *frontendPortRangeEnd*.

![inboundNatPools][InboundNatPools]

### <a name="after-cluster-deployment"></a>Nach der Clusterbereitstellung
Nach der Clusterbereitstellung ist es etwas komplizierter und kann dazu führen, dass die virtuellen Computern neu gestartet werden. Legen Sie neue Werte mit Azure PowerShell fest. Stellen Sie sicher, dass Azure PowerShell 1.0 oder höher auf Ihrem Computer installiert ist. Falls Sie nicht über Azure PowerShell 1.0 oder höher verfügen, sollten Sie unbedingt die Schritte unter [Installieren und Konfigurieren von Azure PowerShell](/powershell/azure/overview) ausführen.

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

## <a name="how-to-change-the-rdp-username--password-for-nodes"></a>So ändern Sie den RDP-Benutzernamen und das Kennwort für Knoten

Die folgenden Schritte beschreiben, wie Sie das Kennwort für alle Knoten eines bestimmten Knotentyps ändern. Diese Änderungen gelten für alle aktuellen und zukünftigen Knoten in der VM-Skalierungsgruppe.

### <a name="step-1-open-powershell-with-elevated-privileges-administrator-mode"></a>Schritt 1: Öffnen Sie PowerShell mit erhöhten Rechten (im Administratormodus). 
### <a name="step-2-run-the-following-commands-to-log-in-and-select-your-subscription-for-the-session-change-the-subscriptionid-parameter-to-your-subscription-id"></a>Schritt 2: Führen Sie die folgenden Befehle zum Anmelden aus, und wählen Sie Ihr Abonnement für die Sitzung. Ändern Sie den `SUBSCRIPTIONID`-Parameter in Ihre Abonnement-ID. 

```powershell
Login-AzureRmAccount
Get-AzureRmSubscription -SubscriptionId 'SUBSCRIPTIONID' | Select-AzureRmSubscription
```

### <a name="step-3-run-the-following-script-with-the-appropriate-nodetypename-resourcegroup-username-and-password-values-the-username-and-password-values-will-be-the-new-credentials-that-should-be-used-in-future-rdp-sessions"></a>Schritt 3: Führen Sie das folgende Skript mit den entsprechenden Werten für `NODETYPENAME`, `RESOURCEGROUP`, `USERNAME` und `PASSWORD` aus. Die Werte `USERNAME` und `PASSWORD` sind die neuen Anmeldeinformationen, die in zukünftigen RDP-Sitzungen verwendet werden sollen. 

```powershell
$nodeTypeName = 'NODETYPENAME'
$resourceGroup = 'RESOURCEGROUP'
$publicConfig = @{'UserName' = 'USERNAME'}
$privateConfig = @{'Password' = 'PASSWORD'}
$extName = 'VMAccessAgent'
$publisher = 'Microsoft.Compute'
$node = Get-AzureRmVmss -ResourceGroupName $resourceGroup -VMScaleSetName $nodeTypeName
$node = Add-AzureRmVmssExtension -VirtualMachineScaleSet $node -Name $extName -Publisher $publisher -Setting $publicConfig -ProtectedSetting $privateConfig -Type $extName -TypeHandlerVersion '2.0' -AutoUpgradeMinorVersion $true

Update-AzureRmVmss -ResourceGroupName $resourceGroup -Name $nodeTypeName -VirtualMachineScaleSet $node
```

## <a name="next-steps"></a>Nächste Schritte
* [Übersicht über das Feature „Deploy Anywhere“ (umgebungsunabhängige Bereitstellung) und ein Vergleich mit Clustern, die von Azure verwaltet werden](service-fabric-deploy-anywhere.md)
* [Clustersicherheit](service-fabric-cluster-security.md)
* [ Service Fabric-SDK und erste Schritte](service-fabric-get-started.md)

<!--Image references-->
[NodeTypes]: ./media/service-fabric-cluster-nodetypes/NodeTypes.png
[Resources]: ./media/service-fabric-cluster-nodetypes/Resources.png
[InboundNatPools]: ./media/service-fabric-cluster-nodetypes/InboundNatPools.png
[LBBlade]: ./media/service-fabric-cluster-nodetypes/LBBlade.png
[NATRules]: ./media/service-fabric-cluster-nodetypes/NATRules.png
[RDP]: ./media/service-fabric-cluster-nodetypes/RDP.png

