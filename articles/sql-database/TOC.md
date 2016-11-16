# Übersicht
## [Was ist SQL Database?](sql-database-technical-overview.md)
## [SQL-Datenbank – Häufig gestellte Fragen](sql-database-faq.md)
## Features
### [Dienstebenen](sql-database-service-tiers.md)
### [Datenbanktransaktionseinheiten (DTUs)](sql-database-what-is-a-dtu.md)
### [DTU-Benchmark – Übersicht](sql-database-benchmark-overview.md)
### [Verwaltungstools](sql-database-manage-overview.md)
## Aspekte und Einschränkungen
### [SQL-Datenbank im Vergleich mit SQL auf einem virtuellen Computer](sql-database-paas-vs-sql-server-iaas.md)
### [T-SQL-Unterschiede](sql-database-transact-sql-information.md)
### [Ressourceneinschränkungen](sql-database-resource-limits.md)
### [Allgemeine Einschränkungen](sql-database-general-limitations.md)
### [Sicherheitsrichtlinien](sql-database-security-guidelines.md)

## Vorteile
### [Lernt und passt sich an](sql-database-learn-and-adapt.md)
### [Skalierung erfolgt während der Ausführung](sql-database-scale-on-the-fly.md)
### [Erstellt mehrinstanzenfähige Apps](sql-database-build-multi-tenant-apps.md)
### [Sichert und schützt](sql-database-helps-secures-and-protects.md)
### [Funktioniert in Ihrer Umgebung](sql-database-works-in-your-environment.md)

## Szenarien

### Server, Pools, Datenbanken und Firewalls
#### [Wann sollte ein Pool für elastische Datenbanken verwendet werden?](sql-database-elastic-pool-guidance.md)
#### [Pools für elastische Datenbanken](sql-database-elastic-pool.md)
#### [Automation](sql-database-manage-automation.md)

### Horizontal hochskalierte Datenbanken
#### [Übersicht](sql-database-elastic-scale-introduction.md)
#### [Erstellen skalierbarer Clouddatenbanken](sql-database-elastic-database-client-library.md)
#### [Datenbankübergreifende Aufträge](sql-database-elastic-jobs-overview.md)
#### [Tools für elastische Datenbanken – Glossar](sql-database-elastic-scale-glossary.md)
#### [Häufig gestellte Fragen](sql-database-elastic-scale-faq.md)

### Zugriff und Berechtigungen
#### [Übersicht](sql-database-security.md)
#### [Azure Security Center für Azure SQL-Datenbank](../security-center/security-center-sql-database.md?toc=%2fazure%2fsql-database%2ftoc.json)
#### [SQL Security Center](https://msdn.microsoft.com/library/azure/bb510589)

### Geschäftskontinuität
#### [Übersicht](sql-database-business-continuity.md)
#### [Datenbanksicherungen](sql-database-automated-backups.md)
#### [Datenbankwiederherstellung mithilfe von Sicherungen](sql-database-recovery-using-backups.md)
#### [Authentifizierungsanforderungen für die Notfallwiederherstellung](sql-database-geo-replication-security-config.md)
#### [Entwurfsszenarien für Geschäftskontinuität](sql-database-designing-cloud-solutions-for-disaster-recovery.md)
#### [Strategien für die Notfallwiederherstellung mit elastischen Pools](sql-database-disaster-recovery-strategies-for-applications-with-elastic-pool.md)
#### [Parallele Upgrades](sql-database-manage-application-rolling-upgrade.md)

### [Datenbankentwicklung](sql-database-develop-overview.md)
### [Migrieren von SQL Server-Datenbanken](sql-database-cloud-migrate.md)

## Kundenimplementierungen
### [Daxko/CSI Software](sql-database-implementation-daxko.md)
### [GEP](sql-database-implementation-gep.md)
### [SnelStart](sql-database-implementation-snelstart.md)
### [Umbraco](sql-database-implementation-umbraco.md)


