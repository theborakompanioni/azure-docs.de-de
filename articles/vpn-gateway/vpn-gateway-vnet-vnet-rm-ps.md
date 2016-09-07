<properties
   pageTitle="Verbinden von Azure-VNets mit VPN Gateway und PowerShell | Microsoft Azure"
   description="Dieser Artikel führt Sie durch das Verbinden virtueller Netzwerke mithilfe von Azure-Ressourcen-Manager und PowerShell."
   services="vpn-gateway"
   documentationCenter="na"
   authors="cherylmc"
   manager="carmonm"
   editor=""
   tags="azure-resource-manager"/>

<tags
   ms.service="vpn-gateway"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="08/31/2016"
   ms.author="cherylmc"/>

# Konfigurieren einer VNet-zu-VNet-Verbindung für Resource Manager mithilfe von PowerShell

> [AZURE.SELECTOR]
- [Klassisches Azure-Portal](virtual-networks-configure-vnet-to-vnet-connection.md)
- [PowerShell – Resource Manager](vpn-gateway-vnet-vnet-rm-ps.md)

In diesem Artikel wird Schritt für Schritt erläutert, wie Sie mit VPN Gateway eine Verbindung zwischen VNets im Resource Manager-Bereitstellungsmodell erstellen. Die virtuellen Netzwerke können sich in derselben oder in unterschiedlichen Regionen befinden und aus demselben oder unterschiedlichen Abonnements stammen.


![v2v-Diagramm](./media/vpn-gateway-vnet-vnet-rm-ps/v2vrmps.png)


### Bereitstellungsmodelle und Tools für VNet-zu-VNet


[AZURE.INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-classic-rm-include.md)]

Eine VNET-zu-VNET-Verbindung kann in beiden Bereitstellungsmodellen und mit unterschiedlichen Tools konfiguriert werden. Ausführlichere Informationen finden Sie in der Tabelle weiter unten. Wir aktualisieren diese Tabelle, wenn neue Artikel, neue Bereitstellungsmodelle und weitere Tools für diese Konfiguration verfügbar werden. Wenn ein Artikel verfügbar ist, fügen wir in der Tabelle einen direkten Link dazu ein.

[AZURE.INCLUDE [vpn-gateway-table-vnet-vnet](../../includes/vpn-gateway-table-vnet-to-vnet-include.md)]


#### VNet-Peering

Unter Umständen können Sie das VNet-Peering verwenden, um Ihre Verbindung zu erstellen, solange die Konfiguration Ihres virtuellen Netzwerks bestimmte Anforderungen erfüllt. Beim VNet-Peering wird kein Gateway für das virtuelle Netzwerk verwendet. Das [VNet-Peering](../virtual-network/virtual-network-peering-overview.md) befindet sich derzeit in der Vorschauphase.


## Über VNet-zu-VNet-Verbindungen

Das Verbinden eines virtuellen Netzwerks mit einem anderen virtuellen Netzwerk (VNet-zu-VNet) ähnelt dem Verbinden eines VNet mit einem lokalen Standort. Beide Verbindungstypen verwenden ein Azure VPN Gateway, um einen sicheren Tunnel mit IPSec/IKE bereitzustellen. Die VNets, die Sie verbinden, können sich in verschiedenen Regionen befinden. Sie können sich auch in verschiedenen Abonnements befinden. Sie können sogar VNet-zu-VNet-Kommunikation mit Konfigurationen für mehrere Standorte kombinieren. Auf diese Weise können Sie Netzwerktopologien einrichten, die wie in der folgenden Abbildung dargestellt standortübergreifende Konnektivität mit Konnektivität zwischen virtuellen Netzwerken kombinieren.


![Informationen zu Verbindungen](./media/vpn-gateway-vnet-vnet-rm-ps/aboutconnections.png)
 
### Gründe für Verbindungen zwischen virtuellen Netzwerken

