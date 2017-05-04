---
title: "Häufig gestellte Fragen zur Azure Active Directory B2B-Zusammenarbeit | Microsoft-Dokumentation"
description: "Häufig gestellte Fragen zur Azure Active Directory B2B-Zusammenarbeit"
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
ms.date: 04/13/2017
ms.author: sasubram
translationtype: Human Translation
ms.sourcegitcommit: e851a3e1b0598345dc8bfdd4341eb1dfb9f6fb5d
ms.openlocfilehash: c06e8b98fea6036704b07b0f12397fc4bb59cb3f
ms.lasthandoff: 04/15/2017


---

# <a name="azure-active-directory-b2b-collaboration-frequently-asked-questions-faq"></a>Häufig gestellte Fragen (FAQ) zur Azure Active Directory B2B-Zusammenarbeit

Der Abschnitt mit häufig gestellten Fragen wird in regelmäßigen Abständen aktualisiert, um neu aufkommende Fragen zu beantworten.

### <a name="is-this-functionality-available-in-the-azure-classic-portal"></a>Ist diese Funktion im klassischen Azure-Portal verfügbar?
Die neuen Funktionen in der Azure AD B2B-Zusammenarbeit sind nur über das [Azure-Portal](https://portal.azure.com) und den neuen Zugriffsbereich verfügbar. 

### <a name="is-it-possible-to-customize-our-sign-in-page-so-its-more-intuitive-for-your-b2b-collaboration-guest-users"></a>Können wir unsere Anmeldeseite anpassen, damit sie für Gastbenutzer der B2B-Zusammenarbeit intuitiver ist?
Auf jeden Fall. Es gibt einen [Blogbeitrag mit einer Erläuterung des Features](https://blogs.technet.microsoft.com/enterprisemobility/2017/04/07/improving-the-branding-logic-of-azure-ad-login-pages/) (in englischer Sprache), und das Verfahren ist außerdem in [Hinzufügen Ihres Unternehmensbranding zur Anmelde- und Zugriffsbereichsseite](active-directory-add-company-branding.md) dokumentiert.

### <a name="can-b2b-collaboration-users-access-sharepoint-online-and-onedrive"></a>Können B2B-Zusammenarbeitsbenutzer auf SharePoint Online und OneDrive zugreifen?
Ja. Allerdings ist die Möglichkeit, in der SharePoint Online Personenauswahl nach vorhandenen Gastbenutzern zu suchen dem Verhalten in älteren Versionen entsprechend standardmäßig deaktiviert. Sie können die Suche mit der Einstellung „ShowPeoplePickerSuggestionsForGuestUsers“ auf Mandanten- und Websitesammlungsebene aktivieren. Diese Einstellung kann mithilfe der Cmdlets „Set-SPOTenant“ und „Set-SPOSite“ vorgenommen werden, die Mitgliedern das Durchsuchen des Verzeichnisses nach allen vorhandenen Gastbenutzern ermöglichen. Änderungen im Mandantenbereich wirken sich nicht auf bereits bereitgestellte SharePoint Online-Websites aus.

### <a name="is-the-csv-upload-mechanism-still-supported"></a>Wird der Mechanismus zum Hochladen von CSV-Dateien noch unterstützt?
Ja. Informationen dazu finden Sie im eingefügten [PowerShell-Beispiel](active-directory-b2b-code-samples.md).

### <a name="how-can-i-customize-my-invitation-emails"></a>Wie kann ich meine Einladungs-E-Mails anpassen?
Mithilfe der [B2B-Einladungs-APIs](active-directory-b2b-api.md) können Sie nahezu jedes Element des Einladungsprozesses anpassen.

### <a name="can-the-invited-external-user-leave-the-organization-to-which-he-was-invited"></a>Kann der eingeladene externe Benutzer die Organisation verlassen, zu der er eingeladen wurde?
Dies ist momentan nicht verfügbar.

### <a name="now-that-multi-factor-authentication-mfa-is-available-for-guest-users-can-they-also-reset-their-mfa-method"></a>Die Multi-Factor Authentication (MFA) ist jetzt für Gastbenutzer verfügbar – können diese Benutzer ihre MFA-Methode zurücksetzen?
Ja, dies funktioniert auf die gleiche Weise wie für normale Benutzer.

### <a name="which-organization-is-responsible-for-mfa-licenses"></a>Welche Organisation ist für die MFA-Lizenzen verantwortlich?
Die einladende Organisation ist für die Durchführung der MFA zuständig. Daher muss die einladende Organisation auch sicherstellen, dass sie über genügend Lizenzen für ihre B2B-Benutzer verfügt, die eine MFA durchführen.

### <a name="what-if-my-partner-org-already-has-mfa-set-up-can-we-trust-their-mfa-and-not-use-our-mfa"></a>Was passiert, wenn meine Partnerorganisation bereits einen MFA-Prozess eingerichtet hat? Kann ich dem MFA-Prozess des Partners vertrauen und nicht meine eigene MFA einsetzen?
Dies wird in zukünftigen Versionen unterstützt. Wenn die entsprechende Version veröffentlicht wurde, können Sie bestimmte Partner auswählen, die Sie als einladende Organisation von Ihrem MFA-Prozess ausschließen möchten.

### <a name="how-can-i-achieve-delayed-invitations"></a>Wie kann ich verzögerte Einladungen senden?
Einige Organisationen möchten B2B-Zusammenarbeitsbenutzer hinzufügen, die notwendigen Bereitstellungen für die Anwendungen einrichten und erst dann die Einladungen senden. Sie können die Einladungs-API für die B2B-Zusammenarbeit verwenden, um den Onboardingworkflow benutzerdefiniert anzupassen.

### <a name="can-i-make-my-guest-users-limited-admins"></a>Kann ich meinen Gastbenutzern eingeschränkte Administratorrechte zuweisen?
Absolut. Wenn Ihre Organisation diese Funktionalität benötigt, informieren Sie sich unter [Hinzufügen von Gastbenutzern zu einer Rolle](active-directory-users-assign-role-azure-portal.md).

### <a name="does-azure-ad-b2b-collaboration-support-permitting-b2b-users-to-access-the-azure-portal"></a>Unterstützt die Azure AD B2B-Zusammenarbeit das Gewähren des Zugriffs von B2B-Benutzern auf das Azure-Portal?
B2B-Zusammenarbeitsbenutzer sollten nicht auf das Azure-Portal zugreifen müssen, sofern ihnen nicht eine Administratorrolle mit eingeschränkten Berechtigungen oder eine globale Administratorrolle zugewiesen wurde. In diesem Fall können sie auf das Portal zugreifen. Ein Gastbenutzer, dem keine dieser Rollen zugewiesen wurde, kann auf einige Teile des Portals zugreifen, da die Gastbenutzerrolle über bestimmte Berechtigungen im Verzeichnis verfügt, wie in vorherigen Abschnitten beschrieben.

### <a name="can-i-block-access-to-the-azure-portal-for-guest-users"></a>Kann ich den Zugriff auf das Azure-Portal für Gastbenutzer sperren?
Ja. Gehen Sie beim Konfigurieren dieser Richtlinie umsichtig vor, um zu vermeiden, dass versehentlich der Zugriff für Mitglieder und Administratoren gesperrt wird.
Sie können den Zugriff auf das [Azure-Portal](https://portal.azure.com) für Gastbenutzer sperren, indem Sie die folgenden drei Schritte ausführen, um in der Microsoft Azure-Dienstverwaltungs-API eine Richtlinie für bedingten Zugriff einzurichten.
1. Ändern Sie die Gruppe **Alle Benutzer** so, dass nur noch Mitglieder enthalten sind: ![Screenshot zum Ändern der Gruppe](media/active-directory-b2b-faq/modify-all-users-group.png)
2. Erstellen Sie eine dynamische Gruppe mit Gastbenutzern: ![Screenshot zum Erstellen der Gruppe](media/active-directory-b2b-faq/group-with-guest-users.png)
3. Richten Sie eine Richtlinie für bedingten Zugriff ein, um den Zugriff auf das Portal durch Gastbenutzer zu sperren, wie im folgenden Video gezeigt.
  
  >[!VIDEO https://channel9.msdn.com/Blogs/Azure/b2b-block-guest-user/Player] 

### <a name="what-is-the-timeline-by-which-azure-ad-b2b-collaboration-will-start-support-for-mfa-and-consumer-email-accounts"></a>Wann wird die Azure AD B2B-Zusammenarbeit die MFA und E-Mail-Konten für Endbenutzer unterstützen?
Sowohl MFA als auch E-Mail-Konten für Endbenutzer werden jetzt unterstützt.

### <a name="is-there-a-plan-to-support-password-reset-for-azure-ad-b2b-collaboration-users"></a>Ist die Unterstützung der Kennwortzurücksetzung für Azure AD B2B-Zusammenarbeitsbenutzer geplant?
Ja. Dies sind die Details zur Self-Service-Kennwortzurücksetzung (Self-Service Password Reset, SSPR) für einen B2B-Benutzer, der vom Identitätsmandanten zu einem Ressourcenmandanten eingeladen wurde:
 
* SSPR erfolgt nur im Identitätsmandanten des B2B-Benutzers.
* Der Identitätsmandant ist ein Microsoft-Konto – er verwendet den SSPR-Mechanismus für Microsoft-Konten.
* Wenn der Identitätsmandant ein Just-in-Time-Mandant/viraler Mandant ist, wird eine E-Mail für die Kennwortzurücksetzung gesendet.
* Für andere Mandanten wird der SSPR-Standardprozess für B2B-Benutzer verwendet. Ähnlich wie bei Mitgliedern wird SSPR für B2B-Benutzer im Kontext des Ressourcenmandanten blockiert.

### <a name="is-it-also-enabled-for-users-in-a-viral-tenant"></a>Ist die Funktion auch für Benutzer in einem viralen Mandanten aktiviert?
Derzeit nicht.

### <a name="does-microsoft-crm-provide-online-support-to-azure-ad-b2b-collaboration"></a>Bietet Microsoft CRM Onlineunterstützung für die Azure AD B2B-Zusammenarbeit?
Die Unterstützung hierfür ist in Vorbereitung.

### <a name="what-is-the-lifetime-of-an-initial-password-for-a-newly-created-b2b-collaboration-user"></a>Wie lang ist ein anfängliches Kennwort für einen neu erstellten B2B-Zusammenarbeitsbenutzer gültig?
Azure AD verfügt über einen festen Satz von Anforderungen an Zeichen, Kennwortstärke und Kontosperre, die gleichermaßen für alle Azure AD-Cloudbenutzerkonten gelten. Cloudbenutzerkonten sind diejenigen Konten, die nicht mit einem anderen Identitätsanbieter wie Microsoft-Konto, Facebook, AD FS oder (im Fall der B2B-Zusammenarbeit) einem anderen Cloudmandanten in einem Verbund stehen. Bei Verbundkonten richtet sich die Kennwortrichtlinie nach der Richtlinie im lokalen Mandanten und den Einstellungen des Microsoft-Kontos des Benutzers.

### <a name="applications-want-to-differentiate-their-experience-between-a-tenant-user-and-a-guest-user-is-there-standard-guidance-for-this-is-the-presence-of-the-identity-provider-claim-the-right-model-for-this"></a>Die Oberfläche einer Anwendung soll sich für Mandantenbenutzer und Gastbenutzer unterscheiden. Gibt es dafür eine Standardanleitung? Ist das Vorhandensein des Identitätsanbieteranspruchs das richtige Modell dafür?
 
Ein Gastbenutzer kann wie in [Eigenschaften eines B2B-Zusammenarbeitsbenutzers](active-directory-b2b-user-properties.md) erörtert einen beliebigen Identitätsanbieter zur Authentifizierung verwenden. Daher ist UserType die richtige Eigenschaft, um dies festzulegen. Der UserType-Anspruch ist derzeit nicht im Token enthalten. Anwendungen sollten zur Abfrage des Verzeichnisses nach dem Benutzer und zum Abrufen seiner UserType-Eigenschaft die Graph-API verwenden.

### <a name="where-can-find-a-b2b-collaboration-community-to-share-solutions-and-submit-ideas"></a>Wo finde ich eine Community für die B2B-Zusammenarbeit, um dort Lösungen und Ideen zu teilen?

Ihr Feedback zur Verbesserung der B2B-Zusammenarbeit nehmen wir sehr ernst. Werden Sie Teil der [Microsoft Tech Community](https://techcommunity.microsoft.com/t5/Azure-Active-Directory-B2B/bd-p/AzureAD_B2b): Posten Sie Diskussionsbeiträge, teilen Sie uns Ihre Benutzerszenarien und bewährten Methoden mit, und lassen Sie uns wissen, was Ihnen an der Azure AD B2B-Zusammenarbeit gefällt.
 
Senden Sie uns zudem über die Website [B2B Collaboration Ideas](https://techcommunity.microsoft.com/t5/Azure-Active-Directory-B2B-Ideas/idb-p/AzureAD_B2B_Ideas) Ihre Anregungen und Ideen, und stimmen Sie für zukünftige Features ab.

### <a name="is-there-a-way-to-invite-the-user-such-that-the-invitation-is-automatically-redeemed-and-the-user-is-just-ready-to-go-or-will-the-user-always-have-to-click-through-to-the-redemption-url"></a>Gibt es eine Möglichkeit, den Benutzer so einzuladen, dass die Einladung automatisch eingelöst wird und der Benutzer sie sofort wahrnehmen kann, oder muss der Benutzer sich immer bis zur Einlösungs-URL durchklicken?
Einladungen, die von einem Benutzer in der einladenden Organisation gesendet werden, der auch Mitglied in der eingeladenen Organisation ist (die Organisation des B2B-Benutzers), müssen nicht vom B2B-Benutzer eingelöst werden.

Wir empfehlen, zu diesem Zweck einen Benutzer aus der eingeladenen Organisation in die einladende Organisation einzuladen. [Fügen Sie diesen Benutzer der Rolle „Gasteinladender“ in der Ressourcenorganisation zu](active-directory-b2b-add-guest-to-role.md). Dieser Benutzer kann über die Anmeldebenutzeroberfläche, über PowerShell-Skripts oder APIs andere Benutzer in der eingeladenen Organisation einladen, ohne dass der B2B-Benutzer aus dieser Organisation die Einladung einlösen muss.

### <a name="how-does-b2b-collaboration-work-when-the-invited-partner-is-using-federation-to-add-their-own-on-premises-authentication"></a>Wie erfolgt die B2B-Zusammenarbeit, wenn der eingeladene Partner den Partnerverbund verwendet, um die eigene lokale Authentifizierung hinzuzufügen?
Wenn der Partner über einen Azure AD-Mandanten verfügt, der über einen Verbund in die lokale Authentifizierungsinfrastruktur eingebunden ist, wird automatisch lokales einmaliges Anmelden (Single Sign-On, SSO) ausgeführt. Wenn der Partner über keinen Azure AD-Mandanten verfügt, wird für den hinzukommenden Benutzer ein Azure AD-Konto erstellt. 

### <a name="i-didnt-think-azure-ad-b2b-would-accept-gmailcom-and-outlookcom-email-addresses-b2c-was-what-you-used-for-those"></a>Ich dachte, dass Azure AD B2B E-Mail-Adressen mit der Domäne „gmail.com“ oder „outlook.com“ nicht akzeptiert. Für diese Adressen wurde B2C verwendet.
Wir beseitigen die Unterschiede zwischen B2B und B2C im Hinblick auf die unterstützten Identitäten. Die verwendete Identität ist kein gutes Kriterium für die Entscheidung zwischen der Verwendung von B2B und B2C. Der folgende Artikel hilft Ihnen zu entscheiden, was Sie verwenden sollten: [Vergleichen von B2B-Zusammenarbeit und B2C in Azure Active Directory](active-directory-b2b-compare-b2c.md).

### <a name="what-applications-and-services-support-azure-b2b-guest-users"></a>Welche Anwendungen und Dienste unterstützen Azure B2B-Gastbenutzer?
Alle in Azure AD integrierten Anwendungen. 

### <a name="is-it-possible-to-force-mfa-for-b2b-guest-users-if-partners-have-no-mfa-enabled"></a>Kann MFA für B2B-Gastbenutzer erzwungen werden, wenn Partner MFA nicht aktiviert haben?
Ja. Weitere Informationen finden Sie unter [Multi-Factor Authentication für Benutzer der Azure Active Directory B2B-Zusammenarbeit](active-directory-b2b-mfa-instructions.md).

### <a name="within-sharepoint-you-can-define-an-allow-or-deny-list-for-external-users-any-plans-to-extend-this-to-azure-or-across-all-of-office-365"></a>Sie können in SharePoint Listen für das Zulassen und Ablehnen des Zugriffs durch externe Benutzer definieren. Gibt es Pläne, dies auf Azure oder auf Office 365 insgesamt zu erweitern?
Ja. Die Azure AD B2B-Zusammenarbeit unterstützt Zulassungs-/Ablehnungslisten. 

### <a name="what-licenses-are-needed-for-azure-ad-b2b"></a>Welche Lizenzen sind für Azure AD B2B erforderlich?
Informationen hierzu finden Sie im [Leitfaden zur Lizenzierung von Azure Active Directory B2B-Kollaboration](active-directory-b2b-licensing.md).

### <a name="next-steps"></a>Nächste Schritte

Weitere Artikel zur Azure AD B2B-Kollaboration:

* [Was ist die Azure AD B2B-Zusammenarbeit?](active-directory-b2b-what-is-azure-ad-b2b.md)
* [Wie fügen Azure Active Directory-Administratoren B2B-Zusammenarbeitsbenutzer hinzu?](active-directory-b2b-admin-add-users.md)
* [Wie fügen Information-Worker B2B-Zusammenarbeitsbenutzer hinzu?](active-directory-b2b-iw-add-users.md)
* [Die Elemente der Einladungs-E-Mail von B2B-Zusammenarbeit](active-directory-b2b-invitation-email.md)
* [B2B-Zusammenarbeit: Einlösen von Einladungen](active-directory-b2b-redemption-experience.md)
* [Lizenzierung von Azure AD B2B-Zusammenarbeit](active-directory-b2b-licensing.md)
* [Problembehandlung für die Azure Active Directory B2B-Zusammenarbeit](active-directory-b2b-troubleshooting.md)
* [Azure Active Directory B2B-Zusammenarbeit: API und Anpassung](active-directory-b2b-api.md)
* [Multi-Factor Authentication für Benutzer der B2B-Zusammenarbeit](active-directory-b2b-mfa-instructions.md)
* [Hinzufügen von Benutzern der B2B-Zusammenarbeit ohne Einladung](active-directory-b2b-add-user-without-invite.md)
* [Artikelindex für die Anwendungsverwaltung in Azure Active Directory](active-directory-apps-index.md)

