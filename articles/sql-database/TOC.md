# Übersicht
## [Was ist SQL Database?](sql-database-technical-overview.md)
## Features
### [Dienstebenen](sql-database-service-tiers.md)
### [Was ist eine DTU?](sql-database-what-is-a-dtu.md)
### [DTU-Benchmark – Übersicht](sql-database-benchmark-overview.md)
### [SQL-Datenbank-Firewall und Firewallregeln](sql-database-firewall-configure.md)
### [Verwaltungstools](sql-database-manage-overview.md)
## Aspekte und Einschränkungen
### [SQL-Datenbank im Vergleich mit SQL auf einem virtuellen Computer](sql-database-paas-vs-sql-server-iaas.md)
### [T-SQL-Unterschiede](sql-database-transact-sql-information.md)
### [Ressourceneinschränkungen](sql-database-resource-limits.md)
### [Allgemeine Einschränkungen](sql-database-general-limitations.md)
## [Preise](https://azure.microsoft.com/pricing/details/sql-database/)
## [Neuerungen](https://azure.microsoft.com/updates/?service=sql-database)
## [SQL-Datenbank – Häufig gestellte Fragen](sql-database-faq.md)

## Vorteile
### [Lernt und passt sich an](sql-database-learn-and-adapt.md)
### [Skalierung erfolgt während der Ausführung](sql-database-scale-on-the-fly.md)
### [Erstellt mehrinstanzenfähige Apps](sql-database-build-multi-tenant-apps.md)
### [Sichert und schützt](sql-database-helps-secures-and-protects.md)
### [Funktioniert in Ihrer Umgebung](sql-database-works-in-your-environment.md)

## Szenarien
### Erstellen und Verwalten von Servern, Pools, Datenbanken und Firewalls
#### Erstellen und Verwalten von Pools für elastische Datenbanken
#### [Wann sollte ein Pool für elastische Datenbanken verwendet werden?](sql-database-elastic-pool-guidance.md)
#### [Sicherheitsrichtlinien](sql-database-security-guidelines.md)
#### [Pools für elastische Datenbanken](sql-database-elastic-pool.md)
#### [Automation](sql-database-manage-automation.md)
#### Ändern von Tarifen und Leistungsebenen
##### [Azure-[Portal](sql-database-scale-up.md)
##### [PowerShell](sql-database-scale-up-powershell.md)
### Erstellen und Verwalten von horizontal hochskalierten Datenbanken
#### [Übersicht](sql-database-elastic-scale-introduction.md)
#### [Erstellen skalierbarer Clouddatenbanken](sql-database-elastic-database-client-library.md)
#### [Horizontales Hochskalieren mit Shard-Zuordnungs-Manager](sql-database-elastic-scale-shard-map-management.md)
#### [Migrieren von vorhandenen Datenbanken zu horizontaler Hochskalierung](sql-database-elastic-convert-to-use-elastic-tools.md)
#### [Verschieben von Daten zwischen horizontal hochskalierten Clouddatenbanken](sql-database-elastic-scale-overview-split-and-merge.md)
#### [Verwenden von Entity Framework](sql-database-elastic-scale-use-entity-framework-applications-visual-studio.md)
#### [Verwenden von Dapper](sql-database-elastic-scale-working-with-dapper.md)
#### [Leistungsindikatoren für den Shardzuordnungs-Manager](sql-database-elastic-database-perf-counters.md)
#### Datenbankübergreifende Aufträge und Abfragen
##### [Datenbankübergreifende Abfragen](sql-database-elastic-query-overview.md)
##### [Datenbankübergreifende Abfragen mit verschiedenen Schemas](sql-database-elastic-query-vertical-partitioning.md)
##### [Datenbankübergreifende Berichte](sql-database-elastic-query-horizontal-partitioning.md)
##### [Datenbankübergreifende Aufträge](sql-database-elastic-jobs-overview.md)
##### [Datenabhängiges Routing](sql-database-elastic-scale-data-dependent-routing.md)
##### [Abfragen von mehreren Shards](sql-database-elastic-scale-multishard-querying.md)
##### [Sicherheit auf Zeilenebene bei Mehrinstanzenfähigkeit](sql-database-elastic-tools-multi-tenant-row-level-security.md)
##### [Übersicht über elastische Datenbanktransaktionen mit Azure SQL-Datenbank](sql-database-elastic-transactions-overview.md)
####[Tools für elastische Datenbanken – Glossar](sql-database-elastic-scale-glossary.md)
#### [Häufig gestellte Fragen](sql-database-elastic-scale-faq.md)
### Erstellen und Verwalten von Zugriff und Berechtigungen
#### [Übersicht](sql-database-security.md)
#### [Sicherheitsrichtlinien](sql-database-security-guidelines.md)
#### [Verwalten von Anmeldungen](sql-database-manage-logins.md)
#### [Azure Security Center für Azure SQL-Datenbank](https://azure.microsoft.com/documentation/articles/security-center-sql-database/)
#### [SQL Security Center](https://msdn.microsoft.com/library/azure/bb510589)
### [Datenbank- und Anwendungsentwicklung](sql-database-develop-overview.md)
### Datenbankmigration
#### [Migrieren einer SQL Server-Datenbank](sql-database-cloud-migrate.md)
### Entwerfen für Geschäftskontinuität
#### [Übersicht](sql-database-business-continuity.md)
#### [Datenbanksicherungen](sql-database-automated-backups.md) 
#### [Datenbankwiederherstellung mithilfe von Sicherungen](sql-database-recovery-using-backups.md)
#### [Wiederherstellen nach dem Ausfall eines Rechenzentrums](sql-database-disaster-recovery.md)
#### [Authentifizierungsanforderungen für die Notfallwiederherstellung](sql-database-geo-replication-security-config.md)
#### [Entwurfsszenarien für Geschäftskontinuität](sql-database-designing-cloud-solutions-for-disaster-recovery.md)
#### [Strategien für die Notfallwiederherstellung mit elastischen Pools](sql-database-disaster-recovery-strategies-for-applications-with-elastic-pool.md)
#### [Parallele Upgrades](sql-database-manage-application-rolling-upgrade.md)
### Überwachen und Optimieren von Datenbanken
#### [Einzeldatenbanken](sql-database-single-database-monitor.md)
#### [Leitfaden für Einzeldatenbanken](sql-database-performance-guidance.md)
#### [Einblicke in Workloads im Azure-Portal](sql-database-performance.md)

