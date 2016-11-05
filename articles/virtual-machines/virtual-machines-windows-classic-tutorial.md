---
title: Erstellen einer virtuellen Maschine unter Windows im klassischen Portal | Microsoft Docs
description: Erstellen Sie einen virtuellen Windows-Computer im klassischen Azure-Portal.
services: virtual-machines-windows
documentationcenter: ''
author: cynthn
manager: timlt
editor: ''
tags: azure-service-management

ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 09/27/2016
ms.author: cynthn

---
# <a name="create-a-virtual-machine-running-windows-in-the-azure-classic-portal"></a>Erstellen eines virtuellen Computers unter Windows im klassischen Azure-Portal
> [!div class="op_single_selector"]
> * [Klassisches Azure-Portal](virtual-machines-windows-classic-tutorial.md)
> * [PowerShell: Klassische Bereitstellung](virtual-machines-windows-classic-create-powershell.md)
> 
> 

<br>

[!INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]

Erfahren Sie, wie Sie [diese Schritte mit dem Resource Manager-Bereitstellungsmodell ausführen](virtual-machines-windows-hero-tutorial.md). Falls Sie das neue Azure-Portal verwenden möchten, lesen Sie die Informationen unter [Erstellen Ihres ersten virtuellen Windows-Computers im Azure-Portal](virtual-machines-windows-hero-tutorial.md).

In diesem Tutorial erfahren Sie, wie einfach es ist, eine virtuelle Azure-Maschine (VM) mit Windows im klassischen Azure-Portal zu erstellen. Als Beispiel wird ein Windows Server-Image verwendet. Dies ist jedoch nur eines von vielen Images, die Azure bietet. Beachten Sie, dass Ihre Imageauswahl von Ihrem Abonnement abhängt. Beispielsweise können Windows-Desktop-Images für MSDN-Abonnenten verfügbar sein.

In diesem Abschnitt erfahren Sie, wie Sie die Option **Aus Galerie** im klassischen Azure-Portal verwenden, um einen virtuellen Computer zu erstellen. Mit dieser Option erhalten Sie mehr Konfigurationsoptionen als mit **Schnellerfassung** . Wenn Sie beispielsweise einen virtuellen Computer mit einem virtuellen Netzwerk verbinden möchten, sollten Sie die Option **Aus Galerie** verwenden.

Sie können auch mithilfe [eigener Images](virtual-machines-windows-classic-createupload-vhd.md)virtuelle Computer erstellen. Informationen zu dieser und zu anderen Methoden finden Sie unter [Verschiedene Möglichkeiten zum Erstellen eines virtuellen Windows-Computers](virtual-machines-windows-creation-choices.md).

## <a name="video-walkthrough"></a>Exemplarische Vorgehensweise per Video
Hier finden Sie eine exemplarische Vorgehensweise für dieses Lernprogramm.

[!VIDEO https://channel9.msdn.com/Blogs/Azure-Documentation-Shorts/Creating-a-Windows-VM-on-Microsoft-Azure-GA-Portal/player]


## <a name="<a-id="createvirtualmachine">-</a>create-the-virtual-machine"></a><a id="createvirtualmachine"> </a>Erstellen des virtuellen Computers
[!INCLUDE [virtual-machines-create-WindowsVM](../../includes/virtual-machines-create-windowsvm.md)]

## <a name="next-steps"></a>Nächste Schritte
* Melden Sie sich beim virtuellen Computer an. Weitere Informationen finden Sie unter [Anmelden bei einem virtuellen Computer, auf dem Windows Server ausgeführt wird](virtual-machines-windows-classic-connect-logon.md).
* Fügen Sie einen Datenträger zum Speichern von Daten hinzu. Sie können sowohl leere Datenträger als auch Datenträger mit Daten anfügen. Anweisungen finden Sie unter [Anfügen eines Datenträgers an einen virtuellen Windows-Computer, der mit dem klassischen Bereitstellungsmodell erstellt wurde](virtual-machines-windows-classic-attach-disk.md).

<!--HONumber=Oct16_HO2-->


