---
title: "Übersicht über Funktionen der Azure SQL-Datenbank | Microsoft Docs"
description: "Diese Seite bietet eine Übersicht über die logischen Azure SQL-Datenbankserver und Datenbanken und umfasst eine Funktionsunterstützungsmatrix mit Links über die einzelnen aufgeführten Funktionen."
services: sql-database
documentationcenter: na
author: CarlRabeler
manager: jhubbard
editor: 
ms.assetid: d1a46fa4-53d2-4d25-a0a7-92e8f9d70828
ms.service: sql-database
ms.custom: overview
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: data-management
ms.date: 03/03/2017
ms.author: carlrab; jognanay
translationtype: Human Translation
ms.sourcegitcommit: 2f03ba60d81e97c7da9a9fe61ecd419096248763
ms.openlocfilehash: 3f8ad9bf72555b9ab19af4c7a15f66b982fe66fe
ms.lasthandoff: 03/04/2017


---
# <a name="azure-sql-database-features"></a>Funktionen von Azure SQL-Datenbank
Dieses Thema bietet eine Übersicht über die logischen Azure SQL-Datenbankserver und Datenbanken und enthält eine Funktionssupportmatrix mit Links über die einzelnen aufgeführten Funktionen. 

## <a name="what-is-an-azure-sql-database-logical-server"></a>Was ist ein logischer Azure SQL-Datenbankserver?
Ein logischer Azure SQL-Datenbankserver fungiert als zentraler Verwaltungspunkt für mehrere Datenbanken. In SQL-Datenbank ist ein Server ein logisches Konstrukt, das von einer SQL Server-Instanz unterschieden wird, mit der Sie möglicherweise in der lokalen Welt vertraut sind. Insbesondere der SQL-Datenbankdienst gibt keine Garantie bezüglich des Speicherorts der Datenbanken in Bezug auf ihre logischen Server, und bietet kein Zugriff oder Funktion auf Instanzebene. Weitere Informationen zu logischen Azure SQL-Servern, finden Sie unter [Logical Servers (Logische Server)](sql-database-server-overview.md). 

## <a name="what-is-an-azure-sql-database"></a>Was ist eine Azure SQL-Datenbank?
Jede Datenbank in Azure SQL-Datenbank ist einem logischen Server zugeordnet. Die Datenbank kann Folgendes sein:

- Eine einzelne Datenbank mit ihrem [eigenen Ressourcensatz](sql-database-what-is-a-dtu.md#what-are-database-transaction-units-dtus) (DTUs)
- Teil eines [Pools von Datenbanken](sql-database-elastic-pool.md), der [einen Ressourcensatz teilt](sql-database-what-is-a-dtu.md#what-are-elastic-database-transaction-units-edtus) (eDTUs)
- Teil eines [skalierten Satzes von Sharddatenbanken](sql-database-elastic-scale-introduction.md#horizontal-and-vertical-scaling), die entweder einfache oder in einem Pool zusammengefasste Datenbanken sein können
- Teil von Datenbanksätzen, die an einem [mehrinstanzfähigen SaaS-Entwurfsmuster](sql-database-design-patterns-multi-tenancy-saas-applications.md) teilnehmen, und deren Datenbanken entweder einzelne oder in einem Pool zusammengefasste Datenbanken (oder beides) sein können 

Weitere Informationen zu Azure SQL-Datenbanken, finden Sie unter [SQL databases (SQL-Datenbanken)](sql-database-overview.md).

## <a name="what-features-are-supported"></a>Welche Funktionen werden unterstützt?

In der folgenden Tabelle sind die wichtigsten Funktionen von Azure SQL-Datenbank und SQL Server aufgeführt, gibt deren Unterstützungsmöglichkeiten und stellt einen Link zu weiteren Informationen über die Funktion auf jeder Plattform bereit. Für Funktionen von Transact-SQL folgen Sie dem Link in der Tabelle zu der Kategorie der Funktion. Siehe auch [Azure SQL-Datenbank – Abweichungen bei Transact-SQL](sql-database-transact-sql-information.md) für weitere Hintergrundinformationen zu den Gründen über fehlende Unterstützung von bestimmten Funktionsarten.

Es werden auch weiterhin neue Funktionen zu V12 hinzugefügt. Daher empfehlen wir Ihnen, die Webseite "Dienstupdates" für Azure zu besuchen und diese Filter zu verwenden:

* Gefiltert nach [SQL-Datenbank](https://azure.microsoft.com/updates/?service=sql-database).
* Gefiltert nach allgemeiner Verfügbarkeit [Ankündigungen](http://azure.microsoft.com/updates/?service=sql-database&update-type=general-availability) für SQL-Datenbankfunktionen.

> [!TIP]
> Informationen zum Testen einer vorhandenen Datenbank auf Kompatibilität mit Azure SQL-Datenbank finden Sie unter [Migrieren einer SQL Server-Datenbank zu Azure](sql-database-cloud-migrate.md).
>

| **Feature** | **SQL Server** | **Azure SQL-Datenbank** | 
| --- | :---: | :---: | 
| Aktive Georeplikation | Nicht unterstützt – siehe [Always On-Verfügbarkeitsgruppen](https://msdn.microsoft.com/library/hh510230.aspx) | [Unterstützt](sql-database-geo-replication-overview.md)
| Always Encrypted | [Unterstützt](https://msdn.microsoft.com/library/mt163865.aspx) | [Unterstützt](sql-database-always-encrypted.md) |
| AlwaysOn-Verfügbarkeitsgruppen | [Unterstützt](https://msdn.microsoft.com/library/hh510230.aspx) | Nicht unterstützt – siehe [aktive Georeplikation](sql-database-geo-replication-overview.md) |
| Anfügen einer Datenbank | [Unterstützt](https://msdn.microsoft.com/library/ms190209.aspx) | Nicht unterstützt |
| Anwendungsrollen | [Unterstützt](https://msdn.microsoft.com/library/ms190998.aspx) | [Unterstützt](https://msdn.microsoft.com/library/ms190998.aspx) |
| Automatische Skalierung | Nicht unterstützt | [Unterstützt](sql-database-service-tiers.md) |
| Azure Active Directory | Nicht unterstützt | [Unterstützt](sql-database-aad-authentication.md) |
| Azure Data Factory | Nicht unterstützt – [SQL Server Integration Services (SSIS)](https://msdn.microsoft.com/library/ms141026.aspx) | [Unterstützt](https://azure.microsoft.com/services/data-factory/) |
| Überwachung | [Unterstützt](https://msdn.microsoft.com/library/cc280386.aspx) | [Unterstützt](sql-database-auditing.md) |
| BACPAC Datei (exportieren) | [Unterstützt](https://msdn.microsoft.com/library/hh213241.aspx) | [Unterstützt](sql-database-export.md) |
| BACPAC Datei (importieren) | [Unterstützt](https://msdn.microsoft.com/library/hh710052.aspx) | [Unterstützt](sql-database-import-portal.md) |
| BACKUP- und RESTORE-Anweisungen | [Unterstützt](https://msdn.microsoft.com/library/ff848768.aspx) | Nicht unterstützt |
| Integrierte Funktionen | [Unterstützt](https://msdn.microsoft.com/library/ms174318.aspx) | [Die meisten](https://msdn.microsoft.com/library/ms174318.aspx) |
| Erfassung geänderter Daten | [Unterstützt](https://msdn.microsoft.com/library/cc645937.aspx) | Nicht unterstützt |
| Änderungsnachverfolgung | [Unterstützt](https://msdn.microsoft.com/library/bb933875.aspx) | [Unterstützt](https://msdn.microsoft.com/library/bb933875.aspx) |
| Sortierung-Anweisungen | [Unterstützt](https://msdn.microsoft.com/library/ff848763.aspx) | [Unterstützt](https://msdn.microsoft.com/library/ff848763.aspx) |
| ColumnStore-Indizes | [Unterstützt](https://msdn.microsoft.com/library/gg492088.aspx) | [Nur Premium Edition](https://msdn.microsoft.com/library/gg492088.aspx) |
| Common Language Runtime (CLR) | [Unterstützt](https://msdn.microsoft.com/library/ms131102.aspx) | Nicht unterstützt |
| Eigenständige Datenbanken | [Unterstützt](https://msdn.microsoft.com/library/ff929071.aspx) | Integriert |
| Enthaltene Benutzer | [Unterstützt](https://msdn.microsoft.com/library/ff929188.aspx) | [Unterstützt](sql-database-manage-logins.md#non-administrator-users) |
| Schlüsselwörter der Sprache zur Ablaufsteuerung | [Unterstützt](https://msdn.microsoft.com/library/ms174290.aspx) | [Unterstützt](https://msdn.microsoft.com/library/ms174290.aspx) |
| Datenbankübergreifende Abfragen | [Unterstützt](https://msdn.microsoft.com/library/dn584627.aspx) | [Elastische Abfragen](sql-database-elastic-query-overview.md) |
| Cursor | [Unterstützt](https://msdn.microsoft.com/library/ms181441.aspx) | [Unterstützt](https://msdn.microsoft.com/library/ms181441.aspx) | 
| Datenkomprimierung | [Unterstützt](https://msdn.microsoft.com/library/cc280449.aspx) | [Unterstützt](https://msdn.microsoft.com/library/cc280449.aspx) |
| Datenbanksicherungen | [Für Benutzer verfügbar](https://msdn.microsoft.com/library/ms187048.aspx) | [Integriert](sql-database-automated-backups.md) |
| Datenbank-E-Mail | [Unterstützt](https://msdn.microsoft.com/library/ms189635.aspx) | Nicht unterstützt |
| Datenbankspiegelung | [Unterstützt](https://msdn.microsoft.com/library/ms189852.aspx) | Nicht unterstützt |
| Datenbank-Konfigurationsoptionen | [Unterstützt](https://msdn.microsoft.com/library/mt629158.aspx) | [Unterstützt](https://msdn.microsoft.com/library/mt629158.aspx) |
| Data Quality Services (DQS) | [Unterstützt](https://msdn.microsoft.com/library/ff877925.aspx) | Nicht unterstützt |
| Datenbank-Momentaufnahmen | [Unterstützt](https://msdn.microsoft.com/library/ms175158.aspx) | Nicht unterstützt |
| Datentypen | [Unterstützt](https://msdn.microsoft.com/library/ms187752.aspx) | [Unterstützt](https://msdn.microsoft.com/library/ms187752.aspx) |  
| DBCC-Anweisungen | [Alle](https://msdn.microsoft.com/library/ms188796.aspx) | [Einige](https://msdn.microsoft.com/library/ms188796.aspx) |
| DDL-Anweisungen | [Unterstützt](https://msdn.microsoft.com/library/ff848799.aspx) | [Die meisten](https://msdn.microsoft.com/library/ff848799.aspx)
| DDL-Trigger | [Unterstützt](https://msdn.microsoft.com/library/ms175941.aspx) | [Nur Datenbank](https://msdn.microsoft.com/library/ms175941.aspx) |
| Verteilte Transaktionen | [MS DTC](https://msdn.microsoft.com/library/ms131665.aspx) | Nur begrenzte interne SQL-Datenbank-Szenarios |
| DML-Anweisungen | [Unterstützt](https://msdn.microsoft.com/library/ff848766.aspx) | [Die meisten](https://msdn.microsoft.com/library/ff848766.aspx) |
| DML-Trigger | [Unterstützt](https://msdn.microsoft.com/library/ms178110.aspx) | [Unterstützt](https://msdn.microsoft.com/library/ms178110.aspx) |
| DMVs | [Alle](https://msdn.microsoft.com/library/ms188754.aspx) | [Einige](https://msdn.microsoft.com/library/ms188754.aspx) |
| Elastische Pools | Nicht unterstützt | [Unterstützt](sql-database-elastic-pool.md) |
| Elastische Aufträge | Nicht unterstützt – siehe [SQL Server-Agent](https://msdn.microsoft.com/library/ms189237.aspx) | [Unterstützt](sql-database-elastic-jobs-getting-started.md) | 
| Elastische Abfragen | Nicht unterstützt – siehe [Datenbankübergreifende Abfragen](https://msdn.microsoft.com/library/dn584627.aspx) | [Unterstützt](sql-database-elastic-query-overview.md) |
| Ereignisbenachrichtigungen | [Unterstützt](https://msdn.microsoft.com/library/ms186376.aspx) | [Unterstützt](sql-database-insights-alerts-portal.md) |
| Ausdrücke | [Unterstützt](https://msdn.microsoft.com/library/ms190286.aspx) | [Unterstützt](https://msdn.microsoft.com/library/ms190286.aspx) |
| Erweiterte Ereignisse | [Unterstützt](https://msdn.microsoft.com/library/bb630282.aspx) | [Einige](sql-database-xevent-db-diff-from-svr.md) |
| Erweiterte gespeicherte Prozeduren | [Unterstützt](https://msdn.microsoft.com/library/ms164627.aspx) | Nicht unterstützt |
| Dateigruppen | [Unterstützt](https://msdn.microsoft.com/library/ms189563.aspx#Anchor_2) | [Nur primär](https://msdn.microsoft.com/library/ms189563.aspx#Anchor_2) |
| Filestream | [Unterstützt](https://msdn.microsoft.com/library/gg471497.aspx) | Nicht unterstützt |
| Volltextsuche | [Unterstützt](https://msdn.microsoft.com/library/ms142571.aspx) | [Nicht unterstützte Worttrennungen von Drittanbietern](https://msdn.microsoft.com/library/ms142571.aspx) |
| Functions | [Unterstützt](https://msdn.microsoft.com/library/ms174318.aspx) | [Die meisten](https://msdn.microsoft.com/library/ms174318.aspx) |
| In-Memory-Optimierung | [Unterstützt](https://msdn.microsoft.com/library/dn133186.aspx) | [Nur Premium Edition](https://msdn.microsoft.com/library/dn133186.aspx) |
| Aufträge | [SQL Server-Agent](https://msdn.microsoft.com/library/ms189237.aspx) | [Unterstützt](sql-database-elastic-jobs-getting-started.md) |
| Unterstützung von JSON-Daten | [Unterstützt](https://msdn.microsoft.com/library/dn921897.aspx) | [Unterstützt](sql-database-json-features.md) |
| Sprachelemente | [Unterstützt](https://msdn.microsoft.com/library/ff848807.aspx) | [Die meisten](https://msdn.microsoft.com/library/ff848807.aspx) |  
| Verknüpfte Server | [Unterstützt](https://msdn.microsoft.com/library/ms188279.aspx) | Nicht unterstützt – siehe [Elastic query (Elastische Abfrage)](sql-database-elastic-query-horizontal-partitioning.md) |
| Protokollversand | [Unterstützt](https://msdn.microsoft.com/library/ms187103.aspx) | Nicht unterstützt – siehe [aktive Georeplikation](sql-database-geo-replication-overview.md) |
| Befehle für Verwaltung | [Unterstützt](https://msdn.microsoft.com/library/ms190286.aspx)| [Nicht unterstützt](https://msdn.microsoft.com/library/ms190286.aspx) |
| Master Data Services (MDS) | [Unterstützt](https://msdn.microsoft.com/library/ff487003.aspx) | Nicht unterstützt |
| Minimale Protokollierung bei Massenimport | [Unterstützt](https://msdn.microsoft.com/library/ms190422.aspx) | Nicht unterstützt |
| Ändern von Systemdaten | [Unterstützt](https://msdn.microsoft.com/library/ms178028.aspx) | Nicht unterstützt |
| Online-Indexvorgänge | [Unterstützt](https://msdn.microsoft.com/library/ms177442.aspx) | [Transaktionsgröße durch Dienstebene eingeschränkt](https://msdn.microsoft.com/library/ms177442.aspx) |
| Operatoren | [Unterstützt](https://msdn.microsoft.com/library/ms174986.aspx) | [Die meisten](https://msdn.microsoft.com/library/ms174986.aspx) |
| Point-in-Time-Wiederherstellung einer Datenbank | [Unterstützt](https://msdn.microsoft.com/library/ms179451.aspx) | [Unterstützt](sql-database-recovery-using-backups.md#point-in-time-restore) |
| PolyBase | [Unterstützt](https://msdn.microsoft.com/library/mt143171.aspx) | [Nicht unterstützt]
| Richtlinienbasierte Verwaltung | [Unterstützt](https://msdn.microsoft.com/library/bb510667.aspx) | Nicht unterstützt |
| Prädikate | [Unterstützt](https://msdn.microsoft.com/library/ms189523.aspx) | [Die meisten](https://msdn.microsoft.com/library/ms189523.aspx)
| R Services | [Unterstützt](https://msdn.microsoft.com/library/mt604845.aspx)
| Ressourcenkontrolle | [Unterstützt](https://msdn.microsoft.com/library/bb933866.aspx) | Nicht unterstützt |
| Wiederherstellen einer Datenbank aus der Sicherung | [Unterstützt](https://msdn.microsoft.com/library/ms187048.aspx#anchor_6) | [Nur von integrierten Sicherungen](sql-database-recovery-using-backups.md) |
| Sicherheit auf Zeilenebene | [Unterstützt](https://msdn.microsoft.com/library/dn765131.aspx) | [Unterstützt](https://msdn.microsoft.com/library/dn765131.aspx) |
| Sicherheitsanweisungen | [Unterstützt](https://msdn.microsoft.com/library/ff848791.aspx) | [Einige](https://msdn.microsoft.com/library/ff848791.aspx) |
| Semantische Suche | [Unterstützt](https://msdn.microsoft.com/library/gg492075.aspx) | Nicht unterstützt |
| Sequenznummern | [Unterstützt](https://msdn.microsoft.com/library/ff878058.aspx) | [Unterstützt](https://msdn.microsoft.com/library/ff878058.aspx) |
| Service Broker | [Unterstützt](https://msdn.microsoft.com/library/bb522893.aspx) | Nicht unterstützt |
| Server-Konfigurationsoptionen | [Unterstützt](https://msdn.microsoft.com/library/ms189631.aspx) | Nicht unterstützt – siehe [Database configuration options (Datenbank-Konfigurationsoptionen)](https://msdn.microsoft.com/library/mt629158.aspx) |
| SET-Anweisungen | [Unterstützt](https://msdn.microsoft.com/library/ms190356.aspx) | [Die meisten](https://msdn.microsoft.com/library/ms190356.aspx) 
| Spatial | [Unterstützt](https://msdn.microsoft.com/library/bb933790.aspx) | [Unterstützt](https://msdn.microsoft.com/library/bb933790.aspx) |
| SQL Server-Agent | [Unterstützt](https://msdn.microsoft.com/library/ms189237.aspx) | Nicht unterstützt – siehe [elastische Einzelvorgänge](sql-database-elastic-jobs-getting-started.md) |
| SQL Server Analysis Services (SSAS) | [Unterstützt](https://msdn.microsoft.com/library/bb522607.aspx) | Nicht unterstützt – siehe [Azure Analysis Services](https://azure.microsoft.com/services/analysis-services/) |
| SQL Server Integration Services (SSIS) | [Unterstützt](https://msdn.microsoft.com/library/ms141026.aspx) | Nicht unterstützt – siehe [Azure Data Factory](https://azure.microsoft.com/services/data-factory/) |
| SQL Server PowerShell | [Unterstützt](https://msdn.microsoft.com/library/hh245198.aspx) | [Unterstützt](https://msdn.microsoft.com/library/hh245198.aspx) |
| SQL Server Profiler | [Unterstützt](https://msdn.microsoft.com/library/ms181091.aspx) | Nicht unterstützt – siehe [Erweiterte Ereignisse](https://msdn.microsoft.com/library/ms181091.aspx) |
| SQL Server-Replikation | [Unterstützt](https://msdn.microsoft.com/library/ms151198.aspx) | [Nur für Transaktions- und Momentaufnahmenreplikationsabonnent](sql-database-cloud-migrate.md) |
| SQL Server Reporting Services (SSRS) | [Unterstützt](https://msdn.microsoft.com/library/ms159106.aspx) | Nicht unterstützt |
| Gespeicherte Prozeduren | [Unterstützt](https://msdn.microsoft.com/library/ms190782.aspx) | [Unterstützt](https://msdn.microsoft.com/library/ms190782.aspx) |
| Gespeicherte Systemfunktionen | [Unterstützt](https://msdn.microsoft.com/library/ff848780.aspx) | [Einige](https://msdn.microsoft.com/library/ff848780.aspx) |
| Gespeicherte Systemprozeduren | [Unterstützt](https://msdn.microsoft.com/library/ms187961.aspx) | [Einige](https://msdn.microsoft.com/library/ms187961.aspx) |
| Systemtabellen | [Unterstützt](https://msdn.microsoft.com/library/ms179932.aspx) | [Einige](https://msdn.microsoft.com/library/ms179932.aspx) |
| Systemsichten | [Unterstützt](https://msdn.microsoft.com/library/ms177862.aspx) | [Einige](https://msdn.microsoft.com/library/ms177862.aspx)
| Tabellenpartitionierung | [Unterstützt](https://msdn.microsoft.com/library/ms190787.aspx) | [Nur primäre Dateigruppe](https://msdn.microsoft.com/library/ms190787.aspx) |
| Temporäre Tabellen | [Lokal und global](https://msdn.microsoft.com/library/ms174979.aspx#Anchor_4) | [Nur lokal](https://msdn.microsoft.com/library/ms174979.aspx#Anchor_4) |
| Temporäre Tabellen | [Unterstützt](https://msdn.microsoft.com/library/dn935015.aspx) | [Unterstützt](sql-database-temporal-tables.md) |
| Transaktionsanweisungen | [Unterstützt](https://msdn.microsoft.com/library/ms174377.aspx) | [Unterstützt](https://msdn.microsoft.com/library/ms174377.aspx) |
| Variablen | [Unterstützt](https://msdn.microsoft.com/library/ff848809.aspx) | | [Unterstützt](https://msdn.microsoft.com/library/ff848809.aspx) | 
| Transparent Data Encryption (TDE)  | [Unterstützt](https://msdn.microsoft.com/library/bb934049.aspx) | [Unterstützt](https://msdn.microsoft.com/dn948096.aspx) |
| Windows Server-Failoverclustering | [Unterstützt](https://msdn.microsoft.com/library/hh270278.aspx) | Nicht unterstützt – siehe [aktive Georeplikation](sql-database-geo-replication-overview.md) |
| XML-Indizes | [Unterstützt](http://msdn.microsoft.com/library/bb934097.aspx) | [Unterstützt](http://msdn.microsoft.com/library/bb934097.aspx) |
| XML-Anweisungen | [Unterstützt](https://msdn.microsoft.com/library/ff848798.aspx) | [Unterstützt](https://msdn.microsoft.com/library/ff848798.aspx) |

## <a name="next-steps"></a>Nächste Schritte

- Informationen über den Dienst für Azure SQL-Datenbank finden Sie unter [Was ist SQL Database? Einführung in SQL-Datenbank](sql-database-technical-overview.md)
- Eine Übersicht über logische Azure SQL-Server finden Sie unter [SQL Database logical server overview (Übersicht über logische SQL-Datenbankserver)](sql-database-server-overview.md)
- Unter [Übersicht zu Azure SQL-Datenbank](sql-database-overview.md) finden Sie eine Übersicht über Azure SQL-Datenbanken.
- Informationen zur Unterstützung von Transact-SQL und Unterschiede finden Sie unter [Azure SQL-Datenbank – Abweichungen bei Transact-SQL](sql-database-transact-sql-information.md).
- Informationen zu bestimmten Ressourcenkontingenten und Einschränkungen basierend auf Ihrer **Dienstebene**. Eine Übersicht über die Dienstebenen finden Sie unter [SQL-Datenbanken-Dienstebenen](sql-database-service-tiers.md).
- Eine Übersicht über Sicherheit finden Sie unter [Azure SQL Database Security Overview](sql-database-security-overview.md) (Übersicht über die Sicherheit von Azure SQL-Datenbank).
- Informationen zur Verfügbarkeit von Treibern und Unterstützung für SQL-Datenbank finden Sie unter [Verbindungsbibliotheken für SQL-Datenbank und SQL Server](sql-database-libraries.md).

