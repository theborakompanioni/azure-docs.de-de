# Übersicht
## [Was ist SQL-Datenbank?](sql-database-technical-overview.md)
### [Dienstebenen](sql-database-service-tiers.md)
### [DTUs und eDTUs](sql-database-what-is-a-dtu.md)
### [DTU-Benchmark – Übersicht](sql-database-benchmark-overview.md)
### [Ressourceneinschränkungen](sql-database-resource-limits.md)
### [Funktionen](sql-database-features.md)
### [SQL-Datenbank – Häufig gestellte Fragen](sql-database-faq.md)
## Vergleiche
### [SQL-Datenbank im Vergleich mit SQL auf einem virtuellen Computer](sql-database-paas-vs-sql-server-iaas.md)
### [T-SQL-Unterschiede](sql-database-transact-sql-information.md)
### [SQL im Vergleich mit NoSQL](../documentdb/documentdb-nosql-vs-sql.md)
## [SQL-Datenbanktools](sql-database-manage-overview.md)
## [SQL-Datenbanktutorials](sql-database-explore-tutorials.md)
## Sicherheit
### [Sicherheitsübersicht](sql-database-security-overview.md)
### [Azure Security Center für Azure SQL-Datenbank](https://azure.microsoft.com/documentation/articles/security-center-sql-database/)
### [SQL Security Center](https://msdn.microsoft.com/library/azure/bb510589)
# Erste Schritte
## Datenbanken und Server
### Lernen
#### [Server](sql-database-server-overview.md)
#### [Einzeldatenbanken](sql-database-overview.md)
#### [Mehrere Datenbanken](sql-database-elastic-scale-introduction.md)
##### Zuordnen von Mandanten
###### [Elastische Clientbibliothek](sql-database-elastic-database-client-library.md)
###### [Shard-Zuordnungs-Manager](sql-database-elastic-scale-shard-map-management.md)
###### [Datenabhängiges Routing](sql-database-elastic-scale-data-dependent-routing.md)
###### [Verwalten von Anmeldeinformationen](sql-database-elastic-scale-manage-credentials.md)
###### [Abfragen von mehreren Shards](sql-database-elastic-scale-multishard-querying.md)
##### Elastische Pools (Ressourcenpools)
###### [Was ist ein elastischer Pool?](sql-database-elastic-pool.md)
###### [Wann sollte ein elastischer Pool verwendet werden?](sql-database-elastic-pool-guidance.md)
###### [Elastische Pools – Preisgestaltung](sql-database-elastic-pool-price.md)
##### Sharddatenbanken
###### [Elastische Tools – Glossar](sql-database-elastic-scale-glossary.md)
###### [Verschieben von Daten zwischen Shards](sql-database-elastic-scale-overview-split-and-merge.md)
###### [Elastische Tools – häufig gestellte Fragen](sql-database-elastic-scale-faq.md)
##### Elastische Abfrage (datenbankübergreifende Abfragen)
###### [Was ist eine elastischer Abfrage?](sql-database-elastic-query-overview.md)
##### Elastische Transaktionen (verteilte Transaktionen)
###### [Clouddatenbankübergreifende Transaktionen](sql-database-elastic-transactions-overview.md)
##### Elastische Aufträge (datenbankübergreifende Aufträge)
###### [Was ist ein elastischer Auftrag?](sql-database-elastic-jobs-overview.md)
#### [Herstellen einer Verbindung mit SQL-Datenbank mithilfe von Azure RemoteApp](sql-database-ssms-remoteapp.md)
#### [Verwalten von SQL-Datenbanken mit dem Azure Automation-Dienst](sql-database-manage-automation.md)
### Empfohlene Vorgehensweise
#### [Erstellen einer Einzeldatenbank über das Azure-Portal](sql-database-get-started.md)
#### [Erstellen einer Einzeldatenbank mithilfe von PowerShell](sql-database-get-started-powershell.md)
#### [Erstellen einer Einzeldatenbank mithilfe von C#](sql-database-get-started-csharp.md)
#### [Erstellen einer Anwendung mit Sharding](sql-database-elastic-scale-get-started.md)
#### [Verschieben von Daten zwischen Shards](sql-database-elastic-scale-configure-deploy-split-and-merge.md)
#### [Erste Schritte mit elastischen Aufträgen](sql-database-elastic-jobs-getting-started.md)
#### [Erste Schritte mit elastischen Abfragen](sql-database-elastic-query-getting-started-vertical.md)
#### [Erstellen von Berichten mithilfe elastischer Abfragen](sql-database-elastic-query-getting-started.md)
#### [Abfragen von Datenbanken mit unterschiedlichen Schemas](sql-database-elastic-query-vertical-partitioning.md)
#### [Erstellen von übergreifenden Berichten für horizontal hochskalierte Datenbanken](sql-database-elastic-query-horizontal-partitioning.md)
## Migrieren und Verschieben von Daten
### Lernen
#### [Migrieren einer Datenbank](sql-database-cloud-migrate.md)
#### [Exportieren einer Datenbank](sql-database-export.md)
#### [Datensynchronisierung](sql-database-get-started-sql-data-sync.md)
#### [Kopieren einer SQL-Datenbank](sql-database-copy.md)
## Firewallregeln, Authentifizierung und Autorisierung
### Lernen
#### [Zugriffssteuerung](sql-database-control-access.md)
#### [Firewallregeln](sql-database-firewall-configure.md)
#### [Verwalten von Anmeldungen](sql-database-manage-logins.md)
#### [Azure AD-Authentifizierung](sql-database-aad-authentication.md)
#### [Multi-Factor Authentication](sql-database-ssms-mfa-authentication.md)
### Empfohlene Vorgehensweise
#### [SQL-Authentifizierung und -Autorisierung](sql-database-control-access-sql-authentication-get-started.md)
#### [Azure AD-Authentifizierung und Autorisierung](sql-database-control-access-aad-authentication-get-started.md)
## Sichern und Schützen von Daten
### Lernen
#### Überwachung
##### [Überwachung](sql-database-auditing.md)
##### [Unterstützung für kompatible Clients und IP-Endpunkt-Änderungen für die Überwachung](sql-database-auditing-and-dynamic-data-masking-downlevel-clients.md)
#### [Bedrohungserkennung](sql-database-threat-detection.md)
#### Verschlüsseln von Daten
##### [Transparent Data Encryption](https://msdn.microsoft.com/library/azure/dn948096)
##### [Spaltenverschlüsselung](https://msdn.microsoft.com/library/azure/ms179331)
#### Maskieren von Daten
##### [Dynamische Datenmaskierung](sql-database-dynamic-data-masking-get-started.md)
### Empfohlene Vorgehensweise
#### [Dynamische Datenmaskierung mithilfe des Azure-Portals](sql-database-dynamic-data-masking-get-started-portal.md)
#### [Konfigurieren der Überwachung über das Azure-Portal](sql-database-auditing-portal.md)
#### [Konfigurieren der Überwachung mit PowerShell](sql-database-auditing-powershell.md)
#### [Konfigurieren der Überwachung mit der REST-API](sql-database-auditing-rest.md)
#### [Always Encrypted mithilfe des Windows-Zertifikats](sql-database-always-encrypted.md)
#### [Always Encrypted mithilfe von Azure Key Vault](sql-database-always-encrypted-azure-key-vault.md)
## Geschäftskontinuität
### Lernen
#### [Übersicht](sql-database-business-continuity.md)
#### [Datenbanksicherungen](sql-database-automated-backups.md)
#### [Langfristige Aufbewahrung](sql-database-long-term-retention.md)
#### [Datenbankwiederherstellung mithilfe von Sicherungen](sql-database-recovery-using-backups.md)
#### [Wiederherstellen einer einzelnen Tabelle](sql-database-cloud-migrate-restore-single-table-azure-backup.md)
#### [Wiederherstellen nach dem Ausfall eines Rechenzentrums](sql-database-disaster-recovery.md)
#### [Authentifizierungsanforderungen für die Notfallwiederherstellung](sql-database-geo-replication-security-config.md)
#### [Entwurfsszenarien für Geschäftskontinuität](sql-database-designing-cloud-solutions-for-disaster-recovery.md)
#### [Strategien für die Notfallwiederherstellung mit elastischen Pools](sql-database-disaster-recovery-strategies-for-applications-with-elastic-pool.md)
#### [Parallele Upgrades](sql-database-manage-application-rolling-upgrade.md)
#### [Durchführen von Übungen zur Notfallwiederherstellung](sql-database-disaster-recovery-drills.md)
#### [Aktive Georeplikation – Übersicht](sql-database-geo-replication-overview.md)
### Empfohlene Vorgehensweise
#### [Azure-Portal: Sicherung und Wiederherstellung](sql-database-get-started-backup-recovery-portal.md)
#### [PowerShell: Sicherung und Wiederherstellung](sql-database-get-started-backup-recovery-powershell.md)
## App-Entwicklung
### Lernen
#### [Datenbankanwendungsentwicklung – Übersicht](sql-database-develop-overview.md)
#### [Verbindungsbibliotheken](sql-database-libraries.md)
#### [Mehrinstanzenfähige SaaS-Anwendungen](sql-database-design-patterns-multi-tenancy-saas-applications.md)
#### [Skalieren mehrinstanzenfähiger SaaS-Anwendungen mit Sicherheit auf Zeilenebene](sql-database-elastic-tools-multi-tenant-row-level-security.md)
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
#### [Verwenden von Entity Framework](sql-database-elastic-scale-use-entity-framework-applications-visual-studio.md)
#### [Verwenden der Clientbibliothek mit Dapper](sql-database-elastic-scale-working-with-dapper.md)
### Kundenimplementierungen
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
#### [In-Memory-Optimierung](sql-database-in-memory.md)
### Empfohlene Vorgehensweise
#### [SQL Server-Entwicklung](https://msdn.microsoft.com/library/ms179422.aspx)
#### [Einführen von In-Memory OLTP](sql-database-in-memory-oltp-migration.md)
## Überwachen und Optimieren
### Lernen
#### [Einzeldatenbanken](sql-database-single-database-monitor.md)
#### [SQL-Datenbankratgeber – Übersicht](sql-database-advisor.md)
#### [Leitfaden für Einzeldatenbanken](sql-database-performance-guidance.md)
#### [Performance Insights: Azure-Portal](sql-database-performance.md)
#### [Verwenden der Batchverarbeitung](sql-database-use-batching-to-improve-performance.md)
#### [Erweiterte Ereignisse](sql-database-xevent-db-diff-from-svr.md)
## SQL-Datenbank V11
### [Einstellung von Web Edition und Business Edition](sql-database-web-business-sunset-faq.md)
### [Dienstebenenratgeber](sql-database-service-tier-advisor.md)
### [Bewertungstool für elastische Pools](sql-database-elastic-pool-database-assessment-powershell.md)
### [Upgrade auf V12](sql-database-v12-plan-prepare-upgrade.md)
#### [Upgrade mithilfe des Azure-Portals](sql-database-upgrade-server-portal.md)
#### [Upgrade mithilfe von PowerShell](sql-database-upgrade-server-powershell.md)
# Anleitung
## Erstellen und Verwalten
### [Verwalten mit dem Azure-Portal](sql-database-manage-portal.md)
### [Verwalten mit PowerShell](sql-database-manage-powershell.md)
### [Verwalten mithilfe von SSMS](sql-database-manage-azure-ssms.md)
### Server
#### [Verwalten mit dem Azure-Portal](sql-database-manage-servers-portal.md)
#### [Verwalten mit PowerShell](sql-database-manage-servers-powershell.md)
### Einzeldatenbanken
#### [Verwalten mit dem Azure-Portal](sql-database-manage-single-databases-portal.md)
#### [Verwalten mit PowerShell](sql-database-manage-single-databases-powershell.md)
#### [Verwalten mithilfe von Transact-SQL](sql-database-manage-single-databases-tsql.md)
### Firewallregeln
#### [Erstellen mithilfe des Azure-Portals](sql-database-configure-firewall-settings.md)
#### [Erstellen mithilfe von PowerShell](sql-database-configure-firewall-settings-powershell.md)
#### [Erstellen mithilfe der REST-API](sql-database-configure-firewall-settings-rest.md)
#### [Erstellen mit T-SQL](sql-database-configure-firewall-settings-tsql.md)
### Mehrere Datenbanken
#### [Upgraden der Clientbibliothek in Clientanwendungen](sql-database-elastic-scale-upgrade-client-library.md)
#### Sharddatenbanken
##### [Sicherheitskonfiguration](sql-database-elastic-scale-split-merge-security-configuration.md)
##### [Hinzufügen eines Shards](sql-database-elastic-scale-add-a-shard.md)
##### [Beheben von Problemen bei der Shardzuordnung](sql-database-elastic-database-recovery-manager.md)
##### [Migrieren zu Sharddatenbanken](sql-database-elastic-convert-to-use-elastic-tools.md)
##### [Erstellen von Leistungsindikatoren für den Shardzuordnungs-Manager](sql-database-elastic-database-perf-counters.md)
#### Elastische Aufträge
##### [Wie funktioniert die Installation?](sql-database-elastic-jobs-service-installation.md)
##### [Verwalten mit PowerShell](sql-database-elastic-jobs-powershell.md) 
##### [Verwalten mit dem Azure-Portal](sql-database-elastic-jobs-create-and-manage.md)
##### [Wie funktioniert die Deinstallation?](sql-database-elastic-jobs-uninstall.md)
#### Elastische Pools
##### [Verwalten mit dem Azure-Portal](sql-database-elastic-pool-manage-portal.md)
##### [Verwalten mit PowerShell](sql-database-elastic-pool-manage-powershell.md)
##### [Verwalten mit C#](sql-database-elastic-pool-manage-csharp.md)
##### [Verwalten mit T-SQL](sql-database-elastic-pool-manage-tsql.md)
## Authentifizieren und Autorisieren
### [Konfigurieren der Azure AD-Authentifizierung](sql-database-aad-authentication-configure.md)
### [Konfigurieren der mehrstufigen Authentifizierung](sql-database-ssms-mfa-authentication-configure.md)
## Konfigurieren der Bedrohungserkennung
### [Konfigurieren der Bedrohungserkennung mit dem Azure-Portal](sql-database-threat-detection-portal.md)
## Verschlüsseln von Daten
### [Transparent Data Encryption](https://msdn.microsoft.com/library/azure/dn948096)
### [Spaltenverschlüsselung](https://msdn.microsoft.com/library/azure/ms179331)
## Überwachen und Optimieren
### [Query Performance Insight](sql-database-query-performance.md)
### [SQL-Datenbankratgeber](sql-database-advisor-portal.md)
### [DMVs](sql-database-monitoring-with-dmvs.md)
### [Kompatibilitätsgrade](sql-database-compatibility-level-query-performance-130.md)
### [Tipps zur Leistungsoptimierung](sql-database-troubleshoot-performance.md)
### [Erstellen von Warnungen](sql-database-insights-alerts-portal.md)
### [Überwachen des In-Memory-OLTP-Speichers](sql-database-in-memory-oltp-monitoring.md)
### Abfragespeicher
#### [Überwachen der Leistung mit dem Abfragespeicher](https://msdn.microsoft.com/library/dn817826.aspx)
#### [Verwendungsszenarien für den Abfragespeicher](https://msdn.microsoft.com/library/mt614796.aspx)
#### [Verwenden des Abfragespeichers](sql-database-operate-query-store.md)
### Erweiterte Ereignisse
#### [Code des Ereignisdateiziels](sql-database-xevent-code-event-file.md)
#### [Code des Ringpufferziels](sql-database-xevent-code-ring-buffer.md)
## Verschieben von Daten
### Kopieren einer SQL-Datenbank
#### [Kopieren mithilfe des Azure-Portals](sql-database-copy-portal.md)
#### [Kopieren mithilfe von PowerShell](sql-database-copy-powershell.md)
#### [Kopieren mithilfe von T-SQL](sql-database-copy-transact-sql.md)
### Exportieren der Datenbank in eine BACPAC-Datei
#### [Exportieren über das Azure-Portal](sql-database-export-portal.md)
#### [Exportieren mithilfe des SQL-Pakethilfsprogramms](sql-database-export-sqlpackage.md)
#### [Exportieren mithilfe von PowerShell](sql-database-export-powershell.md)
#### [Exportieren mithilfe von SSMS](sql-database-export-ssms.md)
## Daten importieren
### [Importieren über das Azure-Portal](sql-database-import-portal.md)
### [Importieren mithilfe von PowerShell](sql-database-import-powershell.md)
### [Importieren mithilfe von SQLPackage](sql-database-import-sqlpackage.md)
### [Laden aus CSV-Datei mit BCP](sql-database-load-from-csv-with-bcp.md)
## Abfrage
### [Abfragen mithilfe von SSMS](sql-database-connect-query-ssms.md)
## Sichern und Wiederherstellen
### Langfristiges Aufbewahren von Sicherungen
#### [Verwalten der langfristigen Aufbewahrung von Sicherungen mit dem Azure-Portal](sql-database-manage-long-term-backup-retention-portal.md)
#### [Verwalten der langfristigen Aufbewahrung von Sicherungen mit PowerShell](sql-database-manage-long-term-backup-retention-powershell.md)
### Wiederherstellen einer gelöschten Datenbank
#### [Wiederherstellen gelöschter Datenbanken über das Azure-Portal](sql-database-restore-deleted-database-portal.md)
#### [Wiederherstellen gelöschter Datenbanken mithilfe von PowerShell](sql-database-restore-deleted-database-powershell.md)
### Point-in-Time-Wiederherstellung einer Datenbank
#### [Point-in-Time-Wiederherstellung mit dem Azure-Portal](sql-database-point-in-time-restore-portal.md)
#### [Point-in-Time-Wiederherstellung einer Datenbank mit PowerShell](sql-database-point-in-time-restore-powershell.md)
### Wiederherstellen aus einer georedundanten Sicherung
### [Durchführen einer Geowiederherstellung über das Azure-Portal](sql-database-geo-restore-portal.md)
### [Durchführen einer Geowiederherstellung mithilfe von PowerShell](sql-database-geo-restore-powershell.md)
## Aktive Georeplikation
### [Konfigurieren über das Azure-Portal](sql-database-geo-replication-portal.md)
### [Konfigurieren mithilfe von PowerShell](sql-database-geo-replication-powershell.md)
### [Konfigurieren mithilfe von T-SQL](sql-database-geo-replication-transact-sql.md)
### [Durchführen eines Failovers über das Azure-Portal](sql-database-geo-replication-failover-portal.md)
### [Durchführen eines Failovers mithilfe von PowerShell](sql-database-geo-replication-failover-powershell.md)
### [Durchführen eines Failovers mithilfe von T-SQL](sql-database-geo-replication-failover-transact-sql.md)
## Problembehandlung
### [Verbindungsprobleme](sql-database-troubleshoot-common-connection-issues.md)
### [Vorübergehender Verbindungsfehler](sql-database-troubleshoot-connection.md)
### [Diagnostizieren und Verhindern](sql-database-connectivity-issues.md)
### [Berechtigungen](sql-database-troubleshoot-permissions.md)
### [Verschieben von Datenbanken](sql-database-troubleshoot-moving-data.md)
# Referenz
## [PowerShell](/powershell/resourcemanager/azurerm.sql/v2.3.0/azurerm.sql)
## [PowerShell (Elastische Datenbank)](/powershell/elasticdatabasejobs/v0.8.33/elasticdatabasejobs)
## [Azure CLI 2.0](/cli/azure/appservice)
## [.NET](/dotnet/api/microsoft.azure.management.sql.models)
## [Java](/java/api/com.microsoft.azure.management.sql)
## [Node.js](https://msdn.microsoft.com/library/mt652093.aspx)
## [Python](https://msdn.microsoft.com/library/mt652092.aspx)
## [Ruby](https://msdn.microsoft.com/library/mt691981.aspx)
## [PHP](https://msdn.microsoft.com/library/dn865013.aspx)
## [T-SQL](https://msdn.microsoft.com/library/bb510741.aspx)
## [REST](https://msdn.microsoft.com/library/azure/mt163571.aspx)

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
