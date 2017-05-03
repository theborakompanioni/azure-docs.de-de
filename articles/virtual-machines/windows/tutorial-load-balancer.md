---
title: "Durchführen eines Lastenausgleichs bei virtuellen Windows-Computern in Azure | Microsoft-Dokumentation"
description: "Hier erfahren Sie, wie Sie mit Azure Load Balancer eine hochverfügbare und sichere Anwendung für drei virtuelle Windows-Computer erstellen."
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
ms.date: 04/17/2017
ms.author: iainfou
translationtype: Human Translation
ms.sourcegitcommit: 2c33e75a7d2cb28f8dc6b314e663a530b7b7fdb4
ms.openlocfilehash: e9a91a9a933104e4f885b009490ec35468398662
ms.lasthandoff: 04/21/2017

---

# <a name="how-to-load-balance-windows-virtual-machines-in-azure-to-create-a-highly-available-application"></a>Gewusst wie: Durchführen eines Lastenausgleichs bei virtuellen Windows-Computern in Azure, um eine hochverfügbare Anwendung zu erstellen
In diesem Tutorial lernen Sie die verschiedenen Komponenten von Azure Load Balancer kennen, die den Datenverkehr verteilen und hohe Verfügbarkeit bereitstellen. Um den Load Balancer in Aktion zu sehen, erstellen Sie eine einfache IIS-Website, die auf drei virtuellen Windows-Computern (VMs) ausgeführt wird.

Die Schritte in diesem Tutorial können mit dem neuesten [Azure PowerShell](/powershell/azureps-cmdlets-docs/)-Modul ausgeführt werden.


## <a name="azure-load-balancer-overview"></a>Übersicht über Azure Load Balancer
Eine Azure Load Balancer-Instanz ist ein Layer-4-Lastenausgleich (TCP, UDP), der durch Verteilen des eingehenden Datenverkehrs auf fehlerfreie VMs hohe Verfügbarkeit bietet. Ein Load Balancer-Integritätstest überwacht einen bestimmten Port auf jedem virtuellen Computer und verteilt Datenverkehr nur an einen betriebsbereiten virtuellen Computer.

Sie definieren eine Front-End-IP-Konfiguration, die eine oder mehrere öffentliche IP-Adressen enthält. Diese Front-End-IP-Konfiguration ermöglicht den Internetzugriff auf Ihren Load Balancer und Ihre Anwendungen. 

Virtuelle Computer stellen über ihre virtuelle Netzwerkkarte (NIC) eine Verbindung mit einem Load Balancer her. Zum Verteilen von Datenverkehr auf die virtuellen Computer enthält ein Back-End-Adresspool die IP-Adressen der virtuellen NICs, die mit dem Load Balancer verbunden sind.

Um den Fluss des Datenverkehrs zu steuern, definieren Sie Load Balancer-Regeln für bestimmte Ports und Protokolle, die Ihren virtuellen Computern zugeordnet sind.


## <a name="create-azure-load-balancer"></a>Erstellen einer Azure Load Balancer-Instanz
In diesem Abschnitt wird erläutert, wie Sie jede Komponente des Load Balancers erstellen und konfigurieren. Bevor Sie eine Load Balancer-Instanz erstellen können, müssen Sie mit [New-AzureRmResourceGroup](/powershell/resourcemanager/AzureRM.Resources/new-azurermresourcegroup) eine Ressourcengruppe erstellen. Das folgende Beispiel erstellt eine Ressourcengruppe mit dem Namen `myResourceGroupLoadBalancer` am Standort `westus`:

```powershell
New-AzureRmResourceGroup `
  -ResourceGroupName myResourceGroupLoadBalancer `
  -Location westus
```

### <a name="create-a-public-ip-address"></a>Erstellen einer öffentlichen IP-Adresse
Um über das Internet auf Ihre App zugreifen zu können, benötigen Sie eine öffentliche IP-Adresse für den Load Balancer. Erstellen Sie mit [New-AzureRmPublicIpAddress](/powershell/resourcemanager/azurerm.network/new-azurermpublicipaddress) eine öffentliche IP-Adresse. Im folgenden Beispiel wird eine öffentliche IP-Adresse namens `myPublicIP` in der `myResourceGroupLoadBalancer`-Ressourcengruppe erstellt:

```powershell
$publicIP = New-AzureRmPublicIpAddress `
  -ResourceGroupName myResourceGroupLoadBalancer `
  -Location westus `
  -AllocationMethod Static `
  -Name myPublicIP
```

### <a name="create-a-load-balancer"></a>Einrichten eines Load Balancers
Erstellen Sie eine Front-End-IP-Adresse mit [New-AzureRmLoadBalancerFrontendIpConfig](/powershell/resourcemanager/azurerm.network/new-azurermloadbalancerfrontendipconfig). Im folgenden Beispiel wird eine Front-End-IP-Adresse namens `myFrontEndPool` erstellt: 

```powershell
$frontendIP = New-AzureRmLoadBalancerFrontendIpConfig `
  -Name myFrontEndPool `
  -PublicIpAddress $publicIP
```

Erstellen Sie einen Back-End-Adresspool mit [New-AzureRmLoadBalancerBackendAddressPoolConfig](/powershell/resourcemanager/azurerm.network/new-azurermloadbalancerbackendaddresspoolconfig). Im folgenden Beispiel wird ein Back-End-Adresspool namens `myBackEndPool` erstellt:

```powershell
$backendPool = New-AzureRmLoadBalancerBackendAddressPoolConfig -Name myBackEndPool
```

Erstellen Sie nun mit [New-AzureRmLoadBalancer](/powershell/resourcemanager/azurerm.network/new-azurermloadbalancer) den Load Balancer. Im folgenden Beispiel wird unter Verwendung der `myPublicIP`-Adresse ein Load Balancer namens `myLoadBalancer` erstellt:

```powershell
$lb = New-AzureRmLoadBalancer `
  -ResourceGroupName myResourceGroupLoadBalancer `
  -Name myLoadBalancer `
  -Location westus `
  -FrontendIpConfiguration $frontendIP `
  -BackendAddressPool $backendPool
```

### <a name="create-a-health-probe"></a>Erstellen eines Integritätstests
Damit der Load Balancer den Status Ihrer App überwachen kann, verwenden Sie einen Integritätstest. Abhängig von der Reaktion auf Integritätsüberprüfungen werden der Load Balancer-Rotation durch den Integritätstest dynamisch virtuelle Computer hinzugefügt oder daraus entfernt. Falls bei einem virtuellen Computer innerhalb eines Intervalls von 15 Sekunden zwei aufeinanderfolgende Fehler auftreten, wird er standardmäßig aus der Load Balancer-Verteilung entfernt. Integritätstests werden auf der Grundlage eines Protokolls oder einer spezifischen Integritätsprüfungsseite für Ihre App erstellt. 

Im folgenden Beispiel wird ein TCP-Test erstellt. Sie können auch benutzerdefinierte HTTP-Tests für differenziertere Integritätsprüfungen erstellen. Wenn Sie einen benutzerdefinierte HTTP-Test verwenden, müssen Sie die Integritätsprüfungsseite erstellen, z.B. `healthcheck.aspx`. Der Test muss die Antwort **HTTP 200 OK** zurückgeben, damit der Load Balancer den Host nicht aus der Rotation entfernt.

Um einen TCP-Integritätstest zu erstellen, verwenden Sie [Add-AzureRmLoadBalancerProbeConfig](/powershell/resourcemanager/azurerm.network/add-azurermloadbalancerprobeconfig). Im folgenden Beispiel wird ein benannter Integritätstest namens `myHealthProbe` erstellt, der die einzelnen virtuellen Computer überwacht:

```powershell
Add-AzureRmLoadBalancerProbeConfig -Name myHealthProbe `
  -LoadBalancer $lb `
  -Protocol tcp `
  -Port 80 `
  -IntervalInSeconds 15 `
  -ProbeCount 2
```

### <a name="create-a-load-balancer-rule"></a>Erstellen einer Load Balancer-Regel
Mithilfe einer Load Balancer-Regel wird definiert, wie Datenverkehr auf die virtuellen Computer verteilt werden soll. Sie definieren die Front-End-IP-Konfiguration für den eingehenden Datenverkehr und den Back-End-IP-Pool für den Empfang des Datenverkehrs zusammen mit dem erforderlichen Quell- und Zielport. Um sicherzustellen, dass nur fehlerfreie VMs Datenverkehr empfangen, definieren Sie auch den zu verwendenden Integritätstest.

