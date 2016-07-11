<properties
	pageTitle="Anfügen eines Datenträgers an einen virtuellen Computer | Microsoft Azure"
	description="Fügen Sie einen Datenträger an einen virtuellen Windows-Computer an, der mit dem klassischen Bereitstellungsmodell erstellt wurde, und initialisieren Sie ihn."
	services="virtual-machines-windows, storage"
	documentationCenter=""
	authors="cynthn"
	manager="timlt"
	editor="tysonn"
	tags="azure-service-management"/>

<tags
	ms.service="virtual-machines-windows"
	ms.workload="infrastructure-services"
	ms.tgt_pltfrm="vm-windows"
	ms.devlang="na"
	ms.topic="article"
	ms.date="05/17/2016"
	ms.author="cynthn"/>

# Anfügen eines Datenträgers an einen virtuellen Windows-Computer, der mit dem klassischen Bereitstellungsmodell erstellt wurde.

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)] Erfahren Sie, wie Sie [diese Schritte mit dem Resource Manager-Modell ausführen](virtual-machines-windows-attach-disk-portal.md).

Wenn Sie einen zusätzlichen Datenträger benötigen, können Sie einen leeren Datenträger oder einen vorhandenen Datenträger mit Daten an einen virtuellen Computer anfügen. In beiden Fällen sind die Datenträger VHD-Dateien, die sich in einem Azure-Speicherkonto befinden. Einen neuen Datenträger müssen Sie nach dem Anfügen außerdem noch initialisieren, damit er durch einen virtuellen Windows-Computer genutzt werden kann.

Weitere Detailinformationen zu Datenträgern finden Sie unter [Informationen zu Datenträgern und VHDs für virtuelle Computer](virtual-machines-windows-about-disks-vhds.md).

## Exemplarische Vorgehensweise per Video

Hier finden Sie ein [Video mit einer exemplarischen Vorgehensweise](https://azure.microsoft.com/documentation/videos/attaching-a-data-disk-to-a-windows-vm/) für die Schritte dieses Tutorials.


[AZURE.INCLUDE [howto-attach-disk-windows-linux](../../includes/howto-attach-disk-windows-linux.md)]

## Datenträger initialisieren

1. Stellen Sie eine Verbindung mit dem virtuellen Computer her. Weitere Informationen finden Sie unter [Anmelden bei einem virtuellen Computer, auf dem Windows Server ausgeführt wird][logon].

2. Öffnen Sie den **Server-Manager**, nachdem Sie sich auf dem virtuellen Computer angemeldet haben. Klicken Sie im linken Bereich auf **Datei- und Speicherdienste**.

	![Server-Manager öffnen](./media/virtual-machines-windows-classic-attach-disk/fileandstorageservices.png)

3. Erweitern Sie das Menü, und wählen Sie **Datenträger**.

4. Im Abschnitt **Datenträger** werden die Datenträger aufgelistet. In den meisten Fällen sind darin Datenträger 0, Datenträger 1 und Datenträger 2 enthalten. Datenträger 0 ist der Betriebssystemdatenträger. Bei Datenträger 1 handelt es sich um den temporären Datenträger, und Datenträger 2 ist der Datenträger, den Sie gerade an den virtuellen Computer angefügt haben. Für den neuen Datenträger wird die Partition als **Unbekannt** aufgelistet. Klicken Sie mit der rechten Maustaste auf den Datenträger, und wählen Sie **Initialisieren** aus.

5.	Sie werden darüber informiert, dass alle Daten gelöscht werden, wenn der Datenträger initialisiert wird. Klicken Sie auf **Ja**, um die Warnung zu bestätigen und den Datenträger zu initialisieren. Wenn dies abgeschlossen ist, wird die Partition als **GPT** aufgelistet. Klicken Sie erneut mit der rechten Maustaste auf den Datenträger, und wählen Sie **Neues Volume** aus.

6.	Schließen Sie den Assistenten unter Verwendung der angegebenen Standardwerte ab. Sobald der Assistent fertig ist, wird ein neues Volume im Bereich **Volumes** aufgelistet. Der Datenträger ist jetzt online und zum Speichern von Daten bereit.

	![Volume erfolgreich initialisiert](./media/virtual-machines-windows-classic-attach-disk/newvolumecreated.png)

> [AZURE.NOTE] Die Größe des virtuellen Computers bestimmt, wie viele Datenträger Sie anfügen können. Ausführliche Informationen finden Sie unter [Größen für virtuelle Computer](virtual-machines-linux-sizes.md).

## Zusätzliche Ressourcen

[Trennen eines Datenträgers von einem virtuellen Computer unter Windows](virtual-machines-windows-classic-detach-disk.md)

[Informationen zu Datenträgern und VHDs für virtuelle Computer](virtual-machines-linux-about-disks-vhds.md)

[logon]: virtual-machines-windows-classic-connect-logon.md

<!---HONumber=AcomDC_0629_2016-->