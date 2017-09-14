---
title: Erstellen eines virtuellen Linux-Computers in Azure mit mehreren Netzwerkkarten | Microsoft-Dokumentation
description: "Erfahren Sie, wie Sie mithilfe von Azure CLI 2.0 oder mithilfe von Resource Manager-Vorlagen einen virtuellen Linux-Computer mit mehreren angefügten Netzwerkkarten erstellen."
services: virtual-machines-linux
documentationcenter: 
author: iainfoulds
manager: timlt
editor: 
ms.assetid: 5d2d04d0-fc62-45fa-88b1-61808a2bc691
ms.service: virtual-machines-linux
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 05/11/2017
ms.author: iainfou
ms.translationtype: HT
ms.sourcegitcommit: a16daa1f320516a771f32cf30fca6f823076aa96
ms.openlocfilehash: ff3e3121102eedaa1f439e517570d0a97cf07c22
ms.contentlocale: de-de
ms.lasthandoff: 09/02/2017

---
# <a name="how-to-create-a-linux-virtual-machine-in-azure-with-multiple-network-interface-cards"></a>Erstellen eines virtuellen Linux-Computers in Azure mit mehreren Netzwerkschnittstellenkarten
Sie können einen virtuellen Computer in Azure erstellen, an den mehrere Netzwerkkarten angefügt werden. Häufige Szenarien hierfür sind z.B. unterschiedliche Subnetze für Front-End- und Back-End-Verbindung oder ein Netzwerk für eine Überwachungs- oder Sicherungslösung. In diesem Artikel erfahren Sie, wie Sie einen virtuellen Computer mit mehreren Netzwerkkarten erstellen und wie Sie Netzwerkkarten vorhandener virtueller Computer hinzufügen oder entfernen. Ausführliche Informationen hierzu sowie zum Erstellen von mehreren Netzwerkkarten in Ihren eigenen Bash-Skripts finden Sie in den Informationen zum [Bereitstellen eines virtuellen Computers mit mehreren Netzwerkkarten](../../virtual-network/virtual-network-deploy-multinic-arm-cli.md). Verschiedene [VM-Größen](sizes.md) unterstützen eine unterschiedliche Anzahl von Netzwerkkarten, passen Sie die Größe Ihres virtuellen Computers daher entsprechend an.

In diesem Artikel erfahren Sie, wie Sie eine VM mit mehreren Netzwerkkarten mithilfe von Azure CLI 2.0 erstellen. Sie können diese Schritte auch mit [Azure CLI 1.0](multiple-nics-nodejs.md) ausführen.


