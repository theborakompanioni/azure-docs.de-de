---
title: Erstellen einer Azure-VM mit PowerShell | Microsoft Docs
description: "Verwenden Sie Azure PowerShell und Azure Resource Manager, um ohne großen Aufwand einen neuen virtuellen Computer mit Windows Server zu erstellen."
services: virtual-machines-windows
documentationcenter: 
author: davidmu1
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 14fe9ca9-e228-4d3b-a5d8-3101e9478f6e
ms.service: virtual-machines-windows
ms.topic: get-started-article
ms.date: 02/14/2017
ms.author: davidmu
translationtype: Human Translation
ms.sourcegitcommit: c1262b7708918cbdc8ce35f3e65a47a04797f195
ms.openlocfilehash: 8a67352a65e755f2177fb4870f34c41440bf90a2

---

# <a name="create-a-windows-vm-using-resource-manager-and-powershell"></a>Erstellen einer Windows-VM mit dem Resource Manager und PowerShell

In diesem Artikel ist beschrieben, wie Sie einen virtuellen Azure-Computer, der Windows Server ausführt, sowie die dafür erforderlichen Ressourcen mithilfe von [Resource Manager](../azure-resource-manager/resource-group-overview.md) und PowerShell schnell erstellen. Alle in diesem Artikel beschriebenen Schritte sind erforderlich, um einen virtuellen Computer zu erstellen. Die Durchführung dauert etwa 30 Minuten. Ersetzen Sie die Beispielparameterwerte in den Befehlen durch Namen, die für Ihre Umgebung sinnvoll sind.

## <a name="step-1-install-azure-powershell"></a>Schritt 1: Installieren von Azure PowerShell

Unter [Installieren und Konfigurieren von Azure PowerShell](/powershell/azureps-cmdlets-docs) erfahren Sie, wie Sie die neueste Version von Azure PowerShell installieren, Ihr Abonnement auswählen und sich bei Ihrem Konto anmelden.

## <a name="step-2-create-a-resource-group"></a>Schritt 2: Erstellen einer Ressourcengruppe

Alle Ressourcen müssen in einer Ressourcengruppe enthalten sein. Daher erstellen wir zuerst eine Ressourcengruppe.  

1. Rufen Sie eine Liste mit den verfügbaren Standorten ab, an denen Ressourcen erstellt werden können.
   
    ```powershell
    Get-AzureRmLocation | sort Location | Select Location
    ```

2. Legen Sie den Standort für die Ressourcen fest. Mit diesem Befehl wird der Standort auf **centralus** festgelegt.
   
    ```powershell
    $location = "centralus"
    ```

3. Erstellen Sie eine Ressourcengruppe. Mit diesem Befehl wird eine Ressourcengruppe mit dem Namen **myResourceGroup** am von Ihnen festgelegten Standort erstellt.
   
    ```powershell
    $myResourceGroup = "myResourceGroup"
    New-AzureRmResourceGroup -Name $myResourceGroup -Location $location
    ```

## <a name="step-3-optional-create-a-storage-account"></a>Schritt 3: (optional) Speicherkonto erstellen

Derzeit können Sie beim Erstellen eines virtuellen Computers zwischen der Verwendung von [Azure Managed Disks](../storage/storage-managed-disks-overview.md) oder nicht verwalteten Datenträgern wählen. Wenn Sie sich für die Verwendung eines nicht verwalteten Datenträgers entscheiden, , müssen Sie ein [Speicherkonto](../storage/storage-introduction.md) zum Speichern der virtuellen Festplatte erstellen, die vom erstellten virtuellen Computer verwendet wird. Bei Verwendung eines verwalteten Datenträgers wird das Speicherkonto nicht benötigt. Speicherkontonamen müssen zwischen 3 und 24 Zeichen lang sein und dürfen nur Zahlen und Kleinbuchstaben enthalten.

1. Testen Sie den Speicherkontonamen auf Eindeutigkeit. Mit diesem Befehl wird der Name **myStorageAccount** getestet.
   
    ```powershell
    $myStorageAccountName = "mystorageaccount"
    Get-AzureRmStorageAccountNameAvailability $myStorageAccountName
    ```
   
    Wenn dieser Befehl **True**zurückgibt, ist der vorgeschlagene Name in Azure eindeutig. 