## Kundenimplementierungen
### [Daxko/CSI Software](sql-database-implementation-daxko.md)
### [GEP](sql-database-implementation-gep.md)
### [SnelStart](sql-database-implementation-snelstart.md)
### [Umbraco](sql-database-implementation-umbraco.md)

# Erste Schritte
## Erstellen von Servern, Pools, Datenbanken und Firewalls
### [Azure-Portal](sql-database-get-started.md)
### [PowerShell](sql-database-get-started-powershell.md)
### [C#](sql-database-get-started-csharp.md)
## Abfragen von Daten
### [SQL Server Management Studio](sql-database-connect-query-ssms.md)
## Verwalten von Servern, Pools, Datenbanken und Firewalls
### [Azure-Portal](sql-database-manage-portal.md)
### [SQL Server Management Studio](sql-database-manage-azure-ssms.md)
### [PowerShell](sql-database-manage-powershell.md)
## [Erstellen und Verwalten von Zugriff und Berechtigungen](sql-database-get-started-security.md)
## Sichern und Schützen von Daten
### [Überwachung](sql-database-auditing-get-started.md)
### [Bedrohungserkennung](sql-database-threat-detection-get-started.md)
### Dynamische Datenmaskierung
#### [Azure-Portal](sql-database-dynamic-data-masking-get-started-portal.md)
## Erstellen und Verwalten von horizontal hochskalierten Datenbanken
### [Elastische Skalierung](sql-database-elastic-scale-get-started.md)
### [Elastische Aufträge](sql-database-elastic-jobs-getting-started.md)
### Elastische Abfragen
### [Datenbankübergreifende Berichte](sql-database-elastic-query-getting-started.md)
### [Datenbankübergreifende Abfragen](sql-database-elastic-query-getting-started-vertical.md)
## [In-Memory-Optimierung](sql-database-in-memory.md)
## [Verschieben von Datenbanken](sql-database-troubleshoot-moving-data.md)
## [Datensynchronisierung](sql-database-get-started-sql-data-sync.md)
##Überwachen und Optimieren von Datenbanken
### [SQL-Datenbankratgeber – Übersicht](sql-database-advisor.md)
### [Einblicke in Workloads im Azure-Portal](sql-database-performance.md)
## [Schnellstarts für Lösungen](sql-database-solution-quick-starts.md)
# Anleitung
## [Überwachung](sql-database-auditing-get-started.md)
## Erstellen
### Ressourcengruppe
#### [PowerShell](sql-database-manage-powershell.md#create-a-resource-group)
### Server
#### [Azure-Portal](sql-database-get-started.md)
#### [C#](sql-database-get-started-csharp.md)
#### [PowerShell](sql-database-manage-powershell.md#create-a-sql-database-server)
### Elastische Datenbankpools
#### [Azure-Portal](sql-database-elastic-pool-create-portal.md)
#### [C#](sql-database-elastic-pool-create-csharp.md)
#### [PowerShell](sql-database-elastic-pool-create-powershell.md)
### Datenbanken
#### Einzeldatenbanken
##### [Azure-Portal](sql-database-get-started.md)
##### [C#](sql-database-get-started-csharp.md)
##### [T-SQL](sql-database-manage-azure-ssms.md#create-and-manage-azure-sql-databases)
##### [PowerShell](sql-database-get-started-powershell.md)
#### [SQL Server Management Studio](sql-database-manage-azure-ssms.md#create-and-manage-azure-sql-databases)
#### Sharddatenbanken
##### [Verwenden des Shard-Zuordnungs-Managers](sql-database-elastic-scale-shard-map-management.md)
##### [Split-Merge-Sicherheitskonfiguration](sql-database-elastic-scale-split-merge-security-configuration.md)
##### [Verwenden von Dapper](sql-database-elastic-scale-working-with-dapper.md)
##### [Verwenden von Entity Framework](sql-database-elastic-scale-use-entity-framework-applications-visual-studio.md)
##### [Datenabhängiges Routing](sql-database-elastic-scale-data-dependent-routing.md)
##### [Sicherheit auf Zeilenebene bei Mehrinstanzenfähigkeit](sql-database-elastic-tools-multi-tenant-row-level-security.md)
### Firewallregeln
#### Server
##### [Azure-Portal](sql-database-configure-firewall-settings.md)
##### [PowerShell](sql-database-configure-firewall-settings-powershell.md)
##### [REST-API](sql-database-configure-firewall-settings-rest.md)
##### [T-SQL](sql-database-configure-firewall-settings-tsql.md#server-level-firewall-rules)
#### Datenbank
##### [T-SQL](sql-database-configure-firewall-settings-tsql.md#database-level-firewall-rules)
### Aufträge
#### [Dienstinstallation](sql-database-elastic-jobs-service-installation.md)
#### [Azure-Portal](sql-database-elastic-jobs-create-and-manage.md)
#### [PowerShell](sql-database-elastic-jobs-powershell.md)
### Anmeldungen
#### [T-SQL](sql-database-manage-azure-ssms.md#create-and-manage-logins.md)
## Entwickeln
### [Übersicht](https://msdn.microsoft.com/library/mt763826.aspx)
### Szenarios
#### [Mehrinstanzenfähige SaaS-Anwendungen](sql-database-design-patterns-multi-tenancy-saas-applications.md)
#### Temporäre Tabellen
##### [Temporäre Tabellen](sql-database-temporal-tables.md)
##### [Aufbewahrungsrichtlinien](sql-database-temporal-tables-retention-policy.md)
#### [JSON-Daten](sql-database-json-features.md)
#### [In-Memory](sql-database-in-memory.md)
###Erste Schritte
#### [Verbindungsbibliotheken](sql-database-libraries.md)
#### Herstellen einer Verbindung für eine Anwendung
##### [.NET](sql-database-develop-dotnet-simple.md)
##### [Java](sql-database-develop-java-simple.md)
##### [Node.js](sql-database-develop-nodejs-simple.md)
##### [PHP](sql-database-develop-php-simple.md)
##### [Python](sql-database-develop-python-simple.md)
##### [Ruby](sql-database-develop-ruby-simple.md)
##### [Excel](sql-database-connect-excel.md)
#### [Verbinden mit Visual Studio](sql-database-connect-query.md)
### Anleitung
#### Erstellen von Servern
##### [PowerShell](sql-database-get-started-powershell.md)
##### [C#](sql-database-get-started-csharp.md)
##### Erstellen von elastischen Pools
###### [PowerShell](sql-database-elastic-pool-create-powershell.md)
###### [C#](sql-database-elastic-pool-create-csharp.md)
#### Erstellen von Datenbanken
##### [PowerShell](sql-database-get-started-powershell.md)
##### [C#](sql-database-get-started-csharp.md)
##### Erstellen von Firewallregeln
###### [PowerShell](sql-database-configure-firewall-settings-powershell.md)
###### [REST-API](sql-database-configure-firewall-settings-rest.md)
#### Verwalten von Servern, Pools, Datenbanken und Firewalls
##### [PowerShell](sql-database-manage-powershell.md)
##### Verwalten von elastischen Pools
###### [PowerShell](sql-database-elastic-pool-manage-powershell.md)
###### [C#](sql-database-elastic-pool-manage-csharp.md)
##### [Ändern von Tarifen und Leistungsstufen](sql-database-scale-up-powershell.md)
#### Verschieben von Daten
##### [Exportieren der Datenbank in eine BACPAC-Datei](sql-database-export-powershell.md)
##### [Importieren der Datenbank aus einer BACPAC-Datei](sql-database-import-powershell.md)
##### [Kopieren einer Datenbank an einen anderen Azure-Standort](sql-database-copy-powershell.md)
#### [Abrufen der erforderlichen Werte für die Authentifizierung einer Anwendung](sql-database-client-id-keys.md)
#### [Elastische Aufträge](sql-database-elastic-jobs-powershell.md)
#### Wiederherstellen einer Datenbank
##### Wiederherstellen einer gelöschten Datenbank
###### [PowerShell](sql-database-restore-deleted-database-powershell.md)
##### Point-in-Time-Wiederherstellung einer Datenbank
###### [PowerShell](sql-database-point-in-time-restore-powershell.md)
##### Geowiederherstellung
###### [PowerShell](sql-database-geo-restore-powershell.md)
#### Replizieren von Daten mit der aktiven Georeplikation
##### Konfigurieren
###### [PowerShell](sql-database-geo-replication-powershell.md)
##### Failover
###### [PowerShell](sql-database-geo-replication-failover-powershell.md)
#### [Verwenden von anderen Ports als 1433 für ADO.NET 4.5](sql-database-develop-direct-route-ports-adonet-v12.md)
#### [Arbeiten mit Fehlermeldungen](sql-database-develop-error-messages.md)
#### [Verwenden der Batchverarbeitung](sql-database-use-batching-to-improve-performance.md)
### Referenz
#### [Transact-SQL](https://msdn.microsoft.com/library/azure/bb510741.aspx)
#### [.NET Framework-Datenanbieter für SQL Server (Konzepte)](https://msdn.microsoft.com/library/kb9s9ks0.aspx)
#### [.NET Framework-Datenanbieter für SQL Server (API-Referenz)](https://msdn.microsoft.com/library/system.data.sqlclient.aspx)
#### SQL-PowerShell
##### [Azure SQL-Datenbank-Cmdlets (Ressourcenverwaltung)](https://msdn.microsoft.com/library/azure/mt574084(v=azure.300\).aspx)
##### [Azure SQL-Datenbank-Cmdlets (Dienstverwaltung)](https://msdn.microsoft.com/library/azure/dn546723(v=azure.300\).aspx)
##### [SQL Server-Cmdlets](https://msdn.microsoft.com/library/mt740629.aspx)
#### SQL-Datenbank-REST-API
##### [REST-API (Ressourcenverwaltung)](https://msdn.microsoft.com/library/azure/mt420159)
##### [REST-API (Dienstverwaltung)](https://msdn.microsoft.com/library/azure/dn505719.aspx)
#### SQL-Datenbank-Verwaltungsbibliothek
##### [SQL-Datenbank-Verwaltungsbibliothek – Referenz](https://msdn.microsoft.com/library/azure/mt349017.aspx)
##### [Abrufen des Pakets mit der SQL-Datenbank-Verwaltungsbibliothek](https://www.nuget.org/packages/Microsoft.Azure.Management.Sql)
#### Entity Framework
##### [Abrufen des Entity Framework-Pakets](https://www.nuget.org/packages/EntityFramework/)
#### [SQL Server-Treiber](https://msdn.microsoft.com/library/mt654049.aspx)
##### [ADO.NET](https://msdn.microsoft.com/library/mt657768.aspx)
##### [JDBC](https://msdn.microsoft.com/library/mt484311.aspx)
##### [Node.js](https://msdn.microsoft.com/library/mt652093.aspx)
##### [ODBC](https://msdn.microsoft.com/library/mt654048.aspx)
##### [PHP](https://msdn.microsoft.com/library/dn865013.aspx)
##### [Python](https://msdn.microsoft.com/library/mt652092.aspx)
##### [Ruby](https://msdn.microsoft.com/library/mt691981.aspx)
#### [Azure SDK (Download)](https://www.visualstudio.com/vs/azure-tools/)
#### [Azure SDK (Dokumentation)](https://azure.microsoft.com/documentation/articles/dotnet-sdk/)
### Ressourcen
#### [SQL Server-Tools](https://msdn.microsoft.com/library/mt238365.aspx)
#### [SQL Server Management Studio (SSMS)](https://msdn.microsoft.com/library/mt238290.aspx)
#### [SQL Server Data Tools (SSDT)](https://msdn.microsoft.com/library/mt204009.aspx)
#### [BCP](https://msdn.microsoft.com/library/ms162802.aspx)
#### [SQLCMD](https://msdn.microsoft.com/library/ms162773.aspx)
#### [SqlPackage](https://msdn.microsoft.com/hh550080.aspx)
## Löschen
### Datenbank
#### [PowerShell](sql-database-manage-powershell.md#delete-a-sql-database)
### Server
#### [PowerShell](sql-database-manage-powershell.md#delete-a-sql-database-server)
## Erkennen von Bedrohungen
### [Bedrohungserkennung](sql-database-threat-detection-get-started.md)
### [Firewall](sql-database-firewall-configure.md)
## Verschlüsseln von Daten
### Always Encrypted
#### [Always Encrypted – Übersicht](sql-database-always-encrypted.md)
#### [Always Encrypted – Azure Key Vault](sql-database-always-encrypted-azure-key-vault.md)
### [Transparent Data Encryption](https://msdn.microsoft.com/library/azure/dn948096)
### [Spaltenverschlüsselung](https://msdn.microsoft.com/library/azure/ms179331)
## Verwalten
###  Authentifizierung
#### SQL-Authentifizierung
#### [Authentifizierung über Azure Active Directory](sql-database-aad-authentication.md)
#### [Mehrstufige Authentifizierung](sql-database-ssms-mfa-authentication.md)
### Server
### Elastische Pools
#### [Azure-Portal](sql-database-elastic-pool-manage-portal.md)
#### [PowerShell](sql-database-elastic-pool-manage-powershell.md)
#### [T-SQL](sql-database-elastic-pool-manage-tsql.md)
#### [C#](sql-database-elastic-pool-manage-csharp.md)
### Datenbanken
#### Einzeldatenbanken
##### [Azure-Portal](sql-database-manage-portal.md)
##### [T-SQL](sql-database-manage-azure-ssms.md#create-and-manage-azure-sql-databases)
##### [PowerShell](sql-database-manage-powershell.md#create-a-sql-database-blank)
#### Ändern von Tarifen und Leistungsstufen
#### [Azure-[Portal](sql-database-scale-up.md)
#### [PowerShell](sql-database-manage-powershell.md#change-the-performance-level-of-a-sql-database)
#### Sharddatenbanken
##### [Migrieren von vorhandenen horizontal hochskalierten Datenbanken zur horizontalen Hochskalierung](sql-database-elastic-convert-to-use-elastic-tools.md)
##### [Verwalten von Anmeldeinformationen](sql-database-elastic-scale-manage-credentials.md)
##### [Skalierung mit dem Split-Merge-Tool für elastische Datenbanken](sql-database-elastic-scale-overview-split-and-merge.md)
##### [Bereitstellen eines Split-Merge-Diensts](sql-database-elastic-scale-configure-deploy-split-and-merge.md)
##### [Hinzufügen eines Shards](sql-database-elastic-scale-add-a-shard.md)
##### [Verwenden der RecoveryManager-Klasse zur Behebung von Problemen mit der Shard-Zuordnung](sql-database-elastic-database-recovery-manager.md)
### Firewallregeln
#### Server
##### [Azure-Portal](sql-database-configure-firewall-settings.md)
##### [PowerShell](sql-database-configure-firewall-settings-powershell.md)
##### [REST-API](sql-database-configure-firewall-settings-rest.md)
##### [T-SQL](sql-database-configure-firewall-settings-tsql.md#server-level-firewall-rules)
#### Datenbank
##### [T-SQL](sql-database-configure-firewall-settings-tsql.md#database-level-firewall-rules)
### Aufträge
#### [Dienstinstallation](sql-database-elastic-jobs-service-installation.md)
#### [Azure-Portal](sql-database-elastic-jobs-create-and-manage.md)
#### [PowerShell](sql-database-elastic-jobs-powershell.md)
#### [Upgraden der Clientbibliothek](sql-database-elastic-scale-upgrade-client-library.md)
### Anmeldungen
#### [Verwalten von Anmeldungen](sql-database-manage-logins.md)
## Maskieren von Daten
### Dynamische Datenmaskierung
#### [Azure-Portal](sql-database-dynamic-data-masking-get-started-portal.md)
#### [Kompatible Clients](sql-database-auditing-and-dynamic-data-masking-downlevel-clients.md)
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
### Exportieren der Datenbank in eine BACPAC-Datei
#### [SQL Server Management Studio](sql-database-cloud-migrate-compatible-export-bacpac-ssms.md)
#### [SQL-Pakethilfsprogramm](sql-database-cloud-migrate-compatible-export-bacpac-sqlpackage.md)
#### [PowerShell](sql-database-export-powershell.md)
### Importieren der Datenbank aus einer BACPAC-Datei
#### [SQL Server Management Studio](sql-database-cloud-migrate-compatible-import-bacpac-ssms.md)
#### [SQL-Pakethilfsprogramm](sql-database-cloud-migrate-compatible-import-bacpac-sqlpackage.md)
#### [Azure-Portal](sql-database-import.md)
#### [PowerShell](sql-database-import-powershell.md)

