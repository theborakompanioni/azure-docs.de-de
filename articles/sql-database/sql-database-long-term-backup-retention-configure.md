---
title: "Konfigurieren einer langfristig aufbewahrten Sicherung – Azure SQL-Datenbank | Microsoft-Dokumentation"
description: Lernen Sie, wie automatisierte Sicherungen im Azure Recovery Services-Tresor gespeichert und aus dem Azure Recovery Services-Tresor wiederhergestellt werden.
services: sql-database
documentationcenter: 
author: CarlRabeler
manager: jhubbard
editor: 
ms.assetid: aeb8c4c3-6ae2-45f7-b2c3-fa13e3752eed
ms.service: sql-database
ms.custom: business continuity
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/10/2017
ms.author: carlrab
translationtype: Human Translation
ms.sourcegitcommit: aaf97d26c982c1592230096588e0b0c3ee516a73
ms.openlocfilehash: 5fea9dfcd323ecf497742173a66119be4f734909
ms.lasthandoff: 04/27/2017


---
# <a name="configure-and-restore-from-azure-sql-database-long-term-backup-retention"></a>Konfiguration einer langfristig aufbewahrten Sicherung einer Azure SQL-Datenbank und die Wiederherstellung daraus

Sie können konfigurieren, dass der Azure Recovery Services-Tresor Azure SQL-Datenbanksicherungen speichert und dann eine Datenbank mithilfe im Tresor aufbewahrter Sicherungen mit dem Azure-Portal oder PowerShell wiederherstellen.

## <a name="azure-portal"></a>Azure-Portal

Die folgenden Abschnitte zeigen Ihnen, wie Sie das Azure-Portal zum Konfigurieren des Azure Recovery Services-Tresors verwenden, Sicherungen im Tresor anzeigen und aus dem Tresor wiederherstellen.

### <a name="configure-the-vault-register-the-server-and-select-databases"></a>Konfigurieren des Tresors, Registrieren des Servers und Auswählen der Datenbanken

Sie [konfigurieren einen Azure Recovery Services-Tresor zur Aufbewahrung von automatisierten Sicherungen](sql-database-long-term-retention.md) für einen längeren Zeitraum, als gemäß der Aufbewahrungsdauer für Ihre Dienstebene vorgesehen ist. 

> [!TIP]
> Informationen zum Löschen von Sicherungen mit langer Aufbewahrungsdauer finden Sie unter [Konfiguration einer langfristig aufbewahrten Sicherung einer Azure SQL-Datenbank und die Wiederherstellung daraus](sql-database-long-term-backup-retention-configure.md).
>

1. Öffnen Sie die Seite **SQL Server** für Ihren Server.

   ![SQL Server-Seite](./media/sql-database-get-started-portal/sql-server-blade.png)

2. Klicken Sie auf **Long-term backup retention** (Langfristige Beibehaltung der Sicherung).

   ![Link „Long-term backup retention“ (Langfristige Beibehaltung der Sicherung)](./media/sql-database-get-started-backup-recovery/long-term-backup-retention-link.png)

3. Lesen Sie auf der Seite **Langfristige Sicherungsaufbewahrung** für Ihren Server die Preview-Bedingungen, und akzeptieren Sie sie. (Dies ist nicht erforderlich, wenn Sie diesen Schritt bereits ausgeführt haben oder sich das Feature nicht mehr in der Vorschauphase befindet.)

   ![Preview-Bedingungen akzeptieren](./media/sql-database-get-started-backup-recovery/accept-the-preview-terms.png)

4. Um die langfristige Sicherungsaufbewahrung zu konfigurieren, wählen Sie die Datenbank im Raster aus und klicken dann auf der Symbolleiste auf **Konfigurieren**.

   ![Datenbank für langfristige Beibehaltung der Sicherung auswählen](./media/sql-database-get-started-backup-recovery/select-database-for-long-term-backup-retention.png)

5. Klicken Sie auf der Seite **Konfigurieren** unter **Recovery Services-Tresor** auf **Erforderliche Einstellungen konfigurieren**.

   ![Link zum Konfigurieren des Tresors](./media/sql-database-get-started-backup-recovery/configure-vault-link.png)

