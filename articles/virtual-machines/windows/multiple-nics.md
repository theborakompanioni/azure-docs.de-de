---
title: Erstellen und Verwalten virtueller Windows-Computer in Azure mit mehreren Netzwerkkarten | Microsoft-Dokumentation
description: "Erfahren Sie, wie Sie über Azure PowerShell oder mithilfe von Resource Manager-Vorlagen einen virtuellen Windows-Computer mit mehreren angefügten Netzwerkkarten erstellen und verwalten."
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
ms.date: 07/05/2017
ms.author: iainfou
ms.translationtype: Human Translation
ms.sourcegitcommit: bb794ba3b78881c967f0bb8687b1f70e5dd69c71
ms.openlocfilehash: 92f5181dbf36ef0f7e2568d557faa7c5f2144ad9
ms.contentlocale: de-de
ms.lasthandoff: 07/06/2017


---
# <a name="create-and-manage-a-windows-virtual-machine-that-has-multiple-nics"></a>Erstellen und Verwalten eines virtuellen Windows-Computers mit mehrere Netzwerkkarten
Virtuelle Computer (VMs) in Azure können über mehrere virtuelle Netzwerkkarten (Network Interface Cards, NICs) verfügen. Häufige Szenarien hierfür sind z.B. unterschiedliche Subnetze für Front-End- und Back-End-Verbindung oder ein Netzwerk für eine Überwachungs- oder Sicherungslösung. In diesem Artikel erfahren Sie, wie Sie einen virtuellen Computer mit mehreren Netzwerkkarten erstellen. Außerdem erfahren Sie, wie Sie Netzwerkkarten zu einem vorhandenen virtuellen Computer hinzufügen oder davon entfernen. Verschiedene [VM-Größen](sizes.md) unterstützen eine unterschiedliche Anzahl von Netzwerkkarten, passen Sie die Größe Ihres virtuellen Computers daher entsprechend an.

Ausführliche Informationen wie u.a. das Erstellen mehrerer Netzwerkkarten in Ihren eigenen PowerShell-Skripts finden Sie unter [Bereitstellen virtueller Computer mit mehreren Netzwerkkarten](../../virtual-network/virtual-network-deploy-multinic-arm-ps.md).

## <a name="prerequisites"></a>Voraussetzungen
Stellen Sie sicher, dass Sie die [neueste Azure PowerShell-Version installiert und konfiguriert](/powershell/azure/overview) haben.

Ersetzen Sie in den folgenden Beispielen die Beispielparameternamen durch Ihre eigenen Werte. Beispielparameternamen sind u.a. *myResourceGroup*, *myVnet* und *myVM*.


## <a name="create-a-vm-with-multiple-nics"></a>Erstellen eines virtuellen Computers mit mehreren Netzwerkschnittstellenkarten (NICs)
Erstellen Sie zunächst eine Ressourcengruppe. Das folgende Beispiel erstellt eine Ressourcengruppe mit dem Namen *myResourceGroup* am Standort *EastUs*:

```powershell
New-AzureRmResourceGroup -Name "myResourceGroup" -Location "EastUS"
```

### <a name="create-virtual-network-and-subnets"></a>Erstellen des virtuellen Netzwerks und der Subnetze
Ein häufiges Szenario ist ein virtuelles Netzwerk mit zwei oder mehr Subnetzen. Ein Subnetz kann für den Front-End-Datenverkehr, das andere für den Back-End-Datenverkehr verwendet werden. Sie verwenden mehrere Netzwerkkarten auf Ihrem virtuellen Computer, um eine Verbindung mit beiden Subnetzen herzustellen.

1. Definieren Sie mit [New-AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/new-azurermvirtualnetworksubnetconfig) zwei virtueller Netzwerksubnetze. Das folgende Beispiel definiert die Subnetze für *mySubnetFrontEnd* und *mySubnetBackEnd*:

    ```powershell
    $mySubnetFrontEnd = New-AzureRmVirtualNetworkSubnetConfig -Name "mySubnetFrontEnd" `
        -AddressPrefix "192.168.1.0/24"
    $mySubnetBackEnd = New-AzureRmVirtualNetworkSubnetConfig -Name "mySubnetBackEnd" `
        -AddressPrefix "192.168.2.0/24"
    ```

