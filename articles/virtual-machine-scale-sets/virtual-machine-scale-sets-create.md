---
title: Erstellen einer Azure-VM-Skalierungsgruppe | Microsoft-Dokumentation
description: Erstellen und Bereitstellen einer Linux- oder Windows-Azure VM-Skalierungsgruppe mit der Azure-Befehlszeilenschnittstelle, PowerShell, einer Vorlage oder Visual Studio.
services: virtual-machine-scale-sets
documentationcenter: 
author: Thraka
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machine-scale-sets
ms.workload: infrastructure-services
ms.tgt_pltfrm: na
ms.devlang: azurecli
ms.topic: article
ms.date: 05/23/2017
ms.author: adegeo
ms.translationtype: Human Translation
ms.sourcegitcommit: aaf97d26c982c1592230096588e0b0c3ee516a73
ms.openlocfilehash: 736918ea310f276d961fa396f719b2b7809f0c0f
ms.contentlocale: de-de
ms.lasthandoff: 04/27/2017

---

# <a name="create-and-deploy-a-virtual-machine-scale-set"></a>Erstellen und Bereitstellen einer VM-Skalierungsgruppe
VM-Skalierungsgruppen erleichtern das Bereitstellen und Verwalten identischer virtueller Computer als Gruppe. Skalierungsgruppen stellen eine hoch skalierbare und anpassbare Compute-Ebene für Anwendungen mit Hyperskalierung bereit und bieten Unterstützung für Images der Windows-Plattform, Images der Linux-Plattform, benutzerdefinierte Images und Erweiterungen. Weitere Informationen zu Skalierungsgruppen finden Sie unter [VM-Skalierungsgruppen](virtual-machine-scale-sets-overview.md).

In diesem Tutorial erfahren Sie, wie Sie eine VM-Skalierungsgruppe erstellen können, **ohne** das Azure-Portal zu verwenden. Informationen dazu, wie das Azure-Portal verwendet wird, finden Sie unter [Erstellen einer VM-Skalierungsgruppe mit dem Azure-Portal](virtual-machine-scale-sets-portal-create.md).

>[!NOTE]
>Weitere Informationen über Azure Resource Manager-Ressourcen finden Sie unter [Azure Resource Manager im Vergleich zur klassischen Bereitstellung](../azure-resource-manager/resource-manager-deployment-model.md).

## <a name="sign-in-to-azure"></a>Anmelden bei Azure

Wenn Sie Azure CLI 2.0 oder Azure PowerShell verwenden, um eine Skalierungsgruppe zu erstellen, müssen Sie sich zunächst bei Ihrem Abonnement anmelden.

Weitere Informationen zum Installieren von, Einrichten von und Anmelden bei Azure mit Azure CLI oder PowerShell finden Sie unter [Erste Schritte mit Azure CLI 2.0](/cli/azure/get-started-with-azure-cli.md) oder [Erste Schritte mit Azure PowerShell-Cmdlets](/powershell/azure/overview).

```azurecli
az login
```

```powershell
Login-AzureRmAccount
```

## <a name="create-a-resource-group"></a>Erstellen einer Ressourcengruppe

Sie müssen zuerst eine Ressourcengruppe erstellen, der die VM-Skalierungsgruppe zugeordnet ist.

```azurecli
az group create --location westus2 --name MyResourceGroup1
```

```powershell
New-AzureRmResourceGroup -Location westus2 -Name MyResourceGroup1
```

## <a name="create-from-azure-cli"></a>Erstellen über Azure CLI

Mit Azure CLI können Sie eine VM-Skalierungsgruppe mit minimalem Aufwand erstellen. Für Optionen, für die Sie keine Werte angeben, werden Standardwerte verwendet. Wenn Sie beispielsweise keine Informationen zum virtuellen Netzwerk angeben, werden diese für Sie erstellt. Wenn Sie die folgenden Elemente nicht angeben, werden sie für Sie erstellt: 
- Ein Load Balancer
- Ein virtuelles Netzwerk
- Eine öffentliche IP-Adresse

Bei der Auswahl des Image des virtuellen Computers, den Sie für die VM-Skalierungsgruppe verwenden möchten, sind folgende Optionen verfügbar:

- URN  
Der Bezeichner einer Ressource:  
**Win2012R2Datacenter**

- URN-Alias  
Der Anzeigename eines URN:  
**MicrosoftWindowsServer:WindowsServer:2012-R2-Datacenter:latest**

- Benutzerdefinierte Ressourcen-ID  
Der Pfad zu einer Azure-Ressource:  
**/subscriptions/subscription-guid/resourceGroups/MyResourceGroup/providers/Microsoft.Compute/images/MyImage**

- Webressource  
Der Pfad zu einer HTTP-URI:  
**http://contoso.blob.core.windows.net/vhds/osdiskimage.vhd**

