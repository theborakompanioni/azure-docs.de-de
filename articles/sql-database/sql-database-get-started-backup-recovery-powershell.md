---
title: 'PowerShell: Sichern und Wiederherstellen von Azure SQL-Datenbanken | Microsoft-Dokumentation'
description: In diesem Tutorial wird gezeigt, wie Sie mithilfe von PowerShell Daten aus automatisierten Sicherungen in einem bestimmten Zustand wiederherstellen, automatisierte Sicherungen im Azure Recovery Services-Tresor speichern und Daten aus dem Azure Recovery Services-Tresor wiederherstellen.
keywords: Tutorial zur SQL-Datenbank
services: sql-database
documentationcenter: 
author: stevestein
manager: jhubbard
editor: 
ms.assetid: 
ms.service: sql-database
ms.custom: tutorial
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: PowerShell
ms.topic: hero-article
ms.date: 12/19/2016
ms.author: sstein
translationtype: Human Translation
ms.sourcegitcommit: 97acd09d223e59fbf4109bc8a20a25a2ed8ea366
ms.openlocfilehash: 85897322bc341f549c52dbe4758e8c7f860df22c
ms.lasthandoff: 03/10/2017


---


# <a name="tutorial-back-up-and-restore-an-azure-sql-database-using-powershell"></a>Tutorial: Sichern und Wiederherstellen einer Azure SQL-Datenbank mithilfe von PowerShell

In diesem Tutorial erfahren Sie, wie Sie mithilfe von Azure PowerShell Folgendes ausführen:

- Anzeigen vorhandener Sicherungen einer Datenbank
- Wiederherstellen eines früheren Zustands einer Datenbank
- Konfigurieren der langfristigen Beibehaltung einer Datenbanksicherungsdatei im Azure Recovery Services-Tresor
- Wiederherstellen einer Datenbank aus dem Azure Recovery Services-Tresor

**Geschätzte Zeit**: Dieses Tutorial ist in ca. 30 Minuten abgeschlossen (sofern die Voraussetzungen bereits erfüllt sind).


## <a name="prerequisites"></a>Voraussetzungen

