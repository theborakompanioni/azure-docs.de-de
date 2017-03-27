---
title: "Problembehandlung für die Azure Active Directory B2B-Zusammenarbeit | Microsoft-Dokumentation"
description: "Lösungen für häufig auftretende Probleme bei der Azure Active Directory B2B-Zusammenarbeit"
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
ms.date: 03/14/2017
ms.author: sasubram
translationtype: Human Translation
ms.sourcegitcommit: a087df444c5c88ee1dbcf8eb18abf883549a9024
ms.openlocfilehash: 66c0084c89b5c7510196142afd27b58953d0dc86
ms.lasthandoff: 03/15/2017


---

# <a name="troubleshooting-azure-active-directory-b2b-collaboration"></a>Problembehandlung für die Azure Active Directory B2B-Zusammenarbeit

Hier finden Sie Lösungen für häufig auftretende Probleme bei der Azure Active Directory B2B-Zusammenarbeit.

## <a name="i-cant-create-an-external-user-due-to-an-existing-contact"></a>Ich kann aufgrund eines schon vorhandenen Kontakts einen externen Benutzer nicht erstellen.

Wenn für den externen Benutzer, den Sie einladen möchten, bereits ein Kontaktobjekt vorhanden ist, können Sie diesen Benutzer erst dann einladen, wenn der Konflikt gelöst ist – üblicherweise durch Entfernen des Kontaktobjekts. Bis zur allgemeinen Verfügbarkeit der B2B-Zusammenarbeit müssen solche Konflikte manuell gelöst werden.

## <a name="ive-added-an-external-user-but-do-not-see-them-in-my-global-address-book-or-in-the-people-picker"></a>Ich habe einen externen Benutzer hinzugefügt, dieser wird aber im globalen Adressbuch oder in der Personenauswahl nicht angezeigt.

Falls externe Benutzer in der Liste nicht angezeigt werden: Es kann einige Minuten dauern, bis das Objekt repliziert wurde.

## <a name="a-b2b-guest-user-is-not-showing-up-in-sharepoint-onlineonedrive-people-picker"></a>Ein B2B-Gastbenutzer wird in der SharePoint Online/OneDrive-Personenauswahl nicht angezeigt. 
 
Die Möglichkeit, in der SharePoint Online Personenauswahl nach vorhandenen Gastbenutzern zu suchen, ist dem Verhalten in älteren Versionen entsprechend standardmäßig deaktiviert.
Sie können die Suche mit der Einstellung „ShowPeoplePickerSuggestionsForGuestUsers“ auf Mandanten- und Websitesammlungsebene aktivieren. Diese Einstellung kann mithilfe der Cmdlets „Set-SPOTenant“ und „Set-SPOSite“ vorgenommen werden, die Mitgliedern das Durchsuchen des Verzeichnisses nach allen vorhandenen Gastbenutzern ermöglichen. Änderungen im Mandantenbereich wirken sich nicht auf bereits bereitgestellte SPO-Websites aus.

## <a name="invitations-have-been-disabled-for-directory"></a>Einladungen wurden für das Verzeichnis deaktiviert.

Wenn Sie in einer Fehlermeldung darüber informiert werden, dass Sie nicht über die Berechtigung verfügen, Benutzer einzuladen, überprüfen Sie, ob Ihr Benutzerkonto für die Einladung externer Benutzer autorisiert ist. Dies kann unter „Benutzereinstellungen“ erfolgen:

![](media/active-directory-b2b-troubleshooting/external-user-settings.png)

Wenn Sie diese Einstellungen kürzlich geändert oder einem Benutzer die Rolle „Gasteinladender“ zugewiesen haben, kann es 15–60 Minuten dauern, bis die Änderungen wirksam werden.

## <a name="the-user-that-i-invited-is-receiving-an-error-during-redemption"></a>Der von mir eingeladene Benutzer erhält beim Einlösen eine Fehlermeldung.

Häufige Fehler sind z.B. folgende:

### <a name="invitees-admin-has-disallowed-emailverified-users-from-being-created-in-their-tenant"></a>Der Administrator des Eingeladenen hat die Option deaktiviert, Benutzer im Mandanten zu erstellen, die per E-Mail verifiziert werden.

