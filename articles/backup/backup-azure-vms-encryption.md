---
title: "Sichern und Wiederherstellen von verschlüsselten VMs mit Azure Backup"
description: "In diesem Artikel wird die Benutzeroberfläche für die Sicherung und Wiederherstellung von VMs mit Azure Disk Encryption beschrieben."
services: backup
documentationcenter: 
author: JPallavi
manager: vijayts
editor: 
ms.assetid: 8387f186-7d7b-400a-8fc3-88a85403ea63
ms.service: backup
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 07/27/2017
ms.author: pajosh;markgal;trinadhk
ms.custom: H1Hack27Feb2017
ms.translationtype: HT
ms.sourcegitcommit: 54774252780bd4c7627681d805f498909f171857
ms.openlocfilehash: 89492cda8eff23509bee7693bb5f7e6ab5eb10d1
ms.contentlocale: de-de
ms.lasthandoff: 07/27/2017

---
# <a name="how-to-back-up-and-restore-encrypted-virtual-machines-with-azure-backup"></a>Sichern und Wiederherstellen verschlüsselter virtueller Computer mit Azure Backup
In diesem Artikel werden die Schritte zum Sichern und Wiederherstellen von virtuellen Computern mit Azure Backup beschrieben. Außerdem enthält er Details zu den unterstützten Szenarien, Voraussetzungen und Problembehandlungsschritten für Fehler.

## <a name="supported-scenarios"></a>Unterstützte Szenarien
> [!NOTE]
> * Die Sicherung und Wiederherstellung von verschlüsselten VMs wird nur für virtuelle Computer unterstützt, die mit Resource Manager bereitgestellt wurden. Für klassisch bereitgestellte virtuelle Computer ist keine Unterstützung vorhanden. <br>
> * Das Feature wird für virtuelle Windows- und Linux-Computer mit Azure Disk Encryption unterstützt, wobei die Datenträger mithilfe des BitLocker-Features von Windows (Branchenstandard) bzw. mithilfe des DM-Crypt-Features von Linux verschlüsselt werden. <br>
> * Unterstützung besteht nur für virtuelle Computer, die sowohl per BitLocker-Verschlüsselungsschlüssel als auch per Schlüsselverschlüsselungsschlüssel verschlüsselt wurden. Es ist keine Unterstützung für virtuelle Computer vorhanden, die nur per BitLocker-Verschlüsselungsschlüssel verschlüsselt wurden. <br>
>
>

