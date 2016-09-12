<properties 
    pageTitle="Initiieren eines geplanten oder ungeplanten Failovers für die Azure SQL-Datenbank mit PowerShell | Microsoft Azure" 
    description="Initiieren eines geplanten oder ungeplanten Failovers für die Azure SQL-Datenbank mit PowerShell" 
    services="sql-database" 
    documentationCenter="" 
    authors="stevestein" 
    manager="jhubbard" 
    editor=""/>

<tags
    ms.service="sql-database"
    ms.devlang="NA"
    ms.topic="article"
    ms.tgt_pltfrm="powershell"
    ms.workload="data-management" 
    ms.date="08/29/2016"
    ms.author="sstein"/>

# Initiieren eines geplanten oder ungeplanten Failovers für die Azure SQL-Datenbank mit PowerShell



> [AZURE.SELECTOR]
- [Azure-Portal](sql-database-geo-replication-failover-portal.md)
- [PowerShell](sql-database-geo-replication-failover-powershell.md)
- [T-SQL](sql-database-geo-replication-failover-transact-sql.md)


Dieser Artikel erläutert das Initiieren eines geplanten oder ungeplanten Failovers für die Azure SQL-Datenbank mit PowerShell. Informationen zum Konfigurieren der Georeplikation finden Sie unter [Konfigurieren der Georeplikation für die Azure SQL-Datenbank mit PowerShell](sql-database-geo-replication-powershell.md).



## Initiieren eines geplanten Failovers

Mit dem Cmdlet **Set-AzureRmSqlDatabaseSecondary** und dem **-Failover**-Parameter können Sie eine sekundäre Datenbank zur neuen primären Datenbank heraufstufen und die vorhandene primäre Datenbank zu einer sekundären Datenbank herabstufen. Diese Funktionalität ist auf ein geplantes Failover ausgelegt, z. B. bei Notfallwiederherstellungsverfahren, und erfordert, dass die primäre Datenbank verfügbar ist.

Der Befehl hat den folgenden Workflow:

1. Für die Replikation wird vorübergehend in den synchronen Modus gewechselt. Dadurch werden alle ausstehenden Transaktionen in die sekundäre Datenbank übertragen.

2. Vertauschen Sie die Rollen der beiden Datenbanken in der Georeplikationspartnerschaft.

