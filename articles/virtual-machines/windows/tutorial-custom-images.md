---
title: Erstellen benutzerdefinierter VM-Images mithilfe von Azure PowerShell | Microsoft-Dokumentation
description: 'Tutorial: Erstellen eines benutzerdefinierten VM-Images mithilfe von Azure PowerShell'
services: virtual-machines-windows
documentationcenter: virtual-machines
author: cynthn
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 05/08/2017
ms.author: cynthn
ms.translationtype: Human Translation
ms.sourcegitcommit: 18d4994f303a11e9ce2d07bc1124aaedf570fc82
ms.openlocfilehash: 773b37ec8f775d68f1faca0d252f3064c7de0317
ms.contentlocale: de-de
ms.lasthandoff: 05/09/2017

---

# <a name="create-a-custom-image-of-an-azure-vm-using-powershell"></a>Erstellen eines benutzerdefinierten Images eines virtuellen Azure-Computers mithilfe von PowerShell

Benutzerdefinierte Images sind wie Marketplace-Images, Sie erstellen sie jedoch selbst. Benutzerdefinierte Images können zum Starten von Konfigurationen verwendet werden, z.B. zum Vorabladen von Anwendungen, Anwendungskonfigurationen und anderen Betriebssystemkonfigurationen. In diesem Tutorial erstellen Sie ein eigenes benutzerdefiniertes Image eines virtuellen Azure-Computers. Folgendes wird vermittelt:

> [!div class="checklist"]
> * Sysprep und Generalisieren virtueller Computer
> * Erstellen eines benutzerdefinierten Images
> * Erstellen eines virtuellen Computers aus einem benutzerdefinierten Image
> * Liste aller Images in Ihrem Abonnement
> * Löschen eines Images

Für dieses Tutorial ist das Azure PowerShell-Modul Version 3.6 oder höher erforderlich. Führen Sie ` Get-Module -ListAvailable AzureRM` aus, um die Version zu finden. Wenn Sie ein Upgrade ausführen müssen, finden Sie unter [Installieren des Azure PowerShell-Moduls](/powershell/azure/install-azurerm-ps) Informationen dazu.

## <a name="before-you-begin"></a>Voraussetzungen

In den folgenden Schritten wird erläutert, wie Sie eine vorhandene VM in ein wiederverwendbares benutzerdefiniertes Image umwandeln, mit dem Sie neue VM-Instanzen erstellen können.

Für das Beispiel in diesem Tutorial muss ein virtueller Computer vorhanden sein. Gegebenenfalls können Sie mit diesem [Beispielskript](../scripts/virtual-machines-windows-powershell-sample-create-vm.md) einen virtuellen Computer erstellen. Ersetzen Sie beim Durcharbeiten des Tutorials bei Bedarf den Namen der Ressourcengruppe und des virtuellen Computers.

## <a name="prepare-vm"></a>Vorbereiten der VM

Zum Erstellen eines Images eines virtuellen Computers müssen Sie den virtuellen Computer vorbereiten, indem Sie ihn generalisieren, die Zuordnung aufheben und dann den virtuellen Quellcomputer in Azure als generalisiert markieren.

### <a name="generalize-the-windows-vm-using-sysprep"></a>Generalisieren der Windows-VM mithilfe von Sysprep

