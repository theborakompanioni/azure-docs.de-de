# Übersicht
## [Was ist Site Recovery?](site-recovery-overview.md)
## [Wie funktioniert Site Recovery?](site-recovery-components.md)
## [Welche Workloads können Sie schützen?](site-recovery-workload.md)
## [Site Recovery-Supportmatrix](site-recovery-support-matrix.md)
## [HÄUFIG GESTELLTE FRAGEN](site-recovery-faq.md)
## [Ansehen einer Einführung](https://www.youtube.com/watch?v=eOOwMQPBKfM)

# Erste Schritte
## [Replizieren von VMware-VMs in Azure](site-recovery-vmware-to-azure.md)
## [Replizieren von VMware-VMs in Azure in einer mehrinstanzenfähigen Bereitstellung (CSP)](site-recovery-multi-tenant-support-vmware-using-csp.md)
## [Replizieren von Hyper-V-VMs in Azure (mit VMM)](site-recovery-vmm-to-azure.md)
## [Replizieren von Hyper-V-VMs in Azure](site-recovery-hyper-v-site-to-azure.md)
## [Replizieren von VMware-VMs und physischen Servern an einem sekundären Standort](site-recovery-vmware-to-vmware.md)
## [Replizieren von Hyper-V-VMs an einem sekundären Standort (mit VMM)](site-recovery-vmm-to-vmm.md)

# Anleitung
## Planen
### [Voraussetzungen für die Bereitstellung](site-recovery-prereq.md)
### [Aspekte der Netzwerkinfrastruktur](site-recovery-network-design.md)
### [Verwenden von Site Recovery Capacity Planner](site-recovery-capacity-planner.md)
### [Planen der Kapazität und Skalieren der VMware-Replikation in Azure](site-recovery-plan-capacity-vmware.md)
## Konfigurieren
### [Einrichten der Quellreplikationsumgebung](site-recovery-set-up-vmware-to-azure.md)
### [Konfigurieren der Replikationseinstellungen](site-recovery-setup-replication-settings-vmware.md)
### [Bereitstellen des Mobilitätsdiensts für die VMware-Replikation](site-recovery-vmware-to-azure-install-mob-svc.md)
#### [Bereitstellen des Mobilitätsdiensts mithilfe von System Center Configuration Manager](site-recovery-install-mobility-service-using-sccm.md)
#### [Bereitstellen des Mobilitätsdiensts mithilfe von Azure Automation DSC](site-recovery-automate-mobility-service-install.md)
### [Entfernen von Servern und Deaktivieren des Schutzes](site-recovery-manage-registration-and-protection.md)
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
## Failover und Failback
### [Failover in Site Recovery](site-recovery-failover.md)
### [Einrichten von Wiederherstellungsplänen](site-recovery-create-recovery-plans.md)
#### [Hinzufügen von Azure-Runbooks zu Wiederherstellungsplänen](site-recovery-runbook-automation.md)
### [Ausführen eines Testfailovers von VMware auf Azure](site-recovery-test-failover-to-azure.md)
### [Ausführen eines Testfailovers zwischen zwei VMM-Standorten](site-recovery-test-failover-vmm-to-vmm.md)
### [Failback für VMware-VMs und physische Server](site-recovery-failback-azure-to-vmware.md)
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


<!--HONumber=Feb17_HO2-->