6. Wählen Sie auf der Seite **Recovery Services-Tresor** einen vorhandenen Tresor aus. Falls kein Recovery Services-Tresor für Ihr Abonnement vorhanden ist, klicken Sie auf den entsprechenden Link, um den Vorgang zu beenden, und erstellen Sie einen Recovery Services-Tresor.

   ![Tresorlink erstellen](./media/sql-database-get-started-backup-recovery/create-new-vault-link.png)

7. Klicken Sie auf der Seite **Recovery Services-Tresore** auf **Hinzufügen**.

   ![Tresorlink hinzufügen](./media/sql-database-get-started-backup-recovery/add-new-vault-link.png)
   
8. Geben Sie auf der Seite **Recovery Services-Tresor** einen gültigen Namen für den Recovery Services-Tresor ein.

   ![Neuer Tresorname](./media/sql-database-get-started-backup-recovery/new-vault-name.png)

9. Wählen Sie Ihr Abonnement und die Ressourcengruppe und dann den Standort für den Tresor aus. Klicken Sie anschließend auf **Erstellen**.

   ![Tresor erstellen](./media/sql-database-get-started-backup-recovery/create-new-vault.png)

   > [!IMPORTANT]
   > Der Tresor muss sich in der gleichen Region wie der logische SQL Azure-Server befinden und die gleiche Ressourcengruppe wie der logische Server verwenden.
   >

10. Führen Sie nach der Erstellung des neuen Tresors die notwendigen Schritte durch, um zur Seite **Recovery Services-Tresor** zurückzukehren.

11. Klicken Sie auf der Seite **Recovery Services-Tresor** auf den Tresor und dann auf **Auswählen**.

   ![Vorhandenen Tresor auswählen](./media/sql-database-get-started-backup-recovery/select-existing-vault.png)

12. Geben Sie auf der Seite **Konfigurieren** einen gültigen Namen für die neue Aufbewahrungsrichtlinie ein, ändern Sie die Standardaufbewahrungsrichtlinie wie erforderlich, und klicken Sie dann auf **OK**.

   ![Aufbewahrungsrichtlinie definieren](./media/sql-database-get-started-backup-recovery/define-retention-policy.png)

13. Klicken Sie auf der Seite **Langfristige Sicherungsaufbewahrung** für Ihre Datenbank auf **Speichern** und dann auf **OK**, um die Richtlinie für langfristige Beibehaltung der Sicherung auf alle ausgewählten Datenbanken anzuwenden.

   ![Aufbewahrungsrichtlinie definieren](./media/sql-database-get-started-backup-recovery/save-retention-policy.png)

14. Klicken Sie auf **Speichern**, um die langfristige Sicherungsaufbewahrung anhand der neuen Richtlinie für den von Ihnen konfigurierten Azure Recovery Services-Tresor zu aktivieren.

   ![Aufbewahrungsrichtlinie definieren](./media/sql-database-get-started-backup-recovery/enable-long-term-retention.png)

> [!IMPORTANT]
> Nach der Konfiguration werden innerhalb der nächsten sieben Tage Sicherungen im Tresor angezeigt. Setzen Sie dieses Tutorial erst fort, wenn Sicherungen im Tresor angezeigt werden.
>

### <a name="view-backups-in-long-term-retention-using-azure-portal"></a>Anzeigen von Sicherungen mit langfristiger Beibehaltung mit dem Azure-Portal

Zeigen Sie Informationen zu Ihren Datenbanksicherungen mit [langfristiger Beibehaltung der Sicherung](sql-database-long-term-retention.md) an. 

1. Öffnen Sie im Azure-Portal den Azure Recovery Services-Tresor für Ihre Datenbanksicherungen (wechseln Sie zu **Alle Ressourcen**, und wählen Sie den Tresor in der Liste der Ressourcen für Ihr Abonnement aus), um den Speicherplatz anzuzeigen, den Ihre Datenbanksicherungen im Tresor belegen.

   ![Recovery Services-Tresor mit Sicherungen anzeigen](./media/sql-database-get-started-backup-recovery/view-recovery-services-vault-with-data.png)

2. Öffnen Sie die Seite **SQL-Datenbank** für Ihre Datenbank.

   ![Neue Beispieldatenbankseite](./media/sql-database-get-started-portal/new-sample-db-blade.png)

