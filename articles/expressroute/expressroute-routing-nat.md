---
title: "NAT für Azure ExpressRoute | Microsoft-Dokumentation"
description: "Diese Seite enthält ausführliche Anforderungen für das Konfigurieren und Verwalten des Routings für ExpressRoute-Verbindungen."
documentationcenter: na
services: expressroute
author: osamazia
manager: ganesr
editor: 
ms.assetid: eaaf0393-d384-4496-9a5c-328e94c262a7
ms.service: expressroute
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/12/2017
ms.author: osamam
ms.translationtype: Human Translation
ms.sourcegitcommit: 9568210d4df6cfcf5b89ba8154a11ad9322fa9cc
ms.openlocfilehash: 5c039a80b24feda61da0793fa64b48cb4783c3f1
ms.contentlocale: de-de
ms.lasthandoff: 05/15/2017


---
# <a name="nat-for-expressroute"></a>NAT für ExpressRoute

Zum Herstellen einer Verbindung mit Microsoft-Clouddiensten per ExpressRoute müssen Sie das Routing einrichten und verwalten. Einige Konnektivitätsanbieter bieten das Einrichten und Verwalten des Routings als verwalteten Dienst an. Fragen Sie bei Ihrem Konnektivitätsanbieter nach, ob dieser Dienst angeboten wird. Wenn dies nicht der Fall ist, müssen Sie die folgenden Anforderungen erfüllen. 

Der Artikel [ExpressRoute-Verbindungen und Routingdomänen](expressroute-circuit-peerings.md) enthält eine Beschreibung der Routingsitzungen, die zum Herstellen der Konnektivität eingerichtet werden müssen.

> [!NOTE]
> Microsoft unterstützt keine Routerredundanzprotokolle (z.B. HSRP, VRRP) für Konfigurationen mit hoher Verfügbarkeit. Wir nutzen ein redundantes Paar mit BGP-Sitzungen pro Peering, um die hohe Verfügbarkeit sicherzustellen.
> 
> 

## <a name="ip-addresses-used-for-peerings"></a>IP-Adressen für Peerings

Sie müssen einige Blöcke mit IP-Adressen reservieren, um das Routing zwischen Ihrem Netzwerk und den Microsoft Enterprise-Edgeroutern (MSEEs) zu konfigurieren. Dieser Abschnitt enthält eine Liste mit den Anforderungen, und es werden die Regeln zur Beschaffung und Verwendung dieser IP-Adressen beschrieben.

### <a name="ip-addresses-used-for-azure-private-peering"></a>IP-Adressen für privates Azure-Peering

Sie können entweder private IP-Adressen oder öffentliche IP-Adressen verwenden, um die Peerings zu konfigurieren. Der Adressbereich zum Konfigurieren der Routen darf sich nicht mit Adressbereichen überlappen, die in Azure zum Erstellen von virtuellen Netzwerken verwendet werden. 

