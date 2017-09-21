---
title: Azure Service Fabric-Knotentypen und VM-Skalierungsgruppen | Microsoft-Dokumentation
description: Erfahren Sie, wie Azure Service Fabric-Knotentypen mit VM-Skalierungsgruppen in Zusammenhang stehen und wie eine Remoteverbindung mit einer Skalierungsgruppeninstanz oder einem Clusterknoten hergestellt wird.
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
ms.sourcegitcommit: d24c6777cc6922d5d0d9519e720962e1026b1096
ms.openlocfilehash: 6cc3be57ed283cafa686d46d4b376c69f06301ea
ms.contentlocale: de-de
ms.lasthandoff: 09/14/2017

---
# <a name="azure-service-fabric-node-types-and-virtual-machine-scale-sets"></a>Azure Service Fabric-Knotentypen und VM-Skalierungsgruppen
VM-Skalierungsgruppen sind eine Azure Compute-Ressource. Sie können Skalierungsgruppen verwenden, um eine Sammlung virtueller Computer als Gruppe bereitzustellen und zu verwalten. Richten Sie eine separate Skalierungsgruppe für jeden Knotentyp ein, den Sie in einem Azure Service Fabric-Cluster definieren möchten. Sie können jeden Knotentyp einzeln zentral hoch- oder herunterskalieren, bei jedem Typ unterschiedliche Portgruppen öffnen und verschiedene Kapazitätsmetriken verwenden.

Die folgende Abbildung zeigt einen Cluster mit den beiden Knotentypen FrontEnd und BackEnd. Jeder Knotentyp verfügt über fünf Knoten.

![Ein Cluster mit zwei Knotentypen][NodeTypes]

## <a name="map-virtual-machine-scale-set-instances-to-nodes"></a>Zuordnen von VM-Skalierungsgruppeninstanzen zu Knoten
Wie in der Abbildung oben gezeigt, beginnen Skalierungsgruppeninstanzen mit der Instanz 0. Die Instanznummer wird dann jeweils um 1 erhöht. Die Knotennamen spiegeln die Nummerierung wider. Der Knoten „BackEnd_0“ ist z.B. die Instanz 0 der Skalierungsgruppe „BackEnd“. Diese bestimmte Skalierungsgruppe hat fünf Instanzen mit den Namen „BackEnd_0“, „BackEnd_1“, „BackEnd_2“, „BackEnd_3“ und „BackEnd_4“.

Wenn Sie eine Skalierungsgruppe zentral hochskalieren, wird eine neue Instanz erstellt. Der neue Skalierungsgruppen-Instanzname hat in der Regel folgendes Format: Skalierungsgruppenname + Nummer der nächsten Instanz. In diesem Beispiel lautet er „BackEnd_5“.

## <a name="map-scale-set-load-balancers-to-node-types-and-scale-sets"></a>Zuordnen des Lastenausgleichs von Skalierungsgruppen zu Knotentypen und Skalierungsgruppen
Wenn Sie Ihren Cluster aus dem Azure-Portal bereitgestellt oder die Azure Resource Manager-Beispielvorlage verwendet haben, werden alle Ressourcen aufgelistet, die zu einer Ressourcengruppe gehören. Der Lastenausgleich jeder Skalierungsgruppe oder jedes Knotentyps wird angezeigt. Für den Namen des Lastenausgleichs wird folgendes Format verwendet: **LB-&lt;Knotentypname&gt;**. Ein Beispiel ist „LB-sfcluster4doc-0“, wie in der folgenden Abbildung gezeigt:

![Ressourcen][Resources]
## <a name="remote-connect-to-a-virtual-machine-scale-set-instance-or-a-cluster-node"></a>Herstellen einer Remoteverbindung mit einer VM-Skalierungsgruppeninstanz oder einem Clusterknoten
Richten Sie eine separate Skalierungsgruppe für jeden Knotentyp ein, den Sie in einem Cluster definiert haben. Sie können die Knotentypen separat zentral hoch- oder herunterskalieren. Sie können auch andere VM-SKUs verwenden. Im Gegensatz zu Einzelinstanz-VMs besitzen Skalierungsgruppeninstanzen keine eigenen virtuellen IP-Adressen. Daher kann es schwierig sein, nach einer IP-Adresse und einem Port zum Herstellen einer Remoteverbindung mit einer bestimmten Instanz zu suchen.

Um eine IP-Adresse und einen Port für eine Remoteverbindung mit einer bestimmten Instanz zu finden, führen Sie die folgenden Schritte aus.

**Schritt 1:** Suchen der virtuellen IP-Adresse für den Knotentyp durch Abrufen der NAT-Eingangsregeln für das Remotedesktopprotokoll (RDP)

Rufen Sie zunächst die NAT-Eingangsregelwerte ab, die als Teil der Ressourcendefinition für `Microsoft.Network/loadBalancers` definiert wurden.

Wählen Sie im Azure-Portal auf der Seite für den Lastenausgleich **Einstellungen** > **NAT-Eingangsregeln** aus. Damit erhalten Sie die IP-Adresse und den Port zum Herstellen einer Remoteverbindung mit der ersten Skalierungsgruppeninstanz. 

![Load Balancer][LBBlade]

In der folgenden Abbildung lautet die IP-Adresse **104.42.106.156** und der Port **3389**.

![NAT-Regeln][NATRules]

**Schritt 2:** Suchen des Ports, mit dem eine Remoteverbindung mit der gewünschten Skalierungsgruppeninstanz bzw. dem gewünschten Knoten hergestellt werden kann

Skalierungsgruppeninstanzen sind Knoten zugeordnet. Verwenden Sie die Informationen zur Skalierungsgruppe, um den zu verwendenden Port zu ermitteln.

