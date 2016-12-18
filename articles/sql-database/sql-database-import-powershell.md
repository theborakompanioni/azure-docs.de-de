---
title: Importieren einer BACPAC-Datei zum Erstellen einer Azure SQL-Datenbank mithilfe von PowerShell | Microsoft Docs
description: Importieren einer BACPAC-Datei zum Erstellen einer Azure SQL-Datenbank mithilfe von PowerShell
services: sql-database
documentationcenter: 
author: stevestein
manager: jhubbard
editor: 
ms.assetid: 8d78da13-43fe-4447-92e0-0a41d0321fd4
ms.service: sql-database
ms.custom: migrate and move; how to
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: powershell
ms.workload: data-management
ms.date: 08/31/2016
ms.author: sstein
translationtype: Human Translation
ms.sourcegitcommit: 219dcbfdca145bedb570eb9ef747ee00cc0342eb
ms.openlocfilehash: 4e1cf312cf8213cbeea84b4fb0ab605248800fcd


---
# <a name="import-a-bacpac-file-to-create-an-azure-sql-database-by-using-powershell"></a>Importieren einer BACPAC-Datei zum Erstellen einer Azure SQL-Datenbank mithilfe von PowerShell
**Einzeldatenbank**

> [!div class="op_single_selector"]
> * [Azure-Portal](sql-database-import.md)
> * [PowerShell](sql-database-import-powershell.md)
> * [SSMS](sql-database-cloud-migrate-compatible-import-bacpac-ssms.md)
> * [SqlPackage](sql-database-cloud-migrate-compatible-import-bacpac-sqlpackage.md)
> 
> 

