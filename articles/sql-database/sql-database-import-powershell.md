<properties
    pageTitle="Importieren einer BACPAC-Datei zum Erstellen einer Azure SQL-Datenbank mithilfe von PowerShell | Microsoft Azure"
    description="Importieren einer BACPAC-Datei zum Erstellen einer Azure SQL-Datenbank mithilfe von PowerShell"
    services="sql-database"
    documentationCenter=""
    authors="stevestein"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.devlang="NA"
    ms.topic="article"
    ms.tgt_pltfrm="powershell"
    ms.workload="data-management"
    ms.date="08/31/2016"
    ms.author="sstein"/>

# Importieren einer BACPAC-Datei zum Erstellen einer Azure SQL-Datenbank mithilfe von PowerShell

**Einzeldatenbank**

> [AZURE.SELECTOR]
- [Azure-Portal](sql-database-import.md)
- [PowerShell](sql-database-import-powershell.md)
- [SSMS](sql-database-cloud-migrate-compatible-import-bacpac-ssms.md)
- [SqlPackage](sql-database-cloud-migrate-compatible-import-bacpac-sqlpackage.md)

Dieser Artikel enthält Anweisungen zum Erstellen einer Azure SQL-Datenbank durch Importieren einer [BACPAC](https://msdn.microsoft.com/library/ee210546.aspx#Anchor_4)-Datei mit PowerShell.

Die Datenbank wird aus einer BACPAC-Datei (.bacpac) erstellt, die aus einem Azure-Speicherblobcontainer importiert wurde. Wenn in Azure Storage keine BACPAC-Datei vorhanden ist, siehe [Archivieren einer Azure SQL-Datenbank in eine BACPAC-Datei mit PowerShell](sql-database-export-powershell.md). Wenn Sie bereits über eine BACPAC-Datei verfügen, die nicht in Azure Storage vorhanden ist, [laden Sie sie mit AzCopy problemlos in Ihr Azure Storage-Konto hoch](../storage/storage-use-azcopy.md#blob-upload).

> [AZURE.NOTE] Azure SQL-Datenbank erstellt und verwaltet automatisch Sicherungen für jede Benutzerdatenbank, die Sie wiederherstellen können. Weitere Informationen finden Sie unter [Übersicht: Automatisierte SQL-Datenbanksicherungen](sql-database-automated-backups.md).


Zum Importieren einer SQL-Datenbank benötigen Sie Folgendes:

- Ein Azure-Abonnement. Wenn Sie ein Azure-Abonnement benötigen, müssen Sie lediglich oben auf dieser Seite auf den Link **Kostenlose Testversion** klicken. Lesen Sie anschließend den Artikel weiter.
- Eine BACPAC-Datei der Datenbank, die Sie importieren möchten. Die BACPAC-Datei muss sich in einem [Azure Storage-Konto](../storage/storage-create-storage-account.md)-Blobcontainer befinden.



[AZURE.INCLUDE [Starten der PowerShell-Sitzung](../../includes/sql-database-powershell.md)]



## Einrichten der Variablen für Ihre Umgebung

Es gibt einige Variablen, bei denen Sie die Beispielwerte durch die speziellen Werte für Ihre Datenbank und Ihr Speicherkonto ersetzen müssen.

Beim Servernamen muss es sich um einen Server handeln, der in dem im vorherigen Schritt ausgewählten Abonnement derzeit vorhanden ist. Es muss sich zudem um den Server handeln, auf dem Sie die Datenbank erstellen möchten. Das direkte Importieren einer Datenbank in einen elastischen Pool wird nicht unterstützt. Sie können jedoch Daten zuerst in eine Einzeldatenbank importieren und die Datenbank anschließend in einen Pool verschieben.

Der Datenbankname ist der gewünschte Name für die neue Datenbank.

    $ResourceGroupName = "resource group name"
    $ServerName = "server name"
    $DatabaseName = "database name"


Die folgenden Variablen stammen aus dem Speicherkonto, in dem sich die BACPAC-Datei befindet. Navigieren Sie im [Azure-Portal](https://portal.azure.com) zu Ihrem Speicherkonto, um diese Werte abzurufen. Sie können auf den primären Zugriffsschlüssel zugreifen, indem Sie auf dem Blatt Ihres Speicherkontos auf **Alle Einstellungen** und auf **Schlüssel** klicken.

Der Blobname ist der Name einer vorhandenen BACPAC-Datei, aus der Sie die Datenbank erstellen möchten. Sie müssen die Erweiterung „.bacpac“ angeben.

    $StorageName = "storageaccountname"
    $StorageKeyType = "StorageAccessKey"
    $StorageUri = "http://$StorageName.blob.core.windows.net/containerName/filename.bacpac"
    $StorageKey = "primaryaccesskey"


Wenn Sie das [Get-Credential](https://msdn.microsoft.com/library/hh849815.aspx)-Cmdlet ausführen, werden Sie in einem Fenster zum Eingeben des Benutzernamens und Kennworts aufgefordert. Geben Sie den Administratorbenutzernamen und das zugehörige Kennwort für den SQL-Datenbankserver ($ServerName, siehe oben) ein, nicht den Benutzernamen und das Kennwort für Ihr Azure-Konto.

    $credential = Get-Credential


## Importieren der Datenbank

Mit diesem Befehl wird eine Anforderung zum Importieren der Datenbank an den Dienst gesendet. Je nach Größe Ihrer Datenbank kann es einige Zeit dauern, bis der Importvorgang abgeschlossen ist.

    $importRequest = New-AzureRmSqlDatabaseImport –ResourceGroupName $ResourceGroupName –ServerName $ServerName –DatabaseName $DatabaseName –StorageKeytype $StorageKeyType –StorageKey $StorageKey -StorageUri $StorageUri –AdministratorLogin $credential.UserName –AdministratorLoginPassword $credential.Password –Edition Standard –ServiceObjectiveName S0 -DatabaseMaxSizeBytes 50000


## Überwachen des Fortschritts des Vorgangs

Nach dem Ausführen von [New-AzureRmSqlDatabaseImport](https://msdn.microsoft.com/library/mt707793.aspx) können Sie den Status der Anforderung überprüfen, indem Sie [Get-AzureRmSqlDatabaseImportExportStatus](https://msdn.microsoft.com/library/mt707794.aspx) ausführen.

    Get-AzureRmSqlDatabaseImportExportStatus -OperationStatusLink $importRequest.OperationStatusLink



## SQL-Datenbank-PowerShell-Importskript


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



## Nächste Schritte

- Informationen zum Herstellen einer Verbindung mit einer importierten SQL-Datenbank und zum Abfragen einer solchen Datenbank finden Sie unter [Herstellen einer Verbindung mit einer SQL-Datenbank mit SQL Server Management Studio und Ausführen einer T-SQL-Beispielabfrage](sql-database-connect-query-ssms.md).

<!---HONumber=AcomDC_0907_2016-->