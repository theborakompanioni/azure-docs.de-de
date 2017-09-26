---
title: "Grundlegendes zur Azure-Identität | Microsoft-Dokumentation"
description: "Erhalten Sie grundlegende Informationen zu den Begriffen, Konzepten und Empfehlungen der Microsoft Azure-Identitätslösung, um für Ihre Organisation eine fundierte Entscheidung zur Identitätsgovernance zu treffen."
keywords: 
author: jeffgilb
manager: femila
ms.reviewer: jsnow
ms.author: jeffgilb
ms.date: 7/17/2017
ms.topic: article
ms.prod: 
ms.service: azure
ms.technology: 
ms.assetid: 
ms.custom: it-pro
ms.translationtype: HT
ms.sourcegitcommit: 4f77c7a615aaf5f87c0b260321f45a4e7129f339
ms.openlocfilehash: e368f14638c480a632afa7c17023aa8ae4c8833f
ms.contentlocale: de-de
ms.lasthandoff: 09/22/2017

---
# <a name="understand-azure-identity-solutions"></a>Grundlegendes zu Azure-Identitätslösungen
Microsoft Azure Active Directory (Azure AD) ist eine Cloudlösung für die Identitäts- und Zugriffsverwaltung, die Verzeichnisdienste, Indentitätsgovernance und Möglichkeiten für die Anwendungszugriffsverwaltung bietet. Sie ermöglicht Ihnen das [einmalige Anmelden (SSO)](https://docs.microsoft.com/azure/active-directory/active-directory-enterprise-apps-manage-sso) bei tausenden vorab integrierten geschäftlichen und benutzerdefinierten Apps aus dem [Azure AD-Anwendungskatalog](https://azure.microsoft.com/marketplace/active-directory/all/). Viele dieser Apps verwenden Sie wahrscheinlich bereits, wie z.B Salesforce.com, Office 365, Box, ServiceNow und Workday.

Für ein Azure-Abonnements wird automatisch ein einzelnes Azure AD-Verzeichnis erstellt und zugeordnet. Azure AD stellt als Identitätsdienst in Azure anschließend für alle cloudbasierten Ressourcen die Funktionen für die Identitätsverwaltung und die Zugriffssteuerung bereit. Für einen einzelnen Mandanten (Organisation) können diese Ressourcen Benutzer, Apps und Gruppen einschließen, wie im nachstehenden Diagramm gezeigt:

![Azure Active Directory](./media/understand-azure-identity-solutions/azure-ad.png)

Für die individuellen Anforderungen Ihrer Organisation bietet Microsoft Azure mehrere Möglichkeiten mit unterschiedlichem Komplexitätsgrad, um die Identität als Dienst (IDaaS) zu nutzen. In diesem Artikel lernen Sie die grundlegenden Konzepte und Begriffe der Azure-Identität kennen und erhalten Empfehlungen, wie Sie aus den verfügbaren Optionen die für Sie passendsten auswählen.

## <a name="terms-to-know"></a>Relevante Terminologie

Bevor Sie eine Entscheidung hinsichtlich der Azure-Identitätsdienstlösung für Ihre Organisation treffen können, müssen Sie die Begriffe kennen, die im Zusammenhang mit den Azure-Identitätsdiensten häufig verwendet werden.

|Begriff| Beschreibung|
|-----|-----|
|Azure-Abonnement |Abonnements werden zum Bezahlen von Azure-Clouddiensten verwendet und sind in der Regel mit einer Kreditkarte verknüpft. Sie können mehrere Abonnements besitzen, aber es kann schwierig sein, Ressourcen über mehrere Abonnements gemeinsam zu verwenden.|
|Azure-Mandant | Ein Azure AD-Mandant steht für eine einzelne Organisation. Es handelt sich um eine dedizierte, vertrauenswürdige Instanz von Azure AD, die automatisch erstellt wird, wenn sich eine Organisation für ein Abonnement eines Microsoft-Clouddienst wie Azure, Intune oder Office 365 registriert. Mandanten können Zugriff auf Dienste in einer dedizierten Umgebung (einzelner Mandant) oder in einer mit anderen Organisationen gemeinsam genutzten Umgebung (mehrinstanzenfähig) erhalten.|
|Azure AD-Verzeichnis | Jeder Azure-Mandant verfügt über ein dediziertes, vertrauenswürdiges Azure AD-Verzeichnis mit den Benutzern, Gruppen und Anwendungen des Mandanten. Es wird zum Ausführen von Funktionen zur Identitäts- und Zugriffsverwaltung für Mandantenressourcen verwendet. Da automatisch ein eindeutiges Azure AD-Verzeichnis für Ihre Organisation bereitgestellt wird, sobald Sie sich für einen Microsoft-Clouddienst wie Azure, Microsoft Intune oder Office 365 registrieren, werden die Begriffe *Mandant*, *Azure AD* und *Azure AD-Verzeichnis* gelegentlich synonym verwendet. |
|Benutzerdefinierte Domäne | Wenn Sie sich erstmals für ein Abonnement eines Microsoft-Clouddiensts registrieren, wird für Ihre Domäne ein Name verwendet, der *.onmicrosoft.com* enthält. Die meisten Organisationen besitzen jedoch mindestens einen Domänennamen, der für die Geschäftstätigkeit und von Endbenutzern für den Zugriff auf Unternehmensressourcen verwendet wird. Sie können Ihren benutzerdefinierten Domänennamen in Azure AD hinzufügen, damit Ihre Benutzer mit einer bekannten Domäne arbeiten können, z.B. *alice@contoso.com* anstelle von *alice@contoso.onmicrosoft.com*. |
|Azure AD-Konto | Hierbei handelt es sich um Identitäten, die über Azure AD oder einen anderen Microsoft-Clouddienst wie Office 365 erstellt werden. Sie werden in Azure AD gespeichert und sind für jedes Clouddienstabonnement der Organisation zugänglich. |
|Azure-Abonnementadministrator| Der Kontoadministrator ist die Person, die sich für das Azure-Abonnement registriert bzw. die es erworben hat. Er darf über das [Kontocenter](https://account.azure.com/Subscriptions) verschiedene Verwaltungsaufgaben ausführen, z.B. Erstellen/Kündigen von Abonnements, Ändern der Abrechnung für ein Abonnement oder Ändern des Dienstadministrators. |
|Globaler Azure AD-Administrator | Globale Azure AD-Administratoren besitzen Vollzugriff auf alle Verwaltungsfunktionen von Azure AD. Die Person, die sich für ein Microsoft-Clouddienstabonnement registriert, wird standardmäßig automatisch als globaler Administrator festgelegt. Sie können mehrere globale Administratoren festlegen, aber nur globale Administratoren können Benutzern eine der [weiteren Administratorrollen](https://docs.microsoft.com/azure/active-directory/active-directory-assign-admin-roles-azure-portal) zuweisen. |
|Microsoft-Konto | Microsoft-Konten (von Ihnen privat erstellt) bieten Zugriff auf alle für den normalen Computerbenutzer entwickelten Microsoft-Produkte und Clouddienste wie Outlook (Hotmail), OneDrive, MSN, Xbox LIVE oder Office 365. Diese Identitäten werden im Microsoft-Kontosystem für Endbenutzeridentitäten erstellt und gespeichert, das von Microsoft geführt wird.|
|Geschäfts-, Schul- oder Unikonten | Geschäfts-, Schul- oder Unikonten (von einem Administrator zu Geschäfts-/Bildungszwecken vergeben) bieten Zugriff auf alle Microsoft-Clouddienste für Unternehmen, darunter Azure, Intune oder Office 365.|


## <a name="concepts-to-understand"></a>Konzepte

Nun, da Sie die Grundbegriffe der Azure-Identität kennen, sollten Sie sich über die nachstehenden Konzepte der Azure-Identität informieren, um sich für einen Azure-Identitätsdienst entscheiden zu können.

|Konzept |Beschreibung|
|-----|-----|
|[Beziehung zwischen Azure-Abonnements und Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-how-subscriptions-associated-directory) |Jedes Azure-Abonnement verfügt über eine Vertrauensstellung bei einem Azure AD-Verzeichnis zur Authentifizierung von Benutzern, Diensten und Geräten. *Mehrere Abonnements können demselben Azure AD-Verzeichnis vertrauen, aber ein Abonnement vertraut immer nur einem einzigen Azure AD-Verzeichnis*. Diese Vertrauensstellung unterscheidet sich von der Beziehung, die ein Abonnement mit anderen Azure-Ressourcen aufweist (Websites, Datenbanken usw.). Diese ähneln eher den untergeordneten Ressourcen eines Abonnements. Wenn ein Abonnement abläuft, wird der Zugriff auf die ihm zugeordneten Ressourcen mit Ausnahme von Azure AD ebenfalls beendet. Das Azure AD-Verzeichnis verbleibt jedoch in Azure, und Sie können diesem Verzeichnis ein anderes Abonnement zuordnen und die Mandantenressourcen weiterhin verwalten.|
|[Funktionsweise der Azure AD-Lizenzierung](https://docs.microsoft.com/azure/active-directory/active-directory-licensing-get-started-azure-portal) | Wenn Sie Enterprise Mobility Suite, Azure AD Premium oder Azure AD Basic erwerben oder aktivieren, wird das Verzeichnis mit dem Abonnement aktualisiert, einschließlich der Gültigkeitsdauer und der vorausbezahlten Lizenzen. Nach Aktivierung des Abonnements können der Dienst von globalen Azure AD-Administratoren verwaltet und von lizenzierten Benutzern verwendet werden. Ihre Abonnementinformationen (einschließlich der Anzahl zugewiesener oder verfügbarer Lizenzen) stehen im Azure-Portal auf dem Blatt **Azure Active Directory** > **Lizenzen**. Dies ist auch der beste Ort für die Verwaltung Ihrer Lizenzzuweisungen.|
|[Rollenbasierte Zugriffssteuerung im Azure-Portal](https://docs.microsoft.com/azure/active-directory/role-based-access-control-what-is)|Die rollenbasierte Azure-Zugriffssteuerung (Role-Based Access Control, RBAC) ermöglicht eine präzise Verwaltung des Zugriffs für Azure-Ressourcen. Zu viele Berechtigungen können ein Konto zum leichten Angriffsziel machen. Wenn die Berechtigungen nicht ausreichen, können Mitarbeiter nicht effizient arbeiten. Mithilfe der RBAC können Sie Mitarbeitern genau die Berechtigungen erteilen, die sie brauchen. Dazu verwenden Sie drei grundlegende Rollen, die für alle Ressourcengruppen gelten: Besitzer, Mitwirkender, Leser. Außerdem können Sie bis zu 2.000 eigene [benutzerdefinierte RBAC-Rollen](https://docs.microsoft.com/azure/active-directory/role-based-access-control-custom-roles) erstellen, die Ihren speziellen Anforderungen entsprechen. |
|[Hybrididentität](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect)|Hybrididentität wird erzielt, indem Sie Ihr lokales Windows Server Active Directory (AD DS) über [Azure AD Connect](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect) in Azure AD integrieren. So können Sie für Ihre Benutzer eine einzige Identität für in Azure AD integrierte Office 365-, Azure- und lokale Apps oder SaaS-Anwendungen bereitstellen. Mit Hybrididentität erweitern Sie effektiv Ihre lokale Umgebung in die Cloud, um die Identitäts- und Zugriffsverwaltung zu ermöglichen.|

### <a name="the-difference-between-windows-server-ad-ds-and-azure-ad"></a>Der Unterschied zwischen Windows Server AD DS und Azure AD
Azure Active Directory (Azure AD) und das lokale Active Directory (Active Directory Domain Services oder AD DS) sind Systeme, in denen Verzeichnisdaten gespeichert werden und die Kommunikation zwischen Benutzern und Ressourcen verwaltet wird – z.B. Benutzeranmelde- und Authentifizierungsvorgänge sowie Verzeichnissuchen.

Wenn Sie bereits mit den lokalen Active Directory Domain Services (AD DS) von Windows Server vertraut sind, die erstmals mit Windows 2000 Server eingeführt wurden, ist Ihnen das grundlegende Konzept eines Identitätsdiensts wahrscheinlich bekannt. Allerdings ist unbedingt zu beachten, dass es sich bei Azure AD nicht einfach um einen Domänencontroller in der Cloud handelt. Vielmehr bietet es eine ganz neue Möglichkeit der IDaaS-Bereitstellung (Identity-as-a-Service) in Azure, die eine völlig neue Denkweise erfordert, um die cloudbasierten Funktionen voll auszuschöpfen und Ihre Organisation vor modernen Bedrohungen zu schützen. 

AD DS ist eine Serverrolle in Windows Server, was bedeutet, dass sie auf physischen oder virtuellen Computern bereitgestellt werden kann. Die Rolle weist eine hierarchische Struktur auf, die auf X.500 basiert. AD DS verwendet DNS für die Suche nach Objekten, kann über LDAP interagieren und nutzt in erster Linie Kerberos für die Authentifizierung. Active Directory aktiviert Organisationseinheiten (Organizational Units, OUs) und Gruppenrichtlinienobjekte (Group Policy Objects, GPOs), bindet Computer in die Domäne ein, und erstellt Vertrauensstellungen zwischen den Domänen.

Die IT schützt ihren Sicherheitsumkreis seit Jahren mithilfe von AD DS, aber moderne Unternehmen ohne Umkreisnetzwerk, die die Identitätsanforderungen von Mitarbeitern, Kunden und Partnern unterstützen, benötigen eine neue Steuerungsebene. Diese Ebene der Identitätssteuerung wird von Azure AD bereitgestellt. Die Sicherheit hat sich über die Unternehmensfirewall hinaus in die Cloud verlagert, wo Azure AD für den Schutz der Unternehmensressourcen und des Zugriffs sorgt, indem eine gemeinsame Identität für Benutzer (entweder lokal oder in der Cloud) bereitgestellt wird. So erhalten Ihre Benutzer die Flexibilität, mit nahezu jedem Gerät sicher auf die Apps zuzugreifen, die sie für ihre alltägliche Arbeit benötigen. Zudem werden nahtlos Steuerelemente für den risikobasierten Datenschutz bereitgestellt, die durch Machine Learning-Funktionen und ausführliche Berichterstellung unterstützt werden, und so der IT-Abteilung das Schützen von Unternehmensdaten ermöglichen.

Azure AD ist ein öffentlicher Verzeichnisdienst mit mehreren Kunden, was bedeutet, dass Sie in Azure AD einen Mandanten für Ihre Cloudserver und Anwendungen, wie z.B. Office 365, erstellen können. Benutzer und Gruppen werden in einer flachen Struktur ohne Organisationseinheiten oder Gruppenrichtlinienobjekte erstellt. Die Authentifizierung erfolgt über Protokolle wie SAML, WS-Federation und OAuth. Es ist zwar möglich, Azure AD abzufragen, dann müssen Sie aber statt LDAP eine REST-API namens „AD Graph-API“ verwenden. All diese Komponenten arbeiten mit HTTP und HTTPS.

### <a name="extend-office-365-management-and-security-capabilities"></a>Erweitern von Office 365-Verwaltungs- und -Sicherheitsfunktionen
Verwenden Sie bereits Office 365? Sie können Ihre digitale Transformation beschleunigen, indem Sie die integrierten Office 365-Funktionen mit Azure AD erweitern, um all Ihre Ressourcen zu sichern und in Ihrer gesamten Belegschaft für Sicherheit und Produktivität zu sorgen. Wenn Sie zusätzlich zu den in Office 365 verfügbaren Funktionen Azure AD verwenden, können Sie Ihr gesamtes Anwendungsportfolio mit einer einzigen Identität absichern, die einmaliges Anmelden für alle Apps ermöglicht. Sie können Ihre Funktionen für den bedingten Zugriff erweitern, sodass sie nicht nur auf dem Gerätestatus, sondern auch auf Benutzer, Standort, Anwendung und Risiko beruhen. MFA-Funktionen (Multi-Factor Authentication) bieten bei Bedarf sogar noch mehr Schutz. Sie erhalten eine zusätzliche Übersicht über Benutzerberechtigungen sowie bedarfsgerechten Just-in-Time-Administratorzugriff. Ihre Benutzer können produktiver sein und dank der Self-Service-Funktionen von Azure AD fallen weniger Helpdesktickets an. Diese Funktionen umfassen beispielsweise das Zurücksetzen vergessener Kennwörter, Anwendungszugriffsanforderungen sowie die Erstellung und Verwaltung von Gruppen.

> [!TIP]
> Wünschen Sie weitere Informationen zur Verwendung der Azure AD-Identitätsverwaltung mit Office 365? [Rufen Sie das E-Book ab](https://info.microsoft.com/Extend-Office-365-security-with-EMS.html).

## <a name="microsoft-azure-identity-solutions"></a>Microsoft Azure-Identitätslösungen

Microsoft Azure bietet verschiedene Methoden zum Verwalten von Benutzeridentitäten, unabhängig davon, ob sie vollständig lokal, nur in der Cloud oder sogar irgendwo dazwischen verwaltet werden. Zu den verfügbaren Optionen gehören: AD DS im DIY-Verfahren (Do-it-Yourself) in Azure, Azure Active Directory (Azure AD), Hybrididentität und Azure AD Domain Services.

### <a name="do-it-yourself-diy-ad-ds"></a>Do-it-yourself (DIY) AD DS
Für Unternehmen, die nur einen geringen Ressourcenbedarf in der Cloud haben, kann **AD DS im DIY-Verfahren** in Azure verwendet werden. Diese Option unterstützt viele Windows Server-AD DS-Szenarien, die für die Bereitstellung als virtuelle Computer (VMs) in Azure gut geeignet sind. Beispielsweise können Sie einen virtuellen Azure-Computer als Domänencontroller erstellen, der in einem weit entfernten Rechenzentrum ausgeführt wird und mit dem Remotenetzwerk verbunden ist. Von dort aus kann der virtuelle Computer Authentifizierungsanforderungen von Remotebenutzern unterstützen und die Leistungsfähigkeit der Authentifizierung verbessern. Diese Option eignet sich außerdem als relativ kostengünstige Ersatzlösung für ansonsten kostspielige Notfallwiederherstellungsstandorte, da eine geringe Anzahl von Domänencontrollern und ein einziges virtuelles Netzwerk in Azure gehostet werden. Es kann auch sein, dass Sie eine Anwendung in Azure bereitstellen möchten, z.B. SharePoint. Hierfür ist Windows Server AD DS erforderlich, aber es besteht keine Abhängigkeit vom lokalen Netzwerk oder von der Unternehmensinstanz von Windows Server Active Directory. In diesem Fall können Sie eine isolierte Gesamtstruktur unter Azure bereitstellen, um die Anforderungen der SharePoint-Serverfarm zu erfüllen. Außerdem wird die Bereitstellung von Netzwerkanwendungen unterstützt, für die die Konnektivität mit dem lokalen Netzwerk und dem lokalen Active Directory erforderlich ist.

### <a name="azure-active-directory-azure-ad"></a>Azure Active Directory (Azure AD)
Die **eigenständige Version von Azure Active Directory (Azure AD)** ist eine vollständig cloudbasierte Lösung zur Identitäts- und Zugriffsverwaltung als Dienst (IDaaS). Azure AD bietet Ihnen eine Reihe robuster Funktionen zur Verwaltung von Benutzern und Gruppen. Damit können Sie den sicheren Zugriff auf lokale und Cloudanwendungen gewährleisten, beispielsweise auf Microsoft-Webdienste wie Office 365 oder zahlreiche SaaS-Anwendungen (Software-as-a-Service) anderer Anbieter. Azure AD ist in drei Editionen erhältlich: Free, Basic und Premium. Azure AD steigert die Effektivität von Organisationen und erweitert die Sicherheit über die Umkreisfirewall hinaus auf eine neue Steuerungsebene, die durch Azure Machine Learning und andere erweiterte Sicherheitsfunktionen geschützt wird.

### <a name="hybrid-identity"></a>Hybrididentität
Statt sich zwischen lokalen oder cloudbasierten Identitätslösungen zu entscheiden, dehnen viele zukunftsorientierte CIOs und Unternehmen, die ihre langfristige Ausrichtung planen, ihre lokalen Verzeichnisse mithilfe von **Hybrididentitätslösungen** in die Cloud aus. Durch die Hybrididentität erhalten Sie eine globale Lösung zur Identitäts- und Zugriffsverwaltung, die einen sicheren und produktiven Zugriff auf die Anwendungen bietet, die Ihre Benutzer für ihre Aufgaben benötigen.

> [!TIP]
> Um zu erfahren, wie CIOs Azure Active Directory zu einem zentralen Bestandteil ihrer IT-Strategien gemacht haben, laden Sie den [CIO’s Guide to Azure Active Directory](https://aka.ms/AzureADCIOGuide) (CIO-Leitfaden für Azure Active Directory) herunter.

### <a name="azure-ad-domain-services"></a>Azure AD Domain Services
**Azure AD Domain Services** bieten eine cloudbasierte Option zur Nutzung von AD DS für eine sehr einfache Konfigurationsteuerung der Azure-VM sowie eine Möglichkeit zum Erfüllen lokaler Identitätsanforderungen für das Entwickeln und Testen von Netzwerkanwendungen. Der Zweck von Azure AD Domain Services besteht nicht darin, Ihre lokale AD DS-Infrastruktur per Lift & Shift zu virtuellen Azure-Computern zu migrieren, die von Azure AD Domain Services verwaltet werden. Stattdessen sollten virtuelle Azure-Computer in verwalteten Domänen dazu verwendet werden, die Entwicklung, das Testen und die Verschiebung lokaler Anwendungen, die AD DS-Authentifizierungsmethoden benötigen, in die Cloud zu unterstützen.

## <a name="common-scenarios-and-recommendations"></a>Gängige Szenarien und Empfehlungen

Im Folgenden sind einige Identitäts- und Zugriffsszenarien mit Empfehlungen zur Auswahl der jeweils geeigneten Azure-Identitätsoption aufgeführt.

|Identitätsszenario| Empfehlung|
|-----|-----|
|Meine Organisation hat umfangreich in lokales Windows Server Active Directory investiert, aber wir möchten die Identitätsverwaltung auf die Cloud ausdehnen.| Die am weitesten verbreitete Azure-Identitätslösung ist die [Hybrididentität](https://docs.microsoft.com/azure/active-directory/active-directory-hybrid-identity-design-considerations-overview). Wenn Sie bereits Investitionen in lokale AD DS getätigt haben, können Sie die Identitätsverwaltung mithilfe von Azure AD Connect ganz einfach auf die Cloud ausdehnen.|
|Mein Unternehmen basiert vollständig auf der Cloud, und wir haben keinerlei Investitionen in lokale Identitätslösungen getätigt.| [Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-whatis) ist die beste Wahl für rein cloudbasierte Unternehmen ohne Investitionen in eine lokale Infrastruktur.|
|Ich benötige eine einfache Möglichkeit zur Konfiguration und Steuerung einer Azure-VM, um die lokalen Identitätsanforderungen an App-Entwicklung und -Tests zu erfüllen.|[Azure AD Domain Services](https://docs.microsoft.com/azure/active-directory-domain-services/active-directory-ds-overview) ist eine gute Wahl, wenn Sie AD DS für eine einfache Konfigurationssteuerung einer Azure-VM benötigen oder alte, verzeichnisabhängige, lokale Anwendungen zur Cloud migrieren oder dort entwickeln möchten.|  
|Ich möchte eine kleine Anzahl virtueller Computer in Azure unterstützen, aber mein Unternehmen hat umfangreiche Investitionen in lokales Active Directory (AD DS) getätigt.|Verwenden Sie [AD DS im DIY-Verfahren](https://msdn.microsoft.com/library/azure/jj156090.aspx) für die Nutzung virtueller Azure-Computer, wenn Sie eine geringe Anzahl virtueller Computer unterstützen möchten und große lokale AD DS-Investitionen getätigt haben. |

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

## <a name="next-steps"></a>Nächste Schritte

Da Sie jetzt die Azure-Identitätskonzepte und die Ihnen zur Verfügung stehenden Optionen kennen, können Sie die folgenden Ressourcen verwenden, um mit der Implementierung der Option Ihrer Wahl zu beginnen:

[Weitere Informationen zu Azure-Hybrididentitätslösungen](https://docs.microsoft.com/azure/active-directory/choose-hybrid-identity-solution)

[Erlangen weiterer Kenntnisse in einer Azure Proof of Concept-Umgebung](https://aka.ms/aad-poc)

[Bereitstellen von Azure AD in der Produktion](https://aka.ms/aad-onboard)