## <a name="prerequisites"></a>Voraussetzungen
1. Der virtuelle Computer wurde per [Azure Disk Encryption](../security/azure-security-disk-encryption.md) verschlüsselt. Er sollte sowohl per BitLocker-Verschlüsselungsschlüssel als auch per Schlüsselverschlüsselungsschlüssel verschlüsselt werden.
2. Der Recovery Services-Tresor wurde erstellt, und die Speicherreplikation wurde eingerichtet, indem die Schritte im Artikel zum Thema [Vorbereiten der Umgebung für die Sicherung](backup-azure-arm-vms-prepare.md) ausgeführt wurden.
3. Azure Backup wurden die [Berechtigungen zum Zugriff auf den Schlüsseltresor](#provide-permissions-to-azure-backup) mit Schlüsseln und Geheimnissen für verschlüsselte virtuelle Computer gewährt.

## <a name="backup-encrypted-vm"></a>Sichern von verschlüsselten VMs
Verwenden Sie die folgenden Schritte zum Festlegen des Sicherungsziels, Definieren der Richtlinie, Konfigurieren von Elementen und Auslösen der Sicherung.

### <a name="configure-backup"></a>Konfigurieren der Sicherung
1. Falls Sie bereits über einen geöffneten Recovery Services-Tresor verfügen, können Sie mit dem nächsten Schritt fortfahren. Wenn Sie keinen Recovery Services-Tresor geöffnet haben und sich im Azure-Portal befinden, klicken Sie im Hub-Menü auf **Durchsuchen**.

   * Geben Sie in der Liste mit den Ressourcen **Recovery Services**ein.
   * Sobald Sie mit der Eingabe beginnen, wird die Liste auf der Grundlage Ihrer Eingabe gefiltert. Wenn **Recovery Services-Tresore**angezeigt wird, klicken Sie auf den Eintrag.

      ![Erstellen eines Recovery Services-Tresors – Schritt 1](./media/backup-azure-vms-encryption/browse-to-rs-vaults.png) <br/>

     Die Liste mit den Recovery Services-Tresoren wird angezeigt. Wählen Sie in der Liste mit den Recovery Services-Tresoren einen Tresor aus.

     Das ausgewählte Tresor-Dashboard wird geöffnet.
2. Klicken Sie in der Liste mit den Elementen, die im Tresor angezeigt werden, auf **Sicherung**, um das Blatt „Sicherung“ zu öffnen.

      ![Blatt „Sicherung“ öffnen](./media/backup-azure-vms-encryption/select-backup.png)
3. Klicken Sie auf dem Blatt „Sicherung“ auf **Sicherungsziel**, um das gleichnamige Blatt zu öffnen.

      ![Blatt „Szenario“ öffnen](./media/backup-azure-vms-encryption/select-backup-goal-one.png)
4. Legen Sie auf dem Blatt für das Sicherungsziel die Option **Wo wird Ihre Workload ausgeführt?** auf „Azure“ und die Option **Was möchten Sie sichern?** auf „Virtueller Computer“ fest, und klicken Sie anschließend auf **OK**.

   Das Blatt für das Sicherungsziel wird geschlossen, und das Blatt „Sicherungsrichtlinie“ wird geöffnet.

   ![Blatt „Szenario“ öffnen](./media/backup-azure-vms-encryption/select-backup-goal-two.png)
5. Wählen Sie auf dem Blatt „Sicherungsrichtlinie“ die Sicherungsrichtlinie aus, die Sie auf den Tresor anwenden möchten, und klicken Sie auf **OK**.

      ![Sicherungsrichtlinie auswählen](./media/backup-azure-vms-encryption/setting-rs-backup-policy-new.png)

    In den Details werden die Informationen zur Standardrichtlinie aufgeführt. Wählen Sie im Dropdownmenü die Option **Neu erstellen** , wenn Sie eine Richtlinie erstellen möchten. Nachdem Sie auf **OK**geklickt haben, wird die Sicherungsrichtlinie dem Tresor zugeordnet.

    Wählen Sie als Nächstes die VMs aus, die dem Tresor zugeordnet werden sollen.
6. Wählen Sie die verschlüsselten virtuellen Computer aus, die der angegebenen Richtlinie zugeordnet werden sollen, und klicken Sie auf **OK**.

      ![Auswählen von verschlüsselten VMs](./media/backup-azure-vms-encryption/selected-encrypted-vms.png)
7. Auf dieser Seite wird eine Meldung mit einem Hinweis zum Schlüsseltresor angezeigt, der den ausgewählten verschlüsselten VMs zugeordnet ist. Für den Backup-Dienst ist Lesezugriff auf die Schlüssel und Geheimnisse im Schlüsseltresor erforderlich. Diese Berechtigungen werden verwendet, um den Schlüssel und das Geheimnis zusammen mit den zugeordneten VMs zu sichern. **Sie müssen dem Sicherungsdienst die Berechtigungen zum Zugriff auf den Schlüsseltresor gewähren, damit Sicherungen funktionieren**. Sie können diese Berechtigungen mit den [im Abschnitt weiter unten beschriebenen Schritten](#provide-permissions-to-azure-backup) gewähren.

      ![Meldung zu verschlüsselten VMs](./media/backup-azure-vms-encryption/encrypted-vm-warning-message.png)

      Nachdem Sie nun alle Einstellungen für den Tresor definiert haben, klicken Sie unten auf dem Blatt „Sicherung“ auf „Sicherung aktivieren“. Mit „Sicherung aktivieren“ wird die Richtlinie im Tresor und auf den VMs bereitgestellt.
8. Die nächste Phase der Vorbereitung ist die Installation des VM-Agents, bzw. sicherzustellen, dass der VM-Agent installiert ist. Führen Sie hierzu die Schritte aus, die im Artikel zum Thema [Vorbereiten der Umgebung für die Sicherung](backup-azure-arm-vms-prepare.md) beschrieben sind.

### <a name="triggering-backup-job"></a>Auslösen des Sicherungsauftrags
Verwenden Sie die Schritte im Artikel zum Thema [Sichern von Azure-VMs im Recovery Services-Tresor](backup-azure-arm-vms.md), um den Sicherungsauftrag auszulösen.

### <a name="continue-backups-of-already-backed-up-vms-with-encryption-enabled"></a>Fortsetzen von Sicherungen von bereits gesicherten VMs mit aktivierter Verschlüsselung  
Wenn virtuelle Computer bereits im Recovery Services-Tresor gesichert und für die Verschlüsselung zu einem späteren Zeitpunkt aktiviert wurden, müssen Sie dem Sicherungsdienst Berechtigungen für den Zugriff auf den Schlüsseltresor erteilen, damit Sicherungen fortgesetzt werden können. Die erforderlichen Berechtigungen können Sie [anhand der Schritte im Abschnitt unten](#provide-permissions-to-azure-backup) oder über PowerShell erteilen. Führen Sie dazu die Schritte im Abschnitt **Aktivieren der Sicherung** in der [PowerShell-Dokumentation](backup-azure-vms-automation.md) aus. 

## <a name="provide-permissions-to-azure-backup"></a>Gewähren von Berechtigungen für Azure Backup
Gehen Sie anhand der folgenden Schritte vor, um Azure Backup die erforderlichen Berechtigungen für den Zugriff auf den Schlüsseltresor und für die Ausführung der Sicherung verschlüsselten VMs zu gewähren:
1. Wählen Sie **Weitere Dienste** aus, und suchen Sie nach **Schlüsseltresore**.

    ![Schlüsseltresor suchen](./media/backup-azure-vms-encryption/search-key-vault.png)
    
2. Wählen Sie in der Liste von Schlüsseltresoren den Schlüsseltresor aus, der der verschlüsselten VM zugeordnet ist, die gesichert werden muss.

     ![Schlüsseltresor auswählen](./media/backup-azure-vms-encryption/select-key-vault.png)
     
3. Klicken Sie auf **Zugriffsrichtlinien** und dann auf **Neue hinzufügen**.

    ![Zugriffsrichtlinie hinzufügen](./media/backup-azure-vms-encryption/select-key-vault-access-policy.png)
    
4. Klicken Sie auf **Prinzipal auswählen**, und geben Sie auf der Suchleiste **Sicherungsverwaltungsdienst** ein. 

    ![Sicherungsdienst suchen](./media/backup-azure-vms-encryption/search-backup-service.png)
    
5. Wählen Sie den **Sicherungsverwaltungsdienst** aus, und klicken Sie auf die Schaltfläche „Auswählen“.

    ![Sicherungsdienst auswählen](./media/backup-azure-vms-encryption/select-backup-service.png)
    
6. Wählen Sie in der Dropdownliste „Anhand einer Vorlage konfigurieren (optional)“ die Option **Azure Backup** aus. In den Dropdownlisten „Schlüsselberechtigungen“ und „Berechtigungen für Geheimnis“ sind bereits die erforderlichen Berechtigungen angegeben. 

    ![Azure Backup auswählen](./media/backup-azure-vms-encryption/select-backup-template.png)
    
7. Klicken Sie auf **OK**. Beachten Sie, dass der Sicherungsverwaltungsdienst auf dem Blatt „Zugriffsrichtlinien“ hinzugefügt wird. 

    ![Zugriffsrichtlinie für Sicherungsdienst](./media/backup-azure-vms-encryption/backup-service-access-policy.png)
    
8. Klicken Sie auf **Speichern**. Damit erhält Azure Backup die erforderlichen Berechtigungen.

    ![Zugriffsrichtlinie für Sicherungsdienst](./media/backup-azure-vms-encryption/save-access-policy.png)

Sobald Berechtigungen erfolgreich gewährt wurden, können Sie damit fortfahren, Sicherungen für verschlüsselte VMs zu aktivieren.

## <a name="restore-encrypted-vm"></a>Wiederherstellen der verschlüsselten VM
Stellen Sie zum Wiederherstellen verschlüsselter virtueller Computer zunächst Datenträger gemäß den Schritten des Abschnitts **Wiederherstellen von gesicherten Datenträgern** (unter [Auswählen einer Konfiguration für die VM-Wiederherstellung](backup-azure-arm-restore-vms.md#choosing-a-vm-restore-configuration)) wieder her. Sie können anschließend eine der folgenden Optionen wählen:
* Führen Sie die unter [Erstellen eines virtuellen Computers aus wiederhergestellten Datenträgern](backup-azure-vms-automation.md#create-a-vm-from-restored-disks) beschriebenen PowerShell-Schritte aus, um auf der Grundlage der wiederhergestellten Datenträger einen vollständigen virtuellen Computer zu erstellen.
* Oder [verwenden Sie eine als Teil von „Datenträger wiederherstellen“ generierte Vorlage](backup-azure-arm-restore-vms.md#use-templates-to-customize-restore-vm), um VMs anhand wiederhergestellter Datenträger zu erstellen. Vorlagen können nur für Wiederherstellungspunkte verwendet werden, die nach dem 26. April 2017 erstellt wurden.

## <a name="troubleshooting-errors"></a>Problembehandlung
| Vorgang | Fehlerdetails | Lösung |
| --- | --- | --- |
| Sicherung |Bei der Überprüfung ist ein Fehler aufgetreten, da der virtuelle Computer nur per BEK verschlüsselt wurde. Sicherungen können nur für virtuelle Computer aktiviert werden, die per BEK und KEK verschlüsselt wurden. |Der virtuelle Computer sollte per BEK und KEK verschlüsselt werden. Entschlüsseln Sie zunächst den virtuellen Computer, und verschlüsseln Sie ihn dann mit BEK und KEK. Aktivieren Sie die Sicherung, sobald die VM mit BEK und KEK verschlüsselt wurde. Erfahren Sie mehr über das [Entschlüsseln und Verschlüsseln des virtuellen Computers](../security/azure-security-disk-encryption.md).  |
| Wiederherstellen |Sie können diese verschlüsselte VM nicht wiederherstellen, da der Schlüsseltresor, der dieser VM zugeordnet ist, nicht vorhanden ist. |Erstellen Sie den Schlüsseltresor anhand der Vorgehensweise unter [Erste Schritte mit dem Azure-Schlüsseltresor](../key-vault/key-vault-get-started.md). Verwenden Sie die Informationen im Artikel zum Thema [Wiederherstellen des Schlüssels und Geheimnisses für den Schlüsseltresor per Azure Backup](backup-azure-restore-key-secret.md), um den Schlüssel und das Geheimnis wiederherzustellen, falls diese Angaben nicht schon vorhanden sind. |
| Wiederherstellen |Sie können diese verschlüsselte VM nicht wiederherstellen, da der Schlüssel und das Geheimnis für diese VM nicht vorhanden sind. |Verwenden Sie die Informationen im Artikel zum Thema [Wiederherstellen des Schlüssels und Geheimnisses für den Schlüsseltresor per Azure Backup](backup-azure-restore-key-secret.md), um den Schlüssel und das Geheimnis wiederherzustellen, falls diese Angaben nicht schon vorhanden sind. |
| Wiederherstellen |Der Speicherdienst ist nicht zum Zugreifen auf Ressourcen in Ihrem Abonnement autorisiert. |Stellen Sie wie weiter oben bereits erläutert zunächst Datenträger gemäß den Schritten des Abschnitts **Wiederherstellen von gesicherten Datenträgern** (unter [Auswählen einer Konfiguration für die VM-Wiederherstellung](backup-azure-arm-restore-vms.md#choosing-a-vm-restore-configuration)) wieder her. Verwenden Sie anschließend PowerShell zum [Erstellen eines virtuellen Computers aus wiederhergestellten Datenträgern](backup-azure-vms-automation.md#create-a-vm-from-restored-disks). |
|Sicherung | Der Azure Backup-Dienst weist nicht genügend Berechtigungen für den Schlüsseltresor auf, um verschlüsselte virtuelle Computer zu sichern. | Der virtuelle Computer muss sowohl per BitLocker-Verschlüsselungsschlüssel als auch per Schlüsselverschlüsselungsschlüssel verschlüsselt werden. Anschließend sollte die Sicherung aktiviert werden.  Dem Sicherungsdienst müssen diese Berechtigungen [anhand der Schritte im Abschnitt oben](#provide-permissions-to-azure-backup) oder mittels PowerShell über die Schritte im Abschnitt **Aktivieren des Schutzes** in der PowerShell-Dokumentation unter [Verwenden von AzureRM.RecoveryServices.Backup-Cmdlets zum Sichern virtueller Computer](backup-azure-vms-automation.md#back-up-azure-vms) erteilt werden. |  

