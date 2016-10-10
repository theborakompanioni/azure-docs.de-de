<properties
    pageTitle="Erstellen eines Load Balancers mit Internetzugriff über IPv6 unter Verwendung von PowerShell für Resource Manager | Microsoft Azure"
    description="Erfahren Sie, wie Sie einen Load Balancer mit Internetzugriff über IPv6 unter Verwendung von PowerShell für Resource Manager erstellen."
    services="load-balancer"
    documentationCenter="na"
    authors="sdwheeler"
    manager="carmonm"
    editor=""
    tags="azure-resource-manager"
/>
<tags
    ms.service="load-balancer"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="infrastructure-services"
    ms.date="09/14/2016"
    ms.author="sewhee"
/>

# Erste Schritte zum Erstellen eines Load Balancers mit Internetzugriff über IPv6 unter Verwendung von PowerShell für Resource Manager

> [AZURE.SELECTOR]
- [PowerShell](load-balancer-IPv6-internet-ps.md)
- [Azure-Befehlszeilenschnittstelle](load-balancer-IPv6-internet-cli.md)
- [Vorlage](load-balancer-IPv6-internet-template.md)

Ein Azure Load Balancer ist ein Layer-4-Load Balancer (TCP, UDP). Der Load Balancer sorgt für hohe Verfügbarkeit, indem er eingehenden Datenverkehr zwischen funktionierenden Dienstinstanzen in Clouddiensten oder auf virtuelle Computer verteilt, die in einer Gruppe für den Lastenausgleich definiert wurden. Der Azure Load Balancer kann diese Dienste auch auf mehreren Ports, mehreren IP-Adressen oder beidem leisten.

## Beispielszenario für die Bereitstellung

Das folgende Diagramm veranschaulicht die Lösung mit Lastenausgleich, die in diesem Artikel bereitgestellt wird.

![Load Balancer-Szenarios](./media/load-balancer-ipv6-internet-ps/lb-ipv6-scenario.png)

In diesem Szenario erstellen Sie die folgenden Azure-Ressourcen:

- einen Load Balancer mit Internetzugriff mit je einer öffentlichen IPv4- und IPv6-IP-Adresse
- zwei Lastenausgleichsregeln, um die öffentlichen virtuellen IP-Adressen den privaten Endpunkten zuzuordnen
- eine Verfügbarkeitsgruppe, die die zwei virtuellen Computer enthält
- zwei virtuelle Computer (VMs)
- eine virtuelle Netzwerkschnittstelle für jeden virtuellen Computer mit zugewiesenen IPv4- und IPv6-Adressen

## Bereitstellen der Lösung mithilfe von Azure PowerShell

Die folgenden Schritte zeigen, wie Sie einen internen Load Balancer mit Internetzugriff mit dem Azure Resource Manager und PowerShell erstellen. Mit Azure Resource Manager werden die einzelnen Ressourcen erstellt sowie individuell konfiguriert und dann zusammengeführt, um eine Ressource zu erstellen.

Zum Bereitstellen eines Load Balancers erstellen und konfigurieren Sie die folgenden Objekte:

- Front-End-IP-Konfiguration: Enthält öffentliche IP-Adressen für eingehenden Netzwerkdatenverkehr.
- Back-End-Adresspool: Enthält Netzwerkschnittstellen (NICs), die virtuellen Computern den Empfang von Netzwerkdatenverkehr des Load Balancers ermöglichen.
- Lastenausgleichsregeln: Enthält Regeln für das Zuordnen eines öffentlichen Ports des Load Balancers zu einem Port im Back-End-Adresspool.
- NAT-Eingangsregeln: Enthält Regeln für das Zuordnen eines öffentlichen Ports des Load Balancers zu einem Port für einen bestimmten virtuellen Computer im Back-End-Adresspool.
- Tests: Enthält Integritätstests zum Prüfen der Verfügbarkeit von VM-Instanzen im Back-End-Adresspool.

Weitere Informationen finden Sie unter [Unterstützung des Azure Resource Managers für Load Balancer](load-balancer-arm.md).

## Einrichten von PowerShell für die Verwendung des Resource Managers

Stellen Sie sicher, dass Sie über die neueste Produktionsversion des Azure Resource Manager-Moduls für PowerShell verfügen.

