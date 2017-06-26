# Übersicht
## [Was ist Azure AD Connect?](active-directory-aadconnect.md)
## [Was ist Azure AD Connect-Synchronisierung?](active-directory-aadconnectsync-whatis.md)
### [Benutzer und Kontakte](active-directory-aadconnectsync-understanding-users-and-contacts.md)
### [Architektur](active-directory-aadconnectsync-understanding-architecture.md)
### [Deklarative Bereitstellung](active-directory-aadconnectsync-understanding-declarative-provisioning.md)
#### [Ausdrücke für die deklarative Bereitstellung](active-directory-aadconnectsync-understanding-declarative-provisioning-expressions.md)
### [Standardkonfiguration](active-directory-aadconnectsync-understanding-default-configuration.md)
## [Was ist Azure AD Connect und Verbund?](active-directory-aadconnectfed-whatis.md)


# Erste Schritte
## [Voraussetzungen](active-directory-aadconnect-prerequisites.md)
## [Installieren von Azure AD Connect](active-directory-aadconnect-select-installation.md)
### [Express-Einstellungen](active-directory-aadconnect-get-started-express.md)
### [Benutzerdefinierte Einstellungen](active-directory-aadconnect-get-started-custom.md)
### [Upgrade von DirSync](active-directory-aadconnect-dirsync-upgrade-get-started.md)
### [Upgrade von einer früheren Version](active-directory-aadconnect-upgrade-previous-version.md)


# Anleitung
## Planen und Entwerfen
### [Entwurfskonzepte](active-directory-aadconnect-design-concepts.md)
### [Topologien für Azure AD Connect](active-directory-aadconnect-topologies.md)
### [Active Directory-Verbunddienste in Azure](active-directory-aadconnect-azure-adfs.md)
### [Besondere Überlegungen zu Instanzen](active-directory-aadconnect-instances.md)
### [Wenn Sie bereits über Azure AD verfügen](active-directory-aadconnect-existing-tenant.md)
## [Verwalten von Azure AD Connect](active-directory-aadconnect-whats-next.md)
### [Erneuern von Zertifikaten für O365 und Azure AD](active-directory-aadconnect-o365-certs.md)
### [Aktivieren des Geräterückschreibens](active-directory-aadconnect-feature-device-writeback.md)
### [Anmeldeoptionen für Benutzer](active-directory-aadconnect-user-signin.md)
#### [Nahtloses einmaliges Anmelden](active-directory-aadconnect-sso.md)
##### [Schnellstart](active-directory-aadconnect-sso-quick-start.md)
##### [Wie funktioniert das?](active-directory-aadconnect-sso-how-it-works.md)
##### [Häufig gestellte Fragen](active-directory-aadconnect-sso-faq.md)
##### [Problembehandlung](active-directory-aadconnect-troubleshoot-sso.md)
#### [Passthrough-Authentifizierung](active-directory-aadconnect-pass-through-authentication.md)
##### [Schnellstart](active-directory-aadconnect-pass-through-authentication-quick-start.md)
##### [Aktuelle Einschränkungen](active-directory-aadconnect-pass-through-authentication-current-limitations.md)
##### [Wie funktioniert das?](active-directory-aadconnect-pass-through-authentication-how-it-works.md)
##### [Häufig gestellte Fragen](active-directory-aadconnect-pass-through-authentication-faq.md)
##### [Problembehandlung](active-directory-aadconnect-troubleshoot-pass-through-authentication.md)
### [Unterstützung mehrerer Domänen für den Verbund](active-directory-aadconnect-multiple-domains.md)
### [Automatisches Upgrade](active-directory-aadconnect-feature-automatic-upgrade.md)
### [Verwenden eines SAML 2.0-Identitätsanbieters für einmaliges Anmelden](active-directory-aadconnect-federation-saml-idp.md)



