---
title: "Ausführliche Informationen: Azure AD SSPR | Microsoft-Dokumentation"
description: "Ausführliche Informationen zur Self-Service-Kennwortzurücksetzung in Azure AD"
services: active-directory
keywords: 
documentationcenter: 
author: MicrosoftGuyJFlo
manager: femila
ms.assetid: 618c5908-5bf6-4f0d-bf88-5168dfb28a88
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/26/2017
ms.author: joflore
ms.translationtype: Human Translation
ms.sourcegitcommit: be3ac7755934bca00190db6e21b6527c91a77ec2
ms.openlocfilehash: 02826ffad9838c3e22721cc3c189e8cc13020059
ms.contentlocale: de-de
ms.lasthandoff: 05/03/2017


---
# <a name="self-service-password-reset-in-azure-ad-deep-dive"></a>Ausführliche Informationen zur Self-Service-Kennwortzurücksetzung in Azure AD

Wie funktioniert die Self-Service-Kennwortzurücksetzung (SSPR)? Was bedeutet diese Option in der Benutzeroberfläche? Im weiteren Verlauf erfahren Sie mehr über die Self-Service-Kennwortzurücksetzung in Azure AD.

## <a name="how-does-the-password-reset-portal-work"></a>Wie funktioniert das Portal für die Kennwortzurücksetzung?

Wenn ein Benutzer zum Portal für das Zurücksetzen von Benutzerkennwörtern navigiert, wird ein Workflow gestartet, um Folgendes zu bestimmen:

   * Wie soll die Seite lokalisiert werden?
   * Ist das Benutzerkonto gültig?
   * Zu welcher Organisation gehört der Benutzer?
   * Wo wird das Kennwort des Benutzers verwaltet?
   * Ist der Benutzer zur Verwendung des Features lizenziert?


Nachfolgend wird beschrieben, welche Logik hinter der Seite zur Kennwortzurücksetzung steckt.

