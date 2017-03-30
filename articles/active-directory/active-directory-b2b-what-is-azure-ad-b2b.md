---
title: Informationen zur Vorschau der Azure Active Directory B2B-Zusammenarbeit | Microsoft-Dokumentation
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
ms.date: 03/18/2017
ms.author: sasubram
translationtype: Human Translation
ms.sourcegitcommit: 0d8472cb3b0d891d2b184621d62830d1ccd5e2e7
ms.openlocfilehash: 4bfd76a00ba694096ef0b045b14beb757ba0809f
ms.lasthandoff: 03/21/2017


---

# <a name="about-the-azure-ad-b2b-collaboration-preview"></a>Informationen zur Vorschau der Azure AD B2B-Zusammenarbeit
In diesem Artikel erfahren Sie mehr über die Ziele, Features und Vorteile der Aktualisierung der öffentlichen Vorschau der Azure Active Directory (Azure AD)-B2B-Zusammenarbeit.

Als IT-Experten und Information-Worker können Sie von zahlreichen Funktionen der Azure AD B2B-Zusammenarbeit profitieren. Sie ermöglicht eine enge Zusammenarbeit mit Partnern in anderen Organisationen weltweit, ungeachtet ihrer Größe, Branche oder Compliance- und Governance-Anforderungen. Sie können ihnen Zugriff auf Dokumente, Ressourcen und Anwendungen gewähren, ohne die vollständige Kontrolle über Ihre internen Daten zu verlieren.

Als Entwickler können Sie die Azure AD B2B-APIs verwenden, um Anwendungen zu schreiben, die Organisationen sicher miteinander verbinden. Information-Worker profitieren von einem einfachen Prozess und einer einfachen Navigation.

## <a name="how-b2b-collaboration-works"></a>Funktionsweise der B2B-Zusammenarbeit

IT-Experten und Information-Worker können in der aktuellen Vorschauversion Beziehungen zu anderen Organisationen einrichten, indem sie über das Azure-Portal oder die Einladungs-Manager-API deren Benutzer einzeln oder als Gruppe hinzufügen.

