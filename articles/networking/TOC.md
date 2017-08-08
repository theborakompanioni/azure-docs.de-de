# Übersicht
## [Informationen zu Azure-Netzwerken](networking-overview.md)
## Architektur
### [Virtuelle Datencenter](networking-virtual-datacenter.md)
### [Asymmetrisches Routing mit mehreren Netzwerkpfaden](../expressroute/expressroute-asymmetric-routing.md?toc=%2fazure%2fnetworking%2ftoc.json)
### [Sichere Netzwerkdesigns](../best-practices-network-security.md?toc=%2fazure%2fnetworking%2ftoc.json)
### [Hub-Spoke-Topologie](https://docs.microsoft.com/azure/architecture/reference-architectures/hybrid-networking/hub-spoke)
### [Bewährte Methoden für die Netzwerksicherheit](../security/azure-security-network-security-best-practices.md?toc=%2fazure%2fnetworking%2ftoc.json)
### [Hoch verfügbare virtuelle Netzwerkgeräte](https://docs.microsoft.com/azure/architecture/reference-architectures/dmz/nva-ha )
### [Kombinieren von Lastenausgleichsmethoden](../traffic-manager/traffic-manager-load-balancing-azure.md?toc=%2fazure%2fnetworking%2ftoc.json)
## Planen und Entwerfen
### [Virtuelle Netzwerke](../virtual-network/virtual-network-vnet-plan-design-arm.md?toc=%2fazure%2fnetworking%2ftoc.json)
### [Filtern von Datenverkehr](../virtual-network/virtual-networks-nsg.md?toc=%2fazure%2fnetworking%2ftoc.json)
### [Standortübergreifende Konnektivität – VPN](../vpn-gateway/vpn-gateway-plan-design.md?toc=%2fazure%2fnetworking%2ftoc.json)
### [Standortübergreifende Konnektivität – dediziert privat](../expressroute/expressroute-workflows.md?toc=%2fazure%2fnetworking%2ftoc.json)

##  Konzepte
### [Virtuelle Netzwerke](../virtual-network/virtual-networks-overview.md?toc=%2fazure%2fnetworking%2ftoc.json)
### [Netzwerklastenausgleich](../load-balancer/load-balancer-overview.md?toc=%2fazure%2fnetworking%2ftoc.json)
### [Anwendungslastenausgleich](../application-gateway/application-gateway-introduction.md?toc=%2fazure%2fnetworking%2ftoc.json)
### [DNS](../dns/dns-overview.md?toc=%2fazure%2fnetworking%2ftoc.json)
### [DNS-basierte Datenverkehrsverteilung](../traffic-manager/traffic-manager-overview.md?toc=%2fazure%2fnetworking%2ftoc.json)
### [Lokal verbinden – VPN](../vpn-gateway/vpn-gateway-about-vpngateways.md?toc=%2fazure%2fnetworking%2ftoc.json)
### [Lokal verbinden – dediziert](../expressroute/expressroute-introduction.md?toc=%2fazure%2fnetworking%2ftoc.json)

# Erste Schritte
## [Erstellen Ihres ersten virtuellen Netzwerks](../virtual-network/virtual-network-get-started-vnet-subnet.md?toc=%2fazure%2fnetworking%2ftoc.json)

