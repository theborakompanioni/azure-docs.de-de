<properties 
    pageTitle="Archivieren einer Azure SQL-Datenbank in eine BACPAC-Datei mit PowerShell" 
    description="Archivieren einer Azure SQL-Datenbank in eine BACPAC-Datei mit PowerShell" 
	services="sql-database"
	documentationCenter=""
	authors="stevestein"
	manager="jhubbard"
	editor=""/>

<tags
	ms.service="sql-database"
	ms.devlang="NA"
	ms.date="04/06/2016"
	ms.author="sstein"
	ms.workload="data-management"
	ms.topic="article"
	ms.tgt_pltfrm="NA"/>


# Archivieren einer Azure SQL-Datenbank in eine BACPAC-Datei mit PowerShell

> [AZURE.SELECTOR]
- [Azure-Portal](sql-database-export.md)
- [PowerShell](sql-database-export-powershell.md)


In diesem Artikel wird beschrieben, wie Sie mit PowerShell Ihre Azure SQL-Datenbank in einer BACPAC-Datei archivieren, die im Azure-Blobspeicher gespeichert ist.

Wenn Sie ein Archiv einer Azure SQL-Datenbank erstellen müssen, können Sie das Datenbankschema und die Daten in eine BACPAC-Datei exportieren. Eine BACPAC-Datei ist einfach eine ZIP-Datei mit der Erweiterung BACPAC. Eine BACPAC-Datei kann später im Azure-Blobspeicher oder im lokalen Speicher an einem lokalen Standort gespeichert werden und dann zurück in die Azure SQL-Datenbank oder in eine lokale SQL-Serverinstallation importiert werden.

***Überlegungen***

