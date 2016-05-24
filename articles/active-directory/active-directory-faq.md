<properties
	pageTitle="Häufig gestellte Fragen zu Azure Active Directory | Microsoft Azure"
	description="Häufig gestellte Fragen zu Azure Active Directory"
	services="active-directory"
	documentationCenter=""
	authors="markusvi"
	manager="stevenpo"
	editor=""/>

<tags
	ms.service="active-directory"
	ms.workload="identity"
	ms.tgt_pltfrm="na"
	ms.devlang="na"
	ms.topic="get-started-article"
	ms.date="04/20/2016"
	ms.author="markusvi"/>

# Häufig gestellte Fragen zu Azure Active Directory

Azure Active Directory (Azure AD) ist eine umfassende cloudbasierte IDaaS-Lösung (Identity as a Service) mit Identitäts- und Zugriffsverwaltungsfunktionen. Mit Azure AD als IDaaS-Anbieter können Sie in Ihrem Computernetzwerk steuern, *wer wozu berechtigt ist*. Darüber hinaus bietet Azure AD einmaliges Anmelden (SSO) für Ihre Anwendungen. Mit SSO können Sie Anmeldeprozesse für Benutzer erheblich verbessern. Weitere Informationen finden Sie unter [Was ist Azure Active Directory?](active-directory-whatis.md).



## Zugreifen auf Azure und Azure Active Directory

