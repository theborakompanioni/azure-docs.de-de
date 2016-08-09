<properties
   pageTitle="Erstellen des VNet-Peerings mit PowerShell-Cmdlets | Microsoft Azure"
   description="Erfahren Sie, wie Sie ein virtuelles Netzwerk mit dem Azure-Portal im Resource Manager erstellen."
   services="virtual-network"
   documentationCenter=""
   authors="narayanannamalai"
   manager="jefco"
   editor=""
   tags="azure-resource-manager"/>

<tags
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="03/15/2016"
   ms.author="telmos"/>

# Erstellen des VNet-Peerings mit PowerShell-Cmdlets

[AZURE.INCLUDE [virtual-networks-create-vnet-selectors-arm-include](../../includes/virtual-networks-create-vnetpeering-selectors-arm-include.md)]

[AZURE.INCLUDE [virtual-networks-create-vnet-intro](../../includes/virtual-networks-create-vnetpeering-intro-include.md)]

[AZURE.INCLUDE [virtual-networks-create-vnet-scenario-basic-include](../../includes/virtual-networks-create-vnetpeering-scenario-basic-include.md)]

Führen Sie die folgenden Schritte aus, um ein VNet-Peering mit PowerShell zu erstellen:

1. Wenn Sie Azure PowerShell zuvor noch nicht verwendet haben, lesen Sie [Installieren und Konfigurieren von Azure PowerShell](../powershell-install-configure.md), und befolgen Sie die komplette Anleitung, um sich bei Azure anzumelden und Ihr Abonnement auszuwählen.

