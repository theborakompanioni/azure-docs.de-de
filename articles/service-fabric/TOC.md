
# Übersicht
## [Was ist Service Fabric?](service-fabric-overview.md)
## [Grundlegendes zu Microservices](service-fabric-overview-microservices.md)
## [Anwendungsszenarien](service-fabric-application-scenarios.md)
## [Muster und Szenarien](service-fabric-patterns-and-scenarios.md)
## [Architektur](service-fabric-architecture.md)
## [Terminologie](service-fabric-technical-overview.md)
## [Einführung](service-fabric-content-roadmap.md)

# Erste Schritte
## Einrichten der Entwicklungsumgebung
### [Windows](service-fabric-get-started.md)
### [Linux](service-fabric-get-started-linux.md)
### [Mac OS](service-fabric-get-started-mac.md)
## Erstellen Ihrer ersten Anwendung
### [C# unter Windows](service-fabric-create-your-first-application-in-visual-studio.md)
### [Java unter Linux](service-fabric-create-your-first-linux-application-with-java.md)
### [C# unter Linux](service-fabric-create-your-first-linux-application-with-csharp.md)
## [Bereitstellen von Apps in einem lokalen Cluster](service-fabric-get-started-with-a-local-cluster.md)
## [Erstellen Ihres ersten Clusters in Azure](service-fabric-get-started-azure-cluster.md)
## [Erstellen Ihres ersten eigenständigen Clusters](service-fabric-get-started-standalone-cluster.md)

# Anleitung
## Erstellen einer Anwendung
  
### Konzepte
#### [Unterstützte Programmiermodelle](service-fabric-choose-framework.md)
#### [Anwendungsmodell](service-fabric-application-model.md)
#### [Dienstmanifestressourcen](service-fabric-service-manifest-resources.md)
#### [Dienststatus](service-fabric-concepts-state.md)
#### [Dienstpartitionierung](service-fabric-concepts-partitioning.md)
#### [Verfügbarkeit von Diensten](service-fabric-availability-services.md)
#### [Skalierbarkeit von Anwendungen](service-fabric-concepts-scalability.md)
#### [ASP.NET Core](service-fabric-reliable-services-communication-aspnetcore.md)

### [Planen der App-Kapazität](service-fabric-capacity-planning.md)

### Erstellen eines Diensts mit ausführbarer Gastdatei
#### [Bereitstellen einer ausführbaren Gastanwendungsdatei](service-fabric-deploy-existing-app.md)
#### [Bereitstellen mehrerer ausführbarer Gastanwendungsdateien](service-fabric-deploy-multiple-apps.md)

### Erstellen eines Containerdiensts
#### [Übersicht](service-fabric-containers-overview.md)
#### [Bereitstellen von Windows Containern](service-fabric-deploy-container.md)
#### [Bereitstellen von Linux-Containern](service-fabric-deploy-container-linux.md)

### Erstellen eines Reliable Services-Diensts
#### [Übersicht](service-fabric-reliable-services-introduction.md)
#### Konzepte
##### [Reliable Services-Lebenszyklus – C#](service-fabric-reliable-services-lifecycle.md)
##### [Reliable Services-Lebenszyklus – Java](service-fabric-reliable-services-lifecycle-java.md)
##### [Zuverlässige Auflistungen](service-fabric-reliable-services-reliable-collections.md)

#### Erste Schritte
##### [C# unter Windows](service-fabric-reliable-services-quick-start.md)
##### [Java unter Linux](service-fabric-reliable-services-quick-start-java.md)

#### Reliable Services-Lebenszyklus
#### [Verwenden zuverlässiger Auflistungen](service-fabric-work-with-reliable-collections.md)
#### [Konfigurieren](service-fabric-reliable-services-configuration.md)
#### [Senden von Benachrichtigungen](service-fabric-reliable-services-notifications.md)
#### [Sichern und Wiederherstellen](service-fabric-reliable-services-backup-restore.md)

#### Kommunizieren mit Diensten
##### [Kommunizieren mit Reliable Services](service-fabric-reliable-services-communication.md)
##### [Dienstremoting – C#](service-fabric-reliable-services-communication-remoting.md)
##### [Dienstremoting – Java](service-fabric-reliable-services-communication-remoting-java.md)
##### [WCF](service-fabric-reliable-services-communication-wcf.md)
##### [Sichere Kommunikation – C#](service-fabric-reliable-services-secure-communication.md)
##### [Sichere Kommunikation – Java](service-fabric-reliable-services-secure-communication-java.md)

