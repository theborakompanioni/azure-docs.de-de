<properties
	pageTitle="Verwalten der Sicherheit nach der Wiederherstellung einer Datenbank auf einem neuen Server oder nach einem Failover einer Datenbank in eine sekundäre Datenbankkopie | Microsoft Azure"
	description="In diesem Thema werden Sicherheitsaspekte bei der Verwaltung der Sicherheit nach einer Wiederherstellung oder einem Failover einer Datenbank erläutert."
	services="sql-database"
	documentationCenter="na"
	authors="carlrabeler"
	manager="jhubbard"
	editor="monicar" />


<tags
	ms.service="sql-database"
	ms.devlang="na"
	ms.topic="article"
	ms.tgt_pltfrm="na"
	ms.workload="data-management"
	ms.date="06/16/2016"
	ms.author="carlrab" />

# Verwalten der Sicherheit der Azure SQL-Datenbank nach der Notfallwiederherstellung

>[AZURE.NOTE] [Active Geo-Replication](sql-database-geo-replication-overview.md) ist jetzt für alle Datenbanken in allen Diensttarifen verfügbar.

## Übersicht über die Authentifizierungsanforderungen für die Notfallwiederherstellung

In diesem Thema werden die Authentifizierungsanforderungen zum Konfigurieren und Steuern der [aktiven Georeplikation](sql-database-geo-replication-overview.md) sowie die erforderlichen Schritte zum Einrichten des Benutzerzugriffs auf die sekundäre Datenbank beschrieben. Außerdem wird beschrieben, wie nach der Geowiederherstellung der Zugriff auf die wiederhergestellte Datenbank aktiviert wird. Weitere Informationen zu Wiederherstellungsoptionen finden Sie unter [Restore an Azure SQL Database or failover to a secondary](sql-database-disaster-recovery.md) (Wiederherstellen einer Azure SQL-Datenbank oder Failover zu einer sekundären Datenbank).

## Notfallwiederherstellung mit eigenständigen Benutzern