2. Erstellen Sie jetzt das Speicherkonto.
   
    ```powershell    
    $myStorageAccount = New-AzureRmStorageAccount -ResourceGroupName $myResourceGroup `
        -Name $myStorageAccountName -SkuName "Standard_LRS" -Kind "Storage" -Location $location
    ```

## <a name="step-4-create-a-virtual-network"></a>Schritt 4: Erstellen eines virtuellen Netzwerks

Alle virtuellen Computer sind Teil eines [virtuellen Netzwerks](../virtual-network/virtual-networks-overview.md).

1. Erstellen Sie ein Subnetz für das virtuelle Netzwerk. Mit diesem Befehl wird ein Subnetz mit dem Namen **mySubnet** mit dem Adresspräfix 10.0.0.0/24 erstellt.
   
    ```powershell
    $mySubnet = New-AzureRmVirtualNetworkSubnetConfig -Name "mySubnet" -AddressPrefix 10.0.0.0/24
    ```

2. Erstellen Sie jetzt das virtuelle Netzwerk. Mit diesem Befehl wird ein virtuelles Netzwerk mit dem Namen **myVnet** erstellt, wofür das von Ihnen erstellte Subnetz und das Adresspräfix **10.0.0.0/16** verwendet wird.
   
    ```powershell
    $myVnet = New-AzureRmVirtualNetwork -Name "myVnet" -ResourceGroupName $myResourceGroup `
        -Location $location -AddressPrefix 10.0.0.0/16 -Subnet $mySubnet
    ```

## <a name="step-5-create-a-public-ip-address-and-network-interface"></a>Schritt 5: Erstellen einer öffentlichen IP-Adresse und Netzwerkschnittstelle

Sie benötigen eine [öffentliche IP-Adresse](../virtual-network/virtual-network-ip-addresses-overview-arm.md) und eine Netzwerkschnittstelle, um die Kommunikation mit dem virtuellen Computer im virtuellen Netzwerk zu ermöglichen.

1. Erstellen Sie die öffentliche IP-Adresse. Mit diesem Befehl wird eine öffentliche IP-Adresse mit dem Namen **myPublicIp** und der Zuordnungsmethode **Dynamisch** erstellt.
   
    ```powershell
    $myPublicIp = New-AzureRmPublicIpAddress -Name "myPublicIp" -ResourceGroupName $myResourceGroup `
        -Location $location -AllocationMethod Dynamic
    ```
2. Erstellen Sie die Netzwerkschnittstelle. Mit diesem Befehl wird eine Netzwerkschnittstelle mit dem Namen **myNIC** erstellt.
   
    ```powershell
    $myNIC = New-AzureRmNetworkInterface -Name "myNIC" -ResourceGroupName $myResourceGroup `
        -Location $location -SubnetId $myVnet.Subnets[0].Id -PublicIpAddressId $myPublicIp.Id
    ```

## <a name="step-6-create-a-virtual-machine"></a>Schritt 6: Erstellen eines virtuellen Computers

Nachdem Sie nun alle Komponenten eingerichtet haben, können Sie den virtuellen Computer erstellen. Sie können einen virtuellen Computer mit einem [Marketplace-Image](virtual-machines-windows-cli-ps-findimage.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json), einem [benutzerdefinierten generalisierten Image (mit Systemvorbereitung )](virtual-machines-windows-create-vm-generalized.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json), oder einem [benutzerdefinierten spezialisierten Image (ohne Systemvorbereitung)](virtual-machines-windows-create-vm-specialized.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) erstellen. In diesem Beispiel wird ein Windows Server-Image aus dem Marketplace verwendet. 

1. Führen Sie diesen Befehl zum Festlegen des Administratorkontonamens und des dazugehörigen Kennworts für den virtuellen Computer aus.

    ```powershell
    $cred = Get-Credential -Message "Type the name and password of the local administrator account."
    ```
   
    Das Kennwort muss 12 bis 123 Zeichen lang sein und mindestens einen Kleinbuchstaben, einen Großbuchstaben, eine Zahl und ein Sonderzeichen enthalten.

