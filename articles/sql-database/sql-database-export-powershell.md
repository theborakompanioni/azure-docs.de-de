---
title: 'PowerShell: Exportieren einer Azure SQL-Datenbank in eine BACPAC-Datei | Microsoft-Dokumentation'
description: Exportieren einer Azure SQL-Datenbank in eine BACPAC-Datei mit PowerShell
services: sql-database
documentationcenter: 
author: stevestein
manager: jhubbard
editor: 
ms.assetid: 9439dd83-812f-4688-97ea-2a89a864d1f3
ms.service: sql-database
ms.custom: migrate and move
ms.devlang: NA
ms.date: 02/07/2017
ms.author: sstein
ms.workload: data-management
ms.topic: article
ms.tgt_pltfrm: NA
translationtype: Human Translation
ms.sourcegitcommit: 503f5151047870aaf87e9bb7ebf2c7e4afa27b83
ms.openlocfilehash: 41774c7f1c038ee017d719e59ed79fb68999dac8
ms.lasthandoff: 03/28/2017


---
# <a name="export-an-azure-sql-database-or-a-sql-server-to-a-bacpac-file-by-using-powershell"></a>Exportieren einer Azure SQL- oder SQL Server-Datenbank in eine BACPAC-Datei mit PowerShell

In diesem Artikel wird beschrieben, wie Sie mit PowerShell Ihre Azure SQL-Datenbank oder eine SQL Server-Datenbank in eine BACPAC-Datei exportieren, die in Azure-Blobspeicher gespeichert ist. Einen Überblick über den Export in eine BACPAC-Datei finden Sie unter [Exportieren in eine BACPAC-Datei](sql-database-export.md).

> [!NOTE]
> Sie können Ihre Azure SQL-Datenbankdatei auch über das [Azure-Portal](sql-database-export-portal.md), [SQL Server Management Studio](sql-database-export-ssms.md) oder [SQLPackage](sql-database-export-sqlpackage.md) in eine BACPAC-Datei exportieren.
>

## <a name="prerequisites"></a>Voraussetzungen

Damit Sie die Anweisungen in diesem Artikel ausführen können, benötigen Sie Folgendes:

* Ein Azure-Abonnement.
* Eine Azure SQL-Datenbank.
* Ein [Azure-Standardspeicherkonto](../storage/storage-create-storage-account.md)mit einem Blobcontainer, um die BACPAC-Dateien im Standardspeicher zu speichern.

[!INCLUDE [Start your PowerShell session](../../includes/sql-database-powershell.md)]

