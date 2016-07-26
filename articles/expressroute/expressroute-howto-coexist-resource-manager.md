<properties
   pageTitle="Konfigurieren von ExpressRoute- und Standort-zu-Standort-VPN-Verbindungen, die gleichzeitig bestehen können, für das Resource Manager-Bereitstellungsmodell | Microsoft Azure"
   description="Dieser Artikel führt Sie durch die Konfiguration von ExpressRoute- und Standort-zu-Standort-VPN-Verbindungen, die für das Resource Manager-Modell gleichzeitig bestehen können."
   documentationCenter="na"
   services="expressroute"
   authors="charwen"
   manager="carmonm"
   editor=""
   tags="azure-resource-manager"/>
<tags
   ms.service="expressroute"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="07/19/2016"
   ms.author="charleywen"/>

# Konfigurieren von parallel bestehenden ExpressRoute- und Standort-zu-Standort-Verbindungen für das Resource Manager-Bereitstellungsmodell

> [AZURE.SELECTOR]
- [PowerShell – Resource Manager](expressroute-howto-coexist-resource-manager.md)
- [PowerShell – klassisch](expressroute-howto-coexist-classic.md)

Die Möglichkeit zum Konfigurieren von Standort-zu-Standort-VPN und ExpressRoute bietet mehrere Vorteile. Sie können Standort-zu-Standort-VPNs als sicheren Failoverpfad für ExpressRoute konfigurieren oder für die Verbindung mit Websites nutzen, die nicht über ExpressRoute verbunden sind. Wir werden die Schritte zum Konfigurieren von beiden Szenarien in diesem Artikel behandeln. Dieser Artikel gilt für das Resource Manager-Bereitstellungsmodell. Diese Konfiguration ist nicht im Azure-Portal verfügbar.


**Informationen zu Azure-Bereitstellungsmodellen**

[AZURE.INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-classic-rm-include.md)]

>[AZURE.IMPORTANT] ExpressRoute-Verbindungen müssen bereits konfiguriert sein, bevor Sie die folgenden Schritte ausführen. Stellen Sie sicher, dass Sie die Handbücher zu [ExpressRoute-Verbindung erstellen](expressroute-howto-circuit-arm.md) und [Konfigurieren des Routings](expressroute-howto-routing-arm.md) gelesen haben, bevor Sie die folgenden Schritte unternehmen.

## Grenzwerte und Einschränkungen

- **Transitrouting wird nicht unterstützt:** Ein Routing (über Azure) zwischen Ihrem lokalen Netzwerk mit Standort-zu-Standort-VPN-Verbindung und Ihrem lokalen Netzwerk mit ExpressRoute-Verbindung ist nicht möglich.
- **Erzwungenes Tunneln kann nicht auf dem Standort-zu-Standort-VPN Gateway aktiviert werden:** Sie können nur „erzwingen“, dass sämtlicher Internetdatenverkehr über ExpressRoute an Ihr lokales Netzwerk zurückgeleitet wird.
- **Nur Standard- oder Hochleistungsgateways:** Sie müssen ein Standard- oder Hochleistungsgateway sowohl für das ExpressRoute-Gateway als auch das Standort-zu-Standort-VPN Gateway verwenden. Informationen zu Gateway-SKUs finden Sie unter [Gateway-SKUs](../vpn-gateway/vpn-gateway-about-vpngateways.md).
- **Nur routenbasiertes VPN Gateway:** Sie müssen ein routenbasiertes VPN Gateway verwenden. Informationen zum routenbasierten VPN Gateway finden Sie unter [VPN Gateway](../vpn-gateway/vpn-gateway-about-vpngateways.md).
- **Voraussetzungen für eine statische Route:** Wenn Ihr lokales Netzwerk mit ExpressRoute und einem Standort-zu-Standort-VPN verbunden ist, müssen Sie eine statische Route in Ihrem lokalen Netzwerk konfiguriert haben, um die Standort-zu-Standort-VPN-Verbindung an das öffentliche Internet weiterzuleiten.
- **ExpressRoute-Gateway muss zuerst konfiguriert werden:** Sie müssen zuerst das ExpressRoute-Gateway erstellen, bevor Sie das Standort-zu-Standort-VPN Gateway hinzufügen.


