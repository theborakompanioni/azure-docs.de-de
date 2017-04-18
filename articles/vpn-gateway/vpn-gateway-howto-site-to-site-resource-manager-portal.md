---
title: 'Herstellen einer Verbindung zwischen Ihrem lokalen Netzwerk und einem virtuellen Azure-Netzwerk: Site-to-Site-VPN: Portal | Microsoft-Dokumentation'
description: "Schritte zum Erstellen einer IPsec-Verbindung zwischen Ihrem lokalen Netzwerk und einem virtuellen Azure-Netzwerk über das öffentliche Internet. Anhand dieser Schritte können Sie über das Portal eine standortübergreifende Site-to-Site-VPN Gateway-Verbindung erstellen."
services: vpn-gateway
documentationcenter: na
author: cherylmc
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 827a4db7-7fa5-4eaf-b7e1-e1518c51c815
ms.service: vpn-gateway
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/04/2017
ms.author: cherylmc
translationtype: Human Translation
ms.sourcegitcommit: 785d3a8920d48e11e80048665e9866f16c514cf7
ms.openlocfilehash: d6f4caebeeced1286f24dd5fcb4f5fc7d8591785
ms.lasthandoff: 04/12/2017


---
# <a name="create-a-site-to-site-connection-in-the-azure-portal"></a>Erstellen einer Site-to-Site-Verbindung im Azure-Portal

Eine VPN Gateway-S2S-Verbindung (Site-to-Site) ist eine Verbindung über einen VPN-Tunnel vom Typ „IPsec/IKE“ (IKEv1 oder IKEv2). Für diese Art von Verbindung wird ein lokales VPN-Gerät benötigt, dem eine öffentliche IP-Adresse zugewiesen ist und das nicht hinter einer NAT-Einheit angeordnet ist. Site-to-Site-Verbindungen können für standortübergreifende Konfigurationen und Hybridkonfigurationen verwendet werden.

![Diagramm für die standortübergreifende Site-to-Site-VPN Gateway-Verbindung](./media/vpn-gateway-howto-site-to-site-resource-manager-portal/site-to-site-diagram.png)

In diesem Artikel werden das Erstellen eines virtuellen Netzwerks und das Herstellen einer Site-to-Site-VPN-Gatewayverbindung mit Ihrem lokalen Netzwerk mithilfe des Azure Resource Manager-Bereitstellungsmodells und des Azure-Portals beschrieben. Sie können diese Konfiguration auch erstellen, indem Sie verschiedene Bereitstellungstools verwenden. Für das klassische Bereitstellungsmodell können Sie in der folgenden Liste eine andere Option wählen:

> [!div class="op_single_selector"]
> * [Resource Manager – Azure-Portal](vpn-gateway-howto-site-to-site-resource-manager-portal.md)
> * [Resource Manager – PowerShell](vpn-gateway-create-site-to-site-rm-powershell.md)
> * [Klassisch – Azure-Portal](vpn-gateway-howto-site-to-site-classic-portal.md)
> * [Klassisch – Klassisches Portal](vpn-gateway-site-to-site-create.md)
>
>


#### <a name="additional-configurations"></a>Zusätzliche Konfigurationen
Wenn Sie VNets miteinander verbinden möchten, aber keine Verbindung mit einem lokalen Standort erstellen, finden Sie unter [Konfigurieren einer VNet-zu-VNet-Verbindung](vpn-gateway-vnet-vnet-rm-ps.md)entsprechende Informationen. Wenn Sie eine Site-to-Site-Verbindung mit einem VNET hinzufügen möchten, das bereits über eine Verbindung verfügt, lesen Sie unter [Hinzufügen einer S2S-Verbindung mit einem VNET mit einer vorhandenen VPN-Gatewayverbindung](vpn-gateway-howto-multi-site-to-site-resource-manager-portal.md) nach.

## <a name="before-you-begin"></a>Voraussetzungen

[!INCLUDE [deployment models](../../includes/vpn-gateway-deployment-models-include.md)]

Vergewissern Sie sich vor Beginn der Konfiguration, dass Sie über Folgendes verfügen:

* Ein kompatibles VPN-Gerät (und eine Person, die es konfigurieren kann). Weitere Informationen finden Sie unter [Informationen zu VPN-Geräten](vpn-gateway-about-vpn-devices.md).
* Falls Sie mit den IP-Adressräumen in Ihrem lokalen Netzwerk nicht vertraut sind, sollten Sie sich an eine Person wenden, die Ihnen diese Details geben kann. Site-to-Site-Verbindungen können keine sich überlappenden Adressräume aufweisen.
* Eine externe öffentliche IP-Adresse für Ihr VPN-Gerät. Diese IP-Adresse darf sich nicht hinter einer NAT befinden.
* Ein Azure-Abonnement. Wenn Sie noch kein Azure-Abonnement besitzen, können Sie Ihre [MSDN-Abonnentenvorteile](http://azure.microsoft.com/pricing/member-offers/msdn-benefits-details) aktivieren oder sich für ein [kostenloses Konto](http://azure.microsoft.com/pricing/free-trial) registrieren.

### <a name="values"></a>Beispielwerte
Sie können die folgenden Beispielwerte nutzen, wenn Sie diese Schritte als Übung verwenden:

* **VNET-Name:** TestVNet1
* **Adressraum:** 
    * 10.11.0.0/16
    * 10.12.0.0/16 (für diese Übung optional)
* **Subnetze:**
  * FrontEnd: 10.11.0.0/24
  * BackEnd: 10.12.0.0/24 (für diese Übung optional)
  * GatewaySubnet: 10.11.255.0/27
* **Ressourcengruppe:** TestRG1
* **Standort:** USA, Osten
* **DNS-Server:** IP-Adresse Ihres DNS-Servers
* **Name des Gateways für virtuelle Netzwerke:** VNet1GW
* **Öffentliche IP-Adresse:** VNet1GWIP
* **VPN-Typ:** Routenbasiert
* **Verbindungstyp:** Site-to-Site (IPsec)
* **Gatewaytyp:** VPN
* **Name des Gateways für das lokale Netzwerk:** Site2
* **Verbindungsname:** VNet1toSite2

## <a name="CreatVNet"></a>1. Erstellen eines virtuellen Netzwerks

[!INCLUDE [vpn-gateway-basic-vnet-rm-portal](../../includes/vpn-gateway-basic-vnet-s2s-rm-portal-include.md)]

## <a name="dns"></a>2. Angeben eines DNS-Servers
DNS ist für Site-to-Site-Verbindungen nicht erforderlich. Wenn Sie für Ressourcen, die in Ihren virtuellen Netzwerken bereitgestellt werden, die Namensauflösung verwenden möchten, müssen Sie aber einen DNS-Server angeben. Diese Einstellung bietet die Möglichkeit, den DNS-Server anzugeben, den Sie zur Namensauflösung für dieses virtuelle Netzwerk verwenden möchten. Mit dieser Einstellung wird kein DNS-Server erstellt.

[!INCLUDE [vpn-gateway-add-dns-rm-portal](../../includes/vpn-gateway-add-dns-rm-portal-include.md)]

## <a name="gatewaysubnet"></a>3. Erstellen eines Gatewaysubnetzes
Sie müssen für Ihr VPN-Gateway ein Gatewaysubnetz erstellen. Das Gatewaysubnetz enthält die IP-Adressen, die von den Diensten des VPN-Gateways verwendet werden. Erstellen Sie nach Möglichkeit ein Gatewaysubnetz, indem Sie einen CIDR-Block vom Typ /28 oder /27 verwenden. So wird sichergestellt, dass Sie über eine ausreichende Anzahl von IP-Adressen für weitere Gatewayfeatures verfügen, die später ggf. hinzugefügt werden.

[!INCLUDE [vpn-gateway-add-gwsubnet-rm-portal](../../includes/vpn-gateway-add-gwsubnet-s2s-rm-portal-include.md)]

## <a name="VNetGateway"></a>4. Erstellen eines Gateways für das virtuelle Netzwerk

[!INCLUDE [vpn-gateway-add-gw-s2s-rm-portal](../../includes/vpn-gateway-add-gw-s2s-rm-portal-include.md)]

## <a name="LocalNetworkGateway"></a>5. Erstellen eines Gateways für das lokale Netzwerk
Mit dem Gateway für das lokale Netzwerk ist Ihr lokaler Standort gemeint. Die Einstellungen, die Sie für das Gateway für das lokale Netzwerk angeben, bestimmen die Adressräume, die an Ihr lokales VPN-Gerät weitergeleitet werden.

[!INCLUDE [vpn-gateway-add-lng-s2s-rm-portal](../../includes/vpn-gateway-add-lng-s2s-rm-portal-include.md)]

## <a name="VPNDevice"></a>6. Konfigurieren des VPN-Geräts
[!INCLUDE [vpn-gateway-configure-vpn-device-rm](../../includes/vpn-gateway-configure-vpn-device-rm-include.md)]

## <a name="CreateConnection"></a>7. Erstellen einer Site-to-Site-VPN-Verbindung

In diesem Schritt erstellen Sie die Site-to-Site-VPN-Verbindung zwischen dem Gateway Ihres virtuellen Netzwerks und Ihrem lokalen VPN-Gerät. Stellen Sie sicher, dass das Gateway für das virtuelle Netzwerk und die Gateways für das lokale Netzwerk fertig erstellt wurden, bevor Sie mit diesem Abschnitt beginnen.

[!INCLUDE [vpn-gateway-add-site-to-site-connection-rm-portal](../../includes/vpn-gateway-add-site-to-site-connection-s2s-rm-portal-include.md)]

## <a name="VerifyConnection"></a>8. Überprüfen der VPN-Verbindung

[!INCLUDE [Azure portal](../../includes/vpn-gateway-verify-connection-portal-rm-include.md)]

## <a name="next-steps"></a>Nächste Schritte
*  Sobald die Verbindung hergestellt ist, können Sie Ihren virtuellen Netzwerken virtuelle Computer hinzufügen. Weitere Informationen finden Sie unter [Virtuelle Computer](https://docs.microsoft.com/azure/#pivot=services&panel=Compute) .
*  Informationen zu BGP finden Sie in der [Übersicht über BGP](vpn-gateway-bgp-overview.md) und unter [Konfigurieren von BGP](vpn-gateway-bgp-resource-manager-ps.md).


