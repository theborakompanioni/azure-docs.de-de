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
ms.date: 06/30/2017
ms.author: kaanan
ms.translationtype: Human Translation
ms.sourcegitcommit: 6dbb88577733d5ec0dc17acf7243b2ba7b829b38
ms.openlocfilehash: 6286e059da7df95303bca00b5910632302bd19b7
ms.contentlocale: de-de
ms.lasthandoff: 07/04/2017


---
<a id="expressroute-partners-and-peering-locations" class="xliff"></a>

# ExpressRoute-Partner und Peeringstandorte

> [!div class="op_single_selector"]
> * [Standorte nach Anbieter](expressroute-locations.md)
> * [Anbieter nach Standort](expressroute-locations-providers.md)


In den Tabellen in diesem Artikel finden Sie Informationen zu ExpressRoute-Konnektivitätsanbietern, zum geografischen Geltungsbereich von ExpressRoute, zu Microsoft-Clouddiensten, die über ExpressRoute unterstützt werden, und zu ExpressRoute-Systemintegratoren (SIs).

## <a name="partners"></a>ExpressRoute-Konnektivitätsanbieter
ExpressRoute wird in allen Azure-Regionen und an allen Standorten unterstützt. Die folgende Karte zeigt die Azure-Regionen und ExpressRoute-Standorte. ExpressRoute-Standorte beziehen sich auf jene Orte, an denen Microsoft eine Peeringkooperation mit mehreren Service Providern bietet.

![Standortkarte][0]

Wenn Sie mit mindestens einem ExpressRoute-Standort innerhalb der geopolitischen Region eine Verbindung hergestellt haben, haben Sie Zugriff auf die Azure-Dienste in allen Regionen dieser geopolitischen Region.

<a id="azure-regions-to-expressroute-locations-within-a-geopolitical-region" class="xliff"></a>

### Azure-Regionen mit ExpressRoute-Standorten in einer geopolitischen Region
Die folgende Tabelle bietet eine Übersicht über die Azure-Regionen mit ExpressRoute-Standorten in einer geopolitischen Region.

| **Geopolitische Region** | **Azure-Regionen** | **ExpressRoute-Standorte** |
| --- | --- | --- |
| **Nordamerika** |USA, Osten; USA, Westen; USA, Osten 2; USA, Westen 2; USA, Mitte; USA, Süden-Mitte; USA, Norden-Mitte; USA, Westen-Mitte; Kanada, Mitte; Kanada, Osten |Atlanta, Chicago, Dallas, Denver, Las Vegas, Los Angeles, Miami, New York, San Antonio, Seattle, Silicon Valley, Washington DC, Montreal, Quebec City, Toronto |
| **Südamerika** |Brasilien Süd |Sao Paulo |
| **Europa** |Europa, Norden, Europa, Westen, Großbritannien Westen, Großbritannien Süden |Amsterdam, Dublin, London, Newport (Wales), Paris |
| **Asien** |Ostasien, Südostasien |Hongkong, Singapur |
| **Japan** |Japan West, Japan Ost |Osaka, Tokio |
| **Australien** |Südostaustralien, Ostaustralien |Melbourne, Sydney |
| **Indien** |Indien, Westen, Indien, Mitte, Indien, Süden |Chennai, Mumbai |
| **Südkorea** |Korea, Mitte, Korea, Süden |Busan, Seoul |

<a id="regions-and-geopolitical-boundaries-for-national-clouds" class="xliff"></a>

### Regionen und geopolitische Grenzen für nationale Clouds
In der folgenden Tabelle finden Sie Informationen zu Regionen und geopolitischen Grenzen für nationale Clouds.

| **Geopolitische Region** | **Azure-Regionen** | **ExpressRoute-Standorte** |
| --- | --- | --- |
| **US-Government Cloud** |USA Gov Iowa, USA Gov Virginia, US DoD, Mitte+, US DoD, Osten  |Chicago, Dallas, New York, Seattle, Silicon Valley, Washington DC |
| **China** |China (Norden), China (Osten) |Peking, Shanghai |
| **Deutschland** |Deutschland, Mitte, Deutschland, Ost |Berlin, Frankfurt |

Konnektivität zwischen geopolitischen Regionen wird bei der ExpressRoute-Standard-SKU nicht unterstützt. Sie müssen das ExpressRoute Premium-Add-On aktivieren, um Unterstützung für globale Konnektivität zu erhalten. Verbindungen mit nationalen Cloudumgebungen werden nicht unterstützt. Wenden Sie sich an Ihren Konnektivitätsanbieter, wenn Sie derartige Verbindungen implementieren möchten.

