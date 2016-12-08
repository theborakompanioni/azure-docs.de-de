---
title: ExpressRoute-Standorte | Microsoft Docs
description: "Dieser Artikel enthält eine ausführliche Übersicht über die Standorte, an denen Dienste angeboten werden, sowie Informationen darüber, wie Sie eine Verbindung mit den Azure-Regionen herstellen können."
services: expressroute
documentationcenter: na
author: cherylmc
manager: carmonm
editor: 
ms.assetid: feb67da3-5abc-4acb-bad4-f78e3c541ded
ms.service: expressroute
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 11/28/2016
ms.author: cherylmc
translationtype: Human Translation
ms.sourcegitcommit: eb618aea1ee5aba24cebd4d8518e9c7f546f5f4f
ms.openlocfilehash: ae1e750a5f97325df064a02ed156610b4576f1d3


---
# <a name="expressroute-partners-and-peering-locations"></a>ExpressRoute-Partner und Peeringstandorte
In den Tabellen in diesem Artikel finden Sie Informationen zu ExpressRoute-Konnektivitätsanbietern, zum geografischen Geltungsbereich von ExpressRoute, zu Microsoft-Clouddiensten, die über ExpressRoute unterstützt werden, und zu ExpressRoute-Systemintegratoren (SIs).

## <a name="a-namepartnersaexpressroute-connectivity-providers"></a><a name="partners"></a>ExpressRoute-Konnektivitätsanbieter
ExpressRoute wird in allen Azure-Regionen und an allen Standorten unterstützt. Die folgende Karte zeigt die Azure-Regionen und ExpressRoute-Standorte. ExpressRoute-Standorte beziehen sich auf jene Orte, an denen Microsoft eine Peeringkooperation mit mehreren Service Providern bietet.

![Standortkarte][0]

Wenn Sie mit mindestens einem ExpressRoute-Standort innerhalb der geopolitischen Region eine Verbindung hergestellt haben, haben Sie Zugriff auf die Azure-Dienste in allen Regionen dieser geopolitischen Region. 

### <a name="azure-regions-to-expressroute-locations-within-a-geopolitical-region"></a>Azure-Regionen mit ExpressRoute-Standorten in einer geopolitischen Region
Die folgende Tabelle bietet eine Übersicht über die Azure-Regionen mit ExpressRoute-Standorten in einer geopolitischen Region.

| **Geopolitische Region** | **Azure-Regionen** | **ExpressRoute-Standorte** |
| --- | --- | --- |
| **Nordamerika** |USA, Osten; USA, Westen; USA, Osten 2; USA, Mitte; USA, Süden-Mitte; USA, Norden-Mitte; Kanada, Mitte; Kanada, Osten |Atlanta, Chicago, Dallas, Las Vegas, Los Angeles, New York, Seattle, Silicon Valley, Washington DC, Montreal+, Quebec City+, Toronto |
| **Südamerika** |Brasilien Süd |Sao Paulo |
| **Europa** |Europa, Norden, Europa, Westen, Großbritannien Westen, Großbritannien Süden |Amsterdam, Dublin, London, Newport(Wales)+, Paris |
| **Asien** |Ostasien, Südostasien |Hongkong, Singapur |
| **Japan** |Japan West, Japan Ost |Osaka, Tokio |
| **Australien** |Südostaustralien, Ostaustralien |Melbourne, Sydney |
| **Indien** |Indien, Westen, Indien, Mitte, Indien, Süden |Chennai, Mumbai |

### <a name="regions-and-geopolitical-boundaries-for-national-clouds"></a>Regionen und geopolitische Grenzen für nationale Clouds
In der folgenden Tabelle finden Sie Informationen zu Regionen und geopolitischen Grenzen für nationale Clouds.

| **Geopolitische Region** | **Azure-Regionen** | **ExpressRoute-Standorte** |
| --- | --- | --- | --- |
| **US-Government Cloud** |US-Behörde Iowa, US-Behörde Virginia |Chicago, Dallas, New York, Washington DC |
| **China** |China (Norden), China (Osten) |Peking, Shanghai |
| **Deutschland** |Deutschland, Mitte, Deutschland, Ost |Berlin, Frankfurt |

