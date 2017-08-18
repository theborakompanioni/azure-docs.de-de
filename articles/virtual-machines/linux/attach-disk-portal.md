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
ms.date: 03/07/2017
ms.author: cynthn
ms.translationtype: HT
ms.sourcegitcommit: 398efef3efd6b47c76967563251613381ee547e9
ms.openlocfilehash: fb3b4ea612777d0ba801342dacf4b2ece46afed6
ms.contentlocale: de-de
ms.lasthandoff: 08/11/2017

---
# <a name="how-to-attach-a-data-disk-to-a-linux-vm-in-the-azure-portal"></a>Anfügen eines Datenträgers an eine Linux-VM im Azure-Portal 
In diesem Artikel wird beschrieben, wie Sie über das Azure-Portal neue und vorhandene Datenträger an einen virtuellen Linux-Computer anfügen können. Sie können auch [einen Datenträger an eine Windows-VM im Azure-Portal anfügen](../windows/attach-managed-disk-portal.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). Sie können entweder verwaltete oder nicht verwaltete Azure-Datenträger verwenden. Verwaltete Datenträger werden von der Azure-Plattform verarbeitet und erfordern weder Vorbereitung und noch einen Speicherort. Nicht verwaltete Datenträger erfordern ein Speicherkonto, und es gelten einige [Kontingente und Grenzwerte](../../azure-subscription-service-limits.md#storage-limits). Weitere Informationen zu Azure Managed Disks finden Sie in der [Übersicht über Azure Managed Disks](../../storage/storage-managed-disks-overview.md).

Bevor Sie Datenträger an Ihren virtuellen Computer anfügen, lesen Sie diese Tipps:

* Die Größe des virtuellen Computers bestimmt, wie viele Datenträger Sie anfügen können. Ausführliche Informationen finden Sie unter [Größen für virtuelle Computer](sizes.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
* Für die Verwendung von Premium-Speicher benötigen Sie einen virtuellen Computer der DS- oder GS-Serie. Sie können für diese virtuellen Computer sowohl Premium- als auch Standard-Datenträger verwenden. Premium-Speicher ist in bestimmten Regionen verfügbar. Nähere Informationen finden Sie unter [Premium-Speicher: Hochleistungsspeicher für Workloads auf virtuellen Azure-Computern](../../storage/storage-premium-storage.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
* Bei an virtuelle Computer angefügten Datenträgern handelt es sich um VHD-Dateien, die in Azure gespeichert sind. Weitere Informationen finden Sie unter [Informationen zu Datenträgern und VHDs für virtuelle Computer](../../storage/storage-about-disks-and-vhds-linux.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).


## <a name="find-the-virtual-machine"></a>Suchen des virtuellen Computers
1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com/)an.
2. Klicken Sie im Menü „Hub“ auf **Virtuelle Computer**.
3. Wählen Sie den gewünschten virtuellen Computer aus der Liste aus.
4. Klicken Sie auf dem Blatt „Virtuelle Computer“ unter **Zusammenfassung** auf **Datenträger**.
   
    ![Öffnen der Datenträgereinstellungen](./media/attach-disk-portal/find-disk-settings.png)

Folgen Sie den Anweisungen zum Anfügen eines [verwalteten](#use-azure-managed-disks) oder eines [nicht verwalteten](#use-unmanaged-disks) Datenträgers.

## <a name="use-azure-managed-disks"></a>Verwenden von verwalteten Azure-Datenträgern

### <a name="attach-a-new-disk"></a>Anfügen eines neuen Datenträgers

1. Klicken Sie auf dem Blatt **Datenträger** auf **+ Datenträger anfügen**.
2. Klicken Sie auf das Dropdownmenü **Name**, und wählen Sie **Datenträger erstellen** aus:

    ![Erstellen eines verwalteten Azure-Datenträgers](./media/attach-disk-portal/create-new-md.png)

3. Geben Sie einen Namen für Ihren verwalteten Datenträger ein. Überprüfen und aktualisieren Sie bei Bedarf die Standardeinstellungen, und klicken Sie dann auf **Erstellen**.
   
   ![Überprüfen der Datenträgereinstellungen](./media/attach-disk-portal/create-new-md-settings.png)

4. Klicken Sie auf **Speichern**, um den verwalteten Datenträger zu speichern und die Konfiguration des virtuellen Computers zu aktualisieren:

   ![Speichern des neuen verwalteten Azure-Datenträgers](./media/attach-disk-portal/confirm-create-new-md.png)

5. Nachdem der Datenträger von Azure erstellt und an den virtuellen Computer angefügt wurde, wird der neue Datenträger in den Datenträgereinstellungen des virtuellen Computers unter **Datenträger**angezeigt. Da es sich bei verwalteten Datenträgern um Ressourcen der obersten Ebene handelt, werden sie im Stammverzeichnis der Ressourcengruppe angezeigt:

   ![Verwalteter Azure-Datenträger in Ressourcengruppe](./media/attach-disk-portal/view-md-resource-group.png)

### <a name="attach-an-existing-disk"></a>Anfügen eines vorhandenen Datenträgers
1. Klicken Sie auf dem Blatt **Datenträger** auf **+ Datenträger anfügen**.
2. Klicken Sie auf das Dropdownmenü **Name**, um eine Liste der vorhandenen verwalteten Datenträger anzuzeigen, auf die von Ihrem Azure-Abonnement aus zugegriffen werden kann. Wählen Sie den verwalteten Datenträger aus, den Sie anfügen möchten:

   ![Anfügen eines vorhandenen verwalteten Azure-Datenträgers](./media/attach-disk-portal/select-existing-md.png)

3. Klicken Sie auf **Speichern**, um den vorhandenen verwalteten Datenträger anzufügen und die Konfiguration des virtuellen Computers zu aktualisieren:
   
   ![Speichern von Aktualisierungen verwalteter Azure-Datenträger](./media/attach-disk-portal/confirm-attach-existing-md.png)

4. Nachdem der Datenträger von Azure an den virtuellen Computer angefügt wurde, wird er in den Datenträgereinstellungen des virtuellen Computers unter **Datenträger**angezeigt.

## <a name="use-unmanaged-disks"></a>Verwenden von nicht verwalteten Datenträgern

### <a name="attach-a-new-disk"></a>Anfügen eines neuen Datenträgers

1. Klicken Sie auf dem Blatt **Datenträger** auf **+ Datenträger anfügen**.
2. Überprüfen und aktualisieren Sie nach Bedarf die Standardeinstellungen, und klicken Sie anschließend auf **OK**.
   
   ![Überprüfen der Datenträgereinstellungen](./media/attach-disk-portal/attach-new.png)
3. Nachdem der Datenträger von Azure erstellt und an den virtuellen Computer angefügt wurde, wird der neue Datenträger in den Datenträgereinstellungen des virtuellen Computers unter **Datenträger**angezeigt.

### <a name="attach-an-existing-disk"></a>Anfügen eines vorhandenen Datenträgers
1. Klicken Sie auf dem Blatt **Datenträger** auf **+ Datenträger anfügen**.
2. Klicken Sie unter **Vorhandenen Datenträger anfügen** auf **VHD-Datei**.
   
   ![Anfügen eines vorhandenen Datenträgers](./media/attach-disk-portal/attach-existing.png)
3. Wählen Sie unter **Speicherkonten**das Konto und den Container aus, der die VHD-Datei enthält.
   
   ![Suchen nach VHD-Dateien](./media/attach-disk-portal/find-storage-container.png)
4. Wählen Sie die VHD-Datei aus.
5. Die ausgewählte Datei wird unter **Vorhandenen Datenträger anfügen** und dort unter **VHD-Datei** angezeigt. Klicken Sie auf **OK**.
6. Nachdem der Datenträger von Azure an den virtuellen Computer angefügt wurde, wird er in den Datenträgereinstellungen des virtuellen Computers unter **Datenträger**angezeigt.


## <a name="next-steps"></a>Nächste Schritte
Nachdem der Datenträger hinzugefügt wurde, müssen Sie ihn für die Verwendung vorbereiten. Weitere Informationen finden Sie unter [Initialisieren eines neuen Datenträgers unter Linux](add-disk.md).

