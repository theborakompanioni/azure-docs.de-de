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
ms.date: 08/25/2017
ms.author: carlrab
ms.translationtype: HT
ms.sourcegitcommit: 5b6c261c3439e33f4d16750e73618c72db4bcd7d
ms.openlocfilehash: 4ef119d5218c844f3949bf4fc17fb320a7cbf7b4
ms.contentlocale: de-de
ms.lasthandoff: 08/28/2017

---
# <a name="azure-sql-database-features"></a>Funktionen von Azure SQL-Datenbank

Azure SQL-Datenbank verwendet die gleiche Codebasis wie SQL Server und unterstützt auf der Datenbankebene größtenteils die gleichen Features. Die Features von Azure SQL-Datenbank und SQL Server unterscheiden sich hauptsächlich auf der Instanzebene. 

Wir fügen Azure SQL-Datenbank ständig weitere Features hinzu. Daher empfehlen wir Ihnen, die Webseite "Dienstupdates" für Azure zu besuchen und diese Filter zu verwenden:

* Gefiltert nach [SQL-Datenbank](https://azure.microsoft.com/updates/?service=sql-database).
* Gefiltert nach allgemeiner Verfügbarkeit [Ankündigungen](http://azure.microsoft.com/updates/?service=sql-database&update-type=general-availability) für SQL-Datenbankfunktionen.

## <a name="sql-server-and-sql-database-feature-support"></a>Unterstützung von SQL Server- und SQL-Datenbank-Features

Die folgende Tabelle enthält die wichtigsten Features von SQL Server und gibt Aufschluss darüber, ob die einzelnen Features unterstützt werden. Darüber hinaus enthält die Tabelle jeweils einen Link zu weiteren Featureinformationen. Informationen zu Transact-SQL-Unterschieden, die beim Migrieren einer vorhandenen Datenbanklösung berücksichtigt werden sollten, finden Sie unter [Auflösen von Transact-SQL-Unterschieden während der Migration zur SQL-Datenbank](sql-database-transact-sql-information.md).


| **SQL Server-Feature** | **Unterstützt in Azure SQL-Datenbank** | 
| --- | --- |  
| [Always Encrypted](https://docs.microsoft.com/sql/relational-databases/security/encryption/always-encrypted-database-engine) | Ja (siehe [Zertifikatspeicher](sql-database-always-encrypted.md) und [Schlüsseltresor](sql-database-always-encrypted-azure-key-vault.md))|
| [AlwaysOn-Verfügbarkeitsgruppen](https://docs.microsoft.com/sql/database-engine/availability-groups/windows/always-on-availability-groups-sql-server) | Nein (siehe [Failovergruppen und aktive Georeplikation](sql-database-geo-replication-overview.md)) |
| [Anfügen einer Datenbank](https://docs.microsoft.com/sql/relational-databases/databases/attach-a-database) | Nein |
| [Anwendungsrollen](https://docs.microsoft.com/sql/relational-databases/security/authentication-access/application-roles) | Ja |
| [BACPAC Datei (Export)](https://docs.microsoft.com/sql/relational-databases/data-tier-applications/export-a-data-tier-application) | Ja (siehe [SQL-­Datenbank-Export](sql-database-export.md)) |
| [BACPAC Datei (Import)](https://docs.microsoft.com/sql/relational-databases/data-tier-applications/import-a-bacpac-file-to-create-a-new-user-database) | Ja (siehe [SQL-­Datenbank-Import](sql-database-import.md)) |
| [Befehl „BACKUP“](https://docs.microsoft.com/sql/t-sql/statements/backup-transact-sql) | Nein (siehe [Automatisierte Sicherungen](sql-database-automated-backups.md)) |
| [Integrierte Funktionen](https://docs.microsoft.com/sql/t-sql/functions/functions) | Die meisten (siehe einzelne Funktionen) |
| [Erfassung geänderter Daten](https://docs.microsoft.com/sql/relational-databases/track-changes/about-change-data-capture-sql-server) | Nein |
| [Änderungsnachverfolgung](https://docs.microsoft.com/sql/relational-databases/track-changes/about-change-tracking-sql-server) | Ja |
| [Sortierungsanweisungen](https://docs.microsoft.com/sql/t-sql/statements/collations) | Ja |
| [ColumnStore-Indizes](https://docs.microsoft.com/sql/relational-databases/indexes/columnstore-indexes-overview) | Ja ([nur Premium Edition](https://docs.microsoft.com/sql/relational-databases/indexes/columnstore-indexes-overview)) |
| [Common Language Runtime (CLR)](https://docs.microsoft.com/sql/relational-databases/clr-integration/common-language-runtime-clr-integration-programming-concepts) | Nein |
| [Eigenständige Datenbanken](https://docs.microsoft.com/sql/relational-databases/databases/contained-databases) | Ja |
| [Eigenständige Benutzer](https://docs.microsoft.com/sql/relational-databases/security/contained-database-users-making-your-database-portable) | Ja |
| [Schlüsselwörter der Sprache zur Ablaufsteuerung](https://docs.microsoft.com/sql/t-sql/language-elements/control-of-flow) | Ja |
| [Datenbankübergreifende Abfragen](https://docs.microsoft.com/sql/relational-databases/in-memory-oltp/cross-database-queries) | Teilweise – siehe [Elastische Abfragen](sql-database-elastic-query-overview.md) |
| [Cursor](https://docs.microsoft.com/sql/t-sql/language-elements/cursors-transact-sql) | Ja | 
| [Datenkomprimierung](https://docs.microsoft.com/sql/relational-databases/data-compression/data-compression) | Ja |
| [Datenbank-E-Mails](https://docs.microsoft.com/sql/relational-databases/database-mail/database-mail) | Nein |
| [Spiegeln von Datenbanken](https://docs.microsoft.com/sql/database-engine/database-mirroring/database-mirroring-sql-server) | Nein |
| [Datenbankkonfigurationseinstellungen](https://docs.microsoft.com/sql/t-sql/statements/alter-database-scoped-configuration-transact-sql) | Ja |
| [Data Quality Services (DQS)](https://docs.microsoft.com/sql/data-quality-services/data-quality-services) | Nein |
| [Datenbankmomentaufnahmen](https://docs.microsoft.com/sql/relational-databases/databases/database-snapshots-sql-server) | Nein |
| [Datentypen](https://docs.microsoft.com/sql/t-sql/data-types/data-types-transact-sql) | Ja |  
| [DBCC-Anweisungen](https://docs.microsoft.com/sql/t-sql/database-console-commands/dbcc-transact-sql) | Die meisten (siehe einzelne Anweisungen) |
| [DDL-Anweisungen](https://docs.microsoft.com/sql/t-sql/statements/statements) | Die meisten (siehe einzelne Anweisungen)
| [DDL-Trigger](https://docs.microsoft.com/sql/relational-databases/triggers/ddl-triggers) | Nur Datenbank |
| [Verteilte Transaktionen – MS DTC](https://docs.microsoft.com/sql/relational-databases/native-client-ole-db-transactions/supporting-distributed-transactions) | Nein (siehe [Elastische Transaktionen](sql-database-elastic-transactions-overview.md)) |
| [DML-Anweisungen](https://docs.microsoft.com/sql/t-sql/queries/queries) | Die meisten (siehe einzelne Anweisungen) |
| [DML-Trigger](https://docs.microsoft.com/sql/relational-databases/triggers/dml-triggers) |
| [DMVs](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/system-dynamic-management-views) | Einige (siehe einzelne DMVs) |
| [Ereignisbenachrichtigungen](https://docs.microsoft.com/sql/relational-databases/service-broker/event-notifications) | Nein (siehe [Warnungen](sql-database-insights-alerts-portal.md)) |
| [Ausdrücke](https://docs.microsoft.com/sql/t-sql/language-elements/expressions-transact-sql) |Ja |
| [Erweiterte Ereignisse](https://docs.microsoft.com/sql/relational-databases/extended-events/extended-events) | Einige (siehe einzelne Ereignisse) |
| [Erweiterte gespeicherte Prozeduren](https://docs.microsoft.com/sql/relational-databases/extended-stored-procedures-programming/creating-extended-stored-procedures) | Nein |
| [Dateien und Dateigruppen](https://docs.microsoft.com/sql/relational-databases/databases/database-files-and-filegroups) | Nur primäre Dateigruppe |
| [Filestream](https://docs.microsoft.com/sql/relational-databases/blob/filestream-sql-server) | Nein |
| [Volltextsuche](https://docs.microsoft.com/sql/relational-databases/search/full-text-search) | Worttrennungen von Drittanbietern werden nicht unterstützt. |
| [Funktionen](https://docs.microsoft.com/sql/t-sql/functions/functions) | Die meisten (siehe einzelne Funktionen) |
| [Graph-Verarbeitung](/sql/relational-databases/graphs/sql-graph-overview) | Ja |
| [In-Memory-Optimierung](https://docs.microsoft.com/sql/relational-databases/in-memory-oltp/in-memory-oltp-in-memory-optimization) | Ja ([nur Premium Edition](sql-database-in-memory.md)) |
| [Unterstützung von JSON-Daten](https://docs.microsoft.com/sql/relational-databases/json/json-data-sql-server) | Ja |
| [Sprachelemente](https://docs.microsoft.com/sql/t-sql/language-elements/language-elements-transact-sql) | Die meisten (siehe einzelne Elemente) |  
| [Verknüpfte Server](https://docs.microsoft.com/sql/relational-databases/linked-servers/linked-servers-database-engine) | Nein (siehe [Elastische Abfrage](sql-database-elastic-query-horizontal-partitioning.md)) |
| [Protokollversand](https://docs.microsoft.com/sql/database-engine/log-shipping/about-log-shipping-sql-server) | Nein (siehe [Failovergruppen und aktive Georeplikation](sql-database-geo-replication-overview.md)) |
| [Master Data Services (MDS)](https://docs.microsoft.com/sql/master-data-services/master-data-services-overview-mds) | Nein |
| [Minimale Protokollierung bei Massenimport](https://docs.microsoft.com/sql/relational-databases/import-export/prerequisites-for-minimal-logging-in-bulk-import) | Nein |
| [Ändern von Systemdaten](https://docs.microsoft.com/sql/relational-databases/databases/system-databases) | Nein |
| [Onlineindexvorgänge](https://docs.microsoft.com/sql/relational-databases/indexes/perform-index-operations-online) | Unterstützt (Transaktionsgröße durch Dienstebene eingeschränkt) |
| [Operatoren](https://docs.microsoft.com/sql/t-sql/language-elements/operators-transact-sql) | Die meisten (siehe einzelne Operatoren) |
| [Point-in-Time-Wiederherstellung einer Datenbank](https://docs.microsoft.com/sql/relational-databases/backup-restore/restore-a-sql-server-database-to-a-point-in-time-full-recovery-model) | Ja (siehe [SQL-­Datenbank-Wiederherstellung](sql-database-recovery-using-backups.md#point-in-time-restore)) |
| [Polybase](https://docs.microsoft.com/sql/relational-databases/polybase/polybase-guide) | Nein |
| [Richtlinienbasierte Verwaltung](https://docs.microsoft.com/sql/relational-databases/policy-based-management/administer-servers-by-using-policy-based-management) | Nein |
| [Prädikate](https://docs.microsoft.com/sql/t-sql/queries/predicates) | Die meisten (siehe einzelne Prädikate) |
| [R Services](https://docs.microsoft.com/sql/advanced-analytics/r-services/sql-server-r-services) | Nein |
| [Ressourcenkontrolle](https://docs.microsoft.com/sql/relational-databases/resource-governor/resource-governor) | Nein |
| [RESTORE-Anweisungen](https://docs.microsoft.com/sql/t-sql/statements/restore-statements-for-restoring-recovering-and-managing-backups-transact-sql) | Nein | 
| [Wiederherstellen einer Datenbank aus der Sicherung](https://docs.microsoft.com/sql/relational-databases/backup-restore/back-up-and-restore-of-sql-server-databases#restore-data-backups) | Nur auf der Grundlage integrierter Sicherungen (siehe [SQL-Datenbank-Wiederherstellung](sql-database-recovery-using-backups.md)) |
| [Sicherheit auf Zeilenebene](https://docs.microsoft.com/sql/relational-databases/security/row-level-security) | Ja |
| [Semantische Suche](https://docs.microsoft.com/sql/relational-databases/search/semantic-search-sql-server) | Nein |
| [Sequenznummern](https://docs.microsoft.com/sql/relational-databases/sequence-numbers/sequence-numbers) | Ja |
| [Service Broker](https://docs.microsoft.com/sql/database-engine/configure-windows/sql-server-service-broker) | Nein |
| [Serverkonfigurationseinstellungen](https://docs.microsoft.com/sql/database-engine/configure-windows/server-configuration-options-sql-server) | Nein |
| [Set-Anweisungen](https://docs.microsoft.com/sql/t-sql/statements/set-statements-transact-sql) | Die meisten (siehe einzelne Anweisungen) 
| [Spatial](https://docs.microsoft.com/sql/relational-databases/spatial/spatial-data-sql-server) | Ja |
| [SQL Server-Agent](https://docs.microsoft.com/sql/ssms/agent/sql-server-agent) | Nein (siehe [Elastische Aufträge](sql-database-elastic-jobs-getting-started.md)) |
| [SQL Server Analysis Services (SSAS)](https://docs.microsoft.com/sql/analysis-services/analysis-services) | Nein (siehe [Azure Analysis Services](https://azure.microsoft.com/services/analysis-services/)) |
| [SQL Server-Überwachung](https://docs.microsoft.com/sql/relational-databases/security/auditing/sql-server-audit-database-engine) | Nein (siehe [SQL-Datenbank-Überwachung](sql-database-auditing.md)) |
| [SQL Server Integration Services (SSIS)](https://docs.microsoft.com/sql/integration-services/sql-server-integration-services) | Nein (siehe [Azure Data Factory](https://azure.microsoft.com/services/data-factory/)) |
| [SQL Server PowerShell](https://docs.microsoft.com/sql/relational-databases/scripting/sql-server-powershell) | Ja |
| SQL Server Profiler | [Unterstützt](https://docs.microsoft.com/sql/tools/sql-server-profiler/sql-server-profiler) | Nein (siehe [Erweiterte Ereignisse](sql-database-xevent-db-diff-from-svr.md)) |
| [SQL Server-Replikation](https://docs.microsoft.com/sql/relational-databases/replication/sql-server-replication) | [Nur für Transaktions- und Momentaufnahmenreplikationsabonnent](sql-database-cloud-migrate.md) |
| [SQL Server Reporting Services (SSRS)](https://docs.microsoft.com/sql/reporting-services/create-deploy-and-manage-mobile-and-paginated-reports) | Nein |
| [Gespeicherten Prozeduren](https://docs.microsoft.com/sql/relational-databases/stored-procedures/stored-procedures-database-engine) | Ja |
| [Gespeicherte Systemfunktionen](https://docs.microsoft.com/sql/relational-databases/system-functions/system-functions-for-transact-sql) | Einige (siehe einzelne Funktionen) |
| [Gespeicherte Systemprozeduren](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/system-stored-procedures-transact-sql) | Einige (siehe einzelne gespeicherte Prozeduren) |
| [Systemtabellen](https://docs.microsoft.com/sql/relational-databases/system-tables/system-tables-transact-sql) | Einige (siehe einzelne Tabellen) |
| [Systemkatalogsichten](https://docs.microsoft.com/sql/relational-databases/system-catalog-views/catalog-views-transact-sql) | Einige (siehe einzelne Sichten) |
| [Tabellenpartitionierung](https://docs.microsoft.com/sql/relational-databases/partitions/partitioned-tables-and-indexes) | Ja (nur primäre Dateigruppe) |
| [Temporäre Tabellen](https://docs.microsoft.com/sql/t-sql/statements/create-table-transact-sql#temporary-tables) | Nur lokale und datenbankbezogene globale temporäre Tabellen |
| [Temporäre Tabellen](https://docs.microsoft.com/sql/relational-databases/tables/temporal-tables) | Ja |
| [Transaktionen](https://docs.microsoft.com/sql/t-sql/language-elements/transactions-transact-sql) | Nein |
| [Variablen](https://docs.microsoft.com/sql/t-sql/language-elements/variables-transact-sql) | Ja | 
| [Transparent Data Encryption (TDE)](https://docs.microsoft.com/sql/relational-databases/security/encryption/transparent-data-encryption-tde) | Ja |
| [ Windows Server-Failoverclustering](https://docs.microsoft.com/sql/sql-server/failover-clusters/windows/windows-server-failover-clustering-wsfc-with-sql-server) | Nein (siehe [Failovergruppen und aktive Georeplikation](sql-database-geo-replication-overview.md)) |
| [XML-Indizes](https://docs.microsoft.com/sql/t-sql/statements/create-xml-index-transact-sql) | Ja |

## <a name="next-steps"></a>Nächste Schritte

- Informationen über den Azure SQL-Datenbank-Dienst finden Sie unter [Was ist SQL Database? Einführung in SQL-Datenbank](sql-database-technical-overview.md).
- Informationen zur Transact-SQL-Unterstützung und zu den Unterschieden finden Sie unter [Auflösen von Transact-SQL-Unterschieden während der Migration zur SQL-Datenbank](sql-database-transact-sql-information.md).

