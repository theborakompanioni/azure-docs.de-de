---
title: Verwalten von Azure-VMs mithilfe von PowerShell | Microsoft-Dokumentation
description: Verwalten von Azure-VMs mithilfe von PowerShell.
services: virtual-machines-windows
documentationcenter: 
author: davidmu1
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 48930854-7888-4e4c-9efb-7d1971d4cc14
ms.service: virtual-machines-windows
ms.workload: na
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 03/07/2017
ms.author: davidmu
translationtype: Human Translation
ms.sourcegitcommit: 197ebd6e37066cb4463d540284ec3f3b074d95e1
ms.openlocfilehash: ca695dd140af0a6f9b13f75e0402e1675b9e79d7
ms.lasthandoff: 03/31/2017


---
# <a name="manage-azure-virtual-machines-using-powershell"></a>Verwalten von Azure-VMs mithilfe von PowerShell

In diesem Artikel werden Ihnen häufige Verwaltungsaufgaben gezeigt, die Sie möglicherweise auf einer Azure-VM durchführen.

Unter [Installieren und Konfigurieren von Azure PowerShell](/powershell/azureps-cmdlets-docs) erfahren Sie, wie Sie die neueste Version von Azure PowerShell installieren, Ihr Abonnement auswählen und sich bei Ihrem Konto anmelden.

> [!NOTE]
> Unter Umständen müssen Sie Azure PowerShell neu installieren, um die in diesem Artikel beschriebene Funktionalität zu nutzen. Die Managed Disks-Funktionen sind in Version 3.5 und höher enthalten.
> 
> 

Erstellen Sie diese Variablen, um das Ausführen der Befehle zu erleichtern und die Werte so zu ändern, dass sie mit Ihrer Umgebung übereinstimmen:
    
```powershell
$myResourceGroup = "myResourceGroup"
$myVM = "myVM"
$location = "centralus"
```

## <a name="display-information-about-a-virtual-machine"></a>Anzeigen von Informationen zu einem virtuellen Computer

Rufen Sie Informationen zu einem virtuellen Computer ab.

```powershell
Get-AzureRmVM -ResourceGroupName $myResourceGroup -Name $myVM -DisplayHint Expand
```

Die Rückgabe sollte in etwa wie das folgende Beispiel aussehen:

    ResourceGroupName             : myResourceGroup
    Id                            : /subscriptions/{subscription-id}/resourceGroups/
                                    myResourceGroup/providers/Microsoft.Compute/virtualMachines/myVM
    VmId                          : #########-####-####-####-############
    Name                          : myVM
    Type                          : Microsoft.Compute/virtualMachines
    Location                      : centralus
    Tags                          : {}
    DiagnosticsProfile            :
      BootDiagnostics             :
      Enabled                     : True
      StorageUri                  : https://mystorage1.blob.core.windows.net/
    AvailabilitySetReference      : 
      Id                          : /subscriptions/{subscription-id}/resourceGroups/
                                    myResourceGroup/providers/Microsoft.Compute/availabilitySets/myAV1
    Extensions[0]                 :
      Id                          : /subscriptions/{subscription-id}/resourceGroups/
                                    myResourceGroup/providers/Microsoft.Compute/
                                    virtualMachines/myVM/extensions/BGInfo
      Name                        : BGInfo
      Type                        : Microsoft.Compute/virtualMachines/extensions
      Location                    : centralus
      Publisher                   : Microsoft.Compute
      VirtualMachineExtensionType : BGInfo
      TypeHandlerVersion          : 2.1
      AutoUpgradeMinorVersion     : True
      ProvisioningState           : Succeeded
    HardwareProfile               : 
      VmSize                      : Standard_DS1_v2
    NetworkProfile          
      NetworkInterfaces[0]        :
        Id                        : /subscriptions/{subscription-id}/resourceGroups/
                                    myResourceGroup/providers/Microsoft.Network/networkInterfaces/myNIC
    OSProfile                     : 
      ComputerName                : myVM
      AdminUsername               : myaccount1
      WindowsConfiguration        :
        ProvisionVMAgent          : True
         EnableAutomaticUpdates   : True
    ProvisioningState             : Succeeded
    StorageProfile                :
      ImageReference              :
        Publisher                 : MicrosoftWindowsServer
        Offer                     : WindowsServer
        Sku                       : 2012-R2-Datacenter
        Version                   : latest   
      OsDisk                      :
        OsType                    : Windows
        Name                      : myosdisk
        Vhd                       : 
          Uri                     : http://mystore1.blob.core.windows.net/vhds/myosdisk.vhd
        Caching                   : ReadWrite
        CreateOption              : FromImage
    NetworkInterfaceIDs[0]        : /subscriptions/{subscription-id}/resourceGroups/
                                    myResourceGroup/providers/Microsoft.Network/networkInterfaces/myNIC

