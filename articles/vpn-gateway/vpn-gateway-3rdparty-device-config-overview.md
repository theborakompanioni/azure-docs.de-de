---
title: "Konfiguration von VPN-Drittanbietergeräten für die Verbindung mit Azure-VPN-Gateways | Microsoft-Dokumentation"
description: "Dieser Artikel bietet eine Übersicht über die Konfigurationen von VPN-Drittanbietergeräten für die Verbindung mit Azure-VPN-Gateways."
services: vpn-gateway
documentationcenter: na
author: yushwang
manager: rossort
editor: 
tags: 
ms.assetid: a8bfc955-de49-4172-95ac-5257e262d7ea
ms.service: vpn-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 06/20/2017
ms.author: yushwang
ms.translationtype: HT
ms.sourcegitcommit: d941879aee6042b38b7f5569cd4e31cb78b4ad33
ms.openlocfilehash: 72dab85bb882b05d72cef26bef70437695b70416
ms.contentlocale: de-de
ms.lasthandoff: 07/10/2017


---
# <a name="overview-of-3rd-party-vpn-device-configurations"></a>Übersicht über die Konfigurationen von VPN-Drittanbietergeräten
Dieser Artikel bietet eine Übersicht über die Konfigurationen von lokalen VPN-Geräten für die Verbindung mit Azure-VPN-Gateways. Um sich mit denselben Parametern mit verschiedenen lokalen VPN-Geräten zu verbinden, werden das virtuelle Azure-Beispielnetzwerk und die VPN-Gatewayeinrichtung verwendet.

## <a name="device-requirements"></a>Geräteanforderungen
Azure-VPN-Gateways verwenden für S2S-VPN-Tunnel Standard-IPsec/IKE-Protokollsammlungen. Die ausführlichen PSsec-/IKE-Protokollparameter und die standardmäßigen Kryptoalgorithmen für Azure-VPN-Gateways finden Sie unter [Informationen zu VPN-Geräten](vpn-gateway-about-vpn-devices.md). Sie können die genaue Kombination der Kryptoalgorithmen und Schlüsselstärken für eine spezifische Verbindung angeben. Dies ist optional und wird unter [Informationen zu kryptografischen Anforderungen](vpn-gateway-about-compliance-crypto.md) beschrieben.

## <a name ="singletunnel"></a>Einmaliger VPN-Tunnel
Diese erste Topologie besteht aus einem einmaligen S2S-VPN-Tunnel zwischen dem Azure-VPN-Gateway und Ihrem lokalen VPN-Gerät. Als Option können Sie im VPN-Tunnel BGP konfigurieren.

![Einmaliger Tunnel](./media/vpn-gateway-3rdparty-device-config-overview/singletunnel.png)

Eine ausführliche Anleitung finden Sie unter [Erstellen einer Site-to-Site-Verbindung im Azure-Portal](vpn-gateway-howto-site-to-site-resource-manager-portal.md). Die folgenden Abschnitte enthalten die erforderlichen Parameter sowie ein PowerShell-Beispielskript für Ihre ersten Schritte.

### <a name="network-and-vpn-gateway-information"></a>Netzwerk- und VPN-Gatewayinformationen
In diesem Abschnitt sind die Parameter für das obige Beispiel aufgeführt.

| **Parameter**                | **Wert**                    |
| ---                          | ---                          |
| VNET-Adresspräfixe        | 10.11.0.0/16<br>10.12.0.0/16 |
| IP-Adresse des Azure-VPN-Gateways         | IP-Adresse des Azure-VPN-Gateways         |
| Lokale Adresspräfixe | 10.51.0.0/16<br>10.52.0.0/16 |
| IP-Adresse des lokalen VPN-Geräts    | IP-Adresse des lokalen VPN-Geräts    |
| *VNET-BGP-ASN                | 65010                        |
| *IP-Adresse des Azure-BGP-Peers           | 10.12.255.30                 |
| *Lokale BGP-ASN         | 65050                        |
| *IP-Adresse des lokalen BGP-Peers     | 10.52.255.254                |

