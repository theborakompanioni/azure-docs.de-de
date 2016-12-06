# Architektur

## Cloudgrundlagen

### [Entwerfen robuster Anwendungen](guidance-resiliency-overview.md)
#### [Checkliste für Resilienz](guidance-resiliency-checklist.md)
#### [Fehlermodusanalyse](guidance-resiliency-failure-mode-analysis.md)
#### [Notfallwiederherstellung](..\resiliency\resiliency-disaster-recovery-azure-applications.md)
#### [Notfallwiederherstellung und hohe Verfügbarkeit](..\resiliency\resiliency-disaster-recovery-high-availability-azure-applications.md)
#### [Hohe Verfügbarkeit](..\resiliency\resiliency-high-availability-azure-applications.md)
#### [Checkliste für hohe Verfügbarkeit](..\resiliency\resiliency-high-availability-checklist.md)
#### [Leitfaden zur Resilienz von Diensten in Microsoft Azure](..\resiliency\resiliency-service-guidance-index.md)
#### [Wiederherstellung nach Datenbeschädigung oder unbeabsichtigtem Löschen](..\resiliency\resiliency-technical-guidance-recovery-data-corruption.md)
#### [Wiederherstellung nach lokalen Fehlern](..\resiliency\resiliency-technical-guidance-recovery-local-failures.md)
#### [Wiederherstellung nach einer regionsweiten Dienstunterbrechung](..\resiliency\resiliency-technical-guidance-recovery-loss-azure-region.md)
#### [Wiederherstellung von einem lokalen Standort nach Azure](..\resiliency\resiliency-technical-guidance-recovery-on-premises-azure.md)
#### [Technischer Leitfaden zur Resilienz in Azure](..\resiliency\resiliency-technical-guidance.md)


## Referenzarchitekturen

### [Ausführen von SQL Server-Workloads in Azure](guidance-ra-compute.md)
#### [Ausführen eines virtuellen Linux-Computers in Azure](guidance-compute-single-vm-linux.md)
#### [Ausführen eines virtuellen Windows-Computers in Azure](guidance-compute-single-vm.md)
#### [Ausführen mehrerer virtueller Computer in Azure zur Steigerung von Skalierbarkeit und Verfügbarkeit](guidance-compute-multi-vm.md)
#### [Ausführen virtueller Linux-Computer in einer Architektur mit n Ebenen](guidance-compute-n-tier-vm-linux.md)
#### [Ausführen virtueller Windows-Computer in einer Architektur mit n Ebenen](guidance-compute-n-tier-vm.md)
#### [Ausführen virtueller Linux-Computer in mehreren Regionen für hohe Verfügbarkeit](guidance-compute-multiple-datacenters-linux.md)
#### [Ausführen virtueller Windows-Computer in mehreren Regionen für hohe Verfügbarkeit](guidance-compute-multiple-datacenters.md)

### [Verbinden Ihres lokalen Netzwerks mit Azure](guidance-ra-hybrid-networking.md)
#### [Hybrid-Netzwerkarchitektur mit Azure ExpressRoute](guidance-hybrid-network-expressroute.md)
#### [Hybrid-Netzwerkarchitektur mit Azure und lokalem VPN](guidance-hybrid-network-vpn.md)
#### [Hoch verfügbare Hybrid-Netzwerkarchitektur](guidance-hybrid-network-expressroute-vpn-failover.md)

### [Schützen der Cloudgrenze in Azure](guidance-ra-network-security.md)
#### [Sichere Hybrid-Netzwerkarchitektur in Azure](guidance-iaas-ra-secure-vnet-hybrid.md)
#### [DMZ zwischen Azure und dem Internet](guidance-iaas-ra-secure-vnet-dmz.md)

### [Verwalten von Identitäten in Azure](guidance-ra-identity.md)
#### [Implementieren von Microsoft Azure Active Directory](guidance-identity-aad.md)
#### [Erweitern von ADDS auf Azure](guidance-identity-adds-extend-domain.md)
#### [Erstellen einer ADDS-Ressourcengesamtstruktur in Azure](guidance-identity-adds-resource-forest.md)
#### [Implementieren von ADFS in Azure](guidance-identity-adfs.md)

### [Webanwendungsarchitekturen für Azure App Service](guidance-ra-app-service.md)
#### [Einfache Webanwendung](guidance-web-apps-basic.md)
#### [Webanwendung mit hoher Verfügbarkeit](guidance-web-apps-multi-region.md)
#### [Verbessern der Skalierbarkeit in einer Webanwendung](guidance-web-apps-scalability.md)


