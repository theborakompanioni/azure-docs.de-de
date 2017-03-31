---
title: Erstellen von VM-Skalierungsgruppen mit PowerShell-Cmdlets | Microsoft Docs
description: Erste Schritte zum Erstellen und Verwalten von Azure-VM-Skalierungsgruppen mit Azure PowerShell-Cmdlets
services: virtual-machines-windows
documentationcenter: 
author: danielsollondon
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 430d9d64-1f35-48f0-a4fd-9b69910ffa59
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/21/2017
ms.author: danielsollondon
translationtype: Human Translation
ms.sourcegitcommit: b2e89f0d5e6b14ce8d5847f8adc3d57a6122172e
ms.openlocfilehash: a3a36028a75d6cb7eb36277f3e2b5ab833c96a96
ms.lasthandoff: 02/21/2017


---
# <a name="creating-virtual-machine-scale-sets-using-powershell-cmdlets"></a>Erstellen von Skalierungsgruppen virtueller Computer mit PowerShell-Cmdlets
In diesem Artikel wird anhand eines Beispiels das Erstellen einer VM-Skalierungsgruppe (VMSS) beschrieben. Dabei wird eine Skalierungsgruppe mit drei Knoten mit zugeordnetem Netzwerk und Speicher erstellt.

## <a name="first-steps"></a>Erste Schritte
Vergewissern Sie sich, dass Sie das neueste Azure PowerShell-Modul installiert haben, damit Sie über die erforderlichen PowerShell-Cmdlets zum Verwalten und Erstellen von Skalierungsgruppen verfügen.
Wechseln Sie [hier](http://aka.ms/webpi-azps) zu den Befehlszeilentools, um auf die aktuellen verfügbaren Azure-Module zuzugreifen.

Um VMSS bezogene Cmdlets zu suchen, verwenden Sie die Zeichenfolge \*VMSS\*. Beispiel: _gcm *vmss*_

## <a name="creating-a-vmss"></a>Erstellen einer VMSS
#### <a name="create-resource-group"></a>Ressourcengruppe erstellen
```
$loc = 'westus';
$rgname = 'mynewrgwu';
  New-AzureRmResourceGroup -Name $rgname -Location $loc -Force;
```

### <a name="create-networking-vnet--subnet"></a>Erstellen des Netzwerks (VNET/Subnetz)
#### <a name="subnet-specification"></a>Subnetzspezifikation
```
$subnetName = 'websubnet'
  $subnet = New-AzureRmVirtualNetworkSubnetConfig -Name $subnetName -AddressPrefix "10.0.0.0/24";
```

#### <a name="vnet-specification"></a>VNET-Spezifikation
```
$vnet = New-AzureRmVirtualNetwork -Force -Name ('vnet' + $rgname) -ResourceGroupName $rgname -Location $loc -AddressPrefix "10.0.0.0/16" -Subnet $subnet;
$vnet = Get-AzureRmVirtualNetwork -Name ('vnet' + $rgname) -ResourceGroupName $rgname;

# In this case assume the new subnet is the only one
$subnetId = $vnet.Subnets[0].Id;
```

#### <a name="create-public-ip-resource-to-allow-external-access"></a>Erstellen von öffentlichen IP-Ressourcen, um externen Zugriff zu ermöglichen
Dies ist an den Load Balancer gebunden.

```
$pubip = New-AzureRmPublicIpAddress -Force -Name ('pubip' + $rgname) -ResourceGroupName $rgname -Location $loc -AllocationMethod Dynamic -DomainNameLabel ('pubip' + $rgname);
$pubip = Get-AzureRmPublicIpAddress -Name ('pubip' + $rgname) -ResourceGroupName $rgname;
```

#### <a name="create-load-balancer"></a>Erstellen oder Aktualisieren eines Lastenausgleichs
```
$frontendName = 'fe' + $rgname
$backendAddressPoolName = 'bepool' + $rgname
$probeName = 'vmssprobe' + $rgname
$inboundNatPoolName = 'innatpool' + $rgname
$lbruleName = 'lbrule' + $rgname
$lbName = 'vmsslb' + $rgname

# Bind Public IP to Load Balancer
$frontend = New-AzureRmLoadBalancerFrontendIpConfig -Name $frontendName -PublicIpAddress $pubip
```

#### <a name="configure-load-balancer"></a>Konfigurieren des Load Balancers
Erstellen Sie die Back-End-Adresspoolkonfiguration. Sie wird von den NICs der VMs in der Skalierungsgruppe gemeinsam genutzt.

```
$backendAddressPool = New-AzureRmLoadBalancerBackendAddressPoolConfig -Name $backendAddressPoolName
```

Legen Sie den Lastenausgleich-Testport fest, ändern Sie die Einstellungen nach Bedarf für Ihre Anwendung.

```
$probe = New-AzureRmLoadBalancerProbeConfig -Name $probeName -RequestPath healthcheck.aspx -Protocol http -Port 80 -IntervalInSeconds 15 -ProbeCount 2
```

Erstellen Sie einen NAT-Pool für eingehenden Datenverkehr für direkte geroutete Verbindungen (ohne Load Balancer) für die VMs in der Skalierungsgruppe über den Load Balancer. Dies veranschaulicht die Verwendung von RDP und ist in Ihrer Anwendung möglicherweise nicht erforderlich.

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

Überprüfen Sie die LB-Einstellungen und die Portkonfigurationen für den Load Balancer. Beachten Sie, dass Regeln für eingehenden NAT-Datenverkehr erst angezeigt werden, wenn die VMs in der Skalierungsgruppe erstellt wurden.

```
$expectedLb = Get-AzureRmLoadBalancer -Name $lbName -ResourceGroupName $rgname
```

##### <a name="configure-and-create-the-scale-set"></a>Konfigurieren und Erstellen der Skalierungsgruppe
Beachten Sie, dass dieses Infrastrukturbeispiel zeigt, wie die Verteilung und Skalierung des Webdatenverkehrs für die Skalierungsgruppe eingerichtet wird, für die hier angegebenen Images der VMs sind jedoch keine Webdienste installiert.

```
# specify scale set Name
$vmssName = 'vmss' + $rgname;

## specify VMSS specific details
$adminUsername = 'azadmin';
$adminPassword = "Password1234!";

$PublisherName = 'MicrosoftWindowsServer'
$Offer         = 'WindowsServer'
$Sku          = '2012-R2-Datacenter'
$Version       = 'latest'
$vmNamePrefix = 'winvmss'

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
```

Erstellen der Skalierungsgruppenkonfiguration

```
# Specify number of nodes
$numberofnodes = 3

$vmss = New-AzureRmVmssConfig -Location $loc -SkuCapacity $numberofnodes -SkuName 'Standard_A2' -UpgradePolicyMode 'automatic' `
    | Add-AzureRmVmssNetworkInterfaceConfiguration -Name $subnetName -Primary $true -IPConfiguration $ipCfg `
    | Set-AzureRmVmssOSProfile -ComputerNamePrefix $vmNamePrefix -AdminUsername $adminUsername -AdminPassword $adminPassword `
    | Set-AzureRmVmssStorageProfile -OsDiskCreateOption 'FromImage' -OsDiskCaching 'None' `
    -ImageReferenceOffer $Offer -ImageReferenceSku $Sku -ImageReferenceVersion $Version `
    -ImageReferencePublisher $PublisherName `
    | Add-AzureRmVmssExtension -Name $extname -Publisher $publisher -Type $exttype -TypeHandlerVersion $extver -AutoUpgradeMinorVersion $true
```

Kompilieren der Skalierungsgruppenkonfiguration

```
New-AzureRmVmss -ResourceGroupName $rgname -Name $vmssName -VirtualMachineScaleSet $vmss -Verbose;
```

Damit haben Sie nun die Skalierungsgruppe erstellt. In diesem Beispiel können Sie die Verbindung mit einzelnen VMs mit RDP testen:

```
VM0 : pubipmynewrgwu.westus.cloudapp.azure.com:3360
VM1 : pubipmynewrgwu.westus.cloudapp.azure.com:3361
VM2 : pubipmynewrgwu.westus.cloudapp.azure.com:3362
```

