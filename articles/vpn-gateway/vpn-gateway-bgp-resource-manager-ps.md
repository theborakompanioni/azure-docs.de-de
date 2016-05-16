<properties
   pageTitle="Konfigurieren von BGP auf Azure VPN Gateways mithilfe von Azure Resource Manager und PowerShell | Microsoft Azure"
   description="Dieser Artikel führt Sie durch die Konfiguration von BGP mit Azure VPN Gateways mithilfe von Azure Resource Manager-Manager und PowerShell."
   services="vpn-gateway"
   documentationCenter="na"
   authors="yushwang"
   manager="rossort"
   editor=""
   tags="azure-resource-manager"/>

<tags
   ms.service="vpn-gateway"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="04/15/2016"
   ms.author="yushwang"/>

# Konfigurieren von BGP auf Azure VPN Gateways mithilfe von Azure Resource Manager und PowerShell

Dieser Artikel beschreibt die Schritte zum Aktivieren von BGP auf einer standortübergreifenden Site-to-Site (S2)-VPN-Verbindung und einer VNet-to-VNet-Verbindung mithilfe des Resource Manager-Bereitstellungsmodells und PowerShell.


**Informationen zu Azure-Bereitstellungsmodellen**

[AZURE.INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-classic-rm-include.md)]

## Informationen zu BGP

BGP ist das standardmäßige Routingprotokoll, mit dem im Internet üblicherweise Routing- und Erreichbarkeitsinformationen zwischen mehren Netzwerken ausgetauscht werden. Azure VPN Gateways und Ihre lokalen VPN-Geräte (so genannte BGP-Peers oder Nachbarn) können über BGP Routen austauschen, die beide Gateways über die Verfügbarkeit und Erreichbarkeit der Präfixe informieren, welche die beteiligten Gateways oder Router durchlaufen. BGP ermöglicht auch Transitrouting zwischen mehreren Netzwerken. Hierzu werden von einem BGP-Gateway ermittelte Routen eines BGP-Peers an alle anderen BGP-Peers weitergegeben.

Unter [Overview of BGP with Azure VPN Gateways](./vpn-gateway-bgp-overview.md) (Übersicht über BGP mit Azure VPN Gateways) finden Sie weitere Informationen zu den Vorteilen von BGP sowie zu den technischen Anforderungen und den Überlegungen in Bezug auf die Verwendung von BGP.

## Erste Schritte mit BGP auf Azure VPN Gateways

In diesem Artikel lernen Sie die Schritte kennen, mit denen Sie die folgenden Aufgaben ausführen können:

