---
title: 'Schnellstart: Azure AD SSPR | Microsoft-Dokumentation'
description: "Schnelle Bereitstellung der Self-Service-Kennwortzurücksetzung in Azure AD"
services: active-directory
keywords: 
documentationcenter: 
author: MicrosoftGuyJFlo
manager: femila
editor: gahug
ms.assetid: bde8799f-0b42-446a-ad95-7ebb374c3bec
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 05/12/2017
ms.author: joflore
ms.custom: it-pro
ms.translationtype: Human Translation
ms.sourcegitcommit: afa23b1395b8275e72048bd47fffcf38f9dcd334
ms.openlocfilehash: 5a338029cea5f448fc4d84a1a9c01c867ecd5028
ms.contentlocale: de-de
ms.lasthandoff: 05/12/2017


---
# <a name="quick-start-azure-ad-self-service-password-reset"></a>Schnellstart: Self-Service-Kennwortzurücksetzung in Azure AD

## <a name="rapidly-deploy-self-service-password-reset"></a>Schnelle Bereitstellung der Self-Service-Kennwortzurücksetzung

Die Self-Service-Kennwortzurücksetzung (Self-Service Password Reset, SSPR) ist für IT-Administratoren eine einfache Möglichkeit, Benutzern das Zurücksetzen oder Entsperren ihrer Kennwörter oder Konten zu erlauben. Das System verfügt über eine ausführliche Berichterstellung zur Nutzung des Systems durch Benutzer sowie über eine Benachrichtigungsfunktion, damit Sie über eine fehlerhafte oder missbräuchliche Nutzung informiert sind.

