<properties
	pageTitle="Häufige PowerShell-Netzwerkbefehle für VMs | Microsoft Azure"
	description="Enthält häufig verwendete PowerShell-Befehle als Einstiegshilfe für die Erstellung eines virtuellen Netzwerks und der zugeordneten Ressourcen für VMs."
	services="virtual-machines-windows"
	documentationCenter=""
	authors="davidmu1"
	manager="timlt"
	editor=""
	tags="azure-resource-manager"/>

<tags
	ms.service="virtual-machines-windows"
	ms.workload="infrastructure-services"
	ms.tgt_pltfrm="na"
	ms.devlang="na"
	ms.topic="article"
	ms.date="06/07/2016"
	ms.author="davidmu"/>

# Häufige Azure PowerShell-Netzwerkbefehle für VMs

Zur Erstellung eines virtuellen Computers müssen Sie ein [virtuelles Netzwerk](../virtual-network/virtual-networks-overview.md) erstellen oder über ein vorhandenes virtuelles Netzwerk verfügen, dem die VM hinzugefügt werden kann. Normalerweise müssen Sie beim Erstellen einer VM auch die in diesem Artikel beschriebenen Ressourcen erstellen.

## Erstellen von Ressourcen mit Azure PowerShell

Informationen dazu, wie Sie die aktuelle Version von Azure PowerShell installieren, das gewünschte Abonnement auswählen und sich beim Azure-Konto anmelden, finden Sie unter [Installieren und Konfigurieren von Azure PowerShell](../powershell-install-configure.md).

