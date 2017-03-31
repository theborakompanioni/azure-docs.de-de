---
title: "Konvertieren eines virtuellen Computers von nicht verwalteten Datenträgern in verwaltete Datenträger – Azure | Microsoft-Dokumentation"
description: "Konvertieren eines virtuellen Computers von nicht verwalteten Datenträgern in verwaltete Datenträger mithilfe von PowerShell im Resource Manager-Bereitstellungsmodell"
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
ms.date: 02/22/2017
ms.author: cynthn
translationtype: Human Translation
ms.sourcegitcommit: 4f2230ea0cc5b3e258a1a26a39e99433b04ffe18
ms.openlocfilehash: 96137d49d3caa0444400edcfcfaa097dc71ba8e3
ms.lasthandoff: 03/25/2017


---
# <a name="convert-a-vm-from-unmanaged-disks-to-managed-disks"></a>Konvertieren eines virtuellen Computers von nicht verwalteten Datenträgern in verwaltete Datenträger

Wenn Sie über virtuelle Azure-Computer verfügen, für die nicht verwaltete Datenträger in Speicherkonten verwendet werden, und die Verwendung von [Managed Disks](../storage/storage-managed-disks-overview.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) möglich sein soll, können Sie die virtuellen Computer konvertieren. Bei diesem Prozess werden sowohl der Betriebssystemdatenträger als auch sämtliche angefügten Datenträger von der Verwendung nicht verwalteter Datenträger in einem Speicherkonto zur Verwendung verwalteter Datenträger konvertiert. Die virtuellen Computer werden heruntergefahren und freigegeben. Verwenden Sie dann Powershell zum Konvertieren des virtuellen Computers zur Verwendung verwalteter Datenträger. Nach der Konvertierung starten Sie den virtuellen Computer neu. Dieser verwendet jetzt verwaltete Datenträger.

