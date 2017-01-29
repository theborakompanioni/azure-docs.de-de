---
title: "Erstellen eines virtuellen Netzwerks über PowerShell | Microsoft Docs"
description: "Erfahren Sie, wie Sie ein virtuelles Netzwerk über PowerShell erstellen | Ressourcen-Manager."
services: virtual-network
documentationcenter: 
author: jimdial
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: a31f4f12-54ee-4339-b968-1a8097ca77d3
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/03/2017
ms.author: jdial
translationtype: Human Translation
ms.sourcegitcommit: ba7a67b8ae57da165f45bd3552a3dfac5f4ef64b
ms.openlocfilehash: 33f3a1f490b51123054440b6098de1ae4aca941f


---
# <a name="create-a-virtual-network-using-powershell"></a>Erstellen eines virtuellen Netzwerks über PowerShell

[!INCLUDE [virtual-networks-create-vnet-intro](../../includes/virtual-networks-create-vnet-intro-include.md)]

Azure verfügt über zwei Bereitstellungsmodelle: Azure Resource Manager und klassisch. Microsoft empfiehlt, Ressourcen mit dem Resource Manager-Bereitstellungsmodell zu erstellen. Weitere Informationen zu den Unterschieden zwischen den beiden Modellen finden Sie im Artikel zum Thema [Understand Azure deployment models](../azure-resource-manager/resource-manager-deployment-model.md) (Grundlegendes zu Azure-Bereitstellungsmodellen).
 
In diesem Artikel wird beschrieben, wie Sie mit dem Resource Manager-Bereitstellungsmodell über PowerShell ein VNET erstellen. Sie können ein VNet per Resource Manager auch mit anderen Tools erstellen oder dafür das klassische Bereitstellungsmodell verwenden, indem Sie in der folgenden Liste eine andere Option wählen:

> [!div class="op_single_selector"]
- [Portal](virtual-networks-create-vnet-arm-pportal.md)
- [PowerShell](virtual-networks-create-vnet-arm-ps.md)
- [BEFEHLSZEILENSCHNITTSTELLE (CLI)](virtual-networks-create-vnet-arm-cli.md)
- [Vorlage](virtual-networks-create-vnet-arm-template-click.md)
- [Portal (klassisch)](virtual-networks-create-vnet-classic-pportal.md)
- [PowerShell (klassisch)](virtual-networks-create-vnet-classic-netcfg-ps.md)
- [CLI (klassisch)](virtual-networks-create-vnet-classic-cli.md)

[!INCLUDE [virtual-networks-create-vnet-scenario-include](../../includes/virtual-networks-create-vnet-scenario-include.md)]

## <a name="create-a-virtual-network"></a>Erstellen eines virtuellen Netzwerks

Um ein virtuelles Netzwerk mit PowerShell zu erstellen, müssen Sie die folgenden Schritte ausführen:

1. Installieren und konfigurieren Sie Azure PowerShell mithilfe der Anweisungen unter [Installieren und Konfigurieren von Azure PowerShell](/powershell/azureps-cmdlets-docs).

2. Erstellen Sie ggf. eine neue Ressourcengruppe, wie unten dargestellt. Erstellen Sie für dieses Szenario eine Ressourcengruppe mit dem Namen *TestRG*. Weitere Informationen zu Ressourcengruppen finden Sie unter [Übersicht über den Azure-Ressourcen-Manager](../azure-resource-manager/resource-group-overview.md).

    ```powershell   
    New-AzureRmResourceGroup -Name TestRG -Location centralus
    ```

    Erwartete Ausgabe:

        ResourceGroupName : TestRG
        Location          : centralus
        ProvisioningState : Succeeded
        Tags              :
        ResourceId        : /subscriptions/[Subscription Id]/resourceGroups/TestRG    
3. Erstellen Sie ein neues VNET namens *TestVNet*:

    ```powershell
    New-AzureRmVirtualNetwork -ResourceGroupName TestRG -Name TestVNet `
    -AddressPrefix 192.168.0.0/16 -Location centralus
    ```

    Erwartete Ausgabe:

        Name                  : TestVNet
        ResourceGroupName     : TestRG
        Location              : centralus
        Id                    : /subscriptions/[Subscription Id]/resourceGroups/TestRG/providers/Microsoft.Network/virtualNetworks/TestVNet
        Etag                   : W/"[Id]"
        ProvisioningState          : Succeeded
        Tags                       : 
        AddressSpace               : {
                                   "AddressPrefixes": [
                                     "192.168.0.0/16"
                                   ]
                                  }
        DhcpOptions                : {}
        Subnets                    : []
        VirtualNetworkPeerings     : []
4. Speichern Sie das virtuelle Netzwerkobjekt als Variable:

    ```powershell
    $vnet = Get-AzureRmVirtualNetwork -ResourceGroupName TestRG -Name TestVNet
    ```

   > [!TIP]
   > Sie können die Schritte 3 und 4 kombinieren, indem Sie `$vnet = New-AzureRmVirtualNetwork -ResourceGroupName TestRG -Name TestVNet -AddressPrefix 192.168.0.0/16 -Location centralus` ausführen.
   > 

5. Fügen Sie der neuen VNET-Variable ein Subnetz hinzu:

    ```powershell
    Add-AzureRmVirtualNetworkSubnetConfig -Name FrontEnd `
    -VirtualNetwork $vnet -AddressPrefix 192.168.1.0/24
    ```

    Erwartete Ausgabe:
   
        Name                  : TestVNet
        ResourceGroupName     : TestRG
        Location              : centralus
        Id                    : /subscriptions/[Subscription Id]/resourceGroups/TestRG/providers/Microsoft.Network/virtualNetworks/TestVNet
        Etag                  : W/"[Id]"
        ProvisioningState     : Succeeded
        Tags                  :
        AddressSpace          : {
                                  "AddressPrefixes": [
                                    "192.168.0.0/16"
                                  ]
                                }
        DhcpOptions           : {}
        Subnets             : [
                                  {
                                    "Name": "FrontEnd",
                                    "AddressPrefix": "192.168.1.0/24"
                                  }
                                ]
        VirtualNetworkPeerings     : []

