<properties
   pageTitle="Datenbankwiederherstellung in Azure SQL Data Warehouse (PowerShell) | Microsoft Azure"
   description="Aufgaben von PowerShell zum Wiederherstellen einer Livedatenbank, einer gelöschten Datenbank oder einer Datenbank, auf die nicht zugegriffen werden kann, in Azure SQL Data Warehouse"
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="elfisher"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="05/05/2016"
   ms.author="elfish;barbkess;sonyama"/>

# Sichern und Wiederherstellen einer Datenbank in Azure SQL Data Warehouse (PowerShell)

> [AZURE.SELECTOR]
- [Übersicht](sql-data-warehouse-overview-manage-database-restore.md)
- [Portal](sql-data-warehouse-manage-database-restore-portal.md)
- [PowerShell](sql-data-warehouse-manage-database-restore-powershell.md)
- [REST](sql-data-warehouse-manage-database-restore-rest-api.md)

Aufgaben mit PowerShell zum Wiederherstellen einer Live-Datenbank, einer gelöschten Datenbank oder einer Datenbank, auf die nicht zugegriffen werden kann, in SQL Data Warehouse

Aufgaben in diesem Thema:

- Wiederherstellen einer Livedatenbank
- Wiederherstellen einer gelöschten Datenbank
- Wiederherstellen einer Datenbank, auf die nicht zugegriffen werden kann, von einer anderen geografischen Azure-Region aus

[AZURE.INCLUDE [SQL Data Warehouse-Aufbewahrungsrichtlinie für die Sicherung](../../includes/sql-data-warehouse-backup-retention-policy.md)]


## Voraussetzungen

### Überprüfen Sie Ihre Kapazität der SQL-Datenbank-DTU. 
SQL Data Warehouse führt die Wiederherstellung so aus, dass eine neue Datenbank auf dem logischen Server erstellt wird. Daher sollten Sie sicherstellen, dass der SQL-Server, auf den Sie wiederherstellen, über ausreichend DTU-Kapazität für die neue Datenbank verfügt. Weitere Informationen zum Anzeigen und Erhöhen des DTU-Kontingents finden Sie in [diesem Blogbeitrag][].

### Installieren von PowerShell

Damit Sie Azure PowerShell mit SQL Data Warehouse verwenden können, müssen Sie Azure PowerShell Version 1.0 oder höher installieren. Sie können die Version überprüfen, indem Sie **Get-Module -ListAvailable -Name Azure** ausführen. Sie können die neueste Version installieren, indem Sie sie über [Microsoft-Web Platform Installer][] herunterladen. Weitere Informationen zum Installieren der neuesten Version finden Sie unter [Installieren und Konfigurieren von Azure PowerShell][].

## Wiederherstellen einer Livedatenbank

Verwenden Sie das PowerShell-Cmdlet [Restore-AzureRmSqlDatabase][], um eine Datenbank aus einer Momentaufnahme wiederherzustellen.

1. Öffnen Sie Windows PowerShell.
2. Stellen Sie eine Verbindung mit Ihrem Azure-Konto her, und listen Sie alle Abonnements auf, die Ihrem Konto zugeordnet sind.
3. Wählen Sie das Abonnement aus, in dem die wiederherzustellende Datenbank enthalten ist.
4. Listen Sie die Wiederherstellungspunkte für die Datenbank auf.
5. Wählen Sie den gewünschten Wiederherstellungspunkt mit RestorePointCreationDate aus.
6. Stellen Sie die Datenbank mit dem gewünschten Wiederherstellungspunkt wieder her.
7. Überprüfen Sie, ob die wiederhergestellte Datenbank online ist.

