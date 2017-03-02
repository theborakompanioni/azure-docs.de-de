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
ms.date: 01/18/2017
ms.author: pajosh;markgal;trinadhk
translationtype: Human Translation
ms.sourcegitcommit: d7a2b9c13b2c3372ba2e83f726c7bf5cc7e98c02
ms.openlocfilehash: b6ddaf7dc26d0efac782752e55594262531e794a
ms.lasthandoff: 02/17/2017


---
# <a name="backup-and-restore-encrypted-vms-using-azure-backup"></a>Sichern und Wiederherstellen von verschlüsselten VMs mit Azure Backup
In diesem Artikel werden die Schritte zum Sichern und Wiederherstellen von virtuellen Computern mit Azure Backup beschrieben. Außerdem enthält er Details zu den unterstützten Szenarien, Voraussetzungen und Problembehandlungsschritten für Fehler.

## <a name="supported-scenarios"></a>Unterstützte Szenarien
> [!NOTE]
> 1. Die Sicherung und Wiederherstellung von verschlüsselten VMs wird nur für virtuelle Computer unterstützt, die mit Resource Manager bereitgestellt wurden. Für klassisch bereitgestellte virtuelle Computer ist keine Unterstützung vorhanden. <br>
> 2. Das Feature wird für virtuelle Windows- und Linux-Computer mit Azure Disk Encryption unterstützt, wobei die Datenträger mithilfe des BitLocker-Features von Windows (Branchenstandard) bzw. mithilfe des DM-Crypt-Features von Linux verschlüsselt werden. <br>
> 3. Unterstützung besteht nur für virtuelle Computer, die sowohl per BitLocker-Verschlüsselungsschlüssel als auch per Schlüsselverschlüsselungsschlüssel verschlüsselt wurden. Es ist keine Unterstützung für virtuelle Computer vorhanden, die nur per BitLocker-Verschlüsselungsschlüssel verschlüsselt wurden. <br>
>
>

## <a name="pre-requisites"></a>Voraussetzungen
1. Der virtuelle Computer wurde per [Azure Disk Encryption](../security/azure-security-disk-encryption.md) verschlüsselt. Er sollte sowohl per BitLocker-Verschlüsselungsschlüssel als auch per Schlüsselverschlüsselungsschlüssel verschlüsselt werden.
2. Der Recovery Services-Tresor wurde erstellt, und die Speicherreplikation wurde eingerichtet, indem die Schritte im Artikel zum Thema [Vorbereiten der Umgebung für die Sicherung](backup-azure-arm-vms-prepare.md) ausgeführt wurden.

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
7. Auf dieser Seite wird eine Meldung mit einem Hinweis zum Schlüsseltresor angezeigt, der den ausgewählten verschlüsselten VMs zugeordnet ist. Für den Backup-Dienst ist Lesezugriff auf die Schlüssel und Geheimnisse im Schlüsseltresor erforderlich. Diese Berechtigungen werden verwendet, um den Schlüssel und das Geheimnis zusammen mit den zugeordneten VMs zu sichern.

      ![Meldung zu verschlüsselten VMs](./media/backup-azure-vms-encryption/encrypted-vm-message.png)

      Nachdem Sie nun alle Einstellungen für den Tresor definiert haben, klicken Sie unten auf dem Blatt „Sicherung“ auf „Sicherung aktivieren“. Mit „Sicherung aktivieren“ wird die Richtlinie im Tresor und auf den VMs bereitgestellt.
8. Die nächste Phase der Vorbereitung ist die Installation des VM-Agents, bzw. sicherzustellen, dass der VM-Agent installiert ist. Führen Sie hierzu die Schritte aus, die im Artikel zum Thema [Vorbereiten der Umgebung für die Sicherung](backup-azure-arm-vms-prepare.md) beschrieben sind.

### <a name="triggering-backup-job"></a>Auslösen des Sicherungsauftrags
Verwenden Sie die Schritte im Artikel zum Thema [Sichern von Azure-VMs im Recovery Services-Tresor](backup-azure-arm-vms.md), um den Sicherungsauftrag auszulösen.

## <a name="restore-encrypted-vm"></a>Wiederherstellen der verschlüsselten VM
Stellen Sie zum Wiederherstellen verschlüsselter virtueller Computer zunächst Datenträger gemäß den Schritten des Abschnitts **Wiederherstellen von gesicherten Datenträgern** (unter [Auswählen einer Konfiguration für die VM-Wiederherstellung](backup-azure-arm-restore-vms.md#choosing-a-vm-restore-configuration)) wieder her. Führen Sie anschließend die unter [Erstellen eines virtuellen Computers aus wiederhergestellten Datenträgern](backup-azure-vms-automation.md#create-a-vm-from-restored-disks) beschriebenen PowerShell-Schritte aus, um auf der Grundlage der wiederhergestellten Datenträger einen vollständigen virtuellen Computer zu erstellen.

## <a name="troubleshooting-errors"></a>Problembehandlung
| Vorgang | Fehlerdetails | Lösung |
| --- | --- | --- |
| Sicherung |Bei der Überprüfung ist ein Fehler aufgetreten, da der virtuelle Computer nur per BEK verschlüsselt wurde. Sicherungen können nur für virtuelle Computer aktiviert werden, die per BEK und KEK verschlüsselt wurden. |Der virtuelle Computer sollte per BEK und KEK verschlüsselt werden. Entschlüsseln Sie zunächst den virtuellen Computer, und verschlüsseln Sie ihn dann mit BEK und KEK. Aktivieren Sie die Sicherung, sobald die VM mit BEK und KEK verschlüsselt wurde. Erfahren Sie mehr über das [Entschlüsseln und Verschlüsseln des virtuellen Computers](../security/azure-security-disk-encryption.md).  |
| Wiederherstellen |Sie können diese verschlüsselte VM nicht wiederherstellen, da der Schlüsseltresor, der dieser VM zugeordnet ist, nicht vorhanden ist. |Erstellen Sie den Schlüsseltresor anhand der Vorgehensweise unter [Erste Schritte mit dem Azure-Schlüsseltresor](../key-vault/key-vault-get-started.md). Verwenden Sie die Informationen im Artikel zum Thema [Wiederherstellen des Schlüssels und Geheimnisses für den Schlüsseltresor per Azure Backup](backup-azure-restore-key-secret.md), um den Schlüssel und das Geheimnis wiederherzustellen, falls diese Angaben nicht schon vorhanden sind. |
| Wiederherstellen |Sie können diese verschlüsselte VM nicht wiederherstellen, da der Schlüssel und das Geheimnis für diese VM nicht vorhanden sind. |Verwenden Sie die Informationen im Artikel zum Thema [Wiederherstellen des Schlüssels und Geheimnisses für den Schlüsseltresor per Azure Backup](backup-azure-restore-key-secret.md), um den Schlüssel und das Geheimnis wiederherzustellen, falls diese Angaben nicht schon vorhanden sind. |
| Wiederherstellen |Der Speicherdienst ist nicht zum Zugreifen auf Ressourcen in Ihrem Abonnement autorisiert. |Stellen Sie wie weiter oben bereits erläutert zunächst Datenträger gemäß den Schritten des Abschnitts **Wiederherstellen von gesicherten Datenträgern** (unter [Auswählen einer Konfiguration für die VM-Wiederherstellung](backup-azure-arm-restore-vms.md#choosing-a-vm-restore-configuration)) wieder her. Verwenden Sie anschließend PowerShell zum [Erstellen eines virtuellen Computers aus wiederhergestellten Datenträgern](backup-azure-vms-automation.md#create-a-vm-from-restored-disks).

