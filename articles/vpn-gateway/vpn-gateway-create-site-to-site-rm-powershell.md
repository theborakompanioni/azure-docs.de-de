---
title: 'Herstellen einer Verbindung zwischen Ihrem lokalen Netzwerk und einem virtuellen Azure-Netzwerk: Site-to-Site-VPN: PowerShell | Microsoft-Dokumentation'
description: "Schritte zum Erstellen einer IPsec-Verbindung zwischen Ihrem lokalen Netzwerk und einem virtuellen Azure-Netzwerk über das öffentliche Internet. Anhand dieser Schritte können Sie mithilfe von PowerShell eine standortübergreifende Site-to-Site-VPN Gateway-Verbindung erstellen."
services: vpn-gateway
documentationcenter: na
author: cherylmc
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: fcc2fda5-4493-4c15-9436-84d35adbda8e
ms.service: vpn-gateway
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/01/2017
ms.author: cherylmc
ms.translationtype: Human Translation
ms.sourcegitcommit: 64bd7f356673b385581c8060b17cba721d0cf8e3
ms.openlocfilehash: f485dc6a52488b44bbd0e68432d3fd2bcdb060a9
ms.contentlocale: de-de
ms.lasthandoff: 05/02/2017


---
# <a name="create-a-vnet-with-a-site-to-site-vpn-connection-using-powershell"></a>Erstellen eines VNET mit einer Site-to-Site-VPN-Verbindung per PowerShell

In diesem Artikel erfahren Sie, wie Sie PowerShell zum Erstellen einer Site-to-Site-VPN Gateway-Verbindung zwischen Ihrem lokalen Netzwerk und dem VNET verwenden. Die Schritte in diesem Artikel gelten für das Resource Manager-Bereitstellungsmodell. Sie können diese Konfiguration auch mit einem anderen Bereitstellungstool oder -modell erstellen. Wählen Sie hierzu in der folgenden Liste eine andere Option:

> [!div class="op_single_selector"]
> * [Resource Manager – Azure-Portal](vpn-gateway-howto-site-to-site-resource-manager-portal.md)
> * [Resource Manager – PowerShell](vpn-gateway-create-site-to-site-rm-powershell.md)
> * [Resource Manager – Befehlszeilenschnittstelle](vpn-gateway-howto-site-to-site-resource-manager-cli.md)
> * [Klassisch – Azure-Portal](vpn-gateway-howto-site-to-site-classic-portal.md)
> * [Klassisch – Klassisches Portal](vpn-gateway-site-to-site-create.md)
> 
>


![Diagramm für die standortübergreifende Site-to-Site-VPN Gateway-Verbindung](./media/vpn-gateway-create-site-to-site-rm-powershell/site-to-site-connection-diagram.png)

Eine Site-to-Site-VPN-Gateway-Verbindung wird verwendet, um Ihr lokales Netzwerk über einen IPsec/IKE-VPN-Tunnel (IKEv1 oder IKEv2) mit einem virtuellen Azure-Netzwerk zu verbinden. Für diese Art von Verbindung wird ein lokales VPN-Gerät benötigt, dem eine extern zugängliche, öffentliche IP-Adresse zugewiesen ist. Weitere Informationen zu VPN-Gateways finden Sie unter [Informationen zu VPN Gateway](vpn-gateway-about-vpngateways.md).

## <a name="before-you-begin"></a>Voraussetzungen

Vergewissern Sie sich vor Beginn der Konfiguration, dass die folgenden Voraussetzungen erfüllt sind bzw. Folgendes vorhanden ist:

