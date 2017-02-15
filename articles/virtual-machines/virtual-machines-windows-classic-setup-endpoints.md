---
title: Einrichten von Endpunkten auf einem klassischen virtuellen Windows-Computer | Microsoft Docs
description: "Erfahren Sie, wie Endpunkte für einen virtuellen Windows-Computer im klassischen Azure-Portal eingerichtet werden, um die Kommunikation mit einem virtuellen Windows-Computer in Azure zu ermöglichen."
services: virtual-machines-windows
documentationcenter: 
author: cynthn
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: 8afc21c2-d3fb-43a3-acce-aa06be448bb6
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 09/27/2016
ms.author: cynthn
translationtype: Human Translation
ms.sourcegitcommit: f6537e4ebac76b9f3328223ee30647885ee15d3e
ms.openlocfilehash: f022bd4d430fbb94ca9e52f03fd7e5bf69e336ad


---
# <a name="how-to-set-up-endpoints-on-a-classic-windows-virtual-machine-in-azure"></a>Gewusst wie: Einrichten von Endpunkten auf einem klassischen virtuellen Windows-Computer in Azure
Alle virtuellen Windows-Computer, die Sie mit dem klassischen Bereitstellungsmodell in Azure erstellen, können automatisch über einen privaten Netzwerkkanal mit anderen virtuellen Computern im gleichen Clouddienst oder virtuellen Netzwerk kommunizieren. Computer im Internet oder anderen virtuellen Netzwerken erfordern jedoch von den Endpunkten, dass sie den eingehenden Netzwerkdatenverkehr auf einen virtuellen Computer leiten. Dieser Artikel ist auch für [virtuelle Linux-Computer](virtual-machines-linux-classic-setup-endpoints.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json)verfügbar.

> [!IMPORTANT] 
> Azure verfügt über zwei verschiedene Bereitstellungsmodelle für das Erstellen und Verwenden von Ressourcen: [Resource Manager- und klassische Bereitstellung](../azure-resource-manager/resource-manager-deployment-model.md). Dieser Artikel befasst sich mit der Verwendung des klassischen Bereitstellungsmodells. Microsoft empfiehlt für die meisten neuen Bereitstellungen die Verwendung des Ressourcen-Manager-Modells.

Im **Resource Manager**-Bereitstellungsmodell werden Endpunkte mithilfe von **Netzwerksicherheitsgruppen (NSGs)** konfiguriert. Weitere Informationen finden Sie unter [Ermöglichen des externen Zugriffs auf einen virtuellen Computer über das Azure-Portal](virtual-machines-windows-nsg-quickstart-portal.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

Beim Erstellen eines virtuellen Windows-Computers im klassischen Azure-Portal werden allgemeine Endpunkte, z.B. für Remotedesktop und Windows PowerShell-Remoting, in der Regel automatisch erstellt. Sie können zusätzliche Endpunkte beim Erstellen des virtuellen Computers oder später nach Bedarf konfigurieren.

[!INCLUDE [virtual-machines-common-classic-setup-endpoints](../../includes/virtual-machines-common-classic-setup-endpoints.md)]

## <a name="next-steps"></a>Nächste Schritte
* Ein Azure PowerShell-Cmdlet zum Einrichten eines VM-Endpunkts finden Sie unter [Add-AzureEndpoint](https://msdn.microsoft.com/library/azure/dn495300.aspx).
* Informationen dazu, wie Sie ein Azure PowerShell-Cmdlet zum Verwalten einer ACL für einen Endpunkt verwenden, finden Sie unter [Verwalten von Zugriffssteuerungslisten (ACLs) für Endpunkte mithilfe von PowerShell](../virtual-network/virtual-networks-acl-powershell.md).
* Wenn Sie einen virtuellen Computer im Resource Manager-Bereitstellungsmodell erstellt haben, können Sie Azure PowerShell verwenden, um [Netzwerksicherheitsgruppen](../virtual-network/virtual-networks-create-nsg-arm-ps.md) zum Steuern des Datenverkehrs an den virtuellen Computer zu erstellen.




<!--HONumber=Feb17_HO2-->