3. Klicken Sie auf der Symbolleiste auf **Wiederherstellen**.

   ![„Wiederherstellen“ auf der Symbolleiste](./media/sql-database-get-started-backup-recovery/restore-toolbar.png)

4. Klicken Sie auf der Seite „Wiederherstellen“ auf **Langfristig**.

5. Klicken Sie unter „Azure vault backups“ (Sicherungen im Azure-Tresor) auf **Sicherung auswählen**, um die verfügbaren Datenbanksicherungen mit langfristiger Beibehaltung anzuzeigen.

   ![Sicherungen im Tresor](./media/sql-database-get-started-backup-recovery/view-backups-in-vault.png)

### <a name="restore-a-database-from-a-backup-in-long-term-backup-retention-using-the-azure-portal"></a>Wiederherstellen einer Datenbank aus einer Sicherung mit langfristiger Beibehaltung mit dem Azure-Portal

Sie stellen die Datenbank aus einer Sicherung im Azure Recovery Services-Tresor in einer neuen Datenbank wieder her.

1. Klicken Sie auf der Seite **Azure-Tresorsicherungen** auf die Sicherung, die Sie wiederherstellen möchten, und dann auf **Auswählen**.

   ![Sicherung im Tresor auswählen](./media/sql-database-get-started-backup-recovery/select-backup-in-vault.png)

2. Geben Sie im Textfeld **Datenbankname** den Namen für die wiederhergestellte Datenbank an.

   ![Neuer Datenbankname](./media/sql-database-get-started-backup-recovery/new-database-name.png)

3. Klicken Sie auf **OK**, um die Datenbank aus der Sicherung im Tresor in der neuen Datenbank wiederherzustellen.

4. Klicken Sie auf der Symbolleiste auf das Benachrichtigungssymbol, um den Status des Wiederherstellungsauftrags anzuzeigen.

   ![Status des Wiederherstellungsauftrags (aus dem Tresor)](./media/sql-database-get-started-backup-recovery/restore-job-progress-long-term.png)

5. Öffnen Sie nach Abschluss des Wiederherstellungsauftrags die Seite **SQL-Datenbanken**, um die neu wiederhergestellte Datenbank anzuzeigen.

   ![Aus dem Tresor wiederhergestellte Datenbank](./media/sql-database-get-started-backup-recovery/restored-database-from-vault.png)

