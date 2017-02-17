---
title: 'PowerShell: Erste Schritte mit Azure SQL-Datenbank | Microsoft-Dokumentation'
description: "Es wird beschrieben, wie Sie einen logischen SQL-Datenbank-Server, eine Firewallregel auf Serverebene und Datenbanken mit PowerShell erstellen. Außerdem wird beschrieben, wie Sie Datenbanken mit PowerShell abfragen."
keywords: Erstellen einer neuen SQL-Datenbank, Datenbankeinrichtung
services: sql-database
documentationcenter: 
author: stevestein
manager: jhubbard
editor: cgronlun
ms.assetid: 7d99869b-cec5-4583-8c1c-4c663f4afd4d
ms.service: sql-database
ms.custom: single databases
ms.devlang: NA
ms.topic: hero-article
ms.tgt_pltfrm: powershell
ms.workload: data-management
ms.date: 12/09/2016
ms.author: sstein
translationtype: Human Translation
ms.sourcegitcommit: 7f1983e871ea425a095e3a4bf6c7a1b89d6f45a9
ms.openlocfilehash: 51ed691eb206f284cb8bbd682c723f87362caa33


---

# <a name="tutorial-provision-and-access-an-azure-sql-database-using-powershell"></a>Tutorial: Bereitstellen und Verwenden einer Azure SQL-Datenbank mithilfe von PowerShell

In diesem Tutorial zu den ersten Schritten erfahren Sie, wie Sie PowerShell für folgende Aufgaben verwenden:

* Erstellen einer neuen Azure-Ressourcengruppe
* Erstellen eines logischen Azure SQL-Servers
* Anzeigen der Eigenschaften eines Azure SQL-Servers
* Erstellen einer Firewallregel auf Serverebene
* Erstellen der AdventureWorksLT-Beispieldatenbank als Einzeldatenbank
* Anzeigen von Eigenschaften der AdventureWorksLT-Beispieldatenbank
* Erstellen einer leeren Einzeldatenbank

In diesem Tutorial führen Sie außerdem die folgenden Schritte aus:

* Herstellen einer Verbindung mit dem logischen Server und der dazugehörigen Masterdatenbank
* Anzeigen von Eigenschaften der Masterdatenbank
* Herstellen einer Verbindung mit der Beispieldatenbank
* Anzeigen von Eigenschaften der Benutzerdatenbank

Nach Abschluss dieses Tutorials verfügen Sie über eine Beispieldatenbank und eine leere Datenbank, die in einer Azure-Ressourcengruppe ausgeführt wird und einem logischen Server zugeordnet ist. Außerdem verfügen Sie über eine Firewallregel auf Serverebene, die so konfiguriert ist, dass sich der Prinzipal auf Serverebene am Server über eine angegebene IP-Adresse (oder einen IP-Adressbereich) anmelden kann. 

**Geschätzte Zeit**: Dieses Tutorial ist in ca. 30 Minuten abgeschlossen (sofern die Voraussetzungen bereits erfüllt sind).


> [!TIP]
> Sie können diese Aufgaben auch in einem Tutorial zu den ersten Schritten mit dem [Azure-Portal](sql-database-get-started.md) durchführen.
>


## <a name="prerequisites"></a>Voraussetzungen

* Sie benötigen ein Azure-Konto. Sie können entweder ein [kostenloses Azure-Konto erstellen](/pricing/free-trial/?WT.mc_id=A261C142F) oder [Visual Studio-Abonnementvorteile aktivieren](/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F). 

* Sie müssen mit einem Konto angemeldet sein, das über die Rolle „Besitzer“ oder „Mitwirkender“ für das Abonnement verfügt. Weitere Informationen zur rollenbasierten Zugriffssteuerung finden Sie unter [Erste Schritte mit der Zugriffsverwaltung im Azure-Portal](../active-directory/role-based-access-control-what-is.md).

* Sie benötigen die BACPAC-Datei der AdventureWorksLT-Beispieldatei in Azure Blob Storage.

### <a name="download-the-adventureworkslt-sample-database-bacpac-file-and-save-it-in-azure-blob-storage"></a>Herunterladen der BACPAC-Datei der AdventureWorksLT-Beispieldatenbank und Speichern der Datei in Azure Blob Storage

