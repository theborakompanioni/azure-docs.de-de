---
title: "T-SQL: Verwalten eines Pools für elastische Datenbanken in Azure SQL-Datenbank | Microsoft-Dokumentation"
description: "Verwenden Sie T-SQL zum Verwalten eines Pools für elastische Datenbanken in Azure SQL-Datenbank"
services: sql-database
documentationcenter: 
author: srinia
manager: jhubbard
editor: 
ms.assetid: 4e288e17-bc3e-4255-9fbe-0a2ac0dbd7dd
ms.service: sql-database
ms.custom: multiple databases
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-management
ms.date: 05/27/2016
ms.author: srinia
translationtype: Human Translation
ms.sourcegitcommit: 6453cca9f876e6c363fbed463263c0f9684a3e70
ms.openlocfilehash: 11b1a6dd9ecaaa3abaf088b1fed73aae783252f5
ms.lasthandoff: 02/08/2017


---
# <a name="monitor-and-manage-an-elastic-pool-with-transact-sql"></a>Überwachen und Verwalten eines elastischen Pools per Transact-SQL
In diesem Thema wird das Verwalten skalierbarer [Pools für elastische Datenbanken](sql-database-elastic-pool.md) mit Transact-SQL erläutert.  Ein Azure-Pool für elastische Datenbanken kann auch mit dem [Azure-Portal](https://portal.azure.com/), mit [PowerShell](sql-database-elastic-pool-manage-powershell.md), der REST-API oder mithilfe von [C#][Erstellen und Verwalten eines Pools für elastische Datenbanken mithilfe von C#](sql-database-elastic-pool-manage-csharp.md) erstellt und verwaltet werden. Mithilfe von [Transact-SQL](sql-database-elastic-pool-manage-tsql.md) können Sie Datenbanken auch erstellen und in und aus Pools für elastische Datenbanken verschieben.


Verwenden Sie die Befehle [Create Database (Azure SQL-Datenbank)](https://msdn.microsoft.com/library/dn268335.aspx) und [Alter Database(Azure SQL-Datenbank)](https://msdn.microsoft.com/library/mt574871.aspx), um Datenbanken zu erstellen und in elastische Pools bzw. aus elastischen Pools zu verschieben. Der elastische Pool muss vorhanden sein, bevor Sie diese Befehle verwenden können. Diese Befehle wirken sich nur auf Datenbanken aus. Die Erstellung eines neuen Pools und die Einstellung der Pooleigenschaften (z.B. min. und max. eDTUs) kann mit T-SQL-Befehlen nicht geändert werden.

## <a name="create-a-pooled-database-in-an-elastic-pool"></a>Erstellen einer gruppierten Datenbank in einem Pool für elastische Datenbanken
Verwenden Sie den Befehl CREATE DATABASE mit der Option SERVICE_OBJECTIVE.   

    CREATE DATABASE db1 ( SERVICE_OBJECTIVE = ELASTIC_POOL (name = [S3M100] ));
    -- Create a database named db1 in an elastic named S3M100.

Alle Datenbanken in einem elastischen Pool erben die Dienstebene des elastischen Pools (Basic, Standard, Premium). 

## <a name="move-a-database-between-elastic-pools"></a>Verschieben einer Datenbank zwischen elastischen Pools
Verwenden Sie den Befehl ALTER DATABASE mit MODIFY, und legen Sie die Option SERVICE\_OBJECTIVE als ELASTIC\_POOL fest. Legen Sie den Namen auf den Namen des Zielpools fest.

    ALTER DATABASE db1 MODIFY ( SERVICE_OBJECTIVE = ELASTIC_POOL (name = [PM125] ));
    -- Move the database named db1 to an elastic named P1M125  

## <a name="move-a-database-into-an-elastic-pool"></a>Verschieben einer Datenbank in einen elastischen Pool
Verwenden Sie den Befehl ALTER DATABASE mit MODIFY, und legen Sie die Option SERVICE\_OBJECTIVE als ELASTIC_POOL fest. Legen Sie den Namen auf den Namen des Zielpools fest.

    ALTER DATABASE db1 MODIFY ( SERVICE_OBJECTIVE = ELASTIC_POOL (name = [S3100] ));
    -- Move the database named db1 to an elastic named S3100.

## <a name="move-a-database-out-of-an-elastic-pool"></a>Verschieben einer Datenbank aus einem elastischen Pool
Verwenden Sie den Befehl ALTER DATABASE, und legen Sie SERVICE_OBJECTIVE auf eine Leistungsebene fest (etwa S0 oder S1).

    ALTER DATABASE db1 MODIFY ( SERVICE_OBJECTIVE = 'S1');
    -- Changes the database into a stand-alone database with the service objective S1.

## <a name="list-databases-in-an-elastic-pool"></a>Auflisten von Datenbanken in einem elastischen Pool
Verwenden Sie die Sicht [sys.database\_service\_objectives](https://msdn.microsoft.com/library/mt712619), um alle Datenbanken in einem elastischen Pool aufzulisten. Melden Sie sich an der Masterdatenbank an, um die Sicht abzufragen.

    SELECT d.name, slo.*  
    FROM sys.databases d 
    JOIN sys.database_service_objectives slo  
    ON d.database_id = slo.database_id
    WHERE elastic_pool_name = 'MyElasticPool'; 

## <a name="get-resource-usage-data-for-an-elastic-pool"></a>Abrufen der Daten zur Ressourcenauslastung für einen Pool für elastische Datenbanken
Verwenden Sie die Sicht [sys.elastic\_pool\_resource\_stats](https://msdn.microsoft.com/library/mt280062.aspx), um die Statistik zur Ressourcenauslastung eines elastischen Pools auf einem logischen Server zu untersuchen. Melden Sie sich an der Masterdatenbank an, um die Sicht abzufragen.

    SELECT * FROM sys.elastic_pool_resource_stats 
    WHERE elastic_pool_name = 'MyElasticPool'
    ORDER BY end_time DESC;

## <a name="get-resource-usage-for-a-pooled-database"></a>Abrufen der Ressourcenauslastung für eine gruppierte Datenbank
Verwenden Sie die Sichten [sys.dm\_ db\_ resource\_stats view](https://msdn.microsoft.com/library/dn800981.aspx) oder [sys.resource \_stats](https://msdn.microsoft.com/library/dn269979.aspx), um die Statistik zur Ressourcenauslastung einer Datenbank in einem elastischen Pool zu untersuchen. Dieser Prozess ähnelt dem Abfragen der Ressourcenauslastung für eine Einzeldatenbank.

## <a name="next-steps"></a>Nächste Schritte
Nach dem Erstellen eines elastischen Pools können Sie elastische Datenbanken im Pool mit elastischen Aufträgen verwalten. Elastische Aufträge erleichtern die Ausführung von T-SQL-Skripts für eine beliebige Anzahl von Datenbanken im Pool. Weitere Informationen finden Sie unter [Übersicht über elastische Datenbankaufträge](sql-database-elastic-jobs-overview.md). 

Unter [Horizontales Hochskalieren mit Azure SQL-Datenbank](sql-database-elastic-scale-introduction.md) finden Sie Informationen zur Verwendung elastischer Datenbanktools für die horizontale Hochskalierung, zum Verschieben von Daten, für die Abfrage oder zum Erstellen von Transaktionen.


