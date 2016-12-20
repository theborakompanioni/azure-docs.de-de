---
title: Archivieren einer Azure SQL-Datenbank in eine BACPAC-Datei mit PowerShell
description: Archivieren einer Azure SQL-Datenbank in eine BACPAC-Datei mit PowerShell
services: sql-database
documentationcenter: 
author: stevestein
manager: jhubbard
editor: 
ms.assetid: 9439dd83-812f-4688-97ea-2a89a864d1f3
ms.service: sql-database
ms.custom: migrate and move
ms.devlang: NA
ms.date: 08/15/2016
ms.author: sstein
ms.workload: data-management
ms.topic: article
ms.tgt_pltfrm: NA
translationtype: Human Translation
ms.sourcegitcommit: c886e8e61b00de2f07d7f5a98c2d2f4d5b29b7cf
ms.openlocfilehash: dd264dfc73962a575f0d4b1a32a9ec02752c33ba


---
# <a name="archive-an-azure-sql-database-to-a-bacpac-file-by-using-powershell"></a>Archivieren einer Azure SQL-Datenbank in eine BACPAC-Datei mit PowerShell
> [!div class="op_single_selector"]
> * [Azure-Portal](sql-database-export.md)
> * [SSMS](sql-database-cloud-migrate-compatible-export-bacpac-ssms.md)
> * [SqlPackage](sql-database-cloud-migrate-compatible-export-bacpac-sqlpackage.md)
> * [PowerShell](sql-database-export-powershell.md)
> 

