---
title: "Konnektivitätsanbieter und Standorte: Azure ExpressRoute | Microsoft-Dokumentation"
description: "Dieser Artikel enthält eine ausführliche Übersicht über die Standorte, an denen Dienste angeboten werden, sowie Informationen darüber, wie Sie eine Verbindung mit den Azure-Regionen herstellen können. Sortiert nach Konnektivitätsanbieter."
services: expressroute
documentationcenter: na
author: cherylmc
manager: timlt
editor: 
ms.assetid: c878513a-d594-42ad-8b0e-403efd0c4b25
ms.service: expressroute
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/13/2017
ms.author: cherylmc
translationtype: Human Translation
ms.sourcegitcommit: 4f20e8d3d5774eb57038e40fac6a4bac2e17909e
ms.openlocfilehash: 569786c864d5f8ceb93422c65ae1ffb47f66a787
ms.lasthandoff: 03/01/2017


---
# <a name="expressroute-partners-and-peering-locations"></a>ExpressRoute-Partner und Peeringstandorte

> [!div class="op_single_selector"]
> * [Standorte nach Anbieter](expressroute-locations.md)
> * [Anbieter nach Standort](expressroute-locations-providers.md)


In den Tabellen in diesem Artikel finden Sie Informationen zu ExpressRoute-Konnektivitätsanbietern, zum geografischen Geltungsbereich von ExpressRoute, zu Microsoft-Clouddiensten, die über ExpressRoute unterstützt werden, und zu ExpressRoute-Systemintegratoren (SIs).

## <a name="a-namepartnersaexpressroute-connectivity-providers"></a><a name="partners"></a>ExpressRoute-Konnektivitätsanbieter
ExpressRoute wird in allen Azure-Regionen und an allen Standorten unterstützt. Die folgende Karte zeigt die Azure-Regionen und ExpressRoute-Standorte. ExpressRoute-Standorte beziehen sich auf jene Orte, an denen Microsoft eine Peeringkooperation mit mehreren Service Providern bietet.

![Standortkarte][0]

Wenn Sie mit mindestens einem ExpressRoute-Standort innerhalb der geopolitischen Region eine Verbindung hergestellt haben, haben Sie Zugriff auf die Azure-Dienste in allen Regionen dieser geopolitischen Region.

### <a name="azure-regions-to-expressroute-locations-within-a-geopolitical-region"></a>Azure-Regionen mit ExpressRoute-Standorten in einer geopolitischen Region
Die folgende Tabelle bietet eine Übersicht über die Azure-Regionen mit ExpressRoute-Standorten in einer geopolitischen Region.

| **Geopolitische Region** | **Azure-Regionen** | **ExpressRoute-Standorte** |
| --- | --- | --- |
| **Nordamerika** |USA, Osten; USA, Westen; USA, Osten 2; USA, Westen 2; USA, Mitte; USA, Süden-Mitte; USA, Norden-Mitte; USA, Westen-Mitte; Kanada, Mitte; Kanada, Osten |Atlanta, Chicago, Dallas, Las Vegas, Los Angeles, New York, Seattle, Silicon Valley, Washington DC, Montreal, Québec (Stadt), Toronto |
| **Südamerika** |Brasilien Süd |Sao Paulo |
| **Europa** |Europa, Norden, Europa, Westen, Großbritannien Westen, Großbritannien Süden |Amsterdam, Dublin, London, Newport (Wales), Paris |
| **Asien** |Ostasien, Südostasien |Hongkong, Singapur |
| **Japan** |Japan West, Japan Ost |Osaka, Tokio |
| **Australien** |Südostaustralien, Ostaustralien |Melbourne, Sydney |
| **Indien** |Indien, Westen, Indien, Mitte, Indien, Süden |Chennai, Mumbai |
| **Südkorea** |Korea, Mitte, Korea, Süden |Busan, Seoul |

### <a name="regions-and-geopolitical-boundaries-for-national-clouds"></a>Regionen und geopolitische Grenzen für nationale Clouds
In der folgenden Tabelle finden Sie Informationen zu Regionen und geopolitischen Grenzen für nationale Clouds.

| **Geopolitische Region** | **Azure-Regionen** | **ExpressRoute-Standorte** |
| --- | --- | --- | --- |
| **US-Government Cloud** |USA Gov Iowa, USA Gov Virginia, US DoD, Mitte+, US DoD, Osten  |Chicago, Dallas, New York, Silicon Valley, Washington DC |
| **China** |China (Norden), China (Osten) |Peking, Shanghai |
| **Deutschland** |Deutschland, Mitte, Deutschland, Ost |Berlin, Frankfurt |

