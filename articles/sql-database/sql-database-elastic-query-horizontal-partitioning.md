<properties
    pageTitle="Horizontal hochskalierte Clouddatenbanken übergreifende Berichte | Microsoft Azure"
    description="Informationen zum Einrichten elastischer Abfragen, die horizontal partitionierte Datenbanken übergreifen"    
    services="sql-database"
    documentationCenter=""  
    manager="jhubbard"
    authors="torsteng"/>

<tags
    ms.service="sql-database"
    ms.workload="sql-database"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="04/26/2016"
    ms.author="torsteng;sidneyh" />

# Horizontal hochskalierte Clouddatenbanken übergreifende Berichte (Vorschau)

![Abfrage über Shards hinweg][1]

Horizontal partitionierte Datenbanken verteilen Zeilen auf einer horizontal hochskalierten Datenebene. Das Schema ist auf allen teilnehmenden Datenbanken identisch, auch bekannt als horizontale Partitionierung. Mit einer flexiblen Abfrage können Sie Berichte erstellen, die alle Datenbanken in einer horizontal partitionierten Datenbank umfassen.

Wie Sie einen Schnellstart durchführen, erfahren Sie unter [Reporting across scaled-out cloud databases](sql-database-elastic-query-getting-started.md) (Horizontal hochskalierte Clouddatenbanken übergreifende Berichte).

Informationen für nicht partitionierte Datenbanken finden Sie unter [Queries across sharded cloud databases (vertically partitioned)](sql-database-elastic-query-vertical-partitioning.md) (Partitionierte Clouddatenbanken [vertikal partitioniert] übergreifende Abfragen).

 
## Voraussetzungen

* Erstellen Sie eine Shardzuordnung mithilfe der Clientbibliothek für elastische Datenbanken (siehe [Shard-Zuordnungsverwaltung](sql-database-elastic-scale-shard-map-management.md)). Oder verwenden Sie die Beispiel-App in [Erste Schritte mit Tools für elastische Datenbanken](sql-database-elastic-scale-get-started.md).
* Alternativ dazu können Sie [Migrate existing databases to scaled-out databases](sql-database-elastic-convert-to-use-elastic-tools.md) (Migrieren vorhandener Datenbanken zu horizontal hochskalierten Datenbanken) lesen.
* Der Benutzer muss über die Berechtigung ALTER ANY EXTERNAL DATA SOURCE verfügen. Diese Berechtigung ist in der Berechtigung ALTER DATABASE enthalten.
* ALTER ANY EXTERNAL DATA SOURCE-Berechtigungen sind erforderlich, um auf die zu Grunde liegende Datenquelle zu verweisen.

## Übersicht

Diese Anweisungen erstellen die Metadatendarstellung Ihrer Shardingdatenebene in der elastischen Abfragedatenbank.


