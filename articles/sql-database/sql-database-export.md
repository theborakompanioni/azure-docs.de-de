---
title: Exportieren einer Azure SQL-Datenbank in eine BACPAC-Datei | Microsoft-Dokumentation
description: Exportieren einer Azure SQL-Datenbank in eine BACPAC-Datei mithilfe des Azure-Portals
services: sql-database
documentationcenter: 
author: CarlRabeler
manager: jhubbard
editor: 
ms.assetid: 41d63a97-37db-4e40-b652-77c2fd1c09b7
ms.service: sql-database
ms.custom: load & move data
ms.devlang: NA
ms.date: 06/15/2017
ms.author: carlrab
ms.workload: data-management
ms.topic: article
ms.tgt_pltfrm: NA
ms.translationtype: Human Translation
ms.sourcegitcommit: 857267f46f6a2d545fc402ebf3a12f21c62ecd21
ms.openlocfilehash: bb5a4bed556e12cd6295ba0b999f359ceb66c204
ms.contentlocale: de-de
ms.lasthandoff: 06/28/2017


---
# <a name="export-an-azure-sql-database-to-a-bacpac-file"></a>Exportieren einer Azure SQL-Datenbank in eine BACPAC-Datei

Wenn Sie eine Datenbank zur Archivierung oder zum Verschieben auf eine andere Plattform exportieren müssen, können Sie das Datenbankschema und die Daten in eine [BACPAC](https://msdn.microsoft.com/library/ee210546.aspx#Anchor_4)-Datei exportieren. Eine BACPAC-Datei ist eine ZIP-Datei mit der Erweiterung BACPAC. Sie enthält die Metadaten und Daten aus einer SQL Server-Datenbank. Eine BACPAC-Datei kann im Azure Blob Storage oder im lokalen Speicher an einem lokalen Standort gespeichert werden und dann zurück in die Azure SQL-Datenbank oder in eine lokale SQL-Serverinstallation importiert werden. 

> [!IMPORTANT] 
> Der automatisierte Export von Azure SQL-Datenbanken wurde am 1. März 2017 eingestellt. Sie können die [langfristige Sicherungsaufbewahrung](sql-database-long-term-retention.md
) oder [Azure Automation](https://github.com/Microsoft/azure-docs-pr/blob/2461f706f8fc1150e69312098640c0676206a531/articles/automation/automation-intro.md) zum regelmäßigen Archivieren von SQL-Datenbanken mithilfe von PowerShell nach einem Zeitplan Ihrer Wahl verwenden. Als Beispiel Sie das [PowerShell-Beispielskript](https://github.com/Microsoft/sql-server-samples/tree/master/samples/manage/azure-automation-automated-export) aus GitHub herunterladen.
>

## <a name="considerations-when-exporting-an-azure-sql-database"></a>Überlegungen beim Exportieren einer Azure SQL-Datenbank

* Damit ein Export hinsichtlich der Transaktionen konsistent ist, müssen Sie entweder sicherstellen, dass während des Exports keine Schreibaktivitäten erfolgen, oder den Export aus einer [in Hinblick auf Transaktionen konsistenten Kopie](sql-database-copy.md) Ihrer Azure SQL-Datenbank durchführen.
* Beim Exportieren in Blobspeicher beträgt die maximale Größe einer BACPAC-Datei 200 GB. Führen Sie zum Archivieren einer größeren BACPAC-Datei einen Export in lokalen Speicher durch.
* Das Exportieren einer BACPAC-Datei in Azure Storage Premium mit den in diesem Artikel erläuterten Methoden wird nicht unterstützt.
* Falls der Exportvorgang aus Azure SQL-Datenbank länger als 20 Stunden dauert, wird er unter Umständen abgebrochen. Um die Leistung während des Exports zu erhöhen, können Sie Folgendes tun:
  * Erhöhen Sie vorübergehend Ihr Servicelevel.
  * Verhindern Sie jegliche Lese- und Schreibaktivitäten während des Exports.
  * Verwenden Sie einen [gruppierten Index](https://msdn.microsoft.com/library/ms190457.aspx) mit Werten ungleich NULL in allen großen Tabellen. Ohne gruppierte Indizes schlägt ein Export, der länger als sechs bis zwölf Stunden dauert, ggf. fehl. Dies liegt daran, dass der Exportdienst einen Tabellenscan durchführen muss, um die gesamte Tabelle zu exportieren. Eine gute Möglichkeit, um zu ermitteln, ob Ihre Tabellen für den Export optimiert sind, besteht darin, **DBCC SHOW_STATISTICS** auszuführen und sicherzustellen, dass *RANGE_HI_KEY* nicht NULL ist und der Wert eine gute Verteilung aufweist. Weitere Informationen finden Sie unter [DBCC SHOW_STATISTICS](https://msdn.microsoft.com/library/ms174384.aspx).

> [!NOTE]
> BACPAC-Dateien eignen sich nicht für Backup- und Wiederherstellungsvorgänge. Azure SQL-Datenbank erstellt für jede Benutzerdatenbank automatisch Sicherungen. Weitere Informationen finden Sie unter [Übersicht über die Geschäftskontinuität](sql-database-business-continuity.md) und [Sicherungen von SQL-Datenbanken](sql-database-automated-backups.md).  
> 

## <a name="export-to-a-bacpac-file-using-the-azure-portal"></a>Exportieren in eine BACPAC-Datei mithilfe des Azure-Portals

Um eine Datenbank über das [Azure-Portal](https://portal.azure.com) zu exportieren, öffnen Sie die Seite für Ihre Datenbank, und klicken Sie auf der Symbolleiste auf **Exportieren**. Geben Sie den BACPAC-Dateinamen, das Azure Storage-Konto und den Container für den Export sowie die Anmeldeinformationen für die Verbindung mit der Quelldatenbank an.  

![­Datenbankexport](./media/sql-database-export/database-export.png)

Um den Status des Exportvorgangs zu überwachen, öffnen Sie die Seite für den logischen Server mit der zu exportierenden Datenbank. Scrollen Sie nach unten bis zu **Vorgänge**, und klicken Sie dann auf **Import-/Exportverlauf**.

![Exportverlaufs](./media/sql-database-export/export-history.png)
![Exportverlaufstatus](./media/sql-database-export/export-history2.png)

## <a name="export-to-a-bacpac-file-using-the-sqlpackage-utility"></a>Exportieren in eine BACPAC-Datei mit dem Hilfsprogramm „SQLPackage“

Informationen zum Exportieren einer SQL-Datenbank mit dem Befehlszeilenprogramm [SqlPackage](https://msdn.microsoft.com/library/hh550080.aspx) finden Sie unter [Exportparameter und -eigenschaften](https://msdn.microsoft.com/library/hh550080.aspx#Export Parameters and Properties). Das SQLPackage-Hilfsprogramm wird mit den neuesten Versionen von [SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx) und [SQL Server Data Tools für Visual Studio](https://msdn.microsoft.com/library/mt204009.aspx) ausgeliefert. Alternativ dazu können Sie die neueste Version von [SqlPackage](https://www.microsoft.com/download/details.aspx?id=53876) direkt aus dem Microsoft Download Center herunterladen.

Die Verwendung des SQLPackage-Hilfsprogramms wird aus Gründen der Skalierbarkeit und Leistung für die meisten Produktionsumgebungen empfohlen. Einen Blogbeitrag des SQL Server-Kundenberatungsteams zur Migration mithilfe von BACPAC-Dateien finden Sie unter [Migrating from SQL Server to Azure SQL Database using BACPAC Files](https://blogs.msdn.microsoft.com/sqlcat/2016/10/20/migrating-from-sql-server-to-azure-sql-database-using-bacpac-files/) (Migrieren von SQL Server zu Azure SQL-Datenbank mithilfe von BACPAC-Dateien).

In diesem Beispiel wird gezeigt, wie eine Datenbank mithilfe von „SqlPackage.exe“ mit universeller Active Directory-Authentifizierung exportiert wird:

```cmd
SqlPackage.exe /a:Export /tf:testExport.bacpac /scs:"Data Source=apptestserver.database.windows.net;Initial Catalog=MyDB;" /ua:True /tid:"apptest.onmicrosoft.com"
```

## <a name="export-to-a-bacpac-file-using-sql-server-management-studio-ssms"></a>Exportieren in eine BACPAC-Datei mit SQL Server Management Studio (SSMS)

Die neuesten Versionen von SQL Server Management Studio enthalten auch einen Assistenten zum Exportieren einer Azure SQL-Datenbank in eine BACPAC-Datei. Weitere Informationen finden Sie unter [Exportieren einer Datenebenenanwendung](https://docs.microsoft.com/sql/relational-databases/data-tier-applications/export-a-data-tier-application).

## <a name="export-to-a-bacpac-file-using-powershell"></a>Exportieren in eine BACPAC-Datei mit PowerShell

Verwenden Sie das [New-AzureRmSqlDatabaseImport](/powershell/module/azurerm.sql/new-azurermsqldatabaseexport)-Cmdlet, um eine Anforderung für einen Datenbankexport beim Azure SQL-Datenbankdienst einzureichen. Je nach Größe Ihrer Datenbank kann es einige Zeit dauern, bis der Exportvorgang abgeschlossen ist.

 ```powershell
 $exportRequest = New-AzureRmSqlDatabaseExport -ResourceGroupName $ResourceGroupName -ServerName $ServerName `
   -DatabaseName $DatabaseName -StorageKeytype $StorageKeytype -StorageKey $StorageKey -StorageUri $BacpacUri `
   -AdministratorLogin $creds.UserName -AdministratorLoginPassword $creds.Password
 ```

Zum Überprüfen des Status der Exportanforderung verwenden Sie das [Get-AzureRmSqlDatabaseImportExportStatus](/powershell/module/azurerm.sql/get-azurermsqldatabaseimportexportstatus)-Cmdlet. Wenn Sie dieses Cmdlet direkt nach der Anforderung ausführen, wird in der Regel **Status: InProgress** zurückgegeben. Wenn **Status: Succeeded** angezeigt wird, ist der Export abgeschlossen.

```powershell
$importStatus = Get-AzureRmSqlDatabaseImportExportStatus -OperationStatusLink $importRequest.OperationStatusLink
[Console]::Write("Exporting")
while ($importStatus.Status -eq "InProgress")
{
    $importStatus = Get-AzureRmSqlDatabaseImportExportStatus -OperationStatusLink $importRequest.OperationStatusLink
    [Console]::Write(".")
    Start-Sleep -s 10
}
[Console]::WriteLine("")
$importStatus
```

## <a name="next-steps"></a>Nächste Schritte

* Informationen zur langfristigen Archivierung von Azure SQL-Datenbanksicherungen als Alternative zum Exportieren einer Datenbank für Archivzwecke finden Sie unter [Langfristige Archivierung von Sicherungen](sql-database-long-term-retention.md).
- Einen Blogbeitrag des SQL Server-Kundenberatungsteams zur Migration mithilfe von BACPAC-Dateien finden Sie unter [Migrating from SQL Server to Azure SQL Database using BACPAC Files](https://blogs.msdn.microsoft.com/sqlcat/2016/10/20/migrating-from-sql-server-to-azure-sql-database-using-bacpac-files/) (Migrieren von SQL Server zu Azure SQL-Datenbank mithilfe von BACPAC-Dateien).
* Informationen zum Importieren einer BACPAC-Datei in eine SQL Server-Datenbank finden Sie unter [Importieren einer BACPAC-Datei in eine SQL Server-Datenbank](https://msdn.microsoft.com/library/hh710052.aspx).
* Informationen zum Exportieren einer BACPAC-Datei aus einer SQL Server-Datenbank finden Sie unter [Exportieren einer Datenebenenanwendung](https://docs.microsoft.com/sql/relational-databases/data-tier-applications/export-a-data-tier-application) und [Migrieren Ihrer ersten Datenbank](sql-database-migrate-your-sql-server-database.md).
* Wenn der Export aus SQL Server als Vorbereitung auf die Migration zu Azure SQL-Datenbank erfolgt, finden Sie unter [Migrieren einer SQL Server-Datenbank zu Azure SQL-Datenbank](sql-database-cloud-migrate.md) nützliche Hinweise.

