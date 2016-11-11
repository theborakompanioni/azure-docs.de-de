# Übersicht
## [Virtuelle Netzwerke](virtual-networks-overview.md)
## [Netzwerksicherheitsgruppen](virtual-networks-nsg.md)
## [Benutzerdefinierte Routen und IP-Weiterleitung](virtual-networks-udr-overview.md)
## IP-Adressierung
### [Ressourcen-Manager](virtual-network-ip-addresses-overview-arm.md)
### [Klassisch](virtual-network-ip-addresses-overview-classic.md)
## [Peering in virtuellen Netzwerken](virtual-network-peering-overview.md)
## Virtuelle Computer
### [Netzwerkschnittstellen](virtual-network-network-interface-overview.md)
### [Namensauflösung](virtual-networks-name-resolution-for-vms-and-role-instances.md)
## [Geschäftskontinuität](virtual-network-disaster-recovery-guidance.md)
## [Preise](https://azure.microsoft.com/pricing/details/virtual-network)

# Erste Schritte
## [Erstellen eines virtuellen Netzwerks](virtual-networks-create-vnet-arm-pportal.md)
## [Bereitstellen einer VM in einem virtuellen Netzwerk](../virtual-machines/virtual-machines-windows-hero-tutorial.md)

# Anleitung
## Planen und Entwerfen
### [Virtuelle Netzwerke](virtual-network-vnet-plan-design-arm.md)
### [Netzwerksicherheitsgruppen](virtual-networks-nsg.md)

## Bereitstellen
### Virtuelle Netzwerke
#### [Portal](virtual-networks-create-vnet-arm-pportal.md)
#### [PowerShell](virtual-networks-create-vnet-arm-ps.md)
#### [BEFEHLSZEILENSCHNITTSTELLE (CLI)](virtual-networks-create-vnet-arm-cli.md)
#### [Vorlage](virtual-networks-create-vnet-arm-template-click.md)
#### [Portal (klassisch)](virtual-networks-create-vnet-classic-pportal.md)
#### [PowerShell (klassisch)](virtual-networks-create-vnet-classic-netcfg-ps.md)
#### [CLI (klassisch)](virtual-networks-create-vnet-classic-cli.md)

### Netzwerksicherheitsgruppen
#### [Portal](virtual-networks-create-nsg-arm-portal.md)
#### [PowerShell](virtual-networks-create-nsg-arm-ps.md)
#### [BEFEHLSZEILENSCHNITTSTELLE (CLI)](virtual-networks-create-nsg-arm-cli.md)
#### [Vorlage](virtual-networks-create-nsg-arm-template.md)
#### [PowerShell (klassisch)](virtual-networks-create-nsg-classic-ps.md)
#### [CLI (klassisch)](virtual-networks-create-nsg-classic-cli.md)

### Benutzerdefinierte Routen
#### [PowerShell](virtual-network-create-udr-arm-ps.md)
#### [BEFEHLSZEILENSCHNITTSTELLE (CLI)](virtual-network-create-udr-arm-cli.md)
#### [Vorlage](virtual-network-create-udr-arm-template.md)
#### [PowerShell (klassisch)](virtual-network-create-udr-classic-ps.md)
#### [CLI (klassisch)](virtual-network-create-udr-classic-cli.md)

### Peering in virtuellen Netzwerken
#### [Portal](virtual-networks-create-vnetpeering-arm-portal.md)
#### [PowerShell](virtual-networks-create-vnetpeering-arm-ps.md)
#### [Vorlage](virtual-networks-create-vnetpeering-arm-template-click.md)

### Virtuelle Computer

#### Statische öffentliche IP-Adressen
##### [Portal](virtual-network-deploy-static-pip-arm-portal.md)
##### [PowerShell](virtual-network-deploy-static-pip-arm-ps.md)
##### [BEFEHLSZEILENSCHNITTSTELLE (CLI)](virtual-network-deploy-static-pip-arm-cli.md)
##### [Vorlage](virtual-network-deploy-static-pip-arm-template.md)
##### [PowerShell (klassisch)](virtual-networks-reserved-public-ip.md)

#### Statische private IP-Adressen
##### [Portal](virtual-networks-static-private-ip-arm-pportal.md)
##### [PowerShell](virtual-networks-static-private-ip-arm-ps.md)
##### [BEFEHLSZEILENSCHNITTSTELLE (CLI)](virtual-networks-static-private-ip-arm-cli.md)
##### [Portal (klassisch)](virtual-networks-static-private-ip-classic-pportal.md)
##### [PowerShell (klassisch)](virtual-networks-static-private-ip-classic-ps.md)
##### [CLI (klassisch)](virtual-networks-static-private-ip-classic-cli.md)