#### [Erweiterte Verwendung](service-fabric-reliable-services-advanced-usage.md)

### Erstellen eines Reliable Actors-Diensts
#### [Übersicht](service-fabric-reliable-actors-introduction.md)
#### Konzepte
#### [Architektur](service-fabric-reliable-actors-platform.md)
#### [Lebenszyklus und Garbage Collection](service-fabric-reliable-actors-lifecycle.md)
#### [Zustandsverwaltung](service-fabric-reliable-actors-state-management.md)
#### [Polymorphie](service-fabric-reliable-actors-polymorphism.md)
#### [Eintrittsinvarianz](service-fabric-reliable-actors-reentrancy.md)
#### [Typserialisierung](service-fabric-reliable-actors-notes-on-actor-type-serialization.md)

#### Erste Schritte
##### [C# unter Windows](service-fabric-reliable-actors-get-started.md)
##### [Java unter Linux](service-fabric-reliable-actors-get-started-java.md)

#### [Senden von Benachrichtigungen](service-fabric-reliable-actors-events.md) 
#### [Festlegen von Timern und Erinnerungen](service-fabric-reliable-actors-timers-reminders.md)
#### [Konfigurieren von „KvsActorStateProvider“](service-fabric-reliable-actors-kvsactorstateprovider-configuration.md)
#### [Konfigurieren von Kommunikationseinstellungen](service-fabric-reliable-actors-fabrictransportsettings.md) 
#### [Konfigurieren von „ReliableDictionaryActorStateProvider“](service-fabric-reliable-actors-reliabledictionarystateprovider-configuration.md)

### Kommunizieren mit Diensten
#### [Dienstkommunikation](service-fabric-connect-and-communicate-with-services.md)
#### [Reverseproxy](service-fabric-reverseproxy.md)

### [Hinzufügen eines Web-Front-Ends](service-fabric-add-a-web-frontend.md)

### Arbeiten in einer IDE
#### [Erste Schritte mit dem Eclipse-Plug-In für die Entwicklung mit Java](service-fabric-get-started-eclipse.md)
#### [Verwalten von Apps in Visual Studio](service-fabric-manage-application-in-visual-studio.md)
#### [Konfigurieren sicherer Verbindungen in Visual Studio](service-fabric-visualstudio-configure-secure-connections.md)
#### [Konfigurieren Ihrer Anwendung für mehrere Umgebungen](service-fabric-manage-multiple-environment-app-configuration.md)

### Konfigurieren der Sicherheit
#### [Verwalten von Anwendungsgeheimnissen](service-fabric-application-secret-management.md)  
#### [Konfigurieren von Sicherheitsrichtlinien für Ihre Anwendung](service-fabric-application-runas-security.md)

### Debuggen
#### [Debuggen eines C#-Diensts in VS](service-fabric-debugging-your-application.md)
#### [Debuggen eines Java-Diensts in Eclipse](service-fabric-debugging-your-application-java.md)
#### [Allgemeine Fehler und Ausnahmen](service-fabric-errors-and-exceptions.md)

### Lokales Überwachen und Diagnostizieren
#### [Windows](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md)
#### [Linux](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally-linux.md)

### Migrieren aus Cloud Services
#### [Unterschiede zwischen Cloud Services und Service Fabric](service-fabric-cloud-services-migration-differences.md)
#### [Migrieren zu Service Fabric](service-fabric-cloud-services-migration-worker-role-stateless-service.md)
#### [Empfohlene Vorgehensweisen](/azure/architecture/service-fabric/migrate-from-cloud-services)

## Verwalten des Anwendungslebenszyklus
### [Übersicht](service-fabric-application-lifecycle.md)
### [Packen einer Anwendung](service-fabric-package-apps.md)
### [Grundlegendes zur ImageStoreConnectionString-Einstellung](service-fabric-image-store-connection-string.md)
### Bereitstellen oder Entfernen von Anwendungen
#### [PowerShell](service-fabric-deploy-remove-applications.md)
#### [Visual Studio](service-fabric-publish-app-remote-cluster.md)
#### [FabricClient-APIs](service-fabric-deploy-remove-applications-fabricclient.md)
### Upgraden einer Anwendung
#### [Übersicht](service-fabric-application-upgrade.md)
#### [Konfigurieren des Anwendungsupgrades](service-fabric-visualstudio-configure-upgrade.md)
#### [Parameter für Anwendungsupgrades](service-fabric-application-upgrade-parameters.md)
#### Upgrade
##### [PowerShell](service-fabric-application-upgrade-tutorial-powershell.md)
##### [Visual Studio](service-fabric-application-upgrade-tutorial.md)
#### [Problembehandlung bei Anwendungsupgrades](service-fabric-application-upgrade-troubleshooting.md)
#### [Datenserialisierung bei Anwendungsupgrades](service-fabric-application-upgrade-data-serialization.md)
#### [Weiterführende Themen zu Anwendungsupgrades](service-fabric-application-upgrade-advanced.md)

