<properties
	pageTitle="Sicherheitskonfiguration für die aktive Georeplikation"
	description="In diesem Thema werden Sicherheitsaspekte für die Verwaltung von Szenarien der aktiven Georeplikation für die SQL-Datenbank erläutert."
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
	ms.date="04/27/2016"
	ms.author="carlrab" />

# Sicherheitskonfiguration für die Georeplikation

>[AZURE.NOTE] [Active Geo-Replication](sql-database-geo-replication-overview.md) ist jetzt für alle Datenbanken in allen Diensttarifen verfügbar.

## Übersicht über die Authentifizierungsanforderungen für die aktive Georeplikation
In diesem Thema werden die Authentifizierungsanforderungen zum Konfigurieren und Steuern der [aktiven Georeplikation](sql-database-geo-replication-overview.md) sowie die erforderlichen Schritte zum Einrichten des Benutzerzugriffs auf die sekundäre Datenbank beschrieben. Weitere Informationen zur Verwendung der Georeplikation finden Sie unter [Wiederherstellen einer Azure SQL-Datenbank nach einem Ausfall](sql-database-disaster-recovery.md).

## Verwenden der aktiven Georeplikation mit eigenständigen Benutzern
Mit der [V12-Version von Azure SQL-Datenbank](sql-database-v12-whats-new.md) unterstützt SQL-Datenbank jetzt eigenständige Benutzer. Im Gegensatz zu herkömmlichen Benutzern, die Anmeldungen in der „master“-Datenbank zugeordnet werden müssen, wird ein eigenständiger Benutzer vollständig von der Datenbank selbst verwaltet. Dies hat zwei Vorteile. Beim Georeplikationsszenario können sich die Benutzer weiter ohne zusätzliche Konfiguration mit der sekundären Datenbank verbinden, da die Datenbank die Benutzer verwaltet. Es gibt bei dieser Konfiguration auch vom Standpunkt der Anmeldung potenzielle Skalierbarkeits- und Leistungsvorteile. Weitere Informationen finden Sie unter [Eigenständige Datenbankbenutzer – machen Sie Ihre Datenbank portabel](https://msdn.microsoft.com/library/ff929188.aspx).

Wenn mehrere Ihrer Datenbanken dieselben Anmeldedaten verwenden, kann sich die Verwaltung der Anmeldeinformationen anhand von eigenständigen Benutzern in mehreren Datenbanken nachteilig auswirken. Bei einer Kennwortänderung kann das Kennwort für den Anmeldenamen beispielsweise nicht einmalig auf Serverebene geändert werden. Stattdessen muss die Änderung für den eigenständigen Benutzer separat in jeder Datenbank erfolgen. Wenn Sie daher mehrere Datenbanken mit demselben Benutzernamen und Kennwort verwenden, wird von der Verwendung eigenständiger Benutzer abgeraten.

## Verwenden von Anmeldenamen und Benutzern mit der aktiven Georeplikation
Wenn Sie mit Anmeldungen und Benutzern (nicht mit eigenständigen Benutzern) arbeiten, müssen zusätzliche Schritte durchgeführt werden, um sicherzustellen, dass dieselben Anmeldenamen auf dem sekundären Datenbankserver vorhanden sind. In den folgenden Abschnitten werden die entsprechenden Schritte und zusätzliche Aspekte behandelt.

### Einrichten des Benutzerzugriffs auf eine sekundäre Datenbank
Damit die sekundäre Datenbank nach einem Failover als schreibgeschützte sekundäre Datenbank oder einsetzbare primäre Datenbank genutzt werden kann, muss die sekundäre Datenbank über die entsprechende Sicherheitskonfiguration verfügen.

Der Serveradministrator oder Benutzer mit entsprechenden Berechtigungen können die in diesem Thema beschriebenen Konfigurationsschritte ausführen. Die spezifischen Berechtigungen für die einzelnen Schritte werden weiter unten in diesem Thema beschrieben.

Die Vorbereitung des Benutzerzugriffs auf eine sekundäre Onlinedatenbank für die aktive Georeplikation kann jederzeit erfolgen. Dies umfasst die drei folgenden Schritte:

1. Bestimmen der Anmeldungen mit Zugriff auf die primäre Datenbank
2. Suchen der SID für diese Anmeldungen auf dem Quellserver
3. Erstellen Sie die Anmeldenamen auf dem Zielserver mit der entsprechenden SID vom Quellserver.

>[AZURE.NOTE] Wenn die Anmeldenamen auf dem Zielserver den Benutzern in der sekundären Datenbank nicht ordnungsgemäß zugeordnet sind, ist der schreibgeschützte Zugriff auf die Datenbank oder der Zugriff auf die neue primäre Datenbank nach dem Failover nur auf den Serveradministrator beschränkt.

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
Weitere Informationen zur aktiven Georeplikation finden Sie unter [Aktive Georeplikation](sql-database-geo-replication-overview.md).


## Zusätzliche Ressourcen

- [Übersicht über die Geschäftskontinuität](sql-database-business-continuity.md)
- [Aktive Georeplikation](sql-database-geo-replication-overview.md)
- [Entwerfen einer Anwendung für die cloudbasierte Notfallwiederherstellung](sql-database-designing-cloud-solutions-for-disaster-recovery.md)
- [Abschließen der wiederhergestellten Azure SQL-Datenbank](sql-database-recovered-finalize.md)
- [BCDR in SQL-Datenbank – Häufig gestellte Fragen](sql-database-bcdr-faq.md)

<!---HONumber=AcomDC_0504_2016-->