Durch diese Sequenz wird sichergestellt, dass die beiden Datenbanken vor dem Rollenwechsel synchronisiert werden, damit keine Daten verloren gehen. Es gibt einen kurzer Zeitraum, in dem beide Datenbanken während des Rollenwechsels (ca. 0 bis 25 Sekunden) nicht verfügbar sind. Unter normalen Umständen dauert der gesamte Vorgang nicht länger als 1 Minute. Weitere Informationen finden Sie unter [Set- AzureRmSqlDatabaseSecondary](https://msdn.microsoft.com/library/mt619393.aspx).




Dieses Cmdlet wird zurückgegeben, wenn der Wechsel von der sekundären zur primären Datenbank abgeschlossen ist.

Mit dem folgenden Befehl werden die Rollen der „mydb“-Datenbank auf dem Server „srv2“ in der Ressourcengruppe „rg2“ mit der primären Datenbank getauscht. Die ursprüngliche primäre Datenbank, mit der „db2“ verbunden war, wird nach der vollständigen Synchronisierung der beiden Datenbanken zur sekundären Datenbank.

    $database = Get-AzureRmSqlDatabase –DatabaseName "mydb" –ResourceGroupName "rg2” –ServerName "srv2”
    $database | Set-AzureRmSqlDatabaseSecondary -Failover


> [AZURE.NOTE] In seltenen Fällen ist es möglich, dass der Vorgang nicht abgeschlossen werden kann und nicht mehr zu reagieren scheint. In diesem Fall kann der Benutzer den Befehl zum Erzwingen des Failovers (ungeplantes Failover) aufrufen und den Datenverlust akzeptieren.


## Auslösen eines ungeplanten Failovers von der primären Datenbank zur sekundären Datenbank


Mit dem Cmdlet **Set-AzureRmSqlDatabaseSecondary** mit den Parametern **-Failover** und **-AllowDataLoss** können Sie eine sekundäre Datenbank auf ungeplante Weise zur neuen primären Datenbank heraufstufen und das Herabstufen der vorhandenen primären Datenbank zu einer sekundären Datenbank erzwingen, sobald die primäre Datenbank nicht mehr verfügbar ist.

Diese Funktion dient zur Notfallwiederherstellung, wenn das Wiederherstellen der Verfügbarkeit der Datenbank überaus wichtig und ein gewisser Datenverlust akzeptabel ist. Beim Auslösen eines erzwungenen Failovers wird die angegebene sekundäre Datenbank sofort zur primären Datenbank und beginnt mit dem Akzeptieren von Schreibtransaktionen. Sobald sich die ursprüngliche primäre Datenbank wieder mit dieser neuen primären Datenbank verbinden kann, wird eine inkrementelle Sicherung der ursprünglichen Datenbank erstellt. Die alte primäre Datenbank wird zu einer sekundären Datenbank der neuen primären Datenbank. Anschließend ist sie lediglich ein Replikat der neuen primären Datenbank.

Da die Point-in-Time-Wiederherstellung jedoch nicht auf sekundären Datenbanken unterstützt wird, müssen Sie zum Wiederherstellen von Daten aus der alten primären Datenbank, die nicht in der neuen primären Datenbank repliziert wurden, CSS erfassen, um eine Datenbank in der bekannten Protokollsicherung wiederherstellen.

> [AZURE.NOTE] Falls der Befehl aufgerufen wird, wenn die primäre und sekundäre Datenbank online sind, wird die alte primäre Datenbank sofort zur neuen sekundären Datenbank, ohne dass eine Datensynchronisierung stattfindet. Wenn die primäre Datenbank bei Ausgabe des Befehls gerade Commits für Transaktionen ausführt, können einige Daten verloren gehen.


Falls die primäre Datenbank mehrere sekundäre Instanzen hat, wird der Befehl teilweise erfolgreich ausgeführt. Die sekundäre Datenbank, auf der der Befehl ausgeführt wurde, wird zur primären Datenbank. Die alten primären Datenbanken bleiben jedoch primäre Datenbanken. Die beiden primären Datenbanken befinden sich dadurch in einem inkonsistenten Zustand und sind über eine ausgesetzte Replizierungsverknüpfung miteinander verbunden. Der Benutzer muss diese Konfiguration mit einer „remove secondary“-API auf einer dieser primären Datenbanken manuell reparieren.


Mit dem folgenden Befehl tauschen Sie die Rollen der „mydb“-Datenbank, wenn die primäre Datenbank nicht verfügbar ist. Die ursprüngliche primäre Datenbank, mit der „mydb“ verbunden war, wird zur sekundären Datenbank, sobald sie wieder online ist. Zu diesem Zeitpunkt kann bei der Synchronisierung Datenverlust entstehen.

    $database = Get-AzureRmSqlDatabase –DatabaseName "mydb" –ResourceGroupName "rg2” –ServerName "srv2”
    $database | Set-AzureRmSqlDatabaseSecondary –Failover -AllowDataLoss




## Nächste Schritte   

- Stellen Sie nach dem Failover sicher, dass die Authentifizierungsanforderungen für Ihren Server und Ihre Datenbank auf der neuen primären konfiguriert sind. Weitere Informationen finden Sie unter [Verwalten der Sicherheit der Azure SQL-Datenbank nach der Notfallwiederherstellung](sql-database-geo-replication-security-config.md).
- Informationen zur Wiederherstellung nach einem Ausfall mithilfe der aktiven Georeplikation, einschließlich der vor und nach der Wiederherstellung erforderlichen Schritte und der Ausführung eines Notfallwiederherstellungsverfahrens, finden Sie unter [Notfallwiederherstellungsverfahren](sql-database-disaster-recovery.md).
- Den Blogpost von Sasha Nosov über die aktive Georeplikation finden Sie unter [Spotlight on new Geo-Replication capabilities](https://azure.microsoft.com/blog/spotlight-on-new-capabilities-of-azure-sql-database-geo-replication/) (Die neuen Georeplikationsfunktionen im Überblick).
- Informationen zum Vorbereiten von Cloudanwendungen für die aktive Georeplikation finden Sie unter [Entwerfen von Cloudanwendungen zum Sicherstellen der Geschäftskontinuität mithilfe der Georeplikation](sql-database-designing-cloud-solutions-for-disaster-recovery.md).
- Informationen zur Verwendung der aktiven Georeplikation mit elastischen Datenbankpools finden Sie unter [Strategien zur Notfallwiederherstellung mit elastischen Pools](sql-database-disaster-recovery-strategies-for-applications-with-elastic-pool.md).
- Eine Übersicht zum Thema Geschäftskontinuität finden Sie unter [Übersicht über die Geschäftskontinuität](sql-database-business-continuity.md).

<!---HONumber=AcomDC_0831_2016-->