* Vergewissern Sie sich, dass Sie das Resource Manager-Bereitstellungsmodell verwenden möchten. [!INCLUDE [deployment models](../../includes/vpn-gateway-deployment-models-include.md)] 
* Ein kompatibles VPN-Gerät (und eine Person, die es konfigurieren kann). Weitere Informationen zu kompatiblen VPN-Geräten und zur Gerätekonfiguration finden Sie unter [Informationen zu VPN-Geräten](vpn-gateway-about-vpn-devices.md).
* Eine extern zugängliche, öffentliche IPv4-Adresse für Ihr VPN-Gerät. Diese IP-Adresse darf sich nicht hinter einer NAT befinden.
* Falls Sie nicht mit den IP-Adressbereichen in Ihrer lokalen Netzwerkkonfiguration vertraut sind, wenden Sie sich an eine Person, die Ihnen diese Informationen zur Verfügung stellen kann. Beim Erstellen dieser Konfiguration müssen Sie die Präfixe für die IP-Adressbereiche angeben, die Azure an Ihren lokalen Standort weiterleitet. Kein Subnetz Ihres lokalen Netzwerks darf sich mit den Subnetzen des virtuellen Netzwerks überschneiden, mit dem Sie eine Verbindung herstellen möchten.
* Die aktuelle Version der PowerShell-Cmdlets für Azure Resource Manager. Weitere Informationen zur Installation der PowerShell-Cmdlets finden Sie unter [Installieren und Konfigurieren von Azure PowerShell](/powershell/azure/overview) .

### <a name="example-values"></a>Beispielwerte

In den Beispielen dieses Artikels werden die folgenden Werte verwendet. Sie können diese Werte zum Erstellen einer Testumgebung verwenden oder zum besseren Verständnis der Beispiele in diesem Artikel heranziehen.

```
#Example values

VnetName                = testvnet 
ResourceGroup           = testrg 
Location                = West US 
AddressSpace            = 10.0.0.0/16 
SubnetName              = Subnet1 
Subnet                  = 10.0.1.0/28 
GatewaySubnet           = 10.0.0.0/27
LocalNetworkGatewayName = LocalSite
LNG Public IP           = <VPN device IP address> 
Local Address Prefixes  = 10.0.0.0/24','20.0.0.0/24
Gateway Name            = vnetgw1
PublicIP                = gwpip
Gateway IP Config       = gwipconfig1 
VPNType                 = RouteBased 
GatewayType             = Vpn 
ConnectionName          = myGWConnection
```

## <a name="Login"></a>1. Verbinden mit Ihrem Abonnement

[!INCLUDE [vpn-gateway-ps-login](../../includes/vpn-gateway-ps-login-include.md)]

## <a name="VNet"></a>2. Erstellen eines virtuelles Netzwerks und eines Gatewaysubnetzes

Falls Sie noch nicht über ein virtuelles Netzwerk verfügen, erstellen Sie eines. Stellen Sie beim Erstellen eines virtuellen Netzwerks sicher, dass sich die angegebenen Adressräume und die Adressräume im lokalen Netzwerk nicht überschneiden. Bei dieser Konfiguration benötigen Sie auch ein Gatewaysubnetz. Für das virtuelle Netzwerkgateway wird ein Gatewaysubnetz verwendet, das die von den VPN-Gatewaydiensten genutzten IP-Adressen enthält. Wenn Sie ein Gatewaysubnetz erstellen, muss es den Namen „GatewaySubnet“ erhalten. Wenn Sie einen anderen Namen verwenden, erstellen Sie zwar ein Subnetz, aber es wird von Azure nicht wie ein Gatewaysubnetz behandelt.

Die Größe des von Ihnen angegebenen Gatewaysubnetzes richtet sich nach der VPN-Gatewaykonfiguration, die Sie erstellen möchten. Es ist zwar möglich, ein Gatewaysubnetz mit einer Größe von nur /29 zu erstellen, es wird jedoch empfohlen, /27 oder /28 auszuwählen, um ein größeres Subnetz mit mehr Adressen zu erstellen. Die Verwendung des größeren Gatewaysubnetzes ermöglicht die Vergabe einer ausreichenden Zahl von IP-Adressen für potenzielle zukünftige Konfigurationen.

[!INCLUDE [vpn-gateway-no-nsg](../../includes/vpn-gateway-no-nsg-include.md)]

### <a name="to-create-a-virtual-network-and-a-gateway-subnet"></a>So erstellen Sie ein virtuelles Netzwerk und ein Gatewaysubnetz