## <a name="create-supporting-resources"></a>Erstellen von unterstützenden Ressourcen
Installieren Sie die neueste Version von [Azure CLI 2.0](/cli/azure/install-az-cli2), und melden Sie sich mit [az login](/cli/azure/#login) bei einem Azure-Konto an.

Ersetzen Sie in den folgenden Beispielen die Beispielparameternamen durch Ihre eigenen Werte. Beispielparameternamen sind u.a. *myResourceGroup*, *mystorageaccount* und *myVM*.

Erstellen Sie zunächst mit [az group create](/cli/azure/group#create) eine Ressourcengruppe. Im folgenden Beispiel wird eine Ressourcengruppe mit dem Namen *myResourceGroup* am Standort *eastus* erstellt:

```azurecli
az group create --name myResourceGroup --location eastus
```

Erstellen Sie das virtuelle Netzwerk mit [az network vnet create](/cli/azure/network/vnet#create). Im folgenden Beispiel werden das virtuelle Netzwerk *myVnet* und das Subnetz *mySubnetFrontEnd* erstellt:

```azurecli
az network vnet create \
    --resource-group myResourceGroup \
    --name myVnet \
    --address-prefix 192.168.0.0/16 \
    --subnet-name mySubnetFrontEnd \
    --subnet-prefix 192.168.1.0/24
```

Erstellen Sie mit [az network vnet subnet create](/cli/azure/network/vnet/subnet#create) ein Subnetz für den Back-End-Datenverkehr. Im folgenden Beispiel wird das Subnetz *mySubnetBackEnd* erstellt:

```azurecli
az network vnet subnet create \
    --resource-group myResourceGroup \
    --vnet-name myVnet \
    --name mySubnetBackEnd \
    --address-prefix 192.168.2.0/24
```

Erstellen Sie mit [az network nsg create](/cli/azure/network/nsg#create) eine Netzwerksicherheitsgruppe. Im folgenden Beispiel wird eine Netzwerksicherheitsgruppe namens *myNetworkSecurityGroup* erstellt:

```azurecli
az network nsg create \
    --resource-group myResourceGroup \
    --name myNetworkSecurityGroup
```

## <a name="create-and-configure-multiple-nics"></a>Erstellen und Konfigurieren mehrerer Netzwerkkarten
Erstellen Sie mit [az network nic create](/cli/azure/network/nic#create) zwei Netzwerkkarten. Im folgenden Beispiel werden die beiden mit der Netzwerksicherheitsgruppe verknüpften Netzwerkkarten *myNic1* und *myNic2* erstellt. Jede dieser Netzwerkkarten ist mit allen Subnetzen verbunden:

```azurecli
az network nic create \
    --resource-group myResourceGroup \
    --name myNic1 \
    --vnet-name myVnet \
    --subnet mySubnetFrontEnd \
    --network-security-group myNetworkSecurityGroup
az network nic create \
    --resource-group myResourceGroup \
    --name myNic2 \
    --vnet-name myVnet \
    --subnet mySubnetBackEnd \
    --network-security-group myNetworkSecurityGroup
```

## <a name="create-a-vm-and-attach-the-nics"></a>Erstellen eines virtuellen Computers und Anfügen der Netzwerkkarten
Wenn Sie den virtuellen Computer erstellen, geben Sie mit `--nics` die Netzwerkkarten an, die Sie erstellt haben. Gehen Sie umsichtig vor, wenn Sie die Größe der virtuellen Computer auswählen. Sie können einem virtuellen Computer nur eine bestimmte Anzahl von Netzwerkkarten hinzufügen. Weitere Informationen finden Sie unter [Linux-VM-Größen](sizes.md). 

Erstellen Sie mit [az vm create](/cli/azure/vm#create) einen virtuellen Computer. Im folgenden Beispiel wird ein virtueller Computer namens *myVM* erstellt:

```azurecli
az vm create \
    --resource-group myResourceGroup \
    --name myVM \
    --image UbuntuLTS \
    --size Standard_DS3_v2 \
    --admin-username azureuser \
    --generate-ssh-keys \
    --nics myNic1 myNic2
```

## <a name="add-a-nic-to-a-vm"></a>Hinzufügen einer Netzwerkkarte zu einem virtuellen Computer
In den vorangegangenen Schritten wurde ein virtueller Computer mit mehreren Netzwerkkarten erstellt. Sie können mit Azure CLI 2.0 auch einem vorhandenen virtuellen Computer Netzwerkkarten hinzufügen. 

Erstellen Sie mit [az network nic create](/cli/azure/network/nic#create) eine weitere Netzwerkkarte. Das folgende Beispiel erstellt die Netzwerkkarte *myNic3*, die mit dem Back-End-Subnetz und der Netzwerksicherheitsgruppe verbunden ist, die in den vorherigen Schritten erstellt wurden:

```azurecli
az network nic create \
    --resource-group myResourceGroup \
    --name myNic3 \
    --vnet-name myVnet \
    --subnet mySubnetBackEnd \
    --network-security-group myNetworkSecurityGroup
```

Um einem vorhandenen virtuellen Computer eine Netzwerkkarte hinzuzufügen, heben Sie zunächst die Zuordnung des virtuellen Computers mit [az vm deallocate](/cli/azure/vm#deallocate) auf. Im folgenden Beispiel wird die Zuordnung für den virtuellen Computer *myVM* aufgehoben:


```azurecli
az vm deallocate --resource-group myResourceGroup --name myVM
```

Fügen Sie die Netzwerkkarte mit [az vm nic add](/cli/azure/vm/nic#add) hinzu. Im folgenden Beispiel wird *myVM* die Netzwerkkarte *myNic3* hinzugefügt:

```azurecli
az vm nic add \
    --resource-group myResourceGroup \
    --vm-name myVM \
    --nics myNic3
```

Starten Sie den virtuellen Computer mit [az vm start](/cli/azure/vm#start):

```azurecli
az vm start --resource-group myResourceGroup --name myVM
```

## <a name="remove-a-nic-from-a-vm"></a>Entfernen einer Netzwerkkarte von einem virtuellen Computer
Um eine Netzwerkkarte von einem vorhandenen virtuellen Computer zu entfernen, heben Sie zunächst die Zuordnung des virtuellen Computers mit [az vm deallocate](/cli/azure/vm#deallocate) auf. Im folgenden Beispiel wird die Zuordnung für den virtuellen Computer *myVM* aufgehoben:

```azurecli
az vm deallocate --resource-group myResourceGroup --name myVM
```

Entfernen Sie die Netzwerkkarte mit [az vm nic remove](/cli/azure/vm/nic#remove). Im folgenden Beispiel wird *myNic3* von *myVM* entfernt:

```azurecli
az vm nic remove \
    --resource-group myResourceGroup \
    --vm-name myVM 
    --nics myNic3
```

Starten Sie den virtuellen Computer mit [az vm start](/cli/azure/vm#start):

```azurecli
az vm start --resource-group myResourceGroup --name myVM
```


## <a name="create-multiple-nics-using-resource-manager-templates"></a>Erstellen von mehreren Netzwerkkarten mithilfe von Resource Manager-Vorlagen
Azure Resource Manager-Vorlagen verwenden deklarative JSON-Dateien zum Definieren Ihrer Umgebung. Lesen Sie eine [Übersicht über Azure Resource Manager](../../azure-resource-manager/resource-group-overview.md). Resource Manager-Vorlagen bieten eine Möglichkeit, während der Bereitstellung mehrere Instanzen einer Ressource zu erstellen – z.B. mehrere Netzwerkkarten. Mit *copy* geben Sie die Anzahl der zu erstellenden Instanzen an:

```json
"copy": {
    "name": "multiplenics"
    "count": "[parameters('count')]"
}
```

Informieren Sie sich über das [Erstellen mehrerer Instanzen mithilfe von *copy*](../../resource-group-create-multiple.md). 

Sie können auch `copyIndex()` verwenden und eine Zahl an einen Ressourcennamen anfügen, sodass Sie `myNic1`, `myNic2` usw. erstellen können. Das folgende Beispiel veranschaulicht das Anfügen des Indexwerts:

```json
"name": "[concat('myNic', copyIndex())]", 
```

Ein vollständiges Beispiel finden Sie unter [Erstellen von mehreren Netzwerkkarten mithilfe von Resource Manager-Vorlagen](../../virtual-network/virtual-network-deploy-multinic-arm-template.md).

## <a name="configure-guest-os-for-multiple-nics"></a>Konfigurieren von Gastbetriebssystem für mehrere Netzwerkadapter

Beim Erstellen mehrerer Netzwerkadapter für eine VM, die auf einem Linux-Gastbetriebssystem basiert, müssen zusätzliche Routingregeln erstellt werden, die das Senden und Empfangen ausschließlich von Datenverkehr ermöglichen, der zu einem bestimmten Netzwerkadapter gehört. Andernfalls kann der zu „eth1“ gehörende Datenverkehr aufgrund der definierten Standardroute nicht korrekt verarbeitet werden.  


### <a name="solution"></a>Lösung

Fügen Sie zunächst der Datei „/etc/iproute2/rt_tables“ zwei Routingtabellen hinzu.

```bash
echo "200 eth0-rt" >> /etc/iproute2/rt_tables
echo "201 eth1-rt" >> /etc/iproute2/rt_tables
```

Damit die Änderung dauerhaft ist und während der Netzwerkstapelaktivierung angewendet wird, müssen die Dateien */etc/sysconfig/network-scipts/ifcfg-eth0* und */etc/sysconfig/network-scipts/ifcfg-eth1* geändert werden.
Ändern Sie die Zeile *NM_CONTROLLED=yes* in *NM_CONTROLLED=no*.
Ohne diesen Schritt sind die hinzugefügten zusätzlichen Regeln und das hinzugefügte zusätzliche Routing unwirksam.
 
Als Nächstes werden die Routingtabellen erweitert. Damit die nächsten Schritte anschaulicher sind, setzen wir das folgende Setup voraus.

*Routing*

```bash
default via 10.0.1.1 dev eth0 proto static metric 100
10.0.1.0/24 dev eth0 proto kernel scope link src 10.0.1.4 metric 100
10.0.1.0/24 dev eth1 proto kernel scope link src 10.0.1.5 metric 101
168.63.129.16 via 10.0.1.1 dev eth0 proto dhcp metric 100
169.254.169.254 via 10.0.1.1 dev eth0 proto dhcp metric 100
```
    
*Schnittstellen*

```bash
lo: inet 127.0.0.1/8 scope host lo
eth0: inet 10.0.1.4/24 brd 10.0.1.255 scope global eth0    
eth1: inet 10.0.1.5/24 brd 10.0.1.255 scope global eth1
```
    
    
Mit den obigen Informationen können die folgenden zusätzlichen Dateien als Stamm erstellt werden.

*   /etc/sysconfig/network-scripts/rule-eth0
*   /etc/sysconfig/network-scripts/route-eth0
*   /etc/sysconfig/network-scripts/rule-eth1
*   /etc/sysconfig/network-scripts/route-eth1

Der Inhalt der einzelnen Dateien lautet wie folgt:
```bash
cat /etc/sysconfig/network-scripts/rule-eth0
from 10.0.1.4/32 table eth0-rt
to 10.0.1.4/32 table eth0-rt

cat /etc/sysconfig/network-scripts/route-eth0
10.0.1.0/24 dev eth0 table eth0-rt
default via 10.0.1.1 dev eth0 table eth0-rt

cat /etc/sysconfig/network-scripts/rule-eth1
from 10.0.1.5/32 table eth1-rt
to 10.0.1.5/32 table eth1-rt

cat /etc/sysconfig/network-scripts/route-eth1
10.0.1.0/24 dev eth1 table eth1-rt
default via 10.0.1.1 dev eth1 table eth1-rt
```

Nachdem die Dateien erstellt und gefüllt worden sind, muss der Netzwerkdienst neu gestartet werden (`systemctl restart network`).

Das Herstellen einer Verbindung von außen mit „eth0“ oder „eth1“ ist jetzt möglich.

## <a name="next-steps"></a>Nächste Schritte
Überprüfen Sie die [Linux-VM-Größen](sizes.md), wenn Sie einen virtuellen Computer mit mehreren Netzwerkkarten erstellen. Achten Sie auf die maximale Anzahl von Netzwerkkarten, die von jeder VM-Größe unterstützt wird. 