1. [CREATE MASTER KEY](https://msdn.microsoft.com/library/ms174382.aspx)
2. [CREATE DATABASE SCOPED CREDENTIAL](https://msdn.microsoft.com/library/mt270260.aspx)
3. [CREATE EXTERNAL DATA SOURCE](https://msdn.microsoft.com/library/dn935022.aspx)
4. [CREATE EXTERNAL TABLE](https://msdn.microsoft.com/library/dn935021.aspx) 

## 1\.1 Erstellen des Datenbankhauptschlüssels und der Anmeldeinformationen 

Die Anmeldeinformationen werden von der elastischen Abfrage für die Verbindung mit Ihren Remotedatenbanken verwendet.

    CREATE MASTER KEY ENCRYPTION BY PASSWORD = 'password';
    CREATE DATABASE SCOPED CREDENTIAL <credential_name>  WITH IDENTITY = '<username>',  
    SECRET = '<password>'
    [;]
 
**Hinweis** Stellen Sie sicher, dass *<username>* kein *@servername*-Suffix enthält.

## 1\.2 Erstellen externer Datenquellen

Syntax:

	<External_Data_Source> ::=    
	CREATE EXTERNAL DATA SOURCE <data_source_name> WITH                               	           
			(TYPE = SHARD_MAP_MANAGER,
                   	LOCATION = '<fully_qualified_server_name>',
			DATABASE_NAME = ‘<shardmap_database_name>',
			CREDENTIAL = <credential_name>, 
			SHARD_MAP_NAME = ‘<shardmapname>’ 
                   ) [;] 

### Beispiel 

	CREATE EXTERNAL DATA SOURCE MyExtSrc 
	WITH 
	( 
		TYPE=SHARD_MAP_MANAGER,
		LOCATION='myserver.database.windows.net', 
		DATABASE_NAME='ShardMapDatabase', 
		CREDENTIAL= SMMUser, 
		SHARD_MAP_NAME='ShardMap' 
	);
 
Rufen Sie die Liste der aktuellen externen Datenquellen ab:

	select * from sys.external_data_sources; 

Die externe Datenquelle verweist auf Ihre Shardzuordnung. Eine elastische Abfrage verwendet anschließend die externe Datenquelle und zugrunde liegende Shardzuordnung zum Auflisten der Datenbanken, die zur Datenebene gehören. Die gleichen Anmeldeinformationen werden während der Verarbeitung der elastischen Abfrage zum Lesen der Shardzuordnung und für den Zugriff auf die Daten in den Shards verwendet .

## 1\.3 Erstellen externer Tabellen 
 
Syntax:

	CREATE EXTERNAL TABLE [ database_name . [ schema_name ] . | schema_name. ] table_name  
        ( { <column_definition> } [ ,...n ])     
	    { WITH ( <sharded_external_table_options> ) }
	) [;]  
	
	<sharded_external_table_options> ::= 
      DATA_SOURCE = <External_Data_Source>,       
	  [ SCHEMA_NAME = N'nonescaped_schema_name',] 
      [ OBJECT_NAME = N'nonescaped_object_name',] 
      DISTRIBUTION = SHARDED(<sharding_column_name>) | REPLICATED |ROUND_ROBIN

**Beispiel**

	CREATE EXTERNAL TABLE [dbo].[order_line]( 
		 [ol_o_id] int NOT NULL, 
		 [ol_d_id] tinyint NOT NULL,
		 [ol_w_id] int NOT NULL, 
		 [ol_number] tinyint NOT NULL, 
		 [ol_i_id] int NOT NULL, 
		 [ol_delivery_d] datetime NOT NULL, 
		 [ol_amount] smallmoney NOT NULL, 
		 [ol_supply_w_id] int NOT NULL, 
		 [ol_quantity] smallint NOT NULL, 
		 [ol_dist_info] char(24) NOT NULL 
	) 
	
	WITH 
	( 
		DATA_SOURCE = MyExtSrc, 
	 	SCHEMA_NAME = 'orders', 
	 	OBJECT_NAME = 'order_details', 
		DISTRIBUTION=SHARDED(ol_w_id)
	); 

Rufen Sie die Liste der externen Tabellen aus der aktuellen Datenbank ab:

	SELECT * from sys.external_tables; 

So löschen Sie externe Tabellen:

	DROP EXTERNAL TABLE [ database_name . [ schema_name ] . | schema_name. ] table_name[;]

### Anmerkungen

Die DATA\_SOURCE-Klausel definiert die externe Datenquelle (eine Shardzuordnung), die für die externe Tabelle verwendet wird.

Die Klauseln SCHEMA\_NAME und OBJECT\_NAME ordnen die Definition der externen Tabelle einer Tabelle in einem anderen Schema zu. Falls nicht angegeben, wird davon ausgegangen, dass das Schema des Remoteobjekts „dbo“ und sein Name mit dem definierten Namen der externen Tabelle identisch ist. Dies ist nützlich, wenn der Name der Remotetabelle bereits in der Datenbank verwendet wird, in der Sie die externe Tabelle erstellen möchten. Sie möchten z.B. eine externe Tabelle zum Abrufen einer aggregrierten Sicht von Katalogsichten oder DMVs für Ihre horizontal hochskalierte Datenebene definieren. Da Katalogsichten und DMVs bereits lokal vorhanden sind, können Sie ihre Namen nicht für die Definition der externen Tabelle verwenden. Verwenden Sie stattdessen in den Klauseln SCHEMA\_NAME und/oder OBJECT\_NAME einen anderen Namen und den Namen der Katalogsicht oder DMV. (Betrachten Sie das folgende Beispiel.)

Die DISTRIBUTION-Klausel gibt die Datenverteilung für diese Tabelle an: Der Abfrageprozessor nutzt die Informationen in der DISTRIBUTION-Klausel, um die effizientesten Abfragepläne zu erstellen.

1. **SHARDED** bedeutet, dass Daten Datenbanken übergreifend horizontal partitioniert werden. Der Partitionierungsschlüssel für die Datenverteilung wird im **<sharding_column_name>**-Parameter erfasst.
2. **REPLICATED** bedeutet, dass identische Kopien der Tabelle in jeder Datenbank vorhanden sind. Sie müssen sicherstellen, dass die Replikate in allen Datenbanken identisch sind.
3. **ROUND\_ROBIN** bedeutet, dass die Tabelle mit einer anwendungsabhängigen Verteilungsmethode horizontal partitioniert wurde. 

**Datenebenenverweis**: Die DDL für externe Tabellen verweist auf eine externe Datenquelle. Die externe Datenquelle gibt eine Shardzuordnung an, die der externen Tabelle die Informationen bereitstellt, die benötigt werden, um alle Datenbanken in Ihrer Datenebene zu finden.


### Sicherheitshinweise 

Benutzer mit Zugriff auf die externe Tabelle erhalten automatisch Zugriff auf die zugrunde liegenden Remotetabellen gemäß den Anmeldeinformationen, die in der externen Datenquellendefinition angegeben sind. Vermeiden Sie eine unerwünschte Erhöhung von Berechtigungen durch die Anmeldeinformationen der externen Datenquelle. Verwenden Sie GRANT oder REVOKE für eine externe Tabelle, als handele es sich um eine normale Tabelle.

Nachdem Sie die externe Datenquelle und die externen Tabellen definiert haben, können Sie jetzt vollständiges T-SQL in den externen Tabellen verwenden.

## Beispiel: Abfragen von horizontal partitionierten Datenbanken 

Die folgende Abfrage führt eine Verknüpfung in drei Richtungen zwischen Lagern, Bestellungen und Auftragspositionen aus. Sie nutzt mehrere Aggregate und einen selektiven Filter. Es wird Folgendes vorausgesetzt: 1.) eine horizontale Partitionierung, 2.) dass für Lager, Aufträge und Auftragspositionen ein Sharding anhand der Spalte „warehouse id“ erfolgt ist, und 3.) dass die elastische Abfrage die Verknüpfungen für die Shards anordnen und den aufwendigen Teil der Abfrage in den Shards parallel verarbeiten kann.

	select  
		 w_id as warehouse,
		 o_c_id as customer,
		 count(*) as cnt_orderline,
		 max(ol_quantity) as max_quantity,
		 avg(ol_amount) as avg_amount, 
		 min(ol_delivery_d) as min_deliv_date
	from warehouse 
	join orders 
	on w_id = o_w_id
	join order_line 
	on o_id = ol_o_id and o_w_id = ol_w_id 
	where w_id > 100 and w_id < 200 
	group by w_id, o_c_id 
 