2. Erstellen Sie mit [New-AzureRmVirtualNetwork](/powershell/module/azurerm.network/new-azurermvirtualnetwork) Ihr virtuelles Netzwerk und die Subnetze. Im folgenden Beispiel wird ein virtuelles Netzwerk namens *myVnet* erstellt:

    ```powershell
    $myVnet = New-AzureRmVirtualNetwork -ResourceGroupName "myResourceGroup" `
        -Location "EastUs" `
        -Name "myVnet" `
        -AddressPrefix "192.168.0.0/16" `
        -Subnet $mySubnetFrontEnd,$mySubnetBackEnd
    ```


### <a name="create-multiple-nics"></a>Erstellen von mehreren Netzwerkkarten
Erstellen Sie mit [New-AzureRmNetworkInterface](/powershell/module/azurerm.network/new-azurermnetworkinterface) zwei Netzwerkkarten. Fügen Sie eine Netzwerkkarte an das Front-End-Subnetz und die andere an das Back-End-Subnetz an. Im folgenden Beispiel werden zwei Netzwerkkarten namens *myNic1* und *myNic2* erstellt:

```powershell
$frontEnd = $myVnet.Subnets|?{$_.Name -eq 'mySubnetFrontEnd'}
$myNic1 = New-AzureRmNetworkInterface -ResourceGroupName "myResourceGroup" `
    -Name "myNic1" `
    -Location "EastUs" `
    -SubnetId $frontEnd.Id

$backEnd = $myVnet.Subnets|?{$_.Name -eq 'mySubnetBackEnd'}
$myNic2 = New-AzureRmNetworkInterface -ResourceGroupName "myResourceGroup" `
    -Name "myNic2" `
    -Location "EastUs" `
    -SubnetId $backEnd.Id
```

Üblicherweise erstellen Sie auch eine [Netzwerksicherheitsgruppe](../../virtual-network/virtual-networks-nsg.md) oder einen [Lastenausgleich](../../load-balancer/load-balancer-overview.md), um den Datenverkehr zwischen den virtuellen Computern zu verwalten und zu verteilen. Im [detaillierteren Artikel zu virtuellen Computern mit mehreren Netzwerkkarten](../../virtual-network/virtual-network-deploy-multinic-arm-ps.md) finden Sie ausführlichere Informationen zum Erstellen einer Netzwerksicherheitsgruppe und zum Zuweisen von Netzwerkkarten.

### <a name="create-the-virtual-machine"></a>Erstellen des virtuellen Computers
Beginnen Sie jetzt damit, Ihre VM-Konfiguration zu erstellen. Jede VM-Größe weist eine maximale Anzahl von Netzwerkkarten auf, die Sie einem virtuellen Computer hinzufügen können. Weitere Informationen finden Sie unter [Windows-VM-Größen](sizes.md) .

1. Legen Sie Ihre VM-Anmeldeinformationen wie folgt auf die Variable `$cred` fest:

    ```powershell
    $cred = Get-Credential
    ```

2. Definieren Sie Ihren virtuellen Computer mit [New-AzureRmVMConfig](/powershell/module/azurerm.compute/new-azurermvmconfig). Im folgenden Beispiel wird ein virtueller Computer mit dem Namen *myVM* definiert und eine VM-Größe verwendet, die mehr als zwei Netzwerkkarten unterstützt (*Standard_DS3_v2*):

    ```powershell
    $vmConfig = New-AzureRmVMConfig -VMName "myVM" -VMSize "Standard_DS3_v2"
    ```

