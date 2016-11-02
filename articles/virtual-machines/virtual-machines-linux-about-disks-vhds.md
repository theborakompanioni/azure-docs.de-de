<properties
	pageTitle="Informationen zu Datenträgern und VHDs für virtuelle Linux-Computer | Microsoft Azure"
	description="Machen Sie sich mit den Grundlagen von Datenträgern und VHDs für virtuelle Linux-Computer vertraut."
	services="virtual-machines-linux"
	documentationCenter=""
	authors="cynthn"
	manager="timlt"
	editor="tysonn"
	tags="azure-resource-manager,azure-service-management"/>

<tags
	ms.service="virtual-machines-linux"
	ms.workload="infrastructure-services"
	ms.tgt_pltfrm="vm-linux"
	ms.devlang="na"
	ms.topic="article"
	ms.date="06/16/2016"
	ms.author="cynthn"/>

# Informationen zu Datenträgern und VHDs für virtuelle Azure-Computer

Virtuelle Computer in Azure verwenden wie alle anderen Computer auch einen Datenträger, auf dem das Betriebssystem, Anwendungen und Daten gespeichert sind. Alle virtuellen Azure-Computer verfügen über mindestens zwei Datenträger – einen Datenträger mit dem Linux-Betriebssystem (im Fall eines virtuellen Linux-Computers) und einen temporären Datenträger. Der Betriebssystem-Datenträger wird aus einem Image erstellt. Sowohl der Betriebssystem-Datenträger als auch das Image sind virtuelle Festplatten (VHDs), die im Azure-Speicherkonto gespeichert sind. Virtuelle Computer können auch über einen oder mehrere Datenträger verfügen, die ebenfalls als VHDs gespeichert werden. Dieser Artikel ist auch für [virtuelle Windows-Computer](virtual-machines-windows-about-disks-vhds.md) verfügbar.

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]