In diesem Artikel wird beschrieben, wie Sie PowerShell verwenden, um Ihre Azure SQL-Datenbank in einer [BACPAC](https://msdn.microsoft.com/library/ee210546.aspx#Anchor_4) -Datei zu archivieren, die im Azure-Blobspeicher gespeichert ist.

Wenn Sie ein Archiv einer Azure SQL-Datenbank erstellen müssen, können Sie das Datenbankschema und die Daten in eine BACPAC-Datei exportieren. Eine BACPAC-Datei ist einfach eine ZIP-Datei mit der Erweiterung BACPAC. Eine BACPAC-Datei kann später im Azure-Blobspeicher oder in einem lokalen Speicherort gespeichert werden. Die Datei kann auch wieder in Azure SQL-Datenbank oder in eine lokal SQL Server-Installation importiert werden.

## <a name="considerations"></a>Überlegungen

* Damit ein Archiv hinsichtlich der Transaktionen konsistent ist, müssen Sie entweder sicherstellen, dass während des Exports keine Schreibaktivitäten durchgeführt werden, oder den Export von einer [in Hinblick auf Transaktionen konsistenten Kopie](sql-database-copy.md) Ihrer Azure SQL-Datenbank durchführen.
* Die Maximalgröße für eine BACPAC-Datei, die in den Azure-Blobspeicher archiviert wird, beträgt 200 GB. Verwenden Sie das Eingabeaufforderungs-Hilfsprogramm [SqlPackage](https://msdn.microsoft.com/library/hh550080.aspx) , um eine große BACPAC-Datei im lokalen Speicher zu archivieren. Dieses Hilfsprogramm wird mit Visual Studio und SQL Server bereitgestellt. Sie können auch die aktuelle Version der SQL Server Data Tools [herunterladen](https://msdn.microsoft.com/library/mt204009.aspx) , um dieses Hilfsprogramm zu erhalten.
* Das Archivieren in Azure Storage Premium mithilfe einer BACPAC-Datei wird nicht unterstützt.
* Falls der Exportvorgang länger als 20 Stunden dauert, wird er unter Umständen abgebrochen. Um die Leistung während des Exports zu erhöhen, können Sie Folgendes tun:
  * Erhöhen Sie vorübergehend Ihr Servicelevel.
  * Verhindern Sie jegliche Lese- und Schreibaktivitäten während des Exports.
  * Verwenden Sie einen [gruppierten Index](https://msdn.microsoft.com/library/ms190457.aspx) mit Werten ungleich NULL in allen großen Tabellen. Ohne gruppierte Indizes schlägt ein Export, der länger als sechs bis zwölf Stunden dauert, ggf. fehl. Dies liegt daran, dass der Exportdienst einen Tabellenscan durchführen muss, um die gesamte Tabelle zu exportieren. Eine gute Möglichkeit, um zu ermitteln, ob Ihre Tabellen für den Export optimiert sind, besteht darin, **DBCC SHOW_STATISTICS** auszuführen und sicherzustellen, dass *RANGE_HI_KEY* nicht NULL ist und der Wert eine gute Verteilung aufweist. Weitere Informationen finden Sie unter [DBCC SHOW_STATISTICS](https://msdn.microsoft.com/library/ms174384.aspx).

> [!NOTE]
> BACPAC-Dateien eignen sich nicht für Backup- und Wiederherstellungsvorgänge. Azure SQL-Datenbank erstellt für jede Benutzerdatenbank automatisch Sicherungen. Weitere Informationen finden Sie unter [Übersicht: Automatisierte SQL-Datenbanksicherungen](sql-database-automated-backups.md).
> 
> 

Damit Sie die Anweisungen in diesem Artikel ausführen können, benötigen Sie Folgendes:

* Ein Azure-Abonnement.
* Eine Azure SQL-Datenbank.
* Ein [Azure-Standardspeicherkonto](../storage/storage-create-storage-account.md)mit einem Blobcontainer, um die BACPAC-Dateien im Standardspeicher zu speichern.

[!INCLUDE [Start your PowerShell session](../../includes/sql-database-powershell.md)]

## <a name="export-your-database"></a>Exportieren der Datenbank
Das Cmdlet [New-AzureRmSqlDatabaseExport](https://msdn.microsoft.com/library/azure/mt707796\(v=azure.300\).aspx) sendet eine Anforderung zum Exportieren der Datenbank an den Dienst. Je nach Größe Ihrer Datenbank kann es einige Zeit dauern, bis der Exportvorgang abgeschlossen ist.

> [!IMPORTANT]
> Um eine im Hinblick auf Transaktionen konsistente BACPAC-Datei sicherzustellen, sollten Sie zunächst [eine Kopie Ihrer Datenbank erstellen](sql-database-copy-powershell.md)und dann die Datenbankkopie exportieren.
> 
> 

     $exportRequest = New-AzureRmSqlDatabaseExport –ResourceGroupName $ResourceGroupName –ServerName $ServerName `
       –DatabaseName $DatabaseName –StorageKeytype $StorageKeytype –StorageKey $StorageKey -StorageUri $BacpacUri `
       –AdministratorLogin $creds.UserName –AdministratorLoginPassword $creds.Password


## <a name="monitor-the-progress-of-the-export-operation"></a>Überwachen des Fortschritts des Exportvorgangs
Nach dem Ausführen von [New-AzureRmSqlDatabaseExport](https://msdn.microsoft.com/library/azure/mt603644\(v=azure.300\).aspx)können Sie den Status der Anforderung überprüfen, indem Sie [Get-AzureRmSqlDatabaseImportExportStatus](https://msdn.microsoft.com/library/azure/mt707794\(v=azure.300\).aspx)ausführen. Wenn Sie dies direkt nach der Anforderung ausführen, wird in der Regel **Status : InProgress**zurückgegeben. Wenn **Status : Succeeded** angezeigt wird, ist der Export abgeschlossen.

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
    $securePassword = ConvertTo-SecureString –String $serverPassword –AsPlainText -Force
    $creds = New-Object –TypeName System.Management.Automation.PSCredential –ArgumentList $serverAdmin, $securePassword

    # Generate a unique filename for the BACPAC
    $bacpacFilename = $DatabaseName + (Get-Date).ToString("yyyyMMddHHmm") + ".bacpac"

    # Storage account info for the BACPAC
    $BaseStorageUri = "https://STORAGE-NAME.blob.core.windows.net/BLOB-CONTAINER-NAME/"
    $BacpacUri = $BaseStorageUri + $bacpacFilename
    $StorageKeytype = "StorageAccessKey"
    $StorageKey = "YOUR STORAGE KEY"

    $exportRequest = New-AzureRmSqlDatabaseExport –ResourceGroupName $ResourceGroupName –ServerName $ServerName `
       –DatabaseName $DatabaseName –StorageKeytype $StorageKeytype –StorageKey $StorageKey -StorageUri $BacpacUri `
       –AdministratorLogin $creds.UserName –AdministratorLoginPassword $creds.Password
    $exportRequest

    # Check status of the export
    Get-AzureRmSqlDatabaseImportExportStatus -OperationStatusLink $exportRequest.OperationStatusLink

## <a name="automate-export-using-azure-automation"></a>Automatisieren des Exports mithilfe von Azure Automation

Sie können Azure Automation verwenden, um SQL-Datenbanken regelmäßig nach einem von Ihnen gewählten Zeitplan zu archivieren. Sie können das [Beispielskript von GitHub](https://github.com/Microsoft/sql-server-samples/tree/master/samples/manage/azure-automation-automated-export) herunterladen. Weitere Informationen zu Azure Automation finden Sie unter [Azure Automation – Übersicht](../automation/automation-intro.md).

## <a name="next-steps"></a>Nächste Schritte
* Informationen zum Importieren einer Azure SQL-Datenbank unter Verwendung von PowerShell finden Sie unter [Importieren einer BACPAC-Datei mithilfe von PowerShell](sql-database-import-powershell.md).

## <a name="additional-resources"></a>Zusätzliche Ressourcen
* [New-AzureRmSqlDatabaseExport](https://msdn.microsoft.com/library/azure/mt707796\(v=azure.300\).aspx)
* [Get-AzureRmSqlDatabaseImportExportStatus](https://msdn.microsoft.com/library/azure/mt707794\(v=azure.300\).aspx)




<!--HONumber=Nov16_HO3-->


