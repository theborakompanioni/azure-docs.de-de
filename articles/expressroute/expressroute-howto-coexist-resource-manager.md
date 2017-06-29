---
title: "Konfigurieren von ExpressRoute- und Site-to-Site-VPN-Verbindungen, die gleichzeitig bestehen können: Resource Manager: Azure | Microsoft-Dokumentation"
description: "Dieser Artikel führt Sie durch die Konfiguration von ExpressRoute- und Standort-zu-Standort-VPN-Verbindungen, die für das Resource Manager-Modell gleichzeitig bestehen können."
documentationcenter: na
services: expressroute
author: charwen
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: c7717b14-3da3-4a6d-b78e-a5020766bc2c
ms.service: expressroute
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/19/2017
ms.author: charwen,cherylmc
ms.translationtype: Human Translation
ms.sourcegitcommit: 09f24fa2b55d298cfbbf3de71334de579fbf2ecd
ms.openlocfilehash: ffa791cf4c4be15645a67fef4e94bf6ebdc42a6a
ms.contentlocale: de-de
ms.lasthandoff: 06/07/2017


---
# <a name="configure-expressroute-and-site-to-site-coexisting-connections"></a>Konfigurieren von parallel bestehenden ExpressRoute- und Standort-zu-Standort-Verbindungen
> [!div class="op_single_selector"]
> * [PowerShell – Resource Manager](expressroute-howto-coexist-resource-manager.md)
> * [PowerShell – klassisch](expressroute-howto-coexist-classic.md)
> 
> 

Das Konfigurieren paralleler Site-to-Site-VPN- und ExpressRoute-Verbindungen bietet mehrere Vorteile. Sie können ein Site-to-Site-VPN als sicheren Failoverpfad für ExpressRoute konfigurieren oder für Verbindungen mit Websites nutzen, die nicht über ExpressRoute verbunden sind. In diesem Artikel werden die Konfigurationsschritte für beide Szenarien behandelt. Dieser Artikel gilt für das Resource Manager-Bereitstellungsmodell und für die Verwendung von PowerShell. Diese Konfiguration ist nicht im Azure-Portal verfügbar.

> [!IMPORTANT]
> ExpressRoute-Verbindungen müssen bereits konfiguriert sein, bevor Sie die folgenden Schritte ausführen. Vergewissern Sie sich, dass Sie die Schritte zum [Erstellen einer ExpressRoute-Verbindung](expressroute-howto-circuit-arm.md) und zum [Konfigurieren des Routings](expressroute-howto-routing-arm.md) ausgeführt haben, bevor Sie den Vorgang fortsetzen.
> 
> 

## <a name="limits-and-limitations"></a>Grenzwerte und Einschränkungen
* **Transitrouting wird nicht unterstützt.** Ein Routing (über Azure) zwischen Ihrem lokalen Netzwerk mit Standort-zu-Standort-VPN-Verbindung und Ihrem lokalen Netzwerk mit ExpressRoute-Verbindung ist nicht möglich.
* **Das einfache SKU-Gateway wird nicht unterstützt.** Sie müssen für das [ExpressRoute-Gateway](expressroute-about-virtual-network-gateways.md) und das [VPN-Gateway](../vpn-gateway/vpn-gateway-about-vpngateways.md) jeweils ein anderes Gateway als ein einfaches SKU-Gateway verwenden.
* **Nur das routenbasierte VPN-Gateway wird unterstützt.** Sie müssen ein routenbasiertes [VPN-Gateway](../vpn-gateway/vpn-gateway-about-vpngateways.md) verwenden.
* **Für das VPN-Gateway sollte eine statische Route konfiguriert werden.** Wenn Ihr lokales Netzwerk mit ExpressRoute und einem Standort-zu-Standort-VPN verbunden ist, müssen Sie eine statische Route in Ihrem lokalen Netzwerk konfiguriert haben, um die Standort-zu-Standort-VPN-Verbindung an das öffentliche Internet weiterzuleiten.
* **Das ExpressRoute-Gateway muss zuerst konfiguriert und mit einer Verbindung verknüpft werden.** Vor dem Hinzufügen des Site-to-Site-VPN-Gateways muss zunächst das ExpressRoute-Gateway erstellt und mit einer Verbindung verknüpft werden.

