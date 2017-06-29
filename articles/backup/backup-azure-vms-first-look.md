---
title: "Einführung: Sichern von Azure-VMs mit einem Sicherungstresor | Microsoft-Dokumentation"
description: "Verwenden Sie das klassische Portal, um virtuelle Azure-Computer in einem Sicherungstresor zu sichern. In diesem Tutorial werden alle Phasen erläutert, d.h. die Erstellung des Sicherungstresors, das Registrieren der virtuellen Computer, das Erstellen der Sicherungsrichtlinie und das Ausführen des ersten Sicherungsauftrags."
services: backup
documentationcenter: 
author: markgalioto
manager: carmonm
editor: 
ms.assetid: 722820dc-b65f-425c-a9e5-c1946e896a87
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 6/14/2017
ms.author: markgal;
ms.translationtype: Human Translation
ms.sourcegitcommit: ef1e603ea7759af76db595d95171cdbe1c995598
ms.openlocfilehash: 61328e32763faea90074fc6d499e660c4109ab6d
ms.contentlocale: de-de
ms.lasthandoff: 06/16/2017


---
# <a name="first-look-backing-up-azure-virtual-machines"></a>Einführung: Sichern von virtuellen Azure-Computern
> [!div class="op_single_selector"]
> * [Schützen von VMs mit einem Recovery Services-Tresor](backup-azure-vms-first-look-arm.md)
> * [Schützen von Azure-VMs mit einem Sicherungstresor](backup-azure-vms-first-look.md)
>
>

In diesem Tutorial werden die Schritte zum Sichern eines virtuellen Azure-Computers (VM) in einem Sicherungstresor in Azure beschrieben. In diesem Artikel wird das klassische Modell bzw. Service Manager-Bereitstellungsmodell zum Sichern von virtuellen Computern erläutert. Die folgenden Schritte gelten nur für Sicherungstresors, die im klassischen Portal erstellt werden. Microsoft empfiehlt das Resource Manager-Modell für neue Bereitstellungen.

Wenn Sie eine VM in einem Recovery Services-Tresor sichern möchten, der einer Ressourcengruppe angehört, helfen Ihnen die Informationen unter [Einführung: Schützen von VMs mit einem Recovery Services-Tresor](backup-azure-vms-first-look-arm.md) weiter.

Die folgenden Voraussetzungen müssen erfüllt sein, damit Sie das Tutorial erfolgreich durcharbeiten können:

* Sie haben in Ihrem Azure-Abonnement einen virtuellen Computer erstellt.
* Der virtuelle Computer ist mit öffentlichen Azure-IP-Adressen verbunden. Weitere Informationen finden Sie unter [Netzwerkverbindung](backup-azure-vms-prepare.md#network-connectivity).


> [!NOTE]
> Azure verfügt über zwei Bereitstellungsmodelle zum Erstellen und Verwenden von Ressourcen: [Resource Manager-Modell und klassisches Modell](../azure-resource-manager/resource-manager-deployment-model.md). Dieses Tutorial ist für die Verwendung mit virtuellen Computern bestimmt, die im klassischen Portal erstellt werden.
>
>

## <a name="create-a-backup-vault"></a>Erstellen eines Sicherungstresors
Bei einem Sicherungstresor handelt es sich um eine Entität, in der alle Sicherungen und Wiederherstellungspunkte gespeichert werden, die im Laufe der Zeit erstellt wurden. Der Sicherungstresor enthält auch die Sicherungsrichtlinien, die auf die zu sichernden virtuellen Computer angewendet werden.

> [!IMPORTANT]
> Ab März 2017 können im klassischen Portal keine Sicherungstresore mehr erstellt werden.
> Sie können nun ein Upgrade für Ihre Sicherungstresore auf Recovery Services-Tresore durchführen. Detaillierte Informationen finden Sie im Artikel [Durchführen eines Upgrades für einen Sicherungstresor auf einen Recovery Services-Tresor](backup-azure-upgrade-backup-to-recovery-services.md). Microsoft empfiehlt, ein Upgrade für Ihre Sicherungstresore auf Recovery Services-Tresore durchzuführen.<br/> **Ab dem 1. November 2017**:
>- Für alle verbleibenden Sicherungstresore wird automatisch ein Upgrade auf Recovery Services-Tresore durchgeführt.
>- Der Zugriff auf Ihre Sicherungsdaten im klassischen Portal wird nicht möglich sein. Verwenden Sie stattdessen das Azure-Portal, um auf Ihre Sicherungsdaten in Recovery Services-Tresoren zuzugreifen.
>

## <a name="discover-and-register-azure-virtual-machines"></a>Ermitteln und Registrieren virtueller Azure-Computer
Führen Sie vor dem Registrieren der VM mit einem Tresor den Ermittlungsvorgang durch, um neue VMs zu identifizieren. Dadurch wird eine Liste mit im Abonnement enthaltenen virtuellen Computern sowie mit zusätzlichen Informationen wie Clouddienstname und Region ausgegeben.

1. Melden Sie sich beim [klassischen Azure-Portal](http://manage.windowsazure.com/)
2. Klicken Sie im klassischen Azure-Portal auf **Recovery Services** , um die Liste der Recovery Services-Tresore zu öffnen.
    ![Workload auswählen](./media/backup-azure-vms-first-look/recovery-services-icon.png)
3. Wählen Sie in der Liste mit den Tresoren den Tresor zum Sichern einer VM aus.

    Wenn Sie Ihren Tresor auswählen, wird er auf der Seite **Schnellstart** geöffnet.
4. Klicken Sie im Tresormenü auf **Registrierte Elemente**.

    ![Workload auswählen](./media/backup-azure-vms-first-look/configure-registered-items.png)
5. Wählen Sie im Menü **Typ** die Option **Virtueller Azure-Computer** aus.

    ![Workload auswählen](./media/backup-azure-vms/discovery-select-workload.png)
6. Klicken Sie unten auf der Seite auf **ERMITTELN** .
    ![Schaltfläche „Ermitteln“](./media/backup-azure-vms/discover-button-only.png)

    Der Ermittlungsvorgang kann einige Minuten dauern, während die virtuellen Computer in einer Tabelle aufgeführt werden. Am unteren Rand des Bildschirms wird eine Benachrichtigung angezeigt, die Ihnen mitteilt, dass der Vorgang ausgeführt wird.

    ![VMs ermitteln](./media/backup-azure-vms/discovering-vms.png)

    Die Benachrichtigung ändert sich, sobald der Vorgang abgeschlossen ist.

    ![Ermittlung abgeschlossen](./media/backup-azure-vms-first-look/discovery-complete.png)
7. Klicken Sie unten auf der Seite auf **REGISTRIEREN** .
    ![Schaltfläche „Registrieren“](./media/backup-azure-vms-first-look/register-icon.png)
8. Wählen Sie im Kontextmenü **Elemente registrieren** die virtuellen Computer aus, die Sie registrieren möchten.

   > [!TIP]
   > Mehrere virtuelle Computer können gleichzeitig registriert werden.
   >
   >

    Für jeden virtuellen Computer, den Sie ausgewählt haben, wird ein Auftrag erstellt.
9. Klicken Sie in der Benachrichtigung auf **Auftrag anzeigen**, um zur Seite **Aufträge** zu gelangen.

    ![Registrierungsauftrag](./media/backup-azure-vms/register-create-job.png)

    Der virtuelle Computer wird auch in der Liste der registrierten Elemente aufgeführt, ebenso wie der Status des Registrierungsvorgangs.

    ![Registrierungsstatus 1](./media/backup-azure-vms/register-status01.png)

    Wenn der Vorgang abgeschlossen ist, ändert sich der Status gemäß dem *registrierten* Status.

    ![Registrierungsstatus 2](./media/backup-azure-vms/register-status02.png)

## <a name="install-the-vm-agent-on-the-virtual-machine"></a>Installieren des VM-Agents auf dem virtuellen Computer
Der Azure VM-Agent muss auf dem virtuellen Azure-Computer installiert werden, damit die Sicherungserweiterung funktioniert. Wenn Ihr virtueller Computer aus dem Azure-Katalog erstellt wurde, ist der VM-Agent auf dem virtuellen Computer bereits vorhanden, und Sie können mit [Schützen Ihrer virtuellen Computer](backup-azure-vms-first-look.md#create-the-backup-policy) fortfahren.

Falls Sie Ihren virtueller Computer aus einem lokalen Rechenzentrum migriert haben, wurde der VM-Agent wahrscheinlich noch nicht für den virtuellen Computer installiert. Sie müssen den VM-Agent auf dem virtuellen Computer installieren, bevor Sie mit dem Schützen des virtuellen Computers fortfahren. Ausführliche Schritte zum Installieren des VM-Agents finden Sie im [Abschnitt zum VM-Agent im Artikel, in dem das Sichern virtueller Computer behandelt wird](backup-azure-vms-prepare.md#vm-agent).

## <a name="create-the-backup-policy"></a>Erstellen der Sicherungsrichtlinie
Bevor Sie den ersten Sicherungsauftrag ausführen, legen Sie einen Zeitplan für Sicherungsmomentaufnahmen fest. Die Sicherungsrichtlinie besteht aus dem Zeitplan für Sicherungsmomentaufnahmen sowie der Dauer, für die diese Aufnahmen gespeichert werden. Als Rotationsschema für die Beibehaltungsdauer wird das Generationenprinzip angewandt.

1. Navigieren Sie zum Sicherungstresor, der sich im klassischen Azure-Portal unter **Recovery Services** befindet, und klicken Sie auf **Registrierte Elemente**.
2. Wählen Sie im Dropdownmenü **Virtueller Azure-Computer** aus.

    ![Workload im Portal auswählen](./media/backup-azure-vms/select-workload.png)
3. Klicken Sie unten auf der Seite auf **SCHÜTZEN** .
    ![Auf „Schützen“ klicken](./media/backup-azure-vms-first-look/protect-icon.png)

    Der **Assistent zum Schützen von Elementen** wird angezeigt. Darin sind *nur* die virtuellen Computer aufgeführt, die registriert wurden und nicht geschützt sind.

    ![Effektives Konfigurieren von Schutzeinstellungen](./media/backup-azure-vms/protect-at-scale.png)
4. Wählen Sie die virtuellen Computer aus, die Sie schützen möchten.

    Wenn zwei oder mehr virtuelle Computer denselben Namen aufweisen, verwenden Sie den Clouddienst, um zwischen den virtuellen Computern zu unterscheiden.
5. Wählen Sie im Menü **Schutz konfigurieren** eine vorhandene Richtlinie aus, oder erstellen Sie eine neue Richtlinie, um die ermittelten virtuellen Computer zu schützen.

    Bei neuen Sicherungstresoren ist dem Tresor eine Standardrichtlinie zugeordnet. Dabei wird jeden Abend eine Momentaufnahmen gemacht, die jeweils 30 Tage lang aufbewahrt wird. Jeder Sicherungsrichtlinie können mehrere virtuelle Computer zugeordnet sein. Ein virtueller Computer kann jedoch immer nur einer Richtlinie zugeordnet sein.

    ![Schützen mit der neuen Richtlinie](./media/backup-azure-vms/policy-schedule.png)

   > [!NOTE]
   > Eine Sicherungsrichtlinie umfasst auch ein Aufbewahrungsschema für die geplanten Sicherungen. Bei Wahl einer vorhandenen Sicherungsrichtlinie können Sie die Aufbewahrungsoptionen im nächsten Schritt nicht ändern.
   >
   >
6. Wählen Sie unter **Beibehaltungsdauer** die täglichen, wöchentlichen, monatlichen und jährlichen Bereiche für die einzelnen Sicherungspunkte aus.

    ![Virtueller Computer wird mit Wiederherstellungspunkt gesichert](./media/backup-azure-vms/long-term-retention.png)

    Die Aufbewahrungsrichtlinie legt fest, für welchen Zeitraum eine Sicherung aufbewahrt wird. Je nach Erstellungszeitpunkt der Sicherung können Sie unterschiedliche Aufbewahrungsrichtlinien festlegen.
7. Klicken Sie auf **Aufträge**, um die Liste mit den Aufträgen vom Typ **Schutz konfigurieren** anzuzeigen.

    ![Schutzauftrag konfigurieren](./media/backup-azure-vms/protect-configureprotection.png)

    Nachdem Sie diese Richtlinie nun eingerichtet haben, können Sie mit dem nächsten Schritt fortfahren und die erste Sicherung durchführen.

## <a name="initial-backup"></a>Erste Sicherung
Wenn ein virtueller Computer mit einer Richtlinie geschützt wird, können Sie diese Beziehung auf der Registerkarte **Geschützte Elemente** anzeigen. Bis zur Durchführung der ersten Sicherung wird der **Schutzstatus** als **Geschützt (erste Sicherung ausstehend)** angezeigt. Standardmäßig ist die erste geplante Sicherung die *Anfangssicherung*.

![Sicherung ausstehend](./media/backup-azure-vms-first-look/protection-pending-border.png)

Starten Sie die anfängliche Sicherung:

1. Klicken Sie unten auf der Seite **Geschützte Elemente** auf **Jetzt sichern**.
    ![Symbol „Jetzt sichern“](./media/backup-azure-vms-first-look/backup-now-icon.png)

    Der Azure Backup-Dienst erstellt einen Sicherungsauftrag für die erste Sicherung.
2. Klicken Sie auf die Registerkarte **Aufträge** , um die Liste der Aufträge anzuzeigen.

    ![Sicherung wird ausgeführt](./media/backup-azure-vms-first-look/protect-inprogress.png)

    Sobald die erste Sicherung abgeschlossen ist, wird der Status des virtuellen Computers auf der Registerkarte **Geschützte Elemente** als *Geschützt*angezeigt.

    ![Virtueller Computer wird mit Wiederherstellungspunkt gesichert](./media/backup-azure-vms/protect-backedupvm.png)

   > [!NOTE]
   > Das Sichern virtueller Computer ist ein örtlich gebundener Vorgang. Sie können keine virtuellen Computer aus einer Region in einem Sicherungstresor in einer anderen Region sichern. Deshalb muss in jeder Azure-Region mit virtuellen Computern, die gesichert werden sollen, mindestens ein Sicherungstresor erstellt werden.
   >
   >

## <a name="next-steps"></a>Nächste Schritte
Nachdem Sie einen virtuellen Computer erfolgreich gesichert haben, sind unter Umständen weitere Schritte von Interesse. Der logische nächste Schritt besteht darin, sich mit dem Wiederherstellen von Daten auf einer VM vertraut zu machen. Es gibt aber auch Verwaltungsaufgaben, mit denen Sie besser verstehen können, wie Sie Ihre Daten schützen und die Kosten senken.

* [Verwalten und Überwachen Ihrer virtuellen Computer](backup-azure-manage-vms.md)
* [Wiederherstellen virtueller Computer](backup-azure-restore-vms.md)
* [Anleitungen zur Problembehandlung](backup-azure-vms-troubleshoot.md)

## <a name="questions"></a>Fragen?
Wenn Sie Fragen haben oder Anregungen zu gewünschten Funktionen mitteilen möchten, [senden Sie uns Ihr Feedback](http://aka.ms/azurebackup_feedback).

