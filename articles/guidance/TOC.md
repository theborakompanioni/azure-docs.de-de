# Architektur

## Cloudgrundlagen

### [Entwickeln robuster Anwendungen für Azure](guidance-resiliency-overview.md)
#### [Checkliste für Resilienz](guidance-resiliency-checklist.md)
#### [Fehlermodusanalyse](guidance-resiliency-failure-mode-analysis.md)

#### [Notfallwiederherstellung für in Microsoft Azure erstellte Anwendungen](..\resiliency\resiliency-disaster-recovery-azure-applications.md)
#### [Notfallwiederherstellung und hohe Verfügbarkeit für in Microsoft Azure erstellte Anwendungen](..\resiliency\resiliency-disaster-recovery-high-availability-azure-applications.md)
#### [Hohe Verfügbarkeit für in Microsoft Azure erstellte Anwendungen](..\resiliency\resiliency-high-availability-azure-applications.md)
#### [Checkliste für hohe Verfügbarkeit](..\resiliency\resiliency-high-availability-checklist.md)
#### [Leitfaden zur Resilienz von Diensten in Microsoft Azure](..\resiliency\resiliency-service-guidance-index.md)
#### [Wiederherstellung nach Datenbeschädigung oder unbeabsichtigtem Löschen](..\resiliency\resiliency-technical-guidance-recovery-data-corruption.md)
#### [Wiederherstellung nach lokalen Fehlern in Azure](..\resiliency\resiliency-technical-guidance-recovery-local-failures.md)
#### [Wiederherstellung nach einer regionsweiten Dienstunterbrechung](..\resiliency\resiliency-technical-guidance-recovery-loss-azure-region.md)
#### [Wiederherstellung von einem lokalen Standort nach Azure](..\resiliency\resiliency-technical-guidance-recovery-on-premises-azure.md)
#### [Technischer Leitfaden zur Resilienz in Azure](..\resiliency\resiliency-technical-guidance.md)


## Referenzarchitekturen

### Computereferenzarchitektur
#### [Ausführen einer Linux-VM in Azure](guidance-compute-single-vm-linux.md)
#### [Ausführen einer Windows-VM in Azure](guidance-compute-single-vm.md)
#### [Ausführen mehrerer VMs in Azure zur Steigerung von Skalierbarkeit und Verfügbarkeit](guidance-compute-multi-vm.md)
#### [Ausführen virtueller Linux-Computer in einer Azure-Architektur mit n Ebenen](guidance-compute-n-tier-vm-linux.md)
#### [Ausführen virtueller Windows-Computer in einer Azure-Architektur mit n Ebenen](guidance-compute-n-tier-vm.md)
#### [Ausführen virtueller Linux-Computer in mehreren Regionen für hohe Verfügbarkeit](guidance-compute-multiple-datacenters-linux.md)
#### [Ausführen virtueller Windows-Computer in mehreren Regionen für hohe Verfügbarkeit](guidance-compute-multiple-datacenters.md)

### [Verbinden Ihres lokalen Netzwerks mit Azure](guidance-connecting-your-on-premises-network-to-azure.md)
#### [Implementieren einer sicheren Hybrid-Netzwerkarchitektur mit Azure ExpressRoute](guidance-hybrid-network-expressroute.md)
#### [Implementieren einer sicheren Hybrid-Netzwerkarchitektur mit Azure und lokalem VPN](guidance-hybrid-network-vpn.md)
#### [Implementieren einer hoch verfügbaren Hybrid-Netzwerkarchitektur](guidance-hybrid-network-expressroute-vpn-failover.md)

### Schützen der Cloudgrenze in Azure
#### [Implementieren einer sicheren Hybrid-Netzwerkarchitektur in Azure](guidance-iaas-ra-secure-vnet-hybrid.md)
#### [Implementieren einer DMZ zwischen Azure und dem Internet](guidance-iaas-ra-secure-vnet-dmz.md)

### [Verwalten von Identitäten in Azure](guidance-ra-identity.md)
#### [Implementieren von Azure Active Directory](guidance-identity-aad.md)
#### [Erweitern von Active Directory Directory Services (AD DS) auf Azure](guidance-identity-adds-extend-domain.md)
#### [Erstellen einer AD DS-Ressourcengesamtstruktur (Active Directory Directory Services) in Azure](guidance-identity-adds-resource-forest.md)
#### [Implementieren von Active Directory-Verbunddienste (AD FS) in Azure](guidance-identity-adfs.md)

