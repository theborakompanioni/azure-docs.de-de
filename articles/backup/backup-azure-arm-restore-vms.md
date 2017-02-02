---
title: "Wiederherstellen virtueller Computer von einem Sicherungspunkt über das Azure-Portal | Microsoft-Dokumentation"
description: "Wiederherstellen eines virtuellen Azure-Computers mithilfe eines Wiederherstellungspunkts über das Azure-Portal"
services: backup
documentationcenter: 
author: markgalioto
manager: cfreeman
editor: 
keywords: Sicherung wiederherstellen; Wiederherstellungsschritte; Wiederherstellungspunkt;
ms.assetid: 372b87c6-3544-4dc5-bbc9-c742ca502159
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/09/2016
ms.author: trinadhk; jimpark;
translationtype: Human Translation
ms.sourcegitcommit: f13e9aca1715b839ed26495da91dd6c38ab0d06c
ms.openlocfilehash: 42d00e3e6058c7a2f88e3488923a1793c128f07e


---
# <a name="use-azure-portal-to-restore-virtual-machines"></a>Wiederherstellen virtueller Computer über das Azure-Portal
> [!div class="op_single_selector"]
> * [Wiederherstellen virtueller Computer im klassischen Portal](backup-azure-restore-vms.md)
> * [Wiederherstellen virtueller Computer im Azure-Portal](backup-azure-arm-restore-vms.md)
> 
> 

Schützen Sie Ihre Daten, indem Sie in festgelegten Abständen Momentaufnahmen Ihrer Daten erstellen. Diese Momentaufnahmen werden als Wiederherstellungspunkte bezeichnet. Sie werden in Recovery Services-Tresoren gespeichert. Wenn es erforderlich ist, einen virtuellen Computer zu reparieren oder wiederherzustellen, können Sie den virtuellen Computer aus einem der gespeicherten Wiederherstellungspunkte wiederherstellen. Beim Wiederherstellen eines Wiederherstellungspunkts setzen Sie den virtuellen Computer auf den Zustand zurück, in dem er sich beim Erstellen des Wiederherstellungspunkts befand. In diesem Artikel wird das Wiederherstellen eines virtuellen Computers erläutert.

> [!NOTE]
> Azure verfügt über zwei Bereitstellungsmodelle zum Erstellen und Verwenden von Ressourcen: [Resource Manager-Modell und klassisches Modell](../azure-resource-manager/resource-manager-deployment-model.md). Dieser Artikel enthält die Informationen und Verfahren für die Wiederherstellung virtueller Computer, die über das Resource Manager-Bereitstellungsmodell bereitgestellt werden.
> 
> 

