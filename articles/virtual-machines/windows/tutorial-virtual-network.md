---
title: Virtuelle Azure-Netzwerke und virtuelle Windows-Computer | Microsoft-Dokumentation
description: 'Tutorial: Verwalten von virtuellen Azure-Netzwerken und virtuellen Windows-Computern mit Azure PowerShell'
services: virtual-machines-windows
documentationcenter: virtual-machines
author: davidmu1
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
ms.author: davidmu
ms.custom: mvc
ms.translationtype: HT
ms.sourcegitcommit: 3eb68cba15e89c455d7d33be1ec0bf596df5f3b7
ms.openlocfilehash: 54a4a37d581f023610cd61835bdc76814fbd46e0
ms.contentlocale: de-de
ms.lasthandoff: 09/01/2017

---

# <a name="manage-azure-virtual-networks-and-windows-virtual-machines-with-azure-powershell"></a>Verwalten von virtuellen Azure-Netzwerken und virtuellen Windows-Computern mit Azure PowerShell

Virtuelle Azure-Computer nutzen Azure-Netzwerke für interne und externe Kommunikation. In diesem Tutorial erstellen Sie mehrere virtuelle Computer (VMs) in einem virtuellen Netzwerk und konfigurieren die Netzwerkverbindung zwischen ihnen. Folgendes wird vermittelt:

> [!div class="checklist"]
> * Erstellen eines virtuellen Netzwerks
> * Erstellen von Subnetzen in virtuellen Netzwerken
> * Steuern des Netzwerkdatenverkehrs mit Netzwerksicherheitsgruppen
> * Anzeigen der Datenverkehrsregeln in Aktion

Für dieses Tutorial ist das Azure PowerShell-Modul Version 3.6 oder höher erforderlich. Führen Sie ` Get-Module -ListAvailable AzureRM` aus, um die Version zu finden. Wenn Sie ein Upgrade ausführen müssen, finden Sie unter [Installieren und Konfigurieren von Azure PowerShell](/powershell/azure/install-azurerm-ps) Informationen dazu.

## <a name="create-vnet"></a>Erstellen des VNET

Ein VNet ist eine Darstellung Ihres eigenen Netzwerks in der Cloud. Bei einem VNet handelt es sich um eine logische Isolation von der dedizierten Azure-Cloud für Ihr Abonnement. Ein VNET umfasst Subnetze, Regeln für die Verbindung mit diesen Subnetzen und Verbindungen von den virtuellen Computern zu den Subnetzen.

Vor der Erstellung anderer Azure-Ressourcen müssen Sie zunächst mit [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup) eine Ressourcengruppe erstellen. Das folgende Beispiel erstellt am Standort *EastUS* eine Ressourcengruppe mit dem Namen *myRGNetwork*:

```powershell
New-AzureRmResourceGroup -ResourceGroupName myRGNetwork -Location EastUS
```

Ein Subnetz ist eine untergeordnete Ressource eines VNet und hilft, die Segmente von Adressräumen innerhalb eines CIDR-Blocks mithilfe von IP-Adressenpräfixen zu definieren. NICs können zu Subnetzen hinzugefügt und mit virtuellen Computern verbunden werden, sodass sie Konnektivität für verschiedene Workloads bereitstellen.

Erstellen Sie mit [New-AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/new-azurermvirtualnetworksubnetconfig) ein Subnetz:

```powershell
$frontendSubnet = New-AzureRmVirtualNetworkSubnetConfig `
  -Name myFrontendSubnet `
  -AddressPrefix 10.0.0.0/24
```

Erstellen Sie ein VNet mit dem Namen *MyVNet* unter Angabe von *MyFrontendSubnet* in [New-AzureRmVirtualNetwork](/powershell/module/azurerm.network/new-azurermvirtualnetwork):

```powershell
$vnet = New-AzureRmVirtualNetwork `
  -ResourceGroupName myRGNetwork `
  -Location EastUS `
  -Name myVNet `
  -AddressPrefix 10.0.0.0/16 `
  -Subnet $frontendSubnet
