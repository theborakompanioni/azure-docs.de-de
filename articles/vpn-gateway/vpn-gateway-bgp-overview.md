<properties
   pageTitle="Übersicht über BGP mit Azure-VPN-Gateways | Microsoft Azure"
   description="Dieser Artikel enthält eine Übersicht über die Verwendung von BGP mit Azure-VPN-Gateways."
   services="vpn-gateway"
   documentationCenter="na"
   authors="yushwang"
   manager="rossort"
   editor=""
   tags=""/>

<tags
   ms.service="vpn-gateway"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="04/26/2016"
   ms.author="yushwang"/>

# Übersicht über BGP mit Azure-VPN-Gateways

Dieser Artikel enthält eine Übersicht über die Unterstützung des Border Gateway-Protokolls (BGP) in Azure-VPN-Gateways.

## Informationen zu BGP

BGP ist das standardmäßige Routingprotokoll, mit dem im Internet üblicherweise Routing- und Erreichbarkeitsinformationen zwischen mehren Netzwerken ausgetauscht werden. Im Kontext virtueller Azure-Netzwerke können Azure-VPN-Gateways und Ihre lokalen VPN-Geräte (so genannte BGP-Peers oder Nachbarn) über BGP Routen austauschen, die beide Gateways über die Verfügbarkeit und Erreichbarkeit der Präfixe informieren, die die beteiligten Gateways oder Router durchlaufen. BGP ermöglicht auch Transitrouting zwischen mehreren Netzwerken. Hierzu werden von einem BGP-Gateway ermittelte Routen eines BGP-Peers an alle anderen BGP-Peers weitergegeben.
 
### Argumente für die Verwendung von BGP

BGP ist ein optionales Feature für routenbasierte Azure-VPN-Gateways. Vergewissern Sie sich vor dem Aktivieren des Features, dass BGP von Ihren lokalen VPN-Geräten unterstützt wird. Azure-VPN-Gateways und Ihre lokalen VPN-Geräte können auch weiterhin ohne BGP verwendet werden. Sie haben somit die Wahl zwischen statischen Routen (ohne BGP) *oder* dynamischem Routing mit BGP zwischen Ihren Netzwerken und Azure.

BGP bietet einige Vorteile und neue Möglichkeiten:

#### Unterstützung automatischer und flexibler Präfixaktualisierungen

Mit BGP müssen Sie lediglich ein Mindestpräfix für einen bestimmten BGP-Peer über den IPsec-S2S-VPN-Tunnel deklarieren. Das Präfix muss mindestens die Größe eines Hostpräfix (/32) der BGP-Peer-IP-Adresse Ihres lokalen VPN-Geräts besitzen. Sie können steuern, welche lokalen Netzwerkpräfixe sie gegenüber Azure ankündigen möchten, um Ihrem virtuellen Azure-Netzwerk den Zugriff darauf zu ermöglichen.
	
Sie können auch größere Präfixe ankündigen, die ggf. einige Ihrer VNET-Adresspräfixe umfassen – beispielsweise die Standardroute (0.0.0.0/0) oder einen großen privaten IP-Adressraum (etwa 10.0.0.0/8). Beachten Sie jedoch, dass die Präfixe keinem Ihrer VNET-Präfixe entsprechen dürfen. Mit Ihren VNET-Präfixen identische Routen werden abgelehnt.

#### Unterstützung mehrerer Tunnel zwischen einem VNET und einem lokalen Standort mit automatischem Failover auf der Grundlage von BGP

Sie können mehrere Verbindungen zwischen Ihrem Azure-VNET und Ihren lokalen VPN-Geräten am gleichen Standort herstellen. Dies ermöglicht die Einrichtung mehrerer Tunnel (Pfade) zwischen den beiden Netzwerken in einer Aktiv/Aktiv-Konfiguration. Wird die Verbindung eines der Tunnel getrennt, werden die entsprechenden Routen per BGP zurückgezogen, und der Datenverkehr wird automatisch auf die verbleibenden Tunnel verlagert.
	
Das folgende Diagramm zeigt ein einfaches Beispiel für dieses hochverfügbare Setup:
	
![Mehrere aktive Pfade](./media/vpn-gateway-bgp-overview/multiple-active-tunnels.png)

