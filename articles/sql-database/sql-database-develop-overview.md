---
title: "SQL-Datenbankanwendungsentwicklung – Übersicht | Microsoft-Dokumentation"
description: "Hier erfahren Sie mehr über verfügbare Verbindungsbibliotheken und bewährte Methoden für Anwendungen, die eine Verbindung mit SQL-Datenbank herstellen."
services: sql-database
documentationcenter: 
author: stevestein
manager: jhubbard
editor: genemi
ms.assetid: 67c02204-d1bd-4622-acce-92115a7cde03
ms.service: sql-database
ms.custom: development
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 08/17/2016
ms.author: sstein
translationtype: Human Translation
ms.sourcegitcommit: e5b5751facb68ae4a62e3071fe4dfefc02434a9f
ms.openlocfilehash: 18dc3cce7451d90b6b65b990b80c05e7f6decb56


---
# <a name="sql-database-application-development-overview"></a>SQL-Datenbankanwendungsentwicklung – Übersicht
In diesem Artikel werden die grundlegenden Aspekte beschrieben, die ein Entwickler beim Schreiben von Code zum Herstellen einer Verbindung mit Azure SQL-Datenbanken berücksichtigen sollte.

> [!TIP]
> Das [Tutorial zu den ersten Schritten](sql-database-get-started.md) veranschaulicht, wie Sie einen Server und eine serverbasierte Firewall erstellen, Servereigenschaften anzeigen, eine Verbindung mithilfe von SQL Server Management Studio herstellen, die Masterdatenbank abfragen, eine Beispieldatenbank und eine leere Datenbank erstellen, Datenbankeigenschaften abfragen, und die Beispieldatenbank abfragen.
>

## <a name="language-and-platform"></a>Sprache und Plattform
Für eine Vielzahl von Programmiersprachen und Plattformen sind Codebeispiele verfügbar. Links zu den Codebeispielen finden Sie unter: 

* Weitere Informationen: [Verbindungsbibliotheken für SQL-Datenbank und SQL Server](sql-database-libraries.md)

## <a name="resource-limitations"></a>Ressourcenbeschränkungen
Azure SQL-Datenbank verwaltet die für eine Datenbank verfügbaren Ressourcen mithilfe zweier verschiedener Mechanismen: Ressourcenkontrolle und Durchsetzung von Grenzen.

* Weitere Informationen: [Ressourceneinschränkungen für Azure SQL-Datenbank](sql-database-resource-limits.md)

## <a name="security"></a>Sicherheit
Azure SQL-Datenbank stellt Ressourcen zum Einschränken des Zugriffs, zum Schützen von Daten sowie zum Überwachen von Aktivitäten in einer SQL-Datenbank bereit.

* Weitere Informationen: [Sichern der SQL-Datenbank](sql-database-security-overview.md)

## <a name="authentication"></a>Authentifizierung
* Azure SQL-Datenbank unterstützt Benutzer und Anmeldungen mit SQL Server-Authentifizierung sowie Benutzer und Anmeldungen mit [Azure Active Directory-Authentifizierung](sql-database-aad-authentication.md) .
* Sie müssen eine bestimmte Datenbank angeben, anstatt automatisch die *master* -Datenbank zu verwenden.
* Sie können die Transact-SQL-Anweisung **USE myDatabaseName;** in SQL-Datenbanken nicht verwenden, um zu einer anderen Datenbank zu wechseln.
* Weitere Informationen: [Sicherheit von SQL-Datenbank: Verwalten von Datenbankzugriff und Anmeldesicherheit](sql-database-manage-logins.md)

## <a name="resiliency"></a>Resilienz
Tritt beim Verbinden mit SQL-Datenbank ein vorübergehender Fehler auf, muss der Code den Aufruf wiederholen.  Die Wiederholungslogik sollte Backofflogik verwenden, damit die SQL-Datenbank nicht unnötig überlastet wird, wenn mehrere Clients die Wiederholung gleichzeitig durchführen.

* Codebeispiele: Codebeispiele zum Veranschaulichen der Wiederholungslogik finden Sie in den Beispielen für die Sprache Ihrer Wahl unter [Verbindungsbibliotheken für SQL-Datenbank und SQL Server](sql-database-libraries.md).
* Weitere Informationen: [Fehlermeldungen für Clientprogramme der SQL-Datenbank](sql-database-develop-error-messages.md)

## <a name="managing-connections"></a>Verwalten von Verbindungen
* Ändern Sie in der Clientverbindungslogik das Standardtimeout in 30 Sekunden.  Der Standardwert von 15 Sekunden ist zu kurz für Verbindungen, die über das Internet hergestellt werden.
* Wenn Sie einen [Verbindungspool](http://msdn.microsoft.com/library/8xx3tyca.aspx)verwenden, trennen Sie unbedingt die Verbindung, sobald Ihre Anwendung sie nicht aktiv verwendet und nicht gerade auf die erneute Verwendung der Verbindung vorbereitet wird.

## <a name="network-considerations"></a>Netzwerküberlegungen
* Vergewissern Sie sich, dass auf dem Computer, der das Clientprogramm hostet, die Firewall ausgehende TCP-Kommunikation über Port 1433 zulässt.  Weitere Informationen: [Konfigurieren einer Firewall für die Azure SQL-Datenbank mit dem Azure-Portal](sql-database-configure-firewall-settings.md)
* Wenn Ihr Clientprogramm eine Verbindung mit SQL-Datenbank V12 herstellt, wobei der Client auf einem virtuellen Azure-Computer ausgeführt wird, müssen Sie bestimmte Portbereiche auf dem virtuellen Computer öffnen. Weitere Informationen: [Andere Ports als 1433 für ADO.NET 4.5 und SQL-Datenbank V12](sql-database-develop-direct-route-ports-adonet-v12.md)
* Bei Clientverbindungen mit Azure SQL-Datenbank V12 wird der Proxy manchmal umgangen und direkt mit der Datenbank interagiert. Andere Ports als 1433 werden wichtig. Weitere Informationen: [Andere Ports als 1433 für ADO.NET 4.5 und SQL-Datenbank V12](sql-database-develop-direct-route-ports-adonet-v12.md)

## <a name="data-sharding-with-elastic-scale"></a>Datensharding mit elastischer Skalierung
Die elastische Skalierung vereinfacht das horizontale Hoch- und Herunterskalieren. 

* [Entwurfsmuster für mehrinstanzenfähige SaaS-Anwendungen und Azure SQL-Datenbank](sql-database-design-patterns-multi-tenancy-saas-applications.md)
* [Datenabhängiges Routing](sql-database-elastic-scale-data-dependent-routing.md)
* [Erste Schritte mit der Vorschauversion von Elastic Scale für Azure SQL-Datenbank](sql-database-elastic-scale-get-started.md)

## <a name="next-steps"></a>Nächste Schritte
Entdecken Sie alle [Funktionen von SQL-Datenbank](https://azure.microsoft.com/services/sql-database/).




<!--HONumber=Dec16_HO4-->


