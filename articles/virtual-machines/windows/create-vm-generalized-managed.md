---
title: Erstellen einer VM auf der Grundlage eines verwalteten VM-Images in Azure | Microsoft-Dokumentation
description: Erstellen Sie einen virtuellen Windows-Computer auf der Grundlage eines generalisierten verwalteten VM-Images mit Azure PowerShell im Resource Manager-Bereitstellungsmodell.
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
ms.date: 02/7/2017
ms.author: cynthn
ms.translationtype: Human Translation
ms.sourcegitcommit: c308183ffe6a01f4d4bf6f5817945629cbcedc92
ms.openlocfilehash: e308b75581ca5372a4de168895f2eeb140a69a78
ms.contentlocale: de-de
ms.lasthandoff: 05/17/2017


---
# <a name="create-a-vm-from-a-generalized-managed-vm-image"></a>Erstellen eines virtuellen Computers auf der Grundlage eines generalisierten verwalteten VM-Images

Sie können mehrere virtuelle Computer auf der Grundlage eines verwalteten VM-Images in Azure erstellen. Ein verwaltetes VM-Image enthält die erforderlichen Informationen zum Erstellen eines virtuellen Computers, einschließlich der Datenträger für Betriebssystem und Daten. Die VHDs, aus denen das Image besteht, einschließlich der Betriebssystem-Datenträger und sonstiger Datenträger, werden als verwaltete Datenträger gespeichert. 

Auf einer generalisierten VM wurden alle Ihre persönlichen Kontoinformationen mit [Sysprep](generalize-vhd.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) entfernt. Sie können eine generalisierte VM erstellen, indem Sie Sysprep auf einem lokalen virtuellen Computer ausführen, dann [die VHD in Azure hochladen](upload-image.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) oder Sysprep auf einer bestehenden Azure-VM ausführen und dann [ein Image der VM erfassen](capture-image-resource.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).



## <a name="prerequisites"></a>Voraussetzungen

Sie müssen bereits [ein verwaltetes VM-Image erstellt](capture-image-resource.md) haben, das zum Erstellen des neuen virtuellen Computers verwendet werden soll. 

Stellen Sie sicher, dass Sie die neueste Version des AzureRM.Compute-PowerShell-Moduls verwenden. Führen Sie den folgenden Befehl aus, um es zu installieren.

```powershell
Install-Module AzureRM.Compute -RequiredVersion 2.6.0
```
Weitere Informationen finden Sie unter [Azure PowerShell-Versionsverwaltung](/powershell/azure/overview).



## <a name="collect-information-about-the-image"></a>Sammeln von Informationen zu dem Image

Zunächst müssen wir grundlegende Informationen zum Image erfassen und eine Variable für das Image erstellen. In diesem Beispiel wird ein verwaltetes VM-Image mit dem Namen **myImage** verwendet, das sich in der Ressourcengruppe **myResourceGroup** in der Region **USA, Westen-Mitte** befindet. 

```powershell
$rgName = "myResourceGroup"
$location = "West Central US"
$imageName = "myImage"
$image = Get-AzureRMImage -ImageName $imageName -ResourceGroupName $rgName
```

## <a name="create-a-virtual-network"></a>Erstellen eines virtuellen Netzwerks
Erstellen Sie das VNet und das Subnetz des [virtuellen Netzwerks](../../virtual-network/virtual-networks-overview.md).

1. Erstellen Sie das Subnetz. Mit diesem Beispielbefehl wird ein Subnetz namens **mySubnet** mit dem Adresspräfix **10.0.0.0/24** erstellt.  
   
    ```powershell
    $subnetName = "mySubnet"
    $singleSubnet = New-AzureRmVirtualNetworkSubnetConfig -Name $subnetName -AddressPrefix 10.0.0.0/24
    ```
