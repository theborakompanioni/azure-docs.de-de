---
title: Konfigurieren von mehreren Netzwerkkarten auf einem virtuellen Windows-Computer | Microsoft Docs
description: Erfahren Sie, wie Sie über Azure PowerShell oder mithilfe von Resource Manager-Vorlagen einen virtuellen Computer mit mehreren angefügten Netzwerkkarten erstellen.
services: virtual-machines-windows
documentationcenter: ''
author: iainfoulds
manager: timlt
editor: ''

ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 08/04/2016
ms.author: iainfou

---
# Erstellen eines virtuellen Computers mit mehreren Netzwerkkarten
Sie können einen virtuellen Computer in Azure erstellen, an den mehrere Netzwerkkarten angefügt werden. Häufige Szenarien hierfür sind z.B. unterschiedliche Subnetze für Front-End- und Back-End-Verbindung oder ein Netzwerk für eine Überwachungs- oder Sicherungslösung. Dieser Artikel bietet Informationen zu Schnellbefehlen zum Erstellen eines virtuellen Computers, an den mehrere Netzwerkkarten angefügt werden. Ausführliche Informationen hierzu sowie zum Erstellen von mehreren Netzwerkkarten in Ihren eigenen PowerShell-Skripts finden Sie unter [Bereitstellen von Multi-NIC-VMs](../virtual-network/virtual-network-deploy-multinic-arm-ps.md). Verschiedene [VM-Größen](virtual-machines-windows-sizes.md) unterstützen eine unterschiedliche Anzahl von Netzwerkkarten, passen Sie die Größe Ihres virtuellen Computers daher entsprechend an.

> [!WARNING]
> Das Anfügen der Netzwerkkarten muss während der Erstellung des virtuellen Computers erfolgen – Sie können keine Netzwerkkarten an einen vorhandenen virtuellen Computer anfügen. Sie können [einen neuen virtuellen Computer basierend auf dem/den ursprünglichen virtuellen Datenträger(n) erstellen](virtual-machines-windows-specialized-image.md) und beim Bereitstellen des virtuellen Computers mehrere Netzwerkkarten erstellen.
> 
> 

## Erstellen von Kernressourcen
Stellen Sie sicher, dass Sie die [neueste Azure PowerShell-Version installiert und konfiguriert](../powershell-install-configure.md) haben.

Erstellen Sie zunächst eine Ressourcengruppe:

```powershell
New-AzureRmResourceGroup -Name TestRG -Location WestUS
```

Erstellen Sie ein Speicherkonto für Ihre virtuellen Computer:

```powershell
$storageAcc = New-AzureRmStorageAccount -Name teststorage `
    -ResourceGroupName TestRG -Kind Storage -SkuName Premium_LRS -Location WestUS
```

## Erstellen des virtuellen Netzwerks und der Subnetze
Definieren Sie zwei virtuelle Subnetze – eins für den Front-End-Datenverkehr, eins für den Back-End-Datenverkehr. Erstellen Sie Ihr virtuelles Netzwerk mit diesen Subnetzen:

```powershell
$frontEndSubnet = New-AzureRmVirtualNetworkSubnetConfig -Name "FrontEnd" `
    -AddressPrefix 192.168.1.0/24
$backEndSubnet = New-AzureRmVirtualNetworkSubnetConfig -Name "BackEnd" `
    -AddressPrefix 192.168.2.0/24


$vnet = New-AzureRmVirtualNetwork -ResourceGroupName TestRG -Name TestVNet `
    -AddressPrefix 192.168.0.0/16 -Location WestUS `
    -Subnet $frontEndSubnet,$backEndSubnet
```


## Erstellen von mehreren Netzwerkkarten
Erstellen Sie zwei Netzwerkkarten, und fügen Sie eine an das Front-End-Subnetz und die andere an das Back-End-Subnetz an:

```powershell
$frontEnd = $vnet.Subnets|?{$_.Name -eq 'FrontEnd'}
$NIC1 = New-AzureRmNetworkInterface -Name NIC1 -ResourceGroupName TestRG `
        -Location WestUS -SubnetId $FrontEnd.Id

$backEnd = $vnet.Subnets|?{$_.Name -eq 'BackEnd'}
$NIC2 = New-AzureRmNetworkInterface -Name NIC2 -ResourceGroupName TestRG `
        -Location WestUS -SubnetId $BackEnd.Id