****F: Warum finde ich Azure AD nicht im Azure-Portal (http://portal.azure.com)?**

**A:** Die Verwaltungsumgebung für Azure AD ist weiterhin im klassischen Azure-Portal (`http://manage.windowsazure.com`) verfügbar. Eine öffentliche Vorschau der neuen, in das Azure-Portal integrierten Verwaltungsumgebung wird voraussichtlich im September veröffentlicht. Neueste Informationen zu Veröffentlichungen finden Sie im [Active Directory-Teamblog](https://blogs.technet.microsoft.com/ad/).


---

****F: Warum wird „Keine Abonnements gefunden“ angezeigt, wenn ich versuche, über das klassische Azure-Portal auf Azure AD zuzugreifen (http://manage.windowsazure.com)?**

**A:** Sie benötigen Berechtigungen für ein Azure-Abonnement. Wenn Sie über eine kostenpflichtige Version von Office 365 oder Azure AD verfügen, können Sie unter [http://aka.ms/accessAAD](http://aka.ms/accessAAD) einen einmaligen Aktivierungsschritt durchführen. Andernfalls müssen Sie eine vollwertige [Azure-Testversion](https://azure.microsoft.com/pricing/free-trial/) oder ein kostenpflichtiges Abonnement aktivieren.

---

**F: Welche Beziehung besteht zwischen Azure AD, Office 365 und Azure?**

**A:** Microsoft Azure ist eine offene, flexible, erstklassige Cloud Computing-Plattform und -Infrastruktur zum Erstellen, Bereitstellen und Verwalten von Anwendungen und Diensten über ein globales Netzwerk aus von Microsoft verwalteten Datencentern. Azure bietet unter anderem SaaS-Lösungen (Software as a Service) wie etwa Office 365. Bei Office 365 handelt es sich um eine Gruppe von Software plus Service-Abonnements, über die Ihnen Produktivitätssoftware und entsprechende Onlinedienste zur Verfügung gestellt werden. Azure AD ergänzt Office 365 mit einer umfassenden cloudbasierten IDaaS-Lösung (Identity as a Service) mit Identitäts- und Zugriffsverwaltungsfunktionen. Wenn Sie über ein Office 365-Abonnement verfügen, haben Sie auch eine Azure AD-Instanz unter Azure.


---

**F: Welche Azure AD-Features stehen kostenlos zur Verfügung?**

**A:** Alle allgemeinen Features eines Azure-Abonnements stehen kostenlos zur Verfügung. Eine vollständige Featureliste finden Sie unter [Allgemeine Features](active-directory-editions.md/#common-features).



---

**F: Wie kann ich mein lokales Verzeichnis mit Azure AD verknüpfen?**

A: Die Verbindung zwischen Ihrem lokalen Verzeichnis und Azure AD kann mithilfe von **Azure AD Connect** hergestellt werden. Ausführlichere Informationen finden Sie unter [Integrieren Ihrer lokalen Identitäten in Azure Active Directory](active-directory-aadconnect.md).


---

**F: Wie richte ich SSO zwischen meinem lokalen Verzeichnis und meinen Cloudanwendungen ein?**

**A:** SSO muss lediglich zwischen Ihrem lokalen Verzeichnis und Azure AD eingerichtet werden. Solange Ihre Cloudanwendungen von Azure AD verwaltet werden, können Sie die SSO-Reichweite auf Ihre lokale Umgebung ausdehnen, indem Sie einen Verbund mit AD FS oder eine Kennwortsynchronisierung implementieren. Beide Optionen stehen in **Azure AD Connect** zur Verfügung. Ausführlichere Informationen finden Sie unter [Integrieren Ihrer lokalen Identitäten in Azure Active Directory](active-directory-aadconnect.md).
  


---

**F: Unterstützt mich Azure AD bei der Verwaltung meiner lokalen Infrastruktur?**

**A:** Ja. In der Premium Edition von Azure AD ist **Connect Health** enthalten. Azure AD Connect Health unterstützt Sie bei der Überwachung Ihrer lokalen Identitätsinfrastruktur und Synchronisierungsdienste und vermittelt Ihnen wichtige Einblicke. Ausführlichere Informationen finden Sie unter [Überwachen Ihrer lokalen Identitätsinfrastruktur und Synchronisierung von Diensten in der Cloud](active-directory-aadconnect-health.md).



## Kennwortverwaltung

**F: Kann ich das Kennwortrückschreiben-Feature von Azure AD ohne Kennwortsynchronisierung verwenden? (Ich möchte die Kennwortzurücksetzung mit dem Kennwortrückschreiben-Feature von Azure AD verwenden, ohne dass meine Kennwörter in der Cloud gespeichert werden.)**

**A:** Sie müssen Ihre AD-Kennwörter nicht mit Azure AD synchronisieren, um das Kennwortrückschreiben-Feature verwenden zu können. Bei Verwendung einer Verbundumgebung überlässt Azure AD-SSO die Benutzerauthentifizierung dem lokalen Verzeichnis. In diesem Szenario muss das lokale Kennwort nicht in Azure AD nachverfolgt werden.

---

**F: Wie lange dauert das Zurückschreiben eines Kennworts in das lokale AD?**

**A:** Kennwörter werden in Echtzeit zurückgeschrieben. Ausführlichere Informationen finden Sie unter [Erste Schritte mit der Kennwortverwaltung](active-directory-passwords-getting-started.md).


---

**F: Kann ich das Kennwortrückschreiben-Feature für Kennwörter verwenden, die von einem Administrator verwaltet werden?**

**A:** Ja. Bei aktiviertem Kennwortrückschreiben werden die von einem Administrator durchgeführten Kennwortvorgänge in Ihre lokale Umgebung zurückgeschrieben. Weitere Antworten auf kennwortbezogene Fragen finden Sie unter [Häufig gestellte Fragen zur Kennwortverwaltung](active-directory-passwords-faq.md).



## Anwendungszugriff


**F: Wo finde ich eine Liste mit bereits in Azure AD integrierten Anwendungen und deren Funktionen?**

**A:** Für Azure AD stehen bereits über 2600 integrierte Anwendungen von Microsoft, Anwendungsdienstanbietern und Partnern zur Verfügung. Eine vollständige Liste mit integrierten Anwendungen finden Sie unter [Active Directory-Marketplace](https://azure.microsoft.com/marketplace/active-directory/).


---

**F: Was kann ich tun, wenn die Anwendung, die ich benötige, nicht im Azure AD-Marketplace enthalten ist?**

**A:** Mit Azure AD Premium können Sie beliebige Anwendungen hinzufügen und konfigurieren. Darüber hinaus können Sie (abhängig von den Funktionen Ihrer Anwendung und Ihren Präferenzen) SSO und die automatische Bereitstellung konfigurieren. Weitere Informationen finden Sie unter:

- [Konfigurieren des einmaligen Anmeldens für Anwendungen, die nicht im Azure Active Directory-Anwendungskatalog enthalten sind](active-directory-saas-custom-apps.md)
- [Verwenden von SCIM für die automatische Bereitstellung von Benutzern und Gruppen aus Azure Active Directory für Anwendungen](active-directory-scim-provisioning.md) 



---

**F: Kann ich lokal ausgeführte Anwendungen hinzufügen?**

**A:** Mit dem Anwendungsproxy von Azure AD können Sie auf Ihre lokalen Webanwendungen auf die gleiche Weise zugreifen wie auf Ihre SaaS-Apps in Azure Active Directory – ganz ohne VPN oder Anpassung der Netzwerkinfrastruktur. Ausführlichere Informationen finden Sie unter [Bereitstellen von sicherem Remotezugriff auf lokale Anwendungen](active-directory-application-proxy-get-started.md).


--- 

**F: Wie kann ich für Benutzer, die auf eine bestimmte Anwendung zugreifen, die Verwendung von MFA erforderlich machen?**

**A:** Mit dem bedingten Zugriff von Azure AD können Sie jeder Anwendung eine individuelle Zugriffsrichtlinie zuweisen. In dieser Richtlinie können Sie festlegen, dass MFA immer erforderlich ist oder nur dann, wenn Benutzer gerade nicht mit dem lokalen Netzwerk verbunden sind. Ausführlichere Informationen finden Sie unter [Sichern des Zugriffs auf Office 365 und andere mit Azure Active Directory verbundene Apps](active-directory-conditional-access.md).

<!---HONumber=AcomDC_0518_2016-->