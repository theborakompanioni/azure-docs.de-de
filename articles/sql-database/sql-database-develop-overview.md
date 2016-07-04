<properties
	pageTitle="Übersicht über die Entwicklung von SQL-Datenbanken | Microsoft Azure"
	description="Hier erfahren Sie mehr über verfügbare Verbindungsbibliotheken und bewährte Methoden für Anwendungen, die eine Verbindung mit SQL-Datenbank herstellen."
	services="sql-database"
	documentationCenter=""
	authors="annemill"
	manager="jhubbard"
	editor="genemi"/>


<tags
	ms.service="sql-database"
	ms.workload="data-management"
	ms.tgt_pltfrm="na"
	ms.devlang="na"
	ms.topic="article"
	ms.date="05/02/2016"
	ms.author="annemill"/>

# Übersicht über die Entwicklung von SQL-Datenbanken
In diesem Artikel werden die grundlegenden Aspekte beschrieben, die ein Entwickler beim Schreiben von Code zum Herstellen einer Verbindung mit Azure SQL-Datenbanken berücksichtigen sollte.

## Sprache und Plattform
Für eine Vielzahl von Programmiersprachen und Plattformen sind Codebeispiele verfügbar. Links zu den Codebeispielen finden Sie unter:

* Weitere Informationen: [Verbindungsbibliotheken für SQL-Datenbank und SQL Server](sql-database-libraries.md)

## Ressourcenbeschränkungen
Azure SQL-Datenbank verwaltet die für eine Datenbank verfügbaren Ressourcen mithilfe zweier verschiedener Mechanismen: Ressourcenkontrolle und Durchsetzung von Grenzen.

* Weitere Informationen: [Ressourceneinschränkungen für Azure SQL-Datenbank](sql-database-resource-limits.md)

## Sicherheit
Azure SQL-Datenbank stellt Ressourcen zum Einschränken des Zugriffs, zum Schützen von Daten sowie zum Überwachen von Aktivitäten in einer SQL-Datenbank bereit.

* Weitere Informationen: [Sichern der SQL-Datenbank](sql-database-security.md)

## Authentifizierung
* Azure SQL-Datenbank unterstützt Benutzer und Anmeldungen mit SQL Server-Authentifizierung sowie Benutzer und Anmeldungen mit [Azure Active Directory-Authentifizierung](sql-database-aad-authentication.md).
* Sie müssen eine bestimmte Datenbank angeben, anstatt automatisch die *master*-Datenbank zu verwenden.
* Sie können die Transact-SQL-Anweisung **USE myDatabaseName;** in SQL-Datenbanken nicht verwenden, um zu einer anderen Datenbank zu wechseln.
* Weitere Informationen: [Sicherheit von SQL-Datenbank: Verwalten von Datenbankzugriff und Anmeldesicherheit](sql-database-manage-logins.md)

## Resilienz
Tritt beim Verbinden mit SQL-Datenbank ein vorübergehender Fehler auf, muss der Code den Aufruf wiederholen. Die Wiederholungslogik sollte Backoff-Logik verwenden, damit die SQL-Datenbank nicht unnötig überlastet wird, wenn mehrere Clients die Wiederholung gleichzeitig durchführen.

* Codebeispiele: Codebeispiele, die die Wiederholungslogik veranschaulichen, finden Sie in den Beispielen für die Sprache Ihrer Wahl unter [Verbindungsbibliotheken für SQL-Datenbank und SQL Server](sql-database-libraries.md).
* Weitere Informationen: [Fehlermeldungen für Clientprogramme der SQL-Datenbank](sql-database-develop-error-messages.md)

## Verwalten von Verbindungen
* Ändern Sie in der Clientverbindungslogik das Standardtimeout in 30 Sekunden. Der Standardwert von 15 Sekunden ist zu kurz für Verbindungen, die über das Internet hergestellt werden.
* Wenn Sie einen [Verbindungspool](http://msdn.microsoft.com/library/8xx3tyca.aspx) verwenden, trennen Sie unbedingt die Verbindung, sobald Ihre Anwendung sie nicht aktiv verwendet und nicht gerade auf die erneute Verwendung der Verbindung vorbereitet wird.

## Netzwerküberlegungen
* Vergewissern Sie sich, dass auf dem Computer, der das Clientprogramm hostet, die Firewall ausgehende TCP-Kommunikation über Port 1433 zulässt. Weitere Informationen: [Konfigurieren einer Firewall für die Azure SQL-Datenbank mit dem Azure-Portal](sql-database-configure-firewall-settings.md)
* Wenn Ihr Clientprogramm eine Verbindung mit SQL-Datenbank V12 herstellt, wobei der Client auf einem virtuellen Azure-Computer ausgeführt wird, müssen Sie bestimmte Portbereiche auf dem virtuellen Computer öffnen. Weitere Informationen: [Andere Ports als 1433 für ADO.NET 4.5 und SQL-Datenbank V12](sql-database-develop-direct-route-ports-adonet-v12.md)
* Bei Clientverbindungen mit Azure SQL-Datenbank V12 wird der Proxy manchmal umgangen und direkt mit der Datenbank interagiert. Andere Ports als 1433 werden wichtig. Weitere Informationen: [Andere Ports als 1433 für ADO.NET 4.5 und SQL-Datenbank V12](sql-database-develop-direct-route-ports-adonet-v12.md)

## Datensharding mit elastischer Skalierung
Die elastische Skalierung vereinfacht das horizontale Hoch- und Herunterskalieren.

[Entwurfsmuster für mehrinstanzenfähige SaaS-Anwendungen und Azure SQL-Datenbank]
* (./sql-database/sql-database-design-patterns-multi-tenancy-saas-applications.md)
* [Datenabhängiges Routing](sql-database-elastic-scale-data-dependent-routing.md)
* [Erste Schritte mit der Vorschauversion von Elastic Scale für Azure SQL-Datenbank](sql-database-elastic-scale-get-started.md)

## Nächste Schritte

Entdecken Sie alle [Funktionen von SQL-Datenbank](https://azure.microsoft.com/services/sql-database/).

<!---HONumber=AcomDC_0622_2016-->