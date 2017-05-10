---
title: "Tutorial zu Verfügbarkeitsgruppen für virtuelle Windows-Computer in Azure | Microsoft-Dokumentation"
description: "Erfahren Sie mehr über die Verfügbarkeitsgruppen für virtuelle Windows-Computer in Azure."
documentationcenter: 
services: virtual-machines-windows
author: cynthn
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 04/26/2017
ms.author: cynthn
ms.translationtype: Human Translation
ms.sourcegitcommit: be3ac7755934bca00190db6e21b6527c91a77ec2
ms.openlocfilehash: 831c55939ad3673aa8e44f165e18e826f64b54cc
ms.contentlocale: de-de
ms.lasthandoff: 05/03/2017


---

# <a name="how-to-use-availability-sets"></a>Verwenden von Verfügbarkeitsgruppen

In diesem Tutorial erhalten Sie Informationen dazu, wie Sie die Verfügbarkeit von virtuellen Computern (VMs) steigern können, indem Sie sie einer logischen Gruppierung – einer sogenannten Verfügbarkeitsgruppe – zuweisen. Wenn Sie virtuelle Computer in einer Verfügbarkeitsgruppe erstellen, werden diese auf der Azure-Plattform in der zugrunde liegenden Infrastruktur verteilt. Bei einem Hardwarefehler oder einer geplanten Wartung auf der Plattform wird durch die Verwendung von Verfügbarkeitsgruppen sichergestellt, dass zumindest ein virtueller Computer weiterhin ausgeführt wird.

