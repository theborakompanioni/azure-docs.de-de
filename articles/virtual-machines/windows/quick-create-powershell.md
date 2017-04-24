---
title: "Azure-Schnellstart – Erstellen einer Windows-VM mit PowerShell | Microsoft-Dokumentation"
description: Hier lernen Sie schnell, virtuelle Windows-Computer mit PowerShell zu erstellen.
services: virtual-machines-windows
documentationcenter: virtual-machines
author: neilpeterson
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 04/03/2017
ms.author: nepeters
translationtype: Human Translation
ms.sourcegitcommit: 0d9afb1554158a4d88b7f161c62fa51c1bf61a7d
ms.openlocfilehash: 91ef7f432d0954cc8456e5d98c48943aa0ad72a7
ms.lasthandoff: 04/12/2017


---

# <a name="create-a-windows-virtual-machine-with-powershell"></a>Erstellen eines virtuellen Windows-Computers mit PowerShell

Das Azure PowerShell-Modul dient zum Erstellen und Verwalten von Azure-Ressourcen über die PowerShell-Befehlszeile oder mit Skripts. In dieser Anleitung wird das Erstellen einer Windows Server 2016 ausführenden Azure-VM mit PowerShell ausführlich beschrieben.  Nach Abschluss der Bereitstellung stellen wir eine Verbindung mit dem Server her und installieren IIS.  

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/en-us/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.

Stellen Sie außerdem sicher, dass die neueste Version des Azure PowerShell-Moduls installiert ist. Weitere Informationen finden Sie unter [Installieren und Konfigurieren von Azure PowerShell](/powershell/azureps-cmdlets-docs).

## <a name="log-in-to-azure"></a>Anmelden an Azure

Melden Sie sich mit dem Befehl `Login-AzureRmAccount` bei Ihrem Azure-Abonnement an, und befolgen Sie die Anweisungen auf dem Bildschirm.

```powershell
Login-AzureRmAccount
```

## <a name="create-resource-group"></a>Ressourcengruppe erstellen

Erstellen Sie eine Azure-Ressourcengruppe. Eine Ressourcengruppe ist ein logischer Container, in dem Azure-Ressourcen bereitgestellt und verwaltet werden. 

```powershell
New-AzureRmResourceGroup -Name myResourceGroup -Location westeurope
```

## <a name="create-networking-resources"></a>Erstellen von Netzwerkressourcen

### <a name="create-a-virtual-network-subnet-and-a-public-ip-address"></a>Erstellen Sie ein virtuelles Netzwerk, ein Subnetz und eine öffentliche IP-Adresse. 
Diese Ressourcen dienen dazu, dem virtuellen Computer Netzwerkkonnektivität bereitzustellen und ihn mit dem Internet zu verbinden.

```powershell
# Create a subnet configuration
$subnetConfig = New-AzureRmVirtualNetworkSubnetConfig -Name mySubnet -AddressPrefix 192.168.1.0/24

# Create a virtual network
$vnet = New-AzureRmVirtualNetwork -ResourceGroupName myResourceGroup -Location westeurope `
-Name MYvNET -AddressPrefix 192.168.0.0/16 -Subnet $subnetConfig

# Create a public IP address and specify a DNS name
$pip = New-AzureRmPublicIpAddress -ResourceGroupName myResourceGroup -Location westeurope `
-AllocationMethod Static -IdleTimeoutInMinutes 4 -Name "mypublicdns$(Get-Random)"
```

### <a name="create-a-network-security-group-and-a-network-security-group-rule"></a>Erstellen Sie eine Netzwerksicherheitsgruppe und eine Netzwerksicherheitsgruppen-Regel. 
Die Netzwerksicherheitsgruppe sichert den virtuellen Computer mithilfe von Regeln für eingehenden und ausgehenden Datenverkehr. In diesem Fall wird eine Regel für eingehenden Datenverkehr für Port 3389 erstellt, die eingehende Remotedesktopverbindungen zulässt.  Wir möchten auch eine Eingangsregel für Port 80 erstellen, um eingehenden Webdatenverkehr zuzulassen.

