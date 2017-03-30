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
ms.devlang: na
ms.topic: article
ms.date: 03/20/2017
ms.author: adegeo
translationtype: Human Translation
ms.sourcegitcommit: 424d8654a047a28ef6e32b73952cf98d28547f4f
ms.openlocfilehash: 949e59b64557ac3efc07da73a205c808e25aeaab
ms.lasthandoff: 03/22/2017

---

# <a name="create-and-deploy-a-virtual-machine-scale-set"></a>Erstellen und Bereitstellen einer VM-Skalierungsgruppe
VM-Skalierungsgruppen erleichtern das Bereitstellen und Verwalten identischer virtueller Computer als Gruppe. Skalierungsgruppen stellen eine hoch skalierbare und anpassbare Compute-Ebene für Anwendungen mit Hyperskalierung bereit und bieten Unterstützung für Images der Windows-Plattform, Images der Linux-Plattform, benutzerdefinierte Images und Erweiterungen. Weitere Informationen zu Skalierungsgruppen finden Sie unter [VM-Skalierungsgruppen](virtual-machine-scale-sets-overview.md).

In diesem Tutorial erfahren Sie, wie Sie eine VM-Skalierungsgruppe erstellen können, **ohne** das Azure-Portal zu verwenden. Informationen dazu, wie das Azure-Portal verwendet wird, finden Sie unter [Erstellen einer VM-Skalierungsgruppe mit dem Azure-Portal](virtual-machine-scale-sets-portal-create.md).

>[!NOTE]
>Weitere Informationen über Azure Resource Manager-Ressourcen finden Sie unter [Azure Resource Manager im Vergleich zur klassischen Bereitstellung](../azure-resource-manager/resource-manager-deployment-model.md).

## <a name="log-in-to-azure"></a>Anmelden bei Azure

Wenn Sie die Azure CLI 2.0 oder PowerShell verwenden, um eine Skalierungsgruppe zu erstellen, müssen Sie sich zunächst bei Ihrem Abonnement anmelden.

Weitere Informationen zum Installieren von, Einrichten von und Anmelden bei Azure mit Azure CLI 2.0 oder PowerShell finden Sie unter [Erste Schritte mit Azure CLI 2.0](/cli/azure/get-started-with-azure-cli.md) oder [Erste Schritte mit Azure PowerShell-Cmdlets](/powershell/resourcemanager/).

```azurecli
az login
```

```powershell
Login-AzureRmAccount
```

## <a name="prep-create-a-resource-group"></a>Vorbereitung: Erstellen einer Ressourcengruppe

Sie müssen zuerst eine Ressourcengruppe erstellen, der die VM-Skalierungsgruppe zugeordnet ist.

```azurecli
az group create --location westus2 --name vmss-test-1
```

```powershell
New-AzureRmResourceGroup -Location westus2 -Name vmss-test-1
```

## <a name="create-from-azure-cli"></a>Erstellen über Azure CLI

Mit Azure CLI können Sie eine VM-Skalierungsgruppe mit minimalem Aufwand erstellen. Für alle Einstellungen, für die Sie keine Werte angeben, werden Standardwerte bereitgestellt. Wenn Sie beispielsweise keine Informationen zum virtuellen Netzwerk angeben, werden diese für Sie erstellt. Wenn Sie die folgenden Teile nicht angeben, werden diese für Sie erstellt: ein Load Balancer, ein VNET und eine öffentliche IP-Adresse.

Bei der Auswahl des Image des virtuellen Computers, den Sie für die VM-Skalierungsgruppe verwenden möchten, haben Sie einige Optionen:

1. URN  
Der Bezeichner einer Ressource:  
**Win2012R2Datacenter**.

2. URN-Alias  
Der Anzeigename eines URN:  
**MicrosoftWindowsServer:WindowsServer:2012-R2-Datacenter:latest**.

3. Benutzerdefinierte Ressourcen-ID  
Der Pfad zu einer Azure-Ressource:  
**/subscriptions/subscription-guid/resourceGroups/MyResourceGroup/providers/Microsoft.Compute/images/MyImage**.

4. Webressource  
Der Pfad zu einer HTTP-URI:  
**http://contoso.blob.core.windows.net/vhds/osdiskimage.vhd**.

>[!TIP]
>Eine Liste der verfügbaren Images können Sie mit `az vm image list` abrufen.

Um eine VM-Skalierungsgruppe zu erstellen, müssen Sie Folgendes angeben: _Ressourcengruppe_, _Name_, _Betriebssystemimage_ und _Authentifizierungsinformationen_. Das folgende Beispiel erstellt eine elementare VM-Skalierungsgruppe (dieser Schritt kann einige Minuten dauern).

```azurecli
az vmss create --resource-group vmss-test-1 --name MyScaleSet --image UbuntuLTS --authentication-type password --admin-username azureuser --admin-password P@ssw0rd!
```

## <a name="create-from-powershell"></a>Erstellen über PowerShell

