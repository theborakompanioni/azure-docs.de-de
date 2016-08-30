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
   ms.date="08/22/2016"
   ms.author="cherylmc" />

# Informationen zu VPN Gateway


Das VPN Gateway umfasst eine Sammlung von Einstellungen, die zum Senden von Netzwerkdatenverkehr zwischen virtuellen Netzwerken und lokalen Standorten verwendet werden. Das VPN Gateway wird für Standort-zu-Standort-, Punkt-zu-Standort- und ExpressRoute-Verbindungen verwendet. Außerdem dient das VPN Gateway zum Senden von Datenverkehr zwischen mehreren virtuellen Netzwerken in Azure (VNET-zu-VNET).

Jedes virtuelle Netzwerk kann nur über ein Gateway für ein virtuelles Netzwerk verfügen. Um eine Verbindung zu erstellen, fügen Sie das Gateway für ein virtuelles Netzwerk einem VNet hinzu und konfigurieren weitere Einstellungen für das VPN Gateway. In einigen Fällen handelt es sich bei der erstellten Verbindung um eine VPN-Verbindung. In anderen Fällen ist für die Konfiguration kein VPN erforderlich. Die Sammlung der Einstellungen wird unabhängig davon, ob für die Verbindung ein VPN benötigt wird, als „VPN Gateway“ bezeichnet.

Beim Konfigurieren des VPN Gateways richtet sich die verwendete Anleitung nach dem Bereitstellungsmodell, das Sie bei der Erstellung Ihres virtuellen Netzwerks genutzt haben. Wenn Sie Ihr VNET beispielsweise mit dem klassischen Bereitstellungsmodell erstellt haben, verwenden Sie die Richtlinien und Anleitungen für das klassische Bereitstellungsmodell, um die Einstellungen für das VPN Gateway zu erstellen und zu konfigurieren. Weitere Informationen finden Sie unter [Grundlagen des Resource Manager- und klassischen Bereitstellungsmodells](../resource-manager-deployment-model.md).

Die folgenden Abschnitte enthalten Tabellen, in denen die folgenden Informationen für die Konfiguration aufgeführt sind:

- Verfügbares Bereitstellungsmodell
- Verfügbare Konfigurationstools
- Direkte Links zu Artikeln, falls verfügbar


Nutzen Sie die Grafiken und Beschreibungen als Hilfe beim Auswählen der Konfigurationstopologie, die Ihre Anforderungen erfüllt. Die Diagramme zeigen die grundlegenden Topologien, aber Sie können auch komplexere Topologien erstellen, indem Sie die Diagramme als Anhaltspunkte verwenden. Jede Konfiguration basiert auf den Einstellungen für das VPN Gateway, die Sie ausgewählt haben.

Da das VPN Gateway eine Sammlung von Einstellungen umfasst, können Sie einige Einstellungen mit einem Tool konfigurieren und dann zu einem anderen Tool wechseln. Derzeit ist es nicht möglich, alle Einstellungen für das VPN Gateway im Azure-Portal zu konfigurieren. In den Anleitungen in den Artikeln der einzelnen Konfigurationen ist angegeben, ob ein bestimmtes Tool benötigt wird. Wenn Sie mit dem klassischen Bereitstellungsmodell arbeiten, empfiehlt es sich, vorerst das klassische Portal oder PowerShell zu verwenden. Informationen zu den einzelnen verfügbaren Einstellungen finden Sie unter [About VPN Gateway settings](vpn-gateway-about-vpn-gateway-settings.md) (Informationen zu VPN Gateway-Einstellungen).


## Site-to-Site und Multi-Site

### Site-to-Site

Eine Site-to-Site-Verbindung (S2S) ist eine Verbindung über einen VPN-Tunnel vom Typ „IPsec/IKE“ (IKEv1 oder IKEv2). Für diese Art von Verbindung wird ein lokales VPN-Gerät benötigt, dem eine öffentliche IP-Adresse zugewiesen ist und das nicht hinter einer NAT-Einheit angeordnet ist. S2S-Verbindungen können für standortübergreifende Konfigurationen und Hybridkonfigurationen verwendet werden.

![S2S-Verbindung](./media/vpn-gateway-about-vpngateways/demos2s.png "Site-to-Site")


### Multi-Site

Sie können eine VPN-Verbindung zwischen Ihrem VNet und mehreren lokalen Netzwerken erstellen und konfigurieren. Wenn Sie mehrere Verbindungen nutzen, müssen Sie einen routenbasierten VPN-Typ verwenden (dynamisches Gateway für klassische VNets). Da ein VNet nur über ein Gateway für ein virtuelles Netzwerk verfügen kann, wird für alle Verbindungen über das Gateway die verfügbare Bandbreite gemeinsam genutzt. Diese Art der Konfiguration wird häufig als Multi-Site-Verbindung bezeichnet.
 

![Multi-Site-Verbindung](./media/vpn-gateway-about-vpngateways/demomulti.png "Multi-Site")

### Bereitstellungsmodelle und -methoden

[AZURE.INCLUDE [vpn-gateway-table-site-to-site](../../includes/vpn-gateway-table-site-to-site-include.md)]

## VNet-zu-VNet