```powershell
# Create an inbound network security group rule for port 3389
$nsgRuleRDP = New-AzureRmNetworkSecurityRuleConfig -Name myNetworkSecurityGroupRuleRDP  -Protocol Tcp `
-Direction Inbound -Priority 1000 -SourceAddressPrefix * -SourcePortRange * -DestinationAddressPrefix * `
-DestinationPortRange 3389 -Access Allow

# Create an inbound network security group rule for port 80
$nsgRuleWeb = New-AzureRmNetworkSecurityRuleConfig -Name myNetworkSecurityGroupRuleWWW  -Protocol Tcp `
-Direction Inbound -Priority 1001 -SourceAddressPrefix * -SourcePortRange * -DestinationAddressPrefix * `
-DestinationPortRange 80 -Access Allow

# Create a network security group
$nsg = New-AzureRmNetworkSecurityGroup -ResourceGroupName myResourceGroup -Location westeurope `
-Name myNetworkSecurityGroup -SecurityRules $nsgRuleRDP,$nsgRuleWeb
```

### <a name="create-a-network-card-for-the-virtual-machine"></a>Erstellen Sie eine Netzwerkkarte für die VM. 
Die Netzwerkkarte verbindet die VM mit einem Subnetz, einer Netzwerksicherheitsgruppe und einer öffentlichen IP-Adresse.

```powershell
# Create a virtual network card and associate with public IP address and NSG
$nic = New-AzureRmNetworkInterface -Name myNic -ResourceGroupName myResourceGroup -Location westeurope `
-SubnetId $vnet.Subnets[0].Id -PublicIpAddressId $pip.Id -NetworkSecurityGroupId $nsg.Id
```

## <a name="create-virtual-machine"></a>Erstellen eines virtuellen Computers

Erstellen Sie eine VM-Konfiguration. Diese Konfiguration umfasst die beim Bereitstellen der VM verwendeten Einstellungen, z.B. ein Image des virtuellen Computers, Größe und Authentifizierungskonfiguration. Wenn Sie diesen Schritt ausführen, werden Sie aufgefordert, Anmeldeinformationen einzugeben. Die Werte, die Sie eingeben, werden als Benutzername und Kennwort für den virtuellen Computer konfiguriert.

```powershell
# Define a credential object
$cred = Get-Credential

# Create a virtual machine configuration
$vmConfig = New-AzureRmVMConfig -VMName myVM -VMSize Standard_DS2 | `
Set-AzureRmVMOperatingSystem -Windows -ComputerName myVM -Credential $cred | `
Set-AzureRmVMSourceImage -PublisherName MicrosoftWindowsServer -Offer WindowsServer -Skus 2016-Datacenter -Version latest | `
Add-AzureRmVMNetworkInterface -Id $nic.Id
```

Erstellen Sie den virtuellen Computer.

```powershell
New-AzureRmVM -ResourceGroupName myResourceGroup -Location westeurope -VM $vmConfig
```

## <a name="connect-to-virtual-machine"></a>Herstellen der Verbindung mit dem virtuellen Computer

Nachdem die Bereitstellung abgeschlossen ist, stellen Sie eine Remotedesktopverbindung mit dem virtuellen Computer her.

Führen Sie die folgenden Befehle aus, um die öffentliche IP-Adresse des virtuellen Computers zurückzugeben.  Notieren Sie sich diese IP-Adresse. Sie wird in einem späteren Schritt über den Browser aufgerufen, um die Webkonnektivität zu testen.

```powershell
Get-AzureRmPublicIpAddress -ResourceGroupName myResourceGroup | Select IpAddress
```

Erstellen Sie mit dem folgenden Befehl eine Remotedesktopsitzung mit dem virtuellen Computer. Ersetzen Sie die IP-Adresse durch den Wert von `publicIPAddress` Ihres virtuellen Computers. Geben Sie nach Aufforderung die beim Erstellen des virtuellen Computers verwendeten Anmeldeinformationen ein.

```bash 
mstsc /v:<publicIpAddress>
```

## <a name="install-iis-via-powershell"></a>Installieren von IIS mithilfe von PowerShell

Nach der Anmeldung bei dem virtuellen Computer können Sie mit einer einzelnen PowerShell-Codezeile IIS installieren und die lokale Firewallregel aktivieren, um Webdatenverkehr zuzulassen.  Öffnen Sie eine PowerShell-Eingabeaufforderung, und führen Sie den folgenden Befehl aus:

```powershell
Install-WindowsFeature -name Web-Server -IncludeManagementTools
```

## <a name="view-the-iis-welcome-page"></a>Anzeigen der IIS-Willkommensseite

Nachdem Sie IIS installiert und Port 80 auf Ihrem virtuellen Computer für den Zugriff über das Internet geöffnet haben, können Sie nun mit einem Webbrowser Ihrer Wahl die IIS-Standardwillkommensseite anzeigen. Verwenden Sie dabei den weiter oben dokumentierten Wert von `publicIpAddress`, um die Standardseite zu besuchen. 

![IIS-Standardwebsite](./media/quick-create-powershell/default-iis-website.png) 

## <a name="delete-virtual-machine"></a>Löschen des virtuellen Computers

Wenn Ressourcengruppe, VM und alle zugehörigen Ressourcen nicht mehr benötigt werden, können Sie sie mit dem folgenden Befehl entfernen.

```powershell
Remove-AzureRmResourceGroup -Name myResourceGroup
```

## <a name="next-steps"></a>Nächste Schritte

[Experimentieren mit der Installation einer Rolle auf der Windows-VM](hero-role.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)

[Azure Virtual Machine PowerShell samples](powershell-samples.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) (Azure-VMs – PowerShell-Beispiele)
