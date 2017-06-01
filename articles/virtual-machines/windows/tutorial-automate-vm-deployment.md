---
title: Anpassen einer Windows-VM in Azure | Microsoft-Dokumentation
description: "Erfahren Sie, wie Sie Windows-VMs in Azure mithilfe der benutzerdefinierten Skripterweiterungen und des Schlüsseltresors anpassen."
services: virtual-machines-windows
documentationcenter: virtual-machines
author: iainfoulds
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 05/02/2017
ms.author: iainfou
ms.translationtype: Human Translation
ms.sourcegitcommit: 2db2ba16c06f49fd851581a1088df21f5a87a911
ms.openlocfilehash: 014d282daffdbfc03e7f3495f8e59bfda4cdb396
ms.contentlocale: de-de
ms.lasthandoff: 05/08/2017

---

# <a name="how-to-customize-a-windows-virtual-machine-in-azure"></a>Anpassen eines virtuellen Windows-Computers in Azure
Zum Konfigurieren von virtuellen Computern (VMs) auf schnelle und einheitliche Weise ist meist eine Form der Automatisierung erwünscht. Eine gängige Methode zum Anpassen von Windows-VMs ist die Verwendung der [benutzerdefinierten Skripterweiterung für Windows](extensions-customscript.md). In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Verwenden der benutzerdefinierten Skripterweiterung zur Installation von IIS
> * Erstellen eines virtuellen Computers, der die benutzerdefinierte Skripterweiterung verwendet
> * Anzeigen einer ausgeführten IIS-Website aus nach Anwendung der Erweiterung

Für dieses Tutorial ist das Azure PowerShell-Modul Version 3.6 oder höher erforderlich. Führen Sie ` Get-Module -ListAvailable AzureRM` aus, um die Version zu finden. Wenn Sie ein Upgrade ausführen müssen, finden Sie unter [Installieren des Azure PowerShell-Moduls](/powershell/azure/install-azurerm-ps) Informationen dazu.


## <a name="custom-script-extension-overview"></a>Übersicht über benutzerdefinierte Skripterweiterungen
Die benutzerdefinierte Skripterweiterung lädt Skripts auf Azure-VMs herunter und führt sie aus. Diese Erweiterung ist hilfreich bei der Konfiguration nach der Bereitstellung, bei der Softwareinstallation oder bei anderen Konfigurations-/Verwaltungsaufgaben. Skripts können aus Azure Storage oder GitHub heruntergeladen oder dem Azure-Portal zur Laufzeit für die Erweiterung bereitgestellt werden.

Die benutzerdefinierte Skripterweiterung kann in Azure Resource Manager-Vorlagen integriert und auch mithilfe der Azure-Befehlszeilenschnittstelle, mithilfe von PowerShell, über das Azure-Portal oder unter Verwendung der REST-API für virtuelle Azure-Computer ausgeführt werden.

Sie können die benutzerdefinierte Skripterweiterung mit Windows- und Linux-VMs verwenden.


## <a name="create-virtual-machine"></a>Erstellen eines virtuellen Computers
Bevor Sie eine VM erstellen können, müssen Sie mit [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup) eine Ressourcengruppe erstellen. Das folgende Beispiel erstellt am Standort *EastUS* eine Ressourcengruppe mit dem Namen *myResourceGroupAutomate*:

```powershell
New-AzureRmResourceGroup -ResourceGroupName myResourceGroupAutomate -Location EastUS
```

