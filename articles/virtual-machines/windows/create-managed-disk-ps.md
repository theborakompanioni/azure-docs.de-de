---
title: "Erstellen eines verwalteten Datenträgers auf der Grundlage einer VHD in Azure | Microsoft-Dokumentation"
description: "Erstellen Sie mit dem Resource Manager-Bereitstellungsmodell auf der Grundlage einer virtuellen Festplatte, die sich zurzeit in einem Azure-Speicherkonto befindet, einen verwalteten Datenträger."
services: virtual-machines-windows
documentationcenter: 
author: cynthn
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 02/05/2017
ms.author: cynthn
translationtype: Human Translation
ms.sourcegitcommit: aaf97d26c982c1592230096588e0b0c3ee516a73
ms.openlocfilehash: 0a6e034da0445e94178e04cecc3ce09d86d2d29a
ms.lasthandoff: 04/27/2017


---

# <a name="create-managed-disks-from-unmanaged-disks-in-a-storage-account"></a>Erstellen verwalteter Datenträger auf der Grundlage nicht verwalteter Datenträger in einem Speicherkonto

Auf der Grundlage eines vorhandenen Datenträgers oder Betriebssystem-Datenträgers, der sich zurzeit in einem Azure-Speicherkonto befindet, kann ein verwalteter Datenträger erstellt werden. Sie können auch einen leeren Datenträger erstellen, der als neuer Datenträger für einen virtuellen Computer verwendet werden kann. 

## <a name="before-you-begin"></a>Voraussetzungen
Wenn Sie PowerShell verwenden, vergewissern Sie sich, dass Sie die neueste Version des AzureRM.Compute-PowerShell-Moduls verwenden. Führen Sie den folgenden Befehl aus, um es zu installieren.

```powershell
Install-Module AzureRM.Compute -RequiredVersion 2.6.0
```
Weitere Informationen finden Sie unter [Azure PowerShell-Versionsverwaltung](/powershell/azure/overview).


## <a name="create-a-managed-disk-from-a-vhd-in-an-azure-storage-account"></a>Erstellen eines verwalteten Datenträgers auf der Grundlage einer VHD in einem Azure-Speicherkonto

Im Beispiel erstellen wir einen verwalteten Datenträger auf der Grundlage einer VHD und weisen ihm zur späteren Verwendung den Parameter **$disk1** zu. 

Der verwaltete Datenträger wird am Speicherort **USA, Westen** in einer Ressourcengruppe mit dem Namen **myResourceGroup** erstellt. Der Datenträger erhält den Namen **myDisk** und wird auf der Grundlage einer VHD-Datei mit dem Namen **myDisk.vhd** erstellt, die wir zuvor in ein Speicherkonto namens **mystorageaccount** hochgeladen haben. Der verwaltete Datenträger wird in lokal redundantem Storage Premium-Speicher (LRS, Locally Redundant Storage) erstellt. StandardLRS und PremiumLRS sind die einzigen verfügbaren **-AccountType**-Optionen für verwaltete Datenträger. 

1.  Festlegen einiger Parameter

    ```powershell
    $rgName = "myResourceGroup"
    $location = "West Central US"
    $diskName = "myDisk"
    $vhdUri = "https://mystorageaccount.blob.core.windows.net/vhds/myDisk.vhd"
    ```

2. Erstellen des Datenträgers 
    ```powershell
    $disk1 = New-AzureRmDisk -DiskName $diskName -Disk (New-AzureRmDiskConfig -AccountType PremiumLRS -Location $location -CreateOption Import -SourceUri $vhdUri) -ResourceGroupName $rgName
    ```
    
    

## <a name="create-an-empty-data-disk-as-a-managed-disk"></a>Erstellen eines leeren Datenträger als verwalteter Datenträger

Im Beispiel erstellen wir einen leeren Datenträger als verwalteten Datenträger und weisen ihm zur späteren Verwendung den Parameter **$dataDisk2** zu. Ein leerer Datenträger muss durch Anmelden bei dem virtuellen Computer und Verwendung von „diskmgmt.msc“ oder [remote mit WinRM und einem Skript](attach-disk-ps.md#initialize-the-disk) initialisiert werden, sobald er einem ausgeführten virtuellen Computer angefügt ist.

Der leere Datenträger wird am Speicherort **USA, Westen-Mitte** in einer Ressourcengruppe mit dem Namen **myResourceGroup** erstellt. Der Datenträger erhält den Namen **myEmptyDataDisk**. Der leere Datenträger wird in lokal redundantem Storage Premium-Speicher (LRS, Locally Redundant Storage) erstellt. StandardLRS und PremiumLRS sind die einzigen verfügbaren **-AccountType**-Optionen für verwaltete Datenträger.

In diesem Beispiel beträgt die Größe des Datenträgers 128GB, aber Sie sollten eine Größe auswählen, die die Anforderungen der auf Ihrem virtuellen Computer ausgeführten Anwendungen erfüllt.

1.  Festlegen einiger Parameter

    ```powershell
    $rgName = "myResourceGroup"
    $location = "West Central US"
    $dataDiskName = "myEmptyDataDisk"
    ```

2. Erstellen des Datenträgers
    ```powershell
    $dataDisk2 = New-AzureRmDisk -DiskName $dataDiskName -Disk (New-AzureRmDiskConfig -AccountType PremiumLRS -Location $location -CreateOption Empty -DiskSizeGB 128) -ResourceGroupName $rgName
    ```
    
## <a name="next-steps"></a>Nächste Schritte    
- Wenn Sie bereits einen virtuellen Computer haben, können Sie [einen Datenträger anfügen](attach-disk-portal.md).