2. Erstellen des virtuellen Netzwerks Im folgenden Beispiel wird ein virtuelles Netzwerk namens **myVnet** mit dem Adresspräfix **10.0.0.0/16** erstellt.  
   
    ```powershell
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

Damit Sie sich über RDP bei Ihrem virtuellen Computer anmelden können, benötigen Sie eine Netzwerksicherheitsregel (Network Security Rule, NSG), die RDP-Zugriff auf Port 3389 zulässt. 

Dieses Beispiel erstellt eine NSG namens **myNsg**, das eine Regel namens **myRdpRule** enthält, die RDP-Datenverkehr über Port 3389 zulässt. Weitere Informationen zu NSGs finden Sie unter [Öffnen von Ports für einen virtuellen Computer in Azure mithilfe von PowerShell](nsg-quickstart-powershell.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

```powershell
$nsgName = "myNsg"
$ruleName = "myRdpRule"
$rdpRule = New-AzureRmNetworkSecurityRuleConfig -Name $ruleName -Description "Allow RDP" `
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

## <a name="get-the-credentials-for-the-vm"></a>Abrufen der Anmeldeinformationen für die VM

Mit dem folgenden Cmdlet wird ein Fenster geöffnet, in dem Sie einen neuen Benutzernamen und ein Kennwort als lokales Administratorkonto für den Remotezugriff auf die VM eingeben. 

```powershell
$cred = Get-Credential
```

## <a name="set-variables-for-the-vm-name-computer-name-and-the-size-of-the-vm"></a>Festlegen von Variablen für den VM-Namen, Computernamen und die VM-Größe

1. Erstellen Sie Variablen für den VM- und Computernamen. In diesem Beispiel wird der VM-Name als **myVM** und der Name des Computers als **myComputer** festgelegt.

    ```powershell
    $vmName = "myVM"
    $computerName = "myComputer"
    ```
2. Legen Sie die Größe des virtuellen Computers fest. In diesem Beispiel wird eine VM der Größe **Standard_DS1_v2** erstellt. Weitere Informationen finden Sie in der Dokumentation zu den [VM-Größen](https://azure.microsoft.com/documentation/articles/virtual-machines-windows-sizes/).

    ```powershell
    $vmSize = "Standard_DS1_v2"
    ```

3. Fügen Sie der VM-Konfiguration den VM-Namen und die VM-Größe hinzu.

```powershell
$vm = New-AzureRmVMConfig -VMName $vmName -VMSize $vmSize
```

## <a name="set-the-vm-image-as-source-image-for-the-new-vm"></a>Festlegen des VM-Images als Quellimage für die neue VM

Legen Sie das Quellimage mithilfe der ID des verwalteten VM-Images fest.

```powershell
$vm = Set-AzureRmVMSourceImage -VM $vm -Id $image.Id
```

## <a name="set-the-os-configuration-and-add-the-nic"></a>Legen Sie die Betriebssystemkonfiguration fest, und fügen Sie die Netzwerkkarte hinzu.

Geben Sie den Speichertyp (PremiumLRS oder StandardLRS) und die Größe des Betriebssystem-Datenträgers ein. Bei diesem Beispiel wird der Kontotyp auf **PremiumLRS**, die Datenträgergröße auf **128GB** und das Datenträgercaching auf **ReadWrite** festgelegt.

```powershell
$vm = Set-AzureRmVMOSDisk -VM $vm  -StorageAccountType PremiumLRS -DiskSizeInGB 128 `
-CreateOption FromImage -Caching ReadWrite

$vm = Set-AzureRmVMOperatingSystem -VM $vm -Windows -ComputerName $computerName `
-Credential $cred -ProvisionVMAgent -EnableAutoUpdate

$vm = Add-AzureRmVMNetworkInterface -VM $vm -Id $nic.Id
```

## <a name="create-the-vm"></a>Erstellen des virtuellen Computers

Erstellen Sie die neue VM mithilfe der Konfiguration, die Sie erstellt und in der Variablen **$vm** gespeichert haben.

```powershell
New-AzureRmVM -VM $vm -ResourceGroupName $rgName -Location $location
```

## <a name="verify-that-the-vm-was-created"></a>Stellen Sie sicher, dass der virtuelle Computer erstellt wurde
Anschließend müsste der neu erstellte virtuelle Computer im [Azure-Portal](https://portal.azure.com) unter **Durchsuchen** > **Virtuelle Computer** angezeigt werden. Alternativ können Sie ihn auch mit den folgenden PowerShell-Befehlen anzeigen:

```powershell
    $vmList = Get-AzureRmVM -ResourceGroupName $rgName
    $vmList.Name
```

## <a name="next-steps"></a>Nächste Schritte
Informationen zum Verwalten Ihres neuen virtuellen Computers mit Azure PowerShell finden Sie unter [Erstellen und Verwalten von virtuellen Windows-Computern mit dem Azure PowerShell-Modul](tutorial-manage-vm.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).


