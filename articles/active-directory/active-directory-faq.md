<properties
	pageTitle="Häufig gestellte Fragen zu Azure Active Directory | Microsoft Azure"
	description="Häufig gestellte Fragen zu Azure Active Directory und Antworten in Bezug auf den Zugriff auf Azure und Azure Active Directory, Kennwortverwaltung und Anwendungszugriff."
	services="active-directory"
	documentationCenter=""
	authors="markusvi"
	manager="femila"
	editor=""/>

<tags
	ms.service="active-directory"
	ms.workload="identity"
	ms.tgt_pltfrm="na"
	ms.devlang="na"
	ms.topic="get-started-article"
	ms.date="08/16/2016"
	ms.author="markusvi"/>

# Häufig gestellte Fragen zu Azure Active Directory

Azure Active Directory ist eine umfassende IDaaS-Lösung (Identity as a Service), mit der alle Aspekte von Identität, Zugriffsverwaltung und Sicherheit abgedeckt werden.


Weitere Informationen finden Sie unter [Was ist Azure Active Directory?](active-directory-whatis.md).



## Zugreifen auf Azure und Azure Active Directory


**F: Warum wird „Keine Abonnements gefunden“ angezeigt, wenn ich versuche, über das klassische Azure-Portal auf Azure AD zuzugreifen (https://manage.windowsazure.com)?**

**A:** Für den Zugriff auf das klassische Azure-Portal müssen alle Benutzer über Berechtigungen für ein Azure-Abonnement verfügen. Wenn Sie über eine kostenpflichtige Version von Office 365 oder Azure AD verfügen, können Sie unter [http://aka.ms/accessAAD](http://aka.ms/accessAAD) einen einmaligen Aktivierungsschritt durchführen. Andernfalls müssen Sie eine vollwertige [Azure-Testversion](https://azure.microsoft.com/pricing/free-trial/) oder ein kostenpflichtiges Abonnement aktivieren.

Weitere Informationen finden Sie unter:

- [Beziehung zwischen Azure-Abonnements und Azure Active Directory](active-directory-how-subscriptions-associated-directory.md)

- [Verwalten des Verzeichnisses für Ihr Office 365-Abonnement in Azure](active-directory-manage-o365-subscription.md)

---

**F: Welche Beziehung besteht zwischen Azure AD, Office 365 und Azure?**

**A:** Mit Azure Active Directory verfügen Sie über allgemeine Identitäts- und Zugriffsfunktionen für alle Onlinedienste von Microsoft. Wenn Sie Office 365, Microsoft Azure, Intune oder andere Anwendungen nutzen, verwenden Sie bereits eine Azure AD-Instanz, um die Anmeldung und Zugriffsverwaltung für diese Dienste zu ermöglichen.

Alle Benutzer, die Sie für Microsoft Online Services aktiviert haben, werden als Benutzerkonten in einer oder mehreren Azure AD-Instanzen definiert. Sie können kostenlose Azure AD-Funktionen für diese Konten aktivieren, z.B. den Zugriff auf Cloudanwendungen.
 
Außerdem stellen kostenpflichtige Azure AD-Dienste (z.B. Azure AD Basic, Premium, EMS usw.) eine Erweiterung anderer Onlinedienste, wie Office 365 und Microsoft Azure, durch umfassende Verwaltungs- und Sicherheitslösungen für Unternehmen dar.


---



## Erste Schritte mit Azure AD als Hybridanwendung


**F: Wie kann ich eine Verbindung meines lokalen Verzeichnisses mit Azure AD verknüpfen?**

**A:** Die Verbindung zwischen Ihrem lokalen Verzeichnis und Azure AD kann mithilfe von **Azure AD Connect** hergestellt werden.

Ausführlichere Informationen finden Sie unter [Integrieren Ihrer lokalen Identitäten in Azure Active Directory](active-directory-aadconnect.md).


---

**F: Wie richte ich SSO zwischen meinem lokalen Verzeichnis und meinen Cloudanwendungen ein?**

**A:** SSO muss lediglich zwischen Ihrem lokalen Verzeichnis und Azure AD eingerichtet werden. Solange Sie auf Ihre Cloudanwendungen über Azure AD zugreifen, wird vom Dienst für die Benutzer automatisch die richtige Authentifizierung mit ihren lokalen Anmeldeinformationen verwendet.

Das Implementieren des einmaligen Anmeldens (SSO) aus einer lokalen Umgebung lässt sich mit Verbundlösungen wie AD FS oder durch die Konfiguration der Kennworthashsynchronisierung leicht erreichen. Sie können mit dem Assistenten zum Konfigurieren von Azure AD Connect problemlos beide Optionen bereitstellen.
  

Ausführlichere Informationen finden Sie unter [Integrieren Ihrer lokalen Identitäten in Azure Active Directory](active-directory-aadconnect.md).
  

---

**F: Wird unter Azure Active Directory ein Self-Service-Portal für Benutzer in meiner Organisation bereitgestellt?**

**A:** Ja. Für Self-Service-Aktionen und den Anwendungszugriff der Benutzer stellt Azure Active Directory den [Azure AD-Zugriffsbereich](http://myapps.microsoft.com) bereit. Wenn Sie Office 365-Kunde sind, stehen Ihnen im Office 365-Portal viele dieser Funktionen bereits zur Verfügung.

Weitere Informationen finden Sie unter [Einführung in den Zugriffsbereich](active-directory-saas-access-panel-introduction.md).



---

**F: Unterstützt mich Azure AD bei der Verwaltung meiner lokalen Infrastruktur?**

**A:** Ja. In der Premium Edition von Azure AD ist **Connect Health** enthalten. Azure AD Connect Health unterstützt Sie bei der Überwachung Ihrer lokalen Identitätsinfrastruktur und Synchronisierungsdienste und vermittelt Ihnen wichtige Einblicke.

Ausführlichere Informationen finden Sie unter [Überwachen Ihrer lokalen Identitätsinfrastruktur und Synchronisierung von Diensten in der Cloud](active-directory-aadconnect-health.md).

---

## Kennwortverwaltung

**F: Kann ich das Kennwortrückschreiben-Feature von Azure AD ohne Kennwortsynchronisierung verwenden? (Ich möchte die Kennwortzurücksetzung mit dem Kennwortrückschreiben-Feature von Azure AD verwenden, ohne dass meine Kennwörter in der Cloud gespeichert werden.)**

**A:** Sie müssen Ihre AD-Kennwörter nicht mit Azure AD synchronisieren, um das Kennwortrückschreiben-Feature verwenden zu können. Bei Verwendung einer Verbundumgebung überlässt Azure AD-SSO die Benutzerauthentifizierung dem lokalen Verzeichnis. In diesem Szenario muss das lokale Kennwort nicht in Azure AD nachverfolgt werden.

---

**F: Wie lange dauert das Zurückschreiben eines Kennworts in das lokale AD?**

**A:** Kennwörter werden in Echtzeit zurückgeschrieben.

Ausführlichere Informationen finden Sie unter [Erste Schritte mit der Kennwortverwaltung](active-directory-passwords-getting-started.md).


---

**F: Kann ich das Kennwortrückschreiben-Feature für Kennwörter verwenden, die von einem Administrator verwaltet werden?**

**A:** Ja. Bei aktiviertem Kennwortrückschreiben werden die von einem Administrator durchgeführten Kennwortvorgänge in Ihre lokale Umgebung zurückgeschrieben.

Weitere Antworten auf kennwortbezogene Fragen finden Sie unter [Häufig gestellte Fragen zur Kennwortverwaltung](active-directory-passwords-faq.md).

---

## Anwendungszugriff


**F: Wo finde ich eine Liste mit bereits in Azure AD integrierten Anwendungen und deren Funktionen?**

**A:** Für Azure AD stehen bereits über 2.600 integrierte Anwendungen von Microsoft, Anwendungsdienstanbietern und Partnern zur Verfügung. Alle vorab integrierten Anwendungen unterstützen SSO. SSO ermöglicht Ihnen die Verwendung der Anmeldeinformationen Ihres Unternehmens zum Zugreifen auf Apps. Einige Anwendungen unterstützen auch die automatisierte Bereitstellung und Aufhebung der Bereitstellung.

Eine vollständige Liste mit integrierten Anwendungen finden Sie unter [Active Directory-Marketplace](https://azure.microsoft.com/marketplace/active-directory/).


---

**F: Was kann ich tun, wenn die Anwendung, die ich benötige, nicht im Azure AD-Marketplace enthalten ist?**

**A:** Mit Azure AD Premium können Sie beliebige Anwendungen hinzufügen und konfigurieren. Abhängig von den Funktionen Ihrer Anwendung und Ihren Präferenzen können Sie SSO und die automatische Bereitstellung konfigurieren.

Weitere Informationen finden Sie unter:

- [Konfigurieren des einmaligen Anmeldens für Anwendungen, die nicht im Azure Active Directory-Anwendungskatalog enthalten sind](active-directory-saas-custom-apps.md)
- [Verwenden von SCIM für die automatische Bereitstellung von Benutzern und Gruppen aus Azure Active Directory für Anwendungen](active-directory-scim-provisioning.md)


---

**F: Wie melden sich Benutzer mit Azure Active Directory an Anwendungen an?**
 
**A:** In Azure Active Directory können Benutzer ihre Anwendungen auf unterschiedliche Weise anzeigen und auf sie zugreifen. Im Anschluss finden Sie einige Beispiele für verfügbare Möglichkeiten:

- Azure AD-Zugriffsbereich

- Office 365-Anwendungsstartprogramm

- Direkte Anmeldung bei Verbund-Apps

- Deep-Links zu verbundenen, kennwortbasierten oder vorhandene Apps

Weitere Informationen finden Sie unter [Bereitstellen von in Azure AD integrierten Anwendungen für Benutzer](active-directory-appssoaccess-whatis.md#deploying-azure-ad-integrated-applications-to-users).


---

**F: Auf welche Weise ermöglicht Azure Active Directory die Authentifizierung und das einmalige Anmelden für Anwendungen?**
 
**A:** Azure Active Directory unterstützt viele standardisierte Protokolle für die Authentifizierung und Autorisierung wie SAML 2.0, OpenID Connect, OAuth 2.0 und WS-Federation. Außerdem unterstützt Azure AD Kennworttresore und Funktionen für die automatisierte Anmeldung für Apps, die nur die formularbasierte Authentifizierung unterstützen.

Weitere Informationen finden Sie unter:

- [Authentifizierungsszenarien für Azure AD](active-directory-authentication-scenarios.md)

- [Active Directory-Authentifizierungsprotokolle](https://msdn.microsoft.com/library/azure/dn151124.aspx)

- [Wie funktioniert das einmalige Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md#how-does-single-sign-on-with-azure-active-directory-work)


---

**F: Kann ich lokal ausgeführte Anwendungen hinzufügen?**

**A:** Mit dem Azure AD-Anwendungsproxy haben Sie einfachen und sicheren Zugriff auf lokale Webanwendungen Ihrer Wahl. Sie können auf diese Anwendungen genauso zugreifen, wie Sie in Azure Active Directory auf Ihre SaaS-Apps zugreifen. Ein VPN bzw. eine Änderung der Netzwerkinfrastruktur ist nicht erforderlich.

Ausführlichere Informationen finden Sie unter [Bereitstellen von sicherem Remotezugriff auf lokale Anwendungen](active-directory-application-proxy-get-started.md).


--- 

**F: Wie kann ich für Benutzer, die auf eine bestimmte Anwendung zugreifen, die Verwendung von MFA erforderlich machen?**

**A:** Mit dem bedingten Zugriff von Azure AD können Sie jeder Anwendung eine individuelle Zugriffsrichtlinie zuweisen. In dieser Richtlinie können Sie festlegen, dass MFA immer erforderlich ist oder nur dann, wenn Benutzer gerade nicht mit dem lokalen Netzwerk verbunden sind.

Ausführlichere Informationen finden Sie unter [Bedingter Zugriff mit Azure Active Directory](active-directory-conditional-access.md).


---

**F: Was ist eine automatisierte Benutzerbereitstellung für SaaS-Apps?**

**A:** Azure Active Directory ermöglicht die Automatisierung der Erstellung, Wartung und Entfernung von Benutzeridentitäten in vielen gängigen Cloudanwendungen (SaaS).

Weitere Informationen finden Sie unter [Automatisieren der Bereitstellung und Bereitstellungsaufhebung von Benutzern für SaaS-Anwendungen mit Azure Active Directory](active-directory-saas-app-provisioning.md).

---

<!---HONumber=AcomDC_0817_2016-->