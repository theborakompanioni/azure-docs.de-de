---
title: Was ist eine Azure SQL-Datenbank? | Microsoft Docs
description: "Dieser Artikel enthält eine Übersicht über Azure SQL-Datenbanken."
services: sql-database
documentationcenter: na
author: CarlRabeler
manager: jhubbard
editor: 
ms.assetid: 
ms.service: sql-database
ms.custom: single databases
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-management
ms.date: 11/28/2016
ms.author: carlrab
translationtype: Human Translation
ms.sourcegitcommit: 757d6f778774e4439f2c290ef78cbffd2c5cf35e
ms.openlocfilehash: b986fc881a39ca3a28d6f5f968cefe19c05b5dc5
ms.lasthandoff: 04/10/2017


---
# <a name="azure-sql-database-overview"></a>Übersicht zu Azure SQL-Datenbank
Dieses Thema enthält eine Übersicht über Azure SQL-Datenbanken. Weitere Informationen zu logischen Azure SQL-Servern finden Sie unter [Logical Servers (Logische Server)](sql-database-server-overview.md).

## <a name="what-is-azure-sql-database"></a>Was ist Azure SQL-Datenbank?
Jede Datenbank in Azure SQL-Datenbank ist einem logischen Server zugeordnet. Die Datenbank kann Folgendes sein:

- Eine einzelne Datenbank mit ihrem [eigenen Ressourcensatz](sql-database-what-is-a-dtu.md#what-are-database-transaction-units-dtus) (DTUs)
- Teil eines [elastischen Pools](sql-database-elastic-pool.md) mit [gemeinsamer Nutzung eines Ressourcensatzes](sql-database-what-is-a-dtu.md#what-are-elastic-database-transaction-units-edtus) (eDTUs)
- Teil eines [skalierten Satzes von Sharddatenbanken](sql-database-elastic-scale-introduction.md#horizontal-and-vertical-scaling), die entweder einfache oder in einem Pool zusammengefasste Datenbanken sein können
- Teil von Datenbanksätzen, die an einem [mehrinstanzfähigen SaaS-Entwurfsmuster](sql-database-design-patterns-multi-tenancy-saas-applications.md) teilnehmen, und deren Datenbanken entweder einzelne oder in einem Pool zusammengefasste Datenbanken (oder beides) sein können 

## <a name="how-do-i-connect-and-authenticate-to-an-azure-sql-database"></a>Wie verbinde und authentifiziere ich mich mit einem logischen Azure SQL-Datenbankserver?

- **Authentifizierung und Autorisierung**: Azure SQL-Datenbank unterstützt SQL-Authentifizierung und die Authentifizierung über Azure Active Directory (unter bestimmten Einschränkungen – siehe [Herstellen einer Verbindung mit SQL-Datenbank oder SQL Data Warehouse unter Verwendung der Azure Active Directory-Authentifizierung](sql-database-aad-authentication.md) für die Authentifizierung. Sie können sich mit Azure SQL-Datenbanken über die Server-Masterdatenbank oder direkt mit einer Benutzerdatenbank verbinden und authentifizieren. Weitere Informationen finden Sie unter [SQL-Datenbank-Authentifizierung und -Autorisierung: Gewähren von Zugriff](sql-database-manage-logins.md). Windows-Authentifizierung wird nicht unterstützt. 
- **TDS**: Microsoft Azure SQL-Datenbank unterstützt den TDS-Client (Tabular Data Stream) ab Version 7.3.
- **TCP/IP**: Nur TCP/IP-Verbindungen sind zulässig.
- **Firewall der SQL-Datenbank**: Zum Schutz Ihrer Daten verhindert eine Firewall der SQL-Datenbank jeglichen Zugriff auf Ihren Datenbankserver oder dessen Datenbanken, bis Sie angeben, welche Computer zugriffsberechtigt sind. Weitere Informationen hierzu finden Sie unter [Firewalls](sql-database-firewall-configure.md).

## <a name="what-collations-are-supported"></a>Welche Sortierungen werden unterstützt?
Die standardmäßige Datenbanksortierung von Microsoft Azure SQL-Datenbank ist **SQL_LATIN1_GENERAL_CP1_CI_AS**, wobei **LATIN1_GENERAL** für Englisch (USA), **CP1** für Codepage 1252, **CI** für keine Unterscheidung von Groß-/Kleinschreibung und **AS** für die Unterscheidung nach Akzent steht. Weitere Informationen zum Festlegen der Sortierung finden Sie unter [COLLATE (Transact-SQL)](https://msdn.microsoft.com/library/ms184391.aspx).

## <a name="what-are-the-naming-requirements-for-database-objects"></a>Was sind die Benennungsanforderungen für Datenbankobjekte?

Namen für alle neuen Objekte müssen den SQL Server-Regeln für Bezeichner entsprechen. Weitere Informationen finden Sie unter [Bezeichner](https://msdn.microsoft.com/library/ms175874.aspx).

## <a name="what-features-are-supported-by-azure-sql-databases"></a>Welche Funktionen werden von Azure SQL-Datenbanken unterstützt?

Informationen zu unterstützten Funktionen finden Sie unter [Features (Funktionen)](sql-database-features.md). Unter [Azure SQL-Datenbank – Abweichungen bei Transact-SQL](sql-database-transact-sql-information.md) finden Sie weitere Hintergrundinformationen zu den Gründen für die fehlende Unterstützung bestimmter Funktionstypen.

## <a name="how-do-i-manage-an-azure-sql-database"></a>Wie verwalte ich eine Azure SQL-Datenbank?

Sie können logische Azure SQL-Datenbankserver mit verschiedenen Methoden verwalten:
- [Azure-Portal](sql-database-manage-portal.md)
- [PowerShell](sql-database-manage-powershell.md)
- [Transact-SQL](sql-database-manage-azure-ssms.md)
- [REST](/rest/api/sql/)

## <a name="next-steps"></a>Nächste Schritte

- Informationen über den Azure SQL-Datenbank-Dienst finden Sie unter [Was ist SQL Database? Einführung in SQL-Datenbank](sql-database-technical-overview.md).
- Informationen zu unterstützten Funktionen finden Sie unter [Features (Funktionen)](sql-database-features.md).
- Eine Übersicht über logische Azure SQL-Server finden Sie unter [SQL Database logical server overview (Übersicht über logische SQL-Datenbankserver)](sql-database-server-overview.md).
- Informationen zur Unterstützung von Transact-SQL und Unterschieden finden Sie unter [Azure SQL-Datenbank – Abweichungen bei Transact-SQL](sql-database-transact-sql-information.md).
- Informationen zu bestimmten Ressourcenkontingenten und Einschränkungen basierend auf Ihrer **Dienstebene**. Eine Übersicht über die Dienstebenen finden Sie unter [SQL-Datenbanken-Dienstebenen](sql-database-service-tiers.md).
- Eine Übersicht über Sicherheit finden Sie unter [Azure SQL Database Security Overview](sql-database-security-overview.md) (Übersicht über die Sicherheit von Azure SQL-Datenbank).
- Informationen zur Verfügbarkeit von Treibern und Unterstützung für SQL-Datenbank finden Sie unter [Verbindungsbibliotheken für SQL-Datenbank und SQL Server](sql-database-libraries.md).