Dieser Artikel enthält Anweisungen zum Erstellen einer Azure SQL-Datenbank durch Importieren einer [BACPAC](https://msdn.microsoft.com/library/ee210546.aspx#Anchor_4) -Datei mit PowerShell.

Die Datenbank wird aus einer BACPAC-Datei (.bacpac) erstellt, die aus einem Azure-Speicherblobcontainer importiert wurde. Wenn in Azure Storage keine BACPAC-Datei vorhanden ist, siehe [Archivieren einer Azure SQL-Datenbank in eine BACPAC-Datei mit PowerShell](sql-database-export-powershell.md). Wenn Sie bereits über eine BACPAC-Datei verfügen, die nicht in Azure Storage vorhanden ist, [laden Sie sie mit AzCopy problemlos in Ihr Azure Storage-Konto hoch](../storage/storage-use-azcopy.md#blob-upload).

> [!NOTE]
> Azure SQL-Datenbank erstellt und verwaltet automatisch Sicherungen für jede Benutzerdatenbank, die Sie wiederherstellen können. Weitere Informationen finden Sie unter [Übersicht: Automatisierte SQL-Datenbanksicherungen](sql-database-automated-backups.md).
> 
> 

Zum Importieren einer SQL-Datenbank benötigen Sie Folgendes:

* Ein Azure-Abonnement. Wenn Sie ein Azure-Abonnement benötigen, klicken Sie einfach oben auf dieser Seite auf den Link **Kostenloses Konto**. Lesen Sie anschließend den Artikel weiter.
* Eine BACPAC-Datei der Datenbank, die Sie importieren möchten. Die BACPAC-Datei muss sich in einem [Azure Storage-Konto](../storage/storage-create-storage-account.md) -Blobcontainer befinden.

[!INCLUDE [Start your PowerShell session](../../includes/sql-database-powershell.md)]

## <a name="set-up-the-variables-for-your-environment"></a>Einrichten der Variablen für Ihre Umgebung
Es gibt einige Variablen, bei denen Sie die Beispielwerte durch die speziellen Werte für Ihre Datenbank und Ihr Speicherkonto ersetzen müssen.

Beim Servernamen muss es sich um einen Server handeln, der in dem im vorherigen Schritt ausgewählten Abonnement derzeit vorhanden ist. Es muss sich zudem um den Server handeln, auf dem Sie die Datenbank erstellen möchten. Das direkte Importieren einer Datenbank in einen elastischen Pool wird nicht unterstützt. Sie können jedoch Daten zuerst in eine Einzeldatenbank importieren und die Datenbank anschließend in einen Pool verschieben.

Der Datenbankname ist der gewünschte Name für die neue Datenbank.

    $ResourceGroupName = "resource group name"
    $ServerName = "server name"
    $DatabaseName = "database name"


Die folgenden Variablen stammen aus dem Speicherkonto, in dem sich die BACPAC-Datei befindet. Navigieren Sie im [Azure-Portal](https://portal.azure.com)zu Ihrem Speicherkonto, um diese Werte abzurufen. Sie finden den primären Zugriffsschlüssel, indem Sie auf dem Blatt Ihres Speicherkontos auf **Alle Einstellungen** und dann auf **Schlüssel** klicken.

Der Blobname ist der Name einer vorhandenen BACPAC-Datei, aus der Sie die Datenbank erstellen möchten. Sie müssen die Erweiterung „.bacpac“ angeben.

    $StorageName = "storageaccountname"
    $StorageKeyType = "StorageAccessKey"
    $StorageUri = "http://$StorageName.blob.core.windows.net/containerName/filename.bacpac"
    $StorageKey = "primaryaccesskey"


Wenn Sie das [Get-Credential](https://msdn.microsoft.com/library/azure/hh849815\(v=azure.300\).aspx)-Cmdlet ausführen, werden Sie in einem Fenster zum Eingeben des Benutzernamens und Kennworts aufgefordert. Geben Sie den Administratorbenutzernamen und das zugehörige Kennwort für den SQL-Datenbankserver ($ServerName, siehe oben) ein, nicht den Benutzernamen und das Kennwort für Ihr Azure-Konto.

    $credential = Get-Credential


## <a name="import-the-database"></a>Importieren der Datenbank
Mit diesem Befehl wird eine Anforderung zum Importieren der Datenbank an den Dienst gesendet. Je nach Größe Ihrer Datenbank kann es einige Zeit dauern, bis der Importvorgang abgeschlossen ist.

    $importRequest = New-AzureRmSqlDatabaseImport –ResourceGroupName $ResourceGroupName –ServerName $ServerName –DatabaseName $DatabaseName –StorageKeytype $StorageKeyType –StorageKey $StorageKey -StorageUri $StorageUri –AdministratorLogin $credential.UserName –AdministratorLoginPassword $credential.Password –Edition Standard –ServiceObjectiveName S0 -DatabaseMaxSizeBytes 50000


## <a name="monitor-the-progress-of-the-operation"></a>Überwachen des Fortschritts des Vorgangs
Nach dem Ausführen von [New-AzureRmSqlDatabaseImport](https://msdn.microsoft.com/library/azure/mt707793\(v=azure.300\).aspx) können Sie den Status der Anforderung überprüfen, indem Sie [Get-AzureRmSqlDatabaseImportExportStatus](https://msdn.microsoft.com/library/azure/mt707794\(v=azure.300\).aspx) ausführen.

    Get-AzureRmSqlDatabaseImportExportStatus -OperationStatusLink $importRequest.OperationStatusLink



## <a name="sql-database-powershell-import-script"></a>SQL-Datenbank-PowerShell-Importskript
    $ResourceGroupName = "resourceGroupName"
    $ServerName = "servername"
    $DatabaseName = "databasename"

    $StorageName = "storageaccountname"
    $StorageKeyType = "StorageAccessKey"
    $StorageUri = "http://$StorageName.blob.core.windows.net/containerName/filename.bacpac"
    $StorageKey = "primaryaccesskey"

    $credential = Get-Credential

    $importRequest = New-AzureRmSqlDatabaseImport –ResourceGroupName $ResourceGroupName –ServerName $ServerName –DatabaseName $DatabaseName –StorageKeytype $StorageKeyType –StorageKey $StorageKey -StorageUri $StorageUri –AdministratorLogin $credential.UserName –AdministratorLoginPassword $credential.Password –Edition Standard –ServiceObjectiveName S0 -DatabaseMaxSizeBytes 50000

    Get-AzureRmSqlDatabaseImportExportStatus -OperationStatusLink $importRequest.OperationStatusLink



## <a name="next-steps"></a>Nächste Schritte
* Informationen zum Herstellen einer Verbindung mit einer importierten SQL-Datenbank und zum Abfragen einer solchen Datenbank finden Sie unter [Herstellen einer Verbindung mit einer SQL-Datenbank mit SQL Server Management Studio und Ausführen einer T-SQL-Beispielabfrage](sql-database-connect-query-ssms.md)




<!--HONumber=Nov16_HO3-->