Das Verbinden eines virtuellen Netzwerks mit einem anderen virtuellen Netzwerk (VNet-zu-VNet) ähnelt dem Verbinden eines VNet mit einem lokalen Standort. Beide Verbindungstypen verwenden ein Azure VPN Gateway, um einen sicheren Tunnel mit IPSec/IKE bereitzustellen. Sie können sogar VNet-zu-VNet-Kommunikation mit Konfigurationen für mehrere Standorte kombinieren. Auf diese Weise können Sie Netzwerktopologien einrichten, die standortübergreifende Konnektivität mit Konnektivität zwischen virtuellen Netzwerken kombinieren.

Die verbundenen VNets können wie folgt angeordnet sein:

- In derselben Region oder in verschiedenen Regionen
- In demselben Abonnement oder in verschiedenen Abonnements
- In demselben Bereitstellungsmodell oder in verschiedenen Bereitstellungsmodellen



![VNet-zu-VNet-Verbindung](./media/vpn-gateway-about-vpngateways/demov2v.png "VNet-zu-VNet")



### Verbindungen zwischen Bereitstellungsmodellen

In Azure sind zurzeit zwei Bereitstellungsmodelle verfügbar: klassisches Modell und Resource Manager-Modell. Wenn Sie Azure seit einiger Zeit verwenden, verfügen Sie wahrscheinlich über Azure-VMs und Instanzrollen, die in einem klassischen VNet ausgeführt werden. Ihre neueren VMs und Rolleninstanzen werden möglicherweise in einem in Resource Manager erstellten VNet ausgeführt. Sie können eine Verbindung zwischen den VNets erstellen, damit die Ressourcen in einem VNet direkt mit Ressourcen im anderen VNet kommunizieren können.

### VNet-Peering

Unter Umständen können Sie das VNet-Peering verwenden, um Ihre Verbindung zu erstellen, solange die Konfiguration Ihres virtuellen Netzwerks bestimmte Anforderungen erfüllt. Beim VNet-Peering wird kein Gateway für das virtuelle Netzwerk verwendet. Das [VNet-Peering](../virtual-network/virtual-network-peering-overview.md) befindet sich derzeit in der Vorschauphase.


### Bereitstellungsmodelle und -methoden

[AZURE.INCLUDE [vpn-gateway-table-vnet-to-vnet](../../includes/vpn-gateway-table-vnet-to-vnet-include.md)]


## Punkt-zu-Standort

Mit einer P2S-Konfiguration (Punkt-zu-Standort) können Sie von einem einzelnen Clientcomputer aus eine sichere Verbindung mit Ihrem virtuellen Netzwerk herstellen. P2S ist eine VPN-Verbindung über SSTP (Secure Socket Tunneling-Protokoll). Damit P2S-Verbindungen funktionieren, ist kein VPN-Gerät und keine öffentliche IP-Adresse erforderlich. Sie stellen die VPN-Verbindung her, indem Sie sie vom Clientcomputer aus initiieren. Dies ist eine nützliche Lösung, wenn Sie von einem Remotestandort, z.B. von zu Hause oder in einer Konferenz, eine Verbindung mit Ihrem VNet herstellen möchten. Diese Methode eignet sich auch, wenn Sie nur wenige Clients besitzen, die mit einem VNet verbunden werden müssen.


![Point-to-Site-Verbindung](./media/vpn-gateway-about-vpngateways/demop2s.png "Point-to-Site")

### Bereitstellungsmodelle und -methoden

[AZURE.INCLUDE [vpn-gateway-table-point-to-site](../../includes/vpn-gateway-table-point-to-site-include.md)]


## ExpressRoute

[AZURE.INCLUDE [expressroute-intro](../../includes/expressroute-intro-include.md)]

Weitere Informationen über ExpressRoute finden Sie unter [ExpressRoute – Technische Übersicht](../expressroute/expressroute-introduction.md).


## Parallel bestehende Site-to-Site- und ExpressRoute-Verbindungen

ExpressRoute ist eine direkte, dedizierte Verbindung aus Ihrem WAN mit Microsoft Services (einschließlich Azure), die nicht über das öffentliche Internet verläuft. Site-to-Site-VPN-Datenverkehr wird verschlüsselt über das öffentliche Internet gesendet. Die Möglichkeit, Site-to-Site-VPN- und ExpressRoute-Verbindungen für dasselbe virtuelle Netzwerk zu konfigurieren, hat mehrere Vorteile. Sie können eine Site-to-Site-VPN-Verbindung als sicheren Failoverpfad für ExpressRoute konfigurieren oder für die Verbindung mit Websites nutzen, die nicht Teil Ihres Netzwerks, aber über ExpressRoute verbunden sind.


![Gleichzeitig bestehende Verbindungen](./media/vpn-gateway-about-vpngateways/demoer.png "expressroute-site2site")


### Bereitstellungsmodelle und -methoden

[AZURE.INCLUDE [vpn-gateway-table-coexist](../../includes/vpn-gateway-table-coexist-include.md)]


## Nächste Schritte

Weitere Informationen zu VPN Gateways finden Sie unter [Häufig gestellte Fragen zum VPN-Gateway](vpn-gateway-vpn-faq.md).

Verbinden Sie Ihren lokalen Standort mit einem VNet. Weitere Informationen finden Sie unter [Erstellen eines VNet mit Site-to-Site-VPN-Verbindung über das Azure-Portal und Azure Resource Manager](vpn-gateway-howto-site-to-site-resource-manager-portal.md).





 

<!---HONumber=AcomDC_0824_2016-->