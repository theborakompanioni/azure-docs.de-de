
# Übersicht
## [Was ist Azure Backup?](backup-introduction-to-azure-backup.md)

# Erste Schritte
## [Sichern virtueller Azure-Computer](backup-azure-vms-first-look-arm.md)
## [Sichern von Windows Server- oder Windows-Computern](backup-try-azure-backup-in-10-mins.md)
## [Sichern von VMware-Servern](backup-azure-backup-server-vmware.md)

# Anleitung
## Virtuelle Azure-Computer
### Vorbereiten des virtuellen Computers
#### [Vorbereiten von mit Resource Manager bereitgestellten virtuellen Computern](backup-azure-arm-vms-prepare.md)
#### [Vorbereiten virtueller Azure-Computer](backup-azure-vms-prepare.md)
### Planen der Umgebung
#### [Planen der Sicherungsinfrastruktur für virtuelle Computer](backup-azure-vms-introduction.md)
### Sichern virtueller Computer
#### [Sichern von virtuellen Computern in einem Recovery Services-Tresor](backup-azure-arm-vms.md)
#### [Sichern verschlüsselter virtueller Computer](backup-azure-vms-encryption.md)
#### [Sichern von virtuellen Azure-Computern](backup-azure-vms.md)
### Verwalten und Überwachen virtueller Computer
#### [Verwalten von Azure-VM-Sicherungen im Azure-Portal](backup-azure-manage-vms.md)
#### [Überwachen von Warnungen für Azure-VM-Sicherungen im Azure-Portal](backup-azure-monitor-vms.md)
#### [Verwalten und Überwachen von Azure-VM-Sicherungen im klassischen Portal](backup-azure-manage-vms-classic.md)
### Wiederherstellen von Daten von virtuellen Computern
#### [Wiederherstellen von Dateien aus Azure-VM-Sicherungen](backup-azure-restore-files-from-vm.md)
#### [Wiederherstellen der mit Azure Resource Manager bereitgestellten virtuellen Computer im Azure-Portal](backup-azure-arm-restore-vms.md)
#### [Wiederherstellen verschlüsselter virtueller Computer](backup-azure-vms-encryption.md)
#### [Wiederherstellen virtueller Computer in Azure](backup-azure-restore-vms.md)
#### [Wiederherstellen von Key Vault-Schlüssel und -Geheimnis für verschlüsselte virtuelle Computer](backup-azure-restore-key-secret.md)


## Windows Server
### [Sichern von Windows Server-Dateien und -Ordnern](backup-configure-vault.md)
### [Sichern des Systemstatus von Windows Server](backup-azure-system-state.md)
### [Wiederherstellen von Dateien aus Azure für Windows Server](backup-azure-restore-windows-server.md)
### [Wiederherstellen des Systemstatus von Windows Server](backup-azure-restore-system-state.md)
### [Überwachen und Verwalten von Recovery Services-Tresoren](backup-azure-manage-windows-server.md)
### Sichern und Wiederherstellen über das klassische Portal
#### [Windows Server mit dem klassischen Bereitstellungsmodell](backup-configure-vault-classic.md)
#### [Verwalten von Backup-Tresoren mit dem klassischen Bereitstellungsmodell](backup-azure-manage-windows-server-classic.md)
#### [Wiederherstellen von Dateien auf einem Windows-Server mit dem klassischen Bereitstellungsmodell](backup-azure-restore-windows-server-classic.md)

## Azure Backup Server
### [Azure Backup Server-Schutzmatrix](backup-mabs-protection-matrix.md)
### Installieren oder Aktualisieren
#### [Vorbereiten der Sicherung von Azure Backup Server-Workloads im Azure-Portal](backup-azure-microsoft-azure-backup.md)
#### [Vorbereiten der Sicherung von Azure Backup Server-Workloads im klassischen Portal](backup-azure-microsoft-azure-backup-classic.md)
#### [Hinzufügen von Speicher zu Azure Backup Server](backup-mabs-add-storage.md)
#### [Upgraden von Azure Backup Server auf v.2](backup-mabs-upgrade-to-v2.md)
#### [Unbeaufsichtigte Installation von Azure Backup Server](backup-mabs-unattended-install.md)
### Schützen von Workloads
#### [Sichern eines VMware-Servers mit Azure Backup Server](backup-azure-backup-server-vmware.md)
#### [Sichern von Exchange mit Azure Backup Server](backup-azure-exchange-mabs.md)
#### [Sichern einer SharePoint-Farm mit Azure Backup Server](backup-azure-backup-sharepoint-mabs.md)
#### [Sichern von SQL mit Azure Backup Server](backup-azure-sql-mabs.md)
#### [Schützen des Systemstatus und Bare-Metal-Recovery](backup-mabs-system-state-and-bmr.md)

