---
title: Erstellen eines virtuellen Linux-Computers in Azure mit mehreren Netzwerkkarten | Microsoft-Dokumentation
description: "Erfahren Sie, wie Sie über die Azure-Befehlszeilenschnittstelle oder mithilfe von Resource Manager-Vorlagen einen virtuellen Linux-Computer mit mehreren angefügten Netzwerkkarten erstellen."
services: virtual-machines-linux
documentationcenter: 
author: iainfoulds
manager: timlt
editor: 
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 02/09/2017
ms.author: iainfou
translationtype: Human Translation
ms.sourcegitcommit: db3d95cf7efd1caebb9e568ba729b9cbc30e5b78
ms.openlocfilehash: 22001eec6402054239c9215a686512bd74d9bb9f


---
# <a name="create-a-linux-vm-with-multiple-nics-using-the-azure-cli-10"></a>Erstellen eines virtuellen Linux-Computers mit mehreren Netzwerkkarten mithilfe der Azure-Befehlszeilenschnittstelle 1.0
Sie können einen virtuellen Computer in Azure erstellen, an den mehrere Netzwerkkarten angefügt werden. Häufige Szenarien hierfür sind z.B. unterschiedliche Subnetze für Front-End- und Back-End-Verbindung oder ein Netzwerk für eine Überwachungs- oder Sicherungslösung. Dieser Artikel bietet Informationen zu Schnellbefehlen zum Erstellen eines virtuellen Computers, an den mehrere Netzwerkkarten angefügt werden. Ausführliche Informationen hierzu sowie zum Erstellen von mehreren Netzwerkkarten in Ihren eigenen Bash-Skripts finden Sie in den Informationen zum [Bereitstellen eines virtuellen Computers mit mehreren Netzwerkkarten](../virtual-network/virtual-network-deploy-multinic-arm-cli.md). Verschiedene [VM-Größen](virtual-machines-linux-sizes.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) unterstützen eine unterschiedliche Anzahl von Netzwerkkarten, passen Sie die Größe Ihres virtuellen Computers daher entsprechend an.

> [!WARNING]
> Das Anfügen der Netzwerkkarten muss während der Erstellung des virtuellen Computers erfolgen – Sie können keine Netzwerkkarten an einen vorhandenen virtuellen Computer anfügen. Sie können [einen virtuellen Computer basierend auf dem bzw. den ursprünglichen virtuellen Datenträgern erstellen](virtual-machines-linux-copy-vm.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) und beim Bereitstellen des virtuellen Computers mehrere Netzwerkkarten erstellen.


## <a name="cli-versions-to-complete-the-task"></a>CLI-Versionen zum Durchführen dieser Aufgabe
Führen Sie die Aufgabe mit einer der folgenden CLI-Versionen durch:

- [Azure CLI 1.0:](#create-supporting-resources) Unsere CLI für das klassische Bereitstellungsmodell und das Resource Manager-Bereitstellungsmodell
- [Azure CLI 2.0 (Vorschau):](virtual-machines-linux-multiple-nics.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) Unsere Befehlszeilenschnittstelle der nächsten Generation für das Resource Manager-Bereitstellungsmodell (dieser Artikel)


## <a name="create-supporting-resources"></a>Erstellen von unterstützenden Ressourcen
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

## <a name="create-and-configure-multiple-nics"></a>Erstellen und Konfigurieren mehrerer Netzwerkkarten
Informieren Sie sich über das [Bereitstellen von mehreren Netzwerkkarten mithilfe der Azure-Befehlszeilenschnittstelle](../virtual-network/virtual-network-deploy-multinic-arm-cli.md). Hier finden Sie Informationen darüber, wie Sie ein Skript für den Prozess zum Durchlaufen einer Schleife verwenden, um alle Netzwerkkarten zu erstellen.

Im folgenden Beispiel werden die zwei Netzwerkkarten `myNic1` und `myNic2` erstellt. Jede dieser Netzwerkkarten wird mit einem der Subnetze verbunden:

```azurecli
azure network nic create --resource-group myResourceGroup --location WestUS \
    -n myNic1 --subnet-vnet-name myVnet --subnet-name mySubnetFrontEnd
azure network nic create --resource-group myResourceGroup --location WestUS \
    -n myNic2 --subnet-vnet-name myVnet --subnet-name mySubnetBackEnd
```

Üblicherweise erstellen Sie auch eine [Netzwerksicherheitsgruppe](../virtual-network/virtual-networks-nsg.md) oder einen [Lastenausgleich](../load-balancer/load-balancer-overview.md), um den Datenverkehr zwischen den virtuellen Computern zu verwalten und zu verteilen. Im folgenden Beispiel wird eine Netzwerksicherheitsgruppe namens `myNetworkSecurityGroup` erstellt:

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

## <a name="create-a-vm-and-attach-the-nics"></a>Erstellen eines virtuellen Computers und Anfügen der Netzwerkkarten
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


