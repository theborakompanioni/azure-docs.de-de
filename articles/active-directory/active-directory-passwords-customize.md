---
title: 'Anpassen: Azure AD SSPR | Microsoft-Dokumentation'
description: "Anpassungsoptionen für die Self-Service-Kennwortzurücksetzung in Azure AD"
services: active-directory
keywords: 
documentationcenter: 
author: MicrosoftGuyJFlo
manager: femila
ms.reviewer: gahug
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/17/2017
ms.author: joflore
ms.custom: it-pro
ms.translationtype: HT
ms.sourcegitcommit: 94d1d4c243bede354ae3deba7fbf5da0652567cb
ms.openlocfilehash: ee58d0c6703d7e6688ba9959a7f75c3b52a9411c
ms.contentlocale: de-de
ms.lasthandoff: 07/18/2017

---
# <a name="customize-azure-ad-functionality-for-self-service-password-reset"></a>Anpassen von Azure AD-Funktionen für die Self-Service-Kennwortzurücksetzung

IT-Experten, die die Self-Service-Kennwortzurücksetzung bereitstellen möchten, können diese an ihre Benutzer anpassen.

## <a name="customize-the-contact-your-administrator-link"></a>Anpassen des Kontakts Ihres Administratorlinks

Auch wenn SSPR nicht aktiviert ist, wird den Benutzern im Portal für die Kennwortzurücksetzung ein Link „Wenden Sie sich an Ihren Administrator“ angezeigt.  Durch Klicken auf diesen Link werden Ihre Administratoren per E-Mail um Unterstützung beim Ändern des Kennworts des Benutzers gebeten. Diese E-Mail wird in der folgenden Reihenfolge an die folgenden Empfänger gesendet:

1. Wenn die Rolle **Kennwortadministrator** zugewiesen ist, werden die Administratoren mit dieser Rolle benachrichtigt
2. Falls keine Kennwortadministratoren zugewiesen sind, werden die Administratoren mit der Rolle **Benutzeradministrator** benachrichtigt
3. Wenn keine der obigen Rollen zugewiesen ist, werden **globale Administratoren** benachrichtigt.

In jedem Fall werden bis zu 100 Empfänger benachrichtigt.

Weitere Informationen zu den verschiedenen Administratorrollen und wie sie zugewiesen werden können finden Sie unter [Assigning administrator roles in Azure Active Directory (Zuweisen von Administratorrollen in Azure Active Directory)](active-directory-assign-admin-roles.md)

### <a name="disable-contact-your-administrator-emails"></a>Deaktivieren von „Wenden Sie sich an Ihren Administrator“-E-Mails

Wenn Ihre Organisation nicht möchte, dass Administratoren über Anforderungen zur Kennwortzurücksetzung benachrichtigt werden, kann die folgende Konfiguration aktiviert werden:

* Aktivieren Sie die Self-Service-Kennwortzurücksetzung für alle Endbenutzer. Diese Option finden Sie unter **Password Reset > Properties** (Kennwortzurücksetzung, Eigenschaften).
    * Wenn Sie nicht möchten, dass Benutzer ihre eigenen Kennwörter zurücksetzen, können Sie den Zugriff auf eine leere Gruppe beschränken **diese Option ist nicht empfehlenswert**.
* Passen Sie den Helpdesklink an, um eine Web-URL oder eine mailto:-Adresse bereitzustellen, die Benutzer verwenden können, um Unterstützung zu erhalten. Diese Option finden Sie unter **Password Reset > Customization > Custom helpdesk email or URL** (Kennwortzurücksetzung, Anpassung, Benutzerdefinierte Helpdesk-E-Mail oder -URL).

## <a name="customize-adfs-sign-in-page-for-sspr"></a>Anpassen der ADFS-Anmeldeseite für SSPR

