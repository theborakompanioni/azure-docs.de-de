---
title: Migrieren eines klassischen virtuellen Computers zu einem virtuellen ARM Managed Disks-Computer | Microsoft-Dokumentation
description: Migrieren Sie eine einzelne Azure-VM aus dem klassischen Bereitstellungsmodell zu Managed Disks im Ressourcen-Manager-Bereitstellungsmodell.
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
ms.date: 06/15/2017
ms.author: cynthn
ms.translationtype: Human Translation
ms.sourcegitcommit: ef1e603ea7759af76db595d95171cdbe1c995598
ms.openlocfilehash: 4bae56abfdc609ad40e6fbefe120493f1cd4e66d
ms.contentlocale: de-de
ms.lasthandoff: 06/16/2017


---

# <a name="manually-migrate-a-classic-vm-to-a-new-arm-managed-disk-vm-from-the-vhd"></a>Manuelles Migrieren eines klassischen virtuellen Computers zu einem neuen virtuellen ARM Managed Disks-Computer von der virtuellen Festplatte 


In diesem Abschnitt erhalten Sie Informationen zum Migrieren Ihrer vorhandenen Azure-VMs vom klassischen Bereitstellungsmodell zu [Managed Disks](../../storage/storage-managed-disks-overview.md) im Ressourcen-Manager-Bereitstellungsmodell.


## <a name="plan-for-the-migration-to-managed-disks"></a>Planen der Migration zu Managed Disks

Dieser Abschnitt hilft Ihnen, hinsichtlich VM- und Datenträgertypen die beste Entscheidung zu treffen.


### <a name="location"></a>Standort

