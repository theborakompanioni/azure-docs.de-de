---
title: "Anfügen eines nicht verwalteten Datenträgers an einen virtuellen Windows-Computer – Azure | Microsoft-Dokumentation"
description: "Hier erhalten Sie Informationen zum Anfügen eines neuen oder vorhandenen nicht verwalteten Datenträgers an einen virtuellen Windows-Computer im Azure-Portal mit dem Resource Manager-Bereitstellungsmodell."
services: virtual-machines-windows
documentationcenter: 
author: cynthn
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 3790fc59-7264-41df-b7a3-8d1226799885
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 05/09/2017
ms.author: cynthn
ms.translationtype: Human Translation
ms.sourcegitcommit: 5e92b1b234e4ceea5e0dd5d09ab3203c4a86f633
ms.openlocfilehash: d02f92a8809efd6f58312af8cb40739299ea28f6
ms.contentlocale: de-de
ms.lasthandoff: 05/10/2017


---
# <a name="how-to-attach-an-unmanaged-data-disk-to-a-windows-vm-in-the-azure-portal"></a>Anfügen eines nicht verwalteten Datenträgers an eine Windows-VM im Azure-Portal

In diesem Artikel wird beschrieben, wie Sie über das Azure-Portal neue und vorhandene nicht verwaltete Datenträger an einen virtuellen Windows-Computer anfügen können. Sie können auch [einen Datenträger mithilfe von PowerShell anfügen](./attach-disk-ps.md). Lesen Sie die folgenden Tipps, bevor Sie mit diesem Vorgang fortfahren:

* Die Größe des virtuellen Computers bestimmt, wie viele Datenträger Sie anfügen können. Ausführliche Informationen finden Sie unter [Größen für virtuelle Computer](sizes.md).
* Für die Verwendung von Premium-Speicher benötigen Sie einen virtuellen Computer der DS- oder GS-Serie. Sie können Datenträger aus Premium- und aus Standard-Speicherkonten für diese virtuellen Computer verwenden. Premium-Speicher ist in bestimmten Regionen verfügbar. Nähere Informationen finden Sie unter [Premium-Speicher: Hochleistungsspeicher für Workloads auf virtuellen Azure-Computern](../../storage/storage-premium-storage.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
* Neue Datenträger werden von Azure beim Anfügen automatisch erstellt. Sie müssen den Datenträger also nicht manuell erstellen.


Sie können auch [einen Datenträger mithilfe von PowerShell anfügen](attach-disk-ps.md).


## <a name="find-the-virtual-machine"></a>Suchen des virtuellen Computers
1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com/)an.
2. Klicken Sie im Menü auf der linken Seite auf **Virtuelle Computer**.
3. Wählen Sie den gewünschten virtuellen Computer aus der Liste aus.
4. Klicken Sie auf dem Blatt „Virtuelle Computer“ auf **Datenträger**.
   
