---
title: "Übersicht über die logischen Server von Azure SQL-Datenbank | Microsoft Docs"
description: "Diese Seite enthält Aspekte und Richtlinien für die Arbeit mit logischen Azure SQL-Servern."
services: sql-database
documentationcenter: na
author: CarlRabeler
manager: jhubbard
editor: 
ms.assetid: 
ms.service: sql-database
ms.custom: DBs and servers
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-management
ms.date: 02/01/2017
ms.author: carlrab
translationtype: Human Translation
ms.sourcegitcommit: e851a3e1b0598345dc8bfdd4341eb1dfb9f6fb5d
ms.openlocfilehash: 430af168e1bd19c15170996247bc7c7a62fe52d4
ms.lasthandoff: 04/15/2017


---
# <a name="azure-sql-database-logical-servers"></a>Logische Azure SQL-Datenbankserver

Dieses Thema enthält Aspekte und Richtlinien für die Arbeit mit logischen Azure SQL-Servern. Informationen über Azure SQL-Datenbanken, finden Sie unter [SQL databases (SQL-Datenbanken)](sql-database-overview.md).

## <a name="what-is-an-azure-sql-database-logical-server"></a>Was ist ein logischer Azure SQL-Datenbankserver?
Ein logischer Azure SQL-Datenbankserver fungiert als zentraler Verwaltungspunkt für mehrere Datenbanken. In SQL-Datenbank ist ein Server ein logisches Konstrukt, das von einer SQL Server-Instanz unterschieden wird, mit der Sie möglicherweise in der lokalen Welt vertraut sind. Insbesondere der SQL-Datenbankdienst gibt keine Garantie bezüglich des Speicherorts der Datenbanken in Bezug auf ihre logischen Server, und bietet kein Zugriff oder Funktion auf Instanzebene.  

Ein logischer Azure Datenbankserver:

- wird in einem Azure-Abonnement erstellt, kann aber mit seinen enthaltenen Ressourcen in ein anderes Abonnement verschoben werden
- ist die übergeordnete Ressource für Datenbanken, elastische Pools und Data Warehouses
- bietet einen Namespace für Datenbanken, elastische Pools und Data Warehouses
- ist ein logischer Container mit Abhängigkeiten von hoher Lebensdauer – beim Löschen eines Servers werden die enthaltenen Datenbanken, elastischen Pools und Data Warehouses gelöscht
- beteiligt sich an der rollenbasierten Zugriffssteuerung (role-based access control, RBAC) in Azure; Datenbanken, elastische Pools innerhalb eines Servers erben dessen Zugriffsrechte
- ist ein oberes Element der Identität von Datenbanken und elastischen Pools für Zwecke der Azure-Ressourcenverwaltung (siehe das URL-Schema für Datenbanken und Pools)
- stellt in einer Region Ressourcen zusammen
- bietet eine Verbindungsendpunkt für den Datenbankzugriff (<serverName>.database.windows.net)
- bietet Zugriff auf Metadaten bezüglich der darin enthaltenen Ressourcen über DMVs (dynamic management views), indem eine Verbindung mit einer Masterdatenbank hergestellt wird 
- stellt den Bereich für Verwaltungsrichtlinien bereit, die für seine Datenbanken gelten: Anmeldungen, Firewall, Audit, Bedrohungserkennung usw. 
- ist durch ein Kontingent innerhalb des übergeordneten Abonnements eingeschränkt (sechs Server pro Abonnement – [ siehe Subscription limits (Grenzwerte für Abonnements)](../azure-subscription-service-limits.md))
- stellt den Bereich für Datenbank- und DTU-Kontingente für die darin enthaltenen Ressourcen (z.B. 45.000 DTU9) bereit
- ist der Versionsverwaltungsbereich für Funktionen, die in enthaltenen Ressourcen aktiviert wurden 
- Serverebenenprinzipal-Anmeldungen können alle Datenbanken auf einem Server verwalten
- kann Anmeldungen enthalten, die denen in SQL-Instanzen vor Ort ähnlich sind und die über Zugriff auf eine oder mehrere Datenbanken auf dem Server verfügen und denen beschränkte Administratorrechte zugewiesen werden können. Weitere Informationen finden Sie unter [Logins (Anmeldungen)](sql-database-manage-logins.md).

## <a name="how-do-i-connect-and-authenticate-to-an-azure-sql-database-logical-server"></a>Wie verbinde und authentifiziere ich mich mit einem logischen Azure SDL-Datenbankserver?