Aus den folgenden Gründen empfiehlt sich das Herstellen von Verbindungen zwischen virtuellen Netzwerken:

- **Regionsübergreifende Georedundanz und Geopräsenz**
	- Sie können Ihre eigene Georeplikation oder -synchronisierung mit sicheren Verbindungen einrichten, ohne Endpunkte mit Internetzugriff verwenden zu müssen.
	- Mit Azure Traffic Manager und Load Balancer können Sie eine hoch verfügbare Workload mit Georedundanz über mehrere Azure-Regionen hinweg einrichten. Ein wichtiges Beispiel ist die Einrichtung von SQL Always On mit Verfügbarkeitsgruppen, die sich über mehrere Azure-Regionen erstrecken.

- **Regionale Anwendungen mit mehreren Ebenen und Isolation oder Verwaltungsgrenze**
	- In derselben Region können Sie Anwendungen mit mehreren Ebenen und mehreren virtuellen Netzwerken einrichten, die aufgrund von Isolations- oder Verwaltungsanforderungen miteinander verbunden sind.


### FAQs zu VNet-zu-VNet

[AZURE.INCLUDE [vpn-gateway-vnet-vnet-faq](../../includes/vpn-gateway-vnet-vnet-faq-include.md)]


## Welche Schritte soll ich ausführen?

