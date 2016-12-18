---
title: "Überwachen und Verwalten eines Pools für elastische Datenbanken mit C# | Microsoft Docs"
description: "Methoden zur Entwicklung von C#-Datenbanken verwenden, um einen Pool für elastische Azure SQL-Datenbanken zu verwalten"
services: sql-database
documentationcenter: 
author: stevestein
manager: jhubbard
editor: 
ms.assetid: f6e6ff3b-6b60-43c1-afe9-575991e38237
ms.service: sql-database
ms.custom: sharded databases pool; how to
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: csharp
ms.workload: data-management
ms.date: 10/04/2016
ms.author: sstein
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: 62e13424d8dd652bc20c5746f81df7958963c43a


---
# <a name="monitor-and-manage-an-elastic-database-pool-with-cx23"></a>Überwachen und Verwalten eines Pools für elastische Datenbanken mit C&#x23;
> [!div class="op_single_selector"]
> * [Azure-Portal](sql-database-elastic-pool-manage-portal.md)
> * [PowerShell](sql-database-elastic-pool-manage-powershell.md)
> * [C#](sql-database-elastic-pool-manage-csharp.md)
> * [T-SQL](sql-database-elastic-pool-manage-tsql.md)
> 
> 

Erfahren Sie, wie Sie einen [Pool für elastische Datenbanken](sql-database-elastic-pool.md) mithilfe von C&#x23; verwalten. 

> [!NOTE]
> Viele neue Funktionen von SQL-Datenbank werden nur bei Verwendung des [Azure Resource Manager-Bereitstellungsmodells](../azure-resource-manager/resource-group-overview.md) unterstützt. Daher wird empfohlen, stets die aktuelle **Azure SQL-Datenbank-Verwaltungsbibliothek für .NET zu verwenden ([Dokumentationen](https://msdn.microsoft.com/library/azure/mt349017.aspx) | [NuGet-Paket](https://www.nuget.org/packages/Microsoft.Azure.Management.Sql))**. Die älteren [auf dem klassischen Bereitstellungsmodell basierenden Bibliotheken](https://www.nuget.org/packages/Microsoft.WindowsAzure.Management.Sql) werden nur zum Zweck der Abwärtskompatibilität unterstützt. Daher wird die Verwendung der neueren Resource Manager-basierten Bibliotheken empfohlen.
> 
> 

Damit Sie die in diesem Artikel aufgeführten Schritte ausführen können, benötigen Sie Folgendes:

* Einen elastischen Pool (den Pool, den Sie verwalten möchten). Informationen zum Erstellen eines Pools finden Sie unter [Erstellen eines Pools für elastische Datenbanken mit C#](sql-database-elastic-pool-create-csharp.md).
* Visual Studio. Eine kostenlose Version von Visual Studio finden Sie auf der Seite [Visual Studio-Downloads](https://www.visualstudio.com/downloads/download-visual-studio-vs) .

## <a name="move-a-database-into-an-elastic-pool"></a>Verschieben einer Datenbank in einen elastischen Pool
Sie können eine eigenständige Datenbank in einen Pool oder aus diesem verschieben.  

    // Retrieve current database properties.

    currentDatabase = sqlClient.Databases.Get("resourcegroup-name", "server-name", "Database1").Database;

    // Configure create or update parameters with existing property values, override those to be changed.
    DatabaseCreateOrUpdateParameters updatePooledDbParameters = new DatabaseCreateOrUpdateParameters()
    {
        Location = currentDatabase.Location,
        Properties = new DatabaseCreateOrUpdateProperties()
        {
            Edition = "Standard",
            RequestedServiceObjectiveName = "ElasticPool",
            ElasticPoolName = "ElasticPool1",
            MaxSizeBytes = currentDatabase.Properties.MaxSizeBytes,
            Collation = currentDatabase.Properties.Collation,
        }
    };

    // Update the database.
    var dbUpdateResponse = sqlClient.Databases.CreateOrUpdate("resourcegroup-name", "server-name", "Database1", updatePooledDbParameters);

## <a name="list-databases-in-an-elastic-pool"></a>Auflisten von Datenbanken in einem elastischen Pool
Um alle Datenbanken in einem Pool abzurufen, rufen Sie die [ListDatabases](https://msdn.microsoft.com/library/microsoft.azure.management.sql.elasticpooloperationsextensions.listdatabases) -Methode auf.

    //List databases in the elastic pool
    DatabaseListResponse dbListInPool = sqlClient.ElasticPools.ListDatabases("resourcegroup-name", "server-name", "ElasticPool1");
    Console.WriteLine("Databases in Elastic Pool {0}", "server-name.ElasticPool1");
    foreach (Database db in dbListInPool)
    {
        Console.WriteLine("  Database {0}", db.Name);
    }

## <a name="change-performance-settings-of-a-pool"></a>Ändern Sie die Leistungseinstellungen eines Pools
Rufen Sie die vorhandenen Pooleigenschaften ab. Ändern Sie die Werte, und führen Sie die CreateOrUpdate-Methode aus.

    var currentPool = sqlClient.ElasticPools.Get("resourcegroup-name", "server-name", "ElasticPool1").ElasticPool;

    // Configure create or update parameters with existing property values, override those to be changed.
    ElasticPoolCreateOrUpdateParameters updatePoolParameters = new ElasticPoolCreateOrUpdateParameters()
    {
        Location = currentPool.Location,
        Properties = new ElasticPoolCreateOrUpdateProperties()
        {
            Edition = currentPool.Properties.Edition,
            DatabaseDtuMax = 50, /* Setting DatabaseDtuMax to 50 limits the eDTUs that any one database can consume */
            DatabaseDtuMin = 10, /* Setting DatabaseDtuMin above 0 limits the number of databases that can be stored in the pool */
            Dtu = (int)currentPool.Properties.Dtu,
            StorageMB = currentPool.Properties.StorageMB,  /* For a Standard pool there is 1 GB of storage per eDTU. */
        }
    };

    newPoolResponse = sqlClient.ElasticPools.CreateOrUpdate("resourcegroup-name", "server-name", "ElasticPool1", newPoolParameters);


## <a name="latency-of-elastic-pool-operations"></a>Latenzzeit der elastischen Poolvorgänge
* Änderungen der minimalen oder maximalen Anzahl von eDTUs pro Datenbank werden in der Regel in höchstens fünf Minuten durchgeführt.
* Wie lange es dauert, die Poolgröße (eDTUs) zu ändern, hängt von der kombinierten Größe aller Datenbanken im Pool ab. Änderungen dauern durchschnittlich 90 Minuten oder weniger pro 100 GB. Wenn beispielsweise der gesamte Speicherplatz für alle Datenbanken im Pool 200 GB beträgt, ist für die Änderung der eDTUs pro Pool eine Latenzzeit von drei Stunden oder weniger zu erwarten.

## <a name="additional-resources"></a>Zusätzliche Ressourcen
* [SQL-Fehlercodes für SQL-Datenbank-Clientanwendungen: Datenbankverbindungsfehler und andere Probleme](sql-database-develop-error-messages.md).
* [SQL-Datenbank](https://azure.microsoft.com/documentation/services/sql-database/)
* [APIs für Azure-Ressourcenverwaltung](https://msdn.microsoft.com/library/azure/dn948464.aspx)
* [Erstellen eines neuen Pools für elastische Datenbanken mit C#](sql-database-elastic-pool-create-csharp.md)
* [Wo sollte ein Pool für elastische Datenbanken verwendet werden?](sql-database-elastic-pool-guidance.md)
* Unter [Horizontales Hochskalieren mit Azure SQL-Datenbank](sql-database-elastic-scale-introduction.md)finden Sie Informationen zur Verwendung elastischer Datenbanktools für die horizontale Hochskalierung, zum Verschieben von Daten, für die Abfrage oder zum Erstellen von Transaktionen.




<!--HONumber=Nov16_HO3-->


