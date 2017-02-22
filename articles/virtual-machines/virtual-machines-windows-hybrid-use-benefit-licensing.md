---
title: "Azure-Vorteil bei Hybridnutzung für Windows Server | Microsoft Docs"
description: Erfahren Sie, wie Sie die Vorteile der Windows Server Software Assurance optimal nutzen, um lokale Lizenzen in Azure zu verwenden.
services: virtual-machines-windows
documentationcenter: 
author: iainfoulds
manager: timlt
editor: 
ms.assetid: 332583b6-15a3-4efb-80c3-9082587828b0
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 11/17/2016
ms.author: georgem
translationtype: Human Translation
ms.sourcegitcommit: 7167048a287bee7c26cfc08775dcb84f9e7c2eed
ms.openlocfilehash: df86e73814ceb0c5137c654bce84c8d42ae41820


---
# <a name="azure-hybrid-use-benefit-for-windows-server"></a>Azure-Vorteil bei Hybridnutzung für Windows Server
Sie können für Kunden, die Windows Server mit Software Assurance verwenden, lokale Windows Server-Lizenzen in Azure übertragen und virtuelle Computer mit Windows Server in Azure zu geringeren Kosten ausführen. Durch den Azure-Vorteil bei Hybridnutzung können Sie virtuelle Windows Server-Computer (VMs) in Azure ausführen und müssen nur die Grundgebühr für die Computekapazität bezahlen. Weitere Informationen finden Sie auf der [Lizenzierungsseite für den Azure-Vorteil bei Hybridnutzung](https://azure.microsoft.com/pricing/hybrid-use-benefit/). In diesem Artikel wird die Bereitstellung von virtuellen Windows Server-Computern in Azure zur Nutzung dieses Lizenzierungsvorteils erläutert.


## <a name="ways-to-use-azure-hybrid-use-benefit"></a>Möglichkeiten zur Verwendung des Azure-Vorteils bei Hybridnutzung
Es gibt verschiedene Möglichkeiten zum Bereitstellen von Windows-VMs mit dem Azure-Vorteil bei Hybridnutzung:

1. Wenn Sie über ein Enterprise Agreement-Abonnement verfügen, können Sie [VMs über bestimmte Marketplace-Images bereitstellen](#deploy-a-vm-using-the-azure-marketplace), für die der Azure-Vorteil bei Hybridnutzung vorkonfiguriert ist.
2. Ohne Enterprise Agreement [laden Sie eine benutzerdefinierte VM hoch](#upload-a-windows-server-vhd) und [führen die Bereitstellung per Resource Manager-Vorlage durch](#deploy-a-vm-via-resource-manager) oder nutzen [Azure PowerShell](#detailed-powershell-deployment-walkthrough).

## <a name="deploy-a-vm-using-the-azure-marketplace"></a>Bereitstellen einer VM über den Azure Marketplace
Für Kunden mit [Enterprise Agreement-Abonnements](https://www.microsoft.com/Licensing/licensing-programs/enterprise.aspx) stehen über den Marketplace Images zur Verfügung, für die der Azure-Vorteil bei Hybridnutzung vorkonfiguriert ist. Diese Images können beispielsweise direkt aus dem Azure-Portal, mithilfe von Resource Manager-Vorlagen oder über Azure PowerShell bereitgestellt werden. Die Images sind auf dem Marketplace wie folgt durch den Namenszusatz `[HUB]` gekennzeichnet:

![Azure-Vorteil bei Hybridnutzung – Images auf dem Azure Marketplace](./media/virtual-machines-windows-hybrid-use-benefit/ahub-images-portal.png)

Sie können diese Images direkt über das Azure-Portal bereitstellen. Zeigen Sie die Liste mit den Images zur Verwendung in Resource Manager-Vorlagen und für Azure PowerShell wie folgt an:

```powershell
Get-AzureRMVMImageSku -Location "West US" -Publisher "MicrosoftWindowsServer" `
    -Offer "WindowsServer-HUB"
```

Falls Sie nicht über ein Enterprise Agreement-Abonnement verfügen, können Sie weiterlesen und sich informieren, wie Sie eine benutzerdefinierte VM hochladen und die Bereitstellung mit „Azure-Vorteil bei Hybridnutzung“ durchführen.


## <a name="upload-a-windows-server-vhd"></a>Hochladen einer Windows Server-VHD
Für die Bereitstellung eines virtuellen Windows Server-Computers in Azure müssen Sie zunächst eine virtuelle Festplatte erstellen, die den Windows Server-Basisbuild enthält. Diese virtuelle Festplatte muss entsprechend über Sysprep vorbereitet werden, bevor Sie sie in Azure hochladen. Informieren Sie sich über die [VHD-Anforderungen und den Sysprep-Prozess](virtual-machines-windows-upload-image.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). Weitere Informationen finden Sie auch unter [Sysprep Support for Server Roles](https://msdn.microsoft.com/windows/hardware/commercialize/manufacture/desktop/sysprep-support-for-server-roles) (Sysprep-Unterstützung für Serverrollen). Sichern Sie den virtuellen Computer vor dem Ausführen von Sysprep. 

Stellen Sie sicher, dass Sie die [neueste Azure PowerShell-Version installiert und konfiguriert](/powershell/azureps-cmdlets-docs)haben. Nachdem Sie die virtuelle Festplatte vorbereitet haben, laden Sie sie mit dem `Add-AzureRmVhd`-Cmdlet wie folgt in Ihr Azure Storage-Konto hoch:

```powershell
Add-AzureRmVhd -ResourceGroupName "myResourceGroup" -LocalFilePath "C:\Path\To\myvhd.vhd" `
    -Destination "https://mystorageaccount.blob.core.windows.net/vhds/myvhd.vhd"
```

> [!NOTE]
> Microsoft SQL Server, SharePoint Server und Dynamics können ebenfalls Ihre Software Assurance-Lizenzierung verwenden. Sie müssen weiterhin das Windows Server-Image vorbereiten, indem Sie die Anwendungskomponenten installieren, Lizenzschlüssel bereitstellen und anschließend das Datenträgerimage in Azure hochladen. Lesen Sie in der entsprechenden Dokumentation die Informationen zum Ausführen von Sysprep mit Ihrer Anwendung, etwa [Überlegungen zur Installation von SQL Server mit Sysprep](https://msdn.microsoft.com/library/ee210754.aspx) oder [Build a SharePoint Server 2016 Reference Image (Sysprep)](http://social.technet.microsoft.com/wiki/contents/articles/33789.build-a-sharepoint-server-2016-reference-image-sysprep.aspx) (Erstellen eines SharePoint Server 2016-Referenzimages [Sysprep]).
>
>

Informieren Sie sich auch über das [Hochladen der VHD in Azure](virtual-machines-windows-upload-image.md#upload-the-vhd-to-your-storage-account).


## <a name="deploy-an-uploaded-vm-via-resource-manager"></a>Bereitstellen einer hochgeladenen VM mit Resource Manager
In den Resource Manager-Vorlagen kann ein zusätzlicher Parameter für `licenseType` angegeben werden. Informieren Sie sich weiter über das [Erstellen von Azure Resource Manager-Vorlagen](../azure-resource-manager/resource-group-authoring-templates.md). Nachdem Sie die virtuelle Festplatte in Azure hochgeladen haben, bearbeiten Sie die Resource Manager-Vorlage, um den Lizenztyp als Teil des Computeanbieters einzuschließen, und stellen die Vorlage als normale Vorlage bereit:

```json
"properties": {  
   "licenseType": "Windows_Server",
   "hardwareProfile": {
        "vmSize": "[variables('vmSize')]"
   },
```


## <a name="deploy-an-uploaded-vm-via-powershell-quickstart"></a>Bereitstellen einer hochgeladenen VM per PowerShell-Schnellstart
Beim Bereitstellen des virtuellen Windows Server-Computers über PowerShell ist ein zusätzlicher Parameter für `-LicenseType`vorhanden. Nachdem Sie die virtuelle Festplatte in Azure hochgeladen haben, erstellen Sie einen virtuellen Computer mit `New-AzureRmVM` und geben den Lizenzierungstyp wie folgt an:

```powershell
New-AzureRmVM -ResourceGroupName "myResourceGroup" -Location "West US" -VM $vm -LicenseType "Windows_Server"
```

Eine [ausführliche Anleitung zum Bereitstellen eines virtuellen Computers in Azure mit PowerShell](virtual-machines-windows-hybrid-use-benefit-licensing.md#detailed-powershell-deployment-walkthrough) finden Sie weiter unten. Alternativ können Sie eine ausführliche Anleitung zu den verschiedenen Schritten beim [Erstellen eines virtuellen Windows-Computers mit Resource Manager und PowerShell](virtual-machines-windows-ps-create.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) lesen.


## <a name="verify-your-vm-is-utilizing-the-licensing-benefit"></a>Überprüfen, ob für den virtuellen Computer der Lizenzierungsvorteil genutzt wird
Sobald Sie den virtuellen Computer über die PowerShell- oder Resource Manager-Bereitstellungsmethode bereitgestellt haben, überprüfen Sie die den Lizenztyp wie folgt mit `Get-AzureRmVM` :

```powershell
Get-AzureRmVM -ResourceGroup "myResourceGroup" -Name "myVM"
```

Die Ausgabe sieht in etwa wie das folgende Beispiel aus:

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
Die folgenden ausführlichen Schritte für PowerShell zeigen eine vollständige Bereitstellung eines virtuellen Computers. Weitere Informationen zu den Cmdlets und den verschiedenen erstellten Komponenten finden Sie unter [Erstellen einer Windows-VM mit dem Resource Manager und PowerShell](virtual-machines-windows-ps-create.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). Dabei wird das Erstellen der Ressourcengruppe, des Speicherkontos und des virtuellen Netzwerks beschrieben, anschließend wird der virtuelle Computer definiert und schließlich erstellt.

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

```powershell
New-AzureRmVM -ResourceGroupName $resourceGroupName -Location $location -VM $vm -LicenseType "Windows_Server"
```

## <a name="next-steps"></a>Nächste Schritte
Erfahren Sie mehr zur [Lizenzierung für den Azure-Vorteil bei Hybridnutzung](https://azure.microsoft.com/pricing/hybrid-use-benefit/).

Erfahren Sie mehr zum [Verwenden von Resource Manager-Vorlagen](../azure-resource-manager/resource-group-overview.md).



<!--HONumber=Jan17_HO1-->


