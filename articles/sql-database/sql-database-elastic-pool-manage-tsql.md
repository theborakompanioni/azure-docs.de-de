<properties 
    pageTitle="Erstellen oder Verschieben einer Azure SQL-Datenbank in einen elastischen Pool mit T-SQL | Microsoft Azure" 
    description="Es wird beschrieben, wie Sie T-SQL verwenden, um eine Azure SQL-Datenbank in einem elastischen Pool zu erstellen. Außerdem können Sie T-SQL nutzen, um die Datenbank in Pools bzw. aus Pools zu verschieben." 
	services="sql-database" 
    documentationCenter="" 
    authors="sidneyh" 
    manager="jhubbard" 
    editor=""/>

<tags
    ms.service="sql-database"
    ms.devlang="NA"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="NA"
    ms.workload="data-management" 
    ms.date="04/01/2016"
    ms.author="sidneyh"/>

# Überwachen und Verwalten eines Pools für elastische Datenbanken per Transact-SQL  

> [AZURE.SELECTOR]
- [Azure-Portal](sql-database-elastic-pool-manage-portal.md)
- [C#](sql-database-elastic-pool-manage-csharp.md)
- [PowerShell](sql-database-elastic-pool-manage-powershell.md)
- [T-SQL](sql-database-elastic-pool-manage-tsql.md)

Verwenden Sie die Befehle [Create Database (Azure SQL-Datenbank)](https://msdn.microsoft.com/library/dn268335.aspx) und [Alter Database(Azure SQL-Datenbank)](https://msdn.microsoft.com/library/mt574871.aspx), um Datenbanken zu erstellen und in elastische Pools bzw. aus elastischen Pools zu verschieben. Der elastische Pool muss vorhanden sein, bevor Sie diese Befehle verwenden können. Diese Befehle wirken sich nur auf Datenbanken aus. Die Erstellung eines neuen Pools und die Einstellung der Pooleigenschaften (z.B. min. und max. eDTUs) kann mit T-SQL-Befehlen nicht geändert werden.


> [AZURE.NOTE] Pools für elastische Datenbanken sind derzeit als Vorschauversion ausschließlich für Server mit SQL-Datenbank V12 verfügbar. Wenn Sie über einen SQL-Datenbank V11-Server verfügen, können Sie in einem Schritt [mithilfe von PowerShell auf V12 aktualisieren und einen Pool erstellen](sql-database-upgrade-server-portal.md).


## Erstellen einer neuen Datenbank in einem elastischen Pool
Verwenden Sie den Befehl CREATE DATABASE mit der Option SERVICE\_OBJECTIVE.

	CREATE DATABASE db1 ( SERVICE_OBJECTIVE = ELASTIC_POOL (name = [S3M100] ));
	-- Create a database named db1 in a pool named S3M100.

Alle Datenbanken in einem elastischen Pool erben die Dienstebene des elastischen Pools (Basic, Standard, Premium).


## Verschieben einer Datenbank zwischen elastischen Pools
Verwenden Sie den Befehl ALTER DATABASE mit MODIFY, und legen Sie die Option SERVICE\_OBJECTIVE als ELASTIC\_POOL fest. Legen Sie den Namen auf den Namen des Zielpools fest.

	ALTER DATABASE db1 MODIFY ( SERVICE_OBJECTIVE = ELASTIC_POOL (name = [PM125] ));
	-- Move the database named db1 to a pool named P1M125  


## Verschieben einer Datenbank in einen elastischen Pool 
Verwenden Sie den Befehl ALTER DATABASE mit MODIFY, und legen Sie die Option SERVICE\_OBJECTIVE als ELASTIC\_POOL fest. Legen Sie den Namen auf den Namen des Zielpools fest.

	ALTER DATABASE db1 MODIFY ( SERVICE_OBJECTIVE = ELASTIC_POOL (name = [S3100] ));
	-- Move the database named db1 to a pool named S3100.

## Verschieben einer Datenbank aus einem elastischen Pool
Verwenden Sie den Befehl ALTER DATABASE, und legen Sie SERVICE\_OBJECTIVE auf eine Leistungsebene fest (S0, S1 usw.).

	ALTER DATABASE db1 MODIFY ( SERVICE_OBJECTIVE = 'S1');
	-- Changes the database into a stand-alone database with the service objective S1.

## Auflisten von Datenbanken in einem elastischen Pool
Verwenden Sie die Sicht [sys.database\_service\_objectives](https://msdn.microsoft.com/library/mt712619), um alle Datenbanken in einem elastischen Pool aufzulisten. Melden Sie sich an der Masterdatenbank an, um die Sicht abzufragen.

>[AZURE.NOTE] Derzeit gibt „service\_objective\_column“ für Datenbanken in elastischen Pools ein internes Token der Dienstziel-Zeichenfolge zurück. Es wird durch die Zeichenfolge „ElasticPool“ ersetzt.
>

	SELECT d.name, slo.*  
	FROM sys.databases d 
	JOIN sys.database_service_objectives slo  
	ON d.database_id = slo.database_id
	WHERE elastic_pool_name = 'MyElasticPool'; 

## Überwachen der Ressourcenauslastung von elastischen Pools

Verwenden Sie die Sicht [sys.elastic\_pool\_resource\_stats](https://msdn.microsoft.com/library/mt280062.aspx), um die Statistik zur Ressourcenauslastung eines elastischen Pools auf einem logischen Server zu untersuchen. Melden Sie sich an der Masterdatenbank an, um die Sicht abzufragen.

	SELECT * FROM sys.elastic_pool_resource_stats 
	WHERE elastic_pool_name = 'MyElasticPool'
	ORDER BY end_time DESC;

## Überwachen der Ressourcenauslastung einer Datenbank in einem elastischen Pool
Verwenden Sie die Sicht [sys.dm\_resource\_stats](https://msdn.microsoft.com/library/dn800981.aspx) oder [sys.resource\_stats](https://msdn.microsoft.com/library/dn269979.aspx), um die Statistik zur Ressourcenauslastung einer Datenbank in einem elastischen Pool zu untersuchen. Dieser Prozess ähnelt dem Abfragen der Ressourcenauslastung für eine beliebige Einzeldatenbank.

## Nächste Schritte

Nach dem Erstellen eines Pools für elastische Datenbanken können Sie elastische Datenbanken im Pool mit elastischen Aufträgen verwalten. Elastische Aufträge erleichtern die Ausführung von T-SQL-Skripts für eine beliebige Anzahl von Datenbanken im Pool. Weitere Informationen finden Sie unter [Übersicht über elastische Datenbankaufträge](sql-database-elastic-jobs-overview.md).

<!---HONumber=AcomDC_0406_2016-->