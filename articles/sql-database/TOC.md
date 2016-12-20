# Übersicht
## [Was ist SQL-Datenbank?](sql-database-technical-overview.md)
### [Dienstebenen](sql-database-service-tiers.md)
### [Datenbanktransaktionseinheiten (DTUs)](sql-database-what-is-a-dtu.md)
### [DTU-Benchmark – Übersicht](sql-database-benchmark-overview.md)
### [Ressourceneinschränkungen](sql-database-resource-limits.md)
### [Funktionen](sql-database-features.md)
### [SQL-Datenbank – Häufig gestellte Fragen](sql-database-faq.md)
## Vorteile
### [Lernt und passt sich an](sql-database-learn-and-adapt.md)
### [Skalierung erfolgt während der Ausführung](sql-database-scale-on-the-fly.md)
### [Erstellt mehrinstanzenfähige Apps](sql-database-build-multi-tenant-apps.md)
### [Sichert und schützt](sql-database-helps-secures-and-protects.md)
### [Funktioniert in Ihrer Umgebung](sql-database-works-in-your-environment.md)
## Vergleiche
### [SQL-Datenbank im Vergleich mit SQL auf einem virtuellen Computer](sql-database-paas-vs-sql-server-iaas.md)
### [T-SQL-Unterschiede](sql-database-transact-sql-information.md)
### [SQL im Vergleich mit NoSQL](../documentdb/documentdb-nosql-vs-sql.md)
## [SQL-Datenbanktools](sql-database-manage-overview.md)
## [SQL-Datenbanktutorials](sql-database-explore-tutorials.md)
## [Schnellstarts für Lösungen](sql-database-solution-quick-starts.md)
## Sicherheit
### [Azure Security Center für Azure SQL-Datenbank](https://azure.microsoft.com/documentation/articles/security-center-sql-database/)
### [SQL Security Center](https://msdn.microsoft.com/library/azure/bb510589)

