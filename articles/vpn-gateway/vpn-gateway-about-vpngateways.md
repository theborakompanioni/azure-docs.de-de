<properties 
   pageTitle="Informationen zu VPN Gateway | Microsoft Azure"
   description="Enthält Informationen zu VPN Gateway-Verbindungen für virtuelle Azure-Netzwerke."
   services="vpn-gateway"
   documentationCenter="na"
   authors="cherylmc"
   manager="carmonm"
   editor=""
   tags="azure-resource-manager,azure-service-management"/>
<tags 
   ms.service="vpn-gateway"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="09/21/2016"
   ms.author="cherylmc" />

# Informationen zu VPN Gateway


Ein virtuelles Netzwerkgateway dient zum Senden von Netzwerkdatenverkehr zwischen virtuellen Azure-Netzwerken und lokalen Standorten sowie zwischen virtuellen Netzwerken innerhalb von Azure (VNet-zu-VNet). Beim Konfigurieren eines VPN-Gateways müssen Sie ein Gateway des virtuellen Netzwerks und eine dazugehörige Verbindung erstellen und konfigurieren.

Wenn Sie beim Resource Manager-Bereitstellungsmodell eine Ressource vom Typ „Gateway des virtuellen Netzwerks“ erstellen, geben Sie mehrere Einstellungen an. Eine dieser Einstellungen ist „-GatewayType“. Zur Auswahl stehen zwei Arten von virtuellen Netzwerkgateways: „Vpn“ und „ExpressRoute“.

Verwenden Sie den Gatewaytyp „ExpressRoute“, wenn Netzwerkdatenverkehr über eine dedizierte private Verbindung gesendet wird. Dies wird auch als ExpressRoute-Gateway bezeichnet. Wenn Netzwerkdatenverkehr verschlüsselt und über eine öffentliche Verbindung gesendet wird, verwenden Sie den Gatewaytyp „Vpn“. Dies wird als VPN Gateway bezeichnet. Bei Site-to-Site-, Point-to-Site- und VNet-zu-VNet-Verbindungen wird jeweils VPN Gateway verwendet.

Ein virtuelles Netzwerk kann pro Gatewaytyp immer nur über ein einzelnes virtuelles Netzwerkgateway verfügen. So können Sie beispielsweise ein virtuelles Netzwerkgateway mit „-GatewayType ExpressRoute“ und ein virtuelles Netzwerkgateway mit „-GatewayType Vpn“ verwenden. Dieser Artikel konzentriert sich in erster Linie auf VPN Gateway. Weitere Informationen über ExpressRoute finden Sie unter [ExpressRoute – Technische Übersicht](../expressroute/expressroute-introduction.md).

## Preise

[AZURE.INCLUDE [vpn-gateway-about-pricing-include](../../includes/vpn-gateway-about-pricing-include.md)]


## Gateway-SKUs

[AZURE.INCLUDE [vpn-gateway-gwsku-include](../../includes/vpn-gateway-gwsku-include.md)]

In der folgenden Tabelle sind die Gatewaytypen und der geschätzte zusammengefasste Durchsatz angegeben. Diese Tabelle betrifft sowohl das Resource Manager-Bereitstellungsmodell als auch das klassische Bereitstellungsmodell.

[AZURE.INCLUDE [vpn-gateway-table-gwtype-aggthroughput](../../includes/vpn-gateway-table-gwtype-aggtput-include.md)]

## Konfigurieren von VPN Gateway

Die Konfigurationsschritte für VPN Gateway hängen davon ab, mit welchem Bereitstellungsmodell Sie das virtuelle Netzwerk erstellt haben. Wenn Sie Ihr VNET beispielsweise mit dem klassischen Bereitstellungsmodell erstellt haben, verwenden Sie die Richtlinien und Anleitungen für das klassische Bereitstellungsmodell, um die Einstellungen für das VPN Gateway zu erstellen und zu konfigurieren. Weitere Informationen zu Bereitstellungsmodellen finden Sie unter [Azure Resource Manager-Bereitstellung im Vergleich zur klassischen Bereitstellung: Grundlegendes zu Bereitstellungsmodellen und zum Status von Ressourcen](../resource-manager-deployment-model.md).