# Erste Schritte
## [Schnellstarts für Lösungen](sql-database-solution-quick-starts.md)
## Erstellen einer SQL-Datenbank
### [Azure-Portal](sql-database-get-started.md)
### [C#](sql-database-get-started-csharp.md)
### [PowerShell](sql-database-get-started-powershell.md)
## Erstellen und Verwalten von horizontal hochskalierten Datenbanken
### [Elastische Skalierung](sql-database-elastic-scale-get-started.md)
### [Elastische Aufträge](sql-database-elastic-jobs-getting-started.md)
### [Datenbankübergreifende Berichte](sql-database-elastic-query-getting-started.md)
### [Datenbankübergreifende Abfragen](sql-database-elastic-query-getting-started-vertical.md)
## Überwachen und Optimieren von Datenbanken
### [SQL-Datenbankratgeber – Übersicht](sql-database-advisor.md)
### [Einblicke in Workloads im Azure-Portal](sql-database-performance.md)
## [Erstellen und Verwalten von Zugriff und Berechtigungen](sql-database-get-started-security.md)
## [In-Memory-Optimierung](sql-database-in-memory.md)
## [Datensynchronisierung](sql-database-get-started-sql-data-sync.md)

# Anleitung

## Erstellen und Verwalten
### Server und Datenbanken
#### [Einzeldatenbanken](sql-database-manage-portal.md)
#### [Azure-Portal](sql-database-get-started.md)
#### [C#](sql-database-get-started-csharp.md)
#### [PowerShell](sql-database-manage-powershell.md)
#### [SQL Server Management Studio](sql-database-manage-azure-ssms.md)
### Elastische Datenbankpools
#### [Azure-Portal](sql-database-elastic-pool-create-portal.md)
#### [PowerShell](sql-database-elastic-pool-create-powershell.md)
#### [C#](sql-database-elastic-pool-create-csharp.md)
#### [T-SQL](sql-database-elastic-pool-manage-tsql.md)
### Sharddatenbanken
#### [Verwenden des Shard-Zuordnungs-Managers](sql-database-elastic-scale-shard-map-management.md)
#### [Split-Merge-Sicherheitskonfiguration](sql-database-elastic-scale-split-merge-security-configuration.md)
#### [Verwenden von Dapper](sql-database-elastic-scale-working-with-dapper.md)
#### [Verwenden von Entity Framework](sql-database-elastic-scale-use-entity-framework-applications-visual-studio.md)
#### [Datenabhängiges Routing](sql-database-elastic-scale-data-dependent-routing.md)
#### [Sicherheit auf Zeilenebene bei Mehrinstanzenfähigkeit](sql-database-elastic-tools-multi-tenant-row-level-security.md)
#### [Verwalten von Anmeldeinformationen](sql-database-elastic-scale-manage-credentials.md)
#### [Bereitstellen eines Split-Merge-Diensts](sql-database-elastic-scale-configure-deploy-split-and-merge.md)
#### [Hinzufügen eines Shards](sql-database-elastic-scale-add-a-shard.md)
#### [Verwenden der RecoveryManager-Klasse zur Behebung von Problemen mit der Shard-Zuordnung](sql-database-elastic-database-recovery-manager.md)
###  Authentifizierung
#### [Azure AD-Authentifizierung](sql-database-aad-authentication.md)
#### [Multi-Factor Authentication](sql-database-ssms-mfa-authentication.md)
### Firewallregeln
#### [Azure-Portal](sql-database-configure-firewall-settings.md)
#### [PowerShell](sql-database-configure-firewall-settings-powershell.md)
#### [REST-API](sql-database-configure-firewall-settings-rest.md)
#### [T-SQL](sql-database-configure-firewall-settings-tsql.md)
### Aufträge
#### [Dienstinstallation](sql-database-elastic-jobs-service-installation.md)
#### [Azure-Portal](sql-database-elastic-jobs-create-and-manage.md)
#### [PowerShell](sql-database-elastic-jobs-powershell.md)
#### [Upgraden der Clientbibliothek](sql-database-elastic-scale-upgrade-client-library.md)
### [Anmeldungen](sql-database-manage-logins.md)

