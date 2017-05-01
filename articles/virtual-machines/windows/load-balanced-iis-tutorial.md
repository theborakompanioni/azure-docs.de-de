---
title: "Tutorial: Erstellen einer hochverfügbaren Anwendung auf virtuellen Azure-Computern | Microsoft-Dokumentation"
description: "Hier erfahren Sie, wie Sie eine hochverfügbare und sichere Anwendung für drei virtuelle Windows-Computer mit einem Load Balancer in Azure erstellen."
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
ms.date: 03/30/2017
ms.author: davidmu
translationtype: Human Translation
ms.sourcegitcommit: 303cb9950f46916fbdd58762acd1608c925c1328
ms.openlocfilehash: ae0fcbf3e8ba28f104e5b44ebe5676875065e2e9
ms.lasthandoff: 04/04/2017

---

# <a name="build-a-load-balanced-highly-available-application-on-windows-virtual-machines-in-azure"></a>Erstellen einer hochverfügbaren Anwendung mit Load Balancer auf virtuellen Windows-Computern in Azure

In diesem Tutorial erstellen Sie eine hoch verfügbare, wartungsresistente Anwendung. Die App verwendet einen Load Balancer, eine Verfügbarkeitsgruppe und drei virtuelle Windows-Computer (VMs). In diesem Tutorial wird zwar IIS installiert, Sie können das Tutorial aber auch zum Bereitstellen eines anderen Anwendungsframeworks mit den gleichen Hochverfügbarkeitskomponenten und Richtlinien verwenden. 

## <a name="step-1---azure-prerequisites"></a>Schritt 1: Azure-Voraussetzungen