3. Erstellen Sie die restliche Konfiguration des virtuellen Computers mit [Set-AzureRmVMOperatingSystem](/powershell/module/azurerm.compute/set-azurermvmoperatingsystem) und [Set-AzureRmVMSourceImage](/powershell/module/azurerm.compute/set-azurermvmsourceimage). Im folgenden Beispiel wird eine Windows Server 2016-VM erstellt:

    ```powershell
    $vmConfig = Set-AzureRmVMOperatingSystem -VM $vmConfig `
        -Windows `
        -ComputerName "myVM" `
        -Credential $cred `
        -ProvisionVMAgent `
        -EnableAutoUpdate
    $vmConfig = Set-AzureRmVMSourceImage -VM $vmConfig `
        -PublisherName "MicrosoftWindowsServer" `
        -Offer "WindowsServer" `
        -Skus "2016-Datacenter" `
        -Version "latest"
   ```

4. Fügen Sie die beiden Netzwerkkarten, die Sie zuvor erstellt haben, mit [Add-AzureRmVMNetworkInterface](/powershell/module/azurerm.compute/add-azurermvmnetworkinterface) an:

    ```powershell
    $vmConfig = Add-AzureRmVMNetworkInterface -VM $vmConfig -Id $myNic1.Id -Primary
    $vmConfig = Add-AzureRmVMNetworkInterface -VM $vmConfig -Id $myNic2.Id
    ```

5. Erstellen Sie dann Ihren virtuellen Computer mit [New-AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm):

    ```powershell
    New-AzureRmVM -VM $vmConfig -ResourceGroupName "myResourceGroup" -Location "EastUs"
    ```

## <a name="add-a-nic-to-an-existing-vm"></a>Hinzufügen einer Netzwerkkarte auf einem vorhandenen virtuellen Computer
Wenn Sie einem vorhandenen virtuellen Computer eine virtuelle Netzwerkkarte hinzufügen möchten, heben Sie die Zuordnung des virtuellen Computers auf, fügen Sie die virtuelle Netzwerkkarte hinzu, und starten Sie anschließend den virtuellen Computer.

1. Heben Sie die Zuordnung des virtuellen Computers mit [Stop-AzureRmVM](/powershell/module/azurerm.compute/stop-azurermvm) auf. Im folgenden Beispiel wird die Zuordnung für den virtuellen Computer *myVM* in *myResourceGroup* aufgehoben:

    ```powershell
    Stop-AzureRmVM -Name "myVM" -ResourceGroupName "myResourceGroup"
    ```

2. Rufen Sie die vorhandene Konfiguration des virtuellen Computers mit [Get-AzureRmVm](/powershell/module/azurerm.compute/get-azurermvm) ab. Im folgenden Beispiel werden Informationen für den virtuellen Computer namens *myVM* in der Ressourcengruppe mit dem Namen *myResourceGroup* abgerufen:

    ```powershell
    $vm = Get-AzureRmVm -Name "myVM" -ResourceGroupName "myResourceGroup"
    ```

3. Das folgende Beispiel erstellt eine virtuelle Netzwerkkarte mit [New-AzureRmNetworkInterface](/powershell/module/azurerm.network/new-azurermnetworkinterface) mit dem Namen *myNic3*, die an *MySubnetBackEnd* angefügt ist. Die virtuelle Netzwerkkarte wird dann an den virtuellen Computer mit dem Namen *myVM* in *MyResourceGroup* mit [Add-AzureRmVMNetworkInterface](/powershell/module/azurerm.compute/add-azurermvmnetworkinterface) angefügt:

    ```powershell
    # Get info for the back end subnet
    $myVnet = Get-AzureRmVirtualNetwork -Name "myVnet" -ResourceGroupName "myResourceGroup"
    $backEnd = $myVnet.Subnets|?{$_.Name -eq 'mySubnetBackEnd'}

    # Create a virtual NIC
    $myNic3 = New-AzureRmNetworkInterface -ResourceGroupName "myResourceGroup" `
        -Name "myNic3" `
        -Location "EastUs" `
        -SubnetId $backEnd.Id

    # Get the ID of the new virtual NIC and add to VM
    $nicId = (Get-AzureRmNetworkInterface -ResourceGroupName "myResourceGroup" -Name "MyNic3").Id
    Add-AzureRmVMNetworkInterface -VM $vm -Id $nicId | Update-AzureRmVm -ResourceGroupName "myResourceGroup"
    ```

    ### <a name="primary-virtual-nics"></a>Primäre virtuelle Netzwerkkarten
    Eine der Netzwerkkarten auf einem virtuellen Computer mit mehreren Netzwerkkarten muss primär sein. Wenn eine der vorhandenen virtuellen Netzwerkkarten auf dem virtuellen Computer bereits als primär festgelegt wurde, können Sie diesen Schritt überspringen. Im folgenden Beispiel wird davon ausgegangen, dass auf einem virtuellen Computer zwei virtuelle Netzwerkkarten vorhanden sind und Sie die erste Netzwerkkarte (`[0]`) als primäre Netzwerkkarte hinzufügen möchten:
        
    ```powershell
    # List existing NICs on the VM and find which one is primary
    $vm.NetworkProfile.NetworkInterfaces
    
    # Set NIC 0 to be primary
    $vm.NetworkProfile.NetworkInterfaces[0].Primary = $true
    $vm.NetworkProfile.NetworkInterfaces[1].Primary = $false
    
    # Update the VM state in Azure
    Update-AzureRmVM -VM $vm -ResourceGroupName "myResourceGroup"
    ```

4. Starten Sie den virtuellen Computer mit [Start-AzureRmVm](/powershell/module/azurerm.compute/start-azurermvm):

    ```powershell
    Start-AzureRmVM -ResourceGroupName "myResourceGroup" -Name "myVM"
    ```

