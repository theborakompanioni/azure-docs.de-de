---
title: Konfigurieren von MSI auf einem virtuellen Azure-Computer mit PowerShell
description: "Schrittweise Anweisungen zum Konfigurieren einer verwalteten Dienstidentität (Managed Service Identity, MSI) auf einem virtuellen Azure-Computer mit PowerShell."
services: active-directory
documentationcenter: 
author: bryanla
manager: mbaldwin
editor: 
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 09/14/2017
ms.author: bryanla
ms.translationtype: HT
ms.sourcegitcommit: 47ba7c7004ecf68f4a112ddf391eb645851ca1fb
ms.openlocfilehash: 104c43e6fab2c3f18824a00860c30c8d6f82bbc4
ms.contentlocale: de-de
ms.lasthandoff: 09/14/2017

---

# <a name="configure-a-vm-managed-service-identity-msi-using-powershell"></a>Konfigurieren einer VM-MSI (Managed Service Identity, verwaltete Dienstidentität) mit PowerShell

[!INCLUDE[preview-notice](../../includes/active-directory-msi-preview-notice.md)]

Eine verwaltete Dienstidentität stellt für Azure-Dienste eine automatisch verwaltete Identität in Azure Active Directory bereit. Sie können diese Identität für die Authentifizierung bei jedem Dienst verwenden, der die Azure AD-Authentifizierung unterstützt. Hierfür müssen keine Anmeldeinformationen im Code enthalten sein. 

In diesem Artikel erfahren Sie, wie Sie MSI für einen virtuellen Azure-Windows-Computer mit PowerShell aktivieren und entfernen.

## <a name="prerequisites"></a>Voraussetzungen

[!INCLUDE [msi-qs-configure-prereqs](../../includes/msi-qs-configure-prereqs.md)]

