---
title: "Herstellen einer Verbindung für ein virtuelles Azure-Netzwerk mit einem anderen VNet: PowerShell | Microsoft-Dokumentation"
description: "Dieser Artikel führt Sie durch das Verbinden virtueller Netzwerke mithilfe von Azure-Ressourcen-Manager und PowerShell."
services: vpn-gateway
documentationcenter: na
author: cherylmc
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 0683c664-9c03-40a4-b198-a6529bf1ce8b
ms.service: vpn-gateway
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/22/2017
ms.author: cherylmc
ms.translationtype: Human Translation
ms.sourcegitcommit: d9ae8e8948d82b9695d7d144d458fe8180294084
ms.openlocfilehash: 56f101f4aecb6987d7ea6fb81bbf7f19a52baacd
ms.contentlocale: de-de
ms.lasthandoff: 05/23/2017


---
# <a name="configure-a-vnet-to-vnet-vpn-gateway-connection-using-powershell"></a>Konfigurieren einer VNet-zu-VNet-VPN-Gatewayverbindung mithilfe von PowerShell

Dieser Artikel zeigt, wie Sie eine VPN-Gatewayverbindung zwischen virtuellen Netzwerken erstellen. Die virtuellen Netzwerke können sich in derselben oder in unterschiedlichen Regionen befinden und aus demselben oder unterschiedlichen Abonnements stammen. Die Schritte in diesem Artikel gelten für das Resource Manager-Bereitstellungsmodell und für die Verwendung von PowerShell. Sie können diese Konfiguration auch mit einem anderen Bereitstellungstool oder -modell erstellen. Wählen Sie hierzu in der folgenden Liste eine andere Option:

> [!div class="op_single_selector"]
> * [Resource Manager – Azure-Portal](vpn-gateway-howto-vnet-vnet-resource-manager-portal.md)
> * [Resource Manager – PowerShell](vpn-gateway-vnet-vnet-rm-ps.md)
> * [Resource Manager – Azure CLI](vpn-gateway-howto-vnet-vnet-cli.md)
> * [Klassisch – Azure-Portal](vpn-gateway-howto-vnet-vnet-portal-classic.md)
> * [Verbinden von verschiedenen Bereitstellungsmodellen – Azure-Portal](vpn-gateway-connect-different-deployment-models-portal.md)
> * [Verbinden von virtuellen Netzwerken aus verschiedenen Bereitstellungsmodellen mit PowerShell](vpn-gateway-connect-different-deployment-models-powershell.md)
>
>

Das Verbinden eines virtuellen Netzwerks mit einem anderen virtuellen Netzwerk (VNet-zu-VNet) ähnelt dem Verbinden eines VNet mit einem lokalen Standort. Beide Verbindungstypen verwenden ein VPN-Gateway, um einen sicheren Tunnel mit IPsec/IKE bereitzustellen. Wenn sich Ihre VNETs in der gleichen Region befinden, kann es hilfreich sein, sie mittels VNET-Peering zu verbinden. Beim VNET-Peering wird kein VPN-Gateway verwendet. Weitere Informationen finden Sie unter [VNet-Peering](../virtual-network/virtual-network-peering-overview.md).

Die VNET-zu-VNET-Kommunikation kann mit Konfigurationen für mehrere Standorte kombiniert werden. Auf diese Weise können Sie Netzwerktopologien einrichten, die wie in der folgenden Abbildung dargestellt standortübergreifende Konnektivität mit Konnektivität zwischen virtuellen Netzwerken kombinieren:

![Informationen zu Verbindungen](./media/vpn-gateway-vnet-vnet-rm-ps/aboutconnections.png)

### <a name="why-connect-virtual-networks"></a>Gründe für Verbindungen zwischen virtuellen Netzwerken

Aus den folgenden Gründen empfiehlt sich das Herstellen von Verbindungen zwischen virtuellen Netzwerken:

* **Regionsübergreifende Georedundanz und Geopräsenz**

  * Sie können Ihre eigene Georeplikation oder -synchronisierung mit sicheren Verbindungen einrichten, ohne Endpunkte mit Internetzugriff verwenden zu müssen.
  * Mit Azure Traffic Manager und Load Balancer können Sie eine hoch verfügbare Workload mit Georedundanz über mehrere Azure-Regionen hinweg einrichten. Ein wichtiges Beispiel ist die Einrichtung von SQL Always On mit Verfügbarkeitsgruppen, die sich über mehrere Azure-Regionen erstrecken.