Dieser Fehler kann auftreten, wenn Benutzer eingeladen werden, deren Organisation Azure Active Directory verwenden, das Konto des entsprechenden Benutzers aber dort nicht vorhanden ist (Benutzer ist z.B. in Azure AD für contoso.com nicht vorhanden). Der Administrator von contoso.com hat möglicherweise eine Richtlinie festgelegt, die das Erstellen von Benutzern verhindert. Der Benutzer muss sich bei seinem Administrator informieren, ob externe Benutzer zulässig sind. Der Administrator des externen Benutzers muss ggf. per E-Mail verifizierte Benutzer in seiner Domäne zulassen (Informationen dazu finden Sie in diesem [Artikel](https://docs.microsoft.com/powershell/msonline/v1/set-msolcompanysettings#parameters) zum Zulassen per E-Mail verifizierter Benutzer).

![](media/active-directory-b2b-troubleshooting/allow-email-verified-users.png)

### <a name="external-user-does-not-exist-already-in-a-federated-domain"></a>Ein Benutzer ist in einer Verbunddomäne noch nicht vorhanden.

Wenn ein externer Benutzer eine Verbundlösung verwendet, in der die Authentifizierung lokal erfolgt, und der Benutzer in Azure Active Directory noch nicht vorhanden ist, kann der Benutzer nicht eingeladen werden.

Um dieses Problem zu lösen, muss der Administrator des externen Benutzers das Benutzerkonto mit Azure Active Directory synchronisieren.

## <a name="how-does--which-is-not-normally-a-valid-character-sync-with-azure-ad"></a>Wie wird „\#“ – normalerweise ein ungültiges Zeichen – in Azure AD synchronisiert?

„\#“ ist ein reserviertes Zeichen in UPNs für die Azure AD B2B-Zusammenarbeit oder externe Benutzer (der UPN &lt;user@contoso.com&gt; eines eingeladenen Benutzers wird zu &lt;user_contoso.com#EXT@fabrikam.onmicrosoft.com&gt;). Aus lokalen Systemen abgerufene UPNs mit \# dürfen sich daher nicht am Azure-Portal anmelden.

## <a name="i-receive-an-error-when-adding-external-users-to-a-synchronized-group"></a>Ich erhalte eine Fehlermeldung, wenn ich externe Benutzer zu einer synchronisierten Gruppe hinzufüge.

Externe Benutzer können nur zu „zugewiesenen“ Gruppen oder „Sicherheitsgruppen“ hinzugefügt werden, nicht zu lokal verwalteten Gruppen.

## <a name="my-external-user-did-not-receive-an-email-to-redeem"></a>Mein externer Benutzer hat keine E-Mail zur Einlösung erhalten.

Der Eingeladene sollte seinen Spamfilter überprüfen oder sich mit seinem ISP in Verbindung setzen, um sicherzustellen, dass die folgende Adresse zulässig ist: &lt;Invites@microsoft.com&gt;

## <a name="my-recipient-received-multiple-emails-from-me"></a>Mein Empfänger hat mehrere E-Mails von mir erhalten.

Wenn der Empfänger einer Einladung über mehrere Aliase für sein Konto verfügt, erhält er möglicherweise zwei Einladungen. In diesen Fällen wird der erste eingelöste Link zu dem Konto, das erstellt wird. Der zweite Einlösungslink ist dann ungültig.

## <a name="next-steps"></a>Nächste Schritte

Weitere Artikel zur Azure AD B2B-Kollaboration:

* [Was ist die Azure AD B2B-Zusammenarbeit?](active-directory-b2b-what-is-azure-ad-b2b.md)
* [Wie fügen Azure Active Directory-Administratoren B2B-Zusammenarbeitsbenutzer hinzu?](active-directory-b2b-admin-add-users.md)
* [Wie fügen Information-Worker B2B-Zusammenarbeitsbenutzer hinzu?](active-directory-b2b-iw-add-users.md)
* [Die Elemente der Einladungs-E-Mail von B2B-Zusammenarbeit](active-directory-b2b-invitation-email.md)
* [B2B-Zusammenarbeit: Einlösen von Einladungen](active-directory-b2b-redemption-experience.md)
* [Lizenzierung der Azure AD B2B-Zusammenarbeit](active-directory-b2b-licensing.md)
* [Häufig gestellte Fragen zur Azure Active Directory B2B-Zusammenarbeit](active-directory-b2b-faq.md)
* [Azure Active Directory B2B-Zusammenarbeit: API und Anpassung](active-directory-b2b-api.md)
* [Multi-Factor Authentication für Benutzer der B2B-Zusammenarbeit](active-directory-b2b-mfa-instructions.md)
* [Hinzufügen von Benutzern der B2B-Zusammenarbeit ohne Einladung](active-directory-b2b-add-user-without-invite.md)
* [Artikelindex für die Anwendungsverwaltung in Azure Active Directory](active-directory-apps-index.md)