6. Wiederholen Sie Schritt 5 für jedes Subnetz, das Sie erstellen möchten. Der folgende Befehl erstellt das *BackEnd*-Subnetz für dieses Szenario:

    ```powershell
    Add-AzureRmVirtualNetworkSubnetConfig -Name BackEnd `
    -VirtualNetwork $vnet -AddressPrefix 192.168.2.0/24
    ```

7. Obwohl Sie Subnetze erstellen, sind diese derzeit nur in der lokalen Variablen enthalten, mit der das in Schritt 4 erstellte VNet abgerufen wird. Um Ihre Änderungen per Push an Azure zu übertragen, führen Sie folgenden Befehl aus:

    ```powershell
    Set-AzureRmVirtualNetwork -VirtualNetwork $vnet
    ```
   
    Erwartete Ausgabe:
   
        Name                  : TestVNet
        ResourceGroupName     : TestRG
        Location              : centralus
        Id                    : /subscriptions/[Subscription Id]/resourceGroups/TestRG/providers/Microsoft.Network/virtualNetworks/TestVNet
        Etag                  : W/"[Id]"
        ProvisioningState     : Succeeded
        Tags                  :
        AddressSpace          : {
                                  "AddressPrefixes": [
                                    "192.168.0.0/16"
                                  ]
                                }
        DhcpOptions           : {
                                  "DnsServers": []
                                }
        Subnets               : [
                                  {
                                    "Name": "FrontEnd",
                                    "Etag": "W/\"[Id]\"",
                                    "Id": "/subscriptions/[Subscription Id]/resourceGroups/TestRG/providers/Microsoft.Network/virtualNetworks/TestVNet/subnets/FrontEnd",
                                    "AddressPrefix": "192.168.1.0/24",
                                    "IpConfigurations": [],
                                    "ProvisioningState": "Succeeded"
                                  },
                                  {
                                    "Name": "BackEnd",
                                    "Etag": "W/\"[Id]\"",
                                    "Id": "/subscriptions/[Subscription Id]/resourceGroups/TestRG/providers/Microsoft.Network/virtualNetworks/TestVNet/subnets/BackEnd",
                                    "AddressPrefix": "192.168.2.0/24",
                                    "IpConfigurations": [],
                                    "ProvisioningState": "Succeeded"
                                  }
                                ]
        VirtualNetworkPeerings : []

## <a name="next-steps"></a>Nächste Schritte

Informieren Sie sich, wie folgende Verbindungen hergestellt werden:

- Eine Verbindung eines virtuellen Computers (VM) mit einem virtuellen Netzwerk. Lesen Sie hierzu den Artikel [Erstellen eines Windows-Computers](../virtual-machines/virtual-machines-windows-ps-create.md). Anstatt gemäß den Schritten in den Artikeln ein VNet und ein Subnetz zu erstellen, können Sie für die Verbindung mit der VM auch ein vorhandenes VNet und Subnetz auswählen.
- Eine Verbindung des virtuellen Netzwerks mit anderen virtuellen Netzwerken. Lesen Sie hierzu den Artikel [Konfigurieren einer VNet-zu-VNet-Verbindung über das Azure-Portal](../vpn-gateway/vpn-gateway-vnet-vnet-rm-ps.md).
- Eine Verbindung des virtuellen Netzwerks mit einem lokalen Netzwerk. Verwenden Sie hierfür ein Site-to-Site-VPN oder eine ExpressRoute-Verbindung. Informationen zur Vorgehensweise finden Sie in den Artikeln [Add a Site-to-Site connection to a VNet with an existing VPN gateway connection](../vpn-gateway/vpn-gateway-howto-multi-site-to-site-resource-manager-portal.md) (Hinzufügen einer Site-to-Site-Verbindung zu einem VNet mit einer vorhandenen VPN-Gatewayverbindung) und [Verknüpfen eines VNet mit einer ExpressRoute-Verbindung](../expressroute/expressroute-howto-linkvnet-arm.md).



<!--HONumber=Jan17_HO1-->