## <a name="export-your-database"></a>Exportieren der Datenbank
Das Cmdlet [New-AzureRmSqlDatabaseExport](https://docs.microsoft.com/powershell/resourcemanager/azurerm.sql/v2.7.0/new-azurermsqldatabaseexport) sendet eine Anforderung zum Exportieren der Datenbank an den Dienst. Je nach Größe Ihrer Datenbank kann es einige Zeit dauern, bis der Exportvorgang abgeschlossen ist.

> [!IMPORTANT]
> Um eine im Hinblick auf Transaktionen konsistente BACPAC-Datei sicherzustellen, sollten Sie zunächst [eine Kopie Ihrer Datenbank erstellen](scripts/sql-database-copy-database-to-new-server-powershell.md)und dann die Datenbankkopie exportieren.
> 
> 

     $exportRequest = New-AzureRmSqlDatabaseExport -ResourceGroupName $ResourceGroupName -ServerName $ServerName `
       -DatabaseName $DatabaseName -StorageKeytype $StorageKeytype -StorageKey $StorageKey -StorageUri $BacpacUri `
       -AdministratorLogin $creds.UserName -AdministratorLoginPassword $creds.Password


## <a name="monitor-the-progress-of-the-export-operation"></a>Überwachen des Fortschritts des Exportvorgangs
Nach dem Ausführen von [New-AzureRmSqlDatabaseExport](https://docs.microsoft.com//powershell/resourcemanager/azurerm.sql/v2.7.0/new-azurermsqldatabaseexport)können Sie den Status der Anforderung überprüfen, indem Sie [Get-AzureRmSqlDatabaseImportExportStatus](https://docs.microsoft.com/powershell/resourcemanager/azurerm.sql/v2.7.0/get-azurermsqldatabaseimportexportstatus)ausführen. Wenn Sie dies direkt nach der Anforderung ausführen, wird in der Regel **Status : InProgress**zurückgegeben. Wenn **Status: Succeeded** angezeigt wird, ist der Export abgeschlossen.

    Get-AzureRmSqlDatabaseImportExportStatus -OperationStatusLink $exportRequest.OperationStatusLink



## <a name="export-sql-database-example"></a>Beispiel für den Export einer SQL-Datenbank
Das folgende Beispiel exportiert eine vorhandene SQL-Datenbank in eine BACPAC-Datei und zeigt dann, wie sich der Status des Exportvorgangs überprüfen lässt.

Um das Beispiel auszuführen, müssen Sie die Beispielwerte einiger Variablen durch die speziellen Werte für Ihre Datenbank und Ihr Speicherkonto ersetzen. Navigieren Sie im [Azure-Portal](https://portal.azure.com)zu Ihrem Speicherkonto, um den Namen, den Blobcontainernamen und den Schlüsselwert des Speicherkontos abzurufen. Sie finden diesen Schlüssel, indem Sie auf dem Blatt Ihres Speicherkontos auf **Zugriffsschlüssel** klicken.

Ersetzen Sie die folgenden `VARIABLE-VALUES` durch die Werte für Ihre jeweiligen Azure-Ressourcen. Beim Datenbanknamen handelt es sich um den Namen der vorhandenen Datenbank, die Sie exportieren möchten.

    $subscriptionId = "YOUR AZURE SUBSCRIPTION ID"

    Login-AzureRmAccount
    Set-AzureRmContext -SubscriptionId $subscriptionId

    # Database to export
    $DatabaseName = "DATABASE-NAME"
    $ResourceGroupName = "RESOURCE-GROUP-NAME"
    $ServerName = "SERVER-NAME"
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

## <a name="automate-export-using-azure-automation"></a>Automatisieren des Exports mithilfe von Azure Automation

Der automatisierte Export von Azure SQL-Datenbanken ist jetzt als Vorschau verfügbar und wird am 1. März 2017 eingestellt. Seit dem 1. Dezember 2016 ist das Konfigurieren des automatisierten Exports für SQL-Datenbanken nicht mehr möglich. Alle vorhandenen Aufträge für automatisierten Export werden bis zum 1. März 2017 weiterhin ausgeführt. Seit dem 1. Dezember 2016 können Sie die [langfristige Sicherungsaufbewahrung](sql-database-long-term-retention.md) oder [Azure Automation](../automation/automation-intro.md) zum regelmäßigen Archivieren von SQL-Datenbanken mithilfe von PowerShell nach einem Zeitplan Ihrer Wahl verwenden. Als Beispielskript können Sie das [Beispielskript von GitHub](https://github.com/Microsoft/sql-server-samples/tree/master/samples/manage/azure-automation-automated-export) herunterladen. 


## <a name="next-steps"></a>Nächste Schritte
* Informationen zum Importieren einer Azure SQL-Datenbank unter Verwendung von PowerShell finden Sie unter [Importieren einer BACPAC-Datei mithilfe von PowerShell](scripts/sql-database-import-from-bacpac-powershell.md).
* Informationen zum Importieren einer BACPAC-Datei finden Sie unter [Importieren einer BACPAC-Datei in eine Azure SQL-Datenbank mithilfe von SqlPackage](sql-database-import-sqlpackage.md).
* Informationen zum Importieren einer BACPAC-Datei über das Azure-Portal finden Sie unter [Importieren einer BACPAC-Datei in eine Azure SQL-Datenbank mithilfe des Azure-Portals](sql-database-import-portal.md).
* Eine Erläuterung des gesamten Migrationsprozesses von SQL Server-Datenbanken, einschließlich Empfehlungen zur Leistung, finden Sie unter [Migrieren einer SQL Server-Datenbank zu Azure SQL-Datenbank](sql-database-cloud-migrate.md).
* Informationen zur langfristigen Archivierung von Azure SQL-Datenbanksicherungen als Alternative zum Exportieren einer Datenbank für Archivzwecke finden Sie unter [Langfristige Archivierung von Sicherungen](sql-database-long-term-retention.md).
* Informationen zum Importieren einer BACPAC-Datei in eine SQL Server-Datenbank finden Sie unter [Importieren einer BACPAC-Datei in eine SQL Server-Datenbank](https://msdn.microsoft.com/library/hh710052.aspx)



## <a name="additional-resources"></a>Zusätzliche Ressourcen
* [New-AzureRmSqlDatabaseExport](https://docs.microsoft.com/powershell/resourcemanager/azurerm.sql/v2.7.0/new-azurermsqldatabaseexport)
* [Get-AzureRmSqlDatabaseImportExportStatus](https://docs.microsoft.com/powershell/resourcemanager/azurerm.sql/v2.7.0/get-azurermsqldatabaseimportexportstatus)