#### Unterstützung von Transitrouting zwischen Ihren lokalen Netzwerken und mehreren Azure-VNETs

Mit BGP können mehrere Gateways Präfixe aus unterschiedlichen Netzwerken ermitteln und weitergeben (sowohl bei direkter als auch bei indirekter Verbindung). Dies ermöglicht die Verwendung von Transitrouting mit Azure-VPN-Gateways zwischen Ihren lokalen Standorten oder zwischen mehreren virtuellen Azure-Netzwerken.
	
Das folgende Diagramm veranschaulicht ein Beispiel für eine Mehrfachhop-Topologie mit mehreren Pfaden und Transitrouting von Datenverkehr zwischen den beiden lokalen Netzwerken über Azure-VPN-Gateways innerhalb der Microsoft-Netzwerke:

![Mehrfachhop-Übertragung](./media/vpn-gateway-bgp-overview/full-mesh-transit.png)

## Häufig gestellte Fragen zu BGP

#### Wird BGP von allen Azure-VPN-Gateway-SKUs unterstützt?

Nein. BGP wird von Azure-VPN-Gateways vom Typ **Standard** und **HighPerformance** unterstützt. Die SKU **Basic** wird NICHT unterstützt.

#### Kann ich BGP mit richtlinienbasierten Azure-VPN-Gateways verwenden?

Nein. BGP wird nur für routenbasierte VPN-Gateways unterstützt.

#### Kann ich private ASNs (Autonome Systemnummern) verwenden?

Ja. Sie können eigene öffentliche ASNs oder private ASNs verwenden – sowohl für Ihre lokalen Netzwerke als auch für Ihre virtuellen Azure-Netzwerke.

#### Kann ich die gleiche ASN sowohl für lokale VPN-Netzwerke als auch für Azure-VNETs verwenden?

Nein. Lokalen Netzwerken und Azure-VNETs muss jeweils eine unterschiedliche ASN zugewiesen werden, wenn diese per BGP miteinander verbunden werden. Azure-VPN-Gateways ist standardmäßig die ASN 65515 zugewiesen. Dabei spielt es keine Rolle, ob BGP für Ihre standortübergreifende Konnektivität aktiviert ist. Diesen Standardwert können Sie überschreiben, indem Sie beim Erstellen des VPN-Gateways eine andere ASN zuweisen oder die ASN nach der Gatewayerstellung ändern. Ihre lokalen ASNs müssen den entsprechenden lokalen Azure-Netzwerkgateways zugewiesen werden.



#### Welche Adresspräfixe kündigen Azure-VPN-Gateways mir gegenüber an?

Das Azure-VPN-Gateway kündigt Ihren lokalen BGP-Geräten gegenüber folgende Routen an:

- Ihre VNET-Adresspräfixe
- Adresspräfixe für die einzelnen lokalen Netzwerkgateways, die mit dem Azure-VPN-Gateway verbunden sind
- Routen, die in anderen, mit dem Azure-VPN-Gateway verbundenen BGP-Peeringsitzungen ermittelt wurden (**mit Ausnahme der Standardrouten, die sich mit VNET-Präfixen überschneiden**)

#### Kann ich BGP mit meinen VNET-zu-VNET-Verbindungen verwenden?

Ja. BGP kann sowohl für standortübergreifende Verbindungen als auch für VNET-zu-VNET-Verbindungen verwendet werden.

#### Kann ich BGP-Verbindungen mit BGP-fremden Verbindungen für meine Azure-VPN-Gateways kombinieren?

Ja. Für ein Azure-VPN-Gateway kann eine Kombination aus BGP-Verbindungen und BGP-fremden Verbindungen verwendet werden.

#### Wird BGP-Transitrouting vom Azure-VPN-Gateway unterstützt?

Ja. BGP-Transitrouting wird unterstützt. Einzige Einschränkung: Azure-VPN-Gateways kündigen gegenüber anderen BGP-Peers **KEINE** Standardrouten an. Wenn Sie Transitrouting über mehrere Azure-VPN-Gateways hinweg nutzen möchten, müssen Sie BGP für alle VNET-zu-VNET-Zwischenverbindungen aktivieren.

