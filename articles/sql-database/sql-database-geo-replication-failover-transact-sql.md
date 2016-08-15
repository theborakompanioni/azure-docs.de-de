<properties 
    pageTitle="Initiieren eines geplanten oder ungeplanten Failovers für die Azure SQL-Datenbank mit Transact-SQL | Microsoft Azure" 
    description="Initiieren eines geplanten oder ungeplanten Failovers für die Azure SQL-Datenbank mit Transact-SQL" 
    services="sql-database" 
    documentationCenter="" 
    authors="CarlRabeler" 
    manager="jhubbard" 
    editor=""/>

<tags
    ms.service="sql-database"
    ms.devlang="NA"
    ms.topic="article"
    ms.tgt_pltfrm="NA"
    ms.workload="data-management"
    ms.date="07/19/2016"
    ms.author="carlrab"/>

# Initiieren eines geplanten oder ungeplanten Failovers für die Azure SQL-Datenbank mit Transact-SQL


> [AZURE.SELECTOR]
- [Azure-Portal](sql-database-geo-replication-failover-portal.md)
- [PowerShell](sql-database-geo-replication-failover-powershell.md)
- [T-SQL](sql-database-geo-replication-failover-transact-sql.md)


In diesem Artikel wird beschrieben, wie Sie mit Transact-SQL ein Failover zu einer sekundären SQL-Datenbank konfigurieren. Informationen zum Konfigurieren der Georeplikation finden Sie unter [Konfigurieren der Georeplikation für Azure SQL-Datenbank mit Transact-SQL](sql-database-geo-replication-transact-sql.md).



Zum Initiieren eines Failovers benötigen Sie Folgendes:

- Eine Anmeldung mit der Berechtigung „DBManager“ für die primäre Datenbank, mit der Berechtigung „db\_ownership“ für die lokale Datenbank für die Georeplikation und der Berechtigung „DBManager“ für die Partnerserver, für die Sie die Georeplikation konfigurieren.
- SQL Server Management Studio (SSMS)


> [AZURE.IMPORTANT] Es wird empfohlen, immer die neueste Version von Management Studio zu verwenden, damit Sie mit Updates von Microsoft Azure und SQL-Datenbank synchron sind. [Aktualisieren Sie SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx).




## Auslösen eines geplanten Failovers mit Heraufstufen einer sekundären Datenbank zur neuen primären Datenbank

Mit der **ALTER DATABASE**-Anweisung können Sie eine sekundäre Datenbank auf geplante Weise zur neuen primären Datenbank heraufstufen und die vorhandene primäre Datenbank zu einer sekundären Datenbank herabstufen. Diese Anweisung wird für die „master“-Datenbank auf dem logischen Azure SQL-Datenbankserver ausgeführt, in dem sich die geografisch replizierte Datenbank befindet, die heraufgestuft wird. Diese Funktionalität ist auf ein geplantes Failover ausgelegt, z. B. während der Notfallwiederherstellungsverfahren, und erfordert, dass die primäre Datenbank verfügbar ist.

Der Befehl hat den folgenden Workflow:

1. Die Replikation wird vorübergehend in den synchronen Modus umgeschaltet, was bewirkt, dass alle ausstehenden Transaktionen in die sekundäre Datenbank erfolgen und alle neuen Transaktionen blockiert werden.

2. Die Rollen der beiden Datenbanken in der Georeplikationspartnerschaft werden getauscht.

