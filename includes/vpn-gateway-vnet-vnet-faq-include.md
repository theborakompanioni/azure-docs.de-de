- Die virtuellen Netzwerke können sich in der gleichen Azure-Region oder in verschiedenen Azure-Regionen (Standorte) befinden.

- Ein Clouddienst oder Endpunkt mit Lastenausgleich darf auch dann NICHT mehrere virtuelle Netzwerke umfassen, wenn diese verbunden sind.

- Für das Verbinden virtueller Azure-Netzwerke sind keine lokalen VPN-Gateways erforderlich. Diese werden nur benötigt, wenn standortübergreifende Konnektivität erforderlich ist.

- VNet-zu-VNet unterstützt das Verbinden virtueller Netzwerke. Nicht unterstützt hingegen werden Verbindungen virtueller Computer oder Clouddienste, die sich NICHT in einem virtuellen Netzwerk befinden.

- VNet-zu-VNet erfordert Azure-VPN Gateways mit routenbasierten VPN-Typen (früher als „dynamisches Routing“ bezeichnet).

- Virtuelle Netzwerkverbindungen können gleichzeitig mit VPNs mit mehreren Standorten und maximal 10 (Standardgateways) bzw. 30 (Hochleistungsgateways) VPN-Tunneln für ein VPN Gateway des virtuellen Netzwerks verwendet werden, das Verbindungen mit anderen virtuellen Netzwerken oder lokalen Standorten herstellt.

- Die Adressräume der virtuellen Netzwerke und der lokalen Netzwerkstandorte dürfen sich nicht überschneiden. Wenn sich Adressräume überlappen, tritt bei der Erstellung von VNet-zu-VNet-Verbindungen ein Fehler auf.

- Redundante Tunnel zwischen einem Paar virtueller Netzwerke werden nicht unterstützt.

- Alle VPN-Tunnel des virtuellen Netzwerks verwenden die verfügbare Bandbreite auf dem Azure-VPN-Gateway und die gleiche SLA für die Verfügbarkeit des VPN-Gateways in Azure gemeinsam.

- VNet-zu-VNet-Datenverkehr wird über das Microsoft-Netzwerk übertragen, nicht über das Internet.

- VNet-zu-VNet-Datenverkehr innerhalb derselben Region ist in beiden Richtungen kostenlos. Ausgehender regionsübergreifender VNet-zu-VNet-Datenverkehr wird zu den Raten für die ausgehende Datenübertragung zwischen VNets basierend auf den Quellregionen berechnet. Angaben zu den Preisen finden Sie in der [Preisübersicht](https://azure.microsoft.com/pricing/details/vpn-gateway/).

<!---HONumber=AcomDC_0720_2016-->