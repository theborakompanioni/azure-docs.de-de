# Übersicht
## [Was ist Azure Automation?](automation-intro.md)
# Erste Schritte
## [Erste Schritte mit Azure Automation](automation-offering-get-started.md)
## Runbook-Tutorial
### [Erstellen eines grafischen Runbooks](automation-first-runbook-graphical.md)
### [Erstellen eines PowerShell-Runbooks](automation-first-runbook-textual-powershell.md)
### [Mein erstes Python-Runbook](automation-first-runbook-textual-python2.md)
### [Erstellen eines PowerShell-Workflow-Runbooks](automation-first-runbook-textual.md)
# Anleitung
## Authentifizierung und Sicherheit
### [Erstellen eines eigenständigen Automation-Kontos](automation-create-standalone-account.md)
### [Erstellen eines Azure AD-Benutzerkontos](automation-create-aduser-account.md)
### [Konfigurieren der Authentifizierung mit AWS](automation-config-aws-account.md)
### [Erstellen des ausführenden Automation-Kontos](automation-create-runas-account.md)
### [Überprüfen der Konfiguration eines Automation-Kontos](automation-verify-runas-authentication.md)
### [Verwalten der rollenbasierten Zugriffssteuerung in Azure](automation-role-based-access-control.md)
### [Verwalten eines Automation-Kontos](automation-manage-account.md)
## Erstellen von Runbooks
### [Runbooktypen](automation-runbook-types.md)
### [Erstellen und Importieren von Runbooks](automation-creating-importing-runbook.md)
### [Bearbeiten von Textrunbooks](automation-edit-textual-runbook.md)
### [Bearbeiten von grafischen Runbooks](automation-graphical-authoring-intro.md)
### [Testen eines Runbooks](automation-testing-runbook.md)
### [Grundlagen des PowerShell-Workflows](automation-powershell-workflow.md)
### [Untergeordnete Runbooks](automation-child-runbooks.md)
### [Runbookausgabe](automation-runbook-output-and-messages.md)
### [Integration der Quellcodeverwaltung](automation-source-control-integration.md)
## Automatisieren
### [Starten eines Runbooks](automation-starting-a-runbook.md)
### [Starten eines Runbooks über einen Webhook](automation-webhooks.md)
### [Konfigurieren von Runbookeingabeparametern](automation-runbook-input-parameters.md)
### [Fehlerbehandlung bei grafischen Runbooks](automation-runbook-graphical-error-handling.md)
### [Nachverfolgen eines Runbookauftrags](automation-runbook-execution.md)
### [Ändern der Runbookeinstellungen](automation-runbook-settings.md)
### [Verwalten von Azure Automation-Daten](automation-managing-data.md)
### [Aufrufen eines Azure Automation-Runbooks über eine Log Analytics-Warnung](automation-invoke-runbook-from-omsla-alert.md)
### [Übergeben eines JSON-Objekts an ein Azure Automation-Runbook](automation-pass-json-string.md)
## Hybrid Runbook Worker
### [Bereitstellen von Hybrid Runbook Worker](automation-hybrid-runbook-worker.md)
### [Azure Automation Hybrid Runbook Worker (Windows)](automation-windows-hrw-install.md)
### [Azure Automation Hybrid Runbook Worker (Linux)](automation-linux-hrw-install.md)
### [Ausführen von Runbooks im Worker](automation-hrw-run-runbooks.md)
### [Entfernen von Azure Automation Hybrid-Runbook Workern](automation-remove-hrw.md)
## Bereitstellen der Konfigurationsverwaltung (DSC)
### [Übersicht über das Konfigurieren des gewünschten Zustands (DSC)](automation-dsc-overview.md)
### [Erste Schritte](automation-dsc-getting-started.md)
### [Aufnehmen von Computern in die Verwaltung](automation-dsc-onboarding.md)
### [Kompilieren von DSC-Konfigurationen](automation-dsc-compile.md)
### [Continuous Deployment mit Chocolatey](automation-dsc-cd-chocolatey.md)
### [Weiterleiten von Azure Automation DSC-Berichtsdaten an OMS Log Analytics](automation-dsc-diagnostics.md)
## Verwalten von Objekten
### [Zertifikate](automation-certificates.md)
### [Verbindungen](automation-connections.md)
### [Anmeldeinformationen](automation-credentials.md)
### [Integrationsmodule](automation-integration-modules.md)
### [Zeitpläne](automation-schedules.md)
### [Variablen](automation-variables.md)
### [Aktualisieren von Azure PowerShell-Modulen](automation-update-azure-modules.md)
## Szenarien
### [Runbook-Katalog](automation-runbook-gallery.md)
### [Erstellen einer Amazon Web Service-VM](automation-scenario-aws-deployment.md)
### [Wartung nach Azure-VM-Warnung](automation-azure-vm-alert-integration.md)
### [Starten und Beenden von VMs mit JSON-Tags](automation-scenario-start-stop-vm-wjson-tags.md)
### [Entfernen von Ressourcengruppen](automation-scenario-remove-resourcegroup.md)
### [Integration der Quellcodeverwaltung mit GitHub Enterprise](automation-scenario-source-control-integration-with-github-ent.md)
### [Integration der Quellcodeverwaltung mit VSTS](automation-scenario-source-control-integration-with-VSTS.md)
### [Aufrufen eines Azure Automation-Runbooks über eine Log Analytics-Warnung](automation-invoke-runbook-from-omsla-alert.md)
### [Bereitstellen einer Azure Resource Manager-Vorlage in einem Azure Automation-PowerShell-Runbook](automation-deploy-template-runbook.md)
## Lösungen
### [Änderungsnachverfolgung](../log-analytics/log-analytics-change-tracking.md)
### [Updateverwaltung](../operations-management-suite/oms-solution-update-management.md)
### [Starten und Beenden von VMs außerhalb der Kernzeit](automation-solution-vm-management.md)
## Überwachen
### [Weiterleiten von Azure Automation-Auftragsdaten an Log Analytics](automation-manage-send-joblogs-log-analytics.md)
### [Aufheben der Verknüpfung eines Azure Automation-Kontos mit Log Analytics](automation-unlink-from-log-analytics.md)
## Migrieren
### [Migration von Orchestrator](automation-orchestrator-migration.md)
### [Verschieben des Automation-Kontos](automation-migrate-account-subscription.md)
## Problembehandlung
### [Problembehandlung für häufige Fehler](automation-troubleshooting-automation-errors.md)
### [Problembehandlung für Hybrid Runbook Worker](automation-troubleshooting-hybrid-runbook-worker.md)
# Referenz
## [PowerShell](/powershell/module/azurerm.automation)
## [PowerShell (klassisch)](/powershell/module/azure/?view=azuresmps-3.7.0)
## [.NET](/dotnet/api/microsoft.azure.management.automation)
## [REST](/rest/api/automation)
## [REST (klassisch)](https://msdn.microsoft.com/library/azure/mt163781)
# Ressourcen
## [Einführungsvideo für Automation](https://azure.microsoft.com/documentation/videos/azure-automation-101-with-powershell-and-eamon-o-reilly/)
## [Azure Automation-Training](https://mva.microsoft.com/en-US/training-courses/automating-the-cloud-with-azure-automation-8323?l=C6mIpCay_4804984382)
## [Azure-Roadmap](https://azure.microsoft.com/roadmap/?category=monitoring-management)
## [Lernpfad](https://azure.microsoft.com/documentation/learning-paths/automation/)
## [MSDN-Forum](https://social.msdn.microsoft.com/Forums/azure/en-US/home?forum=azureautomation)  
## [Preise](https://azure.microsoft.com/pricing/details/automation/)  
## [Preisrechner](https://azure.microsoft.com/pricing/calculator/)
## [Versionshinweise](https://azure.microsoft.com/updates/?product=automation)
## [Dienstupdates](https://azure.microsoft.com/updates/?product=automation)
## [Stack Overflow](http://stackoverflow.com/questions/tagged/azure-automation)
## [Videos](https://azure.microsoft.com/documentation/videos/index/?services=automation)
