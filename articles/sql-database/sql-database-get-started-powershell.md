---
title: 'Azure PowerShell: Erstellen einer SQL-Datenbank | Microsoft-Dokumentation'
description: Erfahren Sie, wie Sie einen logischen SQL-Datenbankserver, eine Firewallregel auf Serverebene und Datenbanken mit dem Azure-Portal erstellen.
keywords: Tutorial zu SQL-Datenbank, Erstellen einer SQL-Datenbank
services: sql-database
documentationcenter: 
author: CarlRabeler
manager: jhubbard
editor: 
ms.assetid: 
ms.service: sql-database
ms.custom: mvc,DBs & servers
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: PowerShell
ms.topic: quickstart
ms.date: 04/17/2017
ms.author: carlrab
ms.translationtype: Human Translation
ms.sourcegitcommit: c785ad8dbfa427d69501f5f142ef40a2d3530f9e
ms.openlocfilehash: d1acc548dca4c89572eece8dbdae0eae4853a97c
ms.contentlocale: de-de
ms.lasthandoff: 05/26/2017

---

# <a name="create-a-single-azure-sql-database-using-powershell"></a>Erstellen einer einzelnen Azure SQL-Datenbank mithilfe von PowerShell

PowerShell dient zum Erstellen und Verwalten von Azure-Ressourcen über die Befehlszeile oder mit Skripts. In dieser Anleitung wird die Verwendung von PowerShell zum Bereitstellen einer Azure SQL-Datenbank in einer [Azure-Ressourcengruppe](../azure-resource-manager/resource-group-overview.md) auf einem [logischen SQL-Datenbankserver](sql-database-features.md) ausführlich beschrieben.

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/) erstellen, bevor Sie beginnen.

Für dieses Tutorial ist das Azure PowerShell-Modul Version 4.0 oder höher erforderlich. Führen Sie ` Get-Module -ListAvailable AzureRM` aus, um die Version zu finden. Wenn Sie eine Installation oder ein Upgrade ausführen müssen, finden Sie unter [Install and configure Azure PowerShell](/powershell/azure/install-azurerm-ps) (Installieren des Azure PowerShell-Moduls) Informationen dazu. 

## <a name="log-in-to-azure"></a>Anmelden an Azure

Melden Sie sich mit dem Befehl [Add-AzureRmAccount](/powershell/module/azurerm.profile/add-azurermaccount) an Ihrem Azure-Abonnement an, und befolgen Sie die Anweisungen auf dem Bildschirm.

```powershell
Add-AzureRmAccount
```

## <a name="create-variables"></a>Erstellen von Variablen

Definieren Sie Variablen zur Verwendung in den Skripts dieses Schnellstarts.

```powershell
# The data center and resource name for your resources
$resourcegroupname = "myResourceGroup"
$location = "WestEurope"
# The logical server name: Use a random value or replace with your own value (do not capitalize)
$servername = "server-$(Get-Random)"
# Set an admin login and password for your database
# The login information for the server
$adminlogin = "ServerAdmin"
$password = "ChangeYourAdminPassword1"
# The ip address range that you want to allow to access your server - change as appropriate
$startip = "0.0.0.0"
$endip = "0.0.0.0"
# The database name
$databasename = "mySampleDatabase"
```

## <a name="create-a-resource-group"></a>Erstellen einer Ressourcengruppe

Erstellen Sie mit dem Befehl [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup) eine [Azure-Ressourcengruppe](../azure-resource-manager/resource-group-overview.md). Eine Ressourcengruppe ist ein logischer Container, in dem Azure-Ressourcen bereitgestellt und als Gruppe verwaltet werden. Im folgenden Beispiel wird eine Ressourcengruppe mit dem Namen `myResourceGroup` am Standort `westeurope` erstellt.

```powershell
New-AzureRmResourceGroup -Name $resourcegroupname -Location $location
```
## <a name="create-a-logical-server"></a>Erstellen eines logischen Servers

Erstellen Sie mit dem Befehl [New-AzureRmSqlServer](/powershell/module/azurerm.sql/new-azurermsqlserver) einen [logischen Azure SQL-Datenbankserver](sql-database-features.md). Ein logischer Server enthält eine Gruppe von Datenbanken, die als Gruppe verwaltet werden. Im folgenden Beispiel wird in Ihrer Ressourcengruppe ein zufällig benannter Server mit einem Administrator namens `ServerAdmin` und dem Kennwort `ChangeYourAdminPassword1` erstellt. Ersetzen Sie ggf. diese vordefinierten Werte.

