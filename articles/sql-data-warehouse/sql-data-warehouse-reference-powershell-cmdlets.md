<properties
   pageTitle="PowerShell-Cmdlets für Azure SQL Data Warehouse"
   description="Hier finden Sie die wichtigsten PowerShell-Cmdlets für Azure SQL Data Warehouse, einschließlich der Cmdlets zum Anhalten und Fortsetzen einer Datenbank."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="barbkess"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="04/02/2016"
   ms.author="barbkess;mausher;sonyama"/>

# PowerShell-Cmdlets und REST-APIs für SQL Data Warehouse

Für die Verwaltung von SQL Data Warehouse können entweder Azure PowerShell-Cmdlets oder REST-APIs verwendet werden.

Die meisten für **Azure SQL-Datenbank** definierten Befehle werden auch für **SQL Data Warehouse** verwendet. Eine aktuelle Liste finden Sie unter [Azure SQL-Cmdlets](https://msdn.microsoft.com/library/mt574084.aspx). Die Cmdlets [Suspend-AzureRmSqlDatabase][] und [Resume-AzureRmSqlDatabase][] wurden speziell für SQL Data Warehouse hinzugefügt.

Analog dazu können die REST-APIs für **SQL Azure-Datenbank** auch für Instanzen von **SQL Data Warehouse** verwendet werden. Eine aktuelle Liste finden Sie unter [Vorgänge für Azure SQL-Datenbanken](https://msdn.microsoft.com/library/azure/dn505719.aspx).

## Erste Schritte mit Azure PowerShell-Cmdlets

1. Führen Sie zum Herunterladen des Azure PowerShell-Moduls den [Microsoft-Webplattform-Installer](http://aka.ms/webpi-azps) aus. Weitere Informationen zu diesem Installationsprogramm finden Sie unter [Installieren und Konfigurieren von Azure PowerShell][].
2. Um PowerShell zu starten, klicken Sie auf **Start** und geben Sie **Windows PowerShell** ein.
3. Zum Anmelden bei Azure Resource Manager und Auswählen Ihres Abonnements führen Sie bei der PowerShell-Eingabeaufforderung die folgenden Befehle aus:

    ```PowerShell
    Login-AzureRmAccount
    Get-AzureRmSubscription
    Select-AzureRmSubscription -SubscriptionName "MySubscription"
    ```


## Häufig verwendete PowerShell-Cmdlets

Diese PowerShell-Cmdlets werden häufig mit Azure SQL Data Warehouse verwendet:


- [Get-AzureRmSqlDatabase][]
- [Get-AzureRmSqlDeletedDatabaseBackup][]
- [Get-AzureRmSqlDatabaseRestorePoints][]
- [New-AzureRmSqlDatabase][]
- [Remove-AzureRmSqlDatabase][]
- [Restore-AzureRmSqlDatabase][] 
- [Resume-AzureRmSqlDatabase][]
- [Select-AzureRmSubscription][]
- [Set-AzureRmSqlDatabase][]
- [Suspend-AzureRmSqlDatabase][]


## Beispiele für SQL Data Warehouse

Diese Beispiele beziehen sich auf Funktionen, die nur für SQL Data Warehouse gelten.

### [Suspend-AzureRmSqlDatabase][]

Halten Sie eine Datenbank mit dem Namen „Database02“ an, die auf einem Server mit dem Namen „Server01“ gehostet wird. Der Server befindet sich in einer Azure-Ressourcengruppe namens „ResourceGroup1“.

```Powershell
Suspend-AzureRmSqlDatabase –ResourceGroupName "ResourceGroup1" –ServerName "Server01" –DatabaseName "Database02"
```
Abweichung: Das abgerufene Objekt wird an [Suspend-AzureRmSqlDatabase][] weitergeleitet. Dadurch wird die Datenbank angehalten. Der letzte Befehl zeigt die Ergebnisse an.

```Powershell
$database = Get-AzureRmSqlDatabase –ResourceGroupName "ResourceGroup1" –ServerName "Server01" –DatabaseName "Database02"
$resultDatabase = $database | Suspend-AzureRmSqlDatabase
$resultDatabase
```

### [Resume-AzureRmSqlDatabase][]

Setzen Sie den Betrieb der auf dem Server „Server01“ gehosteten Datenbank „Database02“ fort. Der Server befindet sich in einer Ressourcengruppe namens „ResourceGroup1“.

```Powershell
Resume-AzureRmSqlDatabase –ResourceGroupName "ResourceGroup1" –ServerName "Server01" -DatabaseName "Database02"
```

Abweichung: Eine Datenbank „Database02“ von einem Server „Server01“, der sich in einer Ressourcengruppe „ResourceGroup1“ befindet, wird abgerufen. Das abgerufene Objekt wird an [Resume-AzureRmSqlDatabase][] weitergeleitet.

```Powershell
$database = Get-AzureRmSqlDatabase –ResourceGroupName "ResourceGroup1" –ServerName "Server01" –DatabaseName "Database02"
$resultDatabase = $database | Resume-AzureRmSqlDatabase
```

> [AZURE.NOTE] Hinweis: Wenn Ihr Server „foo.database.windows.net“ heißt, verwenden Sie in den PowerShell-Cmdlets für „-ServerName“ den Namen „foo“.


## Nächste Schritte
Weitere Referenzinformationen finden Sie unter [SQL Data Warehouse-Referenz – Übersicht][]. Weitere PowerShell-Beispiele finden Sie unter:
- [Erstellen von SQL Data Warehouse mithilfe von PowerShell](sql-data-warehouse-get-started-provision-powershell.md)
- [Wiederherstellung von einer Momentaufnahme](sql-data-warehouse-backup-and-restore-from-snapshot.md)
- [Geowiederherstellung von einer Momentaufnahme](sql-data-warehouse-backup-and-restore-from-geo-restore-snapshot.md)

<!--Image references-->

<!--Article references-->
[SQL Data Warehouse-Referenz – Übersicht]: sql-data-warehouse-overview-reference.md
[Installieren und Konfigurieren von Azure PowerShell]: ../articles/powershell-install-configure.md

<!--MSDN references-->
[Get-AzureRmSqlDatabase]: https://msdn.microsoft.com/library/mt603648.aspx
[Get-AzureRmSqlDeletedDatabaseBackup]: https://msdn.microsoft.com/library/mt693387.aspx
[Get-AzureRmSqlDatabaseRestorePoints]: https://msdn.microsoft.com/library/mt603642.aspx
[New-AzureRmSqlDatabase]: https://msdn.microsoft.com/library/mt619339.aspx
[Remove-AzureRmSqlDatabase]: https://msdn.microsoft.com/library/mt619368.aspx
[Restore-AzureRmSqlDatabase]: https://msdn.microsoft.com/library/mt693390.aspx
[Resume-AzureRmSqlDatabase]: http://msdn.microsoft.com/library/mt619347.aspx
<!-- It appears that Select-AzureRmSubscription isn't documented, so this points to Select-AzureRmSubscription -->
[Select-AzureRmSubscription]: https://msdn.microsoft.com/library/dn722499.aspx
[Set-AzureRmSqlDatabase]: https://msdn.microsoft.com/library/mt619433.aspx
[Suspend-AzureRmSqlDatabase]: http://msdn.microsoft.com/library/mt619337.aspx



<!--Other Web references-->

<!---HONumber=AcomDC_0406_2016-->