Erstellen Sie mit [Add-AzureRmLoadBalancerRuleConfig](/powershell/resourcemanager/AzureRM.Network/add-azurermloadbalancerruleconfig) eine Load Balancer-Regel. Im folgenden Beispiel wird eine Load Balancer-Regel mit dem Namen `myLoadBalancerRule` erstellt und der Datenverkehr an Port `80` ausgeglichen:

```powershell
Add-AzureRmLoadBalancerRuleConfig -Name myLoadBalancerRule `
  -LoadBalancer $lb `
  -FrontendIpConfiguration $lb.FrontendIpConfigurations[0] `
  -BackendAddressPool $lb.BackendAddressPools[0] `
  -Protocol Tcp `
  -FrontendPort 80 `
  -BackendPort 80
```

Aktualisieren Sie den Load Balancer mit [Set-AzureRmLoadBalancer](/powershell/resourcemanager/azurerm.network/set-azurermloadbalancer):

```powershell
Set-AzureRmLoadBalancer -LoadBalancer $lb
```


## <a name="configure-virtual-network"></a>Konfigurieren eines virtuellen Netzwerks
Bevor Sie einige VMs bereitstellen und Ihren Balancer testen können, erstellen Sie die unterstützenden virtuellen Netzwerkressourcen. Weitere Informationen über virtuelle Netzwerke erhalten Sie im Tutorial [Manage Azure Virtual Networks and Windows Virtual Machines with Azure PowerShell](tutorial-virtual-network.md) (Verwalten von virtuellen Azure-Netzwerken und Windows Virtual Machines mit Azure PowerShell).

### <a name="create-network-resources"></a>Erstellen von Netzwerkressourcen
Erstellen Sie mit [New-AzureRmVirtualNetwork](/powershell/resourcemanager/azurerm.network/new-azurermvirtualnetwork) ein virtuelles Netzwerk. Im folgenden Beispiel wird ein virtuelles Netzwerk namens `myVnet` mit `mySubnet` erstellt:

```powershell
$subnetConfig = New-AzureRmVirtualNetworkSubnetConfig `
  -Name mySubnet `
  -AddressPrefix 192.168.1.0/24
$vnet = New-AzureRmVirtualNetwork `
  -ResourceGroupName myResourceGroupLoadBalancer `
  -Location westus `
  -Name myVnet `
  -AddressPrefix 192.168.0.0/16 `
  -Subnet $subnetConfig
```

Erstellen Sie eine Netzwerksicherheits-Gruppenregel mit [New-AzureRmNetworkSecurityRuleConfig](/powershell/resourcemanager/azurerm.network/new-azurermnetworksecurityruleconfig) und dann eine Netzwerksicherheitsgruppe mit [New-AzureRmNetworkSecurityGroup](/powershell/resourcemanager/azurerm.network/new-azurermnetworksecuritygroup). Fügen Sie die Netzwerksicherheitsgruppe mit [Set-AzureRmVirtualNetworkSubnetConfig](/powershell/resourcemanager/azurerm.network/set-azurermvirtualnetworksubnetconfig) dem Subnetz hinzu, und aktualisieren Sie dann das virtuelle Netzwerk mit [Set-AzureRmVirtualNetwork](/powershell/resourcemanager/azurerm.network/set-azurermvirtualnetwork). 

Im folgenden Beispiel wird eine Netzwerksicherheitsgruppen-Regel namens `myNetworkSecurityGroup` erstellt und auf `mySubnet` angewandt:

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
$nsg = New-AzureRmNetworkSecurityGroup `
  -ResourceGroupName myResourceGroupLoadBalancer `
  -Location westus `
  -Name myNetworkSecurityGroup `
  -SecurityRules $nsgRule
Set-AzureRmVirtualNetworkSubnetConfig -VirtualNetwork $vnet `
  -Name mySubnet `
  -NetworkSecurityGroup $nsg `
  -AddressPrefix 192.168.1.0/24
