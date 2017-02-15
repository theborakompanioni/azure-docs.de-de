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
ms.date: 07/13/2016
ms.author: georgem
translationtype: Human Translation
ms.sourcegitcommit: 5919c477502767a32c535ace4ae4e9dffae4f44b
ms.openlocfilehash: 5c40b318be2503fe2ec05e9f2a5a09c46b88a0dc


---
# <a name="azure-hybrid-use-benefit-for-windows-server"></a>Azure-Vorteil bei Hybridnutzung für Windows Server
Sie können für Kunden, die Windows Server mit Software Assurance verwenden, lokale Windows Server-Lizenzen in Azure übertragen und virtuelle Computer mit Windows Server in Azure zu geringeren Kosten ausführen. Durch den Azure-Vorteil bei Hybridnutzung können Sie virtuelle Windows Server-Computer in Azure ausführen und müssen nur die Grundgebühr für die Computekapazität bezahlen. Weitere Informationen finden Sie auf der [Lizenzierungsseite für den Azure-Vorteil bei Hybridnutzung](https://azure.microsoft.com/pricing/hybrid-use-benefit/). In diesem Artikel wird die Bereitstellung von virtuellen Windows Server-Computern in Azure zur Nutzung dieses Lizenzierungsvorteils erläutert.

> [!NOTE]
> Sie können keine Azure Marketplace-Images verwenden, um virtuelle Windows Server-Computer bereitzustellen, und dabei den Azure-Vorteil bei Hybridnutzung nutzen. Sie müssen Ihre virtuellen Computer mit PowerShell- oder Resource Manager-Vorlagen bereitstellen, um die virtuellen Computer ordnungsgemäß als für die rabattierte Grundgebühr für Computekapazität berechtigt zu registrieren.
>
>

## <a name="pre-requisites"></a>Voraussetzungen
Es gibt einige Voraussetzungen, um den Azure-Vorteil bei Hybridnutzung für virtuelle Windows Server-Computer in Azure nutzen zu können:

* Azure PowerShell-Modul ist installiert.
* Windows Server-VHD wurde in Azure Storage hochgeladen.

### <a name="install-azure-powershell"></a>Installieren von Azure PowerShell
Stellen Sie sicher, dass Sie die [neueste Azure PowerShell-Version installiert und konfiguriert](../powershell-install-configure.md)haben. Auch wenn Sie Ihre virtuellen Computer mit Resource Manager-Vorlagen bereitstellen möchten, muss Azure PowerShell installiert sein, um die Windows Server-VHD hochladen zu können (siehe nächsten Schritt weiter unten).

### <a name="upload-a-windows-server-vhd"></a>Hochladen einer Windows Server-VHD
Für die Bereitstellung eines virtuellen Windows Server-Computers in Azure müssen Sie zunächst eine virtuelle Festplatte erstellen, die den Windows Server-Basisbuild enthält. Diese virtuelle Festplatte muss entsprechend über Sysprep vorbereitet werden, bevor Sie sie in Azure hochladen. Informieren Sie sich über die [VHD-Anforderungen und den Sysprep-Prozess](virtual-machines-windows-upload-image.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). Weitere Informationen finden Sie auch unter [Sysprep Support for Server Roles](https://msdn.microsoft.com/windows/hardware/commercialize/manufacture/desktop/sysprep-support-for-server-roles) (Sysprep-Unterstützung für Serverrollen). Sichern Sie den virtuellen Computer vor dem Ausführen von Sysprep. Nachdem Sie die virtuelle Festplatte vorbereitet haben, laden Sie sie mit dem `Add-AzureRmVhd`-Cmdlet wie folgt in Ihr Azure Storage-Konto hoch:

```powershell
Add-AzureRmVhd -ResourceGroupName "myResourceGroup" -LocalFilePath "C:\Path\To\myvhd.vhd" `
    -Destination "https://mystorageaccount.blob.core.windows.net/vhds/myvhd.vhd"
```

> [!NOTE]
> Microsoft SQL Server, SharePoint Server und Dynamics können ebenfalls Ihre Software Assurance-Lizenzierung verwenden. Sie müssen weiterhin das Windows Server-Image vorbereiten, indem Sie die Anwendungskomponenten installieren, Lizenzschlüssel bereitstellen und anschließend das Datenträgerimage in Azure hochladen. Lesen Sie in der entsprechenden Dokumentation die Informationen zum Ausführen von Sysprep mit Ihrer Anwendung, etwa [Überlegungen zur Installation von SQL Server mit Sysprep](https://msdn.microsoft.com/library/ee210754.aspx) oder [Build a SharePoint Server 2016 Reference Image (Sysprep)](http://social.technet.microsoft.com/wiki/contents/articles/33789.build-a-sharepoint-server-2016-reference-image-sysprep.aspx) (Erstellen eines SharePoint Server 2016-Referenzimages [Sysprep]).
>
>

Informieren Sie sich auch über das [Hochladen der VHD in Azure](virtual-machines-windows-upload-image.md#upload-the-vhd-to-your-storage-account).

> [!TIP]
> Thema dieses Artikels ist die Bereitstellung von Windows Server-VMs. Sie können auch Windows-Client-VMs in gleicher Weise bereitstellen. Ersetzen Sie in den folgenden Beispielen entsprechend `Server` durch `Client`.
>
>

## <a name="deploy-a-vm-via-powershell-quick-start"></a>Schnellstartanleitung für das Bereitstellen eines virtuellen Computers über PowerShell
Beim Bereitstellen des virtuellen Windows Server-Computers über PowerShell ist ein zusätzlicher Parameter für `-LicenseType`vorhanden. Nachdem Sie die virtuelle Festplatte in Azure hochgeladen haben, erstellen Sie einen neuen virtuellen Computer mit `New-AzureRmVM` und geben den Lizenzierungstyp wie folgt an:

```powershell
New-AzureRmVM -ResourceGroupName "myResourceGroup" -Location "West US" -VM $vm -LicenseType "Windows_Server"
```

Eine [ausführliche Anleitung zum Bereitstellen eines virtuellen Computers in Azure mit PowerShell](virtual-machines-windows-hybrid-use-benefit-licensing.md#detailed-powershell-walkthrough) finden Sie weiter unten. Alternativ können Sie eine ausführliche Anleitung zu den verschiedenen Schritten beim [Erstellen eines virtuellen Windows-Computers mit Resource Manager und PowerShell](virtual-machines-windows-ps-create.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) lesen.

## <a name="deploy-a-vm-via-resource-manager"></a>Bereitstellen eines virtuellen Computers mit Resource Manager
In den Resource Manager-Vorlagen kann ein zusätzlicher Parameter für `licenseType` angegeben werden. Informieren Sie sich weiter über das [Erstellen von Azure Resource Manager-Vorlagen](../resource-group-authoring-templates.md). Nachdem Sie die virtuelle Festplatte in Azure hochgeladen haben, bearbeiten Sie die Resource Manager-Vorlage, um den Lizenztyp als Teil des Computeanbieters einzuschließen, und stellen die Vorlage als normale Vorlage bereit:

```json
"properties": {  
   "licenseType": "Windows_Server",
   "hardwareProfile": {
        "vmSize": "[variables('vmSize')]"
   },
```

## <a name="verify-your-vm-is-utilizing-the-licensing-benefit"></a>Überprüfen, ob für den virtuellen Computer der Lizenzierungsvorteil genutzt wird
Sobald Sie den virtuellen Computer über die PowerShell- oder Resource Manager-Bereitstellungsmethode bereitgestellt haben, überprüfen Sie die den Lizenztyp wie folgt mit `Get-AzureRmVM` :

```powershell
Get-AzureRmVM -ResourceGroup "myResourceGroup" -Name "myVM"
```

Die Ausgabe sieht in etwa wie folgt aus:

```powershell
Type                     : Microsoft.Compute/virtualMachines
Location                 : westus
LicenseType              : Windows_Server
```

Dies steht im Gegensatz zu den folgenden virtuellen Computern, die ohne Lizenzierung für den Azure-Vorteil bei Hybridnutzung bereitgestellt wurden, z.B. ein virtueller Computer, der direkt aus dem Azure-Katalog bereitgestellt wurde:

```powershell
Type                     : Microsoft.Compute/virtualMachines
Location                 : westus
LicenseType              :
```

## <a name="detailed-powershell-walkthrough"></a>Ausführliche exemplarische Vorgehensweise zur PowerShell
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



<!--HONumber=Nov16_HO3-->


