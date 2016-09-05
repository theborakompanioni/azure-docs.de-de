<properties 
   pageTitle="Informationen zu VPN Gateway-Einstellungen | Microsoft Azure"
   description="Hier erhalten Sie Informationen zu VPN Gateway-Einstellungen für Azure Virtual Network."
   services="vpn-gateway"
   documentationCenter="na"
   authors="cherylmc"
   manager="carmonm"
   editor=""
   tags="azure-resource-manager,azure-service-management"/>
<tags 
   ms.service="vpn-gateway"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="08/17/2016"
   ms.author="cherylmc" />

# Informationen zu VPN Gateway-Einstellungen

Das VPN Gateway umfasst eine Sammlung von Einstellungen, die zum Senden von Netzwerkdatenverkehr zwischen virtuellen Netzwerken und lokalen Standorten verwendet werden. Über VPN Gateway können Sie auch den Datenverkehr zwischen den VNets in Azure senden. In den Abschnitten in diesem Artikel werden die Einstellungen beschrieben, die sich auf das VPN Gateway beziehen.

Manchmal ist es hilfreich, die verfügbaren Konfigurationen mithilfe von Verbindungsdiagrammen anzuzeigen. Diagramme zum Bereitstellen der Konfigurationen finden Sie im Abschnitt [Informationen zu VPN Gateway-Verbindungen](vpn-gateway-topology.md) des Artikels.


## <a name="gwsku"></a>Gateway-SKUs

