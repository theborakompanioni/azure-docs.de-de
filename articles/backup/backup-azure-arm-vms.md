---
title: Sichern von virtuellen Azure-Computern | Microsoft Docs
description: Ermitteln, registrieren und sichern Sie virtuelle Azure-Computer in einem Recovery Services-Tresor.
services: backup
documentationcenter: 
author: markgalioto
manager: carmonm
editor: 
keywords: Sicherung virtueller Computer; Sichern virtueller Computer; Sicherung und Notfallwiederherstellung; ARM-VM-Sicherung
ms.assetid: 5c68481d-7be3-4e68-b87c-0961c267053e
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 2/15/2017
ms.author: trinadhk;jimpark;markgal;
ms.custom: H1Hack27Feb2017
translationtype: Human Translation
ms.sourcegitcommit: 82b7541ab1434179353247ffc50546812346bda9
ms.openlocfilehash: 72f1ed52c9e0f39009efc54967d7cc68517182a3
ms.lasthandoff: 03/02/2017


---
# <a name="back-up-azure-virtual-machines-to-a-recovery-services-vault"></a>Sichern von virtuellen Azure-Computern in einem Recovery Services-Tresor
> [!div class="op_single_selector"]
> * [Sichern virtueller Azure Resource Manager-Computer (ARM)](backup-azure-arm-vms.md)
> * [Sichern von virtuellen Azure-Computern](backup-azure-vms.md)
>
>

Dieser Artikel beschreibt das Verfahren zum Sichern virtueller (mit Resource Manager oder dem klassischen Modell bereitgestellter) Azure-Computer in einem Recovery Services-Tresor. Beim Sichern von virtuellen Computern macht die Vorbereitung die meiste Arbeit. Bevor Sie einen virtuellen Computer sichern oder schützen können, müssen Sie die [Voraussetzungen](backup-azure-arm-vms-prepare.md) zum Vorbereiten der Umgebung auf den Schutz Ihrer VMs schaffen. Wenn Sie die Voraussetzungen geschaffen haben, können Sie den Sicherungsvorgang starten, um Momentaufnahmen Ihres virtuellen Computers zu erstellen.


[!INCLUDE [learn about backup deployment models](../../includes/backup-deployment-models.md)]

