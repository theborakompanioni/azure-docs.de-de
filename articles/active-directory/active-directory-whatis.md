---
title: Was ist Azure Active Directory?
description: "Verwenden Sie Azure Active Directory, um Ihre vorhandenen lokalen Identitäten in die Cloud zu erweitern oder in Azure AD integrierte Anwendungen zu entwickeln."
services: active-directory
documentationcenter: 
author: curtand
manager: femila
editor: 
ms.assetid: 498820c4-9ebe-42be-bda2-ecf38cc514ca
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/17/2016
ms.author: curtand
translationtype: Human Translation
ms.sourcegitcommit: 21cd9fa752f6f3bf2e3aab572ad3336fbd2bee92
ms.openlocfilehash: 067bef695d8d6a340f7974478e6580b10a1e8dc8


---
# <a name="what-is-azure-active-directory"></a>Was ist Azure Active Directory?
Azure Active Directory (Azure AD) ist der mehrinstanzenfähige cloudbasierte Verzeichnis- und Identitätsverwaltungsdienst von Microsoft.

Für IT-Administratoren bietet Azure AD eine kostengünstige, benutzerfreundliche Lösung, um Mitarbeitern und Geschäftspartnern SSO-Zugriff (Single Sign-On, einmaliges Anmelden) auf [Tausende von Cloud-SaaS-Anwendungen](http://blogs.technet.com/b/ad/archive/2014/09/03/50-saas-apps-now-support-federation-with-azure-ad.aspx) wie Office 365, Salesforce.com, DropBox und Concur bereitzustellen.

Anwendungsentwickler können sich dank Azure AD vollständig auf die Erstellung von Anwendungen konzentrieren, da Azure AD die schnelle und einfache Integration in eine erstklassige Identitätsverwaltungslösung ermöglicht, die von Millionen von Unternehmen auf der ganzen Welt verwendet wird.

Azure AD bietet auch einen vollständigen Satz von Identitätsverwaltungsfunktionen, wie z. B.: mehrstufige Authentifizierung, Geräteregistrierung, Self-Service-Kennwortverwaltung, Self-Service-Gruppenverwaltung, privilegierte Kontenverwaltung, rollenbasierte Zugriffssteuerung, Überwachung der Anwendungsnutzung, umfassende Auditfunktionen sowie Sicherheitsüberwachung und -warnungen. Mit diesen Funktionen können Sie cloudbasierte Anwendungen sichern, IT-Prozesse optimieren, Kosten senken und sicherstellen, dass unternehmensweite Kompatibilitätsziele erreicht werden.

Darüber hinaus lässt sich Azure AD mit nur [vier Klicks](http://blogs.technet.com/b/ad/archive/2014/08/04/connecting-ad-and-azure-ad-only-4-clicks-with-azure-ad-connect.aspx)in eine vorhandene Windows Server Active Directory-Instanz integrieren, sodass Unternehmen ihre Investitionen in vorhandene lokale Identitätsverwaltungslösungen nutzen können, um den Zugriff auf cloudbasierte SaaS-Anwendungen zu verwalten.

Wenn Sie Office 365-, Azure- oder Dynamics CRM Online-Kunde sind, haben Sie möglicherweise noch nicht bemerkt, dass Sie bereits Azure AD verwenden. Jeder Azure-, Office 365- und Dynamics CRM-Mandant ist bereits ein Azure AD-Mandant. Sie können jederzeit damit beginnen, diesen Mandanten zum Verwalten des Zugriffs auf Tausende anderer Cloudanwendungen zu verwenden, in die Azure AD integriert ist!

![Azure AD Connect-Stapel](./media/active-directory-whatis/Azure_Active_Directory.png)

## <a name="how-reliable-is-azure-ad"></a>Wie zuverlässig ist Azure AD?
Dank der mehrinstanzenfähigen, geografisch verteilten Azure AD-Struktur mit hoher Verfügbarkeit können Sie sich bei Ihren wichtigsten Geschäftsanforderungen auf Azure AD verlassen. Azure AD wird in 28 Datencentern auf der ganzen Welt mit automatischen Failover ausgeführt – so können Sie sicher sein, dass Azure AD hochgradig zuverlässig funktioniert. Selbst wenn einmal ein Datencenter ausfällt, befinden sich Kopien Ihrer Verzeichnisdaten in mindestens zwei weiteren regional verteilten Datencentern und stehen für den sofortigen Zugriff zur Verfügung.

Weitere Einzelheiten finden Sie unter [Vereinbarungen zum Servicelevel](https://azure.microsoft.com/support/legal/sla/).

## <a name="what-are-the-benefits-of-azure-ad"></a>Welche Vorteile bietet Azure AD?
Ihr Unternehmen kann Azure AD verwenden, um in vielerlei Hinsicht die Mitarbeiterproduktivität zu verbessern, IT-Prozesse zu optimieren, die Sicherheit zu erhöhen und die Kosten zu senken:

* Führen Sie Clouddienste in kürzester Zeit ein, um Mitarbeitern und Partnern den Zugriff über die einmalige Anmeldung zu vereinfachen, und profitieren Sie von den vollständig automatisierten Azure AD-Funktionen für die SaaS-App-Zugriffsverwaltung und die Bereitstellung von Diensten.
* Bieten Sie Mitarbeitern Zugriff auf erstklassige Cloud-Apps und Self-Service-Funktionen – von jedem Ort der Welt und mit allen Geräten, die Ihre Mitarbeiter gerne nutzen.
* Verwalten Sie den Zugriff Ihrer Mitarbeiter und Lieferanten auf Ihre Unternehmenskonten für soziale Medien.
* Erhöhen Sie die Sicherheit von Anwendungen mit der mehrstufigen Authentifizierung und dem bedingten Zugriff von Azure AD.
* Implementieren Sie eine konsistente Self-Service-Verwaltung des Anwendungszugriffs, sodass Geschäftsinhaber schnell reagieren und gleichzeitig IT-Aufwand und -Kosten senken können.
* Überwachen Sie die Anwendungsnutzung, und schützen Sie Ihr Unternehmen mit Funktionen für Sicherheitsberichte und -überwachung vor Bedrohungen.
* Sichern Sie den mobilen bzw. Remotezugriff auf lokale Anwendungen.

## <a name="how-does-azure-ad-compare-to-on-premises-active-directory-domain-services-ad-ds"></a>Azure AD und lokale Active Directory Domain Services (AD DS) im Vergleich

Azure Active Directory (Azure AD) und das lokale Active Directory (Active Directory Domain Services oder AD DS) sind Systeme, in denen Verzeichnisdaten gespeichert werden und die Kommunikation zwischen Benutzern und Ressourcen verwaltet wird – z.B. Benutzeranmelde- und Authentifizierungsvorgänge sowie Verzeichnissuchen.

AD DS ist eine Serverrolle in Windows Server, was bedeutet, dass sie auf physischen oder virtuellen Computern bereitgestellt werden kann. Die Rolle weist eine hierarchische Struktur auf, die auf X.500 basiert. AD DS verwendet DNS für die Suche nach Objekten, kann über LDAP interagieren und nutzt in erster Linie Kerberos für die Authentifizierung. Active Directory aktiviert Organisationseinheiten (Organizational Units, OUs) und Gruppenrichtlinienobjekte (Group Policy Objects, GPOs), bindet Computer in die Domäne ein, und erstellt Vertrauensstellungen zwischen den Domänen.

Azure AD ist ein öffentlicher Verzeichnisdienst mit mehreren Kunden, was bedeutet, dass Sie in Azure AD einen Mandanten für Ihre Cloudserver und Anwendungen, wie z.B. Office 365, erstellen können. Benutzer und Gruppen werden in einer flachen Struktur ohne Organisationseinheiten oder Gruppenrichtlinienobjekte erstellt. Die Authentifizierung erfolgt über Protokolle wie SAML, WS-Federation und OAuth. Es ist zwar möglich, Azure AD abzufragen, dann müssen Sie aber statt LDAP eine REST-API namens „AD Graph-API“ verwenden. All diese Komponenten arbeiten mit HTTP und HTTPS.

Sie können Azure AD Connect zum Synchronisieren Ihrer lokalen Identitäten mit Azure AD verwenden.

### <a name="authentication-and-authorization-details"></a>Informationen zu Authentifizierung und Autorisierung

| Azure AD                                   | AD DS lokal |
|--------------------------------------------|-------------------|
| <li>   SAML      <li>   WS-Federation-    <li>   interaktiv mit unterstützten Anmeldeinformationen  <li>   OAuth 2.0   <li>   OpenID Connect                          | <li>   SAML   <li>   WS-Federation-  <li>   NTLM  <li>   Kerberos   <li>   MD5   <li>   Basic          |

### <a name="object-repository-details"></a>Informationen zum Objektrepository

| Azure AD                                          | AD DS lokal |
|---------------------------------------------------|-------------------|
| Zugriff über Azure AD Graph und Microsoft Graph | X.500 LDAP    |

### <a name="programmatic-access-details"></a>Informationen zum programmgesteuerten Zugriff

| Azure AD                        | AD DS lokal |
|---------------------------------|-------------------|
| MS/Azure AD Graph REST-APIs | LDAP          |

### <a name="sso-to-applications-details"></a>Informationen zum einmaligen Anmelden bei Anwendungen

| Azure AD           | AD DS lokal   |
|--------------------|---------------------|
| <li>   OpenID Connect  <li>   SAML          | <li>   SAML      <li>   WS-Fed   <li>   Open-ID connect  |

### <a name="access-management-details"></a>Informationen zur Zugriffsverwaltung

| Azure AD         | AD DS lokal     |
|------------------------------------|------------------------------------------------------------------------|
| <li>   Ressourcendefinierter Bereich und rollenbasierte Zugriffssteuerung   <li>   Clientdefinierte Delegierung und Anwendungsberechtigungen        <li>   Consent Framework (erzwingt die ordnungsgemäße Zustimmung durch Benutzer/Administrator, wie von der Ressource/vom Client definiert/angefordert)  <li>   Über App-Rolle, kann individuell oder über Gruppen angewendet werden, unterstützt:   <ul> <li>   Verwaltung durch Administrator       <li>   Self-Service-Anwendungszugriff  <li>   Benutzerzustimmung </ul>         | <li>   Über Zugriffssteuerungslisten, kann individuell oder über Gruppen angewendet werden, unterstützt:  <ul><li>   Verwaltung durch Administrator  </ul>                                                 |

### <a name="group-management-details"></a>Informationen zur Gruppenverwaltung

| Azure AD                          | AD DS lokal                                 |
|-----------------------------------|---------------------------------------------------|
| <li>   Verwaltung durch Administrator    <li>   Verwaltung durch Regeln oder dynamisch   <li>   Self-Service-Gruppenverwaltung  | <li>   Verwaltung durch Administrator      <li>   Externes System (FIM oder anderes) erforderlich für:  <ul><li>   Verwaltung durch Regeln oder dynamisch      </ul>                 |

### <a name="supported-credentials-details"></a>Unterstützte Anmeldeinformationen

| Azure AD           | AD DS lokal  |
|--------------------|--------------------|
| <li>   Benutzername und Kennwort  <li>   Smartcard     | <li>   Benutzername und Kennwort  <li>   Smartcard     |


## <a name="how-can-i-get-started"></a>Wie fange ich an?
* Wenn Sie IT-Administrator sind:
  * [Probieren Sie es aus!](https://azure.microsoft.com/trial/get-started-active-directory/)  – Registrieren Sie sich noch heute über diesen Link für eine kostenlose 30-Tage-Testversion, und stellen Sie Ihre erste Cloudlösung in weniger als 5 Minuten bereit.
  * Lesen Sie den Artikel „Erste Schritte mit Azure AD“ mit Tipps und Tricks zum schnellen Einrichten und Ausführen eines Azure AD-Mandanten.
* Wenn Sie Entwickler sind:
  * Sehen Sie sich die [Anleitung für Entwickler](active-directory-developers-guide.md) für Azure Active Directory an.
  * [Starten Sie eine Testversion:](https://azure.microsoft.com/trial/get-started-active-directory/) Registrieren Sie sich noch heute für eine kostenlose 30-Tage-Testversion, und beginnen Sie damit, Ihre Apps in Azure AD zu integrieren.

## <a name="where-can-i-learn-more"></a>Wo kann ich mehr erfahren?
Wir haben viele hervorragende Ressourcen online gestellt, in denen Sie alles Wissenswerte über Azure AD erfahren. Hier finden Sie eine Liste mit wichtigen Artikeln, um Ihnen den Einstieg zu erleichtern:

* [Aktivieren Sie Ihr Verzeichnis für das Hybrid-Management mit Azure AD Connect](active-directory-aadconnect.md)
* [Mehr Sicherheit für eine allzeit vernetzte Welt](../multi-factor-authentication/multi-factor-authentication.md)
* [Automatisieren der Bereitstellung und Bereitstellungsaufhebung von Benutzern für SaaS-Anwendungen mit Azure Active Directory](active-directory-saas-app-provisioning.md)
* [Erste Schritte mit Azure AD Reporting](active-directory-reporting-getting-started.md)
* [Verwalten von Kennwörtern von einem beliebigen Ort aus](active-directory-passwords.md)
* [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md)
* [Automatisieren der Bereitstellung und Bereitstellungsaufhebung von Benutzern für SaaS-Anwendungen mit Azure Active Directory](active-directory-saas-app-provisioning.md)
* [Bereitstellen von sicherem Remotezugriff auf lokale Anwendungen](active-directory-application-proxy-get-started.md)
* [Verwalten des Zugriffs auf Ressourcen mit Azure Active Directory-Gruppen](active-directory-manage-groups.md)
* [Was ist Microsoft Azure Active Directory-Lizenzierung?](active-directory-licensing-what-is.md)
* [Wie ermittle ich nicht genehmigte Cloud-Apps, die in meiner Organisation verwendet werden?](active-directory-cloudappdiscovery-whatis.md)



<!--HONumber=Nov16_HO4-->