Wählen Sie einen Standort, an dem Azure Managed Disks verfügbar sind. Wenn Sie zu Premium Managed Disks migrieren, stellen Sie außerdem sicher, dass Storage Premium in der Region verfügbar ist, in die die Migration erfolgen soll. Aktuelle Informationen zu verfügbaren Standorten finden Sie unter [Azure-Dienste nach Region](https://azure.microsoft.com/regions/#services).

### <a name="vm-sizes"></a>VM-Größen

Wenn Sie zu Premium Managed Disks migrieren, müssen Sie die Größe der VM in eine Storage Premium-fähige Größe ändern, die in der Region mit dem Standort der VM verfügbar ist. Überprüfen Sie die Storage Premium-fähigen VM-Größen. Die Größenspezifikationen der Azure-VM sind unter [Größen für virtuelle Computer](sizes.md)aufgelistet.
Sehen Sie sich die Leistungsmerkmale von virtuellen Computern für Storage Premium an, und wählen Sie die am besten für Ihre Workload geeignete VM-Größe aus. Stellen Sie sicher, dass auf Ihrem virtuellen Computer ausreichend Bandbreite zum Steuern des Datenverkehrs des Datenträgers verfügbar ist.

### <a name="disk-sizes"></a>Datenträgergrößen

**Premium Managed Disks**

Es gibt sieben verwaltete Premium-Datenträgertypen, die Sie mit Ihrem virtuellen Computer verwenden können. Jeder Typ weist bestimmte IOPS- und Durchsatzeinschränkungen auf. Berücksichtigen Sie diese Einschränkungen beim Auswählen des Premium-Datenträgertyps für Ihren virtuellen Computer je nach Anforderungen Ihrer Anwendung hinsichtlich Kapazität, Leistung, Skalierbarkeit und Spitzenlasten.

| Premium-Datenträgertyp  | P4    | P6    | P10   | P20   | P30   | P40   | P50   | 
|---------------------|-------|-------|-------|-------|-------|-------|-------|
| Datenträgergröße           | 128 GB| 512 GB| 128 GB| 512 GB            | 1024 GB (1 TB)    | 2048 GB (2 TB)    | 4095 GB (4 TB)    | 
| IOPS pro Datenträger       | 120   | 240   | 500   | 2.300              | 5.000              | 7.500              | 7.500              | 
| Durchsatz pro Datenträger | 25 MB pro Sekunde  | 50 MB pro Sekunde  | 100 MB pro Sekunde | 150 MB pro Sekunde | 200 MB pro Sekunde | 250 MB pro Sekunde | 250 MB pro Sekunde | 

**Verwaltete Standard-Datenträger**

Es gibt sieben Arten von verwalteten Standard-Datenträgern, die mit Ihrem virtuellen Computer verwendet werden können. Jede davon bietet unterschiedliche Kapazität, jedoch gelten für alle dieselben IOPS- und Durchsatzgrenzwerte. Wählen Sie den Typ der verwalteten Standard-Datenträger basierend auf den Kapazitätsanforderungen Ihrer Anwendung.

| Standard-Datenträgertyp  | S4               | S6               | S10              | S20              | S30              | S40              | S50              | 
|---------------------|---------------------|---------------------|------------------|------------------|------------------|------------------|------------------| 
| Datenträgergröße           | 30 GB            | 64 GB            | 128 GB           | 512 GB           | 1024 GB (1 TB)   | 2048 GB (2 TB)    | 4095 GB (4 TB)   | 
| IOPS pro Datenträger       | 500              | 500              | 500              | 500              | 500              | 500             | 500              | 
| Durchsatz pro Datenträger | 60 MB pro Sekunde | 60 MB pro Sekunde | 60 MB pro Sekunde | 60 MB pro Sekunde | 60 MB pro Sekunde | 60 MB pro Sekunde | 60 MB pro Sekunde | 


### <a name="disk-caching-policy"></a>Zwischenspeicherungsrichtlinie für Datenträger 

**Premium Managed Disks**

Standardmäßig ist die Richtlinie für das Zwischenspeichern für alle Premium-Datenträger *Schreibgeschützt* und für die Premium-Betriebssystem-Datenträger, die an den virtuellen Computer angeschlossen sind, *Lesen/Schreiben*. Diese Konfigurationseinstellung wird empfohlen, um die optimale E/A-Leistung für Ihre Anwendung zu erreichen. Für Datenträger mit hohem oder ausschließlichem Schreibzugriff (z. B. SQL Server-Protokolldateien) deaktivieren Sie das Zwischenspeichern, sodass Sie eine bessere Anwendungsleistung erzielen können.

### <a name="pricing"></a>Preise

Überprüfen Sie die [Preise für Managed Disks](https://azure.microsoft.com/en-us/pricing/details/managed-disks/). Die Preise für Premium Managed Disks (verwaltete Premium-Datenträger) sind identisch mit denen für Premium Unmanaged Disks (nicht verwaltete Premium-Datenträger). Doch die Preise für verwaltete Standard-Datenträger unterscheiden sich von denen für nicht verwaltete Standard-Datenträger.


## <a name="checklist"></a>Checkliste

1.  Ehe Sie zu Premium Managed Disks migrieren, müssen Sie prüfen, ob diese in der Zielregion angeboten werden.

2.  Bestimmen Sie die neue VM-Serie, die Sie verwenden möchten. Sie muss Storage Premium-fähig sein, wenn Sie zu Premium Managed Disks migrieren.

3.  Bestimmen Sie, ob die gewünschte VM-Größe, die Sie verwenden möchten, in der Zielregion der Migration angeboten wird. Die VM-Größe muss groß genug sein, um die Anzahl der vorhandenen Datenträger zu unterstützen. Wenn Sie z.B. über vier Datenträger verfügen, muss die VM mindestens zwei Kerne aufweisen. Berücksichtigen Sie auch die Anforderungen hinsichtlich Verarbeitungsleistung, Arbeitsspeicher und Netzwerkbandbreite.

4.  Halten Sie die Details zum aktuellen virtuellen Computer bereit, einschließlich der Liste der Datenträger und die zugehörigen VHD-Blobs.

Bereiten Sie Ihre Anwendung für Ausfallzeiten vor. Für eine einwandfreie Migration müssen Sie die gesamte Verarbeitung im aktuellen System beenden. Erst dann können Sie den konsistenten Zustand erreichen, den Sie auf die neue Plattform migrieren können. Die Dauer der Ausfallzeit hängt von der Datenmenge auf den zu migrierenden Datenträgern ab.


## <a name="migrate-the-vm"></a>Migrieren der VM

Bereiten Sie Ihre Anwendung für Ausfallzeiten vor. Für eine einwandfreie Migration müssen Sie die gesamte Verarbeitung im aktuellen System beenden. Erst dann können Sie den konsistenten Zustand erreichen, den Sie auf die neue Plattform migrieren können. Die Dauer der Ausfallzeit hängt von der Datenmenge auf den zu migrierenden Datenträgern ab.


1.  Legen Sie zunächst die allgemeinen Parameter fest:

    ```powershell
    $resourceGroupName = 'yourResourceGroupName'
    
    $location = 'your location' 
    
    $virtualNetworkName = 'yourExistingVirtualNetworkName'
    
    $virtualMachineName = 'yourVMName'
    
    $virtualMachineSize = 'Standard_DS3'
    
    $adminUserName = "youradminusername"
    
    $adminPassword = "yourpassword" | ConvertTo-SecureString -AsPlainText -Force
    
    $imageName = 'yourImageName'
    
    $osVhdUri = 'https://storageaccount.blob.core.windows.net/vhdcontainer/osdisk.vhd'
    
    $dataVhdUri = 'https://storageaccount.blob.core.windows.net/vhdcontainer/datadisk1.vhd'
    
    $dataDiskName = 'dataDisk1'
    ```

2.  Erstellen Sie einen verwalteten Betriebssystem-Datenträger mithilfe des VHD-Images der klassischen VM.

    Vergewissern Sie sich, dass Sie den vollständigen URI der Betriebssystem-VHD im Parameter „$osVhdUri“ angegeben haben. Geben Sie außerdem **-AccountType** je nach Datenträgertypen (Premium oder Standard), zu denen Sie migrieren, als **PremiumLRS** oder **StandardLRS** an.

    ```powershell
    $osDisk = New-AzureRmDisk -DiskName $osDiskName -Disk (New-AzureRmDiskConfig '
    -AccountType PremiumLRS -Location $location -CreateOption Import -SourceUri $osVhdUri) '
    -ResourceGroupName $resourceGroupName
    ```

3.  Fügen Sie den Betriebssystem-Datenträger an die neue VM an.

    ```powershell
    $VirtualMachine = New-AzureRmVMConfig -VMName $virtualMachineName -VMSize $virtualMachineSize
    $VirtualMachine = Set-AzureRmVMOSDisk -VM $VirtualMachine -ManagedDiskId $osDisk.Id '
    -StorageAccountType PremiumLRS -DiskSizeInGB 128 -CreateOption Attach -Windows
    ```

4.  Erstellen Sie anhand der VHD-Datendatei einen verwalteten Datenträger, und fügen Sie ihn der neuen VM hinzu.

    ```powershell
    $dataDisk1 = New-AzureRmDisk -DiskName $dataDiskName -Disk (New-AzureRmDiskConfig '
    -AccountType PremiumLRS -Location $location -CreationDataCreateOption Import '
    -SourceUri $dataVhdUri ) -ResourceGroupName $resourceGroupName
    
    $VirtualMachine = Add-AzureRmVMDataDisk -VM $VirtualMachine -Name $dataDiskName '
    -CreateOption Attach -ManagedDiskId $dataDisk1.Id -Lun 1
    ```

5.  Erstellen Sie die neue VM, indem Sie die öffentliche IP-Adresse, das virtuelle Netzwerk und die Netzwerkkarte festlegen.

    ```powershell
    $publicIp = New-AzureRmPublicIpAddress -Name ($VirtualMachineName.ToLower()+'_ip') '
    -ResourceGroupName $resourceGroupName -Location $location -AllocationMethod Dynamic
    
    $vnet = Get-AzureRmVirtualNetwork -Name $virtualNetworkName -ResourceGroupName $resourceGroupName
    
    $nic = New-AzureRmNetworkInterface -Name ($VirtualMachineName.ToLower()+'_nic') '
    -ResourceGroupName $resourceGroupName -Location $location -SubnetId $vnet.Subnets[0].Id '
    -PublicIpAddressId $publicIp.Id
    
    $VirtualMachine = Add-AzureRmVMNetworkInterface -VM $VirtualMachine -Id $nic.Id
    
    New-AzureRmVM -VM $VirtualMachine -ResourceGroupName $resourceGroupName -Location $location
    ```

> [!NOTE]
>Möglicherweise sind weitere Schritte zur Unterstützung Ihrer Anwendung erforderlich, die in dieser Anleitung nicht behandelt werden.
>
>

## <a name="next-steps"></a>Nächste Schritte

- Stellen Sie eine Verbindung mit dem virtuellen Computer her. Anweisungen dazu finden Sie unter [Gewusst wie: Herstellen einer Verbindung mit einem virtuellen Azure-Computer unter Windows und Anmelden auf diesem Computer](connect-logon.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).