Konnektivität zwischen geopolitischen Regionen wird bei der ExpressRoute-Standard-SKU nicht unterstützt. Sie müssen das ExpressRoute Premium-Add-On aktivieren, um Unterstützung für globale Konnektivität zu erhalten. Verbindungen mit nationalen Cloudumgebungen werden nicht unterstützt. Wenden Sie sich an Ihren Konnektivitätsanbieter, wenn Sie derartige Verbindungen implementieren möchten.

## <a name="a-namelocationsaconnectivity-provider-locations"></a><a name="locations"></a>Standorte von Konnektivitätsanbietern

Die folgende Tabelle enthält die Standorte nach Service Provider. [Hier](expressroute-locations-providers.md#locations) finden Sie eine Tabelle, die nach den verfügbaren Service Providern mit den jeweiligen Standorten sortiert ist.


### <a name="production-azure"></a>Azure-Produktionsumgebungen
| **Service Provider** | **Microsoft Azure** | **Office 365 und CRM Online** | **Standorte** |
| --- | --- | --- | --- |
| **[AARNet](https://www.aarnet.edu.au/network-and-services/cloud-services-applications/azure-expressroute/)** |Unterstützt |Unterstützt |Melbourne, Sydney |
| **Airtel** | In Kürze verfügbar | In Kürze verfügbar | Chennai, Mumbai |
| **[Aryaka Networks](http://www.aryaka.com/)** |Unterstützt |Unterstützt |Amsterdam, Dallas, Silicon Valley, Singapur, Tokio, Washington, D.C. |
| **[AT&T NetBond](https://www.synaptic.att.com/clouduser/html/productdetail/ATT_NetBond.htm)** |Unterstützt |Unterstützt |Amsterdam, Chicago, Dallas, London, Silicon Valley, Singapore, Sydney, Washington DC |
| **[Bell Canada](https://business.bell.ca/shop/enterprise/cloud-connect-access-to-cloud-partner-services)** |Unterstützt |Unterstützt |Toronto |
| **[British Telecom](http://www.globalservices.bt.com/uk/en/news/bt_to_provide_connectivity_to_microsoft_azure)** |Unterstützt |Unterstützt |Amsterdam, Hongkong, London, Silicon Valley, Singapur, Sydney, Tokio, Washington DC |
| **[CenturyLink](http://www.centurylink.com/business/enterprise/services/data-network/mpls-vpn.html)** |In Kürze verfügbar |In Kürze verfügbar |Silicon Valley |
| **China Telecom Global** |Unterstützt |Nicht unterstützt |Hongkong |
| **[Cologix](http://www.cologix.com/solutions/cloud-connect/public-clouds/microsoft-cloud/)** |Unterstützt |Unterstützt |Dallas, Montreal, Toronto |
| **[Colt](http://www.colt.net/uk/en/news/colt-announces-dedicated-cloud-access-for-microsoft-azure-services-en.htm)** |Unterstützt |Unterstützt |Amsterdam, Dublin, London, Tokio |
| **Comcast** |Unterstützt |Unterstützt |Chicago, Silicon Valley, Washington, D.C. |
| **Console**| Unterstützt | Unterstützt |Silicon Valley, Toronto |
| **[CoreSite](http://www.coresite.com/solutions/cloud-services/public-cloud-providers/microsoft-azure-expressroute)** |Unterstützt |Unterstützt |Los Angeles, New York |
| **[Equinix](http://www.equinix.com/partners/microsoft-azure/)** |Unterstützt |Unterstützt |Amsterdam, Atlanta, Chicago, Dallas, Hongkong, London, Los Angeles, Melbourne, New York, Osaka, Sao Paulo, Seattle, Silicon Valley, Singapur, Sydney, Tokio, Toronto, Washington DC |
| **euNetworks** |Unterstützt |Unterstützt |Amsterdam |
| **Global CloudXchange (GCX)** | Unterstützt| Unterstützt | Chennai |
| **GÉANT** |Unterstützt |Unterstützt |Amsterdam |
| **[Internet Initiative Japan Inc. - IIJ](http://www.iij.ad.jp/en/news/pressrelease/2015/1216-2.html)** |Unterstützt |Unterstützt |Osaka, Tokio |
| **[InterCloud](https://www.intercloud.com/)** |Unterstützt |Unterstützt |Amsterdam, London, Singapur, Washington, D.C. |
| **Internet Solutions – Cloud Connect** |Unterstützt |Unterstützt |Amsterdam, London |
| **[Interxion](http://www.interxion.com/why-interxion/colocate-with-the-clouds/Microsoft-Azure/)** |Unterstützt |Unterstützt |Amsterdam, London, Paris |
| **Jisc** |Unterstützt |Unterstützt |London |
| **KINX** |Unterstützt |Unterstützt |Seoul |
| **[KPN](http://www.kpn.com/cloudconnect)** | Unterstützt | Unterstützt | Amsterdam | 
| **[Level 3 Communications](http://your.level3.com/LP=882?WT.tsrc=02192014LP882AzureVanityAzureText)** |Unterstützt |Unterstützt |Amsterdam, Chicago, Dallas, Las Vegas+, London, Seattle, Silicon Valley, Singapur, Washington DC |
| **LG CNS** |In Kürze verfügbar |In Kürze verfügbar |Busan+ |
| **[Megaport](https://www.megaport.com/services/microsoft-expressroute/)** |Unterstützt |Unterstützt |Dallas, Hongkong, Las Vegas, Los Angeles, Melbourne, New York, Québec (Stadt), Seattle, Singapur, Sydney, Toronto, Washington DC |
| **MTN** |Unterstützt |Unterstützt |London |
| **[Daten der nächsten Generation](http://www.nextgenerationdata.co.uk/ngd-cloud-gateway/)** |Unterstützt |Unterstützt |Newport(Wales) |
| **NEXTDC** |Unterstützt |Unterstützt |Melbourne, Sydney |
| **[NTT Communications](http://www.ntt.com/en/services/network/virtual-private-network.html)** |Unterstützt |Unterstützt |London, Los Angeles, Osaka, Singapur, Tokio, Washington, D.C. |
| **[Orange](http://www.orange-business.com/en/products/business-vpn-galerie)** |Unterstützt |Unterstützt |Amsterdam, Hongkong, London, Silicon Valley, Singapur, Sydney, Washington DC |
| **PCCW Global Limited** |Unterstützt |Unterstützt |Hongkong |
| **Sejong Telecom** |Unterstützt |Unterstützt |Busan, Seoul |
| **[SIFY](http://telecom.sify.com/azure-expressroute.html)** |Unterstützt |Unterstützt |Chennai |
| **[SingTel](http://info.singtel.com/about-us/news-releases/singtel-provide-secure-private-access-microsoft-azure-public-cloud)** |Unterstützt |Unterstützt |Singapur |
| **SoftBank** |Unterstützt |Unterstützt |Osaka, Tokio |
| **[Tata Communications](http://www.tatacommunications.com/lp/izo/azure/azure_index.html)** |Unterstützt |Unterstützt |Amsterdam, Chennai, Hongkong, London, Mumbai, Silicon Valley, Singapur, Washington DC |
| **[TeleCity Group](http://www.telecitygroup.com/investor-centre/news_details.htm?locid=03100500400b00d&xml)** |Unterstützt |Unterstützt |Amsterdam, Dublin, London |
| **[Telefonica](https://www.business-solutions.telefonica.com/es/enterprise/solutions/efficient-infrastructure/managed-voice-data-connectivity/)** |Unterstützt |Unterstützt |Sao Paulo |
| **Telenor** |Unterstützt |Unterstützt |Amsterdam, London |
| **[Telstra Corporation](http://www.telstra.com.au/business-enterprise/network-services/networks/cloud-direct-connect/)** |Unterstützt |Unterstützt |Melbourne, Sydney |
| **[Verizon](http://www.verizonenterprise.com/products/networking/secure-cloud-interconnect/)** |Unterstützt |Unterstützt |Amsterdam, Hongkong, London, Silicon Valley, Singapur, Sydney, Tokio, Washington DC |
| **[Vodafone](http://www.vodafone.com/business/global-enterprise/global-connectivity/vodafone-ip-vpn-cloud-connect)** |Unterstützt |Nicht unterstützt |London |
| **[Zayo Group](http://www.zayo.com/solutions/industries/cloud-connectivity/microsoft-expressroute)** |Unterstützt |Unterstützt |Chicago, Los Angeles, New York, Silicon Valley, Toronto, Washington DC |

 **+** steht für "In Kürze"

### <a name="national-cloud-environment"></a>Nationale Cloudumgebungen

### <a name="us-government-cloud"></a>US-Government Cloud
| **Service Provider** | **Microsoft Azure** | **Office 365** | **Standorte** |
| --- | --- | --- | --- |
| **[AT&T NetBond](https://www.synaptic.att.com/clouduser/html/productdetail/ATT_NetBond.htm)** |Unterstützt |Unterstützt |Chicago, Washington DC |
| **[Equinix](http://www.equinix.com/partners/microsoft-azure/)** |Unterstützt |Unterstützt |Chicago, Dallas, New York, Silicon Valley, Washington DC |
| **[Level 3 Communications](http://your.level3.com/LP=882?WT.tsrc=02192014LP882AzureVanityAzureText)** |Unterstützt |Unterstützt |Chicago, New York+, Washington DC |
| **[Megaport](https://www.megaport.com/services/microsoft-expressroute/)** |Unterstützt | Unterstützt | Dallas |
| **[Verizon](http://news.verizonenterprise.com/2014/04/secure-cloud-interconnect-solutions-enterprise/)** |Unterstützt |Unterstützt |Chicago, Dallas, New York, Washington DC |

### <a name="china"></a>China
| **Service Provider** | **Microsoft Azure** | **Office 365** | **Standorte** |
| --- | --- | --- | --- |
| **China Telecom** |Unterstützt |Nicht unterstützt |Peking, Shanghai |

Weitere Informationen finden Sie unter [ExpressRoute in China](http://www.windowsazure.cn/home/features/expressroute/).

### <a name="germany"></a>Deutschland
| **Service Provider** | **Microsoft Azure** | **Office 365** | **Standorte** |
| --- | --- | --- | --- |
| **[Colt](http://www.colt.net/uk/en/news/colt-announces-dedicated-cloud-access-for-microsoft-azure-services-en.htm)** |Unterstützt |Nicht unterstützt |Berlin+, Frankfurt |
| **[Equinix](http://www.equinix.com/partners/microsoft-azure/)** |Unterstützt |Nicht unterstützt |Frankfurt |
| **e-shelter** |Unterstützt |Nicht unterstützt |Berlin |
| **Interxion** |Unterstützt |Nicht unterstützt |Frankfurt |
| **[Megaport](https://www.megaport.com/services/microsoft-expressroute/)** |Unterstützt  | Nicht unterstützt | Berlin |

## <a name="a-namenonpartnersaconnectivity-through-service-providers-not-listed"></a><a name="nonpartners"></a>Konnektivität über nicht aufgeführte Service Providers
Wenn Ihr Konnektivitätsanbieter nicht in den vorherigen Abschnitten enthalten ist, können Sie dennoch eine Verbindung erstellen.

* Fragen Sie Ihren Konnektivitätsanbieter, ob eine Verbindung mit einem der Exchange-Standorte aus der vorherigen Tabelle bereitgestellt werden kann. Sie können auch die folgenden Links überprüfen, um weitere Informationen über die von den Exchange-Anbietern angebotenen Dienste zu erhalten. Viele Konnektivitätsanbieter sind bereits mit Ethernet-Exchanges verbunden.
  * [Cologix](http://www.cologix.com/)
  * [CoreSite](http://www.coresite.com/)
  * [Equinix Cloud Exchange](http://www.equinix.com/services/interconnection-connectivity/cloud-exchange/)
  * [Interxion](http://www.interxion.com/products/interconnection/cloud-connect/)
  * [Megaport](https://www.megaport.com/services/microsoft-expressroute/)
  * [NextDC](http://www.nextdc.com/)
  * [TeleCity CloudIX](http://www.telecitygroup.com/colocation-services/cloud-ix.htm)
* Bitten Sie Ihren Konnektivitätsanbieter, Ihr Netzwerk auf den Peeringstandort Ihrer Wahl zu erweitern.
  * Stellen Sie sicher, dass Ihr Konnektivitätsanbieter Ihre Konnektivität mit hoher Verfügbarkeit erweitert, sodass es keine einzelnen Fehlerquellen mehr gibt.
* Bestellen Sie eine ExpressRoute-Verbindung mit der Exchange als Konnektivitätsanbieter für die Verbindung mit Microsoft.
  * Führen Sie die Schritte in [Erstellen einer ExpressRoute-Verbindung](expressroute-howto-circuit-classic.md) aus, um die Verbindung einzurichten.

| **Konnektivitätsanbieter** | **Exchange** | **Standorte** |
| --- | --- | --- |
| **[1CLOUDSTAR](http://www.1cloudstar.com/service/cloudconnect-azure-expressroute/)** |Equinix |Singapur |
| **[Airgate Technologies, Inc.](http://airgate.ca/cloud-express/)** | Equinix, Cologix | Toronto, Montreal |
| **[Arteria Networks Corporation](https://arteria-net.com/business/service/cloud_access/sca/)** |Equinix |Tokio |
| **[Alaska Communications](http://www.alaskacommunications.com/For-Your-Business/Direct-Cloud-Service)** |Equinix |Seattle |
| **[Bezeq International Ltd.](https://www.bezeqint.net/english)** | euNetworks | London
| **[Cogeco Peer 1](https://www.cogecopeer1.com/en/)**| Equinix | Montreal, Toronto |
| **[Data Foundry](https://www.datafoundry.com/services/cloud-connect)** | Megaport | Dallas
| **[Eurofiber](https://eurofiber.nl/microsoft-azure/)** | Equinix | Amsterdam |
| **[Exponential E](http://www.exponential-e.com/services/connectivity-services/cloud-connect-exchange)** | Equinix | London |
| **[Fastweb S.p.A](http://www.fastweb.it/grandi-aziende/connessione-voce-e-wifi/scheda-prodotto/rete-privata-virtuale/)** | Equinix | Amsterdam |
| **[HSO](http://www.hso.co.uk/products/cloud-direct)** |Equinix | London, Slough |
| **[Lightower](http://www.lightower.com/network-solutions/cloud-connect/#microsoft-azure)** |Equinix |New York, Washington DC |
| **[Macquarie Telecom Group](https://macquariegovernment.com/secure-cloud/secure-cloud-exchange/)** | Megaport | Sydney |
| **[Masergy](https://www.masergy.com/solutions/hybrid-networking/cloud-marketplace/microsoft-azure)** | Equinix | Washington DC |
| **[Nianet](https://nianet.dk/produkter/internet/microsoft-expressroute)** |Telecity | Amsterdam, Frankfurt |  
| **[Tamares Telecom](http://www.tamarestelecom.com/our-services/#Connectivity)** | Telecity | London
| **[Transtelco](http://www.transtelco.net/tcloud/microsoft)** |Equinix | Dallas, Los Angeles |  
| **[QSC AG](https://www.qsc.de/de/produkte-loesungen/cloud-services-und-it-outsourcing/pure-enterprise-cloud/multi-cloud-management/azure-expressroute/)** |Interxion | Frankfurt |  
| **[Windstream](http://www.windstreambusiness.com/solutions/cloud-services/cloud-and-managed-hosting-services)**| Equinix |Chicago, Silicon Valley, Washington, D.C. |
| **[Zertia](http://www.zertia.es/index.php/novedades)**| Level 3 | Madrid |


## <a name="expressroute-system-integrators"></a>ExpressRoute-Systemintegratoren
Je nach Ihrer Netzwerkgröße kann das Aktivieren privater Verbindungen für Ihre Anforderungen problematisch sein. Alle Systemintegratoren, die in der folgenden Tabelle aufgeführt sind, können Ihnen beim Integrieren von ExpressRoute helfen.

| **Systemintegrator** | **Kontinent** |
| --- | --- |
| **[Altogee](http://www.altogee.be/expressroute)** | Europa |
| **[Avanade Inc.](http://www.avanade.com/)** | Asien, Europa, Nordamerika, Südamerika |
| **Bright Skies GmbH** | Europa
| **[Dotnet Solutions](http://www.dotnetsolutions.co.uk/)** | Europa |
| **[Equinix Professional Services](http://www.equinix.com/services/consulting/)** | Nordamerika |
| **[The IT Consultancy Group](http://itconsult.com.au/microsoft-expressroute)** | Australien |
| **[MSG Services](https://www.msg-services.de/it-services/managed-services/cloud-outsourcing/)** | Europa (Deutschland) |
| **[Nelite](http://nelite.com/)** | Europa |
| **[OneAs1a](http://www.oneas1a.com/express-connect-any-cloud-ecac)** | Asien |
| **[Orange Networks](https://orange-networks.com/blog/88-azureexpressroute)** | Europa |
| **[Perficient](http://www.perficient.com/Partners/Microsoft/Cloud/Azure-ExpressRoute)** | Nordamerika |
| **[Project Leadership](http://www.projectleadership.net/azure)** | Nordamerika |


## <a name="next-steps"></a>Nächste Schritte
* Weitere Informationen über ExpressRoute finden Sie unter [ExpressRoute – FAQ](expressroute-faqs.md).
* Stellen Sie sicher, dass alle Voraussetzungen erfüllt sind. Informationen finden Sie unter [ExpressRoute-Voraussetzungen](expressroute-prerequisites.md).

<!--Image References-->
[0]: ./media/expressroute-locations/expressroute-locations-map.png "Standortkarte"