## Data Protection Manager
### [Vorbereiten von DPM-Workloads im Azure-Portal](backup-azure-dpm-introduction.md)
### [Vorbereiten von DPM-Workloads im klassischen Portal](backup-azure-dpm-introduction-classic.md)
### [Verwenden von System Center DPM zum Sichern eines Exchange-Servers](backup-azure-backup-exchange-server.md)
### [Wiederherstellen von Daten auf einem anderen DPM-Server](backup-azure-alternate-dpm-server.md)
### [Verwenden von DPM zum Sichern von SQL Server-Workloads](backup-azure-backup-sql.md)
### [Verwenden von DPM zum Sichern einer SharePoint-Farm](backup-azure-backup-sharepoint.md)

## Azure SQL-Datenbank
### [Konfigurieren der langfristigen Sicherungsaufbewahrung](../sql-database/sql-database-configure-long-term-retention.md?toc=%2fazure%2fbackup%2ftoc.json)
### [Anzeigen von Sicherungen in einem Recovery Services-Tresor](../sql-database/sql-database-view-backups-in-vault.md?toc=%2fazure%2fbackup%2ftoc.json)
### [Wiederherstellen langfristig aufbewahrter Sicherungen](../sql-database/sql-database-restore-from-long-term-retention.md?toc=%2fazure%2fbackup%2ftoc.json)
### [Löschen von langfristigen Azure SQL-Sicherungen](../sql-database/sql-database-long-term-retention-delete.md?toc=%2fazure%2fbackup%2ftoc.json)

## Verwenden von PowerShell
### [Azure-VMs im Azure-Portal](backup-azure-vms-automation.md)
### [Azure-VMs im klassischen Portal](backup-azure-vms-classic-automation.md)
### [DPM im Azure-Portal](backup-dpm-automation.md)
### [DPM im klassischen Portal](backup-dpm-automation-classic.md)
### [Windows Server im Azure-Portal](backup-client-automation.md)
### [Windows Server im klassischen Portal](backup-client-automation-classic.md)

## Häufig gestellte Fragen
### [Häufig gestellte Fragen zum Recovery Services-Tresor](backup-azure-backup-faq.md)
### [Häufig gestellte Fragen zur Azure-VM-Sicherung](backup-azure-vm-backup-faq.md)
### [Häufig gestellte Fragen zur Sicherung von Dateien/Ordnern mit dem Azure Backup-Agent](backup-azure-file-folder-backup-faq.md)

## Problembehandlung
### [Probleme bei der Azure-VM-Sicherung im Azure-Portal](backup-azure-vms-troubleshoot.md)
### [Probleme bei der Azure-VM-Sicherung im klassischen Portal](backup-azure-vms-troubleshoot-classic.md)
### [Fehler bei der Azure-VM-Sicherung: Keine Kommunikation mit dem VM-Agent zum Abrufen des Momentaufnahmestatus möglich. Zeitüberschreitung bei Momentaufnahme-Teilvorgang für VM.](backup-azure-troubleshoot-vm-backup-fails-snapshot-timeout.md)
### [Langsame Sicherung von Dateien und Ordnern in Azure Backup](backup-azure-troubleshoot-slow-backup-performance-issue.md)
### [Behandeln von Problemen mit Azure Backup Server](backup-azure-mabs-troubleshoot.md)

# Konzepte
## [Übersicht über Recovery Services-Tresore](backup-azure-recovery-services-vault-overview.md)
## [Aktualisieren eines Backup-Tresors auf einen Recovery Services-Tresor](backup-azure-upgrade-backup-to-recovery-services.md)
## [Löschen eines Recovery Services-Tresors](backup-azure-delete-vault.md)
## [Rollenbasierte Zugriffssteuerung](backup-rbac-rs-vault.md)
## [Sicherheit für Hybrid-Sicherungen](backup-azure-security-feature.md)
## [Konfigurieren von Azure Backup-Berichten](backup-azure-configure-reports.md)
## [Datenmodell für Azure Backup-Berichte](backup-azure-reports-data-model.md)
## [Log Analytics-Datenmodell für Azure Backup](backup-azure-log-analytics-data-model.md)
## [Konfigurieren der Offlinesicherung](backup-azure-backup-import-export.md)
## [Ersetzen der Bandbibliothek](backup-azure-backup-cloud-as-tape.md)
## [Anwendungskonsistente Sicherungen von virtuellen Linux-Computern](backup-azure-linux-app-consistent.md)

# Referenz
## [PowerShell](/powershell/module/azurerm.recoveryservices.backup)
## [.NET](/dotnet/api/microsoft.azure.management.recoveryservices.backup)

# Ressourcen
## [Azure-Roadmap](https://azure.microsoft.com/roadmap/)
## [MSDN-Forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=windowsazureonlinebackup)
## [Preise](https://azure.microsoft.com/pricing/details/backup/)
## [Preisrechner](https://azure.microsoft.com/pricing/calculator/)
## [Dienstupdates](https://azure.microsoft.com/updates/?product=backup)
## [Videos](https://azure.microsoft.com/documentation/videos/index/?services=backup)