- **Authentifizierung und Autorisierung**: Azure SQL-Datenbank unterstützt SQL-Authentifizierung und die Authentifizierung über Azure Active Directory (unter bestimmten Einschränkungen – siehe [Herstellen einer Verbindung mit SQL-Datenbank oder SQL Data Warehouse unter Verwendung der Azure Active Directory-Authentifizierung](sql-database-aad-authentication.md) für die Authentifizierung. Sie können sich mit Azure SQL-Datenbanken über die Server-Masterdatenbank oder direkt mit einer Benutzerdatenbank verbinden und authentifizieren. Weitere Informationen finden Sie unter [SQL-Datenbank-Authentifizierung und -Autorisierung: Gewähren von Zugriff](sql-database-manage-logins.md). Windows-Authentifizierung wird nicht unterstützt. 
- **TDS**: Microsoft Azure SQL-Datenbank unterstützt den TDS-Client (Tabular Data Stream) ab Version 7.3.
- **TCP/IP**: Nur TCP/IP-Verbindungen sind zulässig.
- **Firewall der SQL-Datenbank**: Zum Schutz Ihrer Daten verhindert eine Firewall der SQL-Datenbank jeglichen Zugriff auf Ihren Datenbankserver oder dessen Datenbanken, bis Sie angeben, welche Computer zugriffsberechtigt sind. Weitere Informationen hierzu finden Sie unter [Firewalls](sql-database-firewall-configure.md).

## <a name="what-collations-are-supported"></a>Welche Sortierungen werden unterstützt?

Die standardmäßige Datenbanksortierung von Microsoft Azure SQL-Datenbank ist **SQL_LATIN1_GENERAL_CP1_CI_AS**, wobei **LATIN1_GENERAL** für Englisch (USA), **CP1** für Codepage 1252, **CI** für keine Unterscheidung von Groß-/Kleinschreibung und **AS** für die Unterscheidung nach Akzent steht. Es wird nicht empfohlen, die Sortierung für Datenbanken nach deren Erstellung zu ändern. Weitere Informationen über Sortierungen finden Sie unter [COLLATE (Transact-SQL)](https://msdn.microsoft.com/library/ms184391.aspx).

## <a name="what-are-the-naming-requirements-for-database-objects"></a>Was sind die Benennungsanforderungen für Datenbankobjekte?

Namen für alle neuen Objekte müssen den SQL Server-Regeln für Bezeichner entsprechen. Weitere Informationen finden Sie unter [Bezeichner](https://msdn.microsoft.com/library/ms175874.aspx).

## <a name="what-features-are-supported"></a>Welche Funktionen werden unterstützt?

Informationen zu unterstützten Funktionen finden Sie unter [Features (Funktionen)](sql-database-features.md). Siehe auch [Azure SQL-Datenbank – Abweichungen bei Transact-SQL](sql-database-transact-sql-information.md) für weitere Hintergrundinformationen zu den Gründen über fehlende Unterstützung von bestimmten Funktionsarten.

## <a name="how-do-i-manage-a-logical-server"></a>Wie verwalte ich einen logischen Server?

Sie können die logischen Azure SQL-Datenbankserver mit verschiedenen Methoden verwalten:
- [Azure-Portal](sql-database-manage-overview.md)
- [PowerShell](sql-database-manage-overview.md)
- [REST](/rest/api/sql/)

## <a name="next-steps"></a>Nächste Schritte

- Informationen über den Azure SQL-Datenbank-Dienst finden Sie unter [Was ist SQL Database? Einführung in SQL-Datenbank](sql-database-technical-overview.md).
- Informationen zu unterstützten Funktionen finden Sie unter [Features (Funktionen)](sql-database-features.md).
- Unter [Übersicht zu Azure SQL-Datenbank](sql-database-overview.md) finden Sie eine Übersicht über Azure SQL-Datenbanken.
- Informationen zur Unterstützung von Transact-SQL und Unterschiede finden Sie unter [Azure SQL-Datenbank – Abweichungen bei Transact-SQL](sql-database-transact-sql-information.md).
- Informationen zu bestimmten Ressourcenkontingenten und Einschränkungen basierend auf Ihrer **Dienstebene**. Eine Übersicht über die Dienstebenen finden Sie unter [SQL-Datenbanken-Dienstebenen](sql-database-service-tiers.md).
- Eine Übersicht über Sicherheit finden Sie unter [Azure SQL Database Security Overview](sql-database-security-overview.md) (Übersicht über die Sicherheit von Azure SQL-Datenbank).
- Informationen zur Verfügbarkeit von Treibern und Unterstützung für SQL-Datenbank finden Sie unter [Verbindungsbibliotheken für SQL-Datenbank und SQL Server](sql-database-libraries.md).