- [Teil 1: Aktivieren von BGP auf Ihrem Azure VPN Gateway](#enablebgp)

- [Teil 2: Herstellen einer standortübergreifenden Verbindung mit BGP](#crossprembgp)

- [Teil 3: Herstellen einer VNet-to-VNet-Verbindung mit BGP](#v2vbgp)

Jeder Teil der Anweisungen ist ein zentraler Baustein zum Aktivieren von BGP in Ihrer Netzwerkverbindung. Wenn Sie alle drei Teile abgeschlossen haben, erstellen Sie die Topologie wie im folgenden Diagramm:

![BGP-Topologie](./media/vpn-gateway-bgp-resource-manager-ps/bgp-crosspremv2v.png)

Zum Erstellen eines komplexeren Multihop-Übertragungsnetzwerk, das Ihren Anforderungen entspricht, können Sie diese auch kombinieren.

## <a name ="enablebgp"></a>Teil 1: Konfigurieren von BGP auf dem Azure VPN Gateway

Die folgenden Konfigurationsschritte richten die BGP-Parameter auf dem Azure VPN Gateway wie im folgenden Diagramm dargestellt ein:

![BGP Gateway](./media/vpn-gateway-bgp-resource-manager-ps/bgp-gateway.png)

### Voraussetzungen

- Stellen Sie sicher, dass Sie über ein Azure-Abonnement verfügen. Wenn Sie noch kein Abonnement haben, können Sie Ihre [MSDN-Abonnentenvorteile](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) aktivieren oder sich für ein [kostenloses Konto](https://azure.microsoft.com/pricing/free-trial/) registrieren.
	
- Sie müssen die PowerShell-Cmdlets für Azure-Ressourcen-Manager installieren. Weitere Informationen zur Installation der PowerShell-Cmdlets finden Sie unter [Installieren und Konfigurieren von Azure PowerShell](../powershell-install-configure.md).

### Schritt 1: Erstellen und Konfigurieren von VNet1 

#### 1\. Deklarieren von Variablen

Bei dieser Übung beginnen wir mit dem Deklarieren der Variablen. Im folgenden Beispiel werden die Variablen mit den Werten für diese Übung deklariert. Achten Sie darauf, dass Sie die Werte durch Ihre eigenen Werte ersetzen, wenn Sie die Konfiguration für die Produktion durchführen. Sie können diese Variablen verwenden, wenn Sie die Schritte durchgehen, um sich mit dieser Art der Konfiguration vertraut zu machen. Ändern Sie die Variablen, kopieren Sie sie, und fügen Sie sie in die PowerShell-Konsole ein.

	$Sub1          = "Replace_With_Your_Subcription_Name"
	$RG1           = "TestBGPRG1"
	$Location1     = "East US"
	$VNetName1     = "TestVNet1"
	$FESubName1    = "FrontEnd"
	$BESubName1    = "Backend"
	$GWSubName1    = "GatewaySubnet"
	$VNetPrefix11  = "10.11.0.0/16"
	$VNetPrefix12  = "10.12.0.0/16"
	$FESubPrefix1  = "10.11.0.0/24"
	$BESubPrefix1  = "10.12.0.0/24"
	$GWSubPrefix1  = "10.12.255.0/27"
	$VNet1ASN      = 65010
	$DNS1          = "8.8.8.8"
	$GWName1       = "VNet1GW"
	$GWIPName1     = "VNet1GWIP"
	$GWIPconfName1 = "gwipconf1"
	$Connection12  = "VNet1toVNet2"
	$Connection15  = "VNet1toSite5"

#### 2\. Herstellen einer Verbindung mit Ihrem Abonnement und Erstellen einer neuen Ressourcengruppe

Stellen Sie sicher, dass Sie in den PowerShell-Modus wechseln, um die Ressourcen-Manager-Cmdlets zu verwenden. Weitere Informationen finden Sie unter [Verwenden von Windows PowerShell mit Resource Manager](../powershell-azure-resource-manager.md).

Öffnen Sie die PowerShell-Konsole, und stellen Sie eine Verbindung mit Ihrem Konto her. Verwenden Sie das folgende Beispiel, um eine Verbindung herzustellen:

	Login-AzureRmAccount
	Select-AzureRmSubscription -SubscriptionName $Sub1
	New-AzureRmResourceGroup -Name $RG1 -Location $Location1

#### 3\. Erstellen von TestVNet1

Im folgenden Beispiel wird ein virtuelles Netzwerk mit dem Namen TestVNet1 und drei Subnetzen erstellt. Diese haben die Namen „GatewaySubnet“, „FrontEnd“ und „Backend“. Beim Ersetzen der Werte ist es wichtig, dass Sie Ihrem Gatewaysubnetz immer den Namen „GatewaySubnet“ geben. Wenn Sie einen anderen Namen verwenden, tritt beim Erstellen des Gateways ein Fehler auf.

	$fesub1 = New-AzureRmVirtualNetworkSubnetConfig -Name $FESubName1 -AddressPrefix $FESubPrefix1
	$besub1 = New-AzureRmVirtualNetworkSubnetConfig -Name $BESubName1 -AddressPrefix $BESubPrefix1
	$gwsub1 = New-AzureRmVirtualNetworkSubnetConfig -Name $GWSubName1 -AddressPrefix $GWSubPrefix1

	New-AzureRmVirtualNetwork -Name $VNetName1 -ResourceGroupName $RG1 -Location $Location1 -AddressPrefix $VNetPrefix11,$VNetPrefix12 -Subnet $fesub1,$besub1,$gwsub1

### Schritt 2: Erstellen des VPN Gateway für TestVNet1 mit BGP-Parameter

#### 1\. Erstellen der IP- und Subnetzkonfigurationen

Fordern Sie eine öffentliche IP-Adresse an, die dem Gateway zugewiesen wird, das Sie für Ihr VNet erstellen. Definieren Sie auch die erforderlichen Subnetz- und IP-Konfigurationen.

	$gwpip1    = New-AzureRmPublicIpAddress -Name $GWIPName1 -ResourceGroupName $RG1 -Location $Location1 -AllocationMethod Dynamic
	
	$vnet1     = Get-AzureRmVirtualNetwork -Name $VNetName1 -ResourceGroupName $RG1
	$subnet1   = Get-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet1
	$gwipconf1 = New-AzureRmVirtualNetworkGatewayIpConfig -Name $GWIPconfName1 -Subnet $subnet1 -PublicIpAddress $gwpip1

#### 2\. Erstellen des VPN Gateway mit der AS-Nummer

Erstellen Sie das virtuelle Netzwerkgateway für Ihr TestVNet1. BGP erfordert ein routenbasiertes VPN Gateway sowie den Additionspameter (-Asn) zum Festlegen der AS-Nummer (ASN) für TestVNet1. Das Erstellen eines Gateways kann einige Zeit dauern (30 Minuten oder mehr).

	New-AzureRmVirtualNetworkGateway -Name $GWName1 -ResourceGroupName $RG1 -Location $Location1 -IpConfigurations $gwipconf1 -GatewayType Vpn -VpnType RouteBased -GatewaySku HighPerformance -Asn $VNet1ASN

#### 3\. Abrufen der Azure-BGP-Peer-IP-Adresse

Nachdem das Gateway erstellt wurde, müssen Sie die BGP-Peer-IP-Adresse auf dem Azure VPN Gateway abrufen. Diese Adresse ist erforderlich, um das Azure VPN Gateway als BGP-Peer für Ihre lokalen VPN-Geräte zu konfigurieren.

	$vnet1gw = Get-AzureRmVirtualNetworkGateway -Name $GWName1 -ResourceGroupName $RG1
	$vnet1gw.BgpSettingsText

Mit dem letzten Befehl werden die entsprechenden BGP-Konfigurationen auf dem Azure VPN Gateway angezeigt, wie etwa:

	$vnet1gw.BgpSettingsText
	{
		"Asn": 65010,
		"BgpPeeringAddress": "10.12.255.30",
		"PeerWeight": 0
	}

Nachdem das Gateway erstellt wurde, können Sie dieses Gateway verwenden, um eine standortübergreifende Verbindung oder eine VNet-to-VNet-Verbindung mit BGP herzustellen. In den folgenden Abschnitten werden die Schritte beschrieben, mit denen Sie die Übung abschließen.

## <a name ="crossprembbgp"></a>Teil 2: Herstellen einer standortübergreifenden Verbindung mit BGP

Um eine standortübergreifende Verbindung herzustellen, müssen Sie ein lokales Netzwerkgateway erstellen, um das lokale VPN-Gerät darzustellen, sowie eine Verbindung, um das Azure VPN Gateway mit dem lokalen Netzwerkgateway zu verbinden. Der Unterschied zwischen den Anweisungen in diesem Artikel besteht in den zusätzlichen Eigenschaften, die zum Festlegen der BGP-Konfigurationsparameter erforderlich sind.

![BGP für übergreifende Standorte](./media/vpn-gateway-bgp-resource-manager-ps/bgp-crossprem.png)

Schließen Sie zunächst [Teil 1](#enablebgp) dieser Übung ab, bevor Sie fortfahren.

### Schritt 1: Erstellen und Konfigurieren des lokalen Netzwerkgateways

#### 1\. Deklarieren von Variablen

In dieser Übung fahren wir mit dem Erstellen der im Diagramm gezeigten Konfiguration fort. Achten Sie darauf, dass Sie die Werte durch die Werte ersetzen, die Sie für die Konfiguration verwenden möchten.

	$RG5           = "TestBGPRG5"
	$Location5     = "East US 2"
	$LNGName5      = "Site5"
	$LNGPrefix50   = "10.52.255.254/32"
	$LNGIP5        = "Your_VPN_Device_IP"
	$LNGASN5       = 65050
	$BGPPeerIP5    = "10.52.255.254"

In Bezug auf die lokalen Netzwerkgateway-Parameter sind ein paar Dinge zu beachten:

- Der Speicherort und die Ressourcengruppe für das lokale Netzwerkgateway und für das VPN Gateway können gleich oder auch unterschiedlich sein. Dieses Beispiel zeigt sie in verschiedenen Ressourcengruppen und mit verschiedenen Speicherorten.

- Das Mindestpräfix, das Sie für das lokale Netzwerkgateway deklarieren müssen, ist die Hostadresse Ihrer BGP-Peer-IP-Adresse auf Ihrem VPN-Gerät. In diesem Fall ist es ein /32-Präfix von „10.52.255.254/32“.

- Zur Erinnerung: Sie müssen zwischen Ihren lokalen Netzwerken und Azure VNet unterschiedliche BGP-ASNs verwenden. Wenn sie gleich sind, müssen Sie Ihre VNet-ASN ändern, falls Ihr lokales VPN-Gerät bereits die ASN für eine Peerverbindung mit anderen BGP-Nachbarn verwendet.
	
Stellen Sie vor dem Fortfahren sicher, dass die Verbindung mit Abonnement 1 noch besteht.

#### 2\. Erstellen des lokalen Netzwerkgateways für Site5

Erstellen Sie vor dem lokalen Netzwerkgateway zunächst die Ressourcengruppe (sofern noch nicht geschehen). Beachten Sie die zwei zusätzlichen Parameter für das lokale Netzwerkgateway: Asn und BgpPeerAddress.

	New-AzureRmResourceGroup -Name $RG5 -Location $Location5

	New-AzureRmLocalNetworkGateway -Name $LNGName5 -ResourceGroupName $RG5 -Location $Location5 -GatewayIpAddress $LNGIP5 -AddressPrefix $LNGPrefix50 -Asn $LNGASN5 -BgpPeeringAddress $BGPPeerIP5

### Schritt 2: Verbinden des VNet Gateways und des lokalen Netzwerkgateways

#### 1\. Abrufen der beiden Gateways

		$vnet1gw = Get-AzureRmVirtualNetworkGateway -Name $GWName1  -ResourceGroupName $RG1
		$lng5gw  = Get-AzureRmVirtualNetworkGateway -Name $LNGName5 -ResourceGroupName $RG5

#### 2\. Erstellen der Verbindung von TestVNet1 zu Site5

In diesem Schritt erstellen Sie die Verbindung von TestVNet1 zu Site5. Legen Sie „-EnableBGP True“ fest, um BGP für diese Verbindung zu aktivieren. Wie bereits erwähnt, sind für das gleiche Azure VPN Gateway BGP und Nicht-BGP-Verbindungen möglich. Sofern BGP in der Verbindungseigenschaft nicht aktiviert ist, wird BGP für diese Verbindung von Azure nicht aktiviert, auch nicht, wenn BGP-Parameter bereits für beide Gateways konfiguriert wurden.

	New-AzureRmVirtualNetworkGatewayConnection -Name $Connection15 -ResourceGroupName $RG1 -VirtualNetworkGateway1 $vnet1gw -LocalNetworkGateway2 $lng5gw -Location $Location1 -ConnectionType IPsec -SharedKey 'AzureA1b2C3' -EnableBGP True


Das folgende Beispiel listet die Parameter auf, die Sie in den BGP-Konfigurationsbereich auf dem lokalen VPN-Gerät für diese Übung eingeben:

	- Site5 ASN            : 65050
	- Site5 BGP IP         : 10.52.255.254
	- Prefixes to announce : (for example) 10.51.0.0/16 and 10.52.0.0/16
	- Azure VNet ASN       : 65010
	- Azure VNet BGP IP    : 10.12.255.30
	- Static route         : Add a route for 10.12.255.30/32, with nexthop being the VPN tunnel interface on your device
	- eBGP Multihop        : Ensure the "multihop" option for eBGP is enabled on your device if needed

Die Verbindung sollte nach einigen Minuten hergestellt sein, und die BGP-Peeringsitzung wird gestartet, sobald die IPsec-Verbindung hergestellt wurde.
 
## <a name ="v2vbgp"></a>Teil 3: Herstellen einer VNet-to-VNet-Verbindung mit BGP

In diesem Abschnitt wird eine VNet-zu-VNet-Verbindung mit BGP hinzugefügt, wie im folgenden Diagramm dargestellt.

![BGP für VNet-to-VNet](./media/vpn-gateway-bgp-resource-manager-ps/bgp-vnet2vnet.png)

Die Anleitung unten ist eine Fortsetzung der vorherigen obigen Schritte. Sie müssen [Teil I](#enablebgp) ausführen, um TestVNet1 und das VPN Gateway mit BGP zu erstellen und zu konfigurieren.

### Schritt 1: Erstellen von TestVNet2 und des VPN Gateways

Es ist wichtig sicherzustellen, dass sich der IP-Adressbereich des neuen virtuellen Netzwerks (TestVNet2) nicht mit einem Ihrer VNet-Bereiche überschneidet.

In diesem Beispiel gehören die virtuellen Netzwerke zum gleichen Abonnement. Sie können VNet-zu-VNet-Verbindungen zwischen verschiedenen Abonnements einrichten. Informationen dazu finden Sie unter [Konfigurieren einer VNet-zu-VNet-Verbindung mit Azure Resource Manager und PowerShell](./vpn-gateway-vnet-vnet-rm-ps.md). Fügen Sie beim Erstellen der Verbindungen zum Aktivieren von BGP „-EnableBgp True“ hinzu.

#### 1\. Deklarieren von Variablen

Achten Sie darauf, dass Sie die Werte durch die Werte ersetzen, die Sie für die Konfiguration verwenden möchten.

	$RG2           = "TestBGPRG2"
	$Location2     = "West US"
	$VNetName2     = "TestVNet2"
	$FESubName2    = "FrontEnd"
	$BESubName2    = "Backend"
	$GWSubName2    = "GatewaySubnet"
	$VNetPrefix21  = "10.21.0.0/16"
	$VNetPrefix22  = "10.22.0.0/16"
	$FESubPrefix2  = "10.21.0.0/24"
	$BESubPrefix2  = "10.22.0.0/24"
	$GWSubPrefix2  = "10.22.255.0/27"
	$VNet2ASN      = 65020
	$DNS2          = "8.8.8.8"
	$GWName2       = "VNet2GW"
	$GWIPName2     = "VNet2GWIP"
	$GWIPconfName2 = "gwipconf2"
	$Connection21  = "VNet2toVNet1"
	$Connection12  = "VNet1toVNet2"

#### 2\. Erstellen von TestVNet2 in der neuen Ressourcengruppe

	New-AzureRmResourceGroup -Name $RG2 -Location $Location2
	
	$fesub2 = New-AzureRmVirtualNetworkSubnetConfig -Name $FESubName2 -AddressPrefix $FESubPrefix2
	$besub2 = New-AzureRmVirtualNetworkSubnetConfig -Name $BESubName2 -AddressPrefix $BESubPrefix2
	$gwsub2 = New-AzureRmVirtualNetworkSubnetConfig -Name $GWSubName2 -AddressPrefix $GWSubPrefix2

	New-AzureRmVirtualNetwork -Name $VNetName2 -ResourceGroupName $RG2 -Location $Location2 -AddressPrefix $VNetPrefix21,$VNetPrefix22 -Subnet $fesub2,$besub2,$gwsub2

#### 3\. Erstellen des VPN Gateways für TestVNet2 mit BGP-Parametern

Fordern Sie eine öffentliche IP-Adresse an, die dem Gateway zugewiesen wird, das Sie für Ihr VNet erstellen. Definieren Sie auch die erforderlichen Subnetz- und IP-Konfigurationen.

	$gwpip2    = New-AzureRmPublicIpAddress -Name $GWIPName2 -ResourceGroupName $RG2 -Location $Location2 -AllocationMethod Dynamic

	$vnet2     = Get-AzureRmVirtualNetwork -Name $VNetName2 -ResourceGroupName $RG2
	$subnet2   = Get-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet2
	$gwipconf2 = New-AzureRmVirtualNetworkGatewayIpConfig -Name $GWIPconfName2 -Subnet $subnet2 -PublicIpAddress $gwpip2

Erstellen Sie das VPN Gateway mit der AS-Nummer. Beachten Sie, dass Sie die Standard-ASN auf Ihren Azure VPN Gateways überschreiben müssen. Die ASNs für die verbundenen VNets müssen für die Aktivierung von BGP und Transitrouting unterschiedlich sein.

	New-AzureRmVirtualNetworkGateway -Name $GWName2 -ResourceGroupName $RG2 -Location $Location2 -IpConfigurations $gwipconf2 -GatewayType Vpn -VpnType RouteBased -GatewaySku Standard -Asn $VNet2ASN

### Schritt 2: Verbinden der TestVNet1 und TestVNet2-Gateways

In diesem Beispiel befinden sich beide Gateways im selben Abonnement. Sie können diesen Schritt in der gleichen PowerShell-Sitzung ausführen.

#### 1\. Abrufen der beiden Gateways

Stellen Sie sicher, dass Sie sich an „Abonnement 1“ anmelden und die Verbindung damit herstellen.

	$vnet1gw = Get-AzureRmVirtualNetworkGateway -Name $GWName1 -ResourceGroupName $RG1
	$vnet2gw = Get-AzureRmLocalNetworkGateway -Name $GWName2 -ResourceGroupName $RG2
	
#### 2\. Erstellen der beiden Verbindungen

In diesem Schritt erstellen Sie die Verbindung von TestVNet1 zu TestVNet2 sowie die Verbindung von TestVNet2 zu TestVNet1.

	New-AzureRmVirtualNetworkGatewayConnection -Name $Connection12 -ResourceGroupName $RG1 -VirtualNetworkGateway1 $vnet1gw -VirtualNetworkGateway2 $vnet2gw -Location $Location1 -ConnectionType Vnet2Vnet -SharedKey 'AzureA1b2C3' -EnableBgp True

	New-AzureRmVirtualNetworkGatewayConnection -Name $Connection21 -ResourceGroupName $RG2 -VirtualNetworkGateway1 $vnet2gw -VirtualNetworkGateway2 $vnet1gw -Location $Location2 -ConnectionType Vnet2Vnet -SharedKey 'AzureA1b2C3' -EnableBgp True

>[AZURE.IMPORTANT] Aktivieren Sie BGP für BEIDE Verbindungen.

Danach wird die Verbindung innerhalb weniger Minuten hergestellt, und die BGP-Peeringsitzung ist eingerichtet, sobald die VNet-to-VNet-Verbindung fertiggestellt ist.

Wenn Sie alle drei Teile dieser Übung abgeschlossen haben, haben Sie eine Netzwerktopologie wie unten dargestellt eingerichtet:

![BGP für VNet-to-VNet](./media/vpn-gateway-bgp-resource-manager-ps/bgp-crosspremv2v.png)

## Nächste Schritte

Sobald die Verbindung hergestellt ist, können Sie Ihren virtuellen Netzwerken virtuelle Computer hinzufügen. Für diese Schritte finden Sie Informationen unter [Erstellen eines virtuellen Computers](../virtual-machines/virtual-machines-windows-hero-tutorial.md).

<!---HONumber=AcomDC_0504_2016-->