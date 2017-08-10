---
title: "Konvertieren einer Windows-VM von nicht verwalteten Datenträgern in verwaltete Datenträger – Azure Managed Disks | Microsoft-Dokumentation"
description: "Hier wird beschrieben, wie Sie eine Windows-VM von nicht verwalteten Datenträgern mithilfe von PowerShell im Resource Manager-Bereitstellungsmodell in verwaltete Datenträger konvertieren."
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
ms.date: 06/23/2017
ms.author: cynthn
ms.translationtype: HT
ms.sourcegitcommit: 99523f27fe43f07081bd43f5d563e554bda4426f
ms.openlocfilehash: 53681c58ca1eff394d6a3db2d6a026845ac03df1
ms.contentlocale: de-de
ms.lasthandoff: 08/05/2017

---

# <a name="convert-a-windows-virtual-machine-from-unmanaged-disks-to-managed-disks"></a>Konvertieren einer Windows-VM von nicht verwalteten Datenträgern in verwaltete Datenträger

Wenn Sie über vorhandene virtuelle Windows-Computer (VMs) verfügen, die nicht verwaltete Datenträger verwenden, können Sie die VMs über den [Azure Managed Disks-Dienst](../../storage/storage-managed-disks-overview.md) konvertieren, sodass verwaltete Datenträger verwendet werden. Bei diesem Prozess werden sowohl der Betriebssystemdatenträger als auch alle anderen angefügten Datenträger konvertiert.

In diesem Artikel wird beschrieben, wie Sie VMs mithilfe von Azure PowerShell konvertieren. Wenn Sie PowerShell installieren oder aktualisieren ausführen müssen, finden Sie unter [Installieren und Konfigurieren von Azure PowerShell](/powershell/azure/install-azurerm-ps.md) Informationen dazu.

## <a name="before-you-begin"></a>Voraussetzungen