## Entwickeln
### [Übersicht](https://msdn.microsoft.com/library/mt763826.aspx)
### [Verbindungsbibliotheken](sql-database-libraries.md)
### Szenarien
#### [Mehrinstanzenfähige SaaS-Anwendungen](sql-database-design-patterns-multi-tenancy-saas-applications.md)
#### [JSON-Daten](sql-database-json-features.md)
#### [Temporäre Tabellen](sql-database-temporal-tables.md)
#### [Aufbewahrungsrichtlinien](sql-database-temporal-tables-retention-policy.md)
### Herstellen einer Verbindung für eine Anwendung
#### [.NET](sql-database-develop-dotnet-simple.md)
#### [Java](sql-database-develop-java-simple.md)
#### [Node.js](sql-database-develop-nodejs-simple.md)
#### [PHP](sql-database-develop-php-simple.md)
#### [Python](sql-database-develop-python-simple.md)
#### [Ruby](sql-database-develop-ruby-simple.md)
#### [Excel](sql-database-connect-excel.md)
#### [Visual Studio](sql-database-connect-query.md)

### [Erstellen von Datenbanken](sql-database-get-started-powershell.md)
### Verwalten von elastischen Pools
#### [PowerShell](sql-database-elastic-pool-manage-powershell.md)
#### [C#](sql-database-elastic-pool-manage-csharp.md)
### [Abrufen der erforderlichen Werte für die Authentifizierung einer Anwendung](sql-database-client-id-keys.md)
### [Verwenden von anderen Ports als 1433 für ADO.NET 4.5](sql-database-develop-direct-route-ports-adonet-v12.md)
### [Arbeiten mit Fehlermeldungen](sql-database-develop-error-messages.md)
### [Verwenden der Batchverarbeitung](sql-database-use-batching-to-improve-performance.md)

### Referenz
#### [.NET (Konzepte)](https://msdn.microsoft.com/library/kb9s9ks0.aspx)
#### [.NET (API-Referenz)](https://msdn.microsoft.com/library/system.data.sqlclient.aspx)
#### [Entity Framework](https://www.nuget.org/packages/EntityFramework/)
#### [Azure SDK (Download)](https://www.visualstudio.com/vs/azure-tools/)
#### [Azure SDK (Dokumentation)](../dotnet-sdk.md)
#### [PowerShell-Cmdlets für die Dienstverwaltung](https://msdn.microsoft.com/library/azure/dn546723.aspx)
#### REST
##### [REST (Ressourcenverwaltung)](https://msdn.microsoft.com/library/azure/mt420159)
##### [REST (Dienstverwaltung)](https://msdn.microsoft.com/library/azure/dn505719.aspx)

## Erkennen von Bedrohungen
### [Bedrohungserkennung](sql-database-threat-detection-get-started.md)
### [Firewall](sql-database-firewall-configure.md)

## Verschlüsseln von Daten
### [Always Encrypted (Übersicht)](sql-database-always-encrypted.md)
### [Always Encrypted (Azure Key Vault)](sql-database-always-encrypted-azure-key-vault.md)
### [Transparent Data Encryption](https://msdn.microsoft.com/library/azure/dn948096)
### [Spaltenverschlüsselung](https://msdn.microsoft.com/library/azure/ms179331)

## Maskieren von Daten
### [Dynamische Datenmaskierung](sql-database-dynamic-data-masking-get-started-portal.md)
### [Kompatible Clients](sql-database-auditing-and-dynamic-data-masking-downlevel-clients.md)

## Migrieren
### Ermitteln der Kompatibilität
#### [SQL-Pakethilfsprogramm](sql-database-cloud-migrate-determine-compatibility-sqlpackage.md)
#### [SQL Server Management Studio](sql-database-cloud-migrate-determine-compatibility-ssms.md)
### Beheben von Kompatibilitätsproblemen
#### [SQL Server Data Tools](sql-database-cloud-migrate-fix-compatibility-issues-ssdt.md)
#### [SQL Server Management Studio](sql-database-cloud-migrate-fix-compatibility-issues-ssms.md)
#### [SQL Azure-Migrations-Assistent](sql-database-cloud-migrate-fix-compatibility-issues.md)
### [Verwenden des Migrations-Assistenten von SQL Server Management Studio](sql-database-cloud-migrate-compatible-using-ssms-migration-wizard.md)
### [Verwenden der Transaktionsreplikation](sql-database-cloud-migrate-compatible-using-transactional-replication.md)
### [Migrieren von vorhandenen horizontal hochskalierten Datenbanken zur horizontalen Hochskalierung](sql-database-elastic-convert-to-use-elastic-tools.md)

