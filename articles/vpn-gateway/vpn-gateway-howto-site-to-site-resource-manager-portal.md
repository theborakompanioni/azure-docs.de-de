<properties
   pageTitle="Erstellen eines virtuellen Netzwerks mit einer Site-to-Site-VPN-Verbindung mit dem Azure Resource Manager und dem Azure-Portal | Microsoft Azure"
   description="In diesem Artikel werden Sie durch die Erstellung eines VNet mit dem Ressourcen-Manager-Modell und das Herstellen einer dazugehörigen Verbindung mit dem lokalen Netzwerk mit einer S2S-VPN Gateway-Verbindung geführt."
   services="vpn-gateway"
   documentationCenter="na"
   authors="cherylmc"
   manager="carmonm"
   editor=""
   tags="azure-resource-manager"/>

<tags
   ms.service="vpn-gateway"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="05/13/2016"
   ms.author="cherylmc"/>

# Erstellen eines VNet mit Site-to-Site-VPN-Verbindung über das Azure-Portal und Azure Resource Manager

> [AZURE.SELECTOR]
- [Azure-Portal](vpn-gateway-howto-site-to-site-resource-manager-portal.md)
- [Klassisches Azure-Portal](vpn-gateway-site-to-site-create.md)
- [PowerShell – Resource Manager](vpn-gateway-create-site-to-site-rm-powershell.md)


In diesem Artikel werden Sie durch das Erstellen eines virtuellen Netzwerks und das Herstellen einer Site-to-Site-VPN-Verbindung mit Ihrem lokalen Netzwerk mithilfe des Azure Resource Manager-Bereitstellungsmodells und des Azure-Portals geführt. Mit den unten angegebenen Schritten erstellen Sie ein VNet und fügen ein Gatewaysubnetz, ein Gateway, einen lokalen Standort und eine Verbindung hinzu. Außerdem müssen Sie Ihr VPN-Gerät konfigurieren.



**Informationen zu Azure-Bereitstellungsmodellen**

[AZURE.INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-classic-rm-include.md)]

## Verbindungsdiagramm

![Standort-zu-Standort](./media/vpn-gateway-howto-site-to-site-resource-manager-portal/site2site.png)

**Bereitstellungsmodelle und Tools für Site-to-Site-Verbindungen**

[AZURE.INCLUDE [vpn-gateway-table-site-to-site-table](../../includes/vpn-gateway-table-site-to-site-include.md)]

Wenn Sie VNets miteinander verbinden möchten, aber keine Verbindung mit einem lokalen Standort erstellen, finden Sie unter [Konfigurieren einer VNet-zu-VNet-Verbindung](vpn-gateway-vnet-vnet-rm-ps.md) entsprechende Informationen. Andere Verbindungskonfigurationstypen finden Sie im Artikel [VPN Gateway-Verbindungstopologien](vpn-gateway-topology.md).

## Voraussetzungen

Vergewissern Sie sich vor Beginn der Konfiguration, dass Sie über Folgendes verfügen:

- Ein kompatibles VPN-Gerät (und eine Person, die es konfigurieren kann). Weitere Informationen finden Sie unter [Informationen zu VPN-Geräten](vpn-gateway-about-vpn-devices.md). Wenn Sie sich mit dem Konfigurieren des VPN-Geräts oder mit den IP-Adressbereichen Ihrer lokalen Netzwerkkonfiguration nicht auskennen, müssen Sie sich an eine Person wenden, die Ihnen diese Details liefern kann.

- Eine externe öffentliche IP-Adresse für Ihr VPN-Gerät. Diese IP-Adresse darf sich nicht hinter einer NAT befinden.
	
