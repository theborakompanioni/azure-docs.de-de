---

title: "Hinzufügen von Benutzern der B2B-Zusammenarbeit zu Azure Active Directory ohne Einladung | Microsoft-Dokumentation"
description: "Die Azure Active Directory B2B-Zusammenarbeit ermöglicht es Information-Workern, Benutzer aus ihrer Organisationen zu Azure AD hinzuzufügen, um ihnen Zugriff auf die unternehmenseigenen Anwendungen zu gewähren."
services: active-directory
documentationcenter: 
author: sasubram
manager: femila
editor: 
tags: 
ms.assetid: 
ms.service: active-directory
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: identity
ms.date: 02/10/2017
ms.author: sasubram
translationtype: Human Translation
ms.sourcegitcommit: 0eef684115f9c21ea61502a10576f74cac0ace8e
ms.openlocfilehash: 8fa8ba169ca85c33e52eee4a7cd5b84aa4012673


---

# <a name="add-b2b-collaboration-users-without-an-invitation"></a>Hinzufügen von Benutzern der B2B-Zusammenarbeit ohne Einladung

Wenn der Einladende zu einer Rolle gehört, die im Verzeichnis der Partnerorganisation, aus der er Benutzer hinzufügt, über Enumerationsberechtigungen verfügt, werden die eingeladenen Benutzer zur einladenden Organisation hinzugefügt, ohne dass explizite Einladungen erforderlich sind.

Im folgenden Szenario ist dies besonders hilfreich:

1. Ein Benutzer in der Hostorganisation (z.B. WoodGrove) lädt einen Benutzer aus der Partnerorganisation (z.B. Sam@litware.com) als Gast ein.
2. Der Administrator der Hostorganisation legt Richtlinien fest, die es Sam erlauben, weitere Benutzer aus der Partnerorganisation (Litware) zu identifizieren und hinzuzufügen.
3. Jetzt kann Sam weitere Benutzer von Litware zum Verzeichnis sowie zu den Gruppen und Anwendungen von WoodGrove hinzufügen, ohne dass Einladungen eingelöst werden müssen. Wenn Sam über entsprechende Enumerationsberechtigungen in Litware verfügt, erfolgt dies automatisch.

### <a name="next-steps"></a>Nächste Schritte

Weitere Artikel zur Azure AD B2B-Kollaboration:

* [Was ist die Azure AD B2B-Zusammenarbeit?](active-directory-b2b-what-is-azure-ad-b2b.md)
* [Wie fügen Azure Active Directory-Administratoren B2B-Zusammenarbeitsbenutzer hinzu?](active-directory-b2b-admin-add-users.md)
* [Wie fügen Information-Worker B2B-Zusammenarbeitsbenutzer hinzu?](active-directory-b2b-iw-add-users.md)
* [Die Elemente der Einladungs-E-Mail von B2B-Zusammenarbeit](active-directory-b2b-invitation-email.md)
* [B2B-Zusammenarbeit: Einlösen von Einladungen](active-directory-b2b-redemption-experience.md)
* [Lizenzierung von Azure AD B2B-Zusammenarbeit](active-directory-b2b-licensing.md)
* [Problembehandlung für die Azure Active Directory B2B-Zusammenarbeit](active-directory-b2b-troubleshooting.md)
* [Häufig gestellte Fragen zur Azure Active Directory B2B-Zusammenarbeit](active-directory-b2b-faq.md)
* [Azure Active Directory B2B-Zusammenarbeit: API und Anpassung](active-directory-b2b-api.md)
* [Multi-Factor Authentication für Benutzer der B2B-Zusammenarbeit](active-directory-b2b-mfa-instructions.md)
* [Artikelindex für die Anwendungsverwaltung in Azure Active Directory](active-directory-apps-index.md)



<!--HONumber=Feb17_HO2-->


