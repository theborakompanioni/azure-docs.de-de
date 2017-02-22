---
title: 'PowerShell: Erstellen und Verwalten von Azure SQL-Datenbankservern | Microsoft-Dokumentation'
description: "Kurzübersicht über das Erstellen und Verwalten von Azure SQL-Datenbankservern mithilfe von PowerShell."
services: sql-database
documentationcenter: 
author: CarlRabeler
manager: jhubbard
editor: 
ms.assetid: 
ms.service: sql-database
ms.custom: servers
ms.devlang: NA
ms.workload: data-management
ms.topic: article
ms.tgt_pltfrm: NA
ms.date: 02/06/2017
ms.author: carlrab
translationtype: Human Translation
ms.sourcegitcommit: 144774c9106bf5a0e389c99075c822d1c5282692
ms.openlocfilehash: c22243db06c783bc4f5b311aa4d49ce12032ab02


---
 
# <a name="create-and-manage-azure-sql-database-servers-with-powershell"></a>Erstellen und Verwalten von Azure SQL-Datenbankservern mit PowerShell

Sie können einen Azure SQL-Datenbankserver mit dem [Azure-Portal](https://portal.azure.com/), PowerShell, der REST-API oder C# erstellen und verwalten. In diesem Thema wird die Verwendung von PowerShell erläutert. Informationen zur Verwendung des Azure-Portals finden Sie unter [Create and manage Azure SQL Database servers with the Azure portal](sql-database-manage-servers-portal.md) (Erstellen und Verwalten von Azure SQL-Datenbankservern mit dem Azure-Portal). 

## <a name="create-an-azure-sql-database-server-using-powershell"></a>Erstellen eines Azure SQL-Datenbankservers mithilfe von PowerShell

Verwenden Sie das Cmdlet [New-AzureRmSqlServer](https://docs.microsoft.com/powershell/resourcemanager/azurerm.sql/v2.3.0/new-azurermsqlserver), um einen SQL-Datenbank-Server zu erstellen. Ersetzen Sie *server1* durch den Namen Ihres Servers. Servernamen müssen auf allen Azure SQL-Datenbankservern eindeutig sein. Es wird eine Fehlermeldung angezeigt, wenn der Servername bereits vergeben ist. Dieser Befehl kann mehrere Minuten in Anspruch nehmen. Die Ressourcengruppe muss bereits in Ihrem Abonnement vorhanden sein.

```
$resourceGroupName = "resourcegroup1"

$sqlServerName = "server1"
$sqlServerVersion = "12.0"
$sqlServerLocation = "northcentralus"
$serverAdmin = "loginname"
$serverPassword = "password" 
$securePassword = ConvertTo-SecureString -String $serverPassword -AsPlainText -Force
$creds = New-Object -TypeName System.Management.Automation.PSCredential -ArgumentList $serverAdmin, $securePassword


$sqlServer = New-AzureRmSqlServer -ServerName $sqlServerName `
 -SqlAdministratorCredentials $creds -Location $sqlServerLocation `
 -ResourceGroupName $resourceGroupName -ServerVersion $sqlServerVersion
```

> [!TIP]
> Ein Beispielskript finden Sie unter [SQL-Datenbank-Tutorial: Erste Schritte mit Azure SQL-Datenbank-Servern, -Datenbanken und -Firewallregeln mit PowerShell](sql-database-get-started-powershell.md).
>

## <a name="next-steps"></a>Nächste Schritte
* Eine Übersicht über Verwaltungstools finden Sie unter [Übersicht: Verwaltungstools für SQL-Datenbank](sql-database-manage-overview.md).
* Informationen zum Ausführen von Verwaltungsaufgaben mit dem Azure-Portal finden Sie unter [Verwalten von Azure SQL-Datenbanken über das Azure-Portal](sql-database-manage-portal.md).
* Informationen zum Ausführen von Verwaltungsaufgaben mithilfe von PowerShell finden Sie unter [Verwalten von Azure SQL-Datenbank mithilfe von PowerShell](sql-database-manage-powershell.md).
* Informationen zum Ausführen zusätzlicher Aufgaben mithilfe von SQL Server Management Studio finden Sie unter [Verwalten einer Azure SQL-Datenbank mit SQL Server Management Studio](sql-database-manage-azure-ssms.md).
* Informationen zum SQL-Datenbank-Dienst finden Sie unter [Was ist SQL Database? Einführung in SQL-Datenbank](sql-database-technical-overview.md). 
* Informationen zu Azure-Datenbankservern und Datenbankfeatures finden Sie unter [Funktionen von Azure SQL-Datenbank](sql-database-features.md).



<!--HONumber=Feb17_HO3-->


