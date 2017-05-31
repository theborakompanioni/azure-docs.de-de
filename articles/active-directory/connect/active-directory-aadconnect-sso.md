---
title: 'Azure AD Connect: Nahtloses einmaliges Anmelden | Microsoft-Dokumentation'
description: "In diesem Thema wird die nahtlose einmalige Anmeldung von Azure Active Directory (Azure AD) beschrieben und wie Sie damit eine echte einmalige Anmeldung für Desktopbenutzer im Unternehmen innerhalb Ihres Unternehmensnetzwerks bereitstellen können."
services: active-directory
keywords: "Was ist Azure AD Connect, Active Directory installieren, erforderliche Komponenten für Azure AD, SSO, Single Sign-On, einmaliges Anmelden"
documentationcenter: 
author: swkrish
manager: femila
ms.assetid: 9f994aca-6088-40f5-b2cc-c753a4f41da7
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/08/2017
ms.author: billmath
ms.translationtype: Human Translation
ms.sourcegitcommit: 71fea4a41b2e3a60f2f610609a14372e678b7ec4
ms.openlocfilehash: 2601850f99188445cf63a6a4f185bdc4ebb92c29
ms.contentlocale: de-de
ms.lasthandoff: 05/10/2017

---

# <a name="azure-active-directory-seamless-single-sign-on"></a>Nahtlose einmalige Anmeldung mit Azure Active Directory

## <a name="what-is-azure-active-directory-seamless-single-sign-on"></a>Was ist die nahtlose einmalige Anmeldung mit Azure Active Directory?

Die nahtlose einmalige Anmeldung mit Azure Active Directory (Azure AD) bietet eine echte einmalige Anmeldung für Benutzer, die sich auf ihren Unternehmenscomputern, die mit dem Unternehmensnetzwerk verbunden sind, anmelden. Wenn diese Funktion aktiviert ist, müssen Benutzer zur Anmeldung bei Azure AD nicht ihre Kennwörter und in den meisten Fällen nicht einmal ihre Benutzernamen eingeben. Diese Funktion ermöglicht Ihren Benutzern einen einfachen Zugriff auf Ihre cloudbasierten Dienste, ohne dass zusätzliche lokale Komponenten erforderlich sind.

Die nahtlose einmalige Anmeldung kann über Azure AD Connect aktiviert und mit der [Kennwortsynchronisierung](active-directory-aadconnectsync-implement-password-synchronization.md) oder [Passthrough-Authentifizierung](active-directory-aadconnect-pass-through-authentication.md) kombiniert werden.

>[!NOTE]
>Diese Funktion ist NICHT auf Active Directory-Verbunddienste (AD FS) anwendbar, die diese Funktion bereits bereitstellen.

Damit diese Funktion bei einem bestimmten Benutzer funktioniert, müssen folgende Bedingungen erfüllt sein:

- Ihr Benutzer meldet sich auf einem Desktopcomputer des Unternehmens an.
- Der Desktop ist zuvor Ihrer Active Directory(AD)-Domäne beigetreten.
- Es muss eine direkte Verbindung zwischen dem Desktop und Ihrem Domänencontroller (DC) bestehen, entweder über das Unternehmensnetzwerk (Kabel- oder Funknetzwerk) oder per Remotezugriff, z.B. über eine VPN-Verbindung.
- Unsere Dienstendpunkte wurden zur Intranetzone des Browsers hinzugefügt.

Wenn eine der oben genannten Bedingungen nicht erfüllt wird, wird der Benutzer wie zuvor zur Eingabe seines Benutzernamens und Kennworts aufgefordert.

![Nahtloses einmaliges Anmelden](./media/active-directory-aadconnect-sso/sso1.png)

Die anderen Funktionen des nahtlosen einmaligen Anmeldens lauten wie folgt:

- Wenn eine (von einer Anwendung in Ihrem Mandanten ausgelöste) Azure AD-Anmeldeanforderung den Parameter `domain_hint` oder `login_hint` enthält, werden diese bei der nahtlosen einmaligen Anmeldung genutzt, sodass die Benutzer ihren Benutzernamen und ihr Kennwort nicht eingeben müssen.
- Nahtloses einmaliges Anmelden unterstützt Benutzernamen, bei denen es sich entweder um den lokalen Benutzernamen (in der Regel „userPrincipalName“) oder ein anderes (als „Alternative ID“ bezeichnetes) Attribut handeln kann, das in Azure AD Connect konfiguriert ist.
- Das nahtlose einmalige Anmelden ist ein opportunistisches Feature, d.h., wenn ein Fehler auftritt, wird auf die reguläre Benutzeranmeldung zurückgegriffen – die Benutzer müssen also ihr Kennwort auf der Anmeldeseite eingeben.

## <a name="whats-available-during-preview"></a>Was ist in der Preview-Version verfügbar?

>[!NOTE]
>Die nahtlose einmalige Anmeldung mit Azure AD ist aktuell in der Preview-Version verfügbar. Es handelt sich um eine kostenlose Funktion, sodass Sie für deren Verwendung keine kostenpflichtigen Versionen von Azure AD benötigen.