2. Erstellen Sie das Konfigurationsobjekt für den virtuellen Computer. Mit diesem Befehl wird ein Konfigurationsobjekt mit dem Namen **myVmConfig** erstellt, mit dem der Name und die Größe der VM definiert werden.
   
    ```powershell
    $myVM = New-AzureRmVMConfig -VMName "myVM" -VMSize "Standard_DS1_v2"
    ```
   
    Eine Liste mit den verfügbaren Größen für einen virtuellen Computer finden Sie unter [Größen für virtuelle Computer in Azure](virtual-machines-windows-sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) .

3. Konfigurieren Sie Betriebssystemeinstellungen für den virtuellen Computer. Mit diesem Befehl werden der Computername, der Betriebssystemtyp und die Kontoanmeldeinformationen für die VM festgelegt.
   
    ```powershell
    $myVM = Set-AzureRmVMOperatingSystem -VM $myVM -Windows -ComputerName "myVM" -Credential $cred `
        -ProvisionVMAgent -EnableAutoUpdate
    ```

4. Definieren Sie das Image, das für die Bereitstellung der VM verwendet werden soll. Mit diesem Befehl wird das Windows Server-Image definiert, das für die VM verwendet werden soll. 

    ```powershell
    $myVM = Set-AzureRmVMSourceImage -VM $myVM -PublisherName "MicrosoftWindowsServer" `
        -Offer "WindowsServer" -Skus "2012-R2-Datacenter" -Version "latest"
    ```

5. Fügen Sie die erstellte Netzwerkschnittstelle der Konfiguration hinzu.
   
    ```powershell
    $myVM = Add-AzureRmVMNetworkInterface -VM $myVM -Id $myNIC.Id
    ```

6. Wenn Sie einen nicht verwalteten Datenträger verwenden, führen Sie diesen Befehl aus, um den Namen und Speicherort der Festplatte des virtuellen Computers zu definieren; andernfalls überspringen Sie diesen Schritt. Die Datei der virtuellen Festplatte für einen nicht verwalteten Datenträger wird in einem Container gespeichert. Mit diesem Befehl wird der Datenträger in einem Container mit dem Namen **vhds/myOsDisk1.vhd** unter dem von Ihnen erstellten Speicherkonto erstellt.
   
    ```powershell
    $blobPath = "vhds/myOsDisk1.vhd"
    $osDiskUri = $myStorageAccount.PrimaryEndpoints.Blob.ToString() + $blobPath
    ```

7. Fügen Sie die Informationen zum Betriebssystemdatenträger der VM-Konfiguration hinzu. Dieser Befehl erstellt einen Datenträger mit dem Namen **myOsDisk1**.
   
    Wenn Sie einen verwalteten Datenträger verwenden, führen Sie diesen Befehl aus, um den Betriebssystem-Datenträgers in der Konfiguration festzulegen:

    ```powershell
    $myVM = Set-AzureRmVMOSDisk -VM $myVM -Name "myOsDisk1" -StorageAccountType PremiumLRS -DiskSizeInGB 128 -CreateOption FromImage -Caching ReadWrite
    ```

    Wenn Sie einen nicht verwalteten Datenträger verwenden, führen Sie diesen Befehl aus, um den Betriebssystem-Datenträgers in der Konfiguration festzulegen:

    ```powershell
    $myVM = Set-AzureRmVMOSDisk -VM $myVM -Name "myOsDisk1" -VhdUri $osDiskUri -CreateOption fromImage
    ```

8. Erstellen Sie als Letztes den virtuellen Computer.
   
    ```powershell
    New-AzureRmVM -ResourceGroupName $myResourceGroup -Location $location -VM $myVM
    ```

## <a name="next-steps"></a>Nächste Schritte

* Falls bei der Bereitstellung Probleme aufgetreten sind, sollten Sie den Artikel [Beheben gängiger Azure-Bereitstellungsfehler mit Azure Resource Manager](../azure-resource-manager/resource-manager-common-deployment-errors.md) lesen.
* Informationen zur Verwaltung des erstellten virtuellen Computers finden Sie unter [Verwalten von virtuellen Computern mit Azure Resource Manager und PowerShell](virtual-machines-windows-ps-manage.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
* Nutzen Sie die Vorteile der vorlagenbasierten Erstellung virtueller Computer, indem Sie sich die Informationen unter [Erstellen eines virtuellen Windows-Computers mit einer Resource Manager-Vorlage](virtual-machines-windows-ps-template.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)




<!--HONumber=Feb17_HO3-->


