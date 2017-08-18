---
title: "Verwenden von internen DNS-Namen für die VM-Namensauflösung in Azure | Microsoft-Dokumentation"
description: "Verwenden von internen DNS-Namen für die VM-Namensauflösung in Azure."
services: virtual-machines-linux
documentationcenter: 
author: vlivech
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 12/05/2016
ms.author: v-livech
ms.translationtype: HT
ms.sourcegitcommit: 398efef3efd6b47c76967563251613381ee547e9
ms.openlocfilehash: bfba2cf38a0624e8480a32bf153f391d820da5a1
ms.contentlocale: de-de
ms.lasthandoff: 08/11/2017

---

# <a name="using-internal-dns-for-vm-name-resolution-on-azure"></a>Verwenden von internen DNS-Namen für die VM-Namensauflösung in Azure

In diesem Artikel wird gezeigt, wie Sie mithilfe von virtuellen Netzwerkkarten (VNic) und DNS-Bezeichnungsnamen statische interne DNS-Namen für Linux-VMs festlegen. Statische DNS-Namen werden für permanente Infrastrukturdienste wie einen Jenkins-Buildserver, der in diesem Dokument als Beispiel dient, oder einen Git-Server verwendet.

Folgende Anforderungen müssen erfüllt sein:

* [ein Azure-Konto](https://azure.microsoft.com/pricing/free-trial/)
* [Dateien mit den öffentlichen und privaten SSH-Schlüsseln](mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)


## <a name="cli-versions-to-complete-the-task"></a>CLI-Versionen zum Durchführen dieser Aufgabe
Führen Sie die Aufgabe mit einer der folgenden CLI-Versionen durch:

- [Azure-CLI 1.0](#quick-commands): Unsere CLI für das klassische Bereitstellungsmodell und das Resource Manager-Bereitstellungsmodell (in diesem Artikel)
- [Azure CLI 2.0:](static-dns-name-resolution-for-linux-on-azure.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) Unsere CLI der nächsten Generation für das Resource Manager-Bereitstellungsmodell


## <a name="quick-commands"></a>Schnellbefehle

Falls Sie die Aufgabe schnell durchführen müssen, finden Sie im folgenden Abschnitt eine Erläuterung der erforderlichen Befehle. Ausführlichere Informationen und Kontext für die einzelnen Schritte finden Sie im übrigen Dokument ([ab hier](#detailed-walkthrough)).  

Voraussetzungen: Ressourcengruppe, VNet, NSG mit SSH für eingehende Daten, Subnetz.

### <a name="create-a-vnic-with-a-static-internal-dns-name"></a>Erstellen einer VNic mit einem statischen internen DNS-Namen

Das CLI-Flag `-r` dient zum Festlegen der DNS-Bezeichnung, die den statischen DNS-Namen für die VNic bereitstellt.

```azurecli
azure network nic create jenkinsVNic \
-g myResourceGroup \
-l westus \
-m myVNet \
-k mySubNet \
-r jenkins
```

### <a name="deploy-the-vm-into-the-vnet-nsg-and-connect-the-vnic"></a>Bereitstellen der VM im VNet, NSG und Verbinden der VNic

Das Flag `-N` verbindet die VNic während der Bereitstellung in Azure mit der neuen VM.

```azurecli
azure vm create jenkins \
-g myResourceGroup \
-l westus \
-y linux \
-Q Debian \
-o myStorageAcct \
-u myAdminUser \
-M ~/.ssh/id_rsa.pub \
-F myVNet \
-j mySubnet \
-N jenkinsVNic
```

## <a name="detailed-walkthrough"></a>Ausführliche exemplarische Vorgehensweise

Für eine vollständige CiCd-Infrastruktur (Continuous Integration und Continuous Deployment) in Azure müssen bestimmte Server statisch oder langlebige Server sein.  Azure-Ressourcen wie virtuelle Netzwerke (VNets) und Netzwerksicherheitsgruppen (NSGs) sollten der Empfehlung nach statische und langlebige Ressourcen sein, die nur selten bereitgestellt werden.  Sobald ein VNET bereitgestellt wurde, kann es von neuen Bereitstellungen ohne Nebenwirkungen auf die Infrastruktur wiederverwendet werden.  Sie können CiCd für Ihre Entwicklungs- oder Testumgebungen bereitstellen, indem Sie diesem statischen Netzwerk einen Git-Repositoryserver und einen Jenkins-Automatisierungsserver hinzufügen.  

Interne DNS-Namen können nur innerhalb eines virtuellen Azure-Netzwerks aufgelöst werden.  Da die DNS-Namen intern sind, können sie nicht für das externe Internet aufgelöst werden, wodurch die Sicherheit der Infrastruktur zusätzlich erhöht wird.

_Ersetzen Sie alle Beispiele durch Ihre eigenen Einstellungen._

## <a name="create-the-resource-group"></a>Erstellen der Ressourcengruppe

Wir benötigen eine Ressourcengruppe, um alles zu organisieren, was wir in dieser exemplarischen Vorgehensweise erstellen.  Weitere Informationen zu Azure-Ressourcengruppen finden Sie unter [Übersicht über den Azure Resource Manager](../../azure-resource-manager/resource-group-overview.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

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
--source-address-prefix * \
--source-port-range * \
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

## <a name="creating-static-dns-names"></a>Erstellen von statischen DNS-Namen

Azure ist äußerst flexibel. Um DNS-Namen für die Namensauflösung von VMs verwenden zu können, müssen Sie sie jedoch mithilfe von DNS-Bezeichnungen als virtuelle Netzwerkkarten (VNics) erstellen.  VNics sind wichtig, da Sie sie zur Verbindung mit anderen VMs wiederverwenden können. Dadurch bleibt die VNic als statische Ressource bestehen, während die VMs temporär sein können.  Durch die Verwendung von DNS-Bezeichnungen für die VNic können wir eine einfache Namensauflösung von anderen VMs im VNet ermöglichen.  Wenn auflösbare Namen verwendet werden, können andere VMs anhand des DNS-Namens `Jenkins` auf den Automatisierungsserver oder mit `gitrepo` auf den Git-Server zugreifen.  Erstellen Sie eine VNic und ordnen Sie sie dem Subnetz zu, das im vorherigen Schritt erstellt wurde.

```azurecli
azure network nic create jenkinsVNic \
-g myResourceGroup \
-l westus \
-m myVNet \
-k mySubNet \
-r jenkins
```

## <a name="deploy-the-vm-into-the-vnet-and-nsg"></a>Bereitstellen der VM im VNET und in der NSG

Wir verfügen jetzt über eine VNET, ein Subnetz innerhalb dieses VNET und eine NSG, die als Firewall fungiert, um unsere Subnetz zu schützen, indem der eingehenden Datenverkehr mit Ausnahme des Port 22 für SSH blockiert wird.  Die VM kann nun innerhalb dieser bestehenden Netzwerkinfrastruktur bereitgestellt werden.

Mithilfe der Azure-CLI und dem `azure vm create`-Befehl wird die Linux-VM für die vorhandene Azure Resource Group, das VNET, Subnetz und die VNic bereitgestellt.  Weitere Informationen zur Verwendung der CLI zur Bereitstellung einer vollständigen VM finden Sie unter [Create a complete Linux environment by using the Azure CLI](create-cli-complete.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) (Erstellen einer vollständigen Linux-Umgebung mithilfe der Azure-CLI)

```azurecli
azure vm create jenkins \
--resource-group myResourceGroup myVM \
--location westus \
--os-type linux \
--image-urn Debian \
--storage-account-name mystorageaccount \
--admin-username myAdminUser \
--ssh-publickey-file ~/.ssh/id_rsa.pub \
--vnet-name myVNet \
--vnet-subnet-name mySubnet \
--nic-name jenkinsVNic
```

Mithilfe der CLI-Flags zum Aufruf vorhandener Ressourcen weisen wir Azure an, die VM im vorhandenen Netzwerk bereitzustellen.  In anderen Worten: Sobald ein VNET und ein Subnetz bereitgestellt wurden, können sie als statische oder permanente Ressourcen innerhalb Ihrer Azure-Region beibehalten werden.  

## <a name="next-steps"></a>Nächste Schritte
* [Direktes Erstellen einer benutzerdefinierten Umgebung für einen virtuellen Linux-Computer über Azure-CLI-Befehle](create-cli-complete.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Erstellen einer Linux-VM in Azure mithilfe von Vorlagen](create-ssh-secured-vm-from-template.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