In diesem Artikel finden Sie zwei unterschiedliche Anleitungen: eine Reihe von Schritten für [VNets, die sich in demselben Abonnement befinden](#samesub), und eine weitere für [VNets, die sich in verschiedenen Abonnements befinden](#difsub). Der wichtigste Unterschied zwischen den beiden Anleitungen besteht darin, ob Sie alle virtuellen Netzwerk- und Gatewayressourcen innerhalb derselben PowerShell-Sitzung erstellen und konfigurieren können.

In den Schritten in diesem Artikel werden Variablen verwendet, die am Anfang jedes Abschnitts deklariert werden. Wenn Sie bereits mit vorhandenen VNets arbeiten, ändern Sie die Variablen entsprechend den Einstellungen Ihrer eigenen Umgebung.

![Beide Verbindungen](./media/vpn-gateway-vnet-vnet-rm-ps/differentsubscription.png)


## <a name="samesub"></a>Verbinden von VNets in demselben Abonnement

![v2v-Diagramm](./media/vpn-gateway-vnet-vnet-rm-ps/v2vrmps.png)

### Voraussetzungen
	
Bevor Sie beginnen, müssen Sie die PowerShell-Cmdlets für Azure Resource Manager installieren. Weitere Informationen zur Installation der PowerShell-Cmdlets finden Sie unter [Installieren und Konfigurieren von Azure PowerShell](../powershell-install-configure.md).

### <a name="Step1"></a>Schritt 1: Planen der IP-Adressbereiche


In den folgenden Schritten erstellen wir zwei virtuelle Netzwerke sowie die jeweiligen Gatewaysubnetze und Konfigurationen. Anschließend erstellen wir eine VPN-Verbindung zwischen den beiden VNets. Es ist wichtig, die IP-Adressbereiche für Ihre Netzwerkkonfiguration zu planen. Denken Sie daran, dass Sie sicherstellen müssen, dass keiner der VNet-Bereiche oder der Bereiche des lokalen Netzwerks Überschneidungen aufweist.

In den Beispielen werden die folgenden Werte verwendet:

**Werte für TestVNet1:**

- VNet-Name: TestVNet1
- Ressourcengruppe: TestRG1
- Standort: USA, Osten
- TestVNet1: 10.11.0.0/16 & 10.12.0.0/16
- FrontEnd: 10.11.0.0/24
- BackEnd: 10.12.0.0/24
- GatewaySubnet: 10.12.255.0/27
- DNS-Server: 8.8.8.8
- GatewayName: VNet1GW
- Öffentliche IP-Adresse: VNet1GWIP
- VPNType: RouteBased
- Verbindung (1to4): VNet1toVNet4
- Verbindung (1to5): VNet1toVNet5
- ConnectionType: VNet2VNet


**Werte für TestVNet4:**

- VNet-Name: TestVNet4
- TestVNet2: 10.41.0.0/16 & 10.42.0.0/16
- FrontEnd: 10.41.0.0/24
- BackEnd: 10.42.0.0/24
- GatewaySubnet: 10.42.255.0/27
- Ressourcengruppe: TestRG4
- Standort: USA, Westen
- DNS-Server: 8.8.8.8
- GatewayName: VNet4GW
- Öffentliche IP-Adresse: VNet4GWIP
- VPNType: RouteBased
- Verbindung: VNet4toVNet1
- ConnectionType: VNet2VNet



### <a name="Step2"></a>Schritt 2: Erstellen und Konfigurieren von TestVNet1

1. Deklarieren von Variablen

	Deklarieren Sie zunächst die Variablen. Im Beispiel werden die Variablen mit den Werten für diese Übung deklariert. In den meisten Fällen sollten Sie die Werte durch Ihre eigenen Werte ersetzen. Sie können diese Variablen jedoch verwenden, wenn Sie die Schritte nur durchgehen, um sich mit dieser Art von Konfiguration vertraut zu machen. Ändern Sie die Variablen ggf., kopieren Sie sie, und fügen Sie sie in die PowerShell-Konsole ein.

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
		$DNS1 = "8.8.8.8"
		$GWName1 = "VNet1GW"
		$GWIPName1 = "VNet1GWIP"
		$GWIPconfName1 = "gwipconf1"
		$Connection14 = "VNet1toVNet4"
		$Connection15 = "VNet1toVNet5"

2. Verbinden mit Ihrem Abonnement

	Wechseln Sie in den PowerShell-Modus, um die Resource Manager-Cmdlets zu verwenden. Öffnen Sie die PowerShell-Konsole, und stellen Sie eine Verbindung mit Ihrem Konto her. Verwenden Sie das folgenden Beispiel, um eine Verbindung herzustellen:

		Login-AzureRmAccount

	Überprüfen Sie die Abonnements für das Konto.

		Get-AzureRmSubscription 

	Geben Sie das Abonnement an, das Sie verwenden möchten.

		Select-AzureRmSubscription -SubscriptionName $Sub1

3. Erstellen einer neuen Ressourcengruppe

		New-AzureRmResourceGroup -Name $RG1 -Location $Location1

4. Erstellen der Subnetzkonfigurationen für TestVNet1

	In diesem Beispiel wird ein virtuelles Netzwerk mit dem Namen TestVNet1 und drei Subnetzen namens „GatewaySubnet“, „FrontEnd“ und „Backend“ erstellt. Beim Ersetzen der Werte ist es wichtig, dass Sie Ihrem Gatewaysubnetz immer den Namen „GatewaySubnet“ geben. Wenn Sie einen anderen Namen verwenden, tritt beim Erstellen des Gateways ein Fehler auf.

	Im folgenden Beispiel werden die zuvor festgelegten Variablen verwendet. Im Beispiel wird ein Gatewaysubnetz mit einem Subnetz der Größe /27 verwendet. Sie können auch ein Gatewaysubnetz mit einem kleinen Subnetz der Größe /29 erstellen, dies wird jedoch nicht empfohlen. Wir empfehlen die Verwendung eines größeren Subnetzes, z.B. /27 oder /26. Dadurch können Sie vorhandene oder zukünftige Konfigurationen nutzen, die möglicherweise ein größeres Gatewaysubnetz erfordern.

		$fesub1 = New-AzureRmVirtualNetworkSubnetConfig -Name $FESubName1 -AddressPrefix $FESubPrefix1
		$besub1 = New-AzureRmVirtualNetworkSubnetConfig -Name $BESubName1 -AddressPrefix $BESubPrefix1
		$gwsub1 = New-AzureRmVirtualNetworkSubnetConfig -Name $GWSubName1 -AddressPrefix $GWSubPrefix1


5. Erstellen von TestVNet1

		New-AzureRmVirtualNetwork -Name $VNetName1 -ResourceGroupName $RG1 `
		-Location $Location1 -AddressPrefix $VNetPrefix11,$VNetPrefix12 -Subnet $fesub1,$besub1,$gwsub1

6. Anfordern einer öffentlichen IP-Adresse

	Fordern Sie eine öffentliche IP-Adresse an, die dem Gateway zugewiesen wird, das Sie für Ihr VNet erstellen. Beachten Sie, dass die Zuordnungsmethode (AllocationMethod) dynamisch ist. Es ist nicht möglich, die IP-Adresse selbst anzugeben. Sie wird Ihrem Gateway dynamisch zugewiesen.

		$gwpip1 = New-AzureRmPublicIpAddress -Name $GWIPName1 -ResourceGroupName $RG1 `
		-Location $Location1 -AllocationMethod Dynamic

7. Erstellen der Gatewaykonfiguration

	Die Gatewaykonfiguration definiert das zu verwendende Subnetz und die zu verwendende öffentliche IP-Adresse. Verwenden Sie das Beispiel, um Ihre Gatewaykonfiguration zu erstellen.

		$vnet1 = Get-AzureRmVirtualNetwork -Name $VNetName1 -ResourceGroupName $RG1
		$subnet1 = Get-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet1
		$gwipconf1 = New-AzureRmVirtualNetworkGatewayIpConfig -Name $GWIPconfName1 `
		-Subnet $subnet1 -PublicIpAddress $gwpip1

8. Erstellen des Gateways für TestVNet1

	In diesem Schritt erstellen Sie das virtuelle Netzwerkgateway für TestVNet1. VNet-zu-VNet-Konfigurationen erfordern einen routenbasierten VPN-Typ. Das Erstellen eines Gateways kann einige Zeit dauern (45 Minuten oder mehr).

		New-AzureRmVirtualNetworkGateway -Name $GWName1 -ResourceGroupName $RG1 `
		-Location $Location1 -IpConfigurations $gwipconf1 -GatewayType Vpn `
		-VpnType RouteBased -GatewaySku Standard

### Schritt 3: Erstellen und Konfigurieren von TestVNet4

Nachdem Sie TestVNet1 konfiguriert haben, erstellen Sie TestVNet4. Führen Sie die unten angegebenen Schritte aus, und ersetzen Sie die Werte bei Bedarf durch Ihre eigenen Werte. Sie können diesen Schritt in derselben PowerShell-Sitzung ausführen, da hierfür dasselbe Abonnement gilt.

1. Deklarieren von Variablen

	Achten Sie darauf, dass Sie die Werte durch die Werte ersetzen, die Sie für die Konfiguration verwenden möchten.

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
		$DNS4 = "8.8.8.8"
		$GWName4 = "VNet4GW"
		$GWIPName4 = "VNet4GWIP"
		$GWIPconfName4 = "gwipconf4"
		$Connection41 = "VNet4toVNet1"

	Stellen Sie sicher, dass die Verbindung mit Abonnement 1 noch besteht, bevor Sie fortfahren.

2. Erstellen einer neuen Ressourcengruppe

		New-AzureRmResourceGroup -Name $RG4 -Location $Location4

3. Erstellen der Subnetzkonfigurationen für TestVNet4

		$fesub4 = New-AzureRmVirtualNetworkSubnetConfig -Name $FESubName4 -AddressPrefix $FESubPrefix4
		$besub4 = New-AzureRmVirtualNetworkSubnetConfig -Name $BESubName4 -AddressPrefix $BESubPrefix4
		$gwsub4 = New-AzureRmVirtualNetworkSubnetConfig -Name $GWSubName4 -AddressPrefix $GWSubPrefix4

4. Erstellen von TestVNet4

		New-AzureRmVirtualNetwork -Name $VnetName4 -ResourceGroupName $RG4 `
		-Location $Location4 -AddressPrefix $VnetPrefix41,$VnetPrefix42 -Subnet $fesub4,$besub4,$gwsub4

5. Anfordern einer öffentlichen IP-Adresse

		$gwpip4 = New-AzureRmPublicIpAddress -Name $GWIPName4 -ResourceGroupName $RG4 `
		-Location $Location4 -AllocationMethod Dynamic

6. Erstellen der Gatewaykonfiguration

		$vnet4 = Get-AzureRmVirtualNetwork -Name $VnetName4 -ResourceGroupName $RG4
		$subnet4 = Get-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet4
		$gwipconf4 = New-AzureRmVirtualNetworkGatewayIpConfig -Name $GWIPconfName4 -Subnet $subnet4 -PublicIpAddress $gwpip4

7. Erstellen des TestVNet4-Gateways

		New-AzureRmVirtualNetworkGateway -Name $GWName4 -ResourceGroupName $RG4 `
		-Location $Location4 -IpConfigurations $gwipconf4 -GatewayType Vpn `
		-VpnType RouteBased -GatewaySku Standard

### Schritt 4: Verbinden der Gateways

1. Abrufen von beiden Gateways des virtuellen Netzwerks

	Da sich in diesem Beispiel beide Gateways in demselben Abonnement befinden, kann dieser Schritt in derselben PowerShell-Sitzung durchgeführt werden.

		$vnet1gw = Get-AzureRmVirtualNetworkGateway -Name $GWName1 -ResourceGroupName $RG1
		$vnet4gw = Get-AzureRmVirtualNetworkGateway -Name $GWName4 -ResourceGroupName $RG4


2. Erstellen der Verbindung von TestVNet1 mit TestVNet4

	In diesem Schritt erstellen Sie die Verbindung von TestVNet1 mit TestVNet4. In den Beispielen wird auf einen gemeinsam verwendeten Schlüssel verwiesen. Sie können eigene Werte für den gemeinsam verwendeten Schlüssel verwenden. Wichtig ist dabei, dass der gemeinsam verwendete Schlüssel für beide Verbindungen übereinstimmen muss. Das Erstellen einer Verbindung kann etwas dauern.

		New-AzureRmVirtualNetworkGatewayConnection -Name $Connection14 -ResourceGroupName $RG1 `
		-VirtualNetworkGateway1 $vnet1gw -VirtualNetworkGateway2 $vnet4gw -Location $Location1 `
		-ConnectionType Vnet2Vnet -SharedKey 'AzureA1b2C3'

3. Erstellen der Verbindung von TestVNet4 mit TestVNet1

	Dieser Schritt ähnelt dem obigen Schritt, aber hier erstellen Sie jetzt die Verbindung von TestVNet4 mit TestVNet1. Stellen Sie sicher, dass die freigegebenen Schlüssel übereinstimmen.

		New-AzureRmVirtualNetworkGatewayConnection -Name $Connection41 -ResourceGroupName $RG4 `
		-VirtualNetworkGateway1 $vnet4gw -VirtualNetworkGateway2 $vnet1gw -Location $Location4 `
		-ConnectionType Vnet2Vnet -SharedKey 'AzureA1b2C3'

	Nach einigen Minuten sollte die Verbindung hergestellt werden.

4. Überprüfen Sie die Verbindung. Informationen hierzu finden Sie im Abschnitt [Überprüfen der Verbindung](#verify).


## <a name="difsub"></a>Verbinden von VNets aus unterschiedlichen Abonnements


![v2v-Diagramm](./media/vpn-gateway-vnet-vnet-rm-ps/v2vdiffsub.png)

In diesem Szenario verbinden wir TestVNet1 und TestVNet5. TestVNet1 und TestVNet5 befinden sich in unterschiedlichen Abonnements. In den Schritten für diese Konfiguration wird eine zusätzliche VNet-zu-VNet-Verbindung hinzugefügt, um TestVNet1 mit TestVNet5 zu verbinden.

Der Unterschied besteht hierbei darin, dass ein Teil der Konfigurationsschritte in einer separaten PowerShell-Sitzung im Kontext des zweiten Abonnements ausgeführt werden muss. Dies gilt insbesondere dann, wenn die beiden Abonnements unterschiedlichen Organisationen gehören.

Die Anleitung ist eine Fortsetzung der obigen Schritte. Sie müssen [Schritt 1](#Step1) und [Schritt 2](#Step2) ausführen, um TestVNet1 und das VPN-Gateway für TestVNet1 zu erstellen und zu konfigurieren. Nachdem Sie die Schritte 1 und 2 ausgeführt haben, fahren Sie mit Schritt 5 fort, um TestVNet5 zu erstellen.

### Schritt 5: Überprüfen der zusätzlichen IP-Adressbereiche

Es ist wichtig sicherzustellen, dass sich der IP-Adressbereich des neuen virtuellen Netzwerks (TestVNet5) nicht mit einem Ihrer VNet-Bereiche oder Gatewaybereiche des lokalen Netzwerks überlappt.

In diesem Beispiel können die virtuellen Netzwerke unterschiedlichen Organisationen gehören. Bei dieser Übung können Sie für TestVNet5 die folgenden Werte verwenden:

**Werte für TestVNet5:**

- VNet-Name: TestVNet5
- Ressourcengruppe: TestRG5
- Standort: Japan, Osten
- TestVNet5: 10.51.0.0/16 & 10.52.0.0/16
- FrontEnd: 10.51.0.0/24
- BackEnd: 10.52.0.0/24
- GatewaySubnet: 10.52.255.0.0/27
- DNS-Server: 8.8.8.8
- GatewayName: VNet5GW
- Öffentliche IP-Adresse: VNet5GWIP
- VPNType: RouteBased
- Verbindung: VNet5toVNet1
- ConnectionType: VNet2VNet

**Weitere Werte für TestVNet1:**

- Verbindung: VNet1toVNet5


### Schritt 6: Erstellen und Konfigurieren von TestVNet5

Dieser Schritt muss im Kontext des neuen Abonnements ausgeführt werden. Dieser Teil kann vom Administrator in einer anderen Organisation ausgeführt werden, in deren Besitz sich das Abonnement befindet.

1. Deklarieren von Variablen

	Achten Sie darauf, dass Sie die Werte durch die Werte ersetzen, die Sie für die Konfiguration verwenden möchten.

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
		$DNS5 = "8.8.8.8"
		$GWName5 = "VNet5GW"
		$GWIPName5 = "VNet5GWIP"
		$GWIPconfName5 = "gwipconf5"
		$Connection51 = "VNet5toVNet1"

2. Verbinden mit Abonnement 5

	Öffnen Sie die PowerShell-Konsole, und stellen Sie eine Verbindung mit Ihrem Konto her. Verwenden Sie das folgende Beispiel, um eine Verbindung herzustellen:

		Login-AzureRmAccount

	Überprüfen Sie die Abonnements für das Konto.

		Get-AzureRmSubscription 

	Geben Sie das Abonnement an, das Sie verwenden möchten.

		Select-AzureRmSubscription -SubscriptionName $Sub5

3. Erstellen einer neuen Ressourcengruppe

		New-AzureRmResourceGroup -Name $RG5 -Location $Location5

4. Erstellen der Subnetzkonfigurationen für TestVNet4
	
		$fesub5 = New-AzureRmVirtualNetworkSubnetConfig -Name $FESubName5 -AddressPrefix $FESubPrefix5
		$besub5 = New-AzureRmVirtualNetworkSubnetConfig -Name $BESubName5 -AddressPrefix $BESubPrefix5
		$gwsub5 = New-AzureRmVirtualNetworkSubnetConfig -Name $GWSubName5 -AddressPrefix $GWSubPrefix5

5. Erstellen von TestVNet5

		New-AzureRmVirtualNetwork -Name $VnetName5 -ResourceGroupName $RG5 -Location $Location5 `
		-AddressPrefix $VnetPrefix51,$VnetPrefix52 -Subnet $fesub5,$besub5,$gwsub5

6. Anfordern einer öffentlichen IP-Adresse

		$gwpip5 = New-AzureRmPublicIpAddress -Name $GWIPName5 -ResourceGroupName $RG5 `
		-Location $Location5 -AllocationMethod Dynamic


7. Erstellen der Gatewaykonfiguration

		$vnet5 = Get-AzureRmVirtualNetwork -Name $VnetName5 -ResourceGroupName $RG5
		$subnet5  = Get-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet5
		$gwipconf5 = New-AzureRmVirtualNetworkGatewayIpConfig -Name $GWIPconfName5 -Subnet $subnet5 -PublicIpAddress $gwpip5

8. Erstellen des TestVNet5-Gateways

		New-AzureRmVirtualNetworkGateway -Name $GWName5 -ResourceGroupName $RG5 -Location $Location5 `
		-IpConfigurations $gwipconf5 -GatewayType Vpn -VpnType RouteBased -GatewaySku Standard

### Schritt 7: Verbinden der Gateways

Da sich die Gateways in diesem Beispiel in unterschiedlichen Abonnements befinden, haben wir diesen Schritt in zwei PowerShell-Sitzungen mit den Bezeichnungen [Abonnement 1] und [Abonnement 5] aufgeteilt.

1. **[Abonnement 1]** Abrufen des virtuellen Netzwerkgateways für Abonnement 1

	Stellen Sie sicher, dass Sie sich bei Abonnement 1 anmelden und die Verbindung mit diesem Abonnement herstellen.

		$vnet1gw = Get-AzureRmVirtualNetworkGateway -Name $GWName1 -ResourceGroupName $RG1

	Kopieren Sie die Ausgabe der folgenden Elemente, und senden Sie diese per E-Mail oder auf anderem Wege an den Administrator von Abonnement 5.

		$vnet1gw.Name
		$vnet1gw.Id

	Diese beiden Elemente weisen Werte auf, die den Werten in der folgenden Beispielausgabe ähneln:

		PS D:> $vnet1gw.Name
		VNet1GW
		PS D:> $vnet1gw.Id
		/subscriptions/b636ca99-6f88-4df4-a7c3-2f8dc4545509/resourceGroupsTestRG1/providers/Microsoft.Network/virtualNetworkGateways/VNet1GW

2. **[Abonnement 5]** Abrufen des virtuellen Netzwerkgateways für Abonnement 5

	Stellen Sie sicher, dass Sie sich bei Abonnement 5 anmelden und die Verbindung mit diesem Abonnement herstellen.

		$vnet5gw = Get-AzureRmVirtualNetworkGateway -Name $GWName5 -ResourceGroupName $RG5

	Kopieren Sie die Ausgabe der folgenden Elemente, und senden Sie diese per E-Mail oder auf anderem Wege an den Administrator von Abonnement 1.

		$vnet5gw.Name
		$vnet5gw.Id

	Diese beiden Elemente weisen Werte auf, die den Werten in der folgenden Beispielausgabe ähneln:

		PS C:\> $vnet5gw.Name
		VNet5GW
		PS C:\> $vnet5gw.Id
		/subscriptions/66c8e4f1-ecd6-47ed-9de7-7e530de23994/resourceGroups/TestRG5/providers/Microsoft.Network/virtualNetworkGateways/VNet5GW

3. **[Abonnement 1]** Erstellen der Verbindung von TestVNet1 mit TestVNet5

	In diesem Schritt erstellen Sie die Verbindung von TestVNet1 mit TestVNet5. Der Unterschied hierbei ist, dass „$vnet5gw“ nicht direkt abgerufen werden kann, da es sich in einem anderen Abonnement befindet. Sie müssen ein neues PowerShell-Objekt mit den Werten erstellen, die in den obigen Schritten aus Abonnement 1 übermittelt wurden. Ersetzen Sie Name, ID und gemeinsam verwendeten Schlüssel durch Ihre eigenen Werte. Wichtig ist dabei, dass der gemeinsam verwendete Schlüssel für beide Verbindungen übereinstimmen muss. Das Erstellen einer Verbindung kann etwas dauern.

	Stellen Sie sicher, dass Sie eine Verbindung mit Abonnement 1 herstellen.
	
		$vnet5gw = New-Object Microsoft.Azure.Commands.Network.Models.PSVirtualNetworkGateway
		$vnet5gw.Name = "VNet5GW"
		$vnet5gw.Id   = "/subscriptions/66c8e4f1-ecd6-47ed-9de7-7e530de23994/resourceGroups/TestRG5/providers/Microsoft.Network/virtualNetworkGateways/VNet5GW"
		$Connection15 = "VNet1toVNet5"
		New-AzureRmVirtualNetworkGatewayConnection -Name $Connection15 -ResourceGroupName $RG1 -VirtualNetworkGateway1 $vnet1gw -VirtualNetworkGateway2 $vnet5gw -Location $Location1 -ConnectionType Vnet2Vnet -SharedKey 'AzureA1b2C3'

4. **[Abonnement 5]** Erstellen der Verbindung von TestVNet5 mit TestVNet1

	Dieser Schritt ähnelt dem obigen Schritt, aber hier erstellen Sie jetzt die Verbindung von TestVNet5 mit TestVNet1. Auch hier gilt der gleiche Prozess zur Erstellung eines PowerShell-Objekts basierend auf den Werten, die aus Abonnement 1 abgerufen wurden. Stellen Sie in diesem Schritt sicher, dass die freigegebenen Schlüssel übereinstimmen.

	Stellen Sie sicher, dass Sie eine Verbindung mit Abonnement 5 herstellen.

		$vnet1gw = New-Object Microsoft.Azure.Commands.Network.Models.PSVirtualNetworkGateway
		$vnet1gw.Name = "VNet1GW"
		$vnet1gw.Id = "/subscriptions/b636ca99-6f88-4df4-a7c3-2f8dc4545509/resourceGroups/TestRG1/providers/Microsoft.Network/virtualNetworkGateways/VNet1GW "
		New-AzureRmVirtualNetworkGatewayConnection -Name $Connection51 -ResourceGroupName $RG5 -VirtualNetworkGateway1 $vnet5gw -VirtualNetworkGateway2 $vnet1gw -Location $Location5 -ConnectionType Vnet2Vnet -SharedKey 'AzureA1b2C3'

## <a name="verify"></a>Überprüfen einer Verbindung

[AZURE.INCLUDE [vpn-gateway-verify-connection-rm](../../includes/vpn-gateway-verify-connection-rm-include.md)]


## Nächste Schritte

- Sobald die Verbindung hergestellt ist, können Sie Ihren virtuellen Netzwerken virtuelle Computer hinzufügen. Für diese Schritte finden Sie Informationen unter [Erstellen eines virtuellen Computers](../virtual-machines/virtual-machines-windows-hero-tutorial.md).
- Informationen zu BGP finden Sie in der [Übersicht über BGP](vpn-gateway-bgp-overview.md) und unter [Konfigurieren von BGP](vpn-gateway-bgp-resource-manager-ps.md).

<!---HONumber=AcomDC_0831_2016-->