* **Regionale Anwendungen mit mehreren Ebenen und Isolation oder Verwaltungsgrenze**

  * In derselben Region können Sie Anwendungen mit mehreren Ebenen und mehreren virtuellen Netzwerken einrichten, die aufgrund von Isolations- oder Verwaltungsanforderungen miteinander verbunden sind.

Weitere Informationen zu VNet-zu-VNet-Verbindungen finden Sie am Ende dieses Artikels unter [Häufig gestellte Fragen zu VNet-zu-VNet-Verbindungen](#faq) .

## <a name="which-set-of-steps-should-i-use"></a>Welche Schritte soll ich ausführen?

In diesem Artikel finden Sie zwei unterschiedliche Anleitungen: eine Reihe von Schritten für [VNets, die sich in demselben Abonnement befinden](#samesub), und eine weitere für [VNets, die sich in verschiedenen Abonnements befinden](#difsub). Der wichtigste Unterschied zwischen den beiden Anleitungen besteht darin, ob Sie alle virtuellen Netzwerk- und Gatewayressourcen innerhalb derselben PowerShell-Sitzung erstellen und konfigurieren können.

In den Schritten in diesem Artikel werden Variablen verwendet, die am Anfang jedes Abschnitts deklariert werden. Wenn Sie bereits mit vorhandenen VNets arbeiten, ändern Sie die Variablen entsprechend den Einstellungen Ihrer eigenen Umgebung. Wenn Sie eine Namensauflösung für die virtuellen Netzwerke verwenden möchten, helfen Ihnen die Informationen unter [Namensauflösung](../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md) weiter.

## <a name="samesub"></a>Verbinden von VNets in demselben Abonnement

![v2v-Diagramm](./media/vpn-gateway-vnet-vnet-rm-ps/v2vrmps.png)

### <a name="before-you-begin"></a>Voraussetzungen

Bevor Sie beginnen, müssen Sie die PowerShell-Cmdlets für Azure Resource Manager installieren. Weitere Informationen zur Installation der PowerShell-Cmdlets finden Sie unter [Overview of Azure PowerShell](/powershell/azure/overview) (Übersicht über Azure PowerShell). 

### <a name="Step1"></a>Schritt 1: Planen der IP-Adressbereiche

In den folgenden Schritten erstellen wir zwei virtuelle Netzwerke sowie die jeweiligen Gatewaysubnetze und Konfigurationen. Anschließend erstellen wir eine VPN-Verbindung zwischen den beiden VNets. Es ist wichtig, die IP-Adressbereiche für Ihre Netzwerkkonfiguration zu planen. Denken Sie daran, dass Sie sicherstellen müssen, dass keiner der VNet-Bereiche oder der Bereiche des lokalen Netzwerks Überschneidungen aufweist. In diesen Beispielen verwenden wir keinen Namenserver. Wenn Sie eine Namensauflösung für die virtuellen Netzwerke möchten, finden Sie unter [Namensauflösung](../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md) Informationen dazu.

In den Beispielen werden die folgenden Werte verwendet:

**Werte für TestVNet1:**

* VNet-Name: TestVNet1
* Ressourcengruppe: TestRG1
* Standort: USA, Osten
* TestVNet1: 10.11.0.0/16 & 10.12.0.0/16
* FrontEnd: 10.11.0.0/24
* BackEnd: 10.12.0.0/24
* GatewaySubnet: 10.12.255.0/27
* GatewayName: VNet1GW
* Öffentliche IP-Adresse: VNet1GWIP
* VPNType: RouteBased
* Verbindung (1to4): VNet1toVNet4
* Verbindung (1to5): VNet1toVNet5
* ConnectionType: VNet2VNet

**Werte für TestVNet4:**

* VNet-Name: TestVNet4
* TestVNet2: 10.41.0.0/16 & 10.42.0.0/16
* FrontEnd: 10.41.0.0/24
* BackEnd: 10.42.0.0/24
* GatewaySubnet: 10.42.255.0/27
* Ressourcengruppe: TestRG4
* Standort: USA, Westen
* GatewayName: VNet4GW
* Öffentliche IP-Adresse: VNet4GWIP
* VPNType: RouteBased
* Verbindung: VNet4toVNet1
* ConnectionType: VNet2VNet


### <a name="Step2"></a>Schritt 2: Erstellen und Konfigurieren von TestVNet1

1. Deklarieren Sie Ihre Variablen. Im Beispiel werden die Variablen mit den Werten für diese Übung deklariert. In den meisten Fällen sollten Sie die Werte durch Ihre eigenen Werte ersetzen. Sie können diese Variablen jedoch verwenden, wenn Sie die Schritte nur durchgehen, um sich mit dieser Art von Konfiguration vertraut zu machen. Ändern Sie die Variablen ggf., kopieren Sie sie, und fügen Sie sie in die PowerShell-Konsole ein.

  ```powershell
  $Sub1 = "Replace_With_Your_Subcription_Name"
  $RG1 = "TestRG1"
  $Location1 = "East US"
  $VNetName1 = "TestVNet1"
  $FESubName1 = "FrontEnd"
  $BESubName1 = "Backend"
  $GWSubName1 = "GatewaySubnet"
  $VNetPrefix11 = "10.11.0.0/16"
  $VNetPrefix12 = "10.12.0.0/16"
  $FESubPrefix1 = "10.11.0.0/24"
  $BESubPrefix1 = "10.12.0.0/24"
  $GWSubPrefix1 = "10.12.255.0/27"
  $GWName1 = "VNet1GW"
  $GWIPName1 = "VNet1GWIP"
  $GWIPconfName1 = "gwipconf1"
  $Connection14 = "VNet1toVNet4"
  $Connection15 = "VNet1toVNet5"
  ```

2. Stellen Sie eine Verbindung mit Ihrem Konto her. Verwenden Sie das folgende Beispiel, um eine Verbindung herzustellen:

  ```powershell
  Login-AzureRmAccount
  ```

  Überprüfen Sie die Abonnements für das Konto.

  ```powershell
  Get-AzureRmSubscription
  ```

  Geben Sie das Abonnement an, das Sie verwenden möchten.

  ```powershell
  Select-AzureRmSubscription -SubscriptionName $Sub1
  ```
3. Erstellen Sie eine neue Ressourcengruppe.

  ```powershell
  New-AzureRmResourceGroup -Name $RG1 -Location $Location1
  ```
4. Erstellen Sie die Subnetzkonfigurationen für „TestVNet1“. In diesem Beispiel wird ein virtuelles Netzwerk mit dem Namen TestVNet1 und drei Subnetzen namens „GatewaySubnet“, „FrontEnd“ und „Backend“ erstellt. Beim Ersetzen der Werte ist es wichtig, dass Sie Ihrem Gatewaysubnetz immer den Namen „GatewaySubnet“ geben. Wenn Sie einen anderen Namen verwenden, tritt beim Erstellen des Gateways ein Fehler auf.

  Im folgenden Beispiel werden die zuvor festgelegten Variablen verwendet. Im Beispiel wird ein Gatewaysubnetz mit einem Subnetz der Größe /27 verwendet. Es ist zwar möglich, ein Gatewaysubnetz von /29 zu erstellen, es wird jedoch empfohlen, ein größeres Subnetz mit mehr Adressen zu erstellen und mindestens /28 oder /27 auszuwählen. Dadurch steht eine ausreichend hohe Anzahl von Adressen für mögliche zusätzliche Konfigurationen zur Verfügung, die Sie zukünftig vielleicht benötigen.

  ```powershell
  $fesub1 = New-AzureRmVirtualNetworkSubnetConfig -Name $FESubName1 -AddressPrefix $FESubPrefix1
  $besub1 = New-AzureRmVirtualNetworkSubnetConfig -Name $BESubName1 -AddressPrefix $BESubPrefix1
  $gwsub1 = New-AzureRmVirtualNetworkSubnetConfig -Name $GWSubName1 -AddressPrefix $GWSubPrefix1
  ```
5. Erstellen Sie „TestVNet1“.

  ```powershell
  New-AzureRmVirtualNetwork -Name $VNetName1 -ResourceGroupName $RG1 `
  -Location $Location1 -AddressPrefix $VNetPrefix11,$VNetPrefix12 -Subnet $fesub1,$besub1,$gwsub1
  ```
6. Fordern Sie eine öffentliche IP-Adresse an, die dem Gateway zugewiesen wird, das Sie für Ihr VNet erstellen. Beachten Sie, dass die Zuordnungsmethode (AllocationMethod) dynamisch ist. Es ist nicht möglich, die IP-Adresse selbst anzugeben. Sie wird Ihrem Gateway dynamisch zugewiesen. 

  ```powershell
  $gwpip1 = New-AzureRmPublicIpAddress -Name $GWIPName1 -ResourceGroupName $RG1 `
  -Location $Location1 -AllocationMethod Dynamic
  ```
7. Erstellen Sie die Gatewaykonfiguration. Die Gatewaykonfiguration definiert das zu verwendende Subnetz und die zu verwendende öffentliche IP-Adresse. Verwenden Sie das Beispiel, um Ihre Gatewaykonfiguration zu erstellen.

  ```powershell
  $vnet1 = Get-AzureRmVirtualNetwork -Name $VNetName1 -ResourceGroupName $RG1
  $subnet1 = Get-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet1
  $gwipconf1 = New-AzureRmVirtualNetworkGatewayIpConfig -Name $GWIPconfName1 `
  -Subnet $subnet1 -PublicIpAddress $gwpip1
  ```
8. Erstellen Sie das Gateway für „TestVNet1“. In diesem Schritt erstellen Sie das virtuelle Netzwerkgateway für TestVNet1. VNet-zu-VNet-Konfigurationen erfordern einen routenbasierten VPN-Typ. Häufig kann die Erstellung eines Gateways je nach ausgewählter Gateway-SKU mindestens 45 Minuten dauern.

  ```powershell
  New-AzureRmVirtualNetworkGateway -Name $GWName1 -ResourceGroupName $RG1 `
  -Location $Location1 -IpConfigurations $gwipconf1 -GatewayType Vpn `
  -VpnType RouteBased -GatewaySku VpnGw1
  ```

### <a name="step-3---create-and-configure-testvnet4"></a>Schritt 3: Erstellen und Konfigurieren von TestVNet4

Nachdem Sie TestVNet1 konfiguriert haben, erstellen Sie TestVNet4. Führen Sie die unten angegebenen Schritte aus, und ersetzen Sie die Werte bei Bedarf durch Ihre eigenen Werte. Sie können diesen Schritt in derselben PowerShell-Sitzung ausführen, da hierfür dasselbe Abonnement gilt.

1. Deklarieren Sie Ihre Variablen. Achten Sie darauf, dass Sie die Werte durch die Werte ersetzen, die Sie für die Konfiguration verwenden möchten.

  ```powershell
  $RG4 = "TestRG4"
  $Location4 = "West US"
  $VnetName4 = "TestVNet4"
  $FESubName4 = "FrontEnd"
  $BESubName4 = "Backend"
  $GWSubName4 = "GatewaySubnet"
  $VnetPrefix41 = "10.41.0.0/16"
  $VnetPrefix42 = "10.42.0.0/16"
  $FESubPrefix4 = "10.41.0.0/24"
  $BESubPrefix4 = "10.42.0.0/24"
  $GWSubPrefix4 = "10.42.255.0/27"
  $GWName4 = "VNet4GW"
  $GWIPName4 = "VNet4GWIP"
  $GWIPconfName4 = "gwipconf4"
  $Connection41 = "VNet4toVNet1"
  ```
2. Erstellen Sie eine neue Ressourcengruppe.

  ```powershell
  New-AzureRmResourceGroup -Name $RG4 -Location $Location4
  ```
3. Erstellen Sie die Subnetzkonfigurationen für „TestVNet4“.

  ```powershell
  $fesub4 = New-AzureRmVirtualNetworkSubnetConfig -Name $FESubName4 -AddressPrefix $FESubPrefix4
  $besub4 = New-AzureRmVirtualNetworkSubnetConfig -Name $BESubName4 -AddressPrefix $BESubPrefix4
  $gwsub4 = New-AzureRmVirtualNetworkSubnetConfig -Name $GWSubName4 -AddressPrefix $GWSubPrefix4
  ```
4. Erstellen Sie „TestVNet4“.

  ```powershell
  New-AzureRmVirtualNetwork -Name $VnetName4 -ResourceGroupName $RG4 `
  -Location $Location4 -AddressPrefix $VnetPrefix41,$VnetPrefix42 -Subnet $fesub4,$besub4,$gwsub4
  ```
5. Fordern Sie eine öffentliche IP-Adresse an.

  ```powershell
  $gwpip4 = New-AzureRmPublicIpAddress -Name $GWIPName4 -ResourceGroupName $RG4 `
  -Location $Location4 -AllocationMethod Dynamic
  ```
6. Erstellen Sie die Gatewaykonfiguration.

  ```powershell
  $vnet4 = Get-AzureRmVirtualNetwork -Name $VnetName4 -ResourceGroupName $RG4
  $subnet4 = Get-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet4
  $gwipconf4 = New-AzureRmVirtualNetworkGatewayIpConfig -Name $GWIPconfName4 -Subnet $subnet4 -PublicIpAddress $gwpip4
  ```
7. Erstellen Sie das Gateway „TestVNet4“. Häufig kann die Erstellung eines Gateways je nach ausgewählter Gateway-SKU mindestens 45 Minuten dauern.

  ```powershell
  New-AzureRmVirtualNetworkGateway -Name $GWName4 -ResourceGroupName $RG4 `
  -Location $Location4 -IpConfigurations $gwipconf4 -GatewayType Vpn `
  -VpnType RouteBased -GatewaySku VpnGw1
  ```

### <a name="step-4---create-the-connections"></a>Schritt 4: Erstellen der Verbindungen

1. Rufen Sie die beiden virtuellen Netzwerkgateways ab. Falls beide Gateways in demselben Abonnement enthalten sind (wie im Beispiel), können Sie diesen Schritt in derselben PowerShell-Sitzung ausführen.

  ```powershell
  $vnet1gw = Get-AzureRmVirtualNetworkGateway -Name $GWName1 -ResourceGroupName $RG1
  $vnet4gw = Get-AzureRmVirtualNetworkGateway -Name $GWName4 -ResourceGroupName $RG4
  ```
2. Erstellen Sie die Verbindung zwischen „TestVNet1“ und „TestVNet4“. In diesem Schritt erstellen Sie die Verbindung von TestVNet1 mit TestVNet4. In den Beispielen wird auf einen gemeinsam verwendeten Schlüssel verwiesen. Sie können eigene Werte für den gemeinsam verwendeten Schlüssel verwenden. Wichtig ist dabei, dass der gemeinsam verwendete Schlüssel für beide Verbindungen übereinstimmen muss. Das Erstellen einer Verbindung kann etwas dauern.

  ```powershell
  New-AzureRmVirtualNetworkGatewayConnection -Name $Connection14 -ResourceGroupName $RG1 `
  -VirtualNetworkGateway1 $vnet1gw -VirtualNetworkGateway2 $vnet4gw -Location $Location1 `
  -ConnectionType Vnet2Vnet -SharedKey 'AzureA1b2C3'
  ```
3. Erstellen Sie die Verbindung zwischen „TestVNet4“ und „TestVNet1“. Dieser Schritt ähnelt dem obigen Schritt, aber hier erstellen Sie jetzt die Verbindung von TestVNet4 mit TestVNet1. Stellen Sie sicher, dass die freigegebenen Schlüssel übereinstimmen. Nach einigen Minuten wird die Verbindung hergestellt.

  ```powershell
  New-AzureRmVirtualNetworkGatewayConnection -Name $Connection41 -ResourceGroupName $RG4 `
  -VirtualNetworkGateway1 $vnet4gw -VirtualNetworkGateway2 $vnet1gw -Location $Location4 `
  -ConnectionType Vnet2Vnet -SharedKey 'AzureA1b2C3'
  ```
4. Überprüfen Sie die Verbindung. Informationen hierzu finden Sie im Abschnitt [Überprüfen der Verbindung](#verify).

## <a name="difsub"></a>Verbinden von VNets aus unterschiedlichen Abonnements

![v2v-Diagramm](./media/vpn-gateway-vnet-vnet-rm-ps/v2vdiffsub.png)

In diesem Szenario verbinden wir TestVNet1 und TestVNet5. TestVNet1 und TestVNet5 befinden sich in unterschiedlichen Abonnements. Der Unterschied zwischen diesen und den vorherigen Schritten besteht darin, dass ein Teil der Konfigurationsschritte in einer separaten PowerShell-Sitzung im Kontext des zweiten Abonnements ausgeführt werden muss. Dies gilt insbesondere dann, wenn die beiden Abonnements unterschiedlichen Organisationen gehören.

### <a name="step-5---create-and-configure-testvnet1"></a>Schritt 5: Erstellen und Konfigurieren von TestVNet1

Sie müssen [Schritt 1](#Step1) und [Schritt 2](#Step2) aus dem vorherigen Abschnitt ausführen, um „TestVNet1“ und das VPN Gateway für „TestVNet1“ zu erstellen und zu konfigurieren. Für diese Konfiguration ist es nicht erforderlich „TestVNet4“ aus dem vorherigen Abschnitt zu erstellen. Wenn Sie das VNet dennoch erstellen, führt dies aber nicht zu Konflikten mit den folgenden Schritten. Nachdem Sie die Schritte 1 und 2 ausgeführt haben, fahren Sie mit Schritt 6 fort, um TestVNet5 zu erstellen. 

### <a name="step-6---verify-the-ip-address-ranges"></a>Schritt 6: Überprüfen der IP-Adressbereiche

Es ist wichtig sicherzustellen, dass sich der IP-Adressbereich des neuen virtuellen Netzwerks (TestVNet5) nicht mit einem Ihrer VNet-Bereiche oder Gatewaybereiche des lokalen Netzwerks überlappt. In diesem Beispiel können die virtuellen Netzwerke unterschiedlichen Organisationen gehören. Bei dieser Übung können Sie für TestVNet5 die folgenden Werte verwenden:

**Werte für TestVNet5:**

* VNet-Name: TestVNet5
* Ressourcengruppe: TestRG5
* Standort: Japan, Osten
* TestVNet5: 10.51.0.0/16 & 10.52.0.0/16
* FrontEnd: 10.51.0.0/24
* BackEnd: 10.52.0.0/24
* GatewaySubnet: 10.52.255.0.0/27
* GatewayName: VNet5GW
* Öffentliche IP-Adresse: VNet5GWIP
* VPNType: RouteBased
* Verbindung: VNet5toVNet1
* ConnectionType: VNet2VNet

### <a name="step-7---create-and-configure-testvnet5"></a>Schritt 7: Erstellen und Konfigurieren von „TestVNet5“

Dieser Schritt muss im Kontext des neuen Abonnements ausgeführt werden. Dieser Teil kann vom Administrator in einer anderen Organisation ausgeführt werden, in deren Besitz sich das Abonnement befindet.

1. Deklarieren Sie Ihre Variablen. Achten Sie darauf, dass Sie die Werte durch die Werte ersetzen, die Sie für die Konfiguration verwenden möchten.

  ```powershell
  $Sub5 = "Replace_With_the_New_Subcription_Name"
  $RG5 = "TestRG5"
  $Location5 = "Japan East"
  $VnetName5 = "TestVNet5"
  $FESubName5 = "FrontEnd"
  $BESubName5 = "Backend"
  $GWSubName5 = "GatewaySubnet"
  $VnetPrefix51 = "10.51.0.0/16"
  $VnetPrefix52 = "10.52.0.0/16"
  $FESubPrefix5 = "10.51.0.0/24"
  $BESubPrefix5 = "10.52.0.0/24"
  $GWSubPrefix5 = "10.52.255.0/27"
  $GWName5 = "VNet5GW"
  $GWIPName5 = "VNet5GWIP"
  $GWIPconfName5 = "gwipconf5"
  $Connection51 = "VNet5toVNet1"
  ```
2. Stellen Sie eine Verbindung mit Abonnement 5 her. Öffnen Sie die PowerShell-Konsole, und stellen Sie eine Verbindung mit Ihrem Konto her. Verwenden Sie das folgende Beispiel, um eine Verbindung herzustellen:

  ```powershell
  Login-AzureRmAccount
  ```

  Überprüfen Sie die Abonnements für das Konto.

  ```powershell
  Get-AzureRmSubscription
  ```

  Geben Sie das Abonnement an, das Sie verwenden möchten.

  ```powershell
  Select-AzureRmSubscription -SubscriptionName $Sub5
  ```
3. Erstellen Sie eine neue Ressourcengruppe.

  ```powershell
  New-AzureRmResourceGroup -Name $RG5 -Location $Location5
  ```
4. Erstellen Sie die Subnetzkonfigurationen für „TestVNet5“.

  ```powershell
  $fesub5 = New-AzureRmVirtualNetworkSubnetConfig -Name $FESubName5 -AddressPrefix $FESubPrefix5
  $besub5 = New-AzureRmVirtualNetworkSubnetConfig -Name $BESubName5 -AddressPrefix $BESubPrefix5
  $gwsub5 = New-AzureRmVirtualNetworkSubnetConfig -Name $GWSubName5 -AddressPrefix $GWSubPrefix5
  ```
5. Erstellen Sie „TestVNet5“.

  ```powershell
  New-AzureRmVirtualNetwork -Name $VnetName5 -ResourceGroupName $RG5 -Location $Location5 `
  -AddressPrefix $VnetPrefix51,$VnetPrefix52 -Subnet $fesub5,$besub5,$gwsub5
  ```
6. Fordern Sie eine öffentliche IP-Adresse an.

  ```powershell
  $gwpip5 = New-AzureRmPublicIpAddress -Name $GWIPName5 -ResourceGroupName $RG5 `
  -Location $Location5 -AllocationMethod Dynamic
  ```
7. Erstellen Sie die Gatewaykonfiguration.

  ```powershell
  $vnet5 = Get-AzureRmVirtualNetwork -Name $VnetName5 -ResourceGroupName $RG5
  $subnet5  = Get-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet5
  $gwipconf5 = New-AzureRmVirtualNetworkGatewayIpConfig -Name $GWIPconfName5 -Subnet $subnet5 -PublicIpAddress $gwpip5
  ```
8. Erstellen Sie das TestVNet5-Gateway.

  ```powershell
  New-AzureRmVirtualNetworkGateway -Name $GWName5 -ResourceGroupName $RG5 -Location $Location5 `
  -IpConfigurations $gwipconf5 -GatewayType Vpn -VpnType RouteBased -GatewaySku VpnGw1
  ```

### <a name="step-8---create-the-connections"></a>Schritt 8: Erstellen der Verbindungen

Da sich die Gateways in diesem Beispiel in unterschiedlichen Abonnements befinden, haben wir diesen Schritt in zwei PowerShell-Sitzungen mit den Bezeichnungen [Abonnement 1] und [Abonnement 5] aufgeteilt.

1. **[Abonnement 1]** Rufen Sie das virtuelle Netzwerkgateway für Abonnement 1 ab. Melden Sie sich an, und stellen Sie eine Verbindung mit Abonnement 1 her, bevor Sie das folgende Beispiel ausführen:

  ```powershell
  $vnet1gw = Get-AzureRmVirtualNetworkGateway -Name $GWName1 -ResourceGroupName $RG1
  ```

  Kopieren Sie die Ausgabe der folgenden Elemente, und senden Sie diese per E-Mail oder auf anderem Wege an den Administrator von Abonnement 5.

  ```powershell
  $vnet1gw.Name
  $vnet1gw.Id
  ```

  Diese beiden Elemente weisen Werte auf, die den Werten in der folgenden Beispielausgabe ähneln:

  ```
  PS D:\> $vnet1gw.Name
  VNet1GW
  PS D:\> $vnet1gw.Id
  /subscriptions/b636ca99-6f88-4df4-a7c3-2f8dc4545509/resourceGroupsTestRG1/providers/Microsoft.Network/virtualNetworkGateways/VNet1GW
  ```
2. **[Abonnement 5]** Rufen Sie das virtuelle Netzwerkgateway für Abonnement 5 ab. Melden Sie sich an, und stellen Sie eine Verbindung mit Abonnement 5 her, bevor Sie das folgende Beispiel ausführen:

  ```powershell
  $vnet5gw = Get-AzureRmVirtualNetworkGateway -Name $GWName5 -ResourceGroupName $RG5
  ```

  Kopieren Sie die Ausgabe der folgenden Elemente, und senden Sie diese per E-Mail oder auf anderem Wege an den Administrator von Abonnement 1.

  ```powershell
  $vnet5gw.Name
  $vnet5gw.Id
  ```

  Diese beiden Elemente weisen Werte auf, die den Werten in der folgenden Beispielausgabe ähneln:

  ```
  PS C:\> $vnet5gw.Name
  VNet5GW
  PS C:\> $vnet5gw.Id
  /subscriptions/66c8e4f1-ecd6-47ed-9de7-7e530de23994/resourceGroups/TestRG5/providers/Microsoft.Network/virtualNetworkGateways/VNet5GW
  ```
3. **[Abonnement 1]** Erstellen Sie die Verbindung zwischen „TestVNet1“ und „TestVNet5“. In diesem Schritt erstellen Sie die Verbindung von TestVNet1 mit TestVNet5. Der Unterschied hierbei ist, dass „$vnet5gw“ nicht direkt abgerufen werden kann, da es sich in einem anderen Abonnement befindet. Sie müssen ein neues PowerShell-Objekt mit den Werten erstellen, die in den obigen Schritten aus Abonnement 1 übermittelt wurden. Sehen Sie sich das Beispiel unten an. Ersetzen Sie Name, ID und gemeinsam verwendeten Schlüssel durch Ihre eigenen Werte. Wichtig ist dabei, dass der gemeinsam verwendete Schlüssel für beide Verbindungen übereinstimmen muss. Das Erstellen einer Verbindung kann etwas dauern.

  Stellen Sie eine Verbindung mit Abonnement 1 her, bevor Sie das folgende Beispiel ausführen:

  ```powershell
  $vnet5gw = New-Object Microsoft.Azure.Commands.Network.Models.PSVirtualNetworkGateway
  $vnet5gw.Name = "VNet5GW"
  $vnet5gw.Id   = "/subscriptions/66c8e4f1-ecd6-47ed-9de7-7e530de23994/resourceGroups/TestRG5/providers/Microsoft.Network/virtualNetworkGateways/VNet5GW"
  $Connection15 = "VNet1toVNet5"
  New-AzureRmVirtualNetworkGatewayConnection -Name $Connection15 -ResourceGroupName $RG1 -VirtualNetworkGateway1 $vnet1gw -VirtualNetworkGateway2 $vnet5gw -Location $Location1 -ConnectionType Vnet2Vnet -SharedKey 'AzureA1b2C3'
  ```
4. **[Abonnement 5]** Erstellen Sie die Verbindung zwischen „TestVNet5“ und „TestVNet1“. Dieser Schritt ähnelt dem obigen Schritt, aber hier erstellen Sie jetzt die Verbindung von TestVNet5 mit TestVNet1. Auch hier gilt der gleiche Prozess zur Erstellung eines PowerShell-Objekts basierend auf den Werten, die aus Abonnement 1 abgerufen wurden. Stellen Sie in diesem Schritt sicher, dass die freigegebenen Schlüssel übereinstimmen.

  Stellen Sie eine Verbindung mit Abonnement 5 her, bevor Sie das folgende Beispiel ausführen:

  ```powershell
  $vnet1gw = New-Object Microsoft.Azure.Commands.Network.Models.PSVirtualNetworkGateway
  $vnet1gw.Name = "VNet1GW"
  $vnet1gw.Id = "/subscriptions/b636ca99-6f88-4df4-a7c3-2f8dc4545509/resourceGroups/TestRG1/providers/Microsoft.Network/virtualNetworkGateways/VNet1GW "
  New-AzureRmVirtualNetworkGatewayConnection -Name $Connection51 -ResourceGroupName $RG5 -VirtualNetworkGateway1 $vnet5gw -VirtualNetworkGateway2 $vnet1gw -Location $Location5 -ConnectionType Vnet2Vnet -SharedKey 'AzureA1b2C3'
  ```

## <a name="verify"></a>Überprüfen einer Verbindung

[!INCLUDE [vpn-gateway-no-nsg-include](../../includes/vpn-gateway-no-nsg-include.md)]

[!INCLUDE [verify connections powershell](../../includes/vpn-gateway-verify-connection-ps-rm-include.md)]

## <a name="faq"></a>Häufig gestellte Fragen zu VNet-zu-VNet-Verbindungen

[!INCLUDE [vpn-gateway-vnet-vnet-faq](../../includes/vpn-gateway-vnet-vnet-faq-include.md)]

## <a name="next-steps"></a>Nächste Schritte

* Sobald die Verbindung hergestellt ist, können Sie Ihren virtuellen Netzwerken virtuelle Computer hinzufügen. Weitere Informationen finden Sie unter [Dokumentation zu Virtual Machines](https://docs.microsoft.com/azure/#pivot=services&panel=Compute).
* Informationen zu BGP finden Sie in der [Übersicht über BGP](vpn-gateway-bgp-overview.md) und unter [Konfigurieren von BGP](vpn-gateway-bgp-resource-manager-ps.md).