Legen Sie mit [Get-Credential](https://msdn.microsoft.com/powershell/reference/5.1/microsoft.powershell.security/Get-Credential) den Benutzernamen und das Kennwort des Administrators der VMs fest:

```powershell
$cred = Get-Credential
```

Nun können nun mit [New-AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm) die VM erstellen. Das folgende Beispiel erstellt die erforderlichen virtuellen Netzwerkkomponenten, die Betriebssystemkonfiguration und dann einen virtuellen Computer mit dem Namen *myVM*:

```powershell
# Create a subnet configuration
$subnetConfig = New-AzureRmVirtualNetworkSubnetConfig `
    -Name mySubnet `
    -AddressPrefix 192.168.1.0/24

# Create a virtual network
$vnet = New-AzureRmVirtualNetwork `
    -ResourceGroupName myResourceGroupAutomate `
    -Location EastUS `
    -Name myVnet `
    -AddressPrefix 192.168.0.0/16 `
    -Subnet $subnetConfig

# Create a public IP address and specify a DNS name
$publicIP = New-AzureRmPublicIpAddress `
    -ResourceGroupName myResourceGroupAutomate `
    -Location EastUS `
    -AllocationMethod Static `
    -IdleTimeoutInMinutes 4 `
    -Name "myPublicIP"

# Create an inbound network security group rule for port 3389
$nsgRuleRDP = New-AzureRmNetworkSecurityRuleConfig `
    -Name myNetworkSecurityGroupRuleRDP  `
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
    -Name myNetworkSecurityGroupRuleWWW  `
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
    -ResourceGroupName myResourceGroupAutomate `
    -Location EastUS `
    -Name myNetworkSecurityGroup `
    -SecurityRules $nsgRuleRDP,$nsgRuleWeb

# Create a virtual network card and associate with public IP address and NSG
$nic = New-AzureRmNetworkInterface `
    -Name myNic `
    -ResourceGroupName myResourceGroupAutomate `
    -Location EastUS `
    -SubnetId $vnet.Subnets[0].Id `
    -PublicIpAddressId $publicIP.Id `
    -NetworkSecurityGroupId $nsg.Id

# Create a virtual machine configuration
$vmConfig = New-AzureRmVMConfig -VMName myVM -VMSize Standard_DS2 | `
Set-AzureRmVMOperatingSystem -Windows -ComputerName myVM -Credential $cred | `
Set-AzureRmVMSourceImage -PublisherName MicrosoftWindowsServer `
    -Offer WindowsServer -Skus 2016-Datacenter -Version latest | `
Add-AzureRmVMNetworkInterface -Id $nic.Id

New-AzureRmVM -ResourceGroupName myResourceGroupAutomate -Location EastUS -VM $vmConfig
```

Die Erstellung der Ressourcen und VM dauert einige Minuten.


## <a name="automate-iis-install"></a>Automatisieren der Installation von IIS
Verwenden Sie [Set-AzureRmVMExtension](/powershell/module/azurerm.compute/set-azurermvmextension) zum Installieren der benutzerdefinierten Skripterweiterung. Die Erweiterung führt `powershell Add-WindowsFeature Web-Server` zum Installieren des IIS-Webservers aus und aktualisiert dann die Seite *Default.htm* mit dem Hostnamen der VM:

```powershell
Set-AzureRmVMExtension -ResourceGroupName myResourceGroupAutomate `
    -ExtensionName IIS `
    -VMName myVM `
    -Publisher Microsoft.Compute `
    -ExtensionType CustomScriptExtension `
    -TypeHandlerVersion 1.4 `
    -SettingString '{"commandToExecute":"powershell Add-WindowsFeature Web-Server; powershell Add-Content -Path \"C:\\inetpub\\wwwroot\\Default.htm\" -Value $($env:computername)"}' `
    -Location EastUS
```


## <a name="test-web-site"></a>Testen der Website
Rufen Sie mit [Get-AzureRmPublicIPAddress](/powershell/module/azurerm.network/get-azurermpublicipaddress) die öffentliche IP-Adresse Ihres Load Balancers ab. Im folgenden Beispiel wird die IP-Adresse für *myPublicIP* abgerufen, die wir zuvor erstellt haben:

```powershell
Get-AzureRmPublicIPAddress `
    -ResourceGroupName myResourceGroupAutomate `
    -Name myPublicIP | select IpAddress
```

Geben Sie die öffentliche IP-Adresse in einen Webbrowser ein. Die Website wird mit dem Hostnamen der VM angezeigt, an die der Load Balancer den Datenverkehr wie im folgenden Beispiel verteilt hat:

![Ausführen der IIS-Website](./media/tutorial-automate-vm-deployment/running-iis-website.png)


## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie die IIS-Installation auf einem virtuellen Computer automatisiert. Es wurde Folgendes vermittelt:

> [!div class="checklist"]
> * Verwenden der benutzerdefinierten Skripterweiterung zur Installation von IIS
> * Erstellen eines virtuellen Computers, der die benutzerdefinierte Skripterweiterung verwendet
> * Anzeigen einer ausgeführten IIS-Website aus nach Anwendung der Erweiterung

Im nächsten Tutorial erfahren Sie, wie Sie benutzerdefinierte VM-Images erstellen.

> [!div class="nextstepaction"]
> [Erstellen von benutzerdefinierten VM-Images](./tutorial-custom-images.md)

