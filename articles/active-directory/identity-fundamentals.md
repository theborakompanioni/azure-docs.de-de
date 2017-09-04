---
title: "Grundlagen der Azure-Identitätsverwaltung | Microsoft-Dokumentation"
description: "Cloudbasierte Identitäten sind nun die beste Möglichkeit, die Kontrolle darüber und den Einblick darin zu behalten, wie und wann Benutzer auf Unternehmensanwendungen und -daten zugreifen."
keywords: 
author: jeffgilb
manager: femila
ms.reviewr: jsnow
ms.author: jeffgilb
ms.date: 07/05/2017
ms.topic: article
ms.prod: 
ms.service: azure
ms.technology: 
ms.assetid: 
ms.custom: it-pro
ms.translationtype: HT
ms.sourcegitcommit: 1c730c65194e169121e3ad1d1423963ee3ced8da
ms.openlocfilehash: 52f05ee8a5c07fc008da40aef12d1ad8e8136429
ms.contentlocale: de-de
ms.lasthandoff: 08/30/2017

---
# <a name="fundamentals-of-azure-identity-management"></a>Grundlagen der Azure-Identitätsverwaltung
Da sich immer mehr digitale Unternehmensressourcen außerhalb des Unternehmensnetzwerks, in der Cloud und auf Geräten befinden, wird eine herausragende, cloudbasierte Identitäts- und Zugriffsverwaltungslösung zur Notwendigkeit. Cloudbasierte Identitäten sind nun die beste Möglichkeit, die Kontrolle darüber und den Einblick darin zu behalten, wie und wann Benutzer auf Unternehmensanwendungen und -daten zugreifen.

