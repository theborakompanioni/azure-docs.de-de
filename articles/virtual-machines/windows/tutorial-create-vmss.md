---
title: "Erstellen einer VM-Skalierungsgruppe für Windows in Azure | Microsoft-Dokumentation"
description: "Erstellen und Bereitstellen einer hoch verfügbaren Anwendung auf virtuellen Windows-Computern mithilfe einer VM-Skalierungsgruppe"
services: virtual-machine-scale-sets
documentationcenter: 
author: iainfoulds
manager: timlt
editor: 
tags: 
ms.assetid: 
ms.service: virtual-machine-scale-sets
ms.workload: infrastructure-services
ms.tgt_pltfrm: na
ms.devlang: 
ms.topic: article
ms.date: 05/02/2017
ms.author: iainfou
ms.custom: mvc
ms.translationtype: Human Translation
ms.sourcegitcommit: 2db2ba16c06f49fd851581a1088df21f5a87a911
ms.openlocfilehash: 8a5f6e8bf01c8bc38f3fd327acd0ddc8f9cdd7de
ms.contentlocale: de-de
ms.lasthandoff: 05/08/2017

---

# <a name="create-a-virtual-machine-scale-set-and-deploy-a-highly-available-app-on-windows"></a>Erstellen einer VM-Skalierungsgruppe und Bereitstellen einer hoch verfügbaren App unter Windows
Mit einer VM-Skalierungsgruppe können Sie eine Gruppe identischer, automatisch skalierender virtueller Computer bereitstellen und verwalten. Sie können die Anzahl der virtuellen Computer in der Skalierungsgruppe manuell skalieren oder basierend auf CPU-Auslastung, Speicherbedarf oder Netzwerkdatenverkehr Regeln für die automatische Skalierung definieren. In diesem Tutorial stellen Sie eine Skalierungsgruppe für virtuelle Computer bereit. Folgendes wird vermittelt:

> [!div class="checklist"]
> * Verwenden der Benutzerdefinierten Skripterweiterung zum Definieren einer IIS-Website zur Skalierung
> * Erstellen eines Lastenausgleichs für Ihre Skalierungsgruppe
> * Erstellen einer Skalierungsgruppe für virtuelle Computer
> * Erhöhen oder Verringern der Anzahl der Instanzen in einer Skalierungsgruppe
> * Erstellen von Regeln zur automatischen Skalierung

Für dieses Tutorial ist das Azure PowerShell-Modul Version 3.6 oder höher erforderlich. Führen Sie ` Get-Module -ListAvailable AzureRM` aus, um die Version zu finden. Wenn Sie ein Upgrade ausführen müssen, finden Sie unter [Installieren des Azure PowerShell-Moduls](/powershell/azure/install-azurerm-ps) Informationen dazu.


## <a name="scale-set-overview"></a>Übersicht über Skalierungsgruppen
Für Skalierungsgruppen werden Konzepte ähnlich denen befolgt, die Sie im vorherigen Tutorial [Erstellen von hoch verfügbaren virtuellen Computern](tutorial-availability-sets.md) kennengelernt haben. Virtuelle Computer in einer Skalierungsgruppe werden wie VMs in einer Verfügbarkeitsgruppe auf logische Fehler- und Updatedomänen verteilt.

Virtuelle Computer werden nach Bedarf in einer Skalierungsgruppe erstellt. Sie können Regeln für die automatische Skalierung definieren, um zu steuern, wie und wann virtuelle Computer in der Skalierungsgruppe hinzugefügt oder entfernt werden. Diese Regeln können basierend auf Metriken wie CPU-Auslastung, Speicherauslastung oder Netzwerkdatenverkehr ausgelöst werden.

Bei Verwendung eines Azure-Plattformimages unterstützen Skalierungsgruppen bis zu 1.000 virtuelle Computer. Für Workloads mit beträchtlichen Installations- oder VM-Anpassungsanforderungen können Sie nach Wunsch ein [benutzerdefiniertes VM-Image erstellen](tutorial-custom-images.md). Bei Verwendung eines benutzerdefinierten Images können Sie bis zu 100 virtuelle Computer in einer Skalierungsgruppe erstellen.


## <a name="create-an-app-to-scale"></a>Erstellen einer App für die Skalierung
Bevor Sie eine Skalierungsgruppe erstellen können, müssen Sie mit [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup) eine Ressourcengruppe erstellen. Das folgende Beispiel erstellt am Standort *EastUS* eine Ressourcengruppe mit dem Namen *myResourceGroupAutomate*:

```powershell
New-AzureRmResourceGroup -ResourceGroupName myResourceGroupScaleSet -Location EastUS
```

