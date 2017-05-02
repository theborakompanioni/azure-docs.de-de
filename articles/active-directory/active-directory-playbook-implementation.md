---
title: Implementierung eines Azure Active Directory-POC-Playbooks | Microsoft-Dokumentation
description: "Untersuchen und schnelles Implementieren von Szenarien mit Identitäts- und Zugriffsverwaltung"
services: active-directory
keywords: Azure Active Directory, Playbook, Proof of Concept, POC
documentationcenter: 
author: dstefanMSFT
manager: asuthar
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 4/12/2017
ms.author: dstefan
translationtype: Human Translation
ms.sourcegitcommit: 785d3a8920d48e11e80048665e9866f16c514cf7
ms.openlocfilehash: 298ff9b851f604b89e5672e41ab112b67990b0d6
ms.lasthandoff: 04/12/2017


---
# <a name="azure-active-directory-proof-of-concept-playbook-implementation"></a>Azure Active Directory-POC-Playbook: Implementierung

## <a name="foundation---syncing-ad-to-azure-ad"></a>Grundlage – Synchronisieren von AD mit Azure AD 

Eine Hybrididentität ist die Grundlage für die meisten Unternehmenskunden, die bereits ein lokales Verzeichnis besitzen. Das Ziel dieses Artikels besteht ganz bewusst darin, in möglichst kurzer Zeit die Vorteile der tatsächlichen Identitäts- und Zugriffsszenarien zu vermitteln. 

