---
title: "Informationen zu Datenträgern und VHDs für virtuelle Microsoft Azure-Windows-Computer | Microsoft-Dokumentation"
description: "Machen Sie sich mit den Grundlagen zu Datenträgern und VHDs für virtuelle Windows-Computer in Azure vertraut."
services: storage
documentationcenter: 
author: robinsh
manager: timlt
editor: tysonn
ms.assetid: 0142c64d-5e8c-4d62-aa6f-06d6261f485a
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/15/2017
ms.author: robinsh
ms.translationtype: HT
ms.sourcegitcommit: 398efef3efd6b47c76967563251613381ee547e9
ms.openlocfilehash: 34a4d8fa176484fbadb1b385d794cada5be607c8
ms.contentlocale: de-de
ms.lasthandoff: 08/11/2017

---
# <a name="about-disks-and-vhds-for-azure-windows-vms"></a>Informationen zu Datenträgern und VHDs für Azure-Windows-VMs
Virtuelle Computer in Azure verwenden wie alle anderen Computer auch einen Datenträger, auf dem das Betriebssystem, Anwendungen und Daten gespeichert sind. Alle virtuellen Azure-Computer verfügen über mindestens zwei Datenträger – einen Datenträger mit dem Windows-Betriebssystem und einen temporären Datenträger. Der Betriebssystem-Datenträger wird aus einem Image erstellt. Sowohl der Betriebssystem-Datenträger als auch das Image sind virtuelle Festplatten (VHDs), die in einem Azure-Speicherkonto gespeichert sind. Virtuelle Computer können auch über einen oder mehrere Datenträger verfügen, die ebenfalls als VHDs gespeichert werden. 

In diesem Artikel werden die unterschiedlichen Verwendungsmöglichkeiten der Datenträger erörtert und die verschiedenen Typen von Datenträgern erläutert, die Sie erstellen und nutzen können. Dieser Artikel ist auch für [virtuelle Linux-Computer](storage-about-disks-and-vhds-linux.md)verfügbar.

[!INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]

## <a name="disks-used-by-vms"></a>Von VMs verwendete Datenträger

Werfen wir einen Blick darauf, wie die Datenträger von den virtuellen Computern verwendet werden.

### <a name="operating-system-disk"></a>Betriebssystem-Datenträger
Jedem virtuellen Computer ist ein Betriebssystem-Datenträger zugeordnet. Der Datenträger ist standardmäßig als SATA-Laufwerk registriert und als Laufwerk „C:“ gekennzeichnet. Dieser Datenträger weist eine maximale Kapazität von 2048 Gigabyte (GB). 

