---
title: "Einführung: Schützen von Azure-VMs mit einem Recovery Services-Tresor | Microsoft Docs"
description: "Es wird beschrieben, wie Sie Azure-VMs mit einem Recovery Services-Tresor schützen. Verwenden Sie Sicherungen von mit Resource Manager bereitgestellten virtuellen Computern, klassischen bereitgestellten Computern und virtuellen Storage Premium-Computern, verschlüsselten virtuellen Computer und virtuellen Computern auf verwalteten Datenträgern, um Ihre Daten zu schützen. Erstellen und registrieren Sie einen Recovery Services-Tresor. Registrieren Sie VMs, erstellen Sie eine Richtlinie, und schützen Sie VMs in Azure."
services: backup
documentationcenter: 
author: markgalioto
manager: cfreeman
editor: 
keyword: backups; vm backup
ms.assetid: 45e773d6-c91f-4501-8876-ae57db517cd1
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 02/06/2017
ms.author: markgal; jimpark
translationtype: Human Translation
ms.sourcegitcommit: b73d5bb1f4f9e7adc65bf38a3c98db846a341cb3
ms.openlocfilehash: 2d8bedcf51cf55e13cc22d5a2d258218f0a786bb


---
# <a name="first-look-protect-azure-vms-with-a-recovery-services-vault"></a>Einführung: Schützen von Azure-VMs mit einem Recovery Services-Tresor
> [!div class="op_single_selector"]
> * [Schützen von VMs mit einem Recovery Services-Tresor](backup-azure-vms-first-look-arm.md)
> * [Schützen von VMs mit einem Sicherungstresor](backup-azure-vms-first-look.md)
>
>

In diesem Tutorial werden die Schritte zum Erstellen eines Recovery Services-Tresors und Sichern eines virtuellen Azure-Computers (VM) beschrieben. Recovery Services-Tresore schützen Folgendes:

* Mit Azure Resource Manager bereitgestellte virtuelle Computer
* Klassische virtuelle Computer
* Virtuelle Standardspeichercomputer
* Virtuelle Storage Premium-Computer 
* Auf verwalteten Datenträgern ausgeführte virtuelle Computer
* Per Azure Disk Encryption verschlüsselte VMs mit BEK und KEK

Weitere Informationen zum Schutz virtueller Storage Premium-Computer finden Sie unter [Sichern und Wiederherstellen virtueller Storage Premium-Computer](backup-introduction-to-azure-backup.md#using-premium-storage-vms-with-azure-backup). Weitere Informationen zur Unterstützung für virtuelle Computer auf verwalteten Datenträgern finden Sie unter [Sichern und Wiederherstellen von virtuellen Computern auf verwalteten Datenträgern](backup-introduction-to-azure-backup.md#using-managed-disk-vms-with-azure-backup).

> [!NOTE]
> Dieses Tutorial setzt voraus, dass Sie in Ihrem Azure-Abonnement bereits einen virtuellen Computer verwenden und die entsprechenden Schritte durchgeführt haben, um dem Backup-Dienst den Zugriff auf den virtuellen Computer zu erlauben.
>
>

[!INCLUDE [learn-about-Azure-Backup-deployment-models](../../includes/backup-deployment-models.md)]

Basierend auf der Anzahl von zu schützenden VMs können Sie von unterschiedlichen Ausgangspunkten aus beginnen. Wenn Sie mehrere virtuelle Computer in einem einzelnen Vorgang sichern möchten, können Sie zum Recovery Services-Tresor navigieren und die Sicherung über das Tresordashboard starten. Falls Sie eine einzelne VM sichern möchten, können Sie die Sicherung direkt über das Blatt für die VM-Verwaltung durchführen.

## <a name="configure-backup-from-vm-management-blade"></a>Konfigurieren der Sicherung über das Blatt „VM-Verwaltung“
1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com/) an.
2. Klicken Sie im Menü „Hub“ auf **Weitere Dienste**, und geben Sie in der Liste mit den Ressourcen **Virtuelle Computer** ein.  Die Liste mit den virtuellen Computern wird angezeigt. Wählen Sie in der Liste mit den virtuellen Computern einen virtuellen Computer aus, den Sie sichern möchten. Das Blatt für die VM-Verwaltung wird geöffnet.
 ![Blatt „VM-Verwaltung“](./media/backup-azure-vms-first-look-arm/vm-management-blade.png)

