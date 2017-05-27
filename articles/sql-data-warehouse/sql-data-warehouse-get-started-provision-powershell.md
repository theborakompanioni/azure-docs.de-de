---
title: Erstellen von SQL Data Warehouse mithilfe von PowerShell | Microsoft Docs
description: Erstellen von SQL Data Warehouse mithilfe von PowerShell
services: sql-data-warehouse
documentationcenter: NA
author: hirokib
manager: jhubbard
editor: 
ms.assetid: 97434863-7938-4129-8949-5a119f5949e3
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: get-started-article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: create
ms.date: 10/31/2016
ms.author: elbutter;barbkess
ms.translationtype: Human Translation
ms.sourcegitcommit: 97acd09d223e59fbf4109bc8a20a25a2ed8ea366
ms.openlocfilehash: 58ba34f8f99b7cd2b6a9a199bc70d79431405100
ms.contentlocale: de-de
ms.lasthandoff: 03/10/2017


---
# <a name="create-sql-data-warehouse-using-powershell"></a>Erstellen von SQL Data Warehouse mithilfe von PowerShell
> [!div class="op_single_selector"]
> * [Azure-Portal](sql-data-warehouse-get-started-provision.md)
> * [TSQL](sql-data-warehouse-get-started-create-database-tsql.md)
> * [PowerShell](sql-data-warehouse-get-started-provision-powershell.md)
>
>

In diesem Artikel erfahren Sie, wie Sie eine SQL Data Warehouse-Instanz mithilfe von PowerShell erstellen.

## <a name="prerequisites"></a>Voraussetzungen
Zunächst benötigen Sie Folgendes:

* **Azure-Konto:** Lesen Sie zum Erstellen eines Kontos die Informationen unter [Kostenlose Azure-Testversion][Azure Free Trial] oder [MSDN-Azure-Gutschriften][MSDN Azure Credits].
* **Azure SQL Server:** Ausführliche Informationen finden Sie unter [Erstellen einer Azure SQL-Datenbank im Azure-Portal][Create an Azure SQL database in the Azure Portal] oder [Erstellen einer Azure SQL-Datenbank mithilfe von PowerShell][Create an Azure SQL database with PowerShell].
* **Ressourcengruppe:**Verwenden Sie entweder die gleiche Ressourcengruppe wie Ihre Azure SQL Server-Instanz, oder lesen Sie die Informationen zum [Erstellen einer neuen Ressourcengruppe](../azure-resource-manager/resource-group-portal.md).
* **PowerShell-Version 1.0.3 oder höher**: Sie können die Version überprüfen, indem Sie **Get-Module -ListAvailable -Name Azure** ausführen.  Sie können die neueste Version über den [Microsoft-Webplattform-Installer][Microsoft Web Platform Installer] installieren.  Weitere Informationen zum Installieren der neuesten Version finden Sie unter [Installieren und Konfigurieren von Azure PowerShell][How to install and configure Azure PowerShell].

> [!NOTE]
> Wenn Sie eine SQL Data Warehouse-Instanz erstellen, wird unter Umständen auch ein neuer abrechenbarer Dienst erstellt.  Unter [SQL Data Warehouse – Preise][SQL Data Warehouse pricing] finden Sie weitere Informationen zu den Preisen.
>
>

## <a name="create-a-sql-data-warehouse"></a>Erstellen eines SQL Data Warehouse
1. Öffnen Sie Windows PowerShell.
2. Führen Sie dieses Cmdlet aus, um sich am Azure-Ressourcen-Manager anzumelden.

    ```Powershell
    Login-AzureRmAccount
    ```
3. Wählen Sie das Abonnement aus, das Sie für Ihre aktuelle Sitzung verwenden möchten.

    ```Powershell
    Get-AzureRmSubscription    -SubscriptionName "MySubscription" | Select-AzureRmSubscription
    ```