### <a name="temporary-disk"></a>Temporärer Datenträger
Jede VM verfügt über einen temporären Datenträger. Der temporäre Datenträger bietet kurzfristigen Speicher für Anwendungen und Prozesse und ist ausschließlich dafür ausgelegt, Daten wie z.B. Seiten-oder Auslagerungsdateien zu speichern. Daten auf dem temporären Datenträger können während eines [Wartungsereignisses](../virtual-machines/windows/manage-availability.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json#understand-vm-reboots---maintenance-vs-downtime) verloren gehen, oder wenn Sie [eine VM erneut bereitstellen](../virtual-machines/windows/redeploy-to-new-node.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). Während eines standardmäßigen Neustarts der VM sollten die Daten auf dem virtuellen Datenträger erhalten bleiben.

Der temporäre Datenträger ist standardmäßig als Laufwerk „D:“ gekennzeichnet. Er wird zum Speichern von „pagefile.sys“ verwendet. Informationen zum erneuten Zuordnen dieses Datenträgers zu einem anderen Laufwerkbuchstaben finden Sie unter [Ändern des Laufwerkbuchstabens des temporären Windows-Datenträgers](../virtual-machines/windows/change-drive-letter.md). Die Größe des temporären Datenträgers variiert basierend auf der Größe des virtuellen Computers. Weitere Informationen finden Sie unter [Größen für virtuelle Computer in Azure](../virtual-machines/windows/sizes.md).

Weitere Informationen zur Verwendung des temporären Datenträgers in Azure finden Sie unter [Understanding the temporary drive on Microsoft Azure Virtual Machines](https://blogs.msdn.microsoft.com/mast/2013/12/06/understanding-the-temporary-drive-on-windows-azure-virtual-machines/)


### <a name="data-disk"></a>Datenträger
Ein Datenträger ist eine VHD, die zum Speichern von Anwendungsdaten oder anderen Daten, die Sie aufbewahren müssen, an einen virtuellen Computer angebunden ist. Datenträger werden als SCSI-Laufwerke registriert und mit einem von Ihnen ausgewählten Buchstaben gekennzeichnet. Jeder Datenträger weist eine maximale Kapazität von 4095 GB auf. Die Größe des virtuellen Computers bestimmt die Anzahl der Datenträger, die Sie anfügen können, und den Typ des Speichers, den Sie zum Hosten der Datenträger verwenden können.

> [!NOTE]
> Weitere Informationen zu Kapazitäten virtueller Computer finden Sie unter [Größen für virtuelle Computer in Azure](../virtual-machines/windows/sizes.md).
> 

Azure erstellt einen Betriebssystem-Datenträger, wenn Sie einen virtuellen Computer aus einem Image erstellen. Wenn das verwendete Image Datenträger enthält, erstellt Azure auch diese Datenträger beim Erstellen des virtuellen Computers. Andernfalls fügen Sie die Datenträger nach dem Erstellen des virtuellen Computers hinzu.

Sie können jederzeit Datenträger einem virtuellen Computer hinzufügen, indem Sie den Datenträger dem virtuellen Computer **anfügen** . Sie können eine VHD verwenden, die Sie in das Speicherkonto hochgeladen oder kopiert haben, oder eine VHD, die Azure für Sie erstellt hat. Beim Anfügen eines Datenträgers wird die VHD-Datei durch eine „Lease“ mit dem virtuellen Computer verknüpft, sodass die VHD nicht aus dem Speicher gelöscht werden kann, solange sie angefügt ist.


[!INCLUDE [storage-about-vhds-and-disks-windows-and-linux](../../includes/storage-about-vhds-and-disks-windows-and-linux.md)]

## <a name="one-last-recommendation-use-trim-with-unmanaged-standard-disks"></a>Eine letzte Empfehlung: Verwenden Sie TRIM mit nicht verwalteten Standarddatenträgern. 

Wenn Sie nicht verwaltete Standarddatenträger (HDD) verwenden, sollten Sie TRIM aktivieren. Mit TRIM werden ungenutzte Blöcke auf dem Datenträger verworfen, sodass nur der Speicher in Rechnung gestellt wird, den Sie tatsächlich verwenden. Dies kann Kosten sparen, wenn Sie große Dateien erstellen und dann löschen. 

Sie können den folgenden Befehl ausführen, um die TRIM-Einstellung zu überprüfen. Öffnen Sie eine Eingabeaufforderung auf dem virtuellen Windows-Computer, und geben Sie Folgendes ein:


```
fsutil behavior query DisableDeleteNotify
```

Wenn „0“ zurückgegeben wird, ist TRIM ordnungsgemäß aktiviert. Wenn „1“ zurückgegeben wird, führen Sie den folgenden Befehl aus, um TRIM zu aktivieren:

```
fsutil behavior set DisableDeleteNotify 0
```

> [!NOTE]
> Hinweis: Unterstützung für TRIM ist ab Windows Server 2012/Windows 8 verfügbar. Weitere Informationen finden Sie unter [Neue API ermöglicht Apps das Senden von Hinweisen zu „TRIM und Aufheben der Zuordnung“ an Speichermedien](https://msdn.microsoft.com/windows/compatibility/new-api-allows-apps-to-send-trim-and-unmap-hints).
> 

<!-- Might want to match next-steps from overview of managed disks -->
## <a name="next-steps"></a>Nächste Schritte
* [Anfügen eines Datenträgers](../virtual-machines/windows/attach-managed-disk-portal.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) , um zusätzlichen Speicherplatz für den virtuellen Computer hinzuzufügen.
* [Ändern des Laufwerkbuchstabens des temporären Windows-Datenträgers](../virtual-machines/windows/change-drive-letter.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json) , damit die Anwendung das Laufwerk „D:“ für Daten verwenden kann.


