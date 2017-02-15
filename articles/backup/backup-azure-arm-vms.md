---
title: Sichern von virtuellen Azure-Computern in einem Recovery Services-Tresor | Microsoft Docs
description: "Ermitteln, registrieren und sichern Sie Ihre virtuellen Azure-Computer mithilfe dieser Verfahren für das Sichern virtueller Azure-Computer in einem Recovery Services-Tresor."
services: backup
documentationcenter: 
author: markgalioto
manager: cfreeman
editor: 
keywords: Sicherung virtueller Computer; Sichern virtueller Computer; Sicherung und Notfallwiederherstellung; ARM-VM-Sicherung
ms.assetid: 5c68481d-7be3-4e68-b87c-0961c267053e
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/11/2016
ms.author: trinadhk; jimpark; markgal;
translationtype: Human Translation
ms.sourcegitcommit: ac8df40db8ddcc84a0a6221dddd0f17fecbe6586
ms.openlocfilehash: e80d4fdb6f189bf46096422602508b0827f41a67


---
# <a name="back-up-azure-vms-to-a-recovery-services-vault"></a>Sichern von virtuellen Azure-Computern in einem Recovery Services-Tresor
> [!div class="op_single_selector"]
> * [Sichern virtueller Azure Resource Manager-Computer (ARM)](backup-azure-arm-vms.md)
> * [Sichern von virtuellen Azure-Computern](backup-azure-vms.md)
>
>

Dieser Artikel beschreibt das Verfahren zum Sichern virtueller (mit Resource Manager oder dem klassischen Modell bereitgestellten) Azure-Computer in einem Recovery Services-Tresor. Beim Sichern von virtuellen Computern macht die Vorbereitung die meiste Arbeit. Bevor Sie einen virtuellen Computer sichern oder schützen können, müssen Sie die [Voraussetzungen](backup-azure-arm-vms-prepare.md) zum Vorbereiten der Umgebung auf den Schutz Ihrer VMs schaffen. Wenn Sie die Voraussetzungen geschaffen haben, können Sie den Sicherungsvorgang starten, um Momentaufnahmen Ihres virtuellen Computers zu erstellen.


[!INCLUDE [learn about backup deployment models](../../includes/backup-deployment-models.md)]

Weitere Informationen finden Sie in den Artikeln [Planen der Sicherungsinfrastruktur für virtuelle Computer in Azure](backup-azure-vms-introduction.md) und [Dokumentation zu virtuellen Computern](https://azure.microsoft.com/documentation/services/virtual-machines/).

## <a name="triggering-the-backup-job"></a>Auslösen des Sicherungsauftrags
Die Sicherungsrichtlinie des Recovery Services-Tresors definiert, wie oft und wann der Sicherungsvorgang ausgeführt wird. Standardmäßig ist die erste geplante Sicherung die Anfangssicherung. Bis zur Durchführung der ersten Sicherung wird der Status der letzten Sicherung auf dem Blatt **Sicherungsaufträge** als **Warnung (anfängliche Sicherung steht aus)** angezeigt.

![Sicherung ausstehend](./media/backup-azure-vms-first-look-arm/initial-backup-not-run.png)

Falls die erste Sicherung nicht ohnehin in Kürze durchgeführt wird, empfiehlt es sich, **Jetzt sichern**auszuführen. Das folgende Verfahren startet das Tresordashboard. Dieses Verfahren dient zur Ausführung des ersten Sicherungsauftrags, nachdem Sie alle erforderlichen Voraussetzungen geschaffen haben. Wenn der erste Sicherungsauftrag bereits ausgeführt wurde, ist dieses Verfahren nicht verfügbar. Die zugeordnete Sicherungsrichtlinie bestimmt den nächsten Sicherungsauftrag.  

So führen Sie den ersten Sicherungsauftrag aus:

1. Klicken Sie im Tresordashboard auf der Kachel **Sicherung** auf **Virtuelle Azure-Computer**. <br/>
    ![Symbol „Einstellungen“](./media/backup-azure-vms-first-look-arm/rs-vault-in-dashboard-backup-vms.png)

    Das Blatt **Sicherungselemente** wird geöffnet.
2. Klicken Sie auf dem Blatt **Sicherungselemente** mit der rechten Maustaste auf den Tresor, den Sie sichern möchten, und klicken Sie dann auf **Jetzt sichern**.

    ![Symbol „Einstellungen“](./media/backup-azure-vms-first-look-arm/back-up-now.png)

    Der Sicherungsauftrag wird ausgelöst. <br/>

    ![Sicherungsauftrag ausgelöst](./media/backup-azure-vms-first-look-arm/backup-triggered.png)
3. Klicken Sie im Tresordashboard auf der Kachel **Sicherungsaufträge** auf **Virtuelle Azure-Computer**, um zu prüfen, ob die erste Sicherung abgeschlossen wurde.

    ![Kachel „Sicherungsaufträge“](./media/backup-azure-vms-first-look-arm/open-backup-jobs.png)

    Das Blatt „Sicherungsaufträge“ wird geöffnet.
4. Im Blatt **Sicherungsaufträge** wird der Status aller Aufträge angezeigt.

    ![Kachel „Sicherungsaufträge“](./media/backup-azure-vms-first-look-arm/backup-jobs-in-jobs-view.png)

   > [!NOTE]
   > Während des Sicherungsvorgangs löscht die Sicherungserweiterung auf jedem virtuellen Computer alle Schreibvorgänge und erstellt eine konsistente Momentaufnahme.
   >
   >

    Wenn der Sicherungsauftrag abgeschlossen ist, lautet der Status *Abgeschlossen*.

## <a name="troubleshooting-errors"></a>Problembehandlung
Wenn beim Sichern des virtuellen Computers Probleme auftreten, finden Sie Hilfe im Artikel [Problembehandlung bei der Sicherung virtueller Azure-Computer](backup-azure-vms-troubleshoot.md) .

## <a name="next-steps"></a>Nächste Schritte
Nachdem Sie Ihren virtuellen Computer geschützt haben, können Sie sich mit den folgenden Artikeln über Verwaltungsaufgaben für virtuelle Computer und über die Wiederherstellung virtueller Computer informieren.

* [Verwalten und Überwachen Ihrer virtuellen Computer](backup-azure-manage-vms.md)
* [Wiederherstellen virtueller Computer](backup-azure-arm-restore-vms.md)



<!--HONumber=Nov16_HO3-->