## Überwachen und Optimieren
### [Einzeldatenbanken](sql-database-performance-guidance.md)
### Elastische Pools
#### [Azure-Portal](sql-database-elastic-pool-manage-portal.md)
#### [PowerShell](sql-database-elastic-pool-manage-powershell.md)
#### [T-SQL](sql-database-elastic-pool-manage-tsql.md)
#### [C#](sql-database-elastic-pool-manage-csharp.md)
### [Query Performance Insight](sql-database-query-performance.md)
### SQL-Datenbankratgeber
#### [Azure-Portal](sql-database-advisor-portal.md)
### Ändern von Tarifen und Leistungsstufen
#### [Azure-Portal](sql-database-scale-up.md)
#### [PowerShell](sql-database-scale-up-powershell.md)
### [Tipps zur Leistungsoptimierung](sql-database-troubleshoot-performance.md)
### In-Memory OLTP
#### [Einführen von In-Memory OLTP](sql-database-in-memory-oltp-migration.md)
#### [Überwachen des In-Memory-OLTP-Speichers](sql-database-in-memory-oltp-monitoring.md)
### Abfragespeicher
#### [Überwachen der Leistung mit dem Abfragespeicher](https://msdn.microsoft.com/library/dn817826.aspx)
#### [Verwendungsszenarien für den Abfragespeicher](https://msdn.microsoft.com/library/mt614796.aspx)
#### [Verwenden des Abfragespeichers](sql-database-operate-query-store.md)
### [Kompatibilitätsgrade](sql-database-compatibility-level-query-performance-130.md)
### [Ereignisüberwachung](sql-database-auditing-get-started.md)
### [Leistungsindikatoren für den Shardzuordnungs-Manager](sql-database-elastic-database-perf-counters.md)
### Erweiterte Ereignisse
#### [Erweiterte Ereignisse](sql-database-xevent-db-diff-from-svr.md)
#### [Code des Ereignisdateiziels](sql-database-xevent-code-event-file.md)
#### [Code des Ringpufferziels](sql-database-xevent-code-ring-buffer.md)
### DMVs
#### [DMVs](sql-database-monitoring-with-dmvs.md)
#### [DMVs](sql-database-manage-azure-ssms#monitor-sql-database-using-dynamic-management-views