## Konfigurationsentwürfe

### Konfigurieren eines Standort-zu-Standort-VPN als Failoverpfad für ExpressRoute

Sie können eine Standort-zu-Standort-VPN-Verbindung als Sicherung für ExpressRoute konfigurieren. Dies gilt nur für virtuelle Netzwerke, die mit dem privaten Azure-Peeringpfad verknüpft sind. Es gibt keine VPN-basierte Failoverlösung für Dienste, auf die über öffentliche Azure- und Microsoft-Peerings zugegriffen werden. Die ExpressRoute-Verbindung ist immer der primäre Link. Durch den Pfad des Standort-zu-Standort-VPN fließen die Daten nur, wenn bei der ExpressRoute-Verbindung ein Fehler auftritt.

![Koexistenz](media/expressroute-howto-coexist-resource-manager/scenario1.jpg)

### Konfigurieren einer Standort-zu-Standort-VPN zum Herstellen einer Verbindung mit Websites, die nicht über ExpressRoute verbunden sind

Sie können Ihr Netzwerk so konfigurieren, dass einige Sites direkt mit Azure über ein Standort-zu-Standort-VPN und andere über ExpressRoute verbunden sind.

![Koexistenz](media/expressroute-howto-coexist-resource-manager/scenario2.jpg)

>[AZURE.NOTE] Sie können ein virtuelles Netzwerk nicht als Transitrouter konfigurieren.

## Auswählen der zu verwendenden Schritte

Es stehen zwei unterschiedliche Verfahren für die Konfiguration von Verbindungen zur Auswahl, die gleichzeitig bestehen können. Welches Konfigurationsverfahren Sie wählen, hängt davon ab, ob Sie über ein vorhandenes virtuelles Netzwerk verfügen, mit dem Sie eine Verbindung aufbauen möchten, oder ob Sie ein neues virtuelles Netzwerk erstellen möchten.