Konnektivität zwischen geopolitischen Regionen wird bei der ExpressRoute-Standard-SKU nicht unterstützt. Sie müssen das ExpressRoute Premium-Add-On aktivieren, um Unterstützung für globale Konnektivität zu erhalten. Verbindungen mit nationalen Cloudumgebungen werden nicht unterstützt. Wenden Sie sich an Ihren Konnektivitätsanbieter, wenn Sie derartige Verbindungen implementieren möchten.

## <a name="a-namelocationsaconnectivity-provider-locations"></a><a name="locations"></a>Standorte von Konnektivitätsanbietern
> [!div class="op_single_selector"]
> * [Standorte nach Anbieter](expressroute-locations.md#locations)
> * [Anbieter nach Standort](expressroute-locations-providers.md#locations)
> 
> 

### <a name="production-azure"></a>Azure-Produktionsumgebungen
| **Standort** | **Service Providers** |
| --- | --- |
| **Amsterdam** |Aryaka Networks, AT&T NetBond, British Telecom, Colt, Equinix, euNetworks, GÉANT, InterCloud, Internet Solutions – Cloud Connect, Interxion, Level 3 Communications, Orange, Tata Communications, TeleCity Group, Telenor, Verizon |
| **Atlanta** |Equinix |
| **Chennai** |SIFY, Tata Communications |
| **Chicago** |AT&T NetBond, Comcast, Equinix, Level 3 Communications, Zayo Group |
| **Dallas** |Aryaka Networks, AT&T NetBond, Cologix, Equinix, Level 3 Communications, Megaport |
| **Dublin** |Colt, Telecity Group |
| **Hongkong** |British Telecom, China Telecom Global, Equinix, Megaport, Orange, PCCW Global Limited, Tata Communications, Verizon |
| **London** |AT&T NetBond, British Telecom, Colt, Equinix, InterCloud, Internet Solutions - Cloud Connect, Interxion, Jisc, Level 3 Communications, MTN, NTT Communications, Orange, Tata Communications, Telecity Group, Telenor, Verizon, Vodafone |
| **Las Vegas** |Level 3 Communications+, Megaport |
| **Los Angeles** |CoreSite, Equinix, Megaport, NTT, Zayo Group |
| **Melbourne** |AARNet, Equinix, Megaport, NEXTDC, Telstra Corporation |
| **New York** |Equinix, Megaport, Zayo Group |
| **Newport(Wales)+** |Daten der nächsten Generation |
| **Montreal** |Cologix+ |
| **Mumbai** |Tata Communications |
| **Osaka** |Equinix, Internet Initiative Japan Inc. - IIJ, NTT Communications, Softbank |
| **Paris** |Interxion, Equinix+ |
| **Sao Paulo** |Equinix, Telefonica |
| **Seattle** |Equinix, Level 3 Communications, Megaport |
| **Silicon Valley** |Aryaka Networks, AT&T NetBond, British Telecom, CenturyLink+, Comcast, Equinix, Level 3 Communications, Orange, Tata Communications, Verizon, Zayo Group |
| **Singapur** |Aryaka Networks, AT&T NetBond, British Telecom, Equinix, InterCloud, Megaport, NTT Communications, Orange, SingTel, Tata Communications, Verizon |
| **Sydney** |AARNet, AT&T NetBond, British Telecom, Equinix, Megaport, NEXTDC, Orange, Telstra Corporation, Verizon |
| **Tokio** |Aryaka Networks, British Telecom, Colt, Equinix, Internet Initiative Japan Inc. - IIJ, NTT Communications, Softbank, Verizon |
| **Toronto** |Cologix, Equinix, Megaport, Zayo Group |
| **Washington DC** |Aryaka Networks, AT&T NetBond, British Telecom, Comcast, Equinix, InterCloud, Level 3 Communications, Megaport, NTT Communications, Orange, Tata Communications, Verizon, Zayo Group |

 **+** steht für "In Kürze"

### <a name="national-cloud-environments"></a>Nationale Cloudumgebungen

### <a name="us-government-cloud"></a>US-Government Cloud
| **Standort** | **Service Providers** |
| --- | --- |
| **Chicago** |AT&T NetBond, Equinix, Level 3 Communications, Verizon |
| **Dallas** |Equinix, Verizon |
| **New York** |Equinix, Level 3 Communications+, Verizon |
| **Washington DC** |AT&T NetBond, Equinix, Level 3 Communications, Verizon |

### <a name="china"></a>China
| **Standort** | **Service Providers** |
| --- | --- |
| **Peking** |China Telecom |
| **Shanghai** |China Telecom |

Weitere Informationen finden Sie unter [ExpressRoute in China](http://www.windowsazure.cn/home/features/expressroute/)

### <a name="germany"></a>Deutschland
| **Standort** | **Service Providers** |
| --- | --- |
| **Berlin** |Colt+, e-shelter |
| **Frankfurt** |Colt, Equinix, Interxion |

## <a name="a-namenonpartnersaconnectivity-through-service-providers-not-listed"></a><a name="nonpartners"></a>Konnektivität über nicht aufgeführte Service Providers
Wenn Ihr Konnektivitätsanbieter nicht in den vorherigen Abschnitten enthalten ist, können Sie dennoch eine Verbindung erstellen.

* Fragen Sie Ihren Konnektivitätsanbieter, ob eine Verbindung mit einem der Exchange-Standorte aus der vorherigen Tabelle bereitgestellt werden kann. Sie können auch die folgenden Links überprüfen, um weitere Informationen über die von den Exchange-Anbietern angebotenen Dienste zu erhalten. Viele Konnektivitätsanbieter sind bereits mit Ethernet-Exchanges verbunden.
  * [Cologix](http://www.cologix.com/)
  * [CoreSite](http://www.coresite.com/)
  * [Equinix Cloud Exchange](http://www.equinix.com/services/interconnection-connectivity/cloud-exchange/)
  * [InterXion](http://www.interxion.com/)
  * [NextDC](http://www.nextdc.com/)
  * [Megaport](https://www.megaport.com/services/microsoft-expressroute/)
  * [TeleCity CloudIX](http://www.telecitygroup.com/colocation-services/cloud-ix.htm)
* Bitten Sie Ihren Konnektivitätsanbieter, Ihr Netzwerk auf den Peeringstandort Ihrer Wahl zu erweitern.
  * Stellen Sie sicher, dass Ihr Konnektivitätsanbieter Ihre Konnektivität mit hoher Verfügbarkeit erweitert, sodass es keine einzelnen Fehlerquellen mehr gibt.
* Bestellen Sie eine ExpressRoute-Verbindung mit der Exchange als Konnektivitätsanbieter für die Verbindung mit Microsoft.
  * Führen Sie die Schritte in [Erstellen einer ExpressRoute-Verbindung](expressroute-howto-circuit-classic.md) aus, um die Verbindung einzurichten.

| **Standort** | **Exchange** | **Konnektivitätsanbieter** |
| --- | --- | --- |
| **New York** |Equinix |Lightower |
| **Seattle** |Equinix |Alaska Communications |
| **Silicon Valley** |Equinix |XO Communications |
| **Singapur** |Equinix |1CLOUDSTAR |
| **Washington DC** |Equinix |Lightower |

## <a name="expressroute-system-integrators"></a>ExpressRoute-Systemintegratoren
Je nach Ihrer Netzwerkgröße kann das Aktivieren privater Verbindungen für Ihre Anforderungen problematisch sein. Alle Systemintegratoren, die in der folgenden Tabelle aufgeführt sind, können Ihnen beim Integrieren von ExpressRoute helfen.

| **Kontinent** | **Systemintegratoren** |
| --- | --- |
| **Asien** |Avanade Inc., OneAs1a |
| **Europa** |Avanade Inc., Dotnet Solutions |
| **USA** |Avanade Inc., Equinix Professional Services, Perficient, Project Leadership |

## <a name="next-steps"></a>Nächste Schritte
* Weitere Informationen über ExpressRoute finden Sie unter [ExpressRoute – FAQ](expressroute-faqs.md).
* Stellen Sie sicher, dass alle Voraussetzungen erfüllt sind. Informationen finden Sie unter [ExpressRoute-Voraussetzungen](expressroute-prerequisites.md).

<!--Image References-->
[0]: ./media/expressroute-locations/expressroute-locations-map.png "Standortkarte"



<!--HONumber=Nov16_HO5-->


