---
title: "Verwalten und Überwachen der Sicherung von virtuellen Azure-Computern | Microsoft Docs"
description: "Erfahren Sie, wie Sie die Sicherung von virtuellen Azure-Computern verwalten und überwachen können"
services: backup
documentationcenter: 
author: trinadhk
manager: shreeshd
editor: 
ms.assetid: 4372944e-d33a-4f6a-bd5f-d04af2842713
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/31/2016
ms.author: trinadhk; jimpark; markgal;
translationtype: Human Translation
ms.sourcegitcommit: 0af5a4e2139a202c7f62f48c7a7e8552457ae76d
ms.openlocfilehash: 7a855e4f4af81f9316cb53be3807c6b290941bf4


---
# <a name="manage-and-monitor-azure-virtual-machine-backups"></a>Verwalten und Überwachen der Sicherung von virtuellen Azure-Computern
> [!div class="op_single_selector"]
> * [Verwalten von Azure-VM-Sicherungen](backup-azure-manage-vms.md)
> * [Verwalten von klassischen VM-Sicherungen](backup-azure-manage-vms-classic.md)
> 
> 

Dieser Artikel enthält Informationen zu allgemeinen Verwaltungs- und Überwachungsaufgaben für nach dem klassischen Modell bereitgestellte virtuelle Computer, die in Azure geschützt werden.  

> [!NOTE]
> Azure verfügt über zwei Bereitstellungsmodelle zum Erstellen und Verwenden von Ressourcen: [Resource Manager-Modell und klassisches Modell](../azure-resource-manager/resource-manager-deployment-model.md). Unter [Vorbereiten der Umgebung für die Sicherung virtueller Azure-Computer](backup-azure-vms-prepare.md) finden Sie ausführliche Informationen zum Arbeiten mit VMs, die mit dem klassischen Bereitstellungsmodell bereitgestellt wurden.
> 
> 

## <a name="manage-protected-virtual-machines"></a>Verwalten geschützter virtueller Computer
So verwalten Sie geschützte virtuelle Computer

1. Zum Anzeigen und Verwalten von Sicherungseinstellungen für einen virtuellen Computer klicken Sie auf die Registerkarte **Geschützte Elemente** .
2. Klicken Sie auf den Namen eines geschützten Elements, um die Registerkarte **Sicherungsdetails** anzuzeigen, auf der Sie Informationen zur letzten Sicherung finden.
   
    ![Sicherung virtueller Computer](./media/backup-azure-manage-vms/backup-vmdetails.png)
3. Zum Anzeigen und Verwalten von Sicherungsrichtlinieneinstellungen für einen virtuellen Computer klicken Sie auf die Registerkarte **Richtlinien** .
   
    ![VM-Richtlinie](./media/backup-azure-manage-vms/manage-policy-settings.png)
   
    Auf der Registerkarte **Sicherungsrichtlinien** wird die vorhandene Richtlinie angezeigt. Sie können sie nach Bedarf ändern. Wenn Sie eine neue Richtlinie erstellen müssen, klicken Sie auf der Seite **Richtlinien** auf **Erstellen**. Wenn Sie eine Richtlinie entfernen möchten, dürfen ihr keine virtuellen Computer zugeordnet sein.
   
    ![VM-Richtlinie](./media/backup-azure-manage-vms/backup-vmpolicy.png)
4. Sie erhalten weitere Informationen zu Aktionen oder zum Status eines virtuellen Computers auf der Seite **Aufträge** . Klicken Sie in der Liste auf einen Auftrag, um weitere Details zu erhalten, oder filtern Sie die Aufträge für einen bestimmten virtuellen Computer.
   
    ![Aufträge](./media/backup-azure-manage-vms/backup-job.png)

## <a name="on-demand-backup-of-a-virtual-machine"></a>On-Demand-Sicherung eines virtuellen Computers
Sie können eine bedarfsabhängige Sicherung eines virtuellen Computers erstellen, sobald dieser für den Schutz konfiguriert ist. Wenn die erste Sicherung für den virtuellen Computer ansteht, wird mithilfe einer bedarfsabhängigen Sicherung eine vollständige Kopie des virtuellen Computers im Azure-Sicherungstresor erstellt. Wenn die erste Sicherung abgeschlossen ist, werden bei einer bedarfsabhängigen Sicherung nur Änderungen an einer vorherigen Sicherung an den Azure-Sicherungstresor gesendet, d.h. sie ist stets inkrementell.

