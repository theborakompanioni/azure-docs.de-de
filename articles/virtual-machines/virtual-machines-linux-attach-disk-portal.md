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
ms.date: 11/28/2016
ms.author: cynthn
translationtype: Human Translation
ms.sourcegitcommit: 7c481cfae5f97b71c0ab184419ceaa46ab3f5a5b
ms.openlocfilehash: 394d82e444bdbc8b07243d92743ceb660f142509


---
# <a name="how-to-attach-a-data-disk-to-a-linux-vm-in-the-azure-portal"></a>Anfügen eines Datenträgers an eine Linux-VM im Azure-Portal 
In diesem Artikel wird beschrieben, wie Sie über das Azure-Portal neue und vorhandene Datenträger an einen virtuellen Linux-Computer anfügen können. Sie können auch [einen Datenträger an eine Windows-VM im Azure-Portal anfügen](virtual-machines-windows-attach-disk-portal.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). Lesen Sie die folgenden Tipps, bevor Sie mit diesem Vorgang fortfahren:

* Die Größe des virtuellen Computers bestimmt, wie viele Datenträger Sie anfügen können. Ausführliche Informationen finden Sie unter [Größen für virtuelle Computer](virtual-machines-linux-sizes.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
* Für die Verwendung von Premium-Speicher benötigen Sie einen virtuellen Computer der DS- oder GS-Serie. Sie können Datenträger aus Premium- und aus Standard-Speicherkonten für diese virtuellen Computer verwenden. Premium-Speicher ist in bestimmten Regionen verfügbar. Nähere Informationen finden Sie unter [Premium-Speicher: Hochleistungsspeicher für Workloads auf virtuellen Azure-Computern](../storage/storage-premium-storage.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
* Bei Datenträgern, die an virtuelle Computer angefügt werden, handelt es sich um VHD-Dateien in einem Azure-Speicherkonto. Weitere Informationen finden Sie unter [Informationen zu Datenträgern und VHDs für virtuelle Computer](virtual-machines-linux-about-disks-vhds.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
* Neue Datenträger werden von Azure beim Anfügen automatisch erstellt. Sie müssen den Datenträger also nicht manuell erstellen.
* Bei einem bereits vorhandenen Datenträger muss die VHD-Datei in einem Azure-Speicherkonto verfügbar sein. Sie können eine bereits vorhandene VHD verwenden, die noch nicht an einen anderen virtuellen Computer angefügt wurde, oder eine eigene VHD-Datei in das Speicherkonto hochladen.


## <a name="find-the-virtual-machine"></a>Suchen des virtuellen Computers
1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com/)an.
2. Klicken Sie im Menü „Hub“ auf **Virtuelle Computer**.
3. Wählen Sie den gewünschten virtuellen Computer aus der Liste aus.
4. Klicken Sie auf dem Blatt „Virtuelle Computer“ unter **Zusammenfassung** auf **Datenträger**.
   
    ![Öffnen der Datenträgereinstellungen](./media/virtual-machines-linux-attach-disk-portal/find-disk-settings.png)

Folgen Sie den Anweisungen zum Anfügen eines [neuen](#option-1-attach-a-new-disk) oder eines [vorhandenen](#option-2-attach-an-existing-disk) Datenträgers.

## <a name="option-1-attach-a-new-disk"></a>Option 1: Anfügen eines neuen Datenträgers
1. Klicken Sie auf dem Blatt **Datenträger** auf **Neuen anfügen**.
2. Überprüfen und aktualisieren Sie nach Bedarf die Standardeinstellungen, und klicken Sie anschließend auf **OK**.
   
   ![Überprüfen der Datenträgereinstellungen](./media/virtual-machines-linux-attach-disk-portal/attach-new.png)
3. Nachdem der Datenträger von Azure erstellt und an den virtuellen Computer angefügt wurde, wird der neue Datenträger in den Datenträgereinstellungen des virtuellen Computers unter **Datenträger**angezeigt.

## <a name="option-2-attach-an-existing-disk"></a>Option 2: Anfügen eines vorhandenen Datenträgers
1. Klicken Sie auf dem Blatt **Datenträger** auf **Vorhandenen anfügen**.
2. Klicken Sie unter **Vorhandenen Datenträger anfügen** auf **VHD-Datei**.
   
   ![Anfügen eines vorhandenen Datenträgers](./media/virtual-machines-linux-attach-disk-portal/attach-existing.png)
3. Wählen Sie unter **Speicherkonten**das Konto und den Container aus, der die VHD-Datei enthält.
   
   ![Suchen nach VHD-Dateien](./media/virtual-machines-linux-attach-disk-portal/find-storage-container.png)
4. Wählen Sie die VHD-Datei aus.
5. Die ausgewählte Datei wird unter **Vorhandenen Datenträger anfügen** und dort unter **VHD-Datei** angezeigt. Klicken Sie auf **OK**.
6. Nachdem der Datenträger von Azure an den virtuellen Computer angefügt wurde, wird er in den Datenträgereinstellungen des virtuellen Computers unter **Datenträger**angezeigt.



## <a name="next-steps"></a>Nächste Schritte
Nachdem der Datenträger hinzugefügt wurde, müssen Sie ihn für die Verwendung vorbereiten. Weitere Informationen finden Sie unter [Initialisieren eines neuen Datenträgers unter Linux](virtual-machines-linux-classic-attach-disk.md#initialize-a-new-data-disk-in-linux).



<!--HONumber=Nov16_HO5-->


