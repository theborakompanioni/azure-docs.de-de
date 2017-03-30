---
title: Verwalten von mit Resource Manager bereitgestellten Sicherungen virtueller Computer | Microsoft Docs
description: "Es wird beschrieben, wie Sie Sicherungen virtueller Computer verwalten und überwachen, die von Resource Manager bereitgestellt werden."
services: backup
documentationcenter: 
author: trinadhk
manager: shreeshd
editor: 
ms.assetid: f3050283-d60f-472d-b464-cb844e70d67e
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/28/2016
ms.author: trinadhk;markgal
translationtype: Human Translation
ms.sourcegitcommit: bb1ca3189e6c39b46eaa5151bf0c74dbf4a35228
ms.openlocfilehash: f8cb0030290d200ab79e340b9b0fe8f5765d49f0
ms.lasthandoff: 03/18/2017


---
# <a name="manage-azure-virtual-machine-backups"></a>Verwalten der Sicherung virtueller Computer
> [!div class="op_single_selector"]
> * [Verwalten von Azure-VM-Sicherungen](backup-azure-manage-vms.md)
> * [Verwalten von klassischen VM-Sicherungen](backup-azure-manage-vms-classic.md)
>
>

Dieser Artikel enthält hilfreiche Informationen zum Verwalten von VM-Sicherungen und geht auf die Sicherungswarnungen ein, die auf dem Portaldashboard zur Verfügung stehen. Die Anleitung in diesem Artikel gilt für die Verwendung von VMs mit Recovery Services-Tresoren. Die Erstellung von virtuellen Computern und der Schutz virtueller Computer werden in diesem Artikel nicht beschrieben. Eine Einführung zu dem Thema, wie Sie von Azure Resource Manager bereitgestellte VMs in Azure mit einem Recovery Services-Tresor schützen können, finden Sie unter [First look: Back up VMs to a Recovery Services vault](backup-azure-vms-first-look-arm.md)(Einführung: Sichern von VMs in einem Recovery Services-Tresor).

## <a name="manage-vaults-and-protected-virtual-machines"></a>Verwalten von Tresoren und geschützten virtuellen Computern
Im Azure-Portal ermöglicht das Dashboard für den Recovery Services-Tresor den Zugriff auf Informationen zum Tresor, z.B.:

* Letzte Sicherungsmomentaufnahme, die gleichzeitig der letzte Wiederherstellungspunkt ist <br\>
* Die Sicherungsrichtlinie <br\>
* Gesamtgröße aller Sicherungsmomentaufnahmen <br\>
* Anzahl von virtuellen Computern, die durch den Tresor geschützt sind <br\>

Bei vielen Verwaltungsaufgaben mit einer VM-Sicherung muss zunächst der Tresor über das Dashboard geöffnet werden. Da Tresore aber zum Schützen mehrerer Elemente (bzw. mehrerer virtueller Computer) verwendet werden können, müssen Sie zum Anzeigen der Details zu einem bestimmten virtuellen Computer das Dashboard für Tresorelemente öffnen. Im folgenden Verfahren wird veranschaulicht, wie Sie das *Tresordashboard* öffnen und dann mit dem *Dashboard für Tresorelemente* fortfahren. Beide Verfahren enthalten Tipps dazu, wie Sie den Tresor und das Tresorelement dem Azure-Dashboard hinzufügen, indem Sie den Befehl „An Dashboard anheften“ verwenden. Mit der Option „An Dashboard anheften“ können Sie eine Verknüpfung zum Tresor oder Element erstellen. Über die Verknüpfung können Sie auch häufig verwendete Befehle ausführen.

> [!TIP]
> Wenn mehrere Dashboards und Blätter geöffnet sind, verwenden Sie den dunkelblauen Schieberegler im unteren Fensterbereich, um das Azure-Dashboard zu verschieben.
>
>

![Vollständige Ansicht mit Schieberegler](./media/backup-azure-manage-vms/bottom-slider.png)