>[!TIP]
>Eine Liste der verfügbaren Images können Sie mit `az vm image list` abrufen.

Zum Erstellen einer VM-Skalierungsgruppe müssen Sie Folgendes angeben:

- Ressourcengruppe 
- Name
- Betriebssystemimage
- Authentifizierungsinformationen 
 
Mit dem folgenden Beispiel wird eine elementare VM-Skalierungsgruppe erstellt (dieser Schritt kann einige Minuten dauern).

```azurecli
az vmss create --resource-group MyResourceGroup1 --name MyScaleSet --image UbuntuLTS --authentication-type password --admin-username azureuser --admin-password P@ssw0rd!
```

Nachdem der Befehl ausgeführt wurde, müssen Sie die VM-Skalierungsgruppe erstellen. Möglicherweise müssen Sie die IP-Adresse des virtuellen Computers abrufen, um eine Verbindung herstellen zu können. Mit dem folgenden Befehl können Sie verschiedene Informationen zum virtuellen Computer (einschließlich der IP-Adresse) abrufen. 

```azurecli
az vmss list-instance-connection-info --resource-group MyResourceGroup1 --name MyScaleSet
```

## <a name="create-from-powershell"></a>Erstellen über PowerShell

PowerShell ist in der Verwendung komplizierter als die Azure-Befehlszeilenschnittstelle. Während über die Azure-Befehlszeilenschnittstelle Standardeinstellungen für netzwerkbezogene Ressourcen (z.B. Load Balancer, IP-Adressen, virtuelle Netzwerke) bereitgestellt werden, ist dies in PowerShell nicht der Fall. Auch das Verweisen auf ein Image mit PowerShell ist etwas komplizierter. Sie können Images mit den folgenden Cmdlets abrufen:

1. Get-AzureRMVMImagePublisher
2. Get-AzureRMVMImageOffer
3. Get-AzureRmVMImageSku

Die Cmdlets können beim Ausführen der Reihe nach kann verkettet werden (Pipe). Es folgt ein Beispiel, wie alle Images für die Region **WestUS2** abgerufen werden können, deren Veröffentlicher den Namen **microsoft** enthält.

```powershell
Get-AzureRMVMImagePublisher -Location WestUS2 | Where-Object PublisherName -Like *microsoft* | Get-AzureRMVMImageOffer | Get-AzureRmVMImageSku | Select-Object PublisherName, Offer, Skus
```

```
PublisherName              Offer                    Skus
-------------              -----                    ----
microsoft-ads              linux-data-science-vm    linuxdsvm
microsoft-ads              standard-data-science-vm standard-data-science-vm
MicrosoftAzureSiteRecovery Process-Server           Windows-2012-R2-Datacenter
MicrosoftBizTalkServer     BizTalk-Server           2013-R2-Enterprise
MicrosoftBizTalkServer     BizTalk-Server           2013-R2-Standard
MicrosoftBizTalkServer     BizTalk-Server           2016-Developer
MicrosoftBizTalkServer     BizTalk-Server           2016-Enterprise
...
```

Der Workflow zum Erstellen einer VM-Skalierungsgruppe gestaltet sich wie folgt:

1. Erstellen Sie ein Konfigurationsobjekt, das Informationen über die Skalierungsgruppe enthält.
2. Verweisen Sie auf das Betriebssystem-Basisimage.
3. Konfigurieren Sie die Einstellungen des Betriebssystems: Authentifizierung, VM-Namenspräfix und Benutzer/Kennwort.
4. Konfigurieren Sie die Netzwerkeinstellungen.
5. Erstellen Sie die Skalierungsgruppe.

Mit diesem Beispiel wird eine elementare Skalierungsgruppe mit zwei Instanzen für einen Computer erstellt, auf dem Windows Server 2016 installiert ist.

