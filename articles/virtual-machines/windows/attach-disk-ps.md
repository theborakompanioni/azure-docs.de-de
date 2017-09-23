---
title: "Anfügen eines Datenträgers an einen virtuellen Windows-Computer in Azure mithilfe von PowerShell | Microsoft-Dokumentation"
description: "Hier erhalten Sie Informationen zum Anfügen von neuen oder vorhandenen Datenträgern an einen virtuellen Windows-Computer mithilfe von PowerShell mit dem Resource Manager-Bereitstellungsmodell."
services: virtual-machines-windows
documentationcenter: 
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
ms.date: 02/07/2017
ms.author: cynthn
ms.translationtype: HT
ms.sourcegitcommit: 83f19cfdff37ce4bb03eae4d8d69ba3cbcdc42f3
ms.openlocfilehash: 486e6a27fa28ec63001d824fe9f59c03a7aea5a7
ms.contentlocale: de-de
ms.lasthandoff: 08/21/2017

---

# <a name="attach-a-data-disk-to-a-windows-vm-using-powershell"></a>Anfügen eines Datenträgers an einen virtuellen Windows-Computer mithilfe von PowerShell

In diesem Artikel wird beschrieben, wie Sie mithilfe von PowerShell neue und vorhandene Datenträger an einen virtuellen Windows-Computer anfügen können. Wenn Ihr virtueller Computer verwaltete Datenträger verwendet, können Sie weitere verwaltete Datenträger anfügen. Sie können auch nicht verwaltete Datenträger an einen virtuellen Computer anfügen, der nicht verwaltete Datenträger in einem Speicherkonto verwendet.

