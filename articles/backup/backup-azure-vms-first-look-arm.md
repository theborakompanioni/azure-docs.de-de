<properties
	pageTitle="Schützen von ARM-VMs per Azure Backup | Microsoft Azure"
	description="Schützen Sie ARM-VMs mit dem Azure Backup-Dienst. Verwenden Sie Sicherungen virtueller ARM- und Storage Premium-Computer, um Ihre Daten zu schützen. Erstellen und registrieren Sie einen Recovery Services-Tresor. Registrieren Sie VMs, erstellen Sie eine Richtlinie, und schützen Sie VMs in Azure."
	services="backup"
	documentationCenter=""
	authors="markgalioto"
	manager="jwhit"
	editor=""
	keyword="backups; vm backup"/>

<tags
	ms.service="backup"
	ms.workload="storage-backup-recovery"
	ms.tgt_pltfrm="na"
	ms.devlang="na"
	ms.topic="hero-article"
	ms.date="03/31/2016"
	ms.author="markgal; jimpark"/>


# Einführung: Sichern von ARM-VMs in einem Recovery Services-Tresor

> [AZURE.SELECTOR]
- [Sichern von ARM-VMs](backup-azure-vms-first-look-arm.md)
- [Sichern von VMs im klassischen Modus](backup-azure-vms-first-look.md)

In diesem Tutorial werden Sie durch die Schritte zum Erstellen eines Recovery Services-Tresors und Sichern eines virtuellen Azure-Computers (VM) geführt. Recovery Services-Tresore schützen Folgendes:

- Support für virtuelle Azure Resource Manager (ARM)-Computer
- Klassische virtuelle Computer
- Virtuelle Standardspeichercomputer
- Virtuelle Storage Premium-Computer

Weitere Informationen zum Schutz virtueller Storage Premium-Computer finden Sie unter [Back up and Restore Premium Storage VMs](backup-introduction-to-azure-backup.md#back-up-and-restore-premium-storage-vms) (Sichern und Wiederherstellen virtueller Storage Premium-Computer).

>[AZURE.NOTE] Dieses Tutorial setzt voraus, dass Sie in Ihrem Azure-Abonnement bereits einen virtuellen Computer verwenden und die entsprechenden Schritte durchgeführt haben, um dem Backup-Dienst den Zugriff auf den virtuellen Computer zu erlauben. Azure verfügt über zwei Bereitstellungsmodelle zum Erstellen und Verwenden von Ressourcen: [das Resource Manager-Modell und das klassische Modell](../resource-manager-deployment-model.md). Dieser Artikel ist für die Verwendung mit Resource Manager und ARM-basierten VMs bestimmt.

Dies sind im Allgemeinen die Schritte, die auszuführen sind.

1. Erstellen Sie einen Recovery Services-Tresor für eine VM.
2. Verwenden Sie das Azure-Portal, um ein Szenario zu wählen, die Richtlinie festzulegen und zu schützende Elemente zu ermitteln.
3. Führen Sie die erste Sicherung durch.



## Schritt 1: Erstellen eines Recovery Services-Tresors für eine VM

Bei einem Recovery Services-Tresor handelt es sich um eine Entität, in der alle Sicherungen und Wiederherstellungspunkte gespeichert werden, die im Laufe der Zeit erstellt wurden. Der Recovery Services-Tresor enthält auch die Sicherungsrichtlinie, die auf die geschützten VMs angewendet wird.

>[AZURE.NOTE] Das Sichern von VMs ist ein lokaler Vorgang. Es ist nicht möglich, VMs eines Standorts in einem Recovery Services-Tresor an einem anderen Standort zu sichern. An jedem Azure-Standort, der über zu sichernde VMs verfügt, muss also mindestens ein Recovery Services-Tresor vorhanden sein.


So erstellen Sie einen Recovery Services-Tresor

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com/) an.

2. Klicken Sie im Hub-Menü auf **Durchsuchen**, und geben Sie in der Liste mit den Ressourcen **Recovery Services** ein. Wenn Sie mit der Eingabe beginnen, wird die Liste anhand Ihrer Eingaben gefiltert. Klicken Sie auf **Recovery Services-Tresor**.

    ![Erstellen eines Recovery Services-Tresors – Schritt 1](./media/backup-azure-vms-first-look-arm/browse-to-rs-vaults.png) <br/>

    Die Liste mit den Recovery Services-Tresoren wird angezeigt.