3. Klicken Sie auf dem Blatt „VM-Verwaltung“ links unter „Einstellungen“ auf die Option „Sicherung“.
![Option „Sicherung“ auf dem Blatt „VM-Verwaltung“](./media/backup-azure-vms-first-look-arm/backup-option-vm-management-blade.png)

4. Das Blatt „Sicherung aktivieren“ wird geöffnet. Auf diesem Blatt werden zwei Eingaben erwartet: Recovery Services-Tresor: Eine Azure Backup-Ressource, die zum Speichern von Sicherungen der VMs verwendet wird. Sicherungsrichtlinie: Mit der Sicherungsrichtlinie werden der Zeitplan für die Sicherungen und die Aufbewahrungsdauer für Sicherungskopien angegeben. Dieses Blatt verfügt über Standardeinstellungen. Sie können die Standardeinstellungen je nach Ihren Sicherungsanforderungen anpassen.

  ![Aktivieren des Sicherungs-Assistenten](./media/backup-azure-vms-first-look-arm/vm-blade-enable-backup.png)

5. Für den Recovery Services-Tresor können Sie einen vorhandenen Tresor auswählen oder einen neuen erstellen. Bei einer Neuerstellung wird der Tresor in derselben Ressourcengruppe und an demselben Standort wie für den virtuellen Computer erstellt. Gehen Sie wie folgt vor, wenn Sie einen Recovery Services-Tresor mit anderen Werten erstellen möchten: [Erstellen Sie einen Recovery Services-Tresor](backup-azure-vms-first-look-arm.md#create-a-recovery-services-vault-for-a-vm), bevor Sie in Schritt&3; auf die Option „Sicherung“ klicken, und treffen Sie auf diesem Blatt die entsprechende Auswahl.

6. Wählen Sie auf dem Blatt „Sicherungsrichtlinie“ die Sicherungsrichtlinie aus, die Sie auf den Tresor anwenden möchten, und klicken Sie auf **OK**.
    ![Auswählen der Sicherungsrichtlinie](./media/backup-azure-vms-first-look-arm/setting-rs-backup-policy-new.png)

    In den Details werden die Informationen zur Standardrichtlinie aufgeführt. Wählen Sie im Dropdownmenü die Option **Neu erstellen** , wenn Sie eine Richtlinie erstellen möchten. Im Dropdownmenü steht auch eine Option zur Verfügung, mit der die Zeit der Momentaufnahmenerstellung festgelegt werden kann. Eine Anleitung zum Definieren einer Sicherungsrichtlinie finden Sie unter [Definieren einer Sicherungsrichtlinie](backup-azure-vms-first-look-arm.md#defining-a-backup-policy). Nachdem Sie auf **OK** geklickt haben, wird die Sicherungsrichtlinie dem virtuellen Computer zugeordnet.

7. Klicken Sie auf „Sicherung aktivieren“, um die Sicherung auf dem virtuellen Computer zu konfigurieren. Hierdurch wird eine Bereitstellung ausgelöst.
![Schaltfläche „Sicherung aktivieren“](./media/backup-azure-vms-first-look-arm/vm-management-blade-enable-backup-button.png)

8. Sie können den Status der Konfiguration mithilfe von Benachrichtigungen nachverfolgen.
![Benachrichtigung zu „Sicherung aktivieren“](./media/backup-azure-vms-first-look-arm/vm-management-blade-enable-backup-notification.png)

9. Nachdem die Bereitstellung für „Sicherung konfigurieren“ abgeschlossen ist, gelangen Sie beim Klicken auf die Option „Sicherung“ auf dem Blatt „VM-Verwaltung“ zum Blatt „Sicherungselement“ für die gesicherte VM.
![VM-Ansicht „Sicherungselement“](./media/backup-azure-vms-first-look-arm/backup-item-view.png)

## <a name="configure-backup-from-recovery-services-vault-view"></a>Konfigurieren der Sicherung über die Ansicht „Recovery Services-Tresor“
Dies sind die allgemeinen Schritte, die auszuführen sind:  

1. Erstellen Sie einen Recovery Services-Tresor für eine VM.
2. Verwenden Sie das Azure-Portal, um ein Szenario zu wählen, die Richtlinie festzulegen und zu schützende Elemente zu ermitteln.
3. Führen Sie die erste Sicherung durch.

## <a name="create-a-recovery-services-vault-for-a-vm"></a>Erstellen eines Recovery Services-Tresor für eine VM
Bei einem Recovery Services-Tresor handelt es sich um eine Entität, in der alle Sicherungen und Wiederherstellungspunkte gespeichert werden, die im Laufe der Zeit erstellt wurden. Der Recovery Services-Tresor enthält auch die Sicherungsrichtlinie, die auf die geschützten VMs angewendet wird.

> [!NOTE]
> Das Sichern von VMs ist ein lokaler Vorgang. Es ist nicht möglich, VMs eines Standorts in einem Recovery Services-Tresor an einem anderen Standort zu sichern. An jedem Azure-Standort, der über zu sichernde VMs verfügt, muss also mindestens ein Recovery Services-Tresor vorhanden sein.
>
>

So erstellen Sie einen Recovery Services-Tresor:

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com/)an.
2. Klicken Sie im Hub-Menü auf **Durchsuchen**, und geben Sie in der Liste mit den Ressourcen **Recovery Services** ein. Sobald Sie mit der Eingabe beginnen, wird die Liste auf der Grundlage Ihrer Eingabe gefiltert. Klicken Sie auf **Recovery Services-Tresor**.

    ![Erstellen eines Recovery Services-Tresors – Schritt 1](./media/backup-azure-vms-first-look-arm/browse-to-rs-vaults.png) <br/>

    Die Liste mit den Recovery Services-Tresoren wird angezeigt.
3. Klicken Sie im Menü **Recovery Services-Tresore** auf **Hinzufügen**.

    ![Erstellen eines Recovery Services-Tresors – Schritt 2](./media/backup-azure-vms-first-look-arm/rs-vault-menu.png)

    Das Blatt „Recovery Services-Tresor“ wird geöffnet, und Sie werden aufgefordert, **Name**, **Abonnement**, **Ressourcengruppe** und **Standort** anzugeben.

    ![Erstellen eines Recovery Services-Tresors – Schritt 5](./media/backup-azure-vms-first-look-arm/rs-vault-attributes.png)
4. Geben Sie unter **Name**einen Anzeigenamen für den Tresor ein. Der Name muss für das Azure-Abonnement eindeutig sein. Geben Sie einen Namen ein, der zwischen 2 und 50 Zeichen enthält. Er muss mit einem Buchstaben beginnen und darf nur Buchstaben, Zahlen und Bindestriche enthalten.
5. Klicken Sie auf **Abonnement** , um die Liste mit den verfügbaren Abonnements anzuzeigen. Falls Sie nicht sicher sind, welches Abonnement geeignet ist, können Sie das Standardabonnement bzw. das vorgeschlagene Abonnement verwenden. Es sind nur dann mehrere Auswahlmöglichkeiten verfügbar, wenn Ihr Organisationskonto mehreren Azure-Abonnements zugeordnet ist.
6. Klicken Sie auf **Ressourcengruppe**, um die Liste mit den verfügbaren Ressourcengruppen anzuzeigen, oder klicken Sie auf **Neu**, um eine Ressourcengruppe zu erstellen. Weitere Informationen zu Ressourcengruppen finden Sie unter [Übersicht über den Azure Resource Manager](../azure-resource-manager/resource-group-overview.md).
7. Klicken Sie auf **Standort** , um die geografische Region für den Tresor auszuwählen. Der Tresor **muss** sich in derselben Region wie die zu schützenden virtuellen Computer befinden.

   > [!IMPORTANT]
   > Wenn Sie sich nicht sicher sind, an welchem Standort sich Ihr virtueller Computer befindet, schließen Sie das Dialogfeld zur Tresorerstellung, und wechseln Sie zur Liste der virtuellen Computer im Portal. Wenn Sie über virtuelle Computer in mehreren Regionen verfügen, sollten Sie in jeder Region einen Recovery Services-Tresor erstellen. Erstellen Sie den Tresor am ersten Standort, bevor Sie mit dem nächsten Standort fortfahren. Es müssen keine Speicherkonten zum Speichern der Sicherungsdaten angegeben werden. Dies erfolgt automatisch über den Recovery Services-Tresor und den Azure Backup-Dienst.
   >
   >
8. Klicken Sie auf **Erstellen**. Es kann einige Zeit dauern, bis der Recovery Services-Tresor erstellt wurde. Verfolgen Sie die Benachrichtigungen oben rechts im Portal. Nach Abschluss des Erstellungsvorgangs wird der Tresor in der Liste mit den Recovery Services-Tresoren angezeigt.

    ![Liste der Sicherungstresore](./media/backup-azure-vms-first-look-arm/rs-list-of-vaults.png)

Nachdem Sie Ihren Tresor erstellt haben, widmen wir uns nun dem Festlegen der Speicherreplikation.

### <a name="set-storage-replication"></a>Festlegen der Speicherreplikation
Bei der Speicherreplikation haben Sie die Wahl zwischen georedundantem Speicher und lokal redundantem Speicher. Standardmäßig verfügt Ihr Tresor über einen georedundanten Speicher. Behalten Sie den georedundanten Speicher bei, wenn es sich hierbei um Ihre primäre Sicherung handelt. Wählen Sie lokal redundanten Speicher, wenn Sie eine günstigere und weniger langfristige Option wünschen. Weitere Informationen zu den Optionen für [georedundanten](../storage/storage-redundancy.md#geo-redundant-storage) und [lokal redundanten](../storage/storage-redundancy.md#locally-redundant-storage) Speicher finden Sie in der [Übersicht über die Azure Storage-Replikation](../storage/storage-redundancy.md).

So bearbeiten Sie die Einstellung für die Speicherreplikation:

1. Wählen Sie Ihren Tresor aus, um das Tresordashboard und das Blatt „Einstellungen“ zu öffnen. Sollte das Blatt **Einstellungen** nicht geöffnet werden, klicken Sie im Tresordashboard auf **Alle Einstellungen**.
2. Klicken Sie auf dem Blatt **Einstellungen** auf **Sicherungsinfrastruktur** > **Sicherungskonfiguration**, um das Blatt **Sicherungskonfiguration** zu öffnen. Wählen Sie auf dem Blatt **Speicherkonfiguration** die Speicherreplikationsoption für Ihren Tresor aus.

    ![Liste der Sicherungstresore](./media/backup-azure-vms-first-look-arm/choose-storage-configuration-rs-vault.png)

    Wenn Sie die Speicheroption für Ihren Tresor ausgewählt haben, können Sie den virtuellen Computer dem Tresor zuordnen. Ermitteln und registrieren Sie die virtuellen Azure-Computer, um mit der Zuordnung zu beginnen.

## <a name="select-a-backup-goal-set-policy-and-define-items-to-protect"></a>Auswählen eines Sicherungsziels, Festlegen der Richtlinie und Definieren von zu schützenden Elementen
Führen Sie zunächst den Ermittlungsprozess durch, bevor Sie einen virtuellen Computer registrieren, um sicherzustellen, dass alle neuen virtuellen Computer, die dem Abonnement hinzugefügt wurden, identifiziert werden. Bei diesem Vorgang wird Azure nach der Liste virtueller Computer im Abonnement abgefragt. Außerdem werden zusätzliche Informationen wie der Clouddienstname und die Region erfasst. „Szenario“ bezeichnet im Kontext des Azure-Portals das, was Sie dem Recovery Services-Tresor hinzufügen möchten. Die Richtlinie gibt die Häufigkeit und den Zeitpunkt für die Erstellung von Wiederherstellungspunkten an. Darüber hinaus enthält die Richtlinie die Beibehaltungsdauer für die Wiederherstellungspunkte.

1. Falls Sie bereits über einen geöffneten Recovery Services-Tresor verfügen, können Sie mit Schritt 2 fortfahren. Wenn Sie keinen Recovery Services-Tresor geöffnet haben und sich im Azure-Portal befinden, klicken Sie im Hub-Menü auf **Durchsuchen**.

   * Geben Sie in der Liste mit den Ressourcen **Recovery Services**ein.
   * Sobald Sie mit der Eingabe beginnen, wird die Liste auf der Grundlage Ihrer Eingabe gefiltert. Wenn **Recovery Services-Tresore**angezeigt wird, klicken Sie auf den Eintrag.

     ![Erstellen eines Recovery Services-Tresors – Schritt 1](./media/backup-azure-vms-first-look-arm/browse-to-rs-vaults.png) <br/>

     Die Liste mit den Recovery Services-Tresoren wird angezeigt.
   * Wählen Sie in der Liste mit den Recovery Services-Tresoren einen Tresor aus.

     Das ausgewählte Tresor-Dashboard wird geöffnet.

     ![Tresorblatt öffnen](./media/backup-azure-vms-first-look-arm/vault-settings.png)
2. Klicken Sie im Menü des Tresordashboards auf **Sicherung** , um das Blatt „Sicherung“ zu öffnen.

    ![Blatt „Sicherung“ öffnen](./media/backup-azure-vms-first-look-arm/backup-button.png)

    Nachdem das Blatt geöffnet wurde, sucht der Backup-Dienst nach neuen virtuellen Computern im Abonnement.

    ![VMs ermitteln](./media/backup-azure-vms-first-look-arm/discovering-new-vms.png)
3. Klicken Sie auf dem Blatt „Sicherung“ auf **Sicherungsziel** , um das gleichnamige Blatt zu öffnen.

    ![Blatt „Szenario“ öffnen](./media/backup-azure-vms-first-look-arm/select-backup-goal-one.png)
4. Legen Sie auf dem Blatt für das Sicherungsziel die Option **Wo wird Ihre Workload ausgeführt?** auf „Azure“ und die Option **Was möchten Sie sichern?** auf „Virtueller Computer“ fest, und klicken Sie anschließend auf **OK**.

    Das Blatt für das Sicherungsziel wird geschlossen, und das Blatt „Sicherungsrichtlinie“ wird geöffnet.

    ![Blatt „Szenario“ öffnen](./media/backup-azure-vms-first-look-arm/select-backup-goal-two.png)
5. Wählen Sie auf dem Blatt „Sicherungsrichtlinie“ die Sicherungsrichtlinie aus, die Sie auf den Tresor anwenden möchten, und klicken Sie auf **OK**.

    ![Sicherungsrichtlinie auswählen](./media/backup-azure-vms-first-look-arm/setting-rs-backup-policy-new.png)

    In den Details werden die Informationen zur Standardrichtlinie aufgeführt. Wählen Sie im Dropdownmenü die Option **Neu erstellen** , wenn Sie eine Richtlinie erstellen möchten. Im Dropdownmenü steht auch eine Option zur Verfügung, mit der die Zeit der Momentaufnahmenerstellung auf 19 Uhr festgelegt werden kann. Eine Anleitung zum Definieren einer Sicherungsrichtlinie finden Sie unter [Definieren einer Sicherungsrichtlinie](backup-azure-vms-first-look-arm.md#defining-a-backup-policy). Nachdem Sie auf **OK**geklickt haben, wird die Sicherungsrichtlinie dem Tresor zugeordnet.

    Wählen Sie als Nächstes die VMs aus, die dem Tresor zugeordnet werden sollen.
6. Wählen Sie die virtuellen Computer aus, die der angegebenen Richtlinie zugeordnet werden sollen, und klicken Sie auf **Auswählen**.

    ![Workload auswählen](./media/backup-azure-vms-first-look-arm/select-vms-to-backup-new.png)

    Sollte der gewünschte virtuelle Computer nicht angezeigt werden, überprüfen Sie, ob er sich am gleichen Azure-Standort befindet wie der Recovery Services-Tresor.
7. Nachdem Sie nun alle Einstellungen für den Tresor definiert haben, klicken Sie unten auf dem Blatt „Sicherung“ auf **Sicherung aktivieren** . Die Richtlinie wird im Tresor und auf den VMs bereitgestellt.

    ![Sicherung aktivieren](./media/backup-azure-vms-first-look-arm/enable-backup-settings-new.png)

## <a name="initial-backup"></a>Erste Sicherung
Nachdem eine Sicherungsrichtlinie auf dem virtuellen Computer bereitgestellt wurde, bedeutet dies nicht, dass die Daten gesichert wurden. Die erste geplante Sicherung (gemäß Definition in der Sicherungsrichtlinie) ist die erste Sicherung. Bis zur Durchführung der ersten Sicherung wird der Status der letzten Sicherung auf dem Blatt **Sicherungsaufträge** als **Warnung (anfängliche Sicherung steht aus)** angezeigt.

![Sicherung ausstehend](./media/backup-azure-vms-first-look-arm/initial-backup-not-run.png)

Falls die erste Sicherung nicht ohnehin in Kürze durchgeführt wird, empfiehlt es sich, **Jetzt sichern**auszuführen.

So führen Sie **Jetzt sichern**aus

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
4. Im Blatt „Sicherungsaufträge“ wird der Status aller Aufträge angezeigt.

    ![Kachel „Sicherungsaufträge“](./media/backup-azure-vms-first-look-arm/backup-jobs-in-jobs-view.png)

   > [!NOTE]
   > Als Teil des Sicherungsvorgangs gibt der Azure Backup-Dienst einen Befehl an die Sicherungserweiterung auf jeder VM aus, damit alle Schreibvorgänge geleert werden und eine konsistente Momentaufnahme erstellt wird.
   >
   >

    Wenn der Sicherungsauftrag abgeschlossen ist, lautet der Status *Abgeschlossen*.

[!INCLUDE [backup-create-backup-policy-for-vm](../../includes/backup-create-backup-policy-for-vm.md)]

## <a name="install-the-vm-agent-on-the-virtual-machine"></a>Installieren des VM-Agents auf dem virtuellen Computer
Diese Informationen werden bereitgestellt, falls es erforderlich ist. Der Azure VM-Agent muss auf dem virtuellen Azure-Computer installiert werden, damit die Sicherungserweiterung funktioniert. Wenn Ihr virtueller Computer aber über den Azure-Katalog erstellt wurde, ist der VM-Agent auf dem virtuellen Computer bereits vorhanden. Auf virtuellen Computern, die aus lokalen Rechenzentren migriert werden, ist der VM-Agent nicht installiert. In diesem Fall muss der VM-Agent installiert werden. Falls beim Sichern des virtuellen Azure-Computers Probleme auftreten, müssen Sie sicherstellen, dass der Azure VM-Agent auf dem virtuellen Computer korrekt installiert ist (s. Tabelle unten). Wenn Sie einen benutzerdefinierten virtuellen Computer erstellen, [müssen Sie das Kontrollkästchen **VM-Agent installieren** aktivieren](../virtual-machines/virtual-machines-windows-classic-agents-and-extensions.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json), bevor Sie den virtuellen Computer bereitstellen.

Erfahren Sie mehr über den [VM-Agent](https://go.microsoft.com/fwLink/?LinkID=390493&clcid=0x409) und seine [Installation](../virtual-machines/virtual-machines-windows-classic-manage-extensions.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json) (in englischer Sprache).

Die folgende Tabelle enthält weitere Informationen zum VM-Agent für virtuelle Windows- und Linux-Computer.

| **Vorgang** | **Windows** | **Linux** |
| --- | --- | --- |
| Installieren des VM-Agent |<li>Laden Sie den [Agent-MSI](http://go.microsoft.com/fwlink/?LinkID=394789&clcid=0x409)herunter, und installieren Sie ihn. Zum Durchführen der Installation benötigen Sie Administratorberechtigungen. <li>[Aktualisieren Sie die VM-Eigenschaft](http://blogs.msdn.com/b/mast/archive/2014/04/08/install-the-vm-agent-on-an-existing-azure-vm.aspx) , um anzugeben, dass der Agent installiert wurde. |<li> Installieren Sie den neuesten [Linux-Agent](https://github.com/Azure/WALinuxAgent) aus GitHub. Zum Durchführen der Installation benötigen Sie Administratorberechtigungen. <li> [Aktualisieren Sie die VM-Eigenschaft](http://blogs.msdn.com/b/mast/archive/2014/04/08/install-the-vm-agent-on-an-existing-azure-vm.aspx) , um anzugeben, dass der Agent installiert wurde. |
| Aktualisieren des VM-Agents |Das Aktualisieren des VM-Agents ist so einfach wie das Neuinstallieren der [Binärdateien für den VM-Agent](http://go.microsoft.com/fwlink/?LinkID=394789&clcid=0x409). <br>Stellen Sie sicher, dass kein Sicherungsvorgang ausgeführt wird, während der VM-Agent aktualisiert wird. |Folgen Sie den Anweisungen unter [Aktualisieren des Linux-VM-Agents ](../virtual-machines/virtual-machines-linux-update-agent.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). <br>Stellen Sie sicher, dass kein Sicherungsvorgang ausgeführt wird, während der VM-Agent aktualisiert wird. |
| Überprüfen der VM-Agent-Installation |<li>Navigieren Sie auf dem virtuellen Azure-Computer zum Ordner *C:\WindowsAzure\Packages*. <li>Dieser Ordner enthält die Datei "WaAppAgent.exe".<li> Klicken Sie mit der rechten Maustaste auf die Datei, wechseln Sie zu **Eigenschaften**, und wählen Sie dann die Registerkarte **Details** aus. Im Feld mit der Produktversion sollte 2.6.1198.718 oder eine höhere Version angegeben sein. |N/V |

### <a name="backup-extension"></a>Backup-Erweiterung
Sobald der VM-Agent auf dem virtuellen Computer installiert wurde, installiert der Azure Backup-Dienst die Sicherungserweiterung für den VM-Agent. Der Azure Backup-Dienst installiert nahtlos und ohne zusätzlichen Benutzereingriff Upgrades und Patches für die Backup-Erweiterung.

Die Sicherungserweiterung wird vom Backup-Dienst installiert, und zwar unabhängig davon, ob der virtuelle Computer ausgeführt wird. Bei einem ausgeführten virtuellen Computer besteht zudem die größte Chance auf einen anwendungskonsistenten Wiederherstellungspunkt. Der Azure Backup-Dienst setzt die Sicherung des virtuellen Computers jedoch auch dann fort, wenn dieser ausgeschaltet wurde und die Erweiterung nicht installiert werden konnte. Dies wird als Offline-VM bezeichnet. In diesem Fall ist der Wiederherstellungspunkt *absturzkonsistent*.

## <a name="troubleshooting-information"></a>Informationen zur Problembehandlung
Falls bei der Durchführung einiger Aufgaben in diesem Artikel Probleme auftreten, helfen Ihnen die [Anleitungen zur Problembehandlung](backup-azure-vms-troubleshoot.md)weiter.

## <a name="pricing"></a>Preise
Der Preis für die Sicherung von virtuellen Azure-Computern wird auf Grundlage der geschützten Instanzen berechnet. Weitere Informationen zu [Preisen für die Sicherung](https://azure.microsoft.com/pricing/details/backup/)

## <a name="questions"></a>Fragen?
Wenn Sie Fragen haben oder Anregungen zu gewünschten Funktionen mitteilen möchten, [senden Sie uns Ihr Feedback](http://aka.ms/azurebackup_feedback).



<!--HONumber=Feb17_HO2-->