> [!NOTE]
> Der Aufbewahrungszeitraum einer bedarfsabhängigen Sicherung wird auf den Wert festgelegt, der in der Sicherungsrichtlinie für den virtuellen Computer für die tägliche Aufbewahrung festgelegt ist.  
> 
> 

So führen Sie die bedarfsabhängige Sicherung eines virtuellen Computers aus:

1. Navigieren Sie zur Seite **Geschützte Elemente**, wählen Sie **Virtueller Azure-Computer** als **Typ** aus (sofern nicht bereits ausgewählt), und klicken Sie auf die Schaltfläche **Auswählen**.
   
    ![VM-Typ](./media/backup-azure-manage-vms/vm-type.png)
2. Wählen Sie den virtuellen Computer aus, auf dem Sie die abhängige Sicherung erstellen möchten, und klicken am unteren Rand der Seite auf **Jetzt sichern** .
   
    ![Jetzt sichern](./media/backup-azure-manage-vms/backup-now.png)
   
    Dadurch wird ein Sicherungsauftrag auf dem ausgewählten virtuellen Computer erstellt. Die Beibehaltungsdauer des Wiederherstellungspunkts, der durch diesen Auftrag erstellt wird, entspricht der Angabe in der Richtlinie für den virtuellen Computer.
   
    ![Erstellen eines Sicherungsauftrag](./media/backup-azure-manage-vms/creating-job.png)
   
   > [!NOTE]
   > Zum Anzeigen der Richtlinie eines virtuellen Computers führen Sie auf der Seite **Geschützte Elemente** des virtuellen Computers eine Detailsuche durch und navigieren zur Registerkarte "Sicherungsrichtlinie".
   > 
   > 
3. Nachdem der Auftrag erstellt wurde, klicken Sie auf der Befehlsleiste auf die Schaltfläche **Auftrag anzeigen** , um den entsprechenden Auftrag auf der Seite "Aufträge" anzuzeigen.
   
    ![Sicherungsauftrag erstellt](./media/backup-azure-manage-vms/created-job.png)
4. Nach erfolgreichem Abschluss des Auftrags wird ein Wiederherstellungspunkt erstellt, den Sie zum Wiederherstellen des virtuellen Computers verwenden können. Dadurch wird auch auf der Seite **Geschützte Elemente** der Wert in der Wiederherstellungspunktspalte um 1 erhöht.

## <a name="stop-protecting-virtual-machines"></a>Beenden des Schutzes für virtuelle Computer
Mit den folgenden Optionen können Sie künftige Sicherungen eines virtuellen Computers beenden:

* Sicherungsdaten beibehalten, die dem virtuellen Computer im Azure Backup-Tresor zugeordnet sind
* Sicherungsdaten löschen, die dem virtuellen Computer zugeordnet sind Sicherungsdaten löschen, die dem virtuellen Computer zugeordnet sind