* **Ein Azure-Konto.** Sie benötigen ein Azure-Konto. Sie können entweder ein [kostenloses Azure-Konto erstellen](https://azure.microsoft.com/free/) oder [Visual Studio-Abonnementvorteile aktivieren](https://azure.microsoft.com/pricing/member-offers/msdn-benefits/). 

* **Azure-Erstellungsberechtigungen.** Sie müssen mit einem Konto, das über die Rolle „Besitzer“ oder „Mitwirkender“ für das Abonnement verfügt, eine Verbindung mit dem Azure-Portal herstellen können. Weitere Informationen zur rollenbasierten Zugriffssteuerung finden Sie unter [Erste Schritte mit der Zugriffsverwaltung im Azure-Portal](../active-directory/role-based-access-control-what-is.md).

* **PowerShell** Die aktuelle Version von Azure PowerShell muss installiert sein und ausgeführt werden. Weitere Informationen finden Sie unter [Installieren und Konfigurieren von Azure PowerShell](/powershell/azureps-cmdlets-docs).

* **SQL Server Management Studio**: Die neueste Version von SQL Server Management Studio (SSMS) können Sie über [Herunterladen von SQL Server Management Studio (SSMS)](https://msdn.microsoft.com/library/mt238290.aspx) herunterladen und installieren. Verwenden Sie immer die neueste Version von SSMS, wenn Sie eine Verbindung mit Azure SQL-Datenbank herstellen, da immer wieder neue Funktionen veröffentlicht werden.

* **Basisserver und -datenbanken** Zum Installieren und Konfigurieren eines Servers und der beiden Datenbanken, die in diesem Tutorial verwendet werden, klicken Sie auf die Schaltfläche **Bereitstellen in Azure**. Durch Klicken auf die Schaltfläche wird das Blatt **Deploy from a template** (Bereitstellen aus einer Vorlage) geöffnet. Erstellen Sie eine neue Ressourcengruppe, und geben Sie das **Administrator-Anmeldekennwort** für den neuen Server an, der erstellt wird:

   [![Herunterladen](http://azuredeploy.net/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fsqldbtutorial.blob.core.windows.net%2Ftemplates%2Fsqldbgetstarted.json)

> [!TIP]
> Sie können diese Aufgaben auch in einem Einstiegstutorial über das [Azure-Portal](sql-database-get-started-backup-recovery-portal.md) durchführen.

[!INCLUDE [Start your PowerShell session](../../includes/sql-database-powershell.md)]

## <a name="view-the-oldest-restore-point-from-the-service-generated-backups-of-a-database"></a>Anzeigen des ältesten Wiederherstellungspunkts aus den vom Dienst generierten Sicherungen einer Datenbank

In diesem Abschnitt des Tutorials zeigen Sie Informationen zum ältesten Wiederherstellungspunkt aus den [vom Dienst generierten automatisierten Sicherungen](sql-database-automated-backups.md) Ihrer Datenbank an. 

Sie können eine Datenbank auf einen beliebigen Zeitpunkt zwischen dem ältesten Wiederherstellungspunkt und der letzten verfügbaren Sicherung (sechs Minuten vor dem aktuellen Zeitpunkt) wiederherstellen. 

Im folgenden Codeausschnitt wird mithilfe des Cmdlets [Get-AzureRmSqlDatabase](https://docs.microsoft.com/powershell/resourcemanager/azurerm.sql/v2.3.0/get-azurermsqldatabase) der älteste Wiederherstellungspunkt der wiederherzustellenden Datenbank wiederhergestellt. Die Zeit wird als zwar UTC zurückgegeben, in den folgenden Codeausschnitten wird jedoch gezeigt, wie Sie die Ortszeit verwenden können. Der neueste verfügbare Wiederherstellungspunkt einer aktiven Datenbank liegt üblicherweise ca. sechs Minuten zurück. Daher werden von der aktuellen Zeit einfach sechs Minuten abgezogen, um den neuesten Wiederherstellungspunkt zu erhalten. 

```
# Get available restore points

$resourceGroupName = "{resource-group-name}"
$serverName = "{server-name}"
$databaseName = "{database-name}"

$databaseToRestore = Get-AzureRmSqlDatabase -ResourceGroupName $resourceGroupName -ServerName $serverName -DatabaseName $databaseName

$earliestRestorePoint = $databaseToRestore.EarliestRestoreDate.ToLocalTime()
$latestRestorePoint = (Get-Date).AddMinutes(-6).ToLocalTime()

Write-Host "'$databaseName' on '$serverName' can be restored to any point-in-time between '$earliestRestorePoint' thru '$latestRestorePoint'"
```



## <a name="restore-a-database-to-a-previous-point-in-time"></a>Wiederherstellen eines früheren Zustands einer Datenbank

In diesem Abschnitt des Tutorials stellen Sie einen bestimmten Zustand Ihrer Datenbank in einer neuen Datenbank wieder her. 

>[!NOTE]
>Der Server, auf dem Sie einen bestimmten Zustand wiederherstellen, kann nicht geändert werden. Verwenden Sie [Geowiederherstellung](sql-database-disaster-recovery.md#recover-using-geo-restore), wenn Sie die Datenbank auf einem anderen Server wiederherstellen möchten. Sie können die Datenbank auch in einem [Pool für elastische Datenbanken](sql-database-elastic-jobs-overview.md) oder unter einem anderen Tarif wiederherstellen. 

Im folgenden Codeausschnitt wird mithilfe des Cmdlets [Restore-AzureRmSqlDatabase](https://docs.microsoft.com/powershell/resourcemanager/azurerm.sql/v2.3.0/restore-azurermsqldatabase) das im vorherigen Codeausschnitt festgelegte Element „$databaseToRestore“ wiederhergestellt. Für den Parameter **-PointInTime** muss ein Zeitwert im UTC-Format angegeben werden, der in etwa wie folgt aussieht: *12/09/2016 20:00:00*. Der Zeitwert wird durch den Codeausschnitt in die Ortszeit konvertiert.

```
# Restore a database to a previous point in time

#$resourceGroupName = {resource-group-name}
#$serverName = {server-name}
$newRestoredDatabaseName = "{new-database-name}"
$localTimeToRestoreTo = "{12/9/2016 12:00:00 PM}" # change to a valid restore point for your database
$restorePointInTime = (Get-Date $localTimeToRestoreTo).ToUniversalTime()
$newDatabaseEdition = "Basic"
$newDatabaseServiceLevel = "Basic"

Write-Host "Restoring database '$databaseName' to its state at:"(Get-Date $restorePointInTime).ToLocalTime()", to a new database named '$newRestoredDatabaseName'."

$restoredDb = Restore-AzureRmSqlDatabase -FromPointInTimeBackup -PointInTime $restorePointInTime -ResourceGroupName $resourceGroupName `
 -ServerName $serverName -TargetDatabaseName $newRestoredDatabaseName -Edition $newDatabaseEdition -ServiceObjectiveName $newDatabaseServiceLevel `
 -ResourceId $databaseToRestore.ResourceID

$restoredDb
```

> [!NOTE]
> Hier können Sie mithilfe von [SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx) eine Verbindung mit der wiederhergestellten Datenbank herstellen, um erforderliche Aufgaben durchzuführen. So können Sie beispielsweise [einen Teil der Daten aus der wiederhergestellten Datenbank extrahieren und in die vorhandene Datenbank kopieren oder die vorhandene Datenbank löschen und die wiederhergestellte Datenbank mit dem Namen der vorhandenen Datenbank benennen](sql-database-recovery-using-backups.md#point-in-time-restore).

## <a name="configure-long-term-retention-of-automated-backups-in-an-azure-recovery-services-vault"></a>Konfigurieren der langfristigen Beibehaltung von automatisierten Sicherungen in einem Azure Recovery Services-Tresor 

In diesem Abschnitt des Tutorials [konfigurieren Sie einen Azure Recovery Services-Tresor zur Beibehaltung von automatisierten Sicherungen](sql-database-long-term-retention.md) für einen längeren Zeitraum als gemäß der Beibehaltungsdauer für Ihre Dienstebene (bis zu zehn Jahre) vorgesehen ist. 


> [!TIP]
> Informationen zum Löschen von Sicherungen mit langfristiger Aufbewahrung finden Sie unter [Manage long-term backup retention using PowerShell](sql-database-manage-long-term-backup-retention-powershell.md) (Verwalten von Sicherungen mit langer Aufbewahrungsdauer per PowerShell).


### <a name="create-a-recovery-services-vault"></a>Erstellen eines Recovery Services-Tresors

> [!IMPORTANT]
> Der Tresor muss sich in der gleichen Region wie der logische SQL Azure-Server befinden und die gleiche Ressourcengruppe wie der logische Server verwenden.

Im folgenden Codeausschnitt wird mithilfe der Cmdlets [New-AzureRmRecoveryServicesVault](https://docs.microsoft.com/powershell/resourcemanager/azurerm.recoveryservices/v2.3.0/new-azurermrecoveryservicesvault) und [Set-AzureRmRecoveryServicesBackupProperties](https://docs.microsoft.com/powershell/resourcemanager/azurerm.recoveryservices/v2.3.0/set-azurermrecoveryservicesbackupproperties) ein neuer Recovery Services-Tresor erstellt und die Redundanzebene für Sicherungen im Tresor festgelegt. 

```
# Create a recovery services vault

#$resourceGroupName = "{resource-group-name}"
#$serverName = "{server-name}"
$serverLocation = (Get-AzureRmSqlServer -ServerName $serverName -ResourceGroupName $resourceGroupName).Location
$recoveryServiceVaultName = "{new-vault-name}"

$vault = New-AzureRmRecoveryServicesVault -Name $recoveryServiceVaultName -ResourceGroupName $ResourceGroupName -Location $serverLocation 
Set-AzureRmRecoveryServicesBackupProperties -BackupStorageRedundancy LocallyRedundant -Vault $vault
```

### <a name="set-your-server-to-use-the-recovery-vault-you-just-created-for-its-long-term-retention-backups"></a>Konfigurieren des Servers für die Verwendung des Recovery Services-Tresors, den Sie soeben für die langfristige Aufbewahrung von Sicherungen erstellt haben

Im folgenden Codeausschnitt wird der zuvor erstellte Recovery Services-Tresor mithilfe des Cmdlets [Set-AzureRmSqlServerBackupLongTermRetentionVault](https://docs.microsoft.com/powershell/resourcemanager/azurerm.sql/v2.3.0/set-azurermsqlserverbackuplongtermretentionvault) einem bestimmten Azure SQL-Server zugeordnet.

```
# Set your server to use the vault to for long-term backup retention 

Set-AzureRmSqlServerBackupLongTermRetentionVault -ResourceGroupName $resourceGroupName -ServerName $serverName -ResourceId $vault.Id
```

### <a name="create-a-retention-policy"></a>Erstellen einer Aufbewahrungsrichtlinie

In einer Aufbewahrungsrichtlinie wird festgelegt, wie lange eine Datenbanksicherung aufbewahrt werden soll. 

Im folgenden Codeausschnitt wird mithilfe des Cmdlets [Get-AzureRmRecoveryServicesBackupRetentionPolicyObject](https://docs.microsoft.com/powershell/resourcemanager/azurerm.recoveryservices.backup/v2.3.0/get-azurermrecoveryservicesbackupretentionpolicyobject) die standardmäßige Aufbewahrungsrichtlinie abgerufen, um sie bei der Erstellung neuer Richtlinien als Vorlage heranzuziehen. Wir konfigurieren unsere Vorlage so, dass die Sicherung zwei Jahre lang aufbewahrt wird, und führen dann das Cmdlet [New-AzureRmRecoveryServicesBackupProtectionPolicy](https://docs.microsoft.com/powershell/resourcemanager/azurerm.recoveryservices.backup/v2.3.0/new-azurermrecoveryservicesbackupprotectionpolicy) aus, um unsere neue Richtlinie zu erstellen. 

Da vor der Ausführung mancher Cmdlets zuerst der Tresorkontext festgelegt werden muss ([Set-AzureRmRecoveryServicesVaultContext](https://docs.microsoft.com/powershell/resourcemanager/azurerm.recoveryservices/v2.3.0/set-azurermrecoveryservicesvaultcontext)), ist dieses Cmdlet in einigen verwandten Codeausschnitten vorhanden. Der Kontext wird festgelegt, da die Richtlinie Teil des Tresors ist. Sie können mehrere Aufbewahrungsrichtlinien pro Tresor erstellen und dann die gewünschte Richtlinie auf bestimmte Datenbanken anwenden. 


```
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

Im folgenden Codeausschnitt wird unsere neue Richtlinie mithilfe des Cmdlets [Set-AzureRmSqlDatabaseBackupLongTermRetentionPolicy](https://docs.microsoft.com/powershell/resourcemanager/azurerm.sql/v2.3.0/set-azurermsqldatabasebackuplongtermretentionpolicy) auf eine bestimmte Datenbank angewendet.

```
# Enable long-term retention for a specific SQL database
$policyState = "enabled"
Set-AzureRmSqlDatabaseBackupLongTermRetentionPolicy -ResourceGroupName $resourceGroupName -ServerName $serverName -DatabaseName $databaseName -State $policyState -ResourceId $policy.Id
```

> [!IMPORTANT]
> Nach der Konfiguration werden innerhalb der nächsten sieben Tage Sicherungen im Tresor angezeigt. Setzen Sie dieses Tutorial fort, wenn Sicherungen im Tresor angezeigt werden.

## <a name="view-backup-info-and-backups-in-long-term-retention"></a>Anzeigen von Sicherungsinformationen sowie von Sicherungen mit langfristiger Aufbewahrung

In diesem Abschnitt des Tutorials zeigen Sie Informationen zu Ihren Datenbanksicherungen mit [langfristiger Beibehaltung der Sicherung](sql-database-long-term-retention.md) an. 

Die folgenden Codeausschnitte dienen zum Abfragen von Informationen aus dem Tresor. Hierzu werden die Cmdlets [Get-AzureRmRecoveryServicesBackupContainer](https://docs.microsoft.com/en-us/powershell/resourcemanager/azurerm.recoveryservices.backup/v2.3.0/get-azurermrecoveryservicesbackupcontainer), [Get-AzureRmRecoveryServicesBackupItem](https://docs.microsoft.com/en-us/powershell/resourcemanager/azurerm.recoveryservices.backup/v2.3.0/get-azurermrecoveryservicesbackupitem) und [Get-AzureRmRecoveryServicesBackupRecoveryPoint](https://docs.microsoft.com/en-us/powershell/resourcemanager/azurerm.recoveryservices.backup/v2.3.0/get-azurermrecoveryservicesbackuprecoverypoint) verwendet.

```
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


## <a name="restore-a-database-from-a-backup-in-long-term-backup-retention"></a>Wiederherstellen einer Datenbank aus einer Sicherung mit langfristiger Beibehaltung

In diesem Abschnitt des Tutorials stellen Sie die Datenbank aus einer Sicherung im Azure Recovery Services-Tresor in einer neuen Datenbank wieder her.

Genau wie in den Codeausschnitten für die Zeitpunktwiederherstellung weiter oben in diesem Tutorial wird auch beim Wiederherstellen langfristig aufbewahrter Sicherungen das Cmdlet [Restore-AzureRmSqlDatabase](https://docs.microsoft.com/powershell/resourcemanager/azurerm.sql/v2.3.0/restore-azurermsqldatabase) verwendet – diesmal allerdings mit dem Parameter **-FromLongTermRetentionBackup** (anstelle des zuvor verwendeten Parameters **-FromPointInTimeBackup**).

```
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

## <a name="complete-azure-powershell-script-to-restore-from-a-previous-point-in-time-and-to-configure-and-restore-from-long-term-backup-retention-using-powershell"></a>Vollständiges Azure PowerShell-Skript zum Wiederherstellen eines früheren Zeitpunkts sowie zum Konfigurieren und Wiederherstellen aus einer langfristigen Sicherung mithilfe von PowerShell

> [!NOTE]
> Beim Wiederherstellen der neuesten Sicherung am Ende dieses Skripts tritt die Ausnahme *Es ist nicht möglich, einen Index auf ein NULL-Array anzuwenden.* auf, wenn im Tresor keine Sicherung vorhanden ist.

```
# Sign in to Azure and set the subscription to work with
########################################################

$subscriptionId = "{subscription-id}"

Add-AzureRmAccount
Set-AzureRmContext -SubscriptionId $subscriptionId


# User variables
################

$myResourceGroupName = "{resource-group-name}"
$myServerName = "{server-name}"
$myDatabaseToRestoreFromPointInTime = "{database-name}"
$myLocalTimeToRestoreTo = "{12/08/2016 16:00:00}" # change to a valid restore point for your database
$myNewDatabaseRestoredFromPointInTime = "{new-database-name}"

$myRecoveryServiceVaultName = "{vault-name}"
$myRetentionPolicyDurationType = "{duration-period}" # set to Years, Months, or Weeks
$myRetentionPolicyDuration = {2}
$myRetentionPolicyName = "{retention-policy-name}"
$myDatabaseToRestoreFromLTR = "{database-name}"
$myNewDatabaseRestoredFromLTR = "{new-database-name}"


# Get available restore points for a specific database
######################################################

$resourceGroupName = $myResourceGroupName
$serverName = $myServerName
$databaseName = $myDatabaseToRestoreFromPointInTime


$databaseToRestore = Get-AzureRmSqlDatabase -ResourceGroupName $resourceGroupName -ServerName $serverName -DatabaseName $databaseName

$earliestRestorePoint = $databaseToRestore.EarliestRestoreDate.ToLocalTime()
$latestRestorePoint = (Get-Date).AddMinutes(-6).ToLocalTime()

Write-Host "'$databaseName' on '$serverName' can be restored to any point-in-time between '$earliestRestorePoint' thru '$latestRestorePoint'"


# Restore a database to a previous point in time
################################################

$newRestoredDatabaseName = $myNewDatabaseRestoredFromPointInTime
$localTimeToRestoreTo = $myLocalTimeToRestoreTo
$restorePointInTime = (Get-Date $localTimeToRestoreTo).ToUniversalTime()
$newDatabaseEdition = "Basic"
$newDatabaseServiceLevel = "Basic"

Write-Host "Restoring database '$databaseName' to its state at:"(Get-Date $restorePointInTime).ToLocalTime()", to a new database named '$newRestoredDatabaseName'."

$restoredDb = Restore-AzureRmSqlDatabase -FromPointInTimeBackup -PointInTime $restorePointInTime -ResourceGroupName $resourceGroupName `
 -ServerName $serverName -TargetDatabaseName $newRestoredDatabaseName -Edition $newDatabaseEdition -ServiceObjectiveName $newDatabaseServiceLevel `
 -ResourceId $databaseToRestore.ResourceID

$restoredDb



# CONFIGURE LONG-TERM RETENTION

# Create a recovery services vault
##################################

$resourceGroupName = $myResourceGroupName
$serverName = $myServerName
$serverLocation = (Get-AzureRmSqlServer -ServerName $serverName -ResourceGroupName $resourceGroupName).Location
$recoveryServiceVaultName = $myRecoveryServiceVaultName

$vault = New-AzureRmRecoveryServicesVault -Name $recoveryServiceVaultName -ResourceGroupName $resourceGroupName -Location $serverLocation 
Set-AzureRmRecoveryServicesBackupProperties -BackupStorageRedundancy LocallyRedundant -Vault $vault
$vault

Set-AzureRmSqlServerBackupLongTermRetentionVault -ResourceGroupName $resourceGroupName -ServerName $serverName -ResourceId $vault.Id

# Retrieve the default retention policy for the AzureSQLDatabase workload type
##############################################################################

$retentionPolicy = Get-AzureRmRecoveryServicesBackupRetentionPolicyObject -WorkloadType AzureSQLDatabase

# Set the retention values
$retentionPolicy.RetentionDurationType = $myRetentionPolicyDurationType
$retentionPolicy.RetentionCount = $myRetentionPolicyDuration

$retentionPolicyName = $myRetentionPolicyName

# Set the vault context to the vault you are creating the policy for
Set-AzureRmRecoveryServicesVaultContext -Vault $vault

# Create the new policy
$policy = New-AzureRmRecoveryServicesBackupProtectionPolicy -name $retentionPolicyName -WorkloadType AzureSQLDatabase -retentionPolicy $retentionPolicy
$policy

$policyState = "enabled"
Set-AzureRmSqlDatabaseBackupLongTermRetentionPolicy -ResourceGroupName $resourceGroupName -ServerName $serverName -DatabaseName $databaseName -State $policyState -ResourceId $policy.Id


$databaseNeedingRestore = $myDatabaseToRestoreFromLTR

# Set the vault context to the vault we want to restore from
Set-AzureRmRecoveryServicesVaultContext -Vault $vault

# Get the container associated with your vault
$container = Get-AzureRmRecoveryServicesBackupContainer -ContainerType AzureSQL -FriendlyName $vault.Name

# Get the long-term retention metadata associated with a specific database
$item = Get-AzureRmRecoveryServicesBackupItem -Container $container -WorkloadType AzureSQLDatabase -Name $databaseNeedingRestore


# Get all available backups for the previously indicated database
# Optionally, set the -StartDate and -EndDate parameters to return backups within a specific time period
$availableBackups = Get-AzureRmRecoveryServicesBackupRecoveryPoint -Item $item
$availableBackups
###

# This command restores the most recent backup: $availableBackups[0]
$resourceGroupName = $myResourceGroupName
$serverName = $myServerName
$restoredDatabaseName = $myNewDatabaseRestoredFromLTR
$edition = "Basic"
$performanceLevel = "Basic"

$restoredDbFromLtr = Restore-AzureRmSqlDatabase -FromLongTermRetentionBackup -ResourceId $availableBackups[0].Id -ResourceGroupName $resourceGroupName `
 -ServerName $serverName -TargetDatabaseName $restoredDatabaseName -Edition $edition -ServiceObjectiveName $performanceLevel

$restoredDbFromLtr
```

## <a name="next-steps"></a>Nächste Schritte

- Weitere Informationen zu vom Dienst generierten automatischen Sicherungen finden Sie im Artikel zu [automatischen Sicherungen](sql-database-automated-backups.md).
- Weitere Informationen zur langfristigen Beibehaltung von Sicherungen finden Sie im Artikel zur [langfristigen Beibehaltung von Sicherungen](sql-database-long-term-retention.md).
- Weitere Informationen zum Wiederherstellen von Daten aus Sicherungen finden Sie im Artikel zur [Wiederherstellung aus einer Sicherung](sql-database-recovery-using-backups.md).

