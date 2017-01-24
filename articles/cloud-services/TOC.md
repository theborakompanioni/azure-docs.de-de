# Übersicht
## [Was ist Cloud Services?](cloud-services-choose-me.md)
## [Konfigurationsdateien und Packen von Clouddiensten](cloud-services-model-and-package.md)

# Erste Schritte
## [Beispiel für die Verwendung von .NET mit Cloud Services](cloud-services-dotnet-get-started.md)
## [Beispiel für die Verwendung von Python für eine Visual Studio-Cloud Services-Instanz](cloud-services-python-ptvs.md)
## [Einrichten eines Hybrid-HPC-Clusters mit Microsoft HPC Pack](cloud-services-setup-hybrid-hpcpack-cluster.md)

# Anleitung
## Planen
### [Größen virtueller Computer](cloud-services-sizes-specs.md)
### [Updates](cloud-services-update-azure-service.md)

## Entwickeln
### [Erstellen von PHP-Web- und Workerrollen](../cloud-services-php-create-web-role.md)
### [Erstellen und Bereitstellen einer Node.js-Anwendung](cloud-services-nodejs-develop-deploy-app.md)
### [Erstellen einer Node.js-Webanwendung mithilfe von Express](cloud-services-nodejs-develop-deploy-express-app.md)
### Storage und Visual Studio
#### [Blob Storage und verbundene Dienste](../storage/vs-storage-cloud-services-getting-started-blobs.md)
#### [Queue Storage und verbundene Dienste](../storage/vs-storage-cloud-services-getting-started-queues.md)
#### [Table Storage und verbundene Dienste](../storage/vs-storage-cloud-services-getting-started-tables.md)
### Konfigurieren von Paketen für fortlaufende Builds
#### [Visual Studio Team Services und Git](cloud-services-continuous-delivery-use-vso-git.md)
#### [Visual Studio Team Services](cloud-services-continuous-delivery-use-vso.md)
#### [TFS und Team Build](cloud-services-dotnet-continuous-delivery.md)
### [Konfigurieren von Datenverkehrsregeln für eine Rolle](cloud-services-enable-communication-role-instances.md)
### [Behandeln von Cloud Services-Lebenszyklusereignissen](cloud-services-role-lifecycle-dotnet.md)
### [Socket.io (Node.js)](cloud-services-nodejs-chat-app-socketio.md)
### [Tätigen eines Telefonanrufs über Twilio (.NET)](../partner-twilio-cloud-services-dotnet-phone-call-web-role.md)
### [New Relic](../store-new-relic-cloud-services-dotnet-application-performance-management.md)

### Konfigurieren von Starttasks
#### [Erstellen von Starttasks](cloud-services-startup-tasks.md)
#### [Gängige Starttasks](cloud-services-startup-tasks-common.md)
#### [Installieren von .NET mithilfe eines Tasks auf einer Cloud Services-Rolle](cloud-services-dotnet-install-dotnet.md)

### Konfigurieren von Remotedesktop
#### [Visual Studio](cloud-services-role-enable-remote-desktop.md)
#### [Node.js](cloud-services-nodejs-enable-remote-desktop.md)
#### [PowerShell](cloud-services-role-enable-remote-desktop-powershell.md)

## Bereitstellen
### Erstellen und Bereitstellen eines Clouddiensts im Portal
#### [Portal](cloud-services-how-to-create-deploy-portal.md)
#### [Klassisches Portal](cloud-services-how-to-create-deploy.md)
### [Erstellen eines leeren Clouddienstcontainers in PowerShell](cloud-services-powershell-create-cloud-container.md)
### Konfigurieren eines benutzerdefinierten Domänennamens
#### [Portal](cloud-services-custom-domain-name-portal.md)
#### [Klassisches Portal](cloud-services-custom-domain-name.md)
### [Bereitstellen eines Clouddiensts (Node.js)](cloud-services-nodejs-stage-application.md)
### [Herstellen einer Verbindung mit einem benutzerdefinierten Domänencontroller](cloud-services-connect-to-custom-domain.md)

## Verwalten eines Diensts
### Allgemeine Verwaltungsaufgaben
#### [Portal](cloud-services-how-to-manage-portal.md)
#### [Klassisches Portal](cloud-services-how-to-manage.md)
### Konfigurieren eines Clouddiensts
#### [Portal](cloud-services-how-to-configure-portal.md)
#### [Klassisches Portal](cloud-services-how-to-configure.md)
### [Verwalten einer Cloud Services-Instanz mit Azure Automation](automation-manage-cloud-services.md)
### Konfigurieren der automatischen Skalierung
#### [Portal](cloud-services-how-to-scale-portal.md)
#### [Klassisches Portal](cloud-services-how-to-scale.md)
### [Verwenden von Python zum Verwalten von Azure-Ressourcen](cloud-services-python-how-to-use-service-management.md)