In dieser Anleitung wird vorausgesetzt, dass Sie bereits über einen funktionierenden Testmandanten oder einen lizenzierten Azure AD-Mandanten verfügen. Hilfe zur Einrichtung von Azure AD finden Sie im Artikel [Erste Schritte mit einem Azure-Abonnement](https://azure.microsoft.com/trial/get-started-active-directory/).

1. Wählen Sie unter Ihrem vorhandenen Azure AD-Mandanten die Option **Kennwortzurücksetzung**.

2. Wählen Sie in der Bildschirmansicht **Eigenschaften** unter „Self-Service-Kennwortzurücksetzung aktiviert“ eine der folgenden Optionen:
    * Niemand: Niemand kann die SSPR-Funktion nutzen.
    * Eine Gruppe: Nur Mitglieder einer bestimmten Azure AD-Gruppe, die Sie ausgewählt haben, können die SSPR-Funktion nutzen.
    * Jeder: Alle Benutzer mit Konten unter Ihrem Azure AD-Mandanten können die SSPR-Funktion nutzen.

3. Wählen Sie in der Bildschirmansicht **Authentifizierungsmethoden** Folgendes:
    * Anzahl von Methoden, die zurückgesetzt werden müssen: Hierfür werden eine oder zwei Methoden unterstützt.
    * Für Benutzer verfügbare Methoden: Mindestens eine Methode ist erforderlich, aber es kann nicht schaden, wenn noch mehr Auswahl besteht.
        * Mit der Option **E-Mail** wird eine E-Mail mit einem Code an die E-Mail-Adresse des Benutzers gesendet, die für die Authentifizierung konfiguriert ist.
        * Mit der Option **Mobiltelefon** können Benutzer einen Anruf oder eine SMS mit einem Code unter der konfigurierten Mobiltelefonnummer erhalten.
        * Bei der Option **Bürotelefon** erhalten Benutzer einen Anruf mit einem Code unter der konfigurierten Bürotelefonnummer.
        * Unter **Sicherheitsfragen** besteht die folgende Auswahl:
            * Anzahl der für die Registrierung erforderlichen Fragen: Gibt die Mindestanzahl an, die für eine erfolgreiche Registrierung erforderlich ist. Ein Benutzer kann also auch mehr Fragen beantworten, um einen Pool mit verfügbaren Fragen zu erstellen. Sie können für diese Option einen Wert von 3 bis 5 festlegen, und der Wert muss größer oder gleich der Anzahl von Fragen sein, die für das Zurücksetzen erforderlich sind.
            * Anzahl der für die Zurücksetzung erforderlichen Fragen: Sie können für diese Option einen Wert von 3 bis 5 Fragen festlegen, die richtig beantwortet werden müssen, bevor das Kennwort eines Benutzers zurückgesetzt oder entsperrt werden kann.
                * Sie können benutzerdefinierte Fragen hinzufügen, indem Sie beim Auswählen der Sicherheitsfragen auf die Schaltfläche „Benutzerdefiniert“ klicken.

4. EMPFOHLEN: Mit der Option **Anpassung** können Sie den Link „Wenden Sie sich an Ihren Administrator.“ so ändern, dass er auf eine von Ihnen definierte Seite oder E-Mail-Adresse verweist.

5. OPTIONAL: Die Bildschirmansicht **Registrierung** enthält die folgenden Optionen für Administratoren:
    * Sollen Benutzer sich bei der Anmeldung registrieren müssen?
    * Anzahl der Tage, bevor Benutzer aufgefordert werden, ihre Authentifizierungsinformationen erneut zu bestätigen

6. OPTIONAL: Für die Option **Benachrichtigung** werden die folgenden Optionen für Administratoren bereitgestellt:
    * Benutzer über Kennwortzurücksetzungen benachrichtigen?
    * Sollen alle Administratoren benachrichtigt werden, wenn andere Administratoren ihr Kennwort zurücksetzen?

**Sie haben SSPR jetzt für Ihren Azure AD-Mandanten konfiguriert**. Sie können hier aufhören oder weitermachen, um die Synchronisierung von Kennwörtern in einer lokalen AD-Domäne zu konfigurieren.

> [!NOTE]
> Testen Sie SSPR nicht als Administrator, sondern als normaler Benutzer, da Microsoft für Azure-Administratorkonten strenge Anforderungen an die Authentifizierung stellt. Weitere Informationen zur Richtlinie für Administratorkennwörter finden Sie im Artikel zu [Kennwortrichtlinien](active-directory-passwords-policy.md#administrator-password-policy-differences).

## <a name="configure-synchronization-to-existing-identity-source"></a>Konfigurieren der Synchronisierung mit der vorhandenen Identitätsquelle

Um die lokale Identitätssynchronisierung mit Azure AD zu aktivieren, müssen Sie [Azure AD Connect](./connect/active-directory-aadconnect.md) auf einem Server Ihrer Organisation installieren und konfigurieren. Mit dieser Anwendung wird die Synchronisierung von Benutzern und Gruppen von Ihrer vorhandenen Identitätsquelle mit Ihrer Azure AD-Domäne durchgeführt.

* [Aktualisieren der Windows Azure Active Directory-Synchronisierung und Azure Active Directory-Synchronisierung](./connect/active-directory-aadconnect-dirsync-deprecated.md)
* [Erste Schritte mit Azure AD Connect mit Expresseinstellungen](./connect/active-directory-aadconnect-get-started-express.md)
* [Konfigurieren Sie das Kennwortrückschreiben](active-directory-passwords-writeback.md#configuring-password-writeback), um Kennwörter aus Azure AD wieder in Ihr lokales Verzeichnis zu schreiben.

## <a name="disabling-self-service-password-reset"></a>Deaktivieren der Self-Service-Kennwortzurücksetzung

Das Deaktivieren der Self-Service-Kennwortzurücksetzung ist einfach. Öffnen Sie Ihren Azure AD-Mandanten, und navigieren Sie zu **Kennwortzurücksetzung** > **Eigenschaften**, und wählen Sie unter **Self-Service-Kennwortzurücksetzung aktiviert** die Option **Niemand**.

## <a name="next-steps"></a>Nächste Schritte
Die folgenden Links führen zu weiteren Informationen zur Kennwortzurücksetzung mit Azure AD:

* [**Lizenzierung:**](active-directory-passwords-licensing.md) Konfigurieren Sie Ihre Azure AD-Lizenzierung.
* [**Daten:**](active-directory-passwords-data.md) Erfahren Sie, welche Daten erforderlich sind und wie sie für die Kennwortverwaltung verwendet werden.
* [**Rollout:**](active-directory-passwords-best-practices.md) Mithilfe der hier enthaltenen Anleitungen können Sie SSPR planen und für Ihre Benutzer bereitstellen.
* [**Anpassung:**](active-directory-passwords-customize.md) Passen Sie das Aussehen und Verhalten von SSPR für Ihr Unternehmen an.
* [**Richtlinie:**](active-directory-passwords-policy.md) Machen Sie sich mit den Azure AD-Kennwortrichtlinien vertraut, und legen Sie sie fest.
* [**Berichterstellung:**](active-directory-passwords-reporting.md) Ermitteln Sie, ob, wann und wo Ihre Benutzer auf SSPR-Funktionen zugreifen.
* [**Ausführliche technische Informationen:**](active-directory-passwords-how-it-works.md) Steigen Sie tiefer ein, um zu verstehen, wie alles funktioniert.
* [**Häufig gestellte Fragen (FAQ):**](active-directory-passwords-faq.md) Wie? Warum? Was? Wo? Wer? Wann? - Antworten auf Fragen, die Sie schon immer stellen wollten
* [**Problembehandlung:**](active-directory-passwords-troubleshoot.md) Erfahren Sie, wie Sie häufig auftretende Probleme bei SSPR beheben.

