---
title: "Azure-Schnellstart – Erstellen einer VM mit PowerShell | Microsoft-Dokumentation"
description: Hier lernen Sie schnell, virtuelle Linux-Computer mit PowerShell zu erstellen.
services: virtual-machines-linux
documentationcenter: virtual-machines
author: neilpeterson
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 04/03/2017
ms.author: nepeters
translationtype: Human Translation
ms.sourcegitcommit: 0d9afb1554158a4d88b7f161c62fa51c1bf61a7d
ms.openlocfilehash: bffbeb5238ec69d763e1cc7ad3c8a6e4fad34306
ms.lasthandoff: 04/12/2017


---

# <a name="create-a-linux-virtual-machine-with-powershell"></a>Erstellen eines virtuellen Linux-Computers mit PowerShell

Das Azure PowerShell-Modul dient zum Erstellen und Verwalten von Azure-Ressourcen über die PowerShell-Befehlszeile oder mit Skripts. In dieser Anleitung wird das Erstellen einer Ubuntu 14.04 LTS ausführenden Azure-VM mit PowerShell ausführlich beschrieben.

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/en-us/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.

Stellen Sie außerdem sicher, dass die neueste Version des Azure PowerShell-Moduls installiert ist. Weitere Informationen finden Sie unter [Installieren und Konfigurieren von Azure PowerShell](/powershell/azureps-cmdlets-docs).

