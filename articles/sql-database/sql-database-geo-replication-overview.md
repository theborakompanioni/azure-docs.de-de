<properties
	pageTitle="Aktive Georeplikation für Azure SQL-Datenbank"
	description="Die aktive Georeplikation ermöglicht Ihnen das Einrichten von vier Replikaten Ihrer Datenbank in einem beliebigen Azure-Rechenzentrum."
	services="sql-database"
	documentationCenter="na"
	authors="stevestein"
	manager="jhubbard"
	editor="monicar" />


<tags
	ms.service="sql-database"
	ms.devlang="na"
	ms.topic="article"
	ms.tgt_pltfrm="na"
   ms.workload="NA"
	ms.date="08/29/2016"
	ms.author="sstein" />

# Übersicht: Aktive Georeplikation in Azure SQL-Datenbank

Mit der aktiven Georeplikation können Sie bis zu vier lesbare sekundäre Datenbanken in der gleichen oder in verschiedenen Standorten von Rechenzentren (Regionen) konfigurieren. Sekundäre Datenbanken stehen für Abfragen und Failover zur Verfügung, wenn ein Rechenzentrum ausgefallen ist oder keine Verbindung mit der primären Datenbank möglich ist.

>[AZURE.NOTE] Die aktive Georeplikation (lesbare sekundäre Datenbanken) ist jetzt für alle Datenbanken in allen Diensttarifen verfügbar. Im April 2017 wird der nicht lesbare sekundäre Typ eingestellt, und vorhandene nicht lesbare Datenbanken werden automatisch auf lesbare sekundäre Datenbanken aktualisiert.

 Sie können die aktive Georeplikation auf folgende Weise konfigurieren: im [Azure-Portal](sql-database-geo-replication-portal.md), mit [PowerShell](sql-database-geo-replication-powershell.md), mit [Transact-SQL](sql-database-geo-replication-transact-sql.md) oder mit der [REST-API – Create or Update Database](https://msdn.microsoft.com/library/azure/mt163685.aspx).

> [AZURE.SELECTOR]
- [Konfigurieren: im Azure-Portal](sql-database-geo-replication-portal.md)
- [Konfigurieren: mit PowerShell](sql-database-geo-replication-powershell.md)
- [Konfigurieren: mit T-SQL](sql-database-geo-replication-transact-sql.md)

Falls Ihre primäre Datenbank aus irgendeinem Grund ausfällt oder einfach offline geschaltet werden muss, können Sie ein *Failover* auf eine der sekundären Datenbanken durchführen. Wenn das Failover auf eine sekundäre Datenbank aktiviert ist, werden alle anderen sekundären Datenbanken automatisch mit der neuen primären Datenbank verknüpft.

Das Failover auf eine sekundäre Datenbank können Sie auf folgende Weise ausführen: im [Azure-Portal](sql-database-geo-replication-failover-portal.md), mit [PowerShell](sql-database-geo-replication-failover-powershell.md), mit [Transact-SQL](sql-database-geo-replication-failover-transact-sql.md), mit der [REST-API – Planned Failover](https://msdn.microsoft.com/ibrary/azure/mt575007.aspx) oder mit der [REST-API – Unplanned Failover](https://msdn.microsoft.com/library/azure/mt582027.aspx).


> [AZURE.SELECTOR]
- [Failover: im Azure-Portal](sql-database-geo-replication-failover-portal.md)
- [Failover: mit PowerShell](sql-database-geo-replication-failover-powershell.md)
- [Failover: mit T-SQL](sql-database-geo-replication-failover-transact-sql.md)

Stellen Sie nach dem Failover sicher, dass die Authentifizierungsanforderungen für Ihren Server und Ihre Datenbank auf der neuen primären konfiguriert sind. Weitere Informationen finden Sie unter [Verwalten der Sicherheit der Azure SQL-Datenbank nach der Notfallwiederherstellung](sql-database-geo-replication-security-config.md).


Das Feature „Aktive Georeplikation“ implementiert einen Mechanismus zum Bereitstellen von Datenbankredundanz innerhalb derselben Microsoft Azure-Region bzw. verschiedenen Regionen (Georedundanz). Bei aktiver Georeplikation werden Transaktionen mit ausgeführtem Commit aus einer Datenbank asynchron in bis zu vier Kopien der Datenbank auf verschiedene Server repliziert. Hierfür wird die Read Commited-Momentaufnahmeisolation (Read Committed Snapshot Isolation, RCSI) verwendet. Wenn die aktive Georeplikation konfiguriert ist, wird auf dem angegebenen Server eine sekundäre Datenbank erstellt. Die ursprüngliche Datenbank wird zur primären Datenbank. Die primäre Datenbank repliziert Transaktionen mit ausgeführtem Commit asynchron in alle sekundären Datenbanken. Nur vollständige Transaktionen werden repliziert. Wenngleich die sekundäre Datenbank stets ein wenig hinter der primären Datenbank zurückliegt, sind unvollständige Transaktionen bei sekundären Daten garantiert ausgeschlossen. Die spezifischen RPO-Daten finden Sie unter [Übersicht über die Geschäftskontinuität mit Azure SQL-Datenbank](sql-database-business-continuity.md).

Einer der Hauptvorteile der aktiven Georeplikation ist, dass es sich um eine Lösung für die Notfallwiederherstellung auf Datenbankebene handelt, die eine geringe Wiederherstellungszeit erfordert. Wenn Sie die sekundäre Datenbank auf einem Server in einer anderen Region platzieren, stellen Sie die maximale Resilienz (Ausfallsicherheit) Ihrer Anwendung sicher. Regionsübergreifende Redundanz ermöglicht Anwendungen die Wiederherstellung nach einem dauerhaften Ausfall eines gesamten Rechenzentrums oder von Teilen eines Rechenzentrums aufgrund von Naturkatastrophen, schwerwiegendem menschlichen Versagen oder böswilligen Handlungen. Die folgende Abbildung zeigt ein Beispiel für die Konfiguration der aktiven Georeplikation für eine Premium-Datenbank, wobei sich die primäre Datenbank in der Region „USA, Norden-Mitte“ und die sekundäre Datenbank in der Region „USA, Süden-Mitte“ befindet.

![Georeplikationsbeziehung](./media/sql-database-active-geo-replication/geo-replication-relationship.png)

Ein anderer wichtiger Vorteil ist, dass die sekundären Datenbanken lesbar sind und zum Auslagern schreibgeschützter Workloads wie Berichtsaufträge verwendet werden können. Wenn Sie die sekundäre Datenbank nur für den Lastenausgleich verwenden möchten, können Sie sie in der gleichen Region wie die primäre Datenbank erstellen. Das Erstellen einer sekundären Datenbank erhöht jedoch nicht die Resilienz der Anwendung gegenüber schwerwiegenden Fehlern.

Es folgen weitere Szenarien, in denen die aktive Georeplikation verwendet werden kann:

- **Datenbankmigration**: Sie können die aktive Georeplikation zur Onlinemigration einer Datenbank von einem Server auf einen anderen bei minimalen Ausfallzeiten nutzen.
- **Anwendungsupgrades**: Sie können bei Anwendungsupgrades eine zusätzliche sekundäre Datenbank als Failbackkopie erstellen.

Wenn Sie echte Geschäftskontinuität erreichen möchten, ist das Bereitstellen von Datenbankredundanz zwischen Rechenzentren jedoch nur ein Teil der Lösung. Für die komplette Wiederherstellung einer Anwendung bzw. eines Diensts nach einem schwerwiegenden Fehler ist das Wiederherstellen aller Komponenten erforderlich, aus denen sich der Dienst und alle abhängigen Dienste zusammensetzen. Beispiele dieser Komponenten sind die Clientsoftware (z. B. ein Browser mit benutzerdefiniertem JavaScript), Web-Front-Ends, Speicher und DNS. Es ist wichtig, dass alle Komponenten hinsichtlich derselben Fehler gegen Ausfälle geschützt und innerhalb des RTO (Recovery Time Objective) der Anwendung wieder verfügbar sind. Daher müssen Sie alle abhängigen Dienste bestimmen und mit dem Leistungsumfang und den Funktionen vertraut sein, die sie bieten. Dann müssen Sie entsprechende Maßnahmen ergreifen, um sicherzustellen, dass Ihr Dienst während des Failovers der Dienste funktioniert, von denen er abhängig ist. Weitere Informationen zum Entwerfen von Lösungen für die Notfallwiederherstellung finden Sie unter [Entwerfen von Cloudlösungen für die Notfallwiederherstellung mithilfe der aktiven Georeplikation](sql-database-designing-cloud-solutions-for-disaster-recovery.md).

## Möglichkeiten der aktiven Georeplikation
Das Feature „Aktive Georeplikation“ bietet die folgenden wichtigen Möglichkeiten:

- **Automatische asynchrone Replikation**: Sie können eine sekundäre Datenbank nur durch Hinzufügen zu einer vorhandenen Datenbank erstellen. Die sekundäre Datenbank kann nur auf einem anderen Azure SQL-Datenbank-Server erstellt werden. Nach der Erstellung wird die sekundäre Datenbank mit den Daten aufgefüllt, die aus der primären Datenbank kopiert wurden. Dieser Prozess wird als Seeding bezeichnet. Nachdem eine sekundäre Datenbank erstellt wurde und das Seeding erfolgt ist, werden Aktualisierungen der primären Datenbank automatisch asynchron in die sekundäre Datenbank repliziert. Asynchrone Replikation bedeutet, dass für Transaktionen in der primären Datenbank ein Commit erfolgt, ehe sie in die sekundäre Datenbank repliziert werden.

- **Mehrere sekundäre Datenbanken**: Mehrere sekundäre Datenbanken sorgen für mehr Redundanz und einen besseren Schutz der primären Datenbank und Anwendung. Wenn mehrere sekundäre Datenbanken vorhanden sind, bleibt die Anwendung auch bei Ausfall einer der sekundären Datenbanken geschützt. Wenn es nur eine sekundäre Datenbank gibt und diese ausfällt, ist die Anwendung bis zum Erstellen einer neuen sekundären Datenbank einem höheren Risiko ausgesetzt.

- **Lesbare sekundäre Datenbanken**: Eine Anwendung kann für schreibgeschützte Vorgänge auf eine sekundäre Datenbank zugreifen, indem sie die Sicherheitsprinzipale für den Zugriff auf die primäre Datenbank oder andere Sicherheitsprinzipale verwendet. Die sekundären Datenbanken werden im Momentaufnahme-Isolationsmodus betrieben. Auf diese Weise wird sichergestellt, dass die Replikation von Aktualisierungen der primären Datenbank (Protokollwiedergabe) nicht durch Abfragen verzögert wird, die für die sekundäre Datenbank ausgeführt werden.

>[AZURE.NOTE] Die Protokollwiedergabe wird in der sekundären Datenbank verzögert, wenn sie Schemaaktualisierungen von der primären Datenbank erhält, die eine Schemasperre für die sekundäre Datenbank erfordern.

- **Aktive Georeplikation von elastischen Pooldatenbanken**: Die aktive Georeplikation kann für jede Datenbank in einem beliebigen Pool für elastische Datenbanken konfiguriert werden. Die sekundäre Datenbank kann sich in einem anderen Pool für elastische Datenbanken befinden. Für normale Datenbanken kann die sekundäre Datenbank ein Pool für elastische Datenbanken sein und umgekehrt, sofern die Dienstebenen identisch sind.

- **Konfigurierbare Leistungsstufe der sekundären Datenbank**: Eine sekundäre Datenbank kann mit einer niedrigeren Leistungsstufe erstellt werden als die primäre Datenbank. Sowohl die primäre als auch die sekundäre Datenbank müssen die gleiche Dienstebene aufweisen. Diese Option wird nicht für Anwendungen mit Datenbanken empfohlen, die eine hohe Schreibaktivität aufweisen, da die größere Replikationsverzögerung das Risiko erheblicher Datenverluste nach einem Failover steigert. Darüber hinaus wird nach einem Failover die Leistung der Anwendung beeinträchtigt, bis die neue primäre Datenbank auf eine höhere Leistungsstufe aktualisiert ist. Das Diagramm mit dem Protokoll-E/A-Prozentsatz im Azure-Portal bietet eine gute Möglichkeit, einzuschätzen, welche Leistungsstufe für die sekundäre Datenbank mindestens erforderlich ist, um die Replikationslast zu bewältigen. Wenn die Leistungsstufe der primären Datenbank beispielsweise P6 (1.000 DTU) ist und ihr Protokoll-E/A-Prozentsatz 50 % beträgt, muss die Leistungsstufe der sekundären Datenbank mindestens P4 (500 DTU) sein. Sie können die Protokoll-E/A-Daten auch mithilfe der Datenbanksicht [sys.resource\_stats](https://msdn.microsoft.com/library/dn269979.aspx) oder [sys.dm\_db\_resource\_stats](https://msdn.microsoft.com/library/dn800981.aspx) abrufen. Weitere Informationen zu den SQL-Datenbankleistungsstufen finden Sie unter [SQL-Datenbankoptionen und -leistung](sql-database-service-tiers.md).

- **Benutzergesteuertes Failover und Failback**: Eine sekundäre Datenbank kann jederzeit über eine explizite Aktion durch die Anwendung oder den Benutzer die primäre Rolle erhalten. Bei einem echten Ausfall sollte die Option „Nicht geplant“ verwendet werden, die eine sekundäre Datenbank umgehend zu einer primären Datenbank heraufstuft. Wenn die ausgefallene primäre Datenbank wiederhergestellt ist und wieder zur Verfügung steht, kennzeichnet das System diese wiederhergestellte primäre Datenbank automatisch als sekundäre Datenbank und bringt sie auf den aktuellen Stand der neuen primären Datenbank. Aufgrund der asynchronen Natur der Replikation kann eine kleine Menge von Daten bei einem nicht geplanten Failover verloren gehen, wenn eine primäre Datenbank ausfällt, bevor die aktuellen Änderungen in die sekundäre Datenbank repliziert wurden. Wenn für eine primäre Datenbank mit mehreren sekundären Datenbanken ein Failover durchgeführt wird, konfiguriert das System automatisch die Replikationsbeziehungen neu und verknüpft die verbleibenden sekundären Datenbanken mit der soeben heraufgestuften primären Datenbank, ohne dass ein Benutzereingriff erforderlich ist. Wenn der Ausfall behoben ist, durch den das Failover verursacht wurde, kann es wünschenswert sein, die Anwendung wieder in die primäre Region zurückzuführen. Zu diesem Zweck sollte der Failoverbefehl mit der Option „Geplant“ aufgerufen werden.

- **Synchronisieren von Anmeldeinformationen und Firewallregeln**: Wir empfehlen die Verwendung von [Datenbank-Firewallregeln](sql-database-firewall-configure.md) für georeplizierte Datenbanken, damit diese Regeln mit der Datenbank repliziert werden können. So wird sichergestellt, dass alle sekundären Datenbanken die gleichen Firewallregeln besitzen wie die primäre Datenbank. Mit diesem Ansatz müssen Kunden auf Servern, auf denen sowohl die primäre als auch die sekundäre Datenbank gehostet wird, keine Firewallregeln mehr manuell konfigurieren und verwalten. Analog dazu wird durch die Verwendung von [eigenständigen Datenbankbenutzern](sql-database-manage-logins.md) für den Datenzugriff sichergestellt, dass für primäre und sekundäre Datenbanken immer die gleichen Benutzeranmeldeinformationen gelten, damit bei einem Failover keine Unterbrechungen durch Benutzernamen- und Kennwortkonflikte auftreten. Durch Hinzufügen von [Azure Active Directory](../active-directory/active-directory-whatis.md) können Kunden den Benutzerzugriff sowohl für primäre als auch für sekundäre Datenbanken verwalten, sodass die Notwendigkeit der Verwaltung von Anmeldeinformationen in Datenbanken vollständig entfällt.

## Verhindern des Verlusts wichtiger Daten
Aufgrund der hohen Latenz von WANs wird für die fortlaufende Kopie ein asynchroner Replikationsmechanismus verwendet. Asynchrone Replikation macht Datenverlust unvermeidlich, sobald ein Ausfall auftritt. Bei einigen Anwendung dürfen jedoch ggf. keine Daten verloren gehen. Um diese kritischen Aktualisierungen zu schützen, kann ein Anwendungsentwickler die Systemprozedur [sp\_wait\_for\_database\_copy\_sync](https://msdn.microsoft.com/library/dn467644.aspx) aufrufen, unmittelbar nachdem der Commit für die Transaktion erfolgt ist. Das Aufrufen von **sp\_wait\_for\_database\_copy\_sync** blockiert den aufrufenden Thread so lange, bis die letzte Transaktion mit erfolgtem Commit in die sekundäre Datenbank repliziert wurde. Die Prozedur wartet, bis alle in der Warteschlange befindlichen Transaktionen von der sekundären Datenbank bestätigt wurden. **sp\_wait\_for\_database\_copy\_sync** ist auf eine spezifische fortlaufende Kopierverknüpfung begrenzt. Jeder Benutzer mit den Rechten zum Herstellen der Verbindung mit der primären Datenbank kann diese Prozedur aufrufen.

>[AZURE.NOTE] Die Verzögerung durch einen Aufruf der Prozedur **sp\_wait\_for\_database\_copy\_sync** kann erheblich sein. Die Verzögerung hängt von der aktuellen Größe des Transaktionsprotokolls ab, und dieser Aufruf sendet erst dann eine Rückmeldung, wenn das gesamte Protokoll repliziert ist. Vermeiden Sie den Aufruf dieser Prozedur, es sei denn, er ist unbedingt erforderlich.

## Programmgesteuertes Verwalten der aktiven Georeplikation

Wie bereits zuvor erwähnt, kann die aktive Georeplikation auch programmgesteuert mit Azure PowerShell und der REST-API verwaltet werden. Die folgenden Tabellen beschreiben den verfügbaren Satz von Befehlen.

- **Azure Resource Manager-API und rollenbasierte Sicherheit**: Die aktive Georeplikation umfasst eine Reihe von [Azure Resource Manager-APIs](https://msdn.microsoft.com/library/azure/mt163571.aspx) für die Verwaltung. Hierzu zählen auch [Azure Resource Manager-basierte PowerShell-Cmdlets](sql-database-geo-replication-powershell.md). Diese APIs erfordern die Verwendung von Ressourcengruppen und unterstützen rollenbasierte Sicherheit (RBAC). Weitere Informationen zur Implementierung von Zugriffsrollen finden Sie unter [Verwenden von Rollenzuweisungen zum Verwalten Ihrer Azure Active Directory-Ressourcen](../active-directory/role-based-access-control-configure.md).

>[AZURE.NOTE] Viele neue Features für die aktive Georeplikation werden nur in der auf [Azure Resource Manager](../resource-group-overview.md) basierenden [Azure SQL-REST-API](https://msdn.microsoft.com/library/azure/mt163571.aspx) und mit den [Azure SQL-Datenbank-PowerShell-Cmdlets](https://msdn.microsoft.com/library/azure/mt574084.aspx) unterstützt. Zur Gewährleistung der Abwärtskompatibilität werden zwar die (klassische) REST-API](https://msdn.microsoft.com/library/azure/dn505719.aspx) sowie [(klassische) Azure SQL-Datenbank-Cmdlets](https://msdn.microsoft.com/library/azure/dn546723.aspx) unterstützt, Sie sollten jedoch die Azure Resource Manager-basierten APIs verwenden.


### Transact-SQL

|Befehl|Beschreibung|
|-------|-----------|
|[ALTER DATABASE (Azure SQL-Datenbank)](https://msdn.microsoft.com/library/mt574871.aspx)|Verwenden Sie das Argument ADD SECONDARY ON SERVER, um eine sekundäre Datenbank für eine vorhandene Datenbank zu erstellen und die Datenreplikation zu starten.|
|[ALTER DATABASE (Azure SQL-Datenbank)](https://msdn.microsoft.com/library/mt574871.aspx)|Verwenden Sie FAILOVER oder FORCE\_FAILOVER\_ALLOW\_DATA\_LOSS, um die sekundäre Datenbank zur primären zu erklären und zu ihr zu wechseln – damit starten Sie das Failover.
|[ALTER DATABASE (Azure SQL-Datenbank)](https://msdn.microsoft.com/library/mt574871.aspx)|Verwenden Sie REMOVE SECONDARY ON SERVER, um die Datenreplikation zwischen einer SQL-Datenbank und der angegebenen sekundären Datenbank zu beenden.|
|[sys.geo\_replication\_links (Azure SQL-Datenbank)](https://msdn.microsoft.com/library/mt575501.aspx)|Gibt Informationen über alle vorhandenen Replikationsverknüpfungen für alle Datenbanken auf dem logischen Azure SQL-Datenbankserver zurück.|
|[sys.dm\_geo\_replication\_link\_status (Azure SQL-Datenbank)](https://msdn.microsoft.com/library/mt575504.aspx)|Ruft den Zeitpunkt der letzten Replikation, die Verzögerung der letzten Replikation und andere Informationen über die Replikationsverknüpfung für eine angegebene SQL-Datenbank ab.|
|[sys.dm\_operation\_status (Azure SQL-Datenbank)](https://msdn.microsoft.com/library/dn270022.aspx)|Zeigt den Status für alle Datenbankvorgänge an, einschließlich des Status der Replikationsverknüpfungen.|
|[sp\_wait\_for\_database\_copy\_sync (Azure SQL-Datenbank)](https://msdn.microsoft.com/library/dn467644.aspx)|Bewirkt, dass die Anwendung wartet, bis alle Transaktionen mit erfolgtem Commit repliziert und von der aktiven sekundären Datenbank bestätigt wurden.|
||||

### PowerShell

|Cmdlet|Beschreibung|
|------|-----------|
|[Get-AzureRmSqlDatabase](https://msdn.microsoft.com/de-DE/library/azure/mt603648.aspx)|Ruft mindestens eine Datenbank ab.|
|[New-AzureRmSqlDatabaseSecondary](https://msdn.microsoft.com/library/mt603689.aspx)|Erstellt eine sekundäre Datenbank für eine vorhandene Datenbank und startet die Datenreplikation.|
|[Set-AzureRmSqlDatabaseSecondary](https://msdn.microsoft.com/de-DE/library/mt619393.aspx)|Erklärt die sekundäre Datenbank zur primären und wechselt zu ihr – dadurch wird das Failover gestartet.|
|[Remove-AzureRmSqlDatabaseSecondary](https://msdn.microsoft.com/de-DE/library/mt603457.aspx)|Beendet die Datenreplikation zwischen einer SQL-Datenbank und der angegebenen sekundären Datenbank.|
|[Get-AzureRmSqlDatabaseReplicationLink](https://msdn.microsoft.com/library/mt619330.aspx)|Ruft die Georeplikationsverknüpfungen zwischen einer Azure SQL-Datenbank und einer Ressourcengruppe oder einer SQL Server-Instanz ab.|
||||

### REST-API

|API|Beschreibung|
|---|-----------|
|[Create or Update Database (createMode=Restore)](https://msdn.microsoft.com/library/azure/mt163685.aspx)|Erstellt oder aktualisiert eine primäre oder sekundäre Datenbank oder stellt diese wieder her.|
|[Get Create or Update Database Status](https://msdn.microsoft.com/library/azure/mt643934.aspx)|Ruft den Status während eines Erstellungsvorgangs ab.|
|[Set Secondary Database as Primary (Planned Failover)](https://msdn.microsoft.com/ibrary/azure/mt575007.aspx)|Stuft eine sekundäre Datenbank in einer Georeplikationspartnerschaft zur neuen primären Datenbank hoch.|
|[Set Secondary Database as Primary (Unplanned Failover)](https://msdn.microsoft.com/library/azure/mt582027.aspx)|Erzwingt ein Failover zur sekundären Datenbank und legt die sekundäre Datenbank als primäre fest.|
|[Get Replication Links](https://msdn.microsoft.com/library/azure/mt600929.aspx)|Ruft alle Replikationsverknüpfungen für eine angegebene SQL-Datenbank in einer Georeplikationspartnerschaft ab. Es werden die Informationen abgerufen, die in der Katalogsicht „sys.geo\_replication\_links“ sichtbar sind.|
|[Get Replication Link](https://msdn.microsoft.com/library/azure/mt600778.aspx)|Ruft eine spezifische Replikationsverknüpfung für eine angegebene SQL-Datenbank in einer Georeplikationspartnerschaft ab. Es werden die Informationen abgerufen, die in der Katalogsicht „sys.geo\_replication\_links“ sichtbar sind.|
||||



## Nächste Schritte

- Eine Übersicht und verschiedene Szenarien zum Thema Geschäftskontinuität finden Sie unter [Übersicht über die Geschäftskontinuität](sql-database-business-continuity.md).
- Informationen über automatisierte Sicherungen von Azure SQL-Datenbanken finden Sie unter [Übersicht: Automatisierte SQL-Datenbanksicherungen](sql-database-automated-backups.md).
- Informationen zum Verwenden automatisierter Sicherungen für die Wiederherstellung finden Sie unter [Wiederherstellen einer Datenbank aus vom Dienst initiierten Sicherungen](sql-database-recovery-using-backups.md).
- Informationen zum Verwenden automatisierter Sicherungen für die Archivierung finden Sie unter [Datenbankkopie](sql-database-copy.md).
- Weitere Informationen zu Authentifizierungsanforderungen für einen neuen primären Server und die Datenbank finden Sie unter [Verwalten der Sicherheit der Azure SQL-Datenbank nach der Notfallwiederherstellung](sql-database-geo-replication-security-config.md).

<!---HONumber=AcomDC_0907_2016-->