Weitere Informationen finden Sie in den Artikeln [Planen der Sicherungsinfrastruktur für virtuelle Computer in Azure](backup-azure-vms-introduction.md) und [Dokumentation zu virtuellen Computern](https://azure.microsoft.com/documentation/services/virtual-machines/).

## <a name="triggering-the-backup-job"></a>Auslösen des Sicherungsauftrags
Die Sicherungsrichtlinie des Recovery Services-Tresors definiert, wie oft und wann der Sicherungsvorgang ausgeführt wird. Standardmäßig ist die erste geplante Sicherung die Anfangssicherung. Bis zur Durchführung der ersten Sicherung wird der Status der letzten Sicherung auf dem Blatt **Sicherungsaufträge** als **Warnung (anfängliche Sicherung steht aus)** angezeigt.

![Sicherung ausstehend](./media/backup-azure-vms-first-look-arm/initial-backup-not-run.png)

Falls die erste Sicherung nicht ohnehin in Kürze durchgeführt wird, empfiehlt es sich, **Jetzt sichern**auszuführen. Das folgende Verfahren startet das Tresordashboard. Dieses Verfahren dient zur Ausführung des ersten Sicherungsauftrags, nachdem Sie alle erforderlichen Voraussetzungen geschaffen haben. Wenn der erste Sicherungsauftrag bereits ausgeführt wurde, ist dieses Verfahren nicht verfügbar. Die zugeordnete Sicherungsrichtlinie bestimmt den nächsten Sicherungsauftrag.  

So führen Sie den ersten Sicherungsauftrag aus:

1. Klicken Sie im Tresordashboard auf die Zahl unter **Sicherungselemente**, oder klicken Sie auf die Kachel **Sicherungselemente**. <br/>
  ![Symbol „Einstellungen“](./media/backup-azure-vms-first-look-arm/rs-vault-config-vm-back-up-now-1.png)

  Das Blatt **Sicherungselemente** wird geöffnet.

  ![Sicherungselemente](./media/backup-azure-vms-first-look-arm/back-up-items-list.png)

2. Wählen Sie das Element auf dem Blatt **Sicherungselemente** aus.

  ![Symbol „Einstellungen“](./media/backup-azure-vms-first-look-arm/back-up-items-list-selected.png)

  Die Liste **Sicherungselemente** wird geöffnet. <br/>

  ![Sicherungsauftrag ausgelöst](./media/backup-azure-vms-first-look-arm/backup-items-not-run.png)

3. Klicken Sie in der Liste **Sicherungselemente** auf die Auslassungszeichen (**...**), um das Kontextmenü zu öffnen.

  ![Kontextmenü](./media/backup-azure-vms-first-look-arm/context-menu.png)

  Das Kontextmenü wird angezeigt.

  ![Kontextmenü](./media/backup-azure-vms-first-look-arm/context-menu-small.png)

4. Klicken Sie im Kontextmenü auf **Jetzt sichern**.

  ![Kontextmenü](./media/backup-azure-vms-first-look-arm/context-menu-small-backup-now.png)

  Das Blatt „Jetzt sichern“ wird geöffnet.

  ![Blatt „Jetzt sichern“](./media/backup-azure-vms-first-look-arm/backup-now-blade-short.png)

5. Klicken Sie auf dem Blatt „Jetzt sichern“ auf das Kalendersymbol, verwenden Sie das Kalendersteuerelement zum Auswählen des letzten Aufbewahrungstags des Wiederherstellungspunkts, und klicken Sie auf **Sicherung**.

  ![Festlegen des letzten Aufbewahrungstags für den Wiederherstellungspunkt für „Jetzt sichern“](./media/backup-azure-vms-first-look-arm/backup-now-blade-calendar.png)

  Anhand von Bereitstellungsbenachrichtigungen werden Sie darüber informiert, dass der Sicherungsauftrag ausgelöst wurde und dass Sie den Status des Auftrags auf der Seite „Sicherungsaufträge“ überwachen können. Je nach Größe Ihrer VM kann das Erstellen der ersten Sicherung einige Zeit dauern.

6. Klicken Sie im Tresordashboard auf der Kachel **Sicherungsaufträge** auf **In Bearbeitung**, um den Status der ersten Sicherung anzuzeigen oder nachzuverfolgen.

  ![Kachel „Sicherungsaufträge“](./media/backup-azure-vms-first-look-arm/open-backup-jobs-1.png)

  Das Blatt „Sicherungsaufträge“ wird geöffnet.

  ![Kachel „Sicherungsaufträge“](./media/backup-azure-vms-first-look-arm/backup-jobs-in-jobs-view-1.png)

  Im Blatt **Sicherungsaufträge** wird der Status aller Aufträge angezeigt. Überprüfen Sie, ob der Sicherungsvorgang für Ihre VM noch aktiv ist oder beendet wurde. Wenn ein Sicherungsauftrag abgeschlossen ist, lautet der Status *Abgeschlossen*.

  > [!NOTE]
  > Als Teil des Sicherungsvorgangs gibt der Azure Backup-Dienst einen Befehl an die Sicherungserweiterung auf jeder VM aus, damit alle Schreibvorgänge geleert werden und eine konsistente Momentaufnahme erstellt wird.
  >
  >

## <a name="troubleshooting-errors"></a>Problembehandlung
Wenn beim Sichern des virtuellen Computers Probleme auftreten, finden Sie Hilfe im Artikel [Problembehandlung bei der Sicherung virtueller Azure-Computer](backup-azure-vms-troubleshoot.md) .

## <a name="next-steps"></a>Nächste Schritte
Nachdem Sie Ihren virtuellen Computer geschützt haben, können Sie sich mit den folgenden Artikeln über Verwaltungsaufgaben für virtuelle Computer und über die Wiederherstellung virtueller Computer informieren.

* [Verwalten und Überwachen Ihrer virtuellen Computer](backup-azure-manage-vms.md)
* [Wiederherstellen virtueller Computer](backup-azure-arm-restore-vms.md)

