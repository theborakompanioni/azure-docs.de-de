---
title: 'Richtlinie: Azure AD SSPR | Microsoft-Dokumentation'
description: "Richtlinienoptionen zur Self-Service-Kennwortzurücksetzung in Azure AD"
services: active-directory
keywords: "Active Directory-Kennwortverwaltung, Kennwortverwaltung, Self-Service-Kennwortzurücksetzung in Azure AD"
documentationcenter: 
author: MicrosoftGuyJFlo
manager: femila
ms.reviewer: sahenry
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/28/2017
ms.author: joflore
ms.custom: it-pro
ms.translationtype: HT
ms.sourcegitcommit: 94d1d4c243bede354ae3deba7fbf5da0652567cb
ms.openlocfilehash: 4b35c5d126375735f070a7fe2331896c524b5a61
ms.contentlocale: de-de
ms.lasthandoff: 07/18/2017

---
# <a name="password-policies-and-restrictions-in-azure-active-directory"></a>Kennwortrichtlinien und -einschränkungen in Azure Active Directory

Dieser Artikel beschreibt die Kennwortrichtlinien und Komplexitätsanforderungen im Zusammenhang mit den in Ihrem Azure AD-Mandanten gespeicherten Benutzerkonten.

## <a name="administrator-password-policy-differences"></a>Unterschiede zu Richtlinien für Administratorkennwörter

Microsoft erzwingt eine starke Standardrichtlinie mit **zweistufiger Überprüfung** für die Kennwortzurücksetzung für Azure-Administratorrollen (Beispiel: globaler Administrator, Helpdeskadministrator, Kennwortadministrator usw.).

Damit wird verhindert, dass Administratoren Sicherheitsfragen verwenden, und Folgendes erzwungen.

Die Richtlinie für die zweistufige Überprüfung erfordert die Angabe von zwei Elementen der Authentifizierungsdaten (E-Mail-Adresse **und** Telefonnummer). Sie gilt in folgenden Situationen:

* Alle Azure-Administratorrollen
  * Helpdeskadministrator
  * Dienstunterstützungsadministrator
  * Abrechnungsadministrator
  * Partnersupport der Ebene 1
  * Partnersupport der Ebene 2
  * Exchange-Dienstadministrator
  * Lync-Dienstadministrator
  * Benutzerkontoadministrator
  * Verzeichnis schreiben
  * Globaler Administrator/Unternehmensadministrator
  * SharePoint-Dienstadministrator
  * Complianceadministrator
  * Anwendungsadministrator
  * Sicherheitsadministrator
  * Administrator für privilegierte Rollen
  * Intune-Dienstadministrator
  * Dienstadministrator des Anwendungsproxys
  * CRM-Dienstadministrator
  * Power BI-Dienstadministrator
  
* Die 30 Tage einer Testversion sind abgelaufen, **ODER**
* Es ist eine benutzerdefinierte Domäne (contoso.com) vorhanden, **ODER**
* Azure AD Connect synchronisiert Identitäten aus Ihrem lokalen Verzeichnis.

### <a name="exceptions"></a>Ausnahmen
Die Richtlinie für die einstufige Überprüfung erfordert die Angabe von einem Element der Authentifizierungsdaten (E-Mail-Adresse **oder** Telefonnummer). Sie gilt in folgenden Situationen:

* Erste 30 Tage einer Testversion **ODER**
* Es ist keine benutzerdefinierte Domäne (*.onmicrosoft.com) vorhanden, **UND** Azure AD Connect synchronisiert keine Identitäten.


## <a name="userprincipalname-policies-that-apply-to-all-user-accounts"></a>UserPrincipalName-Richtlinien, die für alle Benutzerkonten gelten

Jedes Benutzerkonto, das sich bei Azure AD anmeldet, muss über ein eindeutiges, diesem Konto zugeordnetes Benutzerprinzipalnamen-Attribut (UPN, User Principal Name) verfügen. Die folgende Tabelle zeigt die Richtlinien, die sowohl für lokale Active Directory-Benutzerkonten (mit Cloudsynchronisierung) als auch für Benutzerkonten gelten, die ausschließlich in der Cloud gespeichert werden.

| Eigenschaft | UserPrincipalName-Richtlinien |
| --- | --- |
| Zulässige Zeichen |<ul> <li>A – Z</li> <li>a – z</li><li>0 – 9</li> <li> zu erstellen und zu verwalten. - \_ ! \# ^ \~</li></ul> |
| Unzulässige Zeichen |<ul> <li>Alle @-Zeichen, die nicht den Benutzernamen und die Domäne trennen.</li> <li>Darf keinen Punkt (.) unmittelbar vor dem @-Symbol enthalten.</li></ul> |
| Längenbeschränkungen |<ul> <li>Die Gesamtlänge darf 113 Zeichen nicht überschreiten.</li><li>64 Zeichen vor dem @-Symbol</li><li>48 Zeichen nach dem @-Symbol</li></ul> |

