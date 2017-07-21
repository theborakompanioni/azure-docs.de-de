# Übersicht
## [Virtuelle Netzwerke](virtual-networks-overview.md)
## [Benutzerdefinierte Routen und IP-Weiterleitung](virtual-networks-udr-overview.md)
## [Peering in virtuellen Netzwerken](virtual-network-peering-overview.md)
## [Geschäftskontinuität](virtual-network-disaster-recovery-guidance.md)
## [HÄUFIG GESTELLTE FRAGEN](virtual-networks-faq.md)
## [IP-Adressierung](virtual-network-ip-addresses-overview-arm.md)
## Klassisch
### [IP-Adressierung](virtual-network-ip-addresses-overview-classic.md)
### [Zugriffssteuerungslisten](virtual-networks-acl.md)

# Erste Schritte
## [Erstellen Ihres ersten virtuellen Netzwerks](virtual-network-get-started-vnet-subnet.md)

# Anleitung
## Planen und Entwerfen
### [Virtuelle Netzwerke](virtual-network-vnet-plan-design-arm.md)
### [Netzwerksicherheitsgruppen](virtual-networks-nsg.md)

## Bereitstellen
### [Virtuelle Netzwerke](virtual-networks-create-vnet-arm-pportal.md)
#### [PowerShell](virtual-networks-create-vnet-arm-ps.md)
#### [BEFEHLSZEILENSCHNITTSTELLE (CLI)](virtual-networks-create-vnet-arm-cli.md)
#### [Vorlage](virtual-networks-create-vnet-arm-template-click.md)
#### Klassisch
##### [Portal](virtual-networks-create-vnet-classic-pportal.md)
##### [PowerShell](virtual-networks-create-vnet-classic-netcfg-ps.md)
##### [BEFEHLSZEILENSCHNITTSTELLE (CLI)](virtual-networks-create-vnet-classic-cli.md)

### Netzwerksicherheitsgruppen
#### [Portal](virtual-networks-create-nsg-arm-pportal.md)
#### [PowerShell](virtual-networks-create-nsg-arm-ps.md)
#### [BEFEHLSZEILENSCHNITTSTELLE (CLI)](virtual-networks-create-nsg-arm-cli.md)
#### [Vorlage](virtual-networks-create-nsg-arm-template.md)
#### Klassisch
##### [PowerShell](virtual-networks-create-nsg-classic-ps.md)
##### [BEFEHLSZEILENSCHNITTSTELLE (CLI)](virtual-networks-create-nsg-classic-cli.md)

### Benutzerdefinierte Routen
#### [PowerShell](virtual-network-create-udr-arm-ps.md)
#### [BEFEHLSZEILENSCHNITTSTELLE (CLI)](virtual-network-create-udr-arm-cli.md)
#### [Vorlage](virtual-network-create-udr-arm-template.md)
#### Klassisch
##### [PowerShell](virtual-network-create-udr-classic-ps.md)
##### [BEFEHLSZEILENSCHNITTSTELLE (CLI)](virtual-network-create-udr-classic-cli.md)

### Peering in virtuellen Netzwerken
#### [Gleiches Bereitstellungsmodell – gleiches Abonnement](virtual-network-create-peering.md)
#### [Gleiches Bereitstellungsmodell – andere Abonnements](create-peering-different-subscriptions.md)
#### [Andere Bereitstellungsmodelle – gleiches Abonnement](create-peering-different-deployment-models.md)
#### [Andere Bereitstellungsmodelle – andere Abonnements](create-peering-different-deployment-models-subscriptions.md)

### Virtuelle Computer
#### Erstellen eines virtuellen Computers mit einer statischen öffentlichen IP-Adresse
##### [Portal](virtual-network-deploy-static-pip-arm-portal.md)
##### [PowerShell](virtual-network-deploy-static-pip-arm-ps.md)
##### [BEFEHLSZEILENSCHNITTSTELLE (CLI)](virtual-network-deploy-static-pip-arm-cli.md)
##### [Vorlage](virtual-network-deploy-static-pip-arm-template.md)
##### Klassisch
###### [PowerShell](virtual-networks-reserved-public-ip.md)

#### Erstellen eines virtuellen Computers mit einer statischen privaten IP-Adresse
##### [Portal](virtual-networks-static-private-ip-arm-pportal.md)
##### [PowerShell](virtual-networks-static-private-ip-arm-ps.md)
##### [BEFEHLSZEILENSCHNITTSTELLE (CLI)](virtual-networks-static-private-ip-arm-cli.md)
##### Klassisch
###### [Portal](virtual-networks-static-private-ip-classic-pportal.md)
###### [PowerShell](virtual-networks-static-private-ip-classic-ps.md)
###### [BEFEHLSZEILENSCHNITTSTELLE (CLI)](virtual-networks-static-private-ip-classic-cli.md)