> [AZURE.NOTE] Falls Sie einen Moment Zeit haben, würden wir uns freuen, wenn Sie an dieser [kurzen Umfrage](https://aka.ms/linuxdocsurvey) teilnehmen könnten, um zur Verbesserung der Dokumentation für virtuelle Azure-Computer unter Linux beizutragen. Jede Antwort hilft uns dabei, Sie noch besser bei Ihrer Arbeit zu unterstützen.

## Betriebssystem-Datenträger

Jedem virtuellen Computer ist ein Betriebssystem-Datenträger zugeordnet. Der Datenträger ist standardmäßig als SATA-Laufwerk registriert und als Laufwerk „C:“ gekennzeichnet. Dieser Datenträger hat eine maximale Kapazität von 1023 Gigabytes (GB).

##Temporärer Datenträger

Der temporäre Datenträger wird automatisch für Sie erstellt. Auf virtuellen Linux-Computern lautet der Datenträger in der Regel "/dev/sdb". Er wird vom Azure Linux Agent formatiert und an /mnt/resource angefügt.

Die Größe des temporären Datenträgers variiert basierend auf der Größe des virtuellen Computers. Weitere Informationen finden Sie unter [Größen für virtuelle Computer in Azure](virtual-machines-linux-sizes.md).

>[AZURE.WARNING] Speichern Sie keine Daten auf dem temporären Datenträger. Er dient als temporärer Speicher für Anwendungen und Prozesse und ist ausschließlich dafür ausgelegt, Daten wie z. B. Seiten-oder Auslagerungsdateien zu speichern.

Weitere Informationen zur Verwendung des temporären Datenträgers in Azure finden Sie unter [Understanding the temporary drive on Microsoft Azure Virtual Machines](https://blogs.msdn.microsoft.com/mast/2013/12/06/understanding-the-temporary-drive-on-windows-azure-virtual-machines/) (Grundlegendes zum temporären Laufwerk in Microsoft Azure Virtual Machines).

## Datenträger

Ein Datenträger ist eine VHD, die zum Speichern von Anwendungsdaten oder anderen Daten, die Sie aufbewahren müssen, an einen virtuellen Computer angebunden ist. Datenträger werden als SCSI-Laufwerke registriert und mit einem von Ihnen ausgewählten Buchstaben gekennzeichnet. Jeder Datenträger hat eine maximale Kapazität von 1023 GB. Die Größe des virtuellen Computers bestimmt die Anzahl der Datenträger, die Sie anfügen können, und den Typ des Speichers, den Sie zum Hosten der Datenträger verwenden können.

>[AZURE.NOTE] Weitere Informationen zu Kapazitäten virtueller Computer finden Sie unter [Größen für virtuelle Computer in Azure](virtual-machines-linux-sizes.md).

Azure erstellt einen Betriebssystem-Datenträger, wenn Sie einen virtuellen Computer aus einem Image erstellen. Wenn das verwendete Image Datenträger enthält, erstellt Azure auch diese Datenträger beim Erstellen des virtuellen Computers. Andernfalls fügen Sie die Datenträger nach dem Erstellen des virtuellen Computers hinzu.

Sie können jederzeit Datenträger einem virtuellen Computer hinzufügen, indem Sie den Datenträger dem virtuellen Computer **anfügen**. Sie können eine VHD verwenden, die Sie in das Speicherkonto hochgeladen oder kopiert haben, oder eine VHD, die Azure für Sie erstellt hat. Beim Anfügen eines Datenträgers wird die VHD-Datei aus dem Speicherkonto durch eine „Lease“ auf der VHD mit dem virtuellen Computer verknüpft, sodass die VHD nicht aus dem Speicher gelöscht werden kann, solange sie angefügt ist.

## Informationen zu VHDs

Die in Azure verwendeten VHDs sind VHD-Dateien, die als Seiten-Blobs in einem Standard- oder Premium-Speicherkonto in Azure gespeichert sind. Informationen zu Seitenblobs finden Sie unter [Grundlegendes zu Blockblobs und Seitenblobs](https://msdn.microsoft.com/library/ee691964.aspx). Informationen zu Premium-Speicher finden Sie unter [Premium-Speicher: Hochleistungsspeicher für Workloads virtueller Azure-Computer](../storage/storage-premium-storage.md).

Azure unterstützt das VHD-Format mit eingebauten („festen“) Datenträgern. Das feste Format legt den logischen Datenträger in der Datei linear aus, daher wird Datenträger-Offset X bei Blob-Offset X gespeichert. Eine kleinere Fußzeile am Ende des Blobs beschreibt die Eigenschaften der VHD. Oftmals verschwendet das feste Format Speicherplatz, da die meisten Datenträger über große ungenutzte Bereiche davon verfügen. Azure speichert VHD-Dateien jedoch in einem platzsparenden Format. Daher profitieren Sie gleichzeitig von den Vorteilen fester und dynamischer Datenträger. Nähere Informationen finden Sie unter [Erste Schritte mit virtuellen Festplatten](https://technet.microsoft.com/library/dd979539.aspx).

Alle VHD-Dateien in Azure, die Sie als Quelle zum Erstellen von Datenträgern oder Images verwenden möchten, sind schreibgeschützt. Beim Erstellen eines Datenträgers oder Images werden Kopien der VHD-Dateien von Azure erstellt. Diese Kopien sind je nach Verwendung der VHD entweder schreibgeschützt oder ermöglichen Lese- und Schreibvorgänge.

Beim Erstellen eines virtuellen Computers mithilfe eines Images, erstellt Azure einen Datenträger für den virtuellen Computer, bei dem es sich um eine Kopie der VHD-Quelldatei handelt. Um ein versehentliches Löschen zu vermeiden, setzt Azure eine Lease für alle VHD-Quelldateien, die zum Erstellen eines Images, Betriebssystem-Datenträgers oder Datenträgers für Daten verwendet wird.

Um eine VHD-Quelldatei löschen zu können, müssen Sie die Lease entfernen, indem Sie den Datenträger oder das Image löschen. Um eine VHD-Datei zu löschen, die von einem virtuellen Computer als Betriebssystem-Datenträger verwendet wird, können Sie den virtuellen Computer, den Betriebssystem-Datenträger und die VHD-Quelldatei auf einmal entfernen, indem Sie den virtuellen Computer und alle verbundenen Datenträger löschen. Zum Entfernen einer VHD-Datei, die als Quelle für einen Datenträger verwendet wird, sind jedoch mehrere Schritte in einer bestimmten Reihenfolge erforderlich: Trennen Sie den Datenträger vom virtuellen Computer, löschen Sie den Datenträger und dann die VHD-Datei.

>[AZURE.WARNING] Wenn Sie eine VHD-Quelldatei aus dem Speicher löschen oder das Speicherkonto löschen, kann Microsoft diese Daten nicht wiederherstellen.


## Problembehandlung
[AZURE.INCLUDE [virtual-machines-linux-lunzero](../../includes/virtual-machines-linux-lunzero.md)]

## Nächste Schritte

-  [Anfügen eines Datenträgers](virtual-machines-linux-add-disk.md), um zusätzlichen Speicherplatz für den virtuellen Computer hinzuzufügen.
-  [Konfigurieren von Software-RAID](virtual-machines-linux-configure-raid.md) für Redundanz
-  [Erfassen eines virtuellen Linux-Computers](virtual-machines-linux-classic-capture-image.md) für die schnelle Bereitstellung zusätzlicher virtueller Computer

<!---HONumber=AcomDC_0914_2016-->