1. Anmelden bei Azure

        Login-AzureRmAccount

    Geben Sie bei Aufforderung Ihre Anmeldeinformationen ein.

2. Überprüfen Sie die Abonnements für das Konto.

        Get-AzureRmSubscription

3. Wählen Sie aus, welches Azure-Abonnement Sie verwenden möchten.

        Select-AzureRmSubscription -SubscriptionId 'GUID of subscription'

4. Erstellen Sie eine Ressourcengruppe (überspringen Sie diesen Schritt, wenn Sie eine vorhandene Ressourcengruppe verwenden).

        New-AzureRmResourceGroup -Name NRP-RG -location "West US"

## Erstellen eines virtuellen Netzwerks und einer öffentlichen IP-Adresse für den Front-End-IP-Adresspool

1. Erstellen Sie ein virtuelles Netzwerk mit einem Subnetz.

        $backendSubnet = New-AzureRmVirtualNetworkSubnetConfig -Name LB-Subnet-BE -AddressPrefix 10.0.2.0/24
        $vnet = New-AzureRmvirtualNetwork -Name VNet -ResourceGroupName NRP-RG -Location 'West US' -AddressPrefix 10.0.0.0/16 -Subnet $backendSubnet

2. Erstellen Sie öffentliche Azure-IP-Adressressourcen (PIP) für den Front-End-IP-Adresspool.

        $publicIPv4 = New-AzureRmPublicIpAddress -Name 'pub-ipv4' -ResourceGroupName NRP-RG -Location 'West US' -AllocationMethod Static -IpAddressVersion IPv4 -DomainNameLabel lbnrpipv4
        $publicIPv6 = New-AzureRmPublicIpAddress -Name 'pub-ipv6' -ResourceGroupName NRP-RG -Location 'West US' -AllocationMethod Dynamic -IpAddressVersion IPv6 -DomainNameLabel lbnrpipv6

    >[AZURE.IMPORTANT] Der Load Balancer verwendet die Domänenbezeichnung der öffentlichen IP-Adresse als Präfix seines vollqualifizierten Domänennamens (FQDN). In diesem Beispiel sind die FQDNs *lbnrpipv4.westus.cloudapp.azure.com* und *lbnrpipv6.westus.cloudapp.azure.com*.

## Erstellen einer Front-End-IP-Konfiguration und eines Back-End-Adresspools

1. Erstellen Sie die Front-End-Adresskonfiguration, die die öffentlichen IP-Adressen verwendet, die Sie erstellt haben.

        $FEIPConfigv4 = New-AzureRmLoadBalancerFrontendIpConfig -Name "LB-Frontendv4" -PublicIpAddress $publicIPv4
        $FEIPConfigv6 = New-AzureRmLoadBalancerFrontendIpConfig -Name "LB-Frontendv6" -PublicIpAddress $publicIPv6

2. Erstellen Sie die Back-End-Adresspools.

        $backendpoolipv4 = New-AzureRmLoadBalancerBackendAddressPoolConfig -Name "BackendPoolIPv4"
        $backendpoolipv6 = New-AzureRmLoadBalancerBackendAddressPoolConfig -Name "BackendPoolIPv6"


## Erstellen von LB-Regeln, NAT-Regeln, Test und Load Balancer

In diesem Beispiel werden die folgenden Elemente erstellt:

- eine NAT-Regel, um sämtlichen eingehenden Datenverkehr an Port 443 für Port 4443 zu übersetzen
- eine Load Balancer-Regel für die gleichmäßige Verteilung des gesamten an Port 80 eingehenden Datenverkehrs an Port 80 der Adressen im Back-End-Pool.
- eine Testregel, die den Integritätsstatus der Seite *HealthProbe.aspx* oder eines Diensts an Port 8080 überprüft
- ein Load Balancer, der alle diese Objekte verwendet

1. Erstellen Sie die NAT-Regeln.

        $inboundNATRule1v4 = New-AzureRmLoadBalancerInboundNatRuleConfig -Name "NicNatRulev4" -FrontendIpConfiguration $FEIPConfigv4 -Protocol TCP -FrontendPort 443 -BackendPort 4443
        $inboundNATRule1v6 = New-AzureRmLoadBalancerInboundNatRuleConfig -Name "NicNatRulev6" -FrontendIpConfiguration $FEIPConfigv6 -Protocol TCP -FrontendPort 443 -BackendPort 4443

