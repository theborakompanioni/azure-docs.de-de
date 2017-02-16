---
title: Erstellen einer Azure-VM mit PowerShell | Microsoft Docs
description: "Verwenden Sie Azure PowerShell und Azure Resource Manager, um ohne großen Aufwand eine neue VM mit Windows Server zu erstellen."
services: virtual-machines-windows
documentationcenter: 
author: davidmu1
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 14fe9ca9-e228-4d3b-a5d8-3101e9478f6e
ms.service: virtual-machines-windows
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 10/21/2016
ms.author: davidmu
translationtype: Human Translation
ms.sourcegitcommit: 94c18aa0c4fe38fb74931d5ed61fece207c8b5ce
ms.openlocfilehash: 701a5515cd1d52f7ca8d3562dabcdf0e4d31183d


---
# <a name="create-a-windows-vm-using-resource-manager-and-powershell"></a>Erstellen einer Windows-VM mit dem Resource Manager und PowerShell
In diesem Artikel ist beschrieben, wie Sie einen virtuellen Azure-Computer, der Windows Server ausführt, sowie die dafür erforderlichen Ressourcen mithilfe von [Resource Manager](../azure-resource-manager/resource-group-overview.md) und PowerShell schnell erstellen. 

Alle in diesem Artikel beschriebenen Schritte sind erforderlich, um einen virtuellen Computer zu erstellen. Die Durchführung dauert etwa 30 Minuten. Ersetzen Sie die Beispielparameterwerte in den Befehlen durch Namen, die für Ihre Umgebung sinnvoll sind.

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

## <a name="step-3-create-a-storage-account"></a>Schritt 3: Erstellen eines Speicherkontos
Ein [Speicherkonto](../storage/storage-introduction.md) wird zum Speichern der virtuellen Festplatte benötigt, die von dem erstellten virtuellen Computer verwendet wird. Speicherkontonamen müssen zwischen 3 und 24 Zeichen lang sein und dürfen nur Zahlen und Kleinbuchstaben enthalten.

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
Nachdem Sie nun alle Komponenten eingerichtet haben, können Sie den virtuellen Computer erstellen.

1. Führen Sie diesen Befehl zum Festlegen des Administratorkontonamens und des dazugehörigen Kennworts für den virtuellen Computer aus.

    ```powershell
    $cred = Get-Credential -Message "Type the name and password of the local administrator account."
    ```
   
    Das Kennwort muss 12 bis 123 Zeichen lang sein und mindestens einen Kleinbuchstaben, einen Großbuchstaben, eine Zahl und ein Sonderzeichen enthalten. 
2. Erstellen Sie das Konfigurationsobjekt für den virtuellen Computer. Mit diesem Befehl wird ein Konfigurationsobjekt mit dem Namen **myVmConfig** erstellt, mit dem der Name und die Größe der VM definiert werden.
   
    ```powershell
    $myVm = New-AzureRmVMConfig -VMName "myVM" -VMSize "Standard_DS1_v2"
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
   
    Weitere Informationen zum Auswählen von Images finden Sie unter [Navigieren zu und Auswählen von Images virtueller Windows-Computer in Azure mithilfe von PowerShell oder der Befehlszeilenschnittstelle](virtual-machines-windows-cli-ps-findimage.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
5. Fügen Sie die erstellte Netzwerkschnittstelle der Konfiguration hinzu.
   
    ```powershell
    $myVM = Add-AzureRmVMNetworkInterface -VM $myVM -Id $myNIC.Id
    ```
6. Definieren Sie den Namen und den Speicherort der VM-Festplatte. Die Datei der virtuellen Festplatte wird in einem Container gespeichert. Mit diesem Befehl wird der Datenträger in einem Container mit dem Namen **vhds/myOsDisk1.vhd** unter dem von Ihnen erstellten Speicherkonto erstellt.
   
    ```powershell
    $blobPath = "vhds/myOsDisk1.vhd"
    $osDiskUri = $myStorageAccount.PrimaryEndpoints.Blob.ToString() + $blobPath
    ```
7. Fügen Sie die Informationen zum Betriebssystemdatenträger der VM-Konfiguration hinzu. Ersetzen Sie den Wert von **$diskName** durch den Namen des Betriebssystem-Datenträgers. Erstellen Sie die Variable, und fügen Sie der Konfiguration die Festplatteninformationen hinzu.
   
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




<!--HONumber=Jan17_HO2-->


