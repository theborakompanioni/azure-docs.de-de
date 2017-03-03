---
title: "Peering in virtuellen Azure-Netzwerken – PowerShell | Microsoft-Dokumentation"
description: Erfahren Sie, wie Sie mit Azure PowerShell ein Peering in einem virtuellen Netzwerk erstellen.
services: virtual-network
documentationcenter: 
author: NarayanAnnamalai
manager: jefco
editor: 
tags: azure-resource-manager
ms.assetid: dac579bd-7545-461a-bdac-301c87434c84
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/14/2016
ms.author: narayan; annahar
translationtype: Human Translation
ms.sourcegitcommit: 3fcd6583b415cea6b2151651297c55c93e59c796
ms.openlocfilehash: b0375a99f5ea3d6af2d3ead382f9a43f1fd285f0
ms.lasthandoff: 02/22/2017


---
# <a name="create-a-virtual-network-peering-using-azure-powershell"></a>Erstellen eines VNET-Peerings mit Azure PowerShell
[!INCLUDE [virtual-networks-create-vnet-selectors-arm-include](../../includes/virtual-networks-create-vnetpeering-selectors-arm-include.md)]

[!INCLUDE [virtual-networks-create-vnet-intro](../../includes/virtual-networks-create-vnetpeering-intro-include.md)]

[!INCLUDE [virtual-networks-create-vnet-scenario-basic-include](../../includes/virtual-networks-create-vnetpeering-scenario-basic-include.md)]

Führen Sie die folgenden Schritte aus, um ein VNet-Peering mit PowerShell zu erstellen:

