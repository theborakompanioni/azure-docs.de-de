---
title: "Informationen zu VPN-Gatewayeinstellungen für Gateways virtueller Netzwerke | Microsoft Docs"
description: "Hier erhalten Sie Informationen zu VPN Gateway-Einstellungen für Azure Virtual Network."
services: vpn-gateway
documentationcenter: na
author: cherylmc
manager: carmonm
editor: 
tags: azure-resource-manager,azure-service-management
ms.assetid: ae665bc5-0089-45d0-a0d5-bc0ab4e79899
ms.service: vpn-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 10/18/2016
ms.author: cherylmc
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: fc3c8469657b50e4e09a1849d9c63f4ef1e0c414


---
# <a name="about-vpn-gateway-settings"></a>Informationen zu VPN Gateway-Einstellungen
Für eine Verbindungslösung mit VPN-Gateways müssen mehrere Ressourcen konfiguriert werden, um den Netzwerkdatenverkehr zwischen virtuellen Netzwerken und lokalen Standorten zu senden. Jede Ressource enthält konfigurierbare Einstellungen. Die Kombination von Ressourcen und Einstellungen bestimmt die resultierende Verbindung.

In den Abschnitten in diesem Artikel werden die Ressourcen und Einstellungen beschrieben, die sich im **Resource Manager** -Bereitstellungsmodell auf ein VPN-Gateway beziehen. Vielleicht finden Sie es hilfreich, die verfügbaren Konfigurationen mithilfe von Verbindungstopologiediagrammen anzuzeigen. Die Beschreibungen und Topologiediagramme für jede Verbindungslösung finden Sie im Artikel [Informationen zu VPN Gateway](vpn-gateway-about-vpngateways.md) . 

## <a name="a-namegwtypeagateway-types"></a><a name="gwtype"></a>Gatewaytypen
Jedes virtuelle Netzwerk kann nur über ein einziges virtuelles Netzwerkgateway jedes Typs verfügen. Beim Erstellen eines Gateways des virtuellen Netzwerks müssen Sie sicherstellen, dass der Gatewaytyp für Ihre Konfiguration korrekt ist.

Die verfügbaren Werte für -GatewayType lauten: 

* VPN
* ExpressRoute

Ein VPN-Gateway erfordert den `-GatewayType` *Vpn*.  

Beispiel:

    New-AzureRmVirtualNetworkGateway -Name vnetgw1 -ResourceGroupName testrg `
    -Location 'West US' -IpConfigurations $gwipconfig -GatewayType Vpn `
    -VpnType RouteBased


## <a name="a-namegwskuagateway-skus"></a><a name="gwsku"></a>Gateway-SKUs
[!INCLUDE [vpn-gateway-gwsku-include](../../includes/vpn-gateway-gwsku-include.md)]

### <a name="configuring-the-gateway-sku"></a>Konfigurieren der Gateway-SKU
**Angeben der Gateway-SKU im Azure-Portal**

Wenn Sie das Azure-Portal verwenden, um ein Resource Manager-Gateway für virtuelle Netzwerke zu erstellen, können Sie die Gateway-SKU in der Dropdownliste auswählen. Die angegebenen Optionen entsprechen dem ausgewählten Gateway- und VPN-Typ.

Beispiel: Wenn Sie als Gatewaytyp „VPN“ und als VPN-Typ „Richtlinienbasiert“ auswählen, wird nur die SKU „Basic“ angezeigt, da nur diese SKU für richtlinienbasierte VPNs verfügbar ist. Wenn Sie „Routenbasiert“ festlegen, können Sie zwischen den SKUs „Basic“, „Standard“ und „HighPerformance“ wählen. 

**Angeben der Gateway-SKU mithilfe von PowerShell**

Das folgende PowerShell-Beispiel gibt `-GatewaySku` als *Standard*an.

    New-AzureRmVirtualNetworkGateway -Name vnetgw1 -ResourceGroupName testrg `
    -Location 'West US' -IpConfigurations $gwipconfig -GatewaySku Standard `
    -GatewayType Vpn -VpnType RouteBased

**Ändern einer Gateway-SKU**

Wenn Sie für Ihre Gateway-SKU ein Upgrade auf eine leistungsfähigere SKU (von „Basic“/„Standard“ auf „HighPerformance“) ausführen möchten, können Sie das PowerShell-Cmdlet `Resize-AzureRmVirtualNetworkGateway` verwenden. Sie können mit diesem Cmdlet auch ein Downgrade der Gateway-SKU-Größe durchführen.