- Ein Azure-Abonnement. Wenn Sie noch kein Azure-Abonnement haben, können Sie Ihre [MSDN-Abonnentenvorteile](http://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) aktivieren oder sich für ein [kostenloses Konto](http://azure.microsoft.com/pricing/free-trial/) registrieren.

### <a name="values"></a>Beispielkonfigurationswerte für diese Übung


Sie können die Beispielkonfigurationswerte nutzen, wenn Sie diese Schritte als Übung verwenden.

- VNet-Name: TestVNet1
- Adressraum: 10.11.0.0/16 und 10.12.0.0/16
- Subnetze: 
	- FrontEnd: 10.11.0.0/24
	- BackEnd: 10.12.0.0/24
	- GatewaySubnet: 10.12.255.0/27
- Ressourcengruppe: TestRG1
- Standort: USA, Osten
- DNS-Server: 8.8.8.8
- Gatewayname: VNet1GW
- Öffentliche IP-Adresse: VNet1GWIP
- VPN-Typ: Routenbasiert
- Verbindungstyp: Site-to-Site (IPsec)
- Gatewaytyp: VPN
- Name des Gateways für das lokale Netzwerk: Site2
- Verbindungsname: VNet1toSite2



## 1\. Erstellen eines virtuellen Netzwerks 

Falls Sie bereits ein virtuelles Netzwerk erstellt haben, sollten Sie überprüfen, ob die Einstellungen mit Ihrem VPN Gateway-Design kompatibel sind. Achten Sie dabei besonders auf alle Subnetze, die sich unter Umständen mit anderen Netzwerken überlappen. Bei überlappenden Subnetzen funktioniert die Verbindung nicht einwandfrei. Wenn Sie überprüft haben, ob für Ihr VNet die richtigen Einstellungen konfiguriert wurden, können Sie mit den Schritten im Abschnitt [Angeben eines DNS-Servers](#dns) beginnen.

### So erstellen Sie ein virtuelles Netzwerk

[AZURE.INCLUDE [vpn-gateway-basic-vnet-rm-portal](../../includes/vpn-gateway-basic-vnet-rm-portal-include.md)]

## 2\. Hinzufügen weiterer Adressräume und Subnetze

Sie können dem VNet nach dem Erstellen weitere Adressräume und Subnetze hinzufügen.

[AZURE.INCLUDE [vpn-gateway-additional-address-space](../../includes/vpn-gateway-additional-address-space-include.md)]

## <a name="dns"></a>3. Angeben eines DNS-Servers

Wenn Sie diese Konfiguration zu Übungszwecken erstellen, können Sie beim Angeben des DNS-Servers [diese Werte](#values) verwenden.

### So geben Sie einen DNS-Server an

[AZURE.INCLUDE [vpn-gateway-add-dns-rm-portal](../../includes/vpn-gateway-add-dns-rm-portal-include.md)]

## 4\. Erstellen eines Gatewaysubnetzes

Bevor Sie für das virtuelle Netzwerk eine Verbindung mit einem Gateway herstellen, müssen Sie zuerst das Gatewaysubnetz für das virtuelle Netzwerk erstellen, mit dem die Verbindung hergestellt werden soll. Das von Ihnen erstellte Gatewaysubnetz muss den Namen *GatewaySubnet* haben, da es sonst nicht richtig funktioniert.

Als Gatewaysubnetz-Präfix ist für einige Konfigurationen ein Subnetz der Größe /28 oder mehr erforderlich, um die Menge von IP-Adressen unterbringen zu können, die im Pool benötigt werden. Dies bedeutet, dass das Gatewaysubnetz-Präfix /28, /27, /26 usw. lauten muss. Es kann ratsam sein, hier ein größeres Subnetz zu erstellen, um für zukünftige Hinzufügungen gerüstet zu sein, die für die Konfiguration ggf. vorgenommen werden.

Wenn Sie diese Konfiguration als Übung erstellen, können Sie beim Erstellen des Gatewaysubnetzes [diese Werte](#values) verwenden.

### So erstellen Sie ein Gatewaysubnetz

[AZURE.INCLUDE [vpn-gateway-no-nsg](../../includes/vpn-gateway-no-nsg-include.md)]

[AZURE.INCLUDE [vpn-gateway-add-gwsubnet-rm-portal](../../includes/vpn-gateway-add-gwsubnet-rm-portal-include.md)]

## 5\. Erstellen eines Gateways für das virtuelle Netzwerk

Wenn Sie diese Konfiguration als Übung erstellen, können Sie beim Erstellen des Gateways [diese Werte](#values) verwenden.

### So erstellen Sie ein Gateway für das virtuelle Netzwerk

[AZURE.INCLUDE [vpn-gateway-add-gw-rm-portal](../../includes/vpn-gateway-add-gw-rm-portal-include.md)]

## 6\. Erstellen eines Gateways für das lokale Netzwerk

Mit dem *Gateway des lokalen Netzwerks* ist Ihr lokaler Standort gemeint. Geben Sie dem Gateway des lokalen Netzwerks einen Namen, mit dem von Azure darauf verwiesen werden kann.

Wenn Sie diese Konfiguration als Übung erstellen, können Sie beim Hinzufügen Ihres lokalen Standorts [diese Werte](#values) verwenden.

### So erstellen Sie ein Gateway für das lokale Netzwerk

[AZURE.INCLUDE [vpn-gateway-add-lng-rm-portal](../../includes/vpn-gateway-add-lng-rm-portal-include.md)]

## 7\. Konfigurieren des VPN-Geräts

[AZURE.INCLUDE [vpn-gateway-configure-vpn-device-rm](../../includes/vpn-gateway-configure-vpn-device-rm-include.md)]

## 8\. Erstellen einer Site-to-Site-VPN-Verbindung

Erstellen Sie als Nächstes die Site-to-Site-VPN-Verbindung zwischen dem Gateway Ihres virtuellen Netzwerks und Ihrem VPN-Gerät. Achten Sie darauf, dass Sie die Werte durch Ihre eigenen Werte ersetzen. Der gemeinsame Schlüssel muss dem Wert entsprechen, den Sie für Ihre VPN-Gerätekonfiguration verwendet haben.

Stellen Sie sicher, dass das Gateway für das virtuelle Netzwerk und die Gateways für das lokale Netzwerk fertig erstellt wurden, bevor Sie mit diesem Abschnitt beginnen. Wenn Sie diese Konfiguration als Übung erstellen, können Sie beim Erstellen der Verbindung [diese Werte](#values) verwenden.

### So erstellen Sie die VPN-Verbindung


[AZURE.INCLUDE [vpn-gateway-add-site-to-site-connection-rm-portal](../../includes/vpn-gateway-add-site-to-site-connection-rm-portal-include.md)]

## 9\. Überprüfen der VPN-Verbindung

Sie können Ihre VPN-Verbindung entweder im Portal oder mithilfe von PowerShell überprüfen.

[AZURE.INCLUDE [vpn-gateway-verify-connection-rm](../../includes/vpn-gateway-verify-connection-rm-include.md)]

## Nächste Schritte

- Sobald die Verbindung hergestellt ist, können Sie Ihren virtuellen Netzwerken virtuelle Computer hinzufügen. Weitere Informationen finden Sie unter dem [Lernpfad](https://azure.microsoft.com/documentation/learning-paths/virtual-machines) für virtuelle Computer.

- Informationen zu BGP finden Sie in der [Übersicht über BGP](vpn-gateway-bgp-overview.md) und unter [How to configure BGP](vpn-gateway-bgp-resource-manager-ps.md) (Gewusst wie: Konfiguration von BGP).

<!---HONumber=AcomDC_0518_2016-->