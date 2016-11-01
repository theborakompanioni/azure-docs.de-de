<properties
   pageTitle="Konfigurieren von mehreren Netzwerkkarten auf einem virtuellen Linux-Computer | Microsoft Azure"
   description="Erfahren Sie, wie Sie über die Azure-Befehlszeilenschnittstelle oder mithilfe von Resource Manager-Vorlagen einen virtuellen Computer mit mehreren angefügten Netzwerkkarten erstellen."
   services="virtual-machines-linux"
   documentationCenter=""
   authors="iainfoulds"
   manager="timlt"
   editor=""/>

<tags
   ms.service="virtual-machines-linux"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-linux"
   ms.workload="infrastructure"
   ms.date="08/02/2016"
   ms.author="iainfou"/>

# Erstellen eines virtuellen Computers mit mehreren Netzwerkkarten
Sie können einen virtuellen Computer in Azure erstellen, an den mehrere Netzwerkkarten angefügt werden. Häufige Szenarien hierfür sind z.B. unterschiedliche Subnetze für Front-End- und Back-End-Verbindung oder ein Netzwerk für eine Überwachungs- oder Sicherungslösung. Dieser Artikel bietet Informationen zu Schnellbefehlen zum Erstellen eines virtuellen Computers, an den mehrere Netzwerkkarten angefügt werden. Ausführliche Informationen hierzu sowie zum Erstellen von mehreren Netzwerkkarten in Ihren eigenen Bash-Skripts finden Sie unter [Bereitstellen von Multi-NIC-VMs](../virtual-network/virtual-network-deploy-multinic-arm-cli.md). Verschiedene [VM-Größen](virtual-machines-linux-sizes.md) unterstützen eine unterschiedliche Anzahl von Netzwerkkarten, passen Sie die Größe Ihres virtuellen Computers daher entsprechend an.

>[AZURE.WARNING] Das Anfügen der Netzwerkkarten muss während der Erstellung des virtuellen Computers erfolgen – Sie können keine Netzwerkkarten an einen vorhandenen virtuellen Computer anfügen. Sie können [einen neuen virtuellen Computer basierend auf dem/den ursprünglichen virtuellen Datenträger(n) erstellen](virtual-machines-linux-copy-vm.md) und beim Bereitstellen des virtuellen Computers mehrere Netzwerkkarten erstellen.

## Schnellbefehle
Vergewissern Sie sich, dass die [Azure-Befehlszeilenschnittstelle](../xplat-cli-install.md) angemeldet ist und den Resource Manager-Modus (`azure config mode arm`) nutzt.

Erstellen Sie zunächst eine Ressourcengruppe:

```bash
azure group create TestRG --location WestUS
```

Erstellen Sie ein Speicherkonto für Ihre virtuellen Computer:

```bash
azure storage account create teststorage --resource-group TestRG \
    --location WestUS --kind Storage --sku-name PLRS
```

Erstellen Sie ein virtuelles Netzwerk, mit Ihre virtuellen Computer eine Verbindung herstellen können:

```bash
azure network vnet create --resource-group TestRG --location WestUS \
    --name TestVNet --address-prefixes 192.168.0.0/16 
```

Erstellen Sie zwei virtuelle Subnetze – eins für den Front-End-Datenverkehr, eins für den Back-End-Datenverkehr:

```bash
azure network vnet subnet create --resource-group TestRG --vnet-name TestVNet \
    --name FrontEnd --address-prefix 192.168.1.0/24
azure network vnet subnet create --resource-group TestRG --vnet-name TestVNet \
    --name BackEnd --address-prefix 192.168.2.0/24
```

Erstellen Sie zwei Netzwerkkarten, und fügen Sie eine an das Front-End-Subnetz und die andere an das Back-End-Subnetz an:

```bash
azure network nic create --resource-group TestRG --location WestUS \
    -n NIC1 --subnet-vnet-name TestVNet --subnet-name FrontEnd
azure network nic create --resource-group TestRG --location WestUS \
    -n NIC2 --subnet-vnet-name TestVNet --subnet-name BackEnd
```

Schließlich erstellen Sie Ihren virtuellen Computer und fügen die beiden zuvor erstellten Netzwerkkarten an:

```bash
azure vm create \            
    --resource-group TestRG \
    --name TestVM1 \
    --location WestUS \
    --os-type linux \
    --nic-names NIC1,NIC2 \
    --vm-size Standard_DS2_v2
    --storage-account-name teststorage \
    --image-urn UbuntuLTS \
    --admin-username ops \
    --ssh-publickey-file ~/.ssh/id_rsa.pub
```

## Erstellen mehrerer Netzwerkkarten über die Azure-Befehlszeilenschnittstelle
Wenn Sie bereits früher einen virtuellen Computer über die Azure-Befehlszeilenschnittstelle erstellt haben, sollten Ihnen die Schnellbefehle vertraut sein. Ob Sie eine oder mehrere Netzwerkkarten erstellen – der Prozess ist der gleiche. Informieren Sie sich über das [Bereitstellen von mehreren Netzwerkkarten mithilfe der Azure-Befehlszeilenschnittstelle](../virtual-network/virtual-network-deploy-multinic-arm-cli.md). Hier finden Sie Informationen darüber, wie Sie ein Skript für den Prozess zum Durchlaufen einer Schleife verwenden, um alle Netzwerkkarten zu erstellen.

Das folgende Beispiel erstellt zwei Netzwerkkarten. Jede dieser Netzwerkkarten wird mit einem der Subnetze verbunden:

```bash
azure network nic create --resource-group TestRG --location WestUS \
    -n NIC1 --subnet-vnet-name TestVNet --subnet-name FrontEnd
azure network nic create --resource-group TestRG --location WestUS \
    -n NIC2 --subnet-vnet-name TestVNet --subnet-name BackEnd
```

Üblicherweise erstellen Sie auch eine [Netzwerksicherheitsgruppe](../virtual-network/virtual-networks-nsg.md) oder einen [Lastenausgleich](../load-balancer/load-balancer-overview.md), um den Datenverkehr zwischen den virtuellen Computern zu verwalten und zu verteilen. Die Befehle sind die gleichen, wenn Sie mit mehreren Netzwerkkarten arbeiten. Die von Ihnen erstellen Netzwerkkarten werden mithilfe von `azure network nic set` an eine Netzwerksicherheitsgruppe oder einen Lastenausgleich gebunden, wie im folgenden Beispiel gezeigt:

```bash
azure network nic set --resource-group TestRG --name NIC1 \
    --network-security-group-name TestNSG
```

Beim Erstellen des virtuellen Computers geben Sie jetzt mehrere Netzwerkkarten an. Statt `--nic-name` (zum Bereitstellen einer einzelnen Netzwerkkarte) verwenden Sie `--nic-names` und stellen eine durch Trennzeichen getrennte Liste von Netzwerkkarten bereit. Gehen Sie umsichtig vor, wenn Sie die Größe der virtuellen Computer auswählen. Sie können einem virtuellen Computer nur eine bestimmte Anzahl von Netzwerkkarten hinzufügen. Weitere Informationen finden Sie unter [Linux-VM-Größen](virtual-machines-linux-sizes.md). Das folgende Beispiel zeigt, wie Sie mehrere Netzwerkkarten angeben und eine Größe für die virtuellen Computer festlegen, die die Verwendung von mehreren Netzwerkkarten unterstützt (`Standard_DS2_v2`):

```bash
azure vm create \            
    --resource-group TestRG \
    --name TestVM1 \
    --location WestUS \
    --os-type linux \
    --nic-names NIC1,NIC2 \
    --vm-size Standard_DS2_v2
    --storage-account-name teststorage \
    --image-urn UbuntuLTS \
    --admin-username ops \
    --ssh-publickey-file ~/.ssh/id_rsa.pub
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
Überprüfen Sie die [Linux-VM-Größen](virtual-machines-linux-sizes.md), wenn Sie einen virtuellen Computer mit mehreren Netzwerkkarten erstellen. Achten Sie auf die maximale Anzahl von Netzwerkkarten, die von jeder VM-Größe unterstützt wird.

Denken Sie daran, dass Sie einem vorhandenen virtuellen Computer keine weiteren Netzwerkkarten hinzufügen können. Sie müssen alle Netzwerkkarten während der Bereitstellung des virtuellen Computers erstellen. Planen Sie Ihre Bereitstellungen sorgfältig, um sicherzustellen, dass Sie die erforderliche Netzwerkkonnektivität von Anfang an einberechnen.

<!---HONumber=AcomDC_0817_2016-->