<properties 
   pageTitle="Notfallwiederherstellung einer SQL-Datenbank" 
   description="Erfahren Sie, wie Sie eine Datenbank nach Störungen in einem regionalen Rechenzentrum oder dessen Ausfall mithilfe der aktiven Georeplikation oder der geografischen Wiederherstellung von Azure SQL-Datenbank wiederherstellen." 
   services="sql-database" 
   documentationCenter="" 
   authors="elfisher" 
   manager="jhubbard" 
   editor="monicar"/>

<tags
   ms.service="sql-database"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-management" 
   ms.date="05/10/2016"
   ms.author="elfish"/>

# Wiederherstellen einer Azure SQL-Datenbank oder Failover auf eine sekundäre Datenbank

Azure SQL-Datenbank bietet die folgenden Features zur Wiederherstellung nach einem Ausfall:

- [Aktive Georeplikation](sql-database-geo-replication-overview.md)
- [Geografische Wiederherstellung](sql-database-geo-restore.md)

Informationen zur Vorbereitung auf Notfälle und zur Wiederherstellung der Datenbanken finden Sie auf der Seite [Entwurf für Geschäftskontinuität](sql-database-business-continuity-design.md).

## Startzeitpunkt für die Wiederherstellung

Der Wiederherstellungsvorgang wirkt sich auf die Anwendung aus. Er erfordert eine Änderung der SQL-Verbindungszeichenfolge und kann zu dauerhaftem Datenverlust führen. Er sollte daher nur ausgeführt werden, wenn der Ausfall wahrscheinlich länger als die Anwendungs-RTO dauert. Wenn die Anwendung in der Produktionsumgebung bereitgestellt wird, sollten Sie regelmäßige Überwachungen der Anwendungsintegrität durchführen und die folgenden Datenpunkte verwenden, um zu bestätigen, dass die Wiederherstellung erforderlich ist:

1.	Permanente Verbindungsfehler zwischen Anwendungsebene und Datenbank.
2.	Das Azure-Portal zeigt eine Warnung zu einem Incident in der Region, der weit reichende Auswirkungen hat.
3.	Der Azure SQL-Datenbankserver wird als beeinträchtigt gekennzeichnet. 

Je nach Toleranz Ihrer Anwendung gegenüber einer Downtime und einer möglichen geschäftlichen Haftung können Sie die folgenden Wiederherstellungsoptionen in Betracht ziehen.

## Warten auf die Dienstwiederherstellung

