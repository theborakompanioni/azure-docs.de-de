<properties
   pageTitle="Sichern einer Datenbank in SQL Data Warehouse | Microsoft Azure"
   description="Tipps für das Sichern einer Datenbank in Azure SQL Data Warehouse für die Entwicklung von Lösungen."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="ronortloff"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="07/22/2016"
   ms.author="rortloff;barbkess;sonyama"/>

# Sichern einer Datenbank in SQL Data Warehouse

> [AZURE.SELECTOR]
- [Sicherheitsübersicht](sql-data-warehouse-overview-manage-security.md)
- [Bedrohungserkennung](sql-data-warehouse-security-threat-detection.md)
- [Verschlüsselung (Portal)](sql-data-warehouse-encryption-tde.md)
- [Verschlüsselung (T-SQL)](sql-data-warehouse-encryption-tde-tsql.md)
- [Übersicht über die Überwachung](sql-data-warehouse-auditing-overview.md)
- [Überwachung für Vorgängerversionsclients](sql-data-warehouse-auditing-downlevel-clients.md)



Dieser Artikel beschreibt die Grundlagen zum Sichern der Azure SQL Data Warehouse-Datenbank. Insbesondere erhalten Sie in diesem Artikel erste Informationen über Ressourcen zum Einschränken des Zugriffs, zum Schützen von Daten und zum Überwachen von Aktivitäten in einer Datenbank.

## Verbindungssicherheit

Verbindungssicherheit bezieht sich darauf, auf welche Weise Sie die Verbindungen zu Ihrer Datenbank mithilfe von Firewall-Regeln und Verbindungsverschlüsselung einschränken und sichern.

Firewall-Regeln werden vom Server und der Datenbank verwendet, um Verbindungsversuche von IP-Adressen abzuwehren, die nicht explizit der weißen Liste hinzugefügt wurden. Damit von Ihrer Anwendung oder von der öffentlichen IP-Adresse Ihres Clientcomputers aus Verbindungen hergestellt werden können, müssen Sie zuerst über das klassische Azure-Portal, über REST-API oder PowerShell eine Firewallregel auf Serverebene erstellen. Eine bewährte Methode besteht darin, die von der Server-Firewall zugelassenen IP-Adressbereiche so weit wie möglich einzuschränken. Um von Ihrem lokalen Computer aus auf Azure SQL Data Warehouse zuzugreifen, stellen Sie sicher, dass die Firewall im Netzwerk und auf dem lokalen Computer eine ausgehende Kommunikation an TCP-Port 1433 zulässt. Weitere Informationen finden Sie unter [Azure SQL-Datenbankfirewall][], [sp\_set\_firewall\_rule][] und [sp\_set\_database\_firewall\_rule][].

Verbindungen mit SQL Data Warehouse können verschlüsselt werden, indem Sie den Verschlüsselungsmodus in der Verbindungszeichenfolge festlegen. Die Syntax zum Aktivieren der Verschlüsselung für die Verbindung variiert je nach Protokoll. Um Ihre Verbindungszeichenfolge festzulegen, navigieren Sie im Azure-Portal zu Ihrer Datenbank. Klicken Sie unter *Essentials* auf *Datenbank-Verbindungszeichenfolgen anzeigen*.


## Authentifizierung

Authentifizierung bezieht sich darauf, auf welche Weise Sie Ihre Identität beim Herstellen der Verbindung mit der Datenbank nachweisen. SQL Data Warehouse unterstützt derzeit die SQL Server-Authentifizierung mit Benutzername und Kennwort sowie eine Vorschauversion von Azure Active Directory.

Bei der Erstellung des logischen Servers für die Datenbank haben Sie eine "Server Admin"-Anmeldung mit Benutzername und Kennwort angegeben. Mit diesen Anmeldeinformationen können Sie sich mittels SQL Server-Authentifizierung bei jeder Datenbank auf diesem Server als Datenbankbesitzer bzw. „dbo“ (database owner) authentifizieren.