Die nahtlose einmalige Anmeldung ist auf browserbasierten Clients und Office-Clients möglich, die [moderne Authentifizierungsmechanismen](https://aka.ms/modernauthga) auf Desktops unterstützen, die eine Kerberos-Authentifizierung z.B. auf Windows-basierten Computern ausführen können. Die folgende Matrix zeigt Informationen zu den browserbasierten Clients unter verschiedenen Betriebssystemen:

| Betriebssystem/Browser |Internet Explorer|Google Chrome|Mozilla Firefox|Edge
| --- | --- |--- | --- | --- |
|Windows 10|Ja|Ja|Ja\*|Nein
|Windows 8.1|Ja|Ja|Ja\*|N/V
|Windows 8|Ja|Ja|Ja\*|N/V
|Windows 7|Ja|Ja|Ja\*|N/V
|Mac OS X|N/V|Ja\*|Ja\*|N/V

\*Erfordert zusätzliche Konfigurationsschritte.

>[!NOTE]
>Bei Windows 10 wird empfohlen, [Azure AD Join](../active-directory-azureadjoin-overview.md) zu verwenden, um eine optimale Funktionsweise von Azure AD sicherzustellen.

## <a name="how-does-azure-ad-seamless-sso-work"></a>Wie funktioniert die nahtlose einmalige Anmeldung mit Azure AD?

Sie können die nahtlose einmalige Anmeldung mit Azure AD Connect wie [nachfolgend](#how-to-enable-azure-ad-seamless-sso?) gezeigt aktivieren. Wenn diese Funktion aktiviert ist, wird in Ihrem lokalen Active Directory (AD) ein Computerkonto namens AZUREADSSOACCT erstellt, und der Kerberos-Entschlüsselungsschlüssel wird sicher in Azure AD freigegeben. Darüber hinaus werden zwei Kerberos-Dienstprinzipalnamen (SPNs) erstellt, um die zwei Dienst-URLs darzustellen, die während der Azure AD-Anmeldung verwendet werden.

>[!NOTE]
> Für jede AD-Gesamtstruktur, die Sie mit Azure AD (über Azure AD Connect) synchronisieren und für deren Benutzer Sie die nahtlose einmalige Anmeldung aktivieren möchten, müssen das Computerkonto und die Kerberos-SPNs erstellt werden. Verfügt Ihre Active Directory-Gesamtstruktur über Organisationseinheiten (OUs), müssen Sie nach der Aktivierung der nahtlosen einmaligen Anmeldung das Computerkonto AZUREADSSOACCT in eine Organisationseinheit verschieben, um dafür zu sorgen, dass es nicht gelöscht und wie andere Computerkonten verwaltet wird.

Nach Abschluss der Einrichtung funktioniert die Azure AD-Anmeldung auf dieselbe Weise wie jede andere Anmeldung, die die integrierte Windows-Authentifizierung (IWA) verwendet. Der Prozess zur nahtlosen einmaligen Anmeldung funktioniert wie folgt:

Nehmen wir an, Ihr Benutzer versucht, auf eine durch Azure AD geschützte cloudbasierte Ressource zuzugreifen, wie etwa SharePoint Online. SharePoint Online leitet den Browser des Benutzers zur Anmeldung an Azure AD um.

Wenn die Anmeldeanforderung bei Azure AD einen `domain_hint`-Parameter (identifiziert Ihren Azure AD-Mandanten, z.B. contoso.onmicrosoft.com) oder einen `login_hint`-Parameter (identifiziert den Benutzernamen des Benutzers, z.B. user@contoso.onmicrosoft.com oder user@contoso.com) enthält, führen Sie die Schritte 1–5 durch.

Wenn einer dieser beiden Parameter nicht in der Anforderung enthalten ist, wird der Benutzer auf der Azure AD-Anmeldeseite zur Eingabe seines Benutzernamens aufgefordert. Die Schritte 1 bis 5 werden erst ausgeführt, nachdem der Benutzer den Fokus vom Feld „Benutzername“ entfernt oder auf die Schaltfläche „Weiter“ klickt.

1. Azure AD fordert über eine „401 – Nicht autorisiert“-Antwort den Client auf, ein Kerberos-Ticket bereitzustellen.
2. Der Client fordert ein Ticket von Active Directory für Azure AD an (dargestellt vom Computerkonto, das zuvor eingerichtet wurde).
3. Active Directory sucht das Computerkonto und gibt ein Kerberos-Ticket an den Client zurück, das mit dem Geheimnis des Computerkontos verschlüsselt ist. Das Ticket enthält die Identität des Benutzers, der zurzeit beim Desktop angemeldet ist.
4. Der Client sendet das von Active Directory abgerufene Kerberos-Ticket an Azure AD.
5. Azure AD entschlüsselt das Kerberos-Ticket mit dem zuvor freigegebenen Schlüssel. Wenn der Vorgang erfolgreich durchgeführt wurde, gibt Azure AD ein Token zurück oder fordert den Benutzer auf, zusätzliche Nachweise z.B. per Multi-Factor Authentication bereitzustellen – je nachdem, was die Ressource erfordert.

Das nahtlose einmalige Anmelden ist ein opportunistisches Feature, d.h., wenn ein Fehler auftritt, wird auf die reguläre Benutzeranmeldung zurückgegriffen – die Benutzer müssen also ihr Kennwort auf der Anmeldeseite eingeben.

Dieser Prozess wird auch in der Abbildung dargestellt:

![Nahtloses einmaliges Anmelden](./media/active-directory-aadconnect-sso/sso2.png)

## <a name="how-to-enable-azure-ad-seamless-sso"></a>Wie wird die nahtlose einmalige Anmeldung mit Azure AD aktiviert?

### <a name="pre-requisites"></a>Voraussetzungen

Wenn Sie die nahtlose einmalige Anmeldung mit Passthrough-Authentifizierung aktivieren, sind neben den Voraussetzungen für die Passthrough-Authentifizierung keine zusätzlichen Voraussetzungen zu erfüllen.

Wenn Sie die nahtlose einmalige Anmeldung mit Kennwortsynchronisierung aktivieren und es eine Firewall zwischen Azure AD Connect und Azure AD gibt, stellen Sie Folgendes sicher:

- Der Azure AD Connect-Server kann mit `*.msappproxy.net`-URLs kommunizieren.
- Azure AD Connect (Version 1.1.484.0 oder höher) kann über den Port 443 HTTPS-Anforderungen an Azure AD senden. Dieser wird nur zur Aktivierung des Features verwendet, nicht für die eigentlichen Benutzeranmeldungen.
- Azure AD Connect kann auch direkte IP-Verbindungen zu den [IP-Bereichen des Azure-Rechenzentrums](https://www.microsoft.com/download/details.aspx?id=41653) herstellen. Auch dies wird nur zum Aktivieren des Features verwendet.

>[!NOTE]
> Ältere Versionen von Azure AD Connect (vor 1.1.484.0) müssen über Port 9090 mit Azure AD kommunizieren können.

### <a name="enabling-the-azure-ad-seamless-sso-feature"></a>Aktivieren der nahtlosen einmaligen Anmeldefunktion mit Azure AD

Die nahtlose einmalige Anmeldung mit Azure AD kann über Azure AD Connect aktiviert werden.

Wenn Sie eine neue Installation von Azure AD Connect durchführen, wählen Sie den [benutzerdefinierten Installationspfad](active-directory-aadconnect-get-started-custom.md). Aktivieren Sie auf der Seite „Benutzeranmeldung“ die Option „Einmaliges Anmelden aktivieren“.

![Azure AD Connect – Benutzeranmeldung](./media/active-directory-aadconnect-sso/sso8.png)

Wenn bereits eine Installation von Azure AD Connect vorhanden ist, richten Sie den [Expressinstallations-](active-directory-aadconnect-get-started-express.md) oder [benutzerdefinierten Installationspfad](active-directory-aadconnect-get-started-custom.md) ein, wählen Sie in Azure AD Connect „Benutzeranmeldeseite ändern“ und klicken Sie auf „Weiter“. Aktivieren Sie dann die Option „Einmaliges Anmelden aktivieren“.

![Azure AD Connect – Benutzeranmeldung ändern](./media/active-directory-aadconnect-user-signin/changeusersignin.png)

Fahren Sie mit dem Assistenten fort, bis Sie zur Seite „Einmaliges Anmelden aktivieren“ gelangen. Für jede AD-Gesamtstruktur, die Sie mit Azure AD (über Azure AD Connect) synchronisieren und für deren Benutzer Sie die nahtlose einmalige Anmeldung aktivieren möchten, müssen Sie die Anmeldeinformationen des Domänenadministrators angeben. Beachten Sie, dass die Anmeldeinformationen des Domänenadministrators nicht in Azure AD Connect oder Azure AD gespeichert werden, sondern nur zur Erstellung des Computerkontos und zur Konfiguration der Kerberos-SPNs verwendet werden, wie zuvor beschrieben wurde.

Nach Abschluss des Assistenten ist nahtloses einmaliges Anmelden für Ihren Mandanten aktiviert. Beachten Sie, dass Sie erst die Schritte im nächsten Abschnitt durchführen müssen, bevor Ihre Benutzer von dieser Funktion profitieren können.

## <a name="rolling-the-feature-out-to-your-users"></a>Bereitstellen der Funktion für Ihre Benutzer

Wenn Sie das Feature zum nahtlosen einmaligen Anmelden für Ihre Benutzer einführen möchten, müssen Sie der Intranetzone der Benutzer über Gruppenrichtlinien in Active Directory zwei Azure AD-URLs (https://autologon.microsoftazuread-sso.com und https://aadg.windows.net.nsatc.net) hinzufügen. Beachten Sie, dass dies nur für Internet Explorer und Google Chrome (wenn der gleiche Satz von URLs vertrauenswürdiger Websites wie von Internet Explorer verwendet wird) funktioniert. Mozilla Firefox müssen Sie separat konfigurieren.

### <a name="why-do-you-need-this"></a>Warum benötigen Sie das?

Standardmäßig senden Browser Kerberos-Tickets nur an einen Cloudendpunkt, wenn die jeweilige URL als Teil der Intranetzone des Browsers definiert ist. Der Browser berechnet anhand der URL automatisch die richtige Zone (Internet oder Intranet). Beispielsweise wird http://contoso/ der Intranetzone und http://intranet.contoso.com/ der Internetzone zugeordnet (da die URL einen Punkt enthält).

Da die für die nahtlose einmalige Anmeldung verwendeten Azure AD-URLs einen Punkt enthalten, müssen sie explizit den Einstellungen jedes Browsers für Intranetzonen hinzugefügt werden. Dies bewirkt, dass der Browser die Kerberos-Tickets des aktuell angemeldeten Benutzers automatisch an Azure AD sendet. Sie können diese Einstellung zwar auf jedem Computer manuell vornehmen, doch die einfachste Möglichkeit, die erforderlichen URLs der Intranetzone für alle Benutzer hinzuzufügen, besteht darin, eine Gruppenrichtlinie in Active Directory zu erstellen.

### <a name="detailed-steps"></a>Ausführliche Schritte

1. Öffnen Sie das Gruppenrichtlinienverwaltungstool.
2. Bearbeiten Sie die Gruppenrichtlinie, die auf alle Benutzer angewendet wird, beispielsweise die **Standarddomänenrichtlinie**.
3. Navigieren Sie zu **Benutzerkonfiguration\Administrative Vorlagen\Windows-Komponenten\Internet Explorer\Internetsystemsteuerung\Sicherheitsseite**, und wählen Sie die Option **Liste der Site zu Zonenzuweisungen**.
![Einmaliges Anmelden](./media/active-directory-aadconnect-sso/sso6.png)  
4. Aktivieren Sie die Richtlinie, und geben Sie die folgenden Werte bzw. Daten in das Dialogfeld ein. Hierbei handelt es sich um die Azure AD-URLs, an die die Kerberos-Tickets gesendet werden.

        Value: https://autologon.microsoftazuread-sso.com
        Data: 1
        Value: https://aadg.windows.net.nsatc.net
        Data: 1
5. Klicken Sie zweimal nacheinander auf **OK******.

Diese sollte wie folgt aussehen:

![Einmaliges Anmelden](./media/active-directory-aadconnect-sso/sso7.png)

>[!NOTE]
>Standardmäßig verwendet Chrome den gleichen Satz vertrauenswürdiger Website-URLs wie Internet Explorer. Wenn Sie andere Einstellungen für Chrome konfiguriert haben, müssen Sie diese Einstellungen separat aktualisieren.

## <a name="disabling-azure-ad-seamless-sso"></a>Deaktivieren des nahtlose einmaligen Anmeldens mit Azure AD

Nahtloses einmaliges Anmelden mit Azure AD kann über Azure AD Connect deaktiviert werden.

Führen Sie Azure AD Connect aus, wählen Sie „Benutzeranmeldung ändern“, und klicken Sie auf „Weiter“. Deaktivieren Sie dann die Option „Einmaliges Anmelden aktivieren“. Setzen Sie den Assistenten fort. Nach Abschluss des Assistenten ist nahtloses einmaliges Anmelden für Ihren Mandanten deaktiviert. Allerdings wird auf dem Bildschirm die folgende Meldung angezeigt:

„Das einmalige Anmelden (SSO) ist jetzt deaktiviert, aber zur Bereinigung müssen manuell zusätzliche Schritte ausgeführt werden. Weitere Informationen“

Diese manuellen Schritte müssen ausgeführt werden:

- Rufen Sie die Liste der AD-Gesamtstrukturen ab, für die nahtloses einmaliges Anmelden aktiviert wurde.
  - Rufen Sie in PowerShell `New-AzureADSSOAuthenticationContext` auf. Ein Fenster sollte eingeblendet werden, in das Sie Ihre Anmeldeinformationen als Azure AD-Mandantenadministrator eingeben.
  - Rufen Sie `Get-AzureADSSOStatus` auf. Dadurch erhalten Sie die Liste der AD-Gesamtstrukturen (siehe die Liste „Domänen“), in denen dieses Feature aktiviert ist.
- Löschen Sie das Computerkonto AZUREADSSOACCT manuell aus jeder AD-Gesamtstruktur, die Sie in der Liste finden.

## <a name="next-steps"></a>Nächste Schritte

- Lesen Sie unsere [Anleitung zur Problembehandlung](active-directory-aadconnect-troubleshoot-sso.md), um zu erfahren, wie Sie häufig auftretende Probleme mit nahtlosem einmaligen Anmelden mit Azure AD beheben.

## <a name="feedback"></a>Feedback

Ihr Feedback ist uns sehr wichtig. Geben Sie etwaige Fragen in den nachstehenden Abschnitt „Kommentare“ ein. Nutzen Sie unser [UserVoice-Forum](https://feedback.azure.com/forums/169401-azure-active-directory/category/160611-directory-synchronization-aad-connect), um sich neue Features zu wünschen.