### <a name="open-a-recovery-services-vault-in-the-dashboard"></a>Öffnen Sie im Dashboard einen Recovery Services-Tresor:
1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com/)an.
2. Klicken Sie im Hub-Menü auf **Durchsuchen**, und geben Sie in der Liste mit den Ressourcen **Recovery Services** ein. Sobald Sie mit der Eingabe beginnen, wird die Liste auf der Grundlage Ihrer Eingabe gefiltert. Klicken Sie auf **Recovery Services-Tresor**.

    ![Erstellen eines Recovery Services-Tresors – Schritt 1](./media/backup-azure-manage-vms/browse-to-rs-vaults.png) <br/>

    Die Liste mit den Recovery Services-Tresoren wird angezeigt.

    ![Liste der Recovery Services-Tresore ](./media/backup-azure-manage-vms/list-o-vaults.png) <br/>

   > [!TIP]
   > Wenn Sie einen Tresor an das Azure-Dashboard anheften, ist der Tresor nach dem Öffnen des Azure-Portals sofort verfügbar. Klicken Sie zum Anheften eines Tresors an das Dashboard in der Tresorliste mit der rechten Maustaste auf den Tresor, und wählen Sie **An Dashboard anheften**.
   >
   >
3. Wählen Sie in der Liste mit den Tresoren den Tresor aus, dessen Dashboard Sie öffnen möchten. Wenn Sie den Tresor ausgewählt haben, werden das Tresor-Dashboard und das Blatt **Einstellungen** geöffnet. In der folgenden Abbildung ist das Dashboard **Contoso-vault***** hervorgehoben.

    ![Tresor-Dashboard und Blatt „Einstellungen“ öffnen](./media/backup-azure-manage-vms/full-view-rs-vault.png)

### <a name="open-a-vault-item-dashboard"></a>Öffnen eines Dashboards für Tresorelemente
Im vorherigen Verfahren haben Sie das Tresor-Dashboard geöffnet. So öffnen Sie das Dashboard für Tresorelemente

1. Klicken Sie im Tresordashboard auf der Kachel **Sicherungselemente** auf **Virtuelle Azure-Computer**.

    ![Kachel „Sicherungselemente“ öffnen](./media/backup-azure-manage-vms/contoso-vault-1606.png)

    Auf dem Blatt **Sicherungselemente** ist jeweils der letzte Sicherungsauftrag für jedes Element aufgeführt. In diesem Beispiel ist ein virtueller Computer „demovm-markgal“ vorhanden, der mit diesem Tresor geschützt wird.  

    ![Kachel „Sicherungselemente“](./media/backup-azure-manage-vms/backup-items-blade.png)

   > [!TIP]
   > Um den Zugriff zu erleichtern, können Sie ein Tresorelement an das Azure-Dashboard anheften. Klicken Sie zum Anheften eines Tresorelements in der Liste mit den Tresorelementen mit der rechten Maustaste auf das Element, und wählen Sie **An Dashboard anheften**.
   >
   >
