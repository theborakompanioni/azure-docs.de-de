---
title: Erstellen einer Windows-VM auf Grundlage einer speziellen VHD in Azure | Microsoft-Dokumentation
description: "Erstellen Sie eine neue Windows-VM, indem Sie einen speziellen verwalteten Datenträger als Betriebssystemdatenträger im Resource Manager-Bereitstellungsmodell anfügen."
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
ms.date: 06/29/2017
ms.author: cynthn
ms.translationtype: Human Translation
ms.sourcegitcommit: 6efa2cca46c2d8e4c00150ff964f8af02397ef99
ms.openlocfilehash: b315a37f72f34d45bb55c2bbe6be20cca8c42424
ms.contentlocale: de-de
ms.lasthandoff: 07/01/2017


---
# <a name="create-a-windows-vm-from-a-specialized-disk"></a>Erstellen einer Windows-VM von einem speziellen Datenträger

Erstellen Sie eine neue VM, indem Sie mit PowerShell einen speziellen nicht verwalteten Datenträger als Betriebssystemdatenträger anfügen. Ein spezieller Datenträger ist eine Kopie einer virtuellen Festplatte (VHD) einer bereits vorhandenen VM, bei der die Benutzerkonten, Anwendungen und andere Statusdaten Ihrer ursprünglichen VM beibehalten werden. 

Wenn Sie zur Erstellung einer neuen VM eine spezielle VHD verwenden, behält die neue VM den Computernamen der ursprünglichen VM bei. Andere computerspezifische Informationen werden ebenfalls beibehalten. In einigen Fällen können diese doppelten Informationen Probleme verursachen. Achten Sie beim Kopieren einer VM darauf, auf welche Arten von computerspezifischen Informationen Ihre Anwendungen angewiesen sind.

