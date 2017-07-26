---
title: "Häufig gestellte Fragen zur Azure Active Directory B2B-Zusammenarbeit | Microsoft-Dokumentation"
description: "Erhalten Sie Antworten auf häufig gestellte Fragen zur Azure Active Directory B2B-Zusammenarbeit."
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
ms.date: 05/23/2017
ms.author: sasubram
ms.translationtype: Human Translation
ms.sourcegitcommit: a30a90682948b657fb31dd14101172282988cbf0
ms.openlocfilehash: bfedbbf8b26e1b129584a6a644e64a15635f5723
ms.contentlocale: de-de
ms.lasthandoff: 05/25/2017


---

# <a name="azure-active-directory-b2b-collaboration-faqs"></a>Häufig gestellte Fragen zur Azure Active Directory B2B-Zusammenarbeit

Diese häufig gestellten Fragen (FAQs) zur Azure AD B2B-Zusammenarbeit (Azure Active Directory Business-to-Business) werden regelmäßig mit neuen Themen aktualisiert.

### <a name="is-azure-ad-b2b-collaboration-available-in-the-azure-classic-portal"></a>Ist die Azure AD B2B-Zusammenarbeit im klassischen Azure-Portal verfügbar?
Nein. Die Funktionen für die Azure AD B2B-Zusammenarbeit sind nur im [Azure-Portal](https://portal.azure.com) und im [Zugriffsbereich](https://myapps.microsoft.com/) verfügbar. 

### <a name="can-we-customize-our-sign-in-page-so-it-is-more-intuitive-for-our-b2b-collaboration-guest-users"></a>Können wir unsere Anmeldeseite anpassen, damit sie für Gastbenutzer der B2B-Zusammenarbeit intuitiver ist?
Auf jeden Fall. Lesen Sie unseren [Blogbeitrag zu diesem Feature](https://blogs.technet.microsoft.com/enterprisemobility/2017/04/07/improving-the-branding-logic-of-azure-ad-login-pages/). Weitere Informationen zum Anpassen der Anmeldeseite Ihrer Organisation finden Sie unter [Hinzufügen von Unternehmensbranding zu Anmelde- und Zugriffsbereichsseiten](active-directory-add-company-branding.md).

### <a name="can-b2b-collaboration-users-access-sharepoint-online-and-onedrive"></a>Können B2B-Zusammenarbeitsbenutzer auf SharePoint Online und OneDrive zugreifen?
Ja. Allerdings ist die Möglichkeit, in SharePoint Online mithilfe der Personenauswahl nach vorhandenen Gastbenutzern zu suchen, standardmäßig auf **Aus** gesetzt. Um die Option zum Suchen nach vorhandenen Gastbenutzern zu aktivieren, setzen Sie **ShowPeoplePickerSuggestionsForGuestUsers** auf **Ein**. Sie können diese Einstellung entweder auf Mandantenebene oder Websitesammlungsebene aktivieren. Sie können diese Einstellung mithilfe der Cmdlets „Set-SPOTenant“ und „Set-SPOSite“ ändern. Mit diesen Cmdlets können Mitglieder nach allen vorhandenen Gastbenutzern im Verzeichnis suchen. Änderungen im Mandantenbereich wirken sich nicht auf SharePoint Online-Websites aus, die bereits bereitgestellt wurden.

### <a name="is-the-csv-upload-feature-still-supported"></a>Wird die Funktion zum Hochladen von CSV-Dateien noch unterstützt?
Ja. Weitere Informationen zur Verwendung der Funktion zum Hochladen von CSV-Dateien finden Sie in [diesem PowerShell-Beispiel](active-directory-b2b-code-samples.md).

### <a name="how-can-i-customize-my-invitation-emails"></a>Wie kann ich meine Einladungs-E-Mails anpassen?
Mithilfe der [B2B-Einladungs-APIs](active-directory-b2b-api.md) können Sie nahezu jedes Element des Einladungsprozesses anpassen.

### <a name="can-an-invited-external-user-leave-the-organization-after-being-invited"></a>Kann ein eingeladener externer Benutzer die Organisation verlassen, nachdem er eingeladen wurde?
Der Administrator der einladenden Organisation kann einen B2B-Zusammenarbeit-Gastbenutzer aus dem eigenen Verzeichnis löschen, der Gastbenutzer kann das Verzeichnis der einladenden Organisation jedoch nicht selbst verlassen. 

### <a name="can-guest-users-reset-their-multi-factor-authentication-method"></a>Können Gastbenutzer ihre Multi-Factor Authentication-Methode zurücksetzen?
Ja. Gastbenutzer können ihre Multi-Factor Authentication-Methode auf dieselbe Weise zurücksetzen wie normale Benutzer.

### <a name="which-organization-is-responsible-for-multi-factor-authentication-licenses"></a>Welche Organisation ist für Multi-Factor Authentication-Lizenzen verantwortlich?
Die einladende Organisation führt Multi-Factor Authentication durch. Die einladende Organisation muss sicherstellen, dass die Organisation genügend Lizenzen für B2B-Benutzer besitzt, die Multi-Factor Authentication verwenden.

### <a name="what-if-a-partner-organization-already-has-multi-factor-authentication-set-up-can-we-trust-their-multi-factor-authentication-and-not-use-our-own-multi-factor-authentication"></a>Was geschieht, wenn eine Partnerorganisation bereits Multi-Factor Authentication eingerichtet hat? Können wir ihrer Multi-Factor Authentication vertrauen und auf die Verwendung unserer eigenen Multi-Factor Authentication verzichten?
Dieses Feature ist für ein zukünftiges Release geplant. Sie können dann bestimmte Partner auswählen, die Sie als einladende Organisation von Ihrer mehrstufigen Authentifizierung ausschließen möchten.

### <a name="how-can-i-use-delayed-invitations"></a>Wie kann ich verzögerte Einladungen senden?
Es kann vorkommen, dass eine Organisation Benutzer der B2B-Zusammenarbeit hinzufügen, diese bei Bedarf für Anwendungen bereitstellen und erst dann die Einladungen senden möchte. Sie können die Einladungs-API für die B2B-Zusammenarbeit verwenden, um den Onboardingworkflow benutzerdefiniert anzupassen.

### <a name="can-i-make-a-guest-user-a-limited-administrator"></a>Kann ich einen Gastbenutzer in einen eingeschränkten Administrator konvertieren?
Absolut. Weitere Informationen finden Sie unter [Zuweisen einer Rolle für Gastbenutzer](active-directory-users-assign-role-azure-portal.md).

### <a name="does-azure-ad-b2b-collaboration-allow-b2b-users-to-access-the-azure-portal"></a>Erlaubt die Azure AD B2B-Zusammenarbeit B2B-Benutzern den Zugriff auf das Azure-Portal?
B2B-Zusammenarbeitsbenutzer benötigen keinen Zugriff auf das Azure-Portal, sofern ihnen nicht eine Administratorrolle mit eingeschränkten Berechtigungen oder eine globale Administratorrolle zugewiesen wurde. Allerdings können B2B-Zusammenarbeitsbenutzer, denen eine Administratorrolle mit eingeschränkten Berechtigungen oder eine globale Administratorrolle zugewiesen ist, auf das Portal zugreifen. Auch wenn ein Gastbenutzer, dem keine dieser Administratorrollen zugewiesen ist, auf das Portal zugreift, kann der Benutzer auf bestimmte Funktionen zugreifen. Die Rolle des Gastbenutzers ist mit einigen Berechtigungen im Verzeichnis versehen.

### <a name="can-i-block-access-to-the-azure-portal-for-guest-users"></a>Kann ich den Zugriff auf das Azure-Portal für Gastbenutzer sperren?
Ja. Gehen Sie beim Konfigurieren dieser Richtlinie umsichtig vor, um zu vermeiden, dass versehentlich der Zugriff für Mitglieder und Administratoren gesperrt wird.
Um den Zugriff eines Gastbenutzers auf das [Azure-Portal](https://portal.azure.com) zu blockieren, verwenden Sie eine Richtlinie für bedingten Zugriff in der API für das klassische Microsoft Azure-Bereitstellungsmodell:
1. Ändern Sie die Gruppe **Alle Benutzer**, sodass sie nur Mitglieder enthält.
  ![Screenshot zum Ändern der Gruppe](media/active-directory-b2b-faq/modify-all-users-group.png)
2. Erstellen Sie eine dynamische Gruppe mit Gastbenutzern.
  ![Screenshot zum Erstellen von Gruppen](media/active-directory-b2b-faq/group-with-guest-users.png)
3. Richten Sie eine Richtlinie für bedingten Zugriff ein, um den Zugriff auf das Portal durch Gastbenutzer zu blockieren, wie im folgenden Video gezeigt wird:
  
  > [!VIDEO https://channel9.msdn.com/Blogs/Azure/b2b-block-guest-user/Player] 

### <a name="does-azure-ad-b2b-collaboration-support-multi-factor-authentication-and-consumer-email-accounts"></a>Unterstützt die Azure AD B2B-Zusammenarbeit Multi-Factor Authentication und E-Mail-Konten für Consumer?
Ja. Sowohl Multi-Factor Authentication als auch E-Mail-Konten für Consumer werden für die Azure AD B2B-Zusammenarbeit unterstützt.

### <a name="do-you-plan-to-support-password-reset-for-azure-ad-b2b-collaboration-users"></a>Ist die Unterstützung der Kennwortzurücksetzung für Azure AD B2B-Zusammenarbeitsbenutzer geplant?
Ja. Die folgenden Informationen sind für SSPR (Self-Service Password Reset, selbstständige Kennwortrücksetzung) für einen B2B-Benutzer wichtig, der aus einer Partnerorganisation eingeladen wird:
 
* SSPR erfolgt nur im Identitätsmandanten des B2B-Benutzers.
* Der Identitätsmandant ist ein Microsoft-Konto – er verwendet den SSPR-Mechanismus für Microsoft-Konten.
* Wenn der Identitätsmandant ein Just-in-Time-Mandant (JIT) oder „viraler“ Mandant ist, wird eine E-Mail für die Kennwortzurücksetzung gesendet.
* Bei anderen Mandanten wird der standardmäßige SSPR-Prozess für B2B-Benutzer durchgeführt. Wie bei Mitglieds-SSPR für B2B-Benutzer werden Mandanten im Kontext der Ressource blockiert. 

### <a name="is-password-reset-available-for-guest-users-in-a-just-in-time-jit-or-viral-tenant-who-accepted-invitations-with-a-work-or-school-email-address-but-who-didnt-have-a-pre-existing-azure-ad-account"></a>Ist die Kennwortrücksetzung bei Gastbenutzern eines JIT- (Just-In-Time) oder „viralen“ Mandanten verfügbar, der Einladungen mit einer Geschäfts-, Schul- oder Uni-E-Mail-Adresse akzeptiert, jedoch zuvor über kein Azure AD-Konto verfügt hat?
Ja. Es kann eine E-Mail für die Kennwortrücksetzung gesendet werden, über die ein Benutzer sein Kennwort im JIT-Mandanten zurücksetzen kann.

### <a name="does-microsoft-dynamics-crm-provide-online-support-for-azure-ad-b2b-collaboration"></a>Bietet Microsoft Dynamics CRM Onlineunterstützung für die Azure AD B2B-Zusammenarbeit?
Derzeit bietet Microsoft Dynamics CRM keine Onlineunterstützung für die Azure AD B2B-Zusammenarbeit. Allerdings ist geplant, diese Funktion in Zukunft einzuführen.

### <a name="what-is-the-lifetime-of-an-initial-password-for-a-newly-created-b2b-collaboration-user"></a>Wie lang ist ein anfängliches Kennwort für einen neu erstellten B2B-Zusammenarbeitsbenutzer gültig?
Azure AD verfügt über einen festen Satz von Anforderungen an Zeichen, Kennwortstärke und Kontosperre, die gleichermaßen für alle Azure AD-Cloudbenutzerkonten gelten. Cloudbenutzerkonten sind Konten, die nicht mit einem anderen Identitätsanbieter verbunden sind, z.B. 
* Microsoft-Konto
* Facebook
* Active Directory-Verbunddienste (AD FS)
* Ein anderer Cloudmandant (für B2B-Zusammenarbeit)

Bei Verbundkonten richtet sich die Kennwortrichtlinie nach der Richtlinie, die im lokalen Mandanten und den Einstellungen des Microsoft-Kontos des Benutzers angewendet wird.

### <a name="an-organization-might-want-to-have-different-experiences-in-their-applications-for-tenant-users-and-guest-users-is-there-standard-guidance-for-this-is-the-presence-of-the-identity-provider-claim-the-correct-model-to-use"></a>Eine Organisation möchte ggf. verschiedene Funktionen in ihren Anwendungen für Mandanten- und Gastbenutzer bereitstellen. Gibt es dafür eine Standardanleitung? Ist das Vorhandensein des Identitätsanbieteranspruchs das richtige Modell dafür?
 Ein Gastbenutzer kann einem beliebigen Identitätsanbieter zur Authentifizierung verwenden. Weitere Informationen finden Sie unter [Eigenschaften eines B2B-Zusammenarbeitsbenutzers](active-directory-b2b-user-properties.md). Verwenden Sie die **UserType**-Eigenschaft, um die Benutzerfreundlichkeit zu bestimmen. Der **UserType**-Anspruch ist derzeit nicht im Token enthalten. Anwendungen sollten zur Abfrage des Verzeichnisses nach dem Benutzer und zum Abrufen seiner UserType-Eigenschaft die Graph-API verwenden.

### <a name="where-can-i-find-a-b2b-collaboration-community-to-share-solutions-and-to-submit-ideas"></a>Wo finde ich eine Community für die B2B-Zusammenarbeit, um dort Lösungen und Ideen zu teilen?
Ihr Feedback zur Verbesserung der B2B-Zusammenarbeit nehmen wir sehr ernst. Wir würden uns freuen, wenn Sie uns Ihre Benutzerszenarien und bewährten Methoden mitteilen und uns wissen lassen würden, was Ihnen an der Azure AD B2B-Zusammenarbeit gefällt. Werden Sie Teil der [Microsoft Tech Community](https://techcommunity.microsoft.com/t5/Azure-Active-Directory-B2B/bd-p/AzureAD_B2b) und posten Sie Diskussionsbeiträge.
 
Senden Sie uns zudem über die Website [B2B Collaboration Ideas](https://techcommunity.microsoft.com/t5/Azure-Active-Directory-B2B-Ideas/idb-p/AzureAD_B2B_Ideas) Ihre Anregungen und Ideen, und stimmen Sie über zukünftige Features ab.

### <a name="can-we-send-an-invitation-that-is-automatically-redeemed-so-that-the-user-is-just-ready-to-go-or-does-the-user-always-have-to-click-through-to-the-redemption-url"></a>Können eine automatisch eingelöste Einladung gesendet werden, damit der Benutzer sofort loslegen kann, oder muss sich der Benutzer stets bis zur Einlöse-URL durchklicken?
Einladungen, die von einem Benutzer in der einladenden Organisation gesendet werden, der auch Mitglied in der Partnerorganisation ist, müssen nicht vom B2B-Benutzer eingelöst werden.

Es wird empfohlen, einen Benutzer aus der Partnerorganisation in die einladende Organisation einzuladen. [Fügen Sie diesen Benutzer der Rolle „Gasteinladender“ in der Ressourcenorganisation zu](active-directory-b2b-add-guest-to-role.md). Dieser Benutzer kann über die Anmeldeoberfläche, PowerShell-Skripts oder APIs andere Benutzer in der Partnerorganisation einladen. Anschließend müssen B2B-Zusammenarbeitsbenutzer aus dieser Organisation ihre Einladungen nicht zwingend einlösen.

### <a name="how-does-b2b-collaboration-work-when-the-invited-partner-is-using-federation-to-add-their-own-on-premises-authentication"></a>Wie erfolgt die B2B-Zusammenarbeit, wenn der eingeladene Partner den Partnerverbund verwendet, um die eigene lokale Authentifizierung hinzuzufügen?
Wenn der Partner über einen Azure AD-Mandanten verfügt, der über einen Verbund in die lokale Authentifizierungsinfrastruktur eingebunden ist, wird automatisch lokales einmaliges Anmelden (Single Sign-On, SSO) ausgeführt. Wenn der Partner keinen Azure AD-Mandanten aufweist, wird für neue Benutzer ein Azure AD-Konto erstellt. 

### <a name="i-thought-azure-ad-b2b-didnt-accept-gmailcom-and-outlookcom-email-addresses-and-that-b2c-was-used-for-those-kinds-of-accounts"></a>Stimmt es, dass Azure AD B2B keine E-Mail-Adressen von gmail.com und outlook.com akzeptiert und B2C für diese Arten von Konten verwendet wird?
Wir heben die Unterschiede zwischen der B2B- und B2C-Zusammenarbeit (Business-to-Company) im Hinblick auf die unterstützten Identitäten auf. Die verwendete Identität ist kein gutes Kriterium für die Entscheidung zwischen der Verwendung von B2B und B2C. Informationen zur Auswahl der Option für die Zusammenarbeit finden Sie unter [Vergleich der B2B- und B2C-Zusammenarbeit in Azure Active Directory](active-directory-b2b-compare-b2c.md).

### <a name="what-applications-and-services-support-azure-b2b-guest-users"></a>Welche Anwendungen und Dienste unterstützen Azure B2B-Gastbenutzer?
Alle in Azure AD integrierte Anwendungen unterstützen Azure B2B-Gastbenutzer. 

### <a name="can-we-force-multi-factor-authentication-for-b2b-guest-users-if-our-partners-dont-have-multi-factor-authentication"></a>Kann Multi-Factor Authentication für B2B-Gastbenutzer erzwungen werden, wenn unsere Partner nicht über Multi-Factor Authentication verfügen?
Ja. Weitere Informationen finden Sie unter [Bedingter Zugriff für Benutzer der B2B-Zusammenarbeit](active-directory-b2b-mfa-instructions.md).

### <a name="in-sharepoint-you-can-define-an-allow-or-deny-list-for-external-users-can-we-do-this-in-azure"></a>In SharePoint können Listen für das Zulassen und Ablehnen des Zugriffs durch externe Benutzer definiert werden. Ist dies in Azure möglich?
Ja. Die Azure AD B2B-Zusammenarbeit unterstützt Zulassungs- und Verweigerungslisten. 

### <a name="what-licenses-do-we-need-to-use-azure-ad-b2b"></a>Welche Lizenzen sind für die Verwendung von Azure AD B2B erforderlich?
Informationen darüber, welche Lizenzen Ihre Organisation für die Verwendung von Azure AD B2B benötigt, finden Sie unter [Leitfaden zur Lizenzierung der Azure Active Directory B2B-Zusammenarbeit](active-directory-b2b-licensing.md).

### <a name="next-steps"></a>Nächste Schritte

Weitere Artikel zur Azure AD B2B-Kollaboration:

* [Was ist die Azure AD B2B-Zusammenarbeit?](active-directory-b2b-what-is-azure-ad-b2b.md)
* [Wie fügen Azure AD-Administratoren B2B-Zusammenarbeitsbenutzer hinzu?](active-directory-b2b-admin-add-users.md)
* [Wie fügen Information-Worker B2B-Zusammenarbeitsbenutzer hinzu?](active-directory-b2b-iw-add-users.md)
* [Die Elemente der Einladungs-E-Mail von B2B-Zusammenarbeit](active-directory-b2b-invitation-email.md)
* [B2B-Zusammenarbeit: Einlösen von Einladungen](active-directory-b2b-redemption-experience.md)
* [Lizenzierung von Azure AD B2B-Zusammenarbeit](active-directory-b2b-licensing.md)
* [Behandlung von Problemen in Bezug auf die Azure AD B2B-Zusammenarbeit](active-directory-b2b-troubleshooting.md)
* [Azure AD B2B-Zusammenarbeit: API und Anpassung](active-directory-b2b-api.md)
* [Multi-Factor Authentication für Benutzer der B2B-Zusammenarbeit](active-directory-b2b-mfa-instructions.md)
* [Hinzufügen von Benutzern der B2B-Zusammenarbeit ohne Einladung](active-directory-b2b-add-user-without-invite.md)
* [Artikelindex für die Anwendungsverwaltung in Azure AD](active-directory-apps-index.md)