## <a name="password-policies-that-apply-only-to-cloud-user-accounts"></a>Kennwortrichtlinien, die nur für Cloudbenutzerkonten gelten

Die folgende Tabelle beschreibt die verfügbaren Kennwortrichtlinieneinstellungen, die auf die in Azure AD erstellten und verwalteten Benutzerkonten angewendet werden können.

| Eigenschaft | Anforderungen |
| --- | --- |
| Zulässige Zeichen |<ul><li>A – Z</li><li>a – z</li><li>0 – 9</li> <li>@ # $ % ^ & * - _ ! + = [ ] { } &#124; \ : ‘ , . ? / ` ~ “ ( ) ;</li></ul> |
| Unzulässige Zeichen |<ul><li>Unicode-Zeichen</li><li>Leerzeichen</li><li> **Nur für sichere Kennwörter**: Dürfen keinen Punkt (.) unmittelbar vor dem @-Symbol enthalten.</li></ul> |
| Kennworteinschränkungen |<ul><li>Mindestens 8 Zeichen und höchstens 16 Zeichen</li><li>**Nur für sichere Kennwörter**: Muss drei der folgenden vier Elemente enthalten:<ul><li>Kleinbuchstaben</li><li>Großbuchstaben</li><li>Zahlen (0 bis 9)</li><li>Symbole (siehe „Kennworteinschränkungen“ weiter oben)</li></ul></li></ul> |
| Zeitraum bis zum Ablauf des Kennworts |<ul><li>Standardwert: **90** Tage </li><li>Der Wert kann im Azure Active Directory-Modul für Windows PowerShell mit dem Cmdlet „Set-MsolPasswordPolicy“ konfiguriert werden.</li></ul> |
| Benachrichtigung bei Ablauf des Kennworts |<ul><li>Standardwert: **14** Tage (bevor das Kennwort abläuft)</li><li>Der Wert kann mit dem Cmdlet „Set-MsolPasswordPolicy“ konfiguriert werden.</li></ul> |
| Ablauf des Kennworts |<ul><li>Standardwert: **false** Tage (Gibt an, dass der Kennwortablauf aktiviert ist.) </li><li>Der Wert kann mit dem Cmdlet „Set-MsolUser“ für einzelne Benutzerkonten konfiguriert werden. </li></ul> |
| Verlauf der **Kennwortänderungen** |Das letzte Kennwort kann beim **Ändern** eines Kennworts **nicht** erneut verwendet werden. |
| Verlauf der **Kennwortzurücksetzungen** | Das letzte Kennwort **darf** erneut verwendet werden, wenn ein vergessenes Kennwort **zurückgesetzt** wird. |
| Kontosperrung |Nach zehn nicht erfolgreichen Anmeldeversuchen (falsches Kennwort) wird der Benutzer für eine Minute gesperrt. Mit jeder weiteren nicht erfolgreichen Anmeldung verlängert sich die Dauer, die der Benutzer gesperrt ist. |

## <a name="set-password-expiration-policies-in-azure-active-directory"></a>Festlegen von Kennwortablaufrichtlinien in Azure Active Directory

Ein globaler Administrator für einen Microsoft-Clouddienst kann mit dem Microsoft Azure Active Directory-Modul für Windows PowerShell festlegen, dass Benutzerkennwörter nicht ablaufen. Sie können auch Windows PowerShell-Cmdlets verwenden, um die Konfiguration zu entfernen, die angibt, dass Kennwörter nicht ablaufen, oder um anzuzeigen, welche Benutzerkennwörter nicht ablaufen. Diese Anleitung gilt für andere Anbieter wie Microsoft Intune und Office 365, die auf Microsoft Azure Active Directory als Identitäts- und Verzeichnisdienste zurückgreifen.

> [!NOTE]
> Nur Kennwörter für Benutzerkonten, die nicht über die Verzeichnissynchronisierung synchronisiert werden, können so konfiguriert werden, dass sie nicht ablaufen. Weitere Informationen zur Verzeichnissynchronisierung finden Sie unter [Connect AD mit Azure AD](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect).
>
>

## <a name="set-or-check-password-policies-using-powershell"></a>Festlegen oder Überprüfen von Kennwortrichtlinien mithilfe von PowerShell