Wenn Sie ausgewählt haben, dass die dem virtuellen Computer zugeordneten Sicherungsdaten aufbewahrt werden sollen, können Sie den virtuellen Computer anhand der Sicherungsdaten wiederherstellen. Klicken Sie [hier](https://azure.microsoft.com/pricing/details/backup/), um die Preisübersicht für virtuelle Computer anzuzeigen.

So beenden Sie den Schutz für einen virtuellen Computer:

1. Navigieren Sie zur Seite **Geschützte Elemente**, wählen Sie **Virtueller Azure-Computer** als Filtertyp aus (sofern nicht bereits ausgewählt), und klicken Sie auf die Schaltfläche **Auswählen**.
   
    ![VM-Typ](./media/backup-azure-manage-vms/vm-type.png)
2. Wählen Sie den virtuellen Computer aus, und klicken Sie am unteren Rand der Seite auf **Schutz beenden** .
   
    ![Schutz beenden](./media/backup-azure-manage-vms/stop-protection.png)
3. Standardmäßig löscht Azure Backup die dem virtuellen Computer zugeordneten Sicherungsdaten nicht.
   
    ![Bestätigen des Beendens des Schutzes](./media/backup-azure-manage-vms/confirm-stop-protection.png)
   
    Wenn Sie die gesicherten Daten löschen möchten, aktivieren Sie das Kontrollkästchen.
   
    ![Kontrollkästchen](./media/backup-azure-manage-vms/checkbox.png)
   
    Wählen Sie einen Grund für das Beenden der Sicherung. Dies ist zwar optional, hilft aber dem Azure Backup-Team beim Berücksichtigen von Feedback und Priorisieren der Kundenszenarien.
4. Klicken Sie auf die Schaltfläche **Senden**, um den Auftrag **Schutz beenden** zu senden. Klicken Sie auf **Auftrag anzeigen**, um den entsprechenden Auftrag auf der Seite **Aufträge** anzuzeigen.
   
    ![Schutz beenden](./media/backup-azure-manage-vms/stop-protect-success.png)
   
    Wenn Sie im Assistenten **Schutz beenden** nicht **Zugeordnete Sicherungsdaten löschen** ausgewählt haben, ändert sich nach Abschluss des Auftrags der Schutzstatus in **Schutz beendet**. Die Daten bleiben bei Azure Backup, bis sie explizit gelöscht werden. Sie können die Daten jederzeit löschen, indem Sie den virtuellen Computer auf der Seite **Geschützte Elemente** auswählen und auf **Löschen** klicken.
   
    ![Beendeter Schutz](./media/backup-azure-manage-vms/protection-stopped-status.png)
   
    Wenn Sie die Option **Zugeordnete Sicherungsdaten löschen** ausgewählt haben, wird der virtuelle Computer nicht mehr auf der Seite **Geschützte Elemente** angezeigt.

## <a name="re-protect-virtual-machine"></a>Erneutes Schützen virtueller Computer
Wenn Sie im Assistenten **Schutz beenden** nicht **Zugeordnete Sicherungsdaten** löschen ausgewählt haben, können Sie den virtuellen Computer erneut schützen, indem Sie ähnliche Schritte wie beim Sichern registrierter virtueller Computer ausführen. Nach Aktivierung des Schutzes werden vor Beenden des Schutzes die Sicherungsdaten dieses virtuellen Computers beibehalten und nach erneuter Aktivierung des Schutzes Wiederherstellungspunkte erstellt.

Nach erneuter Aktivierung des Schutzes ändert sich der Schutzstatus des virtuellen Computers in **Geschützt**, wenn es Wiederherstellungspunkte gibt, die vor dem Zeitpunkt der Aktivierung von **Schutz beenden** liegen.

  ![VM erneut geschützt](./media/backup-azure-manage-vms/reprotected-status.png)

> [!NOTE]
> Wenn Sie den virtuellen Computer erneut schützen, können Sie eine andere Richtlinie als die Richtlinie auswählen, gemäß der der virtuelle Computer zuvor geschützt wurde.
> 
> 

## <a name="unregister-virtual-machines"></a>Aufheben der Registrierung virtueller Computer
So entfernen Sie den virtuellen Computer aus dem Sicherungstresor

1. Klicken Sie unten auf der Seite auf die Schaltfläche **REGISTRIERUNG AUFHEBEN** .
   
    ![Schutz deaktivieren](./media/backup-azure-manage-vms/unregister-button.png)
   
    In einer Popupbenachrichtigung am unteren Bildschirmrand werden Sie zur Bestätigung aufgefordert. Klicken Sie auf **JA** , um fortzufahren.
   
    ![Schutz deaktivieren](./media/backup-azure-manage-vms/confirm-unregister.png)

## <a name="delete-backup-data"></a>Löschen von Sicherungsdaten
Sie können die Sicherungsdaten löschen, die einem virtuellen Computer zugeordnet sind, und zwar:

* Während des Auftrags "Schutz beenden"
* Nach Abschluss des Auftrags "Schutz beenden" auf einem virtuellen Computer

So löschen Sie Sicherungsdaten auf einem virtuellen Computer mit dem Status *Schutz beendet* nach erfolgreichem Auftrag **Sicherung beenden** :

1. Navigieren Sie zur Seite **Geschützte Elemente**, wählen Sie **Virtueller Azure-Computer** als *Typ* aus, und klicken Sie auf die Schaltfläche **Auswählen**.
   
    ![VM-Typ](./media/backup-azure-manage-vms/vm-type.png)
2. Wählen Sie den virtuellen Computer aus. Der virtuelle Computer hat den Status **Schutz beendet** .
   
    ![Schutz beendet](./media/backup-azure-manage-vms/protection-stopped-b.png)
3. Klicken Sie am unteren Rand der Seite auf die Schaltfläche **LÖSCHEN** .
   
    ![Sicherung löschen](./media/backup-azure-manage-vms/delete-backup.png)
4. Wählen Sie im Assistenten **Sicherungsdaten löschen** einen Grund für das Löschen von Sicherungsdaten aus (dringend empfohlen), und klicken Sie auf **Senden**.
   
    ![Löschen von Sicherungsdaten](./media/backup-azure-manage-vms/delete-backup-data.png)
5. Dadurch wird ein Auftrag zum Löschen von Sicherungsdaten vom ausgewählten virtuellen Computer erstellt. Klicken Sie auf **Auftrag anzeigen** , um den entsprechenden Auftrag auf der Seite "Aufträge" anzuzeigen.
   
    ![Löschen von Daten erfolgreich](./media/backup-azure-manage-vms/delete-data-success.png)
   
    Nach Abschluss des Auftrags wird der Eintrag des virtuellen Computers von der Seite **Geschützte Elemente** entfernt.

## <a name="dashboard"></a>Dashboard
Auf der Seite **Dashboard** können Sie Informationen zu virtuellen Computern in Azure, ihrer Speicherung und den ihnen in den letzten 24 Stunden zugeordneten Aufträgen prüfen. Sie können den Sicherungsstatus und alle zugehörigen Sicherungsfehler anzeigen.

![Dashboard](./media/backup-azure-manage-vms/dashboard-protectedvms.png)

> [!NOTE]
> Werte im Dashboard werden einmal alle 24 Stunden aktualisiert.
> 
> 

## <a name="auditing-operations"></a>Überwachen von Vorgängen
Azure Backup bietet einen Überblick über die "Vorgangsprotokolle" der vom Kunden ausgelösten Sicherungsvorgänge, sodass leicht zu erkennen ist, welche Verwaltungsvorgänge für den Sicherungstresor ausgeführt wurden. Vorgangsprotokolle bieten eine hervorragende Grundlage für Nachbesprechungen und für die Überwachung von Sicherungsvorgängen.

Die folgenden Vorgänge werden in Vorgangsprotokollen aufgezeichnet:

* Registrieren
* Unregister 
* Konfigurieren der Schutzoptionen
* Sicherung (sowohl geplante als auch bedarfsgerechte Sicherung über BackupNow)
* Wiederherstellen
* Schutz beenden
* Löschen von Sicherungsdaten
* Richtlinie hinzufügen
* Löschen von Richtlinien
* Aktualisieren von Richtlinien
* Auftrag abbrechen

So zeigen Sie die einem Sicherungstresor entsprechenden Vorgangsprotokolle an:

1. Navigieren Sie im Azure-Portal zu **Verwaltungsdienste**, und klicken Sie dann auf die Registerkarte **Vorgangsprotokolle**.
   
    ![Vorgangsprotokolle](./media/backup-azure-manage-vms/ops-logs.png)
2. Wählen Sie in den Filtern **Sicherung** als *Typ* aus, geben Sie unter *Dienstname* den Namen des Sicherungstresors an, und klicken Sie auf **Senden**.
   
    ![Vorgangsprotokollfilter](./media/backup-azure-manage-vms/ops-logs-filter.png)
3. Wählen Sie in den Vorgangsprotokollen einen beliebigen Vorgang aus, und klicken Sie auf **Details**, um die Einzelheiten zu einem Vorgang anzuzeigen.
   
    ![Details zum Abrufen von Vorgangsprotokollen](./media/backup-azure-manage-vms/ops-logs-details.png)
   
    Der **Detail-Assistent** enthält Informationen über den ausgelösten Vorgang, die Auftrags-ID, die Ressource, für die dieser Vorgang ausgelöst wurde, und die Startzeit des Vorgangs.
   
    ![Vorgangsdetails](./media/backup-azure-manage-vms/ops-logs-details-window.png)

## <a name="alert-notifications"></a>Warnungsbenachrichtigungen
Sie können benutzerdefinierte Warnungsbenachrichtigungen für die Aufträge im Portal abrufen. Definieren Sie hierfür auf PowerShell basierende Warnungsregeln für Vorgangsprotokollereignisse. Wir empfehlen die Verwendung von *PowerShell, Version 1.3.0 oder höher*.

Der folgende Beispielbefehl dient zum Definieren einer benutzerdefinierten Benachrichtigung zur Warnung bei Sicherungsfehlern:

```
PS C:\> $actionEmail = New-AzureRmAlertRuleEmail -CustomEmail contoso@microsoft.com
PS C:\> Add-AzureRmLogAlertRule -Name backupFailedAlert -Location "East US" -ResourceGroup RecoveryServices-DP2RCXUGWS3MLJF4LKPI3A3OMJ2DI4SRJK6HIJH22HFIHZVVELRQ-East-US -OperationName Microsoft.Backup/backupVault/Backup -Status Failed -TargetResourceId /subscriptions/86eeac34-eth9a-4de3-84db-7a27d121967e/resourceGroups/RecoveryServices-DP2RCXUGWS3MLJF4LKPI3A3OMJ2DI4SRJK6HIJH22HFIHZVVELRQ-East-US/providers/microsoft.backupbvtd2/BackupVault/trinadhVault -Actions $actionEmail
```

**ResourceId**: Sie erhalten diese aus dem Popup "Vorgangsprotokolle", wie im vorherigen im Abschnitt beschrieben. „ResourceUri“ im Popupdetailfenster eines Vorgangs entspricht der „ResourceId“, die für dieses Cmdlet bereitgestellt werden muss.

**OperationName**: Hat das Format „Microsoft.Backup/backupvault/<EventName>“, wobei EventName folgende Werte aufweisen kann: Register, Unregister, ConfigureProtection, Backup, Restore, StopProtection, DeleteBackupData, CreateProtectionPolicy, DeleteProtectionPolicy, UpdateProtectionPolicy

**Status**: Unterstützte Werte sind "Started", "Succeeded" und "Failed".

**ResourceGroup**: Ressourcengruppe der Ressource, für die der Vorgang ausgelöst wird. Sie können diese aus dem ResourceId-Wert abrufen. Der Wert zwischen den Feldern */resourceGroups/* und */providers/* im ResourceId-Wert ist der Wert für ResourceGroup.

**Name**: Name der Warnungsregel.

**CustomEmails**: Geben Sie die benutzerdefinierte E-Mail-Adresse an, an die die Warnungsbenachrichtigung gesendet werden soll.

**SendToServiceOwners**: Mit dieser Option wird die Warnungsbenachrichtigung an alle Administratoren und Co-Administratoren des Abonnements gesendet. Sie kann im **New-AzureRmAlertRuleEmail** -Cmdlet verwendet werden.

### <a name="limitations-on-alerts"></a>Einschränkungen für Warnungen
Ereignisbasierte Warnungen unterliegen den folgenden Einschränkungen:

1. Warnungen werden auf allen virtuellen Computern im Sicherungstresor ausgelöst. Sie können diese Einstellung nicht so anpassen, dass Sie Warnungen für eine bestimmte Gruppe von virtuellen Computern in einem Sicherungstresor erhalten.
2. Dieses Feature befindet sich in der Vorschau. [Weitere Informationen](../monitoring-and-diagnostics/insights-powershell-samples.md#create-alert-rules)
3. Sie erhalten Warnungen von "alerts-noreply@mail.windowsazure.com". Zurzeit können Sie den E-Mail-Absender nicht ändern.

## <a name="next-steps"></a>Nächste Schritte
* [Wiederherstellen virtueller Azure-Computer](backup-azure-restore-vms.md)




<!--HONumber=Dec16_HO2-->


