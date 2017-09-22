---
title: Bausteine eines Azure Active Directory-PoC-Playbooks | Microsoft-Dokumentation
description: "Untersuchen und schnelles Implementieren von Szenarien mit Identitäts- und Zugriffsverwaltung"
services: active-directory
keywords: Azure Active Directory, Playbook, Proof of Concept, PoC
documentationcenter: 
author: dstefanMSFT
manager: femila
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/04/2017
ms.author: dstefan
ms.translationtype: HT
ms.sourcegitcommit: 8f9234fe1f33625685b66e1d0e0024469f54f95c
ms.openlocfilehash: bdbdebe069b3150bed4aa26f1f6e677a66f75f32
ms.contentlocale: de-de
ms.lasthandoff: 09/20/2017

---
# <a name="azure-active-directory-proof-of-concept-playbook-building-blocks"></a>Azure Active Directory-PoC-Playbook: Bausteine

## <a name="catalog-of-roles"></a>Katalog mit Rollen

| Rolle | Beschreibung | Verantwortung für Proof of Concept (PoC) |
| --- | --- | --- |
| **Identitätsarchitektur / Entwicklerteam** | Dieses Team ist üblicherweise für den Entwurf der Lösung zuständig, implementiert Prototypen, erteilt Genehmigungen und übergibt fertige Produkte schließlich an das operative Geschäftsteam. | Das Entwicklerteam stellt die Umgebungen bereit und bewertet verschiedene Szenarien hinsichtlich der Verwaltbarkeit. |
| **Lokales Team für Identitätsvorgänge** | Dieses Team verwaltet vor Ort die verschiedenen Identitätsquellen: Active Directory-Gesamtstrukturen, LDAP-Verzeichnisse, Personalverwaltungssysteme und Verbundidentitätsanbieter. | Zugriff auf lokale Ressourcen, die in PoC-Szenarien erforderlich sind, wird von diesem Team bereitgestellt.<br/>Die Beteiligung dieses Teams sollte auf ein Mindestmaß beschränkt werden.|
| **Technische Anwendungsbesitzer** | Technische Besitzer der verschiedenen Cloud-Apps und -Dienste, die in Azure AD integriert werden | Stellen Einzelheiten zu SaaS-Anwendungen bereit (mögliche Testinstanzen) |
| **Globaler Azure AD-Administrator** | Verwaltet die Azure AD-Konfiguration | Stellt Anmeldeinformationen für das Konfigurieren des Synchronisierungsdiensts bereit. Während der PoC-Phase üblicherweise Teil des Identitätsarchitektur-Teams, in der operativen Phase aber eigenständig|
| **Datenbankteam** | Besitzer der Datenbankinfrastruktur | Gewährt für die Vorbereitung auf bestimmte Szenarien Zugriff auf die SQL-Umgebung (ADFS oder Azure AD Connect).<br/>Die Beteiligung dieses Teams sollte auf ein Mindestmaß beschränkt werden. |
| **Netzwerkteam** | Besitzer der Netzwerkinfrastruktur | Gewährt den erforderlichen Zugriff auf Netzwerkebene, damit die Synchronisierungsserver auf die erforderlichen Datenquellen und Clouddienste zugreifen können (Firewallregeln, geöffnete Ports, IPsec-Regeln usw.) |
| **Sicherheitsteam** | Definiert die Sicherheitsstrategie, analysiert Sicherheitsberichte aus verschiedenen Quellen und setzt Erkenntnisse in die Tat um. | Auswertung von Ziel-Sicherheitsszenarien |

## <a name="common-prerequisites-for-all-building-blocks"></a>Allgemeine Voraussetzungen für alle Bausteine

Diese Voraussetzungen gelten für Azure AD Premium-PoC in jedem Fall.

