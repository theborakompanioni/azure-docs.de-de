---
title: Verwalten von Computeleistung in Azure SQL Data Warehouse (PowerShell) | Microsoft Docs
description: "PowerShell-Aufgaben zum Verwalten von Computeleistung. Skalieren von Computeressourcen durch Anpassen der DWUs Sie können Computeressourcen auch nach Bedarf anhalten und fortsetzen, um Kosten zu sparen."
services: sql-data-warehouse
documentationcenter: NA
author: barbkess
manager: jhubbard
editor: 
ms.assetid: 8354a3c1-4e04-4809-933f-db414a8c74dc
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: manage
ms.date: 10/31/2016
ms.author: barbkess
translationtype: Human Translation
ms.sourcegitcommit: 07635b0eb4650f0c30898ea1600697dacb33477c
ms.openlocfilehash: b2b87ed07a26fa30e49a19e34ca8b06b283da9b3
ms.lasthandoff: 03/28/2017


---
# <a name="manage-compute-power-in-azure-sql-data-warehouse-powershell"></a>Verwalten von Computeleistung für Azure SQL Data Warehouse (PowerShell)
> [!div class="op_single_selector"]
> * [Übersicht](sql-data-warehouse-manage-compute-overview.md)
> * [Portal](sql-data-warehouse-manage-compute-portal.md)
> * [PowerShell](sql-data-warehouse-manage-compute-powershell.md)
> * [REST](sql-data-warehouse-manage-compute-rest-api.md)
> * [TSQL](sql-data-warehouse-manage-compute-tsql.md)
>
>

## <a name="before-you-begin"></a>Voraussetzungen
### <a name="install-the-latest-version-of-azure-powershell"></a>Installieren der neuesten Version von Azure PowerShell
> [!NOTE]
> Damit Sie Azure Powershell mit SQL Data Warehouse verwenden können, benötigen Sie Azure Powershell Version 1.0.3 oder höher.  Um Ihre aktuelle Version zu überprüfen, führen Sie den Befehl **Get-Module -ListAvailable -Name Azure**aus. Sie können die neueste Version von [Microsoft-Webplattform-Installer][Microsoft Web Platform Installer] herunterladen.  Weitere Informationen finden Sie unter [Gewusst wie: Installieren und Konfigurieren von Azure PowerShell][How to install and configure Azure PowerShell].
>
> 

### <a name="get-started-with-azure-powershell-cmdlets"></a>Erste Schritte mit Azure PowerShell-Cmdlets
Erste Schritte:

1. Öffnen Sie Azure PowerShell.
2. Zum Anmelden bei Azure Resource Manager und Auswählen Ihres Abonnements führen Sie bei der PowerShell-Eingabeaufforderung die folgenden Befehle aus:

    ```PowerShell
    Login-AzureRmAccount
    Get-AzureRmSubscription
    Select-AzureRmSubscription -SubscriptionName "MySubscription"
    ```

<a name="scale-performance-bk"></a>
<a name="scale-compute-bk"></a>

## <a name="scale-compute-power"></a>Skalieren von Computeleistung
[!INCLUDE [SQL Data Warehouse scale DWUs description](../../includes/sql-data-warehouse-scale-dwus-description.md)]

Um die DWUs zu ändern, verwenden Sie das PowerShell-Cmdlet [Set AzureRmSqlDatabase][Set-AzureRmSqlDatabase]. Im folgenden Beispiel wird der Servicelevel-Zielpunkt für die Datenbank „MySQLDW“, die auf dem Server „MyServer“ gehostet wird, auf „DW1000“ gesetzt.

```Powershell
Set-AzureRmSqlDatabase -DatabaseName "MySQLDW" -ServerName "MyServer" -RequestedServiceObjectiveName "DW1000"
```

<a name="pause-compute-bk"></a>

## <a name="pause-compute"></a>Anhalten von Computeressourcen
[!INCLUDE [SQL Data Warehouse pause description](../../includes/sql-data-warehouse-pause-description.md)]

Um eine Datenbank anzuhalten, verwenden Sie das Cmdlet [Suspend-AzureRmSqlDatabase][Suspend-AzureRmSqlDatabase]. Im folgenden Beispiel wird die auf dem Server „Server01“ gehostete Datenbank mit der Bezeichnung „Database02“ angehalten. Der Server befindet sich in einer Azure-Ressourcengruppe namens „ResourceGroup1“.

> [!NOTE]
> Hinweis: Wenn Ihr Server „foo.database.windows.net“ heißt, verwenden Sie in den PowerShell-Cmdlets für „-ServerName“ den Namen „foo“.
>
> 

```Powershell
Suspend-AzureRmSqlDatabase –ResourceGroupName "ResourceGroup1" `
–ServerName "Server01" –DatabaseName "Database02"
```
In einer Variation ruft das nächste Beispiel die Datenbank in das $database-Objekt ab. Das abgerufene Objekt wird dann an [Suspend-AzureRmSqlDatabase][Suspend-AzureRmSqlDatabase] weitergeleitet. Die Ergebnisse werden in dem Objekt „resultDatabase“ gespeichert. Der letzte Befehl zeigt die Ergebnisse an.

```Powershell
$database = Get-AzureRmSqlDatabase –ResourceGroupName "ResourceGroup1" `
–ServerName "Server01" –DatabaseName "Database02"
$resultDatabase = $database | Suspend-AzureRmSqlDatabase
$resultDatabase
```