```powershell
New-AzureRmSqlServer -ResourceGroupName $resourcegroupname `
    -ServerName $servername `
    -Location $location `
    -SqlAdministratorCredentials $(New-Object -TypeName System.Management.Automation.PSCredential -ArgumentList $adminlogin, $(ConvertTo-SecureString -String $password -AsPlainText -Force))
```

## <a name="configure-a-server-firewall-rule"></a>Konfigurieren einer Serverfirewallregel

Erstellen Sie mit dem Befehl [New-AzureRmSqlServerFirewallRule](/powershell/module/azurerm.sql/new-azurermsqlserverfirewallrule) eine [Firewallregel für Azure SQL-Datenbank auf Serverebene](sql-database-firewall-configure.md). Eine Firewallregel auf Serverebene lässt zu, dass eine externe Anwendung wie SQL Server Management Studio oder das SQLCMD-Hilfsprogramm über die Firewall des SQL-Datenbank-Dienstes eine Verbindung mit einer SQL-Datenbank herstellt. Im folgenden Beispiel wird die Firewall nur für andere Azure-Ressourcen geöffnet. Ändern Sie die IP-Adresse in eine für Ihre Umgebung geeignete Adresse, um die externe Konnektivität zu ermöglichen. Verwenden Sie 0.0.0.0 als IP-Startadresse und 255.255.255.255 als Endadresse, wenn Sie alle IP-Adressen öffnen möchten.

```powershell
New-AzureRmSqlServerFirewallRule -ResourceGroupName $resourcegroupname `
    -ServerName $servername `
    -FirewallRuleName "AllowSome" -StartIpAddress $startip -EndIpAddress $endip
```

> [!NOTE]
> SQL-Datenbank kommuniziert über Port 1433. Wenn Sie versuchen, eine Verbindung aus einem Unternehmensnetzwerk heraus herzustellen, wird der ausgehende Datenverkehr über Port 1433 von der Firewall Ihres Netzwerks unter Umständen nicht zugelassen. In diesem Fall können Sie nur dann eine Verbindung mit Ihrem Azure SQL-Datenbankserver herstellen, wenn Ihre IT-Abteilung Port 1433 öffnet.
>

## <a name="create-a-database-in-the-server-with-sample-data"></a>Erstellen einer Datenbank auf dem Server mit Beispieldaten

Erstellen Sie auf dem Server mit dem Befehl [New-AzureRmSqlDatabase](/powershell/module/azurerm.sql/new-azurermsqldatabase) eine Datenbank mit der [Leistungsstufe „S0“](sql-database-service-tiers.md). Im folgenden Beispiel wird eine Datenbank mit dem Namen `mySampleDatabase` erstellt, und die AdventureWorksLT-Beispieldaten werden in diese Datenbank geladen. Ersetzen Sie diese vordefinierten Werte wie gewünscht (andere Schnellstarts in dieser Sammlung bauen auf den Werten dieses Schnellstarts auf).

```powershell
New-AzureRmSqlDatabase  -ResourceGroupName $resourcegroupname `
    -ServerName $servername `
    -DatabaseName $databasename `
    -SampleName "AdventureWorksLT" `
    -RequestedServiceObjectiveName "S0"
```

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Andere Schnellstarts dieser Sammlung bauen auf diesem Schnellstart auf. 

> [!TIP]
> Wenn Sie planen, mit den nachfolgenden Schnellstarts fortzufahren, sollten Sie die in diesem Schnellstart erstellten Ressourcen nicht bereinigen. Falls Sie nicht fortfahren möchten, können Sie die folgenden Schritte ausführen, um alle erstellten Ressourcen dieses Schnellstarts im Azure-Portal zu löschen.
>

```powershell
Remove-AzureRmResourceGroup -ResourceGroupName $resourcegroupname
```

## <a name="next-steps"></a>Nächste Schritte

Sie besitzen nun eine Datenbank, können eine Verbindung herstellen und Abfragen mit Ihren bevorzugten Tools ausführen. Weitere Informationen erhalten Sie, indem Sie eines der folgenden Tools auswählen:

- [SQL Server Management Studio](sql-database-connect-query-ssms.md)
- [Visual Studio Code](sql-database-connect-query-vscode.md)
- [.NET](sql-database-connect-query-dotnet.md)
- [PHP](sql-database-connect-query-php.md)
- [Node.js](sql-database-connect-query-nodejs.md)
- [Java](sql-database-connect-query-java.md)
- [Python](sql-database-connect-query-python.md)
- [Ruby](sql-database-connect-query-ruby.md)