Set-AzureRmVirtualNetwork -VirtualNetwork $vnet
```

Virtuelle NICs werden mit [New-AzureRmNetworkInterface](/powershell/resourcemanager/azurerm.network/new-azurermnetworkinterface) erstellt. Im folgenden Beispiel werden drei virtuelle NICs erstellt (jeweils eine virtuelle NIC pro virtuellem Computer, den Sie in den folgenden Schritten für Ihre App erstellen). Sie können jederzeit weitere virtuelle NICs und virtuelle Computer erstellen und dem Load Balancer hinzufügen:

```powershell
for ($i=1; $i -le 3; $i++)
{
   New-AzureRmNetworkInterface -ResourceGroupName myResourceGroupLoadBalancer `
     -Name myNic$i `
     -Location westus `
     -Subnet $vnet.Subnets[0] `
     -LoadBalancerBackendAddressPool $lb.BackendAddressPools[0]
}
```

## <a name="create-virtual-machines"></a>Erstellen von virtuellen Computern
Fügen Sie die virtuellen Computer in eine Verfügbarkeitsgruppe ein, um die hohe Verfügbarkeit Ihrer App zu optimieren.

Erstellen Sie mithilfe von [New-AzureRmAvailabilitySet](/powershell/resourcemanager/azurerm.compute/new-azurermavailabilityset) eine Verfügbarkeitsgruppe. Im folgenden Beispiel wird eine Verfügbarkeitsgruppe namens `myAvailabilitySet` erstellt:

```powershell
$availabilitySet = New-AzureRmAvailabilitySet `
  -ResourceGroupName myResourceGroupLoadBalancer `
  -Name myAvailabilitySet `
  -Location westus `
  -Managed `
  -PlatformFaultDomainCount 3 `
  -PlatformUpdateDomainCount 2
```

Legen Sie mit [Get-Credential](https://msdn.microsoft.com/powershell/reference/5.1/microsoft.powershell.security/Get-Credential) den Benutzernamen und das Kennwort des Administrators der VMs fest:

```powershell
$cred = Get-Credential
```

Nun können Sie mit [New-AzureRmVM](/powershell/resourcemanager/azurerm.compute/new-azurermvm) die VMs erstellen. Im folgenden Beispiel werden drei VMs erstellt:

```powershell
for ($i=1; $i -le 3; $i++)
{
   $vm = New-AzureRmVMConfig -VMName myVM$i -VMSize Standard_D1 -AvailabilitySetId $availabilitySet.Id
   $vm = Set-AzureRmVMOperatingSystem -VM $vm -Windows -ComputerName myVM$i -Credential $cred -ProvisionVMAgent -EnableAutoUpdate
   $vm = Set-AzureRmVMSourceImage -VM $vm -PublisherName MicrosoftWindowsServer -Offer WindowsServer -Skus 2016-Datacenter -Version latest
   $vm = Set-AzureRmVMOSDisk -VM $vm -Name myOsDisk$i -DiskSizeInGB 128 -CreateOption FromImage -Caching ReadWrite
   $nic = Get-AzureRmNetworkInterface -ResourceGroupName myResourceGroupLoadBalancer -Name myNic$i
   $vm = Add-AzureRmVMNetworkInterface -VM $vm -Id $nic.Id
   New-AzureRmVM -ResourceGroupName myResourceGroupLoadBalancer -Location westus -VM $vm
}
```

Die Erstellung und Konfiguration aller drei virtuellen Computer dauert einige Minuten.

### <a name="install-iis-with-custom-script-extension"></a>Installieren von IIS mit benutzerdefinierter Skripterweiterung
In einem vorherigen Tutorial zum [Anpassen eines virtuellen Windows-Computers](tutorial-automate-vm-deployment.md) haben Sie erfahren, wie die Anpassung für virtuelle Computer mit der benutzerdefinierten Skripterweiterung für Windows automatisiert wird. Sie können den gleichen Ansatz verwenden, um IIS für Ihre virtuellen Computer zu installieren und konfigurieren.

Verwenden Sie [Set-AzureRmVMExtension](/powershell/resourcemanager/azurerm.compute/set-azurermvmextension) zum Installieren der benutzerdefinierten Skripterweiterung. Die Erweiterung führt `powershell Add-WindowsFeature Web-Server` zum Installieren des IIS-Webservers aus und aktualisiert dann die Seite „`Default.htm`“ mit dem Hostnamen der VM:

```powershell
for ($i=1; $i -le 3; $i++)
{
   Set-AzureRmVMExtension `
     -ResourceGroupName myResourceGroupLoadBalancer `
     -ExtensionName IIS `
     -VMName myVM$i `
     -Publisher Microsoft.Compute `
     -ExtensionType CustomScriptExtension `
     -TypeHandlerVersion 1.4 `
     -SettingString '{"commandToExecute":"powershell Add-WindowsFeature Web-Server; powershell Add-Content -Path \"C:\\inetpub\\wwwroot\\Default.htm\" -Value $($env:computername)"}' `
     -Location westus
}
```

## <a name="test-load-balancer"></a>Testen des Load Balancers
Rufen Sie mit [Get-AzureRmPublicIPAddress](/powershell/resourcemanager/azurerm.network/get-azurermpublicipaddress) die öffentliche IP-Adresse Ihres Load Balancers ab. Im folgenden Beispiel wird die IP-Adresse für `myPublicIP` abgerufen, die wir zuvor erstellt haben:

```powershell
Get-AzureRmPublicIPAddress `
  -ResourceGroupName myResourceGroupLoadBalancer `
  -Name myPublicIP | select IpAddress
