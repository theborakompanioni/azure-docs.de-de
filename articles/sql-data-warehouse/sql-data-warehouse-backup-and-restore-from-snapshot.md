<properties
   pageTitle="Wiederherstellen einer Datenbank nach einem Benutzerfehler in SQL Data Warehouse | Microsoft Azure"
   description="Schritte zum Wiederherstellen einer Datenbank nach einem Benutzerfehler in SQL Data Warehouse"
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="sahaj08"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="03/28/2016"
   ms.author="sahajs;barbkess;sonyama"/>

# Wiederherstellen einer Datenbank nach einem Benutzerfehler in SQL Data Warehouse

SQL Data Warehouse verfügt über zwei Hauptfunktionen für die Wiederherstellung nach einem Benutzerfehler, bei dem versehentlich Daten beschädigt oder gelöscht werden:

- Wiederherstellen einer Livedatenbank
- Wiederherstellen einer gelöschten Datenbank

Bei beiden Funktionen wird eine neue Datenbank auf demselben Server wiederhergestellt. Es muss sichergestellt werden, dass der Server, auf dem die Wiederherstellung erfolgt, über ausreichend DTU-Kapazität für die neue Datenbank verfügt. [Wenden Sie sich an den Support][], um dieses Kontingent zu erhöhen.


## Wiederherstellen einer Livedatenbank
Der Azure SQL Data Warehouse-Dienst schützt alle Livedatenbanken durch Datenbankmomentaufnahmen mindestens alle acht Stunden. Diese werden sieben Tage lang aufbewahrt, um für Sie einen diskreten Satz von Wiederherstellungspunkten bereitzustellen. Datenbankmomentaufnahmen werden auch erstellt, wenn Sie Ihre Datenbank anhalten oder löschen und werden sieben Tage lang aufbewahrt. Wenn es durch einen Benutzerfehler zu einer unbeabsichtigten Änderung von Daten kommt, können Sie für die Datenbank alle Wiederherstellungspunkte innerhalb der Aufbewahrungsdauer wiederherstellen.


### Azure-Portal

Führen Sie die folgenden Schritte zum Wiederherstellen über das Azure-Portal aus.

1. Melden Sie sich beim [Azure-Portal][] an.
2. Wählen Sie auf der linken Bildschirmseite **DURCHSUCHEN** und dann **SQL-Datenbanken** aus.
3. Navigieren Sie zu Ihrer Datenbank, und wählen Sie sie aus.
4. Klicken Sie oben im Blatt für die Datenbank auf **Wiederherstellen**.
5. Geben Sie einen neuen **Datenbanknamen** an, wählen Sie einen **Wiederherstellungspunkt**, und klicken Sie anschließend auf **Erstellen**.
6. Der Datenbank-Wiederherstellungsvorgang beginnt und kann mithilfe von **BENACHRICHTIGUNGEN** überwacht werden.


### PowerShell