## Verschieben von Daten
### Kopieren einer SQL-Datenbank
#### [Übersicht](sql-database-copy.md)
#### [Azure-Portal](sql-database-copy-portal.md)
#### [PowerShell](sql-database-copy-powershell.md)
#### [T-SQL](sql-database-copy-transact-sql.md)
### Exportieren der Datenbank in eine BACPAC-Datei
#### [Azure-Portal](sql-database-export.md)
#### [SQL Server Management Studio](sql-database-cloud-migrate-compatible-export-bacpac-ssms.md)
#### [PowerShell](sql-database-export-powershell.md)
### Importieren der Datenbank aus einer BACPAC-Datei
#### [Azure-Portal](sql-database-import.md)
#### [PowerShell](sql-database-import-powershell.md)
### [Datensynchronisierung](sql-database-get-started-sql-data-sync.md)
### [Laden aus CSV-Datei mit BCP](sql-database-load-from-csv-with-bcp.md)

## Abfrage
### [SQL Server Management Studio](sql-database-connect-query-ssms.md)
### [Abfragen von mehreren Shards](sql-database-elastic-scale-multishard-querying.md)
### Datenbankübergreifende Abfragen
#### [Datenbankübergreifende Abfragen mit verschiedenen Schemas](sql-database-elastic-query-vertical-partitioning.md)
#### [Datenbankübergreifende Berichte](sql-database-elastic-query-horizontal-partitioning.md)
#### [Übersicht über elastische Datenbanktransaktionen mit Azure SQL-Datenbank](sql-database-elastic-transactions-overview.md)
#### [Upgraden der Clientbibliothek](sql-database-elastic-scale-upgrade-client-library.md)
#### [Abfragen von mehreren Shards](sql-database-elastic-scale-multishard-querying.md)