## Gespeicherte Prozedur für T-SQL-Remoteausführung: sp\_execute\_remote

Mit der elastischen Abfrage wurde auch eine gespeicherte Prozedur eingeführt, die einen Direktzugriff auf die Shards bietet. Die gespeicherte Prozedur hat den Namen **sp\_execute\_remote** und kann verwendet werden, um remote gespeicherte Prozeduren oder T-SQL-Code in den Remotedatenbanken auszuführen. Hierfür werden die folgenden Parameter verwendet:

* Datenquellenname (nvarchar): Der Name der externen Datenquelle vom Typ RDBMS. 
* Abfrage (nvarchar): Die T-SQL-Abfrage, die für die einzelnen Shards ausgeführt wird. 
* Parameterdeklaration (nvarchar) – optional: Zeichenfolge mit Datentypdefinitionen für die Parameter, die im „Query“-Parameter verwendet werden (z. B. sp\_executesql). 
* Parameterwertliste – optional: Durch Trennzeichen getrennte Liste von Parameterwerten (z.B. sp\_executesql).

Für „sp\_execute\_remote“ wird die externe Datenquelle verwendet, die in den Aufrufparametern angegeben ist, um die jeweilige T-SQL-Anweisung in den Remotedatenbanken auszuführen. Die Anmeldeinformationen der externen Datenquelle werden verwendet, um die Verbindung mit der ShardMapManager-Datenbank und den Remotedatenbanken herzustellen.

Beispiel:

	EXEC sp_execute_remote
		N'MyExtSrc',
		N'select count(w_id) as foo from warehouse' 

## Konnektivität für Tools  

Verwenden Sie herkömmliche SQL Server-Verbindungszeichenfolgen, um Ihre Anwendung sowie Ihre BI- und Datenintegrationstools für die Datenbank mit Ihren Definitionen externer Tabellen zu verbinden. Stellen Sie sicher, dass SQL Server als Datenquelle für das Tool unterstützt wird. Verweisen Sie dann auf die elastische Abfragedatenbank wie auf beliebige andere mit dem Tool verbundenen SQL Server-Datenbanken, und nutzen Sie externe Tabellen in Ihren Tools oder Anwendungen, als wären es lokale Tabellen.

## Bewährte Methoden 

* Stellen Sie sicher, dass die Endpunktdatenbank für elastische Abfragen durch die Firewalls von Azure SQL-Datenbank hindurch Zugriff auf die Datenbank mit der Shardzuordnung und alle Shards hat.  

* Überprüfen oder erzwingen Sie die in der externen Tabelle definierte Verteilung der Daten. Wenn Ihre tatsächliche Datenverteilung sich von der Verteilung in der Tabellendefinition unterscheidet, können Ihre Abfragen zu unerwarteten Ergebnissen führen.

* Die elastische Abfrage führt derzeit keine Shardlöschung durch, wenn Prädikate für Shardingschlüssel ein gefahrloses Ausschließen der Verarbeitung bestimmter Shards zulassen würden.

* Eine elastische Abfrage funktioniert am besten für Abfragen, in denen der größte Teil der Berechnung in den Shards erfolgt. In der Regel erhalten Sie optimale Abfrageleistung mit benutzerdefinierten Filterprädikaten, die in den Shards oder Verknüpfungen über die Partitionierungsschlüssel ausgewertet werden können, die auf partitionsbezogene Weise auf allen Shards ausgeführt werden können. Für andere Abfragemuster müssen möglicherweise große Mengen von Daten aus den Shards auf den Hauptknoten geladen werden, wodurch Leistungseinbußen auftreten.

[AZURE.INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]

<!--Image references-->
[1]: ./media/sql-database-elastic-query-horizontal-partitioning/horizontalpartitioning.png
<!--anchors-->

<!---HONumber=AcomDC_0427_2016-->