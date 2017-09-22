---
title: Erstellen eines virtuellen Windows-Computers mithilfe von PowerShell in Azure Stack | Microsoft-Dokumentation
description: Erstellen Sie einen virtuellen Windows-Computers mithilfe von PowerShell in Azure Stack.
services: azure-stack
documentationcenter: 
author: SnehaGunda
manager: byronr
editor: 
ms.assetid: 
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/10/2017
ms.author: sngun
ms.translationtype: HT
ms.sourcegitcommit: 5b6c261c3439e33f4d16750e73618c72db4bcd7d
ms.openlocfilehash: 4b6706b289e323706009c40e9d1ad0149f8accc5
ms.contentlocale: de-de
ms.lasthandoff: 09/15/2017

---

# <a name="create-a-windows-virtual-machine-by-using-powershell-in-azure-stack"></a>Erstellen eines virtuellen Windows-Computers mithilfe von PowerShell in Azure Stack

Virtuelle Computer in Azure Stack bieten Ihnen die Flexibilität der Virtualisierung, ohne dass Sie Zeit und Geld für den Kauf und das Verwalten der Hardware aufwenden zu müssen, die diese hostet. Bei der Verwendung virtueller Computer gibt es einige Unterschiede zwischen den Features, die in Azure bzw. Azure Stack verfügbar sind. Informationen dazu finden Sie im Thema [Considerations for virtual machines in Azure Stack](azure-stack-vm-considerations.md) (Aspekte von virtuellen Computern in Azure Stack). 

In dieser Anleitung wird das Erstellen einer Windows Server 2016-VM mithilfe von PowerShell in Azure Stack beschrieben. Führen Sie die in diesem Artikel beschriebenen Schritte entweder über das Azure Stack Development Kit oder über einen Windows-basierten externen Client (wenn Sie über VPN verbunden sind) aus. 

## <a name="prerequisites"></a>Voraussetzungen

1. Das Windows Server 2016-Image ist standardmäßig nicht im Azure Stack-Marketplace enthalten. Bevor Sie einen virtuellen Computer erstellen, stellen Sie sicher, dass der Azure Stack-Betreiber [das Windows Server 2016-Image zum Azure Stack-Marketplace hinzufügt](azure-stack-add-default-image.md). 
2. Azure Stack erfordert eine spezifische Version des Azure PowerShell-Moduls, um die Ressourcen zu erstellen und zu verwalten. Führen Sie die im Thema [Install PowerShell for Azure Stack](azure-stack-powershell-install.md) (Installieren von PowerShell für Azure Stack) beschriebenen Schritte aus, um die erforderliche Version zu installieren.
3. [Konfigurieren der PowerShell-Umgebung des Azure Stack-Benutzers](azure-stack-powershell-configure-user.md) 

## <a name="create-a-resource-group"></a>Erstellen einer Ressourcengruppe

Eine Ressourcengruppe ist ein logischer Container, in dem Azure Stack-Ressourcen bereitgestellt und verwaltet werden. Mithilfe des folgenden Codes können Sie eine Ressourcengruppe erstellen: Wir haben allen Variablen in diesem Dokument Werte zugewiesen. Sie können entweder diese Werte verwenden oder einen anderen Wert zuweisen.  

```powershell
# Create variables to store the location and resource group names.
$location = "local"
$ResourceGroupName = "myResourceGroup"

New-AzureRmResourceGroup `
  -Name $ResourceGroupName `
  -Location $location
```

## <a name="create-storage-resources"></a>Erstellen von Speicherressourcen 

Erstellen Sie ein Speicherkonto und einen Speichercontainer zum Speichern des Windows Server 2016-Images.

```powershell
# Create variables to store the storage account name and the storage account SKU information
$StorageAccountName = "mystorageaccount"
$SkuName = "Standard_LRS"

# Create a new storage account
$StorageAccount = New-AzureRMStorageAccount `
  -Location $location `
  -ResourceGroupName $ResourceGroupName `
  -Type $SkuName `
  -Name $StorageAccountName

Set-AzureRmCurrentStorageAccount `
  -StorageAccountName $storageAccountName `
  -ResourceGroupName $resourceGroupName

# Create a storage container to store the virtual machine image
$containerName = 'osdisks'
$container = New-AzureStorageContainer `
  -Name $containerName `
  -Permission Blob
```

## <a name="create-networking-resources"></a>Erstellen von Netzwerkressourcen

Erstellen Sie ein virtuelles Netzwerk, ein Subnetz und eine öffentliche IP-Adresse. Diese Ressourcen dienen dazu, Netzwerkkonnektivität für den virtuellen Computer bereitzustellen.  

```powershell
# Create a subnet configuration
$subnetConfig = New-AzureRmVirtualNetworkSubnetConfig `
  -Name mySubnet `
  -AddressPrefix 192.168.1.0/24

# Create a virtual network
$vnet = New-AzureRmVirtualNetwork `
  -ResourceGroupName $ResourceGroupName `
  -Location $location `
  -Name MyVnet `
  -AddressPrefix 192.168.0.0/16 `
  -Subnet $subnetConfig

# Create a public IP address and specify a DNS name
$pip = New-AzureRmPublicIpAddress `
  -ResourceGroupName $ResourceGroupName `
  -Location $location `
  -AllocationMethod Static `
  -IdleTimeoutInMinutes 4 `
  -Name "mypublicdns$(Get-Random)"
```

### <a name="create-a-network-security-group-and-a-network-security-group-rule"></a>Erstellen Sie eine Netzwerksicherheitsgruppe und eine Netzwerksicherheitsgruppen-Regel.

