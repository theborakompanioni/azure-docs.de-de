---
title: Erstellen eines virtuellen Windows-Computers mit mehreren Netzwerkkarten | Microsoft Docs
description: "Erfahren Sie, wie Sie über Azure PowerShell oder mithilfe von Resource Manager-Vorlagen einen virtuellen Windows-Computer mit mehreren angefügten Netzwerkkarten erstellen."
services: virtual-machines-windows
documentationcenter: 
author: iainfoulds
manager: timlt
editor: 
ms.assetid: 9bff5b6d-79ac-476b-a68f-6f8754768413
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 10/27/2016
ms.author: iainfou
translationtype: Human Translation
ms.sourcegitcommit: 7167048a287bee7c26cfc08775dcb84f9e7c2eed
ms.openlocfilehash: 46156a3331585b47761432c13462dffeb0b7eeb5


---
# <a name="creating-a-windows-vm-with-multiple-nics"></a>Erstellen eines virtuellen Windows-Computers mit mehreren Netzwerkkarten
Sie können einen virtuellen Computer in Azure erstellen, an den mehrere Netzwerkkarten angefügt werden. Häufige Szenarien hierfür sind z.B. unterschiedliche Subnetze für Front-End- und Back-End-Verbindung oder ein Netzwerk für eine Überwachungs- oder Sicherungslösung. Dieser Artikel bietet Informationen zu Schnellbefehlen zum Erstellen eines virtuellen Computers, an den mehrere Netzwerkkarten angefügt werden. Ausführliche Informationen hierzu sowie zum Erstellen von mehreren Netzwerkkarten in Ihren eigenen PowerShell-Skripts finden Sie unter [Bereitstellen von Multi-NIC-VMs](../virtual-network/virtual-network-deploy-multinic-arm-ps.md). Verschiedene [VM-Größen](virtual-machines-windows-sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) unterstützen eine unterschiedliche Anzahl von Netzwerkkarten. Passen Sie die Größe Ihres virtuellen Computers daher entsprechend an.

> [!WARNING]
> Das Anfügen der Netzwerkkarten muss während der Erstellung des virtuellen Computers erfolgen – Sie können keine Netzwerkkarten an einen vorhandenen virtuellen Computer anfügen. Sie können [einen virtuellen Computer basierend auf dem bzw. den ursprünglichen virtuellen Datenträgern erstellen](virtual-machines-windows-vhd-copy.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) und beim Bereitstellen des virtuellen Computers mehrere Netzwerkkarten erstellen.
> 
> 

## <a name="create-core-resources"></a>Erstellen von Kernressourcen
Stellen Sie sicher, dass Sie die [neueste Azure PowerShell-Version installiert und konfiguriert](/powershell/azureps-cmdlets-docs)haben. Melden Sie sich an Ihrem Azure-Konto an:

```powershell
Login-AzureRmAccount
```

Ersetzen Sie in den folgenden Beispielen die Beispielparameternamen durch Ihre eigenen Werte. Zu den Beispielparameternamen zählen `myResourceGroup`, `mystorageaccount` und `myVM`.

Erstellen Sie zunächst eine Ressourcengruppe. Das folgende Beispiel erstellt eine Ressourcengruppe mit dem Namen `myResourceGroup` am Standort `WestUs`:

```powershell
New-AzureRmResourceGroup -Name "myResourceGroup" -Location "WestUS"
```

Erstellen Sie ein Speicherkonto für Ihre virtuellen Computer: Das folgende Beispiel erstellt ein Speicherkonto namens `mystorageaccount`:

```powershell
$storageAcc = New-AzureRmStorageAccount -ResourceGroupName "myResourceGroup" `
    -Location "WestUS" -Name "mystorageaccount" `
    -Kind "Storage" -SkuName "Premium_LRS" 
```

## <a name="create-virtual-network-and-subnets"></a>Erstellen des virtuellen Netzwerks und der Subnetze
Definieren Sie zwei virtuelle Subnetze – eins für den Front-End-Datenverkehr, eins für den Back-End-Datenverkehr. Im folgenden Beispiel werden zwei Regeln (`mySubnetFrontEnd` und `mySubnetBackEnd`) definiert:

```powershell
$mySubnetFrontEnd = New-AzureRmVirtualNetworkSubnetConfig -Name "mySubnetFrontEnd" `
    -AddressPrefix "192.168.1.0/24"
$mySubnetBackEnd = New-AzureRmVirtualNetworkSubnetConfig -Name "mySubnetBackEnd" `
    -AddressPrefix "192.168.2.0/24"
```

Erstellen Sie das virtuelle Netzwerk und die Subnetze. Im folgenden Beispiel wird ein virtuelles Netzwerk namens `myVnet` erstellt:

```powershell
$myVnet = New-AzureRmVirtualNetwork -ResourceGroupName "myResourceGroup" `
    -Location "WestUS" -Name "myVnet" -AddressPrefix "192.168.0.0/16" `
    -Subnet $mySubnetFrontEnd,$mySubnetBackEnd
```


## <a name="create-multiple-nics"></a>Erstellen von mehreren Netzwerkkarten
Erstellen Sie zwei Netzwerkkarten, und fügen Sie eine an das Front-End-Subnetz und die andere an das Back-End-Subnetz an. Im folgenden Beispiel werden zwei Netzwerkkarten (`myNic1` und `myNic2`) erstellt:

```powershell
$frontEnd = $myVnet.Subnets|?{$_.Name -eq 'mySubnetFrontEnd'}
$myNic1 = New-AzureRmNetworkInterface -ResourceGroupName "myResourceGroup" `
    -Location "WestUS" -Name "myNic1" -SubnetId $frontEnd.Id

