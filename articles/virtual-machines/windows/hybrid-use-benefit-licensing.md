---
title: "Azure-Vorteil bei Hybridnutzung für Windows Server | Microsoft Docs"
description: Erfahren Sie, wie Sie die Vorteile der Windows Software Assurance optimal nutzen, um lokale Lizenzen in Azure zu verwenden.
services: virtual-machines-windows
documentationcenter: 
author: kmouss
manager: timlt
editor: 
ms.assetid: 332583b6-15a3-4efb-80c3-9082587828b0
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 5/26/2017
ms.author: xujing
ms.translationtype: HT
ms.sourcegitcommit: 9569f94d736049f8a0bb61beef0734050ecf2738
ms.openlocfilehash: a986ddf22f059dc55bb9bff5c6eaf27324b716cd
ms.contentlocale: de-de
ms.lasthandoff: 08/31/2017

---
# <a name="azure-hybrid-use-benefit-for-windows-server"></a>Azure-Vorteil bei Hybridnutzung für Windows Server
Für Kunden mit Software Assurance erlaubt die Hybridnutzung von Azure die Verwendung der lokalen Windows Server- und Windows-Clientlizenzen und die Ausführung von virtuellen Windows-Computern in Azure zu reduzierten Kosten. Der Azure-Vorteil bei Hybridnutzung für Windows Server umfasst Windows Server 2008 R2, Windows Server 2012, Windows Server 2012 R2 und Windows Server 2016. Weitere Informationen finden Sie auf der [Lizenzierungsseite für den Azure-Vorteil bei Hybridnutzung](https://azure.microsoft.com/pricing/hybrid-use-benefit/).

> [!NOTE]
> In diesem Artikel erfahren Sie, wie Sie den Lizenzierungsvorteil für Windows Server-Images implementieren. Sie können diese Schritte auch für [Windows 10 Desktop-Images](#windows-desktop-multitenant-hosting-deployment) ausführen.
>

## <a name="ways-to-use-azure-hybrid-use-benefit"></a>Möglichkeiten zur Verwendung des Azure-Vorteils bei Hybridnutzung
Es gibt verschiedene Möglichkeiten zum Bereitstellen von Windows-VMs mit dem Azure-Vorteil bei Hybridnutzung:

1. Sie können virtuelle Computer aus [bestimmten Marketplace-Images] bereitstellen.
2. Sie können [eine benutzerdefinierte VM hochladen](#upload-a-windows-vhd) und [die Bereitstellung per Resource Manager-Vorlage durchführen](#deploy-a-vm-via-resource-manager) oder dazu [Azure PowerShell](#detailed-powershell-deployment-walkthrough) nutzen.

## <a name="deploy-a-vm-using-the-azure-marketplace"></a>Bereitstellen einer VM über den Azure Marketplace
Die folgenden Images sind mit dem Azure-Vorteil bei Hybridnutzung vorkonfiguriert: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012 und Windows Server 2008 SP1. Diese Images können direkt aus dem Azure-Portal, mithilfe von Resource Manager-Vorlagen oder über Azure PowerShell bereitgestellt werden.

Sie können diese Images direkt über das Azure-Portal bereitstellen. Zeigen Sie die Liste mit den Images zur Verwendung in Resource Manager-Vorlagen und für Azure PowerShell wie folgt an:

Für Windows Server:
```powershell
Get-AzureRmVMImagesku -Location westus -PublisherName MicrosoftWindowsServer -Offer WindowsServer
```
- 2016-Datacenter-Version 2016.127.20170406 oder höher

- 2012-R2-Datacenter-Version 4.127.20170406 oder höher

- 2012-Datacenter-Version 3.127.20170406 oder höher

- 2008-R2-SP1-Version 2.127.20170406 oder höher

Für Windows-Clients:
```powershell
Get-AzureRMVMImageSku -Location "West US" -Publisher "MicrosoftWindowsServer" `
    -Offer "Windows-HUB"
```

## <a name="upload-a-windows-server-vhd"></a>Hochladen einer Windows Server-VHD
Für die Bereitstellung eines virtuellen Windows Server-Computers in Azure müssen Sie zunächst eine virtuelle Festplatte (VHD) erstellen, die den Windows-Basisbuild enthält. Diese virtuelle Festplatte muss entsprechend über Sysprep vorbereitet werden, bevor Sie sie in Azure hochladen. Informieren Sie sich über die [VHD-Anforderungen und den Sysprep-Prozess](upload-generalized-managed.md). Weitere Informationen finden Sie auch unter [Sysprep Support for Server Roles](https://msdn.microsoft.com/windows/hardware/commercialize/manufacture/desktop/sysprep-support-for-server-roles) (Sysprep-Unterstützung für Serverrollen). Sichern Sie den virtuellen Computer vor dem Ausführen von Sysprep. 

Stellen Sie sicher, dass Sie die [neueste Azure PowerShell-Version installiert und konfiguriert](/powershell/azure/overview)haben. Nachdem Sie die virtuelle Festplatte vorbereitet haben, laden Sie sie mit dem `Add-AzureRmVhd`-Cmdlet wie folgt in Ihr Azure Storage-Konto hoch:

```powershell
Add-AzureRmVhd -ResourceGroupName "myResourceGroup" -LocalFilePath "C:\Path\To\myvhd.vhd" `
    -Destination "https://mystorageaccount.blob.core.windows.net/vhds/myvhd.vhd"
```

> [!NOTE]
> Microsoft SQL Server, SharePoint Server und Dynamics können ebenfalls Ihre Software Assurance-Lizenzierung verwenden. Sie müssen weiterhin das Windows Server-Image vorbereiten, indem Sie die Anwendungskomponenten installieren, Lizenzschlüssel bereitstellen und anschließend das Datenträgerimage in Azure hochladen. Lesen Sie in der entsprechenden Dokumentation die Informationen zum Ausführen von Sysprep mit Ihrer Anwendung, etwa [Überlegungen zur Installation von SQL Server mit Sysprep](https://msdn.microsoft.com/library/ee210754.aspx) oder [Build a SharePoint Server 2016 Reference Image (Sysprep)](http://social.technet.microsoft.com/wiki/contents/articles/33789.build-a-sharepoint-server-2016-reference-image-sysprep.aspx) (Erstellen eines SharePoint Server 2016-Referenzimages [Sysprep]).
>
>

Informieren Sie sich auch über das [Hochladen der VHD in Azure](upload-generalized-managed.md#upload-the-vhd-to-your-storage-account).


## <a name="deploy-a-vm-via-resource-manager-template"></a>Bereitstellen virtueller Computer mit Resource Manager-Vorlagen
In den Resource Manager-Vorlagen kann ein zusätzlicher Parameter für `licenseType` angegeben werden. Informieren Sie sich weiter über das [Erstellen von Azure Resource Manager-Vorlagen](../../resource-group-authoring-templates.md). Nachdem Sie die virtuelle Festplatte in Azure hochgeladen haben, bearbeiten Sie die Resource Manager-Vorlage, um den Lizenztyp als Teil des Computeanbieters einzuschließen, und stellen die Vorlage als normale Vorlage bereit:

Für Windows Server:
```json
"properties": {  
   "licenseType": "Windows_Server",
   "hardwareProfile": {
        "vmSize": "[variables('vmSize')]"
   }
```

Nur für Windows-Clients mit Azure Marketplace-Image:
```json
"properties": {  
   "licenseType": "Windows_Client",
   "hardwareProfile": {
        "vmSize": "[variables('vmSize')]"
   }
```

## <a name="deploy-a-vm-via-powershell-quickstart"></a>Schnellstartanleitung für das Bereitstellen eines virtuellen Computers über PowerShell
Beim Bereitstellen des virtuellen Windows Server-Computers über PowerShell ist ein zusätzlicher Parameter für `-LicenseType`vorhanden. Nachdem Sie die virtuelle Festplatte in Azure hochgeladen haben, erstellen Sie einen virtuellen Computer mit `New-AzureRmVM` und geben den Lizenzierungstyp wie folgt an:

Für Windows Server:
```powershell
New-AzureRmVM -ResourceGroupName "myResourceGroup" -Location "West US" -VM $vm -LicenseType "Windows_Server"
```

Nur für Windows-Clients mit Azure Marketplace-Image:
```powershell
New-AzureRmVM -ResourceGroupName "myResourceGroup" -Location "West US" -VM $vm -LicenseType "Windows_Client"
```

Eine [ausführliche Anleitung zum Bereitstellen eines virtuellen Computers in Azure mit PowerShell](hybrid-use-benefit-licensing.md#detailed-powershell-deployment-walkthrough) finden Sie weiter unten. Alternativ können Sie eine ausführliche Anleitung zu den verschiedenen Schritten beim [Erstellen eines virtuellen Windows-Computers mit Resource Manager und PowerShell](../virtual-machines-windows-ps-create.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) lesen.


## <a name="verify-your-vm-is-utilizing-the-licensing-benefit"></a>Überprüfen, ob für den virtuellen Computer der Lizenzierungsvorteil genutzt wird
Sobald Sie den virtuellen Computer über die PowerShell- oder Resource Manager-Bereitstellungsmethode bereitgestellt haben, überprüfen Sie die den Lizenztyp wie folgt mit `Get-AzureRmVM` :

```powershell
Get-AzureRmVM -ResourceGroup "myResourceGroup" -Name "myVM"
```

Die Ausgabe sieht in etwa wie das folgende Beispiel für Windows Server aus:

```powershell
Type                     : Microsoft.Compute/virtualMachines
Location                 : westus
LicenseType              : Windows_Server
```

Diese Ausgabe steht im Gegensatz zu den folgenden virtuellen Computern, die ohne Lizenzierung für den Azure-Vorteil bei Hybridnutzung bereitgestellt wurden, z.B. ein virtueller Computer, der direkt aus dem Azure-Katalog bereitgestellt wurde:

```powershell
Type                     : Microsoft.Compute/virtualMachines
Location                 : westus
LicenseType              :
```

## <a name="detailed-powershell-deployment-walkthrough"></a>PowerShell-Bereitstellung – Ausführliche exemplarische Vorgehensweise
Die folgenden ausführlichen Schritte für PowerShell zeigen eine vollständige Bereitstellung eines virtuellen Computers. Weitere Informationen zu den Cmdlets und den verschiedenen erstellten Komponenten finden Sie unter [Erstellen einer Windows-VM mit dem Resource Manager und PowerShell](../virtual-machines-windows-ps-create.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). Dabei wird das Erstellen der Ressourcengruppe, des Speicherkontos und des virtuellen Netzwerks beschrieben, anschließend wird der virtuelle Computer definiert und schließlich erstellt.

Rufen Sie zunächst sicher Anmeldeinformationen ab, und legen Sie einen Standort und einen Namen für die Ressourcengruppe fest:

```powershell
$cred = Get-Credential
$location = "West US"
$resourceGroupName = "myResourceGroup"
```

Erstellen Sie eine öffentliche IP-Adresse:

```powershell
$publicIPName = "myPublicIP"
$publicIP = New-AzureRmPublicIpAddress -Name $publicIPName -ResourceGroupName $resourceGroupName `
    -Location $location -AllocationMethod "Dynamic"
```

Definieren Sie Subnetz, NIC und VNET:

```powershell
$subnetName = "mySubnet"
$nicName = "myNIC"
$vnetName = "myVnet"
$subnetconfig = New-AzureRmVirtualNetworkSubnetConfig -Name $subnetName -AddressPrefix 10.0.0.0/8
$vnet = New-AzureRmVirtualNetwork -Name $vnetName -ResourceGroupName $resourceGroupName -Location $location `
    -AddressPrefix 10.0.0.0/8 -Subnet $subnetconfig
$nic = New-AzureRmNetworkInterface -Name $nicName -ResourceGroupName $resourceGroupName -Location $location `
    -SubnetId $vnet.Subnets[0].Id -PublicIpAddressId $publicIP.Id
```

Benennen Sie den virtuellen Computer, und erstellen Sie eine VM-Konfiguration:

```powershell
$vmName = "myVM"
$vmConfig = New-AzureRmVMConfig -VMName $vmName -VMSize "Standard_A1"
```

Definieren Sie das Betriebssystem:

```powershell
$computerName = "myVM"
$vm = Set-AzureRmVMOperatingSystem -VM $vmConfig -Windows -ComputerName $computerName -Credential $cred `
    -ProvisionVMAgent -EnableAutoUpdate
```

Fügen Sie die Netzwerkschnittstelle dem virtuellen Computer hinzu.

```powershell
$vm = Add-AzureRmVMNetworkInterface -VM $vm -Id $nic.Id
```

Definieren Sie das zu verwendende Speicherkonto:

```powershell
$storageAcc = Get-AzureRmStorageAccount -ResourceGroupName $resourceGroupName -AccountName mystorageaccount
```

Laden Sie die VHD entsprechend vorbereitet hoch, und fügen Sie sie zur Verwendung an den virtuellen Computer an:

```powershell
$osDiskName = "licensing.vhd"
$osDiskUri = '{0}vhds/{1}{2}.vhd' -f $storageAcc.PrimaryEndpoints.Blob.ToString(), $vmName.ToLower(), $osDiskName
$urlOfUploadedImageVhd = "https://mystorageaccount.blob.core.windows.net/vhd/myvhd.vhd"
$vm = Set-AzureRmVMOSDisk -VM $vm -Name $osDiskName -VhdUri $osDiskUri -CreateOption FromImage `
    -SourceImageUri $urlOfUploadedImageVhd -Windows
```

Erstellen Sie schließlich den virtuellen Computer, und definieren Sie den Lizenzierungstyp so, dass der Azure-Vorteil bei Hybridnutzung verwendet wird:

Für Windows Server:
```powershell
New-AzureRmVM -ResourceGroupName $resourceGroupName -Location $location -VM $vm -LicenseType "Windows_Server"
```

## <a name="deploy-a-virtual-machine-scale-set-via-resource-manager-template"></a>Bereitstellen einer VM-Skalierungsgruppe mit einer Resource Manager-Vorlage
In den VMSS-Resource Manager-Vorlagen muss ein zusätzlicher Parameter für `licenseType` angegeben werden. Informieren Sie sich weiter über das [Erstellen von Azure Resource Manager-Vorlagen](../../resource-group-authoring-templates.md). Bearbeiten Sie Ihre Resource Manager-Vorlage so, dass die „LicenseType“-Eigenschaft als Teil von „VirtualMachineProfile“ der Skalierungsgruppe hinzugefügt wird, und stellen Sie Ihre Vorlage als „normal“ bereit. Siehe das folgende Beispiel unter Verwendung eines Windows Server 2016-Images:


```json
"virtualMachineProfile": {
    "storageProfile": {
        "osDisk": {
            "createOption": "FromImage"
        },
        "imageReference": {
            "publisher": "MicrosoftWindowsServer",
            "offer": "WindowsServer",
            "sku": "2016-Datacenter",
            "version": "latest"
        }
    },
    "licenseType": "Windows_Server",
    "osProfile": {
            "computerNamePrefix": "[parameters('vmssName')]",
            "adminUsername": "[parameters('adminUsername')]",
            "adminPassword": "[parameters('adminPassword')]"
    }
```

> [!NOTE]
> Unterstützung für die Bereitstellung einer VM-Skalierungsgruppe mit AHUB-Vorteilen über PowerShell und andere SDK-Tools wird bald verfügbar sein.
>

## <a name="next-steps"></a>Nächste Schritte
Erfahren Sie mehr zur [Lizenzierung für den Azure-Vorteil bei Hybridnutzung](https://azure.microsoft.com/pricing/hybrid-use-benefit/).

Erfahren Sie mehr zum [Verwenden von Resource Manager-Vorlagen](../../azure-resource-manager/resource-group-overview.md).

Erfahren Sie mehr darüber, [wie Sie mithilfe des Azure-Vorteils bei Hybridnutzung und Azure Site Recovery das Migrieren von Anwendungen in Azure wirtschaftlicher gestalten können](https://azure.microsoft.com/blog/hybrid-use-benefit-migration-with-asr/).