Im Gegensatz zu herkömmlichen Benutzern, die Anmeldungen in der „master“-Datenbank zugeordnet werden müssen, wird ein eigenständiger Benutzer vollständig von der Datenbank selbst verwaltet. Dies hat zwei Vorteile. Beim Notfallwiederherstellungs-Szenario können sich die Benutzer weiter ohne zusätzliche Konfiguration mit der neuen primären Datenbank bzw. mit der Geowiederherstellung wiederhergestellten Datenbank verbinden, da die Datenbank die Benutzer verwaltet. Es gibt bei dieser Konfiguration auch vom Standpunkt der Anmeldung potenzielle Skalierbarkeits- und Leistungsvorteile. Weitere Informationen finden Sie unter [Eigenständige Datenbankbenutzer – machen Sie Ihre Datenbank portabel](https://msdn.microsoft.com/library/ff929188.aspx).

Der Hauptaspekt des Kompromisses ist, dass die Verwaltung des Notfallwiederherstellungs-Prozesses im Verhältnis eine größere Herausforderung darstellt. Wenn mehrere Ihrer Datenbanken dieselben Anmeldedaten verwenden, kann sich die Verwaltung der Anmeldeinformationen anhand von eigenständigen Benutzern in mehreren Datenbanken nachteilig auswirken. Die Kennwortrotationsrichtlinie erfordert beispielsweise, dass Änderungen konsistent in mehreren Datenbanken durchgeführt werden, anstatt das Kennwort für die Anmeldung einmal in der Masterdatenbank zu ändern. Wenn Sie mehrere Datenbanken mit dem gleichen Benutzernamen und Kennwort verwenden, sollten Sie darum keine eigenständigen Benutzer verwenden.

## Konfigurieren von Anmeldungen und Benutzern

Wenn Sie mit Anmeldungen und Benutzern (nicht mit eigenständigen Benutzern) arbeiten, müssen Sie zusätzliche Schritte durchführen, um sicherzustellen, dass die gleichen Anmeldenamen in der Masterdatenbank vorhanden sind. In den folgenden Abschnitten werden die entsprechenden Schritte und zusätzliche Aspekte behandelt.

### Einrichten des Benutzerzugriffs auf eine sekundäre bzw. wiederhergestellte Datenbank

Damit die sekundäre Datenbank als schreibgeschützte sekundäre Datenbank verwendet werden kann, und um den ordnungsgemäßen Zugriff auf die neue primäre Datenbank bzw. mit der Geowiederherstellung wiederhergestellte Datenbank sicherzustellen, muss für die Masterdatenbank des Zielservers vor der Wiederherstellung die geeignete Sicherheitskonfiguration durchgeführt werden.

Die spezifischen Berechtigungen für die einzelnen Schritte werden weiter unten in diesem Thema beschrieben.

Die Vorbereitung des Benutzerzugriffs auf eine mit Georeplikation erzeugte sekundäre Datenbank sollte im Rahmen der Konfiguration der Georeplikation erfolgen. Die Vorbereitung des Benutzerzugriffs auf mit der Geowiederherstellung wiederhergestellte Datenbanken sollte zu einem beliebigen Zeitpunkt erfolgen, wenn der ursprüngliche Server online ist (z.B. als Teil des DR-Drills).

>[AZURE.NOTE] Wenn Sie das Failover zu einem Server oder die Geowiederherstellung auf einem Server ausführen, für den die Anmeldenamen nicht ordnungsgemäß konfiguriert sind, beschränkt sich der Zugriff auf das Serveradministratorkonto.

Das Einrichten von Anmeldenamen auf dem Zielserver umfasst die unten beschriebenen drei Schritte:

#### 1\. Bestimmen der Anmeldungen mit Zugriff auf die primäre Datenbank:
Der erste Schritt des Prozesses ist das Bestimmen, welche Anmeldungen auf dem Zielserver dupliziert werden müssen. Dies geschieht mit zwei SELECT-Anweisungen, die auf die logische „master“-Datenbank auf dem Quellserver und auf die primäre Datenbank selbst angewendet werden.

Nur der Serveradministrator oder ein Mitglied der Serverrolle **LoginManager** kann mit der folgenden SELECT-Anweisung die Anmeldungen auf dem Quellserver bestimmen.

	SELECT [name], [sid] 
	FROM [sys].[sql_logins] 
	WHERE [type_desc] = 'SQL_Login'

Nur ein Mitglied der Datenbankrolle „db\_owner“, der Benutzer „dbo“ oder der Serveradministrator kann alle Datenbankbenutzerprinzipale in der primären Datenbank bestimmen.

	SELECT [name], [sid]
	FROM [sys].[database_principals]
	WHERE [type_desc] = 'SQL_USER'

#### 2\. Suchen der SID für die in Schritt 1 identifizierten Anmeldungen:
Durch Vergleichen der Ausgabe der Abfragen aus dem vorherigen Abschnitt und Zuordnen der SIDs können Sie die Serveranmeldung dem Datenbankbenutzer zuordnen. Anmeldungen, die über einen Datenbankbenutzer mit einer entsprechenden SID verfügen, haben als Datenbankbenutzerprinzipal Benutzerzugriff auf diese Datenbank.

Die folgende Abfrage kann verwendet werden, um alle Benutzerprinzipale mit ihre SIDs in einer Datenbank anzuzeigen. Nur ein Mitglied der Datenbankrolle „db\_owner“ oder Serveradministrator kann diese Abfrage ausführen.

	SELECT [name], [sid]
	FROM [sys].[database_principals]
	WHERE [type_desc] = 'SQL_USER'

>[AZURE.NOTE] Die Benutzer **INFORMATION\_SCHEMA** und **sys** haben die SID *NULL*, und die SID von **guest** ist **0x00**. Die SID von **dbo** kann mit *0x01060000000001648000000000048454* beginnen, wenn der Datenbankersteller der Serveradministrator und kein Mitglied von **DbManager** war.

#### 3\. Erstellen der Anmeldenamen auf dem Zielserver:
Der letzte Schritt besteht darin, auf dem oder den Zielservern die Anmeldungen mit den entsprechenden SIDs zu erstellen. Die grundlegende Syntax ist wie folgt.

	CREATE LOGIN [<login name>]
	WITH PASSWORD = <login password>,
	SID = <desired login SID>

>[AZURE.NOTE] Wenn Sie Benutzerzugriff auf die sekundäre Datenbank, aber nicht auf die primäre gewähren möchten, ändern Sie dazu die Benutzeranmeldung auf dem primären Server mithilfe der folgenden Syntax.
>
>ALTER LOGIN <login name> DISABLE
>
>DISABLE ändert nicht das Kennwort, sodass Sie sie bei Bedarf stets aktivieren können.

## Nächste Schritte

- Weitere Informationen zum Verwalten von Datenbankzugriff und Anmeldungen finden Sie unter [Sicherheit von SQL-Datenbank: Verwalten von Datenbankzugriff und Anmeldesicherheit](sql-database-manage-logins.md).
- Weitere Informationen zu eigenständigen Datenbankbenutzern finden Sie unter [Eigenständige Datenbankbenutzer – machen Sie Ihre Datenbank portabel](https://msdn.microsoft.com/library/ff929188.aspx).
- Informationen zur Verwendung und Konfiguration der aktiven Georeplikation finden Sie unter [Aktive Georeplikation](sql-database-geo-replication-overview.md).
- Informationen zur Verwendung der Geowiederherstellung finden Sie unter [Geowiederherstellung](sql-database-geo-restore.md)

## Zusätzliche Ressourcen

- [Geschäftskontinuität und Notfallwiederherstellung mit SQL-Datenbank](sql-database-business-continuity.md)
- [Point-in-Time-Wiederherstellung](sql-database-point-in-time-restore.md)
- [Geografische Wiederherstellung](sql-database-geo-restore.md)
- [Aktive Georeplikation](sql-database-geo-replication-overview.md)
- [Entwerfen einer Anwendung für die cloudbasierte Notfallwiederherstellung](sql-database-designing-cloud-solutions-for-disaster-recovery.md)
- [Abschließen der wiederhergestellten Azure SQL-Datenbank](sql-database-recovered-finalize.md)
- [Sicherheitskonfiguration für die Georeplikation](sql-database-geo-replication-security-config.md)
- [BCDR in SQL-Datenbank – Häufig gestellte Fragen](sql-database-bcdr-faq.md)

<!---HONumber=AcomDC_0622_2016-->