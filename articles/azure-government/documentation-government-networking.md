---
title: Azure Government-Netzwerk | Microsoft Docs
description: "Dieser Artikel enthält einen Vergleich der Features sowie Anleitungen für private Verbindungen mit Azure Government."
services: azure-government
cloud: gov
documentationcenter: 
author: jawalte
manager: zakramer
ms.assetid: 3da70579-ecda-421a-8ebf-d52906334e9b
ms.service: azure-government
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: azure-government
ms.date: 01/24/2017
ms.author: jawalte
translationtype: Human Translation
ms.sourcegitcommit: 8288ca2e85ec0cd1a1b53862b2d60fcb88e7be4b
ms.openlocfilehash: c6b8c27299c1bfceb045ee7d82981654f3a446d8


---
# <a name="azure-government-networking"></a>Azure Government-Netzwerk
## <a name="expressroute-private-connectivity"></a>ExpressRoute (private Verbindung)
ExpressRoute ist allgemein in Azure Government verfügbar. Weitere Informationen (u.a. zu Partnern und Peeringstandorten) finden Sie in der [öffentlichen Dokumentation zu ExpressRoute](../expressroute/index.md).

### <a name="variations"></a>Variationen
ExpressRoute ist in Azure Government allgemein verfügbar. 

* Government-Kunden nutzen physisch isolierte Kapazität über eine dedizierte Azure Government-ExpressRoute-Leitung.
* Azure Government (Gov) bietet höhere Verfügbarkeit und Stabilität, indem mehrere Regionspaare genutzt werden, die mindestens 800 Kilometer voneinander entfernt sind. 
* Die Azure Gov-ExpressRoute-Konnektivität wird standardmäßig mit Aktiv/Aktiv-Redundanz und Unterstützung von Bursting und einer Übertragungsrate von bis zu 10 GBit/s bereitgestellt (der Mindestwert ist 50 MBit/s).
* Azure Gov-ExpressRoute-Standorte bieten optimierte Pfade (kürzeste Hops, niedrigste Latenz, höchste Leistung usw.) für Kunden und geografisch redundante Azure Gov-Regionen.
* Die private Azure Gov-ExpressRoute-Verbindung nutzt das Internet nicht und ist nicht vom Internet abhängig.
* Die physische und logische Azure Gov-Infrastruktur ist physisch dediziert und getrennt, und der Zugriff auf die Infrastruktur ist auf US-Bürger beschränkt.
* Microsoft besitzt und betreibt die gesamte Fiberinfrastruktur zwischen Azure Gov-Regionen und Azure Gov-ExpressRoute-Meet-Me-Standorten.
* Azure Gov-ExpressRoute bietet Konnektivität mit Microsoft Azure, O365 und CRM-Clouddiensten.

### <a name="considerations"></a>Überlegungen
Es gibt zwei grundlegende Dienste, die private Netzwerkverbindungen in Azure Government bereitstellen: VPN (Site-to-Site für eine typische Organisation) und ExpressRoute.

Azure ExpressRoute wird zum Herstellen privater Verbindungen zwischen Azure Government-Datencentern und einer Infrastruktur verwendet, die sich an Ihrem Standort oder in einer Kollokationsumgebung befindet. ExpressRoute-Verbindungen werden nicht über das öffentliche Internet hergestellt. Sie bieten mehr Zuverlässigkeit, eine höhere Geschwindigkeit und weniger Latenz als herkömmliche Internetverbindungen. In bestimmten Fällen lassen sich durch die Verwendung von ExpressRoute-Verbindungen zum Übertragen von Daten zwischen lokalen Systemen und Azure erhebliche Kosteneinsparungen erzielen.   

Mit ExpressRoute stellen Sie Verbindungen mit Azure an einem ExpressRoute-Standort (Exchange-Anbietereinrichtung) her oder verbinden sich direkt über Ihr vorhandenes WAN (z.B. ein MPLS-VPN), das von einem Netzwerkdienstanbieter bereitgestellt wird.

![alt text](./media/azure-government-capability-private-connectivity-options.PNG)  ![alt text](./media/government-capability-expressroute.PNG)  

Damit Netzwerkdienste Azure Government-Kundenanwendungen und -Lösungen unterstützen, wird dringend empfohlen, ExpressRoute (private Verbindung) für das Herstellen einer Verbindung mit Azure Government zu implementieren. Bei der Verwendung von VPN-Verbindungen sollte Folgendes berücksichtigt werden:

* Kunden müssen sich an die autorisierende Person/Agentur wenden, um zu erfragen, ob eine private Verbindung oder ein anderer sicherer Verbindungsmechanismus erforderlich ist, und weitere zu berücksichtigende Einschränkungen zu identifizieren.
* Kunden müssen entscheiden, ob das Site-to-Site-VPN über eine Zone für private Verbindungen weitergeleitet werden soll.
* Kunden müssen eine MPLS-Verbindung oder ein VPN von einem lizenzierten Dienstanbieter für private Verbindungen anfordern.

Alle Kunden, die eine Architektur mit privaten Verbindungen nutzen, müssen überprüfen, ob für die Kundenverbindung mit dem GN/I-Edgerouter-Abgrenzungspunkt (Gateway Network/Internet) für Azure Government eine entsprechende Implementierung durchgeführt und beibehalten wird. Dementsprechend muss Ihre Organisation die Netzwerkverbindung zwischen Ihrer lokalen Umgebung und dem GN/C-Edgerouter-Abgrenzungspunkt (Gateway Network/Customer) für Azure Government einrichten.

