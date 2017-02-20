---
title: 'PowerShell: Erstellen und Verwalten einzelner Azure SQL-Datenbanken | Microsoft-Dokumentation'
description: "Kurzübersicht über das Erstellen und Verwalten einzelner Azure SQL-Datenbanken mit dem Azure-Portal"
services: sql-database
documentationcenter: 
author: CarlRabeler
manager: jhubbard
editor: 
ms.assetid: 
ms.service: sql-database
ms.custom: single databases
ms.devlang: NA
ms.workload: data-management
ms.topic: article
ms.tgt_pltfrm: NA
ms.date: 02/06/2017
ms.author: carlrab
translationtype: Human Translation
ms.sourcegitcommit: c54e6541dc3694f23237945e22021d5f90a2687d
ms.openlocfilehash: 1d55c5c7fe99a9a744f69bad38faf3ce01d2227f


---
# <a name="create-and-manage-single-azure-sql-databases-with-powershell"></a>Erstellen und Verwalten einzelner Azure SQL-Datenbanken mit PowerShell

Sie können einzelne Azure SQL-Datenbanken mit dem [Azure-Portal](https://portal.azure.com/), mit PowerShell, Transact-SQL, der REST-API oder C# erstellen und verwalten. In diesem Thema wird die Verwendung von PowerShell erläutert. Informationen zur Verwendung des Azure-Portals finden Sie unter [Erstellen und Verwalten einzelner Azure SQL-Datenbanken mit dem Azure-Portal](sql-database-manage-single-databases-powershell.md). Informationen zur Verwendung von Transact-SQL finden Sie unter [Erstellen und Verwalten einzelner Azure SQL-Datenbanken mit Transact-SQL](sql-database-manage-single-databases-tsql.md). 

[!INCLUDE [Start your PowerShell session](../../includes/sql-database-powershell.md)]

## <a name="create-an-azure-sql-database-using-powershell"></a>Erstellen einer Azure SQL-Datenbank mithilfe von PowerShell

Um eine SQL-Datenbank zu erstellen, verwenden Sie das Cmdlet [New-AzureRmSqlDatabase](https://docs.microsoft.com/powershell/resourcemanager/azurerm.sql/v2.3.0/new-azurermsqldatabase). Die Ressourcengruppe und der Server müssen bereits in Ihrem Abonnement vorhanden sein. 

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

> [!TIP]
> Ein Beispielskript finden Sie unter [Erstellen eines SQL-Datenbank-PowerShell-Skripts](sql-database-get-started-powershell.md).
>

## <a name="change-the-service-tier-and-performance-level-of-a-single-database"></a>Ändern der Dienstebene und Leistungsstufe einer Einzeldatenbank

Führen Sie das Cmdlet [Set-AzureRmSqlDatabase](https://msdn.microsoft.com/library/azure/mt619433\(v=azure.300\).aspx) aus, und legen Sie **-RequestedServiceObjectiveName** auf die Leistungsstufe des gewünschten Tarifs fest, z.B. *S0*, *S1*, *S2*, *S3*, *P1*, *P2*...

Ersetzen Sie ```{variables}``` durch Ihre Werte (ohne geschweifte Klammern).

```
$SubscriptionId = "{4cac86b0-1e56-bbbb-aaaa-000000000000}"

$ResourceGroupName = "{resourceGroup}"
$Location = "{AzureRegion}"

$ServerName = "{server}"
$DatabaseName = "{database}"

$NewEdition = "{Standard}"
$NewPricingTier = "{S2}"

Add-AzureRmAccount
Set-AzureRmContext -SubscriptionId $SubscriptionId

Set-AzureRmSqlDatabase -DatabaseName $DatabaseName -ServerName $ServerName -ResourceGroupName $ResourceGroupName -Edition $NewEdition -RequestedServiceObjectiveName $NewPricingTier
```

## <a name="next-steps"></a>Nächste Schritte
* Eine Übersicht über Verwaltungstools finden Sie unter [Übersicht: Verwaltungstools für SQL-Datenbank](sql-database-manage-overview.md).
* Informationen zum Ausführen von Verwaltungsaufgaben mit dem Azure-Portal finden Sie unter [Verwalten von Azure SQL-Datenbanken über das Azure-Portal](sql-database-manage-portal.md).
* Informationen zum Ausführen von Verwaltungsaufgaben mithilfe von PowerShell finden Sie unter [Verwalten von Azure SQL-Datenbank mithilfe von PowerShell](sql-database-manage-powershell.md).
* Informationen zum Ausführen von Verwaltungsaufgaben mithilfe von SQL Server Management Studio finden Sie unter [Verwalten einer Azure SQL-Datenbank mit SQL Server Management Studio](sql-database-manage-azure-ssms.md).
* Informationen zum SQL-Datenbank-Dienst finden Sie unter [Was ist SQL Database? Einführung in SQL-Datenbank](sql-database-technical-overview.md). 
* Informationen zu Azure-Datenbankservern und Datenbankfeatures finden Sie unter [Funktionen von Azure SQL-Datenbank](sql-database-features.md).



<!--HONumber=Feb17_HO2-->