Microsoft beschäftigt sich schon seit mehr als einem Jahrzehnt mit dem Schutz cloudbasierter Identitäten, und diese Schutzsysteme stehen mit [Azure Active Directory (AD)](https://docs.microsoft.com/azure/active-directory/active-directory-editions) jetzt auch Ihnen zur Verfügung. Mit Azure AD können Unternehmensadministratoren den Benutzern und Administratoren problemlos mehr Sicherheit und Kontrolle als je zuvor an die Hand zu geben.

Azure AD Premium ist eine cloudbasierte Identitäts- und Zugriffsverwaltungslösung mit erweiterten Schutzfunktionen, die eine sichere Identität für alle Apps, Schutz der Identität (verbessert durch den [Microsoft Intelligence Security Graph](https://www.microsoft.com/en-us/security/intelligence)) und Privileged Identity Management bietet. Azure AD Premium ist nicht einfach ein weiteres Überwachungs- und Berichterstellung – es kann die Identitäten Ihrer Benutzer in Echtzeit schützen und ermöglicht Ihnen das Erstellen risikobasierter, adaptiver Zugriffsrichtlinien zum Schutz der Daten Ihrer Organisation.

In diesem kurzen Video erhalten Sie einen schnellen Überblick über Identitätsverwaltung und Schutz mit Azure AD:
<iframe width="560" height="315" src="https://www.youtube.com/embed/9LGIJ2-FKIM" frameborder="0" allowfullscreen></iframe>

Microsoft stellt nicht nur die Identität bereit, mit der Sie umfassenden Zugriff haben, sondern auch einen Satz von Tools zum Automatisieren, Sichern und Verwalten der IT in Ihrer Organisation. Auch nach der Einführung von Cloud Computing besteht immer noch eine Nachfrage nach der Verwaltung und Steuerung von IT-Aufgaben wie z.B. Anrufe beim Helpdesk, um Benutzerkennwörter zurückzusetzen, Benutzergruppenverwaltung und Anwendungsanforderungen. Dass Mitarbeiter jetzt ihre persönlichen Geräte zur Arbeit mitbringen und frei verfügbare SaaS-Anwendungen nutzen, macht die Dinge noch komplizierter. So wird das Aufrechterhalten der Kontrolle über Anwendungen in den Unternehmensrechenzentren und auf öffentlichen Cloudplattformen zu einer erheblichen Herausforderung.

[!INCLUDE [identity](../../includes/azure-ad-licenses.md)]

## <a name="connect-on-premises-active-directory-with-azure-ad-and-office-365"></a>Verbinden einer lokalen Active Directory-Instanz mit Azure AD und Office 365
Organisationen, die umfangreiche Investitionen in eine lokale Active Directory-Struktur getätigt haben, können diese Investitionen in die Cloud erweitern, indem sie ihre lokalen Verzeichnisse mit Azure AD in eine [Hybrididentitätsverwaltung](https://docs.microsoft.com/azure/active-directory/active-directory-hybrid-identity-design-considerations-overview) integrieren. Dadurch werden Benutzer produktiver, da eine allgemeine Identität für den standortunabhängigen Zugriff auf Ressourcen bereitgestellt wird. Benutzer und Organisationen können dann das einmalige Anmelden (SSO) für den Zugriff sowohl auf lokale Ressourcen als auch Clouddienste wie Office 365 nutzen.

[Azure AD Connect](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect) ist das einzige Tool, das Sie für die Integration benötigen. Azure AD Connect bietet die Funktionen, um Ihre Anforderungen an Identitätssynchronisierung zu erfüllen, und ersetzt ältere Versionen von Identitätsintegrationstools wie DirSync und Azure AD Sync. Mit Azure AD wird die Identitätsverwaltung und -synchronisierung zwischen lokaler Umgebung und Azure AD durch folgende Funktionen ermöglicht:

- Synchronisierung: Diese Komponente ist verantwortlich für das Erstellen von Benutzern, Gruppen und anderen Objekten. Er stellt ebenfalls sicher, dass Identitätsinformationen für Ihre lokalen Benutzer und Gruppen denen in der Cloud entsprechen. Kennwortrückschreiben kann auch aktiviert werden, um lokale Verzeichnisse synchron zu halten, wenn ein Benutzer sein Kennwort in Azure AD aktualisiert.
- AD FS: Der Verbund ist eine optionale Funktion von Azure AD Connect, die zum Konfigurieren einer Hybridumgebung mithilfe einer lokalen AD FS-Infrastruktur verwendet werden kann. Der Verbund kann von Organisationen verwendet werden, um sich mit komplexen Bereitstellungen zu befassen, z.B. einmaliges Anmelden, Erzwingen von AD-Anmelderichtlinien und Smartcard- bzw. Drittanbieter-MFA.
- Systemüberwachung: [Azure AD Connect Health](https://docs.microsoft.com/azure/active-directory/connect-health/active-directory-aadconnect-health) bietet eine stabile Überwachung und einen zentralen Speicherort im Azure-Portal, um diese Aktivität anzuzeigen.

## <a name="increase-productivity-and-reduce-helpdesk-costs-with-self-service-and-single-sign-on-experiences"></a>Mehr Produktivität und weniger Helpdeskkosten durch Self-Service und Single Sign-On

Mitarbeiter sind produktiver, wenn sie sich nur einen Benutzernamen und ein Kennwort merken müssen und eine einheitliche Umgebung auf allen Geräten nutzen können. Außerdem wird Zeit gespart, wenn Mitarbeiter Self-Service-Aufgaben wie das [Zurücksetzen eines vergessenen Kennworts](https://docs.microsoft.com/azure/active-directory/active-directory-passwords) oder das Anfordern des Zugriffs auf eine Anwendung durchführen können, ohne auf Unterstützung durch den Helpdesk zu warten.

Azure AD [weitet das lokale Active Directory](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect) auf die Cloud aus und ermöglicht den Benutzern auf diese Weise, sich mit ihrem primären Organisationskonto sowohl bei den mit ihrer Domäne verbundenen Geräten und Unternehmensressourcen als auch bei sämtlichen Web- und SaaS-Anwendungen anzumelden, die sie für ihre Arbeit benötigen. Neben dem Vorteil, dass sich Benutzer keine Vielzahl von Benutzernamen und Kennwörtern mehr merken müssen, kann der Anwendungszugriff für Benutzer kann basierend auf ihrer Gruppenmitgliedschaft in der Organisation sowie ihrem Mitarbeiterstatus automatisch bereitgestellt (oder deaktiviert) werden. Und Sie können diesen Zugriff für Katalog-Apps oder für Ihre eigenen lokalen Apps steuern, die Sie über den [Azure AD-Anwendungsproxy](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-get-started) entwickelt und veröffentlicht haben.

## <a name="manage-and-control-access-to-corporate-resources"></a>Verwalten und Steuern des Zugriffs auf Unternehmensressourcen
Lösungen zur Identitäts- und Zugriffsverwaltung von Microsoft unterstützen IT-Profis dabei, den Zugriff auf Anwendungen und Ressourcen über das Unternehmensrechenzentrum und in der Cloud zu schützen, wobei zusätzliche Ebenen der Überprüfungen aktiviert werden, z.B. [mehrstufige Authentifizierung](https://docs.microsoft.com/azure/multi-factor-authentication/multi-factor-authentication-whats-next) und [Richtlinien für bedingten Zugriff](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-azure-portal). Die Überwachung verdächtiger Aktivitäten über erweiterte Sicherheitsberichtserstellung, Überwachung und Warnung trägt dazu bei, potenzielle Sicherheitsprobleme zu verringern.

Richtlinien für bedingten Zugriff in Azure AD Premium bieten Ihnen als Unternehmensadministrator die Möglichkeit zum Erstellen von richtlinienbasierten Zugriffsregeln für jede mit Azure AD verbundene Anwendung (SaaS-Apps, in der Cloud ausgeführte benutzerdefinierte Apps oder lokale Webanwendungen). Azure AD wertet diese Richtlinien in Echtzeit aus, und setzt sie durch, wenn ein Benutzer versucht, auf eine Anwendung zuzugreifen. Mit Azure-Identitätsschutzrichtlinien können Sie automatisch Maßnahmen ergreifen, wenn verdächtige Aktivität erkannt wird. Diese Aktionen können das Blockieren des Zugriffs für Benutzer mit hohem Risiko, Erzwingen der Multi-Factor Authentication und Zurücksetzen von Benutzerkennwörtern, wenn anscheinend Anmeldeinformationen beschädigt wurden, umfassen.


## <a name="azure-active-directory-privileged-identity-management"></a>Azure Active Directory Privileged Identity Management

[Privileged Identity Management](https://docs.microsoft.com/azure/active-directory/active-directory-privileged-identity-management-getting-started) ist im Azure Active Directory Premium P2-Angebot enthalten und ermöglicht Ihnen das Ermitteln, Einschränken und Überwachen von Administratorkonten und deren Zugriff auf Ressourcen in Ihrem Azure Active Directory und anderen Microsoft-Onlinediensten. Außerdem können Sie damit bei Bedarf Administratorzugriff für genau die Zeitspanne verwalten, die Sie benötigen.

Privileged Identity Management kann bei Bedarf Administratorrechte erzwingen, sodass Administratoren mit mehrstufiger Authentifizierung eine temporäre Erweiterung ihrer Berechtigungen für vorkonfigurierte Zeiträume anfordern können, bevor ihre Konten wieder in einen normalen Benutzerzustand zurückversetzt werden.

## <a name="benefits-of-azure-identity"></a>Vorteile einer Azure-Identität

Mit der Azure-Identitätsverwaltung haben Sie folgende Möglichkeiten:

-   Erstellen und Verwalten einer einzelnen Identität für jeden Benutzer in Ihrer gesamten Unternehmensumgebung, wobei Benutzer, Gruppen und Geräte mit [Azure Active Directory Connect](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect) synchronisiert werden.

-   Bereitstellen des Zugriffs über Single Sign-On auf Ihre Anwendungen, einschließlich tausende vorab integrierter SaaS-Apps, oder Bereitstellen von sicherem Remotezugriff auf lokale SaaS-Anwendungen mithilfe des [Azure AD-Anwendungsproxys](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-get-started)

-   Aktivieren der Sicherheit für den Anwendungszugriff durch Erzwingen der regelbasierten [Multi-Factor Authentication](https://docs.microsoft.com/azure/multi-factor-authentication/multi-factor-authentication-whats-next) für lokale Anwendungen und Cloudanwendungen

-   Verbessern der Benutzerproduktivität durch [Self-Service-Kennwortzurücksetzung](https://docs.microsoft.com/azure/active-directory/active-directory-passwords) sowie Gruppen- und Anwendungszugriffsanforderungen mithilfe des [MyApps-Portals](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-user-help)

-   Nutzen der [hohen Verfügbarkeit und Zuverlässigkeit](https://docs.microsoft.com/azure/architecture/resiliency/high-availability-azure-applications) einer Lösung für die Identitäts- und Zugriffsverwaltung, die weltweit verfügbar, für Großunternehmen geeignet und cloudbasiert ist

## <a name="next-steps"></a>Nächste Schritte
[Weitere Informationen über Azure-Identitätslösungen](https://docs.microsoft.com/azure/active-directory/understand-azure-identity-solutions)