<a name="resume-compute-bk"></a>

## <a name="resume-compute"></a>Fortsetzen von Computeressourcen
[!INCLUDE [SQL Data Warehouse resume description](../../includes/sql-data-warehouse-resume-description.md)]

Verwenden Sie das Cmdlet [Resume-AzureRmSqlDatabase][Resume-AzureRmSqlDatabase], um eine Datenbank zu starten. Im folgenden Beispiel wird die auf dem Server „Server01“ gehostete Datenbank mit der Bezeichnung „Database02“ gestartet. Der Server befindet sich in einer Azure-Ressourcengruppe namens „ResourceGroup1“.

```Powershell
Resume-AzureRmSqlDatabase –ResourceGroupName "ResourceGroup1" `
–ServerName "Server01" -DatabaseName "Database02"
```

In einer Variation ruft das nächste Beispiel die Datenbank in das $database-Objekt ab. Dann wird das Objekt an [Resume-AzureRmSqlDatabase][Resume-AzureRmSqlDatabase] weitergeleitet, und die Ergebnisse werden in $resultDatabase gespeichert. Der letzte Befehl zeigt die Ergebnisse an.

```Powershell
$database = Get-AzureRmSqlDatabase –ResourceGroupName "ResourceGroup1" `
–ServerName "Server01" –DatabaseName "Database02"
$resultDatabase = $database | Resume-AzureRmSqlDatabase
$resultDatabase
```

<a name="check-database-state-bk"></a>

## <a name="check-database-state"></a>Überprüfen des Datenbankzustands

Wie in den obigen Beispielen gezeigt wird, können Sie das Cmdlet [Get-AzureRmSqlDatabase][Get-AzureRmSqlDatabase] zum Abrufen von Informationen zu einer Datenbank und damit zum Überprüfen des Status und auch als Argument verwenden. 

```powershell
Get-AzureRmSqlDatabase [-ResourceGroupName] <String> [-ServerName] <String> [[-DatabaseName] <String>]
 [-InformationAction <ActionPreference>] [-InformationVariable <String>] [-Confirm] [-WhatIf]
 [<CommonParameters>]
```

Das Ergebnis ähnelt folgendem: 

```powershell    
ResourceGroupName             : nytrg
ServerName                    : nytsvr
DatabaseName                  : nytdb
Location                      : West US
DatabaseId                    : 86461aae-8e3d-4ded-9389-ac9d4bc69bbb
Edition                       : DataWarehouse
CollationName                 : SQL_Latin1General_CP1CI_AS
CatalogCollation              :
MaxSizeBytes                  : 32212254720
Status                        : Online
CreationDate                  : 10/26/2016 4:33:14 PM
CurrentServiceObjectiveId     : 620323bf-2879-4807-b30d-c2e6d7b3b3aa
CurrentServiceObjectiveName   : System2
RequestedServiceObjectiveId   : 620323bf-2879-4807-b30d-c2e6d7b3b3aa
RequestedServiceObjectiveName :
ElasticPoolName               :
EarliestRestoreDate           : 1/1/0001 12:00:00 AM
```

Damit können Sie den *Status* der Datenbank überprüfen. In diesem Fall sehen Sie sich, dass diese Datenbank online ist. 

Wenn Sie diesen Befehl ausführen, erhalten Sie als Statuswert entweder „Online“, „Wird angehalten“, „Wird fortgesetzt“, „Wird skaliert“ oder „Angehalten“.

<a name="next-steps-bk"></a>

## <a name="next-steps"></a>Nächste Schritte
Andere Verwaltungsaufgaben finden Sie in der [Verwaltungsübersicht][Management overview].

<!--Image references-->

<!--Article references-->
[Service capacity limits]: ./sql-data-warehouse-service-capacity-limits.md
[Management overview]: ./sql-data-warehouse-overview-manage.md
[How to install and configure Azure PowerShell]: /powershell/azureps-cmdlets-docs
[Manage compute overview]: ./sql-data-warehouse-manage-compute-overview.md

<!--MSDN references-->
[Resume-AzureRmSqlDatabase]: https://msdn.microsoft.com/library/mt619347.aspx
[Suspend-AzureRmSqlDatabase]: https://msdn.microsoft.com/library/mt619337.aspx
[Set-AzureRmSqlDatabase]: https://msdn.microsoft.com/library/mt619433.aspx
[Get-AzureRmSqlDatabase]: /powershell/servicemanagement/azure.sqldatabase/v1.6.1/get-azuresqldatabase

<!--Other Web references-->
[Microsoft Web Platform Installer]: https://aka.ms/webpi-azps
[Azure portal]: http://portal.azure.com/