4. Erstellen Sie eine Datenbank. In diesem Beispiel wird eine Datenbank namens „mynewsqldw“ mit der Dienstzielebene „DW400“ auf dem Server „sqldwserver1“ erstellt, der sich in der Ressourcengruppe „mywesteuroperesgp1“ befindet.

   ```Powershell
   New-AzureRmSqlDatabase -RequestedServiceObjectiveName "DW400" -DatabaseName "mynewsqldw" -ServerName "sqldwserver1" -ResourceGroupName "mywesteuroperesgp1" -Edition "DataWarehouse" -CollationName "SQL_Latin1_General_CP1_CI_AS" -MaxSizeBytes 10995116277760
   ```

Erforderliche Parameter:

* **RequestedServiceObjectiveName**: die angeforderte [DWU][DWU]-Menge.  Unterstützte Werte: DW100, DW200, DW300, DW400, DW500, DW600, DW1000, DW1200, DW1500, DW2000, DW3000 und DW6000.
* **DatabaseName**: der Name des SQL Data Warehouse, das Sie erstellen.
* **ServerName**: der Name des Servers, den Sie für die Erstellung verwenden (muss V12 sein).
* **ResourceGroupName**: die Ressourcengruppe, die Sie verwenden.  Verwenden Sie zum Abrufen der in Ihrem Abonnement verfügbaren Ressourcengruppen das Cmdlet „Get-AzureResource“.
* **Edition**: Muss für die Erstellung einer SQL Data Warehouse-Instanz auf „Data Warehouse“ festgelegt werden.

Optionale Parameter:

* **CollationName**: Ohne Angabe wird die Standardsortierung „SQL_Latin1_General_CP1_CI_AS“ verwendet.  Die Sortierung kann für eine Datenbank nicht geändert werden.
* **MaxSizeBytes**: Die maximale Größe einer Datenbank beträgt standardmäßig 10 GB.

Weitere Informationen zu den Parameteroptionen finden Sie unter [New-AzureRmSqlDatabase][New-AzureRmSqlDatabase] sowie unter [CREATE DATABASE (Azure SQL Data Warehouse)][Create Database (Azure SQL Data Warehouse)].

## <a name="next-steps"></a>Nächste Schritte
Nach der SQL Data Warehouse-Bereitstellung können Sie [Beispieldaten laden][loading sample data] oder sich über die Schritte zum [Entwickeln][develop], [Laden][load] oder [Migrieren][migrate] informieren.

Weitere Informationen zur programmgesteuerten Verwaltung von SQL Data Warehouse finden Sie im Artikel zur Verwendung von [PowerShell-Cmdlets und REST-APIs][PowerShell cmdlets and REST APIs].

<!--Image references-->

<!--Article references-->
[DWU]: ./sql-data-warehouse-overview-what-is.md
[migrate]: ./sql-data-warehouse-overview-migrate.md
[develop]: ./sql-data-warehouse-overview-develop.md
[load]: ./sql-data-warehouse-load-with-bcp.md
[loading sample data]: ./sql-data-warehouse-load-sample-databases.md
[PowerShell cmdlets and REST APIs]: ./sql-data-warehouse-reference-powershell-cmdlets.md
[firewall rules]: ../sql-database-configure-firewall-settings.md

[How to install and configure Azure PowerShell]: /powershell/azureps-cmdlets-docs
[how to create a SQL Data Warehouse from the Azure Portal]: ./sql-data-warehouse-get-started-provision.md
[Create an Azure SQL database in the Azure Portal]: ../sql-database/sql-database-get-started.md
[Create an Azure SQL database with PowerShell]: ../sql-database/sql-database-get-started-powershell.md
[how to create a resource group]: ../azure-resource-manager/resource-group-template-deploy-portal.md#create-resource-group

<!--MSDN references-->
[MSDN]: https://msdn.microsoft.com/library/azure/dn546722.aspx
[New-AzureRmSqlDatabase]: https://msdn.microsoft.com/library/mt619339.aspx
[Create Database (Azure SQL Data Warehouse)]: https://msdn.microsoft.com/library/mt204021.aspx

<!--Other Web references-->
[Microsoft Web Platform Installer]: https://aka.ms/webpi-azps
[SQL Data Warehouse pricing]: https://azure.microsoft.com/pricing/details/sql-data-warehouse/
[Azure Free Trial]: https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F
[MSDN Azure Credits]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F