* (*) Optionale Parameter nur für BGP

### <a name="sample-powershell-script"></a>PowerShell-Beispielskript
Eine ausführliche Anleitung finden Sie unter [Erstellen eines VNET mit einer Standort-zu-Standort-VPN-Verbindung per PowerShell](vpn-gateway-create-site-to-site-rm-powershell.md). Dieser Abschnitt enthält ein Beispielskript, um Ihnen den Einstieg zu erleichtern.

```powershell
# Declare your variables

$Sub1          = "Replace_With_Your_Subcription_Name"
$RG1           = "TestRG1"
$Location1     = "East US 2"
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
$Connection15  = "VNet1toSite5"
$LNGName5      = "Site5"
$LNGPrefix50   = "10.52.255.254/32"
$LNGPrefix51   = "10.51.0.0/16"
$LNGPrefix52   = "10.52.0.0/16"
$LNGIP5        = "Your_VPN_Device_IP"
$LNGASN5       = 65050
$BGPPeerIP5    = "10.52.255.254"

# Connect to your subscription and create a new resource group

Login-AzureRmAccount
Select-AzureRmSubscription -SubscriptionName $Sub1
New-AzureRmResourceGroup -Name $RG1 -Location $Location1

# Create virtual network

$fesub1 = New-AzureRmVirtualNetworkSubnetConfig -Name $FESubName1 -AddressPrefix $FESubPrefix1 $besub1 = New-AzureRmVirtualNetworkSubnetConfig -Name $BESubName1 -AddressPrefix $BESubPrefix1
$gwsub1 = New-AzureRmVirtualNetworkSubnetConfig -Name $GWSubName1 -AddressPrefix $GWSubPrefix1

New-AzureRmVirtualNetwork -Name $VNetName1 -ResourceGroupName $RG1 -Location $Location1 -AddressPrefix $VNetPrefix11,$VNetPrefix12 -Subnet $fesub1,$besub1,$gwsub1

# Create VPN gateway

$gwpip1    = New-AzureRmPublicIpAddress -Name $GWIPName1 -ResourceGroupName $RG1 -Location $Location1 -AllocationMethod Dynamic
$vnet1     = Get-AzureRmVirtualNetwork -Name $VNetName1 -ResourceGroupName $RG1
$subnet1   = Get-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet1
$gwipconf1 = New-AzureRmVirtualNetworkGatewayIpConfig -Name $GWIPconfName1 -Subnet $subnet1 -PublicIpAddress $gwpip1

New-AzureRmVirtualNetworkGateway -Name $GWName1 -ResourceGroupName $RG1 -Location $Location1 -IpConfigurations $gwipconf1 -GatewayType Vpn -VpnType RouteBased -GatewaySku VpnGw1 -Asn $VNet1ASN

# Create local network gateway

New-AzureRmLocalNetworkGateway -Name $LNGName5 -ResourceGroupName $RG1 -Location $Location1 -GatewayIpAddress $LNGIP5 -AddressPrefix $LNGPrefix51,$LNGPrefix52 -Asn $LNGASN5 -BgpPeeringAddress $BGPPeerIP5

# Create the S2S VPN connection

$vnet1gw = Get-AzureRmVirtualNetworkGateway -Name $GWName1  -ResourceGroupName $RG1
$lng5gw  = Get-AzureRmLocalNetworkGateway -Name $LNGName5 -ResourceGroupName $RG1

New-AzureRmVirtualNetworkGatewayConnection -Name $Connection15 -ResourceGroupName $RG1 -VirtualNetworkGateway1 $vnet1gw -LocalNetworkGateway2 $lng5gw -Location $Location1 -ConnectionType IPsec -SharedKey 'AzureA1b2C3' -EnableBGP $False
```

