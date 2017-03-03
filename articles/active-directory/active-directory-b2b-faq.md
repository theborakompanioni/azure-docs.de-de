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
ms.date: 02/16/2017
ms.author: sasubram
translationtype: Human Translation
ms.sourcegitcommit: 0c07c842ba8c6214d6746b0361af7b416069a6f5
ms.openlocfilehash: 3e92da61128a18c42726f4c3c85cd6e5024a697f
ms.lasthandoff: 02/17/2017


---

# <a name="azure-active-directory-b2b-collaboration-frequently-asked-questions-faq"></a>Häufig gestellte Fragen (FAQ) zur Azure Active Directory B2B-Zusammenarbeit

Der Abschnitt mit häufig gestellten Fragen wird in regelmäßigen Abständen aktualisiert, um neu aufkommende Fragen zu beantworten.

### <a name="is-this-functionality-available-in-the-azure-classic-portal"></a>Ist diese Funktion im klassischen Azure-Portal verfügbar?
Die neuen Funktionen in dieser aktualisierten öffentlichen Vorschau der Azure AD B2B-Zusammenarbeit sind nur über das [Azure-Portal](https://portal.azure.com) und den neuen Zugriffsbereich verfügbar. Testen

### <a name="can-b2b-collaboration-users-access-sharepoint-online-and-onedrive"></a>Können B2B-Zusammenarbeitsbenutzer auf SharePoint Online und OneDrive zugreifen?
Ihre Gastbenutzer für die B2B-Zusammenarbeit befinden sich im Verzeichnis. Sie können diese Benutzer zu Gruppen hinzuzufügen, denen Sie Berechtigungen für SharePoint Online- und OneDrive-Websites gewähren können. Sie können diese Benutzer auch über die SharePoint Online-Personenauswahl direkt auswählen. Da es sich um Gastbenutzer handelt, muss die externe Freigabe für die SharePoint-Websites aktiviert sein.

### <a name="is-the-csv-upload-mechanism-still-supported"></a>Wird der Mechanismus zum Hochladen von CSV-Dateien noch unterstützt?
Ja. Informationen dazu finden Sie im eingefügten PowerShell-Beispiel.

### <a name="how-can-i-customize-my-invitation-emails"></a>Wie kann ich meine Einladungs-E-Mails anpassen?
Mithilfe der B2B-Einladungs-APIs können Sie nahezu jedes Element des Einladungsprozesses anpassen.

### <a name="can-the-invited-external-user-leave-the-organization-to-which-he-was-invited"></a>Kann der eingeladene externe Benutzer die Organisation verlassen, zu der er eingeladen wurde?
Diese Funktionalität ist in dieser aktualisierten öffentlichen Vorschau derzeit nicht verfügbar.

### <a name="can-i-use-my-microsoft-account-johncontosomicrosoftacctcom-to-sign-in-to-resources"></a>Kann ich mein Microsoft-Konto (John@contosomicrosoftacct.com) verwenden, um mich an Ressourcen anzumelden?
In dieser aktualisierten öffentlichen Vorschau ist es nicht möglich, sich mit einem Microsoft-Konto anzumelden. Wenn Sie über ein nicht standardmäßiges Microsoft-Kontosuffix verfügen (z.B. für Unternehmens-E-Mails wie @contoso.com),, wird für Sie ein Azure Active Directory-Mandant erstellt.

### <a name="now-that-multi-factor-authentication-mfa-is-available-for-guest-users-can-they-also-reset-their-mfa-method"></a>Die Multi-Factor Authentication (MFA) ist jetzt für Gastbenutzer verfügbar – können diese Benutzer ihre MFA-Methode zurücksetzen?
Ja, dies funktioniert auf die gleiche Weise wie für normale Benutzer.

### <a name="which-organization-is-responsible-for-mfa-licenses"></a>Welche Organisation ist für die MFA-Lizenzen verantwortlich?
Die einladende Organisation ist für die Durchführung der MFA zuständig. Daher muss die einladende Organisation auch sicherstellen, dass sie über genügend Lizenzen für ihre B2B-Benutzer verfügt, die eine MFA durchführen.

### <a name="what-if-my-partner-org-already-has-mfa-set-up-can-we-trust-their-mfa-and-not-use-our-mfa"></a>Was passiert, wenn meine Partnerorganisation bereits einen MFA-Prozess eingerichtet hat? Kann ich dem MFA-Prozess des Partners vertrauen und nicht meine eigene MFA einsetzen?
Diese Funktionalität ist in dieser aktualisierten öffentlichen Vorschau noch nicht verfügbar, wird aber in zukünftigen Versionen unterstützt. Wenn die entsprechende Version veröffentlicht wurde, können Sie bestimmte Partner auswählen, die Sie als einladende Organisation von Ihrem MFA-Prozess ausschließen möchten.

### <a name="how-can-i-achieve-delayed-invitations"></a>Wie kann ich verzögerte Einladungen senden?
Einige Organisationen möchten B2B-Zusammenarbeitsbenutzer hinzufügen, die notwendigen Bereitstellungen für die Anwendungen einrichten und erst dann die Einladungen senden. Sie können die Einladungs-API für die B2B-Zusammenarbeit verwenden, um den Onboardingworkflow benutzerdefiniert anzupassen.

### <a name="can-guest-users-and-contacts-co-exist"></a>Können Gastbenutzer und Kontakte nebeneinander existieren?
Ihre Organisation hat möglicherweise Kontakte hinzugefügt, die externe Projektmitarbeiter repräsentieren. Diese werden in der globalen Adressliste und beim Erstellen von E-Mails als Vorschläge für E-Mail-Adressen angezeigt. Sie fragen sich vielleicht, was passiert, wenn Sie diese Projektmitarbeiter jetzt als B2B-Zusammenarbeitsbenutzer zum Verzeichnis hinzufügen. In einer zukünftigen Version können B2B-Zusammenarbeitsbenutzer und Kontaktobjekte im Unternehmensverzeichnis nebeneinander existieren. Informieren Sie sich in unseren Ankündigungen.

### <a name="can-i-make-my-guest-users-limited-admins"></a>Kann ich meinen Gastbenutzern eingeschränkte Administratorrechte zuweisen?
Absolut. Wenn Ihre Organisation diese Funktionalität benötigt, informieren Sie sich unter [Hinzufügen von Gastbenutzern zu einer Rolle](active-directory-users-assign-role-azure-portal.md).

### <a name="does-azure-ad-b2b-collaboration-support-permitting-b2b-users-to-access-the-azure-portal"></a>Unterstützt die Azure AD B2B-Zusammenarbeit das Gewähren des Zugriffs von B2B-Benutzern auf das Azure-Portal?
B2B-Zusammenarbeitsbenutzer sollten nicht auf das Azure-Portal zugreifen müssen, sofern ihnen nicht eine Administratorrolle mit eingeschränkten Berechtigungen oder eine globale Administratorrolle zugewiesen wurde. In diesem Fall können sie auf das Portal zugreifen. Ein Gastbenutzer, dem keine dieser Rollen zugewiesen wurde, kann auf einige Teile des Portals zugreifen, da die Gastbenutzerrolle über bestimmte Berechtigungen im Verzeichnis verfügt, wie in vorherigen Abschnitten beschrieben.

### <a name="can-i-block-access-to-the-azure-portal-for-guest-users"></a>Kann ich den Zugriff auf das Azure-Portal für Gastbenutzer sperren?
Ja. Gehen Sie beim Konfigurieren dieser Richtlinie umsichtig vor, um zu vermeiden, dass versehentlich der Zugriff für Mitglieder und Administratoren gesperrt wird.
Sie können den Zugriff auf das [Azure-Portal](https://portal.azure.com) für Gastbenutzer sperren, indem Sie die folgenden drei Schritte ausführen, um in der Microsoft Azure-Dienstverwaltungs-API eine Richtlinie für bedingten Zugriff einzurichten.
1. Ändern Sie die Gruppe **Alle Benutzer** so, dass nur noch Mitglieder enthalten sind: ![](media/active-directory-b2b-faq/modify-all-users-group.png)
2. Erstellen Sie eine dynamische Gruppe mit Gastbenutzern: ![](media/active-directory-b2b-faq/group-with-guest-users.png)
3. Richten Sie eine Richtlinie für bedingten Zugriff ein, um den Zugriff auf das Portal durch Gastbenutzer zu sperren, wie im folgenden Video gezeigt.

  >[!VIDEO https://channel9.msdn.com/Blogs/Azure/b2b-block-guest-user/Player]

  Wenn dieses Video nicht eingebettet angezeigt wird, können Sie es [hier](https://channel9.msdn.com/Blogs/Azure/b2b-block-guest-user) aufrufen.

### <a name="what-is-the-timeline-by-which-azure-ad-b2b-collaboration-will-start-support-for-mfa-and-consumer-email-accounts"></a>Wann wird die Azure AD B2B-Zusammenarbeit die MFA und E-Mail-Konten für Endbenutzer unterstützen?
Sowohl MFA als auch E-Mail-Konten für Endbenutzer werden jetzt in dieser aktualisierten öffentlichen Vorschau unterstützt.

### <a name="what-is-the-ga-timeline-for-azure-ad-b2b"></a>Für welchen Zeitraum ist die allgemeine Verfügbarkeit der Azure AD B2B-Zusammenarbeit geplant?
Der Zeitplan richtet sich nach dem Feedback, das wir für den aktuellen Featuresatz von Kunden erhalten.

### <a name="is-there-a-plan-to-support-password-reset-for-azure-ad-b2b-collaboration-users"></a>Ist die Unterstützung der Kennwortzurücksetzung für Azure AD B2B-Zusammenarbeitsbenutzer geplant?
Ja, diese Funktion wird für Benutzer und Gastbenutzer der B2B-Zusammenarbeit unterstützt.

### <a name="is-it-also-enabled-for-users-in-a-viral-tenant"></a>Ist die Funktion auch für Benutzer in einem viralen Mandanten aktiviert?
Derzeit nicht.

### <a name="does-microsoft-crm-provide-online-support-to-azure-ad-b2b-collaboration"></a>Bietet Microsoft CRM Onlineunterstützung für die Azure AD B2B-Zusammenarbeit?
CRM wird die Azure AD B2B-Zusammenarbeit unterstützen, sobald das Feature allgemein verfügbar ist.

### <a name="what-is-the-lifetime-of-an-initial-password-for-a-newly-created-b2b-collaboration-user"></a>Wie lang ist ein anfängliches Kennwort für einen neu erstellten B2B-Zusammenarbeitsbenutzer gültig?
Azure AD verfügt über einen festen Satz von Anforderungen an Zeichen, Kennwortstärke und Kontosperre, die gleichermaßen für alle Azure AD-Cloudbenutzerkonten gelten. Cloudbenutzerkonten sind diejenigen Konten, die nicht mit einem anderen Identitätsanbieter wie Microsoft-Konto, Facebook, AD FS oder (im Fall der B2B-Zusammenarbeit) einem anderen Cloudmandanten in einem Verbund stehen. Bei Verbundkonten richtet sich die Kennwortrichtlinie nach der Richtlinie im lokalen Mandanten und den Einstellungen des Microsoft-Kontos des Benutzers.

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

