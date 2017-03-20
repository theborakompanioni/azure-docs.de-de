---
title: "Informationen zu Datenträgern und VHDs für virtuelle Microsoft Azure-Linux-Computer | Microsoft-Dokumentation"
description: "Machen Sie sich mit den Grundlagen von Datenträgern und VHDs für virtuelle Linux-Computer vertraut."
services: storage
documentationcenter: 
author: robinsh
manager: timlt
editor: tysonn
ms.assetid: 7be8dd52-98f7-4187-9b78-55197915bc9b
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/14/2017
ms.author: robinsh
translationtype: Human Translation
ms.sourcegitcommit: e34a2bfbf5f1ae544a729c994d91c485d48bb440
ms.openlocfilehash: 7cd157cd1c5730443a710ac46d923319789aec61
ms.lasthandoff: 03/01/2017


---
# <a name="about-disks-and-vhds-for-azure-linux-vms"></a>Informationen zu Datenträgern und VHDs für Azure-Linux-VMs
Virtuelle Computer in Azure verwenden wie alle anderen Computer auch einen Datenträger, auf dem das Betriebssystem, Anwendungen und Daten gespeichert sind. Alle virtuellen Azure-Computer verfügen über mindestens zwei Datenträger: einen Datenträger mit dem Linux-Betriebssystem und einen temporären Datenträger. Der Betriebssystem-Datenträger wird aus einem Image erstellt. Sowohl der Betriebssystem-Datenträger als auch das Image sind virtuelle Festplatten (VHDs), die im Azure-Speicherkonto gespeichert sind. Virtuelle Computer können auch über einen oder mehrere Datenträger verfügen, die ebenfalls als VHDs gespeichert werden. 

In diesem Artikel werden die unterschiedlichen Verwendungsmöglichkeiten der Datenträger erörtert und die verschiedenen Typen von Datenträgern erläutert, die Sie erstellen und nutzen können. Dieser Artikel ist auch für [virtuelle Windows-Computer](storage-about-disks-and-vhds-windows.md)verfügbar.

[!INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]

## <a name="disks-used-by-vms"></a>Von VMs verwendete Datenträger

Werfen wir einen Blick darauf, wie die Datenträger von den virtuellen Computern verwendet werden.

## <a name="operating-system-disk"></a>Betriebssystem-Datenträger
Jedem virtuellen Computer ist ein Betriebssystem-Datenträger zugeordnet. Dieser ist standardmäßig als SATA-Laufwerk registriert und mit „/dev/sda“ bezeichnet. Dieser Datenträger hat eine maximale Kapazität von 1023 Gigabytes (GB). 

## <a name="temporary-disk"></a>Temporärer Datenträger
Jede VM verfügt über einen temporären Datenträger. Der temporäre Datenträger bietet kurzfristigen Speicher für Anwendungen und Prozesse und ist ausschließlich dafür ausgelegt, Daten wie z.B. Seiten-oder Auslagerungsdateien zu speichern. Daten auf dem temporären Datenträger können während eines [Wartungsereignisses](../virtual-machines/virtual-machines-linux-manage-availability.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json#understand-planned-vs-unplanned-maintenance) verloren gehen, oder wenn Sie [eine VM erneut bereitstellen](../virtual-machines/virtual-machines-linux-redeploy-to-new-node.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). Während eines standardmäßigen Neustarts der VM sollten die Daten auf dem virtuellen Datenträger erhalten bleiben.

Auf virtuellen Linux-Computern lautet der Datenträger in der Regel **/dev/sdb**. Er wird vom Azure Linux-Agent formatiert und in **/mnt/** eingebunden. Die Größe des temporären Datenträgers variiert basierend auf der Größe des virtuellen Computers. Weitere Informationen finden Sie unter [Größen für virtuelle Computer in Azure](../virtual-machines/virtual-machines-linux-sizes.md).

Weitere Informationen zur Verwendung des temporären Datenträgers in Azure finden Sie unter [Understanding the temporary drive on Microsoft Azure Virtual Machines](https://blogs.msdn.microsoft.com/mast/2013/12/06/understanding-the-temporary-drive-on-windows-azure-virtual-machines/)

## <a name="data-disk"></a>Datenträger
Ein Datenträger ist eine VHD, die zum Speichern von Anwendungsdaten oder anderen Daten, die Sie aufbewahren müssen, an einen virtuellen Computer angebunden ist. Datenträger werden als SCSI-Laufwerke registriert und mit einem von Ihnen ausgewählten Buchstaben gekennzeichnet. Jeder Datenträger hat eine maximale Kapazität von 1023 GB. Die Größe des virtuellen Computers bestimmt die Anzahl der Datenträger, die Sie anfügen können, und den Typ des Speichers, den Sie zum Hosten der Datenträger verwenden können.

> [!NOTE]
> Weitere Informationen zu Kapazitäten virtueller Computer finden Sie unter [Größen für virtuelle Computer in Azure](../virtual-machines/virtual-machines-linux-sizes.md).
> 

Azure erstellt einen Betriebssystem-Datenträger, wenn Sie einen virtuellen Computer aus einem Image erstellen. Wenn das verwendete Image Datenträger enthält, erstellt Azure auch diese Datenträger beim Erstellen des virtuellen Computers. Andernfalls fügen Sie die Datenträger nach dem Erstellen des virtuellen Computers hinzu.

Sie können jederzeit Datenträger einem virtuellen Computer hinzufügen, indem Sie den Datenträger dem virtuellen Computer **anfügen** . Sie können eine VHD verwenden, die Sie in das Speicherkonto hochgeladen oder kopiert haben, oder eine VHD, die Azure für Sie erstellt hat. Beim Anfügen eines Datenträgers wird die VHD-Datei durch eine „Lease“ mit dem virtuellen Computer verknüpft, sodass die VHD nicht aus dem Speicher gelöscht werden kann, solange sie angefügt ist.

[!INCLUDE [storage-about-vhds-and-disks-windows-and-linux](../../includes/storage-about-vhds-and-disks-windows-and-linux.md)]

## <a name="troubleshooting"></a>Problembehandlung
[!INCLUDE [virtual-machines-linux-lunzero](../../includes/virtual-machines-linux-lunzero.md)]

## <a name="next-steps"></a>Nächste Schritte
* [Anfügen eines Datenträgers](../virtual-machines/virtual-machines-linux-add-disk.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) , um zusätzlichen Speicherplatz für den virtuellen Computer hinzuzufügen.
* [Konfigurieren von Software-RAID](../virtual-machines/virtual-machines-linux-configure-raid.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) für Redundanz.
* [Erfassen eines virtuellen Linux-Computers](../virtual-machines/virtual-machines-linux-classic-capture-image.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json) für die schnelle Bereitstellung zusätzlicher virtueller Computer.


