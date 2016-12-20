---
title: "Einführung in ExpressRoute | Microsoft Docs"
description: "Diese Seite enthält eine Übersicht über den ExpressRoute-Dienst, darunter Informationen zur Funktionsweise einer ExpressRoute-Verbindung."
documentationcenter: na
services: expressroute
author: cherylmc
manager: carmonm
editor: 
ms.assetid: fd95dcd5-df1d-41d6-85dd-e91d0091af05
ms.service: expressroute
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 10/10/2016
ms.author: cherylmc
translationtype: Human Translation
ms.sourcegitcommit: dcda8b30adde930ab373a087d6955b900365c4cc
ms.openlocfilehash: 57b95d5018fa197096992f7cd255a54501875dea


---
# <a name="expressroute-technical-overview"></a>ExpressRoute - Technische Übersicht
Mit Microsoft Azure ExpressRoute können Sie Ihre lokalen Netzwerke über eine dedizierte private Verbindung, die von einem Konnektivitätsanbieter bereitgestellt wird, in die Microsoft Cloud erweitern. Mit ExpressRoute können Sie Verbindungen mit Microsoft-Clouddiensten herstellen, z. B. Microsoft Azure, Office 365 und CRM Online. Die Konnektivität kann über ein Any-to-Any-Netzwerk (IP VPN), ein Point-to-Point-Ethernet-Netzwerk oder eine virtuelle Querverbindung über einen Konnektivitätsanbieter in einer Co-Location-Einrichtung bereitgestellt werden. ExpressRoute-Verbindungen verlaufen nicht über das öffentliche Internet. Auf diese Weise können ExpressRoute-Verbindungen eine höhere Sicherheit, größere Zuverlässigkeit und schnellere Geschwindigkeit bei geringerer Latenz als herkömmliche Verbindungen über das Internet bieten.

![](./media/expressroute-introduction/expressroute-basic.png)

**Die wichtigsten Vorteile sind:**