3. Klicken Sie im Menü **Recovery Services-Tresore** auf **Hinzufügen**.

    ![Erstellen eines Recovery Services-Tresors – Schritt 2](./media/backup-azure-vms-first-look-arm/rs-vault-menu.png)

    Das Blatt „Recovery Services-Tresor“ wird geöffnet, und Sie werden aufgefordert, **Name**, **Abonnement**, **Ressourcengruppe** und **Standort** anzugeben.

    ![Erstellen eines Recovery Services-Tresors – Schritt 5](./media/backup-azure-vms-first-look-arm/rs-vault-attributes.png)

4. Geben Sie unter **Name** einen Anzeigenamen für den Tresor ein. Der Name muss für das Azure-Abonnement eindeutig sein. Geben Sie einen Namen ein, der zwischen 2 und 50 Zeichen enthält. Er muss mit einem Buchstaben beginnen und darf nur Buchstaben, Zahlen und Bindestriche enthalten.

5. Klicken Sie auf **Abonnement**, um die Liste mit den verfügbaren Abonnements anzuzeigen. Falls Sie nicht sicher sind, welches Abonnement geeignet ist, können Sie das Standardabonnement bzw. das vorgeschlagene Abonnement verwenden. Es sind nur dann mehrere Auswahlmöglichkeiten verfügbar, wenn Ihr Organisationskonto mehreren Azure-Abonnements zugeordnet ist.

6. Klicken Sie auf **Ressourcengruppe**, um die Liste mit den verfügbaren Ressourcengruppen anzuzeigen, oder klicken Sie auf **Neu**, um eine neue Ressourcengruppe zu erstellen. Die vollständigen Informationen zu Ressourcengruppen finden Sie unter [Verwenden des Azure-Portals zum Bereitstellen und Verwalten Ihrer Azure-Ressourcen](../azure-portal/resource-group-portal.md).

7. Klicken Sie auf **Standort**, um die geografische Region für den Tresor auszuwählen. Der Tresor **muss** sich in der gleichen Region wie die zu schützenden virtuellen Computer befinden.

    >[AZURE.IMPORTANT] Wenn Sie sich nicht sicher sind, an welchem Standort sich Ihr virtueller Computer befindet, schließen Sie das Dialogfeld zur Tresorerstellung, und wechseln Sie zur Liste der virtuellen Computer im Portal. Wenn Sie über virtuelle Computer in mehreren Regionen verfügen, müssen Sie in jeder Region einen Recovery Services-Tresor erstellen. Erstellen Sie den Tresor am ersten Standort, bevor Sie mit dem nächsten Standort fortfahren. Es müssen keine Speicherkonten zum Speichern der Sicherungsdaten angegeben werden. Dies erfolgt automatisch über den Recovery Services-Tresor und den Azure Backup-Dienst.

8. Klicken Sie auf **Erstellen**. Es kann einige Zeit dauern, bis der Recovery Services-Tresor erstellt wurde. Verfolgen Sie die Benachrichtigungen oben rechts im Portal. Nachdem der Tresor erstellt wurde, wird er im Portal geöffnet.

