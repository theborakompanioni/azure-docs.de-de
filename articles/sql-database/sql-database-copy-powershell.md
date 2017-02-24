---
title: Kopieren einer Azure SQL-Datenbank mithilfe von PowerShell | Microsoft Docs
description: Erstellen der Kopie einer Azure SQL-Datenbank mithilfe von PowerShell
services: sql-database
documentationcenter: 
author: stevestein
manager: jhubbard
editor: 
ms.assetid: 6d9839d7-9303-48d2-be0f-21ce84f95a94
ms.service: sql-database
ms.custom: migrate and move
ms.devlang: NA
ms.date: 09/08/2016
ms.author: sstein
ms.workload: data-management
ms.topic: article
ms.tgt_pltfrm: NA
translationtype: Human Translation
ms.sourcegitcommit: edfbc0d94d9d5b33a25788772a98173187e394b8
ms.openlocfilehash: 031a17d179cee8d6cceb9ed3aefd993add1958b8


---
# <a name="copy-an-azure-sql-database-using-powershell"></a>Kopieren einer Azure SQL-Datenbank mithilfe von PowerShell

Dieser Artikel zeigt, wie Sie eine SQL-Datenbank mit PowerShell auf den gleichen oder einen anderen Server kopieren und wie Sie eine Datenbank in einen [Pool für elastische Datenbanken](sql-database-elastic-pool.md) kopieren. Zum Kopieren der Datenbank wird das Cmdlet [New-AzureRmSqlDatabaseCopy](https://msdn.microsoft.com/library/mt603644\(v=azure.300\).aspx) verwendet. 

> [!NOTE]
> Sie können eine SQL-Datenbank auch über das [Azure-Portal](sql-database-copy-portal.md) oder [Transact-SQL](sql-database-copy-transact-sql.md) kopieren.
>

Damit Sie die Anweisungen in diesem Artikel ausführen können, benötigen Sie Folgendes:

* Azure SQL-Datenbank (eine zu kopierende Datenbank). Wenn Sie nicht über eine SQL-Datenbank verfügen, können Sie die Erstellung anhand der Schritte im folgenden Artikel durchführen: [Erstellen der ersten Azure SQL-Datenbank](sql-database-get-started.md).
* Die aktuelle Version von Azure PowerShell. Weitere Informationen finden Sie unter [Installieren und Konfigurieren von Azure PowerShell](/powershell/azureps-cmdlets-docs).

Viele neue Funktionen von SQL-Datenbank werden nur bei Verwendung des [Azure Resource Manager-Bereitstellungsmodells](../azure-resource-manager/resource-group-overview.md) unterstützt. Daher werden in den Beispielen die [Azure SQL-Datenbank-PowerShell-Cmdlets](https://msdn.microsoft.com/library/azure/mt574084\(v=azure.300\).aspx) für Resource Manager verwendet. Die vorhandenen [(klassischen) Azure SQL-Datenbank-Cmdlets](https://msdn.microsoft.com/library/azure/dn546723\(v=azure.300\).aspx) des klassischen Bereitstellungsmodells werden zur Abwärtskompatibilität zwar unterstützt, dennoch wird die Verwendung der Resource Manager-Cmdlets empfohlen.

> [!NOTE]
> Je nach Größe Ihrer Datenbank kann es einige Zeit dauern, bis der Kopiervorgang abgeschlossen ist.
> 
> 

## <a name="copy-a-sql-database-to-the-same-server"></a>Kopieren einer SQL-Datenbank auf denselben Server
Zum Erstellen der Kopie auf dem gleichen Server lassen Sie den `-CopyServerName` -Parameter weg (oder legen ihn auf den gleichen Server fest).

    New-AzureRmSqlDatabaseCopy -ResourceGroupName "resourcegroup1" -ServerName "server1" -DatabaseName "database1" -CopyDatabaseName "database1_copy"

## <a name="copy-a-sql-database-to-a-different-server"></a>Kopieren einer SQL-Datenbank auf einen anderen Server
Um die Kopie auf einem anderen Server zu erstellen, geben den `-CopyServerName` -Parameter an und legen ihn auf einen anderen Server fest. Der Server, *auf den kopiert wird* , muss bereits vorhanden sein. Wenn er sich in einer anderen Ressourcengruppe befindet, müssen Sie auch den `-CopyResourceGroupName` -Parameter hinzufügen.

    New-AzureRmSqlDatabaseCopy -ResourceGroupName "resourcegroup1" -ServerName "server1" -DatabaseName "database1" -CopyServerName "server2" -CopyDatabaseName "database1_copy"


## <a name="copy-a-sql-database-into-an-elastic-pool"></a>Kopieren einer SQL-Datenbank in einen Pool für elastische Datenbanken
Um eine Kopie einer SQL-Datenbank in einem Pool zu erstellen, legen den `-ElasticPoolName` -Parameter auf einen vorhandenen Pool fest.

    New-AzureRmSqlDatabaseCopy -ResourceGroupName "resourcegoup1" -ServerName "server1" -DatabaseName "database1" -CopyResourceGroupName "poolResourceGroup" -CopyServerName "poolServer1" -CopyDatabaseName "database1_copy" -ElasticPoolName "poolName"


## <a name="resolve-logins"></a>Auflösen von Anmeldungen
Informationen zum Auflösen von Anmeldungen, nachdem der Kopiervorgang abgeschlossen wurde, finden Sie unter [Auflösen von Anmeldungen](sql-database-copy-transact-sql.md#resolve-logins-after-the-copy-operation-completes)

## <a name="example-powershell-script"></a>PowerShell-Beispielskript
Das folgende Skript setzt voraus, dass alle Ressourcengruppen, Server und der Pool bereits vorhanden sind (ersetzen Sie die Variablenwerte durch Ihre vorhandenen Ressourcen). Mit Ausnahme der Datenbankkopie muss alles vorhanden sein.

    # Sign in to Azure and set the subscription to work with
    # ------------------------------------------------------
    $SubscriptionId = "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
    Add-AzureRmAccount
    Set-AzureRmContext -SubscriptionId $SubscriptionId


    # SQL database source (the existing database to copy)
    # ---------------------------------------------------
    $sourceDbName = "db1"
    $sourceDbServerName = "server1"
    $sourceDbResourceGroupName = "rg1"

    # SQL database copy (the new db to be created)
    # --------------------------------------------
    $copyDbName = "db1_copy"
    $copyDbServerName = "server2"
    $copyDbResourceGroupName = "rg2"

    # Copy a database to the same server
    # ----------------------------------
    New-AzureRmSqlDatabaseCopy -ResourceGroupName $sourceDbResourceGroupName -ServerName $sourceDbServerName -DatabaseName $sourceDbName -CopyDatabaseName $copyDbName

    # Copy a database to a different server
    # -------------------------------------
    New-AzureRmSqlDatabaseCopy -ResourceGroupName $sourceDbResourceGroupName -ServerName $sourceDbServerName -DatabaseName $sourceDbName -CopyResourceGroupName $copyDbResourceGroupName -CopyServerName $copyDbServerName -CopyDatabaseName $copyDbName

    # Copy a database into an elastic pool
    # ---------------------------------------------
    $poolName = "pool1"

    New-AzureRmSqlDatabaseCopy -ResourceGroupName $sourceDbResourceGroupName -ServerName $sourceDbServerName -DatabaseName $sourceDbName -CopyResourceGroupName $copyDbResourceGroupName -CopyServerName $copyDbServerName -ElasticPoolName $poolName -CopyDatabaseName $copyDbName





## <a name="next-steps"></a>Nächste Schritte
* Informationen zum Verwalten von Benutzern und Anmeldungen beim Kopieren einer Datenbank auf einen anderen logischen Server finden Sie unter [Verwalten der Sicherheit der Azure SQL-Datenbank nach der Notfallwiederherstellung](sql-database-geo-replication-security-config.md).
* Informationen zum Exportieren einer Datenbank in eine BACPAC-Datei über PowerShell finden Sie unter [Exportieren der Datenbank in eine BACPAC-Datei über PowerShell](sql-database-export-powershell.md).
* [Übersicht über die Geschäftskontinuität](sql-database-business-continuity.md)
* [SQL-Datenbank-Dokumentation](https://azure.microsoft.com/documentation/services/sql-database/)

## <a name="additional-resources"></a>Zusätzliche Ressourcen
* [New-AzureRmSqlDatabase](https://msdn.microsoft.com/library/mt603644\(v=azure.300\).aspx)
* [Get-AzureRmSqlDatabaseActivity](https://msdn.microsoft.com/library/mt603687\(v=azure.300\).aspx)
* [Verwalten von Anmeldungen](sql-database-manage-logins.md)
* [Herstellen einer Verbindung mit einer Azure SQL-Datenbank mit SQL Server Management Studio und Ausführen einer T-SQL-Beispielabfrage](sql-database-connect-query-ssms.md)
* [Exportieren der Datenbank in eine BACPAC-Datei](sql-database-export.md)
* [Übersicht über die Geschäftskontinuität](sql-database-business-continuity.md)
* [SQL-Datenbankdokumentation](https://azure.microsoft.com/documentation/services/sql-database/)
* [Azure SQL-Datenbank: PowerShell-Cmdlet-Referenz](https://msdn.microsoft.com/library/mt574084\(v=azure.300\).aspx)




<!--HONumber=Feb17_HO2-->