Das folgende PowerShell-Beispiel zeigt die Änderung der Größe einer Gateway-SKU auf High Performance.

    $gw = Get-AzureRmVirtualNetworkGateway -Name vnetgw1 -ResourceGroupName testrg
    Resize-AzureRmVirtualNetworkGateway -VirtualNetworkGateway $gw -GatewaySku HighPerformance

### <a name="estimated-aggregate-throughput-by-gateway-sku-and-type"></a>Voraussichtlicher aggregierter Durchsatz nach Gateway-SKU und -Typ
[!INCLUDE [vpn-gateway-table-gwtype-aggthroughput](../../includes/vpn-gateway-table-gwtype-aggtput-include.md)]

## <a name="a-nameconnectiontypeaconnection-types"></a><a name="connectiontype"></a>Verbindungstypen
Im Resource Manager-Bereitstellungsmodell ist für jede Konfiguration ein bestimmter Typ der Verbindung mit dem Gateway eines virtuellen Netzwerks erforderlich. Die verfügbaren Resource Manager-PowerShell-Werte für `-ConnectionType` sind:

* IPsec
* Vnet2Vnet
* ExpressRoute
* VPNClient

Im folgenden PowerShell-Beispiel erstellen wir eine S2S-Verbindung, die den Verbindungstyp *IPsec*erfordert.

    New-AzureRmVirtualNetworkGatewayConnection -Name localtovon -ResourceGroupName testrg `
    -Location 'West US' -VirtualNetworkGateway1 $gateway1 -LocalNetworkGateway2 $local `
    -ConnectionType IPsec -RoutingWeight 10 -SharedKey 'abc123'


## <a name="a-namevpntypeavpn-types"></a><a name="vpntype"></a>VPN-Typen
Wenn Sie das Gateway des virtuellen Netzwerks für eine VPN-Gatewaykonfiguration erstellen, müssen Sie einen VPN-Typ angeben. Der ausgewählte VPN-Typ hängt von der Verbindungstopologie ab, die Sie erstellen möchten. Beispielsweise erfordert eine P2S-Verbindung den VPN-Typ „RouteBased“. Ein VPN-Typ kann außerdem von der Hardware abhängen, die Sie verwenden möchten. S2S-Konfigurationen erfordern ein VPN-Gerät. Einige VPN-Geräte unterstützen nur einen bestimmten VPN-Typ.

Der ausgewählte VPN-Typ muss alle Verbindungsanforderungen für die Lösung erfüllen, die Sie erstellen möchten. Beispiel: Wenn Sie eine S2S-VPN-Gatewayverbindung und eine P2S-VPN-Gatewayverbindung für dasselbe virtuelle Netzwerk erstellen möchten, verwenden Sie den VPN-Typ *RouteBased* , da P2S diesen VPN-Typ erfordert. Sie müssen auch sicherstellen, dass Ihr VPN-Gerät eine RouteBased-VPN-Verbindung unterstützt. 

Nachdem ein Gateway des virtuellen Netzwerks erstellt wurde, können Sie den VPN-Typ nicht mehr ändern. Sie müssen das Gateway des virtuellen Netzwerks löschen und ein neues erstellen. Es gibt zwei VPN-Typen:

[!INCLUDE [vpn-gateway-vpntype](../../includes/vpn-gateway-vpntype-include.md)]

Das folgende PowerShell-Beispiel gibt `-VpnType` als *RouteBased*an. Beim Erstellen eines Gateways müssen Sie sicherstellen, dass -VpnType für Ihre Konfiguration korrekt ist. 

    New-AzureRmVirtualNetworkGateway -Name vnetgw1 -ResourceGroupName testrg `
    -Location 'West US' -IpConfigurations $gwipconfig `
    -GatewayType Vpn -VpnType RouteBased

## <a name="a-namerequirementsagateway-requirements"></a><a name="requirements"></a>Gatewayanforderungen
[!INCLUDE [vpn-gateway-table-requirements](../../includes/vpn-gateway-table-requirements-include.md)]

## <a name="a-namegwsubagateway-subnet"></a><a name="gwsub"></a>Gatewaysubnetz
Sie müssen zuerst ein Gatewaysubnetz für Ihr VNet erstellen, um ein Gateway des virtuellen Netzwerks zu konfigurieren. Das Gatewaysubnetz muss den Namen *GatewaySubnet* aufweisen, damit es einwandfrei funktioniert. Durch diesen Namen weiß Azure, dass dieses Subnetz für das Gateway verwendet werden soll.