# Erste Schritte
## Datenbanken und Server
### Einzeldatenbanken
#### Lernen
##### [Tutorial im Azure-Portal: Erstellen einer Azure SQL-Datenbank mithilfe des Azure-Portals](sql-database-get-started.md)
#### Empfohlene Vorgehensweise
##### [Tutorial im Azure-Portal: Erstellen einer Azure SQL-Datenbank mithilfe des Azure-Portals](sql-database-get-started.md)
##### [Tutorial im Azure-Portal: Erstellen einer Azure SQL-Datenbank mithilfe von PowerShell](sql-database-get-started-powershell.md)
##### [Tutorial im Azure-Portal: Erstellen einer Azure SQL-Datenbank mithilfe von C#](sql-database-get-started-csharp.md)
### Elastische Datenbankpools
#### Lernen
##### [Pools für elastische Datenbanken](sql-database-elastic-pool.md)
##### [Wann sollte ein Pool für elastische Datenbanken verwendet werden?](sql-database-elastic-pool-guidance.md)
##### [Elastische Pools – Preisgestaltung](sql-database-elastic-pool-price.md)
#### Empfohlene Vorgehensweise
##### [Erstellen mithilfe des Azure-Portals](sql-database-elastic-pool-create-portal.md)
##### [Erstellen mithilfe von PowerShell](sql-database-elastic-pool-create-powershell.md)
##### [Erstellen mithilfe von C#](sql-database-elastic-pool-create-csharp.md)
### Horizontales Skalieren
#### Lernen
##### [Horizontales Skalieren](sql-database-elastic-scale-introduction.md)
##### [Elastische Skalierung](sql-database-elastic-scale-get-started.md)
##### [Erstellen skalierbarer Clouddatenbanken](sql-database-elastic-database-client-library.md)
##### [Datenbankübergreifende Aufträge](sql-database-elastic-jobs-overview.md)
##### [Elastische Tools – häufig gestellte Fragen](sql-database-elastic-scale-faq.md)
##### [Tools für elastische Datenbanken – Glossar](sql-database-elastic-scale-glossary.md)
##### [Skalierung mit dem Split-Merge-Tool für elastische Datenbanken](sql-database-elastic-scale-overview-split-and-merge.md)
#### Empfohlene Vorgehensweise
##### [Elastische Aufträge](sql-database-elastic-jobs-getting-started.md)
##### [Datenbankübergreifende Berichte](sql-database-elastic-query-getting-started.md)
##### [Datenbankübergreifende Abfragen](sql-database-elastic-query-getting-started-vertical.md)
##### [Deinstallieren von elastischen Aufträgen](sql-database-elastic-jobs-uninstall.md)
## Migrieren und Verschieben von Daten
### Lernen
#### [Migrieren einer Datenbank](sql-database-cloud-migrate.md)
#### [Transaktionsreplikation](sql-database-cloud-migrate-compatible-using-transactional-replication.md)
#### [Datensynchronisierung](sql-database-get-started-sql-data-sync.md)
#### [Kopieren einer SQL-Datenbank](sql-database-copy.md)
### Empfohlene Vorgehensweise
#### Ermitteln der Datenbankkompatibilität
##### [SQL-Pakethilfsprogramm](sql-database-cloud-migrate-determine-compatibility-sqlpackage.md)
##### [SQL Server Management Studio](sql-database-cloud-migrate-determine-compatibility-ssms.md)
#### Beheben von Problemen mit der Datenbankkompatibilität
##### [SQL Server Data Tools](sql-database-cloud-migrate-fix-compatibility-issues-ssdt.md)
##### [SQL Server Management Studio](sql-database-cloud-migrate-fix-compatibility-issues-ssms.md)
##### [SQL Azure-Migrations-Assistent](sql-database-cloud-migrate-fix-compatibility-issues.md)
##### [Migrations-Assistent von SQL Server Management Studio](sql-database-cloud-migrate-compatible-using-ssms-migration-wizard.md)
#### Kopieren einer Datenbank
##### [Kopieren mithilfe des Azure-Portals](sql-database-copy-portal.md)
##### [Kopieren mithilfe von PowerShell](sql-database-copy-powershell.md)
##### [Kopieren mithilfe von T-SQL](sql-database-copy-transact-sql.md)
#### Exportieren der Datenbank in eine BACPAC-Datei
##### [Azure-Portal](sql-database-export.md)
##### [SQL Server Management Studio](sql-database-cloud-migrate-compatible-export-bacpac-ssms.md)
##### [SQL-Pakethilfsprogramm](sql-database-cloud-migrate-compatible-export-bacpac-sqlpackage.md)
##### [PowerShell](sql-database-export-powershell.md)
#### Importieren der Datenbank aus einer BACPAC-Datei
##### [Azure-Portal](sql-database-import.md)
##### [PowerShell](sql-database-import-powershell.md)
##### [SQL Server Management Studio](sql-database-cloud-migrate-compatible-import-bacpac-ssms.md)
##### [SQL-Pakethilfsprogramm](sql-database-cloud-migrate-compatible-import-bacpac-sqlpackage.md)
#### [Laden aus CSV-Datei mit BCP](sql-database-load-from-csv-with-bcp.md)
## Authentifizieren und Gewähren von Zugriff
### Lernen
#### [Übersicht](sql-database-security.md)
#### [Sicherheitsrichtlinien](sql-database-security-guidelines.md)
#### [Verwalten von Anmeldungen](sql-database-manage-logins.md)
### Empfohlene Vorgehensweise
### [Tutorial: SQL-Authentifizierung und Zugriff](sql-database-get-started-security.md)
## Sichern und Schützen von Daten
### Lernen
#### Überwachung
##### [Überwachung](sql-database-auditing-get-started.md)
##### [Unterstützung für kompatible Clients und IP-Endpunkt-Änderungen für die Überwachung](sql-database-auditing-and-dynamic-data-masking-downlevel-clients.md)
#### [Bedrohungserkennung](sql-database-threat-detection-get-started.md)
#### Verschlüsseln von Daten
##### [Azure-Schlüsseltresor](sql-database-always-encrypted-azure-key-vault.md)
##### [Windows-Zertifikatspeicher](sql-database-always-encrypted.md)
##### [Transparent Data Encryption](https://msdn.microsoft.com/library/azure/dn948096)
##### [Spaltenverschlüsselung](https://msdn.microsoft.com/library/azure/ms179331)
#### Maskieren von Daten
##### Dynamische Datenmaskierung
###### [Azure-Portal](sql-database-dynamic-data-masking-get-started.md)
### Empfohlene Vorgehensweise
#### [Dynamische Datenmaskierung mithilfe des Azure-Portals](sql-database-dynamic-data-masking-get-started.md)
## Geschäftskontinuität
### Lernen
#### [Übersicht](sql-database-business-continuity.md)
#### [Datenbanksicherungen](sql-database-automated-backups.md)
#### [Langfristige Aufbewahrung](sql-database-long-term-retention.md)
#### [Datenbankwiederherstellung mithilfe von Sicherungen](sql-database-recovery-using-backups.md)
#### [Authentifizierungsanforderungen für die Notfallwiederherstellung](sql-database-geo-replication-security-config.md)
#### [Entwurfsszenarien für Geschäftskontinuität](sql-database-designing-cloud-solutions-for-disaster-recovery.md)
#### [Strategien für die Notfallwiederherstellung mit elastischen Pools](sql-database-disaster-recovery-strategies-for-applications-with-elastic-pool.md)
#### [Parallele Upgrades](sql-database-manage-application-rolling-upgrade.md)
### Empfohlene Vorgehensweise
#### Wiederherstellen einer gelöschten Datenbank
##### [Azure-Portal](sql-database-restore-deleted-database-portal.md)
##### [PowerShell](sql-database-restore-deleted-database-powershell.md)
#### Point-in-Time-Wiederherstellung
##### [Azure-Portal](sql-database-point-in-time-restore-portal.md)
##### [PowerShell](sql-database-point-in-time-restore-powershell.md)
#### Geowiederherstellung von Datenbanken
##### [Azure-Portal](sql-database-geo-restore-portal.md)
##### [PowerShell](sql-database-geo-restore-powershell.md)
#### [Einzelne Tabelle](sql-database-cloud-migrate-restore-single-table-azure-backup.md)
#### [Wiederherstellen nach dem Ausfall eines Rechenzentrums](sql-database-disaster-recovery.md)
#### [Durchführen von Übungen zur Notfallwiederherstellung](sql-database-disaster-recovery-drills.md)
### Replizieren von Datenbanken
#### [Aktive Georeplikation – Übersicht](sql-database-geo-replication-overview.md)
#### Konfigurieren der aktiven Georeplikation
##### [Azure-Portal](sql-database-geo-replication-portal.md)
##### [PowerShell](sql-database-geo-replication-powershell.md)
##### [T-SQL](sql-database-geo-replication-transact-sql.md)
#### Failover mit der aktiven Georeplikation
##### [Azure-Portal](sql-database-geo-replication-failover-portal.md)
##### [PowerShell](sql-database-geo-replication-failover-powershell.md)
##### [T-SQL](sql-database-geo-replication-failover-transact-sql.md)
## App-Entwicklung
### Lernen
#### [Datenbankanwendungsentwicklung – Übersicht](sql-database-develop-overview.md)
#### [Verbindungsbibliotheken](sql-database-libraries.md)
#### [Mehrinstanzenfähige SaaS-Anwendungen](sql-database-design-patterns-multi-tenancy-saas-applications.md)
#### [Verwenden von anderen Ports als 1433 für ADO.NET 4.5](sql-database-develop-direct-route-ports-adonet-v12.md)
#### [Abrufen der erforderlichen Werte für die Authentifizierung einer Anwendung](sql-database-client-id-keys.md)
### Empfohlene Vorgehensweise
#### Herstellen einer Verbindung für eine Anwendung
##### [.NET](sql-database-develop-dotnet-simple.md)
##### [C und C++](sql-database-develop-cplusplus-simple.md)
##### [Java](sql-database-develop-java-simple.md)
##### [Node.js](sql-database-develop-nodejs-simple.md)
##### [PHP](sql-database-develop-php-simple.md)
##### [Python](sql-database-develop-python-simple.md)
##### [Ruby](sql-database-develop-ruby-simple.md)
##### [Excel](sql-database-connect-excel.md)
#### [Verbinden mit Visual Studio](sql-database-connect-query.md)
#### [Erstellen einer Clientanwendung](https://www.microsoft.com/sql-server/developer-get-started)
#### [Arbeiten mit Fehlermeldungen](sql-database-develop-error-messages.md)
### Kundenimplementierungen von Software-as-a-Service
#### [Daxko/CSI Software](sql-database-implementation-daxko.md)
#### [GEP](sql-database-implementation-gep.md)
#### [SnelStart](sql-database-implementation-snelstart.md)
#### [Umbraco](sql-database-implementation-umbraco.md)
## Datenbankentwicklung
### Lernen
#### Temporäre Tabellen
##### [Temporäre Tabellen](sql-database-temporal-tables.md)
##### [Aufbewahrungsrichtlinien](sql-database-temporal-tables-retention-policy.md)
#### [JSON-Daten](sql-database-json-features.md)
#### [In-Memory](sql-database-in-memory.md)
### Empfohlene Vorgehensweise
#### [SQL Server-Entwicklung](https://msdn.microsoft.com/library/ms179422.aspx)
## Überwachung und Optimierung
### Lernen
#### [Einzeldatenbanken](sql-database-single-database-monitor.md)
#### [SQL-Datenbankratgeber – Übersicht](sql-database-advisor.md)
#### [Leitfaden für Einzeldatenbanken](sql-database-performance-guidance.md)
#### [Einblicke in Workloads im Azure-Portal](sql-database-performance.md)
#### [Verwenden der Batchverarbeitung](sql-database-use-batching-to-improve-performance.md)
## SQL-Datenbank V11
### [Einstellung von Web Edition und Business Edition](sql-database-web-business-sunset-faq.md)
### [Dienstebenenratgeber](sql-database-service-tier-advisor.md)
### [Bewertungstool für elastische Pools](sql-database-elastic-pool-database-assessment-powershell.md)
### [Upgrade auf V12](sql-database-v12-plan-prepare-upgrade.md)
#### [Upgrade mithilfe des Azure-Portals](sql-database-upgrade-server-portal.md)
#### [Upgrade mithilfe von PowerShell](sql-database-upgrade-server-powershell.md)
#### [Tarifempfehlungen](sql-database-service-tier-advisor.md)

