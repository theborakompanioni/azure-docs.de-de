---
title: "Häufig gestellte Fragen: Azure AD SSPR | Microsoft-Dokumentation"
description: "Häufig gestellte Fragen zur Self-Service-Kennwortzurücksetzung in Azure AD"
services: active-directory
keywords: "Active Directory-Kennwortverwaltung, Kennwortverwaltung, Self-Service-Kennwortzurücksetzung in Azure AD"
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
ms.translationtype: Human Translation
ms.sourcegitcommit: be3ac7755934bca00190db6e21b6527c91a77ec2
ms.openlocfilehash: 00acd4090ed981ab2b05e955e93d1c689ea1a2e6
ms.contentlocale: de-de
ms.lasthandoff: 05/03/2017

---
# Häufig gestellte Fragen zur Kennwortverwaltung
<a id="password-management-frequently-asked-questions" class="xliff"></a>

Im Folgenden werden einige häufig gestellte Fragen rund um die Kennwortzurücksetzung aufgeführt.

Wenn Sie eine allgemeine Frage zu Azure AD und zur Self-Service-Kennwortzurücksetzung haben, die hier nicht beantwortet wird, können Sie die Community in den [Azure AD-Foren](https://social.msdn.microsoft.com/Forums/en-US/home?forum=WindowsAzureAD) um Unterstützung bitten. Zu den Mitgliedern der Community gehören Techniker, Produktmanager, MVPs und andere IT-Experten.

Diese FAQ sind in folgende Abschnitte unterteilt:

* [**Fragen zum Registrieren für die Kennwortzurücksetzung**](#password-reset-registration)
* [**Fragen zum Zurücksetzen des Kennworts**](#password-reset)
* [**Fragen zum Ändern des Kennworts**](#password-change)
* [**Fragen zu Kennwortverwaltungsberichten**](#password-management-reports)
* [**Fragen zur Kennwortrückschreibung**](#password-writeback)

## Registrieren für die Kennwortzurücksetzung
<a id="password-reset-registration" class="xliff"></a>
* **F: Können meine Benutzer ihre eigenen Daten zur Kennwortzurücksetzung registrieren?**

  > **A:** Ja, wenn die Kennwortzurücksetzung aktiviert ist und die Benutzer lizenziert sind, können diese das Kennwortregistrierungsportal auf „http://aka.ms/ssprsetup“ öffnen und ihre Authentifizierungsdaten registrieren. Benutzer können sich auch registrieren, indem sie in den Zugriffsbereich unter „http://myapps.microsoft.com“ wechseln und auf die Registerkarte für die Kennwortzurücksetzung klicken.
  >
  >
* **F: Kann ich im Namen meiner Benutzer Daten zur Kennwortzurücksetzung definieren?**

  > **A:** Ja, Sie können dies mit Azure AD Connect, PowerShell, dem [Azure-Portal](https://portal.azure.com) oder dem Office-Verwaltungsportal tun. Weitere Informationen finden Sie im Artikel [Von der Azure AD Self-Service-Kennwortzurücksetzung verwendete Daten](active-directory-passwords-data.md).
  >
  >
* **F: Kann ich Daten für Sicherheitsfragen vom lokalen Standort aus synchronisieren?**

  > **A:** Dies ist zurzeit noch nicht möglich.
  >
  >
* **F: Können meine Benutzer Daten so registrieren, dass anderen Benutzern diese Daten nicht angezeigt werden?**

  > **A:** Ja. Wenn Benutzer Daten über das Registrierungsportal für die Kennwortzurücksetzung registrieren, werden diese in privaten Authentifizierungsfeldern gespeichert, die nur für globale Administratoren und den Benutzer selbst sichtbar sind.
    >
    > [!NOTE]
    > Wenn ein **Azure-Administratorkonto** eine Authentifizierungstelefonnummer registriert, wird sie auch in das Feld „Mobiltelefon“ eingetragen und ist sichtbar.
    >
  >
  >
* **F: Müssen meine Benutzer registriert werden, damit sie das Zurücksetzen von Kennwörtern verwenden können?**

  > **A:** Nein, wenn Sie genügend Authentifizierungsinformationen in ihrem Auftrag definieren, müssen Benutzer sich nicht registrieren. Das Zurücksetzen des Kennworts funktioniert, solange Sie in den entsprechenden Feldern im Verzeichnis ordnungsgemäß formatierte Daten gespeichert haben.
  >
  >
* **F: Kann ich die Felder „Telefon für Authentifizierung“, „E-Mail-Adresse zur Authentifizierung“ oder „Alternatives Telefon für Authentifizierung“ im Namen meiner Benutzer synchronisieren oder festlegen?**

  > **A:** Dies ist zurzeit noch nicht möglich.
  >
  >
* **F: Wie erkennt das Registrierungsportal, welche Optionen meinen Benutzern angezeigt werden sollen?**

  > **A:** Das Registrierungsportal für die Kennwortzurücksetzung zeigt nur die Optionen an, die Sie für Ihre Benutzer aktiviert haben. Diese Optionen finden Sie im Abschnitt „Richtlinie zum Zurücksetzen des Benutzerkennworts“ auf der Registerkarte „Konfigurieren“ Ihres Verzeichnisses. Wenn Sie also z.B. die Sicherheitsfragen nicht aktivieren, können Benutzer sich für diese Option nicht registrieren.
  >
  >
* **F: Wann gilt ein Benutzer als registriert?**

  > **A:** Ein Benutzer gilt als für SSPR registriert, wenn er mindestens die **Anzahl von Methoden, die zum Zurücksetzen erforderlich sind**, registriert hat, die Sie im [Azure-Portal](https://portal.azure.com) festgelegt haben.
  >
  >
## Zurücksetzen des Kennworts
<a id="password-reset" class="xliff"></a>
* **F: Wie lange muss ich warten, bis eine E-Mail, eine SMS oder ein Anruf von der Kennwortzurücksetzung eintrifft?**

  > **A:** E-Mails, SMS-Nachrichten und Telefonanrufe sollten in weniger als einer Minute, normalerweise innerhalb von 5 bis 20 Sekunden eingehen.
    >Wenn Sie in diesem Zeitraum die Benachrichtigung nicht erhalten:
        > * Überprüfen Sie Ihren Ordner mit Junk-E-Mails.
        > * Überprüfen Sie, ob die erwartete Nummer oder E-Mail-Adresse kontaktiert wird.
        > * Überprüfen Sie, ob die Authentifizierungsdaten im Verzeichnis korrekt formatiert sind.
                >     * Beispiel: „+1 4255551234“ oder „user@contoso.com“
  >
  >
* **F: Welche Sprachen werden von der Kennwortzurücksetzung unterstützt?**

  > **A:** Die Benutzeroberfläche der Kennwortzurücksetzung, die SMS-Nachrichten und die Sprachanrufe wurden in dieselben Sprachen lokalisiert, die in Office 365 unterstützt werden.
  >
  >
* **F: Welche Teile der Benutzeroberfläche zum Zurücksetzen von Kennwörtern werden angepasst, wenn ich organisationsspezifisches Branding auf der Registerkarte „Konfigurieren“ für mein Verzeichnis festlege?**

  > **A:** Das Portal für die Kennwortzurücksetzung zeigt das Logo Ihrer Organisation und erlaubt Ihnen zudem, den Link „Wenden Sie sich an Ihren Administrator“ so zu konfigurieren, dass er auf eine benutzerdefinierte E-Mail-Adresse oder eine URL verweist. E-Mails, die von der Kennwortzurücksetzung gesendet werden, enthalten das Logo, die Farben sowie den Namen Ihrer Organisation im Nachrichtentext der E-Mail. Zudem weisen sie einen angepassten Absendernamen auf.
  >
  >
* **F: Wie informiere ich meine Benutzer, wo sie ihre Kennwörter zurücksetzen können?**

  > **A:** Sie können Ihre Benutzer direkt an „https://passwordreset.microsoftonline.com“ weiterleiten, oder Sie können sie auffordern, auf den Link **Sie können nicht auf Ihr Konto zugreifen?** zu klicken, der auf jeder Geschäfts-, Schul- oder Uni-Anmeldeseite angezeigt wird. Sie können diese Links auch an einem Ort veröffentlichen, der problemlos für Ihre Benutzer erreichbar ist.
  >
  >
* **F: Kann ich diese Seite von einem mobilen Gerät aus verwenden?**

  > **A:** Ja, diese Seite funktioniert auf mobilen Geräten.
  >
  >
* **F: Wird das Entsperren lokaler Active Directory-Konten unterstützt, wenn Benutzer ihre Kennwörter zurücksetzen?**

  > **A:** Ja, wenn ein Benutzer sein Kennwort zurücksetzt und das Kennwortrückschreiben mit Azure AD Connect bereitgestellt wurde, wird das Konto des Benutzers automatisch entsperrt, wenn das Kennwort zurückgesetzt wird.
  >
  >
* **F: Wie kann ich die Kennwortzurücksetzung direkt in die Anmeldeoberfläche auf den Desktops meiner Benutzer integrieren?**

  > **A:** Wenn Sie ein Azure AD Premium-Kunde sind, können Sie Microsoft Identity Manager ohne zusätzliche Kosten installieren und die Lösung zur lokalen Kennwortzurücksetzung bereitstellen, um diese Anforderung zu erfüllen.
  >
  >
* **F: Kann ich für verschiedene Gebietsschemas unterschiedliche Sicherheitsfragen festlegen?**

  > **A:** Dies ist zurzeit noch nicht möglich.
  >
  >
* **F: Wie viele Fragen können wir für die Authentifizierungsoption mit Sicherheitsfragen konfigurieren?**

  > **A:** Sie können bis zu 20 benutzerdefinierte Sicherheitsfragen im [Azure-Portal](https://portal.azure.com) konfigurieren.
  >
  >
* **F: Wie lang dürfen die Sicherheitsfragen sein?**

  > **A:** Sicherheitsfragen können zwischen 3 und 200 Zeichen lang sein.
  >
  >
* **F: Wie lang dürfen die Antworten auf Sicherheitsfragen sein?**

  > **A:** Antworten dürfen 3 bis 40 Zeichen lang sein.
  >
  >
* **F: Werden doppelte Antworten auf Sicherheitsfragen abgelehnt?**

  > **A:** Ja, doppelte Antworten auf Sicherheitsfragen werden abgelehnt.
  >
  >
* **F: Kann ein Benutzer sich für dieselbe Sicherheitsfrage mehrmals registrieren?**

  > **A:** Nein, sobald ein Benutzer sich für eine bestimmte Frage registriert, kann er sich kein zweites Mal für dieselbe Frage registrieren.
  >
  >
* **F: Ist es möglich, eine Mindestanzahl von Sicherheitsfragen für die Registrierung und Zurücksetzung festzulegen?**

  > **A:** Ja, es kann ein Grenzwert für die Registrierung und ein anderer für die Zurücksetzung festgelegt werden. Drei bis fünf Fragen können für die Registrierung und drei bis fünf Fragen für die Zurücksetzung verlangt werden.
  >
  >
* **F: Wie werden die Sicherheitsfragen während der Zurücksetzung ausgewählt, wenn ein Benutzer mehr als die maximale Anzahl von Fragen registriert, die für die Zurücksetzung erforderlich sind?**

  > **A:** Aus der Gesamtzahl von Fragen, für die sich ein Benutzer registriert hat, werden N Sicherheitsfragen nach dem Zufallsprinzip ausgewählt, wobei N für die **Anzahl der für die Zurücksetzung erforderlichen Fragen** steht. Wenn für einen Benutzer beispielsweise fünf Sicherheitsfragen registriert wurden, aber nur drei für das Zurücksetzen erforderlich sind, werden aus diesen fünf nach dem Zufallsprinzip drei ausgewählt und bei der Zurücksetzung angezeigt. Wenn der Benutzer die Fragen falsch beantwortet, wird der Auswahlvorgang erneut durchgeführt, um eine wiederholte Fragestellung zu verhindern.
  >
  >
* **F: Werden Benutzer daran gehindert, Kennwörter innerhalb kurzer Zeit mehrmals zurückzusetzen?**

  > **A:** Ja, es wurden Sicherheitsfunktionen in die Kennwortzurücksetzung integriert, um Schutz vor Missbrauch zu bieten. Benutzer dürfen innerhalb einer Stunde nur fünfmal versuchen, das Kennwort zurückzusetzen, bevor sie für 24 Stunden gesperrt werden. Benutzer dürfen innerhalb einer Stunde nur fünfmal versuchen, eine Telefonnummer zu validieren, bevor sie für 24 Stunden gesperrt werden. Benutzer dürfen innerhalb einer Stunde nur fünfmal versuchen, die einmalige Anmeldung durchzuführen, bevor sie für 24 Stunden gesperrt werden.
  >
  >
* **F: Wie lange ist die Einmalkennung per E-Mail und SMS gültig?**

  > **A:** Die Sitzungsdauer für das Zurücksetzen von Kennwörtern beträgt 105 Minuten. Vom Beginn des Vorgangs zur Kennwortzurücksetzung hat der Benutzer 105 Minuten Zeit, um das Kennwort zurückzusetzen. Die E-Mail- und SMS-Einmalkennungen sind nach Ablauf dieses Zeitraums ungültig.
  >
  >

## Kennwortänderung
<a id="password-change" class="xliff"></a>
* **F: Wo können meine Benutzer ihre Kennwörter ändern?**

  > **A:** Benutzer können ihre Kennwörter überall dort ändern, wo ihr Profilbild oder -symbol angezeigt wird (beispielsweise in [Office 365](https://portal.office.com) oder im [Zugriffsbereich](https://myapps.microsoft.com) in der rechten oberen Ecke). Benutzer können ihre Kennwörter über die [Profilseite des Zugriffsbereichs](https://account.activedirectory.windowsazure.com/r#/profile) ändern. Außerdem werden Benutzer unter Umständen auf dem Azure AD-Anmeldebildschirm zum Ändern ihres Kennworts aufgefordert, falls ihr Kennwort abgelaufen ist. Und schließlich können Benutzer direkt zum [Kennwortänderungsportal von Azure AD](https://account.activedirectory.windowsazure.com/ChangePassword.aspx) navigieren und ihr Kennwort dort ändern.
  >
  >
* **F: Können meine Benutzer im Office-Portal benachrichtigt werden, wenn ihr lokales Kennwort abläuft?**

  > **A:** Dies ist aktuell möglich, wenn Sie AD FS verwenden. Eine entsprechende Anleitung finden Sie unter [Configure AD FS to Send Password Expiry Claims](https://technet.microsoft.com/windows-server-docs/identity/ad-fs/operations/configure-ad-fs-to-send-password-expiry-claims?f=255&MSPPError=-2147217396) (Konfigurieren von AD FS zum Senden von Kennwortablaufansprüchen). Bei Verwendung der Kennworthashsynchronisierung ist dies derzeit nicht möglich. Da wir keine Kennwortrichtlinien aus der lokalen Umgebung synchronisieren, können wir auch keine Ablaufbenachrichtigungen in Cloudumgebungen veröffentlichen. In beiden Fällen können Sie [Benutzer, deren Kennwörter demnächst ablaufen, auch mithilfe von PowerShell benachrichtigen](https://social.technet.microsoft.com/wiki/contents/articles/23313.notify-active-directory-users-about-password-expiry-using-powershell.aspx).
  >
  >

## Berichte zur Kennwortverwaltung
<a id="password-management-reports" class="xliff"></a>
* **F: Wie lange dauert es, bis Daten in den Berichten zur Kennwortverwaltung angezeigt werden?**

  > **A:** Daten sollten innerhalb von 5 bis 10 Minuten in den Berichten zur Kennwortverwaltung angezeigt werden. In manchen Fällen kann es bis zu einer Stunde dauern.
  >
  >
* **F: Wie kann ich die Berichte zur Kennwortverwaltung filtern?**

  > **A:** Sie können die Berichte zur Kennwortverwaltung filtern, indem Sie auf die kleine Lupe ganz rechts neben den Spaltenbeschriftungen oben im Bericht klicken. Wenn Sie umfangreichere Filterfunktionen benötigen, können Sie den Bericht in Excel exportieren und eine Pivottabelle erstellen.
  >
  >
* **F: Wie hoch ist die maximale Anzahl der Ereignisse, die in den Kennwortverwaltungsberichten gespeichert werden?**

  > **A** : In den Kennwortverwaltungsberichten werden bis zu 75.000 Kennwortzurücksetzungsereignisse oder Ereignisse im Zusammenhang mit der Registrierung für die Kennwortzurücksetzung für einen Zeitraum von bis zu 30 Tagen gespeichert.  Eine Erweiterung dieser Anzahl der Ereignisse ist in Arbeit.
  >
  >
* **F: Wie weit reichen die Berichte zur Kennwortverwaltung zurück?**

  > **A:** Die Berichte zur Kennwortverwaltung zeigen Vorgänge der letzten 30 Tage an. Wenn Sie diese Daten archivieren möchten, können Sie die Berichte in regelmäßigen Abständen herunterladen und an einem anderen Speicherort speichern.
  >
  >
* **F: Gibt es eine maximale Anzahl von Zeilen, die in den Berichten zur Kennwortverwaltung angezeigt werden können?**

  > **A:** Ja, jeder der Kennwortverwaltungsberichte kann maximal 75.000 Zeilen umfassen – unabhängig davon, ob er auf der Benutzeroberfläche angezeigt oder heruntergeladen wird.
  >
  >
* **F: Gibt es eine API, um auf die Daten der Kennwortzurücksetzung oder des Registrierens für die Kennwortzurücksetzung zuzugreifen?**

  > **A:** Ja, in der folgenden Dokumentation erfahren Sie, wie Sie auf den Berichtdatenstrom über die Kennwortzurücksetzung zugreifen können.  [Azure AD-Berichte und -Ereignisse (Vorschau)](https://msdn.microsoft.com/library/azure/mt126081.aspx#BKMK_SsprActivityEvent).
  >
  >

## Rückschreiben von Kennwörtern
<a id="password-writeback" class="xliff"></a>
* **F: Wie funktioniert das Rückschreiben von Kennwörtern im Detail?**

  > **A:** Unter [Funktionsweise des Kennwortrückschreibens](active-directory-passwords-writeback.md) wird erläutert, was geschieht, wenn Sie das Kennwortrückschreiben aktivieren, und wie Daten durch das System wieder in Ihre lokale Umgebung übertragen werden.
  >
  >
* **F: Wie lange dauert es, bis das Rückschreiben von Kennwörtern funktioniert?  Gibt es eine Synchronisierungsverzögerung wie bei der Kennworthashsynchronisierung?**

  > **A:** Die Kennwortrückschreibung findet sofort statt. Sie ist eine synchrone Pipeline, die grundlegend anders funktioniert als die Kennworthashsynchronisierung. Durch das Kennwortrückschreiben erhalten Benutzer in Echtzeit Feedback über den Erfolg der Kennwortzurücksetzung oder -änderung. Die durchschnittliche Zeit für das erfolgreiche Rückschreiben eines Kennworts liegt bei unter 500 ms.
  >
  >
* **F: Wenn mein lokales Konto deaktiviert ist, welchen Einfluss hat dies auf mein Cloudkonto bzw. meinen Cloudzugriff?**

  > **A:** Wenn Ihre lokale ID deaktiviert ist, wird auch Ihre Cloud-ID bzw. Ihr Cloudzugriff beim nächsten Synchronisierungsintervall über AAD Connect deaktiviert (standardmäßig alle 30 Minuten).
  >
  >
* **F: Wenn mein lokales Konto von lokalen Active Directory-Kennwortrichtlinien eingeschränkt wird, beachtet SSPR diese Richtlinien bei Kennwortänderungen?**

  > **A:** Ja, SSPR nutzt und beachtet lokale AD-Kennwortrichtlinien, einschließlich typischer Kennwortrichtlinien für AD-Domänen sowie differenzierter Kennwortrichtlinien, die für einen bestimmten Benutzer definiert wurden.
  >
  >
* **F: Für welche Arten von Konten funktioniert das Rückschreiben von Kennwörtern?**

  > **A:** Das Kennwortrückschreiben funktioniert für Benutzer mit Verbundauthentifizierung und mit Kennworthashsynchronisierung.
  >
  >
* **F: Werden die Kennwortrichtlinien meiner Domäne durch die Kennwortrückschreibung durchgesetzt?**

  > **A:** Ja, das Kennwortrückschreiben setzt das Kennwortalter, den Verlauf, die Komplexität, Filter und alle anderen Einschränkungen durch, die Sie in Ihrer lokalen Domäne für Kennwörter festlegen können.
  >
  >
* **F: Ist die Kennwortrückschreibung sicher?  Wie kann ich sicher sein, dass ich nicht gehackt werde?**

  > **A:** Ja, das Kennwortrückschreiben ist sicher. Weitere Informationen zu den vier Sicherheitsstufen, die durch den Dienst zum Kennwortrückschreiben implementiert werden, finden Sie im Abschnitt [Sicherheitsmodell für das Kennwortrückschreiben](active-directory-passwords-writeback.md#password-writeback-security-model) im Artikel „Funktionsweise des Kennwortrückschreibens“.
  >
  >

## Nächste Schritte
<a id="next-steps" class="xliff"></a>

Die folgenden Links führen zu weiteren Informationen zur Kennwortzurücksetzung mit Azure AD

* [**Schnellstart:**](active-directory-passwords-getting-started.md) Informieren Sie sich schnell über die Self-Service-Kennwortverwaltung von Azure AD. 
* [**Lizenzierung:**](active-directory-passwords-licensing.md) Konfigurieren Sie Ihre Azure AD-Lizenzierung.
* [**Daten:**](active-directory-passwords-data.md) Erfahren Sie, welche Daten erforderlich sind und wie sie für die Kennwortverwaltung verwendet werden.
* [**Rollout:**](active-directory-passwords-best-practices.md) Mithilfe der hier enthaltenen Anleitungen können Sie SSPR planen und für Ihre Benutzer bereitstellen.
* [**Anpassung:**](active-directory-passwords-customize.md) Passen Sie das Aussehen und Verhalten von SSPR für Ihr Unternehmen an.
* [**Berichterstellung:**](active-directory-passwords-reporting.md) Ermitteln Sie, ob, wann und wo Ihre Benutzer auf SSPR-Funktionen zugreifen.
* [**Richtlinie:**](active-directory-passwords-policy.md): Verstehen Sie Azure AD-Kennwortrichtlinien, und legen Sie sie fest.
* [**Kennwortrückschreiben:**](active-directory-passwords-writeback.md) Funktionsweise des Kennwortrückschreibens in Ihrem lokalen Verzeichnis.
* [**Ausführliche technische Informationen:**](active-directory-passwords-how-it-works.md) Schauen Sie hinter den Vorhang, um zu verstehen, wie alles funktioniert.
* [**Problembehandlung:**](active-directory-passwords-troubleshoot.md) Erfahren Sie, wie Sie häufig auftretende Probleme bei SSPR beheben.