Sie haben zwei Möglichkeiten:
* [Hochladen einer VHD-Datei](#option-1-upload-a-specialized-vhd)
* [Kopieren einer vorhandenen Azure-VM](#option-2-copy-an-existing-azure-vm)

In diesem Thema wird gezeigt, wie verwaltete Datenträger verwendet werden. Wenn Sie über eine Legacybereitstellung verfügen, die ein Speicherkonto erfordert, finden Sie unter [Erstellen einer VM auf Grundlage einer speziellen VHD in einem Speicherkonto](sa-create-vm-specialized.md) weitere Informationen dazu.

## <a name="before-you-begin"></a>Voraussetzungen
Wenn Sie PowerShell verwenden, vergewissern Sie sich, dass Sie die neueste Version des AzureRM.Compute-PowerShell-Moduls verwenden. 

```powershell
Install-Module AzureRM.Compute -RequiredVersion 2.6.0
```
Weitere Informationen finden Sie unter [Azure PowerShell-Versionsverwaltung](/powershell/azure/overview).


## <a name="option-1-upload-a-specialized-vhd"></a>Option 1: Hochladen einer speziellen VHD

Sie können die VHD über eine spezielle VM hochladen, die mit einem lokalen Virtualisierungstool (z.B. Hyper-V) erstellt wurde, oder über eine VM, die aus einer anderen Cloud exportiert wurde.

### <a name="prepare-the-vm"></a>Vorbereiten des virtuellen Computers
Wenn Sie die VHD unverändert zum Erstellen eines neuen virtuellen Computers verwenden möchten, führen Sie auf jeden Fall die folgenden Schritte aus. 
  
  * [Vorbereiten einer Windows-VHD für das Hochladen in Azure.](prepare-for-upload-vhd-image.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) Generalisieren Sie den virtuellen Computer **nicht** mithilfe von Sysprep.
  * Entfernen Sie alle auf der VM installierten Gastvirtualisierungstools und Agents (z.B. VMware-Tools).
  * Stellen Sie sicher, dass der virtuelle Computer so konfiguriert ist, dass die zugehörigen IP-Adressen und DNS-Einstellungen per Pullvorgang über DHCP übertragen werden. Dadurch wird sichergestellt, dass der Server beim Starten eine IP-Adresse innerhalb des VNET bezieht. 


### <a name="get-the-storage-account"></a>Abrufen des Speicherkontos
Sie benötigen ein Speicherkonto in Azure, in dem die hochgeladene VHD gespeichert wird. Sie können ein vorhandenes Speicherkonto auswählen oder ein neues erstellen. 

Geben Sie zum Anzeigen der verfügbaren Speicherkonten Folgendes ein:

```powershell
Get-AzureRmStorageAccount
```

Wenn Sie ein vorhandenes Speicherkonto verwenden möchten, fahren Sie mit dem Abschnitt [Hochladen der VHD](#upload-the-vhd-to-your-storage-account) fort.

Wenn Sie ein neues Speicherkonto erstellen möchten, gehen Sie wie folgt vor:

1. Sie benötigen den Namen der Ressourcengruppe, in der das Speicherkonto erstellt werden soll. Geben Sie den folgenden Befehl ein, um alle in Ihrem Abonnement enthaltenen Ressourcengruppen zu ermitteln:
   
    ```powershell
    Get-AzureRmResourceGroup
    ```

    Zum Erstellen einer Ressourcengruppe mit dem Namen *myResourceGroup* in der Region *USA, Westen* geben Sie den folgenden Befehl ein:

    ```powershell
    New-AzureRmResourceGroup -Name myResourceGroup -Location "West US"
    ```

2. Erstellen Sie mit dem Cmdlet [New-AzureRmStorageAccount](/powershell/module/azurerm.storage/new-azurermstorageaccount) das Speicherkonto *mystorageaccount* in dieser Ressourcengruppe:
   
    ```powershell
    New-AzureRmStorageAccount -ResourceGroupName myResourceGroup -Name mystorageaccount -Location "West US" `
        -SkuName "Standard_LRS" -Kind "Storage"
    ```

### <a name="upload-the-vhd-to-your-storage-account"></a>Hochladen der VHD in Ihr Speicherkonto 
Verwenden Sie das Cmdlet [Add-AzureRmVhd](/powershell/module/azurerm.compute/add-azurermvhd), um die VHD in einen Container in Ihrem Speicherkonto hochzuladen. In diesem Beispiel wird die Datei *myVHD.vhd* aus `"C:\Users\Public\Documents\Virtual hard disks\"` in das Speicherkonto *mystorageaccount* in der Ressourcengruppe *myResourceGroup* hochgeladen. Die Datei wird im Container *mycontainer* gespeichert. Der neue Dateiname lautet *myUploadedVHD.vhd*.

```powershell
$resourceGroupName = "myResourceGroup"
$urlOfUploadedVhd = "https://mystorageaccount.blob.core.windows.net/mycontainer/myUploadedVHD.vhd"
Add-AzureRmVhd -ResourceGroupName $resourceGroupName -Destination $urlOfUploadedVhd `
    -LocalFilePath "C:\Users\Public\Documents\Virtual hard disks\myVHD.vhd"
```


Bei erfolgreicher Ausführung erhalten Sie eine Antwort, die etwa wie folgt aussieht:

```powershell
MD5 hash is being calculated for the file C:\Users\Public\Documents\Virtual hard disks\myVHD.vhd.
MD5 hash calculation is completed.
Elapsed time for the operation: 00:03:35
Creating new page blob of size 53687091712...
Elapsed time for upload: 01:12:49

LocalFilePath           DestinationUri
-------------           --------------
C:\Users\Public\Doc...  https://mystorageaccount.blob.core.windows.net/mycontainer/myUploadedVHD.vhd
```

Abhängig von Ihrer Netzwerkverbindung und der Größe Ihrer VHD-Datei kann die Ausführung dieses Befehls einige Zeit in Anspruch nehmen.

### <a name="create-a-managed-disk-from-the-vhd"></a>Erstellen verwalteter Datenträger aus der VHD

Erstellen Sie mithilfe von [New-AzureRMDisk](/powershell/module/azurerm.compute/new-azurermdisk) einen verwalteten Datenträger auf Grundlage der speziellen VHD in Ihrem Speicherkonto. In diesem Beispiel wird **myOSDisk1** als Name des Datenträgers verwendet, der Datenträger im *StandardLRS*-Speicher gespeichert und *https://storageaccount.blob.core.windows.net/vhdcontainer/osdisk.vhd* als URI für die Quell-VHD verwendet.

Erstellen Sie eine neue Ressourcengruppe für die neue VM.

```powershell
$destinationResourceGroup = 'myDestinationResourceGroup'
New-AzureRmResourceGroup -Location $location -Name $destinationResourceGroup
```

Erstellen Sie den neuen Betriebssystemdatenträger der hochgeladenen VHD. 

```powershell
$sourceUri = https://storageaccount.blob.core.windows.net/vhdcontainer/osdisk.vhd)
$osDiskName = 'myOsDisk'
$osDisk = New-AzureRmDisk -DiskName $osDiskName -Disk `
    (New-AzureRmDiskConfig -AccountType StandardLRS  -Location $location -CreateOption Import `
    -SourceUri $sourceUri) `
    -ResourceGroupName $destinationResourceGroup
```

## <a name="option-2-copy-an-existing-azure-vm"></a>Option 2: Kopieren einer vorhandenen Azure-VM

Sie können eine Kopie einer VM mit verwalteten Datenträgern erstellen, indem Sie eine Momentaufnahme von der VM erstellen und damit einen neuen verwalteten Datenträger und eine neue VM erstellen.


### <a name="take-a-snapshot-of-the-os-disk"></a>Erstellen einer Momentaufnahme des Betriebssystemdatenträgers

Sie können eine Momentaufnahme der gesamten VM (einschließlich aller Datenträger) oder nur von einem einzelnen Datenträger erstellen. Die folgenden Schritte zeigen, wie Sie mit dem Cmdlet [New-AzureRmSnapshot](/powershell/module/azurerm.compute/new-azurermsnapshot) eine Momentaufnahme von lediglich dem Betriebssystemdatenträger Ihrer VM erstellen. 

Legen Sie einige Parameter fest. 

 ```powershell
$resourceGroupName = 'myResourceGroup' 
$vmName = 'myVM'
$location = 'westus' 
$snapshotName = 'mySnapshot'  
```

Rufen Sie das VM-Objekt ab.

```powershell
$vm = Get-AzureRmVM -Name $vmName -ResourceGroupName $resourceGroupName
```
Rufen Sie den Namen des Betriebssystemdatenträgers ab.

 ```powershell
$disk = Get-AzureRmDisk -ResourceGroupName $resourceGroupName -DiskName $vm.StorageProfile.OsDisk.Name
```

Erstellen Sie die Momentaufnahmekonfiguration. 

 ```powershell
$snapshotConfig =  New-AzureRmSnapshotConfig -SourceUri $disk.Id -OsType Windows -CreateOption Copy -Location $location 
```

Erstellen Sie die Momentaufnahme.

```powershell
$snapShot = New-AzureRmSnapshot -Snapshot $snapshotConfig -SnapshotName $snapshotName -ResourceGroupName $resourceGroupName
```


Wenn Sie vorhaben, die Momentaufnahme zum Erstellen eine auf hohe Leistung ausgelegte VM zu nutzen, verwenden Sie den Parameter `-AccountType Premium_LRS` mit dem Befehl „New-AzureRmSnapshot“. Der Parameter erstellt die Momentaufnahme so, dass sie als verwalteter Premium-Datenträger gespeichert wird. Verwaltete Premium-Datenträger sind teurer als die Standardvariante. Vergewissern Sie sich daher, dass Sie Premium tatsächlich benötigen, ehe Sie diesen Parameter verwenden.

### <a name="create-a-new-disk-from-the-snapshot"></a>Erstellen eines neuen Datenträgers über die Momentaufnahme

Erstellen Sie mit [New-AzureRMDisk](/powershell/module/azurerm.compute/new-azurermdisk) einen verwalteten Datenträger über die Momentaufnahme. Dieses Beispiel verwendet *myOSDisk* für den Namen des Datenträgers an.

Erstellen Sie eine neue Ressourcengruppe für die neue VM.

```powershell
$destinationResourceGroup = 'myDestinationResourceGroup'
New-AzureRmResourceGroup -Location $location -Name $destinationResourceGroup
```

Legen Sie den Namen des Betriebssystemdatenträgers fest. 

```powershell
$osDiskName = 'myOsDisk'
```

Erstellen Sie den verwalteten Datenträger.

```powershell
$osDisk = New-AzureRmDisk -DiskName $osDiskName -Disk `
    (New-AzureRmDiskConfig  -Location $location -CreateOption Copy `
    -SourceResourceId $snapshot.Id) `
    -ResourceGroupName $destinationResourceGroup
```


## <a name="create-the-new-vm"></a>Erstellen des neuen virtuellen Computers 

Erstellen Sie Netzwerkressourcen und andere VM-Ressourcen für die neue VM.

### <a name="create-the-subnet-and-vnet"></a>Erstellen des Subnetzes und des virtuellen Netzwerks

Erstellen Sie das vNet und das Subnetz des [virtuellen Netzwerks](../../virtual-network/virtual-networks-overview.md).

Erstellen Sie das Subnetz. In diesem Beispiel wird ein Subnetz mit dem Namen **mySubNet** in der Ressourcengruppe **myDestinationResourceGroup** erstellt und das Adresspräfix des Subnetzes auf **10.0.0.0/24** festgelegt.
   
```powershell
$subnetName = 'mySubNet'
$singleSubnet = New-AzureRmVirtualNetworkSubnetConfig -Name $subnetName -AddressPrefix 10.0.0.0/24
```

Erstellen Sie das virtuelle Netzwerk. Dieses Beispiel legt den Namen des virtuellen Netzwerks auf **myVnetName**, den Standort auf **USA, Westen** und das Adresspräfix für das virtuelle Netzwerk auf **10.0.0.0/16** fest. 
   
```powershell
$vnetName = "myVnetName"
$vnet = New-AzureRmVirtualNetwork -Name $vnetName -ResourceGroupName $destinationResourceGroup -Location $location `
    -AddressPrefix 10.0.0.0/16 -Subnet $singleSubnet
```    


### <a name="create-the-network-security-group-and-an-rdp-rule"></a>Erstellen einer Netzwerksicherheitsgruppe und einer RDP-Regel
Damit Sie sich mit RDP an Ihrem virtuellen Computer anmelden können, benötigen Sie eine Sicherheitsregel, die RDP-Zugriff auf Port 3389 zulässt. Da die VHD für die neue VM aus einer vorhandenen spezialisierten VM erstellt wurde, können Sie für das RDP ein Konto des virtuellen Quellcomputers verwenden.

In diesem Beispiel wird der Name der Netzwerksicherheitsgruppe auf **myNsg** und der Name der RDP-Regel auf **myRdpRule** festgelegt.

```powershell
$nsgName = "myNsg"

$rdpRule = New-AzureRmNetworkSecurityRuleConfig -Name myRdpRule -Description "Allow RDP" `
    -Access Allow -Protocol Tcp -Direction Inbound -Priority 110 `
    -SourceAddressPrefix Internet -SourcePortRange * `
    -DestinationAddressPrefix * -DestinationPortRange 3389
$nsg = New-AzureRmNetworkSecurityGroup -ResourceGroupName $destinationResourceGroup -Location $location `
    -Name $nsgName -SecurityRules $rdpRule
    
```

Weitere Informationen zu Endpunkten und Netzwerksicherheitsgruppen finden Sie unter [Öffnen von Ports für einen virtuellen Computer in Azure mithilfe von PowerShell](nsg-quickstart-powershell.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

### <a name="create-a-public-ip-address-and-nic"></a>Erstellen einer öffentlichen IP-Adresse und einer NIC
Sie benötigen eine [öffentliche IP-Adresse](../../virtual-network/virtual-network-ip-addresses-overview-arm.md) und eine Netzwerkschnittstelle, um die Kommunikation mit dem virtuellen Computer im virtuellen Netzwerk zu ermöglichen.

Erstellen Sie die öffentliche IP-Adresse. In diesem Beispiel ist der Name der öffentlichen IP-Adresse auf **myIP** festgelegt.
   
```powershell
$ipName = "myIP"
$pip = New-AzureRmPublicIpAddress -Name $ipName -ResourceGroupName $destinationResourceGroup -Location $location `
   -AllocationMethod Dynamic
```       

Erstellen Sie die NIC. In diesem Beispiel ist der Name der NIC auf **myNicName** festgelegt.
   
```powershell
$nicName = "myNicName"
$nic = New-AzureRmNetworkInterface -Name $nicName -ResourceGroupName $destinationResourceGroup `
    -Location $location -SubnetId $vnet.Subnets[0].Id -PublicIpAddressId $pip.Id -NetworkSecurityGroupId $nsg.Id
```



### <a name="set-the-vm-name-and-size"></a>Festlegen von Name und Größe des virtuellen Computers

In diesem Beispiel wird der VM-Name auf *myVM* und die VM-Größe auf *Standard_A2* festgelegt.

```powershell
$vmName = "myVM"
$vmConfig = New-AzureRmVMConfig -VMName $vmName -VMSize "Standard_A2"
```

### <a name="add-the-nic"></a>Hinzufügen der Netzwerkkarte
    
```powershell
$vm = Add-AzureRmVMNetworkInterface -VM $vmConfig -Id $nic.Id
```
    

### <a name="add-the-os-disk"></a>Hinzufügen des Betriebssystemdatenträgers 

Fügen Sie den Betriebssystemdatenträger mit [Set-AzureRmVMOSDisk](/powershell/module/azurerm.compute/set-azurermvmosdisk) zur Konfiguration hinzu. In diesem Beispiel wird die Größe des Datenträgers auf *128 GB* festgelegt und der verwaltete Datenträger als *Windows*-Betriebssystem-Datenträger angefügt.
 
```powershell
$vm = Set-AzureRmVMOSDisk -VM $vm -ManagedDiskId $osDisk.Id -StorageAccountType StandardLRS `
    -DiskSizeInGB 128 -CreateOption Attach -Windows
```

### <a name="complete-the-vm"></a>Fertigstellen der VM 

Erstellen Sie die VM anhand von [New-AzureRMVM](/powershell/module/azurerm.compute/new-azurermvm) mithilfe der soeben erstellten Konfigurationen.

```powershell
New-AzureRmVM -ResourceGroupName $destinationResourceGroup -Location $location -VM $vm
```

War dieser Befehl erfolgreich, erscheint eine Ausgabe wie die folgende:

```powershell
RequestId IsSuccessStatusCode StatusCode ReasonPhrase
--------- ------------------- ---------- ------------
                         True         OK OK   

```

### <a name="verify-that-the-vm-was-created"></a>Sicherstellen, dass der virtuelle Computer erstellt wurde
Der neu erstellte virtuelle Computer müsste nun im [Azure-Portal](https://portal.azure.com) unter **Durchsuchen** > **Virtuelle Computer** angezeigt werden. Alternativ dazu können Sie ihn mit den folgenden PowerShell-Befehlen anzeigen:

```powershell
$vmList = Get-AzureRmVM -ResourceGroupName $destinationResourceGroup
$vmList.Name
```

## <a name="next-steps"></a>Nächste Schritte
Melden Sie sich beim neuen virtuellen Computer an, wechseln Sie zum virtuellen Computer im [Portal](https://portal.azure.com), klicken Sie auf **Verbinden**, und öffnen Sie die Remotedesktop-RDP-Datei. Verwenden Sie die Kontoanmeldeinformationen des ursprünglichen virtuellen Computers für die Anmeldung bei Ihrem neuen virtuellen Computer. Anweisungen dazu finden Sie unter [Herstellen einer Verbindung mit einem virtuellen Azure-Computer unter Windows und Anmelden bei diesem Computer](connect-logon.md).


