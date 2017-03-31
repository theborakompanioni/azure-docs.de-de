---
title: 'T-SQL: Erstellen und Verwalten einzelner Azure SQL-Datenbanken | Microsoft-Dokumentation'
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
ms.sourcegitcommit: 07635b0eb4650f0c30898ea1600697dacb33477c
ms.openlocfilehash: 951c88124219a6962f655b91275aec64c716b1cd
ms.lasthandoff: 03/28/2017


---
# <a name="create-and-manage-single-azure-sql-databases-with-transact-sql"></a>Erstellen und Verwalten einzelner Azure SQL-Datenbanken mit Transact-SQL

Sie können einzelne Azure SQL-Datenbanken mit dem [Azure-Portal](https://portal.azure.com/), mit PowerShell, Transact-SQL, der REST-API oder C# erstellen und verwalten. In diesem Thema wird die Verwendung des Azure-Portals erläutert. Informationen zur Verwendung von PowerShell finden Sie unter [Erstellen und Verwalten einzelner Azure SQL-Datenbanken mit PowerShell](scripts/sql-database-create-and-configure-database-powershell.md). Informationen zur Verwendung von Transact-SQL finden Sie unter [Erstellen und Verwalten einzelner Azure SQL-Datenbanken mit Transact-SQL](sql-database-manage-single-databases-tsql.md). 

## <a name="create-an-azure-sql-database-using-transact-sql-in-sql-server-management-studio"></a>Erstellen einer Azure SQL-Datenbank mit Transact-SQL in SQL Server Management Studio

So erstellen Sie eine SQL-Datenbank mit Transact-SQL in SQL Server Management Studio:

1. Stellen Sie mithilfe von SQL Server Management Studio eine Verbindung mit dem Azure-Datenbankserver her. Verwenden Sie dabei die Prinzipalanmeldung auf Serverebene oder eine Anmeldung, die Mitglied der **Dbmanager**-Rolle ist. Weitere Informationen zu Anmeldungen finden Sie unter [Verwalten von Anmeldungen](sql-database-manage-logins.md).
2. Öffnen Sie im Objekt-Explorer den Knoten „Datenbanken“, erweitern Sie den Ordner **Systemdatenbanken**, klicken Sie mit der rechten Maustaste auf **master**, und klicken Sie anschließend auf **Neue Abfrage**.
3. Verwenden Sie die Anweisung **CREATE DATABASE** , um eine Datenbank zu erstellen. Weitere Informationen finden Sie unter [CREATE DATABASE (SQL-Datenbank)](https://msdn.microsoft.com/library/dn268335.aspx). Die folgende Anweisung erstellt eine Datenbank mit dem Namen **myTestDB** und gibt an, dass es sich um eine Datenbank der Standard S0-Edition mit einer maximalen Standardgröße von 250 GB handelt.
  
      CREATE DATABASE myTestDB    (EDITION='Standard',     SERVICE_OBJECTIVE='S0');

4. Klicken Sie auf **Ausführen** , um die Abfrage durchzuführen.
5. Klicken Sie im Objekt-Explorer mit der rechten Maustaste auf den Knoten „Datenbanken“, und klicken Sie auf **Aktualisieren**, um die neue Datenbank im Objekt-Explorer anzuzeigen. 


## <a name="change-the-service-tier-and-performance-level-of-a-single-database"></a>Ändern der Dienstebene und Leistungsstufe einer Einzeldatenbank
Ändern Sie die maximale Datenbankgröße mit [Transact-SQL (T-SQL)](https://msdn.microsoft.com/library/mt574871.aspx).

> [!TIP]
> Ein Tutorial zum Erstellen einer Datenbank mit Transact-SQL finden Sie unter [SQL-Datenbank-Tutorial: Enthält Informationen zu den ersten Schritten mit Azure SQL-Datenbankservern, -Datenbanken und -Firewallregeln mit dem Azure-Portal und SQL Server Management Studio](sql-database-get-started.md).
>

## <a name="next-steps"></a>Nächste Schritte
* Eine Übersicht über Verwaltungstools finden Sie unter [Übersicht: Verwaltungstools für SQL-Datenbank](sql-database-manage-overview.md).
* Informationen zum Ausführen von Verwaltungsaufgaben mit dem Azure-Portal finden Sie unter [Verwalten von Azure SQL-Datenbanken über das Azure-Portal](sql-database-manage-portal.md).
* Informationen zum Ausführen von Verwaltungsaufgaben mithilfe von PowerShell finden Sie unter [Verwalten von Azure SQL-Datenbank mithilfe von PowerShell](sql-database-manage-powershell.md).
* Informationen zum Ausführen von Verwaltungsaufgaben mithilfe von SQL Server Management Studio finden Sie unter [Verwalten einer Azure SQL-Datenbank mit SQL Server Management Studio](sql-database-manage-azure-ssms.md).
* Informationen zum SQL-Datenbank-Dienst finden Sie unter [Was ist SQL Database? Einführung in SQL-Datenbank](sql-database-technical-overview.md). 
* Informationen zu Azure-Datenbankservern und Datenbankfeatures finden Sie unter [Funktionen von Azure SQL-Datenbank](sql-database-features.md).