#### Mehrere Netzwerkschnittstellen
##### [PowerShell](virtual-network-deploy-multinic-arm-ps.md)
##### [BEFEHLSZEILENSCHNITTSTELLE (CLI)](virtual-network-deploy-multinic-arm-cli.md)
##### [Vorlage](virtual-network-deploy-multinic-arm-template.md)
##### [PowerShell (klassisch)](virtual-network-deploy-multinic-classic-ps.md)
##### [CLI (klassisch)](virtual-network-deploy-multinic-classic-cli.md)

#### [Mehrere IP-Adressen](virtual-network-multiple-ip-addresses-powershell.md)

### Konnektivitätsszenarien
#### [Virtuelles Netzwerk (VNET) zu VNET](../vpn-gateway/vpn-gateway-vnet-vnet-rm-ps.md?toc=%2fazure%2fvirtual-network%2ftoc.json)
#### [VNET (Resource Manager) zu VNET (klassisch)](../vpn-gateway/vpn-gateway-connect-different-deployment-models-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json)
#### [VNET zu lokalem Netzwerk (VPN)](../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json)
#### [VNET zu lokalem Netzwerk (ExpressRoute)](../expressroute/expressroute-howto-linkvnet-portal-resource-manager.md?toc=%2fazure%2fvirtual-network%2ftoc.json)
#### [Hoch verfügbare Hybrid-Netzwerkarchitektur](../guidance/guidance-hybrid-network-expressroute-vpn-failover.md?toc=%2fazure%2fvirtual-network%2ftoc.json)

### Sicherheitsszenarien
#### [Schützen von Netzwerken mit virtuellen Geräten](virtual-network-scenario-udr-gw-nva.md)
#### [DMZ zwischen Azure und dem Internet](../guidance/guidance-iaas-ra-secure-vnet-dmz.md?toc=%2fazure%2fvirtual-network%2ftoc.json)
#### [Clouddienst und Netzwerksicherheit](../best-practices-network-security.md?toc=%2fazure%2fvirtual-network%2ftoc.json)
##### [Einfache DMZ mit NSGs](virtual-networks-dmz-nsg-asm.md)
##### [DMZ mit Firewall und NSGs](virtual-networks-dmz-nsg-fw-asm.md)
##### [DMZ mit Firewall, UDR und NSGs](virtual-networks-dmz-nsg-fw-udr-asm.md)
##### [Beispielanwendung](virtual-networks-sample-app.md)

## Verwalten
### Netzwerksicherheitsgruppen
#### [Portal](virtual-network-manage-nsg-arm-portal.md)
#### [PowerShell](virtual-network-manage-nsg-arm-ps.md)
#### [BEFEHLSZEILENSCHNITTSTELLE (CLI)](virtual-network-manage-nsg-arm-cli.md)
#### [Protokolle](virtual-network-nsg-manage-log.md)
#### Problembehandlung
##### [Portal](virtual-network-nsg-troubleshoot-portal.md)
##### [PowerShell](virtual-network-nsg-troubleshoot-powershell.md)
### Problembehandlung bei Routen
#### [Portal](virtual-network-routes-troubleshoot-portal.md)
#### [PowerShell](virtual-network-routes-troubleshoot-powershell.md)
### Virtuelle Computer
#### [Anzeigen und Ändern von Hostnamen](virtual-networks-viewing-and-modifying-hostnames.md)
#### [Verschieben einer VM in ein anderes Subnetz](virtual-networks-move-vm-role-to-subnet.md)

# Referenz
## [PowerShell-Cmdlets (Resource Manager)](https://msdn.microsoft.com/library/mt163510(v=azure.300))
## [PowerShell-Cmdlets (klassisch)](https://msdn.microsoft.com/library/mt270335(v=azure.300))
## [APIs (Resource Manager)](https://msdn.microsoft.com/library/mt163658.aspx)
## [APIs (klassisch)](https://msdn.microsoft.com/library/jj157182.aspx)
## [Netzwerkblog](http://azure.microsoft.com/blog/topics/networking)
## [Häufig gestellte Fragen](virtual-networks-faq.md)

# Verwandte Themen
## [Virtuelle Computer](https://azure.microsoft.com/documentation/services/virtual-machines)
## [Application Gateway](https://azure.microsoft.com/documentation/services/application-gateway)
## [Azure DNS](https://azure.microsoft.com/documentation/services/dns)
## [Traffic Manager](https://azure.microsoft.com/documentation/services/traffic-manager)
## [Load Balancer](https://azure.microsoft.com/documentation/services/load-balancer)
## [VPN Gateway](https://azure.microsoft.com/documentation/services/vpn-gateway)
## [ExpressRoute](https://azure.microsoft.com/documentation/services/expressroute)



<!--HONumber=Nov16_HO2-->


