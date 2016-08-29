<properties
   pageTitle="Azure-Vorteil bei Hybridnutzung für Windows Server | Microsoft Azure"
   description="Erfahren Sie, wie Sie die Vorteile der Windows Server Software Assurance optimal nutzen, um lokale Lizenzen in Azure zu verwenden."
   services="virtual-machines-windows"
   documentationCenter=""
   authors="iainfoulds"
   manager="timlt"
   editor=""/>

<tags
   ms.service="virtual-machines-windows"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-windows"
   ms.workload="infrastructure-services"
   ms.date="07/13/2016"
   ms.author="georgem"/>

# Azure-Vorteil bei Hybridnutzung für Windows Server

Sie können für Kunden, die Windows Server mit Software Assurance verwenden, lokale Windows Server-Lizenzen in Azure übertragen und virtuelle Computer mit Windows Server in Azure zu geringeren Kosten ausführen. Durch den Azure-Vorteil bei Hybridnutzung können Sie virtuelle Windows Server-Computer in Azure ausführen und müssen nur die Grundgebühr für die Computekapazität bezahlen. Weitere Informationen finden Sie auf der [Lizenzierungsseite für den Azure-Vorteil bei Hybridnutzung](https://azure.microsoft.com/pricing/hybrid-use-benefit/). In diesem Artikel wird die Bereitstellung von virtuellen Windows Server-Computern in Azure erläutert, durch die dieser Lizenzierungsvorteil genutzt werden kann.

> [AZURE.NOTE] Sie können keine Azure Marketplace-Images verwenden, um virtuelle Windows Server-Computer bereitzustellen, und dabei den Azure-Vorteil bei Hybridnutzung nutzen. Sie müssen Ihre virtuellen Computer mit PowerShell- oder Resource Manager-Vorlagen bereitstellen, um die virtuellen Computer ordnungsgemäß als für die rabattierte Grundgebühr für Computekapazität berechtigt zu registrieren.

## Voraussetzungen
Es gibt einige Voraussetzungen, um den Azure-Vorteil bei Hybridnutzung für virtuelle Windows Server-Computer in Azure nutzen zu können:

- Azure PowerShell-Modul ist installiert.
- Windows Server-VHD wurde in Azure Storage hochgeladen.

### Installieren von Azure PowerShell
Unter [Installieren und Konfigurieren von Azure PowerShell](../powershell-install-configure.md) finden Sie Informationen dazu, wie Sie die neueste Version von Azure PowerShell installieren, das gewünschte Abonnement auswählen und sich bei Ihrem Azure-Konto anmelden. Auch wenn Sie Ihre virtuellen Computer mit Resource Manager-Vorlagen bereitstellen möchten, muss Azure PowerShell installiert sein, um die Windows Server-VHD hochladen zu können (siehe den nächsten Schritt weiter unten).

### Hochladen einer Windows Server-VHD

Für die Bereitstellung eines virtuellen Windows Server-Computers in Azure müssen Sie zunächst eine virtuelle Festplatte erstellen, die den Windows Server-Basisbuild enthält. Diese virtuelle Festplatte muss entsprechend über Sysprep vorbereitet werden, bevor Sie sie in Azure hochladen. Informieren Sie sich über die [VHD-Anforderungen und den Sysprep-Prozess](./virtual-machines-windows-upload-image.md). Weitere Informationen finden Sie auch unter [Sysprep Support for Server Roles](https://msdn.microsoft.com/windows/hardware/commercialize/manufacture/desktop/sysprep-support-for-server-roles) (Sysprep-Unterstützung für Serverrollen). Nachdem Sie die virtuelle Festplatte vorbereitet haben, laden Sie sie mit dem `Add-AzureRmVhd`-Cmdlet wie folgt in Ihr Azure-Storage-Konto hoch:

```
Add-AzureRmVhd -ResourceGroupName MyResourceGroup -Destination "https://mystorageaccount.blob.core.windows.net/vhds/myvhd.vhd" -LocalFilePath 'C:\Path\To\myvhd.vhd'
```

> [AZURE.NOTE] Microsoft SQL Server, SharePoint Server und Dynamics können ebenfalls Ihre Software Assurance-Lizenzierung verwenden. Sie müssen weiterhin das Windows Server-Image vorbereiten, indem Sie die Anwendungskomponenten installieren, Lizenzschlüssel bereitstellen und anschließend das Datenträgerimage in Azure hochladen. Lesen Sie in der entsprechenden Dokumentation die Informationen zum Ausführen von Sysprep mit Ihrer Anwendung, etwa [Überlegungen zur Installation von SQL Server mit Sysprep](https://msdn.microsoft.com/library/ee210754.aspx) oder [Build a SharePoint Server 2016 Reference Image (Sysprep)](http://social.technet.microsoft.com/wiki/contents/articles/33789.build-a-sharepoint-server-2016-reference-image-sysprep.aspx) (Erstellen eines SharePoint Server 2016-Referenzimages [Sysprep]).

Informieren Sie sich auch über das [Hochladen der VHD in Azure](./virtual-machines-windows-upload-image.md#upload-the-vm-image-to-your-storage-account).

> [AZURE.TIP] Der vorliegende Artikel konzentriert sich auf die Bereitstellung von virtuellen Windows Server-Computern, Sie können auf diese Weise jedoch auch virtuelle Windows-Clientcomputer bereitstellen. Ersetzen Sie in den folgenden Beispielen entsprechend `Server` durch `Client`.

## Schnellstartanleitung für das Bereitstellen eines virtuellen Computers über PowerShell
Beim Bereitstellen des virtuellen Windows Server-Computers über PowerShell ist ein zusätzlicher Parameter für `-LicenseType` vorhanden. Nachdem Sie die virtuelle Festplatte in Azure hochgeladen haben, erstellen Sie einen neuen virtuellen Computer mit `New-AzureRmVM` und geben den Lizenzierungstyp wie folgt an:

```
New-AzureRmVM -ResourceGroupName MyResourceGroup -Location "West US" -VM $vm -LicenseType Windows_Server
```

Eine [ausführliche Anleitung zum Bereitstellen eines virtuellen Computers in Azure mit PowerShell](./virtual-machines-windows-hybrid-use-benefit-licensing.md#deploy-windows-server-vm-via-powershell-detailed-walkthrough) finden Sie weiter unten. Alternativ können Sie eine ausführliche Anleitung zu den verschiedenen Schritten beim [Erstellen eines virtuellen Windows-Computers mit Resource Manager und PowerShell](./virtual-machines-windows-ps-create.md) lesen.

## Bereitstellen eines virtuellen Computers mit Resource Manager
In den Resource Manager-Vorlagen kann ein zusätzlicher Parameter für `licenseType` angegeben werden. Informieren Sie sich weiter über das [Erstellen von Azure Resource Manager-Vorlagen](../resource-group-authoring-templates.md). Nachdem Sie die virtuelle Festplatte in Azure hochgeladen haben, bearbeiten Sie die Resource Manager-Vorlage, um den Lizenztyp als Teil des Computeanbieters einzuschließen, und stellen die Vorlage als normale Vorlage bereit:

```
"properties": {  
   "licenseType": "Windows_Server",
   "hardwareProfile": {
        "vmSize": "[variables('vmSize')]"
   },
```
 
## Überprüfen, ob für den virtuellen Computer der Lizenzierungsvorteil genutzt wird
Sobald Sie den virtuellen Computer über die PowerShell- oder Resource Manager-Bereitstellungsmethode bereitgestellt haben, überprüfen Sie die den Lizenztyp wie folgt mit `Get-AzureRmVM`:
 
```
Get-AzureRmVM -ResourceGroup MyResourceGroup -Name MyVM
```

Eine Ausgabe ähnlich der folgenden wird angezeigt.

```
Type                     : Microsoft.Compute/virtualMachines
Location                 : westus
LicenseType              : Windows_Server
```

Dies steht im Gegensatz zu den folgenden virtuellen Computern, die ohne Lizenzierung für den Azure-Vorteil bei Hybridnutzung bereitgestellt wurden, z.B. ein virtueller Computer, der direkt aus dem Azure-Katalog bereitgestellt wurde:

```
Type                     : Microsoft.Compute/virtualMachines
Location                 : westus
LicenseType              : 
```
 
## Ausführliche exemplarische Vorgehensweise zur PowerShell

Die folgenden ausführlichen Schritte für PowerShell zeigen eine vollständige Bereitstellung eines virtuellen Computers. Weitere Informationen zu den Cmdlets und den verschiedenen erstellten Komponenten finden Sie unter [Erstellen einer Windows-VM mit dem Resource Manager und PowerShell](./virtual-machines-windows-ps-create.md). Dabei wird das Erstellen der Ressourcengruppe, des Speicherkontos und des virtuellen Netzwerks beschrieben, anschließend wird der virtuelle Computer definiert und schließlich erstellt.
 
Rufen Sie zunächst sicher Anmeldeinformationen ab, und legen Sie einen Standort und einen Namen für die Ressourcengruppe fest:

```
$cred = Get-Credential
$location = "West US"
$resourceGroupName = "TestLicensing"
```

Erstellen Sie eine öffentliche IP-Adresse:

```
$publicIPName = "testlicensingpublicip"
$publicIP = New-AzureRmPublicIpAddress -Name $publicIPName -ResourceGroupName $resourceGroupName -Location $location -AllocationMethod Dynamic
```

Definieren Sie Subnetz, NIC und VNET:

```
$subnetName = "testlicensingsubnet"
$nicName = "testlicensingnic"
$vnetName = "testlicensingvnet"
$subnetconfig = New-AzureRmVirtualNetworkSubnetConfig -Name $subnetName -AddressPrefix 10.0.0.0/8
$vnet = New-AzureRmVirtualNetwork -Name $vnetName -ResourceGroupName $resourceGroupName -Location $location -AddressPrefix 10.0.0.0/8 -Subnet $subnetconfig
$nic = New-AzureRmNetworkInterface -Name $nicName -ResourceGroupName $resourceGroupName -Location $location -SubnetId $vnet.Subnets[0].Id -PublicIpAddressId $publicIP.Id
```

Benennen Sie den virtuellen Computer, und erstellen Sie eine VM-Konfiguration:

```
$vmName = "testlicensing"
$vmConfig = New-AzureRmVMConfig -VMName $vmName -VMSize "Standard_A1"
```

Definieren Sie das Betriebssystem:

```
$computerName = "testlicensing"
$vm = Set-AzureRmVMOperatingSystem -VM $vmConfig -Windows -ComputerName $computerName -Credential $cred -ProvisionVMAgent -EnableAutoUpdate
```

Fügen Sie die Netzwerkschnittstelle dem virtuellen Computer hinzu.

```
$vm = Add-AzureRmVMNetworkInterface -VM $vm -Id $nic.Id
```

Definieren Sie das zu verwendende Speicherkonto:

```
$storageAcc = Get-AzureRmStorageAccount -ResourceGroupName $resourceGroupName -AccountName testlicensing
```

Laden Sie die VHD entsprechend vorbereitet hoch, und fügen Sie sie zur Verwendung an den virtuellen Computer an:

```
$osDiskName = "licensing.vhd"
$osDiskUri = '{0}vhds/{1}{2}.vhd' -f $storageAcc.PrimaryEndpoints.Blob.ToString(), $vmName.ToLower(), $osDiskName
$urlOfUploadedImageVhd = "https://testlicensing.blob.core.windows.net/vhd/licensing.vhd"
$vm = Set-AzureRmVMOSDisk -VM $vm -Name $osDiskName -VhdUri $osDiskUri -CreateOption FromImage -SourceImageUri $urlOfUploadedImageVhd -Windows
```

Erstellen Sie schließlich den virtuellen Computer, und definieren Sie den Lizenzierungstyp so, dass der Azure-Vorteil bei Hybridnutzung verwendet wird:

```
New-AzureRmVM -ResourceGroupName $resourceGroupName -Location $location -VM $vm -LicenseType Windows_Server
```

## Nächste Schritte

Erfahren Sie mehr zur [Lizenzierung für den Azure-Vorteil bei Hybridnutzung](https://azure.microsoft.com/pricing/hybrid-use-benefit/).

Erfahren Sie mehr zum [Verwenden von Resource Manager-Vorlagen](../resource-group-overview.md).

<!---HONumber=AcomDC_0817_2016-->