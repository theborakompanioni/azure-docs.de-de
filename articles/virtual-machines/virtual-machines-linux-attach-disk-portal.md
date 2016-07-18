<properties
	pageTitle="Anfügen eines Datenträgers an eine Linux-VM | Microsoft Azure"
	description="So fügen Sie neue oder vorhandene Datenträgern an eine Linux-VM im Azure-Portal unter Verwendung des Resource Manager-Bereitstellungsmodells an."
	services="virtual-machines-linux"
	documentationCenter=""
	authors="cynthn"
	manager="timlt"
	editor=""
	tags="azure-resource-manager"/>

<tags
	ms.service="virtual-machines-linux"
	ms.workload="infrastructure-services"
	ms.tgt_pltfrm="vm-linux"
	ms.devlang="na"
	ms.topic="article"
	ms.date="07/06/2016"
	ms.author="cynthn"/>

# Anfügen eines Datenträgers an eine Linux-VM im Azure-Portal

In diesem Artikel wird beschrieben, wie Sie über das Azure-Portal neue und vorhandene Datenträger an einen virtuellen Linux-Computer anfügen können. Sie können auch [einen Datenträger an eine Windows-VM im Azure-Portal anfügen](virtual-machines-windows-attach-disk-portal.md) Lesen Sie die folgenden Tipps, bevor Sie mit diesem Vorgang fortfahren:

- Die Größe des virtuellen Computers bestimmt, wie viele Datenträger Sie anfügen können. Ausführliche Informationen finden Sie unter [Größen für virtuelle Computer](virtual-machines-linux-sizes.md).
- Für die Verwendung von Premium-Speicher benötigen Sie einen virtuellen Computer der DS- oder GS-Serie. Sie können Datenträger aus Premium- und aus Standard-Speicherkonten für diese virtuellen Computer verwenden. Premium-Speicher ist in bestimmten Regionen verfügbar. Nähere Informationen finden Sie unter [Premium-Speicher: Hochleistungsspeicher für Workloads auf virtuellen Azure-Computern](../storage/storage-premium-storage.md).
- Bei Datenträgern, die an virtuelle Computer angefügt werden, handelt es sich um VHD-Dateien in einem Azure-Speicherkonto. Weitere Informationen finden Sie unter [Informationen zu Datenträgern und VHDs für virtuelle Computer](virtual-machines-linux-about-disks-vhds.md).
- Neue Datenträger werden von Azure beim Anfügen automatisch erstellt. Sie müssen den Datenträger also nicht manuell erstellen.
- Bei einem bereits vorhandenen Datenträger muss die VHD-Datei in einem Azure-Speicherkonto verfügbar sein. Sie können eine bereits vorhandene VHD verwenden, die noch nicht an einen anderen virtuellen Computer angefügt wurde, oder eine eigene VHD-Datei in das Speicherkonto hochladen.


[AZURE.INCLUDE [virtual-machines-common-attach-disk-portal](../../includes/virtual-machines-common-attach-disk-portal.md)]

## Nächste Schritte

Nachdem der Datenträger hinzugefügt wurde, müssen Sie ihn für die Verwendung vorbereiten. Lesen Sie im Betriebssystem der VM die Informationen unter „How to: Initialize a new data disk in Linux“ („Vorgehensweise: Initialisieren eines neuen Datenträgers unter Linux“) in [diesem Artikel](virtual-machines-linux-classic-attach-disk.md#how-to-initialize-a-new-data-disk-in-linux).

<!---HONumber=AcomDC_0706_2016-->