2. Erstellen Sie einen Integritätstest. Es gibt zwei Möglichkeiten, einen Test zu konfigurieren:

    HTTP-Test

        $healthProbe = New-AzureRmLoadBalancerProbeConfig -Name 'HealthProbe-v4v6' -RequestPath 'HealthProbe.aspx' -Protocol http -Port 80 -IntervalInSeconds 15 -ProbeCount 2

    oder TCP-Test

        $healthProbe = New-AzureRmLoadBalancerProbeConfig -Name 'HealthProbe-v4v6' -Protocol Tcp -Port 8080 -IntervalInSeconds 15 -ProbeCount 2

    In diesem Beispiel verwenden wir den TCP-Test.

3. Erstellen Sie eine Load Balancer-Regel.

        $lbrule1v4 = New-AzureRmLoadBalancerRuleConfig -Name "HTTPv4" -FrontendIpConfiguration $FEIPConfigv4 -BackendAddressPool backendpoolipv4 -Probe $healthProbe -Protocol Tcp -FrontendPort 80 -BackendPort 8080
        $lbrule1v6 = New-AzureRmLoadBalancerRuleConfig -Name "HTTPv6" -FrontendIpConfiguration $FEIPConfigv6 -BackendAddressPool backendpoolipv6 -Probe $healthProbe -Protocol Tcp -FrontendPort 80 -BackendPort 8080

4. Erstellen Sie den Load Balancer mithilfe der zuvor erstellten Objekte.

        $NRPLB = New-AzureRmLoadBalancer -ResourceGroupName NRP-RG -Name 'myNrpIPv6LB' -Location 'West US' -FrontendIpConfiguration $FEIPConfigv4,$FEIPConfigv6 -InboundNatRule $inboundNATRule1v6,$inboundNATRule1v4 -BackendAddressPool $backendpoolipv4,$backendpoolipv6 -Probe $healthProbe -LoadBalancingRule $lbrule1v4,$lbrule1v6

## Erstellen von NICs für die Back-End-VMs

1. Rufen Sie das virtuelle Netzwerk und das zugehörige Subnetz ab, in denen die Netzwerkkarten erstellt werden sollen.

        $vnet = Get-AzureRmVirtualNetwork -Name NRPVNet -ResourceGroupName NRP-RG
        $backendSubnet = Get-AzureRmVirtualNetworkSubnetConfig -Name LB-Subnet-BE -VirtualNetwork $vnet

2. Erstellen Sie die IP-Konfigurationen und NICs für die virtuellen Computer.

        $nic1IPv4 = New-AzureRmNetworkInterfaceIpConfig -Name "IPv4IPConfig" -PrivateIpAddressVersion "IPv4" -Subnet $backendSubnet -LoadBalancerBackendAddressPool $backendpoolipv4 -LoadBalancerInboundNatRule $inboundNATRule1v4
        $nic1IPv6 = New-AzureRmNetworkInterfaceIpConfig -Name "IPv6IPConfig" -PrivateIpAddressVersion "IPv6" -LoadBalancerBackendAddressPool $backendpoolipv6 -LoadBalancerInboundNatRule $inboundNATRule1v6
        $nic1 = New-AzureRmNetworkInterface -Name 'myNrpIPv6Nic0' -IpConfiguration $nic1IPv4,$nic1IPv6 -ResourceGroupName NRP-RG -Location 'West US'

        $nic2IPv4 = New-AzureRmNetworkInterfaceIpConfig -Name "IPv4IPConfig" -PrivateIpAddressVersion "IPv4" -Subnet $backendSubnet -LoadBalancerBackendAddressPool $backendpoolipv4
        $nic2IPv6 = New-AzureRmNetworkInterfaceIpConfig -Name "IPv6IPConfig" -PrivateIpAddressVersion "IPv6" -LoadBalancerBackendAddressPool $backendpoolipv6
        $nic2 = New-AzureRmNetworkInterface -Name 'myNrpIPv6Nic1' -IpConfiguration $nic2IPv4,$nic2IPv6 -ResourceGroupName NRP-RG -Location 'West US'