Allerdings sollten die Benutzer in Ihrer Organisation als bewährte Methode ein anderes Konto für die Authentifizierung verwenden. Auf diese Weise können Sie die Berechtigungen für die Anwendung beschränken und die Risiken schädlicher Aktivitäten reduzieren, falls Ihr Anwendungscode für einen SQL-Injection-Angriff anfällig ist.

Um einen authentifizierten SQL Server-Benutzer zu erstellen, stellen Sie mit den Anmeldeinformationen des Serveradministrators eine Verbindung mit der **Masterdatenbank** auf dem Server her, und erstellen Sie eine neue Serveranmeldung.

```sql
-- Connect to master database and create a login
CREATE LOGIN ApplicationLogin WITH PASSWORD = 'strong_password';

```

Stellen Sie dann mit den Anmeldedaten für den Serveradministrator eine Verbindung mit der **SQL Data Warehouse-Datenbank** her, und erstellen Sie einen Datenbankbenutzer basierend auf der gerade erstellten Serveranmeldung.

```sql
-- Connect to SQL DW database and create a database user
CREATE USER ApplicationUser FOR LOGIN ApplicationLogin;

```

Weitere Informationen zur Authentifizierung bei einer SQL-Datenbank finden Sie unter [Verwalten von Datenbanken und Anmeldungen in der Azure SQL-Datenbank][]. Weitere Informationen zur Verwendung der Azure AD-Vorschau für SQL Data Warehouse finden Sie unter [Herstellen einer Verbindung mit SQL Data Warehouse mithilfe der Azure Active Directory-Authentifizierung][].


## Autorisierung

Autorisierung bezieht sich darauf, welche Aufgaben Sie in einer Azure SQL Data Warehouse-Datenbank ausführen können. Dies wird durch die Rollenmitgliedschaften und Berechtigungen Ihres Benutzerkontos gesteuert. Als bewährte Methode sollten Sie Benutzern nur die minimal erforderlichen Berechtigungen erteilen. Azure SQL Data Warehouse vereinfacht die Verwaltung durch Rollen in T-SQL:

```sql
EXEC sp_addrolemember 'db_datareader', 'ApplicationUser'; -- allows ApplicationUser to read data
EXEC sp_addrolemember 'db_datawriter', 'ApplicationUser'; -- allows ApplicationUser to write data
```

Das Server-Admin-Konto, mit dem Sie eine Verbindung herstellen, ist Mitglied von db\_owner und verfügt daher über alle Berechtigungen in der Datenbank. Speichern Sie dieses Konto für die Bereitstellung von Schemaänderungen und andere Verwaltungsvorgänge. Verwenden Sie das Konto "ApplicationUser" mit eingeschränkteren Berechtigungen, um eine Verbindung von Ihrer Anwendung zur Datenbank mit den geringsten Berechtigungen herzustellen, die von Ihrer Anwendung benötigt werden.

Es gibt Möglichkeiten, Benutzerberechtigungen für die Azure SQL-Datenbank noch weiter einzuschränken:

- Mithilfe von granularen [Berechtigungen][] können Sie steuern, welche Aufgaben in einzelnen Spalten, Tabellen, Ansichten, Prozeduren und anderen Objekten in der Datenbank ausgeführt werden dürfen. Verwenden Sie präzise Berechtigungen, um die größtmögliche Kontrolle zu haben und die geringstmöglichen Berechtigungen zu erteilen. Das präzise Berechtigungssystem ist etwas kompliziert, und seine effektive Nutzung erfordert etwas Übung.
- [Datenbankrollen][] können, mit Ausnahme von "db\_datareader" und "db\_datawriter", dazu verwendet werden, leistungsstärkere Benutzerkonten für Anwendungen oder weniger leistungsstarke Verwaltungskonten zu erstellen. Die integrierten festen Datenbankrollen bieten eine einfache Möglichkeit zum Erteilen von Berechtigungen, können jedoch auch dazu führen, dass mehr Berechtigungen als nötig erteilt werden.
- [Gespeicherten Prozeduren][] können verwendet werden, um die Aktionen zu begrenzen, die in der Datenbank ausgeführt werden können.

