
# Übersicht
## [Was ist Azure Backup?](backup-introduction-to-azure-backup.md)

# Erste Schritte
## [Sichern von Dateien und Ordnern](backup-try-azure-backup-in-10-mins.md)
## [Sichern von virtuellen Azure-Computern](backup-azure-vms-first-look.md)
## [Schützen von Azure-VMs](backup-azure-vms-first-look-arm.md)

# Anleitung
## Verwenden von PowerShell
### [Azure-VMs im Azure-Portal](backup-azure-vms-automation.md)
### [Azure-VMs im klassischen Portal](backup-azure-vms-classic-automation.md)
### [DPM im Azure-Portal](backup-dpm-automation.md)
### [DPM im klassischen Portal](backup-dpm-automation-classic.md)
### [Windows Server im Azure-Portal](backup-client-automation.md)
### [Windows Server im klassischen Portal](backup-client-automation-classic.md)

## Azure Backup Server
### [Vorbereiten der Sicherung von Azure Backup Server-Workloads im Azure-Portal](backup-azure-microsoft-azure-backup.md)
### [Vorbereiten der Sicherung von Azure Backup Server-Workloads im klassischen Portal](backup-azure-microsoft-azure-backup-classic.md)
### [Sichern eines VMware-Servers mit Azure Backup Server](backup-azure-backup-server-vmware.md)
### [Sichern von Exchange mit Azure Backup Server](backup-azure-exchange-mabs.md)
### [Sichern einer SharePoint-Farm mit Azure Backup Server](backup-azure-backup-sharepoint-mabs.md)
### [Sichern von SQL mit Azure Backup Server](backup-azure-sql-mabs.md)

## Data Protection Manager
### [Vorbereiten von DPM-Workloads im Azure-Portal](backup-azure-dpm-introduction.md)
### [Vorbereiten von DPM-Workloads im klassischen Portal](backup-azure-dpm-introduction-classic.md)
### [Verwenden von System Center DPM zum Sichern eines Exchange-Servers](backup-azure-backup-exchange-server.md)
### [Wiederherstellen von Daten im Backup-Tresor auf einem anderen DPM-Server](backup-azure-alternate-dpm-server.md)
### [Verwenden von DPM zum Sichern von SQL Server-Workloads](backup-azure-backup-sql.md)
### [Verwenden von DPM zum Sichern einer SharePoint-Farm](backup-azure-backup-sharepoint.md)

## Virtuelle Azure-Computer
### [Vorbereiten virtueller Azure-Computer](backup-azure-vms-prepare.md)
### [Vorbereiten von mit Resource Manager bereitgestellten virtuellen Computern](backup-azure-arm-vms-prepare.md)
### [Planen der Sicherungsinfrastruktur für virtuelle Computer](backup-azure-vms-introduction.md)
### [Sichern virtueller Azure-Computer im Sicherungstresor](backup-azure-vms.md)
### [Sichern von virtuellen Computern in einem Recovery Services-Tresor](backup-azure-arm-vms.md)
### [Sichern und Wiederherstellen verschlüsselter virtueller Computer](backup-azure-vms-encryption.md)
### [Verwalten und Überwachen von Azure-VM-Sicherungen im klassischen Portal](backup-azure-manage-vms-classic.md)
### [Verwalten von Azure-VM-Sicherungen im Azure-Portal](backup-azure-manage-vms.md)
### [Überwachen von Warnungen für Azure-VM-Sicherungen im Azure-Portal](backup-azure-monitor-vms.md)
### [Wiederherstellen von Dateien aus Azure-VM-Sicherungen](backup-azure-restore-files-from-vm.md)
### [Wiederherstellen virtueller Computer in Azure](backup-azure-restore-vms.md)
### [Wiederherstellen der mit Azure Resource Manager bereitgestellten virtuellen Computer im Azure-Portal](backup-azure-arm-restore-vms.md)
### [Wiederherstellen von Key Vault-Schlüssel und -Geheimnis für verschlüsselte virtuelle Computer mithilfe von Azure Backup](backup-azure-restore-key-secret.md)

## Azure SQL-Datenbank
### [Konfigurieren der langfristigen Sicherungsaufbewahrung](../sql-database/sql-database-configure-long-term-retention.md?toc=%2fazure%2fbackup%2ftoc.json)
### [Anzeigen von Sicherungen in einem Recovery Services-Tresor](../sql-database/sql-database-view-backups-in-vault.md?toc=%2fazure%2fbackup%2ftoc.json)
### [Wiederherstellen langfristig aufbewahrter Sicherungen](../sql-database/sql-database-restore-from-long-term-retention.md?toc=%2fazure%2fbackup%2ftoc.json)
### [Löschen von langfristigen Azure SQL-Sicherungen](../sql-database/sql-database-long-term-retention-delete.md?toc=%2fazure%2fbackup%2ftoc.json)

## Windows-Dateien und Ordner
### [Windows Server mit dem klassischen Bereitstellungsmodell](backup-configure-vault-classic.md)
### [Windows Server in Azure mit dem Resource Manager-Bereitstellungsmodell](backup-configure-vault.md)
### [Verwalten von Backup-Tresoren mit dem klassischen Bereitstellungsmodell](backup-azure-manage-windows-server-classic.md)
### [Überwachen und Verwalten von Recovery Services-Tresoren](backup-azure-manage-windows-server.md)
### [Wiederherstellen von Dateien auf einem Windows-Server mit dem Resource Manager-Bereitstellungsmodell](backup-azure-restore-windows-server.md)
### [Wiederherstellen von Dateien auf einem Windows-Server mit dem klassischen Bereitstellungsmodell](backup-azure-restore-windows-server-classic.md)

## [Häufig gestellte Fragen](backup-azure-backup-faq.md)

## Problembehandlung
### [Probleme bei der Azure-VM-Sicherung im Azure-Portal](backup-azure-vms-troubleshoot.md)
### [Probleme bei der Azure-VM-Sicherung im klassischen Portal](backup-azure-vms-troubleshoot-classic.md)
### [Azure Backup Server](backup-azure-mabs-troubleshoot.md)
### [Fehler bei der Azure-VM-Sicherung: Keine Kommunikation mit dem VM-Agent zum Abrufen des Momentaufnahmestatus möglich. Zeitüberschreitung bei Momentaufnahme-Teilvorgang für VM.](backup-azure-troubleshoot-vm-backup-fails-snapshot-timeout.md)
### [Langsame Sicherung von Dateien und Ordnern in Azure Backup](backup-azure-troubleshoot-slow-backup-performance-issue.md)

# Konzepte
## [Rollenbasierte Zugriffssteuerung](backup-rbac-rs-vault.md)
## [Sicherheit für Hybrid-Sicherungen](backup-azure-security-feature.md)
## [Löschen eines Azure Backup-Tresors](backup-azure-delete-vault.md)
## [Konfigurieren der Offlinesicherung](backup-azure-backup-import-export.md)
## [Ersetzen der Bandbibliothek](backup-azure-backup-cloud-as-tape.md)

# Referenz
## [PowerShell](/powershell/resourcemanager/azurerm.recoveryservices.backup/v2.3.0/azurerm.recoveryservices.backup)
## [.NET](/dotnet/api/microsoft.azure.management.recoveryservices.backup)

# Ressourcen
## [Preise](https://azure.microsoft.com/pricing/details/backup/)
## [MSDN-Forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=windowsazureonlinebackup)
## [Videos](https://azure.microsoft.com/documentation/videos/index/?services=backup)
## [Dienstupdates](https://azure.microsoft.com/updates/?product=backup)
