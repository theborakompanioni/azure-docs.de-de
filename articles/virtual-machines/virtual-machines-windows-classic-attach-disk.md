---
title: Anfügen eines Datenträgers an einen virtuellen Computer | Microsoft Docs
description: Fügen Sie einen Datenträger an einen virtuellen Windows-Computer an, der mit dem klassischen Bereitstellungsmodell erstellt wurde, und initialisieren Sie ihn.
services: virtual-machines-windows, storage
documentationcenter: ''
author: cynthn
manager: timlt
editor: tysonn
tags: azure-service-management

ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 06/27/2016
ms.author: cynthn

---
# <a name="attach-a-data-disk-to-a-windows-virtual-machine-created-with-the-classic-deployment-model"></a>Anfügen eines Datenträgers an einen virtuellen Windows-Computer, der mit dem klassischen Bereitstellungsmodell erstellt wurde.
[!INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]

Wenn Sie das neue Portal verwenden möchten, lesen Sie die Informationen unter [Anfügen eines Datenträgers an eine Windows-VM im Azure-Portal](virtual-machines-windows-attach-disk-portal.md).

Wenn Sie einen zusätzlichen Datenträger benötigen, können Sie einen leeren Datenträger oder einen vorhandenen Datenträger mit Daten an einen virtuellen Computer anfügen. In beiden Fällen sind die Datenträger VHD-Dateien, die sich in einem Azure-Speicherkonto befinden. Einen neuen Datenträger müssen Sie nach dem Anfügen außerdem noch initialisieren, damit er durch einen virtuellen Windows-Computer genutzt werden kann.

Weitere Detailinformationen zu Datenträgern finden Sie unter [Informationen zu Datenträgern und VHDs für virtuelle Computer](virtual-machines-windows-about-disks-vhds.md).

[!INCLUDE [howto-attach-disk-windows-linux](../../includes/howto-attach-disk-windows-linux.md)]

## <a name="initialize-the-disk"></a>Datenträger initialisieren
1. Stellen Sie eine Verbindung mit dem virtuellen Computer her. Weitere Informationen finden Sie unter [Anmelden bei einem virtuellen Computer, auf dem Windows Server ausgeführt wird][logon].
2. Öffnen Sie den **Server-Manager**, nachdem Sie sich auf dem virtuellen Computer angemeldet haben. Klicken Sie im linken Bereich auf **Datei- und Speicherdienste**.
   
    ![Server-Manager öffnen](./media/virtual-machines-windows-classic-attach-disk/fileandstorageservices.png)
3. Erweitern Sie das Menü, und wählen Sie **Datenträger**.
4. Im Abschnitt **Datenträger** werden die Datenträger aufgelistet. In den meisten Fällen sind darin Datenträger 0, Datenträger 1 und Datenträger 2 enthalten. Datenträger 0 ist der Betriebssystemdatenträger. Bei Datenträger 1 handelt es sich um den temporären Datenträger, und Datenträger 2 ist der Datenträger, den Sie gerade an den virtuellen Computer angefügt haben. Für den neuen Datenträger wird die Partition als **Unbekannt** aufgelistet. Klicken Sie mit der rechten Maustaste auf den Datenträger, und wählen Sie **Initialisieren** aus.
5. Sie werden darüber informiert, dass alle Daten gelöscht werden, wenn der Datenträger initialisiert wird. Klicken Sie auf **Ja** , um die Warnung zu bestätigen und den Datenträger zu initialisieren. Wenn dies abgeschlossen ist, wird die Partition als **GPT**aufgelistet. Klicken Sie erneut mit der rechten Maustaste auf den Datenträger, und wählen Sie **Neues Volume**.
6. Schließen Sie den Assistenten unter Verwendung der angegebenen Standardwerte ab. Sobald der Assistent fertig ist, wird ein neues Volume im Bereich **Volumes** aufgelistet. Der Datenträger ist jetzt online und zum Speichern von Daten bereit.
   
   ![Volume erfolgreich initialisiert](./media/virtual-machines-windows-classic-attach-disk/newvolumecreated.png)

> [!NOTE]
> Die Größe des virtuellen Computers bestimmt, wie viele Datenträger Sie anfügen können. Ausführliche Informationen finden Sie unter [Größen für virtuelle Computer](virtual-machines-linux-sizes.md).
> 
> 

## <a name="additional-resources"></a>Zusätzliche Ressourcen
[Trennen eines Datenträgers von einem virtuellen Computer unter Windows](virtual-machines-windows-classic-detach-disk.md)

[Informationen zu Datenträgern und VHDs für virtuelle Computer](virtual-machines-linux-about-disks-vhds.md)

[Anmeldung]: virtual-machines-windows-classic-connect-logon.md



<!--HONumber=Oct16_HO2-->


