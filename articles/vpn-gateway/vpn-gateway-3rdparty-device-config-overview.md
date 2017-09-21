---
title: "Konfigurationen für VPN-Partnergeräte für das Verbinden mit Azure-VPN-Gateways | Microsoft-Dokumentation"
description: "Dieser Artikel bietet eine Übersicht über die Konfigurationen von VPN-Partnergeräten für die Verbindung mit Azure-VPN-Gateways."
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
ms.sourcegitcommit: 2c6cf0eff812b12ad852e1434e7adf42c5eb7422
ms.openlocfilehash: b3806d16d3b78347e183ecbd2ab5a463a2142110
ms.contentlocale: de-de
ms.lasthandoff: 09/13/2017

---
# <a name="overview-of-partner-vpn-device-configurations"></a>Übersicht über Konfigurationen von VPN-Partnergeräten
Dieser Artikel bietet eine Übersicht über das Konfigurieren von lokalen VPN-Geräten für die Verbindung mit Azure-VPN-Gateways. Um sich mit denselben Parametern mit verschiedenen lokalen VPN-Geräten zu verbinden, werden ein virtuelles Azure-Beispielnetzwerk und die VPN-Gatewayeinrichtung verwendet.

## <a name="device-requirements"></a>Geräteanforderungen
Azure-VPN-Gateways verwenden für Site-to-Site-VPN-Tunnel (S2S) Standard-IPsec/IKE-Protokollsammlungen. Eine Liste der IPsec-/IKE-Parameter und die Kryptografiealgorithmen für Azure-VPN-Gateways finden Sie unter [Informationen zu VPN-Geräten](vpn-gateway-about-vpn-devices.md). Sie können auch die genauen Algorithmen und Schlüsselstärken für eine spezifische Verbindung angeben. Dies wird unter [Informationen zu kryptografischen Anforderungen](vpn-gateway-about-compliance-crypto.md) beschrieben.