Die Zuweisung der Ports erfolgt in der aufsteigenden Reihenfolge gemäß der Skalierungsgruppeninstanz. Im obigen Beispiel für den Knotentyp „FrontEnd“ enthält die folgende Tabelle die Ports der fünf Knoteninstanzen. Wenden Sie dieselbe Zuordnung auf Ihre Skalierungsgruppeninstanz an.

| **VM-Skalierungsgruppeninstanz** | **Port** |
| --- | --- |
| FrontEnd_0 |3389 |
| FrontEnd_1 |3390 |
| FrontEnd_2 |3391 |
| FrontEnd_3 |3392 |
| FrontEnd_4 |3393 |
| FrontEnd_5 |3394 |

**Schritt 3:** Herstellen der Remoteverbindung mit der gewünschten Skalierungsgruppeninstanz

Die folgende Abbildung veranschaulicht das Herstellen einer Remotedesktopverbindung mit der Skalierungsgruppeninstanz „FrontEnd_1“:

![Remotedesktopverbindung][RDP]

## <a name="change-the-rdp-port-range-values"></a>Ändern der Werte des RDP-Portbereichs

### <a name="before-cluster-deployment"></a>Vor der Clusterbereitstellung
Wenn Sie den Cluster mit einer Resource Manager-Vorlage einrichten, geben Sie den Bereich in `inboundNatPools` an.

Navigieren Sie zur Ressourcendefinition für `Microsoft.Network/loadBalancers`. Suchen Sie die Beschreibung für `inboundNatPools`.  Ersetzen Sie die Werte `frontendPortRangeStart` und `frontendPortRangeEnd`.

![inboundNatPools-Werte][InboundNatPools]

### <a name="after-cluster-deployment"></a>Nach der Clusterbereitstellung
Das Ändern der Werte für den RDP-Portbereich ist nach der Bereitstellung des Clusters deutlich komplexer. Um sicherzustellen, dass die virtuellen Computer nicht wiederverwendet werden, verwenden Sie Azure PowerShell zum Festlegen neuer Werte. 

> [!NOTE]
> Stellen Sie sicher, dass Azure PowerShell 1.0 oder höher auf Ihrem Computer installiert ist. Falls Sie nicht über Azure PowerShell 1.0 oder höher verfügen, befolgen Sie die Schritte unter [Installieren und Konfigurieren von Azure PowerShell](/powershell/azure/overview).

1. Melden Sie sich beim Azure-Konto an. Wenn beim folgenden PowerShell-Befehl ein Fehler auftritt, überprüfen Sie die ordnungsgemäße Installation von PowerShell.

    ```
    Login-AzureRmAccount
    ```

2. Um die Details zum Lastenausgleich abzurufen und die Werte zur Beschreibung für `inboundNatPools` anzuzeigen, führen Sie folgenden Code aus:

    ```
    Get-AzureRmResource -ResourceGroupName <resource group name> -ResourceType Microsoft.Network/loadBalancers -ResourceName <load balancer name>
    ```

3. Legen Sie `frontendPortRangeEnd` und `frontendPortRangeStart` auf die gewünschten Werte fest.

    ```
    $PropertiesObject = @{
        #Property = value;
    }
    Set-AzureRmResource -PropertyObject $PropertiesObject -ResourceGroupName <resource group name> -ResourceType Microsoft.Network/loadBalancers -ResourceName <load balancer name> -ApiVersion <use the API version that is returned> -Force
    ```

## <a name="change-the-rdp-user-name-and-password-for-nodes"></a>Ändern des RDP-Benutzernamens und des Kennworts für Knoten

Führen Sie die folgenden Schritte aus, um das Kennwort für alle Knoten eines bestimmten Knotentyps zu ändern. Diese Änderungen gelten für alle aktuellen und zukünftigen Knoten in der Skalierungsgruppe.

1. Starten Sie PowerShell als Administrator. 
2. Führen Sie die folgenden Befehle aus, um sich anzumelden und Ihr Abonnement für die Sitzung auszuwählen. Ändern Sie den `SUBSCRIPTIONID`-Parameter in Ihre Abonnement-ID. 

    ```powershell
    Login-AzureRmAccount
    Get-AzureRmSubscription -SubscriptionId 'SUBSCRIPTIONID' | Select-AzureRmSubscription
    ```

3. Führen Sie das folgende Skript aus. Verwenden Sie die relevanten Werte für `NODETYPENAME`, `RESOURCEGROUP`, `USERNAME` und `PASSWORD`. Die Werte `USERNAME` und `PASSWORD` sind die neuen Anmeldeinformationen, die Sie in zukünftigen RDP-Sitzungen verwenden. 

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
* Weitere Informationen finden Sie unter [Übersicht über das Feature „Deploy Anywhere“ (umgebungsunabhängige Bereitstellung) und ein Vergleich mit Clustern, die von Azure verwaltet werden](service-fabric-deploy-anywhere.md).
* Erfahren Sie mehr über [Clustersicherheit](service-fabric-cluster-security.md).
* Erfahren Sie mehr über das [Service Fabric-SDK und die ersten Schritte](service-fabric-get-started.md).

<!--Image references-->
[NodeTypes]: ./media/service-fabric-cluster-nodetypes/NodeTypes.png
[Resources]: ./media/service-fabric-cluster-nodetypes/Resources.png
[InboundNatPools]: ./media/service-fabric-cluster-nodetypes/InboundNatPools.png
[LBBlade]: ./media/service-fabric-cluster-nodetypes/LBBlade.png
[NATRules]: ./media/service-fabric-cluster-nodetypes/NATRules.png
[RDP]: ./media/service-fabric-cluster-nodetypes/RDP.png

