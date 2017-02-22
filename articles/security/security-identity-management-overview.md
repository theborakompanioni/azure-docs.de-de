---
title: "Azure-Sicherheitsfeatures, die die Identitätsverwaltung unterstützen | Microsoft-Dokumentation"
description: " Dieser Artikel bietet eine Übersicht über die wichtigsten Azure-Sicherheitsfeatures, die die Identitätsverwaltung unterstützen. Lösungen zur Identitäts- und Zugriffsverwaltung von Microsoft unterstützen IT-Profis dabei, den Zugriff auf Anwendungen und Ressourcen über das Unternehmensdatencenter und in der Cloud zu schützen, wobei zusätzliche Ebenen der Überprüfungen aktiviert werden, z.B. mehrstufige Authentifizierung und Richtlinien für bedingten Zugriff.. "
services: security
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: TomSh
ms.assetid: 5aa0a7ac-8f18-4ede-92a1-ae0dfe585e28
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/23/2017
ms.author: terrylan
translationtype: Human Translation
ms.sourcegitcommit: af191f7eed529c7d4541b28eb62979f25da89892
ms.openlocfilehash: 9c5af888ec0130ec8506602ab805221f0a83ea47


---
# <a name="azure-identity-management-security-overview"></a>Übersicht über die Sicherheit der Azure-Identitätsverwaltung
Lösungen zur Identitäts- und Zugriffsverwaltung von Microsoft unterstützen IT-Profis dabei, den Zugriff auf Anwendungen und Ressourcen über das Unternehmensrechenzentrum und in der Cloud zu schützen, wobei zusätzliche Ebenen der Überprüfungen aktiviert werden, z.B. mehrstufige Authentifizierung und Richtlinien für bedingten Zugriff. Die Überwachung verdächtiger Aktivitäten über erweiterte Sicherheitsberichtserstellung, Überwachung und Warnung trägt dazu bei, potenzielle Sicherheitsprobleme zu verringern. [Azure Active Directory Premium](../active-directory/active-directory-editions.md) ermöglicht einmaliges Anmelden bei Tausenden von Cloudanwendungen (SaaS) und Zugriff auf Webanwendungen, die Sie lokal ausführen.

Zu den Sicherheitsvorteilen von Azure Active Directory (AD) zählen folgende Möglichkeiten:

* Erstellen und Verwalten einer einzelnen Identität für jeden Benutzer in Ihrer gesamten hybriden Unternehmensumgebung, wobei Benutzer, Gruppen und Geräte synchronisiert werden
* Bereitstellen des Zugriffs auf Ihre Anwendungen mit einmaligem Anmelden, inklusive Tausender bereits integrierter SaaS-Apps
* Aktivieren der Sicherheit für den Anwendungszugriff durch Erzwingen der regelbasierten Multi-Factor Authentication für lokale und Cloudanwendungen
* Bereitstellen des sicheren Remotezugriffs auf lokale Webanwendungen mit dem Azure AD-Anwendungsproxy

Das Ziel dieses Artikels ist, eine Übersicht über die wichtigsten Azure-Sicherheitsfunktionen zu bieten, die die Identitätsverwaltung unterstützen. Wir bieten auch Links zu Artikeln mit weiteren Informationen zu jedem Feature.  

Der Artikel konzentriert sich auf die folgenden wesentlichen Funktionen der Azure-Identitätsverwaltung:

* Einmaliges Anmelden
* Reverseproxy
* Multi-Factor Authentication
* Sicherheitsüberwachung, Warnungen und Machine Learning-basierte Berichte
* Kundenidentitäts- und Kundenzugriffsverwaltung
* Geräteregistrierung
* Privileged Identity Management
* Schutz der Identität (Identity Protection)
* Hybrididentitätsverwaltung

## <a name="single-sign-on"></a>Einmaliges Anmelden
Einmaliges Anmelden (Single sign-on; SSO) bedeutet, dass Sie Zugriff auf sämtliche für Ihre Geschäftsaktivitäten benötigten Anwendungen und Ressourcen erhalten, indem Sie sich nur einmal mit einem einzigen Benutzerkonto anmelden. Nach der Anmeldung können Sie auf alle benötigten Anwendungen zugreifen, ohne sich ein zweites Mal (z.B. durch Eingabe eines Kennworts) authentifizieren zu müssen.

Viele Organisationen nutzen SaaS-Anwendungen (Software as a Service) wie z. B. Office 365, Box und Salesforce, um die Endbenutzerproduktivität zu steigern. In der Vergangenheit musste das IT-Personal Benutzerkonten in jeder SaaS-Anwendung individuell erstellen und aktualisieren, und Benutzer mussten sich für jede SaaS-Anwendung ein Kennwort merken.