> [!NOTE]
> Auf diesem Blatt können Sie mithilfe von SQL Server Management Studio eine Verbindung mit der wiederhergestellten Datenbank herstellen, um erforderliche Aufgaben durchzuführen. Sie können beispielsweise [einen Teil der Daten aus der wiederhergestellten Datenbank extrahieren und in die vorhandene Datenbank kopieren oder die vorhandene Datenbank löschen und die wiederhergestellte Datenbank in den vorhandenen Datenbanknamen umbenennen](sql-database-recovery-using-backups.md#point-in-time-restore).
>

## <a name="powershell"></a>PowerShell

Die folgenden Abschnitte zeigen Ihnen, wie Sie PowerShell zum Konfigurieren des Azure Recovery Services-Tresors verwenden, Sicherungen im Tresor anzeigen und aus dem Tresor wiederherstellen.

### <a name="create-a-recovery-services-vault"></a>Erstellen eines Recovery Services-Tresors

Erstellen Sie mithilfe des Cmdlets [New-AzureRmRecoveryServicesVault](/powershell/module/azurerm.recoveryservices/new-azurermrecoveryservicesvault) einen neuen Recovery Services-Tresor.

> [!IMPORTANT]
> Der Tresor muss sich in der gleichen Region wie der logische SQL Azure-Server befinden und die gleiche Ressourcengruppe wie der logische Server verwenden.

```PowerShell
# Create a recovery services vault

#$resourceGroupName = "{resource-group-name}"
#$serverName = "{server-name}"
$serverLocation = (Get-AzureRmSqlServer -ServerName $serverName -ResourceGroupName $resourceGroupName).Location
$recoveryServiceVaultName = "{new-vault-name}"

$vault = New-AzureRmRecoveryServicesVault -Name $recoveryServiceVaultName -ResourceGroupName $ResourceGroupName -Location $serverLocation 
Set-AzureRmRecoveryServicesBackupProperties -BackupStorageRedundancy LocallyRedundant -Vault $vault
```

### <a name="set-your-server-to-use-the-recovery-vault-for-its-long-term-retention-backups"></a>Konfigurieren des Servers für die Verwendung des Recovery Services-Tresors für die langfristige Aufbewahrung von Sicherungen

Ordnen Sie mithilfe des Cmdlets [Set-AzureRmSqlServerBackupLongTermRetentionVault](/powershell/module/azurerm.sql/set-azurermsqlserverbackuplongtermretentionvault) einem bestimmten Azure SQL-Server einen zuvor erstellten Recovery Services-Tresor zu.

```PowerShell
# Set your server to use the vault to for long-term backup retention 

Set-AzureRmSqlServerBackupLongTermRetentionVault -ResourceGroupName $resourceGroupName -ServerName $serverName -ResourceId $vault.Id
```

### <a name="create-a-retention-policy"></a>Erstellen einer Aufbewahrungsrichtlinie

In einer Aufbewahrungsrichtlinie wird festgelegt, wie lange eine Datenbanksicherung aufbewahrt werden soll. Rufen Sie mithilfe des Cmdlets [Get-AzureRmRecoveryServicesBackupRetentionPolicyObject](https://docs.microsoft.com/powershell/resourcemanager/azurerm.recoveryservices.backup/v2.3.0/get-azurermrecoveryservicesbackupretentionpolicyobject) die standardmäßige Aufbewahrungsrichtlinie ab, um sie bei der Erstellung neuer Richtlinien als Vorlage zu verwenden. In dieser Vorlage ist die Beibehaltungsdauer auf zwei Jahre festgelegt. Führen Sie als Nächstes [New-AzureRmRecoveryServicesBackupProtectionPolicy](/powershell/module/azurerm.recoveryservices.backup/new-azurermrecoveryservicesbackupprotectionpolicy) aus, um das Erstellen der Richtlinie abzuschließen. 

> [!NOTE]
> Bei manchen Cmdlets müssen Sie vor der Ausführung den Tresorkontext festlegen ([Set-AzureRmRecoveryServicesVaultContext](/powershell/module/azurerm.recoveryservices/set-azurermrecoveryservicesvaultcontext)), darum sehen Sie dieses Cmdlet in ein paar zugehörigen Codeausschnitten. Der Kontext wird festgelegt, da die Richtlinie Teil des Tresors ist. Sie können mehrere Aufbewahrungsrichtlinien pro Tresor erstellen und dann die gewünschte Richtlinie auf bestimmte Datenbanken anwenden. 


```PowerShell
# Retrieve the default retention policy for the AzureSQLDatabase workload type
$retentionPolicy = Get-AzureRmRecoveryServicesBackupRetentionPolicyObject -WorkloadType AzureSQLDatabase

# Set the retention value to two years (you can set to any time between 1 week and 10 years)
$retentionPolicy.RetentionDurationType = "Years"
$retentionPolicy.RetentionCount = 2
$retentionPolicyName = "my2YearRetentionPolicy"

# Set the vault context to the vault you are creating the policy for
Set-AzureRmRecoveryServicesVaultContext -Vault $vault

# Create the new policy
$policy = New-AzureRmRecoveryServicesBackupProtectionPolicy -name $retentionPolicyName -WorkloadType AzureSQLDatabase -retentionPolicy $retentionPolicy
$policy
```

### <a name="configure-a-database-to-use-the-previously-defined-retention-policy"></a>Konfigurieren einer Datenbank zum Verwenden der zuvor definierten Aufbewahrungsrichtlinie

Wenden Sie die neue Richtlinie mithilfe des Cmdlets [Set-AzureRmSqlDatabaseBackupLongTermRetentionPolicy](/powershell/module/azurerm.sql/set-azurermsqldatabasebackuplongtermretentionpolicy) auf eine bestimmte Datenbank an.

```PowerShell
# Enable long-term retention for a specific SQL database
$policyState = "enabled"
Set-AzureRmSqlDatabaseBackupLongTermRetentionPolicy -ResourceGroupName $resourceGroupName -ServerName $serverName -DatabaseName $databaseName -State $policyState -ResourceId $policy.Id
```

> [!IMPORTANT]
> Nach der Konfiguration werden innerhalb der nächsten sieben Tage Sicherungen im Tresor angezeigt. Setzen Sie dieses Tutorial fort, wenn Sicherungen im Tresor angezeigt werden.

### <a name="view-backup-info-and-backups-in-long-term-retention"></a>Anzeigen von Sicherungsinformationen sowie von Sicherungen mit langfristiger Aufbewahrung

Zeigen Sie Informationen zu Ihren Datenbanksicherungen mit [langfristiger Beibehaltung der Sicherung](sql-database-long-term-retention.md) an. 

Verwenden Sie die folgenden Cmdlets, um die Sicherungsinformationen anzuzeigen:

- [Get-AzureRmRecoveryServicesBackupContainer](/powershell/module/azurerm.recoveryservices.backup/get-azurermrecoveryservicesbackupcontainer)
- [Get-AzureRmRecoveryServicesBackupItem](/powershell/module/azurerm.recoveryservices.backup/get-azurermrecoveryservicesbackupitem)
- [Get-AzureRmRecoveryServicesBackupRecoveryPoint](/powershell/module/azurerm.recoveryservices.backup/get-azurermrecoveryservicesbackuprecoverypoint)

```PowerShell
#$resourceGroupName = "{resource-group-name}"
#$serverName = "{server-name}"
$databaseNeedingRestore = $databaseName

# Set the vault context to the vault we want to restore from
#$vault = Get-AzureRmRecoveryServicesVault -ResourceGroupName $resourceGroupName
Set-AzureRmRecoveryServicesVaultContext -Vault $vault

# the following commands find the container associated with the server 'myserver' under resource group 'myresourcegroup'
$container = Get-AzureRmRecoveryServicesBackupContainer -ContainerType AzureSQL -FriendlyName $vault.Name

# Get the long-term retention metadata associated with a specific database
$item = Get-AzureRmRecoveryServicesBackupItem -Container $container -WorkloadType AzureSQLDatabase -Name $databaseNeedingRestore

# Get all available backups for the previously indicated database
# Optionally, set the -StartDate and -EndDate parameters to return backups within a specific time period
$availableBackups = Get-AzureRmRecoveryServicesBackupRecoveryPoint -Item $item
$availableBackups
```

### <a name="restore-a-database-from-a-backup-in-long-term-backup-retention"></a>Wiederherstellen einer Datenbank aus einer Sicherung mit langfristiger Beibehaltung

Bei der Wiederherstellung aus einer Sicherung mit langfristiger Beibehaltung wird das [Restore-AzureRmSqlDatabase](/powershell/module/azurerm.sql/restore-azurermsqldatabase)-Cmdlet verwendet.

```PowerShell
# Restore the most recent backup: $availableBackups[0]
#$resourceGroupName = "{resource-group-name}"
#$serverName = "{server-name}"
$restoredDatabaseName = "{new-database-name}"
$edition = "Basic"
$performanceLevel = "Basic"

$restoredDb = Restore-AzureRmSqlDatabase -FromLongTermRetentionBackup -ResourceId $availableBackups[0].Id -ResourceGroupName $resourceGroupName `
 -ServerName $serverName -TargetDatabaseName $restoredDatabaseName -Edition $edition -ServiceObjectiveName $performanceLevel
$restoredDb
```


> [!NOTE]
> Auf diesem Blatt können Sie mithilfe von SQL Server Management Studio eine Verbindung mit der wiederhergestellten Datenbank herstellen, um erforderliche Aufgaben durchzuführen. Sie können beispielsweise [einen Teil der Daten aus der wiederhergestellten Datenbank extrahieren und in die vorhandene Datenbank kopieren oder die vorhandene Datenbank löschen und die wiederhergestellte Datenbank in den vorhandenen Datenbanknamen umbenennen](sql-database-recovery-using-backups.md#point-in-time-restore).

## <a name="next-steps"></a>Nächste Schritte

- Weitere Informationen zu vom Dienst generierten automatischen Sicherungen finden Sie im Artikel zu [automatischen Sicherungen](sql-database-automated-backups.md).
- Weitere Informationen zur langfristigen Beibehaltung von Sicherungen finden Sie im Artikel zur [langfristigen Beibehaltung von Sicherungen](sql-database-long-term-retention.md).
- Weitere Informationen zum Wiederherstellen von Daten aus Sicherungen finden Sie im Artikel zur [Wiederherstellung aus einer Sicherung](sql-database-recovery-using-backups.md).