### <a name ="policybased"></a>[Optional] Verwenden der individuelle IPsec/IKE-Richtlinie mit „UsePolicyBasedTrafficSelectors“
Wenn Ihre VPN-Geräte keine „Any-to-any-Datenverkehrsselektoren“ unterstützen (routenbasierte/VTI-basierte Konfiguration), müssen Sie eine individuelle IPsec/IKE-Richtlinie erstellen und die Option „UsePolicyBasedTrafficSelectors“ verwenden, wie es [in diesem Artikel](vpn-gateway-connect-multiple-policybased-rm-ps.md) beschrieben wird.

> [!IMPORTANT]
> Sie müssen eine IPsec/IKE-Richtlinie erstellen, um für die Verbindung die Option „UsePolicyBasedTrafficSelectors“ zu aktivieren.

Im Beispielskript unten wird eine IPsec/IKE-Richtlinie mit den folgenden Algorithmen und Parametern erstellt:
* IKEv2: AES256, SHA384, DHGroup24
* IPsec: AES256, SHA1, PFS24, SA Gültigkeitsdauer 7200 Sekunden & 20480000KB (20GB)

Dies wendet die Richtlinie an und aktiviert „UsePolicyBasedTrafficSelectors“ bei der Verbindung.

```powershell
$ipsecpolicy5 = New-AzureRmIpsecPolicy -IkeEncryption AES256 -IkeIntegrity SHA384 -DhGroup DHGroup24 -IpsecEncryption AES256 -IpsecIntegrity SHA1 -PfsGroup PFS24 -SALifeTimeSeconds 7200 -SADataSizeKilobytes 20480000

$vnet1gw = Get-AzureRmVirtualNetworkGateway -Name $GWName1  -ResourceGroupName $RG1
$lng5gw  = Get-AzureRmLocalNetworkGateway -Name $LNGName5 -ResourceGroupName $RG1

New-AzureRmVirtualNetworkGatewayConnection -Name $Connection15 -ResourceGroupName $RG1 -VirtualNetworkGateway1 $vnet1gw -LocalNetworkGateway2 $lng5gw -Location $Location1 -ConnectionType IPsec -SharedKey 'AzureA1b2C3' -EnableBGP $False -IpsecPolicies $ipsecpolicy5 -UsePolicyBasedTrafficSelectors $True
```

### <a name ="bgp"></a>[Optional] Verwenden einer BGB-S2S-Verbindung
Sie können auch BGP bei der Verbindung verwenden. Dies wird in [BGP für ein VPN-Gateway](vpn-gateway-bgp-resource-manager-ps.md) beschrieben. Es gibt zwei Unterschiede:

Bei den lokalen Adresspräfixen kann es sich um eine einzelne Hostadresse handeln, nämlich um die lokale BGP-Peer-IP-Adresse:

```powershell
New-AzureRmLocalNetworkGateway -Name $LNGName5 -ResourceGroupName $RG1 -Location $Location1 -GatewayIpAddress $LNGIP5 -AddressPrefix $LNGPrefix50 -Asn $LNGASN5 -BgpPeeringAddress $BGPPeerIP5
```

Beim Verbindungsaufbau müssen Sie „-EnableBGP“ auf $True setzen:

```powershell
New-AzureRmVirtualNetworkGatewayConnection -Name $Connection15 -ResourceGroupName $RG1 -VirtualNetworkGateway1 $vnet1gw -LocalNetworkGateway2 $lng5gw -Location $Location1 -ConnectionType IPsec -SharedKey 'AzureA1b2C3' -EnableBGP $True
```

## <a name="next-steps"></a>Nächste Schritte
Die Schritte zum Konfigurieren von standortübergreifenden Verbindungen und VNet-zu-VNet-Verbindungen vom Typ „Aktiv/Aktiv“ finden Sie unter [Konfigurieren von Aktiv/Aktiv-VPN-Gateways für standortübergreifende und VNet-zu-VNet-Verbindungen](vpn-gateway-activeactive-rm-powershell.md).