* Lesen Sie den Abschnitt [Planen der Migration zu Managed Disks](on-prem-to-azure.md#plan-for-the-migration-to-managed-disks).

[!INCLUDE [virtual-machines-common-convert-disks-considerations](../../../includes/virtual-machines-common-convert-disks-considerations.md)]




## <a name="convert-single-instance-vms"></a>Konvertieren von Einzelinstanz-VMs
In diesem Abschnitt wird beschrieben, wie Sie für Einzelinstanz-VMs von Azure die Konvertierung von nicht verwalteten Datenträgern in verwaltete Datenträger durchführen. (Wenn Ihre VMs in einer Verfügbarkeitsgruppe enthalten sind, lesen Sie den nächsten Abschnitt.) 

1. Heben Sie die Zuordnung der VM mit dem Cmdlet [Stop AzureRmVM](/powershell/module/azurerm.compute/stop-azurermvm) auf. Im folgenden Beispiel wird die Zuordnung für die VM `myVM` in der Ressourcengruppe `myResourceGroup` aufgehoben: 

  ```powershell
  $rgName = "myResourceGroup"
  $vmName = "myVM"
  Stop-AzureRmVM -ResourceGroupName $rgName -Name $vmName -Force
  ```

2. Konvertieren Sie die VM mit dem Cmdlet [ConvertTo-AzureRmVMManagedDisk](/powershell/module/azurerm.compute/convertto-azurermvmmanageddisk) in verwaltete Datenträger. Mit dem folgenden Prozess wird die vorherige VM konvertiert, einschließlich des Betriebssystemdatenträgers und der anderen Datenträger:

  ```powershell
  ConvertTo-AzureRmVMManagedDisk -ResourceGroupName $rgName -VMName $vmName
  ```

3. Starten Sie die VM nach der Konvertierung in verwaltete Datenträger mit [Start-AzureRmVM](/powershell/module/azurerm.compute/start-azurermvm). Im folgende Beispiel wird die vorherige VM neu gestartet:

  ```powershell
  Start-AzureRmVM -ResourceGroupName $rgName -Name $vmName
  ```


## <a name="convert-vms-in-an-availability-set"></a>Konvertieren von VMs in einer Verfügbarkeitsgruppe

Falls sich die VMs, die Sie in verwaltete Datenträger konvertieren möchten, in einer Verfügbarkeitsgruppe befinden, müssen Sie zuerst für die Verfügbarkeitsgruppe die Konvertierung in eine verwaltete Verfügbarkeitsgruppe durchführen.

1. Konvertieren Sie die Verfügbarkeitsgruppe mit dem Cmdlet [Update AzureRmAvailabilitySet](/powershell/module/azurerm.compute/update-azurermavailabilityset). Im folgenden Beispiel wird die Verfügbarkeitsgruppe `myAvailabilitySet` aus der Ressourcengruppe `myResourceGroup` aktualisiert:

  ```powershell
  $rgName = 'myResourceGroup'
  $avSetName = 'myAvailabilitySet'

  $avSet = Get-AzureRmAvailabilitySet -ResourceGroupName $rgName -Name $avSetName
  Update-AzureRmAvailabilitySet -AvailabilitySet $avSet -Sku Aligned 
  ```

  Wenn die Region, in der sich die Verfügbarkeitsgruppe befindet, nur 2 verwaltete Fehlerdomänen umfasst, aber die Anzahl der nicht verwalteten Fehlerdomänen 3 beträgt, wird bei diesem Befehl eine ähnliche Fehlermeldung wie die Folgende angezeigt: „Die angegebene Fehlerdomänenanzahl 3 muss im Bereich 1 bis 2 liegen“. Um den Fehler zu beheben, ändern Sie die Fehlerdomäne in 2, und `Sku` in `Aligned` wie im Folgenden gezeigt:

  ```powershell
  $avSet.PlatformFaultDomainCount = 2
  Update-AzureRmAvailabilitySet -AvailabilitySet $avSet -Sku Aligned
  ```

2. Heben Sie die Zuordnung der VMs in der Verfügbarkeitsgruppe auf, und konvertieren Sie sie. Das folgende Skript hebt die Zuordnung aller VMs mit dem Cmdlet [Stop-AzureRmVM](/powershell/module/azurerm.compute/stop-azurermvm) auf, konvertiert sie mit [ConvertTo-AzureRmVMManagedDisk](/powershell/module/azurerm.compute/convertto-azurermvmmanageddisk) und startet sie mit [Start-AzureRmVM](/powershell/module/azurerm.compute/start-azurermvm) neu:

  ```powershell
  $avSet = Get-AzureRmAvailabilitySet -ResourceGroupName $rgName -Name $avSetName

  foreach($vmInfo in $avSet.VirtualMachinesReferences)
  {
     $vm = Get-AzureRmVM -ResourceGroupName $rgName | Where-Object {$_.Id -eq $vmInfo.id}
     Stop-AzureRmVM -ResourceGroupName $rgName -Name $vm.Name -Force
     ConvertTo-AzureRmVMManagedDisk -ResourceGroupName $rgName -VMName $vm.Name
     Start-AzureRmVM -ResourceGroupName $rgName -Name $vmName
  }
  ```


## <a name="convert-standard-managed-disks-to-premium"></a>Konvertieren von verwalteten Standarddatenträgern in Premium
Nachdem Sie Ihre VM in verwaltete Datenträger konvertiert haben, können Sie zwischen den Speichertypen wechseln. Sie können auch eine Mischung von Datenträgern einsetzen, die Standard- und Premium-Speicher verwenden. 

Im folgenden Beispiel wird gezeigt, wie Sie zwischen Standard- und Premium-Speicher wechseln. Zur Verwendung von verwalteten Premium-Datenträgern muss Ihre VM eine [VM-Größe](sizes.md) verwenden, die Premium-Speicher unterstützt. In diesem Beispiel erfolgt ein Wechsel zu einer Größe, die Premium-Speicher unterstützt.

```powershell
$rgName = 'myResourceGroup'
$vmName = 'YourVM'
$size = 'Standard_DS2_v2'
$vm = Get-AzureRmVM -Name $vmName -resourceGroupName $rgName

# Stop and deallocate the VM before changing the size
Stop-AzureRmVM -ResourceGroupName $rgName -Name $vmName -Force

# Change the VM size to a size that supports premium storage
$vm.HardwareProfile.VmSize = $size
Update-AzureRmVM -VM $vm -ResourceGroupName $rgName

# Get all disks in the resource group of the VM
$vmDisks = Get-AzureRmDisk -ResourceGroupName $rgName 

# For disks that belong to the selected VM, convert to premium storage
foreach ($disk in $vmDisks)
{
    if ($disk.OwnerId -eq $vm.Id)
    {
        $diskUpdateConfig = New-AzureRmDiskUpdateConfig –AccountType PremiumLRS
        Update-AzureRmDisk -DiskUpdate $diskUpdateConfig -ResourceGroupName $rgName `
        -DiskName $disk.Name
    }
}

Start-AzureRmVM -ResourceGroupName $rgName -Name $vmName
```

## <a name="troubleshooting"></a>Problembehandlung

Wenn während der Konvertierung ein Fehler auftritt oder sich eine VM aufgrund von Problemen bei einer vorherigen Konvertierung in einen fehlerhaften Zustand befindet, führen Sie das Cmdlet `ConvertTo-AzureRmVMManagedDisk` erneut aus. Derartige Probleme können normalerweise durch eine einfache Wiederholung behoben werden.


## <a name="next-steps"></a>Nächste Schritte

Erstellen Sie eine schreibgeschützte Kopie eines virtuellen Computers mit [Momentaufnahmen](snapshot-copy-managed-disk.md).


