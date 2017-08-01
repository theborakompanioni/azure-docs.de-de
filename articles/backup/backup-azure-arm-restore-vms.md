---
title: "Azure Backup: Wiederherstellen virtueller Computer über das Azure-Portal | Microsoft-Dokumentation"
description: "Wiederherstellen eines virtuellen Azure-Computers mithilfe eines Wiederherstellungspunkts über das Azure-Portal"
services: backup
documentationcenter: 
author: markgalioto
manager: carmonm
editor: 
keywords: Sicherung wiederherstellen; Wiederherstellungsschritte; Wiederherstellungspunkt;
ms.assetid: 372b87c6-3544-4dc5-bbc9-c742ca502159
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 3/12/2017
ms.author: markgal;trinadhk;
ms.translationtype: Human Translation
ms.sourcegitcommit: ef1e603ea7759af76db595d95171cdbe1c995598
ms.openlocfilehash: 2ab86ed8aafb01e97b3ac9ba0411f4b80f88ac5b
ms.contentlocale: de-de
ms.lasthandoff: 06/16/2017


---
# <a name="use-azure-portal-to-restore-virtual-machines"></a>Wiederherstellen virtueller Computer über das Azure-Portal
> [!div class="op_single_selector"]
> * [Wiederherstellen virtueller Computer im klassischen Portal](backup-azure-restore-vms.md)
> * [Wiederherstellen virtueller Computer im Azure-Portal](backup-azure-arm-restore-vms.md)
>
>

Schützen Sie Ihre Daten, indem Sie in festgelegten Abständen Momentaufnahmen Ihrer Daten erstellen. Diese Momentaufnahmen werden als Wiederherstellungspunkte bezeichnet. Sie werden in Recovery Services-Tresoren gespeichert. Wenn es erforderlich ist, einen virtuellen Computer zu reparieren oder wiederherzustellen, können Sie den virtuellen Computer aus einem der gespeicherten Wiederherstellungspunkte wiederherstellen. Wenn Sie einen Wiederherstellungspunkt wiederherstellen, können Sie einen neuen virtuellen Computer erstellen, der den gesicherten virtuellen Computer zu einem bestimmten Zeitpunkt darstellt. Sie können aber auch Datenträger wiederherstellen und die zugehörige Vorlage verwenden, um den wiederhergestellten virtuellen Computer anzupassen oder einzelne Dateien wiederherzustellen. In diesem Artikel wird erläutert, wie ein virtueller Computer als neuer virtueller Computer wiederhergestellt wird oder alle gesicherten Datenträger wiederhergestellt werden. Informationen zur Wiederherstellung einzelner Dateien finden Sie unter [Wiederherstellen von Dateien aus Azure-VM-Sicherungen](backup-azure-restore-files-from-vm.md)

![3-ways-restore-from-vm-backup](./media/backup-azure-arm-restore-vms/azure-vm-backup-restore.png)

> [!NOTE]
> Azure verfügt über zwei Bereitstellungsmodelle zum Erstellen und Verwenden von Ressourcen: [Resource Manager-Modell und klassisches Modell](../azure-resource-manager/resource-manager-deployment-model.md). Dieser Artikel enthält die Informationen und Verfahren für die Wiederherstellung virtueller Computer, die über das Resource Manager-Bereitstellungsmodell bereitgestellt werden.
>
>

Das Wiederherstellen eines virtuellen Computers oder aller Datenträger aus einer VM-Sicherung umfasst zwei Schritte:

1. Auswählen eines Wiederherstellungspunkts für die Wiederherstellung
2. Auswählen des Wiederherstellungstyps: Erstellen eines neuen virtuellen Computers oder Wiederherstellen eines Datenträgers und Angeben erforderlicher Parameter. 

## <a name="select-restore-point-for-restore"></a>Auswählen eines Wiederherstellungspunkts für die Wiederherstellung
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