### Testen von Anwendungen und Diensten
#### [Übersicht über die Fehleranalyse](service-fabric-testability-overview.md)
#### [Testen der Kommunikation zwischen Diensten](service-fabric-testability-scenarios-service-communication.md)
#### Simulieren von Ausfällen
##### [Verwenden des kontrollierten Chaos](service-fabric-controlled-chaos.md)
##### [Verwenden von Testaktionen](service-fabric-testability-actions.md)
##### [Bei Ausführung von Workloads](service-fabric-testability-workload-tests.md)
##### [Verwenden von Testszenarien](service-fabric-testability-scenarios.md)
##### [Verwenden der Knotenübergang-APIs](service-fabric-node-transition-apis.md)
#### [Durchführen eines Auslastungstests für Ihre Anwendung](service-fabric-vso-load-test.md)

### Einrichten von Continuous Integration
#### [Einrichten von Continuous Integration mit VSTS](service-fabric-set-up-continuous-integration.md)
#### [Bereitstellen Ihrer Linux-Java-App mithilfe von Jenkins](service-fabric-cicd-your-linux-java-application-with-jenkins.md)

## Erstellen und Verwalten von Clustern

### [Übersicht](service-fabric-deploy-anywhere.md)
### Konzepte
#### [Beschreiben eines Clusters](service-fabric-cluster-resource-manager-cluster-description.md)
#### [Clustersicherheit](service-fabric-cluster-security.md)
#### [Funktionsunterschiede zwischen Linux und Windows](service-fabric-linux-windows-differences.md)

### Planen und Vorbereiten
#### [Kapazitätsplanung](service-fabric-cluster-capacity.md)
#### [Notfallwiederherstellung](service-fabric-disaster-recovery.md)

### Cluster in Azure
#### Konzepte
##### [Knotentypen und VM-Skalierungsgruppen](service-fabric-cluster-nodetypes.md)
##### [Clusternetzwerkmuster](service-fabric-patterns-networking.md)
#### Erstellen 
##### [Azure-Portal](service-fabric-cluster-creation-via-portal.md)
##### [Azure Resource Manager](service-fabric-cluster-creation-via-arm.md)
##### [Visual Studio und Azure Resource Manager](service-fabric-cluster-creation-via-visual-studio.md)
#### Skalieren 
##### [Manuell](service-fabric-cluster-scale-up-down.md)
##### [Programmgesteuert](service-fabric-cluster-programmatic-scaling.md)
#### [Upgrade](service-fabric-cluster-upgrade.md)
#### [Festlegen der Zugriffssteuerung](service-fabric-cluster-security-roles.md)
#### [Konfigurieren](service-fabric-cluster-fabric-settings.md)
#### [Verwalten von Clusterzertifikaten](service-fabric-cluster-security-update-certs-azure.md) 
#### [Löschen](service-fabric-cluster-delete.md)

### Eigenständige Cluster
#### [Inhalt des eigenständigen Pakets](service-fabric-cluster-standalone-package-contents.md)
#### [Planen und Vorbereiten der Bereitstellung](service-fabric-cluster-standalone-deployment-preparation.md)
#### Erstellen
##### [Lokales Erstellen](service-fabric-cluster-creation-for-windows-server.md)
##### [Erstellen auf virtuellen Azure-Computern](service-fabric-cluster-creation-with-windows-azure-vms.md)
##### [Schützen mithilfe von Zertifikaten](service-fabric-windows-cluster-x509-security.md)  
##### [Schützen mithilfe der Windows-Sicherheit](service-fabric-windows-cluster-windows-security.md)
#### [Skalieren](service-fabric-cluster-windows-server-add-remove-nodes.md)
#### [Festlegen der Zugriffssteuerung](service-fabric-cluster-security-roles.md)
#### [Konfigurieren](service-fabric-cluster-manifest.md)
#### [Upgrade](service-fabric-cluster-upgrade-windows-server.md) 