Ressource | Befehl 
-------------- | -------------------------
Subnetz | $internetSubnet = [New-AzureRmVirtualNetworkSubnetConfig](https://msdn.microsoft.com/library/mt619412.aspx) -Name internetSubnet -AddressPrefix 10.0.1.0/24<BR>$internalSubnet = New-AzureRmVirtualNetworkSubnetConfig -Name internalSubnet -AddressPrefix 10.0.2.0/24<BR><BR>Ein typisches Netzwerk kann über ein Subnetz für einen [Load Balancer mit Internetzugriff](../load-balancer/load-balancer-internet-overview.md) und ein separates Subnetz für einen [internen Load Balancer](../load-balancer/load-balancer-internal-overview.md) verfügen. |
Virtuelles Netzwerk | Erstellen Sie ein virtuelles Netzwerk:<BR><BR>$rgName = "[resource-group-name](../powershell-azure-resource-manager.md)"<BR>$locName = "[location-name](https://msdn.microsoft.com/library/azure/dn495177.aspx)"<BR>$vnetName = "virtual-network-name"<BR>$vnet = [New-AzureRmVirtualNetwork](https://msdn.microsoft.com/library/mt603657.aspx) -Name $vnetName -ResourceGroupName $rgName -Location $locName -AddressPrefix 10.0.0.0/16 -Subnet $internetSubnet,$internalSubnet<BR><BR>Rufen Sie eine Liste mit virtuellen Netzwerken ab:<BR><BR>[Get-AzureRmVirtualNetwork](https://msdn.microsoft.com/library/mt603515.aspx) -ResourceGroupName $rgName &#124; Sort Name &#124; Select Name<BR><BR>Listen Sie die Subnetze in einem virtuellen Netzwerk auf:<BR><BR>Get-AzureRmVirtualNetwork -Name $vnetName -ResourceGroupName $rgName &#124; Select Subnets<BR><BR>Die Liste mit den Subnetzen wird wie folgt angezeigt:<BR><BR>Subnetze<BR>-------<BR>{internetSubnet, internalSubnet}<BR><BR>Der Subnetzindex für internetSubnet ist 0, und der Subnetzindex für internalSubnet ist 1.
Domänennamenbezeichnung | $domName = "domain-name"<BR>[Test-AzureRmDnsAvailability](https://msdn.microsoft.com/library/mt619419.aspx) -DomainQualifiedName $domName -Location $locName<BR><BR>Sie können eine DNS-Domänennamenbezeichnung für eine [öffentliche IP-Ressource](../virtual-network/virtual-network-ip-addresses-overview-arm.md) angeben. Es wird eine Zuordnung für „domainnamelabel.location.cloudapp.azure.com“ zur öffentlichen IP-Adresse auf Azure-verwalteten DNS-Servern erstellt. Die Bezeichnung darf nur Buchstaben, Zahlen und Bindestriche enthalten. Das erste und letzte Zeichen muss jeweils ein Buchstabe oder eine Zahl sein, und die Domänennamenbezeichnung muss für den Azure-Standort eindeutig sein. Sie sollten immer testen, ob Ihre Domänennamenbezeichnung global eindeutig ist. Wenn **True** zurückgegeben wird, ist der vorgeschlagene Name global eindeutig.
Öffentliche IP-Adresse | $ipName = "public-ip-address-name"<BR>$pip = [New-AzureRmPublicIpAddress](https://msdn.microsoft.com/library/mt603620.aspx) -Name $ipName -ResourceGroupName $rgName -DomainNameLabel $domName -Location $locName -AllocationMethod Dynamic<BR><BR>Für die öffentliche IP-Adresse wird die Domänennamenbezeichnung verwendet, die Sie zuvor erstellt haben. Sie wird bei der Front-End-Konfiguration des Load Balancers genutzt.
Front-End-IP-Konfiguration | $feConfigName = "frontend-ip-config-name"<BR>$frontendIP = [New-AzureRmLoadBalancerFrontendIpConfig](https://msdn.microsoft.com/library/mt603510.aspx) -Name $feConfigName -PublicIpAddress $pip<BR><BR>Die Front-End-Konfiguration enthält die öffentliche IP-Adresse, die Sie zuvor für eingehenden Netzwerk-Datenverkehr erstellt haben.
Back-End-Adresspool | $bePoolName = "back-end-pool-name"<BR>$beAddressPool = [New-AzureRmLoadBalancerBackendAddressPoolConfig](https://msdn.microsoft.com/library/mt603791.aspx) -Name $bePoolName<BR><BR>Stellt interne Adressen für das Back-End des Load Balancers bereit, auf die über eine Netzwerkschnittstelle zugegriffen wird.
Test | $probeName = "health-probe-name"<BR>$healthProbe = [New-AzureRmLoadBalancerProbeConfig](https://msdn.microsoft.com/library/mt603847.aspx) -Name $probeName -RequestPath 'HealthProbe.aspx' -Protocol http -Port 80 -IntervalInSeconds 15 -ProbeCount 2<BR><BR>Enthält Integritätstests, die zum Überprüfen der Verfügbarkeit von Instanzen virtueller Computer im Back-End-Adresspool verwendet werden.
Lastenausgleichsregel | $lbRule = [New-AzureRmLoadBalancerRuleConfig](https://msdn.microsoft.com/library/mt619391.aspx) -Name HTTP -FrontendIpConfiguration $frontendIP -BackendAddressPool $beAddressPool -Probe $healthProbe -Protocol Tcp -FrontendPort 80 -BackendPort 80<BR><BR>Enthält Regeln, mit denen ein öffentlicher Port auf dem Load Balancer einem Port im Back-End-Adresspool zugeordnet wird.
Eingehende NAT-Regel | $ruleName = "NAT-rule-name"<BR>$inboundNATRule = [New-AzureRmLoadBalancerInboundNatRuleConfig](https://msdn.microsoft.com/library/mt603606.aspx) -Name $ruleName -FrontendIpConfiguration $frontendIP -Protocol TCP -FrontendPort 3441 -BackendPort 3389<BR><BR>Enthält Regeln, mit denen ein öffentlicher Port auf dem Load Balancer einem Port für einen bestimmten virtuellen Computer im Back-End-Adresspool zugeordnet wird.
Load Balancer | $lbName = "load-balancer-name"<BR>$loadBalancer = [New-AzureRmLoadBalancer](https://msdn.microsoft.com/library/mt619450.aspx) -ResourceGroupName $rgName -Name $lbName -Location $locName -FrontendIpConfiguration $frontendIP -InboundNatRule $inboundNATRule -LoadBalancingRule $lbRule -BackendAddressPool $beAddressPool -Probe $healthProbe
Netzwerkschnittstelle | $vnet = [Get-AzureRmVirtualNetwork](https://msdn.microsoft.com/library/mt603515.aspx) -Name $vnetName -ResourceGroupName $rgName<BR>$internalSubnet = [Get-AzureRmVirtualNetworkSubnetConfig](https://msdn.microsoft.com/library/mt603817.aspx) -Name "internalSubnet" -VirtualNetwork $vnet<BR>$nicName = "network-interface-name"<BR>$nic1= [New-AzureRmNetworkInterface](https://msdn.microsoft.com/library/mt619370.aspx) -ResourceGroupName $rgName -Name $nicName -Location $locName -PrivateIpAddress 10.0.2.6 -Subnet $internalSubnet -LoadBalancerBackendAddressPool $loadBalancer.BackendAddressPools[0] -LoadBalancerInboundNatRule $loadBalancer.InboundNatRules[0]<BR><BR>Erstellen Sie eine Netzwerkschnittstelle, indem Sie die öffentliche IP-Adresse und das zuvor erstellte virtuelle Netzwerksubnetz verwenden.
	
## Nächste Schritte

- Verwenden Sie die eben erstellte Netzwerkschnittstelle beim [Erstellen einer VM](virtual-machines-windows-ps-create.md).
- Erfahren Sie, wie Sie eine [VM mit mehreren Netzwerkschnittstellen erstellen](../virtual-network/virtual-networks-multiple-nics.md).

<!---HONumber=AcomDC_0629_2016-->