```Powershell

$SubscriptionName="<YourSubscriptionName>"
$ResourceGroupName="<YourResourceGroupName>"
$ServerName="<YourServerNameWithoutURLSuffixSeeNote>"  # Without database.windows.net
$DatabaseName="<YourDatabaseName>"
$NewDatabaseName="<YourDatabaseName>"

Login-AzureRmAccount
Get-AzureRmSubscription
Select-AzureRmSubscription -SubscriptionName $SubscriptionName

# List the last 10 database restore points
((Get-AzureRMSqlDatabaseRestorePoints -ResourceGroupName $ResourceGroupName -ServerName $ServerName -DatabaseName ($DatabaseName).RestorePointCreationDate)[-10 .. -1]

# Or list all restore points
Get-AzureRmSqlDatabaseRestorePoints -ResourceGroupName $ResourceGroupName -ServerName $ServerName -DatabaseName $DatabaseName

# Get the specific database to restore
$Database = Get-AzureRmSqlDatabase -ResourceGroupName $ResourceGroupName -ServerName $ServerName -DatabaseName $DatabaseName

# Pick desired restore point using RestorePointCreationDate
$PointInTime="<RestorePointCreationDate>"  

# Restore database from a restore point
$RestoredDatabase = Restore-AzureRmSqlDatabase –FromPointInTimeBackup –PointInTime $PointInTime -ResourceGroupName $Database.ResourceGroupName -ServerName $Database.$ServerName -TargetDatabaseName $NewDatabaseName –ResourceId $Database.ResourceID

# Verify the status of restored database
$RestoredDatabase.status

```

>[AZURE.NOTE] Verwenden Sie in den oben aufgeführten PowerShell-Cmdlets „foo“ als „-ServerName“ für den Server „foo.database.windows.net“.

Nachdem die Wiederherstellung abgeschlossen ist, können Sie Ihre wiederhergestellte Datenbank konfigurieren. Befolgen Sie hierzu die Anleitung [Abschließen der wiederhergestellten Azure SQL-Datenbank][].

## Wiederherstellen einer gelöschten Datenbank

Verwenden Sie das Cmdlet [Restore-AzureRmSqlDatabase][], um eine gelöschte Datenbank wiederherzustellen.

1. Öffnen Sie Windows PowerShell.
2. Stellen Sie eine Verbindung mit Ihrem Azure-Konto her, und listen Sie alle Abonnements auf, die Ihrem Konto zugeordnet sind.
3. Wählen Sie das Abonnement aus, das die gelöschte Datenbank enthält, die wiederhergestellt werden soll.
4. Rufen Sie die spezifische gelöschte Datenbank ab.
5. Stellen Sie die gelöschte Datenbank wieder her.
6. Überprüfen Sie, ob die wiederhergestellte Datenbank online ist.

```Powershell

$SubscriptionName="<YourSubscriptionName>"
$ResourceGroupName="<YourResourceGroupName>"
$ServerName="<YourServerNameWithoutURLSuffixSeeNote>"  # Without database.windows.net
$DatabaseName="<YourDatabaseName>"
$NewDatabaseName="<YourDatabaseName>"

Login-AzureRmAccount
Get-AzureRmSubscription
Select-AzureRmSubscription -SubscriptionName $SubscriptionName

# Get the deleted database to restore
$DeletedDatabase = Get-AzureRmSqlDeletedDatabaseBackup -ResourceGroupName $ResourceGroupNam -ServerName $ServerName -DatabaseName $DatabaseName

# Restore deleted database
$RestoredDatabase = Restore-AzureRmSqlDatabase –FromDeletedDatabaseBackup –DeletionDate $DeletedDatabase.DeletionDate -ResourceGroupName $DeletedDatabase.ResourceGroupName -ServerName $DeletedDatabase.ServerName -TargetDatabaseName $NewDatabaseName –ResourceId $DeletedDatabase.ResourceID

# Verify the status of restored database
$RestoredDatabase.status

```

>[AZURE.NOTE] Verwenden Sie in den oben aufgeführten PowerShell-Cmdlets „foo“ als „-ServerName“ für den Server „foo.database.windows.net“.

Nachdem die Wiederherstellung abgeschlossen ist, können Sie Ihre wiederhergestellte Datenbank konfigurieren. Befolgen Sie hierzu die Anleitung [Abschließen der wiederhergestellten Azure SQL-Datenbank][].

## Wiederherstellen von einer geografischen Azure-Region aus

Verwenden Sie das Cmdlet [Restore-AzureRmSqlDatabase][], um eine Datenbank wiederherzustellen.