9. Klicken Sie in Ihrem Tresor auf **Alle Einstellungen** > **Sicherungskonfiguration**, um den **Speicherreplikationstyp** anzuzeigen. Wählen Sie die Speicherreplikationsoption für den Tresor aus.

    ![Liste der Sicherungstresore](./media/backup-azure-vms-first-look-arm/choose-storage-configuration.png)

    Standardmäßig verfügt Ihr Tresor über einen georedundanten Speicher. Wenn Sie Azure als primären Speicherendpunkt für die Sicherung verwenden, wird empfohlen, dass Sie den georedundanten Speicher weiterhin empfehlen. Wenn Sie Azure als nicht primären Speicherendpunkt für die Sicherung verwenden, können Sie auch den lokal redundanten Speicher wählen, was die Kosten für die Datenspeicherung in Azure verringert. In dieser [Übersicht](../storage/storage-redundancy.md) erfahren Sie mehr über die Optionen für [georedundante](../storage/storage-redundancy.md#geo-redundant-storage) und [lokal redundante](../storage/storage-redundancy.md#locally-redundant-storage) Speicher.

    Wenn Sie die Speicheroption für Ihren Tresor ausgewählt haben, können Sie den virtuellen Computer dem Tresor zuordnen. Ermitteln und registrieren Sie die virtuellen Azure-Computer, um mit der Zuordnung zu beginnen.

## Schritt 2: Auswählen der Szenariosatzrichtlinie und Definieren von zu schützenden Elementen
Führen Sie zunächst den Ermittlungsprozess durch, bevor Sie einen virtuellen Computer registrieren, um sicherzustellen, dass alle neuen virtuellen Computer, die dem Abonnement hinzugefügt wurden, identifiziert werden. Bei diesem Vorgang wird Azure nach der Liste virtueller Computer im Abonnement abgefragt. Außerdem werden zusätzliche Informationen wie der Clouddienstname und die Region erfasst.

1. Falls Sie bereits über einen geöffneten Recovery Services-Tresor verfügen, können Sie mit Schritt 2 fortfahren. Wenn Sie keinen Recovery Services-Tresor geöffnet haben und sich im Azure-Portal befinden, klicken Sie im Hub-Menü auf **Durchsuchen**.

    - Geben Sie in der Liste mit den Ressourcen **Recovery Services** ein.
    - Wenn Sie mit der Eingabe beginnen, wird die Liste anhand Ihrer Eingaben gefiltert. Klicken Sie auf **Recovery Services-Tresore**, wenn der Eintrag angezeigt wird.

    ![Erstellen eines Recovery Services-Tresors – Schritt 1](./media/backup-azure-vms-first-look-arm/browse-to-rs-vaults.png) <br/>

    Die Liste mit den Recovery Services-Tresoren wird angezeigt.

    - Wählen Sie in der Liste mit den Recovery Services-Tresoren einen Tresor aus.

    Das ausgewählte Tresor-Dashboard wird geöffnet.

    ![Tresorblatt öffnen](./media/backup-azure-vms-first-look-arm/vault-settings.png)

2. Klicken Sie im Menü des Tresor-Dashboards auf **Sicherung**, um das Blatt „Sicherung“ zu öffnen.

    ![Blatt „Sicherung“ öffnen](./media/backup-azure-vms-first-look-arm/backup-button.png)

    Nachdem das Blatt geöffnet wurde, sucht der Backup-Dienst nach neuen VMs im Abonnement.

    ![VMs ermitteln](./media/backup-azure-vms-first-look-arm/discovering-new-vms.png)

3. Klicken Sie im Blatt „Sicherung“ auf **Szenario**, um das Blatt „Szenario“ zu öffnen.

    ![Blatt „Szenario“ öffnen](./media/backup-azure-vms-first-look-arm/select-backup-scenario-one.png)

4. Wählen Sie im Blatt „Szenario“ im Menü **Sicherungstyp** die Option **Azure Virtual Machines Backup** aus, und klicken Sie auf **OK**.

    ![Blatt „Szenario“ öffnen](./media/backup-azure-vms-first-look-arm/select-rs-backup-scenario-two.png)

    Das Blatt „Szenario“ wird geschlossen, und das Blatt „Sicherungsrichtlinie“ wird geöffnet.

5. Wählen Sie auf dem Blatt „Sicherung“ die Sicherungsrichtlinie aus, die Sie auf den Tresor anwenden möchten, und klicken Sie auf **OK**.

    ![Sicherungsrichtlinie auswählen](./media/backup-azure-vms-first-look-arm/setting-rs-backup-policy.png)

    Die Standardrichtlinie wird in den Details aufgeführt. Wählen Sie **Neu erstellen**, wenn Sie eine neue Richtlinie erstellen möchten. Eine Anleitung zum Definieren einer Sicherungsrichtlinie finden Sie unter [Definieren einer Sicherungsrichtlinie](backup-azure-vms-first-look-arm.md#defining-a-backup-policy). Nachdem Sie auf „OK“ geklickt haben, wird die Sicherungsrichtlinie dem Tresor zugeordnet. Wählen Sie als Nächstes die VMs aus, die dem Tresor zugeordnet werden sollen.

6. Wählen Sie die virtuellen Computer aus, die der angegebenen Richtlinie zugeordnet werden sollen, und klicken Sie auf **Auswählen**.

    ![Workload auswählen](./media/backup-azure-vms-first-look-arm/select-vms-to-backup.png)

    Klicken Sie auf **Aktualisieren**, wenn der gewünschte virtuelle Computer nicht in der Liste enthalten ist. Falls die gewünschte VM immer noch nicht angezeigt wird, sollten Sie überprüfen, ob sie an demselben Azure-Standort wie der Recovery Services-Tresor vorhanden ist.

7. Nachdem Sie nun alle Einstellungen für den Tresor definiert haben, klicken Sie im Blatt „Sicherung“ unten auf der Seite auf **Sicherung aktivieren**. Die Richtlinie wird im Tresor und auf den VMs bereitgestellt.

    ![Sicherung aktivieren](./media/backup-azure-vms-first-look-arm/enable-backup-settings.png)


## Schritt 3: Erste Sicherung

Nachdem eine Sicherungsrichtlinie auf dem virtuellen Computer bereitgestellt wurde, bedeutet dies nicht, dass die Daten gesichert wurden. Die erste geplante Sicherung (gemäß Definition in der Sicherungsrichtlinie) ist die erste Sicherung. Bis die erste Sicherung durchgeführt wird, wird der „Status der letzten Sicherung“ im Blatt **Sicherungsaufträge** als **Warnung (anfängliche Sicherung steht aus)** angezeigt.

![Sicherung ausstehend](./media/backup-azure-vms-first-look-arm/initial-backup-not-run.png)

Falls die erste Sicherung nicht in Kürze durchgeführt wird, empfiehlt es sich, die Option **Jetzt sichern** auszuführen.

So führen Sie **Jetzt sichern** aus:

1. Klicken Sie im Tresor-Dashboard auf der Kachel **Sicherung** auf **Azure Virtual Machines**. <br/> ![Symbol „Einstellungen“](./media/backup-azure-vms-first-look-arm/rs-vault-in-dashboard-backup-vms.png)

    Das Blatt **Sicherungselemente** wird geöffnet.

2. Klicken Sie im Blatt **Sicherungselemente** mit der rechten Maustaste auf den Tresor, den Sie sichern möchten, und klicken Sie dann auf **Jetzt sichern**.

    ![Symbol „Einstellungen“](./media/backup-azure-vms-first-look-arm/back-up-now.png)

    Der Sicherungsauftrag wird ausgelöst. <br/>

    ![Sicherungsauftrag ausgelöst](./media/backup-azure-vms-first-look-arm/backup-triggered.png)

3. Klicken Sie im Tresor-Dashboard auf der Kachel **Sicherungsaufträge** auf **Azure Virtual Machines**, um nachzusehen, ob die erste Sicherung abgeschlossen wurde.

    ![Kachel „Sicherungsaufträge“](./media/backup-azure-vms-first-look-arm/open-backup-jobs.png)

    Das Blatt „Sicherungsaufträge“ wird geöffnet.

4. Im Blatt „Sicherungsaufträge“ wird der Status aller Aufträge angezeigt.

    ![Kachel „Sicherungsaufträge“](./media/backup-azure-vms-first-look-arm/backup-jobs-in-jobs-view.png)

    >[AZURE.NOTE] Als Teil des Sicherungsvorgangs gibt der Azure Backup-Dienst einen Befehl an die Sicherungserweiterung auf jedem virtuellen Computer aus, damit alle Schreibvorgänge geleert werden und eine konsistente Momentaufnahme erstellt wird.

    Wenn der Sicherungsauftrag abgeschlossen ist, lautet der Status *Abgeschlossen*.

## Definieren einer Sicherungsrichtlinie

Mit einer Sicherungsrichtlinie wird eine Matrix dafür definiert, wann Datenmomentaufnahmen erstellt werden und wie lange diese Momentaufnahmen aufbewahrt werden. Wenn Sie eine Richtlinie zum Sichern eines virtuellen Computers definieren, können Sie einen Sicherungsauftrag *einmal pro Tag* auslösen. Wenn Sie eine neue Richtlinie erstellen, wird sie auf den Tresor angewendet. Die Oberfläche für die Sicherungsrichtlinie sieht wie folgt aus:

![Sicherungsrichtlinie](./media/backup-azure-vms-first-look-arm/backup-policy-daily-raw.png)

So erstellen Sie eine Richtlinie

1. Geben Sie unter **Richtlinienname** einen Namen für die Richtlinie ein.

2. Sie können in täglichen oder wöchentlichen Abständen Momentaufnahmen Ihrer Daten erstellen. Verwenden Sie das Dropdownmenü **Sicherungshäufigkeit**, um auszuwählen, ob Momentaufnahmen der Daten täglich oder wöchentlich erstellt werden.

    - Wenn Sie als Intervall „Täglich“ auswählen, können Sie das hervorgehobene Steuerelement verwenden, um die Tageszeit für die Momentaufnahme auszuwählen. Deaktivieren Sie zum Ändern der Stundenangabe die vorhandene Stunde, und wählen Sie dann die neue Stunde aus.

    ![Tägliche Sicherungsrichtlinie](./media/backup-azure-vms-first-look-arm/backup-policy-daily.png) <br/>

    - Verwenden Sie bei Auswahl des Intervalls „Wöchentlich“ die hervorgehobenen Steuerelemente, um die Wochentage und die Uhrzeit für die Erstellung der Momentaufnahme auszuwählen. Wählen Sie im Menü „Tag“ einen oder mehrere Tage aus. Wählen Sie im Menü „Stunde“ eine Stunde aus. Deaktivieren Sie zum Ändern der Stundenangabe die ausgewählte Stunde, und wählen Sie dann die neue Stunde aus.

    ![Wöchentliche Sicherungsrichtlinie](./media/backup-azure-vms-first-look-arm/backup-policy-weekly.png)

3. Standardmäßig sind alle Optionen unter **Beibehaltungsdauer** aktiviert. Deaktivieren Sie alle einschränkenden Optionen für die Beibehaltungsdauer, die Sie nicht verwenden möchten.

    >[AZURE.NOTE] Wenn eine VM geschützt wird, wird einmal pro Tag ein Sicherungsauftrag ausgeführt. Der Zeitpunkt, zu dem die Sicherung ausgeführt wird, ist für jede Beibehaltungsdauer gleich.

    Geben Sie mit den entsprechenden Steuerelementen die gewünschten Intervalle an. Mit monatlichen und jährlichen Beibehaltungsdauern können Sie die Momentaufnahmen basierend auf einem wöchentlichen oder täglichen Inkrement angeben.

4. Nachdem Sie alle Optionen für die Richtlinie festgelegt haben, klicken Sie unten auf dem Blatt auf **OK**.

    Die neue Richtlinie wird so festgelegt, dass sie auf den Tresor angewendet wird, nachdem die Einstellungen für den Recovery Services-Tresor abgeschlossen wurden. Kehren Sie zu Schritt 6 des Abschnitts [Auswählen der Szenariosatzrichtlinie und Definieren von zu schützenden Elementen](backup-azure-vms-first-look-arm.md#step-2---select-scenario-set-policy-and-define-items-to-protect) zurück.

## Installieren des VM-Agents auf dem virtuellen Computer

Diese Informationen werden bereitgestellt, falls es erforderlich ist. Der Azure VM-Agent muss auf dem virtuellen Azure-Computer installiert werden, damit die Sicherungserweiterung funktioniert. Wenn Ihr virtueller Computer aber über den Azure-Katalog erstellt wurde, ist der VM-Agent auf dem virtuellen Computer bereits vorhanden. Auf virtuellen Computern, die aus lokalen Rechenzentren migriert werden, ist der VM-Agent nicht installiert. In diesem Fall muss der VM-Agent installiert werden. Falls beim Sichern des virtuellen Azure-Computers Probleme auftreten, müssen Sie sicherstellen, dass der Azure VM-Agent auf dem virtuellen Computer korrekt installiert ist (s. Tabelle unten). Wenn Sie einen benutzerdefinierten virtuellen Computer erstellen, [müssen Sie das Kontrollkästchen **VM-Agent installieren** aktivieren](../virtual-machines/virtual-machines-windows-classic-agents-and-extensions.md), bevor Sie den virtuellen Computer bereitstellen.

Erfahren Sie mehr über den [VM-Agent](https://go.microsoft.com/fwLink/?LinkID=390493&clcid=0x409) und [seine Installation](../virtual-machines/virtual-machines-windows-classic-manage-extensions.md) (in englischer Sprache).

Die folgende Tabelle enthält weitere Informationen zum VM-Agent für virtuelle Windows- und Linux-Computer.

| **Vorgang** | **Windows** | **Linux** |
| --- | --- | --- |
| Installieren des VM-Agent | <li>Laden Sie den [Agent-MSI](http://go.microsoft.com/fwlink/?LinkID=394789&clcid=0x409) herunter, und installieren Sie ihn. Zum Durchführen der Installation benötigen Sie Administratorberechtigungen. <li>[Aktualisieren Sie die VM-Eigenschaft](http://blogs.msdn.com/b/mast/archive/2014/04/08/install-the-vm-agent-on-an-existing-azure-vm.aspx), um anzugeben, dass der Agent installiert wurde. | <li>Installieren Sie den neuesten [Linux-Agent](https://github.com/Azure/WALinuxAgent) aus GitHub. Zum Durchführen der Installation benötigen Sie Administratorberechtigungen. <li> [Aktualisieren Sie die VM-Eigenschaft](http://blogs.msdn.com/b/mast/archive/2014/04/08/install-the-vm-agent-on-an-existing-azure-vm.aspx), um anzugeben, dass der Agent installiert wurde. |
| Aktualisieren des VM-Agents | Das Aktualisieren des VM-Agents ist so einfach wie das Neuinstallieren der [Binärdateien für den VM-Agent](http://go.microsoft.com/fwlink/?LinkID=394789&clcid=0x409). <br>Stellen Sie sicher, dass kein Sicherungsvorgang ausgeführt wird, während der VM-Agent aktualisiert wird. | Folgen Sie den Anweisungen unter [Aktualisieren des Linux-VM-Agents](../virtual-machines-linux-update-agent.md). <br>Stellen Sie sicher, dass kein Sicherungsvorgang ausgeführt wird, während der VM-Agent aktualisiert wird. |
| Überprüfen der VM-Agent-Installation | <li>Navigieren Sie auf dem virtuellen Azure-Computer zum Ordner *C:\\WindowsAzure\\Packages*. <li>Dieser Ordner enthält die Datei „WaAppAgent.exe“.<li> Klicken Sie mit der rechten Maustaste auf die Datei, wechseln Sie zu **Eigenschaften**, und wählen Sie dann die Registerkarte **Details** aus. Im Feld mit der Produktversion sollte 2.6.1198.718 oder eine höhere Version angegeben sein. | N/V |


### Backup-Erweiterung

Sobald der VM-Agent auf dem virtuellen Computer installiert wurde, installiert der Azure Backup-Dienst die Sicherungserweiterung für den VM-Agent. Der Azure Backup-Dienst installiert nahtlos und ohne zusätzlichen Benutzereingriff Upgrades und Patches für die Backup-Erweiterung.

Die Sicherungserweiterung wird vom Sicherungsdienst installiert, unabhängig davon, ob der virtuelle Computer ausgeführt wird oder nicht. Bei einem ausgeführten virtuellen Computer besteht zudem die größte Chance auf einen anwendungskonsistenten Wiederherstellungspunkt. Der Azure Backup-Dienst setzt die Sicherung des virtuellen Computers jedoch auch dann fort, wenn dieser ausgeschaltet wurde und die Erweiterung nicht installiert werden konnte. Dies wird als Offline-VM bezeichnet. In diesem Fall ist der Wiederherstellungspunkt *absturzkonsistent*.

## Informationen zur Problembehandlung
Falls bei der Durchführung einiger Aufgaben in diesem Artikel Probleme auftreten, helfen Ihnen die [Anleitungen zur Problembehandlung](backup-azure-vms-troubleshoot.md) weiter.


## Fragen?
Wenn Sie Fragen haben oder Anregungen zu gewünschten Funktionen mitteilen möchten, [senden Sie uns Ihr Feedback](http://aka.ms/azurebackup_feedback).

<!---HONumber=AcomDC_0420_2016-->