## <a name="stop-a-virtual-machine"></a>Beenden eines virtuellen Computers

Sie können einen virtuellen Computer auf zwei Arten beenden. Sie können einen virtuellen Computer beenden und sämtliche Einstellungen beibehalten – dafür werden allerdings weiterhin Gebühren berechnet –, oder Sie können einen virtuellen Computer beenden und die Zuordnung aufheben. Wenn die Zuordnung eines virtuellen Computers aufgehoben wird, wird auch die Zuordnung aller mit diesem Computer verknüpften Ressourcen aufgehoben, und die Abrechnung wird beendet.

### <a name="stop-and-deallocate"></a>Beenden und Aufheben der Zuordnung
    
```powershell
Stop-AzureRmVM -ResourceGroupName $myResourceGroup -Name $myVM
```

Sie werden aufgefordert, den Vorgang zu bestätigen:

    Virtual machine stopping operation
    This cmdlet will stop the specified virtual machine. Do you want to continue?
    [Y] Yes [N] No [S] Suspend [?] Help (default is "Y"):
 
    Enter **Y** to stop the virtual machine.

Nach wenigen Minuten sollte eine Rückgabe ähnlich dem folgenden Beispiel angezeigt werden:

    OperationId :
    Status      : Succeeded
    StartTime   : 9/13/2016 12:11:57 PM
    EndTime     : 9/13/2016 12:14:40 PM
    Error       :

### <a name="stop-and-stay-provisioned"></a>Beenden und Bereitstellung erhalten

```powershell
Stop-AzureRmVM -ResourceGroupName $myResourceGroup -Name $myVM -StayProvisioned
```

Sie werden aufgefordert, den Vorgang zu bestätigen:

    Virtual machine stopping operation
    This cmdlet will stop the specified virtual machine. Do you want to continue?
    [Y] Yes [N] No [S] Suspend [?] Help (default is "Y"):
 
Geben Sie **Y** ein, um den virtuellen Computer zu beenden.

Nach wenigen Minuten sollte eine Rückgabe ähnlich dem folgenden Beispiel angezeigt werden:

    OperationId :
    Status      : Succeeded
    StartTime   : 9/13/2016 12:11:57 PM
    EndTime     : 9/13/2016 12:14:40 PM
    Error       :

## <a name="start-a-virtual-machine"></a>Starten eines virtuellen Computers
 
Starten Sie einen virtuellen Computer, wenn dieser angehalten wurde.

```powershell
Start-AzureRmVM -ResourceGroupName $myResourceGroup -Name $myVM
```

Nach wenigen Minuten sollte eine Rückgabe ähnlich dem folgenden Beispiel angezeigt werden:

    OperationId :
    Status      : Succeeded
    StartTime   : 9/13/2016 12:32:55 PM
    EndTime     : 9/13/2016 12:35:09 PM
    Error       :

