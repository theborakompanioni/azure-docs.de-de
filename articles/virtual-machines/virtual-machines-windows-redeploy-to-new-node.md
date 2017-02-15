---
title: Erneutes Bereitstellen von virtuellen Windows-Computern | Microsoft Docs
description: Beschreibt, wie Sie virtuelle Windows-Computer zum Beheben von Problemen mit der Remotedesktopverbindung erneut bereitstellen.
services: virtual-machines-windows
documentationcenter: virtual-machines
author: iainfoulds
manager: timlt
tags: azure-resource-manager,top-support-issue
ms.assetid: 0ee456ee-4595-4a14-8916-72c9110fc8bd
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: support-article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 09/19/2016
ms.author: iainfou
translationtype: Human Translation
ms.sourcegitcommit: 5919c477502767a32c535ace4ae4e9dffae4f44b
ms.openlocfilehash: 61dbf8fb96233d2e0f592deacf139a83cee51954


---
# <a name="redeploy-virtual-machine-to-new-azure-node"></a>Einen virtuellen Computer in einem neuen Azure-Knoten erneut bereitstellen
Wenn Sie Schwierigkeiten mit der Problembehandlung bei der Remotedesktopverbindung (RDP) oder dem Anwendungszugriff auf einen Windows-basierten virtuellen Azure-Computer haben, lassen sich diese u. U. durch das erneute Bereitstellen des virtuellen Computers beseitigen. Wenn Sie einen virtuellen Computer erneut bereitstellen, wird er innerhalb der Azure-Infrastruktur auf einen neuen Knoten verschoben und dann wieder eingeschaltet. Dabei werden alle Ihre Konfigurationsoptionen und zugehörigen Ressourcen beibehalten. In diesem Artikel erfahren Sie, wie ein virtueller Computer mithilfe von Azure PowerShell oder dem Azure-Portal erneut bereitgestellt wird.

> [!NOTE]
> Nachdem Sie einen virtuellen Computer erneut bereitgestellt haben, geht der temporäre Datenträger verloren, und die der virtuellen Netzwerkschnittstelle zugeordneten dynamischen IP-Adressen werden aktualisiert. 
> 
> 

## <a name="using-azure-powershell"></a>Verwenden von Azure PowerShell
Stellen Sie sicher, dass Sie das aktuelle Azure PowerShell 1.x auf Ihrem virtuellen Computer installiert haben. Weitere Informationen finden Sie unter [Installieren und Konfigurieren von Azure PowerShell](../powershell-install-configure.md).

Verwenden Sie diesen Azure PowerShell-Befehl, um Ihren virtuellen Computer erneut bereitzustellen:

    Set-AzureRmVM -Redeploy -ResourceGroupName $rgname -Name $vmname 


[!INCLUDE [virtual-machines-common-redeploy-to-new-node](../../includes/virtual-machines-common-redeploy-to-new-node.md)]

## <a name="next-steps"></a>Nächste Schritte
Falls beim Herstellen einer Verbindung mit Ihrem virtuellen Computer Probleme auftreten, finden Sie spezifische Hilfe unter [Problembehandlung bei RDP-Verbindungen](virtual-machines-windows-troubleshoot-rdp-connection.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) oder [Ausführliche Schritte zur Problembehandlung bei RDP](virtual-machines-windows-detailed-troubleshoot-rdp.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). Sie können auch die Informationen zur [Problembehandlung bei der Anwendung](virtual-machines-windows-troubleshoot-app-connection.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) lesen, wenn Sie auf eine Anwendung, die auf Ihrem virtuellen Computer ausgeführt wird, nicht zugreifen können.




<!--HONumber=Nov16_HO3-->