#### Erstellen eines virtuellen Computers mit mehreren Netzwerkschnittstellen
##### [PowerShell](../virtual-machines/windows/multiple-nics.md?toc=%2fazure%2fvirtual-network%2ftoc.json)
##### [BEFEHLSZEILENSCHNITTSTELLE (CLI)](../virtual-machines/linux/multiple-nics.md?toc=%2fazure%2fvirtual-network%2ftoc.json)
##### Klassisch
###### [PowerShell](virtual-network-deploy-multinic-classic-ps.md)
###### [BEFEHLSZEILENSCHNITTSTELLE (CLI)](virtual-network-deploy-multinic-classic-cli.md)

#### Erstellen eines virtuellen Computers mit mehreren IP-Adressen
##### [Azure-Portal](virtual-network-multiple-ip-addresses-portal.md)
##### [PowerShell](virtual-network-multiple-ip-addresses-powershell.md)
##### [BEFEHLSZEILENSCHNITTSTELLE (CLI)](virtual-network-multiple-ip-addresses-cli.md)
##### [Vorlage](virtual-network-multiple-ip-addresses-template.md)

#### [Erstellen eines virtuellen Computers mit beschleunigten Netzwerken](virtual-network-create-vm-accelerated-networking.md)

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
### Virtuelle Computer
#### [Hinzufügen oder Entfernen von Netzwerkschnittstellen](virtual-network-network-interface-vm.md)
#### [Namensauflösung für virtuelle Computer und Clouddienste](virtual-networks-name-resolution-for-vms-and-role-instances.md)
#### [Optimieren des Netzwerkdurchsatzes](virtual-network-optimize-network-bandwidth.md)
#### [Anzeigen und Ändern von Hostnamen](virtual-networks-viewing-and-modifying-hostnames.md)
### Klassisch
#### Zugriffssteuerungslisten
##### [Portal](../virtual-machines/windows/classic/setup-endpoints.md?toc=%2fazure%2fvirtual-network%2ftoc.json)
##### [PowerShell](virtual-networks-acl-powershell.md)

## Verwalten
### [Virtuelle Netzwerke](virtual-network-manage-network.md)
#### [Subnetze](virtual-network-manage-subnet.md)
#### [Peerings](virtual-network-manage-peering.md)
#### Klassisch
##### [Netzwerkkonfigurationsdatei](virtual-networks-using-network-configuration-file.md)
##### [Migrieren von einer Affinitätsgruppe in eine Region](virtual-networks-migrate-to-regional-vnet.md)
### Netzwerksicherheitsgruppen
#### [Portal](virtual-network-manage-nsg-arm-portal.md)
#### [PowerShell](virtual-network-manage-nsg-arm-ps.md)
#### [BEFEHLSZEILENSCHNITTSTELLE (CLI)](virtual-network-manage-nsg-arm-cli.md)
#### [Protokolle](virtual-network-nsg-manage-log.md)
### Netzwerkschnittstellen (NICs)
#### [Erstellen, Ändern oder Löschen von NICs](virtual-network-network-interface.md)
#### [Hinzufügen, Ändern oder Entfernen von IP-Adressen](virtual-network-network-interface-addresses.md)
### Virtuelle Computer
#### [Verschieben einer VM in ein anderes Subnetz](virtual-networks-move-vm-role-to-subnet.md)
### [Öffentliche IP-Adressen](virtual-network-public-ip-address.md)

## Problembehandlung
### Netzwerksicherheitsgruppen
#### [Portal](virtual-network-nsg-troubleshoot-portal.md)
#### [PowerShell](virtual-network-nsg-troubleshoot-powershell.md)
### Routen
#### [Portal](virtual-network-routes-troubleshoot-portal.md)
#### [PowerShell](virtual-network-routes-troubleshoot-powershell.md)
### [Durchsatztests](virtual-network-bandwidth-testing.md)
### [Löschen virtueller Netzwerke nicht möglich](virtual-network-troubleshoot-cannot-delete-vnet.md)

# Referenz
## [PowerShell (Resource Manager)](/powershell/module/azurerm.network)
## [PowerShell (klassisch)](/powershell/module/azure/)
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
## [Azure-Roadmap](https://azure.microsoft.com/roadmap/)
## [Netzwerkblog](http://azure.microsoft.com/blog/topics/networking)
## [Netzwerkforum](https://social.msdn.microsoft.com/Forums/azure/home?forum=WAVirtualMachinesVirtualNetwork)
## [Preise](https://azure.microsoft.com/pricing/details/virtual-network)
## [Stack Overflow](http://stackoverflow.com/questions/tagged/azure-virtual-network)
