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
ms.custom: DBs & servers
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-management
ms.date: 03/03/2017
ms.author: carlrab; jognanay
ms.translationtype: Human Translation
ms.sourcegitcommit: 95b8c100246815f72570d898b4a5555e6196a1a0
ms.openlocfilehash: 9daf8be93216aefbcf02e3fcba44d048ab95b43d
ms.contentlocale: de-de
ms.lasthandoff: 05/18/2017


---
# <a name="azure-sql-database-features"></a>Funktionen von Azure SQL-Datenbank

In den folgenden Tabellen sind die wichtigsten Features von Azure SQL-Datenbank und die entsprechenden Features von SQL Server aufgeführt. Außerdem enthalten sie Informationen dazu, ob die einzelnen Features unterstützt werden, und einen Link zu weiteren Informationen zum Feature für die jeweilige Plattform. Informationen zu Transact-SQL-Unterschieden, die beim Migrieren einer vorhandenen Datenbanklösung berücksichtigt werden sollten, finden Sie unter [Auflösen von Transact-SQL-Unterschieden während der Migration zur SQL-Datenbank](sql-database-transact-sql-information.md).

Wir fügen Azure SQL-Datenbank ständig weitere Features hinzu. Daher empfehlen wir Ihnen, die Webseite "Dienstupdates" für Azure zu besuchen und diese Filter zu verwenden:

* Gefiltert nach [SQL-Datenbank](https://azure.microsoft.com/updates/?service=sql-database).
* Gefiltert nach allgemeiner Verfügbarkeit [Ankündigungen](http://azure.microsoft.com/updates/?service=sql-database&update-type=general-availability) für SQL-Datenbankfunktionen.

| **Feature** | **SQL Server** | **Azure SQL-Datenbank** | 
| --- | :---: | :---: | 
| Aktive Georeplikation | Nicht unterstützt – siehe [Always On-Verfügbarkeitsgruppen](https://docs.microsoft.com/sql/database-engine/availability-groups/windows/always-on-availability-groups-sql-server) | [Unterstützt](sql-database-geo-replication-overview.md)
| Always Encrypted | [Unterstützt](https://docs.microsoft.com/sql/relational-databases/security/encryption/always-encrypted-database-engine) | Unterstützt – siehe [Zertifikatspeicher](sql-database-always-encrypted.md) und [Schlüsseltresor](sql-database-always-encrypted-azure-key-vault.md)|
| AlwaysOn-Verfügbarkeitsgruppen | [Unterstützt](https://docs.microsoft.com/sql/database-engine/availability-groups/windows/always-on-availability-groups-sql-server) | Nicht unterstützt – siehe [Aktive Georeplikation](sql-database-geo-replication-overview.md) |
| Anfügen einer Datenbank | [Unterstützt](https://docs.microsoft.com/sql/relational-databases/databases/attach-a-database) | Nicht unterstützt |
| Anwendungsrollen | [Unterstützt](https://docs.microsoft.com/sql/relational-databases/security/authentication-access/application-roles) | [Unterstützt](https://docs.microsoft.com/sql/relational-databases/security/authentication-access/application-roles) |
| Automatische Skalierung | Nicht unterstützt | Unterstützt – siehe [Dienstebenen](sql-database-service-tiers.md) |
| Azure Active Directory | Nicht unterstützt | [Unterstützt](sql-database-aad-authentication.md) |
| Azure Data Factory | [Unterstützt](../data-factory/data-factory-introduction.md) | [Unterstützt](../data-factory/data-factory-introduction.md) |
| Überwachung | [Unterstützt](https://docs.microsoft.com/sql/relational-databases/security/auditing/sql-server-audit-database-engine) | [Unterstützt](sql-database-auditing.md) |
| BACPAC Datei (exportieren) | [Unterstützt](https://docs.microsoft.com/sql/relational-databases/data-tier-applications/export-a-data-tier-application) | [Unterstützt](sql-database-export.md) |
| BACPAC Datei (importieren) | [Unterstützt](https://docs.microsoft.com/sql/relational-databases/data-tier-applications/import-a-bacpac-file-to-create-a-new-user-database) | [Unterstützt](sql-database-import.md) |
| SICHERUNG | [Unterstützt](https://docs.microsoft.com/sql/t-sql/statements/backup-transact-sql) | Nicht unterstützt |
| Integrierte Funktionen | [Unterstützt](https://docs.microsoft.com/sql/t-sql/functions/functions) | Die meisten – siehe [Einzelne Funktionen] (https://docs.microsoft.com/sql/t-sql/functions/functions) |
| Erfassung geänderter Daten | [Unterstützt](https://docs.microsoft.com/sql/relational-databases/track-changes/about-change-data-capture-sql-server) | Nicht unterstützt |
| Änderungsnachverfolgung | [Unterstützt](https://docs.microsoft.com/sql/relational-databases/track-changes/about-change-tracking-sql-server) | [Unterstützt](https://docs.microsoft.com/sql/relational-databases/track-changes/about-change-tracking-sql-server) |
| Sortierung-Anweisungen | [Unterstützt](https://docs.microsoft.com/sql/t-sql/statements/collations) | [Unterstützt](https://docs.microsoft.com/sql/t-sql/statements/collations) |
| ColumnStore-Indizes | [Unterstützt](https://docs.microsoft.com/sql/relational-databases/indexes/columnstore-indexes-overview) | [Nur Premium Edition](https://docs.microsoft.com/sql/relational-databases/indexes/columnstore-indexes-overview) |
| Common Language Runtime (CLR) | [Unterstützt](https://docs.microsoft.com/sql/relational-databases/clr-integration/common-language-runtime-clr-integration-programming-concepts) | Nicht unterstützt |
| Eigenständige Datenbanken | [Unterstützt](https://docs.microsoft.com/sql/relational-databases/databases/contained-databases) | [Unterstützt](https://docs.microsoft.com/sql/relational-databases/databases/contained-databases) |
| Enthaltene Benutzer | [Unterstützt](https://docs.microsoft.com/sql/relational-databases/security/contained-database-users-making-your-database-portable) | [Unterstützt](sql-database-manage-logins.md#non-administrator-users) |
| Schlüsselwörter der Sprache zur Ablaufsteuerung | [Unterstützt](https://docs.microsoft.com/sql/t-sql/language-elements/control-of-flow) | [Unterstützt](https://docs.microsoft.com/sql/t-sql/language-elements/control-of-flow) |
| Datenbankübergreifende Abfragen | [Unterstützt](https://docs.microsoft.com/sql/relational-databases/in-memory-oltp/cross-database-queries) | Teilweise – siehe [Elastische Abfragen](sql-database-elastic-query-overview.md) |
| Cursor | [Unterstützt](https://docs.microsoft.com/sql/t-sql/language-elements/cursors-transact-sql) | [Unterstützt](https://docs.microsoft.com/sql/t-sql/language-elements/cursors-transact-sql) | 
| Datenkomprimierung | [Unterstützt](https://docs.microsoft.com/sql/relational-databases/data-compression/data-compression) | [Unterstützt](https://docs.microsoft.com/sql/relational-databases/data-compression/data-compression) |
| Datenbanksicherungen | [Vom Benutzer verwaltet](https://docs.microsoft.com/sql/relational-databases/backup-restore/back-up-and-restore-of-sql-server-databases) | [Vom SQL-Datenbankdienst verwaltet](sql-database-automated-backups.md) |
| Datenbank-E-Mail | [Unterstützt](https://docs.microsoft.com/sql/relational-databases/database-mail/database-mail) | Nicht unterstützt |
| Datenbankspiegelung | [Unterstützt](https://docs.microsoft.com/sql/database-engine/database-mirroring/database-mirroring-sql-server) | Nicht unterstützt |
| Konfigurationseinstellungen für Datenbank | [Unterstützt](https://docs.microsoft.com/sql/t-sql/statements/alter-database-scoped-configuration-transact-sql) | [Unterstützt](https://docs.microsoft.com/sql/t-sql/statements/alter-database-scoped-configuration-transact-sql) |
| Data Quality Services (DQS) | [Unterstützt](https://docs.microsoft.com/sql/data-quality-services/data-quality-services) | Nicht unterstützt |
| Datenbank-Momentaufnahmen | [Unterstützt](https://docs.microsoft.com/sql/relational-databases/databases/database-snapshots-sql-server) | Nicht unterstützt |
| Datentypen | [Unterstützt](https://docs.microsoft.com/sql/t-sql/data-types/data-types-transact-sql) | [Unterstützt](https://docs.microsoft.com/sql/t-sql/data-types/data-types-transact-sql) |  
| DBCC-Anweisungen | [Unterstützt](https://docs.microsoft.com/sql/t-sql/database-console-commands/dbcc-transact-sql) | Die meisten – siehe [Einzelne Anweisungen](https://docs.microsoft.com/sql/t-sql/database-console-commands/dbcc-transact-sql) |
| DDL-Anweisungen | [Unterstützt](https://docs.microsoft.com/sql/t-sql/statements/statements) | Die meisten – siehe [Einzelne Anweisungen](https://docs.microsoft.com/sql/t-sql/statements/statements)
| DDL-Trigger | [Unterstützt](https://docs.microsoft.com/sql/relational-databases/triggers/ddl-triggers) | [Nur Datenbank](https://docs.microsoft.com/sql/relational-databases/triggers/ddl-triggers) |
| Verteilte Transaktionen | [MS DTC](https://docs.microsoft.com/sql/relational-databases/native-client-ole-db-transactions/supporting-distributed-transactions) | Nur begrenzte interne SQL-Datenbank-Szenarios |
| DML-Anweisungen | [Unterstützt](https://docs.microsoft.com/sql/t-sql/queries/queries) | Die meisten – siehe [Einzelne Anweisungen]](https://docs.microsoft.com/sql/t-sql/queries/queries) |
| DML-Trigger | [Unterstützt](https://docs.microsoft.com/sql/relational-databases/triggers/dml-triggers) | [Unterstützt](https://docs.microsoft.com/sql/relational-databases/triggers/dml-triggers) |
| DMVs | [Alle](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/system-dynamic-management-views) | Einige – siehe [Einzelne DMVs](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/system-dynamic-management-views) |
| Elastische Pools | Nicht unterstützt | [Unterstützt](sql-database-elastic-pool.md) |
| Elastische Aufträge | Nicht unterstützt – siehe [SQL Server-Agent](https://docs.microsoft.com/sql/ssms/agent/sql-server-agent) | [Unterstützt](sql-database-elastic-jobs-getting-started.md) | 
| Elastische Abfragen | Nicht unterstützt – siehe [Datenbankübergreifende Abfragen](https://docs.microsoft.com/sql/relational-databases/in-memory-oltp/cross-database-queries) | [Unterstützt](sql-database-elastic-query-overview.md) |
| Ereignisbenachrichtigungen | [Unterstützt](https://docs.microsoft.com/sql/relational-databases/service-broker/event-notifications) | [Unterstützt](sql-database-insights-alerts-portal.md) |
| Ausdrücke | [Unterstützt](https://docs.microsoft.com/sql/t-sql/language-elements/expressions-transact-sql) | [Unterstützt](https://docs.microsoft.com/sql/t-sql/language-elements/expressions-transact-sql) |
| Erweiterte Ereignisse | [Unterstützt](https://docs.microsoft.com/sql/relational-databases/extended-events/extended-events) | Einige – siehe [Einzelne Ereignisse](sql-database-xevent-db-diff-from-svr.md) |
| Erweiterte gespeicherte Prozeduren | [Unterstützt](https://docs.microsoft.com/sql/relational-databases/extended-stored-procedures-programming/creating-extended-stored-procedures) | Nicht unterstützt |
| Dateien und Dateigruppen | [Unterstützt](https://docs.microsoft.com/sql/relational-databases/databases/database-files-and-filegroups) | [Nur primär](https://docs.microsoft.com/sql/relational-databases/databases/database-files-and-filegroups) |
| Filestream | [Unterstützt](https://docs.microsoft.com/sql/relational-databases/blob/filestream-sql-server) | Nicht unterstützt |
| Volltextsuche | [Unterstützt](https://docs.microsoft.com/sql/relational-databases/search/full-text-search) | [Nicht unterstützte Worttrennungen von Drittanbietern](https://docs.microsoft.com/sql/relational-databases/search/full-text-search) |
| Functions | [Unterstützt](https://docs.microsoft.com/sql/t-sql/functions/functions) | Die meisten – siehe [Einzelne Funktionen](https://docs.microsoft.com/sql/t-sql/functions/functions) |
| In-Memory-Optimierung | [Unterstützt](https://docs.microsoft.com/sql/relational-databases/in-memory-oltp/in-memory-oltp-in-memory-optimization) | [Nur Premium Edition](sql-database-in-memory.md) |
| Aufträge | Siehe [SQL Server-Agent](https://docs.microsoft.com/sql/ssms/agent/sql-server-agent) | Siehe [Elastische Aufträge](sql-database-elastic-jobs-getting-started.md) |
| Unterstützung von JSON-Daten | [Unterstützt](https://docs.microsoft.com/sql/relational-databases/json/json-data-sql-server) | [Unterstützt](sql-database-json-features.md) |
| Sprachelemente | [Unterstützt](https://docs.microsoft.com/sql/t-sql/language-elements/language-elements-transact-sql) | Die meisten – siehe [Einzelne Elemente](https://docs.microsoft.com/sql/t-sql/language-elements/language-elements-transact-sql) |  
| Verknüpfte Server | [Unterstützt](https://docs.microsoft.com/sql/relational-databases/linked-servers/linked-servers-database-engine) | Nicht unterstützt – siehe [Elastic query (Elastische Abfrage)](sql-database-elastic-query-horizontal-partitioning.md) |
| Protokollversand | [Unterstützt](https://docs.microsoft.com/sql/database-engine/log-shipping/about-log-shipping-sql-server) | Nicht unterstützt – siehe [Aktive Georeplikation](sql-database-geo-replication-overview.md) |
| Master Data Services (MDS) | [Unterstützt](https://docs.microsoft.com/sql/master-data-services/master-data-services-overview-mds) | Nicht unterstützt |
| Minimale Protokollierung bei Massenimport | [Unterstützt](https://docs.microsoft.com/sql/relational-databases/import-export/prerequisites-for-minimal-logging-in-bulk-import) | Nicht unterstützt |
| Ändern von Systemdaten | [Unterstützt](https://docs.microsoft.com/sql/relational-databases/databases/system-databases) | Nicht unterstützt |
| Online-Indexvorgänge | [Unterstützt](https://docs.microsoft.com/sql/relational-databases/indexes/perform-index-operations-online) | [Unterstützt – Transaktionsgröße durch Dienstebene eingeschränkt](https://docs.microsoft.com/sql/relational-databases/indexes/perform-index-operations-online) |
| Operatoren | [Unterstützt](https://docs.microsoft.com/sql/t-sql/language-elements/operators-transact-sql) | Die meisten – siehe [Einzelne Operatoren](https://docs.microsoft.com/sql/t-sql/language-elements/operators-transact-sql) |
| Point-in-Time-Wiederherstellung einer Datenbank | [Unterstützt](https://docs.microsoft.com/sql/relational-databases/backup-restore/restore-a-sql-server-database-to-a-point-in-time-full-recovery-model) | [Unterstützt](sql-database-recovery-using-backups.md#point-in-time-restore) |
| PolyBase | [Unterstützt](https://docs.microsoft.com/sql/relational-databases/polybase/polybase-guide) | Nicht unterstützt
| Richtlinienbasierte Verwaltung | [Unterstützt](https://docs.microsoft.com/sql/relational-databases/policy-based-management/administer-servers-by-using-policy-based-management) | Nicht unterstützt |
| Prädikate | [Unterstützt](https://docs.microsoft.com/sql/t-sql/queries/predicates) | Die meisten – siehe [Einzelne Prädikate](https://docs.microsoft.com/sql/t-sql/queries/predicates)
| R Services | [Unterstützt](https://docs.microsoft.com/sql/advanced-analytics/r-services/sql-server-r-services)
| Ressourcenkontrolle | [Unterstützt](https://docs.microsoft.com/sql/relational-databases/resource-governor/resource-governor) | Nicht unterstützt |
| RESTORE-Anweisungen | [Unterstützt](https://docs.microsoft.com/sql/t-sql/statements/restore-statements-for-restoring-recovering-and-managing-backups-transact-sql) | Nicht unterstützt | 
| Wiederherstellen einer Datenbank aus der Sicherung | [Unterstützt](https://docs.microsoft.com/sql/relational-databases/backup-restore/back-up-and-restore-of-sql-server-databases#restore-data-backups) | [Nur von integrierten Sicherungen](sql-database-recovery-using-backups.md) |
| Sicherheit auf Zeilenebene | [Unterstützt](https://docs.microsoft.com/sql/relational-databases/security/row-level-security) | [Unterstützt](https://docs.microsoft.com/sql/relational-databases/security/row-level-security) |
| Semantische Suche | [Unterstützt](https://docs.microsoft.com/sql/relational-databases/search/semantic-search-sql-server) | Nicht unterstützt |
| Sequenznummern | [Unterstützt](https://docs.microsoft.com/sql/relational-databases/sequence-numbers/sequence-numbers) | [Unterstützt](https://docs.microsoft.com/sql/relational-databases/sequence-numbers/sequence-numbers) |
| Service Broker | [Unterstützt](https://docs.microsoft.com/sql/database-engine/configure-windows/sql-server-service-broker) | Nicht unterstützt |
| Konfigurationseinstellungen für Server | [Unterstützt](https://docs.microsoft.com/sql/database-engine/configure-windows/server-configuration-options-sql-server) | Nicht unterstützt – siehe [Database configuration options (Datenbank-Konfigurationsoptionen)](https://docs.microsoft.com/sql/t-sql/statements/alter-database-scoped-configuration-transact-sql) |
| SET-Anweisungen | [Unterstützt](https://docs.microsoft.com/sql/t-sql/statements/set-statements-transact-sql) | Die meisten – siehe [Einzelne Anweisungen](https://docs.microsoft.com/sql/t-sql/statements/set-statements-transact-sql) 
| Spatial | [Unterstützt](https://docs.microsoft.com/sql/relational-databases/spatial/spatial-data-sql-server) | [Unterstützt](https://docs.microsoft.com/sql/relational-databases/spatial/spatial-data-sql-server) |
| SQL Server-Agent | [Unterstützt](https://docs.microsoft.com/sql/ssms/agent/sql-server-agent) | Nicht unterstützt – siehe [elastische Einzelvorgänge](sql-database-elastic-jobs-getting-started.md) |
| SQL Server Analysis Services (SSAS) | [Unterstützt](https://docs.microsoft.com/sql/analysis-services/analysis-services) | Nicht unterstützt – siehe [Azure Analysis Services](https://azure.microsoft.com/services/analysis-services/) |
| SQL Server Integration Services (SSIS) | [Unterstützt](https://docs.microsoft.com/sql/integration-services/sql-server-integration-services) | Nicht unterstützt – siehe [Azure Data Factory](https://azure.microsoft.com/services/data-factory/) |
| SQL Server PowerShell | [Unterstützt](https://docs.microsoft.com/sql/relational-databases/scripting/sql-server-powershell) | [Unterstützt](https://docs.microsoft.com/sql/relational-databases/scripting/sql-server-powershell) |
| SQL Server Profiler | [Unterstützt](https://docs.microsoft.com/sql/tools/sql-server-profiler/sql-server-profiler) | Nicht unterstützt – siehe [Erweiterte Ereignisse](sql-database-xevent-db-diff-from-svr.md) |
| SQL Server-Replikation | [Unterstützt](https://docs.microsoft.com/sql/relational-databases/replication/sql-server-replication) | [Nur für Transaktions- und Momentaufnahmenreplikationsabonnent](sql-database-cloud-migrate.md) |
| SQL Server Reporting Services (SSRS) | [Unterstützt](https://docs.microsoft.com/sql/reporting-services/create-deploy-and-manage-mobile-and-paginated-reports) | Nicht unterstützt |
| Gespeicherte Prozeduren | [Unterstützt](https://docs.microsoft.com/sql/relational-databases/stored-procedures/stored-procedures-database-engine) | [Unterstützt](https://docs.microsoft.com/sql/relational-databases/stored-procedures/stored-procedures-database-engine) |
| Gespeicherte Systemfunktionen | [Unterstützt](https://docs.microsoft.com/sql/relational-databases/system-functions/system-functions-for-transact-sql) | Einige – siehe [Einzelne Funktionen](https://docs.microsoft.com/sql/relational-databases/system-functions/system-functions-for-transact-sql) |
| Gespeicherte Systemprozeduren | [Unterstützt](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/system-stored-procedures-transact-sql) | Einige – siehe [Einzelne gespeicherte Prozeduren](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/system-stored-procedures-transact-sql) |
| Systemtabellen | [Unterstützt](https://docs.microsoft.com/sql/relational-databases/system-tables/system-tables-transact-sql) | Einige – siehe [Einzelne Tabellen](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/system-stored-procedures-transact-sql) |
| Systemkatalogsichten | [Unterstützt](https://docs.microsoft.com/sql/relational-databases/system-catalog-views/catalog-views-transact-sql) | Einige – siehe [Einzelne Sichten](https://docs.microsoft.com/sql/relational-databases/system-catalog-views/catalog-views-transact-sql)
| Tabellenpartitionierung | [Unterstützt](https://docs.microsoft.com/sql/relational-databases/partitions/partitioned-tables-and-indexes) | Unterstützt – [Nur primäre Dateigruppe](https://docs.microsoft.com/sql/relational-databases/partitions/partitioned-tables-and-indexes) |
| Temporäre Tabellen | [Lokal und global](https://docs.microsoft.com/sql/t-sql/statements/create-table-transact-sql#temporary-tables) | [Nur lokal](https://docs.microsoft.com/sql/t-sql/statements/create-table-transact-sql#temporary-tables) |
| Temporäre Tabellen | [Unterstützt](https://docs.microsoft.com/sql/relational-databases/tables/temporal-tables) | [Unterstützt](sql-database-temporal-tables.md) |
| Transaktionen | [Unterstützt](https://docs.microsoft.com/sql/t-sql/language-elements/transactions-transact-sql) | [Unterstützt](https://docs.microsoft.com/sql/t-sql/language-elements/transactions-transact-sql) |
| Variablen | [Unterstützt](https://docs.microsoft.com/sql/t-sql/language-elements/variables-transact-sql) | [Unterstützt](https://docs.microsoft.com/sql/t-sql/language-elements/variables-transact-sql) | 
| Transparent Data Encryption (TDE)  | [Unterstützt](https://docs.microsoft.com/sql/relational-databases/security/encryption/transparent-data-encryption-tde) | [Unterstützt](https://docs.microsoft.com/sql/relational-databases/security/encryption/transparent-data-encryption-tde) |
| Windows Server-Failoverclustering | [Unterstützt](https://docs.microsoft.com/sql/sql-server/failover-clusters/windows/windows-server-failover-clustering-wsfc-with-sql-server) | Nicht unterstützt – siehe [Aktive Georeplikation](sql-database-geo-replication-overview.md) |
| XML-Indizes | [Unterstützt](https://docs.microsoft.com/sql/t-sql/statements/create-xml-index-transact-sql) | [Unterstützt](https://docs.microsoft.com/sql/t-sql/statements/create-xml-index-transact-sql) |

## <a name="next-steps"></a>Nächste Schritte

- Informationen über den Azure SQL-Datenbank-Dienst finden Sie unter [Was ist SQL Database? Einführung in SQL-Datenbank](sql-database-technical-overview.md).
- Informationen zur Transact-SQL-Unterstützung und zu den Unterschieden finden Sie unter [Auflösen von Transact-SQL-Unterschieden während der Migration zur SQL-Datenbank](sql-database-transact-sql-information.md).

