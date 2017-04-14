---
title: Bereitstellen von virtuellen Linux-Computern in einem vorhandenen Netzwerk mit der Azure CLI 2.0 | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie virtuelle Linux-Computer mit der Azure CLI 2.0 in einem vorhandenen Netzwerk bereitstellen.
services: virtual-machines-linux
documentationcenter: virtual-machines
author: iainfoulds
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-linux
ms.workload: infrastructure
ms.tgt_pltfrm: vm-linux
ms.devlang: azurecli
ms.topic: article
ms.date: 01/31/2017
ms.author: iainfou
translationtype: Human Translation
ms.sourcegitcommit: eeb56316b337c90cc83455be11917674eba898a3
ms.openlocfilehash: a43e740bde8d91a47b84787e4bf72e4667b84de6
ms.lasthandoff: 04/03/2017


---

# <a name="deploy-a-linux-vm-into-an-existing-virtual-network"></a>Bereitstellen eines virtuellen Linux-Computers in einem vorhandenen virtuellen Netzwerk

Dieser Artikel veranschaulicht, wie Sie die Azure CLI 2.0 verwenden, um einen virtuellen Computer in einem vorhandenen virtuellen Netzwerk bereitzustellen. Folgende Anforderungen müssen erfüllt sein:

- [ein Azure-Konto](https://azure.microsoft.com/pricing/free-trial/)
- [Dateien mit den öffentlichen und privaten SSH-Schlüsseln](mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

Sie können diese Schritte auch mit der [Azure CLI 1.0](deploy-linux-vm-into-existing-vnet-using-cli-nodejs.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) ausführen.


## <a name="quick-commands"></a>Schnellbefehle
Im folgenden Abschnitt werden die Befehle beschrieben, falls Sie die Aufgabe schnell durchführen müssen. Ausführlichere Informationen und Kontext für die einzelnen Schritte finden Sie im übrigen Dokument ([ab hier](#detailed-walkthrough)).

Zum Erstellen dieser benutzerdefinierten Umgebung benötigen Sie die installierte neueste Version [Azure CLI 2.0](/cli/azure/install-az-cli2) und müssen mithilfe von [az login](/cli/azure/#login) bei einem Azure-Konto angemeldet sein.

Ersetzen Sie in den folgenden Beispielen die Beispielparameternamen durch Ihre eigenen Werte. Als Beispielparameternamen werden `myResourceGroup`, `myVnet` und `myVM` verwendet.

**Voraussetzungen**: Azure-Ressourcengruppe, virtuelles Netzwerk und Subnetz, Netzwerksicherheitsgruppe mit eingehenden SSH-Verbindungen und eine virtuelle Netzwerkschnittstellenkarte.

### <a name="deploy-the-vm-into-the-virtual-network-infrastructure"></a>Bereitstellen des virtuellen Computers in der virtuellen Netzwerkinfrastruktur

```azurecli
az vm create \
    --resource-group myResourceGroup \
    --name myVM \
    --image Debian \
    --admin-username azureuser \
    --ssh-key-value ~/.ssh/id_rsa.pub \
    --nics myNic
```

## <a name="detailed-walkthrough"></a>Ausführliche exemplarische Vorgehensweise

Azure-Ressourcen wie virtuelle Netzwerke und Netzwerksicherheitsgruppen sollten statische und langlebige Ressourcen sein, die nur selten bereitgestellt werden. Sobald ein virtuelles Netzwerk bereitgestellt wurde, kann es von neuen Bereitstellungen ohne Nebenwirkungen auf die Infrastruktur wiederverwendet werden. Stellen Sie sich ein virtuelles Netzwerk als traditionellen Hardwarenetzwerkswitch vor – Sie müssen nicht bei jeder Bereitstellung einen neuen Hardwarenetzwerkswitch konfigurieren. Mit einem ordnungsgemäß konfigurierten virtuellen Netzwerk können Sie immer wieder neue Server in diesem Netzwerk bereitstellen und müssen während der Lebensdauer des Netzwerks nur wenige oder gar keine Änderungen vornehmen.

Zum Erstellen dieser benutzerdefinierten Umgebung benötigen Sie die installierte neueste Version [Azure CLI 2.0](/cli/azure/install-az-cli2) und müssen mithilfe von [az login](/cli/azure/#login) bei einem Azure-Konto angemeldet sein.

Ersetzen Sie in den folgenden Beispielen die Beispielparameternamen durch Ihre eigenen Werte. Als Beispielparameternamen werden `myResourceGroup`, `myVnet` und `myVM` verwendet.

## <a name="create-the-resource-group"></a>Ressourcengruppe erstellen

Zuerst erstellen Sie eine Azure-Ressourcengruppe, um alle Elemente zu organisieren, die in dieser exemplarischen Vorgehensweise erstellt werden. Weitere Informationen zu Ressourcengruppen finden Sie unter [Übersicht über den Azure Resource Manager](../../azure-resource-manager/resource-group-overview.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). Erstellen Sie die Ressourcengruppe mithilfe von [az group create](/cli/azure/group#create). Das folgende Beispiel erstellt eine Ressourcengruppe mit dem Namen `myResourceGroup` am Standort `westus`:

```azurecli
az group create \
    --name myResourceGroup \
    --location westus
```

## <a name="create-the-virtual-network"></a>Erstellen des virtuellen Netzwerks

Erstellen Sie zunächst ein virtuelles Azure-Netzwerk, in dem die virtuellen Computer gestartet werden sollen. Weitere Informationen zu virtuellen Netzwerken finden Sie unter [Erstellen eines virtuellen Netzwerks über die Azure-Befehlszeilenschnittstelle](../../virtual-network/virtual-networks-create-vnet-arm-cli.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). Erstellen Sie das virtuelle Netzwerk mit [az network vnet create](/cli/azure/network/vnet#create). Im folgenden Beispiel wird ein virtuelles Netzwerk mit dem Namen `myVnet` und ein Subnetz mit dem Namen `mySubnet` erstellt:

```azurecli
az network vnet create \
    --resource-group myResourceGroup \
    --location westus \
    --name myVnet \
    --address-prefix 10.10.0.0/16 \
    --subnet-name mySubnet \
    --subnet-prefix 10.10.1.0/24
```

## <a name="create-the-network-security-group"></a>Erstellen der Netzwerksicherheitsgruppe

Azure-Netzwerksicherheitsgruppen sind gleichwertig mit einer Firewall auf Netzwerkebene. Weitere Informationen zu Netzwerksicherheitsgruppen finden Sie unter [Erstellen von NSGs in der Azure-Befehlszeilenschnittstelle](../../virtual-network/virtual-networks-create-nsg-arm-cli.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). Erstellen Sie die Netzwerksicherheitsgruppe mit [az network nsg create](/cli/azure/network/nsg#create). Im folgenden Beispiel wird eine Netzwerksicherheitsgruppe namens `myNetworkSecurityGroup` erstellt:

```azurecli
az network nsg create \
    --resource-group myResourceGroup \
    --location westus \
    --name myNetworkSecurityGroup
```

## <a name="add-an-inbound-ssh-allow-rule"></a>Hinzufügen einer eingehenden SSH-Zulassungsregel

Der virtuelle Linux-Computer benötigt Zugriff über das Internet. Daher wird eine Regel benötigt, die den an Port 22 eingehenden Datenverkehr über das Netzwerk an Port 22 auf dem virtuellen Linux-Computer übergibt. Fügen Sie mithilfe von [az network nsg rule create](/cli/azure/network/nsg/rule#create) eine eingehende Regel für die Netzwerksicherheitsgruppe hinzu. Im folgenden Beispiel wird eine Regel namens `myNetworkSecurityGroupRuleSSH` erstellt:

```azurecli
az network nsg rule create \
    --resource-group myResourceGroup \
    --nsg-name myNetworkSecurityGroup \
    --name myNetworkSecurityGroupRuleSSH \
    --protocol tcp \
    --direction inbound \
    --priority 1000 \
    --source-address-prefix '*' \
    --source-port-range '*' \
    --destination-address-prefix '*' \
    --destination-port-range 22 \
    --access allow
```

## <a name="attach-the-subnet-to-the-network-security-group"></a>Hinzufügen des Subnetzes zur Netzwerksicherheitsgruppe

Regeln für Netzwerksicherheitsgruppen können auf ein Subnetz oder eine bestimmte virtuelle Netzwerkschnittstelle angewendet werden. Wir fügen jetzt die Netzwerksicherheitsgruppe an das Subnetz an. Fügen Sie Ihr Subnetz mithilfe von [az network vnet subnet update](/cli/azure/network/vnet/subnet#update) an die Netzwerksicherheitsgruppe an:

```azurecli
az network vnet subnet update \
    --resource-group myResourceGroup \
    --vnet-name myVnet \
    --name mySubnet \
    --network-security-group myNetworkSecurityGroup
```

## <a name="add-a-virtual-network-interface-card-to-the-subnet"></a>Hinzufügen einer virtuellen Netzwerkschnittstellenkarte zum Subnetz

Virtuelle Netzwerkschnittstellenkarten (VNics) sind wichtig, da Sie sie durch Verbinden mit verschiedenen virtuellen Computern wiederverwenden können. Dank dieser Wiederverwendung können Sie die VNic als statische Ressource beibehalten, während die virtuellen Computer temporär sein können. Erstellen Sie eine VNic, und ordnen Sie sie mit [az network nic create](/cli/azure/network/nic#create) dem Subnetz zu. Im folgenden Beispiel wird eine VNic namens `myNic` erstellt:

```azurecli
az network nic create \
    --resource-group myResourceGroup \
    --location westus \
    --name myNic \
    --vnet-name myVnet \
    --subnet mySubnet
```

## <a name="deploy-the-vm-into-the-virtual-network-infrastructure"></a>Bereitstellen des virtuellen Computers in der virtuellen Netzwerkinfrastruktur

Wir verfügen jetzt über ein virtuelles Netzwerk, ein Subnetz und eine Netzwerksicherheitsgruppe, die als Firewall fungiert. Letztere schützt unser Subnetz, indem sämtlicher eingehender Datenverkehr mit Ausnahme von Port 22 für SSH blockiert wird. Die VM kann nun innerhalb dieser bestehenden Netzwerkinfrastruktur bereitgestellt werden.

Erstellen Sie den virtuellen Computer mit [az vm create](/cli/azure/vm#create). Weitere Informationen zu den Flags, die mit der Azure CLI 2.0 zur Bereitstellung eines vollständigen virtuellen Computers verwendet werden, finden Sie unter [Erstellen einer vollständigen Linux-Umgebung mithilfe der Azure CLI](create-cli-complete.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

Im folgenden Beispiel wird ein virtueller Computer erstellt, der Azure Managed Disks verwendet. Diese Datenträger werden von der Azure-Plattform verarbeitet und erfordern keine Vorbereitung und keinen Speicherort zur Aufbewahrung. Weitere Informationen zu verwalteten Datenträgern finden Sie in der [Übersicht über Managed Disks](../../storage/storage-managed-disks-overview.md). Wenn Sie nicht verwaltete Datenträger verwenden möchten, finden Sie weitere Informationen im obigen Zusatzhinweis.

```azurecli
az vm create \
    --resource-group myResourceGroup \
    --name myVM \
    --image Debian \
    --admin-username azureuser \
    --ssh-key-value ~/.ssh/id_rsa.pub \
    --nics myNic
```

Wenn Sie Managed Disks verwenden, überspringen Sie diesen Schritt. Wenn Sie nicht verwaltete Datenträger verwenden möchten, fügen Sie dem oben stehenden Befehl die folgenden zusätzlichen Parameter hinzu, um die nicht verwalteten Datenträger im Speicherkonto `mystorageaccount` zu erstellen: 

```azurecli
    --use-unmanaged-disk \
    --storage-account mystorageaccount
```

Mithilfe der CLI-Flags zum Aufruf vorhandener Ressourcen weisen wir Azure an, die VM im vorhandenen Netzwerk bereitzustellen. Nochmals: Sobald ein virtuelles Netzwerk und ein Subnetz bereitgestellt wurden, können sie als statische oder permanente Ressourcen innerhalb Ihrer Azure-Region beibehalten werden. In diesem Beispiel haben wir keine öffentliche IP-Adresse erstellt und der VNic zugewiesen, daher ist dieser virtuelle Computer nicht öffentlich über das Internet zugänglich. Weitere Informationen finden Sie unter [Erstellen einer VM mit einer statischen öffentlichen IP-Adresse mithilfe der Azure-Befehlszeilenschnittstelle](../../virtual-network/virtual-network-deploy-static-pip-arm-cli.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen zu verschiedenen Methoden zum Erstellen von virtuellen Computern in Azure finden Sie in den folgenden Ressourcen:

* [Bereitstellen und Verwalten von virtuellen Computern mit Azure-Ressourcen-Manager-Vorlagen und der Azure-CLI](../windows/cli-deploy-templates.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Direktes Erstellen einer benutzerdefinierten Umgebung für einen virtuellen Linux-Computer über Azure-CLI-Befehle](create-cli-complete.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Erstellen einer Linux-VM in Azure mithilfe von Vorlagen](create-ssh-secured-vm-from-template.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