In diesem Beispiel werden ein virtuelles Netzwerk und ein Gatewaysubnetz erstellt. Falls Sie bereits über ein virtuelles Netzwerk verfügen, dem Sie ein Gatewaysubnetz hinzufügen möchten, lesen Sie unter [So fügen Sie einem virtuellen Netzwerk, das Sie bereits erstellt haben, ein Gatewaysubnetz hinzu](#gatewaysubnet) weiter.

Erstellen Sie eine Ressourcengruppe:

```powershell
New-AzureRmResourceGroup -Name testrg -Location 'West US'
```

Erstellen Sie Ihr virtuelles Netzwerk. 

1. Legen Sie die Variablen fest.

  ```powershell
  $subnet1 = New-AzureRmVirtualNetworkSubnetConfig -Name 'GatewaySubnet' -AddressPrefix 10.0.0.0/27
  $subnet2 = New-AzureRmVirtualNetworkSubnetConfig -Name 'Subnet1' -AddressPrefix '10.0.1.0/28'
  ```
2. Erstellen Sie das VNet.

  ```powershell
  New-AzureRmVirtualNetwork -Name testvnet -ResourceGroupName testrg `
  -Location 'West US' -AddressPrefix 10.0.0.0/16 -Subnet $subnet1, $subnet2
  ```

### <a name="gatewaysubnet"></a>So fügen Sie einem virtuellen Netzwerk, das Sie bereits erstellt haben, ein Gatewaysubnetz hinzu

1. Legen Sie die Variablen fest.

  ```powershell
  $vnet = Get-AzureRmVirtualNetwork -ResourceGroupName testrg -Name testvnet
  ```
2. Erstellen Sie das Gatewaysubnetz.

  ```powershell
  Add-AzureRmVirtualNetworkSubnetConfig -Name 'GatewaySubnet' -AddressPrefix 10.0.0.0/27 -VirtualNetwork $vnet
  ```
3. Legen Sie die Konfiguration fest.

  ```powershell
  Set-AzureRmVirtualNetwork -VirtualNetwork $vnet
  ```

## 3. <a name="localnet"></a>Erstellen des lokalen Netzwerkgateways

Mit dem Gateway des lokalen Netzwerks ist normalerweise Ihr lokaler Standort gemeint. Sie geben dem Standort einen Namen, über den Azure darauf verweisen kann, und geben dann die IP-Adresse des lokalen VPN-Geräts an, mit dem Sie eine Verbindung herstellen. Außerdem geben Sie die IP-Adresspräfixe an, die über das VPN-Gateway an das VPN-Gerät weitergeleitet werden. Die von Ihnen angegebenen Adresspräfixe befinden sich in Ihrem lokalen Netzwerk. Sie können diese Präfixe leicht aktualisieren, wenn sich Ihr lokales Netzwerk ändert.

Verwenden Sie die folgenden Werte:

* *GatewayIPAddress* ist die IP-Adresse Ihres lokalen VPN-Geräts. Das VPN-Gerät darf sich nicht hinter einer NAT befinden.
* *AddressPrefix* ist Ihr lokaler Adressraum.

- So fügen Sie ein Gateway für das lokale Netzwerk mit einem Adresspräfix hinzu

  ```powershell
  New-AzureRmLocalNetworkGateway -Name LocalSite -ResourceGroupName testrg `
  -Location 'West US' -GatewayIpAddress '23.99.221.164' -AddressPrefix   '10.0.0.0/24'
  ```

- So fügen Sie ein Gateway für das lokale Netzwerk mit mehreren Adresspräfixen hinzu

  ```powershell
  New-AzureRmLocalNetworkGateway -Name LocalSite -ResourceGroupName testrg `
  -Location 'West US' -GatewayIpAddress '23.99.221.164' -AddressPrefix @('10.0.0.0/24','20.0.0.0/24')
  ```

- So ändern Sie die IP-Adresspräfixe für Ihr lokales Netzwerkgateway:<br>
Es kann vorkommen, dass sich die Präfixe für das Gateway für das lokale Netzwerk ändern. Die Schritte, die Sie zum Ändern der IP-Adresspräfixe ausführen, richten sich danach, ob Sie eine VPN Gateway-Verbindung erstellt haben. Weitere Informationen finden Sie in diesem Artikel im Abschnitt [So ändern Sie die IP-Adresspräfixe für ein lokales Netzwerkgateway](#modify) .

## <a name="PublicIP"></a>4. Anfordern einer öffentlichen IP-Adresse

Ein VPN-Gateway muss über eine öffentliche IP-Adresse verfügen. Sie fordern zuerst die IP-Adressressource an und verweisen dann beim Erstellen des Gateways des virtuellen Netzwerks darauf. Die IP-Adresse wird bei der Erstellung des VPN-Gateways der Ressource dynamisch zugewiesen. VPN Gateway unterstützt derzeit nur die *dynamische* Zuweisung öffentlicher IP-Adressen. Die Zuweisung einer statischen öffentlichen IP-Adresse kann nicht angefordert werden. Das bedeutet jedoch nicht, dass sich die IP-Adresse ändert, nachdem sie Ihrem VPN-Gateway zugewiesen wurde. Die öffentliche IP-Adresse ändert sich nur, wenn das Gateway gelöscht und neu erstellt wird. Sie ändert sich nicht, wenn die Größe geändert wird, das VPN-Gateway zurückgesetzt wird oder andere interne Wartungs-/Upgradevorgänge für das VPN-Gateway durchgeführt werden.

Fordern Sie eine öffentliche IP-Adresse an, die Ihrem VPN-Gateway für das virtuelle Netzwerk zugewiesen wird.

```powershell
$gwpip= New-AzureRmPublicIpAddress -Name gwpip -ResourceGroupName testrg -Location 'West US' -AllocationMethod Dynamic
```

## <a name="GatewayIPConfig"></a>5. Erstellen der Gateway-IP-Adressierung

Die Gatewaykonfiguration definiert das zu verwendende Subnetz und die zu verwendende öffentliche IP-Adresse. Verwenden Sie das folgende Beispiel, um Ihre Gatewaykonfiguration zu erstellen:

```powershell
$vnet = Get-AzureRmVirtualNetwork -Name testvnet -ResourceGroupName testrg
$subnet = Get-AzureRmVirtualNetworkSubnetConfig -Name 'GatewaySubnet' -VirtualNetwork $vnet
$gwipconfig = New-AzureRmVirtualNetworkGatewayIpConfig -Name gwipconfig1 -SubnetId $subnet.Id -PublicIpAddressId $gwpip.Id
```

## <a name="CreateGateway"></a>6. Erstellen des VPN-Gateways

Erstellen Sie das VPN-Gateway für das virtuelle Netzwerk. Die Erstellung eines VPN-Gateways kann bis zu 45 Minuten dauern.

Verwenden Sie die folgenden Werte:

* *-GatewayType* für eine Site-to-Site-Konfiguration lautet *Vpn*. Der Gatewaytyp ist immer spezifisch für die Konfiguration, die Sie implementieren. Beispielsweise kann für andere Gatewaykonfigurationen für -GatewayType die Einstellung „ExpressRoute“ erforderlich sein.
* *-VpnType* kann *RouteBased* (in einigen Dokumentationen als dynamisches Gateway bezeichnet) oder *PolicyBased* lauten (in einigen Dokumentationen als statisches Gateway bezeichnet). Weitere Informationen zu VPN-Gatewaytypen finden Sie unter [Informationen zu VPN-Gateways](vpn-gateway-about-vpngateways.md).
* *-GatewaySku* kann auf „Basic“, „Standard“ oder „HighPerformance“ festgelegt werden. Für bestimmte SKUs gelten Einschränkungen bei der Konfiguration. Weitere Informationen finden Sie unter [Gateway-SKUs](vpn-gateway-about-vpngateways.md#gateway-skus).

```powershell
New-AzureRmVirtualNetworkGateway -Name vnetgw1 -ResourceGroupName testrg `
-Location 'West US' -IpConfigurations $gwipconfig -GatewayType Vpn `
-VpnType RouteBased -GatewaySku Standard
```

## <a name="ConfigureVPNDevice"></a>7. Konfigurieren des VPN-Geräts

[!INCLUDE [Configure VPN device](../../includes/vpn-gateway-configure-vpn-device-rm-include.md)]

Verwenden Sie das folgende Beispiel, um die öffentliche IP-Adresse des virtuellen Netzwerkgateways über PowerShell zu ermitteln:

```powershell
Get-AzureRmPublicIpAddress -Name GW1PublicIP -ResourceGroupName TestRG
```

## <a name="CreateConnection"></a>8. Erstellen der VPN-Verbindung

Erstellen Sie als Nächstes die Site-to-Site-VPN-Verbindung zwischen dem Gateway Ihres virtuellen Netzwerks und Ihrem VPN-Gerät. Achten Sie darauf, dass Sie die Werte durch Ihre eigenen Werte ersetzen. Der gemeinsame Schlüssel muss dem Wert entsprechen, den Sie für Ihre VPN-Gerätekonfiguration verwendet haben. Beachten Sie, dass „-ConnectionType“ für „Site-to-Site“ *IPsec* lautet.

1. Legen Sie die Variablen fest.
  ```powershell
  $gateway1 = Get-AzureRmVirtualNetworkGateway -Name vnetgw1 -ResourceGroupName testrg
  $local = Get-AzureRmLocalNetworkGateway -Name LocalSite -ResourceGroupName testrg
  ```

2. Erstellen Sie die Verbindung.
  ```powershell
  New-AzureRmVirtualNetworkGatewayConnection -Name MyGWConnection -ResourceGroupName testrg `
  -Location 'West US' -VirtualNetworkGateway1 $gateway1 -LocalNetworkGateway2 $local `
  -ConnectionType IPsec -RoutingWeight 10 -SharedKey 'abc123'
  ```

Die Verbindung wird nach kurzer Zeit hergestellt.

## <a name="toverify"></a>9. Überprüfen der VPN-Verbindung
Es gibt mehrere Möglichkeiten, wie Sie Ihre VPN-Verbindung überprüfen können.

[!INCLUDE [Verify connection](../../includes/vpn-gateway-verify-connection-ps-rm-include.md)]

## <a name="connectVM"></a>Herstellen einer Verbindung mit einem virtuellem Computer

[!INCLUDE [Connect to VM](../../includes/vpn-gateway-connect-vm-s2s-include.md)]


## <a name="modify"></a>Ändern der IP-Adresspräfixe für ein Gateway des lokalen Netzwerks

Wenn sich die IP-Adressen ändern, die an den lokalen Standort weitergeleitet werden sollen, können Sie das Gateway des lokalen Netzwerks anpassen. Es sind zwei Anleitungen vorhanden. Welche Anleitung für Sie geeignet ist, hängt davon ab, ob Sie die Gatewayverbindung bereits erstellt haben.

[!INCLUDE [Modify prefixes](../../includes/vpn-gateway-modify-ip-prefix-rm-include.md)]

## <a name="modifygwipaddress"></a>Ändern der Gateway-IP-Adresse für ein Gateway des lokalen Netzwerks

[!INCLUDE [Modify gw IP](../../includes/vpn-gateway-modify-lng-gateway-ip-rm-include.md)]

## <a name="next-steps"></a>Nächste Schritte

*  Sobald die Verbindung hergestellt ist, können Sie Ihren virtuellen Netzwerken virtuelle Computer hinzufügen. Weitere Informationen finden Sie unter [Virtuelle Computer](https://docs.microsoft.com/azure/#pivot=services&panel=Compute) .
* Informationen zu BGP finden Sie in der [Übersicht über BGP](vpn-gateway-bgp-overview.md) und unter [Konfigurieren von BGP](vpn-gateway-bgp-resource-manager-ps.md).