3. Erstellen Sie NICs, und verknüpfen Sie sie mit den NAT-Regeln und dem Back-End-Adresspool.

        $backendnic1= New-AzureRmNetworkInterface -ResourceGroupName NRP-RG -Name lb-nic1-be -Location 'West US' -PrivateIpAddress 10.0.2.6 -Subnet $backendSubnet -LoadBalancerBackendAddressPool $nrplb.BackendAddressPools[0] -LoadBalancerInboundNatRule $nrplb.InboundNatRules[0]

## Erstellen von virtuellen Computern und Zuweisen der neu erstellten NICs

Weitere Informationen zum Erstellen eines virtuellen Computers finden Sie unter [Erstellen und Vorkonfigurieren eines virtuellen Windows-Computers mit Resource Manager und Azure PowerShell](..\virtual-machines\virtual-machines-windows-ps-create.md).

1. Erstellen Sie eine Verfügbarkeitsgruppe und ein Speicherkonto.

        New-AzureRmAvailabilitySet -Name $availabilitySetName -ResourceGroupName NRP-RG -location 'West US'
        $availabilitySet = Get-AzureRmAvailabilitySet -Name 'myNrpIPv6AvSet' -ResourceGroupName NRP-RG
        New-AzureRmStorageAccount -ResourceGroupName NRP-RG -Name $vmStorageAccount -Location 'West US' -SkuName $LRS
        $CreatedStorageAccount = Get-AzureRmStorageAccount -ResourceGroupName NRP-RG -Name $vmStorageAccount

2. Erstellen Sie die einzelnen virtuellen Computer, und weisen Sie die zuvor erstellten NICs zu.

        $vm1 = New-AzureRmVMConfig -VMName 'myNrpIPv6VM0 -VMSize 'Standard_G1' -AvailabilitySetId $availabilitySet.Id
        $vm1 = Set-AzureRmVMOperatingSystem -VM $vm1 -Windows -ComputerName 'myNrpIPv6VM0 -Credential $mySecureCredentials -ProvisionVMAgent -EnableAutoUpdate
        $vm1 = Set-AzureRmVMSourceImage -VM $vm1 -PublisherName MicrosoftWindowsServer -Offer WindowsServer -Skus 2012-R2-Datacenter -Version "latest"
        $vm1 = Add-AzureRmVMNetworkInterface -VM $vm1 -Id $nic1.Id -Primary
        $osDisk1Uri = $CreatedStorageAccount.PrimaryEndpoints.Blob.ToString() + "vhds/myNrpIPv6VM0osdisk.vhd"
        $vm1 = Set-AzureRmVMOSDisk -VM $vm1 -Name 'myNrpIPv6VM0osdisk' -VhdUri $osDisk1Uri -CreateOption FromImage
        New-AzureRmVM -ResourceGroupName NRP-RG -Location 'West US' -VM $vm1

        $vm2 = New-AzureRmVMConfig -VMName 'myNrpIPv6VM1 -VMSize 'Standard_G1' -AvailabilitySetId $availabilitySet.Id
        $vm2 = Set-AzureRmVMOperatingSystem -VM $vm2 -Windows -ComputerName 'myNrpIPv6VM1 -Credential $mySecureCredentials -ProvisionVMAgent -EnableAutoUpdate
        $vm2 = Set-AzureRmVMSourceImage -VM $vm2 -PublisherName MicrosoftWindowsServer -Offer WindowsServer -Skus 2012-R2-Datacenter -Version "latest"
        $vm2 = Add-AzureRmVMNetworkInterface -VM $vm2 -Id $nic2.Id -Primary
        $osDisk2Uri = $CreatedStorageAccount.PrimaryEndpoints.Blob.ToString() + "vhds/myNrpIPv6VM1osdisk.vhd"
        $vm2 = Set-AzureRmVMOSDisk -VM $vm2 -Name 'myNrpIPv6VM1osdisk' -VhdUri $osDisk2Uri -CreateOption FromImage
        New-AzureRmVM -ResourceGroupName NRP-RG -Location 'West US' -VM $vm2

## Nächste Schritte

[Erste Schritte zum Konfigurieren des internen Lastenausgleichs](load-balancer-get-started-ilb-arm-ps.md)

[Konfigurieren eines Lastenausgleichs-Verteilungsmodus](load-balancer-distribution-mode.md)

[Konfigurieren von TCP-Leerlauftimeout-Einstellungen für den Lastenausgleich](load-balancer-tcp-idle-timeout.md)

<!---HONumber=AcomDC_0928_2016-->