Installieren Sie [Azure PowerShell, Version 4.3.1](https://www.powershellgallery.com/packages/AzureRM/4.3.1), sofern noch nicht geschehen.

## <a name="enable-msi-during-creation-of-an-azure-vm"></a>Aktivieren von MSI beim Erstellen eines virtuellen Azure-Computers

Eine neue, für MSI aktivierte virtuelle Windows-Computerressource wird in einer neuen Ressourcengruppe mit den angegebenen Konfigurationsparametern erstellt. Beachten Sie außerdem, dass viele dieser Cmdlets möglicherweise 30 Sekunden oder länger ausgeführt werden, bevor ein Ergebnis zurückgegeben wird. Daher kann die Erstellung eines virtuellen Computers letztlich mehrere Minuten in Anspruch nehmen.

1. Melden Sie sich mit `Login-AzureRmAccount` bei Azure an. Verwenden Sie ein Konto, das dem Azure-Abonnement zugeordnet ist, unter dem Sie den virtuellen Computer bereitstellen möchten.

   ```powershell
   Login-AzureRmAccount
   ```

2. Erstellen Sie eine [Ressourcengruppe](../azure-resource-manager/resource-group-overview.md#terminology) für das Einschließen und Bereitstellen des virtuellen Computers und der zugehörigen Ressourcen. Verwenden Sie hierfür das `New-AzureRmResourceGroup`-Cmdlet. Sie können diesen Schritt überspringen, wenn Sie bereits über eine Ressourcengruppe verfügen, die Sie stattdessen verwenden möchten:

   ```powershell
   New-AzureRmResourceGroup -Name myResourceGroup -Location WestUS
   ```
3. Erstellen Sie Netzwerkressourcen für den virtuellen Computer.

   a. Erstellen Sie ein virtuelles Netzwerk, ein Subnetz und eine öffentliche IP-Adresse. Diese Ressourcen dienen dazu, dem virtuellen Computer Netzwerkkonnektivität bereitzustellen und ihn mit dem Internet zu verbinden.

   ```powershell
   # Create a subnet configuration
   $subnetConfig = New-AzureRmVirtualNetworkSubnetConfig -Name mySubnet -AddressPrefix 192.168.1.0/24

   # Create a virtual network
   $vnet = New-AzureRmVirtualNetwork -ResourceGroupName myResourceGroup -Location WestUS -Name MYvNET -AddressPrefix 192.168.0.0/16 -Subnet $subnetConfig

   # Create a public IP address and specify a DNS name
   $pip = New-AzureRmPublicIpAddress -ResourceGroupName myResourceGroup -Location WestUS -AllocationMethod Static -IdleTimeoutInMinutes 4 -Name "mypublicdns$(Get-Random)"
   ```

   b. Erstellen Sie eine Netzwerksicherheitsgruppe und eine Netzwerksicherheitsgruppen-Regel. Die Netzwerksicherheitsgruppe sichert den virtuellen Computer mithilfe von Regeln für eingehenden und ausgehenden Datenverkehr. In diesem Fall wird eine Regel für eingehenden Datenverkehr für Port 3389 erstellt, die eingehende Remotedesktopverbindungen zulässt. Wir möchten auch eine Eingangsregel für Port 80 erstellen, um eingehenden Webdatenverkehr zuzulassen:

   ```powershell
   # Create an inbound network security group rule for port 3389
   $nsgRuleRDP = New-AzureRmNetworkSecurityRuleConfig -Name myNetworkSecurityGroupRuleRDP  -Protocol Tcp -Direction Inbound -Priority 1000 -SourceAddressPrefix * -SourcePortRange * -DestinationAddressPrefix * -DestinationPortRange 3389 -Access Allow

   # Create an inbound network security group rule for port 80
   $nsgRuleWeb = New-AzureRmNetworkSecurityRuleConfig -Name myNetworkSecurityGroupRuleWWW  -Protocol Tcp -Direction Inbound -Priority 1001 -SourceAddressPrefix * -SourcePortRange * -DestinationAddressPrefix * -DestinationPortRange 80 -Access Allow

   # Create a network security group
   $nsg = New-AzureRmNetworkSecurityGroup -ResourceGroupName myResourceGroup -Location WestUS -Name myNetworkSecurityGroup -SecurityRules $nsgRuleRDP,$nsgRuleWeb
   ```

   c. Erstellen Sie eine virtuelle Netzwerkkarte für den virtuellen Computer. Die Netzwerkkarte verbindet den virtuellen Computer mit einem Subnetz, einer Netzwerksicherheitsgruppe und einer öffentlichen IP-Adresse:

   ```powershell
   # Create a virtual network card and associate with public IP address and NSG
   $nic = New-AzureRmNetworkInterface -Name myNic -ResourceGroupName myResourceGroup -Location WestUS -SubnetId $vnet.Subnets[0].Id -PublicIpAddressId $pip.Id -NetworkSecurityGroupId $nsg.Id
   ```

4. Erstellen Sie den virtuellen Computer.

   a. Erstellen Sie ein konfigurierbares VM-Objekt. Diese Einstellungen werden beim Bereitstellen des virtuellen Computers verwendet, z.B. ein Image des virtuellen Computers, Größe und Authentifizierungskonfiguration. Der `-IdentityType "SystemAssigned"`-Parameter, der im Cmdlet [New-AzureRmVMConfig](/powershell/module/azurerm.compute/new-azurermvm) verwendet wird, führt dazu, dass eine MSI für den virtuellen Computer bereitgestellt wird. Das Cmdlet `Get-Credential` fordert Sie zur Eingabe von Anmeldeinformationen auf. Diese werden als Benutzername und Kennwort für den virtuellen Computer konfiguriert:

   ```powershell
   # Define a credential object (prompts for user/password to be used for VM authentication)
   $cred = Get-Credential

   # Create a configurable VM object with a Managed Service Identity
   $vmConfig = New-AzureRmVMConfig -VMName myVM -VMSize Standard_DS2 -IdentityType "SystemAssigned" | Set-AzureRmVMOperatingSystem -Windows -ComputerName myVM -Credential $cred | Set-AzureRmVMSourceImage -PublisherName MicrosoftWindowsServer -Offer WindowsServer -Skus 2016-Datacenter -Version latest | Add-AzureRmVMNetworkInterface -Id $nic.Id
   ```

   b. Bereitstellen des neuen virtuellen Computers:

   ```powershell
   New-AzureRmVM -ResourceGroupName myResourceGroup -Location WestUS -VM $vmConfig
   ```

5. Fügen Sie die MSI-VM-Erweiterung hinzu, indem Sie den `-Type "ManagedIdentityExtensionForWindows"`-Parameter im Cmdlet [Set-AzureRmVMExtension](/powershell/module/azurerm.compute/set-azurermvmextension) verwenden. Der `-Settings`-Parameter gibt den Port an, der vom OAuth-Token-Endpunkt für den Tokenabruf verwendet wird:

   ```powershell
   $settings = @{ "port" = 50342 }
   Set-AzureRmVMExtension -ResourceGroupName myResourceGroup -Location WestUS -VMName myVM -Name "ManagedIdentityExtensionForWindows" -Type "ManagedIdentityExtensionForWindows" -Publisher "Microsoft.ManagedIdentity" -TypeHandlerVersion "1.0" -Settings $settings 
   ```

## <a name="enable-msi-on-an-existing-azure-vm"></a>Aktivieren von MSI auf einem vorhandenen virtuellen Azure-Computer

Wenn Sie MSI auf einem vorhandenen virtuellen Computer aktivieren möchten, gehen Sie wie folgt vor:

1. Melden Sie sich mit `Login-AzureRmAccount` bei Azure an. Verwenden Sie ein Konto, das dem Azure-Abonnement zugeordnet ist, unter dem Sie den virtuellen Computer bereitstellen möchten:

   ```powershell
   Login-AzureRmAccount
   ```

2. Rufen Sie zunächst die VM-Einstellungen mithilfe des Cmdlets `Get-AzureRmVM` ab. Verwenden Sie dann zum Aktivieren von MSI den `-IdentityType`-Switch im Cmdlet [Update-AzureRmVM](/powershell/module/azurerm.compute/update-azurermvm):

   ```powershell
   $vm = Get-AzureRmVM -ResourceGroupName myResourceGroup -Name myVM
   Update-AzureRmVM -ResourceGroupName myResourceGroup -VM $vm -IdentityType "SystemAssigned"
   ```

3. Fügen Sie die MSI-VM-Erweiterung hinzu, indem Sie den `-Type "ManagedIdentityExtensionForWindows"`-Parameter im Cmdlet [Set-AzureRmVMExtension](/powershell/module/azurerm.compute/set-azurermvmextension) verwenden. Der `-Settings`-Parameter gibt den Port an, der vom OAuth-Token-Endpunkt für den Tokenabruf verwendet wird. Achten Sie darauf, den richtigen `-Location`-Parameter anzugeben, der dem Speicherort des vorhandenen virtuellen Computers entspricht:

   ```powershell
   $settings = @{ "port" = 50342 }
   Set-AzureRmVMExtension -ResourceGroupName myResourceGroup -Location WestUS -VMName myVM -Name "ManagedIdentityExtensionForWindows" -Type "ManagedIdentityExtensionForWindows" -Publisher "Microsoft.ManagedIdentity" -TypeHandlerVersion "1.0" -Settings $settings 
   ```

## <a name="remove-msi-from-an-azure-vm"></a>Entfernen von MSI von einem virtuellen Azure-Computer

Wenn MSI auf einem virtuellen Computer nicht mehr benötigt wird, können Sie das Cmdlet `RemoveAzureRmVMExtension` verwenden, um MSI von diesem virtuellen Computer zu entfernen:

1. Verwenden Sie den `-Name "ManagedIdentityExtensionForWindows"`-Switch mit dem Cmdlet [Remove-AzureRmVMExtension](/powershell/module/azurerm.compute/remove-azurermvmextension):

   ```powershell
   Remove-AzureRmVMExtension -ResourceGroupName myResourceGroup -Name "ManagedIdentityExtensionForWindows" -VMName myVM
   ```

## <a name="related-content"></a>Verwandte Inhalte

- [Übersicht über verwaltete Dienstidentitäten](msi-overview.md)
- Dieser Artikel basiert auf dem Schnellstart [Erstellen eines virtuellen Windows-Computers mit PowerShell](../virtual-machines/windows/quick-create-powershell.md) und wurde durch MSI-spezifische Anweisungen ergänzt. 

Verwenden Sie den folgenden Kommentarabschnitt, um uns Feedback zu senden und uns bei der Verbesserung unserer Inhalte zu unterstützen.

