Azure AD weitet das lokale Active Directory auf die Cloud aus und ermöglicht den Benutzern auf diese Weise, sich mit ihrem primären Organisationskonto nicht nur bei den mit ihrer Domäne verknüpften Geräten und Unternehmensressourcen anzumelden, sondern auch bei sämtlichen Web- und SaaS-Anwendungen, die sie für ihre Arbeit benötigen.

Benutzer müssen sich keine Vielzahl von Benutzernamen und Kennwörtern mehr merken, und der Anwendungszugriff für Benutzer kann basierend auf Organisationsgruppen sowie ihrem Mitarbeiterstatus automatisch bereitgestellt oder deaktiviert werden. Azure AD stellt Funktionen zum Steuern von Sicherheit und Zugriff bereit, mit denen Sie den Benutzerzugriff auf SaaS-Anwendungen zentral verwalten können.

Weitere Informationen:

* [Übersicht über „Einmaliges Anmelden“](https://azure.microsoft.com/documentation/videos/overview-of-single-sign-on/)
* [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](../active-directory/active-directory-appssoaccess-whatis.md)
* [Integrieren des einmaligen Anmeldens mit Azure Active Directory in SaaS-Apps](../active-directory/active-directory-sso-integrate-saas-apps.md)

## <a name="reverse-proxy"></a>Reverseproxy
Der Azure AD-Anwendungsproxy ermöglicht Ihnen das Veröffentlichen von lokalen Anwendungen, z.B. [SharePoint](https://support.office.com/article/What-is-SharePoint-97b915e6-651b-43b2-827d-fb25777f446f?ui=en-US&rs=en-US&ad=US)-Websites, [Outlook Web App](https://technet.microsoft.com/library/jj657718.aspx) und [IIS](http://www.iis.net/)-basierte Apps innerhalb Ihres privaten Netzwerks und bietet sicheren Zugriff für Benutzer außerhalb Ihres Netzwerks. Der Anwendungsproxy bietet Remotezugriff und einmaliges Anmelden für eine Vielzahl von lokalen Webanwendungen sowie Tausende von SaaS-Anwendungen, die Azure AD unterstützt werden. Mitarbeiter können sich auf ihren eigenen Geräten von zu Hause aus bei Ihren Apps anmelden und sich über diesen cloudbasierten Proxy authentifizieren.

Weitere Informationen:

* [Aktivieren des Azure AD-Anwendungsproxys](../active-directory/active-directory-application-proxy-enable.md)
* [Veröffentlichen von Anwendungen mit Azure AD-Anwendungsproxy](../active-directory/active-directory-application-proxy-publish.md)
* [Einmaliges Anmelden mit Anwendungsproxy](../active-directory/active-directory-application-proxy-sso-using-kcd.md)
* [Arbeiten mit bedingtem Zugriff](../active-directory/active-directory-application-proxy-conditional-access.md)

## <a name="multi-factor-authentication"></a>Multi-Factor Authentication
Azure Multi-Factor Authentication (MFA) ist eine Authentifizierungsmethode, für die die Verwendung von mehr als einer Verifizierungsmethode erforderlich ist und die eine wichtige zweite Sicherheitsebene für Benutzeranmeldungen und Transaktionen darstellt. MFA hilft beim Schützen des Zugriffs auf Daten und Anwendungen und erfüllt gleichzeitig die Anforderungen von Benutzern an ein einfaches Anmeldeverfahren. Sie bietet eine leistungsfähige Authentifizierung mit verschiedenen Überprüfungsoptionen – Telefonanruf, SMS oder per Benachrichtigung bzw. Überprüfungscode in einer mobilen App sowie OAuth-Token von Drittanbietern.

Weitere Informationen:

* [Multi-Factor Authentication](https://azure.microsoft.com/documentation/services/multi-factor-authentication/)
* [Was ist Azure Multi-Factor Authentication?](../multi-factor-authentication/multi-factor-authentication.md)
* [Funktionsweise von Azure Multi-Factor Authentication](../multi-factor-authentication/multi-factor-authentication-how-it-works.md)

## <a name="security-monitoring-alerts-and-machine-learning-based-reports"></a>Sicherheitsüberwachung, Warnungen und Machine Learning-basierte Berichte
Sicherheitsüberwachung und Warnungen sowie Berichte auf Machine Learning-Basis, die inkonsistente Zugriffsmuster erkennen und Ihnen helfen können, Ihr Unternehmen zu schützen. Sie können die Zugriffs- und Nutzungsberichte von Azure Active Directory verwenden, um sich einen Einblick in die Integrität und Sicherheit des Verzeichnisses Ihrer Organisation zu verschaffen. Mithilfe dieser Informationen kann ein Verzeichnisadministrator mögliche Sicherheitsrisiken besser bestimmen, um angemessen zu planen, wie diese Risiken eingedämmt werden können.

Im klassischen Azure-Portal werden Berichte auf folgende Weise kategorisiert:

* Anomalieberichte: Enthalten Anmeldeereignisse, die wir als anomal eingestuft haben. Unser Ziel ist, Sie auf solche Aktivitäten aufmerksam zu machen und Ihnen zu ermöglichen, eine Entscheidung zu treffen, ob ein Ereignis verdächtig ist.
* Integrierte Anwendungsberichte – Bieten Einblicke, wie Cloudanwendungen in Ihrer Organisation verwendet werden. Azure Active Directory ermöglicht die Integration in Tausende von Cloudanwendungen.
* Fehlerberichte: Enthalten Hinweise auf Fehler, die bei der Bereitstellung von Konten für externe Anwendungen auftreten können.
* Benutzerspezifische Berichte: Enthalten Geräte-/Anmeldeaktivitätsdaten für einen bestimmten Benutzer.
* Aktivitätsprotokolle – Enthalten eine Aufzeichnung aller überwachten Ereignisse in den letzten 24 Stunden, 7 Tagen oder 30 Tagen sowie geänderte Gruppenaktivitäten und Kennwortzurücksetzungs- und Registrierungsaktivitäten

Weitere Informationen:

* [Anzeigen Ihrer Zugriffs- und Nutzungsberichte](../active-directory/active-directory-view-access-usage-reports.md)
* [Erste Schritte mit Azure Active Directory-Berichten](../active-directory/active-directory-reporting-getting-started.md)
* [Anleitung für Azure Active Directory-Berichte](../active-directory/active-directory-reporting-guide.md)

## <a name="consumer-identity-and-access-management"></a>Kundenidentitäts- und Kundenzugriffsverwaltung
Azure Active Directory B2C ist ein hoch verfügbarer, globaler Identitätsverwaltungsdienst für kundenorientierte Anwendungen, der für Hunderte Millionen von Identitäten skaliert werden kann. Er kann über mobile und Webplattformen integriert werden. Ihre Kunden können sich über eine anpassbare Oberfläche bei all Ihren Anwendungen anmelden und zu diesem Zweck entweder vorhandene Konten aus sozialen Netzwerken nutzen oder neue Anmeldeinformationen festlegen.

In der Vergangenheit mussten Anwendungsentwickler ihren eigenen Code schreiben, wenn Sie Verbraucher in ihren Anwendungen registrieren und anmelden wollten. Und sie hätten lokale Datenbanken oder Systeme zum Speichern von Benutzernamen und Kennwörtern verwendet. Azure Active Directory B2C bietet Ihrer Organisation eine bessere Lösung, um die Verwaltung von Kundenidentitäten in ihre Anwendungen zu integrieren. Dabei kommen eine sichere, standardbasierte Plattform und ein umfassender Satz von erweiterbaren Richtlinien zum Einsatz.

Wenn Sie Azure Active Directory B2C verwenden, können sich Ihre Kunden mit vorhandenen Konten in sozialen Netzwerken (Facebook, Google, Amazon, LinkedIn) oder durch Erstellen neuer Anmeldeinformationen (E-Mail-Adresse und Kennwort oder Benutzername und Kennwort) bei Ihren Anwendungen registrieren.

Weitere Informationen:

* [Was ist Azure Active Directory B2C?](https://azure.microsoft.com/services/active-directory-b2c/)
* [Azure Active Directory B2C-Vorschau: Registrieren und Anmelden von Kunden in den Anwendungen](../active-directory-b2c/active-directory-b2c-overview.md)
* [Azure Active Directory B2C-Vorschau: Anwendungstypen](../active-directory-b2c/active-directory-b2c-apps.md)

## <a name="device-registration"></a>Geräteregistrierung
Die Azure AD-Geräteregistrierung ist die Grundlage gerätebasierter Szenarien für den [bedingten Zugriff](../active-directory/active-directory-conditional-access-on-premises-setup.md) . Wenn ein Gerät registriert wird, stellt die Azure Active Directory-Geräteregistrierung eine Identität für das Gerät bereit, die bei der Benutzeranmeldung zum Authentifizieren des Geräts dient. Das authentifizierte Gerät und die Attribute des Geräts können anschließend verwendet werden, um bedingte Zugriffsrichtlinien für Anwendungen zu erzwingen, die in der Cloud und lokal gehostet werden.

In Kombination mit einer Lösung für die Verwaltung mobiler Geräte, wie z.B. Intune, werden die Geräteattribute in Azure Active Directory mit zusätzlichen Informationen über das Gerät aktualisiert. So können Sie Regeln für den bedingten Zugriff erstellen, die erzwingen, dass der Zugriff von Geräten Ihren Standards für Sicherheit und Kompatibilität entspricht.

Weitere Informationen:

* [Erste Schritte bei der Azure Active Directory-Geräteregistrierung](../active-directory/active-directory-conditional-access-device-registration-overview.md)
* [Einrichten des lokalen bedingten Zugriffs mithilfe der Azure Active Directory-Geräteregistrierung](../active-directory/active-directory-conditional-access-on-premises-setup.md)
* [Automatische Geräteregistrierung bei Azure Active Directory für in Domänen eingebundene Windows-Geräte](../active-directory/active-directory-conditional-access-automatic-device-registration.md)

## <a name="privileged-identity-management"></a>Privileged Identity Management
Mithilfe von Azure Active Directory (AD) Privileged Identity Management (PIM) können Sie Ihre privilegierten Identitäten und deren Zugriff auf Ressourcen in Azure AD und anderen Microsoft Online Services wie Office 365 oder Microsoft Intune verwalten, steuern und überwachen.

Manchmal müssen Benutzer privilegierte Vorgänge in Azure- oder Office 365-Ressourcen oder anderen SaaS-Apps ausführen. Dies bedeutet häufig, dass Organisationen diesen Benutzern in Azure AD dauerhaften privilegierten Zugriff gewähren müssen. Dies stellt ein wachsendes Sicherheitsrisiko für die in der Cloud gehosteten Ressourcen dar, da Organisationen die Aktionen, die diese Benutzer mit ihren Administratorberechtigungen ausführen, nicht ausreichend überwachen können. Darüber hinaus kann die Sicherheit der gesamten Cloud in Gefahr sein, wenn ein Benutzerkonto mit privilegiertem Zugriff kompromittiert wird. Mit Azure AD Privileged Identity Management können Sie dieses Risiko in den Griff bekommen.

Azure AD Privileged Identity Management ermöglicht Ihnen Folgendes:

* Ermitteln, welche Benutzer Azure AD-Administratoren sind
* Aktivieren von bedarfsgesteuertem Just-In-Time-Administratorzugriff auf Microsoft Online Services wie z. B. Office 365 und Intune
* Abrufen von Berichten zum Administratorzugriffsverlauf und zu Änderungen bei Administratorzuweisungen
* Aktivieren von Benachrichtigungen zum Zugriff auf eine privilegierte Rolle

Weitere Informationen:

* [Azure AD Privileged Identity Management](../active-directory/active-directory-privileged-identity-management-configure.md)
* [Rollen in Azure AD Privileged Identity Management](../active-directory/active-directory-privileged-identity-management-roles.md)
* [Azure AD Privileged Identity Management: Hinzufügen oder Entfernen einer Benutzerrolle](../active-directory/active-directory-privileged-identity-management-how-to-add-role-to-user.md)

## <a name="identity-protection"></a>Schutz der Identität (Identity Protection)
Azure AD Identity Protection ist ein Sicherheitsdienst, der eine umfassende Übersicht über Risikoereignisse und potenzielle Sicherheitsrisiken bietet, die für die Identitäten Ihrer Organisation bestehen. Für Identity Protection werden die vorhandenen Azure Active Directory-Funktionen zur Erkennung von Anomalien genutzt (über die Berichte zu anomalen Aktivitäten von Azure AD), und es werden neue Risikoereignistypen eingeführt, mit denen Anomalien in Echtzeit erkannt werden können.

Weitere Informationen:

* [Azure Active Directory Identity Protection](../active-directory/active-directory-identityprotection.md)
* [Channel 9: Azure AD and Identity Show: Identity Protection Preview (Channel 9: Azure AD- und Identity-Show: Vorschau von Identity Protection)](https://channel9.msdn.com/Series/Azure-AD-Identity/Azure-AD-and-Identity-Show-Identity-Protection-Preview)

## <a name="hybrid-identity-management"></a>Hybrididentitätsverwaltung
Der Identitätsansatz von Microsoft deckt sowohl den lokalen Bereich als auch die Cloud ab, und es wird eine einzelne Benutzeridentität für die standortunabhängige Authentifizierung und Autorisierung gegenüber allen Ressourcen erstellt.

Weitere Informationen:

* [Whitepaper zur Hybrididentität](http://download.microsoft.com/download/D/B/A/DBA9E313-B833-48EE-998A-240AA799A8AB/Hybrid_Identity_White_Paper.pdf)
* [Azure Active Directory](https://azure.microsoft.com/documentation/services/active-directory/)
* [Active Directory Team Blog](https://blogs.technet.microsoft.com/ad/)



<!--HONumber=Jan17_HO4-->


