<properties
	pageTitle="Erstellen von Skalierungsgruppen virtueller Computer mit PowerShell-Cmdlets | Microsoft Azure"
	description="Erste Schritte zum Erstellen und Verwalten von Azure-VM-Skalierungsgruppen mit Azure PowerShell-Cmdlets"
	services="virtual-machines-windows"
	documentationCenter=""
	authors="danielsollondon"
	manager="timlt"
	editor=""
	tags="azure-resource-manager"/>

<tags
	ms.service="virtual-machines-windows"
	ms.workload="infrastructure-services"
	ms.tgt_pltfrm="na"
	ms.devlang="na"
	ms.topic="article"
	ms.date="03/30/2016"
	ms.author="danielsollondon"/>

# Erstellen von Skalierungsgruppen virtueller Computer mit PowerShell-Cmdlets

Dies ist ein Beispiel für das Erstellen einer Skalierungsgruppe virtueller Computer (Virtual Machine Scale Set, VMSS), in dem eine VMSS aus 3 Knoten mit zugeordnetem Netzwerk und Speicher erstellt wird.

## Erste Schritte
Stellen Sie sicher, dass Sie das neueste Azure PowerShell-Modul installiert haben, das die erforderlichen PowerShell-Cmdlets zum Verwalten und Erstellen der VMSS enthält. Wechseln Sie [hier](http://aka.ms/webpi-azps) zu den Befehlszeilentools, um auf die aktuellen verfügbaren Azure-Module zuzugreifen.

Um auf VMSS bezogene Cmdlets zu suchen, verwenden Sie die Zeichenfolge *VMSS*.

## Erstellen einer VMSS

##### Ressourcengruppe erstellen

```
$loc = 'westus';
$rgname = 'mynewrgwu';
  New-AzureRmResourceGroup -Name $rgname -Location $loc -Force;
```

##### Speicherkonto erstellen

Legen Sie Typ/Namen des Speicherkontos fest.

```
$stoname = 'sto' + $rgname;
$stotype = 'Standard_LRS';
 New-AzureRmStorageAccount -ResourceGroupName $rgname -Name $stoname -Location $loc -SkuName $stotype -Kind "Storage";

$stoaccount = Get-AzureRmStorageAccount -ResourceGroupName $rgname -Name $stoname;
```

#### Erstellen des Netzwerks (VNET/Subnetz)

##### Subnetzspezifikation

```
$subnetName = 'websubnet'
  $subnet = New-AzureRmVirtualNetworkSubnetConfig -Name $subnetName -AddressPrefix "10.0.0.0/24";
```

##### VNET-Spezifikation

```
$vnet = New-AzureRmVirtualNetwork -Force -Name ('vnet' + $rgname) -ResourceGroupName $rgname -Location $loc -AddressPrefix "10.0.0.0/16" -DnsServer "10.1.1.1" -Subnet $subnet;
$vnet = Get-AzureRmVirtualNetwork -Name ('vnet' + $rgname) -ResourceGroupName $rgname;

#In this case below we assume the new subnet is the only one, note difference if you have one already or have adjusted this code to more than one subnet.
$subnetId = $vnet.Subnets[0].Id;
```

##### Erstellen von öffentlichen IP-Ressourcen, um externen Zugriff zu ermöglichen

Dies ist an den Load Balancer gebunden.

```
$pubip = New-AzureRmPublicIpAddress -Force -Name ('pubip' + $rgname) -ResourceGroupName $rgname -Location $loc -AllocationMethod Dynamic -DomainNameLabel ('pubip' + $rgname);
$pubip = Get-AzureRmPublicIpAddress -Name ('pubip' + $rgname) -ResourceGroupName $rgname;
```

##### Erstellen und Konfigurieren des Load Balancers

```
$frontendName = 'fe' + $rgname
$backendAddressPoolName = 'bepool' + $rgname
$probeName = 'vmssprobe' + $rgname
$inboundNatPoolName = 'innatpool' + $rgname
$lbruleName = 'lbrule' + $rgname
$lbName = 'vmsslb' + $rgname

#Bind Public IP to Load Balancer
$frontend = New-AzureRmLoadBalancerFrontendIpConfig -Name $frontendName -PublicIpAddress $pubip
```

##### Konfigurieren des Load Balancers
Erstellen Sie die Back-End-Adresspool-Konfiguration. Sie wird von den NICs der VMs in der VMSS gemeinsam genutzt.

```
$backendAddressPool = New-AzureRmLoadBalancerBackendAddressPoolConfig -Name $backendAddressPoolName
```

Legen Sie den Lastenausgleich-Testport fest, ändern Sie die Einstellungen nach Bedarf für Ihre Anwendung.

```
$probe = New-AzureRmLoadBalancerProbeConfig -Name $probeName -RequestPath healthcheck.aspx -Protocol http -Port 80 -IntervalInSeconds 15 -ProbeCount 2
```

Erstellen Sie NAT-Regeln (Network Address Translation, Netzwerkadressenübersetzung) für direkte geroutete Verbindungen (ohne Lastenausgleich) für die VMs in der VMSS über den Load Balancer. Hinweis: Dies veranschaulicht die Verwendung von RDP und dient nur zur Demonstration. Interne VNET-Methoden sollten für RDP-Verbindungen mit diesen Servern verwendet werden.

```
$frontendpoolrangestart = 3360
$frontendpoolrangeend = 3370
$backendvmport = 3389
$inboundNatPool = New-AzureRmLoadBalancerInboundNatPoolConfig -Name $inboundNatPoolName -FrontendIPConfigurationId `
$frontend.Id -Protocol Tcp -FrontendPortRangeStart $frontendpoolrangestart -FrontendPortRangeEnd $frontendpoolrangeend -BackendPort $backendvmport;
```

Erstellen Sie die Lastenausgleichsregel. Dieses Beispiel zeigt Anforderungen über den Lastenausgleichsport 80, wobei die Einstellungen aus den vorherigen Schritten verwendet werden.

```
$protocol = 'Tcp'
$feLBPort = 80
$beLBPort = 80

$lbrule = New-AzureRmLoadBalancerRuleConfig -Name $lbruleName `
-FrontendIPConfiguration $frontend -BackendAddressPool $backendAddressPool `
-Probe $probe -Protocol $protocol -FrontendPort $feLBPort -BackendPort $beLBPort `
-IdleTimeoutInMinutes 15 -EnableFloatingIP -LoadDistribution SourceIP -Verbose;
```

Erstellen Sie den Load Balancer mit der Konfiguration.

```
$actualLb = New-AzureRmLoadBalancer -Name $lbName -ResourceGroupName $rgname -Location $loc `
-FrontendIpConfiguration $frontend -BackendAddressPool $backendAddressPool `
-Probe $probe -LoadBalancingRule $lbrule -InboundNatPool $inboundNatPool -Verbose;
```

Überprüfen Sie die LB-Einstellungen und die Portkonfigurationen für den Lastenausgleich. Beachten Sie, dass eingehende NAT-Regeln erst angezeigt werden, wenn die VMs in der VMSS erstellt worden sind.

```
$expectedLb = Get-AzureRmLoadBalancer -Name $lbName -ResourceGroupName $rgname
```

##### Konfigurieren und Erstellen von VMSS

Beachten Sie, dass dieses Infrastrukturbeispiel zeigt, wie die Verteilung und Skalierung des Webdatenverkehrs über die VMSS eingerichtet wird, aber für die hier angegebenen Images der VMs werden keine Webdienste installiert.

```
#specify VMSS Name
$vmssName = 'vmss' + $rgname;

##specify VMSS specific details
$adminUsername = 'azadmin';
$adminPassword = "Password1234!";

$PublisherName = 'MicrosoftWindowsServer'
$Offer         = 'WindowsServer'
$Sku          = '2012-R2-Datacenter'
$Version       = 'latest'
$vmNamePrefix = 'winvmss'

$vhdContainer = "https://" + $stoname + ".blob.core.windows.net/" + $vmssName;

###add an extension
$extname = 'BGInfo';
$publisher = 'Microsoft.Compute';
$exttype = 'BGInfo';
$extver = '2.1';
```

Binden der NIC an Load Balancer und Subnetz

```
$ipCfg = New-AzureRmVmssIPConfig -Name 'nic' `
-LoadBalancerInboundNatPoolsId $actualLb.InboundNatPools[0].Id `
-LoadBalancerBackendAddressPoolsId $actualLb.BackendAddressPools[0].Id `
-SubnetId $subnetId;

$ipCfg.LoadBalancerBackendAddressPools.Add($actualLb.BackendAddressPools[0].Id);
$ipCfg.LoadBalancerInboundNatPools.Add($actualLb.InboundNatPools[0].Id);
```

Erstellen der VMSS-Konfiguration

```
#Specify number of nodes
$numberofnodes = 3

$vmss = New-AzureRmVmssConfig -Location $loc -SkuCapacity $numberofnodes -SkuName 'Standard_A2' -UpgradePolicyMode 'automatic' `
    | Add-AzureRmVmssNetworkInterfaceConfiguration -Name $subnetName -Primary $true -IPConfiguration $ipCfg `
    | Set-AzureRmVmssOSProfile -ComputerNamePrefix $vmNamePrefix -AdminUsername $adminUsername -AdminPassword $adminPassword `
    | Set-AzureRmVmssStorageProfile -Name 'test' -OsDiskCreateOption 'FromImage' -OsDiskCaching 'None' `
    -ImageReferenceOffer $Offer -ImageReferenceSku $Sku -ImageReferenceVersion $Version `
    -ImageReferencePublisher $PublisherName -VhdContainer $vhdContainer `
    | Add-AzureRmVmssExtension -Name $extname -Publisher $publisher -Type $exttype -TypeHandlerVersion $extver -AutoUpgradeMinorVersion $true
```

Kompilieren der VMSS-Konfiguration

```
New-AzureRmVmss -ResourceGroupName $rgname -Name $vmssName -VirtualMachineScaleSet $vmss -Verbose;
```

Sie haben nun die VMSS erstellt. In diesem Beispiel können Sie die Verbindung mit einzelnen VMs mit RDP testen:

```
VM0 : pubipmynewrgwu.westus.cloudapp.azure.com:3360
VM1 : pubipmynewrgwu.westus.cloudapp.azure.com:3361
VM2 : pubipmynewrgwu.westus.cloudapp.azure.com:3362
```

<!---HONumber=AcomDC_0824_2016-->