Administratoren können die neuen Benutzeroberflächen im Azure-Portal (https://portal.azure.com) und in PowerShell verwenden, um Beziehungen einzurichten.

Information-Worker können zudem die Zugriffsbereichsoberflächen unter http://myapps.microsoft.com einsetzen.

Entwickler können Anwendungen mit der Einladungs-Manager-API von Azure AD B2B erstellen, um Benutzer von B2B-Zusammenarbeit hinzuzufügen und die Einladungs- und Onboardingworkflows anzupassen.

Benutzer von B2B-Zusammenarbeit werden in der Regel über eine Einladung und einen Annahmevorgang hinzugefügt. Dies funktioniert folgendermaßen:

1. John Doe von WoodGrove möchte Sam Oogle über seine Gmail-E-Mail-Adresse (gsamoogle@gmail.com) als Benutzer hinzufügen.

2. John ruft das WoodGrove-Portal (portal.azure.com) oder den Zugriffsbereich (myapps.microsoft.com) auf, meldet sich an und fügt den Benutzer Sam Oogle dem WoodGrove-Verzeichnis, einer Gruppe oder einer Anwendung hinzu.

3. John Doe gibt eine benutzerdefinierte Einladungs-E-Mail an, die an Sam Oogle gesendet werden soll.

4. Wenn John Doe die Einladung sendet, wird der Benutzer Sam Oogle wie folgt in Azure AD für WoodGove erstellt:

  ![Die Administrator-Benutzeroberfläche in portal.azure.com](media/active-directory-b2b-what-is-azure-ad-b2b/user-is-added.png)

5. Nachdem der Benutzer erstellt wurde, sendet Azure AD eine Einladungs-E-Mail an Sam Oogle:

  ![An Sam Oogle gesendete Einladungs-E-Mail](media/active-directory-b2b-what-is-azure-ad-b2b/invitation-mail-sent-to-sam.png)

6. In der Einladung wählt Sam Oogle **Erste Schritte**.  
Das Azure-Portal wird geöffnet.

7. SAM Oogle meldet sich im Azure-Portal an.

8. Azure AD aktualisiert das Benutzerobjekt „Sam Oogle“ mit Informationen aus seinem Token in Azure AD:

  ![Benutzerprofil von SAM Oogle im Azure-Portal](media/active-directory-b2b-what-is-azure-ad-b2b/user-profile-is-populated.png)

9. Nachdem der Benutzer Sam Oogle die Einladung angenommen hat, kann er auf WoodGrove-Ressourcen zugreifen. Wie alle anderen Benutzer in Azure AD kann auch der Benutzer Sam Oogle vom Administrator verwaltet werden. Die Benutzerliste wird hier gezeigt:

  ![Die Azure AD-Benutzerliste](media/active-directory-b2b-what-is-azure-ad-b2b/sam-now-user-in-azure-ad.png)

## <a name="public-preview-features"></a>Features der öffentlichen Vorschau
Anhand des Feedbacks der Benutzer wurde die öffentliche Vorschau der B2B-Zusammenarbeit um wichtige Funktionen ergänzt, darunter:

* Für Administratoren: Abrufen von Erweiterungen der Benutzeroberfläche im [Azure-Portal](https://portal.azure.com). Administratoren können z.B. B2B-Benutzer zum Verzeichnis oder zu einer beliebigen Gruppe oder Anwendung hinzufügen.  

* Information-Worker: Abrufen von Self-Service-Einladungsfunktionen zu B2B-Zusammenarbeit für Information-Worker im [Zugriffsbereich](https://myapps.microsoft.com). Information-Worker können Benutzer von B2B-Zusammenarbeit zu einer beliebigen Self-Service-Gruppe oder -Anwendung einladen, die von ihnen verwaltet wird.

* Eingeladene Benutzer können über eine beliebige E-Mail-Adresse verfügen. Unabhängig davon, ob sie eine Office 365 oder eine lokale Microsoft Exchange-Adresse, eine Outlook.com-Adresse oder eine beliebige andere Adresse (Gmail, Yahoo! etc) verwenden, können Benutzer bei der Erstellung eines Azure AD- oder Microsoft-Kontos über auf die eingeladene Organisation zugreifen.

* Erstellen professioneller Einladungs-E-Mails mit dem Branding des Mandanten

* Anpassen der Benutzerausrichtung mithilfe von Einladungs-APIs

* Einrichten der Multi-Factor Authentication für Benutzer von B2B-Zusammenarbeit in der einladenden Organisation

* Delegieren von Einladungen an Nicht-Administratoren.

* Bereitstellen von PowerShell-Unterstützung für B2B-Zusammenarbeit

* Bereitstellen von Überwachungs- und Berichtsfunktionen

## <a name="help-us-shape-your-features"></a>Unterstützung bei der Entwicklung der Features
 
Ihr Feedback zur Verbesserung der B2B-Zusammenarbeit nehmen wir sehr ernst. Werden Sie Teil der [Microsoft Tech Community](https://techcommunity.microsoft.com/t5/Azure-Active-Directory-B2B/bd-p/AzureAD_B2b): Posten Sie Diskussionsbeiträge, teilen Sie uns Ihre Benutzerszenarien und bewährten Methoden mit, und lassen Sie uns wissen, was Ihnen an der Azure AD B2B-Zusammenarbeit gefällt.
 
Senden Sie uns zudem über die Website [B2B Collaboration Ideas](https://techcommunity.microsoft.com/t5/Azure-Active-Directory-B2B-Ideas/idb-p/AzureAD_B2B_Ideas) Ihre Anregungen und Ideen, und stimmen Sie für zukünftige Features ab.

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