## <a name="support-for-bgp-communities"></a>Unterstützung für BGP-Communitys
Dieser Abschnitt enthält eine Übersicht über die Verwendung von BGP-Communitys mit ExpressRoute in AzureGov. Microsoft kündigt Routen in den Pfaden für das öffentliche Peering und Microsoft-Peering an, und die Routen sind dabei mit den entsprechenden Communitywerten versehen. Die Gründe für diese Vorgehensweise und die Details zu den Communitywerten sind weiter unten beschrieben. Microsoft berücksichtigt aber keine Communitywerte, mit denen Routen gekennzeichnet sind, die gegenüber Microsoft angekündigt werden.

Wenn Sie an einem Peeringstandort innerhalb der AzureGov-Region per ExpressRoute eine Verbindung mit Microsoft herstellen, haben Sie Zugriff auf sämtliche Microsoft-Clouddienste in allen Regionen innerhalb des Regierungsgebiets. 

Wenn Sie also beispielsweise in Washington D.C. über ExpressRoute mit Microsoft verbunden sind, haben Sie Zugriff auf alle in AzureGov gehosteten Microsoft-Clouddienste.

Ausführliche Informationen zu Standorten und Partnern sowie eine detaillierte Liste mit ExpressRoute-Peeringstandorten für AzureGov finden Sie in der [öffentlichen ExpressRoute-Dokumentation](../expressroute/index.md) unter „Übersicht“.

Sie können mehrere ExpressRoute-Verbindungen erwerben. Wenn Sie über mehrere Verbindungen verfügen, ergeben sich für Sie aufgrund der Georedundanz daraus erhebliche Vorteile in Bezug auf hohe Verfügbarkeit. Bei Verwendung mehrerer ExpressRoute-Verbindungen erhalten Sie die gleiche Gruppe von Präfixen, die von Microsoft über die Pfade für das öffentliche Peering und das Microsoft-Peering angekündigt werden. Dies bedeutet, dass Sie dann mehrere Pfade aus dem Netzwerk zu Microsoft nutzen können. Dies kann unter Umständen dazu führen, dass in Ihrem Netzwerk suboptimale Routingentscheidungen getroffen werden. Daraus können sich für verschiedene Dienste suboptimale Konnektivitätsleistungen ergeben. 

Microsoft kennzeichnet Präfixe, die über das öffentliche Peering und Microsoft-Peering angekündigt werden, mit den passenden BGP-Communitywerten, um die Region anzugeben, in der die Präfixe gehostet werden. Sie können die Communitywerte nutzen, um die richtigen Routingentscheidungen zu treffen und Kunden optimales Routing zu bieten.  Weitere Informationen finden Sie in der [öffentlichen ExpressRoute-Dokumentation](../expressroute/index.md) unter „Erste Schritte“ > „Optimieren des Routings“.

| **Azure-Region für nationale Clouds**| **BGP-Communitywert** |
| --- | --- |
| **US Government** |  |
| US Government, Iowa | 12076:51109 |
| US Government, Virginia | 12076:51105 |

Alle Routen, die von Microsoft angekündigt werden, werden mit dem entsprechenden Communitywert gekennzeichnet. 

Zusätzlich zu den obigen Kennzeichnungen versieht Microsoft Präfixe auch basierend auf dem zugehörigen Dienst mit einer Kennzeichnung. Dies gilt nur für das Microsoft-Peering. Die Tabelle unten enthält die Zuordnung des Diensts zum BGP-Communitywert.

| **Dienst in nationalen Clouds** | **BGP-Communitywert** |
| --- | --- |
| **US Government** |  |
| Exchange Online |12076:5110 |
| SharePoint Online |12076:5120 |
| Skype for Business Online |12076:5130 |
| CRM Online |12076:5140 |
| Andere Office 365 Online-Dienste |12076:5200 |

> [!NOTE]
> Microsoft berücksichtigt keine BGP-Communitywerte, die von Ihnen für die gegenüber Microsoft angekündigten Routen festgelegt werden.

## <a name="support-for-load-balancer"></a>Load Balancer-Unterstützung
Load Balancer ist in Azure Government allgemein verfügbar. Weitere Informationen finden Sie in der [öffentlichen Load Balancer-Dokumentation](../load-balancer/load-balancer-overview.md). 

## <a name="support-for-traffic-manger"></a>Traffic Manager-Unterstützung
Traffic Manager ist in Azure Government allgemein verfügbar. Weitere Informationen finden Sie in der [öffentlichen Traffic Manager-Dokumentation](../traffic-manager/traffic-manager-overview.md). 

## <a name="support-for-vnet-peering"></a>VNet-Peering-Unterstützung 
VNet-Peering ist in Azure Government allgemein verfügbar. Weitere Informationen finden Sie in der [öffentlichen VNet-Peering-Dokumentation](../virtual-network/virtual-network-peering-overview.md). 

## <a name="support-for-vpn-gateway"></a>VPN Gateway-Unterstützung 
VPN Gateway ist in Azure Government allgemein verfügbar. Weitere Informationen finden Sie in der [öffentlichen VPN Gateway-Dokumentation](../vpn-gateway/vpn-gateway-about-vpngateways.md). 

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen und Updates erhalten Sie, indem Sie den <a href="https://blogs.msdn.microsoft.com/azuregov/">Microsoft Azure Government-Blog</a> abonnieren.




<!--HONumber=Jan17_HO5-->


