---
title: 'Erste Schritte: Azure AD-Kennwortverwaltung | Microsoft Docs'
description: "Ermöglichen Sie es Benutzern, ihre eigenen Kennwörter zurückzusetzen, ermitteln Sie Voraussetzungen für die Kennwortzurücksetzung, und aktivieren Sie das Zurückschreiben von Kennwörtern zum Verwalten lokaler Kennwörter in Active Directory."
services: active-directory
keywords: "Active Directory-Kennwortverwaltung, Kennwortverwaltung, Azure AD-Kennwort zurücksetzen"
documentationcenter: 
author: MicrosoftGuyJFlo
manager: femila
editor: curtand
ms.assetid: bde8799f-0b42-446a-ad95-7ebb374c3bec
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 03/08/2017
ms.author: joflore
translationtype: Human Translation
ms.sourcegitcommit: 7f469fb309f92b86dbf289d3a0462ba9042af48a
ms.openlocfilehash: d1094e1b350883c206a9180ff71bd8613156360a
ms.lasthandoff: 04/13/2017


---
# <a name="getting-started-with-password-management"></a>Erste Schritte mit der Kennwortverwaltung
> [!IMPORTANT]
> **Sind Sie hier, weil Sie Probleme bei der Anmeldung haben?** Wenn ja, helfen Ihnen die Informationen zum [Ändern und Zurücksetzen Ihres eigenen Kennworts](active-directory-passwords-update-your-own-password.md#reset-my-password)weiter.
>
>

Es sind lediglich einige einfache Schritte erforderlich, um Ihren Benutzern das Verwalten eigener Cloudkennwörter für Azure Active Directory oder lokaler Kennwörter für Active Directory zu ermöglichen. Nachdem Sie sichergestellt haben, dass einige wenige Voraussetzungen erfüllt sind, haben Sie im Handumdrehen eine Kennwortänderung und -zurücksetzung für Ihre gesamte Organisation eingerichtet. In diesem Artikel lernen Sie die folgenden Konzepte kennen:

* [**Die besten Tipps von unseren Kunden für den Einstieg**](#top-tips-from-our-customers-to-read-before-you-begin)
 * [**TOP-TIPP: NAVIGATION DER DOKUMENTATION** – verwenden Sie unser Inhaltsverzeichnis und die Suchfunktion Ihres Browsers, um Antworten zu finden.](#top-tip-documentation-navigation---use-our-table-of-contents-and-your-browsers-find-feature-to-find-answers)
 * [**Tipp 1: LIZENZIERUNG** – stellen Sie sicher, dass Sie die Lizenzanforderungen verstehen.](#tip-1-licensing---make-sure-you-understand-the-licensing-requirements)
 * [**Tipp 2: TESTEN** - testen Sie nicht mit einem Administrator, sondern einem Endbenutzer, und führen Sie das Pilotprojekt mit einer kleinen Gruppe von Benutzern durch.](#tip-2-testing---test-with-an-end-user-not-an-administrator-and-pilot-with-a-small-set-of-users)
 * [**Tipp 3: BEREITSTELLUNG** – füllen Sie Daten für Ihre Benutzer vorab aus, damit sie sich nicht registrieren müssen.](#tip-3-deployment---pre-populate-data-for-your-users-so-they-dont-have-to-register)
 * [**Tipp 4: BEREITSTELLUNG** – verwenden Sie die Kennwortzurücksetzung, damit keine temporären Kennwörter weitergegeben werden müssen.](#tip-4-deployment---use-password-reset-to-obviate-the-need-to-communicate-temporary-passwords)
 * [**Tipp 5: RÜCKSCHREIBEN** – sehen Sie sich das Anwendungsereignisprotokoll auf dem AAD Connect-Computer an, um Probleme mit dem Rückschreiben von Kennwörtern zu beheben.](#tip-5-writeback---look-at-the-application-event-log-on-your-aad-connect-machine-to-troubleshoot-password-writeback)
 * [**Tipp 6: RÜCKSCHREIBEN** – stellen Sie sicher, dass die richtigen Berechtigungen, Firewallregeln und Verbindungseinstellungen für das Rückschreiben von Kennwörtern festgelegt sind.](#tip-6-writeback---ensure-you-enable-the-correct-permissions-firewall-rules-and-connection-settings-for-password-writeback)
 * [**Tipp 7: Berichterstellung** – ermitteln Sie anhand der Azure AD SSPR-Überwachungsprotokolle, wer sich registriert oder Kennwörter zurücksetzt.](#tip-7-reporting---see-who-is-registering-or-resetting-passwords-with-the-azure-ad-sspr-audit-logs)
 * [**Tipp 8: PROBLEMBEHANDLUNG** – lesen Sie unser Handbuch und die häufig gestellten Fragen zur Problembehandlung, um die Lösung für zahlreiche Probleme zu finden.](#tip-8-troubleshoot---read-our-troubleshooting-guide-and-faq-to-solve-many-issues)
 * [**Tipp 9: PROBLEMBEHANDLUNG** – wenn Sie weitere Unterstützung benötigen, stellen Sie uns genügend Informationen zur Verfügung, damit wir Ihnen helfen können.](#tip-9-troubleshoot---if-you-still-need-help-include-enough-information-for-us-to-assist-you)
* [**Aktivieren von Benutzern für das Zurücksetzen ihrer Azure Active Directory-Kennwörter**](#enable-users-to-reset-their-azure-ad-passwords)
 * [Voraussetzungen für die Self-Service-Kennwortzurücksetzung](#prerequisites)
 * [Schritt 1: Konfigurieren der Richtlinie für die Kennwortzurücksetzung](#step-1-configure-password-reset-policy)
 * [Schritt 2: Hinzufügen von Kontaktdaten für Ihren Testbenutzer](#step-2-add-contact-data-for-your-test-user)
 * [Schritt 3: Zurücksetzen Ihres Kennworts als Benutzer](#step-3-reset-your-azure-ad-password-as-a-user)
* [**Aktivieren von Benutzern für das Zurücksetzen oder Ändern ihrer lokalen Active Directory-Kennwörter**](#enable-users-to-reset-or-change-their-ad-passwords)
 * [Voraussetzungen für das Zurückschreiben von Kennwörtern](#writeback-prerequisites)
 * [Schritt 1: Herunterladen der aktuellen Version von Azure AD Connect](#step-1-download-the-latest-version-of-azure-ad-connect)
 * [Schritt 2: Aktivieren der Kennwortrückschreibung in Azure AD Connect über die Benutzeroberfläche oder PowerShell mit anschließender Überprüfung](#step-2-enable-password-writeback-in-azure-ad-connect)
 * [Schritt 3: Konfigurieren der Firewall](#step-3-configure-your-firewall)
 * [Schritt 4: Einrichten der geeigneten Berechtigungen](#step-4-set-up-the-appropriate-active-directory-permissions)
 * [Schritt 5: Zurücksetzen Ihres AD-Kennworts als Benutzer mit anschließender Überprüfung](#step-5-reset-your-ad-password-as-a-user)

## <a name="top-tips-from-our-customers-to-read-before-you-begin"></a>Die besten Tipps von unseren Kunden für den Einstieg
Nachfolgend finden Sie einige der besten Tipps, die sich für Kunden bei der Bereitstellung der Kennwortverwaltung in ihrer Organisation als hilfreich erwiesen haben.

* [**TOP-TIPP: NAVIGATION DER DOKUMENTATION** – verwenden Sie unser Inhaltsverzeichnis und die Suchfunktion Ihres Browsers, um Antworten zu finden.](#top-tip-documentation-navigation---use-our-table-of-contents-and-your-browsers-find-feature-to-find-answers)
* [**Tipp 1: LIZENZIERUNG** – stellen Sie sicher, dass Sie die Lizenzanforderungen verstehen.](#tip-1-licensing---make-sure-you-understand-the-licensing-requirements)
* [**Tipp 2: TESTEN** - testen Sie nicht mit einem Administrator, sondern einem Endbenutzer, und führen Sie das Pilotprojekt mit einer kleinen Gruppe von Benutzern durch.](#tip-2-testing---test-with-an-end-user-not-an-administrator-and-pilot-with-a-small-set-of-users)
* [**Tipp 3: BEREITSTELLUNG** – füllen Sie Daten für Ihre Benutzer vorab aus, damit sie sich nicht registrieren müssen.](#tip-3-deployment---pre-populate-data-for-your-users-so-they-dont-have-to-register)
* [**Tipp 4: BEREITSTELLUNG** – verwenden Sie die Kennwortzurücksetzung, damit keine temporären Kennwörter weitergegeben werden müssen.](#tip-4-deployment---use-password-reset-to-obviate-the-need-to-communicate-temporary-passwords)
* [**Tipp 5: RÜCKSCHREIBEN** – sehen Sie sich das Anwendungsereignisprotokoll auf dem AAD Connect-Computer an, um Probleme mit dem Rückschreiben von Kennwörtern zu beheben.](#tip-5-writeback---look-at-the-application-event-log-on-your-aad-connect-machine-to-troubleshoot-password-writeback)
* [**Tipp 6: RÜCKSCHREIBEN** – stellen Sie sicher, dass die richtigen Berechtigungen, Firewallregeln und Verbindungseinstellungen für das Rückschreiben von Kennwörtern festgelegt sind.](#tip-6-writeback---ensure-you-enable-the-correct-permissions-firewall-rules-and-connection-settings-for-password-writeback)
* [**Tipp 7: Berichterstellung** – ermitteln Sie anhand der Azure AD SSPR-Überwachungsprotokolle, wer sich registriert oder Kennwörter zurücksetzt.](#tip-7-reporting---see-who-is-registering-or-resetting-passwords-with-the-azure-ad-sspr-audit-logs)
* [**Tipp 8: PROBLEMBEHANDLUNG** – lesen Sie unser Handbuch und die häufig gestellten Fragen zur Problembehandlung, um die Lösung für zahlreiche Probleme zu finden.](#tip-8-troubleshoot---read-our-troubleshooting-guide-and-faq-to-solve-many-issues)
* [**Tipp 9: PROBLEMBEHANDLUNG** – wenn Sie weitere Unterstützung benötigen, stellen Sie uns genügend Informationen zur Verfügung, damit wir Ihnen helfen können.](#tip-9-troubleshoot---if-you-still-need-help-include-enough-information-for-us-to-assist-you)

### <a name="top-tip-documentation-navigation---use-our-table-of-contents-and-your-browsers-find-feature-to-find-answers"></a>TOP-TIPP: NAVIGATION DER DOKUMENTATION – verwenden Sie unser Inhaltsverzeichnis und die Suchfunktion Ihres Browsers, um Antworten zu finden.
Wenn Sie eine unserer Dokumentationen verwenden, haben wir uns bemüht, im Inhaltsverzeichnis Quicklinks zu allen interessanten Inhalten mit Informationen für Administratoren bereitzustellen.

Sehen Sie sich das folgende Inhaltsverzeichnis an:
* [Zurücksetzen des Azure AD-Kennworts: Dokumentation – Inhaltsverzeichnis](https://docs.microsoft.com/azure/active-directory/active-directory-passwords)

### <a name="tip-1-licensing---make-sure-you-understand-the-licensing-requirements"></a>Tipp 1: LIZENZIERUNG – stellen Sie sicher, dass Sie die Lizenzanforderungen verstehen.
In Ihrer Organisation muss mindestens eine Lizenz zugewiesen sein, damit die Kennwortzurücksetzung in Azure AD funktioniert. Für die Kennwortzurücksetzung an sich erzwingen wir keine benutzerbasierte Lizenzierung. Wenn Sie die Funktion jedoch für einen Benutzer ohne zugewiesene Lizenz nutzen, gelten die Bestimmungen Ihres Microsoft-Lizenzvertrags als nicht erfüllt, und Sie müssen den entsprechenden Benutzern Lizenzen zuweisen.

In diesen Dokumenten können Sie sich informieren, welche Lizenzen für das Zurücksetzen von Kennwörtern erforderlich sind.
* [Lizenzinformationen zur allgemeinen Kennwortzurücksetzung](https://docs.microsoft.com/azure/active-directory/active-directory-passwords-customize#what-customization-options-are-available)
* [Lizenzinformationen zur featurebasierten Kennwortzurücksetzung](https://docs.microsoft.com/azure/active-directory/active-directory-passwords#pricing-and-availability)
* [Unterstützte Szenarien für die Kennwortrückschreibung](https://docs.microsoft.com/azure/active-directory/active-directory-passwords-learn-more#scenarios-supported-for-password-writeback)

### <a name="tip-2-testing---test-with-an-end-user-not-an-administrator-and-pilot-with-a-small-set-of-users"></a>Tipp 2: TESTEN - testen Sie nicht mit einem Administrator, sondern einem Endbenutzer, und führen Sie das Pilotprojekt mit einer kleinen Gruppe von Benutzern durch.
Beim Testen mit einem Administrator wird die unten definierte Richtlinie zur Administratorkennwortrücksetzung erzwungen.  Dies bedeutet, dass NICHT die erwarteten Ergebnisse der Richtlinie angezeigt werden, die Sie für Ihre Endbenutzer konfiguriert haben.

Die auf der administrativen Benutzeroberfläche konfigurierten Richtlinien gelten NUR für Endbenutzer, nicht für Administratoren. Microsoft erzwingt strikte Standardrichtlinien für die Kennwortzurücksetzung für Ihre Administratoren – die sich u.U. von den Richtlinien unterscheiden, die Sie für Ihre Endbenutzer festlegen – um die Sicherheit Ihrer Organisation zu gewährleisten.

#### <a name="administrator-password-reset-policy"></a>Richtlinie zur Administratorkennwortrücksetzung
* **Gilt für**: alle Administratorrollen (globaler Administrator, Helpdeskadministrator, Kennwortadministrator usw.)
* **Richtlinie für die einstufige Überprüfung gilt...**
 * ... 30 Tage ab dem Start einer Testversion **ODER**
 * ... wenn keine Vanity-Domäne vorhanden ist **UND** Azure AD Connect Identitäten nicht synchronisiert.
 * **_Anforderung_**: Für **eine** der folgenden Informationen muss ein Wert angegeben sein: E-Mail-Adresse für Authentifizierung, alternative E-Mail-Adresse, Telefonnummer für Authentifizierung, Mobiltelefon oder Bürotelefon.
* **Richtlinie für die zweistufige Überprüfung gilt...**
 * ... nach Ablauf der ersten 30 Tage einer Testversion **ODER**
 * ... wenn eine Vanity-Domäne vorhanden ist **ODER**
 * ... wenn Sie Azure AD Connect aktiviert haben, um Identitäten aus Ihrer lokalen Umgebung zu synchronisieren.
 * _**Anforderung**_: Für **zwei** der folgenden Informationen muss ein Wert angegeben sein: E-Mail-Adresse für Authentifizierung, alternative E-Mail-Adresse, Telefonnummer für Authentifizierung, Mobiltelefon oder Bürotelefon.

### <a name="tip-3-deployment---pre-populate-data-for-your-users-so-they-dont-have-to-register"></a>Tipp 3: BEREITSTELLUNG – füllen Sie Daten für Ihre Benutzer vorab aus, damit sie sich nicht registrieren müssen.
Vielen Personen ist nicht bekannt, dass sich Benutzer nicht für die Kennwortzurücksetzung registrieren müssen, um die Funktion zu verwenden.  Wenn Sie im voraus Telefon- oder E-Mail-Eigenschaften für Ihre Benutzer festlegen, können Sie die Kennwortzurücksetzung sofort in Ihrer gesamten Organisation einführen, **ohne dass Ihre Benutzer etwas unternehmen müssen!**

In der folgenden Dokumentation erfahren Sie, wie dazu eine API, PowerShell oder Azure AD Connect verwendet wird:
* [Bereitstellen der Kennwortzurücksetzung ohne erforderliche Benutzerregistrierung](https://docs.microsoft.com/azure/active-directory/active-directory-passwords-learn-more#deploying-password-reset-without-requiring-end-user-registration)
* [Welche Daten werden bei der Kennwortzurücksetzung verwendet?](https://docs.microsoft.com/azure/active-directory/active-directory-passwords-learn-more#what-data-is-used-by-password-reset)

### <a name="tip-4-deployment---use-password-reset-to-obviate-the-need-to-communicate-temporary-passwords"></a>Tipp 4: BEREITSTELLUNG – verwenden Sie die Kennwortzurücksetzung, damit keine temporären Kennwörter weitergegeben werden müssen.
Dies ist eine Ergänzung zu Tipp 3. Sobald Sie Ihre Benutzer für das Zurücksetzen von Kennwörtern konfiguriert haben, stellen Sie sich ein Szenario vor, bei dem ein Mitarbeiter in Ihr Unternehmen eintritt. Anstatt ihm das temporäre Kennwort mitzuteilen, können Sie den Mitarbeiter einfach anweisen, zum [Azure AD-Kennwortzurücksetzungsportal](https://passwordreset.microsoftonline.com) zu navigieren und seine Kennwörter zurückzusetzen.

Wenn der Benutzer ein [Windows 10-Gerät verwendet, das in eine Azure AD-Domäne eingebunden ist](https://docs.microsoft.com/azure/active-directory/active-directory-azureadjoin-devices-group-policy), ist dies direkt auf dem standardmäßigen Windows 10-Anmeldebildschirm möglich, sodass der Benutzer ohne Ihr Eingreifen den Zugriff auf einen völlig neuen PC erhält.

In der folgenden Dokumentation erfahren Sie, wie dazu eine API, PowerShell oder Azure AD Connect verwendet wird. Nachdem Sie diese Daten vorab ausgefüllt haben, weisen Sie einfach Ihre Benutzer zum Zurücksetzen ihrer Kennwörter an, und sie erhalten sofort Zugriff auf ihre jeweiligen Konten:
* [Bereitstellen der Kennwortzurücksetzung ohne erforderliche Benutzerregistrierung](https://docs.microsoft.com/azure/active-directory/active-directory-passwords-learn-more#deploying-password-reset-without-requiring-end-user-registration)
* [Welche Daten werden bei der Kennwortzurücksetzung verwendet?](https://docs.microsoft.com/azure/active-directory/active-directory-passwords-learn-more#what-data-is-used-by-password-reset)

### <a name="tip-5-writeback---look-at-the-application-event-log-on-your-aad-connect-machine-to-troubleshoot-password-writeback"></a>Tipp 5: RÜCKSCHREIBEN – sehen Sie sich das Anwendungsereignisprotokoll auf dem AAD Connect-Computer an, um Probleme mit dem Rückschreiben von Kennwörtern zu beheben.
Das Azure AD Connect-Anwendungsereignisprotokoll enthält umfassende Protokollierungsinformationen, in denen ein Großteil der Vorgänge im Zusammenhang mit dem Dienst zum Rückschreiben von Kennwörtern in Echtzeit beschrieben wird. Führen Sie die folgenden Schritte aus, um Zugriff auf dieses Protokoll zu erhalten:

1. Melden Sie sich bei Ihrem **Azure AD Connect**-Computer an.
2. Öffnen Sie die **Windows-Ereignisanzeige** durch Drücken von **Starten** und Eingeben von **„Ereignisanzeige“**.
3. Öffnen Sie das **Anwendungsereignisprotokoll**.
4. Suchen Sie nach Ereignissen aus den folgenden Quellen: **PasswordResetService** oder **ADSync**, um weitere Informationen zu einem potenziellen Problem zu erhalten.

Eine vollständige Liste der Ereignisse, die dieses Protokoll enthalten kann, sowie einige weitere Hinweise zur Problembehandlung beim Kennwortrückschreiben finden Sie unter:
* [Problembehandlung: Kennwortrückschreiben](https://docs.microsoft.com/azure/active-directory/active-directory-passwords-troubleshoot#troubleshoot-password-writeback)
* [Ereignisprotokoll-Fehlercodes für das Rückschreiben](https://docs.microsoft.com/azure/active-directory/active-directory-passwords-troubleshoot#password-writeback-event-log-error-codes)
* [Problembehandlung: Konnektivität beim Kennwortrückschreiben](https://docs.microsoft.com/azure/active-directory/active-directory-passwords-troubleshoot#troubleshoot-password-writeback-connectivity)
* [Bereitstellung der Rückschreibung – Schritt 3: Konfigurieren der Firewall](#step-3-configure-your-firewall)
* [Bereitstellung der Rückschreibung – Schritt 4: Einrichten der geeigneten Berechtigungen](#step-4-set-up-the-appropriate-active-directory-permissions)

### <a name="tip-6-writeback---ensure-you-enable-the-correct-permissions-firewall-rules-and-connection-settings-for-password-writeback"></a>Tipp 6: RÜCKSCHREIBEN – stellen Sie sicher, dass die richtigen Berechtigungen, Firewallregeln und Verbindungseinstellungen für das Rückschreiben von Kennwörtern festgelegt sind.
Damit das Rückschreiben ordnungsgemäß funktioniert, müssen Sie Folgendes sicherstellen:

1. Die richtigen **Active Directory-Berechtigungen** für Benutzer wurden mithilfe der Funktion für die Kennwortrückschreibung festgelegt, sodass sie berechtigt sind, ihre eigenen Kennwörter und Kennzeichen zum Entsperren von Konten in AD zu ändern.
2. Die richtigen **Firewallports** wurden geöffnet, damit die Kennwortrückschreibung über eine ausgehende Verbindung sicher mit der Außenwelt kommunizieren kann.
3. Die richtigen **Firewallausnahmen** wurden für wichtige URLs des Diensts zum Zurücksetzen von Kennwörtern festgelegt, z.B. Service Bus.
4. Ihr **Proxy und Ihre Firewall trennen ausgehende Verbindungen im Leerlauf nicht**. Wir empfehlen mindestens 10 Minuten.

Eine vollständige Liste der Anleitungen zur Problembehandlung und spezifische Anleitungen für das Konfigurieren von Berechtigungen und Firewallregeln für das Rückschreiben von Kennwörtern finden Sie unter:
* [Problembehandlung: Kennwortrückschreiben](https://docs.microsoft.com/azure/active-directory/active-directory-passwords-troubleshoot#troubleshoot-password-writeback)
* [Ereignisprotokoll-Fehlercodes für das Rückschreiben](https://docs.microsoft.com/azure/active-directory/active-directory-passwords-troubleshoot#password-writeback-event-log-error-codes)
* [Problembehandlung: Konnektivität beim Kennwortrückschreiben](https://docs.microsoft.com/azure/active-directory/active-directory-passwords-troubleshoot#troubleshoot-password-writeback-connectivity)
* [Bereitstellung der Rückschreibung – Schritt 3: Konfigurieren der Firewall](#step-3-configure-your-firewall)
* [Bereitstellung der Rückschreibung – Schritt 4: Einrichten der geeigneten Berechtigungen](#step-4-set-up-the-appropriate-active-directory-permissions)

### <a name="tip-7-reporting---see-who-is-registering-or-resetting-passwords-with-the-azure-ad-sspr-audit-logs"></a>Tipp 7: Berichterstellung – ermitteln Sie anhand der Azure AD SSPR-Überwachungsprotokolle, wer sich registriert oder Kennwörter zurücksetzt.
Sobald die Kennwortrückschreibung bereitgestellt wurde und funktioniert, ist der nächste logische Schritt, sie auszuführen und zu analysieren, wer sich noch registrieren muss, welche Probleme beim Zurücksetzen häufig auftreten, und den Nutzen Ihrer Investition in die Funktion zu ermitteln.

Mithilfe der Überwachungsprotokolle für die Kennwortzurücksetzung in Azure AD haben Sie diese und viele weitere Möglichkeiten im Azure-Portal, über PowerBI, die Azure AD-Berichterstellungs-API für Ereignisse oder über PowerShell.  Weitere Informationen zur Verwendung dieser Berichterstellungsfunktionen finden Sie unter:
* [Übersicht über Berichte zur Kennwortverwaltung](https://docs.microsoft.com/azure/active-directory/active-directory-passwords-get-insights#overview-of-password-management-reports)
* [Anzeigen von Kennwortverwaltungsberichten im Azure-Portal](https://docs.microsoft.com/azure/active-directory/active-directory-passwords-get-insights#how-to-view-password-management-reports)
* [Aktivitätstypen für die Self-Service-Kennwortverwaltung im Azure-Portal](https://docs.microsoft.com/azure/active-directory/active-directory-passwords-get-insights#self-service-password-management-activity-types-in-the-new-azure-portal)
* [Abrufen von Kennwortverwaltungsereignissen von der API für Azure AD-Berichte und -Ereignisse](https://docs.microsoft.com/azure/active-directory/active-directory-passwords-get-insights#how-to-retrieve-password-management-events-from-the-azure-ad-reports-and-events-api)
* [Schnelles Herunterladen von Ereignissen im Zusammenhang mit der Registrierung für die Kennwortzurücksetzung mit PowerShell](https://docs.microsoft.com/azure/active-directory/active-directory-passwords-get-insights#how-to-download-password-reset-registration-events-quickly-with-powershell)

### <a name="tip-8-troubleshoot---read-our-troubleshooting-guide-and-faq-to-solve-many-issues"></a>Tipp 8: PROBLEMBEHANDLUNG – lesen Sie unser Handbuch und die häufig gestellten Fragen zur Problembehandlung, um die Lösung für zahlreiche Probleme zu finden.
Wussten Sie, dass für die Kennwortzurücksetzung umfangreiche Anleitungen zur Problembehandlung sowie häufig gestellte Fragen verfügbar sind? Sollten Sie eine Frage haben, finden Sie die Antwort darauf wahrscheinlich unter den folgenden Links.

Außerdem finden Sie im [Azure-Portal](https://portal.azure.com) auf dem Blatt **„Support und Problembehandlung“** umfassenden Inhalt zur Problembehandlung. Dieses befindet sich direkt auf der administrativen Benutzeroberfläche im linken Navigationsbereich unter **Azure Active Directory** -> **Benutzer und Gruppen** -> **Kennwortzurücksetzung** -> **Support und Problembehandlung**.

Links zu den Anleitungen zur Problembehandlung und häufig gestellten Fragen zur Kennwortzurücksetzung:
* [Problembehandlung für die Kennwortverwaltung](https://docs.microsoft.com/azure/active-directory/active-directory-passwords-troubleshoot)
* [Häufig gestellte Fragen zur Kennwortverwaltung](https://docs.microsoft.com/azure/active-directory/active-directory-passwords-faq)

### <a name="tip-9-troubleshoot---if-you-still-need-help-include-enough-information-for-us-to-assist-you"></a>Tipp 9: PROBLEMBEHANDLUNG – wenn Sie weitere Unterstützung benötigen, stellen Sie uns genügend Informationen zur Verfügung, damit wir Ihnen helfen können.
Wenn Sie weitere Hilfe bei der Problembehandlung benötigen, sind wir für Sie da. Sie können entweder eine Supportanfrage erstellen oder sich an Ihr Kundenbetreuungsteam wenden, um einen direkten Kontakt mit uns herzustellen. Wir freuen uns darauf, von Ihnen zu hören.

Bevor Sie sich an uns wenden, **stellen Sie bitte sicher, alle unten angeforderten Informationen zu sammeln**, damit wir Ihnen schnell helfen!
* [Erforderliche Informationen beim Anfordern von Hilfe](https://docs.microsoft.com/azure/active-directory/active-directory-passwords-troubleshoot#information-to-include-when-you-need-help)

#### <a name="ways-to-provide-password-reset-feedback"></a>Methoden zur Bereitstellung von Feedback zur Kennwortzurücksetzung
* [Funktionsanfragen oder Problembehandlung: Verfassen Sie einen Beitrag in den Azure AD-MSDN-Foren.](https://social.msdn.microsoft.com/Forums/azure/home?forum=WindowsAzureAD)
* [Funktionsanfragen oder Problembehandlung: Verfassen Sie einen Beitrag in StackOverflow.](http://stackoverflow.com/questions/tagged/azure-active-directory)
* [Funktionsanfragen oder Problembehandlung: Verfassen Sie einen Tweet@azuread!](https://twitter.com/azuread)
* [Funktionsanfragen oder Problembehandlung: Hinterlassen Sie eine Nachricht in UserVoice.](https://feedback.azure.com/forums/169401-azure-active-directory)

## <a name="enable-users-to-reset-their-azure-ad-passwords"></a>Aktivieren von Benutzern für das Zurücksetzen ihrer Azure AD-Kennwörter
In diesem Abschnitt werden Sie durch den Vorgang zum Aktivieren der Self-Service-Kennwortzurücksetzung für Ihr AAD-Cloudverzeichnis, das Registrieren von Benutzern für die Self-Service-Kennwortzurücksetzung und das Testen einer Self-Service-Kennwortzurücksetzung als ein Benutzer geleitet.

* [Voraussetzungen für die Self-Service-Kennwortzurücksetzung](#prerequisites)
* [Schritt 1: Konfigurieren der Richtlinie für die Kennwortzurücksetzung](#step-1-configure-password-reset-policy)
* [Schritt 2: Hinzufügen von Kontaktdaten für Ihren Testbenutzer](#step-2-add-contact-data-for-your-test-user)
* [Schritt 3: Zurücksetzen Ihres Kennworts als Benutzer](#step-3-reset-your-azure-ad-password-as-a-user)

### <a name="prerequisites"></a>Voraussetzungen
Bevor Sie die Self-Service-Kennwortzurücksetzung aktivieren und verwenden können, müssen Sie die folgenden Voraussetzungen erfüllen:

* Erstellen Sie einen AAD-Mandanten. Weitere Informationen finden Sie unter [Erste Schritte mit Azure AD](https://azure.microsoft.com/trial/get-started-active-directory/)
* Schließen Sie ein Azure-Abonnement ab. Weitere Informationen finden Sie unter [Was ist ein Azure AD-Mandant?](active-directory-administer.md#what-is-an-azure-ad-tenant).
* Ordnen Sie Ihren AAD-Mandanten Ihrem Azure-Abonnement zu. Weitere Informationen zu Azure-Abonnements finden Sie unter [Wie Azure-Abonnements mit Azure AD verknüpft sind](https://msdn.microsoft.com/library/azure/dn629581.aspx).
* Führen Sie ein Upgrade auf Azure AD Premium oder Basic durch, oder verwenden Sie eine kostenpflichtige O365-Lizenz. Weitere Informationen finden Sie unter [Azure Active Directory-Editionen](https://azure.microsoft.com/pricing/details/active-directory/).

  > [!NOTE]
  > Um die Self-Service-Kennwortzurücksetzung für Cloudbenutzer zu aktivieren, müssen Sie ein Upgrade auf Azure AD Premium, Azure AD Basic oder eine kostenpflichtige O365-Lizenz durchführen.  Um die Self-Service-Kennwortzurücksetzung für Ihre lokalen Benutzer zu aktivieren, müssen Sie ein Upgrade auf Azure AD Premium durchführen. Weitere Informationen finden Sie unter [Azure Active Directory-Editionen](https://azure.microsoft.com/pricing/details/active-directory/). Hier finden Sie u. a. ausführliche Informationen dazu, wie Sie sich für Azure AD Premium oder Basic registrieren, wie Sie Ihren Lizenzplan und den Azure AD-Zugriff aktivieren, und wie Sie Administratoren und Benutzerkonten Zugriff gewähren.
  >
  >
* Erstellen Sie mindestens ein Administratorkonto und ein Benutzerkonto in Ihrem AAD-Verzeichnis.
* Weisen Sie dem erstellten Administrator- und Benutzerkonto eine AAD Premium-, Basic- oder kostenpflichtige O365-Lizenz zu.

### <a name="step-1-configure-password-reset-policy"></a>Schritt 1: Konfigurieren der Richtlinie für die Kennwortzurücksetzung
Führen Sie die folgenden Schritte aus, um die Richtlinie für das Zurücksetzen von Benutzerkennwörtern zu konfigurieren:

1. Öffnen Sie einen Browser Ihrer Wahl, und wechseln Sie zum [klassischen Azure-Portal](https://manage.windowsazure.com).
2. Suchen Sie im [klassischen Azure-Portal](https://manage.windowsazure.com) auf der Navigationsleiste links nach der **Active Directory-Erweiterung**.

   ![Kennwortverwaltung in Azure AD][001]
3. Klicken Sie unterhalb der Registerkarte **Verzeichnis** auf das Verzeichnis, in dem Sie die Richtlinie für das Zurücksetzen von Benutzerkennwörtern konfigurieren möchten, beispielsweise Wingtip Toys.

    ![][002]
4. Klicken Sie auf die Registerkarte **Configure** .

   ![][003]

5. Scrollen Sie unterhalb der Registerkarte **Konfigurieren** bis zum Abschnitt **Richtlinie zum Zurücksetzen des Benutzerkennworts**.  Hier können Sie alle Aspekte der Richtlinie zum Zurücksetzen von Benutzerkennwörtern für ein vorgegebenes Verzeichnis konfigurieren. *Falls die Registerkarte „Konfigurieren“ nicht angezeigt wird, vergewissern Sie sich, dass Sie sich für Azure Active Directory Premium oder Basic registriert und dem Administratorkonto, das zum Konfigurieren des Features verwendet wird, eine __Lizenz zugewiesen__ haben.*  

   > [!NOTE]
   > **Die von Ihnen festgelegte Richtlinie gilt nur für Endbenutzer in Ihrer Organisation, nicht für Administratoren**. Aus Sicherheitsgründen steuert Microsoft die Richtlinie zum Zurücksetzen von Administratorkennwörtern. Für die aktuelle Richtlinie für Administratoren sind zwei Abfragen erforderlich: Mobiltelefon und E-Mail-Adresse.

   ![][004]
6. Legen Sie zum Konfigurieren der Richtlinie für das Zurücksetzen von Benutzerkennwörtern den Umschalter **Benutzer sind für das Zurücksetzen des Kennworts aktiviert** auf **Ja** fest.  Es werden verschiedene weitere Steuerelemente angezeigt, mit deren Hilfe Sie die Funktionsweise dieses Features in Ihrem Verzeichnis konfigurieren können.  Passen Sie die Kennwortzurücksetzung nach Ihren Anforderungen an.  Wenn Sie mehr über die verschiedenen Steuerelemente zum Anpassen der Richtlinie für die Kennwortzurücksetzung wissen möchten, lesen Sie den Artikel [Anpassen: Azure AD-Kennwortverwaltung](active-directory-passwords-customize.md).

   ![][005]
7. Nachdem Sie die Richtlinie für die Kennwortzurücksetzung wie gewünscht für Ihren Mandanten konfiguriert haben, klicken Sie im unteren Bereich des Bildschirms auf **Speichern** .

   > [!NOTE]
   > Es wird empfohlen, eine Richtlinie für die Kennwortzurücksetzung mit einer Überprüfung in zwei Schritten zu verwenden, damit Sie sehen können, wie die Richtlinie in sehr komplexen Szenarien funktioniert.
   >
   >

   ![][006]

   > [!NOTE]
   > **Die von Ihnen festgelegte Richtlinie gilt nur für Endbenutzer in Ihrer Organisation, nicht für Administratoren**. Aus Sicherheitsgründen steuert Microsoft die Richtlinie zum Zurücksetzen von Administratorkennwörtern. Für die aktuelle Richtlinie für Administratoren sind zwei Abfragen erforderlich: Mobiltelefon und E-Mail-Adresse.
   >
   >

### <a name="step-2-add-contact-data-for-your-test-user"></a>Schritt 2: Hinzufügen von Kontaktdaten für Ihren Testbenutzer
Sie haben mehrere Möglichkeiten, für Organisationsbenutzer Daten einzugeben, die für das Zurücksetzen von Kennwörtern verwendet werden.

* Bearbeiten Sie Benutzer im [klassischen Azure-Portal](https://manage.windowsazure.com) oder im [Office 365-Verwaltungsportal](https://portal.microsoftonline.com).
* Verwenden Sie AAD Connect, um Benutzereigenschaften aus einer lokalen Active Directory-Domäne mit Azure AD zu synchronisieren.
* Verwenden Sie Windows PowerShell zum Bearbeiten der Benutzereigenschaften.
* Ermöglichen Sie es Benutzern, ihre eigenen Daten zu registrieren, indem Sie sie auf das Registrierungsportal unter [http://aka.ms/ssprsetup](http://aka.ms/ssprsetup)
* Legen Sie die Benutzerregistrierung für die Kennwortzurücksetzung bei Anmeldung beim Zugriffsbereich unter [http://myapps.microsoft.com](http://myapps.microsoft.com) als erforderlich fest, indem Sie die Option für die SSPR-Konfiguration **Sollen sich Benutzer bei der Anmeldung beim Zugriffsbereich registrieren müssen?** auf **Ja** festlegen.

Weitere Informationen dazu, welche Daten beim Zurücksetzen von Kennwörtern verwendet werden und welche Formatierungsanforderungen für diese Daten gelten, finden Sie unter [Welche Daten werden bei der Kennwortzurücksetzung verwendet?](active-directory-passwords-learn-more.md#what-data-is-used-by-password-reset).

#### <a name="to-add-user-contact-data-via-the-user-registration-portal"></a>So fügen Sie über das Benutzerregistrierungsportal Benutzerkontaktdaten hinzu
1. Um das Registrierungsportal für die Kennwortzurücksetzung zu verwenden, müssen Sie für die Benutzer in Ihrer Organisation einen Link auf diese Seite bereitstellen ([http://aka.ms/ssprsetup](http://aka.ms/ssprsetup)) oder die Option zur Anforderung einer automatischen Benutzerregistrierung aktivieren.  Wenn die Benutzer auf diesen Link klicken, werden sie aufgefordert, sich mit ihrem Organisationskonto anzumelden.  Anschließend wird die folgende Seite angezeigt:

   ![][007]
2. Hier können Benutzer ihre Mobiltelefonnummer, eine alternative E-Mail-Adresse oder Sicherheitsfragen angeben.  Der folgende Screenshot zeigt Überprüfung mit einem Mobiltelefon.

   ![][008]
3. Nachdem ein Benutzer diese Informationen angegeben hat, wird die Seite aktualisiert um anzuzeigen, dass die Informationen gültig sind (nachstehend geschwärzt).  Durch Klicken auf die Schaltflächen **Fertig stellen** oder **Abbrechen** wird der Benutzer an den Zugriffsbereich weitergeleitet.

   ![][009]
4. Nachdem ein Benutzer die Informationen bestätigt hat, wird das Profil mit den bereitgestellten Daten aktualisiert.  In diesem Beispiel wurde die Nummer für **Bürotelefon** manuell eingegeben, deshalb kann der Benutzer diese Nummer auch als Kontaktmethode für das Zurücksetzen des Kennworts verwenden.

   ![][010]

### <a name="step-3-reset-your-azure-ad-password-as-a-user"></a>Schritt 3: Zurücksetzen Ihres AD-Kennworts als Benutzer mit anschließender Überprüfung
Nachdem Sie eine Richtlinie für die Kennwortzurücksetzung konfiguriert und Kontaktdaten für Ihren Benutzer angegeben haben, kann dieser Benutzer eine Self-Service-Kennwortzurücksetzung durchführen.

#### <a name="to-perform-a-self-service-password-reset"></a>So führen Sie eine Self-Service-Kennwortzurücksetzung durch
1. Wenn Sie zu einer Website wie [**portal.microsoftonline.com**](http://portal.microsoftonline.com) wechseln, wird ein Anmeldebildschirm wie in der folgenden Abbildung angezeigt.  Klicken Sie auf den Link **Können Sie nicht auf Ihr Konto zugreifen?** , um die Benutzeroberfläche für die Kennwortzurücksetzung zu testen.

   ![][011]
2. Nach dem Klicken auf **Können Sie nicht auf Ihr Konto zugreifen?** werden Sie auf eine neue Seite umgeleitet, auf der Sie gefragt werden, für welche **Benutzer-ID** Sie ein Kennwort zurücksetzen möchten.  Geben Sie hier Ihre Test-**Benutzer-ID** ein, führen Sie die Captchaprüfung aus, und klicken Sie auf **Weiter**.

   ![][012]
3. Da der Benutzer in diesem Fall ein **Bürotelefon**, ein **Mobiltelefon** und eine **alternative E-Mail-Adresse** angegeben hat, werden dem Benutzer für die erste Überprüfung alle diese Optionen angeboten.

   ![][013]
4. Wählen Sie in diesem Fall zunächst die Option für einen **Anruf** beim **Bürotelefon** aus.  Beachten Sie, dass der Benutzer bei Auswahl einer telefonbasierten Methode zunächst gebeten wird, die **Telefonnummer zu überprüfen** , bevor das Kennwort zurückgesetzt werden kann.  Auf diese Weise wird verhindert, dass böswillige Benutzer die Telefonnummern von Benutzern in Ihrer Organisation nicht für ein Spamming missbrauchen.

   ![][014]
5. Nachdem der Benutzer die Telefonnummer bestätigt hat, erscheint nach Auswahl der Anrufoption ein Drehfeld, und das Telefon des Benutzers klingelt.  Sobald der Benutzer den Hörer abhebt, wird eine Nachricht abgespielt. In dieser wird der Benutzer aufgefordert, zum Bestätigen des Kontos **die #-Taste zu drücken**.  Durch das Drücken der #-Taste wird automatisch bestätigt, dass der Benutzer den ersten Überprüfungsschritt erfolgreich ausgeführt hat, und die Benutzeroberfläche wechselt zum zweiten Überprüfungsschritt. 

   ![][015]
6. Nachdem Sie den ersten Überprüfungsschritt erfolgreich ausgeführt haben, wird die Benutzeroberfläche automatisch aktualisiert, um die erste Überprüfungsmethode aus der Auswahlliste zu entfernen.  Da Sie in diesem Fall Ihr **Bürotelefon** für den ersten Überprüfungsschritt verwendet haben, verbleiben nur **Mobiltelefon** und **Alternative E-Mail-Adresse** als Optionen für den zweiten Überprüfungsschritt.  Klicken Sie auf die E-Mail-Option **E-Mail an meine alternative E-Mail-Adresse senden** .  Nachdem Sie dies getan haben, wird beim Klicken auf "E-Mail" eine E-Mail an die alternative E-Mail-Adresse gesendet.

   ![][016]
7. Nachstehend sehen Sie ein Beispiel für eine E-Mail, die der Benutzer erhält. Beachten Sie hierbei das Branding für den Mandanten:

   ![][017]
8. Nach dem Empfang der E-Mail wird die Seite aktualisiert, und Sie können den in der E-Mail enthaltenen Code in das nachstehend gezeigte Textfeld eingeben.  Nach der erfolgreichen Eingabe des Codes wird die Schaltfläche "Weiter" angezeigt, und Sie können den zweiten Überprüfungsschritt abschließen.

   ![][018]
9. Nachdem Sie die Anforderungen der Organisationsrichtlinie erfüllt haben, sind Sie jetzt berechtigt, ein neues Kennwort auszuwählen.  Das Kennwort muss die AAD-Anforderungen für sichere Kennwörter erfüllen (siehe [Kennwortrichtlinie in Azure AD](https://msdn.microsoft.com/library/azure/jj943764.aspx)). Ein Indikator für die Kennwortsicherheit zeigt dem Benutzer an, ob das eingegebene Kennwort dieser Richtlinie entspricht.

   ![][019]
10. Nachdem Sie übereinstimmende Kennwörter eingegeben haben, die der Organisationsrichtlinie entsprechen, wird das Kennwort zurückgesetzt, und Sie können sich sofort mit dem neuen Kennwort anmelden.

    ![][020]

## <a name="enable-users-to-reset-or-change-their-ad-passwords"></a>Aktivieren von Benutzern für das Zurücksetzen ihrer AD-Kennwörter
In diesem Abschnitt werden Sie durch den Vorgang zum Konfigurieren der Kennwortzurücksetzung für das Zurückschreiben von Kennwörtern in ein lokales Active Directory-Verzeichnis geleitet.

* [Voraussetzungen für das Zurückschreiben von Kennwörtern](#writeback-prerequisites)
* [Schritt 1: Herunterladen der aktuellen Version von Azure AD Connect](#step-1-download-the-latest-version-of-azure-ad-connect)
* [Schritt 2: Aktivieren der Kennwortrückschreibung in Azure AD Connect über die Benutzeroberfläche oder PowerShell mit anschließender Überprüfung](#step-2-enable-password-writeback-in-azure-ad-connect)
* [Schritt 3: Konfigurieren der Firewall](#step-3-configure-your-firewall)
* [Schritt 4: Einrichten der geeigneten Berechtigungen](#step-4-set-up-the-appropriate-active-directory-permissions)
* [Schritt 5: Zurücksetzen Ihres AD-Kennworts als Benutzer mit anschließender Überprüfung](#step-5-reset-your-ad-password-as-a-user)

### <a name="writeback-prerequisites"></a>Voraussetzungen für das Zurückschreiben von Kennwörtern
Bevor Sie das Zurückschreiben von Kennwörtern aktivieren und verwenden können, müssen Sie die folgenden Voraussetzungen erfüllen:

* Sie verfügen über einen Azure AD-Mandanten, für den Azure AD Premium aktiviert ist.  Weitere Informationen finden Sie unter [Azure Active Directory-Editionen](active-directory-editions.md).
* Sie müssen für Azure AD Connect eine Verbindung mit dem primären Domänencontrolleremulator herstellen, damit das Kennwortrückschreiben funktioniert.  Wenn Sie möchten, können Sie Azure AD Connect für die Verwendung eines primären Domänencontrollers konfigurieren, indem Sie mit der rechten Maustaste auf die **Eigenschaften** des Active Directory-Synchronisierungsconnectors klicken und die Option **configure directory partitions** (Verzeichnispartitionen konfigurieren) wählen. Suchen Sie nach dem Abschnitt **domain controller connection settings** (Domänencontroller-Verbindungseinstellungen), und aktivieren Sie das Kontrollkästchen **only use preferred domain controllers** (Nur bevorzugte Domänencontroller verwenden).  Hinweis: Wenn der bevorzugte DC kein PDC-Emulator ist, nutzt Azure AD Connect trotzdem den PDC für das Kennwortrückschreiben.
* Für Ihren Mandanten wurde die Kennwortzurücksetzung konfiguriert und aktiviert.  Weitere Informationen finden Sie unter [Aktivieren von Benutzern für das  Zurücksetzen ihrer Azure AD-Kennwörter](#enable-users-to-reset-their-azure-ad-passwords)
* Sie verfügen über mindestens ein Administratorkonto und ein Testbenutzerkonto mit einer Azure AD Premium-Lizenz zum Testen dieser Funktion.  Weitere Informationen finden Sie unter [Azure Active Directory-Editionen](active-directory-editions.md).

  > [!NOTE]
  > Stellen Sie sicher, dass es sich bei dem zum Aktivieren der Kennwortrückschreibung verwendeten Administratorkonto um ein Cloudadministratorkonto handelt (erstellt in Azure AD), und nicht um ein Verbundkonto (erstellt im lokalen AD-Verzeichnis und synchronisiert mit Azure AD).
  >
  >
* Sie verfügen über eine lokale AD-Bereitstellung mit einer oder mehreren Gesamtstrukturen, in der Windows Server 2008, Windows Server 2008 R2, Windows Server 2012 oder Windows Server 2012 R2 mit den aktuellen Service Packs ausgeführt wird.

  > [!NOTE]
  > Wenn Sie eine ältere Version von Windows Server 2008 oder 2008 R2 ausführen, können Sie diese Funktion verwenden, müssen aber [KB 2386717 herunterladen und installieren](https://support.microsoft.com/kb/2386717) , bevor Sie Ihre lokale AD-Kennwortrichtlinie in der Cloud erzwingen können.
  >
  >
* Sie haben Azure AD Connect installiert und Ihre AD-Umgebung für eine Synchronisierung mit der Cloud vorbereitet.  Weitere Informationen finden Sie unter [Verwenden Ihrer lokalen Identitätsinfrastruktur in der Cloud](connect/active-directory-aadconnect.md).

  > [!NOTE]
  > Bevor Sie das Kennwortrückschreiben testen, stellen Sie sicher, dass Sie zuerst einen vollständigen Import und eine vollständige Synchronisierung aus AD und Azure AD in und mit Azure AD Connect ausführen.
  >
  >
* Wenn Sie Azure AD Sync oder Azure AD Connect verwenden, muss **TCP 443** ausgehend (und in einigen Fällen **TCP 9350-9354**) geöffnet werden.  Unter [Schritt 3: Konfigurieren der Firewall](#step-3-configure-your-firewall) finden Sie weitere Informationen. Die Verwendung von DirSync für dieses Szenario wird nicht mehr unterstützt.  Wenn Sie DirSync weiterhin verwenden möchten, aktualisieren Sie auf die neueste Version von Azure AD Connect,  bevor Sie das Zurückschreiben von Kennwörtern bereitstellen.

  > [!NOTE]
  > Es wird dringend empfohlen, bei Verwendung von Azure AD Sync oder DirSync ein Upgrade auf die neueste Version von Azure AD Connect durchzuführen, um eine bestmögliche Funktionalität sicherzustellen und von neuen Features zu profitieren.
  >
  >

### <a name="step-1-download-the-latest-version-of-azure-ad-connect"></a>Schritt 1: Herunterladen der aktuellen Version von Azure AD Connect
Das Zurückschreiben von Kennwörtern steht in Versionen von Azure AD Connect oder im Azure AD Sync-Tool mit Versionsnummer **1.0.0419.0911** oder höher zur Verfügung.  Das Zurückschreiben von Kennwörtern mit automatischer Entsperrung von Konten steht in Versionen von Azure AD Connect oder im Azure AD Sync-Tool mit Versionsnummer **1.0.0485.0222** oder höher zur Verfügung. Wenn Sie eine ältere Version verwenden, führen Sie ein Upgrade auf diese oder eine höhere Version durch, bevor Sie fortfahren. [Klicken Sie hier, um die aktuelle Version von Azure AD Connect herunterzuladen](connect/active-directory-aadconnect.md#install-azure-ad-connect).

#### <a name="to-check-the-version-of-azure-ad-sync"></a>So überprüfen Sie die Version von Azure AD Sync
1. Navigieren Sie zu **%Programme%\Azure Active Directory Sync\**.
2. Suchen Sie nach der ausführbaren Datei **ConfigWizard.exe** .
3. Klicken Sie mit der rechten Maustaste auf die ausführbare Datei, und wählen Sie im Kontextmenü die Option **Eigenschaften** aus.
4. Klicken Sie auf die Registerkarte **Details** .
5. Suchen Sie nach dem Feld **Dateiversion** .

   ![][021]

Wenn die angezeigte Versionsnummer größer oder gleich **1.0.0419.0911** ist oder wenn Sie Azure AD Connect installieren, können Sie zu [Schritt 2: Aktivieren der Kennwortrückschreibung in Azure AD Connect über die Benutzeroberfläche oder PowerShell mit anschließender Überprüfung](#step-2-enable-password-writeback-in-azure-ad-connect) wechseln.

> [!NOTE]
> Wenn Sie Azure AD Connect zum ersten Mal installieren, wird empfohlen, die Best Practices zum Vorbereiten Ihrer Umgebung auf eine Verzeichnissynchronisierung zu befolgen.  Bevor Sie Azure AD Connect installieren, müssen Sie entweder im [Office 365-Verwaltungsportal](https://portal.microsoftonline.com) oder im [klassischen Azure-Portal](https://manage.windowsazure.com) die Verzeichnissynchronisierung aktivieren.  Weitere Informationen finden Sie unter [Verwalten von Azure AD Connect](active-directory-aadconnect-whats-next.md).
>
>

### <a name="step-2-enable-password-writeback-in-azure-ad-connect"></a>Schritt 2: Aktivieren der Kennwortrückschreibung in Azure AD Connect
Nun, da Sie das Azure AD Connect-Tool heruntergeladen haben, können Sie das Zurückschreiben von Kennwörtern aktivieren.  Hierzu stehen zwei Möglichkeiten zur Verfügung.  Sie können das Zurückschreiben von Kennwörtern entweder im Bildschirm für die optionalen Features im Setup-Assistenten für Azure AD Connect aktivieren, oder Sie verwenden zur Aktivierung Windows PowerShell.

#### <a name="to-enable-password-writeback-in-the-configuration-wizard"></a>So aktivieren Sie das Zurückschreiben von Kennwörtern im Konfigurations-Assistenten
1. Öffnen Sie auf Ihrem **Computer für die Verzeichnissynchronisierung** den **Azure AD Connect**-Konfigurations-Assistenten.
2. Klicken Sie durch die Schritte, bis Sie zum Konfigurationsbildschirm **Optionale Features** gelangen.
3. Aktivieren Sie die Option **Kennwort zurückschreiben** .

   ![][022]
4. Schließen Sie den Assistenten ab. Auf der letzten Seite werden die Änderungen zusammengefasst. Dort wird auch die Änderung zur Aktivierung der Kennwortrückschreibung angezeigt.

> [!NOTE]
> Sie können die Funktion zum Zurückschreiben von Kennwörtern jederzeit deaktivieren, indem Sie diesen Assistenten erneut ausführen, oder indem Sie die Einstellung **Kennwörter in das lokale Verzeichnis zurückschreiben** auf **Nein** festlegen. Sie finden diese Einstellung im [klassischen Azure-Portal](https://manage.windowsazure.com) auf der Registerkarte **Konfigurieren** im Abschnitt **Richtlinie zum Zurücksetzen des Benutzerkennworts**.  Weitere Informationen zum Anpassen Ihrer Konfiguration für die Kennwortzurücksetzung finden Sie unter [Anpassen: Azure AD-Kennwortverwaltung](active-directory-passwords-customize.md).
>
>

#### <a name="to-enable-password-writeback-using-windows-powershell"></a>So aktivieren Sie das Zurückschreiben von Kennwörtern mithilfe von Windows PowerShell
1. Öffnen Sie auf Ihrem **Computer für die Verzeichnissynchronisierung** ein neues **Windows PowerShell-Fenster mit erhöhten Rechten**.
2. Wenn das Modul nicht bereits geladen ist, geben Sie den `import-module ADSync`-Befehl ein, um die Azure AD Connect-Cmdlets in Ihre aktuelle Sitzung zu laden.
3. Rufen Sie eine Liste mit den Azure AD-Connectors in Ihrem System ab, indem Sie das `Get-ADSyncConnector`-Cmdlet ausführen und die Ergebnisse in `$aadConnectorName` speichern, z.B. `$aadConnectorName = Get-ADSyncConnector|where-object {$_.name -like "*AAD"}`.
4. Rufen Sie den aktuellen Status der Rückschreibung für den aktuellen Connector ab, indem Sie das folgende Cmdlet ausführen: `Get-ADSyncAADPasswordResetConfiguration –Connector $aadConnectorName.name`
5. Aktivieren Sie das Zurückschreiben von Kennwörtern, indem Sie dieses Cmdlet ausführen: `Set-ADSyncAADPasswordResetConfiguration –Connector $aadConnectorName.name –Enable $true`

> [!NOTE]
> Stellen Sie bei der Aufforderung zur Eingabe von Anmeldeinformationen sicher, dass es sich bei dem für „AzureADCredential“ angegebenen Administratorkonto um ein **Cloudadministratorkonto (erstellt in Azure AD)** handelt, und nicht um ein Verbundkonto (erstellt im lokalen AD-Verzeichnis und synchronisiert mit Azure AD).
>
> [!NOTE]
> Sie können die Funktion zum Zurückschreiben von Kennwörtern über PowerShell deaktivieren, indem Sie die obigen Anweisungen erneut ausführen, aber hierbei `$false` übergeben, oder indem Sie die Einstellung **Kennwörter in das lokale Verzeichnis zurückschreiben** auf **Nein** festlegen. Sie finden diese Einstellung im [klassischen Azure-Portal](https://manage.windowsazure.com) auf der Registerkarte **Konfigurieren** im Abschnitt **Richtlinie zum Zurücksetzen des Benutzerkennworts**.
>
>

#### <a name="verify-that-the-configuration-was-successful"></a>Überprüfen, ob die Konfiguration erfolgreich war
Nachdem Sie die Konfiguration abgeschlossen haben, wird im Windows PowerShell-Fenster die Meldung "Das Zurückschreiben der Kennwortzurücksetzung ist aktiviert" angezeigt, in der Konfigurationsoberfläche wird eine Erfolgsmeldung eingeblendet.

Sie können die erfolgreiche Installation des Diensts auch überprüfen, indem Sie die Ereignisanzeige öffnen, zum Anwendungsereignisprotokoll navigieren und in der Quelle **PasswordResetService** nach dem Ereignis **31005 - OnboardingEventSuccess** suchen.

  ![][023]

### <a name="step-3-configure-your-firewall"></a>Schritt 3: Konfigurieren der Firewall
Nachdem Sie das Kennwortrückschreiben aktiviert haben, müssen Sie sicherstellen, dass der Computer, auf dem Azure AD Connect ausgeführt wird, eine Verbindung mit Microsoft-Clouddiensten herstellen und somit Anforderungen zum Kennwortrückschreiben empfangen kann. Dieser Schritt beinhaltet die Aktualisierung von Verbindungsregeln in Ihren Netzwerkgeräten (Proxyserver, Firewalls usw.), um ausgehende Verbindungen mit bestimmten [URLs und IP-Adressen von Microsoft](https://support.office.com/article/Office-365-URLs-and-IP-address-ranges-8548a211-3fe7-47cb-abb1-355ea5aa88a2?ui=en-US&rs=en-US&ad=US) über bestimmte Netzwerkports zuzulassen. Diese Änderungen variieren je nach Version des Azure AD Connect-Tools. Weitere Informationen finden Sie unter [Funktionsweise der Kennwortrückschreibung](active-directory-passwords-learn-more.md#how-password-writeback-works) und [Sicherheitsmodell für die Kennwortrückschreibung](active-directory-passwords-learn-more.md#password-writeback-security-model).

#### <a name="why-do-i-need-to-do-this"></a>Weshalb ist dieser Schritt erforderlich?

Damit das Kennwortrückschreiben richtig funktioniert, muss der Computer, auf dem Azure AD Connect ausgeführt wird, mit dem Dienst zum Zurücksetzen des Kennworts und mit Azure Service Bus kommunizieren können.

Für das Azure AD Connect-Tool **1.1.443.0** und höher:

- Die neueste Version des Azure AD Connect-Tools benötigt **ausgehenden HTTPS-Zugriff** auf Folgendes:
    - *passwordreset.microsoftonline.com*
    - *servicebus.windows.net*

Für die Azure AD Connect-Toolversionen **1.0.8667.0** bis **1.1.380.0** gilt Folgendes:

- **Option 1:** Zulassen aller ausgehenden HTTPS-Verbindungen über Port 443 (anhand der URL oder IP-Adresse)
    - Verwendung dieser Option:
        - Verwenden Sie diese Option, wenn Sie die einfachste Konfiguration einsetzen möchten, die nicht aktualisiert werden muss, wenn sich die IP-Adressbereiche für Azure-Datencenter im Laufe der Zeit ändern.
    - Erforderliche Schritte:
        - Lassen Sie alle ausgehenden HTTPS-Verbindungen über Port 443 anhand der URL oder IP-Adresse zu.
<br><br>
- **Option 2:** Zulassen ausgehender HTTPS-Verbindungen mit bestimmten IP-Adressbereichen und URLs
    - Verwendung dieser Option:
        - Verwenden Sie diese Option, wenn für Ihre Netzwerkumgebung Einschränkungen gelten oder Sie aus anderen Gründen Bedenken haben, ausgehende Verbindungen zuzulassen.
        - Bei dieser Konfiguration müssen Sie sicherstellen, dass Ihre Netzwerkgeräte wöchentlich mit den aktuellen IP-Adressen aus der Liste der IP-Adressbereiche für Microsoft Azure-Datencenter aktualisiert werden, damit das Kennwortrückschreiben korrekt funktioniert. Diese IP-Adressbereiche stehen als XML-Datei zur Verfügung, die jeden Mittwoch (Pacific Time) aktualisiert wird und am folgenden Montag (Pacific Time) in Kraft tritt.
    - Erforderliche Schritte:
        - Lassen Sie alle ausgehenden HTTPS-Verbindungen mit *.servicebus.windows.net zu.
        - Lassen Sie alle ausgehenden HTTPS-Verbindungen mit allen IP-Adressen in der Liste der IP-Adressbereiche für Microsoft Azure-Datencenter zu, und aktualisieren Sie diese Konfiguration wöchentlich. Die Liste ist [hier](https://www.microsoft.com/download/details.aspx?id=41653) als Download verfügbar.

> [!NOTE]
> Wenn Sie das Kennwortrückschreiben gemäß den obigen Anweisungen konfiguriert haben und keine Fehler im Azure AD Connect-Ereignisprotokoll angezeigt werden, aber beim Testen Verbindungsfehler auftreten, blockiert möglicherweise ein Netzwerkgerät in Ihrer Umgebung HTTPS-Verbindungen mit IP-Adressen. Während eine Verbindung mit *https://*.servicebus.windows.net* zulässig ist, kann beispielsweise eine Verbindung mit einer bestimmten IP-Adresse innerhalb dieses Bereichs blockiert sein. Um dieses Problem zu beheben, müssen Sie entweder Ihre Netzwerkumgebung zum Zulassen aller ausgehenden HTTPS-Verbindungen über Port 443 mit beliebigen URLs oder IP-Adressen konfigurieren (Option 1 oben) oder in Zusammenarbeit mit Ihrem Netzwerkteam explizit nur HTTPS-Verbindungen mit bestimmten IP-Adressen zulassen (Option 2 oben).

**Ältere Versionen:**

- Zulassen ausgehender TCP-Verbindungen über Port 443, 9350 – 9354 und Port 5671
- Zulassen ausgehender Verbindungen mit *https://ssprsbprodncu-sb.accesscontrol.windows.net/*

> [!NOTE]
> Wenn Sie mit einer Azure AD Connect-Version vor 1.0.8667.0 arbeiten, empfiehlt Microsoft dringend, auf die [neueste Version von Azure AD Connect](https://www.microsoft.com/download/details.aspx?id=47594) zu aktualisieren, die eine Reihe von Verbesserungen für das Rückschreiben im Netzwerk enthält und die Konfiguration vereinfacht.

Nachdem die Netzwerkgeräte konfiguriert wurden, starten Sie den Computer neu, auf dem das Azure AD Connect-Tool ausgeführt wird.

#### <a name="idle-connections-on-azure-ad-connect-114430-and-up"></a>Leerlaufverbindungen in Azure AD Connect (ab 1.1.443.0)
Das Azure AD Connect-Tool sendet regelmäßig Pings/Keep-Alives an ServiceBus-Endpunkte, um die Verbindungen aufrechtzuerhalten. Falls das Tool erkennt, dass zu viele Verbindungen getrennt werden, verkürzt es automatisch das Pingintervall für den Endpunkt. Das kleinstmögliche Pingintervall beträgt 60 Sekunden. **Wir empfehlen jedoch dringend, bei Proxys/Firewalls Leerlaufverbindungen mit einer Dauer von mindestens zwei bis drei Minuten zuzulassen.** \*Bei älteren Versionen wird ein Zeitraum von mindestens vier Minuten empfohlen.

### <a name="step-4-set-up-the-appropriate-active-directory-permissions"></a>Schritt 4: Einrichten der geeigneten Active Directory-Berechtigungen
Bei der anfänglichen Konfiguration mit dem Konfigurations-Assistenten wird ein Konto X eingerichtet. Für jede Gesamtstruktur, die Benutzer enthält, deren Kennwörter zurückgesetzt werden, müssen X die Berechtigungen **Kennwort zurücksetzen**, **Kennwort ändern**, **Schreibzugriff** für `lockoutTime` und **Schreibzugriff** für `pwdLastSet`, die erweiterten Rechte für entweder das Stammobjekt jeder Domäne in dieser Gesamtstruktur ODER für die Benutzerorganisationseinheit(en) erteilt werden, für die Kennwortzurücksetzung verfügbar sein soll.  Sie können letztere Option verwenden, wenn Sie die Berechtigungen zum Zurücksetzen nur einen bestimmten Satz von Benutzerobjekten beschränken möchten, falls sie für den Stamm der Domäne nicht zulässig sind. Die Rechte müssen hierbei so festgelegt werden, dass sie an alle Benutzerobjekte vererbt werden.  

Wenn Sie nicht sicher sind, auf welches Konto sich die oben genannten Angaben beziehen, öffnen Sie die Azure Active Directory Connect-Konfigurationsoberfläche, und klicken Sie auf die Option **Lösung prüfen** .  Das Konto, dem Sie die Berechtigung hinzufügen müssen, ist im Screenshot unten rot unterstrichen.

**<font color="red">Achten Sie darauf, diese Berechtigung für jede Domäne in jeder Gesamtstruktur des Systems einzurichten, da sonst das Kennwortrückschreiben nicht richtig funktioniert.</font>**

  ![][032]

  Durch das Festlegen dieser Berechtigungen kann das MA-Dienstkonto für jede Gesamtstruktur Kennwörter im Namen der Benutzerkonten innerhalb dieser Gesamtstruktur verwalten. Wenn Sie diese Berechtigungen nicht gewähren, erhalten die Benutzer – obwohl das Zurückschreiben scheinbar ordnungsgemäß konfiguriert ist – Fehler bei dem Versuch, ihre lokalen Kennwörter über die Cloud verwalten. Nachfolgend finden Sie eine detaillierte Schrittanleitung zum Gewähren dieser Berechtigungen im MMC-Snap-In **Active Directory-Benutzer und -Gruppen** .

> [!NOTE]
> Es kann bis zu einer Stunde dauern, bis diese Berechtigungen auf alle Objekte in Ihrem Verzeichnis repliziert wurden.
>
>

#### <a name="to-set-up-the-right-permissions-for-writeback-to-occur"></a>So richten Sie die geeigneten Berechtigungen für das Zurückschreiben von Kennwörtern ein
1. Öffnen Sie **Active Directory-Benutzer und -Computer** mit einem Konto, das über geeignete Berechtigungen für die Domänenverwaltung verfügt.
2. Vergewissern Sie sich, dass unter **Menü „Ansicht“** die Option **Erweiterte Features** aktiviert ist.
3. Klicken Sie im linken Bereich mit der rechten Maustaste auf das Objekt, das den Stamm der Domäne repräsentiert.
4. Klicken Sie auf die Registerkarte **Sicherheit** .
5. Klicken Sie dann auf **Erweitert**.

   ![][024]
6. Klicken Sie auf der Registerkarte **Berechtigungen** auf **Hinzufügen**.

   ![][025]
7. Wählen Sie das Konto aus, dem Sie Berechtigungen hinzufügen möchten (es sollte sich um dasselbe Konto handeln, das beim Einrichten der Synchronisierung für diese Gesamtstruktur angegeben wurde).
8. Wählen Sie in der Dropdownliste oben den Eintrag **Nachfolgerbenutzerobjekt**.
9. Aktivieren Sie im nun angezeigten Dialogfeld **Berechtigungseintrag** die Kontrollkästchen für **Kennwort zurücksetzen**, **Kennwort ändern**, **Schreibzugriff** für `lockoutTime` sowie **Schreibzugriff** für `pwdLastSet`.

   ![][026]
   ![][027]
   ![][028]
10. Klicken Sie anschließend in allen geöffneten Dialogfeldern auf **Anwenden/OK** .

### <a name="step-5-reset-your-ad-password-as-a-user"></a>Schritt 5: Zurücksetzen Ihres AD-Kennworts als Benutzer
Jetzt, da das Zurückschreiben von Kennwörtern aktiviert ist, können Sie die ordnungsgemäße Funktion testen, indem Sie das Kennwort eines Benutzers zurücksetzen, dessen Konto mit Ihrem Cloudmandanten synchronisiert wurde.

#### <a name="to-verify-password-writeback-is-working-properly"></a>So prüfen Sie, ob das Zurückschreiben von Kennwörtern ordnungsgemäß funktioniert
1. Navigieren Sie zu [https://passwordreset.microsoftonline.com](https://passwordreset.microsoftonline.com) , oder wechseln Sie zu einem beliebigen Organisations-ID-Anmeldebildschirm, und klicken Sie auf den Link **Können Sie nicht auf Ihr Konto zugreifen?** .

   ![][029]
2. Sie sollten jetzt eine neue Seite sehen, in der Sie nach der Benutzer-ID gefragt werden, für die Sie ein Kennwort zurücksetzen möchten. Geben Sie die ID des Testbenutzers ein, und führen Sie die Schritte zur Kennwortzurücksetzung aus.
3. Nachdem Sie Ihr Kennwort zurückgesetzt haben, wird ein Bildschirm angezeigt, der dem folgenden ähnelt. Diese Anzeige weist darauf hin, dass Sie Ihr Kennwort im lokalen Verzeichnis und/oder in den Cloudverzeichnissen erfolgreich zurückgesetzt haben.

   ![][030]
4. Um zu überprüfen, ob der Vorgang erfolgreich war oder ob Fehler ermittelt wurden, öffnen Sie auf Ihrem **Computer für die Verzeichnissynchronisierung** die **Ereignisanzeige**, navigieren zum **Anwendungsereignisprotokoll** und suchen nach dem Ereignis **31002 - PasswordResetSuccess** mit der Quelle **PasswordResetService** für Ihren Testbenutzer.

   ![][031]



## <a name="next-steps"></a>Nächste Schritte
Im Folgenden finden Sie Links zu allen Webseiten mit Informationen zur Kennwortzurücksetzung für Azure AD:

* **Sind Sie hier, weil Sie Probleme bei der Anmeldung haben?** Wenn ja, helfen Ihnen die Informationen zum [Ändern und Zurücksetzen Ihres eigenen Kennworts](active-directory-passwords-update-your-own-password.md#reset-my-password).
* [**Funktionsweise**](active-directory-passwords-how-it-works.md) – Erfahren Sie mehr über die sechs verschiedenen Komponenten des Diensts und deren Funktionen.
* [**Anpassen**](active-directory-passwords-customize.md) – Erfahren Sie, wie Sie das Aussehen und Verhalten des Diensts an die Anforderungen Ihrer Organisation anpassen.
* [**Best Practices**](active-directory-passwords-best-practices.md) – Erfahren Sie, wie Sie Kennwörter in Ihrer Organisation schnell bereitstellen und effektiv verwalten.
* [**Einblicke erhalten**](active-directory-passwords-get-insights.md) – Erfahren Sie mehr über unsere integrierten Berichtsfunktionen.
* [**Häufig gestellte Fragen**](active-directory-passwords-faq.md) – Hier erhalten Sie Antworten auf häufig gestellte Fragen.
* [**Problembehandlung**](active-directory-passwords-troubleshoot.md) – Erfahren Sie, wie Sie Probleme mit dem Dienst schnell beheben.
* [**Weitere Informationen**](active-directory-passwords-learn-more.md) – Erhalten Sie tiefgehende technische Details zur Funktionsweise des Diensts.

[001]: ./media/active-directory-passwords-getting-started/001.jpg "Image_001.jpg"
[002]: ./media/active-directory-passwords-getting-started/002.jpg "Image_002.jpg"
[003]: ./media/active-directory-passwords-getting-started/003.jpg "Image_003.jpg"
[004]: ./media/active-directory-passwords-getting-started/004.jpg "Image_004.jpg"
[005]: ./media/active-directory-passwords-getting-started/005.jpg "Image_005.jpg"
[006]: ./media/active-directory-passwords-getting-started/006.jpg "Image_006.jpg"
[007]: ./media/active-directory-passwords-getting-started/007.jpg "Image_007.jpg"
[008]: ./media/active-directory-passwords-getting-started/008.jpg "Image_008.jpg"
[009]: ./media/active-directory-passwords-getting-started/009.jpg "Image_009.jpg"
[010]: ./media/active-directory-passwords-getting-started/010.jpg "Image_010.jpg"
[011]: ./media/active-directory-passwords-getting-started/011.jpg "Image_011.jpg"
[012]: ./media/active-directory-passwords-getting-started/012.jpg "Image_012.jpg"
[013]: ./media/active-directory-passwords-getting-started/013.jpg "Image_013.jpg"
[014]: ./media/active-directory-passwords-getting-started/014.jpg "Image_014.jpg"
[015]: ./media/active-directory-passwords-getting-started/015.jpg "Image_015.jpg"
[016]: ./media/active-directory-passwords-getting-started/016.jpg "Image_016.jpg"
[017]: ./media/active-directory-passwords-getting-started/017.jpg "Image_017.jpg"
[018]: ./media/active-directory-passwords-getting-started/018.jpg "Image_018.jpg"
[019]: ./media/active-directory-passwords-getting-started/019.jpg "Image_019.jpg"
[020]: ./media/active-directory-passwords-getting-started/020.jpg "Image_020.jpg"
[021]: ./media/active-directory-passwords-getting-started/021.jpg "Image_021.jpg"
[022]: ./media/active-directory-passwords-getting-started/022.jpg "Image_022.jpg"
[023]: ./media/active-directory-passwords-getting-started/023.jpg "Image_023.jpg"
[024]: ./media/active-directory-passwords-getting-started/024.jpg "Image_024.jpg"
[025]: ./media/active-directory-passwords-getting-started/025.jpg "Image_025.jpg"
[026]: ./media/active-directory-passwords-getting-started/026.jpg "Image_026.jpg"
[027]: ./media/active-directory-passwords-getting-started/027.jpg "Image_027.jpg"
[028]: ./media/active-directory-passwords-getting-started/028.jpg "Image_028.jpg"
[029]: ./media/active-directory-passwords-getting-started/029.jpg "Image_029.jpg"
[030]: ./media/active-directory-passwords-getting-started/030.jpg "Image_030.jpg"
[031]: ./media/active-directory-passwords-getting-started/031.jpg "Image_031.jpg"
[032]: ./media/active-directory-passwords-getting-started/032.jpg "Image_032.jpg"

