---
title: Erstellen eines virtuellen Computers im Azure-Portal | Microsoft Docs
description: Erstellen Sie einen virtuellen Windows-Computer im Azure-Portal.
services: virtual-machines-windows
documentationcenter: 
author: cynthn
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: 1871f823-ebd7-4eff-9a22-8e2411555595
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 02/27/2017
ms.author: cynthn
translationtype: Human Translation
ms.sourcegitcommit: 15e6eb0fcd3baf34c2144aa3ce13a8aff59f9a5a
ms.openlocfilehash: 502c420b927ab835e585e848f153328d9f9565ee
ms.lasthandoff: 03/01/2017


---
# <a name="create-a-virtual-machine-running-windows-in-the-azure-portal"></a>Erstellen eines virtuellen Windows-Computers im Azure-Portal
> [!div class="op_single_selector"]
> * [klassischen Azure-Portal](virtual-machines-windows-classic-tutorial.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json)
> * [PowerShell: Klassische Bereitstellung](virtual-machines-windows-classic-create-powershell.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json)
>
>

<br>

> [!IMPORTANT]
> Azure verfügt über zwei verschiedene Bereitstellungsmodelle für das Erstellen und Verwenden von Ressourcen: [Resource Manager- und klassische Bereitstellung](../azure-resource-manager/resource-manager-deployment-model.md). Dieser Artikel befasst sich mit der Verwendung des klassischen Bereitstellungsmodells. Microsoft empfiehlt für die meisten neuen Bereitstellungen die Verwendung des Ressourcen-Manager-Modells. Erfahren Sie, wie Sie [diese Schritte mit dem Resource Manager-Bereitstellungsmodell](virtual-machines-windows-hero-tutorial.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) im **Azure-Portal** ausführen.

In diesem Tutorial erfahren Sie, wie Sie im Azure-Portal einen virtuellen Azure-Computer unter Windows erstellen. Als Beispiel wird ein Windows Server-Image verwendet. Dies ist jedoch nur eines von vielen Images, die Azure bietet. Beachten Sie, dass Ihre Imageauswahl von Ihrem Abonnement abhängt. Beispielsweise können Windows-Desktop-Images für MSDN-Abonnenten verfügbar sein.

In diesem Abschnitt erfahren Sie, wie Sie das **Dashboard** im Azure-Portal verwenden, um einen virtuellen Computer auszuwählen und dann zu erstellen.

Sie können auch mithilfe [eigener Images](virtual-machines-windows-classic-createupload-vhd.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json)virtuelle Computer erstellen. Informationen zu dieser und zu anderen Methoden finden Sie unter [Verschiedene Möglichkeiten zum Erstellen eines virtuellen Windows-Computers](virtual-machines-windows-creation-choices.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

<!-- 02/27/2017 Video removed as it was based on the classic portal. -->

## <a id="createvirtualmachine"> </a>Erstellen des virtuellen Computers
[!INCLUDE [virtual-machines-create-WindowsVM](../../includes/virtual-machines-create-windowsvm.md)]

## <a name="next-steps"></a>Nächste Schritte
* Erfahren Sie, wie Sie im Azure-Portal [mit dem Resource Manager-Bereitstellungsmodell einen virtuellen Computer erstellen](virtual-machines-windows-hero-tutorial.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
* Melden Sie sich beim virtuellen Computer an. Weitere Informationen finden Sie unter [Anmelden bei einem virtuellen Computer, auf dem Windows Server ausgeführt wird](virtual-machines-windows-classic-connect-logon.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).
* Fügen Sie einen Datenträger zum Speichern von Daten hinzu. Sie können sowohl leere Datenträger als auch Datenträger mit Daten anfügen. Anweisungen finden Sie unter [Anfügen eines Datenträgers an einen virtuellen Windows-Computer, der mit dem klassischen Bereitstellungsmodell erstellt wurde](virtual-machines-windows-classic-attach-disk.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).