### [Visualisieren eines Clusters](service-fabric-visualizing-your-cluster.md)
### [Herstellen einer Verbindung mit einem sicheren Cluster](service-fabric-connect-to-secure-cluster.md)

### [Verwalten eines Clusters mithilfe der Azure-CLI](service-fabric-azure-cli.md)

### Verwalten und Orchestrieren von Clusterressourcen
#### [Übersicht über den Clusterressourcen-Manager](service-fabric-cluster-resource-manager-introduction.md)
#### [Architektur des Clusterressourcen-Managers](service-fabric-cluster-resource-manager-architecture.md)
#### [Beschreiben eines Clusters](service-fabric-cluster-resource-manager-cluster-description.md)
#### [Übersicht über Anwendungsgruppen](service-fabric-cluster-resource-manager-application-groups.md)
#### [Konfigurieren der Einstellungen des Clusterressourcen-Managers](service-fabric-cluster-resource-manager-configure-services.md)
#### [Metriken zum Ressourcenverbrauch](service-fabric-cluster-resource-manager-metrics.md)
#### [Nutzen der Dienstaffinität](service-fabric-cluster-resource-manager-advanced-placement-rules-affinity.md)
#### [Richtlinien zur Dienstplatzierung](service-fabric-cluster-resource-manager-advanced-placement-rules-placement-policies.md)
#### [Verwalten eines Clusters](service-fabric-cluster-resource-manager-management-integration.md)
#### [Clusterdefragmentierung](service-fabric-cluster-resource-manager-defragmentation-metrics.md)
#### [Ausbalancieren eines Clusters](service-fabric-cluster-resource-manager-balancing.md)
#### [Drosselung](service-fabric-cluster-resource-manager-advanced-throttling.md)
#### [Dienstverschiebung](service-fabric-cluster-resource-manager-movement-cost.md)

### [Herstellen einer Verbindung mit einem sicheren Cluster](service-fabric-connect-to-secure-cluster.md)

## Untersuchen der Anwendungs- und Clusterintegrität
### [Überwachen der Service Fabric-Integrität](service-fabric-health-introduction.md)
### [Melden und Überprüfen der Dienstintegrität](service-fabric-diagnostics-how-to-report-and-check-service-health.md)
### [Hinzufügen von benutzerdefinierten Berichten zur Integrität](service-fabric-report-health.md)
### [Behandeln von Problemen anhand von Berichten zur Systemintegrität](service-fabric-understand-and-troubleshoot-with-system-health-reports.md)
### [Anzeigen von Integritätsberichten](service-fabric-view-entities-aggregated-health.md)

## Überwachen und Diagnostizieren
### [Überwachen und Diagnostizieren von Anwendungen](service-fabric-diagnostics-overview.md)
### Lokales Überwachen und Diagnostizieren von Diensten
#### [Windows](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md)
#### [Linux](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally-linux.md)
### Azure-Diagnoseprotokolle
#### [Windows](service-fabric-diagnostics-how-to-setup-wad.md)
#### [Linux](service-fabric-diagnostics-how-to-setup-lad.md)
### [Sammeln von Protokollen in einem Dienstprozess](service-fabric-diagnostic-collect-logs-without-an-agent.md)
### [Diagnose für zustandsbehaftete Reliable Services](service-fabric-reliable-services-diagnostics.md)
### [Diagnose in Reliable Actors](service-fabric-reliable-actors-diagnostics.md)
### [Behandeln von Problemen mit dem lokalen Cluster](service-fabric-troubleshoot-local-cluster-setup.md)

# Referenz
## [PowerShell](/powershell/azure/overview?view=azureservicefabricps)
## [Java-API](/java/api/)
## [.NET](/dotnet/api/)
## [REST](/rest/api/servicefabric)

# Ressourcen
## [Allgemeine Fragen zu Service Fabric](service-fabric-common-questions.md)
## [Supportoptionen für Service Fabric](service-fabric-support.md)
## [Beispielcode](http://aka.ms/servicefabricsamples)
## [Lernpfad](https://azure.microsoft.com/documentation/learning-paths/service-fabric/)
## [Preise](https://azure.microsoft.com/pricing/details/service-fabric/)
## [Dienstupdates](https://azure.microsoft.com/updates/?product=service-fabric)
## [MSDN-Forum](https://social.msdn.microsoft.com/Forums/home?forum=AzureServiceFabric)
## [Videos](https://azure.microsoft.com/documentation/videos/index/?services=service-fabric)