## <a name="remove-a-nic-from-an-existing-vm"></a>Entfernen einer Netzwerkkarte von einem vorhandenen virtuellen Computer
Wenn Sie von einem vorhandenen virtuellen Computer eine virtuelle Netzwerkkarte entfernen möchten, heben Sie die Zuordnung des virtuellen Computers auf, entfernen Sie die virtuelle Netzwerkkarte, und starten Sie anschließend den virtuellen Computer.

1. Heben Sie die Zuordnung des virtuellen Computers mit [Stop-AzureRmVM](/powershell/module/azurerm.compute/stop-azurermvm) auf. Im folgenden Beispiel wird die Zuordnung für den virtuellen Computer *myVM* in *myResourceGroup* aufgehoben:

    ```powershell
    Stop-AzureRmVM -Name "myVM" -ResourceGroupName "myResourceGroup"
    ```

2. Rufen Sie die vorhandene Konfiguration des virtuellen Computers mit [Get-AzureRmVm](/powershell/module/azurerm.compute/get-azurermvm) ab. Im folgenden Beispiel werden Informationen für den virtuellen Computer namens *myVM* in der Ressourcengruppe mit dem Namen *myResourceGroup* abgerufen:

    ```powershell
    $vm = Get-AzureRmVm -Name "myVM" -ResourceGroupName "myResourceGroup"
    ```

3. Rufen Sie die Informationen zur zu entfernenden Netzwerkkarte mit [Get-AzureRmNetworkInterface](/powershell/module/azurerm.network/get-azurermnetworkinterface) ab. Im folgenden Beispiel werden Informationen über *myNic3*abgerufen.

    ```powershell
    # List existing NICs on the VM if you need to determine NIC name
    $vm.NetworkProfile.NetworkInterfaces

    $nicId = (Get-AzureRmNetworkInterface -ResourceGroupName "myResourceGroup" -Name "myNic3").Id   
    ```

4. Entfernen Sie die Netzwerkkarte mit [Remove-AzureRmVMNetworkInterface](/powershell/module/azurerm.compute/remove-azurermvmnetworkinterface), und aktualisieren Sie anschließend den virtuellen Computer mit [Update-AzureRmVm](/powershell/module/azurerm.compute/update-azurermvm). Im folgenden Beispiel wird *myNic3* (im vorherigen Schritt durch `$nicId` abgerufen) entfernt:

    ```powershell
    Remove-AzureRmVMNetworkInterface -VM $vm -NetworkInterfaceIDs $nicId | `
        Update-AzureRmVm -ResourceGroupName "myResourceGroup"
    ```   

5. Starten Sie den virtuellen Computer mit [Start-AzureRmVm](/powershell/module/azurerm.compute/start-azurermvm):

    ```powershell
    Start-AzureRmVM -Name "myVM" -ResourceGroupName "myResourceGroup"
    ```   

## <a name="create-multiple-nics-with-templates"></a>Erstellen Sie mehrere Netzwerkkarten mit Vorlagen
Azure Resource Manager-Vorlagen bieten eine Möglichkeit, während der Bereitstellung mehrere Instanzen einer Ressource zu erstellen – z.B. mehrere Netzwerkkarten. Resource Manager-Vorlagen verwenden deklarative JSON-Dateien zum Definieren Ihrer Umgebung. Weitere Informationen finden Sie in der [Übersicht über den Azure Resource Manager](../../azure-resource-manager/resource-group-overview.md). Mithilfe von *copy* können Sie die Anzahl der zu erstellenden Instanzen angeben:

```json
"copy": {
    "name": "multiplenics",
    "count": "[parameters('count')]"
}
```

Weitere Informationen finden Sie unter [Erstellen mehrerer Instanzen mithilfe von *copy*](../../resource-group-create-multiple.md). 

Sie können einem Ressourcennamen auch mithilfe von `copyIndex()` eine Nummer anfügen. Daraufhin können Sie *myNic1*, *MyNic2* usw. erstellen. Der folgende Code veranschaulicht das Anfügen des Indexwerts:

```json
"name": "[concat('myNic', copyIndex())]", 
```

Hier finden Sie auch ein vollständiges Beispiel für das [Erstellen von mehreren Netzwerkkarten mithilfe von Resource Manager-Vorlagen](../../virtual-network/virtual-network-deploy-multinic-arm-template.md).

## <a name="next-steps"></a>Nächste Schritte
Überprüfen Sie die [Größen für virtuelle Windows-Computer](sizes.md), wenn Sie einen virtuellen Computer mit mehreren Netzwerkkarten erstellen. Achten Sie auf die maximale Anzahl von Netzwerkkarten, die von jeder VM-Größe unterstützt wird. 



