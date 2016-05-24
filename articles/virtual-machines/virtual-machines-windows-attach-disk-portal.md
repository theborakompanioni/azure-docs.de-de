<properties
	pageTitle="Anfügen eines Datenträgers an einen virtuellen Windows-Computer | Microsoft Azure"
	description="Hier erhalten Sie Informationen zum Anfügen von neuen oder vorhandenen Datenträgern an einen virtuellen Windows-Computer im Azure-Portal mit dem Resource Manager-Bereitstellungsmodell."
	services="virtual-machines-windows"
	documentationCenter=""
	authors="cynthn"
	manager="timlt"
	editor=""
	tags="azure-resource-manager"/>

<tags
	ms.service="virtual-machines-windows"
	ms.workload="infrastructure-services"
	ms.tgt_pltfrm="vm-windows"
	ms.devlang="na"
	ms.topic="article"
	ms.date="05/09/2016"
	ms.author="cynthn"/>

# Anfügen eines Datenträgers an eine Windows-VM im Azure-Portal

In diesem Artikel wird beschrieben, wie Sie über das Azure-Portal neue und vorhandene Datenträger an einen virtuellen Windows-Computer anfügen können. Sie können auch [einen Datenträger an eine Linux-VM im Azure-Portal anfügen](virtual-machines-linux-attach-disk-portal.md). Lesen Sie die folgenden Tipps, bevor Sie mit diesem Vorgang fortfahren:

- Die Größe des virtuellen Computers bestimmt, wie viele Datenträger Sie anfügen können. Ausführliche Informationen finden Sie unter [Größen für virtuelle Computer](virtual-machines-windows-sizes.md).
- Für die Verwendung von Premium-Speicher benötigen Sie einen virtuellen Computer der DS- oder GS-Serie. Sie können Datenträger aus Premium- und aus Standard-Speicherkonten für diese virtuellen Computer verwenden. Premium-Speicher ist in bestimmten Regionen verfügbar. Nähere Informationen finden Sie unter [Premium-Speicher: Hochleistungsspeicher für Workloads auf virtuellen Azure-Computern](../storage/storage-premium-storage.md).
- Bei Datenträgern, die an virtuelle Computer angefügt werden, handelt es sich um VHD-Dateien in einem Azure-Speicherkonto. Weitere Informationen finden Sie unter [Informationen zu Datenträgern und VHDs für virtuelle Computer](virtual-machines-windows-about-disks-vhds.md).
- Neue Datenträger werden von Azure beim Anfügen automatisch erstellt. Sie müssen den Datenträger also nicht manuell erstellen.
- Bei einem bereits vorhandenen Datenträger muss die VHD-Datei in einem Azure-Speicherkonto verfügbar sein. Sie können eine bereits vorhandene VHD verwenden, die noch nicht an einen anderen virtuellen Computer angefügt wurde, oder eine eigene VHD-Datei in das Speicherkonto hochladen.

[AZURE.INCLUDE [virtual-machines-common-attach-disk-portal](../../includes/virtual-machines-common-attach-disk-portal.md)]

## <a id="initializeinWS"></a>Gewusst wie: Initialisieren eines neuen Datenträgers unter Windows Server

1. Stellen Sie eine Verbindung mit dem virtuellen Computer her. Anweisungen dazu finden Sie unter [Gewusst wie: Herstellen einer Verbindung mit einem virtuellen Azure-Computer unter Windows und Anmelden auf diesem Computer](virtual-machines-windows-connect-logon.md).

2. Öffnen Sie den **Server-Manager**, nachdem Sie sich auf dem virtuellen Computer angemeldet haben. Klicken Sie im linken Bereich auf **Datei- und Speicherdienste**.

	![Server-Manager öffnen](./media/virtual-machines-windows-classic-attach-disk/fileandstorageservices.png)

3. Erweitern Sie das Menü, und wählen Sie **Datenträger**.

4. Im Abschnitt **Datenträger** werden die Datenträger aufgelistet. In den meisten Fällen sind darin Datenträger 0, Datenträger 1 und Datenträger 2 enthalten. Datenträger 0 ist der Betriebssystemdatenträger. Bei Datenträger 1 handelt es sich um den temporären Datenträger, und Datenträger 2 ist der Datenträger, den Sie gerade an den virtuellen Computer angefügt haben. Für den neuen Datenträger wird die Partition als **Unbekannt** aufgelistet. Klicken Sie mit der rechten Maustaste auf den Datenträger, und wählen Sie **Initialisieren** aus.

5.	Sie werden darüber informiert, dass alle Daten gelöscht werden, wenn der Datenträger initialisiert wird. Klicken Sie auf **Ja**, um die Warnung zu bestätigen und den Datenträger zu initialisieren. Wenn dies abgeschlossen ist, wird die Partition als **GPT** aufgelistet. Klicken Sie erneut mit der rechten Maustaste auf den Datenträger, und wählen Sie **Neues Volume** aus.

6.	Schließen Sie den Assistenten unter Verwendung der angegebenen Standardwerte ab. Sobald der Assistent fertig ist, wird ein neues Volume im Bereich **Volumes** aufgelistet. Der Datenträger ist jetzt online und zum Speichern von Daten bereit.


	![Volume erfolgreich initialisiert](./media/virtual-machines-windows-classic-attach-disk/newvolumecreated.png)

> [AZURE.NOTE] Die Größe des virtuellen Computers bestimmt, wie viele Datenträger Sie anfügen können. Ausführliche Informationen finden Sie unter [Größen für virtuelle Computer](virtual-machines-linux-sizes.md).


## Nächste Schritte

Wenn Ihre Anwendung das Laufwerk „D:“ für die Datenspeicherung verwenden muss, können Sie [den Laufwerkbuchstaben des temporären Windows-Datenträgers ändern](virtual-machines-windows-classic-change-drive-letter.md).

<!---HONumber=AcomDC_0511_2016-->