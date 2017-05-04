---
title: Was ist die Azure Active Directory B2B-Zusammenarbeit? | Microsoft Docs
description: "Die Azure Active Directory B2B-Zusammenarbeit ermöglicht Geschäftspartnern den gezielten Zugriff auf Ihre Unternehmensanwendungen und unterstützt so Ihre unternehmensübergreifenden Beziehungen."
services: active-directory
documentationcenter: 
author: sasubram
manager: femila
editor: 
tags: 
ms.assetid: 1464387b-ee8b-4c7c-94b3-2c5567224c27
ms.service: active-directory
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: identity
ms.date: 04/12/2017
ms.author: sasubram
translationtype: Human Translation
ms.sourcegitcommit: 7f469fb309f92b86dbf289d3a0462ba9042af48a
ms.openlocfilehash: 9cd8edf47059a5f574489a604156890d157c8b1a
ms.lasthandoff: 04/13/2017


---

# <a name="what-is-azure-ad-b2b-collaboration"></a>Was ist die Azure AD B2B-Zusammenarbeit?

<iframe width="560" height="315" src="https://www.youtube.com/embed/AhwrweCBdsc" frameborder="0" allowfullscreen></iframe>

Mit den Funktionen der Azure AD B2B-Zusammenarbeit (Business-to-Business) kann jede Organisation, die Azure AD verwendet, auf sichere Weise mit Benutzern aus jeder anderen Organisation zusammenarbeiten. Diese andere Organisation kann groß oder klein sein, sie muss nicht Azure AD verwenden, und sie muss nicht über eine IT-Organisation verfügen. 

Organisationen, die Azure AD verwenden, können für ihre Partner Zugriff auf Dokumente, Ressourcen und Anwendungen bereitstellen und dabei die vollständige Kontrolle über die Unternehmensdaten behalten. Entwickler können mit den Azure AD B2B-APIs Anwendungen schreiben, die zwei Organisationen auf sichere Weise zusammenbringen sowie den Endbenutzern eine nahtlose und intuitive Navigation ermöglichen.

**97 %** unserer Kunden haben uns mitgeteilt, dass die Azure AD B2B-Zusammenarbeit sehr wichtig für sie ist.

![Kreisdiagramm](media/active-directory-b2b-what-is-azure-ad-b2b/97-percent-support.png)

Bereits Anfang April 2017 nutzten ca. 3 Millionen Benutzer Funktionen der Azure AD B2B-Zusammenarbeit. Und mehr als 23 % der Azure AD-Organisationen, die über mehr als 10 Benutzer verfügen, profitieren bereits von diesen Funktionen.

## <a name="the-key-benefits-of-azure-ad-b2b-collaboration-to-your-organization"></a>Die wichtigsten Vorteile der Azure AD B2B-Zusammenarbeit für Ihre Organisation

### <a name="work-with-any-user-from-any-partner"></a>Zusammenarbeit mit jedem Benutzer bei jedem Partner

* Partner verwenden eigene Anmeldeinformationen

* Partner müssen nicht Azure AD verwenden

* Keine externen Verzeichnisse und keine komplexe Einrichtung erforderlich

### <a name="simple-and-secure-collaboration"></a>Einfache und sichere Zusammenarbeit

* Zugriff auf alle Unternehmens-Apps oder -daten gemäß differenzierten Azure AD-Autorisierungsrichtlinien

* Nahtlose Benutzererfahrung

* Sicherheit auf Unternehmensniveau für Apps und Daten

### <a name="no-management-overhead"></a>Kein zusätzlicher Verwaltungsaufwand

* Keine externe Konto- oder Kennwortverwaltung

* Keine Synchronisierung oder manuelle Verwaltung des Kontolebenszyklus

* Kein zusätzlicher externer Verwaltungsaufwand

## <a name="you-can-easily-add-b2b-collaboration-users-to-your-organization"></a>Sie können Ihrer Organisation problemlos B2B-Zusammenarbeitsbenutzer hinzufügen