Abschließend müssen Sie noch einen öffentlichen SSH-Schlüssel mit dem Namen `id_rsa.pub` im `.ssh`-Verzeichnis Ihres Windows-Benutzerprofils speichern. Ausführliche Informationen zum Erstellen von SSH-Schlüsseln für Azure finden Sie unter [Erstellen eines öffentlich-privaten SSH-Schlüsselpaars für virtuelle Linux-Computer](mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

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

Erstellen Sie ein virtuelles Netzwerk, ein Subnetz und eine öffentliche IP-Adresse. Diese Ressourcen dienen dazu, dem virtuellen Computer Netzwerkkonnektivität bereitzustellen und ihn mit dem Internet zu verbinden.

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

Erstellen Sie eine Netzwerksicherheitsgruppe und eine Netzwerksicherheitsgruppen-Regel. Die Netzwerksicherheitsgruppe sichert den virtuellen Computer mithilfe von Regeln für eingehenden und ausgehenden Datenverkehr. In diesem Fall wird eine Regel für eingehenden Datenverkehr für Port 22 erstellt, die eingehende SSH-Verbindungen zulässt. Wir möchten auch eine Eingangsregel für Port 80 erstellen, um eingehenden Webdatenverkehr zuzulassen.

```powershell
# Create an inbound network security group rule for port 22
$nsgRuleSSH = New-AzureRmNetworkSecurityRuleConfig -Name myNetworkSecurityGroupRuleSSH  -Protocol Tcp `
-Direction Inbound -Priority 1000 -SourceAddressPrefix * -SourcePortRange * -DestinationAddressPrefix * `
-DestinationPortRange 22 -Access Allow

# Create an inbound network security group rule for port 80
$nsgRuleWeb = New-AzureRmNetworkSecurityRuleConfig -Name myNetworkSecurityGroupRuleWWW  -Protocol Tcp `
-Direction Inbound -Priority 1001 -SourceAddressPrefix * -SourcePortRange * -DestinationAddressPrefix * `
-DestinationPortRange 80 -Access Allow

# Create a network security group
$nsg = New-AzureRmNetworkSecurityGroup -ResourceGroupName myResourceGroup -Location westeurope `
-Name myNetworkSecurityGroup -SecurityRules $nsgRuleSSH,$nsgRuleWeb
```

Erstellen Sie eine Netzwerkkarte für die VM. Die Netzwerkkarte verbindet die VM mit einem Subnetz, einer Netzwerksicherheitsgruppe und einer öffentlichen IP-Adresse.

```powershell
# Create a virtual network card and associate with public IP address and NSG
$nic = New-AzureRmNetworkInterface -Name myNic -ResourceGroupName myResourceGroup -Location westeurope `
-SubnetId $vnet.Subnets[0].Id -PublicIpAddressId $pip.Id -NetworkSecurityGroupId $nsg.Id
```

## <a name="create-virtual-machine"></a>Erstellen eines virtuellen Computers

Erstellen Sie eine VM-Konfiguration. Diese Konfiguration umfasst die beim Bereitstellen der VM verwendeten Einstellungen, z.B. ein Image des virtuellen Computers, Größe und Authentifizierungskonfiguration.

```powershell
# Define a credential object
$securePassword = ConvertTo-SecureString ' ' -AsPlainText -Force
$cred = New-Object System.Management.Automation.PSCredential ("azureuser", $securePassword)

# Create a virtual machine configuration
$vmConfig = New-AzureRmVMConfig -VMName myVM -VMSize Standard_D1 | `
Set-AzureRmVMOperatingSystem -Linux -ComputerName myVM -Credential $cred -DisablePasswordAuthentication | `
Set-AzureRmVMSourceImage -PublisherName Canonical -Offer UbuntuServer -Skus 14.04.2-LTS -Version latest | `
Add-AzureRmVMNetworkInterface -Id $nic.Id

# Configure SSH Keys
$sshPublicKey = Get-Content "$env:USERPROFILE\.ssh\id_rsa.pub"
Add-AzureRmVMSshPublicKey -VM $vmconfig -KeyData $sshPublicKey -Path "/home/azureuser/.ssh/authorized_keys"
```

Erstellen Sie den virtuellen Computer.

```powershell
New-AzureRmVM -ResourceGroupName myResourceGroup -Location westeurope -VM $vmConfig
```

## <a name="connect-to-virtual-machine"></a>Herstellen der Verbindung mit dem virtuellen Computer

Nachdem die Bereitstellung abgeschlossen ist, stellen Sie eine SSH-Verbindung mit dem virtuellen Computer her.

Führen Sie die folgenden Befehle aus, um die öffentliche IP-Adresse des virtuellen Computers zurückzugeben.

```powershell
Get-AzureRmPublicIpAddress -ResourceGroupName myResourceGroup | Select IpAddress
```

Verwenden Sie von einem System aus, auf dem SSH installiert ist, den folgenden Befehl, um die Verbindung mit dem virtuellen Computer herzustellen. Wenn Sie unter Windows arbeiten, können Sie [Putty](https://docs.microsoft.com/azure/virtual-machines/virtual-machines-linux-ssh-from-windows?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json#create-a-private-key-for-putty) verwenden, um die Verbindung herzustellen. 

```bash 
ssh <Public IP Address>
```

Geben Sie bei Aufforderung den Anmeldebenutzernamen `azureuser` ein. Wenn beim Erstellen der SSH-Schlüssel eine Passphrase eingegeben wurde, müssen Sie diese auch hier eingeben.


## <a name="install-nginx"></a>Installieren von NGINX

Verwenden Sie das folgende Bash-Skript, um Paketquellen zu aktualisieren und das neueste NGINX-Paket zu installieren. 

```bash 
#!/bin/bash

# update package source
apt-get -y update

# install NGINX
apt-get -y install nginx
```

## <a name="view-the-ngix-welcome-page"></a>Anzeigen der NGIX-Willkommensseite

Nachdem Sie NGINX installiert und Port 80 auf Ihrem virtuellen Computer für den Zugriff über das Internet geöffnet haben, können Sie nun mit einem Webbrowser Ihrer Wahl die NGINX-Standardwillkommensseite anzeigen. Verwenden Sie dabei den weiter oben dokumentierten Wert von `publicIpAddress`, um die Standardseite zu besuchen. 

![NGINX-Standardwebsite](./media/quick-create-cli/nginx.png) 
## <a name="delete-virtual-machine"></a>Löschen des virtuellen Computers

Wenn Ressourcengruppe, VM und alle zugehörigen Ressourcen nicht mehr benötigt werden, können Sie sie mit dem folgenden Befehl entfernen.

```powershell
Remove-AzureRmResourceGroup -Name myResourceGroup
```

## <a name="next-steps"></a>Nächste Schritte

[Erstellen einer vollständigen Linux-Umgebung mit der Azure CLI 2.0](create-cli-complete.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

[Azure Virtual Machine PowerShell samples](../windows/powershell-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) (Azure-VMs – PowerShell-Beispiele)

