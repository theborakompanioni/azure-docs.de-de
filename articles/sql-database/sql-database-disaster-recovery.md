<properties 
   pageTitle="Notfallwiederherstellung für SQL-Datenbank | Microsoft Azure" 
   description="Erfahren Sie, wie Sie eine Datenbank nach Störungen in einem regionalen Rechenzentrum oder dessen Ausfall mithilfe der aktiven Georeplikation oder der Geowiederherstellung von Azure SQL-Datenbank wiederherstellen." 
   services="sql-database" 
   documentationCenter="" 
   authors="carlrabeler" 
   manager="jhubbard" 
   editor="monicar"/>

<tags
   ms.service="sql-database"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="sqldb-bcdr" 
   ms.date="06/16/2016"
   ms.author="carlrab"/>

# Wiederherstellen einer Azure SQL-Datenbank oder Failover auf eine sekundäre Datenbank

Azure SQL-Datenbank bietet die folgenden Features zur Wiederherstellung nach einem Ausfall:

- [Aktive Georeplikation](sql-database-geo-replication-overview.md)
- [Geowiederherstellung](sql-database-recovery-using-backups.md#point-in-time-restore)

Informationen zur Vorbereitung auf Notfälle und zur Wiederherstellung der Datenbanken finden Sie unter [Geschäftskontinuität](sql-database-business-continuity.md) und [Geschäftskontinuitätsszenarien]().

## Startzeitpunkt für die Wiederherstellung

Der Wiederherstellungsvorgang wirkt sich auf die Anwendung aus. Er erfordert eine Änderung der SQL-Verbindungszeichenfolge und kann zu dauerhaftem Datenverlust führen. Er sollte daher nur ausgeführt werden, wenn der Ausfall wahrscheinlich länger als die Anwendungs-RTO dauert. Wenn die Anwendung in der Produktionsumgebung bereitgestellt wird, sollten Sie regelmäßige Überwachungen der Anwendungsintegrität durchführen und die folgenden Datenpunkte verwenden, um zu bestätigen, dass die Wiederherstellung erforderlich ist:

1.	Permanente Verbindungsfehler zwischen Anwendungsebene und Datenbank.
2.	Das Azure-Portal zeigt eine Warnung zu einem Incident in der Region, der weit reichende Auswirkungen hat.
3.	Der Azure SQL-Datenbankserver wird als beeinträchtigt gekennzeichnet.

Je nach Toleranz Ihrer Anwendung gegenüber einer Downtime und einer möglichen geschäftlichen Haftung können Sie die folgenden Wiederherstellungsoptionen in Betracht ziehen.

Verwenden Sie [Get Recoverable Database](https://msdn.microsoft.com/library/dn800985.aspx) (*LastAvailableBackupDate*) zum Abrufen des neuesten georeplizierten Wiederherstellungspunkts.

## Warten auf die Dienstwiederherstellung

Die Azure-Teams arbeiten intensiv daran, die Dienstverfügbarkeit so schnell wie möglich wiederherzustellen, aber je nach Fehlerursache kann die Wiederherstellung Stunden oder Tage in Anspruch nehmen. Wenn Ihre Anwendung eine solche Downtime tolerieren kann, können Sie einfach warten, bis der Dienst wiederhergestellt wurde. In diesem Fall ist keine weitere Aktion erforderlich. Sie können den aktuellen Dienststatus auf unserem [Dashboard zur Azure-Dienstintegrität](https://azure.microsoft.com/status/) anzeigen. Nach der Dienstwiederherstellung in Ihrer Region wird die Verfügbarkeit Ihrer Anwendung wiederhergestellt.

## Failover auf eine georeplizierte sekundäre Datenbank

Wenn eine Downtime für Ihre Anwendung zu einer geschäftlichen Haftung führen kann, sollten Sie georeplizierte Datenbanken für Ihre Anwendung einsetzen. So kann die Anwendungsverfügbarkeit bei einem Ausfall schnell in einer anderen Region wiederhergestellt werden. Hier finden Sie Informationen zum [Konfigurieren der Georeplikation](sql-database-geo-replication-portal.md).

Um die Verfügbarkeit der benötigten Datenbank(en) wiederherzustellen, müssen Sie mithilfe einer der unterstützten Methoden ein Failover auf die georeplizierte sekundäre Datenbank ausführen.


In den folgenden Leitfäden finden Sie Informationen zum Failover auf eine georeplizierte sekundäre Datenbank:

- [Failover auf eine georeplizierte sekundäre Datenbank mit dem Azure-Portal](sql-database-geo-replication-portal.md)
- [Failover auf eine georeplizierte sekundäre Datenbank mit PowerShell](sql-database-geo-replication-powershell.md)
- [Failover auf eine georeplizierte sekundäre Datenbank mit T-SQL](sql-database-geo-replication-transact-sql.md)



## Wiederherstellen mit Geowiederherstellung

Wenn eine Downtime Ihrer Anwendung keine geschäftliche Haftung nach sich zieht, können Sie die Geowiederherstellung als Methode zum Wiederherstellen Ihrer Anwendungsdatenbank(en) nutzen. Hierbei wird eine Kopie der Datenbank aus der letzten georedundanten Sicherung erstellt.

In den folgenden Leitfäden finden Sie Informationen zur Geowiederherstellung einer Datenbank in einer neuen Region:

- [Geowiederherstellung einer Datenbank in einer neuen Region mit dem Azure-Portal](sql-database-geo-restore-portal.md)
- [Geowiederherstellung einer Datenbank in einer neuen Region mit PowerShell](sql-database-geo-restore-powershell.md)


## Konfigurieren der Datenbank nach der Wiederherstellung

Wenn Sie ein Georeplikationsfailover oder die Geowiederherstellung ausführen, um Ihre Anwendung nach einem Ausfall wiederherzustellen, müssen Sie sicherstellen, dass die Verbindung mit der neuen Datenbank ordnungsgemäß konfiguriert ist, sodass der normale Anwendungsbetrieb wieder aufgenommen werden kann. Dies ist eine Prüfliste mit Aufgaben, anhand derer Sie die wiederhergestellte Datenbank für die Produktion vorbereiten können.

### Aktualisieren von Verbindungszeichenfolgen

Da Ihre wiederhergestellte Datenbank auf einem anderen Server vorliegt, müssen Sie die Verbindungszeichenfolge Ihrer Anwendung so anpassen, dass sie auf den neuen Server verweist.

Weitere Informationen zum Ändern von Verbindungszeichenfolgen finden Sie unter der entsprechenden Programmiersprache für Ihre [Verbindungsbibliothek](sql-database-libraries.md).

### Konfigurieren von Firewallregeln

Sie müssen sicherstellen, dass die für den Server und die Datenbank konfigurierten Firewallregeln mit denen übereinstimmen, die für den primären Server und die primäre Datenbank konfiguriert wurden. Weitere Informationen finden Sie unter [Vorgehensweise: Konfigurieren von Firewalleinstellungen (Azure SQL-Datenbank)](sql-database-configure-firewall-settings.md).


### Konfigurieren von Anmeldungen und Datenbankbenutzern

Stellen Sie sicher, dass alle von der Anwendung verwendeten Anmeldungen auf dem Server vorhanden sind, der die wiederhergestellte Datenbank hostet. Weitere Informationen finden Sie unter [Sicherheitskonfiguration für die Georeplikation](sql-database-geo-replication-security-config.md).

>[AZURE.NOTE] Konfigurieren und Testen Sie die Serverfirewallregeln und -anmeldungen (sowie deren Berechtigungen) während eines Notfallwiederherstellungverfahrens. Diese Objekte auf Serverebene und deren Konfiguration sind während des Ausfalls möglicherweise nicht verfügbar. Weitere Informationen finden Sie unter [Ausführen von Notfallwiederherstellungsverfahren](sql-database-disaster-recovery-drills.md).

### Einrichten von Telemetrie-Warnungen

Sie müssen sicherstellen, dass Ihre vorhandenen Einstellungen zu Warnungsregeln aktualisiert werden, sodass auf die wiederhergestellte Datenbank und den neuen Server verwiesen wird.

Weitere Informationen zu Datenbankwarnungsregeln finden Sie unter [Empfangen von Warnbenachrichtigungen](../azure-portal/insights-receive-alert-notifications.md) und [Nachverfolgen der Dienstintegrität](../azure-portal/insights-service-health.md).

### Aktivieren der Überwachung

Wenn für den Zugriff auf die Datenbank Überwachung erforderlich ist, müssen Sie nach der Wiederherstellung der Datenbank die Überwachung aktivieren. Weitere Informationen finden Sie unter [Erste Schritte mit der SQL-Datenbanküberwachung](sql-database-auditing-get-started.md). Informationen zu Vorgängerversionsclients finden Sie zudem unter [SQL-Datenbank – Unterstützung für kompatible Clients und IP-Endpunkt-Änderungen für die Überwachung](sql-database-auditing-and-dynamic-data-masking-downlevel-clients.md).


## Nächste Schritte

- Informationen über automatisierte Sicherungen von Azure SQL-Datenbanken finden Sie unter [Übersicht: Automatisierte SQL-Datenbanksicherungen](sql-database-automated-backups.md).
- Informationen über Entwurfs- und Wiederherstellungsszenarien für die Geschäftskontinuität finden Sie unter [Geschäftskontinuitätsszenarien](sql-database-business-continuity-scenarios.md).
- Informationen zum Verwenden automatisierter Sicherungen für die Wiederherstellung finden Sie unter [Wiederherstellen einer Datenbank aus vom Dienst initiierten Sicherungen](sql-database-recovery-using-backups.md).
- Informationen über schnellere Wiederherstellungsoptionen finden Sie unter [Aktive Georeplikation](sql-database-geo-replication-overview.md).
- Informationen zum Verwenden automatisierter Sicherungen für die Archivierung finden Sie unter [Datenbankkopie](sql-database-copy.md).

<!---HONumber=AcomDC_0713_2016-->