1. Der Benutzer klickt auf den Link „Können Sie nicht auf Ihr Konto zugreifen?“ oder wechselt direkt zu [https://passwordreset.microsoftonline.com](https://passwordreset.microsoftonline.com).
2. Die Benutzeroberfläche wird basierend auf dem Browsergebietsschema in der entsprechenden Sprache wiedergegeben. Die Benutzeroberfläche für das Zurücksetzen des Kennworts wird in alle Sprachen lokalisiert, die Office 365 unterstützt.
3. Der Benutzer gibt eine Benutzer-ID ein und durchläuft erfolgreich die Captchaprüfung.
4. Azure AD prüft folgendermaßen, ob der Benutzer diese Funktion verwenden darf:
   * Es wird geprüft, ob die Funktion für diesen Benutzer aktiviert ist und ob eine Azure AD-Lizenz zugewiesen ist.
     * Wenn diese Funktion für den Benutzer nicht aktiviert ist oder keine Lizenz vorliegt, wird der Benutzer aufgefordert, sich zum Zurücksetzen des Kennworts an den Administrator zu wenden.
   * Es wird überprüft, ob der Benutzer in seinem Konto Daten für die Überprüfung in mehreren Schritten definiert hat, die der Administratorrichtlinie entsprechen.
     * Wenn die Richtlinie nur eine Überprüfung in einem Schritt vorsieht, wird sichergestellt, dass der Benutzer für mindestens eine der durch die Administratorrichtlinie aktivierten Überprüfungen geeignete Daten definiert hat.
       * Sind keine entsprechenden Daten konfiguriert, wird der Benutzer aufgefordert, sich zum Zurückzusetzen des Kennworts an den Administrator zu wenden.
     * Wenn die Richtlinie eine Überprüfung in zwei Schritten vorsieht, wird sichergestellt, dass der Benutzer für mindestens zwei der durch die Administratorrichtlinie aktivierten Überprüfungen geeignete Daten definiert hat.
       * Sind keine entsprechenden Daten konfiguriert, wird der Benutzer aufgefordert, sich zum Zurückzusetzen des Kennworts an den Administrator zu wenden.
   * Es wird überprüft, ob das Benutzerkennwort lokal verwaltet wird oder nicht (im Verbund oder mit Kennworthashsynchronisierung).
     * Wenn das Rückschreiben von Kennwörtern konfiguriert ist und das Benutzerkennwort lokal verwaltet wird, kann der Benutzer mit der Authentifizierung fortfahren und sein Kennwort zurücksetzen.
     * Wenn das Rückschreiben von Kennwörtern nicht konfiguriert ist und das Benutzerkennwort lokal verwaltet wird, wird der Benutzer aufgefordert, sich zum Zurückzusetzen des Kennworts an den Administrator zu wenden.
5. Wenn festgestellt wird, dass der Benutzer sein Kennwort zurücksetzen darf, wird er durch den Vorgang für die Kennwortzurücksetzung geleitet.

## <a name="authentication-methods"></a>Authentifizierungsmethoden

Wenn die Self-Service-Kennwortzurücksetzung (SSPR) aktiviert ist, müssen Sie mindestens eine der folgenden Optionen als Authentifizierungsmethode auswählen. Für mehr Flexibilität für Ihre Benutzer wird dringend empfohlen, mindestens zwei Authentifizierungsmethoden auszuwählen.

* E-Mail
* Mobiltelefon
* Bürotelefon
* Sicherheitsfragen

### <a name="what-fields-are-used-in-the-directory-for-authentication-data"></a>Welche Felder werden im Verzeichnis für Authentifizierungsdaten verwendet?

* „Bürotelefon“ entspricht „Bürotelefon“
    * Benutzer können dieses Feld nicht selbst festlegen. Es muss von einem Administrator definiert werden.
* „Mobiltelefon“ entspricht entweder „Authentifizierungstelefon“ (nicht öffentlich sichtbar) oder „Mobiltelefon“ (öffentlich sichtbar)
    * Der Dienst sucht zuerst nach „Authentifizierungstelefon“, und greift dann, wenn dieses nicht vorhanden ist, auf „Mobiltelefon“ zurück.
* „Alternative E-Mail-Adresse“ entspricht entweder „E-Mail für Authentifizierung“ (nicht öffentlich sichtbar) oder „Alternative E-Mail-Adresse“
    * Der Dienst sucht zuerst nach „E-Mail für Authentifizierung“, und greift dann auf „Alternative E-Mail-Adresse“ zurück.

Standardmäßig werden nur die Cloudattribute „Bürotelefon“ und „Mobiltelefon“ mit Ihrem Cloudverzeichnis aus dem lokalen Verzeichnis für Authentifizierungsdaten synchronisiert.

Benutzer können ihr Kennwort nur dann zurücksetzen, wenn für sie Daten in den Authentifizierungsmethoden vorliegen, die der Administrator aktiviert und als erforderlich festgelegt hat.

Wenn ein Benutzer nicht möchte, dass seine Mobiltelefonnummer im Verzeichnis sichtbar ist, sie aber dennoch für das Zurücksetzen von Kennwörtern verwenden möchte, sollten Administratoren die Daten nicht in das Verzeichnis übernehmen, und der Benutzer sollte sein **Authentifizierungstelefon**-Attribut dann über das [Registrierungsportal für die Kennwortzurücksetzung](http://aka.ms/ssprsetup) auffüllen. Administratoren können diese Informationen im Profil des Benutzers weiterhin anzeigen, sie werden jedoch nicht an anderer Stelle veröffentlicht. Wenn ein Azure-Administratorkonto eine Authentifizierungstelefonnummer registriert, wird sie in das Feld „Mobiltelefon“ eingetragen und ist sichtbar.

### <a name="number-of-authentication-methods-required"></a>Anzahl erforderlicher Authentifizierungsmethoden

Diese Option bestimmt die Mindestanzahl der verfügbaren Authentifizierungsmethoden, die ein Benutzer zum Zurücksetzen oder Entsperren des Kennworts durchlaufen muss. Sie kann auf 1 oder 2 festgelegt werden.

Benutzer können wählen, weitere Authentifizierungsmethoden bereitzustellen, wenn sie vom Administrator aktiviert sind.

Sind für einen Benutzer nicht die mindestens erforderlichen Methoden registriert, wird eine Fehlerseite angezeigt, die ihn auffordert, einen Administrator zu bitten, sein Kennwort zurückzusetzen.

### <a name="how-secure-are-my-security-questions"></a>Wie sicher sind meine Sicherheitsfragen?

Wenn Sie Sicherheitsfragen verwenden, sollten sie mit einer anderen Methode verwendet werden, weil sie weniger sicher als die anderen Methoden sein können, da möglicherweise einige Personen die Sicherheitsfragen anderer Benutzer beantworten können.

> [!NOTE] 
> Sicherheitsfragen werden privat und sicher in einem Benutzerobjekt im Verzeichnis gespeichert und können nur während der Registrierung von Benutzern beantwortet werden. Es gibt keine Möglichkeit für einen Administrator, die Fragen oder Antworten eines Benutzers zu lesen oder zu ändern.
>

### <a name="security-question-localization"></a>Lokalisierung der Sicherheitsfragen

Sämtliche folgenden vordefinierten Fragen werden in alle Office 365-Sprachen lokalisiert, basierend auf dem Gebietsschema des Browsers des Benutzers.

* In welcher Stadt haben Sie Ihren ersten Partner bzw. Ihre erste Partnerin kennengelernt?
* In welcher Stadt haben sich Ihre Eltern kennengelernt?
* In welcher Stadt lebt Ihr Bruder/Ihre Schwester?
* Wie heißt die Geburtsstadt Ihres Vaters?
* In welcher Stadt haben Sie Ihre erste Arbeitsstelle angetreten?
* Wie heißt die Geburtsstadt Ihrer Mutter?
* In welcher Stadt haben Sie den Neujahrstag 2000 verbracht?
* Wie hieß Ihr Lieblingslehrer bzw. Ihre Lieblingslehrerin in der Grundschule mit Nachnamen?
* Bei welcher Universität haben Sie sich beworben, dort jedoch nicht studiert?
* An welchem Ort fand Ihre Hochzeitsfeier statt?
* Wie lautet der 2. Vorname Ihres Vaters?
* Was ist Ihr Lieblingsgericht?
* Wie lautet der Vor- und Nachname Ihrer Großmutter mütterlicherseits?
* Wie lautet der 2. Vorname Ihrer Mutter?
* In welchem Monat und Jahr ist Ihre älteste Schwester bzw. Ihr ältester Bruder geboren? (z.B. November 1985)
* Wie lautet der 2. Vorname Ihrer ältesten Schwester bzw. Ihres ältesten Bruders?
* Wie lautet der Vor- und Nachname Ihres Großvaters väterlicherseits?
* Wie lautet der 2. Vorname Ihrer jüngsten Schwester bzw. Ihres jüngsten Bruders?
* Auf welche Schule sind Sie in der sechsten Klasse gegangen?
* Wie lautet der Vor- und Nachname Ihres besten Freundes bzw. Ihrer besten Freundin in Ihrer Kindheit?
* Wie lautet der Vor- und Nachname Ihres ersten Partners bzw. Ihrer ersten Partnerin?
* Wie lautete der Nachname des Lehrers bzw. der Lehrerin Ihres Lieblingsfachs?
* Welche Marke und welches Modell hatte Ihr erstes Auto oder Motorrad?
* Wie heißt die erste Schule, die Sie besucht haben?
* In welchem Krankenhaus wurden Sie geboren?
* In welcher Straße haben Sie als Kind zuerst gewohnt?
* Wer war der Held Ihrer Kindheit?
* Wie hieß Ihr Lieblingskuscheltier?
* Wie hieß Ihr erstes Haustier?
* Welchen Spitznamen hatten Sie als Kind?
* Was war in der Schulzeit Ihre Lieblingssportart?
* Was war Ihr erster Job?
* Wie lauten die letzten vier Ziffern der Telefonnummer in Ihrer Kindheit?
* Was wollten Sie als Kind später einmal werden?
* Welches ist die berühmteste Person, die Sie kennen gelernt haben?

### <a name="custom-security-questions"></a>Benutzerdefinierte Sicherheitsfragen

Benutzerdefinierte Sicherheitsfragen sind nicht für verschiedene Gebietsschemas lokalisiert. Alle benutzerdefinierten Fragen werden auch bei abweichendem Gebietsschema im Browser in der Sprache angezeigt, in der sie in der administrativen Benutzeroberfläche eingegeben wurden. Wenn Sie lokalisierte Fragen benötigen, verwenden Sie die vordefinierten Fragen.

Die Länge von benutzerdefinierten Sicherheitsfragen ist auf 200 Zeichen begrenzt.

### <a name="security-question-requirements"></a>Anforderungen an Sicherheitsfragen

* Die Mindestanzahl von Zeichen für Antworten ist 3 Zeichen
* Die maximale Zeichenanzahl für Antworten ist 40 Zeichen
* Benutzer können dieselbe Frage nicht mehrmals beantworten
* Benutzer können für nicht dieselbe Antwort für mehrere Fragen angeben
* Bei der Definition von Fragen und Antworten kann ein beliebiger Zeichensatz verwendet werden (einschließlich Unicode)
* Die Anzahl der definierten Fragen muss größer als oder gleich der Anzahl der für die Registrierung erforderlichen Fragen sein

## <a name="registration"></a>Registrierung

### <a name="require-users-to-register-when-signing-in"></a>Registrierung von Benutzern bei der Anmeldung verlangen

Durch Aktivieren dieser Option müssen Benutzer, für die die Kennwortzurücksetzung aktiviert ist, die Registrierung für die Kennwortzurücksetzung abschließen, wenn sie sich bei Anwendungen anmelden, die Azure AD verwenden, z.B. folgende:

* Office 365
* Azure-Portal
* Anpassung des Zugriffsbereichs
* Verbundanwendungen
* Benutzerdefinierte Anwendungen, die Azure AD verwenden

Wird dieses Feature deaktiviert, können Benutzer ihre Kontaktinformationen weiterhin unter [http://aka.ms/ssprsetup](http://aka.ms/ssprsetup) oder durch Klicken auf den Link **Für das Zurücksetzen des Kennworts registrieren** auf der Registerkarte Profil im Zugriffsbereich manuell registrieren.

> [!NOTE]
> Benutzer können das Registrierungsportal für die Kennwortzurücksetzung durch Klicken auf „Abbrechen“ oder durch Schließen des Fensters schließen, werden aber bis zum Abschluss der Registrierung bei jeder Anmeldung erneut dazu aufgefordert.
>

### <a name="number-of-days-before-users-are-asked-to-reconfirm-their-authentication-information"></a>Anzahl der Tage, bevor Benutzer aufgefordert werden, ihre Authentifizierungsinformationen erneut zu bestätigen

Diese Option bestimmt den Zeitraum zwischen dem Festlegen und dem erneuten Bestätigen der Authentifizierungsinformationen und ist nur verfügbar, wenn die Option **Registrierung von Benutzern bei der Anmeldung verlangen** aktiviert ist.

Gültige Werte sind 0 bis 730 Tage, wobei „0“ bedeutet, dass Benutzer nie zum Bestätigen ihrer Authentifizierungsinformationen aufgefordert werden.

## <a name="notifications"></a>Benachrichtigungen

### <a name="notify-users-on-password-resets"></a>Benutzer über Kennwortzurücksetzungen benachrichtigen?

Ist diese Option auf „Ja“ festgelegt, erhält der Benutzer, der sein Kennwort zurücksetzt, eine E-Mail an seine in Azure AD erfasste primäre und alternative E-Mail-Adresse, in der er darüber informiert wird, dass sein Kennwort über das SSPR-Portal geändert wurde. Niemand sonst wird über das Zurücksetzen informiert.

### <a name="notify-all-admins-when-other-admins-reset-their-passwords"></a>Sollen alle Administratoren benachrichtigt werden, wenn andere Administratoren ihr Kennwort zurücksetzen?

Ist diese Option auf „Ja“ festgelegt, erhalten **alle Administratoren** eine E-Mail an ihre in Azure AD erfasste primäre E-Mail-Adresse, in der sie darüber informiert werden, dass ein anderer Administrator ihr Kennwort mithilfe von SSPR geändert hat.

Beispiel: Es gibt vier Administratoren in einer Umgebung. Administrator A setzt das Kennwort mithilfe von SSPR zurück. Administratoren B, C und D erhalten eine E-Mail-Nachricht, in der sie über die Änderung informiert werden.

## <a name="on-premises-integration"></a>Lokale Integration

Wenn Sie Azure AD Connect installiert, konfiguriert und aktiviert haben, stehen zusätzliche Optionen für lokale Integrationen zur Verfügung.

### <a name="write-back-passwords-to-your-on-premises-directory"></a>Kennwörter in Ihr lokales Verzeichnis zurückschreiben?

Steuert, ob das Rückschreiben von Kennwörtern für dieses Verzeichnis aktiviert ist. In diesem Fall wird der Status des lokalen Rückschreibungsdiensts angezeigt. Dies ist nützlich, wenn Sie das Rückschreiben von Kennwörtern vorübergehend deaktivieren möchten, ohne Azure AD Connect erneut zu konfigurieren.

* Wenn die Option auf „Ja“ gesetzt ist, wird das Rückschreiben aktiviert, und Verbundbenutzer und Benutzer mit Kennworthashsynchronisierung können ihre Kennwörter zurücksetzen.
* Wenn die Option auf „Nein“ gesetzt ist, wird das Rückschreiben deaktiviert, und Verbundbenutzer und Benutzer mit Kennworthashsynchronisierung können sich nicht für das Zurücksetzen ihrer Kennwörter registrieren.

### <a name="allow-users-to-unlock-accounts-without-resetting-their-password"></a>Benutzern das Entsperren von Konten ohne Zurücksetzen des Kennworts erlauben

Legt fest, ob Benutzer, die das Kennwortzurücksetzungsportal aufrufen, die Option zum Entsperren ihrer lokalen Active Directory-Konten ohne Zurücksetzen ihres Kennworts erhalten sollen. Standardmäßig werden Azure AD-Konten beim Zurücksetzen von Kennwörtern stets entsperrt – mit dieser Einstellung können Sie diese beiden Vorgänge trennen. 

* Bei der Einstellung „Ja“ erhalten die Benutzer die Option zum Zurücksetzen ihres Kennworts und Entsperren ihres Kontos, und auch die Option des Entsperrens ohne ein Zurücksetzen des Kennworts.
* Bei der Einstellung „Nein“ können die Benutzer das Entsperren des Kontos nur in Kombination mit dem Zurücksetzen des Kennworts vornehmen.

## <a name="network-requirements"></a>Netzwerkanforderungen

### <a name="firewall-rules"></a>Firewallregeln

[Liste der Microsoft Office-URLs und -IP-Adressen](https://support.office.com/article/Office-365-URLs-and-IP-address-ranges-8548a211-3fe7-47cb-abb1-355ea5aa88a2)

Für Azure AD Connect, Version 1.1.443.0 und höher, benötigen Sie ausgehenden HTTPS-Zugriff auf Folgendes:
* passwordreset.microsoftonline.com
* servicebus.windows.net

Für präziseren Zugriff finden Sie [hier](https://www.microsoft.com/download/details.aspx?id=41653) die aktualisierte Liste der Microsoft Azure Datacenter-IP-Adressbereiche, die jeden Mittwoch aktualisiert wird und jeweils am darauffolgenden Montag in Kraft tritt.

### <a name="idle-connection-timeouts"></a>Timeouts für Leerlaufverbindungen

Das Azure AD Connect-Tool sendet regelmäßig Pings/Keep-Alives an ServiceBus-Endpunkte, um die Verbindungen aufrechtzuerhalten. Falls das Tool erkennt, dass zu viele Verbindungen getrennt werden, verkürzt es automatisch das Pingintervall für den Endpunkt. Das kleinstmögliche Pingintervall beträgt 60 Sekunden. Wir empfehlen jedoch dringend, bei Proxys/Firewalls Leerlaufverbindungen mit einer Dauer von mindestens zwei bis drei Minuten zuzulassen. *Bei älteren Versionen wird ein Zeitraum von mindestens vier Minuten empfohlen.

## <a name="active-directory-permissions"></a>Active Directory-Berechtigungen

Das im Azure AD Connect-Dienstprogramm angegebene Konto muss über Berechtigungen zum Zurücksetzen und Ändern des Kennworts sowie über Schreibberechtigungen für LockoutTime und Schreibberechtigungen für „pwdLastSet“ und erweiterte Rechte entweder für das Stammobjekt **jeder Domäne** in dieser Gesamtstruktur **ODER** für die Benutzerorganisationseinheiten verfügen, die innerhalb des Bereichs für die SSPR liegen sollen.

Wenn Sie nicht sicher sind, auf welches Konto sich die oben genannten Angaben beziehen, öffnen Sie die Azure Active Directory Connect-Konfigurationsoberfläche, und klicken Sie auf die Option „Lösung prüfen“. Das Konto, dem Sie Berechtigungen hinzufügen müssen, wird unter „Synchronisierte Verzeichnisse“ angezeigt.

Durch das Festlegen dieser Berechtigungen kann das MA-Dienstkonto für jede Gesamtstruktur Kennwörter im Namen der Benutzerkonten innerhalb dieser Gesamtstruktur verwalten. **Wenn Sie diese Berechtigungen nicht gewähren, erhalten die Benutzer – obwohl das Zurückschreiben scheinbar ordnungsgemäß konfiguriert ist – Fehler bei dem Versuch, ihre lokalen Kennwörter über die Cloud verwalten.**

> [!NOTE]
> Es kann bis zu einer Stunde oder länger dauern, bis diese Berechtigungen auf alle Objekte in Ihrem Verzeichnis repliziert wurden.
>

So richten Sie die entsprechenden Berechtigungen für das Rückschreiben von Kennwörtern ein

1. Öffnen Sie „Active Directory-Benutzer und -Computer“ mit einem Konto, das über geeignete Berechtigungen für die Domänenverwaltung verfügt.
2. Vergewissern Sie sich, dass im Menü „Ansicht“ die Option „Erweiterte Features“ aktiviert ist.
3. Klicken Sie im linken Bereich mit der rechten Maustaste auf das Objekt, das den Stamm der Domäne repräsentiert, und wählen Sie Eigenschaften aus.
    * Klicken Sie auf die Registerkarte „Sicherheit“.
    * Klicken Sie dann auf „Erweitert“.
4. Klicken Sie auf der Registerkarte „Berechtigungen“ auf „Hinzufügen“.
5. Wählen Sie das Konto aus, dem Berechtigungen zugewiesen werden sollen (vom Azure AD Connect-Setup).
6. Wählen Sie im Dropdownfeld „Gilt für“ den Eintrag „Nachfolgerbenutzerobjekt“ aus.
7. Aktivieren Sie unter „Berechtigungen“ die Kontrollkästchen für „Kennwort zurücksetzen“, „Kennwort ändern“, L“ockoutTime schreiben“ und „pwdLastSet schreiben“.
8. Klicken Sie auf „Übernehmen/OK“, um die Änderungen zu übernehmen und alle geöffneten Dialogfelder zu schließen.

## <a name="how-does-password-reset-work-for-b2b-users"></a>Funktionsweise der Kennwortzurücksetzung für B2B-Benutzer
Das Zurücksetzen und Ändern von Kennwörtern wird von allen B2B-Konfigurationen uneingeschränkt unterstützt.  Im Anschluss finden Sie Informationen zu den drei expliziten B2B-Szenarien, die von der Kennwortzurücksetzung unterstützt werden.

1. **Benutzer aus einer Partnerorganisation mit einem vorhandenen Azure AD-Mandanten:** Falls die Organisation, mit der Sie eine Partnerschaft eingegangen sind, über einen Azure AD-Mandanten verfügt, **respektieren wir die in diesem Mandanten aktivierten Kennwortzurücksetzungsrichtlinien**. Für die Kennwortzurücksetzung muss die Partnerorganisation lediglich sicherstellen, dass die Self-Service-Kennwortzurücksetzung von Azure AD aktiviert ist. Für Office 365-Kunden fallen dadurch keine zusätzlichen Gebühren an. Eine entsprechende Anleitung finden Sie unter [Erste Schritte mit der Kennwortverwaltung](https://azure.microsoft.com/documentation/articles/active-directory-passwords-getting-started/#enable-users-to-reset-or-change-their-aad-passwords).
2. **Benutzer, die sich mit der [Self-Service-Registrierung](active-directory-self-service-signup.md) registriert haben:** Wenn die Organisation, mit der Sie eine Partnerschaft eingegangen sind, das [Self-Service-Registrierungsfeature](active-directory-self-service-signup.md) verwendet, um auf einen Mandanten zuzugreifen, steht eine Zurücksetzung unter Verwendung der E-Mail-Adresse zur Verfügung, mit der die Registrierung erfolgt ist.
3. **B2B-Benutzer:** Alle neuen B2B-Benutzer, die mithilfe der neuen [Azure AD-B2B-Funktionen](active-directory-b2b-what-is-azure-ad-b2b.md) erstellt werden, können ihre Kennwörter unter Verwendung der E-Mail-Adresse zurücksetzen, mit der sie sich im Rahmen des Einladungsprozesses registriert haben.

Navigieren Sie zum Testen mit einem dieser Partnerbenutzer zu „http://passwordreset.microsoftonline.com“. Sofern keine alternative E-Mail-Adresse und keine E-Mail-Adresse für die Authentifizierung definiert sind, funktioniert die Kennwortzurücksetzung wie erwartet.

## <a name="next-steps"></a>Nächste Schritte

Die folgenden Links führen zu weiteren Informationen zur Kennwortzurücksetzung mit Azure AD

* [**Schnellstart:**](active-directory-passwords-getting-started.md) Informieren Sie sich schnell über die Self-Service-Kennwortverwaltung von Azure AD. 
* [**Lizenzierung:**](active-directory-passwords-licensing.md) Konfigurieren Sie Ihre Azure AD-Lizenzierung.
* [**Daten:**](active-directory-passwords-data.md) Erfahren Sie, welche Daten erforderlich sind und wie sie für die Kennwortverwaltung verwendet werden.
* [**Rollout:**](active-directory-passwords-best-practices.md) Mithilfe der hier enthaltenen Anleitungen können Sie SSPR planen und für Ihre Benutzer bereitstellen.
* [**Richtlinie:**](active-directory-passwords-policy.md) Verstehen Sie Azure AD-Kennwortrichtlinien, und legen Sie sie fest.
* [**Kennwortrückschreiben:**](active-directory-passwords-writeback.md) Funktionsweise des Kennwortrückschreibens in Ihrem lokalen Verzeichnis
* [**Anpassung:**](active-directory-passwords-customize.md) Passen Sie das Aussehen und Verhalten von SSPR für Ihr Unternehmen an.
* [**Berichterstellung:**](active-directory-passwords-reporting.md) Ermitteln Sie, ob, wann und wo Ihre Benutzer auf SSPR-Funktionen zugreifen.
* [**Häufig gestellte Fragen (FAQ):**](active-directory-passwords-faq.md) Wie? Warum? Was? Wo? Wer? Wann? Antworten auf Fragen, die Sie schon immer stellen wollten
* [**Problembehandlung:**](active-directory-passwords-troubleshoot.md) Erfahren Sie, wie Sie häufig auftretende Probleme bei SSPR beheben.


