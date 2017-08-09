---
title: "Konfigurieren der Tunnelerzwingung für Azure-Site-to-Site-Verbindungen: Resource Manager | Microsoft-Dokumentation"
description: "Gewusst wie: „Erzwingen“ der Umleitung des gesamten Internetdatenverkehrs an Ihren lokalen Standort."
services: vpn-gateway
documentationcenter: na
author: cherylmc
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: cbe58db8-b598-4c9f-ac88-62c865eb8721
ms.service: vpn-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/31/2017
ms.author: cherylmc
ms.translationtype: HT
ms.sourcegitcommit: 79bebd10784ec74b4800e19576cbec253acf1be7
ms.openlocfilehash: 207c53924863eb51ee369fe46d5ad12fb1905c53
ms.contentlocale: de-de
ms.lasthandoff: 08/03/2017

---
# <a name="configure-forced-tunneling-using-the-azure-resource-manager-deployment-model"></a>Konfigurieren der Tunnelerzwingung mit dem Azure Resource Manager-Bereitstellungsmodell

Über die Tunnelerzwingung können Sie die Umleitung des gesamten Internetdatenverkehrs an Ihren lokalen Standort „erzwingen“. Sie verwenden dazu einen Standort-zu-Standort-VPN-Tunnel für die Kontrolle und Überwachung. Dies ist eine wichtige Sicherheitsvoraussetzung der IT-Richtlinien für die meisten Unternehmen. Ohne die Tunnelerzwingung wird der Datenverkehr Ihrer virtuellen Computer zum Internet in Azure immer direkt von der Azure-Netzwerkinfrastruktur an das Internet geleitet, ohne dass Sie die Möglichkeit haben, diesen zu überprüfen oder zu überwachen. Nicht autorisierter Zugriff auf das Internet kann potenziell zur Offenlegung von Informationen oder anderen Arten von Sicherheitsverletzungen führen.

[!INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-classic-rm-include.md)] 

Dieser Artikel beschreibt die Konfiguration der Tunnelerzwingung für virtuelle Netzwerke, die mit dem Resource Manager-Bereitstellungsmodell erstellt wurden. Das erzwungene Tunneling kann mithilfe von PowerShell konfiguriert werden, nicht über das Portal. Wenn Sie das erzwungene Tunneling für das klassische Bereitstellungsmodell konfigurieren möchten, wählen Sie aus der folgenden Dropdownliste den klassischen Artikel aus:

> [!div class="op_single_selector"]
> * [PowerShell – klassisch](vpn-gateway-about-forced-tunneling.md)
> * [PowerShell – Resource Manager](vpn-gateway-forced-tunneling-rm.md)
> 
> 

## <a name="about-forced-tunneling"></a>Informationen zur Tunnelerzwingung

Das folgende Diagramm veranschaulicht die Funktionsweise der Tunnelerzwingung. 

![Tunnelerzwingung](./media/vpn-gateway-forced-tunneling-rm/forced-tunnel.png)

Im obigen Beispiel wird für das Frontend-Subnetz kein Tunneln erzwungen. Die Workloads im Frontend-Subnetz können weiterhin Kundenanfragen direkt aus dem Internet akzeptieren und darauf reagieren. Für die Subnetze "Midtier" und "Backend" wird das Tunneln erzwungen. Bei allen ausgehenden Verbindungen mit dem Internet aus diesen zwei Subnetzen wird eine Umleitung an einen lokalen Standort über einen der S2S-VPN-Tunnel erzwungen.

Dadurch können Sie den Internetzugriff über die virtuellen Computer oder Clouddienste in Azure einschränken und überprüfen, während die erforderliche mehrschichtige Dienstarchitektur weiterhin in Betrieb bleibt. Wenn in diesen keine Workloads mit Internetverbindung erforderlich sind, können Sie das erzwungene Tunneln auch auf alle virtuellen Netzwerke anwenden.

## <a name="requirements-and-considerations"></a>Anforderungen und Überlegungen

Die Tunnelerzwingung in Azure wird über benutzerdefinierte Routen im virtuellen Netzwerk konfiguriert. Das Umleiten von Datenverkehr an einen lokalen Standort wird als eine Standardroute zum Azure-VPN-Gateway umgesetzt. Weitere Informationen zu benutzerdefiniertem Routing und virtuellen Netzwerken finden Sie unter [Was sind benutzerdefinierte Routen und IP-Weiterleitung?](../virtual-network/virtual-networks-udr-overview.md).

* Jedes Subnetz des virtuellen Netzwerks verfügt über eine integrierte Systemroutingtabelle. Die Systemroutingtabelle verfügt über die folgenden drei Gruppen von Routen:
  
  * **Lokale VNET-Routen:** direkt zu den virtuellen Zielcomputern im gleichen virtuellen Netzwerk.
  * **Lokale Routen:** zum Azure VPN Gateway.
  * **Standardroute:** direkt zum Internet. Pakete an private IP-Adressen, die nicht durch die vorherigen beiden Routen abgedeckt sind, werden verworfen.
