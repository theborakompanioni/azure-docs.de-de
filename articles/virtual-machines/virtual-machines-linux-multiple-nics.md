---
title: Erstellen eines virtuellen Linux-Computers mit mehreren Netzwerkkarten | Microsoft Docs
description: "Erfahren Sie, wie Sie über die Azure-Befehlszeilenschnittstelle oder mithilfe von Resource Manager-Vorlagen einen virtuellen Linux-Computer mit mehreren angefügten Netzwerkkarten erstellen."
services: virtual-machines-linux
documentationcenter: 
author: iainfoulds
manager: timlt
editor: 
ms.assetid: 5d2d04d0-fc62-45fa-88b1-61808a2bc691
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 10/27/2016
ms.author: iainfou
translationtype: Human Translation
ms.sourcegitcommit: d4fa4187b25dcbb7cf3b75cb9186b5d245c89227
ms.openlocfilehash: 12da49e49782869153dcecbf6e4ca0ec24fa5960


---
# <a name="creating-a-linux-vm-with-multiple-nics"></a>Erstellen eines virtuellen Linux-Computers mit mehreren Netzwerkkarten
Sie können einen virtuellen Computer in Azure erstellen, an den mehrere Netzwerkkarten angefügt werden. Häufige Szenarien hierfür sind z.B. unterschiedliche Subnetze für Front-End- und Back-End-Verbindung oder ein Netzwerk für eine Überwachungs- oder Sicherungslösung. Dieser Artikel bietet Informationen zu Schnellbefehlen zum Erstellen eines virtuellen Computers, an den mehrere Netzwerkkarten angefügt werden. Ausführliche Informationen hierzu sowie zum Erstellen von mehreren Netzwerkkarten in Ihren eigenen Bash-Skripts finden Sie in den Informationen zum [Bereitstellen eines virtuellen Computers mit mehreren Netzwerkkarten](../virtual-network/virtual-network-deploy-multinic-arm-cli.md). Verschiedene [VM-Größen](virtual-machines-linux-sizes.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) unterstützen eine unterschiedliche Anzahl von Netzwerkkarten, passen Sie die Größe Ihres virtuellen Computers daher entsprechend an.

> [!WARNING]
> Das Anfügen der Netzwerkkarten muss während der Erstellung des virtuellen Computers erfolgen – Sie können keine Netzwerkkarten an einen vorhandenen virtuellen Computer anfügen. Sie können [einen virtuellen Computer basierend auf dem/den ursprünglichen virtuellen Datenträger(n) erstellen](virtual-machines-linux-copy-vm.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) und beim Bereitstellen des virtuellen Computers mehrere Netzwerkkarten erstellen.
> 
> 

## <a name="quick-commands"></a>Schnellbefehle
Vergewissern Sie sich, dass die [Azure-Befehlszeilenschnittstelle](../xplat-cli-install.md) angemeldet ist und den Resource Manager-Modus nutzt:

```azurecli
azure config mode arm
```

Ersetzen Sie in den folgenden Beispielen die Beispielparameternamen durch Ihre eigenen Werte. Zu den Beispielparameternamen zählen `myResourceGroup`, `mystorageaccount` und `myVM`.

Erstellen Sie zunächst eine Ressourcengruppe. Das folgende Beispiel erstellt eine Ressourcengruppe mit dem Namen `myResourceGroup` am Standort `WestUS`:

```azurecli
azure group create myResourceGroup -l WestUS
```

Erstellen Sie ein Speicherkonto für Ihre virtuellen Computer: Das folgende Beispiel erstellt ein Speicherkonto namens `mystorageaccount`:

```azurecli
azure storage account create mystorageaccount -g myResourceGroup \
    -l WestUS --kind Storage --sku-name PLRS
```

Erstellen Sie ein virtuelles Netzwerk, mit dem Ihre virtuellen Computer eine Verbindung herstellen können. Im folgenden Beispiel wird ein virtuelles Netzwerk namens `myVnet` mit dem Adresspräfix `192.168.0.0/16` erstellt:

```azurecli
azure network vnet create -g myResourceGroup -l WestUS \
    -n myVnet -a 192.168.0.0/16
```

Erstellen Sie zwei virtuelle Subnetze – eins für den Front-End-Datenverkehr, eins für den Back-End-Datenverkehr. Im folgenden Beispiel werden zwei Regeln (`mySubnetFrontEnd` und `mySubnetBackEnd`) erstellt:

```azurecli
azure network vnet subnet create -g myResourceGroup -e myVnet \
    -n mySubnetFrontEnd -a 192.168.1.0/24
azure network vnet subnet create -g myResourceGroup -e myVnet \
    -n mySubnetBackEnd -a 192.168.2.0/24
```

Erstellen Sie zwei Netzwerkkarten, und fügen Sie eine an das Front-End-Subnetz und die andere an das Back-End-Subnetz an. Im folgenden Beispiel werden die zwei Netzwerkkarten `myNic1` und `myNic2` erstellt und Ihren Subnetzen hinzugefügt:

```azurecli
azure network nic create -g myResourceGroup -l WestUS \
    -n myNic1 -m myVnet -k mySubnetFrontEnd
azure network nic create -g myResourceGroup -l WestUS \
    -n myNic2 -m myVnet -k mySubnetBackEnd
```

Schließlich erstellen Sie Ihren virtuellen Computer und fügen die beiden zuvor erstellten Netzwerkkarten an. Im folgenden Beispiel wird ein virtueller Computer namens `myVM` erstellt:

```azurecli
azure vm create \
    --resource-group myResourceGroup \
    --name myVM \
    --location WestUS \
    --os-type linux \
    --nic-names myNic1,myNic2 \
    --vm-size Standard_DS2_v2 \
    --storage-account-name mystorageaccount \
    --image-urn UbuntuLTS \
    --admin-username azureuser \
    --ssh-publickey-file ~/.ssh/id_rsa.pub
```

## <a name="creating-multiple-nics-using-azure-cli"></a>Erstellen mehrerer Netzwerkkarten über die Azure-Befehlszeilenschnittstelle
Wenn Sie bereits früher einen virtuellen Computer über die Azure-Befehlszeilenschnittstelle erstellt haben, sollten Ihnen die Schnellbefehle vertraut sein. Ob Sie eine oder mehrere Netzwerkkarten erstellen – der Prozess ist der gleiche. Informieren Sie sich über das [Bereitstellen von mehreren Netzwerkkarten mithilfe der Azure-Befehlszeilenschnittstelle](../virtual-network/virtual-network-deploy-multinic-arm-cli.md). Hier finden Sie Informationen darüber, wie Sie ein Skript für den Prozess zum Durchlaufen einer Schleife verwenden, um alle Netzwerkkarten zu erstellen.

Im folgenden Beispiel werden die zwei Netzwerkkarten `myNic1` und `myNic2` erstellt. Jede dieser Netzwerkkarten wird mit einem der Subnetze verbunden:

```azurecli
azure network nic create --resource-group myResourceGroup --location WestUS \
    -n myNic1 --subnet-vnet-name myVnet --subnet-name mySubnetFrontEnd
azure network nic create --resource-group myResourceGroup --location WestUS \
    -n myNic2 --subnet-vnet-name myVnet --subnet-name mySubnetBackEnd
```

Üblicherweise erstellen Sie auch eine [Netzwerksicherheitsgruppe](../virtual-network/virtual-networks-nsg.md) oder einen [Lastenausgleich](../load-balancer/load-balancer-overview.md), um den Datenverkehr zwischen den virtuellen Computern zu verwalten und zu verteilen. Die Befehle sind die gleichen, wenn Sie mit mehreren Netzwerkkarten arbeiten. Im folgenden Beispiel wird eine Netzwerksicherheitsgruppe namens `myNetworkSecurityGroup` erstellt:

```azurecli
azure network nsg create --resource-group myResourceGroup --location WestUS \
    --name myNetworkSecurityGroup
```

Binden Sie Ihre Netzwerkkarten mit `azure network nic set` an die Netzwerksicherheitsgruppe. Das folgende Beispiel bindet `myNic1` und `myNic2` an `myNetworkSecurityGroup`:

```azurecli
azure network nic set --resource-group myResourceGroup --name myNic1 \
    --network-security-group-name myNetworkSecurityGroup
azure network nic set --resource-group myResourceGroup --name myNic2 \
    --network-security-group-name myNetworkSecurityGroup
```

Beim Erstellen des virtuellen Computers geben Sie jetzt mehrere Netzwerkkarten an. Statt `--nic-name` (zum Bereitstellen einer einzelnen Netzwerkkarte) verwenden Sie `--nic-names` und stellen eine durch Trennzeichen getrennte Liste von Netzwerkkarten bereit. Gehen Sie umsichtig vor, wenn Sie die Größe der virtuellen Computer auswählen. Sie können einem virtuellen Computer nur eine bestimmte Anzahl von Netzwerkkarten hinzufügen. Weitere Informationen finden Sie unter [Linux-VM-Größen](virtual-machines-linux-sizes.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). Das folgende Beispiel zeigt, wie Sie mehrere Netzwerkkarten angeben und eine Größe für die virtuellen Computer festlegen, die die Verwendung von mehreren Netzwerkkarten unterstützt (`Standard_DS2_v2`):

```azurecli
azure vm create \
    --resource-group myResourceGroup \
    --name myVM \
    --location WestUS \
    --os-type linux \
    --nic-names myNic1,myNic2 \
    --vm-size Standard_DS2_v2 \
    --storage-account-name mystorageaccount \
    --image-urn UbuntuLTS \
    --admin-username azureuser \
    --ssh-publickey-file ~/.ssh/id_rsa.pub
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

Sie können auch `copyIndex()` verwenden und eine Zahl an einen Ressourcennamen anfügen, sodass Sie `myNic1`, `myNic2` usw. erstellen können. Das folgende Beispiel veranschaulicht das Anfügen des Indexwerts:

```json
"name": "[concat('myNic', copyIndex())]", 
```

Ein vollständiges Beispiel finden Sie unter [Erstellen von mehreren Netzwerkkarten mithilfe von Resource Manager-Vorlagen](../virtual-network/virtual-network-deploy-multinic-arm-template.md).

## <a name="next-steps"></a>Nächste Schritte
Überprüfen Sie die [Linux-VM-Größen](virtual-machines-linux-sizes.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) , wenn Sie einen virtuellen Computer mit mehreren Netzwerkkarten erstellen. Achten Sie auf die maximale Anzahl von Netzwerkkarten, die von jeder VM-Größe unterstützt wird. 

Denken Sie daran, dass Sie einem vorhandenen virtuellen Computer keine weiteren Netzwerkkarten hinzufügen können. Sie müssen alle Netzwerkkarten während der Bereitstellung des virtuellen Computers erstellen. Planen Sie Ihre Bereitstellungen sorgfältig, um sicherzustellen, dass Sie die erforderliche Netzwerkkonnektivität von Anfang an einberechnen.




<!--HONumber=Jan17_HO1-->