```

Üblicherweise erstellen Sie auch eine [Netzwerksicherheitsgruppe](../virtual-network/virtual-networks-nsg.md) oder einen [Lastenausgleich](../load-balancer/load-balancer-overview.md), um den Datenverkehr zwischen den virtuellen Computern zu verwalten und zu verteilen. Im [detaillierteren Artikel zu virtuellen Computern mit mehreren Netzwerkkarten](../virtual-network/virtual-network-deploy-multinic-arm-ps.md) finden Sie ausführlichere Informationen zum Erstellen einer Netzwerksicherheitsgruppe und zum Zuweisen von Netzwerkkarten.

## Erstellen des virtuellen Computers
Beginnen Sie jetzt damit, Ihre VM-Konfiguration zu erstellen. Jede VM-Größe weist eine maximale Anzahl von Netzwerkkarten auf, die Sie einem virtuellen Computer hinzufügen können. Weitere Informationen finden Sie unter [Windows-VM-Größen](virtual-machines-windows-sizes.md). Das folgende Beispiel verwendet eine VM-Größe, die bis zu zwei Netzwerkkarten unterstützt (`Standard_DS2_v2`):

```powershell
$cred = Get-Credential

$vmConfig = New-AzureRmVMConfig -VMName TestVM -VMSize "Standard_DS2_v2"

$vmConfig = Set-AzureRmVMOperatingSystem -VM $vmConfig -Windows -ComputerName TestVM `
    -Credential $cred -ProvisionVMAgent -EnableAutoUpdate
$vmConfig = Set-AzureRmVMSourceImage -VM $vmConfig -PublisherName MicrosoftWindowsServer `
    -Offer WindowsServer -Skus 2012-R2-Datacenter -Version "latest"
```

Fügen Sie die beiden Netzwerkkarten an, die Sie zuvor erstellt haben:

```powershell
$vmConfig = Add-AzureRmVMNetworkInterface -VM $vmConfig -Id $NIC1.Id -Primary
$vmConfig = Add-AzureRmVMNetworkInterface -VM $vmConfig -Id $NIC2.Id
```

Konfigurieren Sie den Speicher und den virtuellen Datenträger für Ihren neuen virtuellen Computer:

```powershell
$blobPath = "vhds/WindowsVMosDisk.vhd"
$osDiskUri = $storageAcc.PrimaryEndpoints.Blob.ToString() + $blobPath
$diskName = "windowsvmosdisk"
$vmConfig = Set-AzureRmVMOSDisk -VM $vmConfig -Name $diskName -VhdUri $osDiskUri `
    -CreateOption fromImage
```

Zum Schluss erstellen Sie einen virtuellen Computer:

```powershell
New-AzureRmVM -VM $vmConfig -ResourceGroupName TestRG -Location WestUS
```

## Erstellen von mehreren Netzwerkkarten mithilfe von Resource Manager-Vorlagen
Azure Resource Manager-Vorlagen verwenden deklarative JSON-Dateien zum Definieren Ihrer Umgebung. Lesen Sie eine [Übersicht über Azure Resource Manager](../resource-group-overview.md). Resource Manager-Vorlagen bieten eine Möglichkeit, während der Bereitstellung mehrere Instanzen einer Ressource zu erstellen – z.B. mehrere Netzwerkkarten. Mit *copy* geben Sie die Anzahl der zu erstellenden Instanzen an:

```bash
"copy": {
    "name": "multiplenics"
    "count": "[parameters('count')]"
}
```

Informieren Sie sich über das [Erstellen mehrerer Instanzen mithilfe von *copy*](../resource-group-create-multiple.md).

Sie können auch `copyIndex()` verwenden und eine Zahl an einen Ressourcennamen anfügen, sodass Sie `NIC1`, `NIC2` usw. erstellen können. Das folgende Beispiel veranschaulicht das Anfügen des Indexwerts:

```bash
"name": "[concat('NIC-', copyIndex())]", 
```

Ein vollständiges Beispiel finden Sie unter [Erstellen von mehreren Netzwerkkarten mithilfe von Resource Manager-Vorlagen](../virtual-network/virtual-network-deploy-multinic-arm-template.md).

## Nächste Schritte
Überprüfen Sie die [Windows-VM-Größen](virtual-machines-windows-sizes.md), wenn Sie einen virtuellen Computer mit mehreren Netzwerkkarten erstellen. Achten Sie auf die maximale Anzahl von Netzwerkkarten, die von jeder VM-Größe unterstützt wird.

Denken Sie daran, dass Sie einem vorhandenen virtuellen Computer keine weiteren Netzwerkkarten hinzufügen können. Sie müssen alle Netzwerkkarten während der Bereitstellung des virtuellen Computers erstellen. Planen Sie Ihre Bereitstellungen sorgfältig, um sicherzustellen, dass Sie die erforderliche Netzwerkkonnektivität von Anfang an einberechnen.

<!---HONumber=AcomDC_0817_2016-->