## <a name="choosing-a-vm-restore-configuration"></a>Auswählen einer Konfiguration für die VM-Wiederherstellung
Da Sie jetzt den Wiederherstellungspunkt ausgewählt haben, wählen Sie jetzt eine Konfiguration für den wiederhergestellten virtuellen Computer. Es gibt zwei Möglichkeiten für die Konfiguration der wiederhergestellten VM: Azure-Portal oder PowerShell.

1. Wechseln Sie zum Blatt **Wiederherstellen** , sofern Sie sich nicht bereits dort befinden. Stellen Sie sicher, dass ein [Wiederherstellungspunkt ausgewählt wurde](#select-restore-point-for-restore), und klicken Sie auf **Wiederherstellungskonfiguration**, um das Blatt **Wiederherstellungskonfiguration** zu öffnen.

    ![Assistent für die Wiederherstellungskonfiguration wird festgelegt](./media/backup-azure-arm-restore-vms/recovery-configuration-wizard-recovery-type.png)
2. Auf dem Blatt **Wiederherstellungskonfiguration** haben Sie zwei Möglichkeiten:
   * Wiederherstellen des gesamten virtuellen Computers
   * Wiederherstellen von gesicherten Datenträgern

Das Portal bietet für wiederhergestellte VMs die Option „Schnellerfassung“. Wenn Sie die VM-Konfiguration oder Namen der Ressourcen anpassen möchten, die beim Erstellen einer neuen VM-Auswahl erstellt wurden, verwenden Sie PowerShell oder das Portal zum Wiederherstellen gesicherter Datenträger, und verwenden Sie PowerShell-Befehle, um sie der Auswahl der VM-Konfiguration anzufügen. Alternativ können Sie eine Vorlage verwenden, die zum Wiederherstellungsdatenträger gehört, um den wiederhergestellten virtuellen Computer anzupassen. Weitere Informationen zum Wiederherstellen von virtuellen Computern mit mehreren Netzwerkkarten oder Lastenausgleichsmodulen finden Sie unter [Wiederherstellen eines virtuellen Computers mit speziellen Netzwerkkonfigurationen](#restoring-vms-with-special-network-configurations). Wenn Ihre Windows-VM [HUB-Lizenzierung](../virtual-machines/windows/hybrid-use-benefit-licensing.md) verwendet, müssen Sie wie nachfolgend angegeben Datenträger wiederherstellen und mithilfe von PowerShell bzw. einer Vorlage die VM erstellen. Stellen Sie dabei sicher, dass Sie beim Erstellen der VM für „LicenseType“ den Wert „Windows_Server“ festlegen, um von den verfügbaren HUB-Vorteilen auf einer wiederhergestellten VM zu profitieren. 
 
## <a name="create-a-new-vm-from-restore-point"></a>Erstellen eines neuen virtuellen Computer über einen Wiederherstellungspunkt
Falls noch nicht geschehen, [wählen Sie einen Wiederherstellungspunkt aus](#restoring-vms-with-special-network-configurations), bevor Sie mit der Erstellung eines neuen virtuellen Computers beginnen. Sobald der Wiederherstellungspunkt ausgewählt wurde, geben Sie auf dem Blatt **Konfiguration wiederherstellen** Werte für jedes der folgenden Felder ein, bzw. wählen Sie sie aus:

* **Wiederherstellungstyp**: Erstellen Sie den virtuellen Computer.
* **Name des virtuellen Computers** : Geben Sie einen Namen für den virtuellen Computer an. Der Name muss in der Ressourcengruppe (für eine durch den Resource Manager bereitgestellte VM) oder im Clouddienst (für eine klassische VM) eindeutig sein. Sie können die VM nicht ersetzen, wenn sie bereits im Abonnement vorhanden ist.
* **Ressourcengruppe**: Verwenden Sie eine vorhandene Ressourcengruppe, oder erstellen Sie eine neue Ressourcengruppe. Wenn Sie einen klassischen virtuellen Computer wiederherstellen möchten, verwenden Sie dieses Feld, um den Namen eines neuen Clouddiensts anzugeben. Wenn Sie eine neue Ressourcengruppe/einen neuen Clouddienst erstellen, muss der Name global eindeutig sein. In der Regel wird der Name des Clouddiensts einer öffentlich zugänglichen URL zugeordnet, z. B. [cloudservice].cloudapp.net. Wenn Sie versuchen, einen Namen für die Cloudressourcengruppe/den Clouddienst zu verwenden, der bereits vorhanden ist, weist Azure der Ressourcengruppe/dem Clouddienst den gleichen Namen zu, den auch der virtuelle Computer verwendet. Azure zeigt Ressourcengruppen/Clouddienste und virtuelle Computer an, die keiner Affinitätsgruppe zugeordnet sind. Weitere Informationen finden Sie unter [Migrieren von Affinitätsgruppen zu einem regionalen virtuellen Netzwerk (VNET)](../virtual-network/virtual-networks-migrate-to-regional-vnet.md).
* **Virtuelles Netzwerk** : Wählen Sie beim Erstellen des virtuellen Computers das virtuelle Netzwerk (VNET) aus. Das Feld enthält alle dem Abonnement zugeordneten VNETs. Die Ressourcengruppe des virtuellen Computers wird in Klammern angezeigt.
* **Subnetz** : Wenn das VNET über Subnetze verfügt, ist das erste Subnetz standardmäßig ausgewählt. Wenn zusätzliche Subnetze vorhanden sind, wählen Sie das gewünschte Subnetz aus.
* **Speicherkonto** : In diesem Menü werden die Speicherkonten aufgelistet, die sich am gleichen Standort wie der Recovery Services-Tresor befinden. Zonenredundante Speicherkonten werden nicht unterstützt. Wenn keine Speicherkonten mit dem gleichen Standort wie der Recovery Services-Tresor vorhanden sind, müssen Sie vor dem Starten des Wiederherstellungsvorgangs eins erstellen. Der Replikationstyp des Speicherkontos ist in Klammern aufgeführt.

![Assistent für die Wiederherstellungskonfiguration wird festgelegt](./media/backup-azure-arm-restore-vms/recovery-configuration-wizard.png)

> [!NOTE]
> 1. Wenn Sie eine durch den Resource Manager bereitgestellte VM wiederherstellen möchten, müssen Sie ein virtuelles Netzwerk (VNET) identifizieren. Ein virtuelles Netzwerk (VNET) ist für eine klassische VM optional.
> 2. Wenn Sie virtuelle Computer mit verwalteten Datenträgern wiederherstellen, stellen Sie sicher, dass das ausgewählte Speicherkonto während seiner Gültigkeitsdauer nicht für die Speicherdienstverschlüsselung aktiviert wird.
> 3. Basierend auf dem Speichertyp des ausgewählten Speicherkontos (Premium oder Standard) werden alle wiederhergestellten Datenträger entweder Premium- oder Standard-Datenträger. Derzeit wird der gemischte Modus beim Wiederherstellen von Datenträgern nicht unterstützt.  
>
>

Klicken Sie auf dem Blatt **Wiederherstellungskonfiguration** auf **OK**, um die Wiederherstellungskonfiguration abzuschließen. Klicken Sie auf dem Blatt **Wiederherstellen** auf **Wiederherstellen**, um den Wiederherstellungsvorgang auszulösen.

## <a name="restore-backed-up-disks"></a>Wiederherstellen von gesicherten Datenträgern
Wenn Sie den virtuellen Computer, der von gesicherten Datenträgern wiederhergestellt werden soll, an die aktuellen Informationen auf dem Blatt „Wiederherstellungskonfiguration“ anpassen möchten, wählen Sie **Restore disks** (Wiederherstellungsdatenträger) als Wert für **Wiederherstellungstyp** aus. Bei dieser Option muss ein Speicherkonto angegeben werden, in das die Datenträger aus den Sicherungen kopiert werden. Wählen Sie bei der Auswahl eines Speicherkontos ein Konto aus, das den gleichen Standort wie der Recovery Services-Tresor verwendet. Zonenredundante Speicherkonten werden nicht unterstützt. Wenn keine Speicherkonten mit dem gleichen Standort wie der Recovery Services-Tresor vorhanden sind, müssen Sie vor dem Starten des Wiederherstellungsvorgangs eins erstellen. Der Replikationstyp des Speicherkontos ist in Klammern aufgeführt.

Nachdem der Wiederherstellungsvorgang abgeschlossen wurde, können Sie folgende Schritte ausführen:
* [Verwenden der Vorlage, um den wiederhergestellten virtuellen Computer anzupassen](#use-templates-to-customize-restore-vm)
* [Verwenden der wiederhergestellten Datenträger, um sie einem vorhandenen virtuellen Computer anzufügen](../virtual-machines/windows/attach-disk-portal.md)
* [Erstellen eines neuen virtuellen Computers aus wiederhergestellten Datenträgern mithilfe von PowerShell](./backup-azure-vms-automation.md#restore-an-azure-vm)

Klicken Sie auf dem Blatt **Wiederherstellungskonfiguration** auf **OK**, um die Wiederherstellungskonfiguration abzuschließen. Klicken Sie auf dem Blatt **Wiederherstellen** auf **Wiederherstellen**, um den Wiederherstellungsvorgang auszulösen.

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
    
## <a name="use-templates-to-customize-restore-vm"></a>Verwenden von Vorlagen zum Anpassen des wiederhergestellten virtuellen Computers
Sobald [das Wiederherstellen von Datenträgern abgeschlossen ist](#Track-the-restore-operation), können Sie die Vorlage verwenden, die im Rahmen des Wiederherstellungsvorgangs generiert wurde, um einen neuen virtuellen Computer mit einer anderen Konfiguration als der Sicherungskonfiguration zu erstellen oder um Namen von Ressourcen anzupassen, die beim Erstellen eines neuen virtuellen Computers über den Wiederherstellungspunkt erstellt wurden. 

> [!NOTE]
> Vorlagen werden als Teil der Wiederherstellung von Datenträgern für Wiederherstellungspunkte hinzugefügt, die nach dem 1. März 2017 erstellt werden. Sie gelten für nicht verschlüsselte virtuelle Computer und solche mit nicht verwalteten Datenträgern. Unterstützung für verschlüsselte virtuelle Computer und solche mit verwalteten Datenträgern wird in zukünftigen Versionen bereitgestellt. 
>
>

Gehen Sie wie folgt vor, um die als Teil der Wiederherstellung von Datenträgern generierte Vorlage zu erhalten:

1. Wechseln Sie zu den Details des Wiederherstellungsauftrags für den Auftrag. 
2. Dadurch wird der Vorlagen-URI aufgeführt, über den Sie die Vorlage herunterladen können. Notieren Sie den in den Werten angegebenen Containernamen. 

     ![Drilldown des Wiederherstellungsauftrags](./media/backup-azure-arm-restore-vms/restore-job-drill-down.png)
     
3. Notieren Sie den Namen des Zielspeicherkontos, den Containernamen und den Blob-URI der Vorlage, die in den Werten aufgeführt sind. Wechseln Sie zu *Zielspeicherkonto > Blobs auswählen > Container* und dann zur Datei, und laden Sie die Datei herunter, deren Name mit *azuredeploy* beginnt.

    ![download-template-storage-account](./media/backup-azure-arm-restore-vms/download-template.png)
    
   Alternativ können Sie den [Azure-Speicher-Explorer](http://storageexplorer.com/) verwenden, um zum entsprechenden Abonnement, zum Zielspeicherkonto und dann zum Blobcontainer zu wechseln. Wählen Sie dann den Containernamen aus, den Sie im Schritt oben notiert haben. Laden Sie aus dem Bereich auf der rechten Seite, der die Dateien im Container anzeigt, die Datei herunter, deren Name mit *azuredeploy* beginnt. 
   
   ![download-template-storage-explorer](./media/backup-azure-arm-restore-vms/template-storage-explorer-download.png)
     
Sobald die Vorlage heruntergeladen wurde, verwenden Sie die Vorlagenbereitstellung, um [die Vorlage zu bearbeiten und bereitzustellen](../azure-resource-manager/resource-group-template-deploy-portal.md#deploy-resources-from-custom-template), oder fügen Sie vor der Bereitstellung weitere Anpassungen durch [Erstellen einer Vorlage](../azure-resource-manager/resource-group-authoring-templates.md) hinzu. Sie können die Option „Datei laden“ zum Bereitstellen der oben heruntergeladenen Vorlage verwenden. 

   ![Vorlagenbereitstellung durch Laden](./media/backup-azure-arm-restore-vms/loading-template.png)
   
Akzeptieren Sie nach der Eingabe der erforderlichen Werte die *Geschäftsbedingungen*, und klicken Sie auf **Kaufen**.

   ![Übermitteln der Vorlagenbereitstellung](./media/backup-azure-arm-restore-vms/submitting-template.png)

## <a name="post-restore-steps"></a>Schritte nach der Wiederherstellung
* Bei Verwendung einer Cloud-Init-basierten Linux-Verteilung wie etwa Ubuntu wird das Kennwort aus Sicherheitsgründen nach der Wiederherstellung blockiert. Verwenden Sie zum [Zurücksetzen des Kennworts](../virtual-machines/linux/classic/reset-access.md)die VMAccess-Erweiterung auf dem wiederhergestellten virtuellen Computer. Es wird empfohlen, SSH-Schlüssel für diese Verteilungen zu verwenden, um das Zurücksetzen des Kennworts nach der Wiederherstellung zu vermeiden.
* Erweiterungen, die während der Konfiguration der Sicherung vorhanden waren, werden zwar installiert, aber nicht aktiviert. Installieren Sie Erweiterungen neu, wenn Probleme auftreten. 
* Wenn die gesicherte VM über eine statische IP-Adresse verfügt, erhält die wiederhergestellte VM eine dynamische IP-Adresse, um Konflikte beim Erstellen wiederhergestellter virtueller Computer zu vermeiden. Erfahren Sie mehr über das [Hinzufügen einer statischen IP-Adresse für wiederhergestellte virtuelle Computer](../virtual-network/virtual-networks-reserved-private-ip.md#how-to-add-a-static-internal-ip-to-an-existing-vm).
* Für wiederhergestellte virtuelle Computer ist kein Verfügbarkeitswert festgelegt. Es wird empfohlen, die Option zur Datenträgerwiederherstellung zu verwenden und [Verfügbarkeitsgruppen hinzuzufügen](../virtual-machines/windows/create-availability-set.md#use-powershell-to-create-an-availability-set), wenn Sie einen virtuellen Computer mithilfe von PowerShell oder Vorlagen aus wiederhergestellten Datenträgern erstellen. 


## <a name="backup-for-restored-vms"></a>Sicherung für wiederhergestellte virtuelle Computer
Wenn Sie den virtuellen Computer in derselben Ressourcengruppe mit dem Namen des ursprünglich gesicherten virtuellen Computers wiederhergestellt haben, wird der virtuelle Computer nach der Wiederherstellung weiterhin gesichert. Wenn Sie entweder den virtuellen Computer in einer anderen Ressourcengruppe wiederhergestellt oder einen anderen Namen dafür angegeben haben, wird er als neuer virtueller Computer behandelt, und Sie müssen die Sicherung dafür einrichten.

## <a name="restoring-a-vm-during-azure-datacenter-disaster"></a>Wiederherstellen eines virtuellen Computers während einer Azure-Datencenter-Notfallwiederherstellung
Mit Azure Backup können gesicherte virtuelle Computer im gekoppelten Datencenter wiederhergestellt werden, falls in dem primären Datencenter, in dem VMs ausgeführt werden, ein Notfall eingetreten ist, und Sie den Sicherungstresor als georedundant konfiguriert haben. Bei solchen Szenarien müssen Sie ein Speicherkonto auswählen, das im gekoppelten Datencenter vorhanden ist, ansonsten bleibt der Wiederherstellungsprozess gleich. Azure Backup verwendet den Computedienst des gekoppelten georedundanten Datencenters, um den wiederhergestellten virtuellen Computer zu erstellen. Erfahren Sie mehr über die [Resilienz der Azure-Rechenzentren](../resiliency/resiliency-technical-guidance-recovery-loss-azure-region.md).

## <a name="restoring-domain-controller-vms"></a>Wiederherstellen von virtuellen Computern des Domänencontrollers
Die Sicherung von virtuellen Computern des Domänencontrollers wird in Azure Backup unterstützt. Der Wiederherstellungsvorgang erfordert jedoch Sorgfalt. Der ordnungsgemäße Wiederherstellungsvorgang hängt von der Struktur der Domäne ab. Im einfachsten Fall verfügen Sie über einen einzelnen Domänencontroller in einer einzelnen Domäne. Doch in Produktionsumgebungen ist es üblicher, über eine einzelne Domäne mit mehreren Domänencontrollern zu verfügen, von denen einige lokal sein können. Möglich ist auch, dass Sie über eine Gesamtstruktur mit mehreren Domänen verfügen. 

Aus Active Directory-Sicht entspricht die Azure-VM beliebigen anderen VMs auf einem modernen unterstützten Hypervisor. Der Hauptunterschied bei lokalen Hypervisoren ist, dass keine VM-Konsole in Azure verfügbar ist. Eine Konsole ist für bestimmte Szenarien erforderlich, z.B. zur Wiederherstellung mithilfe einer Sicherung des Typs Bare-Metal-Recovery (BMR). Die VM-Wiederherstellung aus dem Sicherungstresor ist jedoch ein vollständiger Ersatz für BMR. Der Active Directory-Wiederherstellungsmodus ist auch verfügbar, sodass alle Active Directory-Wiederherstellungsszenarien realisierbar sind. Weitere Hintergrundinformationen finden Sie unter [Sicherungs- und Wiederherstellungsaspekte für virtualisierte Domänencontroller](https://technet.microsoft.com/en-us/library/virtual_active_directory_domain_controller_virtualization_hyperv(v=ws.10).aspx#backup_and_restore_considerations_for_virtualized_domain_controllers) und [Planen der Wiederherstellung der Active Directory-Gesamtstruktur](https://technet.microsoft.com/en-us/library/planning-active-directory-forest-recovery(v=ws.10).aspx).

### <a name="single-dc-in-a-single-domain"></a>Einzelner Domänencontroller in einer einzelnen Domäne
Der virtuelle Computer kann (wie alle anderen virtuellen Computer) über das Azure-Portal oder mithilfe von PowerShell wiederhergestellt werden.

### <a name="multiple-dcs-in-a-single-domain"></a>Mehrere Domänencontroller in einer einzelnen Domäne
Wenn andere Domänencontroller in der gleichen Domäne über das Netzwerk erreicht werden können, kann der Domänencontroller wie ein beliebiger virtueller Computer wiederhergestellt werden. Wenn es sich um den letzten verbleibenden Domänencontroller in der Domäne handelt oder eine Wiederherstellung in einem isolierten Netzwerk erfolgt, muss ein Vorgang zur Wiederherstellung der Gesamtstruktur folgen.

### <a name="multiple-domains-in-one-forest"></a>Mehrere Domänen in einer Gesamtstruktur
Wenn andere Domänencontroller in der gleichen Domäne über das Netzwerk erreicht werden können, kann der Domänencontroller wie ein beliebiger virtueller Computer wiederhergestellt werden. In allen anderen Fällen wird allerdings eine Wiederherstellung der Gesamtstruktur empfohlen.

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

