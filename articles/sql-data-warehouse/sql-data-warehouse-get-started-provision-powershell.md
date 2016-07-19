<properties
   pageTitle="Erstellen von SQL Data Warehouse mithilfe von Powershell | Microsoft Azure"
   description="Erstellen von SQL Data Warehouse mithilfe von Powershell"
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="lodipalm"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="07/11/2016"
   ms.author="lodipalm;barbkess;sonyama"/>

# Erstellen von SQL Data Warehouse mithilfe von Powershell

> [AZURE.SELECTOR]
- [Azure-Portal](sql-data-warehouse-get-started-provision.md)
- [TSQL](sql-data-warehouse-get-started-create-database-tsql.md)
- [PowerShell](sql-data-warehouse-get-started-provision-powershell.md)

## Voraussetzungen
Stellen Sie sicher, dass die folgenden Voraussetzungen erfüllt sind, bevor Sie beginnen.

- **Azure-Konto:** Lesen Sie zum Erstellen eines Kontos die Informationen unter [Kostenlose Azure-Testversion][] oder [MSDN-Azure-Gutschriften][].
- **V12 Azure SQL Server:** Informationen finden Sie unter [Erstellen eines logischen Azure SQL-Datenbankservers mit dem Azure-Portal][] oder [Erstellen einer neuen SQL-Datenbank und Ausführen gängiger Datenbankeinrichtungsaufgaben mithilfe von PowerShell-Cmdlets][].
- **Ressourcengruppenname:** Verwenden Sie entweder die gleiche Ressourcengruppe wie V12 Azure SQL Server, oder lesen Sie die Informationen unter [Ressourcengruppen][], um eine neue Ressourcengruppe zu erstellen.
- **PowerShell-Version 1.0.3 oder höher:** Sie können die Version überprüfen, indem Sie **Get-Module -ListAvailable -Name Azure** ausführen. Sie können die neueste Version über [Microsoft-Webplattform-Installer][] installieren. Weitere Informationen zum Installieren der neuesten Version finden Sie unter [Installieren und Konfigurieren von Azure PowerShell][].

> [AZURE.NOTE] Wenn Sie ein neues SQL Data Warehouse erstellen, wird dadurch unter Umständen auch ein neuer abrechenbarer Dienst erstellt. Unter [SQL Data Warehouse – Preise][] finden Sie weitere Informationen zu den Preisen.

## Erstellen eines SQL Data Warehouse
1. Öffnen Sie Windows PowerShell.
2. Führen Sie dieses Cmdlet aus, um sich am Azure-Ressourcen-Manager anzumelden.

	```Powershell
	Login-AzureRmAccount
	```
	
3. Wählen Sie das Abonnement aus, das Sie für Ihre aktuelle Sitzung verwenden möchten.

	```Powershell
	Get-AzureRmSubscription	-SubscriptionName "MySubscription" | Select-AzureRmSubscription
	```

4.  Erstellen Sie eine Datenbank. In diesem Beispiel wird eine neue Datenbank mit dem Namen „mynewsqldw“ mit der Dienstzielebene „DW400“ auf dem Server „sqldwserver1“ erstellt, der sich in der Ressourcengruppe „mywesteuroperesgp1“ befindet.

	```Powershell
	New-AzureRmSqlDatabase -RequestedServiceObjectiveName "DW400" -DatabaseName "mynewsqldw" -ServerName "sqldwserver1" -ResourceGroupName "mywesteuroperesgp1" -Edition "DataWarehouse"
	```

Die für das Cmdlet erforderlichen Parameter sind:

- **RequestedServiceObjectiveName**: die angeforderte [DWU][]-Menge. Unterstützte Werte: DW100, DW200, DW300, DW400, DW500, DW600, DW1000, DW1200, DW1500, DW2000, DW3000 und DW6000.
- **DatabaseName**: der Name des SQL Data Warehouse, das Sie erstellen.
- **ServerName**: der Name des Servers, den Sie für die Erstellung verwenden (muss V12 sein).
- **ResourceGroupName**: die Ressourcengruppe, die Sie verwenden. Verwenden Sie zum Abrufen der in Ihrem Abonnement verfügbaren Ressourcengruppen das Cmdlet "Get-AzureResourceGroup":
- **Edition**: Sie müssen die Edition auf "Data Warehouse" festlegen, um ein SQL Data Warehouse zu erstellen.

Weitere Informationen zu den Parameteroptionen finden Sie unter [Create Database (Azure SQL Data Warehouse)][]. Die Befehlsreferenz finden Sie unter [New-AzureRmSqlDatabase][].

## Nächste Schritte

Nach der SQL Data Warehouse-Bereitstellung können Sie [Beispieldaten laden][] oder die Schritte zum [Entwickeln][], [Laden][] oder [Migrieren][] kennenlernen.

Weitere Informationen zur programmgesteuerten Verwaltung von SQL Data Warehouse finden Sie im Artikel zur Verwendung von [PowerShell-Cmdlets und REST-APIs][].

<!--Image references-->

<!--Article references-->
[DWU]: ./sql-data-warehouse-overview-what-is.md#data-warehouse-units
[Migrieren]: ./sql-data-warehouse-overview-migrate.md
[Entwickeln]: ./sql-data-warehouse-overview-develop.md
[Laden]: ./sql-data-warehouse-load-with-bcp.md
[Beispieldaten laden]: ./sql-data-warehouse-get-started-load-sample-databases.md
[PowerShell-Cmdlets und REST-APIs]: ./sql-data-warehouse-reference-powershell-cmdlets.md
[firewall rules]: ../sql-database-configure-firewall-settings.md

[Installieren und Konfigurieren von Azure PowerShell]: ../powershell/powershell-install-configure.md
[how to create a SQL Data Warehouse from the Azure Portal]: ./sql-data-warehouse-get-started-provision.md
[Erstellen eines logischen Azure SQL-Datenbankservers mit dem Azure-Portal]: ../sql-database/sql-database-get-started.md#create-an-azure-sql-database-logical-server
[Erstellen einer neuen SQL-Datenbank und Ausführen gängiger Datenbankeinrichtungsaufgaben mithilfe von PowerShell-Cmdlets]: ../sql-database/sql-database-get-started-powershell.md#database-setup-create-a-resource-group-server-and-firewall-rule
[Ressourcengruppen]: ../azure-portal/resource-group-portal.md

<!--MSDN references--> 
[MSDN]: https://msdn.microsoft.com/library/azure/dn546722.aspx
[New-AzureRmSqlDatabase]: https://msdn.microsoft.com/library/mt619339.aspx
[Create Database (Azure SQL Data Warehouse)]: https://msdn.microsoft.com/library/mt204021.aspx

<!--Other Web references-->
[Microsoft-Webplattform-Installer]: https://aka.ms/webpi-azps
[SQL Data Warehouse – Preise]: https://azure.microsoft.com/pricing/details/sql-data-warehouse/
[Kostenlose Azure-Testversion]: https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F
[MSDN-Azure-Gutschriften]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F

<!---HONumber=AcomDC_0713_2016-->