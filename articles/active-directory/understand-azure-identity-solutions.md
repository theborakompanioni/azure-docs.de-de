---
title: "Grundlegendes zur Azure-Identität | Microsoft-Dokumentation"
description: "Erhalten Sie grundlegende Informationen zu den Begriffen, Konzepten und Empfehlungen der Microsoft Azure-Identitätslösung, um für Ihre Organisation eine fundierte Entscheidung zur Identitätsgovernance zu treffen."
keywords: 
author: jeffgilb
manager: femila
ms.date: 5/12/2017
ms.topic: article
ms.prod: 
ms.service: azure
ms.technology: 
ms.assetid: 
ms.reviewer: jsnow
ms.custom: it-pro
ms.translationtype: Human Translation
ms.sourcegitcommit: 17c4dc6a72328b613f31407aff8b6c9eacd70d9a
ms.openlocfilehash: 8d550a9c2a6d2f28687aefc3b9db97ca3c68eb82
ms.contentlocale: de-de
ms.lasthandoff: 05/16/2017


---
# <a name="understand-azure-identity-solutions"></a>Grundlegendes zu Azure-Identitätslösungen
Microsoft Azure Active Directory (Azure AD) ist eine umfassende, hochverfügbare Cloudlösung für die Identitäts- und Zugriffsverwaltung, die grundlegende Verzeichnisdienste, erweiterte Identitätsgovernance und Zugriffsverwaltung für Anwendungen kombiniert. Azure AD ermöglicht schnell das [einmalige Anmelden (SSO)](https://docs.microsoft.com/azure/active-directory/active-directory-enterprise-apps-manage-sso) bei Tausenden von vorintegrierten kommerziellen und benutzerdefinierten Apps im [Azure AD-Anwendungskatalog](https://azure.microsoft.com/marketplace/active-directory/all/). Dazu gehören auch die Apps, die Sie täglich verwenden, wie z.B. Office 365, Salesforce.com, Box, ServiceNow und Workday.

In der einfachsten Form wird die Azure-Identität durch ein einziges Azure AD-Verzeichnis bereitgestellt, das bei seiner Erstellung automatisch einem Azure-Abonnement zugeordnet wird. Als Identitätsdienst in Azure stellt Azure AD anschließend sämtliche Funktionen für die Identitätsverwaltung und Zugriffssteuerung für cloudbasierte Ressourcen wie Benutzer, Apps und Gruppen für einen einzelnen Mandanten (Organisation) bereit, wie im folgenden Diagramm dargestellt:

![Azure Active Directory](./media/understand-azure-identity-solutions/azure-ad.png)

Für die individuellen Anforderungen Ihrer Organisation bietet Microsoft Azure mehrere Möglichkeiten mit unterschiedlichem Komplexitätsgrad, um die Identität als Dienst (IDaaS) zu nutzen. Im weiteren Verlauf dieses Artikels lernen Sie die für Sie relevante Terminologie und Konzepte kennen und erhalten Empfehlungen, damit Sie aus den verfügbaren Azure-Identitätsoptionen die ideale Auswahl treffen können.

## <a name="terms-to-know"></a>Relevante Terminologie

Bevor Sie eine Entscheidung hinsichtlich der Azure-Identitätsdienstlösung für Ihre Organisation treffen können, müssen Sie die Begriffe kennen, die im Zusammenhang mit den Azure-Identitätsdiensten häufig verwendet werden.

|Begriff| Beschreibung|
|-----|-----|
|Azure-Abonnement |Abonnements werden zum Bezahlen von Azure-Clouddiensten verwendet und sind in der Regel mit einer Kreditkarte verknüpft. Sie können mehrere Abonnements besitzen, aber es kann schwierig sein, Ressourcen über mehrere Abonnements gemeinsam zu verwenden.|
|Azure-Mandant | Ein Azure AD-Mandant steht für eine einzelne Organisation. Es handelt sich um eine dedizierte, vertrauenswürdige Instanz von Azure AD, die eine Organisation erhält und besitzt, sobald sie sich für ein Abonnement für einen Microsoft-Clouddienst wie Azure, Intune oder Office 365 registriert hat. Mandanten können Zugriff auf Dienste in einer dedizierten Umgebung (einzelner Mandant) oder in einer mit anderen Organisationen gemeinsam genutzten Umgebung (mehrinstanzenfähig) erhalten.|
|Azure AD-Verzeichnis | Jeder Azure-Mandant verfügt über ein dediziertes, vertrauenswürdiges Azure AD-Verzeichnis mit den Benutzern, Gruppen und Anwendungen des Mandanten. Es wird zum Ausführen von Funktionen zur Identitäts- und Zugriffsverwaltung für Mandantenressourcen verwendet. Da automatisch ein eindeutiges Azure AD-Verzeichnis für Ihre Organisation bereitgestellt wird, sobald Sie sich für einen Microsoft-Clouddienst wie Azure, Microsoft Intune oder Office 365 registrieren, werden die Begriffe *Mandant*, *Azure AD* und *Azure AD-Verzeichnis* gelegentlich synonym verwendet. |
|Benutzerdefinierte Domäne | Wenn Sie sich erstmals für ein Abonnement eines Microsoft-Clouddiensts registrieren, erstellt Ihr Mandant (Ihre Organisation) einen Domänennamen *.onmicrosoft.com*. Die meisten Organisationen besitzen jedoch mindestens einen Domänennamen, der für die Geschäftstätigkeit und von Endbenutzern für den Zugriff auf Unternehmensressourcen verwendet wird. Sie können Ihren benutzerdefinierten Domänennamen in Azure AD hinzufügen, damit der Domänenname Ihren Benutzern bekannt ist, z.B. *alice@contoso.com* anstelle von *alice@contoso.onmicrosoft.com*. |
|Azure AD-Konto | Hierbei handelt es sich um Identitäten, die über Azure AD oder einen anderen Microsoft-Clouddienst wie Office 365 erstellt werden. Sie werden im Azure AD eines Mandanten gespeichert und sind für jedes Clouddienstabonnement der Organisation zugänglich. |
|Azure-Abonnementadministrator| Der Kontoadministrator ist die Person, die sich für das Azure-Abonnement registriert bzw. die es erworben hat. Die Person ist zum Zugreifen auf das [Kontocenter](https://account.windowsazure.com/Home/Index) und Durchführen verschiedener Verwaltungsaufgaben berechtigt, z.B. Erstellen von Abonnements, Kündigen von Abonnements, Ändern der Abrechnung für ein Abonnement oder Ändern des Dienstadministrators. |
|Globaler Azure AD-Administrator | Globale Azure AD-Administratoren besitzen Vollzugriff auf alle Verwaltungsfunktionen von Azure AD. Die Person, die sich für ein Microsoft-Clouddienstabonnement registriert, wird standardmäßig automatisch als globaler Administrator festgelegt. Sie können mehrere globale Administratoren festlegen, aber nur globale Administratoren können Benutzern eine der [weiteren Administratorrollen](https://docs.microsoft.com/azure/active-directory/active-directory-assign-admin-roles-azure-portal) zuweisen. |
|Microsoft-Konto | Microsoft-Konten (von Ihnen für den privaten Gebrauch erstellt) bieten Zugriff auf alle für den normalen Computerbenutzer entwickelten Microsoft-Produkte und Clouddienste wie Outlook (Hotmail), Messenger, OneDrive, MSN, Xbox LIVE oder Office 365. Diese Identitäten werden im Microsoft-Kontosystem für Endbenutzeridentitäten erstellt und gespeichert, das von Microsoft geführt wird.|
|Geschäfts-, Schul- oder Unikonten | Geschäfts-, Schul- oder Unikonten (von einem Administrator zu Geschäfts-/Bildungszwecken vergeben) bieten Zugriff auf alle Microsoft-Clouddienste für Unternehmen, darunter Azure, Intune oder Office 365.|


## <a name="concepts-to-understand"></a>Konzepte

Da Ihnen jetzt die grundlegende Identitätsterminologie vertraut ist, können Sie sich mit den folgenden grundlegenden Konzepten befassen, um eine fundierte Entscheidung zu Azure-Identitätsdiensten zu treffen.

|Konzept |Beschreibung|
|-----|-----|
|[Beziehung zwischen Azure-Abonnements und Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-how-subscriptions-associated-directory) |Jedes Azure-Abonnement verfügt über eine Vertrauensstellung bei einem Azure AD-Verzeichnis zur Authentifizierung von Benutzern, Diensten und Geräten. *Mehrere Abonnements können demselben Azure AD-Verzeichnis vertrauen, aber ein Abonnement vertraut immer nur einem einzigen Azure AD-Verzeichnis*. Diese Vertrauensstellung unterscheidet sich von der Beziehung, die ein Abonnement mit anderen Azure-Ressourcen aufweist (Websites, Datenbanken usw.). Diese ähneln eher den untergeordneten Ressourcen eines Abonnements. Wenn ein Abonnement abläuft, wird der Zugriff auf die ihm zugeordneten Ressourcen mit Ausnahme von Azure AD ebenfalls beendet. Das Azure AD-Verzeichnis verbleibt jedoch in Azure, und Sie können diesem Verzeichnis ein anderes Abonnement zuordnen und die Mandantenressourcen weiterhin verwalten.|
|[Funktionsweise der Azure AD-Lizenzierung](https://docs.microsoft.com/azure/active-directory/active-directory-licensing-get-started-azure-portal) | Wenn Sie Enterprise Mobility Suite, Azure AD Premium oder Azure AD Basic erwerben oder aktivieren, wird das Verzeichnis mit dem Abonnement aktualisiert, einschließlich der Gültigkeitsdauer und der vorausbezahlten Lizenzen. Nach Aktivierung des Abonnements können die Dienstfunktionen von globalen Azure AD-Administratoren verwaltet und von lizenzierten Benutzern verwendet werden. Ihre Abonnementinformationen (einschließlich der Anzahl zugewiesener oder verfügbarer Lizenzen) stehen im Azure-Portal auf dem Blatt **Azure Active Directory** > **Lizenzen**. Dies ist auch der beste Ort für die Verwaltung Ihrer Lizenzzuweisungen.|
|[Rollenbasierte Zugriffssteuerung im Azure-Portal](https://docs.microsoft.com/azure/active-directory/role-based-access-control-what-is)|Die rollenbasierte Azure-Zugriffssteuerung (Role-Based Access Control, RBAC) ermöglicht eine präzise Verwaltung des Zugriffs für Azure-Ressourcen. Zu viele Berechtigungen können ein Konto zum leichten Angriffsziel machen. Wenn die Berechtigungen nicht ausreichen, können Mitarbeiter nicht effizient arbeiten. Mithilfe der RBAC können Sie Mitarbeitern genau die Berechtigungen erteilen, die sie brauchen. Dazu verwenden Sie drei grundlegende Rollen, die für alle Ressourcengruppen gelten: Besitzer, Mitwirkender, Leser. Außerdem können Sie bis zu 2.000 eigene [benutzerdefinierte RBAC-Rollen](https://docs.microsoft.com/azure/active-directory/role-based-access-control-custom-roles) erstellen, die Ihren speziellen Anforderungen entsprechen. |
|[Hybrididentität](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect)|Hybrididentität wird erzielt, indem Sie Ihr lokales Windows Server Active Directory (AD DS) über [Azure AD Connect](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect) in Azure AD integrieren. So können Sie für Ihre Benutzer eine einzige Identität für in Azure AD integrierte Office 365-, Azure- und lokale Apps oder SaaS-Anwendungen bereitstellen. Mit Hybrididentität erweitern Sie effektiv Ihre lokale Umgebung in die Cloud, um die Identitäts- und Zugriffsverwaltung zu ermöglichen.|

### <a name="the-difference-between-windows-server-ad-ds-and-azure-ad"></a>Der Unterschied zwischen Windows Server AD DS und Azure AD
Wenn Sie bereits mit den lokalen Active Directory Domain Services (AD DS) von Windows Server vertraut sind, die erstmals mit Windows 2000 Server eingeführt wurden, ist Ihnen das grundlegende Konzept eines Identitätsdiensts wahrscheinlich bekannt. Allerdings ist unbedingt zu beachten, dass es sich bei Azure AD nicht einfach um einen Domänencontroller in der Cloud handelt. Vielmehr bietet es eine ganz neue Möglichkeit der IDaaS-Bereitstellung (Identity-as-a-Service) in Azure, die eine völlig neue Denkweise erfordert, um die cloudbasierten Funktionen voll auszuschöpfen und Ihre Organisation vor modernen Bedrohungen zu schützen. Die IT schützt ihren Sicherheitsumkreis seit Jahren mithilfe von AD DS, aber moderne Unternehmen ohne Umkreisnetzwerk, die die Identitätsanforderungen von Mitarbeitern, Kunden und Partnern unterstützen, benötigen eine neue Steuerungsebene. Diese Ebene der Identitätssteuerung wird von Azure AD bereitgestellt.

Die Sicherheit hat sich über die Unternehmensfirewall hinaus in die Cloud verlagert, wo Azure AD für den Schutz der Unternehmensressourcen und des Zugriffs sorgt, indem eine gemeinsame Identität für Benutzer (entweder lokal oder in der Cloud) bereitgestellt wird. So erhalten Ihre Benutzer die Flexibilität, mit nahezu jedem Gerät sicher auf die Apps zuzugreifen, die sie für ihre alltägliche Arbeit benötigen. Zudem werden nahtlos Steuerelemente für den risikobasierten Datenschutz bereitgestellt, die durch Machine Learning-Funktionen und ausführliche Berichterstellung unterstützt werden, um der IT-Abteilung das Absichern von Unternehmensdaten zu ermöglichen.

### <a name="extend-office-365-management-and-security-capabilities"></a>Erweitern von Office 365-Verwaltungs- und -Sicherheitsfunktionen
Verwenden Sie bereits Office 365? Sie können Ihre digitale Transformation beschleunigen, indem Sie die integrierten Office 365-Verwaltungs- und -Sicherheitsfunktionen auf Azure AD erweitern, um den Zugriff auf all Ihre Ressourcen zu sichern und Ihrer gesamten Belegschaft eine reibungslose, sichere Produktivität zu ermöglichen. Wenn Sie zusätzlich zu den in Office 365 verfügbaren Funktionen Azure AD-Funktionen verwenden, können Sie Ihr gesamtes Anwendungsportfolio mit einer einzigen Identität absichern, die einmaliges Anmelden für alle Apps ermöglicht. Sie können Ihre Funktionen für den bedingten Zugriff erweitern, sodass sie nicht nur auf dem Gerätestatus, sondern auch auf Benutzer, Standort, Anwendung und Risiko beruhen. MFA-Funktionen (Multi-Factor Authentication) bieten bei Bedarf sogar noch mehr Schutz. Sie erhalten eine zusätzliche Übersicht über Benutzerberechtigungen sowie bedarfsgerechten Just-in-Time-Administratorzugriff. Ihre Benutzer können produktiver sein und verursachen dank der Self-Service-Funktionen von Azure AD weniger Helpdesktickets. Diese Funktionen umfassen beispielsweise das Zurücksetzen vergessener Kennwörter, Anwendungszugriffsanforderungen sowie die Erstellung und Verwaltung von Gruppen.

> [!TIP]
> Wünschen Sie weitere Informationen zur Verwendung der Azure AD-Identitätsverwaltung mit Office 365? [Rufen Sie das E-Book ab](https://info.microsoft.com/Extend-Office-365-security-with-EMS.html).

## <a name="microsoft-identity-solutions"></a>Microsoft-Identitätslösungen

Microsoft Azure bietet verschiedene Methoden zum Verwalten von Benutzeridentitäten, unabhängig davon, ob sie vollständig lokal, nur in der Cloud oder sogar irgendwo dazwischen verwaltet werden. Zu den verfügbaren Optionen gehören: AD DS im DIY-Verfahren (Do-it-Yourself) in Azure, Azure Active Directory (Azure AD), Hybrididentität und Azure AD Domain Services.

Für Unternehmen, die nur einen geringen Ressourcenbedarf in der Cloud haben, kann **AD DS im DIY-Verfahren** in Azure verwendet werden. Diese Option unterstützt viele Windows Server-AD DS-Szenarien, die für die Bereitstellung als virtuelle Computer (VMs) in Azure gut geeignet sind. Beispielsweise können Sie einen virtuellen Azure-Computer als Domänencontroller erstellen, der in einem weit entfernten Rechenzentrum ausgeführt wird und mit dem Remotenetzwerk verbunden ist. Von dort aus kann der virtuelle Computer Authentifizierungsanforderungen von Remotebenutzern unterstützen und die Leistungsfähigkeit der Authentifizierung verbessern. Diese Option eignet sich außerdem als relativ kostengünstige Ersatzlösung für ansonsten kostspielige Notfallwiederherstellungsstandorte, da eine geringe Anzahl von Domänencontrollern und ein einziges virtuelles Netzwerk in Azure gehostet werden. Es kann auch sein, dass Sie eine Anwendung in Azure bereitstellen möchten, z.B. SharePoint. Unter Umständen ist hierfür Windows Server AD DS erforderlich, aber es besteht keine Abhängigkeit vom lokalen Netzwerk oder von der Unternehmensinstanz von Windows Server Active Directory. In diesem Fall können Sie eine isolierte Gesamtstruktur unter Azure bereitstellen, um die Anforderungen der SharePoint-Serverfarm zu erfüllen. Außerdem wird die Bereitstellung von Netzwerkanwendungen unterstützt, für die die Konnektivität mit dem lokalen Netzwerk und dem lokalen Active Directory erforderlich ist.

Die eigenständige Version von **Azure Active Directory (Azure AD)** ist eine vollständig cloudbasierte Lösung zur Identitäts- und Zugriffsverwaltung als Dienst (IDaaS). Azure AD bietet Ihnen eine Reihe robuster Funktionen zur Verwaltung von Benutzern und Gruppen. Damit können Sie den sicheren Zugriff auf lokale und Cloudanwendungen gewährleisten, beispielsweise auf Microsoft-Webdienste wie Office 365 oder zahlreiche SaaS-Anwendungen (Software-as-a-Service) anderer Anbieter. Azure AD ist in drei Editionen erhältlich: Free, Basic und Premium. Azure AD steigert die Effektivität von Organisationen und erweitert die Sicherheit über die Umkreisfirewall hinaus auf eine neue Steuerungsebene, die durch Azure Machine Learning und andere erweiterte Sicherheitsfunktionen geschützt wird.

Statt sich zwischen lokalen oder cloudbasierten Identitätslösungen zu entscheiden, dehnen viele zukunftsorientierte CIOs und Unternehmen, die ihre langfristige Ausrichtung planen, ihre lokalen Verzeichnisse mithilfe von **Hybrididentitätslösungen** in die Cloud aus. Durch die Hybrididentität erhalten Sie sowohl jetzt als auch in Zukunft eine globale Lösung zur Identitäts- und Zugriffsverwaltung, die einen sicheren und produktiven Zugriff auf die Anwendungen bietet, die Ihre Benutzer für ihre Aufgaben benötigen.

> [!TIP]
> Um zu erfahren, wie CIOs Azure Active Directory zu einem zentralen Bestandteil ihrer IT-Strategien gemacht haben, laden Sie den [CIO’s Guide to Azure Active Directory](https://aka.ms/AzureADCIOGuide) (CIO-Leitfaden für Azure Active Directory) herunter.

**Azure AD Domain Services** bieten eine cloudbasierte Option zur Nutzung von AD DS für eine sehr einfache Steuerung der Azure-VM-Konfiguration sowie eine Möglichkeit zum Erfüllen lokaler Identitätsanforderungen für das Entwickeln und Testen von Netzwerkanwendungen. Der Zweck von Azure AD Domain Services besteht nicht darin, Ihre lokale AD DS-Infrastruktur per Lift & Shift zu virtuellen Azure-Computern zu migrieren, die von Azure AD Domain Services verwaltet werden. Stattdessen sollen die virtuellen Azure-Computer in verwalteten Domänen verwendet werden, um die Entwicklung, das Testen und die Verlagerung lokaler Anwendungen, die AD DS-Authentifizierungsmethoden benötigen, in die Cloud zu unterstützen.

## <a name="common-scenarios-and-recommendations"></a>Gängige Szenarien und Empfehlungen

Im Folgenden sind einige Identitäts- und Zugriffsszenarien mit Empfehlungen zur Auswahl der jeweils geeigneten Azure-Identitätsoption aufgeführt.

  |Identitätsszenario| Empfehlung|
  |-----|-----|
  |Ich möchte eine kleine Anzahl virtueller Computer in Azure unterstützen, aber mein Unternehmen hat umfangreiche Investitionen in lokales Active Directory (AD DS) getätigt.|Verwenden Sie [AD DS im DIY-Verfahren](https://msdn.microsoft.com/library/azure/jj156090.aspx) für die Nutzung virtueller Azure-Computer, wenn Sie eine geringe Anzahl virtueller Computer unterstützen möchten und große lokale AD DS-Investitionen getätigt haben. |
  |Mein Unternehmen basiert vollständig auf der Cloud, und wir haben keinerlei Investitionen in lokale Identitätslösungen getätigt.| [Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-whatis) ist die beste Wahl für rein cloudbasierte Unternehmen ohne Investitionen in eine lokale Infrastruktur.|
  |Meine Organisation hat umfangreich in lokales Windows Server Active Directory investiert, aber wir möchten die Identitätsverwaltung auf die Cloud ausdehnen.| Die am weitesten verbreitete Azure-Identitätslösung ist die [Hybrididentität](https://docs.microsoft.com/azure/active-directory/active-directory-hybrid-identity-design-considerations-overview). Wenn Sie bereits Investitionen in lokale AD DS getätigt haben, können Sie die Identitätsverwaltung mithilfe von Azure AD Connect ganz einfach auf die Cloud ausdehnen.|
  |Ich benötige eine sehr einfache Möglichkeit zur Azure-VM-Konfiguration und -Steuerung, um die lokalen Identitätsanforderungen an App-Entwicklung und -Tests zu erfüllen.|[Azure AD Domain Services](https://docs.microsoft.com/azure/active-directory-domain-services/active-directory-ds-overview) ist eine gute Wahl, wenn Sie AD DS für eine sehr einfache Azure-VM-Konfigurationssteuerung benötigen oder verzeichnisabhängige, lokale Legacyanwendungen zur Cloud migrieren oder dort entwickeln möchten.|

## <a name="next-steps"></a>Nächste Schritte

Da Sie jetzt die Azure-Identitätskonzepte und die Ihnen zur Verfügung stehenden Optionen kennen, können Sie die folgenden Ressourcen verwenden, um mit der Implementierung der Option Ihrer Wahl zu beginnen:

[Testen Ihrer Kenntnisse zu Azure-Identitätslösungen](https://aka.ms/aad-understand-quiz)

[Erlangen weiterer Kenntnisse in einer Azure Proof of Concept-Umgebung](https://aka.ms/aad-poc)

[Bereitstellen von Azure AD in der Produktion](https://aka.ms/aad-onboard)