## Überwachen und Optimieren
### [Einzeldatenbanken](sql-database-performance-guidance.md)
### [Query Performance Insight](sql-database-query-performance.md)
### [SQL-Datenbankratgeber](sql-database-advisor-portal.md)
### [Datenbankleistung](sql-database-single-database-monitor.md)
### [DMVs](sql-database-monitoring-with-dmvs.md)
### [Kompatibilitätsgrade](sql-database-compatibility-level-query-performance-130.md)
### [Ereignisüberwachung](sql-database-auditing-get-started.md)
### [Leistungsindikatoren für den Shardzuordnungs-Manager](sql-database-elastic-database-perf-counters.md)
### [Tipps zur Leistungsoptimierung](sql-database-troubleshoot-performance.md)
### Ändern von Tarifen und Leistungsstufen
#### [Azure-Portal](sql-database-scale-up.md)
#### [PowerShell](sql-database-scale-up-powershell.md)
### In-Memory OLTP
#### [Einführen von In-Memory OLTP](sql-database-in-memory-oltp-migration.md)
#### [Überwachen des In-Memory-OLTP-Speichers](sql-database-in-memory-oltp-monitoring.md)
### Abfragespeicher
#### [Überwachen der Leistung mit dem Abfragespeicher](https://msdn.microsoft.com/library/dn817826.aspx)
#### [Verwendungsszenarien für den Abfragespeicher](https://msdn.microsoft.com/library/mt614796.aspx)
#### [Verwenden des Abfragespeichers](sql-database-operate-query-store.md)
### Erweiterte Ereignisse
#### [Erweiterte Ereignisse](sql-database-xevent-db-diff-from-svr.md)
#### [Code des Ereignisdateiziels](sql-database-xevent-code-event-file.md)
#### [Code des Ringpufferziels](sql-database-xevent-code-ring-buffer.md)

## Verschieben von Daten
### [Kopieren einer SQL-Datenbank](sql-database-copy.md)
#### [Azure-Portal](sql-database-copy-portal.md)
#### [PowerShell](sql-database-copy-powershell.md)
#### [T-SQL](sql-database-copy-transact-sql.md)
### Exportieren der Datenbank in eine BACPAC-Datei
#### [Azure-Portal](sql-database-export.md)
#### [SQL Server Management Studio](sql-database-cloud-migrate-compatible-export-bacpac-ssms.md)
#### [SQL-Pakethilfsprogramm](sql-database-cloud-migrate-compatible-export-bacpac-sqlpackage.md)
#### [PowerShell](sql-database-export-powershell.md)
### Importieren der Datenbank aus einer BACPAC-Datei
#### [Azure-Portal](sql-database-import.md)
#### [PowerShell](sql-database-import-powershell.md)
#### [SQL Server Management Studio](sql-database-cloud-migrate-compatible-import-bacpac-ssms.md)
#### [SQL-Pakethilfsprogramm](sql-database-cloud-migrate-compatible-import-bacpac-sqlpackage.md)
### [Laden aus CSV-Datei mit BCP](sql-database-load-from-csv-with-bcp.md)
### [Skalierung mit dem Split-Merge-Tool für elastische Datenbanken](sql-database-elastic-scale-overview-split-and-merge.md)

## Abfrage
### [SQL Server Management Studio](sql-database-connect-query-ssms.md)
### [Abfragen von mehreren Shards](sql-database-elastic-scale-multishard-querying.md)
### Datenbankübergreifende Abfragen
#### [Übersicht](sql-database-elastic-query-overview.md)
#### [Datenbankübergreifende Abfragen mit verschiedenen Schemas](sql-database-elastic-query-vertical-partitioning.md)
#### [Datenbankübergreifende Berichte](sql-database-elastic-query-horizontal-partitioning.md)
#### [Übersicht über elastische Datenbanktransaktionen mit Azure SQL-Datenbank](sql-database-elastic-transactions-overview.md)