### [Patches für Gastbetriebssysteme](cloud-services-guestos-msrc-releases.md)
### Deaktivierung von Gastbetriebssystemen
#### [Deaktivierungsrichtlinie](cloud-services-guestos-retirement-policy.md)
#### [Deaktivierungsinformationen für die Betriebssystemfamilie 1](cloud-services-guestos-family1-retirement.md)
### [Neuigkeiten zur Version des Azure-Gastbetriebssystems](cloud-services-guestos-update-matrix.md)
### [Cloud Services-Rollenkonfiguration – XPath-Cheat Sheet](cloud-services-role-config-xpath.md)

## Verwalten von Zertifikaten
### [Cloud Services und Verwaltungszertifikate](cloud-services-certs-create.md)
### Konfigurieren von SSL 
#### [Portal](cloud-services-configure-ssl-certificate-portal.md)
#### [Klassisches Portal](cloud-services-configure-ssl-certificate.md)

## Überwachen
### [Überwachen von Clouddiensten](cloud-services-how-to-monitor.md)
### [Testen der Leistung](../vs-azure-tools-performance-profiling-cloud-services.md)
#### [Testen mithilfe der Visual Studio-Profilerstellung](cloud-services-performance-testing-visual-studio-profiler.md)
### Aktivieren der Diagnosefunktion
#### [PowerShell](cloud-services-diagnostics-powershell.md)
#### [.NET](cloud-services-dotnet-diagnostics.md)
#### [Visual Studio](../vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines.md)
### [Verwenden von Leistungsindikatoren in Azure-Diagnose](cloud-services-dotnet-diagnostics-performance-counters.md)
### [Speichern und Anzeigen von Diagnosedaten in Azure Storage](cloud-services-dotnet-diagnostics-storage.md)
### [Verfolgen eines Clouddiensts mit der Diagnose](cloud-services-dotnet-diagnostics-trace-flow.md)
### [Senden von Diagnosedaten an Application Insights](cloud-services-dotnet-diagnostics-applicationinsights.md)

## Problembehandlung
### Debuggen 
#### [Remotedebuggen mit Continuous Delivery aktivieren](cloud-services-virtual-machines-dotnet-continuous-delivery-remote-debugging.md)
#### [Optionen für einen Clouddienst](../vs-azure-tools-debugging-cloud-services-overview.md)
#### [Lokaler Clouddienst mit Visual Studio](../vs-azure-tools-debug-cloud-services-virtual-machines.md)
#### [Veröffentlichter Clouddienst mit Visual Studio](../vs-azure-tools-intellitrace-debug-published-cloud-services.md)
### [Zuordnungsfehler bei Cloud Services-Instanzen](cloud-services-allocation-failures.md)
### [Häufige Ursachen für die Wiederverwendung von Cloud Services-Rollen](cloud-services-troubleshoot-common-issues-which-cause-roles-recycle.md)
### [Standardgröße für TEMP-Ordner ist zu klein für eine Rolle](cloud-services-troubleshoot-default-temp-folder-size-too-small-web-worker-role.md)
### [Häufige Bereitstellungsprobleme](cloud-services-troubleshoot-deployment-problems.md)
### [Fehler beim Starten der Rolle](cloud-services-troubleshoot-roles-that-fail-start.md)
### [Anleitungen zur Notfallwiederherstellung](cloud-services-disaster-recovery-guidance.md)
### [Häufig gestellte Fragen zu Cloud Services](cloud-services-faq.md)

# Referenz
## [CSDEF (XML-Schema)](https://msdn.microsoft.com/library/azure/ee758711)
## [CSCFG (XML-Schema)](https://msdn.microsoft.com/library/azure/ee758710)
## [REST](https://msdn.microsoft.com/library/azure/ee460812)

# Ressourcen
## [Preise](https://azure.microsoft.com/pricing/details/cloud-services/)
## [MSDN-Forum](https://social.msdn.microsoft.com/Forums/en-us/home?forum=windowsazuredevelopment)
## [Videos](https://azure.microsoft.com/documentation/videos/index/?services=cloud-services)
## [Dienstupdates](https://azure.microsoft.com/updates/?product=cloud-services&updatetype=&platform=)
## [Lernpfad](https://azure.microsoft.com/documentation/learning-paths/cloud-services/)


<!--HONumber=Jan17_HO3-->


