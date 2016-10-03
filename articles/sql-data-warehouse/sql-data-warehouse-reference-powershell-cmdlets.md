<properties
   pageTitle="PowerShell-Cmdlets für Azure SQL Data Warehouse"
   description="Hier finden Sie die wichtigsten PowerShell-Cmdlets für Azure SQL Data Warehouse, einschließlich der Cmdlets zum Anhalten und Fortsetzen einer Datenbank."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="sonyam"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="08/16/2016"
   ms.author="sonyama;barbkess;mausher"/>

# PowerShell-Cmdlets und REST-APIs für SQL Data Warehouse

Viele SQL Data Warehouse-Verwaltungsaufgaben können entweder mithilfe von Azure PowerShell-Cmdlets oder REST-APIs verwaltet werden. Die folgenden Beispiele zeigen einen Einblick, wie PowerShell-Befehle verwendet werden, um häufige Aufgaben in SQL Data Warehouse zu automatisieren. Einige gute REST-Beispiele finden Sie im Artikel [Verwalten von Computeleistung in Azure SQL Data Warehouse (REST)][].

> [AZURE.NOTE]  Damit Sie Azure Powershell mit SQL Data Warehouse verwenden können, benötigen Sie Azure Powershell Version 1.0.3 oder höher. Sie können die Version überprüfen, indem Sie **Get-Module -ListAvailable -Name Azure** ausführen. Sie können die neueste Version installieren, indem Sie sie über [Microsoft Web Platform Installer][] herunterladen. Weitere Informationen zum Installieren der neuesten Version finden Sie unter [Installieren und Konfigurieren von Azure PowerShell][].

## Erste Schritte mit Azure PowerShell-Cmdlets

1. Öffnen Sie Windows PowerShell.
2. Zum Anmelden bei Azure Resource Manager und Auswählen Ihres Abonnements führen Sie bei der PowerShell-Eingabeaufforderung die folgenden Befehle aus:

    ```PowerShell
    Login-AzureRmAccount
    Get-AzureRmSubscription
    Select-AzureRmSubscription -SubscriptionName "MySubscription"
    ```

## Beispiel für das Anhalten von Azure SQL Data Warehouse

Halten Sie eine Datenbank mit dem Namen „Database02“ an, die auf einem Server mit dem Namen „Server01“ gehostet wird. Der Server befindet sich in einer Azure-Ressourcengruppe namens „ResourceGroup1“.

```Powershell
Suspend-AzureRmSqlDatabase –ResourceGroupName "ResourceGroup1" –ServerName "Server01" –DatabaseName "Database02"
```
Variation: Das Beispiel leitet das abgerufene Objekt an [Suspend-AzureRmSqlDatabase][] weiter. Dadurch wird die Datenbank angehalten. Der letzte Befehl zeigt die Ergebnisse an.

```Powershell
$database = Get-AzureRmSqlDatabase –ResourceGroupName "ResourceGroup1" –ServerName "Server01" –DatabaseName "Database02"
$resultDatabase = $database | Suspend-AzureRmSqlDatabase
$resultDatabase
```

## Beispiel für das Starten von Azure SQL Data Warehouse

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

## Häufig verwendete PowerShell-Cmdlets

Diese PowerShell-Cmdlets werden häufig mit Azure SQL Data Warehouse verwendet.

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

## Nächste Schritte
Weitere PowerShell-Beispiele finden Sie unter:

- [Erstellen von SQL Data Warehouse mithilfe von PowerShell][]
- [Datenbankwiederherstellung][]

Eine Liste mit allen Aufgaben, die mit PowerShell automatisiert werden können, finden Sie unter [Azure SQL Database Cmdlets][] \(Azure SQL-Datenbank-Cmdlets). Eine Liste mit Aufgaben, die mit REST automatisiert werden können, finden Sie unter [Vorgänge für Azure SQL-Datenbanken][].

<!--Image references-->

<!--Article references-->
[Installieren und Konfigurieren von Azure PowerShell]: ./powershell-install-configure.md
[Erstellen von SQL Data Warehouse mithilfe von PowerShell]: ./sql-data-warehouse-get-started-provision-powershell.md
[Datenbankwiederherstellung]: ./sql-data-warehouse-restore-database-powershell.md
[Verwalten von Computeleistung in Azure SQL Data Warehouse (REST)]: ./sql-data-warehouse-manage-compute-rest-api.md

<!--MSDN references-->
[Azure SQL Database Cmdlets]: https://msdn.microsoft.com/library/mt574084.aspx
[Vorgänge für Azure SQL-Datenbanken]: https://msdn.microsoft.com/library/azure/dn505719.aspx
[Get-AzureRmSqlDatabase]: https://msdn.microsoft.com/library/mt603648.aspx
[Get-AzureRmSqlDeletedDatabaseBackup]: https://msdn.microsoft.com/library/mt693387.aspx
[Get-AzureRmSqlDatabaseRestorePoints]: https://msdn.microsoft.com/library/mt603642.aspx
[New-AzureRmSqlDatabase]: https://msdn.microsoft.com/library/mt619339.aspx
[Remove-AzureRmSqlDatabase]: https://msdn.microsoft.com/library/mt619368.aspx
[Restore-AzureRmSqlDatabase]: https://msdn.microsoft.com/library/mt693390.aspx
[Resume-AzureRmSqlDatabase]: https://msdn.microsoft.com/library/mt619347.aspx
<!-- It appears that Select-AzureRmSubscription isn't documented, so this points to Select-AzureSubscription -->
[Select-AzureRmSubscription]: https://msdn.microsoft.com/library/dn722499.aspx
[Set-AzureRmSqlDatabase]: https://msdn.microsoft.com/library/mt619433.aspx
[Suspend-AzureRmSqlDatabase]: https://msdn.microsoft.com/library/mt619337.aspx

<!--Other Web references-->
[Microsoft Web Platform Installer]: https://aka.ms/webpi-azps

<!---HONumber=AcomDC_0817_2016-->