---
title: "Erstellen einer VM auf der Grundlage eines spezialisierten Datenträgers in Azure | Microsoft-Dokumentation"
description: "Erstellen Sie einen neuen virtuellen Computer, indem Sie einen spezialisierten verwalteten oder nicht verwalteten Datenträger im Resource Manager-Bereitstellungsmodell anfügen."
services: virtual-machines-windows
documentationcenter: 
author: cynthn
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 3b7d3cd5-e3d7-4041-a2a7-0290447458ea
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 02/06/2017
ms.author: cynthn
translationtype: Human Translation
ms.sourcegitcommit: 197ebd6e37066cb4463d540284ec3f3b074d95e1
ms.openlocfilehash: e9c9f440aa6554e30dc0974d1ab777a9bca50762
ms.lasthandoff: 03/31/2017


---
# <a name="create-a-vm-from-a-specialized-disk"></a>Erstellen eines virtuellen Computers von einem speziellen Datenträger

Erstellen Sie einen neuen virtuellen Computer, indem Sie mit PowerShell einen spezialisierten Datenträger als Betriebssystem-Datenträger anfügen. Ein spezialisierter Datenträger ist eine Kopie einer VHD einer bestehenden VM, auf der die Benutzerkonten, Anwendungen und andere Statusdaten Ihres ursprünglichen virtuellen Computers beibehalten werden. Sie können entweder einen spezialisierten [verwalteten Datenträger](../../storage/storage-managed-disks-overview.md) oder einen spezialisierten nicht verwalteten Datenträger zum Erstellen des neuen virtuellen Computers verwenden.

## <a name="before-you-begin"></a>Voraussetzungen
Wenn Sie PowerShell verwenden, vergewissern Sie sich, dass Sie die neueste Version des AzureRM.Compute-PowerShell-Moduls verwenden. Führen Sie den folgenden Befehl aus, um es zu installieren.

