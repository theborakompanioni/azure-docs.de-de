---
title: "Bereitstellen virtueller Linux-Computer in vorhandenen Netzwerken – Azure CLI | Microsoft-Dokumentation"
description: Bereitstellen einer Linux-VM in einem vorhandenen Virtual Network mithilfe der CLI.
services: virtual-machines-linux
documentationcenter: virtual-machines
author: vlivech
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-linux
ms.workload: infrastructure
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 12/05/2016
ms.author: v-livech
translationtype: Human Translation
ms.sourcegitcommit: eeb56316b337c90cc83455be11917674eba898a3
ms.openlocfilehash: d3f8f35ee3a5b294e77f3bad50edd3d1db221c9c
ms.lasthandoff: 04/03/2017


---

# <a name="deploy-a-linux-vm-into-an-existing-azure-virtual-network-using-the-cli"></a>Bereitstellen einer Linux-VM in einem vorhandenen virtuellen Azure-Netzwerk mithilfe der CLI

In diesem Artikel wird das Bereitstellen eines virtuellen Computer in einem vorhandenen virtuellen Netzwerk (VNET) mit CLI-Flags veranschaulicht.  Folgende Anforderungen müssen erfüllt sein:

- [ein Azure-Konto](https://azure.microsoft.com/pricing/free-trial/)
- [Dateien mit den öffentlichen und privaten SSH-Schlüsseln](mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)


## <a name="cli-versions-to-complete-the-task"></a>CLI-Versionen zum Durchführen dieser Aufgabe
Führen Sie die Aufgabe mit einer der folgenden CLI-Versionen durch:

- [Azure-CLI 1.0](#quick-commands): Unsere CLI für das klassische Bereitstellungsmodell und das Resource Manager-Bereitstellungsmodell (in diesem Artikel)
- [Azure CLI 2.0](deploy-linux-vm-into-existing-vnet-using-cli.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json): Unsere CLI der nächsten Generation für das Resource Manager-Bereitstellungsmodell


## <a name="quick-commands"></a>Schnellbefehle

Im folgenden Abschnitt werden die Befehle beschrieben, falls Sie die Aufgabe schnell durchführen müssen. Ausführlichere Informationen und Kontext für die einzelnen Schritte finden Sie im übrigen Dokument ([ab hier](deploy-linux-vm-into-existing-vnet-using-cli.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json#detailed-walkthrough)).

Voraussetzungen: Ressourcengruppe, VNET, NSG mit SSH für eingehende Daten, Subnetz. Ersetzen Sie alle Beispiele durch Ihre eigenen Einstellungen.

### <a name="deploy-the-vm-into-the-virtual-network-infrastructure"></a>Bereitstellen des virtuellen Computers in der virtuellen Netzwerkinfrastruktur

```azurecli
azure vm create myVM \
-g myResourceGroup \
-l westus \
-y linux \
-Q Debian \
-o myStorageAcct \
-u myAdminUser \
-M ~/.ssh/id_rsa.pub \
-n myVM \
-F myVNet \
-j mySubnet \
-N myVNic
```

## <a name="detailed-walkthrough"></a>Ausführliche exemplarische Vorgehensweise

Es wird empfohlen, dass Azure-Ressourcen wie VNETs und NSGs statische und langlebige Ressourcen sein sollten, die nur selten bereitgestellt werden.  Sobald ein VNET bereitgestellt wurde, kann es von neuen Bereitstellungen ohne Nebenwirkungen auf die Infrastruktur wiederverwendet werden.  Stellen Sie sich ein VNET als einen herkömmlichen Hardwarenetzwerkswitch vor. In diesem Fall müssten Sie keinen neuen Hardwarenetzwerkswitch bei jeder Bereitstellung konfigurieren.  Mit einem ordnungsgemäß konfigurierten VNET können wir immer wieder neue Server in diesem VNET mit wenigen oder keinen Änderungen bereitstellen, die während der Lebensdauer des VNET erforderlich sind.

## <a name="create-the-resource-group"></a>Erstellen der Ressourcengruppe

Zuerst erstellen wir eine Ressourcengruppe, um alles zu organisieren, das wir in dieser exemplarischen Vorgehensweise erstellen.  Weitere Informationen zu Azure-Ressourcengruppen finden Sie unter [Übersicht über den Azure Resource Manager](../../azure-resource-manager/resource-group-overview.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

```azurecli
azure group create myResourceGroup \
--location westus
```

## <a name="create-the-vnet"></a>Erstellen des VNET

Der erste Schritt besteht aus der Erstellung eines VNET, um die VMs darin zu starten.  Das VNET enthält ein Subnetz für diese exemplarische Vorgehensweise.  Weitere Informationen über Azure VNETs finden Sie unter [Create a virtual network by using the Azure CLI](../../virtual-network/virtual-networks-create-vnet-arm-cli.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) (Erstellen eines virtuellen Netzwerks mithelfe der Azure-Befehlszeilenschnittstelle)

```azurecli
azure network vnet create myVNet \
--resource-group myResourceGroup \
--address-prefixes 10.10.0.0/24 \
--location westus
```

## <a name="create-the-nsg"></a>Erstellen der NSG

Das Subnetz wird hinter einer vorhandene Netzwerksicherheitsgruppe aufgebaut. Deshalb erstellen wir die NSG vor dem Subnetz.  Azure-NSGs sind gleichwertig mit einer Firewall auf der Netzwerkebene.  Weitere Informationen zu Azure-NSGs finden Sie unter [Erstellen von NSGs in der Azure-CLI](../../virtual-network/virtual-networks-create-nsg-arm-cli.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

```azurecli
azure network nsg create myNSG \
--resource-group myResourceGroup \
--location westus
```

## <a name="add-an-inbound-ssh-allow-rule"></a>Hinzufügen einer eingehenden SSH-Zulassungsregel

Die Linux-VM benötigt Zugriff über das Internet, damit eine Regel benötigt wird, die den eingehenden Port 22-Datenverkehr über das Netzwerk an Port 22 auf der Linux-VM übergibt.

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

VMs innerhalb des VNET müssen sich in einem Subnetz befinden.  Jedes VNET kann mehrere Subnetze enthalten.  Erstellen des Subnetzes und Zuordnen des Subnetzes zur NSG, um dem Subnetz eine Firewall hinzuzufügen.

```azurecli
azure network vnet subnet create mySubNet \
--resource-group myResourceGroup \
--vnet-name myVNet \
--address-prefix 10.10.0.0/26 \
--network-security-group-name myNSG
```

Das Subnetz ist jetzt innerhalb des VNET hinzugefügt und der NSG und der NSG-Regel zugeordnet.


## <a name="add-a-vnic-to-the-subnet"></a>Hinzufügen einer VNic zum Subnetz

Virtuelle Netzwerkkarten (VNics) sind wichtig, da Sie sie zur Verbindung mit anderen VMs wiederverwendet werden können. Somit bleibt die VNic als statische Ressource bestehen, während die VMs temporär sein können.  Erstellen Sie eine VNic und ordnen Sie sie dem Subnetz zu, das im vorherigen Schritt erstellt wurde.

```azurecli
azure network nic create myVNic \
-g myResourceGroup \
-l westus \
-m myVNet \
-k mySubNet
```

## <a name="deploy-the-vm-into-the-vnet-and-nsg"></a>Bereitstellen der VM im VNET und in der NSG

Wir verfügen jetzt über eine VNET, ein Subnetz innerhalb dieses VNET und eine NSG, die als Firewall fungiert, um unsere Subnetz zu schützen, indem der eingehenden Datenverkehr mit Ausnahme des Port 22 für SSH blockiert wird.  Die VM kann nun innerhalb dieser bestehenden Netzwerkinfrastruktur bereitgestellt werden.

Mithilfe der Azure-CLI und dem `azure vm create`-Befehl wird die Linux-VM für die vorhandene Azure Resource Group, das VNET, Subnetz und die VNic bereitgestellt.  Weitere Informationen zur Verwendung der CLI zur Bereitstellung einer vollständigen VM finden Sie unter [Create a complete Linux environment by using the Azure CLI](create-cli-complete.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) (Erstellen einer vollständigen Linux-Umgebung mithilfe der Azure-CLI)

```azurecli
azure vm create myVM \
--resource-group myResourceGroup \
--location westus \
--os-type linux \
--image-urn Debian \
--storage-account-name mystorageaccount \
--admin-username myAdminUser \
--ssh-publickey-file ~/.ssh/id_rsa.pub \
--vnet-name myVNet \
--vnet-subnet-name mySubnet \
--nic-name myVNic
```

Mithilfe der CLI-Flags zum Aufruf vorhandener Ressourcen weisen wir Azure an, die VM im vorhandenen Netzwerk bereitzustellen.  In anderen Worten: Sobald ein VNET und ein Subnetz bereitgestellt wurden, können sie als statische oder permanente Ressourcen innerhalb Ihrer Azure-Region beibehalten werden.  

## <a name="next-steps"></a>Nächste Schritte

* [Bereitstellen und Verwalten von virtuellen Computern mit Azure-Ressourcen-Manager-Vorlagen und der Azure-CLI](../windows/cli-deploy-templates.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Direktes Erstellen einer benutzerdefinierten Umgebung für einen virtuellen Linux-Computer über Azure-CLI-Befehle](create-cli-complete.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Erstellen einer Linux-VM in Azure mithilfe von Vorlagen](create-ssh-secured-vm-from-template.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

