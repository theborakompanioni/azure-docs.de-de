---
title: Erstellen eines virtuellen Netzwerks mit einer Site-to-Site-VPN-Verbindung mit Azure Resource Manager und dem Azure-Portal | Microsoft Docs
description: "Hier wird beschrieben, wie Sie ein VNet mit dem Resource Manager-Bereitstellungsmodell erstellen und dafür eine Verbindung mit dem lokalen Netzwerk über eine S2S-VPN Gateway-Verbindung herstellen."
services: vpn-gateway
documentationcenter: na
author: cherylmc
manager: carmonm
editor: 
tags: azure-resource-manager
ms.assetid: 827a4db7-7fa5-4eaf-b7e1-e1518c51c815
ms.service: vpn-gateway
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 10/14/2016
ms.author: cherylmc
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: 95268d0197f9c79b1650165056cbf4b3e79f12ec


---
# <a name="create-a-vnet-with-a-sitetosite-connection-using-the-azure-portal"></a>Erstellen eines VNet mit einer Standort-zu-Standort-Verbindung über das Azure-Portal
> [!div class="op_single_selector"]
> * [Resource Manager – Azure-Portal](vpn-gateway-howto-site-to-site-resource-manager-portal.md)
> * [Resource Manager – PowerShell](vpn-gateway-create-site-to-site-rm-powershell.md)
> * [Klassisch – Klassisches Portal](vpn-gateway-site-to-site-create.md)
> 
> 

In diesem Artikel werden das Erstellen eines virtuellen Netzwerks und das Herstellen einer Site-to-Site-VPN-Gatewayverbindung mit Ihrem lokalen Netzwerk mithilfe des Azure Resource Manager-Bereitstellungsmodells und des Azure-Portals beschrieben. Site-to-Site-Verbindungen können für standortübergreifende Konfigurationen und Hybridkonfigurationen verwendet werden.

![Diagramm](./media/vpn-gateway-howto-site-to-site-resource-manager-portal/s2srmportal.png)

### <a name="deployment-models-and-methods-for-sitetosite-connections"></a>Bereitstellungsmodelle und -methoden für Site-to-Site-Verbindungen
[!INCLUDE [deployment models](../../includes/vpn-gateway-deployment-models-include.md)]

Die folgende Tabelle zeigt die derzeit verfügbaren Bereitstellungsmodelle und -methoden für Site-to-Site-Konfigurationen. Falls ein Artikel mit Konfigurationsschritten verfügbar ist, steht in der Tabelle ein direkter Link zur Verfügung.

[!INCLUDE [site-to-site table](../../includes/vpn-gateway-table-site-to-site-include.md)]

#### <a name="additional-configurations"></a>Zusätzliche Konfigurationen
Wenn Sie VNets miteinander verbinden möchten, aber keine Verbindung mit einem lokalen Standort erstellen, finden Sie unter [Konfigurieren einer VNet-zu-VNet-Verbindung](vpn-gateway-vnet-vnet-rm-ps.md)entsprechende Informationen. Wenn Sie eine Site-to-Site-Verbindung mit einem VNET hinzufügen möchten, das bereits über eine Verbindung verfügt, lesen Sie unter [Hinzufügen einer S2S-Verbindung mit einem VNET mit einer vorhandenen VPN-Gatewayverbindung](vpn-gateway-howto-multi-site-to-site-resource-manager-portal.md) nach.

## <a name="before-you-begin"></a>Voraussetzungen
Vergewissern Sie sich vor Beginn der Konfiguration, dass Sie über Folgendes verfügen:

* Ein kompatibles VPN-Gerät (und eine Person, die es konfigurieren kann). Weitere Informationen finden Sie unter [Informationen zu VPN-Geräten](vpn-gateway-about-vpn-devices.md). Wenn Sie sich mit dem Konfigurieren des VPN-Geräts oder mit den IP-Adressbereichen Ihrer lokalen Netzwerkkonfiguration nicht auskennen, müssen Sie sich an eine Person wenden, die Ihnen diese Details liefern kann.
* Eine externe öffentliche IP-Adresse für Ihr VPN-Gerät. Diese IP-Adresse darf sich nicht hinter einer NAT befinden.
* Ein Azure-Abonnement. Wenn Sie noch kein Azure-Abonnement besitzen, können Sie Ihre [MSDN-Abonnentenvorteile](http://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) aktivieren oder sich für ein [kostenloses Konto](http://azure.microsoft.com/pricing/free-trial/) registrieren.

### <a name="a-namevaluesasample-configuration-values-for-this-exercise"></a><a name="values"></a>Beispielkonfigurationswerte für diese Übung
Sie können die Beispielkonfigurationswerte nutzen, wenn Sie diese Schritte als Übung verwenden:

* **VNET-Name:** TestVNet1
* **Adressraum:** 10.11.0.0/16 und 10.12.0.0/16
* **Subnetze:**
  * FrontEnd: 10.11.0.0/24
  * BackEnd: 10.12.0.0/24
  * GatewaySubnet: 10.12.255.0/27
* **Ressourcengruppe:** TestRG1
* **Standort:** USA, Osten
* **DNS-Server:** 8.8.8.8
* **Gatewayname:** VNet1GW
* **Öffentliche IP-Adresse:** VNet1GWIP
* **VPN-Typ:** Routenbasiert
* **Verbindungstyp:** Site-to-Site (IPsec)
* **Gatewaytyp:** VPN
* **Name des Gateways für das lokale Netzwerk:** Site2
* **Verbindungsname:** VNet1toSite2

## <a name="a-namecreatvneta1-create-a-virtual-network"></a><a name="CreatVNet"></a>1. Erstellen eines virtuellen Netzwerks
Wenn Sie bereits über ein VNet verfügen, sollten Sie überprüfen, ob die Einstellungen mit Ihrem Entwurf des VPN Gateways kompatibel sind. Achten Sie besonders auf Subnetze, die sich unter Umständen mit anderen Netzwerken überlappen. Bei überlappenden Subnetzen funktioniert die Verbindung nicht einwandfrei. Wenn Ihr VNet mit den richtigen Einstellungen konfiguriert wurde, können Sie mit den Schritten im Abschnitt [Angeben eines DNS-Servers](#dns) beginnen.

### <a name="to-create-a-virtual-network"></a>So erstellen Sie ein virtuelles Netzwerk
[!INCLUDE [vpn-gateway-basic-vnet-rm-portal](../../includes/vpn-gateway-basic-vnet-rm-portal-include.md)]

## <a name="a-namesubnetsa2-add-additional-address-space-and-subnets"></a><a name="subnets"></a>2. Hinzufügen weiterer Adressräume und Subnetze
Sie können dem VNet nach dem Erstellen weitere Adressräume und Subnetze hinzufügen.

[!INCLUDE [vpn-gateway-additional-address-space](../../includes/vpn-gateway-additional-address-space-include.md)]

## <a name="a-namednsa3-specify-a-dns-server"></a><a name="dns"></a>3. Angeben eines DNS-Servers
### <a name="to-specify-a-dns-server"></a>So geben Sie einen DNS-Server an
[!INCLUDE [vpn-gateway-add-dns-rm-portal](../../includes/vpn-gateway-add-dns-rm-portal-include.md)]

## <a name="a-namegatewaysubneta4-create-a-gateway-subnet"></a><a name="gatewaysubnet"></a>4. Erstellen eines Gatewaysubnetzes
Bevor Sie das virtuelle Netzwerk mit einem Gateway verbinden, müssen Sie das Gatewaysubnetz für das virtuelle Netzwerk erstellen, mit dem Sie eine Verbindung herstellen möchten. Erstellen Sie nach Möglichkeit ein Gatewaysubnetz mit einem CIDR-Block vom Typ „/28“ oder „/27“, damit genügend IP-Adressen für zukünftige zusätzliche Konfigurationsanforderungen zur Verfügung stehen.

Wenn Sie diese Konfiguration als Übung erstellen, können Sie beim Erstellen des Gatewaysubnetzes [diese Werte](#values) verwenden.

### <a name="to-create-a-gateway-subnet"></a>So erstellen Sie ein Gatewaysubnetz
[!INCLUDE [vpn-gateway-add-gwsubnet-rm-portal](../../includes/vpn-gateway-add-gwsubnet-rm-portal-include.md)]

## <a name="a-namevnetgatewaya5-create-a-virtual-network-gateway"></a><a name="VNetGateway"></a>5. Erstellen eines Gateways für das virtuelle Netzwerk
Falls Sie diese Konfiguration zu Übungszwecken erstellen, können Sie die [Beispielkonfigurationswerte](#values) heranziehen.

### <a name="to-create-a-virtual-network-gateway"></a>So erstellen Sie ein Gateway für das virtuelle Netzwerk
[!INCLUDE [vpn-gateway-add-gw-rm-portal](../../includes/vpn-gateway-add-gw-rm-portal-include.md)]

## <a name="a-namelocalnetworkgatewaya6-create-a-local-network-gateway"></a><a name="LocalNetworkGateway"></a>6. Erstellen eines Gateways für das lokale Netzwerk
Mit dem Gateway für das lokale Netzwerk ist Ihr lokaler Standort gemeint. Geben Sie für das lokale Netzwerkgateway einen Namen ein, anhand dessen Azure darauf verweisen kann. 

Falls Sie diese Konfiguration zu Übungszwecken erstellen, können Sie die [Beispielkonfigurationswerte](#values) heranziehen.

### <a name="to-create-a-local-network-gateway"></a>So erstellen Sie ein Gateway für das lokale Netzwerk
[!INCLUDE [vpn-gateway-add-lng-rm-portal](../../includes/vpn-gateway-add-lng-rm-portal-include.md)]

## <a name="a-namevpndevicea7-configure-your-vpn-device"></a><a name="VPNDevice"></a>7. Konfigurieren des VPN-Geräts
[!INCLUDE [vpn-gateway-configure-vpn-device-rm](../../includes/vpn-gateway-configure-vpn-device-rm-include.md)]

## <a name="a-namecreateconnectiona8-create-a-sitetosite-vpn-connection"></a><a name="CreateConnection"></a>8. Erstellen einer Site-to-Site-VPN-Verbindung
Erstellen Sie die Site-to-Site-VPN-Verbindung zwischen dem Gateway Ihres virtuellen Netzwerks und Ihrem VPN-Gerät. Achten Sie darauf, dass Sie die Werte durch Ihre eigenen Werte ersetzen. Der gemeinsame Schlüssel muss dem Wert entsprechen, den Sie für Ihre VPN-Gerätekonfiguration verwendet haben. 

Stellen Sie sicher, dass das Gateway für das virtuelle Netzwerk und die Gateways für das lokale Netzwerk fertig erstellt wurden, bevor Sie mit diesem Abschnitt beginnen. Wenn Sie diese Konfiguration als Übung erstellen, können Sie beim Erstellen der Verbindung [diese Werte](#values) verwenden.

### <a name="to-create-the-vpn-connection"></a>So erstellen Sie die VPN-Verbindung
[!INCLUDE [vpn-gateway-add-site-to-site-connection-rm-portal](../../includes/vpn-gateway-add-site-to-site-connection-rm-portal-include.md)]

## <a name="a-nameverifyconnectiona9-verify-the-vpn-connection"></a><a name="VerifyConnection"></a>9. Überprüfen der VPN-Verbindung
Sie können Ihre VPN-Verbindung entweder im Portal oder mithilfe von PowerShell überprüfen.

[!INCLUDE [vpn-gateway-verify-connection-rm](../../includes/vpn-gateway-verify-connection-rm-include.md)]

## <a name="next-steps"></a>Nächste Schritte
* Sobald die Verbindung hergestellt ist, können Sie Ihren virtuellen Netzwerken virtuelle Computer hinzufügen. Weitere Informationen finden Sie unter dem [Lernpfad](https://azure.microsoft.com/documentation/learning-paths/virtual-machines) für virtuelle Computer.
* Informationen zu BGP finden Sie in der [Übersicht über BGP](vpn-gateway-bgp-overview.md) und unter [Konfigurieren von BGP](vpn-gateway-bgp-resource-manager-ps.md).




<!--HONumber=Nov16_HO2-->