### Referenzarchitektur für PaaS-Webanwendung
#### [Azure-Referenzarchitektur: Einfache Webanwendung](guidance-web-apps-basic.md)
#### [Azure-Referenzarchitektur: Webanwendung mit hoher Verfügbarkeit](guidance-web-apps-multi-region.md)
#### [Verbessern der Skalierbarkeit in einer Webanwendung](guidance-web-apps-scalability.md)


## Cloudentwurfsmuster

## Bewährte Methoden für Cloudanwendungen

### [Leitfaden zum API-Design](..\best-practices-api-design.md)
### [API-Implementierungsleitfaden](..\best-practices-api-implementation.md)
### [Anleitungen für die automatische Skalierung](..\best-practices-auto-scaling.md)
### [Checkliste für Verfügbarkeit](..\best-practices-availability-checklist.md)
### [Anleitungen für Hintergrundaufträge](..\best-practices-background-jobs.md)
### [Geschäftskontinuität und Notfallwiederherstellung: Azure-Regionspaare](..\best-practices-availability-paired-regions.md)
### [Anleitungen zum Caching](..\best-practices-caching.md)
### [Anleitungen zum Content Delivery Network (CDN)](..\best-practices-cdn.md)
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
#### [Optimierung der Datenerfassungsleistung für Elasticsearch in Azure](guidance-elasticsearch-tuning-data-ingestion-performance.md)
#### [Optimieren der Datenaggregation und Abfrageleistung mit Elasticsearch in Azure](guidance-elasticsearch-tuning-data-aggregation-and-query-performance.md)
#### [Konfigurieren der Resilienz und Wiederherstellung unter Elasticsearch in Azure](guidance-elasticsearch-configuring-resilience-and-recovery.md)
#### [Erstellen einer Leistungstestumgebung für Elasticsearch in Azure](guidance-elasticsearch-creating-performance-testing-environment.md)
#### [Implementieren eines JMeter-Testplans für Elasticsearch](guidance-elasticsearch-implementing-jmeter-test-plan.md)
#### [Erstellen und Bereitstellen eines JMeter-JUnit-Samplers zum Testen der Leistung von Elasticsearch](guidance-elasticsearch-deploying-jmeter-junit-sampler.md)
#### [Ausführen der automatisierten Elasticsearch-Resilienztests](guidance-elasticsearch-running-automated-resilience-tests.md)
#### [Ausführen der automatisierten Elasticsearch-Leistungstests](guidance-elasticsearch-running-automated-performance-tests.md)

### [Identitätsverwaltung für mehrinstanzenfähige Anwendungen in Microsoft Azure](guidance-multitenant-identity.md)
#### [Einführung in die Identitätsverwaltung für mehrinstanzenfähige Anwendungen in Microsoft Azure](guidance-multitenant-identity-intro.md)
#### [Informationen über die Tailspin-Anwendung Surveys](guidance-multitenant-identity-tailspin.md)
#### [Authentifizierung in mehrinstanzenfähigen Apps mithilfe von Azure AD und OpenID Connect](guidance-multitenant-identity-authenticate.md)
#### [Arbeiten mit anspruchsbasierten Identitäten in mehrinstanzenfähigen Anwendungen](guidance-multitenant-identity-claims.md)
#### [Registrierung und Onboarding von Mandanten in einer mehrinstanzenfähigen Anwendung](guidance-multitenant-identity-signup.md)
#### [Anwendungsrollen in mehrinstanzenfähigen Anwendungen](guidance-multitenant-identity-app-roles.md)
#### [Rollen- und ressourcenbasierte Autorisierung in mehrinstanzenfähigen Anwendungen](guidance-multitenant-identity-authorize.md)
#### [Schützen einer Back-End-Web-API in einer mehrinstanzenfähigen Anwendung](guidance-multitenant-identity-web-api.md)
#### [Zwischenspeichern von Zugangstoken in einer mehrinstanzenfähigen Anwendung](guidance-multitenant-identity-token-cache.md)
#### [Einrichten eines Verbunds mit den Active Directory-Verbunddiensten (AD FS) eines Kunden für mehrinstanzenfähige Apps in Azure](guidance-multitenant-identity-adfs.md)
#### [Abrufen von Zugriffstoken aus Azure AD mithilfe der Clientassertion](guidance-multitenant-identity-client-assertion.md)
#### [Verwenden von Azure Key Vault zum Schützen geheimer Anwendungsschlüssel](guidance-multitenant-identity-keyvault.md)

#### [Bereitstellen virtueller Geräte mit hoher Verfügbarkeit](guidance-nva-ha.md)


<!--HONumber=Nov16_HO2-->