## <a name="restore-a-recovery-point"></a>Wiederherstellen eines Wiederherstellungspunkts
1. Melden Sie sich beim [Azure-Portal](http://portal.azure.com/)
2. Klicken Sie im Azure-Menü auf **Durchsuchen**, und geben Sie in der Liste mit den Diensten **Recovery Services** ein. Die Liste der Dienste wird während der Eingabe angepasst. Wählen Sie **Recovery Services-Tresore**, wenn der Eintrag angezeigt wird.
   
    ![Öffnen des Recovery Services-Tresors](./media/backup-azure-arm-restore-vms/open-recovery-services-vault.png)
   
    Die Liste der Tresore im Abonnement wird angezeigt.
   
    ![Liste der Recovery Services-Tresore](./media/backup-azure-arm-restore-vms/list-of-rs-vaults.png)
3. Wählen Sie aus der Liste den Tresor aus, der dem virtuellen Computer zugeordnet ist, den Sie wiederherstellen möchten. Wenn Sie auf den Tresor klicken, wird das zugehörige Dashboard geöffnet.
   
    ![Liste der Recovery Services-Tresore](./media/backup-azure-arm-restore-vms/select-vault-open-vault-blade.png)
4. Gehen Sie im Tresordashboard folgendermaßen vor. Klicken Sie in der Kachel **Sicherungselemente** auf **Azure Virtual Machines**, um die dem Tresor zugeordneten virtuellen Computer anzuzeigen.
   
    ![Tresordashboard](./media/backup-azure-arm-restore-vms/vault-dashboard.png)
   
    Das Blatt **Sicherungselemente** wird geöffnet und zeigt die Liste der virtuellen Azure-Computer an.
   
    ![Liste der virtuellen Computer im Tresor](./media/backup-azure-arm-restore-vms/list-of-vms-in-vault.png)
5. Wählen Sie in der Liste einen virtuellen Computer aus, um auf das Dashboard zu öffnen. Das VM-Dashboard wird geöffnet und zeigt den Überwachungsbereich an, der die Kachel „Wiederherstellungspunkte“ enthält.
   
    ![Liste der virtuellen Computer im Tresor](./media/backup-azure-arm-restore-vms/vm-blade.png)
6. Klicken Sie im Dashboardmenü des virtuellen Computers auf **Wiederherstellen**
   
    ![Liste der virtuellen Computer im Tresor](./media/backup-azure-arm-restore-vms/vm-blade-menu-restore.png)
   
    Das Blatt „Wiederherstellen“ wird geöffnet.
   
    ![Blatt „Wiederherstellen“](./media/backup-azure-arm-restore-vms/restore-blade.png)
7. Klicken Sie auf dem Blatt **Wiederherstellen** auf **Wiederherstellungspunkt**, um das Blatt **Wiederherstellungspunkt auswählen** zu öffnen.
   
    ![Blatt „Wiederherstellen“](./media/backup-azure-arm-restore-vms/recovery-point-selector.png)
   
    Standardmäßig zeigt das Dialogfeld alle Wiederherstellungspunkte aus den letzten 30 Tagen an. Verwenden Sie die Option **Filter** , um den Zeitbereich der angezeigten Wiederherstellungspunkte zu ändern. Standardmäßig werden Wiederherstellungspunkte jeglicher Konsistenz angezeigt. Ändern Sie den Filter **Alle Wiederherstellungspunkte** , um eine bestimmte Konsistenz der Wiederherstellungspunkte auszuwählen. Weitere Informationen zu den einzelnen Typen von Wiederherstellungspunkten finden Sie in der Erläuterung unter [Datenkonsistenz](backup-azure-vms-introduction.md#data-consistency).  
   
   * **Konsistenz des Wiederherstellungspunkts** aus dieser Liste aus:
     * Absturzkonsistente Wiederherstellungspunkte
     * Anwendungskonsistente Wiederherstellungspunkte
     * Dateisystemkonsistente Wiederherstellungspunkte
     * Alle Wiederherstellungspunkte  
8. Wählen Sie einen Wiederherstellungspunkt, und klicken Sie auf **OK**.
   
    ![Wiederherstellungspunkt auswählen](./media/backup-azure-arm-restore-vms/select-recovery-point.png)
   
    Auf dem Blatt **Wiederherstellen** wird angezeigt, dass der Wiederherstellungspunkt festgelegt ist.
   
    ![Wiederherstellungspunkt wird festgelegt](./media/backup-azure-arm-restore-vms/recovery-point-set.png)
9. Auf dem Blatt **Wiederherstellen** wird **Konfiguration wiederherstellen** automatisch geöffnet, nachdem der Wiederherstellungspunkt festgelegt wurde.
   
    ![Assistent für die Wiederherstellungskonfiguration wird festgelegt](./media/backup-azure-arm-restore-vms/recovery-configuration-wizard.png)

## <a name="choosing-a-vm-restore-configuration"></a>Auswählen einer Konfiguration für die VM-Wiederherstellung
Da Sie jetzt den Wiederherstellungspunkt ausgewählt haben, wählen Sie jetzt eine Konfiguration für den wiederhergestellten virtuellen Computer. Es gibt zwei Möglichkeiten für die Konfiguration der wiederhergestellten VM: Azure-Portal oder PowerShell.

> [!NOTE]
> Das Portal bietet für wiederhergestellte VMs die Option „Schnellerfassung“. Wenn Sie die Konfiguration des wiederherzustellenden virtuellen Computers anpassen möchten, verwenden Sie PowerShell oder das Portal, um gesicherte Datenträger wiederherzustellen und sie mithilfe von PowerShell-Befehlen der Auswahl von Konfigurationen für virtuelle Computer hinzuzufügen. Weitere Informationen finden Sie unter [Wiederherstellen von VMs mit speziellen Netzwerkkonfigurationen](#restoring-vms-with-special-network-configurations).
> 
> 

1. Wechseln Sie zum Blatt **Wiederherstellen** , sofern Sie sich nicht bereits dort befinden. Stellen Sie sicher, dass ein **Wiederherstellungspunkt** ausgewählt wurde, und klicken Sie auf **Wiederherstellungskonfiguration**, um das Blatt **Wiederherstellungskonfiguration** zu öffnen.
   
    ![Assistent für die Wiederherstellungskonfiguration wird festgelegt](./media/backup-azure-arm-restore-vms/recovery-configuration-wizard.png)
2. Auf dem Blatt **Wiederherstellungskonfiguration** haben Sie zwei Möglichkeiten:

   * Wiederherstellen des gesamten virtuellen Computers
   * Wiederherstellen von gesicherten Datenträgern

   ### <a name="restore-full-virtual-machine"></a>Wiederherstellen des gesamten virtuellen Computers
   Geben Sie auf dem Blatt **Konfiguration wiederherstellen** Werte für jedes der folgenden Felder ein, bzw. wählen Sie sie aus:
   
   * **Wiederherstellungstyp**: Erstellen Sie den virtuellen Computer. 
   * **Name des virtuellen Computers** : Geben Sie einen Namen für den virtuellen Computer an. Der Name muss in der Ressourcengruppe (für eine durch den Resource Manager bereitgestellte VM) oder im Clouddienst (für eine klassische VM) eindeutig sein. Sie können die VM nicht ersetzen, wenn sie bereits im Abonnement vorhanden ist.
   * **Ressourcengruppe**: Verwenden Sie eine vorhandene Ressourcengruppe, oder erstellen Sie eine neue Ressourcengruppe. Wenn Sie einen klassischen virtuellen Computer wiederherstellen möchten, verwenden Sie dieses Feld, um den Namen eines neuen Clouddiensts anzugeben. Wenn Sie eine neue Ressourcengruppe/einen neuen Clouddienst erstellen, muss der Name global eindeutig sein. In der Regel wird der Name des Clouddiensts einer öffentlich zugänglichen URL zugeordnet, z. B. [cloudservice].cloudapp.net. Wenn Sie versuchen, einen Namen für die Cloudressourcengruppe/den Clouddienst zu verwenden, der bereits vorhanden ist, weist Azure der Ressourcengruppe/dem Clouddienst den gleichen Namen zu, den auch der virtuelle Computer verwendet. Azure zeigt Ressourcengruppen/Clouddienste und virtuelle Computer an, die keiner Affinitätsgruppe zugeordnet sind. Weitere Informationen finden Sie unter [Migrieren von Affinitätsgruppen zu einem regionalen virtuellen Netzwerk (VNET)](../virtual-network/virtual-networks-migrate-to-regional-vnet.md).
   * **Virtuelles Netzwerk** : Wählen Sie beim Erstellen des virtuellen Computers das virtuelle Netzwerk (VNET) aus. Das Feld enthält alle dem Abonnement zugeordneten VNETs. Die Ressourcengruppe des virtuellen Computers wird in Klammern angezeigt.
   * **Subnetz** : Wenn das VNET über Subnetze verfügt, ist das erste Subnetz standardmäßig ausgewählt. Wenn zusätzliche Subnetze vorhanden sind, wählen Sie das gewünschte Subnetz aus.
   * **Speicherkonto** : In diesem Menü werden die Speicherkonten aufgelistet, die sich am gleichen Standort wie der Recovery Services-Tresor befinden. Wählen Sie bei der Auswahl eines Speicherkontos ein Konto aus, das den gleichen Standort wie der Recovery Services-Tresor verwendet. Zonenredundante Speicherkonten werden nicht unterstützt. Wenn keine Speicherkonten mit dem gleichen Standort wie der Recovery Services-Tresor vorhanden sind, müssen Sie vor dem Starten des Wiederherstellungsvorgangs eins erstellen. Der Replikationstyp des Speicherkontos ist in Klammern aufgeführt.
     
     > [!NOTE]
     > Wenn Sie eine durch den Resource Manager bereitgestellte VM wiederherstellen möchten, müssen Sie ein virtuelles Netzwerk (VNET) identifizieren. Ein virtuelles Netzwerk (VNET) ist für eine klassische VM optional.
     > 
     > 
     
   ### <a name="restore-backed-up-disks"></a>Wiederherstellen von gesicherten Datenträgern
   Wenn Sie den virtuellen Computer, der von gesicherten Datenträgern wiederhergestellt werden soll, an die aktuellen Informationen auf dem Blatt „Wiederherstellungskonfiguration“ anpassen möchten, wählen Sie **Restore disks** (Wiederherstellungsdatenträger) als Wert für **Wiederherstellungstyp** aus. Diese Option erfordert ein Speicherkonto (es gelten die gleichen Einschränkungen wie bei der Auswahl eines Speicherkontos für das Wiederherstellen des gesamten virtuellen Computers), in das Datenträger von Sicherungen kopiert werden. Sie können [die wiederhergestellten Datenträger zum Anfügen an einen vorhandenen virtuellen Computer verwenden](../virtual-machines/virtual-machines-windows-attach-disk-portal.md) oder [einen neuen virtuellen Computer aus wiederhergestellten Datenträgern erstellen](./backup-azure-vms-automation.md#restore-an-azure-vm).

3. Klicken Sie auf dem Blatt **Wiederherstellungskonfiguration** auf **OK**, um die Wiederherstellungskonfiguration abzuschließen.
4. Klicken Sie auf dem Blatt **Wiederherstellen** auf **Wiederherstellen**, um den Wiederherstellungsvorgang auszulösen.
   
    ![Konfiguration der Wiederherstellung abgeschlossen](./media/backup-azure-arm-restore-vms/trigger-restore-operation.png)

## <a name="track-the-restore-operation"></a>Nachverfolgen des Wiederherstellungsvorgangs
Sobald Sie den Wiederherstellungsvorgang ausgelöst haben, erstellt der Backup-Dienst einen Auftrag zum Nachverfolgen des Wiederherstellungsvorgangs. Der Backupdienst erstellt außerdem die Benachrichtigung und zeigt sie vorübergehend im Benachrichtigungsbereich des Portals an. Wenn die Benachrichtigung nicht angezeigt wird, können Sie jederzeit auf das Benachrichtigungssymbol klicken, um Ihre Benachrichtigungen anzuzeigen.

![Wiederherstellung ausgelöst](./media/backup-azure-arm-restore-vms/restore-notification.png)

Um den Vorgang während seiner Verarbeitung oder nach seinem Abschluss anzuzeigen, öffnen Sie die Liste mit den Backup-Aufträgen.

1. Klicken Sie im Azure-Menü auf **Durchsuchen**, und geben Sie in der Liste mit den Diensten **Recovery Services** ein. Die Liste der Dienste wird während der Eingabe angepasst. Wählen Sie **Recovery Services-Tresore**, wenn der Eintrag angezeigt wird.
   
    ![Öffnen des Recovery Services-Tresors](./media/backup-azure-arm-restore-vms/open-recovery-services-vault.png)
   
    Die Liste der Tresore im Abonnement wird angezeigt.
   
    ![Liste der Recovery Services-Tresore](./media/backup-azure-arm-restore-vms/list-of-rs-vaults.png)
2. Wählen Sie aus der Liste den Tresor aus, der dem wiederhergestellten virtuellen Computer zugeordnet ist. Wenn Sie auf den Tresor klicken, wird das zugehörige Dashboard geöffnet.
3. Klicken Sie im Tresordashboard in der Kachel **Sicherungsaufträge** auf **Azure Virtual Machines**, um die dem Tresor zugeordneten virtuellen Computer anzuzeigen.
   
    ![Tresordashboard](./media/backup-azure-arm-restore-vms/vault-dashboard-jobs.png)
   
    Das Blatt **Sicherungsaufträge** wird geöffnet und zeigt die Liste der Aufträge an.
   
    ![Liste der virtuellen Computer im Tresor](./media/backup-azure-arm-restore-vms/restore-job-in-progress.png)
    
## <a name="post-restore-steps"></a>Schritte nach der Wiederherstellung
Bei Verwendung einer Cloud-Init-basierten Linux-Verteilung wie etwa Ubuntu wird das Kennwort aus Sicherheitsgründen nach der Wiederherstellung blockiert. Verwenden Sie zum [Zurücksetzen des Kennworts](../virtual-machines/virtual-machines-linux-classic-reset-access.md)die VMAccess-Erweiterung auf dem wiederhergestellten virtuellen Computer. Es wird empfohlen, SSH-Schlüssel für diese Verteilungen zu verwenden, um das Zurücksetzen des Kennworts nach der Wiederherstellung zu vermeiden. 

## <a name="backup-for-restored-vms"></a>Sicherung für wiederhergestellte virtuelle Computer
Wenn Sie den virtuellen Computer in derselben Ressourcengruppe mit dem Namen des ursprünglich gesicherten virtuellen Computers wiederhergestellt haben, wird der virtuelle Computer nach der Wiederherstellung weiterhin gesichert. Wenn Sie entweder den virtuellen Computer in einer anderen Ressourcengruppe wiederhergestellt oder einen anderen Namen dafür angegeben haben, wird er als neuer virtueller Computer behandelt, und Sie müssen die Sicherung dafür einrichten.

## <a name="restoring-a-vm-during-azure-datacenter-disaster"></a>Wiederherstellen eines virtuellen Computers während einer Azure-Datencenter-Notfallwiederherstellung
Mit Azure Backup können gesicherte virtuelle Computer im gekoppelten Datencenter wiederhergestellt werden, falls in dem primären Datencenter, in dem VMs ausgeführt werden, ein Notfall eingetreten ist, und Sie den Sicherungstresor als georedundant konfiguriert haben. Bei solchen Szenarien müssen Sie ein Speicherkonto auswählen, das im gekoppelten Datencenter vorhanden ist, ansonsten bleibt der Wiederherstellungsprozess gleich. Azure Backup verwendet den Computedienst des gekoppelten georedundanten Datencenters, um den wiederhergestellten virtuellen Computer zu erstellen. 

## <a name="restoring-vms-with-special-network-configurations"></a>Wiederherstellen von VMs mit speziellen Netzwerkkonfigurationen
Das Sichern und Wiederherstellen ist für virtuelle Computer mit den folgenden speziellen Netzwerkkonfigurationen möglich. Für diese Konfigurationen sind während des Wiederherstellungsvorgangs jedoch einige besondere Überlegungen anzustellen.

* VMs unter Lastenausgleich (intern und extern)
* VMs mit mehreren reservierten IP-Adressen
* VMs mit mehreren Netzwerkkarten (NICs)

> [!IMPORTANT]
> Beim Erstellen der speziellen Netzwerkkonfiguration für virtuelle Computer müssen Sie PowerShell verwenden, um VMs aus den wiederhergestellten Datenträgern zu erstellen.
> 
> 

Zum vollständigen Wiederherstellen der virtuellen Computer nach der Wiederherstellung von Datenträgern gehen Sie folgendermaßen vor:

1. Stellen Sie die Datenträger mit [PowerShell](backup-azure-vms-automation.md#restore-an-azure-vm) aus einem Recovery Services-Tresor wieder her.
2. Erstellen Sie die für den Lastenausgleich/mehrere NICs/mehrere reservierte IP-Adressen erforderliche VM-Konfiguration mithilfe der PowerShell-Cmdlets, und verwenden Sie sie zum Erstellen der VM mit der gewünschten Konfiguration.
   
   * Erstellen Sie die VM im Clouddienst mit [internem Load Balancer ](https://azure.microsoft.com/documentation/articles/load-balancer-internal-getstarted/)
   * Erstellen Sie die VM zum Herstellen einer Verbindung mit dem [Load Balancer mit Internetzugriff](https://azure.microsoft.com/en-us/documentation/articles/load-balancer-internet-getstarted/).
   * Erstellen Sie die VM mit [mehreren NICs](https://azure.microsoft.com/documentation/articles/virtual-networks-multiple-nics/)
   * Erstellen Sie die VM mit [mehreren reservierten IP-Adressen](https://azure.microsoft.com/documentation/articles/virtual-networks-reserved-public-ip/)

## <a name="next-steps"></a>Nächste Schritte
Jetzt können Sie Ihre virtuellen Computer wiederherstellen. Informationen zu häufig bei VMs auftretenden Fehlern finden Sie im Artikel zur Problembehandlung. Lesen Sie auch den Artikel zur Verwaltung von Aufgaben mit Ihren virtuellen Computern.

* [Problembehandlung](backup-azure-vms-troubleshoot.md#restore)
* [Verwalten virtueller Computer](backup-azure-manage-vms.md)




<!--HONumber=Dec16_HO2-->