# Anleitung
## Erstellen und Verwalten
### Server und Datenbanken
#### [Einzeldatenbanken](sql-database-manage-portal.md)
#### [Azure-Portal](sql-database-get-started.md)
#### [C#](sql-database-get-started-csharp.md)
#### [PowerShell](sql-database-manage-powershell.md)
#### [SQL Server Management Studio](sql-database-manage-azure-ssms.md)
### Elastische Datenbankpools
#### [Erstellen mithilfe des Azure-Portals](sql-database-elastic-pool-create-portal.md)
#### [Erstellen mithilfe von PowerShell](sql-database-elastic-pool-create-powershell.md)
#### [Erstellen mithilfe von C#](sql-database-elastic-pool-create-csharp.md)
#### [Verwalten mit dem Azure-Portal](sql-database-elastic-pool-manage-portal.md)
#### [Verwalten mit PowerShell](sql-database-elastic-pool-manage-powershell.md)
#### [Verwalten mit C#](sql-database-elastic-pool-manage-csharp.md)
#### [Verwalten mit T-SQL](sql-database-elastic-pool-manage-tsql.md)
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
### Aufträge und Automation
#### [Dienstinstallation](sql-database-elastic-jobs-service-installation.md)
#### [Erstellen und Verwalten von elastischen Aufträgen im Azure-Portal](sql-database-elastic-jobs-create-and-manage.md)
#### [Erstellen und Verwalten von elastischen Aufträgen mithilfe von PowerShell](sql-database-elastic-jobs-powershell.md)
#### [Upgraden der Clientbibliothek](sql-database-elastic-scale-upgrade-client-library.md)
#### [Verwalten von SQL-Datenbanken mit dem Azure Automation-Dienst](sql-database-manage-automation.md)
### [Firewall](sql-database-firewall-configure.md)
## Verschlüsseln von Daten
### [Always Encrypted (Übersicht)](sql-database-always-encrypted.md)
### [Transparent Data Encryption](https://msdn.microsoft.com/library/azure/dn948096)
### [Spaltenverschlüsselung](https://msdn.microsoft.com/library/azure/ms179331)
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
### [Query Performance Insight](sql-database-query-performance.md)
### [SQL-Datenbankratgeber](sql-database-advisor-portal.md)
### [DMVs](sql-database-monitoring-with-dmvs.md)
### [Kompatibilitätsgrade](sql-database-compatibility-level-query-performance-130.md)
### [Leistungsindikatoren für den Shardzuordnungs-Manager](sql-database-elastic-database-perf-counters.md)
### [Tipps zur Leistungsoptimierung](sql-database-troubleshoot-performance.md)
### Ändern von Tarifen und Leistungsstufen
#### [Verwenden des Azure-Portals](sql-database-scale-up.md)
#### [Verwenden von PowerShell](sql-database-scale-up-powershell.md)
### [Erstellen von Warnungen](sql-database-insights-alerts-portal.md)
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
## [PowerShell](/powershell/resourcemanager/azurerm.sql/v2.3.0/azurerm.sql)
## [PowerShell (Elastische Datenbank)](/powershell/elasticdatabasejobs/v0.8.33/elasticdatabasejobs)
## [.NET](https://docs.microsoft.com)
## [Java](/java/api/com.microsoft.azure.management.sql)
## [Node.js](https://msdn.microsoft.com/library/mt652093.aspx)
## [Python](https://msdn.microsoft.com/library/mt652092.aspx)
## [Ruby](https://msdn.microsoft.com/library/mt691981.aspx)
## [PHP](https://msdn.microsoft.com/library/dn865013.aspx)
## [T-SQL](https://msdn.microsoft.com/library/azure/bb510741.aspx)
## [REST](/rest/api/sql/)