Die Azure-Teams arbeiten intensiv daran, die Dienstverfügbarkeit so schnell wie möglich wiederherzustellen, aber je nach Fehlerursache kann die Wiederherstellung Stunden oder Tage in Anspruch nehmen. Wenn Ihre Anwendung eine solche Downtime tolerieren kann, können Sie einfach warten, bis der Dienst wiederhergestellt wurde. In diesem Fall ist keine weitere Aktion erforderlich. Sie können den aktuellen Dienststatus auf unserem [Dashboard zur Azure-Dienstintegrität](https://azure.microsoft.com/status/) anzeigen. Nach der Dienstwiederherstellung in Ihrer Region wird die Verfügbarkeit Ihrer Anwendung wiederhergestellt.

## Failover auf eine georeplizierte sekundäre Datenbank

Wenn eine Downtime für Ihre Anwendung zu einer geschäftlichen Haftung führen kann, sollten Sie georeplizierte Datenbanken für Ihre Anwendung einsetzen. So kann die Anwendungsverfügbarkeit bei einem Ausfall schnell in einer anderen Region wiederhergestellt werden. Hier finden Sie Informationen zum [Konfigurieren der Georeplikation](sql-database-geo-replication-portal.md).

Um die Verfügbarkeit der benötigten Datenbank(en) wiederherzustellen, müssen Sie mithilfe einer der unterstützten Methoden ein Failover auf die georeplizierte sekundäre Datenbank ausführen.


In den folgenden Leitfäden finden Sie Informationen zum Failover auf eine georeplizierte sekundäre Datenbank:

- [Failover auf eine georeplizierte sekundäre Datenbank mit dem Azure-Portal](sql-database-geo-replication-portal.md)
- [Failover auf eine georeplizierte sekundäre Datenbank mit PowerShell](sql-database-geo-replication-powershell.md)
- [Failover auf eine georeplizierte sekundäre Datenbank mit T-SQL](sql-database-geo-replication-transact-sql.md) 



## Wiederherstellen mit geografischer Wiederherstellung

Wenn eine Downtime Ihrer Anwendung keine geschäftliche Haftung nach sich zieht, können Sie die geografische Wiederherstellung als Methode zum Wiederherstellen Ihrer Anwendungsdatenbank(en) nutzen. Hierbei wird eine Kopie der Datenbank aus der letzten georedundanten Sicherung erstellt.

In den folgenden Leitfäden finden Sie Informationen zur geografischen Wiederherstellung einer Datenbank in einer neuen Region:

- [Geografische Wiederherstellung einer Datenbank in einer neuen Region mit dem Azure-Portal](sql-database-geo-restore-portal.md)
- [Geografische Wiederherstellung einer Datenbank in einer neuen Region mit PowerShell](sql-database-geo-restore-powershell.md) 


## Konfigurieren der Datenbank nach der Wiederherstellung

Wenn Sie ein Georeplikationsfailover mit Optionen für die geografische Wiederherstellung ausführen, um Ihre Anwendung nach einem Ausfall wiederherzustellen, müssen Sie sicherstellen, dass die Verbindung mit der neuen Datenbank ordnungsgemäß konfiguriert ist, sodass der normale Anwendungsbetrieb wieder aufgenommen werden kann. Dies ist eine Prüfliste mit Aufgaben, anhand derer Sie die wiederhergestellte Datenbank für die Produktion vorbereiten können.

### Aktualisieren von Verbindungszeichenfolgen

Da Ihre wiederhergestellte Datenbank auf einem anderen Server vorliegt, müssen Sie die Verbindungszeichenfolge Ihrer Anwendung so anpassen, dass sie auf den neuen Server zeigt.

Weitere Informationen zum Ändern von Verbindungszeichenfolgen finden Sie unter [Verbindungen mit der Azure SQL-Datenbank: Wichtige Empfehlungen](sql-database-connect-central-recommendations.md).

### Konfigurieren von Firewallregeln

Sie müssen sicherstellen, dass die für den Server und die Datenbank konfigurierten Firewallregeln mit denen übereinstimmen, die für den primären Server und die primäre Datenbank konfiguriert wurden. Weitere Informationen finden Sie unter [Vorgehensweise: Konfigurieren von Firewalleinstellungen (Azure SQL-Datenbank)](sql-database-configure-firewall-settings.md).


### Konfigurieren von Anmeldungen und Datenbankbenutzern

Stellen Sie sicher, dass alle von der Anwendung verwendeten Anmeldungen auf dem Server vorhanden sind, der die wiederhergestellte Datenbank hostet. Weitere Informationen finden Sie unter „Verwalten der Sicherheit bei einer Notfallwiederherstellung“. Weitere Informationen finden Sie unter [Sicherheitskonfiguration für die Georeplikation](sql-database-geo-replication-security-config.md).

>[AZURE.NOTE] Wenn Sie die geografische Wiederherstellung zum Wiederherstellen nach einem Ausfall verwenden, sollten Sie im Rahmen des Verfahrens für die Notfallwiederherstellung Ihre Firewallregeln und Anmeldungen konfigurieren, um sicherzustellen, dass der primäre Server weiterhin seine Konfiguration abrufen kann. Da bei der geografischen Wiederherstellung Datenbanksicherungen verwendet werden, steht die Konfiguration auf Serverebene während des Ausfalls möglicherweise nicht zur Verfügung. Nach Abschluss der Notfallwiederherstellung können Sie die wiederhergestellten Datenbanken entfernen, halten Sie jedoch den Server und seine Konfiguration für den Wiederherstellungsvorgang bereit. Weitere Informationen finden Sie unter [Ausführen von Notfallwiederherstellungsverfahren](sql-database-disaster-recovery-drills.md).

### Einrichten von Telemetrie-Warnungen

Sie müssen sicherstellen, dass Ihre vorhandenen Einstellungen zu Warnungsregeln aktualisiert werden, sodass auf die wiederhergestellte Datenbank und den neuen Server verwiesen wird.

Weitere Informationen zu Datenbankwarnungsregeln finden Sie unter [Empfangen von Warnbenachrichtigungen](../azure-portal/insights-receive-alert-notifications.md) und [Nachverfolgen der Dienstintegrität](../azure-portal/insights-service-health.md).

### Aktivieren der Überwachung

Wenn für den Zugriff auf die Datenbank Überwachung erforderlich ist, müssen Sie nach der Wiederherstellung der Datenbank die Überwachung aktivieren. Ein guter Indikator für die Notwendigkeit einer Überwachung ist es, wenn Clientanwendungen sichere Verbindungszeichenfolgen mit dem Muster „*.database.secure.windows.net“ verwenden. Weitere Informationen finden Sie unter [Erste Schritte mit der SQL-Datenbanküberwachung](sql-database-auditing-get-started.md).




## Zusätzliche Ressourcen


- [Geschäftskontinuität und Notfallwiederherstellung mit SQL-Datenbank](sql-database-business-continuity.md)
- [Point-in-Time-Wiederherstellung](sql-database-point-in-time-restore.md)
- [Geografische Wiederherstellung](sql-database-geo-restore.md)
- [Aktive Georeplikation](sql-database-geo-replication-overview.md)
- [Entwerfen einer Anwendung für die cloudbasierte Notfallwiederherstellung](sql-database-designing-cloud-solutions-for-disaster-recovery.md)
- [Abschließen der wiederhergestellten Azure SQL-Datenbank](sql-database-recovered-finalize.md)
- [Sicherheitskonfiguration für die Georeplikation](sql-database-geo-replication-security-config.md)
- [BCDR in SQL-Datenbank – Häufig gestellte Fragen](sql-database-bcdr-faq.md)

<!---HONumber=AcomDC_0511_2016-->