Die Mindestgröße des Gatewaysubnetzes hängt gänzlich von der Konfiguration ab, die Sie erstellen möchten. Obwohl es möglich ist, ein Gatewaysubnetz mit einer Größe von nur /29 zu erstellen, empfehlen wir, ein Gatewaysubnetz von mindestens /28 (/ 28, / 27, /26 usw.) zu erstellen. 

Wenn Sie eine höhere Gatewaygröße wählen, wird verhindert, dass Sie die Größenbeschränkung für Gateways erreichen. Beispiel: Sie könnten ein Gateway für ein virtuelles Netzwerk mit einem Gatewaysubnetz der Größe /29 für eine S2S-Verbindung erstellt haben. Sie möchten jetzt eine Konfiguration einrichten, in der S2S und ExpressRoute gleichzeitig verwendet werden. Diese Konfiguration erfordert ein Gatewaysubnetz mit einer Mindestgröße von /28. Um Ihre Konfiguration zu erstellen, müssten Sie das Gatewaysubnetz an die Mindestanforderung für die Verbindung anpassen, und das ist /28.

Im folgenden Resource Manager-PowerShell-Beispiel ist ein Gatewaysubnetz mit dem Namen GatewaySubnet zu sehen. Sie erkennen, das mit der CIDR-Notation die Größe /27 angegeben wird. Dies ist für eine ausreichende Zahl von IP-Adressen für die meisten Konfigurationen, die derzeit üblich sind, groß genug.

    Add-AzureRmVirtualNetworkSubnetConfig -Name 'GatewaySubnet' -AddressPrefix 10.0.3.0/27

[!INCLUDE [vpn-gateway-no-nsg](../../includes/vpn-gateway-no-nsg-include.md)]

## <a name="a-namelngalocal-network-gateways"></a><a name="lng"></a>Gateways des lokalen Netzwerks
Wenn Sie eine VPN-Gatewaykonfiguration erstellen, stellt das Gateway des lokalen Netzwerks meist Ihren lokalen Standort dar. Beim klassischen Bereitstellungsmodell wurde das Gateway des lokalen Netzwerks als „Lokaler Standort“ bezeichnet. 

Sie geben dem Gateway des lokalen Netzwerks einen Namen, stellen die öffentliche IP-Adresse des lokalen VPN-Geräts bereit und geben die Adresspräfixe an, die sich am lokalen Standort befinden. Azure sucht unter den Zieladresspräfixen nach Netzwerkdatenverkehr, sieht in der Konfiguration nach, die Sie für das Gateway des lokalen Netzwerks angegeben haben, und leitet die Pakete entsprechend weiter. Sie geben auch Gateways des lokalen Netzwerks für VNet-zu-VNet-Konfigurationen an, die eine VPN-Gatewayverbindung verwenden.

Mit dem folgenden PowerShell-Beispiel wird ein neues Gateway des lokalen Netzwerks erstellt:

    New-AzureRmLocalNetworkGateway -Name LocalSite -ResourceGroupName testrg `
    -Location 'West US' -GatewayIpAddress '23.99.221.164' -AddressPrefix '10.5.51.0/24'

Manchmal müssen Sie die Einstellungen des Gateways des lokalen Netzwerks ändern, beispielsweise wenn Sie den Adressbereich erweitern oder ändern oder wenn sich die IP-Adresse des VPN-Geräts ändert. Bei einem klassischen VNet können Sie diese Einstellungen im klassischen Portal auf der Seite „Lokale Netzwerke“ ändern. Informationen zu Resource Manager finden Sie unter [Ändern der Einstellungen des lokalen Netzwerkgateways mit PowerShell](vpn-gateway-modify-local-network-gateway.md).

## <a name="a-nameresourcesarest-apis-and-powershell-cmdlets"></a><a name="resources"></a>REST-APIs und PowerShell-Cmdlets
Zusätzliche technische Ressourcen und spezielle Syntaxanforderungen bei der Verwendung von REST-APIs und PowerShell-Cmdlets für VPN-Gatewaykonfigurationen finden Sie auf den folgenden Seiten:

| **Klassisch** | **Ressourcen-Manager** |
| --- | --- |
| [PowerShell](https://msdn.microsoft.com/library/mt270335.aspx) |[PowerShell](https://msdn.microsoft.com/library/mt163510.aspx) |
| [REST-API](https://msdn.microsoft.com/library/jj154113.aspx) |[REST-API](https://msdn.microsoft.com/library/mt163859.aspx) |

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen zu verfügbaren Verbindungskonfigurationen finden Sie unter [Informationen zu VPN Gateway](vpn-gateway-about-vpngateways.md) . 




<!--HONumber=Nov16_HO3-->


