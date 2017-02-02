---
title: Speichern von Sicherungen von Azure SQL-Datenbank bis zu 10 Jahre lang| Microsoft Docs
description: "Erfahren Sie, wie Azure SQL-Datenbank das Speichern von Sicherungen bis zu 10 Jahre lang unterstützt."
keywords: 
services: sql-database
documentationcenter: 
author: anosov1960
manager: jhubbard
editor: 
ms.assetid: 66fdb8b8-5903-4d3a-802e-af08d204566e
ms.service: sql-database
ms.custom: business continuity
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 11/22/2016
ms.author: carlrab; sashan
translationtype: Human Translation
ms.sourcegitcommit: 145cdc5b686692b44d2c3593a128689a56812610
ms.openlocfilehash: 8b13faf1f6cdac355cc4d22b825cc2362a50e8f9


---
# <a name="storing-azure-sql-database-backups-for-up-to-10-years"></a>Speichern von Sicherungen von Azure SQL-Datenbank bis zu 10 Jahre lang
Viele Anwendungen dienen gesetzlichen, ordnungsgemäßen oder anderen geschäftlichen Zwecken, die voraussetzen, dass die vollständige automatische Datenbanksicherung länger als der Zeitraum zwischen 7-35 Tagen beibehalten wird, der von den [automatischen Sicherungen](sql-database-automated-backups.md) der SQL-Datenbank zur Verfügung gestellt wurde.

Das Feature der **langfristigen Beibehaltung der Sicherung** lässt Sie Ihre Sicherungen von Azure SQL-Datenbank in einem Azure Recovery Services-Tresor bis zu 10 Jahre lang speichern. Sie können bis zu 1000 Datenbanken pro Tresor speichern. Sie können jede Sicherung im Tresor auswählen, um sie als eine neue Datenbank wiederherzustellen.

> [!NOTE]
> Sie können bis zu 200 Datenbanken pro Tresor innerhalb eines Zeitraums von 24 Stunden aktivieren. Darum empfehlen wir, dass Sie einen separaten Tresor für jeden Server verwenden, um die Auswirkung dieser Einschränkung zu minimieren. 
> 
> 

## <a name="how-does-sql-database-long-term-retention-work"></a>Wie funktioniert die langfristige Beibehaltung der SQL-Datenbank?

Mithilfe einer langfristigen Beibehaltung von Sicherungen können Sie einen Azure SQL-Datenbankserver einem Azure Recovery Services-Tresor zuordnen. 

* Der Tresor muss im gleichen Azure-Abonnement erstellt werden, das den SQL-Server erstellt hat und sich in der gleichen geografischen Region und Ressourcengruppe befindet. 
* Dann konfigurieren Sie eine Beibehaltungsrichtlinie für jede beliebige Datenbank. Die Richtlinie sorgt dafür, dass die wöchentlichen vollständigen Datenbanksicherungen in den Recovery Services-Tresor kopiert werden und für die angegebene Beibehaltungsdauer (bis zu 10 Jahre) aufbewahrt werden. 
* Sie können dann aus jeder dieser Sicherungen eine neue Datenbank in jedem beliebigen Server innerhalb des Abonnements wiederherstellen. Die Kopie wird vom Azure-Speicher aus vorhandenen Sicherungen erstellt und schränkt die Leistung auf die vorhandene Datenbank nicht ein.

## <a name="how-do-i-enable-long-term-retention"></a>Wie aktiviere ich die langfristige Beibehaltung?

So konfigurieren Sie die langfristige Beibehaltung der Sicherung für eine Datenbank:

1. Erstellen Sie einen Azure Recovery Services-Tresor in der gleichen Region, im gleichen Abonnement und in der gleichen Ressourcengruppe wie Ihren SQL-Datenbankserver. 
2. Registrieren Sie den Server im Tresor.
3. Erstellen Sie eine Azure Recovery Services-Schutzrichtlinie.
4. Wenden Sie die Schutzrichtlinie auf die Datenbank an, die die langfristige Beibehaltung der Sicherung benötigt.

## <a name="how-do-i-restore-a-database-stored-with-the-long-term-retention-feature"></a>Wie stelle ich eine Datenbank wieder her, die mit dem Feature „Langfristige Beibehaltung“ gespeichert wurde?

So stellen Sie Datenbanken aus einer langfristige Beibehaltung der Sicherung wieder her:

1. Listen Sie auf, wo die Sicherung gespeichert ist.
2. Listen Sie den Container auf, der Ihrem logischen Server zugeordnet ist.
3. Listen Sie die Datenquelle innerhalb des Tresors auf, die Ihrer Datenbank zugeordnet ist.
4. Listen Sie die verfügbaren Wiederherstellungspunkte auf.
5. Stellen Sie die Datenbank aus dem Wiederherstellungspunkt auf den Zielserver innerhalb Ihres Abonnements wieder her.