Zunächst müssen Sie [das Azure AD PowerShell-Modul herunterladen und installieren](https://docs.microsoft.com/powershell/module/Azuread/?view=azureadps-2.0). Sobald Sie es installiert haben, können Sie die folgenden Schritte befolgen, um jedes Feld zu konfigurieren.

### <a name="how-to-check-expiration-policy-for-a-password"></a>Überprüfen der Ablaufrichtlinie für ein Kennwort
1. Stellen Sie mit den Anmeldeinformationen des Unternehmensadministrators eine Verbindung mit Windows PowerShell her.
2. Führen Sie einen der folgenden Befehle aus:

   * Um festzustellen, ob für das Kennwort eines bestimmten Benutzers festgelegt ist, dass es nie abläuft, führen Sie das folgende Cmdlet mit dem Benutzerprinzipalnamen (UPN, z. B. aprilr@contoso.onmicrosoft.com) oder der Benutzer-ID des zu überprüfenden Benutzers aus: `Get-MSOLUser -UserPrincipalName <user ID> | Select PasswordNeverExpires`
   * Um die Einstellung "Kennwort läuft nie ab" für alle Benutzer anzuzeigen, führen Sie das folgende Cmdlet aus: `Get-MSOLUser | Select UserPrincipalName, PasswordNeverExpires`

### <a name="set-a-password-to-expire"></a>Festlegen, dass ein Kennwort abläuft

1. Stellen Sie mit den Anmeldeinformationen des Unternehmensadministrators eine Verbindung mit Windows PowerShell her.
2. Führen Sie einen der folgenden Befehle aus:

   * Um für das Kennwort eines Benutzers festzulegen, dass es abläuft, führen Sie das folgende Cmdlet mit dem Benutzerprinzipalnamen (UPN) oder der Benutzer-ID des Benutzers aus: `Set-MsolUser -UserPrincipalName <user ID> -PasswordNeverExpires $false`
   * Um für die Kennwörter aller Benutzer in der Organisation festzulegen, dass sie ablaufen, verwenden Sie das folgende Cmdlet: `Get-MSOLUser | Set-MsolUser -PasswordNeverExpires $false`

### <a name="set-a-password-to-never-expire"></a>Festlegen, dass ein Kennwort nicht abläuft

1. Stellen Sie mit den Anmeldeinformationen des Unternehmensadministrators eine Verbindung mit Windows PowerShell her.
2. Führen Sie einen der folgenden Befehle aus:

   * Um für das Kennwort eines Benutzers festzulegen, dass es nie abläuft, führen Sie das folgende Cmdlet mit dem Benutzerprinzipalnamen (UPN) oder der Benutzer-ID des Benutzers aus: `Set-MsolUser -UserPrincipalName <user ID> -PasswordNeverExpires $true`
   * Um für die Kennwörter aller Benutzer in einer Organisation festzulegen, dass sie nie ablaufen, führen Sie das folgende Cmdlet aus: `Get-MSOLUser | Set-MsolUser -PasswordNeverExpires $true`

## <a name="next-steps"></a>Nächste Schritte

Die folgenden Links führen zu weiteren Informationen zur Kennwortzurücksetzung mit Azure AD:

* [**Schnellstart:**](active-directory-passwords-getting-started.md) Informieren Sie sich schnell über die Self-Service-Kennwortverwaltung von Azure AD. 
* [**Lizenzierung:**](active-directory-passwords-licensing.md) Konfigurieren Sie Ihre Azure AD-Lizenzierung.
* [**Daten:**](active-directory-passwords-data.md) Erfahren Sie, welche Daten erforderlich sind und wie sie für die Kennwortverwaltung verwendet werden.
* [**Rollout:**](active-directory-passwords-best-practices.md) Mithilfe der hier enthaltenen Anleitungen können Sie SSPR planen und für Ihre Benutzer bereitstellen.
* [**Anpassung:**](active-directory-passwords-customize.md) Passen Sie das Aussehen und Verhalten von SSPR für Ihr Unternehmen an.
* [**Berichterstellung:**](active-directory-passwords-reporting.md) Ermitteln Sie, ob, wann und wo Ihre Benutzer auf SSPR-Funktionen zugreifen.
* [**Ausführliche technische Informationen:**](active-directory-passwords-how-it-works.md) Steigen Sie tiefer ein, um zu verstehen, wie alles funktioniert.
* [**Häufig gestellte Fragen (FAQ):**](active-directory-passwords-faq.md) Wie? Warum? Was? Wo? Wer? Wann? - Antworten auf Fragen, die Sie schon immer stellen wollten
* [**Problembehandlung:**](active-directory-passwords-troubleshoot.md) Erfahren Sie, wie Sie häufig auftretende Probleme bei SSPR beheben.