Die Schritte in diesem Tutorial können mit dem neuesten [Azure PowerShell](https://docs.microsoft.com/powershell/azureps-cmdlets-docs/)-Modul ausgeführt werden.

## <a name="availability-set-overview"></a>Übersicht über Verfügbarkeitsgruppen

Virtuelle Computer können in logischen Hardwaregruppierungen im zugrunde liegenden Azure-Rechenzentrum erstellt werden. Wenn Sie mehrere virtuelle Computer erstellen, werden die Compute- und Speicherressourcen auf die Hardware (z.B. Server, Netzwerkswitches und Speicher) verteilt. Diese Verteilung gewährleistet die Verfügbarkeit Ihrer App, wenn eine der Hardwarekomponenten gewartet wird. Diese logische Gruppierung lässt sich mit Verfügbarkeitsgruppen definieren.

Verfügbarkeitsgruppen bieten hohe Verfügbarkeit für die virtuellen Computer. Sie sollten zudem sicherstellen, dass Ihre Anwendungen auch so konzipiert werden, dass Ausfälle oder Wartungsereignisse toleriert werden.

## <a name="create-an-availability-set"></a>Verfügbarkeitsgruppe erstellen

Sie können mithilfe von [New-AzureRmAvailabilitySet](/powershell/module/azurerm.compute/new-azurermavailabilityset) eine Verfügbarkeitsgruppe erstellen. Im folgenden Beispiel wird die Anzahl der Update- sowie der Fehlerdomänen für die Verfügbarkeitsgruppe *myAvailabilitySet* in der Ressourcengruppe *myResourceGroupAvailability* auf *2* festgelegt.


```powershell
New-AzureRmAvailabilitySet `
   -Location EastUS `
   -Name myAvailabilitySet `
   -ResourceGroupName myResourceGroupAvailability `
   -Managed `
   -PlatformFaultDomainCount 2 `
   -PlatformUpdateDomainCount 2
```

## <a name="create-vms-inside-an-availability-set"></a>Erstellen von virtuellen Computern in einer Verfügbarkeitsgruppe

Virtuelle Computer müssen in der Verfügbarkeitsgruppe erstellt werden, um sicherzustellen, dass sie ordnungsgemäß auf die Hardwarekomponenten verteilt werden. Nach der Erstellung kann einer Verfügbarkeitsgruppe kein vorhandener virtueller Computer mehr hinzugefügt werden. 

Die Hardware an einem Standort ist in mehrere Updatedomänen und Fehlerdomänen unterteilt. Eine **Updatedomäne** ist eine Gruppe von virtuellen Computern und der zugrunde liegenden physischen Hardwarekomponenten, die gleichzeitig neu gestartet werden können. Virtuelle Computer in ein und derselben **Fehlerdomäne** nutzen denselben Speicher sowie eine Stromquelle und einen Netzwerkswitch gemeinsam. 

Beim Erstellen einer VM-Konfiguration mit [New-AzureRMVMConfig](/powershell/module/azurerm.compute/new-azurermvmconfig) legen Sie die Verfügbarkeitsgruppe mit dem Parameter `-AvailabilitySetId` fest, um die ID der Verfügbarkeitsgruppe anzugeben.

Erstellen Sie mit [New-AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm) in der Verfügbarkeitsgruppe zwei virtuelle Computer.

```powershell
$availabilitySet = Get-AzureRmAvailabilitySet `
    -ResourceGroupName myResourceGroupAvailability `
    -Name myAvailabilitySet

$cred = Get-Credential -Message "Enter a username and password for the virtual machine."

$subnetConfig = New-AzureRmVirtualNetworkSubnetConfig `
    -Name mySubnet `
    -AddressPrefix 192.168.1.0/24
$vnet = New-AzureRmVirtualNetwork `
    -ResourceGroupName myResourceGroupAvailability `
    -Location EastUS `
    -Name MYvNET `
    -AddressPrefix 192.168.0.0/16 `
    -Subnet $subnetConfig

for ($i=1; $i -le 2; $i++)
{
   $pip = New-AzureRmPublicIpAddress `
        -ResourceGroupName myResourceGroupAvailability `
        -Location EastUS `
        -Name "mypublicdns$(Get-Random)" `
        -AllocationMethod Static `
        -IdleTimeoutInMinutes 4

   $nsgRuleRDP = New-AzureRmNetworkSecurityRuleConfig `
        -Name myNetworkSecurityGroupRuleRDP$i `
        -Protocol Tcp `
        -Direction Inbound `
        -Priority 1000 `
        -SourceAddressPrefix * `
        -SourcePortRange * `
        -DestinationAddressPrefix * `
        -DestinationPortRange 3389 `
        -Access Allow

   $nsg = New-AzureRmNetworkSecurityGroup `
        -ResourceGroupName myResourceGroupAvailability `
        -Location EastUS `
        -Name myNetworkSecurityGroup$i `
        -SecurityRules $nsgRuleRDP

   $nic = New-AzureRmNetworkInterface `
        -Name myNic$i `
        -ResourceGroupName myResourceGroupAvailability `
        -Location EastUS `
        -SubnetId $vnet.Subnets[0].Id `
        -PublicIpAddressId $pip.Id `
        -NetworkSecurityGroupId $nsg.Id

   # Here is where we specify the availability set
   $vm = New-AzureRmVMConfig `
        -VMName myVM$i `
        -VMSize Standard_D1 `
        -AvailabilitySetId $availabilitySet.Id

   $vm = Set-AzureRmVMOperatingSystem `
        -VM $vm `
        -Windows -ComputerName myVM$i `   
        -Credential $cred `
        -ProvisionVMAgent `
        -EnableAutoUpdate
   $vm = Set-AzureRmVMSourceImage `
        -VM $vm `
        -PublisherName MicrosoftWindowsServer `
        -Offer WindowsServer `
        -Skus 2016-Datacenter `
        -Version latest
   $vm = Set-AzureRmVMOSDisk `
        -VM $vm `
        -Name myOsDisk$i `
        -DiskSizeInGB 128 `
        -CreateOption FromImage `
        -Caching ReadWrite
   $vm = Add-AzureRmVMNetworkInterface -VM $vm -Id $nic.Id
   New-AzureRmVM `
        -ResourceGroupName myResourceGroupAvailability `
        -Location EastUS `
        -VM $vm
}

```

Die Erstellung und Konfiguration der beiden virtuellen Computer dauert einige Minuten. Anschließend verfügen Sie über zwei virtuelle Computer, die auf die zugrunde liegenden Hardwarekomponenten verteilt sind. 

## <a name="check-for-available-vm-sizes"></a>Prüfen der verfügbaren VM-Größen 

Sie können der Verfügbarkeitsgruppe später weitere virtuelle Computer hinzufügen. Dazu müssen Sie jedoch wissen, welche VM-Größen in der Hardware verfügbar sind. Verwenden Sie [Get-AzureRMVMSize](/powershell/module/azurerm.compute/get-azurermvmsize), um alle verfügbaren Größen im Hardwarecluster für die Verfügbarkeitsgruppe aufzulisten.

```powershell
Get-AzureRmVMSize `
   -AvailabilitySetName myAvailabilitySet `
   -ResourceGroupName myResourceGroupAvailability  
```

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie erfahren, wie Sie eine VM-Skalierungsgruppe erstellen. Im nächsten Tutorial erhalten Sie Informationen zu VM-Skalierungsgruppen.

[Erstellen einer VM-Skalierungsgruppe](tutorial-create-vmss.md)