## Verwalten der Azure AD Connect-Synchronisierung
### [Verhindern von versehentlichen Löschungen](active-directory-aadconnectsync-feature-prevent-accidental-deletes.md)
### [Kennwortsynchronisierung](active-directory-aadconnectsync-implement-password-synchronization.md)
### [Azure AD-Dienstkonto](active-directory-aadconnectsync-howto-azureadaccount.md)
### [Installations-Assistent](active-directory-aadconnectsync-installation-wizard.md)
### [Ändern der Standardkonfiguration](active-directory-aadconnectsync-best-practices-changing-default-configuration.md)
### [Konfigurieren der Filterung](active-directory-aadconnectsync-configure-filtering.md)
### [Scheduler](active-directory-aadconnectsync-feature-scheduler.md)
### [Verzeichniserweiterungen](active-directory-aadconnectsync-feature-directory-extensions.md)

### [Ändern des Kennworts für das Azure AD Sync-Dienstkonto](active-directory-aadconnectsync-change-serviceacct-pass.md)
### [Ändern des AD DS-Kontokennworts](active-directory-aadconnectsync-change-addsacct-pass.md)
### [Aktivieren des AD-Papierkorbs](active-directory-aadconnectsync-recycle-bin.md)

### [Synchronization Service Manager](active-directory-aadconnectsync-service-manager-ui.md)
#### [Vorgänge](active-directory-aadconnectsync-service-manager-ui-operations.md)
#### [Connectors](active-directory-aadconnectsync-service-manager-ui-connectors.md)
#### [Metaverse Designer](active-directory-aadconnectsync-service-manager-ui-mvdesigner.md)
#### [Metaverse Search](active-directory-aadconnectsync-service-manager-ui-mvsearch.md)


## Verwalten von Verbunddiensten
### [Verwalten und Anpassen](active-directory-aadconnect-federation-management.md)
### [Erstellen eines Verbunds mit mehreren Instanzen von Azure AD und einer Einzelinstanz von AD FS](active-directory-aadconnectfed-single-adfs-multitenant-federation.md)


## Problembehandlung
### [Konnektivität](active-directory-aadconnect-troubleshoot-connectivity.md)
### [Fehler während der Synchronisierung](active-directory-aadconnect-troubleshoot-sync-errors.md)
### [Objekt ist nicht synchronisiert.](active-directory-aadconnectsync-troubleshoot-object-not-syncing.md)
### [Kennwortsynchronisierung](active-directory-aadconnectsync-troubleshoot-password-synchronization.md)
### [Durch „userCertificate“ verursachter LargeObject-Fehler](active-directory-aadconnectsync-largeobjecterror-usercertificate.md)
### [Wiederherstellen der LocalDB-Obergrenze von 10 GB](active-directory-aadconnect-recover-from-localdb-10gb-limit.md)

# Referenz
## [Identitätssynchronisierung und Resilienz bei doppelten Attributen](active-directory-aadconnectsyncservice-duplicate-attribute-resiliency.md)
## [Erforderliche Ports und Protokolle für die Hybrid-Identität](active-directory-aadconnect-ports.md)
## [Features in der Vorschau](active-directory-aadconnect-feature-preview.md)
## [Versionsverlauf](active-directory-aadconnect-version-history.md)
## [Konten und Berechtigungen](active-directory-aadconnect-accounts-permissions.md)

## Azure AD Connect-Synchronisierung
### [Mit Azure Active Directory synchronisierte Attribute](active-directory-aadconnectsync-attributes-synchronized.md)
### [Connector – Versionsveröffentlichungsverlauf](active-directory-aadconnectsync-connector-version-history.md)
### [Funktionsreferenz](active-directory-aadconnectsync-functions-reference.md)
### [Operative Aufgaben und Überlegungen](active-directory-aadconnectsync-operations.md)
### [Azure AD-Verbund – Kompatibilitätsliste](active-directory-aadconnect-federation-compatibility.md)
### [Technische Konzepte](active-directory-aadconnectsync-technical-concepts.md)
### [Features des Diensts](active-directory-aadconnectsyncservice-features.md)




# Verwandte Themen
## [Überwachen Ihrer lokalen Identitätsinfrastruktur und Synchronisierung von Diensten in der Cloud](../connect-health/active-directory-aadconnect-health.md)
## [Hybrididentität – Entwurfshandbuch](https://azure.microsoft.com/documentation/articles/active-directory-hybrid-identity-design-considerations-overview/)


# Ressourcen
##[Häufig gestellte Fragen zu Azure AD Connect](active-directory-aadconnect-faq.md)
##[DirSync – Veraltet](active-directory-aadconnect-dirsync-deprecated.md)
