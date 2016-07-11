### Wird BGP von allen Azure-VPN-Gateway-SKUs unterstützt?

Nein. BGP wird von Azure-VPN-Gateways vom Typ **Standard** und **HighPerformance** unterstützt. Die SKU **Basic** wird NICHT unterstützt.

### Kann ich BGP mit richtlinienbasierten Azure-VPN-Gateways verwenden?

Nein. BGP wird nur für routenbasierte VPN-Gateways unterstützt.

### Kann ich private ASNs (Autonome Systemnummern) verwenden?

Ja. Sie können eigene öffentliche ASNs oder private ASNs verwenden – sowohl für Ihre lokalen Netzwerke als auch für Ihre virtuellen Azure-Netzwerke.

#### Werden ASNs von Azure reserviert?

Ja. Die folgenden ASNs werden von Azure sowohl für interne als auch für externe Peerings reserviert:

- Öffentliche ASNs: 8075, 8076, 12076
- Private ASNs: 65515, 65517, 65518, 65519, 65520

Sie können diese ASNs beim Herstellen einer Verbindung mit Azure VPN Gateways nicht für Ihre lokalen VPN-Geräte angeben.

### Kann ich die gleiche ASN sowohl für lokale VPN-Netzwerke als auch für Azure-VNETs verwenden?

Nein. Lokalen Netzwerken und Azure-VNETs muss jeweils eine unterschiedliche ASN zugewiesen werden, wenn diese per BGP miteinander verbunden werden. Azure-VPN-Gateways ist standardmäßig die ASN 65515 zugewiesen. Dabei spielt es keine Rolle, ob BGP für Ihre standortübergreifende Konnektivität aktiviert ist. Diesen Standardwert können Sie überschreiben, indem Sie beim Erstellen des VPN-Gateways eine andere ASN zuweisen oder die ASN nach der Gatewayerstellung ändern. Ihre lokalen ASNs müssen den entsprechenden lokalen Azure-Netzwerkgateways zugewiesen werden.

### Welche Adresspräfixe kündigen Azure-VPN-Gateways mir gegenüber an?

Das Azure-VPN-Gateway kündigt Ihren lokalen BGP-Geräten gegenüber folgende Routen an:

- Ihre VNET-Adresspräfixe
- Adresspräfixe für die einzelnen lokalen Netzwerkgateways, die mit dem Azure-VPN-Gateway verbunden sind
- Routen, die in anderen, mit dem Azure VPN Gateway verbundenen BGP-Peeringsitzungen ermittelt wurden (**mit Ausnahme der Standardrouten oder Routen, die sich mit VNET-Präfixen überschneiden**)

#### Kann ich die Standardroute (0.0.0.0/0) für Azure VPN Gateways ankündigen?

Derzeit leider nicht.

#### Kann ich die gleichen Präfixe wie meine Virtual Network-Präfixe ankündigen?

Nein. Das Ankündigen der gleichen Präfixe wie Ihre Virtual Network-Adresspräfixe wird von der Azure-Plattform blockiert oder gefiltert.

### Kann ich BGP mit meinen VNET-zu-VNET-Verbindungen verwenden?

Ja. BGP kann sowohl für standortübergreifende Verbindungen als auch für VNET-zu-VNET-Verbindungen verwendet werden.

### Kann ich BGP-Verbindungen mit BGP-fremden Verbindungen für meine Azure-VPN-Gateways kombinieren?

Ja. Für ein Azure-VPN-Gateway kann eine Kombination aus BGP-Verbindungen und BGP-fremden Verbindungen verwendet werden.

### Wird BGP-Transitrouting vom Azure-VPN-Gateway unterstützt?

Ja. BGP-Transitrouting wird unterstützt. Einzige Einschränkung: Azure-VPN-Gateways kündigen gegenüber anderen BGP-Peers **KEINE** Standardrouten an. Wenn Sie Transitrouting über mehrere Azure-VPN-Gateways hinweg nutzen möchten, müssen Sie BGP für alle VNET-zu-VNET-Zwischenverbindungen aktivieren.