```

Geben Sie die öffentliche IP-Adresse in einen Webbrowser ein. Die Website wird mit dem Hostnamen der VM angezeigt, an die der Load Balancer den Datenverkehr wie im folgenden Beispiel verteilt hat:

![Ausführen der IIS-Website](./media/tutorial-load-balancer/running-iis-website.png)

Sie können eine erzwungene Aktualisierung Ihres Webbrowsers durchführen, um zu verfolgen, wie der Load Balancer den Datenverkehr auf alle drei virtuellen Computer verteilt, auf denen Ihre App ausgeführt wird.


## <a name="add-and-remove-vms"></a>VMs hinzufügen und entfernen
Für die virtuellen Computer, auf denen Ihre App ausgeführt wird, sind unter Umständen gelegentlich Wartungsarbeiten erforderlich (etwa die Installation von Betriebssystemupdates). Zur Bewältigung eines höheren Datenverkehrsaufkommens für Ihre App müssen gegebenenfalls weitere virtuelle Computer hinzugefügt werden. In diesem Abschnitt erfahren Sie, wie Sie einen virtuellen Computer aus dem Load Balancer entfernen oder dem Load Balancer einen virtuellen Computer hinzufügen.

### <a name="remove-a-vm-from-the-load-balancer"></a>Entfernen eines virtuellen Computers aus dem Load Balancer
Rufen Sie die Netzwerkschnittstellenkarte mit [Get-AzureRmNetworkInterface](/powershell/resourcemanager/azurerm.network/get-azurermnetworkinterface) ab, und legen Sie dann die `LoadBalancerBackendAddressPools`-Eigenschaft der virtuellen NIC auf `$null` fest. Zum Schluss aktualisieren Sie die virtuelle NIC:

```powershell
$nic = Get-AzureRmNetworkInterface -ResourceGroupName myResourceGroupLoadBalancer -Name myNic2
$nic.Ipconfigurations[0].LoadBalancerBackendAddressPools=$null
Set-AzureRmNetworkInterface -NetworkInterface $nic
```

Sie können eine erzwungene Aktualisierung Ihres Webbrowsers durchführen, um zu verfolgen, wie der Load Balancer den Datenverkehr auf die beiden verbleibenden virtuellen Computer verteilt, auf denen Ihre App ausgeführt wird. Nun können Sie Wartungsarbeiten für den virtuellen Computer durchführen und beispielsweise Betriebssystemupdates installieren oder den virtuellen Computer neu starten.

### <a name="add-a-vm-to-the-load-balancer"></a>Hinzufügen eines virtuellen Computers zum Load Balancer
Nach dem Ausführen der VM-Wartung, oder wenn Sie die Kapazität erweitern müssen, legen Sie die `LoadBalancerBackendAddressPools`-Eigenschaft der virtuellen NIC auf den `BackendAddressPool` aus [Get AzureRMLoadBalancer]() fest:

Rufen Sie den Load Balancer ab:

```powershell
$lb = Get-AzureRMLoadBalancer -ResourceGroupName myResourceGroupLoadBalancer -Name myLoadBalancer 
$nic.IpConfigurations[0].LoadBalancerBackendAddressPools=$lb.BackendAddressPools[0]
Set-AzureRmNetworkInterface -NetworkInterface $nic
```

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie erfahren, wie Sie eine IIS-Website mit Lastenausgleich erstellen. Im nächsten Tutorial erfahren Sie, wie Sie VM-Netzwerke verwalten.

[Verwalten von Azure-VM-Netzwerken](./tutorial-virtual-network.md)