Folgen Sie den Anweisungen zum Anfügen eines [neuen](#option-1-attach-a-new-disk) oder eines [vorhandenen](#option-2-attach-an-existing-disk) Datenträgers.

## <a name="option-1-attach-and-initialize-a-new-disk"></a>Option 1: Anfügen und Initialisieren eines neuen Datenträgers
1. Klicken Sie auf dem Blatt **Datenträger** auf **+ Datenträger anfügen**.
2. Geben Sie auf dem Blatt **Verwalteten Datenträger anfügen** in **Name** einen Namen für den Datenträger ein, und wählen Sie in **Quelltyp** dann **Neu (leerer Datenträger)** aus.
3. Klicken Sie unter **Speichercontainer** auf die Schaltfläche **Durchsuchen**, und wechseln Sie zum Speicherkonto und Container, in dem die neue virtuelle Festplatte gespeichert werden soll. Klicken Sie dann auf **Auswählen**. 
  
   ![Überprüfen der Datenträgereinstellungen](./media/attach-disk-portal/attach-empty-unmanaged.png)
   
3. Wenn Sie mit den Einstellungen für den Datenträger fertig sind, klicken Sie auf **OK**.
4. Zurück auf dem Blatt **Datenträger** klicken Sie auf **Speichern**, um den Datenträger der Konfiguration des virtuellen Computers hinzuzufügen.


### <a name="initialize-a-new-data-disk"></a>Initialisieren eines neuen Datenträgers

1. Stellen Sie eine Verbindung mit dem virtuellen Computer her. Anweisungen dazu finden Sie unter [Gewusst wie: Herstellen einer Verbindung mit einem virtuellen Azure-Computer unter Windows und Anmelden auf diesem Computer](connect-logon.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
1. Klicken Sie im virtuellen Computer auf **Start**, geben Sie **diskmgmt.msc** ein, und drücken Sie die **EINGABETASTE**. Dadurch wird das Snap-In „Datenträgerverwaltung“ gestartet.
2. Die Datenträgerverwaltung erkennt, dass es einen neuen nicht initialisierten Datenträger gibt, weshalb das Fenster „Datenträger initialisieren“ eingeblendet wird.
3. Stellen Sie sicher, dass der neue Datenträger ausgewählt ist, und klicken Sie auf **OK**, um ihn zu initialisieren.
4. Der neue Datenträger wird jetzt als **Nicht zugeordnet** angezeigt. Klicken Sie mit der rechten Maustaste auf den Datenträger, und wählen **Neues einfaches Volume** aus. Der **Assistent zum Erstellen neuer einfacher Volumes** wird gestartet.
5. Durchlaufen Sie den Assistenten, und behalten alle Standardeinstellungen bei. Klicken Sie abschließend auf **Fertig stellen**.
6. Schließen Sie die Datenträgerverwaltung.
7. In einem Popupfenster werden Sie informiert, dass Sie den neuen Datenträger formatieren müssen, ehe Sie ihn verwenden können. Klicken Sie auf **Datenträger formatieren**.
8. Überprüfen Sie im Dialogfeld **Neuen Datenträger formatieren** die Einstellungen, und klicken Sie dann auf **Starten**.
9. Sie erhalten die Warnung, dass bei Formatierung des Datenträgers alle Daten gelöscht werden. Klicken Sie auf **OK**.
10. Wenn die Formatierung abgeschlossen ist, klicken Sie auf **OK**.


## <a name="option-2-attach-an-existing-disk"></a>Option 2: Anfügen eines vorhandenen Datenträgers
1. Klicken Sie auf dem Blatt **Datenträger** auf **+ Datenträger anfügen**.
2. Klicken Sie auf dem Blatt **Nicht verwalteten Datenträger anfügen** in **Quelltyp** auf **Vorhandenes Blob**.

    ![Überprüfen der Datenträgereinstellungen](./media/attach-disk-portal/attach-existing-unmanaged.png)

    3. Klicken Sie auf **Durchsuchen**, und navigieren Sie zum Speicherkonto und Container, in dem sich die vorhandene virtuellen Festplatte befindet. Klicken Sie auf die virtuelle Festplatte und dann auf **Auswählen**.
4. Klicken Sie auf dem Blatt **Nicht verwalteten Datenträger anfügen** auf **OK**.
5. Zurück auf dem Blatt **Datenträger** klicken Sie auf **Speichern**, um den Datenträger der Konfiguration des virtuellen Computers hinzuzufügen.
   


## <a name="use-trim-with-standard-storage"></a>Verwenden von TRIM mit dem Standardspeicher

Wenn Sie den Standardspeicher (HDD) verwenden, sollten Sie TRIM aktivieren. Mit TRIM werden ungenutzte Blöcke auf dem Datenträger verworfen, sodass nur der Speicher in Rechnung gestellt wird, den Sie tatsächlich verwenden. Dies kann Kosten sparen, wenn Sie große Dateien erstellen und dann löschen. 

Sie können den folgenden Befehl ausführen, um die TRIM-Einstellung zu überprüfen. Öffnen Sie eine Eingabeaufforderung auf dem virtuellen Windows-Computer, und geben Sie Folgendes ein:

```
fsutil behavior query DisableDeleteNotify
```

Wenn „0“ zurückgegeben wird, ist TRIM ordnungsgemäß aktiviert. Wenn „1“ zurückgegeben wird, führen Sie den folgenden Befehl aus, um TRIM zu aktivieren:
```
fsutil behavior set DisableDeleteNotify 0
```
                
Nach dem Löschen von Daten vom Datenträger können Sie sicherstellen, dass die TRIM-Vorgänge ordnungsgemäß geleert werden, indem Sie die Defragmentierung mit TRIM ausführen:

```
defrag.exe <volume:> -l
```

## <a name="next-steps"></a>Nächste Schritte
Wenn Ihre Anwendung das Laufwerk „D:“ für die Datenspeicherung verwenden muss, können Sie [den Laufwerkbuchstaben des temporären Windows-Datenträgers ändern](change-drive-letter.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).