Beim Erstellen eines VPN-Gateways müssen Sie die gewünschte Gateway-SKU angeben. Gateway-SKUs gelten sowohl für ExpressRoute als auch für das VPN Gateway. Für die einzelnen Gateway-SKUs gelten unterschiedliche Preise. Informationen zu den Preisen finden Sie unter [VPN-Gateway – Preise](https://azure.microsoft.com/pricing/details/vpn-gateway/). Weitere Informationen über ExpressRoute finden Sie unter [ExpressRoute – Technische Übersicht](../expressroute/expressroute-introduction.md).

Es gibt drei VPN-Gateway-SKUs:

- Basic
- Standard
- HighPerformance

Das folgende PowerShell-Beispiel gibt `-GatewaySku` als *Standard* an.

	New-AzureRmVirtualNetworkGateway -Name vnetgw1 -ResourceGroupName testrg `
	-Location 'West US' -IpConfigurations $gwipconfig -GatewaySku Standard `
	-GatewayType Vpn -VpnType RouteBased


###  <a name="aggthroughput"></a>Geschätzter aggregierter Durchsatz nach SKU und Gatewaytyp


In der folgenden Tabelle sind die Gatewaytypen und der geschätzte zusammengefasste Durchsatz angegeben. Diese Tabelle betrifft sowohl das Resource Manager-Bereitstellungsmodell als auch das klassische Bereitstellungsmodell.

[AZURE.INCLUDE [vpn-gateway-table-gwtype-aggthroughput](../../includes/vpn-gateway-table-gwtype-aggtput-include.md)]

## <a name="gwtype"></a>Gatewaytypen

Mit dem Gatewaytyp wird angegeben, wie das Gateway die Verbindung hergestellt. Es ist eine erforderliche Konfigurationseinstellung für das Resource Manager-Bereitstellungsmodell. Verwechseln Sie den Gatewaytyp nicht mit dem VPN-Typ, mit dem der Typ der Weiterleitung für Ihr VPN angegeben wird. Die verfügbaren Werte für `-GatewayType` lauten:

- VPN
- ExpressRoute


In diesem PowerShell-Beispiel für das Resource Manager-Bereitstellungsmodell wird „-GatewayType“ als *Vpn* angegeben. Beim Erstellen eines Gateways müssen Sie sicherstellen, dass der Gatewaytyp für Ihre Konfiguration korrekt ist.

	New-AzureRmVirtualNetworkGateway -Name vnetgw1 -ResourceGroupName testrg `
	-Location 'West US' -IpConfigurations $gwipconfig -GatewayType Vpn `
	-VpnType RouteBased

## <a name="connectiontype"></a>Verbindungstypen

Für jede Konfiguration ist ein bestimmter Verbindungstyp erforderlich. Die verfügbaren Resource Manager-PowerShell-Werte für `-ConnectionType` sind:

- IPsec
- Vnet2Vnet
- ExpressRoute
- VPNClient

Im folgenden PowerShell-Beispiel erstellen wir eine S2S-Verbindung, die den Verbindungstyp „IPsec“ erfordert.

	New-AzureRmVirtualNetworkGatewayConnection -Name localtovon -ResourceGroupName testrg `
	-Location 'West US' -VirtualNetworkGateway1 $gateway1 -LocalNetworkGateway2 $local `
	-ConnectionType IPsec -RoutingWeight 10 -SharedKey 'abc123'


## <a name="vpntype"></a>VPN-Typen

Für jede Konfiguration ist ein bestimmter VPN-Typ erforderlich, damit sie funktioniert. Beim Erstellen der Konfiguration wählen Sie den VPN-Typ aus, der für Ihre Verbindung erforderlich ist.

Beispiel: Wenn Sie mit Standort-zu-Standort (S2S) und Punkt-zu-Standort (P2S) eine Verbindung mit einem VNet herstellen möchten, müssen Sie einen VPN-Typ verwenden, der die Verbindungsanforderungen für beide Konfigurationen erfüllt. P2S-Verbindungen erfordern einen routenbasierten VPN-Typ (Gateway mit dynamischem Routing), während S2S-Verbindungen manchmal einen richtlinienbasierten VPN-Typ unterstützen können. Dies bedeutet, dass Sie das Gateway mit dem routenbasierten VPN-Typ (dynamisches Routing) zur Unterstützung von P2S erneut erstellen müssen, wenn Sie bereits über eine S2S-Verbindung verfügen, die einen richtlinienbasierten VPN-Typ verwendet (statisches Routing).

Es gibt zwei VPN-Typen:

[AZURE.INCLUDE [vpn-gateway-vpntype](../../includes/vpn-gateway-vpntype-include.md)]



In diesem PowerShell-Beispiel für das Resource Manager-Bereitstellungsmodell wird `-VpnType` als *RouteBased* angegeben. Beim Erstellen eines Gateways müssen Sie sicherstellen, dass -VpnType für Ihre Konfiguration korrekt ist.

	New-AzureRmVirtualNetworkGateway -Name vnetgw1 -ResourceGroupName testrg `
	-Location 'West US' -IpConfigurations $gwipconfig `
	-GatewayType Vpn -VpnType RouteBased

##  <a name="requirements"></a>Gatewayanforderungen

[AZURE.INCLUDE [vpn-gateway-table-requirements](../../includes/vpn-gateway-table-requirements-include.md)]


## <a name="gwsub"></a>Gatewaysubnetz

Sie müssen zuerst ein Gatewaysubnetz für Ihr VNET erstellen, um ein VPN Gateway zu konfigurieren. Das Gatewaysubnetz muss den Namen *GatewaySubnet* aufweisen, damit es einwandfrei funktioniert. Anhand dieses Namens kann Azure erkennen, dass dieses Subnetz für das Gateway verwendet werden soll.<BR>Wenn Sie das klassische Portal verwenden, erhält das Gatewaysubnetz auf der Portaloberfläche automatisch den Namen *Gateway*. Dies gilt nur speziell für die Anzeige des Gatewaysubnetzes im klassischen Portal. In diesem Fall wird das Subnetz tatsächlich mit dem Wert *GatewaySubnet* erstellt und kann auf diese Weise im Azure-Portal und in PowerShell angezeigt werden.

Die Mindestgröße des Gatewaysubnetzes hängt gänzlich von der Konfiguration ab, die Sie erstellen möchten. Obwohl es möglich ist, ein Gatewaysubnetz mit einer Größe von nur /29 zu erstellen, empfehlen wir, ein Gatewaysubnetz von mindestens /28 (/ 28, / 27, /26 usw.) zu erstellen.

Wenn Sie eine höhere Gatewaygröße wählen, wird verhindert, dass Sie die Größenbeschränkung für Gateways erreichen. Beispiel: Sie könnten ein Gateway für ein virtuelles Netzwerk mit einem Gatewaysubnetz der Größe /29 für eine S2S-Verbindung erstellt haben. Sie möchten jetzt eine Konfiguration einrichten, in der S2S und ExpressRoute gleichzeitig verwendet werden. Diese Konfiguration erfordert ein Gatewaysubnetz mit einer Mindestgröße von /28. Um Ihre Konfiguration zu erstellen, müssten Sie das Gatewaysubnetz an die Mindestanforderung für die Verbindung anpassen, und das ist /28.

Im folgenden PowerShell-Beispiel ist ein Gatewaysubnetz mit dem Namen GatewaySubnet zu sehen. Sie erkennen, das mit der CIDR-Notation die Größe /27 angegeben wird. Dies ist für eine ausreichende Zahl von IP-Adressen für die meisten Konfigurationen, die derzeit üblich sind, groß genug.

	Add-AzureRmVirtualNetworkSubnetConfig -Name 'GatewaySubnet' -AddressPrefix 10.0.3.0/27

>[AZURE.IMPORTANT] Stellen Sie sicher, dass dem Gatewaysubnetz keine Netzwerksicherheitsgruppe (NSG) zugewiesen ist, da dies Verbindungsfehler verursachen kann.


## <a name="lng"></a>Gateways des lokalen Netzwerks

Mit dem Gateway des lokalen Netzwerks ist normalerweise Ihr lokaler Standort gemeint. Beim klassischen Bereitstellungsmodell wurde das Gateway des lokalen Netzwerks als „Lokaler Standort“ bezeichnet. Sie geben dem Gateway des lokalen Netzwerks einen Namen, stellen die öffentliche IP-Adresse des lokalen VPN-Geräts bereit und geben die Adresspräfixe an, die sich am lokalen Standort befinden. Azure sucht unter den Zieladresspräfixen nach Netzwerkdatenverkehr, sieht in der Konfiguration nach, die Sie für das Gateway des lokalen Netzwerks angegeben haben, und leitet die Pakete entsprechend weiter.

Manchmal müssen Sie die Einstellungen des Gateways des lokalen Netzwerks ändern, beispielsweise wenn Sie den Adressbereich erweitern oder ändern oder wenn sich die IP-Adresse des VPN-Geräts ändert. Bei einem klassischen VNet können Sie diese Einstellungen im klassischen Portal auf der Seite „Lokale Netzwerke“ ändern. Informationen für Resource Manager finden Sie unter [Ändern der Einstellungen des lokalen Netzwerkgateways mit PowerShell](vpn-gateway-modify-local-network-gateway.md).

## <a name="resources"></a>REST-APIs und PowerShell

Zusätzliche technische Ressourcen und spezielle Syntaxanforderungen bei der Verwendung von REST-APIs und PowerShell finden Sie auf den folgenden Seiten:

|**Klassisch** | **Ressourcen-Manager**|
|-----|----|
|[PowerShell](https://msdn.microsoft.com/library/mt270335.aspx)|[PowerShell](https://msdn.microsoft.com/library/mt163510.aspx)|
|[REST-API](https://msdn.microsoft.com/library/jj154113.aspx)|[REST-API](https://msdn.microsoft.com/library/mt163859.aspx)|


## Nächste Schritte

Weitere Informationen zu verfügbaren Verbindungskonfigurationen finden Sie unter [Informationen zu VPN Gateway](vpn-gateway-topology.md).







 

<!---HONumber=AcomDC_0824_2016-->