| Voraussetzung | Ressourcen |
| --- | --- |
| Definierter Azure AD-Mandant mit gültigem Azure-Abonnement | [Einrichten eines Azure Active Directory-Mandanten](active-directory-howto-tenant.md)<br/>**Hinweis:** Wenn Sie bereits über eine Umgebung mit Azure AD Premium-Lizenzen verfügen, können Sie unter https://aka.ms/accessaad ein Abonnement ohne Obergrenze abschließen. <br/>Weitere Informationen finden Sie unter: https://blogs.technet.microsoft.com/enterprisemobility/2016/02/26/azure-ad-mailbag-azure-subscriptions-and-azure-ad-2/ und https://technet.microsoft.com/library/dn832618.aspx |
| Definierte und überprüfte Domäne | [Hinzufügen eines benutzerdefinierten Domänennamens zu Azure Active Directory](active-directory-domains-add-azure-portal.md)<br/>**Hinweis:** Einige Workloads wie Power BI können einen Azure AD-Mandanten unbemerkt bereitgestellt haben. Um festzustellen, ob eine bestimmte Domäne einem Mandanten zugeordnet ist, navigieren Sie zu https://login.microsoftonline.com/{domain}/v2.0/.well-known/openid-configuration. Bei Erfolg ist die Domäne bereits einem Mandanten zugeordnet. Dann ist möglicherweise eine Übernahme nötig. Wenn dies der Fall ist, hilft Microsoft Ihnen gerne weiter. Weitere Informationen zu den Übernahmeoptionen finden Sie unter: [Was ist die Self-Service-Registrierung für Azure?](active-directory-self-service-signup.md) |
| Aktivierte Testversion von Azure AD Premium oder EMS | [Azure Active Directory Premium einen Monat lang gratis](https://azure.microsoft.com/trial/get-started-active-directory/) |
| PoC-Benutzern zugewiesene Azure AD Premium- oder EMS-Lizenzen | [Lizenzieren von sich selbst und Ihrer Benutzer in Azure Active Directory](active-directory-licensing-get-started-azure-portal.md) |
| Anmeldeinformationen eines globalen Azure AD-Administrators | [Zuweisen von Administratorrollen in Azure Active Directory](active-directory-assign-admin-roles-azure-portal.md) |
| Optional, jedoch dringend empfohlen: parallele Laborumgebung als Fallback | [Voraussetzungen für Azure AD Connect](./connect/active-directory-aadconnect-prerequisites.md) |

## <a name="directory-synchronization---password-hash-sync-phs---new-installation"></a>Verzeichnissynchronisierung – Kennworthashsynchronisierung – Neue Installation

Geschätzter Zeitaufwand: 60 Minuten für unter 1.000 PoC-Benutzer

### <a name="pre-requisites"></a>Voraussetzungen

| Voraussetzung | Ressourcen |
| --- | --- |
| Server zum Ausführen von Azure AD Connect | [Voraussetzungen für Azure AD Connect](./connect/active-directory-aadconnect-prerequisites.md) |
| Ausrichtung auf PoC-Benutzer in der gleichen Domäne, die Teil einer Sicherheitsgruppe sind, und auf Organisationseinheiten | [Benutzerdefinierte Installation von Azure AD Connect](./connect/active-directory-aadconnect-get-started-custom.md#domain-and-ou-filtering) |
| Ermitteln der für den PoC erforderlichen Azure AD Connect-Features | [Herstellen einer Verbindung zwischen Active Directory und Azure Active Directory – Konfigurieren der Synchronisierungsfunktionen](./connect/active-directory-aadconnect.md#configure-sync-features) |
| Besorgen aller notwendigen Anmeldeinformationen für die lokalen und die Cloudumgebungen  | [Azure AD Connect: Konten und Berechtigungen](./connect/active-directory-aadconnect-accounts-permissions.md) |

### <a name="steps"></a>Schritte

| Schritt | Ressourcen |
| --- | --- |
| Herunterladen der aktuellen Version von Azure AD Connect | [Herunterladen von Microsoft Azure Active Directory Connect](https://www.microsoft.com/download/details.aspx?id=47594) |
| Azure AD Connect auf die einfachste Weise installieren: Express <br/>1. Filtern Sie nach der Zielorganisationseinheit, um den Zeitbedarf des Synchronisierungszyklus zu minimieren.<br/>2. Wählen Sie die Zielbenutzer in der lokalen Gruppe aus.<br/>3. Stellen Sie die für den PoC erforderlichen Features bereit. | [Azure AD Connect: Benutzerdefinierte Installation: Filterung von Domänen und Organisationseinheiten](./connect/active-directory-aadconnect-get-started-custom.md#domain-and-ou-filtering) <br/>[Azure AD Connect: Benutzerdefinierte Installation: Gruppenbasierte Filterung](./connect/active-directory-aadconnect-get-started-custom.md#sync-filtering-based-on-groups)<br/>[Azure AD Connect: Integrieren Ihrer lokalen Identitäten in Azure Active Directory: Konfigurieren der Synchronisierungsfunktionen](./connect/active-directory-aadconnect.md#configure-sync-features) |
| Öffnen Sie die Azure AD Connect-Benutzeroberfläche und zeigen Sie die ausgeführten Profile an (Import, Synchronisation und Export). | [Azure AD Connect Sync: Scheduler](./connect/active-directory-aadconnectsync-feature-scheduler.md) |
| Öffnen Sie das [Azure AD-Verwaltungsportal](https://ms.portal.azure.com/#blade/Microsoft_AAD_IAM/UserManagementMenuBlade/), wechseln Sie zum Blatt „Alle Benutzer“ und fügen Sie die Spalte „Autoritätsquelle“ hinzu. Die Benutzer werden mit der Markierung „Windows Server AD“ angezeigt. | [Azure AD-Verwaltungsportal](https://portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/Overview) |

### <a name="considerations"></a>Überlegungen

1. Die Überlegungen zur Sicherheit der Kennworthashsynchronisierung [finden Sie hier](./connect/active-directory-aadconnectsync-implement-password-synchronization.md).  Wenn die Kennworthashsynchronisierung für Pilotbenutzer keine Option ist, können Sie diese Alternativen in Betracht ziehen:
   * Erstellen Sie Testbenutzer in der Produktionsdomäne. Stellen Sie sicher, dass kein anderes Konto synchronisiert wird.
   * Migrieren Sie zu einer UAT-Umgebung.
2.  Wenn Sie im Verbund arbeiten möchten, sollten Sie die Kosten einer Verbundlösung in Verbindung mit einem lokalen Identitätsanbieter, die nach dem PoC entstehen, kennen und mit den erstrebten Vorteilen abwiegen:
    * Eine solche Lösung ist auf dem kritischen Pfad, sodass Sie für Hochverfügbarkeit planen müssen.
    * Eine solche Lösung umfasst einen lokalen Dienst, der einen Kapazitätsplan erfordert.
    * Eine solche Lösung umfasst einen lokalen Dienst, der überwacht/verwaltet/gepatcht werden muss.

Erfahren Sie mehr: [Grundlegendes zu Office 365-Identitäten und Azure Active Directory – Verbundidentität](https://support.office.com/article/Understanding-Office-365-identity-and-Azure-Active-Directory-06a189e7-5ec6-4af2-94bf-a22ea225a7a9#bk_federated)

## <a name="branding"></a>Branding

Geschätzter Zeitaufwand: 15 Minuten

### <a name="pre-requisites"></a>Voraussetzungen

| Voraussetzung | Ressourcen |
| --- | --- |
| Assets (Bilder, Logos usw.): Achten Sie darauf, dass Assets die richtige Größe für die Visualisierung haben. | [Hinzufügen eines Unternehmensbrandings zur Anmeldeseite in Azure Active Directory](active-directory-branding-custom-signon-azure-portal.md) |
| Optional: Wenn in der Umgebung ein AD FS-Server vorhanden ist, greifen Sie auf den Server zu, um das Webthema anzupassen. | [AD FS Anmelde-Anpassung durch den Benutzer](https://technet.microsoft.com/windows-server-docs/identity/ad-fs/operations/ad-fs-user-sign-in-customization) |
| Clientcomputer für die Anmeldung als Endbenutzer |  |
| Optional: Mobilgeräte zum Testen als Mobilbenutzer |  |

### <a name="steps"></a>Schritte

| Schritt | Ressourcen |
| --- | --- |
| Wechseln Sie zum Azure AD-Verwaltungsportal. | [Azure AD-Verwaltungsportal: Unternehmensbranding](https://portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/LoginTenantBranding) |
| Laden Sie die Assets für die Anmeldeseite hoch (Herologo, kleines Logo, Label usw.). Optional: Wenn Sie über AD FS verfügen, passen Sie diese Assets an die AD FS-Anmeldeseiten an. | [Hinzufügen Ihres Unternehmensbrandings zur Anmelde- und Zugriffsbereichsseite: Anpassbare Elemente](active-directory-add-company-branding.md) |
| Warten Sie einige Minuten, bis die Änderung vollständig wirksam wird. |  |
| Melden Sie sich mit den PoC-Benutzeranmeldeinformationen unter https://myapps.microsoft.com an. |  |
| Überprüfen Sie das Aussehen und Verhalten im Browser. | [Hinzufügen Ihres Unternehmensbrandings zur Anmelde- und Zugriffsbereichsseite](active-directory-add-company-branding.md) |
| Optional: Überprüfen Sie das Aussehen und Verhalten auf anderen Geräten. |  |

### <a name="considerations"></a>Überlegungen

Wenn das bisherige Aussehen und Verhalten nach der Anpassung bestehen bleibt, leeren Sie den Browser-Clientcache, und wiederholen Sie den Vorgang.

## <a name="group-based-licensing"></a>Gruppenbasierte Lizenzierung

Geschätzter Zeitaufwand: 10 Minuten

### <a name="pre-requisites"></a>Voraussetzungen

| Voraussetzung | Ressourcen |
| --- | --- |
| Alle PoC-Benutzer sind Teil der Sicherheitsgruppe (Cloud oder lokal). | [Erstellen einer Gruppe und Hinzufügen von Mitgliedern in Azure Active Directory](active-directory-groups-create-azure-portal.md) |

### <a name="steps"></a>Schritte

| Schritt | Ressourcen |
| --- | --- |
| Wechseln Sie zum Blatt „Lizenzen“ im Azure AD-Verwaltungsportal. | [Azure AD-Verwaltungsportal: Lizenzierung](https://portal.azure.com/#blade/Microsoft_AAD_IAM/LicensesMenuBlade/Products) |
| Weisen Sie die Lizenzen der Sicherheitsgruppe mit PoC-Benutzern zu. | [Zuweisen von Lizenzen zu einer Gruppe von Benutzern in Azure Active Directory](active-directory-licensing-group-assignment-azure-portal.md) |

### <a name="considerations"></a>Überlegungen

Bei Problemen finden Sie hier Unterstützung: [Szenarien, Einschränkungen und bekannte Probleme mit der Verwendung von Gruppen zum Verwalten der Lizenzierung in Azure Active Directory](active-directory-licensing-group-advanced.md)

## <a name="saas-federated-sso-configuration"></a>SSO-Konfiguration mit SaaS-Verbund

Geschätzter Zeitaufwand: 60 Minuten

### <a name="pre-requisites"></a>Voraussetzungen

| Voraussetzung | Ressourcen |
| --- | --- |
| Testumgebung für die SaaS-Anwendung ist verfügbar. In diesem Handbuch verwendet wir ServiceNow als Beispiel.<br/>Es wird dringend empfohlen, eine Testinstanz zu verwenden, um Unstimmigkeiten beim Navigieren mit der vorhandenen Datenqualität und den Zuordnungen zu minimieren. | Testinstanz aufsetzen: https://developer.servicenow.com/app.do#!/home |
| Administratorzugriff auf die Verwaltungskonsole für ServiceNow | [Tutorial: Azure Active Directory-Integration mit ServiceNow](active-directory-saas-servicenow-tutorial.md) |
| Zielbenutzer, denen Sie die Anwendung zuweisen können. Es wird empfohlen, die PoC-Benutzer in einer Sicherheitsgruppe zusammenzufassen. <br/>Wenn die Erstellung der Gruppe nicht möglich ist, weisen Sie die Benutzer für den PoC direkt der Anwendung zu. | [Zuweisen eines Benutzers oder einer Gruppe zu einer Unternehmens-App in Azure Active Directory](active-directory-coreapps-assign-user-azure-portal.md) |

### <a name="steps"></a>Schritte

| Schritt | Ressourcen |
| --- | --- |
| Teilen Sie das Tutorial über die Microsoft-Dokumentation mit allen Akteuren.  | [Tutorial: Azure Active Directory-Integration mit ServiceNow](active-directory-saas-servicenow-tutorial.md) |
| Setzen Sie ein Arbeitstreffen an, und gehen Sie das Tutorial mit jedem Akteur durch. | [Tutorial: Azure Active Directory-Integration mit ServiceNow](active-directory-saas-servicenow-tutorial.md) |
| Weisen Sie die App der in den Voraussetzungen identifizierten Gruppe zu. Wenn der PoC in diesem Bereich bedingten Zugriff erfordert, können Sie später darauf zurückkommen, um MFA u.ä. hinzuzufügen. <br/>Hinweis: Dadurch wird der Bereitstellungsvorgang gestartet (wenn konfiguriert). |  [Zuweisen eines Benutzers oder einer Gruppe zu einer Unternehmens-App in Azure Active Directory](active-directory-coreapps-assign-user-azure-portal.md) <br/>[Erstellen einer Gruppe und Hinzufügen von Mitgliedern in Azure Active Directory](active-directory-groups-create-azure-portal.md) |
| Verwenden Sie das Azure AD-Verwaltungsportal, um die ServiceNow-Anwendung über den Katalog hinzuzufügen.| [Azure AD-Verwaltungsportal: Unternehmensanwendungen](https://portal.azure.com/#blade/Microsoft_AAD_IAM/StartboardApplicationsMenuBlade/Overview) <br/>[Neuerungen bei der Verwaltung von Unternehmensanwendungen in Azure Active Directory](active-directory-enterprise-apps-whats-new-azure-portal.md) |
| Aktivieren Sie in der ServiceNow-App auf dem Blatt „Einmaliges Anmelden“ die Einstellung „SAML-basierte Anmeldung“. |  |
| Füllen Sie die Felder „Anmelde-URL“ und „Bezeichner“ mit der ServiceNow-URL aus.<br/>Aktivieren Sie das Kontrollkästchen „Neues Zertifikat aktivieren“.<br/>Speichern Sie die Einstellungen. |  |
| Öffnen Sie das Blatt „ServiceNow konfigurieren“, das sich im Bereich unten befindet, um spezielle Anweisungen für die Konfiguration von ServiceNow abzurufen. |  |
| Führen Sie die Anweisungen zum Konfigurieren von ServiceNow aus. |  |
| Aktivieren Sie auf dem Blatt „Bereitstellung“ der ServiceNow-App die Bereitstellungsform „Automatisch“. | [Verwalten der Benutzerkontobereitstellung für Unternehmens-Apps im neuen Azure-Portal](active-directory-enterprise-apps-manage-provisioning.md) |
| Warten Sie einige Minuten, während die Bereitstellung abgeschlossen wird.  In der Zwischenzeit können Sie die Bereitstellungsberichte überprüfen. |  |
| Melden Sie sich unter https://myapps.microsoft.com/ als Testbenutzer (Zugriff erforderlich) an. | [Was ist der Zugriffsbereich?](active-directory-saas-access-panel-introduction.md) |
| Klicken Sie auf die Kachel für die Anwendung, die gerade erstellt wurde. Bestätigen Sie den Zugriff. |  |
| Optional: Überprüfen Sie die Nutzungsberichte der Anwendung. Hinweis: Die Zugriffe werden in den Berichten erst nach einer gewissen Wartezeit angezeigt. | [Berichte zu Anmeldeaktivitäten im Azure Active Directory-Portal: Nutzung von verwalteten Anwendungen](active-directory-reporting-activity-sign-ins.md#usage-of-managed-applications)<br/>[Aufbewahrungsrichtlinien für Azure Active Directory-Berichte](active-directory-reporting-retention.md) |

### <a name="considerations"></a>Überlegungen

1. Das [Tutorial](active-directory-saas-servicenow-tutorial.md) oben bezieht sich auf die bisherigen Azure AD-Verwaltungsfunktionen. PoC basiert allerdings auf der [Schnellstart](active-directory-enterprise-apps-whats-new-azure-portal.md#quick-start-get-going-with-your-new-application-right-away)-Verwaltung.
2. Wenn die Anwendung nicht im Katalog vorhanden ist, können Sie die Option „Bring Your Own App“ verwenden. Weitere Informationen: [Neuerungen bei der Verwaltung von Unternehmensanwendungen in Azure Active Directory: Hinzufügen von benutzerdefinierten Anwendungen von einem Ort aus](active-directory-enterprise-apps-whats-new-azure-portal.md#add-custom-applications-from-one-place)

## <a name="saas-password-sso-configuration"></a>SaaS-Konfiguration für Kennwort-SSO

Geschätzter Zeitaufwand: 15 Minuten

### <a name="pre-requisites"></a>Voraussetzungen

| Voraussetzung | Ressourcen |
| --- | --- |
| Testumgebung für SaaS-Anwendungen. Beispiele für Kennwort-SSO sind HipChat und Twitter. Für andere Anwendungen benötigen Sie die genaue URL der Seite, auf der sich das HTML-Anmeldeformular befindet. | [Twitter auf Microsoft Azure Marketplace](https://azuremarketplace.microsoft.com/marketplace/apps/aad.twitter)<br/>[HipChat auf Microsoft Azure Marketplace](https://azuremarketplace.microsoft.com/marketplace/apps/aad.hipchat) |
| Testkonten für die Anwendungen. | [Registrieren bei Twitter](https://twitter.com/signup?lang=en)<br/>[Kostenlos registrieren bei HipChat](https://www.hipchat.com/sign_up) |
| Zielbenutzer, denen Sie die Anwendung zuweisen können. Es wird empfohlen, die Benutzer in einer Sicherheitsgruppe zusammenzufassen. | [Zuweisen eines Benutzers oder einer Gruppe zu einer Unternehmens-App in Azure Active Directory](active-directory-coreapps-assign-user-azure-portal.md) |
| Lokaler Administratorzugriff auf einen Computer zum Bereitstellen der Zugriffsbereichserweiterung für Internet Explorer, Chrome oder Firefox | [Zugriffsbereichserweiterung für IE](https://account.activedirectory.windowsazure.com/Applications/Installers/x64/Access%20Panel%20Extension.msi)<br/>[Zugriffsbereichserweiterung für Chrome](https://go.microsoft.com/fwLink/?LinkID=311859&clcid=0x409)<br/>[Zugriffsbereichserweiterung für Firefox](https://go.microsoft.com/fwLink/?LinkID=626998&clcid=0x409) |

### <a name="steps"></a>Schritte

| Schritt | Ressourcen |
| --- | --- |
| Installieren der Browsererweiterung | [Zugriffsbereichserweiterung für IE](https://account.activedirectory.windowsazure.com/Applications/Installers/x64/Access%20Panel%20Extension.msi)<br/>[Zugriffsbereichserweiterung für Chrome](https://go.microsoft.com/fwLink/?LinkID=311859&clcid=0x409)<br/>[Zugriffsbereichserweiterung für Firefox](https://go.microsoft.com/fwLink/?LinkID=626998&clcid=0x409) |
| Konfigurieren der Anwendung über den Katalog | [Neuerungen bei der Verwaltung von Unternehmensanwendungen in Azure Active Directory: Neuer und verbesserter Anwendungskatalog](active-directory-enterprise-apps-whats-new-azure-portal.md#improvements-to-the-azure-active-directory-application-gallery) |
| Konfigurieren von Kennwort-SSO | [Verwalten des Features für einmaliges Anmelden für Unternehmens-Apps im neuen Azure-Portal: Kennwortbasierte Anmeldung](active-directory-enterprise-apps-manage-sso.md#password-based-sign-on) |
| Weisen Sie die App der in den Voraussetzungen identifizierten Gruppe zu. | [Zuweisen eines Benutzers oder einer Gruppe zu einer Unternehmens-App in Azure Active Directory](active-directory-coreapps-assign-user-azure-portal.md) |
| Melden Sie sich unter https://myapps.microsoft.com/ als Testbenutzer (Zugriff erforderlich) an. |  |
| Klicken Sie auf die Kachel für die Anwendung, die gerade erstellt wurde. | [Was ist der Zugriffsbereich?: Kennwortbasiertes SSO ohne Identitätsbereitstellung](active-directory-saas-access-panel-introduction.md#password-based-sso-without-identity-provisioning) |
| Eingeben der Anwendungsanmeldeinformationen | [Was ist der Zugriffsbereich?: Kennwortbasiertes SSO ohne Identitätsbereitstellung](active-directory-saas-access-panel-introduction.md#password-based-sso-without-identity-provisioning) |
| Schließen Sie den Browser, und wiederholen Sie den Anmeldevorgang. Zu diesem Zeitpunkt sollte der Benutzer die Anwendung direkt öffnen können. |  |
| Optional: Überprüfen Sie die Nutzungsberichte der Anwendung. Hinweis: Die Zugriffe werden in den Berichten erst nach einer gewissen Wartezeit angezeigt. | [Berichte zu Anmeldeaktivitäten im Azure Active Directory-Portal: Nutzung von verwalteten Anwendungen](active-directory-reporting-activity-sign-ins.md#usage-of-managed-applications)<br/>[Aufbewahrungsrichtlinien für Azure Active Directory-Berichte](active-directory-reporting-retention.md) |

### <a name="considerations"></a>Überlegungen

Wenn die Anwendung nicht im Katalog vorhanden ist, können Sie die Option „Bring Your Own App“ verwenden. Weitere Informationen: [Neuerungen bei der Verwaltung von Unternehmensanwendungen in Azure Active Directory: Hinzufügen von benutzerdefinierten Anwendungen von einem Ort aus](active-directory-enterprise-apps-whats-new-azure-portal.md#add-custom-applications-from-one-place)

 Berücksichtigen Sie die folgenden Anforderungen:
   * Die Anmelde-URL der Anwendung sollte bekannt sein.
   * Die Anmeldeseite sollte ein HTML-Formular mit mindestens einem Textfeld umfassen, das die Browsererweiterungen automatisch ausfüllen kann. Mindestens sollte es den Benutzernamen und das Kennwort enthalten.

## <a name="saas-shared-accounts-configuration"></a>SaaS-Konfiguration für gemeinsam genutzte Konten

Geschätzter Zeitaufwand: 30 Minuten

### <a name="pre-requisites"></a>Voraussetzungen

| Voraussetzung | Ressourcen |
| --- | --- |
| Halten Sie die Liste der Zielanwendungen und die genauen Anmelde-URLs bereit. Als Beispiel können Sie Twitter verwenden. | [Twitter auf Microsoft Azure Marketplace](https://azuremarketplace.microsoft.com/marketplace/apps/aad.twitter)<br/>[Registrieren bei Twitter](https://twitter.com/signup?lang=en) |
| Freigegebene Anmeldeinformationen für diese SaaS-Anwendung. | [Gemeinsame Nutzung von Konten mit Azure AD](active-directory-sharing-accounts.md)<br/>[Automatisierte Azure AD-Kennwortrollover für Facebook, Twitter und LinkedIn ab sofort in der Vorschau! – Enterprise Mobility and Security Blog] (https://blogs.technet.microsoft.com/enterprisemobility/2015/02/20/azure-ad-automated-password-roll-over-for-facebook-twitter-and-linkedin-now-in-preview/) |
| Anmeldeinformationen für mindestens zwei Teammitglieder, die auf das gleiche Konto zugreifen werden. Sie müssen einer Sicherheitsgruppe angehören. | [Zuweisen eines Benutzers oder einer Gruppe zu einer Unternehmens-App in Azure Active Directory](active-directory-coreapps-assign-user-azure-portal.md) |
| Lokaler Administratorzugriff auf einen Computer zum Bereitstellen der Zugriffsbereichserweiterung für Internet Explorer, Chrome oder Firefox | [Zugriffsbereichserweiterung für IE](https://account.activedirectory.windowsazure.com/Applications/Installers/x64/Access%20Panel%20Extension.msi)<br/>[Zugriffsbereichserweiterung für Chrome](https://go.microsoft.com/fwLink/?LinkID=311859&clcid=0x409)<br/>[Zugriffsbereichserweiterung für Firefox](https://go.microsoft.com/fwLink/?LinkID=626998&clcid=0x409) |

### <a name="steps"></a>Schritte

| Schritt | Ressourcen |
| --- | --- |
| Installieren der Browsererweiterung | [Zugriffsbereichserweiterung für IE](https://account.activedirectory.windowsazure.com/Applications/Installers/x64/Access%20Panel%20Extension.msi)<br/>[Zugriffsbereichserweiterung für Chrome](https://go.microsoft.com/fwLink/?LinkID=311859&clcid=0x409)<br/>[Zugriffsbereichserweiterung für Firefox](https://go.microsoft.com/fwLink/?LinkID=626998&clcid=0x409) |
| Konfigurieren der Anwendung über den Katalog | [Neuerungen bei der Verwaltung von Unternehmensanwendungen in Azure Active Directory: Neuer und verbesserter Anwendungskatalog](active-directory-enterprise-apps-whats-new-azure-portal.md#improvements-to-the-azure-active-directory-application-gallery) |
| Konfigurieren von Kennwort-SSO | [Verwalten des Features für einmaliges Anmelden für Unternehmens-Apps im neuen Azure-Portal: Kennwortbasierte Anmeldung](active-directory-enterprise-apps-manage-sso.md#password-based-sign-on) |
| Weisen Sie die App der in den Voraussetzungen identifizierten Gruppe zu, während Sie ihnen Anmeldeinformationen zuweisen. | [Zuweisen eines Benutzers oder einer Gruppe zu einer Unternehmens-App in Azure Active Directory](active-directory-coreapps-assign-user-azure-portal.md) |
| Melden Sie sich als verschiedene Benutzer an, die auf die App als **das gleiche freigegebene Konto** zugreifen.  |  |
| Optional: Überprüfen Sie die Nutzungsberichte der Anwendung. Hinweis: Die Zugriffe werden in den Berichten erst nach einer gewissen Wartezeit angezeigt. | [Berichte zu Anmeldeaktivitäten im Azure Active Directory-Portal: Nutzung von verwalteten Anwendungen](active-directory-reporting-activity-sign-ins.md#usage-of-managed-applications)<br/>[Aufbewahrungsrichtlinien für Azure Active Directory-Berichte](active-directory-reporting-retention.md) |


### <a name="considerations"></a>Überlegungen

Wenn die Anwendung nicht im Katalog vorhanden ist, können Sie die Option „Bring Your Own App“ verwenden. Weitere Informationen: [Neuerungen bei der Verwaltung von Unternehmensanwendungen in Azure Active Directory: Hinzufügen von benutzerdefinierten Anwendungen von einem Ort aus](active-directory-enterprise-apps-whats-new-azure-portal.md#add-custom-applications-from-one-place)

 Berücksichtigen Sie die folgenden Anforderungen:
   * Die Anmelde-URL der Anwendung sollte bekannt sein.
   * Die Anmeldeseite sollte ein HTML-Formular mit mindestens einem Textfeld umfassen, das die Browsererweiterungen automatisch ausfüllen kann. Mindestens sollte es den Benutzernamen und das Kennwort enthalten.

## <a name="app-proxy-configuration"></a>App-Proxykonfiguration

Geschätzter Zeitaufwand: 20 Minuten

### <a name="pre-requisites"></a>Voraussetzungen

| Voraussetzung | Ressourcen |
| --- | --- |
| Ein Basic- oder Premium-Abonnement für Microsoft Azure AD und ein Azure AD-Verzeichnis, für das Sie als globaler Administrator fungieren | [Azure Active Directory-Editionen](active-directory-editions.md) |
| Eine lokal gehostete Webanwendung, die Sie für Remotezugriff konfigurieren möchten |  |
| Einen Server, auf dem Windows Server 2012 R2 bzw. Windows 8.1 oder höher installiert ist und auf dem Sie den Anwendungsproxy-Connector installieren können | [Grundlegendes zu Azure AD-Anwendungsproxyconnectors](application-proxy-understand-connectors.md) |
| Wenn der Pfad durch eine Firewall geschützt ist, sollten Sie sich vergewissern, dass diese so konfiguriert ist, dass der Connector HTTPS-Anforderungen (TCP) an den Anwendungsproxy richten kann. | [Aktivieren des Anwendungsproxys über das Azure-Portal: Voraussetzungen für den Anwendungsproxy](active-directory-application-proxy-enable.md#application-proxy-prerequisites) |
| Wenn in Ihrer Organisation Proxyserver zum Herstellen einer Verbindung mit dem Internet verwendet werden, helfen Ihnen die Details zur Konfiguration im Blogbeitrag „Working with existing on-premises proxy servers“ (Verwenden von vorhandenen lokalen Proxyservern) weiter. | [Verwenden von vorhandenen lokalen Proxyservern](application-proxy-working-with-proxy-servers.md) |


### <a name="steps"></a>Schritte

| Schritt | Ressourcen |
| --- | --- |
| Installieren eines Connectors auf dem Server | [Aktivieren des Anwendungsproxys über das Azure-Portal: Installieren und Registrieren des Connectors](active-directory-application-proxy-enable.md#install-and-register-a-connector) |
| Veröffentlichen Sie die lokale Anwendung in Azure AD als Anwendungsproxy. | [Veröffentlichen von Anwendungen mit Azure AD-Anwendungsproxy](application-proxy-publish-azure-portal.md) |
| Zuweisen der Testbenutzer | [Veröffentlichen von Anwendungen mit Azure AD-Anwendungsproxy: Hinzufügen eines Testbenutzers](application-proxy-publish-azure-portal.md#add-a-test-user) |
| Optional: Konfigurieren Sie das einmalige Anmelden für die Benutzer. | [Ermöglichen des einmaligen Anmeldens mit dem Azure AD-Anwendungsproxy](application-proxy-sso-azure-portal.md) |
| Testen Sie die App, indem Sie sich im Portal „Meine Apps“ als zugewiesener Benutzer anmelden. | https://myapps.microsoft.com |

### <a name="considerations"></a>Überlegungen

1. Es wird empfohlen, den Connector in das Unternehmensnetzwerk aufzunehmen. In einigen Fällen ist die Leistung allerdings besser, wenn Sie ihn in der Cloud platzieren. Weitere Informationen: [Aspekte der Netzwerktopologie bei Verwendung des Azure Active Directory-Anwendungsproxys](application-proxy-network-topology-considerations.md)
2. Diese Remotezugriffslösung unterhält nur ausgehende Verbindungen und ist dadurch besonders sicher. Weitere Informationen zur Sicherheit finden Sie unter: [Sicherheitsaspekte beim Remotezugriff auf Apps mit dem Azure AD-Anwendungsproxy](application-proxy-security-considerations.md)

## <a name="generic-ldap-connector-configuration"></a>Konfiguration des generischen LDAP-Connectors

Geschätzter Zeitaufwand: 60 Minuten

> [!IMPORTANT]
> Dies ist eine erweiterte Konfiguration, für die sich der Benutzer etwas mit FIM/MIM auskennen muss. Bei Verwendung dieser Konfiguration in der Produktion sollten Fragen dazu an den [Premier Support](https://support.microsoft.com/premier) gerichtet werden.

### <a name="pre-requisites"></a>Voraussetzungen

| Voraussetzung | Ressourcen |
| --- | --- |
| Azure AD Connect ist installiert und konfiguriert. | Baustein: [Verzeichnissynchronisierung – Kennworthashsynchronisierung](#directory-synchronization--password-hash-sync-phs--new-installation) |
| Die ADLDS-Instanz entspricht den Anforderungen. | [Technische Referenz für den generischen LDAP-Connector: Übersicht über den generischen LDAP-Connector](./connect/active-directory-aadconnectsync-connector-genericldap.md#overview-of-the-generic-ldap-connector) |
| Sie verfügen über eine Liste der Workloads der Benutzer und der Attribute dieser Workloads. | [Azure AD Connect-Synchronisierung: Mit Azure Active Directory synchronisierte Attribute](./connect/active-directory-aadconnectsync-attributes-synchronized.md) |


### <a name="steps"></a>Schritte

| Schritt | Ressourcen |
| --- | --- |
| Hinzufügen eines generischen LDAP-Connectors | [Technische Referenz für den generischen LDAP-Connector: Erstellen eines neuen Connectors](./connect/active-directory-aadconnectsync-connector-genericldap.md#create-a-new-connector) |
| Erstellen Sie Ausführungsprofile für den erstellten Connector (vollständiger Import, Deltaimport, vollständige Synchronisierung, Deltasynchronisierung, Export). | [Create a Management Agent Run Profile](https://technet.microsoft.com/library/jj590219(v=ws.10).aspx) (Erstellen eines Management-Agent-Ausführungsprofils)<br/> [Verwenden von Connectors mit dem Azure AD Connect Synchronization Service Manager](./connect/active-directory-aadconnectsync-service-manager-ui-connectors.md)|
| Führen Sie ein Profil für den vollständigen Import aus. Überprüfen Sie, dass sich im Connectorbereich Objekte befinden. | [Search for a Connector Space Object](https://technet.microsoft.com/library/jj590287(v=ws.10).aspx) (Suchen nach einem Objekt des Connectorbereichs)<br/>[Verwenden von Connectors mit dem Azure AD Connect Synchronization Service Manager: Connectorbereich durchsuchen](./connect/active-directory-aadconnectsync-service-manager-ui-connectors.md#search-connector-space) |
| Erstellen Sie Synchronisierungsregeln, damit die Objekte im Metaverse über die erforderlichen Attribute für die Workloads verfügen. | [Azure AD Connect-Synchronisierung: Bewährte Methoden zum Ändern der Standardkonfiguration: Änderungen an Synchronisierungsregeln](./connect/active-directory-aadconnectsync-best-practices-changing-default-configuration.md#changes-to-synchronization-rules)<br/>[Azure AD Connect-Synchronisierung: Grundlegendes zur deklarativen Bereitstellung](./connect/active-directory-aadconnectsync-understanding-declarative-provisioning.md)<br/>[Azure AD Connect-Synchronisierung: Grundlegendes zu Ausdrücken für die deklarative Bereitstellung](./connect/active-directory-aadconnectsync-understanding-declarative-provisioning-expressions.md) |
| Starten Sie den vollständigen Synchronisierungszyklus. | [Azure AD Connect Sync: Scheduler: Starten des Schedulers](./connect/active-directory-aadconnectsync-feature-scheduler.md#start-the-scheduler) |
| Führen Sie bei Problemen die Problembehandlung aus. | [Problembehandlung: Ein Objekt wird nicht mit Azure AD synchronisiert](./connect/active-directory-aadconnectsync-troubleshoot-object-not-syncing.md) |
| Überprüfen Sie, ob der LDAP-Benutzer sich anmelden und auf die Anwendung zugreifen kann. | https://myapps.microsoft.com |

### <a name="considerations"></a>Überlegungen

> [!IMPORTANT]
> Dies ist eine erweiterte Konfiguration, für die sich der Benutzer etwas mit FIM/MIM auskennen muss. Bei Verwendung dieser Konfiguration in der Produktion sollten Fragen dazu an den [Premier Support](https://support.microsoft.com/premier) gerichtet werden.

## <a name="groups---delegated-ownership"></a>Gruppen – Delegierte Eigentümerschaft

Geschätzter Zeitaufwand: 10 Minuten

### <a name="pre-requisites"></a>Voraussetzungen

| Voraussetzung | Ressourcen |
| --- | --- |
| Die SaaS-Anwendung (Verbund-SSO oder Kennwort-SSO) wurde bereits konfiguriert. | Baustein: [SSO-Konfiguration mit SaaS-Verbund](#saas-federated-sso-configuration) |
| Die Cloudgruppe, der unter 1. Zugriff auf die Anwendung gewährt wurde, wurde identifiziert. | Baustein: [SSO-Konfiguration mit SaaS-Verbund](#saas-federated-sso-configuration) <br/>[Erstellen einer Gruppe und Hinzufügen von Mitgliedern in Azure Active Directory](active-directory-groups-create-azure-portal.md) |
| Anmeldeinformationen sind für den Besitzer der Gruppe verfügbar. | [Verwalten des Zugriffs auf Ressourcen mit Azure Active Directory-Gruppen](active-directory-manage-groups.md) |
| Die Anmeldeinformationen für den Information-Worker, der auf die Apps zugreifen soll, sind bekannt. | [Was ist der Zugriffsbereich?](active-directory-saas-access-panel-introduction.md) |


### <a name="steps"></a>Schritte

| Schritt | Ressourcen |
| --- | --- |
| Identifizieren Sie die Gruppe, der Zugriff auf die Anwendung gewährt wurde, und konfigurieren Sie deren Besitzer.| [Verwalten der Einstellungen für eine Gruppe in Azure Active Directory](active-directory-groups-settings-azure-portal.md) |
| Melden Sie sich als Besitzer der Gruppe an, und zeigen Sie die Gruppenmitgliedschaft auf der Registerkarte „Gruppen“ im Zugriffsbereich an. | [Azure Active Directory-Gruppenverwaltungsseite](https://account.activedirectory.windowsazure.com/r/#/groups) |
| Fügen Sie den Information-Worker hinzu, den Sie testen möchten. |  |
| Melden Sie sich als Information-Worker an. Vergewissern Sie sich, dass die Kachel verfügbar ist. | [Was ist der Zugriffsbereich?](active-directory-saas-access-panel-introduction.md) |

### <a name="considerations"></a>Überlegungen

Wenn die Bereitstellung für die Anwendung aktiviert ist, müssen Sie möglicherweise einige Minuten warten, bis die Bereitstellung abgeschlossen ist, bevor Sie als Information-Worker auf die Anwendung zugreifen können.

## <a name="saas-and-identity-lifecycle"></a>SaaS und Identitätslebenszyklus

### <a name="pre-requisites"></a>Voraussetzungen

| Voraussetzung | Ressourcen |
| --- | --- |
| Die SaaS-Anwendung (Verbund-SSO oder Kennwort-SSO) wurde bereits konfiguriert. | Baustein: [SSO-Konfiguration mit SaaS-Verbund](#saas-federated-sso-configuration) |
| Die Cloudgruppe, der unter 1. Zugriff auf die Anwendung gewährt wurde, wurde identifiziert. | Baustein: [SSO-Konfiguration mit SaaS-Verbund](#saas-federated-sso-configuration) <br/>[Erstellen einer Gruppe und Hinzufügen von Mitgliedern in Azure Active Directory](active-directory-groups-create-azure-portal.md) |
| Die Anmeldeinformationen für den Information-Worker, der auf die Apps zugreifen soll, sind bekannt. | [Was ist der Zugriffsbereich?](active-directory-saas-access-panel-introduction.md) |


### <a name="steps"></a>Schritte

| Schritt | Ressourcen |
| --- | --- |
| Entfernen des Benutzers aus der Gruppe, der die App zugewiesen ist | [Verwalten der Gruppenmitgliedschaft für Benutzer in Ihrem Azure Active Directory-Mandanten](active-directory-groups-members-azure-portal.md) |
| Warten Sie einige Minuten bis zur Aufhebung der Bereitstellung. | [Automatisierte SaaS-Benutzerbereitstellung in Azure AD: Wie funktioniert die automatisierte Bereitstellung?](active-directory-saas-app-provisioning.md#how-does-automatic-provisioning-work) |
| Melden Sie sich in einer anderen Browsersitzung als der Information-Worker beim Portal „Meine Apps“ an, und bestätigen Sie, dass die Kachel fehlt. | http://myapps.microsoft.com |


### <a name="considerations"></a>Überlegungen

Extrapolieren Sie das PoC-Szenario für Abwesende und Urlauber. Wenn der Benutzer im lokalen Active Directory deaktiviert oder daraus entfernt wird, besteht keine Möglichkeit der Anmeldung bei der SaaS-Anwendung mehr.

## <a name="self-service-access-to-application-management"></a>Self-Service-Zugriff auf Anwendungsverwaltung

Geschätzter Zeitaufwand: 10 Minuten

### <a name="pre-requisites"></a>Voraussetzungen

| Voraussetzung | Ressourcen |
| --- | --- |
| Ermitteln der PoC-Benutzer, die als Teil der Sicherheitsgruppe Zugriff auf die Anwendungen anfordern werden | Baustein: [SSO-Konfiguration mit SaaS-Verbund](#saas-federated-sso-configuration) |
| Die Zielanwendung ist bereitgestellt. | Baustein: [SSO-Konfiguration mit SaaS-Verbund](#saas-federated-sso-configuration) |

### <a name="steps"></a>Schritte

| Schritt | Ressourcen |
| --- | --- |
| Aufrufen des Blatts „Unternehmensanwendungen“ im Azure AD-Verwaltungsportal | [Azure AD-Verwaltungsportal: Unternehmensanwendungen](https://portal.azure.com/#blade/Microsoft_AAD_IAM/StartboardApplicationsMenuBlade/AllApps/menuId/) |
| Konfigurieren Sie die Anwendung aus den Voraussetzungen mit Self-Service. | [Neuerungen bei der Verwaltung von Unternehmensanwendungen in Azure Active Directory: Konfigurieren des Self-Service-Anwendungszugriffs](active-directory-enterprise-apps-whats-new-azure-portal.md#configure-self-service-application-access) |
| Melden Sie sich als Information-Worker im Portal „Meine Apps“ an. | http://myapps.microsoft.com |
| Beachten Sie die Schaltfläche „+App hinzufügen“ oben auf der Seite. Über diese Schaltfläche können Sie auf die App zugreifen. |  |

### <a name="considerations"></a>Überlegungen

Die ausgewählten Anwendungen verfügen möglicherweise über Bereitstellungsanforderungen. Wenn Sie die App direkt aufrufen, kann es daher zu Fehlern kommen. Wenn die ausgewählte Anwendung die Bereitstellung über Azure AD unterstützt und konfiguriert ist, können Sie bei dieser Gelegenheit den gesamten Flow vorführen. Siehe den Baustein [SSO-Konfiguration mit SaaS-Verbund](#saas-federated-sso-configuration) für weitere Empfehlungen.

## <a name="self-service-password-reset"></a>Self-Service-Kennwortzurücksetzung

Geschätzter Zeitaufwand: 15 Minuten

### <a name="pre-requisites"></a>Voraussetzungen

| Voraussetzung | Ressourcen |
| --- | --- |
| Aktivieren Sie die Self-Service-Kennwortverwaltung in Ihrem Mandanten. | [Azure Active Directory – Zurücksetzen von Kennwörtern für IT-Administratoren](active-directory-passwords.md) |
| Aktivieren Sie das Kennwortrückschreiben, um Kennwörter lokal zu verwalten. Hinweis: Dies erfordert bestimmte Azure AD Connect-Versionen. | [Voraussetzungen für das Zurückschreiben von Kennwörtern](active-directory-passwords-writeback.md) |
| Identifizieren Sie die PoC-Benutzer, die diese Funktion verwenden werden, und stellen Sie sicher, dass sie Mitglied einer Sicherheitsgruppe sind. Bei diesen Benutzern muss es sich um Nicht-Administratoren handeln, da die Funktion sonst nicht vollständig vorgestellt werden kann. | [Anpassen: Azure AD-Kennwortverwaltung: Zugriff auf die Kennwortrücksetzung beschränken](active-directory-passwords-writeback.md) |


### <a name="steps"></a>Schritte

| Schritt | Ressourcen |
| --- | --- |
| Navigieren Sie zu „Azure AD-Verwaltungsportal: Kennwortzurücksetzung“. | [Azure AD-Verwaltungsportal: Kennwortzurücksetzung](https://portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/PasswordReset) |
| Legen Sie die Richtlinie für die Kennwortzurücksetzung fest. Für den PoC können Sie Telefonanrufe und Q&A verwenden. Es wird empfohlen, die Pflichtregistrierung für die Anmeldung beim Zugriffsbereich zu aktivieren. |  |
| Melden Sie sich ab und als ein Information-Worker an. |  |
| Geben Sie die Self-Service-Kennwortzurücksetzungsdaten an, die Sie in Schritt 2 konfiguriert haben. | http://aka.ms/ssprsetup |
| Schließen Sie den Browser. |  |
| Melden Sie sich erneut an, und zwar als der Information-Worker, den Sie in Schritt 4 verwendet haben. |  |
| Setzen Sie das Kennwort zurück. | [Aktualisieren Ihres eigenen Kennworts: Zurücksetzen des eigenen Kennworts](active-directory-passwords-update-your-own-password.md) |
| Versuchen Sie, sich mit dem neuen Kennwort sowohl bei Azure AD als auch bei lokalen Ressourcen anzumelden. |  |

### <a name="considerations"></a>Überlegungen

1. Wenn ein Upgrade von Azure AD Connect Konflikte verursachen kann, ziehen Sie in Betracht, hierfür Cloudkonten zu verwenden oder eine Demo in einer separaten Umgebung zu erstellen.
2. Die Administratoren haben eine andere Richtlinie. Wenn Sie das Administratorkonto verwenden, um das Kennwort zurückzusetzen, kann das den PoC beeinträchtigen und Konflikte hervorrufen. Stellen Sie sicher, dass Sie ein normales Benutzerkonto verwenden, um die Zurücksetzungsvorgänge zu testen.


## <a name="azure-multi-factor-authentication-with-phone-calls"></a>Azure Multi-Factor Authentication mithilfe von Telefonanrufen

Geschätzter Zeitaufwand: 10 Minuten

### <a name="pre-requisites"></a>Voraussetzungen

| Voraussetzung | Ressourcen |
| --- | --- |
| Ermitteln der PoC-Benutzer, die MFA verwenden werden  |  |
| Sie benötigen ein Telefon mit gutem Empfang für die MFA-Überprüfung.  | [Was ist Azure Multi-Factor Authentication?](../multi-factor-authentication/multi-factor-authentication.md) |

### <a name="steps"></a>Schritte

| Schritt | Ressourcen |
| --- | --- |
| Navigieren Sie zum Blatt „Benutzer und Gruppen“ im Azure AD-Verwaltungsportal. | [Azure AD-Verwaltungsportal: Benutzer und Gruppen](https://portal.azure.com/#blade/Microsoft_AAD_IAM/UserManagementMenuBlade/Overview/menuId/) |
| Wechseln Sie zum Blatt „Alle Benutzer“. |  |
| Wählen Sie in der oberen Menüleiste die Schaltfläche „Multi-Factor Authentication“ aus. | Direkte URL für das Azure MFA-Portal: https://aka.ms/mfaportal |
| Wählen Sie in den Einstellungen für „Benutzer“ die PoC-Benutzer aus, und aktivieren Sie MFA für diese Benutzer. | [Benutzerstatus in Azure Multi-Factor Authentication](../multi-factor-authentication/multi-factor-authentication-get-started-user-states.md) |
| Melden Sie sich als der PoC-Benutzer an, und durchlaufen Sie den Nachweis-Prozess.  |  |

### <a name="considerations"></a>Überlegungen

1. Mit den PoC-Schritten in diesem Baustein aktivieren Sie MFA für den Benutzer bei allen Anmeldungen. Es gibt andere Tools wie „Bedingter Zugriff“ und „Schutz der Identität“, die MFA bei zielgerichteteren Szenarien verwenden. Darauf ist zu achten, wenn Sie vom PoC zur Produktion übergehen.
2. Die PoC-Schritte in diesem Baustein verwenden Telefonanrufe explizit aus Gründen der MFA-Zweckmäßigkeit. Beim Übergang vom PoC zur Produktion wird empfohlen, möglichst Anwendungen wie den [Microsoft Authenticator](../multi-factor-authentication/end-user/microsoft-authenticator-app-how-to.md) als zweiten Faktor zu verwenden.
Weitere Informationen: [DRAFT NIST Special Publication 800-63B](https://pages.nist.gov/800-63-3/sp800-63b.html)

## <a name="mfa-conditional-access-for-saas-applications"></a>Bedingter MFA-Zugriff für SaaS-Anwendungen

Geschätzter Zeitaufwand: 10 Minuten

### <a name="pre-requisites"></a>Voraussetzungen

| Voraussetzung | Ressourcen |
| --- | --- |
| Ermitteln Sie die PoC-Benutzer zum Ausrichten der Richtlinie. Diese Benutzer sollten Mitglied einer Sicherheitsgruppe sein, um den Umfang der Richtlinie für den bedingten Zugriff zu ermitteln. | [SSO-Konfiguration mit SaaS-Verbund](#saas-federated-sso-configuration) |
| Die SaaS-Anwendung wurde bereits konfiguriert. |  |
| Die PoC-Benutzer sind der Anwendung bereits zugewiesen. |  |
| Anmeldeinformationen für den POC-Benutzer sind verfügbar. |  |
| Der POC-Benutzer ist für MFA registriert. Verwenden Sie ein Telefon mit gutem Empfang. | http://aka.ms/ssprsetup |
| Gerät im internen Netzwerk Die IP-Adresse ist Teil des internen Adressraums. | IP-Adresse ermitteln: https://www.bing.com/search?q=what%27s+my+ip |
| Gerät im externen Netzwerk (z.B. ein Telefon im Mobilnetzwerk des Telefonanbieters) |  |

### <a name="steps"></a>Schritte

| Schritt | Ressourcen |
| --- | --- |
| Wechseln Sie zum Blatt „Azure AD-Verwaltungsportal: Bedingter Zugriff“. | [Azure AD-Verwaltungsportal: Bedingter Zugriff](https://portal.azure.com/#blade/Microsoft_AAD_IAM/ConditionalAccessBlade/Policies) |
| Erstellen Sie die Richtlinie für den bedingten Zugriff:<br/>- Ausrichtung auf PoC-Benutzer unter „Benutzer und Gruppen“<br/>- Ausrichtung auf PoC-Anwendung unter „Cloud-Apps“<br/>- Ausrichtung auf alle Standorte außer vertrauenswürdigen unter „Bedingungen“ > „Standorte“. **Hinweis:** Vertrauenswürdige IPs werden im [MFA-Portal](https://account.activedirectory.windowsazure.com/UserManagement/MfaSettings.aspx) konfiguriert.<br/>- Erfordern von Multi-Factor Authentication unter „Gewährung“ | [Erste Schritte mit dem bedingten Zugriff in Azure Active Directory: Schritte für die Richtlinienkonfiguration](active-directory-conditional-access-azure-portal-get-started.md#policy-configuration-steps) |
| Greifen Sie auf die Anwendung über das Unternehmensnetzwerk zu. | [Erste Schritte mit dem bedingten Zugriff in Azure Active Directory: Testen der Richtlinie](active-directory-conditional-access-azure-portal-get-started.md#testing-the-policy) |
| Greifen Sie auf die Anwendung über das öffentliche Netzwerk zu. | [Erste Schritte mit dem bedingten Zugriff in Azure Active Directory: Testen der Richtlinie](active-directory-conditional-access-azure-portal-get-started.md#testing-the-policy) |

### <a name="considerations"></a>Überlegungen

Bei einem Verbund können Sie einen lokalen Identitätsanbieter (IdP) verwenden, um den internen/externen Unternehmensnetzwerkstatus mit Ansprüchen zu übermitteln. Sie können diese Methode verwenden, ohne die Liste der IP-Adressen verwalten zu müssen, was in großen Unternehmen unübersichtlich und schwierig zu verwalten wäre. In diesem Setup benötigen Sie ein Konto für das Szenario „Netzwerkroaming“ (wenn ein Benutzer im internen Netzwerk angemeldet ist und dann für eine Kaffeepause das Haus verlässt). Es wird empfohlen, sich mit den Folgen vertraut zu machen. Weitere Informationen: [Sichern von Cloudressourcen mit Azure Multi-Factor Authentication und AD FS: Vertrauenswürdige IPs für Partnerbenutzer](../multi-factor-authentication/multi-factor-authentication-get-started-adfs-cloud.md#trusted-ips-for-federated-users)

## <a name="privileged-identity-management-pim"></a>Privileged Identity Management (PIM)

Geschätzter Zeitaufwand: 15 Minuten

### <a name="pre-requisites"></a>Voraussetzungen

| Voraussetzung | Ressourcen |
| --- | --- |
| Bestimmen Sie den globalen Administrator, der Teil des PoC für PIM wird. | [Erste Schritte mit Azure AD Privileged Identity Management](active-directory-privileged-identity-management-getting-started.md) |
| Bestimmen Sie den globalen Administrator, der Sicherheitsadministrator wird. | [Erste Schritte mit Azure AD Privileged Identity Management](active-directory-privileged-identity-management-getting-started.md)<br/> [Verschiedene Administratorrollen in Azure Active Directory PIM](active-directory-privileged-identity-management-roles.md) |
| Optional: Überprüfen Sie, ob die globalen Administratoren E-Mail-Zugriff haben, um E-Mail-Benachrichtigungen mit PIM zu testen. | [Was ist Azure AD Privileged Identity Management?: Konfigurieren der Rollenaktivierungseinstellungen](active-directory-privileged-identity-management-configure.md#configure-the-role-activation-settings)


### <a name="steps"></a>Schritte

| Schritt | Ressourcen |
| --- | --- |
| Melden Sie sich unter https://portal.azure.com als ein globaler Administrator (GA) an, und führen Sie den Bootstrap-Vorgang für das Blatt „PIM“ durch. Für den globalen Administrator, der diesen Schritt ausführt, erfolgt das Seeding als Sicherheitsadministrator.  Nennen wir diesen Akteur „GA1“. | [Verwenden des Sicherheits-Assistenten in Azure AD Privileged Identity Management](active-directory-privileged-identity-management-security-wizard.md) |
| Bestimmen Sie den globalen Administrator, und ändern Sie seine Rolle von „Permanent“ in „Berechtigt“. Dabei sollte es sich der Übersichtlichkeit halber um einen anderen Administrator als den in Schritt 1 handeln. Nennen wir diesen Akteur „GA2“. | [Azure AD Privileged Identity Management: Hinzufügen oder Entfernen einer Benutzerrolle](active-directory-privileged-identity-management-how-to-add-role-to-user.md)<br/>[Was ist Azure AD Privileged Identity Management?: Konfigurieren der Rollenaktivierungseinstellungen](active-directory-privileged-identity-management-configure.md#configure-the-role-activation-settings)  |
| Melden Sie sich jetzt als GA2 unter https://portal.azure.com an, und versuchen Sie, die „Benutzereinstellungen“ zu ändern. Hinweis: Einige Optionen sind abgeblendet. | |
| Navigieren Sie in der gleichen Sitzung wie in Schritt 3 auf einer neuen Registerkarte zu https://portal.azure.com, und fügen Sie das Blatt „PIM“ zum Dashboard hinzu. | [Aktivieren oder Deaktivieren von Rollen in Azure AD Privileged Identity Management: Hinzufügen der Anwendung Privileged Identity Management](active-directory-privileged-identity-management-how-to-activate-role.md#add-the-privileged-identity-management-application) |
| Fordern Sie die Aktivierung der Rolle „Globaler Administrator“ an. | [Aktivieren oder Deaktivieren von Rollen in Azure AD Privileged Identity Management: Aktivieren einer Rolle](active-directory-privileged-identity-management-how-to-activate-role.md#activate-a-role) |
| Hinweis: Wenn GA2 nicht für Azure MFA registriert wurde, muss dies jetzt nachgeholt werden. |  |
| Wechseln Sie zurück zur ursprünglichen Registerkarte (Schritt 3). Klicken Sie dann auf die Aktualisierungsschaltfläche des Browsers. Sie können die „Benutzereinstellungen“ jetzt ändern. | |
| Optional: Wenn E-Mail für die globalen Administratoren aktiviert ist, können Sie den Posteingang von GA1 und GA2 überprüfen. Dort finden Sie eine Benachrichtigung darüber, dass die Rolle aktiviert wurde. |  |
| 8 Überprüfen Sie im Überwachungsverlauf, ob die Beförderung von GA2 im Bericht angezeigt wird. | [Was ist Azure AD Privileged Identity Management?: Überprüfen der Rollenaktivität](active-directory-privileged-identity-management-configure.md#review-role-activity) |

### <a name="considerations"></a>Überlegungen

Diese Funktion ist Teil von Azure AD Premium P2 und/oder EMS E5.

## <a name="discovering-risk-events"></a>Ermitteln von Risikoereignissen

Geschätzter Zeitaufwand: 20 Minuten

### <a name="pre-requisites"></a>Voraussetzungen

| Voraussetzung | Ressourcen |
| --- | --- |
| Gerät mit heruntergeladenem und installiertem Tor-Browser | [Herunterladen des Tor-Browsers](https://www.torproject.org/projects/torbrowser.html.en#downloads) |
| Zugriff auf PoC-Benutzer für die Anmeldung | [Azure Active Directory Identity Protection-Playbook](active-directory-identityprotection-playbook.md) |

### <a name="steps"></a>Schritte

| Schritt | Ressourcen |
| --- | --- |
| Öffnen Sie den Tor-Browser. | [Herunterladen des Tor-Browsers](https://www.torproject.org/projects/torbrowser.html.en#downloads) |
| Melden Sie sich unter https://myapps.microsoft.com mit dem PoC-Benutzerkonto an. | [Azure Active Directory Identity Protection-Playbook: Simulieren von Risikoereignissen](active-directory-identityprotection-playbook.md#simulating-risk-events) |
| Warten Sie 5 bis 7 Minuten. |  |
| Melden Sie sich als globaler Administrator unter https://portal.azure.com an, und öffnen Sie das Blatt „Schutz der Identität“ (Identity Protection). | https://aka.ms/aadipgetstarted |
| Öffnen Sie das Blatt „Risikoereignisse“. Unter „Anmeldungen von anonymen IP-Adressen“ sollten Sie einen Eintrag sehen.  | [Azure Active Directory Identity Protection-Playbook: Simulieren von Risikoereignissen](active-directory-identityprotection-playbook.md#simulating-risk-events) |

### <a name="considerations"></a>Überlegungen

Diese Funktion ist Teil von Azure AD Premium P2 und/oder EMS E5.

## <a name="deploying-sign-in-risk-policies"></a>Bereitstellen von Richtlinien für Risiken bei der Anmeldung

Geschätzter Zeitaufwand: 10 Minuten

### <a name="pre-requisites"></a>Voraussetzungen

| Voraussetzung | Ressourcen |
| --- | --- |
| Gerät mit heruntergeladenem und installiertem Tor-Browser | [Herunterladen des Tor-Browsers](https://www.torproject.org/projects/torbrowser.html.en#downloads) |
| Zugriff als PoC-Benutzer zum Testen |  |
| Der PoC-Benutzer ist für MFA registriert. Verwenden Sie ein Telefon mit gutem Empfang. | Baustein: [Azure Multi-Factor Authentication mithilfe von Telefonanrufen](#azure-multi-factor-authentication-with-phone-calls) |


### <a name="steps"></a>Schritte

| Schritt | Ressourcen |
| --- | --- |
| Melden Sie sich als globaler Administrator unter https://portal.azure.com an, und öffnen Sie das Blatt „Schutz der Identität“ (Identity Protection). | https://aka.ms/aadipgetstarted |
| Aktivieren Sie eine Richtlinie zum Anmelderisiko:<br/>- Zugewiesen zu: PoC-Benutzer<br/>- Bedingungen: Anmelderisiko mittel oder höher (die Anmeldung von einem anonymen Standort aus wird als mittleres Risiko aufgefasst)<br/>- Steuerelemente: Anfordern von MFA | [Azure Active Directory Identity Protection-Playbook: Anmelderisiko](active-directory-identityprotection-playbook.md#sign-in-risk) |
| Öffnen Sie den Tor-Browser. | [Herunterladen des Tor-Browsers](https://www.torproject.org/projects/torbrowser.html.en#downloads) |
| Melden Sie sich unter https://myapps.microsoft.com mit dem PoC-Benutzerkonto an. |  |
| Beachten Sie die MFA-Überprüfung. | [Anmeldeverfahren von Azure AD Identity Protection: Wiederherstellung riskanter Anmeldungen](active-directory-identityprotection-flows.md#risky-sign-in-recovery)

### <a name="considerations"></a>Überlegungen

Diese Funktion ist Teil von Azure AD Premium P2 und/oder EMS E5. Weitere Informationen zu Risikoereignissen finden Sie unter: [Azure Active Directory-Risikoereignisse](active-directory-reporting-risk-events.md)

## <a name="configuring-certificate-based-authentication"></a>Konfigurieren der zertifikatbasierten Authentifizierung

Geschätzter Zeitaufwand: 20 Minuten

### <a name="pre-requisites"></a>Voraussetzungen

| Voraussetzung | Ressourcen |
| --- | --- |
| Gerät mit Benutzerzertifikat (Windows, iOS oder Android) über Unternehmens-PKI bereitgestellt | [Bereitstellen von Benutzerzertifikaten](https://msdn.microsoft.com/library/cc770857.aspx) |
| Azure AD-Domäne im Verbund mit AD FS | [Azure AD Connect und Verbund](./connect/active-directory-aadconnectfed-whatis.md)<br/>[Active Directory-Zertifikatdienste: Übersicht](https://technet.microsoft.com/library/hh831740.aspx)|
| iOS-Geräte: Microsoft Authenticator-App installiert | [Erste Schritte mit der Microsoft Authenticator-App](../multi-factor-authentication/end-user/microsoft-authenticator-app-how-to.md) |

### <a name="steps"></a>Schritte

| Schritt | Ressourcen |
| --- | --- |
| Aktivieren Sie die Einstellung „Zertifikatauthentifizierung“ in AD FS. | [Configure Authentication Policies: To configure primary authentication globally in Windows Server 2012 R2](https://technet.microsoft.com/windows-server-docs/identity/ad-fs/operations/configure-authentication-policies#to-configure-primary-authentication-globally-in-windows-server-2012-r2) (Konfigurieren der Authentifizierungsrichtlinien: Globales Konfigurieren der primären Authentifizierung in Windows Server 2012 R2) |
| Optional: Aktivieren Sie die zertifikatbasierte Authentifizierung in Azure AD für Exchange Active Sync-Clients. | [Erste Schritte mit zertifikatbasierter Authentifizierung in Azure Active Directory](active-directory-certificate-based-authentication-get-started.md) |
| Navigieren Sie zum Zugriffsbereich und führen Sie die Authentifizierung mit einem Benutzerzertifikat aus. | https://myapps.microsoft.com |

### <a name="considerations"></a>Überlegungen

Weitere Informationen zu Nachteilen bei dieser Form der Bereitstellung finden Sie unter: [ADFS: Certificate Authentication with Azure AD & Office 365](https://blogs.msdn.microsoft.com/samueld/2016/07/19/adfs-certauth-aad-o365/) (AD FS: Zertifikatauthentifizierung mit Azure AD und Office 365)



> [!NOTE]
> Das Benutzerzertifikat sollte geschützt werden: entweder durch die Geräteverwaltung oder bei Smartcards mit PIN.



[!INCLUDE [active-directory-playbook-toc](../../includes/active-directory-playbook-steps.md)]