- Damit ein Archiv hinsichtlich der Transaktionen konsistent ist, müssen Sie entweder sicherstellen, dass keine Schreibaktivitäten während des Exports durchgeführt werden, oder den Export von einer [in Bezug auf Transaktionen konsistenten Kopie](sql-database-copy.md) Ihrer Azure SQL-Datenbank durchführen.
- Die Maximalgröße für eine BACPAC-Datei, die in den Azure-Blobspeicher archiviert wird, beträgt 200 GB. Verwenden Sie die Eingabeaufforderung [SqlPackage](https://msdn.microsoft.com/library/hh550080.aspx), um große Mengen von BACPAC-Dateien im lokalen Speicher zu archivieren. Dieses Hilfsprogramm wird mit Visual Studio und SQL Server bereitgestellt. Sie können auch die aktuelle Version der SQL Server Data Tools [herunterladen](https://msdn.microsoft.com/library/mt204009.aspx), um dieses Hilfsprogramm zu erhalten.
- Das Archivieren in Azure Storage Premium mithilfe einer BACPAC-Datei wird nicht unterstützt.
- Falls der Exportvorgang länger als 20 Stunden dauert, wird er unter Umständen abgebrochen. Um die Leistung während des Exports zu erhöhen, können Sie Folgendes tun:
 - Erhöhen Sie vorübergehend Ihr Servicelevel. 
 - Unterlassen Sie jegliche Lese- und Schreibaktivitäten während des Exports.
 - Verwenden Sie einen gruppierten Index für alle großen Tabellen. Ohne gruppierte Indizes schlägt ein Export, der länger als sechs bis zwölf Stunden dauert, ggf. fehl. Dies liegt daran, dass die Exportdienste einen Tabellenscan durchführen müssen, um die gesamte Tabelle zu exportieren.
 
> [AZURE.NOTE] BACPAC-Dateien eignen sich nicht für Backup- und Wiederherstellungsvorgänge. Azure SQL-Datenbank erstellt für jede Benutzerdatenbank automatisch Sicherungen. Weitere Informationen finden Sie unter [Übersicht über die Geschäftskontinuität](sql-database-business-continuity.md).

Damit Sie die Anweisungen in diesem Artikel ausführen können, benötigen Sie Folgendes:

- Ein Azure-Abonnement. 
- Azure SQL-Datenbank. 
- Ein [Azure-Standardspeicherkonto](../storage/storage-create-storage-account.md) mit einem Blobcontainer, um den BACPAC im Standardspeicher zu speichern.
- Azure PowerShell. Weitere Informationen finden Sie unter [Installieren und Konfigurieren von Azure PowerShell](../powershell-install-configure.md).


## Konfigurieren der Anmeldeinformationen und Auswählen des Abonnements

Zuerst müssen Sie den Zugriff auf Ihr Azure-Konto einrichten. Starten Sie also PowerShell, und führen Sie dann das folgende Cmdlet aus. Geben Sie auf dem Anmeldebildschirm die E-Mail-Adresse und das Kennwort wie für die Anmeldung beim Azure-Portal ein.

	Add-AzureAccount

Nach der erfolgreichen Anmeldung werden einige Informationen auf dem Bildschirm angezeigt, wie die ID, mit der Sie sich angemeldet haben, und die Azure-Abonnements, auf die Sie zugreifen können.


### Auswählen des Azure-Abonnements

Zur Auswahl des Abonnements benötigen Sie Ihre Abonnement-ID oder den Anmeldenamen für das Abonnement (**-SubscriptionName**). Sie können die Abonnement-ID aus den Informationen im vorherigen Schritt kopieren. Falls Sie über mehrere Abonnements verfügen und mehr Details benötigen, können Sie auch das **Get-AzureSubscription**-Cmdlet ausführen und die gewünschten Abonnementinformationen aus dem Resultset kopieren. Wenn Sie Ihre Abonnementinformationen haben, führen Sie das folgende Cmdlet aus:

	Select-AzureSubscription -SubscriptionId 4cac86b0-1e56-bbbb-aaaa-000000000000

Nach dem erfolgreichen Ausführen von **Select-AzureSubscription** kehren Sie zur PowerShell-Eingabeaufforderung zurück. Wenn Sie über mehrere Abonnements verfügen, können Sie **Get-AzureSubscription** ausführen und überprüfen, ob das gewünschte Abonnement den Wert **IsCurrent: True** aufweist.


## Einrichten der Variablen für Ihre jeweilige Umgebung

Es gibt einige Variablen, bei denen Sie die Beispielwerte durch die speziellen Werte für Ihre Datenbank und Ihr Speicherkonto ersetzen müssen.

Ersetzen Sie den Server- und Datenbanknamen durch den derzeitigen Server und die Datenbank in Ihrem Konto. Geben Sie als BLOB-Name den Namen der BACPAC-Datei ein, die erstellt werden soll. Geben Sie beliebigen Text ein, um die BACPAC-Datei zu benennen. Dabei ist nur die Erweiterung „.bacpac“ obligatorisch.

    $ServerName = "servername"
    $DatabaseName = "nameofdatabasetoexport"
    $BlobName = "filename.bacpac"

Navigieren Sie im [Azure-Portal](https://portal.azure.com) zu Ihrem Speicherkonto, um diese Werte zu erhalten. Sie können auf den primären Zugriffsschlüssel zugreifen, indem Sie auf dem Blatt Ihres Speicherkontos auf **Alle Einstellungen** und auf **Schlüssel** klicken.

    $StorageName = "storageaccountname"
    $ContainerName = "blobcontainername"
    $StorageKey = "primaryaccesskey"

## Erstellen eines Zeigers auf Ihren Server und Ihr Speicherkonto

Wenn Sie das **Get-Credential**-Cmdlet ausführen, werden Sie in einem Fenster zum Eingeben des Benutzernamens und Kennworts aufgefordert. Geben Sie den Administratorbenutzernamen und das dazugehörige Kennwort für Ihren SQL-Server ein (NICHT den Benutzernamen und das Kennwort für Ihr Azure-Konto).

    $credential = Get-Credential
    $SqlCtx = New-AzureSqlDatabaseServerContext -ServerName $ServerName -Credential $credential

    $StorageCtx = New-AzureStorageContext -StorageAccountName $StorageName -StorageAccountKey $StorageKey
    $Container = Get-AzureStorageContainer -Name $ContainerName -Context $StorageCtx


## Exportieren der Datenbank

Mit diesem Befehl wird eine Anforderung zum Exportieren der Datenbank an den Dienst gesendet. Je nach Größe Ihrer Datenbank kann es einige Zeit dauern, bis der Exportvorgang abgeschlossen ist.

> [AZURE.IMPORTANT] Um eine im Hinblick auf Transaktionen konsistente BACPAC-Datei sicherzustellen, sollten Sie zunächst [eine Kopie Ihrer Datenbank erstellen](sql-database-copy-powershell.md) und dann die Datenbankkopie exportieren.


    $exportRequest = Start-AzureSqlDatabaseExport -SqlConnectionContext $SqlCtx -StorageContainer $Container -DatabaseName $DatabaseName -BlobName $BlobName
    

## Überwachen des Fortschritts des Exportvorgangs

Nach dem Ausführen von **Start-AzureSqlDatabaseExport** können Sie den Status der Anforderung überprüfen. Wenn die Ausführung sofort nach der Anforderung erfolgt, wird normalerweise **Status: Ausstehend** oder **Status: Wird ausgeführt** ausgegeben. Sie können die Ausführung also mehrmals durchführen, bis in der Ausgabe **Status: Abgeschlossen** angezeigt wird.

Beim Ausführen dieses Befehls werden Sie zur Eingabe eines Kennworts aufgefordert. Geben Sie das Administratorkennwort für Ihren SQL-Server ein.


    Get-AzureSqlDatabaseImportExportStatus -RequestId $exportRequest.RequestGuid -ServerName $ServerName -Username $credential.UserName
    


## Exportieren eines SQL-Datenbank-PowerShell-Skripts


    Add-AzureAccount
    Select-AzureSubscription -SubscriptionId "4cac86b0-1e56-bbbb-aaaa-000000000000"
    
    $ServerName = "servername"
    $DatabaseName = "databasename"
    $BlobName = "bacpacfilename"
    
    $StorageName = "storageaccountname"
    $ContainerName = "blobcontainername"
    $StorageKey = "primaryaccesskey"
    
    $credential = Get-Credential
    $SqlCtx = New-AzureSqlDatabaseServerContext -ServerName $ServerName -Credential $credential
    
    $StorageCtx = New-AzureStorageContext -StorageAccountName $StorageName -StorageAccountKey $StorageKey
    $Container = Get-AzureStorageContainer -Name $ContainerName -Context $StorageCtx
    
    $exportRequest = Start-AzureSqlDatabaseExport -SqlConnectionContext $SqlCtx -StorageContainer $Container -DatabaseName $DatabaseName -BlobName $BlobName
    
    Get-AzureSqlDatabaseImportExportStatus -RequestId $exportRequest.RequestGuid -ServerName $ServerName -Username $credential.UserName
    


## Nächste Schritte

- [Importieren einer Azure SQL-Datenbank](sql-database-import-powershell.md)


## Zusätzliche Ressourcen

- [Übersicht über die Geschäftskontinuität](sql-database-business-continuity.md)
- [Warnungen zur Notfallwiederherstellung](sql-database-disaster-recovery-drills.md)
- [SQL-Datenbankdokumentation](https://azure.microsoft.com/documentation/services/sql-database/)

<!---HONumber=AcomDC_0525_2016-->