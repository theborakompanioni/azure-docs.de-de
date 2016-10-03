<properties
	pageTitle="Verwenden von PowerShell zum Sichern und Wiederherstellen von App Service-Apps"
	description="Es wird beschrieben, wie Sie PowerShell zum Sichern und Wiederherstellen einer App in Azure App Service verwenden."
	services="app-service"
	documentationCenter=""
	authors="NKing92"
	manager="wpickett"
    editor="" />

<tags
	ms.service="app-service"
	ms.workload="na"
	ms.tgt_pltfrm="na"
	ms.devlang="na"
	ms.topic="article"
	ms.date="08/10/2016"
	ms.author="nicking"/>
# Verwenden von PowerShell zum Sichern und Wiederherstellen von App Service-Apps

> [AZURE.SELECTOR]
- [PowerShell](app-service-powershell-backup.md)
- [REST-API](../app-service-web/websites-csm-backup.md)

Enthält Informationen dazu, wie Sie Azure PowerShell zum Sichern und Wiederherstellen von [App Service-Apps](https://azure.microsoft.com/services/app-service/web/) verwenden. Weitere Informationen zu Sicherungen von Web-Apps, z.B. zu Anforderungen und Einschränkungen, finden Sie unter [Sichern von Web-Apps in Azure App Service](../app-service-web/web-sites-backup.md).

## Voraussetzungen
Sie benötigen Folgendes, um PowerShell zum Verwalten von App-Sicherungen zu verwenden:

- Eine **SAS-URL**, die Ihnen Lese- und Schreibzugriff auf einen Azure Storage-Container ermöglicht. Eine Erläuterung zu SAS-URLs finden Sie unter [Grundlagen zum SAS-Modell](../storage/storage-dotnet-shared-access-signature-part-1.md). Beispiele für die Verwaltung von Azure Storage mithilfe von PowerShell finden Sie unter [Verwenden von Azure PowerShell mit Azure Storage](../storage/storage-powershell-guide-full.md).
- Eine **Datenbankverbindungszeichenfolge**, wenn Sie eine Datenbank zusammen mit Ihrer Web-App sichern möchten.

### Generieren einer SAS-URL zur Verwendung mit den Web-App-Sicherungs-Cmdlets
Eine SAS-URL kann mit PowerShell generiert werden. Dieses Beispiel zeigt das Generieren einer URL, die mit den in diesem Artikel beschriebenen Cmdlets verwendet werden kann.

		$storageAccountName = "<your storage account's name>"
		$storageAccountRg = "<your storage account's resource group>"

		# This returns an array of keys for your storage account. Be sure to select the appropriate key. Here we select the first key as a default.
		$storageAccountKey = Get-AzureRmStorageAccountKey -ResourceGroupName $storageAccountRg -Name $storageAccountName
		$context = New-AzureStorageContext -StorageAccountName $storageAccountName -StorageAccountKey $storageAccountKey[0].Value

		$blobContainerName = "<name of blob container for app backups>"
		$sasUrl = New-AzureStorageContainerSASToken -Name $blobContainerName -Permission rwdl -Context $context -ExpiryTime (Get-Date).AddMonths(1) -FullUri

## Installieren von Azure PowerShell 1.3.2 oder höher

Eine Anleitung zum Installieren und Verwenden von Azure PowerShell finden Sie unter [Verwenden von Azure PowerShell mit dem Azure Resource Manager](../powershell-install-configure.md).

## Erstellen einer Sicherung

Verwenden Sie das New-AzureRmWebAppBackup-Cmdlet, um eine Sicherung einer Web-App zu erstellen.

		$sasUrl = "<your SAS URL>"
		$resourceGroupName = "Default-Web-WestUS"
		$appName = "ContosoApp"

		$backup = New-AzureRmWebAppBackup -ResourceGroupName $resourceGroupName -Name $appName -StorageAccountUrl $sasUrl

Es wird eine Sicherung mit einem automatisch generierten Namen erstellt. Wenn Sie einen Namen für die Sicherung angeben möchten, können Sie den optionalen Parameter „BackupName“ verwenden.

		$backup = New-AzureRmWebAppBackup -ResourceGroupName $resourceGroupName -Name $appName -StorageAccountUrl $sasUrl -BackupName MyBackup

Um eine Datenbank in die Sicherung einzubeziehen, müssen Sie zuerst mit dem New-AzureRmWebAppDatabaseBackupSetting-Cmdlet eine Datenbank-Sicherungseinstellung erstellen und diese Einstellung dann im Parameter „Databases“ des New-AzureRmWebAppBackup-Cmdlets angeben. Der Parameter „Databases“ akzeptiert auch ein Array mit Datenbankeinstellungen, falls Sie mehr als eine Datenbank sichern möchten.

		$dbSetting1 = New-AzureRmWebAppDatabaseBackupSetting -Name DB1 -DatabaseType SqlAzure -ConnectionString "<connection_string>"
		$dbSetting2 = New-AzureRmWebAppDatabaseBackupSetting -Name DB2 -DatabaseType SqlAzure -ConnectionString "<connection_string>"
		$dbBackup = New-AzureRmWebAppBackup -ResourceGroupName $resourceGroupName -Name $appName -BackupName MyBackup -StorageAccountUrl $sasUrl -Databases $dbSetting1,$dbSetting2

## Abrufen von Sicherungen

Das Get-AzureRmWebAppBackupList-Cmdlet gibt ein Array mit allen Sicherungen für eine Web-App zurück. Sie müssen den Namen der Web-App und der dazugehörigen Ressourcengruppe angeben.

		$resourceGroupName = "Default-Web-WestUS"
		$appName = "ContosoApp"
		$backups = Get-AzureRmWebAppBackupList -Name $appName -ResourceGroupName $resourceGroupName

Verwenden Sie das Get-AzureRmWebAppBackup-Cmdlet, um eine bestimmte Sicherung zu erhalten.

		$backup = Get-AzureRmWebAppBackup -Name $appName -ResourceGroupName $resourceGroupName -BackupId 10102

Sie können der Einfachheit halber ein Web-App-Objekt auch per Pipe-Zeichen in Cmdlets für die Sicherungsverwaltung einbinden.

		$app = Get-AzureRmWebApp -Name ContosoApp -ResourceGroupName Default-Web-WestUS
		$backupList = $app | Get-AzureRmWebAppBackupList
		$backup = $app | Get-AzureRmWebAppBackup -BackupId 10102

## Planen von automatischen Backups

Sie können planen, dass Sicherungen automatisch in bestimmten Intervallen durchgeführt werden. Verwenden Sie das Edit-AzureRmWebAppBackupConfiguration-Cmdlet, um einen Zeitplan für die Sicherung zu konfigurieren. Für dieses Cmdlet können mehrere Parameter verwendet werden:

- **Name**: Der Name der Web-App.
- **ResourceGroupName**: Der Name der Ressourcengruppe, die die Web-App enthält.
- **Slot**: Optional. Der Name des Web-App-Slots.
- **StorageAccountUrl**: Die SAS-URL für den Azure Storage-Container, der zum Speichern der Sicherungen verwendet wird.
- **FrequencyInterval**: Numerischer Wert, der festlegt, wie oft die Sicherungen vorgenommen werden sollen. Der Wert muss eine positive ganze Zahl sein.
- **FrequencyUnit**: Zeiteinheit, die festlegt, wie oft die Sicherungen durchgeführt werden sollen. Die Optionen lauten „Hour“ und „Day“.
- **RetentionPeriodInDays**: Gibt an, wie viele Tage die automatischen Sicherungen gespeichert werden sollen, bevor sie automatisch gelöscht werden.
- **StartTime**: Optional. Der Zeitpunkt, zu dem die automatischen Sicherungen beginnen sollen. Der Sicherungsvorgang beginnt sofort, wenn der Wert NULL ist. Es muss ein DateTime-Wert sein.
- **Databases**: Optional. Ein Array mit DatabaseBackupSettings für die zu sichernden Datenbanken.
- **KeepAtLeastOneBackup**: Optionaler Switch-Parameter. Geben Sie diesen Parameter an, falls eine Sicherung unabhängig vom Alter immer im Speicherkonto aufbewahrt werden soll.

Unten ist ein Beispiel für die Verwendung dieses Cmdlets angegeben.

		$resourceGroupName = "Default-Web-WestUS"
		$appName = "ContosoApp"
		$slotName = "StagingSlot"
		$dbSetting1 = New-AzureRmWebAppDatabaseBackupSetting -Name DB1 -DatabaseType SqlAzure -ConnectionString "<connection_string>"
		$dbSetting2 = New-AzureRmWebAppDatabaseBackupSetting -Name DB2 -DatabaseType SqlAzure -ConnectionString "<connection_string>"
		Edit-AzureRmWebAppBackupConfiguration -Name $appName -ResourceGroupName $resourceGroupName -Slot $slotName `
		  -StorageAccountUrl "<your SAS URL>" -FrequencyInterval 6 -FrequencyUnit Hour -Databases $dbSetting1,$dbSetting2 `
		  -KeepAtLeastOneBackup -StartTime (Get-Date).AddHours(1)

Verwenden Sie zum Abrufen des aktuellen Sicherungszeitplans das Get-AzureRmWebAppBackupConfiguration-Cmdlet. Dies kann hilfreich beim Ändern eines Zeitplans sein, der bereits konfiguriert wurde.

		$configuration = Get-AzureRmWebAppBackupConfiguration -Name $appName -ResourceGroupName $resourceGroupName

		# Modify the configuration slightly
		$configuration.FrequencyInterval = 2
		$configuration.FrequencyUnit = "Day"

		# Apply the new configuration by piping it into the Edit-AzureRmWebAppBackupConfiguration cmdlet
		$configuration | Edit-AzureRmWebAppBackupConfiguration

## Wiederherstellen einer Web-App aus einem Backup

Verwenden Sie zum Wiederherstellen einer Web-App aus eine Sicherung das Restore-AzureRmWebAppBackup-Cmdlet. Die einfachste Möglichkeit zur Verwendung dieses Cmdlets ist das Einbinden eines Sicherungsobjekts per Pipe-Zeichen, das aus dem Get-AzureRmWebAppBackup-Cmdlet oder Get-AzureRmWebAppBackupList-Cmdlet abgerufen wurde.

Nachdem Sie ein Sicherungsobjekt erhalten haben, können Sie es per Pipe-Zeichen in das Restore-AzureRmWebAppBackup-Cmdlet einbinden. Sie geben mit dem Switch-Parameter „Overwrite“ an, dass Sie den Inhalt Ihrer Web-App mit dem Inhalt der Sicherung überschreiben möchten. Falls die Sicherung Datenbanken umfasst, werden auch diese Datenbanken wiederhergestellt.

		$backup | Restore-AzureRmWebAppBackup -Overwrite

Es folgt ein Beispiel dafür, wie Sie Restore-AzureRmWebAppBackup verwenden, indem Sie alle Parameter angeben.

		$resourceGroupName = "Default-Web-WestUS"
		$appName = "ContosoApp"
		$slotName = "StagingSlot"
		$blobName = "ContosoBackup.zip"
		$dbSetting1 = New-AzureRmWebAppDatabaseBackupSetting -Name DB1 -DatabaseType SqlAzure -ConnectionString "<connection_string>"
		$dbSetting2 = New-AzureRmWebAppDatabaseBackupSetting -Name DB2 -DatabaseType SqlAzure -ConnectionString "<connection_string>"
		Restore-AzureRmWebAppBackup -ResourceGroupName $resourceGroupName -Name $appName -Slot $slotName -StorageAccountUrl "<your SAS URL>" -BlobName $blobName -Databases $dbSetting1,$dbSetting2 -Overwrite

## Löschen einer Sicherung

Verwenden Sie das Remove-AzureRmWebAppBackup-Cmdlet, um eine Sicherung zu löschen. Die Sicherung wird damit aus Ihrem Speicherkonto entfernt. Geben Sie Ihren App-Namen, die Ressourcengruppe und die ID der Sicherung an, die Sie löschen möchten.

		$resourceGroupName = "Default-Web-WestUS"
		$appName = "ContosoApp"
		Remove-AzureRmWebAppBackup -ResourceGroupName $resourceGroupName -Name $appName -BackupId 10102

Sie können ein Sicherungsobjekt auch per Pipe-Zeichen in das Remove-AzureRmWebAppBackup-Cmdlet einbinden, um es zu löschen.

		$backup = Get-AzureRmWebAppBackup -Name $appName -ResourceGroupName $resourceGroupName -BackupId 10102
		$backup | Remove-AzureRmWebAppBackup -Overwrite

<!---HONumber=AcomDC_0921_2016-->