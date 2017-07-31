---
title: "Häufig gestellte Fragen zu Azure Active Directory | Microsoft Docs"
description: "In den häufig gestellten Fragen zu Azure Active Directory werden Fragen zum Zugriff auf Azure und Azure Active Directory sowie zur Kennwortverwaltung und zum Anwendungszugriff beantwortet."
services: active-directory
documentationcenter: 
author: MarkusVi
manager: femila
editor: 
ms.assetid: b8207760-9714-4871-93d5-f9893de31c8f
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 07/12/2017
ms.author: markvi
ms.translationtype: Human Translation
ms.sourcegitcommit: 7c4d5e161c9f7af33609be53e7b82f156bb0e33f
ms.openlocfilehash: 899a45d59099dec147c96e4cd175536e859e88f8
ms.contentlocale: de-de
ms.lasthandoff: 05/04/2017

---
# <a name="azure-active-directory-faq"></a>Häufig gestellte Fragen zu Azure Active Directory
Azure Active Directory (Azure AD) ist eine umfassende IDaaS-Lösung (Identity as a Service) für sämtliche Aspekte von Identität, Zugriffsverwaltung und Sicherheit.

Weitere Informationen finden Sie unter [Was ist Azure Active Directory?](active-directory-whatis.md).


## <a name="access-azure-and-azure-active-directory"></a>Zugreifen auf Azure und Azure Active Directory
**F: Warum wird „Keine Abonnements gefunden“ angezeigt, wenn ich versuche, über das klassische Azure-Portal auf Azure AD zuzugreifen (https://manage.windowsazure.com)?**

**A:** Für den Zugriff auf das klassische Azure-Portal benötigt jeder Benutzer Berechtigungen mit einem Azure-Abonnement. Wenn Sie über ein kostenpflichtiges Office 365- oder Azure AD-Abonnement verfügen, können Sie unter [http://aka.ms/accessAAD](http://aka.ms/accessAAD) einen einmaligen Aktivierungsschritt ausführen. Andernfalls müssen Sie ein kostenloses [Azure-Konto](https://azure.microsoft.com/pricing/free-trial/) oder ein kostenpflichtiges Abonnement aktivieren.

Weitere Informationen finden Sie unter:

* [Beziehung zwischen Azure-Abonnements und Azure Active Directory](active-directory-how-subscriptions-associated-directory.md)
* [Verwalten des Verzeichnisses für Ihr Office 365-Abonnement in Azure](active-directory-manage-o365-subscription.md)

- - -
**F: Welche Beziehung besteht zwischen Azure AD, Office 365 und Azure?**

**A:** Mit Azure AD verfügen Sie über allgemeine Identitäts- und Zugriffsfunktionen für alle Webdienste. Wenn Sie Office 365, Microsoft Azure, Intune oder andere Anwendungen nutzen, verwenden Sie bereits Azure AD, um die Anmeldung und Zugriffsverwaltung für diese Dienste zu ermöglichen.

Alle Benutzer, die für die Verwendung von Webdiensten eingerichtet sind, sind als Benutzerkonten in mindestens einer Azure AD-Instanz definiert. Für diese Konten können Sie kostenlose Azure AD-Funktionen wie etwa den Zugriff auf Cloudanwendungen einrichten.

Kostenpflichtige Azure AD-Dienste wie Enterprise Mobility + Security ergänzen andere Webdienste wie Office 365 und Microsoft Azure durch umfassende Verwaltungs- und Sicherheitslösungen für Unternehmen.
- - -
**F: Warum kann ich mich beim Azure-Portal, aber nicht beim klassischen Azure-Portal anmelden?**

**A:** Im Gegensatz zum klassischen Portal wird für das Azure-Portal kein gültiges Abonnement benötigt.  Ohne Abonnement können Sie sich nicht beim klassischen Portal anmelden.
- - -
**F: Was sind die Unterschiede zwischen Abonnementadministrator und Verzeichnisadministrator?**

**A:** Standardmäßig werden Sie der Abonnementadministratorrolle zugewiesen, wenn Sie sich für Azure registrieren. Ein Abonnementadministrator kann entweder ein Microsoft-Konto oder ein Geschäfts-, Schul- oder Unikonto aus dem Verzeichnis verwenden, dem das Azure-Abonnement zugeordnet ist.  Diese Rolle ist zum Verwalten von Diensten im Azure-Portal berechtigt.

Andere Benutzer, die sich mithilfe des gleichen Abonnements anmelden und auf die Dienste zugreifen müssen, können als Co-Administratoren hinzugefügt werden. Diese Rolle verfügt über die gleichen Zugriffsrechte wie der Dienstadministrator, kann jedoch die Zuordnung von Abonnements zu Azure-Verzeichnissen nicht ändern.  Weitere Informationen zu Abonnementadministratoren finden Sie unter [How to add or change Azure administrator roles](../billing-add-change-azure-subscription-administrator.md) (Hinzufügen oder Ändern von Azure-Administratorrollen) sowie unter [Beziehung zwischen Azure-Abonnements und Azure Active Directory](active-directory-how-subscriptions-associated-directory.md).


In Azure AD werden für die Verwaltung von Verzeichnis- und Identitätsfeatures andere Administratorrollen verwendet.  Diese Administratoren haben Zugriff auf verschiedene Features des Azure-Portals oder des klassischen Azure-Portals. Die Administratorrolle bestimmt, welche Aktionen der Administrator ausführen kann. Mögliche Aktionen sind beispielsweise das Erstellen oder Bearbeiten von Benutzern, das Zuweisen von Administratorrollen zu anderen Personen, das Zurücksetzen von Benutzerkennwörtern sowie das Verwalten von Benutzerlizenzen oder Domänen.  Weitere Informationen zu Azure AD-Verzeichnisadministratoren und deren Rollen finden Sie unter [Zuweisen von Administratorrollen in Azure Active Directory](active-directory-assign-admin-roles.md).

Kostenpflichtige Azure AD-Dienste wie Enterprise Mobility + Security ergänzen zudem andere Webdienste wie Office 365 und Microsoft Azure durch umfassende Verwaltungs- und Sicherheitslösungen für Unternehmen.

- - -
**F: Gibt es einen Bericht, der zeigt, wann meine Azure AD-Benutzerlizenzen ablaufen?**

**A:** Nein.  Ein solches Feature ist derzeit nicht verfügbar.

- - -

## <a name="get-started-with-hybrid-azure-ad"></a>Erste Schritte mit Azure AD als Hybridanwendung


**F: Wie verlasse ich einen Mandanten, wenn ich als Projektmitarbeiter hinzugefügt werde?**

**A:** Wenn Sie dem Mandanten einer anderen Organisation als Projektmitarbeiter hinzugefügt werden, können Sie rechts oben über den Mandantenumschalter zwischen Mandanten wechseln.  Derzeit gibt es noch keine Möglichkeit, die einladende Organisation zu verlassen, Microsoft arbeitet aber bereits daran.  In der Zwischenzeit können Sie die einladende Organisation bitten, Sie aus dem Mandanten zu entfernen.
- - -
**F: Wie kann ich eine Verbindung meines lokalen Verzeichnisses mit Azure AD verknüpfen?**

**A:** Die Verbindung zwischen Ihrem lokalen Verzeichnis und Azure AD kann mithilfe von Azure AD Connect hergestellt werden.

Weitere Informationen finden Sie unter [Integrieren Ihrer lokalen Identitäten in Azure Active Directory](active-directory-aadconnect.md).

- - -
**F: Wie richte ich SSO zwischen meinem lokalen Verzeichnis und meinen Cloudanwendungen ein?**

**A:** Einmaliges Anmelden (Single Sign-On, SSO) muss lediglich zwischen Ihrem lokalen Verzeichnis und Azure AD eingerichtet werden. Solange Sie auf Ihre Cloudanwendungen über Azure AD zugreifen, wird vom Dienst für die Benutzer automatisch die richtige Authentifizierung mit ihren lokalen Anmeldeinformationen verwendet.

SSO kann in einer lokalen Umgebung mühelos mithilfe von Verbundlösungen wie Active Directory Federation Services (AD FS) oder durch Konfigurieren der Kennworthashsynchronisierung implementiert werden. Mithilfe des Assistenten zum Konfigurieren von Azure AD Connect können Sie problemlos beide Optionen bereitstellen.

Weitere Informationen finden Sie unter [Integrieren Ihrer lokalen Identitäten in Azure Active Directory](active-directory-aadconnect.md).

- - -
**F: Bietet Azure AD ein Self-Service-Portal für Benutzer in meiner Organisation?**

**A:** Ja. Für Self-Service-Aktionen und den Anwendungszugriff der Benutzer stellt Azure AD den [Azure AD-Zugriffsbereich](http://myapps.microsoft.com) bereit. Für Office 365-Kunden stehen viele dieser Funktionen bereits im Office 365-Portal zur Verfügung.

Weitere Informationen finden Sie unter [Einführung in den Zugriffsbereich](active-directory-saas-access-panel-introduction.md).

- - -
**F: Unterstützt mich Azure AD bei der Verwaltung meiner lokalen Infrastruktur?**

**A:** Ja. In der Premium Edition von Azure AD ist Azure AD Connect Health enthalten. Azure AD Connect Health unterstützt Sie bei der Überwachung Ihrer lokalen Identitätsinfrastruktur und Synchronisierungsdienste und vermittelt Ihnen wichtige Einblicke.  

Ausführlichere Informationen finden Sie unter [Überwachen Ihrer lokalen Identitätsinfrastruktur und Synchronisierung von Diensten in der Cloud](active-directory-aadconnect-health.md).  

- - -
## <a name="password-management"></a>Kennwortverwaltung
**F: Kann ich das Kennwortrückschreiben-Feature von Azure AD ohne Kennwortsynchronisierung verwenden? (Ist es in diesem Szenario möglich, die Self-Service-Kennwortzurücksetzung von Azure AD mit dem Kennwortrückschreiben-Feature zu verwenden und Kennwörter nicht in der Cloud zu speichern?)**

**A:** Sie müssen Ihre Active Directory-Kennwörter nicht mit Azure AD synchronisieren, um das Kennwortrückschreiben-Feature verwenden zu können. Bei Verwendung einer Verbundumgebung nutzt das einmalige Anmelden von Azure AD die Benutzerauthentifizierung des lokalen Verzeichnisses. In diesem Szenario muss das lokale Kennwort nicht in Azure AD nachverfolgt werden.

- - -
**F: Wie lange dauert das Zurückschreiben eines Kennworts in das lokale Active Directory?**

**A:** Kennwörter werden in Echtzeit zurückgeschrieben.

Weitere Informationen finden Sie unter [Erste Schritte mit der Kennwortverwaltung](active-directory-passwords-getting-started.md).

- - -
**F: Kann ich das Kennwortrückschreiben-Feature für Kennwörter verwenden, die von einem Administrator verwaltet werden?**

**A:** Ja. Bei aktiviertem Kennwortrückschreiben werden die von einem Administrator durchgeführten Kennwortvorgänge in Ihre lokale Umgebung zurückgeschrieben.  

Weitere Antworten auf kennwortbezogene Fragen finden Sie unter [Häufig gestellte Fragen zur Kennwortverwaltung](active-directory-passwords-faq.md).
- - -
**F: Was kann ich tun, wenn ich mein vorhandenes Office 365-/Azure AD-Kennwort ändern möchte, aber das alte Kennwort vergessen habe?**

**A:** Für diese Situation stehen mehrere Optionen zur Verfügung.  Verwenden Sie die Self-Service-Kennwortzurücksetzung (Self-Service Password Reset, SSPR), sofern verfügbar.  Ob die SSPR funktioniert, hängt davon ab, wie sie konfiguriert ist.  Weitere Informationen finden Sie unter [Wie funktioniert das Portal für die Kennwortzurücksetzung?](active-directory-passwords-best-practices.md).

Für Office 365-Benutzer kann der Administrator das Kennwort mithilfe der Anleitung unter [Administratoren: Benutzerkennwörter zurücksetzen](https://support.office.com/en-us/article/Admins-Reset-user-passwords-7A5D073B-7FAE-4AA5-8F96-9ECD041ABA9C?ui=en-US&rs=en-US&ad=US) zurücksetzen.

Für Azure AD-Konten können Administratoren Kennwörter mit einer der folgenden Methoden zurücksetzen:

- [Zurücksetzen der Konten im Azure-Portal](active-directory-users-reset-password-azure-portal.md)
- [Zurücksetzen der Konten im klassischen Portal](active-directory-create-users-reset-password.md)
- [Verwenden von PowerShell](/powershell/module/msonline/set-msoluserpassword?view=azureadps-1.0)


- - -
## <a name="security"></a>Sicherheit
**F: Werden Konten nach einer bestimmten Anzahl von Versuchen, bei denen Fehler aufgetreten sind, gesperrt, oder wird eine ausgereiftere Strategie angewandt?**</br>
Wir verwenden eine ausgereiftere Strategie zum Sperren von Konten.  Sie basiert auf der IP-Adresse der Anforderung und den eingegebenen Kennwörtern. Die Dauer der Sperre wird ebenfalls gemäß der Wahrscheinlichkeit erhöht, dass es sich um einen Angriff handelt.  

**F: Bestimmte (häufige) Kennwörter werden mit „Dieses Kennwort wurde zu oft verwendet“-Meldungen zurückgewiesen – bezieht sich dies auf Kennwörter, die im aktuellen Active Directory verwendet wurden?**</br>
Dies bezieht sich auf Kennwörter, die global häufig sind, z.B. alle Varianten von „Password“ und „123456“.

**F: Wird eine Anmeldeanforderung aus fragwürdigen Quellen (Botnets, Tor-Endpunkt) in einem B2C-Mandanten blockiert, oder setzt dies einen Basic- oder Premium Edition-Mandanten voraus?**</br>
Wir haben ein Gateway, das Anforderungen filtert, gewissen Schutz gegen Botnets bietet und für alle B2C-Mandanten gilt.

## <a name="application-access"></a>Anwendungszugriff
**F: Wo finde ich eine Liste mit bereits in Azure AD integrierten Anwendungen und deren Funktionen?**

**A:** Für Azure AD stehen bereits über 2.600 integrierte Anwendungen von Microsoft, Anwendungsdienstanbietern und Partnern zur Verfügung. Alle vorab integrierten Anwendungen unterstützen einmaliges Anmelden (Single Sign-On, SSO). Mit SSO können Sie unter Verwendung der Anmeldeinformationen Ihres Unternehmens auf Ihre Apps zugreifen. Einige Anwendungen unterstützen auch die automatisierte Bereitstellung und Aufhebung der Bereitstellung.

Eine vollständige Liste mit integrierten Anwendungen finden Sie unter [Active Directory-Marketplace](https://azure.microsoft.com/marketplace/active-directory/).

- - -
**F: Was kann ich tun, wenn die Anwendung, die ich benötige, nicht im Azure AD-Marketplace enthalten ist?**

**A:** Mit Azure AD Premium können Sie jede beliebige Anwendung hinzufügen und konfigurieren. Abhängig von den Funktionen Ihrer Anwendung und Ihren Präferenzen können Sie SSO und die automatische Bereitstellung konfigurieren.  

Weitere Informationen finden Sie unter:

* [Konfigurieren des einmaligen Anmeldens für Anwendungen, die nicht im Azure Active Directory-Anwendungskatalog enthalten sind](active-directory-saas-custom-apps.md)
* [Verwenden von SCIM für die automatische Bereitstellung von Benutzern und Gruppen aus Azure Active Directory für Anwendungen](active-directory-scim-provisioning.md)

- - -
**F: Wie melden sich Benutzer mit Azure AD bei Anwendungen an?**

**A:** In Azure AD können Benutzer ihre Anwendungen auf unterschiedliche Weise anzeigen und auf sie zugreifen. Im Anschluss finden Sie einige Beispiele für verfügbare Möglichkeiten:

* Azure AD-Zugriffsbereich
* Office 365-Anwendungsstartprogramm
* Direkte Anmeldung bei Verbund-Apps
* Deep-Links zu verbundenen, kennwortbasierten oder vorhandene Apps

Weitere Informationen finden Sie unter [Bereitstellen von in Azure AD integrierten Anwendungen für Benutzer](active-directory-appssoaccess-whatis.md#deploying-azure-ad-integrated-applications-to-users).

- - -
**F: Auf welche Arten ermöglicht Azure AD die Authentifizierung und das einmalige Anmelden bei Anwendungen?**

**A:** Azure AD unterstützt für die Authentifizierung und Autorisierung viele standardisierte Protokolle wie SAML 2.0, OpenID Connect, OAuth 2.0 und WS-Federation. Außerdem unterstützt Azure AD Kennworttresore und Funktionen für die automatisierte Anmeldung für Apps, die nur die formularbasierte Authentifizierung unterstützen.  

Weitere Informationen finden Sie unter:

* [Authentifizierungsszenarien für Azure AD](active-directory-authentication-scenarios.md)
* [Active Directory-Authentifizierungsprotokolle](https://msdn.microsoft.com/library/azure/dn151124.aspx)
* [Wie funktioniert das einmalige Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md#how-does-single-sign-on-with-azure-active-directory-work)

- - -
**F: Kann ich lokal ausgeführte Anwendungen hinzufügen?**

**A:** Mit dem Azure AD-Anwendungsproxy haben Sie einfachen und sicheren Zugriff auf lokale Webanwendungen Ihrer Wahl. Sie können auf diese Anwendungen auf die gleiche Weise zugreifen wie auf Ihre SaaS-Apps (Software as a Service) in Azure AD. Ein VPN bzw. eine Änderung der Netzwerkinfrastruktur ist nicht erforderlich.  

Weitere Informationen finden Sie unter [Bereitstellen von sicherem Remotezugriff auf lokale Anwendungen](active-directory-application-proxy-get-started.md).

- - -
**F: Wie kann ich für Benutzer, die auf eine bestimmte Anwendung zugreifen, die mehrstufige Authentifizierung erforderlich machen?**

**A:** Mit dem bedingten Zugriff von Azure AD können Sie jeder Anwendung eine individuelle Zugriffsrichtlinie zuweisen. In dieser Richtlinie können Sie festlegen, dass die mehrstufige Authentifizierung immer oder nur dann erforderlich sein soll, wenn Benutzer gerade nicht mit dem lokalen Netzwerk verbunden sind.  

Weitere Informationen finden Sie unter [Bedingter Zugriff mit Azure Active Directory](active-directory-conditional-access.md).

- - -
**F: Was ist die automatisierte Benutzerbereitstellung für SaaS-Apps?**

**A:** Azure AD ermöglicht die Automatisierung der Erstellung, Wartung und Entfernung von Benutzeridentitäten in vielen gängigen SaaS-Cloudanwendungen.

Weitere Informationen finden Sie unter [Automatisieren der Bereitstellung und Bereitstellungsaufhebung von Benutzern für SaaS-Anwendungen mit Azure Active Directory](active-directory-saas-app-provisioning.md).

- - -
**F: Kann ich mit Azure AD eine sichere LDAP-Verbindung einrichten?**

**A:** Nein.  Das LDAP-Protokoll wird von Azure AD nicht unterstützt.

