<properties
   pageTitle="Konfigurieren von Aktiv/Aktiv-S2S-VPN-Verbindungen mit Azure VPN Gateways und Azure Resource Manager und PowerShell | Microsoft Azure"
   description="In diesem Artikel wird die Konfiguration von Aktiv/Aktiv-Verbindungen mit Azure VPN Gateways mithilfe von Azure Resource Manager und PowerShell beschrieben."
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
   ms.date="09/26/2016"
   ms.author="yushwang"/>

# Konfigurieren von Aktiv/Aktiv-S2S-VPN-Verbindungen mit Azure VPN Gateways und Azure Resource Manager und PowerShell

In diesem Artikel werden die Schritte zur Herstellung von Aktiv/Aktiv-Verbindungen (lokal und VNet zu VNet) mit dem Resource Manager-Bereitstellungsmodell und PowerShell beschrieben.


**Informationen zu Azure-Bereitstellungsmodellen**

[AZURE.INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-classic-rm-include.md)]

## Informationen zu standortübergreifenden Verbindungen mit hoher Verfügbarkeit

Um für lokale und VNet-zu-VNet-Verbindungen eine hohe Verfügbarkeit zu erzielen, sollten Sie mehrere VPN-Gateways bereitstellen und mehrere parallele Verbindungen zwischen Ihren Netzwerken und Azure herstellen. Eine Übersicht über Verbindungsoptionen und die Topologie finden Sie unter [Standortübergreifende Verbindungen und VNet-zu-VNet-Verbindungen mit hoher Verfügbarkeit](./vpn-gateway-highlyavailable.md).

Dieser Artikel enthält die Anleitung zum Einrichten einer standortübergreifenden Aktiv/Aktiv-VPN-Verbindung und einer Aktiv/Aktiv-Verbindung zwischen zwei virtuellen Netzwerken:

- [Teil 1: Erstellen und Konfigurieren des Azure VPN Gateway im Aktiv/Aktiv-Modus](#aagateway)

- [Teil 2: Herstellen von standortübergreifenden Aktiv/Aktiv-Verbindungen](#aacrossprem)

- [Teil 3: Herstellen von Aktiv/Aktiv-VNet-zu-VNet-Verbindungen](#aav2v)

- [Teil 4: Aktualisieren des vorhandenen Gateways zwischen Aktiv/Aktiv und Aktiv/Standby](#aaupdate)

Sie können dies kombinieren, um eine komplexere Netzwerktopologie mit hoher Verfügbarkeit zu erstellen, die Ihre Anforderungen erfüllt.

>[AZURE.IMPORTANT] Beachten Sie, dass der Aktiv/Aktiv-Modus nur für eine HighPerformance-SKU funktioniert.


## <a name ="aagateway"></a>Teil 1: Erstellen und Konfigurieren von Aktiv/Aktiv-VPN-Gateways

Mit den folgenden Schritten wird Ihr Azure VPN Gateway in Aktiv/Aktiv-Modi konfiguriert. Die wichtigsten Unterschiede zwischen Aktiv/Aktiv- und Aktiv/Standby-Gateways sind:

- Sie müssen zwei Gateway-IP-Konfigurationen mit zwei öffentlichen IP-Adressen erstellen.
- Sie müssen das EnableActiveActiveFeature-Flag festlegen.
- Die Gateway-SKU muss den Typ „HighPerformance“ aufweisen.

Die anderen Eigenschaften sind mit den Eigenschaften von anderen Gateways identisch, die nicht den Aktiv/Aktiv-Modus aufweisen.

### Voraussetzungen

- Stellen Sie sicher, dass Sie über ein Azure-Abonnement verfügen. Wenn Sie noch kein Abonnement haben, können Sie Ihre [MSDN-Abonnentenvorteile](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) aktivieren oder sich für ein [kostenloses Konto](https://azure.microsoft.com/pricing/free-trial/) registrieren.
	
- Sie müssen die PowerShell-Cmdlets für Azure-Ressourcen-Manager installieren. Weitere Informationen zur Installation der PowerShell-Cmdlets finden Sie unter [Installieren und Konfigurieren von Azure PowerShell](../powershell-install-configure.md).

### Schritt 1: Erstellen und Konfigurieren von VNet1

#### 1\. Deklarieren von Variablen

Bei dieser Übung beginnen wir mit dem Deklarieren der Variablen. Im folgenden Beispiel werden die Variablen mit den Werten für diese Übung deklariert. Achten Sie darauf, dass Sie die Werte durch Ihre eigenen Werte ersetzen, wenn Sie die Konfiguration für die Produktion durchführen. Sie können diese Variablen verwenden, wenn Sie die Schritte durchgehen, um sich mit dieser Art der Konfiguration vertraut zu machen. Ändern Sie die Variablen, kopieren Sie sie, und fügen Sie sie in die PowerShell-Konsole ein.

	$Sub1          = "Ross"
	$RG1           = "TestAARG1"
	$Location1     = "West US"
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
	$GW1IPName1    = "VNet1GWIP1"
	$GW1IPName2    = "VNet1GWIP2"
	$GW1IPconf1    = "gw1ipconf1"
	$GW1IPconf2    = "gw1ipconf2"
	$Connection12  = "VNet1toVNet2"
	$Connection151 = "VNet1toSite5_1"
	$Connection152 = "VNet1toSite5_2"

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

### Schritt 2: Erstellen des VPN-Gateways für TestVNet1 mit Aktiv/Aktiv-Modus

#### 1\. Erstellen der öffentlichen IP-Adressen und Gateway-IP-Konfigurationen

Fordern Sie zwei öffentliche IP-Adressen zur Zuordnung zu dem Gateway an, das Sie für Ihr VNet erstellen. Definieren Sie auch die erforderlichen Subnetz- und IP-Konfigurationen.

	$gw1pip1    = New-AzureRmPublicIpAddress -Name $GW1IPName1 -ResourceGroupName $RG1 -Location $Location1 -AllocationMethod Dynamic
	$gw1pip2    = New-AzureRmPublicIpAddress -Name $GW1IPName2 -ResourceGroupName $RG1 -Location $Location1 -AllocationMethod Dynamic

	$vnet1      = Get-AzureRmVirtualNetwork -Name $VNetName1 -ResourceGroupName $RG1
	$subnet1    = Get-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet1
	$gw1ipconf1 = New-AzureRmVirtualNetworkGatewayIpConfig -Name $GW1IPconf1 -Subnet $subnet1 -PublicIpAddress $gw1pip1
	$gw1ipconf2 = New-AzureRmVirtualNetworkGatewayIpConfig -Name $GW1IPconf2 -Subnet $subnet1 -PublicIpAddress $gw1pip2

#### 2\. Erstellen des VPN-Gateways mit Aktiv/Aktiv-Konfiguration

Erstellen Sie das virtuelle Netzwerkgateway für Ihr TestVNet1. Beachten Sie, dass zwei GatewayIpConfig-Einträge vorhanden sind und das EnableActiveActiveFeature-Flag festgelegt wurde. Für den Aktiv/Aktiv-Modus ist ein routenbasiertes VPN-Gateway mit einer HighPerformance-SKU erforderlich. Das Erstellen eines Gateways kann einige Zeit dauern (30 Minuten oder mehr).

	New-AzureRmVirtualNetworkGateway -Name $GWName1 -ResourceGroupName $RG1 -Location $Location1 -IpConfigurations $gw1ipconf1,$gw1ipconf2 -GatewayType Vpn -VpnType RouteBased -GatewaySku HighPerformance -Asn $VNet1ASN -EnableActiveActiveFeature -Debug

#### 3\. Abrufen der öffentlichen IP-Adressen für das Gateway und der BGP-Peer-IP-Adresse

Nachdem das Gateway erstellt wurde, müssen Sie die BGP-Peer-IP-Adresse auf dem Azure VPN Gateway abrufen. Diese Adresse ist erforderlich, um das Azure VPN Gateway als BGP-Peer für Ihre lokalen VPN-Geräte zu konfigurieren.

	$gw1pip1 = Get-AzureRmPublicIpAddress -Name $GW1IPName1 -ResourceGroupName $RG1
	$gw1pip2 = Get-AzureRmPublicIpAddress -Name $GW1IPName2 -ResourceGroupName $RG1
	$vnet1gw = Get-AzureRmVirtualNetworkGateway -Name $GWName1 -ResourceGroupName $RG1

Verwenden Sie die folgenden Cmdlets zum Anzeigen der beiden öffentlichen IP-Adressen, die für Ihr VPN-Gateway zugeordnet wurden, und der entsprechenden BGP-Peer-IP-Adressen für jede Gatewayinstanz:

	PS D:> $gw1pip1.IpAddress
	40.112.190.5

	PS D:> $gw1pip1.IpAddress
	138.91.156.129

	PS D:> $vnet1gw.BgpSettingsText
	{
	  "Asn": 65010,
	  "BgpPeeringAddress": "10.12.255.4,10.12.255.5",
	  "PeerWeight": 0
	}

Die Reihenfolge der öffentlichen IP-Adressen für die Gatewayinstanzen und die entsprechenden BGP-Peering-Adressen ist identisch. In diesem Beispiel wird für die Gateway-VM mit der öffentlichen IP-Adresse 40.112.190.5 die Adresse 10.12.255.4 als BGP-Peering-Adresse verwendet, und für das Gateway mit der IP-Adresse 138.91.156.129 wird die Adresse 10.12.255.5 verwendet. Diese Informationen werden benötigt, wenn Sie Ihre lokalen VPN-Geräte für die Verbindung mit dem Aktiv/Aktiv-Gateway einrichten. Das Gateway wird unten im Diagramm mit allen Adressen angezeigt:

![Aktiv/Aktiv-Gateway](./media/vpn-gateway-activeactive-rm-powershell/active-active-gw.png)

Nachdem das Gateway erstellt wurde, können Sie es verwenden, um eine standortübergreifende Aktiv/Aktiv-Verbindung oder eine VNet-zu-VNet-Verbindung herzustellen. In den folgenden Abschnitten werden die Schritte beschrieben, mit denen Sie die Übung abschließen.


## <a name ="aacrossprem"></a>Teil 2: Herstellen einer standortübergreifenden Aktiv/Aktiv-Verbindung

Um eine standortübergreifende Verbindung herzustellen, müssen Sie ein lokales Netzwerkgateway erstellen, um das lokale VPN-Gerät darzustellen, sowie eine Verbindung, um das Azure VPN Gateway mit dem lokalen Netzwerkgateway zu verbinden. In diesem Beispiel befindet sich das Azure VPN Gateway im Aktiv/Aktiv-Modus. Auch wenn nur ein lokales VPN-Gerät (lokales Netzwerkgateway) und eine Verbindungsressource vorhanden sind, richten daher beide Azure VPN Gateway-Instanzen S2S-VPN-Tunnel zum lokalen Gerät ein.

Schließen Sie zunächst [Teil 1](#aagateway) dieser Übung ab, bevor Sie fortfahren.

### Schritt 1: Erstellen und Konfigurieren des lokalen Netzwerkgateways

#### 1\. Deklarieren von Variablen

In dieser Übung fahren wir mit dem Erstellen der im Diagramm gezeigten Konfiguration fort. Achten Sie darauf, dass Sie die Werte durch die Werte ersetzen, die Sie für die Konfiguration verwenden möchten.

	$RG5           = "TestAARG5"
	$Location5     = "West US"
	$LNGName51     = "Site5_1"
	$LNGPrefix51   = "10.52.255.253/32"
	$LNGIP51       = "131.107.72.22"
	$LNGASN5       = 65050
	$BGPPeerIP51   = "10.52.255.253"

In Bezug auf die lokalen Netzwerkgateway-Parameter sind ein paar Dinge zu beachten:

- Der Speicherort und die Ressourcengruppe für das lokale Netzwerkgateway und für das VPN Gateway können gleich oder auch unterschiedlich sein. In diesem Beispiel werden sie in unterschiedlichen Ressourcengruppen angezeigt, aber an demselben Azure-Standort.

- Falls wie oben dargestellt nur ein lokales VPN-Gerät vorhanden ist, kann die Aktiv/Aktiv-Verbindung mit oder ohne BGP-Protokoll funktionieren. In diesem Beispiel wird BGP für die lokale Verbindung verwendet.

- Bei Aktivierung von BGP müssen Sie als Präfix für das lokale Netzwerkgateway die Hostadresse der BGP-Peer-IP-Adresse auf Ihrem VPN-Gerät deklarieren. In diesem Fall ist es das /32-Präfix „10.52.255.253/32“.

- Zur Erinnerung: Sie müssen zwischen Ihren lokalen Netzwerken und Azure VNet unterschiedliche BGP-ASNs verwenden. Wenn sie gleich sind, müssen Sie Ihre VNet-ASN ändern, falls Ihr lokales VPN-Gerät bereits die ASN für eine Peerverbindung mit anderen BGP-Nachbarn verwendet.

#### 2\. Erstellen des lokalen Netzwerkgateways für Site5
	
Stellen Sie vor dem Fortfahren sicher, dass die Verbindung mit Abonnement 1 noch besteht. Erstellen Sie die Ressourcengruppe, falls dies noch nicht durchgeführt wurde.

	New-AzureRmResourceGroup       -Name $RG5 -Location $Location5
	New-AzureRmLocalNetworkGateway -Name $LNGName51 -ResourceGroupName $RG5 -Location $Location5 -GatewayIpAddress $LNGIP51 -AddressPrefix $LNGPrefix51 -Asn $LNGASN5 -BgpPeeringAddress $BGPPeerIP51

### Schritt 2: Verbinden des VNet Gateways und des lokalen Netzwerkgateways

#### 1\. Abrufen der beiden Gateways

	$vnet1gw = Get-AzureRmVirtualNetworkGateway -Name $GWName1  -ResourceGroupName $RG1
	$lng5gw1 = Get-AzureRmLocalNetworkGateway   -Name $LNGName51 -ResourceGroupName $RG5

#### 2\. Erstellen der Verbindung von TestVNet1 zu Site5

In diesem Schritt erstellen Sie die Verbindung von TestVNet1 mit „Site5\_1“ mit der Einstellung „$True“ für „EnableBGP“.

	New-AzureRmVirtualNetworkGatewayConnection -Name $Connection151 -ResourceGroupName $RG1 -VirtualNetworkGateway1 $vnet1gw -LocalNetworkGateway2 $lng5gw1 -Location $Location1 -ConnectionType IPsec -SharedKey 'AzureA1b2C3' -EnableBGP True

#### 3\. VPN- und BGP-Parameter für Ihr lokales VPN-Gerät

Das folgende Beispiel listet die Parameter auf, die Sie in den BGP-Konfigurationsbereich auf dem lokalen VPN-Gerät für diese Übung eingeben:

	- Site5 ASN            : 65050
	- Site5 BGP IP         : 10.52.255.253
	- Prefixes to announce : (for example) 10.51.0.0/16 and 10.52.0.0/16
	- Azure VNet ASN       : 65010
	- Azure VNet BGP IP 1  : 10.12.255.4 for tunnel to 40.112.190.5
	- Azure VNet BGP IP 2  : 10.12.255.5 for tunnel to 138.91.156.129
	- Static routes        : Destination 10.12.255.4/32, nexthop the VPN tunnel interface to 40.112.190.5
	                         Destination 10.12.255.5/32, nexthop the VPN tunnel interface to 138.91.156.129
	- eBGP Multihop        : Ensure the "multihop" option for eBGP is enabled on your device if needed

Die Verbindung sollte nach einigen Minuten hergestellt sein, und die BGP-Peeringsitzung wird gestartet, sobald die IPsec-Verbindung hergestellt wurde. In diesem Beispiel wurde bisher nur ein lokales VPN-Gerät konfiguriert. Dies führt zu folgendem Diagramm:

![Aktiv/Aktiv, standortübergreifend](./media/vpn-gateway-activeactive-rm-powershell/active-active.png)

### Schritt 3: Verbinden von zwei lokalen VPN-Geräten mit dem Aktiv/Aktiv-VPN-Gateway

Wenn Sie über zwei VPN-Geräte in demselben lokalen Netzwerk verfügen, können Sie duale Redundanz erzielen, indem Sie für das Azure VPN Gateway eine Verbindung mit dem zweiten VPN-Gerät herstellen.

#### 1\. Erstellen des zweiten lokalen Netwerkgateways für Site5

Beachten Sie, dass sich die Gateway-IP-Adresse, das Adresspräfix und die BGP-Peering-Adresse für das zweite lokale Netzwerkgateway nicht mit dem vorherigen lokalen Netzwerkgateway für dasselbe lokale Netzwerk überschneiden dürfen.

	$LNGName52     = "Site5_2"
	$LNGPrefix52   = "10.52.255.254/32"
	$LNGIP52       = "131.107.72.23"
	$BGPPeerIP52   = "10.52.255.254"

	New-AzureRmLocalNetworkGateway -Name $LNGName52 -ResourceGroupName $RG5 -Location $Location5 -GatewayIpAddress $LNGIP52 -AddressPrefix $LNGPrefix52 -Asn $LNGASN5 -BgpPeeringAddress $BGPPeerIP52
 
#### 2\. Verbinden des VNet-Gateways mit dem zweiten lokalen Netzwerkgateway

Erstellen Sie die Verbindung von TestVNet1 mit „Site5\_2“ mit der Einstellung „$True“ für „EnableBGP“.

	$lng5gw2 = Get-AzureRmLocalNetworkGateway   -Name $LNGName52 -ResourceGroupName $RG5

	New-AzureRmVirtualNetworkGatewayConnection -Name $Connection152 -ResourceGroupName $RG1 -VirtualNetworkGateway1 $vnet1gw -LocalNetworkGateway2 $lng5gw2 -Location $Location1 -ConnectionType IPsec -SharedKey 'AzureA1b2C3' -EnableBGP True

#### 3\. VPN- und BGP-Parameter für Ihr zweites lokales VPN-Gerät

Unten sind wieder die Parameter aufgeführt, die Sie für das zweite VPN-Gerät eingeben:

	- Site5 ASN            : 65050
	- Site5 BGP IP         : 10.52.255.254
	- Prefixes to announce : (for example) 10.51.0.0/16 and 10.52.0.0/16
	- Azure VNet ASN       : 65010
	- Azure VNet BGP IP 1  : 10.12.255.4 for tunnel to 40.112.190.5
	- Azure VNet BGP IP 2  : 10.12.255.5 for tunnel to 138.91.156.129
	- Static routes        : Destination 10.12.255.4/32, nexthop the VPN tunnel interface to 40.112.190.5
	                         Destination 10.12.255.5/32, nexthop the VPN tunnel interface to 138.91.156.129
	- eBGP Multihop        : Ensure the "multihop" option for eBGP is enabled on your device if needed

Nachdem die Verbindung (Tunnel) hergestellt wurde, verfügen Sie für die Verbindung Ihres lokalen Netzwerks mit Azure über VPN-Geräte und Tunnel mit dualer Redundanz:

![Duale Redundanz, standortübergreifend](./media/vpn-gateway-activeactive-rm-powershell/dual-redundancy.png)


## <a name ="aav2v"></a>Teil 3: Herstellen einer Aktiv/Aktiv-VNet-zu-VNet-Verbindung

In diesem Abschnitt wird eine Aktiv/Aktiv-VNet-zu-VNet-Verbindung mit BGP hergestellt.

Die Anleitung unten ist eine Fortsetzung der vorherigen obigen Schritte. Sie müssen [Teil 1](#aagateway) ausführen, um TestVNet1 und das VPN-Gateway mit BGP zu erstellen und zu konfigurieren.

### Schritt 1: Erstellen von TestVNet2 und des VPN Gateways

Es ist wichtig sicherzustellen, dass sich der IP-Adressbereich des neuen virtuellen Netzwerks (TestVNet2) nicht mit einem Ihrer VNet-Bereiche überschneidet.

In diesem Beispiel gehören die virtuellen Netzwerke zum gleichen Abonnement. Sie können VNet-zu-VNet-Verbindungen zwischen verschiedenen Abonnements einrichten. Informationen dazu finden Sie unter [Konfigurieren einer VNet-zu-VNet-Verbindung mit Azure Resource Manager und PowerShell](./vpn-gateway-vnet-vnet-rm-ps.md). Fügen Sie beim Erstellen der Verbindungen zum Aktivieren von BGP „-EnableBgp $True“ hinzu.

#### 1\. Deklarieren von Variablen

Achten Sie darauf, dass Sie die Werte durch die Werte ersetzen, die Sie für die Konfiguration verwenden möchten.

	$RG2           = "TestAARG2"
	$Location2     = "East US"
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
	$GW2IPName1    = "VNet2GWIP1"
	$GW2IPconf1    = "gw2ipconf1"
	$GW2IPName2    = "VNet2GWIP2"
	$GW2IPconf2    = "gw2ipconf2"
	$Connection21  = "VNet2toVNet1"
	$Connection12  = "VNet1toVNet2"

#### 2\. Erstellen von TestVNet2 in der neuen Ressourcengruppe

	New-AzureRmResourceGroup -Name $RG2 -Location $Location2

	$fesub2 = New-AzureRmVirtualNetworkSubnetConfig -Name $FESubName2 -AddressPrefix $FESubPrefix2
	$besub2 = New-AzureRmVirtualNetworkSubnetConfig -Name $BESubName2 -AddressPrefix $BESubPrefix2
	$gwsub2 = New-AzureRmVirtualNetworkSubnetConfig -Name $GWSubName2 -AddressPrefix $GWSubPrefix2

	New-AzureRmVirtualNetwork -Name $VNetName2 -ResourceGroupName $RG2 -Location $Location2 -AddressPrefix $VNetPrefix21,$VNetPrefix22 -Subnet $fesub2,$besub2,$gwsub2

#### 3\. Erstellen des Aktiv/Aktiv-VPN-Gateways für TestVNet2

Fordern Sie zwei öffentliche IP-Adressen zur Zuordnung zu dem Gateway an, das Sie für Ihr VNet erstellen. Definieren Sie auch die erforderlichen Subnetz- und IP-Konfigurationen.

	$gw2pip1    = New-AzureRmPublicIpAddress -Name $GW2IPName1 -ResourceGroupName $RG2 -Location $Location2 -AllocationMethod Dynamic
	$gw2pip2    = New-AzureRmPublicIpAddress -Name $GW2IPName2 -ResourceGroupName $RG2 -Location $Location2 -AllocationMethod Dynamic

	$vnet2      = Get-AzureRmVirtualNetwork -Name $VNetName2 -ResourceGroupName $RG2
	$subnet2    = Get-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet2
	$gw2ipconf1 = New-AzureRmVirtualNetworkGatewayIpConfig -Name $GW2IPconf1 -Subnet $subnet2 -PublicIpAddress $gw2pip1
	$gw2ipconf2 = New-AzureRmVirtualNetworkGatewayIpConfig -Name $GW2IPconf2 -Subnet $subnet2 -PublicIpAddress $gw2pip2

Erstellen Sie das VPN-Gateway mit der ASN und dem Flag „EnableActiveActiveFeature“. Beachten Sie, dass Sie die Standard-ASN auf Ihren Azure VPN Gateways überschreiben müssen. Die ASNs für die verbundenen VNets müssen für die Aktivierung von BGP und Transitrouting unterschiedlich sein.

	New-AzureRmVirtualNetworkGateway -Name $GWName2 -ResourceGroupName $RG2 -Location $Location2 -IpConfigurations $gw2ipconf1,$gw2ipconf2 -GatewayType Vpn -VpnType RouteBased -GatewaySku HighPerformance -Asn $VNet2ASN -EnableActiveActiveFeature

### Schritt 2: Verbinden der TestVNet1 und TestVNet2-Gateways

In diesem Beispiel befinden sich beide Gateways im selben Abonnement. Sie können diesen Schritt in der gleichen PowerShell-Sitzung ausführen.

#### 1\. Abrufen der beiden Gateways

Stellen Sie sicher, dass Sie sich bei Abonnement 1 anmelden und die Verbindung mit diesem Abonnement herstellen.

	$vnet1gw = Get-AzureRmVirtualNetworkGateway -Name $GWName1 -ResourceGroupName $RG1
	$vnet2gw = Get-AzureRmVirtualNetworkGateway -Name $GWName2 -ResourceGroupName $RG2
	
#### 2\. Erstellen der beiden Verbindungen

In diesem Schritt erstellen Sie die Verbindung von TestVNet1 zu TestVNet2 sowie die Verbindung von TestVNet2 zu TestVNet1.

	New-AzureRmVirtualNetworkGatewayConnection -Name $Connection12 -ResourceGroupName $RG1 -VirtualNetworkGateway1 $vnet1gw -VirtualNetworkGateway2 $vnet2gw -Location $Location1 -ConnectionType Vnet2Vnet -SharedKey 'AzureA1b2C3' -EnableBgp $True

	New-AzureRmVirtualNetworkGatewayConnection -Name $Connection21 -ResourceGroupName $RG2 -VirtualNetworkGateway1 $vnet2gw -VirtualNetworkGateway2 $vnet1gw -Location $Location2 -ConnectionType Vnet2Vnet -SharedKey 'AzureA1b2C3' -EnableBgp $True

>[AZURE.IMPORTANT] Aktivieren Sie BGP für BEIDE Verbindungen.

Danach wird die Verbindung innerhalb weniger Minuten hergestellt, und die BGP-Peeringsitzung ist eingerichtet, sobald die VNet-zu-VNet-Verbindung mit dualer Redundanz fertiggestellt ist:

![Aktiv/Aktiv, VNet zu VNet](./media/vpn-gateway-activeactive-rm-powershell/vnet-to-vnet.png)

## <a name ="aaupdate"></a>Teil 4: Aktualisieren des vorhandenen Gateways zwischen Aktiv/Aktiv und Aktiv/Standby

Im letzten Abschnitt wird beschrieben, wie Sie ein vorhandenes Azure VPN Gateway aus dem Aktiv/Standby-Modus für den Aktiv/Aktiv-Modus konfigurieren (oder umgekehrt).

>[AZURE.IMPORTANT] Beachten Sie, dass der Aktiv/Aktiv-Modus nur für eine HighPerformance-SKU funktioniert.

### Konfigurieren eines Aktiv/Standby-Gateways in ein Aktiv/Aktiv-Gateway

#### 1\. Gatewayparameter

Im folgenden Beispiel wird ein Aktiv/Standby-Gateway in ein Aktiv/Aktiv-Gateway konvertiert. Sie müssen eine andere öffentliche IP-Adresse erstellen und dann eine zweite Gateway-IP-Konfiguration hinzufügen. Unten sind die verwendeten Parameter aufgeführt:

	$GWName     = "TestVNetAA1GW"
	$VNetName   = "TestVNetAA1"
	$RG         = "TestVPNActiveActive01"
	$GWIPName2  = "gwpip2"
	$GWIPconf2  = "gw1ipconf2"

	$vnet       = Get-AzureRmVirtualNetwork -Name $VNetName -ResourceGroupName $RG
	$subnet     = Get-AzureRmVirtualNetworkSubnetConfig -Name 'GatewaySubnet' -VirtualNetwork $vnet
	$gw         = Get-AzureRmVirtualNetworkGateway -Name $GWName -ResourceGroupName $RG
	$location   = $gw.Location

#### 2\. Erstellen der öffentlichen IP-Adresse und anschließendes Hinzufügen der zweiten Gateway-IP-Konfiguration

	$gwpip2     = New-AzureRmPublicIpAddress -Name $GWIPName2 -ResourceGroupName $RG -Location $location -AllocationMethod Dynamic
	Add-AzureRmVirtualNetworkGatewayIpConfig -VirtualNetworkGateway $gw -Name $GWIPconf2 -Subnet $subnet -PublicIpAddress $gwpip2 

#### 3\. Aktivieren des Aktiv/Aktiv-Modus und Aktualisieren des Gateways

Sie müssen das Gatewayobjekt in PowerShell festlegen, um die eigentliche Aktualisierung auszulösen.

	Set-AzureRmVirtualNetworkGateway -VirtualNetworkGateway $gw -EnableActiveActiveFeature

Diese Aktualisierung kann 30 bis 45 Minuten dauern.

### Konfigurieren eines Aktiv/Aktiv-Gateways in ein Aktiv/Standby-Gateway

#### 1\. Gatewayparameter

Verwenden Sie die gleichen Parameter wie oben, und rufen Sie den Namen der IP-Konfiguration auf, die Sie entfernen möchten.

	$GWName     = "TestVNetAA1GW"
	$RG         = "TestVPNActiveActive01"

	$gw         = Get-AzureRmVirtualNetworkGateway -Name $GWName -ResourceGroupName $RG
	$ipconfname = $gw.IpConfigurations[1].Name

#### 2\. Entfernen der Gateway-IP-Konfiguration und Deaktivieren des Aktiv/Aktiv-Modus

Außerdem müssen Sie das Gatewayobjekt in PowerShell festlegen, um die eigentliche Aktualisierung auszulösen.

	Remove-AzureRmVirtualNetworkGatewayIpConfig -Name $ipconfname -VirtualNetworkGateway $gw
	Set-AzureRmVirtualNetworkGateway -VirtualNetworkGateway $gw -DisableActiveActiveFeature

Diese Aktualisierung kann bis zu 30 bis 45 Minuten dauern.


## Nächste Schritte

Sobald die Verbindung hergestellt ist, können Sie Ihren virtuellen Netzwerken virtuelle Computer hinzufügen. Für diese Schritte finden Sie Informationen unter [Erstellen eines virtuellen Computers](../virtual-machines/virtual-machines-windows-hero-tutorial.md).

<!---HONumber=AcomDC_0928_2016-->