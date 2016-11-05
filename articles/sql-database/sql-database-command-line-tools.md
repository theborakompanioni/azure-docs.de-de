---
title: Verwalten von Azure SQL-Datenbanken mit PowerShell | Microsoft Docs
description: Verwalten von Azure SQL-Datenbank mit PowerShell.
services: sql-database
documentationcenter: ''
author: stevestein
manager: jhubbard
editor: monicar

ms.service: sql-database
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/13/2016
ms.author: sstein

---
# Verwalten von Azure SQL-Datenbanken mit PowerShell
> [!div class="op_single_selector"]
> * [Azure-Portal](sql-database-manage-portal.md)
> * [Transact-SQL (SSMS)](sql-database-manage-azure-ssms.md)
> * [PowerShell](sql-database-command-line-tools.md)
> 
> 

In diesem Thema werden die PowerShell-Cmdlets vorgestellt, mit deren Hilfe zahlreiche Azure SQL-Datenbank-Aufgaben ausgeführt werden. Eine vollständige Liste finden Sie unter [Azure SQL-Datenbank-Cmdlets](https://msdn.microsoft.com/library/mt574084.aspx).

## Erstellen einer Ressourcengruppe
Erstellen Sie mit dem neuen Cmdlet [New-AzureRmResourceGroup](https://msdn.microsoft.com/library/azure/mt759837.aspx) eine Ressourcengruppe für Ihre SQL-Datenbank und die zugehörigen Azure-Ressourcen.

```
$resourceGroupName = "resourcegroup1"
$resourceGroupLocation = "northcentralus"
New-AzureRmResourceGroup -Name $resourceGroupName -Location $resourceGroupLocation
```

Weitere Informationen finden Sie unter [Verwenden von Azure PowerShell mit dem Azure-Ressourcen-Manager](../powershell-azure-resource-manager.md). Ein Beispielskript finden Sie unter [Erstellen eines SQL-Datenbank-PowerShell-Skripts](sql-database-get-started-powershell.md#create-a-sql-database-powershell-script).

## Erstellen eines SQL-Datenbank-Servers
Erstellen Sie mit dem Cmdlet [New-AzureRmSqlServer](https://msdn.microsoft.com/library/azure/mt603715.aspx) einen SQL-Datenbank-Server. Ersetzen Sie *server1* durch den Namen Ihres Servers. Servernamen müssen für alle Azure SQL-Datenbank-Server eindeutig sein. Es wird eine Fehlermeldung angezeigt, wenn der Servername bereits vergeben ist. Dieser Befehl kann mehrere Minuten in Anspruch nehmen. Die Ressourcengruppe muss bereits in Ihrem Abonnement vorhanden sein.

```
$resourceGroupName = "resourcegroup1"

$sqlServerName = "server1"
$sqlServerVersion = "12.0"
$sqlServerLocation = "northcentralus"
$serverAdmin = "loginname"
$serverPassword = "password" 
$securePassword = ConvertTo-SecureString –String $serverPassword –AsPlainText -Force
$creds = New-Object –TypeName System.Management.Automation.PSCredential –ArgumentList $serverAdmin, $securePassword


$sqlServer = New-AzureRmSqlServer -ServerName $sqlServerName `
 -SqlAdministratorCredentials $creds -Location $sqlServerLocation `
 -ResourceGroupName $resourceGroupName -ServerVersion $sqlServerVersion
```

Weitere Informationen stehen unter [Was ist SQL-Datenbank?](sql-database-technical-overview.md). Ein Beispielskript finden Sie unter [Erstellen eines SQL-Datenbank-PowerShell-Skripts](sql-database-get-started-powershell.md#create-a-sql-database-powershell-script).

## Erstellen einer Firewallregel für einen Azure SQL-Datenbank-Server
Um eine Firewallregel für den Zugriff auf den Server zu erstellen, verwenden Sie das Cmdlet [New-AzureRmSqlServerFirewallRule](https://msdn.microsoft.com/library/azure/mt603860.aspx). Führen Sie den folgenden Befehl aus, wobei Sie die Start- und End-IP-Adresse durch gültige Werte für Ihren Client ersetzen. Die Ressourcengruppe und der Server müssen bereits in Ihrem Abonnement vorhanden sein.

```
$resourceGroupName = "resourcegroup1"
$sqlServerName = "server1"

$firewallRuleName = "firewallrule1"
$firewallStartIp = "0.0.0.0"
$firewallEndIp = "255.255.255.255"

New-AzureRmSqlServerFirewallRule -ResourceGroupName $resourceGroupName `
 -ServerName $sqlServerName -FirewallRuleName $firewallRuleName `
 -StartIpAddress $firewallStartIp -EndIpAddress $firewallEndIp
```

Um anderen Azure-Diensten den Zugriff auf Ihren Server zu ermöglichen, erstellen Sie eine Firewallregel, und legen Sie sowohl `-StartIpAddress` und `-EndIpAddress` auf **0.0.0.0** fest. Mit dieser speziellen Firewallregel wird dem gesamten Azure-Datenverkehr der Zugriff auf den Server gestattet.

Weitere Informationen finden Sie unter [Firewall für die Azure SQL-Datenbank](https://msdn.microsoft.com/library/azure/ee621782.aspx). Ein Beispielskript finden Sie unter [Erstellen eines SQL-Datenbank-PowerShell-Skripts](sql-database-get-started-powershell.md#create-a-sql-database-powershell-script).

## Erstellen einer (leeren) SQL-Datenbank
Erstellen Sie mit dem Cmdlet [New-AzureRmSqlDatabase](https://msdn.microsoft.com/library/azure/mt619339.aspx) eine Datenbank. Die Ressourcengruppe und der Server müssen bereits in Ihrem Abonnement vorhanden sein.

```
$resourceGroupName = "resourcegroup1"
$sqlServerName = "server1"

$databaseName = "database1"
$databaseEdition = "Standard"
$databaseServiceLevel = "S0"

$currentDatabase = New-AzureRmSqlDatabase -ResourceGroupName $resourceGroupName `
 -ServerName $sqlServerName -DatabaseName $databaseName `
 -Edition $databaseEdition -RequestedServiceObjectiveName $databaseServiceLevel
```

Weitere Informationen stehen unter [Was ist SQL-Datenbank?](sql-database-technical-overview.md). Ein Beispielskript finden Sie unter [Erstellen eines SQL-Datenbank-PowerShell-Skripts](sql-database-get-started-powershell.md#create-a-sql-database-powershell-script).

## Ändern der Leistungsebene einer SQL-Datenbank
Sie können Ihre Datenbank mit dem Cmdlet [Set-AzureRmSqlDatabase](https://msdn.microsoft.com/library/azure/mt619433.aspx) hoch- oder herunterskalieren. Die Ressourcengruppe, der Server und die Datenbank müssen bereits in Ihrem Abonnement vorhanden sein. Legen Sie für den Tarif „Basic“ `-RequestedServiceObjectiveName` (wie im folgenden Codeausschnitt) auf ein einzelnes Leerzeichen fest. Legen Sie diese Einstellung für andere Tarife wie im vorangehenden Beispiel auf *S0*, *S1*, *P1*, *P6* usw. fest.

```
$resourceGroupName = "resourcegroup1"
$sqlServerName = "server1"

$databaseName = "database1"
$databaseEdition = "Basic"
$databaseServiceLevel = " "

Set-AzureRmSqlDatabase -ResourceGroupName $resourceGroupName `
 -ServerName $sqlServerName -DatabaseName $databaseName `
 -Edition $databaseEdition -RequestedServiceObjectiveName $databaseServiceLevel
```

Weitere Informationen finden Sie unter [SQL-Datenbankoptionen und -leistung: Grundlegendes zum Angebot in den einzelnen Tarifen](sql-database-service-tiers.md). Ein Beispielskript finden Sie unter [PowerShell-Beispielskript zum Ändern der Dienstebene und Leistungsstufe Ihrer SQL-Datenbank](sql-database-scale-up-powershell.md#sample-powershell-script-to-change-the-service-tier-and-performance-level-of-your-sql-database).

## Kopieren einer SQL-Datenbank auf denselben Server
Mit dem Cmdlet [New-AzureRmSqlDatabaseCopy](https://msdn.microsoft.com/library/azure/mt603644.aspx) können Sie eine SQL-Datenbank auf denselben Server kopieren. Legen Sie `-CopyServerName` und `-CopyResourceGroupName` auf die gleichen Werte wie für den Quell-Datenbankserver und die Quellressourcengruppe fest.

```
$resourceGroupName = "resourcegroup1"
$sqlServerName = "server1"
$databaseName = "database1"

$copyDatabaseName = "database1_copy"
$copyServerName = $sqlServerName
$copyResourceGroupName = $resourceGroupName

New-AzureRmSqlDatabaseCopy -DatabaseName $databaseName `
 -ServerName $sqlServerName -ResourceGroupName $resourceGroupName `
 -CopyDatabaseName $copyDatabaseName -CopyServerName $sqlServerName `
 -CopyResourceGroupName $copyResourceGroupName
```

Weitere Informationen finden Sie unter [Kopieren einer Azure SQL-Datenbank](sql-database-copy.md). Ein Beispielskript finden Sie unter [Kopieren eines SQL-Datenbank-PowerShell-Skripts](sql-database-copy-powershell.md#example-powershell-script).

## Löschen einer SQL-Datenbank
Sie können eine SQL-Datenbank mit dem Cmdlet [Remove-AzureRmSqlDatabase](https://msdn.microsoft.com/library/azure/mt619368.aspx) löschen. Die Ressourcengruppe, der Server und die Datenbank müssen bereits in Ihrem Abonnement vorhanden sein.

```
$resourceGroupName = "resourcegroup1"
$sqlServerName = "server1"
$databaseName = "database1"

Remove-AzureRmSqlDatabase -DatabaseName $databaseName `
 -ServerName $sqlServerName -ResourceGroupName $resourceGroupName
```

## Löschen eines SQL-Datenbank-Servers
Mit dem Befehl [Remove-AzureRmSqlServer](https://msdn.microsoft.com/library/azure/mt603488.aspx) können Sie einen Server löschen.

```
$resourceGroupName = "resourcegroup1"
$sqlServerName = "server1"

Remove-AzureRmSqlServer -ServerName $sqlServerName -ResourceGroupName $resourceGroupName
```

## Erstellen und Verwalten von Pools für elastische Datenbanken mithilfe von PowerShell
Einzelheiten zum Erstellen von Pools für elastische Datenbanken mit PowerShell finden Sie unter [Erstellen eines neuen Pools für elastische Datenbanken mit PowerShell](sql-database-elastic-pool-create-powershell.md).

Einzelheiten zum Verwalten von Pools für elastische Datenbanken mit PowerShell finden Sie unter [Überwachen und Verwalten eines Pools für elastische Datenbanken mit PowerShell](sql-database-elastic-pool-manage-powershell.md).

## Verwandte Informationen
* [Azure SQL-Datenbank-Cmdlets](https://msdn.microsoft.com/library/azure/mt574084.aspx)
* [Azure-Cmdlet-Referenz](https://msdn.microsoft.com/library/azure/dn708514.aspx)

<!---HONumber=AcomDC_0914_2016-->