1. Wenn Sie Azure PowerShell zuvor noch nicht verwendet haben, lesen Sie [Installieren und Konfigurieren von Azure PowerShell](/powershell/azureps-cmdlets-docs) , und befolgen Sie die komplette Anleitung, um sich bei Azure anzumelden und Ihr Abonnement auszuwählen.

    > [!NOTE]
    > Das PowerShell-Cmdlet zum Verwalten des VNet-Peerings ist in [Azure PowerShell 1.6](http://www.powershellgallery.com/packages/Azure/1.6.0)
    >

2. Lesen virtueller Netzwerkobjekte:

    ```powershell
    $vnet1 = Get-AzureRmVirtualNetwork -ResourceGroupName vnet101 -Name vnet1
    $vnet2 = Get-AzureRmVirtualNetwork -ResourceGroupName vnet101 -Name vnet2
    ```

3. Zum Einrichten des VNet-Peerings müssen zwei Links (je einer pro Richtung) erstellt werden. Mit dem folgenden Schritt wird zuerst ein VNet-Peeringlink für „VNet1 zu VNet2“ erstellt:

    ```powershell
    Add-AzureRmVirtualNetworkPeering -Name LinkToVNet2 -VirtualNetwork $vnet1 -RemoteVirtualNetworkId $vnet2.Id
    ```

    Zurückgegebene Ausgabe:
        
        Name            : LinkToVNet2
        Id: /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/vnet101/providers/Microsoft.Network/virtualNetworks/vnet1/virtualNetworkPeerings/LinkToVNet2
        Etag            : W/"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
        ResourceGroupName    : vnet101
        VirtualNetworkName    : vnet1
        PeeringState        : Initiated
        ProvisioningState    : Succeeded
        RemoteVirtualNetwork    : {
        "Id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/vnet101/providers/Microsoft.Network/virtualNetworks/vnet2"
                                        }
        AllowVirtualNetworkAccess    : True
        AllowForwardedTraffic    : False
        AllowGatewayTransit    : False
            UseRemoteGateways    : False
            RemoteGateways        : null
            RemoteVirtualNetworkAddressSpace : null

4. Mit dem folgenden Schritt wird ein VNet-Peeringlink für „VNet2 zu VNet1“ erstellt:

    ```powershell
    Add-AzureRmVirtualNetworkPeering -Name LinkToVNet1 -VirtualNetwork $vnet2 -RemoteVirtualNetworkId $vnet1.Id
    ```

    Zurückgegebene Ausgabe:

        Name            : LinkToVNet1
        Id                : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/vnet101/providers/Microsoft.Network/virtualNetworks/vnet2/virtualNetworkPeerings/LinkToVNet1
        Etag            : W/"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
        ResourceGroupName    : vnet101
        VirtualNetworkName    : vnet2
        PeeringState        : Connected
        ProvisioningState    : Succeeded
        RemoteVirtualNetwork    : {
                                            "Id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/vnet101/providers/Microsoft.Network/virtualNetworks/vnet1"
                                        }
        AllowVirtualNetworkAccess    : True
        AllowForwardedTraffic    : False
        AllowGatewayTransit    : False
        UseRemoteGateways    : False
        RemoteGateways        : null
        RemoteVirtualNetworkAddressSpace : null
5. Nach dem Erstellen des VNET-Peering-Links geben Sie den folgenden Befehl ein, um den Linkstatus anzuzeigen:

    ```powershell
    Get-AzureRmVirtualNetworkPeering -VirtualNetworkName vnet1 -ResourceGroupName vnet101 -Name linktovnet2
    ```

    Zurückgegebene Ausgabe:
   
        Name            : LinkToVNet2
        Id                : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/vnet101/providers/Microsoft.Network/virtualNetworks/vnet1/virtualNetworkPeerings/LinkToVNet2
        Etag            : W/"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
        ResourceGroupName    : vnet101
        VirtualNetworkName    : vnet1
        PeeringState        : Connected
        ProvisioningState    : Succeeded
        RemoteVirtualNetwork    : {
                                             "Id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/vnet101/providers/Microsoft.Network/virtualNetworks/vnet2"
                                        }
        AllowVirtualNetworkAccess    : True
        AllowForwardedTraffic            : False
        AllowGatewayTransit              : False
        UseRemoteGateways                : False
        RemoteGateways                   : null
        RemoteVirtualNetworkAddressSpace : null
   
    Für das VNet-Peering können einige Eigenschaften konfiguriert werden:
   
   | Option | Beschreibung | Standard |
   |:--- |:--- |:--- |
   | AllowVirtualNetworkAccess |Gibt an, ob der Adressraum des per Peering verknüpften VNet als Teil des Virtual_network-Tags einbezogen wird. |Ja |
   | AllowForwardedTraffic |Gibt an, ob Datenverkehr, der nicht aus dem mittels Peering verknüpften VNET stammt, angenommen oder verworfen wird. |Nein |
   | AllowGatewayTransit |Ermöglicht dem per Peering verknüpften VNet die Verwendung des VNet-Gateways. |Nein |
   | UseRemoteGateways |Gibt an, dass das VNet-Gateway des per Peering verknüpften VNet verwendet wird. Für das verknüpfte VNet muss ein Gateway konfiguriert sein, und „AllowGatewayTransit“ muss ausgewählt sein. Sie können diese Option nicht verwenden, wenn Sie ein Gateway konfiguriert haben. |Nein |

    Jeder Link in einem VNET-Peering weist die vorherigen Eigenschaften auf. So können Sie beispielsweise die Eigenschaft `AllowVirtualNetworkAccess` beim VNET-Peeringlink für „VNet1 zu VNet2“ auf *TRUE* und beim VNET-Peeringlink für die Gegenrichtung auf *FALSE* festlegen.

    ```powershell
    $LinktoVNet2 = Get-AzureRmVirtualNetworkPeering -VirtualNetworkName vnet1 `
    -ResourceGroupName vnet101 -Name LinkToVNet2

    $LinktoVNet2.AllowForwardedTraffic = $true
    Set-AzureRmVirtualNetworkPeering -VirtualNetworkPeering $LinktoVNet2
    ```

    Sie können `Get-AzureRmVirtualNetworkPeering` ausführen, um den Wert der Eigenschaft nach der Änderung zu überprüfen. In der Ausgabe sehen Sie, dass `AllowForwardedTraffic` Änderungen nach dem Ausführen der vorherigen Cmdlets auf *TRUE* festgelegt sind.

        Name            : LinkToVNet2
        Id            : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/vnet101/providers/Microsoft.Network/virtualNetworks/vnet1/virtualNetworkPeerings/LinkToVNet2
        Etag            : W/"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
        ResourceGroupName    : vnet101
        VirtualNetworkName    : vnet1
        PeeringState        : Connected
        ProvisioningState    : Succeeded
        RemoteVirtualNetwork    : {
                                            "Id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/vnet101/providers/Microsoft.Network/virtualNetworks/vnet2"
                                        }
        AllowVirtualNetworkAccess    : True
        AllowForwardedTraffic        : True
        AllowGatewayTransit        : False
        UseRemoteGateways        : False
        RemoteGateways        : null
        RemoteVirtualNetworkAddressSpace : null

    Nach dem Einrichten des Peerings müssen virtuelle Computer über beide VNETs miteinander kommunizieren können. Standardmäßig ist `AllowVirtualNetworkAccess` auf *TRUE* festgelegt, und das VNET-Peering stellt die passenden ACLs bereit, um die Kommunikation zwischen VNETs zu ermöglichen. Sie können aber trotzdem Netzwerksicherheitsgruppen-Regeln (NSG-Regeln) anwenden, um die Konnektivität zwischen bestimmten Subnetzen oder virtuellen Computern zu blockieren und so eine Feinsteuerung des Zugriffs zwischen den beiden VNETs zu erreichen. Weitere Informationen finden Sie im Artikel zu [Netzwerksicherheitsgruppen](virtual-networks-create-nsg-arm-ps.md).

[!INCLUDE [virtual-networks-create-vnet-scenario-crosssub-include](../../includes/virtual-networks-create-vnetpeering-scenario-crosssub-include.md)]

Führen Sie die folgenden Schritte aus, um mithilfe von PowerShell ein abonnementübergreifendes VNET-Peering zu erstellen:

1. Melden Sie sich bei Azure unter dem Abonnement A mit einem privilegierten Benutzerkonto für Benutzer A an, und führen Sie das folgende Cmdlet aus:

    ```powershell
    New-AzureRmRoleAssignment -SignInName <UserB ID> -RoleDefinitionName "Network Contributor" -Scope /subscriptions/<Subscription-A-ID>/resourceGroups/<ResourceGroupName>/providers/Microsoft.Network/VirtualNetworks/VNet5
    ```

    Dies ist nicht zwingend erforderlich. Das Peering kann auch dann eingerichtet werden, wenn Benutzer individuell Peeringanforderungen für ihre jeweiligen VNETs auslösen, solange die Anforderungen übereinstimmen. Das Hinzufügen eines privilegierten Benutzers des anderen VNets als Benutzer im lokalen VNet vereinfacht aber die Einrichtung.
2. Melden Sie sich bei Azure unter dem Abonnement B mit einem privilegierten Benutzerkonto für Benutzer B an, und führen Sie das folgende Cmdlet aus:

    ```powershell
    New-AzureRmRoleAssignment -SignInName <UserA ID> -RoleDefinitionName "Network Contributor" -Scope /subscriptions/<Subscription-B-ID>/resourceGroups/<ResourceGroupName>/providers/Microsoft.Network/VirtualNetworks/VNet3
    ```

    > [!IMPORTANT]
    > Wenn das Peering, das Sie gerade erstellen, zwischen zwei VNETs liegt, die über das Azure Resource Manager-Bereitstellungsmodell erstellt wurden, fahren Sie mit den übrigen Schritten in diesem Abschnitt fort. Wenn die zwei VNETs über verschiedene Bereitstellungsmodelle erstellt wurden, überspringen Sie die restlichen Schritte in diesem Abschnitt, und führen Sie die Schritte im Abschnitt [Peering virtual networks created through different deployment models](#x-model) (Peering virtueller Netzwerke, die über verschiedene Bereitstellungsmodelle erstellt wurden) dieses Artikels aus.

3. Führen Sie in der Anmeldesitzung von Benutzer B den folgenden Befehl aus:

    ```powershell
    $vnet3 = Get-AzureRmVirtualNetwork -ResourceGroupName hr-vnets -Name vnet3

    Add-AzureRmVirtualNetworkPeering -Name LinkToVNet5 -VirtualNetwork $vnet3 -RemoteVirtualNetworkId "/subscriptions/<Subscription-B-Id>/resourceGroups/<ResourceGroupName>/providers/Microsoft.Network/virtualNetworks/VNet5" -BlockVirtualNetworkAccess
    ```

4. Führen Sie in der Anmeldesitzung von Benutzer B das folgende Cmdlet aus:

    ```powershell
    $vnet5 = Get-AzureRmVirtualNetwork -ResourceGroupName vendor-vnets -Name vnet5

    Add-AzureRmVirtualNetworkPeering -Name LinkToVNet3 -VirtualNetwork $vnet5 -RemoteVirtualNetworkId "/subscriptions/<Subscriptoin-A-Id>/resourceGroups/<ResourceGroupName>/providers/Microsoft.Network/virtualNetworks/VNet3" -BlockVirtualNetworkAccess
    ```

5. Nach dem Einrichten des Peerings sollten alle virtuellen Computer in VNet3 mit allen virtuellen Computern in VNet5 kommunizieren können.

[!INCLUDE [virtual-networks-create-vnet-scenario-transit-include](../../includes/virtual-networks-create-vnetpeering-scenario-transit-include.md)]

1. Führen Sie in diesem Szenario die folgenden PowerShell-Cmdlets aus, um das VNET-Peering einzurichten. Sie müssen die `AllowForwardedTraffic`-Eigenschaft auf *TRUE* festlegen und VNET1 mit HubVNet verknüpfen, um eingehenden Datenverkehr von außerhalb des Adressraums des VNET-Peerings zuzulassen.

    ```powershell
    $hubVNet = Get-AzureRmVirtualNetwork -ResourceGroupName vnet101 -Name HubVNet
    $vnet1 = Get-AzureRmVirtualNetwork -ResourceGroupName vnet101 -Name vnet1

    Add-AzureRmVirtualNetworkPeering -Name LinkToHub -VirtualNetwork $vnet1 -RemoteVirtualNetworkId $HubVNet.Id -AllowForwardedTraffic

    Add-AzureRmVirtualNetworkPeering -Name LinkToVNet1 -VirtualNetwork $HubVNet -RemoteVirtualNetworkId $vnet1.Id
    ```

2. Nachdem das Peering eingerichtet wurde, können Sie diesen [Artikel](virtual-network-create-udr-arm-ps.md) lesen und eine benutzerdefinierte Route (User-Defined Route, UDR) festlegen, um VNet1-Datenverkehr über ein virtuelles Gerät zu leiten und dessen Funktionen zu nutzen. Wenn Sie die Adresse des nächsten Hops der Route angeben, können Sie diese auf die IP-Adresse des virtuellen Geräts im mittels Peering verknüpften VNet (HubVNet) festlegen. Beispiel:

    ```powershell
    $route = New-AzureRmRouteConfig -Name TestNVA -AddressPrefix 10.3.0.0/16 -NextHopType VirtualAppliance -NextHopIpAddress 192.0.1.5

    $routeTable = New-AzureRmRouteTable -ResourceGroupName VNet101 -Location brazilsouth -Name TestRT -Route $route

    $vnet1 = Get-AzureRmVirtualNetwork -ResourceGroupName VNet101 -Name VNet1

    Set-AzureRmVirtualNetworkSubnetConfig -VirtualNetwork $vnet1 -Name subnet-1 -AddressPrefix 10.1.1.0/24 -RouteTable $routeTable

    Set-AzureRmVirtualNetwork -VirtualNetwork $vnet1
    ```

[!INCLUDE [virtual-networks-create-vnet-scenario-asmtoarm-include](../../includes/virtual-networks-create-vnetpeering-scenario-asmtoarm-include.md)]

1. Wenn Sie ein Peering zwischen VNETs erstellen, die über verschiedene Bereitstellungsmodelle im *gleichen* Abonnement bereitgestellt werden, fahren Sie mit Schritt 2 fort. Die Funktion zum Erstellen eines VNET-Peerings zwischen VNETs, die über verschiedene Bereitstellungsmodelle in *unterschiedlichen* Abonnements bereitgestellt werden, ist als **Vorschau** verfügbar. Funktionen in der Vorschau bieten nicht dieselbe Zuverlässigkeit und Vereinbarung zum Servicelevel wie die in der finalen Version veröffentlichten Funktionen. Beim Erstellen eines Peerings zwischen VNETs, die über verschiedene Bereitstellungsmodelle in verschiedenen Abonnements bereitgestellt sind, müssen Sie zuerst die folgenden Aufgaben ausführen:
    - Registrieren Sie die Vorschaufunktion im Azure-Abonnement durch Eingabe der folgenden Befehle aus PowerShell: `Register-AzureRmProviderFeature -FeatureName AllowClassicCrossSubscriptionPeering -ProviderNamespace Microsoft.Network` und `Register-AzureRmResourceProvider -ProviderNamespace Microsoft.Network`.
    - Führen Sie die Schritte 1 bis 2 im Abschnitt [Abonnementübergreifendes Peering](#x-sub) dieses Artikels aus.
2. Lesen Sie das virtuelle Netzwerkobjekt für **VNET1**, das virtuelle Netzwerk von Azure Resource Manager, indem Sie folgenden Befehl eingeben:

    ```powershell
    $vnet1 = Get-AzureRmVirtualNetwork -ResourceGroupName vnet101 -Name vnet1
    ```

3. Zum Einrichten des VNET-Peerings in diesem Szenario ist nur ein Link erforderlich, und zwar zwischen **VNET1** und **VNET2**. Für diesen Schritt benötigen Sie die Ressourcen-ID des klassischen VNet. Das Format der Ressourcengruppen-ID sieht wie im folgenden Beispiel aus:

           subscriptions/{SubscriptionID}/resourceGroups/{ResourceGroupName}/providers/Microsoft.ClassicNetwork/virtualNetworks/{VirtualNetworkName}

    Achten Sie darauf, SubscriptionID, ResourceGroupName und VirtualNetworkName durch die entsprechenden Namen zu ersetzen.

    Dies kann durch Eingabe des folgenden Befehls erreicht werden:

    ```powershell
    Add-AzureRmVirtualNetworkPeering -Name LinkToVNet2 -VirtualNetwork $vnet1 -RemoteVirtualNetworkId /subscriptions/xxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxx/resourceGroups/MyResourceGroup/providers/Microsoft.ClassicNetwork/virtualNetworks/VNET2
    ```

4. Nach der Erstellung des VNET-Peeringlinks wird der Linkzustand wie in der folgenden Ausgabe angezeigt:
   
        Name                             : LinkToVNet2
        Id                               : /subscriptions/xxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxx/resourceGroups/MyResourceGroup/providers/Microsoft.Network/virtualNetworks/VNET1/virtualNetworkPeerings/LinkToVNet2
        Etag                             : W/"acecbd0f-766c-46be-aa7e-d03e41c46b16"
        ResourceGroupName                : MyResourceGroup
        VirtualNetworkName               : VNET1
        PeeringState                     : Connected
        ProvisioningState                : Succeeded
        RemoteVirtualNetwork             : {
                                         "Id": "/subscriptions/xxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxx/resourceGroups/MyResourceGroup/providers/Microsoft.ClassicNetwork/virtualNetworks/VNET2"
                                       }
        AllowVirtualNetworkAccess        : True
        AllowForwardedTraffic            : False
        AllowGatewayTransit              : False
        UseRemoteGateways                : False
        RemoteGateways                   : null
        RemoteVirtualNetworkAddressSpace : null

## <a name="remove-a-vnet-peering"></a>Entfernen eines VNET-Peerings
1. Führen Sie das folgende Cmdlet aus, um ein VNET-Peering zu entfernen:

    ```powershell
    Remove-AzureRmVirtualNetworkPeering
    ```

    Entfernen Sie durch Eingabe der folgenden Befehle beide Links:

    ```powershell
    Remove-AzureRmVirtualNetworkPeering -ResourceGroupName vnet101 -VirtualNetworkName vnet1 -Name linktovnet2
    Remove-AzureRmVirtualNetworkPeering -ResourceGroupName vnet101 -VirtualNetworkName vnet1 -Name linktovnet2
    ```

2. Wenn Sie in einem VNET-Peering einen Link entfernen, wird als Zustand für den Peeringlink *Verbindung getrennt* angezeigt. Bei diesem Status können Sie den Link erst dann wieder neu erstellen, wenn sich der Status des Peeringlinks in *Initiiert* ändert. Es empfiehlt sich, vor dem erneuten Erstellen des VNet-Peerings beide Links zu entfernen.