Eine VPN Gateway-Verbindung basiert auf mehreren, mit spezifischen Einstellungen konfigurierten Ressourcen. Die meisten der Ressourcen können separat konfiguriert werden. In manchen Fällen ist allerdings eine bestimmte Reihenfolge einzuhalten. Sie können zunächst mit einem Konfigurationstool wie dem Azure-Portal Ressourcen erstellen und konfigurieren. Später können Sie dann mit einem anderen Tool (beispielsweise PowerShell) zusätzliche Ressourcen konfigurieren oder ggf. vorhandene Ressourcen ändern. Derzeit können nicht alle Ressourcen und Ressourceneinstellungen über das Azure-Portal konfiguriert werden. Sollte ein bestimmtes Konfigurationstool benötigt werden, ist dies in den Anleitungen der Artikel zu den einzelnen Verbindungstopologien angegeben. Informationen zu einzelnen Ressourcen und Einstellungen für VPN Gateway finden Sie unter [Informationen zu VPN Gateway-Einstellungen](vpn-gateway-about-vpn-gateway-settings.md).

Die folgenden Abschnitte enthalten Tabellen, in denen Folgendes aufgelistet ist:

- Verfügbares Bereitstellungsmodell
- Verfügbare Konfigurationstools
- Direkte Links zu Artikeln, falls verfügbar

Orientieren Sie sich bei der Wahl einer geeigneten Verbindungstopologie an den Diagrammen und Beschreibungen. Die Diagramme zeigen die grundlegenden Topologien, aber Sie können auch komplexere Topologien erstellen, indem Sie die Diagramme als Anhaltspunkte verwenden.

## Site-to-Site und Multi-Site

### Site-to-Site

Eine VPN Gateway-S2S-Verbindung (Site-to-Site) ist eine Verbindung über einen VPN-Tunnel vom Typ „IPsec/IKE“ (IKEv1 oder IKEv2). Für diese Art von Verbindung wird ein lokales VPN-Gerät benötigt, dem eine öffentliche IP-Adresse zugewiesen ist und das nicht hinter einer NAT-Einheit angeordnet ist. S2S-Verbindungen können für standortübergreifende Konfigurationen und Hybridkonfigurationen verwendet werden.

![S2S-Verbindung](./media/vpn-gateway-about-vpngateways/demos2s.png "Site-to-Site")


### Multi-Site

Sie können eine VPN Gateway-Verbindung zwischen Ihrem VNet und mehreren lokalen Netzwerken erstellen und konfigurieren. Wenn Sie mehrere Verbindungen nutzen, müssen Sie den VPN-Typ „RouteBased“ verwenden (dynamisches Gateway für klassische VNets). Da ein VNet nur über eine einzelne VPN Gateway-Instanz verfügen kann, wird die verfügbare Bandbreite von allen Verbindungen über das Gateway gemeinsam genutzt. Dies wird häufig als Multi-Site-Verbindung bezeichnet.
 

![Multi-Site-Verbindung](./media/vpn-gateway-about-vpngateways/demomulti.png "Multi-Site")

### Bereitstellungsmodelle und -methoden für Site-to-Site und Multi-Site

[AZURE.INCLUDE [vpn-gateway-table-site-to-site](../../includes/vpn-gateway-table-site-to-site-include.md)]

## VNet-zu-VNet

Das Verbinden eines virtuellen Netzwerks mit einem anderen virtuellen Netzwerk (VNet-zu-VNet) ähnelt dem Verbinden eines VNet mit einem lokalen Standort. Beide Verbindungstypen verwenden ein VPN-Gateway, um einen sicheren Tunnel mit IPsec/IKE bereitzustellen. Die VNet-zu-VNet-Kommunikation kann sogar mit Multi-Site-Verbindungskonfigurationen kombiniert werden. Auf diese Weise können Sie Netzwerktopologien einrichten, die standortübergreifende Konnektivität mit Konnektivität zwischen virtuellen Netzwerken kombinieren.

Die verbundenen VNets können wie folgt angeordnet sein:

- In derselben Region oder in verschiedenen Regionen
- In demselben Abonnement oder in verschiedenen Abonnements
- In demselben Bereitstellungsmodell oder in verschiedenen Bereitstellungsmodellen


![VNet-zu-VNet-Verbindung](./media/vpn-gateway-about-vpngateways/demov2v.png "VNet-zu-VNet")

#### Verbindungen zwischen Bereitstellungsmodellen

In Azure sind zurzeit zwei Bereitstellungsmodelle verfügbar: klassisches Modell und Resource Manager-Modell. Wenn Sie Azure seit einiger Zeit verwenden, verfügen Sie wahrscheinlich über Azure-VMs und Instanzrollen, die in einem klassischen VNet ausgeführt werden. Ihre neueren VMs und Rolleninstanzen werden möglicherweise in einem in Resource Manager erstellten VNet ausgeführt. Sie können eine Verbindung zwischen den VNets erstellen, damit die Ressourcen in einem VNet direkt mit Ressourcen im anderen VNet kommunizieren können.