- Ich verfüge nicht über ein VNET und muss eines erstellen.
	
	Wenn Sie noch nicht über ein virtuelles Netzwerk verfügen, werden Sie in diesem Verfahren durch die Schritte zum Erstellen eines neuen virtuellen Netzwerks mithilfe des Resource Manager-Bereitstellungsmodells und zum Erstellen neuer ExpressRoute- sowie Standort-zu-Standort-VPN-Verbindungen geführt. Führen Sie für die Konfiguration die Schritte im Artikel [So erstellen Sie ein neues virtuelles Netzwerk und parallele Verbindungen](#new) durch.

- Ich verfüge bereits über ein VNET, das auf dem Resource Manager-Bereitstellungsmodell basiert.

	Möglicherweise haben Sie bereits ein virtuelles Netzwerk mit einer vorhandenen Standort-zu-Standort-VPN-Verbindung oder einer ExpressRoute-Verbindung. Im Abschnitt [So konfigurieren Sie parallele Verbindungen für ein bereits vorhandenes VNET](#add) werden Sie durch die Schritte zum Löschen des Gateways und anschließenden Erstellen neuer ExpressRoute- und Standort-zu-Standort-VPN-Verbindungen geführt. Beachten Sie, dass die Schritte beim Erstellen von neuen Verbindungen in einer ganz bestimmten Reihenfolge ausgeführt werden müssen. Verwenden Sie die Anweisungen nicht in anderen Artikeln, um Ihre Gateways und Verbindungen zu erstellen.

	In diesem Verfahren erfordert das Erstellen von gleichzeitig bestehenden Verbindungen das Löschen Ihres Gateway löschen und das anschließende Konfigurieren neuer Gateways. Das bedeutet, dass Sie für Ihre standortübergreifenden Verbindungen Ausfallzeiten haben werden, während Sie Ihr Gateway und Ihre Verbindungen löschen und neu erstellen, aber Sie müssen keine Ihrer VMs oder Dienste auf ein neues virtuelles Netzwerk migrieren. Die VMs und Dienste werden immer noch in der Lage sein, über den Load Balancer zu kommunizieren, während Sie Ihr Gateway konfigurieren, wenn sie zu diesem Zweck konfiguriert sind.


## <a name="new"></a>So erstellen Sie ein neues virtuelles Netzwerk und parallele Verbindungen

Dieses Verfahren führt Sie durch das Erstellen eines VNET sowie das Erstellen von gleichzeitig bestehenden Standort-zu-Standort- und ExpressRoute-Verbindungen.
	
1. Sie müssen die aktuelle Version der Azure PowerShell-Cmdlets installieren. Weitere Informationen zur Installation der PowerShell-Cmdlets finden Sie unter [Installieren und Konfigurieren von Azure PowerShell](../powershell-install-configure.md). Beachten Sie, dass die Cmdlets, die Sie für diese Konfiguration verwenden, von den Ihnen vertrauten leicht abweichen können. Achten Sie darauf, die in dieser Anleitung angegebenen Cmdlets zu verwenden.

2. Melden Sie sich an Ihrem Konto an, und richten Sie die Umgebung ein.
	
		login-AzureRmAccount
		Select-AzureRmSubscription -SubscriptionName 'yoursubscription'
		$location = "Central US"
		$resgrp = New-AzureRmResourceGroup -Name "ErVpnCoex" -Location $location

3. Erstellen Sie ein virtuelles Netzwerk mit einem Gatewaysubnetz. Weitere Informationen zur Konfiguration des virtuellen Netzwerks finden Sie unter [Azure Virtual Network-Konfiguration](../virtual-network/virtual-networks-create-vnet-arm-ps.md).

	>[AZURE.IMPORTANT] Das Gatewaysubnetz muss die Größe /27 haben oder ein kürzeres Präfix aufweisen (z.B. /26 oder /25).
	
	Erstellen Sie ein neues VNET.

		$vnet = New-AzureRmVirtualNetwork -Name "CoexVnet" -ResourceGroupName $resgrp.ResourceGroupName -Location $location -AddressPrefix "10.200.0.0/16" 

	Fügen Sie Subnetze hinzu.

		Add-AzureRmVirtualNetworkSubnetConfig -Name "App" -VirtualNetwork $vnet -AddressPrefix "10.200.1.0/24"
		Add-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet -AddressPrefix "10.200.255.0/24"

	Speichern Sie die VNET-Konfiguration.

		$vnet = Set-AzureRmVirtualNetwork -VirtualNetwork $vnet

4. <a name="gw"></a>Erstellen Sie ein ExpressRoute-Gateway. Weitere Informationen zur Konfiguration des ExpressRoute-Gateways finden Sie unter [ExpressRoute-Gatewaykonfiguration](expressroute-howto-add-gateway-resource-manager.md). Für die GatewaySKU muss *Standard* oder *HighPerformance* ausgewählt werden.

		$gwSubnet = Get-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet
		$gwIP = New-AzureRmPublicIpAddress -Name "ERGatewayIP" -ResourceGroupName $resgrp.ResourceGroupName -Location $location -AllocationMethod Dynamic
		$gwConfig = New-AzureRmVirtualNetworkGatewayIpConfig -Name "ERGatewayIpConfig" -SubnetId $gwSubnet.Id -PublicIpAddressId $gwIP.Id
		$gw = New-AzureRmVirtualNetworkGateway -Name "ERGateway" -ResourceGroupName $resgrp.ResourceGroupName -Location $location -IpConfigurations $gwConfig -GatewayType "ExpressRoute" -GatewaySku Standard 

5. Verknüpfen Sie das ExpressRoute-Gateway mit dem ExpressRoute-Schaltkreis. Nachdem dieser Schritt abgeschlossen ist, wird die Verbindung zwischen dem lokalen Netzwerk und Azure durch ExpressRoute eingerichtet. Weitere Informationen zum Verknüpfungsvorgang finden Sie unter [Verknüpfen von VNETs mit ExpressRoute](expressroute-howto-linkvnet-arm.md).

		$ckt = Get-AzureRmExpressRouteCircuit -Name "YourCircuit" -ResourceGroupName "YourCircuitResourceGroup"
		New-AzureRmVirtualNetworkGatewayConnection -Name "ERConnection" -ResourceGroupName $resgrp.ResourceGroupName -Location $location -VirtualNetworkGateway1 $gw -PeerId $ckt.Id -ConnectionType ExpressRoute

6. <a name="vpngw"></a>Erstellen Sie als Nächstes Ihr Standort-zu-Standort-VPN-Gateway. Weitere Informationen zur VPN Gateway-Konfiguration finden Sie unter [Konfigurieren von VNet-zu-VNet-Verbindungen](../vpn-gateway/vpn-gateway-vnet-vnet-rm-ps.md). Für „GatewaySKU“ muss *Standard* oder *HighPerformance* ausgewählt werden. Für „VpnType“ muss *RouteBased* verwendet werden.

		$gwSubnet = Get-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet
		$gwIP = New-AzureRmPublicIpAddress -Name "VPNGatewayIP" -ResourceGroupName $resgrp.ResourceGroupName -Location $location -AllocationMethod Dynamic
		$gwConfig = New-AzureRmVirtualNetworkGatewayIpConfig -Name "VPNGatewayIpConfig" -SubnetId $gwSubnet.Id -PublicIpAddressId $gwIP.Id
		New-AzureRmVirtualNetworkGateway -Name "VPNGateway" -ResourceGroupName $resgrp.ResourceGroupName -Location $location -IpConfigurations $gwConfig -GatewayType "Vpn" -VpnType "RouteBased" -GatewaySku "Standard"

7. Erstellen Sie eine VPN-Gateway-Entität für einen lokalen Standort. Mit diesem Befehl wird das lokale VPN-Gateway nicht konfiguriert. Stattdessen können Sie mit ihm die Einstellungen des lokalen Gateway bereitstellen, wie z. B. die öffentliche IP-Adresse und der lokale Adressraum, sodass eine Verbindung mit dem Azure-VPN-Gateway hergestellt werden kann.
	>[AZURE.NOTE] Wenn Ihr lokales Netzwerk mehrere Routen hat, können Sie diese als ein Array übergeben: $MyLocalNetworkAddress = @("10.100.0.0/16","10.101.0.0/16","10.102.0.0/16")

		$localVpn = New-AzureRmLocalNetworkGateway -Name "LocalVPNGateway" -ResourceGroupName $resgrp.ResourceGroupName -Location $location -GatewayIpAddress *<Public IP>* -AddressPrefix '10.100.0.0/16'

8. Konfigurieren Sie Ihr lokales VPN-Gerät für die Verbindung zum neuen Azure VPN Gateway. Weitere Informationen zu VPN-Gerätekonfiguration finden Sie unter [VPN-Gerätekonfiguration](../vpn-gateway/vpn-gateway-about-vpn-devices.md).

9. Verknüpfen Sie das Standort-zu-Standort-VPN-Gateway in Azure mit dem lokalen Gateway.

		$azureVpn = Get-AzureRmVirtualNetworkGateway -Name "VPNGateway" -ResourceGroupName $resgrp.ResourceGroupName
		New-AzureRmVirtualNetworkGatewayConnection -Name "VPNConnection" -ResourceGroupName $resgrp.ResourceGroupName -Location $location -VirtualNetworkGateway1 $azureVpn -LocalNetworkGateway2 $localVpn -ConnectionType IPsec -SharedKey <yourkey>


## <a name="add"></a>So konfigurieren Sie parallele Verbindungen für ein bereits vorhandenes VNET

Wenn Sie über ein vorhandenes virtuelles Netzwerk verfügen, prüfen Sie die Größe des Gateway-Subnetzes. Wenn das Gateway-Subnet /28 oder /29 ist, müssen Sie zunächst das Gateway des virtuellen Netzwerks löschen, um die Größe des Gateway-Subnetzes zu erhöhen. Führen Sie dazu die in diesem Abschnitt beschriebenen Schritte aus.

Wenn das Gateway-Subnetz /27 oder größer ist und das virtuelle Netzwerk über ExpressRoute verbunden ist, können Sie die unten beschriebenen Schritte überspringen und direkt mit [„Schritt 6: Erstellen eines Standort-zu-Standort-VPN-Gateways“](#vpngw) (s. vorheriger Abschnitt) fortfahren.

>[AZURE.NOTE] Wenn Sie das vorhandene Gateway löschen, geht Ihre lokale Verbindung mit Ihrem virtuellen Netzwerk verloren, während Sie an dieser Konfiguration arbeiten.

1. Sie müssen die aktuelle Version der Azure PowerShell-Cmdlets installieren. Weitere Informationen zur Installation der PowerShell-Cmdlets finden Sie unter [Installieren und Konfigurieren von Azure PowerShell](../powershell-install-configure.md). Beachten Sie, dass die Cmdlets, die Sie für diese Konfiguration verwenden, von den Ihnen vertrauten leicht abweichen können. Achten Sie darauf, die in dieser Anleitung angegebenen Cmdlets zu verwenden.

2. Löschen Sie das vorhandene ExpressRoute- oder Standort-zu-Standort-VPN Gateway.

		Remove-AzureRmVirtualNetworkGateway -Name <yourgatewayname> -ResourceGroupName <yourresourcegroup>

3. Löschen Sie das Gatewaysubnetz.
		
		$vnet = Get-AzureRmVirtualNetwork -Name <yourvnetname> -ResourceGroupName <yourresourcegroup> 
		Remove-AzureRmVirtualNetworkSubnetConfig -Name GatewaySubnet -VirtualNetwork $vnet

4. Fügen Sie ein Gatewaysubnetz hinzu, das mindestens die Größe /27 hat.
	>[AZURE.NOTE] Wenn in Ihrem virtuellen Netzwerk nicht mehr ausreichend IP-Adressen vorhanden sind, um die Größe des Gateway-Subnetzes zu erhöhen, müssen Sie mehr IP-Adressraum hinzufügen.

		$vnet = Get-AzureRmVirtualNetwork -Name <yourvnetname> -ResourceGroupName <yourresourcegroup>
		Add-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet -AddressPrefix "10.200.255.0/24"

	Speichern Sie die VNET-Konfiguration.

		$vnet = Set-AzureRmVirtualNetwork -VirtualNetwork $vnet

5. An diesem Punkt verfügen Sie über ein VNet ohne Gateways. Um neue Gateways zu erstellen und Ihre Verbindungen abzuschließen, können Sie mit [Schritt 4: Erstellen eines ExpressRoute-Gateways](#gw) in den vorangegangenen Schritten fortfahren.

## So fügen Sie dem VPN Gateway eine Punkt-zu-Standort-Konfiguration hinzu
Sie können die unten angegebenen Schritte ausführen, um dem VPN Gateway bei einer parallelen Einrichtung eine Punkt-zu-Standort-Konfiguration hinzuzufügen.

1. Fügen Sie einen VPN-Clientadresspool hinzu.

		$azureVpn = Get-AzureRmVirtualNetworkGateway -Name "VPNGateway" -ResourceGroupName $resgrp.ResourceGroupName
		Set-AzureRmVirtualNetworkGatewayVpnClientConfig -VirtualNetworkGateway $azureVpn -VpnClientAddressPool "10.251.251.0/24"

2. Laden Sie das VPN-Stammzertifikat für Ihr VPN Gateway in Azure hoch. In diesem Beispiel wird davon ausgegangen, dass das Stammzertifikat auf dem lokalen Computer gespeichert wird, auf dem die folgenden PowerShell-Cmdlets ausgeführt werden.

		$p2sCertFullName = "RootErVpnCoexP2S.cer"
		$p2sCertMatchName = "RootErVpnCoexP2S"
		$p2sCertToUpload=get-childitem Cert:\CurrentUser\My | Where-Object {$_.Subject -match $p2sCertMatchName}
		if ($p2sCertToUpload.count -eq 1){
		    write-host "cert found"
		} else {
		    write-host "cert not found"
		    exit
		} 
		$p2sCertData = [System.Convert]::ToBase64String($p2sCertToUpload.RawData)
		Add-AzureRmVpnClientRootCertificate -VpnClientRootCertificateName $p2sCertFullName -VirtualNetworkGatewayname $azureVpn.Name -ResourceGroupName $resgrp.ResourceGroupName -PublicCertData $p2sCertData

Weitere Informationen zu Punkt-zu-Standort-VPN-Verbindungen finden Sie unter [Konfigurieren einer Punkt-zu-Standort-Verbindung](../vpn-gateway/vpn-gateway-howto-point-to-site-rm-ps.md).

## Nächste Schritte

Weitere Informationen über ExpressRoute finden Sie unter [ExpressRoute – FAQ](expressroute-faqs.md).

<!---HONumber=AcomDC_0720_2016-->