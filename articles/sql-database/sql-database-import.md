---
title: Importieren einer BACPAC-Datei zum Erstellen einer Azure SQL-Datenbank | Microsoft Docs
description: Erstellen Sie eine neue Azure SQL-Datenbank, indem Sie eine BACPAC-Datei importieren.
services: sql-database
documentationcenter: 
author: CarlRabeler
manager: jhubbard
editor: 
ms.assetid: cf9a9631-56aa-4985-a565-1cacc297871d
ms.service: sql-database
ms.custom: move data
ms.devlang: NA
ms.date: 04/07/2017
ms.author: carlrab
ms.workload: data-management
ms.topic: article
ms.tgt_pltfrm: NA
ms.translationtype: Human Translation
ms.sourcegitcommit: aaf97d26c982c1592230096588e0b0c3ee516a73
ms.openlocfilehash: 42ac754ba24ba3e8f96d4fb6f3abb97169056c8e
ms.contentlocale: de-de
ms.lasthandoff: 04/27/2017


---
# <a name="import-a-bacpac-file-to-a-new-azure-sql-database"></a>Importieren einer BACPAC-Datei in eine neue Azure SQL-Datenbank

In diesem Artikel wird das Importieren einer [BACPAC](https://msdn.microsoft.com/library/ee210546.aspx#Anchor_4)-Datei in eine neue Azure SQL-Datenbank erläutert. In diesem Artikel werden die folgenden Methoden verwendet:
- Das [Azure-Portal](https://portal.azure.com)
- Das Befehlszeilenprogramm [SqlPackage](https://msdn.microsoft.com/library/hh550080.aspx)
- Das [New-AzureRmSqlDatabaseImport](/powershell/module/azurerm.sql/new-azurermsqldatabaseimport)-Cmdlet

## <a name="overview"></a>Übersicht

Wenn Sie eine Datenbank aus einem Archiv importieren müssen oder eine Migration von einer anderen Plattform durchführen, können Sie das Datenbankschema und die Datenbankdaten aus einer BACPAC-Datei importieren. Eine BACPAC-Datei ist eine ZIP-Datei mit der Erweiterung BACPAC. Sie enthält die Metadaten und Daten aus einer SQL Server-Datenbank. Sie können eine BACPAC-Datei aus Azure Blob Storage (nur Standardspeicher) oder aus dem lokalen Speicher an einem lokalen Speicherort importieren. Um die Importgeschwindigkeit zu maximieren, wird empfohlen, eine höhere Dienstebene und Leistungsstufe Servicelevel, z.B. P6, anzugeben und sie anschließend nach Bedarf herunterzuskalieren, nachdem der Import erfolgreich abgeschlossen wurde. Außerdem beruht der Datenbank-Kompatibilitätsgrad nach dem Import auf dem Kompatibilitätsgrad der Quelldatenbank. 

> [!IMPORTANT] 
> Nachdem Sie Ihre Datenbank zu Azure SQL-Datenbank migriert haben, können Sie wählen, die Datenbank mit deren aktuellem Kompatibilitätsgrad (Ebene 100 für die Datenbank „AdventureWorks2008R2“) oder mit einem höheren Grad auszuführen. Weitere Informationen zu den Auswirkungen und Optionen für das Ausführen einer Datenbank mit einem bestimmten Kompatibilitätsgrad finden Sie unter [ALTER DATABASE Compatibility Level](https://docs.microsoft.com/sql/t-sql/statements/alter-database-transact-sql-compatibility-level). Lesen Sie auch [ALTER DATABASE SCOPED CONFIGURATION](https://docs.microsoft.com/sql/t-sql/statements/alter-database-scoped-configuration-transact-sql). Dort finden Sie Informationen zu weiteren Einstellungen auf Datenbankebene, die sich auf Kompatibilitätsgrade beziehen.   >

> [!NOTE]
> Um eine BACPAC-Datei in eine neue Datenbank zu importieren, müssen Sie zunächst einen logischen Azure SQL-Datenbankserver erstellen. Ein Tutorial für die Migration einer SQL Server-Datenbank zu einer Azure SQL-Datenbank mit SQLPackage finden Sie unter [Migrieren einer SQL Server-Datenbank](sql-database-migrate-your-sql-server-database.md).
>

## <a name="azure-portal"></a>Azure-Portal

In diesem Artikel wird beschrieben, wie Sie das [Azure-Portal](https://portal.azure.com) verwenden, um eine Azure SQL-Datenbank aus einer BACPAC-Datei erstellen, die im Azure Blob Storage gespeichert ist. Der Import über das Azure-Portal unterstützt nur das Importieren einer BACPAC-Datei aus dem Azure Blob Storage.

Zum Importieren einer Datenbank über das Azure-Portal öffnen Sie die Seite für Ihre Datenbank, und klicken Sie auf der Symbolleiste auf **Importieren**. Geben Sie das Speicherkonto und den Container an, und wählen Sie die BACPAC-Datei aus, die Sie importieren möchten. Wählen Sie die Größe der neuen Datenbank (in der Regel identisch mit dem Ursprung) aus, und geben Sie die SQL Server-Anmeldeinformationen für das Ziel an.  

   ![Datenbankimport](./media/sql-database-import/import.png)

Um den Status des Importvorgangs zu überwachen, öffnen Sie die Seite für den logischen Server mit der zu importierenden Datenbank. Scrollen Sie nach unten bis zu **Vorgänge**, und klicken Sie dann auf **Import-/Exportverlauf**.

### <a name="monitor-the-progress-of-the-import-operation"></a>Überwachen des Fortschritts des Importvorgangs
1. Klicken Sie auf **SQL-Server**.
2. Klicken Sie auf dem Server, auf dem die Wiederherstellung erfolgt.
3. Klicken Sie auf dem Blatt „SQL Server“ im Bereich „Vorgänge“ auf **Import-/Exportverlauf**:
   
   ![Importieren](./media/sql-database-import/import.png)
   ![Importstatus](./media/sql-database-import/import-status.png)

4. Zum Überprüfen, ob die Datenbank auf dem Server aktiv ist, klicken Sie auf **SQL-Datenbanken** und prüfen, ob der Status der neuen Datenbank **Online** ist.

## <a name="sqlpackage"></a>SQLPackage

Informationen zum Importieren einer SQL-Datenbank mit dem Befehlszeilenprogramm [SqlPackage](https://msdn.microsoft.com/library/hh550080.aspx) finden Sie unter [Importparameter und -eigenschaften](https://msdn.microsoft.com/library/hh550080.aspx#Import Parameters and Properties). Das SQLPackage-Hilfsprogramm wird mit den neuesten Versionen von [SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx) und [SQL Server Data Tools für Visual Studio](https://msdn.microsoft.com/library/mt204009.aspx) ausgeliefert. Alternativ dazu können Sie die neueste Version von [SqlPackage](https://www.microsoft.com/download/details.aspx?id=53876) direkt aus dem Microsoft Download Center herunterladen.

Die Verwendung des SQLPackage-Hilfsprogramms wird aus Gründen der Skalierbarkeit und Leistung für die meisten Produktionsumgebungen empfohlen. Einen Blogbeitrag des SQL Server-Kundenberatungsteams zur Migration mithilfe von BACPAC-Dateien finden Sie unter [Migrating from SQL Server to Azure SQL Database using BACPAC Files](https://blogs.msdn.microsoft.com/sqlcat/2016/10/20/migrating-from-sql-server-to-azure-sql-database-using-bacpac-files/) (Migrieren von SQL Server zu Azure SQL-Datenbank mithilfe von BACPAC-Dateien).

Im folgenden SQLPackage-Befehl wird ein Beispielskript für den Import der Datenbank **AdventureWorks2008R2** aus dem lokalen Speicher in einen logischen Azure SQL-Datenbankserver mit dem Namen **mynewserver20170403** verwendet. Dieses Skript veranschaulicht die Erstellung einer neuen Datenbank mit dem Namen **myMigratedDatabase**, der **Premium**-Dienstebene und dem Dienstziel **P6**. Ändern Sie diese Werte entsprechend Ihrer Umgebung.

```cmd
SqlPackage.exe /a:import /tcs:"Data Source=mynewserver20170403.database.windows.net;Initial Catalog=myMigratedDatabase;User Id=ServerAdmin;Password=<change_to_your_password>" /sf:AdventureWorks2008R2.bacpac /p:DatabaseEdition=Premium /p:DatabaseServiceObjective=P6
```

   ![Importieren mit SQLPackage](./media/sql-database-migrate-your-sql-server-database/sqlpackage-import.png)

> [!IMPORTANT]
> Ein logischer Azure SQL-Datenbankserver lauscht auf Port 1433. Wenn Sie versuchen, über eine Unternehmensfirewall eine Verbindung mit einem logischen Azure SQL-Datenbankserver herzustellen, muss dieser Port in der Unternehmensfirewall geöffnet sein, damit der Vorgang erfolgreich ist.
>

In diesem Beispiel wird gezeigt, wie eine Datenbank mithilfe von „SqlPackage.exe“ mit universeller Active Directory-Authentifizierung importiert wird:

```cmd
SqlPackage.exe /a:Import /sf:testExport.bacpac /tdn:NewDacFX /tsn:apptestserver.database.windows.net /ua:True /tid:"apptest.onmicrosoft.com"
```

## <a name="powershell"></a>PowerShell

Verwenden Sie das [New-AzureRmSqlDatabaseImport](/powershell/module/azurerm.sql/new-azurermsqldatabaseimport)-Cmdlet, um eine Anforderung für einen Datenbankimport beim Azure SQL-Datenbankdienst einzureichen. Je nach Größe Ihrer Datenbank kann es einige Zeit dauern, bis der Importvorgang abgeschlossen ist.

 ```powershell
 $importRequest = New-AzureRmSqlDatabaseImport -ResourceGroupName "myResourceGroup" `
    -ServerName $servername `
    -DatabaseName "MyImportSample" `
    -DatabaseMaxSizeBytes "262144000" `
    -StorageKeyType "StorageAccessKey" `
    -StorageKey $(Get-AzureRmStorageAccountKey -ResourceGroupName "myResourceGroup" -StorageAccountName $storageaccountname).Value[0] `
    -StorageUri "http://$storageaccountname.blob.core.windows.net/importsample/sample.bacpac" `
    -Edition "Standard" `
    -ServiceObjectiveName "P6" `
    -AdministratorLogin "ServerAdmin" `
    -AdministratorLoginPassword $(ConvertTo-SecureString -String "ASecureP@assw0rd" -AsPlainText -Force)

 ```

Zum Überprüfen des Status der Importanforderung verwenden Sie das [Get-AzureRmSqlDatabaseImportExportStatus](/powershell/module/azurerm.sql/get-azurermsqldatabaseimportexportstatus)-Cmdlet. Wenn Sie dieses Cmdlet direkt nach der Anforderung ausführen, wird in der Regel **Status: InProgress** zurückgegeben. Wenn **Status: Succeeded** angezeigt wird, ist der Import abgeschlossen.

```powershell
$importStatus = Get-AzureRmSqlDatabaseImportExportStatus -OperationStatusLink $importRequest.OperationStatusLink
[Console]::Write("Importing")
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
* Informationen zum Herstellen einer Verbindung mit einer importierten SQL-Datenbank und zum Abfragen einer solchen Datenbank finden Sie unter [Herstellen einer Verbindung mit einer SQL-Datenbank mit SQL Server Management Studio und Ausführen einer T-SQL-Beispielabfrage](sql-database-connect-query-ssms.md).
* Einen Blogbeitrag des SQL Server-Kundenberatungsteams zur Migration mithilfe von BACPAC-Dateien finden Sie unter [Migrating from SQL Server to Azure SQL Database using BACPAC Files](https://blogs.msdn.microsoft.com/sqlcat/2016/10/20/migrating-from-sql-server-to-azure-sql-database-using-bacpac-files/) (Migrieren von SQL Server zu Azure SQL-Datenbank mithilfe von BACPAC-Dateien).
* Eine Erläuterung des gesamten Migrationsprozesses von SQL Server-Datenbanken, einschließlich Empfehlungen zur Leistung, finden Sie unter [Migrieren einer SQL Server-Datenbank zu Azure SQL-Datenbank](sql-database-cloud-migrate.md).




