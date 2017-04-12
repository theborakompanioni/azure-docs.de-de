---
title: Verbinden von Linux-VMs in einem Clouddienst | Microsoft Docs
description: Verbinden von virtuellen Linux-Computern, die mit dem klassischen Bereitstellungsmodell erstellt wurden, mit einem Azure-Clouddienst oder einem virtuellen Netzwerk.
services: virtual-machines-linux
documentationcenter: 
author: cynthn
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: 2fd23055-6b34-4ef0-88a8-fc19e32fb3c9
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 07/06/2016
ms.author: cynthn
translationtype: Human Translation
ms.sourcegitcommit: eeb56316b337c90cc83455be11917674eba898a3
ms.openlocfilehash: 92787b7ba6927ecea3e3ee074ed5e1cb869094d1
ms.lasthandoff: 04/03/2017


---
# <a name="connect-linux-virtual-machines-created-with-the-classic-deployment-model-with-a-virtual-network-or-cloud-service"></a>Verbinden virtueller Linux-Computer, die mit dem klassischen Bereitstellungsmodell erstellt wurden, mit einem virtuellen Netzwerk oder einem Clouddienst
> [!IMPORTANT] 
> Azure verfügt über zwei verschiedene Bereitstellungsmodelle für das Erstellen und Verwenden von Ressourcen: [Resource Manager- und klassische Bereitstellung](../../../resource-manager-deployment-model.md). Dieser Artikel befasst sich mit der Verwendung des klassischen Bereitstellungsmodells. Microsoft empfiehlt für die meisten neuen Bereitstellungen die Verwendung des Ressourcen-Manager-Modells.

Virtuelle Linux-Computer, die mit dem klassischen Bereitstellungsmodell erstellt werden, befinden sich immer in einem Clouddienst. Der Clouddienst fungiert als Container und stellt einen eindeutigen öffentlichen DNS-Namen, eine öffentliche IP-Adresse und einen Satz von Endpunkten bereit, damit auf den virtuellen Computer über das Internet zugegriffen werden kann. Der Clouddienst kann sich in einem virtuellen Netzwerk befinden, was jedoch nicht zwingend erforderlich ist. Sie können auch [virtuelle Windows-Computer mit einem virtuellen Netzwerk oder einem Clouddienst verbinden](../../windows/classic/connect-vms.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).

Befindet sich ein Clouddienst nicht in einem virtuellen Netzwerk, handelt es sich um einen *eigenständigen* Clouddienst. Die virtuellen Computer in einem eigenständigen Clouddienst können mit anderen virtuellen Computern nur über deren öffentliche DNS-Namen kommunizieren. Dieser Datenverkehr läuft über das Internet. Wenn ein Clouddienst mit einem virtuellen Netzwerk verbunden ist, können die virtuellen Computer in diesem Clouddienst mit allen anderen virtuellen Computern in dem virtuellen Netzwerk kommunizieren, ohne dass Daten über das Internet gesendet werden.

Wenn Sie Ihre virtuellen Computer im gleichen eigenständigen Clouddienst platzieren, können Sie Lastenausgleich und Verfügbarkeitsgruppen immer noch nutzen. Weitere Informationen finden Sie unter [Lastenausgleich virtueller Computer](../../virtual-machines-linux-load-balance.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) und [Verwalten der Verfügbarkeit virtueller Computer](../manage-availability.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). Sie können die virtuellen Computer jedoch nicht in Subnetzen organisieren oder einen eigenständigen Clouddienst mit Ihrem lokalen Netzwerk verbinden. Hier sehen Sie ein Beispiel:

[!INCLUDE [virtual-machines-common-classic-connect-vms](../../../../includes/virtual-machines-common-classic-connect-vms.md)]

## <a name="next-steps"></a>Nächste Schritte
Nachdem Sie einen virtuellen Computer erstellt haben, empfiehlt es sich, [einen Datenträger hinzuzufügen](attach-disk.md) , damit ein Speicherort für die Daten der Dienste und Workloads verfügbar ist. 