Verwenden Sie Azure PowerShell, um eine Datenbankwiederherstellung programmgesteuert auszuführen. Führen Sie zum Herunterladen des Azure PowerShell-Moduls den [Microsoft-Webplattform-Installer](http://go.microsoft.com/fwlink/p/?linkid=320376&clcid=0x409) aus. Sie können die Version überprüfen, indem Sie „Get-Module -ListAvailable -Name AzureRM.Sql“ ausführen. Dieser Artikel basiert auf Microsoft AzureRM.Sql PowerShell Version 1.0.5.

Verwenden Sie das Cmdlet [Restore-AzureRmSqlDatabase][], um eine Datenbank wiederherzustellen.

1. Öffnen Sie Windows PowerShell.
2. Stellen Sie eine Verbindung mit Ihrem Azure-Konto her, und listen Sie alle Abonnements auf, die Ihrem Konto zugeordnet sind.
3. Wählen Sie das Abonnement aus, in dem die wiederherzustellende Datenbank enthalten ist.
4. Listen Sie die Wiederherstellungspunkte für die Datenbank auf.
5. Wählen Sie den gewünschten Wiederherstellungspunkt mit RestorePointCreationDate aus.
6. Stellen Sie die Datenbank mit dem gewünschten Wiederherstellungspunkt wieder her.
7. Überprüfen Sie, ob die wiederhergestellte Datenbank online ist.

```Powershell

Login-AzureRmAccount
Get-AzureRmSubscription
Select-AzureRmSubscription -SubscriptionName "<Subscription_name>"

# List the last 10 database restore points
((Get-AzureRMSqlDatabaseRestorePoints -ResourceGroupName "<YourResourceGroupName>" -ServerName "<YourServerName>" -DatabaseName "<YourDatabaseName>").RestorePointCreationDate)[-10 .. -1]

# Or list all restore points
Get-AzureRmSqlDatabaseRestorePoints -ResourceGroupName "<YourResourceGroupName>" -ServerName "<YourServerName>" -DatabaseName "<YourDatabaseName>" 

# Pick desired restore point using RestorePointCreationDate
$PointInTime = "<RestorePointCreationDate>"

# Get the specific database to restore
$Database = Get-AzureRmSqlDatabase -ResourceGroupName "<YourResourceGroupName>" -ServerName "<YourServerName>" -DatabaseName "<YourDatabaseName>"

# Restore database from a restore point
$RestoredDatabase = Restore-AzureRmSqlDatabase –FromPointInTimeBackup –PointInTime $PointInTime -ResourceGroupName $Database.ResourceGroupName -ServerName $Database.ServerName -TargetDatabaseName "<NewDatabaseName>" –ResourceId $Database.ResourceID

# Verify the status of restored database
$RestoredDatabase.status

```

>[AZURE.NOTE] Verwenden Sie in den oben aufgeführten PowerShell-Cmdlets „foo“ als „-ServerName“ für den Server „foo.database.windows.net“.


### REST-API
Verwenden Sie REST für eine programmatische Durchführung der Datenbankwiederherstellung.

1. Rufen Sie die Liste mit den Wiederherstellungspunkten für die Datenbank ab, indem Sie den Get Database Restore Points-Vorgang verwenden.
2. Beginnen Sie die Wiederherstellung mit dem Vorgang [Datenbankwiederherstellungsanforderung erstellen][].
3. Verfolgen Sie den Status der Wiederherstellung mithilfe des Vorgangs [Datenbank-Betriebsstatus][] nach.


>[AZURE.NOTE] Nachdem die Wiederherstellung abgeschlossen ist, können Sie Ihre wiederhergestellte Datenbank konfigurieren. Befolgen Sie hierzu die Anleitung [Abschließen der wiederhergestellten Azure SQL-Datenbank][].


## Wiederherstellen einer gelöschten Datenbank
Azure SQL Data Warehouse erstellt eine Datenbankmomentaufnahme, bevor eine Datenbank gelöscht wird, und bewahrt diese sieben Tage lang auf. Wenn eine Datenbank aus Versehen gelöscht wurde, können Sie den Zustand der gelöschten Datenbank zum Löschzeitpunkt wiederherstellen.


### Azure-Portal

Führen Sie die folgenden Schritte aus, um eine gelöschte Datenbank über das Azure-Portal wiederherzustellen.

1. Melden Sie sich beim [Azure-Portal][] an.
2. Wählen Sie auf der linken Bildschirmseite **DURCHSUCHEN** und dann **SQL Server** aus.
3. Navigieren Sie zu Ihrem Server, und wählen Sie ihn aus.
4. Scrollen Sie auf dem Blatt für Ihren Server nach unten zu „Vorgänge“, und klicken Sie auf die Kachel **Gelöschte Datenbanken**.
5. Klicken Sie auf die Datenbank, die Sie wiederherstellen möchten.
5. Geben Sie einen neuen **Datenbanknamen** an, und klicken Sie auf **Erstellen**.
6. Der Datenbank-Wiederherstellungsvorgang beginnt und kann mithilfe von **BENACHRICHTIGUNGEN** überwacht werden.


### PowerShell
Verwenden Sie Azure PowerShell für eine programmgesteuerte Wiederherstellung einer gelöschten Datenbank. Führen Sie zum Herunterladen des Azure PowerShell-Moduls den [Microsoft-Webplattform-Installer](http://go.microsoft.com/fwlink/p/?linkid=320376&clcid=0x409) aus. Sie können die Version überprüfen, indem Sie „Get-Module -ListAvailable -Name AzureRM.Sql“ ausführen. Dieser Artikel basiert auf Microsoft AzureRM.Sql PowerShell Version 1.0.5.

Verwenden Sie das Cmdlet [Restore-AzureRmSqlDatabase][], um eine gelöschte Datenbank wiederherzustellen.

1. Öffnen Sie Windows PowerShell.
2. Stellen Sie eine Verbindung mit Ihrem Azure-Konto her, und listen Sie alle Abonnements auf, die Ihrem Konto zugeordnet sind.
3. Wählen Sie das Abonnement aus, das die gelöschte Datenbank enthält, die wiederhergestellt werden soll.
4. Rufen Sie die spezifische gelöschte Datenbank ab.
5. Stellen Sie die gelöschte Datenbank wieder her.
6. Überprüfen Sie, ob die wiederhergestellte Datenbank online ist.

```Powershell

Login-AzureRmAccount
Get-AzureRmSubscription
Select-AzureRmSubscription -SubscriptionName "<Subscription_name>"

# Get the deleted database to restore
$DeletedDatabase = Get-AzureRmSqlDeletedDatabaseBackup -ResourceGroupName "<YourResourceGroupName>" -ServerName "<YourServerName>" -DatabaseName "<YourDatabaseName>"

# Restore deleted database
$RestoredDatabase = Restore-AzureRmSqlDatabase –FromDeletedDatabaseBackup –DeletionDate $DeletedDatabase.DeletionDate -ResourceGroupName $DeletedDatabase.ResourceGroupName -ServerName $DeletedDatabase.ServerName -TargetDatabaseName "<NewDatabaseName>" –ResourceId $DeletedDatabase.ResourceID

# Verify the status of restored database
$RestoredDatabase.status

```

>[AZURE.NOTE] Verwenden Sie in den oben aufgeführten PowerShell-Cmdlets „foo“ als „-ServerName“ für den Server „foo.database.windows.net“.


### REST-API
Verwenden Sie REST für eine programmatische Durchführung der Datenbankwiederherstellung.

1.	Listen Sie alle gelöschten wiederherstellbaren Datenbanken mithilfe des Vorgangs [Wiederherstellbare gelöschte Datenbanken auflisten][] auf.
2.	Rufen Sie die Details der gelöschten Datenbank, die wiederhergestellt werden soll, mithilfe des Vorgangs [Wiederherstellbare gelöschte Datenbank abrufen][] ab.
3.	Beginnen Sie die Wiederherstellung mit dem Vorgang [Datenbankwiederherstellungsanforderung erstellen][].
4.	Verfolgen Sie den Status der Wiederherstellung mithilfe des Vorgangs [Datenbank-Betriebsstatus][] nach.


>[AZURE.NOTE] Nachdem die Wiederherstellung abgeschlossen ist, können Sie Ihre wiederhergestellte Datenbank konfigurieren. Befolgen Sie hierzu die Anleitung [Abschließen der wiederhergestellten Azure SQL-Datenbank][].


## Nächste Schritte
Informationen zu den Geschäftskontinuitätsfunktionen von Azure SQL-Datenbank-Editionen finden Sie in der [Azure SQL-Datenbank-Übersicht zur Geschäftskontinuität][].


<!--Image references-->

<!--Article references-->
[Azure SQL-Datenbank-Übersicht zur Geschäftskontinuität]: sql-database/sql-database-business-continuity.md
[Abschließen der wiederhergestellten Azure SQL-Datenbank]: sql-database/sql-database-recovered-finalize.md

<!--MSDN references-->
[Datenbankwiederherstellungsanforderung erstellen]: http://msdn.microsoft.com/library/azure/dn509571.aspx
[Datenbank-Betriebsstatus]: http://msdn.microsoft.com/library/azure/dn720371.aspx
[Wiederherstellbare gelöschte Datenbank abrufen]: http://msdn.microsoft.com/library/azure/dn509574.aspx
[Wiederherstellbare gelöschte Datenbanken auflisten]: http://msdn.microsoft.com/library/azure/dn509562.aspx
[Restore-AzureRmSqlDatabase]: https://msdn.microsoft.com/library/mt693390.aspx

<!--Other Web references-->
[Azure-Portal]: https://portal.azure.com/
[Wenden Sie sich an den Support]: https://azure.microsoft.com/blog/azure-limits-quotas-increase-requests/

<!---HONumber=AcomDC_0406_2016-->