Hinweis: Das PowerShell-Cmdlet zum Verwalten des VNet-Peerings ist in [Azure PowerShell 1.6.](http://www.powershellgallery.com/packages/Azure/1.6.0) enthalten.

2. Lesen virtueller Netzwerkobjekte:

        $vnet1 = Get-AzureRmVirtualNetwork -ResourceGroupName vnet101 -Name vnet1
        $vnet2 = Get-AzureRmVirtualNetwork -ResourceGroupName vnet101 -Name vnet2

	
3. Zum Einrichten des VNet-Peerings müssen Sie zwei Links erstellen, also einen für jede Richtung. Bei diesem Schritt wird zuerst der VNet-Peeringlink für VNet1 zu VNet2 erstellt:

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

4. In diesem Schritt wird der VNet-Peeringlink für VNet2 zu VNet1 erstellt.

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

5. Gehen Sie nach der Erstellung dieses VNet-Peeringlinks wie unten angegeben vor. Der Linkstatus wird wie folgt angezeigt:

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

Jeder Link in einem VNet-Peering verfügt über die obigen Eigenschaften. Beispielsweise können Sie für den VNet-Peeringlink von VNet1 zu VNet2 die Eigenschaft „AllowVirtualNetworkAccess“ auf „True“ und für den VNet-Peeringlink der anderen Richtung auf „False“ festlegen.

        $LinktoVNet2 = Get-AzureRmVirtualNetworkPeering -VirtualNetworkName vnet1 -ResourceGroupName vnet101 -Name LinkToVNet2
        $LinktoVNet2.AllowForwardedTraffic = $true
        Set-AzureRmVirtualNetworkPeering -VirtualNetworkPeering $LinktoVNet2

Außerdem können Sie „Get-AzureRmVirtualNetworkPeering“ ausführen, um den Eigenschaftswert nach der Änderung zu überprüfen. In der Ausgabe sehen Sie, dass sich „AllowForwardedTraffic“ nach dem Ausführen der obigen Cmdlets in „True“ ändert.

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

Nachdem das Peering in diesem Szenario eingerichtet wurde, können Sie die Verbindungen von jedem virtuellen Computer mit jedem virtuellen Computer in beiden VNets initiieren. Standardmäßig ist „AllowVirtualNetworkAccess“ auf „True“ festgelegt, und das VNet-Peering stellt die richtigen ACLs bereit, um die Kommunikation zwischen VNets zu ermöglichen. Sie können die NSG-Regeln aber trotzdem anwenden, um die Konnektivität beispielsweise zwischen bestimmten Subnetzen oder virtuellen Computern zu blockieren und so eine Feinsteuerung des Zugriffs zwischen den beiden virtuellen Netzwerken zu erzielen. Weitere Informationen zum Erstellen von NSG-Regeln finden Sie in [diesem Artikel](virtual-networks-create-nsg-arm-ps.md).

[AZURE.INCLUDE [virtual-networks-create-vnet-scenario-crosssub-include](../../includes/virtual-networks-create-vnetpeering-scenario-crosssub-include.md)]

Führen Sie die folgenden Schritte aus, um das VNet-Peering abonnementübergreifend per PowerShell zu erstellen:

1. Melden Sie sich an Azure mit dem privilegierten UserA für Abonnement A an, und führen Sie das Cmdlet aus:

        New-AzureRmRoleAssignment -SignInName <UserB ID> -RoleDefinitionName "Network Contributor" -Scope /subscriptions/<Subscription-A-ID>/resourceGroups/<ResourceGroupName>/providers/Microsoft.Network/VirtualNetwork/VNet5

Dies ist nicht zwingend erforderlich. Das Peering kann auch dann eingerichtet werden, wenn Benutzer individuell Peeringanforderungen für ihre jeweiligen VNets auslösen, solange die Anforderungen übereinstimmen. Das Hinzufügen von privilegierten Benutzern des anderen VNet als Benutzer im lokalen VNet vereinfacht das Setup.

2. Melden Sie sich an Azure mit dem privilegierten UserB für Abonnement B an, und führen Sie das Cmdlet aus:

        New-AzureRmRoleAssignment -SignInName <UserA ID> -RoleDefinitionName "Network Contributor" -Scope /subscriptions/<Subscription-B-ID>/resourceGroups/<ResourceGroupName>/providers/Microsoft.Network/VirtualNetwork/VNet3

3. Führen Sie in der Anmeldesitzung von UserA dann das folgende Cmdlet aus:

        $vnet3 = Get-AzureRmVirtualNetwork -ResourceGroupName hr-vnets -Name vnet3

        Add-AzureRmVirtualNetworkPeering -name LinkToVNet5 -VirtualNetwork $vnet3 -RemoteVirtualNetworkId "/subscriptions/<Subscriptoin-B-Id>/resourceGroups/<ResourceGroupName>/providers/Microsoft.Network/virtualNetworks/VNet5" -BlockVirtualNetworkAccess


4. Führen Sie in der Anmeldesitzung von UserB anschließend das folgende Cmdlet aus:

        $vnet5 = Get-AzureRmVirtualNetwork -ResourceGroupName vendor-vnets -Name vnet5

        Add-AzureRmVirtualNetworkPeering -name LinkToVNet3 -VirtualNetwork $vnet5 -RemoteVirtualNetworkId "/subscriptions/<Subscriptoin-A-Id>/resourceGroups/<ResourceGroupName>/providers/Microsoft.Network/virtualNetworks/VNet3" -BlockVirtualNetworkAccess

5. Nach dem Einrichten des Peerings sollte jeder virtuelle Computer in VNet3 nun mit jedem virtuellen Computer in VNet5 kommunizieren können.

[AZURE.INCLUDE [virtual-networks-create-vnet-scenario-transit-include](../../includes/virtual-networks-create-vnetpeering-scenario-transit-include.md)]

1. In diesem Szenario können Sie die unten angegebenen PowerShell-Cmdlets ausführen, um das VNet-Peering einzurichten. Sie müssen vor allem die AllowForwardedTraffic-Eigenschaft auf „True“ festlegen und VNet1 mit HubVnet verknüpfen, damit der eingehende Datenverkehr von außerhalb des VNet-Adressraums des Peerings zugelassen wird.

        $hubVNet = Get-AzureRmVirtualNetwork -ResourceGroupName vnet101 -Name HubVNet
        $vnet1 = Get-AzureRmVirtualNetwork -ResourceGroupName vnet101 -Name vnet1

        Add-AzureRmVirtualNetworkPeering -name LinkToHub -VirtualNetwork $vnet1 -RemoteVirtualNetworkId $HubVNet.id -AllowForwardedTraffic

        Add-AzureRmVirtualNetworkPeering -name LinkToVNet1 -VirtualNetwork $HubVNet -RemoteVirtualNetworkId $vnet1.id

2. Nachdem das Peering eingerichtet wurde, können Sie diesen [Artikel](virtual-network-create-udr-arm-ps.md) lesen und eine benutzerdefinierte Route (User-Defined Route, UDR) festlegen, um VNet1-Datenverkehr über ein virtuelles Gerät zu leiten und dessen Funktionen zu nutzen. Wenn Sie die Adresse des nächsten Hops der Route angeben, können Sie diese auf die IP-Adresse des virtuellen Geräts im per Peering verknüpften VNet (HubVNet) festlegen. Unten ist ein Beispiel angegeben:

        $route = New-AzureRmRouteConfig -Name TestNVA -AddressPrefix 10.3.0.0/16 -NextHopType VirtualAppliance -NextHopIpAddress 192.0.1.5

        $routeTable = New-AzureRmRouteTable -ResourceGroupName VNet101 -Location brazilsouth -Name TestRT -Route $route

        $vnet1 = Get-AzureRmVirtualNetwork -ResourceGroupName VNet101 -Name VNet1

        Set-AzureRmVirtualNetworkSubnetConfig -VirtualNetwork $vnet1 -Name subnet-1 -AddressPrefix 10.1.1.0/24 -RouteTable $routeTable

        Set-AzureRmVirtualNetwork -VirtualNetwork $vnet1


## Entfernen des VNet-Peerings

1.	Sie müssen das Cmdlet ausführen, um das VNet-Peering zu entfernen.

        Remove-AzureRmVirtualNetworkPeering  
    
    Das Entfernen beider Links ist hier angegeben:

        Remove-AzureRmVirtualNetworkPeering -ResourceGroupName vnet101 -VirtualNetworkName vnet1 -Name linktovnet2 
        Remove-AzureRmVirtualNetworkPeering -ResourceGroupName vnet101 -VirtualNetworkName vnet1 -Name linktovnet2 

3. Wenn Sie einen Link im VNet-Peering entfernen, wird als Status für den Peeringlink „Verbindung getrennt“ angezeigt.
  
4. Bei diesem Status können Sie den Link erst dann wieder neu erstellen, wenn sich der Status des Peeringlinks in „Initiiert“ ändert. Es wird empfohlen, beide Links zu entfernen, bevor Sie das VNet-Peering neu erstellen.

<!---HONumber=AcomDC_0803_2016-->