In einem früheren Tutorial haben Sie das [Automatisieren der VM-Konfiguration](tutorial-automate-vm-deployment.md) mithilfe der benutzerdefinierten Skripterweiterung kennengelernt. Erstellen Sie die Skalierungsgruppenkonfiguration, und wenden Sie dann eine benutzerdefinierte Skripterweiterung zur Installation und Konfiguration von IIS an:

```powershell
# Create a config object
$vmssConfig = New-AzureRmVmssConfig `
    -Location EastUS `
    -SkuCapacity 2 `
    -SkuName Standard_DS2 `
    -UpgradePolicyMode Automatic

# Define the script for your Custom Script Extension to run
$publicSettings = @{
    "fileUris" = (,"https://raw.githubusercontent.com/iainfoulds/azure-samples/master/automate-iis.ps1");
    "commandToExecute" = "powershell -ExecutionPolicy Unrestricted -File automate-iis.ps1"
}

# Use Custom Script Extension to install IIS and configure basic website
Add-AzureRmVmssExtension -VirtualMachineScaleSet $vmssConfig `
    -Name "customScript" `
    -Publisher "Microsoft.Compute" `
    -Type "CustomScriptExtension" `
    -TypeHandlerVersion 1.8 `
    -Setting $publicSettings
```

## <a name="create-scale-load-balancer"></a>Erstellen eines Load Balancers für die Skalierung
Eine Azure Load Balancer-Instanz ist ein Layer-4-Lastenausgleich (TCP, UDP), der durch Verteilen des eingehenden Datenverkehrs auf fehlerfreie VMs hohe Verfügbarkeit bietet. Ein Load Balancer-Integritätstest überwacht einen bestimmten Port auf jedem virtuellen Computer und verteilt Datenverkehr nur an einen betriebsbereiten virtuellen Computer. Weitere Informationen finden Sie im nächsten Tutorial zum [Lastenausgleich von Windows-VMs](tutorial-load-balancer.md).

Erstellen Sie einen Load Balancer, der eine öffentliche IP-Adresse hat und Webdatenverkehr an Port 80 verteilt:

```powershell
# Create a public IP address
$publicIP = New-AzureRmPublicIpAddress `
  -ResourceGroupName myResourceGroupScaleSet `
  -Location EastUS `
  -AllocationMethod Static `
  -Name myPublicIP

# Create a frontend and backend IP pool
$frontendIP = New-AzureRmLoadBalancerFrontendIpConfig `
  -Name myFrontEndPool `
  -PublicIpAddress $publicIP
$backendPool = New-AzureRmLoadBalancerBackendAddressPoolConfig -Name myBackEndPool

# Create the load balancer
$lb = New-AzureRmLoadBalancer `
  -ResourceGroupName myResourceGroupScaleSet `
  -Name myLoadBalancer `
  -Location EastUS `
  -FrontendIpConfiguration $frontendIP `
  -BackendAddressPool $backendPool

# Create a load balancer health probe on port 80
Add-AzureRmLoadBalancerProbeConfig -Name myHealthProbe `
  -LoadBalancer $lb `
  -Protocol tcp `
  -Port 80 `
  -IntervalInSeconds 15 `
  -ProbeCount 2

# Create a load balancer rule to distribute traffic on port 80
Add-AzureRmLoadBalancerRuleConfig `
  -Name myLoadBalancerRule `
  -LoadBalancer $lb `
  -FrontendIpConfiguration $lb.FrontendIpConfigurations[0] `
  -BackendAddressPool $lb.BackendAddressPools[0] `
  -Protocol Tcp `
  -FrontendPort 80 `
  -BackendPort 80

# Update the load balancer configuration
Set-AzureRmLoadBalancer -LoadBalancer $lb
```

## <a name="create-a-scale-set"></a>Erstellen einer Skalierungsgruppe
Erstellen Sie nun mit [New-AzureRmVmss](/powershell/module/azurerm.compute/new-azurermvm) eine VM-Skalierungsgruppe. Im folgenden Beispiel wird eine Skalierungsgruppe namens *myScaleSet* erstellt:

```powershell
# Reference a virtual machine image from the gallery
Set-AzureRmVmssStorageProfile $vmssConfig `
  -ImageReferencePublisher MicrosoftWindowsServer `
  -ImageReferenceOffer WindowsServer `
  -ImageReferenceSku 2016-Datacenter `
  -ImageReferenceVersion latest

# Set up information for authenticating with the virtual machine
Set-AzureRmVmssOsProfile $vmssConfig `
  -AdminUsername azureuser `
  -AdminPassword P@ssword! `
  -ComputerNamePrefix myVM

# Create the virtual network resources
$subnet = New-AzureRmVirtualNetworkSubnetConfig `
  -Name "mySubnet" `
  -AddressPrefix 10.0.0.0/24
