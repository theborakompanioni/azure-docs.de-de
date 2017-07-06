---
title: 'Rollout: Azure AD SSPR | Microsoft-Dokumentation'
description: "Tipps für ein erfolgreiches Rollout der Self-Service-Kennwortzurücksetzung von Azure AD"
services: active-directory
keywords: 
documentationcenter: 
author: MicrosoftGuyJFlo
manager: femila
ms.reviewer: gahug
ms.assetid: f8cd7e68-2c8e-4f30-b326-b22b16de9787
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 05/25/2017
ms.author: joflore
ms.custom: it-pro
ms.translationtype: Human Translation
ms.sourcegitcommit: c785ad8dbfa427d69501f5f142ef40a2d3530f9e
ms.openlocfilehash: 5bd17fc68447b8387dfaad33fe2d0055596ec086
ms.contentlocale: de-de
ms.lasthandoff: 05/26/2017


---
# <a name="roll-out-password-reset-for-users"></a>Rollout der Kennwortzurücksetzung für Benutzer

Die meisten Kunden führen die hier angegebenen Schritte aus, um für ein reibungsloses Rollout der SSPR-Funktionalität (Self-Service Password Reset, Self-Service-Kennwortzurücksetzung) zu sorgen.

1. [Aktivieren Sie die Kennwortzurücksetzung in Ihrem Verzeichnis](active-directory-passwords-getting-started.md).
2. [Konfigurieren Sie die lokalen AD-Berechtigungen für das Kennwortrückschreiben](active-directory-passwords-how-it-works.md#active-directory-permissions).
3. [Konfigurieren Sie das Kennwortrückschreiben](active-directory-passwords-writeback.md#configuring-password-writeback), um Kennwörter aus Azure AD wieder in Ihr lokales Verzeichnis zu schreiben.
4. [Weisen Sie die erforderlichen Lizenzen zu, und überprüfen Sie sie](active-directory-passwords-licensing.md).
5. Wenn Sie das Rollout in Etappen durchführen möchten, können Sie die Kennwortzurücksetzung optional auf eine Gruppe von Benutzern begrenzen, um das Feature nach und nach einzuführen. Ändern Sie hierzu die Umschaltoption **Self-Service-Kennwortzurücksetzung aktiviert** von **Jeder** in **Eine Gruppe**, und wählen Sie eine Sicherheitsgruppe aus, die für die Kennwortzurücksetzung aktiviert werden soll. Allen Mitgliedern dieser Gruppe müssen Lizenzen zugewiesen sein. Dies ist eine gute Gelegenheit, um die [gruppenbasierte Lizenzierung](active-directory-passwords-licensing.md#enable-group-or-user-based-licensing) umzusetzen.
6. Geben Sie basierend auf Ihrer Richtlinie die mindestens erforderlichen [Authentifizierungsdaten](active-directory-passwords-data.md) ein.
7. Schulen Sie Ihre Benutzer in der Nutzung von SSPR, indem Sie ihnen eine Anleitung senden, in der die Registrierung und Zurücksetzung beschrieben ist.
    > [!NOTE]
    > Testen Sie SSPR nicht als Administrator, sondern als normaler Benutzer, da Microsoft für Azure-Administratorkonten strenge Anforderungen an die Authentifizierung stellt. Weitere Informationen zur Richtlinie für Administratorkennwörter finden Sie in [diesem ausführlichen Artikel](active-directory-passwords-how-it-works.md).

8. Sie können einen beliebigen Zeitpunkt für das Erzwingen der Registrierung angeben und außerdem festlegen, dass Benutzer ihre Authentifizierungsinformationen nach einem bestimmten Zeitraum neu bestätigen müssen. Falls Sie nicht möchten, dass Benutzer sich registrieren müssen, können Sie die [Kennwortzurücksetzung bereitstellen, ohne dass eine Registrierung von Endbenutzern erforderlich ist](active-directory-passwords-data.md).
9. Sie können den Verlauf der Registrierung und Nutzung durch die Benutzer verfolgen, indem Sie die [Berichterstellungsfunktion von Azure AD](active-directory-passwords-reporting.md) verwenden.

## <a name="email-based-rollout"></a>E-Mail-basierte Einführung

Viele Kunden sind der Meinung, dass eine E-Mail-Kampagne mit einfachen Anweisungen der einfachste Weg ist, um Benutzer über die Nutzung von SSPR zu informieren. [Wir haben drei einfache E-Mails erstellt, die Sie beim Rollout als Vorlagen verwenden können:](https://onedrive.live.com/?authkey=%21AD5ZP%2D8RyJ2Cc6M&id=A0B59A91C740AB16%2125063&cid=A0B59A91C740AB16)

* E-Mail-Vorlage **Coming Soon** (In Kürze verfügbar): Kann in den Wochen bzw. Tagen vor dem Rollout eingesetzt werden, um die Benutzer darüber zu informieren, dass bald eine Aufgabe erledigt werden muss.
* E-Mail-Vorlage **Available Now** (Jetzt verfügbar): Kann am Tag der Einführung eingesetzt werden, um Benutzer zum Registrieren und Bestätigen ihrer Authentifizierungsdaten zu bewegen, damit sie SSPR bei Bedarf nutzen können.
* E-Mail-Vorlage **Sign up Reminder** (Erinnerung an Registrierung): Kann einige Tage bzw. Wochen nach der Bereitstellung eingesetzt werden, um Benutzer an das Registrieren und Bestätigen ihrer Authentifizierungsdaten zu erinnern.

## <a name="creating-your-own-password-portal"></a>Erstellen eines eigenen Kennwortportals

Viele der größeren Microsoft-Kunden entscheiden sich für das Hosten einer Webseite und das Erstellen eines DNS-Stammeintrags, z.B. „https://passwords.contoso.com“. Sie fügen auf dieser Seite Links hinzu, die zu folgenden Ressourcen führen: Azure AD-Kennwortzurücksetzung, Registrierung für die Kennwortzurücksetzung, Portale für die Kennwortänderung und andere organisationsbezogene Informationen. Auf diese Weise können Sie in der E-Mail-Korrespondenz oder auf ausgegebenen Flyern dann immer eine einheitliche URL angeben, unter der Benutzer alle Informationen zur Nutzung der Dienste finden.

* Portal für die Kennwortzurücksetzung: https://passwordreset.microsoftonline.com/
* Portal zur Registrierung für die Kennwortzurücksetzung: http://aka.ms/ssprsetup
* Portal für die Änderung des Kennworts: https://account.activedirectory.windowsazure.com/ChangePassword.aspx

## <a name="using-enforced-registration"></a>Verwenden der erzwungenen Registrierung

Wenn Sie möchten, dass sich Ihre Benutzer für die Kennwortzurücksetzung registrieren, können Sie die Registrierung bei der Anmeldung an Azure AD erzwingen. Sie können diese Option auf dem Blatt **Kennwortzurücksetzung** Ihres Verzeichnisses aktivieren, indem Sie auf der Registerkarte **Registrierung** die Option **Registrierung von Benutzern bei der Anmeldung verlangen?** aktivieren.

Administratoren können vorgeben, dass sich Benutzer nach einem bestimmten Zeitraum erneut registrieren müssen, indem sie die Option **Anzahl der Tage, bevor Benutzer aufgefordert werden, ihre Authentifizierungsinformationen erneut zu bestätigen** auf einen Wert von 0 bis 730 Tagen festlegen.

Nachdem Sie diese Option aktiviert haben, wird Benutzern bei der Anmeldung eine Meldung mit dem Hinweis angezeigt, dass vom Administrator die Bestätigung ihrer Authentifizierungsinformationen vorgegeben wurde.

## <a name="populate-authentication-data"></a>Auffüllen von Authentifizierungsdaten

Wenn Sie [Authentifizierungsdaten für Ihre Benutzer vorab einfügen](active-directory-passwords-data.md), müssen sich Benutzer nicht für die Kennwortzurücksetzung registrieren, bevor sie SSPR nutzen können. Solange für die Benutzer Authentifizierungsdaten definiert sind, die der von Ihnen definierten Richtlinie für die Kennwortzurücksetzung entsprechen, können Benutzer ihre Kennwörter zurücksetzen.

## <a name="disabling-self-service-password-reset"></a>Deaktivieren der Self-Service-Kennwortzurücksetzung

Das Deaktivieren der Self-Service-Kennwortzurücksetzung ist einfach. Öffnen Sie Ihren Azure AD-Mandanten, und navigieren Sie zu **Kennwortzurücksetzung** > **Eigenschaften**, und wählen Sie unter **Self-Service-Kennwortzurücksetzung aktiviert** die Option **Niemand**.

## <a name="next-steps"></a>Nächste Schritte

Die folgenden Links führen zu weiteren Informationen zur Kennwortzurücksetzung mit Azure AD:

* [**Schnellstart:**](active-directory-passwords-getting-started.md) Informieren Sie sich schnell über die Self-Service-Kennwortverwaltung von Azure AD. 
* [**Lizenzierung:**](active-directory-passwords-licensing.md) Konfigurieren Sie Ihre Azure AD-Lizenzierung.
* [**Daten:**](active-directory-passwords-data.md) Erfahren Sie, welche Daten erforderlich sind und wie sie für die Kennwortverwaltung verwendet werden.
* [**Anpassung:**](active-directory-passwords-customize.md) Passen Sie das Aussehen und Verhalten von SSPR für Ihr Unternehmen an.
* [**Richtlinie:**](active-directory-passwords-policy.md) Machen Sie sich mit den Azure AD-Kennwortrichtlinien vertraut, und legen Sie sie fest.
* [**Kennwortrückschreiben:**](active-directory-passwords-writeback.md) Hier wird die Funktionsweise des Kennwortrückschreibens in Ihrem lokalen Verzeichnis beschrieben.
* [**Berichterstellung:**](active-directory-passwords-reporting.md) Ermitteln Sie, ob, wann und wo Ihre Benutzer auf SSPR-Funktionen zugreifen.
* [**Ausführliche technische Informationen:**](active-directory-passwords-how-it-works.md) Steigen Sie tiefer ein, um zu verstehen, wie alles funktioniert.
* [**Häufig gestellte Fragen (FAQ):**](active-directory-passwords-faq.md) Wie? Warum? Was? Wo? Wer? Wann? - Antworten auf Fragen, die Sie schon immer stellen wollten
* [**Problembehandlung:**](active-directory-passwords-troubleshoot.md) Erfahren Sie, wie Sie häufig auftretende Probleme bei SSPR beheben.