Sysprep entfernt unter anderem alle persönlichen Kontoinformationen, und bereitet den Computer darauf vor, als Image verwendet zu werden. Weitere Informationen zu Sysprep finden Sie unter [How to Use Sysprep: An Introduction](http://technet.microsoft.com/library/bb457073.aspx)(in englischer Sprache).


1. Stellen Sie eine Verbindung mit dem virtuellen Computer her.
2. Öffnen Sie das Eingabeaufforderungsfenster als Administrator. Wechseln Sie zum Verzeichnis *%windir%\system32\sysprep*, und führen Sie anschließend *sysprep.exe* aus.
3. Wählen Sie unter **Systemvorbereitungsprogramm** die Option *Out-of-Box-Experience (OOBE) für System aktivieren*, und vergewissern Sie sich, dass das Kontrollkästchen *Verallgemeinern* aktiviert ist.
4. Wählen Sie unter **Optionen für Herunterfahren** die Option *Herunterfahren* aus, und klicken Sie auf **OK**.
5. Nach Abschluss von Sysprep wird die virtuelle Maschine heruntergefahren. **Starten Sie den virtuellen Computer nicht neu**.

### <a name="deallocate-and-mark-the-vm-as-generalized"></a>Aufheben der Zuordnung und Markieren des virtuellen Computers als generalisiert

Um ein Image zu erstellen, muss die Zuordnung des virtuellen Computers aufgehoben werden, und er muss in Azure als generalisiert markiert werden.

Heben Sie die Zuordnung des virtuellen Computers mit [Stop-AzureRmVM](/powershell/module/azurerm.compute/stop-azurermvm) auf.

```powershell
Stop-AzureRmVM -ResourceGroupName myResourceGroupImages -Name myVM -Force
```

Legen Sie den Status des virtuellen Computers mithilfe von [Set-AzureRmVm](/powershell/module/azurerm.compute/set-azurermvm) auf `-Generalized` fest. 
   
```powershell
Set-AzureRmVM -ResourceGroupName myResourceGroupImages -Name myVM -Generalized
```


## <a name="create-the-image"></a>Erstellen des Images

Nun können Sie ein Image des virtuellen Computers mit [New-AzureRmImageConfig](/powershell/module/azurerm.compute/new-azurermimageconfig) und [New-AzureRmImage](/powershell/module/azurerm.compute/new-azurermimage) erstellen. Im folgenden Beispiel wird ein Image mit dem Namen *myImage* vom virtuellen Computer *myVM* erstellt.

Rufen Sie den virtuellen Computer ab. 

```powershell
$vm = Get-AzureRmVM -Name myVM -ResourceGroupName myResourceGroupImages
```

Erstellen Sie die Imagekonfiguration.

```powershell
$image = New-AzureRmImageConfig -Location EastUS -SourceVirtualMachineId $vm.ID 
```

Erstellen Sie das Image.

```powershell
New-AzureRmImage -Image $image -ImageName myImage -ResourceGroupName myResourceGroupImages
```    

 
## <a name="create-vms-from-the-image"></a>Erstellen von VMs anhand des Images

Nachdem Sie ein Image erstellt haben, können Sie anhand des Images eine oder mehrere neue VMs erstellen. Das Erstellen einer VM anhand eines benutzerdefinierten Images ist vergleichbar mit dem Erstellen einer VM mithilfe eines Marketplace-Images. Wenn Sie ein Marketplace-Image verwenden, müssen Sie Informationen zum Image, zu dessen Anbieter, zum Angebot, zur SKU und Version angeben. Bei einem benutzerdefinierten Image müssen Sie nur die ID der benutzerdefinierten Imageressource bereitstellen. 

Im folgenden Skript erstellen wir die Variable *$image* zum Speichern von Informationen zum benutzerdefinierten Image mithilfe von [Get-AzureRmImage](/powershell/module/azurerm.compute/get-azurermimage). Dann verwenden wir [Set-AzureRmVMSourceImage](/powershell/module/azurerm.compute/set-azurermvmsourceimage) und geben die ID mithilfe der Variablen *$image* an, die wir zuvor erstellt haben. 

Das Skript erstellt einen virtuellen Computer mit dem Namen *MyVMfromImage* anhand unseres benutzerdefinierten Images in einer neuen Ressourcengruppe namens *MyResourceGroupFromImage* in der Region *USA, Westen*.


```powershell
$cred = Get-Credential -Message "Enter a username and password for the virtual machine."

New-AzureRmResourceGroup -Name myResourceGroupFromImage -Location EastUS

$subnetConfig = New-AzureRmVirtualNetworkSubnetConfig `
    -Name mySubnet `
    -AddressPrefix 192.168.1.0/24

$vnet = New-AzureRmVirtualNetwork `
    -ResourceGroupName myResourceGroupFromImage `
    -Location EastUS `
    -Name MYvNET `
    -AddressPrefix 192.168.0.0/16 `
    -Subnet $subnetConfig

$pip = New-AzureRmPublicIpAddress `
    -ResourceGroupName myResourceGroupFromImage `
    -Location EastUS `
    -Name "mypublicdns$(Get-Random)" `
    -AllocationMethod Static `
    -IdleTimeoutInMinutes 4

  $nsgRuleRDP = New-AzureRmNetworkSecurityRuleConfig `
    -Name myNetworkSecurityGroupRuleRDP `
    -Protocol Tcp `
    -Direction Inbound `
    -Priority 1000 `
    -SourceAddressPrefix * `
    -SourcePortRange * `
    -DestinationAddressPrefix * `
    -DestinationPortRange 3389 `
    -Access Allow

  $nsg = New-AzureRmNetworkSecurityGroup `
    -ResourceGroupName myResourceGroupFromImage `
    -Location EastUS `
    -Name myNetworkSecurityGroup `
    -SecurityRules $nsgRuleRDP

$nic = New-AzureRmNetworkInterface `
    -Name myNic `
    -ResourceGroupName myResourceGroupFromImage `
    -Location EastUS `
    -SubnetId $vnet.Subnets[0].Id `
    -PublicIpAddressId $pip.Id `
    -NetworkSecurityGroupId $nsg.Id

$vmConfig = New-AzureRmVMConfig `
    -VMName myVMfromImage `
    -VMSize Standard_D1 | Set-AzureRmVMOperatingSystem -Windows `
        -ComputerName myComputer `
        -Credential $cred 

# Here is where we create a variable to store information about the image 
$image = Get-AzureRmImage `
    -ImageName myImage `
    -ResourceGroupName myResourceGroupImages

# Here is where we specify that we want to create the VM from and image and provide the image ID
$vmConfig = Set-AzureRmVMSourceImage -VM $vmConfig -Id $image.Id

$vmConfig = Add-AzureRmVMNetworkInterface -VM $vmConfig -Id $nic.Id

New-AzureRmVM `
    -ResourceGroupName myResourceGroupFromImage `
    -Location EastUS `
    -VM $vmConfig
```

## <a name="image-management"></a>Verwaltung von Images 

Hier sind einige Beispiele für allgemeine Aufgaben der Imageverwaltung und ihre Ausführung mithilfe von PowerShell.

Auflisten aller Images nach Namen.

```powershell
$images = Find-AzureRMResource -ResourceType Microsoft.Compute/images 
$images.name
```

Löschen eines Images. Dieses Beispiel löscht das Image mit dem Namen *myOldImage* aus *myResourceGroup*.

```powershell
Remove-AzureRmImage `
    -ImageName myOldImage `
    -ResourceGroupName myResourceGroup
```

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie ein benutzerdefiniertes Image eines virtuellen Computers erstellt. Es wurde Folgendes vermittelt:

> [!div class="checklist"]
> * Sysprep und Generalisieren virtueller Computer
> * Erstellen eines benutzerdefinierten Images
> * Erstellen eines virtuellen Computers aus einem benutzerdefinierten Image
> * Liste aller Images in Ihrem Abonnement
> * Löschen eines Images

Im nächsten Tutorial erhalten Sie Informationen zu hoch verfügbaren virtuellen Computern.

> [!div class="nextstepaction"]
> [Erstellen eines hoch verfügbaren virtuellen Computers](tutorial-availability-sets.md)