#### Kann ich zwischen Azure-VPN-Gateway und meinem lokalen Netzwerk mehrere Tunnel verwenden?

Ja. Zwischen einem Azure-VPN-Gateway und Ihrem lokalen Netzwerk können mehrere S2S-VPN-Tunnel eingerichtet werden. Beachten Sie, dass diese Tunnel alle auf die Gesamtanzahl von Tunneln für Ihre Azure-VPN-Gateways angerechnet werden. Wenn also etwa zwischen Ihrem Azure-VPN-Gateway und einem Ihrer lokalen Netzwerke zwei redundante Tunnel bestehen, werden dadurch zwei Tunnel des Gesamtkontingents für Ihr Azure-VPN-Gateway (10 für „Standard“, 30 für „HighPerformance“) belegt.

#### Kann ich zwischen zwei Azure-VNETs mit BGP mehrere Tunnel verwenden?

Nein. Redundante Tunnel zwischen einem Paar virtueller Netzwerke werden nicht unterstützt.

#### Welche Adresse verwendet das Azure-VPN-Gateway als BGP-Peer-IP-Adresse?

Das Azure-VPN-Gateway ordnet eine einzelne IP-Adresse aus dem für das virtuelle Netzwerk definierten GatewaySubnet-Bereich zu. Dabei handelt es sich standardmäßig um die vorletzte Adresse des Bereichs. Wenn also beispielsweise der GatewaySubnet-Bereich 10.12.255.0.0/27 von 10.42.255.0.0 bis 10.42.255.31 reicht, wird 10.12.255.30 als BGP-Peer-IP-Adresse für das Azure-VPN-Gateway verwendet. Diese Information können Sie der Liste mit den Azure-VPN-Gateway-Informationen entnehmen.

#### Welche Anforderungen müssen die BGP-Peer-IP-Adressen auf meinem VPN-Gerät erfüllen?

Ihre lokale BGP-Peeradresse **DARF NICHT** der öffentlichen IP-Adresse Ihres VPN-Geräts entsprechen. Verwenden Sie als BGP-Peer-IP-Adresse eine andere IP-Adresse für das VPN-Gerät. Dabei kann es sich um eine Adresse handeln, die der Loopback-Schnittstelle des Geräts zugewiesen ist. Geben Sie diese Adresse im entsprechenden lokalen Netzwerkgateway an, das den Standort darstellt.

#### Was muss ich bei Verwendung von BGP als Adresspräfixe für das lokale Netzwerkgateway angeben?

Das lokale Azure-Netzwerkgateway gibt die anfänglichen Adresspräfixe für das lokale Netzwerk an. Mit BGP müssen Sie das Hostpräfix (/32-Präfix) Ihrer BGP-Peer-IP-Adresse als Adressraum für das lokale Netzwerk zuordnen. Wenn Ihre BGP-Peer-IP-Adresse 10.52.255.254 lautet, müssen Sie 10.52.255.254/32 als LocalNetworkAddressSpace für das lokale Netzwerkgateway angeben, das dieses lokale Netzwerkgateway darstellt. Dadurch wird sichergestellt, dass das Azure-VPN-Gateway die BGP-Sitzung über den S2S-VPN-Tunnel herstellt.

#### Was muss ich meinem lokalen VPN-Gerät für die BGP-Peeringsitzung hinzufügen?

Sie müssen eine Hostroute der Azure-BGP-Peer-IP-Adresse auf Ihrem VPN-Gerät hinzufügen, die auf den IPsec-S2S-VPN-Tunnel verweist. Lautet die Azure-VPN-Peer-IP-Adresse also etwa 10.12.255.30, müssen Sie eine Hostroute für 10.12.255.30 mit einer Nexthop-Schnittstelle der entsprechenden IPSec-Tunnelschnittstelle auf Ihrem VPN-Gerät hinzufügen.

## Nächste Schritte

Schritte zum Konfigurieren von BGP für standortübergreifende Verbindungen und VNET-zu-VNET-Verbindungen finden Sie unter [Erste Schritte mit BGP für Azure-VPN-Gateways](./vpn-gateway-bgp-resource-manager-ps.md).

<!---HONumber=AcomDC_0427_2016-->