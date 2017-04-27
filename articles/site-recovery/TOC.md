# Übersicht
## [Was ist Site Recovery?](site-recovery-overview.md)
## [Wie funktioniert Site Recovery?](site-recovery-components.md)
## [Wie funktioniert die Hyper-V-Replikation in Azure?](site-recovery-hyper-v-azure-architecture.md)
## [Welche Workloads können Sie schützen?](site-recovery-workload.md)
## [Site Recovery-Supportmatrix](site-recovery-support-matrix-to-azure.md)
## [HÄUFIG GESTELLTE FRAGEN](site-recovery-faq.md)
## [Ansehen einer Einführung](https://azure.microsoft.com/resources/videos/index/?services=site-recovery)

# Erste Schritte
## [Replizieren von VMware-VMs in Azure](site-recovery-vmware-to-azure.md)
## [Replizieren physischer Server in Azure](site-recovery-physical-servers-to-azure.md) 
## [Replizieren von Hyper-V-VMs in Azure (mit VMM)](site-recovery-vmm-to-azure.md)
## [Replizieren von Hyper-V-VMs in Azure](site-recovery-hyper-v-site-to-azure.md)
## [Replizieren von Hyper-V-VMs an einem sekundären Standort (mit VMM)](site-recovery-vmm-to-vmm.md)
## [Replizieren von VMware-VMs und physischen Servern an einem sekundären Standort](site-recovery-vmware-to-vmware.md)
## [Replizieren von VMware-VMs in Azure in einer mehrinstanzenfähigen Bereitstellung (CSP)](site-recovery-multi-tenant-support-vmware-using-csp.md)

# Anleitung
## Planen
### [Voraussetzungen für die Bereitstellung](site-recovery-prereq.md)
### [Planen der Netzwerkinfrastruktur](site-recovery-network-design.md)
### [Planen der Kapazität und Skalieren der VMware-Replikation in Azure](site-recovery-plan-capacity-vmware.md)
### [Deployment Planner für die VMware-Replikation in Azure](site-recovery-deployment-planner.md)
### [Capacity Planner für die Hyper-V-Replikation](site-recovery-capacity-planner.md)

## Konfigurieren
### [Einrichten der Quellumgebung](site-recovery-set-up-vmware-to-azure.md)
### [Einrichten der Zielumgebung](site-recovery-prepare-target-vmware-to-azure.md)
### [Konfigurieren der Replikationseinstellungen](site-recovery-setup-replication-settings-vmware.md)
### [Bereitstellen des Mobilitätsdiensts für die VMware-Replikation](site-recovery-vmware-to-azure-install-mob-svc.md)
#### [Bereitstellen des Mobilitätsdiensts mithilfe von System Center Configuration Manager](site-recovery-install-mobility-service-using-sccm.md)
#### [Bereitstellen des Mobilitätsdiensts mithilfe von Azure Automation DSC](site-recovery-automate-mobility-service-install.md)
### [Replikation aktivieren](site-recovery-replicate-vmware-to-azure.md)
## Failover und Failback
### [Ausführen eines Failovers für geschützte Computer](site-recovery-failover.md)
### [Einrichten von Wiederherstellungsplänen](site-recovery-create-recovery-plans.md)
#### [Hinzufügen von Azure-Runbooks zu Wiederherstellungsplänen](site-recovery-runbook-automation.md)
### [Ausführen eines Testfailovers](site-recovery-test-failover-to-azure.md)
### [Erneutes Schützen von Computern nach einem Failover](site-recovery-how-to-reprotect.md)
### [Failback von Azure](site-recovery-failback-azure-to-vmware.md)

## Migrieren
### [Migrieren zu Azure](site-recovery-migrate-to-azure.md)
### [Zwischen Azure-Regionen migrieren](site-recovery-migrate-azure-to-azure.md)
### [Migrieren von AWS Windows-Instanzen zu Azure](site-recovery-migrate-aws-to-azure.md)
## Workloads
### [Active Directory und DNS](site-recovery-active-directory.md)
### [SQL Server](site-recovery-sql.md)
### [SharePoint](site-recovery-workload.md#protect-sharepoint)
### [Azure IoT Hub](site-recovery-workload.md#protect-dynamics-ax)
### [RDS](site-recovery-workload.md#protect-rds)
### [Exchange](site-recovery-workload.md#protect-exchange)
### [SAP](site-recovery-workload.md#protect-sap)
### [Andere Workloads](site-recovery-workload.md#workload-summary)
## Automatisieren der Replikation
### [Automatisieren der Hyper-V-Replikation in Azure (ohne VMM)](site-recovery-deploy-with-powershell-resource-manager.md)
### [Automatisieren der Hyper-V-Replikation in Azure (mit VMM)](site-recovery-vmm-to-azure-powershell-resource-manager.md)
### [Automatisieren der Hyper-V-Replikation an einem sekundären Standort (mit VMM)](site-recovery-vmm-to-vmm-powershell-resource-manager.md)
## Verwalten
### [Replikationseinstellungen bearbeiten](site-recovery-setup-replication-settings-vmware.md#edit-replication-policy.md)
### [Verwalten von Prozessservern in Azure](site-recovery-vmware-setup-azure-ps-resource-manager.md)
### [Verwalten des Konfigurationsservers](site-recovery-vmware-to-azure-manage-configuration-server.md)
### [Verwalten eines horizontal hochskalierten Prozessservers](site-recovery-vmware-to-azure-manage-scaleout-process-server.md)
### [Verwalten von vCenter-Servern](site-recovery-vmware-to-azure-manage-vCenter.md)
### [Entfernen von Servern und Deaktivieren des Schutzes](site-recovery-manage-registration-and-protection.md)
## [Überwachen und Behandeln von Problemen](site-recovery-monitoring-and-troubleshooting.md)

# Referenz
## [PowerShell](/powershell/resourcemanager/azurerm.siterecovery/v3.2.0/azurerm.siterecovery)
## [PowerShell (klassisch)](/powershell/servicemanagement/azure.siterecovery/v3.1.0/azure.siterecovery)
## [REST](https://msdn.microsoft.com/en-us/library/mt750497)

# Verwandte Themen
## [Azure Automation](/azure/automation/)

# Ressourcen
## [Lernpfad](https://azure.microsoft.com/documentation/learning-paths/site-recovery/)
## [Forum](https://social.msdn.microsoft.com/Forums/azure/en-US/home?forum=hypervrecovmgr)
## [Blog](http://azure.microsoft.com/blog/tag/azure-site-recovery/)
## [Preise](https://azure.microsoft.com/pricing/details/site-recovery/)
## [Dienstupdates](https://azure.microsoft.com/updates/?product=site-recovery)
