<properties
   pageTitle="Erstellen des VNet-Peerings mit PowerShell-Cmdlets | Microsoft Azure"
   description="Erfahren Sie, wie Sie ein virtuelles Netzwerk mit dem Azure-Portal im Resource Manager erstellen."
   services="virtual-network"
   documentationCenter=""
   authors="NarayanAnnamalai"
   manager="jefco"
   editor=""
   tags="azure-resource-manager"/>

<tags
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="09/14/2016"
   ms.author="narayanannamalai; annahar"/>

# Erstellen des VNet-Peerings mit PowerShell-Cmdlets

[AZURE.INCLUDE [virtual-networks-create-vnet-selectors-arm-include](../../includes/virtual-networks-create-vnetpeering-selectors-arm-include.md)]

[AZURE.INCLUDE [virtual-networks-create-vnet-intro](../../includes/virtual-networks-create-vnetpeering-intro-include.md)]

[AZURE.INCLUDE [virtual-networks-create-vnet-scenario-basic-include](../../includes/virtual-networks-create-vnetpeering-scenario-basic-include.md)]

Führen Sie die folgenden Schritte aus, um ein VNet-Peering mit PowerShell zu erstellen:

1. Wenn Sie Azure PowerShell zuvor noch nicht verwendet haben, lesen Sie [Installieren und Konfigurieren von Azure PowerShell](../powershell-install-configure.md), und befolgen Sie die komplette Anleitung, um sich bei Azure anzumelden und Ihr Abonnement auszuwählen.

        > [AZURE.NOTE] PowerShell cmdlet for managing VNet peering is shipped with [Azure PowerShell 1.6.](http://www.powershellgallery.com/packages/Azure/1.6.0)

2. Lesen virtueller Netzwerkobjekte:

        $vnet1 = Get-AzureRmVirtualNetwork -ResourceGroupName vnet101 -Name vnet1
        $vnet2 = Get-AzureRmVirtualNetwork -ResourceGroupName vnet101 -Name vnet2

3. Zum Einrichten des VNet-Peerings müssen zwei Links (je einer pro Richtung) erstellt werden. Mit dem folgenden Schritt wird zuerst ein VNet-Peeringlink für „VNet1 zu VNet2“ erstellt:

        Add-AzureRmVirtualNetworkPeering -name LinkToVNet2 -VirtualNetwork $vnet1 -RemoteVirtualNetworkId $vnet2.id

    Ausgabe:

        Name			: LinkToVNet2
        Id: /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/vnet101/providers/Microsoft.Network/virtualNetworks/vnet1/virtualNetworkPeerings/LinkToVNet2
        Etag			: W/"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
        ResourceGroupName	: vnet101
        VirtualNetworkName	: vnet1
        ProvisioningState		: Succeeded
        RemoteVirtualNetwork	: {
                                            "Id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/vnet101/providers/Microsoft.Network/virtualNetworks/vnet2"
                                        }
        AllowVirtualNetworkAccess	: True
        AllowForwardedTraffic	: False
        AllowGatewayTransit	: False
        UseRemoteGateways	: False
        RemoteGateways		: null
        RemoteVirtualNetworkAddressSpace : null

4. Mit dem folgenden Schritt wird ein VNet-Peeringlink für „VNet2 zu VNet1“ erstellt:

        Add-AzureRmVirtualNetworkPeering -name LinkToVNet1 -VirtualNetwork $vnet2 -RemoteVirtualNetworkId $vnet1.id

    Ausgabe:

        Name			: LinkToVNet1
        Id				: /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/vnet101/providers/Microsoft.Network/virtualNetworks/vnet2/virtualNetworkPeerings/LinkToVNet1
        Etag			: W/"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
        ResourceGroupName	: vnet101
        VirtualNetworkName	: vnet2
        ProvisioningState		: Succeeded
        RemoteVirtualNetwork	: {
                                            "Id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/vnet101/providers/Microsoft.Network/virtualNetworks/vnet1"
                                        }
        AllowVirtualNetworkAccess	: True
        AllowForwardedTraffic	: False
        AllowGatewayTransit	: False
        UseRemoteGateways	: False
        RemoteGateways		: null
        RemoteVirtualNetworkAddressSpace : null

5. Nach Erstellung des VNet-Peeringlinks wird der folgende Linkzustand angezeigt:

        Get-AzureRmVirtualNetworkPeering -VirtualNetworkName vnet1 -ResourceGroupName vnet101 -Name linktovnet2

    Ausgabe:

		Name			: LinkToVNet2
		Id				: /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/vnet101/providers/Microsoft.Network/virtualNetworks/vnet1/virtualNetworkPeerings/LinkToVNet2
		Etag			: W/"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
		ResourceGroupName	: vnet101
		VirtualNetworkName	: vnet1
		ProvisioningState		: Succeeded
		RemoteVirtualNetwork	: {
		                                     "Id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/vnet101/providers/Microsoft.Network/virtualNetworks/vnet2"
		                                }
		AllowVirtualNetworkAccess	: True
		AllowForwardedTraffic            : False
		AllowGatewayTransit              : False
		UseRemoteGateways                : False
		RemoteGateways                   : null
		RemoteVirtualNetworkAddressSpace : null

	Für das VNet-Peering können einige Eigenschaften konfiguriert werden:

	|Option|Beschreibung|Standard|
	|:-----|:----------|:------|
	|AllowVirtualNetworkAccess|Gibt an, ob der Adressraum des per Peering verknüpften VNet als Teil des Virtual\_network-Tags einbezogen wird.|Ja|
	|AllowForwardedTraffic|Ermöglicht, dass Datenverkehr, der nicht aus dem per Peering verknüpften VNet stammt, akzeptiert oder verworfen wird.|Nein|
	|AllowGatewayTransit|Ermöglicht dem per Peering verknüpften VNet die Verwendung des VNet-Gateways.|Nein|
	|UseRemoteGateways|Gibt an, dass das VNet-Gateway des per Peering verknüpften VNet verwendet wird. Für das verknüpfte VNet muss ein Gateway konfiguriert sein, und „AllowGatewayTransit“ muss ausgewählt sein. Sie können diese Option nicht verwenden, wenn Sie ein Gateway konfiguriert haben.|Nein|

	Jeder Link in einem VNet-Peering verfügt über die oben angegebenen Eigenschaften. So können Sie beispielsweise die Eigenschaft „AllowVirtualNetworkAccess“ beim VNet-Peeringlink für „VNet1 zu VNet2“ auf „true“ und beim VNet-Peeringlink für die Gegenrichtung auf „false“ festlegen.

        $LinktoVNet2 = Get-AzureRmVirtualNetworkPeering -VirtualNetworkName vnet1 -ResourceGroupName vnet101 -Name LinkToVNet2
        $LinktoVNet2.AllowForwardedTraffic = $true
        Set-AzureRmVirtualNetworkPeering -VirtualNetworkPeering $LinktoVNet2

    Sie können „Get-AzureRmVirtualNetworkPeering“ ausführen, um den Eigenschaftswert nach der Änderung zu überprüfen. In der Ausgabe sehen Sie, dass sich „AllowForwardedTraffic“ nach dem Ausführen der obigen Cmdlets in „True“ ändert.

        Name			: LinkToVNet2
        Id			: /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/vnet101/providers/Microsoft.Network/virtualNetworks/vnet1/virtualNetworkPeerings/LinkToVNet2
        Etag			: W/"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
        ResourceGroupName	: vnet101
        VirtualNetworkName	: vnet1
        ProvisioningState	: Succeeded
        RemoteVirtualNetwork	: {
                                            "Id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/vnet101/providers/Microsoft.Network/virtualNetworks/vnet2"
                                        }
        AllowVirtualNetworkAccess	: True
        AllowForwardedTraffic		: True
        AllowGatewayTransit		: False
        UseRemoteGateways		: False
        RemoteGateways		: null
        RemoteVirtualNetworkAddressSpace : null

	Nachdem das Peering in diesem Szenario eingerichtet wurde, können Sie die Verbindungen von jedem virtuellen Computer mit jedem virtuellen Computer in beiden VNets initiieren. Standardmäßig ist „AllowVirtualNetworkAccess“ auf „true“ festgelegt, und das VNet-Peering stellt die passenden ACLs bereit, um die Kommunikation zwischen VNets zu ermöglichen. Sie können aber trotzdem Netzwerksicherheitsgruppen-Regeln (NSG-Regeln) anwenden, um die Konnektivität zwischen bestimmten Subnetzen oder virtuellen Computern zu blockieren und so eine Feinsteuerung des Zugriffs zwischen den beiden virtuellen Netzwerken zu erreichen. Weitere Informationen zum Erstellen von NSG-Regeln finden Sie in [diesem Artikel](virtual-networks-create-nsg-arm-ps.md).

[AZURE.INCLUDE [virtual-networks-create-vnet-scenario-crosssub-include](../../includes/virtual-networks-create-vnetpeering-scenario-crosssub-include.md)]

Führen Sie die folgenden Schritte aus, um mithilfe von PowerShell ein abonnementübergreifendes VNet-Peering zu erstellen:

1. Melden Sie sich bei Azure unter dem Abonnement A mit einem privilegierten Benutzerkonto für Benutzer A an, und führen Sie das folgende Cmdlet aus:

        New-AzureRmRoleAssignment -SignInName <UserB ID> -RoleDefinitionName "Network Contributor" -Scope /subscriptions/<Subscription-A-ID>/resourceGroups/<ResourceGroupName>/providers/Microsoft.Network/VirtualNetworks/VNet5

    Dies ist nicht zwingend erforderlich. Das Peering kann auch dann eingerichtet werden, wenn Benutzer individuell Peeringanforderungen für ihre jeweiligen VNets auslösen, solange die Anforderungen übereinstimmen. Das Hinzufügen eines privilegierten Benutzers des anderen VNets als Benutzer im lokalen VNet vereinfacht aber die Einrichtung.

2. Melden Sie sich bei Azure unter dem Abonnement B mit einem privilegierten Benutzerkonto für Benutzer B an, und führen Sie das folgende Cmdlet aus:

        New-AzureRmRoleAssignment -SignInName <UserA ID> -RoleDefinitionName "Network Contributor" -Scope /subscriptions/<Subscription-B-ID>/resourceGroups/<ResourceGroupName>/providers/Microsoft.Network/VirtualNetworks/VNet3

3. Führen Sie in der Anmeldesitzung von Benutzer A das folgende Cmdlet aus:

        $vnet3 = Get-AzureRmVirtualNetwork -ResourceGroupName hr-vnets -Name vnet3

        Add-AzureRmVirtualNetworkPeering -name LinkToVNet5 -VirtualNetwork $vnet3 -RemoteVirtualNetworkId "/subscriptions/<Subscriptoin-B-Id>/resourceGroups/<ResourceGroupName>/providers/Microsoft.Network/virtualNetworks/VNet5" -BlockVirtualNetworkAccess

4. Führen Sie in der Anmeldesitzung von Benutzer B das folgende Cmdlet aus:

        $vnet5 = Get-AzureRmVirtualNetwork -ResourceGroupName vendor-vnets -Name vnet5

        Add-AzureRmVirtualNetworkPeering -name LinkToVNet3 -VirtualNetwork $vnet5 -RemoteVirtualNetworkId "/subscriptions/<Subscriptoin-A-Id>/resourceGroups/<ResourceGroupName>/providers/Microsoft.Network/virtualNetworks/VNet3" -BlockVirtualNetworkAccess

5. Nach dem Einrichten des Peerings sollte nun jeder virtuelle Computer in VNet3 mit jedem virtuellen Computer in VNet5 kommunizieren können.

[AZURE.INCLUDE [virtual-networks-create-vnet-scenario-transit-include](../../includes/virtual-networks-create-vnetpeering-scenario-transit-include.md)]

1. In diesem Szenario können Sie die weiter unten angegebenen PowerShell-Cmdlets ausführen, um das VNet-Peering einzurichten. Sie müssen die AllowForwardedTraffic-Eigenschaft auf „true“ festlegen und VNet1 mit HubVNet verknüpfen, um eingehenden Datenverkehr von außerhalb des Adressraums des VNet-Peerings zuzulassen.

        $hubVNet = Get-AzureRmVirtualNetwork -ResourceGroupName vnet101 -Name HubVNet
        $vnet1 = Get-AzureRmVirtualNetwork -ResourceGroupName vnet101 -Name vnet1

        Add-AzureRmVirtualNetworkPeering -name LinkToHub -VirtualNetwork $vnet1 -RemoteVirtualNetworkId $HubVNet.id -AllowForwardedTraffic

        Add-AzureRmVirtualNetworkPeering -name LinkToVNet1 -VirtualNetwork $HubVNet -RemoteVirtualNetworkId $vnet1.id

2. Nachdem das Peering eingerichtet wurde, können Sie diesen [Artikel](virtual-network-create-udr-arm-ps.md) lesen und eine benutzerdefinierte Route (User-Defined Route, UDR) festlegen, um VNet1-Datenverkehr über ein virtuelles Gerät zu leiten und dessen Funktionen zu nutzen. Wenn Sie die Adresse des nächsten Hops der Route angeben, können Sie diese auf die IP-Adresse des virtuellen Geräts im mittels Peering verknüpften VNet (HubVNet) festlegen. Unten ist ein Beispiel angegeben:

        $route = New-AzureRmRouteConfig -Name TestNVA -AddressPrefix 10.3.0.0/16 -NextHopType VirtualAppliance -NextHopIpAddress 192.0.1.5

        $routeTable = New-AzureRmRouteTable -ResourceGroupName VNet101 -Location brazilsouth -Name TestRT -Route $route

        $vnet1 = Get-AzureRmVirtualNetwork -ResourceGroupName VNet101 -Name VNet1

        Set-AzureRmVirtualNetworkSubnetConfig -VirtualNetwork $vnet1 -Name subnet-1 -AddressPrefix 10.1.1.0/24 -RouteTable $routeTable

        Set-AzureRmVirtualNetwork -VirtualNetwork $vnet1

[AZURE.INCLUDE [virtual-networks-create-vnet-scenario-asmtoarm-include](../../includes/virtual-networks-create-vnetpeering-scenario-asmtoarm-include.md)]

Führen Sie die folgenden Schritte aus, um ein VNet-Peering zwischen einem klassischen virtuellen Netzwerk und einem virtuellen Azure Resource Manager-Netzwerk in PowerShell zu erstellen:

1. Lesen Sie das Objekt für das virtuelle Netzwerk für **VNET1** und das virtuelle Azure Resource Manager-Netzwerk wie folgt: $vnet1 = Get-AzureRmVirtualNetwork -ResourceGroupName vnet101 -Name vnet1

2. Zum Einrichten des VNet-Peerings in diesem Szenario ist nur ein Link erforderlich, und zwar ein Link zwischen **VNET1** und **VNET2**. Für diesen Schritt benötigen Sie die Ressourcen-ID des klassischen VNet. Die Ressourcengruppen-ID hat das folgende Format: /subscriptions/SubscriptionID/resourceGroups/ResourceGroupName/providers/Microsoft.ClassicNetwork/virtualNetworks/VirtualNetworkName

    Achten Sie darauf, SubscriptionID, ResourceGroupName und VirtualNetworkName durch die entsprechenden Namen zu ersetzen.

    Dies erreichen Sie wie folgt:

        Add-AzureRmVirtualNetworkPeering -name LinkToVNet2 -VirtualNetwork $vnet1 -RemoteVirtualNetworkId /subscriptions/xxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxx/resourceGroups/MyResourceGroup/providers/Microsoft.ClassicNetwork/virtualNetworks/VNET2

3. Nach der Erstellung des VNet-Peeringlinks wird der Linkzustand wie in der folgenden Ausgabe angezeigt:

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

## Entfernen des VNet-Peerings

1.	Führen Sie das folgende Cmdlet aus, um das VNet-Peering zu entfernen:

        Remove-AzureRmVirtualNetworkPeering  

        remove both links, as shown below:

        Remove-AzureRmVirtualNetworkPeering -ResourceGroupName vnet101 -VirtualNetworkName vnet1 -Name linktovnet2
        Remove-AzureRmVirtualNetworkPeering -ResourceGroupName vnet101 -VirtualNetworkName vnet1 -Name linktovnet2

2. Wenn Sie einen Link in einem VNet-Peering entfernen, wird als Zustand für den Peeringlink „Verbindung getrennt“ angezeigt. Bei diesem Status können Sie den Link erst dann wieder neu erstellen, wenn sich der Status des Peeringlinks in „Initiiert“ ändert. Es empfiehlt sich, vor dem erneuten Erstellen des VNet-Peerings beide Links zu entfernen.

<!---HONumber=AcomDC_0928_2016-->