Für dieses Tutorial muss die aktuelle Version des Moduls [Azure PowerShell](https://docs.microsoft.com/powershell/azureps-cmdlets-docs/) installiert sein.

Melden Sie sich zuerst mit dem Befehl Login-AzureRmAccount bei Ihrem Azure-Abonnement an, und befolgen Sie die Anweisungen auf dem Bildschirm.

```powershell
Login-AzureRmAccount
```

Eine Azure-Ressourcengruppe ist ein logischer Container, in dem Azure-Ressourcen bereitgestellt und verwaltet werden. Vor der Erstellung anderer Azure-Ressourcen müssen Sie zunächst mit [New-AzureRmResourceGroup](https://docs.microsoft.com/powershell/resourcemanager/AzureRM.Resources/v2.0.3/new-azurermresourcegroup) eine Ressourcengruppe erstellen. Das folgende Beispiel erstellt eine Ressourcengruppe mit dem Namen `myResourceGroup` in der Region `westeurope`: 

```powershell
New-AzureRmResourceGroup -ResourceGroupName myResourceGroup -Location westeurope
```

## <a name="step-2---create-availability-set"></a>Schritt 2: Erstellen der Verfügbarkeitsgruppe

Virtuelle Computer können übergreifend für logische Fehler- und Updatedomänen erstellt werden. Jede logische Domäne stellt einen Teil der Hardware im zugrunde liegenden Azure-Datencenter dar. Wenn Sie mehrere virtuelle Computer erstellen, werden Ihre Compute- und Speicherressourcen auf diese Domänen verteilt. Diese Verteilung gewährleistet die Verfügbarkeit Ihrer App, wenn eine der Hardwarekomponenten gewartet werden muss. Diese logischen Fehler- und Updatedomänen können mithilfe von Verfügbarkeitsgruppen definiert werden.

Erstellen Sie mithilfe von [New-AzureRmAvailabilitySet](https://docs.microsoft.com/powershell/resourcemanager/azurerm.compute/v2.8.0/new-azurermavailabilityset) eine Verfügbarkeitsgruppe. Im folgenden Beispiel wird eine Verfügbarkeitsgruppe namens `myAvailabilitySet` erstellt:

```powershell
$availabilitySet = New-AzureRmAvailabilitySet `
  -ResourceGroupName myResourceGroup `
  -Name myAvailabilitySet `
  -Location westeurope `
  -Managed `
  -PlatformFaultDomainCount 3 `
  -PlatformUpdateDomainCount 2
```

## <a name="step-3---create-load-balancer"></a>Schritt 3: Erstellen des Load Balancers

Ein Azure Load Balancer verteilt Datenverkehr auf der Grundlage von Load Balancer-Regeln auf eine Gruppe definierter virtueller Computer. Ein Integritätstest überwacht einen bestimmten Port auf jedem virtuellen Computer und verteilt Datenverkehr nur an einen betriebsbereiten virtuellen Computer.

### <a name="create-public-ip-address"></a>Erstellen einer öffentlichen IP-Adresse

Um über das Internet auf Ihre App zugreifen zu können, müssen Sie dem Load Balancer eine öffentliche IP-Adresse zuweisen. Erstellen Sie mit [New-AzureRmPublicIpAddress](https://docs.microsoft.com/powershell/resourcemanager/azurerm.network/v3.6.0/new-azurermpublicipaddress) eine öffentliche IP-Adresse. Im folgenden Beispiel wird eine öffentliche IP-Adresse namens `myPublicIP` erstellt:

```powershell
$pip = New-AzureRmPublicIpAddress `
  -ResourceGroupName myResourceGroup `
  -Location westeurope `
  -AllocationMethod Static `
  -Name myPublicIP
```

### <a name="create-load-balancer"></a>Erstellen eines Load Balancers

Erstellen Sie eine Front-End-IP-Adresse mit [New-AzureRmLoadBalancerFrontendIpConfig](https://docs.microsoft.com/powershell/resourcemanager/azurerm.network/v3.6.0/new-azurermloadbalancerfrontendipconfig). Im folgenden Beispiel wird eine Front-End-IP-Adresse namens `myFrontEndPool` erstellt: 

```powershell
$frontendIP = New-AzureRmLoadBalancerFrontendIpConfig -Name myFrontEndPool -PublicIpAddress $pip
```

Erstellen Sie einen Back-End-Adresspool mit [New-AzureRmLoadBalancerBackendAddressPoolConfig](https://docs.microsoft.com/powershell/resourcemanager/azurerm.network/v3.6.0/new-azurermloadbalancerbackendaddresspoolconfig). Im folgenden Beispiel wird ein Back-End-Adresspool namens `myBackEndPool` erstellt:

```powershell
$backendPool = New-AzureRmLoadBalancerBackendAddressPoolConfig -Name myBackEndPool
```

Erstellen Sie nun mit [New-AzureRmLoadBalancer](https://docs.microsoft.com/powershell/resourcemanager/azurerm.network/v3.6.0/new-azurermloadbalancer) den Load Balancer. Im folgenden Beispiel wird unter Verwendung der `myPublicIP`-Adresse ein Load Balancer namens `myLoadBalancer` erstellt:

```powershell
$lb = New-AzureRmLoadBalancer `
  -ResourceGroupName myResourceGroup `
  -Name myLoadBalancer `
  -Location westeurope `
  -FrontendIpConfiguration $frontendIP `
  -BackendAddressPool $backendPool
```

### <a name="create-health-probe"></a>Erstellen eines Integritätstests

Damit der Load Balancer den Status Ihrer App überwachen kann, verwenden Sie einen Integritätstest. Abhängig von der Reaktion auf Integritätsüberprüfungen werden der Load Balancer-Rotation durch den Integritätstest dynamisch virtuelle Computer hinzugefügt oder daraus entfernt. Falls bei einem virtuellen Computer innerhalb eines Intervalls von 15 Sekunden zwei aufeinanderfolgende Fehler auftreten, wird er standardmäßig aus der Load Balancer-Verteilung entfernt.

Erstellen Sie mit [Add-AzureRmLoadBalancerProbeConfig](https://docs.microsoft.com/powershell/resourcemanager/azurerm.network/v3.6.0/add-azurermloadbalancerprobeconfig) einen Integritätstest. Im folgenden Beispiel wird ein benannter Integritätstest namens `myHealthProbe` erstellt, der die einzelnen virtuellen Computer überwacht:

```powershell
Add-AzureRmLoadBalancerProbeConfig -Name myHealthProbe `
  -LoadBalancer $lb `
  -Protocol tcp `
  -Port 80 `
  -IntervalInSeconds 15 `
  -ProbeCount 2
```

### <a name="create-load-balancer-rule"></a>Erstellen einer Load Balancer-Regel

Mithilfe einer Load Balancer-Regel wird definiert, wie Datenverkehr auf die virtuellen Computer verteilt werden soll.

Erstellen Sie mit [Add-AzureRmLoadBalancerRuleConfig](https://docs.microsoft.com/powershell/resourcemanager/AzureRM.Network/v3.6.0/add-azurermloadbalancerruleconfig) eine Load Balancer-Regel. Im folgenden Beispiel wird eine Load Balancer-Regel mit dem Namen `myLoadBalancerRule` erstellt und der Datenverkehr an Port `80` ausgeglichen:

```powershell
Add-AzureRmLoadBalancerRuleConfig -Name myLoadBalancerRule `
  -LoadBalancer $lb `
  -FrontendIpConfiguration $lb.FrontendIpConfigurations[0] `
  -BackendAddressPool $lb.BackendAddressPools[0] `
  -Protocol Tcp `
  -FrontendPort 80 `
  -BackendPort 80
```

Aktualisieren Sie den Load Balancer mit [Set-AzureRmLoadBalancer](https://docs.microsoft.com/powershell/resourcemanager/azurerm.network/v3.6.0/set-azurermloadbalancer):

```powershell
Set-AzureRmLoadBalancer -LoadBalancer $lb
```

## <a name="step-4---configure-networking"></a>Schritt 4: Konfigurieren des Netzwerks

Jeder virtuelle Computer verfügt über mindestens eine virtuelle Netzwerkschnittstellenkarte (Network Interface Card, NIC), die eine Verbindung mit einem virtuellen Netzwerk herstellt. Dieses virtuelle Netzwerk ist geschützt, um Datenverkehr auf der Grundlage definierter Zugriffsregeln zu filtern.

### <a name="create-virtual-network"></a>Virtuelles Netzwerk erstellen

Erstellen Sie zuerst mit [New-AzureRmVirtualNetworkSubnetConfig](https://docs.microsoft.com/powershell/resourcemanager/azurerm.network/v3.6.0/new-azurermvirtualnetworksubnetconfig) ein Subnetz. Im folgenden Beispiel wird ein Subnetz namens `mySubnet` erstellt:

```powershell
$subnetConfig = New-AzureRmVirtualNetworkSubnetConfig -Name mySubnet -AddressPrefix 192.168.1.0/24
```

Erstellen Sie mit [New-AzureRmVirtualNetwork](https://docs.microsoft.com/powershell/resourcemanager/azurerm.network/v3.6.0/new-azurermvirtualnetwork) ein virtuelles Netzwerk, um Netzwerkkonnektivität für Ihre virtuellen Computer bereitzustellen. Im folgenden Beispiel wird ein virtuelles Netzwerk namens `myVnet` mit `mySubnet` erstellt:

```powershell
$vnet = New-AzureRmVirtualNetwork `
  -ResourceGroupName myResourceGroup `
  -Location westeurope `
  -Name myVnet `
  -AddressPrefix 192.168.0.0/16 `
  -Subnet $subnetConfig
```

### <a name="configure-network-security"></a>Konfigurieren der Netzwerksicherheit

Eine Azure[Netzwerksicherheitsgruppe](../../virtual-network/virtual-networks-nsg.md) (NSG) steuert ein- und ausgehenden Datenverkehr für einen oder mehrere virtuelle Computer. Netzwerksicherheitsgruppen-Regeln dienen zum Zulassen oder Verweigern von Netzwerkdatenverkehr an einem bestimmten Port oder in einem Portbereich. Diese Regeln können auch ein Quelladresspräfix enthalten, sodass nur Datenverkehr aus einer vordefinierten Quelle mit einem virtuellen Computer kommunizieren kann.

Erstellen Sie mit [New-AzureRmNetworkSecurityRuleConfig](https://docs.microsoft.com/powershell/resourcemanager/azurerm.network/v3.6.0/new-azurermnetworksecurityruleconfig) eine Netzwerksicherheitsgruppen-Regel, damit Ihre App für Webdatenverkehr erreichbar ist. Im folgenden Beispiel wird eine Netzwerksicherheitsgruppen-Regel namens `myNetworkSecurityGroupRule` erstellt:

```powershell
$nsgRule = New-AzureRmNetworkSecurityRuleConfig `
  -Name myNetworkSecurityGroupRule `
  -Protocol Tcp `
  -Direction Inbound `
  -Priority 1001 `
  -SourceAddressPrefix * `
  -SourcePortRange * `
  -DestinationAddressPrefix * `
  -DestinationPortRange 80 `
  -Access Allow
```

Erstellen Sie eine Netzwerksicherheitsgruppe mit [New-AzureRmNetworkSecurityGroup](https://docs.microsoft.com/powershell/resourcemanager/azurerm.network/v3.6.0/new-azurermnetworksecuritygroup). Im folgenden Beispiel wird eine NSG (Network Security Group, Netzwerksicherheitsgruppe) namens `myNetworkSecurityGroup` erstellt:

```powershell
$nsg = New-AzureRmNetworkSecurityGroup `
  -ResourceGroupName myResourceGroup `
  -Location westeurope `
  -Name myNetworkSecurityGroup `
  -SecurityRules $nsgRule
```

Fügen Sie mit [Set-AzureRmVirtualNetworkSubnetConfig](https://docs.microsoft.com/powershell/resourcemanager/azurerm.network/v3.6.0/set-azurermvirtualnetworksubnetconfig) die Netzwerksicherheitsgruppe dem Subnetz hinzu:

```powershell
Set-AzureRmVirtualNetworkSubnetConfig -VirtualNetwork $vnet `
  -Name mySubnet `
  -NetworkSecurityGroup $nsg `
  -AddressPrefix 192.168.1.0/24
```

Aktualisieren Sie mit [Set-AzureRmVirtualNetwork](https://docs.microsoft.com/powershell/resourcemanager/azurerm.network/v3.6.0/set-azurermvirtualnetwork) das virtuelle Netzwerk:

```powershell
Set-AzureRmVirtualNetwork -VirtualNetwork $vnet
```

### <a name="create-virtual-network-interface-cards"></a>Erstellen virtueller Netzwerkschnittstellenkarten

Load Balancer arbeiten mit der virtuellen NIC-Ressource und nicht mit dem eigentlichen virtuellen Computer. Die virtuelle NIC wird mit dem Load Balancer verbunden und anschließend an einen virtuellen Computer angefügt.

Erstellen Sie eine virtuelle NIC mit [New-AzureRmNetworkInterface](https://docs.microsoft.com/powershell/resourcemanager/azurerm.network/v3.6.0/new-azurermnetworkinterface). Im folgenden Beispiel werden drei virtuelle NICs erstellt (jeweils eine virtuelle NIC pro virtuellem Computer, den Sie in den folgenden Schritten für Ihre App erstellen):


```powershell
for ($i=1; $i -le 3; $i++)
{
   New-AzureRmNetworkInterface -ResourceGroupName myResourceGroup `
     -Name myNic$i `
     -Location westeurope `
     -Subnet $vnet.Subnets[0] `
     -LoadBalancerBackendAddressPool $lb.BackendAddressPools[0]
}

```

## <a name="step-5---create-virtual-machines"></a>Schritt 5: Erstellen von virtuellen Computern

Nachdem alle zugrunde liegenden Komponenten eingerichtet sind, können Sie nun hochverfügbare virtuelle Computer zur Ausführung Ihrer App erstellen. 

Rufen Sie mit [Get-Credential](https://msdn.microsoft.com/powershell/reference/5.1/microsoft.powershell.security/Get-Credential) den Benutzernamen und das Kennwort für das Administratorkonto auf dem virtuellen Computer ab:

```powershell
$cred = Get-Credential
```

Erstellen Sie die VMs mit [New-AzureRmVMConfig](https://docs.microsoft.com/powershell/resourcemanager/azurerm.compute/v2.8.0/new-azurermvmconfig), [Set-AzureRmVMOperatingSystem](https://docs.microsoft.com/powershell/resourcemanager/azurerm.compute/v2.8.0/set-azurermvmoperatingsystem), [Set-AzureRmVMSourceImage](https://docs.microsoft.com/powershell/resourcemanager/azurerm.compute/v2.8.0/set-azurermvmsourceimage), [Set-AzureRmVMOSDisk](https://docs.microsoft.com/powershell/resourcemanager/azurerm.compute/v2.8.0/set-azurermvmosdisk), [Add-AzureRmVMNetworkInterface](https://docs.microsoft.com/powershell/resourcemanager/azurerm.compute/v2.8.0/add-azurermvmnetworkinterface) und [New-AzureRmVM](https://docs.microsoft.com/powershell/resourcemanager/azurerm.compute/v2.8.0/new-azurermvm). Im folgenden Beispiel werden drei VMs erstellt:

```powershell
for ($i=1; $i -le 3; $i++)
{
   $vm = New-AzureRmVMConfig -VMName myVM$i -VMSize Standard_D1 -AvailabilitySetId $availabilitySet.Id
   $vm = Set-AzureRmVMOperatingSystem -VM $vm -Windows -ComputerName myVM$i -Credential $cred -ProvisionVMAgent -EnableAutoUpdate
   $vm = Set-AzureRmVMSourceImage -VM $vm -PublisherName MicrosoftWindowsServer -Offer WindowsServer -Skus 2016-Datacenter -Version latest
   $vm = Set-AzureRmVMOSDisk -VM $vm -Name myOsDisk$i -StorageAccountType StandardLRS -DiskSizeInGB 128 -CreateOption FromImage -Caching ReadWrite
   $nic = Get-AzureRmNetworkInterface -ResourceGroupName myResourceGroup -Name myNic$i
   $vm = Add-AzureRmVMNetworkInterface -VM $vm -Id $nic.Id
   New-AzureRmVM -ResourceGroupName myResourceGroup -Location westeurope -VM $vm
}

```

Die Erstellung und Konfiguration aller drei virtuellen Computer dauert einige Minuten. Der Integritätstest des Load Balancers erkennt automatisch, wenn die App auf den einzelnen virtuellen Computern ausgeführt wird. Sobald die App ausgeführt wird, wird der Datenverkehr auf der Grundlage der Load Balancer-Regel verteilt.

### <a name="install-the-app"></a>Installieren der App 

Azure-VM-Erweiterungen werden verwendet, um Konfigurationsaufgaben für den virtuellen Computer, z.B. Installieren von Anwendungen und Konfigurieren des Betriebssystems, zu automatisieren. Die [benutzerdefinierte Skripterweiterung für Windows](./../virtual-machines-windows-extensions-customscript.md) wird verwendet, um beliebige PowerShell-Skripts auf virtuellen Computern auszuführen. Das Skript kann im Azure-Speicher oder auf einem beliebigen erreichbaren HTTP-Endpunkt gespeichert werden oder in die Konfiguration der benutzerdefinierten Skripterweiterung eingebettet werden. Wenn Sie die benutzerdefinierte Skripterweiterung verwenden, verwaltet der Azure-VM-Agent die Ausführung des Skripts.

Verwenden Sie [Set-AzureRmVMExtension](https://docs.microsoft.com/powershell/resourcemanager/azurerm.compute/v2.8.0/set-azurermvmextension) zum Installieren der benutzerdefinierten Skripterweiterung. Die Erweiterung führt `powershell Add-WindowsFeature Web-Server` zum Installieren des IIS-Webservers aus:

```powershell
for ($i=1; $i -le 3; $i++)
{
   Set-AzureRmVMExtension -ResourceGroupName myResourceGroup `
     -ExtensionName IIS `
     -VMName myVM$i `
     -Publisher Microsoft.Compute `
     -ExtensionType CustomScriptExtension `
     -TypeHandlerVersion 1.4 `
     -SettingString '{"commandToExecute":"powershell Add-WindowsFeature Web-Server"}' `
     -Location westeurope
}
```

### <a name="test-your-app"></a>Testen Ihrer App

Rufen Sie mit [Get-AzureRmPublicIPAddress](https://docs.microsoft.com/powershell/resourcemanager/azurerm.network/v3.6.0/get-azurermpublicipaddress) die öffentliche IP-Adresse Ihres Load Balancers ab. Im folgenden Beispiel wird die IP-Adresse für `myPublicIP` abgerufen, die wir zuvor erstellt haben:

```powershell
Get-AzureRmPublicIPAddress -ResourceGroupName myResourceGroup -Name myPublicIP | select IpAddress
```

Geben Sie die öffentliche IP-Adresse in einen Webbrowser ein. Mit der NSG-Regel wird die IIS-Standardwebsite angezeigt. 

![IIS-Standardwebsite](media/load-balanced-iis-tutorial/iis.png)

## <a name="step-6--management-tasks"></a>Schritt 6: Verwaltungsaufgaben

Für die virtuellen Computer, auf denen Ihre App ausgeführt wird, sind unter Umständen gelegentlich Wartungsarbeiten erforderlich (etwa die Installation von Betriebssystemupdates). Zur Bewältigung eines höheren Datenverkehrsaufkommens für Ihre App müssen gegebenenfalls weitere virtuelle Computer hinzugefügt werden. In diesem Abschnitt erfahren Sie, wie Sie einen virtuellen Computer aus dem Load Balancer entfernen oder dem Load Balancer einen virtuellen Computer hinzufügen. 

### <a name="remove-a-vm-from-the-load-balancer"></a>Entfernen eines virtuellen Computers aus dem Load Balancer

Entfernen Sie einen virtuellen Computer durch Zurücksetzen der Eigenschaft LoadBalancerBackendAddressPools der Netzwerkschnittstellenkarte aus dem Back-End-Adresspool.

Erstellen Sie mit [Get-AzureRmNetworkInterface](https://docs.microsoft.com/powershell/resourcemanager/azurerm.network/v3.6.0/get-azurermnetworkinterface) eine Netzwerkschnittstellenkarte:

```powershell
$nic = Get-AzureRmNetworkInterface -ResourceGroupName myResourceGroup -Name myNic2
``` 

Legen Sie die Eigenschaft LoadBalancerBackendAddressPools der Netzwerkschnittstellenkarte auf „$null“ fest:

```powershell
$nic.Ipconfigurations[0].LoadBalancerBackendAddressPools=$null
```

Aktualisieren Sie die Netzwerkschnittstellenkarte:

```powershell
Set-AzureRmNetworkInterface -NetworkInterface $nic
```

### <a name="add-a-vm-to-the-load-balancer"></a>Hinzufügen eines virtuellen Computers zum Load Balancer

Wenn Sie die Wartungsarbeiten für den virtuellen Computer abgeschlossen haben oder die Kapazität erweitern müssen, fügen Sie dem Back-End-Adresspool des Load Balancers die Netzwerkschnittstellenkarte eines virtuellen Computers hinzu.

Rufen Sie den Load Balancer ab:

```powershell
$lb = Get-AzureRMLoadBalancer -ResourceGroupName myResourceGroup -Name myLoadBalancer 
```

Fügen Sie den Back-End-Adresspool des Load Balancers der Netzwerkschnittstellenkarte hinzu:

```powershell
$nic.IpConfigurations[0].LoadBalancerBackendAddressPools=$lb.BackendAddressPools[0]
```

Aktualisieren Sie die Netzwerkschnittstellenkarte:

```powershell
Set-AzureRmNetworkInterface -NetworkInterface $nic
```

## <a name="next-steps"></a>Nächste Schritte

Beispiele: [Virtueller Azure-Computer – PowerShell-Beispielskripts](./../virtual-machines-windows-powershell-samples.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
