* Die virtuellen Netzwerke können sich in der gleichen Azure-Region oder in verschiedenen Azure-Regionen (Standorte) befinden.
* Ein Clouddienst oder Endpunkt mit Lastenausgleich darf auch dann NICHT mehrere virtuelle Netzwerke umfassen, wenn diese verbunden sind.
* Für das Verbinden virtueller Azure-Netzwerke sind keine lokalen VPN-Gateways erforderlich. Diese werden nur benötigt, wenn standortübergreifende Konnektivität erforderlich ist.
* VNet-zu-VNet unterstützt das Verbinden virtueller Netzwerke. Nicht unterstützt hingegen werden Verbindungen virtueller Computer oder Clouddienste, die sich NICHT in einem virtuellen Netzwerk befinden.
* VNet-zu-VNet erfordert Azure-VPN Gateways mit routenbasierten VPN-Typen (früher als „dynamisches Routing“ bezeichnet). 
* Virtuelle Netzwerkverbindungen können simultan mit VPNs mit mehreren Standorten verwendet werden. Maximal 10 VPN-Tunnel (für Standardgateways) bzw. 30 VPN-Tunnel (für Hochleistungsgateways) können für ein VPN-Gateway des virtuellen Netzwerks verwendet werden, das Verbindungen mit anderen virtuellen Netzwerken oder lokalen Standorten herstellt.
* Die Adressräume der virtuellen Netzwerke und der lokalen Netzwerkstandorte dürfen sich nicht überschneiden. Wenn sich Adressräume überlappen, tritt bei der Erstellung von VNet-zu-VNet-Verbindungen ein Fehler auf.
* Redundante Tunnel zwischen einem Paar virtueller Netzwerke werden unterstützt, wenn ein virtuelles Netzwerkgateway als „Aktiv-Aktiv“ konfiguriert ist.
* Alle VPN-Tunnel des virtuellen Netzwerks verwenden die verfügbare Bandbreite auf dem Azure-VPN-Gateway und die gleiche SLA für die Verfügbarkeit des VPN-Gateways in Azure gemeinsam.
* VNet-zu-VNet-Datenverkehr wird über das Microsoft-Netzwerk übertragen, nicht über das Internet.
* VNet-zu-VNet-Datenverkehr innerhalb einer Region ist in beiden Richtungen kostenlos. Für regionsübergreifenden VNet-zu-VNet-Datenverkehr in ausgehender Richtung werden Gebühren für ausgehende Datenübertragungen zwischen VNets basierend auf den Quellregionen berechnet. Angaben zu den Preisen finden Sie in der [Preisübersicht](https://azure.microsoft.com/pricing/details/vpn-gateway/).



<!--HONumber=Jan17_HO3-->


