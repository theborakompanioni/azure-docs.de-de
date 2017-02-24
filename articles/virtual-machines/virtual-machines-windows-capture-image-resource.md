---
title: Erstellen eines verwalteten Images in Azure | Microsoft-Dokumentation
description: "Erstellen Sie ein verwaltetes Image eines generalisierten virtuellen Computers oder einer VHD in Azure. Mit Images können mehrere virtuelle Computer erstellt werden, die verwaltete Datenträger verwenden."
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
ms.date: 02/06/2017
ms.author: cynthn
translationtype: Human Translation
ms.sourcegitcommit: 204fa369dd6db618ec5340317188681b0a2988e3
ms.openlocfilehash: 12b2d12c5152c6e9b8f2e8c78fbbd6663cb24e45


---
# <a name="how-to-capture-a-managed-image-of-a-generalized-vm-in-azure"></a>Erstellen eines verwalteten Images eines generalisierten virtuellen Computers in Azure

Eine verwaltete Imageressource kann aus einem generalisierten virtuellen Computer erstellt werden, der entweder als verwalteter Datenträger oder als nicht verwalteter Datenträger in einem Speicherkonto gespeichert ist. Mit diesem Image können dann mehrere virtuelle Computer erstellt werden, die verwaltete Datenträger zum Speichern verwenden. 


## <a name="prerequisites"></a>Voraussetzungen
[Der virtuelle Computer muss generalisiert sein](virtual-machines-windows-generalize-vhd.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). Außerdem muss der Computer angehalten und seine Zuordnung aufgehoben sein. Beim Generalisieren einer VM werden u.a. alle persönlichen Kontoinformationen entfernt, und der Computer wird für die Verwendung als Image vorbereitet.



## <a name="create-a-managed-image-in-the-portal"></a>Erstellen eines verwalteten Images im Portal 

