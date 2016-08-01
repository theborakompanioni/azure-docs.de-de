<properties
    pageTitle="Konfigurieren der Georeplikation für Azure SQL-Datenbank mit Transact-SQL | Microsoft Azure"
    description="Konfigurieren der Georeplikation für Azure SQL-Datenbank mit Transact-SQL"
    services="sql-database"
    documentationCenter=""
    authors="carlrabeler"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.devlang="NA"
    ms.topic="article"
    ms.tgt_pltfrm="NA"
    ms.workload="sqldb-bcdr"
    ms.date="07/18/2016"
    ms.author="carlrab"/>

# Konfigurieren der Georeplikation für Azure SQL-Datenbank mit Transact-SQL

> [AZURE.SELECTOR]
- [Übersicht](sql-database-geo-replication-overview.md)
- [Azure-Portal](sql-database-geo-replication-portal.md)
- [PowerShell](sql-database-geo-replication-powershell.md)
- [T-SQL](sql-database-geo-replication-transact-sql.md)

Dieser Artikel beschreibt, wie Sie die aktive Georeplikation für eine Azure SQL-Datenbank mithilfe von Transact-SQL konfigurieren.

Informationen zum Initiieren eines Failovers mit Transact-SQL finden Sie unter [Initiieren eines geplanten oder ungeplanten Failovers für die Azure SQL-Datenbank mit Transact-SQL](sql-database-geo-replication-failover-transact-sql.md).

>[AZURE.NOTE] Die aktive Georeplikation (lesbare sekundäre Datenbanken) ist jetzt für alle Datenbanken in allen Diensttarifen verfügbar. Im April 2017 wird der nicht lesbare sekundäre Typ eingestellt, und vorhandene nicht lesbare Datenbanken werden automatisch auf lesbare sekundäre Datenbanken aktualisiert.

Wenn Sie die aktive Georeplikation mithilfe von Transact-SQL konfigurieren möchten, benötigen Sie Folgendes:

- Ein Azure-Abonnement.
- Einen logischen Azure SQL-Datenbankserver <MyLocalServer> und eine Azure SQL-Datenbank <MyDB>: Die primäre Datenbank, die Sie replizieren möchten.
- Ein oder mehrere logische Azure SQL-Datenbankserver <MySecondaryServer(n)>: Die logischen Server, die als Partnerserver fungieren, in denen Sie sekundäre Datenbanken erstellen.
- Eine Anmeldung mit der Berechtigung „DBManager“ für die primäre Datenbank, mit der Berechtigung „db\_ownership“ für die lokale Datenbank für die Georeplikation und der Berechtigung „DBManager“ für die Partnerserver, für die Sie die Georeplikation konfigurieren.
- SQL Server Management Studio (SSMS)

> [AZURE.IMPORTANT] Es wird empfohlen, immer die neueste Version von Management Studio zu verwenden, damit Sie mit Updates von Microsoft Azure und SQL-Datenbank synchron sind. [Aktualisieren Sie SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx).


## Hinzufügen einer sekundären Datenbank