Durch diese Sequenz wird sichergestellt, dass die beiden Datenbanken vor dem Rollenwechsel synchronisiert werden, damit keine Daten verloren gehen. Es gibt einen kurzer Zeitraum, in dem beide Datenbanken während des Rollenwechsels (ca. 0 bis 25 Sekunden) nicht verfügbar sind. Wenn die primäre Datenbank über mehrere sekundäre Datenbanken verfügt, werden die anderen sekundären Datenbanken durch den Befehl automatisch neu konfiguriert, sodass sie eine Verbindung mit der neuen primären Datenbank herstellen. Unter normalen Umständen dauert der gesamte Vorgang nicht länger als 1 Minute. Weitere Informationen finden Sie unter [ALTER DATABASE (Transact-SQL)](https://msdn.microsoft.com/library/mt574871.aspx) und [Tarife](sql-database-service-tiers.md).


Führen Sie die folgenden Schritte aus, um ein geplantes Failover auszulösen.

1. Verbinden Sie sich in Management Studio mit dem logischen Azure SQL-Datenbankserver, in dem sich eine geografisch repliziert sekundäre Datenbank befindet.

2. Öffnen Sie den Ordner „Datenbanken“, erweitern Sie den Ordner **Systemdatenbanken**, klicken Sie mit der rechten Maustaste auf **master**, und klicken Sie anschließend auf **Neue Abfrage**.

3. Verwenden Sie die folgende **ALTER DATABASE**-Anweisung zum Wechseln von der sekundären Datenbank zur primären Rolle.

        ALTER DATABASE <MyDB> FAILOVER;

4. Klicken Sie auf **Ausführen**, um die Abfrage durchzuführen.

>[AZURE.NOTE] In seltenen Fällen ist es möglich, dass der Vorgang nicht abgeschlossen werden kann und festzustecken scheint. In diesem Fall kann der Benutzer den Befehl zum Erzwingen des Failovers aufrufen und den Datenverlust akzeptieren.


## Auslösen eines ungeplanten Failovers von der primären Datenbank zur sekundären Datenbank

Mit der **ALTER DATABASE**-Anweisung können Sie eine sekundäre Datenbank auf ungeplante Weise zur neuen primären Datenbank heraufstufen und das Herabstufen der vorhandenen primären Datenbank zu einer sekundären Datenbank erzwingen, sobald die primäre Datenbank nicht mehr verfügbar ist. Diese Anweisung wird für die „master“-Datenbank auf dem logischen Azure SQL-Datenbankserver ausgeführt, in dem sich die geografisch replizierte Datenbank befindet, die heraufgestuft wird.

Diese Funktion dient zur Notfallwiederherstellung, wenn das Wiederherstellen der Verfügbarkeit der Datenbank überaus wichtig und ein gewisser Datenverlust akzeptabel ist. Beim Auslösen eines erzwungenen Failovers wird die angegebene sekundäre Datenbank sofort zur primären Datenbank und beginnt mit dem Akzeptieren von Schreibtransaktionen. Sobald sich die ursprüngliche primäre Datenbank mit dieser neuen primären Datenbank verbinden kann, wird eine inkrementelle Sicherung der ursprünglichen Datenbank erstellt. Die alte primäre Datenbank wird zu einer sekundären Datenbank der neuen primären Datenbank. Anschließend ist sie lediglich ein sich synchronsierendes Replikat der neuen primären Datenbank.

Da jedoch die Point-in-Time-Wiederherstellung für sekundäre Datenbanken nicht unterstützt wird, muss der Benutzer, wenn Daten mit erfolgtem Commit in der alten primären Datenbank wiederhergestellt werden sollen, die nicht in die neue primäre Datenbank repliziert wurden, bevor das erzwungene Failover erfolgt ist, den Support bitten, diese verloren gegangenen Daten wiederherzustellen.

Wenn die primäre Datenbank über mehrere sekundäre Datenbanken verfügt, werden die anderen sekundären Datenbanken durch den Befehl automatisch neu konfiguriert, sodass sie eine Verbindung mit der neuen primären Datenbank herstellen.

Führen Sie die folgenden Schritte aus, um ein ungeplantes Failover auszulösen.

1. Verbinden Sie sich in Management Studio mit dem logischen Azure SQL-Datenbankserver, in dem sich eine geografisch repliziert sekundäre Datenbank befindet.

2. Öffnen Sie den Ordner „Datenbanken“, erweitern Sie den Ordner **Systemdatenbanken**, klicken Sie mit der rechten Maustaste auf **master**, und klicken Sie anschließend auf **Neue Abfrage**.

3. Verwenden Sie die folgende **ALTER DATABASE**-Anweisung zum Wechseln von der sekundären Datenbank zur primären Rolle.

        ALTER DATABASE <MyDB>   FORCE_FAILOVER_ALLOW_DATA_LOSS;

4. Klicken Sie auf **Ausführen**, um die Abfrage durchzuführen.

>[AZURE.NOTE] Falls der Befehl aufgerufen wird, wenn die primäre und sekundäre Datenbank online sind, wird die alte primäre Datenbank sofort zur neuen sekundären Datenbank, ohne dass eine Datensynchronisierung stattfindet. Wenn die primäre Datenbank bei Ausgabe des Befehls gerade Commits für Transaktionen ausführt, können einige Daten verloren gehen.



## Nächste Schritte   

- Informationen zur Wiederherstellung nach einem Ausfall mithilfe der aktiven Georeplikation, einschließlich der vor und nach der Wiederherstellung erforderlichen Schritte und der Ausführung eines Notfallwiederherstellungsverfahrens finden Sie unter [Notfallwiederherstellungsverfahren](sql-database-disaster-recovery.md).
- Den Blogpost von Sasha Nosov über die aktive Georeplikation finden Sie unter [Spotlight on new Geo-Replication capabilities](https://azure.microsoft.com/blog/spotlight-on-new-capabilities-of-azure-sql-database-geo-replication/) (Die neuen Georeplikationsfunktionen im Überblick).
- Informationen zum Vorbereiten von Cloudanwendungen für die aktive Georeplikation finden Sie unter [Entwerfen von Cloudanwendungen zum Sicherstellen der Geschäftskontinuität mithilfe der Georeplikation](sql-database-designing-cloud-solutions-for-disaster-recovery.md).
- Informationen zur Verwendung der aktiven Georeplikation mit elastischen Datenbankpools finden Sie unter [Strategien zur Notfallwiederherstellung mit elastischen Pools](sql-database-disaster-recovery-strategies-for-applications-with-elastic-pool.md).
- Eine Übersicht zum Thema Geschäftskontinuität finden Sie unter [Übersicht über die Geschäftskontinuität](sql-database-business-continuity.md).

<!---HONumber=AcomDC_0803_2016-->