```powershell
# Resource group name from above
$rg = "MyResourceGroup1"
$location = "WestUS2"

# Create a config object
$vmssConfig = New-AzureRmVmssConfig -Location $location -SkuCapacity 2 -SkuName Standard_A0  -UpgradePolicyMode Automatic

# Reference a virtual machine image from the gallery
Set-AzureRmVmssStorageProfile $vmssConfig -ImageReferencePublisher MicrosoftWindowsServer -ImageReferenceOffer WindowsServer -ImageReferenceSku 2016-Datacenter -ImageReferenceVersion latest

# Set up information for authenticating with the virtual machine
Set-AzureRmVmssOsProfile $vmssConfig -AdminUsername azureuser -AdminPassword P@ssw0rd! -ComputerNamePrefix myvmssvm

# Create the virtual network resources

## Basics
$subnet = New-AzureRmVirtualNetworkSubnetConfig -Name "my-subnet" -AddressPrefix 10.0.0.0/24
$vnet = New-AzureRmVirtualNetwork -Name "my-network" -ResourceGroupName $rg -Location $location -AddressPrefix 10.0.0.0/16 -Subnet $subnet

## Load balancer
$publicIP = New-AzureRmPublicIpAddress -Name "PublicIP" -ResourceGroupName $rg -Location $location -AllocationMethod Static -DomainNameLabel "myuniquedomain"
$frontendIP = New-AzureRmLoadBalancerFrontendIpConfig -Name "LB-Frontend" -PublicIpAddress $publicIP
$backendPool = New-AzureRmLoadBalancerBackendAddressPoolConfig -Name "LB-backend"
$probe = New-AzureRmLoadBalancerProbeConfig -Name "HealthProbe" -Protocol Tcp -Port 80 -IntervalInSeconds 15 -ProbeCount 2
$inboundNATRule1= New-AzureRmLoadBalancerRuleConfig -Name "webserver" -FrontendIpConfiguration $frontendIP -Protocol Tcp -FrontendPort 80 -BackendPort 80 -IdleTimeoutInMinutes 15 -Probe $probe -BackendAddressPool $backendPool
$inboundNATPool1 = New-AzureRmLoadBalancerInboundNatPoolConfig -Name "RDP" -FrontendIpConfigurationId $frontendIP.Id -Protocol TCP -FrontendPortRangeStart 53380 -FrontendPortRangeEnd 53390 -BackendPort 3389

New-AzureRmLoadBalancer -ResourceGroupName $rg -Name "LB1" -Location $location -FrontendIpConfiguration $frontendIP -LoadBalancingRule $inboundNATRule1 -InboundNatPool $inboundNATPool1 -BackendAddressPool $backendPool -Probe $probe

## IP address config
$ipConfig = New-AzureRmVmssIpConfig -Name "my-ipaddress" -LoadBalancerBackendAddressPoolsId $backendPool.Id -SubnetId $vnet.Subnets[0].Id -LoadBalancerInboundNatPoolsId $inboundNATPool1.Id

# Attach the virtual network to the IP object
Add-AzureRmVmssNetworkInterfaceConfiguration -VirtualMachineScaleSet $vmssConfig -Name "network-config" -Primary $true -IPConfiguration $ipConfig

# Create the scale set with the config object (this step might take a few minutes)
New-AzureRmVmss -ResourceGroupName $rg -Name "MyScaleSet1" -VirtualMachineScaleSet $vmssConfig
```

## <a name="create-from-a-template"></a>Erstellen über eine Vorlage

Sie können eine VM-Skalierungsgruppe über eine Azure Resource Manager-Vorlage bereitstellen. Sie können eine eigene Vorlage erstellen oder eine Vorlage aus dem [Vorlagenrepository](https://azure.microsoft.com/resources/templates/?term=vmss) verwenden. Diese Vorlagen können direkt in Ihrem Azure-Abonnement bereitgestellt werden.

>[!NOTE]
>Um eine eigene Vorlage zu erstellen, erstellen Sie eine JSON-Textdatei. Allgemeine Informationen zum Erstellen und Anpassen einer Vorlage finden Sie unter [Vorlagen für Azure Resource Manager](../azure-resource-manager/resource-group-authoring-templates.md).

Eine Beispielvorlage finden Sie [auf GitHub](https://github.com/gatneil/mvss/tree/minimum-viable-scale-set). Weitere Informationen zum Erstellen und Verwenden dieses Beispiels finden Sie unter [Kleinstmögliche Skalierungsgruppe](.\virtual-machine-scale-sets-mvss-start.md).

## <a name="create-from-visual-studio"></a>Erstellen über Visual Studio

Mit Visual Studio können Sie ein Projekt für eine Azure-Ressourcengruppe erstellen und diesem Projekt eine Vorlage für eine VM-Skalierungsgruppe hinzufügen. Sie können auswählen, ob die Vorlage aus GitHub oder aus der Azure Web App Gallery importiert werden soll. Außerdem wird ein PowerShell-Bereitstellungsskript für Sie generiert. Weitere Informationen finden Sie unter [Erstellen einer VM-Skalierungsgruppe mit Visual Studio](virtual-machine-scale-sets-vs-create.md).

## <a name="create-from-the-azure-portal"></a>Erstellen über das Azure-Portal

Das Azure-Portal bietet eine komfortable Möglichkeit, schnell eine Skalierungsgruppe zu erstellen. Weitere Informationen finden Sie unter [Erstellen einer VM-Skalierungsgruppe mit dem Azure-Portal](virtual-machine-scale-sets-portal-create.md).

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu [Datenträgern](virtual-machine-scale-sets-attached-disks.md).

Informationen zum [Verwalten von Apps](virtual-machine-scale-sets-deploy-app.md).