Bevor Sie beginnen, lesen Sie die Informationen im Artikel [Planen der Migration zu Managed Disks](virtual-machines-windows-on-prem-to-azure.md#plan-for-the-migration-to-managed-disks).
Testen Sie den Migrationsprozess durch Migrieren eines virtuellen Testcomputers vor dem Ausführen der Migration in der Produktion, da der Migrationsvorgang nicht rückgängig gemacht werden kann.


> [!IMPORTANT] 
> Bei der Konvertierung werden Sie den virtuellen Computer freigeben. Das Freigeben des virtuellen Computers bedeutet, dass er eine neue IP-Adresse erhält, wenn er nach der Konvertierung gestartet wird. Verwenden Sie eine reservierte IP, wenn eine Abhängigkeit von einer festen IP-Adresse besteht.


## <a name="managed-disks-and-azure-storage-service-encryption-sse"></a>Managed Disks und Azure Storage Service Encryption (SSE)

Sie können einen nicht verwalteten virtuellen Computer, der im Resource Manager-Bereitstellungsmodell erstellt wurde, nicht in Managed Disks konvertieren, wenn sich einer der nicht verwalteten Datenträger in einem Speicherkonto befindet oder einmal befand, das mithilfe von [Azure Storage Service Encryption (SSE)](../storage/storage-service-encryption.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) verschlüsselt ist. Die folgenden Schritte beschreiben, wie Sie nicht verwaltete virtuelle Computer konvertieren, die sich in einem verschlüsselten Speicherkonto befinden (oder befanden):

**Datenträger:**
1.    Trennen Sie den Datenträger vom virtuellen Computer.
2.    Kopieren Sie die VHD-Datei in ein Speicherkonto, das nie für SSE aktiviert war. Verwenden Sie [AzCopy](../storage/storage-use-azcopy.md) zum Kopieren des Datenträgers in ein anderes Speicherkonto: `AzCopy /Source:https://sourceaccount.blob.core.windows.net/mycontainer1 /Dest:https://destaccount.blob.core.windows.net/mycontainer2 /SourceKey:key1 /DestKey:key2 /Pattern:myDataDisk.vhd`
3.    Fügen Sie den kopierten Datenträger an den virtuellen Computer an, und konvertieren Sie den virtuellen Computer.

**Betriebssystemdatenträger:**
1.    Beenden Sie die Freigabe des virtuellen Computers. Speichern Sie bei Bedarf die Konfiguration des virtuellen Computers.
2.    Kopieren Sie die Betriebssystem-VHD in ein Speicherkonto, das nie für SSE aktiviert war. Verwenden Sie [AzCopy](../storage/storage-use-azcopy.md) zum Kopieren des Datenträgers in ein anderes Speicherkonto: `AzCopy /Source:https://sourceaccount.blob.core.windows.net/mycontainer1 /Dest:https://destaccount.blob.core.windows.net/mycontainer2 /SourceKey:key1 /DestKey:key2 /Pattern:myVhd.vhd`
3.    Erstellen Sie einen virtuellen Computer, der verwaltete Datenträger verwendet, und fügen Sie diese VHD-Datei während der Erstellung als Betriebssystemdatenträger hinzu.

## <a name="convert-vms-in-an-availability-set-to-managed-disks-in-a-managed-availability-set"></a>Konvertieren von virtuellen Computern in einer Verfügbarkeitsgruppe in verwaltete Datenträger in einer verwalteten Verfügbarkeitsgruppe

Falls sich die VMs, die Sie in verwaltete Datenträger konvertieren möchten, in einer Verfügbarkeitsgruppe befinden, müssen Sie zuerst für die Verfügbarkeitsgruppe die Konvertierung in eine verwaltete Verfügbarkeitsgruppe durchführen.

Das folgende Skript aktualisiert die Verfügbarkeitsgruppe zu einer verwalteten Verfügbarkeitsgruppe, gibt sie frei, konvertiert die Datenträger und startet dann jeden virtuellen Computer in der Verfügbarkeitsgruppe neu.

```powershell
$rgName = 'myResourceGroup'
$avSetName = 'myAvailabilitySet'

$avSet =  Get-AzureRmAvailabilitySet -ResourceGroupName $rgName -Name $avSetName

Update-AzureRmAvailabilitySet -AvailabilitySet $avSet -Managed

foreach($vmInfo in $avSet.VirtualMachinesReferences)
    {
   $vm =  Get-AzureRmVM -ResourceGroupName $rgName | Where-Object {$_.Id -eq $vmInfo.id}

   Stop-AzureRmVM -ResourceGroupName $rgName -Name  $vm.Name -Force

   ConvertTo-AzureRmVMManagedDisk -ResourceGroupName $rgName -VMName $vm.Name
   
    }
```

## <a name="convert-existing-azure-vms-to-managed-disks-of-the-same-storage-type"></a>Migrieren vorhandener virtueller Azure-Computer zur Verwendung verwalteter Datenträger desselben Speichertyps

> [!IMPORTANT]
> Nach Abschluss des folgenden Verfahrens ist ein einzelnes Blob vorhanden, das im /vhds-Standardcontainer verbleibt. Der Name der Datei lautet „VMName.xxxxxxx.status“. Diese Datei wird von Azure nur erstellt, wenn Sie [VM-Erweiterungen](windows/classic/agents-and-extensions.md) auf dem virtuellen Computer installiert haben. Achten Sie darauf, dass Sie dieses verbleibende Statusobjekt nicht löschen. Dieses Problem wird in Kürze behoben sein.

In diesem Abschnitt wird beschrieben, wie Sie Ihre vorhandenen virtuellen Azure-Computer aus nicht verwalteten Datenträgern in Speicherkonten in verwaltete Datenträger konvertieren, wenn Sie denselben Speichertyp verwenden. Sie können diesen Prozess verwenden, um von nicht verwalteten Premium-Datenträgern (SDD) auf verwaltete Premium-Datenträger oder von nicht verwalteten Standard-Datenträgern (HDD) auf verwaltete Standard-Datenträger umzustellen. 

1. Erstellen Sie Variablen, und geben Sie den virtuellen Computer frei. In diesem Beispiel wird der Name der Ressourcengruppe auf **myResourceGroup** und der Name des virtuellen Computers auf **myVM** festgelegt.

    ```powershell
    $rgName = "myResourceGroup"
    $vmName = "myVM"
    Stop-AzureRmVM -ResourceGroupName $rgName -Name $vmName -Force
    ```
   
    Der *Status* der VM im Azure-Portal ändert sich von **Beendet** in **Beendet (Zuordnung aufgehoben)**.
    
2. Konvertieren Sie alle Datenträger, die dem virtuellen Computer zugeordnet sind, einschließlich des Betriebssystemdatenträgers und aller anderen Datenträger.

    ```powershell
    ConvertTo-AzureRmVMManagedDisk -ResourceGroupName $rgName -VMName $vmName
    ```


## <a name="migrate-existing-azure-vms-using-standard-unmanaged-disks-to-premium-managed-disks"></a>Migrieren vorhandener virtueller Azure-Computer mit nicht verwalteten Standard-Datenträgern zu verwalteten Premium-Datenträgern

In diesem Abschnitt erfahren Sie, wie Sie Ihre vorhandenen virtuellen Azure-Computer auf nicht verwalteten Standard-Datenträgern in verwaltete Premium-Datenträger konvertieren. Zur Verwendung von verwalteten Premium-Datenträgern muss Ihr virtueller Computer eine [Größe des virtuellen Computers](virtual-machines-windows-sizes.md) verwenden, die Storage Premium unterstützt.


1.  Legen Sie zunächst die allgemeinen Parameter fest. Stellen Sie sicher, dass die von Ihnen ausgewählte [Größe des virtuellen Computers](virtual-machines-windows-sizes.md) Storage Premium unterstützt.

    ```powershell
    $resourceGroupName = 'YourResourceGroupName'
    $vmName = 'YourVMName'
    $size = 'Standard_DS2_v2'
    ```
1.  Rufen Sie den virtuellen Computer mit nicht verwalteten Datenträgern ab:

    ```powershell
    $vm = Get-AzureRmVM -Name $vmName -ResourceGroupName $resourceGroupName
    ```
    
1.  Beenden Sie den virtuellen Computer (freigeben):

    ```powershell
    Stop-AzureRmVM -ResourceGroupName $resourceGroupName -Name $vmName -Force
    ```

1.  Aktualisieren Sie die Größe des virtuellen Computers auf eine Storage Premium-fähige Größe in der Region, in dem sich der virtuelle Computer befindet:

    ```powershell
    $vm.HardwareProfile.VmSize = $size
    Update-AzureRmVM -VM $vm -ResourceGroupName $resourceGroupName
    ```

1.  Konvertieren Sie den virtuellen Computer mit nicht verwalteten Datenträgern in Managed Disks. 

    Wenn Sie einen internen Serverfehler erhalten, versuchen Sie es zwei- bis dreimal, bevor Sie sich an unser Supportteam wenden.

    ```powershell
    ConvertTo-AzureRmVMManagedDisk -ResourceGroupName $resourceGroupName -VMName $vmName
    ```
1. Beenden Sie den virtuellen Computer (freigeben):

    ```powershell
    Stop-AzureRmVM -ResourceGroupName $resourceGroupName -Name $vmName -Force
    ```
2.  Aktualisieren Sie alle Datenträger auf Storage Premium:

    ```powershell
    $vmDisks = Get-AzureRmDisk -ResourceGroupName $resourceGroupName 
    foreach ($disk in $vmDisks) 
        {
        if($disk.OwnerId -eq $vm.Id)
            {
             $diskUpdateConfig = New-AzureRmDiskUpdateConfig –AccountType PremiumLRS
             Update-AzureRmDisk -DiskUpdate $diskUpdateConfig -ResourceGroupName $resourceGroupName `
             -DiskName $disk.Name
            }
        }
    ```
1. Starten Sie den virtuellen Computer.

    ```powershell
    Start-AzureRmVM -ResourceGroupName $resourceGroupName -Name $vmName
    ```
    
Sie können auch eine Mischung von Datenträgern besitzen, die Standard- und Premium-Speicher verwenden.
    

## <a name="next-steps"></a>Nächste Schritte

Erstellen Sie eine schreibgeschützte Kopie eines virtuellen Computers mit [Momentaufnahmen](virtual-machines-windows-snapshot-copy-managed-disk.md).


