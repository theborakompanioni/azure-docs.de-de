---
title: "Anfügen eines Datenträgers an einen klassischen virtuellen Azure-Computer | Microsoft-Dokumentation"
description: "Fügen Sie einen Datenträger an einen virtuellen Windows-Computer an, der mit dem klassischen Bereitstellungsmodell erstellt wurde, und initialisieren Sie ihn."
services: virtual-machines-windows, storage
documentationcenter: 
author: cynthn
manager: timlt
editor: tysonn
tags: azure-service-management
ms.assetid: be4e3e74-05bc-4527-969f-84f10a1d66a7
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 02/21/2017
ms.author: cynthn
translationtype: Human Translation
ms.sourcegitcommit: 356de369ec5409e8e6e51a286a20af70a9420193
ms.openlocfilehash: 4debed5ceed2c94b2e5b666d6fcebec5667bfda7
ms.lasthandoff: 03/27/2017


---
# <a name="attach-a-data-disk-to-a-windows-virtual-machine-created-with-the-classic-deployment-model"></a>Anfügen eines Datenträgers an einen virtuellen Windows-Computer, der mit dem klassischen Bereitstellungsmodell erstellt wurde
<!--
Refernce article:
    If you want to use the new portal, see [How to attach a data disk to a Windows VM in the Azure portal](../../virtual-machines-windows-attach-disk-portal.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
-->

In diesem Artikel wird beschrieben, wie Sie über das Azure-Portal neue und vorhandene Datenträger, die mit dem klassischen Bereitstellungsmodell erstellt wurden, an einen virtuellen Windows-Computer anfügen.

Sie können auch [einen Datenträger an eine Linux-VM im Azure-Portal anfügen](../../virtual-machines-linux-attach-disk-portal.md).

Bevor Sie einen Datenträger anfügen, lesen Sie diese Tipps:

* Die Größe des virtuellen Computers bestimmt, wie viele Datenträger Sie anfügen können. Ausführliche Informationen finden Sie unter [Größen für virtuelle Computer](../../virtual-machines-windows-sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

* Für die Verwendung von Premium-Speicher benötigen Sie einen virtuellen Computer der DS- oder GS-Serie. Sie können Datenträger aus Premium- und aus Standard-Speicherkonten für diese virtuellen Computer verwenden. Premium-Speicher ist in bestimmten Regionen verfügbar. Nähere Informationen finden Sie unter [Premium-Speicher: Hochleistungsspeicher für Workloads auf virtuellen Azure-Computern](../../../storage/storage-premium-storage.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

* Neue Datenträger werden von Azure beim Anfügen automatisch erstellt. Sie müssen den Datenträger also nicht manuell erstellen.

Sie können auch [einen Datenträger mithilfe von PowerShell anfügen](../../virtual-machines-windows-attach-disk-ps.md).

> [!IMPORTANT]
> Azure verfügt über zwei verschiedene Bereitstellungsmodelle für das Erstellen und Verwenden von Ressourcen: [Resource Manager- und klassische Bereitstellung](../../../resource-manager-deployment-model.md).

## <a name="find-the-virtual-machine"></a>Suchen des virtuellen Computers
1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com/)an.
2. Wählen Sie den virtuellen Computer unter den Ressourcen aus, die im Dashboard aufgeführt werden.
3. Klicken Sie im linken Bereich unter **Einstellungen** auf **Datenträger**.

    ![Öffnen der Datenträgereinstellungen](./media/attach-disk/virtualmachinedisks.png)

Folgen Sie den Anweisungen zum Anfügen eines [neuen](#option-1-attach-a-new-disk) oder eines [vorhandenen](#option-2-attach-an-existing-disk) Datenträgers.

## <a name="option-1-attach-and-initialize-a-new-disk"></a>Option 1: Anfügen und Initialisieren eines neuen Datenträgers

1. Klicken Sie auf dem Blatt **Datenträger** auf **Neuen anfügen**.
2. Überprüfen und aktualisieren Sie nach Bedarf die Standardeinstellungen, und klicken Sie anschließend auf **OK**.

   ![Überprüfen der Datenträgereinstellungen](./media/attach-disk/attach-new.png)

3. Nachdem der Datenträger von Azure erstellt und an den virtuellen Computer angefügt wurde, wird der neue Datenträger in den Datenträgereinstellungen des virtuellen Computers unter **Datenträger**angezeigt.

### <a name="initialize-a-new-data-disk"></a>Initialisieren eines neuen Datenträgers

1. Stellen Sie eine Verbindung mit dem virtuellen Computer her. Anweisungen dazu finden Sie unter [Gewusst wie: Herstellen einer Verbindung mit einem virtuellen Azure-Computer unter Windows und Anmelden auf diesem Computer](../../virtual-machines-windows-connect-logon.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
2. Öffnen Sie den **Server-Manager**, nachdem Sie sich auf dem virtuellen Computer angemeldet haben. Klicken Sie im linken Bereich auf **Datei- und Speicherdienste**.

    ![Server-Manager öffnen](../media/attach-disk-portal/fileandstorageservices.png)

3. Wählen Sie **Datenträger** aus.
4. Im Abschnitt **Datenträger** werden die Datenträger aufgelistet. In den meisten Fällen verfügt ein virtueller Computer über die Datenträger 0, 1 und 2. Datenträger 0 ist der Betriebssystemdatenträger. Bei Datenträger 1 handelt es sich um den temporären Datenträger, und Datenträger 2 ist der Datenträger, den Sie gerade an den virtuellen Computer angefügt haben. Für den Datenträger für Daten wird die Partition als **Unbekannt** aufgeführt.

 Klicken Sie mit der rechten Maustaste auf den Datenträger, und wählen Sie **Initialisieren** aus.

5. Sie werden darüber informiert, dass alle Daten gelöscht werden, wenn der Datenträger initialisiert wird. Klicken Sie auf **Ja** , um die Warnung zu bestätigen und den Datenträger zu initialisieren. Wenn dieser Vorgang abgeschlossen ist, wird die Partition als **GPT** aufgeführt. Klicken Sie erneut mit der rechten Maustaste auf den Datenträger, und wählen Sie **Neues Volume**.

6. Schließen Sie den Assistenten unter Verwendung der angegebenen Standardwerte ab. Sobald der Assistent fertig ist, wird ein neues Volume im Bereich **Volumes** aufgelistet. Der Datenträger ist jetzt online und zum Speichern von Daten bereit.

    ![Volume erfolgreich initialisiert](./media/attach-disk/newdiskafterinitialization.png)

## <a name="option-2-attach-an-existing-disk"></a>Option 2: Anfügen eines vorhandenen Datenträgers
1. Klicken Sie auf dem Blatt **Datenträger** auf **Vorhandenen anfügen**.
2. Klicken Sie unter **Vorhandenen Datenträger anfügen** auf **Speicherort**.

   ![Anfügen eines vorhandenen Datenträgers](./media/attach-disk/attachexistingdisksettings.png)
3. Wählen Sie unter **Speicherkonten**das Konto und den Container aus, der die VHD-Datei enthält.

   ![Suchen nach VHD-Dateien](./media/attach-disk/existdiskstorageaccountandcontainer.png)

4. Wählen Sie die VHD-Datei aus.
5. Die ausgewählte Datei wird unter **Vorhandenen Datenträger anfügen** und dort unter **VHD-Datei** angezeigt. Klicken Sie auf **OK**.
6. Nachdem der Datenträger von Azure an den virtuellen Computer angefügt wurde, wird er in den Datenträgereinstellungen des virtuellen Computers unter **Datenträger**angezeigt.

## <a name="use-trim-with-standard-storage"></a>Verwenden von TRIM mit dem Standardspeicher

Wenn Sie den Standardspeicher (HDD) verwenden, sollten Sie TRIM aktivieren. Mit TRIM werden ungenutzte Blöcke auf dem Datenträger verworfen, sodass nur der Speicher in Rechnung gestellt wird, den Sie tatsächlich verwenden. Die Verwendung von TRIM kann Kosten sparen, da dabei auch ungenutzte Blöcke entfernt werden, die beim Löschen großer Dateien entstehen.

Sie können den folgenden Befehl ausführen, um die TRIM-Einstellung zu überprüfen. Öffnen Sie eine Eingabeaufforderung auf dem virtuellen Windows-Computer, und geben Sie Folgendes ein:

```
fsutil behavior query DisableDeleteNotify
```

Wenn „0“ zurückgegeben wird, ist TRIM ordnungsgemäß aktiviert. Wenn „1“ zurückgegeben wird, führen Sie den folgenden Befehl aus, um TRIM zu aktivieren:
```
fsutil behavior set DisableDeleteNotify 0
```

## <a name="next-steps"></a>Nächste Schritte
Wenn Ihre Anwendung das Laufwerk „D:“ für die Datenspeicherung verwenden muss, können Sie [den Laufwerkbuchstaben des temporären Windows-Datenträgers ändern](../../virtual-machines-windows-change-drive-letter.md).

## <a name="additional-resources"></a>Zusätzliche Ressourcen
[Informationen zu Datenträgern und VHDs für virtuelle Computer](../../virtual-machines-linux-about-disks-vhds.md)