Lesen Sie die folgenden Tipps, bevor Sie mit diesem Vorgang fortfahren:
* Die Größe des virtuellen Computers bestimmt, wie viele Datenträger Sie anfügen können. Ausführliche Informationen finden Sie unter [Größen für virtuelle Computer](sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
* Für die Verwendung von Premium-Speicher benötigen Sie einen virtuellen Computer mit einer für Premium-Speicher geeigneten Größe, z.B. die DS- oder GS-Serie. Sie können Datenträger aus Premium- und aus Standard-Speicherkonten für diese virtuellen Computer verwenden. Premium-Speicher ist in bestimmten Regionen verfügbar. Nähere Informationen finden Sie unter [Premium-Speicher: Hochleistungsspeicher für Workloads auf virtuellen Azure-Computern](../../storage/common/storage-premium-storage.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

## <a name="before-you-begin"></a>Voraussetzungen
Wenn Sie PowerShell verwenden, vergewissern Sie sich, dass Sie die neueste Version des AzureRM.Compute-PowerShell-Moduls verwenden. Führen Sie den folgenden Befehl aus, um es zu installieren.

```powershell
Install-Module AzureRM.Compute -RequiredVersion 2.6.0
```
Weitere Informationen finden Sie unter [Azure PowerShell-Versionsverwaltung](/powershell/azure/overview).


## <a name="add-an-empty-data-disk-to-a-virtual-machine"></a>Hinzufügen eines leeren Datenträgers zu einem virtuellen Computer

Dieses Beispiel zeigt, wie Sie einem vorhandenen virtuellen Computer einen leeren Datenträger hinzufügen.

### <a name="using-managed-disks"></a>Verwenden verwalteter Datenträger

```powershell
$rgName = 'myResourceGroup'
$vmName = 'myVM'
$location = 'West Central US' 
$storageType = 'PremiumLRS'
$dataDiskName = $vmName + '_datadisk1'

$diskConfig = New-AzureRmDiskConfig -AccountType $storageType -Location $location -CreateOption Empty -DiskSizeGB 128
$dataDisk1 = New-AzureRmDisk -DiskName $dataDiskName -Disk $diskConfig -ResourceGroupName $rgName

$vm = Get-AzureRmVM -Name $vmName -ResourceGroupName $rgName 
$vm = Add-AzureRmVMDataDisk -VM $vm -Name $dataDiskName -CreateOption Attach -ManagedDiskId $dataDisk1.Id -Lun 1

Update-AzureRmVM -VM $vm -ResourceGroupName $rgName
```


### <a name="using-unmanaged-disks-in-a-storage-account"></a>Verwenden nicht verwalteter Datenträger in einem Speicherkonto

```powershell
    $vm = Get-AzureRmVM -ResourceGroupName $rgName -Name $vmName
    Add-AzureRmVMDataDisk -VM $vm -Name "disk-name" -VhdUri "https://mystore1.blob.core.windows.net/vhds/datadisk1.vhd" -LUN 0 -Caching ReadWrite -DiskSizeinGB 1 -CreateOption Empty
    Update-AzureRmVM -ResourceGroupName $rgName -VM $vm
```


### <a name="initialize-the-disk"></a>Datenträger initialisieren

Nachdem Sie einen leeren Datenträger hinzugefügt haben, müssen Sie ihn initialisieren. Um den Datenträger zu initialisieren, können Sie sich bei einem virtuellen Computer anmelden und die Datenträgerverwaltung verwenden. Wenn Sie bei der Erstellung WinRM aktiviert und ein Zertifikat auf dem virtuellen Computer installiert haben, können Sie den Datenträger mit einem PowerShell-Remoteaufruf initialisieren. Alternativ können Sie auch eine benutzerdefinierte Skripterweiterung verwenden: 

```powershell
    $location = "location-name"
    $scriptName = "script-name"
    $fileName = "script-file-name"
    Set-AzureRmVMCustomScriptExtension -ResourceGroupName $rgName -Location $locName -VMName $vmName -Name $scriptName -TypeHandlerVersion "1.4" -StorageAccountName "mystore1" -StorageAccountKey "primary-key" -FileName $fileName -ContainerName "scripts"
```
        
Der Skriptdateiinhalt für die Datenträgerinitialisierung kann etwa wie folgender Code aussehen:

```powershell
    $disks = Get-Disk | Where partitionstyle -eq 'raw' | sort number

    $letters = 70..89 | ForEach-Object { [char]$_ }
    $count = 0
    $labels = "data1","data2"

    foreach ($disk in $disks) {
        $driveLetter = $letters[$count].ToString()
        $disk | 
        Initialize-Disk -PartitionStyle MBR -PassThru |
        New-Partition -UseMaximumSize -DriveLetter $driveLetter |
        Format-Volume -FileSystem NTFS -NewFileSystemLabel $labels[$count] -Confirm:$false -Force
    $count++
    }
```


## <a name="attach-an-existing-data-disk-to-a-vm"></a>Hinzufügen eines vorhandenen Datenträgers zu einem virtuellen Computer

Sie können auch eine vorhandene VHD als verwalteten Datenträger an einen virtuellen Computer anfügen. 

### <a name="using-managed-disks"></a>Verwenden verwalteter Datenträger

```powershell
$rgName = 'myRG'
$vmName = 'ContosoMdPir3'
$location = 'West Central US' 
$storageType = 'PremiumLRS'
$dataDiskName = $vmName + '_datadisk2'
$dataVhdUri = 'https://mystorageaccount.blob.core.windows.net/vhds/managed_data_disk.vhd' 

$diskConfig = New-AzureRmDiskConfig -AccountType $storageType -Location $location -CreateOption Import -SourceUri $dataVhdUri -DiskSizeGB 128

$dataDisk2 = New-AzureRmDisk -DiskName $dataDiskName -Disk $diskConfig -ResourceGroupName $rgName

$vm = Get-AzureRmVM -Name $vmName -ResourceGroupName $rgName 

$vm = Add-AzureRmVMDataDisk -VM $vm -Name $dataDiskName -CreateOption Attach -ManagedDiskId $dataDisk2.Id -Lun 2

Update-AzureRmVM -VM $vm -ResourceGroupName $rgName
```

## <a name="next-steps"></a>Nächste Schritte

Erstellen einer [Momentaufnahme](snapshot-copy-managed-disk.md)