$vnet = New-AzureRmVirtualNetwork `
  -ResourceGroupName "myResourceGroupScaleSet" `
  -Name "myVnet" `
  -Location "EastUS" `
  -AddressPrefix 10.0.0.0/16 `
  -Subnet $subnet
$ipConfig = New-AzureRmVmssIpConfig `
  -Name "myIPConfig" `
  -LoadBalancerBackendAddressPoolsId $lb.BackendAddressPools[0].Id `
  -SubnetId $vnet.Subnets[0].Id

# Attach the virtual network to the config object
Add-AzureRmVmssNetworkInterfaceConfiguration `
  -VirtualMachineScaleSet $vmssConfig `
  -Name "network-config" `
  -Primary $true `
  -IPConfiguration $ipConfig

# Create the scale set with the config object (this step might take a few minutes)
New-AzureRmVmss `
  -ResourceGroupName myResourceGroupScaleSet `
  -Name myScaleSet `
  -VirtualMachineScaleSet $vmssConfig     
```

Die Erstellung und Konfiguration aller Ressourcen und virtuellen Computer der Skalierungsgruppe dauert einige Minuten.


## <a name="test-your-app"></a>Testen Ihrer App
Um Ihre IIS-Website in Aktion zu sehen, rufen Sie mit [Get-AzureRmPublicIPAddress](/powershell/module/azurerm.network/get-azurermpublicipaddress) die öffentliche IP-Adresse Ihres Load Balancers ab. Im folgenden Beispiel wird die IP-Adresse für *myPublicIP* abgerufen, die als Teil der Skalierungsgruppe erstellt wurde:

```powershell
Get-AzureRmPublicIPAddress -ResourceGroupName myResourceGroupScaleSet -Name myPublicIP | select IpAddress
```

Geben Sie die öffentliche IP-Adresse in einen Webbrowser ein. Die App wird mit dem Hostnamen des virtuellen Computers angezeigt, an den der Load Balancer den Datenverkehr verteilt hat:

![Ausführen der IIS-Website](./media/tutorial-create-vmss/running-iis-site.png)

Um die Skalierungsgruppe in Aktion zu sehen, führen Sie eine erzwungene Aktualisierung Ihres Webbrowsers durch, um zu verfolgen, wie der Load Balancer den Datenverkehr auf alle virtuellen Computer in der Skalierungsgruppe verteilt, auf denen Ihre App ausgeführt wird.


## <a name="management-tasks"></a>Verwaltungsaufgaben
Während des Lebenszyklus der Skalierungsgruppe müssen Sie möglicherweise eine oder mehrere Verwaltungsaufgaben ausführen. Darüber hinaus empfiehlt es sich, Skripts zum Automatisieren von verschiedenen Aufgaben im Lebenszyklus zu erstellen. Azure PowerShell bietet eine schnelle Möglichkeit, diese Aufgaben auszuführen. Im Folgenden sind einige allgemeine Aufgaben aufgeführt.

### <a name="view-vms-in-a-scale-set"></a>Anzeigen von virtuellen Computern in einer Skalierungsgruppe
Verwenden Sie [Get-AzureRmVmssVM](/powershell/module/azurerm.compute/get-azurermvmssvm) wie folgt, um eine Liste der in der Skalierungsgruppe ausgeführten virtuellen Computer anzuzeigen:

```powershell
# Get current scale set
$scaleset = Get-AzureRmVmss `
  -ResourceGroupName myResourceGroupScaleSet `
  -VMScaleSetName myScaleSet

# Loop through the instanaces in your scale set
for ($i=0; $i -le ($scaleset.Sku.Capacity - 1); $i++) {
    Get-AzureRmVmssVM -ResourceGroupName myResourceGroupScaleSet `
      -VMScaleSetName myScaleSet `
      -InstanceId $i
}
```


### <a name="increase-or-decrease-vm-instances"></a>VM-Instanzen erhöhen oder verringern
Verwenden Sie [Get-AzureRmVmss](/powershell/module/azurerm.compute/get-azurermvmss), und führen Sie eine Abfrage nach *sku.capacity* durch, um die Anzahl der zurzeit in einer Skalierungsgruppe vorhandenen Instanzen anzuzeigen:

```powershell
Get-AzureRmVmss -ResourceGroupName myResourceGroupScaleSet `
    -VMScaleSetName myScaleSet | `
    Select -ExpandProperty Sku
```

Sie können dann die Anzahl der virtuellen Computer in der Skalierungsgruppe mit [Update-AzureRmVmss](/powershell/module/azurerm.compute/update-azurermvmss) manuell erhöhen oder verringern. Im folgenden Beispiel wird die Anzahl der virtuellen Computer in der Skalierungsgruppe auf *5* festgelegt:

```powershell
# Get current scale set
$scaleset = Get-AzureRmVmss `
  -ResourceGroupName myResourceGroupScaleSet `
  -VMScaleSetName myScaleSet

# Set and update the capacity of your scale set
$scaleset.sku.capacity = 5
Update-AzureRmVmss -ResourceGroupName myResourceGroupScaleSet `
    -Name myScaleSet `
    -VirtualMachineScaleSet $scaleset
```

Es dauert einige Minuten, bis die angegebene Anzahl von Instanzen in der Skalierungsgruppe aktualisiert ist.


### <a name="configure-autoscale-rules"></a>Konfigurieren von Regeln zur automatischen Skalierung
Anstatt die Anzahl von Instanzen in Ihrer Skalierungsgruppe manuell zu skalieren, definieren Sie Regeln zur automatischen Skalierung. Diese Regeln überwachen die Instanzen in Ihrer Skalierungsgruppe und reagieren entsprechend basierend auf von Ihnen festgelegten Metriken und Schwellenwerten. Im folgenden Beispiel wird die Anzahl der Instanzen um eine Instanz horizontal hochskaliert, wenn die durchschnittliche CPU-Auslastung über einen Zeitraum von 5 Minuten höher als 60 % ist. Wenn die durchschnittliche CPU-Auslastung über einen Zeitraum von 5 Minuten unter 30 % fällt, werden die Instanzen um eine Instanz horizontal herunterskaliert:

```powershell
# Define your scale set information
$mySubscriptionId = (Get-AzureRmSubscription).SubscriptionId
$myResourceGroup = "myResourceGroupScaleSet"
$myScaleSet = "myScaleSet"
$myLocation = "West US"

# Create a scale up rule to increase the number instances after 60% average CPU usage exceeded for a 5 minute period
$myRuleScaleUp = New-AzureRmAutoscaleRule `
  -MetricName "Percentage CPU" `
  -MetricResourceId /subscriptions/$mySubscriptionId/resourceGroups/$myResourceGroup/providers/Microsoft.Compute/virtualMachineScaleSets/$myScaleSet `
  -Operator GreaterThan `
  -MetricStatistic Average `
  -Threshold 60 `
  -TimeGrain 00:01:00 `
  -TimeWindow 00:05:00 `
  -ScaleActionCooldown 00:05:00 `
  -ScaleActionDirection Increase `
  -ScaleActionValue 1

# Create a scale down rule to decrease the number of instances after 30% average CPU usage over a 5 minute period
$myRuleScaleDown = New-AzureRmAutoscaleRule `
  -MetricName "Percentage CPU" `
  -MetricResourceId /subscriptions/$mySubscriptionId/resourceGroups/$myResourceGroup/providers/Microsoft.Compute/virtualMachineScaleSets/$myScaleSet `
  -Operator LessThan `
  -MetricStatistic Average `
  -Threshold 30 `
  -TimeGrain 00:01:00 `
  -TimeWindow 00:05:00 `
  -ScaleActionCooldown 00:05:00 `
  -ScaleActionDirection Decrease `
  -ScaleActionValue 1

# Create a scale profile with your scale up and scale down rules
$myScaleProfile = New-AzureRmAutoscaleProfile `
  -DefaultCapacity 2  `
  -MaximumCapacity 10 `
  -MinimumCapacity 2 `
  -Rules $myRuleScaleUp,$myRuleScaleDown `
  -Name "autoprofile"

# Apply the autoscale rules
Add-AzureRmAutoscaleSetting `
  -Location $myLocation `
  -Name "autosetting" `
  -ResourceGroup $myResourceGroup `
  -TargetResourceId /subscriptions/$mySubscriptionId/resourceGroups/$myResourceGroup/providers/Microsoft.Compute/virtualMachineScaleSets/$myScaleSet `
  -AutoscaleProfiles $myScaleProfile
```


## <a name="next-steps"></a>Nächste Schritte
In diesem Tutorial haben Sie eine Skalierungsgruppe für virtuelle Computer bereitgestellt. Es wurde Folgendes vermittelt:

> [!div class="checklist"]
> * Verwenden der Benutzerdefinierten Skripterweiterung zum Definieren einer IIS-Website zur Skalierung
> * Erstellen eines Lastenausgleichs für Ihre Skalierungsgruppe
> * Erstellen einer Skalierungsgruppe für virtuelle Computer
> * Erhöhen oder Verringern der Anzahl der Instanzen in einer Skalierungsgruppe
> * Erstellen von Regeln zur automatischen Skalierung

Im nächsten Tutorial erhalten Sie weitere Informationen zu den Konzepten des Lastenausgleichs für virtuelle Computer.

> [!div class="nextstepaction"]
> [Lastenausgleich für virtuelle Computer](tutorial-load-balancer.md)