$backEnd = $myVnet.Subnets|?{$_.Name -eq 'mySubnetBackEnd'}
$myNic2 = New-AzureRmNetworkInterface -ResourceGroupName "myResourceGroup" `
    -Location "WestUS" -Name "myNic2" -SubnetId $backEnd.Id
```

Üblicherweise erstellen Sie auch eine [Netzwerksicherheitsgruppe](../virtual-network/virtual-networks-nsg.md) oder einen [Lastenausgleich](../load-balancer/load-balancer-overview.md), um den Datenverkehr zwischen den virtuellen Computern zu verwalten und zu verteilen. Im [detaillierteren Artikel zu virtuellen Computern mit mehreren Netzwerkkarten](../virtual-network/virtual-network-deploy-multinic-arm-ps.md) finden Sie ausführlichere Informationen zum Erstellen einer Netzwerksicherheitsgruppe und zum Zuweisen von Netzwerkkarten.

## <a name="create-the-virtual-machine"></a>Erstellen des virtuellen Computers
Beginnen Sie jetzt damit, Ihre VM-Konfiguration zu erstellen. Jede VM-Größe weist eine maximale Anzahl von Netzwerkkarten auf, die Sie einem virtuellen Computer hinzufügen können. Weitere Informationen finden Sie unter [Windows-VM-Größen](virtual-machines-windows-sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). 

Legen Sie zuerst Ihre VM-Anmeldeinformationen wie folgt auf die Variable `$cred` fest:

```powershell
$cred = Get-Credential
```

Im folgenden Beispiel wird eine VM mit dem Namen `myVM` definiert und eine VM-Größe verwendet, die bis zu zwei Netzwerkkarten unterstützt (`Standard_DS2_v2`):

```powershell
$vmConfig = New-AzureRmVMConfig -VMName "myVM" -VMSize "Standard_DS2_v2"
```

Führen Sie die restliche VM-Konfiguration durch. Im folgenden Beispiel wird eine Windows Server 2012 R2-VM erstellt:

```powershell
$vmConfig = Set-AzureRmVMOperatingSystem -VM $vmConfig -Windows -ComputerName "myVM" `
    -Credential $cred -ProvisionVMAgent -EnableAutoUpdate
$vmConfig = Set-AzureRmVMSourceImage -VM $vmConfig -PublisherName "MicrosoftWindowsServer" `
    -Offer "WindowsServer" -Skus "2012-R2-Datacenter" -Version "latest"
```

Fügen Sie die beiden Netzwerkkarten an, die Sie zuvor erstellt haben:

```powershell
$vmConfig = Add-AzureRmVMNetworkInterface -VM $vmConfig -Id $myNic1.Id -Primary
$vmConfig = Add-AzureRmVMNetworkInterface -VM $vmConfig -Id $myNic2.Id
```

Konfigurieren Sie den Speicher und den virtuellen Datenträger für Ihren neuen virtuellen Computer:

```powershell
$blobPath = "vhds/WindowsVMosDisk.vhd"
$osDiskUri = $storageAcc.PrimaryEndpoints.Blob.ToString() + $blobPath
$diskName = "windowsvmosdisk"
$vmConfig = Set-AzureRmVMOSDisk -VM $vmConfig -Name $diskName -VhdUri $osDiskUri `
    -CreateOption "fromImage"
```

Zum Schluss erstellen Sie einen virtuellen Computer:

```powershell
New-AzureRmVM -VM $vmConfig -ResourceGroupName "myResourceGroup" -Location "WestUS"
```

## <a name="creating-multiple-nics-using-resource-manager-templates"></a>Erstellen von mehreren Netzwerkkarten mithilfe von Resource Manager-Vorlagen
Azure Resource Manager-Vorlagen verwenden deklarative JSON-Dateien zum Definieren Ihrer Umgebung. Lesen Sie eine [Übersicht über Azure Resource Manager](../azure-resource-manager/resource-group-overview.md). Resource Manager-Vorlagen bieten eine Möglichkeit, während der Bereitstellung mehrere Instanzen einer Ressource zu erstellen – z.B. mehrere Netzwerkkarten. Mit *copy* geben Sie die Anzahl der zu erstellenden Instanzen an:

```json
"copy": {
    "name": "multiplenics"
    "count": "[parameters('count')]"
}
```

Informieren Sie sich über das [Erstellen mehrerer Instanzen mithilfe von *copy*](../azure-resource-manager/resource-group-create-multiple.md). 

Sie können auch `copyIndex()` verwenden und eine Zahl an einen Ressourcennamen anfügen, sodass Sie `myNic1`, `MyNic2` usw. erstellen können. Das folgende Beispiel veranschaulicht das Anfügen des Indexwerts:

```json
"name": "[concat('myNic', copyIndex())]", 
```

Ein vollständiges Beispiel finden Sie unter [Erstellen von mehreren Netzwerkkarten mithilfe von Resource Manager-Vorlagen](../virtual-network/virtual-network-deploy-multinic-arm-template.md).

## <a name="next-steps"></a>Nächste Schritte
Überprüfen Sie die [Windows-VM-Größen](virtual-machines-windows-sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) , wenn Sie einen virtuellen Computer mit mehreren Netzwerkkarten erstellen. Achten Sie auf die maximale Anzahl von Netzwerkkarten, die von jeder VM-Größe unterstützt wird. 

Denken Sie daran, dass Sie einem vorhandenen virtuellen Computer keine weiteren Netzwerkkarten hinzufügen können. Sie müssen alle Netzwerkkarten während der Bereitstellung des virtuellen Computers erstellen. Planen Sie Ihre Bereitstellungen sorgfältig, um sicherzustellen, dass Sie die erforderliche Netzwerkkonnektivität von Anfang an einberechnen.




<!--HONumber=Feb17_HO3-->


