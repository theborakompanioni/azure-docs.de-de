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
ms.custom: move data
ms.devlang: NA
ms.date: 04/05/2017
ms.author: carlrab
ms.workload: data-management
ms.topic: article
ms.tgt_pltfrm: NA
translationtype: Human Translation
ms.sourcegitcommit: 757d6f778774e4439f2c290ef78cbffd2c5cf35e
ms.openlocfilehash: eadd300dcda2f160589c5e8e4fb7508445ef9944
ms.lasthandoff: 04/10/2017


---
# <a name="export-an-azure-sql-database-to-a-bacpac-file"></a>Exportieren einer Azure SQL-Datenbank in eine BACPAC-Datei

In diesem Artikel wird das Exportieren einer Azure SQL-Datenbank in eine [BACPAC](https://msdn.microsoft.com/library/ee210546.aspx#Anchor_4)-Datei erläutert. In diesem Artikel werden die folgenden Methoden verwendet:
- Das [Azure-Portal](https://portal.azure.com)
- Das Befehlszeilenprogramm [SqlPackage](https://msdn.microsoft.com/library/hh550080.aspx)
- Das [New-AzureRmSqlDatabaseExport](https://docs.microsoft.com/powershell/resourcemanager/azurerm.sql/v2.7.0/new-azurermsqldatabaseexport)-Cmdlet
- Der Assistent [Datenebenenanwendung exportieren](https://docs.microsoft.com/sql/relational-databases/data-tier-applications/export-a-data-tier-application) in [SQL Server Management Studio](https://msdn.microsoft.com/library/ms174173.aspx).

> [!IMPORTANT] 
> Der automatisierte Export von Azure SQL-Datenbanken ist jetzt als Vorschau verfügbar und wird am 1. März 2017 eingestellt. Seit dem 1. Dezember 2016 ist das Konfigurieren des automatisierten Exports für SQL-Datenbanken nicht mehr möglich. Alle vorhandenen Aufträge für automatisierten Export werden bis zum 1. März 2017 weiterhin ausgeführt. Seit dem 1. Dezember 2016 können Sie die [langfristige Sicherungsaufbewahrung](sql-database-long-term-retention.md
) oder [Azure Automation](https://github.com/Microsoft/azure-docs-pr/blob/2461f706f8fc1150e69312098640c0676206a531/articles/automation/automation-intro.md) zum regelmäßigen Archivieren von SQL-Datenbanken mithilfe von PowerShell nach einem Zeitplan Ihrer Wahl verwenden. Als Beispielskript können Sie das [PowerShell-Beispielskript](https://github.com/Microsoft/sql-server-samples/tree/master/samples/manage/azure-automation-automated-export) aus GitHub herunterladen.
>

> [!IMPORTANT] 
> Der automatisierte Export von Azure SQL-Datenbanken ist jetzt als Vorschau verfügbar und wird am 1. März 2017 eingestellt. Seit dem 1. Dezember 2016 ist das Konfigurieren des automatisierten Exports für SQL-Datenbanken nicht mehr möglich. Alle vorhandenen Aufträge für automatisierten Export werden bis zum 1. März 2017 weiterhin ausgeführt. Seit dem 1. Dezember 2016 können Sie die [langfristige Sicherungsaufbewahrung](sql-database-long-term-retention.md
) oder [Azure Automation](https://github.com/Microsoft/azure-docs-pr/blob/2461f706f8fc1150e69312098640c0676206a531/articles/automation/automation-intro.md) zum regelmäßigen Archivieren von SQL-Datenbanken mithilfe von PowerShell nach einem Zeitplan Ihrer Wahl verwenden. Als Beispielskript können Sie das [PowerShell-Beispielskript](https://github.com/Microsoft/sql-server-samples/tree/master/samples/manage/azure-automation-automated-export) aus GitHub herunterladen.
>

## <a name="overview"></a>Übersicht

Wenn Sie eine Datenbank zur Archivierung oder zum Verschieben auf eine andere Plattform exportieren müssen, können Sie das Datenbankschema und die Daten in eine BACPAC-Datei exportieren. Eine BACPAC-Datei ist eine ZIP-Datei mit der Erweiterung BACPAC. Sie enthält die Metadaten und Daten aus einer SQL Server-Datenbank. Eine BACPAC-Datei kann im Azure Blob Storage oder im lokalen Speicher an einem lokalen Standort gespeichert werden und dann zurück in die Azure SQL-Datenbank oder in eine lokale SQL-Serverinstallation importiert werden. 

> [!IMPORTANT]
> Wenn der Export aus SQL Server als Vorbereitung auf die Migration zu Azure SQL-Datenbank erfolgt, finden Sie unter [Migrieren einer SQL Server-Datenbank zu Azure SQL-Datenbank](sql-database-cloud-migrate.md) nützliche Hinweise.
> 

## <a name="considerations"></a>Überlegungen

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

## <a name="azure-portal"></a>Azure-Portal

Um eine Datenbank über das Azure-Portal zu exportieren, öffnen Sie die Seite für Ihre Datenbank, und klicken Sie auf der Symbolleiste auf **Exportieren**. Geben Sie den Dateinamen im Format „*.bacpac“, das Azure Storage-Konto und den Container für den Export sowie die Anmeldeinformationen für die Verbindung mit der Quelldatenbank an.  

   ![­Datenbankexport](./media/sql-database-export/database-export.png)

Um den Status des Exportvorgangs zu überwachen, öffnen Sie die Seite für den logischen Server mit der zu exportierenden Datenbank. Scrollen Sie nach unten bis zu **Vorgänge**, und klicken Sie dann auf **Import-/Exportverlauf**.

## <a name="sqlpackage-utility"></a>SqlPackage-Hilfsprogramm

Informationen zum Exportieren einer SQL-Datenbank mit dem Befehlszeilenprogramm [SqlPackage](https://msdn.microsoft.com/library/hh550080.aspx) finden Sie unter [Exportparameter und -eigenschaften](https://msdn.microsoft.com/library/hh550080.aspx#Export Parameters and Properties). Das SQLPackage-Hilfsprogramm wird mit den neuesten Versionen von [SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx) und [SQL Server Data Tools für Visual Studio](https://msdn.microsoft.com/library/mt204009.aspx) ausgeliefert. Alternativ dazu können Sie die neueste Version von [SqlPackage](https://www.microsoft.com/download/details.aspx?id=53876) direkt aus dem Microsoft Download Center herunterladen.

Die Verwendung des SQLPackage-Hilfsprogramms wird aus Gründen der Skalierbarkeit und Leistung für die meisten Produktionsumgebungen empfohlen. Einen Blogbeitrag des SQL Server-Kundenberatungsteams zur Migration mithilfe von BACPAC-Dateien finden Sie unter [Migrating from SQL Server to Azure SQL Database using BACPAC Files](https://blogs.msdn.microsoft.com/sqlcat/2016/10/20/migrating-from-sql-server-to-azure-sql-database-using-bacpac-files/) (Migrieren von SQL Server zu Azure SQL-Datenbank mithilfe von BACPAC-Dateien).

## <a name="sql-server-management-studio"></a>SQL Server Management Studio

Die neuesten Versionen von SQL Server Management Studio enthalten auch einen Assistenten zum Exportieren einer Azure SQL-Datenbank in eine BACPAC-Datei. Weitere Informationen finden Sie unter [Exportieren einer Datenebenenanwendung](https://docs.microsoft.com/sql/relational-databases/data-tier-applications/export-a-data-tier-application).

## <a name="powershell"></a>PowerShell

Verwenden Sie das [New-AzureRmSqlDatabaseImport](https://docs.microsoft.com/powershell/resourcemanager/azurerm.sql/v2.7.0/new-azurermsqldatabaseexport)-Cmdlet, um eine Anforderung für einen Datenbankexport beim Azure SQL-Datenbankdienst einzureichen. Je nach Größe Ihrer Datenbank kann es einige Zeit dauern, bis der Exportvorgang abgeschlossen ist.

 ```powershell
 $exportRequest = New-AzureRmSqlDatabaseExport -ResourceGroupName $ResourceGroupName -ServerName $ServerName `
   -DatabaseName $DatabaseName -StorageKeytype $StorageKeytype -StorageKey $StorageKey -StorageUri $BacpacUri `
   -AdministratorLogin $creds.UserName -AdministratorLoginPassword $creds.Password
 ```

Zum Überprüfen des Status der Exportanforderung verwenden Sie das [Get-AzureRmSqlDatabaseImportExportStatus](https://docs.microsoft.com/powershell/resourcemanager/azurerm.sql/v2.7.0/get-azurermsqldatabaseimportexportstatus)-Cmdlet. Wenn Sie dieses Cmdlet direkt nach der Anforderung ausführen, wird in der Regel **Status: InProgress** zurückgegeben. Wenn **Status: Succeeded** angezeigt wird, ist der Export abgeschlossen.

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

### <a name="export-sql-database-example"></a>Beispiel für den Export einer SQL-Datenbank
Das folgende Beispiel exportiert eine vorhandene Azure SQL-Datenbank in eine BACPAC-Datei und zeigt dann, wie sich der Status des Exportvorgangs überprüfen lässt.

Um das Beispiel auszuführen, müssen Sie die Beispielwerte einiger Variablen durch die speziellen Werte für Ihre Datenbank und Ihr Speicherkonto ersetzen. Navigieren Sie im [Azure-Portal](https://portal.azure.com)zu Ihrem Speicherkonto, um den Namen, den Blobcontainernamen und den Schlüsselwert des Speicherkontos abzurufen. Sie finden diesen Schlüssel, indem Sie auf dem Blatt Ihres Speicherkontos auf **Zugriffsschlüssel** klicken.

Ersetzen Sie die folgenden `VARIABLE-VALUES` durch die Werte für Ihre jeweiligen Azure-Ressourcen. Beim Datenbanknamen handelt es sich um den Namen der vorhandenen Datenbank, die Sie exportieren möchten.

```powershell
$subscriptionId = "YOUR AZURE SUBSCRIPTION ID"

Login-AzureRmAccount
Set-AzureRmContext -SubscriptionId $subscriptionId

# Database to export
$DatabaseName = "DATABASE-NAME"
$ResourceGroupName = "RESOURCE-GROUP-NAME"
$ServerName = "SERVER-NAME
$serverAdmin = "ADMIN-NAME"
$serverPassword = "ADMIN-PASSWORD" 
$securePassword = ConvertTo-SecureString -String $serverPassword -AsPlainText -Force
$creds = New-Object -TypeName System.Management.Automation.PSCredential -ArgumentList $serverAdmin, $securePassword

# Generate a unique filename for the BACPAC
$bacpacFilename = $DatabaseName + (Get-Date).ToString("yyyyMMddHHmm") + ".bacpac"

# Storage account info for the BACPAC
$BaseStorageUri = "https://STORAGE-NAME.blob.core.windows.net/BLOB-CONTAINER-NAME/"
$BacpacUri = $BaseStorageUri + $bacpacFilename
$StorageKeytype = "StorageAccessKey"
$StorageKey = "YOUR STORAGE KEY"

$exportRequest = New-AzureRmSqlDatabaseExport -ResourceGroupName $ResourceGroupName -ServerName $ServerName `
  -DatabaseName $DatabaseName -StorageKeytype $StorageKeytype -StorageKey $StorageKey -StorageUri $BacpacUri `
  -AdministratorLogin $creds.UserName -AdministratorLoginPassword $creds.Password

$exportRequest

# Check status of the export
Get-AzureRmSqlDatabaseImportExportStatus -OperationStatusLink $exportRequest.OperationStatusLink
```

## <a name="next-steps"></a>Nächste Schritte

* Informationen zur langfristigen Archivierung von Azure SQL-Datenbanksicherungen als Alternative zum Exportieren einer Datenbank für Archivzwecke finden Sie unter [Langfristige Archivierung von Sicherungen](sql-database-long-term-retention.md).
- Einen Blogbeitrag des SQL Server-Kundenberatungsteams zur Migration mithilfe von BACPAC-Dateien finden Sie unter [Migrating from SQL Server to Azure SQL Database using BACPAC Files](https://blogs.msdn.microsoft.com/sqlcat/2016/10/20/migrating-from-sql-server-to-azure-sql-database-using-bacpac-files/) (Migrieren von SQL Server zu Azure SQL-Datenbank mithilfe von BACPAC-Dateien).
* Informationen zum Importieren einer BACPAC-Datei in eine SQL Server-Datenbank finden Sie unter [Importieren einer BACPAC-Datei in eine SQL Server-Datenbank](https://msdn.microsoft.com/library/hh710052.aspx).
* Informationen zum Exportieren einer BACPAC-Datei aus einer SQL Server-Datenbank finden Sie unter [Exportieren einer Datenebenenanwendung](https://docs.microsoft.com/sql/relational-databases/data-tier-applications/export-a-data-tier-application) und [Migrieren Ihrer ersten Datenbank](sql-database-migrate-your-sql-server-database.md).