# Verwandte Themen
## SQL-Datenbank-Verwaltungsbibliothek
### [Abrufen des Pakets mit der SQL-Datenbank-Verwaltungsbibliothek](https://www.nuget.org/packages/Microsoft.Azure.Management.Sql)
## [SQL Server-Treiber](https://msdn.microsoft.com/library/mt654049.aspx)
### [ADO.NET](https://msdn.microsoft.com/library/mt657768.aspx)
### [JDBC](https://msdn.microsoft.com/library/mt484311.aspx)

### [ODBC](https://msdn.microsoft.com/library/mt654048.aspx)


# Ressourcen
## [Preise](https://azure.microsoft.com/pricing/details/sql-database/)
## [MSDN-Forum](https://social.msdn.microsoft.com/Forums/azure/home?forum=ssdsgetstarted)
## [Stack Overflow](http://stackoverflow.com/questions/tagged/sql-azure)
## [Videos](https://azure.microsoft.com/documentation/videos/index/?services=sql-database)
## [Dienstupdates](https://azure.microsoft.com/updates/?service=sql-database)
## [SQL Server-Tools](https://msdn.microsoft.com/library/mt238365.aspx)
## [SQL Server Management Studio (SSMS)](https://msdn.microsoft.com/library/mt238290.aspx)
## [SQL Server Data Tools (SSDT)](https://msdn.microsoft.com/library/mt204009.aspx)
## [BCP](https://msdn.microsoft.com/library/ms162802.aspx)
## [SQLCMD](https://msdn.microsoft.com/library/ms162773.aspx)
## [SqlPackage](https://msdn.microsoft.com/hh550080.aspx)


<!--HONumber=Dec16_HO1-->