1. Öffnen Sie Windows PowerShell.
2. Stellen Sie eine Verbindung mit Ihrem Azure-Konto her, und listen Sie alle Abonnements auf, die Ihrem Konto zugeordnet sind.
3. Wählen Sie das Abonnement aus, in dem die wiederherzustellende Datenbank enthalten ist.
4. Rufen Sie die Datenbank ab, die Sie wiederherstellen möchten.
5. Erstellen Sie die Wiederherstellungsanforderung für die Datenbank.
6. Überprüfen Sie den Status der mittels Geowiederherstellung wiederhergestellten Datenbank.

```Powershell

Login-AzureRmAccount
Get-AzureRmSubscription
Select-AzureRmSubscription -SubscriptionName "<Subscription_name>"

# Get the database you want to recover
$GeoBackup = Get-AzureRmSqlDatabaseGeoBackup -ResourceGroupName "<YourResourceGroupName>" -ServerName "<YourServerName>" -DatabaseName "<YourDatabaseName>"

# Recover database
$GeoRestoredDatabase = Restore-AzureRmSqlDatabase –FromGeoBackup -ResourceGroupName "<YourResourceGroupName>" -ServerName "<YourTargetServer>" -TargetDatabaseName "<NewDatabaseName>" –ResourceId $GeoBackup.ResourceID

# Verify that the geo-restored database is online
$GeoRestoredDatabase.status

```

### Konfigurieren der Datenbank nach der Durchführung einer Geowiederherstellung
Dies ist eine Prüfliste, mit der Sie die wiederhergestellte Datenbank für die Produktion vorbereiten können.

1. **Aktualisieren von Verbindungszeichenfolgen**: Stellen Sie sicher, dass die Verbindungszeichenfolgen Ihrer Clienttools auf die neu wiederhergestellte Datenbank verweisen.
2. **Ändern von Firewallregeln**: Überprüfen Sie die Firewallregeln auf dem Zielserver, und stellen Sie sicher, dass die Verbindungen von Ihren Clientcomputern oder Azure zum Server und der neu wiederhergestellten Datenbank aktiviert sind.
3. **Überprüfen von Serveranmeldungen und Datenbankbenutzern**: Überprüfen Sie, ob alle von der Anwendung verwendeten Anmeldungen auf dem Server vorhanden sind, der die wiederhergestellte Datenbank hostet. Erstellen Sie die fehlenden Anmeldungen erneut, und gewähren Sie ihnen entsprechende Berechtigungen auf der wiederhergestellten Datenbank. 
4. **Aktivieren der Überwachung**: Wenn die Überwachung für den Zugriff auf die Datenbank benötigt wird, müssen Sie nach der Wiederherstellung der Datenbank die Überwachung aktivieren.

Für die wiederhergestellte Datenbank ist TDE aktiviert, wenn für die Quelldatenbank TDE aktiviert ist.


## Nächste Schritte
Weitere Informationen finden Sie unter [Azure SQL-Datenbank-Geschäftskontinuität][] und [Verwaltungsübersicht][].

<!--Image references-->

<!--Article references-->
[Azure SQL-Datenbank-Geschäftskontinuität]: sql-database-business-continuity.md
[Abschließen der wiederhergestellten Azure SQL-Datenbank]: sql-database-recovered-finalize.md
[Installieren und Konfigurieren von Azure PowerShell]: powershell-install-configure.md
[Verwaltungsübersicht]: sql-data-warehouse-overview-manage.md

<!--MSDN references-->
[Create database restore request]: https://msdn.microsoft.com/library/azure/dn509571.aspx
[Database operation status]: https://msdn.microsoft.com/library/azure/dn720371.aspx
[Get restorable dropped database]: https://msdn.microsoft.com/library/azure/dn509574.aspx
[List restorable dropped databases]: https://msdn.microsoft.com/library/azure/dn509562.aspx
[Restore-AzureRmSqlDatabase]: https://msdn.microsoft.com/library/mt693390.aspx

<!--Blog references-->
[diesem Blogbeitrag]: https://azure.microsoft.com/blog/azure-limits-quotas-increase-requests/

<!--Other Web references-->
[Azure Portal]: https://portal.azure.com/
[Microsoft-Web Platform Installer]: https://aka.ms/webpi-azps

<!---HONumber=AcomDC_0608_2016-->