PowerShell ist komplizierter zu verwenden als die Azure CLI. Während die Azure CLI Standardeinstellungen für netzwerkbezogene Ressourcen (Load Balancer, IP-Adresse, ein virtuelles Netzwerk) bereitstellt, ist dies in PowerShell nicht der Fall. Verweisen auf ein Image ist ebenfalls etwas komplizierter. Sie können Images mit den folgenden Cmdlets abrufen:

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

Der Workflow zum Erstellen einer VM-Skalierungsgruppe ist wie folgt:

1. Erstellen Sie ein Konfigurationsobjekt, das Informationen über die Skalierungsgruppe enthält.
2. Verweisen Sie auf ein Betriebssystem-Basisimage.
3. Konfigurieren Sie die Einstellungen des Betriebssystems: Authentifizierung, VM-Namenspräfix, Benutzer/Kennwort.
4. Konfigurieren Sie die Netzwerkeinstellungen.
5. Erstellen Sie die Skalierungsgruppe.

Dieses Beispiel erstellt eine elementare 2-Instanz-Skalierungsgruppe mit einer Installation von Windows Server 2016.

```powershell
# Create a config object
$vmssConfig = New-AzureRmVmssConfig -Location WestUS2 -SkuCapacity 2 -SkuName Standard_A0  -UpgradePolicyMode Automatic

# Reference a virtual machine image from the gallery
Set-AzureRmVmssStorageProfile $vmssConfig -ImageReferencePublisher MicrosoftWindowsServer -ImageReferenceOffer WindowsServer -ImageReferenceSku 2016-Datacenter -ImageReferenceVersion latest

# Setup information about how to authenticate with the virtual machine
Set-AzureRmVmssOsProfile $vmssConfig -AdminUsername azureuser -AdminPassword P@ssw0rd! -ComputerNamePrefix myvmssvm

# Create the virtual network resources
$subnet = New-AzureRmVirtualNetworkSubnetConfig -Name "my-subnet" -AddressPrefix 10.0.0.0/24
$vnet = New-AzureRmVirtualNetwork -Name "my-network" -ResourceGroupName "vmss-test-1" -Location "westus2" -AddressPrefix 10.0.0.0/16 -Subnet $subnet
$ipConfig = New-AzureRmVmssIpConfig -Name "my-ip-address" -LoadBalancerBackendAddressPoolsId $null -SubnetId $vnet.Subnets[0].Id

# Attach the virtual network to the config object
Add-AzureRmVmssNetworkInterfaceConfiguration -VirtualMachineScaleSet $vmssConfig -Name "network-config" -Primary $true -IPConfiguration $ipConfig

# Create the scale set with the config object (this step may take a few minutes)
New-AzureRmVmss -ResourceGroupName vmss-test-1 -Name my-scale-set -VirtualMachineScaleSet $vmssConfig
```

## <a name="create-from-a-template"></a>Erstellen über eine Vorlage

Sie können eine VM-Skalierungsgruppe bereitstellen, indem Sie eine Azure Resource Manager-Vorlage verwenden. Sie können eine eigene Vorlage erstellen oder eine Vorlage aus dem [Vorlagenrepository](https://azure.microsoft.com/resources/templates/?term=vmss) verwenden. Diese Vorlagen können direkt in Ihrem Azure-Abonnement bereitgestellt werden.

>[!NOTE]
>Um eine eigene Vorlage zu erstellen, erstellen Sie eine _.json_-Textdatei. Allgemeine Informationen zum Erstellen und Anpassen einer Vorlage finden Sie unter [Vorlagen für Azure Resource Manager](../azure-resource-manager/resource-group-authoring-templates.md).

Eine Beispielvorlage finden Sie [auf GitHub](https://github.com/gatneil/mvss/tree/minimum-viable-scale-set). Weitere Informationen zum Erstellen und Verwenden dieses Beispiel finden Sie unter [Einfachste funktionsfähige Skalierungsgruppe](.\virtual-machine-scale-sets-mvss-start.md).

## <a name="create-from-visual-studio"></a>Erstellen über Visual Studio

Mit Visual Studio können Sie ein Projekt für eine Azure-Ressourcengruppe erstellen und diesem Projekt eine VM-Skalierungsgruppenvorlage hinzufügen. Sie können auswählen, welche Vorlage importiert werden soll, etwa aus GitHub oder aus dem Azure-Katalog. Außerdem wird ein PowerShell-Bereitstellungsskript für Sie generiert. Weitere Informationen finden Sie unter [Erstellen einer VM-Skalierungsgruppe mit Visual Studio](virtual-machine-scale-sets-vs-create.md).

## <a name="create-from-the-azure-portal"></a>Erstellen über das Azure-Portal

Das Azure-Portal bietet eine komfortable Möglichkeit, schnell eine Skalierungsgruppe zu erstellen. Weitere Informationen finden Sie unter [Erstellen einer VM-Skalierungsgruppe mit dem Azure-Portal](virtual-machine-scale-sets-portal-create.md).