## <a name="configuration-designs"></a>Konfigurationsentwürfe
### <a name="configure-a-site-to-site-vpn-as-a-failover-path-for-expressroute"></a>Konfigurieren eines Standort-zu-Standort-VPN als Failoverpfad für ExpressRoute
Sie können eine Standort-zu-Standort-VPN-Verbindung als Sicherung für ExpressRoute konfigurieren. Dies gilt nur für virtuelle Netzwerke, die mit dem privaten Azure-Peeringpfad verknüpft sind. Es gibt keine VPN-basierte Failoverlösung für Dienste, auf die über öffentliche Azure- und Microsoft-Peerings zugegriffen werden. Die ExpressRoute-Verbindung ist immer der primäre Link. Daten durchlaufen den Pfad des Site-to-Site-VPNs nur, wenn bei der ExpressRoute-Verbindung ein Fehler auftritt.

> [!NOTE]
> Die ExpressRoute-Verbindung wird dem Site-to-Site-VPN vorgezogen, doch wenn beide Verbindungen identisch sind, verwendet Azure die längste Präfixübereinstimmung, um die Route zum Ziel des Pakets auszuwählen.
> 
> 

![Koexistenz](media/expressroute-howto-coexist-resource-manager/scenario1.jpg)

### <a name="configure-a-site-to-site-vpn-to-connect-to-sites-not-connected-through-expressroute"></a>Konfigurieren einer Standort-zu-Standort-VPN zum Herstellen einer Verbindung mit Websites, die nicht über ExpressRoute verbunden sind
Sie können Ihr Netzwerk so konfigurieren, dass einige Sites direkt mit Azure über ein Standort-zu-Standort-VPN und andere über ExpressRoute verbunden sind. 

![Koexistenz](media/expressroute-howto-coexist-resource-manager/scenario2.jpg)

> [!NOTE]
> Sie können ein virtuelles Netzwerk nicht als Transitrouter konfigurieren.
> 
> 

## <a name="selecting-the-steps-to-use"></a>Auswählen der zu verwendenden Schritte
Sie haben die Wahl zwischen zwei Vorgehensweisen. Welches Konfigurationsverfahren Sie wählen, hängt davon ab, ob Sie über ein vorhandenes virtuelles Netzwerk verfügen, mit dem Sie eine Verbindung herstellen möchten, oder ob Sie ein neues virtuelles Netzwerk erstellen möchten.