1. Öffnen Sie das [Portal](https://portal.azure.com).
2. Klicken Sie auf das Pluszeichen, um eine neue Ressource zu erstellen.
3. Geben Sie in der Filtersuche den Begriff **Image** ein.
4. Wählen Sie **Image** aus den Ergebnissen.
5. Klicken Sie auf dem Blatt **Image** auf **Erstellen**.
6. Geben Sie unter **Name** einen Namen für das Image ein.
7. Unter **Ressourcengruppe** wählen Sie entweder **Neu erstellen** aus und geben einen Namen ein, oder Sie wählen die Option **Aus vorhandenen** und dann aus der Dropdownliste eine Ressourcengruppe aus.
8. Wählen Sie unter **Speicherort** den Speicherort Ihrer Ressourcengruppe aus.
9. Unter **Betriebssystemtyp** wählen Sie den Typ des Betriebssystems, entweder Windows oder Linux.
11. Klicken Sie unter **Speicherblob** auf **Durchsuchen**, um die VHD im Azure-Speicher zu suchen.
12. Wählen Sie unter **Kontotyp** den Typ „Standard_LRS“ oder „Premium_LRS“. Standard verwendet Festplattenlaufwerke, Premium Solid-State Drives. Beide Typen verwenden lokal redundanten Speicher.
13. Wählen Sie unter **Datenträgercaching** die geeignete Datenträgercachingoption. Die Optionen sind **Kein**, **Schreibgeschützt** und **Lesen/Schreiben**.
14. Optional: Sie können dem Image auch einen vorhandenen Datenträger für Daten hinzufügen.  
15. Klicken Sie nach Abschluss der Auswahl auf **Erstellen**.
16. Nachdem das Image erstellt wurde, wird es in der von Ihnen ausgewählten Ressourcengruppe in der Liste der Ressourcen als **Image** angezeigt.



## <a name="create-a-managed-image-of-a-vm-using-powershell"></a>Erstellen eines verwalteten Images eines virtuellen Computers mithilfe von PowerShell

Durch Erstellen eines Images direkt von einem virtuellen Computer lässt sich sicherstellen, dass das Image alle Datenträger umfasst, die dem virtuellen Computer zugeordnet sind, einschließlich des Betriebssystemdatenträgers und allen Datenträgern für Daten.


Stellen Sie vor Beginn sicher, dass Sie die neueste Version des AzureRM.Compute-PowerShell-Moduls verwenden. Führen Sie den folgenden Befehl aus, um es zu installieren.

```powershell
Install-Module AzureRM.Compute -RequiredVersion 2.6.0
```
Weitere Informationen finden Sie unter [Azure PowerShell-Versionsverwaltung](https://docs.microsoft.com/powershell/azureps-cmdlets-docs/#azure-powershell-versioning).


1. Erstellen Sie einige Variablen. 
    ```powershell
    $vmName = "myVM"
    $rgName = "myResourceGroup"
    $location = "EastUS"
    $imageName = "myImage"
    ```
2. Stellen Sie sicher, dass die Zuordnung des virtuellen Computers aufgehoben wurde.

    ```powershell
    Stop-AzureRmVM -ResourceGroupName $rgName -Name $vmName -Force
    ```
    
3. Legen Sie den Status des virtuellen Computers auf **Generalisiert**fest. 
   
    ```powershell
    Set-AzureRmVm -ResourceGroupName $rgName -Name $vmName -Generalized
    ```
    
4. Rufen Sie den virtuellen Computer ab. 

    ```powershell
    $vm = Get-AzureRmVM -Name $vmName -ResourceGroupName $rgName
    ```

5. Erstellen Sie die Imagekonfiguration.

    ```powershell
    $image = New-AzureRmImageConfig -Location $location -SourceVirtualMachineId $vm.ID 
    ```
6. Erstellen Sie das Image.

    ```powershell
    New-AzureRmImage -Image $image -ImageName $imageName -ResourceGroupName $rgName
    ```    



## <a name="create-a-managed-image-of-a-vhd-in-powershell"></a>Erstellen eines verwalteten Images einer VHD mithilfe von PowerShell

Erstellen Sie ein verwaltetes Image mithilfe Ihrer generalisierten Betriebssystem-VHD.


1.  Legen Sie zunächst die allgemeinen Parameter fest:

    ```powershell
    $rgName = "myResourceGroupName"
    $vmName = "myVM"
    $location = "West Central US" 
    $imageName = "yourImageName"
    $osVhdUri = "https://storageaccount.blob.core.windows.net/vhdcontainer/osdisk.vhd"
    ```
2. Halten Sie den virtuellen Computer an, und heben Sie seine Zuordnung auf.

    ```powershell
    Stop-AzureRmVM -ResourceGroupName $rgName -Name $vmName -Force
    ```
    
3. Kennzeichnen Sie den virtuellen Computer als generalisiert.

    ```powershell
    Set-AzureRmVm -ResourceGroupName $rgName -Name $vmName -Generalized    
    ```
4.  Erstellen Sie das Image mithilfe Ihrer generalisierten Betriebssystem-VHD.

    ```powershell
    $imageConfig = New-AzureRmImageConfig -Location $location
    $imageConfig = Set-AzureRmImageOsDisk -Image $imageConfig -OsType Windows -OsState Generalized -BlobUri $osVhdUri
    $image = New-AzureRmImage -ImageName $imageName -ResourceGroupName $rgName -Image $imageConfig
    ```


## <a name="create-a-managed-image-from-a-snapshot-using-powershell"></a>Erstellen eines verwalteten Images aus einer Momentaufnahme mithilfe von PowerShell

Sie können ein verwaltetes Image auch aus einer Momentaufnahme der VHD eines generalisierten virtuellen Computers erstellen.

    
1. Erstellen Sie einige Variablen. 

    ```powershell
    $rgName = "myResourceGroup"
    $location = "EastUS"
    $snapshotName = "mySnapshot"
    $imageName = "myImage"
    ```

2. Rufen Sie die Momentaufnahme ab.

   ```powershell
   $snapshot = Get-AzureRmSnapshot -ResourceGroupName $rgName -SnapshotName $snapshotName
   ```
   
3. Erstellen Sie die Imagekonfiguration.

    ```powershell
    $imageConfig = New-AzureRmImageConfig -Location $location
    $imageConfig = Set-AzureRmImageOsDisk -Image $imageConfig -OsState Generalized -OsType Windows -SnapshotId $snapshot.Id
    ```
4. Erstellen Sie das Image.

    ```powershell
    New-AzureRmImage -ImageName $imageName -ResourceGroupName $rgName -Image $imageConfig
    ```    
    

## <a name="next-steps"></a>Nächste Schritte
- Jetzt können Sie [einen virtuellen Computer aus dem generalisierten verwalteten Image erstellen](virtual-machines-windows-create-vm-generalized-managed.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).    




<!--HONumber=Feb17_HO2-->