#### VNet-Peering

Sofern Ihr virtuelles Netzwerk bestimmte Anforderungen erfüllt, können Sie Ihre Verbindung ggf. mithilfe von VNet-Peering erstellen. Beim VNet-Peering wird kein Gateway für das virtuelle Netzwerk verwendet. Weitere Informationen finden Sie unter [VNet-Peering](../virtual-network/virtual-network-peering-overview.md).


### Bereitstellungsmodelle und -methoden für VNet-zu-VNet

[AZURE.INCLUDE [vpn-gateway-table-vnet-to-vnet](../../includes/vpn-gateway-table-vnet-to-vnet-include.md)]


## Point-to-Site

Mit einer VPN Gateway-P2S-Konfiguration (Point-to-Site) können Sie von einem einzelnen Clientcomputer aus eine sichere Verbindung mit Ihrem virtuellen Netzwerk herstellen. P2S ist eine VPN-Verbindung über SSTP (Secure Socket Tunneling-Protokoll). Damit P2S-Verbindungen funktionieren, ist kein VPN-Gerät und keine öffentliche IP-Adresse erforderlich. Sie stellen die VPN-Verbindung her, indem Sie sie vom Clientcomputer aus initiieren. Dies ist hilfreich, wenn Sie an einem Remotestandort (etwa zu Hause oder in einer Konferenz) eine Verbindung mit Ihrem VNet herstellen möchten oder nur über eine geringe Anzahl von Clients verfügen, die mit einem VNet verbunden werden sollen. P2S-Verbindungen können zusammen mit S2S-Verbindungen über die gleiche VPN Gateway-Instanz verwendet werden – vorausgesetzt, die Konfigurationsanforderungen beider Verbindungen sind kompatibel.


![Point-to-Site-Verbindung](./media/vpn-gateway-about-vpngateways/demop2s.png "Point-to-Site")

### Bereitstellungsmodelle und -methoden für Point-to-Site

[AZURE.INCLUDE [vpn-gateway-table-point-to-site](../../includes/vpn-gateway-table-point-to-site-include.md)]


## ExpressRoute

[AZURE.INCLUDE [expressroute-intro](../../includes/expressroute-intro-include.md)]

Bei einer ExpressRoute-Verbindung wird ein virtuelles Netzwerkgateway nicht mit dem Gatewaytyp „Vpn“, sondern mit „ExpressRoute“ konfiguriert. Weitere Informationen über ExpressRoute finden Sie unter [ExpressRoute – Technische Übersicht](../expressroute/expressroute-introduction.md).


## Parallel bestehende Site-to-Site- und ExpressRoute-Verbindungen

ExpressRoute ist eine direkte, dedizierte Verbindung aus Ihrem WAN mit Microsoft Services (einschließlich Azure), die nicht über das öffentliche Internet verläuft. Site-to-Site-VPN-Datenverkehr wird verschlüsselt über das öffentliche Internet gesendet. Die Möglichkeit, Site-to-Site-VPN- und ExpressRoute-Verbindungen für dasselbe virtuelle Netzwerk zu konfigurieren, hat mehrere Vorteile.

Sie können eine Site-to-Site-VPN-Verbindung als sicheren Failoverpfad für ExpressRoute konfigurieren oder für die Verbindung mit Websites nutzen, die nicht Teil Ihres Netzwerks, aber über ExpressRoute verbunden sind. Dies erfordert zwei virtuelle Netzwerkgateways für das gleiche virtuelle Netzwerk: eins mit „-GatewayType Vpn“ und eins mit „-GatewayType ExpressRoute“.


![Gleichzeitig bestehende Verbindungen](./media/vpn-gateway-about-vpngateways/demoer.png "expressroute-site2site")


### Bereitstellungsmodelle und -methoden für S2S und ExpressRoute

[AZURE.INCLUDE [vpn-gateway-table-coexist](../../includes/vpn-gateway-table-coexist-include.md)]


## Nächste Schritte

Planen Sie Ihre VPN Gateway-Konfiguration. Entsprechende Informationen finden Sie unter [Planung und Entwurf für VPN Gateway](vpn-gateway-plan-design.md) sowie unter [Connecting your on-premises network to Azure](../guidance/guidance-connecting-your-on-premises-network-to-azure.md) (Herstellen einer Verbindung zwischen Ihrem lokalen Netzwerk und Azure).








 

<!---HONumber=AcomDC_0928_2016-->