## Wiederherstellen
### Wiederherstellen einer gelöschten Datenbank
### [Azure-Portal](sql-database-restore-deleted-database-portal.md)
### [PowerShell](sql-database-restore-deleted-database-powershell.md)
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
### Konnektivität
#### [Verbindungsprobleme](sql-database-troubleshoot-common-connection-issues.md)
#### [Vorübergehender Verbindungsfehler](sql-database-troubleshoot-connection.md)
#### [Diagnostizieren und Verhindern](sql-database-connectivity-issues.md)
### [Berechtigungen](sql-database-troubleshoot-permissions.md)

# Referenz
## [T-SQL](https://msdn.microsoft.com/library/azure/bb510741.aspx)
## SQL-PowerShell
### [Azure SQL-Datenbank-Cmdlets (Ressourcenverwaltung)](https://msdn.microsoft.com/library/azure/mt574084(v=azure.300\).aspx)
### [SQL Server-Cmdlets](https://msdn.microsoft.com/library/mt740629.aspx)
## SQL-Datenbank-REST-API
### [REST-API (Ressourcenverwaltung)](https://msdn.microsoft.com/library/azure/mt420159)
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
## [SQL Server-Tools](https://msdn.microsoft.com/library/mt238365.aspx)
## [SQL Server Management Studio (SSMS)](https://msdn.microsoft.com/library/mt238290.aspx)
## [SQL Server Data Tools (SSDT)](https://msdn.microsoft.com/library/mt204009.aspx)
## [BCP](https://msdn.microsoft.com/library/ms162802.aspx)
## [SQLCMD](https://msdn.microsoft.com/library/ms162773.aspx)
## [SqlPackage](https://msdn.microsoft.com/hh550080.aspx)

<!--HONumber=Nov16_HO2-->


