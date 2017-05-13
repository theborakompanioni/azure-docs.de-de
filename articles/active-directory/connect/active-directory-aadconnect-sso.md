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
ms.date: 04/26/2017
ms.author: billmath
ms.translationtype: Human Translation
ms.sourcegitcommit: aaf97d26c982c1592230096588e0b0c3ee516a73
ms.openlocfilehash: b3eebdd714b38ffd9432404944829d05ef3c3dc6
ms.contentlocale: de-de
ms.lasthandoff: 04/27/2017

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

Wenn eine Azure AD-Anmeldeanforderung die Parameter `domain_hint` oder `login_hint` enthält (initiiert von einer Anwendung in Ihrem Mandanten), werden diese bei der nahtlosen einmaligen Anmeldung genutzt, sodass die Benutzer ihren Benutzernamen und ihr Kennwort nicht eingeben müssen.

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
- Azure AD Connect (Version 1.1.484.0 oder höher) kann über den Port 443 HTTPS-Anforderungen an Azure AD senden. Dieser wird nur zur Aktivierung der Funktion verwendet, nicht für die eigentlichen Benutzeranmeldungen.
- Azure AD Connect kann auch direkte IP-Verbindungen zu den [IP-Bereichen des Azure-Rechenzentrums](https://www.microsoft.com/en-us/download/details.aspx?id=41653) herstellen. Auch diese Funktion wird nur für das Aktivieren des Features verwendet.

>[!NOTE]
> Ältere Versionen von Azure AD Connect (vor 1.1.484.0) müssen über Port 9090 mit Azure AD kommunizieren können.

### <a name="enabling-the-azure-ad-seamless-sso-feature"></a>Aktivieren der nahtlosen einmaligen Anmeldefunktion mit Azure AD

Die nahtlose einmalige Anmeldung mit Azure AD kann über Azure AD Connect aktiviert werden.

Wenn Sie eine neue Installation von Azure AD Connect durchführen, wählen Sie den [benutzerdefinierten Installationspfad](active-directory-aadconnect-get-started-custom.md). Aktivieren Sie auf der Seite „Benutzeranmeldung“ die Option „Einmaliges Anmelden aktivieren“.

![Azure AD Connect – Benutzeranmeldung](./media/active-directory-aadconnect-sso/sso8.png)

Wenn bereits eine Installation von Azure AD Connect vorhanden ist, richten Sie den [Expressinstallations-](active-directory-aadconnect-get-started-express.md) oder [benutzerdefinierten Installationspfad](active-directory-aadconnect-get-started-custom.md) ein, wählen Sie in Azure AD Connect „Benutzeranmeldeseite ändern“ und klicken Sie auf „Weiter“. Aktivieren Sie dann die Option „Einmaliges Anmelden aktivieren“.

![Azure AD Connect – Benutzeranmeldung ändern](./media/active-directory-aadconnect-user-signin/changeusersignin.png)

Fahren Sie mit dem Installations-Assistenten fort, bis Sie zu der Seite „Einmaliges Anmelden aktivieren“ gelangen. Für jede AD-Gesamtstruktur, die Sie mit Azure AD (über Azure AD Connect) synchronisieren und für deren Benutzer Sie die nahtlose einmalige Anmeldung aktivieren möchten, müssen Sie die Anmeldeinformationen des Domänenadministrators angeben. Beachten Sie, dass die Anmeldeinformationen des Domänenadministrators nicht in Azure AD Connect oder Azure AD gespeichert werden, sondern nur zur Erstellung des Computerkontos und zur Konfiguration der Kerberos-SPNs verwendet werden, wie zuvor beschrieben wurde.

Nun ist die nahtlose einmalige Anmeldung für Ihren Mandanten aktiviert. Beachten Sie, dass Sie erst die Schritte im nächsten Abschnitt durchführen müssen, bevor Ihre Benutzer von dieser Funktion profitieren können.

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

## <a name="troubleshooting-seamless-sso"></a>Behebung von Problemen in Bezug auf die nahtlose einmalige Anmeldung

Verwenden Sie die folgende Checkliste zur Behebung von Problemen in Bezug auf die nahtlose einmalige Anmeldung:

1. Überprüfen Sie, ob die Funktion zur nahtlosen einmaligen Anmeldung für Ihren Mandanten im Azure AD Connect-Tool aktiviert ist. Wenn Sie die Funktion (z.B. aufgrund eines blockierten Ports) nicht aktivieren können, vergewissern Sie sich, dass Sie alle [Voraussetzungen](#pre-requisites) erfüllen. Wenn die Probleme in Verbindung mit der Aktivierung der Funktion weiterhin bestehen, wenden Sie sich an den Microsoft-Support.
2. Beide Dienst-URLs („https://autologon.microsoftazuread-sso.com“ und „https://aadg.windows.net.nsatc.net“) sind als Teil der Intranetzoneneinstellungen definiert.
3. Stellen Sie sicher, dass der Unternehmensdesktop mit der AD-Domäne verknüpft ist.
4. Stellen Sie sicher, dass der Benutzer mit einem AD-Domänenkonto beim Desktop angemeldet ist.
5. Stellen Sie sicher, dass das Benutzerkonto aus einer AD-Gesamtstruktur stammt, in der das nahtlose einmalige Anmelden eingerichtet wurde.
6. Stellen Sie sicher, dass der Desktop mit dem Unternehmensnetzwerk verbunden ist.
7. Stellen Sie sicher, dass die Uhrzeit des Desktops mit der Uhrzeit von Active Directory und der der Domänencontroller synchronisiert ist und diese nicht mehr als 5 Minuten voneinander abweichen.
8. Bereinigen Sie Ihren Desktop von vorhandenen Kerberos-Tickets. Dies lässt sich durch Ausführung des Befehls **klist-Bereinigung** über eine Eingabeaufforderung erreichen.
9. Überprüfen Sie die Protokolle der Konsole des Browsers (unter „Entwicklertools“), um potenzielle Probleme zu ermitteln.

### <a name="domain-controller-logs"></a>Domänencontrollerprotokolle

Wenn die erfolgreiche Überwachung auf Ihrem Domänencontroller aktiviert ist, wird bei jeder nahtlosen einmaligen Anmeldung durch einen Benutzer ein Sicherheitseintrag (das dem Computerkonto **AzureADSSOAcc$** zugeordnete Ereignis 4769) im Ereignisprotokoll erfasst. Sie können diese Sicherheitsereignisse mithilfe der folgenden Abfrage abrufen:

```
    <QueryList>
      <Query Id="0" Path="Security">
    <Select Path="Security">*[EventData[Data[@Name='ServiceName'] and (Data='AZUREADSSOACC$')]]</Select>
      </Query>
    </QueryList>
```