## Wiederherstellen
### Wiederherstellen einer gelöschten Datenbank
#### [Azure-Portal](sql-database-restore-deleted-database-portal.md)
#### [PowerShell](sql-database-restore-deleted-database-powershell.md)
### Point-in-Time-Wiederherstellung
#### [Azure-Portal](sql-database-point-in-time-restore-portal.md)
#### [PowerShell](sql-database-point-in-time-restore-powershell.md)
### Geowiederherstellung
#### [Azure-Portal](sql-database-geo-restore-portal.md)
#### [PowerShell](sql-database-geo-restore-powershell.md)
#### [Einzelne Tabelle](sql-database-cloud-migrate-restore-single-table-azure-backup.md)
### [Wiederherstellen nach dem Ausfall eines Rechenzentrums](sql-database-disaster-recovery.md)
### [Durchführen von Übungen zur Notfallwiederherstellung](sql-database-disaster-recovery-drills.md)

## Replikat
### [Aktive Georeplikation – Übersicht](sql-database-geo-replication-overview.md)
### Konfigurieren
#### [PowerShell](sql-database-geo-replication-powershell.md)
#### [T-SQL](sql-database-geo-replication-transact-sql.md)
### Failover
#### [Azure-Portal](sql-database-geo-replication-failover-portal.md)
#### [PowerShell](sql-database-geo-replication-failover-powershell.md)
#### [T-SQL](sql-database-geo-replication-failover-transact-sql.md)

## Problembehandlung
### [Verbindungsprobleme](sql-database-troubleshoot-common-connection-issues.md)
### [Vorübergehender Verbindungsfehler](sql-database-troubleshoot-connection.md)
### [Diagnostizieren und Verhindern](sql-database-connectivity-issues.md)
### [Berechtigungen](sql-database-troubleshoot-permissions.md)
### [Verschieben von Datenbanken](sql-database-troubleshoot-moving-data.md)

# Referenz
## [PowerShell](/powershell/azureps-cmdlets-docs/)
## [PowerShell – klassisch](/powershell/servicemanagement/)
## [Java](/java/api/)
## [.NET](/dotnet/api/)
## [T-SQL](https://msdn.microsoft.com/library/azure/bb510741.aspx)
## [Azure SQL-Datenbank-Cmdlets](https://msdn.microsoft.com/library/azure/mt574084(v=azure.300\).aspx)
## [SQL Server-Cmdlets](https://msdn.microsoft.com/library/mt740629.aspx)
## [REST](/rest/api/sql/)

## SQL-Datenbank-Verwaltungsbibliothek
### [SQL-Datenbank-Verwaltungsbibliothek – Referenz](https://msdn.microsoft.com/library/azure/mt349017.aspx)
### [Abrufen des Pakets mit der SQL-Datenbank-Verwaltungsbibliothek](https://www.nuget.org/packages/Microsoft.Azure.Management.Sql)
## [SQL Server-Treiber](https://msdn.microsoft.com/library/mt654049.aspx)
### [ADO.NET](https://msdn.microsoft.com/library/mt657768.aspx)
### [JDBC](https://msdn.microsoft.com/library/mt484311.aspx)
### [Node.js](https://msdn.microsoft.com/library/mt652093.aspx)
### [ODBC](https://msdn.microsoft.com/library/mt654048.aspx)
### [PHP](https://msdn.microsoft.com/library/dn865013.aspx)
### [Python](https://msdn.microsoft.com/library/mt652092.aspx)
### [Ruby](https://msdn.microsoft.com/library/mt691981.aspx)

# Ressourcen
## [Preise](https://azure.microsoft.com/pricing/details/sql-database/)
## [Dienstupdates](https://azure.microsoft.com/updates/?service=sql-database)
## [SQL Server-Tools](https://msdn.microsoft.com/library/mt238365.aspx)
## [SQL Server Management Studio (SSMS)](https://msdn.microsoft.com/library/mt238290.aspx)
## [SQL Server Data Tools (SSDT)](https://msdn.microsoft.com/library/mt204009.aspx)
## [BCP](https://msdn.microsoft.com/library/ms162802.aspx)
## [SQLCMD](https://msdn.microsoft.com/library/ms162773.aspx)
## [SqlPackage](https://msdn.microsoft.com/hh550080.aspx)
## [Forum](https://social.msdn.microsoft.com/Forums/home?forum=ssdsgetstarted)
## [Videos](https://azure.microsoft.com/documentation/videos/index/?services=sql-database)


<!--HONumber=Nov16_HO2-->