Die Netzwerksicherheitsgruppe sichert den virtuellen Computer mithilfe von Regeln für eingehenden und ausgehenden Datenverkehr. Wir erstellen nun eine eingehende Regel für Port 3389, um eingehende Remotedesktopverbindungen zuzulassen, und eine eingehende Regel für Port 80, um eingehenden Webdatenverkehr zuzulassen.

```powershell
# Create an inbound network security group rule for port 3389
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

# Create an inbound network security group rule for port 80
$nsgRuleWeb = New-AzureRmNetworkSecurityRuleConfig `
  -Name myNetworkSecurityGroupRuleWWW `
  -Protocol Tcp `
  -Direction Inbound `
  -Priority 1001 `
  -SourceAddressPrefix * `
  -SourcePortRange * `
  -DestinationAddressPrefix * `
  -DestinationPortRange 80 `
  -Access Allow

# Create a network security group
$nsg = New-AzureRmNetworkSecurityGroup `
  -ResourceGroupName $ResourceGroupName `
  -Location $location `
  -Name myNetworkSecurityGroup `
  -SecurityRules $nsgRuleRDP,$nsgRuleWeb 
```
 
### <a name="create-a-network-card-for-the-virtual-machine"></a>Erstellen einer Netzwerkkarte für den virtuellen Computer

Die Netzwerkkarte verbindet die VM mit einem Subnetz, einer Netzwerksicherheitsgruppe und einer öffentlichen IP-Adresse.

```powershell
# Create a virtual network card and associate it with public IP address and NSG
$nic = New-AzureRmNetworkInterface `
  -Name myNic `
  -ResourceGroupName $ResourceGroupName `
  -Location $location `
  -SubnetId $vnet.Subnets[0].Id `
  -PublicIpAddressId $pip.Id `
  -NetworkSecurityGroupId $nsg.Id 
```

## <a name="create-a-virtual-machine"></a>Erstellen eines virtuellen Computers

Erstellen Sie eine VM-Konfiguration. Diese Konfiguration umfasst die beim Bereitstellen des virtuellen Computers verwendeten Einstellungen, z.B. ein VM-Image, die Größe und die Authentifizierungskonfiguration.

```powershell
# Define a credential object to store the username and password for the virtual machine
$UserName='demouser'
$Password='Password@123'| ConvertTo-SecureString -Force -AsPlainText
$Credential=New-Object PSCredential($UserName,$Password)

# Create the virtual machine configuration object
$VmName = "VirtualMachinelatest"
$VmSize = "Standard_A1"
$VirtualMachine = New-AzureRmVMConfig `
  -VMName $VmName `
  -VMSize $VmSize 

$VirtualMachine = Set-AzureRmVMOperatingSystem `
  -VM $VirtualMachine `
  -Windows `
  -ComputerName "MainComputer" `
  -Credential $Credential 

$VirtualMachine = Set-AzureRmVMSourceImage `
  -VM $VirtualMachine `
  -PublisherName "MicrosoftWindowsServer" `
  -Offer "WindowsServer" `
  -Skus "2016-Datacenter" `
  -Version "latest"

$osDiskName = "OsDisk"
$osDiskUri = '{0}vhds/{1}-{2}.vhd' -f `
  $StorageAccount.PrimaryEndpoints.Blob.ToString(),`
  $vmName.ToLower(), `
  $osDiskName

# Sets the operating system disk properties on a virtual machine. 
$VirtualMachine = Set-AzureRmVMOSDisk `
  -VM $VirtualMachine `
  -Name $osDiskName `
  -VhdUri $OsDiskUri `
  -CreateOption FromImage | `
  Add-AzureRmVMNetworkInterface -Id $nic.Id 

#Create the virtual machine.
New-AzureRmVM `
  -ResourceGroupName $ResourceGroupName `
  -Location $location `
  -VM $VirtualMachine
```

## <a name="connect-to-the-virtual-machine"></a>Herstellen einer Verbindung mit dem virtuellen Computer

Wenn der virtuelle Computer erfolgreich erstellt wurde, öffnen Sie im Development Kit oder über einen Windows-basierten externen Client (wenn Sie über VPN verbunden sind) eine Remotedesktopverbindung mit dem virtuellen Computer. Um eine Remoteverbindung mit dem virtuellen Computer herzustellen, den Sie im vorherigen Schritt erstellt haben, benötigen Sie seine öffentliche IP-Adresse. Führen Sie den folgenden Befehl aus, um die öffentliche IP-Adresse des virtuellen Computers abzurufen: 

```powershell
Get-AzureRmPublicIpAddress `
  -ResourceGroupName $ResourceGroupName | Select IpAddress
```
 
Erstellen Sie mit dem folgenden Befehl eine Remotedesktopsitzung mit dem virtuellen Computer. Ersetzen Sie die IP-Adresse mit dem publicIPAddress-Wert des virtuellen Computers. Wenn Sie dazu aufgefordert werden, geben Sie den Benutzernamen und das Kennwort ein, das Sie beim Erstellen des virtuellen Computers angegeben haben.

```powershell
mstsc /v:<publicIpAddress>
```
## <a name="delete-the-virtual-machine"></a>Löschen des virtuellen Computers

Entfernen Sie die Ressourcengruppe, die den virtuellen Computer und die dazugehörigen Ressourcen enthält, mithilfe des folgenden Befehls, wenn Sie sie nicht mehr benötigen:

```powershell
Remove-AzureRmResourceGroup `
  -Name $ResourceGroupName
```

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zum Speicher in Azure Stack finden Sie in dieser [Übersicht](azure-stack-storage-overview.md).