In diesem Tutorial wird durch Importieren einer BACPAC-Datei aus Azure Storage eine neue AdventureWorksLT-Datenbank erstellt. Der erste Schritt besteht darin, eine Kopie der Datei „AdventureWorksLT.bacpac“ abzurufen und in Blob Storage hochzuladen.
In den folgenden Schritten wird die Beispieldatenbank für den Import vorbereitet:

1. [Laden Sie die Datei „AdventureWorksLT.bacpac“ herunter](https://sqldbbacpacs.blob.core.windows.net/bacpacs/AdventureWorksLT.bacpac), und speichern Sie sie mit der Dateierweiterung „.bacpac“.
2. [Erstellen Sie ein Speicherkonto](../storage/storage-create-storage-account.md#create-a-storage-account) (Sie können es mit den Standardeinstellungen erstellen).
3. Erstellen Sie einen neuen **Container**, indem Sie zum Speicherkonto navigieren, **BLOBs** auswählen und dann auf **+ Container** klicken.
4. Laden Sie die BACPAC-Datei in den Blob-Container in Ihrem Speicherkonto hoch. Sie können die Schaltfläche **Upload** am oberen Rand der Seite „Container“ oder [AzCopy](../storage/storage-use-azcopy.md#blob-upload) verwenden. 
5. Nachdem Sie die Datei „AdventureWorksLT.bacpac“ gespeichert haben, benötigen Sie die URL und den Speicherkontoschlüssel für den Import-Codeausschnitt an späterer Stelle dieses Tutorials. 
   * Wählen Sie die BACPAC-Datei aus, und kopieren Sie die URL. Die URL sieht in etwa wie folgt aus: https://{storage-account-name}.blob.core.windows.net/{container-name}/AdventureWorksLT.bacpac. Klicken Sie auf der Seite „Speicherkonto“ auf **Zugriffsschlüssel**, und kopieren Sie den Schlüssel mit der Bezeichnung **key1**.


[!INCLUDE [Start your PowerShell session](../../includes/sql-database-powershell.md)]


## <a name="create-a-new-logical-sql-server-using-azure-powershell"></a>Erstellen eines neuen logischen SQL-Servers mit Azure PowerShell

Sie benötigen eine Ressourcengruppe, die den Server enthält. Der erste Schritt besteht daher darin, entweder eine neue Ressourcengruppe und einen neuen Server zu erstellen ([New-AzureRmResourceGroup](https://docs.microsoft.com/powershell/resourcemanager/azurerm.resources/v3.3.0/new-azurermresourcegroup), [New-AzureRmSqlServer](https://docs.microsoft.com/powershell/resourcemanager/azurerm.sql/v2.3.0/new-azurermsqlserver)) oder Verweise auf eine vorhandene Ressourcengruppe und einen vorhandenen Server abzurufen ([Get-AzureRmResourceGroup](https://docs.microsoft.com/powershell/resourcemanager/azurerm.resources/v3.3.0/get-azurermresourcegroup), [Get-AzureRmSqlServer](https://docs.microsoft.com/powershell/resourcemanager/azurerm.sql/v2.3.0/get-azurermsqlserver)).
Die folgenden Codeausschnitte erstellen eine Ressourcengruppe und einen Azure SQL-Server, sofern diese noch nicht vorhanden sind:

Den Code zum Abrufen einer Liste der gültigen Azure-Speicherorte und das Zeichenfolgenformat finden Sie weiter unten im Abschnitt [Hilfscodeausschnitte](#helper-snippets).
```
# Create new, or get existing resource group
############################################

$resourceGroupName = "{resource-group-name}"
$resourceGroupLocation = "{resource-group-location}"

$myResourceGroup = Get-AzureRmResourceGroup -Name $resourceGroupName -ea SilentlyContinue

if(!$myResourceGroup)
{
   Write-Output "Creating resource group: $resourceGroupName"
   $myResourceGroup = New-AzureRmResourceGroup -Name $resourceGroupName -Location $resourceGroupLocation
}
else
{
   Write-Output "Resource group $resourceGroupName already exists:"
}
$myResourceGroup



# Create a new, or get existing server
######################################

$serverName = "{server-name}"
$serverVersion = "12.0"
$serverLocation = $resourceGroupLocation
$serverResourceGroupName = $resourceGroupName

$serverAdmin = "{server-admin}"
$serverAdminPassword = "{server-admin-password}"

$securePassword = ConvertTo-SecureString -String $serverAdminPassword -AsPlainText -Force
$serverCreds = New-Object -TypeName System.Management.Automation.PSCredential -ArgumentList $serverAdmin, $securePassword

$myServer = Get-AzureRmSqlServer -ServerName $serverName -ResourceGroupName $serverResourceGroupName -ea SilentlyContinue
if(!$myServer)
{
   Write-Output "Creating SQL server: $serverName"
   $myServer = New-AzureRmSqlServer -ResourceGroupName $serverResourceGroupName -ServerName $serverName -Location $serverLocation -ServerVersion $serverVersion -SqlAdministratorCredentials $serverCreds
}
else
{
   Write-Output "SQL server $serverName already exists:"
}
$myServer

```


## <a name="view-the-logical-sql-server-properties-using-azure-powershell"></a>Anzeigen der Eigenschaften des logischen SQL-Servers mit Azure PowerShell

```
#$serverResourceGroupName = "{resource-group-name}"
#$serverName = "{server-name}"

$myServer = Get-AzureRmSqlServer -ServerName $serverName -ResourceGroupName $serverResourceGroupName 

Write-Host "Server name: " $myServer.ServerName
Write-Host "Fully qualified server name: $serverName.database.windows.net"
Write-Host "Server location: " $myServer.Location
Write-Host "Server version: " $myServer.ServerVersion
Write-Host "Server administrator login: " $myServer.SqlAdministratorLogin
```


## <a name="create-a-server-level-firewall-rule-using-azure-powershell"></a>Erstellen einer Firewallregel auf Serverebene mit Azure PowerShell

Zum Festlegen der Firewallregel benötigen Sie Ihre öffentliche IP-Adresse. Sie können die IP-Adresse in einem beliebigen Browser abrufen (mit der Frage „wie lautet meine IP-Adresse“). Ausführliche Informationen finden Sie unter [Firewallregeln](sql-database-firewall-configure.md).

Im Folgenden werden die Cmdlets [Get-AzureRmSqlServerFirewallRule](https://docs.microsoft.com/powershell/resourcemanager/azurerm.sql/v2.3.0/get-azurermsqlserverfirewallrule) und [New-AzureRmSqlServerFirewallRule](https://docs.microsoft.com/powershell/resourcemanager/azurerm.sql/v2.3.0/new-azurermsqlserverfirewallrule) verwendet, um einen Verweis abzurufen oder eine neue Regel zu erstellen. Wenn die Regel bereits vorhanden ist, ruft dieser Codeausschnitt nur einen Verweis auf die Regel ab, und die Start- und End-IP-Adressen werden nicht aktualisiert. Sie können die **else**-Klausel immer ändern, um [Set-AzureRmSqlServerFirewallRule](https://docs.microsoft.com/powershell/resourcemanager/azurerm.sql/v2.3.0/set-azurermsqlserverfirewallrule) für die Erstellungs- oder Aktualisierungsfunktion zu verwenden.

> [!NOTE] 
> Sie können die SQL-Datenbank-Firewall auf dem Server für eine einzelne IP-Adresse oder einen gesamten Adressbereich öffnen. Mit dem Öffnen der Firewall wird es SQL-Administratoren und Benutzern ermöglicht, sich auf dem Server an allen Datenbanken anzumelden, für die sie über gültige Anmeldeinformationen verfügen.
>

```
#$serverName = "{server-name}"
#$serverResourceGroupName = "{resource-group-name}"
$serverFirewallRuleName = "{server-firewall-rule-name}"
$serverFirewallStartIp = "{server-firewall-rule-startIp}"
$serverFirewallEndIp = "{server-firewall-rule-endIp}"

$myFirewallRule = Get-AzureRmSqlServerFirewallRule -FirewallRuleName $serverFirewallRuleName -ServerName $serverName -ResourceGroupName $serverResourceGroupName -ea SilentlyContinue

if(!$myFirewallRule)
{
   Write-host "Creating server firewall rule: $serverFirewallRuleName"
   $myFirewallRule = New-AzureRmSqlServerFirewallRule -ResourceGroupName $serverResourceGroupName -ServerName $serverName -FirewallRuleName $serverFirewallRuleName -StartIpAddress $serverFirewallStartIp -EndIpAddress $serverFirewallEndIp
}
else
{
   Write-host "Server firewall rule $serverFirewallRuleName already exists:"
}
$myFirewallRule
```


## <a name="connect-to-sql-server-using-azure-powershell"></a>Herstellen einer Verbindung mit einem SQL-Server mit Azure PowerShell

Zunächst führen wir eine schnelle Abfrage der Masterdatenbank aus, um zu überprüfen, ob wir eine Verbindung mit dem Server herstellen können. Im folgenden Codeausschnitt wird der [.NET Framework-Anbieter für SQL Server (System.Data.SqlClient)](https://msdn.microsoft.com/library/system.data.sqlclient(v=vs.110).aspx) verwendet, um eine Verbindung herzustellen und die Masterdatenbank des Servers abzufragen. Dabei wird basierend auf den Variablen, die wir in den vorherigen Codeausschnitten verwendet haben, eine Verbindungszeichenfolge erstellt. Ersetzen Sie die Platzhalter durch den SQL Server-Administrator und das Kennwort, den bzw. das Sie in den vorherigen Schritten zum Erstellen des Servers verwendet haben.


```
#$serverName = "{server-name}"
#$serverAdmin = "{server-admin}"
#$serverAdminPassword = "{server-admin-password}"
$databaseName = "master"

$connectionString = "Server=tcp:" + $serverName + ".database.windows.net" + ",1433;Initial Catalog=" + $databaseName + ";Persist Security Info=False;User ID=$serverAdmin;Password=$serverAdminPassword" + ";MultipleActiveResultSets=False;Encrypt=True;TrustServerCertificate=False;Connection Timeout=30;"



$connection = New-Object System.Data.SqlClient.SqlConnection
$connection.ConnectionString = $connectionString
$connection.Open()
$command = New-Object System.Data.SQLClient.SQLCommand("select * from sys.objects", $connection)

$command.Connection = $connection
$reader = $command.ExecuteReader()


$sysObjects = ""
while ($reader.Read()) {
    $sysObjects += $reader["name"] + "`n"
}
$sysObjects

$connection.Close()
```


## <a name="create-new-adventureworkslt-sample-database-using-azure-powershell"></a>Erstellen einer neuer AdventureWorksLT-Beispieldatenbank mit Azure PowerShell

Der folgende Codeausschnitt importiert mithilfe des Cmdlets [New-AzureRmSqlDatabaseImport](https://docs.microsoft.com/powershell/resourcemanager/azurerm.sql/v2.3.0/new-azurermsqldatabaseimport) eine BACPAC-Datei der AdventureWorksLT-Beispieldatenbank. Die BACPAC-Datei befindet sich in Azure Blob Storage. Nachdem Sie das Import-Cmdlet ausgeführt haben, können Sie den Status des Importvorgangs mit dem Cmdlet [Get-AzureRmSqlDatabaseImportExportStatus](https://docs.microsoft.com/powershell/resourcemanager/azurerm.sql/v2.3.0/get-azurermsqldatabaseimportexportstatus) überwachen.
„$storageUri“ ist die URL-Eigenschaft der BACPAC-Datei, die Sie zuvor in das Portal hochgeladen haben, und sollte in etwa wie folgt lauten: https://{storage-account}.blob.core.windows.net/{container}/AdventureWorksLT.bacpac

```
#$resourceGroupName = "{resource-group-name}"
#$serverName = "{server-name}"

$databaseName = "AdventureWorksLT"
$databaseEdition = "Basic"
$databaseServiceLevel = "Basic"

$storageKeyType = "StorageAccessKey"
$storageUri = "{storage-uri}" # URL of bacpac file you uploaded to your storage account
$storageKey = "{storage-key}" # key1 in the Access keys setting of your storage account

$importRequest = New-AzureRmSqlDatabaseImport -ResourceGroupName $resourceGroupName -ServerName $serverName -DatabaseName $databaseName -StorageKeytype $storageKeyType -StorageKey $storageKey -StorageUri $storageUri -AdministratorLogin $serverAdmin -AdministratorLoginPassword $securePassword -Edition $databaseEdition -ServiceObjectiveName $databaseServiceLevel -DatabaseMaxSizeBytes 5000000


Do {
     $importStatus = Get-AzureRmSqlDatabaseImportExportStatus -OperationStatusLink $importRequest.OperationStatusLink
     Write-host "Importing database..." $importStatus.StatusMessage
     Start-Sleep -Seconds 30
     $importStatus.Status
   }
   until ($importStatus.Status -eq "Succeeded")
$importStatus
```



## <a name="view-database-properties-using-azure-powershell"></a>Anzeigen von Datenbankeigenschaften mit Azure PowerShell

Zeigen Sie nach dem Erstellen der Datenbank einige ihrer Eigenschaften an.

```
#$resourceGroupName = "{resource-group-name}"
#$serverName = "{server-name}"
#$databaseName = "{database-name}"

$myDatabase = Get-AzureRmSqlDatabase -ResourceGroupName $resourceGroupName -ServerName $serverName -DatabaseName $databaseName


Write-Host "Database name: " $myDatabase.DatabaseName
Write-Host "Server name: " $myDatabase.ServerName
Write-Host "Creation date: " $myDatabase.CreationDate
Write-Host "Database edition: " $myDatabase.Edition
Write-Host "Database performance level: " $myDatabase.CurrentServiceObjectiveName
Write-Host "Database status: " $myDatabase.Status
```

## <a name="connect-and-query-the-sample-database-using-azure-powershell"></a>Herstellen einer Verbindung und Abfragen der Beispieldatenbank mit Azure PowerShell

Zunächst führen wir eine schnelle Abfrage der AdventureWorksLT-Datenbank aus, um zu überprüfen, ob wir eine Verbindung herstellen können. Im folgenden Codeausschnitt wird der [.NET Framework-Anbieter für SQL Server (System.Data.SqlClient)](https://msdn.microsoft.com/library/system.data.sqlclient(v=vs.110).aspx) verwendet, um eine Verbindung herzustellen und die Datenbank abzufragen. Dabei wird basierend auf den Variablen, die wir in den vorherigen Codeausschnitten verwendet haben, eine Verbindungszeichenfolge erstellt. Ersetzen Sie den Platzhalter durch das SQL Server-Administratorkennwort.

```
#$serverName = {server-name}
#$serverAdmin = "{server-admin}"
#$serverAdminPassword = "{server-admin-password}"
#$databaseName = {database-name}

$connectionString = "Server=tcp:" + $serverName + ".database.windows.net" + ",1433;Initial Catalog=" + $databaseName + ";Persist Security Info=False;User ID=$serverAdmin;Password=$serverAdminPassword" + ";MultipleActiveResultSets=False;Encrypt=True;TrustServerCertificate=False;Connection Timeout=30;"


$connection = New-Object System.Data.SqlClient.SqlConnection
$connection.ConnectionString = $connectionString
$connection.Open()
$command = New-Object System.Data.SQLClient.SQLCommand("select * from sys.objects", $connection)

$command.Connection = $connection
$reader = $command.ExecuteReader()


$sysObjects = ""
while ($reader.Read()) {
    $sysObjects += $reader["name"] + "`n"
}
$sysObjects

$connection.Close()
```

## <a name="create-a-new-blank-database-using-azure-powershell"></a>Erstellen einer neuen leeren Datenbank mit Azure PowerShell

```
#$resourceGroupName = {resource-group-name}
#$serverName = {server-name}

$blankDatabaseName = "blankdb"
$blankDatabaseEdition = "Basic"
$blankDatabaseServiceLevel = "Basic"


$myBlankDatabase = New-AzureRmSqlDatabase -DatabaseName $blankDatabaseName -ServerName $serverName -ResourceGroupName $resourceGroupName -Edition $blankDatabaseEdition -RequestedServiceObjectiveName $blankDatabaseServiceLevel
$myBlankDatabase
```


## <a name="complete-azure-powershell-script-to-create-a-server-firewall-rule-and-database"></a>Vollständiges Azure PowerShell-Skript zum Erstellen eines Servers, einer Firewallregel und einer Datenbank



```
# Sign in to Azure and set the subscription to work with
########################################################

$SubscriptionId = "{subscription-id}"

Add-AzureRmAccount
Set-AzureRmContext -SubscriptionId $SubscriptionId

# User variables
################

$myResourceGroupName = "{resource-group-name}"
$myResourceGroupLocation = "{resource-group-location}"

$myServerName = "{server-name}"
$myServerVersion = "12.0"
$myServerLocation = $myResourceGroupLocation
$myServerResourceGroupName = $myResourceGroupName
$myServerAdmin = "{server-admin}"
$myServerAdminPassword = "{server-admin-password}" 

$myServerFirewallRuleName = "{server-firewall-rule-name}"
$myServerFirewallStartIp = "{start-ip}"
$myServerFirewallEndIp = "{end-ip}"

$myDatabaseName = "AdventureWorksLT"
$myDatabaseEdition = "Basic"
$myDatabaseServiceLevel = "Basic"

$myStorageKeyType = "StorageAccessKey"
# Get these values from your Azure storage account:
$myStorageUri = "{http://your-storage-account.blob.core.windows.net/your-container/AdventureWorksLT.bacpac}"
$myStorageKey = "{your-storage-key}"


# Create new, or get existing resource group
############################################

$resourceGroupName = $myResourceGroupName
$resourceGroupLocation = $myResourceGroupLocation

$myResourceGroup = Get-AzureRmResourceGroup -Name $resourceGroupName -ea SilentlyContinue

if(!$myResourceGroup)
{
   Write-host "Creating resource group: $resourceGroupName"
   $myResourceGroup = New-AzureRmResourceGroup -Name $resourceGroupName -Location $resourceGroupLocation
}
else
{
   Write-host "Resource group $resourceGroupName already exists:"
}
$myResourceGroup


# Create a new, or get existing server
######################################

$serverName = $myServerName
$serverVersion = $myServerVersion
$serverLocation = $myServerLocation
$serverResourceGroupName = $myServerResourceGroupName

$serverAdmin = $myServerAdmin
$serverAdminPassword = $myServerAdminPassword

$securePassword = ConvertTo-SecureString -String $serverAdminPassword -AsPlainText -Force
$serverCreds = New-Object -TypeName System.Management.Automation.PSCredential -ArgumentList $serverAdmin, $securePassword

$myServer = Get-AzureRmSqlServer -ServerName $serverName -ResourceGroupName $serverResourceGroupName -ea SilentlyContinue
if(!$myServer)
{
   Write-host "Creating SQL server: $serverName"
   $myServer = New-AzureRmSqlServer -ResourceGroupName $serverResourceGroupName -ServerName $serverName -Location $serverLocation -ServerVersion $serverVersion -SqlAdministratorCredentials $serverCreds
}
else
{
   Write-host "SQL server $serverName already exists:"
}
$myServer


# View server properties
##########################

$resourceGroupName = $myResourceGroupName
$serverName = $myServerName

$myServer = Get-AzureRmSqlServer -ServerName $serverName -ResourceGroupName $serverResourceGroupName 

Write-Host "Server name: " $myServer.ServerName
Write-Host "Fully qualified server name: $serverName.database.windows.net"
Write-Host "Server location: " $myServer.Location
Write-Host "Server version: " $myServer.ServerVersion
Write-Host "Server administrator login: " $myServer.SqlAdministratorLogin


# Create or update server firewall rule
#######################################

$serverFirewallRuleName = $myServerFirewallRuleName
$serverFirewallStartIp = $myServerFirewallStartIp
$serverFirewallEndIp = $myServerFirewallEndIp

$myFirewallRule = Get-AzureRmSqlServerFirewallRule -FirewallRuleName $serverFirewallRuleName -ServerName $serverName -ResourceGroupName $serverResourceGroupName -ea SilentlyContinue

if(!$myFirewallRule)
{
   Write-host "Creating server firewall rule: $serverFirewallRuleName"
   $myFirewallRule = New-AzureRmSqlServerFirewallRule -ResourceGroupName $serverResourceGroupName -ServerName $serverName -FirewallRuleName $serverFirewallRuleName -StartIpAddress $serverFirewallStartIp -EndIpAddress $serverFirewallEndIp
}
else
{
   Write-host "Server firewall rule $serverFirewallRuleName already exists:"
}
$myFirewallRule


# Connect to the server and master database
###########################################
$databaseName = "master"

$connectionString = "Server=tcp:" + $serverName + ".database.windows.net" + ",1433;Initial Catalog=" + $databaseName + ";Persist Security Info=False;User ID=" + $myServer.SqlAdministratorLogin + ";Password=" + $myServerAdminPassword + ";MultipleActiveResultSets=False;Encrypt=True;TrustServerCertificate=False;Connection Timeout=30;"


$connection = New-Object System.Data.SqlClient.SqlConnection
$connection.ConnectionString = $connectionString
$connection.Open()
$command = New-Object System.Data.SQLClient.SQLCommand("select * from sys.objects", $connection)

$command.Connection = $connection
$reader = $command.ExecuteReader()

$sysObjects = ""
while ($reader.Read()) {
    $sysObjects += $reader["name"] + "`n"
}
$sysObjects

$connection.Close()


# Create the AdventureWorksLT database from a bacpac
####################################################

$resourceGroupName = $myResourceGroupName
$serverName = $myServerName

$databaseName = $myDatabaseName
$databaseEdition = $myDatabaseEdition
$databaseServiceLevel = $myDatabaseServiceLevel

$storageKeyType = $myStorageKeyType
$storageUri = $myStorageUri
$storageKey = $myStorageKey

$importRequest = New-AzureRmSqlDatabaseImport -ResourceGroupName $resourceGroupName -ServerName $serverName -DatabaseName $databaseName -StorageKeytype $storageKeyType -StorageKey $storageKey -StorageUri $storageUri -AdministratorLogin $serverAdmin -AdministratorLoginPassword $securePassword -Edition $databaseEdition -ServiceObjectiveName $databaseServiceLevel -DatabaseMaxSizeBytes 5000000

Do {
     $importStatus = Get-AzureRmSqlDatabaseImportExportStatus -OperationStatusLink $importRequest.OperationStatusLink
     Write-host "Importing database..." $importStatus.StatusMessage
     Start-Sleep -Seconds 30
     $importStatus.Status
   }
   until ($importStatus.Status -eq "Succeeded")
$importStatus


# View database properties
##########################

$resourceGroupName = $myResourceGroupName
$serverName = $myServerName
$databaseName = $myDatabaseName

$myDatabase = Get-AzureRmSqlDatabase -ResourceGroupName $resourceGroupName -ServerName $serverName -DatabaseName $databaseName

Write-Host "Database name: " $myDatabase.DatabaseName
Write-Host "Server name: " $myDatabase.ServerName
Write-Host "Creation date: " $myDatabase.CreationDate
Write-Host "Database edition: " $myDatabase.Edition
Write-Host "Database performance level: " $myDatabase.CurrentServiceObjectiveName
Write-Host "Database status: " $myDatabase.Status


# Query the database
####################

$connectionString = "Server=tcp:" + $serverName + ".database.windows.net" + ",1433;Initial Catalog=" + $databaseName + ";Persist Security Info=False;User ID=" + $myServer.SqlAdministratorLogin + ";Password=" + $myServerAdminPassword + ";MultipleActiveResultSets=False;Encrypt=True;TrustServerCertificate=False;Connection Timeout=30;"

$connection = New-Object System.Data.SqlClient.SqlConnection
$connection.ConnectionString = $connectionString
$connection.Open()
$command = New-Object System.Data.SQLClient.SQLCommand("select * from sys.objects", $connection)

$command.Connection = $connection
$reader = $command.ExecuteReader()

$sysObjects = ""
while ($reader.Read()) {
    $sysObjects += $reader["name"] + "`n"
}
$sysObjects

$connection.Close()


# Create a blank database
#########################

$blankDatabaseName = "blankdb"
$blankDatabaseEdition = "Basic"
$blankDatabaseServiceLevel = "Basic"


$myBlankDatabase = New-AzureRmSqlDatabase -DatabaseName $blankDatabaseName -ServerName $serverName -ResourceGroupName $resourceGroupName -Edition $blankDatabaseEdition -RequestedServiceObjectiveName $blankDatabaseServiceLevel
$myBlankDatabase
```



> [!TIP]
> Sie können Kosten sparen, während Sie lernen, indem Sie nicht verwendete Datenbanken löschen. Sie haben die Möglichkeit, Datenbanken der Basic Edition innerhalb von sieben Tagen wiederherzustellen. Vermeiden Sie es aber, einen Server zu löschen. Wenn Sie dies tun, ist es nicht möglich, den Server oder die gelöschten Datenbanken wiederherzustellen.

## <a name="helper-snippets"></a>Hilfscodeausschnitte

```
# Get a list of Azure regions where you can create SQL resources

$sqlRegions = (Get-AzureRmLocation | Where-Object { $_.Providers -eq "Microsoft.Sql" })
foreach ($region in $sqlRegions)
{
   $region.Location
}

# Clean up
# Delete a resource group (and all contained resources)
Remove-AzureRmResourceGroup -Name {resource-group-name}
```

> [!TIP]
> Sie können Kosten sparen, während Sie lernen, indem Sie nicht verwendete Datenbanken löschen. Sie haben die Möglichkeit, Datenbanken der Basic Edition innerhalb von sieben Tagen wiederherzustellen. Löschen Sie jedoch nicht den Server. Wenn Sie dies tun, ist es nicht möglich, den Server oder die gelöschten Datenbanken wiederherzustellen.
>

## <a name="next-steps"></a>Nächste Schritte
Nachdem Sie dieses Tutorial zu den ersten Schritten abgeschlossen und eine Datenbank mit einigen Beispieldaten erstellt haben, können Sie weitere Tutorials durcharbeiten, die auf den hier erworbenen Kenntnissen aufbauen. 

- Informationen zu den ersten Schritten im SQL Server-Authentifizierungstutorial finden Sie unter [SQL-Authentifizierung und -Autorisierung](sql-database-control-access-sql-authentication-get-started.md).
- Informationen zu den ersten Schritten im Azure Active Directory-Authentifizierungstutorial finden Sie unter [Azure AD-Authentifizierung und Autorisierung](sql-database-control-access-aad-authentication-get-started.md).
* Informationen zum Abfragen der Beispieldatenbank über das Azure-Portal finden Sie unter [Public preview: Interactive query experience for SQL databases](https://azure.microsoft.com/en-us/updates/azure-sql-database-public-preview-t-sql-editor/) (Öffentliche Vorschau: Interaktives Abfragen von SQL-Datenbanken).
* Wenn Sie mit Excel vertraut sind, informieren Sie sich über das [Herstellen von Verbindungen mit SQL-Datenbank in Azure mit Excel](sql-database-connect-excel.md).
* Wenn Sie mit dem Programmieren beginnen möchten, wählen Sie unter [Verbindungsbibliotheken für SQL-Datenbank und SQL Server](sql-database-libraries.md)Ihre Programmiersprache aus.
* Wenn Sie lokale SQL Server-Datenbanken in Azure verschieben möchten, helfen Ihnen die Informationen unter [Migrieren von Datenbanken zu SQL-Datenbank](sql-database-cloud-migrate.md) weiter.
* Wenn Sie einige Daten aus einer CSV-Datei mit dem BCP-Befehlszeilentool in eine neue Tabelle laden möchten, machen Sie sich mit den Informationen unter [Laden von Daten aus einer CSV-Datei in SQL-Datenbank mit BPC](sql-database-load-from-csv-with-bcp.md)vertraut.
* Wenn Sie mit dem Erstellen von Tabellen und anderen Objekten beginnen möchten, helfen Ihnen die Informationen unter „Erstellen einer Tabelle“ im Thema [Erstellen einer Tabelle](https://msdn.microsoft.com/library/ms365315.aspx) weiter.

## <a name="additional-resources"></a>Zusätzliche Ressourcen
[Was ist SQL Database?](sql-database-technical-overview.md)



<!--HONumber=Feb17_HO3-->