ADFS-Administratoren können einen Link zu ihrer Anmeldeseite hinzufügen, indem sie die Anleitungen im Artikel [Add sign-in page description (Hinzufügen von Anmeldeseitenbeschreibung)](https://docs.microsoft.com/windows-server/identity/ad-fs/operations/add-sign-in-page-description) befolgen.

Wenn Sie den folgenden Befehl auf Ihren ADFS-Server anwenden, wird ein Link zu Ihrer ADFS-Anmeldeseite hinzugefügt, der es Benutzern erlaubt, direkt zum Workflow für die Self-Service-Kennwortzurücksetzung zu gelangen.

``` Set-ADFSGlobalWebContent -SigninPageDescriptionText "<p><A href=’https://passwordreset.microsoftonline.com’>Can’t access your account?<A/></p>" ```

## <a name="customize-the-sign-in-and-access-panel-look-and-feel"></a>Anpassen des Aussehens und Verhaltens des Anmelde- und Zugriffsbereichs

Wenn Ihre Benutzer auf die Anmeldeseite zugreifen, können Sie das Logo anpassen, das zusammen mit dem Bild für Anmeldeseite angezeigt wird, sodass es Ihrem Unternehmensbranding entspricht.

Diese Grafiken werden in den folgenden Situationen angezeigt:

* Nachdem ein Benutzer den Benutzernamen eingegeben hat
* Ein Benutzer greift auf die angepasste URL zu
    * Durch Übergeben des whr-Parameters an die Seite zum Zurücksetzen des Kennworts, z.B. „https://login.microsoftonline.com/?whr=contoso.com“
    * Durch Übergeben des username-Parameters an die Seite zum Zurücksetzen des Kennworts, z.B. „https://login.microsoftonline.com/?username=admin@contoso.com“

### <a name="graphics-details"></a>Grafikdetails

Mit den folgenden Einstellungen können Sie die visuellen Merkmale der Anmeldeseite ändern. Sie finden sie unter **Azure Active Directory**, **Unternehmensbranding**, **Unternehmensbranding bearbeiten**.

* Das Bild für die Anmeldeseite muss eine PNG- oder JPG-Datei mit 1420 x 1200 Pixel und maximal 500 KB sein. Optimale Ergebnisse erzielen Sie mit etwa 200 KB.
* Die Hintergrundfarbe für die Anmeldeseite wird für Verbindungen mit hoher Latenzzeit verwendet, und sie muss das RGB-Hexadezimalformat aufweisen.
* Das Bannerbild muss eine PNG- oder JPG-Datei mit 60 x 280 Pixel und maximal 10 KB sein.
* Die PNG- oder JPG-Datei mit dem quadratischen Logo (normales und dunkles Design) muss 240 x 240 Pixel (änderbar) und maximal 10 KB aufweisen.

### <a name="sign-in-text-options"></a>Optionen für den Anmeldetext

Mit den folgenden Einstellungen können Sie Text zur Anmeldeseite hinzufügen, der für Ihre Organisation relevant ist. Diese Einstellungen finden Sie unter **Azure Active Directory**, **Unternehmensbranding**, **Unternehmensbranding bearbeiten**.

* **Benutzernamenhinweis** ersetzt den Beispieltext someone@example.com mit etwas, das für Ihre Benutzer besser geeignet ist. Für interne und externe Benutzer wird die Standardeinstellung empfohlen.
* **Text für die Anmeldeseite** ist maximal 256 Zeichen lang. Dieser Text wird überall angezeigt, wo sich Ihre Benutzer online anmelden, und in Azure AD Join unter Windows 10. Verwenden Sie diesen Text für Nutzungsbedingungen, Hinweise und Tipps für Ihre Benutzer. **Alle können die Anmeldeseite sehen. Stellen Sie hier also keine vertraulichen Informationen bereit.**

### <a name="keep-me-signed-in-disabled"></a>„Angemeldet bleiben“ deaktiviert

Die Option „"Angemeldet bleiben" deaktiviert“ gibt den Benutzern die Möglichkeit, angemeldet zu bleiben, wenn sie ihr Browserfenster schließen und erneut öffnen. Diese Option wirkt sich nicht auf Lebensdauer der Sitzung aus. Diese Einstellung finden Sie unter **Azure Active Directory > Company branding > Edit company branding (Azure Active Directory, Unternehmensbranding, Unternehmensbranding bearbeiten)**.

Einige Features von SharePoint Online und Office 2010 setzen voraus, dass dieses Kontrollkästchen von den Benutzern aktiviert werden kann. Wenn Sie diese Option ausblenden, werden Benutzer möglicherweise zusätzlich und unerwartet zur Anmeldung aufgefordert.

### <a name="directory-name"></a>Verzeichnisname

Sie können das Namensattribut unter **Azure Active Directory > Properties (Azure Active Directory, Eigenschaften)** ändern, sodass ein verständlicher Organisationsname im Portal und in der automatisierten Kommunikation angezeigt wird. Diese Option wird besonders in automatisierten E-Mails in den folgenden Formen angezeigt:

* Anzeigename der E-Mail in der Demo „Microsoft im Namen von CONTOSO“
* Betreffzeile der E-Mail in der Demo „E-Mail-Prüfnummer für CONTOSO-Konto“

## <a name="next-steps"></a>Nächste Schritte

Die folgenden Links führen zu weiteren Informationen zur Kennwortzurücksetzung mit Azure AD:

* [**Schnellstart:**](active-directory-passwords-getting-started.md) Informieren Sie sich schnell über die Self-Service-Kennwortverwaltung von Azure AD. 
* [**Lizenzierung:**](active-directory-passwords-licensing.md) Konfigurieren Sie Ihre Azure AD-Lizenzierung.
* [**Daten:**](active-directory-passwords-data.md) Erfahren Sie, welche Daten erforderlich sind und wie sie für die Kennwortverwaltung verwendet werden.
* [**Rollout:**](active-directory-passwords-best-practices.md) Mithilfe der hier enthaltenen Anleitungen können Sie SSPR planen und für Ihre Benutzer bereitstellen.
* [**Richtlinie:**](active-directory-passwords-policy.md) Verstehen Sie Azure AD-Kennwortrichtlinien, und legen Sie sie fest.
* [**Kennwortrückschreiben:**](active-directory-passwords-writeback.md) Hier wird die Funktionsweise des Kennwortrückschreibens in Ihrem lokalen Verzeichnis beschrieben.
* [**Berichterstellung:**](active-directory-passwords-reporting.md) Ermitteln Sie, ob, wann und wo Ihre Benutzer auf SSPR-Funktionen zugreifen.
* [**Ausführliche technische Informationen:**](active-directory-passwords-how-it-works.md) Steigen Sie tiefer ein, um zu verstehen, wie alles funktioniert.
* [**Häufig gestellte Fragen (FAQ):**](active-directory-passwords-faq.md) Wie? Warum? Was? Wo? Wer? Wann? - Antworten auf Fragen, die Sie schon immer stellen wollten
* [**Problembehandlung:**](active-directory-passwords-troubleshoot.md) Erfahren Sie, wie Sie häufig auftretende Probleme bei SSPR beheben.


