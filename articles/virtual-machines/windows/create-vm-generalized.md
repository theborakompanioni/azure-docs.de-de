---
title: Erstellen eines virtuellen Computers aus einer generalisierten VHD | Microsoft Docs
description: Erfahren Sie, wie Sie einen virtuellen Windows-Computer aus einem generalisierten VHD-Image in einem Speicherkonto mit Azure PowerShell erstellen.
services: virtual-machines-windows
documentationcenter: 
author: cynthn
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: b4808871-9ef1-49ea-a617-9154d417abb0
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 03/21/2017
ms.author: cynthn
translationtype: Human Translation
ms.sourcegitcommit: 197ebd6e37066cb4463d540284ec3f3b074d95e1
ms.openlocfilehash: 5cad7bf8030bd60c6adc5fb0c9ad39a2d703face
ms.lasthandoff: 03/31/2017


---
# <a name="create-a-vm-from-a-generalized-vhd-image-in-a-storage-account"></a>Erstellen eines virtuellen Computers aus einem generalisierten VHD-Image in einem Speicherkonto 

In diesem Thema wird das Erstellen eines virtuellen Computers aus einem generalisierten, nicht verwalteten Datenträger in einem Speicherkonto beschrieben. Auf einem generalisierten VHD-Image wurden alle Ihre persönlichen Kontoinformationen mit [Sysprep](generalize-vhd.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) entfernt. Sie können eine generalisierte virtuelle Festplatte erstellen, indem Sie Sysprep auf einem lokalen virtuellen Computer ausführen, dann [die VHD in Azure hochladen](upload-image.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) oder Sysprep auf einem virtuellen Azure-VM ausführen und dann [die VHD kopieren](vhd-copy.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

Informationen darüber, wie Sie einen virtuellen Computer aus einer speziellen VHD in einem Speicherkonto erstellen, finden Sie unter [Erstellen eines virtuellen Computers aus einer speziellen VHD](create-vm-specialized.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

Informationen zum Verwenden von Managed Disks anstelle von Datenträgern in einem Speicherkonto finden Sie unter [Erstellen eines verwalteten VM-Images](capture-image-resource.md) und [Erstellen eines virtuellen Computers aus einem verwalteten Image](create-vm-generalized-managed.md).

## <a name="prerequisites"></a>Voraussetzungen
Wenn Sie eine von einem lokalen virtuellen Computer hochgeladene VHD verwenden möchten, z. B. eine mit Hyper-V erstellte, stellen Sie sicher, dass Sie den Anweisungen [Vorbereiten einer Windows-VHD für das Hochladen in Azure](prepare-for-upload-vhd-image.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) folgen. 

Hochgeladene VHDs und vorhandene Azure-VM-VHDs müssen generalisiert werden, bevor Sie mit dieser Methode einen virtuellen Computer erstellen können. Weitere Informationen finden Sie unter [Generalize a Windows virtual machine using Sysprep (Generalisieren eines virtuellen Windows-Computers mit Sysprep)](generalize-vhd.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). 

## <a name="set-the-uri-of-the-vhd"></a>Festlegen des URI der virtuellen Festplatte

Der URI für die zu verwendende virtuelle Festplatte liegt in folgendem Format vor: https://**mystorageaccount**.blob.core.windows.net/**mycontainer**/**MyVhdName**.vhd. In diesem Beispiel befindet sich die virtuelle Festplatte mit dem Namen **myVHD** im Speicherkonto **mystorageaccount** im Container **mycontainer**.

```powershell
$imageURI = "https://mystorageaccount.blob.core.windows.net/mycontainer/myVhd.vhd"
```


## <a name="create-a-virtual-network"></a>Erstellen eines virtuellen Netzwerks
Erstellen Sie das VNet und das Subnetz des [virtuellen Netzwerks](../../virtual-network/virtual-networks-overview.md).

1. Erstellen Sie das Subnetz. Das folgende Beispiel erstellt ein Subnetz mit dem Namen **mySubnet** in der Ressourcengruppe **myResourceGroup** mit dem Adresspräfix **10.0.0.0/24**.  
   
    ```powershell
    $rgName = "myResourceGroup"
    $subnetName = "mySubnet"
    $singleSubnet = New-AzureRmVirtualNetworkSubnetConfig -Name $subnetName -AddressPrefix 10.0.0.0/24
    ```
2. Erstellen des virtuellen Netzwerks Das folgende Beispiel erstellt ein virtuelles Netzwerk mit dem Namen **myVnet** im Speicherort **USA, Westen** mit dem Adresspräfix **10.0.0.0/16**.  
   
    ```powershell
    $location = "West US"
    $vnetName = "myVnet"
    $vnet = New-AzureRmVirtualNetwork -Name $vnetName -ResourceGroupName $rgName -Location $location `
        -AddressPrefix 10.0.0.0/16 -Subnet $singleSubnet
    ```    

## <a name="create-a-public-ip-address-and-network-interface"></a>Erstellen einer öffentlichen IP-Adresse und einer Netzwerkschnittstelle
Sie benötigen eine [öffentliche IP-Adresse](../../virtual-network/virtual-network-ip-addresses-overview-arm.md) und eine Netzwerkschnittstelle, um die Kommunikation mit dem virtuellen Computer im virtuellen Netzwerk zu ermöglichen.

1. Erstellen einer öffentlichen IP-Adresse Dieses Beispiel erstellt eine öffentliche IP-Adresse mit dem Namen **myPip**. 
   
    ```powershell
    $ipName = "myPip"
    $pip = New-AzureRmPublicIpAddress -Name $ipName -ResourceGroupName $rgName -Location $location `
        -AllocationMethod Dynamic
    ```       
2. Erstellen der NIC Dieses Beispiel erstellt eine NIC mit dem Namen **myNic**. 
   
    ```powershell
    $nicName = "myNic"
    $nic = New-AzureRmNetworkInterface -Name $nicName -ResourceGroupName $rgName -Location $location `
        -SubnetId $vnet.Subnets[0].Id -PublicIpAddressId $pip.Id
    ```

## <a name="create-the-network-security-group-and-an-rdp-rule"></a>Erstellen einer Netzwerksicherheitsgruppe und einer RDP-Regel
Damit Sie sich mit RDP an Ihrem virtuellen Computer anmelden können, benötigen Sie eine Sicherheitsregel, die RDP-Zugriff auf Port 3389 zulässt. 

Dieses Beispiel erstellt eine NSG namens **myNsg**, das eine Regel namens **myRdpRule** enthält, die RDP-Datenverkehr über Port 3389 zulässt. Weitere Informationen zu NSGs finden Sie unter [Öffnen von Ports für einen virtuellen Computer in Azure mithilfe von PowerShell](nsg-quickstart-powershell.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

```powershell
$nsgName = "myNsg"

$rdpRule = New-AzureRmNetworkSecurityRuleConfig -Name myRdpRule -Description "Allow RDP" `
    -Access Allow -Protocol Tcp -Direction Inbound -Priority 110 `
    -SourceAddressPrefix Internet -SourcePortRange * `
    -DestinationAddressPrefix * -DestinationPortRange 3389

$nsg = New-AzureRmNetworkSecurityGroup -ResourceGroupName $rgName -Location $location `
    -Name $nsgName -SecurityRules $rdpRule
```


## <a name="create-a-variable-for-the-virtual-network"></a>Erstellen einer Variablen für das virtuelle Netzwerk
Erstellen einer Variablen für das abgeschlossene virtuelle Netzwerk 

```powershell
$vnet = Get-AzureRmVirtualNetwork -ResourceGroupName $rgName -Name $vnetName
```

## <a name="create-the-vm"></a>Erstellen des virtuellen Computers
Das folgende PowerShell-Skript veranschaulicht, wie Sie die Konfiguration für den virtuellen Computer einrichten und das hochgeladene VM-Image als Quelle für die neue Installation verwenden.

</br>

```powershell
    # Enter a new user name and password to use as the local administrator account 
    # for remotely accessing the VM.
    $cred = Get-Credential

    # Name of the storage account where the VHD is located. This example sets the 
    # storage account name as "myStorageAccount"
    $storageAccName = "myStorageAccount"

    # Name of the virtual machine. This example sets the VM name as "myVM".
    $vmName = "myVM"

    # Size of the virtual machine. This example creates "Standard_D2_v2" sized VM. 
    # See the VM sizes documentation for more information: 
    # https://azure.microsoft.com/documentation/articles/virtual-machines-windows-sizes/
    $vmSize = "Standard_D2_v2"

    # Computer name for the VM. This examples sets the computer name as "myComputer".
    $computerName = "myComputer"

    # Name of the disk that holds the OS. This example sets the 
    # OS disk name as "myOsDisk"
    $osDiskName = "myOsDisk"

    # Assign a SKU name. This example sets the SKU name as "Standard_LRS"
    # Valid values for -SkuName are: Standard_LRS - locally redundant storage, Standard_ZRS - zone redundant
    # storage, Standard_GRS - geo redundant storage, Standard_RAGRS - read access geo redundant storage,
    # Premium_LRS - premium locally redundant storage. 
    $skuName = "Standard_LRS"

    # Get the storage account where the uploaded image is stored
    $storageAcc = Get-AzureRmStorageAccount -ResourceGroupName $rgName -AccountName $storageAccName

    # Set the VM name and size
    $vmConfig = New-AzureRmVMConfig -VMName $vmName -VMSize $vmSize

    #Set the Windows operating system configuration and add the NIC
    $vm = Set-AzureRmVMOperatingSystem -VM $vmConfig -Windows -ComputerName $computerName `
        -Credential $cred -ProvisionVMAgent -EnableAutoUpdate
    $vm = Add-AzureRmVMNetworkInterface -VM $vm -Id $nic.Id

    # Create the OS disk URI
    $osDiskUri = '{0}vhds/{1}-{2}.vhd' `
        -f $storageAcc.PrimaryEndpoints.Blob.ToString(), $vmName.ToLower(), $osDiskName

    # Configure the OS disk to be created from the existing VHD image (-CreateOption fromImage).
    $vm = Set-AzureRmVMOSDisk -VM $vm -Name $osDiskName -VhdUri $osDiskUri `
        -CreateOption fromImage -SourceImageUri $imageURI -Windows

    # Create the new VM
    New-AzureRmVM -ResourceGroupName $rgName -Location $location -VM $vm
```

## <a name="verify-that-the-vm-was-created"></a>Stellen Sie sicher, dass der virtuelle Computer erstellt wurde
Anschließend müsste der neu erstellte virtuelle Computer im [Azure-Portal](https://portal.azure.com) unter **Durchsuchen** > **Virtuelle Computer** angezeigt werden. Alternativ können Sie ihn auch mit den folgenden PowerShell-Befehlen anzeigen:

```powershell
    $vmList = Get-AzureRmVM -ResourceGroupName $rgName
    $vmList.Name
```

## <a name="next-steps"></a>Nächste Schritte
Informationen zum Verwalten des neuen virtuellen Computers mit Azure PowerShell finden Sie unter [Verwalten von virtuellen Computern mit Azure Resource Manager und PowerShell](ps-manage.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).


