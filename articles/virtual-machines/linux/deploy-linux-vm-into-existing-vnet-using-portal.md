---
title: Bereitstellen virtueller Linux-Computer in vorhandenen Netzwerken mit dem Azure-Portal | Microsoft-Dokumentation
description: Bereitstellen einer Linux-VM in einem vorhandenen virtuellen Azure-Netzwerk mithilfe des Portals.
services: virtual-machines-linux
documentationcenter: virtual-machines-linux
author: iainfoulds
manager: timlt
editor: 
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 05/11/2017
ms.author: iainfou
ms.translationtype: Human Translation
ms.sourcegitcommit: 97fa1d1d4dd81b055d5d3a10b6d812eaa9b86214
ms.openlocfilehash: 964c0fc41773b50a9fbe476df47460484c2ada66
ms.contentlocale: de-de
ms.lasthandoff: 05/11/2017


---

# <a name="how-to-deploy-a-linux-virtual-machine-into-an-existing-azure-virtual-network-with-the-azure-portal"></a>Erfahren Sie, wie Sie virtuelle Linux-Computer mit dem Azure-Portal in einem vorhandenen virtuellen Azure-Netzwerk bereitstellen.

In diesem Artikel wird veranschaulicht, wie Sie einen virtuellen Computer (VM) in einem vorhandenen virtuellen Netzwerk (VNET) bereitstellen. Azure-Objekte wie VNETs und Netzwerksicherheitsgruppen sollten statische und langlebige Ressourcen sein, die nur selten bereitgestellt werden. Sobald ein VNET bereitgestellt wurde, kann es von neuen Bereitstellungen ohne Nebenwirkungen auf die Infrastruktur konstant wiederverwendet werden. Wenn Sie ein VNET als traditionellen Hardwarenetzwerkswitch ansehen, müssten Sie nicht bei jeder Bereitstellung einen neuen Hardwarenetzwerkswitch konfigurieren.  

Bei einem richtig konfigurierten VNET können Sie darin immer wieder mit nur wenig oder sogar ganz ohne Änderungsaufwand, der während der Lebensdauer des VNET erforderlich ist, neue Server bereitstellen.

## <a name="create-the-resource-group"></a>Ressourcengruppe erstellen

Erstellen Sie zuerst eine Ressourcengruppe, um alle Elemente zu organisieren, die Sie in dieser exemplarischen Vorgehensweise erstellen. Weitere Informationen zu Azure-Ressourcengruppen finden Sie unter [Übersicht über den Azure Resource Manager](../../azure-resource-manager/resource-group-overview.md).

![createResourceGroup](./media/deploy-linux-vm-into-existing-vnet-using-portal/createResourceGroup.png)


## <a name="create-the-vnet"></a>Erstellen des VNET

Erstellen Sie als Nächstes ein VNET, in dem die VMs gestartet werden können. Das VNET enthält ein Subnetz und wird der Netzwerksicherheitsgruppe mit diesem Subnets in einem späteren Schritt zugeordnet.

![createVNet](./media/deploy-linux-vm-into-existing-vnet-using-portal/createVNet.png)

## <a name="add-a-vnic-to-the-subnet"></a>Hinzufügen einer VNic zum Subnetz

Virtuelle Netzwerkkarten (VNICs) sind wichtig, da Sie sie mit verschiedenen virtuellen Computern verbinden können. Auf diese Weise können Sie die VNIC als statische Ressource beibehalten, während die virtuellen Computer temporärer Art sein können. Erstellen Sie eine VNIC, und ordnen Sie sie dem Subnetz zu, das im vorherigen Schritt erstellt wurde.

![createVNic](./media/deploy-linux-vm-into-existing-vnet-using-portal/createVNic.png)

## <a name="create-the-network-security-group"></a>Erstellen der Netzwerksicherheitsgruppe

Azure-Netzwerksicherheitsgruppen sind gleichwertig mit einer Firewall auf Netzwerkebene. Weitere Informationen zu Azure-Netzwerksicherheitsgruppen finden Sie im Artikel, in dem die Frage [Was ist eine Netzwerksicherheitsgruppe (NSG)?](../../virtual-network/virtual-networks-nsg.md) beantwortet wird.

![createNSG](./media/deploy-linux-vm-into-existing-vnet-using-portal/createNSG.png)

## <a name="add-an-inbound-ssh-allow-rule"></a>Hinzufügen einer eingehenden SSH-Zulassungsregel

Da die Linux-VM Zugriff auf das Internet benötigt, wird eine Regel erstellt, die den eingehenden Port 22-Datenverkehr über das Netzwerk an Port 22 auf der Linux-VM übergibt.

![createInboundSSH](./media/deploy-linux-vm-into-existing-vnet-using-portal/createInboundSSH.png)

## <a name="associate-the-nsg-with-the-subnet"></a>Zuordnen der NSG zum Subnetz

Wenn das VNET und das Subnetz erstellt wurden, können Sie die Netzwerksicherheitsgruppe dem Subnetz zuordnen. Netzwerksicherheitsgruppen können einem ganzen Subnetz oder einer einzelnen VNIC zugeordnet werden. Da mit der Firewall der Datenverkehr auf Subnetzebene gefiltert wird, sind alle VNICs und die VMs im Subnetz durch die Netzwerksicherheitsgruppe geschützt. Der andere Ansatz besteht darin, dass die Netzwerksicherheitsgruppe nur einer einzelnen VNIC zugeordnet und nur zum Schützen einer VM verwendet wird.

![associateNSG](./media/deploy-linux-vm-into-existing-vnet-using-portal/associateNSG.png)


## <a name="deploy-the-vm-into-the-vnet-and-nsg"></a>Bereitstellen der VM im VNET und in der NSG

Mithilfe des Azure-Portals wird die Linux-VM für die vorhandene Azure-Ressourcengruppe, das VNET, das Subnetz und die VNic bereitgestellt.

![createVM](./media/deploy-linux-vm-into-existing-vnet-using-portal/createVM.png)

Indem Sie mithilfe des Portals vorhandene Ressourcen auswählen, weisen Sie Azure an, die VM im vorhandenen Netzwerk bereitzustellen. Sobald ein VNET und ein Subnetz bereitgestellt wurden, können sie als statische oder permanente Ressourcen innerhalb Ihrer Azure-Region beibehalten werden.  

## <a name="next-steps"></a>Nächste Schritte

* [Bereitstellen und Verwalten von virtuellen Computern mit Azure-Ressourcen-Manager-Vorlagen und der Azure-CLI](../windows/cli-deploy-templates.md)
* [Direktes Erstellen einer benutzerdefinierten Umgebung für einen virtuellen Linux-Computer über Azure-CLI-Befehle](create-cli-complete.md)
* [Erstellen einer Linux-VM in Azure mithilfe von Vorlagen](create-ssh-secured-vm-from-template.md)

