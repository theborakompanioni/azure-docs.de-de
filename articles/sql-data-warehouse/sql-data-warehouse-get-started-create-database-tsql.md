---
title: Erstellen eines SQL Data Warehouse mit TSQL | Microsoft Docs
description: Erfahren Sie, wie Sie ein Azure SQL Data Warehouse mit TSQL erstellen.
services: sql-data-warehouse
documentationcenter: NA
author: barbkess
manager: jhubbard
editor: 
tags: azure-sql-data-warehouse
ms.assetid: a4e2e68e-aa9c-4dd3-abb0-f7df997d237a
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: get-started-article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.date: 10/31/2016
ms.author: barbkess
translationtype: Human Translation
ms.sourcegitcommit: 5d3bcc3c1434b16279778573ccf3034f9ac28a4d
ms.openlocfilehash: 836d72e32e54ecef9691b55214766a1fc3ff9701
ms.lasthandoff: 12/06/2016


---
# <a name="create-a-sql-data-warehouse-database-by-using-transact-sql-tsql"></a>Erstellen einer SQL Data Warehouse-Datenbank mithilfe von Transact-SQL (TSQL)
> [!div class="op_single_selector"]
> * [Azure-Portal](sql-data-warehouse-get-started-provision.md)
> * [TSQL](sql-data-warehouse-get-started-create-database-tsql.md)
> * [PowerShell](sql-data-warehouse-get-started-provision-powershell.md)
>
>

In diesem Artikel erfahren Sie, wie Sie eine SQL Data Warehouse-Instanz mithilfe von T-SQL erstellen.

## <a name="prerequisites"></a>Voraussetzungen
Zunächst benötigen Sie Folgendes:

* **Azure-Konto:** Lesen Sie zum Erstellen eines Kontos die Informationen unter [Kostenlose Azure-Testversion][Azure Free Trial] oder [MSDN-Azure-Gutschriften][MSDN Azure Credits].
* **Azure SQL Server:** Ausführliche Informationen finden Sie unter [Erstellen eines logischen Azure SQL-Datenbankservers mit dem Azure-Portal][Create an Azure SQL Database logical server with the Azure Portal] oder [Erstellen eines logischen Servers mit Azure SQL-Datenbank mithilfe von PowerShell][Create an Azure SQL Database logical server with PowerShell].
* **Ressourcengruppe:**Verwenden Sie entweder die gleiche Ressourcengruppe wie Ihre Azure SQL Server-Instanz, oder lesen Sie die Informationen zum [Erstellen einer neuen Ressourcengruppe][how to create a resource group].
* **Umgebung für die Ausführung von T-SQL:** Zum Ausführen von T-SQL können Sie [Visual Studio][Installing Visual Studio and SSDT], [sqlcmd][sqlcmd] oder [SSMS][SSMS] verwenden.

> [!NOTE]
> Wenn Sie eine SQL Data Warehouse-Instanz erstellen, wird unter Umständen auch ein neuer abrechenbarer Dienst erstellt.  Unter [SQL Data Warehouse – Preise][SQL Data Warehouse pricing] finden Sie weitere Informationen zu den Preisen.
>
>

## <a name="create-a-database-with-visual-studio"></a>Erstellen einer Datenbank mit Visual Studio
Falls Sie noch nicht mit Visual Studio vertraut sind, finden Sie im Artikel [Abfragen von Azure SQL Data Warehouse (Visual Studio)][Query Azure SQL Data Warehouse (Visual Studio)] weitere Informationen.  Zum Starten öffnen Sie den SQL Server-Objekt-Explorer in Visual Studio und stellen eine Verbindung mit dem Server her, der Ihre SQL Data Warehouse-Datenbank hosten soll.  Wenn die Verbindung hergestellt wurde, können Sie eine SQL Data Warehouse-Instanz erstellen, indem Sie den folgenden SQL-Befehl auf die Datenbank **master** anwenden.  Dieser Befehl erstellt die Datenbank „MySqlDwDb“ mit dem Dienstziel DW400 und legt die maximale Größe der Datenbank auf 10 TB fest.