## Bewährte Methoden für Cloudanwendungen

### [Leitfaden zum API-Design](..\best-practices-api-design.md)
### [API-Implementierungsleitfaden](..\best-practices-api-implementation.md)
### [Anleitungen für die automatische Skalierung](..\best-practices-auto-scaling.md)
### [Checkliste für Verfügbarkeit](..\best-practices-availability-checklist.md)
### [Anleitungen für Hintergrundaufträge](..\best-practices-background-jobs.md)
### [Azure-Regionspaare](..\best-practices-availability-paired-regions.md)
### [Anleitungen zum Caching](..\best-practices-caching.md)
### [Anleitungen zum Content Delivery Network](..\best-practices-cdn.md)
### [Anleitungen zur Datenpartitionierung](..\best-practices-data-partitioning.md)
### [Anleitungen zu Überwachung und Diagnose](..\best-practices-monitoring.md)
### [Microsoft-Clouddienste und Netzwerksicherheit](..\best-practices-network-security.md)
### [Muster für das Entwerfen von Azure Resource Manager-Vorlagen](..\best-practices-resource-manager-design-templates.md)
### [Empfohlene Benennungskonventionen für Azure-Ressourcen](guidance-naming-conventions.md)
### [Sicherheitsaspekte für Azure-Ressourcen-Manager](..\best-practices-resource-manager-security.md)
### [Freigeben des Status in Azure-Ressourcen-Manager-Vorlagen](..\best-practices-resource-manager-state.md)
### [Anleitungen zum Wiederholen von Vorgängen](..\best-practices-retry-general.md)
### [Anleitungen zu dienstspezifischen Wiederholungsmechanismen](..\best-practices-retry-service-specific.md)
### [Checkliste für Skalierbarkeit](..\best-practices-scalability-checklist.md)


## Anleitungen zu Szenarien

### [Anleitungen für Elasticsearch für Azure](guidance-elasticsearch.md)
#### [Ausführen von Elasticsearch in Azure](guidance-elasticsearch-running-on-azure.md)
#### [Optimieren der Datenerfassungsleistung](guidance-elasticsearch-tuning-data-ingestion-performance.md)
#### [Optimieren der Datenaggregations- und abfrageleistung](guidance-elasticsearch-tuning-data-aggregation-and-query-performance.md)
#### [Konfigurieren der Resilienz und Wiederherstellung](guidance-elasticsearch-configuring-resilience-and-recovery.md)
#### [Erstellen einer Umgebung für Leistungstests](guidance-elasticsearch-creating-performance-testing-environment.md)
#### [Implementieren eines JMeter-Testplans](guidance-elasticsearch-implementing-jmeter-test-plan.md)
#### [Bereitstellen eines JMeter JUnit-Samplers](guidance-elasticsearch-deploying-jmeter-junit-sampler.md)
#### [Ausführen der automatisierten Resilienztests](guidance-elasticsearch-running-automated-resilience-tests.md)
#### [Ausführen der automatisierten Leistungstests](guidance-elasticsearch-running-automated-performance-tests.md)

### [Identitätsverwaltung für mehrmandantenfähige Anwendungen](guidance-multitenant-identity.md)
#### [Übersicht](guidance-multitenant-identity-intro.md)
#### [Tailspin-Anwendung „Surveys“](guidance-multitenant-identity-tailspin.md)
#### [Authentifizierung mithilfe von Azure AD und OpenID Connect](guidance-multitenant-identity-authenticate.md)
#### [Anspruchsbasierte Identitäten](guidance-multitenant-identity-claims.md)
#### [Anmeldung und Onboarding von Mandanten](guidance-multitenant-identity-signup.md)
#### [Anwendungsrollen](guidance-multitenant-identity-app-roles.md)
#### [Rollenbasierte und ressourcenbasierte Autorisierung](guidance-multitenant-identity-authorize.md)
#### [Schützen einer Back-End-Web-API](guidance-multitenant-identity-web-api.md)
#### [Zwischenspeichern von Zugriffstoken](guidance-multitenant-identity-token-cache.md)
#### [Verbund mit den AD FS eines Kunden](guidance-multitenant-identity-adfs.md)
#### [Abrufen von Zugriffstoken mithilfe der Clientassertion](guidance-multitenant-identity-client-assertion.md)
#### [Verwenden von Azure Key Vault zum Schützen geheimer Anwendungsschlüssel](guidance-multitenant-identity-keyvault.md)
#### [Bereitstellen virtueller Geräte mit hoher Verfügbarkeit](guidance-nva-ha.md)


<!--HONumber=Nov16_HO4-->


