---
title: Was ist Azure Active Directory B2B-Zusammenarbeit (Vorschau)? | Microsoft Docs
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
ms.date: 02/18/2017
ms.author: sasubram
translationtype: Human Translation
ms.sourcegitcommit: 14028332ecc17478230d035206e8e00f6af67f7b
ms.openlocfilehash: 8b6bf676dfa0df45d59763db70655d85fa08809a
ms.lasthandoff: 02/23/2017


---

# <a name="what-is-azure-ad-b2b-collaboration-preview"></a>Was ist Azure AD B2B-Zusammenarbeit (Vorschau)?

Die Funktionen von Azure AD B2B-Zusammenarbeit ermöglichen IT-Experten und Information-Workern eine enge Zusammenarbeit mit ihren Partnern in anderen Organisationen weltweit. Sie können Zugriff auf Dokumente, Ressourcen und Anwendungen bereitstellen und gleichzeitig die vollständige Kontrolle über ihre interne Daten gewährleisten. Mithilfe der Business-to-Business-APIs in Azure AD können Entwickler Anwendungen schreiben, die die beiden Organisationen sicher miteinander verbinden und den Information-Workern eine nahtlose Anzeige und eine intuitive Navigation bieten.

Die Funktionen von Azure AD B2B-Zusammenarbeit ermöglichen Organisationen aller Größen und Branchen, unabhängig von ihren Konformitäts- und Governanceanforderungen, eine einfache und sichere Zusammenarbeit mit Mitarbeitern auf der ganzen Welt. Das ist das Ziel dieser aktualisierten öffentlichen Vorschau von B2B-Zusammenarbeit.

## <a name="how-does-it-work"></a>Wie funktioniert dies?

Um in der aktuellen Vorschauversion eine Beziehung zu einer Organisation einzurichten, können IT-Profis und Information-Worker über das Portal oder die Einladungs-Manager-API Benutzer aus einer anderen Organisation einzeln oder als Gruppe hinzufügen. Administratoren können zu diesem Zweck die neuen Benutzeroberflächen im Azure-Portal (https://portal.azure.com) und in PowerShell verwenden. Information-Worker können zudem die Zugriffsbereichsoberflächen unter http://myapps.microsoft.com einsetzen. Entwickler können Anwendungen mit der Einladungs-Manager-API von Azure AD B2B erstellen, um Benutzer von B2B-Zusammenarbeit hinzuzufügen und die Einladungs- und Onboardingworkflows anzupassen.

Benutzer von B2B-Zusammenarbeit werden in der Regel über eine Einladung und einen Annahmevorgang aufgenommen. Dies funktioniert folgendermaßen:

1. John Doe von WoodGrove möchte Sam Oogle über seine Gmail-E-Mail-Adresse (gsamoogle@gmail.com) hinzufügen.

2. John ruft das WoodGrove-Portal (portal.azure.com) oder den Zugriffsbereich (myapps.microsoft.com) auf, meldet sich an und fügt den Benutzer dem WoodGrove-Verzeichnis, einer Gruppe oder einer Anwendung hinzu.

3. John gibt eine benutzerdefinierte Einladungs-E-Mail an, die an Sam gesendet werden soll.

4. Sobald er damit fertig ist, wird folgender Benutzer in WoodGrove AD erstellt (der Screenshot zeigt die Verwaltungsoberfläche von portal.azure.com):

  ![Benutzer wird hinzugefügt](media/active-directory-b2b-what-is-azure-ad-b2b/user-is-added.png)

5. Sobald wir diesen Benutzer hinzugefügt haben, sendet Azure AD eine Einladungs-E-Mail an Sam:

  ![An Sam gesendete Einladungs-E-Mail](media/active-directory-b2b-what-is-azure-ad-b2b/invitation-mail-sent-to-sam.png)

6. Jetzt wählt Sam **Loslegen** und meldet sich an. Zu diesem Zeitpunkt aktualisiert Azure AD sein Benutzerobjekt im Verzeichnis mit Informationen aus seinem Token (der Screenshot zeigt die Verwaltungsoberfläche im [Azure-Portal](https://portal.azure.com)):

  ![Benutzerprofil wird aufgefüllt](media/active-directory-b2b-what-is-azure-ad-b2b/user-profile-is-populated.png)

7. Nachdem Sam die Einladung angenommen hat, kann er Zugriff auf WoodGrove-Ressourcen erhalten und natürlich wie jeder andere Benutzer im Verzeichnis vom Administrator verwaltet werden (der Screenshot zeigt die Verwaltungsoberfläche im [Azure-Portal](https://portal.azure.com)):

  ![Sam ist jetzt ein Benutzer in Azure AD](media/active-directory-b2b-what-is-azure-ad-b2b/sam-now-user-in-azure-ad.png)

## <a name="public-preview-features"></a>Features der öffentlichen Vorschau
Sie haben die Funktionen von B2B-Zusammenarbeit, die wir in einer öffentlichen Vorschau vorgestellt hatten, genutzt und uns massenhaft wertvolles Feedback gegeben. Das haben wir umgesetzt. In dieser aktualisierten öffentlichen Vorschau haben wir alle Verbesserungen zusammengefasst. Dies sind die wichtigsten Funktionen der aktualisierten öffentlichen Vorschau von B2B-Zusammenarbeit:

1. Erweiterungen der B2B-Verwaltungsoberfläche
  - Aufrufen von https://portal.azure.com
  - Möglichkeit für Administratoren zum Einladen von B2B-Benutzern zum Verzeichnis oder zu einer beliebigen Gruppe oder Anwendung

2. Self-Service-Einladungsfunktionen zu B2B-Zusammenarbeit für Information-Worker im Zugriffsbereich: https://myapps.microsoft.com. Information-Worker können Benutzer von B2B-Zusammenarbeit zu einer beliebigen Self-Service-Gruppe oder -Anwendung einladen, die von ihnen verwaltet wird.

3. Sie können jetzt einen Benutzer mit einer beliebigen E-Mail-Adresse einladen. Ganz gleich, ob der Benutzer über eine Office&365;- oder eine lokale Microsoft Exchange-E-Mail-Adresse, eine Outlook.com-E-Mail-Adresse oder eine beliebige E-Mail-Adresse aus sozialen Netzwerken (Gmail, Yahoo usw.) verfügt – er kann jetzt durch die einfache Inlineerstellung eines Azure AD-Kontos oder eines Microsoft-Kontos nahtlos auf die eingeladene Organisation zugreifen.

4. Profitieren Sie von einer professionellen Einladungs-E-Mail mit dem Branding des Mandanten.

5. Umfangreiche Möglichkeit zum Anpassen des Onboardings über die Einladung-APIs.

6. Mehrstufige Authentifizierung für Benutzer von B2B-Zusammenarbeit in der einladenden Organisation.

7. Möglichkeit zum Delegieren von Einladungen an Nicht-Administratoren.

8. PowerShell-Unterstützung für B2B-Zusammenarbeit.

9. Überwachungs- und Berichtsfunktionen.

## <a name="next-steps"></a>Nächste Schritte

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