# Anleitung
## Internetkonnektivität
### [Netzwerklastenausgleich – öffentliche Server](../load-balancer/load-balancer-get-started-internet-portal.md?toc=%2fazure%2fnetworking%2ftoc.json)
### [Anwendungslastenausgleich – öffentliche Server](../application-gateway/application-gateway-create-gateway-portal.md?toc=%2fazure%2fnetworking%2ftoc.json)
### [Schützen von Webanwendungen](../application-gateway/application-gateway-web-application-firewall-portal.md?toc=%2fazure%2fnetworking%2ftoc.json)
### [Verteilen von Datenverkehrs auf Standorte](../traffic-manager/traffic-manager-configure-geographic-routing-method.md?toc=%2fazure%2fnetworking%2ftoc.json)
## Interne Konnektivität
### [Netzwerklastenausgleich – private Server](../load-balancer/load-balancer-get-started-ilb-arm-portal.md?toc=%2fazure%2fnetworking%2ftoc.json)
### [Anwendungslastenausgleich – private Server](../application-gateway/application-gateway-ilb-arm.md?toc=%2fazure%2fnetworking%2ftoc.json)
### [Herstellen von Verbindungen zwischen virtuellen Netzwerken (gleicher Standort)](../virtual-network/virtual-networks-create-vnetpeering-arm-portal.md?toc=%2fazure%2fnetworking%2ftoc.json)
### [Herstellen von Verbindungen zwischen virtuellen Netzwerken (verschiedene Standorte)](../vpn-gateway/vpn-gateway-howto-vnet-vnet-resource-manager-portal.md?toc=%2fazure%2fnetworking%2ftoc.json)
## Standortübergreifende Konnektivität
### [Erstellen einer S2S-VPN-Verbindung (IPsec/IKE)](../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal.md?toc=%2fazure%2fnetworking%2ftoc.json)
### [Erstellen einer P2S-VPN-Verbindung (SSTP mit Zertifikaten)](../vpn-gateway/vpn-gateway-howto-point-to-site-resource-manager-portal.md?toc=%2fazure%2fnetworking%2ftoc.json)
### [Erstellen einer dedizierten privaten Verbindung (ExpressRoute)](../expressroute/expressroute-howto-circuit-portal-resource-manager.md?toc=%2fazure%2fnetworking%2ftoc.json)

## Verwaltung
### [Netzwerktopologie](../network-watcher/network-watcher-topology-powershell.md?toc=%2fazure%2fnetworking%2ftoc.json)
### [Verwalten der Paketerfassung](../network-watcher/network-watcher-packet-capture-manage-portal.md?toc=%2fazure%2fnetworking%2ftoc.json)
### [Bestimmen von Routing – nächster Abschnitt](../network-watcher/network-watcher-check-next-hop-portal.md?toc=%2fazure%2fnetworking%2ftoc.json)
### [IP-Datenflussüberprüfung für einen virtuellen Computer](../network-watcher/network-watcher-check-ip-flow-verify-portal.md?toc=%2fazure%2fnetworking%2ftoc.json)

## Beispielskripts
### [Azure-CLI](cli-samples.md)
### [PowerShell](powershell-samples.md)

## Lernprogramme
### [Lastenausgleich für virtuelle Computer](../virtual-machines/linux/tutorial-load-balance-nodejs.md?toc=%2fazure%2fnetworking%2ftoc.json)
### [Verbinden eines Computers mit einem virtuellen Netzwerk](../vpn-gateway/vpn-gateway-howto-point-to-site-resource-manager-portal.md?toc=%2fazure%2fnetworking%2ftoc.json)

# Referenz
## [Azure-Befehlszeilenschnittstelle](https://docs.microsoft.com/cli/azure/network)
## [PowerShell](https://docs.microsoft.com/powershell/module/azurerm.network/?view=azurermps-3.8.0)
## [.Net](https://docs.microsoft.com/dotnet/api/microsoft.azure.management.network?view=azuremgmtnetwork-9.1.0-preview)
## [Node.js](https://azure.microsoft.com/develop/nodejs/#azure-sdk)
## [REST](https://msdn.microsoft.com/library/mt163658.aspx)

# Ressourcen
## [Verfassen von Vorlagen](/azure/azure-resource-manager/resource-group-authoring-templates?toc=%2fazure%2fnetworking%2ftoc.json)
## [Azure-Roadmap](https://azure.microsoft.com/roadmap/?category=networking)
## [Communityvorlagen](https://azure.microsoft.com/resources/templates/)
## [Netzwerkblog](http://azure.microsoft.com/blog/topics/networking)
## [Preise](https://azure.microsoft.com/pricing)
## [Preisrechner](https://azure.microsoft.com/pricing/calculator/)
## [Regionale Verfügbarkeit](https://azure.microsoft.com/regions/services/)
## [Stack Overflow](http://stackoverflow.com/questions/tagged/azure-virtual-network)
## [Videos](https://azure.microsoft.com/resources/videos/index/?services=virtual-network)