* In diesem Verfahren werden benutzerdefinierte Routen (User Defined Routes, UDR) verwendet, um eine Routingtabelle zu erstellen, damit eine Standardroute hinzugefügt wird. Anschließend verknüpfen Sie die Routingtabelle mit den VNet-Subnetzen, um die Tunnelerzwingung in diesen Subnetzen zu aktivieren.
* Die Tunnelerzwingung muss einem VNet zugeordnet werden, das über ein routenbasiertes VPN-Gateway verfügt. Sie müssen einen "Standardstandort" unter den standortübergreifenden lokalen Standorten auswählen, der mit dem virtuellen Netzwerk verbunden ist. Darüber hinaus muss das lokale VPN-Gerät für die Verwendung von „0.0.0.0/0“ als Datenverkehrsauswahl konfiguriert werden. 
* Die ExpressRoute-Tunnelerzwingung kann über diesen Mechanismus nicht konfiguriert werden. Sie wird stattdessen durch Anfordern einer Standardroute über die ExpressRoute-BGP-Peeringsitzungen aktiviert. Weitere Informationen finden Sie in der [Dokumentation zu ExpressRoute](https://azure.microsoft.com/documentation/services/expressroute/).

## <a name="configuration-overview"></a>Konfigurationsübersicht

Das folgende Verfahren unterstützt Sie beim Erstellen einer Ressourcengruppe und eines VNets. Anschließend erstellen Sie ein VPN-Gateway und konfigurieren die Tunnelerzwingung. In dieser Vorgehensweise verfügt das virtuelle Netzwerk „MultiTier-VNet“ über die drei Subnetze „Frontend“, „Midtier“ und „Backend“ mit vier standortübergreifenden Verbindungen: „DefaultSiteHQ“ und drei Verzweigungen.

Mithilfe der Schritte wird festgelegt, dass „DefaultSiteHQ“ die Standard-Standortverbindung für die Tunnelerzwingung ist und die Subnetze „Midtier“ und „Backend“ die Tunnelerzwingung verwenden müssen.

## <a name="before-you-begin"></a>Voraussetzungen

Installieren Sie die aktuelle Version der PowerShell-Cmdlets für Azure Resource Manager. Weitere Informationen zur Installation der PowerShell-Cmdlets finden Sie unter [Installieren und Konfigurieren von Azure PowerShell](/powershell/azure/overview) .

### <a name="to-log-in"></a>Zum Anmelden

[!INCLUDE [To log in](../../includes/vpn-gateway-ps-login-include.md)]

## <a name="configure-forced-tunneling"></a>Konfigurieren der Tunnelerzwingung

1. Erstellen Sie eine Ressourcengruppe.

  ```powershell
  New-AzureRmResourceGroup -Name 'ForcedTunneling' -Location 'North Europe'
  ```
2. Erstellen Sie ein virtuelles Netzwerks, und geben Sie Subnetze an.

  ```powershell 
  $s1 = New-AzureRmVirtualNetworkSubnetConfig -Name "Frontend" -AddressPrefix "10.1.0.0/24"
  $s2 = New-AzureRmVirtualNetworkSubnetConfig -Name "Midtier" -AddressPrefix "10.1.1.0/24"
  $s3 = New-AzureRmVirtualNetworkSubnetConfig -Name "Backend" -AddressPrefix "10.1.2.0/24"
  $s4 = New-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -AddressPrefix "10.1.200.0/28"
  $vnet = New-AzureRmVirtualNetwork -Name "MultiTier-VNet" -Location "North Europe" -ResourceGroupName "ForcedTunneling" -AddressPrefix "10.1.0.0/16" -Subnet $s1,$s2,$s3,$s4
  ```
3. Erstellen Sie die lokalen Netzwerkgateways.

  ```powershell
  $lng1 = New-AzureRmLocalNetworkGateway -Name "DefaultSiteHQ" -ResourceGroupName "ForcedTunneling" -Location "North Europe" -GatewayIpAddress "111.111.111.111" -AddressPrefix "192.168.1.0/24"
  $lng2 = New-AzureRmLocalNetworkGateway -Name "Branch1" -ResourceGroupName "ForcedTunneling" -Location "North Europe" -GatewayIpAddress "111.111.111.112" -AddressPrefix "192.168.2.0/24"
  $lng3 = New-AzureRmLocalNetworkGateway -Name "Branch2" -ResourceGroupName "ForcedTunneling" -Location "North Europe" -GatewayIpAddress "111.111.111.113" -AddressPrefix "192.168.3.0/24"
  $lng4 = New-AzureRmLocalNetworkGateway -Name "Branch3" -ResourceGroupName "ForcedTunneling" -Location "North Europe" -GatewayIpAddress "111.111.111.114" -AddressPrefix "192.168.4.0/24"
  ```
4. Erstellen Sie die Routingtabelle und die Routingregel.

  ```powershell
  New-AzureRmRouteTable –Name "MyRouteTable" -ResourceGroupName "ForcedTunneling" –Location "North Europe"
  $rt = Get-AzureRmRouteTable –Name "MyRouteTable" -ResourceGroupName "ForcedTunneling" 
  Add-AzureRmRouteConfig -Name "DefaultRoute" -AddressPrefix "0.0.0.0/0" -NextHopType VirtualNetworkGateway -RouteTable $rt
  Set-AzureRmRouteTable -RouteTable $rt
  ```
5. Ordnen Sie die Routingtabelle den Subnetzen „Midtier“ und „Backend“ zu.

  ```powershell
  $vnet = Get-AzureRmVirtualNetwork -Name "MultiTier-Vnet" -ResourceGroupName "ForcedTunneling"
  Set-AzureRmVirtualNetworkSubnetConfig -Name "MidTier" -VirtualNetwork $vnet -AddressPrefix "10.1.1.0/24" -RouteTable $rt
  Set-AzureRmVirtualNetworkSubnetConfig -Name "Backend" -VirtualNetwork $vnet -AddressPrefix "10.1.2.0/24" -RouteTable $rt
  Set-AzureRmVirtualNetwork -VirtualNetwork $vnet
  ```
6. Erstellen Sie das Gateway mit einem Standardstandort. Dieser Schritt dauert einige Zeit (manchmal 45 Minuten oder länger), da Sie das Gateway erstellen und konfigurieren.<br> Der Cmdlet-Parameter **-GatewayDefaultSite** sorgt dafür, dass die erzwungene Routingkonfiguration funktioniert. Achten Sie daher darauf, diese Einstellung korrekt zu konfigurieren. Dieser Parameter ist ab PowerShell 1.0 verfügbar.

  ```powershell
  $pip = New-AzureRmPublicIpAddress -Name "GatewayIP" -ResourceGroupName "ForcedTunneling" -Location "North Europe" -AllocationMethod Dynamic
  $gwsubnet = Get-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet
  $ipconfig = New-AzureRmVirtualNetworkGatewayIpConfig -Name "gwIpConfig" -SubnetId $gwsubnet.Id -PublicIpAddressId $pip.Id
  New-AzureRmVirtualNetworkGateway -Name "Gateway1" -ResourceGroupName "ForcedTunneling" -Location "North Europe" -IpConfigurations $ipconfig -GatewayType Vpn -VpnType RouteBased -GatewaySku VpnGw1 -GatewayDefaultSite $lng1 -EnableBgp $false
  ```
7. Richten Sie die Standort-zu-Standort-VPN-Verbindungen ein.

  ```powershell
  $gateway = Get-AzureRmVirtualNetworkGateway -Name "Gateway1" -ResourceGroupName "ForcedTunneling"
  $lng1 = Get-AzureRmLocalNetworkGateway -Name "DefaultSiteHQ" -ResourceGroupName "ForcedTunneling" 
  $lng2 = Get-AzureRmLocalNetworkGateway -Name "Branch1" -ResourceGroupName "ForcedTunneling" 
  $lng3 = Get-AzureRmLocalNetworkGateway -Name "Branch2" -ResourceGroupName "ForcedTunneling" 
  $lng4 = Get-AzureRmLocalNetworkGateway -Name "Branch3" -ResourceGroupName "ForcedTunneling" 
    
  New-AzureRmVirtualNetworkGatewayConnection -Name "Connection1" -ResourceGroupName "ForcedTunneling" -Location "North Europe" -VirtualNetworkGateway1 $gateway -LocalNetworkGateway2 $lng1 -ConnectionType IPsec -SharedKey "preSharedKey"
  New-AzureRmVirtualNetworkGatewayConnection -Name "Connection2" -ResourceGroupName "ForcedTunneling" -Location "North Europe" -VirtualNetworkGateway1 $gateway -LocalNetworkGateway2 $lng2 -ConnectionType IPsec -SharedKey "preSharedKey"
  New-AzureRmVirtualNetworkGatewayConnection -Name "Connection3" -ResourceGroupName "ForcedTunneling" -Location "North Europe" -VirtualNetworkGateway1 $gateway -LocalNetworkGateway2 $lng3 -ConnectionType IPsec -SharedKey "preSharedKey"
  New-AzureRmVirtualNetworkGatewayConnection -Name "Connection4" -ResourceGroupName "ForcedTunneling" -Location "North Europe" -VirtualNetworkGateway1 $gateway -LocalNetworkGateway2 $lng4 -ConnectionType IPsec -SharedKey "preSharedKey"
    
  Get-AzureRmVirtualNetworkGatewayConnection -Name "Connection1" -ResourceGroupName "ForcedTunneling"
  ```