Die Verwaltung von Datenbanken und logischen Servern über das klassische Azure-Portal oder mit der Azure-Ressourcen-Manager-API wird durch die Rollenzuweisungen Ihres Portal-Benutzerkontos gesteuert. Weitere Informationen zu diesem Thema finden Sie unter [Rollenbasierte Zugriffssteuerung im Azure-Portal][].

## Verschlüsselung

Durch [Transparent Data Encryption][] kann die Azure SQL-Datenbank zum Schutz Ihrer Daten beitragen, indem ruhende oder in Datenbankdateien und Sicherungen gespeicherte Daten verschlüsselt werden. Sie müssen Administrator oder ein Mitglied der Rolle „dbmanager“ in der Masterdatenbank sein, um TDE zu aktivieren. Um Ihre Datenbank zu verschlüsseln, stellen Sie auf Ihrem Server eine Verbindung mit der Masterdatenbank her und führen Folgendes aus:


```sql

ALTER DATABASE [AdventureWorks] SET ENCRYPTION ON;

```

Sie können Transparent Data Encryption auch über die Datenbankeinstellungen im [Azure-Portal][] aktivieren. Weitere Informationen hierzu finden Sie unter [Erste Schritte mit Transparent Data Encryption (TDE)][].

## Überwachung

Die Überwachung und Nachverfolgung von Datenbankereignissen kann Sie bei der Einhaltung von gesetzlichen Bestimmungen und der Erkennung von verdächtigen Aktivitäten unterstützen. Mit SQL Data Warehouse-Überwachung können Sie Ereignisse in der Datenbank in einem Überwachungsprotokoll in Ihrem Azure-Speicherkonto aufzeichnen. SQL Data Warehouse-Überwachung kann auch in Microsoft Power BI integriert werden, um detaillierte Berichte und Analysen zu ermöglichen. Weitere Informationen finden Sie unter [Erste Schritte mit der SQL-Datenbanküberwachung][].

## Nächste Schritte
Informationen und Beispiele zum Herstellen einer Verbindung mit SQL Data Warehouse über verschiedene Protokolle finden Sie unter [Herstellen einer Verbindung mit SQL Data Warehouse][].

<!--Image references-->

<!--Article references-->
[Herstellen einer Verbindung mit SQL Data Warehouse]: ./sql-data-warehouse-develop-connections.md
[Erste Schritte mit der SQL-Datenbanküberwachung]: ./sql-data-warehouse-overview-auditing.md
[Erste Schritte mit Transparent Data Encryption (TDE)]: ./sql-data-warehouse-encryption-tde.md
[Herstellen einer Verbindung mit SQL Data Warehouse mithilfe der Azure Active Directory-Authentifizierung]: ./sql-data-warehouse-get-started-connect-aad-authentication.md

<!--MSDN references-->
[Azure SQL-Datenbankfirewall]: https://msdn.microsoft.com/library/ee621782.aspx
[sp\_set\_firewall\_rule]: https://msdn.microsoft.com/library/dn270017.aspx
[sp\_set\_database\_firewall\_rule]: https://msdn.microsoft.com/library/dn270010.aspx
[Datenbankrollen]: https://msdn.microsoft.com/library/ms189121.aspx
[Verwalten von Datenbanken und Anmeldungen in der Azure SQL-Datenbank]: https://msdn.microsoft.com/library/ee336235.aspx
[Berechtigungen]: https://msdn.microsoft.com/library/ms191291.aspx
[Gespeicherten Prozeduren]: https://msdn.microsoft.com/library/ms190782.aspx
[Transparent Data Encryption]: https://go.microsoft.com/fwlink/?LinkId=526242
[Azure-Portal]: https://portal.azure.com/

<!--Other Web references-->
[Rollenbasierte Zugriffssteuerung im Azure-Portal]: https://azure.microsoft.com/documentation/articles/role-based-access-control-configure

<!---HONumber=AcomDC_0727_2016-->