2. Klicken Sie im Blatt **Sicherungselemente** auf das Element, um das Dashboard für die Tresorelemente zu öffnen.

    ![Kachel „Sicherungselemente“](./media/backup-azure-manage-vms/backup-items-blade-select-item.png)

    Das Dashboard für die Tresorelemente wird mit dem Blatt **Einstellungen** geöffnet.

    ![Sicherungselemente-Dashboard mit Blatt „Einstellungen“](./media/backup-azure-manage-vms/item-dashboard-settings.png)

    Über das Dashboard für die Tresorelemente können Sie viele wichtige Verwaltungsaufgaben durchführen, z.B.:

   * Ändern von Richtlinien oder Erstellen einer neuen Sicherungsrichtlinie<br\>
   * Anzeigen von Wiederherstellungspunkten und des Konsistenzstatus<br\>
   * Bedarfsgesteuerte Sicherung eines virtuellen Computers <br\>
   * Beenden des Schutzes virtueller Computer <br\>
   * Fortsetzen des Schutzes eines virtuellen Computers <br\>
   * Löschen von Sicherungsdaten (oder eines Wiederherstellungspunkts) <br\>
   * [Wiederherstellen von Sicherungsdatenträgern](backup-azure-arm-restore-vms.md#restore-backed-up-disks)  <br\>

Für die folgenden Verfahren ist der Startpunkt das Dashboard für die Tresorelemente.

## <a name="manage-backup-policies"></a>Verwalten von Sicherungsrichtlinien
1. Klicken Sie im [Dashboard für die Tresorelemente](backup-azure-manage-vms.md#open-a-vault-item-dashboard) auf **Alle Einstellungen**, um das Blatt **Einstellungen** zu öffnen.

    ![Blatt „Sicherungsrichtlinie“](./media/backup-azure-manage-vms/all-settings-button.png)
2. Klicken Sie im Blatt **Einstellungen** auf **Sicherungsrichtlinie**, um das Blatt zu öffnen.

    Im Blatt werden die Details zur Sicherungshäufigkeit und zur Aufbewahrungsdauer angezeigt.

    ![Blatt „Sicherungsrichtlinie“](./media/backup-azure-manage-vms/backup-policy-blade.png)
3. Im Menü **Sicherungsrichtlinie auswählen** :

   * Wählen Sie zum Ändern von Richtlinien eine andere Richtlinie aus, und klicken Sie auf **Speichern**. Die neue Richtlinie wird sofort auf den Tresor angewendet. <br\>
   * Wählen Sie zum Erstellen einer Richtlinie die Option **Neu erstellen**aus.

     ![Sicherung virtueller Computer](./media/backup-azure-manage-vms/backup-policy-create-new.png)

     Eine Anleitung zum Erstellen einer Sicherungsrichtlinie finden Sie unter [Definieren einer Sicherungsrichtlinie](backup-azure-manage-vms.md#defining-a-backup-policy).

[!INCLUDE [backup-create-backup-policy-for-vm](../../includes/backup-create-backup-policy-for-vm.md)]

## <a name="on-demand-backup-of-a-virtual-machine"></a>On-Demand-Sicherung eines virtuellen Computers
Sie können eine bedarfsabhängige Sicherung eines virtuellen Computers erstellen, sobald dieser für den Schutz konfiguriert ist. Wenn die erste Sicherung ansteht, erstellt die On-Demand-Sicherung eine vollständige Kopie des virtuellen Computers im Recovery Services-Tresor. Nachdem die erste Sicherung durchgeführt wurde, werden bei einer On-Demand-Sicherung nur Änderungen der vorherigen Momentaufnahme an den Recovery Services-Tresor gesendet. Die nachfolgenden Sicherungen sind also immer inkrementell.

> [!NOTE]
> Die Aufbewahrungsdauer für eine bedarfsabhängige Sicherung ist der Aufbewahrungswert, der in der Richtlinie für den Sicherungspunkt „Täglich“ angegeben wird. Wenn kein täglicher Sicherungspunkt ausgewählt ist, wird der wöchentliche Sicherungspunkt verwendet.
>
>

So lösen Sie eine bedarfsabhängige Sicherung eines virtuellen Computers aus

* Klicken Sie auf [Dashboard für die Tresorelemente](backup-azure-manage-vms.md#open-a-vault-item-dashboard) auf **Backup now**(Jetzt sichern).

    ![Schaltfläche „Jetzt sichern“](./media/backup-azure-manage-vms/backup-now-button.png)

    Im Portal wird überprüft, ob Sie einen bedarfsabhängigen Sicherungsauftrag starten möchten. Klicken Sie auf **Ja** , um den Sicherungsauftrag zu starten.

    ![Schaltfläche „Jetzt sichern“](./media/backup-azure-manage-vms/backup-now-check.png)

    Der Sicherungsauftrag erstellt einen Wiederherstellungspunkt. Die Beibehaltungsdauer des Wiederherstellungspunkts entspricht der Beibehaltungsdauer, die in der Richtlinie für den virtuellen Computer angegeben ist. Klicken Sie zum Nachverfolgen des Auftragsstatus auf dem Tresordashboard auf die Kachel **Sicherungsaufträge** .  

## <a name="stop-protecting-virtual-machines"></a>Beenden des Schutzes für virtuelle Computer
Wenn Sie sich für das Beenden des Schutzes für einen virtuellen Computer entscheiden, werden Sie gefragt, ob Sie die Wiederherstellungspunkte beibehalten möchten. Der Schutz für virtuelle Computer kann auf zwei Arten beendet werden:

* Beenden aller zukünftigen Sicherungsaufträge und Löschen aller Wiederherstellungspunkte oder
* Beenden aller zukünftigen Sicherungsaufträge und Beibehaltung der Wiederherstellungspunkte  <br/>

Mit der Beibehaltung der Wiederherstellungspunkte im Speicher sind Kosten verbunden. Der Vorteil der Beibehaltung von Wiederherstellungspunkten ist, dass Sie den virtuellen Computer bei Bedarf später wiederherstellen können. Informationen zu den Kosten, die durch die Beibehaltung der Wiederherstellungspunkte entstehen, finden Sie in der [Preisübersicht](https://azure.microsoft.com/pricing/details/backup/). Wenn Sie alle Wiederherstellungspunkte löschen, kann der virtuelle Computer nicht mehr wiederhergestellt werden.

So beenden Sie den Schutz für einen virtuellen Computer

1. Klicken Sie auf dem [Dashboard für die Tresorelemente](backup-azure-manage-vms.md#open-a-vault-item-dashboard)auf **Sicherung beenden**.

    ![Schaltfläche „Sicherung beenden“](./media/backup-azure-manage-vms/stop-backup-button.png)

    Das Blatt „Sicherung beenden“ wird geöffnet.

    ![Blatt „Sicherung beenden“](./media/backup-azure-manage-vms/stop-backup-blade.png)
2. Wählen Sie auf dem Blatt **Sicherung beenden** aus, ob die Sicherungsdaten beibehalten oder gelöscht werden sollen. Das Feld mit den Informationen enthält jeweils die Details zu Ihrer Wahl.

    ![Schutz beenden](./media/backup-azure-manage-vms/retain-or-delete-option.png)
3. Wenn Sie sich für die Beibehaltung der Sicherungsdaten entschieden haben, fahren Sie mit Schritt 4 fort. Falls Sie das Löschen der Sicherungsdaten gewählt haben, müssen Sie bestätigen, dass die Sicherungsaufträge beendet und die Wiederherstellungspunkte gelöscht werden sollen. Geben Sie den Namen des Elements ein.

    ![Überprüfung beenden](./media/backup-azure-manage-vms/item-verification-box.png)

    Falls Sie unsicher sind, wie der Name lautet, können Sie mit der Maus auf das Ausrufezeichen zeigen, um den Namen anzuzeigen. Der Name des Elements ist auch am oberen Rand des Blatts unter **Sicherung beenden** angegeben.
4. Geben Sie optional einen **Grund** oder einen **Kommentar** ein.
5. Klicken Sie auf die Schaltfläche  ![Sicherung beenden](./media/backup-azure-manage-vms/stop-backup-button-blue.png), um den Sicherungsauftrag für das aktuelle Element zu beenden.

    Sie werden mit einer Benachrichtigung darüber informiert, dass die Sicherungsaufträge beendet wurden.

    ![Bestätigen des Beendens des Schutzes](./media/backup-azure-manage-vms/stop-message.png)

## <a name="resume-protection-of-a-virtual-machine"></a>Fortsetzen des Schutzes eines virtuellen Computers
Wenn beim Beenden des Schutzes für den virtuellen Computer die Option **Sicherungsdaten beibehalten** ausgewählt wurde, kann der Schutz fortgesetzt werden. Wenn die Option **Sicherungsdaten löschen** ausgewählt wurde, kann der Schutz für den virtuellen Computer nicht fortgesetzt werden.

So setzen Sie den Schutz für den virtuellen Computer fort

1. Klicken Sie auf dem [Dashboard für die Tresorelemente](backup-azure-manage-vms.md#open-a-vault-item-dashboard) auf **Sicherung fortsetzen**.

    ![Schutz fortsetzen](./media/backup-azure-manage-vms/resume-backup-button.png)

    Das Blatt „Sicherungsrichtlinie“ wird geöffnet.

   > [!NOTE]
   > Wenn Sie den virtuellen Computer erneut schützen, können Sie eine andere Richtlinie als die Richtlinie auswählen, gemäß der der virtuelle Computer zuvor geschützt wurde.
   >
   >
2. Führen Sie die Schritte unter [Verwalten von Sicherungsrichtlinien](backup-azure-manage-vms.md#manage-backup-policies) aus, um die Richtlinie für den virtuellen Computer zuzuweisen.

    Nachdem die Sicherungsrichtlinie auf den virtuellen Computer angewendet wurde, wird die folgende Meldung angezeigt:

    ![Erfolgreich geschützte VM](./media/backup-azure-manage-vms/success-message.png)

## <a name="delete-backup-data"></a>Sicherungsdaten löschen
Sie können die Sicherungsdaten, die einem virtuellen Computer zugeordnet sind, während des Auftrags **Sicherung beenden** oder jederzeit nach Abschluss des Sicherungsauftrags löschen. Es kann gegebenenfalls sogar von Vorteil sein, mit dem Löschen der Wiederherstellungspunkte mehrere Tage oder Wochen zu warten. Im Gegensatz zur Wiederherstellung von Wiederherstellungspunkten können Sie beim Löschen von Sicherungsdaten keine bestimmten Wiederherstellungspunkte auswählen, die gelöscht werden sollen. Wenn Sie sich für die Löschung Ihrer Sicherungsdaten entscheiden, werden alle dem Element zugeordneten Wiederherstellungspunkte gelöscht.

Beim folgenden Verfahren wird davon ausgegangen, dass der Sicherungsauftrag für den virtuellen Computer beendet oder deaktiviert wurde. Nach dem Deaktivieren des Sicherungsauftrags sind die Optionen **Sicherung fortsetzen** und **Sicherung löschen** im Dashboard für die Tresorelemente verfügbar.

![Schaltflächen zum Fortsetzen und Löschen](./media/backup-azure-manage-vms/resume-delete-buttons.png)

So löschen Sie Sicherungsdaten auf einem virtuellen Computer mit *deaktivierter Sicherung*:

1. Klicken Sie auf dem [Dashboard für die Tresorelemente](backup-azure-manage-vms.md#open-a-vault-item-dashboard)auf **Sicherung löschen**.

    ![VM-Typ](./media/backup-azure-manage-vms/delete-backup-buttom.png)

    Das Blatt **Sicherungsdaten löschen** wird geöffnet.

    ![VM-Typ](./media/backup-azure-manage-vms/delete-backup-blade.png)
2. Geben Sie den Namen des Elements ein, um zu bestätigen, dass Sie die Wiederherstellungspunkte löschen möchten.

    ![Überprüfung beenden](./media/backup-azure-manage-vms/item-verification-box.png)

    Falls Sie unsicher sind, wie der Name lautet, können Sie mit der Maus auf das Ausrufezeichen zeigen, um den Namen anzuzeigen. Der Name des Elements ist auch oben auf dem Blatt unter **Sicherungsdaten löschen** angegeben.
3. Geben Sie optional einen **Grund** oder einen **Kommentar** ein.
4. Klicken Sie auf die Schaltfläche  ![Sicherung beenden](./media/backup-azure-manage-vms/delete-button.png), um die Sicherungsdaten für das aktuelle Element zu löschen.

    Sie werden mit einer Benachrichtigung darüber informiert, dass die Sicherungsdaten gelöscht wurden.

## <a name="next-steps"></a>Nächste Schritte
Informationen zum Neuerstellen eines virtuellen Computers über einen Wiederherstellungspunkt finden Sie unter [Wiederherstellen virtueller Azure-Computer](backup-azure-restore-vms.md). Informationen zum Schutz Ihrer virtuellen Computer finden Sie unter [Einführung: Schützen von Azure-VMs mit einem Recovery Services-Tresor](backup-azure-vms-first-look-arm.md). Informationen zum Überwachen von Ereignissen finden Sie unter [Überwachen von Warnungen für Sicherungen von virtuellen Azure-Computern](backup-azure-monitor-vms.md).

