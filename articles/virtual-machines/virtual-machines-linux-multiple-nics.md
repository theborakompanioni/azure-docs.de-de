---
title: Erstellen eines virtuellen Linux-Computers mit mehreren Netzwerkkarten mithilfe der Azure-Befehlszeilenschnittstelle 2.0 (Vorschau) | Microsoft-Dokumentation
description: "Erfahren Sie, wie Sie über die Azure-Befehlszeilenschnittstelle 2.0 (Vorschau) oder mithilfe von Resource Manager-Vorlagen einen virtuellen Linux-Computer mit mehreren angefügten Netzwerkkarten erstellen."
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
ms.date: 02/10/2017
ms.author: iainfou
translationtype: Human Translation
ms.sourcegitcommit: 368c79b001495e0fb000a4b280023b2299256435
ms.openlocfilehash: a854a15a9119f289344a75638d1042ee6779bb46


---
# <a name="create-a-linux-vm-with-multiple-nics-using-the-azure-cli-20-preview"></a>Erstellen eines virtuellen Linux-Computers mit mehreren Netzwerkkarten mithilfe der Azure-Befehlszeilenschnittstelle 2.0 (Vorschau)
Sie können einen virtuellen Computer in Azure erstellen, an den mehrere Netzwerkkarten angefügt werden. Häufige Szenarien hierfür sind z.B. unterschiedliche Subnetze für Front-End- und Back-End-Verbindung oder ein Netzwerk für eine Überwachungs- oder Sicherungslösung. Dieser Artikel bietet Informationen zu Schnellbefehlen zum Erstellen eines virtuellen Computers, an den mehrere Netzwerkkarten angefügt werden. Ausführliche Informationen hierzu sowie zum Erstellen von mehreren Netzwerkkarten in Ihren eigenen Bash-Skripts finden Sie in den Informationen zum [Bereitstellen eines virtuellen Computers mit mehreren Netzwerkkarten](../virtual-network/virtual-network-deploy-multinic-arm-cli.md). Verschiedene [VM-Größen](virtual-machines-linux-sizes.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) unterstützen eine unterschiedliche Anzahl von Netzwerkkarten, passen Sie die Größe Ihres virtuellen Computers daher entsprechend an.

> [!WARNING]
> Das Anfügen der Netzwerkkarten muss während der Erstellung des virtuellen Computers erfolgen – Sie können keine Netzwerkkarten an einen vorhandenen virtuellen Computer anfügen. Sie können [einen virtuellen Computer basierend auf dem bzw. den ursprünglichen virtuellen Datenträgern erstellen](virtual-machines-linux-copy-vm.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) und beim Bereitstellen des virtuellen Computers mehrere Netzwerkkarten erstellen.


## <a name="cli-versions-to-complete-the-task"></a>CLI-Versionen zum Durchführen dieser Aufgabe
Führen Sie die Aufgabe mit einer der folgenden CLI-Versionen durch:

- [Azure CLI 1.0:](virtual-machines-linux-multiple-nics-nodejs.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) Unsere CLI für das klassische Bereitstellungsmodell und das Resource Manager-Bereitstellungsmodell
- [Azure CLI 2.0 (Vorschau):](#create-supporting-resources) Unsere Befehlszeilenschnittstelle der nächsten Generation für das Resource Manager-Bereitstellungsmodell (dieser Artikel)


## <a name="create-supporting-resources"></a>Erstellen von unterstützenden Ressourcen
Installieren Sie die neueste [Azure-Befehlszeilenschnittstelle 2.0 (Vorschau)](/cli/azure/install-az-cli2), und melden Sie sich mit [az login](/cli/azure/#login) bei einem Azure-Konto an.

Ersetzen Sie in den folgenden Beispielen die Beispielparameternamen durch Ihre eigenen Werte. Zu den Beispielparameternamen zählen `myResourceGroup`, `mystorageaccount` und `myVM`.

Erstellen Sie zunächst mit [az group create](/cli/azure/group#create) eine Ressourcengruppe. Das folgende Beispiel erstellt eine Ressourcengruppe mit dem Namen `myResourceGroup` am Standort `WestUS`:

```azurecli
az group create --name myResourceGroup --location westus
```

Erstellen Sie das virtuelle Netzwerk mit [az network vnet create](/cli/azure/network/vnet#create). Im folgenden Beispiel wird ein virtuelles Netzwerk mit dem Namen `myVnet` und ein Subnetz mit dem Namen `mySubnetFrontEnd` erstellt:

```azurecli
az network vnet create --resource-group myResourceGroup --name myVnet \
  --address-prefix 192.168.0.0/16 --subnet-name mySubnetFrontEnd --subnet-prefix 192.168.1.0/24
```

Erstellen Sie mit [az network vnet subnet create](/cli/azure/network/vnet/subnet#create) ein Subnetz für den Back-End-Datenverkehr. Im folgenden Beispiel wird ein Subnetz namens `mySubnetBackEnd` erstellt:

```azurecli
az network vnet subnet create --resource-group myResourceGroup --vnet-name myVnet \
    --name mySubnetBackEnd --address-prefix 192.168.2.0/24
```

## <a name="create-and-configure-multiple-nics"></a>Erstellen und Konfigurieren mehrerer Netzwerkkarten
Informieren Sie sich über das [Bereitstellen von mehreren Netzwerkkarten mithilfe der Azure-Befehlszeilenschnittstelle](../virtual-network/virtual-network-deploy-multinic-arm-cli.md). Hier finden Sie Informationen darüber, wie Sie ein Skript für den Prozess zum Durchlaufen einer Schleife verwenden, um alle Netzwerkkarten zu erstellen.

Üblicherweise erstellen Sie eine [Netzwerksicherheitsgruppe](../virtual-network/virtual-networks-nsg.md) oder einen [Lastenausgleich](../load-balancer/load-balancer-overview.md), um den Datenverkehr zwischen den virtuellen Computern zu verwalten und zu verteilen. Erstellen Sie mit [az network nsg create](/cli/azure/network/nsg#create) eine Netzwerksicherheitsgruppe. Im folgenden Beispiel wird eine Netzwerksicherheitsgruppe namens `myNetworkSecurityGroup` erstellt:

```azurecli
az network nsg create --resource-group myResourceGroup \
  --name myNetworkSecurityGroup
```

Erstellen Sie mit [az network nic create](/cli/azure/network/nic#create) zwei Netzwerkkarten. Im folgenden Beispiel werden die beiden mit der Netzwerksicherheitsgruppe verknüpften Netzwerkkarten `myNic1` und `myNic2` erstellt. Jede dieser Netzwerkkarten ist mit einem der Subnetze verbunden:

```azurecli
az network nic create --resource-group myResourceGroup --name myNic1 \
  --vnet-name myVnet --subnet mySubnetFrontEnd \
  --network-security-group myNetworkSecurityGroup
az network nic create --resource-group myResourceGroup --name myNic2 \
  --vnet-name myVnet --subnet mySubnetBackEnd \
  --network-security-group myNetworkSecurityGroup
```

## <a name="create-a-vm-and-attach-the-nics"></a>Erstellen eines virtuellen Computers und Anfügen der Netzwerkkarten
Wenn Sie den virtuellen Computer erstellen, geben Sie mit `--nics` die Netzwerkkarten an, die Sie erstellt haben. Gehen Sie umsichtig vor, wenn Sie die Größe der virtuellen Computer auswählen. Sie können einem virtuellen Computer nur eine bestimmte Anzahl von Netzwerkkarten hinzufügen. Weitere Informationen finden Sie unter [Linux-VM-Größen](virtual-machines-linux-sizes.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). 

Erstellen Sie mit [az vm create](/cli/azure/vm#create) einen virtuellen Computer. Im folgenden Beispiel wird ein virtueller Computer namens `myVM` erstellt, der [Azure Managed Disks](../storage/storage-managed-disks-overview.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) verwendet:

```azurecli
az vm create \
    --resource-group myResourceGroup \
    --name myVM \
    --image UbuntuLTS \
    --size Standard_DS2_v2 \
    --admin-username azureuser \
    --ssh-key-value ~/.ssh/id_rsa.pub \
    --nics myNic1 myNic2
```

## <a name="create-multiple-nics-using-resource-manager-templates"></a>Erstellen von mehreren Netzwerkkarten mithilfe von Resource Manager-Vorlagen
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




<!--HONumber=Feb17_HO2-->