```

## <a name="create-front-end-vm"></a>Erstellen des virtuellen Front-End-Computers

Ein virtueller Computer muss für die Kommunikation in einem virtuellen Netzwerk über eine virtuelle Netzwerkschnittstelle (NIC) verfügen. Auf *myFrontendVM* wird über das Internet zugegriffen. Daher muss auch eine öffentliche IP-Adresse festgelegt werden. 

Erstellen Sie mit [New-AzureRmPublicIpAddress](/powershell/module/azurerm.network/new-azurermpublicipaddress) eine öffentliche IP-Adresse:

```powershell
$pip = New-AzureRmPublicIpAddress `
  -ResourceGroupName myRGNetwork `
  -Location EastUS `
  -AllocationMethod Static `
  -Name myPublicIPAddress
```

Erstellen Sie mit [New-AzureRmNetworkInterface](/powershell/module/azurerm.network/new-azurermnetworkinterface) eine Netzwerkschnittstelle:


```powershell
$frontendNic = New-AzureRmNetworkInterface `
  -ResourceGroupName myRGNetwork `
  -Location EastUS `
  -Name myFrontendNic `
  -SubnetId $vnet.Subnets[0].Id `
  -PublicIpAddressId $pip.Id
```

Legen Sie mit [Get-Credential](https://msdn.microsoft.com/powershell/reference/5.1/microsoft.powershell.security/Get-Credential) den Benutzernamen und das Kennwort für das Administratorkonto auf der VM fest. Diese Anmeldeinformationen benötigen Sie, um in weiteren Schritten eine Verbindung mit der VM herzustellen:

```powershell
$cred = Get-Credential
```

Erstellen Sie die virtuellen Computer mit [New-AzureRmVMConfig](/powershell/module/azurerm.compute/new-azurermvmconfig), [Set-AzureRmVMOperatingSystem](/powershell/module/azurerm.compute/set-azurermvmoperatingsystem), [Set-AzureRmVMSourceImage](/powershell/module/azurerm.compute/set-azurermvmsourceimage), [Set-AzureRmVMOSDisk](/powershell/module/azurerm.compute/set-azurermvmosdisk), [Add-AzureRmVMNetworkInterface](/powershell/module/azurerm.compute/add-azurermvmnetworkinterface) und [New-AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm). 

```powershell
$frontendVM = New-AzureRmVMConfig `
    -VMName myFrontendVM `
    -VMSize Standard_D1
$frontendVM = Set-AzureRmVMOperatingSystem `
    -VM $frontendVM `
    -Windows `
    -ComputerName myFrontendVM `
    -Credential $cred `
    -ProvisionVMAgent `
    -EnableAutoUpdate
$frontendVM = Set-AzureRmVMSourceImage `
    -VM $frontendVM `
    -PublisherName MicrosoftWindowsServer `
    -Offer WindowsServer `
    -Skus 2016-Datacenter `
    -Version latest
$frontendVM = Set-AzureRmVMOSDisk `
    -VM $frontendVM `
    -Name myFrontendOSDisk `
    -DiskSizeInGB 128 `
    -CreateOption FromImage `
    -Caching ReadWrite
$frontendVM = Add-AzureRmVMNetworkInterface `
    -VM $frontendVM `
    -Id $frontendNic.Id
New-AzureRmVM `
    -ResourceGroupName myRGNetwork `
    -Location EastUS `
    -VM $frontendVM
```

## <a name="install-web-server"></a>Installieren des Webservers

Sie können IIS über eine Remotedesktopsitzung auf *myFrontendVM* installieren. Sie müssen die öffentliche IP-Adresse des virtuellen Computers abrufen, um darauf Zugriff zu erhalten.

Sie können die öffentliche IP-Adresse von *myFrontendVM* mit [Get-AzureRmPublicIPAddress](/powershell/module/azurerm.network/get-azurermpublicipaddress) abrufen. Im folgenden Beispiel wird die IP-Adresse für *myPublicIPAddress* abgerufen, die wir zuvor erstellt haben:

```powershell
Get-AzureRmPublicIPAddress `
    -ResourceGroupName myRGNetwork `
    -Name myPublicIPAddress | select IpAddress
```

Notieren Sie sich diese IP-Adresse, damit Sie sie in späteren Schritten verwenden können.

Erstellen Sie mit dem folgenden Befehl eine Remotedesktopsitzung mit *myFrontendVM*. Ersetzen Sie *<publicIPAddress>* durch die Adresse, die Sie zuvor notiert haben. Geben Sie bei entsprechender Aufforderung die beim Erstellen des virtuellen Computers verwendeten Anmeldeinformationen ein.

```
mstsc /v:<publicIpAddress>
``` 

Nach der Anmeldung bei *myFrontendVM* können Sie mit einer einzelnen PowerShell-Codezeile IIS installieren und die lokale Firewallregel aktivieren, um Webdatenverkehr zuzulassen. Öffnen Sie über die RDP-Sitzung eine PowerShell-Eingabeaufforderung auf Ihrer VM, und führen Sie den folgenden Befehl aus:

Führen Sie mit [Install-WindowsFeature](https://technet.microsoft.com/itpro/powershell/windows/servermanager/install-windowsfeature) die benutzerdefinierte Skripterweiterung aus, mit der der IIS-Webserver installiert wird:

```powershell
Install-WindowsFeature -name Web-Server -IncludeManagementTools
```

Nun können Sie über die öffentliche IP-Adresse zum virtuellen Computer navigieren und die IIS-Website anzeigen.

![IIS-Standardwebsite](./media/tutorial-virtual-network/iis.png)

## <a name="manage-internal-traffic"></a>Verwalten des internen Datenverkehrs

Eine Netzwerksicherheitsgruppe (NSG) enthält eine Liste mit Sicherheitsregeln, mit denen Netzwerkdatenverkehr für Ressourcen, die mit einem virtuellen Netzwerk verbunden sind, zugelassen oder abgelehnt wird. Netzwerksicherheitsgruppen können Subnetzen oder einzelnen an virtuelle Computer angefügte NICs zugeordnet werden. Das Öffnen oder Schließen des Zugriffs auf virtuelle Computer über Ports erfolgt mithilfe von NSG-Regeln. Beim Erstellen von *myFrontendVM* wurde der eingehende Port 3389 automatisch für RDP-Verbindungen geöffnet.

Die interne Kommunikation von virtuellen Computern kann über eine Netzwerksicherheitsgruppe konfiguriert werden. In diesem Abschnitt erfahren Sie, wie ein weiteres Subnetz im Netzwerk erstellt und ihm eine Netzwerksicherheitsgruppe zugewiesen wird, sodass eine Verbindung von *myFrontendVM* mit *myBackendVM* über Port 1433 zulässig ist. Das Subnetz wird dann bei der Erstellung dem virtuellen Computer zugewiesen.

Durch Erstellen einer Netzwerksicherheitsgruppe für das Back-End-Subnetz können Sie den Datenverkehr zu *myBackendVM* nur auf *myFrontendVM* beschränken. Im folgenden Beispiel wird die NSG-Regel *myBackendNSGRule* mit [New-AzureRmNetworkSecurityRuleConfig](/powershell/module/azurerm.network/new-azurermnetworksecurityruleconfig) erstellt:

```powershell
$nsgBackendRule = New-AzureRmNetworkSecurityRuleConfig `
  -Name myBackendNSGRule `
  -Protocol Tcp `
  -Direction Inbound `
  -Priority 100 `
  -SourceAddressPrefix 10.0.0.0/24 `
  -SourcePortRange * `
  -DestinationAddressPrefix * `
  -DestinationPortRange 1433 `
  -Access Allow
```

Fügen Sie mit [New-AzureRmNetworkSecurityGroup](/powershell/module/azurerm.network/new-azurermnetworksecuritygroup) die Netzwerksicherheitsgruppe *myBackendNSG* hinzu:

```powershell
$nsgBackend = New-AzureRmNetworkSecurityGroup `
  -ResourceGroupName myRGNetwork `
  -Location EastUS `
  -Name myBackendNSG `
  -SecurityRules $nsgBackendRule
```
## <a name="add-back-end-subnet"></a>Hinzufügen des Back-End-Subnetzes

Fügen Sie *myBackEndSubnet*mit [Add-AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/add-azurermvirtualnetworksubnetconfig) zu *myVNet* hinzu:

```powershell
Add-AzureRmVirtualNetworkSubnetConfig `
  -Name myBackendSubnet `
  -VirtualNetwork $vnet `
  -AddressPrefix 10.0.1.0/24 `
  -NetworkSecurityGroup $nsgBackend
Set-AzureRmVirtualNetwork -VirtualNetwork $vnet
$vnet = Get-AzureRmVirtualNetwork `
  -ResourceGroupName myRGNetwork `
  -Name myVNet
```

## <a name="create-back-end-vm"></a>Erstellen des virtuellen Back-End-Computers

Der virtuelle Back-End-Computer kann am einfachsten mithilfe eines SQL Server-Images erstellt werden. In diesem Tutorial wird nur der virtuelle Computer mit dem Datenbankserver erstellt, es werden jedoch keine Informationen für den Zugriff auf die Datenbank angegeben.

Erstellen Sie *myBackendNic*:

```powershell
$backendNic = New-AzureRmNetworkInterface `
  -ResourceGroupName myRGNetwork `
  -Location EastUS `
  -Name myBackendNic `
  -SubnetId $vnet.Subnets[1].Id
```

Legen Sie mit „Get-Credential“ den Benutzernamen und das Kennwort für das Administratorkonto auf dem virtuellen Computer fest:

```powershell
$cred = Get-Credential
```

Erstellen Sie *myBackendVM*:

```powershell
$backendVM = New-AzureRmVMConfig `
  -VMName myBackendVM `
  -VMSize Standard_D1
$backendVM = Set-AzureRmVMOperatingSystem `
  -VM $backendVM `
  -Windows `
  -ComputerName myBackendVM `
  -Credential $cred `
  -ProvisionVMAgent `
  -EnableAutoUpdate
$backendVM = Set-AzureRmVMSourceImage `
  -VM $backendVM `
  -PublisherName MicrosoftSQLServer `
  -Offer SQL2016SP1-WS2016 `
  -Skus Enterprise `
  -Version latest
$backendVM = Set-AzureRmVMOSDisk `
  -VM $backendVM `
  -Name myBackendOSDisk `
  -DiskSizeInGB 128 `
  -CreateOption FromImage `
  -Caching ReadWrite
$backendVM = Add-AzureRmVMNetworkInterface `
  -VM $backendVM `
  -Id $backendNic.Id
New-AzureRmVM `
  -ResourceGroupName myRGNetwork `
  -Location EastUS `
  -VM $backendVM
```

In dem verwendeten Image ist SQL Server installiert, wird aber in diesem Tutorial nicht verwendet. SQL Server ist enthalten, damit Sie sehen, wie Sie einen virtuellen Computer für die Verarbeitung des Webdatenverkehrs und einen virtuellen Computer für die Verarbeitung der Datenbankverwaltung konfigurieren können.

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie Azure-Netzwerke in Bezug auf virtuelle Computer erstellt und erhalten. 

> [!div class="checklist"]
> * Erstellen eines virtuellen Netzwerks
> * Erstellen von Subnetzen in virtuellen Netzwerken
> * Steuern des Netzwerkdatenverkehrs mit Netzwerksicherheitsgruppen
> * Anzeigen der Datenverkehrsregeln in Aktion

Im nächsten Tutorial erhalten Sie Informationen zum Überwachen der Sicherung von Daten auf virtuellen Computern mit Azure Backup. verfügbar.

> [!div class="nextstepaction"]
> [Back up Windows virtual machines in Azure (Sichern virtueller Windows-Computer in Azure)](./tutorial-backup-vms.md)

