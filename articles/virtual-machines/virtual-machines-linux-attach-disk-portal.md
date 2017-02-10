---
title: "Anfügen eines Datenträgers an eine Linux-VM | Microsoft Docs"
description: "So fügen Sie neue oder vorhandene Datenträgern an eine Linux-VM im Azure-Portal unter Verwendung des Resource Manager-Bereitstellungsmodells an."
services: virtual-machines-linux
documentationcenter: 
author: cynthn
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 5e1c6212-976c-4962-a297-177942f90907
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 07/06/2016
ms.author: cynthn
translationtype: Human Translation
ms.sourcegitcommit: 5919c477502767a32c535ace4ae4e9dffae4f44b
ms.openlocfilehash: 743f6abb871bdc4f65f302f0c1efdc4603c52626


---
# <a name="how-to-attach-a-data-disk-to-a-linux-vm-in-the-azure-portal"></a>Anfügen eines Datenträgers an eine Linux-VM im Azure-Portal 
In diesem Artikel wird beschrieben, wie Sie über das Azure-Portal neue und vorhandene Datenträger an einen virtuellen Linux-Computer anfügen können. Sie können auch [einen Datenträger an eine Windows-VM im Azure-Portal anfügen](virtual-machines-windows-attach-disk-portal.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). Lesen Sie die folgenden Tipps, bevor Sie mit diesem Vorgang fortfahren:

* Die Größe des virtuellen Computers bestimmt, wie viele Datenträger Sie anfügen können. Ausführliche Informationen finden Sie unter [Größen für virtuelle Computer](virtual-machines-linux-sizes.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
* Für die Verwendung von Premium-Speicher benötigen Sie einen virtuellen Computer der DS- oder GS-Serie. Sie können Datenträger aus Premium- und aus Standard-Speicherkonten für diese virtuellen Computer verwenden. Premium-Speicher ist in bestimmten Regionen verfügbar. Nähere Informationen finden Sie unter [Premium-Speicher: Hochleistungsspeicher für Workloads auf virtuellen Azure-Computern](../storage/storage-premium-storage.md).
* Bei Datenträgern, die an virtuelle Computer angefügt werden, handelt es sich um VHD-Dateien in einem Azure-Speicherkonto. Weitere Informationen finden Sie unter [Informationen zu Datenträgern und VHDs für virtuelle Computer](virtual-machines-linux-about-disks-vhds.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
* Neue Datenträger werden von Azure beim Anfügen automatisch erstellt. Sie müssen den Datenträger also nicht manuell erstellen.
* Bei einem bereits vorhandenen Datenträger muss die VHD-Datei in einem Azure-Speicherkonto verfügbar sein. Sie können eine bereits vorhandene VHD verwenden, die noch nicht an einen anderen virtuellen Computer angefügt wurde, oder eine eigene VHD-Datei in das Speicherkonto hochladen.

[!INCLUDE [virtual-machines-common-attach-disk-portal](../../includes/virtual-machines-common-attach-disk-portal.md)]

## <a name="next-steps"></a>Nächste Schritte
Nachdem der Datenträger hinzugefügt wurde, müssen Sie ihn für die Verwendung vorbereiten. Lesen Sie in diesem [Artikel](virtual-machines-linux-classic-attach-disk.md#initialize-a-new-data-disk-in-linux) die Informationen zum Betriebssystem der VM unter „Initialisieren eines neuen Datenträgers unter Linux“.



<!--HONumber=Nov16_HO3-->