```powershell
Install-Module AzureRM.Compute -RequiredVersion 2.6.0
```
Weitere Informationen finden Sie unter [Azure PowerShell-Versionsverwaltung](https://docs.microsoft.com/powershell/azureps-cmdlets-docs/#azure-powershell-versioning).


## <a name="create-the-subnet-and-vnet"></a>Erstellen des Subnetzes und des virtuellen Netzwerks

Erstellen Sie das vNet und das Subnetz des [virtuellen Netzwerks](../../virtual-network/virtual-networks-overview.md).

1. Erstellen Sie das Subnetz. Dieses Beispiel erstellt ein Subnetz mit dem Namen **mySubNet** in der Ressourcengruppe **myResourceGroup** und legt das Adresspräfix **10.0.0.0/24** fest.
   
    ```powershell
    $rgName = "myResourceGroup"
    $subnetName = "mySubNet"
    $singleSubnet = New-AzureRmVirtualNetworkSubnetConfig -Name $subnetName -AddressPrefix 10.0.0.0/24
    ```
2. Erstellen Sie das virtuelle Netzwerk. Dieses Beispiel legt den Namen des virtuellen Netzwerks auf **myVnetName**, den Standort auf **USA, Westen** und das Adresspräfix für das virtuelle Netzwerk auf **10.0.0.0/16** fest. 
   
    ```powershell
    $location = "West US"
    $vnetName = "myVnetName"
    $vnet = New-AzureRmVirtualNetwork -Name $vnetName -ResourceGroupName $rgName -Location $location `
        -AddressPrefix 10.0.0.0/16 -Subnet $singleSubnet
    ```    

## <a name="create-a-public-ip-address-and-nic"></a>Erstellen einer öffentlichen IP-Adresse und einer NIC
Sie benötigen eine [öffentliche IP-Adresse](../../virtual-network/virtual-network-ip-addresses-overview-arm.md) und eine Netzwerkschnittstelle, um die Kommunikation mit dem virtuellen Computer im virtuellen Netzwerk zu ermöglichen.

1. Erstellen Sie die öffentliche IP-Adresse. In diesem Beispiel ist der Name der öffentlichen IP-Adresse auf **myIP** festgelegt.
   
    ```powershell
    $ipName = "myIP"
    $pip = New-AzureRmPublicIpAddress -Name $ipName -ResourceGroupName $rgName -Location $location `
        -AllocationMethod Dynamic
    ```       
2. Erstellen Sie die NIC. In diesem Beispiel ist der Name der NIC auf **myNicName** festgelegt.
   
    ```powershell
    $nicName = "myNicName"
    $nic = New-AzureRmNetworkInterface -Name $nicName -ResourceGroupName $rgName `
    -Location $location -SubnetId $vnet.Subnets[0].Id -PublicIpAddressId $pip.Id
    ```

## <a name="create-the-network-security-group-and-an-rdp-rule"></a>Erstellen einer Netzwerksicherheitsgruppe und einer RDP-Regel
Damit Sie sich über RDP bei Ihrem virtuellen Computer anmelden können, benötigen Sie eine Sicherheitsregel, die RDP-Zugriff auf Port 3389 zulässt. Da die VHD für den neuen virtuellen Computer aus einer vorhandenen spezialisierten VM erstellt wurde, können Sie nach dem Erstellen des virtuellen Computers ein vorhandenes Konto des virtuellen Quellcomputers verwenden, der über die Berechtigung zum Anmelden über RDP verfügte.
In diesem Beispiel wird der Name der Netzwerksicherheitsgruppe auf **myNsg** und der Name der RDP-Regel auf **myRdpRule** festgelegt.

```powershell
$nsgName = "myNsg"

$rdpRule = New-AzureRmNetworkSecurityRuleConfig -Name myRdpRule -Description "Allow RDP" `
    -Access Allow -Protocol Tcp -Direction Inbound -Priority 110 `
    -SourceAddressPrefix Internet -SourcePortRange * `
    -DestinationAddressPrefix * -DestinationPortRange 3389
$nsg = New-AzureRmNetworkSecurityGroup -ResourceGroupName $rgName -Location $location `
    -Name $nsgName -SecurityRules $rdpRule
    
```

Weitere Informationen zu Endpunkten und Netzwerksicherheitsgruppen finden Sie unter [Öffnen von Ports für einen virtuellen Computer in Azure mithilfe von PowerShell](nsg-quickstart-powershell.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

## <a name="set-the-vm-name-and-size"></a>Festlegen von Name und Größe des virtuellen Computers

In diesem Beispiel wird der Name des virtuellen Computers auf „myVM“ und ihre Größe auf „Standard_A2“ festgelegt.
```powershell
$vmName = "myVM"
$vmConfig = New-AzureRmVMConfig -VMName $vmName -VMSize "Standard_A2"
```

## <a name="add-the-nic"></a>Hinzufügen der Netzwerkkarte
    
```powershell
$vm = Add-AzureRmVMNetworkInterface -VM $vmConfig -Id $nic.Id
```
    
    
## <a name="configure-the-os-disk"></a>Konfigurieren des Betriebssystem-Datenträgers

Der spezialisierte Betriebssystem-Datenträger könnte eine virtuelle Festplatte sein, die Sie [in Azure hochgeladen](upload-image.md) haben, oder eine [Kopie der VHD einer vorhandenen Azure-VM](vhd-copy.md). 

Sie können eine dieser beiden Optionen wählen:
- **Option 1:** Erstellen Sie einen spezialisierten verwalteten Datenträger auf der Grundlage einer spezialisierten VHD in einem vorhandenen Speicherkonto zur Verwendung als Betriebssystem-Datenträger.

oder 

- **Option 2:** Verwenden Sie eine spezialisierte virtuelle Festplatte, die in Ihrem eigenen Speicherkonto gespeichert ist (ein nicht verwalteter Datenträger). 

### <a name="option-1-create-a-managed-disk-from-an-unmanaged-specialized-disk"></a>Option 1: Erstellen eines verwalteten Datenträgers auf der Grundlage eines nicht verwalteten spezialisierten Datenträgers

1. Erstellen Sie einen verwalteten Datenträger auf der Grundlage der vorhandenen spezialisierten VHD in Ihrem Speicherkonto. In diesem Beispiel wird **myOSDisk1** als Name des Datenträgers verwendet, der Datenträger im **StandardLRS**-Speicher gespeichert und **https://storageaccount.blob.core.windows.net/vhdcontainer/osdisk.vh.vhd** als URI für die Quell-VHD verwendet.

    ```powershell
    $osDisk = New-AzureRmDisk -DiskName "myOSDisk1" -Disk (New-AzureRmDiskConfig `
    -AccountType StandardLRS  -Location $location -CreationDataCreateOption Import `
    -SourceUri https://storageaccount.blob.core.windows.net/vhdcontainer/osdisk.vh.vhd) `
    -ResourceGroupName $rgName
    ```

2. Fügen Sie den Betriebssystem-Datenträger der Konfiguration hinzu. In diesem Beispiel wird die Größe des Datenträgers auf **128 GB** festgelegt und der verwaltete Datenträger als **Windows**-Betriebssystem-Datenträger angefügt.
    
    ```powershell
    $vm = Set-AzureRmVMOSDisk -VM $vm -ManagedDiskId $osDisk.Id -StorageAccountType StandardLRS `
    -DiskSizeInGB 128 -CreateOption Attach -Windows
    ```

Optional: Fügen Sie zusätzliche verwaltete Datenträger als Datenträger für Daten an. Diese Option setzt voraus, dass Sie Ihre verwalteten Datenträger für Daten mit [Verwaltete Datenträger für Daten erstellen](create-managed-disk-ps.md) erstellt haben. 

```powershell
$vm = Add-AzureRmVMDataDisk -VM $VirtualMachine -Name $dataDiskName -CreateOption Attach -ManagedDiskId $dataDisk1.Id -Lun 1
```


### <a name="option-2-attach-a-vhd-that-is-in-an-existing-storage-account"></a>Option 2: Anfügen einer VHD, die in einem vorhandenen Speicherkonto gespeichert ist

1. Legen Sie den URI für die VHD fest, die Sie verwenden möchten. In diesem Beispiel wird die VHD-Datei mit dem Namen **myOsDisk.vhd** in einem Speicherkonto namens **myStorageAccount** in einem Container mit dem Namen **myContainer** gespeichert.

    ```powershell
    $osDiskUri = "https://myStorageAccount.blob.core.windows.net/myContainer/myOsDisk.vhd"
    ```
2. Fügen Sie den Betriebssystem-Datenträger mithilfe der URL der kopierten Betriebssystem-VHD hinzu. In diesem Beispiel wird beim Erstellen des Betriebssystem-Datenträgers der Begriff „osDisk“ dem Namen des virtuellen Computers angefügt, um den Namen des Betriebssystem-Datenträgers zu erstellen. In diesem Beispiel wird auch angegeben, dass diese Windows-basierte VHD dem virtuellen Computer als Betriebssystem-Datenträger angefügt werden soll.
    
    ```powershell
    $osDiskName = $vmName + "osDisk"
    $vm = Set-AzureRmVMOSDisk -VM $vm -Name $osDiskName -VhdUri $osDiskUri -CreateOption attach -Windows
    ```

Optional: Wenn Sie über Datenträger für Daten verfügen, die dem virtuellen Computer angefügt werden müssen, fügen Sie die Datenträger für Daten mit den URLs von Daten-VHDs und den entsprechenden logischen Gerätenummern (Logical Unit Number, LUN) hinzu.

```powershell
$dataDiskName = $vmName + "dataDisk"
$vm = Add-AzureRmVMDataDisk -VM $vm -Name $dataDiskName -VhdUri $dataDiskUri -Lun 1 -CreateOption attach
```

Bei Verwendung eines Speicherkontos sehen die URLs der Daten- und Betriebssystem-Datenträger wie folgt aus: `https://StorageAccountName.blob.core.windows.net/BlobContainerName/DiskName.vhd`. Sie finden dies im Portal, indem Sie zum Zielspeichercontainer wechseln, auf die kopierte Betriebssystem- oder Daten-VHD klicken und dann den Inhalt der URL kopieren.


## <a name="create-the-vm"></a>Erstellen des virtuellen Computers

Erstellen Sie den virtuellen Computer mithilfe der soeben erstellten Konfigurationen.

```powershell
#Create the new VM
New-AzureRmVM -ResourceGroupName $rgName -Location $location -VM $vm
```

War dieser Befehl erfolgreich, erscheint eine Ausgabe wie die folgende:

```powershell
RequestId IsSuccessStatusCode StatusCode ReasonPhrase
--------- ------------------- ---------- ------------
                         True         OK OK   

```

## <a name="verify-that-the-vm-was-created"></a>Sicherstellen, dass der virtuelle Computer erstellt wurde
Der neu erstellte virtuelle Computer müsste nun im [Azure-Portal](https://portal.azure.com) unter **Durchsuchen** > **Virtuelle Computer** angezeigt werden. Alternativ dazu können Sie ihn mit den folgenden PowerShell-Befehlen anzeigen:

```powershell
$vmList = Get-AzureRmVM -ResourceGroupName $rgName
$vmList.Name
```

## <a name="next-steps"></a>Nächste Schritte
Melden Sie sich beim neuen virtuellen Computer an, wechseln Sie zum virtuellen Computer im [Portal](https://portal.azure.com), klicken Sie auf **Verbinden**, und öffnen Sie die Remotedesktop-RDP-Datei. Verwenden Sie die Kontoanmeldeinformationen des ursprünglichen virtuellen Computers für die Anmeldung bei Ihrem neuen virtuellen Computer. Anweisungen dazu finden Sie unter [Herstellen einer Verbindung mit einem virtuellen Azure-Computer unter Windows und Anmelden bei diesem Computer](connect-logon.md).