* Sie müssen ein /29-Subnetz oder zwei /30-Subnetze für Routingschnittstellen reservieren.
* Die für das Routing verwendeten Subnetze können entweder private IP-Adressen oder öffentliche IP-Adressen sein.
* Die Subnetze dürfen nicht mit dem Bereich in Konflikt stehen, der vom Kunden für die Nutzung in der Microsoft Cloud reserviert ist.
* Wenn ein /29-Subnetz verwendet wird, wird es in zwei /30-Subnetze unterteilt. 
  * Das erste /30-Subnetz wird für die primäre Verknüpfung verwendet, und das zweite /30-Subnetz wird für die sekundäre Verknüpfung verwendet.
  * Für jedes /30-Subnetz müssen Sie zuerst die IP-Adresse des /30-Subnetzes auf dem Router verwenden. Microsoft verwendet die zweite IP-Adresse des /30-Subnetzes zum Einrichten einer BGP-Sitzung.
  * Sie müssen beide BGP-Sitzungen einrichten, damit unsere [Vereinbarungen zum Servicelevel](https://azure.microsoft.com/support/legal/sla/) gültig sind.  

#### <a name="example-for-private-peering"></a>Beispiel für privates Peering

Wenn Sie zum Einrichten des Peerings „a.b.c.d/29“ verwenden, wird eine Unterteilung in zwei /30-Subnetze vorgenommen. Im folgenden Beispiel sehen wir uns an, wie das a.b.c.d/29-Subnetz verwendet wird. 

a.b.c.d/29 wird in a.b.c.d/30 und a.b.c.d+4/30 unterteilt und über die Bereitstellungs-APIs an Microsoft übergeben. Sie verwenden a.b.c.d+1 als VRF-IP für das primäre PE-Element, und Microsoft nutzt a.b.c.d+2 als VRF-IP für den primären MSEE. Sie verwenden a.b.c.d+5 als VRF-IP für das sekundäre PE-Element, und Microsoft nutzt a.b.c.d+6 als VRF-IP für den sekundären MSEE.

Betrachten wir den Fall, in dem Sie 192.168.100.128/29 zum Einrichten des privaten Peerings auswählen. 192.168.100.128/29 enthält die Adressen von 192.168.100.128 bis 192.168.100.135, für die Folgendes gilt:

* 192.168.100.128/30 wird link1 zugewiesen, wobei der Anbieter 192.168.100.129 und Microsoft 192.168.100.130 verwendet.
* 192.168.100.132/30 wird link2 zugewiesen, wobei der Anbieter 192.168.100.133 und Microsoft 192.168.100.134 verwendet.

### <a name="ip-addresses-used-for-azure-public-and-microsoft-peering"></a>IP-Adressen für öffentliches Azure-Peering und Microsoft-Peering

Sie müssen eigene öffentliche IP-Adressen zum Einrichten der BGP-Sitzungen verwenden. Microsoft muss in der Lage sein, die Eigentümerschaft der IP-Adressen über Routing Internet Registries (RIR) und Internet Routing Registries (IRR) zu überprüfen. 

* Sie müssen ein eindeutiges /29-Subnetz oder zwei /30-Subnetze verwenden, um das BGP-Peering für jedes Peering pro ExpressRoute-Verbindung einzurichten (falls Sie über mehr als ein Peering verfügen). 
* Wenn ein /29-Subnetz verwendet wird, wird es in zwei /30-Subnetze unterteilt. 
  * Das erste /30-Subnetz wird für die primäre Verknüpfung verwendet, und das zweite /30-Subnetz wird für die sekundäre Verknüpfung verwendet.
  * Für jedes /30-Subnetz müssen Sie zuerst die IP-Adresse des /30-Subnetzes auf dem Router verwenden. Microsoft verwendet die zweite IP-Adresse des /30-Subnetzes zum Einrichten einer BGP-Sitzung.
  * Sie müssen beide BGP-Sitzungen einrichten, damit unsere [Vereinbarungen zum Servicelevel](https://azure.microsoft.com/support/legal/sla/) gültig sind.

## <a name="public-ip-address-requirement"></a>Öffentliche IP-Adresse – Anforderungen

### <a name="private-peering"></a>Privates Peering

Sie können für das private Peering öffentliche oder private IPv4-Adressen verwenden. Wir stellen eine End-to-End-Isolation für Ihren Datenverkehr bereit, sodass beim privaten Peering keine Überlappungen von Adressen mit anderen Kunden möglich sind. Diese Adressen werden nicht im Internet angekündigt. 

### <a name="public-peering"></a>Öffentliches Peering

Der öffentliche Azure-Peeringpfad ermöglicht, dass Sie zu allen in Azure gehosteten Diensten über die öffentliche IP-Adresse eine Verbindung herstellen können. Dazu zählen Dienste, die unter [ExpressRoute – Häufig gestellte Fragen](expressroute-faqs.md) aufgeführt sind und die von ISVs auf Microsoft Azure gehostet werden. Die Konnektivität mit Microsoft Azure-Diensten für öffentliches Peering wird immer von Ihrem Netzwerk aus in das Microsoft-Netzwerk initiiert. Sie müssen öffentliche IP-Adressen für den Datenverkehr verwenden, der für das Microsoft-Netzwerk bestimmt ist.

### <a name="microsoft-peering"></a>Microsoft-Peering

Mit dem Microsoft-Peeringpfad können Sie eine Verbindung mit den Clouddiensten von Microsoft herstellen, die vom öffentlichen Azure-Peeringpfad nicht unterstützt werden. Die Liste der Dienste umfasst Office 365-Dienste wie z.B. Exchange Online, SharePoint Online, Skype for Business und Dynamics 365. Microsoft unterstützt die bidirektionale Konnektivität für das Microsoft-Peering. Für Datenverkehr, der für Microsoft-Clouddienste bestimmt ist, müssen vor dem Eintritt in das Microsoft-Netzwerk gültige, öffentliche IPv4-Adressen verwendet werden.

Stellen Sie sicher, dass Ihre IP-Adresse und die AS-Nummer für Sie in einer der unten aufgeführten Registrierungen registriert sind.

* [ARIN](https://www.arin.net/)
* [APNIC](https://www.apnic.net/)
* [AFRINIC](https://www.afrinic.net/)
* [LACNIC](http://www.lacnic.net/)
* [RIPENCC](https://www.ripe.net/)
* [RADB](http://www.radb.net/)
* [ALTDB](http://altdb.net/)

> [!IMPORTANT]
> Öffentliche IP-Adressen, die gegenüber Microsoft per ExpressRoute angekündigt werden, dürfen nicht im Internet angekündigt werden. Ansonsten wird die Verbindung mit anderen Microsoft-Diensten unter Umständen unterbrochen. Öffentliche IP-Adressen, die von Servern in Ihrem Netzwerk verwendet werden, für die mit O365-Endpunkten innerhalb von Microsoft kommuniziert wird, können aber über ExpressRoute angekündigt werden. 
> 
> 

## <a name="dynamic-route-exchange"></a>Dynamischer Routenaustausch

Der Routingaustausch verläuft über das eBGP-Protokoll. EBGP-Sitzungen werden zwischen den MSEEs und Ihren Routern eingerichtet. Die Authentifizierung von BGP-Sitzungen ist nicht unbedingt erforderlich. Bei Bedarf kann ein MD5-Hash konfiguriert werden. Unter [Konfigurieren des Routings](expressroute-howto-routing-classic.md) und [Bereitstellungsworkflows für ExpressRoute-Verbindungen und Verbindungszustände](expressroute-workflows.md) finden Sie Informationen zum Konfigurieren von BGP-Sitzungen.

## <a name="autonomous-system-numbers"></a>Autonome Systemnummern

Microsoft verwendet AS 12076 für das öffentliche und private Azure-Peering und das Microsoft-Peering. Wir haben ASNs von 65515 bis 65520 für die interne Verwendung reserviert. Sowohl AS-Nummern mit 16 als auch mit 32 Bit werden unterstützt.

Es gibt keine Anforderungen in Bezug auf die Symmetrie der Datenübertragung. Die Weiterleitungs- und Rückgabepfade können unterschiedliche Routerpaare durchlaufen. Identische Routen müssen von beiden Seiten über mehrere Verbindungspaare angekündigt werden, die in Ihrem Besitz sind. Routenmetriken müssen nicht identisch sein.

## <a name="route-aggregation-and-prefix-limits"></a>Routenaggregation- und Präfixgrenzen

Wir unterstützen bis zu 4.000 Präfixe, die uns per privatem Azure-Peering angekündigt werden. Dies kann auf bis zu 10.000 Präfixe erhöht werden, wenn das ExpressRoute Premium-Add-On aktiviert wird. Wir akzeptieren bis zu 200 Präfixe pro BGP-Sitzung für das öffentliche Azure-Peering und das Microsoft-Peering. 

Die BGP-Sitzung wird verworfen, wenn die Anzahl der Präfixe den Grenzwert überschreitet. Wir akzeptieren Standardrouten nur über den Link für privates Peering. Der Anbieter muss die Standardroute und private IP-Adressen (RFC 1918) aus den Pfaden für das öffentliche Azure-Peering und das Microsoft-Peering herausfiltern. 

## <a name="transit-routing-and-cross-region-routing"></a>Transitrouting und regionsübergreifendes Routing

ExpressRoute kann nicht als Transitrouter konfiguriert werden. Transitrouting-Dienste erhalten Sie bei Ihrem Konnektivitätsanbieter.

## <a name="advertising-default-routes"></a>Ankündigen von Standardrouten

Standardrouten sind nur für Sitzungen mit privatem Azure-Peering zulässig. In diesem Fall wird der gesamte Datenverkehr aus den zugeordneten virtuellen Netzwerken an Ihr Netzwerk geleitet. Das Ankündigen von Standardrouten für das private Peering führt dazu, dass der Internetpfad aus Azure blockiert wird. Sie müssen den Edge-Bereich Ihres Unternehmens nutzen, um Datenverkehr für in Azure gehostete Dienste aus dem Internet und in das Internet zu leiten. 

 Um die Konnektivität mit anderen Azure-Diensten und Infrastrukturdiensten zu ermöglichen, müssen Sie sicherstellen, dass eines der folgenden Elemente vorhanden ist:

* Öffentliches Azure-Peering ist zum Leiten von Datenverkehr an öffentliche Endpunkte aktiviert.
* Sie verwenden das benutzerdefinierte Routing, um Internetkonnektivität für alle Subnetze zuzulassen, die dies erfordern.

> [!NOTE]
> Das Ankündigen von Standardrouten führt dazu, dass die Aktivierung von Windows- und anderen VM-Lizenzen verloren geht. Führen Sie die Schritte [dieser Anleitung](http://blogs.msdn.com/b/mast/archive/2015/05/20/use-azure-custom-routes-to-enable-kms-activation-with-forced-tunneling.aspx) aus, um dies zu umgehen.
> 
> 

## <a name="support-for-bgp-communities-preview"></a>Unterstützung für BGP-Communitys (Vorschau)

Dieser Abschnitt enthält eine Übersicht darüber, wie BGP-Communitys mit ExpressRoute verwendet werden. Microsoft kündigt Routen in den Pfaden für das öffentliche Peering und Microsoft-Peering an, und die Routen sind dabei mit den entsprechenden Communitywerten versehen. Die Gründe für diese Vorgehensweise und die Details zu den Communitywerten sind weiter unten beschrieben. Microsoft berücksichtigt aber keine Communitywerte, mit denen Routen gekennzeichnet sind, die gegenüber Microsoft angekündigt werden.

Wenn Sie an einem Peeringstandort in einer geopolitischen Region per ExpressRoute eine Verbindung mit Microsoft herstellen, haben Sie Zugriff auf alle Microsoft-Clouddienste innerhalb dieser geopolitischen Grenze. 

Wenn Sie beispielsweise in Amsterdam per ExpressRoute mit Microsoft verbunden sind, haben Sie Zugriff auf alle Microsoft-Clouddienste, die in Nordeuropa und Westeuropa gehostet werden. 

Die Seite [ExpressRoute-Partner und Peeringstandorte](expressroute-locations.md) enthält eine ausführliche Liste mit geopolitischen Regionen, dazugehörigen Azure-Regionen und entsprechenden ExpressRoute-Peeringstandorten.

Sie können mehr als eine ExpressRoute-Verbindung pro geopolitischer Region erwerben. Wenn Sie über mehrere Verbindungen verfügen, ergeben sich für Sie aufgrund der Georedundanz daraus erhebliche Vorteile in Bezug auf hohe Verfügbarkeit. Bei Verwendung mehrerer ExpressRoute-Verbindungen erhalten Sie die gleiche Gruppe von Präfixen, die von Microsoft über die Pfade für das öffentliche Peering und das Microsoft-Peering angekündigt werden. Dies bedeutet, dass Sie dann mehrere Pfade aus dem Netzwerk zu Microsoft nutzen können. Dies kann unter Umständen dazu führen, dass in Ihrem Netzwerk suboptimale Routingentscheidungen getroffen werden. Daraus können sich für verschiedene Dienste suboptimale Konnektivitätsleistungen ergeben. 

Microsoft kennzeichnet Präfixe, die über das öffentliche Peering und Microsoft-Peering angekündigt werden, mit den passenden BGP-Communitywerten, um die Region anzugeben, in der die Präfixe gehostet werden. Sie können die Communitywerte nutzen, um die richtigen Routingentscheidungen zu treffen und [optimales Routing für Kunden](expressroute-optimize-routing.md)zu bieten.

| **Geopolitische Region** | **Microsoft Azure-Region** | **BGP-Communitywert** |
| --- | --- | --- |
| **Nordamerika** | | |
| USA (Ost) |12076:51004 | |
| USA (Ost) 2 |12076:51005 | |
| USA (West) |12076:51006 | |
| USA, Westen 2 |12076:51026 | |
| USA, Westen-Mitte |12076:51027 | |
| USA (Mitte/Norden) |12076:51007 | |
| USA Süd Mitte |12076:51008 | |
| USA (Mitte) |12076:51009 | |
| Kanada, Mitte |12076:51020 | |
| Kanada, Osten |12076:51021 | |
| **Südamerika** | | |
| Brasilien Süd |12076:51014 | |
| **Europa** | | |
| Nordeuropa |12076:51003 | |
| Westeuropa |12076:51002 | |
| **Asien-Pazifik** | | |
| Ostasien |12076:51010 | |
| Südostasien |12076:51011 | |
| **Japan** | | |
| Japan Ost |12076:51012 | |
| Japan (Westen) |12076:51013 | |
| **Australien** | | |
| Australien (Osten) |12076:51015 | |
| Australien (Südosten) |12076:51016 | |
| **Indien** | | |
| Indien, Süden |12076:51019 | |
| Indien, Westen |12076:51018 | |
| Indien, Mitte |12076:51017 | |

Alle Routen, die von Microsoft angekündigt werden, werden mit dem entsprechenden Communitywert gekennzeichnet. 

> [!IMPORTANT]
> Globale Präfixe werden mit einem entsprechenden Community-Wert markiert und nur dann angekündigt, wenn das Premium-Add-On von ExpressRoute aktiviert ist.
> 
> 

Zusätzlich zu den obigen Kennzeichnungen versieht Microsoft Präfixe auch basierend auf dem zugehörigen Dienst mit einer Kennzeichnung. Dies gilt nur für das Microsoft-Peering. Die Tabelle unten enthält die Zuordnung des Diensts zum BGP-Communitywert.

| **Service** | **BGP-Communitywert** |
| --- | --- |
| **Exchange** |12076:5010 |
| **SharePoint** |12076:5020 |
| **Skype for Business** |12076:5030 |
| **Dynamics 365** |12076:5040 |
| **Andere Office 365-Dienste** |12076:5100 |

> [!NOTE]
> Microsoft berücksichtigt keine BGP-Communitywerte, die von Ihnen für die gegenüber Microsoft angekündigten Routen festgelegt werden.
> 
> 

## <a name="next-steps"></a>Nächste Schritte

* Konfigurieren Sie Ihre ExpressRoute-Verbindung.
  
  * [Erstellen einer ExpressRoute-Verbindung für das klassische Bereitstellungsmodell](expressroute-howto-circuit-classic.md) oder [Erstellen und Ändern einer ExpressRoute-Verbindung mit Azure Resource Manager](expressroute-howto-circuit-arm.md)
  * [Konfigurieren des Routings für das klassische Bereitstellungsmodell](expressroute-howto-routing-classic.md) oder [Konfigurieren des Routings für das Resource Manager-Bereitstellungsmodell](expressroute-howto-routing-arm.md)
  * [Verknüpfen eines klassischen VNET mit einer ExpressRoute-Verbindung](expressroute-howto-linkvnet-classic.md) oder [Verknüpfen eines Resource Manager-VNET mit einer ExpressRoute-Verbindung](expressroute-howto-linkvnet-arm.md)


