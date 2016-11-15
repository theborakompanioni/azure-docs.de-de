---
title: Erstellen von SQL Data Warehouse mithilfe von PowerShell | Microsoft Docs
description: Erstellen von SQL Data Warehouse mithilfe von PowerShell
services: sql-data-warehouse
documentationcenter: NA
author: barbkess
manager: jhubbard
editor: 
ms.assetid: 97434863-7938-4129-8949-5a119f5949e3
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: get-started-article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.date: 10/31/2016
ms.author: barbkess
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: 3d13d4a0dd1d6e0b7361a57e167b06f0b717bfb4


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

* **Azure-Konto:** Lesen Sie zum Erstellen eines Kontos die Informationen unter [Kostenlose Azure-Testversion][Kostenlose Azure-Testversion] oder [MSDN-Azure-Gutschriften][MSDN-Azure-Gutschriften].
* ** Azure SQL Server**: Weitere Details finden Sie unter [Erstellen eines logischen Azure SQL-Datenbankservers mit dem Azure-Portal][Erstellen eines logischen Azure SQL-Datenbankservers mit dem Azure-Portal] oder [Erstellen eines logischen Servers mit Azure SQL-Datenbank mithilfe von PowerShell][Erstellen eines logischen Servers mit Azure SQL-Datenbank mithilfe von PowerShell].
* **Ressourcengruppe:** Verwenden Sie entweder die gleiche Ressourcengruppe wie Ihre Azure SQL Server-Instanz, oder lesen Sie die Informationen zum [Erstellen einer neuen Ressourcengruppe][Erstellen einer neuen Ressourcengruppe].
* **PowerShell-Version 1.0.3 oder höher**: Sie können die Version überprüfen, indem Sie **Get-Module -ListAvailable -Name Azure** ausführen.  Sie können die neueste Version über den [Microsoft-Webplattform-Installer][Microsoft-Webplattform-Installer] installieren.  Weitere Informationen zum Installieren der neuesten Version finden Sie unter [Installieren und Konfigurieren von Azure PowerShell][Installieren und Konfigurieren von Azure PowerShell].

> [!NOTE]
> Wenn Sie eine SQL Data Warehouse-Instanz erstellen, wird unter Umständen auch ein neuer abrechenbarer Dienst erstellt.  Unter [SQL Data Warehouse – Preise][SQL Data Warehouse – Preise] finden Sie weitere Informationen zu den Preisen.
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

Weitere Informationen zu den Parameteroptionen finden Sie unter [New-AzureRmSqlDatabase][New-AzureRmSqlDatabase] sowie unter [CREATE DATABASE (Azure SQL Data Warehouse)][CREATE DATABASE (Azure SQL Data Warehouse)].

## <a name="next-steps"></a>Nächste Schritte
Nach der SQL Data Warehouse-Bereitstellung können Sie [Beispieldaten laden][Beispieldaten laden] oder die Schritte zum [Entwickeln][Entwickeln], [Laden][Laden] oder [Migrieren][Migrieren] lernen.

Weitere Informationen zur programmgesteuerten Verwaltung von SQL Data Warehouse finden Sie im Artikel zur Verwendung von [PowerShell-Cmdlets und REST-APIs][PowerShell-Cmdlets und REST-APIs].

<!--Image references-->

<!--Article references-->
[DWU]: ./sql-data-warehouse-overview-what-is.md#data-warehouse-units
[Migrieren]: ./sql-data-warehouse-overview-migrate.md
[Entwickeln]: ./sql-data-warehouse-overview-develop.md
[Laden]: ./sql-data-warehouse-load-with-bcp.md
[Laden von Beispieldaten]: ./sql-data-warehouse-load-sample-databases.md
[PowerShell-Cmdlets und REST-APIs]: ./sql-data-warehouse-reference-powershell-cmdlets.md
[Firewallregeln]: ../sql-database-configure-firewall-settings.md

[Installieren und Konfigurieren von Azure PowerShell]: ../powershell-install-configure.md
[Erstellen eines SQL Data Warehouse (Azure-Portal)]: ./sql-data-warehouse-get-started-provision.md
[Erstellen eines logischen Azure SQL-Datenbankservers mit dem Azure-Portal]: ../sql-database/sql-database-get-started.md#create-an-azure-sql-database-logical-server
[Erstellen eines logischen Servers mit Azure SQL-Datenbank mithilfe von PowerShell]: ../sql-database/sql-database-get-started-powershell.md#database-setup-create-a-resource-group-server-and-firewall-rule
[Erstellen einer neuen Ressourcengruppe]: ../resource-group-template-deploy-portal.md#create-resource-group

<!--MSDN references-->
[MSDN]: https://msdn.microsoft.com/library/azure/dn546722.aspx
[New-AzureRmSqlDatabase]: https://msdn.microsoft.com/library/mt619339.aspx
[CREATE DATABASE (Azure SQL Data Warehouse)]: https://msdn.microsoft.com/library/mt204021.aspx

<!--Other Web references-->
[Microsoft-Webplattform-Installer]: https://aka.ms/webpi-azps
[SQL Data Warehouse – Preise]: https://azure.microsoft.com/pricing/details/sql-data-warehouse/
[Kostenlose Azure-Testversion]: https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F
[MSDN-Azure-Gutschriften]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F



<!--HONumber=Nov16_HO2-->