Wenn Sie einen virtuellen Computer neu starten möchten, der bereits ausgeführt wird, verwenden Sie **Restart-AzureRmVM**. Diese Vorgehensweise wird nachfolgend beschrieben.

## <a name="restart-a-virtual-machine"></a>Neustarten eines virtuellen Computers

Starten Sie einen ausgeführten virtuellen Computer neu.

```powershell
Restart-AzureRmVM -ResourceGroupName $myResourceGroup -Name $myVM
```

Die Rückgabe sollte in etwa wie das folgende Beispiel aussehen:

    OperationId :
    Status      : Succeeded
    StartTime   : 9/13/2016 12:54:40 PM
    EndTime     : 9/13/2016 12:55:54 PM
    Error       :    

## <a name="add-a-data-disk-to-a-virtual-machine"></a>Hinzufügen eines Datenträgers zu einem virtuellen Computer
    
### <a name="managed-data-disk"></a>Verwalteter Datenträger

```powershell
$diskConfig = New-AzureRmDiskConfig -AccountType PremiumLRS -Location $location -CreateOption Empty -DiskSizeGB 128
$dataDisk = New-AzureRmDisk -DiskName "myDataDisk1" -Disk $diskConfig -ResourceGroupName $myResourceGroup
$vm = Get-AzureRmVM -Name $myVM -ResourceGroupName $myResourceGroup
Add-AzureRmVMDataDisk -VM $vm -Name "myDataDisk1" -VhdUri "https://mystore1.blob.core.windows.net/vhds/myDataDisk1.vhd" -LUN 0 -Caching ReadWrite -DiskSizeinGB 1 -CreateOption Empty
Update-AzureRmVM -ResourceGroupName $myResourceGroup -VM $vm
```

Nach dem Ausführen von Add-AzureRmVMDataDisk sollte Ihnen etwas ähnlich diesem Beispiel angezeigt werden:

    ResourceGroupName        : myResourceGroup
    Id                       : /subscriptions/{subscription-id}/resourceGroups/myResourceGroup/providers/
                               Microsoft.Compute/virtualMachines/myVM
    VmId                     : ########-####-####-####-############
    Name                     : myVM
    Type                     : Microsoft.Compute/virtualMachines
    Location                 : centralus
    Tags                     : {}
    AvailabilitySetReference : {Id}
    DiagnosticsProfile       : {BootDiagnostics}
    HardwareProfile          : {VmSize}
    NetworkProfile           : {NetworkInterfaces}
    OSProfile                : {ComputerName, AdminUsername, WindowsConfiguration, Secrets}
    ProvisioningState        : Succeeded
    StorageProfile           : {ImageReference, OsDisk, DataDisks}
    DataDiskNames            : {myDataDisk1}
    NetworkInterfaceIDs      : {/subscriptions/{subscription-id}/resourceGroups/myResourceGroup/providers/
                               Microsoft.Network/networkInterfaces/myNIC}

Nach dem Ausführen von Update-AzureRmVM sollte Ihnen etwas ähnlich diesem Beispiel angezeigt werden:

    RequestId IsSuccessStatusCode StatusCode ReasonPhrase
    --------- ------------------- ---------- ------------
                             True         OK OK

### <a name="unmanaged-data-disk"></a>Nicht verwalteter Datenträger

```powershell
$vm = Get-AzureRmVM -ResourceGroupName $myResourceGroup -Name $myVM
Add-AzureRmVMDataDisk -VM $vm -Name "myDataDisk1" -VhdUri "https://mystore1.blob.core.windows.net/vhds/myDataDisk1.vhd" -LUN 0 -Caching ReadWrite -DiskSizeinGB 1 -CreateOption Empty
Update-AzureRmVM -ResourceGroupName $myResourceGroup -VM $vm
```