## <a name ="singletunnel"></a>Einmaliger VPN-Tunnel
Die erste Konfiguration im Beispiel besteht aus einem einmaligen Site-to-Site-VPN-Tunnel zwischen dem Azure-VPN-Gateway und einem lokalen VPN-Gerät. Als Option können Sie das [BGP (Border Gateway Protocol) für den VPN-Tunnel](#bgp) konfigurieren.

![Diagramm eines einzelnen S2S-VPN-Tunnels](./media/vpn-gateway-3rdparty-device-config-overview/singletunnel.png)

Ausführliche Anleitungen zum Einrichten eines VPN-Tunnels finden Sie unter [Konfigurieren einer Site-to-Site-Verbindung](vpn-gateway-howto-site-to-site-resource-manager-portal.md). In den folgenden Abschnitten werden die Verbindungsparameter für die Beispielkonfiguration angegeben und ein PowerShell-Skript bereitgestellt, um Ihnen beim Einstieg zu helfen.

### <a name="connection-parameters"></a>Verbindungsparameter
In diesem Abschnitt werden die Parameter für die Beispiele aufgelistet, die in den vorherigen Abschnitten beschrieben werden.

| **Parameter**                | **Wert**                    |
| ---                          | ---                          |
| Adresspräfixe des virtuellen Netzwerks        | 10.11.0.0/16<br>10.12.0.0/16 |
| IP-Adresse des Azure-VPN-Gateways         | IP-Adresse des Azure-VPN-Gateways         |
| Lokale Adresspräfixe | 10.51.0.0/16<br>10.52.0.0/16 |
| IP-Adresse des lokalen VPN-Geräts    | IP-Adresse des lokalen VPN-Geräts    |
| * BGP-ASN des virtuellen Netzwerks                | 65010                        |
| * IP-Adresse des Azure-BGP-Peers           | 10.12.255.30                 |
| * Lokale BGP-ASN         | 65050                        |
| * IP-Adresse des lokalen BGP-Peers     | 10.52.255.254                |

\* Optionale Parameter nur für BGP

### <a name="sample-powershell-script"></a>PowerShell-Beispielskript
Dieser Abschnitt enthält ein Beispielskript, um Ihnen den Einstieg zu erleichtern. Eine ausführliche Anleitung finden Sie unter [Erstellen eines VNET mit einer Site-to-Site-VPN-Verbindung mithilfe von PowerShell](vpn-gateway-create-site-to-site-rm-powershell.md).

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

### <a name ="policybased"></a>(Optional:) Verwenden einer benutzerdefinierten IPsec/IKE-Richtlinie mit UsePolicyBasedTrafficSelectors
Wenn Ihre VPN-Geräte keine „Any-to-any-Datenverkehrsselektoren“ unterstützen (wie z.B. routenbasierte oder VTI-basierte Konfiguration), erstellen Sie eine benutzerdefinierte IPsec/IKE-Richtlinie mit der Option [UsePolicyBasedTrafficSelectors](vpn-gateway-connect-multiple-policybased-rm-ps.md).

> [!IMPORTANT]
> Sie müssen eine IPsec/IKE-Richtlinie erstellen, um für die Verbindung die Option **UsePolicyBasedTrafficSelectors** zu aktivieren.


Im Beispielskript wird eine IPsec/IKE-Richtlinie mit den folgenden Algorithmen und Parametern erstellt:
* IKEv2: AES256, SHA384, DHGroup24
* IPsec: AES256, SHA1, PFS24, SA-Lebensdauer 7.200 Sekunden und 20.480.000 KB (20 GB)

Das Skript wendet eine IPsec/IKE-Richtlinie an und aktiviert für die Verbindung die Option **UsePolicyBasedTrafficSelectors**.

```powershell
$ipsecpolicy5 = New-AzureRmIpsecPolicy -IkeEncryption AES256 -IkeIntegrity SHA384 -DhGroup DHGroup24 -IpsecEncryption AES256 -IpsecIntegrity SHA1 -PfsGroup PFS24 -SALifeTimeSeconds 7200 -SADataSizeKilobytes 20480000

$vnet1gw = Get-AzureRmVirtualNetworkGateway -Name $GWName1  -ResourceGroupName $RG1
$lng5gw  = Get-AzureRmLocalNetworkGateway -Name $LNGName5 -ResourceGroupName $RG1

New-AzureRmVirtualNetworkGatewayConnection -Name $Connection15 -ResourceGroupName $RG1 -VirtualNetworkGateway1 $vnet1gw -LocalNetworkGateway2 $lng5gw -Location $Location1 -ConnectionType IPsec -SharedKey 'AzureA1b2C3' -EnableBGP $False -IpsecPolicies $ipsecpolicy5 -UsePolicyBasedTrafficSelectors $True
```

### <a name ="bgp"></a>(Optional:) Verwenden von BGB bei S2S-VPN-Verbindungen
Wenn Sie die S2S-VPN-Verbindung erstellen, können Sie optional [BGP für das VPN-Gateway](vpn-gateway-bgp-resource-manager-ps.md) verwenden. Dieser Ansatz weist zwei Unterschiede auf:

* Bei den lokalen Adresspräfixen kann es sich um eine einzelne Hostadresse handeln. Die lokale BGP-Peer-IP-Adresse wird wie folgt angegeben:

    ```powershell
    New-AzureRmLocalNetworkGateway -Name $LNGName5 -ResourceGroupName $RG1 -Location $Location1 -GatewayIpAddress $LNGIP5 -AddressPrefix $LNGPrefix50 -Asn $LNGASN5 -BgpPeeringAddress $BGPPeerIP5
    ```

* Wenn Sie die Verbindung erstellen, müssen Sie die Option **-EnableBGP** auf „$True“ festlegen:

    ```powershell
    New-AzureRmVirtualNetworkGatewayConnection -Name $Connection15 -ResourceGroupName $RG1 -VirtualNetworkGateway1 $vnet1gw -LocalNetworkGateway2 $lng5gw -Location $Location1 -ConnectionType IPsec -SharedKey 'AzureA1b2C3' -EnableBGP $True
    ```

## <a name="next-steps"></a>Nächste Schritte
Eine ausführliche Anleitung zum Einrichten von Aktiv/Aktiv-VPN-Gateways finden Sie unter [Konfigurieren von Aktiv/Aktiv-VPN-Gateways für standortübergreifende und VNET-zu-VNET-Verbindungen](vpn-gateway-activeactive-rm-powershell.md).