### Kann ich zwischen Azure-VPN-Gateway und meinem lokalen Netzwerk mehrere Tunnel verwenden?

Ja. Zwischen einem Azure-VPN-Gateway und Ihrem lokalen Netzwerk können mehrere S2S-VPN-Tunnel eingerichtet werden. Beachten Sie, dass diese Tunnel alle auf die Gesamtanzahl von Tunneln für Ihre Azure-VPN-Gateways angerechnet werden. Wenn also etwa zwischen Ihrem Azure-VPN-Gateway und einem Ihrer lokalen Netzwerke zwei redundante Tunnel bestehen, werden dadurch zwei Tunnel des Gesamtkontingents für Ihr Azure-VPN-Gateway (10 für „Standard“, 30 für „HighPerformance“) belegt.

### Kann ich zwischen zwei Azure-VNETs mit BGP mehrere Tunnel verwenden?

Nein. Redundante Tunnel zwischen einem Paar virtueller Netzwerke werden nicht unterstützt.

### Kann ich BGP für S2S-VPN-Verbindungen in einer Konfiguration mit ExpressRoute und S2S-VPN verwenden?

Derzeit leider nicht.

### Welche Adresse verwendet das Azure-VPN-Gateway als BGP-Peer-IP-Adresse?

Das Azure-VPN-Gateway ordnet eine einzelne IP-Adresse aus dem für das virtuelle Netzwerk definierten GatewaySubnet-Bereich zu. Dabei handelt es sich standardmäßig um die vorletzte Adresse des Bereichs. Wenn also beispielsweise der GatewaySubnet-Bereich 10.12.255.0/27 von 10.12.255.0 bis 10.12.255.31 reicht, wird 10.12.255.30 als BGP-Peer-IP-Adresse für das Azure-VPN-Gateway verwendet. Diese Information können Sie der Liste mit den Azure-VPN-Gateway-Informationen entnehmen.

### Welche Anforderungen müssen die BGP-Peer-IP-Adressen auf meinem VPN-Gerät erfüllen?

Ihre lokale BGP-Peeradresse darf **NICHT** der öffentlichen IP-Adresse Ihres VPN-Geräts entsprechen. Verwenden Sie als BGP-Peer-IP-Adresse eine andere IP-Adresse für das VPN-Gerät. Dabei kann es sich um eine Adresse handeln, die der Loopback-Schnittstelle des Geräts zugewiesen ist. Geben Sie diese Adresse im entsprechenden lokalen Netzwerkgateway an, das den Standort darstellt.

### Was muss ich bei Verwendung von BGP als Adresspräfixe für das lokale Netzwerkgateway angeben?

Das lokale Azure-Netzwerkgateway gibt die anfänglichen Adresspräfixe für das lokale Netzwerk an. Mit BGP müssen Sie das Hostpräfix (/32-Präfix) Ihrer BGP-Peer-IP-Adresse als Adressraum für das lokale Netzwerk zuordnen. Wenn Ihre BGP-Peer-IP-Adresse 10.52.255.254 lautet, müssen Sie 10.52.255.254/32 als LocalNetworkAddressSpace für das lokale Netzwerkgateway angeben, das dieses lokale Netzwerkgateway darstellt. Dadurch wird sichergestellt, dass das Azure-VPN-Gateway die BGP-Sitzung über den S2S-VPN-Tunnel herstellt.

### Was muss ich meinem lokalen VPN-Gerät für die BGP-Peeringsitzung hinzufügen?

Sie müssen eine Hostroute der Azure-BGP-Peer-IP-Adresse auf Ihrem VPN-Gerät hinzufügen, die auf den IPsec-S2S-VPN-Tunnel verweist. Lautet die Azure-VPN-Peer-IP-Adresse also etwa 10.12.255.30, müssen Sie eine Hostroute für 10.12.255.30 mit einer Nexthop-Schnittstelle der entsprechenden IPSec-Tunnelschnittstelle auf Ihrem VPN-Gerät hinzufügen.

<!---HONumber=AcomDC_0629_2016-->