* Layer 3-Konnektivität zwischen Ihrem lokalen Netzwerk und der Microsoft Cloud über einen Konnektivitätsanbieter. Die Konnektivität kann über ein Any-to-Any-Netzwerk (IPVPN), eine Point-to-Point-Ethernet-Verbindung oder eine virtuelle Querverbindung über einen Ethernet-Exchange bereitgestellt werden.
* Verbindung mit Microsoft-Clouddiensten in allen Regionen einer geopolitischen Region.
* Globale Konnektivität mit Microsoft-Diensten in allen Regionen mit ExpressRoute Premium-Add-On.
* Dynamisches Routing zwischen Ihrem Netzwerk und Microsoft mit Protokollen nach Branchenstandard (BGP).
* Integrierte Redundanz an jedem Peeringort, um eine höhere Zuverlässigkeit zu erzielen.
* [SLA](https://azure.microsoft.com/support/legal/sla/)zur Verbindungsbetriebszeit.
* Dienstqualität und Unterstützung für mehrere Klassen von Diensten für spezielle Anwendungen, z. B. Skype for Business.

Weitere Informationen finden Sie unter [ExpressRoute – FAQ](expressroute-faqs.md) .

## <a name="a-namehowtoconnectahow-can-i-connect-my-network-to-microsoft-using-expressroute"></a><a name="howtoconnect"></a>Wie kann ich mein Netzwerk mithilfe von ExpressRoute mit Microsoft verbinden?
Sie können eine Verbindung zwischen Ihrem lokalen Netzwerk und der Microsoft Cloud auf drei unterschiedliche Arten herstellen.

### <a name="co-located-at-a-cloud-exchange"></a>Über einen Cloud Exchange
Wenn Sie sich in einer Einrichtung mit einem Cloud Exchange befinden, können Sie virtuelle Querverbindungen zur Microsoft Cloud über den Ethernet Exchange des Co-Location-Anbieters bestellen. Co-Location-Anbieter stellen entweder Layer 2-Querverbindungen oder verwaltete Layer 3-Querverbindungen zwischen Ihrer Infrastruktur in der Co-Location-Einrichtung und der Microsoft Cloud bereit.

### <a name="point-to-point-ethernet-connections"></a>Point-to-Point-Ethernet-Verbindungen
Sie können Ihre lokalen Rechenzentren/Büros über Point-to-Point-Ethernet-Links mit der Microsoft Cloud verbinden. Point-to-Point-Ethernet-Anbieter können Layer 2-Verbindungen oder verwaltete Layer 3-Verbindungen zwischen Ihrem Standort und der Microsoft Cloud bereitstellen.

### <a name="any-to-any-ipvpn-networks"></a>Any-to-Any-Netzwerke (IPVPN)
Sie können Ihr WAN in die Microsoft Cloud integrieren. IPVPN-Anbieter (normalerweise MPLS VPN) bieten Any-to-Any-Konnektivität zwischen Ihren Niederlassungen und den Rechenzentren an. Die Microsoft Cloud kann mit Ihrem WAN verbunden werden, um sie wie eine normale Niederlassung erscheinen zu lassen. WAN-Anbieter stellen in der Regel verwaltete Layer 3-Konnektivität bereit. ExpressRoute-Funktionen und -Features sind für alle oben genannten Konnektivitätsmodelle identisch. 

Konnektivitätsanbieter können ein oder mehrere Konnektivitätsmodelle bereitstellen. Sie können zusammen mit Ihrem Konnektivitätsanbieter das Modell auswählen, das für Sie am besten geeignet ist.

![](./media/expressroute-introduction/expressroute-connectivitymodels.png)

## <a name="expressroute-features"></a>ExpressRoute-Features
ExpressRoute unterstützt die folgenden Features und Funktionen: 

### <a name="layer-3-connectivity"></a>Layer 3-Konnektivität
Microsoft nutzt das Branchenstandardprotokoll für das dynamische Routing (BGP), um Routen zwischen Ihrem lokalen Netzwerk, Ihren Instanzen in Azure und öffentlichen Microsoft-Adressen auszutauschen.  Wir richten für Ihr Netzwerk mehrere BGP-Sitzungen für unterschiedliche Datenverkehrsprofile ein. Weitere Informationen finden Sie im Artikel [ExpressRoute-Verbindung und Routingdomänen](expressroute-circuit-peerings.md) .

### <a name="redundancy"></a>Redundanz
Jede ExpressRoute-Verbindung besteht aus zwei Verbindungen mit zwei Microsoft Enterprise-Edgeroutern (MSEEs) vom Konnektivitätsanbieter bzw. Ihrem Netzwerk-Edge. Microsoft erfordert eine BGP-Dualverbindung vom Konnektivitätsanbieter bzw. von Ihrer Seite – eine für jeden MSEE. Sie können sich auch dafür entscheiden, auf Ihrer Seite keine redundanten Geräte /Ethernet-Verbindungen bereitzustellen. Allerdings verwenden Konnektivitätsanbieter redundante Geräte, um sicherzustellen, dass Ihre Verbindungen auf redundante Weise an Microsoft übergeben werden. Eine redundante Layer 3-Konnektivitätskonfiguration ist eine Anforderung, die erfüllt sein muss, damit unsere [SLA](https://azure.microsoft.com/support/legal/sla/) gültig ist. 

### <a name="connectivity-to-microsoft-cloud-services"></a>Verbindung mit Microsoft-Clouddiensten
[!INCLUDE [expressroute-office365-include](../../includes/expressroute-office365-include.md)]

ExpressRoute-Verbindungen ermöglichen den Zugriff auf die folgenden Dienste:

* Microsoft Azure-Dienste
* Microsoft Office 365-Dienste
* Microsoft CRM Online-Dienste 

Auf der Seite [ExpressRoute – FAQ](expressroute-faqs.md) finden Sie eine ausführliche Liste mit den Diensten, die per ExpressRoute unterstützt werden.

### <a name="connectivity-to-all-regions-within-a-geopolitical-region"></a>Verbindung mit allen Regionen einer geopolitischen Region
Sie können über einen unserer [Peeringstandorte](expressroute-locations.md) eine Verbindung mit Microsoft herstellen und so Zugriff auf alle Regionen innerhalb der jeweiligen geopolitischen Region erhalten. 

Wenn Sie beispielsweise in Amsterdam per ExpressRoute mit Microsoft verbunden sind, haben Sie Zugriff auf alle Microsoft-Clouddienste, die in Nordeuropa und Westeuropa gehostet werden. Der Artikel [ExpressRoute-Partner und Peeringstandorte](expressroute-locations.md) enthält eine Übersicht über die geopolitischen Regionen, dazugehörigen Microsoft-Cloudregionen und entsprechenden ExpressRoute-Peeringstandorte.

### <a name="global-connectivity-with-expressroute-premium-add-on"></a>Globale Konnektivität mit ExpressRoute Premium-Add-On
Sie können das ExpressRoute Premium-Add-On-Feature aktivieren, um die Konnektivität über geopolitische Grenzen hinweg zu erweitern. Wenn Sie beispielsweise in Amsterdam per ExpressRoute mit Microsoft verbunden sind, haben Sie Zugriff auf alle Microsoft-Clouddienste, die in allen Regionen weltweit gehostet werden (ausgenommen nationale Clouds). Sie können auf Dienste, die in Südamerika oder Australien bereitgestellt werden, genauso zugreifen, wie Sie auf die Regionen in Nord- und Westeuropa zugreifen.

### <a name="rich-connectivity-partner-ecosystem"></a>Partner-Ökosystem mit umfassender Konnektivität
ExpressRoute verfügt über ein ständig wachsendes Ökosystem aus Konnektivitätsanbietern und SI-Partnern. Der Artikel [ExpressRoute-Anbieter und -Standorte](expressroute-locations.md) enthält die aktuellen Informationen hierzu.

### <a name="connectivity-to-national-clouds"></a>Verbindung mit nationalen Clouds
Microsoft betreibt isolierte Cloudumgebungen für spezielle geopolitische Regionen und Kundensegmente. Eine Liste mit nationalen Clouds und Anbietern finden Sie auf der Seite [ExpressRoute-Anbieter und -Standorte](expressroute-locations.md) .

### <a name="supported-bandwidth-options"></a>Unterstützte Bandbreitenoptionen
Sie können ExpressRoute-Verbindungen für einen großen Bandbreitenbereich erwerben. Die unterstützten Bandbreiten sind unten in der Liste aufgeführt. Fragen Sie Ihren Konnektivitätsanbieter nach der Liste der Bandbreiten, die er unterstützt.

* 50 MBit/s
* 100 MBit/s
* 200 MBit/s
* 500 MBit/s
* 1 GBit/s
* 2 GBit/s
* 5 GBit/s
* 10 GBit/s

### <a name="dynamic-scaling-of-bandwidth"></a>Dynamische Skalierung der Bandbreite
Sie haben die Möglichkeit, die ExpressRoute-Verbindungsbandbreite zu erhöhen (Best Effort-Basis), ohne Ihre Verbindungen zu unterbrechen. 

### <a name="flexible-billing-models"></a>Flexible Abrechnungsmodelle
Sie können ein Abrechnungsmodell auswählen, das für Sie am besten geeignet ist. Wählen Sie zwischen den unten aufgeführten Abrechnungsmodellen. Weitere Details finden Sie auf der Seite [ExpressRoute – FAQ](expressroute-faqs.md). 

* **Datenflatrate:** Die ExpressRoute-Verbindung wird basierend auf einer monatlichen Gebühr abgerechnet, und alle eingehenden und ausgehenden Datenübertragungen sind kostenlos enthalten. 
* **Datentaktung:** Die ExpressRoute-Verbindung wird basierend auf einer monatlichen Gebühr abgerechnet. Alle eingehenden Datenübertragungen sind kostenlos. Ausgehende Datenübertragungen werden pro GB der Datenübertragung in Rechnung gestellt. Die Datenübertragungsraten variieren je nach Region.
* **ExpressRoute Premium-Add-On:** ExpressRoute Premium ist ein Add-On über die ExpressRoute-Verbindung. Mit dem ExpressRoute Premium-Add-On werden die folgenden Funktionen bereitgestellt: 
  * Erhöhte Routengrenzwerte für öffentliches und privates Azure-Peering von 4.000 Routen auf 10.000 Routen.
  * Globale Konnektivität für Dienste. Eine ExpressRoute-Verbindung, die in einer beliebigen Region (ausgenommen nationale Clouds) erstellt wird, verfügt über Zugriff auf Ressourcen aller anderen Regionen weltweit. Beispielsweise kann auf ein virtuelles Netzwerk, das in Westeuropa erstellt wird, über eine ExpressRoute-Verbindung zugegriffen werden, die im Silicon Valley bereitgestellt wird.
  * Erhöhte Anzahl von VNet-Links pro ExpressRoute-Verbindung von 10 auf einen höheren Grenzwert, je nach Bandbreite der Verbindung.

## <a name="next-steps"></a>Nächste Schritte
* Informieren Sie sich über ExpressRoute-Verbindungen und Routingdomänen. Siehe [ExpressRoute-Verbindungen und Routingdomänen](expressroute-circuit-peerings.md).
* Suchen Sie nach einem Service Provider. Siehe [ExpressRoute-Partner und Peeringstandorte](expressroute-locations.md).
* Stellen Sie sicher, dass alle Voraussetzungen erfüllt werden. Informationen finden Sie unter [ExpressRoute-Voraussetzungen](expressroute-prerequisites.md).
* Sehen Sie sich die Anforderungen für [Routing](expressroute-routing.md), [NAT](expressroute-nat.md) und [QoS](expressroute-qos.md) an.
* Konfigurieren Sie Ihre ExpressRoute-Verbindung.
  * [Erstellen einer ExpressRoute-Verbindung](expressroute-howto-circuit-classic.md)
  * [Konfigurieren des Routings](expressroute-howto-routing-classic.md)
  * [Verknüpfen eines VNet mit einer ExpressRoute-Verbindung](expressroute-howto-linkvnet-classic.md)




<!--HONumber=Dec16_HO2-->


