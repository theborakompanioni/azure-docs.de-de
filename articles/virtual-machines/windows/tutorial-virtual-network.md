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
ms.date: 04/18/2017
ms.author: davidmu
translationtype: Human Translation
ms.sourcegitcommit: 1cc1ee946d8eb2214fd05701b495bbce6d471a49
ms.openlocfilehash: 7015a5317c631dad9079f2694051fa7fb28d232b
ms.lasthandoff: 04/26/2017

---

# <a name="manage-azure-virtual-networks-and-windows-virtual-machines-with-azure-powershell"></a>Verwalten von virtuellen Azure-Netzwerken und virtuellen Windows-Computern mit Azure PowerShell

In diesem Tutorial erfahren Sie, wie Sie mehrere virtuelle Computer (VMs) in einem virtuellen Netzwerk (VNET) erstellen und die Netzwerkverbindung zwischen ihnen konfigurieren. Nach Abschluss des Tutorials kann über das Internet über Port 80 für HTTP-Verbindungen auf einen virtuellen Front-End-Computer zugegriffen werden. Ein virtueller Back-End-Computer mit einer SQL Server-Datenbank wird isoliert und ist nur über den virtuellen Front-End-Computer an Port 1433 verfügbar.

Die Schritte in diesem Tutorial können mit dem neuesten [Azure PowerShell](https://docs.microsoft.com/powershell/azureps-cmdlets-docs/)-Modul ausgeführt werden.

## <a name="create-vnet"></a>Erstellen des VNET

Ein VNet ist eine Darstellung Ihres eigenen Netzwerks in der Cloud. Bei einem VNet handelt es sich um eine logische Isolation von der dedizierten Azure-Cloud für Ihr Abonnement. Ein VNET umfasst Subnetze, Regeln für die Verbindung mit diesen Subnetzen und Verbindungen von den virtuellen Computern zu den Subnetzen.

Vor der Erstellung anderer Azure-Ressourcen müssen Sie zunächst mit [New-AzureRmResourceGroup](https://docs.microsoft.com/powershell/module/azurerm.resources/new-azurermresourcegroup?view=azurermps-3.8.0) eine Ressourcengruppe erstellen. Das folgende Beispiel erstellt eine Ressourcengruppe mit dem Namen `myRGNetwork` am Standort `westus`:

```powershell
New-AzureRmResourceGroup -ResourceGroupName myRGNetwork -Location westus
```

Ein Subnetz ist eine untergeordnete Ressource eines VNet und hilft, die Segmente von Adressräumen innerhalb eines CIDR-Blocks mithilfe von IP-Adressenpräfixen zu definieren. NICs können zu Subnetzen hinzugefügt und mit virtuellen Computern verbunden werden, sodass sie Konnektivität für verschiedene Workloads bereitstellen.

Erstellen Sie mit [New-AzureRmVirtualNetworkSubnetConfig](https://docs.microsoft.com/powershell/module/azurerm.network/new-azurermvirtualnetworksubnetconfig?view=azurermps-3.8.0) ein Subnetz:

```powershell
$frontendSubnet = New-AzureRmVirtualNetworkSubnetConfig `
  -Name myFrontendSubnet `
  -AddressPrefix 10.0.0.0/24
```

Erstellen Sie mit [New-AzureRmVirtualNetwork](https://docs.microsoft.com/powershell/module/azurerm.network/new-azurermvirtualnetwork?view=azurermps-3.8.0) unter Verwendung von `myFrontendSubnet` ein VNET mit dem Namen `myVNet`:

```powershell
$vnet = New-AzureRmVirtualNetwork `
  -ResourceGroupName myRGNetwork `
  -Location westus `
  -Name myVNet `
  -AddressPrefix 10.0.0.0/16 `
  -Subnet $frontendSubnet
```

## <a name="create-front-end-vm"></a>Erstellen des virtuellen Front-End-Computers

Ein virtueller Computer muss für die Kommunikation in einem virtuellen Netzwerk über eine virtuelle Netzwerkschnittstelle (NIC) verfügen. Auf `myFrontendVM` wird über das Internet zugegriffen, daher muss eine öffentliche IP-Adresse festgelegt werden. 

Erstellen Sie mit [New-AzureRmPublicIpAddress](https://docs.microsoft.com/powershell/module/azurerm.network/new-azurermpublicipaddress?view=azurermps-3.8.0) eine öffentliche IP-Adresse:

```powershell
$pip = New-AzureRmPublicIpAddress `
  -ResourceGroupName myRGNetwork `
  -Location westus `
  -AllocationMethod Static `
  -Name myPublicIPAddress
```

Erstellen Sie mit [New-AzureRmNetworkInterface](https://docs.microsoft.com/powershell/module/azurerm.network/new-azurermnetworkinterface?view=azurermps-3.8.0) eine Netzwerkschnittstelle:


```powershell
$frontendNic = New-AzureRmNetworkInterface `
  -ResourceGroupName myRGNetwork `
  -Location westus `
  -Name myFrontendNic `
  -SubnetId $vnet.Subnets[0].Id `
  -PublicIpAddressId $pip.Id
```

Legen Sie mit [Get-Credential](https://msdn.microsoft.com/powershell/reference/5.1/microsoft.powershell.security/Get-Credential) den Benutzernamen und das Kennwort für das Administratorkonto auf dem virtuellen Computer fest:

```powershell
$cred = Get-Credential
```

Erstellen Sie die virtuellen Computer mit [New-AzureRmVMConfig](https://docs.microsoft.com/powershell/module/azurerm.compute/new-azurermvmconfig?view=azurermps-3.8.0), [Set-AzureRmVMOperatingSystem](https://docs.microsoft.com/powershell/module/azurerm.compute/set-azurermvmoperatingsystem?view=azurermps-3.8.0), [Set-AzureRmVMSourceImage](https://docs.microsoft.com/powershell/module/azurerm.compute/set-azurermvmsourceimage?view=azurermps-3.8.0), [Set-AzureRmVMOSDisk](https://docs.microsoft.com/powershell/module/azurerm.compute/set-azurermvmosdisk?view=azurermps-3.8.0), [Add-AzureRmVMNetworkInterface](https://docs.microsoft.com/powershell/module/azurerm.compute/add-azurermvmnetworkinterface?view=azurermps-3.8.0) und [New-AzureRmVM](https://docs.microsoft.com/powershell/module/azurerm.compute/new-azurermvm?view=azurermps-3.8.0). 

```powershell
$frontendVM = New-AzureRmVMConfig -VMName myFrontendVM -VMSize Standard_D1
$frontendVM = Set-AzureRmVMOperatingSystem -VM $frontendVM -Windows -ComputerName myFrontendVM -Credential $cred -ProvisionVMAgent -EnableAutoUpdate
$frontendVM = Set-AzureRmVMSourceImage -VM $frontendVM -PublisherName MicrosoftWindowsServer -Offer WindowsServer -Skus 2016-Datacenter -Version latest
$frontendVM = Set-AzureRmVMOSDisk -VM $frontendVM -Name myFrontendOSDisk -DiskSizeInGB 128 -CreateOption FromImage -Caching ReadWrite
$frontendVM = Add-AzureRmVMNetworkInterface -VM $frontendVM -Id $frontendNic.Id
New-AzureRmVM -ResourceGroupName myRGNetwork -Location westus -VM $frontendVM
```

## <a name="install-web-server"></a>Installieren des Webservers

Sie können IIS über eine Remotedesktopsitzung auf `myFrontendVM` installieren. Sie müssen die öffentliche IP-Adresse des virtuellen Computers abrufen, um darauf Zugriff zu erhalten.

Sie können die öffentliche IP-Adresse von `myFrontendVM` mit [Get-AzureRmPublicIPAddress](https://docs.microsoft.com/powershell/module/azurerm.network/get-azurermpublicipaddress?view=azurermps-3.8.0) abrufen. Im folgenden Beispiel wird die IP-Adresse für `myPublicIPAddress` abgerufen, die wir zuvor erstellt haben:

```powershell
Get-AzureRmPublicIPAddress -ResourceGroupName myRGNetwork -Name myPublicIPAddress | select IpAddress
```

Notieren Sie sich diese IP-Adresse, damit Sie sie in späteren Schritten verwenden können.

Erstellen Sie mit dem folgenden Befehl eine Remotedesktopsitzung mit `myFrontendVM`. Ersetzen Sie `<publicIPAddress>` durch die Adresse, die Sie zuvor notiert haben. Geben Sie bei entsprechender Aufforderung die beim Erstellen des virtuellen Computers verwendeten Anmeldeinformationen ein.

```
mstsc /v:<publicIpAddress>
``` 

Nach der Anmeldung bei `myFrontendVM` können Sie mit einer einzelnen PowerShell-Codezeile IIS installieren und die lokale Firewallregel aktivieren, um Webdatenverkehr zuzulassen. Öffnen Sie eine PowerShell-Eingabeaufforderung, und führen Sie den folgenden Befehl aus:

Führen Sie mit [Install-WindowsFeature](https://technet.microsoft.com/itpro/powershell/windows/servermanager/install-windowsfeature) die benutzerdefinierte Skripterweiterung aus, mit der der IIS-Webserver installiert wird:

```powershell
Install-WindowsFeature -name Web-Server -IncludeManagementTools
```

Nun können Sie über die öffentliche IP-Adresse zum virtuellen Computer navigieren und die IIS-Website anzeigen.

![IIS-Standardwebsite](./media/tutorial-virtual-network/iis.png)

## <a name="manage-internal-traffic"></a>Verwalten des internen Datenverkehrs

Eine Netzwerksicherheitsgruppe (NSG) enthält eine Liste mit Sicherheitsregeln, mit denen Netzwerkdatenverkehr für Ressourcen, die mit einem virtuellen Netzwerk verbunden sind, zugelassen oder abgelehnt wird. Netzwerksicherheitsgruppen können Subnetzen oder einzelnen an virtuelle Computer angefügte NICs zugeordnet werden. Das Öffnen oder Schließen des Zugriffs auf virtuelle Computer über Ports erfolgt mithilfe von NSG-Regeln. Beim Erstellen von `myFrontendVM` wurde der eingehende Port 3389 automatisch für RDP-Verbindungen geöffnet.

Die interne Kommunikation von virtuellen Computern kann über eine Netzwerksicherheitsgruppe konfiguriert werden. In diesem Abschnitt erfahren Sie, wie ein weiteres Subnetz im Netzwerk erstellt und ihm eine Netzwerksicherheitsgruppe zugewiesen wird, sodass eine Verbindung von `myFrontendVM` zu `myBackendVM` über Port 1433 zulässig ist. Das Subnetz wird dann bei der Erstellung dem virtuellen Computer zugewiesen.

Durch Erstellen einer Netzwerksicherheitsgruppe für das Back-End-Subnetz können Sie den Datenverkehr zu `myBackendVM` nur auf `myFrontendVM` beschränken. Im folgenden Beispiel wird die NSG-Regel `myBackendNSGRule` mit [New-AzureRmNetworkSecurityRuleConfig](https://docs.microsoft.com/powershell/module/azurerm.network/new-azurermnetworksecurityruleconfig?view=azurermps-3.8.0) erstellt:

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

Fügen Sie mit [New-AzureRmNetworkSecurityGroup](https://docs.microsoft.com/powershell/module/azurerm.network/new-azurermnetworksecuritygroup?view=azurermps-3.8.0) die neue Netzwerksicherheitsgruppe `myBackendNSG` hinzu:

```powershell
$nsgBackend = New-AzureRmNetworkSecurityGroup `
  -ResourceGroupName myRGNetwork `
  -Location westus `
  -Name myBackendNSG `
  -SecurityRules $nsgBackendRule
```
## <a name="add-back-end-subnet"></a>Hinzufügen des Back-End-Subnetzes

Fügen Sie mit [Add-AzureRmVirtualNetworkSubnetConfig](https://docs.microsoft.com/powershell/module/azurerm.network/add-azurermvirtualnetworksubnetconfig?view=azurermps-3.8.0) `myBackEndSubnet` zu `myVNet` hinzu:

```powershell
Add-AzureRmVirtualNetworkSubnetConfig -Name myBackendSubnet `
  -VirtualNetwork $vnet `
  -AddressPrefix 10.0.1.0/24 `
  -NetworkSecurityGroup $nsgBackend
Set-AzureRmVirtualNetwork -VirtualNetwork $vnet
$vnet = Get-AzureRmVirtualNetwork -ResourceGroupName myRGNetwork -Name myVNet
```

## <a name="create-back-end-vm"></a>Erstellen des virtuellen Back-End-Computers

Der virtuelle Back-End-Computer kann am einfachsten mithilfe eines SQL Server-Images erstellt werden. In diesem Tutorial wird nur der virtuelle Computer mit dem Datenbankserver erstellt, es werden jedoch keine Informationen für den Zugriff auf die Datenbank angegeben.

Erstellen Sie `myBackendNic`:

```powershell
$backendNic = New-AzureRmNetworkInterface `
  -ResourceGroupName myRGNetwork `
  -Location westus `
  -Name myBackendNic `
  -SubnetId $vnet.Subnets[1].Id
```

Legen Sie mit „Get-Credential“ den Benutzernamen und das Kennwort für das Administratorkonto auf dem virtuellen Computer fest:

```powershell
$cred = Get-Credential
```

Erstellen Sie `myBackendVM`:

```powershell
$backendVM = New-AzureRmVMConfig -VMName myBackendVM -VMSize Standard_D1
$backendVM = Set-AzureRmVMOperatingSystem -VM $backendVM -Windows -ComputerName myBackendVM -Credential $cred -ProvisionVMAgent -EnableAutoUpdate
$backendVM = Set-AzureRmVMSourceImage -VM $backendVM -PublisherName MicrosoftSQLServer -Offer SQL2016-WS2016 -Skus Enterprise -Version latest
$backendVM = Set-AzureRmVMOSDisk -VM $backendVM -Name myBackendOSDisk -DiskSizeInGB 128 -CreateOption FromImage -Caching ReadWrite
$backendVM = Add-AzureRmVMNetworkInterface -VM $backendVM -Id $backendNic.Id
New-AzureRmVM -ResourceGroupName myRGNetwork -Location westus -VM $backendVM
```

In dem verwendeten Image ist SQL Server installiert, wird aber in diesem Tutorial nicht verwendet. SQL Server ist enthalten, damit Sie sehen, wie Sie einen virtuellen Computer für die Verarbeitung des Webdatenverkehrs und einen virtuellen Computer für die Verarbeitung der Datenbankverwaltung konfigurieren können.


