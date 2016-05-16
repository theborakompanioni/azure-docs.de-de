<properties
    pageTitle="Konfigurieren der Georeplikation für Azure SQL-Datenbank mit Transact-SQL | Microsoft Azure"
    description="Konfigurieren der Georeplikation für Azure SQL-Datenbank mithilfe von Transact-SQL"
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
    ms.workload="data-management"
    ms.date="04/27/2016"
    ms.author="carlrab"/>

# Konfigurieren der Georeplikation für Azure SQL-Datenbank mit Transact-SQL



> [AZURE.SELECTOR]
- [Azure-Portal](sql-database-geo-replication-portal.md)
- [PowerShell](sql-database-geo-replication-powershell.md)
- [Transact-SQL](sql-database-geo-replication-transact-sql.md)


Dieser Artikel beschreibt, wie Sie die Georeplikation für eine Azure SQL-Datenbank mithilfe von Transact-SQL konfigurieren.

Informationen zum Initiieren eines Failovers finden Sie unter [Initiieren eines geplanten oder ungeplanten Failovers für die Azure SQL-Datenbank](sql-database-geo-replication-failover-transact-sql.md).

>[AZURE.NOTE] Die aktive Georeplikation (lesbare sekundäre Datenbanken) ist jetzt für alle Datenbanken in allen Diensttarifen verfügbar. Im April 2017 wird der nicht lesbare sekundäre Typ eingestellt, und vorhandene nicht lesbare Datenbanken werden automatisch auf lesbare sekundäre Datenbanken aktualisiert.

Sie können bis zu vier lesbare sekundäre Datenbanken in derselben oder in verschiedenen Standorten von Rechenzentren (Regionen) konfigurieren. Sekundäre Datenbanken stehen zur Verfügung, wenn ein Datencenter ausgefallen ist oder keine Verbindung mit der primären Datenbank möglich ist.


Zum Konfigurieren der Georeplikation benötigen Sie Folgendes:

- Ein Azure-Abonnement: Wenn Sie kein Azure-Abonnement haben, klicken Sie einfach oben auf dieser Seite auf den Link **KOSTENLOSE TESTVERSION** und kehren dann zu diesem Artikel zurück.
- Einen logische Azure SQL-Datenbankserver <MyLocalServer> und eine Azure SQL-Datenbank <MyDB>: Die primäre Datenbank, die in eine andere geografische Region repliziert werden soll.
- Ein oder mehrere logische Azure SQL-Datenbankserver <MySecondaryServer(n)>: Die logischen Server, die als Partnerserver fungieren, in denen Sie sekundäre Datenbanken erstellen.
- Eine Anmeldung mit der Berechtigung „DBManager“ für die primäre Datenbank, mit der Berechtigung „db\_ownership“ für die lokale Datenbank für die Georeplikation und der Berechtigung „DBManager“ für die Partnerserver, für die Sie die Georeplikation konfigurieren.
- Neueste Version von SQL Server Management Studio: Die neueste Version von SQL Server Management Studio (SSMS) finden Sie unter [Herunterladen von SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx). Informationen zur Verwendung von SQL Server Management Studio zum Verwalten von logischen Azure SQL-Datenbankservern und Datenbanken finden Sie unter [Verwalten von Azure SQL-Datenbank mit SQL Server Management Studio](sql-database-manage-azure-ssms.md).

## Hinzufügen einer sekundären Datenbank

Mithilfe der **ALTER DATABASE**-Anweisung können Sie auf einem Partnerserver eine georeplizierte sekundäre Datenbank erstellen. Sie führen diese Anweisung in der „master“-Datenbank des Servers mit der Datenbank aus, die repliziert werden soll. Die geografisch replizierte Datenbank (die „primäre Datenbank“) hat denselben Namen wie die zu replizierende Datenbank und standardmäßig dieselbe Dienstebene wie die primäre Datenbank. Die sekundäre Datenbank kann lesbar oder nicht lesbar und eine Einzel- oder elastische Datenbank sein. Weitere Informationen finden Sie unter [ALTER DATABASE (Transact-SQL)](https://msdn.microsoft.com/library/mt574871.aspx) und [Tarife](sql-database-service-tiers.md). Nachdem die sekundäre Datenbank erstellt und das Seeding ausgeführt wurde, beginnt die asynchrone Replikation aus der primären Datenbank. In den nachfolgenden Schritten wird beschrieben, wie die Georeplikation mithilfe von Management Studio konfiguriert wird. Schritte zum Erstellen von nicht lesbaren und lesbaren sekundären Datenbanken, entweder mit einer einzelnen oder einer elastischen Datenbank, sind angegeben.

> [AZURE.NOTE] Wenn auf dem angegebenen Partnerserver eine Datenbank mit dem gleichen Namen wie die primäre Datenbank vorhanden ist, wird durch den Befehl ein Fehler verursacht.


### Hinzufügen einer nicht lesbaren sekundären Datenbank (Einzeldatenbank)

Führen Sie zum Erstellen einer nicht lesbaren sekundären Datenbank als Einzeldatenbank die folgenden Schritte aus.

1. Verwenden von Version 13.0.600.65 oder höher von SQL Server Management Studio

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



## Nächste Schritte

- [Initiieren eines geplanten oder ungeplanten Failovers für die Azure SQL-Datenbank](sql-database-geo-replication-failover-transact-sql.md)
- [Warnungen zur Notfallwiederherstellung](sql-database-disaster-recovery-drills.md)


## Zusätzliche Ressourcen

- [Sicherheitskonfiguration für die Georeplikation](sql-database-geo-replication-security-config.md)
- [Spotlight auf die neuen Georeplikationsfunktionen](https://azure.microsoft.com/blog/spotlight-on-new-capabilities-of-azure-sql-database-geo-replication/)
- [BCDR in SQL-Datenbank – Häufig gestellte Fragen](sql-database-bcdr-faq.md)
- [Übersicht über die Geschäftskontinuität](sql-database-business-continuity.md)
- [Aktive Georeplikation](sql-database-geo-replication-overview.md)
- [Entwerfen einer Anwendung für die cloudbasierte Notfallwiederherstellung](sql-database-designing-cloud-solutions-for-disaster-recovery.md)
- [Abschließen der wiederhergestellten Azure SQL-Datenbank](sql-database-recovered-finalize.md)

<!---HONumber=AcomDC_0504_2016-->