---
title: "Erweitern eines Datenträgers, der an eine Windows-VM in Azure angefügt ist | Microsoft-Dokumentation"
description: "Vergrößern Sie einen Datenträger, der an einen virtuellen Windows-Computer angefügt ist, mithilfe von PowerShell."
services: virtual-machines-windows
documentationcenter: na
author: cynthn
manager: timlt
editor: 
tags: 
ms.assetid: 
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/02/2017
ms.author: cynthn
translationtype: Human Translation
ms.sourcegitcommit: abdbb9a43f6f01303844677d900d11d984150df0
ms.openlocfilehash: 620a28f4fb4421179c0ba030c10acba861760adf
ms.lasthandoff: 04/20/2017


---

# <a name="increase-the-size-of-a-data-disk-attached-to-a-windows-vm"></a>Vergrößern Sie einen Datenträger, der an eine Windows-VM angefügt ist

Wenn Sie einen Datenträger, der dem virtuellen Computer angefügt ist, vergrößern müssen, können Sie dies mithilfe von PowerShell erreichen. Nachdem Sie den Datenträger in den Einstellungen des virtuellen Azure-Computers vergrößert haben, müssen Sie auch den neuen Speicherplatz innerhalb des virtuellen Computers zuweisen.


## <a name="use-powershell-to-increase-the-size-of-a-managed-data-disk"></a>Verwenden von PowerShell zum Vergrößern eines verwalteten Datenträgers

Verwenden Sie die folgenden PowerShell-Cmdlets, um einen verwalteten Datenträger zu vergrößern:

|                                                                    |                                                            |
|--------------------------------------------------------------------|------------------------------------------------------------|
| [Get-AzureRMReseourceGroup](/powershell/module/azurerm.resources/get-azurermresourcegroup) | [Get-AzureRMVM](/powershell/module/azurerm.compute/get-azurermvm)                 |
| [Stop-AzureRMVM](/powershell/module/azurerm.compute/stop-azurermvm)                        | [Set-AzureRmVMDataDisk](/powershell/module/azurerm.compute/set-azurermvmdatadisk) |
| [Update-AzureRmVM](/powershell/module/azurerm.compute/update-azurermvm)                    | [Start-AzureRmVM](/powershell/module/azurerm.compute/start-azurermvm)             |
<br>

Das folgende Skript begleitet Sie beim Abrufen der Informationen der VM, dem Auswählen des Datenträgers und dem Angeben der neuen Größe.

```powershell
# Select resource group

    $rg = Get-AzureRMReseourceGroup | Out-GridView `
        -Title "Select the resource group" `
        -PassThru

    $rgName = $rg.ResourceGroupName

# Select the VM

    $vm = Get-AzureRMVM -ResourceGroupName $rgName `
        | Out-GridView `
            -Title "Select a VM" `
             -PassThru

# Select data disk

    $disk = $vm.dataDiskNames | Out-GridView `
        -Title "Select a data disk" `
        -PassThru

# Specify a larger size for the data disk

    $size =  Read-Host `
        -Prompt "New size in GB"

# Stop and Deallocate VM prior to resizing data disk

    $vm | Stop-AzureRMVM -Force

# Set the new disk size

    Set-AzureRmVMDataDisk -VM $vm -Name "$disk" -DiskSizeInGB $size

# View the new size of the data disk(s)

    $vm.StorageProfile.DataDisks

# Update the configuration in Azure

    Update-AzureRmVM -VM $vm -ResourceGroupName $rgName

# Start the VM

    Start-AzureRmVM -ResourceGroupName $rgName -VMName $vm.name

```

## <a name="use-powershell-to-increase-the-size-of-an-unmanaged-data-disk"></a>Verwenden von PowerShell zum Vergrößern eines nicht verwalteten Datenträgers

Verwenden Sie die folgenden PowerShell-Cmdlets, um nicht verwaltete Datenträger in einem Speicherkonto zu vergrößern:

|                                                                    |                                                            |
|--------------------------------------------------------------------|------------------------------------------------------------|
| [Get-AzureRMStorageAccount](/powershell/module/azurerm.storage/get-azurermstorageaccount) | [Get-AzureRMVM](/powershell/module/azurerm.compute/get-azurermvm)                 |
| [Stop-AzureRMVM](/powershell/module/azurerm.compute/stop-azurermvm)                       | [Set-AzureRmVMDataDisk](/powershell/module/azurerm.compute/set-azurermvmdatadisk) |
| [Update-AzureRmVM](/powershell/module/azurerm.compute/update-azurermvm)                   | [Start-AzureRmVM](/powershell/module/azurerm.compute/start-azurermvm)             |

<br>

Das folgende Skript begleitet Sie beim Abrufen der Informationen der VM und des Speicherkontos, dem Auswählen des Datenträgers und dem Angeben der neuen Größe.

```powershell

# Select Azure Storage Account

    $storageAccount =
        Get-AzureRMStorageAccount | Out-GridView `
            -Title "Select Azure Storage Account" `
            -PassThru

    $rgName = $storageAccount.ResourceGroupName

# Select the VM

    $vm = Get-AzureRMVM `
    -ResourceGroupName $rgName | Out-GridView `
            -Title "Select a VM …" `
            -PassThru

# Select Data Disk to resize

    $disk =
        $vm.DataDiskNames | Out-GridView `
            -Title "Select a data disk to resize" `
            -PassThru


# Specify a larger data disk size

    $size =  Read-Host `
        -Prompt "New size in GB"

# Stop and Deallocate VM prior to resizing data disk

    $vm | Stop-AzureRMVM -Force

# Set the new disk size

    Set-AzureRmVMDataDisk -VM $vm -Name "$disk" `
        -DiskSizeInGB $size

# Update the configuration in Azure

    Update-AzureRmVM -VM $vm -ResourceGroupName $rgName

# Start the VM
    Start-AzureRmVM -ResourceGroupName $rgName `
    -VMName $vm.name

```

## <a name="allocate-the-unallocated-disk-space"></a>Zuweisen des nicht zugewiesenen Speicherplatzes

Nachdem Sie das Laufwerk vergrößert haben, müssen Sie den neuen, nicht zugewiesenen Speicherplatz von innerhalb des virtuellen Computers zuweisen. Um den Speicherplatz zuzuweisen, können Sie eine Verbindung zur VM mit der Datenträgerverwaltung (diskmgmt.msc) herstellen. Wenn Sie bei der Erstellung WinRM aktiviert und ein Zertifikat auf dem virtuellen Computer installiert haben, können Sie den Datenträger auch mit einem PowerShell-Remoteaufruf initialisieren. Alternativ können Sie auch eine benutzerdefinierte Skripterweiterung verwenden:

```powershell
    $location = "location-name"
    $scriptName = "script-name"
    $fileName = "script-file-name"
    Set-AzureRmVMCustomScriptExtension -ResourceGroupName $rgName -Location $locName -VMName $vmName -Name $scriptName -TypeHandlerVersion "1.4" -StorageAccountName "mystore1" -StorageAccountKey "primary-key" -FileName $fileName -ContainerName "scripts"
```

Die Skriptdatei kann einen Code wie diesen enthalten, um die Laufwerkzuordnung auf die maximale Größe der Datenträger erhöhen:

```powershell
$driveLetter= "F"

$MaxSize = (Get-PartitionSupportedSize -DriveLetter $driveLetter).sizeMax

Resize-Partition -DriveLetter $driveLetter -Size $MaxSize
```

## <a name="next-steps"></a>Nächste Schritte
- [Erfahren Sie mehr über Datenträger und VHDs](../../storage/storage-about-disks-and-vhds-windows.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