* Ich verfüge nicht über ein VNET und muss eines erstellen.
  
    Wenn Sie noch nicht über ein virtuelles Netzwerk verfügen, werden Sie in diesem Verfahren durch die Schritte zum Erstellen eines neuen virtuellen Netzwerks mithilfe des Resource Manager-Bereitstellungsmodells und zum Erstellen neuer ExpressRoute- sowie Site-to-Site-VPN-Verbindungen geführt. Führen Sie zum Konfigurieren eines virtuellen Netzwerks die Schritte in [So erstellen Sie ein neues virtuelles Netzwerk und parallele Verbindungen](#new) aus.
* Ich verfüge bereits über ein VNET, das auf dem Resource Manager-Bereitstellungsmodell basiert.
  
    Möglicherweise haben Sie bereits ein virtuelles Netzwerk mit einer vorhandenen Standort-zu-Standort-VPN-Verbindung oder einer ExpressRoute-Verbindung. Im Abschnitt [So konfigurieren Sie parallele Verbindungen für ein bereits vorhandenes VNET](#add) werden Sie durch die Schritte zum Löschen des Gateways und zum anschließenden Erstellen neuer ExpressRoute- und Site-to-Site-VPN-Verbindungen geführt. Die Schritte zum Erstellen neuer Verbindungen müssen in einer ganz bestimmten Reihenfolge ausgeführt werden. Verwenden Sie die Anweisungen nicht in anderen Artikeln, um Ihre Gateways und Verbindungen zu erstellen.
  
    In diesem Verfahren müssen Sie zum Erstellen paralleler Verbindungen Ihr Gateway löschen und anschließend neue Gateways konfigurieren. Während der Löschung und Neuerstellung Ihres Gateways und Ihrer Verbindungen kommt es zwar zu Ausfällen bei Ihren standortübergreifenden Verbindungen, Sie müssen jedoch keine Ihrer virtuellen Computer oder Dienste in ein neues virtuelles Netzwerk migrieren. Die VMs und Dienste werden immer noch in der Lage sein, über den Load Balancer zu kommunizieren, während Sie Ihr Gateway konfigurieren, wenn sie zu diesem Zweck konfiguriert sind.

## <a name="new"></a>So erstellen Sie ein neues virtuelles Netzwerk und parallele Verbindungen
Dieses Verfahren führt Sie durch das Erstellen eines VNETs sowie durch das Erstellen paralleler Site-to-Site- und ExpressRoute-Verbindungen.

1. Installieren Sie die neueste Version der Azure PowerShell-Cmdlets. Informationen zum Installieren der Cmdlets finden Sie unter [Installieren und Konfigurieren von Azure PowerShell](/powershell/azure/overview). Für diese Konfiguration werden unter Umständen Cmdlets verwendet, mit denen Sie nicht so vertraut sind. Achten Sie darauf, die in dieser Anleitung angegebenen Cmdlets zu verwenden.
2. Melden Sie sich bei Ihrem Konto an, und richten Sie die Umgebung ein.

  ```powershell
  login-AzureRmAccount
  Select-AzureRmSubscription -SubscriptionName 'yoursubscription'
  $location = "Central US"
  $resgrp = New-AzureRmResourceGroup -Name "ErVpnCoex" -Location $location
  $VNetASN = 65010
  ```
3. Erstellen Sie ein virtuelles Netzwerk mit einem Gatewaysubnetz. Weitere Informationen zur Konfiguration des virtuellen Netzwerks finden Sie unter [Azure Virtual Network-Konfiguration](../virtual-network/virtual-networks-create-vnet-arm-ps.md).
   
   > [!IMPORTANT]
   > Das Gatewaysubnetz muss die Größe /27 haben oder ein kürzeres Präfix aufweisen (z.B. /26 oder /25).
   > 
   > 
   
    Erstellen Sie ein neues VNET.

  ```powershell
  $vnet = New-AzureRmVirtualNetwork -Name "CoexVnet" -ResourceGroupName $resgrp.ResourceGroupName -Location $location -AddressPrefix "10.200.0.0/16"
  ```
   
    Fügen Sie Subnetze hinzu.

  ```powershell
  Add-AzureRmVirtualNetworkSubnetConfig -Name "App" -VirtualNetwork $vnet -AddressPrefix "10.200.1.0/24"
  Add-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet -AddressPrefix "10.200.255.0/24"
  ```
   
    Speichern Sie die VNET-Konfiguration.

  ```powershell
  $vnet = Set-AzureRmVirtualNetwork -VirtualNetwork $vnet
  ```
4. <a name="gw"></a>Erstellen Sie ein ExpressRoute-Gateway. Weitere Informationen zur Konfiguration des ExpressRoute-Gateways finden Sie unter [ExpressRoute-Gatewaykonfiguration](expressroute-howto-add-gateway-resource-manager.md). GatewaySKU muss *Standard*, *HighPerformance* oder *UltraPerformance* lauten.

  ```powershell
  $gwSubnet = Get-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet
  $gwIP = New-AzureRmPublicIpAddress -Name "ERGatewayIP" -ResourceGroupName $resgrp.ResourceGroupName -Location $location -AllocationMethod Dynamic
  $gwConfig = New-AzureRmVirtualNetworkGatewayIpConfig -Name "ERGatewayIpConfig" -SubnetId $gwSubnet.Id -PublicIpAddressId $gwIP.Id
  $gw = New-AzureRmVirtualNetworkGateway -Name "ERGateway" -ResourceGroupName $resgrp.ResourceGroupName -Location $location -IpConfigurations $gwConfig -GatewayType "ExpressRoute" -GatewaySku Standard
  ```
5. Verknüpfen Sie das ExpressRoute-Gateway mit dem ExpressRoute-Schaltkreis. Nachdem dieser Schritt abgeschlossen ist, wird die Verbindung zwischen dem lokalen Netzwerk und Azure durch ExpressRoute eingerichtet. Weitere Informationen zum Verknüpfungsvorgang finden Sie unter [Verknüpfen von VNETs mit ExpressRoute](expressroute-howto-linkvnet-arm.md).

  ```powershell
  $ckt = Get-AzureRmExpressRouteCircuit -Name "YourCircuit" -ResourceGroupName "YourCircuitResourceGroup"
  New-AzureRmVirtualNetworkGatewayConnection -Name "ERConnection" -ResourceGroupName $resgrp.ResourceGroupName -Location $location -VirtualNetworkGateway1 $gw -PeerId $ckt.Id -ConnectionType ExpressRoute
  ```
6. <a name="vpngw"></a>Erstellen Sie als Nächstes Ihr Standort-zu-Standort-VPN-Gateway. Weitere Informationen zur VPN Gateway-Konfiguration finden Sie unter [Erstellen eines VNet mit einer Site-to-Site-Verbindung mithilfe von PowerShell](../vpn-gateway/vpn-gateway-create-site-to-site-rm-powershell.md). GatewaySKU muss *Standard*, *HighPerformance* oder *UltraPerformance* lauten. Für „VpnType“ muss *RouteBased*verwendet werden.

  ```powershell
  $gwSubnet = Get-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet
  $gwIP = New-AzureRmPublicIpAddress -Name "VPNGatewayIP" -ResourceGroupName $resgrp.ResourceGroupName -Location $location -AllocationMethod Dynamic
  $gwConfig = New-AzureRmVirtualNetworkGatewayIpConfig -Name "VPNGatewayIpConfig" -SubnetId $gwSubnet.Id -PublicIpAddressId $gwIP.Id
  New-AzureRmVirtualNetworkGateway -Name "VPNGateway" -ResourceGroupName $resgrp.ResourceGroupName -Location $location -IpConfigurations $gwConfig -GatewayType "Vpn" -VpnType "RouteBased" -GatewaySku "Standard"
  ```
   
    Das Azure-VPN-Gateway unterstützt das BGP-Routingprotokoll. Sie können die AS-Nummer (ASN) für dieses virtuelle Netzwerk angeben, indem Sie den -Asn-Switch im folgenden Befehl hinzufügen. Wenn Sie diesen Parameter nicht angeben, wird standardmäßig die AS-Nummer 65515 verwendet.

  ```powershell
  $azureVpn = New-AzureRmVirtualNetworkGateway -Name "VPNGateway" -ResourceGroupName $resgrp.ResourceGroupName -Location $location -IpConfigurations $gwConfig -GatewayType "Vpn" -VpnType "RouteBased" -GatewaySku "Standard" -Asn $VNetASN
  ```
   
    Sie finden die BGP-Peering-IP und die AS-Nummer, die Azure für das VPN-Gateway verwendet, unter „$azureVpn.BgpSettings.BgpPeeringAddress“ und „$azureVpn.BgpSettings.Asn“. Weitere Informationen finden Sie unter [Konfigurieren von BGP auf Azure VPN Gateways mithilfe von Azure Resource Manager und PowerShell](../vpn-gateway/vpn-gateway-bgp-resource-manager-ps.md) für das Azure VPN Gateway.
7. Erstellen Sie eine VPN-Gateway-Entität für einen lokalen Standort. Mit diesem Befehl wird das lokale VPN-Gateway nicht konfiguriert. Stattdessen können Sie mit ihm die Einstellungen des lokalen Gateway bereitstellen, wie z. B. die öffentliche IP-Adresse und der lokale Adressraum, sodass eine Verbindung mit dem Azure-VPN-Gateway hergestellt werden kann.
   
    Falls Ihr lokales VPN-Gerät nur statisches Routing unterstützt, können Sie die statischen Routen wie folgt konfigurieren:

  ```powershell
  $MyLocalNetworkAddress = @("10.100.0.0/16","10.101.0.0/16","10.102.0.0/16")
  $localVpn = New-AzureRmLocalNetworkGateway -Name "LocalVPNGateway" -ResourceGroupName $resgrp.ResourceGroupName -Location $location -GatewayIpAddress *<Public IP>* -AddressPrefix $MyLocalNetworkAddress
  ```
   
    Falls Ihr lokales VPN-Gerät das BGP unterstützt und Sie das dynamische Routing aktivieren möchten, müssen Sie die BGP-Peering-IP und die AS-Nummer für Ihr lokales VPN-Gerät kennen.

  ```powershell
  $localVPNPublicIP = "<Public IP>"
  $localBGPPeeringIP = "<Private IP for the BGP session>"
  $localBGPASN = "<ASN>"
  $localAddressPrefix = $localBGPPeeringIP + "/32"
  $localVpn = New-AzureRmLocalNetworkGateway -Name "LocalVPNGateway" -ResourceGroupName $resgrp.ResourceGroupName -Location $location -GatewayIpAddress $localVPNPublicIP -AddressPrefix $localAddressPrefix -BgpPeeringAddress $localBGPPeeringIP -Asn $localBGPASN
  ```
8. Konfigurieren Sie Ihr lokales VPN-Gerät für die Verbindung zum neuen Azure VPN Gateway. Weitere Informationen zu VPN-Gerätekonfiguration finden Sie unter [VPN-Gerätekonfiguration](../vpn-gateway/vpn-gateway-about-vpn-devices.md).
9. Verknüpfen Sie das Standort-zu-Standort-VPN-Gateway in Azure mit dem lokalen Gateway.

  ```powershell
  $azureVpn = Get-AzureRmVirtualNetworkGateway -Name "VPNGateway" -ResourceGroupName $resgrp.ResourceGroupName
  New-AzureRmVirtualNetworkGatewayConnection -Name "VPNConnection" -ResourceGroupName $resgrp.ResourceGroupName -Location $location -VirtualNetworkGateway1 $azureVpn -LocalNetworkGateway2 $localVpn -ConnectionType IPsec -SharedKey <yourkey>
  ```

## <a name="add"></a>So konfigurieren Sie parallele Verbindungen für ein bereits vorhandenes VNET
Wenn Sie über ein vorhandenes virtuelles Netzwerk verfügen, prüfen Sie die Größe des Gateway-Subnetzes. Wenn das Gateway-Subnet /28 oder /29 ist, müssen Sie zunächst das Gateway des virtuellen Netzwerks löschen, um die Größe des Gateway-Subnetzes zu erhöhen. Führen Sie dazu die in diesem Abschnitt beschriebenen Schritte aus.

Wenn das Gateway-Subnetz /27 oder größer ist und das virtuelle Netzwerk über ExpressRoute verbunden ist, können Sie die unten beschriebenen Schritte überspringen und direkt mit [„Schritt 6: Erstellen eines Standort-zu-Standort-VPN-Gateways“](#vpngw) (siehe vorheriger Abschnitt) fortfahren. 

> [!NOTE]
> Wenn Sie das vorhandene Gateway löschen, geht Ihre lokale Verbindung mit Ihrem virtuellen Netzwerk verloren, während Sie an dieser Konfiguration arbeiten. 
> 
> 

1. Sie müssen die aktuelle Version der Azure PowerShell-Cmdlets installieren. Weitere Informationen zum Installieren von Cmdlets finden Sie unter [Overview of Azure PowerShell](/powershell/azure/overview) (Übersicht über Azure PowerShell). Für diese Konfiguration werden unter Umständen Cmdlets verwendet, mit denen Sie nicht so vertraut sind. Achten Sie darauf, die in dieser Anleitung angegebenen Cmdlets zu verwenden. 
2. Löschen Sie das vorhandene ExpressRoute- oder Standort-zu-Standort-VPN Gateway.

  ```powershell 
  Remove-AzureRmVirtualNetworkGateway -Name <yourgatewayname> -ResourceGroupName <yourresourcegroup>
  ```
3. Löschen Sie das Gatewaysubnetz.

  ```powershell
  $vnet = Get-AzureRmVirtualNetwork -Name <yourvnetname> -ResourceGroupName <yourresourcegroup> Remove-AzureRmVirtualNetworkSubnetConfig -Name GatewaySubnet -VirtualNetwork $vnet
  ```
4. Fügen Sie ein Gatewaysubnetz hinzu, das mindestens die Größe /27 hat.
   
   > [!NOTE]
   > Wenn in Ihrem virtuellen Netzwerk nicht mehr ausreichend IP-Adressen vorhanden sind, um die Größe des Gateway-Subnetzes zu erhöhen, müssen Sie mehr IP-Adressraum hinzufügen.
   > 
   > 

  ```powershell
  $vnet = Get-AzureRmVirtualNetwork -Name <yourvnetname> -ResourceGroupName <yourresourcegroup>
  Add-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet -AddressPrefix "10.200.255.0/24"
  ```
   
    Speichern Sie die VNET-Konfiguration.

  ```powershell
  $vnet = Set-AzureRmVirtualNetwork -VirtualNetwork $vnet
  ```
5. Sie verfügen nun über ein VNET ohne Gateways. Um neue Gateways zu erstellen und Ihre Verbindungen abzuschließen, können Sie mit [Schritt 4: Erstellen eines ExpressRoute-Gateways](#gw)in den vorangegangenen Schritten fortfahren.

## <a name="to-add-point-to-site-configuration-to-the-vpn-gateway"></a>So fügen Sie dem VPN Gateway eine Punkt-zu-Standort-Konfiguration hinzu
Sie können die unten angegebenen Schritte ausführen, um dem VPN Gateway bei einer parallelen Einrichtung eine Punkt-zu-Standort-Konfiguration hinzuzufügen.

1. Fügen Sie einen VPN-Clientadresspool hinzu.

  ```powershell
  $azureVpn = Get-AzureRmVirtualNetworkGateway -Name "VPNGateway" -ResourceGroupName $resgrp.ResourceGroupName
  Set-AzureRmVirtualNetworkGatewayVpnClientConfig -VirtualNetworkGateway $azureVpn -VpnClientAddressPool "10.251.251.0/24"
  ```
2. Laden Sie das VPN-Stammzertifikat für Ihr VPN Gateway in Azure hoch. In diesem Beispiel wird davon ausgegangen, dass das Stammzertifikat auf dem lokalen Computer gespeichert wird, auf dem die folgenden PowerShell-Cmdlets ausgeführt werden.

  ```powershell
  $p2sCertFullName = "RootErVpnCoexP2S.cer" 
  $p2sCertMatchName = "RootErVpnCoexP2S" 
  $p2sCertToUpload=get-childitem Cert:\CurrentUser\My | Where-Object {$_.Subject -match $p2sCertMatchName} 
  if ($p2sCertToUpload.count -eq 1){write-host "cert found"} else {write-host "cert not found" exit} 
  $p2sCertData = [System.Convert]::ToBase64String($p2sCertToUpload.RawData) Add-AzureRmVpnClientRootCertificate -VpnClientRootCertificateName $p2sCertFullName -VirtualNetworkGatewayname $azureVpn.Name -ResourceGroupName $resgrp.ResourceGroupName -PublicCertData $p2sCertData
  ```

Weitere Informationen zu Punkt-zu-Standort-VPN-Verbindungen finden Sie unter [Konfigurieren einer Punkt-zu-Standort-Verbindung](../vpn-gateway/vpn-gateway-howto-point-to-site-rm-ps.md).

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen über ExpressRoute finden Sie unter [ExpressRoute – FAQ](expressroute-faqs.md).

