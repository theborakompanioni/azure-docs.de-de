# Übersicht
## [Virtuelle Netzwerke](virtual-networks-overview.md)
## [Benutzerdefinierte Routen und IP-Weiterleitung](virtual-networks-udr-overview.md)
## [Peering in virtuellen Netzwerken](virtual-network-peering-overview.md)
## [Geschäftskontinuität](virtual-network-disaster-recovery-guidance.md)
## [Häufig gestellte Fragen](virtual-networks-faq.md)
## IP-Adressierung
### [Ressourcen-Manager](virtual-network-ip-addresses-overview-arm.md)
### [Klassisch](virtual-network-ip-addresses-overview-classic.md)

# Erste Schritte
## [Erstellen Ihres ersten virtuellen Netzwerks](virtual-network-get-started-vnet-subnet.md)

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
#### [Portal](virtual-networks-create-nsg-arm-pportal.md)
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

### [Netzwerkschnittstellen](virtual-network-network-interface.md)

### [Öffentliche IP-Adressen](virtual-network-public-ip-address.md)

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

#### Mehrere IP-Adressen
##### [Azure-Portal](virtual-network-multiple-ip-addresses-portal.md)
##### [PowerShell](virtual-network-multiple-ip-addresses-powershell.md)
##### [BEFEHLSZEILENSCHNITTSTELLE (CLI)](virtual-network-multiple-ip-addresses-cli.md)
##### [Vorlage](virtual-network-multiple-ip-addresses-template.md)

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

## Konfigurieren
### Beschleunigte Netzwerke für virtuelle Computer
#### [Azure-Portal](virtual-network-accelerated-networking-portal.md)
#### [PowerShell](virtual-network-accelerated-networking-powershell.md)
### [Optimieren des VM-Netzwerkdurchsatzes](virtual-network-optimize-network-bandwidth.md)
### Zugriffssteuerungslisten
#### [Klassisches Portal](virtual-networks-acl.md)
#### [PowerShell](virtual-networks-acl-powershell.md)
### [Namensauflösung für virtuelle Computer und Clouddienste](virtual-networks-name-resolution-for-vms-and-role-instances.md)

## Verwalten
### Netzwerksicherheitsgruppen
#### [Portal](virtual-network-manage-nsg-arm-portal.md)
#### [PowerShell](virtual-network-manage-nsg-arm-ps.md)
#### [BEFEHLSZEILENSCHNITTSTELLE (CLI)](virtual-network-manage-nsg-arm-cli.md)
#### [Protokolle](virtual-network-nsg-manage-log.md)
### Virtuelle Computer
#### [Anzeigen und Ändern von Hostnamen](virtual-networks-viewing-and-modifying-hostnames.md)
#### [Verschieben einer VM in ein anderes Subnetz](virtual-networks-move-vm-role-to-subnet.md)

## Problembehandlung
### Netzwerksicherheitsgruppen
#### [Portal](virtual-network-nsg-troubleshoot-portal.md)
#### [PowerShell](virtual-network-nsg-troubleshoot-powershell.md)
### Routen
#### [Portal](virtual-network-routes-troubleshoot-portal.md)
#### [PowerShell](virtual-network-routes-troubleshoot-powershell.md)

# Referenz
## [PowerShell (Resource Manager)](/powershell/resourcemanager/azurerm.network/v3.4.0/azurerm.network)
## [PowerShell (klassisch)](/powershell/servicemanagement/azure.networking/v3.4.0/azure.networking)
## [Azure-Befehlszeilenschnittstelle](/cli/azure/network)
## [Java](/java/api/)
## [REST (Resource Manager)](https://msdn.microsoft.com/library/mt163658.aspx)
## [REST (klassisch)](https://msdn.microsoft.com/library/jj157182.aspx)


# Verwandte Themen
## [Virtuelle Computer](/azure/virtual-machines/)
## [Application Gateway](/azure/application-gateway/)
## [Azure DNS](/azure/dns/)
## [Traffic Manager](/azure/traffic-manager/)
## [Load Balancer](/azure/load-balancer/)
## [VPN Gateway](/azure/vpn-gateway/)
## [ExpressRoute](/azure/expressroute/)

# Ressourcen
## [Netzwerkblog](http://azure.microsoft.com/blog/topics/networking)
## [Netzwerkforum](https://social.msdn.microsoft.com/Forums/azure/home?forum=WAVirtualMachinesVirtualNetwork)
## [Preise](https://azure.microsoft.com/pricing/details/virtual-network)
## [Stack Overflow](http://stackoverflow.com/questions/tagged/azure-virtual-network)
