---
title: Erstellen von Azure SQL-Datenbankservern | Microsoft-Dokumentation
description: "Kurzübersicht über das Erstellen von Azure SQL-Datenbankservern mithilfe von Azure-Portal und PowerShell."
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
ms.date: 02/01/2017
ms.author: carlrab
translationtype: Human Translation
ms.sourcegitcommit: 0a647294b41b7f9ce386b47cf0501a92486b80cc
ms.openlocfilehash: e8fe805da2446895b6616926fe1326dc30b9d0d0


---

# <a name="create-azure-sql-database-servers"></a>Erstellen von Azure SQL-Datenbankservern

Sie können einen Azure SQL-Datenbankserver mit dem [Azure-Portal](https://portal.azure.com/), PowerShell, der REST-API oder C# erstellen. 

## <a name="create-an-azure-sql-database-server-using-the-azure-portal"></a>Erstellen eines Azure SQL-Datenbankservers mithilfe des Azure-Portals

1. Öffnen Sie im [Azure-Portal](https://portal.azure.com/) das Blatt **SQL-Server**. 

    ![SQL-Server](./media/sql-database-get-started/new-sql-server.png)

2. Klicken Sie auf **hinzufügen**, um einen SQL-Server zu erstellen.

    ![Hinzufügen eines neuen SQL-Servers](./media/sql-database-get-started/new-sql-server-add.png)

> [!TIP]
> Ein Einführungstutorial für das Azure-Portal und SQL Server Management Studio finden Sie unter [Erste Schritte mit Azure SQL-Datenbankservern, -Servern, -Datenbanken und -Firewallregeln mit dem Azure-Portal und SQL Server Management Studio](sql-database-get-started.md).
>

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
> Ein Beispielskript finden Sie unter [Erstellen eines SQL-Datenbank-PowerShell-Skripts](sql-database-get-started-powershell.md).
>

## <a name="additional-resources"></a>Zusätzliche Ressourcen
* Eine Übersicht über Verwaltungstools finden Sie unter [Übersicht: Verwaltungstools für SQL-Datenbank](sql-database-manage-overview.md).
* Informationen zum Ausführen von Verwaltungsaufgaben mit dem Azure-Portal finden Sie unter [Verwalten von Azure SQL-Datenbanken über das Azure-Portal](sql-database-manage-portal.md).
* Informationen zum Ausführen von Verwaltungsaufgaben mithilfe von PowerShell finden Sie unter [Verwalten von Azure SQL-Datenbank mithilfe von PowerShell](sql-database-manage-powershell.md).
* Informationen zum Ausführen zusätzlicher Aufgaben mithilfe von SQL Server Management Studio finden Sie unter [Verwalten einer Azure SQL-Datenbank mit SQL Server Management Studio](sql-database-manage-azure-ssms.md).
* Informationen zum SQL-Datenbank-Dienst finden Sie unter [Was ist SQL Database? Einführung in SQL-Datenbank](sql-database-technical-overview.md). 
* Informationen zu Azure-Datenbankservern und Datenbankfeatures finden Sie unter [Funktionen von Azure SQL-Datenbank](sql-database-features.md).



<!--HONumber=Dec16_HO3-->