## <a name="how-much-does-long-term-retention-cost"></a>Wie viel kostet die langfristige Beibehaltung?

Eine langfristige Beibehaltung einer Azure SQL-Datenbank wird gemäß den [Tarifen des Azure-Sicherungsdienstes](https://azure.microsoft.com/pricing/details/backup/) in Rechnung gestellt.

Nachdem der Azure SQL-Datenbankserver im Tresor registriert ist, wird Ihnen der gesamten Speicher, der von den wöchentlichen Sicherungen im Tresor benötigt wird, berechnet.

## <a name="configuring-long-term-retention-in-the-azure-portal"></a>Konfigurieren der langfristigen Beibehaltung im Azure-Portal

Auf dem Blatt „Azure SQL-Datenbankserver“ können Sie die langfristige Beibehaltung konfigurieren und, falls notwendig, einen Azure Recovery Services-Tresor erstellen.

- Informationen zum Konfigurieren der langfristigen Beibehaltung von automatisierten Sicherungen in einem Azure Recovery Services-Tresor finden Sie unter [Configure long-term retention of automated backups in an Azure Recovery Services vault](sql-database-configure-long-term-retention.md) (Konfigurieren der langfristigen Beibehaltung von automatisierten Sicherungen in einem Azure Recovery Services-Tresor).
- Informationen zum Wiederherstellen einer Datenbank aus einer Sicherung mit langfristiger Beibehaltung finden Sie im Artikel zum [Wiederherstellen aus einer Sicherung mit langfristiger Beibehaltung](sql-database-restore-from-long-term-retention.md).
- Informationen zum Anzeigen von Sicherungen im Azure Recovery Services-Tresor finden Sie unter [Anzeigen von Sicherungen mit langfristiger Beibehaltung](sql-database-view-backups-in-vault.md).

> [!TIP]
> Ein Tutorial finden Sie unter [Erste Schritte mit der Sicherung und Wiederherstellung für Datenschutz und Wiederherstellung](sql-database-get-started-backup-recovery.md).
>

## <a name="configuring-long-term-retention-using-powershell"></a>Konfigurieren der langfristigen Beibehaltung mithilfe von PowerShell

Führen Sie die folgenden Schritte aus, um die langfristige Beibehaltung mithilfe von PowerShell zu konfigurieren.
1. Erstellen Sie einen Recovery Service-Tresors
   
   ```
   New-AzureRmResourceGroup -Name $ResourceGroupName –Location 'WestUS' 
   $vault = New-AzureRmRecoveryServicesVault -Name <string> -ResourceGroupName $ResourceGroupName -Location 'WestUS' 
   Set-AzureRmRecoveryServicesBackupProperties   -BackupStorageRedundancy LocallyRedundant  -Vault $vault
   ```
2. Registrieren Sie Ihren Azure SQL-Datenbankserver in den Tresor, damit Datenbanken auf dem Server langfristig gespeichert werden können.
   
   ```
   Set-AzureRmSqlServerBackupLongTermRetentionVault -ResourceGroupName 'RG1' -ServerName 'Server1' –ResourceId $vault.Id
   ```
3. Erstellen Sie eine Beibehaltungsrichtlinie zum Speichern der Sicherungen.
   
   ```
   #retrieve the default in-memory policy object for AzureSQLServer workload and set the retention period
   $RP1 = Get-AzureRmRecoveryServicesBackupRetentionPolicyObject -WorkloadType AzureSQLDatabase
   #Sets the retention value to two years
   $RP1.RetentionDurationType='Years'
   $RP1.RetentionCount=2
   #register the policy for use with any SQL database
   Set-AzureRMRecoveryServicesVaultContext -Vault $vault
   $policy = New-AzureRmRecoveryServicesBackupProtectionPolicy -name 'SQLBackup1' –WorkloadType AzureSQLDatabase -retentionPolicy $RP1
   ```
4. Aktivieren Sie die langfristige Beibehaltung für die SQL-Datenbank, die im Tresor gespeichert werden soll.
   
   ```
   #for your database you can select any policy created in the vault with which your server is registered
   Set-AzureRmSqlDatabaseBackupLongTermRetentionPolicy –ResourceGroupName 'RG1' –ServerName 'Server1' -DatabaseName 'DB1' -State 'enabled' -ResourceId $policy.Id
   ```
5. Führen Sie den Server auf, der dem Tresor zugeordnet ist. Jeder Server ist einem bestimmten Container im Tresor zugeordnet. Sie können die registrierten Server auflisten, indem Sie die folgenden Befehle ausführen:
   
   ```
   #each server has an associated container in the vault
   Set-AzureRMRecoveryServicesVaultContext -Vault $vault
   $container=Get-AzureRmRecoveryServicesBackupContainer –ContainerType AzureSQL   
   #each database has an associated backup item in the respective container
   Get-AzureRmRecoveryServicesBackupItem –container $container
   ```
6. Führen Sie die Datenbanken auf, die über eine Beibehaltungsrichtlinie im Container verfügen. Jede Datenbank verfügt über ein zugeordnetes Sicherungselement im jeweiligen Container. Der Name des Sicherungselements wird anhand des Datenbanknamens abgeleitet.
   
    ```
    #list the backup items in the container
    Get-AzureRmRecoveryServicesBackupItem –container $container
    ```

## <a name="restore-from-a-long-term-retention-backup"></a>So stellen Sie Datenbanken aus einer langfristigen Beibehaltung wieder her:

Führen Sie die folgenden Schritte aus, um eine Datenbank aus einer Sicherung im Azure Recovery Service-Tresor wiederherzustellen:

1. Finden Sie den Recovery Service-Container, der einem SQL-Server zugeordnet ist.
   
   ```
   #the following commands find the container associated with the server 'myserver' under resource group 'myresourcegroup'
   Set-AzureRMRecoveryServicesVaultContext -Vault $vault
   $container=Get-AzureRmRecoveryServicesBackupContainer –ContainerType AzureSQL -Name 'Sql;myresourcegroup;myserver'
   ```
2. Finden Sie das Sicherungselement, das einer Datenbank zugeordnet wurde.
   
    ``` 
    #the following command finds the backup item associated with the database 'mydb'
    $item = Get-AzureRmRecoveryServicesBackupItem -Container $container -WorkloadType AzureSQLDatabase -Name 'mydb' 
    ```
3. Finden Sie die Sicherung, aus der Sie wiederherstellen möchten.
   
   ```
   #The following command lists the backups (also known as the “recovery points”) created in the specific time period.
   $RP=Get-AzureRmRecoveryServicesBackupRecoveryPoint -Item $item –StartDate '2016-02-01' -EndDate '2016-02-20'
   ```
4. Stellen Sie aus dem Wiederherstellungspunkt in eine neue Azure SQL-Datenbank wieder her.
   
   ```
   #This command restores from a selected backup. If there are multiple recovery points in the specified range $RP[0] refers to the first one.
   Restore-AzureRMSqlDatabase –FromLongTermRetentionBackup –ResourceId $RP[0].ID TargetResourceGroupName 'RG2' -TargetServerName 'Server2' -TargetDatabaseName 'DB2' [-Edition <String>] [-ServiceObjectiveName <String>] [-ElasticPoolName <String>] [<CommonParameters>]
   ```

## <a name="disabling-long-term-retention"></a>Deaktivieren der langfristigen Beibehaltung

Der Recovery Service kümmert sich anhand der bereitgestellten Beibehaltungsrichtlinie automatisch um die Bereinigung der Sicherungen. 

* Damit die Sicherungen einer bestimmten Datenbank nicht länger zum Tresor gesendet werden, entfernen Sie die Beibehaltungsrichtlinie für diese Datenbank.
  
    ```
    Set-AzureRmSqlDatabaseBackupLongTermRetentionPolicy –ResourceGroupName 'RG1' –ServerName 'Server1' -DatabaseName 'DB1' -State 'Disabled' -ResourceId $policy.Id
    ```

> [!NOTE]
> Die Sicherungen, die sich bereits im Tresor befinden, werden davon nicht betroffen. Sie werden automatisch vom Recovery Service gelöscht, wenn ihre Beibehaltungsdauer abläuft.
> 
> 

## <a name="removing-backups-from-the-azure-recovery-services-vault"></a>Entfernen von Sicherungen aus dem Azure Recovery Services-Tresor

So entfernen sie manuell Sicherungen aus dem Tresor:

1. Identifizieren Sie den Container im Tresor für „myserver“.
   
    ```
    Set-AzureRMRecoveryServicesVaultContext -Vault $vault 
    $container=Get-AzureRmRecoveryServicesBackupContainer –ContainerType AzureSQL -FriendlyName 'myserver'
    ```
2. Identifizieren Sie das zu löschende Sicherungselement.
   
    ``` 
    $item=Get-AzureRmRecoveryServicesBackupItem –container $container -Name 'mydb'
    ```
3. Löschen Sie die Sicherungselemente (alle Sicherungen für „mydb“ Datenbank).
   
    ```
    $job = Disable-AzureRmRecoveryServicesBackupProtection –item $item -Removerecoverypoints 
    Wait-AzureRmRecoveryServicesBackupJob $job
    ```
4. Löschen Sie den Container, der „myserver“ zugeordnet ist.
   
    ```
    Unregister-AzureRmRecoveryServicesBackupContainer –Container $container $container
    ```

## <a name="long-term-retention-faq"></a>Häufig gestellte Fragen zur langfristigen Beibehaltung:

1. Frage: Kann ich bestimme Sicherungen im Tresor manuell löschen?
   Antwort: Nicht zu diesem Zeitpunkt. Der Tresor bereinigt Sicherungen nach Ablauf des Aufbewahrungszeitraums automatisch.
2. Frage: Kann ich meinen Server registrieren, um in mehr als einem Tresor Sicherungen zu speichern?
   Antwort: Nein, Sie können momentan nur Sicherungen in einem Tresor gleichzeitig speichern.
3. Frage: Kann ich über einen Tresor und Server in verschiedenen Abonnements verfügen?
   Antwort: Nein, derzeit müssen sowohl der Tresor als auch der Server im gleichen Abonnement und der gleichen Ressourcengruppe sein.
4. Frage: Kann ich einen von mir erstellten Tresor in einer anderen Region als die meines Servers verwenden?
   Antwort: Nein, der Tresor und Server müssen in der gleichen Region sein, um die Zeit für den Kopiervorgang und Gebühren für den Datenverkehr zu minimieren.
5. Frage: Wie viele Datenbanken kann ich in einem Tresor speichern?
   Antwort: Derzeit unterstützen wir nur bis zu 1000 Datenbanken pro Tresor. 
6. F: Wie viele Tresore kann ich pro Abonnement erstellen? Antwort: Sie können bis zu 25 Tresore pro Abonnement erstellen.
7. F: Wie viele Datenbanken kann ich pro Tresor und pro Tag konfigurieren? Antwort: Sie können nur 200 Datenbanken pro Tresor und pro Tag einrichten.
8. Frage: Funktioniert die langfristige Beibehaltung mit elastischen Pools?
   A: Ja. Jede Datenbank im Pool kann mit der Beibehaltungsrichtlinie konfiguriert werden.
9. Frage: Kann ich den Zeitpunkt auswählen, an dem die Sicherungen erstellt werden?
   Antwort: SQL-Datenbanken kontrollieren den Zeitplan der Sicherungen, um die Auswirkungen auf die Leistung an Ihren Datenbanken zu minimieren.
10. Frage: Ich habe TDE für meine Datenbank aktiviert. Kann ich TDE mit dem Tresor verwenden? Antwort: Ja, TDE wird unterstützt. Selbst wenn die ursprüngliche Datenbank nicht mehr vorhanden ist, können Sie die Datenbank aus dem Tresor wiederherstellen.
11. F: Was geschieht mit den Sicherungen im Tresor, wenn mein Abonnement unterbrochen wird? Antwort: Falls Ihr Abonnement unterbrochen wird, werden wir die vorhandenen Datenbanken und Sicherungen beibehalten, aber neue Sicherungen werden nicht in den Tresor kopiert. Nachdem Sie das Abonnement reaktiviert haben, setzt der Dienst das Kopieren der Sicherungen in den Tresor fort. Ihr Tresor erhält wieder Zugriff auf die Wiederherstellungsvorgänge, indem die Sicherungen genutzt werden, die vor dem Unterbrechen des Abonnements kopiert wurden. 
12. Frage: Kann ich Zugriff auf die SQL-Datenbank-Sicherungsdateien erhalten, damit ich sie herunterladen/auf dem SQL Server wiederherstellen kann?
   Antwort: Derzeit ist dies nicht möglich.
13. Frage: Ist es möglich, mehrere Zeitpläne (täglich, wöchentlich, monatlich, jährlich) in einer SQL-Beibehaltungsrichtlinie zu haben?
   Antwort: Nein, das ist zu diesem Zeitpunkt nur für Sicherungen von virtuellen Computern verfügbar.

## <a name="next-steps"></a>Nächste Schritte
Datenbanksicherungen sind ein wesentlicher Bestandteil jeder Strategie für Geschäftskontinuität und Notfallwiederherstellung, da Ihre Daten vor versehentlichen Beschädigungen und Löschungen geschützt werden. Weitere Informationen zu den anderen Geschäftskontinuitätslösungen von Azure SQL-Datenbank finden Sie unter [Übersicht über die Geschäftskontinuität mit Azure SQL-Datenbank](sql-database-business-continuity.md).




<!--HONumber=Dec16_HO2-->


