---
title: "Ändern der Größe von virtuellen Azure-Computern in Azure mithilfe von PowerShell | Microsoft-Dokumentation"
description: "Ändern der Größe eines virtuellen Windows-Computers, der im Resource Manager-Bereitstellungsmodell erstellt wurde, mithilfe von Azure Powershell."
services: virtual-machines-windows
documentationcenter: 
author: Drewm3
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 057ff274-6dad-415e-891c-58f8eea9ed78
ms.service: virtual-machines-windows
ms.workload: na
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 10/19/2016
ms.author: drewm
translationtype: Human Translation
ms.sourcegitcommit: 6b80fa78fea505cd22bae2925ce1affbc1e48ab9
ms.openlocfilehash: 3112be84acd3f3e11252270c850955068d35029e


---
# <a name="resize-a-windows-vm"></a>Ändern der Größe eines virtuellen Windows-Computers
In diesem Artikel erfahren Sie, wie die Größe eines virtuellen Windows-Computers, der im Resource Manager-Bereitstellungsmodell erstellt wurde, mithilfe von Azure Powershell geändert wird.

Nachdem Sie einen virtuellen Computer (virtual machine; VM) erstellen, können Sie den virtuellen Computer zentral hoch- oder herunterskalieren, indem Sie die VM-Größe ändern. In einigen Fällen müssen Sie zuerst die Zuordnung des virtuellen Computers aufheben. Dies ist möglicherweise der Fall, falls die neue Größe auf dem Hardwarecluster nicht verfügbar ist, auf dem die VM aktuell gehostet wird.

## <a name="resize-a-windows-vm-not-in-an-availability-set"></a>Ändern der Größe eines virtuellen Windows-Computers nicht in einer Verfügbarkeitsgruppe
1. Listet die VM-Größen auf, die auf dem Hardwarecluster verfügbar sind, auf dem die VM gehostet wird. 
   
    ```powershell
    Get-AzureRmVMSize -ResourceGroupName <resourceGroupName> -VMName <vmName> 
    ```
2. Führen Sie die folgenden Befehle aus, um die Größe der VM zu ändern, sofern die gewünschte Größe aufgelistet ist. Wenn die gewünschte Größe nicht aufgeführt ist, fahren Sie mit Schritt 3 fort.
   
    ```powershell
    $vm = Get-AzureRmVM -ResourceGroupName <resourceGroupName> -VMName <vmName>
    $vm.HardwareProfile.VmSize = "<newVMsize>"
    Update-AzureRmVM -VM $vm -ResourceGroupName <resourceGroupName>
    ```
3. Führen Sie die folgenden Befehle aus, falls die gewünschte Größe nicht aufgelistet ist. Damit heben Sie die Zuordnung der VM auf, ändern ihre Größe, und starten sie neu.
   
    ```powershell
    $rgname = "<resourceGroupName>"
    $vmname = "<vmName>"
    Stop-AzureRmVM -ResourceGroupName $rgname -VMName $vmname -Force
    $vm = Get-AzureRmVM -ResourceGroupName $rgname -VMName $vmname
    $vm.HardwareProfile.VmSize = "<newVMSize>"
    Update-AzureRmVM -VM $vm -ResourceGroupName $rgname
    Start-AzureRmVM -ResourceGroupName $rgname -Name $vmname
    ```

> [!WARNING]
> Die Aufhebung der Zuordnung der VM gibt alle dynamischen IP-Adressen frei, die der VM zugewiesen sind. Das Betriebssystem und die Datenträger sind nicht betroffen. 
> 
> 

## <a name="resize-a-windows-vm-in-an-availability-set"></a>Ändern der Größe eines virtuellen Windows-Computers in einer Verfügbarkeitsgruppe
Wenn die neue Größe für einen virtuellen Computer in einer Verfügbarkeitsgruppe nicht im Hardwarecluster verfügbar ist, auf dem der virtuelle Computer derzeit gehostet wird, muss bei allen virtuellen Computern in der Verfügbarkeitsgruppe die Zuordnung aufgehoben werden, um die Größe des virtuellen Computers ändern zu können. Sie müssen möglicherweise auch die Größe der anderen virtuellen Computer in der Verfügbarkeitsgruppe aktualisieren, nachdem die Größe eines virtuellen Computers geändert wurde. Führen Sie die folgenden Schritte zum Ändern der Größe eines virtuellen Computers in einer Verfügbarkeitsgruppe aus.

1. Listet die VM-Größen auf, die auf dem Hardwarecluster verfügbar sind, auf dem die VM gehostet wird.
   
    ```powershell
    Get-AzureRmVMSize -ResourceGroupName <resourceGroupName> -VMName <vmName>
    ```
2. Führen Sie die folgenden Befehle aus, um die Größe der VM zu ändern, sofern die gewünschte Größe aufgelistet ist. Wenn sie nicht aufgelistet ist, gehen Sie zu Schritt 3.
   
    ```powershell
    $vm = Get-AzureRmVM -ResourceGroupName <resourceGroupName> -VMName <vmName>
    $vm.HardwareProfile.VmSize = "<newVmSize>"
    Update-AzureRmVM -VM $vm -ResourceGroupName <resourceGroupName>
    ```
3. Wenn die gewünschte Größe nicht aufgelistet ist, fahren Sie mit den folgenden Schritten zum Aufheben der Zuordnung aller virtuellen Computer in der Verfügbarkeitsgruppe fort, skalieren die VMs, und starten sie neu.
4. Beenden Sie alle virtuellen Computer in der Verfügbarkeitsgruppe.
   
   ```powershell
   $rg = "<resourceGroupName>"
   $as = Get-AzureRmAvailabilitySet -ResourceGroupName $rg
   $vmIds = $as.VirtualMachinesReferences
   foreach ($vmId in $vmIDs){
     $string = $vmID.Id.Split("/")
     $vmName = $string[8]
     Stop-AzureRmVM -ResourceGroupName $rg -Name $vmName -Force
   } 
   ```
5. Skalieren Sie alle virtuellen Computer in der Verfügbarkeitsgruppe und starten sie neu.
   
   ```powershell
   $rg = "<resourceGroupName>"
   $newSize = "<newVmSize>"
   $as = Get-AzureRmAvailabilitySet -ResourceGroupName $rg
   $vmIds = $as.VirtualMachinesReferences
   foreach ($vmId in $vmIDs){
     $string = $vmID.Id.Split("/")
     $vmName = $string[8]
     $vm = Get-AzureRmVM -ResourceGroupName $rg -Name $vmName
     $vm.HardwareProfile.VmSize = $newSize
     Update-AzureRmVM -ResourceGroupName $rg -VM $vm
     Start-AzureRmVM -ResourceGroupName $rg -Name $vmName
   }
   ```

## <a name="next-steps"></a>Nächste Schritte
* Führen Sie mehrere VM-Instanzen aus, und skalieren Sie diese zentral hoch, um zusätzliche Skalierbarkeit zu erhalten. Weitere Informationen finden Sie unter [Automatisches Skalieren von Windows-Computern in einer VM-Skalierungsgruppe](../virtual-machine-scale-sets/virtual-machine-scale-sets-windows-autoscale.md).




<!--HONumber=Jan17_HO4-->