| Szenario | Bausteine| 
| --- | --- |  
| [Erweitern Ihrer lokalen Identität auf die Cloud](#extending-your-on-premises-identity-to-the-cloud) | [Verzeichnissynchronisierung – Kennworthashsynchronisierung](active-directory-playbook-building-blocks.md#directory-synchronization---password-hash-sync-phs---new-installation) <br/>**Hinweis:** Falls Sie bereits DirSync/ADSync oder ältere Versionen von Azure AD Connect besitzen, ist dieser Schritt optional. Für einige Szenarien in diesem Handbuch ist möglicherweise eine neuere Version von Azure AD Connect erforderlich.  <br/>[Branding](active-directory-playbook-building-blocks.md#branding) | 
| [Zuweisen von Azure AD-Lizenzen mithilfe von Gruppen](#assigning-azure-ad-licenses-using-groups) | [Gruppenbasierte Lizenzierung](active-directory-playbook-building-blocks.md#group-based-licensing) |


### <a name="extending-your-on-premises-identity-to-the-cloud"></a>Erweitern Ihrer lokalen Identität auf die Cloud 

1. Bob ist der Active Directory-Administrator bei Contoso. Ihm wird aufgetragen, Identity as a Service für eine Gruppe von Benutzern zu aktivieren. Nach der Ausführung des Azure AD Connect-Assistenten ist die Identität der Zielbenutzer in der Cloud verfügbar. 
2. Bob bittet die Zielbenutzerin Susie, den Azure Active Directory-Zugriffsbereich zu öffnen und zu überprüfen, ob sie authentifiziert werden kann. Susie sieht eine Anmeldeseite mit Branding und einen leeren Zugriffsbereich, in dem der zukünftige Anwendungszugriff aktiviert werden kann.

### <a name="assigning-azure-ad-licenses-using-groups"></a>Zuweisen von Azure AD-Lizenzen mithilfe von Gruppen 

1. Bob ist der globale Azure AD-Administrator. Er möchte im Rahmen der Ersteinführung von Azure AD einer bestimmten Benutzergruppe Azure AD-Lizenzen zuweisen.
2. Bob erstellt eine Gruppe für die Pilotbenutzer. 
3. Bob weist der Gruppe die Lizenzen zu.
4. Susie ist ein Information-Worker und wird aufgrund ihrer Position der Sicherheitsgruppe hinzugefügt.
5. Nach einiger Zeit hat Susie Zugriff auf die Azure AD Premium-Lizenz. Diese Lizenz wird für einige der weiter unten aufgeführten POC-Szenarien vorausgesetzt.

## <a name="theme---lots-of-apps-one-identity"></a>Design – Viele Apps, eine Identität

| Szenario | Bausteine| 
| --- | --- |  
| [Integrieren von SaaS-Anwendungen – Verbund-SSO](#integrate-saas-applications---federated-sso) | [SSO-Konfiguration mit SaaS-Verbund](active-directory-playbook-building-blocks.md#saas-federated-sso-configuration) <br/>[Gruppen – Delegierte Eigentümerschaft](active-directory-playbook-building-blocks.md#groups---delegated-ownership) |
| [Integrieren von SaaS-Anwendungen – Kennwort-SSO](#integrate-saas-applications---password-sso) | [SaaS-Konfiguration für Kennwort-SSO](active-directory-playbook-building-blocks.md#saas-password-sso-configuration) |
| [SSO und Identitätslebenszyklus-Ereignisse](#sso-and-identity-lifecycle-events) | [SaaS und Identitätslebenszyklus](active-directory-playbook-building-blocks.md#saas-and-identity-lifecycle) |
| [Sicherer Zugriff auf gemeinsam genutzte Konten](#secure-access-to-shared-accounts) | [SaaS-Konfiguration für gemeinsam genutzte Konten](active-directory-playbook-building-blocks.md#saas-shared-accounts-configuration) |
| [Sicherer Remotezugriff auf lokale Anwendungen](#secure-remote-access-to-on-premises-applications) | [Anwendungsproxykonfiguration](active-directory-playbook-building-blocks.md#app-proxy-configuration) |
| [Synchronisieren von LDAP-Identitäten mit Azure AD](#synchronize-ldap-identities-to-azure-ad) |  [Konfiguration des generischen LDAP-Connectors](active-directory-playbook-building-blocks.md#generic-ldap-connector-configuration) |

### <a name="integrate-saas-applications---federated-sso"></a>Integrieren von SaaS-Anwendungen – Verbund-SSO 

1. Bob ist der globale Azure AD-Administrator und erhält von der Marketingabteilung die Aufforderung, den Zugriff auf die ServiceNow-Instanz zu aktivieren. Bob sucht das ausführliche Tutorial in der Azure AD-Dokumentation und führt die dort angegebenen Schritte aus. Er delegiert die Zuweisung der Benutzer zur App an Kevin, den Leiter des Marketingteams. 
2. Kevin meldet sich als Besitzer von ServiceNow-Berechtigungen an und weist Susie der App zu. Kevin stellt außerdem fest, dass Susies Profil automatisch in ServiceNow erstellt wurde.
3. Susie ist ein Information-Worker in der Marketingabteilung. Sie meldet sich bei Azure AD an und sieht alle ihr zugewiesenen SaaS-Anwendungen im MyApps-Portal. Von dort hat sie problemlos Zugriff auf ServiceNow.
4. Die Marketingabteilung möchte überwachen, wer auf ServiceNow zugegriffen hat. Bob lädt einen Aktivitätsbericht herunter und sendet ihn per E-Mail an Kevin.  

### <a name="sso-and-identity-lifecycle-events"></a>SSO und Identitätslebenszyklus-Ereignisse

1. Susie nimmt Urlaub. Gemäß der Unternehmensrichtlinie wird das lokale AD-Konto vorübergehend deaktiviert. Susie kann sich nun nicht bei Azure AD anmelden und daher nicht auf ServiceNow zugreifen. 
2. Susie wechselt von der Marketing- in die Vertriebsabteilung. Kevin entfernt ihren Zugriff aus ServiceNow. Susie meldet sich bei „MyApps“ in Azure AD an. Ihr wird die ServiceNow-Kachel nicht mehr angezeigt. Nach zehn Minuten bestätigt Kevin, dass Susies Konto in der ServiceNow-Verwaltungskonsole deaktiviert wurde.

### <a name="integrate-saas-applications---password-sso"></a>Integrieren von SaaS-Anwendungen – Kennwort-SSO

1. Bob konfiguriert den Zugriff auf Atlassian HipChat. HipChat nutzt Kennwort-SSO-Integration und gewährt Susie Zugriff.
2. Susie meldet sich beim MyApps-Portal an, sieht einen Link zum Herunterladen der Azure AD-IE-Browsererweiterung und lädt die Erweiterung herunter.
3. Als sie auf den Link klickt, wird sie zur Eingabe des Benutzernamens und Kennworts für HipChat aufgefordert. Dieser Schritt muss einmal ausgeführt werden. Anschließend hat sie Zugriff auf HipChat.
4. Einige Tage später öffnet Susie das MyApps-Portal und klickt erneut auf HipChat. Dieses Mal kann sie die Anwendung direkt öffnen.
5. Kevin, der Besitzer der HipChat-App, möchte überwachen, wer auf die Anwendung zugegriffen hat. Bob lädt einen Überwachungsbericht herunter und senden ihn per E-Mail an Kevin. 

### <a name="secure-access-to-shared-accounts"></a>Sicherer Zugriff auf gemeinsam genutzte Konten 

1. Bob wird beauftragt, das freigegebene Twitter-Handle für Mitglieder des Vertriebsteams zu sichern. Er fügt Twitter als SSO-Anwendung hinzu und weist sie dann der Sicherheitsgruppe des Vertriebsteams zu. Ihm wurden die Anmeldeinformationen für das gemeinsam genutzte Konto mitgeteilt, und er gibt diese nun in das System ein. 
2. Das Freigeben von Twitter-Anmeldeinformationen ist nicht mehr sicher, da diese mehreren Benutzern bekannt sind. Bob aktiviert das automatische Rollover des Twitter-Kennworts.
3. Susie, ein Mitglied des Vertriebsteams, meldet sich beim MyApps-Portal an und sieht einen Link zum Herunterladen der Azure AD-IE-Browsererweiterung. Sie installiert sie.
4. Als sie auf die Erweiterung klickt, hat sie direkten Zugriff auf Twitter. Sie kennt das Kennwort nicht.
5. Arnold ist ebenfalls Mitglied des Vertriebsteams. Er macht die gleiche Erfahrung wie Susie in den Schritten 3 und 4.
6. Die Vertriebsabteilung möchte überwachen, wer auf Twitter zugegriffen hat. Bob lädt einen Aktivitätsbericht herunter und sendet ihn per E-Mail an Kevin. 

### <a name="secure-remote-access-to-on-premises-applications"></a>Sicherer Remotezugriff auf lokale Anwendungen

1. Bob, der globale Azure AD-Administrator, hat zahlreiche Anfragen erhalten, Mitarbeitern, die nicht vor Ort arbeiten, den Zugriff auf verschiedene nützliche lokale Ressourcen zu ermöglichen, beispielsweise die Ausgabenanwendung. Er führt die Schritte in der [Anwendungsproxydokumentation](active-directory-application-proxy-enable.md) aus, um einen Connector zu installieren und die Ausgabenanwendung als Anwendungsproxyanwendung zu veröffentlichen. 
2. Bob gibt die externe URL für die Ausgabenanwendung für Susie frei, eine der Mitarbeiter, die Remotezugriff benötigen. Sie klickt auf den Link. Nachdem sie sich bei AAD authentifiziert hat, kann sie die Ausgaben-App auch von einem Remotestandort aus verwenden. 
3. Bob veröffentlicht mit demselben Prozess weitere lokale Anwendungen und gewährt Benutzern nach Bedarf den Zugriff. Er fügt bedingten Zugriff und Multi-Factor Authentication für sensiblere Anwendungen hinzu, um zusätzliche Sicherheit zu gewährleisten.

### <a name="synchronize-ldap-identities-to-azure-ad"></a>Synchronisieren von LDAP-Identitäten mit Azure AD

1. Bobs Unternehmen verwendet eine komplexe Identitätsinfrastruktur. Die meisten Benutzer werden in Windows Server Active Directory Domain Services (ADDS) verwaltet. Einige von ihnen werden über das HR-System in Active Directory Lightweight Directory Services (ADLDS) verwaltet.
2. Bob wird beauftragt, den Zugriff auf SaaS-Apps für alle Benutzer (auch die nicht in ADDS vorhandenen Benutzer) zu ermöglichen.
3. Bob konfiguriert einen generischen LDAP-Connector, um Daten aus ADLDS in Azure AD Connect abzurufen.
4. Bob erstellt Synchronisierungsregeln, sodass LDAP-Benutzer in Metaverse und Azure AD aufgefüllt werden.
5. Susie ist ein LDAP-Benutzer und greift mithilfe der synchronisierten Identität auf die SaaS-App zu.



> [!IMPORTANT] 
> Dies ist eine erweiterte Konfiguration, für die sich der Benutzer etwas mit FIM/MIM auskennen muss. Bei Verwendung dieser Konfiguration in der Produktion sollten Fragen dazu an den [Premier Support](https://support.microsoft.com/premier) gerichtet werden.



## <a name="theme---increase-your-security"></a>Design – Erhöhen der Sicherheit 

| Szenario | Bausteine| 
| --- | --- |  
| [Sicherer Zugriff auf das Administratorkonto](#secure-administrator-account-access) | [Azure MFA bei Telefonanrufen](active-directory-playbook-building-blocks.md#azure-multi-factor-authentication-with-phone-calls) |
| [Sicherer Zugriff für Anwendungen](#secure-access-to-applications) | [Bedingter Zugriff für SaaS-Anwendungen](active-directory-playbook-building-blocks.md#mfa-conditional-access-for-saas-applications) |
| [Aktivieren der Just-in-Time-Verwaltung](#enable-just-in-time-jit-administration) | [Privileged Identity Management](active-directory-playbook-building-blocks.md#privileged-identity-management-pim) |
| [Schützen von Identitäten anhand des Risikos](#protect-identities-based-on-risk) | [Ermitteln von Risikoereignissen](active-directory-playbook-building-blocks.md#discovering-risk-events) <br/>[Bereitstellen von Richtlinien für Risiken bei der Anmeldung](active-directory-playbook-building-blocks.md#deploying-sign-in-risk-policies) |
| [Authentifizieren ohne Kennwörter per zertifikatbasierter Authentifizierung](#authenticate-without-passwords-using-certificate-based-authentication) | [Konfigurieren der zertifikatbasierten Authentifizierung](active-directory-playbook-building-blocks.md#configuring-certificate-based-authentication)

### <a name="secure-administrator-account-access"></a>Sicherer Zugriff auf das Administratorkonto

1. Bob ist der globale Azure AD-Administrator. Er hat Stuart zum Co-Administrator des Diensts ernannt. 
2. Bob konfiguriert Stuarts Konto so, dass zur Verbesserung des Sicherheitsstatus immer MFA erforderlich ist.
3. Stuart meldet sich beim Azure-Portal an und stellt fest, dass er zum Fortsetzen der Anmeldung seine Telefonnummer registrieren muss.
4. Nachfolgende Anmeldungen von Stuart sind nun durch Multi-Factor Authentication geschützt, und er erhält einen Anruf zur Bestätigung seiner Identität.

### <a name="secure-access-to-applications"></a>Sicherer Zugriff auf Anwendungen

1. Kevin ist der Geschäftsinhaber von ServiceNow. Das Unternehmen möchte nun, dass sich diese Benutzer mit MFA anmelden, wenn Sie von außerhalb auf das Unternehmensnetzwerk zugreifen.
2. Bob, der globale Azure AD-Administrator, fügt der ServiceNow-Anwendung eine Richtlinie für bedingten Zugriff hinzu, um MFA für externen Zugriff zu aktivieren.
3. Susie, unser Information-Worker, meldet sich beim MyApps-Portal an und klickt auf die ServiceNow-Kachel. Sie muss nun die MFA durchlaufen.

### <a name="enable-just-in-time-jit-administration"></a>Aktivieren der Just-in-Time-Verwaltung (JIT)

1. Bob und Stuart sind globale Azure AD-Administratoren. Sie möchten JIT-Zugriff auf die Verwaltungsrollen ermöglichen und darüber hinaus die Verwendung der privilegierten Rollen dokumentieren.
2. Bob aktiviert PIM im Azure AD-Mandanten und wird Sicherheitsadministrator. Er ändert seine und Stuarts Mitgliedschaft in der globalen Administratorrolle von „Permanent“ in „Berechtigt“.
3. Bob und Stuart müssen ihre Rolle nun über das Azure-Portal aktivieren, bevor sie Änderungen an der Azure AD-Konfiguration vornehmen. 

### <a name="protect-identities-based-on-risk"></a>Schützen von Identitäten anhand des Risikos 

1. Susie, ein Information-Worker, versucht, sich über einen Tor-Browser anzumelden. 
2. Bob überprüft das Azure AD Identity Protection-Dashboard und bemerkt Susies Anmeldung von einer anonymen IP-Adresse. Das Sicherheitsteam möchte, dass solche Benutzer die MFA durchlaufen.
3. Bob aktiviert die Azure AD Identity Protection-Richtlinie, um MFA für Ereignisse mit mittlerem oder höherem Risiko als erforderlich festzulegen.
4. Nach einiger Zeit meldet sich Susie erneut über einen Tor-Browser an. Dieses Mal wird ihr die MFA-Aufforderung angezeigt.

### <a name="authenticate-without-passwords-using-certificate-based-authentication"></a>Authentifizieren ohne Kennwörter per zertifikatbasierter Authentifizierung

1. Bob ist globaler Administrator für ein Finanzinstitut, das die Nutzung von Kennwörtern als Authentifizierungsfaktor für ihre Anwendungen verbietet.
2. Bob aktiviert und erzwingt die Zertifikatauthentifizierung in AD FS und Azure AD.
3. Susie wird beim Zugriff auf die Anwendung zur Authentifizierung mit Zertifikat aufgefordert.

## <a name="theme---scale-with-self-service"></a>Design – Self-Service-Skalierung

| Szenario | Bausteine| 
| --- | --- |  
| [Self-Service-Kennwortzurücksetzung](#self-service-password-reset) | [Self-Service-Kennwortzurücksetzung](active-directory-playbook-building-blocks.md#self-service-password-reset) |
| [Self-Service-Zugriff auf Anwendungen](#self-service-access-to-applications) | [Self-Service-Zugriff auf Anwendungen](active-directory-playbook-building-blocks.md#self-service-access-to-application-management) |

### <a name="self-service-password-reset"></a>Self-Service-Kennwortzurücksetzung 

1. Bob ist der globale Azure AD-Administrator und aktiviert die Self-Service-Kennwortverwaltung für eine Reihe von Benutzern, einschließlich Susie. 
2. Susie meldet sich beim MyApps-Portal an und sieht eine Meldung, dass sie ihre Sicherheitsinformationen für künftige Kennwortzurücksetzungsereignisse registrieren soll.
3. Einige Tage später: Susie hat ihr Kennwort vergessen und setzt es über das Azure AD-Portal zurück.

### <a name="self-service-access-to-applications"></a>Self-Service-Zugriff auf Anwendungen 

1. Kevin ist der Geschäftsinhaber von ServiceNow. Er möchte nicht alle Benutzer auf einmal hinzufügen, sondern möchte, dass sich Benutzer nach Bedarf „registrieren“.
2. Bob, der globale Azure AD-Administrator, ändert die ServiceNow-Anwendung, um Self-Service-Anforderungen zu ermöglichen.
3. Susie, unser Information-Worker, meldet sich beim MyApps-Portal an und klickt auf die Schaltfläche „Add more applications“ (Mehr Anwendungen hinzufügen). ServiceNow wird als empfohlene Anwendung angezeigt. Anschließend navigiert sie zurück zum MyApps-Portal. Dort wird die ServiceNow-Anwendung angezeigt.

[!INCLUDE [active-directory-playbook-toc](../../includes/active-directory-playbook-steps.md)]