Administratoren können im [Azure-Portal](https://portal.azure.com) B2B-Zusammenarbeitsbenutzer (Gastbenutzer) hinzufügen.

![Kreisdiagramm](media/active-directory-b2b-what-is-azure-ad-b2b/adding-b2b-users-admin.png)

### <a name="enable-your-collaborators-to-bring-their-own-identity"></a>Ermöglichen Sie den Projektmitarbeitern, sich mit der eigenen Identität anzumelden

B2B-Projektmitarbeiter können sich mit einer Identität ihrer Wahl anmelden. Wenn der Benutzer über kein Microsoft-Konto oder Azure AD-Konto verfügt, wird zum Zeitpunkt der Einlösung des Angebots ein solches nahtlos für ihn erstellt.

![Auswahl der Anmeldeidentität](media/active-directory-b2b-what-is-azure-ad-b2b/sign-in-identity-choice.png)

### <a name="delegate-to-application-and-group-owners"></a>Delegation an Anwendungs- und Gruppenbesitzer 
Anwendungs- und Gruppenbesitzer können B2B-Benutzer direkt jeder Anwendung hinzufügen, die Sie wünschen, und dies muss keine Microsoft-Anwendung sein. Administratoren können die Berechtigung zum Hinzufügen von B2B-Benutzern an Nicht-Administratoren delegieren. Nicht-Administratoren können im Bereich [Azure AD Application Access Management](https://myapps.microsoft.com) Anwendungen oder Gruppen B2B-Zusammenarbeitsbenutzer hinzufügen.

![Zugriffsbereich](media/active-directory-b2b-what-is-azure-ad-b2b/access-panel.png)

![Mitglied hinzufügen](media/active-directory-b2b-what-is-azure-ad-b2b/add-member.png)

### <a name="authorization-policies-protect-your-corporate-content"></a>Autorisierungsrichtlinien schützen Ihre Unternehmensinhalte

Administratoren können Richtlinien für den bedingten Zugriff festlegen. Beispielsweise kann Multi-Factor Authentication auf Mandantenebene, Anwendungsebene oder für bestimmte Benutzer erzwungen werden, um Unternehmens-Apps und -daten zu schützen.

![Mitglied hinzufügen](media/active-directory-b2b-what-is-azure-ad-b2b/add-member.png)

### <a name="use-our-apis-and-sample-code-to-easily-build-applications-to-onboard"></a>Mit unseren APIs und unserem Beispielcode können Sie einfach Anwendungen erstellen, die integriert werden sollen
Integrieren Sie Ihre externen Partner entsprechend den Anforderungen Ihrer Organisation.

Organisationen können mit den [Azure AD B2B-Einladungs-APIs](https://developer.microsoft.com/graph/docs/api-reference/v1.0/resources/invitation) die Integration umfassend anpassen, indem sie z.B. Self-Service-Registrierungsportale erstellen. Beispielcode hierfür wird auf Github bereitgestellt: <https://github.com/Azure/active-directory-dotnet-graphapi-b2bportal-web>

![Registrierungsportal](media/active-directory-b2b-what-is-azure-ad-b2b/sign-up-portal.png)

Mit der Azure AD B2B-Zusammenarbeit können Sie das gesamte Potenzial von Azure AD nutzen, um Ihre Partnerbeziehungen auf eine Weise zu schützen, die Endbenutzer als einfach und intuitiv empfinden. Folgen Sie also dem Beispiel Tausender von Organisationen, die bereits Azure AD B2B für ihre externe Zusammenarbeit verwenden!

## <a name="next-steps"></a>Nächste Schritte

* Informationen für Administratoren finden Sie im [Azure-Portal](https://portal.azure.com).

* Informationen für Information-Worker stehen im [Zugriffsbereich](https://myapps.microsoft.com) zur Verfügung.

* Und wie immer empfehlen wir, sich über unsere [Microsoft Tech Community](https://techcommunity.microsoft.com/t5/Azure-Active-Directory-B2B/bd-p/AzureAD_B2b) mit dem Produktteam in Verbindung zu setzen, um Feedback zu geben, Fragen zu erörtern und Vorschläge zu unterbreiten.

Weitere Artikel zur Azure AD B2B-Kollaboration:

* [Wie fügen Azure Active Directory-Administratoren B2B-Zusammenarbeitsbenutzer hinzu?](active-directory-b2b-admin-add-users.md)
* [Wie fügen Information-Worker B2B-Zusammenarbeitsbenutzer hinzu?](active-directory-b2b-iw-add-users.md)
* [Die Elemente der Einladungs-E-Mail von B2B-Zusammenarbeit](active-directory-b2b-invitation-email.md)
* [B2B-Zusammenarbeit: Einlösen von Einladungen](active-directory-b2b-redemption-experience.md)
* [Lizenzierung von Azure AD B2B-Zusammenarbeit](active-directory-b2b-licensing.md)
* [Problembehandlung für die Azure Active Directory B2B-Zusammenarbeit](active-directory-b2b-troubleshooting.md)
* [Häufig gestellte Fragen zur Azure Active Directory B2B-Zusammenarbeit](active-directory-b2b-faq.md)
* [Azure Active Directory B2B-Zusammenarbeit: API und Anpassung](active-directory-b2b-api.md)
* [Multi-Factor Authentication für Benutzer der B2B-Zusammenarbeit](active-directory-b2b-mfa-instructions.md)
* [Hinzufügen von Benutzern der B2B-Zusammenarbeit ohne Einladung](active-directory-b2b-add-user-without-invite.md)
* [Überwachung eines B2B-Zusammenarbeitsbenutzers und Berichterstellung](active-directory-b2b-auditing-and-reporting.md)
* [Artikelindex für die Anwendungsverwaltung in Azure Active Directory](active-directory-apps-index.md)