## <a name="locations"></a>Standorte von Konnektivitätsanbietern

Die folgende Tabelle enthält die Standorte nach Service Provider. [Hier](expressroute-locations-providers.md#locations) finden Sie eine Tabelle, die nach den verfügbaren Service Providern mit den jeweiligen Standorten sortiert ist.


<a id="production-azure" class="xliff"></a>

### Azure-Produktionsumgebungen
| **Service Provider** | **Microsoft Azure** | **Office 365 und Dynamics 365** | **Standorte** |
| --- | --- | --- | --- |
| **[AARNet](https://www.aarnet.edu.au/network-and-services/cloud-services-applications/azure-expressroute/)** |Unterstützt |Unterstützt |Melbourne, Sydney |
| **[Airtel](http://www.airtel.in/business/connexion)** | Unterstützt | Unterstützt | Chennai, Mumbai |
| **[Aryaka Networks](http://www.aryaka.com/)** |Unterstützt |Unterstützt |Amsterdam, Dallas, Hongkong, Silicon Valley, Singapur, Tokio, Washington DC |
| **[Ascenty-Rechenzentren](https://ascenty.com/solucoes/conectividade-e-interconexoes/Microsoft-express-route/)** |In Kürze verfügbar |In Kürze verfügbar |Sao Paulo |
| **[AT&T NetBond](https://www.synaptic.att.com/clouduser/html/productdetail/ATT_NetBond.htm)** |Unterstützt |Unterstützt |Amsterdam, Chicago, Dallas, London, Silicon Valley, Singapur, Sydney, Tokio, Toronto, Washington DC |
| **[Bell Canada](https://business.bell.ca/shop/enterprise/cloud-connect-access-to-cloud-partner-services)** |Unterstützt |Unterstützt |Montreal, Toronto |
| **[British Telecom](http://www.globalservices.bt.com/uk/en/news/bt_to_provide_connectivity_to_microsoft_azure)** |Unterstützt |Unterstützt |Amsterdam, Hongkong, London, Silicon Valley, Singapur, Sydney, Tokio, Washington DC |
| **[CenturyLink](http://www.centurylink.com/business/enterprise/services/data-network/mpls-vpn.html)** |In Kürze verfügbar |In Kürze verfügbar |Silicon Valley |
| **China Telecom Global** |Unterstützt |Nicht unterstützt |Hongkong |
| **[Cologix](http://www.cologix.com/solutions/cloud-connect/public-clouds/microsoft-cloud/)** |Unterstützt |Unterstützt |Dallas, Montreal, Toronto |
| **[Colt](http://www.colt.net/uk/en/news/colt-announces-dedicated-cloud-access-for-microsoft-azure-services-en.htm)** |Unterstützt |Unterstützt |Amsterdam, Dublin, London, Paris, Tokio |
| **Comcast** |Unterstützt |Unterstützt |Chicago, Silicon Valley, Washington, D.C. |
| **[Konsole](https://www.consoleconnect.com/partners/cloudsaas/)**| Unterstützt | Unterstützt |Silicon Valley, Toronto |
| **[CoreSite](http://www.coresite.com/solutions/cloud-services/public-cloud-providers/microsoft-azure-expressroute)** |Unterstützt |Unterstützt |Denver, Los Angeles, New York |
| **[Equinix](http://www.equinix.com/partners/microsoft-azure/)** |Unterstützt |Unterstützt |Amsterdam, Atlanta, Chicago, Dallas, Hongkong, London, Los Angeles, Melbourne, New York, Osaka, Paris, Sao Paulo, Seattle, Silicon Valley, Singapur, Sydney, Tokio, Toronto, Washington DC |
| **euNetworks** |Unterstützt |Unterstützt |Amsterdam |
| **GÉANT** |Unterstützt |Unterstützt |Amsterdam |
| **[Global Cloud Xchange (GCX)] (http://globalcloudxchange.com/cloud-platform/cloud-x-fusion/cloud-x-fusion-for-azure/)** | Unterstützt| Unterstützt | Chennai |
| **[InterCloud](https://www.intercloud.com/)** |Unterstützt |Unterstützt |Amsterdam, London, Singapur, Washington, D.C. |
| **[Internet Initiative Japan Inc. - IIJ](http://www.iij.ad.jp/en/news/pressrelease/2015/1216-2.html)** |Unterstützt |Unterstützt |Osaka, Tokio |
| **Internet Solutions – Cloud Connect** |Unterstützt |Unterstützt |Amsterdam, London |
| **[Interxion](http://www.interxion.com/why-interxion/colocate-with-the-clouds/Microsoft-Azure/)** |Unterstützt |Unterstützt |Amsterdam, London, Paris |
| **Jisc** |Unterstützt |Unterstützt |London |
| **KINX** |Unterstützt |Unterstützt |Seoul |
| **[KPN](http://www.kpn.com/cloudconnect)** | Unterstützt | Unterstützt | Amsterdam | 
| **[Level 3 Communications](http://your.level3.com/LP=882?WT.tsrc=02192014LP882AzureVanityAzureText)** |Unterstützt |Unterstützt |Amsterdam, Chicago, Dallas, Las Vegas+, London, Sao Paolo, Seattle, Silicon Valley, Singapur, Washington DC |
| **LG CNS** |Unterstützt |Unterstützt |Busan |
| **[Megaport](https://www.megaport.com/services/microsoft-expressroute/)** |Unterstützt |Unterstützt |Amsterdam, Chicago, Dallas, Hongkong, Las Vegas, London, Los Angeles, Melbourne, Miami, New York, Quebec City, San Antonio, Seattle, Silicon Valley, Singapur, Sydney, Toronto, Washington DC |
| **MTN** |Unterstützt |Unterstützt |London |
| **[Daten der nächsten Generation](http://www.nextgenerationdata.co.uk/ngd-cloud-gateway/)** |Unterstützt |Unterstützt |Newport(Wales) |
| **NEXTDC** |Unterstützt |Unterstützt |Melbourne, Sydney |
| **[NTT Communications](http://www.ntt.com/en/services/network/virtual-private-network.html)** |Unterstützt |Unterstützt |London, Los Angeles, Osaka, Singapur, Tokio, Washington, D.C. |
| **NTT SmartConnect** |In Kürze verfügbar |In Kürze verfügbar |Osaka |
| **[Orange](http://www.orange-business.com/en/products/business-vpn-galerie)** |Unterstützt |Unterstützt |Amsterdam, Hongkong, London, Paris+, Silicon Valley, Singapur, Sydney, Washington DC |
| **PCCW Global Limited** |Unterstützt |Unterstützt |Hongkong |
| **Sejong Telecom** |Unterstützt |Unterstützt |Seoul |
| **[SIFY](http://telecom.sify.com/azure-expressroute.html)** |Unterstützt |Unterstützt |Chennai |
| **[SingTel](http://info.singtel.com/about-us/news-releases/singtel-provide-secure-private-access-microsoft-azure-public-cloud)** |Unterstützt |Unterstützt |Singapur |
| **[Softbank](http://www.softbank.jp/biz/cloud/cloud_access/direct_access_for_az/)** |Unterstützt |Unterstützt |Osaka, Tokio |
| **[Tata Communications](http://www.tatacommunications.com/lp/izo/azure/azure_index.html)** |Unterstützt |Unterstützt |Amsterdam, Chennai, Hongkong, London, Mumbai, Silicon Valley, Singapur, Washington DC |
| **[TeleCity Group](http://www.telecitygroup.com/investor-centre/news_details.htm?locid=03100500400b00d&xml)** |Unterstützt |Unterstützt |Amsterdam, Dublin, London |
| **[Telefonica](https://www.business-solutions.telefonica.com/es/enterprise/solutions/efficient-infrastructure/managed-voice-data-connectivity/)** |Unterstützt |Unterstützt |Amsterdam, Sao Paulo |
| **[Telehouse - KDDI](http://www.telehouse.net/solutions/cloud-services/cloud-link)** |Unterstützt |Unterstützt |London |
| **Telenor** |Unterstützt |Unterstützt |Amsterdam, London |
| **[Telstra Corporation](http://www.telstra.com.au/business-enterprise/network-services/networks/cloud-direct-connect/)** |Unterstützt |Unterstützt |Melbourne, Sydney |
| **[Telus](http://www.telus.com)** |Unterstützt |Unterstützt |Toronto |
| **[UOLDIVEO](http://www.uoldiveo.com.br/solucoes/cloud.html#rmcl)** |In Kürze verfügbar |In Kürze verfügbar |Sao Paulo+ |
| **[Verizon](http://www.verizonenterprise.com/products/networking/secure-cloud-interconnect/)** |Unterstützt |Unterstützt |Amsterdam, Chicago, Dallas, Hongkong, London, Silicon Valley, Singapur, Sydney, Tokio, Washington DC |
| **[Vodafone](http://www.vodafone.com/business/global-enterprise/global-connectivity/vodafone-ip-vpn-cloud-connect)** |Unterstützt |Nicht unterstützt |London |
| **[Zayo Group](http://www.zayo.com/solutions/industries/cloud-connectivity/microsoft-expressroute)** |Unterstützt |Unterstützt |Amsterdam, Chicago, Dallas+, London+, Los Angeles, New York, Silicon Valley, Toronto, Washington DC |

 **+** steht für "In Kürze"

<a id="national-cloud-environment" class="xliff"></a>

### Nationale Cloudumgebungen

<a id="us-government-cloud" class="xliff"></a>

### US-Government Cloud
| **Service Provider** | **Microsoft Azure** | **Office 365** | **Standorte** |
| --- | --- | --- | --- |
| **[AT&T NetBond](https://www.synaptic.att.com/clouduser/html/productdetail/ATT_NetBond.htm)** |Unterstützt |Unterstützt |Chicago, Washington DC |
| **[Equinix](http://www.equinix.com/partners/microsoft-azure/)** |Unterstützt |Unterstützt |Chicago, Dallas, New York, Seattle, Silicon Valley, Washington DC |
| **[Level 3 Communications](http://your.level3.com/LP=882?WT.tsrc=02192014LP882AzureVanityAzureText)** |Unterstützt |Unterstützt |Chicago, New York+, Silicon Valley, Washington DC |
| **[Megaport](https://www.megaport.com/services/microsoft-expressroute/)** |Unterstützt | Unterstützt | Chicago, Dallas |
| **[Verizon](http://news.verizonenterprise.com/2014/04/secure-cloud-interconnect-solutions-enterprise/)** |Unterstützt |Unterstützt |Chicago, Dallas, New York, Washington DC |

<a id="china" class="xliff"></a>

### China
| **Service Provider** | **Microsoft Azure** | **Office 365** | **Standorte** |
| --- | --- | --- | --- |
| **China Telecom** |Unterstützt |Nicht unterstützt |Peking, Shanghai |

Weitere Informationen finden Sie unter [ExpressRoute in China](http://www.windowsazure.cn/home/features/expressroute/).

<a id="germany" class="xliff"></a>

### Deutschland
| **Service Provider** | **Microsoft Azure** | **Office 365** | **Standorte** |
| --- | --- | --- | --- |
| **[Colt](http://www.colt.net/uk/en/news/colt-announces-dedicated-cloud-access-for-microsoft-azure-services-en.htm)** |Unterstützt |Nicht unterstützt |Berlin+, Frankfurt |
| **[Equinix](http://www.equinix.com/partners/microsoft-azure/)** |Unterstützt |Nicht unterstützt |Frankfurt |
| **[e-shelter](https://www.e-shelter.de/en/microsoft-expressroutetm)** |Unterstützt |Nicht unterstützt |Berlin |
| **Interxion** |Unterstützt |Nicht unterstützt |Frankfurt |
| **[Megaport](https://www.megaport.com/services/microsoft-expressroute/)** |Unterstützt  | Nicht unterstützt | Berlin |

<a id="connectivity-through-exchange-providers" class="xliff"></a>

## Konnektivität über Exchange-Anbieter

Wenn Ihr Konnektivitätsanbieter nicht in den vorherigen Abschnitten enthalten ist, können Sie dennoch eine Verbindung erstellen.

* Fragen Sie Ihren Konnektivitätsanbieter, ob eine Verbindung mit einem der Exchange-Standorte aus der vorherigen Tabelle bereitgestellt werden kann. Sie können auch die folgenden Links überprüfen, um weitere Informationen über die von den Exchange-Anbietern angebotenen Dienste zu erhalten. Viele Konnektivitätsanbieter sind bereits mit Ethernet-Exchanges verbunden.
  * [Cologix](http://www.cologix.com/)
  * [Console](https://www.consoleconnect.com/partners/cloudsaas/)
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

<a id="connectivity-through-additional-service-providers" class="xliff"></a>

## Konnektivität über zusätzliche Dienstanbieter

| **Konnektivitätsanbieter** | **Exchange** | **Standorte** |
| --- | --- | --- |
| **[1CLOUDSTAR](http://www.1cloudstar.com/service/cloudconnect-azure-expressroute/)** |Equinix |Singapur |
| **[Airgate Technologies, Inc.](http://airgate.ca/cloud-express/)** | Equinix, Cologix | Toronto, Montreal |
| **[Alaska Communications](http://www.alaskacommunications.com/For-Your-Business/Direct-Cloud-Service)** |Equinix |Seattle |
| **[Altice Business](https://golightpath.com/transport)** |Equinix |New York, Washington DC |
| **[Arteria Networks Corporation](https://arteria-net.com/business/service/cloud_access/sca/)** |Equinix |Tokio |
| **[Bezeq International Ltd.](https://www.bezeqint.net/english)** | euNetworks | London |
| **[C3ntro Telecom](http://www.c3ntro.com/data/cloud-conectivity/)** | Equinix, Megaport | Dallas |
| **[Cogeco Peer 1](https://www.cogecopeer1.com/en/)**| Equinix | Montreal, Toronto |
| **[Cox Business](https://www.cox.com/business/networking/cloud-connectivity.html)** | Equinix | Dallas, Silicon Valley, Washington D.C. | 
| **[Data Foundry](https://www.datafoundry.com/services/cloud-connect)** | Megaport | Dallas |
| **[Epsilon Telecommunications Limited](http://www.epsilontel.com/data-connectivity/cloud-access/)** | Equinix | London, Singapur, Washington D.C. |
| **[Eurofiber](https://eurofiber.nl/microsoft-azure/)** | Equinix | Amsterdam |
| **[Exponential E](http://www.exponential-e.com/services/connectivity-services/cloud-connect-exchange)** | Equinix | London |
| **[Fastweb S.p.A](http://www.fastweb.it/grandi-aziende/connessione-voce-e-wifi/scheda-prodotto/rete-privata-virtuale/)** | Equinix | Amsterdam |
| **[Gtt Communications Inc](https://www.gtt.net/wp-content/uploads/2017/04/EtherCloud-Data-Sheet.pdf)** |Equinix | Washington DC |
| **[HSO](http://www.hso.co.uk/products/cloud-direct)** |Equinix | London, Slough |
| **LGA Telecom** |Equinix |Singapur|
| **[Lightower](http://www.lightower.com/network-solutions/cloud-connect/#microsoft-azure)** |Equinix | Chicago, New York, Washington DC |
| **[Macroview Telecom](http://www.macroview.com/en/scripts/catitem.php?catid=solution&sectionid=expressroute)** |Equinix |Hongkong |
| **[Macquarie Telecom Group](https://macquariegovernment.com/secure-cloud/secure-cloud-exchange/)** | Megaport | Sydney |
| **[Masergy](https://www.masergy.com/solutions/hybrid-networking/cloud-marketplace/microsoft-azure)** | Equinix | Washington DC |
| **[NexGen Networks](http://www.nexgen-net.com/nexgen-networks-direct-connect-microsoft-azure-expressroute.html)** | Interxion | London |
| **[Nianet](https://nianet.dk/produkter/internet/microsoft-expressroute)** |Telecity | Amsterdam, Frankfurt |  
| **[QSC AG](https://www.qsc.de/de/produkte-loesungen/cloud-services-und-it-outsourcing/pure-enterprise-cloud/multi-cloud-management/azure-expressroute/)** |Interxion | Frankfurt |  
| **Rogers** | Cologix, Equinix | Montreal, Toronto |
| **[Tamares Telecom](http://www.tamarestelecom.com/our-services/#Connectivity)** | Telecity | London | 
| **[ThinkTel](http://www.thinktel.ca/services/agile-ix-data/expressroute/)** | Equinix | Toronto | 
| **[Transtelco](http://www.transtelco.net/tcloud/microsoft)** |Equinix | Dallas, Los Angeles |  
| **[United Information Highway (UIH)](https://www.uih.co.th/en/internet-solution/cloud-direct/uih-cloud-direct-for-microsoft-azure-expressroute)**| Equinix | Singapur |
| **[Webair](https://www.webair.com/microsoft-express-route-partnership/)**| Megaport | New York |
| **[Windstream](http://www.windstreambusiness.com/solutions/cloud-services/cloud-and-managed-hosting-services)**| Equinix | Chicago, Silicon Valley, Washington, D.C. |
| **Zain** |Equinix |London|
| **[Zertia](http://www.zertia.es/index.php/novedades)**| Level 3 | Madrid |
| **[Zirro](https://zirro.com/services/)**| Equinix | Toronto, Montreal |

<a id="connectivity-through-datacenter-providers" class="xliff"></a>

## Konnektivität über Rechenzentrumsanbieter
| **Anbieter** | **Exchange** |
| --- | --- |
| **[Cyrus One](https://cyrusone.com/enterprise-data-center-services/connectivity-and-interconnection/)** | Megaport |
| **[Digital Realty](https://www.digitalrealty.com/services/interconnection/service-exchange/)** | Megaport |
| **[EdgeConnex](http://www.edgeconnex.com/services/edge-data-centers-proximity-matters/)** | Megaport |
| **[RagingWire-Rechenzentren](http://www.ragingwire.com/wholesale/wholesale-data-centers-worldwide-nexcenters)** | Konsole |
| **[T5 Datacenters](http://t5datacenters.com/network-cloud-connect/)** | Konsole |

<a id="connectivity-through-national-research-and-education-networks-nren" class="xliff"></a>

## Konnektivität über National Research and Education Networks (NREN)

| **Anbieter**|
| --- |
| **AARNET**| 
| **DeIC über GÉANT**|
| **GARR über GÉANT**|
| **GÉANT**|
| **HEAnet über GÉANT**|
| **JISC**|
| **RedIRIS über GÉANT**|
| **SINET**|
| **Surfnet über GÉANT**|

* Wenn Ihr Konnektivitätsanbieter nicht hier aufgeführt wird, überprüfen Sie, ob dieser mit einem der oben aufgeführten ExpressRoute Exchange-Partner verbunden ist.

<a id="expressroute-system-integrators" class="xliff"></a>

## ExpressRoute-Systemintegratoren
Je nach Ihrer Netzwerkgröße kann das Aktivieren privater Verbindungen für Ihre Anforderungen problematisch sein. Alle Systemintegratoren, die in der folgenden Tabelle aufgeführt sind, können Ihnen beim Integrieren von ExpressRoute helfen.

| **Systemintegrator** | **Kontinent** |
| --- | --- |
| **[Altogee](http://www.altogee.be/expressroute)** | Europa |
| **[Avanade Inc.](http://www.avanade.com/)** | Asien, Europa, Nordamerika, Südamerika |
| **[Bright Skies GmbH](http://bskies.io/expressroute)** | Europa
| **[Ensyst](http://www.ensyst.com.au)** | Asien
| **[Equinix Professional Services](http://www.equinix.com/services/consulting/)** | Nordamerika |
| **[FlexManage](http://www.flexmanage.com/cloud)** | Nordamerika |
| **[Inframon](http://www.inframon.com/partner/microsoft/)** | Europa |
| **[The IT Consultancy Group](http://itconsult.com.au/microsoft-expressroute)** | Australien |
| **[MOQdigital](http://www.moqdigital.com.au/insights/technical/network-connectivity-options-for-azure)** | Australien |
| **[MSG Services](https://www.msg-services.de/it-services/managed-services/cloud-outsourcing/)** | Europa (Deutschland) |
| **[Nelite](http://nelite.com/)** | Europa |
| **[Neue Signatur](https://www.newsignature.co.uk/)** | Europa |
| **[OneAs1a](http://www.oneas1a.com/express-connect-any-cloud-ecac)** | Asien |
| **[Orange Networks](https://orange-networks.com/blog/88-azureexpressroute)** | Europa |
| **[Perficient](http://www.perficient.com/Partners/Microsoft/Cloud/Azure-ExpressRoute)** | Nordamerika |
| **[Presidio](http://info.presidio.com/microsoft-azure-expressroute)** | Nordamerika |
| **[sol-tec](http://www.sol-tec.com/Technologies)** | Europa |
| **[Vigilant.IT](https://vigilant.it/expressroute)** | Australien |


<a id="next-steps" class="xliff"></a>

## Nächste Schritte
* Weitere Informationen über ExpressRoute finden Sie unter [ExpressRoute – FAQ](expressroute-faqs.md).
* Stellen Sie sicher, dass alle Voraussetzungen erfüllt sind. Informationen finden Sie unter [ExpressRoute-Voraussetzungen](expressroute-prerequisites.md).

<!--Image References-->
[0]: ./media/expressroute-locations/expressroute-locations-map.png "Standortkarte"