```sql
CREATE DATABASE MySqlDwDb COLLATE SQL_Latin1_General_CP1_CI_AS (EDITION='datawarehouse', SERVICE_OBJECTIVE = 'DW400', MAXSIZE= 10240 GB);
```

## <a name="create-a-database-with-sqlcmd"></a>Erstellen Sie eine Datenbank mit SQLCMD
Alternativ können Sie den gleichen Befehl mit „sqlcmd“ ausführen, wenn Sie bei einer Eingabeaufforderung Folgendes ausführen.

```sql
sqlcmd -S <Server Name>.database.windows.net -I -U <User> -P <Password> -Q "CREATE DATABASE MySqlDwDb COLLATE SQL_Latin1_General_CP1_CI_AS (EDITION='datawarehouse', SERVICE_OBJECTIVE = 'DW400', MAXSIZE= 10240 GB)"
```

Ohne entsprechende Angabe wird die Standardsortierung „COLLATE SQL_Latin1_General_CP1_CI_AS“ verwendet.  `MAXSIZE` kann zwischen 250 GB und 240 TB liegen.  `SERVICE_OBJECTIVE` kann einen [DWU][DWU]-Wert zwischen DW100 und DW2000 aufweisen.  Eine Liste aller gültigen Werte finden Sie in der MSDN-Dokumentation zu [CREATE DATABASE][CREATE DATABASE].  Sowohl MAXSIZE als auch SERVICE_OBJECTIVE kann mit dem T-SQL-Befehl [ALTER DATABASE][ALTER DATABASE] geändert werden.  Die Sortierung einer Datenbank kann nach der Erstellung nicht mehr geändert werden.   Gehen Sie beim Ändern von „SERVICE_OBJECTIVE“ mit Bedacht vor, da eine DWU-Änderung einen Neustart des Diensts und somit den Abbruch sämtlicher aktueller Abfragen zur Folge hat.  Da MAXSIZE nur ein einfacher Metadatenvorgang ist, werden durch die Änderung dieses Werts keine Dienste neu gestartet.

## <a name="next-steps"></a>Nächste Schritte
Nach der SQL Data Warehouse-Bereitstellung können Sie [Beispieldaten laden][load sample data] oder die Schritte zum [Entwickeln][develop], [Laden][load] oder [Migrieren][migrate] erlernen.

<!--Article references-->
[DWU]: ./sql-data-warehouse-overview-what-is.md
[how to create a SQL Data Warehouse from the Azure portal]: sql-data-warehouse-get-started-provision.md
[Query Azure SQL Data Warehouse (Visual Studio)]: sql-data-warehouse-query-visual-studio.md
[migrate]: sql-data-warehouse-overview-migrate.md
[develop]: sql-data-warehouse-overview-develop.md
[load]: sql-data-warehouse-overview-load.md
[load sample data]: sql-data-warehouse-load-sample-databases.md
[Create an Azure SQL database with the Azure Portal]: ../sql-database/sql-database-get-started.md
[Create an Azure SQL database with PowerShell]: ../sql-database/sql-database-create-and-configure-database-powershell
[how to create a resource group]: ../azure-resource-manager/resource-group-template-deploy-portal.md#create-resource-group
[Installing Visual Studio and SSDT]: sql-data-warehouse-install-visual-studio.md
[sqlcmd]: sql-data-warehouse-get-started-connect-sqlcmd.md

<!--MSDN references-->
[CREATE DATABASE]: https://msdn.microsoft.com/library/mt204021.aspx
[ALTER DATABASE]: https://msdn.microsoft.com/library/mt204042.aspx
[SSMS]: https://msdn.microsoft.com/library/mt238290.aspx

<!--Other Web references-->
[SQL Data Warehouse pricing]: https://azure.microsoft.com/pricing/details/sql-data-warehouse/
[Azure Free Trial]: https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F
[MSDN Azure Credits]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F

