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
   ms.date="04/20/2016"
   ms.author="lodipalm;barbkess;sonyama"/>

# Erstellen von SQL Data Warehouse mithilfe von Powershell

> [AZURE.SELECTOR]
- [Azure-Portal](sql-data-warehouse-get-started-provision.md)
- [TSQL](sql-data-warehouse-get-started-create-database-tsql.md)
- [PowerShell](sql-data-warehouse-get-started-provision-powershell.md)

### Voraussetzungen
Stellen Sie sicher, dass die folgenden Voraussetzungen erfüllt sind, bevor Sie beginnen.

- V12 Azure SQL Server zum Hosten der Datenbank
- Ressourcengruppenname für SQL Server liegt vor

Weitere Informationen zu den o.g. Voraussetzungen finden Sie unter **Configure and create a server** (Konfigurieren und Erstellen eines Servers) im Artikel [Create a SQL Data Warehouse][] (Erstellen eines SQL Data Warehouse).

> [AZURE.NOTE]  Damit Sie Azure Powershell mit SQL Data Warehouse verwenden können, müssen Sie Azure PowerShell Version 1.0.3 oder höher installieren. Sie können Ihre Version feststellen, indem Sie **Get-Module -ListAvailable -Name Azure** ausführen. Sie können die neueste Version von [Microsoft Web Platform Installer][] herunterladen. Weitere Informationen zum Installieren der neuesten Version finden Sie unter [Installieren und Konfigurieren von Azure PowerShell][].

## Erstellen einer SQL Data Warehouse-Datenbank
1. Öffnen Sie Windows PowerShell.
2. Führen Sie dieses Cmdlet aus, um sich am Azure-Ressourcen-Manager anzumelden.

	```Powershell
	Login-AzureRmAccount
	```
	
3. Wählen Sie das Abonnement aus, das Sie für Ihre aktuelle Sitzung verwenden möchten.

	```Powershell
	Get-AzureRmSubscription	-SubscriptionName "MySubscription" | Select-AzureRmSubscription
	```

4.  Erstellen Sie eine Datenbank. In diesem Beispiel wird eine neue Datenbank mit dem Namen „mynewsqldw“ mit der Dienstzielebene „DW400“ auf dem Server „sqldwserver1“ erstellt, der sich in der Ressourcengruppe „mywesteuroperesgp1“ befindet. **HINWEIS: Wenn Sie eine neue SQL Data Warehouse-Datenbank erstellen, fallen dadurch unter Umständen weitere Gebühren an. Unter [SQL Data Warehouse – Preise][] finden Sie weitere Informationen zu den Preisen.**

	```Powershell
	New-AzureRmSqlDatabase -RequestedServiceObjectiveName "DW400" -DatabaseName "mynewsqldw" -ServerName "sqldwserver1" -ResourceGroupName "mywesteuroperesgp1" -Edition "DataWarehouse"
	```

Die für das Cmdlet erforderlichen Parameter sind:

- **RequestedServiceObjectiveName**: die angeforderte DWU-Menge in der Form „DWXXX“. DWU stellt eine Zuweisung von CPU und Arbeitsspeicher dar. Jeder DWU-Wert steht für eine lineare Erhöhung in diesen Ressourcen. Folgende Werte werden unterstützt: 100, 200, 300, 400, 500, 600, 1000, 1200, 1500, 2000.
- **DatabaseName**: der Name des SQL Data Warehouse, das Sie erstellen.
- **ServerName**: der Name des Servers, den Sie für die Erstellung verwenden (muss V12 sein).
- **ResourceGroupName**: die Ressourcengruppe, die Sie verwenden. Verwenden Sie zum Abrufen der in Ihrem Abonnement verfügbaren Ressourcengruppen das Cmdlet "Get-AzureResourceGroup":
- **Edition**: Sie müssen die Edition auf "Data Warehouse" festlegen, um ein SQL Data Warehouse zu erstellen.

Weitere Informationen zu den Parameteroptionen finden Sie unter [Create Database (Azure SQL Data Warehouse)][]. Die Befehlsreferenz finden Sie unter [New-AzureRmSqlDatabase][].

## Nächste Schritte
Nach der SQL Data Warehouse-Bereitstellung können Sie [Beispieldaten laden][] oder die Schritte zum [Entwickeln][], [Laden][] oder [Migrieren][] lernen.

Weitere Informationen zur programmgesteuerten Verwaltung von SQL Data Warehouse finden Sie im Artikel zur Verwendung von [PowerShell-Cmdlets und REST-APIs][].

<!--Image references-->

<!--Article references-->
[Migrieren]: sql-data-warehouse-overview-migrate.md
[Entwickeln]: sql-data-warehouse-overview-develop.md
[Laden]: sql-data-warehouse-load-with-bcp.md
[Beispieldaten laden]: sql-data-warehouse-get-started-manually-load-samples.md
[PowerShell-Cmdlets und REST-APIs]: sql-data-warehouse-reference-powershell-cmdlets.md
[firewall rules]: sql-database-configure-firewall-settings.md
[Installieren und Konfigurieren von Azure PowerShell]: ../powershell/powershell-install-configure.md
[Create a SQL Data Warehouse]: sql-data-warehouse-get-started-provision.md

<!--MSDN references--> 
[MSDN]: https://msdn.microsoft.com/library/azure/dn546722.aspx
[New-AzureRmSqlDatabase]: https://msdn.microsoft.com/library/mt619339.aspx
[Create Database (Azure SQL Data Warehouse)]: https://msdn.microsoft.com/library/mt204021.aspx

<!--Other Web references-->
[Microsoft Web Platform Installer]: https://aka.ms/webpi-azps
[SQL Data Warehouse – Preise]: https://azure.microsoft.com/pricing/details/sql-data-warehouse/
 

<!---HONumber=AcomDC_0511_2016-->