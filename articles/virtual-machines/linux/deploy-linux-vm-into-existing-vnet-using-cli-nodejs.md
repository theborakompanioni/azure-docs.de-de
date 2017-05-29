---
title: Bereitstellen von virtuellen Linux-Computern in einem vorhandenen Netzwerk mit Azure CLI 1.0 | Microsoft-Dokumentation
description: Bereitstellen von virtuellen Linux-Computern in einem vorhandenen virtuellen Netzwerk mit Azure CLI 1.0
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
ms.devlang: na
ms.topic: article
ms.date: 05/11/2017
ms.author: iainfou
ms.translationtype: Human Translation
ms.sourcegitcommit: 97fa1d1d4dd81b055d5d3a10b6d812eaa9b86214
ms.openlocfilehash: 767a3f7cadba6b1e71e5a8f5995a9db090e419dd
ms.contentlocale: de-de
ms.lasthandoff: 05/11/2017


---

# <a name="how-to-deploy-a-linux-virtual-machine-into-an-existing-azure-virtual-network-with-the-azure-cli-10"></a>Bereitstellen von virtuellen Linux-Computern in einem vorhandenen virtuellen Azure-Netzwerk mit Azure CLI 1.0

In diesem Artikel wird beschrieben, wie Sie Azure CLI 1.0 verwenden, um einen virtuellen Computer (VM) in einem vorhandenen virtuellen Netzwerk (VNET) bereitzustellen. Folgende Anforderungen müssen erfüllt sein:

- [ein Azure-Konto](https://azure.microsoft.com/pricing/free-trial/)
- [Dateien mit den öffentlichen und privaten SSH-Schlüsseln](mac-create-ssh-keys.md)


## <a name="cli-versions-to-complete-the-task"></a>CLI-Versionen zum Durchführen dieser Aufgabe
Führen Sie die Aufgabe mit einer der folgenden CLI-Versionen durch:

- [Azure-CLI 1.0](#quick-commands): Unsere CLI für das klassische Bereitstellungsmodell und das Resource Manager-Bereitstellungsmodell (in diesem Artikel)
- [Azure CLI 2.0](deploy-linux-vm-into-existing-vnet-using-cli.md): Unsere CLI der nächsten Generation für das Resource Manager-Bereitstellungsmodell


## <a name="quick-commands"></a>Schnellbefehle

Falls Sie die Aufgabe schnell durchführen müssen, finden Sie im folgenden Abschnitt eine Erläuterung der erforderlichen Befehle. Ausführlichere Informationen und Kontext für die einzelnen Schritte finden Sie im übrigen Dokument ([ab hier](deploy-linux-vm-into-existing-vnet-using-cli.md#detailed-walkthrough)).

Voraussetzungen: Ressourcengruppe, VNET, NSG mit SSH für eingehende Daten, Subnetz. Ersetzen Sie alle Beispiele durch Ihre eigenen Einstellungen.

### <a name="deploy-the-vm-into-the-virtual-network-infrastructure"></a>Bereitstellen des virtuellen Computers in der virtuellen Netzwerkinfrastruktur

```azurecli
azure vm create myVM \
    -g myResourceGroup \
    -l eastus \
    -y linux \
    -Q Debian \
    -o mystorageaccount \
    -u myAdminUser \
    -M ~/.ssh/id_rsa.pub \
    -n myVM \
    -F myVNet \
    -j mySubnet \
    -N myVNic
```

## <a name="detailed-walkthrough"></a>Ausführliche exemplarische Vorgehensweise

Azure-Ressourcen wie VNETs und Netzwerksicherheitsgruppen sollten statische und langlebige Ressourcen sein, die nur selten bereitgestellt werden. Sobald ein VNET bereitgestellt wurde, kann es von neuen Bereitstellungen ohne Nebenwirkungen auf die Infrastruktur wiederverwendet werden. Stellen Sie sich ein VNET als einen herkömmlichen Hardwarenetzwerkswitch vor. In diesem Fall müssten Sie keinen neuen Hardwarenetzwerkswitch bei jeder Bereitstellung konfigurieren. Bei einem richtig konfigurierten VNET können Sie darin immer wieder mit nur wenig oder sogar ganz ohne Änderungsaufwand, der während der Lebensdauer des VNET erforderlich ist, neue Server bereitstellen.

## <a name="create-the-resource-group"></a>Ressourcengruppe erstellen

Erstellen Sie zuerst eine Ressourcengruppe, um alle Elemente zu organisieren, die Sie in dieser exemplarischen Vorgehensweise erstellen. Weitere Informationen zu Ressourcengruppen finden Sie unter [Azure Resource Manager – Übersicht](../../azure-resource-manager/resource-group-overview.md).

```azurecli
azure group create myResourceGroup --location eastus
```

## <a name="create-the-vnet"></a>Erstellen des VNET

Der erste Schritt besteht aus der Erstellung eines VNET, um die VMs darin zu starten. Das VNET enthält ein Subnetz für diese exemplarische Vorgehensweise. Weitere Informationen über Azure VNETs finden Sie unter [Create a virtual network by using the Azure CLI](../../virtual-network/virtual-networks-create-vnet-arm-cli.md) (Erstellen eines virtuellen Netzwerks mithelfe der Azure-Befehlszeilenschnittstelle)

```azurecli
azure network vnet create myVNet \
    --resource-group myResourceGroup \
    --address-prefixes 10.10.0.0/24 \
    --location eastus
```

## <a name="create-the-network-security-group"></a>Erstellen der Netzwerksicherheitsgruppe

Das Subnetz wird hinter einer vorhandenen Netzwerksicherheitsgruppe aufgebaut. Daher erstellen wir die Netzwerksicherheitsgruppe vor dem Subnetz. Azure-Netzwerksicherheitsgruppen sind gleichwertig mit einer Firewall auf Netzwerkebene. Weitere Informationen zu Azure-Netzwerksicherheitsgruppen finden Sie unter [Erstellen von Netzwerksicherheitsgruppen mit der Azure CLI 2.0](../../virtual-network/virtual-networks-create-nsg-arm-cli.md).

```azurecli
azure network nsg create myNetworkSecurityGroup \
    --resource-group myResourceGroup \
    --location eastus
```

## <a name="add-an-inbound-ssh-allow-rule"></a>Hinzufügen einer eingehenden SSH-Zulassungsregel

Der virtuelle Computer benötigt Zugriff über das Internet. Daher wird eine Regel benötigt, die den an Port 22 eingehenden Datenverkehr über das Netzwerk an Port 22 auf dem virtuellen Computer übergibt.

```azurecli
azure network nsg rule create inboundSSH \
    --resource-group myResourceGroup \
    --nsg-name myNSG \
    --access Allow \
    --protocol Tcp \
    --direction Inbound \
    --priority 100 \
    --source-address-prefix Internet \
    --source-port-range 22 \
    --destination-address-prefix 10.10.0.0/24 \
    --destination-port-range 22
```

## <a name="add-a-subnet-to-the-vnet"></a>Hinzufügen eines Subnetz im VNET

VMs innerhalb des VNET müssen sich in einem Subnetz befinden. Jedes VNET kann mehrere Subnetze enthalten. Erstellen Sie das Subnetz, und ordnen Sie es der Netzwerksicherheitsgruppe zu.

```azurecli
azure network vnet subnet create mySubNet \
    --resource-group myResourceGroup \
    --vnet-name myVNet \
    --address-prefix 10.10.0.0/26 \
    --network-security-group-name myNetworkSecurityGroup
```

Das Subnetz wird jetzt im VNET hinzugefügt und der Netzwerksicherheitsgruppe und der dazugehörigen Regel zugeordnet.


## <a name="add-a-vnic-to-the-subnet"></a>Hinzufügen einer VNic zum Subnetz

Virtuelle Netzwerkkarten (VNICs) sind wichtig, da sie durch das Verbinden mit verschiedenen virtuellen Computern wiederverwendet werden können. Auf diese Weise können Sie die VNIC als statische Ressource beibehalten, während die virtuellen Computer temporärer Art sein können. Erstellen Sie eine VNIC, und ordnen Sie sie dem Subnetz zu, das im vorherigen Schritt erstellt wurde.

```azurecli
azure network nic create myVNic \
    --resource-group myResourceGroup \
    --location eastus \
    ---subnet-vnet-name myVNet \
    --subnet-name mySubNet
```

## <a name="deploy-the-vm-into-the-vnet-and-nsg"></a>Bereitstellen der VM im VNET und in der NSG

Sie verfügen nun über ein VNET und ein darin enthaltenes Subnetz sowie über eine Netzwerksicherheitsgruppe als Schutz für das Subnetz, indem darüber der gesamte eingehende Datenverkehr blockiert wird (mit Ausnahme von Port 22 für SSH). Die VM kann nun innerhalb dieser bestehenden Netzwerkinfrastruktur bereitgestellt werden.

Mithilfe der Azure-CLI und dem `azure vm create`-Befehl wird die Linux-VM für die vorhandene Azure Resource Group, das VNET, Subnetz und die VNic bereitgestellt. Weitere Informationen zur Verwendung der CLI zur Bereitstellung einer vollständigen VM finden Sie unter [Create a complete Linux environment by using the Azure CLI](create-cli-complete.md) (Erstellen einer vollständigen Linux-Umgebung mithilfe der Azure-CLI)

```azurecli
azure vm create myVM \
    --resource-group myResourceGroup \
    --location eastus \
    --os-type linux \
    --image-urn Debian \
    --storage-account-name mystorageaccount \
    --admin-username myAdminUser \
    --ssh-publickey-file ~/.ssh/id_rsa.pub \
    --vnet-name myVNet \
    --vnet-subnet-name mySubnet \
    --nic-name myVNic
```

Mithilfe der CLI-Flags zum Aufrufen vorhandener Ressourcen weisen Sie Azure an, die VM im vorhandenen Netzwerk bereitzustellen. Sobald ein VNET und ein Subnetz bereitgestellt wurden, können sie als statische oder permanente Ressourcen innerhalb Ihrer Azure-Region beibehalten werden.  

## <a name="next-steps"></a>Nächste Schritte

* [Bereitstellen und Verwalten von virtuellen Computern mit Azure-Ressourcen-Manager-Vorlagen und der Azure-CLI](../windows/cli-deploy-templates.md)
* [Direktes Erstellen einer benutzerdefinierten Umgebung für einen virtuellen Linux-Computer über Azure-CLI-Befehle](create-cli-complete.md)
* [Erstellen einer Linux-VM in Azure mithilfe von Vorlagen](create-ssh-secured-vm-from-template.md)