Nach dem Ausführen von Add-AzureRmVMDataDisk sollte Ihnen etwas ähnlich diesem Beispiel angezeigt werden:

    ResourceGroupName        : myResourceGroup
    Id                       : /subscriptions/{subscription-id}/resourceGroups/myResourceGroup/providers/
                               Microsoft.Compute/virtualMachines/myVM
    VmId                     : ########-####-####-####-############
    Name                     : myVM
    Type                     : Microsoft.Compute/virtualMachines
    Location                 : centralus
    Tags                     : {}
    AvailabilitySetReference : {Id}
    DiagnosticsProfile       : {BootDiagnostics}
    HardwareProfile          : {VmSize}
    NetworkProfile           : {NetworkInterfaces}
    OSProfile                : {ComputerName, AdminUsername, WindowsConfiguration, Secrets}
    ProvisioningState        : Succeeded
    StorageProfile           : {ImageReference, OsDisk, DataDisks}
    DataDiskNames            : {myDataDisk1}
    NetworkInterfaceIDs      : {/subscriptions/{subscription-id}/resourceGroups/myResourceGroup/providers/
                               Microsoft.Network/networkInterfaces/myNIC}

Nach dem Ausführen von Update-AzureRmVM sollte Ihnen etwas ähnlich diesem Beispiel angezeigt werden:

    RequestId IsSuccessStatusCode StatusCode ReasonPhrase
    --------- ------------------- ---------- ------------
                             True         OK OK

## <a name="update-a-virtual-machine"></a>Aktualisieren eines virtuellen Computers

Dieses Beispiel zeigt, wie Sie die Größe des virtuellen Computers aktualisieren.

```powershell
$vm = Get-AzureRmVM -ResourceGroupName $myResourceGroup -Name $myVM
$vm.HardwareProfile.vmSize = "Standard_DS2_v2"
Update-AzureRmVM -ResourceGroupName $myResourceGroup -VM $vm
```

Die Rückgabe sollte in etwa wie das folgende Beispiel aussehen:

    RequestId  IsSuccessStatusCode  StatusCode  ReasonPhrase
    ---------  -------------------  ----------  ------------
                              True          OK  OK

Eine Liste mit den verfügbaren Größen für einen virtuellen Computer finden Sie unter [Größen für virtuelle Computer in Azure](sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) .

## <a name="delete-a-virtual-machine"></a>Löschen eines virtuellen Computers

Löschen Sie den virtuellen Computer.  

```powershell
Remove-AzureRmVM -ResourceGroupName $myResourceGroup –Name $myVM
```

> [!NOTE]
> Mit dem Parameter **- Force** können Sie die Bestätigungsaufforderung überspringen.
> 
> 

Wenn Sie den Parameter „-Force“ nicht verwendet haben, werden Sie aufgefordert, den Vorgang zu bestätigen:

    Virtual machine removal operation
    This cmdlet will remove the specified virtual machine. Do you want to continue?
    [Y] Yes [N] No [S] Suspend [?] Help (default is "Y"):

Die Rückgabe sollte in etwa wie das folgende Beispiel aussehen:

    RequestId  IsSuccessStatusCode  StatusCode  ReasonPhrase
    ---------  -------------------  ----------  ------------
                              True          OK  OK

## <a name="next-steps"></a>Nächste Schritte

- Falls bei der Bereitstellung Probleme aufgetreten sind, können Sie den Artikel [Beheben gängiger Azure-Bereitstellungsfehler mit Azure Resource Manager](../../resource-manager-common-deployment-errors.md) lesen.
- Erfahren Sie, wie Sie Azure PowerShell zum Erstellen eines virtuellen Computers verwenden können: [Erstellen einer Windows-VM mit dem Resource Manager und PowerShell](../virtual-machines-windows-ps-create.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
- Nutzen Sie die Vorteile der vorlagenbasierten Erstellung virtueller Computer, indem Sie sich die Informationen unter [Erstellen eines virtuellen Windows-Computers mit einer Resource Manager-Vorlage](ps-template.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)