Mithilfe der **ALTER DATABASE**-Anweisung können Sie auf einem Partnerserver eine georeplizierte sekundäre Datenbank erstellen. Sie führen diese Anweisung in der „master“-Datenbank des Servers mit der Datenbank aus, die repliziert werden soll. Die geografisch replizierte Datenbank (die „primäre Datenbank“) hat denselben Namen wie die zu replizierende Datenbank und standardmäßig dieselbe Dienstebene wie die primäre Datenbank. Die sekundäre Datenbank kann lesbar oder nicht lesbar und eine Einzel- oder elastische Datenbank sein. Weitere Informationen finden Sie unter [ALTER DATABASE (Transact-SQL)](https://msdn.microsoft.com/library/mt574871.aspx) und [Tarife](sql-database-service-tiers.md). Nachdem die sekundäre Datenbank erstellt und das Seeding ausgeführt wurde, beginnt die asynchrone Replikation aus der primären Datenbank. In den nachfolgenden Schritten wird beschrieben, wie die Georeplikation mithilfe von Management Studio konfiguriert wird. Schritte zum Erstellen von nicht lesbaren und lesbaren sekundären Datenbanken, entweder mit einer einzelnen oder einer elastischen Datenbank, sind angegeben.

> [AZURE.NOTE] Wenn auf dem angegebenen Partnerserver eine Datenbank mit dem gleichen Namen wie die primäre Datenbank vorhanden ist, wird durch den Befehl ein Fehler verursacht.


### Hinzufügen einer nicht lesbaren sekundären Datenbank (Einzeldatenbank)

Führen Sie zum Erstellen einer nicht lesbaren sekundären Datenbank als Einzeldatenbank die folgenden Schritte aus.

1. Verwenden von Version 13.0.600.65 oder höher von SQL Server Management Studio

 	 > [AZURE.IMPORTANT] Laden Sie die [aktuelle](https://msdn.microsoft.com/library/mt238290.aspx) Version von SQL Server Management Studio herunter. Es wird empfohlen, immer die neueste Version von Management Studio zu verwenden, damit Sie mit Updates des Azure-Portals synchron sind.


2. Öffnen Sie den Ordner „Datenbanken“, erweitern Sie den Ordner **Systemdatenbanken**, klicken Sie mit der rechten Maustaste auf **master**, und klicken Sie anschließend auf **Neue Abfrage**.

3. Verwenden Sie die folgende **ALTER DATABASE**-Anweisung, um eine lokale Datenbank in eine primäre Datenbank mit Georeplikation und nicht lesbarer sekundärer Datenbank auf „MySecondaryServer1“ zu ändern. Hierbei steht „MySecondaryServer1“ für den Anzeigenamen des Servers.

        ALTER DATABASE <MyDB>
           ADD SECONDARY ON SERVER <MySecondaryServer1> WITH (ALLOW_CONNECTIONS = NO);

4. Klicken Sie auf **Ausführen**, um die Abfrage durchzuführen.


### Hinzufügen einer lesbaren sekundären Datenbank (Einzeldatenbank)
Führen Sie zum Erstellen einer lesbaren sekundären Datenbank als Einzeldatenbank die folgenden Schritte aus:

1. Verbinden Sie sich in Management Studio mit dem logischen Azure SQL-Datenbankserver.

2. Öffnen Sie den Ordner „Datenbanken“, erweitern Sie den Ordner **Systemdatenbanken**, klicken Sie mit der rechten Maustaste auf **master**, und klicken Sie anschließend auf **Neue Abfrage**.

3. Verwenden Sie die folgende **ALTER DATABASE**-Anweisung, um eine lokale Datenbank als georeplizierte primäre Datenbank mit einer lesbaren sekundären Datenbank auf einem sekundären Server einzurichten.

        ALTER DATABASE <MyDB>
           ADD SECONDARY ON SERVER <MySecondaryServer2> WITH (ALLOW_CONNECTIONS = ALL);

4. Klicken Sie auf **Ausführen**, um die Abfrage durchzuführen.



### Hinzufügen einer nicht lesbaren sekundären Datenbank (elastische Datenbank)

Führen Sie zum Erstellen einer nicht lesbaren sekundären Datenbank als elastische Datenbank die folgenden Schritte aus:

1. Verbinden Sie sich in Management Studio mit dem logischen Azure SQL-Datenbankserver.

2. Öffnen Sie den Ordner „Datenbanken“, erweitern Sie den Ordner **Systemdatenbanken**, klicken Sie mit der rechten Maustaste auf **master**, und klicken Sie anschließend auf **Neue Abfrage**.

3. Verwenden Sie die folgende **ALTER DATABASE**-Anweisung, um eine lokale Datenbank als georeplizierte primäre Datenbank mit einer nicht lesbaren sekundären Datenbank auf einem sekundären Server in einem elastischen Pool einzurichten.

        ALTER DATABASE <MyDB>
           ADD SECONDARY ON SERVER <MySecondaryServer3> WITH (ALLOW_CONNECTIONS = NO
           , SERVICE_OBJECTIVE = ELASTIC_POOL (name = MyElasticPool1));

4. Klicken Sie auf **Ausführen**, um die Abfrage durchzuführen.



### Hinzufügen einer lesbaren sekundären Datenbank (elastische Datenbank)
Führen Sie zum Erstellen einer lesbaren sekundären Datenbank als elastische Datenbank die folgenden Schritte aus.

1. Verbinden Sie sich in Management Studio mit dem logischen Azure SQL-Datenbankserver.

2. Öffnen Sie den Ordner „Datenbanken“, erweitern Sie den Ordner **Systemdatenbanken**, klicken Sie mit der rechten Maustaste auf **master**, und klicken Sie anschließend auf **Neue Abfrage**.

3. Verwenden Sie die folgende **ALTER DATABASE**-Anweisung, um eine lokale Datenbank als georeplizierte primäre Datenbank mit einer lesbaren sekundären Datenbank auf einem sekundären Server in einem elastischen Pool einzurichten.

        ALTER DATABASE <MyDB>
           ADD SECONDARY ON SERVER <MySecondaryServer4> WITH (ALLOW_CONNECTIONS = ALL
           , SERVICE_OBJECTIVE = ELASTIC_POOL (name = MyElasticPool2));

4. Klicken Sie auf **Ausführen**, um die Abfrage durchzuführen.



## Entfernen einer sekundären Datenbank

Sie können mithilfe der **ALTER DATABASE**-Anweisung die Replikationspartnerschaft zwischen einer sekundären Datenbank und ihrer primären Datenbank dauerhaft beenden. Diese Anweisung wird für die „master“-Datenbank ausgeführt, in der sich die primäre Datenbank befindet. Nach dem Beenden der Beziehung wird die sekundäre Datenbank eine reguläre Datenbank mit Lese-/ Schreibzugriff. Wenn die Verbindung mit der sekundären Datenbank unterbrochen wird, ist der Befehl zwar erfolgreich, aber die sekundäre Datenbank wird erst mit Lese-/ Schreibzugriff versehen, nachdem die Verbindung wiederhergestellt wurde. Weitere Informationen finden Sie unter [ALTER DATABASE (Transact-SQL)](https://msdn.microsoft.com/library/mt574871.aspx) und [Tarife](sql-database-service-tiers.md).

Führen Sie zum Entfernen der sekundären Datenbank aus der Georeplikationsbeziehung folgende Schritte aus.

1. Verbinden Sie sich in Management Studio mit dem logischen Azure SQL-Datenbankserver.

2. Öffnen Sie den Ordner „Datenbanken“, erweitern Sie den Ordner **Systemdatenbanken**, klicken Sie mit der rechten Maustaste auf **master**, und klicken Sie anschließend auf **Neue Abfrage**.

3. Verwenden Sie die folgende **ALTER DATABASE** -Anweisung zum Entfernen einer georeplizierten sekundären Datenbank.

        ALTER DATABASE <MyDB>
           REMOVE SECONDARY ON SERVER <MySecondaryServer1>;

4. Klicken Sie auf **Ausführen**, um die Abfrage durchzuführen.

## Überprüfen der Konfiguration und Integrität der Georeplikation

Zu den Überwachungsaufgaben gehören die Überwachung der Konfiguration der Georeplikation und der Integrität der Datenreplikation. Sie können die dynamische Verwaltungssicht **sys.dm\_geo\_replication\_links** in der Masterdatenbank nutzen, um Informationen zu allen vorhandenen Replikationsverknüpfungen für alle Datenbanken auf dem logischen Azure SQL-Datenbankserver abzurufen. Diese Ansicht enthält eine Zeile für jede Replikationsverknüpfung zwischen primären und sekundären Datenbanken. Sie können die dynamische Verwaltungssicht **sys.dm\_replication\_status** verwenden, um eine Zeile für jede Azure SQL-Datenbank zurückzugeben, die derzeit an einer Replikationsverknüpfung beteiligt ist. Dies schließt primäre und sekundäre Datenbanken ein. Wenn für eine bestimmte primäre Datenbank mehrere Verknüpfungen für die fortlaufende Replikation vorhanden sind, enthält die Tabelle eine Zeile für jede der Beziehungen. Die Sicht wird in allen Datenbanken, einschließlich der logischen „master“-Datenbank, erstellt. Doch bei Abfragen dieser Sicht in der logischen „master“-Datenbank wird ein leeres Ergebnis zurückgegeben. Sie können mithilfe der Verwaltungssicht **sys.dm\_operation\_status** den Status aller Datenbankvorgänge anzeigen, einschließlich des Status der Replikationsverknüpfungen. Weitere Informationen finden Sie unter [sys.dm\_geo\_replication\_links (Azure SQL-Datenbank)](https://msdn.microsoft.com/library/mt575501.aspx), [sys.dm\_geo\_replication\_link\_status (Azure SQL-Datenbank)](https://msdn.microsoft.com/library/mt575504.aspx) und [sys.dm\_operation\_status (Azure SQL-Datenbank)](https://msdn.microsoft.com/library/dn270022.aspx).

Gehen Sie folgendermaßen vor, um eine Georeplikationspartnerschaft zu überwachen.

1. Verbinden Sie sich in Management Studio mit dem logischen Azure SQL-Datenbankserver.

2. Öffnen Sie den Ordner „Datenbanken“, erweitern Sie den Ordner **Systemdatenbanken**, klicken Sie mit der rechten Maustaste auf **master**, und klicken Sie anschließend auf **Neue Abfrage**.

3. Verwenden Sie die folgende Anweisung, um alle Datenbanken mit Georeplikationsverknüpfungen anzuzeigen.

        SELECT database_id, start_date, modify_date, partner_server, partner_database, replication_state_desc, role, secondary_allow_connections_desc FROM [sys].geo_replication_links;

4. Klicken Sie auf **Ausführen**, um die Abfrage durchzuführen.
5. Öffnen Sie den Ordner „Datenbanken“, erweitern Sie den Ordner **Systemdatenbanken**, klicken Sie mit der rechten Maustaste auf **MyDM**, und klicken Sie anschließend auf **Neue Abfrage**.
6. Zeigen Sie mithilfe der folgenden Anweisung die Replikationsverzögerungen und den Zeitpunkt der letzten Replikation Ihrer eigenen sekundären Datenbanken von MyDB an.

        SELECT link_guid, partner_server, last_replication, replication_lag_sec FROM sys.dm_geo_replication_link_status

7. Klicken Sie auf **Ausführen**, um die Abfrage durchzuführen.
8. Verwenden Sie die folgende Anweisung, um die neuesten Georeplikationsvorgänge anzuzeigen, die der Datenbank MyDB zugeordnet sind.

        SELECT * FROM sys.dm_operation_status where major_resource_id = 'MyDB'
        ORDER BY start_time DESC

9. Klicken Sie auf **Ausführen**, um die Abfrage durchzuführen.

## Aktualisieren einer nicht lesbaren sekundären Datenbank auf „lesbar“

Im April 2017 wird der nicht lesbare sekundäre Typ eingestellt, und vorhandene nicht lesbare Datenbanken werden automatisch auf lesbare sekundäre Datenbanken aktualisiert. Wenn Sie heute nicht lesbare sekundäre Datenbanken verwenden und auf lesbare Datenbanken aktualisieren möchten, können Sie die folgenden einfachen Schritte für jede sekundäre Datenbank ausführen.

> [AZURE.IMPORTANT] Es gibt keine Self-Service-Methode der direkten Aktualisierung einer nicht lesbaren sekundären Datenbank auf „lesbar“. Wenn Sie die einzige sekundäre Datenbank löschen, bleibt die primäre Datenbank ungeschützt, bis die neue sekundäre Datenbank vollständig synchronisiert ist. Wenn die SLA Ihrer Anwendung erfordert, dass die primäre Datenbank immer geschützt ist, sollten Sie das Erstellen einer parallelen sekundären Datenbank auf einem anderen Server erwägen, bevor Sie die oben genannten Schritte anwenden. Beachten Sie, dass jede primäre Datenbank bis zu 4 sekundäre Datenbanken haben kann.


1. Stellen Sie zunächst eine Verbindung mit dem *sekundären* Server her, und löschen Sie die nicht lesbare sekundäre Datenbank:
        
        DROP DATABASE <MyNonReadableSecondaryDB>;

2. Stellen Sie jetzt eine Verbindung mit dem *primären* Server her, und fügen Sie eine neue lesbare sekundäre Datenbank hinzu.

        ALTER DATABASE <MyDB>
            ADD SECONDARY ON SERVER <MySecondaryServer> WITH (ALLOW_CONNECTIONS = ALL);




## Nächste Schritte

- Weitere Informationen zur aktiven Georeplikation finden Sie unter [Aktive Georeplikation](sql-database-geo-replication-overview.md).
- Informationen über Entwurfs- und Wiederherstellungsszenarien für die Geschäftskontinuität finden Sie unter [Geschäftskontinuitätsszenarien](sql-database-business-continuity-scenarios.md).

<!---HONumber=AcomDC_0720_2016-->