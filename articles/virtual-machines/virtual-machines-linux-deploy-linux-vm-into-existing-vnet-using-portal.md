---
title: Bereitstellen einer Linux-VM in einem vorhandenen virtuellen Azure-Netzwerk mithilfe des Portals | Microsoft Docs
description: Bereitstellen einer Linux-VM in einem vorhandenen virtuellen Azure-Netzwerk mithilfe des Portals.
services: virtual-machines-linux
documentationcenter: virtual-machines-linux
author: vlivech
manager: timlt
editor: 
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 11/21/2016
ms.author: v-livech
translationtype: Human Translation
ms.sourcegitcommit: 6b285b67c3d5bcc89be4d371e444796501eb7de2
ms.openlocfilehash: 7c07a99f9a72cb69812191ca042fac905d2e3706


---

# <a name="deploy-a-linux-vm-into-an-existing-vnet--nsg-using-the-portal"></a>Bereitstellen einer Linux-VM in einem vorhandenen VNET & NSG mithilfe des Portals

In diesem Artikel wird das Bereitstellen einer VM in einem vorhandenen virtuellen Netzwerk (VNET) veranschaulicht.  Es wird empfohlen, dass Azure-Ressourcen wie VNETs und NSGs (Netzwerksicherheitsgruppen) statische und langlebige Ressourcen sein sollten, die nur selten bereitgestellt werden.  Sobald ein VNET bereitgestellt wurde, kann es von neuen Bereitstellungen ohne Nebenwirkungen auf die Infrastruktur konstant wiederverwendet werden.  Wenn Sie ein VNET als traditionellen Hardwarenetzwerkswitch ansehen, müssten Sie keinen neuen Hardwarenetzwerkswitch bei jeder Bereitstellung konfigurieren.  

Mit einem ordnungsgemäß konfigurierten VNET können wir immer wieder neue Server in diesem VNET mit wenigen oder keinen Änderungen bereitstellen, die während der Lebensdauer des VNET erforderlich sind.

## <a name="create-the-resource-group"></a>Erstellen der Ressourcengruppe

Zuerst stellen wir eine Ressourcengruppe bereit, um alles zu organisieren, was wir in dieser exemplarischen Vorgehensweise erstellen.  Weitere Informationen zu Azure-Ressourcengruppen finden Sie unter [Übersicht über den Azure Resource Manager](../azure-resource-manager/resource-group-overview.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

![createResourceGroup](../media/virtual-machines-linux-deploy-linux-vm-into-existing-vnet-using-portal/createResourceGroup.png)


## <a name="create-the-vnet"></a>Erstellen des VNET

Der erste Schritt besteht aus der Erstellung eines VNET, um die VMs darin zu starten.  Das VNET enthält ein Subnetz, und in einem späteren Schritt ordnen wie die NSG diesem Subnetz zu.

![createVNet](../media/virtual-machines-linux-deploy-linux-vm-into-existing-vnet-using-portal/createVNet.png)

## <a name="add-a-vnic-to-the-subnet"></a>Hinzufügen einer VNic zum Subnetz

Virtuelle Netzwerkkarten (VNics) sind wichtig, da Sie sie zur Verbindung mit anderen VMs verwendet werden können. Somit bleibt die VNic als statische Ressource bestehen, während die VMs temporär sein können. Erstellen Sie eine VNic und ordnen Sie sie dem Subnetz zu, das im vorherigen Schritt erstellt wurde.

![createVNic](../media/virtual-machines-linux-deploy-linux-vm-into-existing-vnet-using-portal/createVNic.png)

## <a name="create-the-nsg"></a>Erstellen der NSG

Azure-NSGs sind gleichwertig mit einer Firewall auf der Netzwerkebene. Weitere Informationen zu Azure NSGs finden Sie unter [Netzwerksicherheitsgruppen](../virtual-network/virtual-networks-nsg.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

![createNSG](../media/virtual-machines-linux-deploy-linux-vm-into-existing-vnet-using-portal/createNSG.png)

## <a name="add-an-inbound-ssh-allow-rule"></a>Hinzufügen einer eingehenden SSH-Zulassungsregel

Die Linux-VM benötigt Zugriff auf das Internet, damit eine Regel erstellt wird, die den eingehenden Port 22-Datenverkehr über das Netzwerk an Port 22 auf der Linux-VM übergibt.

![createInboundSSH](../media/virtual-machines-linux-deploy-linux-vm-into-existing-vnet-using-portal/createInboundSSH.png)

## <a name="associate-the-nsg-with-the-subnet"></a>Zuordnen der NSG zum Subnetz

Mit dem erstellten VNET und Subnetz ordnen wir die NSG dem Subnetz zu.  NSGs können einem ganzen Subnetz oder einer einzelnen VNic zugeordnet werden.  Mit der Firewall, die den Datenverkehr auf der Subnetzebene filtert, werden alle VNics und VMs im Subnetz durch die NSG geschützt, während die NSG nur einer einzelnen VNic zugeordnet ist und nur eine VM schützt.

![associateNSG](../media/virtual-machines-linux-deploy-linux-vm-into-existing-vnet-using-portal/associateNSG.png)


## <a name="deploy-the-vm-into-the-vnet-and-nsg"></a>Bereitstellen der VM im VNET und in der NSG

Mithilfe des Azure-Portals wird die Linux-VM für die vorhandene Azure-Ressourcengruppe, das VNET, das Subnetz und die VNic bereitgestellt.

![createVM](../media/virtual-machines-linux-deploy-linux-vm-into-existing-vnet-using-portal/createVM.png)

Mithilfe des Portals werden vorhandene Ressourcen ausgewählt, und wir weisen Azure an, die VM im vorhandenen Netzwerk bereitzustellen.  In anderen Worten: Sobald ein VNET und ein Subnetz bereitgestellt wurden, können sie als statische oder permanente Ressourcen innerhalb Ihrer Azure-Region beibehalten werden.  

## <a name="next-steps"></a>Nächste Schritte

* [Bereitstellen und Verwalten von virtuellen Computern mit Azure-Ressourcen-Manager-Vorlagen und der Azure-CLI](virtual-machines-linux-cli-deploy-templates.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Direktes Erstellen einer benutzerdefinierten Umgebung für einen virtuellen Linux-Computer über Azure-CLI-Befehle](virtual-machines-linux-create-cli-complete.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Erstellen einer Linux-VM in Azure mithilfe von Vorlagen](virtual-machines-linux-create-ssh-secured-vm-from-template.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)



<!--HONumber=Jan17_HO1-->


